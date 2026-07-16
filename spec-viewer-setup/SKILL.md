---
name: spec-viewer-setup
description: Setup a zero-dependency Node.js server to read, render, and navigate markdown files locally in the browser.
---

# spec-viewer-setup

This skill enables you to set up a local Node.js specification server using only built-in libraries (`http`, `fs`, `path`). It allows users to view and navigate project markdown documentation in a clean, sidebar-driven browser layout.

## When to Use
Use this skill when you want to make specifications, implementation plans, and requirements easily readable and navigable locally without installing heavy NPM packages.

## Implementation Steps

1. Create a `viewer/server.js` file using standard Node.js libraries.
2. The server should serve static files (HTML, CSS, JS, images) and parse request paths.
3. If a request is for a `.md` file, the server should read the markdown file and wrap it in a clean HTML shell with a sidebar, rendering the markdown sections.
4. Add a basic Markdown-to-HTML parser regex (for headers, lists, code blocks, tables, and links) so it renders correctly without external dependencies.
5. Serve the server on port `3000`.

### Node.js Zero-Dependency Server Template
Save this as `viewer/server.js` in the target project workspace:
```javascript
const http = require('http');
const fs = require('fs');
const path = require('path');

const PORT = 3000;

const mimeTypes = {
  '.html': 'text/html',
  '.css': 'text/css',
  '.js': 'text/javascript',
  '.png': 'image/png',
  '.jpg': 'image/jpeg',
  '.svg': 'image/svg+xml',
  '.md': 'text/html'
};

const server = http.createServer((req, res) => {
  let filePath = req.url === '/' || req.url === '' ? './index.html' : '.' + req.url;
  filePath = filePath.split('?')[0]; // strip query strings
  const ext = path.extname(filePath);

  fs.stat(filePath, (err, stats) => {
    if (err || !stats.isFile()) {
      res.writeHead(404, { 'Content-Type': 'text/plain' });
      return res.end('File Not Found');
    }

    if (ext === '.md') {
      // Parse markdown file and wrap in layout template
      fs.readFile(filePath, 'utf8', (err, content) => {
        if (err) {
          res.writeHead(500);
          return res.end('Error reading markdown');
        }
        const html = renderMarkdownPage(content, path.basename(filePath));
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end(html);
      });
    } else {
      // Serve static file
      fs.readFile(filePath, (err, content) => {
        if (err) {
          res.writeHead(500);
          return res.end('Error reading file');
        }
        res.writeHead(200, { 'Content-Type': mimeTypes[ext] || 'application/octet-stream' });
        res.end(content);
      });
    }
  });
});

function renderMarkdownPage(mdContent, filename) {
  // Simple markdown conversion regexes
  let html = mdContent
    .replace(/^# (.*$)/gim, '<h1>$1</h1>')
    .replace(/^## (.*$)/gim, '<h2>$1</h2>')
    .replace(/^### (.*$)/gim, '<h3>$1</h3>')
    .replace(/^\* (.*$)/gim, '<li>$1</li>')
    .replace(/\[([^\]]+)\]\(([^)]+)\)/g, '<a href="$2">$1</a>')
    .replace(/\`\`\`([\s\S]*?)\`\`\`/g, '<pre><code>$1</code></pre>');

  return `
    <!DOCTYPE html>
    <html>
    <head>
      <title>${filename} - Spec Viewer</title>
      <style>
        body { font-family: sans-serif; background: #0f172a; color: #f8fafc; padding: 40px; line-height: 1.6; }
        pre { background: #1e293b; padding: 16px; border-radius: 8px; overflow-x: auto; }
        a { color: #6366f1; text-decoration: none; }
        h1, h2, h3 { color: #f1f5f9; }
      </style>
    </head>
    <body>
      <p><a href="/">← Back to Hub</a></p>
      <hr>
      ${html}
    </body>
    </html>
  `;
}

server.listen(PORT, () => {
  console.log(`Spec viewer running at http://localhost:${PORT}`);
});
```
