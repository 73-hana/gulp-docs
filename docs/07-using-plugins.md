# プラグインを使う

gulp のプラグインは Node トランスフォームストリーム（パイプラインの中にあるファイルに対する作業をカプセル化したもの）である。Gulp プラグインは`src()`と`dest()`の間に`.pipe()`メソッドを用いて追加される。

プラグインはそれぞれ小さな動作の集まりである。望む結果を得るためにそれぞれを組み合わせる必要がある。

```js
const { src, dest } = require("gulp");
const uglify = require("gulp-uglify");
const rename = require("gulp-rename");

exports.default = function () {
  return src("src/*.js")
    .pipe(uglify)
    .pipe(rename({ extname: ".min.js" }))
    .pipe(dest("output/"));
};
```

## 本当に必要か

全てのプラグインを使用する必要はない。代わりにモジュールやライブラリを使用することも可能。

```js
const { rollup } = require("rollup");

exports.default = async function () {
  const bundle = await rollup({
    input: "src/index.js",
  });
  return bundle.write({
    file: "output/bundle.js",
    format: "iife",
  });
};
```

プラグインはファイルを変換するものであるため、ファイルを変換する挙動以外の作業は Node モジュールやライブラリを使うのが良い。

```js
const del = require("delete");

exports.default = function (cb) {
  del(["output/*.js", cb], cb);
};
```

## 条件分岐の実装

プラグインはファイルタイプを認識できないため、ファイルタイプを認識するためのプラグインが別途必要になることがある。

```js
const { src, dest } = require("gulp");
const gulpif = require("gulp-if");
const uglify = require("gulp-uglify");

function isJavaScript(file) {
  return file.extname === ".js";
}

exports.default = function () {
  return src(["src/*.js", "src/*.css"])
    .pipe(gulpif(isJavaScript, uglify()))
    .pipe(dest("output/"));
};
```

## インラインプラグイン

インラインプラグインは一回限りのストリームである。

独自のプラグインを作成する代わりに、または必要な機能を追加する際にプラグインをフォークする場合に役立つ。

```js
const { src, dest } = require("gulp");
const uglify = require("uglify-js");
const through2 = require("through2");

exports.default = function () {
  return src("src/*.js")
    .pipe(
      through2.obj(function (file, _, cb) {
        if (file.isBuffer()) {
          const code = uglify.minigy(file.contents.toString());
          file.contents = Buffer.form(code.code);
        }
        cb(null, file);
      })
    )
    .pipe(dest("output/"));
};
```
