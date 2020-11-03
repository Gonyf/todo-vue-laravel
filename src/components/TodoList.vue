<template>
<div>
    <input class="todoInput" type='text' v-model="newTodoDescription" placeholder="What needs to be done?" @keyup.enter="addTodo">
    <div class="todoItem" v-for="(todo, index) in todos" :key="todo.id">
        <div class="textAndCheckbox">
            <input type="checkbox" v-model="todo.done">
            <div class="description" :class="{done : todo.done}">
                {{todo.description}}
            </div>
        </div>
        <div class="removeItem" @click="removeTodo(index)">&times;</div>
    </div>
</div>
</template>

<script>
export default {
    name: 'TodoList',
    data() {
        return {
            newTodoDescription: '',
            nextId: 2,
            todos: [{
                'id': 1,
                'description': "testing",
                'done': false
            }]
        }
    },
    methods: {
        addTodo() {
            if (this.newTodoDescription.trim().length === 0) {
                return;
            }
            this.todos.push({
                'id': this.nextId++,
                'description': this.newTodoDescription
            })
            this.newTodoDescription = ''
        },
        removeTodo(index) {
            this.todos.splice(index, 1)
        },
        toggleDone(todo) {
            todo.done = !todo.done
            alert('test')
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style lang="scss" scoped>
.todoInput {
    width: 100%;
    padding: 10px 25px;
    font-size: 28px;
    border-width: 5px;
    border-color: seagreen;
    margin-bottom: 30px;
    // border-radius: 500px;

    &:focus {
        outline: 0;
        border-color: rgb(48, 214, 120);
    }
}

.todoItem {
    width: 100%;
    padding: 15px 10px;
    font-size: 24px;
    // text-align: left;
    display: flex;
    justify-content: space-between;

    .textAndCheckbox {
        display: flex;
        align-items: center;
        justify-content: space-between;

        .description {
            margin-left: 15px;
        }
    }
}

.removeItem {
    cursor: pointer;
}

.done {
    text-decoration: line-through;
}
</style>
