<template>
  <!-- <h1>{{ route.meta }}</h1> -->
  <div v-show="isLogin && !$route.meta.isParentView" class="flex h-full w-full">
    <!-- 侧边栏菜单 -->
    <sidebar v-if="isShowMenu" id="sidebar" class="w-200" />
    <div class="flex-1">
      <div id="top">
        <!-- 顶部导航栏 -->
        <navbar class="h-50" />
        <!-- Tabs标签页 -->
        <div :style="{ width: appMainWidth + 'px' }">
          <tabs-view />
        </div>
      </div>
      <!-- 主页面 -->
      <div :style="{ height: appMainHeight + 'px', width: appMainWidth + 'px' }">
        <app-main class="app-main p-10" />
      </div>
    </div>
  </div>
  <div v-if="!isLogin || (isLogin && $route.meta.isParentView)" class="h-full">
    <router-view />
  </div>
</template>

<script setup>
import sidebar from './layout/components/sidebar.vue';
import navbar from './layout/components/navbar.vue';
import appMain from './layout/components/app-main.vue';
import tabsView from './layout/components/tabs-view.vue';
const { proxy } = getCurrentInstance();
let { isLogin } = toRefs(proxy.$store.user.useUserStore());
let { isShowMenu } = toRefs(proxy.$store.settings.useSettingsStore());
let appMainWidth = ref(0);
let appMainHeight = ref(0);

onMounted(() => {
  // 窗口宽高变化时触发 -- tips：window.onresize只能在项目内触发1次
  window.onresize = function windowResize() {
    calWidthAndHeight();
  };
});

// 注册一个回调函数，在组件因为响应式状态变更而更新其 DOM 树之后调用。
onUpdated(() => {
  calWidthAndHeight();
});

watch(
  [isLogin, isShowMenu],
  (newValue) => {
    calWidthAndHeight();
  },
  { immediate: false, deep: true },
);

function calWidthAndHeight() {
  let sidebarW = document.getElementById('sidebar').offsetWidth;
  appMainWidth.value = window.innerWidth - sidebarW;

  let topH = document.getElementById('top').offsetHeight;
  appMainHeight.value = window.innerHeight - topH - 20; // 20 指 p-10
}
</script>
<style lang="scss" scoped>
.app-main {
  // height: calc(100vh - 50px); // 满屏 - navbar
}
</style>
