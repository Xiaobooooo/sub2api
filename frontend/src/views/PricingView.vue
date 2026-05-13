<template>
  <div class="min-h-screen bg-[#171713] text-[#faf9f5]">
    <MarketingHeader />

    <main class="px-5 py-14 sm:px-8 lg:py-20">
      <div class="mx-auto max-w-7xl">
        <section class="max-w-3xl">
          <h1 class="font-serif text-5xl font-semibold leading-tight md:text-6xl">模型价格</h1>
          <p class="mt-5 text-lg leading-8 text-[#d7d2c8]">
            以人民币展示官方价格和分组价格，按 Claude Code、Codex、Gemini 等使用场景直接比较成本。
          </p>
        </section>

        <section class="mt-10 rounded-xl border border-white/10 bg-[#1d2333] p-2">
          <div class="flex flex-col gap-2 sm:flex-row">
            <button
              v-for="product in pricingProducts"
              :key="product.id"
              type="button"
              class="flex h-14 items-center gap-3 rounded-lg px-5 text-left text-base font-semibold transition-colors"
              :class="activeProduct === product.id ? 'bg-primary-600 text-white' : 'text-[#c7d1e5] hover:bg-white/10 hover:text-white'"
              @click="activeProduct = product.id"
            >
              <Icon :name="product.icon" size="md" />
              {{ product.name }}
            </button>
          </div>
        </section>

        <section class="mt-5 rounded-xl border border-white/10 bg-[#1d2333] px-6 py-5">
          <div class="grid gap-4 text-sm text-[#d7d2c8] lg:grid-cols-[auto_1fr_auto] lg:items-center">
            <div class="flex items-center gap-2 font-semibold text-white">
              <Icon name="calculator" size="sm" class="text-primary-300" />
              计价规则
            </div>
            <p>
              官方价格按 <span class="font-semibold text-white">$1 = ￥7</span> 折算，分组价格 =
              官方价格 × 分组倍率 ÷ 7。
            </p>
            <p class="text-[#9ba8bd]">
              示例：{{ activeData.example }}
            </p>
          </div>
        </section>

        <section class="mt-5 overflow-hidden rounded-xl border border-white/10 bg-[#1d2333]">
          <div class="flex flex-col gap-4 border-b border-white/10 px-6 py-6 lg:flex-row lg:items-center lg:justify-between">
            <div>
              <div class="flex items-center gap-3">
                <Icon name="dollar" size="md" class="text-primary-300" />
                <h2 class="text-2xl font-semibold">价格列表</h2>
              </div>
              <p class="mt-2 text-sm text-[#9ba8bd]">选择分组后，直接查看每个模型的人民币价格。</p>
            </div>

            <div class="flex rounded-full bg-black/25 p-1 text-sm font-semibold">
              <button
                type="button"
                class="rounded-full px-4 py-2 transition-colors"
                :class="priceMode === 'group' ? 'bg-primary-600 text-white' : 'text-[#c7d1e5]'"
                @click="priceMode = 'group'"
              >
                分组价格
              </button>
              <button
                type="button"
                class="rounded-full px-4 py-2 transition-colors"
                :class="priceMode === 'official' ? 'bg-primary-600 text-white' : 'text-[#c7d1e5]'"
                @click="priceMode = 'official'"
              >
                官方价格
              </button>
            </div>
          </div>

          <div class="border-b border-white/10 p-6">
            <div class="grid gap-3 lg:grid-cols-3">
              <button
                v-for="group in activeData.groups"
                :key="group.id"
                type="button"
                class="rounded-lg border p-5 text-left transition-colors"
                :class="activeGroup === group.id ? 'border-primary-600 bg-[#252320]' : 'border-white/10 bg-white/[0.03] hover:border-primary-400/60'"
                @click="activeGroup = group.id"
              >
                <div class="flex items-start justify-between gap-4">
                  <div>
                    <h3 class="font-semibold text-white">{{ group.name }}</h3>
                    <p class="mt-3 text-sm text-[#9ba8bd]">{{ group.description }}</p>
                  </div>
                  <span class="rounded-full bg-primary-900/40 px-3 py-1 text-xs font-semibold text-primary-200">
                    {{ group.discount }}
                  </span>
                </div>
              </button>
            </div>

            <p class="mt-5 rounded-lg bg-black/25 px-5 py-4 text-sm leading-7 text-[#d7d2c8]">
              <span class="font-semibold text-primary-200">分组介绍：</span>
              {{ activeGroupData.intro }}
            </p>
          </div>

          <div class="overflow-x-auto p-6">
            <table class="w-full min-w-[920px] overflow-hidden rounded-lg text-sm">
              <thead>
                <tr class="bg-[#2a3548] text-left text-[#d7d2c8]">
                  <th class="px-5 py-4 font-semibold">模型 ID</th>
                  <th class="px-5 py-4 font-semibold">输入价格</th>
                  <th class="px-5 py-4 font-semibold">输出价格</th>
                  <th class="px-5 py-4 font-semibold">缓存创建</th>
                  <th class="px-5 py-4 font-semibold">缓存读取</th>
                  <th class="px-5 py-4 font-semibold">节省幅度</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-white/10 bg-[#1d2333]">
                <tr v-for="model in activeData.models" :key="model.id">
                  <td class="px-5 py-5">
                    <div class="flex items-center gap-2 font-semibold text-white">
                      {{ model.id }}
                      <button
                        type="button"
                        class="text-[#9ba8bd] transition-colors hover:text-primary-200"
                        :aria-label="`复制 ${model.id}`"
                        @click="copyModelId(model.id)"
                      >
                        <Icon name="copy" size="xs" />
                      </button>
                    </div>
                  </td>
                  <td
                    v-for="field in priceFields"
                    :key="field.key"
                    class="px-5 py-5"
                  >
                    <p class="text-lg font-semibold text-primary-300">
                      ￥{{ displayPrice(model[field.key]) }}
                      <span class="text-xs font-normal text-[#9ba8bd]">/ 1M tokens</span>
                    </p>
                    <p class="mt-1 text-xs text-[#7d899d]">
                      官方价格 ￥{{ model[field.key].toFixed(2) }}
                    </p>
                  </td>
                  <td class="px-5 py-5">
                    <span class="rounded-full bg-emerald-500/15 px-3 py-1 text-xs font-semibold text-emerald-300">
                      省 {{ activeGroupData.saveRate }}
                    </span>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </section>

        <section class="mt-10 grid gap-6 lg:grid-cols-3">
          <article
            v-for="note in pricingNotes"
            :key="note.title"
            class="rounded-xl border border-white/10 bg-white/[0.04] p-6"
          >
            <div class="flex h-10 w-10 items-center justify-center rounded-lg bg-primary-600 text-white">
              <Icon :name="note.icon" size="md" />
            </div>
            <h2 class="mt-5 text-lg font-semibold text-white">{{ note.title }}</h2>
            <p class="mt-3 text-sm leading-7 text-[#c7c1b7]">{{ note.description }}</p>
          </article>
        </section>
      </div>
    </main>

    <MarketingFooter />
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import Icon from '@/components/icons/Icon.vue'
import MarketingHeader from '@/components/marketing/MarketingHeader.vue'
import MarketingFooter from '@/components/marketing/MarketingFooter.vue'

type PriceMode = 'group' | 'official'

const pricingProducts = [
  {
    id: 'claude',
    name: 'Claude Code',
    icon: 'brain',
    example: 'claude-opus-4-7 输入价，官方 ￥35.00，Claude（特价） ￥3.50',
    groups: [
      {
        id: 'claude-special',
        name: 'Claude（特价）',
        discount: '1折',
        multiplier: 0.1,
        saveRate: '90%',
        description: '0.7x 倍率 · 相当于约 1折',
        intro: 'Claude 第三方渠道，可平替官方 Max，支持 Opus、Sonnet、Haiku 等模型，日常编码首选。'
      },
      {
        id: 'claude-max-cc',
        name: 'Claude Max（仅限 CC）',
        discount: '2.9折',
        multiplier: 0.29,
        saveRate: '71%',
        description: '2x 倍率 · 相当于约 2.9折',
        intro: '适合 Claude Code 高频用户，兼顾上下文长度、深度思考和连续任务稳定性。'
      },
      {
        id: 'claude-max',
        name: 'Claude Max（不限 CC）',
        discount: '3.6折',
        multiplier: 0.36,
        saveRate: '64%',
        description: '2.5x 倍率 · 相当于约 3.6折',
        intro: '适合需要在不同客户端间复用 Claude 能力的团队，调用方式更灵活。'
      }
    ],
    models: [
      { id: 'claude-opus-4-7', input: 35, output: 175, cacheCreate: 43.75, cacheRead: 3.5 },
      { id: 'claude-opus-4-6', input: 35, output: 175, cacheCreate: 43.75, cacheRead: 3.5 },
      { id: 'claude-sonnet-4-6', input: 21, output: 105, cacheCreate: 26.25, cacheRead: 2.1 },
      { id: 'claude-haiku-4-5', input: 7, output: 35, cacheCreate: 8.75, cacheRead: 0.7 }
    ]
  },
  {
    id: 'codex',
    name: 'Codex',
    icon: 'cpu',
    example: 'gpt-5.2 输入价，官方 ￥8.40，Codex Pro ￥1.68',
    groups: [
      {
        id: 'codex-pro',
        name: 'Codex Pro',
        discount: '2折',
        multiplier: 0.2,
        saveRate: '80%',
        description: '面向日常编码代理任务',
        intro: '适合代码生成、测试修复、批量重构和长时间自动化任务。'
      },
      {
        id: 'codex-fast',
        name: 'Codex Fast',
        discount: '3折',
        multiplier: 0.3,
        saveRate: '70%',
        description: '优先低延迟响应',
        intro: '适合轻量代码问答、快速解释和小型编辑任务。'
      },
      {
        id: 'codex-team',
        name: 'Codex Team',
        discount: '4折',
        multiplier: 0.4,
        saveRate: '60%',
        description: '适合团队共享额度',
        intro: '适合多人共享密钥、分组限额和团队成本归集。'
      }
    ],
    models: [
      { id: 'gpt-5.2', input: 8.4, output: 67.2, cacheCreate: 10.5, cacheRead: 0.84 },
      { id: 'gpt-5.2-codex', input: 10.5, output: 84, cacheCreate: 13.13, cacheRead: 1.05 },
      { id: 'gpt-5.1-mini', input: 1.4, output: 11.2, cacheCreate: 1.75, cacheRead: 0.14 },
      { id: 'o4-mini', input: 7.7, output: 30.8, cacheCreate: 9.63, cacheRead: 0.77 }
    ]
  },
  {
    id: 'gemini',
    name: 'Gemini',
    icon: 'sparkles',
    example: 'gemini-3-pro 输入价，官方 ￥14.00，Gemini Lite ￥4.20',
    groups: [
      {
        id: 'gemini-lite',
        name: 'Gemini Lite',
        discount: '3折',
        multiplier: 0.3,
        saveRate: '70%',
        description: '适合通用生成任务',
        intro: '适合命令行自动化、批量内容处理和轻量代码任务。'
      },
      {
        id: 'gemini-pro',
        name: 'Gemini Pro',
        discount: '4.5折',
        multiplier: 0.45,
        saveRate: '55%',
        description: '更高上下文与稳定性',
        intro: '适合大型上下文、复杂推理和持续工具调用。'
      },
      {
        id: 'gemini-team',
        name: 'Gemini Team',
        discount: '5折',
        multiplier: 0.5,
        saveRate: '50%',
        description: '团队协作场景',
        intro: '适合组织内统一用量统计和多密钥管理。'
      }
    ],
    models: [
      { id: 'gemini-3-pro', input: 14, output: 70, cacheCreate: 17.5, cacheRead: 1.4 },
      { id: 'gemini-2.5-pro', input: 8.75, output: 35, cacheCreate: 10.94, cacheRead: 0.88 },
      { id: 'gemini-2.5-flash', input: 2.1, output: 8.4, cacheCreate: 2.63, cacheRead: 0.21 },
      { id: 'gemini-2.0-flash-lite', input: 0.7, output: 2.8, cacheCreate: 0.88, cacheRead: 0.07 }
    ]
  }
] as const

const priceFields = [
  { key: 'input' },
  { key: 'output' },
  { key: 'cacheCreate' },
  { key: 'cacheRead' }
] as const

const pricingNotes = [
  {
    title: '按需充值',
    icon: 'creditCard',
    description: '适合个人开发者和小团队，先充值后按模型实际消耗扣费。'
  },
  {
    title: '模型成本透明',
    icon: 'chart',
    description: '同时展示官方价与分组价，方便在性能、成本和稳定性之间做选择。'
  },
  {
    title: '配额可治理',
    icon: 'shield',
    description: '配合后台分组、密钥和额度策略，避免团队共享密钥失控。'
  }
] as const

const activeProduct = ref<(typeof pricingProducts)[number]['id']>('claude')
const activeGroup = ref<(typeof pricingProducts)[number]['groups'][number]['id']>('claude-special')
const priceMode = ref<PriceMode>('group')

const activeData = computed(() => {
  return pricingProducts.find((product) => product.id === activeProduct.value) ?? pricingProducts[0]
})

const activeGroupData = computed(() => {
  return activeData.value.groups.find((group) => group.id === activeGroup.value) ?? activeData.value.groups[0]
})

watch(activeProduct, () => {
  activeGroup.value = activeData.value.groups[0].id
})

function displayPrice(value: number): string {
  const finalValue = priceMode.value === 'official' ? value : value * activeGroupData.value.multiplier
  return finalValue.toFixed(2)
}

function copyModelId(modelId: string): void {
  navigator.clipboard?.writeText(modelId).catch(() => {})
}
</script>
