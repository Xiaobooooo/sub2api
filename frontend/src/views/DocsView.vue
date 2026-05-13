<template>
  <div class="min-h-screen bg-[#171713] text-[#faf9f5]">
    <MarketingHeader />

    <main class="px-5 py-14 sm:px-8 lg:py-20">
      <div class="mx-auto grid max-w-7xl gap-12 lg:grid-cols-[280px_1fr]">
        <aside class="lg:sticky lg:top-28 lg:self-start">
          <h1 class="text-sm font-semibold text-white">快速上手</h1>
          <nav class="mt-5 grid gap-2">
            <button
              v-for="doc in docSections"
              :key="doc.id"
              type="button"
              class="flex items-center gap-3 rounded-lg px-4 py-3 text-left text-sm font-semibold transition-colors"
              :class="activeDoc === doc.id ? 'bg-primary-900/40 text-primary-200' : 'text-[#b9b2a7] hover:bg-white/10 hover:text-white'"
              @click="activeDoc = doc.id"
            >
              <Icon :name="doc.icon" size="sm" />
              {{ doc.title }}
            </button>
          </nav>
        </aside>

        <section>
          <div class="max-w-4xl">
            <h2 class="font-serif text-5xl font-semibold leading-tight md:text-6xl">
              {{ activeDocData.heading }}
            </h2>
            <p class="mt-5 text-lg leading-8 text-[#d7d2c8]">
              {{ activeDocData.description }}
            </p>
          </div>

          <article class="mt-12 rounded-xl border border-white/10 bg-white/[0.04] p-6 md:p-8">
            <div class="flex flex-col gap-5 border-b border-white/10 pb-6 lg:flex-row lg:items-center lg:justify-between">
              <div>
                <h3 class="text-2xl font-semibold text-white">按系统选择一键脚本</h3>
                <p class="mt-3 max-w-3xl text-sm leading-7 text-[#c7c1b7]">
                  脚本会自动检测运行环境，缺失时引导安装；随后提示输入 API Key，并写入对应客户端配置。复制命令不包含密钥。
                </p>
              </div>
              <div class="flex rounded-full bg-black/25 p-1 text-sm font-semibold">
                <button
                  v-for="platform in platforms"
                  :key="platform.id"
                  type="button"
                  class="rounded-full px-4 py-2 transition-colors"
                  :class="activePlatform === platform.id ? 'bg-primary-600 text-white' : 'text-[#c7d1e5]'"
                  @click="activePlatform = platform.id"
                >
                  {{ platform.label }}
                </button>
              </div>
            </div>

            <div class="mt-8 overflow-hidden rounded-xl border border-white/10 bg-[#151525]">
              <div class="flex items-center justify-between border-b border-white/10 bg-white/[0.04] px-5 py-3">
                <span class="text-xs font-semibold uppercase text-[#9ba8bd]">{{ activePlatformData.shell }}</span>
                <button
                  type="button"
                  class="inline-flex items-center gap-2 rounded-md border border-white/10 px-3 py-1.5 text-xs font-semibold text-[#d7d2c8] transition-colors hover:bg-white/10 hover:text-white"
                  @click="copyCommand"
                >
                  <Icon name="copy" size="xs" />
                  复制
                </button>
              </div>
              <pre class="overflow-x-auto p-5 text-sm leading-7 text-white"><code>{{ activeDocData.commands[activePlatform] }}</code></pre>
            </div>

            <div class="mt-6 border-l-4 border-primary-400 bg-white/[0.06] px-5 py-4 text-sm leading-7 text-[#d7d2c8]">
              <span class="font-semibold text-primary-200">配置写入：</span>
              <code class="rounded bg-black/30 px-2 py-1 text-primary-100">{{ activeDocData.configPath }}</code>
              后，已打开的 CLI 通常需要重启才会读取新配置。
            </div>
          </article>

          <section class="mt-8 grid gap-6 lg:grid-cols-3">
            <article
              v-for="step in activeDocData.steps"
              :key="step.title"
              class="rounded-xl border border-white/10 bg-white/[0.04] p-6"
            >
              <div class="flex h-10 w-10 items-center justify-center rounded-lg bg-primary-600 text-white">
                <Icon :name="step.icon" size="md" />
              </div>
              <h3 class="mt-5 text-lg font-semibold text-white">{{ step.title }}</h3>
              <p class="mt-3 text-sm leading-7 text-[#c7c1b7]">{{ step.description }}</p>
            </article>
          </section>

          <section class="mt-8 overflow-hidden rounded-xl border border-white/10 bg-white/[0.04]">
            <button
              type="button"
              class="flex w-full items-center justify-between gap-4 px-6 py-5 text-left"
              @click="manualOpen = !manualOpen"
            >
              <span>
                <span class="block text-xl font-semibold text-white">备用方式：手动配置</span>
                <span class="mt-1 block text-sm text-[#9ba8bd]">一键脚本不可用时再展开使用。</span>
              </span>
              <Icon
                name="chevronDown"
                size="md"
                class="shrink-0 text-primary-200 transition-transform"
                :class="manualOpen ? 'rotate-180' : ''"
              />
            </button>

            <div v-if="manualOpen" class="border-t border-white/10 px-6 py-6">
              <ol class="grid gap-5 text-sm leading-7 text-[#d7d2c8]">
                <li v-for="item in activeDocData.manual" :key="item" class="flex gap-3">
                  <Icon name="check" size="sm" class="mt-1 text-primary-300" />
                  <span>{{ item }}</span>
                </li>
              </ol>
            </div>
          </section>

          <section class="mt-12 rounded-xl bg-primary-600 p-8 text-white md:p-10">
            <div class="grid gap-6 lg:grid-cols-[1fr_auto] lg:items-center">
              <div>
                <h2 class="font-serif text-3xl font-semibold">已经创建好 API Key？</h2>
                <p class="mt-3 max-w-2xl text-sm leading-7 text-primary-50">
                  回到控制台创建或复制密钥，再按本页命令写入客户端配置。
                </p>
              </div>
              <router-link
                to="/dashboard"
                class="inline-flex h-12 items-center justify-center rounded-lg bg-[#faf9f5] px-6 text-sm font-semibold text-[#141413] transition-colors hover:bg-white"
              >
                进入控制台
              </router-link>
            </div>
          </section>
        </section>
      </div>
    </main>

    <MarketingFooter />
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import Icon from '@/components/icons/Icon.vue'
import MarketingHeader from '@/components/marketing/MarketingHeader.vue'
import MarketingFooter from '@/components/marketing/MarketingFooter.vue'

type PlatformId = 'windows' | 'unix'

const platforms = [
  { id: 'windows', label: 'Windows PowerShell', shell: 'POWERSHELL' },
  { id: 'unix', label: 'macOS / Linux / WSL', shell: 'SHELL' }
] as const

const docSections = [
  {
    id: 'node',
    title: 'Node.js 环境安装教程',
    icon: 'cube',
    heading: 'Node.js 环境安装教程',
    description: '在本地准备运行 AI 编程工具所需的 Node.js 与包管理环境。',
    configPath: 'node --version',
    commands: {
      windows: 'winget install OpenJS.NodeJS.LTS',
      unix: 'curl -fsSL https://fnm.vercel.app/install | bash && fnm install --lts'
    },
    steps: [
      { title: '检查版本', icon: 'terminal', description: '安装完成后执行 node --version，确认当前终端能读取 Node.js。' },
      { title: '安装工具', icon: 'download', description: '根据客户端要求安装 Codex、Claude Code、Gemini CLI 或其他命令行工具。' },
      { title: '重启终端', icon: 'refresh', description: '环境变量写入后，重启终端能避免命令不可用的问题。' }
    ],
    manual: [
      '访问 Node.js 官网下载 LTS 版本安装包。',
      '安装完成后重新打开终端，执行 node --version 与 npm --version。',
      '如果命令不可用，检查 PATH 环境变量是否包含 Node.js 安装目录。'
    ]
  },
  {
    id: 'codex',
    title: 'Codex 配置教程',
    icon: 'cpu',
    heading: 'Codex (OpenAI) 配置教程',
    description: '安装并配置 Codex，通过 ~/.codex 连接当前平台。',
    configPath: '~/.codex',
    commands: {
      windows: '$env:OPENAI_API_KEY="sk-..."\n$env:OPENAI_BASE_URL="https://your-domain.example/v1"\ncodex',
      unix: 'export OPENAI_API_KEY="sk-..."\nexport OPENAI_BASE_URL="https://your-domain.example/v1"\ncodex'
    },
    steps: [
      { title: '复制命令', icon: 'copy', description: '命令本身不包含 API Key，可直接复制到本地终端执行。' },
      { title: '输入密钥', icon: 'key', description: '脚本提示时粘贴控制台创建的 API Key，不要把密钥写入聊天或日志。' },
      { title: '验证调用', icon: 'checkCircle', description: '重启 Codex CLI 或 Codex App，执行一次简单请求确认配置生效。' }
    ],
    manual: [
      '安装 Codex CLI 后，打开用户目录下的 .codex 配置目录。',
      '写入 API Key 和 Base URL，Base URL 以当前站点控制台展示为准。',
      '保存配置后重启 Codex，再执行一次小任务验证连接。'
    ]
  },
  {
    id: 'claude',
    title: 'Claude Code 配置教程',
    icon: 'brain',
    heading: 'Claude Code 配置教程',
    description: '将 Claude Code 的 API 入口指向平台网关，保留原生命令行体验。',
    configPath: '~/.claude',
    commands: {
      windows: '$env:ANTHROPIC_API_KEY="sk-..."\n$env:ANTHROPIC_BASE_URL="https://your-domain.example"\nclaude',
      unix: 'export ANTHROPIC_API_KEY="sk-..."\nexport ANTHROPIC_BASE_URL="https://your-domain.example"\nclaude'
    },
    steps: [
      { title: '安装 CLI', icon: 'terminal', description: '确保 Claude Code 已经可以在终端中直接运行。' },
      { title: '写入配置', icon: 'document', description: '设置 API Key、Base URL 和默认模型组。' },
      { title: '开始编码', icon: 'sparkles', description: '在项目目录内启动 Claude Code，验证读写文件和模型调用。' }
    ],
    manual: [
      '安装官方 Claude Code 客户端。',
      '在配置文件中写入平台提供的 API Key 与网关地址。',
      '重启 CLI 后在测试项目中发送一次简单请求。'
    ]
  },
  {
    id: 'gemini',
    title: 'Gemini CLI 配置教程',
    icon: 'sparkles',
    heading: 'Gemini CLI 配置教程',
    description: '在终端中配置 Gemini CLI，通过统一网关调用 Gemini 模型。',
    configPath: '~/.gemini',
    commands: {
      windows: '$env:GEMINI_API_KEY="sk-..."\n$env:OPENAI_BASE_URL="https://your-domain.example/v1"\ngemini',
      unix: 'export GEMINI_API_KEY="sk-..."\nexport OPENAI_BASE_URL="https://your-domain.example/v1"\ngemini'
    },
    steps: [
      { title: '选择模型', icon: 'grid', description: '根据任务选择 Flash、Pro 或团队配置中的默认模型。' },
      { title: '配置密钥', icon: 'key', description: '把 API Key 写入本地配置，不要放到项目仓库。' },
      { title: '检查输出', icon: 'clipboard', description: '执行一次问答或文件分析，确认流式输出正常。' }
    ],
    manual: [
      '安装 Gemini CLI 并确认命令可用。',
      '在用户配置目录写入 API Key、Base URL 和模型名称。',
      '重新打开终端后运行一次 gemini 命令验证。'
    ]
  },
  {
    id: 'openclaw',
    title: 'OpenClaw 配置教程',
    icon: 'cloud',
    heading: 'OpenClaw 配置教程',
    description: '为本地 AI 助手配置 OpenAI 兼容入口，让桌面工作流接入统一网关。',
    configPath: 'OpenClaw 设置页',
    commands: {
      windows: '$env:OPENAI_API_KEY="sk-..."\n$env:OPENAI_BASE_URL="https://your-domain.example/v1"',
      unix: 'export OPENAI_API_KEY="sk-..."\nexport OPENAI_BASE_URL="https://your-domain.example/v1"'
    },
    steps: [
      { title: '打开设置', icon: 'cog', description: '进入 OpenClaw 的模型服务或 Provider 配置页。' },
      { title: '填写地址', icon: 'link', description: '选择 OpenAI Compatible，填写 API Key 与 Base URL。' },
      { title: '保存测试', icon: 'checkCircle', description: '保存后发起一次对话，确认模型列表与调用成功。' }
    ],
    manual: [
      '打开 OpenClaw 设置页，新增 OpenAI Compatible Provider。',
      '填写控制台复制的 API Key 和 Base URL。',
      '选择默认模型后保存，并执行一次测试对话。'
    ]
  }
] as const

const activeDoc = ref<(typeof docSections)[number]['id']>('codex')
const activePlatform = ref<PlatformId>('windows')
const manualOpen = ref(false)

const activeDocData = computed(() => {
  return docSections.find((doc) => doc.id === activeDoc.value) ?? docSections[1]
})

const activePlatformData = computed(() => {
  return platforms.find((platform) => platform.id === activePlatform.value) ?? platforms[0]
})

function copyCommand(): void {
  navigator.clipboard?.writeText(activeDocData.value.commands[activePlatform.value]).catch(() => {})
}
</script>
