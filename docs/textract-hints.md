# Textract 安装提示

Textract 需要 [Poppler](https://poppler.freedesktop.org/) 来从 PDF 中提取文本。

## Windows

1. 从 [github.com/oschwartz10612](https://github.com/oschwartz10612/poppler-windows/releases) 下载您选择的最新二进制文件。在此示例中，我们将下载并使用 [Release-22.01.0-0.zip](https://github.com/oschwartz10612/poppler-windows/releases/download/v22.01.0-0/Release-22.01.0-0.zip)。
2. 提取归档文件 _Release-22.01.0-0.zip_
3. 将 _poppler-22.01.0\Library_ 中的文件夹复制到 `C:\Program Files\Poppler`。
4. 因此，目录结构应如下所示：

```
C:\Program Files\Poppler
                        \bin
                        \include
                        \lib
                        \share
```

5. 将 `C:\Program Files\Poppler\bin` 添加到您的系统 PATH！
6. 使用 [filecontent 示例规则](https://organize.readthedocs.io/en/latest/filters/#filecontent) 尝试它
