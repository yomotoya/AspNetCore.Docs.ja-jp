---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[How Do i:] HTTP ヘッダーの情報に基づいて ASP.NET ページのキャッシュ |Microsoft Docs'
author: rick-anderson
description: このビデオの Chris Pels でページの HTTP ヘッダーの情報に基づいて ASP.NET 出力キャッシュでページを保持する方法を示しています。 最初に、潜在的な HTTP hea.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2009
ms.topic: article
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: 3987e6ea1e5ea5575813bdf5598d0499ba37db20
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395254"
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="ca11f-104">[How Do i:] HTTP ヘッダーの情報に基づいて ASP.NET ページ キャッシュ</span><span class="sxs-lookup"><span data-stu-id="ca11f-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>
====================
<span data-ttu-id="ca11f-105">によって[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="ca11f-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="ca11f-106">このビデオの Chris Pels でページの HTTP ヘッダーの情報に基づいて ASP.NET 出力キャッシュでページを保持する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="ca11f-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="ca11f-107">最初に、潜在的な HTTP ヘッダーの値を確認します。</span><span class="sxs-lookup"><span data-stu-id="ca11f-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="ca11f-108">サンプル ページを作成し、値"そのまま使用-language"、ユーザーのブラウザーの言語に基づくキャッシュを制御する、HTTP ヘッダーを含む VaryByHeader 属性を持つ、OutputCache ディレクティブを使用します。</span><span class="sxs-lookup"><span data-stu-id="ca11f-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="ca11f-109">英語に設定されている IE、FireFox を使用して、フランス語に設定されている次のサンプル ページは表示されます。</span><span class="sxs-lookup"><span data-stu-id="ca11f-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="ca11f-110">最後に、web.config ファイルで cacheprofile です、キャッシュの定義に移動するオプションがについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ca11f-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="ca11f-111">&#9654;(12 分) のビデオを見る</span><span class="sxs-lookup"><span data-stu-id="ca11f-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
