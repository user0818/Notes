# Git reset

这条命令的功能主要是将HEAD指针回退到之前的版本。但是回退会牵扯到暂存区和工作空间，所以他有三种用法，如下所示：

- `git reset –-soft`

- `git reset`
- `git reset –-hard`

假设我们有如下的提交记录，1,2,3分别代表第几次提交。

![image-20210527202559321](https://gitee.com/mw515031/image/raw/master/image/image-20210527202559321.png)

##git reset ––soft

话不多说，上图：

![image-20210527203048868](https://gitee.com/mw515031/image/raw/master/image/image-20210527203048868.png)

这个命令可以把HEAD指针指向之前的版本，但是这时工作空间和暂存区都不会改变，对他们而言，当前版本库的状态为2。所以这时候所以这时候暂存区会有已经添加但没提交的内容。

还有一种解释方式

这个命令可以把HEAD指针指向之前的版本，但是之前的版本对我们当前的工作空间来说肯定是有区别的，所以我们把所有的区别保存在暂存区中。所以就有这么一种说法：“`git reset –-soft`会回退到指定版本，将版本差异保存在暂存区中”

## git reset

![image-20210527203557246](https://gitee.com/mw515031/image/raw/master/image/image-20210527203557246.png)

这个命令在`–soft`参数的基础上，把暂存区也清空。相当于时间回到了本地修改完但还没进行任何git命令的时候（如果只回退一个版本的话）。

## git reset ––hard

![image-20210527203944359](https://gitee.com/mw515031/image/raw/master/image/image-20210527203944359.png)

类似的，`––hard`参数把工作空间、暂存区、版本库全部恢复到之前的版本。







