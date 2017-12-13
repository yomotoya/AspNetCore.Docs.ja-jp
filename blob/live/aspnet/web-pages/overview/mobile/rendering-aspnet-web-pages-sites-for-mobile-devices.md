---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: "モバイル デバイスのページ (Razor) サイトを ASP.NET Web をレンダリング |Microsoft ドキュメント"
author: tfitzmac
description: "この記事では、モバイル デバイスで適切に表示される ASP.NET Web Pages (Razor) サイトのページを作成する方法について説明します。 学習内容: する方法."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 08b714eb2ffaefc7c7e2e5c9a7428106b231e5b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="eebf7-104">モバイル デバイス用の ASP.NET Web Pages (Razor) サイトの表示</span><span class="sxs-lookup"><span data-stu-id="eebf7-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="eebf7-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="eebf7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="eebf7-106">この記事では、モバイル デバイスで適切に表示される ASP.NET Web Pages (Razor) サイトのページを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="eebf7-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="eebf7-107">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="eebf7-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="eebf7-108">名前付け規則を使用して、ページがモバイル デバイス向けに設計されているを指定する方法です。</span><span class="sxs-lookup"><span data-stu-id="eebf7-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="eebf7-109">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="eebf7-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="eebf7-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="eebf7-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="eebf7-111">このチュートリアルは、ASP.NET Web Pages 2 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="eebf7-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="eebf7-112">ASP.NET Web ページを使用して、モバイル デバイスまたはその他のデバイスでのコンテンツ表示のカスタムの表示を作成できます。</span><span class="sxs-lookup"><span data-stu-id="eebf7-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="eebf7-113">ASP.NET Web Pages サイトでデバイスに固有のページを作成する最も簡単な方法は次のようにファイルの名前付けパターンを使用して、:*ファイル名*。*Mobile**.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="eebf7-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.**Mobile**.cshtml*.</span></span> <span data-ttu-id="eebf7-114">ページの 2 つのバージョンを作成することができます (たとえば、1 つという名前の*MyFile.cshtml*という名前の 1 つ*MyFile.Mobile.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="eebf7-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="eebf7-115">実行時に、モバイル デバイスを要求したとき*MyFile.cshtml*、ASP.NET のコンテンツを表示する*MyFile.Mobile.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="eebf7-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="eebf7-116">それ以外の場合、 *MyFile.cshtml*が表示されます。</span><span class="sxs-lookup"><span data-stu-id="eebf7-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="eebf7-117">次の例では、モバイル デバイスのコンテンツ ページを追加することで、モバイルのレンダリングを有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="eebf7-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="eebf7-118">*Page1.cshtml*コンテンツにはナビゲーション サイド バーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="eebf7-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="eebf7-119">*Page1.Mobile.cshtml*が同じコンテンツが含まれていますが、サイドバーを省略します。</span><span class="sxs-lookup"><span data-stu-id="eebf7-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="eebf7-120">ASP.NET Web Pages サイトは、という名前のファイルを作成する*Page1.cshtml*して現在のコンテンツを次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="eebf7-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="eebf7-121">という名前のファイルを作成する*Page1.Mobile.cshtml*し、既存のコンテンツを次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="eebf7-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="eebf7-122">ページのモバイル バージョンが小さい画面できれいに表示の [ナビゲーション] セクションを省略することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="eebf7-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="eebf7-123">デスクトップのブラウザーを実行しを参照*Page1.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="eebf7-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="eebf7-124">![mobilesites 1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eebf7-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="eebf7-125">モバイル ブラウザー (または、モバイル デバイス エミュレーター) を実行しを参照*Page1.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="eebf7-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="eebf7-126">(含めないようにすることを確認*.mobile です。*</span><span class="sxs-lookup"><span data-stu-id="eebf7-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="eebf7-127">URL の一部として、します。)要求内容がにもかかわらず*Page1.cshtml*、ASP.NET 表示*Page1.Mobile.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="eebf7-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites 2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="eebf7-129">モバイル ページをテストするには、デスクトップ コンピューターで実行されているモバイル デバイス シミュレーターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="eebf7-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="eebf7-130">このツールを使用して、それらのモバイル デバイスの表示は、web ページをテストできます (つまり、通常より小さな領域を表示)。</span><span class="sxs-lookup"><span data-stu-id="eebf7-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="eebf7-131">シミュレーターの 1 つの例は、[ユーザー エージェント スイッチャー アドオン](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/)Mozilla Firefox に対応することができます Firefox のデスクトップ バージョンからのさまざまなモバイル ブラウザーをエミュレートします。</span><span class="sxs-lookup"><span data-stu-id="eebf7-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="eebf7-132">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="eebf7-132">Additional Resources</span></span>


<span data-ttu-id="eebf7-133">[Windows Phone エミュレーター](https://msdn.microsoft.com/en-us/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="eebf7-133">[Windows Phone Emulator](https://msdn.microsoft.com/en-us/library/ff402563(v=VS.92).aspx)</span></span>
