<script setup>
import { ref } from 'vue'

import contactsData from '@/data/contacts.json'

const contacts = ref(contactsData)
</script>

<template>
    <div class="contacts-panel absolute top-4 left-4 z-10">
        <h3 class="mb-2 text-xl">Контакты</h3>

        <p v-for="(contact, index) in contacts" :key="index" class="inline-block w-full">
            <span class="capitalize">{{ contact.type }}</span>:

            <a v-if="contact.type === 'email'" :href="`mailto:${contact.value}`"
                class="text-blue-400 hover:text-blue-300 underline">
                {{ contact.value }}
            </a>
            <a v-else-if="contact.type === 'telegram'" :href="`https://t.me/${contact.value.replace('@', '')}`"
                target="_blank" rel="noopener noreferrer" class="text-blue-400 hover:text-blue-300 underline">
                {{ contact.value }}
            </a>
            <span v-else>{{ contact.value }}</span>
        </p>
    </div>
</template>