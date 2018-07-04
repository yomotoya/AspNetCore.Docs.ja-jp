---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: バンドルと縮小の資産を ASP.NET web ページ (Razor) サイト |Microsoft Docs
author: microsoft
description: バンドルと縮小には、サイトを速く方法です。 バンドルで結合する複数の JavaScript (.js) ファイルまたは複数のスタイル シート (.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: dd1b184846a7ac9c08df0212a69b154279791d1a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393803"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="c28d5-104">バンドルと ASP.NET Web Pages (Razor) サイトで資産を縮小します。</span><span class="sxs-lookup"><span data-stu-id="c28d5-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="c28d5-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c28d5-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c28d5-106">バンドルと縮小には、サイトを速く方法です。</span><span class="sxs-lookup"><span data-stu-id="c28d5-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="c28d5-107">複数の JavaScript を結合することができますのバンドル (*.js*) ファイルまたは複数のカスケード スタイル シート (*.css*) それらを一度に 1 つずつではなく、単位としてダウンロードできるようにします。</span><span class="sxs-lookup"><span data-stu-id="c28d5-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="c28d5-108">縮小では、空白を絞り出すし、他の種類のことがある小規模としてダウンロードしたファイルを作成するために圧縮を実行します。</span><span class="sxs-lookup"><span data-stu-id="c28d5-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="c28d5-109">ASP.NET Web Pages 2 の RC リリースはサポートしていませんバンドルと縮小に必要な要素を含むパッケージがまだ Microsoft WebMatrix では使用できないためです。</span><span class="sxs-lookup"><span data-stu-id="c28d5-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="c28d5-110">この不便をおかけして申し訳ありません。</span><span class="sxs-lookup"><span data-stu-id="c28d5-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="c28d5-111">パッケージは、ASP.NET Web ページ 2 と WebMatrix 2 の最終リリースで使用できることが必要です。</span><span class="sxs-lookup"><span data-stu-id="c28d5-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
