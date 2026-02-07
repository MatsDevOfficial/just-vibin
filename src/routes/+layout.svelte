<script>
	import "../app.css";
	import favicon from "$lib/assets/favicon.svg";

	let { children } = $props();
	let isVibe = $state(false);

	function toggleVibe() {
		isVibe = !isVibe;
		document.body.classList.toggle("vibe-theme");
	}
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<link
		rel="stylesheet"
		href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css"
	/>
	<link
		href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&family=Orbitron:wght@400;700&display=swap"
		rel="stylesheet"
	/>
	<title>3D Printing Hub</title>
</svelte:head>

<header class={isVibe ? "vibe-header" : ""}>
	<h1><i class="fa-solid fa-cube"></i> 3D Printing Hub</h1>
	<p>Printers, tips, tricks, and savings for makers</p>
	<button class="vibe-toggle" onclick={toggleVibe}>
		{isVibe ? "🚀 Normal Mode" : "🌌 Vibe Mode"}
	</button>
</header>

<nav>
	<a href="/">Home</a>
	<a href="/#tools">Tools</a>
	<a href="/troubleshoot">Troubleshoot</a>
	<a href="/#materials">Materials</a>
	<a href="/#calculator">Calculator</a>
</nav>

<main>
	{@render children()}
</main>

<footer>
	<p>© 2026 3D Printing Hub · Built with SvelteKit</p>
</footer>
