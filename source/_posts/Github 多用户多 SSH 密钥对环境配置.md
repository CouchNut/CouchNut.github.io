---
title: Github 多用户多 SSH 密钥对环境配置
date: 2022-03-24 16:19:36
tags: Github
---

## 问题
有两个 Github 账号：*person*（个人使用）和 *work*(工作使用)

想要在同一个电脑上使用这两个账号，并且每次 push 或者 pull 的时候都不需要输入密码


## 解决
使用 ssh 密钥并且在 ssh config 文件中为每个账户设置一个 host 别名

## 步骤
1. [为每个账号生成一个 SSH 密钥](https://help.github.com/articles/generating-a-new-ssh-key/) 并 [添加到对应的账号中](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/).

2. 创建并且编辑 ssh config 文件 (`~/.ssh/config`):

   ```conf
   # Default github account: person
   Host person
      HostName github.com
      IdentityFile ~/.ssh/person_private_key
      IdentitiesOnly yes

   # Other github account: work
   Host work
      HostName github.com
      IdentityFile ~/.ssh/work_private_key
      IdentitiesOnly yes
   ```

3. [将 ssh 私钥添加到本机代理中](https://help.github.com/articles/adding-a-new-ssh-key-to-the-ssh-agent/):

   ```shell
   $ ssh-add ~/.ssh/person_private_key
   $ ssh-add ~/.ssh/work_private_key
   ```

4. 测试连接

   ```shell
   $ ssh -T git@person
   $ ssh -T git@work
   ```

	对于每个命令，您可能会看到这种警告，请输入“yes”：


   ```shell
   The authenticity of host 'github.com (192.30.252.1)' can't be established.
   RSA key fingerprint is xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:
   Are you sure you want to continue connecting (yes/no)?
   ```

	如果一切正常，您将看到以下消息：


   ```shell
   Hi person! You've successfully authenticated, but GitHub does not provide shell access.
   ```

   ```shell
   Hi work! You've successfully authenticated, but GitHub does not provide shell access.
   ```

5. 使用

	因为 Host 配置的 person，使用clone的时候，要将github.com替换为person, work 同理


   ```shell
   $ git clone git@person:org2/project2.git /path/to/project2
   $ cd /path/to/project2
   $ git config user.email "person@org2.com"
   $ git config user.name  "Person"
   ```

完成