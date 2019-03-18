---
title: ASP.NET Core で Web API を構築する
author: scottaddie
description: ASP.NET Core で Web API を構築するために使用できる機能、および各機能を使用する適切なタイミングについて説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/11/2019
uid: web-api/index
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="88ed1-103">ASP.NET Core で Web API を構築する</span><span class="sxs-lookup"><span data-stu-id="88ed1-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="88ed1-104">作成者: [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="88ed1-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="88ed1-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="88ed1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="88ed1-106">このドキュメントでは、ASP.NET Core での各機能を使用する最も適切な web API をビルドする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="88ed1-107">ControllerBase からクラスを派生する</span><span class="sxs-lookup"><span data-stu-id="88ed1-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="88ed1-108">Web API として機能することを目的としたコント ローラー内の <xref:Microsoft.AspNetCore.Mvc.ControllerBase> クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="88ed1-109">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="88ed1-110">`ControllerBase` クラスを使用すると、いくつかのプロパティとメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="88ed1-111">上のコードでは、例に <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> と <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)> が含まれています。</span><span class="sxs-lookup"><span data-stu-id="88ed1-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="88ed1-112">これらのメソッドはアクション メソッド内で呼び出され、HTTP 400 および 201 のステータス コードをそれぞれ返します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="88ed1-113"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> プロパティ (これも `ControllerBase` によって提供される) は、要求モデル検証を処理する場合にアクセスされます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontroller-attribute"></a><span data-ttu-id="88ed1-114">ApiController 属性を使用した注釈</span><span class="sxs-lookup"><span data-stu-id="88ed1-114">Annotation with ApiController attribute</span></span>

<span data-ttu-id="88ed1-115">ASP.NET Core 2.1 では、Web API コントローラー クラスを表す [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 属性が導入されました。</span><span class="sxs-lookup"><span data-stu-id="88ed1-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="88ed1-116">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-116">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="88ed1-117">この属性をコントローラー レベルで使用するには、2.1 以降の互換性バージョン (<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> で設定) が必須です。</span><span class="sxs-lookup"><span data-stu-id="88ed1-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the controller level.</span></span> <span data-ttu-id="88ed1-118">たとえば、`Startup.ConfigureServices` の強調表示されているコードでは、2.1 の互換性フラグが設定されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-118">For example, the highlighted code in `Startup.ConfigureServices` sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="88ed1-119">詳細については、「<xref:mvc/compatibility-version>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88ed1-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="88ed1-120">ASP.NET Core 2.2 以降では、`[ApiController]` 属性をアセンブリに適用できます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-120">In ASP.NET Core 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="88ed1-121">この方法での注釈では、アセンブリ内のすべてのコントローラーに Web API の動作が適用されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-121">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="88ed1-122">個々のコントローラーを有効にする方法はありません。</span><span class="sxs-lookup"><span data-stu-id="88ed1-122">Beware that there's no way to opt out for individual controllers.</span></span> <span data-ttu-id="88ed1-123">推奨事項として、アセンブリ レベルの属性を `Startup` クラスに適用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88ed1-123">As a recommendation, assembly-level attributes should be applied to the `Startup` class:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

<span data-ttu-id="88ed1-124">この属性をアセンブリ レベルで使用するには、2.2 以降の互換性バージョン (<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> で設定) が必須です。</span><span class="sxs-lookup"><span data-stu-id="88ed1-124">A compatibility version of 2.2 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the assembly level.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="88ed1-125">コントローラーの REST 固有の動作を有効にする場合、通常、`[ApiController]` 属性は `ControllerBase` と組み合わせて使用されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-125">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="88ed1-126">`ControllerBase` では、<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> や <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> などのメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-126">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="88ed1-127">その他に、`[ApiController]` 属性で注釈が付けられたユーザー定義の基本コントローラー クラスを作成するという方法があります。</span><span class="sxs-lookup"><span data-stu-id="88ed1-127">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="88ed1-128">次のセクションでは、属性によって追加される便利な機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-128">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="88ed1-129">自動的な HTTP 400 応答</span><span class="sxs-lookup"><span data-stu-id="88ed1-129">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="88ed1-130">モデル検証エラーが発生すると、HTTP 400 応答が自動的にトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-130">Model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="88ed1-131">その結果、次のコードは実際のアクションでは不要になります。</span><span class="sxs-lookup"><span data-stu-id="88ed1-131">Consequently, the following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="88ed1-132">結果として発生する応答の出力をカスタマイズするには、<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> を使用します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-132">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the resulting response.</span></span>

<span data-ttu-id="88ed1-133">アクションがモデル検証エラーから回復できる場合は、既定の動作を無効にすると便利です。</span><span class="sxs-lookup"><span data-stu-id="88ed1-133">Disabling the default behavior is useful when your action can recover from a model validation error.</span></span> <span data-ttu-id="88ed1-134"><xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> プロパティが `true` に設定されている場合、既定の動作は無効になります。</span><span class="sxs-lookup"><span data-stu-id="88ed1-134">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="88ed1-135">`Startup.ConfigureServices` の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` の後ろに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-135">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="88ed1-136">2.2 以降の互換性フラグを設定した場合の、HTTP 400 応答に対する既定の応答の種類は、<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> です。</span><span class="sxs-lookup"><span data-stu-id="88ed1-136">With a compatibility flag of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="88ed1-137">`ValidationProblemDetails` 型は [RFC 7807 仕様](https://tools.ietf.org/html/rfc7807)に準拠しています。</span><span class="sxs-lookup"><span data-stu-id="88ed1-137">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="88ed1-138">代わりに ASP.NET Core 2.1 エラー形式の <xref:Microsoft.AspNetCore.Mvc.SerializableError> を返すようにするには、`SuppressUseValidationProblemDetailsForInvalidModelStateResponses` プロパティを `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-138">Set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` to instead return the ASP.NET Core 2.1 error format of <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="88ed1-139">`Startup.ConfigureServices` に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-139">Add the following code in `Startup.ConfigureServices`:</span></span>

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

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="88ed1-140">バインディング ソース パラメーター推論</span><span class="sxs-lookup"><span data-stu-id="88ed1-140">Binding source parameter inference</span></span>

<span data-ttu-id="88ed1-141">バインディング ソース属性では、アクション パラメーターの値が存在する場所が定義されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-141">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="88ed1-142">次のバインディング ソース属性が存在します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-142">The following binding source attributes exist:</span></span>

|<span data-ttu-id="88ed1-143">属性</span><span class="sxs-lookup"><span data-stu-id="88ed1-143">Attribute</span></span>|<span data-ttu-id="88ed1-144">バインド ソース</span><span class="sxs-lookup"><span data-stu-id="88ed1-144">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="88ed1-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="88ed1-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="88ed1-146">要求本文</span><span class="sxs-lookup"><span data-stu-id="88ed1-146">Request body</span></span> |
|<span data-ttu-id="88ed1-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="88ed1-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="88ed1-148">要求本文内のフォーム データ</span><span class="sxs-lookup"><span data-stu-id="88ed1-148">Form data in the request body</span></span> |
|<span data-ttu-id="88ed1-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="88ed1-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="88ed1-150">要求ヘッダー</span><span class="sxs-lookup"><span data-stu-id="88ed1-150">Request header</span></span> |
|<span data-ttu-id="88ed1-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="88ed1-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="88ed1-152">要求のクエリ文字列パラメーター</span><span class="sxs-lookup"><span data-stu-id="88ed1-152">Request query string parameter</span></span> |
|<span data-ttu-id="88ed1-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="88ed1-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="88ed1-154">現在の要求からのルート データ</span><span class="sxs-lookup"><span data-stu-id="88ed1-154">Route data from the current request</span></span> |
|<span data-ttu-id="88ed1-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="88ed1-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="88ed1-156">アクション パラメーターとして挿入される要求サービス</span><span class="sxs-lookup"><span data-stu-id="88ed1-156">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="88ed1-157">値に `%2f` (つまり、`/`) が含まれる可能性がある場合は、`[FromRoute]` を使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="88ed1-157">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="88ed1-158">`%2f` は `/` にエスケープ解除されません。</span><span class="sxs-lookup"><span data-stu-id="88ed1-158">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="88ed1-159">値に `%2f` が含まれる可能性がある場合は、`[FromQuery]` を使用してください。</span><span class="sxs-lookup"><span data-stu-id="88ed1-159">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="88ed1-160">`[ApiController]` 属性がない場合は、バインディング ソース属性を明示的に定義します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-160">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="88ed1-161">`[ApiController]` がない場合または `[FromQuery]` などのバインディング ソース属性がない場合は、ASP.NET Core ランタイムにより複合オブジェクト モデル バインダーの使用が試行されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-161">Without `[ApiController]` or other binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="88ed1-162">複合オブジェクト モデル バインダーは、(順序が定義された) 値プロバイダーからデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-162">The complex object model binder pulls data from value providers (which have a defined order).</span></span> <span data-ttu-id="88ed1-163">たとえば、"本文のモデル バインダー" は常にオプトインです。</span><span class="sxs-lookup"><span data-stu-id="88ed1-163">For instance, the 'body model binder' is always opt in.</span></span>

<span data-ttu-id="88ed1-164">次の例では、`discontinuedOnly` パラメーター値が要求 URL のクエリ文字列に指定されていることが `[FromQuery]` によって示されています。</span><span class="sxs-lookup"><span data-stu-id="88ed1-164">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="88ed1-165">アクション パラメーターの既定のデータ ソースには推論規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-165">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="88ed1-166">これらの規則によって、アクション パラメーターに、通常、手動で適用する可能性の高いバインディング ソースが構成されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-166">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="88ed1-167">バインディング ソースの属性は、次のように動作します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-167">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="88ed1-168">**[FromBody]** は複合型のパラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-168">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="88ed1-169">この規則には例外があり、<xref:Microsoft.AspNetCore.Http.IFormCollection> や <xref:System.Threading.CancellationToken> などの、特殊な意味を持つ複雑な組み込み型が該当します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-169">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="88ed1-170">バインディング ソース推論コードでは、そのような特殊な型は無視されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-170">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="88ed1-171">`[FromBody]` は、`string` や `int` などの単純型に対しては推論されません。</span><span class="sxs-lookup"><span data-stu-id="88ed1-171">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="88ed1-172">そのため、その機能が必要な場合、単純型に対しては `[FromBody]` 属性を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88ed1-172">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span> <span data-ttu-id="88ed1-173">アクションの複数のパラメーターが明示的に指定されている場合 (`[FromBody]` によって) または要求本文からバインドとして推論される場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-173">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="88ed1-174">たとえば、次のアクション シグネチャは例外の原因となります。</span><span class="sxs-lookup"><span data-stu-id="88ed1-174">For example, the following action signatures cause an exception:</span></span>

    [!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

    > [!NOTE]
    > <span data-ttu-id="88ed1-175">ASP.NET Core 2.1 では、リストや配列などのコレクション型パラメーターが誤って [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) と推論されています。</span><span class="sxs-lookup"><span data-stu-id="88ed1-175">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute).</span></span> <span data-ttu-id="88ed1-176">これらのパラメーターを要求本文からバインドする場合は、[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88ed1-176">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="88ed1-177">このビヘイビアーは、ASP.NET Core 2.2 以降で修正されており、既定ではコレクション型パラメーターが本文からバインドされることが推論されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-177">This behavior is fixed in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

* <span data-ttu-id="88ed1-178">**[FromForm]** は <xref:Microsoft.AspNetCore.Http.IFormFile> および <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 型のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-178">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="88ed1-179">簡易型またはユーザー定義型に対しては推論されません。</span><span class="sxs-lookup"><span data-stu-id="88ed1-179">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="88ed1-180">**[FromRoute]** は、ルート テンプレート内のパラメーターと一致する任意のアクション パラメーター名に対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-180">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="88ed1-181">複数のルートがアクション パラメーターと一致する場合、ルート値はいずれも `[FromRoute]` と見なされます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-181">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="88ed1-182">**[FromQuery]** は他の任意のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-182">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="88ed1-183"><xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> プロパティが `true` に設定されている場合、既定の推論規則は無効になります。</span><span class="sxs-lookup"><span data-stu-id="88ed1-183">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="88ed1-184">`Startup.ConfigureServices` の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` の後ろに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-184">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="88ed1-185">マルチパート/フォーム データ要求の推論</span><span class="sxs-lookup"><span data-stu-id="88ed1-185">Multipart/form-data request inference</span></span>

<span data-ttu-id="88ed1-186">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) 属性を使用してアクション パラメーターに注釈を付けた場合は、`multipart/form-data` 要求コンテンツ型が推論されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-186">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="88ed1-187"><xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> プロパティが `true` に設定されている場合、既定の動作は無効になります。</span><span class="sxs-lookup"><span data-stu-id="88ed1-187">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="88ed1-188">`Startup.ConfigureServices` に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-188">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="88ed1-189">`Startup.ConfigureServices` の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後ろに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-189">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a><span data-ttu-id="88ed1-190">属性ルーティング要件</span><span class="sxs-lookup"><span data-stu-id="88ed1-190">Attribute routing requirement</span></span>

<span data-ttu-id="88ed1-191">属性ルーティングは要件になります。</span><span class="sxs-lookup"><span data-stu-id="88ed1-191">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="88ed1-192">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-192">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="88ed1-193"><xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> 内で定義されているか、または `Startup.Configure` 内の <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> によって定義されている[従来のルート](xref:mvc/controllers/routing#conventional-routing)を通してアクションにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="88ed1-193">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="88ed1-194">エラー状態コードに対する問題の詳細の応答</span><span class="sxs-lookup"><span data-stu-id="88ed1-194">Problem details responses for error status codes</span></span>

<span data-ttu-id="88ed1-195">ASP.NET Core 2.2 以降では、MVC によってエラー結果 (状態コードが 400 以降の結果) が <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> による結果に変換されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-195">In ASP.NET Core 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="88ed1-196">`ProblemDetails` は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="88ed1-196">`ProblemDetails` is:</span></span>

* <span data-ttu-id="88ed1-197">[RFC 7807 仕様](https://tools.ietf.org/html/rfc7807)に基づいた型。</span><span class="sxs-lookup"><span data-stu-id="88ed1-197">A type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>
* <span data-ttu-id="88ed1-198">HTTP 応答でコンピューターが判読できるエラーの詳細を指定するための標準化された形式。</span><span class="sxs-lookup"><span data-stu-id="88ed1-198">A standardized format for specifying machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="88ed1-199">コントローラー アクションで次のコードがあるとします。</span><span class="sxs-lookup"><span data-stu-id="88ed1-199">Consider the following code in a controller action:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="88ed1-200">`NotFound` の HTTP 応答には 404 状態コードと `ProblemDetails` の本文が含まれています。</span><span class="sxs-lookup"><span data-stu-id="88ed1-200">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="88ed1-201">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-201">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="88ed1-202">問題の詳細機能には、2.2 以降の互換性フラグが必要です。</span><span class="sxs-lookup"><span data-stu-id="88ed1-202">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="88ed1-203">`SuppressMapClientErrors` プロパティが `true` に設定されている場合、既定の動作は無効になります。</span><span class="sxs-lookup"><span data-stu-id="88ed1-203">The default behavior is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="88ed1-204">`Startup.ConfigureServices` に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-204">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

<span data-ttu-id="88ed1-205">`ProblemDetails` の応答の内容を構成するには、`ClientErrorMapping` プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="88ed1-205">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="88ed1-206">たとえば、次のコードにより、404 応答の `type` プロパティが更新されます。</span><span class="sxs-lookup"><span data-stu-id="88ed1-206">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="88ed1-207">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="88ed1-207">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
