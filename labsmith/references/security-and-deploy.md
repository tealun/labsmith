# Security and deployment

Training labs are usually static (no accounts, no forms), which keeps the attack surface small — keep it that way.

## 1. Deploy scope

- Deploy only the build output (app bundle + `public/`). Generators, capture scripts, docs, sources and agent settings stay in the repository and never reach the server. Verify by listing the build directory before the first deploy and after structural changes.
- No secrets in the repository or the bundle; analytics or third-party scripts only after an explicit decision.

## 2. Response headers (static hosting with a `_headers` file)

```
/*
  X-Content-Type-Options: nosniff
  Referrer-Policy: strict-origin-when-cross-origin
  Strict-Transport-Security: max-age=31536000
  Permissions-Policy: camera=(), microphone=(), geolocation=(), payment=(), usb=()
  Cross-Origin-Opener-Policy: same-origin
  Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data: blob:; media-src 'self'; font-src 'self'; connect-src 'self'; object-src 'none'; base-uri 'self'; form-action 'none'; upgrade-insecure-requests
```

- `style-src 'unsafe-inline'` is needed when the 3D UI positions overlays with style attributes; scripts stay same-origin only. JSON-LD blocks are data and are not blocked.
- Guide pages: forbid framing by other sites (`X-Frame-Options: SAMEORIGIN`). The lab itself may stay embeddable for course platforms — make that an explicit decision.
- Long immutable caching for hashed assets; shorter caching for media and hand-written assets.

## 3. Testing headers locally

Local preview servers ignore `_headers`. Intercept responses in Playwright and add the CSP to HTML responses, then load a scene and fail on any console CSP violation or page error. After deployment, spot-check headers and redirects with `curl -sI`.

## 4. Inputs

- URL parameters (scene ids, shared state) are looked up or decoded defensively: own keys only, schema-validated decoding, length limits, and a clear rejection path.
- Redirect rules (`_redirects`) are generated from data and only ever point inside the site.

## 5. External links

- Reference links open publisher pages with `rel="external noopener"`; no user-generated links.
- A source checker reports broken, blocked and unlinked references; it is a maintenance tool, not part of the build.
