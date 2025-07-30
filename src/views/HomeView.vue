<script setup lang="ts">
import { ref, watch, useTemplateRef, onMounted } from 'vue'
import { targetTexts } from '@/data/targetTexts'

type Status = 'correct' | 'incorrect' | 'fixed'

interface CharStatus {
    char: string
    state: Status
}

const index = ref(Math.floor(Math.random() * targetTexts.length))
watch(index, reset)

const targetText = ref(targetTexts[index.value])
const userInput = ref('')
const typedCharacters = ref<CharStatus[]>([])
const targetChars = ref(targetText.value.split(''))
const typingInput = useTemplateRef('typing-input')

onMounted(() => {
    focusInput()
})

function focusInput() {
    typingInput.value?.focus()
}

function nextIndex(): number {
    return (index.value + 1) % targetTexts.length
}

function onInputChange() {
    for (let i = 0; i < userInput.value.length; i++) {
        let state: Status
        if (targetChars.value[i] === userInput.value[i]) {
            state = 'correct'
        } else {
            state = 'incorrect'
        }
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

    // Automatically go to next text if last character is entered correctly
    if (
        userInput.value.length === targetChars.value.length &&
        typedCharacters.value[userInput.value.length - 1]?.state !== 'incorrect'
    ) {
        index.value = nextIndex()
        reset()
    }
}

function reset() {
    targetText.value = targetTexts[index.value]
    targetChars.value = targetText.value.split('')
    userInput.value = ''
    typedCharacters.value = []
}
</script>

<template>
    <div class="max-w-2xl mx-auto mt-10 p-6 border border-gray-300 rounded-lg bg-white shadow">
        <h2 class="text-2xl font-medium mb-6">Typing Practice</h2>
        <div class="text-xl font-mono font-light">
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
            @blur="focusInput"
            ref="typing-input"
            type="text"
            class="opacity-0 h-0 w-0 absolute top-0 left-0"
            :maxlength="targetText.length"
            placeholder="Start typing..."
            autofocus
            autocomplete="off"
        />
    </div>
</template>
