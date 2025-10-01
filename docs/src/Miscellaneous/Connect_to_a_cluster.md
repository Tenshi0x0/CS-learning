# 连接集群

当面临的任务需要较强 GPU （当然也有其他情景使用集群）时，一般需要连接集群。

这里提供的方法都是基于使用 vscode 连接的（感谢 GPT 的辅助）。

## Tunnel 连接步骤

我的学校集群用 **vscode SSH** 连接很不稳定，故使用并在此记录这个替代 SSH 的方案。

首先开个终端，并 SSH 连接上集群（显然，你要保证终端连接 SSH 是稳定的）。

下（1），（2），（3）步均在该终端操作。

1) 下载并验证 VS Code CLI

```sh
# x86_64
curl -fL --retry 3 --retry-delay 2 -o vscode_cli.tar.gz \
  'https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-x64'

# aarch64 (ARM64)
# curl -fL --retry 3 --retry-delay 2 -o vscode_cli.tar.gz \
#   'https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-arm64'

file vscode_cli.tar.gz      # 需包含 "gzip compressed data"
tar -xzf vscode_cli.tar.gz
./code --version            # 应打印出版本号
```

2) 收纳 CLI 并加入 PATH

```sh
mkdir -p ~/.local/vscode-cli
cp ./code ~/.local/vscode-cli/

# GPT 建议加入 .bashrc，但是我发现不加也不影响使用
# echo 'export PATH=$HOME/.local/vscode-cli:$PATH' >> ~/.bashrc
# source ~/.bashrc
```

3) 登录

```sh
# 先给 .bashrc 加行 alias 方便以后连接。
alias vsc='tmux new -As vsc "~/.local/vscode-cli/code tunnel --accept-server-license-terms --cli-data-dir ~/.vscode-cli --name [your_name]"'
```

之后终端 SSH 连上集群后直接 `vsc` 登录就行了（它会询问你使用 microsoft 还是 github 登录）。

登录成功后打开本地 vscode 的 tunnel 用同样的账号连接上即可。