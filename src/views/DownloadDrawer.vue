<template>
  <g-drawer>
    <n-divider dashed> 云端字体 </n-divider>
    <g-fonts-list :fonts="cloudFonts" hoverable @click="download" />
  </g-drawer>
</template>

<script setup lang="ts">
import GDrawer from '@/components/GDrawer.vue'
import GFontsList from '@/components/GFontsList.vue'

import { useMessage } from 'naive-ui'
const message = useMessage()

import { useDBStore, type CloudFontData } from '@/stores/db'
const DB = useDBStore()

import { useRouter } from 'vue-router'
const router = useRouter()

defineProps<{
  cloudFonts: CloudFontData[]
}>()

async function download(fontKey: number) {
  try {
    const font = <number>await DB.downloadFont(fontKey)
    const time = new Date().getTime()
    await router.replace({
      name: 'home',
      params: { font, time },
    })
  } catch (e) {
    message.error('下载失败')
  }
}
</script>
