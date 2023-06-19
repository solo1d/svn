- [SVN拉取命令某个目录或文件命令](#SVN拉取命令某个目录或文件命令)
- [更新本地客户端代码](#更新本地客户端代码)
- [SVN上传文件或目录到服务器](#SVN上传文件或目录到服务器)
- [SVN将本地代码导入服务器_第一次服务器内容时初始化时使用](#SVN将本地代码导入服务器_第一次服务器内容时初始化时使用)
- [SVN提交更改过的代码到服务器](#SVN提交更改过的代码到服务器)
- [查看文件或者目录状态](#查看文件或者目录状态)
- [删除文件](#删除文件)
- [查看日志](#查看日志)
- [查看版本库或文件详细信息](#查看版本库或文件详细信息)
- [将指定的文件回推退到某个版本](#将指定的文件回推退到某个版本)
- [切换项目url地址](#切换项目url地址)
- [SVN服务器搭建_MACOS](#SVN服务器搭建_MACOS)



## SVN拉取命令某个目录或文件命令

```bash
#拉取整个版本库,包括所有历史命令
$ svn checkout url网址 --username=*** --password=*** path
		url : 服务器地址
		path: 要拷贝到电脑的哪个目录下
		
	# 范例
	$ svn checkout "https://xxxxx" --username=xxx --password=xxx "/Users/jingbin/test-svn"

# 拉取服务器的最新版本内容，只有最新版本的文件会被检出，不会有历史版本的内容。
$ svn checkout --depth empty <URL>   --username=*** --password=*** path
				# <URL> 是 SVN 服务器的 URL，这会将工作副本下载到当前目录并设置深度为空
	# 范例：
	$ svn svn checkout --depth empty "https://xxxxx" --username=123 --password=Pass  路径1
			 
			 # 如果你需要查看某些文件的历史记录，你可以使用以下命令：	
			 $ svn update --set-depth=infinity  "文件名或目录"
						#其中 <file_or_folder> 是你想要查看历史记录的文件或文件夹路径。这会将深度设置为“无限”，拉取所有历史版本和当前版本的内容。
			
			


# 下载单个文件或目录
$ svn export 远程服务端文件或目录 本地路径（可为空，则下载到当前位置） --username '用户名'
	# 范例
	$ svn export "https://xxxxx" --username=xxx --password=xxx "/Users/jingbin/test-svn"



从服务器端下载代码到客户端本地
在终端中输入
$ svn checkout svn://localhost/mycode --username=mj --password=123 /Users/apple/Documents/code

我解释下指令的意思：将服务器中mycode仓库的内容下载到/Users/apple/Documents/code目录中
```

## 更新本地客户端代码

```bash
$ cd  home/code
$ svn update
```



## SVN上传文件或目录到服务器

```bash
$ svn import 本地文件或目录 远程服务端目录 --username '用户名' --password '密码' -m '添加描述(可为空)'   

	#范例
	$svn import  "/Users/本地文件或目录" "https://xxxxx" --username=xxx --password=xxx -m '新增文件'
```



## SVN将本地代码导入服务器_第一次服务器内容时初始化时使用

```bash
$ svn import /Users/apple/Documents/eclipse_workspace/weibo svn://localhost/mycode/weibo --username=mj --password=123 -m "初始化导入"

我解释下指令的意思：将/Users/apple/Documents/eclipse_workspace/weibo中的所有内容，上传到服务器mycode仓库的weibo目录下，后面双引号中的"初始化导入"是注释
```





## SVN提交更改过的代码到服务器

```bash
# 将所有修改过的内容全部同步到服务器， 根据目录来区分

#   先查看文件或者目录状态, path 可以不写
$svn status path
		#相当于 git status

$ cd  home/code
$ svn add '改动过的文件'
$ svn commit -m  "提交注释"

可以看到终端的打印信息：
	Sending        weibo/weibo/main.m
	Transmitting file data .
	Committed revision 2.
```



## 查看文件或者目录状态

```bash
$ svn status path		
#相当于 
$ git status

#path 可以不写，会列出所有修改过的文件
?：不在svn的控制中；
M：内容被修改；
C：发生冲突；
A：预定加入到版本库；
K：被锁定
```

## 删除文件

```bash
#方法一：删除和提交操作
 $ svn delete path -m "delete test fle"
 
#方法二：先删除再提交
 $ svn delete 1.log
 $ svn commit -m “”
```





## 查看日志

```bash
# 查看所有日志
$ svn log 

#查看某个文件的日志
$ svn log 		2.txt
```





## 查看版本库或文件详细信息

```bash
# 查看版本库信息
$ svn info   

# 查看某个文件或路径的信息
$ svn info path

会列出文件的修改时间和对应的提交节点内容
```







## 将指定的文件回推退到某个版本

```bash
#查看某个文件的日志

svn log 2.log# 版本号可以通过svn log查看

$ svn update -r 修正版本 文件名 #回退指定文件	
```







## 切换项目url地址

```bash
$svn info  # 获得旧的 url 地址


$ svn switch --relocate "旧的url地址"  "新的url地址"
```





## SVN服务器搭建_MACOS

```bash
# 实验环境  macos 12
我先在/User/apple目录下新建一个svn目录，以后可以在svn目录下创建多个仓库目录
打开终端，创建一个mycode仓库，输入指令：
$ svnadmin create /Users/apple/svn/mycode

指令执行成功后，会发现硬盘上多了个/Users/apple/svn/mycode目录，目录结构如下：
```

![img](assets/10002140-c4cf85b829bd477ea3a3779a4cd0d7fd.png)

```ini
配置svn的用户权限

主要是修改/svn/mycode/conf目录下的三个文件
1.打开svnserve.conf，将下列配置项前面的#和空格都去掉

把下面几个选项前面的 # 都去掉
# anon-access = read
# auth-access = write
# password-db = passwd
# authz-db = authz

说明
anon-access = read代表匿名访问的时候是只读的，若改为anon-access = none代表禁止匿名访问，需要帐号密码才能访问
```

```ini
打开config/passwd，在[users]下面添加帐号和密码，比如：
# 帐号是mj，密码是123
[users]
mj = 123
jj = 456
```

```ini
打开authz，配置用户组和权限
我们可以将在passwd里添加的用户分配到不同的用户组里，以后的话，就可以对不同用户组设置不同的权限，没有必要对每个用户进行单独设置权限。

在[groups]下面添加组名和用户名，多个用户之间用逗号(,)隔开

[groups]
topgroup=mj,jj

#说明mj和jj都是属于topgroup这个组的，接下来再进行权限配置。
#使用[/]代表svn服务器中的所有资源库

[/]
@topgroup = rw
mj = rw

#上面的配置说明topgroup这个组中的所有用户对所有资源库都有读写(rw)权限，组名前面要用@
#如果是用户名，不用加@，比如mj这个用户有读写权限
#至于其他精细的权限控制，可以参考authz文件中的其他内容
```

```bash
4.启动svn服务器

前面配置了这么多，最关键还是看能否正常启动服务器，若启动不来，前面做再多工作也是徒劳。
在终端输入下列指令： $ svnserve -d -r /Users/apple/svn
或者输入：$ svnserve -d -r /Users/apple/svn/mycode

没有任何提示就说明启动成功了
```

```bash
5.关闭svn服务器

如果你想要关闭svn服务器，最有效的办法是打开实用工具里面的“活动监视器” ，结束 svnserver 进程
```

