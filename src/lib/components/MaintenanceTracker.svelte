<script>
    let tasks = $state([
        {
            id: 1,
            text: "Clean build plate",
            done: false,
            frequency: "Every print",
        },
        { id: 2, text: "Check belt tension", done: false, frequency: "Weekly" },
        {
            id: 3,
            text: "Lubricate linear rails",
            done: false,
            frequency: "Monthly",
        },
        {
            id: 4,
            text: "Check nozzle for wear",
            done: false,
            frequency: "Monthly",
        },
        {
            id: 5,
            text: "Dry your filament",
            done: false,
            frequency: "If stringing",
        },
    ]);

    /** @param {number} id */
    function toggle(id) {
        const task = tasks.find((t) => t.id === id);
        if (task) task.done = !task.done;
    }
</script>

<div class="tracker">
    <ul>
        {#each tasks as task}
            <li class={task.done ? "done" : ""}>
                <label>
                    <input
                        type="checkbox"
                        checked={task.done}
                        onchange={() => toggle(task.id)}
                    />
                    <span class="text">{task.text}</span>
                    <span class="freq">{task.frequency}</span>
                </label>
            </li>
        {/each}
    </ul>
</div>

<style>
    .tracker ul {
        list-style: none;
        padding: 0;
    }
    li {
        margin-bottom: 12px;
        padding: 10px;
        border-radius: 8px;
        background: white;
        transition: all 0.2s ease;
    }
    li.done {
        opacity: 0.6;
        background: #f0f0f0;
    }
    label {
        display: flex;
        align-items: center;
        cursor: pointer;
        width: 100%;
    }
    input {
        margin-right: 15px;
        width: 20px;
        height: 20px;
        accent-color: var(--primary);
    }
    .text {
        flex-grow: 1;
        font-weight: 600;
    }
    .freq {
        font-size: 0.8rem;
        color: #888;
        background: #eee;
        padding: 2px 8px;
        border-radius: 4px;
    }
    .done .text {
        text-decoration: line-through;
    }
</style>
