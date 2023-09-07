<template>
  <n-config-provider :theme-overrides="themeOverrides" :locale="zhCN" :date-locale="dateZhCN">
    <n-message-provider>
      <router-view v-slot="{ Component, route }">
        <transition name="fade" mode="out-in">
          <component :is="Component" :key="route.path" />
        </transition>
      </router-view>
    </n-message-provider>
  </n-config-provider>

  <div class="background" :style="{ backgroundImage: `url('/backgrounds/${background}.jpeg')` }" />
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { RouterView } from 'vue-router'

import { themeOverrides } from '@/assets/overrides'
import { zhCN, dateZhCN } from 'naive-ui'

import pinia from '@/stores/pinia'
import { useDBStore } from '@/stores/db'
const DB = useDBStore(pinia)

onMounted(async () => {
  if (localStorage.getItem('userName') !== null) await DB.userLogin()
})

const backgroundCount = 6
const clamp = (value: number, min: number, max: number) => Math.max(min, Math.min(max, value))
const lsBackground = parseInt(localStorage.getItem('background') ?? '0')
const background = ref(clamp(lsBackground, 0, backgroundCount - 1))

defineExpose({
  nextBackground: () => {
    background.value = (background.value + 1) % backgroundCount
    localStorage.setItem('background', background.value.toString())
  },
})
</script>

<style scoped>
.background {
  position: fixed;
  left: 0;
  top: 0;
  z-index: -114514;
  width: 100%;
  height: 100%;
  overflow: hidden;
  background-color: black;
  background-size: cover;
  background-position: 50% 50%;
}
</style>
