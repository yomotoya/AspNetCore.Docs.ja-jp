---
title: ASP.NET Core Web API のコントローラー アクションの戻り値の型
author: scottaddie
description: ASP.NET Core Web API でのさまざまなコントローラー アクション メソッドの戻り値の型の使用について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 03/21/2018
uid: web-api/action-return-types
ms.openlocfilehash: 422db97da222fb5e742e1d8e6ae410edc90dbc18
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273557"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>ASP.NET Core Web API のコントローラー アクションの戻り値の型

作成者: [Scott Addie](https://github.com/scottaddie)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

ASP.NET Core では、Web API コントローラー アクションの戻り値の型に次のオプションを提供しています。

::: moniker range="<= aspnetcore-2.0"
* [特定の型](#specific-type)
* [IActionResult](#iactionresult-type)
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
* [特定の型](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T>](#actionresultt-type)
::: moniker-end

このドキュメントでは、各戻り値の型を使用するのが最適な場合について説明します。

## <a name="specific-type"></a>特定の型

最もシンプルなアクションでは、プリミティブ データ型または複合データ型が返されます (`string` やカスタム オブジェクトの型など)。 カスタム `Product` オブジェクトのコレクションを返す次のアクションを考えてみましょう。

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

アクションの実行中に保護できる既知の条件がない場合は、特定の型を返すだけで十分です。 前のアクションでパラメーターを受け取っていないので、パラメーターの制約の検証は必要ありません。

アクションで既知の条件を考慮する必要がある場合は、複数の戻り値のパスが導入されます。 このような場合、[ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) の戻り値の型とプリミティブまたは複合の戻り値の型を混在させるのが一般的です。 この種類のアクションに対応するには、[IActionResult](#iactionresult-type) または [ActionResult\<T>](#actionresultt-type) のいずれかが必要です。

## <a name="iactionresult-type"></a>IActionResult 型

[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) の戻り値の型は、アクションで複数の [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) の戻り値の型が可能な場合に適しています。 `ActionResult` 型は、さまざまな HTTP 状態コードを表します。 このカテゴリに当てはまる一般的な戻り値の型には、[BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400)、[NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404)、[OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200) などがあります。

アクションには複数の戻り値の型とパスがあるため、[[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) 属性を十分に使える必要があります。 この属性は、[Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger) などのツールで生成される API ヘルプ ページのよりわかりやすい応答の詳細を生成します。 `[ProducesResponseType]` は、アクションによって返される既知の型と HTTP 状態コードを示します。

### <a name="synchronous-action"></a>同期アクション

2 つの戻り値の型が考えられる、次の同期アクションを考えてみましょう。

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

前のアクションでは、`id` によって表される製品が、基になるデータ ストアに存在しないと、404 状態コードが返されます。 `return new NotFoundResult();` へのショートカットとして、[NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) ヘルパー メソッドが呼び出されます。 製品が存在する場合は、ペイロードを表す `Product` オブジェクトが、200 状態コードと共に返されます。 [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) ヘルパー メソッドは、`return new OkObjectResult(product);` の短縮形式として呼び出されます。

### <a name="asynchronous-action"></a>非同期アクション

2 つの戻り値の型が考えられる、次の非同期アクションを考えてみましょう。

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

前のアクションでは、モデルの検証に失敗し、[BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) ヘルパー メソッドが呼び出されるときに、400 状態コードが返されます。 たとえば、次のモデルでは、要求で `Name` プロパティと値を提供する必要があることを示しています。 そのため、要求で適切な `Name` を提供するのに失敗すると、モデルの検証が失敗します。

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

前のアクションのその他の既知のリターン コードは、[CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) ヘルパー メソッドで生成される 201 です。 このパスでは、`Product` オブジェクトが返されます。

::: moniker range=">= aspnetcore-2.1"
## <a name="actionresultt-type"></a>ActionResult\<T> 型

ASP.NET Core 2.1 では、Web API コントローラー アクションに対して、[ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) の戻り値の型が導入されました。 これにより、[ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) から派生する型、または[特定の型](#specific-type)を返すことができます。 `ActionResult<T>` により、[IActionResult 型](#iactionresult-type)は次の利点を得られます。

* [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) 属性の `Type` プロパティを除外することができます。
* [暗黙的なキャスト演算子](/dotnet/csharp/language-reference/keywords/implicit)は、`T` と `ActionResult` の両方の `ActionResult<T>` への変換をサポートしています。 `T` は [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult) に変換されます。つまり、`return new ObjectResult(T);` は `return T;` に簡略化されます。

ほとんどのアクションには、特定の戻り値の型があります。 アクションの実行中に予期しない状態が発生する場合、特定の型は返されません。 たとえば、アクションの入力パラメーターがモデルの検証に失敗する場合があります。 このような場合、通常は特定の型ではなく、適切な `ActionResult` 型が返されます。

### <a name="synchronous-action"></a>同期アクション

2 つの戻り値の型が考えられる、同期アクションを考えてみましょう。

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

上記のコードでは、データベースに製品が存在しない場合に 404 状態コードが返されます。 製品が存在する場合は、対応する `Product` オブジェクトが返されます。 ASP.NET Core 2.1 より前のバージョンでは、`return product;` 行は `return Ok(product);` になっていました。

> [!TIP]
> ASP.NET Core 2.1 以降では、コントローラー クラスが `[ApiController]` 属性で修飾されていると、アクション パラメーター バインディング ソースの推論が有効になります。 ルート テンプレート内の名前と一致するパラメーター名は、要求のルート データを使用して自動的にバインドされます。 したがって、前のアクションの `id` パラメーターは、[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) 属性を使用して明示的に注釈付けされません。

### <a name="asynchronous-action"></a>非同期アクション

2 つの戻り値の型が考えられる、非同期アクションを考えてみましょう。

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

モデルの検証に失敗すると、[BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) メソッドが呼び出され、400 状態コードが返されます。 特定の検証エラーを含む [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) プロパティが渡されます。 モデルの検証に成功すると、データベースに製品が作成されます。 201 状態コードが返されます。

> [!TIP]
> ASP.NET Core 2.1 以降では、コントローラー クラスが `[ApiController]` 属性で修飾されていると、アクション パラメーター バインディング ソースの推論が有効になります。 複合型パラメーターは、要求本文を使用して自動的にバインドされます。 したがって、前のアクションの `product` パラメーターは、[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) 属性を使用して明示的に注釈付けされません。
::: moniker-end

## <a name="additional-resources"></a>その他の技術情報

* [コントローラー アクション](xref:mvc/controllers/actions)
* [モデル検証](xref:mvc/models/validation)
* [Swagger を使用する Web API のヘルプ ページ](xref:tutorials/web-api-help-pages-using-swagger)
