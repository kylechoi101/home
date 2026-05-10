# Security Cleanup Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Improve frontend security by adding `rel="noopener noreferrer"` to external links, adding SRI to D3.js, and injecting a strict CSP meta tag.

**Architecture:** Modifying the static `index.html` file to include security attributes and headers.

**Tech Stack:** HTML, CSP

---

### Task 1: Secure External Links

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Write the failing test**

Run: `grep 'rel="noopener noreferrer"' index.html | wc -l | tr -d ' '`
Expected: `0`

- [ ] **Step 2: Write minimal implementation**

Use the `replace` tool with `allow_multiple: true` or update manually to replace `target="_blank"` with `target="_blank" rel="noopener noreferrer"` where appropriate.
Since the `replace` tool is specific, use it for the following 7 occurrences in `index.html`:

1. Hero Resume Link:
```html
<a href="./files/Resume.pdf" target="_blank" class="btn">View Resume</a>
```
Replace with:
```html
<a href="./files/Resume.pdf" target="_blank" rel="noopener noreferrer" class="btn">View Resume</a>
```

2. Mega Trend PDF Link:
```html
<a href="./files/Mega_Trend.pdf" target="_blank" class="report-item">
```
Replace with:
```html
<a href="./files/Mega_Trend.pdf" target="_blank" rel="noopener noreferrer" class="report-item">
```

3. HMM PDF Link:
```html
<a href="./files/HMM.pdf" target="_blank" class="report-item">
```
Replace with:
```html
<a href="./files/HMM.pdf" target="_blank" rel="noopener noreferrer" class="report-item">
```

4. Projects Script LinksHTML Array:
```javascript
linksHTML = p.links.map(l => `<a href="${l.url}" target="_blank" class="btn" style="padding: 6px 14px; font-size: 0.85rem;">${l.name}</a>`).join('');
```
Replace with:
```javascript
linksHTML = p.links.map(l => `<a href="${l.url}" target="_blank" rel="noopener noreferrer" class="btn" style="padding: 6px 14px; font-size: 0.85rem;">${l.name}</a>`).join('');
```

5. Projects Script LinksHTML Fallback:
```javascript
linksHTML = `<a href="${p.url}" target="_blank" class="btn" style="padding: 6px 14px; font-size: 0.85rem;">View Project</a>`;
```
Replace with:
```javascript
linksHTML = `<a href="${p.url}" target="_blank" rel="noopener noreferrer" class="btn" style="padding: 6px 14px; font-size: 0.85rem;">View Project</a>`;
```

6. Projects Script Card Image Link:
```javascript
<a href="${mainUrl}" target="_blank" style="display: block; text-decoration: none; color: inherit;">
```
Replace with:
```javascript
<a href="${mainUrl}" target="_blank" rel="noopener noreferrer" style="display: block; text-decoration: none; color: inherit;">
```

7. Projects Script Card Title Link:
```javascript
<a href="${mainUrl}" target="_blank" style="color: inherit; text-decoration: none;">${p.title}</a>
```
Replace with:
```javascript
<a href="${mainUrl}" target="_blank" rel="noopener noreferrer" style="color: inherit; text-decoration: none;">${p.title}</a>
```

- [ ] **Step 3: Run test to verify it passes**

Run: `grep 'rel="noopener noreferrer"' index.html | wc -l | tr -d ' '`
Expected: `7`

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "security: add rel=noopener noreferrer to external links"
```

### Task 2: Secure D3 Dependency (SRI)

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Write the failing test**

Run: `grep 'integrity=' index.html | wc -l | tr -d ' '`
Expected: `0`

- [ ] **Step 2: Write minimal implementation**

Find:
```html
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
```
Replace with:
```html
<script src="https://cdn.jsdelivr.net/npm/d3@7.9.0/dist/d3.min.js" integrity="sha256-8glLv2FBs1lyLE/kVOtsSw8OQswQzHr5IfwVj864ZTk=" crossorigin="anonymous"></script>
```

- [ ] **Step 3: Run test to verify it passes**

Run: `grep 'integrity=' index.html | wc -l | tr -d ' '`
Expected: `1`

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "security: add SRI hash and pin D3.js version"
```

### Task 3: Inject Content Security Policy (CSP)

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Write the failing test**

Run: `grep 'Content-Security-Policy' index.html | wc -l | tr -d ' '`
Expected: `0`

- [ ] **Step 2: Write minimal implementation**

Find:
```html
  <meta name="theme-color" content="#0b0f14" />
```
Replace with:
```html
  <meta name="theme-color" content="#0b0f14" />
  <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self' 'unsafe-inline' https://cdn.jsdelivr.net; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; connect-src 'self';" />
```

- [ ] **Step 3: Run test to verify it passes**

Run: `grep 'Content-Security-Policy' index.html | wc -l | tr -d ' '`
Expected: `1`

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "security: add strict Content Security Policy meta tag"
```