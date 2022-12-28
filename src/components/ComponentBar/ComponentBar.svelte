<script>
    import Slope from "./Slope.svelte";

	export let engine;
</script>

<div class="flex overflow-auto w-[280px] max-h-full flex-col">
	<Slope engine={engine}/>
</div>
