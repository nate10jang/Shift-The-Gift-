<script>
	import ComponentBar from "./components/ComponentBar/index.svelte";
	import World from "./components/World.svelte";
	
	let engine;
</script>

<main>
	<div class="flex flex-row h-screen">
		<World bind:engine={engine}/>
		<ComponentBar engine={engine}/>
	</div>
</main>
