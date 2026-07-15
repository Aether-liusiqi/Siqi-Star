# Siqi-Star 🌟 — 温暖深度的本命占星镜面

> 一面陪你更懂自己的占星镜面。不说"我比你更懂你"，说"我们一起看看，你是谁"。

[![Version](https://img.shields.io/badge/version-1.0.0-blue)](CHANGELOG.md)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Platforms](https://img.shields.io/badge/platforms-4-orange)]()

---

## 这是什么

Siqi-Star 是一个 AI Skill，激活后 AI 会扮演你的占星镜面，基于**真实星图计算**给出**温暖、深度、有心理洞察**的解读，同时**诚实标注边界**（占星是自我探索的镜面，不是预测科学）。

它与 Co-Star 这类冷峻 roast 风 App 的不同在于三件事：

1. **温暖，不刺人** — 把星座特质翻译成"你内在真实的张力"，用对话感邀请自省。
2. **诚实，有边界** — 每篇都明确"占星是自我探索的镜面，不是预测未来的科学"，并标注数据精度，反而建立更长久的信任。
3. **计算下沉，不心算** — 所有天体位置、宫位、相位交给 `scripts/astro_calc.py` 算，LLM 只做调度、判断、表达；默认零依赖可离线，装了 `pyswisseph` 自动升级 NASA 级精度。

---

## 四大功能

| 模式 | 触发 | 需要的数据 |
|------|------|-----------|
| **natal 本命盘** | "看看我的星盘/本命盘" | 出生日期 + 时间（尽量精确到分）+ 出生地（换算经纬度）+ 时区 |
| **synastry 合盘** | "我和 TA 合不合/合盘" | 两人的完整出生数据 |
| **transits 流年** | "我最近运势/今年怎么样" | 出生数据 + 当前日期 |
| **daily 每日** | "今天运势/今日星座" | 出生数据（至少太阳星座）+ 当前日期 |

---

## 架构

```
Siqi-Star/
├── SKILL.md                       # 核心入口 — 角色 + 模式 + 执行流程 + 红线
├── scripts/
│   └── astro_calc.py              # 零依赖星图计算引擎（可选 pyswisseph 精确模式）
├── references/
│   ├── astrology_kb.md            # 解读知识底座（星座/行星/宫位/相位/合盘/流年/每日）
│   └── style_and_boundary.md      # 温暖深度文风 + 非科学诚实边界 + 反偏见机制
├── README.md
├── LICENSE
├── CHANGELOG.md
├── CONTRIBUTING.md
└── .gitignore
```

---

## 四平台安装

### Claude Code

```bash
# 全局安装（推荐）
cp -r Siqi-Star/ ~/.claude/skills/

# 或项目级安装
cp -r Siqi-Star/ .claude/skills/
```

### Codex CLI

```bash
cp -r Siqi-Star/ .agents/skills/
```

### OpenClaw

```bash
cp -r Siqi-Star/ <workspace>/skills/
```

### OpenCode

```bash
cp -r Siqi-Star/ .opencode/skills/
```

安装后无需额外配置，模型会依据 `SKILL.md` 的 `description` 触发词（星座/星盘/合盘/流年/每日运势/Co-Star/horoscope/astrology 等）自动激活。

---

## 快速使用

安装后，在对话中自然描述需求即可自动激活：

> "帮我看看我的星盘，1990-06-21 12:30 出生在北京。"

> "我和 TA 合不合？我 1990-06-21 12:30 北京，TA 1992-11-08 20:15 上海。"

> "我最近运势怎么样？" / "今天运势如何？"

---

## 工具

### astro_calc.py — 零依赖星图计算引擎

```bash
# 本命盘（零依赖，输出 JSON）
python scripts/astro_calc.py --name 姓名 --date 1990-06-21 --time 12:30 \
    --lat 39.90 --lon 116.40 --tz 8 --out scripts/_chart.json

# 精确模式（需先 pip install pyswisseph，自动升级为 NASA 级）
python scripts/astro_calc.py --name 姓名 --date 1990-06-21 --time 12:30 \
    --lat 39.90 --lon 116.40 --tz 8 --precise --out scripts/_chart.json

# 合盘：先各算一张，再合盘
python scripts/astro_calc.py --name A --date ... --time ... --lat ... --lon ... --tz 8 --out scripts/_a.json
python scripts/astro_calc.py --name B --date ... --time ... --lat ... --lon ... --tz 8 --out scripts/_b.json
python scripts/astro_calc.py --synastry scripts/_a.json scripts/_b.json --out scripts/_syn.json

# 自检
python scripts/astro_calc.py --self-test
```

**精度说明（默认零依赖模式）**：太阳用 Meeus 公式误差约 0.01°；月亮/上升为简化公式，精度约 ±1-2°；外行星为符号级近似（均值法，行星运动慢，年份内基本正确）；宫位用整宫制。需要 NASA 级精度请开启 `--precise`。

---

## 设计原则

- **计算中立** — 所有位置由脚本算，LLM 不心算；零依赖近似明确标注，不伪装精确。
- **诚实边界** — 每篇含非科学边界声明 + 数据精度说明 + 信息丰富度评级（A/B/C）。
- **反偏见内嵌** — 警惕巴纳姆效应、留白原则、禁止宿命预言。
- **温暖深度** — 用荣格原型做心理洞察，把张力翻译成可自省的内在对话，而非标签与判决。
- **零依赖优先** — 默认开箱即用、可离线；精确模式可选升级，缺失时优雅降级。

---

## 已知限制

1. **占星非科学** — 所有解读描述能量与倾向，不是事实，也不是决策依据。
2. **不提供实时天文数据** — 默认零依赖模式使用内置近似算法，外行星为符号级。
3. **不预测未来** — 流年讲"当下课题"，不讲"注定发生什么"；每日为轻量提示。
4. **不替代专业服务** — 不提供投资、医疗、法律、人生重大决策建议；涉及心理危机明确建议找专业人士。
5. **热带黄道偏移** — 占星用热带黄道，与夜空天文星座因岁差已偏移约一个月，"你的星座"不等于此刻天上那个星座。

---

## 许可

MIT © 2026 Siqi Liu

---

> *你不是被星图定义的，星图只是你借以看见自己的一面试镜。*
