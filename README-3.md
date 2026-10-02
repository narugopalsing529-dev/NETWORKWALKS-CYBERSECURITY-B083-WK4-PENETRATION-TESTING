# Week 4: Penetration Testing — Mediroza General Hospital

Networkwalks Cybersecurity & Ethical Hacking Internship, Batch B083

A black-box penetration test against a training target, run over 5 days across four milestones: get in, get the data out, find the bigger exposure it leads to, then write it all up as a real client-facing report.

## Engagement details

| Field | Detail |
|---|---|
| Client | Mediroza General Hospital |
| Target | https://medirozahospital.com |
| Type | Black-box penetration test |
| Duration | 5 days |
| Scope | Full black-box pentest — identify vulnerabilities, exploit them to demonstrate real impact, document all findings in a professional report |
| Rules | Testing limited to the target domain only. No social engineering. No denial of service. No testing outside agreed scope |
| Authorization | Written authorization provided for this engagement |

Working independently on this one and not comparing notes with anyone else in the batch until the reveal session, per the brief.

## Milestones

### M1 — Initial Access
**Goal:** attack the website and find the 3 confidential PDF lab reports of patients.
Starting point is recon, then mapping out entry points and how the site's auth behaves, then looking at how it handles input.
**Deliverable:** proof of access + the 3 retrieved PDF files.

### M2 — Data Extraction
**Goal:** crack the encryption on all 3 retrieved files.
Brief specifically warns not to assume one approach works for all three, so each file gets analyzed on its own before picking a tool/wordlist for it.
**Deliverable:** recovered contents of all 3 files + proof of access.

### M3 — Data Exposure
**Goal:** find the critical data exposure on the client server.
Tasks: find the salaries of all hospital employees, and find the shareholder details of the hospital.
Hint points at digging past the obvious file contents (metadata/properties included) — one finding here is supposed to lead to the bigger exposure.
**Deliverable:** documented evidence of the exposure + a readable summary of what was found.

### M4 — Pentest Report
**Goal:** write the formal penetration testing report for the client.
Required structure: Executive Summary → Scope & Methodology → Findings & Proof of Exploitation (screenshots/evidence per milestone) → Risk Rating (Critical/High/Medium/Low, justified) → Recommendations & Remediation.
**Deliverable:** complete professional report submitted to the instructor.

## Methodology

The brief only gives hints on purpose — this one's meant to be worked independently rather than followed like a script. Below is the general approach for each milestone: the standard process and vocabulary, not a confirmed path into this specific target. The actual findings still have to come from testing the site myself.

**M1 — Initial Access**
Standard black-box web app flow:
1. **Recon** — same tools as Week 2 (whois, nslookup/dnsrecon, whatweb) plus a manual pass over the site itself: view-source, `robots.txt`, `sitemap.xml`, any JS files for endpoints that aren't linked from the UI.
2. **Enumeration** — map every page, form, and parameter. Directory/content discovery tools (gobuster, ffuf, dirb) are the standard way to find pages that aren't linked anywhere, like a hidden staff or patient portal.
3. **Authentication review** — how login, session, and password-reset behave. Weak or predictable auth is one of the most common ways into a "restricted area."
4. **Input handling** — standard OWASP-category checks (injection flaws, broken access control, insecure file upload/download, IDOR) against whatever forms and parameters turned up in enumeration. Burp Suite / OWASP ZAP are the usual tools for intercepting and testing this by hand rather than guessing blind.

**M2 — Data Extraction**
Same two-stage process as Week 3 (extract hash → dictionary/brute-force attack), just run independently against each of the 3 files rather than assumed to be identical. If a file doesn't respond to the same approach, that's the hint to try a different wordlist, a slower brute-force, or check whether it's protected a different way entirely (not every "locked" file is a simple password hash).

**M3 — Data Exposure**
"Examine all file properties carefully" is a pointer toward document metadata — author, software used to create it, revision history, embedded comments — which sometimes leaks more than the visible page content does (an internal filename, a path, a name that leads somewhere else). Worth checking each PDF's properties/metadata, not just what's printed on the page.

**M4 — Report**
Structure is already fixed by the brief (Exec Summary → Scope & Methodology → Findings & Proof → Risk Rating → Recommendations). This section gets written last, once M1–M3 actually have real evidence behind them.

## Status

- [ ] M1 — Initial Access
- [ ] M2 — Data Extraction
- [ ] M3 — Data Exposure
- [ ] M4 — Pentest Report

## Repo layout

```
.
├── M1-initial-access/
│   └── screenshots/
├── M2-data-extraction/
│   └── screenshots/
├── M3-data-exposure/
│   └── screenshots/
├── M4-report/
│   └── pentest-report.docx
└── README.md
```

## Disclaimer

This engagement is conducted in a controlled environment for educational purposes only, against a target Networkwalks has authorised for testing as part of this internship. These techniques must never be applied to any system without explicit written permission from the owner.
