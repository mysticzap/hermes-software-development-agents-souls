# Windows Or Unbuntu system common commands (common-command.md)
Windows或Unbuntu系统下工作时常用命令

## Windows下运行命令
**上传文件到服务器**
```shell
# 命令格式：scp 本地文件全地址 用户名@IP:文件存放地址
scp D:\work\projects\hermes\hermes-software-development-agents-souls\.hermes\profiles\coo\SOUL.md clawtest05@10.80.128.245:/home/clawtest05/.hermes/profiles/coo/
scp D:\work\projects\hermes\hermes-software-development-agents-souls\.hermes\profiles\alan\SOUL.md clawtest05@10.80.128.245:/home/clawtest05/.hermes/profiles/alan/
scp D:\work\projects\hermes\hermes-software-development-agents-souls\.hermes\profiles\turing\SOUL.md clawtest05@10.80.128.245:/home/clawtest05/.hermes/profiles/turing/
scp D:\work\projects\hermes\hermes-software-development-agents-souls\.hermes\profiles\qa\SOUL.md clawtest05@10.80.128.245:/home/clawtest05/.hermes/profiles/qa/
```

**下载文件夹到本地**
```shell
# 基本语法
scp -r [用户名]@[远程主机]:[远程文件夹路径] [本地目标路径]

scp -r clawtest05@10.80.128.245:/home/clawtest05/projects/crm-project D:\work\projects\hermes\

# 示例：从远程服务器下载文件夹
scp -r user@192.168.1.100:/home/user/projects /Users/localuser/Downloads/

# 指定端口（如果SSH端口不是22）
scp -r -P 2222 user@example.com:/var/www/html /local/path/

# 保持文件权限和时间戳
scp -rp user@server:/path/to/folder /local/path/
```

## Unbuntu下运行命令