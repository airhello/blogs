# make FAQ
## 查找顺序
- Makefile查找顺序为"GNUmakefile” -> "makefile" -> "Makefile"  
- 默认的情况下, make会在工作目录(执行make的目录)下按照文件名顺序寻找makefile文件读取并执行  
- 通常应该使用"makefile"或者"Makefile"作为一个makefile的文件名, 而"GNUmakefile"是我们不推荐使用的文件名, 因为以此命名的文件只有"GNU make"才可以识别，而其他版本的make程序只会在工作目录下"makefile"和“Makefile"这两个文件。

## 控制函数(error/warning/info)
可用于Makefile调试，主要用法如下：
- 错误   
\$(error text...)
- 警告   
\$(warning text...)
- 打印   
\$(info text...)
- 例子  
show_vars = \$(info \$(foreach v,\$1,\$v='\$(value \$v)'))

## foreach函数
- 语法  
\$(foreach <var\>,<list\>,<text\>)，其中text是包含var的变量

- 例子  
names := hello world  
files := \$(foreach n,\$(names),\$(n).o)
