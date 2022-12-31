<script>
    import Slope from "./Slope.svelte";

	export let components;
</script>

<div class="flex w-full h-[40px] justify-center items-center"> <p class="text-[20px]">Components</p> </div>
<Slope components={components}/>