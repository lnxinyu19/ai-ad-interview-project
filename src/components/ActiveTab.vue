<script setup>
import { ref, computed, onMounted } from "vue";

const emit = defineEmits(["update:modelValue", "changeTab"]);
const props = defineProps({
  modelValue: {
    type: String,
    default: "",
  },
});

const activeTab = computed({
  get() {
    return "";
  },
  set(newValue) {
    emit("update:modelValue", newValue);
  },
});

const windowWitdh = ref();

const isMobileWidth = computed(() => windowWitdh.value < 450);
const handleResize = () => {
  windowWitdh.value = document.body.clientWidth;
};
onMounted(() => {
  window.addEventListener("resize", handleResize);
});

const tabs = ref(["民生消費", "食品餐飲", "娛樂休閒"]);
</script>

<template>
  <div class="sm:flex sm:justify-around w-full mb-4">
    <template v-if="isMobileWidth">
      <select
        v-model="activeTab"
        class="text-xl w-full text-center text-blueGreen"
        @change=""
      >
        <option value="" hidden>選擇客戶類型</option>
        <option
          v-for="tab in tabs"
          :value="tab"
          :label="tab"
          class="w-full text-sm text-center"
        ></option>
      </select>
    </template>
    <template v-else>
      <button
        v-for="tab in tabs"
        :key="`${tabs}__tabIndex`"
        class="inline-block w-1/3 py-2"
        :class="{
          'bg-blueGreen': activeTab === tab,
        }"
        :style="[activeTab === tab ? 'color: #ffffff' : 'color: #1fb6ff']"
        @click="emit('changeTab', tab)"
      >
        {{ tab }}
      </button>
    </template>
  </div>
</template>
