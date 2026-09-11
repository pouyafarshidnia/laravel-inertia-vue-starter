<script setup lang="ts">

import { X, Eraser } from "@lucide/vue";

defineProps({
    show: Boolean,
    label: String,
      title: String,
    erasable: { type: Boolean, default: false }
});



</script>

<template>
    <Transition enter-from-class="opacity-0 scale-125" enter-to-class="opacity-100 scale-100"
        enter-active-class="transition duration-300" leave-active-class="transition duration-200"
        leave-from-class="opacity-100 scale-100" leave-to-class="opacity-0 scale-125">

        <div v-if="show" @click.self="$emit('close')"  class="fixed inset-0 z-50 grid place-items-center bg-black/60 p-4">
            <div class="w-full max-w-lg max-h-[90vh] overflow-y-auto rounded-xl border border-zinc-200 bg-white dark:border-zinc-900 dark:bg-zinc-950">

                <!-- Header -->
                <div class="flex items-center justify-between "
                :class="title ? 'border-b border-zinc-200 dark:border-zinc-900 p-3' : 'pr-3 pt-3'"
                >
                    <h1 class="font-bold text-zinc-800 dark:text-zinc-300">{{ title }}</h1>
                    <div class="flex items-center gap-2">
                        <Eraser v-if="erasable" @click="$emit('erase')"
                            class="text-zinc-700 dark:text-zinc-500 w-4 h-4 cursor-pointer" />
                        <X @click="$emit('close')" class="text-zinc-700 dark:text-zinc-500 w-4 h-4 cursor-pointer" />
                    </div>
                </div>

                <!-- Body -->
                <div class="p-6">
                    <slot>default body</slot>
                </div>

                <!-- Footer -->
                <footer v-if="label" class="mt-2 bg-zinc-50 dark:bg-zinc-900 py-2 px-4 text-sm rounded-b-lg">
                    <span class="text-sm text-gray-500">{{ label }}</span>
                </footer>

            </div>
        </div>

    </Transition>
</template>
