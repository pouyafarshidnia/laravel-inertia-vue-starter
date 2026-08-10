<script setup lang="ts">
import { Head } from '@inertiajs/vue3';
import { dashboard } from '@/routes';

defineOptions({
    layout: {
        breadcrumbs: [
            {
                title: 'Dashboard',
                href: dashboard(),
            },
        ],
    },
});
</script>

<template>
    <Head title="Dashboard" />

    <div class="p-5">
        <h1 class="mb-6 text-lg font-bold text-gray-800 dark:text-gray-100">
            Application Overview
        </h1>
    </div>
</template>
