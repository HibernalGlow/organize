# 配置

## 编辑配置

organize 如果没有给出其他文件，则有默认配置文件。

要编辑默认配置文件：

```sh
organize edit  # 在 $EDITOR 中打开
organize edit --editor=vim
EDITOR=code organize edit
```

要打开包含配置文件的文件夹：

```sh
organize show
organize show --path  # 显示默认配置的完整路径
```

要检查您的配置，请运行：

```sh
organize check
organize check --debug  # 使用调试输出检查
```

## 运行和模拟

要运行/模拟默认配置文件：

```sh
organize sim
organize run
```

要运行/模拟特定配置文件：

```shell
organize sim [FILE]
organize run [FILE]
```

可选地，您可以像这样指定工作目录：

```shell
organize sim [FILE] --working-dir=~/Documents
```

## 运行配置中的特定规则

您可以像这样标记您的规则：

```yml
rules:
  - name: 我的第一个规则
    actions:
      - echo: "Hello world"
    tags:
      - debug
      - fast
```

然后使用命令行选项 `--tags` 和 `--skip-tags` 来选择要运行的规则。这些选项接受逗号分隔的标签列表：

```
organize sim --tags=debug,foo --skip-tags=slow
```

特殊标签：

- 标记为特殊标签 `always` 的规则将始终运行（除非指定 `--skip-tags=always`）
- 标记为特殊标签 `never` 的规则将永远不会运行（除非指定 `--tags=never`）

## 环境变量

- `ORGANIZE_CONFIG` - 默认配置文件的路径。
- `ORGANIZE_EXIFTOOL_PATH` - `exiftool` 可执行文件的路径（默认：`""`）
- `ORGANIZE_NORMALIZE_UNICODE` - 是否将字符串标准化为 NFC unicode 形式以进行比较（默认 `"1"`）
- `NO_COLOR` - 如果设置，输出将不带颜色。
- `EDITOR` - 用于编辑配置文件的编辑器。

## 并行化作业

为了加速组织，您可以同时运行多个 organize 进程，如下所示（Linux / macOS）：

```shell
organize run config_1.yaml & \
organize run config_2.yaml & \
organize run config_3.yaml &
```

确保配置文件彼此独立，这意味着没有规则依赖于另一个配置文件中的另一个规则。
