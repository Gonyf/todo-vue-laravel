<template>
<div>
    <input class="todoInput" type='text' v-model="newTodoDescription" placeholder="What needs to be done?" @keyup.enter="addTodo">
    <div class="todoItem" v-for="(todo, index) in filteredTodos" :key="todo.id">
        <div class="textAndCheckbox">
            <input type="checkbox" v-model="todo.done">
            <div class="description" :class="{done : todo.done}">
                <input :ref="'todoInput-'+todo.id" v-if="todo.editing" type="text" v-model="todo.description" @keyup.enter="unsetEditing(todo)" v-on:blur="unsetEditing(todo)" @keyup.esc="cancelEdit(todo)">
                <div v-else class="label" @dblclick="setEditing(todo)">
                    {{todo.description}}
                </div>
            </div>
        </div>
        <div class="removeItem" @click="removeTodo(index)">&times;</div>
    </div>
    <hr />
    <div class="todoExtras">
        <div class="groupCheckbox">
            <input :checked="!anyLeft" type="checkbox" @change="toggleAllTodo">
            <div class="textAfterInput">{{groupCheckBoxText}}</div>
        </div>
        <div class="filters">
            <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
            <button :class="{active: filter == 'todo'}" @click="filter = 'todo'">To do</button>
            <button :class="{active: filter == 'done'}" @click="filter = 'done'">Done</button>
        </div>
        <button class="clearCompleted" @click="clearCompleted">Clear completed</button>
    </div>
    <div class="itemsLeft">{{itemsLeft}} tasks left to do</div>
</div>
</template>

<script>
export default {
    name: 'TodoList',
    data() {
        return {
            newTodoDescription: '',
            nextId: 2,
            savedDescription: "",
            todos: [{
                'id': 1,
                'description': "testing",
                'done': false,
                'editing': false
            }],
            filter: 'all'
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
        },
        setEditing(todo) {
            this.savedDescription = todo.description
            todo.editing = true
            this.$nextTick(() => {
                let input = this.$refs["todoInput-" + todo.id];
                input.focus()
            })
        },
        unsetEditing(todo) {
            if (todo.description.trim().length === 0) {
                todo.description = this.savedDescription
            }
            todo.editing = false
        },
        cancelEdit(todo) {
            todo.description = this.savedDescription
            todo.editing = false
        },
        toggleAllTodo() {
            if (this.itemsLeft === 0) {
                this.todos.forEach((todo) => todo.done = false)
            } else {
                this.todos.forEach((todo) => todo.done = true)
            }
        },
        clearCompleted() {
            this.todos = this.todos.filter((todo) => !todo.done)
        }
    },
    computed: {
        itemsLeft() {
            return this.todos.filter((todo) => !todo.done).length
        },
        anyLeft() {
            return this.itemsLeft != 0
        },
        groupCheckBoxText() {
            if (!this.anyLeft) {
                return 'Uncheck all'
            }
            return 'Check all'
        },
        filteredTodos() {
            if (this.filter == 'done') {
                return this.todos.filter((todo) => todo.done)
            } else if (this.filter == 'todo') {
                return this.todos.filter((todo) => !todo.done)
            }
            return this.todos
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
    border-radius: 7px;
    margin-bottom: 45px;
    // border-radius: 500px;

    &:focus {
        outline: 0;
        border-color: rgb(48, 214, 120);
    }
}

.todoItem {
    width: 100%;
    font-size: 24px;
    // text-align: left;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 12px;

    .textAndCheckbox {
        width: 100%;
        display: flex;
        align-items: center;

        .description {
            margin-left: 15px;
            margin-right: 15px;
            width: 100%;

            input,
            .label {
                text-align: left;
                width: 100%;
                font-size: 24px;
                padding: 10px 10px;
                border: 2px solid rgba(255, 0, 0, 0);
                border-radius: 7px;
            }

            input {
                border: 2px solid seagreen;
            }

            input:focus {
                outline: none;
            }
        }

    }
}

.removeItem {
    cursor: pointer;
}

.done {
    text-decoration: line-through;
}

.todoExtras,
.groupCheckbox {
    display: flex;
    justify-content: space-between;
    align-items: center;

    .clearCompleted {}
}

button {
    font-size: 14px;
    background: white;
    appearance: none;
    border: none;
    margin: 0px 5px;

    // padding: 2px;
    &:focus {
        outline: none;
    }

    &:hover {
        outline: 2px solid seagreen;
    }
}

.filters {
    button {
        border: 2px solid seagreen;
        margin: 0px 5px;

        &:hover,
        &.active {
            background-color: seagreen;
        }
    }
}

.textAfterInput {
    margin-left: 10px;
}

hr {
    display: block;
    height: 1px;
    border: 0;
    border-top: 3px solid seagreen;
    margin: 1em 0;
    padding: 0;
}

.itemsLeft {
    margin: 20px 0px;
}
</style>
