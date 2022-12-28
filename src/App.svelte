<script>
	import Sidebar from "./components/sidebar/Sidebar.svelte";
	import World from "./components/World.svelte";
	
	let engine;
</script>

<main>
	<div class="flex flex-row h-screen">
		<World bind:engine={engine}/>
		<Sidebar engine={engine}/>
	</div>
</main>
