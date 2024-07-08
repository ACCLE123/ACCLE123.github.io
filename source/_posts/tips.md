---
title: tips
date: 2024-05-28 22:15:55
tags:
---

### .zshrc

将terminal的前面缩短

` export PROMPT='%1~ %# ' `

git & docker

```bash

alias ll="ls -al"
alias git-log="git log --pretty=oneline --all --graph --abbrev-commit"
alias docker-ps="docker ps --format 'table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}'"
alias docker-ps-a="docker ps --format 'table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}' -a"

```
