# SnipLite 官方插件目录

在 SnipLite 中打开「首选项 → 插件 → 发现插件」即可安装或更新目录中的插件。也可以按类别找到插件，下载 `.snipplugin` 文件后手动导入。

| 类别 | 插件 |
| --- | --- |
| [截图](plugins/capture/) | [长截图](plugins/capture/scroll-capture/) |

## 提交插件

插件发布后，可以通过 Pull Request 添加到对应的 `plugins/<类别>/<插件>/`，并更新分类列表和 [catalog.json](catalog.json)。类别目录名与 `catalog.json` 中的 `category` 一致。

SnipLite 通过 [在线目录](https://gxdongv.github.io/SnipLite-Plugin-Catalog/catalog.json) 读取发布者、版本、兼容要求、下载地址和 SHA-256。填写目录时，版本与兼容字段取自插件包的清单，SHA-256 取自实际发布的安装包。
