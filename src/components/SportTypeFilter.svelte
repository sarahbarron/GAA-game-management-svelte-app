<script lang="ts">
    import { hurlingChecked, footballChecked } from "../services/store";
    import { onDestroy } from "svelte";
    import { filterGames } from "../services/filterList";
    import DropdownHeaderFilter from "./dropdownHeaderFilter.svelte";
    let hurling = "hurling";
    let football = "football";
    let hurlingX: boolean;
    let footballX: boolean;
    let heading: string = "Sport Type";

    // Subscibe to store value
    const unsubscribeHurlingChecked = hurlingChecked.subscribe((value) => {
        hurlingX = value;
    });
    onDestroy(unsubscribeHurlingChecked);

    const unsubscribeFootballChecked = footballChecked.subscribe((value) => {
        footballX = value;
    });
    onDestroy(unsubscribeFootballChecked);

    // If a checkbox is checked or unchecked update the filter
    $: if (hurlingX || !hurlingX) {
        hurlingChecked.set(hurlingX);
        filterGames();
    }
    $: if (footballX || !footballX) {
        footballChecked.set(footballX);
        filterGames();
    }
</script>

<!-- Sport Type Drop Down -->
<li class="nav-item dropdown dropdown-list-header">
    <DropdownHeaderFilter {heading} />
    <ul
        class="dropdown-menu dropdown-menu-dark dropdown-menu-end"
        aria-labelledby="navbarDarkDropdownMenuLink"
    >
        <li>
            <input
                class="form-check-input dropdown-list-checkbox"
                type="checkbox"
                value="hurling"
                id="hurlingChecked"
                bind:checked={hurlingX}
            />
            <label
                class="form-check-label dropdown-list-label"
                for="hurlingChecked"
            >
                {hurling}
            </label>
        </li>
        <li>
            <input
                class="form-check-input dropdown-list-checkbox"
                type="checkbox"
                value="football"
                id="footballChecked"
                bind:checked={footballX}
            />
            <label
                class="form-check-label dropdown-list-label"
                for="footballChecked"
            >
                {football}
            </label>
        </li>
    </ul>
</li>

<style>
    @media (min-width: 768px) {
        .dropdown-list-checkbox {
            height: 1em;
            width: 1em;
            margin: 20px 10px;
            padding: 12px;
        }
        .dropdown-list-header {
            font-size: x-large;
            padding: 5px 50px;
        }
        .dropdown-list-label {
            font-size: x-large;
            padding: 12px 12px 12px 0px;
        }
    }
</style>
