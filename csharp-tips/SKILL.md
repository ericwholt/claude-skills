---
name: csharp-tips
description: |
  200 C# best practices, patterns, and performance tips. Use when:
  - Writing or reviewing C# code
  - Asking about C# patterns, async, LINQ, EF Core
  - Optimizing performance or memory usage
  - Learning modern C# and .NET features
argument-hint: <keyword>
user-invocable: true
---

# C# Tips Reference

A curated collection of 200 C# tips covering:
- **Runtime & Performance** - Memory, collections, pooling, Span<T>
- **Language & Syntax** - Modern C# features, pattern matching, records
- **Async & Threading** - async/await, Channels, Parallel.ForEachAsync
- **Design & Architecture** - DI, Options pattern, clean code
- **Entity Framework Core** - Performance, queries, migrations
- **ASP.NET & Web API** - Minimal APIs, caching, rate limiting
- **Tooling & DX** - Analyzers, debugging, testing

## Usage

### Search by keyword
```
/csharp-tips async
/csharp-tips performance
/csharp-tips linq
/csharp-tips ef-core
```

### Browse by category
```
/csharp-tips Runtime            # Runtime & performance tips
/csharp-tips Language           # Language & syntax tips
/csharp-tips Design             # Design & architecture tips
/csharp-tips Threading          # Async & threading tips
/csharp-tips Entity             # EF Core tips
```

## Search Results

**Query:** $ARGUMENTS

### Instructions for Claude:
Use the Grep tool to search for "$ARGUMENTS" (case-insensitive) in the `csharp-tips.csv` file located in this skill's directory. Return up to 25 matching lines. Format the results clearly, showing tip number, title, and description.

## Quick Reference

For the **complete formatted tips** with full explanations, read: [csharp-tips.md](csharp-tips.md)

For **raw data** (filterable): [csharp-tips.csv](csharp-tips.csv)

## Tip Categories

| Category | Description |
|----------|-------------|
| Runtime & performance | Memory optimization, collections, pooling |
| Language & syntax | Modern C# features, pattern matching |
| Design & architecture | Clean code, DI, patterns |
| Async & Threading | async/await, parallelism |
| Data & Entity Framework | EF Core optimization |
| Tooling & DX | Developer experience, debugging |
| Security & Web API | ASP.NET, APIs, security |

## How to Apply Tips

When Claude finds relevant tips, it will:
1. Cite the tip number and title
2. Explain how it applies to your current code
3. Show a code example if helpful

You can ask follow-up questions like:
- "Show me an example of tip 47"
- "When should I NOT use this pattern?"
- "What's the video timestamp for this tip?"

## Verification Protocol

**IMPORTANT:** When explaining tips in depth, verify against official Microsoft documentation.

### Environment Detection

Detect which tools are available and adapt:

| Environment | Detection | Verification Method |
|-------------|-----------|---------------------|
| **Claude Code** | MCP tools available (`mcp__microsoft-learn__*`) | Use MCP tools (preferred) |
| **Claude (web/API)** | WebSearch/WebFetch available | Use web search fallback |
| **Other LLMs** | Neither available | Provide doc URLs for manual verification |

---

### Method A: Claude Code with MCP (Preferred)

If `mcp__microsoft-learn__*` tools are available:

```
1. microsoft_docs_search - Search for the feature/API
2. microsoft_code_sample_search - Find official code examples
3. microsoft_docs_fetch - Get full page content when needed
```

**MCP Not Installed?** Install the Microsoft Learn MCP:

```bash
# Add Microsoft Learn MCP server
claude mcp add --transport http microsoft-learn https://mcp.microsoft.com/sse

# Verify installation
claude mcp list

# Check status in Claude Code
/mcp
```

For more on MCP setup: https://docs.anthropic.com/en/docs/claude-code/mcp

---

### Method B: Web Search Fallback

If MCP is unavailable but web tools exist (WebSearch, WebFetch):

```
1. WebSearch: "[feature name] site:learn.microsoft.com"
2. WebSearch: "[feature name] .NET [current year] changes"
3. WebFetch: Fetch the Microsoft Learn URL directly
```

---

### Method C: Manual Verification (Other LLMs)

If no verification tools are available, provide URLs for the user:

- **Microsoft Learn:** `https://learn.microsoft.com/en-us/dotnet/`
- **.NET API Browser:** `https://learn.microsoft.com/en-us/dotnet/api/`
- **.NET Blog (What's New):** `https://devblogs.microsoft.com/dotnet/`
- **GitHub dotnet/runtime:** `https://github.com/dotnet/runtime`

Instruct user: "I recommend verifying this at [URL] as I cannot access live documentation."

---

### Verification Checklist (All Environments)

Regardless of method, always:

1. **Check API Currency** - Confirm namespace exists, not deprecated
2. **Check for Newer Alternatives** - Note if superseded by better patterns
3. **Note Version-Specific Behavior** - Which .NET versions apply
4. **Cite Sources** - Include Microsoft Learn links
5. **Flag Outdated Info** - Explain modern alternatives if tip is dated

.NET releases annually in November (even years = LTS). Always check against the current year's release.
