# ファイルI/O

`File`構造体はすでに開いたファイルを表します。(これはファイル記述子の
ラップです。) また、これに対して読み取りや書き込みの権限を要求できます。

ファイルI/Oには多くの失敗要素があるため, `File`のメソッドは
`Result<T, io::Error>`のエイリアスである`io::Result<T>`を返します。

これによって、ファイルI/Oのエラーを*明確*にできます。これのおかげで、
パスの失敗について、自身を持って処理できます。
