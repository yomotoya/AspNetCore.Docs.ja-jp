---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: "ASP.NET MVC 5 アプリケーションのライフ サイクル |Microsoft ドキュメント"
author: cephalin
description: "ASP.NET MVC 5 アプリケーションのライフ サイクルをグラフ化する PDF ドキュメントをダウンロードします。 このライフ サイクルのドキュメントでは、MVC のライフ サイクルの概要を表示する."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 5692c43168eb261c91f40e2046897a1e5d31a028
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="5df55-104">ASP.NET MVC 5 アプリケーションのライフ サイクル</span><span class="sxs-lookup"><span data-stu-id="5df55-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="5df55-105">によって[Cephas Lin](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="5df55-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="5df55-106">PDF ドキュメントをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="5df55-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="5df55-107">HTTP の受信からのすべての ASP.NET MVC 5 アプリケーションのライフ サイクルを HTTP 応答を送信することを要求するグラフがクライアントに返信する PDF ドキュメントをダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="5df55-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="5df55-108">ASP.NET MVC に追加された新しい管理者向けの教育用のツール、およびアプリケーションの特定の側面にドリル ダウンする必要がある方のための参照としてもされています。</span><span class="sxs-lookup"><span data-stu-id="5df55-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="5df55-109">PDF ドキュメントには、次の機能があります。</span><span class="sxs-lookup"><span data-stu-id="5df55-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="5df55-110">関連する[HttpApplication](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx)に MVC が統合されたを理解するための段階、 [ASP.NET アプリケーションのライフ サイクル](https://msdn.microsoft.com/en-us/library/bb470252.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="5df55-110">Relevant [HttpApplication](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/en-us/library/bb470252.aspx).</span></span>
- <span data-ttu-id="5df55-111">すべての MVC アプリケーションが要求処理パイプラインを通過する主なステージを理解することができます、MVC アプリケーション ライフ サイクルの概要を表示します。</span><span class="sxs-lookup"><span data-stu-id="5df55-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="5df55-112">詳細の表示で、ダウン要求処理パイプラインの詳細にドリルを示しています。</span><span class="sxs-lookup"><span data-stu-id="5df55-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="5df55-113">高レベルのビューと詳細を表示する、さまざまな段階にライフ サイクルの詳細を収集する方法を比較できます。</span><span class="sxs-lookup"><span data-stu-id="5df55-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="5df55-114">[PDF のダウンロード](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)より大きなビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="5df55-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="5df55-115">配置およびオーバーライド可能なすべてのメソッドの目的は、[コント ローラー](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller.aspx)要求処理パイプライン内のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5df55-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="5df55-116">可能性がありますか、任意の 1 つのメソッドをオーバーライドする必要がある可能性がありますが注意する効果のライフ サイクルの適切な段階でコードを作成できるように、アプリケーションのライフ サイクルにおける各自の役割を理解するためです。</span><span class="sxs-lookup"><span data-stu-id="5df55-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="5df55-117">フィルターの種類 (認証、承認、アクション、および結果) の各を呼び出す方法を示す飛んでアップ図。</span><span class="sxs-lookup"><span data-stu-id="5df55-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="5df55-118">詳細ビューで、目的の各ポイントから、役に立つ記事やブログにリンクします。</span><span class="sxs-lookup"><span data-stu-id="5df55-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="5df55-119">次の手順</span><span class="sxs-lookup"><span data-stu-id="5df55-119">Next Steps</span></span>

<span data-ttu-id="5df55-120">このドキュメントは、ニーズを満たしていますか。</span><span class="sxs-lookup"><span data-stu-id="5df55-120">Does this document meet your need?</span></span> <span data-ttu-id="5df55-121">お客様のフィードバックをお待ちしています。</span><span class="sxs-lookup"><span data-stu-id="5df55-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="5df55-122">ASP.NET MVC のライフ サイクルで、アプリケーションでどの質問がある場合[Stackoverflow](http://stackoverflow.com/help)と[ASP.NET MVC フォーラム](https://forums.asp.net/1146.aspx)最適な場所に確認してください。</span><span class="sxs-lookup"><span data-stu-id="5df55-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="5df55-123">次の[me](https://twitter.com/Cephas_MSFT)ツイッター 最新のチュートリアルで更新プログラムを取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="5df55-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
