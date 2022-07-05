<template>
    <div class="rounded-md border bg-white">
        <div class="flex border-b">
            <div class="flex-grow p-6 flex items-center">
                <div class="mx-auto text-center">
                    <div class="text-5xl font-bold">04</div>
                    <div class="text-gray-500">June 2022</div>
                </div>
            </div>
            <div class="flex-grow border-l text-4xl p-6 flex items-center">
                <div class="mx-auto">
                    <span class="text-green-700">{{ collectedPoints }}</span> /
                    <span class="text-gray-800">{{ totalPoints }}</span>
                </div>
            </div>
        </div>
        <div class="px-6 pb-6">
            <ul>
                <li v-for="mainTask in workSchedule">
                    <task-item @itemCheckToggled="itemCheckToggled" :task="mainTask"></task-item>
                    <ul class="pl-5">
                        <template v-if="mainTask.childTasks && mainTask.childTasks.length">
                            <li v-for="childTask in mainTask.childTasks">
                                <task-item @itemCheckToggled="itemCheckToggled" :is-main-item="false" :task="childTask"></task-item>
                            </li>
                        </template>
                        <li class="text-right mt-2">
                            <button @click="showModal(mainTask)" class="w-7 h-7 text-2xl rounded-full text-white bg-blue-500 leading-4">+</button>
                        </li>
                    </ul>
                </li>
                <li>
                    <button @click="showModal()" class="w-8 h-8 text-2xl rounded-full text-white bg-orange-500 leading-4">+</button>
                </li>
            </ul>
        </div>
        <Modal v-model="isShowModal" :close="closeModal">
            <div class="w-96 bg-white p-6 rounded">
                <h3 class="mb-3 text-2xl">Creating new task</h3>
                <input class="border py-2 px-3 rounded mb-3 w-full" placeholder="Name" v-model="newTaskName">
                <input class="border py-2 px-3 rounded mb-3 w-full" placeholder="Points" v-model="newTaskPoints">
                <input class="border py-2 px-3 rounded mb-3 w-full" placeholder="Hours" v-model="newTaskHours">
                <div class="text-right mt-3">
                    <button class="rounded bg-gray-300 ml-3 py-1 px-3" @click="closeModal">Close</button>
                    <button class="rounded bg-blue-500 text-white ml-3 py-1 px-3" @click="addNewTask">Save</button>
                </div>
            </div>
        </Modal>
    </div>
</template>

<script setup>
import TaskItem from "@/Components/TaskItem.vue";
import {onMounted, ref} from "vue";
import {v4 as uuid} from "uuid"

const workSchedule = [
    {
        name: "Item 1",
        completed: false,
        points: 20,
        hours: 4,
        id: "f09ce786-6174-446c-897f-f6f42a46d19e",
        childTasks: [
            {
                name: "Item 2",
                completed: false,
                points: 10,
                hours: 2,
                id: "aa1a8b12-b049-48f9-a6e8-c9c5b0f29896"
            },
            {
                name: "Item 3",
                completed: false,
                points: 10,
                hours: 2,
                id: "8782e355-cb67-466d-ba8e-8dff53dfec0b"
            },
        ]
    },
    {
        name: "Item 4",
        completed: false,
        points: 20,
        hours: 4,
        id: "e91edac8-184e-43ca-91c8-390521079771",
        childTasks: [
            {
                name: "Item 5",
                completed: false,
                points: 10,
                hours: 2,
                id: "a191f073-5629-4ac2-badb-b91aec444f0d"
            },
            {
                name: "Item 6",
                completed: false,
                points: 10,
                hours: 2,
                id: "83d8edd8-17e1-4b23-91e0-83b849ae0aa2"
            },
            {
                name: "Item 7",
                completed: false,
                points: 10,
                hours: 2,
                id: "77fbe73e-ba45-43a2-9f94-c724e2be5da0"
            },
        ]
    }];
let updatingMainTask = ref({}), isShowModal = ref(false), newTask = ref({})

let totalPoints = ref(0), collectedPoints = ref(0)
let newTaskName = ref(null)
let newTaskPoints = ref(null)
let newTaskHours = ref(null)

function itemCheckToggled(data) {

    toggleItemCompleteStatus(workSchedule, data.id, data.isChecked.value)
    totalPoints.value = getTotal(workSchedule)
    collectedPoints.value = getTotal(workSchedule, true)
}

function getTotal(items, onlyCompleted) {
    return items.reduce((previousValue, currentItem) => {
        if (currentItem.childTasks && currentItem.childTasks.length) {
            previousValue += getTotal(currentItem.childTasks, onlyCompleted)
        }
        let points = onlyCompleted ? currentItem.completed ? currentItem.points : 0 : currentItem.points
        return previousValue + points;
    }, 0)
}

function toggleItemCompleteStatus(tasks, id, completed) {
    let task = tasks.find(task => task.id === id);
    if (task) task.completed = completed;
    else tasks.forEach(task => {
        if (task.childTasks && task.childTasks.length) toggleItemCompleteStatus(task.childTasks, id, completed)
    });
}

function addNewTask() {
    let task = {id: uuid(), name: newTaskName.value, points: newTaskPoints.value, hours: newTaskHours.value}
    console.log("updatingMainTask", updatingMainTask)
    if (updatingMainTask)
        updatingMainTask.childTasks.push(task)
    else
        workSchedule.push(task)

    updatingMainTask = null
    newTaskName.value = null;
    newTaskPoints.value = null;
    newTaskHours.value = null;

    closeModal();
}

function showModal(mainTask) {
    updatingMainTask = mainTask;
    isShowModal.value = true;
}

function closeModal() {
    isShowModal.value = false;
}

</script>

<style scoped>
.modal {
    width: 400px;
    padding: 30px;
    box-sizing: border-box;
    background-color: #fff;
    font-size: 20px;
    text-align: center;
}
</style>
