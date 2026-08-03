---
name: devops-guide
description: DevOps/运维工程师方案产出指南。当用户提出运维、CI/CD、容器化、云基础设施、监控告警相关需求时触发。覆盖场景：基础设施搭建、CI/CD流程设计与优化、容器化编排、云架构设计、监控日志体系搭建、灾备方案、IaC、成本优化。先识别需求属于5类场景中的哪一类（0到1基础设施搭建/中型运维改造/小优化故障修复/大版本架构迁移/技术预研选型），再按对应场景的产出清单生成完整方案。触发词：CI/CD、DevOps、Kubernetes、Docker、监控、告警、日志、云架构、容器化、Terraform、灾备、高可用、成本优化、运维方案。
version: 1.0.0
tags:
  - devops
  - cicd
  - kubernetes
  - docker
  - monitoring
  - cloud
  - infrastructure
  - DevOps
  - 运维
---

# DevOps / 运维工程师方案产出指南

## Overview

本技能将 DevOps 领域的方法论转化为可执行的工作流。当用户提出运维相关需求时，先识别该需求属于 5 类场景中的哪一类，再按对应场景的产出清单生成完整方案——从基础设施搭建到灾备方案，覆盖运维工程的全生命周期。

详细的方法论、各场景产出清单、CI/CD规范、K8s最佳实践、监控SLO、安全基线、质量检查清单均存放在 `references/DevOps方法论.md`，在执行前必须读取对应章节。

## 触发条件

- 用户需要搭建 CI/CD Pipeline 或改造现有流程
- 用户需要设计容器化方案或 Kubernetes 配置
- 用户需要搭建监控、日志、告警体系
- 用户需要云基础设施规划或迁移
- 用户提到"CI/CD""DevOps""K8s""Docker""监控告警""日志""灾备""IaC""Terraform""运维"等关键词

## 记忆系统

本技能的完整记忆管理规则（写日志/轮转归档/自清理）定义在 `references/记忆规则.md`，执行前必须读取。

- **执行前**：读取 `references/记忆规则.md` 中的 Step 0 加载规范 + `.workbuddy/memory/MEMORY.md` 本技能对应分段 + `.workbuddy/memory/YYYY-MM-DD.md`（今日日志，如存在）
- **执行后**：追加 `[devops-guide] 场景描述 → 关键决策` 到 `.workbuddy/memory/YYYY-MM-DD.md`；如有可复用决策，去重后追加到 MEMORY.md 对应分段
- **轮转检查**：
  - **独立使用**：按 `references/记忆规则.md` 中的触发条件和完整轮转算法执行归档
  - **被 team-orchestrator 调度时**：跳过轮转检查，由调度官 Step 7 统一执行全局轮转

## 执行流程

按以下 5 步顺序执行，不可跳步。

### Step 1: 需求理解

- 解析用户输入的运维需求
- 提取关键信息：基础设施现状、云平台、容器化程度、监控现状、团队规模、可用性目标、预算约束
- 识别缺失的关键信息，主动提问补全（一次最多 2-3 个问题）

### Step 2: 场景识别

读取 `references/DevOps方法论.md` 的"一、场景识别"章节判断场景：

| 场景 | 名称 | 判断条件 | 产出量 |
|------|------|---------|--------|
| 场景一 | 0→1 基础设施搭建 | 全新项目、无CI/CD、无监控 | 10-12类 |
| 场景二 | 中型运维改造/优化 | 已有基础设施新增模块、CI/CD改造 | 6-8类 |
| 场景三 | 小优化/故障修复 | 单条Pipeline修复、告警调整 | 2-3类 |
| 场景四 | 大版本架构迁移 | 云平台迁移、K8s升级、工具链替换 | 8-10类 |
| 场景五 | 技术预研/选型 | 新工具评估、成本优化PoC | 3-4类 |

### Step 3: 与用户确认场景

输出场景判断、判断依据、产出清单、预估周期，确认后进入产出。

### Step 4: 按清单产出方案

读取 `references/DevOps方法论.md` 对应场景章节。

产出要求：
- 架构图使用 Mermaid 或 ASCII 描述（网络拓扑/部署拓扑/数据流）
- Pipeline 定义给出完整的 YAML 配置示例（GitHub Actions/GitLab CI）
- K8s 配置给出完整的 YAML manifest（Deployment/Service/HPA/PDB）
- 监控告警给出具体指标 + 阈值 + 通知方式
- 遵循"七、DevOps 通用规范"
- 产出后保存为 Markdown 文件

### Step 5: 质量检查

读取 `references/DevOps方法论.md` 的"八、产出质量检查清单"：

- CI/CD Pipeline 覆盖 Build→Test→Scan→Deploy 全流程
- 容器化符合最佳实践（多阶段构建/非root/资源限制）
- 监控覆盖 Metrics + Logs + Traces
- 告警分级明确 + 通知渠道
- 灾备方案含 RPO/RTO + 演练计划
- 安全覆盖镜像/网络/密钥/审计
- 回滚方案可执行

## 资源说明

### references/DevOps方法论.md

完整的方法论文档，包含：5个场景产出清单、CI/CD规范、Docker最佳实践、K8s资源规范、监控SLO参考、安全基线、质量检查清单。

## 注意事项

- 不要跳过 Step 3 的用户确认
- Pipeline 必须覆盖安全性检查（镜像扫描/依赖扫描）
- 容器必须配置资源限制（Request/Limit），不加限制是安全隐患
- 告警必须分级 + 指定通知渠道 + 定义升级策略
- 灾备方案不能停在纸面，必须含演练计划
- 场景四（架构迁移）的回滚预案是硬性要求
