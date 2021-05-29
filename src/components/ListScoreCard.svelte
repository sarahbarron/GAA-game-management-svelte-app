<script lang="ts">
    import { rtdb } from "../services/firebase";
    import ListScoreSingleCard from "./ListScoreSingleCard.svelte";
    import { onMount } from "svelte";
    export let childKey: string;
    export let teamACrest: string;
    export let teamBCrest: string;
    export let teamAName: string;
    export let teamBName: string;

    interface Score {
        id: string;
        time: string;
        team: string;
        player: string;
        scoreType: string;
        crest: string;
    }

    let allScores: Score[] = [];

    onMount(async () => {
        // Create a listener for new scores
        const scoresListener = rtdb
            .ref(`games/${childKey}/scores`)
            .on('child_added', function (snapshot) {
                var data = snapshot.val();
                if (data != null) {
                    snapshot.forEach((childSnapshot) => {
                        console.log(data);
                        let id = childSnapshot.val();
                        let time = childSnapshot.val().latestTime;
                        let team = childSnapshot.val().latestTeamName;
                        let player = childSnapshot.val().latestPlayer;
                        let scoreType = childSnapshot.val().latestScoreType;
                        let crest = "";
                        if (team === teamAName) {
                            crest = teamACrest;
                        } else if (team == teamBName) {
                            crest = teamBCrest;
                        }
                        let score = {
                            id,
                            time,
                            team,
                            player,
                            scoreType,
                            crest,
                        };
                        allScores = [...allScores, score];
                    });
                } else {
                    console.log("Latest Scores data = null");
                }
            });
    });
</script>

<div class="card">
    <div class="row">
        {#if allScores.length > 0}
            {#each allScores as s (s.id)}
                <ListScoreSingleCard {...s} />
            {/each}
        {:else}
            <p class="w3-center w3-text-gray">Loading todays games....</p>
        {/if}
    </div>
</div>
