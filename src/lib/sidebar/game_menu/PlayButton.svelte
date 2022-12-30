<script lang="ts">
    import Matter from "matter-js";
    import presentImage from "../../../assets/present.png";

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
	} = Matter;

    export let engine
	
	let started = false;

    function startGame() {
		started = true;

	    const present = Bodies.rectangle(100, 50, 80, 80, {
		    render: {
			    sprite: {
				    texture: presentImage,
				    xScale: 0.14,
	    			yScale: 0.13,
	    		},
    		},
	    });

        Composite.add(engine.world, present)
    }
</script>

<button on:click={startGame} class="bg-green-400 w-1/2">
    <p class="text-[20px]">
		{#if !started}Play{:else}Replay{/if}
	</p>
</button>