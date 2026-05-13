<template>
  <div class="min-h-screen bg-gray-50 text-gray-900 dark:bg-dark-950 dark:text-gray-100">
    <div class="pointer-events-none fixed inset-0 bg-mesh-gradient"></div>
    <MarketingHeader />

    <main class="relative mx-auto max-w-7xl px-4 py-12 sm:px-6 lg:px-8 lg:py-16">
      <section class="grid gap-8 lg:grid-cols-[0.9fr_1.1fr] lg:items-end">
        <div>
          <div class="badge badge-primary w-fit">
            <Icon name="dollar" size="xs" />
            {{ copy.hero.badge }}
          </div>
          <h1 class="mt-5 text-4xl font-bold tracking-tight text-gray-950 dark:text-white sm:text-5xl">
            {{ copy.hero.title }}
          </h1>
          <p class="mt-4 max-w-2xl text-base leading-8 text-gray-600 dark:text-dark-300">
            {{ copy.hero.description }}
          </p>
        </div>

        <div class="card p-5">
          <div class="grid gap-3 sm:grid-cols-3">
            <div
              v-for="item in copy.hero.notes"
              :key="item.label"
              class="rounded-xl bg-gray-50 p-4 dark:bg-dark-900/70"
            >
              <p class="text-xs text-gray-500 dark:text-dark-400">{{ item.label }}</p>
              <p class="mt-2 text-sm font-semibold text-gray-950 dark:text-white">{{ item.value }}</p>
            </div>
          </div>
        </div>
      </section>

      <section class="mt-10">
        <div class="card overflow-hidden">
          <div class="card-header">
            <div class="flex flex-col gap-4 lg:flex-row lg:items-center lg:justify-between">
              <div>
                <h2 class="text-xl font-semibold text-gray-950 dark:text-white">{{ copy.platforms.title }}</h2>
                <p class="mt-1 text-sm text-gray-500 dark:text-dark-400">{{ copy.platforms.description }}</p>
              </div>
              <div class="tabs w-full overflow-x-auto lg:w-auto">
                <button
                  v-for="platform in platforms"
                  :key="platform.id"
                  type="button"
                  class="tab flex shrink-0 items-center gap-2"
                  :class="{ 'tab-active': activePlatform === platform.id }"
                  @click="activePlatform = platform.id"
                >
                  <Icon :name="platform.icon" size="sm" />
                  {{ platform.label }}
                </button>
              </div>
            </div>
          </div>

          <div class="card-body">
            <div class="rounded-xl border border-primary-100 bg-primary-50 p-4 dark:border-primary-800/50 dark:bg-primary-900/20">
              <div class="flex items-start gap-3">
                <div class="mt-0.5 flex h-9 w-9 items-center justify-center rounded-xl bg-white text-primary-600 shadow-sm dark:bg-dark-800 dark:text-primary-300">
                  <Icon name="creditCard" size="sm" />
                </div>
                <div>
                  <p class="font-semibold text-gray-950 dark:text-white">{{ copy.recharge.title }}</p>
                  <p class="mt-1 text-sm leading-6 text-gray-600 dark:text-dark-300">{{ copy.recharge.description }}</p>
                </div>
              </div>
            </div>

            <div class="mt-6 grid gap-4 lg:grid-cols-[320px_1fr]">
              <div class="rounded-2xl border border-gray-200 bg-white p-5 dark:border-dark-700 dark:bg-dark-800/60">
                <div class="flex h-12 w-12 items-center justify-center rounded-xl bg-primary-100 text-primary-600 dark:bg-primary-900/30 dark:text-primary-300">
                  <Icon :name="activePlatformData.icon" size="lg" />
                </div>
                <h3 class="mt-4 text-lg font-semibold text-gray-950 dark:text-white">{{ activePlatformData.label }}</h3>
                <p class="mt-2 text-sm leading-7 text-gray-500 dark:text-dark-400">{{ activePlatformData.description }}</p>
              </div>

              <div class="table-container">
                <table class="table">
                  <thead>
                    <tr>
                      <th>{{ copy.table.model }}</th>
                      <th>{{ copy.table.platform }}</th>
                      <th>{{ copy.table.balanceUnit }}</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="model in activePlatformData.models" :key="model">
                      <td>
                        <div class="flex items-center gap-2">
                          <span class="font-mono font-semibold text-gray-950 dark:text-white">{{ model }}</span>
                          <button
                            type="button"
                            class="rounded-md p-1 text-gray-400 transition-colors hover:bg-gray-100 hover:text-primary-600 dark:hover:bg-dark-700"
                            :title="copy.table.copyModel"
                            @click="copyModelId(model)"
                          >
                            <Icon name="copy" size="xs" />
                          </button>
                        </div>
                      </td>
                      <td>{{ activePlatformData.label }}</td>
                      <td>{{ copy.table.usdBalance }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
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

type PlatformId = 'claude' | 'openai'

const { locale } = useI18n()
const appStore = useAppStore()
const activePlatform = ref<PlatformId>('claude')

const pricingCopy = {
  zh: {
    pageTitle: '模型价格',
    hero: {
      badge: '支持平台',
      title: '模型价格',
      description: '当前仅支持 Claude 和 OpenAI（GPT）两个平台。充值后按美元余额计费。',
      notes: [
        { label: '支持平台', value: 'Claude / OpenAI' },
        { label: '充值比例', value: '充值 1 人民币 = 1 美元余额' },
        { label: '余额单位', value: '美元余额' }
      ]
    },
    platforms: {
      title: '选择平台',
      description: '查看当前可用平台和模型 ID。'
    },
    recharge: {
      title: '充值比例',
      description: '充值 1 人民币可得到 1 美元余额，模型调用消耗美元余额。'
    },
    table: {
      model: '模型 ID',
      platform: '平台',
      balanceUnit: '计费余额',
      usdBalance: '美元余额',
      copyModel: '复制模型 ID'
    },
    data: {
      claude: {
        label: 'Claude',
        icon: 'beaker',
        description: '当前 Claude 平台支持以下模型。',
        models: ['claude-opus-4-7', 'claude-opus-4-6', 'claude-sonnet-4-6', 'claude-haiku-4-5']
      },
      openai: {
        label: 'OpenAI (GPT)',
        icon: 'cpu',
        description: '当前 OpenAI 平台支持以下 GPT 模型。',
        models: ['gpt-5.5', 'gpt-5.4', 'gpt-5.3-codex']
      }
    }
  },
  en: {
    pageTitle: 'Pricing',
    hero: {
      badge: 'Supported platforms',
      title: 'Model Pricing',
      description: 'Only Claude and OpenAI (GPT) are currently supported. Usage is billed against USD balance after top-up.',
      notes: [
        { label: 'Platforms', value: 'Claude / OpenAI' },
        { label: 'Top-up ratio', value: '1 RMB top-up = 1 USD balance' },
        { label: 'Balance unit', value: 'USD balance' }
      ]
    },
    platforms: {
      title: 'Choose platform',
      description: 'View currently available platforms and model IDs.'
    },
    recharge: {
      title: 'Top-up ratio',
      description: 'A 1 RMB top-up gives 1 USD balance. Model usage consumes USD balance.'
    },
    table: {
      model: 'Model ID',
      platform: 'Platform',
      balanceUnit: 'Billing balance',
      usdBalance: 'USD balance',
      copyModel: 'Copy model ID'
    },
    data: {
      claude: {
        label: 'Claude',
        icon: 'beaker',
        description: 'The Claude platform currently supports these models.',
        models: ['claude-opus-4-7', 'claude-opus-4-6', 'claude-sonnet-4-6', 'claude-haiku-4-5']
      },
      openai: {
        label: 'OpenAI (GPT)',
        icon: 'cpu',
        description: 'The OpenAI platform currently supports these GPT models.',
        models: ['gpt-5.5', 'gpt-5.4', 'gpt-5.3-codex']
      }
    }
  }
} as const

const activeLocale = computed(() => locale.value === 'zh' ? 'zh' : 'en')
const copy = computed(() => pricingCopy[activeLocale.value])
const platforms = computed(() => [
  { id: 'claude' as const, ...copy.value.data.claude },
  { id: 'openai' as const, ...copy.value.data.openai }
])
const activePlatformData = computed(() => copy.value.data[activePlatform.value])
const siteName = computed(() => appStore.cachedPublicSettings?.site_name || appStore.siteName || 'Sub2API')

watchEffect(() => {
  document.title = `${copy.value.pageTitle} - ${siteName.value}`
})

async function copyModelId(id: string): Promise<void> {
  await navigator.clipboard?.writeText(id)
}
</script>
