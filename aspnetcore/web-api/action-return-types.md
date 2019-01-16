---
title: ASP.NET Core Web API のコントローラー アクションの戻り値の型
author: scottaddie
description: ASP.NET Core Web API でのさまざまなコントローラー アクション メソッドの戻り値の型の使用について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/04/2019
uid: web-api/action-return-types
ms.openlocfilehash: 98d70e0379d353cff98a6d7a13f2dd00eb4da206
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098741"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="5080c-103">ASP.NET Core Web API のコントローラー アクションの戻り値の型</span><span class="sxs-lookup"><span data-stu-id="5080c-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="5080c-104">作成者: [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="5080c-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="5080c-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="5080c-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5080c-106">ASP.NET Core では、Web API コントローラー アクションの戻り値の型に次のオプションを提供しています。</span><span class="sxs-lookup"><span data-stu-id="5080c-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="5080c-107">特定の型</span><span class="sxs-lookup"><span data-stu-id="5080c-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="5080c-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="5080c-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="5080c-109">ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="5080c-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="5080c-110">特定の型</span><span class="sxs-lookup"><span data-stu-id="5080c-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="5080c-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="5080c-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="5080c-112">このドキュメントでは、各戻り値の型を使用するのが最適な場合について説明します。</span><span class="sxs-lookup"><span data-stu-id="5080c-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="5080c-113">特定の型</span><span class="sxs-lookup"><span data-stu-id="5080c-113">Specific type</span></span>

<span data-ttu-id="5080c-114">最もシンプルなアクションでは、プリミティブ データ型または複合データ型が返されます (`string` やカスタム オブジェクトの型など)。</span><span class="sxs-lookup"><span data-stu-id="5080c-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="5080c-115">カスタム `Product` オブジェクトのコレクションを返す次のアクションを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="5080c-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="5080c-116">アクションの実行中に保護できる既知の条件がない場合は、特定の型を返すだけで十分です。</span><span class="sxs-lookup"><span data-stu-id="5080c-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="5080c-117">前のアクションでパラメーターを受け取っていないので、パラメーターの制約の検証は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="5080c-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="5080c-118">アクションで既知の条件を考慮する必要がある場合は、複数の戻り値のパスが導入されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="5080c-119">このような場合、[ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) の戻り値の型とプリミティブまたは複合の戻り値の型を混在させるのが一般的です。</span><span class="sxs-lookup"><span data-stu-id="5080c-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="5080c-120">この種類のアクションに対応するには、[IActionResult](#iactionresult-type) または [ActionResult\<T>](#actionresultt-type) のいずれかが必要です。</span><span class="sxs-lookup"><span data-stu-id="5080c-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="5080c-121">IActionResult 型</span><span class="sxs-lookup"><span data-stu-id="5080c-121">IActionResult type</span></span>

<span data-ttu-id="5080c-122">[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) の戻り値の型は、アクションで複数の [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) の戻り値の型が可能な場合に適しています。</span><span class="sxs-lookup"><span data-stu-id="5080c-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="5080c-123">`ActionResult` 型は、さまざまな HTTP 状態コードを表します。</span><span class="sxs-lookup"><span data-stu-id="5080c-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="5080c-124">このカテゴリに当てはまる一般的な戻り値の型には、[BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400)、[NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404)、[OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200) などがあります。</span><span class="sxs-lookup"><span data-stu-id="5080c-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="5080c-125">アクションには複数の戻り値の型とパスがあるため、[[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) 属性を十分に使える必要があります。</span><span class="sxs-lookup"><span data-stu-id="5080c-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="5080c-126">この属性は、[Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger) などのツールで生成される API ヘルプ ページのよりわかりやすい応答の詳細を生成します。</span><span class="sxs-lookup"><span data-stu-id="5080c-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="5080c-127">`[ProducesResponseType]` は、アクションによって返される既知の型と HTTP 状態コードを示します。</span><span class="sxs-lookup"><span data-stu-id="5080c-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="5080c-128">同期アクション</span><span class="sxs-lookup"><span data-stu-id="5080c-128">Synchronous action</span></span>

<span data-ttu-id="5080c-129">2 つの戻り値の型が考えられる、次の同期アクションを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="5080c-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="5080c-130">前のアクションでは、`id` によって表される製品が、基になるデータ ストアに存在しないと、404 状態コードが返されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="5080c-131">`return new NotFoundResult();` へのショートカットとして、[NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) ヘルパー メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="5080c-132">製品が存在する場合は、ペイロードを表す `Product` オブジェクトが、200 状態コードと共に返されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="5080c-133">[Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) ヘルパー メソッドは、`return new OkObjectResult(product);` の短縮形式として呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="5080c-134">非同期アクション</span><span class="sxs-lookup"><span data-stu-id="5080c-134">Asynchronous action</span></span>

<span data-ttu-id="5080c-135">2 つの戻り値の型が考えられる、次の非同期アクションを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="5080c-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="5080c-136">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="5080c-136">In the preceding code:</span></span>

* <span data-ttu-id="5080c-137">製品の説明に "XYZ Widget" が含まれている場合、ASP.NET Core ランタイムによって 400 状態コード ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) が返されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-137">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when the product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="5080c-138">製品が作成されると、[CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) メソッドによって 201 状態コードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-138">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="5080c-139">このコード パスでは、`Product` オブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-139">In this code path, the `Product` object is returned.</span></span>

<span data-ttu-id="5080c-140">たとえば、次のモデルでは、要求に `Name` プロパティと `Description` プロパティを含める必要があることを示しています。</span><span class="sxs-lookup"><span data-stu-id="5080c-140">For example, the following model indicates that requests must include the `Name` and `Description` properties.</span></span> <span data-ttu-id="5080c-141">そのため、要求で `Name` と `Description` を提供するのに失敗すると、モデルの検証が失敗します。</span><span class="sxs-lookup"><span data-stu-id="5080c-141">Therefore, failure to provide `Name` and `Description` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5080c-142">ASP.NET Core 2.1 以降の [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 属性が適用されている場合、モデルの検証エラーによって 400 状態コードが返されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-142">If the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute in ASP.NET Core 2.1 or later  is applied, model validation errors result in a 400 status code.</span></span> <span data-ttu-id="5080c-143">詳細については、「[自動的な HTTP 400 応答](xref:web-api/index#automatic-http-400-responses)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5080c-143">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="actionresultt-type"></a><span data-ttu-id="5080c-144">ActionResult\<T> 型</span><span class="sxs-lookup"><span data-stu-id="5080c-144">ActionResult\<T> type</span></span>

<span data-ttu-id="5080c-145">ASP.NET Core 2.1 では、Web API コントローラー アクションに対して、[ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) の戻り値の型が導入されました。</span><span class="sxs-lookup"><span data-stu-id="5080c-145">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="5080c-146">これにより、[ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) から派生する型、または[特定の型](#specific-type)を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="5080c-146">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="5080c-147">`ActionResult<T>` により、[IActionResult 型](#iactionresult-type)は次の利点を得られます。</span><span class="sxs-lookup"><span data-stu-id="5080c-147">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="5080c-148">[[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) 属性の `Type` プロパティを除外することができます。</span><span class="sxs-lookup"><span data-stu-id="5080c-148">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="5080c-149">たとえば、`[ProducesResponseType(200, Type = typeof(Product))]` は `[ProducesResponseType(200)]` に簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-149">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="5080c-150">アクションの予期される戻り値の型は、代わりに `ActionResult<T>` の `T` から推論されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-150">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="5080c-151">[暗黙的なキャスト演算子](/dotnet/csharp/language-reference/keywords/implicit)は、`T` と `ActionResult` の両方の `ActionResult<T>` への変換をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="5080c-151">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="5080c-152">`T` は [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult) に変換されます。つまり、`return new ObjectResult(T);` は `return T;` に簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-152">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="5080c-153">C# はインターフェイス上での暗黙的なキャスト演算子をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="5080c-153">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="5080c-154">そのため、`ActionResult<T>` を使用するには、インターフェイスを具象型に変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5080c-154">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="5080c-155">たとえば、次の例における `IEnumerable` の使用は機能しません。</span><span class="sxs-lookup"><span data-stu-id="5080c-155">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

<span data-ttu-id="5080c-156">上記のコードを修正するための選択肢の 1 つは、`_repository.GetProducts().ToList();` を返すことです。</span><span class="sxs-lookup"><span data-stu-id="5080c-156">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="5080c-157">ほとんどのアクションには、特定の戻り値の型があります。</span><span class="sxs-lookup"><span data-stu-id="5080c-157">Most actions have a specific return type.</span></span> <span data-ttu-id="5080c-158">アクションの実行中に予期しない状態が発生する場合、特定の型は返されません。</span><span class="sxs-lookup"><span data-stu-id="5080c-158">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="5080c-159">たとえば、アクションの入力パラメーターがモデルの検証に失敗する場合があります。</span><span class="sxs-lookup"><span data-stu-id="5080c-159">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="5080c-160">このような場合、通常は特定の型ではなく、適切な `ActionResult` 型が返されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-160">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="5080c-161">同期アクション</span><span class="sxs-lookup"><span data-stu-id="5080c-161">Synchronous action</span></span>

<span data-ttu-id="5080c-162">2 つの戻り値の型が考えられる、同期アクションを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="5080c-162">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="5080c-163">上記のコードでは、データベースに製品が存在しない場合に 404 状態コードが返されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-163">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="5080c-164">製品が存在する場合は、対応する `Product` オブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-164">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="5080c-165">ASP.NET Core 2.1 より前のバージョンでは、`return product;` 行は `return Ok(product);` になっていました。</span><span class="sxs-lookup"><span data-stu-id="5080c-165">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="5080c-166">ASP.NET Core 2.1 以降では、コントローラー クラスが `[ApiController]` 属性で修飾されていると、アクション パラメーター バインディング ソースの推論が有効になります。</span><span class="sxs-lookup"><span data-stu-id="5080c-166">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="5080c-167">ルート テンプレート内の名前と一致するパラメーター名は、要求のルート データを使用して自動的にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="5080c-167">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="5080c-168">したがって、前のアクションの `id` パラメーターは、[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) 属性を使用して明示的に注釈付けされません。</span><span class="sxs-lookup"><span data-stu-id="5080c-168">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="5080c-169">非同期アクション</span><span class="sxs-lookup"><span data-stu-id="5080c-169">Asynchronous action</span></span>

<span data-ttu-id="5080c-170">2 つの戻り値の型が考えられる、非同期アクションを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="5080c-170">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="5080c-171">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="5080c-171">In the preceding code:</span></span>

* <span data-ttu-id="5080c-172">次の場合に ASP.NET Core ランタイムによって 400 状態コード ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) 返されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-172">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when:</span></span>
  * <span data-ttu-id="5080c-173">[[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 属性が適用されていて、モデルの検証が失敗する。</span><span class="sxs-lookup"><span data-stu-id="5080c-173">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute has been applied and model validation fails.</span></span>
  * <span data-ttu-id="5080c-174">製品の説明に "XYZ Widget" が含まれている。</span><span class="sxs-lookup"><span data-stu-id="5080c-174">The product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="5080c-175">製品が作成されると、[CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) メソッドによって 201 状態コードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-175">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="5080c-176">このコード パスでは、`Product` オブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="5080c-176">In this code path, the `Product` object is returned.</span></span>

> [!TIP]
> <span data-ttu-id="5080c-177">ASP.NET Core 2.1 以降では、コントローラー クラスが `[ApiController]` 属性で修飾されていると、アクション パラメーター バインディング ソースの推論が有効になります。</span><span class="sxs-lookup"><span data-stu-id="5080c-177">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="5080c-178">複合型パラメーターは、要求本文を使用して自動的にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="5080c-178">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="5080c-179">したがって、前のアクションの `product` パラメーターは、[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) 属性を使用して明示的に注釈付けされません。</span><span class="sxs-lookup"><span data-stu-id="5080c-179">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="5080c-180">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="5080c-180">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
