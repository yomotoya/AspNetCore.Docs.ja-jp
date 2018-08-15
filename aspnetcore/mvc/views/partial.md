---
title: ASP.NET Core の部分ビュー
author: ardalis
description: 部分ビューが別のビュー内でどのようにレンダリングされるビューか、ASP.NET Core アプリでどのような場合に使用すべきかについて説明します。
ms.author: riande
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/partial
ms.openlocfilehash: 2223f3c6e42927def4b91ff9da58c228e5904756
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655324"
---
# <a name="partial-views-in-aspnet-core"></a>ASP.NET Core の部分ビュー

作成者: [Steve Smith](https://ardalis.com/)、[Maher JENDOUBI](https://twitter.com/maherjend)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Scott Sauber](https://twitter.com/scottsauber)

ASP.NET Core では部分ビューがサポートされます。 部分ビューは、さまざまなビュー全体で、Web ページの再利用可能な部分を共有するために使用されます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="what-are-partial-views"></a>部分ビューとは

部分ビューとは、別のビュー内でレンダリングされるビューのことです。 部分ビューを実行して生成される HTML 出力は、呼び出し元 (または親) のビューにレンダリングされます。 ビューと同様、部分ビューでは *.cshtml* ファイル拡張子を使用します。

たとえば、ASP.NET Core 2.1 の **Web アプリケーション** プロジェクト テンプレートには、*_CookieConsentPartial.cshtml* 部分ビューが含まれています。 部分ビューは *_Layout.cshtml* 内から読み込まれます。

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_Layout.cshtml?name=snippet_CookieConsentPartial)]

## <a name="when-to-use-partial-views"></a>部分ビューを使用する状況

部分ビューは、より小さい要素に大きいビューを分割する効果的な方法です。 ビュー コンテンツの重複を減らし、ビュー要素を再利用できます。 一般的なレイアウト要素は、[_Layout.cshtml](xref:mvc/views/layout) に指定する必要があります。 レイアウト以外の再利用可能なコンテンツは、部分ビューにカプセル化できます。

いくつかの論理部分で構成される複雑なページでは、それぞれの部分を独自の部分ビューとして操作する際に役立ちます。 ページの各部分は、ページの残りとは別に表示することができます。 ページ自体のビューには全体のページ構造のみが含まれ、部分ビューのレンダリングの場合は呼び出しが行われるため、単純になります。

ASP.NET Core MVC のコントローラーは、アクション メソッドから呼び出される [PartialView](/dotnet/api/microsoft.aspnetcore.mvc.controller.partialview#Microsoft_AspNetCore_Mvc_Controller_PartialView) メソッドを備えています。 Razor Pages には、[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 上の `PartialView` に相当するメソッドがありません。

## <a name="declare-partial-views"></a>部分ビューを宣言する

部分ビューは標準ビューと同様に作成されます&mdash; その場合、*Views* フォルダー内に *.cshtml* ファイルを作成します。 部分ビューと標準ビューのセマンティックに違いはありません。ただし、レンダリングの方法が異なります。 コントローラーの [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult) から直接返されるビューを使用でき、これと同じビューを部分ビューとして使用できます。 ビューと部分ビューのレンダリング方法の主な違いは、部分ビューでは *_ViewStart.cshtml* を実行しないことです。 標準ビューでは *_ViewStart.cshtml* を実行します。 *_ViewStart.cshtml* の詳細については、[レイアウトに関するページ](xref:mvc/views/layout)を参照してください。

通常、部分ビューのファイル名は `_` で始まります。 この名前付け規則は必須ではありませんが、標準ビューと部分ビューを視覚的に区別する場合に便利です。

## <a name="reference-a-partial-view"></a>部分ビューを参照する

ビュー ページ内から、部分ビューをレンダリングする方法はいくつかあります。 非同期のレンダリングを使用することをお勧めします。

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>部分タグ ヘルパー

部分タグ ヘルパーでは、ASP.NET Core 2.1 以降が必要です。 非同期的にレンダリングし、HTML のような構文を使用します。

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialTagHelper)]

詳細については、「<xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>」を参照してください。

::: moniker-end

### <a name="asynchronous-html-helper"></a>非同期の HTML ヘルパー

HTML ヘルパーを使用する場合は、[PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync#Microsoft_AspNetCore_Mvc_Rendering_HtmlHelperPartialExtensions_PartialAsync_Microsoft_AspNetCore_Mvc_Rendering_IHtmlHelper_System_String_) を使用することをお勧めします。 `Task` でラップされた [IHtmlContent](/dotnet/api/microsoft.aspnetcore.html.ihtmlcontent) 型が返されます。 このメソッドは `@` による呼び出しを前に付けることで参照されます。

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialAsync)]

または、[RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync) で部分ビューをレンダリングすることもできます。 このメソッドは結果を返しません。 レンダリングされた出力を直接応答にストリーミングします。 メソッドが結果を返さないため、Razor コード ブロック内で呼び出す必要があります。

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

結果を直接ストリーミングするため、一部のシナリオでは `RenderPartialAsync` のパフォーマンスが向上する場合がありますが、 `PartialAsync` を使用することをお勧めします。

### <a name="synchronous-html-helper"></a>同期の HTML ヘルパー

[Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial) と [RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial) は、それぞれ同期の `PartialAsync` と `RenderPartialAsync` に相当します。 デッドロックが発生するシナリオがあるため、同期に相当する機能の使用はお勧めしません。 今後のリリースには、同期のメソッドは含まれません。

> [!IMPORTANT]
> ビューでコードを実行する必要がある場合は、部分ビューの代わりに[ビュー コンポーネント](xref:mvc/views/view-components)を使用してください。

::: moniker range=">= aspnetcore-2.1"

ASP.NET Core 2.1 以降では、`Partial` または `RenderPartial` を呼び出すと、アナライザーの警告が発生します。 たとえば、`Partial` を使用すると、次の警告メッセージが生成されます。

> IHtmlHelper.Partial を使用すると、アプリケーションのデッドロックが発生する可能性があります。 `<partial>` タグ ヘルパーまたは `IHtmlHelper.PartialAsync` の使用を検討してください。

`@Html.Partial` の呼び出しを、`@await Html.PartialAsync` または部分タグ ヘルパーに置き換えてください。 部分タグ ヘルパーの移行の詳細については、「[HTML ヘルパーから移行する](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper)」を参照してください。

::: moniker-end

## <a name="partial-view-discovery"></a>部分ビューの検出

部分ビューを参照するときに、いくつかの方法でその場所を参照することができます。 例:

::: moniker range=">= aspnetcore-2.1"

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
<partial name="_ViewName" />

// A view with this name must be in the same folder
<partial name="_ViewName.cshtml" />

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
<partial name="~/Views/Folder/_ViewName.cshtml" />
<partial name="/Views/Folder/_ViewName.cshtml" />

// Locate the view using a relative path
<partial name="../Account/_LoginPartial.cshtml" />
```

前の例では、ASP.NET Core 2.1 以降を必要とする部分タグ ヘルパーを使用しています。 次の例では、非同期の HTML ヘルパーを使用して、同じタスクを実行しています。

::: moniker-end

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
@await Html.PartialAsync("_ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("_ViewName.cshtml")

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
@await Html.PartialAsync("~/Views/Folder/_ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/_ViewName.cshtml")

// Locate the view using a relative path
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

別のビュー フォルダーで同じファイル名の別の部分ビューを使用することができます。 名前 (ファイル拡張子なし) でビューを参照する場合、各フォルダー内のビューではそれと同じフォルダーの部分ビューを使用します。 *Shared* フォルダーに配置して、使用する既定の部分ビューを指定することもできます。 共有部分ビューは、部分ビューの独自のバージョンがないすべてのビューで使用されます。 親ビューと同じフォルダーの同じ名前の部分ビューによってオーバーライドされる、既定の部分ビューを (*Shared* 内で) 使用できます。

部分ビューは*チェーンする*ことができます&mdash;部分ビューは別の部分ビューを呼び出すことができます (ループを作成しない場合に限る)。 各ビューまたは部分ビュー内では、相対パスは、ルートや親ビューではなく、常にそのビューに対して相対的になります。

> [!NOTE]
> 部分ビューで定義されている [Razor](xref:mvc/views/razor) `section` は、親ビューに表示されません。 `section` は定義されている部分ビューにのみ表示されます。

## <a name="access-data-from-partial-views"></a>部分ビューからデータにアクセスする

親ビューのインスタンス化の際に、親ビューの `ViewData` ディクショナリのコピーが取得されます。 親ビュー内のデータに対して行われた更新は、親ビューでは保持されません。 部分ビューで変更された `ViewData` は、部分ビューから返されるときに失われます。

[ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) のインスタンスを部分ビューに渡すことができます。

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

部分ビューにモデルを渡すことができます。 モデルは、ページのビュー モデルまたはカスタム オブジェクトにすることができます。 モデルを `PartialAsync` または `RenderPartialAsync` に渡すことができます。

```cshtml
@await Html.PartialAsync("_PartialName", viewModel)
```

`ViewDataDictionary` のインスタンスとビュー モデルを部分ビューに渡すことができます。

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_PartialAsync)]

以下のマークアップは、2 つの部分ビューを含む *Views/Articles/Read.cshtml* ビューを示しています。 2 番目の部分ビューは、モデルと `ViewData` を部分ビューに渡します。 強調表示されている `ViewDataDictionary` のコンストラクター オーバーロードを使用して、既存の `ViewData` を維持したまま、新しい `ViewData` ディクショナリを渡します。

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=17-20)]

*Views/Shared/_AuthorPartial*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

*_ArticleSection* 部分:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

実行時に、この部分は親ビューにレンダリングされます。親ビュー自体は共有されている *_Layout.cshtml* 内にレンダリングされます。

![部分ビューの出力](partial/_static/output.png)

## <a name="additional-resources"></a>その他の技術情報

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>

::: moniker-end
