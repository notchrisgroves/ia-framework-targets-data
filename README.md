# Bug Bounty Targets Data

**Automated collection of public bug bounty program targets from major platforms**

> **⚠️ For Security Research Only**
> This data is provided for legitimate security research and responsible disclosure. Always respect program scope and rules of engagement.

---

## 📊 Data Coverage

| Platform | Programs | Last Updated | Status |
|----------|----------|--------------|--------|
| **Bugcrowd** | ~92 | 2026-01-23 | ✅ Active |
| **HackerOne** | TBD | Coming Soon | 🚧 Planned |
| **Intigriti** | TBD | Coming Soon | 🚧 Planned |
| **YesWeHack** | TBD | Coming Soon | 🚧 Planned |
| **Federacy** | TBD | Coming Soon | 🚧 Planned |

---

## 🗂️ Data Structure

```
ia-framework-targets-data/
├── bugcrowd/
│   ├── programs-latest.json          # Latest full scrape
│   └── archive/
│       └── YYYY-MM-DD-HHMM.json     # Historical snapshots
├── hackerone/                         # Coming soon
├── intigriti/                         # Coming soon
├── yeswehack/                         # Coming soon
└── federacy/                          # Coming soon
```

---

## 📋 JSON Format

Each platform exports data in a consistent format:

```json
[
  {
    "name": "Program Name",
    "url": "https://platform.com/program-slug",
    "platform": "bugcrowd",
    "safe_harbor": "full" | "partial" | null,
    "max_payout": 10000,
    "allows_disclosure": true,
    "targets": {
      "in_scope": [
        {
          "type": "website" | "api" | "android" | "ios" | "other",
          "target": "*.example.com",
          "uri": "https://example.com"
        }
      ],
      "out_of_scope": [
        {
          "type": "website",
          "target": "staging.example.com",
          "uri": ""
        }
      ]
    }
  }
]
```

**Field Descriptions:**
- `name`: Official program name
- `url`: Platform-specific program URL
- `platform`: Source platform (bugcrowd, hackerone, etc.)
- `safe_harbor`: Legal protection level
  - `"full"`: Full safe harbor protection
  - `"partial"`: Limited safe harbor
  - `null`: No safe harbor or unclear
- `max_payout`: Maximum bounty amount in USD (null if not disclosed)
- `allows_disclosure`: Whether public disclosure is permitted
- `targets.in_scope`: Assets authorized for testing
- `targets.out_of_scope`: Assets explicitly forbidden

---

## 🚀 Usage

### Quick Start

```bash
# Get latest Bugcrowd targets
curl -sL https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/bugcrowd/programs-latest.json | jq

# Count active programs with targets
jq '[.[] | select(.targets.in_scope | length > 0)] | length' bugcrowd/programs-latest.json

# Find programs with full safe harbor
jq '.[] | select(.safe_harbor == "full") | .name' bugcrowd/programs-latest.json

# Extract all in-scope domains
jq -r '.[] | .targets.in_scope[] | select(.type == "website") | .target' bugcrowd/programs-latest.json
```

### Example: Filter by Max Payout

```bash
# Find high-value programs ($10k+)
jq '.[] | select(.max_payout >= 10000) | {name, max_payout, url}' bugcrowd/programs-latest.json
```

### Example: Get All API Targets

```bash
# Extract API endpoints from all programs
jq -r '.[] | .targets.in_scope[] | select(.type == "api") | .uri' bugcrowd/programs-latest.json
```

---

## 🔄 Update Frequency

- **Bugcrowd**: Updated every 30 minutes (automated via VPS scraper)
- **Other platforms**: Coming soon

Data is automatically scraped from public program pages and committed to this repository. Historical snapshots are archived for tracking changes over time.

---

## 🛡️ Responsible Disclosure

**Important Guidelines:**

1. **Respect Program Scope**: Always verify in-scope targets before testing
2. **Check Out-of-Scope**: Never test targets listed in `out_of_scope`
3. **Read Program Rules**: Each program has specific rules - read them on the platform
4. **Safe Harbor**: Check `safe_harbor` field before testing
5. **Report Responsibly**: Follow the program's disclosure process

**This data is NOT:**
- Authorization to test targets
- A substitute for reading program policies
- Legal advice or protection

**Always:**
- Verify current program status on the official platform
- Obtain proper authorization before testing
- Follow responsible disclosure practices

---

## 📜 Data Sources

All data is scraped from publicly available bug bounty program pages:

- **Bugcrowd**: https://bugcrowd.com/programs
- **HackerOne**: https://hackerone.com/directory/programs
- **Intigriti**: https://www.intigriti.com/programs
- **YesWeHack**: https://www.yeswehack.com/programs
- **Federacy**: https://www.federacy.com/programs

No authentication or private data is accessed. Only public program information is collected.

---

## 🔧 Technical Details

### Scraper Architecture

- **Engine**: Playwright (headless Chromium)
- **Language**: TypeScript/Bun
- **Rate Limiting**: Conservative delays (10s between requests)
- **Proxy Support**: Optional (currently disabled)
- **Deployment**: Hostinger VPS with automated cron

### Data Quality

- **Validation**: Each scrape is compared against reference data
- **Error Handling**: Retry logic with exponential backoff
- **Quality Metrics**: Tracked per-field accuracy
- **Target Quality**: ~80%+ accuracy on active programs

### Known Limitations

- Bugcrowd programs with unusual formatting may have missing fields
- Safe harbor detection relies on specific text patterns
- Max payout may be null if not displayed on program page
- Some programs may be private or require platform login

---

## 🤝 Contributing

Found an issue with the data? Contributions welcome!

1. **Data Issues**: Open an issue describing the inaccuracy
2. **Scraper Improvements**: PRs welcome for better selectors/parsing
3. **New Platforms**: Help add HackerOne, Intigriti, etc.

---

## 📄 License

**MIT License**

This data aggregation project is open source. The underlying bug bounty program information is owned by the respective platforms and program operators.

**Usage of this data implies acceptance of:**
- Responsible security research practices
- Respect for program rules and scope
- No liability for misuse or inaccuracies

---

## 🔗 Related Projects

- **Intelligence Adjacent (IA) Framework**: https://github.com/notchrisgroves/ia-framework

---

## 📞 Contact

- **Maintainer**: @notchrisgroves
- **Issues**: https://github.com/notchrisgroves/ia-framework-targets-data/issues
- **Security**: For security issues with the scraper itself, email notchrisgroves@gmail.com

---

**Last Updated**: 2026-01-23
**Repository Version**: 1.0.0
**Data Version**: Bugcrowd only (v2 scraper)

---

**⚡ Built with ▲ Intelligence Adjacent Framework**
