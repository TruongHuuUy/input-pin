<script setup lang="ts">
import InputPin from "@/components/inputPin.vue";
import Keyboard from "@/components/keyboard.vue";
import { computed, reactive, ref } from "vue";

const inputFieldsRef = ref(null);
const inputs = reactive<string[]>(["", "", "", "", "", ""]);
const indexInput = ref(0);

const isValidatePin = computed(() => {
  return !inputs.includes("");
});

const handleNumberClick = (number: string) => {
  if (number === "Backspace") {
    if (!isValidatePin.value && indexInput.value > 0) {
      indexInput.value--;
    }
    inputs[indexInput.value] = "";
  } else if (!isValidatePin.value) {
    inputs[indexInput.value] = number;
  }
};

const submit = () => {
  alert(inputs);
};
</script>

<template>
  <section class="max-w-max mx-auto">
    <InputPin
      :index-input="indexInput"
      :value-inputs="inputs"
      ref="inputFieldsRef"
      @update:indexInput="indexInput = $event"
    />
    <button
      class="py-2 px-10 font-bold text-xl rounded-lg w-full my-10"
      :class="[isValidatePin ? 'bg-sky-300 text-orange-500' : 'bg-slate-400']"
      :disabled="!isValidatePin"
      @click="submit()"
    >
      XÁC NHẬN
    </button>
    <Keyboard @number-click="handleNumberClick" />
  </section>
</template>
