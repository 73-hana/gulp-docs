# ファイルの監視

`watch()`API はグロブとタスクを結びつけるものである。ファイルの変更を監視し、変更が発生したらタスクが実行されるようになる。

```js
const { watch, series } = require("gulp");

function clean(cb) {
  //tasks
  cb();
}

function javascript(cb) {
  //tasks
  cb();
}

function css(cb) {
  //tasks
  cb();
}

exports.default = function () {
  watch("src/*.css", css);
  watch("src/*.js", series(clean, javascript));
};
```

## 注意：同期を控える

ウォッチャーは同期的処理が出来ない。同期処理を行った場合は 2 回目以降処理されない。エラーも警告も出ない。

## Watch のイベント

デフォルトでは、ウォッチャーは CRUD のいずれでもタスクを実行させる。もしイベントを個別に設定したい場合は、`watch()`の引数で指定する。

```js
const { watch } = require("gulp");

exports.default = function () {
  // all event will be watched
  watch("src/*.js", { events: "all" }, function (cb) {
    cb();
  });
};
```

## 実行を開始する

`watch()`を呼び出すと、初回はタスクを実行せずファイルの更新を待つ。もし初回にもタスクを実行させたい場合は`ignoreInitial`オプションを設定する。

```js
const { watch } = require("gulp");

exports.default = function () {
  watch("src/*.js", { ignoreInitial: false }, function (cb) {
    cb();
  });
};
```

## 列

それぞれの`watch`は、現在実行中のタスクが、同時に同じものが 2 回起きないことを保証している。タスクの実行中のファイル変更が行われた場合は、別のタスクがキューに入れられる。

この設定をオフにするにはオプションが用意されている。

```js
const { watch } = require("gulp");

exports.default = function () {
  watch("src/*.js", { queue: false }, function (cb) {
    cb();
  });
};
```

## 遅延

ファイルの変更が行われると、ウォッチャーは 200 ミリ秒経過するまでタスクを実行しない。この時間を変更するにはオプションを用いる。

```js
const { watch } = require("gulp");

exports.default = function () {
  watch("src/*.js", { delay: 500 }, function (cb) {
    cb();
  });
};
```
