# QX AI 分流规则

由 `nihAoliu` 自行维护的 AI 分流规则，初始内容来自 [ddgksf2013 的 Ai.yaml](https://ddgksf2013.top/filter/Ai.yaml)，保留原作者 [ddgksf2021](https://t.me/ddgksf2021) 的来源署名。

初始版本包含 91 条规则：23 条完整域名匹配、68 条域名后缀匹配。覆盖 ChatGPT / OpenAI、Claude、Gemini、NotebookLM、Cursor、Copilot、Perplexity、Grok、Poe、Midjourney、Manus 等服务。

## 订阅地址

Quantumult X 原生格式：

```text
https://raw.githubusercontent.com/nihAoliu/qx-rules/main/filter/Ai.list
```

在 QX 的「分流」中添加远程规则资源，填入此地址，并将该资源的策略指定为你原来使用的 AI 策略组或节点。本文件使用 QX 原生格式，无需开启资源解析器。

也可以在现有配置的 `[filter_remote]` 段内添加下面这一行。先将 `你的AI策略组名` 替换成配置里已经存在的策略组名称；不要重复添加该段标题。

```ini
https://raw.githubusercontent.com/nihAoliu/qx-rules/main/filter/Ai.list, tag=AI, force-policy=你的AI策略组名, enabled=true
```

文件内各条规则的默认策略为 `proxy`，资源上的 `force-policy` 会覆盖它。建议沿用旧 AI 规则的策略和位置，确认新资源加载成功后，再停用旧的 `ddgksf2013.top/filter/Ai.yaml` 订阅。

## 自己修改规则

在 GitHub 中编辑 [`filter/Ai.list`](filter/Ai.list) 并保存即可。随后在 QX 中刷新该远程资源；GitHub 原始文件地址可能有短暂缓存。

完整域名匹配示例：

```text
host, chatgpt.com, proxy
```

匹配域名及其子域名的示例：

```text
host-suffix, openai.com, proxy
```

每行一条规则，使用英文逗号；以 `#` 开头的行是注释。分流文件决定流量使用哪个策略，实际能否访问对应服务还取决于所选节点。

## 来源与维护方式

- 初始复制日期：2026-09-30；源文件标注更新日期：2026-08-18。
- [`filter/Ai.list`](filter/Ai.list) 为 QX 日常使用和维护的文件。
- [`filter/Ai.yaml`](filter/Ai.yaml) 保留初始来源快照，采用 Clash classical rule-provider 格式；不与 `Ai.list` 自动同步。
- 本仓库未配置自动同步上游。上游后续变化不会覆盖你的自定义修改。
- 初始版本保留原有全部域名和匹配范围，包括 `amazonaws.com`、`cloudflare.com`、`googleapis.com` 等共享服务域名；这些规则也会使部分非 AI 流量使用同一策略。

QX 格式与 `force-policy` 用法参考 [Quantumult X 官方示例](https://github.com/crossutility/Quantumult-X/blob/master/sample.conf)。
