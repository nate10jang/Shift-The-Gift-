<script lang="ts">
	import StartButton from "./PlayButton.svelte";
	import EditButton from "./EditButton.svelte";
	import { GameState } from "../../GameState";

	export let components;
</script>

<div class="h-[50px] flex-row">
	<StartButton {components} />
	<EditButton {components} />
</div>
