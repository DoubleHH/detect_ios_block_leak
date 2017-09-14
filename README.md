# 脚本简介

> 作者：huanghui  
> 检测在iOS的block（代码块）中可能存在的leak的地方。从业务上来说可能会有误检测的地方

### 大致思路

1. 找有^标识的.m文件
	ag --objc -l "\^"
2. 语法分析单个文件，使用ag命令找到文件中带有`^`的行位置，对每个位置开始解析：

	- 忽略方法定义，dispatch，动画类的一些行，满足这些条件便认为block无leak
	- 以`^`之后的`{`开始匹配，直到`{`的个数和`}`的个数相同，期间是block实现体
	- 检查block内的实现提，出现类似`self.`，`[self`等一些标识，且没有使用类似`strongify(self)`这种类似的将`self`作为一个局部变量来使用的方式，判定该block是可能存在leak的

# 使用方法

~~~shell
Usage（使用方式）: 
python find_block_leak.py code_folder

code_folder: 代码文件夹地址
~~~

#### 例子

![](http://i.niupic.com/images/2017/09/14/oxTupi.png)

# <mark>注意</mark>


#### 若提示ag命令找不到，请安装ag库。可用brew安装，命令如下：

> ### brew install ag
