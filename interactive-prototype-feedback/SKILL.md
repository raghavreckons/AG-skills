---
name: interactive-prototype-feedback
description: Build HTML/CSS/JS frontend prototypes equipped with visual annotation overlays to log user design feedback.
---

# interactive-prototype-feedback

This skill outlines how to build interactive, high-fidelity prototypes that include a built-in visual annotation overlay. Users can drop feedback pins, slide the comments sidebar, swap alignment edges, and copy all notes to their clipboard as a Markdown list.

## When to Use
Use this skill when developing mockups or frontend prototypes for user feedback sessions. It allows users to flag UI components directly inside the browser viewport.

## Core Features to Include

1.  **Leave Feedback Toggle Button**: A sticky button at the bottom-right corner that toggles Feedback Mode.
2.  **Crosshair Cursor, Pin Dropping & DOM Context Capture**: When active, the body cursor changes to `crosshair`. Clicking elements logs `(pageX, pageY)` coordinates, traverses the DOM upward to extract the element's CSS selector path (prioritizing explicit `id` and `data-feedback-id` attributes) along with the nearest textual context (heading, label, button text), and opens a text comment input.
3.  **Comments Sidebar**: Slides out from the left or right edge.
4.  **Swap Sidebar Side**: A button in the sidebar header to toggle side alignment (e.g. `left-aligned` vs `right-aligned`) so the sidebar never occludes target design elements.
5.  **Pin Categorization**: Split pins into:
    *   *Active Session Pins*: Drawn visually on the screen.
    *   *Saved/Previous Comments*: Listed as text notes in a separate list block to avoid misalignment issues from layout changes.
6.  **Markdown Exporter & Session Completion**: Formats new annotations into a markdown list to copy/paste back to the chat, including the **Target CSS Selector Path** and **Textual Context** to help the AI agent easily locate the commented elements. On copy execution, it completes the session and pushes those comments to the saved/previous comments queue (setting their status to saved/inactive, e.g., `isNew = false`), updating localStorage and re-rendering the UI. This ensures that only new feedback pins from the current active session are copied in an optimized way, reducing duplicate comment noise in the chat. If no new active pins exist, it falls back to copying all saved comments.

### Annotations Overlay Boilerplate (HTML/JS)
Inject this code into the prototype HTML file:
```html
<!-- Feedback Toolbar -->
<div class="feedback-toolbar" style="position:fixed; bottom:24px; right:24px; display:flex; gap:12px; z-index:2000;">
  <button id="feedback-list-toggle" onclick="toggleSidebarVisibility()" class="btn">📋 View Pins (<span id="feedback-count">0</span>)</button>
  <button id="feedback-toggle" onclick="toggleFeedbackMode()" class="btn">💬 Leave Feedback</button>
</div>

<!-- Repositionable Sidebar -->
<div class="feedback-sidebar right-aligned" id="comment-sidebar" style="position:fixed; top:0; height:100vh; width:360px; z-index:1999; display:flex; flex-direction:column; transition:0.3s;">
  <div class="header" style="display:flex; justify-content:space-between; align-items:center; padding:20px; border-bottom:1px solid #333;">
    <span onclick="toggleSidebarSide()" style="cursor:pointer; font-size:12px; color:#6366f1;">⇄ Swap Left/Right</span>
    <span onclick="closeSidebar()" style="cursor:pointer; font-size:24px;">&times;</span>
  </div>
  <div id="comment-list-container" style="flex-grow:1; overflow-y:auto; padding:20px;"></div>
  <div class="footer" style="padding:20px; border-top:1px solid #333;">
    <button onclick="copyFeedbackMarkdown()" style="width:100%;">Copy Comments for AI</button>
  </div>
</div>
```
