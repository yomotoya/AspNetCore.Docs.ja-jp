---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'パート 1: 概要と、プロジェクトの作成 |Microsoft ドキュメント'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9cdff0cb0cad9adad546c8f8d46ba9b010e1079
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873327"
---
<a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="a0de8-102">パート 1: 概要と、プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-102">Part 1: Overview and Creating the Project</span></span>
====================
<span data-ttu-id="a0de8-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a0de8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a0de8-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="a0de8-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="a0de8-105">Entity Framework は、/、オブジェクト リレーショナル マッピング フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="a0de8-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="a0de8-106">ドメイン オブジェクトをコードでは、リレーショナル データベース内のエンティティにマップされます。</span><span class="sxs-lookup"><span data-stu-id="a0de8-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="a0de8-107">ほとんどの場合、必要はありません、データベース レイヤーについて心配する Entity Framework は、処理をするためです。</span><span class="sxs-lookup"><span data-stu-id="a0de8-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="a0de8-108">コードは、オブジェクトを操作し、データベースへの変更は保持します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="a0de8-109">チュートリアルについて</span><span class="sxs-lookup"><span data-stu-id="a0de8-109">About the Tutorial</span></span>

<span data-ttu-id="a0de8-110">このチュートリアルでは、単純なストア アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="a0de8-111">アプリケーションに 2 つの主要な部分があります。</span><span class="sxs-lookup"><span data-stu-id="a0de8-111">There are two main parts to the application.</span></span> <span data-ttu-id="a0de8-112">通常のユーザーは、製品を表示し、注文を作成します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="a0de8-113">管理者は、作成、削除、または製品を編集できます。</span><span class="sxs-lookup"><span data-stu-id="a0de8-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="a0de8-114">スキルの学習</span><span class="sxs-lookup"><span data-stu-id="a0de8-114">Skills You'll Learn</span></span>

<span data-ttu-id="a0de8-115">学習する内容を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="a0de8-116">ASP.NET Web API で Entity Framework を使用する方法です。</span><span class="sxs-lookup"><span data-stu-id="a0de8-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="a0de8-117">Knockout.js を使用して動的クライアント UI を作成する方法。</span><span class="sxs-lookup"><span data-stu-id="a0de8-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="a0de8-118">Web API でフォーム認証を使用して、ユーザーを認証する方法です。</span><span class="sxs-lookup"><span data-stu-id="a0de8-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="a0de8-119">このチュートリアルでは自己完結型で、次のチュートリアルを最初に読み取る可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a0de8-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="a0de8-120">初めての ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="a0de8-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="a0de8-121">CRUD 操作をサポートする Web API を作成します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="a0de8-122">一定の知識[ASP.NET MVC](../../../../mvc/index.md)も便利です。</span><span class="sxs-lookup"><span data-stu-id="a0de8-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="a0de8-123">概要</span><span class="sxs-lookup"><span data-stu-id="a0de8-123">Overview</span></span>

<span data-ttu-id="a0de8-124">大まかに言えば、アプリケーションのアーキテクチャを次に示します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="a0de8-125">ASP.NET MVC では、クライアントの HTML ページを生成します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="a0de8-126">ASP.NET Web API は、データ (製品および orders) に対する CRUD 操作を公開します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="a0de8-127">Entity Framework では、データベースのエンティティに Web API が使用する (C#) モデルを変換します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="a0de8-128">次の図は、アプリケーションのさまざまなレイヤーでのドメイン オブジェクトの表現方法: データベース層、オブジェクト モデルでは、および最後に、ワイヤ形式は、HTTP 経由でクライアントにデータを送信するために使用します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="a0de8-129">Visual Studio プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="a0de8-130">Visual Web Developer Express または完全バージョンの Visual Studio を使用してチュートリアル プロジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="a0de8-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="a0de8-131">**開始**] ページで [**新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="a0de8-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="a0de8-132">**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。</span><span class="sxs-lookup"><span data-stu-id="a0de8-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="a0de8-133">**Visual c#** **Web**です。</span><span class="sxs-lookup"><span data-stu-id="a0de8-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="a0de8-134">プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web Application**です。</span><span class="sxs-lookup"><span data-stu-id="a0de8-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="a0de8-135">プロジェクト"ProductStore"の名前し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="a0de8-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="a0de8-136">**新しい ASP.NET MVC 4 プロジェクト**ダイアログで、**インターネット アプリケーション** をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="a0de8-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="a0de8-137">「インターネット アプリケーション」テンプレートでは、フォーム認証をサポートする ASP.NET MVC アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="a0de8-138">今すぐアプリケーションを実行する場合は、一部の機能が既にいます。</span><span class="sxs-lookup"><span data-stu-id="a0de8-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="a0de8-139">新しいユーザーは、右上隅に「登録」リンクをクリックして登録できます。</span><span class="sxs-lookup"><span data-stu-id="a0de8-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="a0de8-140">登録済みユーザーは、「ログイン」リンクをクリックしてログインできます。</span><span class="sxs-lookup"><span data-stu-id="a0de8-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="a0de8-141">メンバーシップ情報は、自動的に作成されるデータベースに保存されます。</span><span class="sxs-lookup"><span data-stu-id="a0de8-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="a0de8-142">ASP.NET MVC でのフォーム認証の詳細については、次を参照してください。[チュートリアル: ASP.NET MVC でのフォーム認証を使用して](https://msdn.microsoft.com/library/ff398049(VS.98).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="a0de8-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="a0de8-143">CSS ファイルを更新します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-143">Update the CSS File</span></span>

<span data-ttu-id="a0de8-144">この手順は表面的なが、以前のスクリーン ショットのように表示するページになります。</span><span class="sxs-lookup"><span data-stu-id="a0de8-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="a0de8-145">ソリューション エクスプ ローラーで、コンテンツ フォルダーを展開し、Site.css をという名前のファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="a0de8-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="a0de8-146">次の CSS スタイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="a0de8-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="a0de8-147">次へ</span><span class="sxs-lookup"><span data-stu-id="a0de8-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
