# 让终端可以使用vpn

## mac zsh
参考 -> <https://www.hangge.com/blog/cache/detail_3138.html#>

这是我自己`.zshrc`中的配置

```zsh
# 终端可以使用vpn
# 这个是针对 clash客户端的
alias clash_proxy='export all_proxy=socks5://127.0.0.1:7890'
# 这个是针对samsock的
alias proxy='export all_proxy=socks5://127.0.0.1:10808'
alias unproxy='unset all_proxy'
```

使用方式

1. 启动: `proxy` 或者 `clash_proxy`
2. 关闭: `unproxy`


## windows powershell
参考 -> https://weilining.github.io/294.html

```powershell
$env:http_proxy="http://127.0.0.1:1080"
$env:https_proxy="http://127.0.0.1:1080"
```

还原

```powershell
$env:http_proxy=""
$env:https_proxy=""
```

### git proxy

```git
# 设置
git config --global http.proxy 'socks5://127.0.0.1:1080' 
git config --global https.proxy 'socks5://127.0.0.1:1080'

# 恢复
git config --global --unset http.proxy
git config --global --unset https.proxy
```


## git clone ssh 如何走代理

### macOS

打开 ~/.ssh/config，如果没有这个文件，自己手动创建
```
# 全局
ProxyCommand nc -X 5 -x 127.0.0.1:1080 %h %p
# 只为特定域名设定
Host github.com
    ProxyCommand nc -X 5 -x 127.0.0.1:1080 %h %p
```

### windows

打开 C:\Users\UserName\.ssh\config 文件，没有看到的话，同样手动创建。

```
# 全局
ProxyCommand connect -S 127.0.0.1:1080 %h %p
# 只为特定域名设定
Host github.com
    ProxyCommand connect -S 127.0.0.1:6600 %h %p

```

tips:

> 终端用了代理，还要给 git 单独设置代理吗？
> git 有两种协议，一种书 https，还有一种是 ssh 。
> 如果是用 https，设置终端代理即可。如果是 ssh，需要单独配置代理

