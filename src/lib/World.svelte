<script lang="ts">
	import { onMount } from "svelte";
	import Matter from "matter-js";
	import presentImage from "../assets/present.png";
	import ComponentBar from "./sidebar/componentbar/ComponentBar.svelte";

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

	export const engine = Engine.create();
	let canvasContainer: HTMLDivElement;
	let canvas: HTMLCanvasElement;
	let render;
	let mouse;
	let mouseConstraint;
	let draggedPoint = Vector.create(0, 0);
	let held;

	const runner = Runner.create();
	Runner.run(runner, engine);

	// gravity is set to zero to start as edit mode
	// likely to be replaced by a function in GameState.ts
	engine.gravity.y = 0;

	const ground = Bodies.rectangle(400, 655, 1400, 10, { isStatic: true });
	export const components = Composite.create();
	const present = Bodies.rectangle(100, 50, 80, 80, {
		render: {
			sprite: {
				texture: presentImage,
				xScale: 0.14,
				yScale: 0.13,
			},
		},
		label: "present",
	});

	Composite.add(components, [present]);
	Composite.add(engine.world, [ground, components]);

	// Scope executed after canvas loaded
	// because Using canvas properties outside of the scope
	// will result in null
	onMount(async () => {
		// cannot use css to size canvas
		// so used a container and made canvas fit it
		// using typescript/javascript
		canvas.width = canvasContainer.clientWidth;
		canvas.height = canvasContainer.clientHeight;

		render = Render.create({
			engine: engine,
			canvas: canvas,
			options: {
				width: canvas.width,
				height: canvas.height,
				wireframes: false,
				hasBounds: true,
			},
		});

		Render.run(render);

		mouse = Mouse.create(render.canvas);
		render.mouse = mouse;
		mouseConstraint = MouseConstraint.create(engine, {
			mouse: mouse,
			constraint: {
				stiffness: 1,
				render: {
					visible: false,
				},
			},
		});
		
		Composite.add(engine.world, mouseConstraint);

		// World size
		let extents = {
			min: { x: -600, y: -300 },
			max: { x: 1400, y: 900 },
		};
		let boundsScaleTarget = 1,
			boundsScale = {
				x: 1,
				y: 1,
			};

		let viewPos = Vector.create(0, 0);

		// document.addEventListener used because
		// Events.on(mouseConstraint) has an issue where
		// when the player holds their mouse while moving it off the canvas,
		// it still thinks that the mouse is held when the mouse is back on the canvas.
		// document.addEventListener captures the mouse clciks on the whole page
		// but when dragging on anything other than the world, it will stay
		// the same because the mouse positions are restricted to the bounds.
		// this may not be the best solution and will probably soon be changed.
		document.addEventListener("mousedown", function () {
			held = true;
			
			// Object.assign is used because using
			// "= Vector.add(viewPos, mouse.absolute)" will
			// reference it instead of assigning it hence Object.assign
			draggedPoint = Object.assign(
				{},
				Vector.add(viewPos, mouse.absolute)
			);
		});
		document.addEventListener("mouseup", function () {
			held = false;
		});

		// After moving the object in edit mode,
		// the object may be flung so after the object is dragged
		// the velocity on the object gets reset.
		Events.on(mouseConstraint, "enddrag", function (event) {
			Body.setVelocity(event.body, Vector.create(0, 0));
		});

		Events.on(engine, "afterUpdate", function () {
			// Object violently spins when collided (some cases)
			// This slows the velocity down by 80% after every update/frame
			if (mouseConstraint.body) {
				Body.setAngularVelocity(mouseConstraint.body, mouseConstraint.body.angularVelocity * 0.8);
			}
		})
		Events.on(render, "beforeRender", function () {
			let scaleFactor = mouse.wheelDelta * -0.1;
			if (scaleFactor !== 0) {
				if (
					(scaleFactor < 0 && boundsScale.x >= 0.6) ||
					(scaleFactor > 0 && boundsScale.x <= 1.4)
				) {
					boundsScaleTarget += scaleFactor;
				}
			}

			if (Math.abs(boundsScale.x - boundsScaleTarget) > 0.01) {
				scaleFactor = (boundsScaleTarget - boundsScale.x) * 0.2;
				boundsScale.x += scaleFactor;
				boundsScale.y += scaleFactor;

				render.bounds.max.x =
					render.bounds.min.x + render.options.width * boundsScale.x;
				render.bounds.max.y =
					render.bounds.min.y + render.options.height * boundsScale.y;

				viewPos = {
					x: render.options.width * scaleFactor * -0.5,
					y: render.options.height * scaleFactor * -0.5,
				};

				Bounds.shift(render.bounds, viewPos);

				Mouse.setScale(mouse, boundsScale);
				Mouse.setOffset(mouse, render.bounds.min);
			}

			// !mouseConstraint.body so that camera doesn't move
			// when dragging an object.
			if (held && !mouseConstraint.body) {
				viewPos = Vector.sub(draggedPoint, mouse.absolute);

				if (render.bounds.min.x + viewPos.x < extents.min.x)
					viewPos.x = extents.min.x - render.bounds.min.x;

				if (render.bounds.max.x + viewPos.x > extents.max.x)
					viewPos.x = extents.max.x - render.bounds.max.x;

				if (render.bounds.min.y + viewPos.y < extents.min.y)
					viewPos.y = extents.min.y - render.bounds.min.y;

				if (render.bounds.max.y + viewPos.y > extents.max.y)
					viewPos.y = extents.max.y - render.bounds.max.y;

				Bounds.shift(render.bounds, viewPos);

				Mouse.setOffset(mouse, render.bounds.min);
			}
		});
	});
</script>

<div
	bind:this={canvasContainer}
	class="max-w-[1400px] max-h-[1200px] w-full h-full"
>
	<canvas bind:this={canvas} />
</div>
