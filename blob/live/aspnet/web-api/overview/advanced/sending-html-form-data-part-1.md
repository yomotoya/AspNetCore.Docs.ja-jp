---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: "ASP.NET Web API で HTML フォームのデータを送信する: フォーム エンコード データ |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="acde2-102">ASP.NET Web API で HTML フォームのデータを送信する: フォーム エンコード データ</span><span class="sxs-lookup"><span data-stu-id="acde2-102">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>
====================
<span data-ttu-id="acde2-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="acde2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="acde2-104">パート 1: フォーム エンコード データ</span><span class="sxs-lookup"><span data-stu-id="acde2-104">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="acde2-105">この記事では、Web API コント ローラーへのフォーム エンコード データを投稿する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="acde2-105">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="acde2-106">HTML フォームの概要</span><span class="sxs-lookup"><span data-stu-id="acde2-106">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="acde2-107">複合型を送信します。</span><span class="sxs-lookup"><span data-stu-id="acde2-107">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="acde2-108">AJAX を使用してフォームのデータを送信します。</span><span class="sxs-lookup"><span data-stu-id="acde2-108">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="acde2-109">単純型を送信します。</span><span class="sxs-lookup"><span data-stu-id="acde2-109">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="acde2-110">[完成したプロジェクトをダウンロード](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007)です。</span><span class="sxs-lookup"><span data-stu-id="acde2-110">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="acde2-111">HTML フォームの概要</span><span class="sxs-lookup"><span data-stu-id="acde2-111">Overview of HTML Forms</span></span>

<span data-ttu-id="acde2-112">HTML フォームの使用は、GET か、または、サーバーにデータを送信する POST。</span><span class="sxs-lookup"><span data-stu-id="acde2-112">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="acde2-113">**メソッド**の属性、**フォーム**要素は、HTTP メソッド。</span><span class="sxs-lookup"><span data-stu-id="acde2-113">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="acde2-114">既定の方法は GET です。</span><span class="sxs-lookup"><span data-stu-id="acde2-114">The default method is GET.</span></span> <span data-ttu-id="acde2-115">フォームで使用する場合の取得、データがクエリ文字列としての URI でエンコードされたフォームです。</span><span class="sxs-lookup"><span data-stu-id="acde2-115">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="acde2-116">フォームは POST を使用している場合、フォーム データが要求本文に配置されます。</span><span class="sxs-lookup"><span data-stu-id="acde2-116">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="acde2-117">データの送信を**enctype**属性が要求本文の形式を指定します。</span><span class="sxs-lookup"><span data-stu-id="acde2-117">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="acde2-118">enctype</span><span class="sxs-lookup"><span data-stu-id="acde2-118">enctype</span></span> | <span data-ttu-id="acde2-119">説明</span><span class="sxs-lookup"><span data-stu-id="acde2-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="acde2-120">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="acde2-120">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="acde2-121">フォームのデータは、URI クエリ文字列のような名前と値のペアとしてエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="acde2-121">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="acde2-122">これは、POST の既定の形式です。</span><span class="sxs-lookup"><span data-stu-id="acde2-122">This is the default format for POST.</span></span> |
| <span data-ttu-id="acde2-123">マルチパート フォーム データ</span><span class="sxs-lookup"><span data-stu-id="acde2-123">multipart/form-data</span></span> | <span data-ttu-id="acde2-124">フォームのデータは、マルチパート MIME メッセージとしてエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="acde2-124">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="acde2-125">サーバーにファイルをアップロードする場合は、この形式を使用します。</span><span class="sxs-lookup"><span data-stu-id="acde2-125">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="acde2-126">この記事のパート 1 x-www-form-urlencoded 形式を見ます。</span><span class="sxs-lookup"><span data-stu-id="acde2-126">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="acde2-127">[第 2 部](sending-html-form-data-part-2.md)マルチパート MIME について説明します。</span><span class="sxs-lookup"><span data-stu-id="acde2-127">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="acde2-128">複合型を送信します。</span><span class="sxs-lookup"><span data-stu-id="acde2-128">Sending Complex Types</span></span>

<span data-ttu-id="acde2-129">通常、複数のフォーム コントロールから取得した値から成る複合型が送信されます。</span><span class="sxs-lookup"><span data-stu-id="acde2-129">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="acde2-130">ステータスの更新を表す次のモデルを考慮してください。</span><span class="sxs-lookup"><span data-stu-id="acde2-130">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="acde2-131">受け付ける、Web API コント ローラーをここでは、 `Update` POST 要求を介してオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="acde2-131">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="acde2-132">このコント ローラーを使用して[アクション ベースのルーティング](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name)ルート テンプレートは、 &quot;api/{controller}/{controller}/{id}&quot;です。</span><span class="sxs-lookup"><span data-stu-id="acde2-132">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="acde2-133">クライアントは、データを送信&quot;/api/updates/complex&quot;です。</span><span class="sxs-lookup"><span data-stu-id="acde2-133">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>


<span data-ttu-id="acde2-134">これでユーザー状態の更新を送信するための HTML フォームを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="acde2-134">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="acde2-135">注意して、**アクション**フォーム上の属性は、コント ローラー アクションの URI。</span><span class="sxs-lookup"><span data-stu-id="acde2-135">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="acde2-136">いくつかの値の入力にフォームを次に示します。</span><span class="sxs-lookup"><span data-stu-id="acde2-136">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="acde2-137">ユーザーには、送信がクリックすると、ブラウザー HTTP 要求を送信すると、次のような。</span><span class="sxs-lookup"><span data-stu-id="acde2-137">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="acde2-138">要求本文に名前/値のペアとして書式設定されたフォームのデータが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="acde2-138">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="acde2-139">Web API がのインスタンスに名前/値ペアを自動的に変換、`Update`クラスです。</span><span class="sxs-lookup"><span data-stu-id="acde2-139">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="acde2-140">AJAX を使用してフォームのデータを送信します。</span><span class="sxs-lookup"><span data-stu-id="acde2-140">Sending Form Data via AJAX</span></span>

<span data-ttu-id="acde2-141">ユーザーは、フォームを送信するときに、ブラウザーは現在のページから移動し、応答メッセージの本文を表示します。</span><span class="sxs-lookup"><span data-stu-id="acde2-141">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="acde2-142">応答が HTML ページの場合は問題ありません。</span><span class="sxs-lookup"><span data-stu-id="acde2-142">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="acde2-143">Web API、ただし、応答本文は通常か空であるか、JSON などの構造化データが含まれています。</span><span class="sxs-lookup"><span data-stu-id="acde2-143">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="acde2-144">その場合は、方が有意義ページが応答を処理できるように、AJAX を使用して、フォーム データが要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="acde2-144">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="acde2-145">次のコードでは、jQuery を使用してデータをフォームを投稿する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="acde2-145">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="acde2-146">JQuery**送信**関数は、新しい関数にフォームのアクションを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="acde2-146">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="acde2-147">これには、[送信] ボタンの既定の動作がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="acde2-147">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="acde2-148">**シリアル化**関数は、名前/値ペアにフォームのデータをシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="acde2-148">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="acde2-149">フォームのデータをサーバーに送信する`$.post()`です。</span><span class="sxs-lookup"><span data-stu-id="acde2-149">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="acde2-150">要求が完了したら、`.success()`または`.error()`ハンドラーでは、ユーザーに適切なメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="acde2-150">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="acde2-151">単純型を送信します。</span><span class="sxs-lookup"><span data-stu-id="acde2-151">Sending Simple Types</span></span>

<span data-ttu-id="acde2-152">前のセクションで、Web API は、モデル クラスのインスタンスに逆シリアル化する複合型を送信します。</span><span class="sxs-lookup"><span data-stu-id="acde2-152">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="acde2-153">文字列などの単純型を送信することもできます。</span><span class="sxs-lookup"><span data-stu-id="acde2-153">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="acde2-154">単純型を送信する前に、複合型で値の代わりに折り返しを検討してください。</span><span class="sxs-lookup"><span data-stu-id="acde2-154">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="acde2-155">これは、利点を得られるモデルの検証のサーバー側でと、必要な場合は、モデルを拡張するが容易です。</span><span class="sxs-lookup"><span data-stu-id="acde2-155">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>


<span data-ttu-id="acde2-156">単純型を送信する基本的な手順は、同じ値が 2 つの微妙な違いがあります。</span><span class="sxs-lookup"><span data-stu-id="acde2-156">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="acde2-157">最初に、コント ローラーにする必要がありますを装飾するパラメーター名の**FromBody**属性。</span><span class="sxs-lookup"><span data-stu-id="acde2-157">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="acde2-158">既定では、Web API は、要求の URI から単純型を取得しようとします。</span><span class="sxs-lookup"><span data-stu-id="acde2-158">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="acde2-159">**FromBody**属性は Web API 要求の本文から値を読み取ることを示します。</span><span class="sxs-lookup"><span data-stu-id="acde2-159">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="acde2-160">Web API、応答本文最大で何度か読み取る、アクションの 1 つのパラメーターは要求の本体からのもののみです。</span><span class="sxs-lookup"><span data-stu-id="acde2-160">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="acde2-161">要求本文から複数の値を取得する必要がある場合は、複合型を定義します。</span><span class="sxs-lookup"><span data-stu-id="acde2-161">If you need to get multiple values from the request body, define a complex type.</span></span>


<span data-ttu-id="acde2-162">次に、クライアントは、次の形式の値を送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="acde2-162">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="acde2-163">具体的には、名前/値ペアの名前の部分は単純型の空にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="acde2-163">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="acde2-164">すべてのブラウザーが HTML フォームは、これをサポートしますが、次のようにスクリプトでこの形式を作成します。</span><span class="sxs-lookup"><span data-stu-id="acde2-164">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="acde2-165">サンプル フォームを次に示します。</span><span class="sxs-lookup"><span data-stu-id="acde2-165">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="acde2-166">フォーム値を送信するスクリプトを次に示します。</span><span class="sxs-lookup"><span data-stu-id="acde2-166">And here is the script to submit the form value.</span></span> <span data-ttu-id="acde2-167">前のスクリプトから唯一の違いに渡される引数、**投稿**関数。</span><span class="sxs-lookup"><span data-stu-id="acde2-167">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="acde2-168">同じアプローチを使用して、単純型の配列を送信できます。</span><span class="sxs-lookup"><span data-stu-id="acde2-168">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="acde2-169">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="acde2-169">Additional Resources</span></span>

[<span data-ttu-id="acde2-170">パート 2: ファイルのアップロードとマルチパート MIME</span><span class="sxs-lookup"><span data-stu-id="acde2-170">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
