🦞 OpenClaw 单机多用户沙箱部署指南（macOS）

在只有一台 Mac 的情况下，构建安全隔离的 AI Agent 运行环境
无需 Docker / Kubernetes / 云服务器

⸻

🧠 一、核心理念

🎯 单机多用户 = 系统级沙箱

通过 macOS 多用户机制，实现：

主工作环境 ≠ AI Agent 环境


⸻

🏗️ 架构图

graph TD
    A[MacBook / macOS] --> B[主用户 Main User]
    A --> C[Sandbox 用户 openclaw]

    C --> D[OpenClaw Gateway]
    D --> E[本地模型 Ollama]
    D --> F[云模型 Kimi / DeepSeek]

    D --> G[127.0.0.1 本地访问]

    style C fill:#1e293b,color:#fff
    style D fill:#0ea5e9,color:#fff


⸻

🧠 为什么这样做？

🔒 安全性
	•	不暴露公网（127.0.0.1）
	•	API Key 完全隔离
	•	主系统不被污染
	•	可随时重置环境

⸻

⚡ 可用性
	•	无需服务器
	•	无需 Docker
	•	原生 macOS
	•	零运维成本

⸻

🧩 灵活性
	•	多 user = 多 agent
	•	支持本地 + 云模型
	•	可独立测试环境

⸻

⚙️ 二、最低配置（MVP）

macOS + 标准用户 + Node.js + OpenClaw + API Key


⸻

🚀 三、完整部署步骤

⸻

👤 Step 1：创建 Sandbox 用户

路径：

System Settings → Users & Groups → Add User

配置：

用户名：openclaw
类型：Standard


⸻

🔑 Step 2：登录新用户

openclaw


⸻

🍺 Step 3：安装 Homebrew

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

配置：

echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"

验证：

brew -v


⸻

🧱 Step 4：安装 Node.js

brew install node@22


⸻

🦞 Step 5：安装 OpenClaw

✅ 推荐（官方脚本）

curl -fsSL https://openclaw.ai/install.sh | bash


⸻

🔁 备用（npm）

npm install -g openclaw

若报权限错误：

mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=$HOME/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc


⸻

⚙️ 四、初始化配置（逐步详解）

执行：

openclaw configure


⸻

🧩 配置步骤

1️⃣ Onboarding

QuickStart


⸻

2️⃣ Gateway

Local gateway


⸻

3️⃣ Workspace

默认路径


⸻

4️⃣ Default Model

Keep current（Kimi）


⸻

5️⃣ Port

18789


⸻

6️⃣ Bind（最关键）

127.0.0.1

❗禁止：

0.0.0.0 / LAN


⸻

7️⃣ Auth

Token


⸻

8️⃣ Tailscale

Off


⸻

9️⃣ Token 生成

Generate/store plaintext


⸻

🔟 Token 存储

Environment variable


⸻

1️⃣1️⃣ 环境变量名

OPENCLAW_GATEWAY_TOKEN


⸻

1️⃣2️⃣ Search

Brave 或 Skip


⸻

1️⃣3️⃣ Hooks

Skip


⸻

1️⃣4️⃣ Service

Yes（必须）


⸻

1️⃣5️⃣ TUI

默认


⸻

🔐 配置总结

Loopback + Token + No Tailscale + Service


⸻

🔑 五、API Key 配置

nano ~/.zshrc

添加：

export MOONSHOT_API_KEY="your_key"
export DEEPSEEK_API_KEY="your_key"

生效：

source ~/.zshrc


⸻

⚠️ LaunchAgent 必须配置

launchctl setenv MOONSHOT_API_KEY "your_key"
launchctl setenv DEEPSEEK_API_KEY "your_key"


⸻

🔄 六、启动服务

openclaw gateway restart


⸻

🧪 七、验证

openclaw gateway probe

浏览器访问：

http://127.0.0.1:18789/


⸻

💻 八、本地模型（可选）

brew install ollama
ollama serve
ollama pull qwen2.5:7b


⸻

🔐 九、安全模型

层级	机制
用户	macOS 隔离
网络	127.0.0.1
认证	Token
访问	Pairing


⸻

当前安全等级

公网 ❌
内网 ❌
本机 ✅


⸻

🧹 十、重置

rm -rf ~/.openclaw
openclaw configure


⸻

🚀 十一、进阶方向
	•	Telegram Bot
	•	Tailscale 远程访问
	•	多模型调度
	•	多用户多 Agent

⸻

⭐ 推荐用途
	•	AI Agent 开发
	•	本地实验环境
	•	一台 Mac + 多用户
	•	模型测试平台

