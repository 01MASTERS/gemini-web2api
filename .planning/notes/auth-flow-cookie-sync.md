---
title: Authentication Flow and Cookie Sync
date: 2026-08-03
context: Explored how gemini-web2api bypasses API keys
---

# Authentication Flow and Cookie Sync

The `gemini-web2api` server bridges OpenAI-compatible requests to the internal Google Gemini *Web* API. Because of this, it does not use a traditional Google developer API key.

## Mechanism
1. **Cookie Impersonation**: The server reads a `cookie_file` containing Google authentication cookies (such as `SAPISID`).
2. **SAPISIDHASH Generation**: In `gemini.py`, the `SAPISID` cookie is used to cryptographically generate a `SAPISIDHASH` header (`Authorization: SAPISIDHASH {timestamp}_{hash}`). This is Google's internal web authentication mechanism, allowing the server to trick Google into treating the request as coming directly from a logged-in browser session.
3. **The Extension**: The `gemini-cookie-sync-extension` exists to simplify obtaining these cookies. It reads the user's `google.com` cookies via the Chrome `chrome.cookies` API and exports them into the JSON/string format required by the server, saving the user from manually extracting them via dev tools.
