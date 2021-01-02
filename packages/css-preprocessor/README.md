# CSS Preprocessor

## Initialize project

```sh
mkdir css-preprocessor && cd css-preprocessor
echo "node_modules/" >> .gitignore

yarn init -y
# or
npm init -y
```

## Install dependencies

```sh
yarn add webpack webpack-cli --dev # use webpack to build package
yarn add postcss-loader autoprefixer --dev # handling css compatible
yarn add css-loader style-loader sass-loader node-sass less-loader less stylus-loader stylus --dev # css preprocessor
# or
npm install webpack webpack-cli --save-dev # use webpack to configure css preprocessor
npm install postcss-loader autoprefixer --save-dev # handling css compatible
npm install css-loader style-loader sass-loader node-sass less-loader less stylus-loader stylus --save-dev # css preprocessor
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

**styles.css:**

```css
h1 {
  color: white;
  background-color: black;
}
```

**styles.less:**

```less
@primary-color: white;
@bg: black;
h1 {
  color: @primary-color;
  background-color: @bg;
}
```

**styles.scss:**

```scss
$primary-color: white;
$bg: black;
h1 {
  color: $primary-color;
  background-color: $bg;
}
```

**styles.styl:**

```stylus
primary-color = white
bg = black
h1
  color: primary-color;
  background-color: bg;
```

**postcss.config.js:**

```js
module.exports = {
  plugins: [
    require('autoprefixer')
  ]
};
```

**webpack.config.js:**

```js
const webpack = require('webpack');
const path = require('path');

const config = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              importLoaders: 1
            }
          },
          'postcss-loader'
        ],
        exclude: /\.module\.css$/
      },
      {
        test: /\.css$/,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              importLoaders: 1,
              modules: true
            }
          },
          'postcss-loader'
        ],
        include: /\.module\.css$/
      },
      {
        test: /\.scss$/,
        use: [
          'style-loader',
          'css-loader',
          'sass-loader'
        ]
      },
      {
        test: /\.less$/,
        use: [
          'style-loader',
          'css-loader',
          'less-loader'
        ]
      },
      {
        test: /\.styl$/,
        use: [
          'style-loader',
          'css-loader',
          'stylus-loader'
        ]
      }
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
