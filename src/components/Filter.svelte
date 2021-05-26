<script lang="ts">
    import { auth, db, rtdb } from "../services/firebase";
    import router from "page";
    export let countyTeams: string[];
    export let clubTeams: string[];
    let hurling = "Hurling";
    let hurlingChecked = false;
    let football = "Football";
    let footballChecked = false;

    let countyTeamsChecked: boolean[] = [];
    let clubTeamsChecked: boolean[] = [];
    let x = 0;
    while (x < countyTeams.length) {
        countyTeamsChecked[x] = false;
        x++;
    }
    let y = 0;
    while (y < clubTeams.length) {
        clubTeamsChecked[y] = false;
        y++;
    }
</script>

<div class="card">
    <div class="row no-gutters">
        <div class="col-12 card-body">
            <h3>Filter Games</h3>
            <hr />
            <h3>Sport Type</h3>
            <hr />
            <div class="col-12 filter-label">
                <label>
                    <input type="checkbox" bind:checked={hurlingChecked} />
                    {hurling || ""}
                </label>
            </div>
            <div class="col-12 filter-label">
                <label>
                    <input type="checkbox" bind:checked={footballChecked} />
                    {football || ""}
                </label>
            </div>
            <hr />
            <h3>County Teams</h3>
            <hr />
            {#if countyTeams.length > 0}
                {#each countyTeams as team}
                    <div class="col-12 filter-label">
                        <label>
                            <input
                                type="checkbox"
                                bind:checked={countyTeamsChecked[
                                    countyTeams.indexOf(team)
                                ]}
                            />
                            {team || ""}
                        </label>
                    </div>
                {/each}
            {:else}
                <p>No County Games Today</p>
            {/if}
            <hr />
            <h3>Club Teams</h3>
            <hr />
            {#if clubTeams.length > 0}
                {#each clubTeams as team}
                    <div class="col-12 filter-label">
                        <label>
                            <input
                                type="checkbox"
                                bind:checked={clubTeamsChecked[
                                    clubTeams.indexOf(team)
                                ]}
                            />
                            {team || ""}
                        </label>
                    </div>
                {/each}
            {:else}
                <p>No County Games Today</p>
            {/if}
        </div>
    </div>
</div>

<style>
    label {
        font-size: x-large;
        padding: 10px;
    }
    input {
        height: 1em;
        width: 1em;
        margin: 0px 10px;
    }
    .card {
        height: 95%;
    }
    h3 {
        margin: 20px;
    }
</style>
