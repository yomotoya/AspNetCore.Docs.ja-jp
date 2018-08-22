---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'パート 6: ASP.NET メンバーシップ |Microsoft Docs'
author: JoeStagner
description: このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 6 では、ASP.NET メンバーシップを追加します。
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: c847db058bc03115210f12eeb0c3c76fecc8a31e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825842"
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="27b84-104">パート 6: ASP.NET メンバーシップ</span><span class="sxs-lookup"><span data-stu-id="27b84-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="27b84-105">によって[Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="27b84-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="27b84-106">Tailspin Spyworks では、.NET プラットフォーム用の強力でスケーラブルなアプリケーションを作成するはどの非常に単純なを示します。</span><span class="sxs-lookup"><span data-stu-id="27b84-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="27b84-107">ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="27b84-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="27b84-108">このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="27b84-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="27b84-109">パート 6 では、ASP.NET メンバーシップを追加します。</span><span class="sxs-lookup"><span data-stu-id="27b84-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="27b84-110">ASP.NET メンバーシップの使用</span><span class="sxs-lookup"><span data-stu-id="27b84-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="27b84-111">[セキュリティ] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="27b84-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="27b84-112">フォーム認証が使用されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="27b84-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="27b84-113">数人のユーザーを作成するのにには、「ユーザーの作成」リンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="27b84-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="27b84-114">完了したら、ソリューション エクスプ ローラー ウィンドウを参照してくださいし、表示を更新します。</span><span class="sxs-lookup"><span data-stu-id="27b84-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="27b84-115">なお、ASPNETDB します。MDF の問題が作成されました。</span><span class="sxs-lookup"><span data-stu-id="27b84-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="27b84-116">このファイルには、メンバーシップのように ASP.NET のコア サービスをサポートするためにテーブルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="27b84-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="27b84-117">これでチェック アウト プロセスの実装を開始できます。</span><span class="sxs-lookup"><span data-stu-id="27b84-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="27b84-118">まず CheckOut.aspx ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="27b84-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="27b84-119">CheckOut.aspx ページのみへのアクセスは制限がログインしてユーザーとリダイレクトのユーザーがログイン ページにログインしていないためにログオンしているユーザーが使用できる場合があります。</span><span class="sxs-lookup"><span data-stu-id="27b84-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="27b84-120">これを行うには次の web.config ファイルの構成セクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="27b84-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="27b84-121">ASP.NET Web フォーム アプリケーション テンプレートは自動的に、web.config ファイルに、[認証] セクションを追加し、既定のログイン ページが確立されています。</span><span class="sxs-lookup"><span data-stu-id="27b84-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="27b84-122">Login.aspx 分離コード ファイルに、ユーザーがログインすると、匿名のショッピング カートを移行するよう変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="27b84-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="27b84-123">ページを変更する\_読み込みイベントは次のようにします。</span><span class="sxs-lookup"><span data-stu-id="27b84-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="27b84-124">新しくログイン ユーザーに、セッション名を設定し、MyShoppingCart クラス MigrateCart メソッドを呼び出しているユーザーのショッピング カートに一時的なセッション id に変更は、このような「当てはまります」イベント ハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="27b84-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="27b84-125">(.Cs ファイルで実装)</span><span class="sxs-lookup"><span data-stu-id="27b84-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="27b84-126">MigrateCart() メソッドの実装では、このような。</span><span class="sxs-lookup"><span data-stu-id="27b84-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="27b84-127">Checkout.aspx で使用します、EntityDataSource と GridView、チェック アウト ページで、ショッピング カート ページで行ったようにします。</span><span class="sxs-lookup"><span data-stu-id="27b84-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="27b84-128">GridView コントロールが MyList という名前の"ondatabound"イベント ハンドラーを指定することに注意してください\_RowDataBound このような場合は、そのイベント ハンドラーを実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="27b84-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="27b84-129">このメソッドは、累計のショッピング カートの各行がバインドされているし、GridView の一番下の行を更新を保持します。</span><span class="sxs-lookup"><span data-stu-id="27b84-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="27b84-130">この段階で配置する順序を"review"プレゼンテーションを実装しました。</span><span class="sxs-lookup"><span data-stu-id="27b84-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="27b84-131">ページに数行のコードを追加することで、空のカート シナリオを処理しましょう\_Load イベント。</span><span class="sxs-lookup"><span data-stu-id="27b84-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="27b84-132">ユーザーが [送信] ボタンをクリックすると、送信ボタンの Click イベント ハンドラーで、次のコードを実行しますが。</span><span class="sxs-lookup"><span data-stu-id="27b84-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="27b84-133">注文の送信処理の「核心部分」MyShoppingCart クラスの SubmitOrder() メソッドで実装することです。</span><span class="sxs-lookup"><span data-stu-id="27b84-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="27b84-134">SubmitOrder を行います。</span><span class="sxs-lookup"><span data-stu-id="27b84-134">SubmitOrder will:</span></span>

- <span data-ttu-id="27b84-135">ショッピング カート内のすべての行項目を実行し、新しい注文レコードと関連付けられている OrderDetails レコードの作成に使用します。</span><span class="sxs-lookup"><span data-stu-id="27b84-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="27b84-136">出荷日を計算します。</span><span class="sxs-lookup"><span data-stu-id="27b84-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="27b84-137">ショッピング カートをオフにします。</span><span class="sxs-lookup"><span data-stu-id="27b84-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="27b84-138">このサンプル アプリケーションの目的で 2 日間を現在の日付に追加するだけで出荷日を計算します。</span><span class="sxs-lookup"><span data-stu-id="27b84-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="27b84-139">これでアプリケーションを実行している、最初から最後までショッピングのプロセスをテストすることが許可されます。</span><span class="sxs-lookup"><span data-stu-id="27b84-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="27b84-140">[前へ](tailspin-spyworks-part-5.md)
> [次へ](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="27b84-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
