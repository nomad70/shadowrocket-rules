# PlayfulSR

个人使用的 Shadowrocket 分流配置。仓库只保存脱敏后的配置、规则说明和节点命名规范，不包含代理节点、订阅链接、密码或 MITM 证书。

## 文件

- `PlayfulSR.conf`：可直接通过 URL 导入 Shadowrocket 的稳定配置。
- `NODE_NAMING.md`：国家、IP 属性和流量属性的节点命名规范。
- `rules/`：按服务拆分、由本仓库维护的在线规则文件。
- `AI_PRIVACY.md`：AI 服务防本地 IP 泄漏边界和 Shadowrocket 必要设置。
- `LICENSE`：PlayfulSR 自身内容采用的 MIT 许可证。
- `THIRD_PARTY_NOTICES.md`：规则参考来源与第三方许可证说明。

## 导入地址

```text
https://raw.githubusercontent.com/nomad70/shadowrocket-rules/main/PlayfulSR.conf
```

## 当前分流

- 国内域名和中国 IP 默认直连。
- 未匹配流量默认代理。
- ChatGPT、Gemini、Claude 使用独立的美国可信 IP 策略组。
- Ollama、LM Studio、Hugging Face 等模型下载使用无限/大流量节点组。
- YouTube、Spotify、Disney+、HBO Max、Netflix 使用独立策略组。
- Bybit 优先使用德国 Rabisu 可信出口，也可在日本、德国通用组和荷兰之间切换。
- `RELAY` 中转节点自动排除出所有最终出口组。
- 服务域名规则由本仓库独立维护，不再直接依赖第三方规则仓库。
- V2.2 扩充了国内常用服务，以及 AI、Netflix、YouTube、Spotify、Disney+ 和 Max 的登录、API、播放与必要 CDN 覆盖。
- V2.3 为 AI 和未匹配境外流量移除 `DIRECT` 路径，并对不支持的 UDP、缺失代理链和代理 QUIC 采用失败关闭策略。
- V2.4 将 Claude 拆分为严格可信出口，ChatGPT/Gemini 改为可手动固定美国可信或常规 ISP/DC；同时恢复其他代理服务的 QUIC。
- V2.4.1 修复本地节点无法进入动态策略组的问题：名称正则直接筛选本地节点，不再误用仅适用于指定订阅的 `use=true`。

## 使用前检查

1. 按 `NODE_NAMING.md` 为可信出口、无限流量和大流量节点添加规范标签。
2. 导入配置后检查各国家组和属性组是否包含预期节点。
3. 为 AI 服务手动固定一个已验证的美国可信 IP。
4. 不要向本仓库提交订阅 URL、代理凭据、私钥、证书或未脱敏日志。
5. 按 `AI_PRIVACY.md` 开启 Shadowrocket“始终开启”，并保持全局路由为“配置”。

## 规则维护

规则拆分在 `rules/` 目录中。新增域名时只修改对应服务文件，并在发布前做语法、重复项和敏感信息检查。规则文件不包含节点、订阅地址、凭据或连接日志。

部分服务规则参考 V2Fly `domain-list-community` 的 MIT 许可数据，并由本仓库转换、筛选和维护；运行时不会直接下载该第三方仓库。详情见 `THIRD_PARTY_NOTICES.md`。

## 许可证

PlayfulSR 自身的配置和文档使用 MIT License；第三方规则来源继续遵循各自许可证，详见 `THIRD_PARTY_NOTICES.md`。
