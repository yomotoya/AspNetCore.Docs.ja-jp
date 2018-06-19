---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: '操作方法: 作成、MVC アプリケーション用のカスタム HTML ヘルパーですか。 | Microsoft Docs'
author: rick-anderson
description: このビデオでは、Chris Pels は、MVC アプリケーションの標準セットに含まれていないカスタム HtmlHelper を作成する方法を示します。 最初に、サンプルの MVC アプリケーションがインストールしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/11/2009
ms.topic: article
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 92faa04e1eefec0852604d51987ddaa9ee58838a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870103"
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="1701e-105">操作方法: 作成、MVC アプリケーション用のカスタム HTML ヘルパーですか。</span><span class="sxs-lookup"><span data-stu-id="1701e-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>
====================
<span data-ttu-id="1701e-106">によって[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="1701e-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="1701e-107">このビデオでは、Chris Pels は、MVC アプリケーションの標準セットに含まれていないカスタム HtmlHelper を作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1701e-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="1701e-108">最初に、サンプルの MVC アプリケーションは、デモのコント ローラーとカスタムの HtmlHelper をテストするビューが作成されます。</span><span class="sxs-lookup"><span data-stu-id="1701e-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="1701e-109">次に、カスタムの HtmlHelper の実装を表す拡張メソッドではパブリックの関数は、モジュールが作成されます。</span><span class="sxs-lookup"><span data-stu-id="1701e-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="1701e-110">作成するためのカスタム ヘルパーは`<img>`ページ内にタグをし、id、url、およびイメージ タグの alt テキストを含むいくつかの受信パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="1701e-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="1701e-111">ロジックが完成したを返すため、関数に追加し、`<img>`指定した情報を持つタグ。</span><span class="sxs-lookup"><span data-stu-id="1701e-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="1701e-112">カスタムの HtmlHelper は、イメージを表示するデモ ページで使用されます。</span><span class="sxs-lookup"><span data-stu-id="1701e-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="1701e-113">異なるを容易に作成の詳細の柔軟性を提供する複数のコンス トラクターのオーバーライドを含むようにカスタムの HtmlHelper を拡張するさらに、`<img>`タグ。</span><span class="sxs-lookup"><span data-stu-id="1701e-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="1701e-114">&#9654;ビデオでは (18 分)</span><span class="sxs-lookup"><span data-stu-id="1701e-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="1701e-115">[前へ](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [次へ](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="1701e-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
