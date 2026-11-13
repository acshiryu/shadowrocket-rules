# Shadowrocket 分流规则

个人维护的 Shadowrocket 分流规则仓库。

## 分流策略

- 国内常用网站、银行服务、局域网：`DIRECT`
- AI 服务及指定海外网站：`PROXY`
- 其他中国大陆 IP：`DIRECT`
- 其余流量：`PROXY`
- 广告拦截单独维护，默认不启用，以免误拦截

## 订阅配置

在 Shadowrocket 的「配置」中通过 URL 导入：

```text
https://raw.githubusercontent.com/acshiryu/shadowrocket-rules/master/shadowrocket.conf
```

导入后请选中该配置，并使用「配置」分流模式。此配置**不提供代理节点**；请先在 Shadowrocket 中添加可用的代理服务器，并确认 `PROXY` 策略使用可用节点。

## 文件结构

| 文件 | 作用 |
| --- | --- |
| `shadowrocket.conf` | 主配置入口，按顺序引用远程规则 |
| `rules/direct.list` | 指定直连域名 |
| `rules/proxy.list` | 指定代理域名 |
| `rules/ai.list` | 常用 AI 服务代理域名 |
| `rules/reject.list` | 预留广告拦截列表（默认未启用） |

## 注意事项

1. `RULE-SET` 引用的规则会按主配置里的顺序匹配，命中后不再继续检查。
2. 远程规则更新依赖 Shadowrocket 对配置及规则集的刷新/缓存机制。
3. 域名列表只是起始版本，不保证覆盖所有子域名或银行 App 的第三方依赖。
4. 如需要启用广告拦截，可在主配置的 `[Rule]` 段加入 `RULE-SET,https://raw.githubusercontent.com/acshiryu/shadowrocket-rules/master/rules/reject.list,REJECT`，并放在直连和代理规则之前；请先检查拦截列表。
5. 不要将节点凭据、订阅 Token 或个人信息提交到公开仓库。

## 维护

规则采用 Shadowrocket 常用的 `DOMAIN-SUFFIX`、`DOMAIN`、`IP-CIDR` 等匹配类型。欢迎按需修改并测试。
