# PlayfulSR 自有规则

本目录保存 PlayfulSR v2.3 使用的 Shadowrocket 规则集。配置文件运行时不直接依赖第三方规则仓库，规则由本仓库筛选、转换和维护。

每个 `.list` 文件只包含匹配条件，不包含策略名称；具体出口由 `PlayfulSR.conf` 中的 `RULE-SET` 决定。规则按配置文件中的引用顺序匹配，AI 模型下载必须先于普通 AI 和 Google。

部分服务规则参考 V2Fly `domain-list-community` 的公开 Geosite 数据，并按 Shadowrocket 语法和本配置的实际用途重新筛选。`domain:` 对应 `DOMAIN-SUFFIX`，`full:` 对应 `DOMAIN`；少数无法直接转换的正则表达式，只在范围足够明确时改写为 `DOMAIN-KEYWORD`。来源及许可证见仓库根目录的 `THIRD_PARTY_NOTICES.md`。

维护规则：

- 优先收录服务官方主域名、官方公布的网络范围和实际验证过的必要域名。
- 覆盖登录、API、播放、必要 CDN、地区检测和常用产品入口；不追求收录整个关联企业集团。
- 避免将整个 AWS、Cloudflare、Akamai 等共享域名划给单一服务。
- 新规则先做语法和重复检查，再通过 Shadowrocket 日志验证命中情况。
- 服务出现漏分流时，只修改对应文件；不把节点、订阅地址、凭据或连接日志写入规则集。
