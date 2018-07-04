---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: ASP.NET Web API 2 のメディア フォーマッタ |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 824fea5a8837ff8b09af832b7d2a094c7d82907b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368917"
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="17b15-102">ASP.NET Web API 2 のメディア フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="17b15-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="17b15-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="17b15-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="17b15-104">このチュートリアルでは、ASP.NET Web API で他のメディア形式をサポートする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="17b15-104">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="17b15-105">インターネット メディアの種類</span><span class="sxs-lookup"><span data-stu-id="17b15-105">Internet Media Types</span></span>

<span data-ttu-id="17b15-106">MIME の種類とも呼ばれます。 メディアの種類は、データの一部の形式を識別します。</span><span class="sxs-lookup"><span data-stu-id="17b15-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="17b15-107">HTTP では、メディアの種類には、メッセージ本文の形式について説明します。</span><span class="sxs-lookup"><span data-stu-id="17b15-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="17b15-108">メディアの種類は、2 つの文字列、種類、およびサブタイプで構成されます。</span><span class="sxs-lookup"><span data-stu-id="17b15-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="17b15-109">例えば:</span><span class="sxs-lookup"><span data-stu-id="17b15-109">For example:</span></span>

- <span data-ttu-id="17b15-110">text/html</span><span class="sxs-lookup"><span data-stu-id="17b15-110">text/html</span></span>
- <span data-ttu-id="17b15-111">image/png</span><span class="sxs-lookup"><span data-stu-id="17b15-111">image/png</span></span>
- <span data-ttu-id="17b15-112">application/json</span><span class="sxs-lookup"><span data-stu-id="17b15-112">application/json</span></span>

<span data-ttu-id="17b15-113">HTTP メッセージにエンティティ ボディが含まれている場合、Content-type ヘッダーには、メッセージ本文の形式を指定します。</span><span class="sxs-lookup"><span data-stu-id="17b15-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="17b15-114">これは、受信側に、メッセージ本文の内容を解析する方法を指示します。</span><span class="sxs-lookup"><span data-stu-id="17b15-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="17b15-115">たとえば、HTTP 応答に PNG イメージが含まれている場合、応答は、次のヘッダーがあります。</span><span class="sxs-lookup"><span data-stu-id="17b15-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="17b15-116">クライアントは、要求メッセージを送信するときに、Accept ヘッダーに含めることができます。</span><span class="sxs-lookup"><span data-stu-id="17b15-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="17b15-117">Accept ヘッダーでは、サーバーを使用してメディアの種類、クライアント、サーバーを指示します。</span><span class="sxs-lookup"><span data-stu-id="17b15-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="17b15-118">例えば:</span><span class="sxs-lookup"><span data-stu-id="17b15-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="17b15-119">このヘッダーは、HTML、XHTML、または XML のいずれかに、クライアントが要求しているサーバーに指示します。</span><span class="sxs-lookup"><span data-stu-id="17b15-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="17b15-120">Web API のシリアル化し、HTTP メッセージ本文を逆シリアル化、メディアの種類を決定します。</span><span class="sxs-lookup"><span data-stu-id="17b15-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="17b15-121">Web API が XML、JSON、BSON、および url エンコード フォーム データの組み込みサポートと、追加のメディアの種類をサポートするには記述することで、*メディア フォーマッタ*します。</span><span class="sxs-lookup"><span data-stu-id="17b15-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="17b15-122">メディア フォーマッタを作成するには、これらのクラスのいずれかから派生します。</span><span class="sxs-lookup"><span data-stu-id="17b15-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="17b15-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="17b15-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="17b15-124">このクラスは非同期の読み取りと書き込みを行うメソッドです。</span><span class="sxs-lookup"><span data-stu-id="17b15-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="17b15-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="17b15-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="17b15-126">このクラスから派生**MediaTypeFormatter**が、同期読み取り/書き込みのメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="17b15-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="17b15-127">派生する**BufferedMediaTypeFormatter**の非同期コードではありませんが、I/O 中に呼び出し元のスレッドをブロックできますをも意味があるため、単純なは。</span><span class="sxs-lookup"><span data-stu-id="17b15-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="17b15-128">例: CSV のメディア フォーマッタを作成します。</span><span class="sxs-lookup"><span data-stu-id="17b15-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="17b15-129">次の例は、コンマ区切り値 (CSV) 形式に Product オブジェクトをシリアル化できるメディア タイプ フォーマッタの一覧です。</span><span class="sxs-lookup"><span data-stu-id="17b15-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="17b15-130">この例は、このチュートリアルで定義されている製品の種類を使用して[そのサポートの CRUD 操作を作成するには、Web API](../older-versions/creating-a-web-api-that-supports-crud-operations.md)します。</span><span class="sxs-lookup"><span data-stu-id="17b15-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="17b15-131">Product オブジェクトの定義を次に示します。</span><span class="sxs-lookup"><span data-stu-id="17b15-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="17b15-132">CSV フォーマッタを実装するから派生したクラスを定義**BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="17b15-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="17b15-133">コンス トラクターに、フォーマッタでサポートされるメディアの種類を追加します。</span><span class="sxs-lookup"><span data-stu-id="17b15-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="17b15-134">この例では、フォーマッタは、1 つのメディアの種類をサポートしています&quot;テキスト/csv&quot;:。</span><span class="sxs-lookup"><span data-stu-id="17b15-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="17b15-135">上書き、**のオーバーライド**をフォーマッタの種類を示すためにメソッドがシリアル化できます。</span><span class="sxs-lookup"><span data-stu-id="17b15-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="17b15-136">この例では、フォーマッタをシリアル化できるシングル`Product`オブジェクトのコレクションおよび`Product`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="17b15-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="17b15-137">同様に、オーバーライド、 **CanReadType**をフォーマッタの種類を示すためにメソッドの逆シリアル化できます。</span><span class="sxs-lookup"><span data-stu-id="17b15-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="17b15-138">この例では、フォーマッタはサポートされない逆シリアル化、ため、メソッドは単に返します**false**します。</span><span class="sxs-lookup"><span data-stu-id="17b15-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="17b15-139">最後に、オーバーライド、 **WriteToStream**メソッド。</span><span class="sxs-lookup"><span data-stu-id="17b15-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="17b15-140">このメソッドでは、ストリームに書き込むことで、型をシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="17b15-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="17b15-141">フォーマッタで逆シリアル化をサポートする場合は上書きも、 **ReadFromStream**メソッド。</span><span class="sxs-lookup"><span data-stu-id="17b15-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="17b15-142">Web API パイプラインへのメディア フォーマッタの追加</span><span class="sxs-lookup"><span data-stu-id="17b15-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="17b15-143">メディア タイプ フォーマッタを Web API パイプラインに追加するには、使用、**フォーマッタ**プロパティを**HttpConfiguration**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="17b15-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="17b15-144">文字エン コード</span><span class="sxs-lookup"><span data-stu-id="17b15-144">Character Encodings</span></span>

<span data-ttu-id="17b15-145">必要に応じて、メディア フォーマッタは、utf-8 または ISO 8859-1 などの複数の文字エンコーディングをサポートできます。</span><span class="sxs-lookup"><span data-stu-id="17b15-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="17b15-146">コンス トラクターの 1 つ以上追加[System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx)型を**SupportedEncodings**コレクション。</span><span class="sxs-lookup"><span data-stu-id="17b15-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="17b15-147">既定の最初のエンコーディングを配置します。</span><span class="sxs-lookup"><span data-stu-id="17b15-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="17b15-148">**WriteToStream**と**ReadFromStream**メソッドを呼び出して[MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx)優先文字エン コードを選択します。</span><span class="sxs-lookup"><span data-stu-id="17b15-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="17b15-149">このメソッドでは、サポートされているエンコーディングのリストに照らして要求ヘッダーと一致します。</span><span class="sxs-lookup"><span data-stu-id="17b15-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="17b15-150">使用して、返された**エンコード**を読み取るか、ストリームからの書き込み。</span><span class="sxs-lookup"><span data-stu-id="17b15-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
