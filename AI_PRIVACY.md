# AI 服务防本地 IP 泄漏说明

PlayfulSR V2.4 对 AI 服务采用分级保护：Claude 使用严格失败关闭；ChatGPT/Gemini 默认使用美国可信出口，也允许手动固定美国 ISP/DC。三者都不能回退到本地网络。

## 配置已经提供的保护

- Claude 规则只指向“Claude严格”，该组只包含“美国可信IP”，没有普通节点或 `DIRECT`。
- ChatGPT/Gemini 指向“AI常规”，默认选择“美国可信IP”，也可手动选择名称规范的美国 `ISP/DC` 节点；该组没有自动测速或 `DIRECT`。
- “默认代理”不再提供 `DIRECT`，因此遗漏的境外辅助域名仍会走代理。
- `FINAL` 指向“默认代理”，未匹配流量不会自动直连。
- `ipv6 = false`，避免节点或规则只接管 IPv4 时发生 IPv6 旁路。
- `dns-direct-system = false`，代理域名由代理连接处理；`hijack-dns = :53` 接管普通 53 端口 DNS 请求。
- `udp-policy-not-supported-behaviour = REJECT`：节点不支持 UDP 时拒绝连接，不回退 `DIRECT`。
- `close-if-proxy-chain-missing = true`：链式代理缺少中转节点时拒绝该连接。
- Claude 规则覆盖 `claude.ai`、`claude.com`、`claudeusercontent.com` 和 `anthropic.com`，包括认证、Console、API、MCP、下载及 Artifact 使用的相关子域名。

## 必须配合的 Shadowrocket 设置

配置文件无法在 Shadowrocket 被关闭或全局路由被手动切换为“直连”时继续保护流量。身份敏感 AI 服务应同时满足：

1. 全局路由保持“配置”，不要切换为“直连”。
2. 在“设置 → 按需求连接”中开启“始终开启”，避免 VPN 意外断开或设备重启后直接联网。
3. 关闭“睡眠时断开”。
4. 为使用的节点开启 UDP 转发；若节点不支持，V2.4 会拒绝 UDP，而不是本地直连。支持 UDP 的普通代理流量仍可正常使用 QUIC。
5. 不要在 AI 使用过程中停用 Shadowrocket、切换配置或选择来源不明的临时节点。

## 验证方法

更新并重新编译配置后，在 Shadowrocket 代理日志中检查以下域名：

```text
claude.ai
platform.claude.com
api.anthropic.com
mcp-proxy.anthropic.com
bridge.claudeusercontent.com
```

它们应全部命中“Claude严格”，并显示预期的美国可信出口。测试代理断开时，Claude 应连接失败，不能成功回退到本地网络。`chatgpt.com` 与 `gemini.google.com` 应命中“AI常规”。

如果启用浏览器语音、视频或其他 WebRTC 功能，还可以在 Shadowrocket 设置中启用“禁用 STUN”进行更严格的隐私保护；这可能导致 ChatGPT 语音、视频通话或其他 WebRTC 应用不可用，因此 V2.4 不默认全局启用。

## 参考资料

- Anthropic 官方网络要求：https://code.claude.com/docs/en/corporate-proxy#network-access-requirements
- Anthropic 支持地区：https://www.anthropic.com/supported-countries
- Shadowrocket 社区手册：https://github.com/LOWERTOP/Shadowrocket/wiki
