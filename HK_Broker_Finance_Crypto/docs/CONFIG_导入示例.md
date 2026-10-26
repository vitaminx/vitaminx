# 八种客户端导入示例

以下所有 `YOUR-HOST`、`Crypto`、`HK-Broker`、`Crypto-Proxy` 都是**占位符**；
应用之前必须替换为你的真实地址/现有策略组或出站标签。
**本 ZIP 没有提供已经上传到公网上的订阅地址。**

## 1. Surge / 2. Loon / 3. Shadowrocket

三者共同使用 `surge-loon-shadowrocket/*.list`（域名/关键词语法一致）。
把文件上传到自己的 HTTPS 地址后，在主规则区使用如下示例：

```ini
[Rule]
RULE-SET,https://YOUR-HOST/rules/hk-banks/surge-loon-shadowrocket/hk-banks-default.list,DIRECT
RULE-SET,https://YOUR-HOST/rules/hk-brokers/surge-loon-shadowrocket/hk-brokers-default.list,HK-Broker
RULE-SET,https://YOUR-HOST/rules/crypto/surge-loon-shadowrocket/crypto-default.list,Crypto
```

Surge / Loon / Shadowrocket 的实际导入入口和策略组定义方式不完全相同；
可以在各自客户端中通过 UI 导入本地或远程规则集。策略组必须已存在。

## 4. Clash / Mihomo / 7. Stash

将压缩包的 `rules/` 目录放在客户端配置文件的同级位置，示例：

```yaml
rule-providers:
  hk-banks:
    type: file
    behavior: classical
    format: yaml
    path: ./rules/hk-banks/clash-stash/hk-banks-default-classical.yaml
  hk-brokers:
    type: file
    behavior: classical
    format: yaml
    path: ./rules/hk-brokers/clash-stash/hk-brokers-default-classical.yaml
  crypto:
    type: file
    behavior: classical
    format: yaml
    path: ./rules/crypto/clash-stash/crypto-default-classical.yaml

rules:
  - RULE-SET,hk-banks,DIRECT
  - RULE-SET,hk-brokers,HK-Broker
  - RULE-SET,crypto,Crypto
  # ...your existing general rules and MATCH...
```

若使用 `*-domain.yaml`，务必将 `behavior: classical` 改为 `behavior: domain`。
这种文件不含 `DOMAIN-KEYWORD`，和 classical 版本**不完全等价**。

如部署远程 Provider，可将 `type: file` 换成 `type: http`，添加对应 `url`，
并设置 `interval`、`path`；路径不可依赖 ZIP 文件直接从互联网访问。

## 5. Quantumult X

采用 `quantumult-x/*.list`：

```ini
[filter_remote]
https://YOUR-HOST/rules/hk-banks/quantumult-x/hk-banks-default.list, tag=HK-Banks, force-policy=direct, update-interval=86400, enabled=true
https://YOUR-HOST/rules/hk-brokers/quantumult-x/hk-brokers-default.list, tag=HK-Brokers, force-policy=HK-Broker, update-interval=86400, enabled=true
https://YOUR-HOST/rules/crypto/quantumult-x/crypto-default.list, tag=Crypto, force-policy=Crypto, update-interval=86400, enabled=true
```

这里的行语法为 `host`、`host-suffix`、`host-keyword`，不是 Surge 的 `DOMAIN-SUFFIX`。

## 6. Egern

将 `egern/*.yaml` 通过本地路径或远程 URL 导入；示例（主配置中）：

```yaml
rules:
  - rule_set:
      match: "https://YOUR-HOST/rules/hk-banks/egern/hk-banks-default.yaml"
      policy: DIRECT
      update_interval: 86400
  - rule_set:
      match: "https://YOUR-HOST/rules/hk-brokers/egern/hk-brokers-default.yaml"
      policy: HK-Broker
      update_interval: 86400
  - rule_set:
      match: "https://YOUR-HOST/rules/crypto/egern/crypto-default.yaml"
      policy: Crypto
      update_interval: 86400
```

文件内使用 `domain_set`、`domain_suffix_set`、`domain_keyword_set` 的规则集结构。

## 8. sing-box

`sing-box/*-v2.json` 为 1.10+ source 规则集，
`sing-box/*-v4.json` 为 1.13+ source 规则集。
文件是 **JSON 源规则**，不是二进制 `.srs` 文件。
以下是示意的 **部分 JSON**，不是完整可运行的 sing-box 配置：

```json
{
  "route": {
    "rule_set": [
      {"tag": "hk-banks", "type": "local", "format": "source",
       "path": "rules/hk-banks/sing-box/hk-banks-default-v4.json"},
      {"tag": "hk-brokers", "type": "local", "format": "source",
       "path": "rules/hk-brokers/sing-box/hk-brokers-default-v4.json"},
      {"tag": "crypto", "type": "local", "format": "source",
       "path": "rules/crypto/sing-box/crypto-default-v4.json"}
    ],
    "rules": [
      {"rule_set": ["hk-banks"], "action": "route", "outbound": "direct"},
      {"rule_set": ["hk-brokers"], "action": "route", "outbound": "broker-proxy"},
      {"rule_set": ["crypto"], "action": "route", "outbound": "crypto-proxy"}
    ]
  }
}
```

这里的 `broker-proxy`、`crypto-proxy` 必须是你已经配置的出站标签。
不同 sing-box 主配置版本的 Route Rule 字段会变化，
在旧版（1.10~1.12）请按对应官方文档调整主配置中的出站规则语法。

已有 sing-box CLI 的情况下，可按版本编译为 `.srs`：

```bash
sing-box rule-set compile --output crypto.srs rules/crypto/sing-box/crypto-default-v4.json
```

将生成的 `.srs` 用 `format: binary` 引用；不能把原始 `.json` 改名为 `.srs`。

## 常见问题

**只有部分银行域名走了指定策略：**
检查 `sc.com`、`hsbc.com` 等全球共享主机是否只出现在扩展版中。
通过网络日志判断应用访问的是哪个 Host，不建议直接将所有 `*.amazonaws.com`、
`*.cloudfront.net`、`*.qcloud.com` 等整体加入银行规则。

**交易所 App 的 API/行情未匹配：**
观察连接日志，确认是真实官方 Host，先加 `DOMAIN` 精确匹配；
不能因为部分连接使用同一 IP 就复制云服务 IP 段。

**能否保证香港 IP 就能登录交易：**
不能。交易商可能依据设备、账号、身份、App 区域、合规限制及风控进行判断。
