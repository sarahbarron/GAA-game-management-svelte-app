<script lang="ts">
    import { auth, db, rtdb } from "../services/firebase";
    import router from "page";
    export let childKey: string;
    export let competitionName: string;
    export let teamACrest: string;
    export let teamAName: string;
    export let teamBCrest: string;
    export let teamBName: string;
    export let startTime: string;
    let teamAGoals: number;
    let teamAPoints: number;
    let teamBGoals: number;
    let teamBPoints: number;
    let latestPlayer: string;
    let latestTime: string;
    let latestScoreType: string;
    let latestTeam: string;

    latestPlayer = undefined;
    latestTime = undefined;
    latestScoreType = undefined;
    latestTeam = undefined;

    const latestScoreListener = rtdb
        .ref(`games/${childKey}/latest`)
        .on("value", function (snapshot) {
            var data = snapshot.val();

            if (data != null) {
                console.log(data);

                teamAGoals = snapshot.val().teamAGoals;
                teamBGoals = snapshot.val().teamBGoals;
                teamAPoints = snapshot.val().teamAPoints;
                teamBPoints = snapshot.val().teamBPoints;
                latestPlayer = snapshot.val().latestPlayer;
                latestScoreType = snapshot.val().latestScoreType;
                latestTeam = snapshot.val().latestTeamName;
                latestTime = snapshot.val().latestTime;

                teamAGoals = teamAGoals;

                console.log(`Latest Score: ${latestTime}: ${latestTeam}, 
            ${latestPlayer}, ${latestScoreType},${teamAGoals}, ${teamAPoints},
            ${teamBGoals}, ${teamBPoints}`);
            } else {
                console.log("Latest Scores data = null");
            }
        });
</script>

<div class="score-card" title={`${competitionName} at ${startTime}`}>
    <div class="row no-gutters">
        <div class="col-auto">
            <h3>
                {competitionName}
            </h3>
            <h4>
                {teamAName} V {teamBName}
            </h4>
        </div>
    </div>
    <div class="row no-gutters">
        <div class="col-6 order-1 col-xl-3 order-xl-1">
            <img
                src={teamACrest || ""}
                id="crest-A-img card-img "
                class="img-fluid crest"
                alt="{teamAName} GAA Crest"
            />
        </div>
        <div class="col-6 order-2 col-xl-3 order-xl-2 score-container">
            <div class="">
                <p id="teamAScore" class="card-txt">
                    {teamAGoals} - {teamAPoints}
                </p>
            </div>
        </div>
        <div class="col-6 order-4 col-xl-3 order-xl-3 score-container">
            <div class="">
                <p class="card-txt">{teamBGoals} - {teamBPoints}</p>
            </div>
        </div>
        <div class="col-6 order-3 col-xl-3 order-xl-4">
            <img
                src={teamBCrest || ""}
                id="crest-B-img"
                class="img-fluid crest float-right"
                alt="{teamBName} GAA Crest"
            />
        </div>
    </div>
    <div class="row no-gutters">
        <div class="col">
            <p class="card-txt">
                {#if latestTime != undefined && latestTeam != undefined && latestPlayer != undefined && latestScoreType != undefined}
                    Latest Score:
                {/if}
                {#if latestTime != undefined}{latestTime}:{/if}
                {#if latestTeam != undefined}{latestTeam} -{/if}
                {#if latestPlayer != undefined}{latestPlayer}-{/if}
                {#if latestScoreType != undefined}{latestScoreType}{/if}
            </p>
        </div>
    </div>
</div>
<hr />

<style>
    .crest {
        padding: 10px;
        max-height: 185px;
        max-width: 185px;
    }
    .score-card {
        padding: 30px 30px 30px 50px;
    }

    .card-txt {
        font-size: x-large;
        text-align: center;
    }

    .score-container {
        align-self: center;
    }
    h3,
    h4 {
        padding: 10px 10px 20px 10px;
    }
</style>
