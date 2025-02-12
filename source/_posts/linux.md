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

## comment

Once you've selected the lines, press `:` to bring up the command line, and Vim will automatically prepend the `:'<,'>` range (which represents the selected lines).

```
:s/^/\/\/ /
```

uncomment

```
:s/^\/\/ //
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