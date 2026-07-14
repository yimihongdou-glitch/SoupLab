# 🐢 海龟汤 - 情境推理游戏

> 纯前端单页面应用，无需后端服务，开箱即玩

## v1.1.0 更新日志（2026-07-12 ~ 2026-07-14）

### 🤖 AI 主持人

- **接入大模型 API**：支持配置 DeepSeek / OpenAI 等大模型 API，AI 真正理解语义，同义词、间接表述都能正确判断
- **内置匹配优化**：重写内置 AI 匹配逻辑，回答更严谨，不会随意回答"是"误导推理方向
- **回答格式优化**：AI 回复不再附带多余标签，只显示干净的是/否/不相关
- **猜中检测**：当玩家说出完整汤底时，AI 会自动恭喜并揭晓真相，游戏结束
- **加载体验**：等待 AI 回复时显示三点跳动动画，超过 3 秒自动提示"请稍候"

### 👥 多人联机（新功能）

- **跨设备联机**：集成 Firebase Realtime Database，创建房间后把 4 位房间号发给朋友，即可真正跨设备联机游玩
- **实时同步**：玩家列表、聊天消息、题目切换实时同步给房间内所有人
- **本地同屏回退**：未配置 Firebase 时自动回退到本地同屏模式

### 🎮 游戏体验

- **聊天输入框升级**：支持自动伸高、回车发送、Shift+Enter 换行
- **题库选题直达**：从题库选择题目后可直接开始单人游戏，无需手动切换
- **汤底优化**：答案更精炼，去除了冗余内容
- **游戏结束交互**：猜中或放弃后输入框自动禁用，点击"下一题"重新开始

### 🔒 安全

- 全面修复 XSS 漏洞：用户输入的昵称、自建题目内容等全部经过转义处理
- 设置页新增 API Key 安全提示
- 新增 Cloudflare Worker 代理部署方式，API Key 存在云端不暴露在源码中
- 房间号增加格式校验，只允许 4 位纯数字

### 🚀 部署

- 开发者可预设 API 配置，用户打开即玩无需自行配置
- 新增 `worker.js` 代理文件，支持 Cloudflare Workers 免费部署
- 支持 GitHub Pages / Vercel / Netlify 等平台一键部署
- 新增 Firebase 配置支持，硬编码后所有人自动联机

---

## 功能概览

| 模式 | 说明 |
|------|------|
| 🤖 单人 AI 模式 | 与 AI 主持人一对一推理，支持 AI 出题或自选题库 |
| 👥 多人在线 | 创建/加入房间，一人出题其他人猜，支持跨设备联机 |
| 📚 题库 / 主持人模式 | 浏览 62 道经典题目，适合线下聚会主持人查阅 |

## 快速开始

直接打开 `sea-turtle-soup.html` 即可游玩，无需任何配置。

### 配置 AI 主持人（推荐）

内置 AI 基于关键词匹配，配置大模型 API 后可获得更好的游戏体验：

1. 在 [DeepSeek 开放平台](https://platform.deepseek.com/) 注册并创建 API Key（几块钱可玩很久）
2. 打开游戏 → 设置 → AI API 配置
3. 选择 DeepSeek，填入 Key，保存
4. 即可享受真正 AI 主持人体验

### 配置多人联机

1. 访问 [Firebase 控制台](https://console.firebase.google.com/) 创建项目
2. 创建 Realtime Database，记下数据库地址
3. 项目设置 → 通用 → 获取网页应用配置
4. 在 `sea-turtle-soup.html` 中找到 `PRESET_FIREBASE_CONFIG`，填入配置值
5. 数据库规则设为允许读写（公开游戏）
6. 重新部署网页，所有人打开即可联机

### 开发者部署（让用户免配置）

**方式一：直接填入 API Key**（适合分享给朋友）

在 `sea-turtle-soup.html` 中找到以下三行，填入你的配置：

```javascript
var PRESET_API_KEY = 'sk-xxx';
var PRESET_API_URL = 'https://api.deepseek.com/v1/chat/completions';
var PRESET_MODEL = 'deepseek-chat';
```

**方式二：Cloudflare Worker 代理**（适合公开发布，Key 不暴露）

1. 注册 [Cloudflare](https://dash.cloudflare.com/)（免费）
2. Workers 和 Pages → 创建 Worker → 粘贴 `worker.js` 内容 → 部署
3. 在 Worker 设置中添加环境变量：
   - `API_KEY` = 你的真实 API Key
   - `API_URL` = `https://api.deepseek.com/v1/chat/completions`
   - `API_MODEL` = `deepseek-chat`
4. 在 HTML 中填入 Worker 地址：

```javascript
var PRESET_API_KEY = 'proxy';
var PRESET_API_URL = 'https://xxx.your-name.workers.dev';
var PRESET_MODEL = 'deepseek-chat';
```

5. 将 `sea-turtle-soup.html` 部署到 GitHub Pages / Vercel / Netlify

## 文件说明

| 文件 | 说明 |
|------|------|
| `sea-turtle-soup.html` | 游戏主文件（单文件应用，含全部 HTML/CSS/JS 和 62 道题库） |
| `worker.js` | Cloudflare Worker 代理代码（可选，用于安全部署 AI API） |
| `README.md` | 本文档 |

## 技术栈

- 纯 HTML / CSS / JavaScript，零依赖
- 支持接入 OpenAI / DeepSeek / 自定义 API
- Firebase Realtime Database（可选，用于多人联机）
- 数据存储：浏览器 localStorage

## 版本历史

| 版本 | 日期 | 说明 |
|------|------|------|
| v1.1.0 | 2026-07-14 | AI 主持人升级、多人联机、猜中检测、安全修复、Worker 代理、代码清理 |
| v1.0.0 | - | 初始版本：62 道题库、单人/多人/题库三大模式 |
