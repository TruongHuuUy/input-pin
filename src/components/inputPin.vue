<script setup lang="ts">
import { ref, watch } from "vue";

interface Props {
  valueInputs: string[];
  indexInput: number;
}
const props = withDefaults(defineProps<Props>(), {});
const emit = defineEmits(["update:indexInput", "update:valueInputs"]);

const inputsRef = ref<Record<number, HTMLInputElement>>({});

const setInputsRef = (el: unknown, index: number) => {
  inputsRef.value[index] = el as HTMLInputElement;
};

const handleClick = (index: number) => {
  props.valueInputs[index] = "";
};

const handleFocus = (index: number) => {
  emit("update:indexInput", index);
};

const handleBlur = (index: number) => {
  inputsRef.value[index].focus();
  inputsRef.value[index].blur();
};

const updateFocus = () => {
  let nextFocus = props.indexInput;
  const isInputEmpty = props.valueInputs[nextFocus] === "";

  if (!isInputEmpty) nextFocus = findNextIndex();

  if (nextFocus === -1) {
    nextFocus = props.valueInputs.length - 1;
    handleBlur(nextFocus);
  } else {
    inputsRef.value[nextFocus].focus();
  }
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
