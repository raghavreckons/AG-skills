---
name: sandboxed-prototype-testing
description: Guidelines and code templates to execute headful visual UI/UX sanity checks and headless logic test suites on dynamic HTML/JS pages.
---

# sandboxed-prototype-testing

This skill outlines how to build and execute testing suites against standard HTML/JS prototype files, combining sandboxed Node.js `vm` logic tests with headful browser UI/UX sanity audits.

## When to Use
Use this skill to verify prototype pages. Use sandboxed `vm` context evaluations to quickly test functional business logic (data state mutations, event handlings) and use a headful browser automation script (via Puppeteer/Playwright) to visually audit layouts, overlaps, unhandled exceptions, and interactive elements.

## Core Mocks Required

1. **Browser Globals**: Mock `window`, `document`, and `localStorage` before running context evaluations:
```javascript
const localStorageMock = (function() {
  let store = {};
  return {
    getItem: (key) => store[key] || null,
    setItem: (key, value) => { store[key] = String(value); },
    removeItem: (key) => { delete store[key]; },
    clear: () => { store = {}; }
  };
})();

const domSandbox = {
  window: {},
  document: {
    getElementById: (id) => mockElement(id),
    querySelectorAll: (selector) => [],
    createElement: (tag) => ({ style: {}, appendChild: () => {} })
  },
  localStorage: localStorageMock,
  console: console,
  parseFloat: parseFloat,
  parseInt: parseInt,
  Math: Math,
  Date: Date
};
```

2. **Scoping Conversions (Block Scopes to Global Scope)**:
In Node.js `vm.runInContext`, variables declared at the top-level using block-scoped keywords (`let`, `const`) are local to that evaluated block and do not populate as properties on the global context sandbox object. To allow your assertions to read state properties, replace them with `var` declarations prior to evaluation:
```javascript
let scriptContent = fs.readFileSync(htmlPath, 'utf8');
// Extract Javascript from script tags
const jsCode = scriptContent.match(/<script>([\s\S]*?)<\/script>/)[1];
// Substitute block scopes with var for global visibility
const varScopedCode = jsCode
  .replace(/const state\s*=/g, 'var state =')
  .replace(/let activeRx\s*=/g, 'var activeRx =');
```

3. **Element Mocks & Assertions**:
Return objects that mirror DOM interactions (value assignments, click triggers, innerText reads) to evaluate state mutations correctly.

## Headful Visual & Usability Testing

When performing visual validations (checking layout overlaps, looking for blocked content, catching rendering bugs), use a headful browser automation script (e.g., using `puppeteer` with `{ headless: false }`).

### 1. Zero-Dependency Local Static Server
Spin up a lightweight HTTP server to host the static prototype during E2E runs:
```javascript
const http = require('http');
const fs = require('fs');
const path = require('path');

const PORT = 8080;
const server = http.createServer((req, res) => {
  fs.readFile(path.join(__dirname, 'index.html'), 'utf8', (err, html) => {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end(html);
  });
});
server.listen(PORT);
```

### 2. Visibility Audit Helper
Visual sanity checks must filter out elements that are hidden (e.g., inside inactive modals, tabs, or dropdown overlays). Use a recursive visibility helper in the browser context:
```javascript
function isVisible(el) {
  if (!el) return false;
  const style = window.getComputedStyle(el);
  if (style.display === 'none' || style.visibility === 'hidden' || style.opacity === '0') return false;
  
  let parent = el.parentElement;
  while (parent) {
    const parentStyle = window.getComputedStyle(parent);
    if (parentStyle.display === 'none' || parentStyle.visibility === 'hidden' || parentStyle.opacity === '0') return false;
    parent = parent.parentElement;
  }
  
  const rect = el.getBoundingClientRect();
  return rect.width > 0 && rect.height > 0;
}
```

### 3. Layout Overlap Audit Template
Inspect active interactive components (like buttons) to make sure they do not overlap physically within the viewport:
```javascript
const activeButtons = Array.from(document.querySelectorAll('button:not([disabled])')).filter(isVisible);
for (let i = 0; i < activeButtons.length; i++) {
  for (let j = i + 1; j < activeButtons.length; j++) {
    const rect1 = activeButtons[i].getBoundingClientRect();
    const rect2 = activeButtons[j].getBoundingClientRect();
    
    const overlap = !(rect1.right < rect2.left || 
                      rect1.left > rect2.right || 
                      rect1.bottom < rect2.top || 
                      rect1.top > rect2.bottom);
    if (overlap) {
      console.warn(`Buttons overlap physically: ${activeButtons[i].innerText} & ${activeButtons[j].innerText}`);
    }
  }
}
```

