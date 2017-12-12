# <a name="adding-search-to-an-aspnet-core-mvc-app"></a>ASP.NET Core MVC アプリへの検索の追加

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このセクションでは、検索機能を `Index` アクション メソッドに追加して、*ジャンル*または*名前*でムービーを検索できるようにします。

`Index` メソッドを次のコードで更新します。
<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

`Index` アクション メソッドの最初の行により、ムービーを選択する [LINQ](https://docs.microsoft.com/dotnet/standard/using-linq) クエリが作成されます。

```csharp
var movies = from m in _context.Movie
             select m;
```

このクエリはこの時点で*のみ*定義されます。データベースに対して**実行されていません**。

`searchString` パラメーターに文字列が含まれる場合、検索文字列の値でフィルターするようにムービー クエリが変更されます。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

上の `s => s.Title.Contains()` コードは[ラムダ式](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)です。 ラムダは、メソッド ベースの [LINQ](https://docs.microsoft.com/dotnet/standard/using-linq) クエリで、[Where](https://docs.microsoft.com//dotnet/api/system.linq.enumerable.where) メソッドや `Contains` (上のコードで使用されている) など、標準クエリ演算子メソッドの引数として使用されます。 LINQ クエリは、`Where`、`Contains`、`OrderBy` などのメソッドの呼び出しで定義または変更されたときには実行されません。 クエリ実行は先送りされます。  つまり、その具体値が実際に繰り返されるか、`ToListAsync` メソッドが呼び出されるまで、式の評価が延期されます。 クエリの遅延実行の詳細については、「[クエリの実行](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution)」を参照してください。

注: [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) メソッドは、上記の C# コードではなく、データベースで実行されます。 クエリの大文字と小文字の区別は、データベースや照合順序に依存します。 SQL Server では、[Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) は大文字と小文字の区別がない [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql) にマッピングされます。 SQLlite の場合、既定の照合順序で、大文字と小文字が区別されます。

`/Movies/Index` に移動します。 `?searchString=Ghost` などのクエリ文字列を URL に追加します。 フィルターされたムービーが表示されます。

![インデックス ビュー](../../tutorials/first-mvc-app/search/_static/ghost.png)

`id` という名前のパラメーターを使用するために `Index` メソッドの署名を変更すると、`id` パラメーターは、*Startup.cs* で設定されている既定ルートの省略可能な `{id}` プレースホルダーと一致するようになります。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
