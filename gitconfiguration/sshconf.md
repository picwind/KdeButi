# Generating a new SSH key
`ssh-keygen -t ed25519 -C "youremail@example.com"`
-t 加密类型 -C 注释
如果不支持ed25519算法，使用一下命令
`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`
回车存在默认位置/home/YOU/.ssh/ALGORITHM

# Adding your SSH key to the ssh-agent
## 后台开启ssh-agent
`eval "$(ssh-agent -s)"`
-s 生成bash命令 eval扫描两次 执行$(ssh-agent -s)生成bash命令
注：取决与你的环境，你可能需要使用不同的命令。例如，在开启ssh-agent之前运行`sudo -s -H` 如果你需要使用根权限，或者你需要使用`exec ssh-agent bash` 或`exec ssh-agent zsh`来运行ssh-agent。
sudo -H 将 HOME 变量设为目标用户的主目录，sudo -s 以目标用户运行 shell；可同时指定一条命令默认是root，shell是childshell。
eval可读取一连串的参数，而后再依参数自己的特性来执行。参数：参数不限数目，彼此之间用分号分开，可进行二次扫描，本例中先扫描执行命令 ssh-agent -s，使用其输出代替字符串，再进行二次扫描，执行字符串中的命令
shell的内建命令exec将并不启动新的shell，而是用要被执行命令替换当前的shell进程，并且将老进程的环境清理掉，而且exec命令后的其它命令将不再执行。因此，如果你在一个shell里面，执行exec ls那么，当列出了当前目录后，这个shell就自己退出了，因为这个shell进程已被替换为仅仅执行ls命令的一个进程，执行结束自然也就退出了。为了避免这个影响我们的使用，一般将exec命令放到一个shell脚本里面，用主脚本调用这个脚本，调用点处可以用bash a.sh，（a.sh就是存放该命令的脚本），这样会为a.sh建立一个sub shell去执行，当执行到exec，该子脚本进程就被替换成了相应的exec的命令。

当我们使用”ssh-agent $SHELL”命令时，会在当前shell中启动一个默认shell，作为当前shell的子shell，ssh-agent程序会在子shell中运行，当执行”ssh-agent $SHELL”命令后，我们也会自动进入到新创建的子shell中。此时，在当前会话中，我们已经可以使用ssh-agent了，ssh-agent会随着当前ssh会话的消失而消失，这也是一种安全机制

eval `ssh-agent`命令并不会启动一个子shell，而是会直接启动一个ssh-agent进程，可以看到，ssh-agent进程已经启动了，此刻，如果我们推出当前bash，此ssh-agnet进程并不会自动关闭，所以，我们应该在退出当前bash之前，手动的关闭这个进程，在当前bash中，使用ssh-agent -k命令可以关闭对应的ssh-agent进程，但是，如果在退出了当前bash以后再使用’ssh-agent -k’命令，是无法关闭对应的ssh-agent进程的，除非使用kill命令，当然，其实在使用 ssh-agent $SHELL 命令时，也可以使用’ssh-agent -k’命令关闭ssh代理。[资料](https://www.zsythink.net/archives/2407), exec ssh-agent bash是运行bash取代当前shell进程。
[childshell subshell资料1](https://segmentfault.com/a/1190000022314808),[资料2参考](https://blog.csdn.net/weixin_38316697/article/details/123110512)[资料3参考](https://cloud.tencent.com/developer/article/1948423)

## 向ssh-agent添加自己的私钥。
如果使用不同的名称创建密钥，或者添加具有不同名称的现有密钥，请将命令中的id_ed25519替换为私钥文件的名称。
`ssh-add ~/.ssh/id_ed25519`
## 添加SSH公钥到GitHub.
复制ssh公钥
`cat ~/.ssh/id_ed25519.pub`
选中并复制终端输出内容。在github账户中选中SSH and GPG keys 选项，选择new SSH key,输入标题，选中key type 为Authentication Key,并在Key一栏粘贴复制的公钥 


