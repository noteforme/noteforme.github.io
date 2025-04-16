---
title: linux
date: 2017-07-19 15:03:07
tags:
categories: "LINUX"

---

##### Linux使用

1. find  ： $ find . -name 'my*'   搜索当前目录（含子目录，以下同）中，所有文件名以my开头的文件。 http://www.ruanyifeng.com/blog/2009/10/5_ways_to_search_for_files_using_the_terminal.html

##### ubuntu　wifi热点

http://blog.csdn.net/sunmc1204953974/article/details/45740853

##### 删除build文件

`find . -name "build"  | xargs rm -rf`

# VIM

## vim-plug Installation

```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

https://github.com/junegunn/vim-plug

https://www.bilibili.com/video/BV1NG4y1p74h?spm_id_from=333.788.videopod.episodes&vd_source=d4c5260002405798a57476b318eccac9&p=18

https://www.bilibili.com/video/BV1sPAAeUE55/?spm_id_from=333.788.player.switch&vd_source=d4c5260002405798a57476b318eccac9&p=8

## coc.nvim plugin

[GitHub - neoclide/coc.nvim: Nodejs extension host for vim &amp; neovim, load extensions like VSCode and host language servers.](https://github.com/neoclide/coc.nvim)

~/.vimrc

```bash
call plug#begin()

" Use release branch (recommended)
Plug 'neoclide/coc.nvim', {'branch': 'release'}

call plug#end()
```

[GitHub - neoclide/coc.nvim: Nodejs extension host for vim &amp; neovim, load extensions like VSCode and host language servers.](https://github.com/neoclide/coc.nvim)

https://www.youtube.com/watch?v=7-dfpQ5sexk

```bash
sudo npm i -g yarn   
cd .vim/plugged/coc.nvim
yarn install
cd ~

vim .vimrc
CocInstall coc-clangd
```

```bash
CocInstall coc-clangd
CocCommand clangd.install
```

## Search

### 1. **Move to the Next Match:**

After the first match is found, to move to the next match, press:

```bash
n
```

This will move the cursor to the next occurrence of the search pattern.

### 2. **Move to the Previous Match:**

If you want to move to the previous match, press:

```
N
```

### 3. **Search Backwards:**

If you want to search backward (from your current cursor position toward the beginning of the file), use:

```
?your_pattern
```

Then, use `n` to move to the previous match (in the reverse direction) and `N` to move in the opposite direction.