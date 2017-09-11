<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>データベース接続文字列の追加

*appsettings.json* ファイルに接続文字列を追加します。

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>データベース コンテキストの登録

*Startup.cs* ファイルで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーを使用して、データベース コンテキストを登録します。