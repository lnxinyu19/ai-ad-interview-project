<script setup>
import StepButton from "./StepButton.vue";
import Pagination from "./Pagination.vue";
import ActiveTab from "./ActiveTab.vue";
import { onMounted, onUnmounted, ref } from "vue";

const imgs = ref([]);
const totalPage = ref(2);
const currentPage = ref(1);
const currentTab = ref("民生消費");
const loading = ref(true);
const changePage = async (page) => {
  if (page > totalPage.value) {
    currentPage.value = 1;
  } else {
    currentPage.value = page;
  }
  await getImgs();
};

const changeTab = async (tab) => {
  currentTab.value = tab;
  await getImgs();
};

const getImgs = async () => {
  try {
    loading.value = true;
    const limit = 10;
    await fetch(
      `https://picsum.photos/v2/list?page=${currentPage.value}&limit=${limit}`
    )
      .then((response) => {
        return response.json();
      })
      .then((json) => {
        imgs.value = json;
      });
  } catch (error) {
    console.log(error);
  } finally {
    loading.value = false;
  }
};

// let interval = null;
onMounted(async () => {
  const nextPage = currentPage.value + 1;
  // 輪播
  // interval = setInterval(() => {
  //   changePage(nextPage);
  // }, 5000); //
  await getImgs();
});

// onUnmounted(() => {
//   clearInterval(interval);
// });
</script>

<template>
  <ActiveTab
    v-model="currentTab"
    @changeTab="(newValue) => changeTab(newValue)"
  />
  <StepButton v-model="currentPage" @changePage="changePage" />
  <div class="mt-2 h-[400px]">
    <template v-if="loading">
      <div class="h-[342px] flex justify-center items-center">
        <div class="loading"></div>
      </div>
    </template>
    <template v-else>
      <div class="flex flex-col flex-wrap h-[400px]">
        <img
          v-for="img in imgs"
          :key="img.id"
          :src="img.download_url"
          class="w-[155px] h-[155px] m-4"
          alt="empty"
        />
      </div>
    </template>
  </div>
  <Pagination
    v-model="currentPage"
    :totalPage="totalPage"
    @changePage="changePage"
  />
</template>
