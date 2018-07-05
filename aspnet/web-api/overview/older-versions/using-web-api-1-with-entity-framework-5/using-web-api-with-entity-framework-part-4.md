---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'パート 4: 管理ビューの追加 |Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 599f684ba200821d7fcd33819937c5a5b5a29ce8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371052"
---
<a name="part-4-adding-an-admin-view"></a><span data-ttu-id="63294-102">パート 4: 管理ビューの追加</span><span class="sxs-lookup"><span data-stu-id="63294-102">Part 4: Adding an Admin View</span></span>
====================
<span data-ttu-id="63294-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="63294-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="63294-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="63294-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="63294-105">管理ビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="63294-105">Add an Admin View</span></span>

<span data-ttu-id="63294-106">ここで、クライアント側を有効にするし、管理コント ローラーからデータを使用できるページを追加します。</span><span class="sxs-lookup"><span data-stu-id="63294-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="63294-107">ページには、作成、編集、またはコント ローラーに AJAX 要求を送信することによって、製品を削除するユーザーはできます。</span><span class="sxs-lookup"><span data-stu-id="63294-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="63294-108">ソリューション エクスプ ローラーでは、Controllers フォルダーを展開し、HomeController.cs をという名前のファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="63294-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="63294-109">このファイルには、MVC コント ローラーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="63294-109">This file contains an MVC controller.</span></span> <span data-ttu-id="63294-110">という名前のメソッドを追加`Admin`:</span><span class="sxs-lookup"><span data-stu-id="63294-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="63294-111">**HttpRouteUrl**メソッドは、web API への URI を作成し、後でビュー バッグに保存すればです。</span><span class="sxs-lookup"><span data-stu-id="63294-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="63294-112">次に、テキスト カーソルを置いて、`Admin`アクション メソッドでは、し、右クリックして選択**ビューの追加**します。</span><span class="sxs-lookup"><span data-stu-id="63294-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="63294-113">これが表示されます、**ビューの追加**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="63294-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="63294-114">**ビューの追加**ダイアログ ボックスで、"Admin"ビューの名前。</span><span class="sxs-lookup"><span data-stu-id="63294-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="63294-115">チェック ボックスをオン**厳密に型指定されたビューを作成する**します。</span><span class="sxs-lookup"><span data-stu-id="63294-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="63294-116">**モデル クラス**、"Product (ProductStore.Models)"を選択します。</span><span class="sxs-lookup"><span data-stu-id="63294-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="63294-117">既定値として、その他のすべてのオプションのままにします。</span><span class="sxs-lookup"><span data-stu-id="63294-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="63294-118">クリックすると**追加**ビュー/ホーム Admin.cshtml という名前のファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="63294-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="63294-119">このファイルを開き、次の HTML を追加します。</span><span class="sxs-lookup"><span data-stu-id="63294-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="63294-120">この HTML ページの構造を定義しますが、機能は、ワイヤード クライアントをまだ。</span><span class="sxs-lookup"><span data-stu-id="63294-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="63294-121">[管理] ページへのリンクを作成します。</span><span class="sxs-lookup"><span data-stu-id="63294-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="63294-122">ソリューション エクスプ ローラーで Views フォルダーを展開し、共有フォルダーを展開します。</span><span class="sxs-lookup"><span data-stu-id="63294-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="63294-123">という名前のファイルを開く\_Layout.cshtml します。</span><span class="sxs-lookup"><span data-stu-id="63294-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="63294-124">検索、 **ul** id を持つ要素 =「メニュー」、および管理ビューのアクション リンク。</span><span class="sxs-lookup"><span data-stu-id="63294-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="63294-125">サンプル プロジェクトでは、いくつかその他の表面的な変更、"Your logo here"という文字列の置換などをしました。</span><span class="sxs-lookup"><span data-stu-id="63294-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="63294-126">これらのアプリケーションの機能は影響はありません。</span><span class="sxs-lookup"><span data-stu-id="63294-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="63294-127">プロジェクトをダウンロードして、ファイルを比較することができます。</span><span class="sxs-lookup"><span data-stu-id="63294-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="63294-128">アプリケーションを実行し、ホーム ページの上部に表示される"Admin"リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="63294-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="63294-129">管理者 ページは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="63294-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="63294-130">右に、ページは何も行いません。</span><span class="sxs-lookup"><span data-stu-id="63294-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="63294-131">次のセクションで Knockout.js 動的 UI の作成に使用します。</span><span class="sxs-lookup"><span data-stu-id="63294-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="63294-132">承認を追加します。</span><span class="sxs-lookup"><span data-stu-id="63294-132">Add Authorization</span></span>

<span data-ttu-id="63294-133">管理者 ページがサイトにアクセスするすべてのユーザーに現在アクセスできません。</span><span class="sxs-lookup"><span data-stu-id="63294-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="63294-134">管理者のアクセス許可を制限するこれを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="63294-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="63294-135">まず、"Administrator"ロールと管理者ユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="63294-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="63294-136">ソリューション エクスプ ローラーでは、フィルター フォルダーを展開し、InitializeSimpleMembershipAttribute.cs という名前のファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="63294-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="63294-137">検索、`SimpleMembershipInitializer`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="63294-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="63294-138">呼び出し後**WebSecurity.InitializeDatabaseConnection**、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="63294-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="63294-139">これは、簡便"Administrator"ロールを追加し、ロールのユーザーを作成する方法です。</span><span class="sxs-lookup"><span data-stu-id="63294-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="63294-140">ソリューション エクスプ ローラーでは、Controllers フォルダーを展開し、HomeController.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="63294-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="63294-141">追加、 **Authorize**属性を`Admin`メソッド。</span><span class="sxs-lookup"><span data-stu-id="63294-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="63294-142">AdminController.cs ファイルを開き、追加、 **Authorize**全体に属性`AdminController`クラス。</span><span class="sxs-lookup"><span data-stu-id="63294-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="63294-143">MVC と Web API の両方を定義**Authorize**異なる名前空間内の属性。</span><span class="sxs-lookup"><span data-stu-id="63294-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="63294-144">MVC を使用して**System.Web.Mvc.AuthorizeAttribute**Web API を使用しますが、 **System.Web.Http.AuthorizeAttribute**します。</span><span class="sxs-lookup"><span data-stu-id="63294-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="63294-145">今すぐ管理者のみでは、管理者 ページを表示できます。</span><span class="sxs-lookup"><span data-stu-id="63294-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="63294-146">また、管理コント ローラーに HTTP 要求を送信する場合、要求は認証クッキーを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="63294-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="63294-147">それ以外の場合は、サーバーが HTTP 401 (未承認) 応答を送信します。</span><span class="sxs-lookup"><span data-stu-id="63294-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="63294-148">これでわかる Fiddler に GET 要求を送信することによって`http://localhost:*port*/api/admin`します。</span><span class="sxs-lookup"><span data-stu-id="63294-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="63294-149">[前へ](using-web-api-with-entity-framework-part-3.md)
> [次へ](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="63294-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
