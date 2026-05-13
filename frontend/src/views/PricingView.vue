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
                <h2 class="text-xl font-semibold text-gray-950 dark:text-white">{{ copy.selector.title }}</h2>
                <p class="mt-1 text-sm text-gray-500 dark:text-dark-400">{{ copy.selector.description }}</p>
              </div>
              <div class="tabs w-full overflow-x-auto lg:w-auto">
                <button
                  v-for="scenario in scenarios"
                  :key="scenario.id"
                  type="button"
                  class="tab flex shrink-0 items-center gap-2"
                  :class="{ 'tab-active': activeScenario === scenario.id }"
                  @click="activeScenario = scenario.id"
                >
                  <Icon :name="scenario.icon" size="sm" />
                  {{ scenario.label }}
                </button>
              </div>
            </div>
          </div>

          <div class="card-body">
            <div class="rounded-xl border border-primary-100 bg-primary-50 p-4 dark:border-primary-800/50 dark:bg-primary-900/20">
              <div class="flex flex-col gap-3 sm:flex-row sm:items-center sm:justify-between">
                <div class="flex items-start gap-3">
                  <div class="mt-0.5 flex h-9 w-9 items-center justify-center rounded-xl bg-white text-primary-600 shadow-sm dark:bg-dark-800 dark:text-primary-300">
                    <Icon name="calculator" size="sm" />
                  </div>
                  <div>
                    <p class="font-semibold text-gray-950 dark:text-white">{{ copy.rule.title }}</p>
                    <p class="mt-1 text-sm leading-6 text-gray-600 dark:text-dark-300">{{ activeScenarioData.rule }}</p>
                  </div>
                </div>
                <span class="badge badge-primary">{{ activeScenarioData.example }}</span>
              </div>
            </div>

            <div class="mt-6 grid gap-4 md:grid-cols-3">
              <button
                v-for="group in activeScenarioData.groups"
                :key="group.name"
                type="button"
                class="rounded-2xl border p-5 text-left transition-all"
                :class="selectedGroup === group.name
                  ? 'border-primary-300 bg-primary-50 shadow-glow dark:border-primary-700 dark:bg-primary-900/20'
                  : 'border-gray-200 bg-white hover:border-primary-200 hover:bg-primary-50/50 dark:border-dark-700 dark:bg-dark-800/60 dark:hover:border-primary-800 dark:hover:bg-primary-900/10'"
                @click="selectedGroup = group.name"
              >
                <div class="flex items-center justify-between gap-3">
                  <h3 class="font-semibold text-gray-950 dark:text-white">{{ group.name }}</h3>
                  <span class="badge badge-gray">{{ group.discount }}</span>
                </div>
                <p class="mt-3 text-sm leading-6 text-gray-500 dark:text-dark-400">{{ group.description }}</p>
              </button>
            </div>

            <div class="mt-6 flex flex-col gap-4 sm:flex-row sm:items-center sm:justify-between">
              <div>
                <h3 class="text-lg font-semibold text-gray-950 dark:text-white">{{ copy.table.title }}</h3>
                <p class="mt-1 text-sm text-gray-500 dark:text-dark-400">{{ copy.table.description }}</p>
              </div>
              <div class="tabs w-fit">
                <button
                  type="button"
                  class="tab"
                  :class="{ 'tab-active': priceMode === 'group' }"
                  @click="priceMode = 'group'"
                >
                  {{ copy.table.groupPrice }}
                </button>
                <button
                  type="button"
                  class="tab"
                  :class="{ 'tab-active': priceMode === 'official' }"
                  @click="priceMode = 'official'"
                >
                  {{ copy.table.officialPrice }}
                </button>
              </div>
            </div>

            <div class="table-container mt-4">
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
                  <tr v-for="model in activeScenarioData.models" :key="model.id">
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
                    <td>{{ formatPrice(model.input) }}</td>
                    <td>{{ formatPrice(model.output) }}</td>
                    <td>{{ formatPrice(model.cacheWrite) }}</td>
                    <td>{{ formatPrice(model.cacheRead) }}</td>
                    <td><span class="badge badge-success">{{ model.saving }}</span></td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </section>

      <section class="mt-10 grid gap-4 md:grid-cols-3">
        <article
          v-for="item in copy.notes"
          :key="item.title"
          class="card p-6"
        >
          <div class="flex h-11 w-11 items-center justify-center rounded-xl bg-primary-100 text-primary-600 dark:bg-primary-900/30 dark:text-primary-300">
            <Icon :name="item.icon" size="md" />
          </div>
          <h2 class="mt-4 font-semibold text-gray-950 dark:text-white">{{ item.title }}</h2>
          <p class="mt-2 text-sm leading-7 text-gray-500 dark:text-dark-400">{{ item.description }}</p>
        </article>
      </section>
    </main>

    <MarketingFooter />
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watch, watchEffect } from 'vue'
import { useI18n } from 'vue-i18n'
import MarketingFooter from '@/components/marketing/MarketingFooter.vue'
import MarketingHeader from '@/components/marketing/MarketingHeader.vue'
import Icon from '@/components/icons/Icon.vue'
import { useAppStore } from '@/stores'

type ScenarioId = 'claude' | 'codex' | 'gemini'
type PriceMode = 'group' | 'official'

const { locale } = useI18n()
const appStore = useAppStore()
const activeScenario = ref<ScenarioId>('claude')
const selectedGroup = ref('')
const priceMode = ref<PriceMode>('group')

const pricingCopy = {
  zh: {
    pageTitle: '模型价格',
    hero: {
      badge: '价格对照',
      title: '模型价格',
      description: '参考 APIKEY.FUN 的价格页内容结构：按使用场景切换、展示计价规则、分组价格和官方价格对照；视觉样式统一使用当前项目的卡片、表格和主色体系。',
      notes: [
        { label: '展示货币', value: '人民币（¥）' },
        { label: '对照方式', value: '官方价 / 分组价' },
        { label: '适用场景', value: 'Claude Code、Codex、Gemini' }
      ]
    },
    selector: {
      title: '选择使用场景',
      description: '不同客户端通常对应不同模型组合和价格分组。'
    },
    rule: {
      title: '计算规则'
    },
    table: {
      title: '价格列表',
      description: '选择分组后，直接查看每个模型的人民币价格。',
      groupPrice: '分组价格',
      officialPrice: '官方价格',
      model: '模型 ID',
      input: '输入价格',
      output: '输出价格',
      cacheWrite: '缓存创建',
      cacheRead: '缓存读取',
      saving: '节省幅度',
      copyModel: '复制模型 ID'
    },
    copied: '已复制',
    notes: [
      { icon: 'calculator', title: '价格只是展示结构', description: '公开页用于说明价格对照方式，实际金额以后台渠道、分组和倍率配置为准。' },
      { icon: 'grid', title: '按场景组织', description: '延续 APIKEY 的场景标签布局，让用户先选客户端，再看对应模型价格。' },
      { icon: 'shield', title: '保留安全边界', description: '公开价格页不暴露密钥、账号池或后台配置，只展示可公开的说明信息。' }
    ],
    scenarios: {
      claude: {
        label: 'Claude Code',
        icon: 'beaker',
        rule: '官方价格按 $1 = ¥7 折算，分组价格 = 官方价格 × 分组倍率 ÷ 7。',
        example: '示例：Sonnet 输入 ¥21.00，Pro 分组 ¥4.20',
        groups: [
          { name: 'Claude Pro', discount: '2 折', description: '日常代码生成与编辑任务。' },
          { name: 'Claude Max', discount: '3 折', description: '长上下文、复杂项目和多文件修改。' },
          { name: 'Team Pool', discount: '4 折', description: '团队共享额度和统一密钥管理。' }
        ],
        models: [
          { id: 'claude-sonnet-4.5', input: 4.2, output: 21, cacheWrite: 5.25, cacheRead: 0.42, saving: '省 80%' },
          { id: 'claude-opus-4.1', input: 10.5, output: 52.5, cacheWrite: 13.13, cacheRead: 1.05, saving: '省 80%' },
          { id: 'claude-haiku-4.5', input: 0.56, output: 2.8, cacheWrite: 0.7, cacheRead: 0.06, saving: '省 80%' }
        ]
      },
      codex: {
        label: 'Codex',
        icon: 'cpu',
        rule: 'Codex 场景按代码代理任务分组，分组倍率越低，适合越稳定的批量使用。',
        example: '示例：gpt-5.2 输入 ¥8.40，Pro 分组 ¥1.68',
        groups: [
          { name: 'Codex Pro', discount: '2 折', description: '面向日常编码代理任务。' },
          { name: 'Codex Fast', discount: '3 折', description: '优先低延迟响应。' },
          { name: 'Codex Team', discount: '4 折', description: '适合团队共享额度。' }
        ],
        models: [
          { id: 'gpt-5.2', input: 1.68, output: 13.44, cacheWrite: 2.1, cacheRead: 0.17, saving: '省 80%' },
          { id: 'gpt-5.2-codex', input: 2.1, output: 16.8, cacheWrite: 2.63, cacheRead: 0.21, saving: '省 80%' },
          { id: 'gpt-5.1-mini', input: 0.42, output: 3.36, cacheWrite: 0.53, cacheRead: 0.04, saving: '省 80%' }
        ]
      },
      gemini: {
        label: 'Gemini',
        icon: 'sparkles',
        rule: 'Gemini 场景用于命令行和文件分析任务，价格按模型输入输出分别展示。',
        example: '示例：Gemini Pro 输入 ¥10.50，Pro 分组 ¥2.10',
        groups: [
          { name: 'Gemini Flash', discount: '2 折', description: '轻量任务与快速响应。' },
          { name: 'Gemini Pro', discount: '3 折', description: '复杂推理与多模态分析。' },
          { name: 'Gemini Team', discount: '4 折', description: '团队共享与统一预算。' }
        ],
        models: [
          { id: 'gemini-2.5-pro', input: 2.1, output: 12.6, cacheWrite: 2.63, cacheRead: 0.21, saving: '省 80%' },
          { id: 'gemini-2.5-flash', input: 0.21, output: 1.68, cacheWrite: 0.26, cacheRead: 0.02, saving: '省 80%' },
          { id: 'gemini-2.0-flash', input: 0.14, output: 1.12, cacheWrite: 0.18, cacheRead: 0.01, saving: '省 80%' }
        ]
      }
    }
  },
  en: {
    pageTitle: 'Pricing',
    hero: {
      badge: 'Price comparison',
      title: 'Model Pricing',
      description: 'The page follows APIKEY.FUN’s pricing structure: scenario tabs, calculation rules, group pricing, and official price comparison. Styling uses this project’s cards, tables, and primary color system.',
      notes: [
        { label: 'Currency', value: 'CNY (¥)' },
        { label: 'Comparison', value: 'Official / Group' },
        { label: 'Scenarios', value: 'Claude Code, Codex, Gemini' }
      ]
    },
    selector: {
      title: 'Choose a scenario',
      description: 'Different clients usually map to different model sets and pricing groups.'
    },
    rule: {
      title: 'Calculation rule'
    },
    table: {
      title: 'Price list',
      description: 'Select a group and review the CNY price for each model.',
      groupPrice: 'Group price',
      officialPrice: 'Official price',
      model: 'Model ID',
      input: 'Input',
      output: 'Output',
      cacheWrite: 'Cache write',
      cacheRead: 'Cache read',
      saving: 'Savings',
      copyModel: 'Copy model ID'
    },
    copied: 'Copied',
    notes: [
      { icon: 'calculator', title: 'Structure first', description: 'The public page explains the comparison model. Final prices follow backend channels, groups, and multipliers.' },
      { icon: 'grid', title: 'Scenario-based', description: 'Following APIKEY’s layout, users choose a client scenario before reading model prices.' },
      { icon: 'shield', title: 'Safe public surface', description: 'The page exposes no keys, account pools, or backend configuration.' }
    ],
    scenarios: {
      claude: {
        label: 'Claude Code',
        icon: 'beaker',
        rule: 'Official price is converted at $1 = ¥7. Group price = official price × group multiplier ÷ 7.',
        example: 'Example: Sonnet input ¥21.00, Pro group ¥4.20',
        groups: [
          { name: 'Claude Pro', discount: '20%', description: 'Daily code generation and editing tasks.' },
          { name: 'Claude Max', discount: '30%', description: 'Long-context, complex projects, and multi-file edits.' },
          { name: 'Team Pool', discount: '40%', description: 'Shared team quota and unified key management.' }
        ],
        models: [
          { id: 'claude-sonnet-4.5', input: 4.2, output: 21, cacheWrite: 5.25, cacheRead: 0.42, saving: '80%' },
          { id: 'claude-opus-4.1', input: 10.5, output: 52.5, cacheWrite: 13.13, cacheRead: 1.05, saving: '80%' },
          { id: 'claude-haiku-4.5', input: 0.56, output: 2.8, cacheWrite: 0.7, cacheRead: 0.06, saving: '80%' }
        ]
      },
      codex: {
        label: 'Codex',
        icon: 'cpu',
        rule: 'Codex pricing is grouped for coding-agent tasks. Lower multipliers fit higher-volume usage.',
        example: 'Example: gpt-5.2 input ¥8.40, Pro group ¥1.68',
        groups: [
          { name: 'Codex Pro', discount: '20%', description: 'Daily coding-agent tasks.' },
          { name: 'Codex Fast', discount: '30%', description: 'Prioritized low-latency responses.' },
          { name: 'Codex Team', discount: '40%', description: 'Shared team usage and quota.' }
        ],
        models: [
          { id: 'gpt-5.2', input: 1.68, output: 13.44, cacheWrite: 2.1, cacheRead: 0.17, saving: '80%' },
          { id: 'gpt-5.2-codex', input: 2.1, output: 16.8, cacheWrite: 2.63, cacheRead: 0.21, saving: '80%' },
          { id: 'gpt-5.1-mini', input: 0.42, output: 3.36, cacheWrite: 0.53, cacheRead: 0.04, saving: '80%' }
        ]
      },
      gemini: {
        label: 'Gemini',
        icon: 'sparkles',
        rule: 'Gemini scenarios cover CLI and file-analysis tasks, with separate input and output prices.',
        example: 'Example: Gemini Pro input ¥10.50, Pro group ¥2.10',
        groups: [
          { name: 'Gemini Flash', discount: '20%', description: 'Lightweight tasks and quick responses.' },
          { name: 'Gemini Pro', discount: '30%', description: 'Complex reasoning and multimodal analysis.' },
          { name: 'Gemini Team', discount: '40%', description: 'Team sharing and unified budget.' }
        ],
        models: [
          { id: 'gemini-2.5-pro', input: 2.1, output: 12.6, cacheWrite: 2.63, cacheRead: 0.21, saving: '80%' },
          { id: 'gemini-2.5-flash', input: 0.21, output: 1.68, cacheWrite: 0.26, cacheRead: 0.02, saving: '80%' },
          { id: 'gemini-2.0-flash', input: 0.14, output: 1.12, cacheWrite: 0.18, cacheRead: 0.01, saving: '80%' }
        ]
      }
    }
  }
} as const

const activeLocale = computed(() => locale.value === 'zh' ? 'zh' : 'en')
const copy = computed(() => pricingCopy[activeLocale.value])
const scenarios = computed(() => [
  { id: 'claude' as const, ...copy.value.scenarios.claude },
  { id: 'codex' as const, ...copy.value.scenarios.codex },
  { id: 'gemini' as const, ...copy.value.scenarios.gemini }
])
const activeScenarioData = computed(() => copy.value.scenarios[activeScenario.value])
const siteName = computed(() => appStore.cachedPublicSettings?.site_name || appStore.siteName || 'Sub2API')

watchEffect(() => {
  document.title = `${copy.value.pageTitle} - ${siteName.value}`
})

watch(
  activeScenarioData,
  (scenario) => {
    selectedGroup.value = scenario.groups[0]?.name || ''
  },
  { immediate: true }
)

function formatPrice(value: number): string {
  const multiplier = priceMode.value === 'official' ? 5 : 1
  return `¥${(value * multiplier).toFixed(2)} / 1M tokens`
}

async function copyModelId(id: string): Promise<void> {
  await navigator.clipboard?.writeText(id)
}
</script>
