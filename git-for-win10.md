git Bash中的复制与粘贴
`Ctrl+insert`  复制
`Shift+insert` 粘贴
### 0 我的尝试
```
git config --global user.name "Ireneyfc"
git config --global user.email "irene6yfc@gmail.com"
git config --list
cd D:
mkdir git_repository
ls


```
### 1 初始配置
#### 1.1 申请一个github账号
因为git和github是穿一条裤子的，所以有必要先申请一个github的账号，然后记下你的用户名和你申请账号时用的邮箱（即是你的gihub登陆账号）
#### 1.2 在git上配置你的用户名和邮箱
打开 **git bash**,输入以下两条命令

```
git config --global user.name "yourname"
git config --global user.email "youremail"
```
这里有几个注意事项：
- yourname处填写你的github用户名，youremail处填写你的github账号
- global前面是有两条横杠的


你可以使用下面这条命令来查看当前的所有配置

```
git config --list
```
如果你发现用户名或者email手贱打错了，还是用上面那两条指令重新设置就可以了

#### 1.3 在本地创建一个保存git项目的文件夹
我专门建了一个文件夹用于保存那些要使用git来管理版本控制的项目
```
cd \e      //转到E盘目录下
mkdir git_repository  //新建一个名叫git_repository的文件夹
```

当然你也可以直接使用eclipse等IDE的project location中的项目文件夹做版本控制，这时候就不需要进行这一步

#### 1.4 初始化
在你的项目文件夹下执行下面这个指令初始化git，这个指令会在当前文件夹下新建一个名叫.git的文件夹，里面放置的都是git进行版本控制的文件，这个文件夹是一个隐藏文件夹，需要点击文件夹上方的“查看”，勾选“隐藏的项目”才能看到它
```
git init
```
#### 1.5 往仓库里添加要进行版本控制的文件夹

```
git add .
```
一定要通过-m参数来加上提交的描述信息，不然会被认为不合法
```
git commit -m  "备注信息"
```



#### 1.6 查看当前文件状态
执行下面这条命令可以看见已经提交的文件和缓存区中有什么文件
```
git status
```


#### 1.7 查看修改部分
假设现在你在本地仓库里修改了文件，commit后可以查看修改了什么内容，执行下面这条指令即可
```
git diff
```
被修改的内容会用绿色字标出来


### 2 和服务器同步
因为我没有其他服务器，所以只用了github为我提供的，github为每个免费用户提供了300M的服务器空间，那么要怎么做才能同步呢？

#### 2.1 初始化操作

首先你得先生成一个ssh密钥，请务必执行下面这些语句：
```
cd ~/.ssh                             //转到C：\用户\你的用户名\.ssh文件夹
ssh-keygen -C "XX@gmail.com" -t rsa   //生成ssh密钥，这条语句会在.ssh文件夹中生成两个必要的文件：id_rsa和id_rsa.pub，
                                     //这是用rsa算法生成的公钥和私钥
```
其中-C后面那个邮箱使用你自己的github账号
执行完上面两条语句后一路按ENTER键即可，然后`Ctrl+insert`复制生成的密钥：

然后在浏览器上登陆你的github账户，然后点击右上角的头像，选择Your profile


然后点击右上角的Edit profile


然后再左边菜单栏选择SSH and GPG keys
进去之后在SSH keys那里点击右上角的new SSH keys
然后按ctrl+v把你的密钥复制进去，title可以随便命名，然后点击add，这时候github会验证你是不是本人让你重新输入登录密码，输完就可以了。
至此本地和github上的初始化操作就算完成了，下面就来连接一下两者

#### 2.2 连接本地和github服务器
执行下面这条指令连接github
```
ssh -T git@github.com
```
如果连接成功会提示"You've successfully anthenticated"

#### 2.3 将服务器上的项目同步到本地仓库
如果本地一个项目文件都没有，需要将服务器上的整个项目同步到本地，请使用下面这个指令：
```
git clone 你的github项目的网址
```
这样就会把整个项目保存在本地仓库中了

如果是本地和服务器都有项目文件，只是想获取本地没有的文件，那么先执行下面这条语句：
```
git remote add origin 你的github项目的网址.git
```
注意要在网址后面加上.git，否则会失败，origin是远程主机的名字，你可以把它理解为当前项目的别名你也可以起其他名字，origin是git上原本存在的一个名字，这个步骤会弹出一个窗口让你登陆github，完毕之后就将origin和你的项目匹配起来了

然后有两种方式可以同步github上的项目到本地
一种是使用fetch指令
```
git fetch origin
```
fetch之后你会发现此时在你本地文件夹中找不到想要同步的项目，因为此时它还在缓存区，你要把它和当前分支合并才行，执行下面这条指令合并分支
```
git merge origin/master
```
执行完毕之后你就能在本地文件夹看到它了
这里的master是本地仓库的主分支

还有一种更简单的方式，使用pull指令，这样一条指令就够了
```
git pull origin master
```
这条指令等价于上一种方法的两条指令的效果

#### 2.4 将本地项目同步到github服务器
使用push指令将本地项目同步到服务器
```
git push origin master
```
这里的master是你github上的项目主分支master


### 3 常用命令
```
git help <verb> //查看git的命令手册
git help config  //查看配置命令
git log //查看在历史日志
git branch -a   //查看当前所有分支
git branch -d 分支名   //删除某个分支
git checkout 分支名    //切换到某个分支
touch 文件名 //在本地新建一个空白文件
git add .文件后缀   //将当前目录中的所有.文件后缀的文件加入到缓存区
```

