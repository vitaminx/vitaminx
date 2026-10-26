# 清单审计 / REVIEW

此清单解释为什么不自动合并某些上游项目。域名是否属于金融机构，不能等同于域名是否安全。

## 券商：排除的共享服务

- `9oju31.launches.appsflyersdk.com`：应用归因第三方服务；不适合通配整棵子域
- `cg.play-analytics.com`：跨产品分析域名
- `loginv6.im.qcloud.com`：腾讯云共享即时通信后端
- `play-analytics.com`：跨产品分析域名
- `sgqt0j.launches.appsflyersdk.com`：应用归因第三方服务
- `shortconn.im.qcloud.com`：腾讯云共享即时通信后端
- `shortconnv6.im.qcloud.com`：腾讯云共享即时通信后端
- `tradingview-user-uploads.b-cdn.net`：共享 CDN 上传服务
- `tradingview-widget.com`：独立图表组件，非券商独占
- `tradingview.com`：独立的多市场行情服务；与加密货币规则冲突
- `tradingviewstore.com`：通用 TradingView 服务

## 银行：排除的共享服务

- `api.mixpanel.com`：通用产品分析
- `app-measurement.com`：通用移动统计
- `appsflyersdk.com`：通用 SDK，属于很多应用
- `cdn.optimizely.com`：通用 A/B 服务
- `crashlytics.com`：跨 App 崩溃上报
- `hsbc.edge.sdk.awswaf.com`：AWS WAF 托管端点，非专属域名
- `liveperson.net`：通用在线客服
- `log-58144bf0.we-stats.com`：跨业务统计域名
- `lpsnmedia.net`：通用客服媒体 CDN
- `mobile.eum-appdynamics.com`：跨平台监控 SDK
- `online-metrix.net`：跨银行反欺诈服务
- `tags.tiqcdn.com`：Tealium 共享基础设施
- `tealiumiq.com`：通用标签/分析平台
- `wise.com`：跨境支付，不属于 HK 银行的专属域名

## 加密货币：值得单独评估的规则

- `appsflyersdk.com`：通用应用归因追踪
- `arweave.net`：公共网关，可能承载任意第三方网页
- `circle.com`：集团层面可能涵盖非加密货币业务
- `ipfs.io`：公共内容网关，原包虽含，但不宜因其包含所有内容就持续扩展到其他共享网关
- `sc.com`：渣打全球域名，香港银行建议扩展版才使用
- `tradingview.com`：通用金融图表，扩展版保留，默认不加
- `wise.com`：跨境支付服务，不等同香港持牌银行

## 旧加密货币包原本排除的域名

- `365huo.xyz`：affiliation not independently verified
- `aad90d5c0e88c4b4285bd912f64d970d-1403944741.ap-northeast-1.elb.amazonaws.com`：ephemeral AWS ELB
- `ably.io`：generic real-time cloud platform
- `addfd325e6e3.cdn4.forter.com`：shared anti-fraud/CDN
- `appsflayer.com`：third-party attribution domain; unverified typo, broad scope
- `b2d8207c-e08a-45cd-bc97-268da85016d2-addfd325e6e3.cdn.forter.com`：shared anti-fraud/CDN
- `bb-os.com`：affiliation not independently verified
- `bin-prod-new-websocket-alb-1463526464.ap-northeast-1.elb.amazonaws.com`：ephemeral AWS ELB
- `braze.eu`：generic customer-engagement provider
- `crt.r2m01.amazontrust.com`：certificate infrastructure
- `crt.rootca1.amazontrust.com`：certificate infrastructure
- `crt.rootg2.amazontrust.com`：certificate infrastructure
- `crunchbase.com`：general business database
- `d3sknckuh1o9tg.cloudfront.net`：shared CloudFront CDN
- `domain-config-1306379396.file.myqcloud.com`：cloud object store
- `gotoda.co`：affiliation not independently verified
- `greentreeone.com`：affiliation not independently verified
- `m.api.forter.com`：shared fraud service
- `mocortech.com`：affiliation not independently verified
- `panpno.com`：affiliation not independently verified
- `public-1306379396.file.myqcloud.com`：cloud object store
- `siftscience.com`：multi-client anti-fraud service
- `static-file-1306379396.file.myqcloud.com`：cloud object store

## 银行 URL 级特例

原 `HK_Banks_Direct.list` 使用 `URL-REGEX,^https?://www\.sc\.com/hk/`，
但域名规则集无法只匹配 `www.sc.com` 下的 `/hk/` 路径。
因此默认版只保留渣打香港专属域名及 `api.sc.com`；扩展版包含 `sc.com`，也会匹配渣打其他国家页面。

## 关于来源中的 IP 规则

从券商来源整理出 63 个不同 CIDR，保存在 review 目录只作审查。
由于 IP 可能属于共享云，未生成默认生效的 IP-CIDR 规则。

## 规则复用与优先级

`crypto-directory` 是 `crypto` 的子集，**不要配置成两个不同策略**，除非特意把目录子集放在前面。
如同时启用默认版和扩展版，扩展版已经覆盖默认版，无须重复加载。
三大主题独立、策略自行指定；在兜底 `MATCH/FINAL` 规则之前加载。

## 虚拟银行/数字银行名称

Airstar 已更名为 ELE Bank，`elebank.com` 在默认版，旧域名 `airstarbank.com` 因未确认归属而未纳入任何版本。
PAO Bank 的公司名称和品牌曾发生变更，域名清单不代表银行法律名称。

## 未经完全验证的域名

部分历史镜像或第三方资料中的服务域名可能已经过期、转手或停止运营。
包内的网站域名并非官方认证名单，请以相应机构官网及香港监管机构登记册为准。
