+++
title = "スタートアップ"
menuTitle = "スタートアップ"
date = 2018-11-10T11:08:02+09:00
weight = 6
pre = "<i class='fab fa-github'></i> "
# draft: true
+++

---

## 開発環境の構築

### Node.jsのインストール

#### Brewによるインストール

```bash
brew update
brew install node
brew upgrade node
node -v
npm -v
```

#### Tarファイルからインストール

```bash
echo 'export PATH=$HOME/local/bin:$PATH' >> ~/.bashrc
. ~/.bashrc

mkdir ~/local ~/node-latest-install
cd ~/node-latest-install
curl http://nodejs.org/dist/node-latest.tar.gz | tar xz --strip-components=1
$./configure --prefix=~/local
$ make install
$ curl https://npmjs.org/install.sh | sh
```

### npmパッケージの実行ツール

```bash
npm i -g npx
```

## Node.js プロジェクトの設定

### Node.jsプロジェクトの作成

```bash
mkdir webapi && cd webapi
mkdir src dist
touch .gitignore .editorconfig .prettierrc
touch src/server.js

npm init -y
git init
```

### Transpilerの準備: Babel

ES2015(ES6)をコンバイルするのに必要なパッケージ`babel`をインストールする。`babel`は7.0よりScopedパッケージとなった。 `babel`は7.0以上、`nodemon`は1.18以上。

1. @babel/cli
2. @babel/core
3. @babel/node
4. @babel/preset-env
5. nodemon  (version 1.18〜)

```bash
npm i -D @babel/core @babel/cli @babel/node @babel/preset-env nodemon
touch .babelrc
```

`.babelrc`の設定内容は以下である。

```json
// .babelrc
{
  "presets": ["@babel/preset-env"]
}
```

#### Reference

- [Hackernoon:Using babel 7 with node](https://hackernoon.com/using-babel-7-with-node-7e401bc28b04)
- [The minimal Node.js with Babel Setup](https://www.robinwieruch.de/minimal-node-js-babel-setup/)

### LinterとFormater

{{% notice info %}}
`Prettier` + `ESLint` + `Airbnb Style Guide`で実現したいのは、「ESLintフォーマットルールをPrettierで取替えることで、ESLintのコンバイルチェック機能のみを採用することにある。
{{% /notice %}}

インストールする対象パッケージは以下である。

1. **elint** 、**prettier**
2. **Airbnb config**
3. **eslint-config-prettier** (ESLintのFormatting機能の無効化)
4. **eslint-plugin-prettier** (PerttierのFormatting機能をESLintと連携、Perttierコーディング規約違反の部分をESLintのエラーとして表示する)

```bash
# 1. Install eslint, prettier
npm i -D eslint prettier

# 2. Install `Airbnb config`
npx install-peerdeps --dev eslint-config-airbnb
npm i -D eslint eslint-config-airbnb-base eslint-plugin-import

# 3. ESLintとPrettierとの連携パッケージのインストール
npm i -D eslint-config-prettier eslint-plugin-prettier

# 4. ESLintの設定
touch .eslintrc.json

# 5. Prettierの設定
touch .prettierignore .prettierrc.json

#npm install eslint-{config,plugin}-prettier eslint prettier --save-dev --save-exact
```

`.eslintrc.js`の設定内容は以下である。

```javascript
module.exports = {
  extends: ['eslint:recommended', 'airbnb', 'prettier'],
  env: {
    es6: true,
    node: true,
    'jest/globals': true,
  },
  plugins: ['prettier', 'jest'],
  rules: {
    'prettier/prettier': ['error'],
    'no-console': 0,
  },
};

```

まず、`.prettierrc.json`の内容は以下の通りである。

```json
{
  "arrowParens": "avoid",
  "bracketSpacing": false,
  "jsxBracketSameLine": false,
  "printWidth": 80,
  "proseWrap": "preserve",
  "requirePragma": false,
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "all",
  "useTabs": false,
  "overrides": [
    {
      "files": "*.json",
      "options": {
        "printWidth": 200
      }
    }
  ]
}
```

次、`.prettierrcignore`の内容は以下の通りである。

```json
dist/
```

#### Reference

* [Medium: Integrating Prettier + ESLint + Airbnb Style Guide in VSCode](<https://blog.echobind.com/integrating-prettier-eslint-airbnb-style-guide-in-vscode-47f07b5d7d6a>)
* [Medium: Setting up ESLint on VS Code with Airbnb JavaScript Style Guide](<https://travishorn.com/setting-up-eslint-on-vs-code-with-airbnb-javascript-style-guide-6eb78a535ba6>)

### TestFramework

TestFrameworkパッケージ：

- jest
- supertest (high-level abstraction for testing HTTP)

```bash
npm install --save-dev jest babel-jest regenerator-runtime
npm install --save-dev babel-core@^7.0.0-bridge.0
npm install --save-dev eslint-plugin-jest
npm install --save-dev supertest
```

`.eslintrs.js`に以下の内容を追加する。

```json
{
  "env": {
    "jest/globals": true
  }
}
```

#### Reference

- [Jest Getting Started](https://jestjs.io/docs/en/getting-started#using-babel)
- [npmjs elint-pluin-jest](https://www.npmjs.com/package/eslint-plugin-jest)
- [Hackernoon: API Testing using SuperTest](<https://hackernoon.com/api-testing-using-supertest-1f830ce838f1>)

### Editorの設定

#### Editor共通の設定: editorconfig

```bash
touch .editorconfig
```

`.editorconfig`に以下の内容を設定する。

```json
root = true

[*]
charset = utf-8
end_of_line = lf
indent_style = space
indent_size = 2
insert_final_newline = true
trim_trailing_whitespace = true
```

#### VSCodeの準備

VSCodeの場合、以下のPluginをインストールしておく:

- ESLint
- Prettier

### 開発スクリプトの設定

- **コンバイルコマンド**
- **開発サーバーの起動**
- **サーバーの起動**

`package.json`に以下の内容を追加する。

```json
  "scripts": {
    "format": "prettier --write src/**/*.js",
    "lint": "eslint src --color",
    "build": "babel src --out-dir dist --ignore node_modules",
    "start": "nodemon --exec babel-node src/server.js",
    "serve": "node dist/server.js",
    "test": "jtest"
  },

```
## HUGOについて

```bash
hugo new mysite
cd mysite/theme
git clone https://github.com/matcornic/hugo-theme-learn.git
```