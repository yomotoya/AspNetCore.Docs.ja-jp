---
title: ASP.NET Core で Web API を構築する
author: scottaddie
description: ASP.NET Core で Web API を構築するために使用できる機能、および各機能を使用する適切なタイミングについて説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: ccee4f7bae0abe1b36088d58e5c1e1362d8de9f0
ms.sourcegitcommit: 5338b1ed9e2ef225ab565d6cba072b474fd9324d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39243098"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="d1724-103">ASP.NET Core で Web API を構築する</span><span class="sxs-lookup"><span data-stu-id="d1724-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="d1724-104">作成者: [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="d1724-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="d1724-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="d1724-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d1724-106">このドキュメントでは、ASP.NET Core での各機能を使用する最も適切な web API をビルドする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d1724-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="d1724-107">ControllerBase からクラスを派生する</span><span class="sxs-lookup"><span data-stu-id="d1724-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="d1724-108">Web API として機能することを目的としたコント ローラー内の [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="d1724-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="d1724-109">例:</span><span class="sxs-lookup"><span data-stu-id="d1724-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="d1724-110">`ControllerBase` クラスを使用すると、いくつかのプロパティとメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="d1724-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="d1724-111">上のコードでは、例に [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) と [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d1724-111">In the preceding code, examples include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="d1724-112">これらのメソッドはアクション メソッド内で呼び出され、HTTP 400 および 201 のステータス コードをそれぞれ返します。</span><span class="sxs-lookup"><span data-stu-id="d1724-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="d1724-113">[ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) プロパティ (これも `ControllerBase` によって提供される) は、要求モデル検証を処理する場合にアクセスされます。</span><span class="sxs-lookup"><span data-stu-id="d1724-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="d1724-114">ApiControllerAttribute でクラスに注釈を付ける</span><span class="sxs-lookup"><span data-stu-id="d1724-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="d1724-115">ASP.NET Core 2.1 では、Web API コントローラー クラスを表す [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 属性が導入されました。</span><span class="sxs-lookup"><span data-stu-id="d1724-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="d1724-116">例:</span><span class="sxs-lookup"><span data-stu-id="d1724-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="d1724-117">この属性を使用するには、2.1 以降の互換性バージョン ([SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion) で設定) が必要です。</span><span class="sxs-lookup"><span data-stu-id="d1724-117">A compatibility version of 2.1 or later, set via [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), is required to use this attribute.</span></span> <span data-ttu-id="d1724-118">たとえば、*Startup.ConfigureServices* の強調表示されているコードでは、2.1 の互換性フラグが設定されます。</span><span class="sxs-lookup"><span data-stu-id="d1724-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="d1724-119">コントローラーの REST 固有の動作を有効にする場合、通常、`[ApiController]` 属性は `ControllerBase` と組み合わせて使用されます。</span><span class="sxs-lookup"><span data-stu-id="d1724-119">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="d1724-120">`ControllerBase` では、[NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) や [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file) などのメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="d1724-120">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="d1724-121">その他に、`[ApiController]` 属性で注釈が付けられたユーザー定義の基本コントローラー クラスを作成するという方法があります。</span><span class="sxs-lookup"><span data-stu-id="d1724-121">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="d1724-122">次のセクションでは、属性によって追加される便利な機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="d1724-122">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="d1724-123">自動的な HTTP 400 応答</span><span class="sxs-lookup"><span data-stu-id="d1724-123">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="d1724-124">検証エラーが発生すると、HTTP 400 応答が自動的にトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="d1724-124">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="d1724-125">次のコードは実際のアクションでは不要になります。</span><span class="sxs-lookup"><span data-stu-id="d1724-125">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="d1724-126">[SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) プロパティが `true` に設定されている場合、既定の動作は無効になります。</span><span class="sxs-lookup"><span data-stu-id="d1724-126">The default behavior is disabled when the [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) property is set to `true`.</span></span> <span data-ttu-id="d1724-127">*Startup.ConfigureServices* の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d1724-127">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="d1724-128">バインディング ソース パラメーター推論</span><span class="sxs-lookup"><span data-stu-id="d1724-128">Binding source parameter inference</span></span>

<span data-ttu-id="d1724-129">バインディング ソース属性では、アクション パラメーターの値が存在する場所が定義されます。</span><span class="sxs-lookup"><span data-stu-id="d1724-129">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="d1724-130">次のバインディング ソース属性が存在します。</span><span class="sxs-lookup"><span data-stu-id="d1724-130">The following binding source attributes exist:</span></span>

|<span data-ttu-id="d1724-131">属性</span><span class="sxs-lookup"><span data-stu-id="d1724-131">Attribute</span></span>|<span data-ttu-id="d1724-132">バインド ソース</span><span class="sxs-lookup"><span data-stu-id="d1724-132">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="d1724-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="d1724-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="d1724-134">要求本文</span><span class="sxs-lookup"><span data-stu-id="d1724-134">Request body</span></span> |
|<span data-ttu-id="d1724-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="d1724-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="d1724-136">要求本文内のフォーム データ</span><span class="sxs-lookup"><span data-stu-id="d1724-136">Form data in the request body</span></span> |
|<span data-ttu-id="d1724-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="d1724-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="d1724-138">要求ヘッダー</span><span class="sxs-lookup"><span data-stu-id="d1724-138">Request header</span></span> |
|<span data-ttu-id="d1724-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="d1724-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="d1724-140">要求のクエリ文字列パラメーター</span><span class="sxs-lookup"><span data-stu-id="d1724-140">Request query string parameter</span></span> |
|<span data-ttu-id="d1724-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="d1724-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="d1724-142">現在の要求からのルート データ</span><span class="sxs-lookup"><span data-stu-id="d1724-142">Route data from the current request</span></span> |
|<span data-ttu-id="d1724-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="d1724-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="d1724-144">アクション パラメーターとして挿入される要求サービス</span><span class="sxs-lookup"><span data-stu-id="d1724-144">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="d1724-145">値に `%2f` (つまり、`/`) が含まれる可能性がある場合は、`[FromRoute]` を使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="d1724-145">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="d1724-146">`%2f` は `/` にエスケープ解除されません。</span><span class="sxs-lookup"><span data-stu-id="d1724-146">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="d1724-147">値に `%2f` が含まれる可能性がある場合は、`[FromQuery]` を使用してください。</span><span class="sxs-lookup"><span data-stu-id="d1724-147">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="d1724-148">`[ApiController]` 属性がない場合は、バインディング ソース属性を明示的に定義します。</span><span class="sxs-lookup"><span data-stu-id="d1724-148">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="d1724-149">次の例では、`discontinuedOnly` パラメーター値が要求 URL のクエリ文字列に指定されていることが `[FromQuery]` によって示されています。</span><span class="sxs-lookup"><span data-stu-id="d1724-149">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="d1724-150">アクション パラメーターの既定のデータ ソースには推論規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="d1724-150">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="d1724-151">これらの規則によって、アクション パラメーターに、通常、手動で適用する可能性の高いバインディング ソースが構成されます。</span><span class="sxs-lookup"><span data-stu-id="d1724-151">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="d1724-152">バインディング ソースの属性は、次のように動作します。</span><span class="sxs-lookup"><span data-stu-id="d1724-152">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="d1724-153">**[FromBody]** は複合型のパラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="d1724-153">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="d1724-154">この規則には例外があり、[IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) や [CancellationToken](/dotnet/api/system.threading.cancellationtoken) などの、特殊な意味を持つ複雑な組み込み型が該当します。</span><span class="sxs-lookup"><span data-stu-id="d1724-154">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="d1724-155">バインディング ソース推論コードでは、そのような特殊な型は無視されます。</span><span class="sxs-lookup"><span data-stu-id="d1724-155">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="d1724-156">アクションの複数のパラメーターが明示的に指定されている場合 (`[FromBody]` によって) または要求本文からバインドとして推論される場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="d1724-156">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="d1724-157">たとえば、次のアクション シグネチャは例外の原因となります。</span><span class="sxs-lookup"><span data-stu-id="d1724-157">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="d1724-158">**[FromForm]** は [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) および [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection) 型のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="d1724-158">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="d1724-159">簡易型またはユーザー定義型に対しては推論されません。</span><span class="sxs-lookup"><span data-stu-id="d1724-159">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="d1724-160">**[FromRoute]** は、ルート テンプレート内のパラメーターと一致する任意のアクション パラメーター名に対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="d1724-160">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="d1724-161">複数のルートがアクション パラメーターと一致する場合、ルート値はいずれも `[FromRoute]` と見なされます。</span><span class="sxs-lookup"><span data-stu-id="d1724-161">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="d1724-162">**[FromQuery]** は他の任意のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="d1724-162">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="d1724-163">[SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) プロパティが `true` に設定されている場合、既定の推論規則は無効になります。</span><span class="sxs-lookup"><span data-stu-id="d1724-163">The default inference rules are disabled when the [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) property is set to `true`.</span></span> <span data-ttu-id="d1724-164">*Startup.ConfigureServices* の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d1724-164">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="d1724-165">マルチパート/フォーム データ要求の推論</span><span class="sxs-lookup"><span data-stu-id="d1724-165">Multipart/form-data request inference</span></span>

<span data-ttu-id="d1724-166">[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) 属性を使用してアクション パラメーターに注釈を付けた場合は、`multipart/form-data` 要求コンテンツ型が推論されます。</span><span class="sxs-lookup"><span data-stu-id="d1724-166">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="d1724-167">[SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) プロパティが `true` に設定されている場合、既定の動作は無効になります。</span><span class="sxs-lookup"><span data-stu-id="d1724-167">The default behavior is disabled when the [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) property is set to `true`.</span></span> <span data-ttu-id="d1724-168">*Startup.ConfigureServices* の `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` の後に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d1724-168">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="d1724-169">属性ルーティング要件</span><span class="sxs-lookup"><span data-stu-id="d1724-169">Attribute routing requirement</span></span>

<span data-ttu-id="d1724-170">属性ルーティングは要件になります。</span><span class="sxs-lookup"><span data-stu-id="d1724-170">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="d1724-171">例:</span><span class="sxs-lookup"><span data-stu-id="d1724-171">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="d1724-172">[UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) 内で定義されているか、または *Startup.Configure* 内の [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) によって定義されている[従来のルート](xref:mvc/controllers/routing#conventional-routing)を通してアクションにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="d1724-172">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d1724-173">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="d1724-173">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
