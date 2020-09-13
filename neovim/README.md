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

Then continue with:

```
set termguicolors     " enable true colors support
"let ayucolor="light"  " for light version of theme
let ayucolor="mirage" " for mirage version of theme
"let ayucolor="dark"   " for dark version of theme
colorscheme ayu

set updatetime=300
set signcolumn=yes
```

In `~/.config/nvim/ginit.vim` :

```
Guifont Noto Mono:h10
```

## Prepare for coding

### CoC Setup

You need to setup CoC in `~/.config/nvim/init.vim`. Check `CoC` section in the file in `files/nvim/init.vim`.

### CoC and clangd

```
sudo apt install bear clangd-10
```

```
make clean
bear make
```


```
CocInstall coc-clangd
```

Run CocConfig

```
{
    "clangd": {
        "path": "clangd-10"
    }
}
```

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

Setup flake8 as linter and if you need to setup some variable use `~/.config/flake8`.

For example to set max line length to 160:

```
[flake8]
max-line-length = 160
```
