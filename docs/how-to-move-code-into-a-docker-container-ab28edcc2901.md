# 如何将代码移动到 Docker 容器中

> 原文：<https://medium.com/hackernoon/how-to-move-code-into-a-docker-container-ab28edcc2901>

[Docker](https://www.docker.com/) 提供了两个很好的选项来将你的代码移动到一个映像或者容器中:[绑定挂载](https://docs.docker.com/storage/bind-mounts/)和 [Dockerfile](https://docs.docker.com/engine/reference/builder/#copy) `[COPY](https://docs.docker.com/engine/reference/builder/#copy)` [指令](https://docs.docker.com/engine/reference/builder/#copy)。在这篇文章中，我将解释为什么在生产中图像应该总是使用`COPY`指令，以及为什么在开发中使用绑定挂载可能更方便。

# Dockerfile `COPY`指令

Dockerfile 中的`COPY`指令用于将文件或目录从主机文件系统复制到映像中。例如，下面的 Dockerfile 设置了一个在生产模式下运行的 NodeJS 应用程序。

```
# Dockerfile
FROM node:carbonWORKDIR /app
ENV NODE_ENV=production# Install dependencies first to take advantage of Docker layer caching. 
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile --no-cache --production# Copy the application files into the image. 
COPY . .EXPOSE 3000
CMD [ "node", "app.js" ]
```

构建并运行:

```
$ docker build -t myapp .
$ docker run -d -p 3000:3000 myapp
```

`COPY`指令递归地将文件和目录从主机复制到映像中，这意味着敏感文件也可能被复制进来。与 Git 的`.gitignore`类似，Docker 的`[.dockerignore](https://docs.docker.com/engine/reference/builder/#dockerignore-file)` [文件](https://docs.docker.com/engine/reference/builder/#dockerignore-file)允许你阻止某些文件被复制到镜像中。使用 `**COPY**` **指令**时，您应该**始终包含一个** `**.dockerignore**` **文件。至少，它应该包括与您的版本控制系统和本地安装的依赖项相关的文件。例如，一个典型的 NodeJS 应用程序可能使用以下代码。**

```
# .dockerignore
.git
Dockerfile
.dockerignore
node_modules
npm-debug.log
```

# 绑定安装

绑定挂载允许您将文件或目录从主机挂载到容器中。在下面的代码片段中，工作目录被挂载并可以从容器中的`/app`访问。一旦容器开始运行，您就可以通过 shell 与它进行交互。

```
$ docker run --rm -it -p 3000:3000 -v $(pwd):/app myapp bash
root@id:/app# yarn global add nodemon
root@id:/app# nodemon app.js
```

`nodemon`是一个工具，用于监视文件更改，并在检测到更改时自动重启应用程序。在上面的例子中，当您更改主机上的文件时，这些更改通过绑定挂载在容器中自动可见，并且您的应用程序将在`nodemon`前自动重启。

# 在`COPY`指令和绑定支架之间选择

什么时候应该使用`COPY`指令，什么时候适合使用绑定挂载？

绑定坐骑非常适合本地开发。它们提供了便利，因为对宿主开发环境和代码的更改会立即反映在容器中，并且应用程序在容器中创建的文件(如构建工件或日志文件)可以从宿主获得。

然而，绑定挂载确实带来了一些安全问题。来自 [Docker 存储文档](https://docs.docker.com/storage/):

> *使用绑定挂载的一个副作用，不管是好是坏，是您可以通过在容器中运行的进程来更改主机文件系统，包括创建、修改或删除重要的系统文件或目录。这是一项强大的功能，可能会带来安全隐患，包括影响主机系统上的非 Docker 进程。*
> 
> *[…]*
> 
> *如果您以这种方式使用 Docker 进行开发，您的生产 Docker 文件会将生产就绪的工件直接复制到映像中，而不是依赖于绑定挂载。*

绑定挂载将主机系统暴露给容器，并降低了容器的安全性和隔离性。即使是一个只读绑定挂载[也可以暴露那些被`.dockerignore`从映像中排除的文件。为了在生产中避免这些问题**，应该使用** `**COPY**` **命令将代码**传输到映像中，而不是绑定挂载。](https://docs.docker.com/storage/bind-mounts/#use-a-read-only-bind-mount)

# 进一步阅读

Docker 文档提供了其他挂载类型的详细信息，并给出了它们可能的用例的更多细节。特别是，在某些用例中，对配置文件使用绑定挂载可能是有利的。

而且真的，[不要忽略](https://codefresh.io/docker-tutorial/not-ignore-dockerignore/) `[.dockerignore](https://codefresh.io/docker-tutorial/not-ignore-dockerignore/)`。