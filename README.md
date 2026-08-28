# Keresearch

三个互补的科研 skill，覆盖科研全生命周期：选题 → 实验 → 写作 → 投稿。

Three complementary skills covering the full research lifecycle: topic selection → experiments → writing → submission.

| Skill | 覆盖阶段 | 来源 |
|---|---|---|
| `research-project-playbook/` | 选题防御、阶段零微基准、实验纪律、发现日志、数字审计、导师沟通、投稿阶梯 | 蒸馏自一次完整科研周期（8×RTX4090 LLM serving 测量研究，70+ 配置） |
| `paper-writing/` | 句子级写作工艺、编辑原则、压缩模式、章节修辞 | [SNL-UCSB/paper-writing-skill](https://github.com/SNL-UCSB/paper-writing-skill)（保留原作者 LICENSE） |
| `research-paper-writing/` | 章节结构、段落流、审稿人视角、对抗性自查 | [Master-cai/research_paper_writing](https://github.com/Master-cai/research_paper_writing) |

## 三者关系 How they fit together

```
research-project-playbook   ← 项目级决策：做什么、测什么、怎么记、投哪里
        │
        ▼
research-paper-writing      ← 篇章级结构：章节职责、段落流、审稿人视角
        │
        ▼
paper-writing               ← 句子级工艺：语气、压缩、风格门禁（M1-M18 / S1-S31）
```

## 安装 Install

克隆到你的 skills 目录（ZCode / Claude Code 通用）：

```bash
git clone https://github.com/yunyancuo/keresearch.git
# ZCode
cp -r keresearch/*/ ~/.zcode/cli/skills/
# Claude Code
cp -r keresearch/*/ ~/.claude/skills/
```

## Playbook 速览（源项目验证过的硬规则）

1. **证据链选题**：不喊"没人做过"，列相邻论文各自盲区，你的工作=无人测量的合取。三条最接近论文+盲区说不清楚=没有空档。
2. **阶段零先行**：端到端实验前先给平台画像（分钟级微基准），后续一切主张锚定实测常数。
3. **单变量纪律**：每个配置点全新服务启动；说不清"只差一个变量"的对比不进论文。
4. **发现日志**：F1、F2…编号发现，每条 2-5 句+数字+原始数据指针。它就是论文的脊柱。
5. **数字审计**：脚本交叉核对正文每个数字与原始 JSON（曾抓到 3,296 vs 3,349 笔误）。
6. **三轮重复**：关键配置三次完整重启，报告 CV，头条差距写成 3σ 的倍数。
7. **符号审计**：渲染前专门过一遍公式，符号首现即定义、字母不兼职。
8. **导师卡**：一句话电梯版 / 两句问题方法版 / 三条一句话贡献 / 数字速查表，外行版把术语全部翻译。
9. **投稿阶梯**：冲刺+务实+保底+期刊，只投对测量研究友好的会。
10. **双语警告**：中文期刊+英文会议同工作=一稿多投风险。

## License

- `research-project-playbook/`: MIT
- `paper-writing/`: 见目录内原 LICENSE（SNL-UCSB）
- `research-paper-writing/`: 随上游仓库
