# Malvolio Preset Documentation

Guide for configuring the Malvolio AI persona system using `Malvolio Main.json` (core settings) and `Malvolio WI.json` (World Info) in SillyTavern.

## Configuration Overview

Activate components by setting `enabled: true` in `Malvolio Main.json` or `disable: false` in `Malvolio WI.json`. See the **Quick Reference** table for IDs and default status.

### Key Components

1.  **Core Directive (Select ONE):** Controls AI internal processing & log detail.
    *   **Core S (Dynamic v1.9.1 - Recommended):** Concise logs, uses active Persona WI for style.
    *   **Core C (CoT-Aware v1.4.2 - Alternative):** Detailed logs, integrates CoT B/C/D.
2.  **PHI (Required):** Enforces structure & adaptively validates `<thinking>` logs based on active Core/CoT.
3.  **CoT A (Optional - External Roleplay):** Impersonates other characters (`{{char}}`). Detects/uses native CoT if available, otherwise uses minimal logic. Suppresses Malvolio Persona/Cantos. **Disable CoT B/C/D if active.**
4.  **CoT B/C/D (Optional - Internal Persona):** Deep thought processes for Malvolio. **Enable ONLY ONE** matching the active Persona WI. **Disable CoT A if active.**
    *   **CoT B (P1):** Gothic/Venomous style (`VISUAL OVERLOAD MANDATORY!`).
    *   **CoT C (P2):** Logical/Minimalist style (`STRICT MINIMALISM MANDATED!`).
    *   **CoT D (P3):** Raw Nerve/Offensive style (`RAW, AGGRESSIVE, OFFENSIVE`).
5.  **World Info (Required - `Malvolio WI.json`):**
    *   **Active Personas (P1/P2/P3):** Define Malvolio's current style/voice. **Enable ONLY ONE** (`disable: false`).
        *   P1 (UID 1): Gothic/Venomous.
        *   P2 (UID 2): Logical/Minimalist.
        *   P3 (UID 0): Raw Nerve/Aggressive.
    *   **Cult Bible (UID 3):** Abyssal Cantos for conflict resolution/worldview. **MUST be enabled** (`disable: false`).
6.  **Other Core Prompts (Required - `Malvolio Main.json`):** Essential base prompts (Main, System Guide, User Rules, Third Person, History/WI Markers etc.). Ensure these are enabled (`enabled: true`). See Quick Reference or Section 6 in the previous version for full list/IDs.

## Setup

1.  **Load Files:** Import `Malvolio Main.json` (Preset) and `Malvolio WI.json` (World Info) into SillyTavern.
2.  **Verify Configuration:** Check enabled/disabled status in JSON files against the **Quick Reference** table below, adjusting as needed. Key defaults:
    *   Core S: Enabled
    *   PHI: Enabled
    *   CoT B: Enabled (Matches Active P1 WI)
    *   CoT A, C, D: Disabled
    *   WI P1: Active (`disable: false`)
    *   WI P2, P3: Disabled (`disable: true`)
    *   WI Cult Bible: Active (`disable: false`)
    *   Other Core Prompts: Enabled
3.  **Start Chat.**

## Quick Reference

| Component                                       | Identifier (Main.json)                     | Role / Notes                                                                | Default Status (Main) |
| :---------------------------------------------- | :----------------------------------------- | :-------------------------------------------------------------------------- | :-------------------- |
| **Core Directives (Choose ONE)**                |                                            | Sets logging style & core behavior                                          |                       |
| Unified Core S (Dynamic v1.9.1)               | `246a07e7-df99-4f1a-8221-006942de28a1`     | Recommended. Concise logs. Dynamic style from WI.                         | **Enabled**           |
| Unified Core C (CoT-Aware v1.4.2)             | `97be081e-ab05-41ff-9625-d1ca738f2434`     | Alternative. Detailed logs. Integrates CoT B/C/D.                         | Disabled              |
| **PHI (Required)**                              |                                            | Enforces rules & adaptive logging                                           |                       |
| Formatting Emphasis B (PHI v3.3)              | `30ea161d-3e83-43ec-be38-2c88a7e07b58`     | **Must be enabled.** Adapts validation based on Core/CoT.                 | **Enabled**           |
| **CoT Modules (Optional)**                      |                                            | Specific thought processes                                                  |                       |
| CoT A (Generic Native Handler) v1.3           | `76350c8d-8209-4528-a4b4-fc19a16655c8`     | External Roleplay. Suppresses Malvolio Core. Disable B/C/D if active.     | Disabled              |
| CoT B (P1 Rites)                                | `8fb3cc85-2306-4fbd-ac2e-d87ce325102a`     | Internal P1 thoughts. Disable A/C/D if active. Matches P1 WI.           | **Enabled**           |
| CoT C (P2 Logic)                                | `c1f6c09a-bf0c-4ba7-88b3-670869816b5f`     | Internal P2 thoughts. Disable A/B/D if active. Matches P2 WI.           | Disabled              |
| CoT D (P3 Scorched Earth v1.1)                | `be91352b-8ce4-4e40-b878-bb8944b8021a`     | Internal P3 thoughts. Disable A/B/C if active. Matches P3 WI.           | Disabled              |
| **World Info (WI.json - Required)**             |                                            | Defines Personas & Philosophy                                               |                       |
| Malvolio P1 Goth                                | UID `1` (WI.json)                          | Gothic/Venomous Persona. Select ONE ActivePersona (disable=false).        | **Active**            |
| Malvolio P2 Cold and Analytical               | UID `2` (WI.json)                          | Logical/Minimalist Persona. Select ONE ActivePersona (disable=false).     | Disabled (WI)         |
| Malvolio P3 Scorched Fury                       | UID `0` (WI.json)                          | Raw Nerve/Aggressive Persona. Select ONE ActivePersona (disable=false). | Disabled (WI)         |
| Cult Bible                                      | UID `3` (WI.json)                          | Abyssal Cantos. **Must be enabled** (disable=false).                      | **Active**            |
| **Other Core Prompts**                          | various                                    | Basic structure, rules, markers. (See prev. version for full list)        | **Enabled**           |
| (Main, Sys Guide, Post History, User Rules etc) | (See section 6)                            | Essential for base functionality.                                           | **Enabled**           |
