---
title: "ASP.NET core 圧縮ミドルウェアの応答"
author: guardrex
description: "応答の圧縮と圧縮ミドルウェアの応答を ASP.NET Core アプリケーションで使用する方法について説明します。"
keywords: "ASP.NET Core、パフォーマンス、応答の圧縮、gzip、受け入れるエンコード、ミドルウェア"
ms.author: riande
manager: wpickett
ms.date: 08/20/2017
ms.topic: article
ms.assetid: de621887-c5c9-4ac8-9efd-f5cc0457a134
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/response-compression
ms.openlocfilehash: 68e8c89f6e5485f25d1a551ab3e524f0e9c53d0d
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/19/2017
---
# <a name="response-compression-middleware-for-aspnet-core"></a><span data-ttu-id="88462-104">ASP.NET core 圧縮ミドルウェアの応答</span><span class="sxs-lookup"><span data-stu-id="88462-104">Response Compression Middleware for ASP.NET Core</span></span>

<span data-ttu-id="88462-105">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="88462-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="88462-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="88462-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="88462-107">ネットワーク帯域幅は、限られたリソースです。</span><span class="sxs-lookup"><span data-stu-id="88462-107">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="88462-108">通常の応答のサイズを小さくする、アプリの応答性を大幅に多くの場合、増加します。</span><span class="sxs-lookup"><span data-stu-id="88462-108">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="88462-109">ペイロードのサイズを小さく 1 つの方法では、圧縮、アプリの応答です。</span><span class="sxs-lookup"><span data-stu-id="88462-109">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="88462-110">応答の圧縮のミドルウェアを使用する場合</span><span class="sxs-lookup"><span data-stu-id="88462-110">When to use Response Compression Middleware</span></span>
<span data-ttu-id="88462-111">IIS、Apache、または Nginx サーバー ベースの応答の圧縮テクノロジを使用します。</span><span class="sxs-lookup"><span data-stu-id="88462-111">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="88462-112">ミドルウェアのパフォーマンス可能性がありますされませんと一致するサーバーにあるモジュール。</span><span class="sxs-lookup"><span data-stu-id="88462-112">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="88462-113">[HTTP.sys サーバー](xref:fundamentals/servers/httpsys)と[Kestrel](xref:fundamentals/servers/kestrel)現在組み込み圧縮サポートが提供されません。</span><span class="sxs-lookup"><span data-stu-id="88462-113">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="88462-114">場合は、圧縮ミドルウェアの応答を使用します。</span><span class="sxs-lookup"><span data-stu-id="88462-114">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="88462-115">次のサーバー ベースの圧縮テクノロジを使用することができません。</span><span class="sxs-lookup"><span data-stu-id="88462-115">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="88462-116">動的な圧縮の IIS モジュール</span><span class="sxs-lookup"><span data-stu-id="88462-116">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="88462-117">Apache mod_deflate モジュール</span><span class="sxs-lookup"><span data-stu-id="88462-117">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="88462-118">NGINX 圧縮および圧縮解除</span><span class="sxs-lookup"><span data-stu-id="88462-118">NGINX Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="88462-119">直接ホスティング。</span><span class="sxs-lookup"><span data-stu-id="88462-119">Hosting directly on:</span></span>
  * <span data-ttu-id="88462-120">[HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (旧称[WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="88462-120">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="88462-121">Kestrel</span><span class="sxs-lookup"><span data-stu-id="88462-121">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="88462-122">応答の圧縮</span><span class="sxs-lookup"><span data-stu-id="88462-122">Response compression</span></span>
<span data-ttu-id="88462-123">通常は、ネイティブに圧縮された応答は、応答の圧縮を利用できます。</span><span class="sxs-lookup"><span data-stu-id="88462-123">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="88462-124">通常ネイティブに圧縮された応答が含まれます: CSS、JavaScript、HTML、XML、および JSON。</span><span class="sxs-lookup"><span data-stu-id="88462-124">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="88462-125">PNG ファイルなどのネイティブ圧縮されたアセットを圧縮することはできません。</span><span class="sxs-lookup"><span data-stu-id="88462-125">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="88462-126">ネイティブ圧縮された応答をさらに圧縮するしようとすると、サイズと転送時間の小さな追加低下は可能性がありますする影が薄くなって、圧縮処理にかかった時間によってです。</span><span class="sxs-lookup"><span data-stu-id="88462-126">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="88462-127">(ファイルの内容および圧縮の効率性) に応じて約 150 ~ 1000 バイト未満のファイルを圧縮されていません。</span><span class="sxs-lookup"><span data-stu-id="88462-127">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="88462-128">圧縮されたファイルのサイズが圧縮されていないファイルよりも小さいファイルの圧縮のオーバーヘッドがあります。</span><span class="sxs-lookup"><span data-stu-id="88462-128">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="88462-129">送信することによってその機能のサーバーにクライアントを通知する必要があります、クライアントは、圧縮されたコンテンツを処理できますが、ときに、`Accept-Encoding`要求でヘッダー。</span><span class="sxs-lookup"><span data-stu-id="88462-129">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="88462-130">内の情報を含めることは、サーバーは、圧縮されたコンテンツを送信するとき、`Content-Encoding`圧縮された応答をエンコードする方法のヘッダー。</span><span class="sxs-lookup"><span data-stu-id="88462-130">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="88462-131">ミドルウェアによってサポートされているコンテンツのエンコードの指定は、次の表に示します。</span><span class="sxs-lookup"><span data-stu-id="88462-131">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="88462-132">`Accept-Encoding`ヘッダーの値</span><span class="sxs-lookup"><span data-stu-id="88462-132">`Accept-Encoding` header values</span></span> | <span data-ttu-id="88462-133">ミドルウェアのサポート</span><span class="sxs-lookup"><span data-stu-id="88462-133">Middleware Supported</span></span> | <span data-ttu-id="88462-134">説明</span><span class="sxs-lookup"><span data-stu-id="88462-134">Description</span></span>                                                 |
| :-----------------------------: | :------------------: | ----------------------------------------------------------- |
| `br`                            | <span data-ttu-id="88462-135">×</span><span class="sxs-lookup"><span data-stu-id="88462-135">No</span></span>                   | <span data-ttu-id="88462-136">Brotli 圧縮データの形式</span><span class="sxs-lookup"><span data-stu-id="88462-136">Brotli Compressed Data Format</span></span>                               |
| `compress`                      | <span data-ttu-id="88462-137">×</span><span class="sxs-lookup"><span data-stu-id="88462-137">No</span></span>                   | <span data-ttu-id="88462-138">UNIX"を圧縮"データの形式</span><span class="sxs-lookup"><span data-stu-id="88462-138">UNIX "compress" data format</span></span>                                 |
| `deflate`                       | <span data-ttu-id="88462-139">×</span><span class="sxs-lookup"><span data-stu-id="88462-139">No</span></span>                   | <span data-ttu-id="88462-140">データの形式が"zlib"内のデータを圧縮する"deflate"</span><span class="sxs-lookup"><span data-stu-id="88462-140">"deflate" compressed data inside the "zlib" data format</span></span>     |
| `exi`                           | <span data-ttu-id="88462-141">×</span><span class="sxs-lookup"><span data-stu-id="88462-141">No</span></span>                   | <span data-ttu-id="88462-142">W3C の効率的な XML インターチェンジ</span><span class="sxs-lookup"><span data-stu-id="88462-142">W3C Efficient XML Interchange</span></span>                               |
| `gzip`                          | <span data-ttu-id="88462-143">[はい] (既定値)</span><span class="sxs-lookup"><span data-stu-id="88462-143">Yes (default)</span></span>        | <span data-ttu-id="88462-144">gzip ファイル形式</span><span class="sxs-lookup"><span data-stu-id="88462-144">gzip file format</span></span>                                            |
| `identity`                      | <span data-ttu-id="88462-145">[はい]</span><span class="sxs-lookup"><span data-stu-id="88462-145">Yes</span></span>                  | <span data-ttu-id="88462-146">「エンコードなし」の識別子: 応答をエンコードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="88462-146">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="88462-147">×</span><span class="sxs-lookup"><span data-stu-id="88462-147">No</span></span>                   | <span data-ttu-id="88462-148">ネットワーク転送形式は Java アーカイブ用</span><span class="sxs-lookup"><span data-stu-id="88462-148">Network Transfer Format for Java Archives</span></span>                   |
| `*`                             | <span data-ttu-id="88462-149">[はい]</span><span class="sxs-lookup"><span data-stu-id="88462-149">Yes</span></span>                  | <span data-ttu-id="88462-150">エンコードは明示的に要求された使用可能なコンテンツ</span><span class="sxs-lookup"><span data-stu-id="88462-150">Any available content encoding not explicitly requested</span></span>     |

<span data-ttu-id="88462-151">詳細については、次を参照してください。、 [IANA 公式コンテンツ コーディング リスト](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)です。</span><span class="sxs-lookup"><span data-stu-id="88462-151">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="88462-152">ミドルウェアでは、カスタムの圧縮は追加のプロバイダーを追加することができます`Accept-Encoding`ヘッダー値。</span><span class="sxs-lookup"><span data-stu-id="88462-152">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="88462-153">詳細については、次を参照してください。[カスタム プロバイダー](#custom-providers)以下です。</span><span class="sxs-lookup"><span data-stu-id="88462-153">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="88462-154">ミドルウェアは品質の値に対応できる (qvalue、 `q`) 圧縮スキームの優先順位をクライアントによって送信されるときに重み付け。</span><span class="sxs-lookup"><span data-stu-id="88462-154">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="88462-155">詳細については、次を参照してください。 [RFC 7231: Accept-encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4)です。</span><span class="sxs-lookup"><span data-stu-id="88462-155">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="88462-156">圧縮アルゴリズムは、圧縮の速度と、圧縮の有効性のバランスが適用されます。</span><span class="sxs-lookup"><span data-stu-id="88462-156">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="88462-157">*有効性*このコンテキストでは、出力のサイズを圧縮した後です。</span><span class="sxs-lookup"><span data-stu-id="88462-157">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="88462-158">最小サイズは最もによって実現*最適な*圧縮します。</span><span class="sxs-lookup"><span data-stu-id="88462-158">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="88462-159">要求に関連するヘッダーを送信する、キャッシュ、および圧縮されたコンテンツを受信は、次の表に記述されています。</span><span class="sxs-lookup"><span data-stu-id="88462-159">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="88462-160">Header</span><span class="sxs-lookup"><span data-stu-id="88462-160">Header</span></span>             | <span data-ttu-id="88462-161">ロール</span><span class="sxs-lookup"><span data-stu-id="88462-161">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="88462-162">コンテンツのエンコードにクライアントに許容されるスキームを示すために、クライアントからサーバーに送信します。</span><span class="sxs-lookup"><span data-stu-id="88462-162">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="88462-163">ペイロード内のコンテンツのエンコードを示すために、サーバーからクライアントに送信します。</span><span class="sxs-lookup"><span data-stu-id="88462-163">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="88462-164">圧縮が発生すると、`Content-Length`本文内容が変更された応答が圧縮される場合に以降のヘッダーは削除します。</span><span class="sxs-lookup"><span data-stu-id="88462-164">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="88462-165">圧縮が発生すると、`Content-MD5`本文の内容が変更されており、ハッシュが無効になったため、ヘッダーが削除されます。</span><span class="sxs-lookup"><span data-stu-id="88462-165">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="88462-166">コンテンツの MIME の種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="88462-166">Specifies the MIME type of the content.</span></span> <span data-ttu-id="88462-167">すべての応答を指定する必要があります、`Content-Type`です。</span><span class="sxs-lookup"><span data-stu-id="88462-167">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="88462-168">ミドルウェアは、応答を圧縮する必要があるかどうかを決定するには、この値を確認します。</span><span class="sxs-lookup"><span data-stu-id="88462-168">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="88462-169">ミドルウェアのセットを指定する[既定の MIME の種類](#mime-types)をエンコードできますが、置き換えるか、MIME の種類を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="88462-169">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="88462-170">値を使用してサーバーによって送信されると`Accept-Encoding`クライアントと、プロキシを`Vary`ヘッダーがクライアントまたはキャッシュするプロキシを示します (異なります) の値に基づいて、応答、`Accept-Encoding`要求のヘッダー。</span><span class="sxs-lookup"><span data-stu-id="88462-170">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="88462-171">コンテンツを返すことの結果、`Vary: Accept-Encoding`ヘッダーは、両方は圧縮され、圧縮されていない応答を個別にキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="88462-171">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="88462-172">応答の圧縮のミドルウェアの機能を調べることができます、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples)です。</span><span class="sxs-lookup"><span data-stu-id="88462-172">You can explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="88462-173">このサンプルを示しています。</span><span class="sxs-lookup"><span data-stu-id="88462-173">The sample illustrates:</span></span>
* <span data-ttu-id="88462-174">Gzip および圧縮のカスタム プロバイダーを使用してアプリの応答の圧縮します。</span><span class="sxs-lookup"><span data-stu-id="88462-174">The compression of app responses using gzip and custom compression providers.</span></span>
* <span data-ttu-id="88462-175">圧縮の MIME の種類の既定のリストに、MIME の種類を追加する方法です。</span><span class="sxs-lookup"><span data-stu-id="88462-175">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="88462-176">Package</span><span class="sxs-lookup"><span data-stu-id="88462-176">Package</span></span>
<span data-ttu-id="88462-177">ミドルウェアをプロジェクトに含めるへの参照を追加、 [ `Microsoft.AspNetCore.ResponseCompression` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)パッケージまたはを使用して、 [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="88462-177">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.ResponseCompression`](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [`Microsoft.AspNetCore.All`](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) package.</span></span> <span data-ttu-id="88462-178">この機能は、ASP.NET Core 1.1 を対象とするアプリの使用可能な以降です。</span><span class="sxs-lookup"><span data-stu-id="88462-178">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="configuration"></a><span data-ttu-id="88462-179">構成</span><span class="sxs-lookup"><span data-stu-id="88462-179">Configuration</span></span>
<span data-ttu-id="88462-180">次のコードと応答の圧縮のミドルウェアを有効にする方法を示しています、および既定の MIME の種類の既定 gzip 圧縮を使用します。</span><span class="sxs-lookup"><span data-stu-id="88462-180">The following code shows how to enable the Response Compression Middleware with the with the default gzip compression and for default MIME types.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="88462-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="88462-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/StartupBasic.cs?name=snippet1&highlight=4,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="88462-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="88462-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/StartupBasic.cs?name=snippet1&highlight=3,8)]

---

> [!NOTE]
> <span data-ttu-id="88462-183">などのツールを使用して[Fiddler](http://www.telerik.com/fiddler)、 [Firebug](http://getfirebug.com/)、または[Postman](https://www.getpostman.com/)を設定する、`Accept-Encoding`要求ヘッダーおよび応答ヘッダー、サイズ、および本文を調査します。</span><span class="sxs-lookup"><span data-stu-id="88462-183">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="88462-184">サンプル アプリをせずに要求を送信、`Accept-Encoding`ヘッダー応答は圧縮されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="88462-184">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="88462-185">`Content-Encoding`と`Vary`ヘッダーが応答に表示されません。</span><span class="sxs-lookup"><span data-stu-id="88462-185">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Fiddler のウィンドウが、要求に Accept-encoding ヘッダーなしの結果を示すです。](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="88462-188">要求のサンプル アプリを送信、`Accept-Encoding: gzip`ヘッダー応答が圧縮されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="88462-188">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="88462-189">`Content-Encoding`と`Vary`ヘッダーが応答に存在します。</span><span class="sxs-lookup"><span data-stu-id="88462-189">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler の Accept-encoding ヘッダーで要求の結果と gzip の値を示すウィンドウです。](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="88462-193">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="88462-193">Providers</span></span>
### <a name="gzipcompressionprovider"></a><span data-ttu-id="88462-194">GzipCompressionProvider</span><span class="sxs-lookup"><span data-stu-id="88462-194">GzipCompressionProvider</span></span>
<span data-ttu-id="88462-195">使用して、`GzipCompressionProvider`に gzip で応答を圧縮します。</span><span class="sxs-lookup"><span data-stu-id="88462-195">Use the `GzipCompressionProvider` to compress responses with gzip.</span></span> <span data-ttu-id="88462-196">これは、指定がない場合、圧縮の既定のプロバイダーです。</span><span class="sxs-lookup"><span data-stu-id="88462-196">This is the default compression provider if none are specified.</span></span> <span data-ttu-id="88462-197">レベルを圧縮を設定することができます、`GzipCompressionProviderOptions`です。</span><span class="sxs-lookup"><span data-stu-id="88462-197">You can set the compression level with the `GzipCompressionProviderOptions`.</span></span> 

<span data-ttu-id="88462-198">Gzip 圧縮プロバイダーの既定値は、最速の圧縮レベル (`CompressionLevel.Fastest`)、これが最も効率的な圧縮を得られない。</span><span class="sxs-lookup"><span data-stu-id="88462-198">The gzip compression provider defaults to the fastest compression level (`CompressionLevel.Fastest`), which might not produce the most efficient compression.</span></span> <span data-ttu-id="88462-199">最も効率的な圧縮を使用する場合は、最適な圧縮のミドルウェアを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="88462-199">If the most efficient compression is desired, you can configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="88462-200">圧縮レベル</span><span class="sxs-lookup"><span data-stu-id="88462-200">Compression Level</span></span>                | <span data-ttu-id="88462-201">説明</span><span class="sxs-lookup"><span data-stu-id="88462-201">Description</span></span>                                                                                                   |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `CompressionLevel.Fastest`       | <span data-ttu-id="88462-202">結果の出力が最適に圧縮されていない場合でも、圧縮は、できるだけ早く完了します。</span><span class="sxs-lookup"><span data-stu-id="88462-202">Compression should complete as quickly as possible, even if the resulting output is not optimally compressed.</span></span> |
| `CompressionLevel.NoCompression` | <span data-ttu-id="88462-203">圧縮は行われません。</span><span class="sxs-lookup"><span data-stu-id="88462-203">No compression should be performed.</span></span>                                                                           |
| `CompressionLevel.Optimal`       | <span data-ttu-id="88462-204">応答を最適に圧縮する、場合でも、圧縮を完了に時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="88462-204">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span>                |


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="88462-205">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="88462-205">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=3,8-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="88462-206">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="88462-206">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,10-13)]

---

## <a name="mime-types"></a><span data-ttu-id="88462-207">MIME タイプ</span><span class="sxs-lookup"><span data-stu-id="88462-207">MIME types</span></span>
<span data-ttu-id="88462-208">ミドルウェアは、圧縮の MIME の種類の既定のセットを指定します。</span><span class="sxs-lookup"><span data-stu-id="88462-208">The middleware specifies a default set of MIME types for compression:</span></span>
* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

<span data-ttu-id="88462-209">置き換えるか、応答の圧縮のミドルウェアのオプションで MIME の種類を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="88462-209">You can replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="88462-210">そのワイルドカード MIME に注意してくださいなどの型`text/*`はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="88462-210">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="88462-211">サンプル アプリの MIME の種類を追加する`image/svg+xml`と圧縮および ASP.NET Core バナー イメージの機能 (*banner.svg*)。</span><span class="sxs-lookup"><span data-stu-id="88462-211">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="88462-212">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="88462-212">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="88462-213">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="88462-213">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7)]

---

### <a name="custom-providers"></a><span data-ttu-id="88462-214">カスタム プロバイダー</span><span class="sxs-lookup"><span data-stu-id="88462-214">Custom providers</span></span>
<span data-ttu-id="88462-215">カスタムの圧縮の実装を作成する`ICompressionProvider`です。</span><span class="sxs-lookup"><span data-stu-id="88462-215">You can create custom compression implementations with `ICompressionProvider`.</span></span> <span data-ttu-id="88462-216">`EncodingName`コンテンツのエンコードにこれを表す`ICompressionProvider`が生成されます。</span><span class="sxs-lookup"><span data-stu-id="88462-216">The `EncodingName` represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="88462-217">ミドルウェアでは、この情報を使用して、指定されたリストに基づくプロバイダーを選択して、`Accept-Encoding`要求のヘッダー。</span><span class="sxs-lookup"><span data-stu-id="88462-217">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="88462-218">サンプル アプリを使用して、クライアント要求を送信すると、`Accept-Encoding: mycustomcompression`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="88462-218">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="88462-219">ミドルウェアがカスタム圧縮の実装を使用し、応答を返し、`Content-Encoding: mycustomcompression`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="88462-219">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="88462-220">クライアントは、カスタムの圧縮の実装が機能するためにはカスタム エンコーディングを圧縮解除できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="88462-220">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="88462-221">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="88462-221">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=4)]

[!code-csharp[Main](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="88462-222">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="88462-222">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[Main](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

<span data-ttu-id="88462-223">サンプル アプリケーションに要求を送信、`Accept-Encoding: mycustomcompression`ヘッダーおよび応答ヘッダーを確認します。</span><span class="sxs-lookup"><span data-stu-id="88462-223">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="88462-224">`Vary`と`Content-Encoding`ヘッダーが応答に存在します。</span><span class="sxs-lookup"><span data-stu-id="88462-224">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="88462-225">(非表示)、応答本文は、このサンプルで圧縮されていません。</span><span class="sxs-lookup"><span data-stu-id="88462-225">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="88462-226">圧縮の実装ではありません、`CustomCompressionProvider`サンプルのクラスです。</span><span class="sxs-lookup"><span data-stu-id="88462-226">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="88462-227">ただし、このような圧縮アルゴリズムを実装するかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="88462-227">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fiddler の Accept-encoding ヘッダーで要求の結果と mycustomcompression の値を示すウィンドウです。](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="88462-230">セキュリティで保護されたプロトコルと圧縮</span><span class="sxs-lookup"><span data-stu-id="88462-230">Compression with secure protocol</span></span>
<span data-ttu-id="88462-231">セキュリティで保護された接続上で圧縮された応答を制御することができます、`EnableForHttps`オプションは、既定で無効にします。</span><span class="sxs-lookup"><span data-stu-id="88462-231">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="88462-232">動的に生成されたページの圧縮を使用してが問題につながるセキュリティなど、 [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit))と[侵害](https://wikipedia.org/wiki/BREACH_(security_exploit))攻撃です。</span><span class="sxs-lookup"><span data-stu-id="88462-232">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="88462-233">Vary ヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="88462-233">Adding the Vary header</span></span>
<span data-ttu-id="88462-234">基づいている場合の応答の圧縮、`Accept-Encoding`ヘッダー、可能性のある複数の圧縮バージョン応答と圧縮されていないバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="88462-234">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="88462-235">複数のバージョンが存在しを保存するか、クライアントとプロキシのキャッシュを指示するために、`Vary`ヘッダーを追加、`Accept-Encoding`値。</span><span class="sxs-lookup"><span data-stu-id="88462-235">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="88462-236">ASP.NET Core で 1.x では、追加、`Vary`を応答にヘッダーを手動で行います。</span><span class="sxs-lookup"><span data-stu-id="88462-236">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually.</span></span> <span data-ttu-id="88462-237">ASP.NET Core で 2.x、ミドルウェアを追加、`Vary`ヘッダー応答が圧縮されるときに自動的にします。</span><span class="sxs-lookup"><span data-stu-id="88462-237">In ASP.NET Core 2.x, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

<span data-ttu-id="88462-238">**ASP.NET Core 1.x のみ**</span><span class="sxs-lookup"><span data-stu-id="88462-238">**ASP.NET Core 1.x only**</span></span>

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet1)]

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="88462-239">Nginx リバース プロキシの背後にある場合のミドルウェアの問題</span><span class="sxs-lookup"><span data-stu-id="88462-239">Middleware issue when behind an Nginx reverse-proxy</span></span>
<span data-ttu-id="88462-240">要求が Nginx がプロキシとなったとき、`Accept-Encoding`ヘッダーを削除します。</span><span class="sxs-lookup"><span data-stu-id="88462-240">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="88462-241">これは、ミドルウェアが応答を圧縮することを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="88462-241">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="88462-242">詳細については、次を参照してください。 [NGINX: 圧縮および圧縮解除](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)です。</span><span class="sxs-lookup"><span data-stu-id="88462-242">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="88462-243">によってこの問題を追跡[nginx (BasicMiddleware #123) の圧縮をパススルーもらう](https://github.com/aspnet/BasicMiddleware/issues/123)です。</span><span class="sxs-lookup"><span data-stu-id="88462-243">This issue is tracked by [Figure out pass-through compression for nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="88462-244">IIS 動的圧縮を使用</span><span class="sxs-lookup"><span data-stu-id="88462-244">Working with IIS dynamic compression</span></span>
<span data-ttu-id="88462-245">モジュールがある場合、アクティブな IIS 動的圧縮に対してアプリを無効にするにはサーバー レベルで構成されている場合、監視できるように、追加すると、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="88462-245">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, you can do so with an addition to your *web.config* file.</span></span> <span data-ttu-id="88462-246">詳細については、次を参照してください。[を無効にする IIS モジュール](xref:hosting/iis-modules#disabling-iis-modules)です。</span><span class="sxs-lookup"><span data-stu-id="88462-246">For more information, see [Disabling IIS modules](xref:hosting/iis-modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="88462-247">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="88462-247">Troubleshooting</span></span>
<span data-ttu-id="88462-248">などのツールを使用して[Fiddler](http://www.telerik.com/fiddler)、 [Firebug](http://getfirebug.com/)、または[Postman](https://www.getpostman.com/)を設定することを`Accept-Encoding`要求ヘッダーおよび応答ヘッダー、サイズ、および本文を調査します。</span><span class="sxs-lookup"><span data-stu-id="88462-248">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="88462-249">応答の圧縮のミドルウェアが圧縮を次の条件を満たす応答。</span><span class="sxs-lookup"><span data-stu-id="88462-249">The Response Compression Middleware compresses responses that meet the following conditions:</span></span>
* <span data-ttu-id="88462-250">`Accept-Encoding`ヘッダーがあり、値は存在`gzip`、 `*`、または確立したユーザー圧縮プロバイダーに一致するカスタム エンコードします。</span><span class="sxs-lookup"><span data-stu-id="88462-250">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="88462-251">値にはできません`identity`または品質の値が (qvalue、 `q`) を 0 (ゼロ) を設定します。</span><span class="sxs-lookup"><span data-stu-id="88462-251">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="88462-252">MIME の種類 (`Content-Type`) が設定されで構成されている MIME の種類に一致する必要があります、`ResponseCompressionOptions`です。</span><span class="sxs-lookup"><span data-stu-id="88462-252">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the `ResponseCompressionOptions`.</span></span>
* <span data-ttu-id="88462-253">要求を含めないでください、`Content-Range`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="88462-253">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="88462-254">要求は、安全なプロトコル (https) が応答の圧縮のミドルウェアのオプションで構成されている場合を除き、安全ではないプロトコル (http) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88462-254">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="88462-255">*危険があることに注意してください[上記](#compression-with-secure-protocol)セキュリティで保護されたコンテンツの圧縮を有効にする場合。*</span><span class="sxs-lookup"><span data-stu-id="88462-255">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="88462-256">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="88462-256">Additional resources</span></span>
* [<span data-ttu-id="88462-257">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="88462-257">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="88462-258">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="88462-258">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="88462-259">Mozilla Developer Network: Accept-encoding</span><span class="sxs-lookup"><span data-stu-id="88462-259">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="88462-260">RFC 7231 セクション 3.1.2.1: コンテンツのコーディング</span><span class="sxs-lookup"><span data-stu-id="88462-260">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="88462-261">RFC 7230 4.2.3: Gzip のコーディング</span><span class="sxs-lookup"><span data-stu-id="88462-261">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="88462-262">GZIP ファイル形式仕様バージョン 4.3</span><span class="sxs-lookup"><span data-stu-id="88462-262">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
