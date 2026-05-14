<template>
  <div
    class="min-h-screen bg-gray-50 text-gray-900 dark:bg-dark-950 dark:text-gray-100"
  >
    <div class="pointer-events-none fixed inset-0 bg-mesh-gradient"></div>
    <MarketingHeader />

    <main
      class="relative mx-auto grid max-w-7xl items-start gap-8 px-4 py-10 sm:px-6 lg:grid-cols-[280px_1fr] lg:px-8 lg:py-14"
    >
      <aside class="lg:sticky lg:top-24 lg:self-start">
        <div class="card docs-toc-card p-4">
          <p
            class="px-2 text-xs font-semibold uppercase tracking-wider text-gray-400 dark:text-dark-500"
          >
            文档目录
          </p>
          <nav class="docs-toc-nav mt-3 grid gap-3">
            <div v-for="group in navGroups" :key="group.title" class="docs-toc-group">
              <p
                class="px-2 pb-1 text-[11px] font-semibold uppercase tracking-wider text-gray-400 dark:text-dark-500"
              >
                {{ group.title }}
              </p>
              <div class="grid gap-1">
                <a
                  v-for="item in group.items"
                  :key="item.id"
                  :href="`#${item.id}`"
                  class="rounded-xl px-3 py-2 text-sm font-medium text-gray-600 transition-colors hover:bg-gray-100 hover:text-gray-950 dark:text-dark-300 dark:hover:bg-dark-800 dark:hover:text-white"
                >
                  {{ item.title }}
                </a>
              </div>
            </div>
          </nav>
        </div>
      </aside>

      <section class="min-w-0">
        <article class="card docs-content overflow-hidden p-6 sm:p-8">
<section id="overview">
    <h2>
      <span class="hero-badge">开发者文档 v1.0</span>
    </h2>
    <h1>一站式 AI 大模型 API 网关</h1>
    <p class="hero-desc">
      智脑API平台为开发者提供统一的 OpenAI 兼容接口，聚合 GPT、Claude、o 系列等主流大模型。
      只需更换 Base URL，即可在现有项目中无缝切换和使用多种顶级 AI 模型，无需分别对接不同厂商。
    </p>

    <div class="feature-grid">
      <div class="feature-card">
        <div class="feature-icon teal">&#9889;</div>
        <h4>多模型聚合</h4>
        <p>一个 API Key 访问 GPT-5.5、GPT Image 2、Claude Opus 4 等 20+ 顶级模型，无需分别注册多家服务。</p>
      </div>
      <div class="feature-card">
        <div class="feature-icon cyan">&#128737;</div>
        <h4>高可用架构</h4>
        <p>多节点自动负载均衡与故障转移，保障 API 全天候稳定运行，SLA 可达 99.9%。</p>
      </div>
      <div class="feature-card">
        <div class="feature-icon green">&#128176;</div>
        <h4>灵活计费</h4>
        <p>按量充值与套餐订阅并行，充值比例 ¥1 = $1 余额，最低充值 10 元，当前提供 Codex / OpenAI 与 Claude 系列套餐。</p>
      </div>
      <div class="feature-card">
        <div class="feature-icon orange">&#128296;</div>
        <h4>OpenAI 兼容</h4>
        <p>完全兼容 OpenAI SDK 接口规范，现有代码只需修改 base_url 即可接入，迁移成本为零。</p>
      </div>
    </div>
  </section>

  <!-- Quick Start -->
  <section id="quickstart">
    <h2>快速开始</h2>
    <p>只需三步，即可完成接入并发起第一次 AI 对话请求。</p>

    <div class="steps">
      <div class="step">
        <h4>注册账号并登录</h4>
        <p>访问 <a href="https://xiaobocode.com" target="_blank" rel="noreferrer">https://xiaobocode.com</a>，完成账号注册。注册后即可进入控制台。</p>
      </div>
      <div class="step">
        <h4>获取 API Key</h4>
        <p>在控制台的「令牌管理」页面，点击「创建新令牌」。系统将生成一个以 <code>sk-</code> 开头的密钥，请妥善保管——该密钥创建后仅显示一次。</p>
        <div class="callout callout-warn">
          <div class="callout-title">&#9888; 安全提示</div>
          <p>API Key 是您账户的访问凭证，请勿在客户端代码、公开仓库或聊天中泄露。如不慎泄露，请立即在控制台删除并重新生成。</p>
        </div>
      </div>
      <div class="step">
        <h4>发起第一次请求</h4>
        <p>使用以下 cURL 命令测试连通性，将 <code>sk-YOUR_API_KEY</code> 替换为您的实际密钥：</p>
        <div class="code-block">
          <div class="code-header">
            <span class="code-lang">bash</span>
            <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
          </div>
<pre><code><span class="var">curl</span> https://api.xiaobocode.com/v1/chat/completions \
  <span class="op">-H</span> <span class="str">"Content-Type: application/json"</span> \
  <span class="op">-H</span> <span class="str">"Authorization: Bearer sk-YOUR_API_KEY"</span> \
  <span class="op">-d</span> <span class="str">'{
    "model": "gpt-5.5",
    "messages": [
      {"role": "user", "content": "你好，请介绍一下你自己。"}
    ]
  }'</span></code></pre>
        </div>
        <p>如果返回了包含 <code>choices</code> 字段的 JSON 响应，说明接入成功。</p>
      </div>
    </div>
  </section>

  <!-- API Basics -->
  <section id="api-basics">
    <h2>API 基础信息</h2>

    <div class="callout callout-warn">
      <div class="callout-title">&#9888; API 请求请使用专用域名</div>
      <p>官网和用户后台访问 <code>https://xiaobocode.com</code>；终端、插件、SDK、Codex、Claude Code 等所有 API 调用请使用 <code>https://api.xiaobocode.com</code>。<code>xiaobocode.com</code> 与 <code>www.xiaobocode.com</code> 已接入 CDN，只适合网页和静态资源，不适合 <code>/v1/responses</code>、<code>/v1/messages</code> 这类流式长连接。继续使用官网域名调用 API，可能出现 408、504、重连或空转。</p>
    </div>
    <h3>接入地址</h3>
    <div class="table-wrapper">
      <table>
        <tr><th>配置项</th><th>值</th></tr>
        <tr><td>Base URL</td><td><code>https://api.xiaobocode.com/v1</code></td></tr>
        <tr><td>Chat Completions</td><td><code>https://api.xiaobocode.com/v1/chat/completions</code></td></tr>
        <tr><td>Responses</td><td><code>https://api.xiaobocode.com/v1/responses</code></td></tr>
        <tr><td>Images</td><td><code>https://api.xiaobocode.com/v1/images/generations</code> / <code>https://api.xiaobocode.com/v1/images/edits</code></td></tr>
        <tr><td>协议</td><td>HTTPS（必须）</td></tr>
        <tr><td>请求格式</td><td>JSON（<code>Content-Type: application/json</code>）</td></tr>
      </table>
    </div>

    <h3>身份认证</h3>
    <p>所有 API 请求均需在 HTTP 请求头中携带 Bearer Token 进行身份验证：</p>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">http header</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code>Authorization: Bearer sk-YOUR_API_KEY</code></pre>
    </div>

    <div class="callout callout-info">
      <div class="callout-title">&#128161; 提示</div>
      <p>本平台完全兼容 OpenAI API 规范。如果您的项目已经使用了 OpenAI 官方 SDK，只需将 <code>base_url</code> 修改为 <code>https://api.xiaobocode.com/v1</code>，并将 <code>api_key</code> 替换为本平台的密钥即可。</p>
    </div>
  </section>

  <!-- Models -->
  <section id="models">
    <h2>支持的模型</h2>
    <p>平台当前支持以下模型。对话模型可通过 <code>/v1/chat/completions</code> 调用；<code>gpt-image-2</code> 支持 <code>/v1/images/generations</code>、<code>/v1/images/edits</code> 和 Responses 图像生成工具，图片能力仅面向按量充值余额用户开放，订阅套餐不可使用 image2 模型。</p>

    <h3>Codex / OpenAI 系列</h3>
    <div class="table-wrapper">
      <table>
        <tr><th>模型标识</th><th>类型</th><th>说明</th><th>适用场景</th></tr>
        <tr>
          <td><code>gpt-5.5</code></td>
          <td><span class="model-badge badge-gpt">GPT</span></td>
          <td>当前旗舰模型，综合能力最强</td>
          <td>复杂推理、长文档分析、专业写作</td>
        </tr>
        <tr>
          <td><code>gpt-5.4</code></td>
          <td><span class="model-badge badge-gpt">GPT</span></td>
          <td>高性能旗舰模型，稳定可靠</td>
          <td>通用对话、复杂分析、生产环境调用</td>
        </tr>
        <tr>
          <td><code>gpt-5.4-mini</code></td>
          <td><span class="model-badge badge-gpt">GPT</span></td>
          <td>5.4 系列轻量版，性价比高</td>
          <td>日常对话、内容生成、翻译</td>
        </tr>
        <tr>
          <td><code>gpt-5.4-nano</code></td>
          <td><span class="model-badge badge-gpt">GPT</span></td>
          <td>5.4 系列极速版，响应最快</td>
          <td>高并发场景、简单问答、分类</td>
        </tr>
        <tr>
          <td><code>gpt-5.3-codex</code></td>
          <td><span class="model-badge badge-gpt">GPT</span></td>
          <td>代码专精模型</td>
          <td>代码生成、Debug、代码审查</td>
        </tr>
        <tr>
          <td><code>gpt-image-2</code></td>
          <td><span class="model-badge badge-gpt">Image</span></td>
          <td>图像生成与编辑模型，按量余额扣费</td>
          <td>文生图、图片编辑、多图创作</td>
        </tr>
        <tr>
          <td><code>gpt-5.2</code></td>
          <td><span class="model-badge badge-gpt">GPT</span></td>
          <td>上一代旗舰，能力均衡</td>
          <td>通用对话、文案创作</td>
        </tr>
        <tr>
          <td><code>gpt-5.1</code></td>
          <td><span class="model-badge badge-gpt">GPT</span></td>
          <td>稳定可靠，长期验证</td>
          <td>生产环境稳定调用</td>
        </tr>
        <tr>
          <td><code>gpt-5</code></td>
          <td><span class="model-badge badge-gpt">GPT</span></td>
          <td>GPT-5 基础版</td>
          <td>通用场景</td>
        </tr>
        <tr>
          <td><code>gpt-4o</code></td>
          <td><span class="model-badge badge-gpt">GPT</span></td>
          <td>多模态旗舰，支持图文</td>
          <td>图片理解、多模态分析</td>
        </tr>
        <tr>
          <td><code>gpt-4o-mini</code></td>
          <td><span class="model-badge badge-gpt">GPT</span></td>
          <td>4o 轻量版，成本更低</td>
          <td>轻量多模态、日常对话</td>
        </tr>
        <tr>
          <td><code>gpt-4-turbo</code></td>
          <td><span class="model-badge badge-gpt">GPT</span></td>
          <td>GPT-4 加速版</td>
          <td>兼容旧项目、稳定生产</td>
        </tr>
        <tr>
          <td><code>o4-mini</code></td>
          <td><span class="model-badge badge-o">o系列</span></td>
          <td>o4 系列轻量推理模型</td>
          <td>快速推理、数学计算</td>
        </tr>
        <tr>
          <td><code>o3</code></td>
          <td><span class="model-badge badge-o">o系列</span></td>
          <td>高级推理模型</td>
          <td>数学证明、逻辑推理、科研</td>
        </tr>
        <tr>
          <td><code>o3-mini</code></td>
          <td><span class="model-badge badge-o">o系列</span></td>
          <td>o3 轻量版</td>
          <td>中等推理任务、编程辅助</td>
        </tr>
        <tr>
          <td><code>o3-pro</code></td>
          <td><span class="model-badge badge-o">o系列</span></td>
          <td>o3 专业版，推理天花板</td>
          <td>竞赛级难题、深度研究</td>
        </tr>
        <tr>
          <td><code>o1</code></td>
          <td><span class="model-badge badge-o">o系列</span></td>
          <td>初代推理模型</td>
          <td>逻辑推理、策略规划</td>
        </tr>
        <tr>
          <td><code>o1-mini</code></td>
          <td><span class="model-badge badge-o">o系列</span></td>
          <td>o1 轻量版</td>
          <td>轻量推理场景</td>
        </tr>
        <tr>
          <td><code>o1-pro</code></td>
          <td><span class="model-badge badge-o">o系列</span></td>
          <td>o1 增强版</td>
          <td>复杂多步推理</td>
        </tr>
      </table>
    </div>

    <h3>Claude 系列</h3>
    <div class="table-wrapper">
      <table>
        <tr><th>模型标识</th><th>类型</th><th>说明</th><th>适用场景</th></tr>
        <tr>
          <td><code>claude-opus-4-7</code></td>
          <td><span class="model-badge badge-claude">Claude</span></td>
          <td>Anthropic 最新旗舰，最强推理能力</td>
          <td>复杂分析、深度推理、专业创作</td>
        </tr>
        <tr>
          <td><code>claude-opus-4-6</code></td>
          <td><span class="model-badge badge-claude">Claude</span></td>
          <td>Opus 4.6，超长上下文</td>
          <td>长文处理、研究辅助</td>
        </tr>
        <tr>
          <td><code>claude-opus-4-5-20251101</code></td>
          <td><span class="model-badge badge-claude">Claude</span></td>
          <td>Opus 4.5，综合性能卓越</td>
          <td>深度对话、研究辅助</td>
        </tr>
        <tr>
          <td><code>claude-sonnet-4-6</code></td>
          <td><span class="model-badge badge-claude">Claude</span></td>
          <td>Sonnet 最新版，能力与速度兼顾</td>
          <td>日常开发、代码辅助、文案</td>
        </tr>
        <tr>
          <td><code>claude-sonnet-4-5-20250929</code></td>
          <td><span class="model-badge badge-claude">Claude</span></td>
          <td>Sonnet 4.5，性价比优秀</td>
          <td>批量处理、内容生成</td>
        </tr>
        <tr>
          <td><code>claude-haiku-4-5-20251001</code></td>
          <td><span class="model-badge badge-claude">Claude</span></td>
          <td>Haiku 极速版，响应飞快</td>
          <td>高并发对话、快速分类、摘要</td>
        </tr>
      </table>
    </div>

    <div class="callout callout-tip">
      <div class="callout-title">&#128218; 模型选择建议</div>
      <p>追求最佳效果选 <code>gpt-5.5</code> 或 <code>claude-opus-4-7</code>；注重性价比选 <code>gpt-5.4-mini</code> 或 <code>claude-sonnet-4-6</code>；需要图像生成或编辑且使用按量余额时选 <code>gpt-image-2</code>；高并发低成本选 <code>gpt-5.4-nano</code> 或 <code>claude-haiku-4-5-20251001</code>。</p>
    </div>
  </section>

  <!-- Request Format -->
  <section id="request-format">
    <h2>请求格式</h2>
    <p>平台采用与 OpenAI 完全兼容的请求格式。文本对话核心端点为 <code>POST /v1/chat/completions</code>，图片生成核心端点为 <code>POST /v1/images/generations</code>。</p>

    <h3>请求参数</h3>
    <div class="table-wrapper">
      <table>
        <tr><th>参数</th><th>类型</th><th>必填</th><th>说明</th></tr>
        <tr><td><code>model</code></td><td>string</td><td>是</td><td>模型标识，如 <code>gpt-5.5</code>、<code>gpt-image-2</code>、<code>claude-opus-4-6</code></td></tr>
        <tr><td><code>messages</code></td><td>array</td><td>是</td><td>对话消息数组，每条包含 <code>role</code> 和 <code>content</code></td></tr>
        <tr><td><code>temperature</code></td><td>number</td><td>否</td><td>采样温度，范围 0-2，默认 1。数值越低输出越确定</td></tr>
        <tr><td><code>max_tokens</code></td><td>integer</td><td>否</td><td>最大输出 token 数</td></tr>
        <tr><td><code>stream</code></td><td>boolean</td><td>否</td><td>是否启用流式输出，默认 false</td></tr>
        <tr><td><code>top_p</code></td><td>number</td><td>否</td><td>核采样概率，范围 0-1</td></tr>
        <tr><td><code>frequency_penalty</code></td><td>number</td><td>否</td><td>频率惩罚，范围 -2 到 2</td></tr>
        <tr><td><code>presence_penalty</code></td><td>number</td><td>否</td><td>存在惩罚，范围 -2 到 2</td></tr>
      </table>
    </div>

    <h3>消息角色</h3>
    <div class="table-wrapper">
      <table>
        <tr><th>角色</th><th>说明</th></tr>
        <tr><td><code>system</code></td><td>系统指令，设定 AI 的行为方式和背景。放在消息数组的第一条。</td></tr>
        <tr><td><code>user</code></td><td>用户发送的消息内容。</td></tr>
        <tr><td><code>assistant</code></td><td>AI 助手的回复，用于提供对话历史上下文。</td></tr>
      </table>
    </div>

    <h3>完整请求示例</h3>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">json</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code>{
  <span class="prop">"model"</span>: <span class="str">"gpt-5.5"</span>,
  <span class="prop">"messages"</span>: [
    {
      <span class="prop">"role"</span>: <span class="str">"system"</span>,
      <span class="prop">"content"</span>: <span class="str">"你是一位专业的技术文档撰写助手。"</span>
    },
    {
      <span class="prop">"role"</span>: <span class="str">"user"</span>,
      <span class="prop">"content"</span>: <span class="str">"请帮我写一段 Python 快速排序代码。"</span>
    }
  ],
  <span class="prop">"temperature"</span>: <span class="num">0.7</span>,
  <span class="prop">"max_tokens"</span>: <span class="num">2048</span>,
  <span class="prop">"stream"</span>: <span class="kw">false</span>
}</code></pre>
    </div>

    <h3>GPT Image 2 图片生成示例</h3>
    <p>生成图片时使用 <code>gpt-image-2</code> 模型和 Images 端点。图片编辑可使用 <code>/v1/images/edits</code>。</p>
    <div class="callout callout-warn">
      <div class="callout-title">&#9888; 图片模型计费说明</div>
      <p><code>gpt-image-2</code> 仅支持按量余额扣费，不包含在 Codex / OpenAI 或 Claude 订阅套餐额度内，套餐用户不可调用 image2。当前扣费标准为：1K / 2K / 4K 图片统一扣 $0.15 余额；具体以支付页和调用结果为准。</p>
    </div>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">bash</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code><span class="var">curl</span> https://api.xiaobocode.com/v1/images/generations \
  <span class="op">-H</span> <span class="str">"Content-Type: application/json"</span> \
  <span class="op">-H</span> <span class="str">"Authorization: Bearer sk-YOUR_API_KEY"</span> \
  <span class="op">-d</span> <span class="str">'{
    "model": "gpt-image-2",
    "prompt": "一张未来感城市夜景，霓虹灯，电影感构图",
    "size": "1024x1024",
    "quality": "high",
    "n": 1
  }'</span></code></pre>
    </div>
  </section>

  <!-- Response Format -->
  <section id="response-format">
    <h2>响应格式</h2>

    <h3>标准响应（非流式）</h3>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">json</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code>{
  <span class="prop">"id"</span>: <span class="str">"chatcmpl-abc123def456"</span>,
  <span class="prop">"object"</span>: <span class="str">"chat.completion"</span>,
  <span class="prop">"created"</span>: <span class="num">1781654400</span>,
  <span class="prop">"model"</span>: <span class="str">"gpt-5.5"</span>,
  <span class="prop">"choices"</span>: [
    {
      <span class="prop">"index"</span>: <span class="num">0</span>,
      <span class="prop">"message"</span>: {
        <span class="prop">"role"</span>: <span class="str">"assistant"</span>,
        <span class="prop">"content"</span>: <span class="str">"以下是 Python 快速排序的实现..."</span>
      },
      <span class="prop">"finish_reason"</span>: <span class="str">"stop"</span>
    }
  ],
  <span class="prop">"usage"</span>: {
    <span class="prop">"prompt_tokens"</span>: <span class="num">42</span>,
    <span class="prop">"completion_tokens"</span>: <span class="num">256</span>,
    <span class="prop">"total_tokens"</span>: <span class="num">298</span>
  }
}</code></pre>
    </div>

    <h3>流式响应（SSE）</h3>
    <p>当请求中设置 <code>"stream": true</code> 时，响应将以 Server-Sent Events 格式逐块返回：</p>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">text</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code>data: {"id":"chatcmpl-abc123","object":"chat.completion.chunk","choices":[{"index":0,"delta":{"role":"assistant"},"finish_reason":null}]}

data: {"id":"chatcmpl-abc123","object":"chat.completion.chunk","choices":[{"index":0,"delta":{"content":"以下"},"finish_reason":null}]}

data: {"id":"chatcmpl-abc123","object":"chat.completion.chunk","choices":[{"index":0,"delta":{"content":"是"},"finish_reason":null}]}

data: [DONE]</code></pre>
    </div>
    <p>每个 <code>data:</code> 行包含一个 JSON 对象，<code>delta.content</code> 字段为本次增量输出的文本片段。当收到 <code>data: [DONE]</code> 时，表示生成结束。</p>
  </section>

  <!-- Error Codes -->
  <section id="error-codes">
    <h2>错误码说明</h2>
    <p>当请求发生错误时，API 将返回对应的 HTTP 状态码和 JSON 格式的错误信息：</p>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">json</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code>{
  <span class="prop">"error"</span>: {
    <span class="prop">"message"</span>: <span class="str">"具体的错误描述信息"</span>,
    <span class="prop">"type"</span>: <span class="str">"error_type"</span>,
    <span class="prop">"code"</span>: <span class="str">"error_code"</span>
  }
}</code></pre>
    </div>

    <div class="table-wrapper">
      <table>
        <tr><th>状态码</th><th>含义</th><th>说明与处理建议</th></tr>
        <tr>
          <td><span class="status-4xx">400</span></td>
          <td>请求参数错误</td>
          <td>请求体格式不正确或缺少必需参数。请检查 JSON 格式、<code>model</code> 字段是否正确、<code>messages</code> 数组是否为空。</td>
        </tr>
        <tr>
          <td><span class="status-4xx">401</span></td>
          <td>身份认证失败</td>
          <td>API Key 无效、已过期或未提供。请检查 <code>Authorization</code> 请求头格式是否为 <code>Bearer sk-xxx</code>。</td>
        </tr>
        <tr>
          <td><span class="status-4xx">403</span></td>
          <td>权限不足</td>
          <td>当前 API Key 无权访问所请求的模型或资源。请确认账户余额充足且未被禁用。</td>
        </tr>
        <tr>
          <td><span class="status-4xx">429</span></td>
          <td>请求过于密集</td>
          <td>短时间内请求过多。建议实现指数退避重试策略，或适当降低并发数。通常等待数秒后重试即可。</td>
        </tr>
        <tr>
          <td><span class="status-5xx">500</span></td>
          <td>服务器内部错误</td>
          <td>服务端出现未预期的异常。此类错误通常为暂时性故障，建议稍后重试。如持续出现，请联系技术支持。</td>
        </tr>
        <tr>
          <td><span class="status-5xx">503</span></td>
          <td>服务暂时不可用</td>
          <td>模型服务暂时过载或维护中。系统将自动进行负载均衡切换，建议等待片刻后重试。</td>
        </tr>
      </table>
    </div>

    <div class="callout callout-tip">
      <div class="callout-title">&#9881; 重试建议</div>
      <p>对于 429、500、503 错误，建议实现自动重试机制：首次等待 1 秒，之后每次翻倍（1s → 2s → 4s），最多重试 3 次。大部分暂时性故障可在数秒内自动恢复。</p>
    </div>
  </section>

  <!-- Pricing -->
  <section id="pricing">
    <h2>计费说明</h2>

    <h3>充值比例</h3>
    <p>平台采用美元余额计费制度，人民币充值采用 1:1 比例：</p>
    <div class="callout callout-info">
      <div class="callout-title">&#128178; 充值比例</div>
      <p><strong>&#165;1 人民币 = $1 美元余额</strong>（1:1），最低充值 10 元。充值后余额以美元显示，API 调用按照模型的 token 消耗从余额中扣除。</p>
    </div>

    <h3>分组倍率说明</h3>
    <p>平台同时支持按量充值和套餐订阅。按量充值按 API Key 所属分组倍率扣费，套餐订阅使用套餐分组额度：</p>
    <div class="table-wrapper">
      <table>
        <tr><th>分组</th><th>倍率</th><th>含义</th><th>适用场景</th></tr>
        <tr><td>OpenAI 默认分组</td><td>0.3x</td><td>文本模型按分组倍率扣费</td><td>按量使用 Codex / OpenAI 模型</td></tr>
        <tr><td>Claude 逆向分组</td><td>0.4x</td><td>Claude 逆向按量分组倍率</td><td>按量使用 Claude 逆向模型</td></tr>
        <tr><td>套餐分组</td><td>1x</td><td>套餐额度按 1:1 扣减</td><td>Codex / Claude 套餐订阅用户</td></tr>
      </table>
    </div>

    <h3>Codex / OpenAI 模型计费</h3>
    <p>GPT 系列、o 系列和 Codex 常用模型支持按量充值与套餐订阅两种方式。按量充值时，系统会按 API Key 所属分组倍率扣费；购买套餐后，套餐额度按 <strong>1x 倍率</strong> 从对应套餐分组扣减。</p>
    <p>例如：您充值 &#165;10 获得 $10 余额，可按量调用已开放模型；购买标准月卡后，在 30 天有效期内可使用 $3000 套餐额度，每日上限 $100。</p>

    <h3>Claude 模型计费</h3>
    <p>Claude 系列模型当前提供周卡、标准月卡和重度月卡三种专属套餐，适合使用 Claude Code、Claude 插件或 Claude 兼容客户端的用户。具体可用模型和套餐状态以控制台实际展示为准。</p>

    <div class="callout callout-warn">
      <div class="callout-title">&#9888; Claude 模型费率说明</div>
      <p>Claude 套餐当前支持 Claude 4.6 系列模型。由于上游模型和账号状态可能调整，购买前请以控制台套餐页、渠道状态和模型可用提示为准。</p>
    </div>

    <h3 id="pricing-compare">订阅套餐</h3>
    <p>当前在售套餐包含 Codex / OpenAI 系列和 Claude 系列，适合有稳定用量的用户。套餐内额度在有效期内可用，具体可用模型、额度和规则以购买页展示为准。订阅套餐不包含 image2 图片模型，图片生成和图片编辑请使用按量充值余额。</p>
    <div class="callout callout-warn">
      <div class="callout-title">&#9888; 订阅套餐退款说明</div>
      <p>订阅套餐属于即时生效的虚拟服务，购买成功后系统会立即发放套餐额度并开始计算有效期。已购买、已激活的订阅套餐不支持退款、退换或折算为按量余额；套餐内未使用额度可在有效期内继续使用，过期后不再结转。</p>
    </div>

    <h4>Codex / OpenAI 套餐（1x 倍率）</h4>
    <div class="pricing-grid">
      <div class="pricing-card">
        <div class="pricing-name">codex周卡</div>
        <div class="pricing-price">&#165;68<span>/7天</span></div>
        <div class="pricing-quota">$700 额度 | 日限 $100</div>
        <div class="pricing-divider"></div>
        <div class="pricing-detail">一周开发冲刺、短期项目</div>
      </div>
      <div class="pricing-card recommended">
        <div class="pricing-name">标准月卡</div>
        <div class="pricing-price">&#165;198<span>/30天</span></div>
        <div class="pricing-quota">$3000 额度 | 日限 $100</div>
        <div class="pricing-divider"></div>
        <div class="pricing-detail">日常开发首选，适合持续使用</div>
      </div>
      <div class="pricing-card">
        <div class="pricing-name">codex高级月卡</div>
        <div class="pricing-price">&#165;298<span>/30天</span></div>
        <div class="pricing-quota">$6000 额度 | 日限 $200</div>
        <div class="pricing-divider"></div>
        <div class="pricing-detail">重度开发、团队协作和持续高用量</div>
      </div>
    </div>

    <h4>Claude 套餐</h4>
    <div class="pricing-grid">
      <div class="pricing-card">
        <div class="pricing-name">Claude 周卡</div>
        <div class="pricing-price">&#165;29.9<span>/7天</span></div>
        <div class="pricing-quota">$100 额度 | 支持 Claude 4.6</div>
        <div class="pricing-divider"></div>
        <div class="pricing-detail">短期体验 Claude Code 与 Claude 模型</div>
      </div>
      <div class="pricing-card recommended">
        <div class="pricing-name">Claude 标准月卡</div>
        <div class="pricing-price">&#165;88<span>/30天</span></div>
        <div class="pricing-quota">$300 额度 | 支持 Claude 4.6</div>
        <div class="pricing-divider"></div>
        <div class="pricing-detail">日常 Claude 编程、插件和对话使用</div>
      </div>
      <div class="pricing-card">
        <div class="pricing-name">Claude 重度月卡</div>
        <div class="pricing-price">&#165;128<span>/30天</span></div>
        <div class="pricing-quota">$500 额度 | 支持 Claude 4.6</div>
        <div class="pricing-divider"></div>
        <div class="pricing-detail">更高频的 Claude Code 与 Claude 客户端使用</div>
      </div>
    </div>

    <h3>套餐对比</h3>
    <p>以下是当前在售套餐的详细对比：</p>
    <div class="table-wrapper">
      <table>
        <tr><th>套餐</th><th>价格（&#165;）</th><th>原价（&#165;）</th><th>有效期</th><th>总额度（$）</th><th>日限额（$）</th><th>倍率</th></tr>
        <tr><td>codex周卡</td><td>68</td><td>140</td><td>7 天</td><td>700</td><td>100</td><td>1x</td></tr>
        <tr><td>标准月卡</td><td>198</td><td>600</td><td>30 天</td><td>3000</td><td>100</td><td>1x</td></tr>
        <tr><td>codex高级月卡</td><td>298</td><td>1200</td><td>30 天</td><td>6000</td><td>200</td><td>1x</td></tr>
        <tr><td>Claude 周卡</td><td>29.9</td><td>40</td><td>7 天</td><td>100</td><td>-</td><td>Claude 专属</td></tr>
        <tr><td>Claude 标准月卡</td><td>88</td><td>120</td><td>30 天</td><td>300</td><td>-</td><td>Claude 专属</td></tr>
        <tr><td>Claude 重度月卡</td><td>128</td><td>200</td><td>30 天</td><td>500</td><td>-</td><td>Claude 专属</td></tr>
      </table>
    </div>

    <h3>直充 vs 套餐，哪个更划算？</h3>
    <p>按量充值采用 &#165;1 = $1 余额（1:1 比例），灵活按量付费。套餐订阅提供更大的额度包和固定有效期，适合有持续用量的用户。</p>
    <div class="table-wrapper">
      <table>
        <tr><th>方式</th><th>充值比例</th><th>适用倍率</th><th>优势</th></tr>
        <tr><td>按量充值（Codex / OpenAI）</td><td>&#165;1 = $1</td><td>OpenAI 默认分组 0.3x</td><td>灵活按量，无有效期限制</td></tr>
        <tr><td>套餐订阅</td><td>套餐定价</td><td>按套餐规则</td><td>额度更大，适合稳定高频使用</td></tr>
      </table>
    </div>

    <div class="callout callout-tip">
      <div class="callout-title">&#128161; 选择建议</div>
      <p>如果只是短期试用或偶尔使用，按量充值更灵活；如果是一周内集中开发，选择 <strong>codex周卡（&#165;68）</strong>；如果会持续使用 Codex / OpenAI 模型，推荐 <strong>标准月卡（&#165;198）</strong>；如果是重度开发或团队使用，选择 <strong>codex高级月卡（&#165;298）</strong>。主要使用 Claude 的用户，可按周期和用量选择 Claude 周卡、Claude 标准月卡或 Claude 重度月卡。</p>
    </div>
  </section>

  <!-- Code Examples -->
  <section id="example-python">
    <h2>代码示例</h2>

    <h3>Python（使用 OpenAI SDK）</h3>
    <p>首先安装 OpenAI Python SDK：</p>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">bash</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code>pip install openai</code></pre>
    </div>

    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">python</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code><span class="kw">from</span> openai <span class="kw">import</span> OpenAI

client = OpenAI(
    <span class="var">api_key</span>=<span class="str">"sk-YOUR_API_KEY"</span>,
    <span class="var">base_url</span>=<span class="str">"https://api.xiaobocode.com/v1"</span>
)

response = client.chat.completions.<span class="fn">create</span>(
    <span class="var">model</span>=<span class="str">"gpt-5.5"</span>,
    <span class="var">messages</span>=[
        {<span class="str">"role"</span>: <span class="str">"system"</span>, <span class="str">"content"</span>: <span class="str">"你是一个有帮助的AI助手。"</span>},
        {<span class="str">"role"</span>: <span class="str">"user"</span>, <span class="str">"content"</span>: <span class="str">"用Python实现一个二分查找算法。"</span>}
    ],
    <span class="var">temperature</span>=<span class="num">0.7</span>,
    <span class="var">max_tokens</span>=<span class="num">1024</span>
)

<span class="fn">print</span>(response.choices[<span class="num">0</span>].message.content)</code></pre>
    </div>
  </section>

  <section id="example-nodejs">
    <h3>Node.js（使用 OpenAI SDK）</h3>
    <p>首先安装 OpenAI Node.js SDK：</p>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">bash</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code>npm install openai</code></pre>
    </div>

    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">javascript</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code><span class="kw">import</span> OpenAI <span class="kw">from</span> <span class="str">'openai'</span>;

<span class="kw">const</span> client = <span class="kw">new</span> <span class="fn">OpenAI</span>({
  <span class="var">apiKey</span>: <span class="str">'sk-YOUR_API_KEY'</span>,
  <span class="var">baseURL</span>: <span class="str">'https://api.xiaobocode.com/v1'</span>,
});

<span class="kw">async function</span> <span class="fn">main</span>() {
  <span class="kw">const</span> completion = <span class="kw">await</span> client.chat.completions.<span class="fn">create</span>({
    <span class="var">model</span>: <span class="str">'gpt-5.5'</span>,
    <span class="var">messages</span>: [
      { <span class="var">role</span>: <span class="str">'system'</span>, <span class="var">content</span>: <span class="str">'你是一个有帮助的AI助手。'</span> },
      { <span class="var">role</span>: <span class="str">'user'</span>, <span class="var">content</span>: <span class="str">'用JavaScript实现一个防抖函数。'</span> }
    ],
    <span class="var">temperature</span>: <span class="num">0.7</span>,
    <span class="var">max_tokens</span>: <span class="num">1024</span>,
  });

  console.<span class="fn">log</span>(completion.choices[<span class="num">0</span>].message.content);
}

<span class="fn">main</span>();</code></pre>
    </div>
  </section>

  <section id="example-curl">
    <h3>cURL</h3>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">bash</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code><span class="var">curl</span> -X POST https://api.xiaobocode.com/v1/chat/completions \
  <span class="op">-H</span> <span class="str">"Content-Type: application/json"</span> \
  <span class="op">-H</span> <span class="str">"Authorization: Bearer sk-YOUR_API_KEY"</span> \
  <span class="op">-d</span> <span class="str">'{
    "model": "gpt-5.4-mini",
    "messages": [
      {"role": "system", "content": "你是一个有帮助的AI助手。"},
      {"role": "user", "content": "什么是量子计算？请用通俗的语言解释。"}
    ],
    "temperature": 0.7,
    "max_tokens": 512
  }'</span></code></pre>
    </div>
  </section>

  <section id="example-stream">
    <h3>流式输出示例</h3>

    <h4>Python 流式输出</h4>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">python</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code><span class="kw">from</span> openai <span class="kw">import</span> OpenAI

client = OpenAI(
    <span class="var">api_key</span>=<span class="str">"sk-YOUR_API_KEY"</span>,
    <span class="var">base_url</span>=<span class="str">"https://api.xiaobocode.com/v1"</span>
)

stream = client.chat.completions.<span class="fn">create</span>(
    <span class="var">model</span>=<span class="str">"gpt-5.5"</span>,
    <span class="var">messages</span>=[
        {<span class="str">"role"</span>: <span class="str">"user"</span>, <span class="str">"content"</span>: <span class="str">"写一首关于春天的七言绝句。"</span>}
    ],
    <span class="var">stream</span>=<span class="kw">True</span>
)

<span class="kw">for</span> chunk <span class="kw">in</span> stream:
    <span class="kw">if</span> chunk.choices[<span class="num">0</span>].delta.content <span class="kw">is not None</span>:
        <span class="fn">print</span>(chunk.choices[<span class="num">0</span>].delta.content, <span class="var">end</span>=<span class="str">""</span>, <span class="var">flush</span>=<span class="kw">True</span>)

<span class="fn">print</span>()  <span class="cm"># 换行</span></code></pre>
    </div>

    <h4>Node.js 流式输出</h4>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">javascript</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code><span class="kw">import</span> OpenAI <span class="kw">from</span> <span class="str">'openai'</span>;

<span class="kw">const</span> client = <span class="kw">new</span> <span class="fn">OpenAI</span>({
  <span class="var">apiKey</span>: <span class="str">'sk-YOUR_API_KEY'</span>,
  <span class="var">baseURL</span>: <span class="str">'https://api.xiaobocode.com/v1'</span>,
});

<span class="kw">async function</span> <span class="fn">main</span>() {
  <span class="kw">const</span> stream = <span class="kw">await</span> client.chat.completions.<span class="fn">create</span>({
    <span class="var">model</span>: <span class="str">'gpt-5.5'</span>,
    <span class="var">messages</span>: [
      { <span class="var">role</span>: <span class="str">'user'</span>, <span class="var">content</span>: <span class="str">'写一首关于春天的七言绝句。'</span> }
    ],
    <span class="var">stream</span>: <span class="kw">true</span>,
  });

  <span class="kw">for await</span> (<span class="kw">const</span> chunk <span class="kw">of</span> stream) {
    <span class="kw">const</span> content = chunk.choices[<span class="num">0</span>]?.delta?.content;
    <span class="kw">if</span> (content) {
      process.stdout.<span class="fn">write</span>(content);
    }
  }

  console.<span class="fn">log</span>();
}

<span class="fn">main</span>();</code></pre>
    </div>

    <h4>cURL 流式输出</h4>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">bash</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code><span class="var">curl</span> -X POST https://api.xiaobocode.com/v1/chat/completions \
  <span class="op">-H</span> <span class="str">"Content-Type: application/json"</span> \
  <span class="op">-H</span> <span class="str">"Authorization: Bearer sk-YOUR_API_KEY"</span> \
  <span class="op">-N</span> \
  <span class="op">-d</span> <span class="str">'{
    "model": "gpt-5.5",
    "messages": [
      {"role": "user", "content": "你好"}
    ],
    "stream": true
  }'</span></code></pre>
    </div>
  </section>

  <!-- Claude Code -->
  <section id="claude-code">
    <h2>Claude Code 接入指南</h2>
    <p>Claude Code 是 Anthropic 推出的命令行 AI 编程助手。您可以通过本平台的 API 网关来使用 Claude Code，享受更优惠的价格和更稳定的国内访问体验。</p>

    <h3>配置方法</h3>
    <p>在您的终端环境中设置以下环境变量：</p>

    <h4>Linux / macOS</h4>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">bash</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code><span class="cm"># 将以下内容添加到 ~/.bashrc 或 ~/.zshrc</span>
<span class="kw">export</span> <span class="var">ANTHROPIC_BASE_URL</span>=<span class="str">"https://api.xiaobocode.com"</span>
<span class="kw">export</span> <span class="var">ANTHROPIC_API_KEY</span>=<span class="str">"sk-YOUR_API_KEY"</span>

<span class="cm"># 使配置生效</span>
<span class="var">source</span> ~/.bashrc  <span class="cm"># 或 source ~/.zshrc</span></code></pre>
    </div>

    <h4>Windows (PowerShell)</h4>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">powershell</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code><span class="cm"># 临时设置（当前会话有效）</span>
<span class="var">$env:ANTHROPIC_BASE_URL</span> = <span class="str">"https://api.xiaobocode.com"</span>
<span class="var">$env:ANTHROPIC_API_KEY</span> = <span class="str">"sk-YOUR_API_KEY"</span>

<span class="cm"># 永久设置（写入用户环境变量）</span>
[Environment]::<span class="fn">SetEnvironmentVariable</span>(<span class="str">"ANTHROPIC_BASE_URL"</span>, <span class="str">"https://api.xiaobocode.com"</span>, <span class="str">"User"</span>)
[Environment]::<span class="fn">SetEnvironmentVariable</span>(<span class="str">"ANTHROPIC_API_KEY"</span>, <span class="str">"sk-YOUR_API_KEY"</span>, <span class="str">"User"</span>)</code></pre>
    </div>

    <h4>Windows (CMD)</h4>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">cmd</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code><span class="cm">:: 永久设置</span>
<span class="kw">setx</span> <span class="var">ANTHROPIC_BASE_URL</span> <span class="str">"https://api.xiaobocode.com"</span>
<span class="kw">setx</span> <span class="var">ANTHROPIC_API_KEY</span> <span class="str">"sk-YOUR_API_KEY"</span></code></pre>
    </div>

    <h3>验证配置</h3>
    <p>配置完成后，直接启动 Claude Code 即可：</p>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">bash</span>
        <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
      </div>
<pre><code><span class="cm"># 启动 Claude Code</span>
<span class="var">claude</span>

<span class="cm"># 或在项目目录中启动</span>
<span class="kw">cd</span> your-project && <span class="var">claude</span></code></pre>
    </div>

    <p>如果一切配置正确，Claude Code 将通过本平台的网关与 Claude 模型通信。您可以像平常一样使用所有 Claude Code 功能，包括代码编辑、项目分析、命令执行等。</p>

    <div class="callout callout-warn">
      <div class="callout-title">&#9888; 注意事项</div>
      <p>Claude Code 调用的是 Claude 系列模型。当前可通过 Claude 周卡、Claude 标准月卡和 Claude 重度月卡使用；具体可购买套餐、支持模型和可用状态请以控制台展示为准。</p>
    </div>

    <div class="callout callout-info">
      <div class="callout-title">&#128161; 提示</div>
      <p>请确保 <code>ANTHROPIC_BASE_URL</code> 设置为 <code>https://api.xiaobocode.com</code>（不含 <code>/v1</code> 后缀），Claude Code 会自动拼接路径。<code>ANTHROPIC_API_KEY</code> 使用您在本平台生成的 API Key。</p>
    </div>
  </section>

  <!-- VS Code 插件 -->
  <section id="vscode">
    <h2>VS Code 插件接入</h2>
    <p>VS Code 是最流行的代码编辑器之一，借助 AI 插件可以极大提升编码效率。以下介绍两款主流 AI 编程插件的接入方法。</p>

    <h3>A) Continue 插件（推荐）</h3>
    <p>Continue 是一款开源的 AI 编程助手插件，支持代码补全、对话、重构等丰富功能，且完美兼容 OpenAI API 格式。</p>

    <div class="steps">
      <div class="step">
        <h4>安装 Continue 插件</h4>
        <p>打开 VS Code，点击左侧扩展图标（或按 <code>Ctrl+Shift+X</code>），在搜索框中输入 <strong>Continue</strong>，找到由 Continue.dev 发布的插件并点击「安装」。</p>
      </div>
      <div class="step">
        <h4>打开配置文件</h4>
        <p>安装完成后，点击左侧边栏的 Continue 图标打开侧边栏，然后点击底部的 <strong>齿轮图标</strong>（设置），即可打开 <code>config.json</code> 配置文件。</p>
      </div>
      <div class="step">
        <h4>填写模型配置</h4>
        <p>在 <code>config.json</code> 中添加以下配置，将 <code>sk-YOUR_API_KEY</code> 替换为您的实际密钥：</p>
        <div class="code-block">
          <div class="code-header">
            <span class="code-lang">json</span>
            <button type="button" class="copy-btn" @click="copyCodeBlock">复制</button>
          </div>
<pre><code>{
  <span class="str">"models"</span>: [
    {
      <span class="str">"title"</span>: <span class="str">"GPT-5.5"</span>,
      <span class="str">"provider"</span>: <span class="str">"openai"</span>,
      <span class="str">"model"</span>: <span class="str">"gpt-5.5"</span>,
      <span class="str">"apiBase"</span>: <span class="str">"https://api.xiaobocode.com/v1"</span>,
      <span class="str">"apiKey"</span>: <span class="str">"sk-YOUR_API_KEY"</span>
    },
    {
      <span class="str">"title"</span>: <span class="str">"Claude Opus 4.7"</span>,
      <span class="str">"provider"</span>: <span class="str">"openai"</span>,
      <span class="str">"model"</span>: <span class="str">"claude-opus-4-7"</span>,
      <span class="str">"apiBase"</span>: <span class="str">"https://api.xiaobocode.com/v1"</span>,
      <span class="str">"apiKey"</span>: <span class="str">"sk-YOUR_API_KEY"</span>
    }
  ]
}</code></pre>
        </div>
        <div class="callout callout-tip">
          <div class="callout-title">&#9989; 提示</div>
          <p>您可以在 <code>models</code> 数组中添加多个模型配置，在使用时随时切换。<code>provider</code> 统一填 <code>openai</code> 即可，本平台兼容 OpenAI API 格式。</p>
        </div>
      </div>
      <div class="step">
        <h4>开始使用</h4>
        <p>保存配置后，在 Continue 侧边栏的模型下拉列表中选择您配置的模型，即可在编辑器中直接与 AI 对话、生成代码、解释代码或进行重构。按 <code>Ctrl+L</code> 可快速打开 Continue 对话框。</p>
      </div>
    </div>

    <h3>B) Cline 插件</h3>
    <p>Cline（原 Claude Dev）是一款强大的 AI 编程助手，支持自主编辑文件、执行命令等高级功能。</p>

    <div class="steps">
      <div class="step">
        <h4>安装 Cline 插件</h4>
        <p>在 VS Code 扩展商店搜索 <strong>Cline</strong>，找到插件并安装。安装后左侧边栏会出现 Cline 图标。</p>
      </div>
      <div class="step">
        <h4>配置 API 提供商</h4>
        <p>点击 Cline 侧边栏图标，在顶部设置区域将 <strong>API Provider</strong> 切换为 <strong>OpenAI Compatible</strong>。</p>
        <div class="config-box">
          <div class="config-row">
            <span class="config-label">API Provider</span>
            <span class="config-value">OpenAI Compatible</span>
          </div>
          <div class="config-row">
            <span class="config-label">Base URL</span>
            <span class="config-value">https://api.xiaobocode.com/v1</span>
          </div>
          <div class="config-row">
            <span class="config-label">API Key</span>
            <span class="config-value">sk-YOUR_API_KEY</span>
          </div>
          <div class="config-row">
            <span class="config-label">Model ID</span>
            <span class="config-value">gpt-5.5</span>
          </div>
        </div>
      </div>
      <div class="step">
        <h4>开始使用</h4>
        <p>配置完成后，在 Cline 对话框中输入您的需求即可。Cline 可以自动分析项目结构、编辑代码文件、运行终端命令，是非常强大的 AI 编程搭档。</p>
      </div>
    </div>

    <div class="callout callout-info">
      <div class="callout-title">&#128161; 提示</div>
      <p>两款插件各有优势：Continue 适合日常代码补全和对话辅助，Cline 更擅长自主完成复杂编码任务。您可以同时安装，根据场景灵活使用。</p>
    </div>
  </section>

  <!-- JetBrains 插件 -->
  <section id="jetbrains">
    <h2>JetBrains 插件接入</h2>
    <p>如果您使用 IntelliJ IDEA、PyCharm、WebStorm 等 JetBrains 系列 IDE，可以通过 CodeGPT 插件接入本平台。</p>

    <h3>CodeGPT 插件配置</h3>

    <div class="steps">
      <div class="step">
        <h4>安装 CodeGPT 插件</h4>
        <p>打开 JetBrains IDE，进入 <code>Settings</code>（<code>Ctrl+Alt+S</code>）&rarr; <code>Plugins</code> &rarr; <code>Marketplace</code>，搜索 <strong>CodeGPT</strong> 并安装。安装后需重启 IDE。</p>
      </div>
      <div class="step">
        <h4>配置自定义 API</h4>
        <p>重启后进入 <code>Settings</code> &rarr; <code>Tools</code> &rarr; <code>CodeGPT</code>，在服务提供商中选择 <strong>Custom OpenAI</strong>，然后填写以下信息：</p>
        <div class="config-box">
          <div class="config-row">
            <span class="config-label">Provider</span>
            <span class="config-value">Custom OpenAI</span>
          </div>
          <div class="config-row">
            <span class="config-label">Base URL (Host)</span>
            <span class="config-value">https://api.xiaobocode.com/v1</span>
          </div>
          <div class="config-row">
            <span class="config-label">API Key</span>
            <span class="config-value">sk-YOUR_API_KEY</span>
          </div>
          <div class="config-row">
            <span class="config-label">Model</span>
            <span class="config-value">gpt-5.5</span>
          </div>
        </div>
        <div class="callout callout-tip">
          <div class="callout-title">&#9989; 提示</div>
          <p>模型名称可以填写本平台支持的任意模型，如 <code>gpt-5.5</code>、<code>gpt-image-2</code>、<code>claude-sonnet-4-6</code> 等。具体可用模型请参阅上方「支持的模型」章节。</p>
        </div>
      </div>
      <div class="step">
        <h4>开始使用</h4>
        <p>配置完成后，您可以在编辑器中右键选择 CodeGPT 功能，或使用快捷键打开 AI 对话窗口。支持代码生成、代码解释、Bug 修复等常用功能。</p>
      </div>
    </div>
  </section>

  <!-- Cherry Studio -->
  <section id="cherry-studio">
    <h2>Cherry Studio 接入</h2>
    <p>Cherry Studio 是一款精美的跨平台 AI 桌面客户端，支持多模型对话、知识库管理、Prompt 模板等丰富功能，界面友好，非常适合日常 AI 对话使用。</p>

    <div class="steps">
      <div class="step">
        <h4>下载安装</h4>
        <p>前往 <a href="https://cherry-ai.com" target="_blank" rel="noreferrer">Cherry Studio 官网</a> 下载适合您操作系统的安装包（支持 Windows、macOS、Linux），完成安装并打开应用。</p>
      </div>
      <div class="step">
        <h4>添加自定义服务商</h4>
        <p>打开 Cherry Studio，进入 <strong>设置</strong> &rarr; <strong>模型服务商</strong>，点击 <strong>添加自定义服务商</strong>，填写以下信息：</p>
        <div class="config-box">
          <div class="config-row">
            <span class="config-label">服务商名称</span>
            <span class="config-value">智脑API</span>
          </div>
          <div class="config-row">
            <span class="config-label">API 地址</span>
            <span class="config-value">https://api.xiaobocode.com/v1</span>
          </div>
          <div class="config-row">
            <span class="config-label">API Key</span>
            <span class="config-value">sk-YOUR_API_KEY</span>
          </div>
        </div>
      </div>
      <div class="step">
        <h4>添加模型</h4>
        <p>在刚添加的服务商下方，点击 <strong>添加模型</strong>，手动输入模型名称（如 <code>gpt-5.5</code>、<code>gpt-image-2</code>、<code>claude-opus-4-7</code>），或点击「获取模型列表」自动拉取可用模型。</p>
      </div>
      <div class="step">
        <h4>开始对话</h4>
        <p>返回主界面，在顶部的模型选择器中切换到您刚添加的模型，即可开始对话。Cherry Studio 支持多轮对话、Markdown 渲染、代码高亮等功能。</p>
      </div>
    </div>

    <div class="callout callout-tip">
      <div class="callout-title">&#9989; 提示</div>
      <p>Cherry Studio 支持同时配置多个服务商和模型，您可以在对话中随时切换模型进行对比。还支持设置系统提示词、调整温度等参数。</p>
    </div>
  </section>

  <!-- 更多客户端 -->
  <section id="more-clients">
    <h2>更多客户端</h2>
    <p>除了以上介绍的工具，还有许多优秀的 AI 客户端支持自定义 OpenAI API 地址，均可无缝接入本平台。</p>

    <div class="tool-grid">
      <div class="tool-card">
        <h4>OpenCat</h4>
        <p>iOS / macOS 原生 AI 客户端，界面精美，支持自定义 API 地址和多模型切换。</p>
      </div>
      <div class="tool-card">
        <h4>BotGPT</h4>
        <p>iOS 平台的 AI 聊天客户端，支持 OpenAI 兼容 API，操作简单直观。</p>
      </div>
      <div class="tool-card">
        <h4>NextChat</h4>
        <p>开源 Web 聊天应用，支持一键部署到 Vercel，自带 Prompt 模板和对话管理。</p>
      </div>
      <div class="tool-card">
        <h4>LibreChat</h4>
        <p>功能强大的开源自托管聊天平台，支持多 AI 服务商、插件、文件上传等高级功能。</p>
      </div>
    </div>

    <div class="callout callout-tip">
      <div class="callout-title">&#9989; 通用接入方法</div>
      <p>只要支持自定义 OpenAI API 地址的工具，都可以接入本平台。通用配置如下：</p>
    </div>

    <div class="config-box">
      <div class="config-row">
        <span class="config-label">API 地址</span>
        <span class="config-value">https://api.xiaobocode.com/v1</span>
      </div>
      <div class="config-row">
        <span class="config-label">API Key</span>
        <span class="config-value">您在平台生成的密钥（sk-开头）</span>
      </div>
      <div class="config-row">
        <span class="config-label">模型名称</span>
        <span class="config-value">参考上方「支持的模型」章节</span>
      </div>
    </div>
  </section>

  <!-- Redeem Code -->
  <section id="redeem">
    <h2>兑换码使用教程</h2>
    <p>如果您获得了兑换码，可以按照以下步骤兑换余额或并发数到您的账户。</p>

    <div class="steps">
      <div class="step">
        <h4>登录平台</h4>
        <p>打开浏览器访问 <a href="https://xiaobocode.com" target="_blank" rel="noreferrer">https://xiaobocode.com</a>，使用您的账号登录平台。</p>
      </div>
      <div class="step">
        <h4>进入兑换页面</h4>
        <p>登录后，点击左侧菜单中的「兑换码」选项，或直接访问兑换页面。</p>
      </div>
      <div class="step">
        <h4>粘贴兑换码</h4>
        <p>在兑换码输入框中，粘贴您获得的兑换码。</p>
      </div>
      <div class="step">
        <h4>点击兑换</h4>
        <p>确认兑换码无误后，点击「兑换」按钮提交。</p>
      </div>
      <div class="step">
        <h4>兑换完成</h4>
        <p>兑换成功后，余额或并发数会自动增加到您的账户，您可以在仪表盘查看更新后的余额。</p>
      </div>
    </div>

    <div class="callout callout-tip">
      <div class="callout-title">&#128161; 兑换提示</div>
      <ul>
        <li>兑换码区分大小写，建议直接复制粘贴</li>
        <li>每个兑换码只能使用一次</li>
        <li>兑换码有有效期，请在有效期内使用</li>
        <li>如果兑换失败，请检查兑换码是否正确或联系客服</li>
      </ul>
    </div>
  </section>

  <!-- FAQ -->
  <section id="faq">
    <h2>常见问题</h2>

    <details class="faq-item">
      <summary class="faq-q">平台支持哪些模型？模型列表会更新吗？</summary>
      <div class="faq-a">当前支持 GPT-5.5、GPT-5.4 系列、GPT Image 2、o 系列推理模型以及 Claude Opus/Sonnet/Haiku 系列。模型列表会随官方发布和平台运营状态更新，具体可用模型请以控制台和「支持的模型」章节为准。</div>
    </details>

    <details class="faq-item">
      <summary class="faq-q">API 接口与 OpenAI 官方完全兼容吗？</summary>
      <div class="faq-a">是的。本平台兼容 OpenAI 的 <code>/v1/chat/completions</code>、<code>/v1/responses</code> 和 Images 接口规范，包括流式输出、function calling、<code>gpt-image-2</code> 图片生成等能力。图片模型仅支持按量余额扣费，订阅套餐不可使用 image2；使用 OpenAI 官方 Python/Node.js SDK 时，只需修改 <code>base_url</code> 和 <code>api_key</code> 两个参数即可切换。</div>
    </details>

    <details class="faq-item">
      <summary class="faq-q">遇到 429 错误怎么办？</summary>
      <div class="faq-a">429 通常表示短时间内请求过多。请在客户端加入指数退避重试，并适当降低并发；如果持续出现，可联系客服协助排查当前账号和模型状态。</div>
    </details>

    <details class="faq-item">
      <summary class="faq-q">Claude 模型和 OpenAI 模型的费率有什么区别？</summary>
      <div class="faq-a">Codex / OpenAI 文本模型支持按量充值和套餐订阅；当前 Codex 套餐包含 codex周卡、标准月卡和 codex高级月卡。Claude 模型当前提供 Claude 周卡、Claude 标准月卡和 Claude 重度月卡三种专属套餐。image2 图片模型不属于订阅套餐范围，仅支持按量余额扣费，具体可用模型和购买状态请以控制台展示为准。</div>
    </details>

    <details class="faq-item">
      <summary class="faq-q">充值余额和订阅套餐可以退款吗？</summary>
      <div class="faq-a">直接充值的未使用余额支持退款，请联系客服处理。订阅套餐属于即时生效的虚拟服务，购买成功后系统会立即发放套餐额度并开始计算有效期；已购买、已激活的订阅套餐不支持退款、退换或折算为按量余额。建议首次使用时先小额充值或购买周卡进行测试。</div>
    </details>

    <details class="faq-item">
      <summary class="faq-q">套餐额度到期后会怎样？可以叠加购买吗？</summary>
      <div class="faq-a">套餐到期后未使用的额度将失效，不会结转到下一周期。不同套餐之间的额度可以叠加——例如您当前持有周卡，可以再购买月卡，两个套餐的额度将同时可用，各自按照自己的有效期管理。</div>
    </details>

    <details class="faq-item">
      <summary class="faq-q">出现 401 错误怎么排查？</summary>
      <div class="faq-a">401 错误意味着身份认证失败，请按以下顺序排查：(1) 检查 API Key 是否正确复制，没有多余的空格或换行符；(2) 确认请求头格式为 <code>Authorization: Bearer sk-xxx</code>，注意 Bearer 后面有一个空格；(3) 在控制台确认该令牌是否仍处于「启用」状态且未被删除；(4) 确认账户余额大于 0。</div>
    </details>

    <details class="faq-item">
      <summary class="faq-q">如何查看 API 调用用量和余额？</summary>
      <div class="faq-a">登录 <a href="https://xiaobocode.com" target="_blank" rel="noreferrer">控制台</a> 后，在「仪表盘」页面可以查看当前余额和近期消费趋势。「日志」页面可以查看每一次 API 调用的详细记录，包括使用的模型、token 消耗数量和扣费金额。</div>
    </details>

    <details class="faq-item">
      <summary class="faq-q">可以同时使用多个 API Key 吗？</summary>
      <div class="faq-a">可以。您可以在控制台创建多个 API Key，分别用于不同的项目或环境（如开发环境和生产环境）。所有 Key 共享同一账户余额，但日志中会区分来源，方便您追踪各项目的用量。您也可以为每个 Key 设置独立的额度上限。</div>
    </details>

    <details class="faq-item">
      <summary class="faq-q">服务的稳定性和可用性如何保障？</summary>
      <div class="faq-a">平台采用多节点架构，配合自动负载均衡和故障转移机制。当某个节点异常时，系统会在毫秒级自动切换到健康节点，用户侧几乎无感知。历史可用性维持在 99.9% 以上。此外，平台有完善的监控告警体系，运维团队全天候值守。</div>
    </details>

    <details class="faq-item">
      <summary class="faq-q">国内网络访问速度如何？需要翻墙吗？</summary>
      <div class="faq-a">不需要。本平台服务器部署在国内优质线路上，直接通过 <code>https://xiaobocode.com</code> 即可访问，无需任何代理或VPN。对于国内用户而言，访问延迟显著优于直连海外 API 服务。这也是使用本平台的核心优势之一。</div>
    </details>

    <details class="faq-item">
      <summary class="faq-q">是否支持 function calling / tools 功能？</summary>
      <div class="faq-a">支持。对于 GPT 系列和 Claude 系列的模型，function calling（工具调用）功能完全透传，参数格式与 OpenAI 官方规范一致。您可以在请求中传入 <code>tools</code> 参数定义可调用的函数，模型会根据上下文判断是否需要调用。</div>
    </details>
  </section>

        </article>
      </section>
    </main>

    <MarketingFooter />
  </div>
</template>

<script setup lang="ts">
import { computed, watchEffect } from "vue";
import MarketingFooter from "@/components/marketing/MarketingFooter.vue";
import MarketingHeader from "@/components/marketing/MarketingHeader.vue";
import { useAppStore } from "@/stores";

const appStore = useAppStore();

const navGroups = [
  {
    title: "入门",
    items: [
      { id: "overview", title: "平台概述" },
      { id: "quickstart", title: "快速开始" },
    ],
  },
  {
    title: "接入指南",
    items: [
      { id: "api-basics", title: "API 基础信息" },
      { id: "models", title: "支持的模型" },
      { id: "request-format", title: "请求格式" },
      { id: "response-format", title: "响应格式" },
      { id: "error-codes", title: "错误码说明" },
    ],
  },
  {
    title: "计费",
    items: [
      { id: "pricing", title: "计费说明" },
      { id: "pricing-compare", title: "套餐对比" },
    ],
  },
  {
    title: "代码示例",
    items: [
      { id: "example-python", title: "Python" },
      { id: "example-nodejs", title: "Node.js" },
      { id: "example-curl", title: "cURL" },
      { id: "example-stream", title: "流式输出" },
    ],
  },
  {
    title: "进阶",
    items: [
      { id: "claude-code", title: "Claude Code 接入" },
      { id: "vscode", title: "VS Code 插件" },
      { id: "jetbrains", title: "JetBrains 插件" },
      { id: "cherry-studio", title: "Cherry Studio" },
      { id: "more-clients", title: "更多客户端" },
    ],
  },
  {
    title: "帮助",
    items: [
      { id: "redeem", title: "兑换码" },
      { id: "faq", title: "常见问题" },
    ],
  },
] as const;

const siteName = computed(
  () =>
    appStore.cachedPublicSettings?.site_name || appStore.siteName || "Sub2API",
);

watchEffect(() => {
  document.title = `开发者文档 - ${siteName.value}`;
});

async function copyCodeBlock(event: MouseEvent): Promise<void> {
  const button =
    event.currentTarget instanceof HTMLButtonElement ? event.currentTarget : null;
  const code = button?.closest(".code-block")?.querySelector("code")?.textContent;
  if (!button || !code) {
    return;
  }

  try {
    await navigator.clipboard?.writeText(code);
  } catch {
    const textarea = document.createElement("textarea");
    textarea.value = code;
    textarea.style.position = "fixed";
    textarea.style.opacity = "0";
    document.body.appendChild(textarea);
    textarea.select();
    document.execCommand("copy");
    document.body.removeChild(textarea);
  }

  button.textContent = "已复制";
  button.classList.add("copied");
  window.setTimeout(() => {
    button.textContent = "复制";
    button.classList.remove("copied");
  }, 1500);
}

</script>

<style scoped>
.docs-toc-card {
  max-height: none;
}

.docs-toc-nav {
  min-height: 0;
}

@media (min-width: 1024px) {
  .docs-toc-card {
    display: flex;
    max-height: calc(100vh - 7rem);
    flex-direction: column;
    overflow: hidden;
  }

  .docs-toc-nav {
    overflow-y: auto;
    padding-right: 0.25rem;
  }
}

.docs-content {
  scroll-margin-top: 7rem;
}

.docs-content :deep(section) {
  scroll-margin-top: 7rem;
}

.docs-content :deep(section + section) {
  margin-top: 3rem;
  padding-top: 2rem;
  border-top: 1px solid rgb(229 231 235);
}

.dark .docs-content :deep(section + section) {
  border-top-color: rgb(55 65 81 / 0.75);
}

.docs-content :deep(h1) {
  margin-top: 0.75rem;
  max-width: 48rem;
  font-size: clamp(2rem, 4vw, 3.5rem);
  font-weight: 800;
  line-height: 1.12;
  letter-spacing: -0.02em;
  color: rgb(3 7 18);
}

.dark .docs-content :deep(h1) {
  color: white;
}

.docs-content :deep(h2) {
  margin-top: 0;
  margin-bottom: 1rem;
  font-size: 1.75rem;
  font-weight: 750;
  line-height: 1.25;
  color: rgb(3 7 18);
}

.dark .docs-content :deep(h2) {
  color: white;
}

.docs-content :deep(h3) {
  margin-top: 2rem;
  margin-bottom: 0.875rem;
  font-size: 1.25rem;
  font-weight: 700;
  color: rgb(17 24 39);
}

.dark .docs-content :deep(h3) {
  color: rgb(243 244 246);
}

.docs-content :deep(h4) {
  margin-top: 1.25rem;
  margin-bottom: 0.5rem;
  font-size: 1rem;
  font-weight: 700;
  color: rgb(31 41 55);
}

.dark .docs-content :deep(h4) {
  color: rgb(229 231 235);
}

.docs-content :deep(p),
.docs-content :deep(li),
.docs-content :deep(td) {
  color: rgb(75 85 99);
  line-height: 1.85;
}

.dark .docs-content :deep(p),
.dark .docs-content :deep(li),
.dark .docs-content :deep(td) {
  color: rgb(209 213 219);
}

.docs-content :deep(p) {
  margin-bottom: 1rem;
}

.docs-content :deep(a) {
  font-weight: 600;
  color: rgb(37 99 235);
  text-decoration: none;
}

.docs-content :deep(a:hover) {
  color: rgb(29 78 216);
  text-decoration: underline;
}

.docs-content :deep(.hero-badge) {
  display: inline-flex;
  width: fit-content;
  align-items: center;
  border-radius: 9999px;
  border: 1px solid rgb(219 234 254);
  background: rgb(239 246 255);
  padding: 0.375rem 0.75rem;
  font-size: 0.75rem;
  font-weight: 700;
  color: rgb(37 99 235);
}

.dark .docs-content :deep(.hero-badge) {
  border-color: rgb(30 64 175 / 0.5);
  background: rgb(30 58 138 / 0.25);
  color: rgb(147 197 253);
}

.docs-content :deep(.hero-desc) {
  max-width: 52rem;
  font-size: 1.05rem;
  line-height: 1.9;
}

.docs-content :deep(.feature-grid),
.docs-content :deep(.pricing-grid),
.docs-content :deep(.tool-grid) {
  display: grid;
  gap: 1rem;
  margin: 1.5rem 0;
}

.docs-content :deep(.feature-grid) {
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
}

.docs-content :deep(.pricing-grid),
.docs-content :deep(.tool-grid) {
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
}

.docs-content :deep(.feature-card),
.docs-content :deep(.pricing-card),
.docs-content :deep(.tool-card),
.docs-content :deep(.config-box),
.docs-content :deep(.faq-item) {
  border-radius: 1rem;
  border: 1px solid rgb(229 231 235);
  background: rgb(255 255 255 / 0.72);
  box-shadow: 0 10px 30px rgb(15 23 42 / 0.04);
}

.dark .docs-content :deep(.feature-card),
.dark .docs-content :deep(.pricing-card),
.dark .docs-content :deep(.tool-card),
.dark .docs-content :deep(.config-box),
.dark .docs-content :deep(.faq-item) {
  border-color: rgb(55 65 81 / 0.7);
  background: rgb(17 24 39 / 0.48);
}

.docs-content :deep(.feature-card),
.docs-content :deep(.pricing-card),
.docs-content :deep(.tool-card) {
  padding: 1.25rem;
}

.docs-content :deep(.feature-icon) {
  display: flex;
  height: 2.75rem;
  width: 2.75rem;
  align-items: center;
  justify-content: center;
  border-radius: 0.875rem;
  background: rgb(219 234 254);
  color: rgb(37 99 235);
  font-size: 1.25rem;
}

.dark .docs-content :deep(.feature-icon) {
  background: rgb(30 64 175 / 0.35);
  color: rgb(147 197 253);
}

.docs-content :deep(.code-block) {
  margin: 1rem 0 1.5rem;
  overflow: hidden;
  border-radius: 1rem;
  border: 1px solid rgb(30 41 59);
  background: rgb(15 23 42);
  box-shadow: 0 18px 40px rgb(15 23 42 / 0.18);
}

.docs-content :deep(.code-header) {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  border-bottom: 1px solid rgb(255 255 255 / 0.1);
  padding: 0.75rem 1rem;
}

.docs-content :deep(.code-lang) {
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  color: rgb(147 197 253);
  text-transform: uppercase;
}

.docs-content :deep(.copy-btn) {
  border-radius: 0.5rem;
  border: 1px solid rgb(255 255 255 / 0.1);
  background: rgb(255 255 255 / 0.08);
  padding: 0.35rem 0.75rem;
  font-size: 0.75rem;
  font-weight: 700;
  color: rgb(226 232 240);
  transition: background-color 0.2s, color 0.2s;
}

.docs-content :deep(.copy-btn:hover),
.docs-content :deep(.copy-btn.copied) {
  background: rgb(37 99 235 / 0.3);
  color: white;
}

.docs-content :deep(pre) {
  margin: 0;
  overflow-x: auto;
  padding: 1rem;
  color: rgb(226 232 240);
  font-size: 0.875rem;
  line-height: 1.7;
}

.docs-content :deep(code) {
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
}

.docs-content :deep(:not(pre) > code) {
  border-radius: 0.375rem;
  border: 1px solid rgb(191 219 254);
  background: rgb(239 246 255);
  padding: 0.125rem 0.375rem;
  color: rgb(29 78 216);
  font-size: 0.88em;
}

.dark .docs-content :deep(:not(pre) > code) {
  border-color: rgb(30 64 175 / 0.55);
  background: rgb(30 58 138 / 0.25);
  color: rgb(147 197 253);
}

.docs-content :deep(.kw),
.docs-content :deep(.op),
.docs-content :deep(.type) {
  color: rgb(125 211 252);
}

.docs-content :deep(.str) {
  color: rgb(167 243 208);
}

.docs-content :deep(.cm) {
  color: rgb(148 163 184);
}

.docs-content :deep(.fn) {
  color: rgb(196 181 253);
}

.docs-content :deep(.num),
.docs-content :deep(.prop),
.docs-content :deep(.var) {
  color: rgb(253 186 116);
}

.docs-content :deep(.table-wrapper) {
  margin: 1rem 0 1.5rem;
  overflow-x: auto;
  border-radius: 1rem;
  border: 1px solid rgb(229 231 235);
}

.dark .docs-content :deep(.table-wrapper) {
  border-color: rgb(55 65 81 / 0.75);
}

.docs-content :deep(table) {
  min-width: 100%;
  border-collapse: collapse;
  font-size: 0.875rem;
}

.docs-content :deep(th) {
  background: rgb(249 250 251);
  padding: 0.75rem 1rem;
  text-align: left;
  font-size: 0.8125rem;
  font-weight: 700;
  color: rgb(17 24 39);
  white-space: nowrap;
}

.dark .docs-content :deep(th) {
  background: rgb(31 41 55 / 0.7);
  color: rgb(243 244 246);
}

.docs-content :deep(td) {
  border-top: 1px solid rgb(229 231 235);
  padding: 0.75rem 1rem;
  vertical-align: top;
}

.dark .docs-content :deep(td) {
  border-top-color: rgb(55 65 81 / 0.75);
}

.docs-content :deep(.model-badge) {
  display: inline-flex;
  border-radius: 9999px;
  padding: 0.125rem 0.5rem;
  font-size: 0.75rem;
  font-weight: 700;
}

.docs-content :deep(.badge-gpt) {
  background: rgb(219 234 254);
  color: rgb(37 99 235);
}

.docs-content :deep(.badge-claude) {
  background: rgb(255 237 213);
  color: rgb(194 65 12);
}

.docs-content :deep(.badge-o) {
  background: rgb(220 252 231);
  color: rgb(22 101 52);
}

.docs-content :deep(.callout) {
  margin: 1.25rem 0;
  border-left: 4px solid rgb(59 130 246);
  border-radius: 0.875rem;
  background: rgb(239 246 255);
  padding: 1rem 1.25rem;
}

.docs-content :deep(.callout-title) {
  margin-bottom: 0.375rem;
  font-weight: 800;
  color: rgb(30 64 175);
}

.docs-content :deep(.callout p) {
  margin-bottom: 0;
}

.docs-content :deep(.callout-warn) {
  border-left-color: rgb(245 158 11);
  background: rgb(255 251 235);
}

.docs-content :deep(.callout-warn .callout-title) {
  color: rgb(146 64 14);
}

.docs-content :deep(.callout-tip) {
  border-left-color: rgb(16 185 129);
  background: rgb(236 253 245);
}

.docs-content :deep(.callout-tip .callout-title) {
  color: rgb(6 95 70);
}

.dark .docs-content :deep(.callout) {
  background: rgb(30 58 138 / 0.18);
}

.dark .docs-content :deep(.callout-warn) {
  background: rgb(120 53 15 / 0.2);
}

.dark .docs-content :deep(.callout-tip) {
  background: rgb(6 78 59 / 0.2);
}

.docs-content :deep(.steps) {
  counter-reset: step;
  margin: 1.5rem 0;
}

.docs-content :deep(.step) {
  position: relative;
  margin-bottom: 1.5rem;
  padding-left: 3.25rem;
}

.docs-content :deep(.step::before) {
  counter-increment: step;
  content: counter(step);
  position: absolute;
  left: 0;
  top: 0.125rem;
  display: flex;
  height: 2.25rem;
  width: 2.25rem;
  align-items: center;
  justify-content: center;
  border-radius: 0.75rem;
  background: linear-gradient(135deg, rgb(59 130 246), rgb(14 165 233));
  color: white;
  font-weight: 800;
}

.docs-content :deep(.pricing-card) {
  position: relative;
}

.docs-content :deep(.pricing-card.recommended) {
  border-color: rgb(59 130 246);
  box-shadow: 0 18px 40px rgb(37 99 235 / 0.14);
}

.docs-content :deep(.pricing-card.recommended::before) {
  content: "推荐";
  position: absolute;
  right: 1rem;
  top: 1rem;
  border-radius: 9999px;
  background: rgb(37 99 235);
  padding: 0.125rem 0.625rem;
  color: white;
  font-size: 0.75rem;
  font-weight: 800;
}

.docs-content :deep(.pricing-name) {
  font-size: 1rem;
  font-weight: 800;
  color: rgb(17 24 39);
}

.dark .docs-content :deep(.pricing-name) {
  color: white;
}

.docs-content :deep(.pricing-price) {
  margin-top: 0.75rem;
  font-size: 2rem;
  font-weight: 850;
  color: rgb(17 24 39);
}

.dark .docs-content :deep(.pricing-price) {
  color: white;
}

.docs-content :deep(.pricing-price span),
.docs-content :deep(.pricing-quota),
.docs-content :deep(.pricing-detail) {
  font-size: 0.875rem;
  color: rgb(107 114 128);
}

.dark .docs-content :deep(.pricing-price span),
.dark .docs-content :deep(.pricing-quota),
.dark .docs-content :deep(.pricing-detail) {
  color: rgb(156 163 175);
}

.docs-content :deep(.pricing-divider) {
  height: 1px;
  margin: 1rem 0;
  background: rgb(229 231 235);
}

.dark .docs-content :deep(.pricing-divider) {
  background: rgb(55 65 81 / 0.75);
}

.docs-content :deep(.config-box) {
  margin: 1rem 0 1.5rem;
  padding: 0.5rem 1rem;
}

.docs-content :deep(.config-row) {
  display: grid;
  grid-template-columns: minmax(120px, 0.45fr) 1fr;
  gap: 1rem;
  padding: 0.75rem 0;
  border-bottom: 1px solid rgb(229 231 235);
}

.docs-content :deep(.config-row:last-child) {
  border-bottom: 0;
}

.dark .docs-content :deep(.config-row) {
  border-bottom-color: rgb(55 65 81 / 0.75);
}

.docs-content :deep(.config-label) {
  color: rgb(107 114 128);
  font-size: 0.8125rem;
}

.docs-content :deep(.config-value) {
  overflow-wrap: anywhere;
  color: rgb(17 24 39);
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
  font-weight: 700;
}

.dark .docs-content :deep(.config-value) {
  color: rgb(243 244 246);
}

.docs-content :deep(.faq-item) {
  margin-bottom: 0.75rem;
  overflow: hidden;
}

.docs-content :deep(.faq-q) {
  display: flex;
  cursor: pointer;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  list-style: none;
  padding: 1rem 1.25rem;
  font-weight: 800;
  color: rgb(17 24 39);
}

.dark .docs-content :deep(.faq-q) {
  color: rgb(243 244 246);
}

.docs-content :deep(.faq-q::-webkit-details-marker) {
  display: none;
}

.docs-content :deep(.faq-q::after) {
  content: "+";
  flex-shrink: 0;
  color: rgb(37 99 235);
  font-size: 1.125rem;
}

.docs-content :deep(.faq-item[open] .faq-q::after) {
  content: "-";
}

.docs-content :deep(.faq-a) {
  border-top: 1px solid rgb(229 231 235);
  padding: 1rem 1.25rem;
  color: rgb(75 85 99);
}

.dark .docs-content :deep(.faq-a) {
  border-top-color: rgb(55 65 81 / 0.75);
  color: rgb(209 213 219);
}

@media (max-width: 640px) {
  .docs-content :deep(.config-row) {
    grid-template-columns: 1fr;
    gap: 0.25rem;
  }

  .docs-content :deep(.step) {
    padding-left: 0;
  }

  .docs-content :deep(.step::before) {
    position: static;
    margin-bottom: 0.75rem;
  }
}
</style>
