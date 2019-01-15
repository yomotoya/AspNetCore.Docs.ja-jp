---
title: ASP.NET Core で Web API を構築する
author: scottaddie
description: ASP.NET Core で Web API を構築するために使用できる機能、および各機能を使用する適切なタイミングについて説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/11/2019
uid: web-api/index
ms.openlocfilehash: a826bdecdd3a25eb23597123166695c169ba4229
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249439"
---
# <a name="build-web-apis-with-aspnet-core"></a>ASP.NET Core で Web API を構築する

作成者: [Scott Addie](https://github.com/scottaddie)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

このドキュメントでは、ASP.NET Core での各機能を使用する最も適切な web API をビルドする方法について説明します。

## <a name="derive-class-from-controllerbase"></a>ControllerBase からクラスを派生する

Web API として機能することを目的としたコント ローラー内の <xref:Microsoft.AspNetCore.Mvc.ControllerBase> クラスから継承します。 次に例を示します。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

`ControllerBase` クラスを使用すると、いくつかのプロパティとメソッドにアクセスできます。 上のコードでは、例に <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> と <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)> が含まれています。 これらのメソッドはアクション メソッド内で呼び出され、HTTP 400 および 201 のステータス コードをそれぞれ返します。 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> プロパティ (これも `ControllerBase` によって提供される) は、要求モデル検証を処理する場合にアクセスされます。

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontroller-attribute"></a>ApiController 属性を使用した注釈

ASP.NET Core 2.1 では、Web API コントローラー クラスを表す [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 属性が導入されました。 次に例を示します。

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

この属性をコントローラー レベルで使用するには、2.1 以降の互換性バージョン (<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> で設定) が必須です。 たとえば、`Startup.ConfigureServices` の強調表示されているコードでは、2.1 の互換性フラグが設定されます。

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

詳細については、「<xref:mvc/compatibility-version>」を参照してください。

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

ASP.NET Core 2.2 以降では、`[ApiController]` 属性をアセンブリに適用できます。 この方法での注釈では、アセンブリ内のすべてのコントローラーに Web API の動作が適用されます。 個々のコントローラーを有効にする方法はありません。 推奨事項として、アセンブリ レベルの属性を `Startup` クラスに適用する必要があります。

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

この属性をアセンブリ レベルで使用するには、2.2 以降の互換性バージョン (<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> で設定) が必須です。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

コントローラーの REST 固有の動作を有効にする場合、通常、`[ApiController]` 属性は `ControllerBase` と組み合わせて使用されます。 `ControllerBase` では、<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> や <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> などのメソッドにアクセスできます。

その他に、`[ApiController]` 属性で注釈が付けられたユーザー定義の基本コントローラー クラスを作成するという方法があります。

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

次のセクションでは、属性によって追加される便利な機能について説明します。

### <a name="automatic-http-400-responses"></a>自動的な HTTP 400 応答

モデル検証エラーが発生すると、HTTP 400 応答が自動的にトリガーされます。 その結果、次のコードは実際のアクションでは不要になります。

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

結果として発生する応答の出力をカスタマイズするには、<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> を使用します。

アクションがモデル検証エラーから回復できる場合は、既定の動作を無効にすると便利です。 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> プロパティが `true` に設定されている場合、既定の動作は無効になります。 `Startup.ConfigureServices` の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` の後ろに次のコードを追加します。

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

2.2 以降の互換性フラグを設定した場合の、HTTP 400 応答に対する既定の応答の種類は、<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> です。 `ValidationProblemDetails` 型は [RFC 7807 仕様](https://tools.ietf.org/html/rfc7807)に準拠しています。 代わりに ASP.NET Core 2.1 エラー形式の <xref:Microsoft.AspNetCore.Mvc.SerializableError> を返すようにするには、`SuppressUseValidationProblemDetailsForInvalidModelStateResponses` プロパティを `true` に設定します。 `Startup.ConfigureServices` に次のコードを追加します。

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2)
    .ConfigureApiBehaviorOptions(options =>
    {
        options
          .SuppressUseValidationProblemDetailsForInvalidModelStateResponses = true;
    });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

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

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

アクション パラメーターの既定のデータ ソースには推論規則が適用されます。 これらの規則によって、アクション パラメーターに、通常、手動で適用する可能性の高いバインディング ソースが構成されます。 バインディング ソースの属性は、次のように動作します。

* **[FromBody]** は複合型のパラメーターに対して推論されます。 この規則には例外があり、<xref:Microsoft.AspNetCore.Http.IFormCollection> や <xref:System.Threading.CancellationToken> などの、特殊な意味を持つ複雑な組み込み型が該当します。 バインディング ソース推論コードでは、そのような特殊な型は無視されます。 `[FromBody]` は、`string` や `int` などの単純型に対しては推論されません。 そのため、その機能が必要な場合、単純型に対しては `[FromBody]` 属性を使用する必要があります。 アクションの複数のパラメーターが明示的に指定されている場合 (`[FromBody]` によって) または要求本文からバインドとして推論される場合は、例外がスローされます。 たとえば、次のアクション シグネチャは例外の原因となります。

    [!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

    > [!NOTE]
    > ASP.NET Core 2.1 では、リストや配列などのコレクション型パラメーターが誤って [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) と推論されています。 これらのパラメーターを要求本文からバインドする場合は、[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) を使用する必要があります。 このビヘイビアーは、ASP.NET Core 2.2 以降で修正されており、既定ではコレクション型パラメーターが本文からバインドされることが推論されます。

* **[FromForm]** は <xref:Microsoft.AspNetCore.Http.IFormFile> および <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 型のアクション パラメーターに対して推論されます。 簡易型またはユーザー定義型に対しては推論されません。
* **[FromRoute]** は、ルート テンプレート内のパラメーターと一致する任意のアクション パラメーター名に対して推論されます。 複数のルートがアクション パラメーターと一致する場合、ルート値はいずれも `[FromRoute]` と見なされます。
* **[FromQuery]** は他の任意のアクション パラメーターに対して推論されます。

<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> プロパティが `true` に設定されている場合、既定の推論規則は無効になります。 `Startup.ConfigureServices` の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` の後ろに次のコードを追加します。

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a>マルチパート/フォーム データ要求の推論

[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) 属性を使用してアクション パラメーターに注釈を付けた場合は、`multipart/form-data` 要求コンテンツ型が推論されます。

<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> プロパティが `true` に設定されている場合、既定の動作は無効になります。

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

`Startup.ConfigureServices` に次のコードを追加します。

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

`Startup.ConfigureServices` の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後ろに次のコードを追加します。

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a>属性ルーティング要件

属性ルーティングは要件になります。 次に例を示します。

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> 内で定義されているか、または `Startup.Configure` 内の <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> によって定義されている[従来のルート](xref:mvc/controllers/routing#conventional-routing)を通してアクションにアクセスすることはできません。

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a>エラー状態コードに対する問題の詳細の応答

ASP.NET Core 2.2 以降では、MVC によってエラー結果 (状態コードが 400 以降の結果) が <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> による結果に変換されます。 `ProblemDetails` は次のようになります。

* [RFC 7807 仕様](https://tools.ietf.org/html/rfc7807)に基づいた型。
* HTTP 応答でコンピューターが判読できるエラーの詳細を指定するための標準化された形式。

コントローラー アクションで次のコードがあるとします。

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

`NotFound` の HTTP 応答には 404 状態コードと `ProblemDetails` の本文が含まれています。 次に例を示します。

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

問題の詳細機能には、2.2 以降の互換性フラグが必要です。 `SuppressMapClientErrors` プロパティが `true` に設定されている場合、既定の動作は無効になります。 `Startup.ConfigureServices` に次のコードを追加します。

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

`ProblemDetails` の応答の内容を構成するには、`ClientErrorMapping` プロパティを使用します。 たとえば、次のコードにより、404 応答の `type` プロパティが更新されます。

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a>その他の技術情報

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
