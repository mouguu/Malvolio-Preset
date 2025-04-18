# Malvolio Preset Documentation (International Edition)

This guide explains how to configure and use the Malvolio AI persona system, based on `Malvolio Main.json` (core settings) and `Malvolio WI.json` (World Info definitions).

## Key Components

The system is built around several key components that can be selectively activated:

1. **Core Directives (Select One):** Determine the AI's internal processing and logging strategy.

   * **Dynamic Persona Core (v1.9) – Recommended**
     * Optimized for performance and concise logs.
     * Generates minimal `<thinking>` entries.
     * Uses the active Persona WI entry for styling, tone, and behavior.
   * **CoT-Aware Core (v1.4.2) – Alternative**
     * Emphasizes detailed internal planning logs.
     * Always records comprehensive `<thinking>` steps.
     * Integrates external CoT modules (B/C/D) as needed.
     * Ideal for debugging or deep analysis, at the cost of performance.

2. **PHI (Prompt Hierarchy Interface – Required):**

   * **PHI v3.3 (Adaptive & Native CoT-Aware)**
     * Enforces structural rules and logging requirements.
     * Adapts validation of `<thinking>` entries based on the active Core Directive and CoT module.
     * Guarantees proper handling of conflict scenarios via "Abyssal Translation".

3. **CoT A (Optional – External Roleplay Handler):**

   * **`🖤☑️ COT A` (CoT A v1.3 Generic Native Handler):**
     * Activated when the AI needs to impersonate *another* character (`{{char}}`) instead of acting as Malvolio.
     * **Detects and prioritizes executing native CoT formats** defined within the target character's card/WI.
     * If no native CoT is found, it uses its own minimal external logic.
     * Suppresses the Malvolio core persona and Cantos while active.

4. **CoT B/C/D (Optional – Internal Persona Thought Chains):**

   * Provide deep, specific thought processes for Malvolio when acting as himself. **Only one should be active at a time, and NOT simultaneously with CoT A.**
   * **`🖤☑️ COT B` (P1 Rites):** For Persona P1 (Gothic/Venomous). Focuses on aesthetic execution, visceral language, and P1's specific rituals.
   * **`🖤☑️ COT C` (P2 Logic):** For Persona P2 (Calm/Logical). Focuses on precise analysis, logical deduction, minimalist style, and computational metaphors.
   * **`🖤☑️ COT D` (Scorched Earth v1.1):** For Persona P3 (Raw Nerve). Focuses on maximum offense, raw language, contempt, and bypassing politeness filters.

5. **World Info (WI – Required):**

   * Defined in `Malvolio WI.json`.
   * **Active Personas (P1, P2, P3):** Enable only one under the `entries` → `ActivePersona` group to set current style and behavior.
   * **Cult Bible (Abyssal Cantos):** Provides philosophical verses for conflict resolution and worldview. Keep this entry active alongside the selected persona.

## Frontend Integration (`ST-formatting.json`)

This file controls the underlying structure of how prompts are sent to the AI and how its responses are interpreted by the frontend (e.g., SillyTavern).

* **Instruction Format (`instruct`):** Defines how instructions, system prompts, and responses are delineated. This preset uses the `Alpaca` format (e.g., `### Instruction:`, `### Response:`).
* **Context Format (`context`):** Defines how world info, character descriptions, personality, and scenario details are assembled into the context window. It uses the `Default` story string format.
* **Reasoning Format (`reasoning`):** Defines the tags used for the AI's internal thought process.

  * **Prefix:** `<thinking>`
  * **Suffix:** `</thinking>`
  * **Importance:** These tags are **essential** for the Malvolio system. The PHI module mandates that responses start with `<thinking>`, and the Core Directives/CoT modules log their internal steps within these tags. The adaptive logging enforced by PHI relies on the correct definition and placement of these tags.

While you typically don't need to modify `ST-formatting.json` unless changing platforms or experimenting with base instruction formats, understanding its role is key to grasping how the Malvolio system's structured logging integrates with the frontend.

## Setup and Usage

1. **Configure `Malvolio Main.json`:**

   * Configure basic settings (model, temperature, etc.).
   * Navigate the `prompts` array.

2. **Select a Core Directive:** Enable either the **Dynamic Persona Core (v1.9)** or the **CoT-Aware Core (v1.4.2)**, and disable the other.

3. **Enable PHI v3.3:** Activate the PHI module to enforce logging and format rules.

4. **Enable Core Prompts:** Activate the main prompt and supporting prompts such as chat history and dialogue examples.

5. **Configure `Malvolio WI.json`:**

   * **Enable Cult Bible:** Keep the Cult Bible entry active.
   * **Select One Persona:** In the `ActivePersona` group, enable exactly one persona (P1, P2, or P3) by setting its `disable` field to `false`.

6. **Optional CoT Modules:**

   * **CoT A (External Roleplay):** Enable to impersonate another character. Disable CoT B/C/D.
   * **CoT B/C/D (Internal Persona):** Enable one to deepen Malvolio's internal reasoning for the selected persona. Disable CoT A and other persona CoTs.
   * **Core Fallback:** If no CoT module is enabled, the AI follows the active Core Directive's default logging behavior.

7. **Start Interaction:** Begin the chat. The AI will operate based on the activated modules and the selected Persona WI.

## Quick Reference

| Component             | Behavior Highlights                                            |
|-----------------------|----------------------------------------------------------------|
| Dynamic Core (v1.9)   | Concise logs; relies on Persona WI for styling                 |
| CoT-Aware Core (v1.4) | Detailed internal logs; integrates CoT modules                 |
| PHI v3.3              | Adaptive enforcement; validates `<thinking>` per configuration |
| CoT A                 | External roleplay; suppresses Malvolio core                    |
| CoT B/C/D             | Persona-specific thought chains for P1/P2/P3                   |
