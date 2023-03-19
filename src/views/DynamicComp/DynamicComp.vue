<template>
  <component :is="comp"></component>
  <!-- <HomeView/> -->
</template>
<script lang="ts" setup>
import {
  toRefs,
  onMounted,
  markRaw,
  ref,
  defineAsyncComponent,
  nextTick,
} from "vue";
// subProjectTs/src/views/SubProjectTsHomeView.vue
// import HomeView from "subProjectTs/src/views/SubProjectTsHomeView.vue";

interface Prop {
  path: string;
}

const props = withDefaults(defineProps<Prop>(), {
  path: "",
});

const { path } = toRefs(props);

const comp = ref();

// comp.value = markRaw(HomeView);

onMounted(async (): Promise<void> => {
  try {
    // ========================================================
    const modules = import.meta.glob("/node_modules/subProjectTs/src/**/*.vue");
    console.log(modules, "[modules]");
    // const dyComp = ((await modules[`/src/views/${path.value}.vue`]()) as any).default
    const dyComp = defineAsyncComponent(
      modules[`/node_modules/subProjectTs/src/${path.value}.vue`] as any
    );
    ////////////////////////////////////以下方法均走不通////////////////////////////////
    // ==============================普通import======================================
    // const path1 = "subProjectTs/src/views/SubProjectTsHomeView.vue";
    // const path2 = "views/SubProjectTsHomeView";
    // const path3 = `/node_modules/subProjectTs/src/${path2}.vue`;
    // const dyComp = (
    //   await import(
    //     `/node_modules/subProjectTs/src/${path.value}.vue`
    //   )
    // ).default;
    /**
     * @description 文档描述，import不能导入二层变量名，但这里却可以
     * 对于别名，似乎确实只能导入一层（例如@）
     * 对于绝对路径，似乎可以导入多层（含本项目内及node_modules）
     * 相对路径...（未验证）
     * 另外，vite似乎可以支持编译时完全动态的路径
     */
    // const dyComp = defineAsyncComponent(
    //   () => import(`/node_modules/subProjectTs/src/${path.value}.vue`)
    // );
    // const dyComp = defineAsyncComponent(
    //   () => import(`${path.value}`)
    // );
    // =======================webpack===============================

    // const dyComp = (
    //   await import(`subProjectTs/src/views/SubProjectTsHomeView.vue`)
    // ).default;
    console.log(path.value, "[path.value]");
    // const dyComp = defineAsyncComponent(
    //   () =>
    //     import(`subProjectTs/src/${path.value}.vue`
    //     )
    // );
    console.log(dyComp, "[dyComp]");
    comp.value = markRaw(dyComp);
  } catch (error) {
    console.log(error, "[error]");
    console.log("[动态组件路径错误]");
  }
});
</script>
<style></style>
