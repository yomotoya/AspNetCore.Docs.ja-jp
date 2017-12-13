---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: "ASP.NET Web API 2.1 で BSON サポート |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 08ef1564b2f8f11294c3bb1ec0ff9a3d063895b6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="62b86-102">ASP.NET Web API 2.1 で BSON サポート</span><span class="sxs-lookup"><span data-stu-id="62b86-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="62b86-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="62b86-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="62b86-104">Web API 2.1 には、BSON のサポートが導入されています。</span><span class="sxs-lookup"><span data-stu-id="62b86-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="62b86-105">このトピックでは、Web API コント ローラー (サーバー側) と .NET クライアント アプリで BSON を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="62b86-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="62b86-106">BSON とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="62b86-106">What is BSON?</span></span>

<span data-ttu-id="62b86-107">[BSON](http://bsonspec.org/)バイナリのシリアル化形式です。</span><span class="sxs-lookup"><span data-stu-id="62b86-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="62b86-108">"BSON"は"Binary JSON"を表しますが、BSON と JSON がまったく違う方法でシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="62b86-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="62b86-109">BSON は「JSON のような」、オブジェクトが JSON のような名前と値のペアとして表されるためです。</span><span class="sxs-lookup"><span data-stu-id="62b86-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="62b86-110">JSON とは異なり、数値データ型がバイト、文字列ではありませんとして格納されます。</span><span class="sxs-lookup"><span data-stu-id="62b86-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="62b86-111">BSON は、軽量、簡単には、scan、エンコードとデコードに高速されるようにするために設計されました。</span><span class="sxs-lookup"><span data-stu-id="62b86-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="62b86-112">BSON は JSON のサイズと同等です。</span><span class="sxs-lookup"><span data-stu-id="62b86-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="62b86-113">データによっては、BSON ペイロードは、JSON ペイロードより小さいか大きいする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="62b86-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="62b86-114">イメージ ファイルなど、バイナリ データをシリアル化するため BSON JSON よりも小さいので、バイナリ データはなく、base64 でエンコードされました。</span><span class="sxs-lookup"><span data-stu-id="62b86-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data does is not base64-encoded.</span></span>
- <span data-ttu-id="62b86-115">BSON ドキュメントは、スキャン、パーサーはそれらをデコードすることがなく要素をスキップすることができますので、長さフィールドを持つ要素が前付くので簡単です。</span><span class="sxs-lookup"><span data-stu-id="62b86-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="62b86-116">エンコードとデコードは、数値データ型がない文字列、数値として格納されているため効率的、です。</span><span class="sxs-lookup"><span data-stu-id="62b86-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="62b86-117">.NET クライアント アプリなどのネイティブ クライアントは、BSON を使用して、JSON または XML などのテキスト ベース形式の代わりに活用できます。</span><span class="sxs-lookup"><span data-stu-id="62b86-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="62b86-118">ブラウザー クライアント用可能性がありますする JSON のオプションを使用するので、JavaScript が JSON ペイロードを直接変換できます。</span><span class="sxs-lookup"><span data-stu-id="62b86-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="62b86-119">幸いにも、Web API を使用して[コンテンツ ネゴシエーション](content-negotiation.md)API の両方の形式をサポートし、クライアントを選択できるよう、します。</span><span class="sxs-lookup"><span data-stu-id="62b86-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="62b86-120">サーバー上の BSON を有効にします。</span><span class="sxs-lookup"><span data-stu-id="62b86-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="62b86-121">構成では、Web API、追加、 **BsonMediaTypeFormatter**フォーマッタのコレクション。</span><span class="sxs-lookup"><span data-stu-id="62b86-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="62b86-122">現在、クライアントは、「application/bson」を要求する Web API には、BSON フォーマッタが使用されます。</span><span class="sxs-lookup"><span data-stu-id="62b86-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="62b86-123">BSON を他のメディア タイプに関連付ける、SupportedMediaTypes コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="62b86-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="62b86-124">次のコードでは、サポートされているメディアの種類に"application/vnd.contoso"を追加します。</span><span class="sxs-lookup"><span data-stu-id="62b86-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="62b86-125">HTTP セッションの例</span><span class="sxs-lookup"><span data-stu-id="62b86-125">Example HTTP Session</span></span>

<span data-ttu-id="62b86-126">この例では、単純な Web API コント ローラーと次のモデル クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="62b86-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="62b86-127">クライアントは、次の HTTP 要求を送信可能性があります。</span><span class="sxs-lookup"><span data-stu-id="62b86-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="62b86-128">応答を次に示します。</span><span class="sxs-lookup"><span data-stu-id="62b86-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="62b86-129">ここでのバイナリ データは置き換えた&quot;.&quot;文字です。</span><span class="sxs-lookup"><span data-stu-id="62b86-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="62b86-130">Fiddler の番組を生の 16 進値の次のスクリーン ショット。</span><span class="sxs-lookup"><span data-stu-id="62b86-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="62b86-131">BSON HttpClient の併用</span><span class="sxs-lookup"><span data-stu-id="62b86-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="62b86-132">.NET クライアント アプリで BSON フォーマッタを使用することができます**HttpClient**です。</span><span class="sxs-lookup"><span data-stu-id="62b86-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="62b86-133">詳細については**HttpClient**を参照してください[Web API から、.NET クライアントを呼び出す](../advanced/calling-a-web-api-from-a-net-client.md)です。</span><span class="sxs-lookup"><span data-stu-id="62b86-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="62b86-134">次のコードとを受け取ってする BSON、応答で BSON ペイロードを逆シリアル化 GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="62b86-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="62b86-135">BSON を要求するには、サーバーから、するには、「application/bson」に Accept ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="62b86-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="62b86-136">応答本文を逆シリアル化を使用して、 **BsonMediaTypeFormatter**です。</span><span class="sxs-lookup"><span data-stu-id="62b86-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="62b86-137">このフォーマッタが既定のフォーマッタのコレクションで応答本体を読み取るときに指定する必要があるためです。</span><span class="sxs-lookup"><span data-stu-id="62b86-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="62b86-138">次の例では、BSON を含む POST 要求を送信する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="62b86-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="62b86-139">このコードの多くは、前の例と同じです。</span><span class="sxs-lookup"><span data-stu-id="62b86-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="62b86-140">しかし、 **PostAsync**メソッドを指定して**BsonMediaTypeFormatter**フォーマッタとして。</span><span class="sxs-lookup"><span data-stu-id="62b86-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="62b86-141">最上位のプリミティブ型をシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="62b86-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="62b86-142">BSON のすべてのドキュメントでは、キー/値ペアの一覧を示します。BSON 仕様では、整数や文字列などの単一の raw 値をシリアル化するための構文は定義されていません。</span><span class="sxs-lookup"><span data-stu-id="62b86-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="62b86-143">このような制限を回避する、 **BsonMediaTypeFormatter**は特殊なケースとしてプリミティブ型を処理します。</span><span class="sxs-lookup"><span data-stu-id="62b86-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="62b86-144">をシリアル化する前に、値に変換キー/値ペア"Value"キーにします。</span><span class="sxs-lookup"><span data-stu-id="62b86-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="62b86-145">たとえば、API コント ローラーは、整数を返します。</span><span class="sxs-lookup"><span data-stu-id="62b86-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="62b86-146">をシリアル化する前に BSON フォーマッタは、これを次のキー/値ペアに変換します。</span><span class="sxs-lookup"><span data-stu-id="62b86-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="62b86-147">逆シリアル化するときに、フォーマッタは、元の値にデータを変換します。</span><span class="sxs-lookup"><span data-stu-id="62b86-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="62b86-148">ただし、異なる BSON パーサーを使用するクライアントは、web API は、生の値を返す場合、このケースを処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="62b86-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="62b86-149">一般に、生の値ではなく、構造化データを返すことを考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="62b86-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="62b86-150">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="62b86-150">Additional Resources</span></span>

[<span data-ttu-id="62b86-151">Web API BSON サンプル</span><span class="sxs-lookup"><span data-stu-id="62b86-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="62b86-152">メディア フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="62b86-152">Media Formatters</span></span>](media-formatters.md)
