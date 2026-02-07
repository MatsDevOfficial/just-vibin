<script>
    import Card from "$lib/components/Card.svelte";
    import CostCalculator from "$lib/components/CostCalculator.svelte";
    import FilamentDatabase from "$lib/components/FilamentDatabase.svelte";
    import MaintenanceTracker from "$lib/components/MaintenanceTracker.svelte";
    import CoolingGuide from "$lib/components/CoolingGuide.svelte";
    import GCodeStats from "$lib/components/GCodeStats.svelte";
    import GCodeIssues from "$lib/components/GCodeIssues.svelte";
</script>

<svelte:head>
    <title>3D Printing Hub - Home</title>
</svelte:head>

<section id="gcode">
    <h2>G-Code Inspection & Reference</h2>
    <div class="grid">
        <Card icon="fa-solid fa-file-shield" title="G-Code Intelligence">
            <GCodeStats />
        </Card>
        <div class="reference-side">
            <Card
                icon="fa-solid fa-book-medical"
                title="Common G-Code Pitfalls"
            >
                <GCodeIssues />
            </Card>
        </div>
    </div>
</section>

<section id="tools">
    <h2>Maintenance & Performance</h2>
    <div class="grid">
        <Card icon="fa-solid fa-list-check" title="Maintenance Tracker">
            <MaintenanceTracker />
        </Card>
        <Card icon="fa-solid fa-wind" title="Cooling Guide">
            <CoolingGuide />
        </Card>
    </div>
</section>

<section id="tips">
    <h2>Tips & Tricks</h2>
    <div class="grid">
        <Card icon="fa-solid fa-layer-group" title="Layer Height">
            <p>Lower layer heights improve detail but increase print time.</p>
        </Card>
        <Card icon="fa-solid fa-temperature-high" title="Temperature Tuning">
            <p>Print temperature affects adhesion, strength, and stringing.</p>
        </Card>
        <Card icon="fa-solid fa-wind" title="Cooling">
            <p>Proper cooling improves overhangs and surface quality.</p>
        </Card>
    </div>
</section>

<section id="cheap">
    <h2>Best Budget Printers</h2>
    <div class="grid">
        <Card icon="fa-solid fa-money-bill" title="Creality Ender 3">
            <p>Popular beginner printer with huge community support.</p>
            <p class="price">~$200</p>
        </Card>
        <Card icon="fa-solid fa-tag" title="Anycubic Kobra Go">
            <p>Affordable printer with auto bed leveling.</p>
            <p class="price">~$220</p>
        </Card>
    </div>
</section>

<section id="quality">
    <h2>High Quality Printers</h2>
    <div class="grid">
        <Card icon="fa-solid fa-gem" title="Prusa MK4">
            <p>Industry-leading reliability and print quality.</p>
            <p class="price">~$800+</p>
        </Card>
        <Card icon="fa-solid fa-rocket" title="Bambu Lab X1 Carbon">
            <p>Fast, enclosed, and highly automated.</p>
            <p class="price">~$1200+</p>
        </Card>
    </div>
</section>

<section id="materials">
    <h2>Filament Database</h2>
    <FilamentDatabase />
</section>

<section id="failures">
    <h2>Common Print Problems</h2>
    <div class="grid">
        <Card icon="fa-solid fa-triangle-exclamation" title="Warping">
            <p>
                Use bed adhesion aids, correct bed temperature, and avoid
                drafts.
            </p>
        </Card>
        <Card icon="fa-solid fa-spaghetti-monster-flying" title="Stringing">
            <p>
                Lower nozzle temperature and enable retraction in slicer
                settings.
            </p>
        </Card>
        <Card icon="fa-solid fa-bug" title="Layer Shifts">
            <p>Check belt tension and ensure motors are not overheating.</p>
        </Card>
    </div>
</section>

<section id="software">
    <h2>Slicers & Software</h2>
    <div class="grid">
        <Card icon="fa-solid fa-scissors" title="Cura">
            <p>
                Free and beginner-friendly slicer with extensive printer
                profiles.
            </p>
        </Card>
        <Card icon="fa-solid fa-gears" title="PrusaSlicer">
            <p>Advanced slicing features with excellent default presets.</p>
        </Card>
        <Card icon="fa-solid fa-cloud" title="OctoPrint">
            <p>Remote printer control, webcam monitoring, and plugins.</p>
        </Card>
    </div>
</section>

<section id="comparison">
    <h2>Printer Comparison</h2>
    <div class="card-wrapper">
        <div class="table-container">
            <table>
                <thead>
                    <tr>
                        <th>Printer</th>
                        <th>Price</th>
                        <th>Bed Size</th>
                        <th>Best For</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>Ender 3</td>
                        <td>$200</td>
                        <td>220×220×250</td>
                        <td>Beginners</td>
                    </tr>
                    <tr>
                        <td>Prusa MK4</td>
                        <td>$800+</td>
                        <td>250×210×220</td>
                        <td>Reliability</td>
                    </tr>
                    <tr>
                        <td>Bambu X1 Carbon</td>
                        <td>$1200+</td>
                        <td>256×256×256</td>
                        <td>Speed & Automation</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</section>

<section id="calculator">
    <h2>Print Cost Calculator</h2>
    <div class="grid">
        <Card icon="fa-solid fa-calculator" title="Calculate Your Print">
            <CostCalculator />
        </Card>
        <Card icon="fa-solid fa-circle-info" title="Why it matters">
            <p>
                Knowing print cost helps pricing parts, managing waste, and
                choosing materials.
            </p>
        </Card>
    </div>
</section>

<section id="safety">
    <h2>Safety & Maintenance</h2>
    <div class="grid">
        <Card icon="fa-solid fa-shield-halved" title="Ventilation">
            <p>
                Always ventilate when printing ABS, nylon, or carbon fiber
                materials.
            </p>
        </Card>
        <Card icon="fa-solid fa-screwdriver-wrench" title="Maintenance">
            <p>Regularly clean nozzles, lubricate rails, and level the bed.</p>
        </Card>
        <Card icon="fa-solid fa-fire-extinguisher" title="Fire Safety">
            <p>
                Never leave printers unattended for long prints without
                safeguards.
            </p>
        </Card>
    </div>
</section>

<style>
    .card-wrapper {
        background: rgb(252, 242, 244);
        padding: 25px;
        border-radius: 16px;
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
    }
    .table-container {
        overflow-x: auto;
    }
    table {
        width: 100%;
        border-collapse: collapse;
    }
    th,
    td {
        padding: 12px;
        text-align: left;
        border-bottom: 1px solid #eee;
    }
    th {
        background: #f8f8f8;
        font-weight: 600;
    }
</style>
