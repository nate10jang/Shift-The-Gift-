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

    export let components; 
	
	let started = false;

    function startGame() {
		started = true;

		for (let i = 0; i < Composite.allBodies(components).length; i++) {
			Body.setStatic(Composite.allBodies(components)[i], false);
		}
    }
</script>

<button on:click={startGame} class="bg-green-400 w-1/2">
    <p class="text-[20px]">
		{#if !started}Play{:else}Replay{/if}
	</p>
</button>