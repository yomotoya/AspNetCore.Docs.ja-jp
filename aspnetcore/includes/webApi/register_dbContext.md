## <a name="register-the-database-context"></a><span data-ttu-id="cccc9-101">データベース コンテキストの登録</span><span class="sxs-lookup"><span data-stu-id="cccc9-101">Register the database context</span></span>

<span data-ttu-id="cccc9-102">コントローラーにデータベース コンテキストを挿入するには、[依存性の注入](xref:fundamentals/dependency-injection)コンテナーに登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cccc9-102">In order to inject the database context into the controller, we need to register it with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="cccc9-103">データベース コンテキストをサービス コンテナーに登録するには、[依存性の注入](xref:fundamentals/dependency-injection)の組み込みサポートを使用します。</span><span class="sxs-lookup"><span data-stu-id="cccc9-103">Register the database context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="cccc9-104">*Startup.cs* ファイルの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="cccc9-104">Replace the contents of the *Startup.cs* file with the following:</span></span>

<span data-ttu-id="cccc9-105">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]</span><span class="sxs-lookup"><span data-stu-id="cccc9-105">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]</span></span>

<span data-ttu-id="cccc9-106">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="cccc9-106">The preceding code:</span></span>

* <span data-ttu-id="cccc9-107">使用していないコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="cccc9-107">Removes the code we're not using.</span></span>
* <span data-ttu-id="cccc9-108">メモリ内データベースがサービス コンテナーに挿入されるように指定します。</span><span class="sxs-lookup"><span data-stu-id="cccc9-108">Specifies an in-memory database is injected into the service container.</span></span>
