# Security Cleanup & Sanity Check Design

## Purpose
Improve the frontend security posture of the static portfolio website by implementing modern security headers and best practices for external resources.

## Architecture & Components

### 1. External Links Security
- **What:** Update all anchor tags containing `target="_blank"`.
- **How:** Add `rel="noopener noreferrer"` to prevent the new page from accessing the `window.opener` object, mitigating reverse tabnabbing attacks.

### 2. Subresource Integrity (SRI)
- **What:** Secure the external dependency (D3.js).
- **How:**
  - Pin the D3.js CDN URL to a specific version (e.g., `v7.9.0`) to ensure file immutability.
  - Generate and inject the SHA-256 integrity hash into the `<script>` tag.
  - Include `crossorigin="anonymous"`.

### 3. Content Security Policy (CSP)
- **What:** Add a strict CSP via HTML meta tag to restrict resource loading.
- **How:** Inject `<meta http-equiv="Content-Security-Policy" content="...">` in the `<head>` of `index.html`.
- **Policy:** `default-src 'self'; script-src 'self' 'unsafe-inline' https://cdn.jsdelivr.net; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; connect-src 'self';`

## Error Handling & Testing
- Load `index.html` locally and verify the browser console for any CSP violations.
- Ensure the D3 visualizations (GPA sparkline) render correctly.
- Verify that clicking external links opens them in a new tab without passing the opener context.
