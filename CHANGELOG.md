# 变更日志

## [未发布]

- 修复了 #438（当 `pdftotext` 未安装时，`filecontent` 过滤器对 PDF 失败，而不是回退到 `pdfminer`）

## v3.3.0 (2024-11-25)

- 添加了一个新的冲突模式 `deduplicate`，它跳过重复文件并重命名非重复文件（感谢 @TheExistingOne）。
- `exif` 过滤器现在支持从非图像文件（如 EPUB 或 PDF 文件）中提取元数据。
- 放宽 pdfminer-six 依赖版本约束，以简化 NixOS 打包。
- 修复 Windows 上的编码问题（感谢 @Alimektor）。

## v3.2.5 (2024-07-09)

- 修复了一个 bug，其中某些位置选项不接受 yaml 别名（#390，感谢 @zany130 报告）。

## v3.2.4 (2024-07-07)

- 修复了一个阻止 organize 启动的 bug（感谢 @feather42）。
- 修复了一个忽略失败 shell 命令不起作用的 bug（感谢 @florianklumb）。

## v3.2.3 (2024-03-28)

- 改进了查找和创建配置文件逻辑。
- 修复了一个 XDG_CONFIG_HOME 中的配置文件未找到的 bug (#371)。
- 修复了移动文件后 `relative_path` 参数在操作中崩溃的 bug (#372)。

## v3.2.2 (2024-03-04)

- 修复了 `organize new` 命令无法创建新配置文件的问题。

## v3.2.1 (2024-02-23)

- 文件和文件夹现在按自然排序顺序处理 (#354)。

## v3.2.0 (2024-02-19)

- 将 `.docx`、`.pdf` 和各种原始文本解析器集成到 `filecontent` 过滤器中。
- 移除了 `textract` 和约 50 MB 的依赖，因为不再需要它们。
- 完全支持 Python 3.12
- 添加支持从 STDIN 管道输入配置文件（`organize run --stdin < file.yml`）

**重要：**

您可能需要调整您的 `filecontent` 正则表达式。现在输出应该更干净一些。

## v3.1.2 (2024-02-16)

- 修复了一个验证错误，其中正确定义的操作在 Python 3.12.2 中不被接受。

## v3.1.1 (2024-02-11)

- 修复了在 v3.1.0 中损坏的 `organize show` 命令。

## v3.1.0 (2024-02-04)

- 添加了一个新的输出格式 `errorsonly`，仅在发生错误时显示输出。
- 修复了一个 bug，其中包含括号的消息和路径未正确打印（#348，感谢 @kwbr！）

## v3.0.1 (2024-01-12)

- 修复了一个 Quicktime 和 mp4 文件返回无 Exif 数据的 bug。（#313，@jleatham 感谢调试！）
- 修复了一个 `exlude_dirs` 未正确排除的 bug。（#339）

## v3.0.0 (2024-01-05)

- 通过设置 `ORGANIZE_EXIFTOOL_PATH` 环境变量，在 `exif` 过滤器中支持 `exiftool`。（感谢 @HernandoR 为 qtff 支持工作）
- 修复了 `min_depth` 位置参数。（感谢 @danielklim 为此工作）

## v3.0.0a2 (2024-01-02)

- 修复首次运行 `organize new` 时的 bug。如果不存在，则创建 organize 配置目录。（感谢 @white-gecko）
- 添加操作 `hardlink`。

## v3.0.0a1 (2024-01-01)

- 对于不使用 Python UTF-8 模式的 Windows 用户，默认使用 UTF-8 编码读取和写入配置文件（环境变量 `PYTHONUTF8=1`）。
- `write` 操作：允许设置文本编码。

## v3.0.0a0 (2023-12-31)

- 新操作：`write` 写入行到文件。
- 新过滤器：`date_lastused`（仅 macOS）。
- 您现在可以在所有基于时间的过滤器中指定时区。
- 移除了隐藏的（已弃用）CLI 选项 `--config-file`。
- 大量新测试和一些 bug 修复。
- `exif` 过滤器现在支持 simplematch 语法。
- 占位符 `{now}` 现在必须是 `{now()}`。
- 每个位置现在支持多个 `path`。
- 位置现在支持 `min_depth` 选项
- 重复过滤器：`detect_original_by` 现在支持 `last_seen`。
- 新的命令行界面（添加了 `new`、`show` 和 `list` 命令）。
- `JSONL` 输出（`organize run --format=JSONL`）
- `move` 和 `copy` 现在智能自动检测您是否要移动到文件夹（此自动检测可以停用）。
- `copy` 操作：您现在可以指定是继续使用原文件还是副本。
- 完全移除了 `pyfilesystem` 依赖。
- 至少 4 倍速度提升。通常超过 10 倍。

## v2.4.3 (2023-10-14)

- 修改过滤器：`exif`。在 exif 数据上启用日期时间字段（问题 #266）（感谢 @FlorianFritz）
- 支持华为和 Honer 手机的 Exif 数据（感谢 @HernandoR）

## v2.4.2 (2023-08-25)

- 修复读取 HEIC 图像的 exif 数据（问题 #267）

## v2.4.1 (2023-08-25)

- 修复日志中的 unicode bug（问题 #294）（感谢 @xdhmoore）
- 更新依赖（感谢 @gaby）
- 移除了对 Python 3.7 的支持（EOL - 2023 年 6 月）（感谢 @gaby）

## v2.4.0 (2022-09-05)

- 新操作：`write`。
- 新过滤器：`date_lastused`（仅 macOS）。
- 冲突解决重命名现在从 2 开始而不是 1。
- 添加支持 FS url 作为配置文件路径和工作目录（CLI 和 ORGANIZE_CONFIG 环境变量中）。
- 移除了隐藏的（已弃用）CLI 选项 `--config-file`。
- 大量新测试和一些 bug 修复。

## v2.3.0 (2022-07-26)

- 新过滤器：`macos_tags`（仅 macOS）。
- 忽略损坏的符号链接（问题 #202）

## v2.2.0 (2022-03-31)

- 标签支持 (#199) 以在配置中运行规则子集。

## v2.1.2 (2022-02-13)

- `filecontent` 过滤器的热修复。

## v2.1.1 (2022-02-13)

- `filecontent` 过滤器：修复 bug #188。
- 修复 #185 和 #184。

## v2.1.0 (2022-02-11)

- 添加过滤器 `date_added`（仅 macOS）
- `created` 过滤器现在支持 gnu coreutils stat 工具用于出生时间检测
- 将基于时间的过滤器重构为通用类

## v2.0.9 (2022-02-10)

- `shell` 在模拟中不运行代码时显示消息
- `shell` 添加选项 `simulation_output` 和 `simulation_returncode`
- 修复位置选项也应用于其他位置的 bug
- `created` 过滤器现在在 Linux 系统上回退到使用 stat 工具，其中出生时间不包括在 `os.stat` 中。

## v2.0.8 (2022-02-09)

- 真正修复 `shell`。

## v2.0.7 (2022-02-09)

- 修复 `shell`。

## v2.0.6 (2022-02-09)

- 加速移动文件。
- `shell` 操作：通过用户的 shell 运行命令。

## v2.0.5 (2022-02-08)

- 修复迁移消息和文档 URL

## v2.0.4 (2022-02-08)

- exclude_dir, system_exclude_dirs, exclude_files, system_exclude_files, filter 和 filter_dirs 现在接受单个字符串。
- 修复名称过滤器中的 bug

## v2.0.3 (2022-02-07)

- 修复拼写错误：`system_exlude_files`

## v2.0.2 (2022-02-07)

- 修复位置中环境变量扩展的 bug

## v2.0.1 (2022-02-07)

- `macos_tags` 操作的小修复。
- 迁移检测中的 bug 修复。

## v2.0.0 (2022-02-07)

这是一个巨大的更新，包含大量改进。
请在运行前备份所有重要内容并使用模拟选项！

[**迁移指南**](docs/updating-from-v1.md)

### 新功能

- 您现在可以使用规则[目标目录](docs/rules.md#targeting-directories)（复制、重命名等整个文件夹）
- [在 (S)FTP、S3 存储桶、Zip 存档等之间组织](docs/locations.md#remote-filesystems-and-archives)（[可用文件系统列表](https://www.pyfilesystem.org/page/index-of-filesystems/)）。
- 递归进入子文件夹时的 [`max_depth`](docs/locations.md#location-options) 设置
- 尊重您的规则顺序 - 更安全，更少魔法，更少惊喜。
  - （organize v1 试图聪明。v2 现在从上到下工作您的配置文件）
- [Jinja2 模板引擎用于占位符](docs/rules.md#templates-and-placeholders)。
- 即时启动。（不需要在开始前收集所有文件）
- [过滤器现在可以被排除](docs/filters.md#how-to-exclude-filters)。
- [过滤器模式](docs/rules.md#rule-options)：`all`、`any` 和 `none`。
- [规则名称](docs/rules.md#rule-options)。
- [`move`](docs/actions.md#move)、[`copy`](docs/actions.md#copy) 和 [`rename`](docs/actions.md#rename) 操作中的新冲突解决设置：
  - 选项是 `skip`、`overwrite`、`trash`、`rename_new` 或 `rename_existing`
  - 您现在可以定义自定义 `rename_template`。
- [`duplicate`](docs/filters.md#duplicate) 现在支持几种选项来区分原始文件和重复文件。
- [`python`](docs/actions.md#python) 操作现在可以在模拟中运行。
- [`shell`](docs/actions.md#shell) 操作现在返回 stdout 和错误代码。
- 添加过滤器 [`empty`](docs/filters.md#empty) - 查找空文件和文件夹
- 添加过滤器 [`hash`](docs/filters.md#hash) - 生成文件哈希
- 添加操作 [`symlink`](docs/actions.md#symlink) - 生成符号链接
- 添加操作 [`confirm`](docs/actions.md#confirm) - 请求确认
- 许多小修复和改进！

### 更改

- [`lastmodified`](docs/filters.md#lastmodified) 和 [`created`](docs/filters.md#created) 的 `timezone` 关键字已被移除。现在默认时区是本地时区。
- `filesize` 过滤器已重命名为 [`size`](docs/filters.md#size)，现在也可以用于获取目录大小。
- `filename` 过滤器已重命名为 [`name`](docs/filters.md#name)，现在也可以用于获取目录名称。
- [`size`](docs/filters.md#size) 过滤器现在返回多种格式

### 移除

- 文件夹中的 Glob 语法已消失（[不再需要](docs/locations.md)）
- `"!"` 文件夹排除语法已消失（[不再需要](docs/locations.md)）

## v1.10.1 (2021-04-21)

- `macos_tags` 操作现在支持颜色和占位符。
- 如果文件夹未找到，则显示完整扩展路径。

## v1.10.0 (2021-04-20)

- 添加过滤器 `mimetype`
- 添加操作 `macos_tags`
- 在 `lename` 过滤器中支持 [`simplematch`](https://github.com/tfeldmann/simplematch) 语法。
- 更新依赖
- 因为在某些平台上安装 `textract` 相当困难，它现在是一个可选依赖。使用 `pip install organize-tool[textract]` 安装它
- 此版本需要 Python 3.6 最低。有些只是回退的依赖（thlib2, typing）已被移除。
- 在 created 和 last_modified 过滤器中添加时区（感谢 @win0err！）

## v1.9.1 (2020-11-10)

- 添加 {env} 变量
- 添加 {now} 变量

## v1.9 (2020-06-12)

- 添加过滤器 `Duplicate`。

## v1.8.2 (2020-04-03)

- 修复文件名过滤器配置解析算法中仅数字文件名的 bug。

## v1.8.1 (2020-03-28)

- 扁平化过滤器和操作列表以允许增强配置文件配置（感谢 @rawdamedia！）
- 添加对多行内容过滤器的支持（感谢 @zor-el！）

## v1.8.0 (2020-03-04)

- 添加操作 `Delete`。
- 添加过滤器 `FileContent`。
- Python 3.4 正式弃用，不再支持。
- `--config-file` 命令行选项现在支持 `~` 用于用户文件夹和扩展环境变量
- 在过滤器 `created` 和 `stmodified` 中添加 `years`、`months`、`weeks` 和 `seconds` 参数

## v1.7.0 (2019-11-26)

- 添加过滤器 `Exif` 以按图像 exif 数据过滤。
- 占位符变量属性现在不区分大小写。

## v1.6.2 (2019-11-22)

- 修复 `Rename` 操作（`'PosixPath' object has no attribute 'items'`）。
- 在所有地方使用类型提示。

## v1.6.1 (2019-10-25)

- 对于缺失文件夹显示警告而不是引发异常。

## v1.6 (2019-08-19)

- 添加过滤器：`Python`
- 添加过滤器：`FileSize`
- organize 模块现在可以直接运行：`python3 -m organize`
- 各种代码简化和服务提升。
- 修复 globstring 文件排除的问题。
- 移除 `clint` 依赖，因为它不再维护。
- 添加各种集成测试
- "~~ SIMULATION ~~" 横幅现在占据整个终端宽度

## v1.5.3 (2019-08-01)

- 文件名过滤器现在支持列表。

## v1.5.2 (2019-07-29)

- 文件夹路径中的环境变量现在扩展（语法 `$name` 或 `${name}`，Windows 上额外 `%name%`）。
  例如，这允许在 Windows 中使用 `%public/Desktop%`。

## v1.5.1 (2019-07-23)

- 新过滤器 "Created" 以按创建日期过滤。
- 修复问题 #39，其中 globstrings 大多数时候不起作用。
- 问题 #39 的集成测试
- 支持缩进配置文件

## v1.5 (2019-07-17)

- 修复问题 #31，其中 {path} 变量总是解析为源路径
- 更新依赖
- 从发布的 wheel 中排除变更日志和自述文件

## v1.4.5 (2019-07-03)

- 过滤器和操作名称现在不区分大小写

## v1.4.4 (2019-07-02)

- 修复 Windows 上配置文件中 umlauts 的问题 #36

## v1.4.3 (2019-06-05)

- 使用安全 YAML 加载器修复弃用警告。（感谢 mope1！）
- 如果文件夹不存在，则更好的错误消息。（再次感谢 mope1！）
- 修复 LastModified 过滤器文档中的示例代码。
- 自定义配置文件位置（由 cmd 行参数或环境变量给出）。
- `config --debug` 现在显示配置文件的完整路径。

## v1.4.2 (2018-11-14)

- 修复 `$EDITOR` 环境变量中命令行参数的 bug。
- 修复空配置不会显示正确错误消息的 bug。
- 通过使用环境标记修复 setup.py 中的二进制 wheel 创建

## v1.4.1 (2018-10-05)

- 现在可以在操作 Move、Cy 和 Rename 中设置自定义分隔符 `counter_separator`。

## v1.4 (2018-09-21)

- 修复 glob 通配符未正确检测的 bug
- 添加通过 glob 语法排除文件夹和文件的支持。
- 确保文件每个规则只处理一次。

## v1.3 (2018-07-06)

- 文件夹配置中的 Glob 支持。
- 新变量 {relative_path} 现在在操作中可用。

## v1.2 (2018-03-19)

- 在子文件夹中显示文件的相对路径。

## v1.1 (2018-03-13)

- 从扩展过滤器输出中移除冒号，因此 `{extension.lower}` 现在返回 `'png'` 而不是 `'.png'`。

## v1.0 (2018-03-13)

- 初始发布。
