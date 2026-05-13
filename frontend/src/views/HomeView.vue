<template>
  <div v-if="homeContent" class="min-h-screen">
    <iframe
      v-if="isUrlContent"
      :src="homeContent.trim()"
      class="h-screen w-full border-0"
      title="Home Content"
    ></iframe>
    <div v-else v-html="homeContent"></div>
  </div>

  <div v-else class="min-h-screen bg-gray-50 text-gray-900 dark:bg-dark-950 dark:text-gray-100">
    <div class="pointer-events-none fixed inset-0 bg-mesh-gradient"></div>
    <MarketingHeader />

    <main class="relative">
      <section class="mx-auto grid max-w-7xl gap-10 px-4 py-14 sm:px-6 lg:grid-cols-[1.05fr_0.95fr] lg:px-8 lg:py-20">
        <div class="flex flex-col justify-center">
          <div class="badge badge-primary w-fit">
            <Icon name="sparkles" size="xs" />
            {{ copy.hero.badge }}
          </div>
          <h1 class="mt-6 max-w-3xl text-4xl font-bold tracking-tight text-gray-950 dark:text-white sm:text-5xl lg:text-6xl">
            {{ copy.hero.title }}
          </h1>
          <p class="mt-5 max-w-2xl text-base leading-8 text-gray-600 dark:text-dark-300 sm:text-lg">
            {{ copy.hero.description }}
          </p>
          <div class="mt-8 flex flex-col gap-3 sm:flex-row">
            <router-link :to="primaryCtaPath" class="btn btn-primary btn-lg">
              {{ isAuthenticated ? copy.hero.dashboard : copy.hero.start }}
              <Icon name="arrowRight" size="sm" />
            </router-link>
            <router-link to="/pricing" class="btn btn-secondary btn-lg">
              {{ copy.hero.pricing }}
            </router-link>
          </div>
          <div class="mt-8 grid gap-3 sm:grid-cols-3">
            <div
              v-for="stat in copy.hero.stats"
              :key="stat.label"
              class="rounded-2xl border border-gray-100 bg-white/80 p-4 shadow-card dark:border-dark-700/50 dark:bg-dark-800/70"
            >
              <div class="text-2xl font-bold text-gray-950 dark:text-white">{{ stat.value }}</div>
              <div class="mt-1 text-sm text-gray-500 dark:text-dark-400">{{ stat.label }}</div>
            </div>
          </div>
        </div>

        <div class="flex items-center">
          <div class="w-full overflow-hidden rounded-2xl border border-gray-100 bg-white p-5 shadow-card dark:border-dark-700/50 dark:bg-dark-800/80">
            <div class="flex items-center justify-between gap-4 border-b border-gray-100 pb-4 dark:border-dark-700">
              <div>
                <p class="text-sm font-semibold text-gray-900 dark:text-white">{{ copy.panel.title }}</p>
                <p class="mt-1 text-xs text-gray-500 dark:text-dark-400">{{ copy.panel.subtitle }}</p>
              </div>
              <span class="badge badge-success">{{ copy.panel.status }}</span>
            </div>

            <div class="relative my-8 min-h-[260px] rounded-2xl bg-gray-50 p-5 dark:bg-dark-900/70">
              <div class="absolute inset-10 rounded-full border border-dashed border-primary-300/60 dark:border-primary-700/60"></div>
              <div class="absolute left-1/2 top-1/2 flex h-24 w-24 -translate-x-1/2 -translate-y-1/2 flex-col items-center justify-center rounded-2xl bg-gradient-primary text-white shadow-glow">
                <Icon name="server" size="lg" />
                <span class="mt-2 text-xs font-semibold">API</span>
              </div>
              <div
                v-for="node in modelNodes"
                :key="node.name"
                class="absolute flex min-w-[132px] items-center gap-2 rounded-xl border border-gray-200 bg-white px-3 py-2 text-sm font-semibold text-gray-700 shadow-sm dark:border-dark-700 dark:bg-dark-800 dark:text-gray-100"
                :class="node.position"
              >
                <Icon :name="node.icon" size="sm" class="text-primary-500" />
                {{ node.name }}
              </div>
            </div>

            <div class="grid gap-3 sm:grid-cols-3">
              <div
                v-for="item in copy.panel.metrics"
                :key="item.label"
                class="rounded-xl bg-gray-50 p-3 dark:bg-dark-900/70"
              >
                <p class="text-xs text-gray-500 dark:text-dark-400">{{ item.label }}</p>
                <p class="mt-1 text-sm font-semibold text-gray-900 dark:text-white">{{ item.value }}</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="border-y border-gray-200 bg-white/70 py-14 dark:border-dark-800 dark:bg-dark-900/40">
        <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
          <div class="grid gap-6 lg:grid-cols-[0.8fr_1.2fr] lg:items-end">
            <div>
              <p class="text-sm font-semibold text-primary-600 dark:text-primary-400">{{ copy.platforms.eyebrow }}</p>
              <h2 class="mt-2 text-3xl font-bold tracking-tight text-gray-950 dark:text-white">{{ copy.platforms.title }}</h2>
            </div>
            <p class="text-sm leading-7 text-gray-600 dark:text-dark-300">{{ copy.platforms.description }}</p>
          </div>
          <div class="mt-8 grid gap-4 md:grid-cols-2">
            <article
              v-for="platform in copy.platforms.items"
              :key="platform.name"
              class="card card-hover p-6"
            >
              <div class="flex h-11 w-11 items-center justify-center rounded-xl bg-primary-100 text-primary-600 dark:bg-primary-900/30 dark:text-primary-300">
                <Icon :name="platform.icon" size="md" />
              </div>
              <h3 class="mt-4 text-lg font-semibold text-gray-950 dark:text-white">{{ platform.name }}</h3>
              <div class="mt-4 flex flex-wrap gap-2">
                <span
                  v-for="model in platform.models"
                  :key="model"
                  class="badge badge-gray font-mono"
                >
                  {{ model }}
                </span>
              </div>
            </article>
          </div>
        </div>
      </section>

      <section class="mx-auto max-w-7xl px-4 py-14 sm:px-6 lg:px-8 lg:py-16">
        <div class="grid gap-8 lg:grid-cols-[0.9fr_1.1fr] lg:items-start">
          <div>
            <p class="text-sm font-semibold text-primary-600 dark:text-primary-400">{{ copy.pricing.eyebrow }}</p>
            <h2 class="mt-2 text-3xl font-bold tracking-tight text-gray-950 dark:text-white">{{ copy.pricing.title }}</h2>
            <p class="mt-4 text-sm leading-7 text-gray-600 dark:text-dark-300">{{ copy.pricing.description }}</p>
            <router-link to="/pricing" class="btn btn-primary btn-md mt-6">
              {{ copy.pricing.action }}
              <Icon name="arrowRight" size="sm" />
            </router-link>
          </div>

          <div class="card p-6">
            <div class="flex items-start gap-4">
              <div class="flex h-12 w-12 items-center justify-center rounded-xl bg-primary-100 text-primary-600 dark:bg-primary-900/30 dark:text-primary-300">
                <Icon name="creditCard" size="lg" />
              </div>
              <div>
                <h3 class="text-lg font-semibold text-gray-950 dark:text-white">{{ copy.pricing.cardTitle }}</h3>
                <p class="mt-2 text-sm leading-7 text-gray-600 dark:text-dark-300">{{ copy.pricing.cardDescription }}</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="mx-auto max-w-7xl px-4 pb-16 sm:px-6 lg:px-8">
        <div class="overflow-hidden rounded-2xl bg-gradient-primary p-8 text-white shadow-glow-lg md:p-10">
          <div class="grid gap-6 md:grid-cols-[1fr_auto] md:items-center">
            <div>
              <h2 class="text-2xl font-bold tracking-tight md:text-3xl">{{ copy.cta.title }}</h2>
              <p class="mt-3 max-w-2xl text-sm leading-7 text-primary-50">{{ copy.cta.description }}</p>
            </div>
            <router-link :to="primaryCtaPath" class="btn bg-white text-primary-700 hover:bg-primary-50">
              {{ isAuthenticated ? copy.hero.dashboard : copy.cta.action }}
              <Icon name="arrowRight" size="sm" />
            </router-link>
          </div>
        </div>
      </section>
    </main>

    <MarketingFooter />
  </div>
</template>

<script setup lang="ts">
import { computed, watchEffect } from 'vue'
import { useI18n } from 'vue-i18n'
import MarketingFooter from '@/components/marketing/MarketingFooter.vue'
import MarketingHeader from '@/components/marketing/MarketingHeader.vue'
import Icon from '@/components/icons/Icon.vue'
import { useAppStore, useAuthStore } from '@/stores'

const appStore = useAppStore()
const authStore = useAuthStore()
const { locale } = useI18n()

const claudeModels = ['claude-opus-4-7', 'claude-opus-4-6', 'claude-sonnet-4-6', 'claude-haiku-4-5'] as const
const openAiModels = ['gpt-5.5', 'gpt-5.4', 'gpt-5.3-codex'] as const

const homeCopy = {
  zh: {
    pageTitle: '首页',
    hero: {
      badge: 'AI API 网关',
      title: '统一接入 Claude 和 OpenAI 模型',
      description: '使用一个 API 网关管理密钥、余额和模型调用，当前支持 Claude 与 OpenAI（GPT）两个平台。',
      start: '立即开始',
      dashboard: '进入控制台',
      pricing: '查看模型',
      stats: [
        { value: '2', label: '支持平台' },
        { value: '7', label: '支持模型' },
        { value: '1:1', label: '充值比例' }
      ]
    },
    panel: {
      title: '统一调用入口',
      subtitle: 'Claude 与 OpenAI 模型集中接入',
      status: '在线',
      metrics: [
        { label: '平台', value: 'Claude / OpenAI' },
        { label: '调用方式', value: 'API Key' },
        { label: '余额', value: '美元余额' }
      ]
    },
    platforms: {
      eyebrow: '支持平台',
      title: '当前支持 Claude 和 OpenAI',
      description: '页面只展示当前可用的平台与模型 ID。',
      items: [
        { name: 'Claude', icon: 'beaker', models: claudeModels },
        { name: 'OpenAI (GPT)', icon: 'cpu', models: openAiModels }
      ]
    },
    pricing: {
      eyebrow: '充值比例',
      title: '充值 1 人民币得到 1 美元余额',
      description: '充值后的美元余额用于模型调用扣费。',
      action: '查看模型列表',
      cardTitle: '余额规则',
      cardDescription: '充值 1 人民币可得到 1 美元余额，当前不展示人民币模型单价。'
    },
    cta: {
      title: '开始使用支持的模型',
      description: '进入控制台创建 API Key 后，即可调用 Claude 或 OpenAI 模型。',
      action: '开始配置'
    }
  },
  en: {
    pageTitle: 'Home',
    hero: {
      badge: 'AI API Gateway',
      title: 'Unified access to Claude and OpenAI models',
      description: 'Use one API gateway to manage keys, balance, and model calls. Claude and OpenAI (GPT) are currently supported.',
      start: 'Get started',
      dashboard: 'Dashboard',
      pricing: 'View models',
      stats: [
        { value: '2', label: 'Platforms' },
        { value: '7', label: 'Models' },
        { value: '1:1', label: 'Top-up ratio' }
      ]
    },
    panel: {
      title: 'Unified endpoint',
      subtitle: 'Centralized access to Claude and OpenAI models',
      status: 'Online',
      metrics: [
        { label: 'Platforms', value: 'Claude / OpenAI' },
        { label: 'Access', value: 'API Key' },
        { label: 'Balance', value: 'USD balance' }
      ]
    },
    platforms: {
      eyebrow: 'Supported platforms',
      title: 'Claude and OpenAI are currently supported',
      description: 'Only currently available platforms and model IDs are shown.',
      items: [
        { name: 'Claude', icon: 'beaker', models: claudeModels },
        { name: 'OpenAI (GPT)', icon: 'cpu', models: openAiModels }
      ]
    },
    pricing: {
      eyebrow: 'Top-up ratio',
      title: '1 RMB top-up gives 1 USD balance',
      description: 'Your USD balance is used for model usage billing.',
      action: 'View model list',
      cardTitle: 'Balance rule',
      cardDescription: 'A 1 RMB top-up gives 1 USD balance. The frontend does not show RMB model unit prices.'
    },
    cta: {
      title: 'Start using supported models',
      description: 'Create an API key in the dashboard, then call Claude or OpenAI models.',
      action: 'Start setup'
    }
  }
} as const

const modelNodes = [
  { name: 'Claude', icon: 'beaker', position: 'left-6 top-10' },
  { name: 'OpenAI (GPT)', icon: 'cpu', position: 'bottom-10 right-6' }
] as const

const activeLocale = computed(() => locale.value === 'zh' ? 'zh' : 'en')
const copy = computed(() => homeCopy[activeLocale.value])
const homeContent = computed(() => appStore.cachedPublicSettings?.home_content || '')
const isAuthenticated = computed(() => authStore.isAuthenticated)
const primaryCtaPath = computed(() => {
  if (!isAuthenticated.value) {
    return '/login'
  }
  return authStore.isAdmin ? '/admin/dashboard' : '/dashboard'
})
const siteName = computed(() => appStore.cachedPublicSettings?.site_name || appStore.siteName || 'Sub2API')

const isUrlContent = computed(() => {
  const content = homeContent.value.trim()
  return content.startsWith('http://') || content.startsWith('https://') || content.startsWith('/')
})

watchEffect(() => {
  document.title = `${copy.value.pageTitle} - ${siteName.value}`
})
</script>
