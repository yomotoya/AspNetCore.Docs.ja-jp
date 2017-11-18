# <a name="working-with-sqlite-in-an-aspnet-core-mvc-project"></a><span data-ttu-id="35e25-101">ASP.NET Core MVC プロジェクトでの SQLite の操作</span><span class="sxs-lookup"><span data-stu-id="35e25-101">Working with SQLite in an ASP.NET Core MVC project</span></span>

<span data-ttu-id="35e25-102">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="35e25-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="35e25-103">`MvcMovieContext` オブジェクトは、データベースへの接続と、データベース レコードへの `Movie` オブジェクトのマッピングのタスクを処理します。</span><span class="sxs-lookup"><span data-stu-id="35e25-103">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="35e25-104">データベース コンテキストは、*Startup.cs* ファイルの `ConfigureServices` メソッドで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーに登録されます。</span><span class="sxs-lookup"><span data-stu-id="35e25-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a><span data-ttu-id="35e25-105">SQLite</span><span class="sxs-lookup"><span data-stu-id="35e25-105">SQLite</span></span>

<span data-ttu-id="35e25-106">[SQLite](https://www.sqlite.org/) の Web サイトは次のようなものです。</span><span class="sxs-lookup"><span data-stu-id="35e25-106">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="35e25-107">SQLite は埋め込みの全機能を備えた、自己完結型かつ高信頼性でパブリック ドメインの SQL データベース エンジンです。</span><span class="sxs-lookup"><span data-stu-id="35e25-107">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="35e25-108">SQLite は、世界で最も使用されているデータベース エンジンです。</span><span class="sxs-lookup"><span data-stu-id="35e25-108">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="35e25-109">SQLite データベースを管理および表示するためにダウンロードできるサードパーティ製ツールは多数あります。</span><span class="sxs-lookup"><span data-stu-id="35e25-109">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="35e25-110">以下の画像は [DB Browser for SQLite](http://sqlitebrowser.org/) のものです。</span><span class="sxs-lookup"><span data-stu-id="35e25-110">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="35e25-111">お気に入りの SQLite ツールがございましたら、そのツールの気に入っている点についてコメントしてください。</span><span class="sxs-lookup"><span data-stu-id="35e25-111">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![movie db を示している DB Browser for SQLite](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="35e25-113">データベースのシード</span><span class="sxs-lookup"><span data-stu-id="35e25-113">Seed the database</span></span>

<span data-ttu-id="35e25-114">*Models* フォルダーに `SeedData` という名前の新しいクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="35e25-114">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="35e25-115">生成されたコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="35e25-115">Replace the generated code with the following:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="35e25-116">DB にムービーがある場合、シード初期化子が返されます。</span><span class="sxs-lookup"><span data-stu-id="35e25-116">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="35e25-117">シード初期化子の追加</span><span class="sxs-lookup"><span data-stu-id="35e25-117">Add the seed initializer</span></span>

<span data-ttu-id="35e25-118">次のように、*Program.cs* ファイルで `Main` メソッドにシード初期化子を追加します。</span><span class="sxs-lookup"><span data-stu-id="35e25-118">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]

### <a name="test-the-app"></a><span data-ttu-id="35e25-119">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="35e25-119">Test the app</span></span>

<span data-ttu-id="35e25-120">DB 内のすべてのレコードを削除します (そのため Seed メソッドが実行されます)。</span><span class="sxs-lookup"><span data-stu-id="35e25-120">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="35e25-121">アプリを停止および起動して、データベースをシードします。</span><span class="sxs-lookup"><span data-stu-id="35e25-121">Stop and start the app to seed the database.</span></span>
   
<span data-ttu-id="35e25-122">アプリにシードされたデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="35e25-122">The app shows the seeded data.</span></span>

![ムービー データが表示された、MVC Movie アプリケーションが開いているブラウザー](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)
