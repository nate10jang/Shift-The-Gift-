<script>
    import { end_hydrating } from "svelte/internal";
	import Sidebar from "./lib/sidebar/Sidebar.svelte";
	import World from "./lib/World.svelte";
	
	let components;
	let engine;
</script>

<main>
	<div class="flex flex-row h-screen">
		<World bind:components={components} bind:engine={engine}/>
		<Sidebar engine={engine} components={components}/>
	</div>
</main>
