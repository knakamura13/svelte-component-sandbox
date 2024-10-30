<script lang="ts">
    import { createDropdownMenu, melt } from '@melt-ui/svelte';
    import { writable } from 'svelte/store';
    import { ArrowDown, Check } from 'lucide-svelte';
    import { fly } from 'svelte/transition';

    // Writable store for the checkbox item
    const notificationsEnabled = writable(false);

    // Dropdown menu configuration
    const {
        elements: { trigger, menu, item, separator },
        builders: { createCheckboxItem },
        states: { open }
    } = createDropdownMenu({
        loop: true,
        forceVisible: true,
        closeOnItemClick: true,
        positioning: {
            placement: 'bottom-start',
            sameWidth: true // Ensures the menu matches the trigger's width
        }
    });

    // Checkbox item configuration
    const {
        elements: { checkboxItem: notificationsCheckboxItem }
    } = createCheckboxItem({
        checked: notificationsEnabled
    });
</script>

<button class="trigger" type="button" use:melt={$trigger}>
    Options
    <ArrowDown class="size-4 ml-2" />
</button>

{#if $open}
    <div class="menu" use:melt={$menu} transition:fly={{ duration: 150, y: -10 }}>
        <div class="item" use:melt={$item}>Profile</div>
        <div class="item" use:melt={$item}>Settings</div>

        <div class="separator" use:melt={$separator} />

        <div class="item" use:melt={$notificationsCheckboxItem}>
            <div class="check">
                {#if $notificationsEnabled}
                    <Check class="size-4" />
                {/if}
            </div>
            Enable Notifications
        </div>

        <div class="separator" use:melt={$separator} />

        <div class="item" use:melt={$item}>Logout</div>
    </div>
{/if}

<style lang="scss">
    .trigger {
        font-size: 1rem;
        display: flex;
        align-items: center;
        justify-content: center;
        width: 200px;
        padding: 0.5rem 1rem;
        cursor: pointer;
        color: white;
        border: none;
        border-radius: 0.375rem;
        background-color: #3490dc;

        &:hover {
            background-color: #2779bd;
        }
    }

    .menu {
        overflow-y: auto;
        width: 200px;
        max-height: 300px;
        padding: 0.5rem 0;
        border: 1px solid #e2e8f0;
        border-radius: 0.375rem;
        background-color: white;
        box-shadow:
            0 10px 15px -3px rgba(0, 0, 0, 0.1),
            0 4px 6px -2px rgba(0, 0, 0, 0.05);
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
        border-radius: 0.125rem; /* 2px */
        outline: none;
        background-color: transparent;

        &:hover {
            background-color: #edf2f7;
        }

        &[data-highlighted] {
            background-color: #e2e8f0;
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
        }
    }

    .separator {
        height: 1px;
        margin: 0.5rem 0;
        background-color: #e2e8f0;
    }
</style>
