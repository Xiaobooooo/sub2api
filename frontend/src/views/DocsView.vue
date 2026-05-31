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
            <div
              v-for="group in navGroups"
              :key="group.title"
              class="docs-toc-group"
            >
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
          <!-- Quick Start -->
          <section id="quickstart">
            <h2>快速开始</h2>
            <p>只需三步，即可完成接入并发起第一次 AI 对话请求。</p>

            <div class="steps">
              <div class="step">
                <h4>注册账号并登录</h4>
                <p>
                  访问
                  <a
                    href="https://xiaobocode.com"
                    target="_blank"
                    rel="noreferrer"
                    >https://xiaobocode.com</a
                  >，完成账号注册。注册后即可进入控制台。
                </p>
              </div>
              <div class="step">
                <h4>获取 API Key</h4>
                <p>
                  在控制台的「令牌管理」页面，点击「创建新令牌」。系统将生成一个以
                  <code>sk-</code>
                  开头的密钥，请妥善保管——该密钥创建后仅显示一次。
                </p>
                <div class="callout callout-warn">
                  <div class="callout-title">&#9888; 安全提示</div>
                  <p>
                    API Key
                    是您账户的访问凭证，请勿在客户端代码、公开仓库或聊天中泄露。如不慎泄露，请立即在控制台删除并重新生成。
                  </p>
                </div>
              </div>
              <div class="step">
                <h4>发起第一次请求</h4>
                <p>
                  使用以下 cURL 命令测试连通性，将
                  <code>sk-YOUR_API_KEY</code> 替换为您的实际密钥：
                </p>
                <div class="code-block">
                  <div class="code-header">
                    <span class="code-lang">bash</span>
                    <button
                      type="button"
                      class="copy-btn"
                      @click="copyCodeBlock"
                    >
                      复制
                    </button>
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
                <p>
                  如果返回了包含 <code>choices</code> 字段的 JSON
                  响应，说明接入成功。
                </p>
              </div>
            </div>
          </section>

          <!-- API Basics -->
          <section id="api-basics">
            <h2>API 基础信息</h2>

            <div class="callout callout-warn">
              <div class="callout-title">&#9888; API 请求请使用专用域名</div>
              <p>
                官网和用户后台访问
                <code>https://xiaobocode.com</code
                >；终端、插件、SDK、Codex、Claude Code 等所有 API 调用请使用
                <code>https://api.xiaobocode.com</code>。<code
                  >xiaobocode.com</code
                >
                与 <code>www.xiaobocode.com</code> 已接入
                CDN，只适合网页和静态资源，不适合
                <code>/v1/chat/completions</code>、<code>/v1/responses</code>
                、<code>/v1/messages</code>
                这类流式长连接。继续使用官网域名调用 API，可能出现
                408、504、重连或空转。
              </p>
            </div>
            <h3>接入地址</h3>
            <div class="table-wrapper">
              <table>
                <thead>
                  <tr>
                    <th>配置项</th>
                    <th>值</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>Base URL</td>
                    <td><code>https://api.xiaobocode.com/v1</code></td>
                  </tr>
                  <tr>
                    <td>Chat Completions</td>
                    <td>
                      <code
                        >https://api.xiaobocode.com/v1/chat/completions</code
                      >
                    </td>
                  </tr>
                  <tr>
                    <td>Responses</td>
                    <td>
                      <code>https://api.xiaobocode.com/v1/responses</code>
                    </td>
                  </tr>
                  <tr>
                    <td>Messages</td>
                    <td>
                      <code>https://api.xiaobocode.com/v1/messages</code>
                    </td>
                  </tr>
                  <tr>
                    <td>Images</td>
                    <td>
                      <code>https://api.xiaobocode.com/v1/images/generations</code> /
                      <code>https://api.xiaobocode.com/v1/images/edits</code>
                    </td>
                  </tr>
                  <tr>
                    <td>协议</td>
                    <td>HTTPS（必须）</td>
                  </tr>
                  <tr>
                    <td>请求格式</td>
                    <td>JSON（<code>Content-Type: application/json</code>）</td>
                  </tr>
                </tbody>
              </table>
            </div>

            <h3>身份认证</h3>
            <p>
              所有 API 请求均需在 HTTP 请求头中携带 Bearer Token 进行身份验证：
            </p>
            <div class="code-block">
              <div class="code-header">
                <span class="code-lang">http header</span>
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
              </div>
              <pre><code>Authorization: Bearer sk-YOUR_API_KEY</code></pre>
            </div>

            <div class="callout callout-info">
              <div class="callout-title">&#128161; 提示</div>
              <p>
                本平台完全兼容 OpenAI API 规范。如果您的项目已经使用了 OpenAI
                官方 SDK，只需将 <code>base_url</code> 修改为
                <code>https://api.xiaobocode.com/v1</code>，并将
                <code>api_key</code> 替换为本平台的密钥即可。
              </p>
            </div>
          </section>
          <!-- Request Format -->
          <section id="request-format">
            <h2>请求格式</h2>
            <p>
              平台兼容 OpenAI 和 Claude 请求API:
              <code>/v1/chat/completions</code>、<code>/v1/responses</code> 、<code>/v1/messages</code>
              ，OpenAI图片生成API: <code>/v1/images/generations</code>
            </p>

            <h3>请求参数</h3>
            <div class="table-wrapper">
              <table>
                <thead>
                  <tr>
                    <th>参数</th>
                    <th>类型</th>
                    <th>必填</th>
                    <th>说明</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td><code>model</code></td>
                    <td>string</td>
                    <td>是</td>
                    <td>
                      模型标识，如
                      <code>gpt-5.5</code>、<code>gpt-image-2</code>、<code>claude-opus-4-8</code>
                    </td>
                  </tr>
                  <tr>
                    <td><code>messages</code></td>
                    <td>array</td>
                    <td>是</td>
                    <td>
                      对话消息数组，每条包含 <code>role</code> 和
                      <code>content</code>
                    </td>
                  </tr>
                  <tr>
                    <td><code>temperature</code></td>
                    <td>number</td>
                    <td>否</td>
                    <td>采样温度，范围 0-2，默认 1。数值越低输出越确定</td>
                  </tr>
                  <tr>
                    <td><code>max_tokens</code></td>
                    <td>integer</td>
                    <td>否</td>
                    <td>最大输出 token 数</td>
                  </tr>
                  <tr>
                    <td><code>stream</code></td>
                    <td>boolean</td>
                    <td>否</td>
                    <td>是否启用流式输出，默认 false</td>
                  </tr>
                  <tr>
                    <td><code>top_p</code></td>
                    <td>number</td>
                    <td>否</td>
                    <td>核采样概率，范围 0-1</td>
                  </tr>
                  <tr>
                    <td><code>frequency_penalty</code></td>
                    <td>number</td>
                    <td>否</td>
                    <td>频率惩罚，范围 -2 到 2</td>
                  </tr>
                  <tr>
                    <td><code>presence_penalty</code></td>
                    <td>number</td>
                    <td>否</td>
                    <td>存在惩罚，范围 -2 到 2</td>
                  </tr>
                </tbody>
              </table>
            </div>

            <h3>消息角色</h3>
            <div class="table-wrapper">
              <table>
                <thead>
                  <tr>
                    <th>角色</th>
                    <th>说明</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td><code>system</code></td>
                    <td>
                      系统指令，设定 AI 的行为方式和背景。放在消息数组的第一条。
                    </td>
                  </tr>
                  <tr>
                    <td><code>user</code></td>
                    <td>用户发送的消息内容。</td>
                  </tr>
                  <tr>
                    <td><code>assistant</code></td>
                    <td>AI 助手的回复，用于提供对话历史上下文。</td>
                  </tr>
                </tbody>
              </table>
            </div>

            <h3>完整请求示例</h3>
            <div class="code-block">
              <div class="code-header">
                <span class="code-lang">json</span>
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
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
          </section>

          <!-- Response Format -->
          <section id="response-format">
            <h2>响应格式</h2>

            <h3>标准响应（非流式）</h3>
            <div class="code-block">
              <div class="code-header">
                <span class="code-lang">json</span>
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
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
            <p>
              当请求中设置 <code>"stream": true</code> 时，响应将以 Server-Sent
              Events 格式逐块返回：
            </p>
            <div class="code-block">
              <div class="code-header">
                <span class="code-lang">text</span>
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
              </div>
              <pre><code>data: {"id":"chatcmpl-abc123","object":"chat.completion.chunk","choices":[{"index":0,"delta":{"role":"assistant"},"finish_reason":null}]}

data: {"id":"chatcmpl-abc123","object":"chat.completion.chunk","choices":[{"index":0,"delta":{"content":"以下"},"finish_reason":null}]}

data: {"id":"chatcmpl-abc123","object":"chat.completion.chunk","choices":[{"index":0,"delta":{"content":"是"},"finish_reason":null}]}

data: [DONE]</code></pre>
            </div>
            <p>
              每个 <code>data:</code> 行包含一个 JSON 对象，<code
                >delta.content</code
              >
              字段为本次增量输出的文本片段。当收到
              <code>data: [DONE]</code> 时，表示生成结束。
            </p>
          </section>

          <!-- Error Codes -->
          <section id="error-codes">
            <h2>错误码说明</h2>
            <p>
              当请求发生错误时，API 将返回对应的 HTTP 状态码和 JSON
              格式的错误信息：
            </p>
            <div class="code-block">
              <div class="code-header">
                <span class="code-lang">json</span>
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
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
                <thead>
                  <tr>
                    <th>状态码</th>
                    <th>含义</th>
                    <th>说明与处理建议</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td><span class="status-4xx">400</span></td>
                    <td>请求参数错误</td>
                    <td>
                      请求体格式不正确或缺少必需参数。请检查 JSON 格式、<code
                        >model</code
                      >
                      字段是否正确、<code>messages</code> 数组是否为空。
                    </td>
                  </tr>
                  <tr>
                    <td><span class="status-4xx">401</span></td>
                    <td>身份认证失败</td>
                    <td>
                      API Key 无效、已过期或未提供。请检查
                      <code>Authorization</code> 请求头格式是否为
                      <code>Bearer sk-xxx</code>。
                    </td>
                  </tr>
                  <tr>
                    <td><span class="status-4xx">403</span></td>
                    <td>权限不足</td>
                    <td>
                      当前 API Key
                      无权访问所请求的模型或资源。请确认账户余额充足且未被禁用。
                    </td>
                  </tr>
                  <tr>
                    <td><span class="status-4xx">429</span></td>
                    <td>请求过于密集</td>
                    <td>
                      短时间内请求过多。建议实现指数退避重试策略，或适当降低并发数。通常等待数秒后重试即可。
                    </td>
                  </tr>
                  <tr>
                    <td><span class="status-5xx">500</span></td>
                    <td>服务器内部错误</td>
                    <td>
                      服务端出现未预期的异常。此类错误通常为暂时性故障，建议稍后重试。如持续出现，请联系技术支持。
                    </td>
                  </tr>
                  <tr>
                    <td><span class="status-5xx">503</span></td>
                    <td>服务暂时不可用</td>
                    <td>
                      模型服务暂时过载或维护中。系统将自动进行负载均衡切换，建议等待片刻后重试。
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>

            <div class="callout callout-tip">
              <div class="callout-title">&#9881; 重试建议</div>
              <p>
                对于 429、500、503 错误，建议实现自动重试机制：首次等待 1
                秒，之后每次翻倍（1s → 2s → 4s），最多重试 3
                次。大部分暂时性故障可在数秒内自动恢复。
              </p>
            </div>
          </section>

          <!-- Pricing -->
          <section id="pricing">
            <h2>计费说明</h2>

            <h3>充值比例</h3>
            <p>平台采用美元余额计费制度，人民币充值采用 1:1 比例</p>
            <div class="callout callout-info">
              <div class="callout-title">&#128178; 充值比例</div>
              <p>
                <strong>&#165;1 人民币 = $1 美元余额</strong>（1:1），最低充值 5 元。充值后余额以美元显示，API 调用按照模型的 token消耗从余额中扣除。
              </p>
            </div>

            <h3>分组倍率说明</h3>
            <p>
              平台同时支持按量充值，按 API Key 所属分组倍率扣费：分组价格 =
              官方价格 × 分组倍率 ÷ 7(美元汇率)
            </p>
          </section>

          <!-- Code Examples -->
          <section id="example-python">
            <h2>代码示例</h2>

            <h3>Python（使用 OpenAI SDK）</h3>
            <p>首先安装 OpenAI Python SDK：</p>
            <div class="code-block">
              <div class="code-header">
                <span class="code-lang">bash</span>
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
              </div>
              <pre><code>pip install openai</code></pre>
            </div>

            <div class="code-block">
              <div class="code-header">
                <span class="code-lang">python</span>
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
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
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
              </div>
              <pre><code>npm install openai</code></pre>
            </div>

            <div class="code-block">
              <div class="code-header">
                <span class="code-lang">javascript</span>
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
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
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
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
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
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
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
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
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
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

          <!-- Codex -->
          <section id="codex">
            <h2>Codex 接入指南</h2>
            <p>
              Codex 是 OpenAI 推出的命令行 AI
              编程助手。您可以通过创建本地配置文件，将 Codex 接入本平台的 OpenAI
              兼容 Responses API。
            </p>

            <h3>Windows 配置</h3>
            <p>
              在 PowerShell 中执行以下命令，自动创建 Codex 配置目录、模型配置和
              API Key 文件：
            </p>

            <div class="code-block">
              <div class="code-header">
                <span class="code-lang">powershell</span>
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
              </div>
              <pre><code># 创建新目录
mkdir "$env:USERPROFILE\.codex"

# 创建 config.toml
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
base_url = "https://api.xiaobocode.com/v1"
wire_api = "responses"
requires_openai_auth = true
"@ | Out-File -FilePath "$env:USERPROFILE\.codex\config.toml" -Encoding utf8

# 创建 auth.json
@"
{
  "OPENAI_API_KEY": "YOUR_API_KEY"
}
"@ | Out-File -FilePath "$env:USERPROFILE\.codex\auth.json" -Encoding utf8</code></pre>
            </div>

            <h3>Linux / macOS 配置</h3>
            <p>
              在终端中执行以下命令，自动创建 Codex 配置目录、模型配置和 API Key
              文件：
            </p>

            <div class="code-block">
              <div class="code-header">
                <span class="code-lang">bash</span>
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
              </div>
              <pre><code># 创建配置目录
mkdir -p ~/.codex

# 创建 config.toml
cat > ~/.codex/config.toml &lt;&lt; 'EOF'
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
base_url = "https://api.xiaobocode.com/v1"
wire_api = "responses"
requires_openai_auth = true
EOF

# 创建 auth.json
cat > ~/.codex/auth.json &lt;&lt; 'EOF'
{
  "OPENAI_API_KEY": "YOUR_API_KEY"
}
EOF</code></pre>
            </div>

            <h3>验证配置</h3>
            <p>配置完成后，在项目目录中启动 Codex 即可：</p>
            <div class="code-block">
              <div class="code-header">
                <span class="code-lang">bash</span>
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
              </div>
              <pre><code><span class="cm"># 启动 Codex</span>
<span class="var">codex</span>

<span class="cm"># 或在项目目录中启动</span>
<span class="kw">cd</span> your-project && <span class="var">codex</span></code></pre>
            </div>

            <div class="callout callout-tip">
              <div class="callout-title">&#9989; 提示</div>
              <p>
                请将 <code>YOUR_API_KEY</code> 替换为您在平台生成的真实 API
                Key。配置文件中的 <code>base_url</code> 需要保留
                <code>/v1</code> 后缀。
              </p>
            </div>
          </section>

          <!-- Claude Code -->
          <section id="claude-code">
            <h2>Claude Code 接入指南</h2>
            <p>
              Claude Code 是 Anthropic
              推出的命令行AI编程助手。您可以通过本平台的 API 网关来使用 Claude
              Code，享受更优惠的价格和更稳定的国内访问体验。
            </p>

            <h3>配置方法</h3>
            <p>在您的终端环境中设置以下环境变量：</p>

            <h4>Linux / macOS</h4>
            <div class="code-block">
              <div class="code-header">
                <span class="code-lang">bash</span>
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
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
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
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
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
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
                <button type="button" class="copy-btn" @click="copyCodeBlock">
                  复制
                </button>
              </div>
              <pre><code><span class="cm"># 启动 Claude Code</span>
<span class="var">claude</span>

<span class="cm"># 或在项目目录中启动</span>
<span class="kw">cd</span> your-project && <span class="var">claude</span></code></pre>
            </div>

            <div class="callout callout-info">
              <div class="callout-title">&#128161; 提示</div>
              <p>
                请确保 <code>ANTHROPIC_BASE_URL</code> 设置为
                <code>https://api.xiaobocode.com</code>（不含
                <code>/v1</code> 后缀），Claude Code 会自动拼接路径。<code
                  >ANTHROPIC_API_KEY</code
                >
                使用您在本平台生成的 API Key。
              </p>
            </div>
          </section>

          <!-- Cherry Studio -->
          <section id="cherry-studio">
            <h2>Cherry Studio 接入</h2>
            <p>
              Cherry Studio 是一款精美的跨平台 AI
              桌面客户端，支持多模型对话、知识库管理、Prompt
              模板等丰富功能，界面友好，非常适合日常 AI 对话使用。
            </p>

            <div class="steps">
              <div class="step">
                <h4>下载安装</h4>
                <p>
                  前往
                  <a
                    href="https://cherry-ai.com"
                    target="_blank"
                    rel="noreferrer"
                    >Cherry Studio 官网</a
                  >
                  下载适合您操作系统的安装包（支持
                  Windows、macOS、Linux），完成安装并打开应用。
                </p>
              </div>
              <div class="step">
                <h4>添加自定义服务商</h4>
                <p>
                  打开 Cherry Studio，进入 <strong>设置</strong> &rarr;
                  <strong>模型服务商</strong>，点击
                  <strong>添加自定义服务商</strong>，填写以下信息：
                </p>
                <div class="config-box">
                  <div class="config-row">
                    <span class="config-label">服务商名称</span>
                    <span class="config-value">XiaoboCode</span>
                  </div>
                  <div class="config-row">
                    <span class="config-label">API 地址</span>
                    <span class="config-value"
                      >https://api.xiaobocode.com/v1</span
                    >
                  </div>
                  <div class="config-row">
                    <span class="config-label">API Key</span>
                    <span class="config-value">sk-YOUR_API_KEY</span>
                  </div>
                </div>
              </div>
              <div class="step">
                <h4>添加模型</h4>
                <p>
                  在刚添加的服务商下方，点击
                  <strong>添加模型</strong>，手动输入模型名称（如
                  <code>gpt-5.5</code
                  >、<code>claude-opus-4-8</code>），或点击「获取模型列表」自动拉取可用模型。
                </p>
              </div>
              <div class="step">
                <h4>开始对话</h4>
                <p>
                  返回主界面，在顶部的模型选择器中切换到您刚添加的模型，即可开始对话。Cherry
                  Studio 支持多轮对话、Markdown 渲染、代码高亮等功能。
                </p>
              </div>
            </div>

            <div class="callout callout-tip">
              <div class="callout-title">&#9989; 提示</div>
              <p>
                Cherry Studio
                支持同时配置多个服务商和模型，您可以在对话中随时切换模型进行对比。还支持设置系统提示词、调整温度等参数。
              </p>
            </div>
          </section>

          <!-- 更多客户端 -->
          <section id="more-clients">
            <h2>更多客户端</h2>
            <p>
              除了以上介绍的工具，还有许多优秀的 AI 客户端支持自定义 OpenAI API
              地址，均可无缝接入本平台。
            </p>

            <div class="callout callout-tip">
              <div class="callout-title">&#9989; 通用接入方法</div>
              <p>
                只要支持自定义 OpenAI API
                地址的工具，都可以接入本平台。通用配置如下：
              </p>
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
            <p>
              如果您获得了兑换码，可以按照以下步骤兑换余额或并发数到您的账户。
            </p>

            <div class="steps">
              <div class="step">
                <h4>登录平台</h4>
                <p>
                  打开浏览器访问
                  <a
                    href="https://xiaobocode.com"
                    target="_blank"
                    rel="noreferrer"
                    >https://xiaobocode.com</a
                  >，使用您的账号登录平台。
                </p>
              </div>
              <div class="step">
                <h4>进入兑换页面</h4>
                <p>
                  登录后，点击左侧菜单中的「兑换码」选项，或直接访问兑换页面。
                </p>
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
                <p>
                  兑换成功后，余额或并发数会自动增加到您的账户，您可以在仪表盘查看更新后的余额。
                </p>
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
              <summary class="faq-q">
                平台支持哪些模型？模型列表会更新吗？
              </summary>
              <div class="faq-a">
                当前支持 GPT-5.5、GPT-5.4 系列模型以及 Claude Opus/Sonnet/Haiku
                系列最新。模型列表会随官方发布和平台运营状态更新，具体可用模型请以控制台和「支持的模型」章节为准。
              </div>
            </details>

            <details class="faq-item">
              <summary class="faq-q">
                API 接口与 OpenAI 官方完全兼容吗？
              </summary>
              <div class="faq-a">
                是的。本平台兼容 OpenAI 和 Claude 的
                <code>/v1/chat/completions</code>、<code>/v1/responses</code>
                、<code>/v1/messages</code> 接口规范，包括流式输出、function
                calling等能力。使用 OpenAI 官方 Python/Node.js SDK 时，只需修改
                <code>base_url</code> 和 <code>api_key</code> 两个参数即可切换。
              </div>
            </details>

            <details class="faq-item">
              <summary class="faq-q">遇到 429 错误怎么办？</summary>
              <div class="faq-a">
                429
                通常表示短时间内请求过多。请在客户端加入指数退避重试，并适当降低并发；如果持续出现，可联系客服协助排查当前账号和模型状态。
              </div>
            </details>

            <details class="faq-item">
              <summary class="faq-q">充值余额可以退款吗？</summary>
              <div class="faq-a">
                直接充值的未使用余额支持退款，请联系客服处理。建议首次使用时先小额充值进行测试。
              </div>
            </details>

            <details class="faq-item">
              <summary class="faq-q">出现 401 错误怎么排查？</summary>
              <div class="faq-a">
                401 错误意味着身份认证失败，请按以下顺序排查：(1) 检查 API Key
                是否正确复制，没有多余的空格或换行符；(2) 确认请求头格式为
                <code>Authorization: Bearer sk-xxx</code>，注意 Bearer
                后面有一个空格；(3)
                在控制台确认该令牌是否仍处于「启用」状态且未被删除；(4)
                确认账户余额大于 0。
              </div>
            </details>

            <details class="faq-item">
              <summary class="faq-q">如何查看 API 调用用量和余额？</summary>
              <div class="faq-a">
                登录
                <a
                  href="https://xiaobocode.com"
                  target="_blank"
                  rel="noreferrer"
                  >控制台</a
                >
                后，在「仪表盘」页面可以查看当前余额和近期消费趋势。「日志」页面可以查看每一次
                API 调用的详细记录，包括使用的模型、token 消耗数量和扣费金额。
              </div>
            </details>

            <details class="faq-item">
              <summary class="faq-q">可以同时使用多个 API Key 吗？</summary>
              <div class="faq-a">
                可以。您可以在控制台创建多个 API
                Key，分别用于不同的项目或环境（如开发环境和生产环境）。所有 Key
                共享同一账户余额，但日志中会区分来源，方便您追踪各项目的用量。您也可以为每个
                Key 设置独立的额度上限。
              </div>
            </details>

            <details class="faq-item">
              <summary class="faq-q">服务的稳定性和可用性如何保障？</summary>
              <div class="faq-a">
                平台采用多节点架构，配合自动负载均衡和故障转移机制。当某个节点异常时，系统会在毫秒级自动切换到健康节点，用户侧几乎无感知。历史可用性维持在
                99.9% 以上。此外，平台有完善的监控告警体系，运维团队全天候值守。
              </div>
            </details>

            <details class="faq-item">
              <summary class="faq-q">
                国内访问需要翻墙吗？
              </summary>
              <div class="faq-a">
                不需要。直接通过浏览器访问 <code>https://xiaobocode.com</code> 即可，无需任何代理或VPN。
              </div>
            </details>

            <details class="faq-item">
              <summary class="faq-q">
                是否支持 function calling / tools 功能？
              </summary>
              <div class="faq-a">
                支持。对于 GPT 系列和 Claude 系列的模型，function
                calling（工具调用）功能完全透传，参数格式与 OpenAI
                官方规范一致。您可以在请求中传入
                <code>tools</code>
                参数定义可调用的函数，模型会根据上下文判断是否需要调用。
              </div>
            </details>
          </section>
        </article>
      </section>
    </main>

    <MarketingFooter />
  </div>
</template>

<script setup lang="ts">
import { computed, nextTick, onMounted, watch, watchEffect } from "vue";
import { useRoute } from "vue-router";
import MarketingFooter from "@/components/marketing/MarketingFooter.vue";
import MarketingHeader from "@/components/marketing/MarketingHeader.vue";
import { useAppStore } from "@/stores";

const appStore = useAppStore();
const route = useRoute();

const navGroups = [
  {
    title: "入门",
    items: [{ id: "quickstart", title: "快速开始" }],
  },
  {
    title: "接入指南",
    items: [
      { id: "api-basics", title: "API 基础信息" },
      { id: "request-format", title: "请求格式" },
      { id: "response-format", title: "响应格式" },
      { id: "error-codes", title: "错误码说明" },
    ],
  },
  {
    title: "计费",
    items: [{ id: "pricing", title: "计费说明" }],
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
      { id: "codex", title: "Codex 接入" },
      { id: "claude-code", title: "Claude Code 接入" },
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

async function scrollToHashTarget(hash: string): Promise<void> {
  const rawId = hash.startsWith("#") ? hash.slice(1) : hash;
  if (!rawId) {
    return;
  }

  let targetId: string;
  try {
    targetId = decodeURIComponent(rawId);
  } catch {
    targetId = rawId;
  }

  await nextTick();
  window.requestAnimationFrame(() => {
    document.getElementById(targetId)?.scrollIntoView({ block: "start" });
  });
}

onMounted(() => {
  void scrollToHashTarget(route.hash);
});

watch(
  () => route.hash,
  (hash) => {
    void scrollToHashTarget(hash);
  },
);

async function copyCodeBlock(event: MouseEvent): Promise<void> {
  const button =
    event.currentTarget instanceof HTMLButtonElement
      ? event.currentTarget
      : null;
  const code = button
    ?.closest(".code-block")
    ?.querySelector("code")?.textContent;
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
  margin-top: 1rem;
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

.docs-content :deep(.pricing-grid) {
  display: grid;
  gap: 1rem;
  margin: 1.5rem 0;
}

.docs-content :deep(.pricing-grid) {
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
}

.docs-content :deep(.feature-card),
.docs-content :deep(.pricing-card),
.docs-content :deep(.config-box),
.docs-content :deep(.faq-item) {
  border-radius: 1rem;
  border: 1px solid rgb(229 231 235);
  background: rgb(255 255 255 / 0.72);
  box-shadow: 0 10px 30px rgb(15 23 42 / 0.04);
}

.dark .docs-content :deep(.feature-card),
.dark .docs-content :deep(.pricing-card),
.dark .docs-content :deep(.config-box),
.dark .docs-content :deep(.faq-item) {
  border-color: rgb(55 65 81 / 0.7);
  background: rgb(17 24 39 / 0.48);
}

.docs-content :deep(.feature-card),
.docs-content :deep(.pricing-card) {
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
  margin: 0.5rem 0 1rem;
  padding: 0 1rem;
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
  padding: 0.5rem 1rem;
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
  transition:
    background-color 0.2s,
    color 0.2s;
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
}

.docs-content :deep(code) {
  font-family:
    ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono",
    monospace;
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
  font-family:
    ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono",
    monospace;
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
