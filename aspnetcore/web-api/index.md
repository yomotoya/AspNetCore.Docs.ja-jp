---
title: ASP.NET Core で Web API を構築する
author: scottaddie
description: ASP.NET Core で Web API を構築するために使用できる機能、および各機能を使用する適切なタイミングについて説明します。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: web-api/index
ms.openlocfilehash: 6afc02c1a966b62d0fcead0349c5f0803309dcbb
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
ms.locfileid: "33972788"
---
# <a name="build-web-apis-with-aspnet-core"></a>ASP.NET Core で Web API を構築する

作成者: [Scott Addie](https://github.com/scottaddie)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

このドキュメントでは、ASP.NET Core での各機能を使用する最も適切な web API をビルドする方法について説明します。

## <a name="derive-class-from-controllerbase"></a>ControllerBase からクラスを派生する

Web API として機能することを目的としたコント ローラー内の [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) クラスから継承します。 例:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

`ControllerBase` クラスを使用すると、さまざまなプロパティおよびメソッドにアクセスできます。 前の例では、[BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) や [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) などがそのようなメソッドに該当します。 これらのメソッドはアクション メソッド内で呼び出され、HTTP 400 および HTTP 201 のステータス コードをそれぞれ返します。 [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) プロパティ (これも `ControllerBase` によって提供される) は、要求モデル検証を実行する場合にアクセスされます。

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a>ApiControllerAttribute でクラスに注釈を付ける

ASP.NET Core 2.1 では、Web API コントローラー クラスを表す [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 属性が導入されました。 例:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

この属性は、通常、有効なメソッドやプロパティにアクセスするために `ControllerBase` と組み合わせて使用されます。 `ControllerBase` では、[NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) や [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file) などのメソッドにアクセスできます。

その他に、`[ApiController]` 属性で注釈が付けられたユーザー定義の基本コントローラー クラスを作成するという方法があります。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

次のセクションでは、属性によって追加される便利な機能について説明します。

### <a name="automatic-http-400-responses"></a>自動的な HTTP 400 応答

検証エラーが発生すると、HTTP 400 応答が自動的にトリガーされます。 次のコードは実際のアクションでは不要になります。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

この既定の動作は、*Startup.ConfigureServices* 内の次のコードで無効にされます。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>バインディング ソース パラメーター推論

バインディング ソース属性では、アクション パラメーターの値が存在する場所が定義されます。 次のバインディング ソース属性が存在します。

|属性|バインド ソース |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | 要求本文 |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | 要求本文内のフォーム データ |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | 要求ヘッダー |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | 要求のクエリ文字列パラメーター |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | 現在の要求からのルート データ |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | アクション パラメーターとして挿入される要求サービス |

> [!NOTE]
> `%2f` は `/` にエスケープ解除されないので、値に `%2f` (つまり `/`) が含まれる可能性がある場合は `[FromRoute]` を使用**しない**でください。 値に `%2f` が含まれる可能性がある場合は、`[FromQuery]` を使用してください。

`[ApiController]` 属性がない場合は、バインディング ソース属性を明示的に定義します。 次の例では、`discontinuedOnly` パラメーター値が要求 URL のクエリ文字列に指定されていることが `[FromQuery]` によって示されています。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

アクション パラメーターの既定のデータ ソースには推論規則が適用されます。 これらの規則によって、アクション パラメーターに、通常、手動で適用する可能性の高いバインディング ソースが構成されます。 バインディング ソースの属性は、次のように動作します。

* **[FromBody]** は複合型のパラメーターに対して推論されます。 この規則には例外があり、[IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) や [CancellationToken](/dotnet/api/system.threading.cancellationtoken) などの、特殊な意味を持つ複雑な組み込み型が該当します。 バインディング ソース推論コードでは、そのような特殊な型は無視されます。 アクションの複数のパラメーターが明示的に指定されている場合 (`[FromBody]` によって) または要求本文からバインドとして推論される場合は、例外がスローされます。 たとえば、次のアクション シグネチャは例外の原因となります。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** は [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) および [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection) 型のアクション パラメーターに対して推論されます。 簡易型またはユーザー定義型に対しては推論されません。
* **[FromRoute]** は、ルート テンプレート内のパラメーターと一致する任意のアクション パラメーター名に対して推論されます。 複数のルートがアクション パラメーターと一致する場合、ルート値はいずれも `[FromRoute]` とみなされます。
* **[FromQuery]** は他の任意のアクション パラメーターに対して推論されます。

既定の推論規則は、*Startup.ConfigureServices* 内の次のコードで無効にされます。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>マルチパート/フォーム データ要求の推論

[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) 属性を使用してアクション パラメーターに注釈を付けた場合は、`multipart/form-data` 要求コンテンツ型が推論されます。

既定の動作は、*Startup.ConfigureServices* 内の次のコードで無効にされます。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>属性ルーティング要件

属性ルーティングは要件になります。 例:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

[UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) 内で定義されているか、または *Startup.Configure* 内の [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) によって定義されている[従来のルート](xref:mvc/controllers/routing#conventional-routing)を通してアクションにアクセスすることはできません。
::: moniker-end

## <a name="additional-resources"></a>その他の技術情報

* [コントローラー アクションの戻り値の型](xref:web-api/action-return-types)
* [カスタム フォーマッタ](xref:web-api/advanced/custom-formatters)
* [応答データの書式設定](xref:web-api/advanced/formatting)
* [Swagger を使用するヘルプ ページ](xref:tutorials/web-api-help-pages-using-swagger)
* [コントローラー アクションへのルーティング](xref:mvc/controllers/routing)
