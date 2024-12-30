---
tags:
  - Tools
  - Git
Date: 2024-12-30
---
# 版本控制之Git
---
>Git可以实现版本控制，便于多人协助开发
## 数据模型
---
### 快照
Git 将顶级目录中的文件和文件夹作为集合，并通过一系列快照来管理其历史记录。在 Git 的术语里，文件被称作 Blob 对象（数据对象），也就是一组数据。目录则被称之为“树”，它将名字与 Blob 对象或树对象进行映射（使得目录中可以包含其他目录）。快照则是被追踪的最顶层的树。像下面一样
```c
<root> (tree)
|
+- foo (tree)
|  |
|  + bar.txt (blob, contents = "hello world")
|
+- baz.txt (blob, contents = "git is wonderful")

```
## 关联快照
在 Git 中，历史记录是一个由快照组成的有向无环图。
```
o <-- o <-- o <-- o <----  o 
            ^            /
             \          v
              --- o <-- o
```
其中每一个o都是一个commit,一个commit可以有多个历史版本。