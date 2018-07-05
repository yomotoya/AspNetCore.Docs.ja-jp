---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: ASP.NET Web API 2.1 の BSON サポート |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7c5e763d92295a83b3431e9ec6e305b07f8a64d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370495"
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="bf896-102">ASP.NET Web API 2.1 の BSON サポート</span><span class="sxs-lookup"><span data-stu-id="bf896-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="bf896-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bf896-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bf896-104">Web API 2.1 の BSON サポートを紹介します。</span><span class="sxs-lookup"><span data-stu-id="bf896-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="bf896-105">このトピックでは、Web API コント ローラー (サーバー側) と .NET クライアント アプリでは、BSON を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="bf896-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="bf896-106">BSON とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="bf896-106">What is BSON?</span></span>

<span data-ttu-id="bf896-107">[BSON](http://bsonspec.org/)はバイナリ シリアル化形式です。</span><span class="sxs-lookup"><span data-stu-id="bf896-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="bf896-108">"BSON"は"Binary JSON"を表しますが、BSON と JSON はまったく違う方法でシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="bf896-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="bf896-109">BSON は、「JSON のような」、オブジェクトが JSON のような名前と値のペアとして表されるためです。</span><span class="sxs-lookup"><span data-stu-id="bf896-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="bf896-110">JSON とは異なり、数値データ型がバイト、文字列ではないとして格納されます。</span><span class="sxs-lookup"><span data-stu-id="bf896-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="bf896-111">BSON は、軽量、簡単にスキャンする、エンコードとデコードに高速に設計されています。</span><span class="sxs-lookup"><span data-stu-id="bf896-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="bf896-112">BSON は JSON のサイズと同等です。</span><span class="sxs-lookup"><span data-stu-id="bf896-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="bf896-113">データによっては、BSON ペイロードは JSON ペイロードより小さいか大きいする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bf896-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="bf896-114">イメージ ファイルなどのバイナリ データをシリアル化するため、BSON は、バイナリ データは base64 でエンコードされたがないため、JSON よりも小さい。</span><span class="sxs-lookup"><span data-stu-id="bf896-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="bf896-115">BSON ドキュメントは、簡単にスキャンするため、パーサーは、それらをデコードすることがなく要素をスキップできますので長のフィールドでは要素が付きます。</span><span class="sxs-lookup"><span data-stu-id="bf896-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="bf896-116">エンコードおよびデコードは数値データ型がない文字列、数値として格納されているため、効率的です。</span><span class="sxs-lookup"><span data-stu-id="bf896-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="bf896-117">.NET クライアント アプリなどのネイティブ クライアントは、BSON を使用して、JSON や XML などのテキスト ベース形式の代わりに活用できます。</span><span class="sxs-lookup"><span data-stu-id="bf896-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="bf896-118">ブラウザー クライアントはおそらく json では、引き続き使用ためにする JavaScript は、JSON ペイロードに直接変換できます。</span><span class="sxs-lookup"><span data-stu-id="bf896-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="bf896-119">さいわい、Web API を使用して[コンテンツ ネゴシエーション](content-negotiation.md)ので、API が両方の形式をサポートして、クライアントを選択します。</span><span class="sxs-lookup"><span data-stu-id="bf896-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="bf896-120">サーバー上の BSON を有効にします。</span><span class="sxs-lookup"><span data-stu-id="bf896-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="bf896-121">Web API 構成を追加、 **BsonMediaTypeFormatter**フォーマッタのコレクションにします。</span><span class="sxs-lookup"><span data-stu-id="bf896-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="bf896-122">これで、クライアントは、"application/bson"を要求する Web API は BSON フォーマッタに使用します。</span><span class="sxs-lookup"><span data-stu-id="bf896-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="bf896-123">BSON を他のメディアの種類に関連付ける、SupportedMediaTypes コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="bf896-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="bf896-124">次のコードでは、サポートされているメディアの種類を"application/vnd.contoso"を追加します。</span><span class="sxs-lookup"><span data-stu-id="bf896-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="bf896-125">例 HTTP セッション</span><span class="sxs-lookup"><span data-stu-id="bf896-125">Example HTTP Session</span></span>

<span data-ttu-id="bf896-126">この例では、単純な Web API コント ローラーと次のモデル クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="bf896-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="bf896-127">クライアントは、次の HTTP 要求を送信可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bf896-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="bf896-128">応答を次に示します。</span><span class="sxs-lookup"><span data-stu-id="bf896-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="bf896-129">ここでのバイナリ データに置き換えました&quot;.&quot;文字。</span><span class="sxs-lookup"><span data-stu-id="bf896-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="bf896-130">Fiddler の表示から生の 16 進値の次のスクリーン ショット。</span><span class="sxs-lookup"><span data-stu-id="bf896-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="bf896-131">BSON HttpClient の併用</span><span class="sxs-lookup"><span data-stu-id="bf896-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="bf896-132">.NET クライアント アプリは BSON フォーマッタを使用できます**HttpClient**します。</span><span class="sxs-lookup"><span data-stu-id="bf896-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="bf896-133">詳細については**HttpClient**を参照してください[Web API から、.NET クライアントを呼び出す](../advanced/calling-a-web-api-from-a-net-client.md)します。</span><span class="sxs-lookup"><span data-stu-id="bf896-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="bf896-134">次のコードは、BSON を受け取り、応答で BSON ペイロードを逆シリアル化する GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="bf896-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="bf896-135">サーバーから BSON を要求するには、"application/bson"に Accept ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="bf896-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="bf896-136">応答本文を逆シリアル化を使用して、 **BsonMediaTypeFormatter**します。</span><span class="sxs-lookup"><span data-stu-id="bf896-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="bf896-137">このフォーマッタでない既定のフォーマッタのコレクションのため、応答本文を読み取るときに指定する必要は。</span><span class="sxs-lookup"><span data-stu-id="bf896-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="bf896-138">次の例では、BSON を含む POST 要求を送信する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="bf896-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="bf896-139">このコードの多くは、前の例と同じです。</span><span class="sxs-lookup"><span data-stu-id="bf896-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="bf896-140">しかし、 **PostAsync**メソッドでは、指定**BsonMediaTypeFormatter**フォーマッタと。</span><span class="sxs-lookup"><span data-stu-id="bf896-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="bf896-141">最上位レベルのプリミティブ型をシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="bf896-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="bf896-142">すべての BSON ドキュメントでは、キー/値ペアの一覧を示します。BSON 仕様では、整数や文字列などの単一の raw 値をシリアル化するための構文が定義されていません。</span><span class="sxs-lookup"><span data-stu-id="bf896-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="bf896-143">このような制限を回避する、 **BsonMediaTypeFormatter**プリミティブ型を特殊なケースとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="bf896-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="bf896-144">をシリアル化する前に、値に変換しますキー/値ペア"Value"キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="bf896-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="bf896-145">たとえば、API コント ローラーは、整数を返します。</span><span class="sxs-lookup"><span data-stu-id="bf896-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="bf896-146">をシリアル化する前に BSON フォーマッタは、これを次のキー/値ペアに変換します。</span><span class="sxs-lookup"><span data-stu-id="bf896-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="bf896-147">逆シリアル化するときに、フォーマッタは、元の値に、データを変換します。</span><span class="sxs-lookup"><span data-stu-id="bf896-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="bf896-148">ただし、異なる BSON パーサーを使用するクライアントは、web API は、生の値を返す場合、このケースを処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bf896-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="bf896-149">一般に、生の値ではなく、構造化データを返すことを検討してください。</span><span class="sxs-lookup"><span data-stu-id="bf896-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bf896-150">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="bf896-150">Additional Resources</span></span>

[<span data-ttu-id="bf896-151">Web API の BSON サンプル</span><span class="sxs-lookup"><span data-stu-id="bf896-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="bf896-152">メディア フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="bf896-152">Media Formatters</span></span>](media-formatters.md)
