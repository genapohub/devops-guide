# DevOps Guide — DevOps / 运维工程师方案产出指南

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)](SKILL.md)

一个面向 AI 编程助手的 **DevOps / 运维工程师 Skill**，将运维领域方法论转化为可执行工作流。自动识别 5 类场景（0→1 基础设施搭建 / 运维改造 / 故障修复 / 架构迁移 / 技术预研），按对应清单产出 CI/CD Pipeline、K8s配置、监控告警体系、灾备方案等完整交付物。

## 适用场景

| 场景 | 示例 | 产出量 |
|------|------|:---:|
| 0→1 基础设施搭建 | 新项目 CI/CD + K8s + 监控体系 | 10-12类 |
| 中型运维改造/优化 | 改造现有 Pipeline、新增监控 | 6-8类 |
| 小优化/故障修复 | Pipeline 修复、告警调整 | 2-3类 |
| 大版本架构迁移 | 云平台迁移、K8s 大版本升级 | 8-10类 |
| 技术预研/选型 | 新工具评估、成本优化方案 | 3-4类 |

## 触发热词

CI/CD、DevOps、Kubernetes、Docker、监控、告警、日志、云架构、容器化、Terraform、灾备、高可用、成本优化、运维方案

---

## 安装

本 Skill 遵循 **Open Agent Skills 标准**（SKILL.md 格式），兼容以下工具：

### WorkBuddy / CodeBuddy

**方式一：克隆到 skills 目录**
```bash
git clone https://github.com/genapohub/devops-guide.git ~/.workbuddy/skills/devops-guide
```

### Trae

**ZIP 导入**
```bash
git clone https://github.com/genapohub/devops-guide.git
zip -r devops-guide.zip devops-guide/
```
Trae → **设置** → **Rules & Skills** → **创建** → 上传 `devops-guide.zip`。

### Codex

```bash
# 克隆到 skills 目录
git clone https://github.com/genapohub/devops-guide.git ~/.codex/skills/devops-guide

# 或使用 cc switch (推荐)
git clone https://github.com/genapohub/devops-guide.git ~/.cc-switch/skills/devops-guide
```

若选 CC Switch 克隆后需在cc switch客户端-技能中心里导入技能，选中Codex等工具，重启Codex客户端后在对话中输入 $devops-guide 手动调用。

### Cursor
```bash
# 克隆到 skills 目录
git clone https://github.com/genapohub/devops-guide.git ~/.cursor/skills-cursor/devops-guide
```

重启 Cursor客户端 后自动发现。也可以在对话中输入 `$devops-guide` 手动调用。

---

## 使用

```
帮我搭建新项目的 CI/CD Pipeline
设计 Kubernetes 集群的部署方案
我们的服务需要完善的监控告警体系
从阿里云迁移到腾讯云，出方案
```
## 许可

[MIT](LICENSE) © zhangmengbo
