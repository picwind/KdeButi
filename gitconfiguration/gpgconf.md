# 生成GPG密钥
i密钥必须使用RSA
1. `gpg --full-generate-key`生成密钥
2. 在提示时，指定要生成的密钥类型，或按 Enter 键接受默认值。
3. 在提示时，指定要生成的密钥大小，或按 Enter 键接受默认值。 密钥必须至少是 4096 位。
4. 输入密钥的有效时长。 按 Enter 键将指定默认选择，表示该密钥不会过期。 除非你需要过期日期，否则我们建议接受此默认值。
5. 验证您的选择是否正确。
6. 输入您的用户 ID 信息。（使用github提供的no-reply邮箱）
7. 输入安全密码。（可以为空）
8. 使用 `gpg --list-secret-keys --keyid-format=long` 命令列出你拥有其公钥和私钥的长形式 GPG 密钥。 签名提交或标记需要私钥。复制GPG密钥ID的长形式，在本例中为3AA5C34371567BD2
9. `gpg --armor --export 3AA5C34371567BD2` Prints the GPG key ID, in ASCII armor format
10. 复制以 -----BEGIN PGP PUBLIC KEY BLOCK----- 开头并以 -----END PGP PUBLIC KEY BLOCK----- 结尾的 GPG 密钥。将 GPG 密钥新增到 GitHub 帐户

# 告诉GIT自己的签名密钥
1. If you have previously configured Git to use a different key format when signing with --gpg-sign, unset this configuration so the default format of openpgp will be used.
`$ git config --global --unset gpg.format`
2. 使用`gpg --list-secret-keys --keyid-format=long`命令列出你拥有其公钥和私钥的长形式 GPG 密钥。 签名提交或标记需要私钥。
3. 从 GPG 密钥列表中复制您想要使用的GPG密钥ID的长形式。在本例中，GPG密钥ID为 3AA5C34371567BD2
4. 若要在 Git 中设置 GPG 签名主钥，请粘贴下面的文本，替换要使用的 GPG 主钥 ID。 在本例中，GPG 密钥 ID 为 3AA5C34371567BD2  `git config --global user.signingkey 3AA5C34371567BD2`，或者在设置子钥时包含 ! 后缀。 在本例中，GPG 子钥 ID 为 4BB6D45482678BE3：`git config --global user.signingkey 4BB6D45482678BE3`
5. To add your GPG key to your .zshrc/.bashrc startup file, run the following command:
`[ -f ~/.zshrc ] && echo 'export GPG_TTY=$(tty)' >> ~/.zshrc
export GPG_TTY=$(tty) tty命令输出当前tty,设置环境变量GPG_TTY为当前tty。
pinentry[是输入密码使用私钥的程序，有GUI和TLI两种形式，GUI需要设置DISPLAY变量，TLI需要GPG_TTY变量](https://qastack.cn/superuser/520980/how-to-force-gpg-to-use-console-mode-pinentry-to-prompt-for-passwords)

[tty/pts资料参考](http://news.558idc.com/282685.html)


