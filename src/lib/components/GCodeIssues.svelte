<script>
    import Card from "$lib/components/Card.svelte";

    const commonIssues = [
        {
            cmd: "M104 vs M109",
            issue: "M104 starts heating and moves on; M109 waits for temp.",
            danger: "Using M104 alone can start a print while the nozzle is still cold, leading to clogs.",
        },
        {
            cmd: "G28 / G29",
            issue: "Homing (G28) must always come BEFORE bed leveling (G29).",
            danger: "If G28 is run after G29, the bed leveling mesh is often cleared.",
        },
        {
            cmd: "Move Before Heat",
            issue: "Moving the head (G1) before the nozzle is hot.",
            danger: "Can grind filament or damage the extruder if the filament is still solid.",
        },
        {
            cmd: "Absolute vs Relative",
            issue: "G90 (Absolute) vs G91 (Relative) positioning.",
            danger: 'Confusion between these leads to "head crashes" into the frame or bed.',
        },
        {
            cmd: "E-Steps Calibration",
            issue: "Default M92 values might over or under-extrude.",
            danger: "Leads to weak parts or messy blobs that ruin the print.",
        },
    ];
</script>

<div class="gcode-issues-guide">
    <div class="grid">
        {#each commonIssues as item}
            <Card title={item.cmd}>
                <div class="issue-content">
                    <p class="desc">{item.issue}</p>
                    <div class="danger-box">
                        <i class="fa-solid fa-triangle-exclamation"></i>
                        <span>{item.danger}</span>
                    </div>
                </div>
            </Card>
        {/each}
    </div>
</div>

<style>
    .issue-content {
        font-size: 0.95rem;
    }
    .desc {
        color: #444;
        margin-bottom: 12px;
    }
    .danger-box {
        background: #fff5f5;
        border-left: 4px solid #c53030;
        padding: 10px;
        border-radius: 4px;
        display: flex;
        gap: 10px;
        font-size: 0.85rem;
        color: #c53030;
    }
    .danger-box i {
        font-size: 1rem;
        margin-top: 2px;
    }
</style>
