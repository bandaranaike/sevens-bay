<template>
    <div class="rounded border py-2 px-3 mt-2 text-gray-500" :class="{'mt-6 text-gray-700':isMainItem, 'bg-green-100':isChecked}">
        <checkbox v-model="isChecked"></checkbox>
        <span class="ml-2">{{ task.name }}</span>
        <span class="ml-2">{{ task.hours }}</span>
        <span class="ml-2">{{ task.points }}</span>
    </div>
</template>

<script setup>
import Checkbox from "@/Components/Checkbox.vue";
import {ref, watch} from "vue";

let isChecked = ref(false);

const emit = defineEmits(['itemCheckToggled'])

watch(isChecked, (n, o) => {
    emit('itemCheckToggled', {isChecked, id: props.task.id})
});

const props = defineProps({
    isMainItem: {
        type: Boolean,
        default: true
    },
    task: {
        type: Object,
        default: () => ({id: null})
    }
})
</script>
