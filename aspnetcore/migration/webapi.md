---
title: ASP.NET Web API から ASP.NET Core への移行します。
author: ardalis
description: ASP.NET Web API からの Web API の実装を ASP.NET のコアの MVC に移行する方法について説明します。
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 9385805d548bc87f4a50b87f2c06aa74abdaf8af
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272531"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>ASP.NET Web API から ASP.NET Core への移行します。

作成者: [Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com)

Web Api は、さまざまなブラウザーやモバイル デバイスを含む、クライアントに到達する HTTP サービスです。 ASP.NET Core MVC には、1 つ、一貫性のある web アプリケーションの構築方法を提供する Web Api を構築するためのサポートが含まれています。 この記事では、ASP.NET Web API から ASP.NET Core MVC に Web API の実装を移行するために必要な手順について説明します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="review-aspnet-web-api-project"></a>レビュー ASP.NET Web API プロジェクト

この資料は、サンプル プロジェクトを使用して*ProductsApp*の記事で作成された、[基本 ASP.NET Web API の 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)の開始点として。 そのプロジェクトで単純な ASP.NET Web API プロジェクトが次のように構成します。

*Global.asax.cs*への呼び出しが行われた`WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` 定義された*App_Start*であり、1 つの静的`Register`メソッド。

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


このクラスを構成[属性がルーティング](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)は実際には、プロジェクトで使用されていますが、します。 また、ASP.NET Web API で使用される、ルーティング テーブルを構成します。 ASP.NET Web API が形式と一致する Url を期待するこの例では、 */api/{controller}/{id}* で *{id}* される省略可能です。

*ProductsApp*プロジェクトに含まれる 1 つだけ単純なコント ローラーから継承される`ApiController`と 2 つのメソッドを公開します。

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

最後に、次のようなモデルがあると、*製品*で使用される、 *ProductsApp*、単純なクラスには。

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

これでが単純なプロジェクトから起動するときに、ASP.NET Core MVC にこの Web API プロジェクトを移行する方法について説明します。

## <a name="create-the-destination-project"></a>コピー先のプロジェクトを作成します。

Visual Studio を使用して、新しい、空のソリューションを作成し、名前を付けます*WebAPIMigration*です。 既存の追加*ProductsApp*プロジェクトを次に、新しい ASP.NET Core Web アプリケーション プロジェクトをソリューションに追加します。 新しいプロジェクトの名前*ProductsCore*です。

![Web テンプレートを新しいプロジェクト ダイアログ ボックスを開く](webapi/_static/add-web-project.png)

次に、Web API プロジェクト テンプレートを選択します。 移行される、 *ProductsApp*内容をこの新しいプロジェクトをします。

![ASP.NET Core テンプレートの一覧で選択されている Web API プロジェクト テンプレートを使用して新しい Web アプリケーションのダイアログ](webapi/_static/aspnet-5-webapi.png)

削除、`Project_Readme.html`新しいプロジェクトからファイルです。 ソリューションは、次のようになります。

![ソリューション エクスプ ローラーで、ProductsApp および ProductsCore プロジェクトのファイルとフォルダーを示すアプリケーション ソリューションを開く](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>構成を移行します。

ASP.NET Core を使用しない*Global.asax*、 *web.config*、または*App_Start*フォルダーです。 すべてのスタートアップ タスクを実行する代わりに、 *Startup.cs* 、プロジェクトのルートで (を参照してください[アプリケーションの起動](../fundamentals/startup.md))。 ASP.NET Core mvc で属性ベースのルーティングはここで既定で含まれてとき`UseMvc()`が呼び出されます。 および、この Web API ルートを構成するための推奨アプローチです (とは、Web API のスタート プロジェクトがルーティングを処理する方法)。

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

属性は、プロジェクトの今後のルーティングを使用すると仮定した場合、追加の構成は必要ありません。 サンプルで行われるように、コント ローラーとアクションに必要な属性を適用`ValuesController`Web API のスタート プロジェクトに含まれているクラス。

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

存在することに注意してください *[コント ローラー]* 8 行です。 これで属性ベースのルーティングをサポートしている特定のトークンなど *[コント ローラー]* と *[アクション]* です。 これらのトークンは置き換えられます実行時に、コント ローラーまたはアクションの名前でそれぞれ、属性が適用されています。 これには、プロジェクトのマジック文字列の数を減らすし、ルート保持される自動名前の変更リファクタリングを適用するときに、対応するコント ローラーとアクションで同期されていることを確認します。

製品の API コント ローラーを移行する必要があります最初にコピー *ProductsController*は新しいプロジェクトにします。 コント ローラーのルート属性を単に含めます。

```csharp
[Route("api/[controller]")]
```

追加する必要があります、`[HttpGet]`両者は、HTTP Get 経由で呼び出す必要がありますので、2 つのメソッドでは、属性します。 属性に"id"パラメーターの期待値を含める`GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

この時点では、ルーティングが正しく構成されています。ただし、できませんまだテストすることです。 追加する必要があります変更する前に*ProductsController*コンパイルされます。

## <a name="migrate-models-and-controllers"></a>モデルとコント ローラーを移行します。

この単純な Web API プロジェクトの移行プロセスの最後の手順では、コント ローラーおよびすべてのモデルを使用して経由でコピーします。 この場合、単にコピー *Controllers/ProductsController.cs*に元のプロジェクトです。 新しいゲートウェイに、元のプロジェクトからモデル フォルダー全体をコピーしてください。 新しいプロジェクト名と一致する名前空間を調整 (*ProductsCore*)。  この時点で、アプリケーションをビルドすることができ、コンパイル エラーの数が表示されます。 一般に、次のカテゴリに分類これら必要があります。

* *ApiController*存在しません

* *System.Web.Http*名前空間が存在しません

* *IHttpActionResult*存在しません

幸いにも、これらはすべて非常に簡単に解決します。

* 変更*ApiController*に*コント ローラー* (追加する必要があります*Microsoft.AspNetCore.Mvc を使用して*)

* 削除を参照するステートメントを使用して*System.Web.Http*

* 変更するメソッドを返す*IHttpActionResult*を返す、 *IActionResult*

これらの変更が加えられた未使用ステートメントを使用して削除を移行された*ProductsController*クラスは、次のようになります。

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

移行されたプロジェクトを実行しを参照することができます */api 製品*; し、3 つの製品の完全な一覧を表示する必要があります。 参照 */api/products/1*と最初の製品を表示する必要があります。

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a>Microsoft.AspNetCore.Mvc.WebApiCompatShim

移行の ASP.NET Web API プロジェクト ASP.NET Core をときに便利なツールは、 [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)ライブラリです。 互換性 shim は、使用する別の Web API 2 規則の数を許可する ASP.NET Core を拡張します。 このドキュメントで以前に移植したサンプルは、基本互換性 shim が必要なことです。 大規模なプロジェクト互換性 shim を使用して役に立ちます一時的にあるギャップを埋める API ASP.NET Core と ASP.NET Web API 2 の間の。

Web API の互換性 shim は、ASP.NET Core に大規模な Web API プロジェクトの移行を容易にするために一時的な措置として使用するものです。 時間の経過と共にパターンを使用する ASP.NET Core 互換性 shim に依存せずにプロジェクトを更新する必要があります。 

Microsoft.AspNetCore.Mvc.WebApiCompatShim に含まれる互換性機能は次のとおりです。

* 追加、`ApiController`コント ローラーの基本データ型を更新する必要があるように入力します。
* Web API スタイル モデル バインディングを有効にします。 既定では、MVC 5 と同様に ASP.NET Core MVC モデル バインディング機能します。 互換性 shim の変更は、ある Web API 2 モデル バインディング規則に類似したバインドをモデルします。 たとえば、複合型は、要求本文から自動的にバインドされます。
* モデル バインディングを拡張できるように、型のパラメーターを受け取り、コント ローラーのアクション`HttpRequestMessage`です。
* 型の結果を返すための操作の結果メッセージ フォーマッタを追加`HttpResponseMessage`です。
* 応答を提供する Web API 2 の操作を使用した可能性がある追加の応答方法を追加します。
    * HttpResponseMessage ジェネレーター。
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * 結果のアクション メソッド。
        * `BadResuestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* インスタンスを追加`IContentNegotiator`アプリの DI コンテナーからの型のネゴシエーションに関連するコンテンツと[Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)使用可能な。 ような種類が含まれます`DefaultContentNegotiator`、 `MediaTypeFormatter`, などです。

互換性 shim を使用する必要があります。

* 参照、 [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet パッケージです。
* 呼び出すことによって、アプリの DI コンテナーとの互換性 shim のサービスを登録`services.AddWebApiConventions()`アプリケーションの`Startup.ConfigureServices`メソッドです。
* 使用して Web API に固有のルートを定義`MapWebApiRoute`上、`IRouteBuilder`アプリケーションの`IApplicationBuilder.UseMvc`呼び出します。

## <a name="summary"></a>まとめ

ASP.NET Core MVC への単純な ASP.NET Web API プロジェクトの移行は、かなり簡単ありがとう ASP.NET Core MVC の Web Api の組み込みサポートです。 すべての ASP.NET Web API プロジェクトは移行する必要があります、メインの部分では、ルート、コント ローラー、およびモデルでは、コント ローラーとアクションで使用される型への更新プログラムと共にがします。
