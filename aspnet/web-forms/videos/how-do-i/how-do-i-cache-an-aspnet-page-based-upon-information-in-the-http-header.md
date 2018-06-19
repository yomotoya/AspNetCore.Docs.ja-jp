---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[How Do i:] ASP.NET ページが HTTP ヘッダーの情報に基づくキャッシュ |Microsoft ドキュメント'
author: rick-anderson
description: このビデオ Chris Pels では、ページの HTTP ヘッダーの情報に基づく ASP.NET 出力キャッシュ内のページを保持する方法を示します。 最初に、潜在的な HTTP hea しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2009
ms.topic: article
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: ce5ea10396d0fe31d72425e2431102a0cb0c3bd0
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
ms.locfileid: "28882218"
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="3b4c9-104">[How Do i:] ASP.NET ページが HTTP ヘッダーの情報に基づくキャッシュ</span><span class="sxs-lookup"><span data-stu-id="3b4c9-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>
====================
<span data-ttu-id="3b4c9-105">によって[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="3b4c9-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="3b4c9-106">このビデオ Chris Pels では、ページの HTTP ヘッダーの情報に基づく ASP.NET 出力キャッシュ内のページを保持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3b4c9-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="3b4c9-107">最初に、潜在的な HTTP ヘッダーの値を確認します。</span><span class="sxs-lookup"><span data-stu-id="3b4c9-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="3b4c9-108">次に、サンプル ページを作成し、OutputCache ディレクティブを使用して"そのまま使用-language"、ユーザーのブラウザーの言語に基づいてキャッシュを制御する、HTTP ヘッダーの値を格納する VaryByHeader 属性を持つ。</span><span class="sxs-lookup"><span data-stu-id="3b4c9-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="3b4c9-109">サンプルのページは英語に設定されている IE に、次にフランス語を使用して設定されている FireFox に表示されます。</span><span class="sxs-lookup"><span data-stu-id="3b4c9-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="3b4c9-110">最後に、web.config ファイルで cacheprofile ですキャッシュ定義に移動するオプションが説明しています。</span><span class="sxs-lookup"><span data-stu-id="3b4c9-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="3b4c9-111">& #9654 です。(12 分) のビデオを見る</span><span class="sxs-lookup"><span data-stu-id="3b4c9-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
