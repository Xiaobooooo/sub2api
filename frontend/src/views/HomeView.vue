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
                <p class="text-sm font-semibold text-gray-900 dark:text-white">{{ copy.hero.panel.title }}</p>
                <p class="mt-1 text-xs text-gray-500 dark:text-dark-400">{{ copy.hero.panel.subtitle }}</p>
              </div>
              <span class="badge badge-success">{{ copy.hero.panel.status }}</span>
            </div>

            <div class="relative my-8 min-h-[300px] rounded-2xl bg-gray-50 p-5 dark:bg-dark-900/70">
              <div class="absolute inset-10 rounded-full border border-dashed border-primary-300/60 dark:border-primary-700/60"></div>
              <div class="absolute inset-20 rounded-full border border-gray-200 dark:border-dark-700"></div>
              <div class="absolute left-1/2 top-1/2 flex h-24 w-24 -translate-x-1/2 -translate-y-1/2 flex-col items-center justify-center rounded-2xl bg-gradient-primary text-white shadow-glow">
                <Icon name="server" size="lg" />
                <span class="mt-2 text-xs font-semibold">API</span>
              </div>
              <div
                v-for="node in modelNodes"
                :key="node.name"
                class="absolute flex min-w-[112px] items-center gap-2 rounded-xl border border-gray-200 bg-white px-3 py-2 text-sm font-semibold text-gray-700 shadow-sm dark:border-dark-700 dark:bg-dark-800 dark:text-gray-100"
                :class="node.position"
              >
                <Icon :name="node.icon" size="sm" class="text-primary-500" />
                {{ node.name }}
              </div>
            </div>

            <div class="grid gap-3 sm:grid-cols-3">
              <div
                v-for="item in copy.hero.panel.metrics"
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
              <p class="text-sm font-semibold text-primary-600 dark:text-primary-400">{{ copy.tools.eyebrow }}</p>
              <h2 class="mt-2 text-3xl font-bold tracking-tight text-gray-950 dark:text-white">{{ copy.tools.title }}</h2>
            </div>
            <p class="text-sm leading-7 text-gray-600 dark:text-dark-300">{{ copy.tools.description }}</p>
          </div>
          <div class="mt-8 grid gap-4 sm:grid-cols-2 lg:grid-cols-4">
            <article
              v-for="tool in copy.tools.items"
              :key="tool.name"
              class="card card-hover p-5"
            >
              <div class="flex h-11 w-11 items-center justify-center rounded-xl bg-primary-100 text-primary-600 dark:bg-primary-900/30 dark:text-primary-300">
                <Icon :name="tool.icon" size="md" />
              </div>
              <h3 class="mt-4 font-semibold text-gray-950 dark:text-white">{{ tool.name }}</h3>
              <p class="mt-2 text-sm leading-6 text-gray-500 dark:text-dark-400">{{ tool.description }}</p>
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

          <div class="card overflow-hidden">
            <div class="card-header flex items-center justify-between gap-4">
              <div>
                <h3 class="font-semibold text-gray-950 dark:text-white">{{ copy.pricing.cardTitle }}</h3>
                <p class="mt-1 text-xs text-gray-500 dark:text-dark-400">{{ copy.pricing.cardHint }}</p>
              </div>
              <span class="badge badge-primary">{{ copy.pricing.badge }}</span>
            </div>
            <div class="divide-y divide-gray-100 dark:divide-dark-700">
              <div
                v-for="row in copy.pricing.rows"
                :key="row.model"
                class="grid gap-3 px-6 py-4 sm:grid-cols-[1fr_auto_auto] sm:items-center"
              >
                <div>
                  <p class="font-mono text-sm font-semibold text-gray-900 dark:text-white">{{ row.model }}</p>
                  <p class="mt-1 text-xs text-gray-500 dark:text-dark-400">{{ row.scene }}</p>
                </div>
                <div class="text-sm text-gray-500 dark:text-dark-400">{{ row.official }}</div>
                <div class="text-right text-base font-bold text-primary-600 dark:text-primary-400">{{ row.group }}</div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="border-y border-gray-200 bg-white/70 py-14 dark:border-dark-800 dark:bg-dark-900/40">
        <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
          <div class="max-w-3xl">
            <p class="text-sm font-semibold text-primary-600 dark:text-primary-400">{{ copy.values.eyebrow }}</p>
            <h2 class="mt-2 text-3xl font-bold tracking-tight text-gray-950 dark:text-white">{{ copy.values.title }}</h2>
            <p class="mt-4 text-sm leading-7 text-gray-600 dark:text-dark-300">{{ copy.values.description }}</p>
          </div>
          <div class="mt-8 grid gap-4 md:grid-cols-3">
            <article
              v-for="item in copy.values.items"
              :key="item.title"
              class="card p-6"
            >
              <div class="flex h-12 w-12 items-center justify-center rounded-xl bg-primary-100 text-primary-600 dark:bg-primary-900/30 dark:text-primary-300">
                <Icon :name="item.icon" size="lg" />
              </div>
              <h3 class="mt-5 text-lg font-semibold text-gray-950 dark:text-white">{{ item.title }}</h3>
              <p class="mt-3 text-sm leading-7 text-gray-500 dark:text-dark-400">{{ item.description }}</p>
            </article>
          </div>
        </div>
      </section>

      <section id="faq" class="mx-auto max-w-5xl px-4 py-14 sm:px-6 lg:px-8 lg:py-16">
        <div class="text-center">
          <p class="text-sm font-semibold text-primary-600 dark:text-primary-400">{{ copy.faq.eyebrow }}</p>
          <h2 class="mt-2 text-3xl font-bold tracking-tight text-gray-950 dark:text-white">{{ copy.faq.title }}</h2>
        </div>
        <div class="mt-8 grid gap-3">
          <details
            v-for="item in copy.faq.items"
            :key="item.question"
            class="card group px-5 py-4"
          >
            <summary class="flex cursor-pointer list-none items-center justify-between gap-4 font-semibold text-gray-900 dark:text-white">
              {{ item.question }}
              <Icon name="chevronDown" size="sm" class="text-gray-400 transition-transform group-open:rotate-180" />
            </summary>
            <p class="mt-3 text-sm leading-7 text-gray-600 dark:text-dark-300">{{ item.answer }}</p>
          </details>
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

const homeCopy = {
  zh: {
    pageTitle: '首页',
    hero: {
      badge: '参考 APIKEY.FUN 的产品叙事',
      title: '连接全球顶级 AI 大模型',
      description: '保留 APIKEY.FUN 的落地页信息结构：模型入口、价格对照、接入教程和常见问题；视觉上沿用 Sub2API 的浅色卡片、陶土主色和后台产品气质。',
      start: '立即开始',
      dashboard: '进入控制台',
      pricing: '查看价格',
      stats: [
        { value: '1 Key', label: '统一调用入口' },
        { value: '3+', label: '主流客户端场景' },
        { value: '¥', label: '人民币价格展示' }
      ],
      panel: {
        title: '统一 AI API 网关',
        subtitle: '模型路由、账号池、计费与密钥管理集中处理',
        status: '在线',
        metrics: [
          { label: '兼容协议', value: 'OpenAI / Claude / Gemini' },
          { label: '调用方式', value: 'API Key' },
          { label: '管理入口', value: 'Dashboard' }
        ]
      }
    },
    tools: {
      eyebrow: '兼容生态',
      title: '支持主流设备与 AI 编程工具',
      description: '布局参考 APIKEY.FUN 的工具入口节奏：首页直接展示开发者最关心的接入对象，便于从落地页进入价格和文档。',
      items: [
        { name: 'Claude Code', icon: 'terminal', description: '适合代码生成、项目修改和长上下文自动化任务。' },
        { name: 'Codex', icon: 'cpu', description: '通过 OpenAI 兼容配置接入当前平台的模型路由。' },
        { name: 'Gemini CLI', icon: 'sparkles', description: '面向命令行和文件分析场景，支持快速切换 Base URL。' },
        { name: 'OpenClaw', icon: 'cloud', description: '兼容更多客户端和代理工具，便于团队统一配置。' }
      ]
    },
    pricing: {
      eyebrow: '模型价格',
      title: '先看价格，再决定接入方式',
      description: '价格区块保留官方价与分组价对照、按场景切换、人民币展示的内容结构，让用户能快速比较 Claude Code、Codex、Gemini 等入口成本。',
      action: '查看完整价格',
      cardTitle: '价格速览',
      cardHint: '示例价格用于展示页面结构，最终以控制台配置为准。',
      badge: '按 ¥ 计价',
      rows: [
        { model: 'claude-sonnet-4.5', scene: 'Claude Code / Pro', official: '官方 ¥21.00', group: '分组 ¥4.20' },
        { model: 'gpt-5.2', scene: 'Codex / Pro', official: '官方 ¥8.40', group: '分组 ¥1.68' },
        { model: 'gemini-2.5-pro', scene: 'Gemini / Pro', official: '官方 ¥10.50', group: '分组 ¥2.10' }
      ]
    },
    values: {
      eyebrow: '平台能力',
      title: '把订阅、账号池和模型调用收束到一个入口',
      description: '内容参考 APIKEY 的价值表达，但交互和视觉保持项目内管理后台的一致性。',
      items: [
        { icon: 'key', title: '统一密钥', description: '团队只需要分发一把 API Key，后台负责映射到不同模型和上游账号。' },
        { icon: 'chart', title: '清晰计费', description: '用量、价格、缓存读写和节省幅度统一展示，便于成本核算。' },
        { icon: 'shield', title: '稳定路由', description: '通过账号池、分组倍率和模型路由降低单点限制带来的中断风险。' }
      ]
    },
    faq: {
      eyebrow: '常见问题',
      title: '开始接入前需要知道什么？',
      items: [
        { question: '这个页面和 APIKEY.FUN 的关系是什么？', answer: '这里只参考它的信息组织方式和页面布局节奏，不复刻配色、字体和装饰风格。' },
        { question: '价格是否会实时读取后台？', answer: '当前公开页展示的是说明型价格结构，正式价格以后台分组、倍率和渠道配置为准。' },
        { question: '是否支持中英文切换？', answer: '支持。页面文案会跟随右上角语言切换实时更新，路由和交互状态不会丢失。' },
        { question: '后端模式能访问公开页吗？', answer: '后端模式保持原有安全边界，未登录访问公开营销页会被引导到登录页。' }
      ]
    },
    cta: {
      title: '准备接入你的 AI 客户端？',
      description: '先查看价格，再按文档配置客户端。已有账号的用户可以直接进入控制台创建 API Key。',
      action: '开始配置'
    }
  },
  en: {
    pageTitle: 'Home',
    hero: {
      badge: 'Content structure inspired by APIKEY.FUN',
      title: 'Connect to top global AI models',
      description: 'The page keeps APIKEY.FUN’s information flow: model entry points, pricing comparison, setup docs, and FAQ. The visual system now follows Sub2API’s light cards, warm primary color, and product-dashboard style.',
      start: 'Get started',
      dashboard: 'Dashboard',
      pricing: 'View pricing',
      stats: [
        { value: '1 Key', label: 'Unified endpoint' },
        { value: '3+', label: 'Client scenarios' },
        { value: '¥', label: 'CNY pricing' }
      ],
      panel: {
        title: 'Unified AI API Gateway',
        subtitle: 'Model routing, account pools, billing, and key management in one place',
        status: 'Online',
        metrics: [
          { label: 'Protocols', value: 'OpenAI / Claude / Gemini' },
          { label: 'Access', value: 'API Key' },
          { label: 'Management', value: 'Dashboard' }
        ]
      }
    },
    tools: {
      eyebrow: 'Compatible ecosystem',
      title: 'Works with mainstream AI coding tools',
      description: 'The section follows APIKEY.FUN’s tool-entry rhythm while using the existing product UI language.',
      items: [
        { name: 'Claude Code', icon: 'terminal', description: 'For code generation, project edits, and long-context automation.' },
        { name: 'Codex', icon: 'cpu', description: 'Connect through OpenAI-compatible settings and Sub2API routing.' },
        { name: 'Gemini CLI', icon: 'sparkles', description: 'For command-line and file analysis workflows with quick Base URL switching.' },
        { name: 'OpenClaw', icon: 'cloud', description: 'Compatible with more clients and proxy tools for team-wide setup.' }
      ]
    },
    pricing: {
      eyebrow: 'Pricing',
      title: 'Compare cost before choosing an integration',
      description: 'The pricing section keeps official vs group price comparison, scenario tabs, and CNY display so users can compare Claude Code, Codex, and Gemini entry costs quickly.',
      action: 'View full pricing',
      cardTitle: 'Pricing snapshot',
      cardHint: 'Example prices show the page structure. Final pricing follows dashboard configuration.',
      badge: 'CNY',
      rows: [
        { model: 'claude-sonnet-4.5', scene: 'Claude Code / Pro', official: 'Official ¥21.00', group: 'Group ¥4.20' },
        { model: 'gpt-5.2', scene: 'Codex / Pro', official: 'Official ¥8.40', group: 'Group ¥1.68' },
        { model: 'gemini-2.5-pro', scene: 'Gemini / Pro', official: 'Official ¥10.50', group: 'Group ¥2.10' }
      ]
    },
    values: {
      eyebrow: 'Platform capabilities',
      title: 'Unify subscriptions, account pools, and model access',
      description: 'The value story is inspired by APIKEY, while the interaction and styling stay aligned with this project.',
      items: [
        { icon: 'key', title: 'Unified key', description: 'Distribute one API key while the backend maps requests to models and upstream accounts.' },
        { icon: 'chart', title: 'Clear billing', description: 'Usage, price, cache reads/writes, and savings are shown in one cost model.' },
        { icon: 'shield', title: 'Stable routing', description: 'Account pools, group multipliers, and model routing reduce interruption risk.' }
      ]
    },
    faq: {
      eyebrow: 'FAQ',
      title: 'What should I know before integrating?',
      items: [
        { question: 'How does this relate to APIKEY.FUN?', answer: 'It references the content structure and layout rhythm only. Colors, typography, and component styling follow Sub2API.' },
        { question: 'Are prices loaded live from the backend?', answer: 'The public page currently shows an explanatory pricing structure. Final prices follow dashboard groups, multipliers, and channels.' },
        { question: 'Does language switching work?', answer: 'Yes. Page copy updates when the language switcher changes locale, without losing route or interaction state.' },
        { question: 'Can backend mode access these public pages?', answer: 'Backend mode keeps the existing security boundary and redirects unauthenticated users to login.' }
      ]
    },
    cta: {
      title: 'Ready to connect your AI client?',
      description: 'Compare pricing first, then follow the docs to configure your client. Existing users can create an API key in the dashboard.',
      action: 'Start setup'
    }
  }
} as const

const modelNodes = [
  { name: 'Claude', icon: 'beaker', position: 'left-6 top-8' },
  { name: 'Codex', icon: 'cpu', position: 'right-6 top-12' },
  { name: 'Gemini', icon: 'sparkles', position: 'bottom-10 right-8' },
  { name: 'ChatGPT', icon: 'chat', position: 'bottom-12 left-8' }
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
