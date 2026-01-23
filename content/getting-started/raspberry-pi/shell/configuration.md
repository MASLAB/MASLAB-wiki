+++
date = '2026-01-22T17:53:36-05:00'
draft = false
title = 'Configuration'
+++

# Configuration
This section details **optional** instructions to install a **[neovim and tmux](../../shell#neovim)** configuration. The install script below will install the "Tmux Plugin Manager," [TPM](https://github.com/tmux-plugins/tpm), and a **Neovim** configuration called [NvChad](https://nvchad.com). 

Configurations are a personal preference; the configuration script provided here is based on the staff's personal experience.

## Install Script

Execute the two commands below in your terminal to install the **tmux** and **Neovim** configuration files.

```
git clone https://github.com/MASLAB/shell-config.git shell-config && cd shell-config
./install.sh
```

> [!CAUTION]
> These configuration files change your **[tmux prefix](https://github.com/tmux/tmux/wiki/Getting-Started#the-prefix-key)** from `^B` to `^SPACEBAR`.

After installing, open **Neovim** by executing `nvim` and allow the plugins to install.
