# Brand AI Readiness Audit
### Adobe University Hackathon 2026

**An Agent Skill Marketplace for auditing website readiness for AI-powered discovery.**

Brand AI Readiness Audit analyzes public websites to identify why AI assistants may struggle to discover, access, understand, retrieve, and cite their content.

It generates an evidence-backed report containing identified issues, their severity, and prioritized recommendations to improve AI discoverability.

---

## 🚀 Project Overview

Traditional search engine indexing does not necessarily mean that AI assistants can understand or reference a website.

Important information may be:
- Hidden inside images or PDFs.
- Blocked by `robots.txt` or crawler restrictions.
- Available only through JavaScript rendering.
- Inconsistent across different pages.
- Missing structured data or clear brand identity.
- Difficult for AI systems to locate and quote accurately.

**Brand AI Readiness Audit** identifies these problems through nine specialized Agent Skills and generates a comprehensive technical audit report.

## ✨ Key Features

- **9 Specialized Agent Skills:** Each skill focuses on a specific stage of the AI discovery process.
- **Evidence-Based Findings:** Every reported issue must include a concrete URL and observed evidence.
- **Read-Only Auditing:** Uses GET/HEAD requests only. Never modifies the audited website.
- **Intelligent Validation:** Avoids reporting valid configurations, non-English websites, and intentional crawler restrictions as defects.
- **Coverage Transparency:** Clearly identifies checks that could not be performed instead of assuming they passed or failed.
- **No External Dependencies:** Uses Python 3.9+ standard library without requiring API keys or LLM calls.

## 🔍 How It Works

The project follows an eight-stage discovery funnel:

```text
Exists
  ↓
Discovered
  ↓
Accessible
  ↓
Readable
  ↓
Understood
  ↓
Retrieved
  ↓
Selected
  ↓
Cited
```

Each stage represents a different requirement for AI-powered website discovery.

### System Architecture

```text
              PUBLIC WEBSITE
                    |
                    v
          Site Snapshot Collector
                    |
                    v
              snapshot.json
                    |
          +---------+---------+
          |         |         |
          v         v         v
       Crawl     Render    Structured
       Access   Readability   Data
          |         |         |
          +---------+---------+
                    |
          +---------+---------+
          |         |         |
          v         v         v
       Answer    Freshness  Engagement
      Retrieval  & Corrob.    Audit
          |         |         |
          +---------+---------+
                    |
                    v
       Opportunity & Monitoring
                    |
                    v
          Merge and Deduplicate
                    |
                    v
         Severity & Prioritization
                    |
                    v
             Final Reports
             report.md
             report.json
```

The Site Snapshot Collector is the only component that accesses the network. All other skills analyze the same frozen evidence snapshot.

## 🧩 Agent Skills

The marketplace contains nine Agent Skills, including one entrypoint and eight supporting skills.

| Agent Skill | Responsibility |
|---|---|
| `brand-ai-readiness-audit` | Main entrypoint that coordinates the complete audit. |
| `site-snapshot-collector` | Collects website pages, robots.txt, sitemaps, HTML, and rendered DOM. |
| `crawl-access-audit` | Checks crawler permissions, indexing directives, redirects, and canonicals. |
| `render-readability-audit` | Identifies content hidden behind JavaScript, images, PDFs, and interactions. |
| `structured-data-entity-audit` | Validates structured data, organization identity, and entity information. |
| `answer-retrievability-audit` | Evaluates whether AI systems can locate and extract specific answers. |
| `freshness-corroboration-audit` | Detects outdated information, contradictions, and inconsistent data. |
| `engagement-audit` | Examines navigation, dead ends, and missing user action paths. |
| `opportunity-and-monitoring` | Identifies combined improvement opportunities and monitoring strategies. |

Findings are evaluated using:

`Severity = Gate Impact × Reach × Certainty`

Additional caps prevent low-confidence observations from becoming exaggerated findings.

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| Python 3.9+ | Core implementation |
| HTML Parsing | Website content extraction |
| JSON | Evidence snapshots and structured reports |
| Markdown | Human-readable audit reports |
| Playwright (Optional) | JavaScript-rendered DOM analysis |
| Python Standard Library | Core execution without external dependencies |

## ⚙️ Getting Started

### Prerequisites

- Python 3.9 or later.
- Git.
- Optional: Playwright and Chromium for rendered-DOM analysis.

### 1. Clone the Repository

```bash
git clone https://github.com/psp2535/AdobeUniversityHackathon26.git
```

### 2. Navigate to the Project

```bash
cd AdobeUniversityHackathon26/brand-ai-readiness-audit
```

### 3. Run an Audit

```bash
python audit.py https://example.com
```

Replace `https://example.com` with the public website you want to audit.

### 4. Optional: Enable Browser Rendering

To analyze JavaScript-rendered content, install Playwright:

```bash
pip install playwright
playwright install chromium
```

Without Playwright, the audit still runs using raw HTML and reports the rendering checks that could not be assessed.

## 📊 Audit Results

After execution, the project generates three files inside `audit-output/`.

| Output File | Description |
|---|---|
| `report.md` | Human-readable audit report with findings and recommendations. |
| `report.json` | Structured, schema-validated audit results. |
| `snapshot.json` | Raw evidence collected from the website. |

The Markdown report includes:

- Identified issues and their severity.
- Example URLs and supporting evidence.
- Recommended corrective actions.
- Priority and estimated implementation effort.
- Suggested ownership.
- Verification steps to confirm improvements.

### Sample Audit Results

Example results from the checked-in Books to Scrape audit:

| Severity | Findings |
|---|---:|
| Critical | 0 |
| High | 2 |
| Medium | 6 |
| Low | 2 |

**Top recommended improvements:**

1. Publish a `robots.txt` file with an explicit crawler policy and sitemap reference.
2. Provide a sitemap containing accurate `lastmod` values.
3. Add an Organization JSON-LD node to the homepage with a stable identifier.

## 🤖 Running Through an AI Agent

The project can also be executed through an AI agent harness such as Claude Code.

Start the agent inside the `brand-ai-readiness-audit/` directory and provide the following instruction:

```text
Read skills/brand-ai-readiness-audit/SKILL.md
and follow it as the marketplace entrypoint.

Audit https://example.com and write the report
to ./audit-output.

Do not invent findings. Anything you cannot
verify must be listed under coverage.not_assessed.
```

The `audit.py` CLI executes the same sequence of scripts defined by the entrypoint skill.

## 🧪 Testing

The project includes 35 website fixtures and an automated test harness.

Run the marketplace validation:

```bash
python tools/validate_marketplace.py --root . --strict
```

Run the test suite:

```bash
python tests/run_tests.py
```

The tests cover:

- Scoring and severity calculations.
- Report schema validation.
- Deterministic execution.
- Website discovery and crawling.
- Negative controls for valid website configurations.

## 📁 Project Structure

```text
AdobeUniversityHackathon26/
│
├── brand-ai-readiness-audit/
│   │
│   ├── marketplace.json
│   ├── audit.py
│   │
│   ├── skills/
│   │   ├── brand-ai-readiness-audit/
│   │   ├── site-snapshot-collector/
│   │   ├── crawl-access-audit/
│   │   ├── render-readability-audit/
│   │   ├── structured-data-entity-audit/
│   │   ├── answer-retrievability-audit/
│   │   ├── freshness-corroboration-audit/
│   │   ├── engagement-audit/
│   │   └── opportunity-and-monitoring/
│   │
│   ├── tests/
│   │   ├── fixtures/
│   │   ├── fixture_server.py
│   │   └── run_tests.py
│   │
│   └── tools/
│       ├── validate_marketplace.py
│       └── run_audit_with_summary.py
│
├── dry-runs/
│   └── books-toscrape/
│       └── report.md
│
└── Backend-Architecture.pdf
```

## 🔒 Design Principles

1. **Evidence or Silence:** No concrete URL and observed value means no finding.
2. **Read-Only Execution:** The audited website is never modified.
3. **Capability-Based Validation:** Unavailable checks are marked as not assessed.
4. **No Fabricated Results:** Findings must be supported by collected evidence.
5. **Honest Recommendations:** Improving AI discoverability does not guarantee that an AI assistant will cite a website.
6. **No Commercial Bias:** Recommendations focus entirely on technical and editorial improvements.

## ⚠️ Limitations

- A maximum of 40 pages are sampled per audit, so large websites are not exhaustively analyzed.
- Rendered-DOM checks require Playwright and Chromium.
- Freshness checks compare information within the website itself and do not perform external corroboration.
- Some checks depend on supported language detection.

## 👥 Team

Built by a team of three for the **Adobe University Hackathon 2026**.

| Team Member | GitHub | LinkedIn |
|---|---|---|
| Pathange Sai Pranav | [@psp2535](https://github.com/psp2535) | [LinkedIn](https://www.linkedin.com/in/pathangesaipranav/) |
| Chakali Surendra | [@suri5790](https://github.com/suri5790) | [LinkedIn](https://www.linkedin.com/in/surendrachakali/) |
| Duppala Satwik | [@Satwik2529](https://github.com/Satwik2529) | [LinkedIn](https://www.linkedin.com/in/satwik2529dsk/) |
| Gedela Sasidhar Chakrapani | [@Sasidhar1906](https://github.com/Sasidhar1906) | [LinkedIn](https://www.linkedin.com/in/sasidhar1906/) |

## 📄 License

This project is licensed under the **Apache License 2.0**.

See [`brand-ai-readiness-audit/LICENSE`](brand-ai-readiness-audit/LICENSE) for details.

---

**Built for a future where websites need to be discoverable not just by search engines, but by AI assistants too.**
