---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: "Web API 2 の操作の結果 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 68b82661b97434795e1c306b168033dfcde529bc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="action-results-in-web-api-2"></a><span data-ttu-id="f3cdf-102">Web API 2 のアクションの結果</span><span class="sxs-lookup"><span data-stu-id="f3cdf-102">Action Results in Web API 2</span></span>
====================
<span data-ttu-id="f3cdf-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f3cdf-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f3cdf-104">このトピックでは、ASP.NET Web API に HTTP 応答メッセージにコント ローラーのアクションからの戻り値を変換する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="f3cdf-105">Web API コント ローラーのアクションには、次のいずれかを返すことができます。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="f3cdf-106">void</span><span class="sxs-lookup"><span data-stu-id="f3cdf-106">void</span></span>
2. <span data-ttu-id="f3cdf-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="f3cdf-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="f3cdf-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="f3cdf-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="f3cdf-109">その他の種類</span><span class="sxs-lookup"><span data-stu-id="f3cdf-109">Some other type</span></span>

<span data-ttu-id="f3cdf-110">に応じて次のうちが返される、Web API は、別のメカニズムを使用して、HTTP 応答を作成します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="f3cdf-111">戻り値の型</span><span class="sxs-lookup"><span data-stu-id="f3cdf-111">Return type</span></span> | <span data-ttu-id="f3cdf-112">Web API が、応答を作成する方法</span><span class="sxs-lookup"><span data-stu-id="f3cdf-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="f3cdf-113">void</span><span class="sxs-lookup"><span data-stu-id="f3cdf-113">void</span></span> | <span data-ttu-id="f3cdf-114">空の 204 (No Content) を返す</span><span class="sxs-lookup"><span data-stu-id="f3cdf-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="f3cdf-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="f3cdf-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="f3cdf-116">HTTP 応答メッセージに直接変換です。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="f3cdf-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="f3cdf-117">**IHttpActionResult**</span></span> | <span data-ttu-id="f3cdf-118">呼び出す**ExecuteAsync**を作成する、 **HttpResponseMessage**、HTTP 応答メッセージに変換します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="f3cdf-119">その他の種類</span><span class="sxs-lookup"><span data-stu-id="f3cdf-119">Other type</span></span> | <span data-ttu-id="f3cdf-120">応答の本体に書き込むためにシリアル化された戻り値200 (OK) を返します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="f3cdf-121">このトピックの残りの部分では、各オプションの詳細について説明します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="f3cdf-122">void</span><span class="sxs-lookup"><span data-stu-id="f3cdf-122">void</span></span>

<span data-ttu-id="f3cdf-123">戻り値の型が場合`void`、Web API では、空の HTTP 応答状態コード 204 (No Content) をだけを返します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="f3cdf-124">例のコント ローラー:</span><span class="sxs-lookup"><span data-stu-id="f3cdf-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="f3cdf-125">HTTP 応答:</span><span class="sxs-lookup"><span data-stu-id="f3cdf-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="f3cdf-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="f3cdf-126">HttpResponseMessage</span></span>

<span data-ttu-id="f3cdf-127">アクションを返す場合、 [HttpResponseMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.aspx)、Web API からのプロパティを使用して、HTTP 応答メッセージに直接、戻り値が変換、 **HttpResponseMessage**を設定するオブジェクト、応答です。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="f3cdf-128">このオプションでは、多数の応答メッセージを制御できます。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="f3cdf-129">たとえば、次のコント ローラーのアクションは、Cache-control ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="f3cdf-130">応答 :</span><span class="sxs-lookup"><span data-stu-id="f3cdf-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="f3cdf-131">ドメイン モデルを渡す場合、 **CreateResponse**メソッドは、Web API では、[メディア フォーマッタ](../formats-and-model-binding/media-formatters.md)応答本文にシリアル化されたモデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="f3cdf-132">Web API では、要求の Accept ヘッダーを使用して、フォーマッタを選択します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="f3cdf-133">詳細については、次を参照してください。[コンテンツ ネゴシエーション](../formats-and-model-binding/content-negotiation.md)です。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="f3cdf-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="f3cdf-134">IHttpActionResult</span></span>

<span data-ttu-id="f3cdf-135">**IHttpActionResult**インターフェイスが Web API 2 で導入されました。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="f3cdf-136">基本的には、定義、 **HttpResponseMessage**ファクトリ。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="f3cdf-137">ここでは、いくつかを使用する利点、 **IHttpActionResult**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="f3cdf-138">簡略化[単体テスト](../testing-and-debugging/unit-testing-controllers-in-web-api.md)コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="f3cdf-139">別のクラスに HTTP 応答を作成するための一般的なロジックを移動します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="f3cdf-140">応答を構築する低水準の詳細を非表示にしてわかりやすく、コント ローラー アクションの目的は、します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="f3cdf-141">**IHttpActionResult** 1 つのメソッドを含む**ExecuteAsync**、非同期に作成される、 **HttpResponseMessage**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="f3cdf-142">コント ローラーのアクションを返す場合、 **IHttpActionResult**、Web API を呼び出す、 **ExecuteAsync**メソッドを作成、 **HttpResponseMessage**です。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="f3cdf-143">変換してから、 **HttpResponseMessage**を HTTP 応答メッセージにします。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="f3cdf-144">単純な実装を次に示します**IHttpActionResult**プレーン テキストの応答を作成します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-144">Here is a simple implementaton of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="f3cdf-145">コント ローラー アクションの例:</span><span class="sxs-lookup"><span data-stu-id="f3cdf-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="f3cdf-146">応答 :</span><span class="sxs-lookup"><span data-stu-id="f3cdf-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="f3cdf-147">多くの場合、使用する、 **IHttpActionResult**実装で定義されている、  **[System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx)** 名前空間。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-147">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="f3cdf-148">**ApiController**クラスは、これらの組み込みのアクション結果を返すヘルパー メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="f3cdf-149">次の例では、要求が既存の製品 ID と一致しない場合、コント ローラーは呼び出し[ApiController.NotFound](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.notfound.aspx) 404 (Not Found) 応答を作成します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="f3cdf-150">それ以外の場合、コント ローラーを呼び出す[ApiController.OK](https://msdn.microsoft.com/en-us/library/dn314591.aspx)、200 (OK) 応答を作成する、製品が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/en-us/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="f3cdf-151">その他の戻り値の型</span><span class="sxs-lookup"><span data-stu-id="f3cdf-151">Other Return Types</span></span>

<span data-ttu-id="f3cdf-152">その他のすべての戻り値型の Web API を使用して、[メディア フォーマッタ](../formats-and-model-binding/media-formatters.md)戻り値をシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="f3cdf-153">Web API は、応答本文にシリアル化された値を書き込みます。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="f3cdf-154">応答状態コードは、200 (OK) です。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="f3cdf-155">この方法の欠点は、404 など、エラー コードを直接返すことはできません。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="f3cdf-156">ただし、スロー、 **HttpResponseException**エラー コード。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="f3cdf-157">詳細については、次を参照してください。 [ASP.NET Web API での例外処理](../error-handling/exception-handling.md)です。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="f3cdf-158">Web API では、要求の Accept ヘッダーを使用して、フォーマッタを選択します。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="f3cdf-159">詳細については、次を参照してください。[コンテンツ ネゴシエーション](../formats-and-model-binding/content-negotiation.md)です。</span><span class="sxs-lookup"><span data-stu-id="f3cdf-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="f3cdf-160">要求の例</span><span class="sxs-lookup"><span data-stu-id="f3cdf-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="f3cdf-161">応答の例:</span><span class="sxs-lookup"><span data-stu-id="f3cdf-161">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
