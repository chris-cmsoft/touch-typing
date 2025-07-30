<script setup lang="ts">
import { ref, computed } from 'vue'

const targetText = 'Giant pandas live in the temperate zone bamboo forests of central China.'
const userInput = ref('')

const highlightedText = computed(() => {
    return targetText
        .split('')
        .map((char, idx) => {
            const isCorrect = userInput.value[idx] === char
            // Use Tailwind classes for color
            return `<span class=\"${isCorrect ? 'text-green-600' : 'text-black'}\">${char}</span>`
        })
        .join('')
})
</script>

<template>
    <div class="max-w-2xl mx-auto mt-10 p-6 border border-gray-300 rounded-lg bg-white shadow">
        <h2 class="text-2xl font-bold mb-6">Typing Practice</h2>
        <div class="text-xl font-mono mb-6">
            <span v-html="highlightedText"></span>
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
body {
    background: #f7f7f7;
}
</style>
