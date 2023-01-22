# Quick Start

## gulp のインストール手順

まずは node と npm、npx の確認をする。バージョンの確認ができない場合はインストールを行う。

```bash
node --version
npm --version
npx --version
```

次に gulp コマンドラインユーティリティをインストールする。これはグローバル環境にインストールする。

```bash
npm install --global gulp-cli
```

プロジェクトディレクトリに移動して、`package.json`ファイルを作成する。

```bash
mkdir <my-project>
cd <my-project>
npm init
```

その後 gulp のパッケージをローカル環境にインストールする。

```bash
npm install --save-dev gulp
```

gulp のバージョンを確認して、インストール完了。

```bash
gulp --version
```

## テストを行う

ルートディレクトリ（package.json があるディレクトリ）に`gulpfile.js`を作成する。

その後、ルートディレクトリで gulp コマンドを実行する。

```bash
gulp
# 実行結果
```
