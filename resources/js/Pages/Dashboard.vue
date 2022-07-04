<script setup>
import BreezeAuthenticatedLayout from '@/Layouts/Authenticated.vue';
import { Head } from '@inertiajs/inertia-vue3';
import DailyWorkSchedule from "@/Components/DailyWorkSchedule.vue";
</script>

<template>
    <Head title="Dashboard" />

    <BreezeAuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Dashboard
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-5xl mx-auto sm:px-6 lg:px-8">
                <div class="overflow-hidden">
                    <daily-work-schedule></daily-work-schedule>
                </div>
            </div>
        </div>
    </BreezeAuthenticatedLayout>
</template>
