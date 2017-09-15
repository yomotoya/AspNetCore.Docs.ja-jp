---
title: "コントローラーを追加する"
author: rick-anderson
description: "単純な ASP.NET Core MVC アプリにコントローラーを追加する方法"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: get-started-article
ms.assetid: e04b6665-d0de-4d99-b78f-d6a0c4634a87
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-controller
ms.openlocfilehash: 1eee67dbde6a9e63b341de5b420ccd3d7ab9ae86
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="adding-a-controller-to-a-aspnet-core-mvc-app-with-visual-studio"></a><span data-ttu-id="c91f5-104">Visual Studio を使用して ASP.NET Core MVC アプリにコントローラーを追加する</span><span class="sxs-lookup"><span data-stu-id="c91f5-104">Adding a controller to a ASP.NET Core MVC app with Visual Studio</span></span>

<span data-ttu-id="c91f5-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c91f5-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[adding-controller1](../../includes/mvc-intro/adding-controller1.md)]

* <span data-ttu-id="c91f5-106">**ソリューション エクスプローラー**で、**[Controllers] を右クリックし、[追加]、[新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="c91f5-106">In **Solution Explorer**, right-click **Controllers > Add > New Item**</span></span>

![コンテキスト メニュー](adding-controller/_static/add_controller.png)

* <span data-ttu-id="c91f5-108">**[MVC コントローラー クラス]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c91f5-108">Select **MVC Controller Class**</span></span>
* <span data-ttu-id="c91f5-109">**[新しい項目の追加]** ダイアログに「**HelloWorldController**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="c91f5-109">In the **Add New Item** dialog, enter **HelloWorldController**.</span></span>

![MVC コント ローラーを追加し、名前を付けます](adding-controller/_static/ac.png)

[!INCLUDE[adding-controller2](../../includes/mvc-intro/adding-controller2.md)]

<span data-ttu-id="c91f5-111">Visual Studio の非デバッグ モード (Ctrl+F5) では、コードの変更後にアプリをビルドする必要がありません。</span><span class="sxs-lookup"><span data-stu-id="c91f5-111">In Visual Studio, in non-debug mode (Ctrl+F5), you don't need to build the app after changing  code.</span></span> <span data-ttu-id="c91f5-112">ファイルを保存し、ブラウザーを更新すれば、変更を確認できます。</span><span class="sxs-lookup"><span data-stu-id="c91f5-112">Just save the file, refresh your browser and you can see the changes.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c91f5-113">[前へ](start-mvc.md)
[次へ](adding-view.md)</span><span class="sxs-lookup"><span data-stu-id="c91f5-113">[Previous](start-mvc.md)
[Next](adding-view.md)</span></span>  
