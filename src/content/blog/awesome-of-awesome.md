---
title: Awesome of Awesome
author: Hazy
pubDatetime: 2026-01-15
featured: false
draft: false
tags:
  - digital-minimalism
  - productivity
  - security
description: "A living map of the tools I use to think, build, and ship."
---

## TLDR
It’s a snapshot of what actually runs my day, the things that sit between an idea and something real.

### Base Layer
- MacOS with [Homebrew](https://brew.sh/) as the main package manager.
- [Raycast](https://www.raycast.com/) replaces Spotlight.
    - Disable shortcut to launch the Spotlight Command (⌘) - Space in Keyboard > Keyboard Shortcuts > Spotlight
- [iTerm2](https://iterm2.com/) as the main terminal
    - Use zsh
    - Use [starship](https://starship.rs/). 
        - ```brew install starship```
        - ```echo 'eval "$(starship init zsh)"' >> ~/.zshrc```
    - Use [catppuccin](https://github.com/catppuccin/catppuccin) as the default profile.
    - [Catppuccin theme](https://github.com/catppuccin/iterm) for iTerm2 

###  Install applications via brew

- ```brew install git gh```
- ```brew install go ruby pipx node```
- ```brew install nmap```
- ```brew install ffuf```
- ```brew install ripgrep```


### Install via go

- ```echo 'export GOPATH="$HOME/go"' >> ~/.zshrc```
- ```echo 'export PATH="$PATH:$GOPATH/bin"' >> ~/.zshrc```
- ```source ~/.zshrc```
- ```go install -v github.com/projectdiscovery/pdtm/cmd/pdtm@latest```



### Notes

- Markdown
- Obsidian / Notion

### Others
- Telegram for the main communication channel, group gosip with friends, archive some notes. Have a channel to stay updates
- Clop for clipboard optimiser
- Brave, Firefox for the browsers
