---
title: "部分ビュー"
author: ardalis
description: "ASP.NET Core mvc 部分ビューを使用します。"
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/partial
ms.openlocfilehash: e5c8aac855a1f4ec4c6f08087dbe77f6820cc506
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="partial-views"></a>部分ビュー

によって[Steve Smith](https://ardalis.com/)、 [Maher JENDOUBI](https://twitter.com/maherjend)、および[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC には、部分ビューは、さまざまなビューの間で共有したい web ページの再利用可能な部分がある場合に便利ですがサポートされています。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="what-are-partial-views"></a>部分ビューとは

部分ビューは、別のビュー内で表示されるビューです。 部分ビューを実行することによって生成された HTML 出力は、呼び出し元 (親) のビューに表示されます。 部分ビューを使用して、ビューと同様に、 *.cshtml*ファイル拡張子。

## <a name="when-should-i-use-partial-views"></a>部分ビューを使用するか。

部分ビューは、効果的な方法を小さな要素に大規模なビューを互換性に影響するのです。 コンテンツの表示の重複を減らすし、ビューの要素を再利用を許可できます。 一般的なレイアウト要素を指定する必要があります[_Layout.cshtml](layout.md)です。 非レイアウトの再利用可能なコンテンツは、部分ビューにカプセル化されることができます。

いくつかの論理的な部分から成る複雑なページがある場合は、それぞれのピースとして独自の部分ビューを使用すると便利ができます。 ページの各部分は、ページの残りの部分から分離で表示できるし、全体のページ構造と、部分ビューを表示するために呼び出しのみが含まれるため、ページ自体のビューをより簡単になります。

ヒント: 次の[しない繰り返します自分で原則](http://deviq.com/don-t-repeat-yourself/)ビューにします。

## <a name="declaring-partial-views"></a>部分ビューの宣言

部分ビューを他のビューと同様に作成されます: を作成する、 *.cshtml*ファイルの場所、*ビュー*フォルダーです。 部分ビューと通常のビューの間のセマンティックの違いはありません - 異なる方法で、レンダリングされるだけです。 コント ローラーから直接返されるビューを持つことができます`ViewResult`、同じビューは、部分ビューとして使用できます。 ビューと、部分ビューをレンダリングする方法の主な違いは、部分ビューが実行しない*は _viewstart.vbhtml* (ビュー-詳細については、*は _viewstart.vbhtml*で[レイアウト](layout.md)).

## <a name="referencing-a-partial-view"></a>部分ビューを参照します。

表示 ページでは、いくつか方法は、部分ビューをレンダリングできます。 使用する最も単純な`Html.Partial`、返された、`IHtmlString`の呼び出しを付けることで参照できます`@`:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=9)]

`PartialAsync`メソッドは、部分ビューの非同期コードを含む (ただし、ビュー内のコードは通常、推奨されません) の使用。

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

部分ビューをレンダリングする`RenderPartial`です。 このメソッドは、結果を返さない応答に直接表示される出力をストリームします。 これは結果を返さないため Razor コード ブロック内で呼び出す必要があります (を呼び出すことも`RenderPartialAsync`必要な場合)。

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=10-12)]

ストリーム出力し、結果を直接ため`RenderPartial`と`RenderPartialAsync`一部のシナリオでパフォーマンスが向上します。 ただし、ことをお勧めするほとんどの場合で次のように使用します。`Partial`と`PartialAsync`です。

> [!NOTE]
> 使用する、推奨パターンは、ビューは、ユーザーは、コードを実行する必要がある場合、[ビュー コンポーネント](view-components.md)部分ビューの代わりにします。

### <a name="partial-view-discovery"></a>部分ビューの検出

部分ビューを参照するときに、いくつかの方法でその場所を参照することができます。

```text
// Uses a view in current folder with this name
// If none is found, searches the Shared folder
@Html.Partial("ViewName")

// A view with this name must be in the same folder
@Html.Partial("ViewName.cshtml")

// Locate the view based on the application root
// Paths that start with "/" or "~/" refer to the application root
@Html.Partial("~/Views/Folder/ViewName.cshtml")
@Html.Partial("/Views/Folder/ViewName.cshtml")

// Locate the view using relative paths
@Html.Partial("../Account/LoginPartial.cshtml")
```

別のビューのフォルダーに同じ名前の別の部分ビューを設定できます。 参照する場合、ビュー名 (ファイル拡張子なし) で、各フォルダー内のビューはそれらの同じフォルダーに、部分ビューを使用します。 指定することも、既定の部分ビューを使用するのに配置することで、 *Shared*フォルダーです。 共有部分ビューは、部分ビューの独自のバージョンがないすべてのビューで使用されます。 既定の部分ビューを持つことができます (で*Shared*)、これは、親ビューと同じフォルダーに同じ名前の部分ビューによってオーバーライドします。

部分ビューは、*チェーン*です。 つまり、部分ビューは、(限りループを作成しない)、別の部分ビューを呼び出すことができます。 各ビューまたは部分ビュー内の相対パスは常にそのルートまたは親ではない、ビューのビューです。

> [!NOTE]
> 宣言する場合、 [Razor](razor.md) `section`部分ビューで、表示されませんをその親です。 これは、部分ビューに限定されます。

## <a name="accessing-data-from-partial-views"></a>部分ビューからデータにアクセスします。

親ビューのコピー、部分ビューをインスタンス化時に取得`ViewData`ディクショナリ。 親ビューには、部分ビュー内のデータに対して加えた更新は保持されません。 `ViewData`部分で変更された部分ビューが返されるときに、ビューは失われます。

インスタンスを渡すことができます`ViewDataDictionary`部分ビューへ。

```csharp
@Html.Partial("PartialName", customViewData)
   ```

部分ビューにモデルを渡すこともできます。 ページのビュー モデル、またはの一部またはカスタム オブジェクトを指定できます。 モデルを渡すことができます`Partial`、`PartialAsync`、 `RenderPartial`、または`RenderPartialAsync`:

```csharp
@Html.Partial("PartialName", viewModel)
   ```

インスタンスを渡すことができます`ViewDataDictionary`と、部分ビューをビュー モデル。

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

マークアップを示しています、 *Views/Articles/Read.cshtml*ビューには 2 つの部分ビューが含まれています。 2 番目の部分ビューは、モデル内に渡しますと`ViewData`部分の表示にします。 新規に渡すことができます`ViewData`既存を維持したままディクショナリ`ViewData`のコンス トラクター オーバー ロードを使用する場合、`ViewDataDictionary`下強調表示されています。

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*ビュー/共有/AuthorPartial*:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

*ArticleSection*部分。

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

パーシャルのレンダリング時に、それ自体が表示される親ビューに、共有内*_Layout.cshtml*

![部分ビューの出力](partial/_static/output.png)
