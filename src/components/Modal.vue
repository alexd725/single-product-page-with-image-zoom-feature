<template>
  <div v-if="modal" @click="closeModalHere" class="w-[100%] left-[0%] fixed top-[0%] overflow-y-auto min-h-[100%] bg-gray-500/90 z-50"></div>
  <div v-if="modal" class="w-[70%] left-[15%] rounded-md absolute top-[5%] min-h-[90%] overflow-y-auto bg-slate-50	 z-50">
    <div class="absolute right-4 top-4">
      <svg @click="closeModalHere" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M6 18 18 6M6 6l12 12"/>
      </svg>
    </div>
    <div class="text-sm font-medium text-center text-gray-500 border-b border-gray-200 dark:text-gray-400 dark:border-gray-700">
      <ul class="flex flex-wrap -mb-px">
        <li class="me-2">
          <a href="#" @click="selectTab('images')" :class="{ 'text-red-600 border-red-600': activeTab === 'images', 'text-gray-600 border-gray-300': activeTab !== 'images' }" class="inline-block p-4 border-b-2 rounded-t-lg hover:text-gray-600 hover:border-gray-300 dark:hover:text-gray-300">Images</a>
        </li>
        <li class="me-2">
          <a href="#" @click="selectTab('videos')" :class="{ 'text-red-600 border-red-600': activeTab === 'videos', 'text-gray-600 border-gray-300': activeTab !== 'videos' }" class="inline-block p-4 border-b-2 rounded-t-lg hover:text-gray-600 hover:border-gray-300 dark:hover:text-gray-300">Videos</a>
        </li>
      </ul>
    </div>
    <div v-if="activeTab === 'images'">
      <slot name="images" />
    </div>
    <div v-else-if="activeTab === 'videos'">
      <slot name="videos" />
    </div>
  </div>
</template>

<script setup>
import { defineProps, ref } from "vue";

const props = defineProps({
  modal: Boolean,
  closeModal: Function,
});

const activeTab = ref('images');

const closeModalHere = () => {
  props.closeModal(true);
};

const selectTab = (tab) => {
  activeTab.value = tab;
};
</script>
