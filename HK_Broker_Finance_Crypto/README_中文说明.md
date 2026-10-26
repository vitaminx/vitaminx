# 加密货币 + 香港券商 + 香港银行 分流规则全集

**整理日期：2026-10-26** · 客户端：Clash / Mihomo、Egern、Loon、Quantumult X、Shadowrocket、sing-box、Stash、Surge。

这份规则合集是**分流规则**，不是对任何金融平台、交易网站的安全认证。
所有客户端配置都需由你本人选择 `DIRECT`、代理或策略组，本包不强行指定节点。

## 1. 核心内容与规则数

| 规则主题 | 版本 | DOMAIN | DOMAIN-SUFFIX | DOMAIN-KEYWORD | 合计 |
|---|---|---:|---:|---:|---:|
| 加密货币 / 区块链 (`crypto`) | `default` | 6 | 396 | 15 | **417** |
| 加密货币 / 区块链 (`crypto`) | `expanded` | 6 | 413 | 64 | **483** |
| 香港及常用境外券商 (`hk-brokers`) | `default` | 3 | 141 | 0 | **144** |
| 香港及常用境外券商 (`hk-brokers`) | `expanded` | 3 | 145 | 0 | **148** |
| 香港银行 (`hk-banks`) | `default` | 1 | 36 | 0 | **37** |
| 香港银行 (`hk-banks`) | `expanded` | 0 | 44 | 0 | **44** |
| 加密货币资料站 / 浏览器 (`crypto-directory`) | `default` | 0 | 51 | 0 | **51** |

**推荐**：优先使用 `default`。遇到香港渣打全球 `sc.com`、汇丰全球站点、其它集团入口等未匹配时，
再按需切换到 `hk-banks-expanded`（注意扩展版会匹配该银行的海外站点）。
券商扩展版包含少量上游额外域名。加密货币扩展版覆盖更多关键词、历史服务及可能更宽泛的站点。

`crypto-directory-default` 是**加密货币资料与目录站点**的独立子规则
（行情、研究、链浏览器和目录），已经完全包含在 `crypto-default` 中；
可以独立使用，也可放在更宽泛的 `crypto` 之前单独分流，不必重复加载两个相同策略。

## 2. 文件对应关系

每个主题位于 `rules/<主题>/`，标准命名为：

| 客户端 | 子文件路径（相对 `rules/<主题>/`） | 备注 |
|---|---|---|
| Surge / Loon / Shadowrocket | `surge-loon-shadowrocket/<主题>-default.list` | 三端共用 |
| Clash / Mihomo / Stash | `clash-stash/<主题>-default-classical.yaml` | 保留关键词，推荐 |
| Clash / Mihomo / Stash（仅域名） | `clash-stash/<主题>-default-domain.yaml` | 不含关键词 |
| Quantumult X | `quantumult-x/<主题>-default.list` | 不能直接用 Surge 文件替代 |
| Egern | `egern/<主题>-default.yaml` | 原生 YAML 集合 |
| sing-box 1.10+ | `sing-box/<主题>-default-v2.json` | source JSON v2 |
| sing-box 1.13+ | `sing-box/<主题>-default-v4.json` | source JSON v4 |

主题目录名称：`crypto`、`hk-brokers`、`hk-banks`、`crypto-directory`。
`crypto-directory` 仅有 `default`；其余三个主题把文件名中的 `default` 改为 `expanded`
即可切换到扩展版。

## 3. 如何导入

具体配置分别见 `docs/CONFIG_导入示例.md`。请不要直接复制 `https://YOUR-HOST/...`
这样的示例地址，它们**不是可访问的订阅链接**。本包可以解压后做本地规则源；
如需自动更新，要先自行托管到 HTTPS 并设置上游更新频率。

**默认排序建议：**
1. 如需独立策略，先放 `crypto-directory`；
2. `hk-banks`、`hk-brokers`、`crypto` 等分类规则；
3. 普通的 GEOIP/GEOSITE/DOMAIN-SUFFIX 总类规则；
4. `MATCH/FINAL` 兜底规则。

注意：同一站点可能同时提供银行和券商服务，规则根据**请求域名**
而不是 App 名称分流。若同时启用多套规则，优先级由你的主配置决定。

## 4. 重要取舍

- 未将来源中券商使用的腾讯云 / AWS 等共享 IP-CIDR 作为活跃规则；
  仅在 `review/DO_NOT_IMPORT_...txt` 里保留参考，防误伤。
- 未直接纳入 AppsFlyer、Firebase/Crashlytics、Mixpanel、Tealium、通用 CDN 等共享域名；
  某些登录/风控功能如有问题，请按 App 日志精确补充域名。
- `hk-banks-default` **故意不匹配所有 `sc.com` 页面**，因为渣打香港官网与全球域名共用主机；
  `hk-banks-expanded` 包含 `sc.com`，会一并匹配渣打其他地区站点。
- 2026 年 ELE Bank 使用 `elebank.com`（原 Airstar Bank），不保证未知历史镜像仍归其拥有。
- 跨客户端的数据内容归一化，但特定客户端不支持的关键词行为不会强行转换成全域名匹配；
  Clash/Stash `domain` 行为是最典型的功能缩水版。
- 中国大陆、香港或其它地区的交易及应用访问政策可能不同；分流规则不等同于绕过服务的地域/身份限制。

## 5. 数据和可审计性

- `data/*.json`：各主题经过归一化的精确域名、域名后缀、关键词；
- `data/catalog_*.tsv`：扩充及分类资料，便于人工维护；
- `docs/SOURCES_来源.md`：所有上游及相关资料目录来源链接；
- `docs/REVIEW_审计说明.md`：明确未合并的宽泛规则、排除理由与冲突记录；
- `docs/CONFIG_导入示例.md`：按客户端加载的模板；
- `manifest.json`：规则量与 SHA256 校验值。

**验证范围**：ZIP 完整性、文件编码、YAML/JSON 语法、跨格式规则一致性、去重、
域名格式和关键命中测试。未在真实登录账户或全部八种客户端逐一实机验证。
