<template>
    <h3>Ajouter une nouvelle tâche</h3>

    <form @submit.prevent="addTask" class="addTask">
        <input type="text" id="newTask" name="newTask" v-model="newTask" placeholder="Nouvelle tâche">
        <button class="addButton" :disabled="newTask.length === 0">Ajouter</button>
    </form>

    <label id="maskLabel" for="mask"><input type="checkbox" name="mask" id="mask" v-model="mask">Masquer les tâches terminées</label>

    <div class="tasks">
        <p v-if="tasks.length == 0" class="noTask">Aucune tâche à compléter</p>

        <ul v-if="tasks.filter(task => !task.completed).length != 0">
            <h3>Tâches non complétés</h3>
            <li :key="`${task.date}`" v-for="task in tasks.filter(task => !task.completed)">
                <span>{{ task.title }}</span>
                <span>{{ new Date(task.date).toLocaleDateString() }} {{ new Date(task.date).toLocaleTimeString() }}</span>
                <button @click="completedTask(task.title, task.date)">✅</button>
            </li>
        </ul>

        <hr v-if="tasks.length != 0">

        <ul v-if="tasks.filter(task => task.completed).length != 0 && !mask">
            <h3>Tâches complétées</h3>
            <li :key="`${task.date}`" v-for="task in tasks.filter(task => task.completed)">
                <span class="line-through">{{ task.title }}</span>
                <span>{{ new Date(task.date).toLocaleDateString() }}  {{ new Date(task.date).toLocaleTimeString() }}</span>
            </li>
        </ul>

    </div>

</template>

<script lang="ts" setup>

import {ref} from 'vue';

const newTask = ref('');
const tasks = ref([]);
const mask = ref(false);

const addTask = () => {

    if(newTask.value !== '') {

        // Ajouter la nouvelle tâche au tableau
        tasks.value.push(
            {
                title: newTask.value,
                completed: false,
                date: Date.now()
            }
        )

        // Réinitialiser le champ
        newTask.value = '';

    }

}


const completedTask = (title:string, date:number) => {
    tasks.value.map((task) => {
        if(task.title === title && task.date === date) {
            task.completed = true;
        }
    })
}

</script>

<style>

body {
    text-align: center;
}

li {
    text-align: left;
}

li span {
    margin: 10px;
}

.addTask {
    display: flex;
    flex-direction: column;
    align-content: center;

    gap: 10px;

    max-width: 30%;

    margin: auto;
}

#newTask {
    border-radius: 10px;

    padding: 10px;
}

.tasks {
    margin: 100px;
}

button {
    border-radius: 7px;

    padding: 5px;
    margin: auto;

    max-width: 100px;

    cursor: pointer;
}

.line-through {
    text-decoration: line-through;
}

#maskLabel {
    margin: 50px;
    display: block;
}

</style>