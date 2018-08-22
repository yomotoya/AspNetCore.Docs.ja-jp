---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: 異なるバージョンの ASP.NET Web Pages (Razor) サイド バイ サイド実行 |Microsoft Docs
author: tfitzmac
description: この記事では、さまざまなバージョンを使用する web サイトが構成されている場合、同じコンピューターまたはサーバーで ASP.NET Web Pages (Razor) の web サイトを実行する方法について説明しています.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 9021f9b7a68b8b20f7f2fbcd5649cc7226401a1b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834742"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="cf6fc-103">ASP.NET Web Pages (Razor) のさまざまなバージョンを並行して実行しています。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="cf6fc-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cf6fc-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cf6fc-105">この記事では、異なるバージョンの ASP.NET Web ページを使用する web サイトが構成されている場合、同じコンピューターまたはサーバーで ASP.NET Web Pages (Razor) の web サイトを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="cf6fc-106">学習内容。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="cf6fc-107">既定の動作は ASP.NET で ASP.NET Web Pages を使用して構築されたサイトがある場合です。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="cf6fc-108">以前のバージョンの ASP.NET Web ページで実行する新しいサイトを構成する方法。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="cf6fc-109">これは、この記事で導入された ASP.NET の機能です。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="cf6fc-110">`webPages:Version`構成設定。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="cf6fc-111">ソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="cf6fc-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="cf6fc-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="cf6fc-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="cf6fc-113">このチュートリアルは、ASP.NET Web Pages 2 および ASP.NET Web Pages 1.0 により連携します。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="cf6fc-114">ASP.NET Web ページには、サイド バイ サイドの web サイトを実行する機能がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="cf6fc-115">これにより、古いバージョンの ASP.NET Web Pages アプリケーションを実行し、新しい ASP.NET Web Pages アプリケーションの構築など、いずれも、同じコンピューター上で実行を続行できます。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="cf6fc-116">WebMatrix を使用した Web ページをインストールするときの注意点を次に示します。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="cf6fc-117">既定では、既存の Web ページ アプリケーションは、コンピューターに最新バージョンとして実行されます。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="cf6fc-118">(アセンブリはグローバル アセンブリ キャッシュ (GAC) がインストールされ自動的に使用されます)。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="cf6fc-119">異なるバージョンの ASP.NET Web ページを使用してサイトを実行する場合は、そのためには、サイトを構成できます。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="cf6fc-120">サイトを持っていない場合、 *web.config*サイトのルートにファイル、新しいものを作成および既存のコンテンツを上書きするのには、次の XML をコピーします。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="cf6fc-121">サイトが既に含まれている場合、 *web.config*ファイルを追加、`<appSettings>`要素に次のいずれかのように、`<configuration>`セクション。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="cf6fc-122">'-で、バージョンを指定しない場合、 *web.config*ファイル、サイトは、最新バージョンとして配置されます。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="cf6fc-123">(、アセンブリにコピー、 *bin*デプロイされたサイトのフォルダーです)。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="cf6fc-124">Web Matrix でサイト テンプレートを使用して作成する新しいアプリケーションが、サイトの Web ページのバージョンのアセンブリを含める*bin*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="cf6fc-125">サイトに適切なアセンブリをインストールする NuGet を使用して、サイトで使用する Web ページのバージョンを常に制御する一般に、 *bin*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="cf6fc-126">パッケージを検索するには、次を参照してください。 [NuGet.org](http://NuGet.org)します。</span><span class="sxs-lookup"><span data-stu-id="cf6fc-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cf6fc-127">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="cf6fc-127">Additional Resources</span></span>

[<span data-ttu-id="cf6fc-128">ASP.NET Web Pages 2 の主な機能</span><span class="sxs-lookup"><span data-stu-id="cf6fc-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
