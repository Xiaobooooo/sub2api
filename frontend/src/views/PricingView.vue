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
              </div>
              <div class="tabs inline-flex w-fit max-w-full overflow-x-auto">
                <button
                  v-for="platform in platforms"
                  :key="platform.id"
                  type="button"
                  class="tab flex shrink-0 items-center gap-2"
                  :class="{ 'tab-active': activePlatform === platform.id }"
                  @click="activePlatform = platform.id"
                >
                  <img :src="platform.logo" :alt="`${platform.label} logo`" class="h-4 w-4 object-contain" />
                  {{ platform.label }}
                </button>
              </div>
            </div>
          </div>

          <div class="card-body">
            <div class="rounded-xl border border-primary-100 bg-primary-50 p-4 dark:border-primary-800/50 dark:bg-primary-900/20">
              <div class="flex items-start gap-3">
                <div class="mt-0.5 flex h-9 w-9 items-center justify-center rounded-xl bg-white text-primary-600 shadow-sm dark:bg-dark-800 dark:text-primary-300">
                  <Icon name="calculator" size="sm" />
                </div>
                <div>
                  <p class="font-semibold text-gray-950 dark:text-white">{{ copy.pricingRule.title }}</p>
                  <p class="mt-1 whitespace-pre-line text-sm leading-6 text-gray-600 dark:text-dark-300">
                    {{ pricingRuleDescription }}
                  </p>
                </div>
              </div>
            </div>

            <div class="mt-5 rounded-xl border border-gray-200 bg-white p-4 dark:border-dark-700 dark:bg-dark-800/60">
              <div class="flex items-start gap-4">
                <div class="flex h-11 w-11 flex-shrink-0 items-center justify-center rounded-xl bg-primary-50 text-primary-600 dark:bg-primary-900/30 dark:text-primary-300">
                  <Icon name="badge" size="sm" />
                </div>
                <div class="min-w-0 flex-1">
                  <p class="text-xs font-semibold text-gray-500 dark:text-dark-400">{{ copy.groupSelector.title }}</p>
                  <div class="mt-3 grid gap-3 md:grid-cols-2">
                    <button
                      v-for="group in activePlatformData.groups"
                      :key="group.id"
                      type="button"
                      class="rounded-xl border p-4 text-left transition"
                      :class="group.id === activeGroupData.id
                        ? 'border-primary-300 bg-primary-50 shadow-sm dark:border-primary-700/70 dark:bg-primary-900/20'
                        : 'border-gray-200 bg-gray-50 hover:border-primary-200 hover:bg-white dark:border-dark-700 dark:bg-dark-900/40 dark:hover:border-primary-700/60 dark:hover:bg-dark-800/70'"
                      @click="selectedGroups[activePlatform] = group.id"
                    >
                      <span class="block text-base font-semibold text-gray-950 dark:text-white">{{ group.name }}</span>
                      <span class="mt-2 block whitespace-pre-line text-sm leading-6 text-gray-600 dark:text-dark-300">
                        {{ group.description }}
                      </span>
                    </button>
                  </div>
                </div>
              </div>
            </div>

            <div class="mt-6 grid gap-4">
              <div class="table-container">
                <table class="table">
                  <thead>
                    <tr>
                      <th>{{ copy.table.model }}</th>
                      <th>{{ copy.table.input }}</th>
                      <th>{{ copy.table.output }}</th>
                      <th>{{ copy.table.cacheWrite }}</th>
                      <th>{{ copy.table.cacheRead }}</th>
                      <th>{{ copy.table.saving }}</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="model in activePlatformData.models" :key="model.id">
                      <td>
                        <div class="flex items-center gap-2">
                          <span class="font-mono font-semibold text-gray-950 dark:text-white">{{ model.id }}</span>
                          <button
                            type="button"
                            class="rounded-md p-1 text-gray-400 transition-colors hover:bg-gray-100 hover:text-primary-600 dark:hover:bg-dark-700"
                            :title="copy.table.copyModel"
                            @click="copyModelId(model.id)"
                          >
                            <Icon name="copy" size="xs" />
                          </button>
                        </div>
                      </td>
                      <td><PriceCell :price="model.input" :multiplier="activeGroupData.multiplier" /></td>
                      <td><PriceCell :price="model.output" :multiplier="activeGroupData.multiplier" /></td>
                      <td><PriceCell :price="model.cacheWrite" :multiplier="activeGroupData.multiplier" /></td>
                      <td><PriceCell :price="model.cacheRead" :multiplier="activeGroupData.multiplier" /></td>
                      <td><span class="badge badge-success">{{ activeGroupData.saving }}</span></td>
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
import { computed, defineComponent, h, ref, watchEffect, type PropType } from 'vue'
import { useI18n } from 'vue-i18n'
import MarketingFooter from '@/components/marketing/MarketingFooter.vue'
import MarketingHeader from '@/components/marketing/MarketingHeader.vue'
import Icon from '@/components/icons/Icon.vue'
import { useAppStore } from '@/stores'

type PlatformId = 'claude' | 'openai'
type ModelPrice = {
  id: string
  input: number
  output: number
  cacheWrite: number | null
  cacheRead: number
}
type PriceGroup = {
  id: string
  name: string
  description: string
  multiplier: number
  saving: string
}

const EXCHANGE_RATE = 7

const { locale } = useI18n()
const appStore = useAppStore()
const activePlatform = ref<PlatformId>('claude')
const selectedGroups = ref<Record<PlatformId, string>>({
  claude: 'claude-official',
  openai: 'openai-official'
})

const pricingCopy = {
  zh: {
    pageTitle: '模型价格',
    hero: {
      badge: '支持平台',
      title: '模型价格',
      description: '按美元余额计费',
      notes: [
        { label: '支持平台', value: 'Claude / OpenAI' },
        { label: '充值比例', value: '充值 1RMB = 1 USD' },
        { label: '余额单位', value: 'USD' }
      ]
    },
    platforms: {
      title: '选择平台',
    },
    pricingRule: {
      title: '计价规则',
      formula: '分组价格 = 官方价格 × 分组倍率 ÷ 7'
    },
    groupSelector: {
      title: '分组选择'
    },
    table: {
      model: '模型 ID',
      input: '输入价格',
      output: '输出价格',
      cacheWrite: '缓存创建',
      cacheRead: '缓存读取',
      saving: '节省幅度',
      officialPrefix: '官方参考',
      balanceSuffix: '美元余额 / 1M Tokens',
      notAvailable: '不适用',
      copyModel: '复制模型 ID'
    },
    data: {
      claude: {
        label: 'Claude',
        logo: '/claude.svg',
        description: '当前 Claude 平台支持以下模型。',
        groups: [
          {
            id: 'claude-official',
            name: 'Claude Official',
            description: '2x 倍率 · 相当于约 2.9 折\n官方满血订阅，只支持 Claude Code、Claude Desktop，不支持 OpenClaw、Hermes 等',
            multiplier: 2,
            saving: '省 71%'
          }
        ] satisfies PriceGroup[],
        models: [
          { id: 'claude-opus-4-7', input: 35, output: 175, cacheWrite: 43.75, cacheRead: 3.5 },
          { id: 'claude-opus-4-6', input: 35, output: 175, cacheWrite: 43.75, cacheRead: 3.5 },
          { id: 'claude-sonnet-4-6', input: 21, output: 105, cacheWrite: 26.25, cacheRead: 2.1 },
          { id: 'claude-haiku-4-5', input: 7, output: 35, cacheWrite: 8.75, cacheRead: 0.7 }
        ] satisfies ModelPrice[]
      },
      openai: {
        label: 'OpenAI (GPT)',
        logo: '/openai.svg',
        description: '当前 OpenAI 平台支持以下 GPT 模型。',
        groups: [
          {
            id: 'openai-official',
            name: 'OpenaiOfficial',
            description: '0.6x 倍率 · 相当于约 8.5 折\n官方直连，GPT 5.5 可平替 Opus 4.7，推荐用于编码和养虾',
            multiplier: 0.6,
            saving: '约 8.5 折'
          }
        ] satisfies PriceGroup[],
        models: [
          { id: 'gpt-5.5', input: 35, output: 210, cacheWrite: null, cacheRead: 3.5 },
          { id: 'gpt-5.4', input: 17.5, output: 105, cacheWrite: null, cacheRead: 1.75 },
          { id: 'gpt-5.3-codex', input: 12.25, output: 98, cacheWrite: null, cacheRead: 1.23 }
        ] satisfies ModelPrice[]
      }
    }
  },
  en: {
    pageTitle: 'Pricing',
    hero: {
      badge: 'Supported platforms',
      title: 'Model Pricing',
      description: 'Usage is billed against USD balance.',
      notes: [
        { label: 'Platforms', value: 'Claude / OpenAI' },
        { label: 'Top-up ratio', value: '1 RMB top-up = 1 USD' },
        { label: 'Balance unit', value: 'USD' }
      ]
    },
    platforms: {
      title: 'Choose platform',
    },
    pricingRule: {
      title: 'Pricing rule',
      formula: 'Group price = official price × group multiplier ÷ 7'
    },
    groupSelector: {
      title: 'Group selection'
    },
    table: {
      model: 'Model ID',
      input: 'Input',
      output: 'Output',
      cacheWrite: 'Cache write',
      cacheRead: 'Cache read',
      saving: 'Savings',
      officialPrefix: 'Official ref.',
      balanceSuffix: 'USD balance / 1M Tokens',
      notAvailable: 'N/A',
      copyModel: 'Copy model ID'
    },
    data: {
      claude: {
        label: 'Claude',
        logo: '/claude.svg',
        description: 'The Claude platform currently supports these models.',
        groups: [
          {
            id: 'claude-official',
            name: 'Claude Official',
            description: '2x multiplier · about 29% of official price\nOfficial full-powered subscription. Claude Code and Claude Desktop only; OpenClaw, Hermes, and similar clients are not supported.',
            multiplier: 2,
            saving: 'Save 71%'
          }
        ] satisfies PriceGroup[],
        models: [
          { id: 'claude-opus-4-7', input: 35, output: 175, cacheWrite: 43.75, cacheRead: 3.5 },
          { id: 'claude-opus-4-6', input: 35, output: 175, cacheWrite: 43.75, cacheRead: 3.5 },
          { id: 'claude-sonnet-4-6', input: 21, output: 105, cacheWrite: 26.25, cacheRead: 2.1 },
          { id: 'claude-haiku-4-5', input: 7, output: 35, cacheWrite: 8.75, cacheRead: 0.7 }
        ] satisfies ModelPrice[]
      },
      openai: {
        label: 'OpenAI (GPT)',
        logo: '/openai.svg',
        description: 'The OpenAI platform currently supports these GPT models.',
        groups: [
          {
            id: 'openai-official',
            name: 'OpenaiOfficial',
            description: '0.6x multiplier · about 85% of official price\nOfficial direct access. GPT 5.5 can replace Opus 4.7 and is recommended for coding and high-volume agent work.',
            multiplier: 0.6,
            saving: 'About 85%'
          }
        ] satisfies PriceGroup[],
        models: [
          { id: 'gpt-5.5', input: 35, output: 210, cacheWrite: null, cacheRead: 3.5 },
          { id: 'gpt-5.4', input: 17.5, output: 105, cacheWrite: null, cacheRead: 1.75 },
          { id: 'gpt-5.3-codex', input: 12.25, output: 98, cacheWrite: null, cacheRead: 1.23 }
        ] satisfies ModelPrice[]
      }
    }
  }
} as const

const activeLocale = computed(() => locale.value === 'zh' ? 'zh' : 'en')
const copy = computed(() => pricingCopy[activeLocale.value])
const PriceCell = defineComponent({
  name: 'PriceCell',
  props: {
    price: {
      type: Number as PropType<number | null>,
      default: null
    },
    multiplier: {
      type: Number,
      required: true
    }
  },
  setup(props) {
    return () => {
      if (props.price === null) {
        return h('span', { class: 'text-sm text-gray-400 dark:text-dark-500' }, copy.value.table.notAvailable)
      }

      const groupPrice = (props.price * props.multiplier) / EXCHANGE_RATE

      return h('div', { class: 'min-w-[140px]' }, [
        h('p', { class: 'text-sm font-semibold text-gray-950 dark:text-white' }, [
          h('span', { class: 'text-base font-bold tabular-nums text-primary-600 dark:text-primary-300' }, groupPrice.toFixed(2)),
          h('span', { class: 'ml-1' }, copy.value.table.balanceSuffix)
        ]),
        h('p', { class: 'mt-1 text-xs text-gray-500 dark:text-dark-400' }, `${copy.value.table.officialPrefix} ￥${props.price.toFixed(2)}`)
      ])
    }
  }
})
const platforms = computed(() => [
  { id: 'claude' as const, ...copy.value.data.claude },
  { id: 'openai' as const, ...copy.value.data.openai }
])
const activePlatformData = computed(() => copy.value.data[activePlatform.value])
const activeGroupData = computed(() => {
  const selectedGroupId = selectedGroups.value[activePlatform.value]
  return activePlatformData.value.groups.find((group) => group.id === selectedGroupId) ?? activePlatformData.value.groups[0]
})
const pricingRuleDescription = computed(() => {
  const model = activePlatformData.value.models[0]
  const groupPrice = formatGroupPrice(model.input, activeGroupData.value.multiplier)

  if (activeLocale.value === 'zh') {
    return `${copy.value.pricingRule.formula}\n示例：${model.id} 输入价，官方 ￥${model.input.toFixed(2)}，${activeGroupData.value.name} ${groupPrice} ${copy.value.table.balanceSuffix}`
  }

  return `${copy.value.pricingRule.formula}\nExample: ${model.id} input price, official ￥${model.input.toFixed(2)}, ${activeGroupData.value.name} ${groupPrice} ${copy.value.table.balanceSuffix}`
})
const siteName = computed(() => appStore.cachedPublicSettings?.site_name || appStore.siteName || 'Sub2API')

watchEffect(() => {
  document.title = `${copy.value.pageTitle} - ${siteName.value}`
})

async function copyModelId(id: string): Promise<void> {
  await navigator.clipboard?.writeText(id)
}

function formatGroupPrice(price: number, multiplier: number): string {
  return ((price * multiplier) / EXCHANGE_RATE).toFixed(2)
}
</script>
