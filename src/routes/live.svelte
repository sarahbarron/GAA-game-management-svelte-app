<script lang="ts">
    import { auth, db, rtdb } from "../services/firebase";
    import router from "page";
    import Score from "../components/Score.svelte";
    import Filter from "../components/Filter.svelte";

    interface ThisGame {
        childKey: string;
        competitionName: string;
        teamACrest: string;
        teamAName: string;
        teamBCrest: string;
        teamBName: string;
        startTime: string;
        sportType: string;
    }

    // let user: User | null;
    let games: ThisGame[] = [];
    let countyTeams: string[] = [];
    let clubTeams: string[] = [];

    const getGames = rtdb.ref(`games/`).once("value", function (snapshot) {
        var data = snapshot.val();
        if (data != null) {
            console.log(data);
            snapshot.forEach((childSnapshot) => {
                var childKey = childSnapshot.key;
                var childData = childSnapshot.val();
                var competitionName =
                    childSnapshot.val().competition.competitionName;
                var teamAName = childSnapshot.val().teamA.teamName;
                var teamACrest = childSnapshot.val().teamA.crest;
                var teamBName = childSnapshot.val().teamB.teamName;
                var teamBCrest = childSnapshot.val().teamB.crest;
                var startTime = childSnapshot.val().times.startTime;
                var clubIdA = childSnapshot.val().teamA.clubId;
                var clubIdB = childSnapshot.val().teamB.clubId;
                var countyIdA = childSnapshot.val().teamA.countyId;
                var countyIdB = childSnapshot.val().teamB.countyId;
                var sportType = childSnapshot.val().sportType;
                if (clubIdA != undefined) {
                    if (!clubTeams.includes(teamAName)) {
                        clubTeams = [...clubTeams, teamAName];
                    }
                } else if (countyIdA != undefined) {
                    if (!countyTeams.includes(countyIdA)) {
                        countyTeams = [...countyTeams, countyIdA];
                    }
                }

                if (clubIdB != undefined) {
                    if (!clubTeams.includes(teamBName)) {
                        clubTeams = [...clubTeams, teamBName];
                    }
                } else if (countyIdB != undefined) {
                    if (!countyTeams.includes(countyIdB)) {
                        countyTeams = [...countyTeams, countyIdB];
                    }
                }
                var game = {
                    childKey: childKey,
                    competitionName: competitionName,
                    teamACrest: teamACrest,
                    teamAName: teamAName,
                    teamBCrest: teamBCrest,
                    teamBName: teamBName,
                    startTime: startTime,
                    sportType: sportType,
                };
                games = [...games, game];

                console.log(`Child: ${childKey}: ${childData}`);
            });
        } else {
            console.log("Games data = null");
        }
    });
</script>

<div class="container-fluid">
    <h1>GAA Live Scores</h1>
    <p>
        Todays Live scores from all the club and county GAA Hurling and Football
        games
    </p>

    <br />
    <div class="row">
        <!-- Hide on screens smaller than medium  -->
        <div class="col-2 d-none d-md-block">
            {#if games.length > 0}
                <Filter {clubTeams} {countyTeams} />
            {/if}
        </div>
        <!-- Current Total Scores -->
        <div class="col-12 col-xl-5">
            <div>
                {#if games.length > 0}
                    {#each games as s (s.childKey)}
                        <Score {...s} />
                    {/each}
                {:else}
                    <p class="w3-center w3-text-gray">
                        Loading todays games....
                    </p>
                {/if}
                <br id="scroll-to" />

                <br />
            </div>
            <br />
        </div>
        <div class="col-5 d-none d-md-block">
            <!-- Show all scores -->
            <h1>show scores</h1>
        </div>
    </div>
</div>
