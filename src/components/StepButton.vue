<script setup>
import { computed } from "vue";
const totalPage = 2;
const props = defineProps({
  modelValue: {
    type: Number,
    default: 1,
  },
});
const emit = defineEmits(["update:modelValue", "changePage"]);

const currentPage = computed({
  get() {
    return props.modelValue;
  },
  set(newValue) {
    emit("update:modelValue", newValue);
  },
});
</script>
<template>
  <div class="ml-auto space-x-4">
    <button
      type="button"
      class="text-blueGreen h-[50px] w-[50px] disabled:cursor-not-allowed"
      @click="emit('changePage', currentPage - 1)"
    >
      <
    </button>
    <button
      type="button"
      class="text-blueGreen h-[50px] w-[50px]"
      @click="emit('changePage', currentPage + 1)"
    >
      >
    </button>
  </div>
</template>
