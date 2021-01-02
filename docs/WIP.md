
# Work in progress

## Initialize Project

```shell
mkdir typescript-configures && cd typescript-configures
git init && echo "# TypeScript Configures" >> README.md

yarn init && echo "node_modules/\nyarn.lock" > .gitignore 
# or
npm init && echo "node_modules/\npackage-lock.json" > .gitignore 
```

## Commit Convention

**安装依赖** | Install dependencies

```shell
yarn add husky lint-staged --dev

# or
npm i husky lint-staged --save-dev
```

**创建文件（夹）** | Create file (folder)

```shell
mkdir scripts && touch scripts/verify-commit-msg.js
```

**配置 Git Hooks** | Configure Git Hooks

**scripts/verify-commit-msg.js:**

```javascript
#!/usr/bin/env node

/**
 * This is a commit-msg sample running in the Node environment,
 *    and will be invoked on the commit-msg git hook.
 *
 * You can use it by renaming it to `commit-msg` (without path extension),
 *    and then copying the renamed file to your project's directory `.git/hooks/`.
 *
 * Note: To ensure it can be run, you should grunt the renamed file (`commit-msg`)
 *    with running command `chmod a+x .git/hooks/commit-msg` in your project's directory.
 */
const chalk = require("chalk")
const message = require("fs").readFileSync(process.argv[2], "utf-8").trim()

const COMMIT_REG = /^(revert: )?(WIP|feat|fix|docs|style|refactor|perf|test|workflow|build|ci|chore|merge|release)(\(.+\))?: .{1,50}/

if (!COMMIT_REG.test(message)) {
    console.log()
    console.error(
        `  ${chalk.bgRed.white(" ERROR: ")} ${chalk.red(`Invalid commit message format.`)}\n\n` +
            chalk.red(`  Proper commit message format is required for automated changelog generation. Examples:\n\n`) +
            `    ${chalk.green(`feat(pencil): add 'graphiteWidth' option`)}\n` +
            `    ${chalk.green(`fix(graphite): stop graphite breaking when width < 0.1 (close #28)`)}\n\n` +
            chalk.red(`  See .github/commit-convention.md for more details.\n`),
    )
    process.exit(1)
}
```

**package.json:**

```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "commit-msg": "node scripts/verify-commit-msg.js ${HUSKY_GIT_PARAMS}"
    }
  },
  "lint-staged": {
    
  },
}
```

## Coding Convention

**安装依赖** | Install dependencies

```shell
yarn add eslint typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin --dev # for eslint
yarn add prettier eslint-config-prettier eslint-plugin-prettier --dev # for prettier

# or
npm i eslint typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin --save-dev # for eslint
npm i  prettier eslint-config-prettier eslint-plugin-prettier --save-dev # for prettier
```

**创建文件（夹）** | Create file (folder)

```shell
touch .editorconfig
touch .eslintrc.js .eslintignore
mkdir .vscode && touch .vscode/settings.json
touch .prettierrc .prettierignore
```

**配置 EditorConfig** | Configure EditorConfig

```toml
# EditorConfig is awesome: https://EditorConfig.org

# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*]
end_of_line = lf
insert_final_newline = true
charset = utf-8
indent_style = space
indent_size = 4

[{*.json,*.md,*.yml,*yaml,*.*rc}]
indent_style = space
indent_size = 2
```

**配置 ESLint** | Configure ESLint

**.eslintrc.js:**

```javascript
module.exports = {
    env: {
        browser: true,
        es2021: true,
        node: true,
    },
    extends: [
        "plugin:@typescript-eslint/recommended",
        // 用于关闭 ESLint 相关的格式规则集，详见：https://github.com/prettier/eslint-config-prettier/blob/master/index.js
        "prettier",
        // 用于关闭 @typescript-eslint/eslint-plugin 插件相关的格式规则集，详见：https://github.com/prettier/eslint-config-prettier/blob/master/@typescript-eslint.js
        "prettier/@typescript-eslint",
    ],
    parser: "@typescript-eslint/parser",
    parserOptions: {
        ecmaFeatures: {
            jsx: true,
        },
        ecmaVersion: 12,
        sourceType: "module",
    },
    plugins: ["@typescript-eslint", "prettier"],
    rules: {}
}
```

**.eslintignore:**

```text
.github
.vscode

node_modules
```

**.vscode/settings.json:**

```json
{
  "editor.formatOnSave": false,
  "editor.codeActionsOnSave": {
    "source.fixAll": true,
    "source.fixAll.eslint": true
  },
  "eslint.format.enable": true,
  "eslint.validate": ["javascript", "typescript", "javascriptreact", "typescriptreact", "vue"],
  "files.autoSave": "onFocusChange",
  "files.associations": {
    "*.ttml": "xml",
    "*.ttss": "css"
  }
}
```

**配置 Prettier** | Configure Prettier

**.prettierrc:**

```json
{
  "trailingComma": "all",
  "tabWidth": 4,
  "semi": false,
  "singleQuote": false,
  "endOfLine": "lf",
  "printWidth": 120,
  "overrides": [
    {
      "files": ["*.md", "*.json", "*.yml", "*.yaml", "*.*rc"],
      "options": {
        "tabWidth": 2
      }
    }
  ]
}
```

**.prettierignore:**

```text
# Ignore artifacts:
/build
/coverage
/docs

/.github
/.vscode
/.history

package.json
package-lock.json
yarn.lock
```

**package.json:**

```json
{
  "scripts": {
    "format": "prettier --write . --ignore-unknown",
    "lint": "eslint . --ext .ts,.tsx",
    "lint:fix": "eslint . --ext .ts,.tsx --fix"
  },
  "lint-staged": {
    "*.ts?(x)": [
      "prettier --write --ignore-unknown",
      "eslint --fix"
    ]
  },
}
```
