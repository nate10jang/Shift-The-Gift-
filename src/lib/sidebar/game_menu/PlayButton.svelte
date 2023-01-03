<script lang="ts">
    import Matter from "matter-js";

	const {
		Engine,
		Render,
		Runner,
		Bodies,
		Composite,
		Mouse,
		MouseConstraint,
		Vector,
		Bounds,
		Events,
		Body,
	} = Matter;

    export let engine; 
	
	let started = false;

    function start() {
		started = true;

		engine.gravity.y = 1;
    }
</script>

<button on:click={start} class="bg-green-400 w-1/2">
    <p class="text-[20px]">
		{#if !started}Play{:else}Replay{/if}
	</p>
</button>