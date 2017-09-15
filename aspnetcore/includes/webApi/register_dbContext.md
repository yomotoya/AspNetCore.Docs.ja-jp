## <a name="register-the-database-context"></a>データベース コンテキストの登録

コントローラーにデータベース コンテキストを挿入するには、[依存性の注入](xref:fundamentals/dependency-injection)コンテナーに登録する必要があります。 データベース コンテキストをサービス コンテナーに登録するには、[依存性の注入](xref:fundamentals/dependency-injection)の組み込みサポートを使用します。 *Startup.cs* ファイルの内容を次のコードに置き換えます。

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

上のコードでは以下の操作が行われます。

* 使用していないコードを削除します。
* メモリ内データベースがサービス コンテナーに挿入されるように指定します。
