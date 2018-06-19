---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'パート 4: 管理ビューを追加する |Microsoft ドキュメント'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: cbf42f1dbd744d7b85dde7d2dcd99a13c6208a13
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879531"
---
<a name="part-4-adding-an-admin-view"></a><span data-ttu-id="6ab75-102">パート 4: 管理ビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="6ab75-102">Part 4: Adding an Admin View</span></span>
====================
<span data-ttu-id="6ab75-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6ab75-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6ab75-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="6ab75-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="6ab75-105">管理ビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="6ab75-105">Add an Admin View</span></span>

<span data-ttu-id="6ab75-106">今すぐおがクライアント側に有効にするし、管理コント ローラーからデータを使用できるページを追加します。</span><span class="sxs-lookup"><span data-stu-id="6ab75-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="6ab75-107">ページは作成、編集、またはコント ローラーに AJAX 要求を送信することによって、製品を削除するようにします。</span><span class="sxs-lookup"><span data-stu-id="6ab75-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="6ab75-108">ソリューション エクスプ ローラーで Controllers フォルダーを展開し、HomeController.cs をという名前のファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="6ab75-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="6ab75-109">このファイルには、ある MVC コント ローラーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6ab75-109">This file contains an MVC controller.</span></span> <span data-ttu-id="6ab75-110">という名前のメソッドを追加`Admin`:</span><span class="sxs-lookup"><span data-stu-id="6ab75-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="6ab75-111">**HttpRouteUrl**メソッドは、web API への URI を作成しこれに格納ビュー バッグ後でします。</span><span class="sxs-lookup"><span data-stu-id="6ab75-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="6ab75-112">次に、カーソルの位置のテキスト内で、`Admin`アクション メソッド、し、右クリックして選択**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="6ab75-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="6ab75-113">これが表示されます、**ビューの追加**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="6ab75-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="6ab75-114">**ビューの追加**ダイアログ ボックスで、名前、ビュー"Admin"です。</span><span class="sxs-lookup"><span data-stu-id="6ab75-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="6ab75-115">チェック ボックスをオン**厳密に型指定されたビューを作成する**です。</span><span class="sxs-lookup"><span data-stu-id="6ab75-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="6ab75-116">**モデル クラス**、"Product (ProductStore.Models)"を選択します。</span><span class="sxs-lookup"><span data-stu-id="6ab75-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="6ab75-117">既定値として、その他のすべてのオプションのままにします。</span><span class="sxs-lookup"><span data-stu-id="6ab75-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="6ab75-118">クリックすると**追加**ビュー/ホーム Admin.cshtml をという名前のファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="6ab75-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="6ab75-119">このファイルを開き、次の HTML を追加します。</span><span class="sxs-lookup"><span data-stu-id="6ab75-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="6ab75-120">この HTML ページの構造を定義するが、機能は、ワイヤード クライアントをまだです。</span><span class="sxs-lookup"><span data-stu-id="6ab75-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="6ab75-121">[管理] ページへのリンクを作成します。</span><span class="sxs-lookup"><span data-stu-id="6ab75-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="6ab75-122">ソリューション エクスプ ローラーで、Views フォルダーを展開し、共有フォルダーを展開します。</span><span class="sxs-lookup"><span data-stu-id="6ab75-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="6ab75-123">という名前のファイルを開く\_Layout.cshtml です。</span><span class="sxs-lookup"><span data-stu-id="6ab75-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="6ab75-124">検索、 **ul** id を持つ要素 =「メニュー」とは、管理ビューのアクション リンク。</span><span class="sxs-lookup"><span data-stu-id="6ab75-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="6ab75-125">サンプル プロジェクトで変更したいくつか他の表面的な「ここにロゴ」の文字列を置き換えることなどです。</span><span class="sxs-lookup"><span data-stu-id="6ab75-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="6ab75-126">これらのアプリケーションの機能は影響しません。</span><span class="sxs-lookup"><span data-stu-id="6ab75-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="6ab75-127">プロジェクトをダウンロードして、ファイルを比較することができます。</span><span class="sxs-lookup"><span data-stu-id="6ab75-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="6ab75-128">アプリケーションを実行し、ホーム ページの上部に表示される"Admin"リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="6ab75-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="6ab75-129">[管理] ページは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="6ab75-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="6ab75-130">右側のようになりましたが、ページ処理しません。</span><span class="sxs-lookup"><span data-stu-id="6ab75-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="6ab75-131">次のセクションで Knockout.js ダイナミック UI の作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="6ab75-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="6ab75-132">承認を追加します。</span><span class="sxs-lookup"><span data-stu-id="6ab75-132">Add Authorization</span></span>

<span data-ttu-id="6ab75-133">[管理] ページがサイトにアクセスするすべてのユーザーに現在アクセスできません。</span><span class="sxs-lookup"><span data-stu-id="6ab75-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="6ab75-134">管理者に権限を制限するこれを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="6ab75-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="6ab75-135">"Administrator"ロールと管理者ユーザーを追加することで開始します。</span><span class="sxs-lookup"><span data-stu-id="6ab75-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="6ab75-136">ソリューション エクスプ ローラーで、フィルター フォルダーを展開し、InitializeSimpleMembershipAttribute.cs をという名前のファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="6ab75-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="6ab75-137">検索、`SimpleMembershipInitializer`コンス トラクターです。</span><span class="sxs-lookup"><span data-stu-id="6ab75-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="6ab75-138">呼び出し後**WebSecurity.InitializeDatabaseConnection**、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="6ab75-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="6ab75-139">これは、"Administrator"ロールを追加し、ロールのユーザーを作成する簡便な方法です。</span><span class="sxs-lookup"><span data-stu-id="6ab75-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="6ab75-140">ソリューション エクスプローラで、Controllers フォルダーを展開および HomeController.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="6ab75-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="6ab75-141">追加、 **Authorize**属性を`Admin`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="6ab75-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="6ab75-142">AdminController.cs ファイルを開き、追加、 **Authorize**属性全体を`AdminController`クラスです。</span><span class="sxs-lookup"><span data-stu-id="6ab75-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="6ab75-143">MVC と Web API の両方を定義する**Authorize**異なる名前空間内の属性です。</span><span class="sxs-lookup"><span data-stu-id="6ab75-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="6ab75-144">MVC を使用して**System.Web.Mvc.AuthorizeAttribute**Web API を使用しますが、 **System.Web.Http.AuthorizeAttribute**です。</span><span class="sxs-lookup"><span data-stu-id="6ab75-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="6ab75-145">今すぐ管理者だけでは、[管理] ページを表示できます。</span><span class="sxs-lookup"><span data-stu-id="6ab75-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="6ab75-146">また、管理コント ローラーに HTTP 要求を送信する場合、要求は認証クッキーを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="6ab75-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="6ab75-147">それ以外の場合は、サーバーは HTTP 401 (未承認) 応答を送信します。</span><span class="sxs-lookup"><span data-stu-id="6ab75-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="6ab75-148">このわかります Fiddler でへの GET 要求を送信することによって`http://localhost:*port*/api/admin`です。</span><span class="sxs-lookup"><span data-stu-id="6ab75-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6ab75-149">[前へ](using-web-api-with-entity-framework-part-3.md)
> [次へ](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="6ab75-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
