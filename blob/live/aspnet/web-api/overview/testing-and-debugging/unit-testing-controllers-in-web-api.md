---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: "単体テストの ASP.NET web API 2 コント ローラー |Microsoft ドキュメント"
author: MikeWasson
description: "このトピックでは、単体テストの Web API 2 コント ローラーの特定の方法について説明します。 このトピックを読む前に単位のチュートリアルを読みたい場合があります."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 167cd24d27977c3652f6a8903054654f5edf7756
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="8e1a7-104">単体テストの ASP.NET web API 2 コント ローラー</span><span class="sxs-lookup"><span data-stu-id="8e1a7-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="8e1a7-105">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8e1a7-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="8e1a7-106">このトピックでは、単体テストの Web API 2 コント ローラーの特定の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="8e1a7-107">このトピックを読む前にすることができます、チュートリアルを読み取る[ASP.NET Web API 2 の単体テスト](unit-testing-with-aspnet-web-api.md)、単体テスト プロジェクトをソリューションに追加する方法が示されます。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8e1a7-108">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="8e1a7-108">Software versions used in the tutorial</span></span>
> 
> - [<span data-ttu-id="8e1a7-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8e1a7-109">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="8e1a7-110">Web API 2</span><span class="sxs-lookup"><span data-stu-id="8e1a7-110">Web API 2</span></span>
> - <span data-ttu-id="8e1a7-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="8e1a7-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="8e1a7-112">Moq を使用しましたが、考え方は同じが任意のモック フレームワークに適用されます。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="8e1a7-113">Moq 4.5.30 (以降) Visual Studio 2017、Roslyn および .NET 4.5 以降のバージョンをサポートします。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="8e1a7-114">単体テストでの一般的なパターンは&quot;整列 act アサート&quot;:</span><span class="sxs-lookup"><span data-stu-id="8e1a7-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="8e1a7-115">配置: を実行するテストのすべての前提条件を設定します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="8e1a7-116">Act: は、テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-116">Act: Perform the test.</span></span>
- <span data-ttu-id="8e1a7-117">: をアサートすると、テストが成功したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="8e1a7-118">配置手順を多くの場合、モックを使用してまたはスタブに対応するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="8e1a7-119">依存関係の数は、テストが 1 つのテストの重点を置いてようにを最小化します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="8e1a7-120">次に、Web API コント ローラーで単体テストを必要があるいくつかの点を示します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="8e1a7-121">アクションでは、適切な種類の応答を返します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="8e1a7-122">無効なパラメーターでは、適切なエラー応答を返します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="8e1a7-123">アクションは、リポジトリまたはサービス層で正しいメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="8e1a7-124">応答には、ドメイン モデルが含まれている場合は、モデルの種類を確認します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="8e1a7-125">いくつの一般的な機能をテストするには、これらがコント ローラーの実装によって異なります。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="8e1a7-126">具体的には、これにより、大きな違いコント ローラーのアクションを返すかどうか**HttpResponseMessage**または**IHttpActionResult**です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="8e1a7-127">結果型の詳細については、次を参照してください。 [Web Api 2 のアクション結果](../getting-started-with-aspnet-web-api/action-results.md)です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="8e1a7-128">HttpResponseMessage を返しますのテストの操作</span><span class="sxs-lookup"><span data-stu-id="8e1a7-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="8e1a7-129">次の例は、コント ローラーのアクションの戻り値**HttpResponseMessage**です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="8e1a7-130">通知は、コント ローラーでは、依存関係の挿入を使用して、挿入、`IProductRepository`です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="8e1a7-131">検証を実現コント ローラーより、モック リポジトリを挿入するためです。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="8e1a7-132">次の単体テストであることを確認、`Get`メソッドの書き込み、`Product`応答の本体。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="8e1a7-133">あると想定`repository`はモック`IProductRepository`です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="8e1a7-134">設定することが重要**要求**と**構成**コント ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="8e1a7-135">テストは失敗するそれ以外の場合、 **ArgumentNullException**または**InvalidOperationException**です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="8e1a7-136">リンクのテストの生成</span><span class="sxs-lookup"><span data-stu-id="8e1a7-136">Testing Link Generation</span></span>

<span data-ttu-id="8e1a7-137">`Post`メソッド呼び出し**UrlHelper.Link**応答でリンクを作成します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="8e1a7-138">これには、単体テストでは少し複雑なセットアップが必要です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="8e1a7-139">**UrlHelper**クラスは、これらの値を設定するため、テスト要求の URL とルート データを必要があります。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="8e1a7-140">別のオプションは、モックまたはスタブ**UrlHelper**です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="8e1a7-141">この方法での既定値を置換する[ApiController.Url](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.url.aspx)モックまたはスタブ バージョン固定値を返しますを使用します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="8e1a7-142">使用してテストを記述し直しましょう、 [Moq](https://github.com/Moq)フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="8e1a7-143">インストール、`Moq`テスト プロジェクトでの NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="8e1a7-144">このバージョンで必要はありません、ルート データを設定するため、モック**UrlHelper**定数文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="8e1a7-145">IHttpActionResult を返すためのテストの操作</span><span class="sxs-lookup"><span data-stu-id="8e1a7-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="8e1a7-146">Web API 2 コント ローラーのアクションを返すことができます**IHttpActionResult**、これに似ています**ActionResult** ASP.NET MVC でします。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="8e1a7-147">**IHttpActionResult**インターフェイスは、HTTP 応答を作成するコマンドのパターンを定義します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="8e1a7-148">応答を直接作成するには、代わりに、コント ローラーを返します、 **IHttpActionResult**です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="8e1a7-149">後で、パイプラインを呼び出す、 **IHttpActionResult**応答を作成します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="8e1a7-150">このアプローチやすく単体テストを記述するために必要なセットアップの多くを省略できます**HttpResponseMessage**です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="8e1a7-151">例のコント ローラーのアクションの戻り値を次に示します**IHttpActionResult**です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="8e1a7-152">この例を使用して、一般的なパターンを示しています**IHttpActionResult**です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="8e1a7-153">見てどの単位にそれらをテストします。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="8e1a7-154">アクションは、応答本体を持つ 200 (OK) を返します</span><span class="sxs-lookup"><span data-stu-id="8e1a7-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="8e1a7-155">`Get`メソッド呼び出し`Ok(product)`場合は、製品が見つかりました。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="8e1a7-156">単体テストでは、戻り値の型を確認してください**OkNegotiatedContentResult**し、返された製品が、右側の id です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="8e1a7-157">単体テストがアクションの結果を実行しないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="8e1a7-158">アクションの結果は、HTTP 応答を正しく作成を想定することができます。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="8e1a7-159">(理由は、Web API フレームワークは、独自の単体テストを含む!)</span><span class="sxs-lookup"><span data-stu-id="8e1a7-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="8e1a7-160">アクションには、404 (Not Found) が返されます</span><span class="sxs-lookup"><span data-stu-id="8e1a7-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="8e1a7-161">`Get`メソッド呼び出し`NotFound()`製品が見つからない場合。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="8e1a7-162">この場合、単体テストの単なるチェック戻り値の型がある場合**NotFoundResult**です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="8e1a7-163">応答本文のない 200 (OK) を返すアクション</span><span class="sxs-lookup"><span data-stu-id="8e1a7-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="8e1a7-164">`Delete`メソッド呼び出し`Ok()`を空の HTTP 200 の応答を返します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="8e1a7-165">前の例のように、単体テストをチェック戻り値の型では、ここでは**OkResult**です。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="8e1a7-166">場所ヘッダーを持つ 201 (Created) を返すアクション</span><span class="sxs-lookup"><span data-stu-id="8e1a7-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="8e1a7-167">`Post`メソッド呼び出し`CreatedAtRoute`Location ヘッダーの URI を持つ、HTTP 201 の応答を返すにします。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="8e1a7-168">単体テストでは、アクションが正しいルーティング値を設定することを確認します。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="8e1a7-169">アクションは、応答本体を持つ別の 2 xx を返します</span><span class="sxs-lookup"><span data-stu-id="8e1a7-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="8e1a7-170">`Put`メソッド呼び出し`Content`応答本文と HTTP 202 (Accepted) 応答が返さにします。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="8e1a7-171">この場合は 200 (OK) を返すことに似ていますが、単体テストは、ステータス コードを確認する必要がありますもします。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="8e1a7-172">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="8e1a7-172">Additional Resources</span></span>

- [<span data-ttu-id="8e1a7-173">Entity Framework をモック作成時に単体テストの ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="8e1a7-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="8e1a7-174">[ASP.NET Web API サービスのテストを記述](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx)(Youssef Moussaoui でブログの投稿)。</span><span class="sxs-lookup"><span data-stu-id="8e1a7-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="8e1a7-175">ASP.NET Web API のルートのデバッガーのデバッグ</span><span class="sxs-lookup"><span data-stu-id="8e1a7-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
