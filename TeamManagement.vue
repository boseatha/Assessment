<script lang="ts">
import { defineComponent } from "vue";

interface Team {
    id: number;
    name: string;
    ownerName: string;
}

export default defineComponent({
    data() {
        return {
            teams: [] as Team[],
            newTeamName: "",
        };
    },
    methods: {
        createTeam() {
            // Simulate team creation
            const newTeam: Team = {
                id: this.teams.length + 1,
                name: this.newTeamName,
                ownerName: "Current User",
            };

            // Add the new team to the list
            this.teams.push(newTeam);

            // Reset the form
            this.newTeamName = "";
        },
    },
});
</script>
<template>
    <div>
        <h2>Team Management</h2>

        <!-- Display Teams List -->
        <div v-if="teams.length > 0">
            <h3>Your Teams</h3>
            <ul>
                // eslint-disable-next-line prettier/prettier
                <li>
                    v-for="team in teams" :key="team.id"
                    {{ teams.name }} (Owner: {{ teams.ownerName }})
                </li>
            </ul>
        </div>

        <!-- Create New Team Form -->
        <div>
            <h3>Create a New Team</h3>
            <form @submit.prevent="createTeam">
                <label for="teamName">Team Name:</label>
                <input id="teamName" v-model="newTeamName" type="text" required />

                <button type="submit">Create Team</button>
            </form>
        </div>
    </div>
</template>
