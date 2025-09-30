# 操作

此页面显示每个操作的具体信息。有关基本操作用法和选项，请查看 [规则](rules.md) 部分。

## 确认

::: organize.actions.Confirm

**示例**

在删除重复文件前确认

```yaml
rules:
  - name: "删除重复文件时确认"
    locations:
      - ~/Downloads
      - ~/Documents
    filters:
      - not empty
      - duplicate
      - name
    actions:
      - confirm: "删除 {name}？"
      - trash
```

## 复制

::: organize.actions.Copy

**示例：**

将所有 PDF 文件复制到 `~/Desktop/somefolder/` 并保留文件名

```yaml
rules:
  - locations: ~/Desktop
    filters:
      - extension: pdf
    actions:
      - copy: "~/Desktop/somefolder/"
```

使用占位符将所有 .pdf 文件复制到 "PDF" 文件夹，将所有 .jpg 文件复制到 "JPG" 文件夹。现有文件将被覆盖。

```yaml
rules:
  - locations: ~/Desktop
    filters:
      - extension:
          - pdf
          - jpg
    actions:
      - copy:
          dest: "~/Desktop/{extension.upper()}/"
          on_conflict: overwrite
```

使用占位符将所有 .pdf 文件复制到 "PDF" 文件夹，将所有 .jpg 文件复制到 "JPG" 文件夹。如果两个文件共享相同的文件名且为重复文件，则跳过重复文件。如果不是重复文件，则第二个文件将被重命名。

```yaml
rules:
  - locations: ~/Desktop
    filters:
      - extension:
          - pdf
          - jpg
    actions:
      - copy:
          dest: "~/Desktop/{extension.upper()}/"
          on_conflict: deduplicate
```

复制到文件夹 `Invoices`。保留文件名但不覆盖现有文件。
为了防止覆盖文件，会在文件名中添加索引，因此 `somefile.jpg` 变为 `somefile 2.jpg`。
计数器分隔符默认为 `' '` ，但可以使用 `counter_separator` 属性更改。

```yaml
rules:
  - locations: ~/Desktop/Invoices
    filters:
      - extension:
          - pdf
    actions:
      - copy:
          dest: "~/Documents/Invoices/"
          on_conflict: "rename_new"
          rename_template: "{name} {counter}{extension}"
```

## 删除

::: organize.actions.delete.Delete

**示例：**

删除旧下载文件。

```yaml
rules:
  - locations: "~/Downloads"
    filters:
      - lastmodified:
          days: 365
      - extension:
          - png
          - jpg
    actions:
      - delete
```

删除所有空子文件夹

```yaml
rules:
  - name: 删除所有空子文件夹
    locations:
      - path: "~/Downloads"
        max_depth: null
    targets: dirs
    filters:
      - empty
    actions:
      - delete
```

## 回显

::: organize.actions.Echo

**示例：**

```yaml
rules:
  - name: "查找超过一年的文件"
    locations: ~/Desktop
    filters:
      - lastmodified:
          days: 365
    actions:
      - echo: "找到旧文件"
```

为桌面上的每个文件打印 "Hello World!" 和文件路径：

```yaml
rules:
  - locations:
      - ~/Desktop
    actions:
      - echo: "Hello World! {path}"
```

这将为桌面上的每个文件打印类似 `找到 ZIP: "backup"` 的内容

```yaml
rules:
  - locations:
      - ~/Desktop
    filters:
      - extension
      - name
    actions:
      - echo: '找到 {extension.upper()}: "{name}"'
```

显示 '~/Downloads'、'~/Desktop' 及其子文件夹中所有文件的 `{relative_path}` 和 `{path}`：

```yaml
rules:
  - locations:
      - path: ~/Desktop
        max_depth: null
      - path: ~/Downloads
        max_depth: null
    actions:
      - echo: "路径:     {path}"
      - echo: "相对路径: {relative_path}"
```

## 硬链接

::: organize.actions.Hardlink

## macOS 标签

::: organize.actions.MacOSTags

**示例：**

```yaml
rules:
  - name: "添加单个标签"
    locations: "~/Documents/Invoices"
    filters:
      - name:
          startswith: "Invoice"
      - extension: pdf
    actions:
      - macos_tags: Invoice
```

添加多个标签（"Invoice" 和 "Important"）

```yaml
rules:
  - locations: "~/Documents/Invoices"
    filters:
      - name:
          startswith: "Invoice"
      - extension: pdf
    actions:
      - macos_tags:
          - Important
          - Invoice
```

指定标签颜色

```yaml
rules:
  - locations: "~/Documents/Invoices"
    filters:
      - name:
          startswith: "Invoice"
      - extension: pdf
    actions:
      - macos_tags:
          - Important (green)
          - Invoice (purple)
```

添加带颜色的模板标签

```yaml
rules:
  - locations: "~/Documents/Invoices"
    filters:
      - created
    actions:
      - macos_tags:
          - Year-{created.year} (red)
```

## 移动

::: organize.actions.Move

**示例：**

将桌面上的所有 PDF 和 JPG 文件移动到文件夹 "~/Desktop/media/"。文件名不变。

```yaml
rules:
  - locations: ~/Desktop
    filters:
      - extension:
          - pdf
          - jpg
    actions:
      - move: "~/Desktop/media/"
```

使用占位符将所有 .pdf 文件移动到 "PDF" 文件夹，将所有 .jpg 文件移动到 "JPG" 文件夹。现有文件将被覆盖。

```yaml
rules:
  - locations: ~/Desktop
    filters:
      - extension:
          - pdf
          - jpg
    actions:
      - move:
          dest: "~/Desktop/{extension.upper()}/"
          on_conflict: "overwrite"
```

将 PDF 文件移动到文件夹 `Invoices`。保留文件名但不覆盖现有文件。为了防止覆盖文件，会在文件名中添加索引，因此 `somefile.jpg` 变为 `somefile 2.jpg`。

```yaml
rules:
  - locations: ~/Desktop/Invoices
    filters:
      - extension:
          - pdf
    actions:
      - move:
          dest: "~/Documents/Invoices/"
          on_conflict: "rename_new"
          rename_template: "{name} {counter}{extension}"
```

## Python

::: organize.actions.Python

**示例：**

一个基本示例，展示如何获取当前文件路径并在 for 循环中进行一些打印。`|` 是 YAML 语法，用于定义跨越多行的字符串字面量。

```yaml
rules:
  - locations: "~/Desktop"
    actions:
      - python: |
          print('当前文件的路径是 %s' % path)
          for _ in range(5):
              print('嘿嘿，是我从循环中来的')
```

```yaml
rules:
  - name: "您可以访问过滤器数据"
    locations: ~/Desktop
    filters:
      - regex: '^(?P<name>.*)\.(?P<extension>.*)$'
    actions:
      - python: |
          print('名称: %s' % regex["name"])
          print('扩展名: %s' % regex["extension"])
```

在模拟中运行和 [YAML 别名](rules.md#advanced-aliases)：

```yaml
my_python_script: &script |
  print("Hello World!")
  print(path)

rules:
  - name: "在模拟中运行和 YAML 别名"
    locations:
      - ~/Desktop/
    actions:
      - python:
          code: *script
          run_in_simulation: yes
```

您可以访问所有 Python 魔法——为每个以下划线开头的文件名进行谷歌搜索：

```yaml
rules:
  - locations: ~/Desktop
    filters:
      - name:
          startswith: "_"
    actions:
      - python: |
          import webbrowser
          webbrowser.open('https://www.google.com/search?q=%s' % name)
```

## 重命名

::: organize.actions.Rename

**示例：**

```yaml
rules:
  - name: "将所有 .PDF 文件扩展名转换为小写 (.pdf)"
    locations: "~/Desktop"
    filters:
      - name
      - extension: PDF
    actions:
      - rename: "{name}.pdf"
```

```yaml
rules:
  - name: "将**所有**文件扩展名转换为小写"
    locations: "~/Desktop"
    filters:
      - name
      - extension
    actions:
      - rename: "{name}.{extension.lower()}"
```

## Shell

::: organize.actions.Shell

**示例：**

```yaml
rules:
  - name: "在 macOS 上：打开桌面上的所有 PDF"
    locations: "~/Desktop"
    filters:
      - extension: pdf
    actions:
      - shell: 'open "{path}"'
```

## 符号链接

::: organize.actions.Symlink

## 回收站

::: organize.actions.Trash

**示例：**

```yaml
rules:
  - name: 将桌面上一年以上的 JPG 和 PNG 文件移动到回收站
    locations: "~/Desktop"
    filters:
      - lastmodified:
          years: 1
          mode: older
      - extension:
          - png
          - jpg
    actions:
      - trash
```

## 写入

::: organize.actions.Write

**示例**

```yaml
rules:
  - name: "记录文件大小"
    locations: ~/Downloads
    filters:
      - size
    actions:
      - write:
          outfile: "./sizes.txt"
          text: "{size.traditional} -- {relative_path}"
          mode: "append"
          clear_before_first_write: true
```

这将在当前工作文件夹中创建一个文件 `sizes.txt`，其中包含 `~/Downloads` 文件夹中所有文件的大小：

```
2.9 MB -- SIM7600.pdf
1.0 MB -- Bildschirmfoto 2022-07-05 um 10.43.16.png
5.9 MB -- Albumcover.png
51.2 KB -- Urlaubsantrag 2022-04-19.pdf
1.8 MB -- ETH_USB_HUB_HAT.pdf
2.1 MB -- ArduinoDUE_V02g_sch.pdf
...
```

您可以在文本和文本文件参数中使用模板：

```yaml
rules:
  - name: "按扩展名记录文件大小"
    locations: ~/Downloads
    filters:
      - size
      - extension
    actions:
      - write:
          outfile: "./sizes.{extension}.txt"
          text: "{size.traditional} -- {relative_path}"
          mode: "prepend"
          clear_before_first_write: true
```

这将按扩展名分离文件大小。
