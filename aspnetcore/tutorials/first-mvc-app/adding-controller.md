---
title: ASP.NET Core MVC アプリにコントローラーを追加する
author: rick-anderson
description: 単純な ASP.NET Core MVC アプリにコントローラーを追加する方法について説明します。
ms.author: riande
ms.date: 02/28/2017
uid: tutorials/first-mvc-app/adding-controller
ms.openlocfilehash: 5686c4de6960e49714b822200212ea66cfa4b37c
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938421"
---
# <a name="add-a-controller-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="c38f0-103">ASP.NET Core MVC アプリにコントローラーを追加する</span><span class="sxs-lookup"><span data-stu-id="c38f0-103">Add a controller to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="c38f0-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c38f0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [adding-controller1](~/includes/mvc-intro/adding-controller1.md)]

* <span data-ttu-id="c38f0-105">**ソリューション エクスプローラー**で、**[Controllers] を右クリックし、[追加]、[新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="c38f0-105">In **Solution Explorer**, right-click **Controllers > Add > New Item**</span></span>

![コンテキスト メニュー](adding-controller/_static/add_controller.png)

* <span data-ttu-id="c38f0-107">**[コントローラー クラス]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c38f0-107">Select **Controller Class**</span></span>
* <span data-ttu-id="c38f0-108">**[新しい項目の追加]** ダイアログに「**HelloWorldController**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="c38f0-108">In the **Add New Item** dialog, enter **HelloWorldController**.</span></span>

![MVC コント ローラーを追加し、名前を付けます](adding-controller/_static/ac.png)

[!INCLUDE [adding-controller2](~/includes/mvc-intro/adding-controller2.md)]

<span data-ttu-id="c38f0-110">Visual Studio の非デバッグ モード (Ctrl+F5) では、コードの変更後にアプリをビルドする必要がありません。</span><span class="sxs-lookup"><span data-stu-id="c38f0-110">In Visual Studio, in non-debug mode (Ctrl+F5), you don't need to build the app after changing  code.</span></span> <span data-ttu-id="c38f0-111">ファイルを保存し、ブラウザーを更新すれば、変更を確認できます。</span><span class="sxs-lookup"><span data-stu-id="c38f0-111">Just save the file, refresh your browser and you can see the changes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c38f0-112">[前へ](start-mvc.md)
> [次へ](adding-view.md)</span><span class="sxs-lookup"><span data-stu-id="c38f0-112">[Previous](start-mvc.md)
[Next](adding-view.md)</span></span>
