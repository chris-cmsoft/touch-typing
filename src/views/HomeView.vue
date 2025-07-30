<script setup lang="ts">
import { ref } from 'vue'

const targetText = 'Giant pandas live in the temperate zone bamboo forests of central China.'
const userInput = ref('')

const targetChars = targetText.split('')
</script>

<template>
    <div class="max-w-2xl mx-auto mt-10 p-6 border border-gray-300 rounded-lg bg-white shadow">
        <h2 class="text-2xl font-medium mb-6">Typing Practice</h2>
        <div class="text-xl font-light mb-6 flex flex-wrap gap-0.5">
            <span
                v-for="(char, idx) in targetChars"
                :key="idx"
                class="whitespace-pre-wrap"
                :class="{
                    'text-green-600': userInput[idx] === char,
                    'text-red-600': userInput[idx] !== char && userInput[idx] !== undefined,
                    'text-black': userInput[idx] === undefined,
                }"
                >{{ char }}</span
            >
        </div>
        <input
            v-model="userInput"
            type="text"
            class="w-full text-lg p-2 rounded border border-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-400"
            :maxlength="targetText.length"
            placeholder="Start typing..."
        />
    </div>
</template>

<style scoped>
@reference '@/assets/main.css';

html,
body {
    font-family: var(--font-sans);
    @apply bg-gray-100;
}
</style>
