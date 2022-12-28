<script lang="ts">
    import Matter from "matter-js";
    import presentImage from "../../assets/present.png";

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
    let started: boolean = false;
    let text: HTMLParagraphElement;
    let buttonColor: string = "bg-green-400";

    function startGame() {
        started = true;
        text.innerText = "Restart";
        buttonColor = "bg-orange-400"

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

<button on:click={startGame}
class="flex justify-center items-center h-[50px] {buttonColor}">
    <p class="text-[20px]" bind:this={text}>Start</p>
</button>