# Bug Bounty Program Data

**Automated scraping of bug bounty program scopes from major platforms.**

**Last Updated:** Auto-updated every 30 minutes
**Source:** https://github.com/notchrisgroves/ia-framework-private (private)

---

## 📊 Platform Coverage

| Platform | Status | Programs | In-Scope Targets | Update Frequency |
|----------|--------|----------|------------------|------------------|
| [**Bugcrowd**](./bugcrowd/) | ✅ LIVE | 92 | ~650 | Every 30 min |
| [**HackerOne**](./hackerone/) | ✅ LIVE | 590 | 16,875+ | Every 30 min |
| **Intigriti** | 🚧 TODO | ~100 | TBD | - |
| **YesWeHack** | 🚧 TODO | ~50 | TBD | - |
| **Federacy** | 🚧 TODO | ~20 | TBD | - |

**Total (when complete):** 850+ programs, 18,000+ targets

---

## 📁 Data Structure

### Bugcrowd

```
bugcrowd/
├── programs-latest.json    # Full program data with enhanced metadata (~5 MB)
├── stats-latest.json       # Platform statistics
└── domains.txt             # All in-scope domains (deduplicated)
```

**Enhanced Metadata Includes:**
- Reward tiers (P1-P4 with min/max dollar amounts)
- Credentials (test account instructions)
- Focus areas (XSS, CSRF, SQLi, RCE, etc.)
- Scope descriptions (in-scope guidelines + out-of-scope vulnerabilities)
- Target tags (GraphQL, Javascript, iOS, Android)
- Program metrics (vulnerabilities rewarded, validation time, avg payout)

**Sample Program:**
```json
{
  "name": "Acorns",
  "url": "https://bugcrowd.com/engagements/acorns",
  "platform": "bugcrowd",
  "max_payout": 4000,
  "reward_tiers": [
    {"priority": "P1", "min": 2000, "max": 4000},
    {"priority": "P2", "min": 750, "max": 1750}
  ],
  "credentials": "Self Signup - use @bugcrowdninja.com",
  "focus_areas": ["XSS", "CSRF", "SQLi", "RCE"],
  "targets": {
    "in_scope": [
      {
        "type": "website",
        "target": "*.acorns.com/",
        "tags": ["Website Testing", "Javascript"],
        "known_issues": 0
      }
    ]
  },
  "program_metrics": {
    "vulnerabilities_rewarded": 209,
    "validation_within_days": 6,
    "average_payout": 383
  }
}
```

### HackerOne

```
hackerone/
├── programs-latest.json    # Full program data with enhanced metadata (~10 MB)
├── targets-latest.json     # Targets-only (quick lookup, ~3 MB)
├── stats-latest.json       # Platform statistics
├── domains.txt             # All in-scope domains (deduplicated)
└── wildcards.txt           # Wildcard domains
```

**Enhanced Metadata Includes:**
- Reward tiers (by severity: critical, high, medium, low)
- Program metrics (response efficiency %, time to bounty, resolved reports)
- Scope descriptions (per-target testing instructions)
- Target details (max severity, bounty eligibility, testing notes)
- Auto-generated tags (Website, Mobile, Bounty Eligible, etc.)
- Submission state (open, paused, disabled)

**Sample Program:**
```json
{
  "name": "Cloudflare Public Bug Bounty",
  "handle": "cloudflare",
  "url": "https://hackerone.com/cloudflare",
  "platform": "hackerone",
  "offers_bounties": true,
  "submission_state": "open",
  "reward_tiers": [
    {"severity": "critical", "min": 5000, "max": 10000},
    {"severity": "high", "min": 2000, "max": 5000}
  ],
  "scope_descriptions": {
    "in_scope": [
      "All attacks must be executed against your own Cloudflare Account",
      "Accounts should be created with a @wearehackerone.com email address"
    ]
  },
  "targets": {
    "in_scope": [
      {
        "type": "other",
        "target": "Vectorize",
        "uri": "https://developers.cloudflare.com/vectorize/",
        "max_severity": "critical",
        "eligible_for_bounty": true,
        "instruction": "https://developers.cloudflare.com/vectorize/",
        "tags": ["Bounty Eligible", "Max: Critical"]
      }
    ]
  },
  "program_metrics": {
    "response_efficiency_percentage": 85,
    "average_time_to_bounty_awarded": 24,
    "average_time_to_first_response": 4,
    "resolved_report_count": 150
  }
}
```

---

## 🔗 Quick Access URLs

**Raw JSON (for automation):**
- Bugcrowd: `https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/bugcrowd/programs-latest.json`
- HackerOne: `https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/hackerone/programs-latest.json`
- HackerOne Targets Only: `https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/hackerone/targets-latest.json`

**Statistics:**
- Bugcrowd: `https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/bugcrowd/stats-latest.json`
- HackerOne: `https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/hackerone/stats-latest.json`

---

## 📖 Usage Examples

### jq (Command Line)

```bash
# Get all Bugcrowd programs with max payout > $5000
curl -s https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/bugcrowd/programs-latest.json \
  | jq '.[] | select(.max_payout > 5000) | {name, max_payout, url}'

# Get all HackerOne programs accepting submissions
curl -s https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/hackerone/programs-latest.json \
  | jq '.[] | select(.submission_state == "open") | {name, handle, offers_bounties}'

# Extract all in-scope domains from HackerOne
curl -s https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/hackerone/programs-latest.json \
  | jq -r '.[] | .targets.in_scope[] | select(.type == "website") | .target'

# Find programs with specific focus areas
curl -s https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/bugcrowd/programs-latest.json \
  | jq '.[] | select(.focus_areas | contains(["XSS"])) | {name, focus_areas}'
```

### Python

```python
import requests

# Fetch all HackerOne programs
response = requests.get(
    'https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/hackerone/programs-latest.json'
)
programs = response.json()

# Filter programs with bounties and open submissions
active_bounties = [
    p for p in programs
    if p['offers_bounties'] and p['submission_state'] == 'open'
]

print(f"Found {len(active_bounties)} active bug bounty programs")

# Extract all website targets
for program in active_bounties[:5]:  # First 5
    targets = [
        t['target'] for t in program['targets']['in_scope']
        if t['type'] == 'website' and t['eligible_for_bounty']
    ]
    print(f"{program['name']}: {len(targets)} bounty-eligible targets")
```

### JavaScript/Node.js

```javascript
// Fetch HackerOne programs
const response = await fetch(
  'https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/hackerone/programs-latest.json'
);
const programs = await response.json();

// Find programs with critical severity bounties
const criticalPrograms = programs.filter(p =>
  p.reward_tiers.some(tier => tier.severity === 'critical')
);

console.log(`${criticalPrograms.length} programs accept critical severity reports`);
```

---

## 📈 Statistics

**Current Data (Updated: Automated):**

**Bugcrowd:**
- Total Programs: 92
- With Bounties: ~80
- Total In-Scope Targets: ~650
- Avg Payout: $300-500
- Update Method: Playwright (browser automation)
- Update Duration: ~30-40 minutes

**HackerOne:**
- Total Programs: 590
- With Bounties: ~400
- Total In-Scope Targets: 16,875+
- Avg Response Efficiency: 75-85%
- Update Method: REST API v1
- Update Duration: ~3 minutes

---

## 🛠️ How It Works

### Data Collection

**Bugcrowd (Playwright-based):**
1. Browser automation using Playwright Chromium
2. Navigate to each program's `/engagements/` page
3. Extract metadata from HTML (reward tiers, focus areas, scope)
4. Parse targets from structured HTML elements
5. Avoid bot detection with randomization + delays
6. Success rate: 60-85% per run (incremental coverage)

**HackerOne (API-based):**
1. Fetch program directory via REST API v1
2. Query each program's structured scopes endpoint
3. Parse JSON responses into enhanced format
4. Extract metrics, severities, and instructions
5. Generate reward tiers from severity + bounty data
6. Success rate: 100% (API stability)

### Update Schedule

- **Bugcrowd:** Every 30 minutes (:00 and :30)
- **HackerOne:** Every 30 minutes (:15 and :45, offset to avoid overlap)
- **Deployment:** Hostinger VPS with cron automation
- **Publishing:** Automatic git commit + push on successful scrape

---

## ⚠️ Important Notes

### Usage Terms

1. **Public Data Only:** This repository contains only publicly accessible program information
2. **No Authentication:** No API keys or authenticated endpoints are scraped
3. **Respectful Scraping:** Rate limits are respected, delays are built-in
4. **Platform Terms:** Use of this data must comply with each platform's terms of service

### Data Accuracy

- **Freshness:** Data is typically <30 minutes old
- **Completeness:** Bugcrowd coverage is incremental (60-85% per run), HackerOne is complete
- **Verification:** Always verify program details on the official platform before testing
- **Changes:** Programs may close, modify scope, or update rules between scrapes

### Bug Bounty Ethics

**Before testing any program:**
1. ✅ Read the complete program policy on the platform
2. ✅ Verify the scope is current (programs update frequently)
3. ✅ Understand out-of-scope items and prohibited techniques
4. ✅ Create test accounts with approved email domains (e.g., @bugcrowdninja.com)
5. ✅ Configure required HTTP headers (X-Bugcrowd-Researcher, etc.)
6. ✅ Only test assets explicitly marked as in-scope

**Never:**
- ❌ Test programs marked as "paused" or "disabled"
- ❌ Test out-of-scope assets
- ❌ Exceed rate limits or cause denial of service
- ❌ Access production data without explicit authorization
- ❌ Test third-party services without separate authorization

---

## 🔄 Changelog

### 2026-01-24: HackerOne Enhanced Scraper

- ✅ Added HackerOne platform (590 programs, 16,875+ targets)
- ✅ API-based scraper (REST API v1)
- ✅ Enhanced metadata: reward tiers, program metrics, scope descriptions
- ✅ 100% success rate, ~3 minute update cycles
- ✅ 20-30x faster than browser-based approach

### 2026-01-23: Bugcrowd Enhanced Metadata

- ✅ Enhanced extraction: reward tiers, focus areas, credentials
- ✅ Scope descriptions: in-scope guidelines + out-of-scope vulnerabilities
- ✅ Target tags: GraphQL, Javascript, iOS, Android
- ✅ Program metrics: vulnerabilities rewarded, validation time, avg payout
- ✅ 40x improvement in metadata extraction (1-4% → 55%)

### 2026-01-22: Bugcrowd Initial Launch

- ✅ Bugcrowd platform scraper (92 programs, ~650 targets)
- ✅ Playwright-based browser automation
- ✅ Bot detection bypass with stealth measures
- ✅ Automated VPS deployment with cron

---

## 📜 License

**Data:** Aggregated from publicly accessible sources
**Code:** Private repository (not included)
**Usage:** Free for security research and bug bounty hunting

---

## 🤝 Contributing

This is an automated data repository. The scraping infrastructure is private.

**Found an issue?**
- Report inaccuracies or missing programs via GitHub Issues
- Note: Data is automatically regenerated every 30 minutes

---

## 🔗 Related Projects

- Original inspiration: [arkadiyt/bounty-targets](https://github.com/arkadiyt/bounty-targets)
- **Differences:** Enhanced metadata extraction, 1:1 program page data, program metrics

---

**Maintained by:** @notchrisgroves
**Infrastructure:** Hostinger VPS + GitHub Actions
**Framework:** [Intelligence Adjacent (IA)](https://github.com/notchrisgroves/ia-framework-public)
