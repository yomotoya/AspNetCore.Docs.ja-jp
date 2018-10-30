---
title: ASP.NET Core で Web API を構築する
author: scottaddie
description: ASP.NET Core で Web API を構築するために使用できる機能、および各機能を使用する適切なタイミングについて説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: 950f4e8afa13bf297ea8658ef1c1bea0c9b62936
ms.sourcegitcommit: 2ef32676c16f76282f7c23154d13affce8c8bf35
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234593"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="428be-103">ASP.NET Core で Web API を構築する</span><span class="sxs-lookup"><span data-stu-id="428be-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="428be-104">作成者: [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="428be-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="428be-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="428be-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="428be-106">このドキュメントでは、ASP.NET Core での各機能を使用する最も適切な web API をビルドする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="428be-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="428be-107">ControllerBase からクラスを派生する</span><span class="sxs-lookup"><span data-stu-id="428be-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="428be-108">Web API として機能することを目的としたコント ローラー内の <xref:Microsoft.AspNetCore.Mvc.ControllerBase> クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="428be-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="428be-109">例:</span><span class="sxs-lookup"><span data-stu-id="428be-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="428be-110">`ControllerBase` クラスを使用すると、いくつかのプロパティとメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="428be-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="428be-111">上のコードでは、例に <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> と <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)> が含まれています。</span><span class="sxs-lookup"><span data-stu-id="428be-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="428be-112">これらのメソッドはアクション メソッド内で呼び出され、HTTP 400 および 201 のステータス コードをそれぞれ返します。</span><span class="sxs-lookup"><span data-stu-id="428be-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="428be-113"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> プロパティ (これも `ControllerBase` によって提供される) は、要求モデル検証を処理する場合にアクセスされます。</span><span class="sxs-lookup"><span data-stu-id="428be-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="428be-114">ApiControllerAttribute でクラスに注釈を付ける</span><span class="sxs-lookup"><span data-stu-id="428be-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="428be-115">ASP.NET Core 2.1 では、Web API コントローラー クラスを表す [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 属性が導入されました。</span><span class="sxs-lookup"><span data-stu-id="428be-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="428be-116">例:</span><span class="sxs-lookup"><span data-stu-id="428be-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="428be-117">この属性を使用するには、2.1 以降の互換性バージョン (<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> で設定) が必須です。</span><span class="sxs-lookup"><span data-stu-id="428be-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="428be-118">たとえば、*Startup.ConfigureServices* の強調表示されているコードでは、2.2 の互換性フラグが設定されます。</span><span class="sxs-lookup"><span data-stu-id="428be-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.2 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="428be-119">詳細については、「<xref:mvc/compatibility-version>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="428be-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="428be-120">コントローラーの REST 固有の動作を有効にする場合、通常、`[ApiController]` 属性は `ControllerBase` と組み合わせて使用されます。</span><span class="sxs-lookup"><span data-stu-id="428be-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="428be-121">`ControllerBase` では、<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> や <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> などのメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="428be-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="428be-122">その他に、`[ApiController]` 属性で注釈が付けられたユーザー定義の基本コントローラー クラスを作成するという方法があります。</span><span class="sxs-lookup"><span data-stu-id="428be-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="428be-123">次のセクションでは、属性によって追加される便利な機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="428be-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="428be-124">エラー状態コードに対する問題の詳細の応答</span><span class="sxs-lookup"><span data-stu-id="428be-124">Problem details responses for error status codes</span></span>

<span data-ttu-id="428be-125">ASP.NET Core 2.1 以降には、[RFC 7807 仕様](https://tools.ietf.org/html/rfc7807)に基づいた型 [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails) が含まれています。</span><span class="sxs-lookup"><span data-stu-id="428be-125">ASP.NET Core 2.1 and later includes [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), a type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="428be-126">`ProblemDetails` 型は機械で読み取り可能なエラーの詳細を HTTP 応答で伝えるための標準化された形式を提供します。</span><span class="sxs-lookup"><span data-stu-id="428be-126">The `ProblemDetails` type provides a standardized format for conveying machine readable details of errors in a HTTP response.</span></span>

<span data-ttu-id="428be-127">ASP.NET Core 2.2 以降では、MVC がエラー状態コードの結果 (状態コード 400 以上) を `ProblemDetails` による結果に変換します。</span><span class="sxs-lookup"><span data-stu-id="428be-127">In ASP.NET Core 2.2 and later, MVC transforms error status code results (status code 400 and higher) to a result with `ProblemDetails`.</span></span> <span data-ttu-id="428be-128">次のコードがあるとします。</span><span class="sxs-lookup"><span data-stu-id="428be-128">Consider the following code:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

<span data-ttu-id="428be-129">`NotFound` の結果の HTTP 応答には、404 状態コードと次のような `ProblemDetails` の本文が含まれています。</span><span class="sxs-lookup"><span data-stu-id="428be-129">The HTTP response for the `NotFound` result has a 404 status code with a `ProblemDetails` body similar to the following:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="428be-130">問題の詳細機能には、2.2 以降の互換性フラグが必要です。</span><span class="sxs-lookup"><span data-stu-id="428be-130">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="428be-131">[SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> プロパティが `true` に設定されている場合、既定の動作は無効になります。</span><span class="sxs-lookup"><span data-stu-id="428be-131">The default behavior is disabled when the [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> property is set to `true`.</span></span> <span data-ttu-id="428be-132">`Startup.ConfigureServices` の次の強調表示されたコードを使用すると、問題の詳細が無効になります。</span><span class="sxs-lookup"><span data-stu-id="428be-132">The following highlighted code from `Startup.ConfigureServices` disables problem details:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

<span data-ttu-id="428be-133">`ProblemDetails` 応答の内容を構成するには、[ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="428be-133">Use the [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="428be-134">たとえば、次のコードにより、404 応答の `type` プロパティが更新されます。</span><span class="sxs-lookup"><span data-stu-id="428be-134">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a><span data-ttu-id="428be-135">自動的な HTTP 400 応答</span><span class="sxs-lookup"><span data-stu-id="428be-135">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="428be-136">検証エラーが発生すると、HTTP 400 応答が自動的にトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="428be-136">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="428be-137">次のコードは実際のアクションでは不要になります。</span><span class="sxs-lookup"><span data-stu-id="428be-137">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="428be-138">結果として発生する応答の出力をカスタマイズするには、<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> を使用します。</span><span class="sxs-lookup"><span data-stu-id="428be-138">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the the resulting response.</span></span>

<span data-ttu-id="428be-139"><xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> プロパティが `true` に設定されている場合、既定の動作は無効になります。</span><span class="sxs-lookup"><span data-stu-id="428be-139">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="428be-140">*Startup.ConfigureServices* の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="428be-140">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

<span data-ttu-id="428be-141">2.2 以降の互換性フラグを設定した場合の、400 応答に対して返される既定の応答の種類は、<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> です。</span><span class="sxs-lookup"><span data-stu-id="428be-141">With a compatibility flag of 2.2 or later, the default response type returned for 400 responses is a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="428be-142">ASP.NET Core 2.1 エラー形式を使用するには、[SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="428be-142">Use the [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> property to use the ASP.NET Core 2.1 error format.</span></span>

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="428be-143">バインディング ソース パラメーター推論</span><span class="sxs-lookup"><span data-stu-id="428be-143">Binding source parameter inference</span></span>

<span data-ttu-id="428be-144">バインディング ソース属性では、アクション パラメーターの値が存在する場所が定義されます。</span><span class="sxs-lookup"><span data-stu-id="428be-144">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="428be-145">次のバインディング ソース属性が存在します。</span><span class="sxs-lookup"><span data-stu-id="428be-145">The following binding source attributes exist:</span></span>

|<span data-ttu-id="428be-146">属性</span><span class="sxs-lookup"><span data-stu-id="428be-146">Attribute</span></span>|<span data-ttu-id="428be-147">バインド ソース</span><span class="sxs-lookup"><span data-stu-id="428be-147">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="428be-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="428be-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="428be-149">要求本文</span><span class="sxs-lookup"><span data-stu-id="428be-149">Request body</span></span> |
|<span data-ttu-id="428be-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="428be-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="428be-151">要求本文内のフォーム データ</span><span class="sxs-lookup"><span data-stu-id="428be-151">Form data in the request body</span></span> |
|<span data-ttu-id="428be-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="428be-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="428be-153">要求ヘッダー</span><span class="sxs-lookup"><span data-stu-id="428be-153">Request header</span></span> |
|<span data-ttu-id="428be-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="428be-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="428be-155">要求のクエリ文字列パラメーター</span><span class="sxs-lookup"><span data-stu-id="428be-155">Request query string parameter</span></span> |
|<span data-ttu-id="428be-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="428be-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="428be-157">現在の要求からのルート データ</span><span class="sxs-lookup"><span data-stu-id="428be-157">Route data from the current request</span></span> |
|<span data-ttu-id="428be-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="428be-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="428be-159">アクション パラメーターとして挿入される要求サービス</span><span class="sxs-lookup"><span data-stu-id="428be-159">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="428be-160">値に `%2f` (つまり、`/`) が含まれる可能性がある場合は、`[FromRoute]` を使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="428be-160">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="428be-161">`%2f` は `/` にエスケープ解除されません。</span><span class="sxs-lookup"><span data-stu-id="428be-161">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="428be-162">値に `%2f` が含まれる可能性がある場合は、`[FromQuery]` を使用してください。</span><span class="sxs-lookup"><span data-stu-id="428be-162">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="428be-163">`[ApiController]` 属性がない場合は、バインディング ソース属性を明示的に定義します。</span><span class="sxs-lookup"><span data-stu-id="428be-163">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="428be-164">次の例では、`discontinuedOnly` パラメーター値が要求 URL のクエリ文字列に指定されていることが `[FromQuery]` によって示されています。</span><span class="sxs-lookup"><span data-stu-id="428be-164">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="428be-165">アクション パラメーターの既定のデータ ソースには推論規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="428be-165">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="428be-166">これらの規則によって、アクション パラメーターに、通常、手動で適用する可能性の高いバインディング ソースが構成されます。</span><span class="sxs-lookup"><span data-stu-id="428be-166">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="428be-167">バインディング ソースの属性は、次のように動作します。</span><span class="sxs-lookup"><span data-stu-id="428be-167">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="428be-168">**[FromBody]** は複合型のパラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="428be-168">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="428be-169">この規則には例外があり、<xref:Microsoft.AspNetCore.Http.IFormCollection> や <xref:System.Threading.CancellationToken> などの、特殊な意味を持つ複雑な組み込み型が該当します。</span><span class="sxs-lookup"><span data-stu-id="428be-169">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="428be-170">バインディング ソース推論コードでは、そのような特殊な型は無視されます。</span><span class="sxs-lookup"><span data-stu-id="428be-170">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="428be-171">`[FromBody]` は、`string` や `int` などの単純型に対しては推論されません。</span><span class="sxs-lookup"><span data-stu-id="428be-171">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="428be-172">そのため、その機能が必要な場合、単純型に対しては `[FromBody]` 属性を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="428be-172">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="428be-173">アクションの複数のパラメーターが明示的に指定されている場合 (`[FromBody]` によって) または要求本文からバインドとして推論される場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="428be-173">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="428be-174">たとえば、次のアクション シグネチャは例外の原因となります。</span><span class="sxs-lookup"><span data-stu-id="428be-174">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="428be-175">**[FromForm]** は <xref:Microsoft.AspNetCore.Http.IFormFile> および <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 型のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="428be-175">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="428be-176">簡易型またはユーザー定義型に対しては推論されません。</span><span class="sxs-lookup"><span data-stu-id="428be-176">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="428be-177">**[FromRoute]** は、ルート テンプレート内のパラメーターと一致する任意のアクション パラメーター名に対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="428be-177">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="428be-178">複数のルートがアクション パラメーターと一致する場合、ルート値はいずれも `[FromRoute]` と見なされます。</span><span class="sxs-lookup"><span data-stu-id="428be-178">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="428be-179">**[FromQuery]** は他の任意のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="428be-179">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="428be-180"><xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> プロパティが `true` に設定されている場合、既定の推論規則は無効になります。</span><span class="sxs-lookup"><span data-stu-id="428be-180">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="428be-181">*Startup.ConfigureServices* の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="428be-181">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="428be-182">マルチパート/フォーム データ要求の推論</span><span class="sxs-lookup"><span data-stu-id="428be-182">Multipart/form-data request inference</span></span>

<span data-ttu-id="428be-183">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) 属性を使用してアクション パラメーターに注釈を付けた場合は、`multipart/form-data` 要求コンテンツ型が推論されます。</span><span class="sxs-lookup"><span data-stu-id="428be-183">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="428be-184"><xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> プロパティが `true` に設定されている場合、既定の動作は無効になります。</span><span class="sxs-lookup"><span data-stu-id="428be-184">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="428be-185">*Startup.ConfigureServices* の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="428be-185">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="428be-186">属性ルーティング要件</span><span class="sxs-lookup"><span data-stu-id="428be-186">Attribute routing requirement</span></span>

<span data-ttu-id="428be-187">属性ルーティングは要件になります。</span><span class="sxs-lookup"><span data-stu-id="428be-187">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="428be-188">例:</span><span class="sxs-lookup"><span data-stu-id="428be-188">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="428be-189"><xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> 内で定義されているか、または *Startup.Configure* 内の <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> によって定義されている[従来のルート](xref:mvc/controllers/routing#conventional-routing)を通してアクションにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="428be-189">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="428be-190">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="428be-190">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
