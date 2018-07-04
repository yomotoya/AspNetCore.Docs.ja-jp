---
title: ASP.NET Core で Web API を構築する
author: scottaddie
description: ASP.NET Core で Web API を構築するために使用できる機能、および各機能を使用する適切なタイミングについて説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/04/2018
ms.locfileid: "36274967"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="3e7a8-103">ASP.NET Core で Web API を構築する</span><span class="sxs-lookup"><span data-stu-id="3e7a8-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="3e7a8-104">作成者: [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="3e7a8-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="3e7a8-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3e7a8-106">このドキュメントでは、ASP.NET Core での各機能を使用する最も適切な web API をビルドする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="3e7a8-107">ControllerBase からクラスを派生する</span><span class="sxs-lookup"><span data-stu-id="3e7a8-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="3e7a8-108">Web API として機能することを目的としたコント ローラー内の [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="3e7a8-109">例:</span><span class="sxs-lookup"><span data-stu-id="3e7a8-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

<span data-ttu-id="3e7a8-110">`ControllerBase` クラスを使用すると、さまざまなプロパティおよびメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-110">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="3e7a8-111">前の例では、[BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) や [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) などがそのようなメソッドに該当します。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-111">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="3e7a8-112">これらのメソッドはアクション メソッド内で呼び出され、HTTP 400 および HTTP 201 のステータス コードをそれぞれ返します。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-112">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="3e7a8-113">[ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) プロパティ (これも `ControllerBase` によって提供される) は、要求モデル検証を実行する場合にアクセスされます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="3e7a8-114">ApiControllerAttribute でクラスに注釈を付ける</span><span class="sxs-lookup"><span data-stu-id="3e7a8-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="3e7a8-115">ASP.NET Core 2.1 では、Web API コントローラー クラスを表す [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 属性が導入されました。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="3e7a8-116">例:</span><span class="sxs-lookup"><span data-stu-id="3e7a8-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="3e7a8-117">この属性は、通常、有効なメソッドやプロパティにアクセスするために `ControllerBase` と組み合わせて使用されます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-117">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="3e7a8-118">`ControllerBase` では、[NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) や [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file) などのメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-118">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="3e7a8-119">その他に、`[ApiController]` 属性で注釈が付けられたユーザー定義の基本コントローラー クラスを作成するという方法があります。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-119">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="3e7a8-120">次のセクションでは、属性によって追加される便利な機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-120">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="3e7a8-121">自動的な HTTP 400 応答</span><span class="sxs-lookup"><span data-stu-id="3e7a8-121">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="3e7a8-122">検証エラーが発生すると、HTTP 400 応答が自動的にトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-122">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="3e7a8-123">次のコードは実際のアクションでは不要になります。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-123">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="3e7a8-124">この既定の動作は、*Startup.ConfigureServices* 内の次のコードで無効にされます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-124">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="3e7a8-125">バインディング ソース パラメーター推論</span><span class="sxs-lookup"><span data-stu-id="3e7a8-125">Binding source parameter inference</span></span>

<span data-ttu-id="3e7a8-126">バインディング ソース属性では、アクション パラメーターの値が存在する場所が定義されます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-126">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="3e7a8-127">次のバインディング ソース属性が存在します。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-127">The following binding source attributes exist:</span></span>

|<span data-ttu-id="3e7a8-128">属性</span><span class="sxs-lookup"><span data-stu-id="3e7a8-128">Attribute</span></span>|<span data-ttu-id="3e7a8-129">バインド ソース</span><span class="sxs-lookup"><span data-stu-id="3e7a8-129">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="3e7a8-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="3e7a8-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="3e7a8-131">要求本文</span><span class="sxs-lookup"><span data-stu-id="3e7a8-131">Request body</span></span> |
|<span data-ttu-id="3e7a8-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="3e7a8-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="3e7a8-133">要求本文内のフォーム データ</span><span class="sxs-lookup"><span data-stu-id="3e7a8-133">Form data in the request body</span></span> |
|<span data-ttu-id="3e7a8-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="3e7a8-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="3e7a8-135">要求ヘッダー</span><span class="sxs-lookup"><span data-stu-id="3e7a8-135">Request header</span></span> |
|<span data-ttu-id="3e7a8-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="3e7a8-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="3e7a8-137">要求のクエリ文字列パラメーター</span><span class="sxs-lookup"><span data-stu-id="3e7a8-137">Request query string parameter</span></span> |
|<span data-ttu-id="3e7a8-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="3e7a8-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="3e7a8-139">現在の要求からのルート データ</span><span class="sxs-lookup"><span data-stu-id="3e7a8-139">Route data from the current request</span></span> |
|<span data-ttu-id="3e7a8-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="3e7a8-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="3e7a8-141">アクション パラメーターとして挿入される要求サービス</span><span class="sxs-lookup"><span data-stu-id="3e7a8-141">The request service injected as an action parameter</span></span> |

> [!NOTE]
> <span data-ttu-id="3e7a8-142">`%2f` は `/` にエスケープ解除されないので、値に `%2f` (つまり `/`) が含まれる可能性がある場合は `[FromRoute]` を使用**しない**でください。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-142">Do **not** use `[FromRoute]` when values might contain `%2f` (that is `/`) because `%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="3e7a8-143">値に `%2f` が含まれる可能性がある場合は、`[FromQuery]` を使用してください。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-143">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="3e7a8-144">`[ApiController]` 属性がない場合は、バインディング ソース属性を明示的に定義します。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-144">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="3e7a8-145">次の例では、`discontinuedOnly` パラメーター値が要求 URL のクエリ文字列に指定されていることが `[FromQuery]` によって示されています。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-145">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

<span data-ttu-id="3e7a8-146">アクション パラメーターの既定のデータ ソースには推論規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-146">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="3e7a8-147">これらの規則によって、アクション パラメーターに、通常、手動で適用する可能性の高いバインディング ソースが構成されます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-147">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="3e7a8-148">バインディング ソースの属性は、次のように動作します。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-148">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="3e7a8-149">**[FromBody]** は複合型のパラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-149">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="3e7a8-150">この規則には例外があり、[IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) や [CancellationToken](/dotnet/api/system.threading.cancellationtoken) などの、特殊な意味を持つ複雑な組み込み型が該当します。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-150">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="3e7a8-151">バインディング ソース推論コードでは、そのような特殊な型は無視されます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-151">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="3e7a8-152">アクションの複数のパラメーターが明示的に指定されている場合 (`[FromBody]` によって) または要求本文からバインドとして推論される場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-152">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="3e7a8-153">たとえば、次のアクション シグネチャは例外の原因となります。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-153">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="3e7a8-154">**[FromForm]** は [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) および [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection) 型のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-154">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="3e7a8-155">簡易型またはユーザー定義型に対しては推論されません。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-155">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="3e7a8-156">**[FromRoute]** は、ルート テンプレート内のパラメーターと一致する任意のアクション パラメーター名に対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-156">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="3e7a8-157">複数のルートがアクション パラメーターと一致する場合、ルート値はいずれも `[FromRoute]` とみなされます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-157">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="3e7a8-158">**[FromQuery]** は他の任意のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-158">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="3e7a8-159">既定の推論規則は、*Startup.ConfigureServices* 内の次のコードで無効にされます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-159">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="3e7a8-160">マルチパート/フォーム データ要求の推論</span><span class="sxs-lookup"><span data-stu-id="3e7a8-160">Multipart/form-data request inference</span></span>

<span data-ttu-id="3e7a8-161">[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) 属性を使用してアクション パラメーターに注釈を付けた場合は、`multipart/form-data` 要求コンテンツ型が推論されます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-161">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="3e7a8-162">既定の動作は、*Startup.ConfigureServices* 内の次のコードで無効にされます。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-162">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="3e7a8-163">属性ルーティング要件</span><span class="sxs-lookup"><span data-stu-id="3e7a8-163">Attribute routing requirement</span></span>

<span data-ttu-id="3e7a8-164">属性ルーティングは要件になります。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-164">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="3e7a8-165">例:</span><span class="sxs-lookup"><span data-stu-id="3e7a8-165">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="3e7a8-166">[UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) 内で定義されているか、または *Startup.Configure* 内の [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) によって定義されている[従来のルート](xref:mvc/controllers/routing#conventional-routing)を通してアクションにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="3e7a8-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="3e7a8-167">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="3e7a8-167">Additional resources</span></span>

* [<span data-ttu-id="3e7a8-168">コントローラー アクションの戻り値の型</span><span class="sxs-lookup"><span data-stu-id="3e7a8-168">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="3e7a8-169">カスタム フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="3e7a8-169">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="3e7a8-170">応答データの書式設定</span><span class="sxs-lookup"><span data-stu-id="3e7a8-170">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="3e7a8-171">Swagger を使用するヘルプ ページ</span><span class="sxs-lookup"><span data-stu-id="3e7a8-171">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="3e7a8-172">コントローラー アクションへのルーティング</span><span class="sxs-lookup"><span data-stu-id="3e7a8-172">Routing to controller actions</span></span>](xref:mvc/controllers/routing)
