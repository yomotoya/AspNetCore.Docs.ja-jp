---
title: ASP.NET Core Web API のコントローラー アクションの戻り値の型
author: scottaddie
description: ASP.NET Core Web API でのさまざまなコントローラー アクション メソッドの戻り値の型の使用について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2018
uid: web-api/action-return-types
ms.openlocfilehash: 82d18d866d4d18613cccb950b2f30ae81bd749de
ms.sourcegitcommit: 6425baa92cec4537368705f8d27f3d0e958e43cd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/24/2018
ms.locfileid: "39220613"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="0180f-103">ASP.NET Core Web API のコントローラー アクションの戻り値の型</span><span class="sxs-lookup"><span data-stu-id="0180f-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="0180f-104">作成者: [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="0180f-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="0180f-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="0180f-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0180f-106">ASP.NET Core では、Web API コントローラー アクションの戻り値の型に次のオプションを提供しています。</span><span class="sxs-lookup"><span data-stu-id="0180f-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="0180f-107">特定の型</span><span class="sxs-lookup"><span data-stu-id="0180f-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="0180f-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="0180f-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="0180f-109">ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="0180f-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="0180f-110">特定の型</span><span class="sxs-lookup"><span data-stu-id="0180f-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="0180f-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="0180f-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="0180f-112">このドキュメントでは、各戻り値の型を使用するのが最適な場合について説明します。</span><span class="sxs-lookup"><span data-stu-id="0180f-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="0180f-113">特定の型</span><span class="sxs-lookup"><span data-stu-id="0180f-113">Specific type</span></span>

<span data-ttu-id="0180f-114">最もシンプルなアクションでは、プリミティブ データ型または複合データ型が返されます (`string` やカスタム オブジェクトの型など)。</span><span class="sxs-lookup"><span data-stu-id="0180f-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="0180f-115">カスタム `Product` オブジェクトのコレクションを返す次のアクションを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="0180f-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="0180f-116">アクションの実行中に保護できる既知の条件がない場合は、特定の型を返すだけで十分です。</span><span class="sxs-lookup"><span data-stu-id="0180f-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="0180f-117">前のアクションでパラメーターを受け取っていないので、パラメーターの制約の検証は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="0180f-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="0180f-118">アクションで既知の条件を考慮する必要がある場合は、複数の戻り値のパスが導入されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="0180f-119">このような場合、[ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) の戻り値の型とプリミティブまたは複合の戻り値の型を混在させるのが一般的です。</span><span class="sxs-lookup"><span data-stu-id="0180f-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="0180f-120">この種類のアクションに対応するには、[IActionResult](#iactionresult-type) または [ActionResult\<T>](#actionresultt-type) のいずれかが必要です。</span><span class="sxs-lookup"><span data-stu-id="0180f-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="0180f-121">IActionResult 型</span><span class="sxs-lookup"><span data-stu-id="0180f-121">IActionResult type</span></span>

<span data-ttu-id="0180f-122">[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) の戻り値の型は、アクションで複数の [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) の戻り値の型が可能な場合に適しています。</span><span class="sxs-lookup"><span data-stu-id="0180f-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="0180f-123">`ActionResult` 型は、さまざまな HTTP 状態コードを表します。</span><span class="sxs-lookup"><span data-stu-id="0180f-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="0180f-124">このカテゴリに当てはまる一般的な戻り値の型には、[BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400)、[NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404)、[OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200) などがあります。</span><span class="sxs-lookup"><span data-stu-id="0180f-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="0180f-125">アクションには複数の戻り値の型とパスがあるため、[[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) 属性を十分に使える必要があります。</span><span class="sxs-lookup"><span data-stu-id="0180f-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="0180f-126">この属性は、[Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger) などのツールで生成される API ヘルプ ページのよりわかりやすい応答の詳細を生成します。</span><span class="sxs-lookup"><span data-stu-id="0180f-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="0180f-127">`[ProducesResponseType]` は、アクションによって返される既知の型と HTTP 状態コードを示します。</span><span class="sxs-lookup"><span data-stu-id="0180f-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="0180f-128">同期アクション</span><span class="sxs-lookup"><span data-stu-id="0180f-128">Synchronous action</span></span>

<span data-ttu-id="0180f-129">2 つの戻り値の型が考えられる、次の同期アクションを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="0180f-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="0180f-130">前のアクションでは、`id` によって表される製品が、基になるデータ ストアに存在しないと、404 状態コードが返されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="0180f-131">`return new NotFoundResult();` へのショートカットとして、[NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) ヘルパー メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="0180f-132">製品が存在する場合は、ペイロードを表す `Product` オブジェクトが、200 状態コードと共に返されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="0180f-133">[Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) ヘルパー メソッドは、`return new OkObjectResult(product);` の短縮形式として呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="0180f-134">非同期アクション</span><span class="sxs-lookup"><span data-stu-id="0180f-134">Asynchronous action</span></span>

<span data-ttu-id="0180f-135">2 つの戻り値の型が考えられる、次の非同期アクションを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="0180f-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="0180f-136">前のアクションでは、モデルの検証に失敗し、[BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) ヘルパー メソッドが呼び出されるときに、400 状態コードが返されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-136">In the preceding action, a 400 status code is returned when model validation fails and the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) helper method is invoked.</span></span> <span data-ttu-id="0180f-137">たとえば、次のモデルでは、要求で `Name` プロパティと値を提供する必要があることを示しています。</span><span class="sxs-lookup"><span data-stu-id="0180f-137">For example, the following model indicates that requests must provide the `Name` property and a value.</span></span> <span data-ttu-id="0180f-138">そのため、要求で適切な `Name` を提供するのに失敗すると、モデルの検証が失敗します。</span><span class="sxs-lookup"><span data-stu-id="0180f-138">Therefore, failure to provide a proper `Name` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

<span data-ttu-id="0180f-139">前のアクションのその他の既知のリターン コードは、[CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) ヘルパー メソッドで生成される 201 です。</span><span class="sxs-lookup"><span data-stu-id="0180f-139">The preceding action's other known return code is a 201, which is generated by the [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) helper method.</span></span> <span data-ttu-id="0180f-140">このパスでは、`Product` オブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-140">In this path, the `Product` object is returned.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="actionresultt-type"></a><span data-ttu-id="0180f-141">ActionResult\<T> 型</span><span class="sxs-lookup"><span data-stu-id="0180f-141">ActionResult\<T> type</span></span>

<span data-ttu-id="0180f-142">ASP.NET Core 2.1 では、Web API コントローラー アクションに対して、[ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) の戻り値の型が導入されました。</span><span class="sxs-lookup"><span data-stu-id="0180f-142">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="0180f-143">これにより、[ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) から派生する型、または[特定の型](#specific-type)を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="0180f-143">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="0180f-144">`ActionResult<T>` により、[IActionResult 型](#iactionresult-type)は次の利点を得られます。</span><span class="sxs-lookup"><span data-stu-id="0180f-144">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="0180f-145">[[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) 属性の `Type` プロパティを除外することができます。</span><span class="sxs-lookup"><span data-stu-id="0180f-145">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="0180f-146">たとえば、`[ProducesResponseType(200, Type = typeof(Product))]` は `[ProducesResponseType(200)]` に簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-146">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="0180f-147">アクションの予期される戻り値の型は、代わりに `ActionResult<T>` の `T` から推論されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-147">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="0180f-148">[暗黙的なキャスト演算子](/dotnet/csharp/language-reference/keywords/implicit)は、`T` と `ActionResult` の両方の `ActionResult<T>` への変換をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="0180f-148">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="0180f-149">`T` は [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult) に変換されます。つまり、`return new ObjectResult(T);` は `return T;` に簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-149">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="0180f-150">C# はインターフェイス上での暗黙的なキャスト演算子をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="0180f-150">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="0180f-151">そのため、`ActionResult<T>` を使用するには、インターフェイスを具象型に変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0180f-151">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="0180f-152">たとえば、次の例における `IEnumerable` の使用は機能しません。</span><span class="sxs-lookup"><span data-stu-id="0180f-152">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

    ```csharp
    [HttpGet]
    public ActionResult<IEnumerable<Product>> Get()
    {
        return _repository.GetProducts();
    }
    ```

<span data-ttu-id="0180f-153">上記のコードを修正するための選択肢の 1 つは、`_repository.GetProducts().ToList();` を返すことです。</span><span class="sxs-lookup"><span data-stu-id="0180f-153">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="0180f-154">ほとんどのアクションには、特定の戻り値の型があります。</span><span class="sxs-lookup"><span data-stu-id="0180f-154">Most actions have a specific return type.</span></span> <span data-ttu-id="0180f-155">アクションの実行中に予期しない状態が発生する場合、特定の型は返されません。</span><span class="sxs-lookup"><span data-stu-id="0180f-155">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="0180f-156">たとえば、アクションの入力パラメーターがモデルの検証に失敗する場合があります。</span><span class="sxs-lookup"><span data-stu-id="0180f-156">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="0180f-157">このような場合、通常は特定の型ではなく、適切な `ActionResult` 型が返されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-157">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="0180f-158">同期アクション</span><span class="sxs-lookup"><span data-stu-id="0180f-158">Synchronous action</span></span>

<span data-ttu-id="0180f-159">2 つの戻り値の型が考えられる、同期アクションを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="0180f-159">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="0180f-160">上記のコードでは、データベースに製品が存在しない場合に 404 状態コードが返されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-160">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="0180f-161">製品が存在する場合は、対応する `Product` オブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-161">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="0180f-162">ASP.NET Core 2.1 より前のバージョンでは、`return product;` 行は `return Ok(product);` になっていました。</span><span class="sxs-lookup"><span data-stu-id="0180f-162">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="0180f-163">ASP.NET Core 2.1 以降では、コントローラー クラスが `[ApiController]` 属性で修飾されていると、アクション パラメーター バインディング ソースの推論が有効になります。</span><span class="sxs-lookup"><span data-stu-id="0180f-163">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="0180f-164">ルート テンプレート内の名前と一致するパラメーター名は、要求のルート データを使用して自動的にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="0180f-164">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="0180f-165">したがって、前のアクションの `id` パラメーターは、[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) 属性を使用して明示的に注釈付けされません。</span><span class="sxs-lookup"><span data-stu-id="0180f-165">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="0180f-166">非同期アクション</span><span class="sxs-lookup"><span data-stu-id="0180f-166">Asynchronous action</span></span>

<span data-ttu-id="0180f-167">2 つの戻り値の型が考えられる、非同期アクションを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="0180f-167">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="0180f-168">モデルの検証に失敗すると、[BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) メソッドが呼び出され、400 状態コードが返されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-168">If model validation fails, the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) method is invoked to return a 400 status code.</span></span> <span data-ttu-id="0180f-169">特定の検証エラーを含む [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) プロパティが渡されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-169">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property containing the specific validation errors is passed to it.</span></span> <span data-ttu-id="0180f-170">モデルの検証に成功すると、データベースに製品が作成されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-170">If model validation succeeds, the product is created in the database.</span></span> <span data-ttu-id="0180f-171">201 状態コードが返されます。</span><span class="sxs-lookup"><span data-stu-id="0180f-171">A 201 status code is returned.</span></span>

> [!TIP]
> <span data-ttu-id="0180f-172">ASP.NET Core 2.1 以降では、コントローラー クラスが `[ApiController]` 属性で修飾されていると、アクション パラメーター バインディング ソースの推論が有効になります。</span><span class="sxs-lookup"><span data-stu-id="0180f-172">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="0180f-173">複合型パラメーターは、要求本文を使用して自動的にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="0180f-173">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="0180f-174">したがって、前のアクションの `product` パラメーターは、[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) 属性を使用して明示的に注釈付けされません。</span><span class="sxs-lookup"><span data-stu-id="0180f-174">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="0180f-175">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="0180f-175">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
