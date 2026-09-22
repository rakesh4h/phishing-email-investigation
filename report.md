# Phishing Investigation Report — Case 01

## Summary
A spoofed "storage full" alert email was investigated. The email failed DKIM 
and DMARC authentication, originated from a data-center hosting IP, and 
linked to a phishing page hosted on Google Cloud Storage that hides behind 
a fake Cloudflare check.

## Email Details
- **Subject:** ⚠️ Action Required: Storage 100% Full
- **From (display):** thakasirakesh
- **From (actual address):** alert-6147@vutew[.]jny
- **To:** me@aol.com
- **Date:** 21 September 2026

## Authentication Analysis
| Check | Result |
|---|---|
| SPF | Pass (from attacker's own domain — not meaningful) |
| DKIM | Fail (domain: 6pl6.sb001.aktiflari.my.id) |
| DMARC | Fail |

## Infrastructure Analysis
- **Sender IP:** 5.196.246.179
- **ISP:** OVH Hosting (data center / hosting provider, not a legitimate mail server)
- **AbuseIPDB:** 0% confidence, 1 prior report

## URL Analysis
- **URL:** hxxps://storage[.]googleapis[.]com/obsidianly/obsidianly.html?act=cl&pid=13677_md&uid=2
- Hosted on legitimate Google Cloud Storage infrastructure (abused by attacker)
- Contains per-victim tracking parameters (uid, pid, cid)
- Redirects behind a fake Cloudflare "checking your browser" page to evade automated scanners
- Final page likely a WordPress-based credential harvesting form

## IOCs
| Type | Value |
|---|---|
| Domain | vutew.jny |
| IP | 5.196.246.179 |
| URL | hxxps://storage[.]googleapis[.]com/obsidianly/obsidianly.html |

## Verdict
**Malicious — Phishing**

## Recommended Actions
- Block sender domain (vutew.jny) and IP (5.196.246.179) at the email gateway
- Block/report the malicious URL to Google (abuse of Cloud Storage)
- Search mailbox for other recipients of the same email
- If any user clicked the link, force a password reset and monitor their account
- Add IOCs to threat intel blocklist
