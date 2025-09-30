# 从旧版本迁移

首先，感谢您长期使用 `organize`！

我试图保持破坏性更改的数量最小，但无法完全避免它们。随意固定 organize 任何您需要的版本，但那样您就错过了派对。

如果您需要帮助迁移配置文件，请在 Github 上打开一个 issue！

<hr>

## 从 v2 迁移到 v3

### 位置

在 organize v3 中，远程文件系统不再受支持。您必须从配置中移除所有 `filesystem` 参数，并且不能再在 `location` 中使用 pyfilesystem URL。

### 占位符

- 使用 `{now()}` 而不是 `{now}`。
- 使用 `{utcnow()}` 而不是 `{utcnow}`。
- 占位符 `{fs}` 和 `{fs_path}` 不再可用。

### 命令行界面

命令行界面发生了很大变化！更新任何使用 CLI 的脚本到新选项：

- `organize check --debug` 变为 `organize debug`
- `organize reveal` 变为 `organize show --reveal`
- `organize reveal --path` 变为 `organize show --path`
- `organize schema` 不再受支持。
- 已弃用的 `--config-file` 选项现在已移除。


就是这样。如果您在迁移过程中遇到任何其他 bug 或问题，请联系我们！

<hr>

## 从 v1 迁移到 v2


### 文件夹

文件夹在 organize v2 中已成为 [位置](locations.md)。

- `folders` 必须在您的配置中重命名为 `locations`。
- 移除：Glob 语法 (`/Docs/**/*.png`)。
  参见 [位置选项](locations.md#location-options)。
- 移除：感叹号排除语法 (`! ~/Desktop/exclude`)。
  参见 [位置选项](locations.md#location-options)。
- 所有键（过滤器名称、操作名称、选项名称）现在必须小写。

### 占位符

organize v2 使用 Jinja 模板引擎。您可能需要更改一些占位符。

- `{basedir}` 不再可用。
- 您必须替换未记录的占位符，如：

```yaml
"{created.year}-{created.month:02}-{created.day:02}"
```

为：

```yaml
"{created.strftime('%Y-%m-%d')}"
```

如果您需要左填充其他数字，您现在可以使用以下语法：

```yaml
"{'%02d' % your_variable}"
# 或
"{ '{:02}'.format(your_variable) }"
```

### 过滤器

- [`filename`](filters.md#name) 重命名为 `name`。
- [`filesize`](filters.md#size) 重命名为 `size`。
- [`created`](filters.md#created) 不再接受时区，默认使用本地时区。
- [`lastmodified`](filters.md#lastmodified) 不再接受时区，默认使用本地时区。
- [`extension`](filters.md#extension) `lower` 和 `upper` 现在是函数，必须像这样调用：
  `"{extension.upper()}"` 和 `"{extension.lower()}"`。

### 操作

复制、移动和重命名操作变得更强大了。您现在有几个冲突选项，可以指定在冲突情况下文件应如何重命名。

这意味着您可能需要更改配置以使用新参数。

- [`copy`](actions.md#copy) 参数更改为支持冲突解决选项。
- [`move`](actions.md#move) 参数更改为支持冲突解决选项。
- [`rename`](actions.md#rename) 参数更改为支持冲突解决选项。

示例：

```yml
rules:
  - folders: ~/Desktop
    filters:
      - extension: pdf
    actions:
      - move:
          dest: ~/Documents/PDFs/
          overwrite: false
          counter_seperator: "-"
```

变为 (organize v2)：

```yaml
rules:
  - locations: ~/Desktop
    filters:
      - extension: pdf
    actions:
      - move:
          dest: ~/Documents/PDFs/
          on_conflict: rename_new
          rename_template: "{name}-{counter}{extension}"
```

如果您使用 `move`、`copy` 或 `rename` 而不带参数，对您来说没什么变化。

### 设置

`system_files` 设置已被移除。为了在搜索中包含系统文件，请使用空列表覆盖默认的 [`system_exclude_files`](locations.md#location-options)：

```yaml
rules:
  - locations:
      - path: ~/Desktop/
        system_exclude_files: []
        system_exclude_dirs: []
    filters:
      - name: .DS_Store
    actions:
      - trash
```

就是这样。再次，如果您在迁移配置时遇到问题，请随意打开一个 issue。
