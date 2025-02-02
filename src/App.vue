<script setup lang="ts">
import { markRaw } from 'vue'
import { useToggle } from '@vueuse/core'

import X from './components/canvas/X.vue'
import XY from './components/canvas/XY.vue'
import XYZ from './components/canvas/XYZ.vue'
import Footer from './components/Footer.vue'

const paramList: IndexExpressionItem[] = [
  {
    text: 't, i, x',
    canvas: markRaw(X),
    defaultExp: 'sin(t)',
  }, {
    text: 't, i, x, y',
    canvas: markRaw(XY),
    defaultExp: 'sin(t)',
  }, {
    text: 't, i, x, y, z',
    canvas: markRaw(XYZ),
    defaultExp: 'sin(t)',
  }
]

let expression = $ref('sin(t)')
let currentParam = $ref(paramList[0])
const [dropDownOpen, toggleDropdown] = useToggle()

const setExpression = (exp: IndexExpressionItem) => {
  expression = exp.defaultExp
  currentParam = exp
  toggleDropdown()
}
</script>

<template>
  <div class="mx-auto my-24 max-w-[400px] px-6">
    <component :is="currentParam.canvas" :exp="expression"></component>
    <div class="mt-12 text-left px-3">
      <div class="relative -mx-2 text-sm select-none">
        <span
          @click="toggleDropdown()"
          class="pl-2 py-1.5 rounded-md cursor-pointer op-50 hover:bg-gray-100 hover:op-80 dark:hover:bg-truegray-700"
        >
          ({{currentParam.text}})
        </span>
        <span class="ml-1 op-50">=></span>
        <div
          v-show="dropDownOpen"
          class="absolute top-full mt-2 py-2 rounded-md shadow-md bg-white border border-gray-100 dark:bg-truegray-800 dark:border-truegray-600"
        >
          <div
            v-for="param in paramList"
            @click="setExpression(param)"
            class="px-3 py-2 cursor-pointer hover:bg-gray-100 dark:hover:bg-truegray-600"
          >
            {{ param.text }}
          </div>
        </div>
      </div>
      <input
        class="text-xl font-600 py-2 mt-1 w-full bg-transparent outline-0 border-b border-gray-400/50"
        v-model="expression"
        autocomplete="false"
        spellcheck="false"
      />
    </div>
    <Footer px-3 mt-8 />
  </div>
</template>

<style>
html {
  --c-bg: #ffffff;
  --c-scroll: #d9d9d9;
  --c-scroll-hover: #bbbbbb;
  background-color: #fafafa;
}

html.dark {
  --c-bg: #212121;
  --c-scroll: #333333;
  --c-scroll-hover: #555555;
  background-color: #1e1e1e;
}

body {
  font-family: 'Fira Code', monospace;
}

::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}
::-webkit-scrollbar-thumb {
  background-color: var(--c-scroll);
  border-radius: 4px;
}
::-webkit-scrollbar-thumb:hover {
  background-color: var(--c-scroll-hover);
}
::-webkit-scrollbar-track {
  background-color: var(--c-bg);
}
</style> 
