---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: 異なるバージョンの ASP.NET Web Pages (Razor) サイド バイ サイド実行 |Microsoft ドキュメント
author: tfitzmac
description: この記事では、さまざまなバージョンを使用する web サイトが構成されている場合、同じコンピューターまたはサーバーの ASP.NET Web Pages (Razor) web サイトを実行する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 1729f3201013926b221afc92d23416b0081d8efb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="2edfc-103">ASP.NET Web Pages (Razor) のさまざまなバージョンをサイド バイ サイドで実行されています。</span><span class="sxs-lookup"><span data-stu-id="2edfc-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="2edfc-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2edfc-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2edfc-105">この記事では、異なるバージョンの ASP.NET Web Pages を使用する web サイトが構成されている場合、同じコンピューターまたはサーバーの ASP.NET Web Pages (Razor) web サイトを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2edfc-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="2edfc-106">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="2edfc-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="2edfc-107">どのような既定の動作は ASP.NET で構築された ASP.NET Web Pages をサイトがある場合です。</span><span class="sxs-lookup"><span data-stu-id="2edfc-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="2edfc-108">古いバージョンの ASP.NET Web Pages を使用して実行する新しいサイトを構成する方法。</span><span class="sxs-lookup"><span data-stu-id="2edfc-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="2edfc-109">これは、アーティクルで導入された ASP.NET の機能です。</span><span class="sxs-lookup"><span data-stu-id="2edfc-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="2edfc-110">`webPages:Version`構成設定。</span><span class="sxs-lookup"><span data-stu-id="2edfc-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="2edfc-111">ソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="2edfc-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="2edfc-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="2edfc-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="2edfc-113">このチュートリアルは、ASP.NET Web Pages 2 と ASP.NET Web Pages 1.0 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="2edfc-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="2edfc-114">ASP.NET Web ページには、サイド バイ サイドの web サイトを実行する機能がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="2edfc-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="2edfc-115">これにより、古いバージョンの ASP.NET Web Pages アプリケーションを実行、新規の ASP.NET Web Pages アプリケーションをビルドおよびそれらのすべてを同じコンピューターで実行し続けることができます。</span><span class="sxs-lookup"><span data-stu-id="2edfc-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="2edfc-116">WebMatrix で Web ページをインストールする際にいくつかの点を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2edfc-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="2edfc-117">既定では、既存の Web Pages アプリケーションは、コンピューターに最新バージョンとして実行されます。</span><span class="sxs-lookup"><span data-stu-id="2edfc-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="2edfc-118">(アセンブリがグローバル アセンブリ キャッシュ (GAC) にインストールされているし、自動的に使用されます。)</span><span class="sxs-lookup"><span data-stu-id="2edfc-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="2edfc-119">別のバージョンの ASP.NET Web Pages を使用してサイトを実行する場合を行うには、サイトを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="2edfc-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="2edfc-120">サイトを持っていない場合、 *web.config*サイトのルートにファイル、新たに作成し、既存のコンテンツを上書きするのには、次の XML をコピーします。</span><span class="sxs-lookup"><span data-stu-id="2edfc-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="2edfc-121">サイトが既に含まれている場合、 *web.config*ファイルに追加し、`<appSettings>`要素に次のいずれかのように、`<configuration>`セクションです。</span><span class="sxs-lookup"><span data-stu-id="2edfc-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="2edfc-122">'-でバージョンが指定されていない場合、 *web.config*ファイル、サイトの最新バージョンとして配置します。</span><span class="sxs-lookup"><span data-stu-id="2edfc-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="2edfc-123">(、アセンブリにコピー、 *bin*配置済みのサイトのフォルダーです)。</span><span class="sxs-lookup"><span data-stu-id="2edfc-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="2edfc-124">Web マトリックス内のサイト テンプレートを使用して作成する新しいアプリケーションが、サイトの Web ページ バージョンのアセンブリを含める*bin*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="2edfc-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="2edfc-125">NuGet を使用して、サイトに適切なアセンブリをインストールすることによって、サイトで使用する Web ページのバージョンを常に制御する一般に、 *bin*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="2edfc-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="2edfc-126">パッケージを検索するには、次を参照してください。 [NuGet.org](http://NuGet.org)です。</span><span class="sxs-lookup"><span data-stu-id="2edfc-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2edfc-127">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="2edfc-127">Additional Resources</span></span>

[<span data-ttu-id="2edfc-128">ASP.NET Web Pages 2 で、上位の特徴</span><span class="sxs-lookup"><span data-stu-id="2edfc-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
