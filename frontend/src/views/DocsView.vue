<template>
  <div
    class="min-h-screen bg-gray-50 text-gray-900 dark:bg-dark-950 dark:text-gray-100"
  >
    <div class="pointer-events-none fixed inset-0 bg-mesh-gradient"></div>
    <MarketingHeader />

    <main
      class="relative mx-auto grid max-w-7xl gap-8 px-4 py-10 sm:px-6 lg:grid-cols-[280px_1fr] lg:px-8 lg:py-14"
    >
      <aside class="lg:sticky lg:top-24 lg:h-fit">
        <div class="card p-4">
          <p
            class="px-2 text-xs font-semibold uppercase tracking-wider text-gray-400 dark:text-dark-500"
          >
            {{ copy.sidebar.title }}
          </p>
          <nav class="mt-3 grid gap-1">
            <button
              v-for="doc in docs"
              :key="doc.id"
              type="button"
              class="flex w-full items-center gap-3 rounded-xl px-3 py-3 text-left text-sm font-medium transition-colors"
              :class="
                activeDoc === doc.id
                  ? 'bg-primary-50 text-primary-700 dark:bg-primary-900/20 dark:text-primary-300'
                  : 'text-gray-600 hover:bg-gray-100 hover:text-gray-950 dark:text-dark-300 dark:hover:bg-dark-800 dark:hover:text-white'
              "
              @click="activeDoc = doc.id"
            >
              <img
                :src="doc.logo"
                :alt="`${doc.shortTitle} logo`"
                class="h-4 w-4 object-contain"
              />
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
          <h1
            class="mt-5 text-4xl font-bold tracking-tight text-gray-950 dark:text-white sm:text-5xl"
          >
            {{ activeDocData.title }}
          </h1>
          <p
            class="mt-4 max-w-3xl text-base leading-8 text-gray-600 dark:text-dark-300"
          >
            {{ activeDocData.description }}
          </p>
        </div>

        <article class="card mt-8 overflow-hidden">
          <div class="card-header">
            <div
              class="flex flex-col gap-4 lg:flex-row lg:items-center lg:justify-between"
            >
              <div>
                <h2 class="text-xl font-semibold text-gray-950 dark:text-white">
                  {{ copy.script.title }}
                </h2>
                <p class="mt-1 text-sm text-gray-500 dark:text-dark-400">
                  {{ activeDocData.modelHint }}
                </p>
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
            <div
              class="overflow-hidden rounded-xl border border-gray-200 bg-gray-950 dark:border-dark-700"
            >
              <div
                class="flex items-center justify-between border-b border-white/10 px-4 py-3"
              >
                <span
                  class="text-xs font-semibold uppercase tracking-wider text-primary-300"
                >
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
              <pre
                class="overflow-x-auto p-4 text-sm leading-7 text-gray-100"
              ><code>{{ activeDocData.commands[activePlatform] }}</code></pre>
            </div>

            <div
              class="mt-5 rounded-xl border-l-4 border-primary-400 bg-primary-50 px-5 py-4 text-sm leading-7 text-gray-700 dark:bg-primary-900/20 dark:text-dark-200"
            >
              <span
                class="font-semibold text-primary-700 dark:text-primary-300"
                >{{ copy.script.baseUrl }}</span
              >
              <code
                class="break-all rounded bg-white px-2 py-1 font-mono text-primary-700 dark:bg-dark-800 dark:text-primary-300"
                >{{ activeDocData.baseUrl }}</code
              >
            </div>
          </div>
        </article>

        <section class="mt-8 grid gap-4 md:grid-cols-3">
          <article
            v-for="step in activeDocData.steps"
            :key="step.title"
            class="card p-6"
          >
            <div
              class="flex h-11 w-11 items-center justify-center rounded-xl bg-primary-100 text-primary-600 dark:bg-primary-900/30 dark:text-primary-300"
            >
              <Icon :name="step.icon" size="md" />
            </div>
            <h2 class="mt-4 font-semibold text-gray-950 dark:text-white">
              {{ step.title }}
            </h2>
            <p class="mt-2 text-sm leading-7 text-gray-500 dark:text-dark-400">
              {{ step.description }}
            </p>
          </article>
        </section>

        <section
          class="mt-8 overflow-hidden rounded-2xl bg-gradient-primary p-8 text-white shadow-glow-lg"
        >
          <div class="grid gap-5 md:grid-cols-[1fr_auto] md:items-center">
            <div>
              <h2 class="text-2xl font-bold tracking-tight">
                {{ copy.cta.title }}
              </h2>
              <p class="mt-3 max-w-2xl text-sm leading-7 text-primary-50">
                {{ copy.cta.description }}
              </p>
            </div>
            <router-link
              to="/dashboard"
              class="btn bg-white text-primary-700 hover:bg-primary-50"
            >
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
import { computed, ref, watchEffect } from "vue";
import { useI18n } from "vue-i18n";
import MarketingFooter from "@/components/marketing/MarketingFooter.vue";
import MarketingHeader from "@/components/marketing/MarketingHeader.vue";
import Icon from "@/components/icons/Icon.vue";
import { useAppStore } from "@/stores";
import claudeCodeLogo from "@/assets/icons/claudecode.svg";
import codexLogo from "@/assets/icons/codex.svg";
import hermesAgentLogo from "@/assets/icons/hermesagent.svg";
import openclawLogo from "@/assets/icons/openclaw.svg";

type DocId = "claude" | "openai" | "openclaw" | "hermes";
type PlatformId = "windows" | "unix";

const { locale } = useI18n();
const appStore = useAppStore();
const activeDoc = ref<DocId>("openai");
const activePlatform = ref<PlatformId>("windows");
const copied = ref(false);

const CLAUDE_BASE_URL = "https://api.xiaobocode.com";
const OPENAI_BASE_URL = "https://api.xiaobocode.com/v1";
const API_KEY_PLACEHOLDER = "sk-...";
const CLAUDE_MODELS =
  "claude-opus-4-7, claude-opus-4-6, claude-sonnet-4-6, claude-haiku-4-5";
const OPENAI_MODELS = "gpt-5.5, gpt-5.4, gpt-5.3-codex";

const claudeCodeCommands = {
  windows: `[System.Environment]::SetEnvironmentVariable("ANTHROPIC_BASE_URL", "${CLAUDE_BASE_URL}", [System.EnvironmentVariableTarget]::User)
[System.Environment]::SetEnvironmentVariable("ANTHROPIC_AUTH_TOKEN", "${API_KEY_PLACEHOLDER}", [System.EnvironmentVariableTarget]::User)
[System.Environment]::SetEnvironmentVariable("CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC", "1", [System.EnvironmentVariableTarget]::User)

claude`,
  unix: `export ANTHROPIC_BASE_URL="${CLAUDE_BASE_URL}"
export ANTHROPIC_AUTH_TOKEN="${API_KEY_PLACEHOLDER}"
export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1

claude`,
} as const;

const codexCommands = {
  windows: `$codexDir = Join-Path $env:USERPROFILE ".codex"
New-Item -ItemType Directory -Force -Path $codexDir | Out-Null

@"
model_provider = "OpenAI"
model = "gpt-5.5"
review_model = "gpt-5.5"
model_reasoning_effort = "high"
disable_response_storage = true
network_access = "enabled"
windows_wsl_setup_acknowledged = true
model_context_window = 270000
model_auto_compact_token_limit = 270000
effective_context_window_percent = 95

[model_providers.OpenAI]
name = "OpenAI"
base_url = "${OPENAI_BASE_URL}"
wire_api = "responses"
requires_openai_auth = true
"@ | Set-Content -Path (Join-Path $codexDir "config.toml") -Encoding UTF8

@"
{
  "OPENAI_API_KEY": "${API_KEY_PLACEHOLDER}"
}
"@ | Set-Content -Path (Join-Path $codexDir "auth.json") -Encoding UTF8

codex`,
  unix: `mkdir -p ~/.codex

cat > ~/.codex/config.toml << 'EOF'
model_provider = "OpenAI"
model = "gpt-5.5"
review_model = "gpt-5.5"
model_reasoning_effort = "high"
disable_response_storage = true
network_access = "enabled"
windows_wsl_setup_acknowledged = true
model_context_window = 270000
model_auto_compact_token_limit = 270000
effective_context_window_percent = 95

[model_providers.OpenAI]
name = "OpenAI"
base_url = "${OPENAI_BASE_URL}"
wire_api = "responses"
requires_openai_auth = true
EOF

cat > ~/.codex/auth.json << 'EOF'
{
  "OPENAI_API_KEY": "${API_KEY_PLACEHOLDER}"
}
EOF

codex`,
} as const;

const openClawCommands = {
  windows: `npm install -g @openclaw/cli

$env:ANTHROPIC_API_KEY="${API_KEY_PLACEHOLDER}"
openclaw onboard --auth-choice custom-api-key --custom-base-url ${CLAUDE_BASE_URL} --custom-api-key-env ANTHROPIC_API_KEY --custom-compatibility anthropic --custom-model claude-opus-4-6

$env:OPENAI_API_KEY="${API_KEY_PLACEHOLDER}"
openclaw onboard --auth-choice custom-api-key --custom-base-url ${OPENAI_BASE_URL} --custom-api-key-env OPENAI_API_KEY --custom-compatibility openai --custom-model gpt-5.5

openclaw`,
  unix: `npm install -g @openclaw/cli

export ANTHROPIC_API_KEY="${API_KEY_PLACEHOLDER}"
openclaw onboard --auth-choice custom-api-key \\
  --custom-base-url ${CLAUDE_BASE_URL} \\
  --custom-api-key-env ANTHROPIC_API_KEY \\
  --custom-compatibility anthropic \\
  --custom-model claude-opus-4-6

export OPENAI_API_KEY="${API_KEY_PLACEHOLDER}"
openclaw onboard --auth-choice custom-api-key \\
  --custom-base-url ${OPENAI_BASE_URL} \\
  --custom-api-key-env OPENAI_API_KEY \\
  --custom-compatibility openai \\
  --custom-model gpt-5.5

openclaw`,
} as const;

const hermesCommands = {
  windows: `$hermesDir = Join-Path $env:USERPROFILE ".hermes"
New-Item -ItemType Directory -Force -Path $hermesDir | Out-Null

@"
model:
  default: claude-opus-4-7
  provider: xiaobocode-claude
providers:
  xiaobocode-claude:
    api_mode: anthropic_messages
    base_url: ${CLAUDE_BASE_URL}
    api_key: ${API_KEY_PLACEHOLDER}
    default_model: claude-opus-4-7
    models:
      - claude-opus-4-7
  xiaobocode-openai:
    api_mode: openai_responses
    base_url: ${OPENAI_BASE_URL}
    api_key: ${API_KEY_PLACEHOLDER}
    default_model: gpt-5.5
    models:
      - gpt-5.5
"@ | Set-Content -Path (Join-Path $hermesDir "config.yaml") -Encoding UTF8

hermes`,
  unix: `mkdir -p ~/.hermes

cat > ~/.hermes/config.yaml << 'EOF'
model:
  default: claude-opus-4-7
  provider: xiaobocode-claude
providers:
  xiaobocode-claude:
    api_mode: anthropic_messages
    base_url: https://api.xiaobocode.com
    api_key: sk-...
    default_model: claude-opus-4-7
    models:
      - claude-opus-4-7
  xiaobocode-openai:
    api_mode: openai_responses
    base_url: https://api.xiaobocode.com/v1
    api_key: sk-...
    default_model: gpt-5.5
    models:
      - gpt-5.5
EOF

hermes`,
} as const;

const docsCopy = {
  zh: {
    pageTitle: "接入文档",
    sidebar: {
      title: "接入平台",
    },
    hero: {
      badge: "接入文档",
    },
    script: {
      title: "手动配置命令",
      baseUrl: "Base URL：",
    },
    cta: {
      title: "已经创建好 API Key？",
      description:
        "进入控制台创建或复制密钥，再按本页命令写入对应客户端配置。",
      action: "进入控制台",
    },
    copy: "复制",
    copied: "已复制",
    platforms: [
      { id: "windows", label: "Windows PowerShell" },
      { id: "unix", label: "macOS / Linux / WSL" },
    ],
    docs: {
      claude: {
        shortTitle: "Claude Code",
        title: "Claude Code 配置教程",
        description:
          "为 Claude Code 写入 Anthropic 兼容地址和密钥，直接调用当前支持的 Claude 模型。",
        modelHint: `支持模型：${CLAUDE_MODELS}`,
        logo: claudeCodeLogo,
        baseUrl: CLAUDE_BASE_URL,
        commands: claudeCodeCommands,
        steps: [
          {
            icon: "key",
            title: "创建密钥",
            description: "在控制台创建 API Key，并替换命令中的 sk-...。",
          },
          {
            icon: "link",
            title: "配置地址",
            description: "Claude Code 使用根地址，不需要添加 /v1。",
          },
          {
            icon: "checkCircle",
            title: "重启终端",
            description: "Windows 写入用户环境变量后，重新打开终端再运行 claude。",
          },
        ],
      },
      openai: {
        shortTitle: "Codex",
        title: "Codex 配置教程",
        description:
          "为 Codex 写入 OpenAI Responses 兼容配置，使用当前支持的 GPT 模型。",
        modelHint: `支持模型：${OPENAI_MODELS}`,
        logo: codexLogo,
        baseUrl: OPENAI_BASE_URL,
        commands: codexCommands,
        steps: [
          {
            icon: "key",
            title: "创建密钥",
            description: "在控制台创建 API Key，并写入 auth.json。",
          },
          {
            icon: "link",
            title: "创建配置",
            description: "Codex 需要同时写入 config.toml 和 auth.json。",
          },
          {
            icon: "checkCircle",
            title: "开始使用",
            description: "进入项目目录后运行 codex。",
          },
        ],
      },
      openclaw: {
        shortTitle: "OpenClaw",
        title: "OpenClaw 配置教程",
        description:
          "通过 OpenClaw 自定义 API Key 接入，可按需选择 Claude 或 OpenAI 通道。",
        modelHint: `Claude：${CLAUDE_MODELS}；OpenAI：${OPENAI_MODELS}`,
        logo: openclawLogo,
        baseUrl: `Claude ${CLAUDE_BASE_URL} · OpenAI ${OPENAI_BASE_URL}`,
        commands: openClawCommands,
        steps: [
          {
            icon: "key",
            title: "安装 CLI",
            description: "先安装 @openclaw/cli，再执行 onboard 配置。",
          },
          {
            icon: "link",
            title: "选择通道",
            description: "Claude 通道使用根地址，OpenAI 通道必须带 /v1。",
          },
          {
            icon: "checkCircle",
            title: "开始使用",
            description: "配置完成后运行 openclaw。",
          },
        ],
      },
      hermes: {
        shortTitle: "Hermes",
        title: "Hermes 配置教程",
        description:
          "为 Hermes 写入 config.yaml，同时保留 Claude 与 OpenAI 两个 provider。",
        modelHint: `Claude：${CLAUDE_MODELS}；OpenAI：${OPENAI_MODELS}`,
        logo: hermesAgentLogo,
        baseUrl: `Claude ${CLAUDE_BASE_URL} · OpenAI ${OPENAI_BASE_URL}`,
        commands: hermesCommands,
        steps: [
          {
            icon: "key",
            title: "创建配置",
            description: "在用户目录写入 ~/.hermes/config.yaml。",
          },
          {
            icon: "link",
            title: "保留双通道",
            description: "Claude 使用根地址，OpenAI 使用 /v1 地址。",
          },
          {
            icon: "checkCircle",
            title: "开始使用",
            description: "配置完成后运行 hermes。",
          },
        ],
      },
    },
  },
  en: {
    pageTitle: "Docs",
    sidebar: {
      title: "Platforms",
    },
    hero: {
      badge: "Integration docs",
    },
    script: {
      title: "Manual configuration commands",
      baseUrl: "Base URL: ",
    },
    cta: {
      title: "Already created an API key?",
      description:
        "Open the dashboard to create or copy your key, then apply the command on this page.",
      action: "Open dashboard",
    },
    copy: "Copy",
    copied: "Copied",
    platforms: [
      { id: "windows", label: "Windows PowerShell" },
      { id: "unix", label: "macOS / Linux / WSL" },
    ],
    docs: {
      claude: {
        shortTitle: "Claude Code",
        title: "Claude Code Configuration",
        description:
          "Set the Anthropic-compatible endpoint and key for Claude Code.",
        modelHint: `Supported models: ${CLAUDE_MODELS}`,
        logo: claudeCodeLogo,
        baseUrl: CLAUDE_BASE_URL,
        commands: claudeCodeCommands,
        steps: [
          {
            icon: "key",
            title: "Create key",
            description: "Create an API key in the dashboard and replace sk-....",
          },
          {
            icon: "link",
            title: "Set endpoint",
            description: "Claude Code uses the root URL without /v1.",
          },
          {
            icon: "checkCircle",
            title: "Restart terminal",
            description: "On Windows, reopen the terminal after writing user env vars.",
          },
        ],
      },
      openai: {
        shortTitle: "Codex",
        title: "Codex Configuration",
        description:
          "Write OpenAI Responses-compatible settings for supported GPT models.",
        modelHint: `Supported models: ${OPENAI_MODELS}`,
        logo: codexLogo,
        baseUrl: OPENAI_BASE_URL,
        commands: codexCommands,
        steps: [
          {
            icon: "key",
            title: "Create key",
            description: "Create an API key and write it to auth.json.",
          },
          {
            icon: "link",
            title: "Create config",
            description: "Codex needs both config.toml and auth.json.",
          },
          {
            icon: "checkCircle",
            title: "Start using",
            description: "Run codex from your project directory.",
          },
        ],
      },
      openclaw: {
        shortTitle: "OpenClaw",
        title: "OpenClaw Configuration",
        description:
          "Connect OpenClaw through a custom API key using Claude or OpenAI mode.",
        modelHint: `Claude: ${CLAUDE_MODELS}; OpenAI: ${OPENAI_MODELS}`,
        logo: openclawLogo,
        baseUrl: `Claude ${CLAUDE_BASE_URL} · OpenAI ${OPENAI_BASE_URL}`,
        commands: openClawCommands,
        steps: [
          {
            icon: "key",
            title: "Install CLI",
            description: "Install @openclaw/cli before running onboard.",
          },
          {
            icon: "link",
            title: "Choose channel",
            description: "Claude uses the root URL; OpenAI requires /v1.",
          },
          {
            icon: "checkCircle",
            title: "Start using",
            description: "Run openclaw after configuration.",
          },
        ],
      },
      hermes: {
        shortTitle: "Hermes",
        title: "Hermes Configuration",
        description:
          "Write config.yaml with Claude and OpenAI providers for Hermes.",
        modelHint: `Claude: ${CLAUDE_MODELS}; OpenAI: ${OPENAI_MODELS}`,
        logo: hermesAgentLogo,
        baseUrl: `Claude ${CLAUDE_BASE_URL} · OpenAI ${OPENAI_BASE_URL}`,
        commands: hermesCommands,
        steps: [
          {
            icon: "key",
            title: "Create config",
            description: "Write ~/.hermes/config.yaml.",
          },
          {
            icon: "link",
            title: "Keep both channels",
            description: "Claude uses the root URL; OpenAI uses /v1.",
          },
          {
            icon: "checkCircle",
            title: "Start using",
            description: "Run hermes after configuration.",
          },
        ],
      },
    },
  },
} as const;

const activeLocale = computed(() => (locale.value === "zh" ? "zh" : "en"));
const copy = computed(() => docsCopy[activeLocale.value]);
const docs = computed(() => [
  { id: "openai" as const, ...copy.value.docs.openai },
  { id: "claude" as const, ...copy.value.docs.claude },
  { id: "openclaw" as const, ...copy.value.docs.openclaw },
  { id: "hermes" as const, ...copy.value.docs.hermes },
]);
const activeDocData = computed(() => copy.value.docs[activeDoc.value]);
const platforms = computed(() => copy.value.platforms);
const activePlatformLabel = computed(
  () =>
    platforms.value.find((item) => item.id === activePlatform.value)?.label ||
    "",
);
const copyButtonText = computed(() =>
  copied.value ? copy.value.copied : copy.value.copy,
);
const siteName = computed(
  () =>
    appStore.cachedPublicSettings?.site_name || appStore.siteName || "Sub2API",
);

watchEffect(() => {
  document.title = `${copy.value.pageTitle} - ${siteName.value}`;
});

async function copyCommand(): Promise<void> {
  await navigator.clipboard?.writeText(
    activeDocData.value.commands[activePlatform.value],
  );
  copied.value = true;
  window.setTimeout(() => {
    copied.value = false;
  }, 1500);
}
</script>
