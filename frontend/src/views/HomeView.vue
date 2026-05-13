<template>
  <div v-if="homeContent" class="min-h-screen">
    <iframe
      v-if="isHomeContentUrl"
      :src="homeContent.trim()"
      class="h-screen w-full border-0"
      allowfullscreen
    ></iframe>
    <div v-else v-html="homeContent"></div>
  </div>

  <div v-else class="min-h-screen bg-[#171713] text-[#faf9f5]">
    <MarketingHeader />

    <main>
      <section class="relative overflow-hidden px-5 pb-20 pt-16 sm:px-8 lg:pb-28 lg:pt-24">
        <div class="absolute inset-x-0 bottom-0 h-40 bg-[#faf9f5]"></div>
        <div class="relative mx-auto grid max-w-7xl items-center gap-14 lg:grid-cols-[1.02fr_0.98fr]">
          <div>
            <p
              class="mb-6 inline-flex rounded-full bg-primary-700 px-4 py-2 text-xs font-semibold uppercase text-primary-50"
            >
              THE UNIVERSAL AI GATEWAY
            </p>
            <h1 class="max-w-3xl font-serif text-5xl font-semibold leading-tight text-white md:text-7xl">
              连接全球顶级
              <span class="block text-primary-200">AI 大模型</span>
            </h1>
            <p class="mt-7 max-w-2xl text-lg leading-8 text-[#d7d2c8] md:text-xl">
              {{ siteSubtitle }}
            </p>
            <div class="mt-10 flex flex-col gap-4 sm:flex-row">
              <router-link
                :to="isAuthenticated ? dashboardPath : '/login'"
                class="inline-flex h-14 items-center justify-center gap-3 rounded-lg bg-primary-600 px-7 text-base font-semibold text-white transition-colors hover:bg-primary-700"
              >
                {{ isAuthenticated ? '进入控制台' : '立即开始' }}
                <Icon name="arrowRight" size="md" />
              </router-link>
              <router-link
                to="/pricing"
                class="inline-flex h-14 items-center justify-center rounded-lg border border-white/15 bg-white/10 px-7 text-base font-semibold text-white transition-colors hover:bg-white/15"
              >
                查看价格
              </router-link>
            </div>
          </div>

          <div class="relative mx-auto flex min-h-[420px] w-full max-w-xl items-center justify-center">
            <div class="absolute inset-0 rounded-full border border-primary-200/10"></div>
            <div class="absolute h-[310px] w-[310px] rounded-full border border-dashed border-primary-300/25"></div>
            <div
              class="relative z-10 flex h-24 w-24 items-center justify-center rounded-2xl border border-primary-300/30 bg-primary-700 text-white shadow-glow"
            >
              <Icon name="server" size="xl" />
            </div>

            <div
              v-for="node in providerNodes"
              :key="node.name"
              class="absolute flex h-24 w-24 flex-col items-center justify-center rounded-full border border-primary-200/20 bg-[#252320] text-center text-primary-100 shadow-card"
              :class="node.position"
            >
              <Icon :name="node.icon" size="lg" />
              <span class="mt-2 text-xs font-semibold">{{ node.name }}</span>
            </div>
          </div>
        </div>
      </section>

      <section class="bg-[#faf9f5] px-5 py-20 text-[#141413] sm:px-8">
        <div class="mx-auto max-w-7xl">
          <p class="text-sm font-semibold text-primary-700">兼容生态</p>
          <div class="mt-4 grid gap-8 lg:grid-cols-[0.8fr_1.2fr] lg:items-end">
            <h2 class="font-serif text-4xl font-semibold leading-tight md:text-5xl">
              支持主流设备与 AI 编程工具
            </h2>
            <p class="text-base leading-8 text-[#5b5850]">
              参考 APIKEY.FUN 的工具入口节奏，首页直接展示开发者最关心的接入对象：命令行代理、桌面客户端、官方 CLI 和 OpenAI 兼容生态。
            </p>
          </div>

          <div class="mt-12 grid gap-5 md:grid-cols-2 xl:grid-cols-4">
            <article
              v-for="tool in toolCards"
              :key="tool.name"
              class="rounded-xl border border-[#e6dfd8] bg-[#efe9de] p-6"
            >
              <div class="flex h-11 w-11 items-center justify-center rounded-lg bg-[#171713] text-primary-200">
                <Icon :name="tool.icon" size="md" />
              </div>
              <h3 class="mt-6 text-xl font-semibold">{{ tool.name }}</h3>
              <p class="mt-3 text-sm leading-7 text-[#5b5850]">{{ tool.description }}</p>
              <p class="mt-6 text-xs font-semibold uppercase text-primary-700">{{ tool.meta }}</p>
            </article>
          </div>
        </div>
      </section>

      <section class="bg-[#faf9f5] px-5 pb-20 text-[#141413] sm:px-8">
        <div class="mx-auto grid max-w-7xl gap-6 lg:grid-cols-3">
          <article class="rounded-xl bg-[#171713] p-8 text-[#faf9f5] lg:col-span-1">
            <p class="text-sm font-semibold text-primary-200">定价方案</p>
            <h2 class="mt-4 font-serif text-4xl font-semibold leading-tight">
              按量付费，按需使用
            </h2>
            <p class="mt-5 text-sm leading-7 text-[#c7c1b7]">
              用价格页的分组结构表达成本差异，把开发者真正要比较的输入、输出、缓存价格放到更完整的 `/pricing` 页面。
            </p>
            <router-link
              to="/pricing"
              class="mt-8 inline-flex h-12 items-center justify-center gap-2 rounded-lg bg-primary-600 px-5 text-sm font-semibold text-white transition-colors hover:bg-primary-700"
            >
              查看详细价格
              <Icon name="arrowRight" size="sm" />
            </router-link>
          </article>

          <article
            v-for="plan in pricingHighlights"
            :key="plan.name"
            class="rounded-xl border border-[#e6dfd8] bg-white p-8"
          >
            <div class="flex items-start justify-between gap-4">
              <div>
                <p class="text-sm font-semibold text-primary-700">{{ plan.tag }}</p>
                <h3 class="mt-3 text-2xl font-semibold">{{ plan.name }}</h3>
              </div>
              <span class="rounded-full bg-primary-50 px-3 py-1 text-xs font-semibold text-primary-700">
                {{ plan.badge }}
              </span>
            </div>
            <p class="mt-6 font-serif text-4xl font-semibold">{{ plan.price }}</p>
            <p class="mt-4 text-sm leading-7 text-[#5b5850]">{{ plan.description }}</p>
            <ul class="mt-7 grid gap-3 text-sm text-[#3d3d3a]">
              <li v-for="feature in plan.features" :key="feature" class="flex gap-3">
                <Icon name="check" size="sm" class="mt-0.5 text-primary-600" />
                <span>{{ feature }}</span>
              </li>
            </ul>
          </article>
        </div>
      </section>

      <section class="bg-[#171713] px-5 py-20 sm:px-8">
        <div class="mx-auto grid max-w-7xl gap-12 lg:grid-cols-[0.85fr_1.15fr] lg:items-center">
          <div>
            <p class="text-sm font-semibold text-primary-200">使用价值</p>
            <h2 class="mt-4 font-serif text-4xl font-semibold leading-tight text-white md:text-5xl">
              释放团队的编程潜能
            </h2>
            <p class="mt-6 text-base leading-8 text-[#d7d2c8]">
              把网络、账号、账单和限额从业务代码里移走，保留 OpenAI 兼容调用体验，让团队专注在产品和自动化流程本身。
            </p>
          </div>

          <div class="grid gap-4 sm:grid-cols-2">
            <article
              v-for="item in valueCards"
              :key="item.title"
              class="rounded-xl border border-white/10 bg-white/[0.04] p-6"
            >
              <div class="flex h-10 w-10 items-center justify-center rounded-lg bg-primary-600 text-white">
                <Icon :name="item.icon" size="md" />
              </div>
              <h3 class="mt-5 text-lg font-semibold text-white">{{ item.title }}</h3>
              <p class="mt-3 text-sm leading-7 text-[#c7c1b7]">{{ item.description }}</p>
            </article>
          </div>
        </div>
      </section>

      <section id="faq" class="bg-[#faf9f5] px-5 py-20 text-[#141413] sm:px-8">
        <div class="mx-auto grid max-w-7xl gap-10 lg:grid-cols-[0.8fr_1.2fr]">
          <div>
            <p class="text-sm font-semibold text-primary-700">FAQ</p>
            <h2 class="mt-4 font-serif text-4xl font-semibold leading-tight">常见问题解答</h2>
          </div>
          <div class="grid gap-3">
            <button
              v-for="(item, index) in faqItems"
              :key="item.question"
              type="button"
              class="rounded-xl border border-[#e6dfd8] bg-white p-6 text-left"
              @click="openFaq = openFaq === index ? null : index"
            >
              <span class="flex items-center justify-between gap-4">
                <span class="text-lg font-semibold">{{ item.question }}</span>
                <Icon
                  name="chevronDown"
                  size="sm"
                  class="shrink-0 text-primary-700 transition-transform"
                  :class="openFaq === index ? 'rotate-180' : ''"
                />
              </span>
              <span
                v-if="openFaq === index"
                class="mt-4 block text-sm leading-7 text-[#5b5850]"
              >
                {{ item.answer }}
              </span>
            </button>
          </div>
        </div>
      </section>

      <section class="bg-[#faf9f5] px-5 pb-20 text-[#141413] sm:px-8">
        <div class="mx-auto max-w-7xl rounded-xl bg-primary-600 p-8 text-white md:p-14">
          <div class="grid gap-8 lg:grid-cols-[1fr_auto] lg:items-center">
            <div>
              <h2 class="font-serif text-4xl font-semibold leading-tight">准备好接入你的第一把 API Key 吗？</h2>
              <p class="mt-4 max-w-2xl text-base leading-8 text-primary-50">
                从控制台创建密钥，按文档改一个 Base URL，就能把现有 OpenAI 兼容客户端接入统一网关。
              </p>
            </div>
            <router-link
              :to="isAuthenticated ? dashboardPath : '/login'"
              class="inline-flex h-12 items-center justify-center rounded-lg bg-[#faf9f5] px-6 text-sm font-semibold text-[#141413] transition-colors hover:bg-white"
            >
              {{ isAuthenticated ? '进入控制台' : '立即登录' }}
            </router-link>
          </div>
        </div>
      </section>
    </main>

    <MarketingFooter />
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import { useAppStore, useAuthStore } from '@/stores'
import Icon from '@/components/icons/Icon.vue'
import MarketingHeader from '@/components/marketing/MarketingHeader.vue'
import MarketingFooter from '@/components/marketing/MarketingFooter.vue'

const authStore = useAuthStore()
const appStore = useAppStore()

const homeContent = computed(() => appStore.cachedPublicSettings?.home_content || '')
const siteSubtitle = computed(
  () =>
    appStore.cachedPublicSettings?.site_subtitle ||
    '免翻墙直连、降低封号风险、余额长期可用。支持支付宝和微信支付，低延迟调用 Claude、ChatGPT、Gemini 等主流模型。'
)
const isAuthenticated = computed(() => authStore.isAuthenticated)
const dashboardPath = computed(() => authStore.isAdmin ? '/admin/dashboard' : '/dashboard')
const openFaq = ref<number | null>(0)

const isHomeContentUrl = computed(() => {
  const content = homeContent.value.trim()
  return content.startsWith('http://') || content.startsWith('https://')
})

const providerNodes = [
  { name: 'Claude', icon: 'brain', position: 'left-[14%] top-[10%]' },
  { name: 'All Models', icon: 'cube', position: 'right-[10%] top-[7%]' },
  { name: 'ChatGPT', icon: 'sparkles', position: 'left-[20%] bottom-[12%]' },
  { name: 'Gemini', icon: 'sparkles', position: 'right-[8%] bottom-[16%]' }
] as const

const toolCards = [
  {
    name: 'OpenClaw',
    icon: 'sparkles',
    meta: '开源 · 本地运行',
    description: '本地 AI 助手入口，适合把长任务、文件编辑和命令行工作流接到统一网关。'
  },
  {
    name: 'Claude Code',
    icon: 'terminal',
    meta: 'Anthropic 官方',
    description: '面向代码理解、重构和自动化执行，保留熟悉的 Claude Code 使用方式。'
  },
  {
    name: 'Codex',
    icon: 'cpu',
    meta: 'OpenAI 官方',
    description: '支持 OpenAI 兼容调用链路，适合代码审查、测试生成和多文件改造。'
  },
  {
    name: 'Gemini CLI',
    icon: 'cloud',
    meta: 'Google 官方',
    description: '在终端内调用 Gemini 完成编码、调试和流程自动化，接入方式更集中。'
  }
] as const

const pricingHighlights = [
  {
    tag: 'PAYGO',
    name: '按量付费',
    badge: '永不过期',
    price: '1 RMB = 1 USD',
    description: '充值后按实际使用扣费，不需要为闲置订阅承担固定成本。',
    features: ['余额长期可用', '按模型实际消耗结算', '适合个人与小团队试用']
  },
  {
    tag: 'MODEL ROUTING',
    name: '模型分组',
    badge: '可切换',
    price: '多组倍率',
    description: '按 Claude、Codex、Gemini 等使用场景拆分分组，清楚比较模型成本。',
    features: ['展示官方价与分组价', '支持缓存创建和缓存读取价格', '适合团队成本治理']
  }
] as const

const valueCards = [
  {
    title: '国内直连',
    icon: 'globe',
    description: '减少网络不可达和长时间等待，让 CLI 与应用调用更稳定。'
  },
  {
    title: '高可用架构',
    icon: 'server',
    description: '账号池、分组和故障转移让关键任务不依赖单一上游账号。'
  },
  {
    title: '简单集成',
    icon: 'link',
    description: '保留 OpenAI 兼容体验，通常只需要替换 Base URL 和密钥。'
  },
  {
    title: '用量可控',
    icon: 'chart',
    description: '控制台统一查看消耗、限额、订单和成员使用情况。'
  }
] as const

const faqItems = [
  {
    question: '会有封号风险吗？',
    answer: '平台通过统一网关和账号池降低单点风险，但任何第三方模型调用都应遵守对应服务条款和使用政策。'
  },
  {
    question: '支持哪些模型？',
    answer: '当前页面重点呈现 Claude、OpenAI、Gemini 等主流入口，实际可用模型以控制台和管理员配置为准。'
  },
  {
    question: '额度会过期吗？',
    answer: '余额型充值适合长期按量使用；如果管理员启用了订阅套餐，则套餐有效期以购买页面展示为准。'
  },
  {
    question: '如何接入现有客户端？',
    answer: '打开接入文档，复制对应客户端的配置命令或手动填写 API Key 和 Base URL 即可。'
  }
] as const

onMounted(() => {
  authStore.checkAuth()
  if (!appStore.publicSettingsLoaded) {
    appStore.fetchPublicSettings()
  }
})
</script>
