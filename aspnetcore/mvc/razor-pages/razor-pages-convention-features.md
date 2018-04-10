---
title: ASP.NET Core での Razor ページのルートとアプリの規則機能
author: guardrex
description: ルートとアプリ モデル プロバイダーの規則機能が、ページのルーティング、検出、および処理の制御にどのように役立つかについて確認します。
manager: wpickett
ms.author: riande
ms.date: 10/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: bf1c895fc972310d5541d0098226d58b8183e320
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a>ASP.NET Core での Razor ページのルートとアプリの規則機能

作成者: [Luke Latham](https://github.com/guardrex)

ページ ルートとアプリ モデル プロバイダーの規則機能を使用して、Razor ページ アプリでページのルーティング、検出、および処理を制御する方法について説明します。 個別のページにカスタムのページ ルートを構成する必要がある場合、このトピックで後述される「[AddPageRoute convention](#configure-a-page-route)」で、ページへのルーティングを構成します。

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample)) を使用して、このトピックで説明される機能について調べます。

| フィーチャー | このサンプルでは、次のデモを実行します。 |
| -------- | --------------------------- |
| [ルート モデル規則とアプリ モデル規則](#add-route-and-app-model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | ルート テンプレートとヘッダーをアプリのページに追加する。 |
| [ページ ルート アクション規則](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | ルート テンプレートをフォルダー内のページおよび単一ページに追加する。 |
| [ページ モデル アクション規則](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (フィルター クラス、ラムダ式、またはフィルター ファクトリ)</li></ul> | ヘッダーをフォルダー内のページに追加する、ヘッダーを単一ページに追加する、ヘッダーをアプリのページに追加するように[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)を構成する。 |
| [既定のページ アプリ モデル プロバイダー](#replace-the-default-page-app-model-provider) | 既定のページ モデル プロバイダーを置き換えて、名前付けハンドラーの規則を変更する。 |

## <a name="add-route-and-app-model-conventions"></a>ルート モデル規則とアプリ モデル規則の追加

[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) の委任を追加して、Razor ページに適用するルート モデル規則とアプリ モデル規則を追加します。

**すべてのページにルート モデル規則を追加する**

[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を使用して、[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) を作成し、ルートとページ モデルの構築中に適用される [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) インスタンスのコレクションに追加します。

サンプル アプリでは、`{globalTemplate?}` ルート テンプレートをアプリ内のすべてのページに追加します。

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> `AttributeRouteModel` の `Order` プロパティに `0` (ゼロ) が設定されます。 この設定によって、単一のルート値が指定されたときに、このテンプレートが優先的に最初のルート データ値の位置に指定されます。 たとえば、このサンプルでは、このトピックの後で `{aboutTemplate?}` ルート テンプレートを追加します。 `{aboutTemplate?}` テンプレートの `Order` には `1` が指定されます。 [About] ページが `/About/RouteDataValue` で要求されると、"RouteDataValue" は `RouteData.Values["globalTemplate"]` (`Order = 0`) に読み込まれ、`Order` プロパティが設定されるため `RouteData.Values["aboutTemplate"]` (`Order = 1`) には読み込まれません。

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

`localhost:5000/About/GlobalRouteValue` でサンプルの [About] ページを要求し、その結果を調べます。

![[About] ページは、GlobalRouteValue のルート セグメントで要求されます。 レンダリングされたページは、ルート データの値がページの OnGet メソッドでキャプチャされたことを示しています。](razor-pages-convention-features/_static/about-page-global-template.png)

**すべてのページにアプリ モデル規則を追加する**

[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を使用して、[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) を作成し、ルートとページ モデルの構築中に適用される [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) インスタンスのコレクションに追加します。

この規則やこのトピックで後述されるその他の規則のデモを実行するには、サンプル アプリに `AddHeaderAttribute` クラスを含めます。 クラス コンストラクターは、`name` 文字列と `values` 文字列配列を受け入れます。 これらの値は、応答ヘッダーを設定するために、その `OnResultExecuting` メソッド内で使用されます。 完全クラスは、このトピックで後述される「[ページ モデル アクション規則](#page-model-action-conventions)」セクションで示されます。

サンプル アプリでは、`AddHeaderAttribute` クラスを使用して、ヘッダー (`GlobalHeader`) をアプリ内のすべてのページに追加します。

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

`localhost:5000/About` でサンプルの [About] ページを要求し、そのヘッダーを調べて結果を確認します。

![[About] ページの応答ヘッダーは、GlobalHeader が追加されたことを示しています。](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a>ページ ルート アクション規則

[IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) から派生する既定のルート モデル プロバイダーは、ページ ルートを構成するための拡張ポイントを提供するようにデザインされた規則を呼び出します。

**フォルダー ルート モデル規則**

[AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) を使用して、指定したフォルダーのページにある [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) のアクションを呼び出す、[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) を作成して追加します。

サンプル アプリでは `AddFolderRouteModelConvention` を使用して、`{otherPagesTemplate?}` ルート テンプレートを *OtherPages* フォルダーのページに追加します。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> `AttributeRouteModel` の `Order` プロパティに `1` が設定されます。 この設定により、単一のルート値が指定されたときに、(このトピックの前半で設定した) `{globalTemplate?}` のテンプレートが優先的に最初のルート データ値の位置に指定されます。 Page1 ページが `/OtherPages/Page1/RouteDataValue` で要求されると、"RouteDataValue" は `RouteData.Values["globalTemplate"]` (`Order = 0`) に読み込まれ、`Order` プロパティが設定されるため `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) には読み込まれません。

`localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` でサンプルの Page1 ページを要求し、その結果を調べます。

![OtherPages フォルダーの Page1 は、GlobalRouteValue と OtherPagesRouteValue のルート セグメントで要求されます。 レンダリングされたページは、ルート データの値がページの OnGet メソッドでキャプチャされたことを示しています。](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

**ページ ルート モデル規則**

[AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) を使用して、指定した名前でページの [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) のアクションを呼び出す、[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) を作成して追加します。

サンプル アプリでは `AddPageRouteModelConvention` を使用して、`{aboutTemplate?}` ルート テンプレートを [About] ページに追加します。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> `AttributeRouteModel` の `Order` プロパティに `1` が設定されます。 この設定により、単一のルート値が指定されたときに、(このトピックの前半で設定した) `{globalTemplate?}` のテンプレートが優先的に最初のルート データ値の位置に指定されます。 [About] ページが `/About/RouteDataValue` で要求されると、"RouteDataValue" は `RouteData.Values["globalTemplate"]` (`Order = 0`) に読み込まれ、`Order` プロパティが設定されるため `RouteData.Values["aboutTemplate"]` (`Order = 1`) には読み込まれません。

`localhost:5000/About/GlobalRouteValue/AboutRouteValue` でサンプルの [About] ページを要求し、その結果を調べます。

![[About] ページは、GlobalRouteValue と AboutRouteValue のルート セグメントで要求されます。 レンダリングされたページは、ルート データの値がページの OnGet メソッドでキャプチャされたことを示しています。](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>ページ ルートの構成

[AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) を使用して、特定のページ パスでページへのルートを構成します。 そのページに対して生成されたリンクでは、指定したルートを使用します。 `AddPageRoute` では、`AddPageRouteModelConvention` を使用してルートを確立します。

サンプル アプリでは、*Contact.cshtml* の `/TheContactPage` へのルートを作成します。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

[Contact] ページには、既定のルート経由の `/Contact` でアクセスすることもできます。

サンプル アプリの [Contact] ページに対するカスタム ルートでは、省略可能な `text` ルート セグメント (`{text?}`) を許可します。 また、訪問者が `/Contact` ルートでページにアクセスする場合、ページの `@page` ディレクティブにはこの省略可能なセグメントも含まれます。

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

レンダリングされたページの **Contact** リンク用に生成された URL には、更新されたルートが反映されることに注意してください。

![ナビゲーション バーのサンプル アプリの [Contact] リンク](razor-pages-convention-features/_static/contact-link.png)

![レンダリングされた HTML の [Contact] リンクを調べると、href に '/TheContactPage' が設定されています](razor-pages-convention-features/_static/contact-link-source.png)

通常のルート (`/Contact`) またはカスタム ルート (`/TheContactPage`) のいずれかで、[Contact] ページにアクセスします。 追加の `text` ルート セグメントを指定した場合、ページには指定した HTML エンコードのセグメントが示されます。

![URL の 'TextValue' の省略可能な 'text' ルート セグメントを適用する Microsoft Edge ブラウザーの例。 レンダリングされたページには、'text' セグメントの値が示されています。](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>ページ モデル アクション規則

[IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) を実装する既定のページ モデル プロバイダーは、ページ モデルを構成するための拡張ポイントを提供するようにデザインされた規則を呼び出します。 これらの規則は、ページ検出をビルドおよび変更したり、機能を処理したりするときに便利です。

このセクションの例の場合、サンプル アプリでは、応答ヘッダーを適用する [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute) である、`AddHeaderAttribute` クラスを使用します。

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

規則を使用して、このサンプルでは、フォルダー内のすべてのページおよび単一ページに属性を適用する方法のデモを実行します。

**フォルダー アプリ モデル規則**

[AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) を使用して、指定したフォルダーにあるすべてのページの [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) インスタンスのアクションを呼び出す、[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) を作成して追加します。

このサンプルでは、ヘッダー (`OtherPagesHeader`) をアプリの *OtherPages* フォルダー内にあるページに追加して、`AddFolderApplicationModelConvention` を使用するデモを実行します。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

`localhost:5000/OtherPages/Page1` でサンプルの Page1 ページを要求し、そのヘッダーを調べて結果を確認します。

![OtherPages/Page1 ページの応答ヘッダーは、OtherPagesHeader が追加されていることを示しています。](razor-pages-convention-features/_static/page1-otherpages-header.png)

**ページ アプリ モデル規則**

[AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) を使用して、指定した名前でページの [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) のアクションを呼び出す、[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) を作成して追加します。

サンプルでは、ヘッダー (`AboutHeader`) を [About] ページに追加して、`AddPageApplicationModelConvention` を使用するデモを実行します。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

`localhost:5000/About` でサンプルの [About] ページを要求し、そのヘッダーを調べて結果を確認します。

![[About] ページの応答ヘッダーは、AboutHeader が追加されたことを示しています。](razor-pages-convention-features/_static/about-page-about-header.png)

**フィルターの構成**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) では、指定したフィルターを適用するように構成します。 フィルター クラスを実装できますが、サンプル アプリでは、ラムダ式でフィルターを実装する方法を示しています。これは、フィルターを返すファクトリとしてバックグラウンドで実装されます。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

ページ アプリ モデルは、*OtherPages* フォルダーの Page2 ページにつながるセグメントの相対パスを確認するために使用されます。 条件を満たすと、ヘッダーが追加されます。 満たさない場合は、`EmptyFilter` が適用されます。

`EmptyFilter` は[アクション フィルター](xref:mvc/controllers/filters#action-filters)です。 アクション フィルターは Razor ページによって無視されるため、パスに `OtherPages/Page2` が含まれない場合は、意図されたように `EmptyFilter` は操作されません。

`localhost:5000/OtherPages/Page2` でサンプルの Page2 ページを要求し、そのヘッダーを調べて結果を確認します。

![OtherPagesPage2Header は、Page2 への応答に追加されます。](razor-pages-convention-features/_static/page2-filter-header.png)

**フィルター ファクトリの構成**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) では、[フィルター](xref:mvc/controllers/filters)をすべての Razor ページに適用するように、指定したファクトリを構成します。

サンプル アプリでは、アプリのページに対する 2 つの値と共にヘッダー (`FilterFactoryHeader`) を追加して、[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)を使用する例を提供します。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

`localhost:5000/About` でサンプルの [About] ページを要求し、そのヘッダーを調べて結果を確認します。

![[About] ページの応答ヘッダーは、FilterFactoryHeader ヘッダーが 2 つ追加されたことを示しています。](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>既定のページ アプリ モデル プロバイダーを置き換える

Razor ページでは、`IPageApplicationModelProvider` インターフェイスを使用して、[DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider) を作成します。 既定のモデル プロバイダーから継承し、ハンドラーの検出と処理に独自の実装ロジックを指定することができます。 既定の実装 ([参照ソース](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) では、以下に示すように、*名前なし*と*名前付き*の名前付けハンドラーの規則を確立します。

**既定の名前なしハンドラー メソッド**

HTTP 動詞のハンドラー メソッド ("名前なし" ハンドラー メソッド) は、`On<HTTP verb>[Async]` の規則に従います (`Async` の追加は省略可能ですが、非同期メソッドの場合は推奨されます)。

| 名前なしハンドラー メソッド     | 操作                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | ページの状態を初期化します。     |
| `OnPost`/`OnPostAsync`     | POST 要求を処理します。          |
| `OnDelete`/`OnDeleteAsync` | DELETE 要求を処理します&#8224;。 |
| `OnPut`/`OnPutAsync`       | PUT 要求を処理します&#8224;。    |
| `OnPatch`/`OnPatchAsync`   | PATCH 要求を処理します&#8224;。  |

&#8224;ページへの API 呼び出しを作成するために使用されます。

**既定の名前付きハンドラー メソッド**

開発者 ("名前付き" ハンドラー メソッド) によって指定されたハンドラー メソッドは、同様の規則に従います。 ハンドラー名は HTTP 動詞の後、または HTTP 動詞と `Async`: `On<HTTP verb><handler name>[Async]` (`Async` の追加は省略可能ですが、非同期メソッドの場合は推奨されます) の間に表示されます。 たとえば、メッセージを処理するメソッドは、以下の表に示されている名前指定を取得する可能性があります。

| 名前付きハンドラー メソッドの例             | 操作の例        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | メッセージを取得します。        |
| `OnPostMessage`/`OnPostMessageAsync`     | メッセージを投稿します。          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | メッセージを削除します&#8224;。 |
| `OnPutMessage`/`OnPutMessageAsync`       | メッセージを配置します&#8224;。    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | メッセージを修正します&#8224;。  |

&#8224;ページへの API 呼び出しを作成するために使用されます。

**ハンドラー メソッド名をカスタマイズする**

名前なしハンドラー メソッドと名前付きハンドラー メソッドに名前を付ける方法を変更するとします。 別の名前付けスキームは、メソッド名が "On" で始まることを避けて、最初の単語セグメントを使用して HTTP 動詞を決定するためのものです。 DELETE、PUT、PATCH の動詞を POST に変換するなど、その他の変更を行うことができます。 このようなスキームは、次の表に示すようなメソッド名を指定します。

| ハンドラー メソッド                       | 操作                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | ページの状態を初期化します。     |
| `Post`/`PostAsync`                   | POST 要求を処理します。          |
| `Delete`/`DeleteAsync`               | DELETE 要求を処理します&#8224;。 |
| `Put`/`PutAsync`                     | PUT 要求を処理します&#8224;。    |
| `Patch`/`PatchAsync`                 | PATCH 要求を処理します&#8224;。  |
| `GetMessage`                         | メッセージを取得します。              |
| `PostMessage`/`PostMessageAsync`     | メッセージを投稿します。                |
| `DeleteMessage`/`DeleteMessageAsync` | 削除するメッセージを投稿します。      |
| `PutMessage`/`PutMessageAsync`       | 配置するメッセージを投稿します。         |
| `PatchMessage`/`PatchMessageAsync`   | 修正するメッセージを投稿します。       |

&#8224;ページへの API 呼び出しを作成するために使用されます。

このスキームを確立するには、`DefaultPageApplicationModelProvider` クラスから継承して [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) メソッドを上書きし、カスタム ロジックを適用して [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) ハンドラー名を解決します。 サンプル アプリは、その `CustomPageApplicationModelProvider` クラスでこれがどのように行われるかを示します。

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

クラスの主な内容は次のとおりです。

* クラスは、`DefaultPageApplicationModelProvider` を継承します。
* `PageHandlerModel` を作成するときに、`TryParseHandlerMethod` は HTTP 動詞 (`httpMethod`) と名前付きハンドラー名 (`handlerName`) を決定するハンドラーを処理します。
  * 存在する場合、`Async` の後置形式は無視されます。
  * メソッド名から HTTP 動詞を解析するために、文字種が使用されます。
  * メソッド名 (`Async` なし) が HTTP 動詞名と同じ場合、名前付きハンドラーはありません。 `handlerName` に `null` が設定され、メソッド名は `Get`、`Post`、`Delete`、`Put`、または `Patch` になります。
  * メソッド名 (`Async` なし) が HTTP 動詞名より長い場合、名前付きハンドラーは存在します。 `handlerName` に `<method name (less 'Async', if present)>` が設定されています。 たとえば、"GetMessage" と "GetMessageAsync" は両方、"GetMessage" のハンドラー名を使用します。
  * DELETE、PUT、および PATCH HTTP 動詞は、POST に変換されます。

`Startup` クラスに `CustomPageApplicationModelProvider` を登録します。

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

*Index.cshtml.cs* のページ モデルは、通常のハンドラー メソッドの名前付け規則が、アプリのページに対してどのように変更されるかを示します。 Razor ページで使用される通常の "On" プレフィックスの名前付けは削除されます。 ページの状態を初期化するメソッドは、`Get` という名前になりました。 いずれかのページでいずれかのモデルを開くと、アプリ全体で使用されるこの規則を表示できます。

その他の各メソッドは、その処理について説明する HTTP 動詞で始まります。 通常、`Delete` で開始する 2 つのメソッドは、DELETE HTTP 動詞として処理されますが、`TryParseHandlerMethod` のロジックは、明示的に両方のハンドラーの動詞を POST に設定します。

`Async` は、`DeleteAllMessages` と `DeleteMessageAsync` の間で省略可能であることに注意してください。 これらは両方、非同期メソッドですが、`Async` の後置形式を使用するかどうかを選択できます。使用することをお勧めします。 ここでは、`DeleteAllMessages` はデモンストレーション目的で使用されますが、メソッド `DeleteAllMessagesAsync` のように名前を付けることをお勧めします。 これはサンプルの実装の処理に影響しませんが、`Async` の後置形式を使用して、非同期メソッドというファクトを呼び出します。

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

*Index.cshtml* で指定されたハンドラー名は、`DeleteAllMessages` と `DeleteMessageAsync` ハンドラー メソッドに一致することに注意してください。

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

ハンドラー メソッド名 `DeleteMessageAsync` の `Async` は、メソッドへの POST 要求に一致するハンドラーの `TryParseHandlerMethod` によって除外されます。 `DeleteMessage` の `asp-page-handler` の名前は、ハンドラー メソッド `DeleteMessageAsync` に一致します。

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC フィルターとページ フィルター (IPageFilter)

Razor ページはハンドラー メソッドを使用するため、MVC [アクション フィルター](xref:mvc/controllers/filters#action-filters)は Razor ページによって無視されます。 その他の型の MVC フィルターは、[承認](xref:mvc/controllers/filters#authorization-filters)、[例外](xref:mvc/controllers/filters#exception-filters)、[リソース](xref:mvc/controllers/filters#resource-filters)、および[結果](xref:mvc/controllers/filters#result-filters)を使用するために利用できます。 詳細については、「[フィルター](xref:mvc/controllers/filters)」トピックを参照してください。

ページ フィルター ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) は、Razor ページに適用されるフィルターです。 このフィルターで、ページ ハンドラー メソッドの実行を囲みます。 これにより、ページ ハンドラー メソッドの実行のステージでカスタムのコードを処理できます。 サンプル アプリの例は次のとおりです。

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

このフィルターは、"TriggerValue" の `globalTemplate` ルート値をチェックし、"ReplacementValue" で入れ替えます。

`ReplaceRouteValueFilter` 属性は `PageModel` に直接適用できます。

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

`localhost:5000/OtherPages/Page3/TriggerValue` でサンプル アプリから Page3 ページを要求します。 フィルターでどのようにルート値を置き換えるのかがわかります。

![TriggerValue ルート セグメントを使った OtherPages/Page3 への要求は、ルートの値を ReplacementValue に置き換えるフィルターになります。](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a>関連項目

* [Razor ページの承認規則](xref:security/authorization/razor-pages-authorization)
