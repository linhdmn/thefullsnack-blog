---
title: Dùng neovim thay thế vimdiff
published: true
date: 2016-08-03 18:55:10
tags: vim, git
description: **neovim** cũng có chế độ diff tuy nhiên cách cài đặt nó để thay thế cho **vimdiff** hơi rườm rà một tí. Để tiết kiệm thời gian thì mình share lên đây luôn. 
image:
---
Nếu xài Git và **vim** hẳn các bạn biết tool **vimdiff**, dùng để xem diff và merge code. 

**neovim** cũng có chế độ diff tuy nhiên cách cài đặt nó để thay thế cho **vimdiff** hơi rườm rà một tí. Để tiết kiệm thời gian thì mình share lên đây luôn. 

Các bạn có thể copy phần cấu hình dưới đây vào file `~/.gitconfig`:

```
[merge]
  tool = vimdiff
[mergetool]
  prompt = true
[mergetool "vimdiff"]
  cmd = nvim -d $LOCAL $REMOTE $MERGED -c '$wincmd w' -c 'wincmd J'
[difftool]
	prompt = false
[diff]
	tool = vimdiff
```

Từ bây giờ có thể gõ: 

```
git difftool <blah-blah>
```

Để sử dụng **neovim diff**. Nếu lười thì các bạn có thể đặt alias như sau:

```
git config --global alias.d vimdiff
```

Rồi thay vì gõ `git difftool`, chúng ta có thể gõ:

```
git d <blah-blah>
```

Happy Vimming ^^
