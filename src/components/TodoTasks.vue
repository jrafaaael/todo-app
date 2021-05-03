<template>
    <div class="todo-tasks">
        <div class="todo-header">
            <h2 class="todo-list-name">
                {{ todo.name }}
            </h2>
            <p class="tasks-count">
                {{ taskRemaining }}
            </p>
        </div>
        <div class="todo-body">
            <div class="tasks">
                <tasks
                    v-for="(task, i) in todo.tasks"
                    :key="i"
                    :task="task"
                    @toggle-completed="toggleCompleted"
                ></tasks>
            </div>
            <div class="new-task-creator">
                <form
                    action=""
                    data-new-task-form
                    @submit.prevent="createNewTask"
                >
                    <input
                        type="text"
                        data-new-task-input
                        class="new task"
                        placeholder="new task name"
                        aria-label="new task name"
                        required
                        @input="removeInvalidity"
                        v-model="newTask"
                    />
                    <button class="btn create" aria-label="create new task">
                        +
                    </button>
                </form>
            </div>
            <div class="delete-stuff">
                <button
                    class="btn delete"
                    data-clear-complete-tasks-button
                    @click="removeCompletedTasks"
                >
                    Clear completed tasks
                </button>
                <button
                    class="btn delete"
                    data-delete-list-button
                    @click="$emit('delete-todo-list', todo.id)"
                >
                    Delete list
                </button>
            </div>
        </div>
    </div>
</template>

<script>
import Tasks from "./Tasks";

export default {
    name: "TodoTasks",
    props: {
        todo: Object,
    },
    components: {
        Tasks,
    },
    data() {
        return {
            newTask: "",
        };
    },
    methods: {
        createNewTask(e) {
            if (this.todo.tasks.some((task) => task.task === this.newTask)) {
                e.target.firstElementChild.setCustomValidity(
                    "Task already exists"
                );
                e.target.firstElementChild.reportValidity();
            } else {
                this.$emit("create-new-task", this.newTask);
                this.newTask = "";
            }
        },
        removeInvalidity(e) {
            e.target.setCustomValidity("");
        },
        toggleCompleted(task) {
            task.complete = !task.complete;
            this.$emit("update");
        },
        removeCompletedTasks() {
            this.todo.tasks = this.todo.tasks.filter((task) => !task.complete);
            this.$emit("update");
        },
    },
    emits: ["create-new-task", "delete-todo-list", "update"],
    computed: {
        taskRemaining() {
            const pendingTask = this.todo.tasks.filter((task) => !task.complete)
                .length;
            return `${pendingTask} ${
                pendingTask > 1 ? "tasks" : "task"
            } remaining`;
        },
    },
};
</script>

<style scoped>
.todo-tasks {
    --spacer: 2rem;

    height: fit-content;

    background: var(--clr-light);
    color: var(--clr-dark);
    grid-area: active;
}

.todo-header {
    padding: var(--spacer);
    background: #e4e4e4;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.todo-list-name {
    margin: 0 1em 0 0;
}

.tasks-count {
    margin: 0;
    font-size: 1rem;
}

.todo-body {
    padding: var(--spacer);
    position: relative;
}

.tasks {
    height: 250px;
    overflow: auto;

    display: flex;
    flex-direction: column;
    gap: 2rem;
}

.tasks::-webkit-scrollbar {
    width: 5px;
}

.tasks::-webkit-scrollbar-track {
    border-radius: 1px;
}

.tasks::-webkit-scrollbar-thumb {
    border-radius: 5px;
    background-color: #777;
}

.tasks::-webkit-scrollbar-thumb:hover {
    background-color: #666;
}

.tasks::-webkit-scrollbar-thumb:active {
    background-color: #444;
}

.new.task {
    margin-bottom: 0;
}

.new-task-creator .create {
    color: var(--clr-primary);
}

.delete-stuff {
    display: flex;
    justify-content: space-evenly;
    position: absolute;
    width: 100%;
    left: 0;
    bottom: -35px;
    color: var(--clr-light);
}

.btn.delete {
    opacity: 0.7;
    font-size: 1rem;
    transition: color 200ms;
}

.btn.delete:hover {
    color: var(--clr-warning);
}
</style>
