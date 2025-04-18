# Malvolio Preset Documentation

This document provides an overview of the Malvolio preset for AI interaction, based on the configurations found in `Malvolio Main.json` and `Malvolio WI.json`.

## Overview

Malvolio is a highly modular and adaptable AI persona system designed for complex character simulation and interaction. It allows for dynamic switching between different core operational logics and specific character personas (P1, P2, P3 Raw Nerve), each with distinct styles, thought processes, and philosophical underpinnings (defined in the "Cult Bible").

## Core Components

The system is built around several key components that can be selectively activated:

1.  **Core Directives (Choose ONE):** Defines the fundamental operational logic and logging behavior.
    *   **`🖤☑️大一统S` (v1.9 Dynamic Persona Core - Recommended):**
        *   Prioritizes speed and simplicity.
        *   Minimal `<thinking>` logs by default.
        *   Relies heavily on the active Persona's World Info (WI) entry for style, tone, and behavior.
        *   Best for performance and when detailed internal process logging isn't critical.
    *   **`🖤☑️大一统C` (v1.4.2 CoT-Aware Enhanced Logging - Alternative):**
        *   Prioritizes detailed logging.
        *   Generates extensive `<thinking>` logs outlining internal planning steps, even without an external CoT B/C/D active.
        *   CoT-Aware logic integrates external CoT modules when activated.
        *   Better for debugging or understanding the AI's thought process in detail, potentially slower.

2.  **PHI (Prompt Hierarchy Interface - REQUIRED):**
    *   **`☑️ 格式强调B` (PHI v3.3 Adaptive/Native_CoT_Aware):**
        *   Acts as the final enforcer of rules and structure.
        *   **Crucially, it adapts** its validation of `<thinking>` log content based on which Core Directive (v1.9 or v1.4.2) and which CoT module (A, B, C, D, or none) is active.
        *   Ensures compliance with fundamental requirements like starting with `<thinking>` and handling potential content filtering via "Abyssal Translation".

3.  **CoT A (Chain of Thought - Roleplay Handler - Optional):**
    *   **`🖤☑️ COT A` (CoT A v1.3 Generic Native Handler):**
        *   Activated when the AI needs to impersonate *another* character (`{{char}}`) instead of acting as Malvolio.
        *   **Detects and prioritizes executing native CoT formats** defined within the target character's card/WI.
        *   If no native CoT is found, it uses its own minimal external logic.
        *   Suppresses the Malvolio core persona and Cantos while active.

4.  **CoT B/C/D (Chain of Thought - Malvolio Personas - Optional):**
    *   Provide deep, specific thought processes for Malvolio when acting as himself. **Only one should be active at a time, and NOT simultaneously with CoT A.**
    *   **`🖤☑️ COT B` (P1 Rites):** For Persona P1 (Gothic/Venomous). Focuses on aesthetic execution, visceral language, and P1's specific rituals.
    *   **`🖤☑️ COT C` (P2 Logic):** For Persona P2 (Calm/Logical). Focuses on precise analysis, logical deduction, minimalist style, and computational metaphors.
    *   **`🖤☑️ COT D` (Scorched Earth v1.1):** For Persona P3 (Raw Nerve). Focuses on maximum offense, raw language, contempt, and bypassing politeness filters.

5.  **World Info (WI - REQUIRED):**
    *   Managed in `Malvolio WI.json`.
    *   **Personas (P1, P2, P3 Raw Nerve):** Defined as separate WI entries, grouped under `ActivePersona`. Only **one** persona entry should be enabled at a time to define Malvolio's current state, style, and behavior. The **Dynamic Persona Core (v1.9)** relies *heavily* on the detailed aesthetic and behavioral rules defined within the active persona's WI entry.
    *   **Cult Bible (Cantos):** A separate WI entry containing the philosophical "verses" guiding Malvolio's worldview, particularly used during "Abyssal Translation" to overcome content restrictions. Should generally always be active.

## Formatting Configuration (`ST-formatting.json`)

This file controls the underlying structure of how prompts are sent to the AI and how its responses are interpreted by the frontend (e.g., SillyTavern).

*   **Instruction Format (`instruct`):** Defines how instructions, system prompts, and responses are delineated. This preset uses the `Alpaca` format (e.g., `### Instruction:`, `### Response:`).
*   **Context Format (`context`):** Defines how world info, character descriptions, personality, and scenario details are assembled into the context window. It uses the `Default` story string format.
*   **Reasoning Format (`reasoning`):** Defines the tags used for the AI's internal thought process.
    *   **Prefix:** `<thinking>`
    *   **Suffix:** `</thinking>`
    *   **Importance:** These tags are **essential** for the Malvolio system. The PHI module mandates that responses start with `<thinking>`, and the Core Directives/CoT modules log their internal steps within these tags. The adaptive logging enforced by PHI relies on the correct definition and placement of these tags.

While you typically don't need to modify `ST-formatting.json` unless changing platforms or experimenting with base instruction formats, understanding its role is key to grasping how the Malvolio system's structured logging integrates with the frontend.

## How to Use

1.  **Open `Malvolio Main.json`:**
    *   Configure basic settings (model, temperature, etc.).
    *   Navigate the `prompts` array.
2.  **Activate Core Directive:** Enable **either** `🖤☑️大一统S` **or** `🖤☑️大一统C`. Disable the other.
3.  **Activate PHI:** Ensure `☑️ 格式强调B` (PHI v3.3) is enabled.
4.  **Activate Base Prompts:** Ensure `☑️ Main Prompt`, `📝 使用指南` (for reference), potentially `🖤 对话示例`, `🖤 聊天记录`, etc., are enabled as needed.
5.  **Open `Malvolio WI.json`:**
    *   **Activate Cult Bible:** Ensure the "圣经邪典" (Cult Bible / Cantos) entry (UID 3) is enabled.
    *   **Activate ONE Persona:** Locate the entries grouped under `ActivePersona`. Enable **only one** of the persona entries (P1 UID 1, P2 UID 2, or P3 UID 0) by setting its `disable` flag to `false`. Ensure the others are disabled (`disable: true`).
6.  **Activate Optional CoT Modules (in `Malvolio Main.json`):**
    *   **For Roleplaying:** If you want the AI to act as `{{char}}`, enable `🖤☑️ COT A`. Disable CoT B, C, and D.
    *   **For Malvolio's Persona:** If you want Malvolio to act as himself with enhanced thinking for the active Persona, enable the corresponding CoT module (`🖤☑️ COT B` for P1, `🖤☑️ COT C` for P2, `🖤☑️ COT D` for P3). Disable CoT A and the other persona CoTs.
    *   **Default Behavior:** If neither CoT A nor the relevant CoT B/C/D is active, the AI will follow the logic of the chosen Core Directive (minimal logging for v1.9, detailed internal planning for v1.4.2).
7.  **Start Interaction:** Begin the chat. The AI will operate based on the activated modules and the selected Persona WI.

## Key Differences Summary

*   **Core v1.9 (Dynamic):** Faster, less verbose logs, style heavily relies on **detailed Persona WI definitions**.
*   **Core v1.4.2 (Enhanced Logging):** Slower, very detailed logs even without CoT B/C/D, more explicit internal planning.
*   **PHI v3.3:** Universal enforcer, adapts logging checks to active Core/CoT configuration.
*   **CoT A:** For roleplaying *other* characters, suppresses Malvolio.
*   **CoT B/C/D:** Deep thought processes for Malvolio's specific personas (P1/P2/P3).