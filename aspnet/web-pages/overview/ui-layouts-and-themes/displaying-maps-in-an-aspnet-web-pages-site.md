---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: (Razor) サイト マップを表示する ASP.NET web Pages |Microsoft ドキュメント
author: tfitzmac
description: この記事では、Bing、Google、Ma によって提供されるサービスのマッピングに基づいて、ASP.NET Web Pages (Razor) web サイトのページに対話型のマップを表示する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 608dab8760bad7b877ab6fd4f89b21e980f5b1db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30893863"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="3dea7-103">ASP.NET Web Pages (Razor) サイトにマップを表示します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="3dea7-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3dea7-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3dea7-105">この記事では、Bing、Google、MapQuest、および yahoo から提供されるサービスのマッピングに基づいて、ASP.NET Web Pages (Razor) web サイトのページに対話型のマップを表示する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="3dea7-106">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="3dea7-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="3dea7-107">アドレスに基づくマップを生成する方法です。</span><span class="sxs-lookup"><span data-stu-id="3dea7-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="3dea7-108">緯度と経度座標に基づくマップを生成する方法です。</span><span class="sxs-lookup"><span data-stu-id="3dea7-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="3dea7-109">Bing マップは、開発者アカウントを登録し、Bing マップで使用するキーを取得する方法。</span><span class="sxs-lookup"><span data-stu-id="3dea7-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="3dea7-110">これは、アーティクルで導入された ASP.NET の機能です。</span><span class="sxs-lookup"><span data-stu-id="3dea7-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="3dea7-111">`Maps`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="3dea7-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3dea7-112">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="3dea7-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3dea7-113">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="3dea7-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="3dea7-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="3dea7-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="3dea7-115">このチュートリアルは、WebMatrix 3 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="3dea7-116">Web ページで表示できますマップ ページを使用して`Maps`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="3dea7-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="3dea7-117">アドレスまたは緯度と経度座標のセットに基づいてマップを生成することができます。</span><span class="sxs-lookup"><span data-stu-id="3dea7-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="3dea7-118">`Maps`クラスには、Bing、Google、MapQuest、yahoo などの一般的なマップ エンジンを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="3dea7-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="3dea7-119">ページへのマッピングを追加する手順は、呼び出すマップ エンジンに関係なく同じです。</span><span class="sxs-lookup"><span data-stu-id="3dea7-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="3dea7-120">マップを表示する使用可能なメソッドは、JavaScript ファイル参照を追加するだけと、その後のメソッドを呼び出す、`Maps`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="3dea7-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="3dea7-121">基になるマップ サービスを選択する`Maps`ヘルパー メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="3dea7-122">これらのいずれかを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="3dea7-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="3dea7-123">必要なコンポーネントをインストールします。</span><span class="sxs-lookup"><span data-stu-id="3dea7-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="3dea7-124">マップを表示するには、これらの要素が必要です。</span><span class="sxs-lookup"><span data-stu-id="3dea7-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="3dea7-125">`Maps`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="3dea7-125">The `Maps` helper.</span></span> <span data-ttu-id="3dea7-126">このヘルパーは、ASP.NET Web Helpers Library のバージョン 2 でです。</span><span class="sxs-lookup"><span data-stu-id="3dea7-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="3dea7-127">ライブラリを既に追加していない場合は、NuGet パッケージとして、サイトでインストールすることができます。</span><span class="sxs-lookup"><span data-stu-id="3dea7-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="3dea7-128">詳細については、「 [ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)です。</span><span class="sxs-lookup"><span data-stu-id="3dea7-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="3dea7-129">(ギャラリーで、検索、`microsoft-web-helpers`パッケージです)。</span><span class="sxs-lookup"><span data-stu-id="3dea7-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="3dea7-130">JQuery ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="3dea7-130">The jQuery library.</span></span> <span data-ttu-id="3dea7-131">いくつかの WebMatrix サイト テンプレートがあらかじめ含まれてで jQuery ライブラリには、*スクリプト*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3dea7-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="3dea7-132">これらのライブラリがない場合、は、最新の jQuery ライブラリから直接をダウンロードすることができます、 [jQuery.org](http://jQuery.org)サイトです。</span><span class="sxs-lookup"><span data-stu-id="3dea7-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="3dea7-133">テンプレートを使用して新しいサイトを作成することもできます (たとえば、**スターター サイト**テンプレート) し、jQuery ファイルをそのサイトから、現在のサイトにコピーします。</span><span class="sxs-lookup"><span data-stu-id="3dea7-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="3dea7-134">最後に、Bing maps を使用する場合は、最初 (無料) アカウントを作成して、キーを取得します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="3dea7-135">キーの取得、これらの手順に従います。</span><span class="sxs-lookup"><span data-stu-id="3dea7-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="3dea7-136">アカウントを作成、 [Bing Maps の開発者アカウント](https://www.microsoft.com/maps/developers/web.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="3dea7-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="3dea7-137">Microsoft アカウント (Windows Live ID) も必要です。</span><span class="sxs-lookup"><span data-stu-id="3dea7-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="3dea7-138">キーを使用することを指定できます**評価/テスト**です。</span><span class="sxs-lookup"><span data-stu-id="3dea7-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="3dea7-139">WebMatrix と IIS Express を使用して、自分のコンピューターで、マッピング関数をテストする場合は、移動、**サイト**ワークスペースとは、サイトの URL (たとえば、`http://localhost:50408`ポート番号が異なる可能性がありますが、)。</span><span class="sxs-lookup"><span data-stu-id="3dea7-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="3dea7-140">これを行うこともできます*localhost*として登録する場合は、サイトのアドレス。</span><span class="sxs-lookup"><span data-stu-id="3dea7-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="3dea7-141">アカウントの登録した後、Bing マップのアカウント センターに移動し、クリックして**作成またはビューのキー**:</span><span class="sxs-lookup"><span data-stu-id="3dea7-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="3dea7-143">Bing を作成するキーを記録します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="3dea7-144">(Google を使用)、アドレスに基づくマップを作成します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="3dea7-145">次の例では、アドレスに基づくマップを表示するページを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="3dea7-146">この例では、Google マップを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="3dea7-147">という名前のファイルを作成する*MapAddress.cshtml*サイトのルートにします。</span><span class="sxs-lookup"><span data-stu-id="3dea7-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="3dea7-148">このページに渡されたアドレスに基づくマップが生成されます。</span><span class="sxs-lookup"><span data-stu-id="3dea7-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="3dea7-149">既存のコンテンツを上書きして、ファイルに次のコードをコピーします。</span><span class="sxs-lookup"><span data-stu-id="3dea7-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="3dea7-150">ページの次の機能を確認してください。</span><span class="sxs-lookup"><span data-stu-id="3dea7-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="3dea7-151">`<script>`内の要素、`<head>`要素。</span><span class="sxs-lookup"><span data-stu-id="3dea7-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="3dea7-152">例では、`<script>`要素の参照、 *jquery 1.6.4.min.js* jQuery ライブラリ バージョン 1.6.4 の縮小された (圧縮) バージョンではファイルです。</span><span class="sxs-lookup"><span data-stu-id="3dea7-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="3dea7-153">参照を前提とする、 *.js*ファイルは、*スクリプト*サイトのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3dea7-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="3dea7-154">JQuery ライブラリの別のバージョンを使用している場合のみをポイントしているそのバージョンを正しく確認してください。</span><span class="sxs-lookup"><span data-stu-id="3dea7-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="3dea7-155">呼び出し、`@Maps.GetGoogleHtml`ページの本文にします。</span><span class="sxs-lookup"><span data-stu-id="3dea7-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="3dea7-156">アドレスをマップするには、アドレス文字列を渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="3dea7-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="3dea7-157">機能の他のマップ エンジンのメソッドは同様の方法で (`@Maps.GetYahooHtml`、 `@Maps.GetMapQuestHtml`)。</span><span class="sxs-lookup"><span data-stu-id="3dea7-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="3dea7-158">ページを実行し、アドレスを入力します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-158">Run the page and enter an address.</span></span> <span data-ttu-id="3dea7-159">ページは、Google マップ、指定した場所を表示するに基づく、マップを表示します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![mapping-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="3dea7-161">緯度と経度に基づいてマップを作成する座標を (Bing を使用)</span><span class="sxs-lookup"><span data-stu-id="3dea7-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="3dea7-162">この例では、座標に基づくマップを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="3dea7-163">この例では、Bing maps を使用する方法と、Bing キーを含める方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="3dea7-164">(Bing キーを使用せず、他のマップのエンジンを使用しても、座標に基づくマップを作成することができます)。</span><span class="sxs-lookup"><span data-stu-id="3dea7-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="3dea7-165">という名前のファイルを作成する*MapCoordinates.cshtml*サイトと置換と、次のコードとマークアップ、既存のコンテンツのルートに。</span><span class="sxs-lookup"><span data-stu-id="3dea7-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="3dea7-166">置き換える`your-key-here`以前に生成した Bing Maps キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="3dea7-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="3dea7-167">実行、 *MapCoordinates.cshtml*ページ、緯度と経度座標を入力し、クリックして、 **Map It!**</span><span class="sxs-lookup"><span data-stu-id="3dea7-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="3dea7-168">ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3dea7-168">button.</span></span> <span data-ttu-id="3dea7-169">(任意の座標がわからない場合を再試行してください以下です。</span><span class="sxs-lookup"><span data-stu-id="3dea7-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="3dea7-170">これは Microsoft レッドモンド上の場所です。)</span><span class="sxs-lookup"><span data-stu-id="3dea7-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="3dea7-171">Latitude: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="3dea7-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="3dea7-172">経度:-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="3dea7-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="3dea7-173">指定した座標を使用して、ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3dea7-173">The page is displayed using the coordinates that you specified.</span></span>

     ![マッピング-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="3dea7-175">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="3dea7-175">Additional Resources</span></span>


[<span data-ttu-id="3dea7-176">Microsoft.Maps API リファレンス</span><span class="sxs-lookup"><span data-stu-id="3dea7-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
