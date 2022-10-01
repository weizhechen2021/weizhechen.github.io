**DOCKER运行**
- docker info
    - 可以知道images、version、running、paused、stopped
- docker 
打开Terminal，不用重新从镜像创建容器（本来就创建了）
如果输入命令的时候出现：
```
#命令如docker info 或者docker load
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.37/info: dial unix /var/run/docker.sock: connect: permission denied

#输入下列命令解决
sudo gpasswd -a username docker
newgrp docker  #更新docker组

```
运行Docker：
```
docker exec -it bioinfo_tsinghua bash #容器的使用
exit #退出回到Terminal
```
Docker里面有什么东西：
alter-spl
chip-seq
gsea
linux
plot
software
blast
diff-exp
homer
mapping
share


**一些问题**
head file 和 cat file | head 有什么区别？
sed和awk是不是只是控制输出道终端的时候我看到的样子，并不会更改文件的内容？


**一些小点**
- "`|`"是管道命令操作符，它可以将左边命令传出的正确输出信息（standard output）作为右边命令的标准输入（standard input）。




**在2.2中常用的命令+用法：**
`cut`取出文件中的特定列或字符，默认分隔符是\t
- `cat file_name | cut -f 1,2,3 | head` 提取文件1，2，3列
- `cut -f 1,2,3 file_name`  提取文件1，2，3列

`sort`排序
- `sort -n`
`uniq`去重复
- `uniq -c`
`chmod`修改文件的访问权限

`head`  前10行
- `head -n`  前n行；；’、
`tail`  后10行
- `tail` 后10行

`ls` 输出当前目录
- `ls -lh` file_name 显示要查看文件大小

`cat`直接查看文件
- `cat file_name | head` 显示文件前10行
- `cat file_name | tail`  显示文件后10行
- `cat file_name | head -n` 显示文件前n行

`grep`文件中关键词搜索，返回行
- `grep "字符串" file_name` 提取关键词所在行
- `grep -v "字符串" file_name` 提取无关键词所在行
- `grep -v '^$'` 排除无意义的空白行，提取其它
- `grep exon file_name` 提取出现”exon“的行（但是awk $3== ”exon“ 会更准确， 确认筛选出的是feature == exon， 而不是其它位置出现exon也输出）
- `grep -v "#" file_name | wc -l `排除comment line(以#开头的部分)，计算其它行数
- `grep -v '^#' file_name | head -n` 排除comment line(以#开头的部分)，显示其它前n行
- `grep -v "#" file_name | grep -v '^$' | wc -l `排除comment line(以#开头的部分)和无意义的空白行，计算行数
- `grep -v '^#' file_name |awk '{print $3}'| sort | uniq -c  ` 计数有多少类feature 
- `?????grep -v '^#' file_name |awk '{print $3}'| sort | uniq `  
#试一下最后一个命令，uniq的用法

`wc`查看文件行数、字数
- `wc -l` file_name 输出文件行数

`awk`过滤筛选，默认分隔符为“ ”和 \t
- `awk '{ xxxx }' `awk使用格式
- `cat file_name | awk '{ print $1, $2, $3 }' | head `把文件内容按分隔符分列，筛选出1、2、3列，显示前面结果
- `awk '{if($0!=" ") print}' file_name | head`  过滤空行，显示前面结果
- `cat file_name | awk '$3 =="gene" { print $1, $3, $9 } ' | head` 提取第三列为gene的行，并只显示1，3，9列信息
- `cat file_name | awk ' { print $3, $5-$4 + 1 } ' | head`
- `cat file_name | awk '$3 =="gene" { len=$5-$4 + 1; size += len; print "Size:", size } ' | tail -n 1`计算所有gene总长度
- `cat file_name | awk '$3 =="CDS" { len=$5-$4 + 1; size += len; print "Size:", size } ' | tail -n 1` 计算所有CDS总长度
- `awk 'BEGIN {s = 0;line = 0 } ;$3 =="CDS" && $1 =="I" { s += ($5 - $4);line += 1}; END {print "mean=" s/line}' file_name` 计算1号染色体CDS平均长度
- 
综合使用：
- `cat file_name | awk '$3 == "gene"{split($10,x,";");name = x[1];gsub("\"", "", name);print name,$5-$4+1}' | head`
- `grep exon file_name | awk '{print $5-$4+1}' | sort -n | tail -3 > new_file_name`  
- `less new_file_name.txt` 查看 new_file_name.txt

`>  file1` 如果file1本身存在 新输入会覆盖原file1
`>>  file1` 如果file1本身存在 新输入会追加到原file1


#为什么要tail -n 1
#是否可以awk file_name


2.2 章学习内容：**
- 查看文件基本信息
- 数据提取
	- 筛选特定列
		- `cat file_name | awk '{ print $1, $2, $3}' `
		- `cut -f 1,2,3 file_name`
		- `cat file_name | cut -f 1,2,3`
	- 筛选特定行
		- `cat file_name | awk $3 =="gene" '{print $1-$3}'`
- 提取计算特定feature
	- 提取并统计feature类型
		- `grep -v '^#' file_name | awk '{ print $3 }' | sort | uniq -c`
	- 计算特定feature长度
		- 
	- 分离并提取基因名字
- 提取数据并存入新文件
	- 命令处理数据 > 写入文件名 （若无 新建，若有 覆盖，>>追加不覆盖）
		- `grep exon 1.gtf  > 1.txt`
	- 查看文件 
		- `less file_name`
		- `q` 退出
		- `vi file_name`
		- `:q` 退出
	- 寻找长度最长的3个exon, 汇报其长度
		- way1
			-  `grep exon 1.gtf | awk '{print $5-$4+1}' | sort -n | tail -3 > 1.txt`
		- way2-vim
			- ![[Vim.png]]


**作业**
-   1. 解释1.gtf文件中第4、5列代表什么，exon长度应该是$5-$4+1还是$5-$4？
	- $5-$4+1
-   2. 列出1.gtf文件中 XI 号染色体上的后 10 个 CDS （按照每个CDS终止位置的基因组坐标进行sort）。
	- `cat 1.gtf | awk '$3=="CDS"' | sort -k 5 -n | tail 
-   3. 统计 IV 号染色体上各类 feature （1.gtf文件的第3列，有些注释文件中还应同时考虑第2列） 的数目，并按升序排列。
	-  `grep -v "^#" 1.gtf | awk '{print $3}' | sort -n | uniq -c sort -n`
-   提交word/md/txt/sh文件均可，第2，3题要求给出结果，并附上使用的命令

 
 
 
 
 
 
 
 
 
 
 sed
```
grep -v "#" 1.gtf | grep -v '^$' | wc -l #用grep -v排除comment line(以#开头的部分)以及无意义的空白行

grep -v '^#' 1.gtf |head -10 #过滤开头注释行comment line(以#开头的部分)并显示前10行

awk '{if($0!=" ") print}' 1.gtf | head -10 #过滤空行，显示前十行结果。

sed -i "s/被替换字符串/替换成字符串/a" file_name
sed -i "2a 插入的字符" file_name
sed '2,5s/原字符串/替换字符串/g' #替换2到5行

sed '/^$/d' file#删除空白行：
sed '2d' file #删除文件的第2行：
sed '2,$d' file #删除文件的第2行到末尾所有行：
sed '$d' file #删除文件最后一行：
sed '/^abc/'d file #删除文件中所有开头是test的行：

^表示空
$表示 一直到
$1 表示第一列

```

[docker随笔7--常用的shell命令sed与awk - callmelx - 博客园 (cnblogs.com)](https://www.cnblogs.com/callmelx/p/11049669.html)


cat和grep 是用于提取文件的，sed 是编辑文件，awk类似于筛选

sort
uniq -c 去重复计数