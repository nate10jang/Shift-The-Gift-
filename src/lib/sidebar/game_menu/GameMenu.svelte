<script lang="ts">
	import StartButton from "./PlayButton.svelte";
	import EditButton from "./EditButton.svelte";
	import { GameState } from "../../GameState";

	export let engine;
</script>

<div class="h-[50px]">
	<EditButton {engine} />
	<StartButton {engine} />
</div>
