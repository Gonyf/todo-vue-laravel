<template>
<div>
    <input class="todoInput" type='text' v-model="newTodoDescription" placeholder="What needs to be done?" @keyup.enter="addTodo">
    <div class="todoItem" v-for="(todo, index) in todos" :key="todo.id">
        <div class="textAndCheckbox">
            <input type="checkbox" v-model="todo.done">
            <div class="description" :class="{done : todo.done}">
                <input :ref="'todoInput-'+todo.id" v-if="todo.editing" type="text" v-model="todo.description" @keyup.enter="unsetEditing(todo)" v-on:blur="unsetEditing(todo)">
                <div v-else class="label" @dblclick="setEditing(todo)">
                    {{todo.description}}
                </div>
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
                'savedDescription': "",
                'done': false,
                'editing': false
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
        },
        setEditing(todo) {
            todo.savedDescription = todo.description
            todo.editing = true
            this.$nextTick(() => {
                let input = this.$refs["todoInput-" + todo.id];
                input.focus()
            })
        },
        unsetEditing(todo) {
            if (todo.description.length === 0) {
                todo.description = todo.savedDescription
            }
            todo.editing = false
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
    margin-bottom: 60px;
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
        display: flex;
        align-items: center;
        justify-content: space-between;

        .description {
            margin-left: 15px;

            input,
            .label {
                font-size: 24px;
                padding: 10px 10px;
                border: 1px solid rgba(255, 0, 0, 0);
                border-radius: 7px;
            }

            input {
                border: 1px solid seagreen;
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
</style>
