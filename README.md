# Bug Bounty Targets Data

Continuously updated database of bug bounty programs from major platforms. Data is automatically scraped twice daily and published to GitHub.

**Note:** This is a public deployment of the bounty scraper tool from the [Intelligence Adjacent (IA) Framework](https://github.com/anthropics/ia-framework). You can download and deploy your own instance with custom configurations.

## 📊 Current Statistics

| Platform | Programs | In-Scope Targets | Update Times (UTC) |
|----------|----------|------------------|------------------|
| **Bugcrowd** | ~45-50 | ~400+ | 8:00, 20:00 |
| **HackerOne** | ~464 | 16,000+ | 9:00, 21:00 |
| **Intigriti** | ~138 | TBD | 8:30, 20:30 |
| **YesWeHack** | ~93 | ~842 | 8:15, 20:15 |
| **Federacy** | ~34 | ~100 | 8:45, 20:45 |

**Total**: ~770 programs, 16,000+ in-scope targets

## 🔄 Update Schedule

Data is scraped **twice daily** in two windows, with each platform running at a specific time to prevent resource exhaustion:

**Morning Window (UTC):**
- 8:00 - Bugcrowd
- 8:15 - YesWeHack
- 8:30 - Intigriti
- 8:45 - Federacy
- 9:00 - HackerOne
- 9:30 - Data published to GitHub

**Evening Window (UTC):**
- 20:00 - Bugcrowd
- 20:15 - YesWeHack
- 20:30 - Intigriti
- 20:45 - Federacy
- 21:00 - HackerOne
- 21:30 - Data published to GitHub

## 📁 Data Format

**JSON Files**: Complete program data with metadata (reward tiers, metrics, scope details)
- `bugcrowd_data.json`
- `hackerone_data.json`
- `intigriti_data.json`
- `yeswehack_data.json`
- `federacy_data.json`

**TXT Files**: Domain-only extracts for quick reference
- `*_targets.txt` (one per platform)

## 🔗 Quick Access

```bash
# Download HackerOne data
curl -O https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/data/hackerone_data.json

# Download Bugcrowd targets
curl -O https://raw.githubusercontent.com/notchrisgroves/ia-framework-targets-data/main/data/bugcrowd_targets.txt
```

## 📝 Data Format

Each JSON file contains programs with:
- `name` - Program name
- `url` - Direct link to program page
- `targets.in_scope[]` - Array of in-scope targets
- `targets.out_of_scope[]` - Array of out-of-scope targets
- `last_updated` - ISO 8601 timestamp of last scrape

## ⚠️ Important Notes

- **Accuracy**: Data is scraped automatically and may contain errors or be outdated
- **Verification**: Always verify program details on the official platform before testing
- **Authorization**: Only test targets you have explicit permission to test
- **Legal**: Unauthorized testing is illegal - always follow responsible disclosure

## 📞 Support

- **Source Code**: [Intelligence Adjacent (IA) Framework](https://github.com/anthropics/ia-framework)
- **Deployment**: Fork this repo or deploy from IA Framework tools
- **Issues**: [GitHub Issues](https://github.com/notchrisgroves/ia-framework-targets-data/issues)

---

**Last Updated**: Twice daily (morning and evening UTC)  
**License**: MIT
