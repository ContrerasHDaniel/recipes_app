<script setup>
import { ref, reactive, computed } from "vue";
import { v4 as uuid } from 'uuid'

const recipe = reactive({
    name: "",
    ingredients: "",
    steps: ""
})

const recipes = ref([])

const isFormValid = computed(() => {
    const { name, ingredients, steps } = recipe

    return name && ingredients && steps
})

const addRecipe = () => {
    if (!isFormValid.value) {
        return;
    }
    recipes.value.push({
        id: uuid(),
        ...recipe
    })
}

const deleteRecipe = (index) => {
    recipes.value.splice(index, 1)
}
</script>

<template>
    <div class="wrapper">
        <fieldset>
            <legend align="right">New recipe</legend>
            <div class="form-input">
                <label>Name</label>
                <input type="text" v-model="recipe.name">
            </div>
            <div class="form-input">
                <label>Ingredients</label>
                <textarea v-model="recipe.ingredients"></textarea>
            </div>
            <div class="form-input">
                <label>Steps</label>
                <textarea v-model="recipe.steps"></textarea>
            </div>
            <button class="btn btn-submit" type="button" @click="addRecipe">Add recipe</button>
        </fieldset>
        <hr class="solid">
        <div class="recipes-container">
            <div v-for="(r, index) of recipes" :key="r.id" class="recipe-item">
                <h1>{{ r.name }}</h1>
                <h2>Ingredients</h2>
                <div class="content">{{ r.ingredients }}</div>
                <h2>Steps:</h2>
                <div class="content">{{ r.steps }}</div>
                <button class="btn btn-delete" type="button" @click="deleteRecipe(index)">Delete</button>
            </div>
        </div>
    </div>
</template>

<style scoped>
.recipes-container {
    width: 100%;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    padding: 10px;
}

.recipe-item {
    padding: 10px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.recipes-row::after {
    content: "";
    clear: both;
    display: table;
}

hr {
    width: 100%;
    margin: 2em 0;
}

.wrapper {
    display: flex;
    width: 100%;
    align-items: center;
    flex-direction: column;
}

.wrapper fieldset {
    width: 40%;
    height: fit-content;
}

.wrapper fieldset legend {
    font-weight: bold;
    padding: 0 10px;
}

.form-input {
    display: flex;
    flex-direction: column;
}

.form-input input {
    line-height: 1.5em;
    font-size: 1rem;
}

.form-input textarea {
    line-height: 1.5em;
    font-size: 1rem;
    height: 6em;
}

.form-input label {
    font-size: 1.2em;

}

.content {
    white-space: pre-wrap;
}

.btn-submit {
    margin-top: 0.5em;
    padding: 5px;
    background-color: var(--vt-c-green-accept);
    font-weight: bold;
}

.btn-delete {
    padding: 5px;
    background-color: var(--vt-c-red-1);
    font-weight: bold;
}

.btn {
    cursor: pointer;
    border: none;
}

.btn:hover {
    box-shadow: 2px 2px 7px rgba(156, 156, 156, 0.5);
}
</style>