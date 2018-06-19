---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: コント ローラー (c#) の作成 |Microsoft ドキュメント
author: StephenWalther
description: このチュートリアルでは、Stephen Walther は、ASP.NET MVC アプリケーション コント ローラーを追加する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 86966f1064d09419e2102542c6d14c4162d153e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868361"
---
<a name="creating-a-controller-c"></a><span data-ttu-id="79a0f-103">コント ローラー (c#) の作成</span><span class="sxs-lookup"><span data-stu-id="79a0f-103">Creating a Controller (C#)</span></span>
====================
<span data-ttu-id="79a0f-104">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="79a0f-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="79a0f-105">このチュートリアルでは、Stephen Walther は、ASP.NET MVC アプリケーション コント ローラーを追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="79a0f-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="79a0f-106">このチュートリアルの目的では、作成する新しい ASP.NET MVC コント ローラーについて説明します。</span><span class="sxs-lookup"><span data-stu-id="79a0f-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="79a0f-107">Visual Studio コント ローラーの追加 メニュー オプションを使用して、クラス ファイルを手動で作成することで、コント ローラーを作成する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="79a0f-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="79a0f-108">使用して、コント ローラーのメニュー オプションを追加</span><span class="sxs-lookup"><span data-stu-id="79a0f-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="79a0f-109">新しいコント ローラーを作成する最も簡単な方法は、Visual Studio ソリューション エクスプ ローラー ウィンドウで、Controllers フォルダーを右クリックし、選択する、**追加、コント ローラー**メニュー オプション (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="79a0f-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="79a0f-110">このメニュー オプションを選択すると開きます、**コント ローラーの追加**ダイアログ (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="79a0f-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="79a0f-111">[![[新しいプロジェクト] ダイアログ ボックス](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="79a0f-111">[![The New Project dialog box](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span></span>

<span data-ttu-id="79a0f-112">**図 01**: 新しいコント ローラーの追加 ([フルサイズのイメージを表示するをクリックして](creating-a-controller-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="79a0f-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-cs/_static/image2.png))</span></span>


<span data-ttu-id="79a0f-113">[![[新しいプロジェクト] ダイアログ ボックス](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="79a0f-113">[![The New Project dialog box](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span></span>

<span data-ttu-id="79a0f-114">**図 02**:「コント ローラーの追加 ダイアログ ([フルサイズのイメージを表示するには、をクリックして](creating-a-controller-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="79a0f-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-cs/_static/image4.png))</span></span>


<span data-ttu-id="79a0f-115">コント ローラー名の最初の部分が強調表示されていることを確認、**コント ローラーの追加**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="79a0f-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="79a0f-116">各コント ローラー名がサフィックスで終わる必要があります*コント ローラー*です。</span><span class="sxs-lookup"><span data-stu-id="79a0f-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="79a0f-117">たとえば、という名前のコント ローラーを作成することができます*ProductController*が名前付きコント ローラー以外*製品*です。</span><span class="sxs-lookup"><span data-stu-id="79a0f-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="79a0f-118">不足しているコント ローラーを作成する場合、*コント ローラー*コント ローラーを起動することはできませんし、サフィックスが付いています。</span><span class="sxs-lookup"><span data-stu-id="79a0f-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="79a0f-119">この--命このようなミスを行った後に膨大な時間は無駄になるしました。</span><span class="sxs-lookup"><span data-stu-id="79a0f-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="79a0f-120">**1 - Controllers\ProductController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="79a0f-120">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

<span data-ttu-id="79a0f-121">常に、コント ローラーのフォルダーに、コント ローラーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="79a0f-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="79a0f-122">それ以外の場合、ASP.NET MVC の規則に違反するし、アプリケーションの理解が難しくの時間がある他の開発者。</span><span class="sxs-lookup"><span data-stu-id="79a0f-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="79a0f-123">スキャフォールディング アクション メソッド</span><span class="sxs-lookup"><span data-stu-id="79a0f-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="79a0f-124">アクション メソッドの作成、更新、および詳細情報を自動的に生成するオプションがあるコント ローラーを作成するときに (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="79a0f-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="79a0f-125">このオプションを選択したリスト 2 でコント ローラーのクラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="79a0f-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="79a0f-126">[![アクション メソッドを自動的に作成します。](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="79a0f-126">[![Creating action methods automatically](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span></span>

<span data-ttu-id="79a0f-127">**図 03**: アクション メソッドを自動的に作成する ([フルサイズのイメージを表示するをクリックして](creating-a-controller-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="79a0f-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-cs/_static/image6.png))</span></span>


<span data-ttu-id="79a0f-128">**2 - Controllers\CustomerController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="79a0f-128">**Listing 2 - Controllers\CustomerController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

<span data-ttu-id="79a0f-129">これらの生成されたメソッドは、スタブ メソッドです。</span><span class="sxs-lookup"><span data-stu-id="79a0f-129">These generated methods are stub methods.</span></span> <span data-ttu-id="79a0f-130">作成、更新、および自分で顧客の詳細を表示、実際のロジックを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="79a0f-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="79a0f-131">ただし、スタブ メソッドは、便利な開始点。</span><span class="sxs-lookup"><span data-stu-id="79a0f-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="79a0f-132">コント ローラー クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="79a0f-132">Creating a Controller Class</span></span>

<span data-ttu-id="79a0f-133">ASP.NET MVC コント ローラーは、クラスだけです。</span><span class="sxs-lookup"><span data-stu-id="79a0f-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="79a0f-134">場合は、便利な Visual Studio コント ローラーのスキャフォールディングを無視し、コント ローラー クラスを手動で作成できます。</span><span class="sxs-lookup"><span data-stu-id="79a0f-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="79a0f-135">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="79a0f-135">Follow these steps:</span></span>

1. <span data-ttu-id="79a0f-136">Controllers フォルダーを右クリックし、メニュー オプションを選択**追加]、[新しい項目の**を選択し、**クラス**テンプレート (図 4 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="79a0f-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="79a0f-137">新しいクラス PersonController.cs の名前を指定し、をクリックして、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="79a0f-137">Name the new class PersonController.cs and click the **Add** button.</span></span>
3. <span data-ttu-id="79a0f-138">基本 System.Web.Mvc.Controller クラス (3 のリストを参照) からクラスを継承できるように、生成されたクラス ファイルを変更します。</span><span class="sxs-lookup"><span data-stu-id="79a0f-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="79a0f-139">[![新しいクラスを作成します。](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="79a0f-139">[![Creating a new class](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span></span>

<span data-ttu-id="79a0f-140">**図 04**: 新しいクラスを作成する ([フルサイズのイメージを表示するをクリックして](creating-a-controller-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="79a0f-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-cs/_static/image8.png))</span></span>


<span data-ttu-id="79a0f-141">**3 - Controllers\PersonController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="79a0f-141">**Listing 3 - Controllers\PersonController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

<span data-ttu-id="79a0f-142">リスト 3 でのコント ローラーは、文字列"Hello World!"を返します Index() をという名前の 1 つのアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="79a0f-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="79a0f-143">このコント ローラーのアクションを起動するには、アプリケーションを実行して、次のように URL を要求します。</span><span class="sxs-lookup"><span data-stu-id="79a0f-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="79a0f-144">ASP.NET 開発サーバーでは、ランダムなポート番号 (たとえば、40071) を使用します。</span><span class="sxs-lookup"><span data-stu-id="79a0f-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="79a0f-145">コント ローラーを起動する URL を入力するときに、右側のポート番号を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="79a0f-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="79a0f-146">Windows 通知領域 (下の画面の右側) で ASP.NET 開発サーバーのアイコンの上にマウスを置くことによって、ポート番号を指定できます。</span><span class="sxs-lookup"><span data-stu-id="79a0f-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="79a0f-147">[前へ](adding-dynamic-content-to-a-cached-page-cs.md)
> [次へ](creating-an-action-cs.md)</span><span class="sxs-lookup"><span data-stu-id="79a0f-147">[Previous](adding-dynamic-content-to-a-cached-page-cs.md)
[Next](creating-an-action-cs.md)</span></span>
