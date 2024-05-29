<script setup lang="ts">
import InputPin from "@/components/inputPin.vue";
import Keyboard from "@/components/keyboard.vue";
import { computed, reactive, ref } from "vue";

const inputFieldsRef = ref(null);
const inputs = reactive<string[]>(["", "", "", "", "", ""]);
const currentInput = ref(0);

const isValidatePin = computed(() => {
  return !inputs.includes("");
});

const findNextIndex = () => {
  let foundIndex = -1;
  for (let i = inputs.length - 1; i >= 0; i--) {
    if (inputs[i] !== "") {
      foundIndex = i;
      break;
    }
  }

  return foundIndex;
};

const handleNumberClick = (number: string) => {
  if (number !== "XOA" && number !== "Backspace")
    return (inputs[currentInput.value] = number);

  if (inputs[currentInput.value] === "") {
    const index = findNextIndex();
    currentInput.value = index === -1 ? 0 : index;
  }
  inputs[currentInput.value] = "";
};

const submit = () => {
  alert(inputs);
};
</script>

<template>
  <section class="w-full mx-auto">
    <div class="flex justify-center">
      <InputPin
        :current-input="currentInput"
        :value-inputs="inputs"
        ref="inputFieldsRef"
        @update:currentInput="currentInput = $event"
      />
    </div>
    <div class="max-w-max mx-auto my-10">
      <button
        class="py-2 px-10 font-bold text-xl rounded-lg"
        :class="[isValidatePin ? 'bg-sky-300 text-orange-500' : 'bg-slate-400']"
        :disabled="!isValidatePin"
        @click="submit()"
      >
        XÁC NHẬN
      </button>
    </div>
    <div>
      <Keyboard @number-click="handleNumberClick" />
    </div>
  </section>
</template>
