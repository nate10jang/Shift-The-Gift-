<script lang="ts">
    import ComponentBar from "./componentbar/ComponentBar.svelte";
    import StartButton from "./StartButton.svelte";

    export let engine;
</script>

<div class="flex overflow-auto w-[280px] max-h-full flex-col">
    <div class="flex w-full h-[40px] justify-center items-center">
        <p class="text-[20px]">Game</p>
    </div>
    <StartButton {engine} />
    <ComponentBar {engine} />
</div>
