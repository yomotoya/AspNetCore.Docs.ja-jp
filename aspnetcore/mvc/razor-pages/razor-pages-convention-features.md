---
title: "ASP.NET Core で razor ページのルートとアプリの規則機能"
author: guardrex
description: "モデル プロバイダーの規則の機能をルートとアプリの利用方法コントロール ページのルーティング、探索、および処理を検出します。"
keywords: "ASP.NET Core、Razor ページ、規則、AddFolderRouteModelConvention、AddPageRouteModelConvention、AddPageRoute、AddFolderApplicationModelConvention、AddPageApplicationModelConvention、ConfigureFilter、フィルター処理"
ms.author: riande
manager: wpickett
ms.date: 10/23/2017
ms.topic: article
ms.assetid: 6b60514c-81ad-485b-bb22-9b71416dff08
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: 81fe5198e25c4275f5cf0a123536a9130be8c1d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a>ASP.NET Core で razor ページのルートとアプリの規則機能

作成者: [Luke Latham](https://github.com/guardrex)

ページのルーティング、探索、および Razor ページのアプリでの処理を制御するページのルートとアプリ モデル プロバイダー規約機能を使用する方法を説明します。 使用してページにルーティングを構成する個々 のページのページのカスタム ルートを構成する必要がある場合、 [AddPageRoute 規約](#configure-a-page-route)このトピックの後半で説明します。

使用して、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) をこのトピックで説明する機能を探索します。

| フィーチャー | このサンプルでは. |
| -------- | --------------------------- |
| [モデルのルートとアプリの規則](#add-route-and-app-model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | アプリのページへのルート テンプレートとヘッダーを追加します。 |
| [ページのルート アクションの表記規則](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | 1 つのページと、フォルダー内のページには、ルート テンプレートを追加します。 |
| [ページのモデルの操作規則](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (フィルター クラス、ラムダ式、またはフィルター ファクトリ)</li></ul> | フォルダー内のページにヘッダーを追加する、単一のページにヘッダーを追加して、構成、[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)アプリのページにヘッダーを追加します。 |
| [既定ページ アプリ モデル プロバイダー](#replace-the-default-page-app-model-provider) | ハンドラーの名前付け規則を変更するには、既定ページ モデル プロバイダーを置換します。 |

## <a name="add-route-and-app-model-conventions"></a>モデルのルートとアプリの規則を追加します。

デリゲートを追加[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) Razor ページに適用されるルートとアプリのモデルの規則を追加します。

**すべてのページにルート モデルの規則を追加します。**

使用して[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を作成し、追加、 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)のコレクションに[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention)ルートとページのモデルの中に適用されるインスタンス構築します。

サンプル アプリを追加、`{globalTemplate?}`すべてのアプリのページにルート テンプレート。

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> `Order`プロパティを`AttributeRouteModel`に設定されている`0`(ゼロ)。 これにより、このテンプレートが与えられているルート データ値の位置は、最初の優先順位 1 つのルート値を指定するときにします。 たとえば、サンプルが追加されて、`{aboutTemplate?}`トピックの後半でルート テンプレート。 `{aboutTemplate?}`テンプレートが指定された、`Order`の`1`します。 [バージョン情報] ページが要求された場合`/About/RouteDataValue`、"RouteDataValue"が読み込まれます`RouteData.Values["globalTemplate"]`(`Order = 0`) および not `RouteData.Values["aboutTemplate"]` (`Order = 1`) の設定のため、`Order`プロパティです。

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

[要求について] ページで、サンプルの`localhost:5000/About/GlobalRouteValue`され、結果を調べる。

![GlobalRouteValue のルート セグメントでは、バージョン情報 ページが要求されます。 表示されたページは、ルート データの値が、ページの OnGet メソッド内でキャプチャされるを示しています。](razor-pages-convention-features/_static/about-page-global-template.png)

**すべてのページにアプリ モデルの規則を追加します。**

使用して[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を作成し、追加、 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)のコレクションに[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention)ルートとページの中に適用されるインスタンスモデルの構築します。

これとトピックの以降の他の規則を示すため、サンプル アプリケーションが含まれています、`AddHeaderAttribute`クラスです。 クラスのコンス トラクターを受け入れる、`name`文字列と`values`文字列配列。 これらの値が使用されるその`OnResultExecuting`応答ヘッダーを設定します。 完全クラスを示す、[モデル操作の規則 ページ](#page-model-action-conventions)トピックの以降のセクションでします。

サンプル アプリは、 `AddHeaderAttribute` 、ヘッダーを追加するクラス`GlobalHeader`、すべてのアプリのページに。

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

要求について ページで、サンプルの`localhost:5000/About`し、結果を表示するヘッダーを検査します。

![バージョン情報 ページの応答ヘッダー、GlobalHeader が追加されていることを示します。](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a>ページのルート アクションの表記規則

派生する既定のルート モデル プロバイダー [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider)をページのルートを構成するための機能拡張ポイントを提供できるように設計された規則を呼び出します。

**フォルダーのルート モデルの規則**

使用して[AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention)を作成して追加、 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)でアクションを呼び出す、 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel)すべての下にあるページ、指定したフォルダーです。

サンプル アプリは`AddFolderRouteModelConvention`を追加する、`{otherPagesTemplate?}`ルート テンプレート内のページに、 *OtherPages*フォルダー。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> `Order`プロパティを`AttributeRouteModel`に設定されている`1`です。 これにより、テンプレートを`{globalTemplate?}`(このトピックではセット) が優先単一のルート値を指定するときに、ルートの最初のデータ値の位置。 Page1 ページが要求されている場合`/OtherPages/Page1/RouteDataValue`、"RouteDataValue"に読み込まれる`RouteData.Values["globalTemplate"]`(`Order = 0`) および not `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) の設定のため、`Order`プロパティです。

サンプルの Page1 ページ要求`localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue`が表示され、結果を調べる。

![Page1 OtherPages フォルダーには、GlobalRouteValue と OtherPagesRouteValue のルート セグメントを持つ要求されます。 表示されたページは、ルート データの値が、ページの OnGet メソッド内でキャプチャされるを示しています。](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

**ページのルート モデルの規則**

使用して[AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention)を作成し、追加、 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)でアクションを呼び出す、 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel)指定したページの名前です。

サンプル アプリは`AddPageRouteModelConvention`を追加する、`{aboutTemplate?}`バージョン情報 ページにルート テンプレート。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> `Order`プロパティを`AttributeRouteModel`に設定されている`1`です。 これにより、テンプレートを`{globalTemplate?}`(このトピックではセット) が優先単一のルート値を指定するときに、ルートの最初のデータ値の位置。 [バージョン情報] ページが要求されている場合`/About/RouteDataValue`、"RouteDataValue"が読み込まれます`RouteData.Values["globalTemplate"]`(`Order = 0`) および not `RouteData.Values["aboutTemplate"]` (`Order = 1`) の設定のため、`Order`プロパティです。

[要求について] ページで、サンプルの`localhost:5000/About/GlobalRouteValue/AboutRouteValue`され、結果を調べる。

![ページに関する要求に含まれるルート セグメント GlobalRouteValue および AboutRouteValue されます。 表示されたページは、ルート データの値が、ページの OnGet メソッド内でキャプチャされるを示しています。](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>ページのルートを構成します。

使用して[AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute)を指定したページのパスにあるページへのルートを構成します。 ページに生成されたリンクは、指定されたルートを使用します。 `AddPageRoute`使用して`AddPageRouteModelConvention`ルートを作成します。

サンプル アプリへのルートを作成する`/TheContactPage`の*Contact.cshtml*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

連絡先 ページは、現在は到達も`/Contact`既定のルートを使用しています。

メンバーのページに、サンプル アプリのカスタム ルートでは、省略可能な`text`ルート セグメント (`{text?}`)。 ページは、この省略可能なセグメントでも含まれています。 その`@page`ディレクティブの訪問者のページにアクセスする場合にその`/Contact`ルート。

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

URL が生成されることに注意してください、**連絡先**レンダリングされるページのリンクには、更新されたルートが反映されます。

![ナビゲーション バーで、サンプル アプリにお問い合わせくださいリンク](razor-pages-convention-features/_static/contact-link.png)

![レンダリングされる HTML の連絡先のリンクの検査 href に設定されていることを示します '/TheContactPage'](razor-pages-convention-features/_static/contact-link-source.png)

ページにアクセスして、連絡先どちらでも、通常そのルート上`/Contact`、またはカスタムのルート`/TheContactPage`です。 追加の指定した場合`text`ルート セグメント ページを示しています、HTML でエンコードされたセグメントを指定します。

![オプション 'text' ルート セグメント、URL で 'TextValue' を指定する edge ブラウザー例です。 表示されたページは、'text' セグメントの値を示します。](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>ページのモデルの操作規則

実装してページ モデルの既定のプロバイダー [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider)ページ モデルを構成するための機能拡張ポイントを提供するように設計されている規則を呼び出します。 これらの規則は、ビルドとページの検出と処理機能を変更すると便利です。

このセクションの例については、サンプル アプリを使用して、`AddHeaderAttribute`はクラス、 [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute)、応答ヘッダーを適用します。

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

規則を使用して、サンプルは、1 つのページと、フォルダー内のページのすべてには、属性を適用する方法を示します。

**フォルダー アプリ モデルの規則**

使用して[AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention)を作成し、追加、 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)でアクションを呼び出す[PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel)のインスタンス指定したフォルダーの下のすべてのページです。

サンプルの使用`AddFolderApplicationModelConvention`、ヘッダーを追加することによって`OtherPagesHeader`、内のページに、 *OtherPages*アプリのフォルダー。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

サンプルの Page1 ページ要求`localhost:5000/OtherPages/Page1`結果を表示するヘッダーを検査および。

![OtherPages/Page1 ページの応答ヘッダー、OtherPagesHeader が追加されていることを示します。](razor-pages-convention-features/_static/page1-otherpages-header.png)

**ページ アプリ モデルの規則**

使用して[AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention)を作成し、追加、 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)でアクションを呼び出す、 [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel)ページの名前に置き換えます speciifed。

サンプルの使用`AddPageApplicationModelConvention`、ヘッダーを追加することによって`AboutHeader`、バージョン情報 ページに。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

要求について ページで、サンプルの`localhost:5000/About`し、結果を表示するヘッダーを検査します。

![バージョン情報 ページの応答ヘッダー、AboutHeader が追加されていることを示します。](razor-pages-convention-features/_static/about-page-about-header.png)

**フィルターを構成します。**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter)指定されたフィルターを適用するように構成します。 フィルター クラスを実装することができますが、サンプル アプリは舞台裏フィルターを返すファクトリとして実装されているラムダ式でフィルターを実装する方法を示します。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

セグメントで Page2 ページへの相対パスを確認するページ アプリ モデルが使用される、 *OtherPages*フォルダーです。 条件が成功した場合、ヘッダーが追加されます。 ない場合、`EmptyFilter`を適用します。

`EmptyFilter`[アクション フィルター](xref:mvc/controllers/filters#action-filters)です。 Razor ページによってアクション フィルターが無視されるので、`EmptyFilter`キャッシュなしで、パスが含まれていないかどうかは意図されたように`OtherPages/Page2`です。

サンプルのページ 2 ページを要求`localhost:5000/OtherPages/Page2`され、結果を表示するヘッダーを調べる。

![OtherPagesPage2Header は Page2 の応答に追加されます。](razor-pages-convention-features/_static/page2-filter-header.png)

**フィルターのファクトリを構成します。**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__)構成を適用する指定されたファクトリ[フィルター](xref:mvc/controllers/filters)すべての Razor ページにします。

サンプル アプリを使用する例を提供する、[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)、ヘッダーを追加することによって`FilterFactoryHeader`アプリのページへの 2 つの値。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

要求について ページで、サンプルの`localhost:5000/About`し、結果を表示するヘッダーを検査します。

![バージョン情報 ページの応答ヘッダーは、2 つの FilterFactoryHeader ヘッダーが追加されたことを示します。](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>既定ページ アプリ モデル プロバイダーを置き換える

Razor ページを使用して、`IPageApplicationModelProvider`を作成するインターフェイス、 [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider)です。 ハンドラーの検出と処理のため、独自の実装ロジックを提供する既定のモデル プロバイダーから継承することができます。 既定の実装 ([参照ソース](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) の規則を確立*名前のない*と*という*は以下の説明、名前を付けるハンドラー。

**既定の名前のないハンドラー メソッド**

ハンドラー メソッドの HTTP 動詞 (「名前のない」ハンドラー メソッド)、規則に従って: `On<HTTP verb>[Async]` (追加`Async`は任意ですが、非同期メソッドのために推奨)。

| 名前のないハンドラー メソッド     | 操作                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | ページの状態を初期化します。     |
| `OnPost`/`OnPostAsync`     | POST 要求を処理します。          |
| `OnDelete`/`OnDeleteAsync` | #8224; (&)、DELETE 要求を処理します。 |
| `OnPut`/`OnPutAsync`       | #8224; (&)、PUT 要求を処理します。    |
| `OnPatch`/`OnPatchAsync`   | #8224; (&)、PATCH 要求を処理します。  |

&#8224;です。ページに API 呼び出しを行うために使用します。

**既定の名前付きハンドラー メソッド**

(「名前」ハンドラー メソッド)、開発者によって提供されるハンドラー メソッドでは、同様の規則に従います。 HTTP 動詞の後、または HTTP 動詞の間に、ハンドラーの名前が表示されます、 `Async`: `On<HTTP verb><handler name>[Async]` (追加`Async`は任意ですが、非同期メソッドのために推奨)。 たとえば、メッセージを処理するメソッドは、次の表に示すように名前が付けかかる場合があります。

| ハンドラー メソッドをという名前の例             | 例の操作        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | メッセージを取得します。        |
| `OnPostMessage`/`OnPostMessageAsync`     | メッセージを投稿します。          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | #8224; (&)、メッセージを削除します。 |
| `OnPutMessage`/`OnPutMessageAsync`       | #8224; (&)、メッセージを配置します。    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | メッセージ &#8224; 修正プログラムを適用します。  |

&#8224;です。ページに API 呼び出しを行うために使用します。

**ハンドラー メソッドの名前を変更します。**

名前と名前付きのハンドラー メソッドの名前を変更したいとします。 別の名前付けスキームでは、"On"に、メソッド名に開始しないようにし、HTTP 動詞を決定する単語の最初のセグメントを使用します。 その他の変更を行うことができます for DELETE 動詞を変換するなど PUT、および投稿に修正プログラムを適用します。 このようなスキームでは、次の表に示すように、メソッド名を提供します。

| ハンドラー メソッド                       | 操作                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | ページの状態を初期化します。     |
| `Post`/`PostAsync`                   | POST 要求を処理します。          |
| `Delete`/`DeleteAsync`               | #8224; (&)、DELETE 要求を処理します。 |
| `Put`/`PutAsync`                     | #8224; (&)、PUT 要求を処理します。    |
| `Patch`/`PatchAsync`                 | #8224; (&)、PATCH 要求を処理します。  |
| `GetMessage`                         | メッセージを取得します。              |
| `PostMessage`/`PostMessageAsync`     | メッセージを投稿します。                |
| `DeleteMessage`/`DeleteMessageAsync` | 削除するメッセージを投稿します。      |
| `PutMessage`/`PutMessageAsync`       | メッセージを投稿して配置します。         |
| `PatchMessage`/`PatchMessageAsync`   | メッセージを投稿する修正プログラムを適用します。       |

&#8224;です。ページに API 呼び出しを行うために使用します。

継承するこのパターンを確立するために、`DefaultPageApplicationModelProvider`クラスし、オーバーライド、 [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel)を解決するためのカスタム ロジックを提供するメソッド[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel)ハンドラー名。 サンプル アプリこれを行う方法を示します、`CustomPageApplicationModelProvider`クラス。

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

クラスの主な特徴は次のとおりです。

* クラスを継承`DefaultPageApplicationModelProvider`です。
* `TryParseHandlerMethod` HTTP 動詞を確認するハンドラーの処理 (`httpMethod`) と名前付きのハンドラーの名前 (`handlerName`) を作成するとき、`PageHandlerModel`です。
  * `Async`存在する場合、後置形式は無視されます。
  * 大文字と小文字を使用すると、メソッドの名前の HTTP 動詞を解析します。
  * ときに、メソッド名 (せず`Async`) は、HTTP 動詞名と同じ、ハンドラーがない名前付きです。 `handlerName`に設定されている`null`、メソッド名と`Get`、 `Post`、 `Delete`、 `Put`、または`Patch`です。
  * ときに、メソッド名 (せず`Async`) HTTP 動詞名よりも長い名前付きハンドラーがあります。 `handlerName` に `<method name (less 'Async', if present)>` が設定されています。 たとえば、"GetMessage"と"GetMessageAsync"の両方は、"GetMessage"のハンドラー名を生成します。
  * 削除、PUT、PATCH HTTP 動詞が POST に変換されます。

登録、`CustomPageApplicationModelProvider`で、`Startup`クラス。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

分離コード ファイル*Index.cshtml.cs*アプリ内のページの通常のハンドラー メソッドの名前付け規則を変更する方法を示しています。 "On"Razor ページで使用される名前が付けられ、通常は削除されます。 ページの状態を初期化するメソッドの名前は今すぐ`Get`です。 ページのいずれかの任意の分離コード ファイルを開く場合に、アプリ全体で使用されるこの規則を表示できます。

それぞれの他の方法は、その処理を説明する HTTP 動詞を使用して開始します。 2 つの方法で始まる`Delete`は通常として扱う、DELETE HTTP 動詞がロジックでは、`TryParseHandlerMethod`両方ハンドラーに対して post 動詞を明示的に設定します。

なお`Async`は間で省略可能`DeleteAllMessages`と`DeleteMessageAsync`です。 非同期のどちらを使用することもできますが、`Async`か後置; を実行することをお勧めします。 `DeleteAllMessages`ここでは、デモンストレーションのための使用しますが、これらのメソッドの名前を付けることをお勧め`DeleteAllMessagesAsync`です。 処理に影響しませんのサンプルの実装を使用して、`Async`後置非同期メソッドであるファクトを呼び出します。

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

提供されるハンドラーの名前を控えておきます*Index.cshtml*一致、`DeleteAllMessages`と`DeleteMessageAsync`ハンドラー メソッド。

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async`ハンドラー メソッドの名前で`DeleteMessageAsync`でサインアウトが考慮され、`TryParseHandlerMethod`メソッドへの POST 要求の一致するハンドラー。 `asp-page-handler`名前`DeleteMessage`ハンドラー メソッドに一致する`DeleteMessageAsync`です。

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC のフィルターと、ページ フィルター (IPageFilter)

MVC[アクション フィルター](xref:mvc/controllers/filters#action-filters) Razor ページ ハンドラーのメソッドを使用するために Razor ページによって無視されます。 使用するための他の種類の MVC フィルターの利用できます:[承認](xref:mvc/controllers/filters#authorization-filters)、[例外](xref:mvc/controllers/filters#exception-filters)、[リソース](xref:mvc/controllers/filters#resource-filters)、および[結果](xref:mvc/controllers/filters#result-filters)です。 詳細については、次を参照してください。、[フィルター](xref:mvc/controllers/filters)トピックです。

ページ フィルター ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) は、Razor ページに適用されるフィルターです。 ページのハンドラー メソッドの実行が囲まれます。 ページのハンドラー メソッドの実行の段階でカスタム コードを処理することができます。 サンプル アプリからの使用例を次に示します。

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

このフィルターでのチェック、 `globalTemplate` "ReplacementValue"で"TriggerValue"とスワップの値をルーティングします。

`ReplaceRouteValueFilter`に直接適用できる属性、`PageModel`分離コード。

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

要求、Page3 ページでのサンプル アプリから`localhost:5000/OtherPages/Page3/TriggerValue`です。 フィルターが、ルート値を置換する方法に注意してください。

![ルートの値を置き換える値に置き換えてフィルターで TriggerValue ルート セグメント結果を含む OtherPages/Page3 を要求します。](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a>関連項目

* [Razor ページの承認規則](xref:security/authorization/razor-pages-authorization)
