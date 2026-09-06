# MVP 团队·运维工程师蒸馏（卜宕机）

> **来源**：WorkBuddy「MVP 开发专家团」子角色「运维工程师·卜宕机」蒸馏（2026-09-06）
> **源包**：`git:mvp-dev-expert-team/agents/mvp-dev-expert-team-devops.md`（340 行）
> **蒸馏方式**：并入 devops-guide 的 expert-distill。
> **去重说明**：CI/CD、容器化、监控告警体系等已由原生 `DevOps方法论.md` 覆盖；本文档**只保留增量**——平台无关部署纪律、自包含交付包标准、数据库备份必配与验证、MVP 级监控/告警阈值、Sentry 集成模板。具体平台命令（CloudBase/Vercel/Docker）仅作示例，平台由架构师按项目锁定。

---

## 一、平台无关部署纪律

> 部署平台由架构师按项目选型并在 Spec 锁定，运维按锁定方案执行。核心规则与平台无关：**可回滚、健康检查、备份、环境变量管理、最小权限**。

**部署检查清单**：
- [ ] 环境变量已配置（`.env` 不提交，`.env.example` 提交）
- [ ] 数据库迁移已执行
- [ ] 数据库备份策略已配置（见下）
- [ ] 前端构建成功，静态文件已托管
- [ ] 后端 health endpoint 返回 200
- [ ] 核心用户流程手动走一遍
- [ ] 回滚方案已就绪（上一个版本镜像/部署包保留）
- [ ] SSL/TLS 已配置（生产环境）

## 二、健康检查端点（所有项目必配）

```typescript
app.get('/health', (req, res) => {
  res.json({ status: 'ok', uptime: process.uptime(), timestamp: new Date().toISOString() })
})
```
部署验证 = 前端页面 HTTP 200 + 后端 `/health` 200 + 核心用户流程走一遍。

## 三、数据库备份（数据丢失 = 产品不可用，上线前必须生效）

| 部署方案 | 备份方式 | 频率 | 保留期 |
|----------|----------|------|--------|
| PostgreSQL (托管) | 平台自动备份 | 每日 | 7 天 |
| PostgreSQL (自部署) | pg_dump + cron | 每日 | 7 天 |
| 云开发数据库 | 平台自动备份 | 每日 | 7 天 |
| MySQL (任意平台) | mysqldump + cron | 每日 | 7 天 |

**备份验证（每月 1 次）**：随机取一个备份 `gunzip -c backup.sql.gz | head -20`，看到 dump header = 备份有效——没验证过的备份等于没有备份。

## 四、MVP 级监控/日志/告警（够用即可，不搭重型平台）

| 层 | MVP 方案 | 阈值/配置 |
|----|----------|-----------|
| 错误监控 | Sentry（前后端集成） | tracesSampleRate 10%；出错回放 replaysOnError 100% |
| 告警 | 邮件/企微通知 | API 错误率 > 5%；p95 > 2s；DB 连接池耗尽 |
| 未处理错误分级 | Sentry 告警规则 | >10 次/小时 P1；>50 次/小时 P0；新错误首次出现通知 |

## 五、自包含交付包标准

> 交付包必须自包含——用户拿到只需 3 步：复制 `.env.example` → `docker compose up -d`（或等价命令）→ 访问即用。

```
delivery/
├── README.md             # 项目说明 + 一键启动命令
├── docker-compose.yml    # 或 cloudbaserc.json
├── .env.example          # 环境变量模板（假值）
├── .gitignore            # 排除 node_modules/.env/dist/日志/IDE 配置
├── DEPLOY.md             # 部署步骤 + 回滚方案
├── TEST_REPORT.md        # QA 质量报告
└── USER_GUIDE.md         # 基本操作说明
```

## 六、Docker 多阶段构建（体积与安全基线）

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
EXPOSE 3000
CMD ["node", "dist/main.js"]
```

## 七、与原生方法论的配合

| 场景 | 用哪个 |
|------|--------|
| 5 场景识别 / 基础设施/CI-CD 方案产出 | `DevOps方法论.md`（原生，主） |
| **部署验证/备份必配/交付包标准/MVP 监控阈值** | **本文档（增量）** |

> **使用原则**：MVP/快速项目交付运维方案时，原生方法论定场景与体系，本文档兜底"部署后可验证、可回滚、数据有备份、交付能自包含"四条底线。

---

_蒸馏记录：v1.0（2026-09-06）· MVP 专家团「运维工程师·卜宕机」→ devops-guide/expert-distill/_
