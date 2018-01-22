---
title: "コントローラーを追加する"
author: rick-anderson
description: "単純な ASP.NET Core MVC アプリにコントローラーを追加する方法"
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-controller
ms.openlocfilehash: daa72762ce5898bdb5546a9d33fecd3f5ccce5a5
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-controller-to-a-aspnet-core-mvc-app-with-visual-studio"></a><span data-ttu-id="6f2f4-103">Visual Studio を使用して ASP.NET Core MVC アプリにコントローラーを追加する</span><span class="sxs-lookup"><span data-stu-id="6f2f4-103">Adding a controller to a ASP.NET Core MVC app with Visual Studio</span></span>

<span data-ttu-id="6f2f4-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6f2f4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[adding-controller1](../../includes/mvc-intro/adding-controller1.md)]

* <span data-ttu-id="6f2f4-105">**ソリューション エクスプローラー**で、**[Controllers] を右クリックし、[追加]、[新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="6f2f4-105">In **Solution Explorer**, right-click **Controllers > Add > New Item**</span></span>

![コンテキスト メニュー](adding-controller/_static/add_controller.png)

* <span data-ttu-id="6f2f4-107">**[MVC コントローラー クラス]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6f2f4-107">Select **MVC Controller Class**</span></span>
* <span data-ttu-id="6f2f4-108">**[新しい項目の追加]** ダイアログに「**HelloWorldController**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="6f2f4-108">In the **Add New Item** dialog, enter **HelloWorldController**.</span></span>

![MVC コント ローラーを追加し、名前を付けます](adding-controller/_static/ac.png)

[!INCLUDE[adding-controller2](../../includes/mvc-intro/adding-controller2.md)]

<span data-ttu-id="6f2f4-110">Visual Studio の非デバッグ モード (Ctrl+F5) では、コードの変更後にアプリをビルドする必要がありません。</span><span class="sxs-lookup"><span data-stu-id="6f2f4-110">In Visual Studio, in non-debug mode (Ctrl+F5), you don't need to build the app after changing  code.</span></span> <span data-ttu-id="6f2f4-111">ファイルを保存し、ブラウザーを更新すれば、変更を確認できます。</span><span class="sxs-lookup"><span data-stu-id="6f2f4-111">Just save the file, refresh your browser and you can see the changes.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6f2f4-112">[前へ](start-mvc.md)
[次へ](adding-view.md)</span><span class="sxs-lookup"><span data-stu-id="6f2f4-112">[Previous](start-mvc.md)
[Next](adding-view.md)</span></span>  
