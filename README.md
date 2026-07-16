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

---
*Created automatically to package and share custom agent skills.*

