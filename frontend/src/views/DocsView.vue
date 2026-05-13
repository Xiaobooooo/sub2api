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
                <p class="mt-1 text-sm text-gray-500 dark:text-dark-400">{{ copy.script.description }}</p>
              </div>
              <div class="tabs w-full overflow-x-auto lg:w-auto">
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
              <span class="font-semibold text-primary-700 dark:text-primary-300">{{ copy.script.writeTo }}</span>
              <code class="rounded bg-white px-2 py-1 font-mono text-primary-700 dark:bg-dark-800 dark:text-primary-300">{{ activeDocData.configPath }}</code>
              {{ copy.script.restart }}
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

        <details class="card group mt-8 overflow-hidden">
          <summary class="flex cursor-pointer list-none items-center justify-between gap-4 px-6 py-5">
            <div>
              <h2 class="font-semibold text-gray-950 dark:text-white">{{ copy.manual.title }}</h2>
              <p class="mt-1 text-sm text-gray-500 dark:text-dark-400">{{ copy.manual.description }}</p>
            </div>
            <Icon name="chevronDown" size="sm" class="text-gray-400 transition-transform group-open:rotate-180" />
          </summary>
          <div class="border-t border-gray-100 px-6 py-5 dark:border-dark-700">
            <ul class="grid gap-4 text-sm leading-7 text-gray-600 dark:text-dark-300">
              <li v-for="item in activeDocData.manual" :key="item" class="flex gap-3">
                <Icon name="check" size="sm" class="mt-1 text-primary-500" />
                <span>{{ item }}</span>
              </li>
            </ul>
          </div>
        </details>

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

type DocId = 'node' | 'codex' | 'claude' | 'gemini' | 'openclaw'
type PlatformId = 'windows' | 'unix'

const { locale } = useI18n()
const appStore = useAppStore()
const activeDoc = ref<DocId>('codex')
const activePlatform = ref<PlatformId>('windows')
const copied = ref(false)

const docsCopy = {
  zh: {
    pageTitle: '接入文档',
    sidebar: {
      title: '快速上手'
    },
    hero: {
      badge: '接入文档'
    },
    script: {
      title: '按系统选择配置命令',
      description: '内容参考 APIKEY.FUN 文档页的左侧导航、平台切换、命令块和备用手动配置结构；样式使用当前项目自己的卡片与表单体系。',
      writeTo: '配置写入：',
      restart: ' 后，已打开的 CLI 通常需要重启才会读取新配置。'
    },
    manual: {
      title: '备用方式：手动配置',
      description: '一键命令不可用时再展开使用。'
    },
    cta: {
      title: '已经创建好 API Key？',
      description: '回到控制台创建或复制密钥，再按本页命令写入客户端配置。',
      action: '进入控制台'
    },
    copy: '复制',
    copied: '已复制',
    platforms: [
      { id: 'windows', label: 'Windows PowerShell' },
      { id: 'unix', label: 'macOS / Linux / WSL' }
    ],
    docs: {
      node: {
        shortTitle: 'Node.js 环境安装教程',
        title: 'Node.js 环境安装教程',
        description: '大部分命令行客户端依赖 Node.js 或 npm/npx。先确认运行环境可用，再配置模型客户端。',
        icon: 'cube',
        configPath: 'node --version',
        commands: {
          windows: 'winget install OpenJS.NodeJS.LTS\nnode --version\nnpm --version',
          unix: 'node --version\nnpm --version\n# 如未安装，请使用系统包管理器或 nvm 安装 Node.js LTS'
        },
        steps: [
          { icon: 'download', title: '安装 Node.js', description: '优先安装 LTS 版本，避免客户端依赖不兼容。' },
          { icon: 'terminal', title: '检查命令', description: '确认 node、npm 或 npx 在终端中可用。' },
          { icon: 'checkCircle', title: '继续配置', description: '环境验证通过后，再选择 Codex、Claude Code 或 Gemini CLI。' }
        ],
        manual: [
          '打开终端执行 node --version。',
          '如命令不存在，安装 Node.js LTS 后重新打开终端。',
          '确认 npm --version 能输出版本号。'
        ]
      },
      codex: {
        shortTitle: 'Codex 配置教程',
        title: 'Codex (OpenAI) 配置教程',
        description: '安装并配置 Codex，通过 OpenAI 兼容 Base URL 连接当前平台。',
        icon: 'cpu',
        configPath: '~/.codex',
        commands: {
          windows: '$env:OPENAI_API_KEY="sk-..."\n$env:OPENAI_BASE_URL="https://your-domain.example/v1"\ncodex',
          unix: 'export OPENAI_API_KEY="sk-..."\nexport OPENAI_BASE_URL="https://your-domain.example/v1"\ncodex'
        },
        steps: [
          { icon: 'copy', title: '复制命令', description: '命令本身不包含真实 API Key，可直接复制到本地终端。' },
          { icon: 'key', title: '输入密钥', description: '把控制台创建的 API Key 写入环境变量，不要放到聊天或日志。' },
          { icon: 'checkCircle', title: '验证调用', description: '重启 Codex CLI 或 Codex App，执行一次简单请求确认配置生效。' }
        ],
        manual: [
          '打开 Codex 配置目录或终端环境变量配置。',
          '写入 OPENAI_API_KEY、OPENAI_BASE_URL 和默认模型名称。',
          '重启客户端后执行一次 codex 命令验证。'
        ]
      },
      claude: {
        shortTitle: 'Claude Code 配置教程',
        title: 'Claude Code 配置教程',
        description: '使用平台分配的密钥和兼容地址，让 Claude Code 请求进入统一网关。',
        icon: 'beaker',
        configPath: '~/.claude',
        commands: {
          windows: '$env:ANTHROPIC_API_KEY="sk-..."\n$env:ANTHROPIC_BASE_URL="https://your-domain.example"\nclaude',
          unix: 'export ANTHROPIC_API_KEY="sk-..."\nexport ANTHROPIC_BASE_URL="https://your-domain.example"\nclaude'
        },
        steps: [
          { icon: 'terminal', title: '打开终端', description: '确认 Claude Code 已安装且 claude 命令可用。' },
          { icon: 'key', title: '配置密钥', description: '把 API Key 和 Base URL 设置到当前 shell 或配置文件。' },
          { icon: 'chat', title: '发起会话', description: '新建一次 Claude Code 会话，检查模型返回是否正常。' }
        ],
        manual: [
          '确认 claude 命令已安装。',
          '在 shell 配置中写入 ANTHROPIC_API_KEY 和 ANTHROPIC_BASE_URL。',
          '重新打开终端后运行 claude 验证。'
        ]
      },
      gemini: {
        shortTitle: 'Gemini CLI 配置教程',
        title: 'Gemini CLI 配置教程',
        description: '为 Gemini CLI 写入密钥、Base URL 和默认模型，适合命令行问答与文件分析。',
        icon: 'sparkles',
        configPath: '~/.gemini',
        commands: {
          windows: '$env:GEMINI_API_KEY="sk-..."\n$env:OPENAI_BASE_URL="https://your-domain.example/v1"\ngemini',
          unix: 'export GEMINI_API_KEY="sk-..."\nexport OPENAI_BASE_URL="https://your-domain.example/v1"\ngemini'
        },
        steps: [
          { icon: 'grid', title: '选择模型', description: '根据任务选择 Flash、Pro 或团队配置中的默认模型。' },
          { icon: 'key', title: '配置密钥', description: '把 API Key 写入本地配置，不要放到项目仓库。' },
          { icon: 'clipboard', title: '检查输出', description: '执行一次问答或文件分析，确认流式输出正常。' }
        ],
        manual: [
          '安装 Gemini CLI 并确认命令可用。',
          '在用户配置目录写入 API Key、Base URL 和模型名称。',
          '重新打开终端后运行一次 gemini 命令验证。'
        ]
      },
      openclaw: {
        shortTitle: 'OpenClaw 配置教程',
        title: 'OpenClaw 配置教程',
        description: '通过 OpenAI 兼容参数连接 Sub2API，适合代理工具和团队统一客户端配置。',
        icon: 'cloud',
        configPath: 'OpenAI compatible settings',
        commands: {
          windows: '$env:OPENAI_API_KEY="sk-..."\n$env:OPENAI_BASE_URL="https://your-domain.example/v1"',
          unix: 'export OPENAI_API_KEY="sk-..."\nexport OPENAI_BASE_URL="https://your-domain.example/v1"'
        },
        steps: [
          { icon: 'link', title: '填写 Base URL', description: '在 OpenClaw 的 OpenAI 兼容配置中填写平台地址。' },
          { icon: 'key', title: '填写密钥', description: '使用控制台创建的 API Key，不要复用管理员密钥。' },
          { icon: 'checkCircle', title: '测试连接', description: '保存后执行一次测试请求，确认模型列表和回复正常。' }
        ],
        manual: [
          '打开 OpenClaw 的 OpenAI 兼容提供商设置。',
          '填写 API Key、Base URL 和模型名称。',
          '保存后执行测试连接。'
        ]
      }
    }
  },
  en: {
    pageTitle: 'Docs',
    sidebar: {
      title: 'Quick start'
    },
    hero: {
      badge: 'Integration docs'
    },
    script: {
      title: 'Choose a setup command by system',
      description: 'The content structure follows APIKEY.FUN’s docs page: left navigation, platform switcher, command block, and manual fallback. Styling uses this project’s own cards and controls.',
      writeTo: 'Writes to: ',
      restart: ' and already-open CLIs usually need a restart before reading the new configuration.'
    },
    manual: {
      title: 'Fallback: manual configuration',
      description: 'Use this only when the command-based setup is unavailable.'
    },
    cta: {
      title: 'Already created an API key?',
      description: 'Open the dashboard to create or copy your key, then apply the command on this page to configure your client.',
      action: 'Open dashboard'
    },
    copy: 'Copy',
    copied: 'Copied',
    platforms: [
      { id: 'windows', label: 'Windows PowerShell' },
      { id: 'unix', label: 'macOS / Linux / WSL' }
    ],
    docs: {
      node: {
        shortTitle: 'Node.js setup',
        title: 'Node.js Environment Setup',
        description: 'Most CLI clients depend on Node.js or npm/npx. Confirm the runtime first, then configure your model client.',
        icon: 'cube',
        configPath: 'node --version',
        commands: {
          windows: 'winget install OpenJS.NodeJS.LTS\nnode --version\nnpm --version',
          unix: 'node --version\nnpm --version\n# If missing, install Node.js LTS through your system package manager or nvm'
        },
        steps: [
          { icon: 'download', title: 'Install Node.js', description: 'Use the LTS release to avoid client dependency issues.' },
          { icon: 'terminal', title: 'Check commands', description: 'Confirm node, npm, or npx are available in your terminal.' },
          { icon: 'checkCircle', title: 'Continue setup', description: 'After validation, choose Codex, Claude Code, or Gemini CLI.' }
        ],
        manual: [
          'Run node --version in your terminal.',
          'If the command is missing, install Node.js LTS and reopen the terminal.',
          'Confirm npm --version prints a version number.'
        ]
      },
      codex: {
        shortTitle: 'Codex setup',
        title: 'Codex (OpenAI) Setup',
        description: 'Configure Codex with an OpenAI-compatible Base URL to connect through this platform.',
        icon: 'cpu',
        configPath: '~/.codex',
        commands: {
          windows: '$env:OPENAI_API_KEY="sk-..."\n$env:OPENAI_BASE_URL="https://your-domain.example/v1"\ncodex',
          unix: 'export OPENAI_API_KEY="sk-..."\nexport OPENAI_BASE_URL="https://your-domain.example/v1"\ncodex'
        },
        steps: [
          { icon: 'copy', title: 'Copy command', description: 'The command does not contain a real API key and can be copied safely.' },
          { icon: 'key', title: 'Enter key', description: 'Set the API key from the dashboard as an environment variable. Do not put it in chat or logs.' },
          { icon: 'checkCircle', title: 'Verify call', description: 'Restart Codex CLI or Codex App and run a simple request.' }
        ],
        manual: [
          'Open the Codex config directory or terminal environment settings.',
          'Set OPENAI_API_KEY, OPENAI_BASE_URL, and the default model name.',
          'Restart the client and run codex once to verify.'
        ]
      },
      claude: {
        shortTitle: 'Claude Code setup',
        title: 'Claude Code Setup',
        description: 'Use the assigned key and compatible endpoint so Claude Code requests go through the unified gateway.',
        icon: 'beaker',
        configPath: '~/.claude',
        commands: {
          windows: '$env:ANTHROPIC_API_KEY="sk-..."\n$env:ANTHROPIC_BASE_URL="https://your-domain.example"\nclaude',
          unix: 'export ANTHROPIC_API_KEY="sk-..."\nexport ANTHROPIC_BASE_URL="https://your-domain.example"\nclaude'
        },
        steps: [
          { icon: 'terminal', title: 'Open terminal', description: 'Confirm Claude Code is installed and the claude command is available.' },
          { icon: 'key', title: 'Set key', description: 'Set API key and Base URL in the current shell or config file.' },
          { icon: 'chat', title: 'Start session', description: 'Create a Claude Code session and confirm the model responds.' }
        ],
        manual: [
          'Confirm the claude command is installed.',
          'Write ANTHROPIC_API_KEY and ANTHROPIC_BASE_URL into your shell config.',
          'Reopen the terminal and run claude to verify.'
        ]
      },
      gemini: {
        shortTitle: 'Gemini CLI setup',
        title: 'Gemini CLI Setup',
        description: 'Set key, Base URL, and default model for command-line chat and file analysis workflows.',
        icon: 'sparkles',
        configPath: '~/.gemini',
        commands: {
          windows: '$env:GEMINI_API_KEY="sk-..."\n$env:OPENAI_BASE_URL="https://your-domain.example/v1"\ngemini',
          unix: 'export GEMINI_API_KEY="sk-..."\nexport OPENAI_BASE_URL="https://your-domain.example/v1"\ngemini'
        },
        steps: [
          { icon: 'grid', title: 'Choose model', description: 'Select Flash, Pro, or the default model from your team configuration.' },
          { icon: 'key', title: 'Set key', description: 'Write the API key locally and never commit it to your project repository.' },
          { icon: 'clipboard', title: 'Check output', description: 'Run one chat or file-analysis command and confirm streaming output works.' }
        ],
        manual: [
          'Install Gemini CLI and confirm the command is available.',
          'Write API key, Base URL, and model name into the user config directory.',
          'Reopen the terminal and run gemini once to verify.'
        ]
      },
      openclaw: {
        shortTitle: 'OpenClaw setup',
        title: 'OpenClaw Setup',
        description: 'Connect through OpenAI-compatible settings for agent tools and team-wide client configuration.',
        icon: 'cloud',
        configPath: 'OpenAI compatible settings',
        commands: {
          windows: '$env:OPENAI_API_KEY="sk-..."\n$env:OPENAI_BASE_URL="https://your-domain.example/v1"',
          unix: 'export OPENAI_API_KEY="sk-..."\nexport OPENAI_BASE_URL="https://your-domain.example/v1"'
        },
        steps: [
          { icon: 'link', title: 'Set Base URL', description: 'Enter the platform endpoint in OpenClaw’s OpenAI-compatible provider settings.' },
          { icon: 'key', title: 'Set key', description: 'Use an API key created in the dashboard, not an administrator credential.' },
          { icon: 'checkCircle', title: 'Test connection', description: 'Save and run a test request to confirm model list and responses.' }
        ],
        manual: [
          'Open OpenClaw’s OpenAI-compatible provider settings.',
          'Enter API key, Base URL, and model name.',
          'Save and run a test connection.'
        ]
      }
    }
  }
} as const

const activeLocale = computed(() => locale.value === 'zh' ? 'zh' : 'en')
const copy = computed(() => docsCopy[activeLocale.value])
const docs = computed(() => [
  { id: 'node' as const, ...copy.value.docs.node },
  { id: 'codex' as const, ...copy.value.docs.codex },
  { id: 'claude' as const, ...copy.value.docs.claude },
  { id: 'gemini' as const, ...copy.value.docs.gemini },
  { id: 'openclaw' as const, ...copy.value.docs.openclaw }
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
