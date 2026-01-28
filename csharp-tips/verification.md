# Verification Protocol

**IMPORTANT:** When explaining tips in depth, verify against official Microsoft documentation.

## Environment Detection

Detect which tools are available and adapt:

| Environment | Detection | Verification Method |
|-------------|-----------|---------------------|
| **Claude Code** | MCP tools available (`mcp__microsoft-learn__*`) | Use MCP tools (preferred) |
| **Claude (web/API)** | WebSearch/WebFetch available | Use web search fallback |
| **Other LLMs** | Neither available | Provide doc URLs for manual verification |

---

## Method A: Claude Code with MCP (Preferred)

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

## Method B: Web Search Fallback

If MCP is unavailable but web tools exist (WebSearch, WebFetch):

```
1. WebSearch: "[feature name] site:learn.microsoft.com"
2. WebSearch: "[feature name] .NET [current year] changes"
3. WebFetch: Fetch the Microsoft Learn URL directly
```

---

## Method C: Manual Verification (Other LLMs)

If no verification tools are available, provide URLs for the user:

- **Microsoft Learn:** `https://learn.microsoft.com/en-us/dotnet/`
- **.NET API Browser:** `https://learn.microsoft.com/en-us/dotnet/api/`
- **.NET Blog (What's New):** `https://devblogs.microsoft.com/dotnet/`
- **GitHub dotnet/runtime:** `https://github.com/dotnet/runtime`

Instruct user: "I recommend verifying this at [URL] as I cannot access live documentation."

---

## Verification Checklist (All Environments)

Regardless of method, always:

1. **Check API Currency** - Confirm namespace exists, not deprecated
2. **Check for Newer Alternatives** - Note if superseded by better patterns
3. **Note Version-Specific Behavior** - Which .NET versions apply
4. **Cite Sources** - Include Microsoft Learn links
5. **Flag Outdated Info** - Explain modern alternatives if tip is dated

.NET releases annually in November (even years = LTS). Always check against the current year's release.
