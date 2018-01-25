---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: "ASP.NET MVC (VB) で 15 分以内にムービー データベース アプリケーションを作成 |Microsoft ドキュメント"
author: StephenWalther
description: "Stephen Walther は、データベースによる ASP.NET MVC アプリケーション全体が開始されてから完了するを構築します。 このチュートリアルでは新しい t のあるユーザーのための優れた紹介しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: b87a69df24a410161dfaf055519eb6137fa76c06
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a><span data-ttu-id="e1b22-104">ASP.NET MVC (VB) で 15 分以内にムービー データベース アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-104">Create a Movie Database Application in 15 Minutes with ASP.NET MVC (VB)</span></span>
====================
<span data-ttu-id="e1b22-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="e1b22-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="e1b22-106">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-106">Download Code</span></span>](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> <span data-ttu-id="e1b22-107">Stephen Walther は、データベースによる ASP.NET MVC アプリケーション全体が開始されてから完了するを構築します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-107">Stephen Walther builds an entire database-driven ASP.NET MVC application from start to finish.</span></span> <span data-ttu-id="e1b22-108">このチュートリアルは、新しい ASP.NET MVC フレームワークにし、ASP.NET MVC アプリケーションを構築するプロセスを把握したいユーザーの導入にも最適です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-108">This tutorial is a great introduction for people who are new to the ASP.NET MVC Framework and who want to get a sense of the process of building an ASP.NET MVC application.</span></span>


<span data-ttu-id="e1b22-109">このチュートリアルの目的は、「は、」の感を与える、ASP.NET MVC アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-109">The purpose of this tutorial is to give you a sense of "what it is like" to build an ASP.NET MVC application.</span></span> <span data-ttu-id="e1b22-110">このチュートリアルでは、ASP.NET MVC アプリケーション全体が開始されてから完了する構築をすれば稼動です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-110">In this tutorial, I blast through building an entire ASP.NET MVC application from start to finish.</span></span> <span data-ttu-id="e1b22-111">どのようにすることができますを一覧表示、作成、およびデータベース レコードを編集について説明する簡単なデータベースに基づくアプリケーションをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-111">I show you how to build a simple database-driven application that illustrates how you can list, create, and edit database records.</span></span>

<span data-ttu-id="e1b22-112">アプリケーションを構築するプロセスを簡略化は、Visual Studio 2008 のスキャフォールディング機能の利点をみましょう。</span><span class="sxs-lookup"><span data-stu-id="e1b22-112">To simplify the process of building our application, we'll take advantage of the scaffolding features of Visual Studio 2008.</span></span> <span data-ttu-id="e1b22-113">Visual Studio によって、最初のコードと、コント ローラー、モデル、およびビューのコンテンツの生成をお知らせします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-113">We'll let Visual Studio generate the initial code and content for our controllers, models, and views.</span></span>

<span data-ttu-id="e1b22-114">Active Server Pages または ASP.NET を使用する場合、しはず ASP.NET MVC 非常になじみです。</span><span class="sxs-lookup"><span data-stu-id="e1b22-114">If you have worked with Active Server Pages or ASP.NET, then you should find ASP.NET MVC very familiar.</span></span> <span data-ttu-id="e1b22-115">ASP.NET MVC ビューは、Active Server Pages アプリケーションのページとよく似ています。</span><span class="sxs-lookup"><span data-stu-id="e1b22-115">ASP.NET MVC views are very much like the pages in an Active Server Pages application.</span></span> <span data-ttu-id="e1b22-116">また、従来の ASP.NET Web フォーム アプリケーションと同じように ASP.NET MVC での言語と .NET framework が提供するクラスの豊富なセットへのフル アクセス。</span><span class="sxs-lookup"><span data-stu-id="e1b22-116">And, just like a traditional ASP.NET Web Forms application, ASP.NET MVC provides you with full access to the rich set of languages and classes provided by the .NET framework.</span></span>

<span data-ttu-id="e1b22-117">自分の望みは、このチュートリアルがどのようにの ASP.NET MVC アプリケーションの構築のエクスペリエンスと同様と、Active Server Pages または ASP.NET Web フォーム アプリケーションのビルドのエクスペリエンスとは異なる意味します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-117">My hope is that this tutorial will give you a sense of how the experience of building an ASP.NET MVC application is both similar and different than the experience of building an Active Server Pages or ASP.NET Web Forms application.</span></span>

## <a name="overview-of-the-movie-database-application"></a><span data-ttu-id="e1b22-118">ムービーのデータベース アプリケーションの概要</span><span class="sxs-lookup"><span data-stu-id="e1b22-118">Overview of the Movie Database Application</span></span>

<span data-ttu-id="e1b22-119">目標は簡単であるため、非常に単純なムービー データベース アプリケーションはビルドします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-119">Because our goal is to keep things simple, we'll build a very simple Movie Database application.</span></span> <span data-ttu-id="e1b22-120">簡単なムービー データベース アプリケーションでは次の 3 つの作業を行うことを許可します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-120">Our simple Movie Database application will allow us to do three things:</span></span>

1. <span data-ttu-id="e1b22-121">ムービーのデータベース レコードのセットを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-121">List a set of movie database records</span></span>
2. <span data-ttu-id="e1b22-122">新しいムービーのデータベース レコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-122">Create a new movie database record</span></span>
3. <span data-ttu-id="e1b22-123">ムービーの既存のデータベース レコードを編集します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-123">Edit an existing movie database record</span></span>

<span data-ttu-id="e1b22-124">もう一度、するために複雑にならない、利点は、アプリケーションの構築に必要な ASP.NET MVC フレームワークの機能の最小数の移動します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-124">Again, because we want to keep things simple, we'll take advantage of the minimum number of features of the ASP.NET MVC framework needed to build our application.</span></span> <span data-ttu-id="e1b22-125">たとえば、おされませんする活用テスト駆動型開発。</span><span class="sxs-lookup"><span data-stu-id="e1b22-125">For example, we won't be taking advantage of Test-Driven Development.</span></span>

<span data-ttu-id="e1b22-126">アプリケーションを作成するのには、それぞれ、次の手順を完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-126">In order to create our application, we need to complete each of the following steps:</span></span>

1. <span data-ttu-id="e1b22-127">ASP.NET MVC Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-127">Create the ASP.NET MVC Web Application Project</span></span>
2. <span data-ttu-id="e1b22-128">データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-128">Create the database</span></span>
3. <span data-ttu-id="e1b22-129">データベース モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-129">Create the database model</span></span>
4. <span data-ttu-id="e1b22-130">ASP.NET MVC コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-130">Create the ASP.NET MVC controller</span></span>
5. <span data-ttu-id="e1b22-131">ASP.NET MVC ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-131">Create the ASP.NET MVC views</span></span>

## <a name="preliminaries"></a><span data-ttu-id="e1b22-132">準備</span><span class="sxs-lookup"><span data-stu-id="e1b22-132">Preliminaries</span></span>

<span data-ttu-id="e1b22-133">Visual Studio 2008 または Visual Web Developer 2008 Express ASP.NET MVC アプリケーションを構築する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-133">You'll need either Visual Studio 2008 or Visual Web Developer 2008 Express to build an ASP.NET MVC application.</span></span> <span data-ttu-id="e1b22-134">また、ASP.NET MVC フレームワークをダウンロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-134">You also need to download the ASP.NET MVC framework.</span></span>

<span data-ttu-id="e1b22-135">Visual Studio 2008 を所有していない場合は、この web サイトから、Visual Studio 2008 の 90 日間試用版をダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-135">If you don't own Visual Studio 2008, then you can download a 90 day trial version of Visual Studio 2008 from this website:</span></span>

[<span data-ttu-id="e1b22-136">https://msdn.microsoft.com/vs2008/products/cc268305.aspx</span><span class="sxs-lookup"><span data-stu-id="e1b22-136">https://msdn.microsoft.com/vs2008/products/cc268305.aspx</span></span>](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

<span data-ttu-id="e1b22-137">または、作成できます ASP.NET MVC アプリケーションを Visual Web Developer Express 2008 で。</span><span class="sxs-lookup"><span data-stu-id="e1b22-137">Alternatively, you can create ASP.NET MVC applications with Visual Web Developer Express 2008.</span></span> <span data-ttu-id="e1b22-138">Visual Web Developer Express を使用すると、Service Pack 1 がインストールされているが必要です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-138">If you decide to use Visual Web Developer Express then you must have Service Pack 1 installed.</span></span> <span data-ttu-id="e1b22-139">Visual Web Developer 2008 Express with Service Pack 1 は、この web サイトからダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-139">You can download Visual Web Developer 2008 Express with Service Pack 1 from this website:</span></span>

[<span data-ttu-id="e1b22-140">https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en</span><span class="sxs-lookup"><span data-stu-id="e1b22-140">https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

<span data-ttu-id="e1b22-141">Visual Studio 2008 または Visual Web Developer 2008 のいずれかをインストールした後は、ASP.NET MVC フレームワークをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-141">After you install either Visual Studio 2008 or Visual Web Developer 2008, you need to install the ASP.NET MVC framework.</span></span> <span data-ttu-id="e1b22-142">ASP.NET MVC フレームワークは、次の web サイトからダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-142">You can download the ASP.NET MVC framework from the following website:</span></span>

[<span data-ttu-id="e1b22-143">https://www.asp.net/mvc/</span><span class="sxs-lookup"><span data-stu-id="e1b22-143">https://www.asp.net/mvc/</span></span>](../../../index.md)

> [!NOTE] 
> 
> <span data-ttu-id="e1b22-144">ASP.NET フレームワークと ASP.NET MVC フレームワークを個別にダウンロードする代わりに、Web Platform Installer の利点を実行できます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-144">Instead of downloading the ASP.NET framework and the ASP.NET MVC framework individually, you can take advantage of the Web Platform Installer.</span></span> <span data-ttu-id="e1b22-145">Web Platform Installer は、アプリケーションを簡単にインストールされているアプリケーションを管理することができますが、コンピューターです。</span><span class="sxs-lookup"><span data-stu-id="e1b22-145">The Web Platform Installer is an application that enables you to easily manage the installed applications are your computer:</span></span>
> 
> [<span data-ttu-id="e1b22-146">https://www.microsoft.com/web/gallery/Install.aspx</span><span class="sxs-lookup"><span data-stu-id="e1b22-146">https://www.microsoft.com/web/gallery/Install.aspx</span></span>](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a><span data-ttu-id="e1b22-147">ASP.NET MVC Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-147">Creating an ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="e1b22-148">Visual Studio 2008 で新しい ASP.NET MVC Web アプリケーション プロジェクトを作成して開始しましょう。</span><span class="sxs-lookup"><span data-stu-id="e1b22-148">Let's start by creating a new ASP.NET MVC Web Application project in Visual Studio 2008.</span></span> <span data-ttu-id="e1b22-149">メニュー オプションを選択**ファイル、新しいプロジェクト**図 1 に新しいプロジェクト ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-149">Select the menu option **File, New Project** and you will see the New Project dialog box in Figure 1.</span></span> <span data-ttu-id="e1b22-150">プログラミング言語と Visual Basic を選択し、ASP.NET MVC Web アプリケーション プロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-150">Select Visual Basic as the programming language and select the ASP.NET MVC Web Application project template.</span></span> <span data-ttu-id="e1b22-151">プロジェクト MovieApp 名前を付け、[ok] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-151">Give your project the name MovieApp and click the OK button.</span></span>


<span data-ttu-id="e1b22-152">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-152">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)</span></span>

<span data-ttu-id="e1b22-153">**図 01**: [新しいプロジェクト] ダイアログ ボックス ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-153">**Figure 01**: The New Project dialog box ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))</span></span>


<span data-ttu-id="e1b22-154">新しいプロジェクト ダイアログの上部にあるドロップダウン リストから .NET Framework 3.5 を選択するか、ASP.NET MVC Web アプリケーション プロジェクト テンプレートが表示されないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="e1b22-154">Make sure that you select .NET Framework 3.5 from the dropdown list at the top of the New Project dialog or the ASP.NET MVC Web Application project template won't appear.</span></span>


<span data-ttu-id="e1b22-155">新しい MVC Web アプリケーション プロジェクトを作成するときに Visual Studio では、個別の単体テスト プロジェクトを作成するように求められます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-155">Whenever you create a new MVC Web Application project, Visual Studio prompts you to create a separate unit test project.</span></span> <span data-ttu-id="e1b22-156">図 2 では、ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-156">The dialog in Figure 2 appears.</span></span> <span data-ttu-id="e1b22-157">おされませんするテストの作成このチュートリアルでは時間の制約により、はい、必要がありますだと考えてについて少しまま) を選択、**いいえ**オプションをクリックして、 **ok**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-157">Because we won't be creating tests in this tutorial because of time constraints (and, yes, we should feel a little guilty about this) select the **No** option and click the **OK** button.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e1b22-158">Visual Web Developer は、テスト プロジェクトをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="e1b22-158">Visual Web Developer does not support test projects.</span></span>


<span data-ttu-id="e1b22-159">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-159">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)</span></span>

<span data-ttu-id="e1b22-160">**図 02**:、単体テスト プロジェクトの作成 ダイアログ ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-160">**Figure 02**: The Create Unit Test Project dialog ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))</span></span>


<span data-ttu-id="e1b22-161">ASP.NET MVC アプリケーションが標準的な一連のフォルダー: モデル、ビュー、およびコント ローラーのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e1b22-161">An ASP.NET MVC application has a standard set of folders: a Models, Views, and Controllers folder.</span></span> <span data-ttu-id="e1b22-162">この標準的な一連のソリューション エクスプ ローラー ウィンドウでフォルダーを表示できます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-162">You can see this standard set of folders in the Solution Explorer window.</span></span> <span data-ttu-id="e1b22-163">ファイルを追加するそれぞれのモデル、ビュー、およびコント ローラーのフォルダーに、ムービーのデータベース アプリケーションを構築するために必要になります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-163">We'll need to add files to each of the Models, Views, and Controllers folders in order to build our Movie Database application.</span></span>

<span data-ttu-id="e1b22-164">Visual Studio で新しい MVC アプリケーションを作成するときに、サンプル アプリケーションを取得します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-164">When you create a new MVC application with Visual Studio, you get a sample application.</span></span> <span data-ttu-id="e1b22-165">最初から開始したいのでこのサンプル アプリケーションのコンテンツを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-165">Because we want to start from scratch, we need to delete the content for this sample application.</span></span> <span data-ttu-id="e1b22-166">次のファイルと、次のフォルダーを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-166">You need to delete the following file and the following folder:</span></span>

- <span data-ttu-id="e1b22-167">Controllers\HomeController.vb</span><span class="sxs-lookup"><span data-stu-id="e1b22-167">Controllers\HomeController.vb</span></span>
- <span data-ttu-id="e1b22-168">Views\Home</span><span class="sxs-lookup"><span data-stu-id="e1b22-168">Views\Home</span></span>

## <a name="creating-the-database"></a><span data-ttu-id="e1b22-169">データベースの作成</span><span class="sxs-lookup"><span data-stu-id="e1b22-169">Creating the Database</span></span>

<span data-ttu-id="e1b22-170">ムービー データベース レコードを保持するためにデータベースを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-170">We need to create a database to hold our movie database records.</span></span> <span data-ttu-id="e1b22-171">幸運にも、Visual Studio には、SQL Server Express という名前のフリー データベースが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e1b22-171">Luckily, Visual Studio includes a free database named SQL Server Express.</span></span> <span data-ttu-id="e1b22-172">データベースを作成する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="e1b22-172">Follow these steps to create the database:</span></span>

1. <span data-ttu-id="e1b22-173">アプリケーションを右クリックして\_メニュー オプションを選択し、ソリューション エクスプ ローラーのウィンドウで、データ フォルダー**追加]、[新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-173">Right-click the App\_Data folder in the Solution Explorer window and select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="e1b22-174">選択、**データ**カテゴリを選択、 **SQL Server データベース**テンプレート (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="e1b22-174">Select the **Data** category and select the **SQL Server Database** template (see Figure 3).</span></span>
3. <span data-ttu-id="e1b22-175">新しいデータベースの名前を付けます*MoviesDB.mdf*  をクリックし、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-175">Name your new database *MoviesDB.mdf* and click the **Add** button.</span></span>

<span data-ttu-id="e1b22-176">アプリにある MoviesDB.mdf ファイルをダブルクリックして、データベースに接続することができます、データベースを作成した後\_データ フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e1b22-176">After you create your database, you can connect to the database by double-clicking the MoviesDB.mdf file located in the App\_Data folder.</span></span> <span data-ttu-id="e1b22-177">サーバー エクスプ ローラー ウィンドウを開きます。 この MoviesDB.mdf ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-177">Double-clicking the MoviesDB.mdf file opens the Server Explorer window.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e1b22-178">サーバー エクスプ ローラー ウィンドウには、Visual Web Developer の場合、データベース エクスプ ローラー ウィンドウがという名前です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-178">The Server Explorer window is named the Database Explorer window in the case of Visual Web Developer.</span></span>


<span data-ttu-id="e1b22-179">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-179">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)</span></span>

<span data-ttu-id="e1b22-180">**図 03**: Microsoft SQL Server データベースの作成 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-180">**Figure 03**: Creating a Microsoft SQL Server Database ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))</span></span>


<span data-ttu-id="e1b22-181">次に、新しいデータベース テーブルを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-181">Next, we need to create a new database table.</span></span> <span data-ttu-id="e1b22-182">サーバー エクスプ ローラー ウィンドウ内でテーブルのフォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-182">From within the Sever Explorer window, right-click the Tables folder and select the menu option **Add New Table**.</span></span> <span data-ttu-id="e1b22-183">このメニュー オプションを選択すると、データベースのテーブル デザイナーが開きます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-183">Selecting this menu option opens the database table designer.</span></span> <span data-ttu-id="e1b22-184">次のデータベース列を作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-184">Create the following database columns:</span></span>

<a id="0.2_table01"></a>


| <span data-ttu-id="e1b22-185">**列名**</span><span class="sxs-lookup"><span data-stu-id="e1b22-185">**Column Name**</span></span> | <span data-ttu-id="e1b22-186">**データ型**</span><span class="sxs-lookup"><span data-stu-id="e1b22-186">**Data Type**</span></span> | <span data-ttu-id="e1b22-187">**Null を許容します。**</span><span class="sxs-lookup"><span data-stu-id="e1b22-187">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e1b22-188">ID</span><span class="sxs-lookup"><span data-stu-id="e1b22-188">Id</span></span> | <span data-ttu-id="e1b22-189">Int</span><span class="sxs-lookup"><span data-stu-id="e1b22-189">Int</span></span> | <span data-ttu-id="e1b22-190">False</span><span class="sxs-lookup"><span data-stu-id="e1b22-190">False</span></span> |
| <span data-ttu-id="e1b22-191">Title</span><span class="sxs-lookup"><span data-stu-id="e1b22-191">Title</span></span> | <span data-ttu-id="e1b22-192">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="e1b22-192">Nvarchar(100)</span></span> | <span data-ttu-id="e1b22-193">False</span><span class="sxs-lookup"><span data-stu-id="e1b22-193">False</span></span> |
| <span data-ttu-id="e1b22-194">ディレクター</span><span class="sxs-lookup"><span data-stu-id="e1b22-194">Director</span></span> | <span data-ttu-id="e1b22-195">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="e1b22-195">Nvarchar(100)</span></span> | <span data-ttu-id="e1b22-196">False</span><span class="sxs-lookup"><span data-stu-id="e1b22-196">False</span></span> |
| <span data-ttu-id="e1b22-197">DateReleased</span><span class="sxs-lookup"><span data-stu-id="e1b22-197">DateReleased</span></span> | <span data-ttu-id="e1b22-198">DateTime</span><span class="sxs-lookup"><span data-stu-id="e1b22-198">DateTime</span></span> | <span data-ttu-id="e1b22-199">False</span><span class="sxs-lookup"><span data-stu-id="e1b22-199">False</span></span> |


<span data-ttu-id="e1b22-200">最初の列、Id 列には、次の 2 つの特殊なプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-200">The first column, the Id column, has two special properties.</span></span> <span data-ttu-id="e1b22-201">最初に、Id 列を主キー列としてマークする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-201">First, you need to mark the Id column as the primary key column.</span></span> <span data-ttu-id="e1b22-202">Id 列を選択するをクリックして、**主キーの設定**(キーのようなアイコンでは) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-202">After selecting the Id column, click the **Set Primary Key** button (it is the icon that looks like a key).</span></span> <span data-ttu-id="e1b22-203">次に、Id 列、Id 列としてマークする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-203">Second, you need to mark the Id column as an Identity column.</span></span> <span data-ttu-id="e1b22-204">列のプロパティ ウィンドウで Identity の指定 セクションまで下にスクロールし、それを展開します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-204">In the Column Properties window, scroll down to the Identity Specification section and expand it.</span></span> <span data-ttu-id="e1b22-205">変更、 **Is Identity**プロパティ値を**はい**です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-205">Change the **Is Identity** property to the value **Yes**.</span></span> <span data-ttu-id="e1b22-206">完了したら、表に、図 4 のようになります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-206">When you are finished, the table should look like Figure 4.</span></span>


<span data-ttu-id="e1b22-207">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-207">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)</span></span>

<span data-ttu-id="e1b22-208">**図 04**:、映画データベース テーブル ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-208">**Figure 04**: The Movies database table ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))</span></span>


<span data-ttu-id="e1b22-209">最後の手順では、新しいテーブルを保存します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-209">The final step is to save the new table.</span></span> <span data-ttu-id="e1b22-210">[保存] ボタン (フロッピー ディスクのアイコン) をクリックし、新しいテーブル名の映画を提供します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-210">Click the Save button (the icon of the floppy) and give the new table the name Movies.</span></span>

<span data-ttu-id="e1b22-211">テーブルの作成が完了したら、ムービー レコードの一部をテーブルに追加します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-211">After you finish creating the table, add some movie records to the table.</span></span> <span data-ttu-id="e1b22-212">サーバー エクスプ ローラー ウィンドウで、ムービーのテーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-212">Right-click the Movies table in the Server Explorer window and select the menu option **Show Table Data**.</span></span> <span data-ttu-id="e1b22-213">(図 5 を参照してください)、お気に入りの映画の一覧を入力します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-213">Enter a list of your favorite movies (see Figure 5).</span></span>


<span data-ttu-id="e1b22-214">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-214">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)</span></span>

<span data-ttu-id="e1b22-215">**図 05**: ムービー レコードの入力 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-215">**Figure 05**: Entering movie records ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))</span></span>


## <a name="creating-the-model"></a><span data-ttu-id="e1b22-216">モデルの作成</span><span class="sxs-lookup"><span data-stu-id="e1b22-216">Creating the Model</span></span>

<span data-ttu-id="e1b22-217">次に、データベースを表すクラスのセットを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-217">We next need to create a set of classes to represent our database.</span></span> <span data-ttu-id="e1b22-218">データベース モデルを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-218">We need to create a database model.</span></span> <span data-ttu-id="e1b22-219">このデータベース モデルのクラスを自動的に生成する Microsoft Entity Framework を活用をみましょう。</span><span class="sxs-lookup"><span data-stu-id="e1b22-219">We'll take advantage of the Microsoft Entity Framework to generate the classes for our database model automatically.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e1b22-220">ASP.NET MVC フレームワークは、Microsoft の Entity Framework に関連付けられていません。</span><span class="sxs-lookup"><span data-stu-id="e1b22-220">The ASP.NET MVC framework is not tied to the Microsoft Entity Framework.</span></span> <span data-ttu-id="e1b22-221">さまざまなオブジェクト リレーショナル マッピングを活用して、データベース モデル クラスを作成できます (または/M) LINQ to SQL、Subsonic、および NHibernate などのツールです。</span><span class="sxs-lookup"><span data-stu-id="e1b22-221">You can create your database model classes by taking advantage of a variety of Object Relational Mapping (OR/M) tools including LINQ to SQL, Subsonic, and NHibernate.</span></span>


<span data-ttu-id="e1b22-222">Entity Data Model ウィザードを起動する次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="e1b22-222">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="e1b22-223">ソリューション エクスプ ローラー ウィンドウのオプションを選択し、メニューの モデル フォルダーを右クリックして**追加、新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-223">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="e1b22-224">選択、**データ**カテゴリを選択、 **ADO.NET エンティティ データ モデル**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="e1b22-224">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="e1b22-225">データ モデルに名前を付ける*MoviesDBModel.edmx*  をクリックし、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-225">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="e1b22-226">Entity Data Model ウィザードでは、[追加] ボタンをクリックした後、(図 6 を参照) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-226">After you click the Add button, the Entity Data Model Wizard appears (see Figure 6).</span></span> <span data-ttu-id="e1b22-227">ウィザードを完了する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="e1b22-227">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="e1b22-228">**モデルのコンテンツ**手順、select、**データベースから生成**オプション。</span><span class="sxs-lookup"><span data-stu-id="e1b22-228">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="e1b22-229">**データ接続の選択**ステップを使用して、 *MoviesDB.mdf*データ接続と名前*MoviesDBEntities*接続の設定。</span><span class="sxs-lookup"><span data-stu-id="e1b22-229">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="e1b22-230">クリックして、**次**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-230">Click the **Next** button.</span></span>
3. <span data-ttu-id="e1b22-231">**データベース オブジェクトの選択**ステップ、[テーブル] ノードを展開し、映画のテーブルを選択します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-231">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="e1b22-232">名前空間を入力*MovieApp.Models*  をクリックし、**完了**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-232">Enter the namespace *MovieApp.Models* and click the **Finish** button.</span></span>


<span data-ttu-id="e1b22-233">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-233">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)</span></span>

<span data-ttu-id="e1b22-234">**図 06**: Entity Data Model ウィザードでデータベース モデルの生成 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-234">**Figure 06**: Generating a database model with the Entity Data Model Wizard ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))</span></span>


<span data-ttu-id="e1b22-235">Entity Data Model ウィザードを完了すると、Entity Data Model デザイナーが開きます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-235">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="e1b22-236">映画データベース テーブルをデザイナーに表示する必要があります (図 7 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="e1b22-236">The Designer should display the Movies database table (see Figure 7).</span></span>


<span data-ttu-id="e1b22-237">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-237">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)</span></span>

<span data-ttu-id="e1b22-238">**図 07**: The Entity Data Model Designer ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-238">**Figure 07**: The Entity Data Model Designer ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))</span></span>


<span data-ttu-id="e1b22-239">続行する前に、1 つの変更を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-239">We need to make one change before we continue.</span></span> <span data-ttu-id="e1b22-240">エンティティ データ ウィザードでは、ムービーのデータベース テーブルを表すモデル クラス映画をという名前を生成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-240">The Entity Data Wizard generates a model class named Movies that represents the Movies database table.</span></span> <span data-ttu-id="e1b22-241">映画クラスを使用して、特定のムービーを表す、ためにクラスの名前を変更する必要があります*ムービー*の代わりに*映画*(単数形ではなく複数形)。</span><span class="sxs-lookup"><span data-stu-id="e1b22-241">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="e1b22-242">デザイナー画面にクラスの名前をダブルクリックし、ムービーからムービーにクラスの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-242">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="e1b22-243">この変更を加えたらをクリックして、**保存**ムービー クラスを生成する (フロッピー ディスクのアイコン) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-243">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="creating-the-aspnet-mvc-controller"></a><span data-ttu-id="e1b22-244">ASP.NET MVC コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-244">Creating the ASP.NET MVC Controller</span></span>

<span data-ttu-id="e1b22-245">次の手順では、ASP.NET MVC コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-245">The next step is to create the ASP.NET MVC controller.</span></span> <span data-ttu-id="e1b22-246">コント ローラーは、ユーザーが、ASP.NET MVC アプリケーションと対話する方法を制御します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-246">A controller is responsible for controlling how a user interacts with an ASP.NET MVC application.</span></span>

<span data-ttu-id="e1b22-247">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="e1b22-247">Follow these steps:</span></span>

1. <span data-ttu-id="e1b22-248">ソリューション エクスプ ローラー ウィンドウで、Controllers フォルダーを右クリックし、メニュー オプションを選択**追加、コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-248">In the Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller**.</span></span>
2. <span data-ttu-id="e1b22-249">コント ローラーの追加 ダイアログ ボックスで、名前を入力してください。 *HomeController*  チェック ボックスを確認および**アクション メソッドの作成、更新、および詳細シナリオを追加**(図 8 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="e1b22-249">In the Add Controller dialog, enter the name *HomeController* and check the checkbox labeled **Add action methods for Create, Update, and Details scenarios** (see Figure 8).</span></span>
3. <span data-ttu-id="e1b22-250">をクリックして、**追加**をプロジェクトに、新しいコント ローラーを追加するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-250">Click the **Add** button to add the new controller to your project.</span></span>

<span data-ttu-id="e1b22-251">次の手順を完了すると、リスト 1 のコント ローラーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-251">After you complete these steps, the controller in Listing 1 is created.</span></span> <span data-ttu-id="e1b22-252">インデックス、詳細については、作成、という名前のメソッドが含まれていることを確認および編集します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-252">Notice that it contains methods named Index, Details, Create, and Edit.</span></span> <span data-ttu-id="e1b22-253">次のセクションでは、作業にこれらのメソッドを取得するために必要なコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-253">In the following sections, we'll add the necessary code to get these methods to work.</span></span>


<span data-ttu-id="e1b22-254">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-254">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)</span></span>

<span data-ttu-id="e1b22-255">**図 08**: 新しい ASP.NET MVC コント ローラーの追加 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-255">**Figure 08**: Adding a new ASP.NET MVC Controller ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))</span></span>


<span data-ttu-id="e1b22-256">**1 – Controllers\HomeController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="e1b22-256">**Listing 1 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a><span data-ttu-id="e1b22-257">データベース レコードの一覧</span><span class="sxs-lookup"><span data-stu-id="e1b22-257">Listing Database Records</span></span>

<span data-ttu-id="e1b22-258">Home コント ローラーの Index() メソッドは、ASP.NET MVC アプリケーションの既定のメソッドです。</span><span class="sxs-lookup"><span data-stu-id="e1b22-258">The Index() method of the Home controller is the default method for an ASP.NET MVC application.</span></span> <span data-ttu-id="e1b22-259">ASP.NET MVC アプリケーションを実行すると、Index() メソッドが呼び出される最初のコント ローラー メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-259">When you run an ASP.NET MVC application, the Index() method is the first controller method that is called.</span></span>

<span data-ttu-id="e1b22-260">Index() メソッドを使用して、ムービーのデータベース テーブルからレコードの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-260">We'll use the Index() method to display the list of records from the Movies database table.</span></span> <span data-ttu-id="e1b22-261">データベースの利用 Index() メソッドを使用して、ムービーのデータベース レコードを取得する前に作成したモデル クラス移動します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-261">We'll take advantage of the database model classes that we created earlier to retrieve the movie database records with the Index() method.</span></span>

<span data-ttu-id="e1b22-262">という名前の新しいプライベート フィールドが含まれているように 2 の一覧でテンプレートを使用するクラスを変更しました\_db します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-262">I've modified the HomeController class in Listing 2 so that it contains a new private field named \_db.</span></span> <span data-ttu-id="e1b22-263">MoviesDBEntities クラスは、データベース モデルを表し、このクラスを使用して、データベースと通信します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-263">The MoviesDBEntities class represents our database model and we'll use this class to communicate with our database.</span></span>

<span data-ttu-id="e1b22-264">また、Index() メソッドを一覧表示する 2 を変更しました。</span><span class="sxs-lookup"><span data-stu-id="e1b22-264">I've also modified the Index() method in Listing 2.</span></span> <span data-ttu-id="e1b22-265">Index() メソッドでは、MoviesDBEntities クラスを使用して、映画データベース テーブルからすべてのムービーのレコードを取得します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-265">The Index() method uses the MoviesDBEntities class to retrieve all of the movie records from the Movies database table.</span></span> <span data-ttu-id="e1b22-266">式 *\_db します。MovieSet.ToList()*映画データベース テーブルからすべてのムービーのレコードの一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-266">The expression *\_db.MovieSet.ToList()* returns a list of all of the movie records from the Movies database table.</span></span>

<span data-ttu-id="e1b22-267">ムービーの一覧は、ビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-267">The list of movies is passed to the view.</span></span> <span data-ttu-id="e1b22-268">データの表示と、ビューに渡さ View() メソッドに渡されたものを取得します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-268">Anything that gets passed to the View() method gets passed to the view as view data.</span></span>

<span data-ttu-id="e1b22-269">**2 – Controllers/HomeController.vb (修飾されているインデックス メソッド) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="e1b22-269">**Listing 2 – Controllers/HomeController.vb (modified Index method)**</span></span>

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

<span data-ttu-id="e1b22-270">Index() メソッドでは、インデックスをという名前のビューを返します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-270">The Index() method returns a view named Index.</span></span> <span data-ttu-id="e1b22-271">ムービー データベース レコードの一覧を表示するには、このビューを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-271">We need to create this view to display the list of movie database records.</span></span> <span data-ttu-id="e1b22-272">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="e1b22-272">Follow these steps:</span></span>


<span data-ttu-id="e1b22-273">プロジェクトをビルドする必要があります (メニュー オプションを選択**ビルド、ソリューションのビルド**) 開く前に、**ビューの追加**ダイアログまたはクラスを持たないに表示されます、**データ クラスを表示**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="e1b22-273">You should build your project (select the menu option **Build, Build Solution**) before opening the **Add View** dialog or no classes will appear in the **View data class** dropdown list.</span></span>


1. <span data-ttu-id="e1b22-274">コード エディターで Index() メソッドを右クリックし、メニュー オプションを選択**ビューの追加**(図 9 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="e1b22-274">Right-click the Index() method in the code editor and select the menu option **Add View** (see Figure 9).</span></span>
2. <span data-ttu-id="e1b22-275">ビューの追加 ダイアログ ボックスで確認するチェック ボックス ラベルの付いた**厳密に型指定されたビューを作成する**がオンになっています。</span><span class="sxs-lookup"><span data-stu-id="e1b22-275">In the Add View dialog, verify that the checkbox labeled **Create a strongly-typed view** is checked.</span></span>
3. <span data-ttu-id="e1b22-276">**コンテンツを表示** ドロップダウン リストで、値を選択*リスト*です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-276">From the **View content** dropdown list, select the value *List*.</span></span>
4. <span data-ttu-id="e1b22-277">**データ クラスを表示** ドロップダウン リストで、値を選択*MovieApp.Movie*です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-277">From the **View data class** dropdown list, select the value *MovieApp.Movie*.</span></span>
5. <span data-ttu-id="e1b22-278">新しいを作成する追加のボタンをクリックして (図 10 参照) を表示します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-278">Click the Add button to create the new view (see Figure 10).</span></span>

<span data-ttu-id="e1b22-279">次の手順を完了すると、Index.aspx をという名前の新しいビューは、views \home フォルダーに追加されます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-279">After you complete these steps, a new view named Index.aspx is added to the Views\Home folder.</span></span> <span data-ttu-id="e1b22-280">3 の一覧表示するのには、インデックス ビューの内容が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e1b22-280">The contents of the Index view are included in Listing 3.</span></span>


<span data-ttu-id="e1b22-281">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-281">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)</span></span>

<span data-ttu-id="e1b22-282">**図 09**: コント ローラーのアクションからビューの追加 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-282">**Figure 09**: Adding a view from a controller action ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))</span></span>


<span data-ttu-id="e1b22-283">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-283">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)</span></span>

<span data-ttu-id="e1b22-284">**図 10**: ビューの追加 ダイアログで、新しいビューを作成する ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-284">**Figure 10**: Creating a new view with the Add View dialog ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))</span></span>


[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

<span data-ttu-id="e1b22-285">インデックス ビューには、すべての HTML テーブル内のムービーのデータベース テーブルからムービー レコードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-285">The Index view displays all of the movie records from the Movies database table within an HTML table.</span></span> <span data-ttu-id="e1b22-286">ビューに含まれる For ViewData.Model プロパティで表される各ムービーを反復処理する各ループします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-286">The view contains a For Each loop that iterates through each movie represented by the ViewData.Model property.</span></span> <span data-ttu-id="e1b22-287">F5 キーを押すことによって、アプリケーションを実行すると、図 11 では、web ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-287">If you run your application by hitting the F5 key, then you'll see the web page in Figure 11.</span></span>


<span data-ttu-id="e1b22-288">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-288">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)</span></span>

<span data-ttu-id="e1b22-289">**図 11**:、インデックス ビュー ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-289">**Figure 11**: The Index view ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))</span></span>


## <a name="creating-new-database-records"></a><span data-ttu-id="e1b22-290">新しいデータベース レコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-290">Creating New Database Records</span></span>

<span data-ttu-id="e1b22-291">前のセクションで作成したインデックス ビューには、新しいデータベース レコードを作成するためのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e1b22-291">The Index view that we created in the previous section includes a link for creating new database records.</span></span> <span data-ttu-id="e1b22-292">では早速しロジックを実装して、新しいムービーのデータベース レコードを作成するために必要なビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-292">Let's go ahead and implement the logic and create the view necessary for creating new movie database records.</span></span>

<span data-ttu-id="e1b22-293">Home コント ローラーには、Create() という名前の 2 つのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e1b22-293">The Home controller contains two methods named Create().</span></span> <span data-ttu-id="e1b22-294">最初の Create() メソッドにパラメーターがありません。</span><span class="sxs-lookup"><span data-stu-id="e1b22-294">The first Create() method has no parameters.</span></span> <span data-ttu-id="e1b22-295">この Create() メソッドのオーバー ロードを使用して、新しいムービーのデータベース レコードを作成するための HTML フォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-295">This overload of the Create() method is used to display the HTML form for creating a new movie database record.</span></span>

<span data-ttu-id="e1b22-296">2 番目の Create() メソッドには、FormCollection パラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-296">The second Create() method has a FormCollection parameter.</span></span> <span data-ttu-id="e1b22-297">この Create() メソッドのオーバー ロードは、新しいムービーを作成するための HTML フォームがサーバーにポストされたときに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-297">This overload of the Create() method is called when the HTML form for creating a new movie is posted to the server.</span></span> <span data-ttu-id="e1b22-298">この 2 つ目の Create() メソッドが AcceptVerbs 属性、メソッドが HTTP Post 操作を実行しない限り、呼び出されることを防ぎますを持つことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e1b22-298">Notice that this second Create() method has an AcceptVerbs attribute that prevents the method from being called unless an HTTP Post operation is performed.</span></span>

<span data-ttu-id="e1b22-299">この 2 つ目の Create() メソッドは、更新されたテンプレートを使用するクラスを一覧表示する 4 で変更されています。</span><span class="sxs-lookup"><span data-stu-id="e1b22-299">This second Create() method has been modified in the updated HomeController class in Listing 4.</span></span> <span data-ttu-id="e1b22-300">新しいバージョンの Create() メソッドは、ムービー パラメーターを確定し、映画データベース テーブルに新しいムービーを挿入するためのロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e1b22-300">The new version of the Create() method accepts a Movie parameter and contains the logic for inserting a new movie into the Movies database table.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e1b22-301">Bind 属性に注意してください。</span><span class="sxs-lookup"><span data-stu-id="e1b22-301">Notice the Bind attribute.</span></span> <span data-ttu-id="e1b22-302">HTML フォームからムービー Id プロパティを更新したくありません、ためこのプロパティを明示的に除外する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-302">Because we don't want to update the Movie Id property from HTML form, we need to explicitly exclude this property.</span></span>


<span data-ttu-id="e1b22-303">**4 – Controllers\HomeController.vb (修飾されている作成メソッド) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="e1b22-303">**Listing 4 – Controllers\HomeController.vb (modified Create method)**</span></span>

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

<span data-ttu-id="e1b22-304">Visual Studio では、新しいムービーのデータベースを作成するためのフォームを作成する簡単 (図 12 を参照してください) を記録します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-304">Visual Studio makes it easy to create the form for creating a new movie database record (see Figure 12).</span></span> <span data-ttu-id="e1b22-305">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="e1b22-305">Follow these steps:</span></span>

1. <span data-ttu-id="e1b22-306">コード エディターで Create() メソッドを右クリックし、メニュー オプションを選択**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-306">Right-click the Create() method in the code editor and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="e1b22-307">チェック ボックスがラベルを確認してください**厳密に型指定されたビューを作成**がオンになっています。</span><span class="sxs-lookup"><span data-stu-id="e1b22-307">Verify that the checkbox labeled **Create a strongly-typed view** is checked.</span></span>
3. <span data-ttu-id="e1b22-308">**コンテンツを表示** ドロップダウン リストで、値を選択*作成*です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-308">From the **View content** dropdown list, select the value *Create*.</span></span>
4. <span data-ttu-id="e1b22-309">**データ クラスを表示** ドロップダウン リストで、値を選択*MovieApp.Movie*です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-309">From the **View data class** dropdown list, select the value *MovieApp.Movie*.</span></span>
5. <span data-ttu-id="e1b22-310">クリックして、**追加**クリックすると、新しいビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-310">Click the **Add** button to create the new view.</span></span>


<span data-ttu-id="e1b22-311">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-311">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)</span></span>

<span data-ttu-id="e1b22-312">**図 12**: 作成ビューの追加 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-312">**Figure 12**: Adding the Create view ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))</span></span>


<span data-ttu-id="e1b22-313">Visual Studio は、自動的に 5 を一覧表示するビューを生成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-313">Visual Studio generates the view in Listing 5 automatically.</span></span> <span data-ttu-id="e1b22-314">このビューには、各ムービー クラスのプロパティに対応するフィールドを含む、HTML フォームが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e1b22-314">This view contains an HTML form that includes fields that correspond to each of the properties of the Movie class.</span></span>

<span data-ttu-id="e1b22-315">**Listing 5 – Views\Home\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="e1b22-315">**Listing 5 – Views\Home\Create.aspx**</span></span>

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> <span data-ttu-id="e1b22-316">ビューの追加 ダイアログで生成された HTML フォームには、Id フォーム フィールドが生成されます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-316">The HTML form generated by the Add View dialog generates an Id form field.</span></span> <span data-ttu-id="e1b22-317">Id 列は Id 列であるため、このフォームのフィールドは不要し、安全に削除することができます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-317">Because the Id column is an Identity column, we don't need this form field and you can safely remove it.</span></span>


<span data-ttu-id="e1b22-318">作成ビューを追加した後は、データベースに新しいムービーのレコードを追加できます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-318">After you add the Create view, you can add new Movie records to the database.</span></span> <span data-ttu-id="e1b22-319">F5 キーを押してアプリケーションを実行し、図 13 で実行されたフォームを新規作成 をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1b22-319">Run your application by pressing the F5 key and click the Create New link to see the form in Figure 13.</span></span> <span data-ttu-id="e1b22-320">完了し、フォームを送信すると、新しいムービーのデータベース レコードが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-320">If you complete and submit the form, a new movie database record is created.</span></span>

<span data-ttu-id="e1b22-321">フォーム検証を自動的に取得することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e1b22-321">Notice that you get form validation automatically.</span></span> <span data-ttu-id="e1b22-322">無視すると、ムービーのリリース日付を入力するか、無効なリリース日を入力する、フォームが再表示し、リリース日 フィールドが強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-322">If you neglect to enter a release date for a movie, or you enter an invalid release date, then the form is redisplayed and the release date field is highlighted.</span></span>


<span data-ttu-id="e1b22-323">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-323">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)</span></span>

<span data-ttu-id="e1b22-324">**図 13**: 新しいムービーのデータベース レコードを作成する ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-324">**Figure 13**: Creating a new movie database record ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))</span></span>


## <a name="editing-existing-database-records"></a><span data-ttu-id="e1b22-325">既存のデータベース レコードの編集</span><span class="sxs-lookup"><span data-stu-id="e1b22-325">Editing Existing Database Records</span></span>

<span data-ttu-id="e1b22-326">前のセクションで、一覧表示し、新しいデータベース レコードを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-326">In the previous sections, we discussed how you can list and create new database records.</span></span> <span data-ttu-id="e1b22-327">この最後のセクションでは、既存のデータベース レコードを編集する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-327">In this final section, we discuss how you can edit existing database records.</span></span>

<span data-ttu-id="e1b22-328">最初に、編集フォームを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-328">First, we need to generate the Edit form.</span></span> <span data-ttu-id="e1b22-329">Visual Studio はご利用の米国編集用のフォームを自動的に生成されるため、この手順は簡単です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-329">This step is easy since Visual Studio will generate the Edit form for us automatically.</span></span> <span data-ttu-id="e1b22-330">Visual Studio コード エディターで HomeController.vb クラスを開き、これらの手順に従います。</span><span class="sxs-lookup"><span data-stu-id="e1b22-330">Open the HomeController.vb class in the Visual Studio code editor and follow these steps:</span></span>

1. <span data-ttu-id="e1b22-331">コード エディターで Edit() メソッドを右クリックし、メニュー オプションを選択**ビューの追加**(図 14 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="e1b22-331">Right-click the Edit() method in the code editor and select the menu option **Add View** (see Figure 14).</span></span>
2. <span data-ttu-id="e1b22-332">チェック ボックス ラベルの付いた**厳密に型指定されたビューを作成する**です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-332">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
3. <span data-ttu-id="e1b22-333">**コンテンツを表示** ドロップダウン リストで、値を選択*編集*です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-333">From the **View content** dropdown list, select the value *Edit*.</span></span>
4. <span data-ttu-id="e1b22-334">**データ クラスを表示** ドロップダウン リストで、値を選択*MovieApp.Movie*です。</span><span class="sxs-lookup"><span data-stu-id="e1b22-334">From the **View data class** dropdown list, select the value *MovieApp.Movie*.</span></span>
5. <span data-ttu-id="e1b22-335">クリックして、**追加**クリックすると、新しいビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-335">Click the **Add** button to create the new view.</span></span>

<span data-ttu-id="e1b22-336">Views \home フォルダーに Edit.aspx をという名前の新しいビューを追加するこれらの手順を完了します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-336">Completing these steps adds a new view named Edit.aspx to the Views\Home folder.</span></span> <span data-ttu-id="e1b22-337">このビューには、ムービーのレコードを編集するため、HTML フォームが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e1b22-337">This view contains an HTML form for editing a movie record.</span></span>


<span data-ttu-id="e1b22-338">[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="e1b22-338">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)</span></span>

<span data-ttu-id="e1b22-339">**図 14**: エディット ビューの追加 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))</span><span class="sxs-lookup"><span data-stu-id="e1b22-339">**Figure 14**: Adding the Edit view ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="e1b22-340">編集ビューには、ムービーの Id プロパティに対応する HTML フォーム フィールドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e1b22-340">The Edit view contains an HTML form field that corresponds to the Movie Id property.</span></span> <span data-ttu-id="e1b22-341">Id プロパティの値を編集しているユーザーを含めない場合は、あるために、このフォームのフィールドを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-341">Because you don't want people editing the value of the Id property, you should remove this form field.</span></span>


<span data-ttu-id="e1b22-342">最後に、データベース レコードの編集をサポートするように、Home コント ローラーを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b22-342">Finally, we need to modify the Home controller so that it supports editing a database record.</span></span> <span data-ttu-id="e1b22-343">更新後のテンプレートを使用するクラスは、6 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="e1b22-343">The updated HomeController class is contained in Listing 6.</span></span>

<span data-ttu-id="e1b22-344">**6 – Controllers\HomeController.vb (編集メソッド) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="e1b22-344">**Listing 6 – Controllers\HomeController.vb (Edit methods)**</span></span>

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

<span data-ttu-id="e1b22-345">リストの 6 Edit() メソッドの両方のオーバー ロードする追加のロジックを追加しました。</span><span class="sxs-lookup"><span data-stu-id="e1b22-345">In Listing 6, I've added additional logic to both overloads of the Edit() method.</span></span> <span data-ttu-id="e1b22-346">最初の Edit() メソッドは、メソッドに渡される Id パラメーターに対応するムービー データベース レコードを返します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-346">The first Edit() method returns the movie database record that corresponds to the Id parameter passed to the method.</span></span> <span data-ttu-id="e1b22-347">2 番目のオーバー ロードは、データベースのムービーのレコードに更新プログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-347">The second overload performs the updates to a movie record in the database.</span></span>

<span data-ttu-id="e1b22-348">オリジナルのムービーを取得し、データベース内の既存のムービーを更新する、ApplyPropertyChanges() を呼び出す必要がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e1b22-348">Notice that you must retrieve the original movie, and then call ApplyPropertyChanges(), to update the existing movie in the database.</span></span>

## <a name="summary"></a><span data-ttu-id="e1b22-349">まとめ</span><span class="sxs-lookup"><span data-stu-id="e1b22-349">Summary</span></span>

<span data-ttu-id="e1b22-350">このチュートリアルの目的は、ASP.NET MVC アプリケーションの構築のエクスペリエンスの感を与えるでした。</span><span class="sxs-lookup"><span data-stu-id="e1b22-350">The purpose of this tutorial was to give you a sense of the experience of building an ASP.NET MVC application.</span></span> <span data-ttu-id="e1b22-351">ASP.NET MVC web アプリケーションのビルドが、Active Server Pages または ASP.NET アプリケーションの構築のエクスペリエンスとよく似ていますを検出することを願っています。</span><span class="sxs-lookup"><span data-stu-id="e1b22-351">I hope that you discovered that building an ASP.NET MVC web application is very similar to the experience of building an Active Server Pages or ASP.NET application.</span></span>

<span data-ttu-id="e1b22-352">このチュートリアルでは、ASP.NET MVC フレームワークの最も基本的な機能だけを検査します。</span><span class="sxs-lookup"><span data-stu-id="e1b22-352">In this tutorial, we examined only the most basic features of the ASP.NET MVC framework.</span></span> <span data-ttu-id="e1b22-353">今後のチュートリアルでは、ここをさらに深くコント ローラー、コント ローラーのアクション、ビュー、データの表示、および HTML ヘルパーなどのトピックです。</span><span class="sxs-lookup"><span data-stu-id="e1b22-353">In future tutorials, we dive deeper into topics such as controllers, controller actions, views, view data, and HTML helpers.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e1b22-354">前へ</span><span class="sxs-lookup"><span data-stu-id="e1b22-354">Previous</span></span>](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
