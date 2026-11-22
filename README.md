# SnipLite 官方插件目录

在 SnipLite 中打开「首选项 → 插件 → 发现插件」即可安装或更新目录中的插件。也可以按类别找到插件，下载 `.snipplugin` 文件后手动导入。

| 类别 | 插件 |
| --- | --- |
| [截图](plugins/capture/) | [长截图](plugins/capture/scroll-capture/) |

## 「发现插件」如何工作

GitHub Pages 将本仓库 `main` 分支根目录的 `catalog.json` 发布到固定网址。支持目录功能的 SnipLite 在打开或刷新「发现插件」时读取这个文件，按插件 ID、已装版本和兼容要求展示安装或更新选项。 [在线目录 JSON](https://gxdongv.github.io/SnipLite-Plugin-Catalog/catalog.json)

用户确认安装或更新后，SnipLite 从 `downloadUrl` 下载 `.snipplugin`，核对文件的 SHA-256，再检查包内 `plugin.json` 的 ID、版本和兼容信息。更新在重启后生效。仓库中的 `plugins/<类别>/<插件>/` 用于在 GitHub 上按类别浏览。

## 发布和收录插件

1. **打包并试装。** 构建版本化的 `.snipplugin`，通过 SnipLite 的「首选项 → 插件」手动导入并启用，确认功能可用。发布新版时沿用原插件 ID，并提高包内 `plugin.json` 的 `version`。 [插件接入与打包规范](https://github.com/GxDongV/SnipLite/blob/main/docs/PLUGIN_DEVELOPMENT.zh-CN.md)
2. **发布安装包。** 在本仓库的 **Releases → Draft a new release** 创建 Release，使用带插件名和版本的标签，例如 `scroll-capture-v0.1.7`。上传 `.snipplugin` 附件并发布。插件作者也可以在自己的仓库发布，由这个目录统一收录。 [长截图发布示例](https://github.com/GxDongV/SnipLite-Plugin-Catalog/releases/tag/scroll-capture-v0.1.7)
3. **取得下载链接和校验值。** 复制 Release 里 `.snipplugin` 附件的 HTTPS 直链，从该地址下载文件，再用下面的 PowerShell 命令读取 SHA-256 的 `Hash` 值。

   ```powershell
   Get-FileHash -Algorithm SHA256 -LiteralPath '.\sniplite.scroll-capture-0.1.7.snipplugin'
   ```

4. **提交目录改动。** 从 `main` 开分支，更新根目录的 `catalog.json`。新插件向 `plugins` 数组添加一条记录，已有插件按原 ID 更新对应条目的版本、下载地址和校验值。新增或调整分类时，同时更新 `plugins/<类别>/<插件>/` 下的说明和 README 分类列表。通过 Pull Request 合并到 `main`。
5. **验证已上线。** 等待 GitHub Pages 部署完成，确认在线 JSON 出现新条目，再在 SnipLite 的「发现插件」点击「刷新插件目录」，试一次安装或更新。

## `catalog.json` 格式

根节点的 `schemaVersion` 是目录文件的格式版本，目前为数字 `1`。`plugins` 是插件条目数组，每个 ID 保留一条记录，ID 比较忽略大小写。`plugin.json` 位于安装包内，声明该包的身份、版本和加载信息。

| 字段 | 用途 |
| --- | --- |
| `id` | 插件的唯一 ID，发布新版时沿用，例如 `sniplite.scroll-capture`。与包内清单一致。 |
| `name`、`description`、`author` | 「发现插件」显示的名称、简介和作者。 |
| `nameZhCn`、`descriptionZhCn` | 可选的中文名称和简介。中文界面优先使用这两个字段。 |
| `version` | 插件版本字符串，例如 `"0.1.7"`。客户端用它比较可用版本和已装版本。 |
| `apiVersion` | 插件 API 主版本，使用数字，例如 `1`。 |
| `minHostVersion` | 最低支持的 SnipLite 版本字符串，例如 `"0.3.16"`。 |
| `architecture` | 插件支持的进程架构，例如 `any` 或 `x64`。 |
| `downloadUrl` | 指向实际安装包的 HTTPS 直链。 |
| `sha256` | 安装包的 SHA-256，填写 64 个十六进制字符。 |
| `category` | 仓库中的分类目录名，例如 `capture` 对应「截图」。 |

`id`、`version`、`apiVersion`、`minHostVersion` 和 `architecture` 取自发布包内的 `plugin.json`。下面是长截图 0.1.7 的完整目录示例。

```json
{
  "schemaVersion": 1,
  "plugins": [
    {
      "id": "sniplite.scroll-capture",
      "name": "Scroll Capture",
      "nameZhCn": "长截图",
      "author": "GxDongV",
      "description": "Capture and stitch a scrolling region automatically or manually.",
      "descriptionZhCn": "自动或手动滚动选区，并拼接成长截图。",
      "version": "0.1.7",
      "apiVersion": 1,
      "minHostVersion": "0.3.16",
      "architecture": "any",
      "downloadUrl": "https://github.com/GxDongV/SnipLite-Plugin-Catalog/releases/download/scroll-capture-v0.1.7/sniplite.scroll-capture-0.1.7.snipplugin",
      "sha256": "9c1e196a6c9f778be919477f00d0c7a435b6c67006a20213a34d9373b5301a04",
      "category": "capture"
    }
  ]
}
```
