---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: "コンテンツの ASP.NET Web API でネゴシエーション |Microsoft ドキュメント"
author: MikeWasson
description: "ASP.NET Web API HTTP コンテンツ ネゴシエーションを実装する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="feffc-103">ASP.NET Web API でコンテンツ ネゴシエーション</span><span class="sxs-lookup"><span data-stu-id="feffc-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="feffc-104">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="feffc-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="feffc-105">この記事では、ASP.NET Web API がコンテンツ ネゴシエーションを実装する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="feffc-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="feffc-106">HTTP の仕様 (RFC 2616)「使用可能な複数の表現が存在する場合は、指定された応答の最適な形式を選択した場合のプロセスです」としてコンテンツ ネゴシエーションを定義します。</span><span class="sxs-lookup"><span data-stu-id="feffc-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="feffc-107">HTTP でコンテンツ ネゴシエーションを主要なメカニズムは、これらの要求ヘッダーです。</span><span class="sxs-lookup"><span data-stu-id="feffc-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="feffc-108">**受信:**どのメディアの種類は"application/json に"など、応答として受け入れ可能な"application/xml、"またはカスタムのメディアの種類など、 &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="feffc-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="feffc-109">**受け入れる Charset:**どの文字セットは許容できますが、utf-8 または ISO 8859-1 などです。</span><span class="sxs-lookup"><span data-stu-id="feffc-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="feffc-110">**Accept-encoding:**コンテンツのエンコード方式は gzip など、許容されます。</span><span class="sxs-lookup"><span data-stu-id="feffc-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="feffc-111">**ブラウザーの言語:**優先される自然言語など"en-us"です。</span><span class="sxs-lookup"><span data-stu-id="feffc-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="feffc-112">サーバーは、HTTP 要求の他の部分にも確認できます。</span><span class="sxs-lookup"><span data-stu-id="feffc-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="feffc-113">たとえば、要求に X-要求-とヘッダーが含まれている場合、AJAX 要求を示すサーバーが既定値を JSON に Accept ヘッダーが存在しない場合。</span><span class="sxs-lookup"><span data-stu-id="feffc-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="feffc-114">この記事では、Web API が Accept、Accept-charset ヘッダーを使用する方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="feffc-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="feffc-115">(この時点ではありません Accept-encoding、Accept-language の組み込みのサポート。)</span><span class="sxs-lookup"><span data-stu-id="feffc-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="feffc-116">シリアル化</span><span class="sxs-lookup"><span data-stu-id="feffc-116">Serialization</span></span>

<span data-ttu-id="feffc-117">Web API コント ローラーには、CLR 型として、リソースが返された場合、パイプラインは、戻り値をシリアル化し、HTTP 応答の本文に書き込みます。</span><span class="sxs-lookup"><span data-stu-id="feffc-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="feffc-118">たとえば、次のコント ローラーのアクションがあるとします。</span><span class="sxs-lookup"><span data-stu-id="feffc-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="feffc-119">クライアントは、この HTTP 要求を送信可能性があります。</span><span class="sxs-lookup"><span data-stu-id="feffc-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="feffc-120">応答として、サーバーが送信可能性があります。</span><span class="sxs-lookup"><span data-stu-id="feffc-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="feffc-121">この例で、クライアント要求、JSON、Javascript、または「すべて」(\*/\*)。</span><span class="sxs-lookup"><span data-stu-id="feffc-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="feffc-122">応答の JSON 表現のサーバー、`Product`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="feffc-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="feffc-123">応答の Content-type ヘッダーが設定されていることを確認&quot;アプリケーション/json&quot;です。</span><span class="sxs-lookup"><span data-stu-id="feffc-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="feffc-124">コント ローラーを返すことも、 **HttpResponseMessage**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="feffc-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="feffc-125">応答本文の CLR オブジェクトを指定するには、呼び出し、 **CreateResponse**拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="feffc-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="feffc-126">このオプションでは、応答の詳細についてより詳細に制御をできます。</span><span class="sxs-lookup"><span data-stu-id="feffc-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="feffc-127">HTTP ヘッダーを追加、状態コードを設定するなどです。</span><span class="sxs-lookup"><span data-stu-id="feffc-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="feffc-128">リソースをシリアル化するオブジェクトが呼び出されると、*メディア フォーマッタ*です。</span><span class="sxs-lookup"><span data-stu-id="feffc-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="feffc-129">メディア フォーマッタから派生して、 **MediaTypeFormatter**クラスです。</span><span class="sxs-lookup"><span data-stu-id="feffc-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="feffc-130">Web API は、XML および JSON 用メディア フォーマッタを提供し、その他のメディアの種類をサポートするためにカスタム フォーマッタを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="feffc-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="feffc-131">カスタム フォーマッタを記述する方法については、次を参照してください。[メディア フォーマッタ](media-formatters.md)です。</span><span class="sxs-lookup"><span data-stu-id="feffc-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="feffc-132">コンテンツ ネゴシエーションのしくみ</span><span class="sxs-lookup"><span data-stu-id="feffc-132">How Content Negotiation Works</span></span>

<span data-ttu-id="feffc-133">最初に、パイプラインの取得、 **IContentNegotiator**からサービス、 **HttpConfiguration**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="feffc-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="feffc-134">メディアのフォーマッタの一覧も取得、 **HttpConfiguration.Formatters**コレクション。</span><span class="sxs-lookup"><span data-stu-id="feffc-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="feffc-135">次に、パイプラインを呼び出す**IContentNegotiatior.Negotiate**を渡して、します。</span><span class="sxs-lookup"><span data-stu-id="feffc-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="feffc-136">シリアル化するオブジェクトの種類</span><span class="sxs-lookup"><span data-stu-id="feffc-136">The type of object to serialize</span></span>
- <span data-ttu-id="feffc-137">メディア フォーマッタのコレクション</span><span class="sxs-lookup"><span data-stu-id="feffc-137">The collection of media formatters</span></span>
- <span data-ttu-id="feffc-138">HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="feffc-138">The HTTP request</span></span>

<span data-ttu-id="feffc-139">**Negotiate**メソッドは 2 つの情報を返します。</span><span class="sxs-lookup"><span data-stu-id="feffc-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="feffc-140">どのフォーマッタを使用するには</span><span class="sxs-lookup"><span data-stu-id="feffc-140">Which formatter to use</span></span>
- <span data-ttu-id="feffc-141">応答のメディアの種類</span><span class="sxs-lookup"><span data-stu-id="feffc-141">The media type for the response</span></span>

<span data-ttu-id="feffc-142">フォーマッタが見つからなかった場合、 **Negotiate**メソッドを返します。 **null**、およびクライアント渡さ HTTP エラー 406 (受容不可)。</span><span class="sxs-lookup"><span data-stu-id="feffc-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="feffc-143">次のコードは、コント ローラーを直接呼び出す方法コンテンツ ネゴシエーションを示しています。</span><span class="sxs-lookup"><span data-stu-id="feffc-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="feffc-144">このコードは、パイプラインを自動的には、何をします。</span><span class="sxs-lookup"><span data-stu-id="feffc-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="feffc-145">既定のコンテンツ ネゴシエーター</span><span class="sxs-lookup"><span data-stu-id="feffc-145">Default Content Negotiator</span></span>

<span data-ttu-id="feffc-146">**DefaultContentNegotiator**クラスの既定の実装を提供する**IContentNegotiator**です。</span><span class="sxs-lookup"><span data-stu-id="feffc-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="feffc-147">いくつかの条件を使用したフォーマッタを選択します。</span><span class="sxs-lookup"><span data-stu-id="feffc-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="feffc-148">最初に、フォーマッタは、型をシリアル化できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="feffc-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="feffc-149">これは呼び出すことによって検証**MediaTypeFormatter.CanWriteType**です。</span><span class="sxs-lookup"><span data-stu-id="feffc-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="feffc-150">次に、コンテンツ ネゴシエーターを使用して、各フォーマッタを調べ、どのようにに適合する HTTP 要求を評価します。</span><span class="sxs-lookup"><span data-stu-id="feffc-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="feffc-151">一致を評価するには、コンテンツ ネゴシエーター 2 つの点を調べ、フォーマッタ。</span><span class="sxs-lookup"><span data-stu-id="feffc-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="feffc-152">**SupportedMediaTypes**コレクションで、サポートされているメディアの種類の一覧が含まれています。</span><span class="sxs-lookup"><span data-stu-id="feffc-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="feffc-153">コンテンツ ネゴシエーターでは、この一覧に対して、要求の Accept ヘッダーを照合します。</span><span class="sxs-lookup"><span data-stu-id="feffc-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="feffc-154">Accept ヘッダーが範囲を含めることができますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="feffc-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="feffc-155">たとえば、テキストと一致するは「テキスト/プレーン」/\*または\* /\*です。</span><span class="sxs-lookup"><span data-stu-id="feffc-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="feffc-156">**MediaTypeMappings**のリストを含むコレクション**MediaTypeMapping**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="feffc-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="feffc-157">**MediaTypeMapping**クラスには、メディアの種類が HTTP 要求を一致するように汎用的な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="feffc-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="feffc-158">たとえば、カスタム HTTP ヘッダーを特定のメディアの種類にマップことでした。</span><span class="sxs-lookup"><span data-stu-id="feffc-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="feffc-159">複数ある場合と一致する、最高品質係数 wins と一致します。</span><span class="sxs-lookup"><span data-stu-id="feffc-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="feffc-160">例:</span><span class="sxs-lookup"><span data-stu-id="feffc-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="feffc-161">この例では、アプリケーション/json に暗黙的な品質ファクター 1.0 がアプリケーションまたは xml 上で優先されるようにします。</span><span class="sxs-lookup"><span data-stu-id="feffc-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="feffc-162">一致が見つからない場合、コンテンツ ネゴシエーターは、存在する場合、要求本文のメディアの種類で一致を試みます。</span><span class="sxs-lookup"><span data-stu-id="feffc-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="feffc-163">たとえば、要求に JSON データが含まれている場合、コンテンツ ネゴシエーターを JSON フォーマッタを探します。</span><span class="sxs-lookup"><span data-stu-id="feffc-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="feffc-164">一致が見つからない、コンテンツ ネゴシエーターは単に型をシリアル化できる最初のフォーマッタを取得します。</span><span class="sxs-lookup"><span data-stu-id="feffc-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="feffc-165">文字エンコーディングを選択します。</span><span class="sxs-lookup"><span data-stu-id="feffc-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="feffc-166">フォーマッタが選択されているコンテンツ ネゴシエーターが選択、最適な文字エン コードを見て、 **SupportedEncodings**プロパティをフォーマッタおよび (存在する場合)、要求で、Accept-charset ヘッダーと一致することです。</span><span class="sxs-lookup"><span data-stu-id="feffc-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
