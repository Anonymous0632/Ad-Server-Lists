# Ad Server Lists

这是基于 Limbopro 的规则构建的广告屏蔽规则仓库，包含适用于 sing-box 的 SRS 文件，以及可用于 Surge 和 Quantumult X 的 `.list` 规则文件。

本仓库保存了广告屏蔽规则的文本源文件、Surge / Quantumult X 可直接引用的 `.list` 文件，以及已经转换好的 sing-box binary rule-set 文件。`srs/` 目录里的 `.srs` 文件可以直接作为 sing-box 远程规则集使用。

## 文件说明

| 文件 | 说明 |
| --- | --- |
| `srs/AD server lists.srs` | 将下面 6 个列表合并并去重后的聚合 SRS 文件，可以直接引用，推荐优先使用 |
| `srs/Adblock4limbo.srs` | 基于 Limbopro / Adblock4limbo 的规则 |
| `srs/BanAD.srs` | BanAD 广告规则 |
| `srs/easylist.srs` | EasyList 广告规则 |
| `srs/easylistchina.srs` | EasyList China 中文补充规则 |
| `srs/easyprivacy.srs` | EasyPrivacy 隐私追踪规则 |
| `srs/Peter_Lowe_adservers.srs` | Peter Lowe ad servers 规则 |
| `my_ad_filter_merged.list` | 结合各大广告过滤平台规则，合并并去重后生成的 `.list` 文件，可用于 Surge 和 Quantumult X |
| `srs/my_ad_filter_merged.srs` | 基于 `my_ad_filter_merged.list` 中 sing-box 可表达的域名、关键词和 IP 规则生成的 SRS 文件 |
| `*.list` | 对应的文本规则源文件，便于查看和重新生成 |

## Surge / Quantumult X 使用方式

`my_ad_filter_merged.list` 是结合各大广告过滤平台给出的规则，去重之后合并生成的广告过滤列表。这个 `.list` 文件保留在仓库根目录，可以直接给 Surge 和 Quantumult X 引用：

```text
https://raw.githubusercontent.com/Anonymous0632/Ad-Server-Lists/main/my_ad_filter_merged.list
```

Quantumult X 可在 `filter_remote` 中引用；Surge 可作为远程规则集引用，并将匹配到的广告规则指向 `REJECT` 或你自己的广告拦截策略。

## sing-box 使用方式

把下面片段合并到现有 sing-box 配置的 `route` 部分中。示例使用聚合规则文件：

`srs/AD server lists.srs` 是将 `Adblock4limbo`、`BanAD`、`easylist`、`easylistchina`、`easyprivacy`、`Peter_Lowe_adservers` 这 6 个列表合并并去重后的文件，可以直接进行引用。

```json
{
  "route": {
    "rule_set": [
      {
        "type": "remote",
        "tag": "ad-server-lists",
        "format": "binary",
        "url": "https://raw.githubusercontent.com/Anonymous0632/Ad-Server-Lists/main/srs/AD%20server%20lists.srs",
        "update_interval": "1d"
      }
    ],
    "rules": [
      {
        "rule_set": "ad-server-lists",
        "action": "reject",
        "method": "drop"
      }
    ]
  }
}
```

如果只想使用某个单独规则文件，把 `url` 改成对应的 raw 地址，并同步修改 `tag` 即可。例如：

```text
https://raw.githubusercontent.com/Anonymous0632/Ad-Server-Lists/main/srs/easyprivacy.srs
```

如果想在 sing-box 中使用新增加的聚合广告列表，可以引用由 `my_ad_filter_merged.list` 生成的 SRS 文件：

```text
https://raw.githubusercontent.com/Anonymous0632/Ad-Server-Lists/main/srs/my_ad_filter_merged.srs
```

## 注意事项

- `.srs` 是 sing-box 的 binary rule-set 格式，适合直接加载；`.list` 文件可以用于 Surge / Quantumult X，也可用于查看、留档或重新构建。
- `my_ad_filter_merged.list` 中的 Surge / Quantumult X 规则会完整保留；对应的 SRS 文件只包含 sing-box 路由规则集可表达的域名、关键词和 IP 规则。
- 远程规则集更新后，sing-box 会按 `update_interval` 定期拉取；也可以重启客户端触发重新加载。
- 本仓库规则基于 Limbopro / Adblock4limbo 规则整理构建，并整合 EasyList、EasyList China、EasyPrivacy、Peter Lowe ad servers、BanAD 以及其他广告过滤平台规则源。上游规则的版权和许可请以各自项目说明为准。

## 参考

- Limbopro / Adblock4limbo: https://github.com/limbopro/Adblock4limbo
- sing-box rule-set 文档: https://sing-box.sagernet.org/configuration/rule-set/
- sing-box route rule 文档: https://sing-box.sagernet.org/configuration/route/rule/
- sing-box rule action 文档: https://sing-box.sagernet.org/configuration/route/rule_action/
