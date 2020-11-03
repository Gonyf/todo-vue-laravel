<template>
<div id="app" class="mainContainer">
    <i class="fas fa-tasks"></i>
    <h1 class="Title">
        Your Todo List
    </h1>
    <TodoList />
</div>
</template>

<script>
import TodoList from './components/TodoList.vue'

export default {
    name: 'App',
    components: {
        TodoList
    }
}
</script>

<style>
* {
    box-sizing: border-box;
}

.mainContainer {
    max-width: 600px;
    margin: 0 auto;
}

#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
