---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: "URL ルーティング |Microsoft ドキュメント"
author: Erikre
description: "このチュートリアルの系列では、お用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 279617e4ebb475d935c0d1e01e08a3a2def0f9e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="url-routing"></a><span data-ttu-id="ee13f-103">URL ルーティング</span><span class="sxs-lookup"><span data-stu-id="ee13f-103">URL Routing</span></span>
====================
<span data-ttu-id="ee13f-104">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="ee13f-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="ee13f-105">[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="ee13f-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="ee13f-106">このチュートリアルの系列では、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="ee13f-107">Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)はこのチュートリアルのシリーズに付随する使用できます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="ee13f-108">このチュートリアルでは、URL ルーティングをサポートするために、Wingtip Toys サンプル アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-108">In this tutorial, you will modify the Wingtip Toys sample application to support URL routing.</span></span> <span data-ttu-id="ee13f-109">ルーティングは、わかりやすい、覚えやすく検索エンジンによってより適切にサポートされている Url を使用して、web アプリケーションを使用します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-109">Routing enables your web application to use URLs that are friendly, easier to remember, and better supported by search engines.</span></span> <span data-ttu-id="ee13f-110">このチュートリアルでは、前のチュートリアルでは、「メンバーシップと管理」を上に構築され、Wingtip Toys チュートリアル シリーズの一部です。</span><span class="sxs-lookup"><span data-stu-id="ee13f-110">This tutorial builds on the previous tutorial "Membership and Administration" and is part of the Wingtip Toys tutorial series.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="ee13f-111">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="ee13f-111">What you'll learn:</span></span>

- <span data-ttu-id="ee13f-112">ASP.NET Web フォーム アプリケーションのルートを登録する方法。</span><span class="sxs-lookup"><span data-stu-id="ee13f-112">How to register routes for an ASP.NET Web Forms application.</span></span>
- <span data-ttu-id="ee13f-113">Web ページへのルートを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="ee13f-113">How to add routes to a web page.</span></span>
- <span data-ttu-id="ee13f-114">ルートをサポートするためにデータベースからデータを選択する方法です。</span><span class="sxs-lookup"><span data-stu-id="ee13f-114">How to select data from a database to support routes.</span></span>

## <a name="aspnet-routing-overview"></a><span data-ttu-id="ee13f-115">ASP.NET ルーティングの概要</span><span class="sxs-lookup"><span data-stu-id="ee13f-115">ASP.NET Routing Overview</span></span>

<span data-ttu-id="ee13f-116">URL ルーティングを使用すると、そのまま使用するアプリケーションを構成する物理ファイルにマップされていない Url を要求します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-116">URL routing allows you to configure an application to accept request URLs that do not map to physical files.</span></span> <span data-ttu-id="ee13f-117">要求 URL は、ユーザーが、web サイトでページを検索する際にブラウザーに入力 URL だけです。</span><span class="sxs-lookup"><span data-stu-id="ee13f-117">A request URL is simply the URL a user enters into their browser to find a page on your web site.</span></span> <span data-ttu-id="ee13f-118">ルーティング定義に使用する Url をユーザーに意味のある意味であるし、検索エンジン最適化 (SEO) に役立つことができます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-118">You use routing to define URLs that are semantically meaningful to users and that can help with search-engine optimization (SEO).</span></span>

<span data-ttu-id="ee13f-119">既定では、Web フォーム テンプレートに含まれる[ASP.NET Friendly Url](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)です。</span><span class="sxs-lookup"><span data-stu-id="ee13f-119">By default, the Web Forms template includes [ASP.NET Friendly URLs](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/).</span></span> <span data-ttu-id="ee13f-120">使用して基本的なルーティング作業の多くを実装する*フレンドリな Url*です。</span><span class="sxs-lookup"><span data-stu-id="ee13f-120">Much of the basic routing work will be implemented by using *Friendly URLs*.</span></span> <span data-ttu-id="ee13f-121">ただし、このチュートリアルでは、カスタマイズされたルーティング機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-121">However, in this tutorial you will add customized routing capabilities.</span></span>

<span data-ttu-id="ee13f-122">URL ルーティングをカスタマイズする前に、Wingtip Toys のサンプル アプリケーションは、次の URL を使用して、製品にリンクできます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-122">Before customizing URL routing, the Wingtip Toys sample application can link to a product using the following URL:</span></span>

`https://localhost:44300/ProductDetails.aspx?productID=2`

<span data-ttu-id="ee13f-123">URL ルーティングをカスタマイズすると、Wingtip Toys のサンプル アプリケーションは、読みやすい URL を使用して、同じ製品にリンクします。</span><span class="sxs-lookup"><span data-stu-id="ee13f-123">By customizing URL routing, the Wingtip Toys sample application will link to the same product using an easier to read URL:</span></span>

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a><span data-ttu-id="ee13f-124">ルート</span><span class="sxs-lookup"><span data-stu-id="ee13f-124">Routes</span></span>

<span data-ttu-id="ee13f-125">ルートは、ハンドラーにマップされている URL パターンです。</span><span class="sxs-lookup"><span data-stu-id="ee13f-125">A route is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="ee13f-126">ハンドラーは、Web フォーム アプリケーションで .aspx ファイルなどの物理ファイルを指定できます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-126">The handler can be a physical file, such as an .aspx file in a Web Forms application.</span></span> <span data-ttu-id="ee13f-127">ハンドラーは、要求を処理するクラスもできます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-127">A handler can also be a class that processes the request.</span></span> <span data-ttu-id="ee13f-128">ルートを定義するのには、URL パターン、ハンドラー、および必要に応じて、ルートの名前を指定して、ルート クラスのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-128">To define a route, you create an instance of the Route class by specifying the URL pattern, the handler, and optionally a name for the route.</span></span>

<span data-ttu-id="ee13f-129">アプリケーションに追加することによって、ルートを追加する、 `Route` 、静的なオブジェクト`Routes`のプロパティ、`RouteTable`クラスです。</span><span class="sxs-lookup"><span data-stu-id="ee13f-129">You add the route to the application by adding the `Route` object to the static `Routes` property of the `RouteTable` class.</span></span> <span data-ttu-id="ee13f-130">ルートのプロパティは、`RouteCollection`アプリケーションのすべてのルートを格納するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="ee13f-130">The Routes property is a `RouteCollection` object that stores all the routes for the application.</span></span>

### <a name="url-patterns"></a><span data-ttu-id="ee13f-131">URL パターン</span><span class="sxs-lookup"><span data-stu-id="ee13f-131">URL Patterns</span></span>

<span data-ttu-id="ee13f-132">URL パターンには、リテラル値と変数のプレース ホルダー (URL パラメーターと呼ばれる) を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-132">A URL pattern can contain literal values and variable placeholders (referred to as URL parameters).</span></span> <span data-ttu-id="ee13f-133">リテラルとプレース ホルダーがスラッシュで区切られますが、URL のセグメントにあります (`/`) 文字です。</span><span class="sxs-lookup"><span data-stu-id="ee13f-133">The literals and placeholders are located in segments of the URL which are delimited by the slash (`/`) character.</span></span>

<span data-ttu-id="ee13f-134">Web アプリケーションに要求が行われたときに、URL はセグメントとプレース ホルダーに解析され、変数の値が要求ハンドラーを提供します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-134">When a request to your web application is made, the URL is parsed into segments and placeholders, and the variable values are provided to the request handler.</span></span> <span data-ttu-id="ee13f-135">このプロセスは、クエリ文字列内のデータを解析および要求ハンドラーに渡される方法に似ています。</span><span class="sxs-lookup"><span data-stu-id="ee13f-135">This process is similar to the way the data in a query string is parsed and passed to the request handler.</span></span> <span data-ttu-id="ee13f-136">どちらの場合、変数の情報が URL に含まれ、キー/値ペアの形式でハンドラーに渡されます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-136">In both cases, variable information is included in the URL and passed to the handler in the form of key-value pairs.</span></span> <span data-ttu-id="ee13f-137">クエリ文字列の場合、キーと値の両方が、URL には。</span><span class="sxs-lookup"><span data-stu-id="ee13f-137">For query strings, both the keys and the values are in the URL.</span></span> <span data-ttu-id="ee13f-138">ルートの場合、キーは、URL パターンで定義されているプレース ホルダー名と値のみが URL には。</span><span class="sxs-lookup"><span data-stu-id="ee13f-138">For routes, the keys are the placeholder names defined in the URL pattern, and only the values are in the URL.</span></span>

<span data-ttu-id="ee13f-139">URL パターンは、中かっこで囲み、プレース ホルダーを定義する (`{`と`}`)。</span><span class="sxs-lookup"><span data-stu-id="ee13f-139">In a URL pattern, you define placeholders by enclosing them in braces ( `{` and `}` ).</span></span> <span data-ttu-id="ee13f-140">セグメントに複数のプレース ホルダーを定義できますが、プレース ホルダーをリテラル値で区切る必要があります。</span><span class="sxs-lookup"><span data-stu-id="ee13f-140">You can define more than one placeholder in a segment, but the placeholders must be separated by a literal value.</span></span> <span data-ttu-id="ee13f-141">たとえば、`{language}-{country}/{action}`は有効なルート パターン。</span><span class="sxs-lookup"><span data-stu-id="ee13f-141">For example, `{language}-{country}/{action}` is a valid route pattern.</span></span> <span data-ttu-id="ee13f-142">ただし、`{language}{country}/{action}`リテラル値またはプレース ホルダーの間の区切り記号がないために、有効なパターンではありません。</span><span class="sxs-lookup"><span data-stu-id="ee13f-142">However, `{language}{country}/{action}` is not a valid pattern, because there is no literal value or delimiter between the placeholders.</span></span> <span data-ttu-id="ee13f-143">そのため、ルーティング、国のプレース ホルダーの言語のプレース ホルダーの値から値を分離する場所を特定することはできません。</span><span class="sxs-lookup"><span data-stu-id="ee13f-143">Therefore, routing cannot determine where to separate the value for the language placeholder from the value for the country placeholder.</span></span>

### <a name="mapping-and-registering-routes"></a><span data-ttu-id="ee13f-144">マッピングとルートを登録します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-144">Mapping and Registering Routes</span></span>

<span data-ttu-id="ee13f-145">Wingtip Toys のサンプル アプリケーションのページへのルートを含めることができます、前に、アプリケーションの起動時に、ルートを登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ee13f-145">Before you can include routes to pages of the Wingtip Toys sample application, you must register the routes when the application starts.</span></span> <span data-ttu-id="ee13f-146">ルートを登録するには、変更、`Application_Start`イベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="ee13f-146">To register the routes, you will modify the `Application_Start` event handler.</span></span>

1. <span data-ttu-id="ee13f-147">**ソリューション エクスプ ローラー**Visual Studio での検索して開く、 *Global.asax.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ee13f-147">In **Solution Explorer**of Visual Studio, find and open the *Global.asax.cs* file.</span></span>
2. <span data-ttu-id="ee13f-148">黄色で強調表示のコードを追加、 *Global.asax.cs*ファイルの次のようにします。</span><span class="sxs-lookup"><span data-stu-id="ee13f-148">Add the code highlighted in yellow to the *Global.asax.cs* file as follows:</span></span>   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

<span data-ttu-id="ee13f-149">Wingtip Toys サンプル アプリケーションの起動時は、呼び出し、`Application_Start`イベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="ee13f-149">When the Wingtip Toys sample application starts, it calls the `Application_Start` event handler.</span></span> <span data-ttu-id="ee13f-150">このイベント ハンドラーの最後に、`RegisterCustomRoutes`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-150">At the end of this event handler, the `RegisterCustomRoutes` method is called.</span></span> <span data-ttu-id="ee13f-151">`RegisterCustomRoutes`メソッドを呼び出して各ルートは追加、`MapPageRoute`のメソッド、`RouteCollection`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="ee13f-151">The `RegisterCustomRoutes` method adds each route by calling the `MapPageRoute` method of the `RouteCollection` object.</span></span> <span data-ttu-id="ee13f-152">ルートを定義するには、ルート名、ルート URL および物理的な URL を使用します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-152">Routes are defined using a route name, a route URL and a physical URL.</span></span>

<span data-ttu-id="ee13f-153">最初のパラメーター ("`ProductsByCategoryRoute`") のルートの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-153">The first parameter ("`ProductsByCategoryRoute`") is the route name.</span></span> <span data-ttu-id="ee13f-154">ルートを呼び出すことが必要な場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-154">It is used to call the route when it is needed.</span></span> <span data-ttu-id="ee13f-155">2 番目のパラメーター ("`Category/{categoryName}`")、わかりやすい動的なことができる URL に基づく置換コードを定義します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-155">The second parameter ("`Category/{categoryName}`") defines the friendly replacement URL that can be dynamic based on code.</span></span> <span data-ttu-id="ee13f-156">データ コントロールとそのデータに基づいて生成されるリンクを作成する場合は、このルートを使用します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-156">You use this route when you are populating a data control with links that are generated based on data.</span></span> <span data-ttu-id="ee13f-157">ルートが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-157">A route is shown as follows:</span></span>

[!code-csharp[Main](url-routing/samples/sample2.cs)]

<span data-ttu-id="ee13f-158">ルートの 2 番目のパラメーターには、中かっこで指定された、動的な値が含まれています (`{ }`)。</span><span class="sxs-lookup"><span data-stu-id="ee13f-158">The second parameter of the route includes a dynamic value specified by braces (`{ }`).</span></span> <span data-ttu-id="ee13f-159">ここで、`categoryName`適切なルーティング パスを決定するために使用される変数です。</span><span class="sxs-lookup"><span data-stu-id="ee13f-159">In this case, the `categoryName` is a variable that will be used to determine the proper routing path.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ee13f-160">**Optional**</span><span class="sxs-lookup"><span data-stu-id="ee13f-160">**Optional**</span></span>
> 
> <span data-ttu-id="ee13f-161">移動することによって、コードの管理が容易に見つけることがあります、`RegisterCustomRoutes`別のクラスにメソッドです。</span><span class="sxs-lookup"><span data-stu-id="ee13f-161">You might find it easier to manage your code by moving the `RegisterCustomRoutes` method to a separate class.</span></span> <span data-ttu-id="ee13f-162">*ロジック*フォルダーごとに別々`RouteActions`クラスです。</span><span class="sxs-lookup"><span data-stu-id="ee13f-162">In the *Logic* folder, create a separate `RouteActions` class.</span></span> <span data-ttu-id="ee13f-163">上移動`RegisterCustomRoutes`メソッドから、 *Global.asax.cs*を新しいファイル`RoutesActions`クラスです。</span><span class="sxs-lookup"><span data-stu-id="ee13f-163">Move the above `RegisterCustomRoutes` method from the *Global.asax.cs* file into the new `RoutesActions` class.</span></span> <span data-ttu-id="ee13f-164">使用して、`RoleActions`クラスおよび`createAdmin`メソッドを呼び出す方法の例として、`RegisterCustomRoutes`メソッドから、 *Global.asax.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ee13f-164">Use the `RoleActions` class and the `createAdmin` method as an example of how to call the `RegisterCustomRoutes` method from the *Global.asax.cs* file.</span></span>


<span data-ttu-id="ee13f-165">お気付き、`RegisterRoutes`メソッドの呼び出しを使用して、`RouteConfig`の先頭にあるオブジェクト、`Application_Start`イベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="ee13f-165">You may also have noticed the `RegisterRoutes` method call using the `RouteConfig` object at the beginning of the `Application_Start` event handler.</span></span> <span data-ttu-id="ee13f-166">この呼び出しが既定のルーティングを実装しようとしています。</span><span class="sxs-lookup"><span data-stu-id="ee13f-166">This call is made to implement default routing.</span></span> <span data-ttu-id="ee13f-167">Visual Studio の Web フォーム テンプレートを使用してアプリケーションを作成したときに既定のコードとして含まれています。</span><span class="sxs-lookup"><span data-stu-id="ee13f-167">It was included as default code when you created the application using Visual Studio's Web Forms template.</span></span>

## <a name="retrieving-and-using-route-data"></a><span data-ttu-id="ee13f-168">取得して、ルート データを使用します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-168">Retrieving and Using Route Data</span></span>

<span data-ttu-id="ee13f-169">前述のように、ルートを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-169">As mentioned above, routes can be defined.</span></span> <span data-ttu-id="ee13f-170">追加したコード、`Application_Start`内のイベント ハンドラー、 *Global.asax.cs*ファイルは、定義可能なルートを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-170">The code that you added to the `Application_Start` event handler in the *Global.asax.cs* file loads the definable routes.</span></span>

### <a name="setting-routes"></a><span data-ttu-id="ee13f-171">ルートの設定</span><span class="sxs-lookup"><span data-stu-id="ee13f-171">Setting Routes</span></span>

<span data-ttu-id="ee13f-172">ルートでは、追加のコードを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ee13f-172">Routes require you to add additional code.</span></span> <span data-ttu-id="ee13f-173">このチュートリアルでは、取得するモデル バインドを使用して.、`RouteValueDictionary`データ コントロールからのデータを使用してルートを生成するときに使用されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="ee13f-173">In this tutorial, you will use model binding to retrieve a `RouteValueDictionary` object that is used when generating the routes using data from a data control.</span></span> <span data-ttu-id="ee13f-174">`RouteValueDictionary`オブジェクトが製品の特定のカテゴリに属する製品名の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-174">The `RouteValueDictionary` object will contain a list of product names that belong to a specific category of products.</span></span> <span data-ttu-id="ee13f-175">データ ルートに各製品のリンクが作成されます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-175">A link is created for each product based on the data and route.</span></span>

#### <a name="enable-routes-for-categories-and-products"></a><span data-ttu-id="ee13f-176">分類と製品のルートを有効にします。</span><span class="sxs-lookup"><span data-stu-id="ee13f-176">Enable Routes for Categories and Products</span></span>

<span data-ttu-id="ee13f-177">次に、使用するアプリケーションを更新します、`ProductsByCategoryRoute`に含める各製品カテゴリのリンクの適切なルートを決定します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-177">Next, you'll update the application to use the `ProductsByCategoryRoute` to determine the correct route to include for each product category link.</span></span> <span data-ttu-id="ee13f-178">更新することもあります、 *ProductList.aspx*ページに各製品のルーティング リンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ee13f-178">You'll also update the *ProductList.aspx* page to include a routed link for each product.</span></span> <span data-ttu-id="ee13f-179">リンクが表示されます、変更前に、と同様に、リンク、URL ルーティングが使用されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="ee13f-179">The links will be displayed as they were before the change, however the links will now use URL routing.</span></span>

1. <span data-ttu-id="ee13f-180">**ソリューション エクスプ ローラー**を開き、 *Site.Master*ページがまだ開いていない場合。</span><span class="sxs-lookup"><span data-stu-id="ee13f-180">In **Solution Explorer**, open the *Site.Master* page if it is not already open.</span></span>
2. <span data-ttu-id="ee13f-181">更新プログラム、 **ListView**という名前のコントロール"`categoryList`"黄色で強調表示、変更のため、マークアップように表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-181">Update the **ListView** control named "`categoryList`" with the changes highlighted in yellow, so the markup appears as follows:</span></span>   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. <span data-ttu-id="ee13f-182">**ソリューション エクスプ ローラー**を開き、 *ProductList.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="ee13f-182">In **Solution Explorer**, open the *ProductList.aspx* page.</span></span>
4. <span data-ttu-id="ee13f-183">更新プログラム、`ItemTemplate`の要素、 *ProductList.aspx*マークアップが次のように表示されるように、黄色で強調表示されている更新プログラムを含むページ。</span><span class="sxs-lookup"><span data-stu-id="ee13f-183">Update the `ItemTemplate` element of the *ProductList.aspx* page with the updates highlighted in yellow, so the markup appears as follows:</span></span>   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. <span data-ttu-id="ee13f-184">コード ビハインドを開く*ProductList.aspx.cs*黄色で強調されている次の名前空間を追加します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-184">Open the code-behind of *ProductList.aspx.cs* and add the following namespace as highlighted in yellow:</span></span>  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. <span data-ttu-id="ee13f-185">置換、`GetProducts`分離コードのメソッド (*ProductList.aspx.cs*) を次のコード。</span><span class="sxs-lookup"><span data-stu-id="ee13f-185">Replace the `GetProducts` method of the code-behind (*ProductList.aspx.cs*) with the following code:</span></span>   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a><span data-ttu-id="ee13f-186">詳細については製品コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-186">Add Code for Product Details</span></span>

<span data-ttu-id="ee13f-187">ここで、分離コードを更新 (*ProductDetails.aspx.cs*) の*ProductDetails.aspx*ルート データを使用するページ。</span><span class="sxs-lookup"><span data-stu-id="ee13f-187">Now, update the code-behind (*ProductDetails.aspx.cs*) for the *ProductDetails.aspx* page to use route data.</span></span> <span data-ttu-id="ee13f-188">注意して、新しい`GetProduct`メソッドも古い非対応、非ルーティングの URL を使用して、ユーザーがブックマークが付けられたリンクを持つ場合、クエリ文字列の値を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="ee13f-188">Notice that the new `GetProduct` method also accepts a query string value for the case where the user has a link bookmarked that uses the older non-friendly, non-routed URL.</span></span>

1. <span data-ttu-id="ee13f-189">置換、`GetProduct`分離コードのメソッド (*ProductDetails.aspx.cs*) を次のコード。</span><span class="sxs-lookup"><span data-stu-id="ee13f-189">Replace the `GetProduct` method of the code-behind (*ProductDetails.aspx.cs*) with the following code:</span></span>   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a><span data-ttu-id="ee13f-190">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="ee13f-190">Running the Application</span></span>

<span data-ttu-id="ee13f-191">今すぐ更新済みのルートを表示するアプリケーションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-191">You can run the application now to see the updated routes.</span></span>

1. <span data-ttu-id="ee13f-192">キーを押して**f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-192">Press **F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="ee13f-193">ブラウザーが開き、表示、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="ee13f-193">The browser opens and shows the *Default.aspx* page.</span></span>
2. <span data-ttu-id="ee13f-194">クリックして、**製品**ページの上部にリンクします。</span><span class="sxs-lookup"><span data-stu-id="ee13f-194">Click the **Products** link at the top of the page.</span></span>  
 <span data-ttu-id="ee13f-195">すべての製品が表示されます、 *ProductList.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="ee13f-195">All products are displayed on the *ProductList.aspx* page.</span></span> <span data-ttu-id="ee13f-196">ブラウザーの (使用するポート番号を使用して) 次の URL が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-196">The following URL (using your port number) is displayed for the browser:</span></span>  
    `https://localhost:44300/ProductList`
3. <span data-ttu-id="ee13f-197">次をクリックして、**車**ページの上部にあるカテゴリのリンク。</span><span class="sxs-lookup"><span data-stu-id="ee13f-197">Next, click the **Cars** category link near the top of the page.</span></span>  
 <span data-ttu-id="ee13f-198">Cars のみが表示されます、 *ProductList.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="ee13f-198">Only cars are displayed on the *ProductList.aspx* page.</span></span> <span data-ttu-id="ee13f-199">ブラウザーの (使用するポート番号を使用して) 次の URL が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-199">The following URL (using your port number) is displayed for the browser:</span></span>  
    `https://localhost:44300/Category/Cars`
4. <span data-ttu-id="ee13f-200">最初の車の名前を含むリンクがページに表示 をクリックします ("**変換可能な車**") 製品の詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-200">Click the link containing the name of the first car listed on the page ("**Convertible Car**") to display the product details.</span></span>  
 <span data-ttu-id="ee13f-201">ブラウザーの (使用するポート番号を使用して) 次の URL が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee13f-201">The following URL (using your port number) is displayed for the browser:</span></span>  
    `https://localhost:44300/Product/Convertible%20Car`
5. <span data-ttu-id="ee13f-202">次に、ブラウザーに (使用するポート番号を使用して) 次の非ルーティング URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-202">Next, enter the following non-routed URL (using your port number) into the browser:</span></span>  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 <span data-ttu-id="ee13f-203">コードでは、ユーザーがブックマークが付けられたリンクを持っている場合、クエリ文字列を含む、URL をまだ認識します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-203">The code still recognizes a URL that includes a query string, for the case where a user has a link bookmarked.</span></span>

## <a name="summary"></a><span data-ttu-id="ee13f-204">概要</span><span class="sxs-lookup"><span data-stu-id="ee13f-204">Summary</span></span>

<span data-ttu-id="ee13f-205">このチュートリアルでは、分類と製品のルートを追加しました。</span><span class="sxs-lookup"><span data-stu-id="ee13f-205">In this tutorial, you have added routes for categories and products.</span></span> <span data-ttu-id="ee13f-206">モデル バインディングを使用するデータ コントロールとルートを統合する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="ee13f-206">You have learned how routes can be integrated with data controls that use model binding.</span></span> <span data-ttu-id="ee13f-207">次のチュートリアルでは、グローバル エラー処理を実装します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-207">In the next tutorial, you will implement global error handling.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ee13f-208">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="ee13f-208">Additional Resources</span></span>

[<span data-ttu-id="ee13f-209">ASP.NET Friendly Url</span><span class="sxs-lookup"><span data-stu-id="ee13f-209">ASP.NET Friendly URLs</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[<span data-ttu-id="ee13f-210">Azure App Service のメンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET Web フォーム アプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="ee13f-210">Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[<span data-ttu-id="ee13f-211">Microsoft Azure の無料試用版</span><span class="sxs-lookup"><span data-stu-id="ee13f-211">Microsoft Azure - Free Trial</span></span>](https://azure.microsoft.com/pricing/free-trial/)

>[!div class="step-by-step"]
<span data-ttu-id="ee13f-212">[前へ](membership-and-administration.md)
[次へ](aspnet-error-handling.md)</span><span class="sxs-lookup"><span data-stu-id="ee13f-212">[Previous](membership-and-administration.md)
[Next](aspnet-error-handling.md)</span></span>
