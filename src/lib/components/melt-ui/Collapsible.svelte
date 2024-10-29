<script>
    /**
     * Collapsible.svelte
     *
     * This is the most basic example usage of a component from Melt UI.
     * Source: https://www.melt-ui.com/docs/usage
     */
    import { createCollapsible, melt } from '@melt-ui/svelte';
    const {
        elements: { root, content, trigger },
        states: { open }
    } = createCollapsible();
</script>

<div use:melt={$root}>
    <button use:melt={$trigger}>{$open ? 'Close' : 'Open'}</button>
    <div use:melt={$content}>Obi-Wan says: Hello there!</div>
</div>
