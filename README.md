# Antigravity (AG) Custom Skills

This repository contains custom agent skills developed for the **Prescribe** project. These skills extend the capabilities of the Antigravity AI agent.

## Included Skills

### 1. [interactive-prototype-feedback](file:///c:/Users/ragha/OneDrive/Desktop/Projects/AG-skills/interactive-prototype-feedback)
- **Purpose**: Build HTML/CSS/JS frontend prototypes equipped with visual annotation overlays to log user design feedback.
- **Usage**: Use this skill when you want to gather interactive annotations and feedback directly on prototype layouts.

### 2. [sandboxed-prototype-testing](file:///c:/Users/ragha/OneDrive/Desktop/Projects/AG-skills/sandboxed-prototype-testing)
- **Purpose**: Guidelines and code templates to execute headful visual UI/UX sanity checks and headless logic test suites on dynamic HTML/JS pages.
- **Usage**: Use this skill for running automated UI and UX validation in a sandboxed environment.

### 3. [spec-viewer-setup](file:///c:/Users/ragha/OneDrive/Desktop/Projects/AG-skills/spec-viewer-setup)
- **Purpose**: Setup a zero-dependency Node.js server to read, render, and navigate markdown specification files locally in the browser.
- **Usage**: Use this skill to instantly view project specs in a clean, readable web interface.

## Using with Claude Code

To use these custom skills with **Claude Code**, follow these steps:

1. **Add the Skill to Context:**
   Add the specific `SKILL.md` file to Claude Code's active context:
   ```bash
   # Add interactive-prototype-feedback skill
   claude add interactive-prototype-feedback/SKILL.md
   
   # Add sandboxed-prototype-testing skill
   claude add sandboxed-prototype-testing/SKILL.md
   
   # Add spec-viewer-setup skill
   claude add spec-viewer-setup/SKILL.md
   ```

2. **Instruct Claude to Execute the Skill:**
   Tell Claude Code to follow the instructions defined in the added skill. For example:
   > *"Set up a spec viewer using the guidelines in `spec-viewer-setup/SKILL.md`."*

3. **Global Integration (.clauderules):**
   To make these instructions always available to Claude without manually adding them, copy the content of the relevant `SKILL.md` file into your local project's `.clauderules` file.

## Token Savings

Using custom agent skills reduces token consumption by leveraging pre-defined templates, standardized libraries, and structured workflows. This eliminates the need for the agent to re-invent code, write complex boilerplates from scratch, or engage in multi-turn debugging cycles.

Here is the estimated token usage comparison and savings:

| Custom Skill | Without Skill (Raw Code Gen & Debug Loops) | With Skill (Modular Templates & Scripts) | Estimated Savings (%) | Typical Tokens Saved |
| :--- | :--- | :--- | :--- | :--- |
| **[interactive-prototype-feedback](file:///c:/Users/ragha/OneDrive/Desktop/Projects/AG-skills/interactive-prototype-feedback)** | **~15,000 - 25,000 tokens**<br>• Writing custom canvas/overlay logic.<br>• Multiple styling/positioning iterations.<br>• Multi-turn feedback schema design. | **~2,000 - 3,000 tokens**<br>• Loading pre-built feedback overlay code.<br>• Standardized logger integration. | **85%** | **~13,000 - 22,000** |
| **[sandboxed-prototype-testing](file:///c:/Users/ragha/OneDrive/Desktop/Projects/AG-skills/sandboxed-prototype-testing)** | **~20,000 - 35,000 tokens**<br>• Generating Playwright/Puppeteer configurations.<br>• Writing custom browser test runners.<br>• Iterative debugging of headless errors. | **~3,000 - 5,000 tokens**<br>• Ready-made test harness templates.<br>• Standardized assertions and viewport configs. | **85%** | **~17,000 - 30,000** |
| **[spec-viewer-setup](file:///c:/Users/ragha/OneDrive/Desktop/Projects/AG-skills/spec-viewer-setup)** | **~8,000 - 12,000 tokens**<br>• Writing custom markdown parsers.<br>• Setting up router and CSS structure.<br>• Debugging Node.js HTTP server. | **~1,000 - 1,500 tokens**<br>• Running a single command with zero-dependency pre-built script. | **88%** | **~7,000 - 10,500** |

### Why these savings occur:
1. **Zero-Shot Reuse:** The agent avoids writing large boilerplate code segments.
2. **Fewer Roundtrips:** Standardized templates reduce compilation/syntax errors, minimizing the number of corrective steps.
3. **Context Optimization:** High-level instructions in `SKILL.md` are extremely compact, reducing input context load compared to full documentation.

---
*Created automatically to package and share custom agent skills.*

