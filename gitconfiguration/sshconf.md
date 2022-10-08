#Generating a new SSH key
`ssh-keygen -t ed25519 -C "youremail@example.com"`
-t 加密类型 -C 注释
如果不支持ed25519算法，使用一下命令
`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`
回车存在默认位置/home/YOU/.ssh/ALGORITHM

#Adding your SSH key to the ssh-agent
##后台开启ssh-agent
`eval "$(ssh-agent -s)"`
-s 生成bash命令 eval扫描两次 执行$(ssh-agent -s)生成bash命令
注：取决与你的环境，你可能需要使用不同的命令。例如，在开启ssh-agent之前运行`sudo -s -H` 如果你需要使用根权限，或者你需要使用`exec ssh-agent bash` 或`exec ssh-agent zsh`来运行ssh-agent。

##向ssh-agent添加自己的私钥。
如果使用不同的名称创建密钥，或者添加具有不同名称的现有密钥，请将命令中的id_ed25519替换为私钥文件的名称。
`ssh-add ~/.ssh/id_ed25519`
##添加SSH公钥到GitHub.
复制ssh公钥
`cat ~/.ssh/id_ed25519.pub`
选中并复制终端输出内容。在github账户中选中SSH and GPG keys 选项，选择new SSH key,输入标题，选中key type 为Authentication Key,并在Key一栏粘贴复制的公钥 


