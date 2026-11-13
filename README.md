# Shadowrocket 分流：懒人规则 + 个人覆盖

本项目通过 Shadowrocket 原生 `[General] include` 继承 [Johnshall 懒人规则](https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever)，并将少量个人规则置于上游配置之前。**不是复制上游文件，也不提供节点。**

## 主配置

```text
https://raw.githubusercontent.com/acshiryu/shadowrocket-rules/master/shadowrocket.conf
```

Shadowrocket：配置 → 右上角 + → 粘贴 URL → 下载 → 选中配置 → **使用配置/编译配置**；首页全局路由设为 **配置**。请预先添加可用代理节点。

## 规则顺序

1. 本地及私有网段直连
2. `rules/direct.list`：指定国内银行、政务直连
3. `rules/ai.list`：常用 AI 服务代理
4. `rules/proxy.list`：指定海外服务代理
5. 继承的 Johnshall `lazy.conf`：更多国内外、Apple、流媒体等分流及最终兜底

Shadowrocket 官方说明：当前配置优先级高于 `include` 的配置。个人规则中**没有额外 FINAL**，以便继续使用上游兜底。

## 文件

- `shadowrocket.conf`：主入口，继承上游并添加覆盖规则
- `rules/direct.list`：国内金融/政务直连例外
- `rules/ai.list`：AI 代理例外
- `rules/proxy.list`：部分海外站点代理例外
- `rules/reject.list`：可选个人广告拦截，**默认不启用**

## 重要提醒

- `include` 与远程 `RULE-SET` 的实际加载、缓存、优先级，请在 Shadowrocket 设备上通过「编译配置」「测试规则」验证；仓库端检查不等于设备端测试。
- 上游会变化；本仓库引用其在线配置，更新时须在客户端更新/重新编译配置。GitHub 更改不会自动即时生效。
- 不要将节点密钥、订阅 Token、真实姓名、账户信息提交到公开仓库。
- **金融类服务没有默认强制代理**。海外银行/券商/交易所可能要求特定网络位置；按其规则使用稳定、合规的连接，不建议频繁切换出口 IP。
- DNS、MITM、重写等原则上交由上游处理，避免两套配置冲突；广告过滤实际效果取决于当前上游配置，不保证已经开启。
- 原先的 `direct.list` / `proxy.list` 已缩小范围，避免过度强制匹配造成冲突。

## 上游

- https://johnshall.github.io/Shadowrocket-ADBlock-Rules-Forever/lazy.conf
- https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever

本项目与上游无隶属关系。
