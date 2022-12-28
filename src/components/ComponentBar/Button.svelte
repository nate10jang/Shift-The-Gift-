<script lang="ts">
    export let name: string;
</script>

<button class="flex h-[180px] justify-center items-center bg-blue-600 hover:opacity-90 active:opacity-80" on:click>
    <p class="text-[20px] text-white">{name}</p>
</button>