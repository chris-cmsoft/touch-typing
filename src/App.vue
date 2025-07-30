<script setup lang="ts">
import { ref, computed } from 'vue'

const targetText = 'Giant pandas live in the temperate zone bamboo forests of central China.'
const userInput = ref('')

const highlightedText = computed(() => {
    return targetText
        .split('')
        .map((char, idx) => {
            const isCorrect = userInput.value[idx] === char
            return `<span style=\"color:${isCorrect ? 'green' : 'black'}\">${char}</span>`
        })
        .join('')
})
</script>

<template>
    <div
        style="
            max-width: 700px;
            margin: 40px auto;
            padding: 24px;
            border: 1px solid #ccc;
            border-radius: 8px;
            background: #fff;
        "
    >
        <h2>Typing Practice</h2>
        <div style="font-size: 1.5em; font-family: 'Fira Mono', monospace; margin-bottom: 24px">
            <span v-html="highlightedText"></span>
        </div>
        <input
            v-model="userInput"
            type="text"
            style="
                width: 100%;
                font-size: 1.2em;
                padding: 8px;
                border-radius: 4px;
                border: 1px solid #aaa;
            "
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
