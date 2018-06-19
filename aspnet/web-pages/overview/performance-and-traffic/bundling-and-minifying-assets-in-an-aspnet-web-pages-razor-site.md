---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: バンドル化と ASP.NET web アセットを縮小するページ (Razor) サイト |Microsoft ドキュメント
author: microsoft
description: バンドルと縮小する方法は、サイトをより速くします。 バンドルを結合する複数の JavaScript (.js) ファイルまたは複数のスタイル シート (.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: df00b5c9e741e429011143d04121df97c1930602
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528551"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="a0292-104">バンドル化と ASP.NET Web Pages (Razor) サイトで資産を縮小します。</span><span class="sxs-lookup"><span data-stu-id="a0292-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="a0292-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a0292-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a0292-106">バンドルと縮小する方法は、サイトをより速くします。</span><span class="sxs-lookup"><span data-stu-id="a0292-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="a0292-107">複数の JavaScript を組み合わせることにより、バンドル (*.js*) ファイルまたは複数のカスケード スタイル シート (*.css*) ファイルを一度に 1 つではなく、単位としてダウンロードできるようにします。</span><span class="sxs-lookup"><span data-stu-id="a0292-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="a0292-108">縮小は空白を絞り、他の種類のことがある小規模としてダウンロードしたファイルを作成するための圧縮を実行します。</span><span class="sxs-lookup"><span data-stu-id="a0292-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="a0292-109">ASP.NET Web Pages 2 の RC リリースはためにサポートされませんバンドルと縮小に必要な要素を含むパッケージはまだ Microsoft WebMatrix では使用できません。</span><span class="sxs-lookup"><span data-stu-id="a0292-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="a0292-110">この不便をおかけして申し訳ありません。</span><span class="sxs-lookup"><span data-stu-id="a0292-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="a0292-111">パッケージは、ASP.NET Web Pages 2 および WebMatrix 2 の最終リリースで使用できると想定されます。</span><span class="sxs-lookup"><span data-stu-id="a0292-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
