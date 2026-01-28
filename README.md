# Claude Code Skills

A collection of reusable skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code), Anthropic's agentic coding tool.

## What are Skills?

Skills are reusable prompt templates that extend Claude Code's capabilities. They can be invoked with slash commands (e.g., `/csharp-tips`) to provide specialized knowledge, workflows, or reference material during coding sessions.

## Available Skills

| Skill | Description | Invoke |
|-------|-------------|--------|
| [csharp-tips](./csharp-tips/) | 200 C# best practices, patterns, and performance tips | `/csharp-tips <keyword>` |

## Installation

### Option 1: Copy to your skills directory

```bash
# Clone this repository
git clone https://github.com/ericwholt/claude-skills.git

# Copy the skill(s) you want to your Claude Code skills directory
cp -r claude-skills/csharp-tips ~/.claude/skills/
```

### Option 2: Symlink (recommended for updates)

```bash
git clone https://github.com/ericwholt/claude-skills.git ~/claude-skills

# Symlink individual skills
ln -s ~/claude-skills/csharp-tips ~/.claude/skills/csharp-tips
```

### Option 3: Add as additional working directory

In Claude Code, you can add the repository as an additional working directory to make skills available without copying:

```bash
claude --add-dir ~/path/to/claude-skills
```

## Skill Structure

Each skill follows this structure:

```
skill-name/
├── SKILL.md          # Skill definition (required)
├── README.md         # Documentation and attribution
└── [data files]      # Supporting files (CSV, JSON, etc.)
```

### SKILL.md Format

```yaml
---
name: skill-name
description: |
  Brief description of what the skill does.
  When to use it.
user-invocable: true
---

# Skill Content

Instructions, templates, and data for the skill.
```

## Contributing

Contributions welcome! Please ensure:

1. Skills are self-contained in their directory
2. No hardcoded paths or personal information
3. Include a README with attribution for any external sources
4. Test the skill works with Claude Code before submitting

## Using with Other LLMs

While the `SKILL.md` files use Claude Code-specific syntax, the underlying data files (CSV, MD) are portable and can be used with any LLM.

### ChatGPT / GPT-4

Upload the data files directly or paste relevant sections:

```
I'm uploading csharp-tips.csv - a collection of 200 C# best practices.
When I ask C# questions, search this file for relevant tips and cite them by number.
```

Or create a Custom GPT with the CSV as a knowledge file.

### GitHub Copilot Chat

Reference the files in your workspace:

```
@workspace /explain Search csharp-tips.csv for tips about async patterns
```

### Local LLMs (Ollama, LM Studio)

Include the markdown file in your system prompt or use RAG:

```python
# Example with LangChain
from langchain.document_loaders import CSVLoader
loader = CSVLoader("csharp-tips.csv")
docs = loader.load()
# Add to your vector store for retrieval
```

### Cursor / Continue.dev

Add the skill directory to your project and reference in chat:

```
Look at csharp-tips/csharp-tips.md for C# best practices relevant to this code
```

### Generic Prompt Template

For any LLM, you can use this pattern:

```
You have access to a C# tips reference. When answering C# questions:
1. Search the tips for relevant advice
2. Cite tips by number (e.g., "Tip 47 - ArrayPool")
3. Include the video timestamp if the user wants to learn more

[Paste relevant portion of csharp-tips.md or full CSV]
```

## License

MIT License - See [LICENSE](./LICENSE) for details.

Individual skills may reference external content (videos, articles) which retain their original copyright. Attribution is provided in each skill's README.
