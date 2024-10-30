<script lang="ts">
    import { createDropdownMenu, melt } from '@melt-ui/svelte';
    import { Bell, Check } from 'lucide-svelte';
    import { writable } from 'svelte/store';
    import { fly } from 'svelte/transition';

    // Writable stores for checkbox items
    const emailNotifications = writable(true);
    const smsNotifications = writable(false);

    // Dropdown menu configuration
    const {
        elements: { trigger, menu, item, separator },
        builders: { createCheckboxItem },
        states: { open }
    } = createDropdownMenu({
        loop: true,
        forceVisible: true,
        closeOnItemClick: false,
        positioning: {
            placement: 'bottom-start'
        }
    });

    // Checkbox item configurations
    const {
        elements: { checkboxItem: emailCheckboxItem }
    } = createCheckboxItem({
        checked: emailNotifications
    });

    const {
        elements: { checkboxItem: smsCheckboxItem }
    } = createCheckboxItem({
        checked: smsNotifications
    });
</script>

<button class="trigger" type="button" use:melt={$trigger}>
    <Bell class="size-4" />
    <span class="sr-only">Notifications</span>
</button>

{#if $open}
    <div class="menu" use:melt={$menu} transition:fly={{ duration: 150, y: -10 }}>
        <div class="item" use:melt={$emailCheckboxItem}>
            <div class="check">
                {#if $emailNotifications}
                    <Check class="size-4" />
                {/if}
            </div>
            Email Notifications
        </div>
        <div class="item" use:melt={$smsCheckboxItem}>
            <div class="check">
                {#if $smsNotifications}
                    <Check class="size-4" />
                {/if}
            </div>
            SMS Notifications
        </div>

        <div class="separator" use:melt={$separator} />

        <div class="item" use:melt={$item}>Notification Settings</div>
    </div>
{/if}

<style lang="scss">
    .trigger {
        display: inline-flex;
        align-items: center;
        justify-content: center;
        width: 2.25rem;
        height: 2.25rem;
        cursor: pointer;
        color: white;
        border: none;
        border-radius: 9999px;
        background-color: #38b2ac;

        &:hover {
            background-color: #319795;
        }
    }

    .menu {
        overflow-y: auto;
        min-width: 180px;
        max-height: 200px;
        padding: 0.5rem 0;
        border: 1px solid #cbd5e0;
        border-radius: 0.375rem;
        background-color: white;
        box-shadow:
            0 10px 15px -3px rgba(0, 0, 0, 0.1),
            0 4px 6px -2px rgba(0, 0, 0, 0.05);
    }

    .item {
        font-size: 0.875rem;
        display: flex;
        align-items: center;
        padding: 0.5rem 1rem;
        cursor: pointer;
        color: #4a5568;

        &:hover {
            background-color: #e6fffa;
        }

        &[data-highlighted] {
            background-color: #b2f5ea;
        }
    }

    .separator {
        height: 1px;
        margin: 0.5rem 0;
        background-color: #e2e8f0;
    }

    .check {
        margin-right: 0.5rem;
    }
</style>
