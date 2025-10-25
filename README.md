# PRFB_FX (5E)

**Proficiency Bonus via Effects — precise, ruleset-friendly control for Fantasy Grounds Unity (D&D 5E).**  
PRFB_FX lets you **raise or lower the effective Proficiency Bonus (PB)** of a creature **via Effects**, either **only during rolls** or **globally** across the sheet (depending on mode). It can optionally **mirror** the effective PB to the character sheet and **adjust DCs** where appropriate.

> **Ruleset:** D&D 5E (Fantasy Grounds Unity)  
> **Scope:** PB computation & (optional) DC deltas; non-invasive to other math  
> **Author:** Yawgmath  
> **License:** MIT

---

## Why PRFB_FX?

- **Granular control:** Adjust PB with effects (buffs, curses, aura modifiers) without rewriting core rules.
- **Roll-time or global:** Choose whether the effective PB should apply only in the roll pipeline or also mirror onto the sheet.
- **Deterministic & transparent:** Clear precedence and simple formulas; optional UI mirroring for easy verification.

---

## Core Idea 

Two effect keywords on the **actor** (PC/NPC):

- `PRFSET: n`  
  *Hard set / cap.* If multiple are present, the **smallest** `n` wins.
- `PRFADJ: n`  
  *Adjustment.* All `PRFADJ` values **sum**.


**Intuition:**  
- `PRFSET` can **lower** PB (e.g., exhaustion-like effects, anti-proficiency fields) but won’t exceed the base from level.  
- `PRFADJ` can **raise or lower** around that result (stacking).

---

## Features

- **Roll-time PB override**  
  Applies `effPB` inside the 5E roll pipeline (skills, attacks, saves, DCs, etc.), without permanently altering core data.
- **Global mirror (optional)**  
  Show `effPB` on the sheet (visual parity with what the engine rolls).  
- **DC integration (optional, conservative)**  
  If a DC originates from the standard base form **“8 + Ability”** (i.e., no proficiency inherently baked into that base), PRFB_FX applies the **PB delta** `ΔPB = (effPB - corePB)` to the resulting DC.  
- **PCs only**  
  Limit the system to PCs only.

---

## Installation

**Forge (recommended):** Subscribe on the Forge and sync in Fantasy Grounds Unity.  
**Manual:** Copy the `.ext` file into your `Fantasy Grounds/extensions` folder and enable **PRFB_FX (5E)** when launching your campaign.

---

## Quick Start

1. **Enable** **PRFB_FX (5E)** in your 5E campaign.
2. (Optional) Open **Options** (gear icon) and set mode/toggles (see *Modes & Options*).
3. Add effects to an actor in the **Combat Tracker** or **Action/Effects** views, for example:
   - `PRFADJ: 2`  → increase PB by 2  
   - `PRFADJ: -1` → decrease PB by 1  
   - `PRFSET: 2`  → cap PB at 2 (or lower if multiple PRFSETs)

Play as usual; PRFB_FX will apply `effPB` to rolls, and (if enabled) mirror & DC deltas.

---



## How it works in FGU (flow)

### Roll Pipeline (roll-only & global)
1. Actor has effects (`PRFSET`, `PRFADJ`) active.  
2. PRFB_FX computes `effPB` from `corePB`, `PRFSET` (min) and `PRFADJ` (sum).  
3. When the 5E ruleset requests proficiency for a roll, PRFB_FX supplies `effPB`.  
4. The roll proceeds normally with all other modifiers intact.

### Global Mirror (global mode)
1. On actor load/update, PRFB_FX **writes/mirrors** `effPB` to the sheet’s PB display.  
2. Any UI element that shows PB reflects the current effective value (for transparency).

### DC Integration (optional, conservative)
1. A power/action is about to **request a DC**.  
2. If the action’s **Base** is exactly **“8 + Ability”** (no embedded proficiency), PRFB_FX computes `ΔPB = effPB − corePB`.  
3. PRFB_FX **adds `ΔPB`** to the resulting DC.  
4. The save proceeds; advantage/disadvantage and other ruleset logic remain unchanged.

---

## Compatibility

- **Designed for:** Fantasy Grounds **Unity**, **D&D 5E** ruleset.  
- **Interops:** Works alongside most automation/effects tools. Extensions that hard-override proficiency or rewrite DC logic may overlap; test combos as needed.  
- **Non-goals:** Does **not** alter advantage/disadvantage logic, damage dice math, or rewrite your action editors beyond the limited DC delta hook.

---

## License

Released under the **MIT License**. See `LICENSE` in the package.
