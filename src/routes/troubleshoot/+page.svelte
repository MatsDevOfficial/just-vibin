<script>
    import Card from "$lib/components/Card.svelte";

    let currentStep = $state("start");

    const steps = {
        start: {
            question: "What is the main issue with your print?",
            options: [
                { text: "Print not sticking to bed", next: "bed_adhesion" },
                { text: "Messy or hairy surface", next: "stringing" },
                { text: "Layers shifted or skewed", next: "layer_shift" },
                { text: "Object is warped/curled", next: "warping" },
            ],
        },
        bed_adhesion: {
            question: "Is the first layer missing or just not sticking?",
            options: [
                { text: "Missing / Nozzle scraping", next: "too_low" },
                { text: "Not sticking / Peeling off", next: "bed_prep" },
            ],
        },
        too_low: {
            question:
                "Solution: Your nozzle is likely too close. Re-level your bed and check your Z-offset.",
            options: [{ text: "Start over", next: "start" }],
        },
        bed_prep: {
            question:
                "Try cleaning the bed with IPA or using hairspray/glue stick. Is the bed heated?",
            options: [
                { text: "Yes, it is heated", next: "temp_check" },
                { text: "No, it is cold", next: "heat_bed" },
            ],
        },
        heat_bed: {
            question:
                "Solution: Heat your bed! PLA: 60°C, PETG: 80°C. This helps adhesion significantly.",
            options: [{ text: "Start over", next: "start" }],
        },
        stringing: {
            question: "Are there fine hairs or blobs between parts?",
            options: [
                { text: "Fine hairs", next: "retraction" },
                { text: "Blobs/Oozing", next: "temp_reduction" },
            ],
        },
        retraction: {
            question:
                "Solution: Enable or increase retraction in your slicer settings. Try 5-7mm at 45mm/s.",
            options: [{ text: "Start over", next: "start" }],
        },
        temp_reduction: {
            question:
                "Solution: Your nozzle is likely too hot. Try reducing temperature by 5-10°C.",
            options: [{ text: "Start over", next: "start" }],
        },
        layer_shift: {
            question: "Is the shift sudden or gradual (slanting)?",
            options: [
                { text: "Sudden shift", next: "belts_check" },
                { text: "Gradual slanting", next: "stepper_heat" },
            ],
        },
        belts_check: {
            question:
                "Solution: Check your belt tension. They should be tight like a guitar string but not overstretched.",
            options: [{ text: "Start over", next: "start" }],
        },
        stepper_heat: {
            question:
                "Solution: Your motors might be overheating or current is too low. Check the Vref on your drivers.",
            options: [{ text: "Start over", next: "start" }],
        },
        warping: {
            question: "Are the corners lifting or is the whole part curling?",
            options: [
                { text: "Corners lifting", next: "brim_check" },
                { text: "Large curl", next: "enclosure_check" },
            ],
        },
        brim_check: {
            question:
                "Solution: Use a 'Brim' or 'Raft' in your slicer settings to increase surface area.",
            options: [{ text: "Start over", next: "start" }],
        },
        enclosure_check: {
            question:
                "Solution: Avoid drafts! Use an enclosure, especially for ABS/ASA materials.",
            options: [{ text: "Start over", next: "start" }],
        },
        temp_check: {
            question:
                "Is your bed actually at the temperature you set? (Check with a thermometer if possible)",
            options: [
                { text: "Yes, it matches", next: "bed_prep" },
                { text: "No, it's lower", next: "pid_tune" },
            ],
        },
        pid_tune: {
            question:
                "Solution: Perform a PID Tune on your bed and nozzle to ensure stable temperatures.",
            options: [{ text: "Start over", next: "start" }],
        },
    };

    /** @param {string} next */
    function selectOption(next) {
        currentStep = next;
    }
</script>

<div class="troubleshoot-container">
    <h2>Interactive Troubleshooting</h2>

    <div class="step-card">
        <Card icon="fa-solid fa-stethoscope" title="Diagnostics">
            <div class="content">
                <p class="question">
                    {steps[/** @type {keyof typeof steps} */ (currentStep)]
                        .question}
                </p>

                <div class="options">
                    {#each steps[/** @type {keyof typeof steps} */ (currentStep)].options as option}
                        <button onclick={() => selectOption(option.next)}>
                            {option.text}
                        </button>
                    {/each}
                </div>
            </div>
        </Card>
    </div>

    <div class="back-link">
        <a href="/">← Back to Hub</a>
    </div>
</div>

<style>
    .troubleshoot-container {
        padding: 40px 10%;
        max-width: 800px;
        margin: 0 auto;
    }

    .question {
        font-size: 1.4rem;
        margin-bottom: 30px;
        color: var(--dark);
        line-height: 1.4;
    }

    .options {
        display: flex;
        flex-direction: column;
        gap: 15px;
    }

    button {
        padding: 15px 25px;
        background: white;
        border: 2px solid var(--primary);
        border-radius: 12px;
        font-size: 1.1rem;
        cursor: pointer;
        transition: all 0.2s ease;
        text-align: left;
        font-weight: 600;
    }

    button:hover {
        background: var(--primary);
        color: white;
        transform: translateX(5px);
    }

    .back-link {
        margin-top: 40px;
        text-align: center;
    }

    .back-link a {
        color: var(--primary);
        text-decoration: none;
        font-weight: 600;
    }
</style>
