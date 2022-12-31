<script lang="ts">
	import Matter from "matter-js";
	import Button from "./Button.svelte";

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

	export let components;

	function place() {
		const ground = Bodies.rectangle(395, 205, 791, 10, { isStatic: true });

		Composite.add(components, ground);
	}
</script>

<Button on:click={place} name="Slope"></Button>