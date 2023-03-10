# グロブの説明

グロブは文字列とワイルドカードの塊である。これはファイルパスの指定に用いられる。グロビングとはグロブを用いてファイルを指定する動作のことである。

`src()`メソッドは引数として、1 つのグロブを文字列で受け取るか、もしくは複数のグロブを配列で受け取ることができる。ファイルを特定できない場合はエラーが発生する。

## セグメントとセパレータ

グロブ内のセパレータは OS に関係なく常に"/"である。"\\"はエスケープ文字になっている。

グロブを生成するために Node.js の path メソッドや`__dirname`を利用するのは避けるべきである。path メソッドで生成されるグロブのセパレータは OS によって異なるからである。

## \*

0 文字以上の任意の文字を表す。これはセグメントを超えて検索することはない。

## \*\*

0 文字以上の任意の文字を表す。これはセグメントを超えて検索する。

## !

配列内で使うことで、前の条件にヒットしたファイルから除外したいファイルの条件を付け足すことができる。後ろの条件からは削除できない。

特に\*\*のと併用すると効力を発揮する。
