<script setup>
import { ref, computed } from "vue";

const props = defineProps({
  modelValue: {
    type: Number,
    default: 1,
  },
  totalPage: {
    type: Number,
    default: 2,
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
  <div class="mx-auto space-x-4">
    <button
      v-for="page in totalPage"
      :key="`${page}__pageBtnIndex`"
      type="button"
      :class="{ 'bg-blueGreen': currentPage === page }"
      class="inline-block bg-white h-[12px] w-[12px] rounded-full"
      @click="emit('changePage', page)"
    ></button>
  </div>
</template>
