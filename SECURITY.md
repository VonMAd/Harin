# Security Policy — Harin (하린 · 夏凜) — TOPIK I (1급–2급) Korean Studio

## Zero-Malware & Defense-in-Depth Architecture

**Harin** is a 100% static, single-file, zero-CDN web application designed to run locally in your browser with minimal attack surface:

- **Zero Server / Zero Database / Zero Third-Party Scripts:** No backend server, no external JS/CSS CDNs, no npm supply-chain dependencies, no analytics, no ad trackers, and no cookies.
- **Strict SHA-256 Content-Security-Policy (CSP):**
  - `default-src 'none'` with an exact cryptographic script hash (`script-src 'sha256-TEdCS0TCkkolFDyuVVYiJiaRMI+ATYJM+BCB4WHYvhY='`) that blocks all injected inline scripts, `eval()`, `new Function()`, inline `on*=` attributes, and external script loads.
  - `object-src 'none'`, `child-src 'none'`, `frame-src 'none'`, `base-uri 'none'`, `form-action 'none'`, `worker-src 'none'`, `manifest-src 'none'`, and `upgrade-insecure-requests`.
  - Network connections (`connect-src`) are locked strictly to the 4 optional user-initiated TTS API hosts (`texttospeech.googleapis.com`, `api.openai.com`, `*.tts.speech.microsoft.com`, `api.elevenlabs.io`).
- **Clickjacking & Framing Defense:** Runtime `enforceFrameSafety()` hides and clears the DOM if framed by any untrusted external origin.
- **Zero-Trust Fetch & Optional API Key Hygiene:**
  - Works out of the box with **no API key**.
  - Optional TTS API keys default to **RAM-only session mode** (wiped automatically on tab close and never exported in backups or downloaded HTML snapshots).
  - All optional `fetch()` calls enforce `credentials: 'omit'`, `referrerPolicy: 'no-referrer'`, `cache: 'no-store'`, `redirect: 'error'`, strict input regex validation, and audio MIME / 10 MB payload guards.
- **Safe Backup Import & Anti-XSS:**
  - Prototype-pollution-proof `safeJsonParse()` (strips `__proto__`, `constructor`, `prototype`) with a 256 KB import ceiling.
  - Strict allowlisted schema sanitization and numeric clamping on imported `.json` backups and stored exam history.
  - Runtime `Object.freeze()` on static security configuration tables.
- **Verified Malware-Free Static Audit:** Verified `0` `eval`/`Function`/`document.write`/`WebSocket`/`XMLHttpRequest`/`sendBeacon` sinks, `0` Trojan-Source Bidi or invisible zero-width characters, and `0` embedded secrets.

## Verify File Integrity (SHA-256 Checksums)

You can cryptographically verify that your local `Harin.html` or `index.html` matches the official unmodified build:

| File | SHA-256 |
| --- | --- |
| `Harin.html` | `d2a7f1907d6d0972d57e4246ede3d206bd31c2aa590cb1b4fd48a82512ca090b` |
| `index.html` | `d2a7f1907d6d0972d57e4246ede3d206bd31c2aa590cb1b4fd48a82512ca090b` |
| Inline `<script>` CSP Digest | `sha256-TEdCS0TCkkolFDyuVVYiJiaRMI+ATYJM+BCB4WHYvhY=` |

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
