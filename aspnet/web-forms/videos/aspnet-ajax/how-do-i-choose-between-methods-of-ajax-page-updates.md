---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[How Do i:]AJAX のメソッド間を選択ページの更新しますか? | Microsoft Docs'
author: JoeStagner
description: このビデオでは、Joe Stagner は、ASP.NET アプリケーションで AJAX スタイルのページの更新プログラムを実行する 2 つの主な方法を比較します。 最初のメソッドでは、Upd を使用する.
ms.author: aspnetcontent
ms.date: 07/09/2007
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: 226fb0423ea05ad9034c909037358331918f2892
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838440"
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="9161d-105">[How Do i:]AJAX のメソッド間を選択ページの更新しますか?</span><span class="sxs-lookup"><span data-stu-id="9161d-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>
====================
<span data-ttu-id="9161d-106">によって[Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="9161d-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="9161d-107">このビデオでは、Joe Stagner は、ASP.NET アプリケーションで AJAX スタイルのページの更新プログラムを実行する 2 つの主な方法を比較します。</span><span class="sxs-lookup"><span data-stu-id="9161d-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="9161d-108">最初のメソッドでコードを追加する必要はありません、クライアント側またはサーバー側のいずれかに書き込まれる、UpdatePanel を使用することです。</span><span class="sxs-lookup"><span data-stu-id="9161d-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="9161d-109">UpdatePanel を使用する利点は、自動的に動作するすべてのものです。</span><span class="sxs-lookup"><span data-stu-id="9161d-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="9161d-110">ペナルティは、多くの AJAX 要求と応答に含まれるデータが必要ですが、クライアントとサーバーを実行する完全なページ ライフ サイクルが必要です。</span><span class="sxs-lookup"><span data-stu-id="9161d-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="9161d-111">2 番目のメソッドでは、追加のコードがクライアント側とサーバー側の両方で記述する必要があるネットワークのコールバックを使用します。</span><span class="sxs-lookup"><span data-stu-id="9161d-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="9161d-112">ネットワーク コールバックを使用する利点は、AJAX 要求と応答に含まれるほとんどのデータが必要ですが、クライアントで、そのサーバーでのみ呼び出されたサービス メソッドを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9161d-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="9161d-113">大幅には、時間と必要なコードの書き込みにかかる労力が。</span><span class="sxs-lookup"><span data-stu-id="9161d-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="9161d-114">Joe は、AJAX スタイルのページの更新プログラムの 2 つの主な方法を選択するときの判断について説明ビデオを終了します。</span><span class="sxs-lookup"><span data-stu-id="9161d-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="9161d-115">(このビデオからコードを使用して、[方法は開始するには ASP.NET AJAX と共に](how-do-i-get-started-with-aspnet-ajax.md)ビデオと[方法 ASP.NET AJAX でクライアント側のネットワーク コールバックを](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md)ビデオ)。</span><span class="sxs-lookup"><span data-stu-id="9161d-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="9161d-116">&#9654;(11 分) のビデオを見る</span><span class="sxs-lookup"><span data-stu-id="9161d-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> <span data-ttu-id="9161d-117">[前へ](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [次へ](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="9161d-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>
