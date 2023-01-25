# ファイル操作

`src()`メソッドと`dest()`メソッドはコンピュータ上にあるファイルを操作するためのメソッドである。

`src()`はノードストリームを生成する。生成されたストリームはタスクから返される必要がある。

```js
const { src, dest } = require("gulp");

exports.default = function () {
  return src("src/*.js").pipe(dest("output/"));
};
```

ストリームの API として、変換ストリームや書込可能ストリームをチェーンするための`.pipe()`メソッドが用意されている。

```js
const { src, dest } = require("gulp");
const babel = require("gulp-babel");

exports.default = function () {
  return src("src/*.js").pipe(babel()).pipe(dest("output/"));
};
```

`dest()`には引数として出力ディレクトリが渡される。`dest()`はターミネーターストリームとしてノードストリームも生成する。

パイプラインを介して渡されたファイルを受信すると、内容とその他の詳細を特定のディレクトリのファイルシステムに書き込む。

ほとんどのプラグインは`.pipe()`を用いて`src()`と`dest()`をつないでいる。

## ファイルをストリームに追加する

`src()`メソッドは、パイプラインの途中で利用することで、他のファイルをストリームに追加することができる。その`src()`の引数としてグロブを渡す事で実行できる。

この機能は、難読化を行う前にトランスパイルを行うようなタイミングで役立つ。

```js
const { src, dest } = require("gulp");
const babel = require("gulp-babel");
const uglify = require("gulp-uglify");

exports.default = function () {
  return src("src/*.js")
    .pipe(babel())
    .pipe(src("vendor/*.js"))
    .pipe(uglify())
    .pipe(dest("output/"));
};
```

## 出力

`dest()`はパイプラインの途中で利用することで、変更途中の状態をファイルシステムに出力することができる。

この機能は難読化する前と後のファイルを出力したいとき、最小化したファイルとしなかったファイルを 2 つとも出力したい場合に最適である。

```js
const { src, dest } = require("gulp");
const babel = require("gulp-babel");
const uglify = require("gulp-uglify");
const rename = require("gulp-rename");

exports.default = function () {
  return src("src/*.js")
    .pipe(babel())
    .pipe(src("vendor/*.js"))
    .pipe(dest("output/"))
    .pipe(uglify())
    .pipe(rename({ extname: ".min.js" }))
    .pipe(dest("output/"));
};
```

## ストリーム、バッファー、空っぽ

`src()`は 3 つのモードで利用できる。バッファリング、ストリーミング、空の 3 つである。

バッファリングモードはデフォルトであり、ファイルの内容をメモリに読みだす。プラグインは基本的にバッファリングモードでファイルを運用する。

ストリーミングモードは巨大な画像や動画など、メモリに収まらないようなファイルを操作するために存在する。コンテンツは一度にロードされるのではなく、ファイルシステムから小さなチャンクでストリーミングされる。

空モードはファイルのメタデータを利用したいときに使う。
