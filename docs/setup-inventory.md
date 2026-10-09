# Setup inventory

Audit date: **2026-10-09**. Scope: the current Linux PC's OMP setup, shared skills, and the Claude Code/Codex configuration surfaces found alongside it. This is a point-in-time inventory, not an installation guide or proof that every integration works.

## 1. Harnesses and supporting tools

Versions below were obtained by running each CLI's version command.

| Component | Version | Observed role/status |
| --- | --- | --- |
| OMP (Oh My Pi) | 18.8.7 | Primary harness; active for this audit |
| Claude Code | 2.1.117 | Installed; secondary configuration found |
| Codex CLI | 0.159.2 | Installed; secondary configuration found |
| Bun | 1.4.2 | Available on PATH |
| Node.js | 26.10.0 | Available on PATH; ECC lifecycle adapter can use Node |
| npm | 12.1.0 | Available on PATH |
| Git | 2.56.0 | Available; repository operations exercised |
| GitHub CLI | 2.102.0 | Repository metadata access exercised |
| Linux | 6.18.54-1-lts, x86_64 | Reported by `uname -srmo` |

A standalone `pi` command was not found on PATH. Pi-oriented package READMEs should not be treated as instructions to install a second harness: this setup uses `omp plugin ...`.

### OMP configuration

Evidence: `~/.omp/agent/config.yml` and effective values returned by `omp config`.

| Setting | Observed value | Meaning/limit |
| --- | --- | --- |
| Default model role | `openai-codex/gpt-6.1-sol` | Configured model identifier, not a benchmark or account-entitlement claim |
| Thinking level | `auto` | Effective configuration |
| Composer / dark theme | `band` / `titanium` | Persisted configuration |
| Tool approval mode | `yolo` | Auto-approves all tool tiers; user policy may still prompt or block |
| Skill discovery | Enabled | Shared Agents and Pi user/project sources enabled; Claude project source enabled |
| Claude/Codex user skill discovery | Disabled / disabled | Their user skill directories are not enabled through these OMP discovery flags |
| Custom/filter skill lists | Empty | No custom directories, ignored skills, or include-only list observed |
| Extension override/disable lists | Empty | No explicit extension paths or disabled-extension overrides observed |
| LSP | Enabled, lazy, shared | Diagnostics on write enabled; formatting on write and diagnostics on edit disabled; language-server startup not tested |
| Task-agent LSP | Disabled | Effective `task.enableLsp` value; distinct from the main harness LSP setting |
| Project MCP configuration | Enabled | Configuration loading is enabled; no project `.mcp.json` existed in this initially empty repo |

The effective `yolo` value is not explicitly set in the small persisted config file. Record the effective state, not an invented user customization. Neither `yolo` nor an LSP setting proves a sandbox or a working language server.

## 2. OMP plugins

Both plugins are recorded as enabled in `~/.omp/plugins/omp-plugins.lock.json` and enumerated by `omp plugin list`.

| Plugin | Installed version | Source and contribution | Verification limit |
| --- | --- | --- | --- |
| ECC / `ecc-universal` | 2.2.2 | [`affaan-m/ECC`](https://github.com/affaan-m/ECC); marketplace identity `ecc@ecc`; canonical skills, command templates, agents, rules, and a Pi lifecycle adapter | Package resources counted; ECC skills/rules available in this session; not all hooks or templates executed |
| `@nicknisi/pi-claude-design` | 0.1.0 | [`nicknisi/pi-extensions`](https://github.com/nicknisi/pi-extensions/tree/main/packages/claude-design); Design skill plus `/design-login`, `/design-status`, `/design-logout` | Listed and package-health checked; Design skill resolves to this package; no login or Design MCP invocation performed |

ECC's installation registry records installation/update on **2026-09-23**. Its marketplace catalog was refreshed on the audit date and advertises **2.2.3**, while the installed package remains **2.2.2**. This is a local-catalog observation, not a claim about the latest upstream release.

The npm dependency requests `@nicknisi/pi-claude-design@^0.1.0`; its installed package and plugin lock both report `0.1.0`. The version range is not an exact reinstall pin.

### What ECC supplies here

Measured from the installed OMP package, not copied from its README:

| Resource | Count | Counting rule |
| --- | --- | --- |
| Skills | 292 | Immediate `skills/*/SKILL.md` files |
| Command templates | 94 | Markdown files under `commands/` |
| Agent definitions | 68 | Immediate `agents/*.md` files |
| Portable injected rule files | 7 | Adapter allowlist; rule block present in this OMP session |

The seven rule files are `coding-style.md`, `testing.md`, `security.md`, `git-workflow.md`, `patterns.md`, `development-workflow.md`, and `code-review.md`. The adapter does not inject language-specific rule directories through this mechanism.

Its lifecycle wiring maps session start to `scripts/hooks/session-start.js` and shutdown to `scripts/hooks/session-end-marker.js`, through `scripts/hooks/run-with-flags.js`. It also registers `/ecc-doctor`. No `ECC_PI_RULES`, `ECC_HOOK_PROFILE`, or `ECC_DISABLED_HOOKS` override was present in the auditing process environment. Source wiring is not proof of successful hook execution.

**Do not confuse ECC's Claude hook bundle with OMP enforcement.** The inspected Pi adapter wires lifecycle events and rule injection, not the full Claude PreToolUse/PostToolUse/Stop hook suite. A GateGuard or delivery-gate skill being available does not establish automatic enforcement in OMP.

OMP already exposes task delegation, todos, and user-question tools in this session; two read-only audit agents were exercised. ECC's Pi README discusses companion packages for stock Pi. None of `pi-subagents`, `pi-mcp-adapter`, `@juicesharp/rpiv-ask-user-question`, or `@juicesharp/rpiv-todo` was found in OMP's plugin dependencies. Do not install them just to duplicate OMP-native tools. The 68 packaged ECC agent files are not asserted to be 68 runtime-loaded agents.

### Design plugin caution

The installed package's README warns that it uses Anthropic's pre-registered OAuth client identity without sanctioned third-party support. It may stop working or create terms-of-service risk. It also documents plaintext token storage, shared Claude usage limits, remote prompt-injection exposure, and tools that can modify/delete real Design projects.

No Design credential file was found at the package's documented default location; overrides were not audited. No `claude-design` MCP entry was found in the scoped configuration projections. **Treat the workflow as installed but not operationally verified**, not as a working Design connection. The README describes setup through `pi-mcp-adapter`; that does not establish an OMP requirement or compatibility result.

## 3. Shared skills

Evidence: `~/.agents/skills/*/SKILL.md` and `~/.agents/.skill-lock.json` (format version 3). OMP's shared Agents user discovery is enabled, and shared skills such as `doc-coauthoring` were resolved and read in this session.

| Shared collection measurement | Count |
| --- | --- |
| Immediate directories containing `SKILL.md` | 328 |
| Entries recorded in the shared skill lock | 84 |
| Installed skills absent from that lock | 244 |
| Locked skills missing an on-disk `SKILL.md` | 0 |
| Shared skill directory names also present in OMP ECC | 124 |

No immediate shared skill directory was a symlink, and no broken immediate symlink was found. The empty `learned/` container is excluded. The lock is a partial provenance ledger, not a complete inventory.

### Lock-backed sources

| Source | Recorded skills | Main coverage |
| --- | --- | --- |
| [Anthropic skills](https://github.com/anthropics/skills) | 20 | Documents, design, Claude API, skill creation, communication |
| [Vercel agent skills](https://github.com/vercel-labs/agent-skills) | 9 | React/React Native, composition, deployment, web/writing guidelines |
| [Better Auth skills](https://github.com/better-auth/skills) | 6 | Auth implementation, security, organizations, passwords, MFA |
| [LambdaTest agent skills](https://github.com/LambdaTest/agent-skills) | 48 | Test frameworks, browser/mobile testing, accessibility, CI/CD |
| [Vercel skills](https://github.com/vercel-labs/skills) | 1 | `find-skills` |

The 244 unrecorded skills include 73 `source-command-*` directories, plus engineering/agent workflows, 21st UI skills, design/brand skills, `ruflo`, `humanizer`, and `ui-ux-pro-max`. Their presence is confirmed; do not attribute them all to ECC or claim their upstream revisions are pinned. Separately inspected local metadata identifies `humanizer` as version 2.9.1 from [`blader/humanizer`](https://github.com/blader/humanizer), and `ck` as version 2.0.0 from [`sreedhargs89/context-keeper`](https://github.com/sreedhargs89/context-keeper). These are metadata versions, not executed CLI versions.

The [snapshot](../inventory/2026-10-09.json) lists every shared skill directory, grouped by lock-backed source where possible, and all OMP ECC skill/command/agent identifiers. Names in those lists are filesystem identifiers; some skills have different display names. `skillFolderHash` is folder-hash metadata, **not a Git commit** or proof that current local content was integrity-checked.

There are overlapping names and workflows. For example, `design` exists in multiple collections; `skill://design` resolved to the Design plugin during this audit. Do not add collection counts to claim a unique loaded-skill total, or assume which duplicate wins without checking resolution.

## 4. Secondary harnesses and integrations

These are separate installed/configured surfaces. They are not proof that OMP imports Claude/Codex user settings, hooks, permissions, or MCP servers.

### Claude Code

- Installed and enabled plugins: **`ecc@ecc` 2.2.2** and **`github@claude-plugins-official`**, revision `6d3752c000e2`; official marketplace source [`anthropics/claude-plugins-official`](https://github.com/anthropics/claude-plugins-official).
- Direct user assets: 68 agent Markdown files, 94 command Markdown files, and 121 ECC rule files across 22 directories, plus a rules README. There are 155 immediate user skill directories containing `SKILL.md`; all 155 names also occur in the shared collection. This name overlap does not prove identical contents, symlink sharing, or a unique/effective Claude skill total.
- The root `~/.claude/plugin.json`/`marketplace.json` says ECC **2.2.1**, whereas the enabled cached package is **2.2.2**. These are distinct artifacts, not evidence that two ECC plugins are enabled.
- User settings register **40 hook commands across 11 events**. The cached ECC bundle declares another **24 commands across 7 events**. Registrations overlap; duplicate dispatch is possible but **unverified**.
- Hook targets include Claude Flow/Ruflo helpers, auto-memory, ECC bootstrap/dispatch, cost tracking, session evaluation, and notification helpers. These names indicate configured behavior, not successful execution.
- Permission settings contain 12 allow entries and no explicit ask/deny entries. Effective approval defaults were not established. An allowed Claude Flow MCP wildcard does not prove a Claude Flow server exists.
- Global Claude MCP configuration names `powerpoint`. Enabled plugin packages contain `github` and `chrome-devtools` definitions. Authentication and startup were not tested.

### Codex

- CLI version: **0.159.2**. Configuration enables **12 plugins**: `codex-app-tools`, `browser`, `unified-computer-use`, `visualize`, `code-review`, `documents`, `pdf`, `spreadsheets`, `presentations`, `template-creator`, `github`, and `ecc`.
- The first five belong to `openai-bundled`; the next five to `openai-primary-runtime`; GitHub belongs to `claude-plugins-official`; ECC belongs to `ecc`.
- The installed Claude ECC cache contains 292 immediate skill files; Codex's ECC cache contains 293, despite both reporting version 2.2.2. Keep each installation's inventory separate rather than assume matching versions imply matching assets.
- Direct MCP entries: `node_repl`, `code-review-graph`, and `powerpoint`. Cached enabled plugin definitions additionally name `chrome-devtools`, `github`, `cua_repl`, `code-review`, and `codex_app`. No live MCP calls were made.
- Standalone skill files: `skill-installer`, `skill-creator`, `review-agent`, `openai-docs`, `imagegen`, and `ui-ux-pro-max`. Plugin-provided skills are additional.
- 68 TOML agent files exist, but effective agent registration/delegation in Codex was not established.
- `hooks.json` registers **14 commands across 9 events**, all referencing `~/.claude/helpers/hook-handler.cjs` or `auto-memory-hook.mjs`: a real dependency on Claude-side files. Hook execution and Codex event/tool-name compatibility were not exercised.
- Configuration sets `approvals_reviewer` to `auto_review` and contains broad trust including the home directory. Explicit approval/sandbox modes were not observed. `default.rules` allows an `sshpass` command prefix and the system skill installer's Python entrypoint; this is broader than allowing a single fixed invocation.
- Cached `openai-templates`, `plugin-management`, `sites`, and `work-pets`, and a configured 21st marketplace, were also found; no enabled plugin entries for these were observed.

### MCP/LSP boundaries

A home-level `~/.mcp.json` defines `code-review-graph`; OMP ECC's packaged `.mcp.json` defines `chrome-devtools`. Configuration/package presence does not prove OMP loaded or connected either one. OMP has native MCP configuration settings, so stock-Pi documentation saying “Pi has no MCP” must not be applied blindly to OMP.

No explicit LSP server declarations were found in the scoped secondary-harness config/cache search. OMP's main LSP feature is enabled; individual language-server availability is unverified. No environment-secret values or credential files were inspected.

## 5. Recommendations — not applied

1. **Prefer workspace-write approval over `yolo` for routine work.** OMP's documented `write` mode auto-approves reads and workspace writes rather than all tiers. Use session flags such as `omp --approval-mode=write` when appropriate; this is not a sandbox. Review Codex's broad home trust and `sshpass` prefix allowance separately.
2. **Curate before adding more plugins.** There are 124 overlapping shared/ECC directory names and several independently managed ECC surfaces. Keep one canonical source per workflow where possible; choose a small everyday skill set based on actual projects. Do not delete or disable anything merely because names overlap.
3. **Verify hooks instead of assuming enforcement.** In OMP, distinguish lifecycle/rule injection from Claude's tool/Stop gates. In disposable Claude/Codex projects, check event compatibility and whether overlapping registrations fire twice before relying on a delivery gate or auto-memory.
4. **Reconcile versions and provenance deliberately.** Review the local ECC 2.2.3 catalog entry and stale Claude 2.2.1 root artifacts before any upgrade. Record exact source revisions for unrecorded shared skills. Existing folder hashes and version labels alone are not a reproducible restore plan.
5. **Only wire integrations you will use.** Treat Design as unverified and account-sensitive; prefer an officially supported connector when available. Verify required MCP connections and one relevant LSP on a disposable project before calling them operational. No evidence from this audit justifies replacing OMP.

Existing capabilities already cover planning, review, testing guidance, GitHub operations, document work, auth guidance, and UI workflows. The gap is verified operation and maintainable ownership, not the absence of another large skill pack.

## 6. Verification and maintenance

### Commands exercised during the baseline audit

- `omp --version`, `omp --help`, `omp plugin list`.
- `omp plugin doctor`: **4 OK, 0 warnings, 0 errors**; its output covered the plugin directory/manifest/dependencies and the npm Design package, not all marketplace assets or remote services.
- `omp plugin features ecc-universal` and `omp plugin features @nicknisi/pi-claude-design`: neither exposes optional features through this command.
- `omp config path`, targeted `omp config get` calls, and an allowlisted projection of `omp config list --json` for effective settings. Raw configuration was not published.
- Version commands for Claude Code, Codex, Bun, Node, npm, Git, and GitHub CLI; `uname -srmo`.
- Separate read-only Python `pathlib` enumeration completed the filesystem checks beyond the scout reports: immediate `*/SKILL.md` files measured Claude/Codex ECC caches at 292/293 and Claude user skills at 155. Shared-directory `is_symlink()`/`exists()` checks measured zero immediate skill-directory symlinks and zero broken immediate links. No skill contents were needed for these measurements.
- GitHub repository metadata access and remote-ref inspection: this public repository initially had no commits or remote branch refs.
- OMP skill resolution/reads, portable rule availability, and two task-agent executions in this audit session.

Two attempted diagnostic keys/actions were unsupported: `omp config get skills` (use the `skills.*` keys) and `omp agents list` (this CLI exposes `agents unpack`, not `list`). They are not maintenance instructions.

Not exercised: MCP authentication/tool calls, Design login, secondary-harness sessions, hook side effects, every skill/template/agent, language-server startup, or model benchmarks. No packages were installed/upgraded/removed, no credentials were read, and no harness settings were changed.

### Refresh procedure

1. Re-run version and OMP plugin commands above. Inspect install registries and lockfiles with an **allowlist of public metadata fields**, not raw dumps.
2. Enumerate skill files and reconcile their directory names with the lock. Keep filesystem counts separate from provenance, display names, and loaded runtime assets.
3. Check relevant effective OMP settings using specific keys, e.g. `omp config get tools.approvalMode`, `omp config get skills.enableAgentsUser`, and `omp config get lsp.enabled`.
4. Exercise only the capability being documented in a disposable project; avoid login, account-changing, or destructive tools unless explicitly intended. Record the exact command/action, result, and remaining limits.
5. Update this document and add `inventory/YYYY-MM-DD.json` with the audit date, public versions/sources, safe names/counts, provenance gaps, and verification boundaries. Preserve older snapshots.
6. Review staged files for secrets/private metadata, validate JSON and local Markdown links, then commit and push. Never copy live `config.toml`, `.claude.json`, auth state, session databases, or helper memory data into the repo.

`.gitignore` is a guardrail, not comprehensive leak prevention: it does not protect already tracked or force-added files, or every standalone configuration filename. Sanitized examples, including `.env.example`, still require content review and should be added only when their behavior and safety are understood.
