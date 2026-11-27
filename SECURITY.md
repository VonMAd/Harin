# Security Policy — Harin (하린 · 夏凜) — TOPIK I (1급–2급) Korean Studio

## Zero-Malware & Defense-in-Depth Architecture

**Harin** is a 100% static, single-file, zero-CDN web application designed to run locally in your browser with minimal attack surface:

- **Zero Server / Zero Database / Zero Third-Party Scripts:** No backend server, no external JS/CSS CDNs, no npm supply-chain dependencies, no analytics, no ad trackers, and no cookies.
- **Strict SHA-256 Content-Security-Policy (CSP):**
  - `default-src 'none'` with an exact cryptographic script hash (`script-src 'sha256-QswborXNGL0zY539MZxmdaNpnACms2JSnvm8xD4FMFY='`) that blocks all injected inline scripts, `eval()`, `new Function()`, inline `on*=` attributes, and external script loads.
  - `object-src 'none'`, `child-src 'none'`, `frame-src 'none'`, `base-uri 'none'`, `form-action 'none'`, `worker-src 'none'`, `manifest-src 'none'`, and `upgrade-insecure-requests`.
  - Network connections (`connect-src`) are locked strictly to the optional user-initiated Private GitHub Gist sync API (`https://api.github.com`) and the 4 optional TTS API hosts (`texttospeech.googleapis.com`, `api.openai.com`, `*.tts.speech.microsoft.com`, `api.elevenlabs.io`).
- **Clickjacking & Framing Defense:** Runtime `enforceFrameSafety()` hides and clears the DOM if framed by any untrusted external origin.
- **Zero-Trust Fetch, Cross-Device Sync & Secret Hygiene:**
  - Works out of the box with **no API key or account**; supports zero-account Magic `#sync` Links & Sync Codes (client-side URL `#hash` only — never sent to servers) and optional automatic Private GitHub Gist Cloud Sync (`public: false`, Gist-only token).
  - Optional TTS API keys default to **RAM-only session mode** (wiped automatically on tab close), and neither TTS API keys nor GitHub sync tokens are ever exported into `.json` backups, Sync Codes/Links, Gist payloads, or downloaded HTML snapshots.
  - All optional `fetch()` calls enforce `credentials: 'omit'`, `referrerPolicy: 'no-referrer'`, `cache: 'no-store'`, `redirect: 'error'`, strict input regex validation, and Content-Type / payload size guards.
- **Safe Backup Import, Smart Merge & Anti-XSS:**
  - Prototype-pollution-proof `safeJsonParse()` (strips `__proto__`, `constructor`, `prototype`) with a 256 KB import/decompression ceiling.
  - Strict allowlisted schema sanitization and numeric clamping on all imported `.json` backups, Sync Codes, `#sync` links, GitHub Gist payloads, and stored exam history.
  - Runtime `Object.freeze()` on static security configuration tables.
- **Verified Malware-Free Static Audit:** Verified `0` `eval`/`Function`/`document.write`/`WebSocket`/`XMLHttpRequest`/`sendBeacon` sinks, `0` Trojan-Source Bidi or invisible zero-width characters, and `0` embedded secrets.

## Verify File Integrity (SHA-256 Checksums)

You can cryptographically verify that your local `Harin.html` or `index.html` matches the official unmodified build:

| File | SHA-256 |
| --- | --- |
| `Harin.html` | `676d79545659eab1c2516c3bd1b6ffabee4b146aecaafd398d7a7190463bc3be` |
| `index.html` | `676d79545659eab1c2516c3bd1b6ffabee4b146aecaafd398d7a7190463bc3be` |
| Inline `<script>` CSP Digest | `sha256-QswborXNGL0zY539MZxmdaNpnACms2JSnvm8xD4FMFY=` |

**Windows (PowerShell):**
```powershell
Get-FileHash Harin.html -Algorithm SHA256
```

**macOS / Linux:**
```bash
sha256sum -c SHA256SUMS.txt
```

## Recommended GitHub Repository Protections (Maintainer Checklist)
1. Keep **Enforce HTTPS** enabled under **Settings → Pages** for `https://vonmad.github.io/Harin/`.
2. Enable **Two-Factor Authentication (2FA)** on the GitHub maintainer account.
3. Enable a **Branch Ruleset / Protection Rule** on `main` (restrict force pushes & deletions) and **Secret Scanning / Push Protection** under **Settings → Code security**.

## Reporting a Vulnerability
If you discover a security concern, please open a private security advisory under the **Security** tab at https://github.com/VonMAd/Harin/security.
