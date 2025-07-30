<script setup lang="ts">
import { ref } from 'vue'
import { targetTexts } from '@/data/targetTexts'

type Status = 'correct' | 'incorrect' | 'fixed'

interface CharStatus {
    char: string
    state: Status
}

const targetText = targetTexts[Math.floor(Math.random() * targetTexts.length)]
const userInput = ref('')
const typedCharacters = ref<CharStatus[]>([])
const targetChars = targetText.split('')

function onInputChange() {
    for (let i = 0; i < userInput.value.length; i++) {
        let state: Status
        if (targetChars[i] === userInput.value[i]) {
            state = 'correct'
        } else {
            state = 'incorrect'
        }

        // Ensure typedCharacters has an entry for the current index
        if (!typedCharacters.value[i]) {
            typedCharacters.value[i] = { char: userInput.value[i], state }
        } else {
            if (typedCharacters.value[i].state == 'incorrect' && state === 'correct') {
                state = 'fixed'
            } else if (typedCharacters.value[i].state == 'fixed' && state === 'correct') {
                state = 'fixed'
            }
            typedCharacters.value[i] = { char: userInput.value[i], state }
        }
    }
}
</script>

<template>
    <div class="max-w-2xl mx-auto mt-10 p-6 border border-gray-300 rounded-lg bg-white shadow">
        <h2 class="text-2xl font-medium mb-6">Typing Practice</h2>
        <div class="text-xl font-mono font-light mb-6">
            <span
                v-for="(char, idx) in targetChars"
                :key="idx"
                :class="{
                    ...(userInput.length > idx
                        ? {
                              'bg-green-200': typedCharacters[idx]?.state === 'correct',
                              'bg-amber-200': typedCharacters[idx]?.state === 'fixed',
                              'bg-red-200': typedCharacters[idx]?.state === 'incorrect',
                          }
                        : {}),
                    'border-b-2 border-b-blue-500': userInput.length == idx,
                }"
                >{{ char }}</span
            >
        </div>
        <input
            v-model="userInput"
            @keyup="onInputChange"
            type="text"
            class="w-full text-lg p-2 rounded border border-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-400"
            :maxlength="targetText.length"
            placeholder="Start typing..."
        />
    </div>
</template>
