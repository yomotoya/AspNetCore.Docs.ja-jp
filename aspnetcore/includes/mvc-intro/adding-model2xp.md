<a name="cli"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="1ac4c-101">最初の移行の実行</span><span class="sxs-lookup"><span data-stu-id="1ac4c-101">Perform initial migration</span></span>

<span data-ttu-id="1ac4c-102">コマンド ラインから、次の .NET Core CLI コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1ac4c-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="1ac4c-103">`dotnet ef migrations add InitialCreate` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="1ac4c-103">The `dotnet ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="1ac4c-104">このスキーマは、`DbContext` で指定されたモデルに基づきます (*Models/MovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="1ac4c-104">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="1ac4c-105">`Initial` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="1ac4c-105">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="1ac4c-106">任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="1ac4c-106">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="1ac4c-107">詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1ac4c-107">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="1ac4c-108">`dotnet ef database update` コマンドは、データベースを作成する、*Migrations/\<time-stamp>_InitialCreate.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1ac4c-108">The `dotnet ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>