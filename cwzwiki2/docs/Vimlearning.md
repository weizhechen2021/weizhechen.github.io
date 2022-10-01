![[Vim-structure.png]]
![[Vim-structure2.png]]
第一个图的Normal模式具体包括了命令模式和底线命令模式

**命令模式**
输入命令i /a /o /r / I/ A/ O/ R 进入insert模式
光标移动，复制粘贴，搜索替换，删除



**底线命令模式**
储存和离开指令，输入命令 :q /:w /:wq 


**具体的用法和语法：**
[Linux vi/vim | 菜鸟教程 (runoob.com)](https://www.runoob.com/linux/linux-vim.html)

Terminal进入vim，写脚本run.sh
```
vi run.sh
```
编辑脚本
```
#!/bin/bash
grep exon *.gtf | awk '{print $5-$4+1}' | sort -n | tail -3
```
脚本run.sh以后，在terminal中运行脚本：
```
chomod u+x run.sh #赋予脚本可执行的权限
./run.sh #运行脚本
```

由于加上了`#!/bin/bash`一行，操作系统会用/bin/bash来运行脚本 如果没有给run.sh可执行的权限，运行`./run.sh`会提示permission denied 但是如果手动指定解释器，使用`bash run.sh`，也是可以正常运行的