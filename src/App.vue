<template>
    <h1 class="title">Stuff I need to do</h1>
    <todo-list
        :todoList="todoList"
        @create-new-todo="createNewTodo"
        @change-current-todo="changeCurrentTodo"
    ></todo-list>
    <todo-tasks
        v-if="currentTodoList"
        :todo="currentTodoList"
        @create-new-task="createNewTask"
        @delete-todo-list="deleteTodoList"
        @update="setLocalStorage({ key: 'todoList', value: this.todoList })"
    ></todo-tasks>
</template>

<script>
import TodoList from "@/components/TodoList";
import TodoTasks from "@/components/TodoTasks";

export default {
    name: "App",
    components: {
        TodoList,
        TodoTasks,
    },
    data() {
        return {
            todoList: this.getLocalStorage({ key: "todoList" }) ?? [],
            currentTodoList: null,
        };
    },
    methods: {
        getLocalStorage({ key }) {
            return localStorage.hasOwnProperty(key)
                ? JSON.parse(localStorage.getItem(key))
                : undefined;
        },
        setLocalStorage({ key, value }) {
            localStorage.setItem(key, JSON.stringify(value));
        },
        createNewTodo(todoName) {
            this.todoList.push({
                name: todoName,
                id: new Date().getTime(),
                tasks: [],
            });
            this.setLocalStorage({
                key: "todoList",
                value: this.todoList,
            });
        },
        changeCurrentTodo(todo) {
            this.currentTodoList = todo;
        },
        createNewTask(task) {
            this.currentTodoList.tasks.push({
                task: task,
                complete: false,
                id: new Date().getTime(),
            });
            this.setLocalStorage({
                key: "todoList",
                value: this.todoList,
            });
        },
        deleteTodoList(id) {
            this.todoList = this.todoList.filter((list) => list.id !== id);
            this.setLocalStorage({
                key: "todoList",
                value: this.todoList,
            });
            this.currentTodoList = null;
        },
    },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Work+Sans:wght@100;200;300;400;500;600;700;800;900&display=swap");

:root {
    --clr-primary: rgb(54, 112, 199);
    --clr-light: #f4f4f4;
    --clr-dark: #333;
    --clr-warning: rgb(99, 36, 36);
}

*,
*::after,
*::before {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html,
body {
    width: 100%;
    min-height: 100vh;
    background-color: var(--clr-primary);
}

main {
    color: var(--clr-light);
    font-family: "Work Sans", sans-serif;
    font-weight: 300;
    font-size: 1.5rem;
    display: grid;
    grid:
        "header header header header" auto
        "...... lists  active ......" auto /
        1fr minmax(100px, 300px) minmax(250px, 500px) 1fr;
}

.title {
    margin: -0.3em 0 0.3em;
    grid-area: header;
    color: rgba(0, 0, 0, 0.1);
    font-size: calc(7vw + 2rem);
    font-weight: 900;
    text-align: center;
    letter-spacing: 2px;
}

form {
    display: flex;
}

.btn {
    cursor: pointer;
    background: 0;
    border: 0;
    padding: 0;
    color: inherit;
}

.btn.create {
    font-size: 1.5rem;
    font-weight: 900;
    margin-right: 0.25em;
    transition: opacity 250ms ease-in;
}

.btn.create:hover {
    opacity: 0.7;
}

.new {
    background: transparent;
    border: 0;
    color: inherit;
    border-bottom: 1px solid currentColor;
    font-size: inherit;
    outline: none;
    padding: 0.25em;

    transition: border-bottom 150ms ease-in;
    order: 2;
}

.new::placeholder {
    opacity: 0.4;
}

.new:focus {
    border-bottom-width: 3px;
}

.new:focus::placeholder {
    opacity: 0.15;
}
</style>
