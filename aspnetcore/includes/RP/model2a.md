<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="cb9ed-101">データベース接続文字列の追加</span><span class="sxs-lookup"><span data-stu-id="cb9ed-101">Add a database connection string</span></span>

<span data-ttu-id="cb9ed-102">*appsettings.json* ファイルに接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="cb9ed-102">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="cb9ed-103">データベース コンテキストの登録</span><span class="sxs-lookup"><span data-stu-id="cb9ed-103">Register the database context</span></span>

<span data-ttu-id="cb9ed-104">*Startup.cs* ファイルで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーを使用して、データベース コンテキストを登録します。</span><span class="sxs-lookup"><span data-stu-id="cb9ed-104">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>
