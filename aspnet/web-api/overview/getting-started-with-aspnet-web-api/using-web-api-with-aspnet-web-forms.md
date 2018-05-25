---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: ASP.NET Web フォームで Web API の使用 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2017
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="fe63c-102">ASP.NET Web フォームで Web API の使用</span><span class="sxs-lookup"><span data-stu-id="fe63c-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="fe63c-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fe63c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fe63c-104">ASP.NET Web API は、ASP.NET MVC にパッケージ化されては簡単に Web API を従来の ASP.NET Web フォーム アプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="fe63c-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="fe63c-105">このチュートリアルでは、手順を示します。</span><span class="sxs-lookup"><span data-stu-id="fe63c-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="fe63c-106">概要</span><span class="sxs-lookup"><span data-stu-id="fe63c-106">Overview</span></span>

<span data-ttu-id="fe63c-107">を Web フォーム アプリケーションで Web API を使用するのには、次の 2 つの主要な手順があります。</span><span class="sxs-lookup"><span data-stu-id="fe63c-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="fe63c-108">派生する Web API コント ローラーを追加、 **ApiController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="fe63c-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="fe63c-109">ルート テーブルを追加、**アプリケーション\_開始**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="fe63c-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="fe63c-110">Web フォーム プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="fe63c-110">Create a Web Forms Project</span></span>

<span data-ttu-id="fe63c-111">Visual Studio を起動し、選択**新しいプロジェクト**から、**開始**ページ。</span><span class="sxs-lookup"><span data-stu-id="fe63c-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="fe63c-112">またはから、**ファイル**メニューの **新規**し**プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="fe63c-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="fe63c-113">**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。</span><span class="sxs-lookup"><span data-stu-id="fe63c-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="fe63c-114">**Visual c#** **Web**です。</span><span class="sxs-lookup"><span data-stu-id="fe63c-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="fe63c-115">プロジェクト テンプレートの一覧で選択**ASP.NET Web フォーム アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="fe63c-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="fe63c-116">プロジェクトの名前を入力し、クリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="fe63c-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="fe63c-117">モデルとコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="fe63c-117">Create the Model and Controller</span></span>

<span data-ttu-id="fe63c-118">このチュートリアルは、同じモデルとコント ローラーとしてのクラスを使用して、[作業の開始](tutorial-your-first-web-api.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="fe63c-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="fe63c-119">最初に、モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="fe63c-119">First, add a model class.</span></span> <span data-ttu-id="fe63c-120">**ソリューション エクスプ ローラー**、プロジェクトを右クリックし **クラスの追加**です。</span><span class="sxs-lookup"><span data-stu-id="fe63c-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="fe63c-121">クラスの製品の名前を次の実装を追加します。</span><span class="sxs-lookup"><span data-stu-id="fe63c-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="fe63c-122">次に、Web API コント ローラーを追加、プロジェクトに、A*コント ローラー* Web API の HTTP 要求を処理するオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="fe63c-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="fe63c-123">**ソリューション エクスプ ローラー**プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="fe63c-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="fe63c-124">選択**新しい項目の追加**です。</span><span class="sxs-lookup"><span data-stu-id="fe63c-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="fe63c-125">**インストールされたテンプレート**、展開**Visual c#** 選択**Web**です。</span><span class="sxs-lookup"><span data-stu-id="fe63c-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="fe63c-126">次に、テンプレートの一覧から次のように選択します。 **Web API コント ローラー クラス**です。</span><span class="sxs-lookup"><span data-stu-id="fe63c-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="fe63c-127">コント ローラー"ProductsController"という名前をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="fe63c-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="fe63c-128">**新しい項目の追加**ProductsController.cs をという名前のファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="fe63c-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="fe63c-129">ウィザードが含まれているメソッドを削除し、次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="fe63c-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="fe63c-130">このコント ローラーでコードの詳細については、次を参照してください。、[作業の開始](tutorial-your-first-web-api.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="fe63c-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="fe63c-131">ルーティング情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="fe63c-131">Add Routing Information</span></span>

<span data-ttu-id="fe63c-132">次に、追加 URI ルートのため、フォームの Uri &quot;/api製品/&quot;コント ローラーにルーティングされます。</span><span class="sxs-lookup"><span data-stu-id="fe63c-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="fe63c-133">**ソリューション エクスプ ローラー**、Global.asax Global.asax.cs 分離コード ファイルを開く をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="fe63c-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="fe63c-134">次の追加**を使用して**ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="fe63c-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="fe63c-135">次のコードを追加、**アプリケーション\_開始**メソッド。</span><span class="sxs-lookup"><span data-stu-id="fe63c-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="fe63c-136">ルーティング テーブルの詳細については、次を参照してください。 [ASP.NET Web API でルーティング](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="fe63c-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="fe63c-137">クライアント側の AJAX を追加します。</span><span class="sxs-lookup"><span data-stu-id="fe63c-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="fe63c-138">Web クライアントがアクセスできる API を作成する必要がありますすべてです。</span><span class="sxs-lookup"><span data-stu-id="fe63c-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="fe63c-139">これで、API の呼び出しに jQuery を使用する HTML ページを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="fe63c-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="fe63c-140">マスター ページを確認してください (たとえば、 *Site.Master*) が含まれています、`ContentPlaceHolder`で`ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="fe63c-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="fe63c-141">Default.aspx ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="fe63c-141">Open the file Default.aspx.</span></span> <span data-ttu-id="fe63c-142">ように、メイン コンテンツのセクションでは、定型のテキストを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="fe63c-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="fe63c-143">次に、jQuery ソース ファイルへの参照を追加、`HeaderContent`セクション。</span><span class="sxs-lookup"><span data-stu-id="fe63c-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="fe63c-144">注: 簡単に、スクリプト参照を追加できますをドラッグ アンド ドロップ ファイルから**ソリューション エクスプ ローラー**コード エディター ウィンドウにします。</span><span class="sxs-lookup"><span data-stu-id="fe63c-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="fe63c-145">JQuery スクリプト タグの下には、次のスクリプト ブロックを追加します。</span><span class="sxs-lookup"><span data-stu-id="fe63c-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="fe63c-146">このスクリプトで AJAX 要求は、ドキュメントが読み込まれると、 &quot;api/製品&quot;です。</span><span class="sxs-lookup"><span data-stu-id="fe63c-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="fe63c-147">要求は、JSON 形式での製品の一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="fe63c-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="fe63c-148">スクリプトは、製品情報を HTML テーブルに追加します。</span><span class="sxs-lookup"><span data-stu-id="fe63c-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="fe63c-149">アプリケーションを実行するときに次のようになります。</span><span class="sxs-lookup"><span data-stu-id="fe63c-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
