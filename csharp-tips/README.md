# C# Tips Skill

A curated collection of 200 C# best practices, patterns, and performance tips for use with Claude Code.

## Source Attribution

This skill is based on two excellent YouTube videos by **Nick Chapsas**:

### Video 1: 100 C# Tips and Tricks That Will Blow Your Mind
- **URL:** https://www.youtube.com/watch?v=8F-Pb-SKO5g
- **Tips:** 1-100
- **Topics:** Runtime performance, language syntax, pattern matching, async/await, design patterns, tooling

### Video 2: Another 100 C# Tips and Tricks
- **URL:** https://www.youtube.com/watch?v=V2uRyxffnHA
- **Tips:** 101-200
- **Topics:** .NET 8/9 features, EF Core, Minimal APIs, architecture patterns, security, observability

## Usage

```bash
# Search by keyword
/csharp-tips async
/csharp-tips performance
/csharp-tips linq

# Browse by category
/csharp-tips Runtime
/csharp-tips Language
/csharp-tips Design
```

## Categories

| Category | Tips | Description |
|----------|------|-------------|
| Runtime & Performance | 1, 3-4, 6, 9, 32-34, 47, 50, 83-85, 113, 116-118, 161, 196 | Memory optimization, collections, pooling, Span<T> |
| Language & Syntax | 8, 11-12, 14, 16, 22-23, 26-31, 38, 40-42, 51-55, 61-69, 94-97, 101-112, 120-131 | Modern C# features, pattern matching, records |
| Design & Architecture | 2, 7, 10, 15, 24, 76-82, 106, 114-115, 121, 123, 180-193 | Clean code, DI, patterns, CQRS, DDD |
| Async & Threading | 56-60, 99-100, 107, 153-154 | async/await, Channels, Parallel.ForEachAsync |
| Data & Entity Framework | 132-143 | EF Core performance, queries, migrations |
| Tooling & DX | 5, 13, 17-18, 20-21, 25, 35-36, 71-75, 90-93, 119, 125, 145-150, 156-157, 166-167, 172 | Analyzers, debugging, testing, CI/CD |
| Security & Web API | 70, 147, 151-152, 155, 158-165, 168-171, 175-178, 194 | ASP.NET, Minimal APIs, rate limiting, OWASP |

## Files

| File | Description |
|------|-------------|
| `SKILL.md` | Skill definition and search functionality |
| `csharp-tips.csv` | Raw tip data (searchable) |
| `csharp-tips.md` | Full formatted tips with explanations |

## CSV Format

```csv
Number,Title,Description,Categories,Video,Timestamp,PrimaryGroup
1,"Array.Empty...","When you need...","runtime-performance,collections","https://...","00:35","Runtime & performance"
```

Each tip includes:
- **Number** - Tip ID for quick reference
- **Title** - Short descriptive title
- **Description** - Full explanation
- **Categories** - Tags for searching
- **Video** - Source YouTube URL
- **Timestamp** - Jump directly to the explanation
- **PrimaryGroup** - Main category

## Example Interaction

```
User: /csharp-tips async

Claude: Found 15 tips related to "async":

**Tip 3 - SemaphoreSlim for async-safe locking**
The lock keyword doesn't work with await. Use SemaphoreSlim(1) with WaitAsync...
Video: [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (02:15)

**Tip 56 - ValueTask**
If your async method often returns a result immediately (synchronously), return ValueTask<T>...
Video: [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (33:10)
...
```

## Verification

Tips should be verified against official Microsoft documentation, especially for:
- API currency (is the namespace still valid?)
- Newer alternatives (has a better pattern emerged?)
- Version-specific behavior (which .NET versions?)

The skill includes instructions for verification using:
1. Microsoft Learn MCP server (preferred)
2. Web search fallback
3. Manual verification URLs

## Using with Other LLMs

The `SKILL.md` is Claude Code-specific, but the data files work anywhere:

| File | Use Case |
|------|----------|
| `csharp-tips.csv` | Upload to ChatGPT, use with RAG, import into vector DB |
| `csharp-tips.md` | Paste into system prompts, reference in Cursor/Copilot |

See the [main README](../README.md#using-with-other-llms) for detailed examples.

## License

This skill is released under the MIT License.

The original video content belongs to Nick Chapsas and is used here as reference material with timestamps for attribution. Please watch the original videos to support the creator!

## Contributing

Found an error or want to add more tips? PRs welcome!
