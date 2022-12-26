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
	} = Matter;

	export let engine;

	function place() {
		const ground = Bodies.rectangle(395, 205, 791, 10, { isStatic: true });

		Composite.add(engine.world, ground);
	}
</script>

<button on:click={place}>Slope</button>
