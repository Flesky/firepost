<script lang="ts" setup>
import { RouterView, useRouter } from "vue-router";
import { getUserDocument, userStore } from "@/utils/userStore";
import { dismissError, errorStore } from "@/utils/errorStore";
import { computed, ref } from "vue";
import BottomBar from "@/components/BottomBar.vue";
import SplashScreen from "@/components/SplashScreen.vue";

const router = useRouter();

const routes = computed(() => {
  if (userStore.value) {
    return router
      .getRoutes()
      .filter(
        (route) =>
          route.meta.authRequired || route.meta.authRequired === undefined
      );
  } else {
    return router
      .getRoutes()
      .filter(
        (route) =>
          route.meta.authRequired === false ||
          route.meta.authRequired === undefined
      );
  }
});

const splash = ref(true);

// const routes = useRouter().getRoutes();
</script>

<template>

  <transition name="fade">
    <SplashScreen v-if="splash" @selfdestruct="splash = false"
    /></transition>

  <Suspense>
    <div class="flex flex-col h-full break-words">
      <div class="h-full overflow-y-auto">
        <div class="p-4 bg-red-800" v-if="errorStore" @click="dismissError">
          <p class="text-white">
            {{ errorStore }}
          </p>
          <p class="text-xs text-red-200 mt-2">Click to dismiss</p>
        </div>

        <div>
          <RouterView v-slot="{ Component }">
            <template v-if="Component">
              <Suspense>
                <!-- main content -->
                <component :is="Component"></component>
              </Suspense>
            </template>
          </RouterView></div>
      </div>

      <BottomBar v-if="userStore" /></div
  ></Suspense>
</template>

<style>
.fade-enter-active,
.fade-leave-active {
  @apply transition;
}

.fade-enter-from,
.fade-leave-to {
  @apply opacity-0;
}
</style>
