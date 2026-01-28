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

## Verification

When explaining tips in depth, verify against official Microsoft documentation. See [verification.md](verification.md) for detailed protocols.
