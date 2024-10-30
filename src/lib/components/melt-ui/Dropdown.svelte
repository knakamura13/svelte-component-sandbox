<script lang="ts">
    /**
     * Dropdown.svelte
     *
     * This Melt component implementation required multiple pre-requisite steps:
     *  1. Installed '@tailwindcss/typography'
     *  2. Installed 'lucide-svelte' icon library
     *  3. Replaced tailwind.config.js with config found in the example
     *  4. Once the example was implemented, I made the following additional customizations:
     *      - Removed the sub-menu functionality
     *      - Removed the menu arrow since it was incompatible with overflow-scroll
     *      - Made the menu scrollable with a constrained max height
     *      - Improved styling for disabled menu items
     *      - Fixed invisible separators with weird pt-[1px] workaround
     *      - Converted Tailwind to SCSS
     *      - Implemented custom defaults for menu options
     *
     * Source: https://www.melt-ui.com/docs/builders/dropdown-menu
     */
    import { createDropdownMenu, melt } from '@melt-ui/svelte';
    import { writable } from 'svelte/store';
    import { AlignJustify, Check } from 'lucide-svelte';
    import { fly } from 'svelte/transition';

    // Writable stores for checkbox items
    const settingsSync = writable(true);
    const hideMeltUI = writable(false);

    // Dropdown menu configuration
    const {
        elements: { trigger, menu, item, separator },
        builders: { createCheckboxItem },
        states: { open }
    } = createDropdownMenu({
        loop: false,
        forceVisible: true,
        closeOnItemClick: false,
        positioning: {
            placement: 'bottom'
        }
    });

    // Dropdown item configuration: setting sync
    const {
        elements: { checkboxItem: settingsCheckboxItem }
    } = createCheckboxItem({
        checked: settingsSync
    });

    // Dropdown item configuration: hide Melt UI
    const {
        elements: { checkboxItem: hideMeltCheckboxItem }
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

        <div class="item" use:melt={$settingsCheckboxItem}>
            <div class="check">
                {#if $settingsSync}
                    <Check class="size-4" />
                {/if}
            </div>
            Settings Sync is On
        </div>

        <div use:melt={$separator} class="separator" />

        <div class="item" use:melt={$hideMeltCheckboxItem}>
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

<style lang="scss">
    $magnum-900: #793a15;
    $magnum-500: #f38d1c;
    $magnum-400: #f7b155;
    $magnum-200: #fce0ac;
    $neutral-300: #d4d4d4;

    .menu {
        z-index: 40;
        display: flex;
        overflow-y: auto;
        flex-direction: column;
        min-width: 220px;
        max-height: 300px;
        padding: 0.25rem; /* 4px */
        border-radius: 0.375rem; /* 6px */
        outline: 0;
        background-color: white;
        box-shadow:
            0 10px 15px -3px rgba(23, 23, 23, 0.3),
            0 4px 6px -2px rgba(23, 23, 23, 0.15);
    }

    .item {
        font-size: 0.875rem; /* 14px */
        font-weight: normal;
        line-height: 1;
        position: relative;
        z-index: 40;
        display: flex;
        align-items: center;
        height: 1.5rem; /* 24px */
        min-height: 24px;
        padding-right: 0.25rem; /* 4px */
        padding-left: 1.5rem; /* 24px */
        cursor: pointer;
        user-select: none;
        color: $magnum-900;
        border-radius: 0.125rem; /* 2px */
        outline: none;
        background-color: transparent;

        &:hover {
            color: $magnum-900;
        }

        &:not([data-disabled]):hover {
            background-color: $magnum-200;
        }

        &[data-highlighted] {
            color: $magnum-900;
            background-color: $magnum-200;
        }

        &[data-disabled],
        &[data-disabled]:hover {
            cursor: default;
            color: $neutral-300;
            background-color: transparent;
        }
    }

    .trigger {
        font-size: 0.875rem; /* 14px */
        font-weight: 500;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        width: 2.25rem; /* 36px */
        height: 2.25rem; /* 36px */
        margin: 0;
        padding: 0;
        transition:
            color 150ms ease-in-out,
            background-color 150ms ease-in-out;
        color: $magnum-900;
        border-radius: 9999px;
        background-color: white;
        box-shadow:
            0 1px 6px -3px rgba(23, 23, 23, 0.1),
            0 1px 6px -2px rgba(23, 23, 23, 0.1);

        &:hover {
            background-color: $magnum-200;
        }

        &[data-highlighted] {
            outline: none;
            outline-offset: 2px;
            box-shadow: 0 0 0 2px $magnum-200;
        }
    }

    .check {
        position: absolute;
        top: 50%;
        left: 0;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        width: 1.5rem; /* 24px */
        transform: translateY(calc(-50% + 1px));
        color: $magnum-500;
    }

    .separator {
        height: 1px;
        margin: 5px;
        padding-top: 1px;
        background-color: $magnum-200;
    }

    .rightSlot {
        margin-left: auto;
        padding-left: 1.25rem; /* 20px */
    }
</style>
