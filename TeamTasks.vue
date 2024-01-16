<script lang="ts">
import { defineComponent } from "vue";

interface Task {
    id: number;
    name: string;
    status: "pending" | "done";
}

interface Team {
    id: number;
    name: string;
    ownerName: string;
}

export default defineComponent({
    data() {
        return {
            teamTasks: [] as Task[],
            currentTeam: null as Team | null,
            newTaskName: "",
        };
    },
    computed: {
        isTeamOwner(): boolean {
            // Simulate authentication.
            // Assuming a user role or permission is stored in the application state.
            return this.currentTeam
                ? this.currentTeam.ownerName === "Current User"
                : false;
        },
    },
    methods: {
        createTask() {
            if (this.isTeamOwner && this.currentTeam) {
                // Simulate task creation
                const newTask: Task = {
                    id: this.teamTasks.length + 1,
                    name: this.newTaskName,
                    status: "pending",
                };

                // Add the new task to the list
                this.teamTasks.push(newTask);

                // Reset the form
                this.newTaskName = "";
            }
        },
        markAsDone(task: Task) {
            if (this.isTeamOwner && this.currentTeam) {
                // Simulate marking the task as done
                task.status = "done";
            }
        },
        deleteTask(task: Task) {
            if (this.isTeamOwner && this.currentTeam) {
                // Simulate deleting the task
                const index = this.teamTasks.findIndex((t) => t.id === task.id);
                if (index !== -1) {
                    this.teamTasks.splice(index, 1);
                }
            }
        },
    },
});
</script>
<template>
    <div>
        <h2>Team Tasks</h2>

        <!-- Display Team Tasks -->
        <div v-if="teamTasks.length > 0">
            <h3>Tasks for {{ currentTeam.name }}</h3>
            <ul>
                <li v-for="task in teamTasks" :key="task.id">
                    {{ task.name }} - Status: {{ task.status }}
                    <button v-if="isTeamOwner" @click="markAsDone(task)">
                        Mark as Done
                    </button>
                    <button v-if="isTeamOwner" @click="deleteTask(task)">
                        Delete
                    </button>
                </li>
            </ul>
        </div>

        <!-- Create New Task Form -->
        <div v-if="isTeamOwner">
            <h3>Create a New Task</h3>
            <form @submit.prevent="createTask">
                <label for="taskName">Task Name:</label>
                <input id="taskName" v-model="newTaskName" type="text" required />

                <button type="submit">Create Task</button>
            </form>
        </div>
    </div>
</template>

<style scoped></style>
