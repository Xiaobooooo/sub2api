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
          <p class="mt-5 max-w-2xl text-base leading-8 text-gray-600 dark:text-dark-300 sm:text-lg whitespace-pre-line">
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
          <div class="terminal-container w-full">
            <div class="terminal-window">
              <div class="terminal-header">
                <div class="terminal-buttons" aria-hidden="true">
                  <span class="btn-close"></span>
                  <span class="btn-minimize"></span>
                  <span class="btn-maximize"></span>
                </div>
                <span class="terminal-title">api-gateway</span>
                <span class="terminal-status">{{ copy.panel.status }}</span>
              </div>

              <div class="terminal-body">
                <div class="mb-5 border-b border-white/10 pb-4">
                  <p class="text-sm font-semibold text-white">{{ copy.panel.title }}</p>
                  <p class="mt-1 text-xs text-slate-400">{{ copy.panel.subtitle }}</p>
                </div>

                <div class="grid gap-2">
                  <div
                    v-for="(line, index) in terminalLines"
                    :key="`${line.text}-${index}`"
                    class="code-line"
                    :style="{ '--line-delay': `${index * 120}ms` }"
                  >
                    <span v-if="line.prompt" class="code-prompt">$</span>
                    <span :class="line.className">{{ line.text }}</span>
                  </div>
                  <div class="code-line" :style="{ '--line-delay': `${terminalLines.length * 120}ms` }">
                    <span class="code-prompt">$</span>
                    <span class="cursor"></span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="bg-white/70 py-14 dark:border-dark-800 dark:bg-dark-900/40">
        <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
          <div class="max-w-3xl">
            <p class="text-sm font-semibold text-primary-600 dark:text-primary-400">{{ copy.ecosystem.eyebrow }}</p>
            <h2 class="mt-2 text-3xl font-bold tracking-tight text-gray-950 dark:text-white">{{ copy.ecosystem.title }}</h2>
            <p class="mt-4 text-sm leading-7 text-gray-600 dark:text-dark-300">{{ copy.ecosystem.description }}</p>
          </div>
          <div class="mt-8 grid gap-4 md:grid-cols-2 xl:grid-cols-5">
            <article
              v-for="tool in copy.ecosystem.items"
              :key="tool.name"
              class="card card-hover flex h-full flex-col p-5"
            >
              <div class="flex items-center gap-3">
                <div class="flex h-11 w-11 flex-shrink-0 items-center justify-center rounded-xl bg-white p-2 shadow-sm ring-1 ring-gray-200 dark:bg-dark-800 dark:ring-dark-700">
                  <img
                    v-if="tool.logo"
                    :src="tool.logo"
                    :alt="`${tool.name} logo`"
                    class="h-full w-full object-contain"
                  />
                  <Icon v-else name="cpu" size="md" class="text-primary-500" />
                </div>
                <div class="min-w-0">
                  <h3 class="truncate text-base font-semibold text-gray-950 dark:text-white">{{ tool.name }}</h3>
                  <p class="mt-1 text-xs font-medium text-gray-500 dark:text-dark-400">{{ tool.meta }}</p>
                </div>
              </div>
              <p class="mt-4 text-sm leading-6 text-gray-600 dark:text-dark-300">{{ tool.description }}</p>
            </article>
          </div>
        </div>
      </section>

      <section class="mx-auto max-w-7xl px-4 py-14 sm:px-6 lg:px-8 lg:py-16">
        <div class="grid gap-10 lg:grid-cols-[0.75fr_1.25fr] lg:items-start">
          <div>
            <p class="text-sm font-semibold text-primary-600 dark:text-primary-400">{{ copy.benefits.eyebrow }}</p>
            <h2 class="mt-2 text-3xl font-bold tracking-tight text-gray-950 dark:text-white">{{ copy.benefits.title }}</h2>
            <p class="mt-4 text-sm leading-7 text-gray-600 dark:text-dark-300">{{ copy.benefits.description }}</p>
          </div>

          <div class="grid gap-4 md:grid-cols-2">
            <article
              v-for="item in copy.benefits.items"
              :key="item.title"
              class="card card-hover p-5"
            >
              <div class="flex items-start gap-4">
                <div class="flex h-11 w-11 flex-shrink-0 items-center justify-center rounded-xl bg-primary-50 text-primary-600 ring-1 ring-primary-100 dark:bg-primary-900/25 dark:text-primary-300 dark:ring-primary-800/60">
                  <Icon :name="item.icon" size="md" />
                </div>
                <div>
                  <h3 class="text-base font-semibold text-gray-950 dark:text-white">{{ item.title }}</h3>
                  <p class="mt-2 text-sm leading-6 text-gray-600 dark:text-dark-300">{{ item.description }}</p>
                </div>
              </div>
            </article>
          </div>
        </div>
      </section>

      <section class="bg-white/70 py-14 dark:border-dark-800 dark:bg-dark-900/40">
        <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
          <div class="grid gap-6 lg:grid-cols-[0.8fr_1.2fr] lg:items-end">
            <div>
              <p class="text-sm font-semibold text-primary-600 dark:text-primary-400">{{ copy.pricing.eyebrow }}</p>
              <h2 class="mt-2 text-3xl font-bold tracking-tight text-gray-950 dark:text-white">{{ copy.pricing.title }}</h2>
              <p class="mt-4 text-sm leading-7 text-gray-600 dark:text-dark-300">{{ copy.pricing.description }}</p>
            </div>
            <div class="flex lg:justify-end">
              <router-link to="/pricing" class="btn btn-primary btn-md">
                {{ copy.pricing.action }}
                <Icon name="arrowRight" size="sm" />
              </router-link>
            </div>
          </div>

          <div class="mt-8 grid gap-4 lg:grid-cols-3">
            <article
              v-for="plan in copy.pricing.plans"
              :key="plan.title"
              class="card card-hover flex h-full flex-col overflow-hidden"
            >
              <div class="flex items-start justify-between gap-4 border-b border-gray-100 p-5 dark:border-dark-700">
                <div class="flex items-start gap-3">
                  <div class="flex h-11 w-11 flex-shrink-0 items-center justify-center rounded-xl bg-primary-50 text-primary-600 ring-1 ring-primary-100 dark:bg-primary-900/25 dark:text-primary-300 dark:ring-primary-800/60">
                    <Icon :name="plan.icon" size="md" />
                  </div>
                  <div>
                    <h3 class="text-lg font-semibold text-gray-950 dark:text-white">{{ plan.title }}</h3>
                    <p class="mt-1 text-sm text-gray-500 dark:text-dark-400">{{ plan.subtitle }}</p>
                  </div>
                </div>
                <span class="badge badge-primary flex-shrink-0">{{ plan.badge }}</span>
              </div>

              <div class="px-5 py-4">
                <p class="text-sm font-semibold text-gray-950 dark:text-white">{{ plan.rate }}</p>
                <p class="mt-2 text-sm leading-6 text-gray-600 dark:text-dark-300">{{ plan.description }}</p>
              </div>

              <div class="mt-auto overflow-x-auto px-5 pb-5">
                <table class="w-full min-w-[420px] text-left text-xs">
                  <thead>
                    <tr class="border-b border-gray-100 text-gray-500 dark:border-dark-700 dark:text-dark-400">
                      <th class="pb-2 font-medium">{{ copy.pricing.columns.model }}</th>
                      <th class="pb-2 font-medium">{{ copy.pricing.columns.input }}</th>
                      <th class="pb-2 font-medium">{{ copy.pricing.columns.output }}</th>
                      <th class="pb-2 font-medium">{{ copy.pricing.columns.saving }}</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="row in plan.models"
                      :key="row.name"
                      class="border-b border-gray-100 last:border-0 dark:border-dark-800"
                    >
                      <td class="py-2 pr-3">
                        <div class="font-mono text-gray-900 dark:text-gray-100">{{ row.name }}</div>
                        <div class="mt-0.5 text-gray-400 dark:text-dark-500">{{ row.official }}</div>
                      </td>
                      <td class="py-2 pr-3 font-medium text-gray-700 dark:text-dark-200">{{ row.input }}</td>
                      <td class="py-2 pr-3 font-medium text-gray-700 dark:text-dark-200">{{ row.output }}</td>
                      <td class="py-2">
                        <span class="badge badge-success">{{ row.saving }}</span>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </article>
          </div>
        </div>
      </section>

      <section class="mx-auto max-w-7xl px-4 py-14 sm:px-6 lg:px-8 lg:py-16">
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
import claudeCodeLogo from '@/assets/icons/claudecode.svg'
import codexLogo from '@/assets/icons/codex.svg'
import hermesLogo from '@/assets/icons/hermesagent.svg'
import openClawLogo from '@/assets/icons/openclaw.svg'

const appStore = useAppStore()
const authStore = useAuthStore()
const { locale } = useI18n()

const homeCopy = {
  zh: {
    pageTitle: '首页',
    hero: {
      badge: 'AI API 网关',
      title: '接入全球顶尖模型',
      description: '免翻墙直连、无封号风险、余额永不过期\n低延迟调用 Claude、ChatGPT 等主流模型',
      start: '立即开始',
      dashboard: '进入控制台',
      pricing: '查看模型',
      stats: [
        { value: '5+', label: '兼容生态' },
        { value: '7', label: '支持模型' },
        { value: '1￥:1＄', label: '充值比例' }
      ]
    },
    panel: {
      title: '统一调用入口',
      subtitle: '全球顶尖模型集中接入',
      status: '在线',
    },
    ecosystem: {
      eyebrow: '兼容生态',
      title: '接入主流 AI Agent 工作流',
      description: '面向日常编码、重构、调试和自动化任务，保持常见 AI Agent 的原有使用方式，只替换 API Key 与调用地址即可接入。',
      items: [
        {
          name: 'Codex',
          meta: 'OpenAI',
          logo: codexLogo,
          description: '适合大型重构、Bug 修复、测试生成和代码审查等工程任务。'
        },
        {
          name: 'Claude Code',
          meta: 'Anthropic',
          logo: claudeCodeLogo,
          description: '保留 CLI 编程助手体验，适合长上下文分析与项目级改动。'
        },
        {
          name: 'OpenClaw',
          meta: '开源 · 本地',
          logo: openClawLogo,
          description: '面向本地工作流的开源 AI Agent，适合可控的桌面开发环境。'
        },
        {
          name: 'Hermes',
          meta: 'AI Agent',
          logo: hermesLogo,
          description: '适合接入自动化代理流程，把模型能力嵌入持续开发任务。'
        },
        {
          name: '其他AI Agent',
          meta: 'OpenAI 兼容',
          logo: '',
          description: '支持通过兼容接口接入更多编辑器插件、终端工具和自建代理。'
        }
      ]
    },
    benefits: {
      eyebrow: '使用价值',
      title: '一个入口覆盖更多模型与场景',
      description: '围绕真实开发工作流设计，减少环境、账号和工具切换带来的额外成本。',
      items: [
        {
          icon: 'key',
          title: '一个 Key 调用全模型',
          description: '统一管理 API Key，在 Claude、OpenAI 等模型间灵活切换。'
        },
        {
          icon: 'globe',
          title: '低延迟直连',
          description: '面向国内访问优化，减少额外网络配置，让工具更快进入可用状态。'
        },
        {
          icon: 'swap',
          title: '改一行 BASE_URL 接入',
          description: '兼容常见 SDK 与 Agent 配置方式，降低迁移和接入成本。'
        },
        {
          icon: 'chartBar',
          title: '统一面板管理',
          description: '集中查看用量、余额和 Key 状态，团队与个人调用都更清晰。'
        }
      ]
    },
    pricing: {
      eyebrow: '定价方案',
      title: '价格透明 · 按量计费',
      description: '1 RMB = 1 USD，官方同价 · 渠道价低至 1 折，余额永不过期。',
      action: '查看完整价格',
      columns: {
        model: '模型',
        input: '输入',
        output: '输出',
        saving: '节省'
      },
      plans: [
        {
          icon: 'brain',
          title: 'Claude Code',
          subtitle: '推荐编码场景',
          badge: '省 90%',
          rate: '1 折 · 0.7x 倍率',
          description: '适合 Claude Code 高频开发，覆盖 Opus、Sonnet 与 Haiku 系列。',
          models: [
            { name: 'claude-opus-4-7', official: '官方 ¥35', input: '¥3.50', output: '¥17.50', saving: '90%' },
            { name: 'claude-opus-4-6', official: '官方 ¥35', input: '¥3.50', output: '¥17.50', saving: '90%' },
            { name: 'claude-sonnet-4-6', official: '官方 ¥21', input: '¥2.10', output: '¥10.50', saving: '90%' },
            { name: 'claude-haiku-4-5', official: '官方 ¥7', input: '¥0.70', output: '¥3.50', saving: '90%' }
          ]
        },
        {
          icon: 'badge',
          title: 'Claude Max',
          subtitle: '稳定长任务',
          badge: '省 71%',
          rate: '2.9 折 · 2x 倍率',
          description: '面向更稳定的 Claude 长上下文任务，适合持续分析与复杂推理。',
          models: [
            { name: 'claude-opus-4-7', official: '官方 ¥35', input: '¥10.00', output: '¥50.00', saving: '71%' },
            { name: 'claude-opus-4-6', official: '官方 ¥35', input: '¥10.00', output: '¥50.00', saving: '71%' },
            { name: 'claude-sonnet-4-6', official: '官方 ¥21', input: '¥6.00', output: '¥30.00', saving: '71%' },
            { name: 'claude-haiku-4-5', official: '官方 ¥7', input: '¥2.00', output: '¥10.00', saving: '71%' }
          ]
        },
        {
          icon: 'bolt',
          title: 'ChatGPT / Codex',
          subtitle: 'OpenAI 系列',
          badge: '省 93%',
          rate: '0.7 折 · 0.5x 倍率',
          description: '适合 Codex、通用对话和自动化编码任务，覆盖 GPT 与 Codex 模型。',
          models: [
            { name: 'gpt-5.5', official: '官方 ¥35', input: '¥2.50', output: '¥15.00', saving: '93%' },
            { name: 'gpt-5.4', official: '官方 ¥35', input: '¥2.50', output: '¥11.25', saving: '93%' },
            { name: 'gpt-5.2', official: '官方 ¥12.25', input: '¥0.88', output: '¥7.00', saving: '93%' },
            { name: 'gpt-5.3-codex', official: '官方 ¥12.25', input: '¥0.88', output: '¥7.00', saving: '93%' }
          ]
        }
      ]
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
      title: 'Access to the World’s Leading AI Models',
      description: 'Direct access without VPN, no account-ban risk, non-expiring balance\nlow-latency calls to mainstream models like Claude and ChatGPT.',
      start: 'Get started',
      dashboard: 'Dashboard',
      pricing: 'View models',
      stats: [
        { value: '5+', label: 'Compatible tools' },
        { value: '7', label: 'Models' },
        { value: '1￥:1＄', label: 'Top-up ratio' }
      ]
    },
    panel: {
      title: 'Unified endpoint',
      subtitle: 'Centralized access to World’s Leading AI Models',
      status: 'Online',
    },
    ecosystem: {
      eyebrow: 'Compatible ecosystem',
      title: 'Works with mainstream AI Agent workflows',
      description: 'Keep your familiar coding, refactoring, debugging, and automation workflow. Replace the API key and base URL to connect through the gateway.',
      items: [
        {
          name: 'Codex',
          meta: 'OpenAI',
          logo: codexLogo,
          description: 'Built for large refactors, bug fixes, test generation, and code review tasks.'
        },
        {
          name: 'Claude Code',
          meta: 'Anthropic',
          logo: claudeCodeLogo,
          description: 'Keeps the CLI assistant workflow for long-context analysis and project changes.'
        },
        {
          name: 'OpenClaw',
          meta: 'Open source · Local',
          logo: openClawLogo,
          description: 'A local open-source agent option for controlled desktop development.'
        },
        {
          name: 'Hermes',
          meta: 'AI Agent',
          logo: hermesLogo,
          description: 'Connect automated agent flows to model capabilities for ongoing development tasks.'
        },
        {
          name: 'Other AI Agents',
          meta: 'OpenAI compatible',
          logo: '',
          description: 'Use compatible endpoints with more editor plugins, terminal tools, and custom agents.'
        }
      ]
    },
    benefits: {
      eyebrow: 'Usage value',
      title: 'One gateway for more models and scenarios',
      description: 'Designed around real development workflows to reduce environment, account, and tool switching overhead.',
      items: [
        {
          icon: 'key',
          title: 'One key for every model',
          description: 'Manage one API key and switch between Claude, OpenAI, and other model families.'
        },
        {
          icon: 'globe',
          title: 'Low-latency direct access',
          description: 'Optimized access reduces extra network setup and gets tools ready faster.'
        },
        {
          icon: 'swap',
          title: 'Change one BASE_URL',
          description: 'Compatible with common SDK and Agent configuration patterns to lower migration cost.'
        },
        {
          icon: 'chartBar',
          title: 'Unified dashboard',
          description: 'Track usage, balance, and key status from one place for individuals and teams.'
        }
      ]
    },
    pricing: {
      eyebrow: 'Pricing plans',
      title: 'Transparent pay-as-you-go pricing',
      description: '1 RMB = 1 USD, official parity with channel pricing down to around 10% of official rates. Balance never expires.',
      action: 'View full pricing',
      columns: {
        model: 'Model',
        input: 'Input',
        output: 'Output',
        saving: 'Save'
      },
      plans: [
        {
          icon: 'brain',
          title: 'Claude Code',
          subtitle: 'Recommended for coding',
          badge: 'Save 90%',
          rate: '0.7x multiplier · about 10% of official',
          description: 'For heavy Claude Code development with Opus, Sonnet, and Haiku models.',
          models: [
            { name: 'claude-opus-4-7', official: 'Official ¥35', input: '¥3.50', output: '¥17.50', saving: '90%' },
            { name: 'claude-opus-4-6', official: 'Official ¥35', input: '¥3.50', output: '¥17.50', saving: '90%' },
            { name: 'claude-sonnet-4-6', official: 'Official ¥21', input: '¥2.10', output: '¥10.50', saving: '90%' },
            { name: 'claude-haiku-4-5', official: 'Official ¥7', input: '¥0.70', output: '¥3.50', saving: '90%' }
          ]
        },
        {
          icon: 'badge',
          title: 'Claude Max',
          subtitle: 'Stable long-running tasks',
          badge: 'Save 71%',
          rate: '2x multiplier · about 29% of official',
          description: 'For stable long-context Claude tasks, continuous analysis, and complex reasoning.',
          models: [
            { name: 'claude-opus-4-7', official: 'Official ¥35', input: '¥10.00', output: '¥50.00', saving: '71%' },
            { name: 'claude-opus-4-6', official: 'Official ¥35', input: '¥10.00', output: '¥50.00', saving: '71%' },
            { name: 'claude-sonnet-4-6', official: 'Official ¥21', input: '¥6.00', output: '¥30.00', saving: '71%' },
            { name: 'claude-haiku-4-5', official: 'Official ¥7', input: '¥2.00', output: '¥10.00', saving: '71%' }
          ]
        },
        {
          icon: 'bolt',
          title: 'ChatGPT / Codex',
          subtitle: 'OpenAI models',
          badge: 'Save 93%',
          rate: '0.5x multiplier · about 7% of official',
          description: 'For Codex, general chat, and automated coding workflows across GPT and Codex models.',
          models: [
            { name: 'gpt-5.5', official: 'Official ¥35', input: '¥2.50', output: '¥15.00', saving: '93%' },
            { name: 'gpt-5.4', official: 'Official ¥35', input: '¥2.50', output: '¥11.25', saving: '93%' },
            { name: 'gpt-5.2', official: 'Official ¥12.25', input: '¥0.88', output: '¥7.00', saving: '93%' },
            { name: 'gpt-5.3-codex', official: 'Official ¥12.25', input: '¥0.88', output: '¥7.00', saving: '93%' }
          ]
        }
      ]
    },
    cta: {
      title: 'Start using supported models',
      description: 'Create an API key in the dashboard, then call Claude or OpenAI models.',
      action: 'Start setup'
    }
  }
} as const

const terminalLines = [
  { prompt: true, text: 'export ANTHROPIC_BASE_URL="https://api.xiaobocode.com"', className: 'code-cmd' },
  { prompt: false, text: 'set base_url', className: 'code-muted' },
  { prompt: true, text: 'export ANTHROPIC_AUTH_TOKEN="sk-..."', className: 'code-cmd' },
  { prompt: false, text: 'set api_key', className: 'code-muted' },
  { prompt: true, text: 'export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1', className: 'code-cmd' },
  { prompt: true, text: 'claude', className: 'code-success' },
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

<style scoped>
.terminal-container {
  display: flex;
  justify-content: center;
}

.terminal-window {
  width: min(100%, 560px);
  overflow: hidden;
  border-radius: 18px;
  background:
    radial-gradient(circle at 85% 15%, rgba(204, 120, 92, 0.2), transparent 30%),
    linear-gradient(145deg, #172033 0%, #0b1020 100%);
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow:
    0 28px 70px -28px rgba(0, 0, 0, 0.75),
    0 0 0 1px rgba(204, 120, 92, 0.12),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
  transform: perspective(1200px) rotateX(2deg) rotateY(-2deg);
  transition: transform 240ms ease, box-shadow 240ms ease;
}

.terminal-window:hover {
  transform: perspective(1200px) rotateX(0deg) rotateY(0deg) translateY(-4px);
  box-shadow:
    0 32px 78px -30px rgba(0, 0, 0, 0.8),
    0 0 42px rgba(204, 120, 92, 0.16),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
}

.terminal-header {
  display: grid;
  grid-template-columns: auto 1fr auto;
  align-items: center;
  gap: 12px;
  padding: 13px 16px;
  background: rgba(8, 12, 24, 0.56);
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
}

.terminal-buttons {
  display: flex;
  gap: 8px;
}

.terminal-buttons span {
  width: 12px;
  height: 12px;
  border-radius: 9999px;
}

.btn-close {
  background: #ef4444;
}

.btn-minimize {
  background: #eab308;
}

.btn-maximize {
  background: #22c55e;
}

.terminal-title {
  min-width: 0;
  text-align: center;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
  font-size: 12px;
  color: #94a3b8;
}

.terminal-status {
  border-radius: 9999px;
  background: rgba(34, 197, 94, 0.12);
  padding: 3px 9px;
  font-size: 11px;
  font-weight: 700;
  color: #86efac;
}

.terminal-body {
  padding: 22px;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
}

.code-line {
  display: flex;
  min-height: 25px;
  align-items: flex-start;
  gap: 8px;
  overflow-wrap: anywhere;
  font-size: 13px;
  line-height: 1.8;
  opacity: 0;
  animation: line-appear 420ms ease forwards;
  animation-delay: var(--line-delay, 0ms);
}

.code-prompt {
  color: #86efac;
  font-weight: 800;
}

.code-cmd {
  color: #e2e8f0;
}

.code-muted {
  color: #94a3b8;
}

.code-success {
  color: #86efac;
}

.cursor {
  display: inline-block;
  width: 8px;
  height: 18px;
  margin-top: 4px;
  background: #86efac;
  animation: blink 1s step-end infinite;
}

@keyframes line-appear {
  from {
    opacity: 0;
    transform: translateY(6px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes blink {
  0%,
  50% {
    opacity: 1;
  }

  51%,
  100% {
    opacity: 0;
  }
}

@media (max-width: 640px) {
  .terminal-window {
    transform: none;
  }

  .terminal-window:hover {
    transform: translateY(-2px);
  }

  .terminal-body {
    padding: 18px;
  }

  .code-line {
    font-size: 12px;
  }
}
</style>
