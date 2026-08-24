# PlayfulSR

个人使用的 Shadowrocket 分流配置。仓库只保存脱敏后的配置、规则说明和节点命名规范，不包含代理节点、订阅链接、密码或 MITM 证书。

## 文件

- `PlayfulSR.conf`：可直接通过 URL 导入 Shadowrocket 的稳定配置。
- `NODE_NAMING.md`：国家、IP 属性和流量属性的节点命名规范。

## 导入地址

```text
https://raw.githubusercontent.com/nomad70/shadowrocket-rules/main/PlayfulSR.conf
```

## 当前分流

- 国内域名和中国 IP 默认直连。
- 未匹配流量默认代理。
- ChatGPT、Gemini、Claude 使用独立的美国住宅 IP 策略组。
- Ollama、LM Studio、Hugging Face 等模型下载使用无限/大流量节点组。
- YouTube、Spotify、Disney+、HBO Max、Netflix 使用独立策略组。
- Bybit 可在德国、日本和荷兰节点间单独选择。

## 使用前检查

1. 按 `NODE_NAMING.md` 为住宅 IP、无限流量和大流量节点添加规范标签。
2. 导入配置后检查各国家组和属性组是否包含预期节点。
3. 为 AI 服务手动固定一个已确认的美国住宅 IP。
4. 不要向本仓库提交订阅 URL、代理凭据、私钥、证书或未脱敏日志。

## 外部规则

配置只引用少量专项在线规则，来源为 [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)。外部规则的版权和许可证归其原项目所有。
