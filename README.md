# Review Prompts for AI-Assisted Code Review

AI-assisted code review prompts for Linux kernel, systemd, and iproute development.
Works with Claude Code and other AI tools.

## Quick Start

### Install Kernel Prompts Only

```bash
cd kernel/scripts
./claude-setup.sh
```

### Install systemd Prompts Only

```bash
cd systemd/scripts
./claude-setup.sh
```

### Install iproute Prompts Only

```bash
cd iproute/scripts
./claude-setup.sh
```

### Install All

```bash
cd kernel/scripts && ./claude-setup.sh
cd ../../systemd/scripts && ./claude-setup.sh
cd ../../iproute/scripts && ./claude-setup.sh
```

## Available Commands

| Project | Review | Debug | Verify |
|---------|--------|-------|--------|
| Kernel | `/kreview` | `/kdebug` | `/kverify` |
| systemd | `/systemd-review` | `/systemd-debug` | `/systemd-verify` |
| iproute | `/iproute-review` | `/iproute-debug` | `/iproute-verify` |

## Project Documentation

* [Kernel Review Prompts](kernel/README.md) - Linux kernel specific patterns and protocols
* [systemd Review Prompts](systemd/README.md) - systemd specific patterns and protocols
* [iproute Review Prompts](iproute/README.md) - iproute specific patterns and protocols

## How It Works

Each project has:
- **Skill file** - Automatically loads context when working in the project tree
- **Slash commands** - Quick access to review, debug, and verify workflows
- **Subsystem files** - Domain-specific knowledge loaded on demand

The skills detect your working directory and load appropriate context:
- In a kernel tree: kernel skill loads automatically
- In a systemd tree: systemd skill loads automatically
- In an iproute tree: iproute skill loads automatically

## Structure

```
review-prompts/
├── kernel/                    # Linux kernel prompts
│   ├── skills/               # Skill template
│   ├── slash-commands/       # /kreview, /kdebug, /kverify
│   ├── scripts/              # Setup script and utilities
│   ├── patterns/             # Bug pattern documentation
│   └── *.md                  # Subsystem and protocol files
│
├── systemd/                   # systemd prompts
│   ├── skills/               # Skill template
│   ├── slash-commands/       # /systemd-review, /systemd-debug, /systemd-verify
│   ├── scripts/              # Setup script
│   ├── patterns/             # Bug pattern documentation
│   └── *.md                  # Subsystem and protocol files
│
├── iproute/                  # iproute prompts
│   ├── skills/               # Skill template
│   ├── slash-commands/       # /iproute-review, /iproute-debug, /iproute-verify
│   ├── scripts/              # Setup script
│   ├── patterns/             # Bug pattern documentation
│   └── *.md                  # Subsystem and protocol files
│
└── README.md                  # This file
```

## Semcode Integration

These prompts work best with [semcode](https://github.com/facebookexperimental/semcode)
for fast code navigation and semantic search.

## License

See [kernel/LICENSE](kernel/LICENSE) for license information.
