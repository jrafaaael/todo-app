<template>
    <div class="task-container">
        <label :for="task.id" class="task">
            <input
                type="checkbox"
                :id="task.id"
                @change="$emit('toggle-completed', task)"
                :checked="task.complete"
            />
            <svg width="34px" height="24px" viewBox="0 0 34 24">
                <path
                    d="M23,14 L23,4 L23,4 C23,2.34314575 21.6568542,1 20,1 L4,1 L4,1 C2.34314575,1 1,2.34314575 1,4 L1,20 L1,20 C1,21.6568542 2.34314575,23 4,23 L20,23 L20,23 C21.6568542,23 23,21.6568542 23,20 L23,14 L23,14 C23,12.8954305 23.8954305,12 25,12 L34,12"
                ></path>
                <polyline points="6.5 13.5 10 16.5 17.5 7.5"></polyline>
            </svg>
            <span>
                {{ task.task }}
            </span>
        </label>
    </div>
</template>

<script>
export default {
    name: "Tasks",
    props: {
        task: Object,
    },
};
</script>

<style scoped>
.task-container {
    display: flex;
    position: relative;
}

.task-container::after {
    content: "";
    position: absolute;
    right: 0;
    left: 0;
    bottom: -1rem;
    height: 1px;
    background: currentColor;
    opacity: 0.1;
}

.task {
    position: relative;
    display: flex;
    cursor: pointer;
}
.task svg {
    fill: none;
    vertical-align: middle;
    float: left;
}
.task svg path {
    stroke: #bac1d0;
    stroke-width: 2;
    stroke-dasharray: 84;
    stroke-dashoffset: 168;
}
.task svg polyline {
    stroke: var(--clr-primary);
    stroke-width: 3;
    stroke-linecap: round;
    stroke-linejoin: round;
    stroke-dashoffset: 17;
    stroke-dasharray: 17;
}
.task:hover svg path,
.task:hover svg polyline {
    stroke: var(--clr-primary);
}
.task span {
    vertical-align: middle;
    -webkit-user-select: none;
    user-select: none;
    pointer-events: none;
    line-height: 24px;
}
.task span:before {
    content: "";
    position: absolute;
    top: 10px;
    left: 10px;
    width: 4px;
    height: 4px;
    border-radius: 50%;
    box-shadow: 0 -24px 0 var(--clr-primary), 24px 0px 0 var(--clr-primary),
        0 24px 0 var(--clr-primary), -24px 0px 0 var(--clr-primary),
        18px -18px 0 var(--clr-primary), 18px 18px 0 var(--clr-primary),
        -18px 18px 0 var(--clr-primary), -18px -18px 0 var(--clr-primary);
    transform: scale(0);
    opacity: 1;
}
.task span:after {
    content: "";
    position: absolute;
    left: 34px;
    right: calc(100% - 34px);
    top: 50%;
    margin-top: -1px;
    height: 2px;
}
.task input {
    display: none;
}
.task input:checked + svg path {
    stroke-dasharray: 87, 95;
    stroke-dashoffset: 87;
    transition: all 0.6s ease-in;
}
.task input:checked + svg polyline {
    stroke-dashoffset: 17;
    stroke-dasharray: 34;
    transition: all 0.2s linear;
    transition-delay: 0.2s;
}
.task input:checked + svg + span {
    color: #bac1d0;
    transition: all 0.2s ease;
    transition-delay: 0.3s;
}
.task input:checked + svg + span:before {
    transform: scale(1);
    opacity: 0;
    transition: all 0.4s ease;
}
.task input:checked + svg + span:after {
    right: -2px;
    background: var(--clr-primary);
    transition: right 0.4s ease;
    transition-delay: 0.25s;
}
</style>
