<p align="center">
  <!--<img width="623" height="168" src="https://github.com/tfeldmann/organize/raw/gh-pages/img/organize.svg?sanitize=true" alt="organize logo">-->
  <a href="https://tfeldmann.github.io/organize/changelog/" target="_blank"><img width="100%" src="https://github.com/tfeldmann/organize/blob/main/docs/img/organize-v3.jpg?raw=true" alt="organize v3 is out"></a>
</p>

<div align="center">

<a href="https://github.com/tfeldmann/organize/actions/workflows/tests.yml"><img src="https://github.com/tfeldmann/organize/actions/workflows/tests.yml/badge.svg" title="tests"></a>
<a href="https://organize.readthedocs.io/en/latest/?badge=latest"><img src="https://readthedocs.org/projects/organize/badge/?version=latest" title="Documentation Status"></a>
<a href="https://results.pre-commit.ci/latest/github/tfeldmann/organize/main"><img src="https://results.pre-commit.ci/badge/github/tfeldmann/organize/main.svg" title="pre-commit.ci status"></a>
<a href="https://github.com/tfeldmann/organize/blob/main/LICENSE.txt"><img src="https://img.shields.io/badge/license-MIT-blue.svg" title="License"></a>
<a href="https://pypi.org/project/organize-tool/"><img src="https://img.shields.io/pypi/v/organize-tool" title="PyPI Version"></a>

</div>

---

<p align="center"> <b>organize</b> - 文件管理自动化工具
<br>
<a href="https://organize.readthedocs.io/" target="_blank">完整文档位于 Read the docs</a>
</p>


## v3 现已可用

新版本应该*快得多*并修复了许多 bug。它还带有一些新的操作、过滤器和选项。

如果您在迁移过程中遇到任何其他 bug 或问题，请联系我们！

- [查看变更日志](https://tfeldmann.github.io/organize/changelog/)
- [迁移指南](https://tfeldmann.github.io/organize/migrating/#migrating-from-v2-to-v3)

## 关于

您的桌面一团糟？您在下载和文档中找不到任何东西？手动排序和重命名所有这些文件太乏味了？是时候自动化一次并永远受益。

**organize** 是命令行、开源替代品，如 Hazel (macOS) 或 File Juggler (Windows)。

### 人们用它来：

- 根据 EXIF 数据将图片排序和标记到各种文件夹结构中
- 根据文件内容排序和重命名 PDF 发票
- 从 ~/Downloads 中移除不完整的下载
- 从 ~/Desktop 中清理未使用的文件
- 通过移除重复文件释放磁盘空间
- 自动化各种业务流程
- 等等

## 功能

一些亮点包括：

- 安全移动、重命名、复制文件和文件夹，具有冲突解决选项。
- 快速重复文件检测。
- Exif 标签提取。
- 通过从 PDF、DOCX 等中提取的文本进行分类。
- 强大的模板引擎。
- 内联 Python 和 shell 命令作为过滤器和操作，以实现最大灵活性。
- 在接触您的文件之前可以模拟一切。
- 在 macOS、Windows 和 Linux 上工作。
- 免费和开源软件。

## 开始使用

### 安装

只需要 Python 3.9+。
通过您的包管理器或从 [python.org](https://python.org) 安装。

通过 pip 安装。请注意包名称是 `organize-tool`：

```bash
pip install -U organize-tool
```

此命令也可用于更新到最新版本。现在您可以运行 `organize --help` 来检查安装是否成功。

### 创建您的第一个规则

在您的 shell 中，运行 `organize new` 然后 `organize edit` 来编辑配置：

```yaml
rules:
  - name: "查找 PDF"
    locations:
      - ~/Downloads
    subfolders: true
    filters:
      - extension: pdf
    actions:
      - echo: "找到 PDF！"
```

> 如果您在编辑配置时遇到问题，您可以运行 `organize show --reveal` 来在文件管理器中显示配置文件夹。然后您可以在您喜欢的编辑器中编辑 `config.yaml`。

保存您的配置文件并运行：

```sh
organize run
```

您将看到下载文件夹（+ 子文件夹）中所有 `.pdf` 文件的列表。
现在我们只为每个文件显示文本 `找到 PDF！`，但这很快就会改变...
（如果显示 `Nothing to do`，您只是下载文件夹中没有任何 pdf）。

再次运行 `organize edit` 并为您的规则添加 `move` 操作：

```yml
actions:
  - echo: "找到 PDF！"
  - move: ~/Documents/PDFs/
```

现在运行 `organize sim` 来查看会发生什么而不接触您的文件。

您将看到您的 pdf 文件将被移动到您的 `Documents/PDFs` 文件夹。

恭喜，您刚刚自动化了您的第一个任务。您现在可以随时运行 `organize run`，所有 pdf 都更有条理了。就是这么简单。

> 还有更多。您想重命名/复制文件，运行自定义 shell 或 python 脚本，使用正则表达式匹配名称或使用占位符变量？organize 涵盖了您。看看下面的高级用法示例！

## 示例规则

以下是一些简单组织和清理规则的示例。根据您的需要修改！

将所有发票、订单或采购文档移动到您的文档文件夹：

```yaml
rules:
  - name: "排序我的发票和收据"
    locations: ~/Downloads
    subfolders: true
    filters:
      - extension: pdf
      - name:
          contains:
            - Invoice
            - Order
            - Purchase
          case_sensitive: false
    actions:
      - move: ~/Documents/Shopping/
```

递归删除所有空目录：

```yaml
rules:
  - name: "递归删除所有空目录"
    locations:
      - path: ~/Downloads
    targets: dirs
    subfolders: true
    filters:
      - empty
    actions:
      - delete
```

<!--<details markdown="1">
  <summary markdown="1">高级示例</summary>

此示例显示了一些高级功能，如占位符变量、可插拔操作、通过子文件夹和文件系统的有限递归（FTP 和 ZIP）：

此规则：

- 在您的文档文件夹（三层深）和 FTP 服务器上递归搜索
- 查找具有 **pdf** 或 **docx** 扩展名的文件
- 具有创建时间戳
- 为每个文件请求用户确认
- 根据它们的扩展名和 **created** 时间戳移动它们：
- `script.docx` 将被移动到 `~/Documents/DOCX/2018-01/script.docx`
- `demo.pdf` 将被移动到 `~/Documents/PDF/2016-12/demo.pdf`
- 如果此新名称已被占用，则在文件名中附加计数器（"rename_new"）
- 在您的桌面上创建一个包含所有文件的 zip 备份文件。

```yaml
rules:
  - name: "下载、清理和备份"
    locations:
      - path: ~/Documents
        max_depth: 3
      - path: ftps://demo:demo@demo.wftpserver.com
    filters:
      - extension:
          - pdf
          - docx
      - created
    actions:
      - confirm:
          msg: "真的继续吗？"
          default: true
      - move:
          dest: "~/Documents/{extension.upper()}/{created.strftime('%Y-%m')}/"
          on_conflict: rename_new
      - copy: "zip:///Users/thomas/Desktop/backup.zip"
```

</details>-->

您可以在 <a href="https://tfeldmann.github.io/organize" target="_blank">完整文档</a> 中找到更多示例。

## 命令行界面

```txt
organize - 文件管理自动化工具。

用法:
  organize run   [options] [<config>]
  organize sim   [options] [<config>]
  organize new   [<config>]
  organize edit  [<config>]
  organize check [<config>]
  organize debug [<config>]
  organize show  [--path|--reveal] [<config>]
  organize list
  organize docs
  organize --version
  organize --help

命令:
  run        组织您的文件。
  sim        模拟组织您的文件。
  new        创建新配置。
  edit       使用 $EDITOR 编辑配置文件。
  check      检查配置文件是否有效。
  debug      显示原始配置解析步骤。
  show       将配置打印到 stdout。
               使用 --reveal 在文件管理器中显示文件
               使用 --path 显示文件路径
  list       列出在默认位置找到的配置文件。
  docs       打开文档。

选项:
  <config>                        配置名称或配置文件路径
  -W --working-dir <dir>          工作目录
  -F --format (default|jsonl)     输出格式 [默认: default]
  -T --tags <tags>                要运行的标签（例如 "initial,release"）
  -S --skip-tags <tags>           要跳过的标签
  -h --help                       显示此帮助页面。
```

## 其他捐赠选项：

ETH:

```
0x8924a060CD533699E230C5694EC95b26BC4168E7
```

BTC:

```
39vpniiZk8qqGB2xEqcDjtWxngFCCdWGjY
```
