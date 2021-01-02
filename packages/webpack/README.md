# Webpack

> 官网 | Official website: <https://webpack.js.org/>
> 文档 | Documentation: <https://webpack.js.org/concepts/>

## Initialize project

```sh
mkdir webpack && cd webpack
echo "node_modules/" >> .gitignore

yarn init -y
# or
npm init -y
```

## Install dependencies

```sh
yarn add webpack webpack-cli --dev
# or
npm install webpack webpack-cli --save-dev
```

## Create file (folder)

```sh
mkdir src && touch src/index.js src/index.html
touch webpack.config.js
```

**src/index.js:**

```javascript
alert("hello world!");
```

**dist/index.html:**

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Empty project</title>
        <meta charset="utf-8">
    </head>
    <body>
        <div id="app"></div>
        <script src="bundle.js"></script>
    </body>
</html>
```

**webpack.config.js:**

```javascript
const webpack = require('webpack');
const path = require('path');

const config = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  }
};

module.exports = config;
```

**package.json:**

```json
{
  "scripts": {
    "clean": "rm dist/bundle.js",
    "build:dev": "webpack --mode development",
    "build:prod": "webpack --mode production"
  },
}
```

## Building

```sh
# To create a production build
yarn build:prod
# or
npm run build:prod

# To create a development build
yarn build:dev
# or
npm run build:dev
```

## Running

```sh
node dist/bundle.js
```
