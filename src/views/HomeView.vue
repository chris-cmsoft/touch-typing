<script setup lang="ts">
import { ref } from 'vue'
import { targetTexts } from '@/data/targetTexts'

type Status = 'correct' | 'incorrect' | 'fixed'

interface CharStatus {
    char: string
    state: Status
}

const currentIndex = ref(Math.floor(Math.random() * targetTexts.length))
const targetText = ref(targetTexts[currentIndex.value])
const userInput = ref('')
const typedCharacters = ref<CharStatus[]>([])
const targetChars = ref(targetText.value.split(''))

function onInputChange() {
    for (let i = 0; i < userInput.value.length; i++) {
        let state: Status
        if (targetChars.value[i] === userInput.value[i]) {
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

function onEnter() {
    // If Enter is pressed and all text is entered, go to next text
    if (userInput.value.length === targetText.value.length) {
        // Move to next index, wrap around if needed
        currentIndex.value = (currentIndex.value + 1) % targetTexts.length
        targetText.value = targetTexts[currentIndex.value]
        targetChars.value = targetText.value.split('')
        userInput.value = ''
        typedCharacters.value = []
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
            @input="onInputChange"
            @keyup.enter="onEnter"
            type="text"
            class="w-full text-lg p-2 rounded border border-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-400"
            :maxlength="targetText.length"
            placeholder="Start typing..."
            autofocus
            autocomplete="off"
        />
    </div>
</template>
