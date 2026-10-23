# App Service on Linux: patch status by release channel

Status as of 23 October 2026, 09:00 UTC, from canary data.

Each table shows, for one critical issue, when each built-in runtime stack was first seen patched on each Platform Release Channel. Times are UTC, rounded down to the hour. EXPOSED means the current image is not patched.

## When each issue became public

| Issue | CVE | Public (UTC) |
|---|---|---|
| eMudhra intermediate CA key compromise | CVE-2026-111943 | 17 Oct 2026, 20:06 (reported to browser root programs) |
| Brotli decoder remote code execution | CVE-2026-111497 | 21 Oct 2026, 10:39 (Node.js security release) |
| php2shell remote code execution | CVE-2026-109618, CVE-2026-109619 | 21 Oct 2026, 14:00 (PHP security release) |

## php2shell remote code execution

| Stack | Latest | Standard | Extended |
|---|---|---|---|
| PHP 8.5 | Before monitoring (≤ 22 Oct 11:00) | Before monitoring (≤ 22 Oct 12:00) | 22 Oct 23:00 |
| PHP 8.4 | Before monitoring (≤ 22 Oct 17:00) | Before monitoring (≤ 22 Oct 17:00) | 22 Oct 23:00 |
| PHP 8.3 | Before monitoring (≤ 22 Oct 17:00) | Before monitoring (≤ 22 Oct 17:00) | 22 Oct 23:00 |
| PHP 8.2 | Before monitoring (≤ 22 Oct 17:00) | Before monitoring (≤ 22 Oct 17:00) | 22 Oct 23:00 |

Patched means the PHP runtime is at or above 8.5.12, 8.4.27, 8.3.36 or 8.2.35.

## Brotli decoder remote code execution

| Stack | Latest | Standard | Extended |
|---|---|---|---|
| Node 26 | 22 Oct 20:00 | 22 Oct 23:00 | EXPOSED |
| Node 24 | 22 Oct 20:00 | 22 Oct 23:00 | EXPOSED |
| Node 22 | 22 Oct 20:00 | 22 Oct 23:00 | EXPOSED |
| PHP 8.5 | EXPOSED | EXPOSED | EXPOSED |
| PHP 8.4 | EXPOSED | EXPOSED | EXPOSED |
| PHP 8.3 | EXPOSED | EXPOSED | EXPOSED |
| PHP 8.2 | EXPOSED | EXPOSED | EXPOSED |
| .NET 11 | EXPOSED | EXPOSED | EXPOSED |
| .NET 10 | EXPOSED | EXPOSED | EXPOSED |
| .NET 9 | EXPOSED | EXPOSED | EXPOSED |
| .NET 8 | EXPOSED | EXPOSED | EXPOSED |

Patched means, for Node, the runtime is at or above 26.12.1, 24.22.1 or 22.23.4. No fix has been released for PHP or .NET. PHP's curl uses the operating system's libbrotli1, which no Linux distribution has patched yet, and the .NET runtime bundles a vulnerable copy. Both are exploitable only when an application decodes Brotli data from an untrusted source.

## eMudhra intermediate CA key compromise

| Stack | Latest | Standard | Extended |
|---|---|---|---|
| Node 26 | 22 Oct 20:00 | 22 Oct 23:00 | EXPOSED |
| Node 24 | 22 Oct 20:00 | 22 Oct 23:00 | EXPOSED |
| Node 22 | 22 Oct 20:00 | 22 Oct 23:00 | EXPOSED |
| PHP 8.5 | 22 Oct 20:00 | 22 Oct 23:00 | 22 Oct 23:00 |
| PHP 8.4 | 22 Oct 20:00 | 22 Oct 23:00 | 22 Oct 23:00 |
| PHP 8.3 | 22 Oct 20:00 | 22 Oct 23:00 | 22 Oct 23:00 |
| PHP 8.2 | EXPOSED | EXPOSED | EXPOSED |
| .NET 11 | 22 Oct 20:00 | 22 Oct 23:00 | EXPOSED |
| .NET 10 | 22 Oct 20:00 | 22 Oct 23:00 | EXPOSED |
| .NET 9 | 22 Oct 20:00 | 22 Oct 23:00 | EXPOSED |
| .NET 8 | 22 Oct 20:00 | 22 Oct 23:00 | EXPOSED |

Patched means the operating system trust store contains neither emSign Root TLS CA - G3 nor emSign ECC Root CA - G3, and certificates cross-signed by AAA Certificate Services are measured as untrusted. PHP 8.2 runs on Debian 11, whose trust store still contains both roots on every channel.

## How this is measured

A canary app runs on every built-in Linux runtime version and release channel (33 in all). Each is restarted every 30 minutes, so each start picks up the image currently served on its channel. Every start reports the runtime version, the operating system, and the eMudhra roots in each trust store it can read. The .NET canaries also build the chains of the eight compromised intermediates, including their AAA Certificate Services cross-signed copies.

A cell's time is the first report on that channel showing the fix, after which every later report stays fixed. A channel can be patched up to about 30 minutes before the canary sees it. "Before monitoring" means the channel was already patched when monitoring began, so the actual patch time is earlier.

An application on App Service picks up a patched image only when it restarts.

## Sources

- php2shell: [Azure App Service blog](https://azure.github.io/AppService/2026/10/21/php-critical-security-release-app-service-linux.html), [Help Net Security](https://www.helpnetsecurity.com/2026/10/21/php2shell-php-vulnerabilities-patched-cve-2026-109618-cve-2026-109619/)
- Brotli: [The Hacker News](https://thehackernews.com/2026/10/nodejs-brotli-decoder-bug-could-allow.html), [explainx.ai](https://explainx.ai/blog/nodejs-security-release-october-2026-brotli-rce-tls-hostname-bypass-emudhra-2026), [HOL Guard](https://hol.org/guard/security/cves/CVE-2026-111497-out-of-bounds-write-in-google-brotli-decoder)
- eMudhra: [Mozilla security blog](https://blog.mozilla.org/security/2026/10/20/protecting-firefox-users-following-the-compromise-of-emudhra-intermediate-certificates/), [eMudhra statement](https://emudhra.com/en/post/certificate-security-incident)
