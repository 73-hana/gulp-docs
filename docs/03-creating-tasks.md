# タスクの作成

gulp タスクはそれぞれ JS の非同期関数である。非同期関数はブラウザによってはサポートされていない可能性があるため、いくつかの代替案がある。

## エクスポート

タスクはパブリックかプライベートかに分かれる。

- public は gulpfile からエクスポートされるタスクである。これは gulp コマンドで実行される
- private は内部で実行されるために作成されるタスクである。

```js
const { series } = require("gulp");

// 下記のclean関数はエクスポートされていないのでプライベートタスクとして扱われる
function clean(cb) {
  cb();
}

// 反対にbuild関数はエクスポートされているのでパブリックタスクとして扱われる
// この関数はgulpコマンドで実行される
function build(cb) {
  cb();
}

exports.build = build;
exports.default = series(clean, build);
```

## タスクの構成

gulp には`series()`メソッドと`parallel()`メソッドが用意されていて、それは個別のタスクを大きなひとまとまりとして構築するために使用されるメソッドである。

タスクを順序良く管理するには`series()`メソッドを使う。

```js
const { series } = require("gulp");

function transpile(cb) {
  cb();
}

function bundle(cb) {
  cb();
}

exports.build = series(transpile, bundle);
```

タスクを出来る限り同時処理したい場合`parallel()`メソッドを使う。

```js
const { parallel } = require("gulp");

function javascript(cb) {
  cb();
}

function css(cb) {
  cb();
}

exports.build = parallel(javascript, css);
```

`series()`メソッドや`parallel()`メソッドを呼び出すとタスクがすぐさま構築される。なので、条件分岐でバリエーションを作ることができる。

```js
const { series } = require("gulp");

// ~~~

if (process.env.NODE_ENV === "production") {
  exports.build = series(transpile, minify);
} else {
  exports.build = series(transpile, livereload);
}
```

`series()`と`parallel()`はネストすることもできる。

```js
const { series, parallel } = require("gulp");

// ~~~

exports.build = series(
  clean,
  parallel(cssTranspile, series(jsTranspile, jsBundle)),
  parallel(cssMinify, jsMinify),
  publish
);
```

合成されたタスク（オペレーション）が実行されると、各タスクは参照されるタイミングで実行される。
