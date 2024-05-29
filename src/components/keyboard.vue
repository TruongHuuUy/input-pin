<script setup lang="ts">
import { onBeforeUnmount } from "vue";

const numbers = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "", "0", "XOA"];

const emit = defineEmits(["numberClick"]);

window.addEventListener("keydown", (event) => {
  handleKeyBoard(event);
});

const handleKeyBoard = (event: KeyboardEvent) => {
  event.preventDefault();
  const isInputNumber = /^[0-9]$/.test(event.key);
  const isBackspaceKey = event.key === "Backspace";
  if (!isBackspaceKey && !isInputNumber) return;
  emit("numberClick", event.key);
};

const handleClickKey = (number: string) => {
  emit("numberClick", number);
};

onBeforeUnmount(() => {
  window.removeEventListener("keydown", (event) => {
    handleKeyBoard(event);
  });
});
</script>

<template>
  <section>
    <div class="grid grid-cols-3">
      <button
        v-for="number in numbers"
        :key="number"
        class="w-16 h-16 text-blue-500 mb-5 rounded-full mx-auto text-xl font-bold"
        :class="[number !== '' ? 'bg-white' : 'cursor-default']"
        @click="number !== '' && handleClickKey(number)"
      >
        {{ number }}
      </button>
    </div>
  </section>
</template>
