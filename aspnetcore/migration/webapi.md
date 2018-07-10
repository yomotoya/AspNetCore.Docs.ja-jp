---
title: ASP.NET Web API から ASP.NET Core に移行します。
author: ardalis
description: ASP.NET Web API から ASP.NET Core mvc Web API の実装を移行する方法について説明します。
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894193"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>ASP.NET Web API から ASP.NET Core に移行します。

作成者: [Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com)

Web Api は、さまざまなブラウザーやモバイル デバイスなどのクライアントに提供される HTTP サービスです。 ASP.NET Core MVC には、1 つで一貫性のある web アプリケーションの構築方法を提供する Web Api を構築するためのサポートが含まれています。 この記事では、ASP.NET Web API から ASP.NET Core mvc Web API の実装を移行するために必要な手順を示します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="review-aspnet-web-api-project"></a>ASP.NET Web API プロジェクトのレビュー

この記事では、サンプル プロジェクトを使用して*ProductsApp*、情報の記事で作成された[ASP.NET Web API 2 の概要](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)を基盤とします。 そのプロジェクトでは、単純な ASP.NET Web API プロジェクトを次のように構成します。

*Global.asax.cs*、呼び出しに`WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` 定義されている*App_Start*、1 つだけの静的であり`Register`メソッド。

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

このクラスを構成します[属性ルーティング](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)、実際には、プロジェクトで使用されています。 ASP.NET Web API で使用される、ルーティング テーブルも構成します。 この場合、ASP.NET Web API の形式と一致する Url が期待 */api/{controller}/{id}* で *{id}* で省略可能します。

*ProductsApp*プロジェクトから継承される 1 つだけの単純なコントロールに含まれる`ApiController`と 2 つのメソッドを公開します。

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

最後に、次のようなモデルがあると、*製品*によって使用される、 *ProductsApp*、単純なクラスには。

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

あるので、単純なプロジェクトを開始するには、ASP.NET Core MVC にこの Web API プロジェクトを移行する方法を紹介します。

## <a name="create-the-destination-project"></a>対象のプロジェクトを作成します。

Visual Studio を使用して、新しい空のソリューションを作成し、名前*WebAPIMigration*します。 既存の追加*ProductsApp*プロジェクトし、新しい ASP.NET Core Web アプリケーション プロジェクトをソリューションに追加します。 新しいプロジェクトの名前*ProductsCore*します。

![Web テンプレートに新しいプロジェクト ダイアログ ボックスを開く](webapi/_static/add-web-project.png)

次に、Web API プロジェクト テンプレートを選択します。 移行は、 *ProductsApp*内容をこの新しいプロジェクトをします。

![Web API プロジェクト テンプレートの ASP.NET Core テンプレートの一覧で選択されている新しい Web アプリケーション ダイアログ](webapi/_static/aspnet-5-webapi.png)

削除、`Project_Readme.html`新しいプロジェクトからファイル。 ソリューションは、次のようになります。

![ProductsApp と ProductsCore プロジェクトのファイルとフォルダーを表示したソリューション エクスプ ローラーでアプリケーション ソリューションを開く](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>構成を移行します。

ASP.NET Core を使用しなく*Global.asax*、 *web.config*、または*App_Start*フォルダー。 すべてのスタートアップ タスクを実行する代わりに、 *Startup.cs*プロジェクトのルート (を参照してください[アプリケーションの起動](../fundamentals/startup.md))。 ASP.NET Core mvc で属性ベースのルーティングに組み込まれた既定と`UseMvc()`が呼び出されます。 および、この Web API ルートを構成するための推奨アプローチです (とは、Web API のスターター プロジェクトがルーティングを処理する方法)。

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

今後、プロジェクト内の属性ルーティングを使用すると仮定すると、追加の構成は必要ありません。 サンプルで行われるように、コント ローラーとアクション、必要な属性を適用`ValuesController`Web API のスタート プロジェクトに含まれているクラス。

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

存在に注意してください *[controller]* 8 行目にします。 など特定のトークンをサポートするようになりました属性ベースのルーティング *[controller]* と *[アクション]* します。 これらのトークンに置き換えられます実行時に、コント ローラーまたはアクションの名前、属性が適用されています。 これには、プロジェクト魔法の文字列の数を減らすし、ルートと同期されている、対応するコント ローラーとアクション自動名前変更リファクタリングを適用した場合になります。

製品の API コント ローラーを移行する必要がありますまずコピー *ProductsController*新しいプロジェクトにします。 コント ローラーのルート属性を含めるだけです。

```csharp
[Route("api/[controller]")]
```

追加する必要があります、`[HttpGet]`両方は、HTTP Get を使用して呼び出す必要があるために、2 つのメソッドに属性します。 属性に"id"パラメーターの想定を含める`GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

この時点では、ルーティングを正しく構成します。ただし、私たちできませんまだテストします。 前に、追加の変更を行う必要があります*ProductsController*コンパイルされます。

## <a name="migrate-models-and-controllers"></a>モデルとコント ローラーを移行します。

この単純な Web API プロジェクトの移行プロセスの最後の手順では、コント ローラーとすべてのモデルを使用して、それらをコピーします。 この場合は、単にコピー *Controllers/ProductsController.cs*から元のプロジェクトに新しいものです。 新しいものを元のプロジェクトからモデル フォルダー全体をコピーしてください。 新しいプロジェクト名と一致する名前空間を調整 (*ProductsCore*)。  この時点で、アプリケーションをビルドして、コンパイル エラーの数が表示されます。 これらは一般に、次のカテゴリに分類されます。

* *ApiController*が存在しません。

* *System.Web.Http*名前空間が存在しません

* *IHttpActionResult*が存在しません。

さいわい、これらはすべて非常に簡単に修正です。

* 変更*ApiController*に*コント ローラー* (追加する必要があります*Microsoft.AspNetCore.Mvc を使用して*)

* 削除のいずれかを参照するステートメントを使用して*System.Web.Http*

* 変更するメソッドを返す*IHttpActionResult*を返す、 *IActionResult*

これらの変更が加えられた未使用とステートメントを使用して削除されると、移行された*ProductsController*クラスは次のようになります。

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

移行されたプロジェクトを実行しを参照できるはず*api/製品*; と、3 つの製品の一覧を表示する必要があります。 参照する */api/products/1*と最初の製品を表示する必要があります。

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a>ASP.NET 4.x Web API 2 の互換性 shim

ASP.NET core プロジェクトを ASP.NET Web API を移行するときに便利なツールは、 [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)ライブラリ。 互換性 shim は、ASP.NET Core を使用する Web API 2 とは異なる規則の数を指定できるを拡張します。 このドキュメントで既に移植サンプルは、基本互換性 shim は必要なかったことです。 大規模なプロジェクトは、互換性 shim を使用してできる一時的に ASP.NET Core と ASP.NET Web API 2 API のギャップを埋めるに便利です。

Web API の互換性 shim は、ASP.NET Core への大規模な Web API プロジェクトの移行を容易に一時的な措置として使用します。 時間の経過と共に ASP.NET Core のパターンを使用して、互換性 shim を使用する代わりにプロジェクトを更新する必要があります。

含まれる互換性機能`Microsoft.AspNetCore.Mvc.WebApiCompatShim`が含まれます。

* 追加、`ApiController`コント ローラーの基本型を更新する必要があるように入力します。
* Web API スタイルのモデル バインドを有効にします。 既定では、MVC 5 と同様に ASP.NET Core MVC モデル バインディング機能します。 互換性 shim の変更はモデル バインドを Web API 2 のモデル バインドの規則に似ています。 たとえば、複合型は、要求本文から自動的にバインドされます。
* コント ローラー アクションは、型のパラメーターを実行できるように、モデル バインドを拡張`HttpRequestMessage`します。
* 型の結果を返すアクションを許可するメッセージ フォーマッタを追加します。`HttpResponseMessage`します。
* Web API 2 の操作が応答を処理するために使用する追加の応答メソッドを追加します。
  * HttpResponseMessage ジェネレーター。
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * 結果のアクション メソッド:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* インスタンスを追加します`IContentNegotiator`アプリの DI コンテナーとコンテンツのネゴシエーションに関連する型から[Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)使用できます。 ような種類が含まれます`DefaultContentNegotiator`、`MediaTypeFormatter`など。

互換性 shim を使用する必要があります。

* インストール、 [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet パッケージ。
* 互換性 shim のサービスを呼び出すことによって、アプリの DI コンテナーを登録`services.AddMvc().AddWebApiConventions()`アプリの`Startup.ConfigureServices`メソッド。
* 使用して Web API に固有のルートを定義`MapWebApiRoute`上、`IRouteBuilder`アプリの`IApplicationBuilder.UseMvc`呼び出します。

## <a name="summary"></a>まとめ

ASP.NET Core MVC への単純な ASP.NET Web API プロジェクトの移行はとても簡単です、ありがとう Web Api ASP.NET Core MVC での組み込みサポートをします。 移行する必要がありますすべての ASP.NET Web API プロジェクトの主要部分は、ルート、コント ローラー、およびモデルでは、コント ローラーとアクションで使用される型の更新とは。
