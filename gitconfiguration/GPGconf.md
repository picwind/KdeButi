gpg --full-generate-key

在提示时，指定要生成的密钥类型，或按 Enter 键接受默认值。
在提示时，指定要生成的密钥大小，或按 Enter 键接受默认值。 密钥必须至少是 4096 位。
输入密钥的有效时长。 按 Enter 键将指定默认选择，表示该密钥不会过期。 除非你需要过期日期，否则我们建议接受此默认值。
验证您的选择是否正确。
输入您的用户 ID 信息。(与github一致)
输入安全密码。
使用 gpg --list-secret-keys --keyid-format=long 命令列出你拥有其公钥和私钥的长形式 GPG 密钥。
从 GPG 密钥列表中复制您想要使用的 GPG 密钥 ID 的长形式。 在本例中，GPG 密钥 ID 为 `3AA5C34371567BD2`： ```shell{:copy} $ gpg --list-secret-keys --keyid-format=long /Users/hubot/.gnupg/secring.gpg ------------------------------------ sec 4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10] uid Hubot ssb 4096R/42B317FD4BA89E7A 2016-03-10 ```
粘贴下面的文本（替换为您要使用的 GPG 密钥 ID）。 在本例中，GPG 密钥 ID 为 3AA5C34371567BD2：

`gpg --armor --export 3AA5C34371567BD2`
Prints the GPG key ID, in ASCII armor format
复制以 -----BEGIN PGP PUBLIC KEY BLOCK----- 开头并以 -----END PGP PUBLIC KEY BLOCK----- 结尾的 GPG 密钥。
将 GPG 密钥新增到 GitHub 帐户。
***
#Telling Git about your GPG key
打开终端。
使用 gpg --list-secret-keys --keyid-format=long 命令列出你拥有其公钥和私钥的长形式 GPG 密钥。签名提交或标记需要私钥。
`gpg --list-secret-keys --keyid-format=long`
从 GPG 密钥列表中复制您想要使用的 GPG 密钥 ID 的长形式。 在本例中，GPG 密钥 ID 为 3AA5C34371567BD2：
若要在 Git 中设置 GPG 签名主键，请粘贴下面的文本，替换要使用的 GPG 主键 ID。 在本例中，GPG 密钥 ID 为 3AA5C34371567BD2：
`git config --global user.signingkey 3AA5C34371567BD2`
可以设置Git为每次commit自动要求签名
`git config --global commit.gpgsign true`
信任Github的GPG密钥
git log --show-signature试试查看本地的某个Git仓库的commit记录和签名信息：
```
$ git log --show-signature
# some output is omitted
commit ec37d4af120a69dafa077052cfdf4f5e33fa1ef3 (HEAD -> master)
gpg: Signature made 2019年08月 4日 12:52:29
gpg:                using RSA key 1BA074F113915706D141348CDC3DB5873563E6B2
gpg: Good signature from "fortest <test@test.com>" [ultimate]
Author: keithnull <keith1126@126.com>
Date:   Sun Aug 4 12:52:29 2019 +0800

    test GPG

commit 6937d638d950362f73bfbf28bc4a39d1700bf26b
gpg: Signature made 2019年07月24日 15:58:46
gpg:                using RSA key 4AEE18F83AFDEB23
gpg: Can't check signature: No public key
Author: Keith Null <20233656+keithnull@users.noreply.github.com>
Date:   Wed Jul 24 15:58:46 2019 +0800

    Initial commit
```
可以发现，虽然所有的commit在Github中查看都是Verified，但是有一些比较特殊：在Github网页端进行的操作，比如创建仓库。这些commit并没有用我们之前生成的密钥进行签名，而是由Github代为签名了。这样的结果就是，我们本地无法确认这些签名的真实性。

为了解决这个问题，我们需要导入并信任Github所用的GPG密钥。
导入Github公钥
`curl https://github.com/web-flow.gpg | gpg --import`
然后是信任（用自己的密钥为其签名验证，需要输入密码）：
```gpg --sign-key 4AEE18F83AFDEB23```
[知乎使用 GPG Key 来构建签名、加密及认证体系](https://zhuanlan.zhihu.com/p/481900853)
我们把对密钥(Public Key)进行签名叫做 Certify。对于Certify，gpg 提供的命令选项是 --sign-key。





引用自[知乎在Github上使用GPG的全过程](https://zhuanlan.zhihu.com/p/76861431)

***
#使用GPG密钥进行SSH身份验证
##创建GPG子密钥对
```
gpg2 --expert --edit-key <key ID|Fingerprint|uid>
gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
Your selection? 8

Possible actions for a RSA key: Sign Encrypt Authenticate
Current allowed actions: Sign Encrypt

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? s
Your selection? e
Your selection? a

Possible actions for a RSA key: Sign Encrypt Authenticate
Current allowed actions: Authenticate

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? q
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (4096)
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N) y
Really create? (y/N) y

sec  rsa2048/8715AF32191DB135
     created: 2019-03-21  expires: 2021-03-20  usage: SC  
     trust: ultimate      validity: ultimate
ssb  rsa2048/150F16909B9AA603
     created: 2019-03-21  expires: 2021-03-20  usage: E  
ssb  rsa2048/17E7403F18CB1123
     created: 2019-03-21  expires: never       usage: A  
[ultimate] (1). Brian Exelbierd

gpg> quit
Save changes? (y/N) y
```
##启用GPG子密钥对
启用GPG子密钥对用于SSH身份验证步骤如下：
让gpg-agent处理SSH请求
选定用于SSH的子密钥对
告诉ssh如何访问gpg-agent
###让gpg-agent处理SSH请求
***让gpg-agent处理ssh请求***，需要在文件~/.gnupg/gpg-agent.conf添加enable-ssh-support
```
#  ~/.gnupg/gpg-agent.conf
enable-ssh-support
```
###选定用于SSH的子密钥对
***预先指定要用于SSH的密钥，这样就不必使用ssh-add来加载密钥***
先找密钥对的keygrip
```
gpg2 --list-keys --with-keygrip
/home/bexelbie/.gnupg/pubring.kbx
------------------------------
sec   rsa2048 2019-03-21 [SC] [expires: 2021-03-20]
      96F33EA7F4E0F7051D75FC208715AF32191DB135
      Keygrip = 90E08830BC1AAD225E657AD4FBE638B3D8E50C9E
uid           [ultimate] Brian Exelbierd
ssb   rsa2048 2019-03-21 [E] [expires: 2021-03-20]
      Keygrip = 5FA04ABEBFBC5089E50EDEB43198B4895BCA2136
ssb   rsa2048 2019-03-21 [A]
      Keygrip = 7710BA0643CC022B92544181FF2EAC2A290CDC0E
```
然后将keygrip加入sshcontrol文件 This file is used when support for the secure shell agent protocol has been enabled (see: [option --enable-ssh-support]). Only keys present in this file are used in the SSH protocol.
`echo 7710BA0643CC022B92544181FF2EAC2A290CDC0E >> ~/.gnupg/sshcontrol`
###告诉ssh如何访问gpg-agent
最后，通过修改环境变量SSH_AUTH_SOCK, ***让SSH知道如何访问gpg-agent***，我们可以将下面的代码加入到~/.bashrc或者~/.zshrc中
```
export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)
gpgconf --launch gpg-agent
```
###分享ssh公钥
最后，用ssh-add -L找到对应的公钥
`ssh-add -L`
添加到github

