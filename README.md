# Ad Server Lists

这是基于 Limbopro 的规则构建的，适用于 sing-box 广告屏蔽的 SRS 文件。

本仓库保存了广告屏蔽规则的文本源文件和已经转换好的 sing-box binary rule-set 文件。`srs/` 目录里的 `.srs` 文件可以直接作为 sing-box 远程规则集使用。

## 文件说明

| 文件 | 说明 |
| --- | --- |
| `srs/AD server lists.srs` | 聚合后的广告屏蔽 SRS 文件，推荐优先使用 |
| `srs/Adblock4limbo.srs` | 基于 Limbopro / Adblock4limbo 的规则 |
| `srs/BanAD.srs` | BanAD 广告规则 |
| `srs/easylist.srs` | EasyList 广告规则 |
| `srs/easylistchina.srs` | EasyList China 中文补充规则 |
| `srs/easyprivacy.srs` | EasyPrivacy 隐私追踪规则 |
| `srs/Peter_Lowe_adservers.srs` | Peter Lowe ad servers 规则 |
| `*.list` | 对应的文本规则源文件，便于查看和重新生成 |

## sing-box 使用方式

把下面片段合并到现有 sing-box 配置的 `route` 部分中。示例使用聚合规则文件：

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

## 注意事项

- `.srs` 是 sing-box 的 binary rule-set 格式，适合直接加载；`.list` 文件主要用于查看、留档或重新构建。
- 远程规则集更新后，sing-box 会按 `update_interval` 定期拉取；也可以重启客户端触发重新加载。
- 本仓库规则基于 Limbopro / Adblock4limbo 规则整理构建，并整合 EasyList、EasyList China、EasyPrivacy、Peter Lowe ad servers、BanAD 等规则源。上游规则的版权和许可请以各自项目说明为准。

## 参考

- Limbopro / Adblock4limbo: https://github.com/limbopro/Adblock4limbo
- sing-box rule-set 文档: https://sing-box.sagernet.org/configuration/rule-set/
- sing-box route rule 文档: https://sing-box.sagernet.org/configuration/route/rule/
- sing-box rule action 文档: https://sing-box.sagernet.org/configuration/route/rule_action/
