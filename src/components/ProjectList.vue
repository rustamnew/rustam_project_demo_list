<template>
    <div class="project-list bg-theme-primary text-white relative">
        <h2 class="text-4xl font-bold text-center absolute top-2 left-0 right-0">Мои проекты</h2>

        <ContactsPanel />

        <div ref="containerRef" class="scroll-container">

            <!-- Первая секция с initial scale: 1 -->
            <section v-for="(project, index) in projects" :key="index" class="panel" ref="sectionRefs"
                :style="index === 0 ? { scale: 1 } : undefined">

                <ProjectCard :project="project" @open-modal="openCardModal" />
            </section>
        </div>

        <ProjectModal v-if="modalOpened" @close-modal="closeCardModal" :github="modalProject?.github"
            :deploy="modalProject?.deploy" />
    </div>

</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { gsap } from 'gsap'
import { Observer } from 'gsap/Observer'

import ContactsPanel from './ContactsPanel.vue'
import ProjectCard from './ProjectCard.vue'
import projectsData from '@/data/projects.json'
import ProjectModal from './ProjectModal.vue'


gsap.registerPlugin(Observer)

const projects = ref(projectsData)
const containerRef = ref(null)
const sectionRefs = ref([])

const modalOpened = ref(false)
const modalProject = ref(null)

let currentIndex = 0
let isAnimating = false
let scrollObserver = null

const goToSection = (targetIndex, animationDuration = 0.8) => {
    if (targetIndex < 0 || targetIndex >= projects.value.length) return

    isAnimating = true

    // const targetPercent = -index * 100
    const targetY = -targetIndex * 300

    gsap.to(containerRef.value, {
        // yPercent: targetPercent,
        y: `${targetY}px`,
        duration: animationDuration,
        ease: 'power2.out',
        onComplete: () => {
            currentIndex = targetIndex
            setTimeout(() => {
                isAnimating = false
            }, 200)
        }
    })

    sectionRefs.value.forEach((section, index) => {

        if (index === targetIndex) {
            gsap.to(sectionRefs.value[index], {
                scale: 1,
                x: 0,
                duration: animationDuration,
                ease: 'power2.out',
            })

            projects.value[index].active = true
        } else {
            const diff = targetIndex - index

            gsap.to(sectionRefs.value[index], {
                scale: 0.5,
                x: `${30 * diff}%`,
                duration: animationDuration,
                ease: 'power2.out',
            })

            projects.value[index].active = false
        }
    })
}

const openCardModal = (project) => {
    modalProject.value = project
    modalOpened.value = true
}

const closeCardModal = () => {
    modalOpened.value = false
}


onMounted(() => {
    scrollObserver = Observer.create({
        type: 'wheel,touch,pointer',
        wheelSpeed: 1,
        tolerance: 50,
        preventDefault: true,

        /* Для touch инвертируем логику */
        onUp: (self) => {
            if (isAnimating) return;
            const isTouch = self.event.type.includes('touch');
            goToSection(isTouch ? currentIndex + 1 : currentIndex - 1);
        },
        onDown: (self) => {
            if (isAnimating) return;
            const isTouch = self.event.type.includes('touch');
            goToSection(isTouch ? currentIndex - 1 : currentIndex + 1);
        }
    })

    if (sectionRefs.value.length > 1) {
        goToSection(1, 0)
    } else if (sectionRefs.value.length === 1) {
        goToSection(0, 0)
    }

})

onUnmounted(() => {
    if (scrollObserver) {
        scrollObserver.kill()
    }
})
</script>

<style scoped lang="scss">
.scroll-container {
    width: 100%;
    height: 100vh;
    will-change: transform;

    padding-top: calc(50vh - 150px);
    padding-bottom: calc(50vh - 150px);
    box-sizing: border-box;
}

.panel {
    width: 100%;
    /* height: 100vh; */
    height: 300px;
    display: flex;
    justify-content: center;
    align-items: center;
    // padding: 2rem;
    box-sizing: border-box;

    scale: 0.5;
}
</style>

<style>
html,
body {
    margin: 0;
    padding: 0;
    overflow: hidden;
    touch-action: none;
    width: 100%;
    height: 100%;
    position: fixed;
}
</style>
