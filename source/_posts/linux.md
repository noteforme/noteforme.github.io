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



## debug



requirement 

vim > 8.1

need gdb installed



cloop.c

```c
#include <stdio.h>

int main() {
    for (int i = 1; i <= 10; i++) {
        printf(format: "%d\n", i);
    }
    return 0;
}
```

compile

```bash
gcc -g cloop.c -o cloop
```

g++  works too



plugin load

命令模式下

```bash
packadd termdebug
Termdebug cloop
```



```bash
b 5
run
p i   // check value i 

- file bin   加载名为 bin 的二进制文件 
- CTRL-C     中断程序
- run/r      运行
- next/n     执行当前行，停在下一行 （step over）
- step/s     执行当前行，进入下一层函数 （step in）
- finish     执行直至离开当前函数
- where      显示栈
- continue/c 继续执行
- break/b N  在第 N 行加断点
- break/b f  在函数 f 处加断点
- delete     删除所有断点
```

https://sourceware.org/gdb/current/onlinedocs/gdb/



https://medium.com/swlh/debugging-c-programs-in-vim-683ec257b9e2

[Vim TermDebug Plugin - YouTube](http://youtube.com/watch?v=Hm2RgjQGp2s)

[Vim Techniques 03 - GDB Debugging right inside Vim - YouTube](https://www.youtube.com/watch?v=jXIG4kz8Pdo)



prepare nodejs
=======

```bash
sudo npm i -g yarn   
cd .vim/plugged/coc.nvim
yarn install
cd ~
vim .vimrc
CocInstall coc-clangd
CocCommand clangd.install
```

## reformat code

As soon you open a file in VIM, just press the "g" key twice. This should move the cursor to the beginning of the file. Then hit the "=" key. Finally, hold "shift" and hit the "g" key again.

https://www.jimlynchcodes.com/blog/vim-auto-indenting-with-gg-g#:~:text=As%20soon%20you%20open%20a,the%20%22g%22%20key%20again.

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