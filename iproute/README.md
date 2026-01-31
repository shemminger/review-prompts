# iproute2 Review Prompts

AI-assisted code review prompts for iproute2 development. Works with Claude Code
and other AI tools.

## Quick Start

```bash
cd iproute2/scripts
./claude-setup.sh
```

## Available Commands

| Command | Description |
|---------|-------------|
| `/ireview` | Deep regression analysis of patches |
| `/idebug` | Debug iproute2 issues |
| `/iverify` | Verify patch correctness |

## Project Overview

iproute2 is a collection of userspace utilities for controlling and monitoring
networking in the Linux kernel. It includes tools like `ip`, `tc`, `ss`, `bridge`,
and others. The project is tightly coupled with Linux kernel networking development.

**Important**: iproute2 is userspace code, not kernel code. While the coding style
is similar to the Linux kernel, some kernel-specific conventions do not apply.

## Repository Information

- **Stable repository**: `git://git.kernel.org/pub/scm/network/iproute2/iproute2.git`
- **Development repository**: `git://git.kernel.org/pub/scm/network/iproute2/iproute2-next.git`
- **Mailing list**: `netdev@vger.kernel.org`
- **Wiki**: https://wiki.linuxfoundation.org/networking/iproute2

## Structure

```
iproute2/
├── skills/                # Skill template for auto-loading
├── slash-commands/        # /ireview, /idebug, /iverify
├── scripts/               # Setup script
├── patterns/              # Bug pattern documentation
├── coding-style.md        # Coding style guidelines
├── json-output.md         # JSON output support
├── argument-parsing.md    # Command-line argument parsing
├── patch-submission.md    # Patch submission guidelines
├── kernel-compat.md       # Kernel compatibility
├── review-core.md         # Core review checklist
└── README.md              # This file
```

## How It Works

The skill file automatically loads context when working in an iproute2 tree.
Slash commands provide quick access to review, debug, and verify workflows.

## License

iproute2 is licensed under GPL-2.0-or-later. Some files may have dual licensing
(GPL-2.0 OR BSD-2-Clause) - check the SPDX identifier in each file.
