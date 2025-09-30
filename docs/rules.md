# 规则

organize 配置文件可以使用 [YAML](https://learnxinyminutes.com/docs/yaml/) 或 [JSON](https://learnxinyminutes.com/docs/json/) 编写。请参阅 [configuration](configuration.md) 以了解如何定位您的配置文件。

顶级元素必须是一个包含 "rules" 键的字典。"rules" 包含一个对象列表，每个对象必须包含 "locations" 和 "actions" 键。

最小配置：

```yaml
rules:
  - locations: "~/Desktop"
    actions:
      - echo: "Hello World!"
```

Organize 从上到下检查您的规则。对于每个位置中的每个资源（从上到下），它将检查过滤器是否适用（从上到下），然后执行给定的操作（从上到下）。

因此，使用此最小配置，它将在您的桌面上找到的每个文件上打印 "Hello World!"。

## 规则选项

```yml
rules:
  # 第一个规则
  - name: ...
    enabled: ...
    targets: ...
    locations: ...
    subfolders: ...
    filter_mode: ...
    filters: ...
    actions: ...
    tags: ...

  # 另一个规则
  - name: ...
    enabled: ...
    # ... 等等
```

规则选项详情：

- **name** (`str`)：规则名称
- **enabled** (`bool`)：规则是否启用/禁用 _(默认：`true`)_
- **targets** (`str`)：`"dirs"` 或 `"files"` _(默认：`"files"`)_
- **locations** (`str`|`list`) - 单个位置字符串或 [locations](locations.md) 列表
- **subfolders** (`bool`)：是否递归进入所有位置的子文件夹 _(默认：`false`)_
- **filter_mode** (`str`)：过滤器必须应用 `"all"`、`"any"` 或 `"none"` _(默认：`"all"`)_
- **filters** (`list`)：[filters](filters.md) 列表 _(默认：`[]`)_
- **actions** (`list`)：[actions](actions.md) 列表
- **tags** (`list`)：[tags](configuration.md#running-specific-rules-of-your-config) 列表

## 针对目录

当 `targets` 设置为 `dirs` 时，organize 将对文件夹而不是文件进行操作。

过滤器会自动调整其含义。例如，`size` 过滤器会汇总给定文件夹中所有文件的大小，而不是返回单个文件的大小。

当然，其他过滤器如 `exif` 或 `filecontent` 不适用于文件夹，将返回错误。

## 模板和占位符

占位符变量使用大括号 `{var}`。

这些变量 **始终可用**：

`{env}` (`dict`)<br>
您的所有环境变量。您可以像这样访问单个环境变量：`{env.MY_VARIABLE}`。

`{path}` ([`pathlib.Path`](https://docs.python.org/3/library/pathlib.html#methods-and-properties))<br>
本地硬盘上当前文件/文件夹的完整路径。

`{relative_path}` (`str`)<br>
当前文件或目录的相对路径。

`{now()}` (`datetime`)<br>
本地时区的当前日期时间。

`{utcnow()}` (`datetime`)<br>
当前的 UTC 日期时间。

`{today()}` (`date`)<br>
今天的日期。

此外，几乎所有过滤器都会添加新的占位符，其中包含有关当前处理的文件/文件夹的信息。

访问文件大小和哈希的示例：

```yaml
rules:
  - locations: ~/Desktop
    filters:
      - size
      - hash
    actions:
      - echo: "{size} {hash}"
```

!!! note

    要使用过滤器返回的值，必须将其列在过滤器中！

## 高级：别名

与其在每个规则中重复相同的 locations/actions/filters，您可以使用别名来引用多个位置，然后在每个规则中引用它。

别名是 YAML 语法的标准功能。

```yml
all_my_messy_folders: &all
  - ~/Desktop
  - ~/Downloads
  - ~/Documents
  - ~/Dropbox

rules:
  - locations: *all
    filters: ...
    actions: ...

  - locations: *all
    filters: ...
    actions: ...
```

您甚至可以使用多个文件夹列表：

```yml
private_folders: &private
  - "/path/private"
  - "~/path/private"

work_folders: &work
  - "/path/work"
  - "~/My work folder"

all_folders: &all
  - *private
  - *work

rules:
  - locations: *private
    filters: ...
    actions: ...

  - locations: *work
    filters: ...
    actions: ...

  - locations: *all
    filters: ...
    actions: ...

  # 与 *all 相同
  - locations:
      - *work
      - *private
    filters: ...
    actions: ...
```