# Shadowrocket 分流：懒人规则 + 个人覆盖

本项目通过 Shadowrocket 原生 `[General] include` 继承 [Johnshall 懒人规则](https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever)，并将少量个人规则置于上游配置之前。**不是复制上游文件，也不提供节点。**

## 主配置

```text
https://raw.githubusercontent.com/acshiryu/shadowrocket-rules/master/shadowrocket.conf
```

Shadowrocket：配置 → 右上角 + → 粘贴 URL → 下载 → 选中配置 → **使用配置/编译配置**；首页全局路由设为 **配置**。请预先添加可用代理节点。

## 规则顺序

1. 本地及私有网段直连
2. `rules/direct.list`：国内银行和政务直连
3. `rules/finance.list`：支付、证券及国内直连金融服务保护
4. `rules/finance-overseas.list`：Ethena、Plasma、ether.fi 等指定海外金融服务代理
5. [Johnshall 去广告](https://johnshall.github.io/Shadowrocket-ADBlock-Rules-Forever/sr_ad_only.conf)：广告域名拒绝访问（REJECT）
6. `rules/ai.list`：AI 服务代理
7. `rules/apple.list`：Apple 核心服务直连
8. `rules/microsoft.list`：Microsoft 日常服务直连
9. `rules/proxy.list`：其他指定代理服务
10. 继承的 Johnshall `lazy.conf`：更多国内外、Apple、流媒体等分流及最终兜底

Shadowrocket 官方说明：当前配置优先级高于 `include` 的配置。个人规则中**没有额外 FINAL**，以便继续使用上游兜底。

## 文件

- `shadowrocket.conf`：主入口，继承上游并添加覆盖规则
- `rules/direct.list`：银行/政务直连例外
- `rules/finance.list`：国内银行、支付、证券及嘉信直连白名单
- `rules/finance-overseas.list`：指定海外金融 App 代理规则（Ethena、Plasma、ether.fi）
- `rules/ai.list`：AI 代理例外（包括 Microsoft Copilot）
- `rules/apple.list`：Apple 服务独立直连规则
- `rules/microsoft.list`：Microsoft 服务独立直连规则
- `rules/proxy.list`：部分海外站点代理例外
- `rules/reject.list`：额外个人广告拦截，**默认不启用**
- 上游广告源：`sr_ad_only.conf`，主配置已启用并放在金融直连之后

## Apple / Microsoft 策略

Apple 和 Microsoft 均使用独立域名规则集，默认 DIRECT；Microsoft Copilot 等明确列出的 AI 域名先由 `rules/ai.list` 代理。没有强制把所有 Microsoft Azure、GitHub 或第三方 CDN 纳入直连，未命中部分由上游分流。此处是域名级规则，不保证覆盖每个 App 的全部请求；请在客户端按实际请求测试。

## 去广告与金融兼容

采用 Johnshall 的 `sr_ad_only.conf`，放在金融白名单之后。其上游文件包含完整的 `[Rule]` 段及已附带 REJECT 策略，不是标准的纯 `.list` 规则集；虽然存在将这类文件直接用于 `RULE-SET` 的使用案例，但客户端实际兼容性**尚未验证**。如果 Shadowrocket 提示下载、解析失败，请先将这条 `RULE-SET` 注释，并改用标准格式的替代源：
```text
RULE-SET,https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Shadowrocket/Advertising/Advertising.list,REJECT
```

金融白名单覆盖的是部分官方域名，并非 App 全部网络请求。出现支付或登录失败时，可通过 Shadowrocket 日志确认具体被拒绝域名，评估后增加**精确域名**放行，避免整个广告统计服务全局放行。白名单遵循优先匹配，因此其中的某些广告/跟踪请求也可能被允许；这是兼容性的取舍。

## 重要提醒

- `include` 与远程 `RULE-SET` 的实际加载、缓存、优先级，请在 Shadowrocket 设备上通过「编译配置」「测试规则」验证；仓库端检查不等于设备端测试。
- 上游会变化；本仓库引用其在线配置，更新时须在客户端更新/重新编译配置。GitHub 更改不会自动即时生效。
- 不要将节点密钥、订阅 Token、真实姓名、账户信息提交到公开仓库。
- **金融类服务没有默认强制代理**。海外银行/券商/交易所可能要求特定网络位置；按其规则使用稳定、合规的连接，不建议频繁切换出口 IP。
- DNS、MITM、重写等原则上交由上游处理。广告规则已在本仓库启用，但生效需要通过 Shadowrocket 客户端完成加载和规则测试。
- 原先的 `direct.list` / `proxy.list` 已缩小范围，避免过度强制匹配造成冲突。

## 上游

- https://johnshall.github.io/Shadowrocket-ADBlock-Rules-Forever/lazy.conf
- https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever

本项目与上游无隶属关系。
