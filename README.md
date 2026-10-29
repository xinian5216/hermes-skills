# hermes-skills

A small collection of agent skills that solve operational problems I actually hit
in production. Each skill is self-contained, has no personal configuration
baked in, and is safe to read before you run anything.

面向真实运维场景的 Agent Skills 集合。每个 skill 自包含、不含个人配置，运行前
可以先完整读一遍。

## Skills

| Skill | What it does | 用途 |
|---|---|---|
| [whmcs-invoice-only-safety](skills/whmcs-invoice-only-safety/) | Guardrails for WHMCS/Lagom order automation that must stop at an unpaid invoice and never move money. Pinned products, positive login checks, scoped cart checks, single-submit semantics, invoice verification. | WHMCS/Lagom 下单自动化只出未付账单、绝不扣款的安全规约 |
| [obsidian-livesync-server](skills/obsidian-livesync-server/) | Stand up a private Obsidian LiveSync backend with no exposed ports: CouchDB on loopback, outbound-only Cloudflare Tunnel, mandatory E2E encryption, seeded audit checklist, rehearsable backup/restore. | 零入站端口的 Obsidian LiveSync 服务端部署（CouchDB + Cloudflare Tunnel） |
| [hermes-config-change-checklist](skills/hermes-config-change-checklist/) | Turn any Hermes Agent config edit into a provable loop: discover, narrowest change, verify with real commands, fixed four-section report, explicit restart boundary. Includes bulk skill-library maintenance. | Hermes 配置变更的可验证闭环：先查现状、最小改动、真实验证、固定回报格式、明确重启边界 |
| [remote-vps-control](skills/remote-vps-control/) | Onboard, harden, rotate and revoke SSH access to other VPS hosts: one key per host, `from="IP/32"` pinning, out-of-band host-key verification, staged lockout-safe hardening, least-privilege sudo. | 多台 VPS 的 SSH 安全接管：每机一钥、来源 IP 白名单、带外核对 Host Key、分阶段防锁死、最小 sudo |

## When to use them

**whmcs-invoice-only-safety** — you are building or reviewing an order adapter
that should create an invoice for a human to pay manually, and the worst possible
bug is spending money. Also useful as an audit checklist when an unexplained
invoice appeared.

**obsidian-livesync-server** — you want your own Obsidian sync backend instead of
a SaaS, on a VPS where you would rather not open any inbound port. Covers the
deployment, the security audit after it, and how to actually verify your backups.

**hermes-config-change-checklist** — you are changing Hermes config and want to
avoid the classic failure: the YAML looks right, nothing was exercised, and the
breakage surfaces a day later. Pairs well with `hermes-agent-skill-authoring`.

**remote-vps-control** — you are handing an agent SSH access to a server and want
to avoid the two usual outcomes: locking yourself out, or leaving a shared key
that opens everything. The chain-hardening order matters more than any single
command in it.

## Install

Standard Agent Skills layout — install with any compatible client:

```bash
# skills.sh / npx skills — all skills in this repo
npx skills add xinian5216/hermes-skills

# only one of them
npx skills add xinian5216/hermes-skills --skill whmcs-invoice-only-safety

# list what is in the repo without installing
npx skills add xinian5216/hermes-skills --list

# Hermes Agent (direct SKILL.md URL)
hermes skills install https://raw.githubusercontent.com/xinian5216/hermes-skills/main/skills/whmcs-invoice-only-safety/SKILL.md
```

Or just read `skills/<name>/SKILL.md` and copy what you need — these are documents,
not packages.

## ClawHub

Versioned releases are published on ClawHub:

- [whmcs-invoice-only-safety](https://clawhub.ai/skills/whmcs-invoice-only-safety)
- [obsidian-livesync-server](https://clawhub.ai/skills/obsidian-livesync-server)

GitHub is the source of truth. Changes land here first, then get published as a
new version.

## License

[MIT-0](LICENSE) — no attribution required, use it however you like. This matches
the license ClawHub applies to every published skill, so the two channels stay
consistent.

## Scope

These skills carry no credentials and no personal hostnames. Anything
environment-specific is a placeholder you fill in. Nothing here is auto-updated:
each repository change is reviewed and published deliberately.
