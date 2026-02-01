# Bug Bounty Program Data

**Automated scraping of bug bounty program scopes from major platforms.**

**Last Updated:** Auto-updated every 30 minutes

---

## 📊 Platform Coverage

| Platform | Status | Programs | In-Scope Targets | Update Frequency |
|----------|--------|----------|------------------|------------------|
| [**Bugcrowd**](./bugcrowd/) | ✅ LIVE | ~45-50 active* | ~400+ | Every 30 min (:00, :30) |
| [**HackerOne**](./hackerone/) | ✅ LIVE | ~464 active* | 16,000+ | Every 30 min (:15, :45) |
| [**Intigriti**](./intigriti/) | ✅ LIVE | ~138* | TBD | Every 30 min (:08, :38) |
| **YesWeHack** | 🚧 TODO | ~50 | TBD | - |
| **Federacy** | 🚧 TODO | ~20 | TBD | - |

**\*Active programs only** - Scrapers automatically skip completed, paused, disabled, and suspended programs

**Total (current):** ~650+ active programs, 16,400+ targets across 3 platforms
**Total (when complete):** ~890+ programs, 18,000+ targets

---

## 🎯 Optimization: Active Programs Only

All scrapers filter out inactive programs to focus on actionable bug bounties:

- **Bugcrowd:** Skips "Completed" programs (~40-50% filtered)
- **HackerOne:** Skips "Paused" and "Disabled" programs (~126 filtered, 21%)
- **Intigriti:** Skips "Closed" and "Suspended" programs (percentage TBD)

This ensures you only get data for programs **currently accepting submissions**.

---

## 📁 Data Structure

### Bugcrowd

```
bugcrowd/
├── programs-latest.json    # Full program data with enhanced metadata
├── stats-latest.json       # Platform statistics
└── domains.txt             # All in-scope domains (deduplicated)
```

**Enhanced Metadata Includes:**
- Program status (Active/Completed), timeline data (started_at, completed_at)
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
  "status": "Active",
  "started_at": "2018-03-01T16:00:00Z",
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
- Submission state (open, paused, disabled) - **only "open" included**
- Reward tiers (by severity: critical, high, medium, low)
- Program metrics (response efficiency %, time to bounty, resolved reports)
- Scope descriptions (per-target testing instructions)
- Target details (max severity, bounty eligibility, testing notes)
- Auto-generated tags (Website, Mobile, Bounty Eligible, etc.)

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

### Intigriti

```
intigriti/
├── programs-latest.json    # Full program data with enhanced metadata
├── stats-latest.json       # Platform statistics
└── domains.txt             # All in-scope domains (deduplicated)
```

**Enhanced Metadata Includes:**
- Program status (Open, Closed, Suspended) - **only "Open" included**
- Confidentiality level (Public, Registered, Private)
- Program type (Bug Bounty vs Responsible Disclosure)
- Min/max bounty ranges (in EUR)
- Last updated timestamps
- Industry classification
- Target URLs and scope details

**Sample Program:**
```json
{
  "name": "Example Program",
  "handle": "example-program",
  "url": "https://app.intigriti.com/programs/example-program",
  "platform": "intigriti",
  "status": "Open",
  "confidentiality_level": "Public",
  "program_type": "Bug Bounty",
  "min_bounty": {"value": 50, "currency": "EUR"},
  "max_bounty": {"value": 5000, "currency": "EUR"},
  "last_updated_at": "2026-01-24T12:00:00Z",
  "industry": "Technology",
  "targets": {
    "in_scope": [
      {
        "type": "url",
        "endpoint": "https://example.com",
        "description": "Main application"
      }
    ]
  }
}
```

---

## 🔗 Quick Access URLs

**Raw JSON (for automation):**
- Bugcrowd: `https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/bugcrowd/programs-latest.json`
- HackerOne: `https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/hackerone/programs-latest.json`
- HackerOne Targets Only: `https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/hackerone/targets-latest.json`
- Intigriti: `https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/intigriti/programs-latest.json`

**Statistics:**
- Bugcrowd: `https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/bugcrowd/stats-latest.json`
- HackerOne: `https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/hackerone/stats-latest.json`
- Intigriti: `https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/intigriti/stats-latest.json`

---

## 📖 Usage Examples

### jq (Command Line)

```bash
# Get all Bugcrowd active programs with max payout > $5000
curl -s https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/bugcrowd/programs-latest.json \
  | jq '.[] | select(.status == "Active" and .max_payout > 5000) | {name, max_payout, url}'

# Get all HackerOne programs accepting submissions
curl -s https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/hackerone/programs-latest.json \
  | jq '.[] | select(.submission_state == "open") | {name, handle, offers_bounties}'

# Get all Intigriti public bug bounty programs
curl -s https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/intigriti/programs-latest.json \
  | jq '.[] | select(.status == "Open" and .program_type == "Bug Bounty") | {name, handle, max_bounty}'

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

# Filter programs with bounties and open submissions (already filtered)
active_bounties = [
    p for p in programs
    if p['offers_bounties']
]

print(f"Found {len(active_bounties)} active bug bounty programs")

# Extract all website targets
for program in active_bounties[:5]:  # First 5
    targets = [
        t['target'] for t in program['targets']['in_scope']
        if t['type'] == 'website' and t.get('eligible_for_bounty', True)
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

**Current Data (Updated: Automated every 30 minutes):**

**Bugcrowd:**
- Total Programs: ~45-50 active (completed programs filtered out)
- With Bounties: ~40
- Total In-Scope Targets: ~400+
- Avg Payout: $300-500
- Update Method: Playwright (browser automation)
- Update Duration: ~20 minutes (optimized from 40)

**HackerOne:**
- Total Programs: ~464 active (126 paused/disabled filtered out)
- With Bounties: ~297
- Total In-Scope Targets: 16,000+
- Avg Response Efficiency: 75-85%
- Update Method: REST API v1
- Update Duration: ~3 minutes

**Intigriti:**
- Total Programs: ~138 (closed/suspended filtered out)
- Program Types: Bug Bounty + Responsible Disclosure
- Confidentiality: Public, Registered, Private
- Update Method: Hybrid (Algolia API + Playwright)
- Update Duration: TBD (first run pending)

---

## 🛠️ How It Works

### Data Collection

**Bugcrowd (Playwright-based):**
1. Browser automation using Playwright Chromium
2. Navigate to each program's `/engagements/` page
3. **Check status first** - skip if "Completed"
4. Extract metadata from HTML (reward tiers, focus areas, scope, timeline)
5. Parse targets from structured HTML elements
6. Avoid bot detection with randomization + delays
7. Success rate: 100% of active programs (completed programs skipped)

**HackerOne (API-based):**
1. Fetch program directory via REST API v1
2. Query each program's structured scopes endpoint
3. Parse JSON responses into enhanced format
4. **Filter out paused/disabled programs** before saving
5. Extract metrics, severities, and instructions
6. Generate reward tiers from severity + bounty data
7. Success rate: 100% (API stability)

**Intigriti (Hybrid):**
1. Fetch program listings via Algolia search API
2. Extract basic metadata (status, confidentiality, bounties)
3. **Filter by status** - only scrape "Open" programs
4. Use Playwright for detailed scope extraction
5. Parse target URLs and scope descriptions
6. Combine Algolia + page-scraped data
7. Success rate: TBD (newly deployed)

### Update Schedule

- **Bugcrowd:** Every 30 minutes (:00 and :30)
- **HackerOne:** Every 30 minutes (:15 and :45, offset)
- **Intigriti:** Every 30 minutes (:08 and :38, offset)
- **Deployment:** Hostinger VPS with cron automation
- **Publishing:** Automatic git commit + push on successful scrape

### Infrastructure & Backup

**Primary Repository Location:**
- **VPS Host:** Hostinger (srv945980.hstgr.cloud)
- **Working Directory:** `~/ia-framework-targets-data/` on VPS
- **Scrapers Location:** `~/bounty-scraper/` on VPS

**Automated Backup Strategy:**
- ✅ Auto-commit after every successful scrape (3x per hour per platform)
- ✅ Auto-push to GitHub immediately after commit
- ✅ GitHub serves as continuous backup with full git history
- ✅ 30-minute update cycle ensures data is never stale

**Data Recovery:**
- Clone from GitHub to restore to any location
- Full git history preserved (can recover any previous scrape)
- Scraper code backed up in private framework repository

---

## ⚠️ Important Notes

### Usage Terms

1. **Public Data Only:** This repository contains only publicly accessible program information
2. **No Authentication:** No API keys or authenticated endpoints are scraped (except public Algolia keys)
3. **Respectful Scraping:** Rate limits are respected, delays are built-in
4. **Platform Terms:** Use of this data must comply with each platform's terms of service

### Data Accuracy

- **Freshness:** Data is typically <30 minutes old
- **Filtering:** Only active/open programs (completed/paused/closed programs excluded)
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
- ❌ Test programs marked as "paused," "disabled," "completed," or "closed"
- ❌ Test out-of-scope assets
- ❌ Exceed rate limits or cause denial of service
- ❌ Access production data without explicit authorization
- ❌ Test third-party services without separate authorization

---

## 🔄 Changelog

### 2026-01-24: Intigriti Platform + Status Filtering

- ✅ Added Intigriti platform (~138 programs)
- ✅ Hybrid scraper (Algolia API + Playwright)
- ✅ **Optimization:** All scrapers now skip inactive programs
  - Bugcrowd: Filters "Completed" (~40-50% reduction)
  - HackerOne: Filters "Paused" and "Disabled" (126 programs, 21%)
  - Intigriti: Filters "Closed" and "Suspended"
- ✅ Added status and timeline extraction for Bugcrowd
- ✅ ~2x speed improvement for Bugcrowd scraper

### 2026-01-24: HackerOne Enhanced Scraper

- ✅ Added HackerOne platform (590 total, ~464 active programs, 16,000+ targets)
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

- ✅ Bugcrowd platform scraper (92 total programs, ~650 targets)
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
- **Differences:** Enhanced metadata extraction, 1:1 program page data, status filtering, program metrics

---

**Maintained by:** @notchrisgroves
**Infrastructure:** Hostinger VPS + GitHub Actions
**Framework:** [Intelligence Adjacent (IA)](https://github.com/notchrisgroves/ia-framework-public)
