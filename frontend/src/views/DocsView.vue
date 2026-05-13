<template>
  <div class="min-h-screen bg-gray-50 text-gray-900 dark:bg-dark-950 dark:text-gray-100">
    <div class="pointer-events-none fixed inset-0 bg-mesh-gradient"></div>
    <MarketingHeader />

    <main class="relative mx-auto grid max-w-7xl gap-8 px-4 py-10 sm:px-6 lg:grid-cols-[280px_1fr] lg:px-8 lg:py-14">
      <aside class="lg:sticky lg:top-24 lg:h-fit">
        <div class="card p-4">
          <p class="px-2 text-xs font-semibold uppercase tracking-wider text-gray-400 dark:text-dark-500">
            {{ copy.sidebar.title }}
          </p>
          <nav class="mt-3 grid gap-1">
            <button
              v-for="doc in docs"
              :key="doc.id"
              type="button"
              class="flex w-full items-center gap-3 rounded-xl px-3 py-3 text-left text-sm font-medium transition-colors"
              :class="activeDoc === doc.id
                ? 'bg-primary-50 text-primary-700 dark:bg-primary-900/20 dark:text-primary-300'
                : 'text-gray-600 hover:bg-gray-100 hover:text-gray-950 dark:text-dark-300 dark:hover:bg-dark-800 dark:hover:text-white'"
              @click="activeDoc = doc.id"
            >
              <Icon :name="doc.icon" size="sm" />
              {{ doc.shortTitle }}
            </button>
          </nav>
        </div>
      </aside>

      <section class="min-w-0">
        <div>
          <div class="badge badge-primary w-fit">
            <Icon name="book" size="xs" />
            {{ copy.hero.badge }}
          </div>
          <h1 class="mt-5 text-4xl font-bold tracking-tight text-gray-950 dark:text-white sm:text-5xl">
            {{ activeDocData.title }}
          </h1>
          <p class="mt-4 max-w-3xl text-base leading-8 text-gray-600 dark:text-dark-300">
            {{ activeDocData.description }}
          </p>
        </div>

        <article class="card mt-8 overflow-hidden">
          <div class="card-header">
            <div class="flex flex-col gap-4 lg:flex-row lg:items-center lg:justify-between">
              <div>
                <h2 class="text-xl font-semibold text-gray-950 dark:text-white">{{ copy.script.title }}</h2>
                <p class="mt-1 text-sm text-gray-500 dark:text-dark-400">{{ activeDocData.modelHint }}</p>
              </div>
              <div class="tabs inline-flex w-fit max-w-full overflow-x-auto">
                <button
                  v-for="platform in platforms"
                  :key="platform.id"
                  type="button"
                  class="tab shrink-0"
                  :class="{ 'tab-active': activePlatform === platform.id }"
                  @click="activePlatform = platform.id"
                >
                  {{ platform.label }}
                </button>
              </div>
            </div>
          </div>

          <div class="card-body">
            <div class="overflow-hidden rounded-xl border border-gray-200 bg-gray-950 dark:border-dark-700">
              <div class="flex items-center justify-between border-b border-white/10 px-4 py-3">
                <span class="text-xs font-semibold uppercase tracking-wider text-primary-300">
                  {{ activePlatformLabel }}
                </span>
                <button
                  type="button"
                  class="inline-flex items-center gap-1.5 rounded-lg px-3 py-1.5 text-xs font-semibold text-gray-200 transition-colors hover:bg-white/10 hover:text-white"
                  @click="copyCommand"
                >
                  <Icon name="copy" size="xs" />
                  {{ copyButtonText }}
                </button>
              </div>
              <pre class="overflow-x-auto p-4 text-sm leading-7 text-gray-100"><code>{{ activeDocData.commands[activePlatform] }}</code></pre>
            </div>

            <div class="mt-5 rounded-xl border-l-4 border-primary-400 bg-primary-50 px-5 py-4 text-sm leading-7 text-gray-700 dark:bg-primary-900/20 dark:text-dark-200">
              <span class="font-semibold text-primary-700 dark:text-primary-300">{{ copy.script.baseUrl }}</span>
              <code class="rounded bg-white px-2 py-1 font-mono text-primary-700 dark:bg-dark-800 dark:text-primary-300">{{ activeDocData.baseUrl }}</code>
            </div>
          </div>
        </article>

        <section class="mt-8 grid gap-4 md:grid-cols-3">
          <article
            v-for="step in activeDocData.steps"
            :key="step.title"
            class="card p-6"
          >
            <div class="flex h-11 w-11 items-center justify-center rounded-xl bg-primary-100 text-primary-600 dark:bg-primary-900/30 dark:text-primary-300">
              <Icon :name="step.icon" size="md" />
            </div>
            <h2 class="mt-4 font-semibold text-gray-950 dark:text-white">{{ step.title }}</h2>
            <p class="mt-2 text-sm leading-7 text-gray-500 dark:text-dark-400">{{ step.description }}</p>
          </article>
        </section>

        <section class="mt-8 overflow-hidden rounded-2xl bg-gradient-primary p-8 text-white shadow-glow-lg">
          <div class="grid gap-5 md:grid-cols-[1fr_auto] md:items-center">
            <div>
              <h2 class="text-2xl font-bold tracking-tight">{{ copy.cta.title }}</h2>
              <p class="mt-3 max-w-2xl text-sm leading-7 text-primary-50">{{ copy.cta.description }}</p>
            </div>
            <router-link to="/dashboard" class="btn bg-white text-primary-700 hover:bg-primary-50">
              {{ copy.cta.action }}
              <Icon name="arrowRight" size="sm" />
            </router-link>
          </div>
        </section>
      </section>
    </main>

    <MarketingFooter />
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watchEffect } from 'vue'
import { useI18n } from 'vue-i18n'
import MarketingFooter from '@/components/marketing/MarketingFooter.vue'
import MarketingHeader from '@/components/marketing/MarketingHeader.vue'
import Icon from '@/components/icons/Icon.vue'
import { useAppStore } from '@/stores'

type DocId = 'claude' | 'openai'
type PlatformId = 'windows' | 'unix'

const { locale } = useI18n()
const appStore = useAppStore()
const activeDoc = ref<DocId>('openai')
const activePlatform = ref<PlatformId>('windows')
const copied = ref(false)

const docsCopy = {
  zh: {
    pageTitle: '接入文档',
    sidebar: {
      title: '接入平台'
    },
    hero: {
      badge: '接入文档'
    },
    script: {
      title: '配置命令',
      baseUrl: 'Base URL：'
    },
    cta: {
      title: '已经创建好 API Key？',
      description: '进入控制台创建或复制密钥，再按本页命令写入客户端配置。',
      action: '进入控制台'
    },
    copy: '复制',
    copied: '已复制',
    platforms: [
      { id: 'windows', label: 'Windows PowerShell' },
      { id: 'unix', label: 'macOS / Linux / WSL' }
    ],
    docs: {
      openai: {
        shortTitle: 'OpenAI (GPT)',
        title: 'OpenAI (GPT) 接入',
        description: '使用 OpenAI 兼容协议调用当前支持的 GPT 模型。',
        modelHint: '支持模型：gpt-5.5、gpt-5.4、gpt-5.3-codex',
        icon: 'cpu',
        baseUrl: 'https://your-domain.example/v1',
        commands: {
          windows: '$env:OPENAI_API_KEY="sk-..."\n$env:OPENAI_BASE_URL="https://your-domain.example/v1"\n# model: gpt-5.5 / gpt-5.4 / gpt-5.3-codex',
          unix: 'export OPENAI_API_KEY="sk-..."\nexport OPENAI_BASE_URL="https://your-domain.example/v1"\n# model: gpt-5.5 / gpt-5.4 / gpt-5.3-codex'
        },
        steps: [
          { icon: 'key', title: '创建密钥', description: '在控制台创建 API Key。' },
          { icon: 'link', title: '配置地址', description: '使用 OpenAI 兼容 Base URL。' },
          { icon: 'checkCircle', title: '选择模型', description: '从支持的 GPT 模型中选择一个模型 ID。' }
        ]
      },
      claude: {
        shortTitle: 'Claude',
        title: 'Claude 接入',
        description: '使用 Claude 兼容配置调用当前支持的 Claude 模型。',
        modelHint: '支持模型：claude-opus-4-7、claude-opus-4-6、claude-sonnet-4-6、claude-haiku-4-5',
        icon: 'beaker',
        baseUrl: 'https://your-domain.example',
        commands: {
          windows: '$env:ANTHROPIC_API_KEY="sk-..."\n$env:ANTHROPIC_BASE_URL="https://your-domain.example"\n# model: claude-opus-4-7 / claude-opus-4-6 / claude-sonnet-4-6 / claude-haiku-4-5',
          unix: 'export ANTHROPIC_API_KEY="sk-..."\nexport ANTHROPIC_BASE_URL="https://your-domain.example"\n# model: claude-opus-4-7 / claude-opus-4-6 / claude-sonnet-4-6 / claude-haiku-4-5'
        },
        steps: [
          { icon: 'key', title: '创建密钥', description: '在控制台创建 API Key。' },
          { icon: 'link', title: '配置地址', description: '使用 Claude 兼容 Base URL。' },
          { icon: 'checkCircle', title: '选择模型', description: '从支持的 Claude 模型中选择一个模型 ID。' }
        ]
      }
    }
  },
  en: {
    pageTitle: 'Docs',
    sidebar: {
      title: 'Platforms'
    },
    hero: {
      badge: 'Integration docs'
    },
    script: {
      title: 'Configuration command',
      baseUrl: 'Base URL: '
    },
    cta: {
      title: 'Already created an API key?',
      description: 'Open the dashboard to create or copy your key, then apply the command on this page.',
      action: 'Open dashboard'
    },
    copy: 'Copy',
    copied: 'Copied',
    platforms: [
      { id: 'windows', label: 'Windows PowerShell' },
      { id: 'unix', label: 'macOS / Linux / WSL' }
    ],
    docs: {
      openai: {
        shortTitle: 'OpenAI (GPT)',
        title: 'OpenAI (GPT) Integration',
        description: 'Use the OpenAI-compatible protocol to call supported GPT models.',
        modelHint: 'Supported models: gpt-5.5, gpt-5.4, gpt-5.3-codex',
        icon: 'cpu',
        baseUrl: 'https://your-domain.example/v1',
        commands: {
          windows: '$env:OPENAI_API_KEY="sk-..."\n$env:OPENAI_BASE_URL="https://your-domain.example/v1"\n# model: gpt-5.5 / gpt-5.4 / gpt-5.3-codex',
          unix: 'export OPENAI_API_KEY="sk-..."\nexport OPENAI_BASE_URL="https://your-domain.example/v1"\n# model: gpt-5.5 / gpt-5.4 / gpt-5.3-codex'
        },
        steps: [
          { icon: 'key', title: 'Create key', description: 'Create an API key in the dashboard.' },
          { icon: 'link', title: 'Set endpoint', description: 'Use the OpenAI-compatible Base URL.' },
          { icon: 'checkCircle', title: 'Choose model', description: 'Choose one supported GPT model ID.' }
        ]
      },
      claude: {
        shortTitle: 'Claude',
        title: 'Claude Integration',
        description: 'Use Claude-compatible settings to call supported Claude models.',
        modelHint: 'Supported models: claude-opus-4-7, claude-opus-4-6, claude-sonnet-4-6, claude-haiku-4-5',
        icon: 'beaker',
        baseUrl: 'https://your-domain.example',
        commands: {
          windows: '$env:ANTHROPIC_API_KEY="sk-..."\n$env:ANTHROPIC_BASE_URL="https://your-domain.example"\n# model: claude-opus-4-7 / claude-opus-4-6 / claude-sonnet-4-6 / claude-haiku-4-5',
          unix: 'export ANTHROPIC_API_KEY="sk-..."\nexport ANTHROPIC_BASE_URL="https://your-domain.example"\n# model: claude-opus-4-7 / claude-opus-4-6 / claude-sonnet-4-6 / claude-haiku-4-5'
        },
        steps: [
          { icon: 'key', title: 'Create key', description: 'Create an API key in the dashboard.' },
          { icon: 'link', title: 'Set endpoint', description: 'Use the Claude-compatible Base URL.' },
          { icon: 'checkCircle', title: 'Choose model', description: 'Choose one supported Claude model ID.' }
        ]
      }
    }
  }
} as const

const activeLocale = computed(() => locale.value === 'zh' ? 'zh' : 'en')
const copy = computed(() => docsCopy[activeLocale.value])
const docs = computed(() => [
  { id: 'openai' as const, ...copy.value.docs.openai },
  { id: 'claude' as const, ...copy.value.docs.claude }
])
const activeDocData = computed(() => copy.value.docs[activeDoc.value])
const platforms = computed(() => copy.value.platforms)
const activePlatformLabel = computed(() => platforms.value.find((item) => item.id === activePlatform.value)?.label || '')
const copyButtonText = computed(() => copied.value ? copy.value.copied : copy.value.copy)
const siteName = computed(() => appStore.cachedPublicSettings?.site_name || appStore.siteName || 'Sub2API')

watchEffect(() => {
  document.title = `${copy.value.pageTitle} - ${siteName.value}`
})

async function copyCommand(): Promise<void> {
  await navigator.clipboard?.writeText(activeDocData.value.commands[activePlatform.value])
  copied.value = true
  window.setTimeout(() => {
    copied.value = false
  }, 1500)
}
</script>
