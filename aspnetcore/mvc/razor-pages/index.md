---
title: "ASP.NET Core での Razor ページの概要"
author: Rick-Anderson
description: "このドキュメントは、ページに重点を置いたシナリオの開発を容易にするために、ASP.NET Core での Razor ページを使用するための概要を提供します。"
keywords: "ASP.NET Core、Razor ページ"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: e9678279db85ec03616e693b9772c6ee71c4fef8
ms.sourcegitcommit: d2f705f7a8ef2c1a940f590e4de188621fd48d2a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a>ASP.NET Core での Razor ページの概要

[Rick Anderson](https://twitter.com/RickAndMSFT) および [Ryan Nowak](https://github.com/rynowak) 著

Razor ページは、ページ コーディングに重点を置いたシナリオをより簡略化し、生産性を高める ASP.NET Core MVC の新機能です。

モデル ビュー コントローラーのアプローチを使用するチュートリアルをお探しの場合は、「[Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)」 (ASP.NET Core MVC の概要) を参照してください。

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a>ASP.NET Core 2.0 の前提条件

[.NET Core](https://www.microsoft.com/net/core) 2.0.0 以降をインストールします。

Visual Studio を使用している場合は、以下のワークロードで [Visual Studio](https://www.visualstudio.com/vs/) 2017 Version 15.3 以降をインストールします。

* **ASP.NET と Web 開発**
* **.NET Core クロスプラットフォームの開発**

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a>Razor ページ プロジェクトの作成

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Visual Studio を使用して Razor ページ プロジェクトを作成する詳細な手順については、「[Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start)」 (Razor ページの概要) を参照してください。

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

コマンド ラインから `dotnet new razor` を実行します。

Visual Studio for Mac から生成された *.csproj* ファイルを開きます。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

コマンド ラインから `dotnet new razor` を実行します。

#   <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

コマンド ラインから `dotnet new razor` を実行します。

---

## <a name="razor-pages"></a>Razor ページ

Razor ページは *Startup.cs* で有効になっています。

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

基本ページを検討します。<a name="OnGet"></a>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

上記のコードは Razor ビュー ファイルに非常によく似ています。 違いは、`@page` ディレクティブにあります。 `@page` はファイルを MVC アクションにします。つまり、コントローラーを経由せずに要求を直接処理します。 `@page` はページで最初の Razor ディレクティブである必要があります。 `@page` はその他の Razor コンストラクトの動作に影響します。

`PageModel` クラスを使用している類似したページが、次の 2 つのファイルにあります。 *Pages/Index2.cshtml* ファイル:

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

*Pages/Index2.cshtml.cs* "code-behind" ファイル:

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

規則により、`PageModel` クラス ファイルは、Razor ページ ファイルと同じ名前に *.cs* が付加された名前になります。 たとえば、上の Razor ページは *Pages/Index2.cshtml* になります。 `PageModel` クラスを含むファイル名は、*Pages/Index2.cshtml.cs* になります。

URL パスのページへの関連付けは、ファイル システム内のページの場所によって決定されます。 次の表に、Razor ページ パスと一致 URL を示します。

| ファイル名とパス               | 一致 URL |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/` または `/Index` |
| */Pages/Contact.cshtml* | `/Contact` |
| */Pages/Store/Contact.cshtml* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` または `/Store/Index` |

メモ:

* 既定では、ランタイムが *Pages* フォルダー内で Razor ページ ファイルを検索します。
* `Index` は、URL にページが含まれない場合の既定のページになります。

## <a name="writing-a-basic-form"></a>基本フォームの作成

Razor ページ機能は、Web ブラウザーで使用される一般的なパターンを簡単にするために設計されています。 [モデル バインド](xref:mvc/models/model-binding)、[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)、および HTML ヘルパーはすべて、Razor ページ クラスで定義されたプロパティで*機能します*。 `Contact` モデルの基本的な "お問い合わせ" フォームを実装するページを考察します。

このドキュメントのサンプルでは、[Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) ファイルで `DbContext` が初期化されます。

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

データ モデル:

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

*Pages/Create.cshtml* ビュー ファイル:

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

ビューの *Pages/Create.cshtml.cs* 分離コード ファイル:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

規則により、`PageModel` クラスは `<PageName>Model` と呼ばれ、ページと同じ名前空間にあります。

`PageModel` 分離コード ファイルを使用すると単体テストがサポートされますが、明示的なコンストラクターとクラスを記述する必要があります。 `PageModel` 分離コード ファイルのないページは、ランタイムのコンパイルをサポートします。これは開発で有利になる場合があります。  <!-- review: advantage because you can make changes and refresh the browser without explicitly compiling the app -->

このページには、(ユーザーがフォームを投稿したときに) `POST` 要求で実行される `OnPostAsync` *ハンドラー メソッド*があります。 任意の HTTP 動詞のハンドラー メソッドを追加できます。 最も一般的なハンドラーは次のとおりです。

* ページに必要な状態を初期化するための `OnGet`。 [OnGet](#OnGet) サンプル。
* フォームの送信を処理するための `OnPost`。

`Async` 名前付けサフィックスは省略可能ですが、非同期関数の規則でよく使用されます。 上記の例の `OnPostAsync` コードは、コントローラーで通常に記述するものに似ています。 上記のコードは、Razor ページでは一般的です。 [モデル バインド](xref:mvc/models/model-binding)、[検証](xref:mvc/models/validation)、およびアクションの結果などのほとんどの MVC プリミティブは共有されます。  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

上記の `OnPostAsync` メソッド:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

`OnPostAsync` の基本的な流れは次のとおりです。

検証エラーを確認します。

*  エラーがない場合は、データを保存し、リダイレクトします。
*  エラーがある場合は、検証メッセージとともにページをもう一度表示します。 クライアント側の検証は、従来の ASP.NET Core MVC アプリケーションと同じです。 多くの場合、検証エラーはクライアントで検出され、サーバーには送信されません。

データが正常に入力されると、`OnPostAsync` ハンドラー メソッドが `RedirectToPage` ヘルパー メソッドを呼び出して `RedirectToPageResult` のインスタンスを返します。 `RedirectToPage` は、`RedirectToAction` や `RedirectToRoute` と同じような新しいアクション結果ですが、ページ用にカスタマイズされています。 上記のサンプルでは、ルート インデックス ページ (`/Index`) にリダイレクトします。 `RedirectToPage` については、「[ページの URL の生成](#url_gen)」セクションで詳しく説明されています。

送信されたフォームに検証エラー (サーバーに渡される) があると、`OnPostAsync` ハンドラー メソッドが `Page` ヘルパー メソッドを呼び出します。 `Page` は `PageResult` のインスタンスを返します。 `Page` を返すのは、コントローラーのアクションが `View` を返す方法に似ています。 `PageResult` はハンドラー メソッドの既定の<!-- Review  -->戻り値の型です。 `void` を返すハンドラー メソッドがページをレンダリングします。

`Customer` プロパティは `[BindProperty]` 属性を使用してモデル バインドにオプトインします。

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

既定では、Razor ページはプロパティを非 GET 動詞とのみバインドします。 プロパティをバインドすることで、記述すべきコードの量を削減できます。 同じプロパティを使用してバインドすることでコードを減らし、フィールド (`<input asp-for="Customer.Name" />`) からレンダリングして入力を受け入れます。

ホーム ページ (*Index.cshtml*):

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

分離コード *Index.cshtml.cs* ファイル:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

*Index.cshtml* ファイルには、各連絡先の編集リンクを作成するために次のマークアップが含まれています。

```cshtml
<a asp-page="./Edit" asp-route-id="@contact.Id">edit</a>
```

[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)は [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper#route) 属性を使用して編集ページへのリンクを生成しました。 リンクには、連絡先 ID とともにルート データが含まれています。 たとえば、`http://localhost:5000/Edit/1` のようにします。

*Pages/Edit.cshtml* ファイル:

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

最初の行には `@page "{id:int}"` ディレクティブが含まれています。 ルーティングの制約 `"{id:int}"` は、`int` ルート データを含むページへの要求を受け入れるようにページに指示します。 ページへの要求に `int` に変換できるルート データが含まれていない場合は、ランタイムで HTTP 404 (見つかりません) エラーが返されます。

*Pages/Edit.cshtml.cs* ファイル:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/CSRF と Razor ページ

[偽造防止検証](xref:security/anti-request-forgery)のためにコードを記述する必要はありません。 偽造防止トークンの生成と検証は、自動的に Razor ページに含まれます。

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>Razor ページでのレイアウト、パーシャル、テンプレート、およびタグ ヘルパーの使用

ページは、Razor ビュー エンジンのすべての機能で機能します。 レイアウト、パーシャル、テンプレート、タグ ヘルパー、*_ViewStart.cshtml*、*_ViewImports.cshtml* は、従来の Razor ビューと同じように動作します。

これらの機能の一部を利用してこのページをまとめてみましょう。

[レイアウト ページ](xref:mvc/views/layout)を *Pages/_Layout.cshtml* に追加します。

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

[レイアウト](xref:mvc/views/layout)は次のことを行います。

* (ページでレイアウトを止めない限り) 各ページのレイアウトを制御します。
* JavaScript やスタイルシートなどの HTML 構造をインポートします。

詳細については、[レイアウトのページ](xref:mvc/views/layout)を参照してください。

[Layout](xref:mvc/views/layout#specifying-a-layout) プロパティは *Pages/_ViewStart.cshtml* で設定されています。

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

**注:** レイアウトは、*Pages* フォルダー内にあります。 ページは現在のページと同じフォルダーから開始して、階層的に他のビュー (レイアウト、テンプレート、パーシャル) を検索します。 *Pages* フォルダー内のレイアウトは、*Pages* フォルダー配下の任意の Razor ページから使用できます。

レイアウト ファイルを *Views/Shared* フォルダー内に配置**しない**ことをお勧めします。 *Views/Shared* は MVC ビュー パターンです。 Razor ページは、パス規則ではなく、フォルダー階層に依存することを意図しています。

Razor ページからのビュー検索には、*Pages* フォルダーが含まれます。 MVC コントローラーで使用しているレイアウト、テンプレート、およびパーシャルと、従来の Razor ビューは*機能します*。

*Pages/_ViewImports.cshtml* ファイルを追加します。

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace` はこのチュートリアルで後ほど説明します。 `@addTagHelper` ディレクティブにより、[組み込みタグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/Index)が *Pages* フォルダー内のすべてのページにもたらされます。

<a name="namespace"></a>

ページで `@namespace` ディレクティブが明示的に使用されている場合:

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

ディレクティブは、ページの名前空間を設定します。 `@model` ディレクティブには、名前空間を含める必要はありません。

`@namespace` ディレクティブが *_ViewImports.cshtml* に含まれていると、指定した名前空間が `@namespace` ディレクティブをインポートするページで生成された名前空間のプレフィックスを提供します。 生成された名前空間の残りの部分 (サフィックスの部分) は、*_ViewImports.cshtml* を含むフォルダーとページを含むフォルダー間のドットで区切られた相対パスです。

たとえば、分離コード ファイル *Pages/Customers/Edit.cshtml.cs* は名前空間を明示的に設定します。

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

*Pages/_ViewImports.cshtml* ファイルは次の名前空間を設定します。

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

*Pages/Customers/Edit.cshtml* Razor ページの生成された名前空間は、分離コード ファイルと同じです。 `@namespace` ディレクティブは、分離コード ファイルの `@using` ディレクティブを追加しなくても、プロジェクトに追加された C# クラスと、ページ生成したコードが*機能する*ように設計されました。

**注:** `@namespace`は従来の Razor ビューでも機能します。

元の *Pages/Create.cshtml* ビュー ファイル:

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

更新された *Pages/Create.cshtml* ビュー ファイル:

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

[Razor ページのスタート プロジェクト](#rpvs17)には、クライアント側の検証をフックする *Pages/_ValidationScriptsPartial.cshtml* が含まれています。

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>ページの URL の生成

上に示した `Create` ページでは、`RedirectToPage` を使用します。

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

アプリには次のファイル/フォルダー構造があります。

* */Pages*

  * *Index.cshtml*
  * */Customer*

    * *Create.cshtml*
    * *Edit.cshtml*
    * *Index.cshtml*

成功すると、*Pages/Customers/Create.cshtml* ページと *Pages/Customers/Edit.cshtml* ページが *Pages/Index.cshtml* にリダイレクトされます。 文字列 `/Index` は前のページにアクセスするための URI の一部です。 文字列 `/Index` は、*Pages/Index.cshtml* ページへの URI を生成するために使用できます。 例:

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

ページ名は、ルート */Pages* フォルダーからページへのパスです (たとえば `/Index` の場合、先頭の `/` を含みます)。 上記の URL の生成サンプルは、単に URL をハードコーディングするよりもはるかに多機能です。 URL の生成は[ルーティング](xref:mvc/controllers/routing)を使用し、ターゲット パスで定義されたルート方法に従って、パラメーターの生成とエンコードができます。

ページの URL 生成は、相対名をサポートします。 次の表に、*Pages/Customers/Create.cshtml* の異なる `RedirectToPage` パラメーターで選択されたインデックス ページを示します。

| RedirectToPage(x)| ページ |
| ----------------- | ------------ |
| RedirectToPage("/Index") | *Pages/Index* |
| RedirectToPage("./Index"); | *Pages/Customers/Index* |
| RedirectToPage("../Index") | *Pages/Index* |
| RedirectToPage("Index")  | *Pages/Customers/Index* |

`RedirectToPage("Index")`、`RedirectToPage("./Index")`、および `RedirectToPage("../Index")` は*相対名*です。 `RedirectToPage` パラメーターは現在のページのパスと*組み合わされて*、ターゲット ページの名前を計算します。  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

相対名のリンクは、複雑な構造を持つサイトを構築する際に役立ちます。 相対名を使用してフォルダー内のページ間をリンクする場合、そのフォルダー名を変更することができます。 すべてのリンクは引き続き機能します (リンクにはフォルダー名が含まれていないため)。

## <a name="tempdata"></a>TempData

ASP.NET Core は [コントローラー](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller)上で [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) プロパティを公開します。 このプロパティは、読み取られるまでデータを格納します。 `Keep` メソッドと `Peek` メソッドは、削除せずにデータを確認するために使用できます。 `TempData` は、複数の要求にデータが必要な場合のリダイレクトに役立ちます。

`[TempData]` は ASP.NET Core 2.0 の新しい属性で、コントローラーとページでサポートされています。

次のコードは、`TempData` を使用して `Message` の値を設定します。

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

*Pages/Customers/Index.cshtml* ファイル内の次のマークアップは、`TempData` を使用して `Message` の値を表示します。

```cshtml
<h3>Msg: @Model.Message</h3>
```

*Pages/Customers/Index.cshtml.cs* 分離コード ファイルは、`[TempData]` 属性を `Message` プロパティに適用します。

```cs
[TempData]
public string Message { get; set; }
```

詳細については、「[TempData](xref:fundamentals/app-state#temp)」を参照してください。

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a>ページあたり複数のハンドラー

次のページは、`asp-page-handler` タグ ヘルパーを使用して 2 つのページ ハンドラーにマークアップを生成します。

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

前の例のフォームには、それぞれが `FormActionTagHelper` を使用して異なる URL に送信する 2 つの送信ボタンがあります。 `asp-page-handler` 属性は、`asp-page` のコンパニオンです。 `asp-page-handler` はページごとに定義されている各ハンドラー メソッドに送信する URL を生成します。 サンプルは現在のページにリンクしているため、`asp-page` は指定されません。

分離コード ファイル:

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

上記のコードは、*名前付きハンドラー メソッド*を使用しています。 名前付きハンドラー メソッドは、名前の `On<HTTP Verb>` の後および `Async` の前 (ある場合) のテキストを取得して作成されます。 前の例では、ページ メソッドは OnPost**JoinList**Async と OnPost**JoinListUC**Async です。 *OnPost* と *Async* を削除すると、ハンドラー名は `JoinList` と `JoinListUC` になります。

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

上記のコードを使用すると、`OnPostJoinListAsync` に送信される URL パスは `http://localhost:5000/Customers/CreateFATH?handler=JoinList` になります。 `OnPostJoinListUCAsync` に送信される URL パスは `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC` です。

## <a name="customizing-routing"></a>ルーティングのカスタマイズ

URL 内のクエリ文字列 `?handler=JoinList` が気に入らない場合は、ルートを変更して URL のパス部分にハンドラー名を挿入することができます。 `@page` ディレクティブの後に二重引用符で囲んだルート テンプレートを追加して、ルートをカスタマイズすることができます。

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

上記のルートでは、クエリ文字列の代わりにハンドラー名を URL パスに挿入しています。 `handler` の後の `?` は、ルート パラメーターが省略可能なことを意味します。

`@page` を使用して、ページのルートに追加のセグメントとパラメーターを追加できます。 ここにあるものは何でもページの既定のルートに**追加**されます。 絶対パスまたは仮想パスを使用してページのルート (`"~/Some/Other/Path"` など) を変更することはサポートされていません。

## <a name="configuration-and-settings"></a>構成と設定

高度なオプションを構成するには、MVC ビルダーで拡張メソッド `AddRazorPagesOptions` を使用します。

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

現在は、`RazorPagesOptions` を使用してページのルート ディレクトリを設定したり、ページのアプリケーション モデルの規則を追加したりすることができます。 将来、この方法でより多くの機能拡張を可能にしたいと考えています。

ビューをプリコンパイルするには、「[Razor view compilation](xref:mvc/views/view-compilation)」 (Razor ビュー コンパイル) を参照してください。

[サンプル コードをダウンロードまたは表示します](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample)。

この概要に基づく、「[Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start)」 (ASP.NET Core での Razor ページの概要) を参照してください。