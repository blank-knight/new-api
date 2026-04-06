# 技术栈文档 — API 中转站

## 核心技术栈

New API 是一个**开箱即用的开源项目**，技术栈已经确定，不需要从零选型。

### 后端
| 技术 | 版本 | 用途 |
|---|---|---|
| Go | 1.22+ | 后端语言，高性能 |
| Gin | latest | HTTP 框架 |
| GORM | v2 | ORM，支持 SQLite/MySQL/PostgreSQL |

### 前端
| 技术 | 版本 | 用途 |
|---|---|---|
| React | 18 | 前端框架 |
| Vite | latest | 构建工具 |
| Semi Design | @douyinfe/semi-ui | UI 组件库（字节跳动出品） |
| Bun | latest | 包管理器（替代 npm/yarn） |

### 数据库
| 技术 | 用途 | 生产推荐 |
|---|---|---|
| PostgreSQL 15 | 主数据库 | ✅ 推荐（生产环境） |
| Redis | 缓存 + 限流 + 会话 | ✅ 必须 |
| SQLite | 开发/测试 | ❌ 不推荐生产 |

### 基础设施
| 技术 | 用途 |
|---|---|
| Docker | 容器化部署 |
| Docker Compose | 编排多容器 |
| Nginx / Caddy | 反向代理 + SSL |
| Cloudflare | CDN + DDoS 防护 + DNS |

### 上游供应商对接
New API 已内置 40+ 供应商适配器（`relay/channel/` 目录）：

```
openai/      — OpenAI GPT 系列
claude/      — Anthropic Claude 系列
gemini/      — Google Gemini 系列
deepseek/    — DeepSeek
ali/         — 通义千问
zhipu/       — 智谱 GLM
volcengine/  — 火山引擎（豆包）
moonshot/    — 月之暗面 Kimi
aws/         — AWS Bedrock
vertex/      — Google Vertex AI
...（共 40+）
```

## 部署方案

### 方案 A：单机 Docker 部署（推荐起步）
```yaml
services:
  new-api:     # 主服务
  postgres:    # 数据库
  redis:       # 缓存
```
- 一台云服务器搞定
- 适合 0-500 用户
- 成本：¥50-200/月

### 方案 B：分离部署（规模增长后）
- API 服务 → 单独服务器（可多实例）
- 数据库 → 云数据库（RDS）
- Redis → 云 Redis
- 适合 500+ 用户

### 方案 C：Kubernetes（大规模）
- 自动扩缩容
- 适合 5000+ 用户
- 初期不需要

## 开发工具
- Git + GitHub — 版本管理
- Docker — 本地开发环境
- Go IDE（GoLand / VS Code + Go 插件）
- React DevTools — 前端调试

## 监控工具
- New API 内置看板 — 用量/费用/错误率
- Prometheus + Grafana（可选）— 高级监控
- 健康检查端点：`/api/status`
