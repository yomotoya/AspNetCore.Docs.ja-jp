---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[How Do i:]AJAX のメソッド間を選択ページの更新しますか? | Microsoft Docs'
author: JoeStagner
description: このビデオでは、行えるは、ASP.NET アプリケーションで AJAX スタイル ページの更新プログラムを実行する 2 つの主な方法を比較します。 最初のメソッドは、Upd を使用するのには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2007
ms.topic: article
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: b86471b93b7e3a1ed371288195f09fa28353ab36
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885537"
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="ead09-105">[How Do i:]AJAX のメソッド間を選択ページの更新しますか?</span><span class="sxs-lookup"><span data-stu-id="ead09-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>
====================
<span data-ttu-id="ead09-106">によって[行える](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="ead09-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="ead09-107">このビデオでは、行えるは、ASP.NET アプリケーションで AJAX スタイル ページの更新プログラムを実行する 2 つの主な方法を比較します。</span><span class="sxs-lookup"><span data-stu-id="ead09-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="ead09-108">最初の方法では、コードを追加する必要があるないクライアント側またはサーバー側のいずれかに書き込まれる、UpdatePanel を使用します。</span><span class="sxs-lookup"><span data-stu-id="ead09-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="ead09-109">UpdatePanel を使用する利点は、自動的に動作するすべてのものです。</span><span class="sxs-lookup"><span data-stu-id="ead09-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="ead09-110">不利な点を大量の AJAX 要求と応答に含まれるデータが必要ですが、クライアントとサーバーを実行する全ページのライフ サイクルが必要です。</span><span class="sxs-lookup"><span data-stu-id="ead09-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="ead09-111">2 番目の方法では、ネットワークのコールバックを使用して、追加のコードが、クライアント側とサーバー側の両方に書き込まれる必要があります。</span><span class="sxs-lookup"><span data-stu-id="ead09-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="ead09-112">ネットワークのコールバックの使用の利点は、AJAX 要求と応答に含まれるほとんどのデータが必要ですが、クライアントで、そのサーバーで呼び出されたサービス メソッドのみを実行する必要がありますを します。</span><span class="sxs-lookup"><span data-stu-id="ead09-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="ead09-113">Penality は、時間と必要なコードを記述するまでにかかる労力です。</span><span class="sxs-lookup"><span data-stu-id="ead09-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="ead09-114">Joe でビデオを終了、AJAX スタイル ページの更新プログラムの 2 つの主な方法を選択するときの判断について説明します。</span><span class="sxs-lookup"><span data-stu-id="ead09-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="ead09-115">(このビデオからコードを使用して、[どのようにすれば開始 ASP.NET AJAX と共に](how-do-i-get-started-with-aspnet-ajax.md)ビデオと[操作方法、クライアント側ネットワークのコールバック ASP.NET AJAX と共に](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md)ビデオです)。</span><span class="sxs-lookup"><span data-stu-id="ead09-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="ead09-116">&#9654;ビデオでは (11 分)</span><span class="sxs-lookup"><span data-stu-id="ead09-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> <span data-ttu-id="ead09-117">[前へ](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [次へ](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="ead09-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>
