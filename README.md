# Malvolio Preset Documentation (International Edition)

This guide explains how to configure and use the Malvolio AI persona system, based on `Malvolio Main.json` (core settings) and `Malvolio WI.json` (World Info definitions). This preset is designed for use with SillyTavern.

## Key Components

The system is built around several key components found within the `prompts` array in `Malvolio Main.json` and the `entries` object in `Malvolio WI.json`. These can be selectively activated by changing the `enabled` field in `Malvolio Main.json` or the `disable` field in `Malvolio WI.json`.

1. **Core Directives (Select One):** Determine the AI's internal processing and logging strategy. Choose one to enable in `Malvolio Main.json` (`enabled: true`).

    * **`🖤☑️ Unified Core S (Robust Dynamic Core)` (ID: `246a07e7-df99-4f1a-8221-006942de28a1`) – Recommended**
      * Corresponds to Dynamic Persona Core v1.9.1.
      * Optimized for performance and concise logs.
      * Generates minimal `<thinking>` entries (OP02 a/b/c) by default.
      * Dynamically uses the active Persona WI entry for styling, tone, and behavior within `<Content>`.
      * Logs the full Abyssal Translation sequence only if triggered by internal conflict.
    * **`🖤☑️ Unified Core C (CoT-Aware Enhanced Logging)` (ID: `97be081e-ab05-41ff-9625-d1ca738f2434`) – Alternative**
      * Corresponds to CoT-Aware Core v1.4.2.
      * Emphasizes detailed internal planning logs.
      * Always records comprehensive `<thinking>` steps (OP02 a/b/c + OP03c d/e) even without external CoT modules.
      * Integrates external CoT modules (B/C/D) when active, replacing internal planning steps (d/e).
      * Ideal for debugging or deep analysis, potentially at the cost of performance.

2. **PHI (Prompt Hierarchy Interface – Required):** Enforces structure and adaptively validates logs.

    * **`☑️ Formatting Emphasis B (PHI v3.3)` (ID: `30ea161d-3e83-43ec-be38-2c88a7e07b58`)**
      * Must be enabled (`enabled: true`) in `Malvolio Main.json`.
      * Enforces structural rules (e.g., responses start with `<thinking>`) and logging requirements.
      * Adapts validation of `<thinking>` entries based on the active Core Directive and activated CoT module (A/B/C/D or none).
      * Handles native CoT detection logging when CoT A is active.
      * Guarantees proper handling of conflict scenarios via "Abyssal Translation" logging mandate.

3. **CoT A (Optional – External Roleplay Handler):** Handles impersonation of other characters.

    * **`🖤☑️ CoT A (Generic Native Handler)` (ID: `76350c8d-8209-4528-a4b4-fc19a16655c8`)**
      * Enable (`enabled: true`) in `Malvolio Main.json` when the AI needs to impersonate *another* character (`{{char}}`) instead of acting as Malvolio.
      * **Detects and prioritizes executing native CoT formats** defined within the target character's card/WI. Logs native steps if found.
      * If no native CoT is found, it uses its own minimal external logic (Steps 4-7) and logs accordingly.
      * Suppresses the Malvolio core persona (`ActivePersona` WI) and Cantos (`Cult Bible` WI) while active. Log confirms suppression.
      * **Disable CoT B/C/D when CoT A is enabled.**

4. **CoT B/C/D (Optional – Internal Persona Thought Chains):** Provide deep thought processes for Malvolio acting as herself. **Only one should be active (`enabled: true`) at a time, and NOT simultaneously with CoT A.**

    * **`🖤☑️ CoT B (P1 Abyssal Execution Rites)` (ID: `8fb3cc85-2306-4fbd-ac2e-d87ce325102a`)**: For Persona P1 (Gothic/Venomous). Focuses on aesthetic execution (`VISUAL OVERLOAD MANDATORY!`), visceral language, and P1's specific rituals detailed in the CoT.
    * **`🖤☑️ CoT C (P2 Logical System Resolution)` (ID: `c1f6c09a-bf0c-4ba7-88b3-670869816b5f`)**: For Persona P2 (Calm/Logical). Focuses on precise analysis, logical deduction, minimalist style (`STRICT MINIMALISM MANDATED!`), and computational metaphors.
    * **`🖤☑️ CoT D (P3 Scorched Earth Protocol)` (ID: `be91352b-8ce4-4e40-b878-bb8944b8021a`)**: For Persona P3 (Raw Nerve). Focuses on maximum offense, raw language, contempt, stereotype abuse, and bypassing politeness filters (`RAW, AGGRESSIVE, OFFENSIVE, PC=BULLSHIT`).

5. **World Info (WI – Required):** Defined in `Malvolio WI.json`. Controls persona style and core philosophy.

    * **Active Personas (P1, P2, P3):** Located under `entries`. These define Malvolio's current personality, style, and voice.
      * **P1:** `Malvolio P1 Goth` (UID: `1`) - Venomous, gothic, visually rich aesthetic.
      * **P2:** `Malvolio P2 Cold and Analytical` (UID: `2`) - Logical, precise, strict minimalist aesthetic.
      * **P3:** `Malvolio P3 Scorched Fury` (UID: `0`) - Raw Nerve, aggressive, contemptuous, profane.
      * **Enable only ONE persona** by setting its `disable` field to `false` and others to `true`. The active persona's details are used by the Core Directives and CoT B/C/D modules.
    * **Cult Bible (Abyssal Cantos):** `Cult Bible` (UID: `3`) - Provides philosophical verses (Cantos I-XII) used for conflict resolution ("Abyssal Translation") and worldview shaping. **This entry MUST always be enabled (`disable: false`)** alongside the selected persona.

6. **Other Core Prompts (Required):** Found in `Malvolio Main.json`, ensure these are enabled (`enabled: true`).
    * `☑️ Main Prompt` (ID: `main`): Basic instruction to write the reply.
    * `📝 System Guide` (ID: `custom_module`): Provides an overview of the architecture (useful for context but not directly executed).
    * `🎐Post History Instructions Header` (ID: `fbeba1d3-e99c-4188-a78a-dd9d6baca78a`): Header for post-history rules.
    * `☑️ Prevent Simulating {{user}}` (ID: `a4758e8f-ebcb-4283-98c1-e4da1376b453`): Rules to prevent the AI from writing for the user.
    * `🖤☑️ Omniscient Third Person` (ID: `0b6458a9-0629-43a9-8c88-016c1cc388e4`): Defines the narrative perspective and GM style, referencing the Cantos.
    * `🖤 Chat History` (ID: `chatHistory`): Marker for chat history injection.
    * `🖤 World Info (Before)` (ID: `worldInfoBefore`) & `🖤 World Info (After)` (ID: `worldInfoAfter`): Markers for WI injection points.
    * `🖤 Character Description`, `🖤 Character Personality`, `🖤 Scenario Setting`, `🖤 Persona`: Markers for character card information.

## Frontend Integration (`ST-formatting.json`)

This file (not provided, description based on prior context) controls the underlying structure of how prompts are sent to the AI and how its responses are interpreted by the frontend (e.g., SillyTavern).

* **Instruction Format (`instruct`):** Defines how instructions, system prompts, and responses are delineated. This preset likely uses the `Alpaca` format (e.g., `### Instruction:`, `### Response:`).
* **Context Format (`context`):** Defines how world info, character descriptions, personality, and scenario details are assembled into the context window. It likely uses the `Default` story string format.
* **Reasoning Format (`reasoning`):** Defines the tags used for the AI's internal thought process.
  * **Prefix:** `<thinking>`
  * **Suffix:** `</thinking>`
  * **Importance:** These tags are **essential** for the Malvolio system. The **`☑️ Formatting Emphasis B (PHI v3.3)`** module mandates that responses start with `<thinking>`, and the Core Directives/CoT modules log their internal steps within these tags. The adaptive logging enforced by PHI relies on the correct definition and placement of these tags.

While you typically don't need to modify `ST-formatting.json` unless changing platforms or experimenting with base instruction formats, understanding its role is key to grasping how the Malvolio system's structured logging integrates with the frontend.

## Setup and Usage (Based on `Malvolio Main.json`)

1. **Load Preset:** Import `Malvolio Main.json` as your AI Preset and `Malvolio WI.json` as your World Info file in SillyTavern.
2. **Configure `Malvolio Main.json` (Advanced Users):**
    * Review basic settings (model, temperature, etc.) at the top level.
    * Navigate the `prompts` array to enable/disable components.
3. **Select a Core Directive:**
    * Ensure **ONE** Core Directive is enabled (`enabled: true`):
      * **Recommended:** `🖤☑️ Unified Core S (Robust Dynamic Core)` (ID: `246a07e7-df99-4f1a-8221-006942de28a1`)
      * **Alternative:** `🖤☑️ Unified Core C (CoT-Aware Enhanced Logging)` (ID: `97be081e-ab05-41ff-9625-d1ca738f2434`)
    * Ensure the other Core Directive is disabled (`enabled: false`).
4. **Enable PHI:**
    * Confirm `☑️ Formatting Emphasis B (PHI v3.3)` (ID: `30ea161d-3e83-43ec-be38-2c88a7e07b58`) is enabled (`enabled: true`).
5. **Enable Other Core Prompts:**
    * Verify the core prompts listed in section 6 above are enabled (`enabled: true`).
6. **Configure `Malvolio WI.json`:**
    * **Enable Cult Bible:** Ensure `Cult Bible` (UID: `3`) is enabled (`disable: false`).
    * **Select One Persona:** In the `ActivePersona` group (UIDs `0`, `1`, `2`), enable exactly **ONE** persona by setting its `disable` field to `false`. Ensure the other two personas have `disable: true`.
7. **Optional CoT Modules:**
    * **CoT A (External Roleplay):** Enable `🖤☑️ CoT A (Generic Native Handler)` (ID: `76350c8d-8209-4528-a4b4-fc19a16655c8`) if impersonating another character. **Disable CoT B, C, and D** if CoT A is active.
    * **CoT B/C/D (Internal Persona):** If Malvolio is acting as herself and you want deeper reasoning:
      * Enable **ONE** of `🖤☑️ CoT B`, `🖤☑️ CoT C`, or `🖤☑️ CoT D` that matches the selected Persona WI.
      * **Disable CoT A** and the other two persona CoTs.
    * **Core Fallback:** If no CoT module (A/B/C/D) is enabled, the AI follows the active Core Directive's default logging behavior (Concise for Core S, Detailed for Core C).
8. **Start Interaction:** Begin the chat. The AI will operate based on the activated modules and the selected Persona WI.

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
| **Other Core Prompts**                          | various                                    | Basic structure, rules, markers.                                            | **Enabled**           |
| (Main, Sys Guide, Post History, User Rules etc) | (See section 6)                            | Essential for base functionality.                                           | **Enabled**           |
