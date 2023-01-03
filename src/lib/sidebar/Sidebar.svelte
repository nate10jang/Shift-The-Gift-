<script lang="ts">
    import ComponentBar from "./componentbar/ComponentBar.svelte";
    import GameMenu from "./game_menu/GameMenu.svelte";

    export let components;
    export let engine;
</script>

<div class="flex overflow-auto w-[280px] max-h-full flex-col">
    <div class="flex w-full h-[40px] justify-center items-center">
        <p class="text-[20px]">Game</p>
    </div>
    <GameMenu {engine} />
    <ComponentBar {components} />
</div>
