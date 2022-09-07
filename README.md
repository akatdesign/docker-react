## docker-react


**環境構築**
```
$ cd docker-react/
$ docker-compose build
$ docker-compose up -d
$ docker exec -it node sh
$ npx create-react-app <project_name> --template typescript --use-npm
$ npm start(ホットリロード,eslint,prettierの設定の後)
``` 
**ホットリロード(windows)**
- windowsの場合jsonが確実、いずれかを試す

package.js
```
"scripts": {
  "start": "WATCHPACK_POLLING=true react-scripts start",
  "build": ...
},
```
.env
```
CHOKIDAR_USEPOLLING=true
```
docker-compose.yml
```
    environment:
      - ....
      - CHOKIDAR_USEPOLLING=true
```
**環境から抜ける**
```
$ exit
$ docker compose down
$ docker image rm imageid
```
**確認**
```
$ docker image ls
$ docker container ls -a
```
**eslintの設定**
```
$ npx eslint --init
```
- 設定内容
```
ESLintのチェックをどこまで行うのですか？
? How would you like to use ESLint? … 
To check syntax only
To check syntax and find problems
To check syntax, find problems, and enforce code style　<-これを選んでください

使っているモジュールは何ですか？
? What type of modules does your project use? … 
JavaScript modules (import/export)<-これを選んでください
CommonJS (require/exports)
None of these

プロジェクトで使用しているフレームワークはどれですか？
? Which framework does your project use? … 
React<-これを選んでください 
Vue.js
None of these

プロジェクトはTypeScriptを使用していますか？
Does your project use TypeScript? · No / Yes <-Yesを選択してください

コードはどこで実行されますか？
 Where does your code run? … (Press <space> to select, <a> to toggle all, <i> to invert selection)
 ✔ Browser<-これを選んでください
 ✔ Node

設定ファイルをどの形式にしますか？
What format do you want your config file to be in? … ❯ 
JavaScript <-これを選んでください
YAML 
JSON

プロジェクトのスタイルをどのように定義しますか？
How would you like to define a style for your project? … 
Use a popular style guide<-これを選んでください
Answer questions about your style
Inspect your JavaScript file(s)
```
- eslintrc.jsができているか確認
```
選択した構成には、次の依存関係が必要です。
eslint-plugin-react の最新バージョンを今すぐ npm でインストールしますか?
eslint-plugin-react@latest
? Would you like to install them now with npm? › No / Yes<-Yesを選択してください（npmインストールが始まります）

Successfully created .eslintrc.js file in /front
ESLint was installed locally. We recommend using this local copy instead of your globally-installed copy.
上記の表示が出ればOK
```
- .eslintrc.jsにeslintの設定を追記
```
module.exports = {
    env: {
        browser: true,
        es2021: true,
        "jest/globals": true,<-追記
    },
    extends: [
        "plugin:react/recommended",
        "standard",
        "plugin:@typescript-eslint/recommended",<-追記
    ],
    parser: "@typescript-eslint/parser",
    parserOptions: {
        ecmaFeatures: {
            jsx: true,
        },
        ecmaVersion: 12,
        sourceType: "module",
    },
    plugins: ["react", "@typescript-eslint", "jest"],
    rules: {
        "no-use-before-define": "off",<-追記
        "@typescript-eslint/no-use-before-define": ["error"],<-追記
    },
    settings: {
        react: {
            version: "detect",<-追記
        },
    },
};
```
**prettierの設定**
- prettierをインストール
```
/front #　npm i -D prettier eslint-config-prettier eslint-plugin-prettier pretty-quick
----------------------------------------------------------------------------
Run `npm audit` for details.
/front
上記になればOK
```
- prettierの設定を.eslintrc.jsに追記
```
module.exports = {
    env: {
        browser: true,
        es2021: true,
        "jest/globals": true,
    },
    extends: [
        "plugin:react/recommended",
        "standard",
        "plugin:@typescript-eslint/recommended",
        "prettier"<-追記
    ],
    parser: "@typescript-eslint/parser",
    parserOptions: {
        ecmaFeatures: {
            jsx: true,
        },
        ecmaVersion: 12,
        sourceType: "module",
    },
    plugins: ["react", "@typescript-eslint", "jest", "prettier"],<-"prettier"追記
    rules: {
        "no-use-before-define": "off",
        "@typescript-eslint/no-use-before-define": ["error"],
        "prettier/prettier": "error",<-追記
    },
    settings: {
        react: {
            version: "detect",
        },
    },
};
```
- フォーマットの実行
```bash
npm strat
npx eslint --fix src/**/*.ts*
```
