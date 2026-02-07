<script>
    import Card from "$lib/components/Card.svelte";

    let searchQuery = $state("");

    const materials = [
        {
            name: "PLA",
            type: "Beginner",
            temp: "190-220°C",
            pros: "Easy, low smell, biodegradable",
            cons: "Low heat resistance, brittle",
        },
        {
            name: "PETG",
            type: "Intermediate",
            temp: "230-250°C",
            pros: "Strong, heat resistant, flexible",
            cons: "Sticky, stringing, moisture sensitive",
        },
        {
            name: "ABS",
            type: "Advanced",
            temp: "240-260°C",
            pros: "Very durable, sandable, heat resistant",
            cons: "Warping, toxic fumes, needs enclosure",
        },
        {
            name: "TPU",
            type: "Advanced",
            temp: "220-240°C",
            pros: "Elastic, durable, impact resistant",
            cons: "Difficult to print, slow speed needed",
        },
        {
            name: "ASA",
            type: "Advanced",
            temp: "240-260°C",
            pros: "UV resistant, durable, like ABS",
            cons: "Warping, fumes",
        },
        {
            name: "Nylon",
            type: "Expert",
            temp: "240-270°C",
            pros: "Extremely strong, wear resistant",
            cons: "Very moisture sensitive, difficult adhesion",
        },
    ];

    let filteredMaterials = $derived(
        materials.filter(
            (m) =>
                m.name.toLowerCase().includes(searchQuery.toLowerCase()) ||
                m.type.toLowerCase().includes(searchQuery.toLowerCase()),
        ),
    );
</script>

<div class="database-container">
    <div class="search-bar">
        <i class="fa-solid fa-magnifying-glass"></i>
        <input
            type="text"
            bind:value={searchQuery}
            placeholder="Search materials (e.g. PLA, Advanced)..."
        />
    </div>

    <div class="grid">
        {#each filteredMaterials as material}
            <Card title={material.name}>
                <div class="material-info">
                    <p>
                        <strong>Level:</strong>
                        <span class="tag">{material.type}</span>
                    </p>
                    <p><strong>Temp:</strong> {material.temp}</p>
                    <p class="pros">✓ {material.pros}</p>
                    <p class="cons">✗ {material.cons}</p>
                </div>
            </Card>
        {/each}
    </div>

    {#if filteredMaterials.length === 0}
        <p class="no-results">No materials found matching "{searchQuery}"</p>
    {/if}
</div>

<style>
    .database-container {
        padding: 20px 0;
    }

    .search-bar {
        position: relative;
        margin-bottom: 30px;
        max-width: 500px;
    }

    .search-bar i {
        position: absolute;
        left: 15px;
        top: 50%;
        transform: translateY(-50%);
        color: var(--primary);
    }

    input {
        width: 100%;
        padding: 15px 15px 15px 45px;
        border: 2px solid #eee;
        border-radius: 12px;
        font-size: 1rem;
        transition: border-color 0.2s;
    }

    input:focus {
        outline: none;
        border-color: var(--primary);
    }

    .material-info p {
        margin: 8px 0;
        font-size: 0.95rem;
    }

    .tag {
        background: var(--light);
        padding: 2px 8px;
        border-radius: 4px;
        font-size: 0.8rem;
        font-weight: 600;
    }

    .pros {
        color: #2e7d32;
    }
    .cons {
        color: #c62828;
    }

    .no-results {
        text-align: center;
        padding: 40px;
        color: #888;
    }
</style>
