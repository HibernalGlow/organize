# 位置

**位置** 是 organize 搜索资源的文件夹。
您可以为每个规则设置多个位置。

最小位置定义只是一个查找文件/文件夹的路径：

```yml
rules:
  - locations: ~/Desktop
    actions: ...
```

如果您想在规则中处理多个位置，请创建列表：

```yml
rules:
  - locations:
      - ~/Desktop
      - /usr/bin/
      - "%PROGRAMDATA%/test"
    actions: ...
```

使用选项：

```yml
rules:
  - name: "位置列表"
    locations:
      - path: "~/Desktop"
        max_depth: 3
    actions: ...
```

## 位置选项

```yml
rules:
  - locations:
      - path: ...
        min_depth: ...
        max_depth: ...
        search: ...
        exclude_files: ...
        exclude_dirs: ...
        system_exclude_files: ...
        system_exclude_dirs: ...
        ignore_errors: ...
        filter: ...
        filter_dirs: ...
```

**path** (`str`)<br>
本地文件夹的路径

**min_depth** (`int` 或 `null`)<br>
搜索的最小目录深度。如果您只想处理 `location` 子目录中的文件，这很有用。

**max_depth** (`int` 或 `null`)<br>
搜索的最大目录深度。

**search** (`"breadth"` 或 `"depth"`)<br>
是否使用广度或深度搜索来递归进入子文件夹。请注意，如果您想从此位置移动或删除文件，则必须将其设置为 `"depth"`。
_(默认: `"depth"`)_

**exclude_files** (`List[str]`)<br>
在此位置应排除的文件名模式列表，例如 `["~*"]`。

**exclude_dirs** (`List[str]`)<br>
将用于在此位置过滤目录名称的文件夹名称模式列表。例如 `["do-not-move", "*-Important", "Backup*"]`

**system_exclude_files** (`List[str]`)<br>
默认排除的文件名模式列表。默认为：
`["thumbs.db", "desktop.ini", "~$*", ".DS_Store", ".localized"]`。使用 `[]` 覆盖以包含系统文件。

**system_exclude_dirs** (`List[str]`)<br>
默认排除的文件夹名称模式列表（`['.git', '.svn']`）。使用 `[]` 覆盖以包含系统目录。

**ignore_errors** (`bool`)<br>
如果为 `true`，读取位置时的任何错误将被忽略。

**filter** (`List[str]`)<br>
在此位置应使用的文件名模式列表，例如 `["*.py"]`。
所有其他文件将被跳过。

**filter_dirs** (`List[str]`)<br>
匹配在此位置包含的目录名称的模式列表。
所有其他目录将被跳过。

### `max_depth` 和 `subfolders`

- 如果在规则上指定了 `subfolders: true`，所有位置默认设置为 `max_depth: null`。
- 位置中的 `max_depth` 设置优先于规则的 `subfolders` 设置。

## 位置中的环境变量

您可以在位置中使用环境变量。您可以通过 `{env}` 占位符或以美元符号为前缀访问它们。

示例：

```yaml
rules:
  - locations:
      # 通过 {env} - "" 这里很重要！
      - "{env.MY_FOLDER}"

      # 通过 $ - 与上面相同。
      - "$MY_FOLDER"

      # 带位置选项
      - path: "{env.OTHER_FOLDER}/Inbox/Invoices"
        max_depth: null
    actions:
      - echo: "{path}"
```

## 相对位置

位置可以是相对的。这允许您创建简单的临时规则，可以在项目之间复制。

有一个命令行选项来更改工作目录，如果您需要的话。

**huge-pic-warner.yaml:**

```yaml
rules:
  - locations: "docs" # 这里 "docs" 相对于当前工作目录
    filters:
      - extension: jpg
      - size: ">3 MB"
    actions:
      - echo: "警告 - 找到巨大图片！"
```

然后使用运行：

```sh
organize sim huge-pic-warner.yaml --working-dir=some/other/dir/
```
