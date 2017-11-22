## <a name="register-the-database-context"></a>データベース コンテキストの登録

この手順では、[依存性の注入](xref:fundamentals/dependency-injection)コンテナーにデータベース コンテキストを登録します。 サービス (DB コンテキストなど) を依存性の注入 (DI) コンテナーに登録すると、コントローラーで使用できるようになります。

DB コンテキストをサービス コンテナーに登録するには、[依存性の注入](xref:fundamentals/dependency-injection)の組み込みサポートを使用します。 *Startup.cs* ファイルの内容を次のコードに置き換えます。

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

上のコードでは以下の操作が行われます。

* 使用されていないコードが削除されます。
* メモリ内データベースがサービス コンテナーに挿入されるように指定します。