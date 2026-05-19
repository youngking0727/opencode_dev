<p align="center">
  <img src="docs/logo.svg" alt="OpenBioMedAgent" width="320">
</p>

<p align="center">
  <strong>OpenBioMedAgent</strong> — 生物医药领域的 AI Agent
</p>

<p align="center">
  <em>面向生物医药领域的 AI Agent · Fork 自 <a href="https://github.com/anomalyco/opencode">OpenCode</a> · MIT 协议</em>
</p>

<p align="center">
  <a href="README.md">English</a> |
  <a href="README.zh.md">简体中文</a>
</p>

---

## 项目状态

**早期开发中**。当前阶段:Web Demo,目标 2 周内交付。

## 这是什么

OpenBioMedAgent 是一个面向生物医药领域的任务型 AI Agent,基于 [OpenCode](https://github.com/anomalyco/opencode) fork 改造,
加入领域专属工具(PubMed、RDKit、ClinicalTrials.gov 等)和生物医药专家 prompt,
用于辅助药企、Biotech、CRO、医院科研机构的研究员、计算化学家、生信工程师、医学经理等日常研发工作。

## 本地启动(开发者)

```bash
git clone git@github.com:youngking0727/opencode_dev.git
cd opencode_dev
bun install

# 启动 Web 模式
bun run --cwd packages/opencode --conditions=browser src/index.ts web --port 4196

# 浏览器访问 http://localhost:4196
```

## Agent 模式

继承自 OpenCode,通过 `Tab` 键切换:

- **build** — 全权限 agent,用于实际开发工作
- **plan** — 只读 agent,用于分析和代码探索,默认拒绝文件编辑

## 文档

- [Fork 维护规范](./MAINTAINING.md)
- 更多文档随项目推进补充

## 致谢

本项目基于 [anomalyco/opencode](https://github.com/anomalyco/opencode) 改造(MIT 协议)。
感谢 OpenCode 团队的开源工作。

## 协议

MIT — 见 [LICENSE](./LICENSE)
