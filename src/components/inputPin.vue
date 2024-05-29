<script setup lang="ts">
import { ref, watch } from "vue";

interface Props {
  valueInputs: string[];
  currentInput: number;
}
const props = withDefaults(defineProps<Props>(), {});
const emit = defineEmits(["update:currentInput", "update:valueInputs"]);

const inputsRef = ref<Record<number, HTMLInputElement>>({});

const setInputsRef = (el: unknown, index: number) => {
  inputsRef.value[index] = el as HTMLInputElement;
};

const handleClick = (index: number) => {
  props.valueInputs[index] = "";
};

const handleFocus = (index: number) => {
  emit("update:currentInput", index);
};

const handleBlur = (index: number) => {
  inputsRef.value[index].blur();
};

const updateFocus = () => {
  let nextFocus = props.currentInput;
  const valueInputs = props.valueInputs;
  const nextIndex = findNextIndex();

  if (valueInputs[nextFocus] === "") {
    nextFocus = props.currentInput;
  } else if (nextIndex !== -1) {
    nextFocus = nextIndex;
  }
  inputsRef.value[nextFocus].focus();

  if (nextIndex === -1 || valueInputs[nextFocus] !== "") handleBlur(nextFocus);
};

const findNextIndex = () => {
  const index = props.valueInputs.findIndex((input) => input === "");
  return index;
};

watch(
  () => props.valueInputs,
  () => {
    updateFocus();
  },
  {
    deep: true,
  }
);
</script>

<template>
  <section>
    <input
      v-for="(value, index) in valueInputs"
      :ref="(el) => setInputsRef(el, index)"
      :key="index"
      class="w-12 h-12 text-amber-700 font-bold text-4xl text-center rounded-md border-2 border-amber-700 mr-3 last:mr-0"
      :class="{ 'bg-amber-200': value !== '' }"
      type="password"
      autofocus
      maxlength="1"
      v-model="valueInputs[index]"
      inputmode="none"
      @click="handleClick(index)"
      @focus="handleFocus(index)"
    />
  </section>
</template>
