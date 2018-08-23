---
title: ASP.NET Core で Web API を構築する
author: scottaddie
description: ASP.NET Core で Web API を構築するために使用できる機能、および各機能を使用する適切なタイミングについて説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: d410f28ff7fda3bf33f73c06b3e626dfd4ee7dd8
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/22/2018
ms.locfileid: "41822141"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="33a51-103">ASP.NET Core で Web API を構築する</span><span class="sxs-lookup"><span data-stu-id="33a51-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="33a51-104">作成者: [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="33a51-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="33a51-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="33a51-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="33a51-106">このドキュメントでは、ASP.NET Core での各機能を使用する最も適切な web API をビルドする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="33a51-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="33a51-107">ControllerBase からクラスを派生する</span><span class="sxs-lookup"><span data-stu-id="33a51-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="33a51-108">Web API として機能することを目的としたコント ローラー内の <xref:Microsoft.AspNetCore.Mvc.ControllerBase> クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="33a51-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="33a51-109">例:</span><span class="sxs-lookup"><span data-stu-id="33a51-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="33a51-110">`ControllerBase` クラスを使用すると、いくつかのプロパティとメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="33a51-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="33a51-111">上のコードでは、例に <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> と <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)> が含まれています。</span><span class="sxs-lookup"><span data-stu-id="33a51-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="33a51-112">これらのメソッドはアクション メソッド内で呼び出され、HTTP 400 および 201 のステータス コードをそれぞれ返します。</span><span class="sxs-lookup"><span data-stu-id="33a51-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="33a51-113"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> プロパティ (これも `ControllerBase` によって提供される) は、要求モデル検証を処理する場合にアクセスされます。</span><span class="sxs-lookup"><span data-stu-id="33a51-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="33a51-114">ApiControllerAttribute でクラスに注釈を付ける</span><span class="sxs-lookup"><span data-stu-id="33a51-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="33a51-115">ASP.NET Core 2.1 では、Web API コントローラー クラスを表す [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 属性が導入されました。</span><span class="sxs-lookup"><span data-stu-id="33a51-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="33a51-116">例:</span><span class="sxs-lookup"><span data-stu-id="33a51-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="33a51-117">この属性を使用するには、2.1 以降の互換性バージョン (<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> で設定) が必須です。</span><span class="sxs-lookup"><span data-stu-id="33a51-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="33a51-118">たとえば、*Startup.ConfigureServices* の強調表示されているコードでは、2.1 の互換性フラグが設定されます。</span><span class="sxs-lookup"><span data-stu-id="33a51-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="33a51-119">詳細については、「<xref:mvc/compatibility-version>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="33a51-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="33a51-120">コントローラーの REST 固有の動作を有効にする場合、通常、`[ApiController]` 属性は `ControllerBase` と組み合わせて使用されます。</span><span class="sxs-lookup"><span data-stu-id="33a51-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="33a51-121">`ControllerBase` では、<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> や <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> などのメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="33a51-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="33a51-122">その他に、`[ApiController]` 属性で注釈が付けられたユーザー定義の基本コントローラー クラスを作成するという方法があります。</span><span class="sxs-lookup"><span data-stu-id="33a51-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="33a51-123">次のセクションでは、属性によって追加される便利な機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="33a51-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="33a51-124">自動的な HTTP 400 応答</span><span class="sxs-lookup"><span data-stu-id="33a51-124">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="33a51-125">検証エラーが発生すると、HTTP 400 応答が自動的にトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="33a51-125">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="33a51-126">次のコードは実際のアクションでは不要になります。</span><span class="sxs-lookup"><span data-stu-id="33a51-126">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="33a51-127"><xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> プロパティが `true` に設定されている場合、既定の動作は無効になります。</span><span class="sxs-lookup"><span data-stu-id="33a51-127">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="33a51-128">*Startup.ConfigureServices* の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="33a51-128">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="33a51-129">バインディング ソース パラメーター推論</span><span class="sxs-lookup"><span data-stu-id="33a51-129">Binding source parameter inference</span></span>

<span data-ttu-id="33a51-130">バインディング ソース属性では、アクション パラメーターの値が存在する場所が定義されます。</span><span class="sxs-lookup"><span data-stu-id="33a51-130">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="33a51-131">次のバインディング ソース属性が存在します。</span><span class="sxs-lookup"><span data-stu-id="33a51-131">The following binding source attributes exist:</span></span>

|<span data-ttu-id="33a51-132">属性</span><span class="sxs-lookup"><span data-stu-id="33a51-132">Attribute</span></span>|<span data-ttu-id="33a51-133">バインド ソース</span><span class="sxs-lookup"><span data-stu-id="33a51-133">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="33a51-134">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="33a51-134">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="33a51-135">要求本文</span><span class="sxs-lookup"><span data-stu-id="33a51-135">Request body</span></span> |
|<span data-ttu-id="33a51-136">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="33a51-136">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="33a51-137">要求本文内のフォーム データ</span><span class="sxs-lookup"><span data-stu-id="33a51-137">Form data in the request body</span></span> |
|<span data-ttu-id="33a51-138">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="33a51-138">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="33a51-139">要求ヘッダー</span><span class="sxs-lookup"><span data-stu-id="33a51-139">Request header</span></span> |
|<span data-ttu-id="33a51-140">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="33a51-140">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="33a51-141">要求のクエリ文字列パラメーター</span><span class="sxs-lookup"><span data-stu-id="33a51-141">Request query string parameter</span></span> |
|<span data-ttu-id="33a51-142">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="33a51-142">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="33a51-143">現在の要求からのルート データ</span><span class="sxs-lookup"><span data-stu-id="33a51-143">Route data from the current request</span></span> |
|<span data-ttu-id="33a51-144">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="33a51-144">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="33a51-145">アクション パラメーターとして挿入される要求サービス</span><span class="sxs-lookup"><span data-stu-id="33a51-145">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="33a51-146">値に `%2f` (つまり、`/`) が含まれる可能性がある場合は、`[FromRoute]` を使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="33a51-146">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="33a51-147">`%2f` は `/` にエスケープ解除されません。</span><span class="sxs-lookup"><span data-stu-id="33a51-147">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="33a51-148">値に `%2f` が含まれる可能性がある場合は、`[FromQuery]` を使用してください。</span><span class="sxs-lookup"><span data-stu-id="33a51-148">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="33a51-149">`[ApiController]` 属性がない場合は、バインディング ソース属性を明示的に定義します。</span><span class="sxs-lookup"><span data-stu-id="33a51-149">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="33a51-150">次の例では、`discontinuedOnly` パラメーター値が要求 URL のクエリ文字列に指定されていることが `[FromQuery]` によって示されています。</span><span class="sxs-lookup"><span data-stu-id="33a51-150">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="33a51-151">アクション パラメーターの既定のデータ ソースには推論規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="33a51-151">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="33a51-152">これらの規則によって、アクション パラメーターに、通常、手動で適用する可能性の高いバインディング ソースが構成されます。</span><span class="sxs-lookup"><span data-stu-id="33a51-152">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="33a51-153">バインディング ソースの属性は、次のように動作します。</span><span class="sxs-lookup"><span data-stu-id="33a51-153">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="33a51-154">**[FromBody]** は複合型のパラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="33a51-154">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="33a51-155">この規則には例外があり、<xref:Microsoft.AspNetCore.Http.IFormCollection> や <xref:System.Threading.CancellationToken> などの、特殊な意味を持つ複雑な組み込み型が該当します。</span><span class="sxs-lookup"><span data-stu-id="33a51-155">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="33a51-156">バインディング ソース推論コードでは、そのような特殊な型は無視されます。</span><span class="sxs-lookup"><span data-stu-id="33a51-156">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="33a51-157">`[FromBody]` は、`string` や `int` などの単純型に対しては推論されません。</span><span class="sxs-lookup"><span data-stu-id="33a51-157">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="33a51-158">そのため、その機能が必要な場合、単純型に対しては `[FromBody]` 属性を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="33a51-158">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="33a51-159">アクションの複数のパラメーターが明示的に指定されている場合 (`[FromBody]` によって) または要求本文からバインドとして推論される場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="33a51-159">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="33a51-160">たとえば、次のアクション シグネチャは例外の原因となります。</span><span class="sxs-lookup"><span data-stu-id="33a51-160">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="33a51-161">**[FromForm]** は <xref:Microsoft.AspNetCore.Http.IFormFile> および <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 型のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="33a51-161">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="33a51-162">簡易型またはユーザー定義型に対しては推論されません。</span><span class="sxs-lookup"><span data-stu-id="33a51-162">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="33a51-163">**[FromRoute]** は、ルート テンプレート内のパラメーターと一致する任意のアクション パラメーター名に対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="33a51-163">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="33a51-164">複数のルートがアクション パラメーターと一致する場合、ルート値はいずれも `[FromRoute]` と見なされます。</span><span class="sxs-lookup"><span data-stu-id="33a51-164">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="33a51-165">**[FromQuery]** は他の任意のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="33a51-165">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="33a51-166"><xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> プロパティが `true` に設定されている場合、既定の推論規則は無効になります。</span><span class="sxs-lookup"><span data-stu-id="33a51-166">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="33a51-167">*Startup.ConfigureServices* の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="33a51-167">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="33a51-168">マルチパート/フォーム データ要求の推論</span><span class="sxs-lookup"><span data-stu-id="33a51-168">Multipart/form-data request inference</span></span>

<span data-ttu-id="33a51-169">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) 属性を使用してアクション パラメーターに注釈を付けた場合は、`multipart/form-data` 要求コンテンツ型が推論されます。</span><span class="sxs-lookup"><span data-stu-id="33a51-169">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="33a51-170"><xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> プロパティが `true` に設定されている場合、既定の動作は無効になります。</span><span class="sxs-lookup"><span data-stu-id="33a51-170">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="33a51-171">*Startup.ConfigureServices* の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="33a51-171">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="33a51-172">属性ルーティング要件</span><span class="sxs-lookup"><span data-stu-id="33a51-172">Attribute routing requirement</span></span>

<span data-ttu-id="33a51-173">属性ルーティングは要件になります。</span><span class="sxs-lookup"><span data-stu-id="33a51-173">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="33a51-174">例:</span><span class="sxs-lookup"><span data-stu-id="33a51-174">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="33a51-175"><xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> 内で定義されているか、または *Startup.Configure* 内の <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> によって定義されている[従来のルート](xref:mvc/controllers/routing#conventional-routing)を通してアクションにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="33a51-175">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="33a51-176">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="33a51-176">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
