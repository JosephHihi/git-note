git using note是针对学习git用法中，进行的错误操作、问题分析、和解决方案的记录
操作1：git add readme.txt
问题：fatal: pathspec 'readme.txt' did not match any files
解决：先touch readme.txt 即新建readme.txt 文件，否则该文件都还没被创建，不能被添加到暂存区
     再git add readme.txt

操作2：git commit readme.txt
问题：命令有误
解决： git commit -m 'readme.txt'

操作3： git commit -m 'readme.txt'
问题： Unable to create 'D:/github/testgit/.git/index.lock': File exists.
Another git process seems to be running in this repository, e.g.
an editor opened by 'git commit'. Please make sure all processes
are terminated then try again. If it still fails, a git process
may have crashed in this repository earlier:
remove the file manually to continue.
解决：在项目根目录下找到 .git 文件夹。打开该文件夹。找到文件夹里面的index.lock 文件，将其删除，即可解决问题（如果提示不能删除文件，就重启系统）
要想删除文件，应该使用命令：rm index.lock 然后 git commit 提交


操作4：返回上一级目录
问题：如何返回上一级目录？
解决：输入命令 cd ..

操作5：寻找.ssh文件并检查
问题：输入ssh代码后，找不到文件？
解决：在命令行所给提示中有路径

操作6：在c/目录下进行git remote add origin https://github.com/JosephHihi/testgit.git
关联本地和远端
问题：fatal: not a git repository (or any of the parent directories): .git
解决：更换目录，切换到本地库目录

操作7：$ git push -u origin master
问题：跳出的窗口无登陆密码框，而是提示：connect to Github sign in with your brower
解决：（1）关闭含有信息：connect to Github sign in with your brower的弹出框
（2）此时出现一个ssh弹出框，显示：
‘https://github.com’: 输入的是github上的邮箱账号, 而不是github中设置的username
Password for ‘https://你的github邮箱@github.com’: 输入github的登录密码,点击enter键即可
（3）此时命令行中显示进程和百分比完成度等多行信息

操作8：$ git push -u origin master
问题：fatal: unable to access 'https://github.com/JosephHihi/testgit.git/': Failed to connect to 127.0.0.1 port 1080 after 2088 ms: Connection refused
解决：发生这种情况是因为代理是在git中配置的。既然它是https代理（而不是http）git config http.proxy和git config --global http.proxy也无济于事。
解决方案一
1、看看你的git配置
git config --global -l
如果你没有任何与https代理相关的内容，例如https_proxy = …问题不在这里。
如果您有与https代理相关的内容，请将其从〜/ .gitconfig文件中删除，然后重试。
2、如果仍然不起作用，请取消设置环境变量
env|grep -i proxy
你应该有一行或几行https_proxy = …
使用以下内容逐个取消设置：取消设置https_proxy（或HTTPS_PROXY，具体取决于变量的名称）
3、再次检查环境变量
env|grep -i proxy
如果它没有显示任何你应该是好的。
注意：此解决方案可以应用于http和https代理问题。只是变量名称从https更改为http。

解决方案二
在开启shadowsocks的前提下，手动配置git的代理。git客户端输入如下两个命令就可以了。
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy http://127.0.0.1:1080
http://也可以改成sockets5://,但是区别在于：socks5不支持通过pubkey免密登录github，每次提交代码只能输入用户名和密码。http可以支持免密登录。
取消代理：
git config --global --unset http.proxy
git config --global --unset https.proxy
其实方案一和方案二是同一种方法，不过方案二更加具体一点罢了，大部分问题都可以用方案二解决，当方案二无效时，考虑使用方案一。

操作9：应用了操作8的方法，结果还是不行
解决：采用操作9中取消代理的两行代码，重新进入了操作7，并且点击brower进入浏览器网页之后可以输入账号密码，然后输入账号密码，结果发现原来是账号输错了。
