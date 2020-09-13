## Introduction

This document is describing a simple neovim setup for development in C and Python with Git as SCM.

Full configuration files are available in the `files` directory:
 
 - `flake8` has to be copied to `~/.config/`
 - Files in `nvim` directory goes to `~/.config/nvim/`

A lot of information have been taken from this excellent blog post: [A Complete Guide to Setting up Neovim for Python Development on Linux](https://jdhao.github.io/2018/12/24/centos_nvim_install_use_guide_en/)

## Installation

```
sudo apt install neovim neovim-qt
```

Install vim-plug to manage plugins:

```
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

## Neovim global setup

In `~/.config/nvim/init.vim`:
```
call plug#begin('~/.local/share/nvim/plugged')

Plug 'vim-airline/vim-airline'
Plug 'davidhalter/jedi-vim'
Plug 'scrooloose/nerdtree'
Plug 'machakann/vim-highlightedyank'
Plug 'airblade/vim-gitgutter'
Plug 'majutsushi/tagbar'
Plug 'tpope/vim-fugitive'
Plug 'jsfaint/gen_tags.vim'
Plug 'ayu-theme/ayu-vim'
Plug 'neoclide/coc.nvim', {'branch': 'release'}

call plug#end()
```

To install the plugins, you need to restart neovim and run `:PlugInstall`.

To update plugins, you can run `:PlugUpdate`.

Then if you want to try the color scheme, continue with:

```
set termguicolors     " enable true colors support
"let ayucolor="light"  " for light version of theme
let ayucolor="mirage" " for mirage version of theme
"let ayucolor="dark"   " for dark version of theme
colorscheme ayu
```

For gitgutter and CoC you need these ones:

```
set updatetime=300
set signcolumn=yes
```

First will allow a interface that react fast. Second will ensure you will not
have flickering of the first column.

For my personal taste, I wanted to use a specific font in `nvim-qt` so I did put in `~/.config/nvim/ginit.vim` :

```
Guifont Noto Mono:h10
```

## Prepare for coding

### Gitgutter

This simply displays signs on the left of the buffer to signal if the code has been modified.

But most interesting part is that you can navigate through changed items by doing `]c` or `[c`

### Vim Fugitive

This is a GUI for git that runs inside vim/neovim. This is [really complete](https://github.com/tpope/vim-fugitive/blob/master/doc/fugitive.txt).

The main command I use is `:Gblame` which displays a `git annotate` result. Nice thing is that commits can be browsed
from there.

You can also just save and add the buffer to the staging area of git (read run `git add`) by doing `:Gw`.

To commit, simply run `:Gcommit`.

And to push you can use `:Gpush`.

### Conquer of Completion

In short [CoC](https://github.com/neoclide/coc.nvim) is a project that brings IDE features
to neovim.

It brings:

 - Completion
 - Linter (showing mistake in the code with explanation)
 - Code browsing

### CoC Setup

You need to setup CoC in `~/.config/nvim/init.vim`. Check `CoC` section in the file in `files/nvim/init.vim`.

#### CoC shortcut

You can easily navigate the code 

* `gd`: Go to definition
* `gy`: Go to type definition
* `gr`: Display a list of reference (users of the function/class)

And you can `K` to see a preview of documentation.

You can use `[g` and `]g` to navigate diagnostics and use `:CocDiagnostics` to get all diagnostics of current buffer in location list.

### CoC and C language

This setup is done on a Debian sid with objective being development on [Suricata](https://suricata-ids.org/)

CoC has a clangd backend that allow him to understand C code.

First we need to install dependencies:

```
sudo apt install bear clangd-10
```

To setup clangd support in `CoC`, do in neovim:

```
CocInstall coc-clangd
```

Then run `:CocConfig`:

```
{
    "clangd": {
        "path": "clangd-10"
    }
}
```

The program `bear` is used to create compilation information (flags, ...) that are
needed to clangd to know how to build your software.

Usage is simple, go to your project directory and run:

```
make clean
bear make
```

You will need to do that every time your build system is updated.

My Suricata tree is in `~/git/oisf` and it needs some specific rules
with regards to tabulation. To do this, just add in `~/.config/nvim/init.vim`:

```
autocmd BufEnter,BufNewFile,BufRead */git/oisf/* set tabstop=4 shiftwidth=4 expandtab modeline
```

### CoC and python

In neovim:

```
CocInstall coc-python
```

[Documentation of coc-python](https://github.com/neoclide/coc-python) is quite complete.

The Python interpreter is displayed in the status line. You can change it by doing:

```
:CocCommand python.setInterpreter
```

Setup flake8 as linter and if you need to setup some variable use `~/.config/flake8`.

For example to set max line length to 160:

```
[flake8]
max-line-length = 160
```
