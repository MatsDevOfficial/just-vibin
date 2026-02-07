<script>
    import Card from "$lib/components/Card.svelte";

    let fileName = $state("");
    let stats = $state(null);
    let issues = $state([]);

    function handleUpload(event) {
        const file = event.target.files[0];
        if (!file) return;
        fileName = file.name;
        issues = [];

        const reader = new FileReader();
        reader.onload = (e) => {
            const content = e.target.result;
            parseGCode(content);
        };
        reader.readAsText(file);
    }

    function parseGCode(content) {
        // Advanced parsing for multiple slicers
        const getMatch = (regexes) => {
            for (const re of regexes) {
                const match = content.match(re);
                if (match) return match[1];
            }
            return null;
        };

        const time = getMatch([
            /TIME:(\d+)/i,
            /estimated printing time \(normal mode\) = (.*)/i,
            /; total_duration = (.*)/i,
            /; print_time = (.*)/i,
        ]);

        const weight = getMatch([
            /Filament used: (.*)g/i,
            /filament used \[g\] = (.*)/i,
            /; total filament weight \[g\] = (.*)/i,
            /; filament_weight = (.*)/i,
        ]);

        const layerCount = getMatch([
            /;LAYER_COUNT:(\d+)/i,
            /; total layers count = (\d+)/i,
            /; layers = (\d+)/i,
        ]);

        const slicer = getMatch([
            /;Generated with (.*)/i,
            /; Slic3r (.*)/i,
            /; PrusaSlicer (.*)/i,
            /; Cura (.*)/i,
            /; BambuStudio (.*)/i,
            /; OrcaSlicer (.*)/i,
        ]);

        const targetNozzleTemp = getMatch([/M104 S(\d+)/, /M109 S(\d+)/]);
        const targetBedTemp = getMatch([/M140 S(\d+)/, /M190 S(\d+)/]);

        stats = {
            time: time || "Unknown",
            weight: weight || "Unknown",
            layers: layerCount || "Unknown",
            slicer: slicer || "Generic / Unknown",
            nozzleTemp: targetNozzleTemp || "Unknown",
            bedTemp: targetBedTemp || "Unknown",
        };

        detectIssues(content, stats);
    }

    function detectIssues(content, stats) {
        const tempIssues = [];

        // Temperature checks
        if (stats.nozzleTemp !== "Unknown") {
            const nt = parseInt(stats.nozzleTemp);
            if (nt > 260) {
                tempIssues.push({
                    type: "danger",
                    msg: `High nozzle temp (${nt}°C)! Ensure you have an all-metal hotend to avoid PTFE fumes.`,
                });
            }
            if (nt < 170) {
                tempIssues.push({
                    type: "warning",
                    msg: `Low nozzle temp (${nt}°C). Cold extrusion might clog or damage the extruder.`,
                });
            }
        }

        // Feature: Bed temp sanity check
        if (stats.bedTemp !== "Unknown") {
            const bt = parseInt(stats.bedTemp);
            if (bt > 110) {
                tempIssues.push({
                    type: "warning",
                    msg: `Very high bed temp (${bt}°C). Watch for component warping or power supply strain.`,
                });
            }
        }

        // G-Code structure checks
        if (!content.includes("M104") && !content.includes("M109")) {
            tempIssues.push({
                type: "danger",
                msg: "No nozzle heating commands found (M104/M109). The print may start cold!",
            });
        }

        if (!content.includes("G28")) {
            tempIssues.push({
                type: "warning",
                msg: "Auto-homing (G28) missing in start G-Code. Ensure your printer homes before starting.",
            });
        }

        if (content.length > 50000000) {
            // 50MB
            tempIssues.push({
                type: "note",
                msg: "Large file detected. Ensure your SD card or OctoPrint can handle the data rate.",
            });
        }

        issues = tempIssues;
    }
</script>

<div class="gcode-stats">
    <div class="upload-zone">
        <i class="fa-solid fa-file-shield"></i>
        <p>{fileName || "Upload G-Code to Inspect"}</p>
        <input type="file" accept=".gcode" onchange={handleUpload} />
    </div>

    {#if stats}
        <div class="stats-grid">
            <div class="stat-item">
                <span class="label">Slicer</span>
                <span class="value">{stats.slicer}</span>
            </div>
            <div class="stat-item">
                <span class="label">Print Time</span>
                <span class="value">{stats.time}</span>
            </div>
            <div class="stat-item">
                <span class="label">Filament</span>
                <span class="value">{stats.weight}g</span>
            </div>
            <div class="stat-item">
                <span class="label">Layers</span>
                <span class="value">{stats.layers}</span>
            </div>
            <div class="stat-item">
                <span class="label">Nozzle</span>
                <span class="value">{stats.nozzleTemp}°C</span>
            </div>
            <div class="stat-item">
                <span class="label">Bed</span>
                <span class="value">{stats.bedTemp}°C</span>
            </div>
        </div>
    {/if}

    {#if issues.length > 0}
        <div class="issues-panel">
            <h3>
                <i class="fa-solid fa-triangle-exclamation"></i> Safety & Logic Scan
            </h3>
            <ul>
                {#each issues as issue}
                    <li class={issue.type}>
                        <i
                            class={issue.type === "danger"
                                ? "fa-solid fa-circle-xmark"
                                : issue.type === "warning"
                                  ? "fa-solid fa-triangle-exclamation"
                                  : "fa-solid fa-circle-info"}
                        ></i>
                        {issue.msg}
                    </li>
                {/each}
            </ul>
        </div>
    {:else if stats}
        <div class="all-clear">
            <i class="fa-solid fa-check-circle"></i> No obvious issues detected!
        </div>
    {/if}
</div>

<style>
    .gcode-stats {
        background: white;
        padding: 24px;
        border-radius: 16px;
        text-align: center;
        border: 1px solid #eee;
    }

    .upload-zone {
        border: 2px dashed var(--primary);
        padding: 30px;
        border-radius: 12px;
        cursor: pointer;
        position: relative;
        background: #fcfdfd;
        transition: all 0.2s ease;
    }

    .upload-zone:hover {
        background: #f0fdf9;
        transform: scale(1.02);
    }

    .upload-zone i {
        font-size: 3rem;
        color: var(--primary);
        margin-bottom: 12px;
    }

    input[type="file"] {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        opacity: 0;
        cursor: pointer;
    }

    .stats-grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 15px;
        margin-top: 24px;
        padding: 15px;
        background: #f8f9fa;
        border-radius: 12px;
    }

    .stat-item {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .label {
        font-size: 0.75rem;
        color: #888;
        text-transform: uppercase;
        letter-spacing: 0.5px;
    }

    .value {
        font-size: 1.1rem;
        font-weight: 700;
        color: var(--dark);
    }

    .issues-panel {
        margin-top: 24px;
        text-align: left;
        border-top: 2px solid #eee;
        padding-top: 15px;
    }

    .issues-panel h3 {
        font-size: 1rem;
        margin-bottom: 15px;
        display: flex;
        align-items: center;
        gap: 10px;
    }

    ul {
        list-style: none;
        padding: 0;
        margin: 0;
    }

    li {
        padding: 12px;
        border-radius: 8px;
        margin-bottom: 10px;
        font-size: 0.9rem;
        display: flex;
        align-items: flex-start;
        gap: 12px;
        line-height: 1.4;
    }

    li i {
        margin-top: 3px;
    }

    .danger {
        background: #fff5f5;
        color: #c53030;
        border-left: 4px solid #c53030;
    }
    .warning {
        background: #fffaf0;
        color: #975a16;
        border-left: 4px solid #975a16;
    }
    .note {
        background: #ebf8ff;
        color: #2b6cb0;
        border-left: 4px solid #2b6cb0;
    }

    .all-clear {
        margin-top: 24px;
        padding: 15px;
        background: #f0fff4;
        color: #2f855a;
        border-radius: 8px;
        font-weight: 600;
    }
</style>
