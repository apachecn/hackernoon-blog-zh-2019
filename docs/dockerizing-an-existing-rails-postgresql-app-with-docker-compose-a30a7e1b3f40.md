# 使用 Docker compose 对现有的 Rails-Postgresql 应用程序进行 Dockerizing

> 原文：<https://medium.com/hackernoon/dockerizing-an-existing-rails-postgresql-app-with-docker-compose-a30a7e1b3f40>

![](img/7dcfc847bbb0163c65722be765423555.png)

使用 Docker compose 对现有 Rails-Postgresql 应用程序进行 Docker 化的快速入门

**Dockerfiles —定义项目**

在 app 的根目录下创建 *Dockerfile* ，通过向 *Dockerfile 添加依赖项来定义项目。*

```
FROM ruby:2.5.3RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejsRUN mkdir /myappWORKDIR /myappCOPY Gemfile /myapp/GemfileCOPY Gemfile.lock /myapp/Gemfile.lockRUN bundle installCOPY . /myapp
```

确保添加您的应用程序使用的特定版本的 ruby

**Docker-compose —构建项目**

现在，在根目录下创建 *docker-compose.yml* 文件。这个文件可以神奇地从当前目录构建您的应用程序，并通过 web 应用程序运行预构建的 postgresql 映像

```
version: '3'
services:
 db:
  image: postgres
  volumes:
   - ./tmp/db:/var/lib/postgresql/data
 web:
  build: .
  command: bundle exec rails s -p 3000 -b '0.0.0.0'
  volumes:
   - .:/myapp
  ports:
   - "3000:3000"
  depends_on:
   - db
```

**Database.yml —配置数据库**

现在，在您的 *database.yml* 文件中将主机改为‘db ’,以指向 docker 服务

```
default: &default adapter: postgresql encoding: unicode host: db username: postgres password: pool: 5development: <<: *default
 database: myapp_development
```

通过运行相应的命令，可以完成以下任务

**构建 Docker 映像**

```
docker-compose build
```

**数据库—创建和迁移**

```
docker-compose run web rake db:create db:migrate
```

**访问 Rails 控制台**

```
docker-compose exec web rails console
```

**开始申请**

```
docker-compose up
```

现在您应该能够浏览到 [http://localhost:3000](http://localhost:3000/) 并看到您的应用程序正在运行。