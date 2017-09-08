---
title: "応答のキャッシュ"
author: rick-anderson
description: "応答の帯域幅を削減し、パフォーマンスを向上させるキャッシュの使用方法について説明します。"
keywords: "ASP.NET Core、応答の HTTP ヘッダーをキャッシュします。"
ms.author: riande
manager: wpickett
ms.date: 7/10/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 97ca23d48763a96da917c993feb8aadebb450ab5
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="response-caching"></a><span data-ttu-id="d23f1-104">応答のキャッシュ</span><span class="sxs-lookup"><span data-stu-id="d23f1-104">Response Caching</span></span>

<span data-ttu-id="d23f1-105">によって[John Luo](https://github.com/JunTaoLuo)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、および[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="d23f1-105">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Steve Smith](http://ardalis.com)</span></span>

[<span data-ttu-id="d23f1-106">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="d23f1-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

## <a name="what-is-response-caching"></a><span data-ttu-id="d23f1-107">応答のキャッシュとは</span><span class="sxs-lookup"><span data-stu-id="d23f1-107">What is Response Caching</span></span>

<span data-ttu-id="d23f1-108">*応答のキャッシュ*応答をキャッシュに関連するヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="d23f1-108">*Response caching* adds cache-related headers to responses.</span></span> <span data-ttu-id="d23f1-109">これらのヘッダーは、クライアント、プロキシとミドルウェアが応答をキャッシュする方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="d23f1-109">These headers specify how you want client, proxy and middleware to cache responses.</span></span> <span data-ttu-id="d23f1-110">応答のキャッシュ クライアントまたはプロキシ web サーバーに、要求の数を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-110">Response caching can reduce the number of requests a client or proxy makes to the web server.</span></span> <span data-ttu-id="d23f1-111">応答のキャッシュ量を減らすことができますも作業の web サーバーを実行して応答を生成します。</span><span class="sxs-lookup"><span data-stu-id="d23f1-111">Response caching can also reduce the amount of work the web server performs to generate the response.</span></span> 

<span data-ttu-id="d23f1-112">キャッシュに使用するプライマリ HTTP ヘッダーは`Cache-Control`します。</span><span class="sxs-lookup"><span data-stu-id="d23f1-112">The primary HTTP header used for caching is `Cache-Control`.</span></span> <span data-ttu-id="d23f1-113">参照してください、 [HTTP 1.1 キャッシュ](https://tools.ietf.org/html/rfc7234#section-5.2)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="d23f1-113">See the [HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234#section-5.2) for more information.</span></span> <span data-ttu-id="d23f1-114">一般的なキャッシュのディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="d23f1-114">Common cache directives:</span></span>

* [<span data-ttu-id="d23f1-115">public</span><span class="sxs-lookup"><span data-stu-id="d23f1-115">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)
* [<span data-ttu-id="d23f1-116">private</span><span class="sxs-lookup"><span data-stu-id="d23f1-116">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)
* [<span data-ttu-id="d23f1-117">キャッシュなし</span><span class="sxs-lookup"><span data-stu-id="d23f1-117">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)
* [<span data-ttu-id="d23f1-118">プラグマ</span><span class="sxs-lookup"><span data-stu-id="d23f1-118">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)
* [<span data-ttu-id="d23f1-119">異なる</span><span class="sxs-lookup"><span data-stu-id="d23f1-119">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)

<span data-ttu-id="d23f1-120">Web サーバーは、応答のミドルウェアのキャッシュを追加することで応答をキャッシュできます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-120">The web server can cache responses by adding the response caching middleware.</span></span> <span data-ttu-id="d23f1-121">参照してください[応答のミドルウェアをキャッシュ](middleware.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="d23f1-121">See [Response caching middleware](middleware.md) for more information.</span></span>

## <a name="distributed-cache-tag-helper"></a><span data-ttu-id="d23f1-122">分散キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="d23f1-122">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="d23f1-123">[分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)有効分散キャッシュします。</span><span class="sxs-lookup"><span data-stu-id="d23f1-123">The [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) enables distributed caching.</span></span>


## <a name="responsecache-attribute"></a><span data-ttu-id="d23f1-124">ResponseCache 属性</span><span class="sxs-lookup"><span data-stu-id="d23f1-124">ResponseCache Attribute</span></span>

<span data-ttu-id="d23f1-125">`ResponseCacheAttribute`応答のキャッシュでは、適切なヘッダーを設定するために必要なパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="d23f1-125">The `ResponseCacheAttribute` specifies the parameters necessary for setting appropriate headers in response caching.</span></span> <span data-ttu-id="d23f1-126">参照してください[ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute)パラメーターの詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="d23f1-126">See [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute)  for a description of the parameters.</span></span>

>[!WARNING]
> <span data-ttu-id="d23f1-127">認証されたクライアントに関する情報を含むコンテンツのキャッシュを無効にします。</span><span class="sxs-lookup"><span data-stu-id="d23f1-127">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="d23f1-128">ユーザーの id に基づいて、コンテンツが変化しない、または、ユーザーがログインしているかどうか、キャッシュを有効のみ必要があります。</span><span class="sxs-lookup"><span data-stu-id="d23f1-128">Caching should only be enabled for content that does not change based on a user's identity, or whether a user is logged in.</span></span>

<span data-ttu-id="d23f1-129">`VaryByQueryKeys string[]`(ASP.NET Core 1.1.0 を必要と以降): 設定すると、応答のキャッシュ ミドルウェアによって異なります。 ストアド応答クエリ キーの指定された一覧の値。</span><span class="sxs-lookup"><span data-stu-id="d23f1-129">`VaryByQueryKeys string[]` (requires ASP.NET Core 1.1.0 and higher): When set, the response caching middleware will vary the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="d23f1-130">設定するには、応答のミドルウェアのキャッシュを有効にする必要があります、`VaryByQueryKeys`プロパティ、それ以外の場合、ランタイム例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-130">The response caching middleware must be enabled to set the `VaryByQueryKeys` property, otherwise a runtime exception will be thrown.</span></span> <span data-ttu-id="d23f1-131">対応する HTTP ヘッダーがない、`VaryByQueryKeys`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="d23f1-131">There is no corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="d23f1-132">このプロパティは、応答のミドルウェアのキャッシュによって処理される HTTP 機能です。</span><span class="sxs-lookup"><span data-stu-id="d23f1-132">This property is an HTTP feature handled by the response caching middleware.</span></span> <span data-ttu-id="d23f1-133">キャッシュされた応答を提供するミドルウェアをクエリ文字列とクエリ文字列の値必要がありますと一致前の要求。</span><span class="sxs-lookup"><span data-stu-id="d23f1-133">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="d23f1-134">たとえば、次のシーケンスがあるとします。</span><span class="sxs-lookup"><span data-stu-id="d23f1-134">For example, consider the following sequence:</span></span>

| <span data-ttu-id="d23f1-135">要求</span><span class="sxs-lookup"><span data-stu-id="d23f1-135">Request</span></span>          | <span data-ttu-id="d23f1-136">結果</span><span class="sxs-lookup"><span data-stu-id="d23f1-136">Result</span></span> |
| ----------------- | ------------ | 
| `http://example.com?key1=value1` | <span data-ttu-id="d23f1-137">サーバーから返される</span><span class="sxs-lookup"><span data-stu-id="d23f1-137">returned from server</span></span> |
| `http://example.com?key1=value1` | <span data-ttu-id="d23f1-138">ミドルウェアから返される</span><span class="sxs-lookup"><span data-stu-id="d23f1-138">returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="d23f1-139">サーバーから返される</span><span class="sxs-lookup"><span data-stu-id="d23f1-139">returned from server</span></span> |

<span data-ttu-id="d23f1-140">最初の要求がサーバーによって返され、ミドルウェア内でキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="d23f1-140">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="d23f1-141">2 番目の要求は、クエリ文字列が、前回の要求と一致するので、ミドルウェアによって返されます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-141">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="d23f1-142">3 番目の要求ではありません、ミドルウェア キャッシュにクエリ文字列の値は、前の要求と一致しません。</span><span class="sxs-lookup"><span data-stu-id="d23f1-142">The third request is not in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="d23f1-143">`ResponseCacheAttribute`構成を作成するために使用 (を介して`IFilterFactory`)、`ResponseCacheFilter`です。</span><span class="sxs-lookup"><span data-stu-id="d23f1-143">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a `ResponseCacheFilter`.</span></span> <span data-ttu-id="d23f1-144">`ResponseCacheFilter`適切な HTTP ヘッダーと応答の機能の更新の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="d23f1-144">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="d23f1-145">フィルター:</span><span class="sxs-lookup"><span data-stu-id="d23f1-145">The filter:</span></span>

* <span data-ttu-id="d23f1-146">既存のすべてのヘッダーを削除`Vary`、 `Cache-Control`、および`Pragma`です。</span><span class="sxs-lookup"><span data-stu-id="d23f1-146">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="d23f1-147">設定されたプロパティに基づいて、適切なヘッダーを書き込みます、`ResponseCacheAttribute`です。</span><span class="sxs-lookup"><span data-stu-id="d23f1-147">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="d23f1-148">更新プログラムの応答の場合は、HTTP 機能をキャッシュ`VaryByQueryKeys`が設定されています。</span><span class="sxs-lookup"><span data-stu-id="d23f1-148">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="d23f1-149">異なる</span><span class="sxs-lookup"><span data-stu-id="d23f1-149">Vary</span></span>

<span data-ttu-id="d23f1-150">このヘッダーがのみ書き込まれるときに、`VaryByHeader`プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="d23f1-150">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="d23f1-151">設定されている、`Vary`プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="d23f1-151">It is set to the `Vary` property's value.</span></span> <span data-ttu-id="d23f1-152">次のサンプルは、`VaryByHeader`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="d23f1-152">The following sample uses the `VaryByHeader` property.</span></span>

<span data-ttu-id="d23f1-153">[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="d23f1-153">[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]</span></span>

<span data-ttu-id="d23f1-154">応答ヘッダーは、ブラウザー、ネットワーク ツールを使用して表示できます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-154">You can view the response headers with your browsers network tools.</span></span> <span data-ttu-id="d23f1-155">次の図は出力エッジ F12、**ネットワーク** タブのときに、`About2`アクション メソッドが更新されます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-155">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed.</span></span> 

![F12 出力をエッジ、* * 'About2' アクション メソッドが呼び出されたときに * * ネットワーク タブ](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="d23f1-157">NoStore と Location.None</span><span class="sxs-lookup"><span data-stu-id="d23f1-157">NoStore and Location.None</span></span>

<span data-ttu-id="d23f1-158">`NoStore`ほとんどの他のプロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="d23f1-158">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="d23f1-159">このプロパティに設定するときに`true`、`Cache-Control`ヘッダーは"no store"に設定されます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-159">When this property is set to `true`, the `Cache-Control` header will be set to "no-store".</span></span> <span data-ttu-id="d23f1-160">場合`Location`に設定されている`None`:</span><span class="sxs-lookup"><span data-stu-id="d23f1-160">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="d23f1-161">`Cache-Control` が `"no-store, no-cache"` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-161">`Cache-Control` is set to `"no-store, no-cache"`.</span></span> 
* <span data-ttu-id="d23f1-162">`Pragma` が `no-cache` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-162">`Pragma` is set to `no-cache`.</span></span> 

<span data-ttu-id="d23f1-163">場合`NoStore`は`false`と`Location`は`None`、`Cache-Control`と`Pragma`に設定されます`no-cache`です。</span><span class="sxs-lookup"><span data-stu-id="d23f1-163">If `NoStore` is `false` and `Location` is `None`,  `Cache-Control` and `Pragma` will be set to `no-cache`.</span></span>

<span data-ttu-id="d23f1-164">通常は設定`NoStore`に`true`エラー ページにします。</span><span class="sxs-lookup"><span data-stu-id="d23f1-164">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="d23f1-165">例:</span><span class="sxs-lookup"><span data-stu-id="d23f1-165">For example:</span></span>

<span data-ttu-id="d23f1-166">[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="d23f1-166">[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]</span></span>

<span data-ttu-id="d23f1-167">これは、次のヘッダーで決定されます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-167">This will result in the following headers:</span></span>

```javascript
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="d23f1-168">位置と長さ</span><span class="sxs-lookup"><span data-stu-id="d23f1-168">Location and Duration</span></span>

<span data-ttu-id="d23f1-169">キャッシュを有効にする`Duration`正の値に設定する必要がありますと`Location`いずれかである必要があります`Any`(既定) または`Client`です。</span><span class="sxs-lookup"><span data-stu-id="d23f1-169">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="d23f1-170">ここで、`Cache-Control`ヘッダーは、場所の値の後の応答の"最大 age"に設定されます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-170">In this case, the `Cache-Control` header will be set to the location value followed by the "max-age" of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="d23f1-171">`Location`オプションの`Any`と`Client`変換`Cache-Control`のヘッダー値`public`と`private`、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="d23f1-171">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="d23f1-172">前述のように、設定`Location`に`None`両方を設定`Cache-Control`と`Pragma`ヘッダーを`no-cache`です。</span><span class="sxs-lookup"><span data-stu-id="d23f1-172">As noted previously, setting `Location` to `None` will set both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="d23f1-173">次のヘッダーを示す例によって生成される設定`Duration`し、既定のままにして`Location`値。</span><span class="sxs-lookup"><span data-stu-id="d23f1-173">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value.</span></span>

<span data-ttu-id="d23f1-174">[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="d23f1-174">[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]</span></span>

<span data-ttu-id="d23f1-175">次のヘッダーを生成します。</span><span class="sxs-lookup"><span data-stu-id="d23f1-175">Produces the following headers:</span></span>

```javascript
Cache-Control: public,max-age=60
   ```

### <a name="cache-profiles"></a><span data-ttu-id="d23f1-176">キャッシュ プロファイル</span><span class="sxs-lookup"><span data-stu-id="d23f1-176">Cache Profiles</span></span>

<span data-ttu-id="d23f1-177">複製ではなく`ResponseCache`で MVC を設定するとき、オプションとしてコント ローラー アクションの多くの属性でキャッシュ プロファイルの設定を構成できます、`ConfigureServices`メソッド`Startup`です。</span><span class="sxs-lookup"><span data-stu-id="d23f1-177">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="d23f1-178">参照されているキャッシュ プロファイル内の値によって既定値として使用される、`ResponseCache`属性があり、この属性で指定したプロパティによってオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-178">Values found in a referenced cache profile will be used as the defaults by the `ResponseCache` attribute, and will be overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="d23f1-179">キャッシュ プロファイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="d23f1-179">Setting up a cache profile:</span></span>

<span data-ttu-id="d23f1-180">[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d23f1-180">[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)]</span></span> 

<span data-ttu-id="d23f1-181">キャッシュ プロファイルを参照するには。</span><span class="sxs-lookup"><span data-stu-id="d23f1-181">Referencing a cache profile:</span></span>

<span data-ttu-id="d23f1-182">[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]</span><span class="sxs-lookup"><span data-stu-id="d23f1-182">[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]</span></span>

<span data-ttu-id="d23f1-183">`ResponseCache` (メソッド) のアクションとコント ローラー (クラス) の両方に属性を適用できます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-183">The `ResponseCache` attribute can be applied both to actions (methods) as well as controllers (classes).</span></span> <span data-ttu-id="d23f1-184">メソッド レベルの属性は、クラス レベルの属性で指定された設定で上書きされます。</span><span class="sxs-lookup"><span data-stu-id="d23f1-184">Method-level attributes will override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="d23f1-185">上記の例では、クラス レベルの属性は、メソッド レベルの属性は、継続時間が 60 秒に設定するキャッシュ プロファイルを参照中に 30 秒の長さを指定します。</span><span class="sxs-lookup"><span data-stu-id="d23f1-185">In the above example, a class-level attribute specifies a duration of 30 seconds while a method-level attributes references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="d23f1-186">結果として得られるヘッダー。</span><span class="sxs-lookup"><span data-stu-id="d23f1-186">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
   ```

  ### <a name="additional-resources"></a><span data-ttu-id="d23f1-187">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="d23f1-187">Additional Resources</span></span>

* [<span data-ttu-id="d23f1-188">指定から HTTP でのキャッシュ</span><span class="sxs-lookup"><span data-stu-id="d23f1-188">Caching in HTTP from the specification</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="d23f1-189">キャッシュ制御</span><span class="sxs-lookup"><span data-stu-id="d23f1-189">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
