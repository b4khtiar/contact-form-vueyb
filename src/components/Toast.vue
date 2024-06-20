<script setup>
import { ref, onMounted } from 'vue'

const classes = ref('opacity-0 -top-16')

onMounted(() => {
    classes.value = 'opacity-100 top-0 md:top-6 transition-all duration-300 ease-in-out'

    setTimeout(() => {
        classes.value = 'opacity-0 -top-16 transition-all duration-300 ease-in-out'
    }, 3000)
})
</script>

<template>
    <div class="fixed left-0 right-0 z-50" :class="classes">
        <div class="w-full max-w-lg p-6 mx-auto shadow text-green-50 bg-green-950 h-fit rounded-2xl">
            <slot />
        </div>
    </div>
</template>