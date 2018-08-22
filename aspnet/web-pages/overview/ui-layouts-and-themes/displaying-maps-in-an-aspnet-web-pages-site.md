---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: ページ (Razor) サイトを ASP.NET web マップを表示する |Microsoft Docs
author: tfitzmac
description: この記事では、Bing、Google、Ma によって提供されるサービスのマッピングに基づいて、ASP.NET Web Pages (Razor) web サイトのページに対話型マップを表示する方法について説明しています.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 0b0879047bc71057884ebef98a598eed8c28cddf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825929"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="7f1c2-103">ASP.NET Web ページ (Razor) サイトでマップを表示します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="7f1c2-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7f1c2-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7f1c2-105">この記事では、Bing、Google、MapQuest、および Yahoo によって提供されるサービスのマッピングに基づいて、ASP.NET Web Pages (Razor) web サイトのページでインタラクティブ マップを表示する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="7f1c2-106">学習内容。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="7f1c2-107">アドレスに基づくマップを生成する方法。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="7f1c2-108">緯度と経度の座標に基づくマップを生成する方法。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="7f1c2-109">Bing Maps の開発者アカウントを登録し、Bing Maps を使用するキーを取得する方法。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="7f1c2-110">これは、この記事で導入された ASP.NET の機能です。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="7f1c2-111">`Maps`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7f1c2-112">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="7f1c2-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7f1c2-113">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="7f1c2-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="7f1c2-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="7f1c2-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="7f1c2-115">このチュートリアルは、WebMatrix 3 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="7f1c2-116">Web ページで表示できますマップ、ページを使用して`Maps`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="7f1c2-117">アドレスまたは経度と緯度の座標のセットのいずれかに基づくマップを生成することができます。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="7f1c2-118">`Maps`クラス Bing、Google、MapQuest、Yahoo などの人気のあるマップ エンジンを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="7f1c2-119">ページへのマッピングを追加する手順は、呼び出すことのマップ エンジンに関係なく同じです。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="7f1c2-120">使用可能なメソッドをマップを表示する JavaScript ファイル参照を追加するだけとのメソッドを呼び出して、`Maps`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="7f1c2-121">基準となるマップ サービスを選択する`Maps`を使用するヘルパー メソッド。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="7f1c2-122">これらのいずれかを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="7f1c2-123">必要なコンポーネントをインストールします。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="7f1c2-124">マップを表示するには、これらの情報が必要です。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="7f1c2-125">`Maps`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-125">The `Maps` helper.</span></span> <span data-ttu-id="7f1c2-126">このヘルパーは、ASP.NET Web Helpers Library のバージョン 2 では。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="7f1c2-127">ライブラリに追加済みしていない場合、NuGet パッケージとしてインストール、サイトのことができます。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="7f1c2-128">詳細については、次を参照してください。 [ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="7f1c2-129">(ギャラリーで検索、`microsoft-web-helpers`パッケージです)。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="7f1c2-130">JQuery ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-130">The jQuery library.</span></span> <span data-ttu-id="7f1c2-131">いくつかの WebMatrix サイトのテンプレートでの jQuery ライブラリに既に含まれて、*スクリプト*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="7f1c2-132">これらのライブラリがない場合、は、直接から最新の jQuery ライブラリをダウンロードすることができます、 [jQuery.org](http://jQuery.org)サイト。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="7f1c2-133">テンプレートを使用して新しいサイトを作成することも (たとえば、**スターター サイト**テンプレート) し、現在のサイトにそのサイトから jQuery ファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="7f1c2-134">最後に、Bing maps を使用する場合は、最初 (無料) アカウントを作成して、キーを取得します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="7f1c2-135">キーを取得するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="7f1c2-136">アカウントを作成、 [Bing Maps デベロッパー アカウント](https://www.microsoft.com/maps/developers/web.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="7f1c2-137">Microsoft アカウント (Windows Live ID) も必要です。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="7f1c2-138">キーを使用することを指定する**評価/テスト**します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="7f1c2-139">自分の WebMatrix と IIS Express を使用してコンピューターにマッピング関数をテストする場合は、移動、**サイト**ワークスペースと、サイトの URL に注意してください (たとえば、`http://localhost:50408`実際のポート番号は異なるかもしれませんが、)。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="7f1c2-140">これを使用することができます*localhost*として登録するときに、サイトのアドレス。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="7f1c2-141">アカウントを登録すると後、は、Bing Maps のアカウント センターに移動し、をクリックして**作成またはビューのキー**:</span><span class="sxs-lookup"><span data-stu-id="7f1c2-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![マッピング 2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="7f1c2-143">Bing を作成するキーを記録します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="7f1c2-144">(Google を使用して) アドレスに基づくマップを作成します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="7f1c2-145">次の例では、アドレスに基づくマップを表示するページを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="7f1c2-146">この例では、Google マップを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="7f1c2-147">という名前のファイルを作成する*MapAddress.cshtml*サイトのルートにします。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="7f1c2-148">このページに渡すことのできる、アドレスに基づくマップが生成されます。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="7f1c2-149">既存のコンテンツを上書き、ファイルに次のコードをコピーします。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="7f1c2-150">ページの次の機能に注目してください。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="7f1c2-151">`<script>`内の要素、`<head>`要素。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="7f1c2-152">例では、`<script>`要素の参照、 *jquery 1.6.4.min.js*ファイル。 これは、jQuery ライブラリ バージョン 1.6.4 の縮小 (圧縮) バージョン。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="7f1c2-153">参照にはことが前提とに注意してください、 *.js*ファイルは、*スクリプト*サイトのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="7f1c2-154">JQuery ライブラリの別のバージョンを使用している場合だけをポイントしているのバージョンに正しくを確認します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="7f1c2-155">呼び出し、`@Maps.GetGoogleHtml`ページの本文にします。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="7f1c2-156">アドレスをマップするには、アドレス文字列を渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="7f1c2-157">その他のマップ エンジンのメソッドと同様の方法では動作 (`@Maps.GetYahooHtml`、 `@Maps.GetMapQuestHtml`)。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="7f1c2-158">ページを実行し、アドレスを入力します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-158">Run the page and enter an address.</span></span> <span data-ttu-id="7f1c2-159">ページには、Google マップ、指定した場所を示すに基づいて、マップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![-1 のマッピング](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="7f1c2-161">緯度と経度に基づくマップの作成を調整 (Bing を使用)</span><span class="sxs-lookup"><span data-stu-id="7f1c2-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="7f1c2-162">この例では、座標に基づいてマップを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="7f1c2-163">この例では、Bing maps を使用する方法と、Bing キーを含める方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="7f1c2-164">(Bing キーを使用せず、他のマップ エンジンを使用しても、座標に基づいてマップを作成することができます)。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="7f1c2-165">という名前のファイルを作成する*MapCoordinates.cshtml*サイトと置換、既存のコンテンツと、次のコードとマークアップのルートに。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="7f1c2-166">置換`your-key-here`先に生成した Bing Maps キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="7f1c2-167">実行、 *MapCoordinates.cshtml*ページ、緯度と経度の座標を入力し、クリックして、 **Map It!**</span><span class="sxs-lookup"><span data-stu-id="7f1c2-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="7f1c2-168">を追加します。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-168">button.</span></span> <span data-ttu-id="7f1c2-169">(任意の座標がわからない場合お試しください以下。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="7f1c2-170">これは、Microsoft のレッドモンド キャンパス上の場所) です。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="7f1c2-171">緯度: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="7f1c2-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="7f1c2-172">経度:-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="7f1c2-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="7f1c2-173">指定した座標を使用して、ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7f1c2-173">The page is displayed using the coordinates that you specified.</span></span>

     ![マッピング-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="7f1c2-175">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="7f1c2-175">Additional Resources</span></span>


[<span data-ttu-id="7f1c2-176">Microsoft.Maps API リファレンス</span><span class="sxs-lookup"><span data-stu-id="7f1c2-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
