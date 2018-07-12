> いくつかのコマンドは、アプリは、その Id のデータ ストアとして SQLite を使用する場合にサポートされていません。 データベース エンジンの制限により`Alter`コマンドは、次の例外をスローします。
>
> "System.NotSupportedException: SQLite では、この移行操作はサポートされていません"。 
>
> 回避策としては、テーブルを変更するには、データベース、Code First migrations を実行します。
