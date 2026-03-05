# Security Policy

## Supported Versions

Only the latest release of VaultClaw receives security fixes.

| Version | Supported |
|---------|-----------|
| Latest stable | Yes |
| Older versions | No |

## Reporting a Vulnerability

**Please do not report security vulnerabilities through public GitHub issues.**

Report security vulnerabilities through [GitHub Private Security Advisories](https://github.com/your-org/vaultclaw/security/advisories/new).

Include:
- A description of the vulnerability
- Steps to reproduce or proof-of-concept code
- Potential impact assessment
- Any suggested mitigations you are aware of

You will receive acknowledgment within 72 hours. If the vulnerability is confirmed, we aim to release a fix within 14 days depending on severity and complexity.

## Scope

The following are in scope for security reports:

- **Gateway authentication bypass** — Circumventing the token/password auth on the WebSocket gateway
- **Channel privilege escalation** — Gaining access to a paired channel session without authorization
- **Secret leakage** — API keys, session tokens, or secrets exposed via logs, error messages, or environment
- **Plugin sandbox escape** — A plugin/skill accessing resources outside its intended scope
- **SSRF via tool invocations** — Using browser or HTTP tools to reach internal network resources
- **Arbitrary code execution** — Via agent tool invocation or config parsing

The following are **not** in scope:

- Vulnerabilities in third-party LLM provider APIs (report to the provider)
- Denial-of-service against your own local gateway instance
- Issues requiring physical access to the host machine
- Social engineering attacks
- Issues in the messaging platforms themselves (Telegram, Discord, Slack, etc.)

## Security Architecture Notes

For context when evaluating reports:

- The gateway binds to loopback (`127.0.0.1`) by default and requires explicit configuration to bind to LAN or public interfaces
- All WebSocket connections must present a valid gateway token before any messages are processed
- Docker deployments run as non-root user `node` (uid 1000) with all Linux capabilities dropped
- Secrets are mounted as files via Docker secrets, not passed as environment variables
- The DM pairing system requires explicit opt-in for unknown senders
- No telemetry or data is sent to external servers beyond your configured LLM providers and channel APIs
