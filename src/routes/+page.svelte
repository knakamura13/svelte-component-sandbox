<script lang="ts">
    import Dropdown from '$lib/components/melt-ui/Dropdown.svelte';
    import DropdownIcon from '$lib/components/melt-ui/DropdownIcon.svelte';
    import DropdownWide from '$lib/components/melt-ui/DropdownWide.svelte';
</script>

<div class="page prose" id="home">
    <section class="intro">
        <h1 class="prose-h1">Svelte Component Sandbox</h1>
    </section>

    <section class="melt-ui">
        <h2 class="prose-h2">Melt UI</h2>
        <p class="prose-strong">Dropdown Menu</p>
        <Dropdown />
        <DropdownIcon />
        <DropdownWide />
    </section>
</div>

<style lang="scss">
</style>
