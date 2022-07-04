<template>
    <div class="rounded-md border bg-white">
        <div class="flex border-b">
            <div class="flex-grow p-6 flex items-center">
                <div class="mx-auto">
                    <div class="text-2xl">June</div>
                    <div class="text-3xl">04</div>
                    <div class="month">2022</div>
                </div>
            </div>
            <div class="flex-grow border-l text-2xl p-6 flex items-center">
                <div class="mx-auto">
                    <span class="text-green-700">320</span> /
                    <span class="text-gray-800">400</span>
                </div>
            </div>
        </div>
        <div class="px-6 pb-6">
            <ul>
                <li v-for="mainTask in workSchedule">
                    <task-item @itemCheckToggled="itemCheckToggled" :task="mainTask"></task-item>
                    <template v-if="mainTask.childTasks.length">
                        <ul class="pl-5">
                            <li v-for="childTask in mainTask.childTasks">
                                <task-item :is-main-item="false" :task="childTask"></task-item>
                            </li>
                        </ul>
                    </template>
                </li>
            </ul>
        </div>
    </div>
</template>

<script setup>
import TaskItem from "@/Components/TaskItem.vue";

function itemCheckToggled(item) {
    console.log(item.value)
}



const workSchedule = [{
    name: "Item 1",
    points: 20,
    hours: 4,
    id: "",
    childTasks: [{
        name: "Item 2",
        points: 10,
        hours: 2,
        id: ""
    }]
}, {
    name: "Item 1",
    points: 20,
    hours: 4,
    id: "",
    childTasks: [{
        name: "Item 2",
        points: 10,
        hours: 2,
        id: ""
    }]
}, {
    name: "Item 1",
    points: 20,
    hours: 4,
    id: "",
    childTasks: [{
        name: "Item 2",
        points: 10,
        hours: 2,
        id: ""
    }]
}, {
    name: "Item 1",
    points: 20,
    hours: 4,
    id: "",
    childTasks: [{
        name: "Item 2",
        points: 10,
        hours: 2,
        id: ""
    }]
}, {
    name: "Item 1",
    points: 20,
    hours: 4,
    id: "",
    childTasks: [{
        name: "Item 2",
        points: 10,
        hours: 2,
        id: ""
    }]
}];
</script>

