# <a name="scaffolded-razor-pages-in-aspnet-core"></a>ASP.NET Core でスキャフォールディングされた Razor ページ

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、前のチュートリアルでスキャフォールディングによって作成された Razor ページについて説明します。 

サンプルを[表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)します。

## <a name="the-create-delete-details-and-edit-pages"></a>[作成]、[削除]、[詳細]、および [編集] ページ

次のように、*Pages/Movies/Index.cshtml.cs* ページ モデルを確認します。

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

Razor ページは `PageModel` から派生します。 慣例により、`PageModel` 派生クラスは `<PageName>Model` と呼ばれます。 コンストラクターは[依存性の注入](xref:fundamentals/dependency-injection)を使用して、`MovieContext` をページに追加します。 スキャフォールディングされたページではすべてこのパターンに従います。 Entity Framework での非同期プログラミングの詳細については、「[非同期コード](xref:data/ef-rp/intro#asynchronous-code)」を参照してください。

ページに対して要求が行われると、`OnGetAsync` メソッドは Razor ページにムービーのリストを返します。 Razor ページで `OnGetAsync` または `OnGet` が呼び出され、ページの状態が初期化されます。 この場合、`OnGetAsync` でムービーのリストを取得し、表示します。

`OnGet` が `void` を返す場合、または `OnGetAsync` が `Task` を返す場合、return メソッドは使用されません。 戻り値の型が `IActionResult` または `Task<IActionResult>` の場合は、return ステートメントを指定する必要があります。 たとえば、*Pages/Movies/Create.cshtml.cs* `OnPostAsync` メソッドでは、次のようになります。

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

次のように、*Pages/Movies/Index.cshtml* Razor ページを確認します。

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor では、HTML から C# または Razor 固有のマークアップに移行できます。 `@` シンボルの後に [Razor で予約済みのキーワード](xref:mvc/views/razor#razor-reserved-keywords)が続いている場合は、Razor 固有のマークアップに移行します。それ以外の場合は、C# に移行します。

`@page` Razor ディレクティブはファイルを MVC アクションに分割します。これは、要求を処理できることを意味します。 `@page` はページで最初の Razor ディレクティブである必要があります。 `@page` は、Razor 固有のマークアップへの移行の例です。 詳細については、「[Razor syntax](xref:mvc/views/razor#razor-syntax)」 (Razor の構文) を参照してください。

次の HTML ヘルパーで使用されるラムダ式を確認します。

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

`DisplayNameFor` HTML ヘルパーは、ラムダ式で参照される `Title` プロパティを検査し、表示名を判別します。 ラムダ式は評価されるのではなく検査されます。 これは、`model`、`model.Movie`、または `model.Movie[0]` が `null` または空である場合、アクセス違反がないことを意味します。 ラムダ式が (`@Html.DisplayFor(modelItem => item.Title)` などを使用して) 評価される場合は、モデルのプロパティ値が評価されます。

<a name="md"></a>
### <a name="the-model-directive"></a>@model ディレクティブ

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` ディレクティブは、Razor ページに渡されるモデルの型を指定します。 前の例の `@model` 行は、Razor ページで `PageModel` 派生クラスを使用できるようにします。 モデルは、ページの `@Html.DisplayNameFor` および `@Html.DisplayName` [HTML ヘルパーで](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)使用されます。

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData とレイアウト

次のコードがあるとします。

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

上の強調表示されたコードは、Razor の C# への移行例です。 `{` 文字と `}` 文字で C# コードのブロックを囲みます。

`PageModel` 基本クラスには `ViewData` 辞書プロパティがあり、これを使用して、ビューに渡すデータを追加できます。 キー/値のパターンを使用して、`ViewData` 辞書にオブジェクトを追加します。 上のサンプルでは、"Title" プロパティが `ViewData` 辞書に追加されます。 

::: moniker range="= aspnetcore-2.0"

"Title" プロパティは *Pages/Shared/_Layout.cshtml* ファイルで使用されます。 次のマークアップは、*Pages/Shared/_Layout.cshtml* ファイルの最初の数行を示しています。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

"Title" プロパティは *Pages/Shared/_Layout.cshtml* ファイルで使用されます。 次のマークアップは、*_Layout.cshtml* ファイルの最初の数行を示しています。

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

`@*Markup removed for brevity.*@` 行は Razor コメントです。 HTML コメント (`<!-- -->`) とは異なり、Razor コメントはクライアントには送信されません。

アプリを実行して、プロジェクトのリンク (**[ホーム]**、**[バージョン情報]**、**[連絡先]**、**[作成]**、**[編集]**、および **[削除]**) をテストします。 各ページで、ブラウザー タブで表示できるタイトルを設定します。ページをブックマークすると、ブックマークでタイトルが使用されます。 現在、*Pages/Index.cshtml* と *Pages/Movies/Index.cshtml* のタイトルは同じですが、変更して別の値を指定することができます。

> [!NOTE]
> `Price` フィールドに小数点のコンマを入力できない場合があります。 小数点にコンマ (",") を使い、英語 (米国) 以外の日付形式を使う英語以外のロケールの [jQuery 検証](https://jqueryvalidation.org/)をサポートするには、アプリをグローバル化する手順を行う必要があります。 小数点のコンマの追加方法については、[こちらの GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) を参照してください。

`Layout` プロパティは *Pages/_ViewStart.cshtml* ファイルで設定されています。

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

上のマークアップでは、*Pages* フォルダーの下のすべての Razor ファイルに対して、レイアウト ファイルを *Pages/Shared/_Layout.cshtml* に設定します。 詳細については、「[Layout](xref:razor-pages/index#layout)」 (レイアウト) を参照してください。

### <a name="update-the-layout"></a>レイアウトの更新

より短い文字列を使用するために、*Pages/Shared/_Layout.cshtml* ファイルの `<title>` 要素を変更します。

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

*Pages/_Layout.cshtml* ファイルで次のアンカー要素を見つけます。

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
上の要素を次のマークアップに置き換えます。

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

上のアンカー要素は[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。 この場合は、[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)です。 `asp-page="/Movies/Index"` タグ ヘルパー属性と値で、`/Movies/Index` Razor ページへのリンクが作成されます。

変更内容を保存し、**RpMovie** リンクをクリックしてアプリをテストします。 GitHub の [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) ファイルを参照してください。

### <a name="the-create-page-model"></a>Create ページのモデル

*Pages/Movies/Create.cshtml.cs* ページ モデルを確認します。

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]

::: moniker-end


`OnGet` メソッドは、ページに必要な状態を初期化します。 [作成] ページには初期化する状態はないため、`Page` が返されます。 このチュートリアルで後ほど、`OnGet` メソッドの状態の初期化を確認できます。 `Page` メソッドでは、*Create.cshtml* ページをレンダリングする `PageResult` オブジェクトが作成されます。

`Movie` プロパティは `[BindProperty]` 属性を使用して、[モデル バインド](xref:mvc/models/model-binding)にオプトインします。 [作成] フォームでフォーム値が投稿されると、ASP.NET Core ランタイムが投稿された値を `Movie` モデルにバインドします。

`OnPostAsync` メソッドは、ページでフォーム データが投稿されたときに実行されます。

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

モデル エラーがある場合は、投稿されたフォーム データと共にフォームが再表示されます。 ほとんどのモデル エラーは、フォームが投稿される前にクライアント側でキャッチできます。 モデル エラーの例では、日付に変換できない日付フィールドの値が投稿されています。 クライアント側の検証とモデルの検証については、チュートリアルの後半で詳しく説明します。

モデル エラーがない場合、データは保存され、ブラウザーはインデックス ページにリダイレクトされます。

### <a name="the-create-razor-page"></a>[作成] Razor ページ

次のように、*Pages/Movies/Create.cshtml* Razor ページ ファイルを確認します。

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
