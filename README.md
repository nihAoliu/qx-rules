# QX AI 分流规则

由 `nihAoliu` 自行维护，初始内容来自 [ddgksf2013 的 Ai.yaml](https://ddgksf2013.top/filter/Ai.yaml)，保留原作者 [ddgksf2021](https://t.me/ddgksf2021) 的来源署名。

2026-10-09 将原有 91 条规则拆成 Claude 专用与其他 AI 两份，方便为 Claude 单独选择节点。此次仅拆分现有规则，没有全量同步上游或扩大域名范围。

## 订阅地址

| 文件 | 规则数 | 用途 |
| --- | ---: | --- |
| [Claude.list](filter/Claude.list) | 4 | Claude / Anthropic 专用域名 |
| [Ai-Other.list](filter/Ai-Other.list) | 87 | 其他 AI 及现有共享依赖 |
| [Ai.list](filter/Ai.list) | 91 | 原有总表，保留兼容旧订阅 |
| [Ai.yaml](filter/Ai.yaml) | 91 | 初始来源快照，不作为新配置的订阅入口 |

新配置使用以下两个 Quantumult X 原生格式地址：

```text
https://raw.githubusercontent.com/nihAoliu/qx-rules/main/filter/Claude.list
https://raw.githubusercontent.com/nihAoliu/qx-rules/main/filter/Ai-Other.list
```

在已有配置的 `[filter_remote]` 段中加入：

```ini
https://raw.githubusercontent.com/nihAoliu/qx-rules/main/filter/Claude.list, tag=Claude, force-policy=Claude, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/nihAoliu/qx-rules/main/filter/Ai-Other.list, tag=其他AI服务, force-policy=AI服务, update-interval=86400, opt-parser=false, enabled=true
```

配置中必须已有名为 `Claude` 和 `AI服务` 的策略组；也可以把示例中的名称改成自己的策略组名称。文件内默认策略为 `proxy`，资源上的 `force-policy` 会覆盖它。原生格式无需资源解析器。

将 Claude 资源放在其他 AI 和通用代理规则前面。确认这两份资源加载成功后，停用旧的 `Ai.list` 或 `Ai-All-In-One` 订阅，避免重复加载。规则匹配也受 QX 匹配优化设置影响，应结合实际请求记录验证。

## Claude 的范围与节点选择

`Claude.list` 包含以下域名及其子域名：

- `anthropic.com`
- `claude.ai`
- `claude.com`
- `claudeusercontent.com`

原 Claude 分类中的 `cdn.usefathom.com` 是共享统计域名，保留在 `Ai-Other.list`。本次没有将 `cloudflare.com`、`amazonaws.com` 等共享服务范围放入 Claude 专用规则。

规则只决定流量使用哪个策略。若希望 Claude 保持一个具体出口，应在专用手动策略中选定节点；选择自动地区组时仍可能自动换节点。固定节点名也不保证服务商永远不更换出口 IP。

Cursor、Poe 等第三方工具若通过自己的后端调用 Claude，QX 看到的是工具自身域名，不能仅凭工具内选择的模型名称识别为 Claude。

## 维护方式

- 初始复制日期：2026-09-30；来源快照标注更新日期：2026-08-18。
- 分拆日期：2026-10-09；分拆时两份新列表无重复，合并后与原 91 条规则完全一致。
- 日常编辑 `Claude.list` 或 `Ai-Other.list`；如果还需兼容旧订阅，手动同步对应变更至 `Ai.list`，避免重新引入 Claude 与其他 AI 的重叠。
- `Ai.yaml` 保留初始来源快照，不自动与任何列表同步。
- 本仓库未配置自动同步上游，后续上游变化不会覆盖自定义修改。
- `Ai-Other.list` 保留原有共享域名，例如 `amazonaws.com`、`cloudflare.com`、`googleapis.com`；它们也可能使部分非 AI 流量使用 AI 策略。本次未缩窄这些范围。
- 修改后在 QX 中刷新远程资源；GitHub 原始文件地址可能有短暂缓存。
- 此公开仓库仅存放规则和说明，不存放机场订阅地址、节点凭据或私人配置。

每行一条规则，使用英文逗号，以 `#` 开头的行是注释：

```text
host, example.com, proxy
host-suffix, example.org, proxy
```

QX 格式与 `force-policy` 用法参考 [Quantumult X 官方示例](https://github.com/crossutility/Quantumult-X/blob/master/sample.conf)。
