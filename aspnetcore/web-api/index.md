---
title: ASP.NET Core を使って Web API を作成する
author: scottaddie
description: ASP.NET Core での Web API の作成の基本について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/11/2019
uid: web-api/index
ms.openlocfilehash: d804a7f1b4f0e89f433a3674116c97804705f7cc
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64882957"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="26118-103">ASP.NET Core を使って Web API を作成する</span><span class="sxs-lookup"><span data-stu-id="26118-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="26118-104">作成者: [Scott Addie](https://github.com/scottaddie)、[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="26118-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="26118-105">ASP.NET Core では、C# を使った RESTful サービス (別名: Web API) の作成がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="26118-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="26118-106">要求を処理するために、Web API ではコントローラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="26118-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="26118-107">Web API の "*コントローラー*" は `ControllerBase` から派生するクラスです。</span><span class="sxs-lookup"><span data-stu-id="26118-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="26118-108">この記事では、コントローラーを使って API 要求を処理する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="26118-108">This article shows how to use controllers for handling API requests.</span></span>

<span data-ttu-id="26118-109">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples)します。</span><span class="sxs-lookup"><span data-stu-id="26118-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="26118-110">([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="26118-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="26118-111">ControllerBase クラス</span><span class="sxs-lookup"><span data-stu-id="26118-111">ControllerBase class</span></span>

<span data-ttu-id="26118-112">Web API には、<xref:Microsoft.AspNetCore.Mvc.ControllerBase> から派生したコントローラー クラスが 1 つ以上含まれます。</span><span class="sxs-lookup"><span data-stu-id="26118-112">A web API has one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="26118-113">たとえば、Web API のプロジェクト テンプレートでは Values コントローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="26118-113">For example, the web API project template creates a Values controller:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=3)]

<span data-ttu-id="26118-114"><xref:Microsoft.AspNetCore.Mvc.Controller> 基底クラスから派生させて Web API のコントローラーを作成しないでください。</span><span class="sxs-lookup"><span data-stu-id="26118-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class.</span></span> <span data-ttu-id="26118-115">`ControllerBase` から派生した `Controller` にはビューのサポートが追加されるため、これは Web API 要求ではなく Web ページを処理するためのものです。</span><span class="sxs-lookup"><span data-stu-id="26118-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span>  <span data-ttu-id="26118-116">このルールには例外があります。ビューと API の両方で同じコントローラーを使うことを計画している場合は、`Controller` から派生させます。</span><span class="sxs-lookup"><span data-stu-id="26118-116">There's an exception to this rule: if you plan to use the same controller for both views and APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="26118-117">`ControllerBase` クラスには、HTTP 要求の処理に役立つプロパティとメソッドが多数用意されています。</span><span class="sxs-lookup"><span data-stu-id="26118-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="26118-118">たとえば、`ControllerBase.CreatedAtAction` では状態コード 201 が返されます。</span><span class="sxs-lookup"><span data-stu-id="26118-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=8-9)]

 <span data-ttu-id="26118-119">次に、`ControllerBase` に用意されているメソッドの例をさらにいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="26118-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="26118-120">メソッド</span><span class="sxs-lookup"><span data-stu-id="26118-120">Method</span></span>  |<span data-ttu-id="26118-121">メモ</span><span class="sxs-lookup"><span data-stu-id="26118-121">Notes</span></span>  |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| <span data-ttu-id="26118-122">400 状態コードを返します。</span><span class="sxs-lookup"><span data-stu-id="26118-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> |<span data-ttu-id="26118-123">404 状態コードを返します。</span><span class="sxs-lookup"><span data-stu-id="26118-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|<span data-ttu-id="26118-124">ファイルを返します。</span><span class="sxs-lookup"><span data-stu-id="26118-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|<span data-ttu-id="26118-125">[モデル バインド](xref:mvc/models/model-binding)を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="26118-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|<span data-ttu-id="26118-126">[モデル検証](xref:mvc/models/validation)を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="26118-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="26118-127">使用可能なすべてのメソッドとプロパティの一覧については、「<xref:Microsoft.AspNetCore.Mvc.ControllerBase>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="26118-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="26118-128">属性</span><span class="sxs-lookup"><span data-stu-id="26118-128">Attributes</span></span>

<span data-ttu-id="26118-129"><xref:Microsoft.AspNetCore.Mvc> 名前空間には、Web API のコントローラーとアクション メソッドの動作の構成に使用できる属性が用意されています。</span><span class="sxs-lookup"><span data-stu-id="26118-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="26118-130">次の例では、属性を使って、受け取る HTTP メソッドと返す状態コードを指定しています。</span><span class="sxs-lookup"><span data-stu-id="26118-130">The following example uses attributes to specify the HTTP method accepted and the status codes returned:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="26118-131">次に、使用できる属性の例をさらにいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="26118-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="26118-132">属性</span><span class="sxs-lookup"><span data-stu-id="26118-132">Attribute</span></span>|<span data-ttu-id="26118-133">メモ</span><span class="sxs-lookup"><span data-stu-id="26118-133">Notes</span></span>|
|---------|-----|
|<span data-ttu-id="26118-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span><span class="sxs-lookup"><span data-stu-id="26118-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span></span>      |<span data-ttu-id="26118-135">コントローラーまたはアクションの URL パターンを指定します。</span><span class="sxs-lookup"><span data-stu-id="26118-135">Specifies URL pattern for a controller or action.</span></span>|
|<span data-ttu-id="26118-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span><span class="sxs-lookup"><span data-stu-id="26118-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span></span>        |<span data-ttu-id="26118-137">モデル バインドのために含めるプレフィックスとプロパティを指定します。</span><span class="sxs-lookup"><span data-stu-id="26118-137">Specifies prefix and properties to include for model binding.</span></span>|
|<span data-ttu-id="26118-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span><span class="sxs-lookup"><span data-stu-id="26118-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span></span>  |<span data-ttu-id="26118-139">HTTP GET メソッドをサポートするアクションを特定します。</span><span class="sxs-lookup"><span data-stu-id="26118-139">Identifies an action that supports the HTTP GET method.</span></span>|
|<span data-ttu-id="26118-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="26118-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span></span>|<span data-ttu-id="26118-141">アクションが受け取るデータ型を指定します。</span><span class="sxs-lookup"><span data-stu-id="26118-141">Specifies data types that an action accepts.</span></span>|
|<span data-ttu-id="26118-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="26118-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span></span>|<span data-ttu-id="26118-143">アクションによって返すデータ型を指定します。</span><span class="sxs-lookup"><span data-stu-id="26118-143">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="26118-144">使用可能な属性を含む一覧については、<xref:Microsoft.AspNetCore.Mvc> 名前空間をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="26118-144">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="26118-145">ApiController 属性</span><span class="sxs-lookup"><span data-stu-id="26118-145">ApiController attribute</span></span>

<span data-ttu-id="26118-146">[[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 属性をコントローラー クラスに適用して、API に固有の動作を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="26118-146">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable API-specific behaviors:</span></span>

* [<span data-ttu-id="26118-147">属性ルーティング要件</span><span class="sxs-lookup"><span data-stu-id="26118-147">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="26118-148">自動的な HTTP 400 応答</span><span class="sxs-lookup"><span data-stu-id="26118-148">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="26118-149">バインディング ソース パラメーター推論</span><span class="sxs-lookup"><span data-stu-id="26118-149">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="26118-150">マルチパート/フォーム データ要求の推論</span><span class="sxs-lookup"><span data-stu-id="26118-150">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="26118-151">エラー状態コードに関する問題の詳細</span><span class="sxs-lookup"><span data-stu-id="26118-151">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="26118-152">これらの機能では[互換性バージョン](<xref:mvc/compatibility-version>) 2.1 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="26118-152">These features require a [compatibility version](<xref:mvc/compatibility-version>) of 2.1 or later.</span></span>

### <a name="apicontroller-on-specific-controllers"></a><span data-ttu-id="26118-153">特定のコントローラーでの ApiController</span><span class="sxs-lookup"><span data-stu-id="26118-153">ApiController on specific controllers</span></span>

<span data-ttu-id="26118-154">プロジェクト テンプレートからの次の例のように、`[ApiController]` 属性は特定のコントローラーに適用できます。</span><span class="sxs-lookup"><span data-stu-id="26118-154">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=2)]

### <a name="apicontroller-on-multiple-controllers"></a><span data-ttu-id="26118-155">複数のコントローラーでの ApiController</span><span class="sxs-lookup"><span data-stu-id="26118-155">ApiController on multiple controllers</span></span>

<span data-ttu-id="26118-156">複数のコントローラーでこの属性を使う方法の 1 つは、`[ApiController]` 属性で注釈を付けたカスタム基本コントローラー クラスを作成することです。</span><span class="sxs-lookup"><span data-stu-id="26118-156">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="26118-157">次は、カスタム基底クラスとそこから派生したコントローラーを示す例です。</span><span class="sxs-lookup"><span data-stu-id="26118-157">Here's an example showing a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

### <a name="apicontroller-on-an-assembly"></a><span data-ttu-id="26118-158">アセンブリでの ApiController</span><span class="sxs-lookup"><span data-stu-id="26118-158">ApiController on an assembly</span></span>

<span data-ttu-id="26118-159">[互換性バージョン](<xref:mvc/compatibility-version>)が 2.2 以降に設定されている場合は、`[ApiController]` 属性をアセンブリに適用できます。</span><span class="sxs-lookup"><span data-stu-id="26118-159">If [compatibility version](<xref:mvc/compatibility-version>) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="26118-160">この方法での注釈では、アセンブリ内のすべてのコントローラーに Web API の動作が適用されます。</span><span class="sxs-lookup"><span data-stu-id="26118-160">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="26118-161">個別のコントローラーを除外する方法はありません。</span><span class="sxs-lookup"><span data-stu-id="26118-161">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="26118-162">次の例に示すように、アセンブリ レベルの属性を `Startup` クラスに適用します。</span><span class="sxs-lookup"><span data-stu-id="26118-162">Apply the assembly-level attribute to the `Startup` class as shown in the following example:</span></span>

```csharp
[assembly: ApiController]
namespace WebApiSample
{
    public class Startup
    {
        ...
    }
}
```

## <a name="attribute-routing-requirement"></a><span data-ttu-id="26118-163">属性ルーティング要件</span><span class="sxs-lookup"><span data-stu-id="26118-163">Attribute routing requirement</span></span>

<span data-ttu-id="26118-164">`ApiController` 属性では、属性ルーティング要件が作成されます。</span><span class="sxs-lookup"><span data-stu-id="26118-164">The `ApiController` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="26118-165">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="26118-165">For example:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=1)]

<span data-ttu-id="26118-166">`Startup.Configure` の <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> または <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> で定義された[規則ルート](xref:mvc/controllers/routing#conventional-routing)経由でアクションにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="26118-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

## <a name="automatic-http-400-responses"></a><span data-ttu-id="26118-167">自動的な HTTP 400 応答</span><span class="sxs-lookup"><span data-stu-id="26118-167">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="26118-168">`ApiController` 属性により、モデル検証エラーが発生すると HTTP 400 応答が自動的にトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="26118-168">The `ApiController` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="26118-169">その結果、アクション メソッド内の次のコードは不要になります。</span><span class="sxs-lookup"><span data-stu-id="26118-169">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

### <a name="default-badrequest-response"></a><span data-ttu-id="26118-170">既定の BadRequest 応答</span><span class="sxs-lookup"><span data-stu-id="26118-170">Default BadRequest response</span></span> 

<span data-ttu-id="26118-171">2.2 以降の互換性バージョンを使う場合、HTTP 400 応答に対する既定の応答の種類は <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> です。</span><span class="sxs-lookup"><span data-stu-id="26118-171">With a compatibility version of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="26118-172">`ValidationProblemDetails` 型は [RFC 7807 仕様](https://tools.ietf.org/html/rfc7807)に準拠しています。</span><span class="sxs-lookup"><span data-stu-id="26118-172">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

<span data-ttu-id="26118-173"><xref:Microsoft.AspNetCore.Mvc.SerializableError> に対する既定の応答を変更するには、次の例のように、`Startup.ConfigureServices` で `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` プロパティを `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="26118-173">To change the default response to <xref:Microsoft.AspNetCore.Mvc.SerializableError>, set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,9)]

### <a name="customize-badrequest-response"></a><span data-ttu-id="26118-174">BadRequest 応答をカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="26118-174">Customize BadRequest response</span></span>

<span data-ttu-id="26118-175">検証エラーに起因する応答をカスタマイズするには、<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> を使います。</span><span class="sxs-lookup"><span data-stu-id="26118-175">To customize the response that results from a validation error, use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span></span> <span data-ttu-id="26118-176">`services.AddMvc().SetCompatibilityVersion` の後に、次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="26118-176">Add the following highlighted code after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=3-20)]

### <a name="disable-automatic-400"></a><span data-ttu-id="26118-177">自動的な 400 を無効にする</span><span class="sxs-lookup"><span data-stu-id="26118-177">Disable automatic 400</span></span>

<span data-ttu-id="26118-178">自動的な 400 の動作を無効にするには、<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> プロパティを `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="26118-178">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="26118-179">次の強調表示されたコードを、`Startup.ConfigureServices` の `services.AddMvc().SetCompatibilityVersion` の後に追加します。</span><span class="sxs-lookup"><span data-stu-id="26118-179">Add the following highlighted code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="26118-180">バインディング ソース パラメーター推論</span><span class="sxs-lookup"><span data-stu-id="26118-180">Binding source parameter inference</span></span>

<span data-ttu-id="26118-181">バインディング ソース属性では、アクション パラメーターの値が存在する場所が定義されます。</span><span class="sxs-lookup"><span data-stu-id="26118-181">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="26118-182">次のバインディング ソース属性が存在します。</span><span class="sxs-lookup"><span data-stu-id="26118-182">The following binding source attributes exist:</span></span>

|<span data-ttu-id="26118-183">属性</span><span class="sxs-lookup"><span data-stu-id="26118-183">Attribute</span></span>|<span data-ttu-id="26118-184">バインド ソース</span><span class="sxs-lookup"><span data-stu-id="26118-184">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="26118-185">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span><span class="sxs-lookup"><span data-stu-id="26118-185">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span></span>     | <span data-ttu-id="26118-186">要求本文</span><span class="sxs-lookup"><span data-stu-id="26118-186">Request body</span></span> |
|<span data-ttu-id="26118-187">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span><span class="sxs-lookup"><span data-stu-id="26118-187">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span></span>     | <span data-ttu-id="26118-188">要求本文内のフォーム データ</span><span class="sxs-lookup"><span data-stu-id="26118-188">Form data in the request body</span></span> |
|<span data-ttu-id="26118-189">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span><span class="sxs-lookup"><span data-stu-id="26118-189">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span></span> | <span data-ttu-id="26118-190">要求ヘッダー</span><span class="sxs-lookup"><span data-stu-id="26118-190">Request header</span></span> |
|<span data-ttu-id="26118-191">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span><span class="sxs-lookup"><span data-stu-id="26118-191">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span></span>   | <span data-ttu-id="26118-192">要求のクエリ文字列パラメーター</span><span class="sxs-lookup"><span data-stu-id="26118-192">Request query string parameter</span></span> |
|<span data-ttu-id="26118-193">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="26118-193">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span></span>   | <span data-ttu-id="26118-194">現在の要求からのルート データ</span><span class="sxs-lookup"><span data-stu-id="26118-194">Route data from the current request</span></span> |
|<span data-ttu-id="26118-195">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span><span class="sxs-lookup"><span data-stu-id="26118-195">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span></span> | <span data-ttu-id="26118-196">アクション パラメーターとして挿入される要求サービス</span><span class="sxs-lookup"><span data-stu-id="26118-196">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="26118-197">値に `%2f` (つまり、`/`) が含まれる可能性がある場合は、`[FromRoute]` を使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="26118-197">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="26118-198">`%2f` は `/` にエスケープ解除されません。</span><span class="sxs-lookup"><span data-stu-id="26118-198">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="26118-199">値に `%2f` が含まれる可能性がある場合は、`[FromQuery]` を使用してください。</span><span class="sxs-lookup"><span data-stu-id="26118-199">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="26118-200">`[ApiController]` 属性や `[FromQuery]` などのバインディング ソース属性がない場合は、ASP.NET Core ランタイムにより複合オブジェクト モデル バインダーの使用が試行されます。</span><span class="sxs-lookup"><span data-stu-id="26118-200">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="26118-201">複合オブジェクト モデル バインダーでは、値プロバイダーから定義された順序でデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="26118-201">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="26118-202">次の例では、`discontinuedOnly` パラメーター値が要求 URL のクエリ文字列に指定されていることが `[FromQuery]` によって示されています。</span><span class="sxs-lookup"><span data-stu-id="26118-202">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="26118-203">`[ApiController]` 属性により、アクション パラメーターの既定のデータ ソースに対する推論規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="26118-203">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="26118-204">これらの規則により、アクション パラメーターに属性を適用することで手動でバインディング ソースを特定する必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="26118-204">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="26118-205">バインディング ソースの推論規則は、次のように動作します。</span><span class="sxs-lookup"><span data-stu-id="26118-205">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="26118-206">`[FromBody]` は複合型パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="26118-206">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="26118-207">`[FromBody]` 推論規則に対する例外は、<xref:Microsoft.AspNetCore.Http.IFormCollection> や <xref:System.Threading.CancellationToken> など、特殊な意味を持つ組み込みの複合型です。</span><span class="sxs-lookup"><span data-stu-id="26118-207">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="26118-208">バインディング ソース推論コードでは、そのような特殊な型は無視されます。</span><span class="sxs-lookup"><span data-stu-id="26118-208">The binding source inference code ignores those special types.</span></span> 
* <span data-ttu-id="26118-209">`[FromForm]` は <xref:Microsoft.AspNetCore.Http.IFormFile> および <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 型のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="26118-209">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="26118-210">簡易型またはユーザー定義型に対しては推論されません。</span><span class="sxs-lookup"><span data-stu-id="26118-210">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="26118-211">`[FromRoute]` は、ルート テンプレート内のパラメーターと一致する任意のアクション パラメーター名に対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="26118-211">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="26118-212">複数のルートがアクション パラメーターと一致する場合、ルート値はいずれも `[FromRoute]` と見なされます。</span><span class="sxs-lookup"><span data-stu-id="26118-212">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="26118-213">`[FromQuery]` は他の任意のアクション パラメーターに対して推論されます。</span><span class="sxs-lookup"><span data-stu-id="26118-213">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="26118-214">FromBody 推論に関するメモ</span><span class="sxs-lookup"><span data-stu-id="26118-214">FromBody inference notes</span></span>

<span data-ttu-id="26118-215">`[FromBody]` は、`string` や `int` などの単純型に対しては推論されません。</span><span class="sxs-lookup"><span data-stu-id="26118-215">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="26118-216">そのため、その機能が必要な場合、単純型に対しては `[FromBody]` 属性を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="26118-216">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="26118-217">要求本文からバインドされるパラメーターがアクションに複数ある場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="26118-217">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="26118-218">たとえば、次のアクション メソッドのシグネチャはすべて例外の原因となります。</span><span class="sxs-lookup"><span data-stu-id="26118-218">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="26118-219">複合型であるため、両方に対して `[FromBody]` が推論されます。</span><span class="sxs-lookup"><span data-stu-id="26118-219">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="26118-220">1 つには `[FromBody]` 属性が使われ、もう 1 つは複合型なので推論されます。</span><span class="sxs-lookup"><span data-stu-id="26118-220">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="26118-221">両方で `[FromBody]` 属性が使われます。</span><span class="sxs-lookup"><span data-stu-id="26118-221">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

> [!NOTE]
> <span data-ttu-id="26118-222">ASP.NET Core 2.1 では、リストや配列などのコレクション型パラメーターが誤って `[FromQuery]` と推論されます。</span><span class="sxs-lookup"><span data-stu-id="26118-222">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="26118-223">これらのパラメーターを要求本文からバインドする場合は、`[FromBody]` 属性を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="26118-223">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="26118-224">この動作は ASP.NET Core 2.2 以降で修正されており、既定でコレクション型パラメーターが本文からバインドされることが推論されます。</span><span class="sxs-lookup"><span data-stu-id="26118-224">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

### <a name="disable-inference-rules"></a><span data-ttu-id="26118-225">推論規則を無効にする</span><span class="sxs-lookup"><span data-stu-id="26118-225">Disable inference rules</span></span>

<span data-ttu-id="26118-226">バインディング ソースの推論を無効にするには、<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="26118-226">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="26118-227">`Startup.ConfigureServices` の `services.AddMvc().SetCompatibilityVersion` の後ろに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="26118-227">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="26118-228">マルチパート/フォーム データ要求の推論</span><span class="sxs-lookup"><span data-stu-id="26118-228">Multipart/form-data request inference</span></span>

<span data-ttu-id="26118-229">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) 属性を使用してアクション パラメーターに注釈を付けた場合、`[ApiController]` 属性が推論規則に適用されます。つまり、`multipart/form-data` 要求コンテンツ型が推論されます。</span><span class="sxs-lookup"><span data-stu-id="26118-229">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute: the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="26118-230">既定の動作を無効にするには、次の例のように、`Startup.ConfigureServices` で <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="26118-230">To disable the default behavior, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters>  to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="26118-231">エラー状態コードに関する問題の詳細</span><span class="sxs-lookup"><span data-stu-id="26118-231">Problem details for error status codes</span></span>

<span data-ttu-id="26118-232">互換性バージョンが 2.2 以上である場合、MVC によってエラー結果 (状態コードが 400 以上の結果) が <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> を含む結果に変換されます。</span><span class="sxs-lookup"><span data-stu-id="26118-232">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="26118-233">`ProblemDetails` 型は、HTTP 応答でコンピューターが判読できるエラーの詳細を提供するための [RFC 7807 仕様](https://tools.ietf.org/html/rfc7807)に基づきます。</span><span class="sxs-lookup"><span data-stu-id="26118-233">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="26118-234">コントローラー アクションで次のコードがあるとします。</span><span class="sxs-lookup"><span data-stu-id="26118-234">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="26118-235">`NotFound` の HTTP 応答には 404 状態コードと `ProblemDetails` の本文が含まれています。</span><span class="sxs-lookup"><span data-stu-id="26118-235">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="26118-236">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="26118-236">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a><span data-ttu-id="26118-237">ProblemDetails 応答をカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="26118-237">Customize ProblemDetails response</span></span>

<span data-ttu-id="26118-238">`ProblemDetails` の応答の内容を構成するには、`ClientErrorMapping` プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="26118-238">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="26118-239">たとえば、次のコードにより、404 応答の `type` プロパティが更新されます。</span><span class="sxs-lookup"><span data-stu-id="26118-239">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

### <a name="disable-problemdetails-response"></a><span data-ttu-id="26118-240">ProblemDetails 応答を無効にする</span><span class="sxs-lookup"><span data-stu-id="26118-240">Disable ProblemDetails response</span></span>

<span data-ttu-id="26118-241">`SuppressMapClientErrors` プロパティが `true` に設定されている場合、`ProblemDetails` の自動作成は無効になります。</span><span class="sxs-lookup"><span data-stu-id="26118-241">The automatic creation of `ProblemDetails` is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="26118-242">`Startup.ConfigureServices` に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="26118-242">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

## <a name="additional-resources"></a><span data-ttu-id="26118-243">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="26118-243">Additional resources</span></span> 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
