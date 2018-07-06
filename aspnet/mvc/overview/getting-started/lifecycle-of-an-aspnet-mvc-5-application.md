---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: ASP.NET MVC 5 アプリケーションのライフ サイクル |Microsoft Docs
author: cephalin
description: ASP.NET MVC 5 アプリケーションのライフ サイクルをグラフ化する PDF ドキュメントをダウンロードします。 このライフ サイクルのドキュメントは、MVC のライフ サイクルの概要を確認する.
ms.author: aspnetcontent
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: cceaa377e77a7229edb2e33a67d000e26b43358a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839256"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="44ca1-104">ASP.NET MVC 5 アプリケーションのライフ サイクル</span><span class="sxs-lookup"><span data-stu-id="44ca1-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="44ca1-105">によって[Cephas Lin](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="44ca1-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="44ca1-106">PDF ドキュメントをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="44ca1-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="44ca1-107">ここで、HTTP の受信からのすべての ASP.NET MVC 5 アプリケーションのライフ サイクルは、HTTP 応答を送信する要求のグラフは、クライアントにバックしている PDF ドキュメントをダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="44ca1-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="44ca1-108">新しい ASP.NET MVC には、教育ツール、およびアプリケーションの特定の側面にドリル ダウンする必要がある方のための参照としても設計されています。</span><span class="sxs-lookup"><span data-stu-id="44ca1-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="44ca1-109">PDF ドキュメントには、次の機能があります。</span><span class="sxs-lookup"><span data-stu-id="44ca1-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="44ca1-110">関連する[HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) MVC が統合を理解するためのステージ、 [ASP.NET アプリケーションのライフ サイクル](https://msdn.microsoft.com/library/bb470252.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="44ca1-110">Relevant [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/library/bb470252.aspx).</span></span>
- <span data-ttu-id="44ca1-111">要求処理パイプラインですべての MVC アプリケーションを通過する主なステージを理解することができます、MVC アプリケーション ライフ サイクルの概要を確認します。</span><span class="sxs-lookup"><span data-stu-id="44ca1-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="44ca1-112">要求処理パイプラインの詳細にダウン ドリルを表示する詳細ビュー。</span><span class="sxs-lookup"><span data-stu-id="44ca1-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="44ca1-113">概要ビューと詳細を表示するさまざまな段階のライフ サイクルの詳細を収集する方法を比較することができます。</span><span class="sxs-lookup"><span data-stu-id="44ca1-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="44ca1-114">[PDF をダウンロード](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)にすると拡大表示を参照してください。</span><span class="sxs-lookup"><span data-stu-id="44ca1-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="44ca1-115">配置およびオーバーライド可能なすべてのメソッドの目的、[コント ローラー](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx)要求処理パイプライン内のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="44ca1-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="44ca1-116">あるか、任意の 1 つのメソッドをオーバーライドする必要がある可能性がありますが、する効果の適切なライフ サイクルの段階でコードを記述するには、アプリケーションのライフ サイクルでのロールを理解するため重要です。</span><span class="sxs-lookup"><span data-stu-id="44ca1-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="44ca1-117">驚くほど衝撃的アップ コンテンツの各フィルターの種類 (認証、承認、アクション、および結果) が呼び出される方法を示す図。</span><span class="sxs-lookup"><span data-stu-id="44ca1-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="44ca1-118">詳細ビューで、関心のある各ポイントから役に立つ記事やブログにリンクします。</span><span class="sxs-lookup"><span data-stu-id="44ca1-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="44ca1-119">次の手順</span><span class="sxs-lookup"><span data-stu-id="44ca1-119">Next Steps</span></span>

<span data-ttu-id="44ca1-120">このドキュメントは、ニーズを満たしているか。</span><span class="sxs-lookup"><span data-stu-id="44ca1-120">Does this document meet your need?</span></span> <span data-ttu-id="44ca1-121">ご意見ご提案があります。</span><span class="sxs-lookup"><span data-stu-id="44ca1-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="44ca1-122">アプリケーションでは、ASP.NET MVC のライフ サイクルでの質問がある場合[Stackoverflow](http://stackoverflow.com/help)と[ASP.NET MVC フォーラム](https://forums.asp.net/1146.aspx)は質問に最適な場所。</span><span class="sxs-lookup"><span data-stu-id="44ca1-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="44ca1-123">次の[me](https://twitter.com/Cephas_MSFT) twitter で最新のチュートリアルでは、更新プログラムを取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="44ca1-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
