---
title: "コントローラーを追加する"
author: rick-anderson
description: "単純な ASP.NET Core MVC アプリにコントローラーを追加する方法"
manager: wpickett
ms.author: riande
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/adding-controller
ms.openlocfilehash: f56cc88c04b587129e242e1a2d0582185675e542
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-controller-to-a-aspnet-core-mvc-app-with-visual-studio"></a><span data-ttu-id="e0daa-103">Visual Studio を使用して ASP.NET Core MVC アプリにコントローラーを追加する</span><span class="sxs-lookup"><span data-stu-id="e0daa-103">Adding a controller to a ASP.NET Core MVC app with Visual Studio</span></span>

<span data-ttu-id="e0daa-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e0daa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[adding-controller1](../../includes/mvc-intro/adding-controller1.md)]

* <span data-ttu-id="e0daa-105">**ソリューション エクスプローラー**で、**[Controllers] を右クリックし、[追加]、[新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="e0daa-105">In **Solution Explorer**, right-click **Controllers > Add > New Item**</span></span>

![コンテキスト メニュー](adding-controller/_static/add_controller.png)

* <span data-ttu-id="e0daa-107">**[MVC コントローラー クラス]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e0daa-107">Select **MVC Controller Class**</span></span>
* <span data-ttu-id="e0daa-108">**[新しい項目の追加]** ダイアログに「**HelloWorldController**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="e0daa-108">In the **Add New Item** dialog, enter **HelloWorldController**.</span></span>

![MVC コント ローラーを追加し、名前を付けます](adding-controller/_static/ac.png)

[!INCLUDE[adding-controller2](../../includes/mvc-intro/adding-controller2.md)]

<span data-ttu-id="e0daa-110">Visual Studio の非デバッグ モード (Ctrl+F5) では、コードの変更後にアプリをビルドする必要がありません。</span><span class="sxs-lookup"><span data-stu-id="e0daa-110">In Visual Studio, in non-debug mode (Ctrl+F5), you don't need to build the app after changing  code.</span></span> <span data-ttu-id="e0daa-111">ファイルを保存し、ブラウザーを更新すれば、変更を確認できます。</span><span class="sxs-lookup"><span data-stu-id="e0daa-111">Just save the file, refresh your browser and you can see the changes.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e0daa-112">[前へ](start-mvc.md)
[次へ](adding-view.md)</span><span class="sxs-lookup"><span data-stu-id="e0daa-112">[Previous](start-mvc.md)
[Next](adding-view.md)</span></span>  
