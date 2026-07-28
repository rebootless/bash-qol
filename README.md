# Bash-QOL

Shell quality-of-life setup for Bash: history, completion, keybindings,
zoxide/fzf hooks, and modern CLI tools (ripgrep, bat, eza). No aliases.
Optionally installs oh-my-bash.

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Bash](https://img.shields.io/badge/Bash-5.0%2B-4EAA25?logo=gnubash&logoColor=white)](https://www.gnu.org/software/bash/)
[![Debian](https://img.shields.io/badge/Debian-Supported-A81D33?logo=debian&logoColor=white)](https://www.debian.org)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-Supported-E95420?logo=ubuntu&logoColor=white)](https://ubuntu.com)

## Structure

```
bash-qol/
├── bash-qol             # entry point: flag parsing + orchestration
├── requirements.sh      # apt: bash-completion, fzf, zoxide, ripgrep, bat, eza, chafa, git
├── lib/
│   ├── paths.sh         # shared path/marker constants
│   ├── shell-config.sh  # ~/.bashrc + ~/.inputrc (history/completion/keybindings/shopt)
│   ├── omb.sh           # oh-my-bash: manual install + theme (interactive or by name)
│   └── demo.sh          # showcase of eza/bat/ripgrep + interactive features
└── README.md
```

## Usage

```
./bash-qol                                     # packages + ~/.bashrc + ~/.inputrc
./bash-qol --omb=interactive                   # + oh-my-bash, chafa theme picker
./bash-qol --omb=non-interactive --theme=NAME  # + oh-my-bash, no prompts
./bash-qol --demo                              # + showcase as a final step
./bash-qol --help
```

Flags combine freely, e.g.:

```
./bash-qol --omb=non-interactive --theme=agnoster --demo
```

Rules:
- `--theme` is only valid together with `--omb=non-interactive`.
- `--omb=non-interactive` requires `--theme=NAME`.
- Unknown theme names fail with the actual list of themes shipped in
  `~/.oh-my-bash/themes`.

## oh-my-bash integration

Only the "manual" integration is supported: oh-my-bash is git-cloned to
`~/.oh-my-bash`, and a small block is **prepended to the top** of
`~/.bashrc` — nothing else in the file is touched. There is no official
upstream-installer mode, since that replaces `~/.bashrc` wholesale.

## Idempotency

Re-running is always safe:
- `requirements.sh` — `apt-get install` no-ops on installed packages.
- `~/.bashrc` managed blocks (`# >>> bash-qol >>>` / `# >>> oh-my-bash >>>`)
  are replaced, not duplicated; each write backs up the previous file as
  `~/.bashrc.bak.<timestamp>`.
- `~/.inputrc` is replaced outright, with the same backup pattern.

## License

This project is licensed under the **GNU General Public License v3.0** — see the [LICENSE](LICENSE) file for details.
