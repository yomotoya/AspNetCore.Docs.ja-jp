---
title: ASP.NET Core で Web API を構築する
author: scottaddie
description: ASP.NET Core で Web API を構築するために使用できる機能、および各機能を使用する適切なタイミングについて説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: 763b95fb8ed3806bc67b7ad199153ea1027efa57
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090421"
---
# <a name="build-web-apis-with-aspnet-core"></a>ASP.NET Core で Web API を構築する

作成者: [Scott Addie](https://github.com/scottaddie)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

このドキュメントでは、ASP.NET Core での各機能を使用する最も適切な web API をビルドする方法について説明します。

## <a name="derive-class-from-controllerbase"></a>ControllerBase からクラスを派生する

Web API として機能することを目的としたコント ローラー内の <xref:Microsoft.AspNetCore.Mvc.ControllerBase> クラスから継承します。 例:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

`ControllerBase` クラスを使用すると、いくつかのプロパティとメソッドにアクセスできます。 上のコードでは、例に <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> と <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)> が含まれています。 これらのメソッドはアクション メソッド内で呼び出され、HTTP 400 および 201 のステータス コードをそれぞれ返します。 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> プロパティ (これも `ControllerBase` によって提供される) は、要求モデル検証を処理する場合にアクセスされます。

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>ApiControllerAttribute でクラスに注釈を付ける

ASP.NET Core 2.1 では、Web API コントローラー クラスを表す [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 属性が導入されました。 例:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

この属性を使用するには、2.1 以降の互換性バージョン (<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> で設定) が必須です。 たとえば、*Startup.ConfigureServices* の強調表示されているコードでは、2.2 の互換性フラグが設定されます。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

詳細については、「<xref:mvc/compatibility-version>」を参照してください。

コントローラーの REST 固有の動作を有効にする場合、通常、`[ApiController]` 属性は `ControllerBase` と組み合わせて使用されます。 `ControllerBase` では、<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> や <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> などのメソッドにアクセスできます。

その他に、`[ApiController]` 属性で注釈が付けられたユーザー定義の基本コントローラー クラスを作成するという方法があります。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

次のセクションでは、属性によって追加される便利な機能について説明します。

### <a name="problem-details-responses-for-error-status-codes"></a>エラー状態コードに対する問題の詳細の応答

ASP.NET Core 2.1 以降には、[RFC 7807 仕様](https://tools.ietf.org/html/rfc7807)に基づいた型 [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails) が含まれています。 `ProblemDetails` 型は機械で読み取り可能なエラーの詳細を HTTP 応答で伝えるための標準化された形式を提供します。

ASP.NET Core 2.2 以降では、MVC がエラー状態コードの結果 (状態コード 400 以上) を `ProblemDetails` による結果に変換します。 次のコードがあるとします。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

`NotFound` の結果の HTTP 応答には、404 状態コードと次のような `ProblemDetails` の本文が含まれています。

```js
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

問題の詳細機能には、2.2 以降の互換性フラグが必要です。 [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> プロパティが `true` に設定されている場合、既定の動作は無効になります。 `Startup.ConfigureServices` の次の強調表示されたコードを使用すると、問題の詳細が無効になります。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

`ProblemDetails` 応答の内容を構成するには、[ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> プロパティを使用します。 たとえば、次のコードにより、404 応答の `type` プロパティが更新されます。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a>自動的な HTTP 400 応答

検証エラーが発生すると、HTTP 400 応答が自動的にトリガーされます。 次のコードは実際のアクションでは不要になります。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

結果として発生する応答の出力をカスタマイズするには、<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> を使用します。

<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> プロパティが `true` に設定されている場合、既定の動作は無効になります。 *Startup.ConfigureServices* の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後に次のコードを追加します。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

2.2 以降の互換性フラグを設定した場合の、400 応答に対して返される既定の応答の種類は、<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> です。 ASP.NET Core 2.1 エラー形式を使用するには、[SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> プロパティを使用します。

### <a name="binding-source-parameter-inference"></a>バインディング ソース パラメーター推論

バインディング ソース属性では、アクション パラメーターの値が存在する場所が定義されます。 次のバインディング ソース属性が存在します。

|属性|バインド ソース |
|---------|---------|
|**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**     | 要求本文 |
|**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**     | 要求本文内のフォーム データ |
|**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)** | 要求ヘッダー |
|**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**   | 要求のクエリ文字列パラメーター |
|**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**   | 現在の要求からのルート データ |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | アクション パラメーターとして挿入される要求サービス |

> [!WARNING]
> 値に `%2f` (つまり、`/`) が含まれる可能性がある場合は、`[FromRoute]` を使用しないでください。 `%2f` は `/` にエスケープ解除されません。 値に `%2f` が含まれる可能性がある場合は、`[FromQuery]` を使用してください。

`[ApiController]` 属性がない場合は、バインディング ソース属性を明示的に定義します。 次の例では、`discontinuedOnly` パラメーター値が要求 URL のクエリ文字列に指定されていることが `[FromQuery]` によって示されています。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

アクション パラメーターの既定のデータ ソースには推論規則が適用されます。 これらの規則によって、アクション パラメーターに、通常、手動で適用する可能性の高いバインディング ソースが構成されます。 バインディング ソースの属性は、次のように動作します。

* **[FromBody]** は複合型のパラメーターに対して推論されます。 この規則には例外があり、<xref:Microsoft.AspNetCore.Http.IFormCollection> や <xref:System.Threading.CancellationToken> などの、特殊な意味を持つ複雑な組み込み型が該当します。 バインディング ソース推論コードでは、そのような特殊な型は無視されます。 `[FromBody]` は、`string` や `int` などの単純型に対しては推論されません。 そのため、その機能が必要な場合、単純型に対しては `[FromBody]` 属性を使用する必要があります。 アクションの複数のパラメーターが明示的に指定されている場合 (`[FromBody]` によって) または要求本文からバインドとして推論される場合は、例外がスローされます。 たとえば、次のアクション シグネチャは例外の原因となります。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** は <xref:Microsoft.AspNetCore.Http.IFormFile> および <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 型のアクション パラメーターに対して推論されます。 簡易型またはユーザー定義型に対しては推論されません。
* **[FromRoute]** は、ルート テンプレート内のパラメーターと一致する任意のアクション パラメーター名に対して推論されます。 複数のルートがアクション パラメーターと一致する場合、ルート値はいずれも `[FromRoute]` と見なされます。
* **[FromQuery]** は他の任意のアクション パラメーターに対して推論されます。

<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> プロパティが `true` に設定されている場合、既定の推論規則は無効になります。 *Startup.ConfigureServices* の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後に次のコードを追加します。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>マルチパート/フォーム データ要求の推論

[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) 属性を使用してアクション パラメーターに注釈を付けた場合は、`multipart/form-data` 要求コンテンツ型が推論されます。

<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> プロパティが `true` に設定されている場合、既定の動作は無効になります。 *Startup.ConfigureServices* の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後に次のコードを追加します。

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>属性ルーティング要件

属性ルーティングは要件になります。 例:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> 内で定義されているか、または *Startup.Configure* 内の <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> によって定義されている[従来のルート](xref:mvc/controllers/routing#conventional-routing)を通してアクションにアクセスすることはできません。

::: moniker-end

## <a name="additional-resources"></a>その他の技術情報

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>