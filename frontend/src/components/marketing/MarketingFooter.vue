<template>
  <footer class="border-t border-gray-200 bg-white px-4 py-10 text-gray-500 dark:border-dark-800 dark:bg-dark-900 dark:text-dark-400 sm:px-6 lg:px-8">
    <div class="mx-auto grid max-w-7xl gap-10 md:grid-cols-[1.4fr_1fr_1fr]">
      <div>
        <router-link to="/home" class="inline-flex items-center gap-3 text-gray-900 dark:text-white">
          <span class="sidebar-logo flex h-9 w-9 shrink-0 items-center justify-center overflow-hidden rounded-xl shadow-glow">
            <img :src="logoSrc" alt="Logo" class="h-full w-full object-contain" />
          </span>
          <span class="text-xl font-bold tracking-tight">{{ siteName }}</span>
        </router-link>
        <p class="mt-4 max-w-md text-sm leading-7">
          {{ copy.description }}
        </p>
        <p class="mt-6 text-sm">&copy; {{ currentYear }} {{ siteName }}. {{ copy.rights }}</p>
      </div>

      <div>
        <h2 class="text-sm font-semibold text-gray-900 dark:text-white">{{ copy.product }}</h2>
        <div class="mt-4 grid gap-3 text-sm">
          <router-link class="transition-colors hover:text-primary-600 dark:hover:text-primary-400" to="/pricing">
            {{ copy.pricing }}
          </router-link>
          <router-link class="transition-colors hover:text-primary-600 dark:hover:text-primary-400" to="/docs">
            {{ copy.docs }}
          </router-link>
          <router-link class="transition-colors hover:text-primary-600 dark:hover:text-primary-400" to="/key-usage">
            {{ copy.usage }}
          </router-link>
        </div>
      </div>

      <div>
        <h2 class="text-sm font-semibold text-gray-900 dark:text-white">{{ copy.resources }}</h2>
        <div class="mt-4 grid gap-3 text-sm">
          <a
            class="transition-colors hover:text-primary-600 dark:hover:text-primary-400"
            href="https://github.com/Wei-Shaw/sub2api"
            target="_blank"
            rel="noopener noreferrer"
          >
            GitHub
          </a>
          <a
            v-if="docUrl"
            class="transition-colors hover:text-primary-600 dark:hover:text-primary-400"
            :href="docUrl"
            target="_blank"
            rel="noopener noreferrer"
          >
            {{ copy.externalDocs }}
          </a>
          <router-link
            class="transition-colors hover:text-primary-600 dark:hover:text-primary-400"
            :to="{ path: '/home', hash: '#faq' }"
          >
            {{ copy.faq }}
          </router-link>
        </div>
      </div>
    </div>
  </footer>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useI18n } from 'vue-i18n'
import { useAppStore } from '@/stores'

const appStore = useAppStore()
const { locale } = useI18n()

const footerCopy = {
  zh: {
    description: '将订阅、账号池、计费和模型路由收束到一个 API 网关，让团队用一把密钥稳定调用主流 AI 模型。',
    rights: '保留所有权利。',
    product: '产品',
    pricing: '模型价格',
    docs: '接入文档',
    usage: '用量查询',
    resources: '资源',
    externalDocs: '外部文档',
    faq: '常见问题'
  },
  en: {
    description: 'Unify subscriptions, account pools, billing, and model routing behind one API gateway for stable access to mainstream AI models.',
    rights: 'All rights reserved.',
    product: 'Product',
    pricing: 'Pricing',
    docs: 'Docs',
    usage: 'Usage lookup',
    resources: 'Resources',
    externalDocs: 'External docs',
    faq: 'FAQ'
  }
} as const

const activeLocale = computed(() => locale.value === 'zh' ? 'zh' : 'en')
const copy = computed(() => footerCopy[activeLocale.value])
const siteName = computed(() => appStore.cachedPublicSettings?.site_name || appStore.siteName || 'Sub2API')
const siteLogo = computed(() => appStore.cachedPublicSettings?.site_logo || appStore.siteLogo || '')
const logoSrc = computed(() => siteLogo.value || '/logo.png')
const docUrl = computed(() => appStore.cachedPublicSettings?.doc_url || appStore.docUrl || '')
const currentYear = computed(() => new Date().getFullYear())
</script>
