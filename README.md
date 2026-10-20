# 🐝 妙语灵蜂 (PromptBee)

面向中小学生的 **AI 提示词工程（Prompt Engineering）学习平台**。通过 12 节体系化课程、AI 智能评分、竞技场对战和积分勋章激励，教学生「学会好好向 AI 提问」。

## ✨ 核心功能

- **体系化课程**：基于 PCF-12 框架的 12 节课，从入门（AI 是什么）到竞赛级挑战，由浅入深
- **AI 智能评分**：学生提交提示词练习后，由 LLM 自动打分（满分 100）并给出改进建议，可反复提交冲高分
- **竞技场（Arena）**：提示词对战比拼，由 AI 评审决出胜负
- **社区**：作品分享与论坛交流
- **商城**：课程、勋章、证书等商品兑换与购买
- **证书系统**：PDF 证书生成、邮件投递、扫码验真
- **积分与勋章**：成长激励体系，含成长报告
- **礼品卡**：激活、赠送与查询
- **多端登录**：邮箱验证码、微信 OAuth、支付宝 OAuth
- **多语言**：简体中文 / 繁体中文 / 英文
- **支付**：微信支付、支付宝、Stripe

## 🛠 技术栈

| 层次 | 技术 |
|------|------|
| 工程化 | pnpm workspace Monorepo · TypeScript 5.9 · Node.js 24 |
| 前端 | React 18 · Vite · Wouter · TanStack Query · TailwindCSS + shadcn/ui · tRPC Client |
| 后端 | Express 5 · tRPC · superjson |
| 数据库 | PostgreSQL · Drizzle ORM |
| 认证 | JWT（`jose`，Cookie `app_session_id`） |
| LLM | 火山方舟（豆包 Doubao） |
| 邮件 | Resend / Nodemailer |
| 支付 | 微信支付 · 支付宝 · Stripe |
| 证书 | PDFKit · 二维码扫码验证 |

## 📁 项目结构

```text
├── artifacts/
│   ├── main/            # 主前端（React + Vite，所有页面与路由）
│   ├── api-server/      # 后端 API（Express 5 + tRPC，tRPC 挂载于 /api/trpc）
│   │   └── src/
│   │       ├── index.ts       # 入口，读取 PORT / API_SERVER_PORT
│   │       ├── routers/       # 子路由（lesson、emailAuth、shop、order 等）
│   │       └── _core/         # 上下文、邮件、LLM、Cookie 等基础设施
│   └── mockup-sandbox/  # UI 组件沙盒（开发用）
├── lib/
│   ├── db/              # Drizzle ORM schema 与数据库连接（lib/db/src/schema/index.ts）
│   ├── shared/          # 前后端共享类型、常量、商品数据
│   ├── api-spec/        # OpenAPI 规范（orval 生成客户端的来源）
│   ├── api-zod/         # 由 OpenAPI 生成的 Zod 类型
│   └── api-client-react/ # 生成的 React API 客户端
├── deploy/              # 生产部署脚本（PM2），见 deploy/README.md
├── scripts/             # 工具脚本（SQL 导入、指南 PDF 生成等）
└── public.sql           # 数据库结构/数据导出
```

## 🚀 快速开始

### 环境要求

- Node.js 24+
- pnpm
- PostgreSQL

### 安装与启动

```bash
# 安装依赖
pnpm install

# 配置环境变量（参考下方说明，在根目录创建 .env）

# 推送数据库 schema
pnpm --filter @workspace/db run push

# 类型检查（从根目录以 project references 方式执行）
pnpm run typecheck

# 启动前端（artifacts/main）
pnpm --filter @workspace/main run dev

# 启动后端 API（artifacts/api-server）
pnpm --filter @workspace/api-server run dev
```

开发模式下 Vite 会将 `/api` 请求代理到后端服务。

### 环境变量

在根目录 `.env` 中配置（切勿提交真实密钥）：

| 变量 | 说明 |
|------|------|
| `DATABASE_URL` | PostgreSQL 连接串 |
| `JWT_SECRET` | JWT 签名密钥 |
| `PORT` / `API_SERVER_PORT` | 服务端口 |
| `ARK_BASE_URL` / `ARK_API_KEY` / `ARK_MODEL` | 火山方舟 LLM 接入配置 |
| `WECHAT_APPID` / `WECHAT_MCHID` 等 | 微信登录与支付 |
| `ALIPAY_APPID` 等 | 支付宝登录与支付 |
| `STRIPE_SECRET_KEY` | Stripe 支付（懒加载） |
| `RESEND_API_KEY` | 邮件发送 |

## 📦 部署

服务器部署采用 PM2，提供一键脚本，详见 [deploy/README.md](deploy/README.md)。

```bash
cd deploy
chmod +x *.sh
./deploy-all.sh
```

## 📚 相关文档

- [妙语灵蜂使用指南（学生版）](妙语灵蜂使用指南（学生版）.md) — 面向学生的产品使用说明
- [妙语灵蜂使用指南.html](妙语灵蜂使用指南.html) — 图文版使用指南
- [IMPORT_SQL.md](IMPORT_SQL.md) — 数据库导入说明
- [replit.md](replit.md) — 开发环境架构备注
