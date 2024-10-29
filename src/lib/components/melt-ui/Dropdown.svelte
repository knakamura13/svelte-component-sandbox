<script lang="ts">
    /**
     * Dropdown.svelte
     *
     * This Melt component required multiple pre-requisite steps:
     *  1. Installed '@tailwindcss/typography'
     *  2. Installed lucide-svelte
     *  3. Replaced tailwind.config.js with config found in the example
     *  4. Once the example was implemented, I made the following additional customizations:
     *      - Removed the sub-menu functionality
     *      - Made the menu scrollable with a constrained max height
     *      - Improved styling for disabled menu items
     *      - Fixed invisible separators with weird pt-[1px] workaround
     *      - Removed the menu arrow since it was incompatible with overflow-scroll
     *
     * Source: https://www.melt-ui.com/docs/builders/dropdown-menu
     */
    import { createDropdownMenu, melt } from '@melt-ui/svelte';
    import { writable } from 'svelte/store';
    import { AlignJustify, Check } from 'lucide-svelte';
    import { fly } from 'svelte/transition';

    const settingsSync = writable(true);
    const hideMeltUI = writable(false);

    const {
        elements: { trigger, menu, item, separator },
        builders: { createCheckboxItem },
        states: { open }
    } = createDropdownMenu({
        forceVisible: true,
        loop: true
    });

    const {
        elements: { checkboxItem }
    } = createCheckboxItem({
        checked: settingsSync
    });

    const {
        elements: { checkboxItem: checkboxItemA }
    } = createCheckboxItem({
        checked: hideMeltUI
    });
</script>

<button aria-label="Update dimensions" class="trigger" type="button" use:melt={$trigger}>
    <AlignJustify class="size-4" />
    <span class="sr-only">Open Popover</span>
</button>

{#if $open}
    <div class="menu" use:melt={$menu} transition:fly={{ duration: 150, y: -10 }}>
        <div class="item" use:melt={$item}>About Melt UI</div>
        <div class="item" use:melt={$item}>Check for Updates...</div>

        <div class="separator" use:melt={$separator} />

        <div class="item" use:melt={$checkboxItem}>
            <div class="check">
                {#if $settingsSync}
                    <Check class="size-4" />
                {/if}
            </div>
            Settings Sync is On
        </div>

        <div use:melt={$separator} class="separator" />

        <div class="item" use:melt={$checkboxItemA}>
            <div class="check">
                {#if $hideMeltUI}
                    <Check class="size-4" />
                {/if}
            </div>
            Hide Melt UI
            <div class="rightSlot">⌘H</div>
        </div>

        <div class="item" use:melt={$item} data-disabled>
            Show All Components
            <div class="rightSlot">⇧⌘N</div>
        </div>

        <div use:melt={$separator} class="separator" />

        <div class="item" use:melt={$item}>
            Quit Melt UI
            <div class="rightSlot">⌘Q</div>
        </div>
    </div>
{/if}

<style lang="postcss">
    .menu {
        @apply z-40 flex max-h-[300px] min-w-[220px] flex-col shadow-lg;
        @apply rounded-md bg-white p-1 shadow-neutral-900/30;
        @apply ring-0 overflow-y-scroll !important;
    }

    .item {
        @apply relative h-6 min-h-[24px] select-none cursor-pointer rounded-sm pl-6 pr-1;
        @apply z-40 text-magnum-900 outline-none;
        @apply hover:text-magnum-900;
        @apply flex items-center text-sm leading-none;
        @apply ring-0 !important;
    }

    /* Apply bg-magnum-200 on hover only when not disabled */
    .item:not([data-disabled]):hover {
        @apply bg-magnum-200;
    }

    /* Styles for highlighted items */
    .item[data-highlighted] {
        @apply bg-magnum-200 text-magnum-900;
    }

    /* Styles for disabled items */
    .item[data-disabled],
    .item[data-disabled]:hover {
        @apply bg-transparent text-neutral-300 cursor-default;
    }

    .trigger {
        @apply inline-flex h-9 w-9 items-center justify-center rounded-full bg-white;
        @apply text-magnum-900 transition-colors hover:bg-white/90;
        @apply data-[highlighted]:ring-magnum-400 data-[highlighted]:ring-offset-2 !important;
        @apply p-0 text-sm font-medium data-[highlighted]:outline-none;
    }
    .check {
        @apply absolute left-2 top-1/2 text-magnum-500;
        translate: 0 calc(-50% + 1px);
    }

    .separator {
        @apply m-[5px] h-[1px] pt-[1px] bg-magnum-200;
    }

    .rightSlot {
        @apply ml-auto pl-5;
    }

    .check {
        @apply absolute left-0 inline-flex w-6 items-center justify-center;
    }
</style>
