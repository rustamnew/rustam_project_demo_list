<script setup>

defineProps({
    github: "",
    deploy: ""
})

const emit = defineEmits(['close-modal'])

const closeCardModal = () => {
    emit('close-modal')
}
</script>

<template>
    <div class="project-modal fixed inset-0 bg-black/50 flex justify-center items-center z-50" @click="closeCardModal">
        <div class="w-100 max-w-2xl bg-white p-8 rounded-2xl shadow-lg border border-gray-100 relative" @click.stop>

            <button class="text-gray-500 hover:text-gray-700 text-xl cursor-pointer absolute top-2 right-6"
                @click="closeCardModal">
                ×
            </button>

            <div class="flex justify-center gap-6">

                <a v-if="github" :href="github" target="_blank" rel="noopener noreferrer"
                    class="hover:opacity-80 transition-opacity flex flex-col items-center" aria-label="GitHub">

                    <p class="text-gray-900 font-medium mb-4">GitHub</p>

                    <img :src="'.' + '/icons/github.svg'" alt="GitHub" class="w-10 h-10" />
                </a>

                <a v-if="deploy" :href="deploy" target="_blank" rel="noopener noreferrer"
                    class="hover:opacity-80 transition-opacity flex flex-col items-center" aria-label="Web-page">

                    <p class="text-gray-900 font-medium mb-4">Web-page</p>

                    <!-- Кастомные размеры из-за особенностей иконки w-[42px] h-[42px] -->
                    <img :src="'.' + '/icons/website.svg'" alt="Web-page" class="w-[42px] h-[42px]" />
                </a>
            </div>
        </div>
    </div>
</template>