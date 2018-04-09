---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: ASP.NET mvc 3 (c#) |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 9b965df6175051a084de35627160161c116be42d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="badcf-103">ASP.NET mvc 3 (c#)</span><span class="sxs-lookup"><span data-stu-id="badcf-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="badcf-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="badcf-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="badcf-105">このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="badcf-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="badcf-106">より安全な非常に簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="badcf-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="badcf-107">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="badcf-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="badcf-108">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="badcf-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="badcf-109">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="badcf-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="badcf-110">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="badcf-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="badcf-111">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="badcf-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="badcf-112">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="badcf-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="badcf-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="badcf-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="badcf-114">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="badcf-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="badcf-115">C# ソース コードの Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="badcf-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="badcf-116">[C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。</span><span class="sxs-lookup"><span data-stu-id="badcf-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="badcf-117">Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="badcf-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="badcf-118">新機能のビルドします。</span><span class="sxs-lookup"><span data-stu-id="badcf-118">What You'll Build</span></span>

<span data-ttu-id="badcf-119">実装する単純なムービー一覧アプリケーションの作成、編集、およびデータベースからムービーの一覧表示をサポートします。</span><span class="sxs-lookup"><span data-stu-id="badcf-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="badcf-120">アプリケーションのビルドの 2 つのスクリーン ショットを次に示します。</span><span class="sxs-lookup"><span data-stu-id="badcf-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="badcf-121">データベースからムービーの一覧を表示するページが含まれています。</span><span class="sxs-lookup"><span data-stu-id="badcf-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="badcf-123">アプリケーションでは、追加、編集、およびムービー、だけでなく 1 つずつに関する詳細を参照してくださいを削除することもできます。</span><span class="sxs-lookup"><span data-stu-id="badcf-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="badcf-124">データ エントリのすべてのシナリオには、データベースに格納されたデータが正しいことを確認する検証が含まれます。</span><span class="sxs-lookup"><span data-stu-id="badcf-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="badcf-125">スキルの学習</span><span class="sxs-lookup"><span data-stu-id="badcf-125">Skills You'll Learn</span></span>

<span data-ttu-id="badcf-126">学習する内容を次に示します。</span><span class="sxs-lookup"><span data-stu-id="badcf-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="badcf-127">新しい ASP.NET MVC プロジェクトを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="badcf-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="badcf-128">コント ローラーとビューは、ASP.NET MVC を作成する方法。</span><span class="sxs-lookup"><span data-stu-id="badcf-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="badcf-129">Entity Framework Code First のパラダイムを使用して新しいデータベースを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="badcf-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="badcf-130">取得する方法とデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="badcf-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="badcf-131">データを編集して、データの検証を有効にする方法。</span><span class="sxs-lookup"><span data-stu-id="badcf-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="badcf-132">作業の開始</span><span class="sxs-lookup"><span data-stu-id="badcf-132">Getting Started</span></span>

<span data-ttu-id="badcf-133">Visual Web Developer 2010 Express ("Visual Web Developer"形) を実行して起動し、選択**新しいプロジェクト**から、**開始**ページ。</span><span class="sxs-lookup"><span data-stu-id="badcf-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="badcf-134">Visual Web Developer は、IDE、または統合開発環境です。</span><span class="sxs-lookup"><span data-stu-id="badcf-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="badcf-135">Microsoft Word を使用してドキュメントを作成するのにのと同じようにアプリケーションを作成するのに、IDE を使用します。</span><span class="sxs-lookup"><span data-stu-id="badcf-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="badcf-136">Visual Web Developer を使用できるさまざまなオプションを示す上部にツールバーがあります。</span><span class="sxs-lookup"><span data-stu-id="badcf-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="badcf-137">IDE でのタスクを実行する別の方法を提供するメニューもあります。</span><span class="sxs-lookup"><span data-stu-id="badcf-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="badcf-138">(選択する代わりに、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **新しいプロジェクト**.)</span><span class="sxs-lookup"><span data-stu-id="badcf-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="badcf-139">初めてアプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="badcf-139">Creating Your First Application</span></span>

<span data-ttu-id="badcf-140">プログラミング言語と Visual Basic または Visual c# を使用してアプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="badcf-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="badcf-141">左側の Visual c# を選択し、 **ASP.NET MVC 3 Web アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="badcf-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="badcf-142">プロジェクト"MvcMovie"の名前を指定し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="badcf-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="badcf-143">(Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルのです)。</span><span class="sxs-lookup"><span data-stu-id="badcf-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="badcf-144">**新しい ASP.NET MVC 3 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="badcf-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="badcf-145">確認**を使用して HTML5 マークアップ**のままにして**Razor**既定のビュー エンジンとして。</span><span class="sxs-lookup"><span data-stu-id="badcf-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="badcf-146">**[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="badcf-146">Click **OK**.</span></span> <span data-ttu-id="badcf-147">Visual Web Developer では、作成した ASP.NET MVC プロジェクトの既定のテンプレートが使用されるため、何もせず作業アプリケーションが現在ある!</span><span class="sxs-lookup"><span data-stu-id="badcf-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="badcf-148">これは、単純です"Hello World!"</span><span class="sxs-lookup"><span data-stu-id="badcf-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="badcf-149">プロジェクト、・ アプリケーションを起動するに適しています。</span><span class="sxs-lookup"><span data-stu-id="badcf-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="badcf-150">**[デバッグ]** メニューの **[デバッグの開始]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="badcf-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="badcf-151">デバッグを開始するキーボード ショートカットは、f5 キーであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="badcf-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="badcf-152">F5 キーでは、開発 web サーバーを起動し、web アプリケーションを実行する Visual Web Developer が発生します。</span><span class="sxs-lookup"><span data-stu-id="badcf-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="badcf-153">Visual Web Developer は、ブラウザーを起動し、アプリケーションのホーム ページが開きます。</span><span class="sxs-lookup"><span data-stu-id="badcf-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="badcf-154">ブラウザーのアドレス バーに表示される`localhost`などのメカニズムではありませんし`example.com`です。</span><span class="sxs-lookup"><span data-stu-id="badcf-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="badcf-155">これはため`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。</span><span class="sxs-lookup"><span data-stu-id="badcf-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="badcf-156">Visual Web Developer は、web プロジェクトを実行する web サーバーのランダムなポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="badcf-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="badcf-157">次の図では、ランダムなポート番号は、43246 がします。</span><span class="sxs-lookup"><span data-stu-id="badcf-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="badcf-158">アプリケーションを実行するときに、別のポート番号をおそらく表示されます。</span><span class="sxs-lookup"><span data-stu-id="badcf-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="badcf-159">すぐこの既定のテンプレートには 2 つのページにアクセスして、基本的なログイン ページ。</span><span class="sxs-lookup"><span data-stu-id="badcf-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="badcf-160">次の手順では、このアプリケーションの動作を変更し、プロセスで ASP.NET MVC について少し説明します。</span><span class="sxs-lookup"><span data-stu-id="badcf-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="badcf-161">ブラウザーを閉じて、いくつかのコードを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="badcf-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="badcf-162">次へ</span><span class="sxs-lookup"><span data-stu-id="badcf-162">Next</span></span>](adding-a-controller.md)
