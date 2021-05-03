<template>
    <div class="todo-list-container">
        <h2 class="todo-title">Todo List</h2>
        <ul class="todo-list">
            <li
                class="todo-name"
                v-for="todo in todoList"
                :key="todo.id"
                :id="todo.id"
                @click="$emit('change-current-todo', todo)"
            >
                {{ todo.name }}
            </li>
        </ul>
        <form action="" data-new-list-form @submit.prevent="createNewTodo">
            <input
                type="text"
                class="new list"
                data-new-list-input
                placeholder="new list name"
                aria-label="new list name"
                required
                @input="removeInvalidity"
                v-model="newTodoName"
            />
            <button class="btn create" aria-label="create new list">+</button>
        </form>
    </div>
</template>

<script>
export default {
    name: "TodoList",
    props: {
        todoList: Array,
    },
    data() {
        return {
            newTodoName: "",
        };
    },
    methods: {
        createNewTodo(e) {
            if (this.todoList.some((todo) => todo.name === this.newTodoName)) {
                e.target.firstElementChild.setCustomValidity(
                    "Todo list already exists"
                );
                e.target.firstElementChild.reportValidity();
            } else {
                this.$emit("create-new-todo", this.newTodoName);
                this.newTodoName = "";
            }
        },
        removeInvalidity(e) {
            e.target.setCustomValidity("");
        },
    },
    emits: ["create-new-todo", "change-current-todo"],
};
</script>

<style scoped>
.todo-list-container {
    grid-area: lists;
}

.todo-list {
    font-size: 1.2rem;
    line-height: 1.7;
    list-style: circle;
    padding-left: 1.1em;
}

.todo-name {
    cursor: pointer;
}

.todo-name:hover {
    opacity: 0.7;
}

.active-list {
    font-weight: 700;
}

.new.list {
    font-size: 1.1rem;
}
</style>
