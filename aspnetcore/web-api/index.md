---
title: ASP.NET Core で Web API を構築する
author: scottaddie
description: ASP.NET Core で Web API を構築するために使用できる機能、および各機能を使用する適切なタイミングについて説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274967"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="f045e-103">ASP.NET Core で Web API を構築する</span><span class="sxs-lookup"><span data-stu-id="f045e-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="f045e-104">作成者: [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="f045e-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="f045e-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="f045e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f045e-106">このドキュメントでは、ASP.NET Core での各機能を使用する最も適切な web API をビルドする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f045e-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="f045e-107">ControllerBase からクラスを派生する</span><span class="sxs-lookup"><span data-stu-id="f045e-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="f045e-108">Web API として機能することを目的としたコント ローラー内の [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="f045e-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="f045e-109">例:</span><span class="sxs-lookup"><span data-stu-id="f045e-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="f045e-110">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="f045e-110">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="f045e-111">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="f045e-111">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]</span></span>
::: moniker-end

<span data-ttu-id="f045e-112">`ControllerBase` クラスを使用すると、さまざまなプロパティおよびメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="f045e-112">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="f045e-113">前の例では、[BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) や [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) などがそのようなメソッドに該当します。</span><span class="sxs-lookup"><span data-stu-id="f045e-113">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="f045e-114">これらのメソッドはアクション メソッド内で呼び出され、HTTP 400 および HTTP 201 のステータス コードをそれぞれ返します。</span><span class="sxs-lookup"><span data-stu-id="f045e-114">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="f045e-115">[ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) プロパティ (これも `ControllerBase` によって提供される) は、要求モデル検証を実行する場合にアクセスされます。</span><span class="sxs-lookup"><span data-stu-id="f045e-115">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="f045e-116">ApiControllerAttribute でクラスに注釈を付ける</span><span class="sxs-lookup"><span data-stu-id="f045e-116">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="f045e-117">ASP.NET Core 2.1 では、Web API コントローラー クラスを表す [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 属性が導入されました。</span><span class="sxs-lookup"><span data-stu-id="f045e-117">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="f045e-118">例:</span><span class="sxs-lookup"><span data-stu-id="f045e-118">For example:</span></span>

<span data-ttu-id="f045e-119">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="f045e-119">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]</span></span>

<span data-ttu-id="f045e-120">この属性は、通常、有効なメソッドやプロパティにアクセスするために `ControllerBase` と組み合わせて使用されます。</span><span class="sxs-lookup"><span data-stu-id="f045e-120">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="f045e-121">`ControllerBase` では、[NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) や [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file) などのメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="f045e-121">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="f045e-122">その他に、`[ApiController]` 属性で注釈が付けられたユーザー定義の基本コントローラー クラスを作成するという方法があります。</span><span class="sxs-lookup"><span data-stu-id="f045e-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

<span data-ttu-id="f045e-123">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]</span><span class="sxs-lookup"><span data-stu-id="f045e-123">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]</span></span>

<span data-ttu-id="f045e-124">次のセクションでは、属性によって追加される便利な機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="f045e-124">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="f045e-125">自動的な HTTP 400 応答</span><span class="sxs-lookup"><span data-stu-id="f045e-125">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="f045e-126">検証エラーが発生すると、HTTP 400 応答が自動的にトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="f045e-126">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="f045e-127">次のコードは実際のアクションでは不要になります。</span><span class="sxs-lookup"><span data-stu-id="f045e-127">The following code becomes unnecessary in your actions:</span></span>

<span data-ttu-id="f045e-128">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]</span><span class="sxs-lookup"><span data-stu-id="f045e-128">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]</span></span>

<span data-ttu-id="f045e-129">この既定の動作は、*Startup.ConfigureServices* 内の次のコードで無効にされます。</span><span class="sxs-lookup"><span data-stu-id="f045e-129">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

<span data-ttu-id="f045e-130">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="f045e-130">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]</span></span>

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="f045e-131">バインディング ソース パラメーター推論</span><span class="sxs-lookup"><span data-stu-id="f045e-131">Binding source parameter inference</span></span>

<span data-ttu-id="f045e-132">バインディング ソース属性では、アクション パラメーターの値が存在する場所が定義されます。</span><span class="sxs-lookup"><span data-stu-id="f045e-132">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="f045e-133">次のバインディング ソース属性が存在します。</span><span class="sxs-lookup"><span data-stu-id="f045e-133">The following binding source attributes exist:</span></span>

|<span data-ttu-id="f045e-134">属性</span><span class="sxs-lookup"><span data-stu-id="f045e-134">Attribute</span></span>|<span data-ttu-id="f045e-135">バインド ソース</span><span class="sxs-lookup"><span data-stu-id="f045e-135">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="f045e-136">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="f045e-136">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="f045e-137">要求本文</span><span class="sxs-lookup"><span data-stu-id="f045e-137">Request body</span></span> |
|<span data-ttu-id="f045e-138">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="f045e-138">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="f045e-139">要求本文内のフォーム データ</span><span class="sxs-lookup"><span data-stu-id="f045e-139">Form data in the request body</span></span> |
|<span data-ttu-id="f045e-140">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="f045e-140">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="f045e-141">要求ヘッダー</span><span class="sxs-lookup"><span data-stu-id="f045e-141">Request header</span></span> |
|<span data-ttu-id="f045e-142">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="f045e-142">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="f045e-143">要求のクエリ文字列パラメーター</span><span class="sxs-lookup"><span data-stu-id="f045e-143">Request query string parameter</span></span> |
|<span data-ttu-id="f045e-144">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="f045e-144">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="f045e-145">現在の要求からのルート データ</span><span class="sxs-lookup"><span data-stu-id="f045e-145">Route data from the current request</span></span> |
|<span data-ttu-id="f045e-146">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="f045e-146">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="f045e-147">アクション パラメーターとして挿入される要求サービス</span><span class="sxs-lookup"><span data-stu-id="f045e-147">The request service injected as an action parameter</span></span> |

> [!NOTE]
> <span data-ttu-id="f045e-148">`%2f` は `/` にエスケープ解除されないので、値に `%2f` (つまり `/`) が含まれる可能性がある場合は `[FromRoute]` を使用**しない**でください。</span><span class="sxs-lookup"><span data-stu-id="f045e-148">Do **not** use `[FromRoute]` when values might contain `%2f` (that is `/`) because `%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="f045e-149">値に `%2f` が含まれる可能性がある場合は、`[FromQuery]` を使用してください。</span><span class="sxs-lookup"><span data-stu-id="f045e-149">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="f045e-150">`[ApiController]` 属性がない場合は、バインディング ソース属性を明示的に定義します。</span><span class="sxs-lookup"><span data-stu-id="f045e-150">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="f045e-151">次の例では、`discontinuedOnly` パラメーター値が要求 URL のクエリ文字列に指定されていることが `[FromQuery]` によって示されています。</span><span class="sxs-lookup"><span data-stu-id="f045e-151">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

<span data-ttu-id="f045e-152">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="f045e-152">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]</span></span>

<span data-ttu-id="f045e-153">アクション パラメーターの既定のデータ ソースには推論規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="f045e-153">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="f045e-154">これらの規則によって、アクション パラメーターに、通常、手動で適用する可能性の高いバインディング ソースが構成されます。</span><span class="sxs-lookup"><span data-stu-id="f045e-154">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="f045e-155">バインディング ソースの属性は、次のように動作します。</span><span class="sxs-lookup"><span data-stu-id="f045e-155">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="f045e-156">**[FromBody]** は複合型のパラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="f045e-156">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="f045e-157">この規則には例外があり、[IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) や [CancellationToken](/dotnet/api/system.threading.cancellationtoken) などの、特殊な意味を持つ複雑な組み込み型が該当します。</span><span class="sxs-lookup"><span data-stu-id="f045e-157">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="f045e-158">バインディング ソース推論コードでは、そのような特殊な型は無視されます。</span><span class="sxs-lookup"><span data-stu-id="f045e-158">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="f045e-159">アクションの複数のパラメーターが明示的に指定されている場合 (`[FromBody]` によって) または要求本文からバインドとして推論される場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="f045e-159">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="f045e-160">たとえば、次のアクション シグネチャは例外の原因となります。</span><span class="sxs-lookup"><span data-stu-id="f045e-160">For example, the following action signatures cause an exception:</span></span>

<span data-ttu-id="f045e-161">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]</span><span class="sxs-lookup"><span data-stu-id="f045e-161">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]</span></span>

* <span data-ttu-id="f045e-162">**[FromForm]** は [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) および [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection) 型のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="f045e-162">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="f045e-163">簡易型またはユーザー定義型に対しては推論されません。</span><span class="sxs-lookup"><span data-stu-id="f045e-163">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="f045e-164">**[FromRoute]** は、ルート テンプレート内のパラメーターと一致する任意のアクション パラメーター名に対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="f045e-164">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="f045e-165">複数のルートがアクション パラメーターと一致する場合、ルート値はいずれも `[FromRoute]` とみなされます。</span><span class="sxs-lookup"><span data-stu-id="f045e-165">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="f045e-166">**[FromQuery]** は他の任意のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="f045e-166">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="f045e-167">既定の推論規則は、*Startup.ConfigureServices* 内の次のコードで無効にされます。</span><span class="sxs-lookup"><span data-stu-id="f045e-167">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

<span data-ttu-id="f045e-168">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="f045e-168">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]</span></span>

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="f045e-169">マルチパート/フォーム データ要求の推論</span><span class="sxs-lookup"><span data-stu-id="f045e-169">Multipart/form-data request inference</span></span>

<span data-ttu-id="f045e-170">[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) 属性を使用してアクション パラメーターに注釈を付けた場合は、`multipart/form-data` 要求コンテンツ型が推論されます。</span><span class="sxs-lookup"><span data-stu-id="f045e-170">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="f045e-171">既定の動作は、*Startup.ConfigureServices* 内の次のコードで無効にされます。</span><span class="sxs-lookup"><span data-stu-id="f045e-171">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

<span data-ttu-id="f045e-172">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="f045e-172">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]</span></span>

### <a name="attribute-routing-requirement"></a><span data-ttu-id="f045e-173">属性ルーティング要件</span><span class="sxs-lookup"><span data-stu-id="f045e-173">Attribute routing requirement</span></span>

<span data-ttu-id="f045e-174">属性ルーティングは要件になります。</span><span class="sxs-lookup"><span data-stu-id="f045e-174">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="f045e-175">例:</span><span class="sxs-lookup"><span data-stu-id="f045e-175">For example:</span></span>

<span data-ttu-id="f045e-176">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="f045e-176">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]</span></span>

<span data-ttu-id="f045e-177">[UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) 内で定義されているか、または *Startup.Configure* 内の [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) によって定義されている[従来のルート](xref:mvc/controllers/routing#conventional-routing)を通してアクションにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="f045e-177">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="f045e-178">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="f045e-178">Additional resources</span></span>

* [<span data-ttu-id="f045e-179">コントローラー アクションの戻り値の型</span><span class="sxs-lookup"><span data-stu-id="f045e-179">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="f045e-180">カスタム フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="f045e-180">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="f045e-181">応答データの書式設定</span><span class="sxs-lookup"><span data-stu-id="f045e-181">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="f045e-182">Swagger を使用するヘルプ ページ</span><span class="sxs-lookup"><span data-stu-id="f045e-182">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="f045e-183">コントローラー アクションへのルーティング</span><span class="sxs-lookup"><span data-stu-id="f045e-183">Routing to controller actions</span></span>](xref:mvc/controllers/routing)
