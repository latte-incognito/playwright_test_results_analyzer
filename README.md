# Playwright Test Analyzer

A Julia web app that fetches a Playwright JSON test report, parses the full test hierarchy, and sends structured results to an LLM for analysis — displayed in a minimal dark UI.

Built as a quick tool to automate post-run test triage without leaving the browser.

## What it does

1. Takes a presigned S3 URL pointing to a Playwright `results.json`
2. Fetches and parses the report — walks `suites → specs → tests → results` to extract status, duration, retry count, and error messages
3. Sends the structured data to a configurable LLM endpoint
4. Shows the raw parsed results alongside the LLM response in a split-panel UI

## Stack

- **Julia** — core language
- **Genie.jl** — HTTP server and routing
- **HTTP.jl** — fetching the S3 report and calling the LLM
- **JSON.jl** — parsing Playwright output

## Setup

**Install Julia:**
```bash
# macOS
brew install juliaup && juliaup add release

# Linux / WSL
curl -fsSL https://install.julialang.org | sh && juliaup add release
```

**Install dependencies:**
```bash
julia --project=. -e 'using Pkg; Pkg.instantiate(); Pkg.precompile()'
```

## Configuration

The app reads from the environment:

| Variable | Description |
|---|---|
| `MY_LLM_KEY` | API key forwarded as `X-Api-Key` header |

The LLM endpoint URL is set directly in `app.jl` — swap `https://example.com/endpoint` for your actual endpoint.

## Run

```bash
export MY_LLM_KEY="your-key-here"
julia --project=. app.jl
# open http://127.0.0.1:8080
```

Paste a presigned S3 URL to a Playwright `results.json` and hit **Send**.
