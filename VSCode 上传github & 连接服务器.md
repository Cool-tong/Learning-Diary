### VSCode上传github

1. 在github先创建一个项目
2. 打开终端 输入 git init 会在本地自动创建一个.git文件夹
3. 终端输入 git remote add origin git@github.com:Cool-tong/xxxxx.git
4. git add 文件名 / 在vscode源代码管理处点+选文件
5. git commit -m "first commit" / 在左上角消息框填好消息、点击 √ 进行commit
6. git push -u origin master / 左下角有个按钮，推送到github（当然了，有可能因为网络的问题传不上去，大概设置完代理重启一下vscode就好了）



### VSCode连接服务器

这篇文章写的很详细 https://zhuanlan.zhihu.com/p/141205262

不过我的服务器是使用密钥连接的，按照下面的设置就可以了。

Host 服务器名字（随便）

　　HostName xxx.xxx.xx.xxx

　　User xxxx

　　Port 1234

　　IdentityFile "密钥文件路径"

