<script lang="ts">
	import { onMount } from "svelte";
	import Matter from "matter-js";
	import presentImage from "../assets/present.png";

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

	const engine = Engine.create();
	let canvasContainer: HTMLDivElement;
	let canvas: HTMLCanvasElement;
	let render;
	let mouse;
	let mouseConstraint;
	let draggedPoint = Vector.create(0, 0);
	let held;

	const boxA = Bodies.rectangle(1300, 400, 80, 80, { isStatic: true });
	const boxB = Bodies.rectangle(100, 50, 80, 80, {
		render: {
			sprite: {
				texture: presentImage,
				xScale: 0.14,
				yScale: 0.13,
			},
		},
	});
	const ground = Bodies.rectangle(395, 505, 791, 10, { isStatic: true });

	Composite.add(engine.world, [boxA, boxB, ground]);

	const runner = Runner.create();
	Runner.run(runner, engine);

	// Executed after elements loaded
	onMount(async () => {
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
		mouseConstraint = MouseConstraint.create(engine, {
			mouse: mouse,
			constraint: {
				stiffness: 0.2,
				render: {
					visible: false,
				},
			},
		});

		Composite.add(engine.world, mouseConstraint);

		let extents = {
			min: { x: -600, y: -300 },
			max: { x: 1400, y: 900 },
		};

		let boundsScaleTarget = 1,
			boundsScale = {
				x: 1,
				y: 1,
			};

		render.mouse = mouse;

		let viewPos = Vector.create(0, 0);

		Events.on(mouseConstraint, "mousedown", function () {
			held = true;
			draggedPoint = Object.assign(
				{},
				Vector.add(viewPos, mouse.absolute)
			);
		});
		Events.on(mouseConstraint, "mouseup", function () {
			held = false;
		});

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

<div bind:this={canvasContainer} class="max-w-[1400px] max-h-[1200px] w-full h-full">
	<canvas bind:this={canvas} />
</div>