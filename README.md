<p align="center">
  <h1 align="center">jko-claude-plugins</h1>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License: MIT"></a>
  <img src="https://img.shields.io/badge/plugins-5-orange" alt="Plugins: 5">
  <img src="https://img.shields.io/badge/commands-15-green" alt="Commands: 15">
</p>

These are the guidelines and patterns I use across my projects. They help coding agents review code, catch mistakes, and stay consistent with how I actually want things built. Each plugin covers a stack I work in.

Skills use the shared [Agent Skills specification](https://developers.openai.com/codex/skills/) — install as full plugins or individual skills.

<h4 align="center">Works with</h4>

<p align="center">
  <a href="#claude-code"><img src="https://img.shields.io/badge/Claude_Code-D97757?style=for-the-badge&logo=claude&logoColor=fff" alt="Claude Code"></a>
  <a href="#github-copilot-cli"><img src="https://img.shields.io/badge/Copilot_CLI-000?style=for-the-badge&logo=githubcopilot&logoColor=fff" alt="GitHub Copilot CLI"></a>
  <a href="#openai-codex-cli"><img src="https://img.shields.io/badge/Codex_CLI-412991?style=for-the-badge&logo=openai&logoColor=fff" alt="OpenAI Codex CLI"></a>
  <a href="#opencode"><img src="https://img.shields.io/badge/⌬_OpenCode-18181B?style=for-the-badge" alt="OpenCode"></a>
</p>

## Plugins

| Plugin | Skill | Cmds | What it covers |
|--------|-------|:----:|----------------|
| **[rust](plugins/rust/)** | `rust` | 5 | Ownership, async, unsafe, error handling, type design, anti-patterns |
| **[esp32-cpp](plugins/esp32-cpp/)** | `esp32` | 4 | FreeRTOS, ESP-IDF/PlatformIO, peripherals, memory, all ESP32 variants |
| **[python-backend](plugins/python-backend/)** | `python-backend` | 3 | Litestar, FastAPI, SQLAlchemy, hexagonal architecture, async patterns |
| **[swiftui](plugins/swiftui/)** | `swiftui` | 1 | iOS/macOS/visionOS patterns, Liquid Glass, accessibility |
| **[dead-code](plugins/dead-code/)** | `dead-code` | 2 | Unused imports, functions, classes, duplicates, any language |

## Install

### Claude Code

```bash
# Add the marketplace
/plugin marketplace add johnkozaris/jko-claude-plugins

# Install whichever plugins you want
/plugin install rust@jko-claude-plugins
/plugin install esp32-cpp@jko-claude-plugins
/plugin install python-backend@jko-claude-plugins
/plugin install swiftui@jko-claude-plugins
/plugin install dead-code@jko-claude-plugins
```

Or try one without installing:

```bash
claude --plugin-dir /path/to/jko-claude-plugins/plugins/rust
```

### GitHub Copilot CLI

```bash
# Add the marketplace
/plugin marketplace add johnkozaris/jko-claude-plugins

# Install a plugin
/plugin install rust@jko-claude-plugins
```

### OpenAI Codex CLI

Codex installs skills directly — no marketplace needed.

```bash
# Inside Codex, use the built-in skill installer
$skill-installer --repo johnkozaris/jko-claude-plugins --path plugins/rust/skills/rust-expert
$skill-installer --repo johnkozaris/jko-claude-plugins --path plugins/esp32-cpp/skills/esp32-expert
$skill-installer --repo johnkozaris/jko-claude-plugins --path plugins/python-backend/skills/python-backend-expert
$skill-installer --repo johnkozaris/jko-claude-plugins --path plugins/swiftui/skills/swiftui-expert
$skill-installer --repo johnkozaris/jko-claude-plugins --path plugins/dead-code/skills/dead-code-expert
```

### OpenCode

Install skills via [skills.sh](https://skills.sh) or copy manually.

```bash
# Via skills.sh (auto-detects installed agents)
npx skills add johnkozaris/jko-claude-plugins --full-depth

# Or copy a skill directory manually
cp -r plugins/rust/skills/rust-expert ~/.config/opencode/skills/rust-expert
```

## Commands

Commands are available in Claude Code and Copilot CLI. Codex and OpenCode get the skills and references but not slash commands.

### Rust

| Command | What it does |
|---------|-------------|
| `/rust-critique` | Full code review for soundness, ownership, error handling, types, async, performance |
| `/rust-harden` | Replace `unwrap` with proper errors, add SAFETY comments to unsafe, validate inputs |
| `/rust-types` | Replace primitives with newtypes, booleans with enums, make illegal states unrepresentable |
| `/rust-polish` | Pre-merge cleanup: dead code, doc comments, clippy, debug artifacts |
| `/rust-teach` | One-time: scans your project and writes Rust conventions to CLAUDE.md |

### ESP32

| Command | What it does |
|---------|-------------|
| `/esp-harden` | Scan firmware for field failures, crashes, memory issues, and security problems |
| `/esp-debug` | Help debug crashes, hangs, and peripheral issues |
| `/esp-optimize` | Optimize for performance, memory, power, or binary size |
| `/esp-teach` | One-time: discover hardware, find datasheets, persist context to CLAUDE.md |

### Python

| Command | What it does |
|---------|-------------|
| `/py-critique` | Architecture review: SOLID compliance, layer boundaries, anti-patterns, design quality |
| `/py-harden` | Run the full anti-pattern catalog (AP-01 through AP-22) and fix what it finds |
| `/py-structure` | Check project layout, file sizes, module splitting, hexagonal architecture compliance |

### SwiftUI

| Command | What it does |
|---------|-------------|
| `/swift-critique` | Review SwiftUI code for patterns, design, accessibility, and performance |

### Dead Code

| Command | What it does |
|---------|-------------|
| `/dead-code-scan` | Read-only scan for unused imports, functions, classes, duplicates |
| `/dead-code-clean` | Actually remove the dead code it finds |

## How it works

Each plugin has a SKILL.md that tells the agent what to look for and a bunch of reference files with the actual patterns, anti-patterns, and examples. The agent only loads the references it needs for the current task so it doesn't waste context.

Hooks run automatically on file saves. `cargo check` after editing `.rs` files, flagging common mistakes in Python/C++ edits, that kind of thing.

### Cross-tool compatibility

| Feature | Claude Code | Copilot CLI | Codex CLI | OpenCode |
|---------|:-----------:|:-----------:|:---------:|:--------:|
| Skills + references | yes | yes | yes | yes |
| Slash commands | yes | yes | -- | -- |
| Hooks | yes | yes | -- | -- |
| Plugin marketplace | yes | yes | -- | -- |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE)
