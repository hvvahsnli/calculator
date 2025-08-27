<script setup>
import { ref } from 'vue'
import { Delete } from 'lucide-vue-next'
const display = ref('0')
let isResult = false

const buttons = [
  'C', '%', '±', '√',
   '7', '8', '9', '/',
  '4', '5', '6', '*',
  '1', '2', '3', '-',
  '.', '0', '=', '+',
]

function press(btn) {
  if (btn === 'C') {
    display.value = '0'
    isResult = false
  } else if (btn === 'DEL') {
    display.value = display.value.slice(0, -1) || '0'
  } else if (btn === '=') {
  try {
    let result = eval(display.value)
    display.value = Math.round(result * 1e12) / 1e12
    isResult = true
  } catch {
    display.value = "Error"
    isResult = true
  }
  } else if (btn === '%') {
    display.value = (parseFloat(display.value) / 100).toString()
    isResult = true
  } else if (btn === '±') {
    display.value = (parseFloat(display.value) * -1).toString()
    isResult = true
  } else if (btn === '√') {
    display.value = Math.sqrt(parseFloat(display.value)).toString()
    isResult = true
  } else {
    if (display.value === '0' || isResult) {
      display.value = btn
      isResult = false
    } else {
      display.value += btn
    }
  }
}
</script>


<template>

  <div class="bg-black p-6 rounded-2xl shadow-xl w-80">
    <!-- Display -->
    <div class="mb-4 bg-pink-100 p-4 rounded-lg text-2xl font-mono flex justify-between items-center h-20 overflow-x-auto">
      <span>{{ display }}</span>
      <button
        @click="press('DEL')"
        class="ml-2 p-1 rounded hover:bg-pink-300"
      >
      <button @click="press('DEL')">
        <Delete class="w-5 h-5 text-pink-600 " />
      </button>

      </button>
    </div>

    <!-- Buttons Grid -->
    <div class="grid grid-cols-4 gap-3">
      <button
        v-for="btn in buttons"
        :key="btn"
        @click="press(btn)"
        :class="[
          'p-4 rounded-full text-lg font-bold transition',
          'bg-gray-800 ring ring-purple-500  text-white'
        ]"
      >
        {{ btn }}
      </button>
    </div>
  </div>
</template>