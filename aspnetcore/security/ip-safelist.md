---
title: ASP.NET Core 用のクライアント IP のセーフリスト
author: damienbod
description: 承認済みの IP アドレスの一覧に対して、リモートの IP アドレスを検証するミドルウェアまたはアクションのフィルターを作成する方法について説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 286f199c0d9164fa70d511aba523210c85c2fdfd
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207733"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="48e57-103">ASP.NET Core 用のクライアント IP のセーフリスト</span><span class="sxs-lookup"><span data-stu-id="48e57-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="48e57-104">によって[Damien Bowden](https://twitter.com/damien_bod)と[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="48e57-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="48e57-105">この記事では、ASP.NET Core アプリで IP セーフリスト (ホワイト リストとも呼ばれます) を実装するために 3 つの方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="48e57-105">This article shows three ways to implement an IP safelist (also known as a whitelist) in an ASP.NET Core app.</span></span> <span data-ttu-id="48e57-106">使用できます。</span><span class="sxs-lookup"><span data-stu-id="48e57-106">You can use:</span></span>

* <span data-ttu-id="48e57-107">すべての要求のリモート IP アドレスを確認するミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="48e57-107">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="48e57-108">アクション フィルターが特定のコント ローラーまたはアクション メソッドの要求のリモート IP アドレスを確認します。</span><span class="sxs-lookup"><span data-stu-id="48e57-108">Action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* <span data-ttu-id="48e57-109">Razor ページの要求のリモート IP アドレスを確認する razor ページのフィルター。</span><span class="sxs-lookup"><span data-stu-id="48e57-109">Razor Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="48e57-110">サンプル アプリでは、両方の方法を示します。</span><span class="sxs-lookup"><span data-stu-id="48e57-110">The sample app illustrates both approaches.</span></span> <span data-ttu-id="48e57-111">各ケースでは、承認されたクライアントの IP アドレスを含む文字列がアプリ設定に格納されます。</span><span class="sxs-lookup"><span data-stu-id="48e57-111">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="48e57-112">ミドルウェアやフィルターは、一覧に文字列を解析し、リモート ip アドレスの一覧を確認します。</span><span class="sxs-lookup"><span data-stu-id="48e57-112">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="48e57-113">それ以外の場合は、HTTP 403 Forbidden 状態コードが返されます。</span><span class="sxs-lookup"><span data-stu-id="48e57-113">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="48e57-114">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="48e57-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="48e57-115">セーフリスト</span><span class="sxs-lookup"><span data-stu-id="48e57-115">The safelist</span></span>

<span data-ttu-id="48e57-116">一覧が構成されている、 *appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="48e57-116">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="48e57-117">セミコロンで区切られた一覧し、IPv4 と IPv6 アドレスを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="48e57-117">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="48e57-118">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="48e57-118">Middleware</span></span>

<span data-ttu-id="48e57-119">`Configure`メソッドは、ミドルウェアを追加し、コンス トラクターのパラメーターでセーフリスト文字列を渡します。</span><span class="sxs-lookup"><span data-stu-id="48e57-119">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="48e57-120">ミドルウェアは、配列に文字列を解析し、配列内のリモート IP アドレスを探します。</span><span class="sxs-lookup"><span data-stu-id="48e57-120">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="48e57-121">リモート IP アドレスが見つからない場合は、HTTP 401 許可されていません、ミドルウェアが返されます。</span><span class="sxs-lookup"><span data-stu-id="48e57-121">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="48e57-122">この検証プロセスが、HTTP Get 要求に対してバイパスされます。</span><span class="sxs-lookup"><span data-stu-id="48e57-122">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="48e57-123">アクション フィルター</span><span class="sxs-lookup"><span data-stu-id="48e57-123">Action filter</span></span>

<span data-ttu-id="48e57-124">特定のコント ローラーまたはアクション メソッドに対してのみ、セーフリストをする場合は、アクション フィルターを使用します。</span><span class="sxs-lookup"><span data-stu-id="48e57-124">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="48e57-125">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="48e57-125">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="48e57-126">アクション フィルターは、サービス コンテナーに追加されます。</span><span class="sxs-lookup"><span data-stu-id="48e57-126">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="48e57-127">フィルターは、コント ローラーまたはアクション メソッドで使用できます。</span><span class="sxs-lookup"><span data-stu-id="48e57-127">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="48e57-128">フィルターを適用するサンプル アプリで、`Get`メソッド。</span><span class="sxs-lookup"><span data-stu-id="48e57-128">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="48e57-129">送信することによって、アプリをテストする場合は、 `Get` API の要求、属性がクライアントの IP アドレスを検証しています。</span><span class="sxs-lookup"><span data-stu-id="48e57-129">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="48e57-130">その他の任意の HTTP メソッドに、API を呼び出すことでテストするときに、ミドルウェア クライアントの ip アドレスの検証です。</span><span class="sxs-lookup"><span data-stu-id="48e57-130">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="48e57-131">Razor ページをフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="48e57-131">Razor Pages filter</span></span> 

<span data-ttu-id="48e57-132">セーフリスト、Razor ページ アプリに必要な場合は、Razor ページのフィルターを使用します。</span><span class="sxs-lookup"><span data-stu-id="48e57-132">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="48e57-133">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="48e57-133">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="48e57-134">このフィルターは MVC フィルター コレクションに追加することによって有効にします。</span><span class="sxs-lookup"><span data-stu-id="48e57-134">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="48e57-135">アプリを実行する Razor ページを要求すると、Razor ページのフィルターは、クライアントの ip アドレスを検証です。</span><span class="sxs-lookup"><span data-stu-id="48e57-135">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="48e57-136">次の手順</span><span class="sxs-lookup"><span data-stu-id="48e57-136">Next steps</span></span>

<span data-ttu-id="48e57-137">[詳細については、ASP.NET Core のミドルウェアは](xref:fundamentals/middleware/index)します。</span><span class="sxs-lookup"><span data-stu-id="48e57-137">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
