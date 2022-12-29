<script>
	import Sidebar from "./lib/sidebar/Sidebar.svelte";
	import World from "./lib/World.svelte";
	
	let engine;
</script>

<main>
	<div class="flex flex-row h-screen">
		<World bind:engine={engine}/>
		<Sidebar engine={engine}/>
	</div>
</main>
