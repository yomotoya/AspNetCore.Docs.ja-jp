## <a name="register-the-database-context"></a><span data-ttu-id="2ead9-101">データベース コンテキストの登録</span><span class="sxs-lookup"><span data-stu-id="2ead9-101">Register the database context</span></span>

<span data-ttu-id="2ead9-102">この手順では、[依存性の注入](xref:fundamentals/dependency-injection)コンテナーにデータベース コンテキストを登録します。</span><span class="sxs-lookup"><span data-stu-id="2ead9-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="2ead9-103">サービス (DB コンテキストなど) を依存性の注入 (DI) コンテナーに登録すると、コントローラーで使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="2ead9-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="2ead9-104">DB コンテキストをサービス コンテナーに登録するには、[依存性の注入](xref:fundamentals/dependency-injection)の組み込みサポートを使用します。</span><span class="sxs-lookup"><span data-stu-id="2ead9-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="2ead9-105">*Startup.cs* ファイルの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2ead9-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="2ead9-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span><span class="sxs-lookup"><span data-stu-id="2ead9-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="2ead9-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span><span class="sxs-lookup"><span data-stu-id="2ead9-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span></span>
::: moniker-end

<span data-ttu-id="2ead9-108">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="2ead9-108">The preceding code:</span></span>

* <span data-ttu-id="2ead9-109">未使用のコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="2ead9-109">Removes the unused code.</span></span>
* <span data-ttu-id="2ead9-110">メモリ内データベースがサービス コンテナーに挿入されるように指定します。</span><span class="sxs-lookup"><span data-stu-id="2ead9-110">Specifies an in-memory database is injected into the service container.</span></span>