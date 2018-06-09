---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: コンテンツの ASP.NET Web API でネゴシエーション |Microsoft ドキュメント
author: MikeWasson
description: ASP.NET Web API HTTP コンテンツ ネゴシエーションを実装する方法について説明します。
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
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "26507021"
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="0a68f-103">ASP.NET Web API でコンテンツ ネゴシエーション</span><span class="sxs-lookup"><span data-stu-id="0a68f-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="0a68f-104">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0a68f-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0a68f-105">この記事では、ASP.NET Web API がコンテンツ ネゴシエーションを実装する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="0a68f-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="0a68f-106">HTTP の仕様 (RFC 2616)「使用可能な複数の表現が存在する場合は、指定された応答の最適な形式を選択した場合のプロセスです」としてコンテンツ ネゴシエーションを定義します。</span><span class="sxs-lookup"><span data-stu-id="0a68f-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="0a68f-107">HTTP でコンテンツ ネゴシエーションを主要なメカニズムは、これらの要求ヘッダーです。</span><span class="sxs-lookup"><span data-stu-id="0a68f-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="0a68f-108">**受信:** どのメディアの種類は"application/json に"など、応答として受け入れ可能な"application/xml、"またはカスタムのメディアの種類など、 &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="0a68f-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="0a68f-109">**受け入れる Charset:** どの文字セットは許容できますが、utf-8 または ISO 8859-1 などです。</span><span class="sxs-lookup"><span data-stu-id="0a68f-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="0a68f-110">**Accept-encoding:** コンテンツのエンコード方式は gzip など、許容されます。</span><span class="sxs-lookup"><span data-stu-id="0a68f-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="0a68f-111">**ブラウザーの言語:** 優先される自然言語など"en-us"です。</span><span class="sxs-lookup"><span data-stu-id="0a68f-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="0a68f-112">サーバーは、HTTP 要求の他の部分にも確認できます。</span><span class="sxs-lookup"><span data-stu-id="0a68f-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="0a68f-113">たとえば、要求に X-要求-とヘッダーが含まれている場合、AJAX 要求を示すサーバーが既定値を JSON に Accept ヘッダーが存在しない場合。</span><span class="sxs-lookup"><span data-stu-id="0a68f-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="0a68f-114">この記事では、Web API が Accept、Accept-charset ヘッダーを使用する方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="0a68f-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="0a68f-115">(この時点ではありません Accept-encoding、Accept-language の組み込みのサポート。)</span><span class="sxs-lookup"><span data-stu-id="0a68f-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="0a68f-116">シリアル化</span><span class="sxs-lookup"><span data-stu-id="0a68f-116">Serialization</span></span>

<span data-ttu-id="0a68f-117">Web API コント ローラーには、CLR 型として、リソースが返された場合、パイプラインは、戻り値をシリアル化し、HTTP 応答の本文に書き込みます。</span><span class="sxs-lookup"><span data-stu-id="0a68f-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="0a68f-118">たとえば、次のコント ローラーのアクションがあるとします。</span><span class="sxs-lookup"><span data-stu-id="0a68f-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="0a68f-119">クライアントは、この HTTP 要求を送信可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0a68f-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="0a68f-120">応答として、サーバーが送信可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0a68f-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="0a68f-121">この例で、クライアント要求、JSON、Javascript、または「すべて」(\*/\*)。</span><span class="sxs-lookup"><span data-stu-id="0a68f-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="0a68f-122">応答の JSON 表現のサーバー、`Product`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="0a68f-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="0a68f-123">応答の Content-type ヘッダーが設定されていることを確認&quot;アプリケーション/json&quot;です。</span><span class="sxs-lookup"><span data-stu-id="0a68f-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="0a68f-124">コント ローラーを返すことも、 **HttpResponseMessage**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="0a68f-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="0a68f-125">応答本文の CLR オブジェクトを指定するには、呼び出し、 **CreateResponse**拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="0a68f-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="0a68f-126">このオプションでは、応答の詳細についてより詳細に制御をできます。</span><span class="sxs-lookup"><span data-stu-id="0a68f-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="0a68f-127">HTTP ヘッダーを追加、状態コードを設定するなどです。</span><span class="sxs-lookup"><span data-stu-id="0a68f-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="0a68f-128">リソースをシリアル化するオブジェクトが呼び出されると、*メディア フォーマッタ*です。</span><span class="sxs-lookup"><span data-stu-id="0a68f-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="0a68f-129">メディア フォーマッタから派生して、 **MediaTypeFormatter**クラスです。</span><span class="sxs-lookup"><span data-stu-id="0a68f-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="0a68f-130">Web API は、XML および JSON 用メディア フォーマッタを提供し、その他のメディアの種類をサポートするためにカスタム フォーマッタを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="0a68f-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="0a68f-131">カスタム フォーマッタを記述する方法については、次を参照してください。[メディア フォーマッタ](media-formatters.md)です。</span><span class="sxs-lookup"><span data-stu-id="0a68f-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="0a68f-132">コンテンツ ネゴシエーションのしくみ</span><span class="sxs-lookup"><span data-stu-id="0a68f-132">How Content Negotiation Works</span></span>

<span data-ttu-id="0a68f-133">最初に、パイプラインの取得、 **IContentNegotiator**からサービス、 **HttpConfiguration**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="0a68f-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="0a68f-134">メディアのフォーマッタの一覧も取得、 **HttpConfiguration.Formatters**コレクション。</span><span class="sxs-lookup"><span data-stu-id="0a68f-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="0a68f-135">次に、パイプラインを呼び出す**IContentNegotiatior.Negotiate**を渡して、します。</span><span class="sxs-lookup"><span data-stu-id="0a68f-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="0a68f-136">シリアル化するオブジェクトの種類</span><span class="sxs-lookup"><span data-stu-id="0a68f-136">The type of object to serialize</span></span>
- <span data-ttu-id="0a68f-137">メディア フォーマッタのコレクション</span><span class="sxs-lookup"><span data-stu-id="0a68f-137">The collection of media formatters</span></span>
- <span data-ttu-id="0a68f-138">HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="0a68f-138">The HTTP request</span></span>

<span data-ttu-id="0a68f-139">**Negotiate**メソッドは 2 つの情報を返します。</span><span class="sxs-lookup"><span data-stu-id="0a68f-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="0a68f-140">どのフォーマッタを使用するには</span><span class="sxs-lookup"><span data-stu-id="0a68f-140">Which formatter to use</span></span>
- <span data-ttu-id="0a68f-141">応答のメディアの種類</span><span class="sxs-lookup"><span data-stu-id="0a68f-141">The media type for the response</span></span>

<span data-ttu-id="0a68f-142">フォーマッタが見つからなかった場合、 **Negotiate**メソッドを返します。 **null**、およびクライアント渡さ HTTP エラー 406 (受容不可)。</span><span class="sxs-lookup"><span data-stu-id="0a68f-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="0a68f-143">次のコードは、コント ローラーを直接呼び出す方法コンテンツ ネゴシエーションを示しています。</span><span class="sxs-lookup"><span data-stu-id="0a68f-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="0a68f-144">このコードは、パイプラインを自動的には、何をします。</span><span class="sxs-lookup"><span data-stu-id="0a68f-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="0a68f-145">既定のコンテンツ ネゴシエーター</span><span class="sxs-lookup"><span data-stu-id="0a68f-145">Default Content Negotiator</span></span>

<span data-ttu-id="0a68f-146">**DefaultContentNegotiator**クラスの既定の実装を提供する**IContentNegotiator**です。</span><span class="sxs-lookup"><span data-stu-id="0a68f-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="0a68f-147">いくつかの条件を使用したフォーマッタを選択します。</span><span class="sxs-lookup"><span data-stu-id="0a68f-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="0a68f-148">最初に、フォーマッタは、型をシリアル化できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="0a68f-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="0a68f-149">これは呼び出すことによって検証**MediaTypeFormatter.CanWriteType**です。</span><span class="sxs-lookup"><span data-stu-id="0a68f-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="0a68f-150">次に、コンテンツ ネゴシエーターを使用して、各フォーマッタを調べ、どのようにに適合する HTTP 要求を評価します。</span><span class="sxs-lookup"><span data-stu-id="0a68f-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="0a68f-151">一致を評価するには、コンテンツ ネゴシエーター 2 つの点を調べ、フォーマッタ。</span><span class="sxs-lookup"><span data-stu-id="0a68f-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="0a68f-152">**SupportedMediaTypes**コレクションで、サポートされているメディアの種類の一覧が含まれています。</span><span class="sxs-lookup"><span data-stu-id="0a68f-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="0a68f-153">コンテンツ ネゴシエーターでは、この一覧に対して、要求の Accept ヘッダーを照合します。</span><span class="sxs-lookup"><span data-stu-id="0a68f-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="0a68f-154">Accept ヘッダーが範囲を含めることができますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0a68f-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="0a68f-155">たとえば、テキストと一致するは「テキスト/プレーン」/\*または\* /\*です。</span><span class="sxs-lookup"><span data-stu-id="0a68f-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="0a68f-156">**MediaTypeMappings**のリストを含むコレクション**MediaTypeMapping**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="0a68f-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="0a68f-157">**MediaTypeMapping**クラスには、メディアの種類が HTTP 要求を一致するように汎用的な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="0a68f-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="0a68f-158">たとえば、カスタム HTTP ヘッダーを特定のメディアの種類にマップことでした。</span><span class="sxs-lookup"><span data-stu-id="0a68f-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="0a68f-159">複数ある場合と一致する、最高品質係数 wins と一致します。</span><span class="sxs-lookup"><span data-stu-id="0a68f-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="0a68f-160">例えば:</span><span class="sxs-lookup"><span data-stu-id="0a68f-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="0a68f-161">この例では、アプリケーション/json に暗黙的な品質ファクター 1.0 がアプリケーションまたは xml 上で優先されるようにします。</span><span class="sxs-lookup"><span data-stu-id="0a68f-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="0a68f-162">一致が見つからない場合、コンテンツ ネゴシエーターは、存在する場合、要求本文のメディアの種類で一致を試みます。</span><span class="sxs-lookup"><span data-stu-id="0a68f-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="0a68f-163">たとえば、要求に JSON データが含まれている場合、コンテンツ ネゴシエーターを JSON フォーマッタを探します。</span><span class="sxs-lookup"><span data-stu-id="0a68f-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="0a68f-164">一致が見つからない、コンテンツ ネゴシエーターは単に型をシリアル化できる最初のフォーマッタを取得します。</span><span class="sxs-lookup"><span data-stu-id="0a68f-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="0a68f-165">文字エンコーディングを選択します。</span><span class="sxs-lookup"><span data-stu-id="0a68f-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="0a68f-166">フォーマッタが選択されているコンテンツ ネゴシエーターが選択、最適な文字エン コードを見て、 **SupportedEncodings**プロパティをフォーマッタおよび (存在する場合)、要求で、Accept-charset ヘッダーと一致することです。</span><span class="sxs-lookup"><span data-stu-id="0a68f-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
