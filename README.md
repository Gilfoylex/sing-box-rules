# sing-box-rules

sing-box 规则集。

## 规则每月更新

工作流：`.github/workflows/update-cn-rules.yml`。

- 每月 1 日北京时间 08:00 自动运行，GitHub 调度可能延迟。
- 支持在 Actions 页面选择 **Update rules monthly**，从 `main` 分支手动运行。
- 定时运行要求该工作流位于仓库默认分支 `main`，且仓库已启用 Actions。

规则来源：

1. https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/cn.json
2. https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/geolocation-cn.json
3. https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geoip/cn.json
4. https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/category-ai-!cn.json
5. https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/category-porn.json
6. https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/gfw.json
7. https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/apple@cn.json
8. https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/microsoft@cn.json
9. https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/geolocation-!cn.json

使用固定版本 sing-box `1.14.3` 的 `rule-set merge` 合并两份域名规则 JSON，输出 `cn-site.json`，再用 `rule-set compile` 编译为 `cn-site.srs`。升级编译器时修改工作流的 `SING_BOX_VERSION`。

第三个来源是中国 IP 规则，下载后保留原始 JSON 内容并命名为 `cn-ip.json`，使用同一版本 sing-box 编译为 `cn-ip.srs`。

第四、第五个来源分别是境外 AI 服务和成人内容域名规则，下载后保留原始 JSON 内容，分别命名为 `ai-site.json`、`porn-site.json`，编译为 `ai-site.srs`、`porn-site.srs`。成人内容规则使用包含 `/geo/geosite/` 的路径，`/sing/geosite/category-porn.json` 返回 HTTP 404。

第六个来源是 GFW 域名规则，下载后保留原始 JSON 内容并命名为 `gfw-site.json`，使用同一版本 sing-box 编译为 `gfw-site.srs`。

第七、第八个来源分别是 Apple 和 Microsoft 带有 `@cn` 标签的域名规则，下载后保留原始 JSON 内容，分别命名为 `apple-cn-site.json`、`microsoft-cn-site.json`，编译为 `apple-cn-site.srs`、`microsoft-cn-site.srs`。

第九个来源是上游归类为非中国的域名规则，下载后保留原始 JSON 内容并命名为 `not-cn-site.json`，编译为 `not-cn-site.srs`。

发布产物：

| 规则 | JSON | SRS |
| --- | --- | --- |
| 中国域名 | `cn-site.json` | `cn-site.srs` |
| 中国 IP | `cn-ip.json` | `cn-ip.srs` |
| 境外 AI 域名 | `ai-site.json` | `ai-site.srs` |
| 成人内容域名 | `porn-site.json` | `porn-site.srs` |
| GFW 域名 | `gfw-site.json` | `gfw-site.srs` |
| Apple 中国域名 | `apple-cn-site.json` | `apple-cn-site.srs` |
| Microsoft 中国域名 | `microsoft-cn-site.json` | `microsoft-cn-site.srs` |
| 非中国域名 | `not-cn-site.json` | `not-cn-site.srs` |

全部成功后，将这十六个产物一起提交到 `release` 分支。首次运行会创建该分支；已有分支的其他文件和提交历史保留。产物没有变化时不创建提交，任意来源下载、校验、合并或编译失败时不发布。

使用 GitHub 自带的 `GITHUB_TOKEN`，无需额外 Secret；仓库策略需允许 Actions 写入，分支保护需允许工作流推送。
