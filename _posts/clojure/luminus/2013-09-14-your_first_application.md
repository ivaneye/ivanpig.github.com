---
layout: post
title: Luminus手册-第一个应用
categories: luminus
tags: [clojure,luminus]
avatarimg: "/img/head.jpg"

---

# Guestbook应用

此文使用Luminus构建一个简单的guestbook应用。guestbook可以保存信息，展示信息。此应用将涉及到简单的HTML模板，数据库访问和项目结构.

# 安装Leiningen

首先你需要安装Leiningen才能使用Luminus。安装Leiningen非常的简单:

-   下载脚本
-   将其设置为可执行权限(chmod +x lein)
-   将脚本放到你的PATH下面
-   运行lein self-install ，然后等待安装结束


```sh
wget https://raw.github.com/technomancy/leiningen/stable/bin/lein
chmod +x lein
mv lein ~/bin
lein self-install
```

# 创建一个新应用

安装完Leiningen后，你就可以在命令行中输入如下的命令

```sh
lein new luminus guestbook +h2
cd guestbook
```

上面的命令将会创建一个使用了H2嵌入式数据库的模板项目。现在我们就可以运行这个项目了.

```sh
lein ring server
guestbook started successfully...
2013-03-01 19:05:30.389:INFO:oejs.Server:jetty-7.6.1.v20120215
Started server on port 3000
2013-03-01 19:05:30.459:INFO:oejs.AbstractConnector:Started SelectChannelConnector@0.0.0.0:3000
```

浏览器会自动打开，你将能看到运行的应用。如果你不想浏览器自动打开，你可以用下面的命令来启动项目.

```sh
lein ring server-headless
```

你也可以自定义端口号，命令如下:

```sh
lein ring server-headless 8000
```

<!-- more -->

# 剖析Luminus应用

新创建的项目的目录结构如下

```example
Procfile
README.md
project.clj
src
  └ log4j.xml
    guestbook
       └ handler.clj
         util.clj
         repl.clj
         models
           └ db.clj
             schema.clj
          routes
           └ home.clj
          views
           └ layout.clj
           └ templates
              └ about.html
                base.html
                home.html
test
  └ guestbook
       └ test
           └ handler.clj
resources
  └ public
       └ css
           └ screen.css
             img
             js
             md
              └ docs.md
```

我们先来看一看根目录下的文件的作用:

-   Procfile - 部署相关信息
-   README.md - 项目相关的文档
-   project.clj - 用于管理项目的配置以及Leiningen依赖关系

# 源代码目录

所有的代码都在src目录下。我们的应用名称叫guestbook，这个名字同时也是源代码的根命名空间。让我们来看一下源代码目录下所有的命名空间。

## guestbook

-   handler.clj - 定义了应用最基本的路由。这是应用的入口。我们自定义的所有的页面都需要在这里添加各自的路由定义。
-   layout.clj - 页面布局帮助命名空间
-   middleware.clj - 中间件命名空间
-   repl.clj - 提供了从REPL启动和停止应用的函数
-   util.clj - 提供了常用的帮助类函数，比较常用的是md-\>html的帮助函数
-   log4j.xml - Korma的日志配置文件

## guestbook.db

db命名空间下定义的是应用所使用的model以及持久化相关操作.

-   core.clj - 包含一组可以和数据库交互的函数
-   schema.clj - 用来定义数据库连接参数以及数据表

## guestbook.routes

routes命名空间是存放路由以及controller的地方。当你要添加路由的时候，比如安全验证，特殊流程等等，你需要在这里创建他们。

-   home.clj - 一个定义了home和about页面的命名空间





# 测试目录

这里是放置测试代码的地方。该目录下已经有样例测试代码。

# 资源目录

这里是存放静态资源的地方。包括css,javascript,images和markdown.

## HTML模板

这个命名空间存放的是Selmer模板文件，用于应用页面的展示。

-   about.html - about页面
-   base.html - base页面
-   home.html - home页面

# 添加依赖

上面已经说过了，项目所有的依赖关系都是由project.clj来管理的。这个文件看起来像这样。

``` {.clojure}
(defproject
  guestbook "0.1.0-SNAPSHOT"

  :url "http://example.com/FIXME"
  :description "FIXME: write description"

  :dependencies
  [[com.h2database/h2 "1.4.178"]
   [ring-server "0.3.1"]
   [environ "1.0.0"]
   [com.taoensso/timbre "3.2.1"]
   [markdown-clj "0.9.55"]
   [korma "0.4.0"]
   [com.taoensso/tower "2.0.2"]
   [selmer "0.7.2"]
   [org.clojure/clojure "1.6.0"]
   [log4j
    "1.2.17"
    :exclusions
    [javax.mail/mail
     javax.jms/jms
     com.sun.jdmk/jmxtools
     com.sun.jmx/jmxri]]
   [compojure "1.2.1"]
   [lib-noir "0.9.4"]]

  :plugins
  [[lein-ring "0.8.13"] [lein-environ "1.0.0"]]

  :ring
  {:handler guestbook.handler/app,
   :init guestbook.handler/init,
   :destroy guestbook.handler/destroy}

  :profiles
  {:uberjar {:aot :all}
   :production
   {:ring
    {:open-browser? false, :stacktraces? false, :auto-reload? false}},
   :dev
   {:dependencies [[ring-mock "0.1.5"] [ring/ring-devel "1.2.2"]],
    :env {:dev true}}}

  :min-lein-version "2.0.0")
```

project.clj就是个简单的Clojure的list，这个list中包含了键值对，描述了应用的方方面面。如果你要添加自定义依赖，只需要简单的将需要的依赖添加到:dependencies这个vector内。

# 访问数据库

首先，我们来创建一个model。我们需要编辑位于src/guestbook/db目录下的schema.clj文件。
这这个文件中，已经定义了数据库连接。定义的方式就是使用了一个包含了jdbc驱动，协议，用户名和密码的map。

``` {.clojure}
(ns guestbook.db.schema
  (:require [clojure.java.jdbc :as sql]
            [clojure.java.io :refer [file]]
            [noir.io :as io]))

(def db-store (str (.getName (file ".")) "/site.db"))

(def db-spec {:classname "org.h2.Driver"
              :subprotocol "h2"
              :subname db-store
              :user "sa"
              :password ""
              :make-pool? true
              :naming {:keys clojure.string/lower-case
                       :fields clojure.string/upper-case}})
```

紧接着是一个叫做create-users-table的函数。这是用来定义名字叫做users的
数据表的。我们替换掉这个函数，如下:

``` {.clojure}
(defn create-guestbook-table []
  (sql/db-do-commands
    db-spec
    (sql/create-table-ddl
      :guestbook
      [:id "INTEGER PRIMARY KEY AUTO_INCREMENT"]
      [:timestamp :timestamp]
      [:name "varchar(30)"]
      [:message "varchar(200)"]))
  (sql/db-do-prepared db-spec
      "CREATE INDEX timestamp_index ON guestbook (timestamp)"))
```

这个函数定义了guestbook数据表结构。同时我们修改create-tables函数来调用它:

``` {.clojure}
(defn create-tables
  "creates the database tables used by the application"
  []
  (create-guestbook-table))
```

数据表创建完成后，我们就可以来读写留言信息了。让我们打开core.clj文件。同样的，文件里已经有了一些代码，我们需要替换它们。

``` {.clojure}
(ns guestbook.db.core
  (:use korma.core
        [korma.db :only (defdb)])
  (:require [guestbook.db.schema :as schema]))

(defdb db schema/db-spec)

(defentity guestbook)

(defn save-message
  [name message]
  (insert guestbook
          (values {:name name
                   :message message
                   :timestamp (new java.util.Date)})))

(defn get-messages []
  (select guestbook))
```

上面我们创建了一个实体来映射guestbook数据表。然后创建了save-message和get-message来操作它.

# 启动时运行

hander命名空间中包含了一个叫做init的函数。这个函数只会在应用启动的时候调用一次。让我们来添加一些代码来验证数据库是否在应用启动前初始化成功。
为了能够使用initialized?和create-tables函数，我们需要一个指向schema命名空间的引用。

``` {.clojure}
(ns guestbook.handler
  (:use ...)
  (:require ...
            [guestbook.db.schema :as schema]))
```

然后我们修改init函数:

``` {.clojure}
(defn init
  "init will be called once when
   app is deployed as a servlet on
   an app server such as Tomcat
   put any initialization code here"
  []
  (timbre/set-config!
    [:appenders :rotor]
    {:min-level :info
     :enabled? true
     :async? false ; should be always false for rotor
     :max-message-per-msecs nil
     :fn rotor/appender-fn})

  (timbre/set-config!
    [:shared-appender-config :rotor]
    {:path "guestbook.log" :max-size (* 512 1024) :backlog 10})

  (if (env :selmer-dev) (parser/cache-off!))

  ;;initialize the database if needed
  (when-not (schema/initialized?)
    (schema/create-tables))

  (timbre/info "guestbook started successfully"))
```

如果我们修改了init函数，我们就需要重启应用。使用CTRL+c来停止应用，再输入lein ring server来启动应用。

# 创建表单页面

应用的路由是定义在guestbook.routes.home命名空间下的。我们来打开它，并添加一些逻辑。首先，需要添加db命名空间.

``` {.clojure}
(ns guestbook.routes.home
  (:use ...)
  (:require ...
            [guestbook.db.core :as db]))
```

然后呢，我们会修改home-page这个controller:

``` {.clojure}
(defn home-page [& [name message error]]
  (layout/render "home.html"
                 {:error    error
                  :name     name
                  :message  message
                  :messages (db/get-messages)}))
```

我们所做的就是多传递了几个参数给模板，其中一个是从数据库中查询到的信息.

我们提供了用户可以发布新留言的功能，所以我们需要在controller中来处理这
个请求:

``` {.clojure}
(defn save-message [name message]
  (cond

    (empty? name)
    (home-page name message "Somebody forgot to leave a name")

    (empty? message)
    (home-page name message "Don't you have something to say?")

    :else
    (do
      (db/save-message name message)
      (home-page))))
```

最后呢，在home-routes定义里面添加这个controller的路由定义。

``` {.clojure}
(defroutes home-routes
  (GET "/" [] (home-page))
  (POST "/" [name message] (save-message name message))
  (GET "/about" [] (about-page)))
```

controller已经编写OK。我们打开处于resources/templates下的home.html模板,目前只是简单的显示内容：

```html
{ % extends "templates/base.html" %}
{ % block content %}
 <div class="jumbotron">
    <h1>Welcome to guestbook</h1>
    <p>Time to start building your site!</p>
    <p><a class="btn btn-primary btn-lg" href="http://luminusweb.net">Learn more &raquo;</a></p>
 </div>

 <div class="row-fluid">
    <div class="span8">
    { {content|safe}}
    </div>
 </div>
{ % endblock %}
```

我们改为如下代码：

```html
{ % extends "templates/base.html" %}
{ % block content %}
 <div class="jumbotron">
    <h1>Welcome to guestbook</h1>
 </div>

 <div class="row-fluid">
    <div class="span8">
      <ul>
      { % for item in messages %}
        <li>
          <time>{ {item.timestamp|date:"yyyy-MM-dd HH:mm"}}</time>
          <p>{ {item.message}}</p>
          <p> - { {item.name}}</p>
        </li>
      { % endfor %}
      </ul>
    </div>
 </div>
{ % endblock %}

```

我们使用了迭代器来遍历信息。而每个迭代结果都是一个包含了信息的map。我们能通过名字来访问它们。同时，我们使用了一个日期过滤器来生成一个适于人类阅读的时间. 接着我们来添加错误信息的展示.

```html
{ % if error %}
    <p>{ {error}}</p>
{ % endif %}

```

我们只是简单的检查了一下是否有错误信息，如果有就展示。最后我们创建一个form来接受用户提交留言.

``` {.html}
<form action="/" method="POST">
    <p>
       Name:
       <input type="text" name="name" value={ {name}}>
    </p>
    <p>
       Message:
       <textarea rows="4" cols="50" name="message">
           { {message}}
       </textarea>
    </p>
    <input type="submit" value="comment">
</form>
```

最后，home.html看起来像这样

```html
{ % extends "guestbook/views/templates/base.html" %}

{ % block content %}
    <ul>
    { % for item in messages %}
      <li>
          <time>{ {item.timestamp|date:"yyyy-MM-dd HH:mm"}}</time>
          <p>{ {item.message}}</p>
          <p> - { {item.name}}</p>
      </li>
    { % endfor %}
    </ul>

{ % if error %}
    <p>{ {error}}</p>
{ % endif %}

<form action="/" method="POST">
    <p>
       Name:
       <input type="text" name="name" value={ {name}}>
    </p>
    <p>
       Message:
       <textarea rows="4" cols="50" name="message">
           { {message}}
       </textarea>
    </p>
    <input type="submit" value="comment">
</form>
{ % endblock %}

```

我们可以修改位于resources/public/css目录下的screen.css来使得页面更好看一些.

``` {.css}
body {
	height: 100%;
	padding-top: 70px;
	font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
	line-height: 1.4em;
	background: #eaeaea;
	color: #4d4d4d;
	width: 550px;
	margin: 0 auto;
	-webkit-font-smoothing: antialiased;
	-moz-font-smoothing: antialiased;
	-ms-font-smoothing: antialiased;
	-o-font-smoothing: antialiased;
	font-smoothing: antialiased;
}

input[type=submit] {
	margin: 0;
	padding: 0;
	border: 0;
  line-height: 1.4em;
	background: none;
	vertical-align: baseline;
}

input[type=submit], textarea {
	font-size: 24px;
	font-family: inherit;
	border: 0;
	padding: 6px;
	border: 1px solid #999;
	box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
	-moz-box-sizing: border-box;
	-ms-box-sizing: border-box;
	-o-box-sizing: border-box;
	box-sizing: border-box;
}

input[type=submit]:hover {
	background: rgba(0, 0, 0, 0.15);
	box-shadow: 0 -1px 0 0 rgba(0, 0, 0, 0.3);
}

textarea {
	position: relative;
	line-height: 1em;
	width: 100%;
}

.error {
  font-weight: bold;
	color: red;
}

.jumbotron {
	position: relative;
	background: white;
	z-index: 2;
	border-top: 1px dotted #adadad;
}

h1 {
	width: 100%;
	font-size: 70px;
	font-weight: bold;
	text-align: center;
}

ul {
	margin: 0;
	padding: 0;
	list-style: none;
}

li {
	position: relative;
	font-size: 16px;
	padding: 5px;
	border-bottom: 1px dotted #ccc;
  box-shadow: 0 2px 6px 0 rgba(0, 0, 0, 0.2),
	            0 25px 50px 0 rgba(0, 0, 0, 0.15);
}

li:last-child {
	border-bottom: none;
}

li time {
	font-size: 12px;
	padding-bottom: 20px;
}

form:before, .error:before {
	content: '';
	position: absolute;
	top: 0;
	right: 0;
	left: 0;
	height: 15px;
	border-bottom: 1px solid #6c615c;
	background: #8d7d77;
}

form, .error {
	width: 520px;
	padding: 30px;
	margin-bottom: 50px;
	background: #fff;
	border: 1px solid #ccc;
	position: relative;
	box-shadow: 0 2px 6px 0 rgba(0, 0, 0, 0.2),
	            0 25px 50px 0 rgba(0, 0, 0, 0.15);
}

form input {
	width: 50%;
	clear: both;
}
```

现在刷新页面就可以看到我们修改的内容了。试试留个言！

# 打包应用

要打包程序，可输入

```sh
lein ring uberjar
```

这将会创建一个可运行的jar。通过下面的命令来运行

```sh
java -jar target/guestbook-0.1.0-SNAPSHOT-standalone.jar
```

如果我们想把应用部署到tomcat这样的服务器上，你可以运行

```sh
lein ring uberwar
```

这将会打包一个war包。

完整的源代码可以到[这里](https://github.com/yogthos/guestbook)下载。

