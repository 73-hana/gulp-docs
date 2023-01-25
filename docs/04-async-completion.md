# 非同期処理

Node における最も一般的な非同期処理のパターンはエラーファーストのコールバックである。

しかしストリームやプロミス、イベントエミッター、子プロセス、オブザーバなどがある。

Gulp はこれらの非同期処理を標準化することができる。

## タスク完了の合図

ストリーム、プロミス、イベントエミッタ、子プロセス、そしてオブザーバが返り値としてタスクから返ってきたとき、それが成功であれば Gulp は処理を続行し、失敗であれば Gulp は処理を中止してエラーを表示する。

`series()`を用いたタスクの場合は、エラーが発生した位置で合成処理が終了し、それ以降のタスクも実行されない。`parallel()`を用いたタスクの場合は、エラーが発生した位置で合成処理が終了するが、並列タスクは終了する場合とそうでない場合がある。

### stream

```js
const { src, dest } = require("gulp");

function streamTask() {
  return src("*.js").pipe(dest("output"));
}

exports.default = streamTask;
```

### promise

```js
function promiseTask() {
  return Promise.resolve("the value is ignored");
}

exports.default = promiseTask();
```

### event emitter

```js
const { EventEmitter } = require("events");

function eventEmitterTask() {
  const emitter = new EventEmitter();
  // emittは非同期処理として実行される、
  setTimeout(() => emitter.emit("finish"), 250);
  return emitter;
}

exports.default = eventEmitterTask;
```

### 子プロセス

```js
const { exec } = require("child_process");

function childProcessTask() {
  return exec("date");
}
exports.default = childProcessTask;
```

### オブザーバ

```js
const { Observable } = require("rxjs");

function observableTask() {
  return Observable.of(1, 2, 3);
}

exports.default = observableTask;
```

## エラーファーストのコールバック

もしタスクから何も返す予定がなければ、エラーファーストのコールバックを使うのが良い。このコールバック関数には引数として`cb()`を渡す。

```js
function callbackTask(cb) {
  // works
  cb();
}

exports.default = callbackTask;
```

エラーファーストのコールバックを使用してエラーを gulp に示すには、唯一の引数として`Error`を指定する。

```js
function callbackError(cb) {
  // task
  cb(new Error("kaboom"));
}

exports.default = callbackError;
```

しかし他の API にコールバックを渡すこともできる。

```js
const fs = require("fs");
function passingCallback(cb) {
  fs.access("gulpfile.js", cb);
}

exports.default = passingCallback;
```

## 同期的でないタスクについて、async/promise の使用

同期的なタスクはサポートされていない。

タスクを Promise でラップする非同期関数としてタスクを定義することができる。

```js
cons fs = require("fs");

async function asyncAwaitTask() {
  const { version } = JSON.parse(fs.reaFileSync("package.json", "utf-8"));
  console.log(version);
  await Promise.resolve("some result");
}

exports.default = asyncAwaitTask;
```
