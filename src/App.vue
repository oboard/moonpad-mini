<script setup lang="ts">
import { Moonpad as MoonpadWc } from '@moonbit/moonpad'
import { onMounted, ref } from 'vue';

const moonPad = ref<MoonpadWc | null>(null);
const output = ref('');

const exampleCode = `fn main {
  println("Hello MoonBit!")
  println(1 + 1)
}
`

onMounted(() => {
  if (!customElements.get('moonpad-wc')) {
    customElements.define('moonpad-wc', MoonpadWc);
  }
  const element = document.createElement('moonpad-wc') as MoonpadWc;
  element.setAttribute("data-content", exampleCode);
  element.setAttribute("data-theme", "dark");
  element.connectedCallback();

  document.getElementById('code')?.appendChild(element);

  moonPad.value = element;
});

const run = async () => {
  if (moonPad.value) {
    const result = await moonPad.value.run();
    output.value = result ?? "";
  }
}

const format = async () => {
  if (moonPad.value) {
    await moonPad.value.format();
  }
}
</script>

<template>
  <div class="p-4 bg-neutral-100 dark:bg-neutral-900">
    <div class="flex flex-row justify-end gap-2">
      <button @click="format"
        class="border-none bg-black hover:bg-neutral-500 active:bg-neutral-100 active:text-black text-white font-bold py-2 px-4 rounded-full transition duration-300 ease-in-out transform hover:-translate-y-1">
        Format
      </button>
      <button @click="run"
        class="border-none bg-black hover:bg-neutral-500 active:bg-neutral-100 active:text-black text-white font-bold py-2 px-4 rounded-full transition duration-300 ease-in-out transform hover:-translate-y-1">
        Run
      </button>
    </div>
    <div class="flex flex flex-col md:flex-row md:gap-2">
      <div id="code" class="mt-4 p-4 bg-[#282c34] rounded-lg md:flex-1">
        <!-- 代码内容将在这里显示 -->
      </div>
      <div class="mt-4 md:mt-0 md:flex-1">
        <h2 class="text-xl font-bold mb-2">Output:</h2>
        <pre class="text-sm bg-neutral-200 dark:bg-neutral-800 p-4 rounded whitespace-pre-wrap">{{ output }}</pre>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
