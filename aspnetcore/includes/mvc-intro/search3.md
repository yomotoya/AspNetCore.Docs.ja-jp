<!--
[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

ここで検索を送信すると、URL に検索クエリ文字列が含まれます。 `HttpPost Index` メソッドがある場合でも、検索時には `HttpGet Index` アクション メソッドにも移動します。

![URL に searchString=ghost が表示されたブラウザー ウィンドウ。返された Ghostbusters および Ghostbusters 2 というムービーには ghost という単語が含まれています](../../tutorials/first-mvc-app/search/_static/search_get.png)

次のマークアップは `form` タグの変更を示しています。

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a>ジャンルによる検索の追加

次の `MovieGenreViewModel` クラスを *Models* フォルダーに追加します。

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

ムービージャンルのビュー モデルには以下が含まれます。

   * ムービーのリスト。
   * ジャンルのリストを含む `SelectList`。 これにより、ユーザーはリストからジャンルを選択できます。
   * 選択されたジャンルを含む、`movieGenre`。

`MoviesController.cs` の `Index` メソッドを次のコードに置き換えます。

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

次のコードは、データベースからすべてのジャンルを取得する `LINQ` クエリです。

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

ジャンルの `SelectList` は、個々のジャンルを投影して作成します (選択リストでジャンルが重複しないようにします)。

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a>インデックス ビューへのジャンルによる検索の追加

次のように `Index.cshtml` を更新します。

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

次の HTML ヘルパーで使用されるラムダ式を確認します。

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
上のコードでは、`DisplayNameFor` HTML ヘルパーは、ラムダ式で参照される `Title` プロパティを検査し、表示名を判別します。 ラムダ式は評価されるのではなく、検査されるため、`model`、`model.movies`、または `model.movies[0]` が `null` または空である場合にアクセス違反が発生することはありません。 ラムダ式が評価される場合 (`@Html.DisplayFor(modelItem => item.Title)` など)、モデルのプロパティ値が評価されます。

ジャンルまたはムービーのタイトル、あるいはその両方で検索して、アプリをテストします。
