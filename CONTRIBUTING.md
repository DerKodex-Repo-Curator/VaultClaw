# Contributing to VaultClaw

Thank you for your interest in contributing. VaultClaw is a personal AI infrastructure project and contributions that improve stability, extend channel support, or improve developer experience are especially welcome.

---

## Prerequisites

| Tool | Minimum Version | Install |
|------|----------------|---------|
| Node.js | 22 | [nodejs.org](https://nodejs.org) |
| pnpm | 10 | `npm install -g pnpm` |
| Bun | Latest (optional) | `curl -fsSL https://bun.sh/install \| bash` |
| Git | Any recent | — |

Bun is used by some build scripts but Node.js + pnpm is sufficient for most development.

---

## Development Setup

```bash
# 1. Fork and clone
git clone https://github.com/<your-username>/VaultClaw.git
cd VaultClaw

# 2. Install all dependencies (root + extensions + skills + ui)
pnpm install

# 3. Set up local environment
cp .env.example .env
# Edit .env with your test API keys

# 4. Build
pnpm build      # TypeScript → dist/
pnpm ui:build   # Web UI → dist/control-ui/

# 5. Verify setup
node dist/index.js doctor
```

---

## Running Tests

```bash
# Unit tests (fast, no API keys needed)
pnpm test

# Unit tests with coverage
pnpm test:coverage

# End-to-end tests (requires .env with valid keys)
pnpm test:e2e

# Type checking only
pnpm typecheck

# Lint
pnpm lint

# Format check
pnpm fmt
```

All tests must pass before submitting a PR. New features should include tests.

---

## Project Structure

```
src/
├── agents/       # Pi agent orchestration — modify with care
├── gateway/      # WebSocket server — core routing logic
├── channels/     # Channel abstraction layer
├── commands/     # CLI command implementations
├── config/       # Config loading and validation
├── memory/       # Vector memory and context
├── security/     # Auth and pairing logic
├── plugin-sdk/   # SDK for extensions and skills — stable API
└── providers/    # LLM provider adapters

extensions/       # Infrastructure extensions (42+)
skills/           # User-facing skill modules (55+)
ui/               # Vite + Lit web control panel
apps/             # Native macOS, iOS, Android apps
```

---

## Commit Style

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
type(scope): short description

feat(channels): add Matrix channel adapter
fix(gateway): handle reconnection on timeout
docs(readme): update Docker quickstart
refactor(agents): simplify block streaming loop
test(telegram): add DM pairing integration test
chore(deps): upgrade discord.js to v15
```

Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `perf`, `ci`

Keep the subject line under 72 characters. Use the body to explain *why*, not *what*.

---

## Pull Request Process

1. **Branch from `main`**: `git checkout -b feat/my-feature`
2. **Keep PRs focused**: One logical change per PR
3. **Test your changes**: Run `pnpm test` and `pnpm typecheck`
4. **Write a clear description**: What changed, why, and how to test it
5. **Link related issues**: Use `Closes #123` in the PR body
6. **Be patient**: Reviews may take a few days

PRs that change behavior in channels or the agent runtime require extra scrutiny — expect detailed review.

---

## Writing a Skill

Skills are the primary extension point. A skill is a TypeScript module in `skills/your-skill-name/`:

```
skills/your-skill/
├── package.json          # name: "@vaultclaw/skill-your-skill"
├── src/
│   └── index.ts          # Default export: skill manifest
└── README.md
```

Minimal skill manifest:

```typescript
// src/index.ts
import type { Skill } from '@vaultclaw/plugin-sdk'

export default {
  name: 'your-skill',
  version: '1.0.0',
  description: 'What this skill does',

  tools: [
    {
      name: 'your_tool',
      description: 'Tool description shown to the LLM',
      inputSchema: {
        type: 'object',
        properties: {
          query: { type: 'string', description: 'Input parameter' }
        },
        required: ['query']
      },
      execute: async (input: { query: string }) => {
        // Implementation
        return { result: 'output' }
      }
    }
  ]
} satisfies Skill
```

See `src/plugin-sdk/` for the full SDK types and `skills/github/` for a real-world example.

---

## Code Style

- **Linter**: Oxlint — run `pnpm lint` before committing
- **Formatter**: Oxfmt — run `pnpm fmt` to auto-format
- **No `any`**: Use proper TypeScript types
- **ESM only**: This project uses ES modules (`"type": "module"`)
- **No default exports for commands**: Commands use named exports
- **Error handling**: Use typed errors, not generic `Error` with string parsing

---

## Reporting Issues

Use the GitHub issue templates for:
- **Bug reports**: Include reproduction steps, OS, Node version, channel, and relevant logs
- **Feature requests**: Describe the use case first, then the proposed solution

For security issues, see [SECURITY.md](SECURITY.md).

---

## Code of Conduct

All contributors are expected to follow the [Code of Conduct](CODE_OF_CONDUCT.md).
