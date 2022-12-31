<script>
	import Sidebar from "./lib/sidebar/Sidebar.svelte";
	import World from "./lib/World.svelte";
	
	let components;
</script>

<main>
	<div class="flex flex-row h-screen">
		<World bind:components={components}/>
		<Sidebar components={components}/>
	</div>
</main>
