# OUNIN 独立站架构文档
> 文档版本：2026-05-12
> 最后更新：MiniMax-M2.7

---

## 一、整体架构概览

OUNIN 独立站采用 **前端与后端分离（Headless）** 架构：

```
┌─────────────────────────────────────────────────────────────┐
│                        用户端                               │
│  https://ounin.pages.dev (Cloudflare Pages CDN)             │
│  https://ouninglobal.com (Cloudflare DNS + Pages)           │
│  https://store.ouninshop.com (Cloudflare DNS → Shopify)     │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                       前端层                                  │
│  GitHub: linoroven897-star/ounin                            │
│  托管：Cloudflare Pages                                      │
│  技术：纯 HTML/CSS/JS，无 CDN 依赖                           │
│  域名：ounin.pages.dev / ouninglobal.com                     │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                       后端层                                  │
│  Shopify（产品目录、订单、支付、税务）                        │
│  GitHub: 无后端代码（纯静态）                                │
└─────────────────────────────────────────────────────────────┘
```

### 核心域名

| 域名 | 类型 | 说明 |
|------|------|------|
| `ounin.pages.dev` | 前端（Cloudflare CDN） | 主站入口 |
| `ouninglobal.com` | 前端（Cloudflare DNS） | 品牌官网 |
| `store.ouninshop.com` | 后端（Shopify） | Shopify 商店后台 |
| `www.ouninglobal.com` | DNS | 品牌官网 www |

---

## 二、前端架构

### 2.1 技术栈

| 组件 | 技术 | 说明 |
|------|------|------|
| 语言 | HTML5 + CSS3 + Vanilla JS | 无框架、无 CDN 依赖 |
| 源码托管 | GitHub | `linoroven897-star/ounin` |
| CDN 分发 | Cloudflare Pages | 自动部署，边缘节点 |
| 图片存储 | GitHub 仓库内 | `assets/images/` |

### 2.2 GitHub 仓库

- **仓库地址**：`https://github.com/linoroven897-star/ounin`
- **分支**：`main`
- **部署方式**：Cloudflare Pages 监听 GitHub，每次 push 自动部署
- **部署状态**：✅ 已启用

### 2.3 Cloudflare Pages 项目

- **项目名称**：`ouni`（或 `ounin`）
- **生产域名**：`https://ounin.pages.dev`
- **自定义域名**：`ouninglobal.com`（通过 Cloudflare DNS 接入）
- **Zone ID**：`91403386245d57e972e35345052bba95`
- **安全配置**：
  - Bot Fight Mode：已开启
  - SSL/TLS：安全模式（Full）
  - 客户端安全（Client-side Security）：已启用

### 2.4 部署流程

```
开发者 → GitHub push → Cloudflare Pages 自动构建 → CDN 分发
```

---

## 三、后端架构

### 3.1 Shopify 商店

- **商店后台**：`https://admin.shopify.com` → 登录后进入
- **商店域名**：`ouunin.myshopify.com`
- **已连接域名**：
  - `store.ouninshop.com`（主域名，Primary）
  - `ouunin.myshopify.com`（Shopify 默认）
  - `ouunsshop.com`
  - `www.ounsshop.com`

### 3.2 Headless 模式说明

当前 Shopify 已切换为 **Headless（无头电商）模式**：
- 在线商店（online store）已关闭，访问会自动跳转到 `ouunin.pages.dev`
- 前端完全由自建静态站接管
- Shopify 仅作为**后端服务**（产品目录、购物车、结算、支付、订单管理、税务）

### 3.3 Shopify 功能模块

| 模块 | 状态 | 说明 |
|------|------|------|
| 产品目录 | ✅ | 5个产品，75个变体 |
| Checkout | ✅ | 已配置，客户信息字段完整 |
| 支付（Payments） | ⚠️ 部分 | PayPal 已接入（Partially active），Shopify Payments 未开通 |
| 税务（Tax Services） | ✅ | 已启用，支持13个国家/地区税务 |
| 域名（Domains） | ✅ | 4个域名全部 Connected |
| 邮件（Email） | ⚠️ | Email 登录已禁用，登录方式为手机/邮箱地址 |

---

## 四、各管理后台登录方式

### 4.1 Shopify 商店后台

| 项目 | 内容 |
|------|------|
| 后台地址 | `https://admin.shopify.com` 或 `https://ouunin.myshopify.com/admin` |
| 登录方式 | Email / 手机号 + 密码 |
| 登录邮箱 | `ouunin2025@gmail.com` |
| **注意** | Email 登录（Social Login）已禁用，需用邮箱密码登录 |

### 4.2 Cloudflare 仪表盘

| 项目 | 内容 |
|------|------|
| 登录地址 | `https://dash.cloudflare.com` |
| 登录方式 | Cloudflare 账号密码 + 2FA |
| 接入域名 | `ouningshop.com`（Zone ID: `91403386245d57e972e35345052bba95`） |
| 主要功能 | DNS 管理、SSL/TLS 配置、Pages 项目、安全设置、Bot Fight Mode |

### 4.3 GitHub 仓库

| 项目 | 内容 |
|------|------|
| 仓库地址 | `https://github.com/linoroven897-star/ounin` |
| 登录方式 | GitHub 账号 + 2FA |
| 主要功能 | 源码管理、分支管理、Deploy key、Actions 日志 |

### 4.4 Google Analytics

| 项目 | 内容 |
|------|------|
| 登录地址 | `https://analytics.google.com` |
| 追踪 ID | GA4（具体 ID 需登录后查看） |
| 追踪范围 | 全站流量、用户行为、转化追踪 |

### 4.5 Google Search Console

| 项目 | 内容 |
|------|------|
| 登录地址 | `https://search.google.com/search-console` |
| 验证域名 | `https://www.ouninglobal.com/` |
| 主要功能 | 关键词排名、索引状态、CTR/Search Appearance |

---

## 五、当前业务数据摘要

> 数据来源：2026-05-11 运营报告

### 5.1 流量（近90天，GA4）

| 指标 | 数值 |
|------|------|
| Sessions | 295 |
| Total Users | 6,487 |
| Engagement Rate | 4.55% |
| 平均每次会话浏览页数 | 4.48 |
| 活跃用户 | 137 人 |
| 跳出率 | 67.21% |
| 平均会话时长 | 2分19秒 |
| 新用户 | 1,730 人 |

### 5.2 搜索表现（近7天，GSC）

| 指标 | 数值 |
|------|------|
| 总展示次数 | 16 |
| 总点击次数 | 205 |
| 平均 CTR | 7.8% |
| TOP 页面 | 首页（14次展示，152次点击） |

### 5.3 销售数据（近30天，Shopify）

| 指标 | 数值 |
|------|------|
| Sessions | 130 |
| Total Sales | $163 USD |
| Orders | 1 |
| Conversion Rate | 0.76% |

---

## 六、域名与 DNS 配置

### 6.1 Cloudflare DNS 记录摘要

| 类型 | 名称 | 内容 | 说明 |
|------|------|------|------|
| A | ouninglobal.com | 指向 Cloudflare Pages | 品牌官网 |
| A | www | 指向 Cloudflare Pages | www 版 |
| CNAME | store | 指向 ouunin.myshopify.com | Shopify 商店 |

### 6.2 DNS 供应商

- **主 DNS**：Cloudflare（域名 `ounsshop.com`）
- **Zone ID**：`91403386245d57e972e35345052bba95`

---

## 七、支付配置

### 7.1 PayPal

| 项目 | 状态 |
|------|------|
| 状态 | Partially Active |
| 手续费率 | 2%（Shopify 标准） |
| 支持场景 | Checkout 支付 |

### 7.2 Shopify Payments

- **状态**：未开通

---

## 八、税务配置

| 项目 | 状态 |
|------|------|
| Shopify Tax Services | ✅ 已启用（Active） |
| 支持国家 | 13个国家/地区自动税务计算 |

---

## 九、运营注意事项

### 9.1 已发现问题

| 问题 | 说明 | 状态 |
|------|------|------|
| 钉钉机器人 Token 失效 | 通知无法送达，Token 已失效超过26天 | ⚠️ 待修复 |
| Email 登录禁用 | Shopify 后台 Email 登录方式已禁用 | ⚠️ 需确认 |

### 9.2 日常维护检查清单

| 检查项 | 频率 | 工具 |
|--------|------|------|
| 网站是否可以正常访问 | 每天 | 浏览器直接访问 |
| 是否有新订单 | 每天 | Shopify 后台 |
| 流量数据变化 | 每周 | Google Analytics |
| 搜索排名变化 | 每周 | Google Search Console |
| SSL 证书状态 | 每周 | Cloudflare 仪表盘 |

---

## 十、相关账号汇总

| 平台 | 账号/邮箱 | 备注 |
|------|----------|------|
| Shopify | `ouunin2025@gmail.com` | 商店后台 |
| Cloudflare | （需向用户确认） | DNS + Pages |
| GitHub | `linoroven897-star` | 源码仓库 |
| Google Analytics | （需向用户确认） | 流量分析 |
| Google Search Console | （需向用户确认） | SEO |
| PayPal | （需向用户确认） | 收款 |

---

## 附录：常用链接

| 服务 | 链接 |
|------|------|
| 商店前台 | `https://ouunin.pages.dev` |
| Shopify 后台 | `https://admin.shopify.com` |
| Cloudflare Dashboard | `https://dash.cloudflare.com` |
| GitHub 仓库 | `https://github.com/linoroven897-star/ounin` |
| Google Analytics | `https://analytics.google.com` |
| Google Search Console | `https://search.google.com/search-console` |

---

*本文档由 AI 根据 2026-05-12 当前状态生成，如有任何变更请手动更新。*