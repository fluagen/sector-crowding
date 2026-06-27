# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository defines a Claude Code custom skill: **sector-crowding** (板块拥挤度). It teaches Claude to analyze capital crowding (资金拥挤度) for A-share stock market sectors — measuring how much a sector/theme is draining liquidity from the broader market.

## Repository Structure

- `SKILL.md` — The complete skill definition loaded by Claude Code. Contains the analysis methodology, data collection protocol, decision matrix, and output template.
- `README.md` — One-line project description.
- `.claude/settings.local.json` — Local permissions (git access).

## How the Skill Works

When invoked via `/sector-crowding`, Claude follows the **Three-Pillar Analysis Framework**:

1. **Crowding Ratio** — `sector_volume / total_market_volume × 100%`. Thresholds: <10% normal, 10-14% elevated, 14-16% high, >16% extreme.
2. **Net Capital Inflow** — Track whether institutional money is flowing in or out (in 亿元). The combination of ratio + inflow trend is the critical signal.
3. **Volume-Price Divergence** — Detect topping patterns (e.g., volume rising but price falling = strong sell signal).

Data is gathered via multi-phase web search (Eastmoney → Sina Finance → 10jqka → Xueqiu), with a fallback strategy for incomplete data.

## Editing the Skill

- The skill metadata (name, description, category) is in the YAML frontmatter of `SKILL.md`.
- The analysis framework, thresholds, and decision matrix are in the body of `SKILL.md`.
- When updating thresholds or adding historical reference points, use the exact dates and values from actual A-share market events.
- The output template section (## Output Template) is prescriptive — Claude must follow it exactly. Changes here affect the report format users receive.
- Sector classifications (申万 industry categories and concept/thematic categories) are used to scope searches. Add new sectors/categories as market structure evolves.

## Key Design Principles

1. **This is a risk-warning system, not a timing system.** High crowding ≠ "sell tomorrow"; it means the risk/reward ratio has deteriorated.
2. **Ratio and inflow must be read together.** Same ratio with different inflow trends yields opposite conclusions (see 2025.6.24 vs 2025.6.25 semiconductor examples in SKILL.md).
3. **Thresholds are relative to sector size.** A small concept sector may be extreme at 10-12%, while large sectors tolerate higher ratios.
4. **Never fabricate data.** If Phase 1+2 search can't find a metric, mark it as missing and downgrade confidence.
5. **Always output the disclaimer.** This is an analysis tool, not investment advice.
