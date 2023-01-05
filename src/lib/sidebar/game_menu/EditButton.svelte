<script lang="ts">
    export let engine;

    function edit() {
        // Turns off gravity for edit mode
        // likely to be replaced by a function in GameState.ts
        engine.gravity.y = 0;
    }
</script>

<button class="bg-orange-400 w-1/2" on:click={edit}>
    <p>Edit</p>
</button>