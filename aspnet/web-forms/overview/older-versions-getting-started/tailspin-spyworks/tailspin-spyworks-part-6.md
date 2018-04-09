---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'パート 6: ASP.NET メンバーシップ |Microsoft ドキュメント'
author: JoeStagner
description: このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 6 部では、ASP.NET メンバーシップを追加します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 83e9bc780ea8face3e0f55fdf8c00e13b60f80a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="0f9b0-104">パート 6: ASP.NET メンバーシップ</span><span class="sxs-lookup"><span data-stu-id="0f9b0-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="0f9b0-105">によって[行える](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="0f9b0-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="0f9b0-106">Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="0f9b0-107">ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="0f9b0-108">このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="0f9b0-109">第 6 部では、ASP.NET メンバーシップを追加します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="0f9b0-110">ASP.NET メンバーシップの使用</span><span class="sxs-lookup"><span data-stu-id="0f9b0-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="0f9b0-111">[セキュリティ] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="0f9b0-112">フォーム認証が使用されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="0f9b0-113">"Create User"リンクを使用すると、数人のユーザーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="0f9b0-114">完了したら、ソリューション エクスプ ローラー ウィンドウを参照してくださいし、ビューを更新します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="0f9b0-115">なお、ASPNETDB です。MDF 正常に用意されています。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="0f9b0-116">このファイルには、メンバーシップと同様に、ASP.NET のコア サービスをサポートするためにテーブルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="0f9b0-117">今すぐチェック アウト プロセスの実装を開始できます。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="0f9b0-118">まず CheckOut.aspx ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="0f9b0-119">CheckOut.aspx ページへのアクセスを制限は、ユーザーとログイン ページにログインしていないリダイレクトのユーザーに記録するためにログオンしているユーザーに利用可能なのみする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="0f9b0-120">これを行う、次の web.config ファイルの configuration セクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="0f9b0-121">ASP.NET Web フォーム アプリケーションのテンプレートは自動的に、web.config ファイルに認証セクションを追加し、既定のログイン ページを確立します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="0f9b0-122">Login.aspx 分離コード ファイルをユーザーがログインすると、匿名のショッピング カートを移行するお変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="0f9b0-123">ページを変更する\_読み込みイベントは次のようにします。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="0f9b0-124">新しくログイン済みユーザー セッション名を設定および MyShoppingCart クラス MigrateCart メソッドを呼び出すことによって、ユーザーのショッピング カートに一時的なセッション id を変更する次のように「当てはまります」イベント ハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="0f9b0-125">(.Cs ファイルで実装される)</span><span class="sxs-lookup"><span data-stu-id="0f9b0-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="0f9b0-126">次のように、MigrateCart() メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="0f9b0-127">Checkout.aspx で使用されます、EntityDataSource および GridView 当社のチェック アウト ページで、買い物カゴのページで行ったようになります。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="0f9b0-128">ユーザーの GridView コントロールが MyList を名前付き"ondatabound"イベント ハンドラーを指定することに注意してください\_RowDataBound では次のような場合は、そのイベント ハンドラーを実装します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="0f9b0-129">このメソッドが、集計の途中経過、ショッピング カートと各行がバインドされて GridView の一番下の行を更新します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="0f9b0-130">この段階で配置するための「確認」プレゼンテーションが実装されました。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="0f9b0-131">ページに数行のコードを追加することで空カート シナリオを処理してみましょう\_Load イベント。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="0f9b0-132">ユーザーが「送信」ボタンをクリックして送信ボタンの Click イベント ハンドラーで、次のコードは実行されます。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="0f9b0-133">注文の送信プロセスの「肉」MyShoppingCart クラスの SubmitOrder() メソッドの実装を開始します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="0f9b0-134">SubmitOrder を行います。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-134">SubmitOrder will:</span></span>

- <span data-ttu-id="0f9b0-135">ショッピング カートにすべての行項目を行い、新しい注文レコードと関連付けられている OrderDetails レコードの作成に使用します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="0f9b0-136">出荷日を計算します。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="0f9b0-137">ショッピング カートをオフにします。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="0f9b0-138">このサンプル アプリケーションの目的で、出荷日を現在の日付の 2 日を追加するだけで計算されます。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="0f9b0-139">今すぐアプリケーションを実行して、開始から終了までショッピング プロセスをテストすることが許可されます。</span><span class="sxs-lookup"><span data-stu-id="0f9b0-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0f9b0-140">[前へ](tailspin-spyworks-part-5.md)
> [次へ](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="0f9b0-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
