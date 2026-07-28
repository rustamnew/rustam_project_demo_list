<script setup>
// import { ref } from 'vue'

const props = defineProps({
    project: {
        type: Object,
        required: true
    },
    active: false
})

const emit = defineEmits(['open-modal'])


const openCardModal = () => {
    emit('open-modal', props.project)
}
</script>

<template>
    <div
        class="project-card w-2xl bg-white p-8 rounded-2xl shadow-lg border border-gray-100 hover:shadow-xl transition-shadow duration-300">
        <h3 class="text-2xl font-semibold text-gray-900 mb-2">{{ project.title }}</h3>

        <!-- loading="lazy" ?? -->
        <img class="h-24 object-cover mb-2" :src="'.' + project.image + '.webp'" :alt="project.title">

        <p class="text-sm font-medium text-blue-600 mb-4">{{ project.stack }}</p>
        <p class="text-gray-600 leading-relaxed">{{ project.desc }}</p>

        <div class="flex justify-end mt-2">
            <button v-if="project.active"
                class="bg-blue-500 hover:bg-blue-600 text-white font-medium py-2 px-4 rounded cursor-pointer"
                @click="openCardModal">
                Перейти
            </button>
        </div>
    </div>
</template>
