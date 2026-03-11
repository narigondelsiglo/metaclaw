# AGENTS.md — MetaClaw Setup Architect

## Project Overview

**MetaClaw Setup Architect** is an OpenClaw skill that generates complete multi-agent setups from natural language descriptions. Users describe their use case, and the skill produces all configuration files (agents, skills, workflows, memory) plus installation steps.

## How to Work on This Project

### Understanding the Architecture

```
skills/metaclaw-setup-architect/
├── SKILL.md                          # Main orchestrator skill
├── knowledge/                        # Reference knowledge base
│   ├── file-system.md                # OpenClaw file system reference
│   ├── tool-catalog.md               # 80+ tools/MCPs by domain
│   ├── agent-patterns.md             # 5 multi-agent architecture patterns
│   └── skill-templates.md            # Domain-specific SKILL.md templates
├── templates/                        # File generation templates
│   ├── AGENTS.md.tmpl
│   ├── SOUL.md.tmpl
│   ├── MEMORY.md.tmpl
│   ├── openclaw.json.tmpl
│   └── ... (11 total)
└── examples/                         # End-to-end examples
    ├── youtube-clipper.md
    └── community-assistant.md
```

### Key Workflows

1. **Discovery Phase**: Ask 4-6 targeted questions to understand user requirements
2. **Design Phase**: Select appropriate multi-agent pattern and tools
3. **Generation Phase**: Render templates with discovered requirements
4. **Installation Phase**: Provide copy-pasteable setup commands

### Multi-Agent Patterns (Knowledge Base)

| Pattern | Use Case |
|---------|----------|
| Pipeline | Sequential processing (A → B → C) |
| Hub-and-Spoke | Coordinator + specialists |
| Autonomous Loop | Self-running recurring tasks |
| Human-in-the-Loop | Agent proposes, human approves |
| Supervisor | Quality control over agents |

## Development Guidelines

### Adding New Content

- **Tools**: Edit `knowledge/tool-catalog.md`
- **Patterns**: Edit `knowledge/agent-patterns.md`
- **Templates**: Edit files in `templates/`
- **Examples**: Add new `.md` files to `examples/`

### Testing

```bash
# Start Docker environment with skill mounted
make up

# Run smoke tests
make test

# View logs
make logs

# Shell into container
make shell
```

**Prerequisites**: Ollama running on host (`ollama serve`) with at least one model pulled.

### Environment Setup

```bash
cp .env.example .env
# Edit .env to configure OLLAMA_BASE_URL (default: http://localhost:11434)
```

## Code Conventions

- **Markdown**: All knowledge, templates, and examples use consistent Markdown formatting
- **Templates**: Use `.tmpl` extension with placeholder syntax for variable substitution
- **Examples**: Follow existing format in `examples/` with clear agent roles and workflows

## Common Tasks

### Debugging Template Generation
1. Check template syntax in `templates/*.tmpl`
2. Verify knowledge base references in `knowledge/`
3. Review example outputs in `examples/`

### Adding a New Tool Category
1. Add tools to `knowledge/tool-catalog.md` under appropriate category
2. Update any relevant skill templates that reference tools
3. Add an example demonstrating the new tool usage

### Validating Multi-Agent Patterns
1. Ensure pattern is documented in `knowledge/agent-patterns.md`
2. Add example in `examples/` showing the pattern in action
3. Test pattern generation via Docker environment

## Testing Checklist

- [ ] Gateway health: `GET /healthz` responds 200
- [ ] Skill mounted: `SKILL.md` exists in container
- [ ] Knowledge base: all 4 files present
- [ ] Templates: all 11 files present
- [ ] Examples: both example files present
- [ ] Skill discovered: appears in `openclaw skills list`
- [ ] Agent trigger: activates on test prompt

## Resources

- **OpenClaw Docs**: Reference in `knowledge/file-system.md`
- **Tool Catalog**: 80+ tools across 12 categories in `knowledge/tool-catalog.md`
- **Docker Testing**: See README.md "Docker Testing" section
