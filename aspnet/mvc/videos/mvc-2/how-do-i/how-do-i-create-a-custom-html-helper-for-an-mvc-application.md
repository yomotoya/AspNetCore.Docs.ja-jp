---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: 操作が、MVC アプリケーション用のカスタム HTML ヘルパーを作成する方法はありますか | Microsoft Docs
author: rick-anderson
description: このビデオでは、Chris Pels はつまり、MVC アプリケーションで標準のセットで使用できないカスタム HtmlHelper を作成する方法を示します。 最初に、サンプルの MVC アプリケーションがインストールしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/11/2009
ms.topic: article
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 87e74ade0182589d22aeaf66a608165df7ea2ee6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380142"
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="81c97-105">操作が、MVC アプリケーション用のカスタム HTML ヘルパーを作成する方法はありますか</span><span class="sxs-lookup"><span data-stu-id="81c97-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>
====================
<span data-ttu-id="81c97-106">によって[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="81c97-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="81c97-107">このビデオでは、Chris Pels はつまり、MVC アプリケーションで標準のセットで使用できないカスタム HtmlHelper を作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="81c97-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="81c97-108">最初に、デモのコント ローラーとビューをカスタムの HtmlHelper をテストするサンプルの MVC アプリケーションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="81c97-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="81c97-109">次に、モジュールは、カスタムの HtmlHelper の実装を表す拡張メソッドであるパブリック関数で作成されます。</span><span class="sxs-lookup"><span data-stu-id="81c97-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="81c97-110">作成するためのカスタム ヘルパーは`<img>`ページでタグし、は、id、url、およびイメージのタグの代替テキストを含むいくつかの受信パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="81c97-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="81c97-111">完成したを返す関数に、ロジックを追加して、`<img>`指定された情報のタグ。</span><span class="sxs-lookup"><span data-stu-id="81c97-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="81c97-112">カスタムの HtmlHelper は、イメージを表示するデモ ページに使用されます。</span><span class="sxs-lookup"><span data-stu-id="81c97-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="81c97-113">カスタムの HtmlHelper を拡張して、簡単に作成するさまざまな詳細は柔軟性を提供する複数のコンス トラクターのオーバーライドを含める最後に、`<img>`タグ。</span><span class="sxs-lookup"><span data-stu-id="81c97-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="81c97-114">&#9654;ビデオ (18 分)</span><span class="sxs-lookup"><span data-stu-id="81c97-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="81c97-115">[前へ](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [次へ](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="81c97-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
