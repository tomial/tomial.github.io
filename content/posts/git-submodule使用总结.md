---
title: "Git Submodule使用总结"
date: 2021-03-10T17:41:08+08:00
tags:
- git

categories:
- git
---

我在将博客源码上传到Github时遇到这样一个问题：<!--more-->hugo使用的主题是作者发布在github上的，我将主题fork之后作了一些修改。可以发现主题相当于另一个项目，可以单独修改和push，很符合git submodule的使用情景，所以hugo在文档里添加主题的地方都使用了git的submodule。但submodule在windows里使用有一些坑，在这篇博客里记录一下

---

一开始尝试多个主题，后面需要删除很多submodule，将submodule删除干净的方法如下：

1. 删除包含该submodule的文件夹
2. `git submodule deinit -f -- a/submodule`
3. 删除`.git/modules`里面的对应的submodule
4. `git rm -f path/to/submodule`
5. 删除在`.git/config`里面对应的内容
6. 删除该submodule在`.gitmodules`里对应的内容

---

添加submodule的方法：`git submodule add <URL> <Path>`

> *fatal: No url found for submodule path 'themes/archie' in .gitmodules* 解决方法

推测是跟windows路径格式有关，将`.gitmodules`的路径格式改为：

```toml
[submodule "themes/archie"]
	path = themes/archie
	url = https://github.com/tomial/archie.git
```



clone项目时先clone主项目，这时候submodule目录是空的，再运行：

```bash
git submodule init
git submodule update
```

即可把submodule的内容拉下来



---

参考资料：

[How do I remove a submodule?](https://stackoverflow.com/questions/1260748/how-do-i-remove-a-submodule)

[Git - 子模块](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)