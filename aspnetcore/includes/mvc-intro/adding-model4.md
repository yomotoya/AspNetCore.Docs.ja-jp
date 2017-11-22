上のコードで強調表示されている部分は、(*Startup.cs* ファイルの) [依存性の注入](xref:fundamentals/dependency-injection)コンテナーに追加されているムービー データベース コンテキストを示しています。 `services.AddDbContext<MvcMovieContext>(options =>` では、使用するデータベースと接続文字列を指定します。 `=>` は[ラムダ演算子](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/operators/lambda-operator)です。

*Controllers/MoviesController.cs* ファイルを開いて、コンストラクターを調べます。

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

コンストラクターでは、[依存性の注入](xref:fundamentals/dependency-injection)を使ってデータベース コンテキスト (`MvcMovieContext `) がコントローラーに挿入されています。 データベース コンテキストは、コントローラーの各 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) メソッドで使用されます。

<a name="strongly-typed-models-keyword-label"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>厳密に型指定されたモデルと @model キーワード

コントローラーで `ViewData` ディクショナリを使ってビューにデータまたはオブジェクトを渡す方法を前に示しました。 `ViewData` ディクショナリは動的オブジェクトであり、ビューに情報を渡すための便利な遅延バインディングの方法を提供します。

MVC にも、厳密に型指定されたモデル オブジェクトをビューに渡す機能があります。 この厳密に型指定された方法を使うと、コンパイル時のコードのチェックが向上します。 スキャフォールディング メカニズムは、メソッドとビューを作成するときに、`MoviesController` クラスとビューでこの方法 (つまり、厳密に型指定されたモデルを渡すこと) を使いました。

*Controllers/MoviesController.cs* ファイルで生成された `Details` メソッドを調べてください。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

通常、`id` パラメーターはルート データとして渡されます。 たとえば、`http://localhost:5000/movies/details/1` は次のように設定します。

* コントローラーを `movies` コントローラーに (最初の URL セグメント)。
* アクションを `details` に (2 番目の URL セグメント)。
* ID を 1 に (最後の URL セグメント)。

次のようにクエリ文字列で `id` を渡すこともできます。

`http://localhost:1234/movies/details?id=1`

ID 値が指定されない場合、`id` パラメーターは [null 許容型](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) として定義されます。

ルート データまたはクエリ文字列の値と一致するムービー エンティティを選択するため、[ラムダ式](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions)が `SingleOrDefaultAsync` に渡されます。

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```

ムービーが見つかった場合、`Movie` モデルのインスタンスが `Details` ビューに渡されます。

```csharp
return View(movie);
   ```

*Views/Movies/Details.cshtml* ファイルの内容を確認してください。

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

ビュー ファイルの先頭に `@model` ステートメントを含めることで、ビューが期待するオブジェクトの型を指定することができます。 ムービー コントローラーを作成したとき、Visual Studio によって *Details.cshtml* ファイルの先頭に `@model` ステートメントが自動的に追加されています。

```HTML
@model MvcMovie.Models.Movie
   ```

この `@model` ディレクティブにより、厳密に型指定された `Model` オブジェクトを使って、コントローラーがビューに渡したムービーにアクセスできます。 たとえば、*Details.cshtml* ビューでは、コードで厳密に型指定された `Model` オブジェクトを使って、`DisplayNameFor` および `DisplayFor` HTML ヘルパーに各ムービー フィールドを渡しています。 `Create` および `Edit` のメソッドとビューも、`Movie` モデル オブジェクトを渡します。

Movies コントローラーの *Index.cshtml* ビューと `Index` メソッドを確認してください。 コードで `View` メソッドを呼び出すときの `List` オブジェクトの作成方法に注意してください。 コードでは、この `Movies` リストを `Index` アクション メソッドからビューに渡しています。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

ムービー コントローラーを作成したとき、スキャフォールディングによって *Index.cshtml* ファイルの先頭に `@model` ステートメントが自動的に追加されています。

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

`@model` ディレクティブにより、厳密に型指定された `Model` オブジェクトを使って、コントローラーがビューに渡したムービーのリストにアクセスできます。 たとえば、*Index.cshtml* ビューのコードでは、`foreach` ステートメントを使って厳密に型指定された `Model` オブジェクトのムービーをループ処理しています。

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

`Model` オブジェクトは厳密に型指定されているので (`IEnumerable<Movie>` オブジェクトとして)、ループ内の各項目は `Movie` として型指定されます。 それ以外の利点としては、コンパイル時にコードのチェックが行われます。
