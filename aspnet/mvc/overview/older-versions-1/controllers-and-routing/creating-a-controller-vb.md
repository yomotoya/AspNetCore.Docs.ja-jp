---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: コント ローラー (VB) の作成 |Microsoft Docs
author: StephenWalther
description: このチュートリアルでは、Stephen Walther は、ASP.NET MVC アプリケーションをコント ローラーを追加する方法について説明します。
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c14ceff17d15b1a8460bad41429b82cb9504a24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838357"
---
<a name="creating-a-controller-vb"></a><span data-ttu-id="494d0-103">コント ローラー (VB) の作成</span><span class="sxs-lookup"><span data-stu-id="494d0-103">Creating a Controller (VB)</span></span>
====================
<span data-ttu-id="494d0-104">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="494d0-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="494d0-105">このチュートリアルでは、Stephen Walther は、ASP.NET MVC アプリケーションをコント ローラーを追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="494d0-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="494d0-106">このチュートリアルの目的では、作成する新しい ASP.NET MVC コント ローラーについて説明します。</span><span class="sxs-lookup"><span data-stu-id="494d0-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="494d0-107">Visual Studio コント ローラーの追加 メニュー オプションを使用して、クラス ファイルを手動で作成して、コント ローラーを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="494d0-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="494d0-108">使用して、コント ローラーのメニュー オプションの追加</span><span class="sxs-lookup"><span data-stu-id="494d0-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="494d0-109">新しいコント ローラーを作成する最も簡単な方法は、Visual Studio ソリューション エクスプ ローラー ウィンドウで、Controllers フォルダーを右クリックして、選択、**追加、コント ローラー**メニュー オプション (図 1 参照)。</span><span class="sxs-lookup"><span data-stu-id="494d0-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="494d0-110">このメニュー オプションを選択すると表示、**コント ローラーの追加**ダイアログ (図 2 参照)。</span><span class="sxs-lookup"><span data-stu-id="494d0-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="494d0-111">[![[新しいプロジェクト] ダイアログ ボックス](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="494d0-111">[![The New Project dialog box](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span></span>

<span data-ttu-id="494d0-112">**図 01**: 新しいコント ローラーの追加 ([フルサイズの画像を表示する をクリックします](creating-a-controller-vb/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="494d0-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-vb/_static/image2.png))</span></span>


<span data-ttu-id="494d0-113">[![[新しいプロジェクト] ダイアログ ボックス](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="494d0-113">[![The New Project dialog box](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span></span>

<span data-ttu-id="494d0-114">**図 02**:、コント ローラーの追加 ダイアログ ([フルサイズの画像を表示する をクリックします](creating-a-controller-vb/_static/image4.png))。</span><span class="sxs-lookup"><span data-stu-id="494d0-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-vb/_static/image4.png))</span></span>


<span data-ttu-id="494d0-115">コント ローラー名の最初の部分が強調表示されますが、**コント ローラーの追加**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="494d0-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="494d0-116">サフィックスを持つすべてのコント ローラー名が終了する必要があります*コント ローラー*します。</span><span class="sxs-lookup"><span data-stu-id="494d0-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="494d0-117">たとえば、という名前のコント ローラーを作成することができます *「productcontroller」* がという名前のコント ローラー以外*製品*します。</span><span class="sxs-lookup"><span data-stu-id="494d0-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="494d0-118">不足しているコント ローラーを作成する場合、*コント ローラー*コント ローラーを起動することはできませんし、サフィックスが付いています。</span><span class="sxs-lookup"><span data-stu-id="494d0-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="494d0-119">これを行わない--この間違いを犯す後に自分の人生に膨大な時間を無駄しました。</span><span class="sxs-lookup"><span data-stu-id="494d0-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="494d0-120">**1 - Controllers\ProductController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="494d0-120">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

<span data-ttu-id="494d0-121">Controllers フォルダーで、常にコント ローラーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="494d0-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="494d0-122">それ以外の場合、ASP.NET MVC の規則に違反するし、他の開発者がアプリケーションを理解することが難しく時間が必要があります。</span><span class="sxs-lookup"><span data-stu-id="494d0-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="494d0-123">アクション メソッドのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="494d0-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="494d0-124">コント ローラーを作成するときに自動的に作成、更新、および詳細のアクション メソッドを生成するオプションがある (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="494d0-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="494d0-125">このオプションを選択する場合は、リスト 2 でコント ローラー クラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="494d0-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="494d0-126">[![アクション メソッドを自動的に作成します。](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="494d0-126">[![Creating action methods automatically](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span></span>

<span data-ttu-id="494d0-127">**図 03**: アクション メソッドを自動的に作成する ([フルサイズの画像を表示する をクリックします](creating-a-controller-vb/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="494d0-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-vb/_static/image6.png))</span></span>


<span data-ttu-id="494d0-128">**2 - Controllers\CustomerController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="494d0-128">**Listing 2 - Controllers\CustomerController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

<span data-ttu-id="494d0-129">これらの生成されたメソッドはスタブ メソッドです。</span><span class="sxs-lookup"><span data-stu-id="494d0-129">These generated methods are stub methods.</span></span> <span data-ttu-id="494d0-130">作成、更新、および自分で顧客の詳細を示す実際のロジックを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="494d0-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="494d0-131">優れた出発点に、スタブ メソッドが提供します。</span><span class="sxs-lookup"><span data-stu-id="494d0-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="494d0-132">コント ローラー クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="494d0-132">Creating a Controller Class</span></span>

<span data-ttu-id="494d0-133">ASP.NET MVC コント ローラーは、単なるクラスです。</span><span class="sxs-lookup"><span data-stu-id="494d0-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="494d0-134">場合は、便利な Visual Studio コント ローラーのスキャフォールディングを無視し、コント ローラー クラスを手動で作成できます。</span><span class="sxs-lookup"><span data-stu-id="494d0-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="494d0-135">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="494d0-135">Follow these steps:</span></span>

1. <span data-ttu-id="494d0-136">Controllers フォルダーを右クリックし、メニュー オプションを選択**追加]、[新しい項目の**を選択し、**クラス**テンプレート (図 4 参照)。</span><span class="sxs-lookup"><span data-stu-id="494d0-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="494d0-137">新しいクラス PersonController.vb の名前を指定し、をクリックして、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="494d0-137">Name the new class PersonController.vb and click the **Add** button.</span></span>
3. <span data-ttu-id="494d0-138">(リスト 3 参照)、基本 System.Web.Mvc.Controller クラスからクラスを継承するので、結果として得られるクラス ファイルを変更します。</span><span class="sxs-lookup"><span data-stu-id="494d0-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="494d0-139">[![新しいクラスを作成します。](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="494d0-139">[![Creating a new class](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span></span>

<span data-ttu-id="494d0-140">**図 04**: 新しいクラスを作成する ([フルサイズの画像を表示する をクリックします](creating-a-controller-vb/_static/image8.png))。</span><span class="sxs-lookup"><span data-stu-id="494d0-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-vb/_static/image8.png))</span></span>


<span data-ttu-id="494d0-141">**3 - Controllers\PersonController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="494d0-141">**Listing 3 - Controllers\PersonController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

<span data-ttu-id="494d0-142">リスト 3 のコント ローラーは、文字列"Hello World!"を返します Index() という名前の 1 つのアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="494d0-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="494d0-143">このコント ローラー アクションを起動するには、アプリケーションを実行して、次のような URL を要求します。</span><span class="sxs-lookup"><span data-stu-id="494d0-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="494d0-144">ASP.NET 開発サーバーは、ランダムなポート番号 (たとえば、40071) を使用します。</span><span class="sxs-lookup"><span data-stu-id="494d0-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="494d0-145">コント ローラーを呼び出す URL を入力すると、右側のポート番号を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="494d0-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="494d0-146">Windows 通知領域 (画面の下部にある-右側) で、ASP.NET 開発サーバーのアイコンの上にマウスを置くことによって、ポート番号を指定できます。</span><span class="sxs-lookup"><span data-stu-id="494d0-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="494d0-147">[前へ](adding-dynamic-content-to-a-cached-page-vb.md)
> [次へ](creating-an-action-vb.md)</span><span class="sxs-lookup"><span data-stu-id="494d0-147">[Previous](adding-dynamic-content-to-a-cached-page-vb.md)
[Next](creating-an-action-vb.md)</span></span>
