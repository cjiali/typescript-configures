# Parcel

Parcel 是一个 Web 应用程序打包程序，其开发人员经验与众不同。它利用多核处理提供了惊人的快速性能，并且需要零配置。

Parcel is a web application bundler, differentiated by its developer experience. It offers blazing fast performance utilizing multi-core processing, and requires zero configuration.

## Initialize project

```sh
mkdir parcel && cd parcel
echo "dist/\n.cache/\nnode_modules/" >> .gitignore

yarn init -y
# or
npm init -y
```

## Install dependencies

```sh
yarn add parcel-bundler --dev
# or
npm install parcel-bundler --save--dev
```

## Create file (folder)

```sh
mkdir src
touch src/index.html src/index.js
```

Parcel 可以使用任何类型的文件作为入口点，但是 HTML 或 JavaScript 文件是一个很好的起点。如果您使用相对路径在HTML中链接主 JavaScript 文件，Parcel 还将为您处理它，并将引用替换为输出文件的URL。

Parcel can take any type of file as an entry point, but an HTML or JavaScript file is a good place to start. If you link your main JavaScript file in the HTML using a relative path, Parcel will also process it for you, and replace the reference with a URL to the output file.

**src/index.html:**

```html
<html>
<body>
  <script src="./index.js"></script>
</body>
</html>
```

> **注意：** Parcel 将 JS 资源转换为 ES5，该资源不会在 `<script type="module">` 标签的上下文中运行，因此只需在源 HTML 中使用不带 type 属性的普通 `<script>` 标签。
>
> **Note:** Parcel converts JS assets to ES5, which won't run in in the context of a `<script type="module">` tag, so just use plain `<script>` tags with no type attribute in your source HTML.

**src/index.js:**

```javascript
alert('hello world!')
```

**package.json:**

```json
{
  "scripts": {
    "start": "parcel src/index.html",
    "watch": "parcel watch src/index.html",
    "build": "parcel build src/index.html"
  }
}
```

## Running

Parcel 内置有一个开发服务器，当更改文件时，它将自动重建应用程序，并支持热模块更换以实现快速开发。

Parcel has a development server built in, which will automatically rebuild your app as you change files and supports hot module replacement for fast development.

```sh
parcel index.html
```

在浏览器中打开 <http://localhost:1234/>。如果无法进行热模块更换，则可能需要配置编辑器。您也可以使用 `-p <port number>` 选项覆盖默认端口。

Now open <http://localhost:1234/> in your browser. If hot module replacement isn't working you may need to configure your editor. You can also override the default port with the `-p <port number>` option.

## Watching

如果没有自己的服务器，或者应用完全由客户端呈现，请使用开发服务器。如果有自己的服务器，则可以在监视模式下运行 Parcel。该文件仍会随着文件更改而自动重建，并支持热模块替换，但不会启动Web服务器。

Use the development server when you don't have your own server, or your app is entirely client rendered. If you do have your own server, you can run Parcel in watch mode instead. This still automatically rebuilds as files change and supports hot module replacement, but doesn't start a web server.

```sh
parcel watch index.html
```

## Building

当准备好进行生产时，构建模式将关闭监视功能，并且仅构建一次。

When you're ready to build for production, the build mode turns off watching and only builds once.

```sh
parcel build
# or
parcel build
```

## Multiple entry files

如果有一个以上的入口文件，假设是 `index.html` 和 `about.html`，有2种方法运行捆绑程序：

In case you have more than one entry file, let's say index.html and about.html, you have 2 ways to run the bundler:

```sh
# Specifying the file names
parcel index.html about.html

# Use tokens and create a glob
parcel *.html
```

**注意：** 如果具有如下的文件结构，则跳转到 <http://localhost:1234/folder-1/> 无效，相反，需要显式指向文件 <http://localhost:1234/folder-1/index.html>。

**NOTE:** In case you have a file structure like the following, Then going to <http://localhost:1234/folder-1/> won't work, instead you will need to explicitly point to the file <http://localhost:1234/folder-1/index.html>.

```text
- folder-1
-- index.html
- folder-2
-- index.html
```
