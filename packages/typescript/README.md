# TypeScript

> 官网 | Official website: <https://www.typescriptlang.org/>
> 文档 | Documentation: <https://www.typescriptlang.org/docs>

## Initialize project

```sh
mkdir typescript && cd typescript
echo "node_modules/" >> .gitignore

yarn init -y
# or
npm init -y
```

## Install dependencies

```sh
yarn add webpack webpack-cli --dev
yarn add typescript ts-loader --dev
# or
npm install webpack webpack-cli --save-dev
npm install typescript ts-loader --save-dev
```

## Create file (folder)

```sh
mkdir src dist
touch src/index.js dist/index.html
touch src/styles.css src/styles.less src/styles.scss src/styles.styl
touch postcss.config.js webpack.config.js
```

**src/index.js:**

```javascript
console.log("hello world!")
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

**tsconfig.json:**

```json
{
    "compilerOptions": {
        "outDir": "./dist/",
        "sourceMap": true,
        "strict": true,
        "noImplicitReturns": true,
        "noImplicitAny": true,
        "module": "es6",
        "moduleResolution": "node",
        "target": "es5",
        "allowJs": true,
    },
    "include": [
        "./src/**/*"
    ]
}
```

**webpack.config.js:**

```js
const webpack = require('webpack');
const path = require('path');

const config = {
  entry: './src/index.ts',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.ts(x)?$/,
        loader: 'ts-loader',
        exclude: /node_modules/
      }
    ]
  },
  resolve: {
    extensions: [
      '.tsx',
      '.ts',
      '.js'
    ]
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
yarn run build:prod
# or
npm run build:prod

# To create a development build
yarn run build:dev
# or
npm run build:dev
```

## Running

```sh
node dist/bundle.js
```
