# Monster.de Crawler & Analyzer

A three-stage pipeline that crawls job postings from Monster.de, parses them into structured data, and runs frequency analysis to find patterns across listings. Built to answer the question: "What do employers actually want?" — with data instead of guesswork.

## What it does

1. **Crawl** — Searches Monster.de for a given term, collects up to 1,000 job posting URLs (respecting rate limits)
2. **Parse** — Extracts structured data from each posting: title, company, location, description, requirements, and benefits. Handles both JSON-LD structured data and raw HTML fallbacks.
3. **Analyze** — Builds a full-text search index (Whoosh) and runs frequency analysis across postings to surface the most common tasks, required skills, and offered benefits

## Running it

```bash
# Install dependencies
pip install requests lxml beautifulsoup4 npyscreen whoosh regex

# Launch (must be run from terminal, not an IDE)
python UserDialog.py
```

**Important:** The terminal UI (`npyscreen`) doesn't play nice with IDE consoles. Run it from an actual terminal.

The text-based UI walks you through:
- Entering a search term (min. 5 characters)
- Selecting operations: link retrieval, posting extraction, analysis
- Viewing results: index stats, frequent activities, common requirements, typical benefits

## How the analysis works

The analysis pipeline tokenizes job descriptions, filters through German and English stopword lists (750+ German, 100+ English entries), and ranks terms by frequency. It extracts words longer than 5 characters and returns the middle 20% of ranked terms — filtering out both the too-generic and the too-specific.

Results are exported as organized text files.

## Architecture

```
UserDialog.py         — Terminal UI (npyscreen) with threaded background processing
MainController.py     — Orchestration: manages crawling, parsing, and analysis flow
Parsing.py            — Link extraction (XPath) and posting parser (JSON-LD + HTML)
Indexing/Indexer.py    — Whoosh full-text search index builder
Indexing/Analyzing.py  — Token frequency analysis with stopword filtering
Util/FileHandler.py   — Persistence layer (pickle serialization, file I/O)
Util/RequestManager.py — HTTP request wrapper
Util/log_listener.py   — TCP-based logging server for real-time progress tracking
```

## Dependencies

- **requests** — HTTP client
- **lxml** — HTML parsing with XPath
- **BeautifulSoup4** — HTML parsing
- **npyscreen** — Terminal-based GUI
- **Whoosh** — Full-text search and indexing
- **regex** — Extended regular expressions

Windows users may need to install curses binaries separately.

## Context

Graduate project (University of Bamberg) for exploring web scraping, text analysis, and information retrieval. The German job market has its own vocabulary and posting conventions — this tool was built to make sense of it at scale.
