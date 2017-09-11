<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="81d76-101">スキャフォールディング ツールの追加と初期移行の実行</span><span class="sxs-lookup"><span data-stu-id="81d76-101">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="81d76-102">コマンド ラインから、次の .NET Core CLI コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="81d76-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="81d76-103">`add package` コマンドでスキャフォールディング エンジンを実行するために必要なツールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="81d76-103">The `add package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="81d76-104">`ef migrations add InitialCreate` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="81d76-104">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="81d76-105">このスキーマは、`DbContext` で指定されたモデルに基づきます (*Models/MovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="81d76-105">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="81d76-106">`Initial` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="81d76-106">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="81d76-107">任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="81d76-107">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="81d76-108">詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="81d76-108">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="81d76-109">`ef database update` コマンドは、データベースを作成する、*Migrations/\<time-stamp>_InitialCreate.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="81d76-109">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>