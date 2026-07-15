# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] — 2026-07-15

### Added
- 初始发布：温暖深度心理洞察向的本命占星镜面 skill
- `SKILL.md` 核心入口（设计理念 / 四模式输入 / 执行流程 / 输出模板 / 红线 / 局限）
- 四大功能：本命盘 (natal) / 合盘 (synastry) / 流年 (transits) / 每日 (daily)
- `scripts/astro_calc.py` — 零依赖星图计算引擎
  - 太阳用 Meeus 高精度公式，月亮/上升用简化公式，外行星用均值近似
  - 可选 `pyswisseph` 精确模式（自动升级 NASA 级、Placidus 宫位），缺失时优雅降级
  - 支持本命盘 / 合盘 / 自检，输出 UTF-8 JSON 无 BOM
- `references/astrology_kb.md` — 解读知识底座（12 星座 / 10 行星 / 12 宫 / 5 相位 / 元素 / 合盘 / 流年 / 每日）
- `references/style_and_boundary.md` — 温暖深度文风 + 非科学诚实边界 + 反偏见机制
- 四平台兼容：Claude Code / Codex CLI / OpenClaw / OpenCode
