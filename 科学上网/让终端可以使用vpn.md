# 让终端可以使用vpn

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
