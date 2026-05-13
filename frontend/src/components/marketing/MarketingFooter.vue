<template>
  <footer class="border-t border-white/10 bg-[#171713] px-5 py-12 text-[#a09d96] sm:px-8">
    <div class="mx-auto grid max-w-7xl gap-10 md:grid-cols-[1.4fr_1fr_1fr]">
      <div>
        <router-link to="/home" class="inline-flex items-center gap-3">
          <span
            class="flex h-10 w-10 items-center justify-center overflow-hidden rounded-lg border border-primary-300/25 bg-[#252320]"
          >
            <img
              v-if="siteLogo"
              :src="siteLogo"
              alt="Logo"
              class="h-full w-full object-contain"
            />
            <Icon v-else name="sparkles" size="md" class="text-primary-300" />
          </span>
          <span class="font-serif text-2xl font-semibold text-[#faf9f5]">{{ siteName }}</span>
        </router-link>
        <p class="mt-5 max-w-md text-sm leading-7">
          将订阅、账号池、计费和模型路由收束到一个 API 网关，让团队用一把密钥稳定调用主流 AI 模型。
        </p>
        <p class="mt-6 text-sm">&copy; {{ currentYear }} {{ siteName }}. 保留所有权利。</p>
      </div>

      <div>
        <h2 class="text-sm font-semibold text-[#faf9f5]">产品</h2>
        <div class="mt-4 grid gap-3 text-sm">
          <router-link class="transition-colors hover:text-white" to="/pricing">模型价格</router-link>
          <router-link class="transition-colors hover:text-white" to="/docs">接入文档</router-link>
          <router-link class="transition-colors hover:text-white" to="/key-usage">用量查询</router-link>
        </div>
      </div>

      <div>
        <h2 class="text-sm font-semibold text-[#faf9f5]">资源</h2>
        <div class="mt-4 grid gap-3 text-sm">
          <a
            class="transition-colors hover:text-white"
            href="https://github.com/Wei-Shaw/sub2api"
            target="_blank"
            rel="noopener noreferrer"
          >
            GitHub
          </a>
          <a
            v-if="docUrl"
            class="transition-colors hover:text-white"
            :href="docUrl"
            target="_blank"
            rel="noopener noreferrer"
          >
            外部文档
          </a>
          <router-link class="transition-colors hover:text-white" :to="{ path: '/home', hash: '#faq' }">
            常见问题
          </router-link>
        </div>
      </div>
    </div>
  </footer>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import Icon from '@/components/icons/Icon.vue'
import { useAppStore } from '@/stores'

const appStore = useAppStore()

const siteName = computed(() => appStore.cachedPublicSettings?.site_name || appStore.siteName || 'Sub2API')
const siteLogo = computed(() => appStore.cachedPublicSettings?.site_logo || appStore.siteLogo || '')
const docUrl = computed(() => appStore.cachedPublicSettings?.doc_url || appStore.docUrl || '')
const currentYear = computed(() => new Date().getFullYear())
</script>
