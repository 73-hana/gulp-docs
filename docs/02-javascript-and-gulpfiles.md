# JavaScript and Gulpfiles

Gulp では JS の知識を用いて Gulpfiles を作成することができ、また Gulpfiles の経験を生かして JS ファイルを作成することができる。
一部の機能はファイルシステムやコマンドラインを必要とするが、基本は全て JS で記述されている。

## Gulpfile について

gulpfile とはプロジェクトディレクトリにある`gulpfile.js`と名づけられたファイルである。このファイルは`gulp`コマンドが実行されると自動的に読み込まれる。gulp API がいくつか用意されてはいるが、もちろんファイル内で JS や Node モジュールを利用することもできる。

gulp のタスクシステムにタスクを登録するには、gulpfile で関数をエクスポートする。

## トランスパイル

TypeScript や Babel などのトランスパイルが必要なプログラミング言語を用いて gulpfile を記述することも可能である。

- gulpefile.ts
- gulpfile.babel.js

## Gulpfile の分割

Gulpfile が大きくなった場合は別のファイルに分割することができる。各タスクを別ファイルに分割し、Gulpfile にインポートして合成する。

gulpfile.js というファイルを gulpfile.js というディレクトリに変更することもできる。gulpfile.js というフォルダには index.js というファイル含んでいる。このディレクトリは独自のモジュールやタスクが含まれている。
