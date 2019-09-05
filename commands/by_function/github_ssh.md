# 通过ssh连接github

一般地，本地生成的SSH密钥对存放在**~/.ssh**目录下。
可以通过查看该目录下是否有密钥对文件。

```
ls -al ~/.ssh
```

通过查看该目录下是否有公钥，来判断是否已经生成了SSH密钥对。
一般地。公钥文件命名为

- id_dsa.pub
- id_ecdsa.pub
- id_ed25519.pub
- id_rsa.pub

如果没有公钥文件，可以通过如下命令生成SSH密钥对。

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# 其中
#   -t  指定密钥加密解密的算法，可以为[dsa,ecdsa,ed25519]。
#   -b  指定生成密钥对的位数。
#   -C  指定comment，只是一个label。
```

启动ssh-agent

```
eval $(ssh-agent -s)
# 其中
#   eval 命令执行后面的cmd命令两次，可用于生成cmd命令的执行。
#   ssh-agent -s 用于生成Bourne shell的命令，$()执行括号中的命令，输出字符作为替代。
```

将私钥添加在ssh-agent进行管理。

```
ssh-add ~/.ssh/id_rsa
```

在github创库的设定中新建SSH，将公钥复制到github上。

```
clip < ~/.ssh/id_rsa.pub
# Windows下的clip命令可以将输入保存到粘贴板。
```