<template>
  <!-- <HomeView0 /> -->
  <DynamicComp v-if="!!path" :path="path" />
  <button @click="onClick">show comp</button>
</template>
<script lang="ts" setup>
import { ref, type Ref } from "vue";
// import HomeView0 from "@/views/HomeView/HomeView.vue";
// import HomeView from "subprojectVue/src/views/SubProjectHomeView.vue";
import DynamicComp from "@/views/DynamicComp/DynamicComp.vue";

// import HomeView from "subProjectTs/src/views/SubProjectTsHomeView.vue";
const path: Ref<string> = ref("");

// const path: Ref<string> = ref("views/SubProjectTsHomeView");
  // const path: Ref<string> = ref("Hello/SubProjectTsHomeView");
    // path.value = localStorage.getItem("dynamicCompPath") || "";
    // "/node_modules/subProjectTs/src/views/SubProjectTsHomeView.vue"
function onClick() {
  // path.value = "views/SubProjectTsHomeView";
  path.value = localStorage.getItem("dynamicCompPath") || "";
}
</script>
