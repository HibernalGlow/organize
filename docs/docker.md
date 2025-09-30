# 使用 organize Docker 镜像

organize Docker 镜像预装了 `exiftool` 和 `pdftotext`，以及所有设置好的 Python 依赖。

!!! danger

    由于 organize 主要用于移动文件，您必须小心您的卷挂载和路径。**如果您将文件移动到未持久化的文件夹，一旦容器停止，它就消失了！**

## 构建镜像

`cd` 进入包含 `Dockerfile` 的 organize 文件夹并构建镜像：

```sh
docker build -t organize .
```

镜像现在标记为 `organize`。现在您可以通过运行来测试镜像

```sh
docker run organize
```

这将显示 organize 使用帮助文本。

## 运行

让我们创建一个基本的配置文件 `docker-conf.yml`：

```yml
rules:
  - locations: /data
    actions:
      - echo: "Found file: {path}"
```

我们现在可以将配置文件挂载到容器路径 `/config/config.yml`。当前目录挂载到 `/data`，所以我们有一些文件存在。
我们现在可以启动容器：

```sh
docker run -v ./docker-conf.yml:/config/config.yml -v .:/data organize run
```

### 从 stdin 传递配置文件

您也可以从 stdin 传递配置文件，而不是将其挂载到容器中：

```sh
docker run -i organize check --stdin < ./docker-conf.yml
```
