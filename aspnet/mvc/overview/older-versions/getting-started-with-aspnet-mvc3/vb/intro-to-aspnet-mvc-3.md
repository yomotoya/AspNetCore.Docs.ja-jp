---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: ASP.NET mvc 3 (VB) |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 3be0de6ea6d49f9c0de659398190b71c36ba222a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870376"
---
<a name="intro-to-aspnet-mvc-3-vb"></a><span data-ttu-id="563ec-103">ASP.NET mvc 3 (VB)</span><span class="sxs-lookup"><span data-stu-id="563ec-103">Intro to ASP.NET MVC 3 (VB)</span></span>
====================
<span data-ttu-id="563ec-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="563ec-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="563ec-105">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="563ec-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="563ec-106">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="563ec-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="563ec-107">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="563ec-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="563ec-108">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="563ec-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="563ec-109">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="563ec-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="563ec-110">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="563ec-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="563ec-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="563ec-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="563ec-112">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="563ec-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="563ec-113">VB.NET のソース コードと Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="563ec-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="563ec-114">[バージョンをダウンロードして VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。</span><span class="sxs-lookup"><span data-stu-id="563ec-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="563ec-115">C# を使用する場合に切り替え、 [c# バージョン](../cs/intro-to-aspnet-mvc-3.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="563ec-115">If you prefer C#, switch to the [C# version](../cs/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="563ec-116">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="563ec-116">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="563ec-117">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="563ec-117">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="563ec-118">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="563ec-118">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="563ec-119">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="563ec-119">Alternatively, you can individually install the prerequisites using the following links:</span></span>

- [<span data-ttu-id="563ec-120">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="563ec-120">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [<span data-ttu-id="563ec-121">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="563ec-121">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- <span data-ttu-id="563ec-122">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="563ec-122">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>

<span data-ttu-id="563ec-123">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="563ec-123">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>

<span data-ttu-id="563ec-124">VB ソース コードを持つ Visual Web Developer プロジェクトは、このトピックの使用可能です。</span><span class="sxs-lookup"><span data-stu-id="563ec-124">A Visual Web Developer project with VB source code is available to accompany this topic.</span></span> <span data-ttu-id="563ec-125">[バージョンをダウンロードして VB ここ](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824)です。</span><span class="sxs-lookup"><span data-stu-id="563ec-125">[Download the VB version here](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824).</span></span> <span data-ttu-id="563ec-126">CSharp の場合に切り替え、 [CSharp バージョン](../cs/intro-to-aspnet-mvc-3.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="563ec-126">If you prefer CSharp, switch to the [CSharp version](../cs/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="563ec-127">新機能のビルドします。</span><span class="sxs-lookup"><span data-stu-id="563ec-127">What You'll Build</span></span>

<span data-ttu-id="563ec-128">実装する単純なムービー一覧アプリケーションの作成、編集、およびデータベースからムービーの一覧表示をサポートします。</span><span class="sxs-lookup"><span data-stu-id="563ec-128">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="563ec-129">アプリケーションのビルドの 2 つのスクリーン ショットを次に示します。</span><span class="sxs-lookup"><span data-stu-id="563ec-129">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="563ec-130">データベースからムービーの一覧を表示するページが含まれています。</span><span class="sxs-lookup"><span data-stu-id="563ec-130">It includes a page that displays a list of movies from a database:</span></span>

<span data-ttu-id="563ec-131">[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="563ec-131">[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)</span></span>

<span data-ttu-id="563ec-132">アプリケーションでは、追加、編集、およびムービー、だけでなく 1 つずつに関する詳細を参照してくださいを削除することもできます。</span><span class="sxs-lookup"><span data-stu-id="563ec-132">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="563ec-133">データ エントリのすべてのシナリオには、データベースに格納されたデータが正しいことを確認する検証が含まれます。</span><span class="sxs-lookup"><span data-stu-id="563ec-133">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

<span data-ttu-id="563ec-134">[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="563ec-134">[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="563ec-135">スキルの学習</span><span class="sxs-lookup"><span data-stu-id="563ec-135">Skills You'll Learn</span></span>

<span data-ttu-id="563ec-136">学習する内容を次に示します。</span><span class="sxs-lookup"><span data-stu-id="563ec-136">Here's what you'll learn:</span></span>

- <span data-ttu-id="563ec-137">新しい ASP.NET MVC プロジェクトを作成する方法</span><span class="sxs-lookup"><span data-stu-id="563ec-137">How to create a new ASP.NET MVC project</span></span>
- <span data-ttu-id="563ec-138">Entity Framework コード優先を使用して新しいデータベースを作成する方法</span><span class="sxs-lookup"><span data-stu-id="563ec-138">How to create a new database using Entity Framework code-first</span></span>
- <span data-ttu-id="563ec-139">ASP.NET MVC のコント ローラーとビューを作成する方法</span><span class="sxs-lookup"><span data-stu-id="563ec-139">How to create ASP.NET MVC controllers and views</span></span>
- <span data-ttu-id="563ec-140">データ取得して表示する方法</span><span class="sxs-lookup"><span data-stu-id="563ec-140">How to retrieve and display data</span></span>
- <span data-ttu-id="563ec-141">データを編集し、データの検証を有効にする方法</span><span class="sxs-lookup"><span data-stu-id="563ec-141">How to edit data and enable data validation</span></span>

## <a name="getting-started"></a><span data-ttu-id="563ec-142">作業の開始</span><span class="sxs-lookup"><span data-stu-id="563ec-142">Getting Started</span></span>

<span data-ttu-id="563ec-143">Visual Web Developer 2010 Express (略しての"VWD") を実行して起動し、選択**新しいプロジェクト**から、**開始**ページ。</span><span class="sxs-lookup"><span data-stu-id="563ec-143">Start by running Visual Web Developer 2010 Express ("VWD" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="563ec-144">Visual Web Developer は、IDE、または統合開発環境です。</span><span class="sxs-lookup"><span data-stu-id="563ec-144">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="563ec-145">Microsoft Word を使用してドキュメントを作成するのにのと同じようにアプリケーションを作成するのに、IDE を使用します。</span><span class="sxs-lookup"><span data-stu-id="563ec-145">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="563ec-146">Visual Web Developer を使用できるさまざまなオプションを示す上部にツールバーがあります。</span><span class="sxs-lookup"><span data-stu-id="563ec-146">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="563ec-147">IDE でのタスクを実行する別の方法を提供するメニューもあります。</span><span class="sxs-lookup"><span data-stu-id="563ec-147">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="563ec-148">(選択する代わりに、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **新しいプロジェクト**.)</span><span class="sxs-lookup"><span data-stu-id="563ec-148">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="563ec-149">初めてアプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="563ec-149">Creating Your First Application</span></span>

<span data-ttu-id="563ec-150">プログラミング言語と Visual Basic または Visual c# のいずれかの選択を使用してアプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="563ec-150">You can create applications using your choice of either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="563ec-151">このチュートリアルでは、左側、Visual Basic を選択し、選択**ASP.NET MVC 3 Web アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="563ec-151">For this tutorial, select Visual Basic on the left, then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="563ec-152">プロジェクト"MvcMovie"の名前を指定し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="563ec-152">Name your project "MvcMovie" and then click **OK**.</span></span>

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="563ec-154">**新しい ASP.NET MVC 3 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="563ec-154">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="563ec-155">ままにして**Razor**既定のビュー エンジンとして。</span><span class="sxs-lookup"><span data-stu-id="563ec-155">Leave **Razor** as the default view engine.</span></span>

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

<span data-ttu-id="563ec-157">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="563ec-157">Click **OK**.</span></span> <span data-ttu-id="563ec-158">Visual Web Developer では、作成した ASP.NET MVC プロジェクトの既定のテンプレートが使用されるため、何もせず作業アプリケーションが現在ある!</span><span class="sxs-lookup"><span data-stu-id="563ec-158">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="563ec-159">これは、単純です"Hello World!"</span><span class="sxs-lookup"><span data-stu-id="563ec-159">This is a simple "Hello World!"</span></span> <span data-ttu-id="563ec-160">プロジェクト、・ アプリケーションを起動するに適しています。</span><span class="sxs-lookup"><span data-stu-id="563ec-160">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="563ec-161">**[デバッグ]** メニューの **[デバッグの開始]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="563ec-161">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image11.png)

<span data-ttu-id="563ec-162">デバッグを開始するキーボード ショートカットは、f5 キーであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="563ec-162">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="563ec-163">F5 キーでは、開発 web サーバーを起動し、web アプリケーションを実行する Visual Web Developer が発生します。</span><span class="sxs-lookup"><span data-stu-id="563ec-163">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="563ec-164">VWD に、ブラウザーを起動し、アプリケーションのホーム ページが開きます。</span><span class="sxs-lookup"><span data-stu-id="563ec-164">VWD then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="563ec-165">ブラウザーのアドレス バーに表示される`localhost`などのメカニズムではありませんし`example.com`です。</span><span class="sxs-lookup"><span data-stu-id="563ec-165">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="563ec-166">これはため`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。</span><span class="sxs-lookup"><span data-stu-id="563ec-166">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="563ec-167">VWD web プロジェクトの実行時に、ランダムなポートがプロジェクトに使用されます。</span><span class="sxs-lookup"><span data-stu-id="563ec-167">When VWD runs a web project, a random port is used for the project.</span></span> <span data-ttu-id="563ec-168">次の図では、ランダムなポート番号は、43246 がします。</span><span class="sxs-lookup"><span data-stu-id="563ec-168">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="563ec-169">プロジェクトでは、別のポート番号は使用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="563ec-169">Your project will probably use a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image12.png)

<span data-ttu-id="563ec-170">すぐは、この既定のテンプレートは、する 2 つのページにアクセスし、基本的なログイン ページを示します。</span><span class="sxs-lookup"><span data-stu-id="563ec-170">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="563ec-171">このアプリケーションの動作を変更して、プロセスで ASP.NET MVC について少し説明しましょう。</span><span class="sxs-lookup"><span data-stu-id="563ec-171">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="563ec-172">ブラウザーを閉じて、いくつかのコードを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="563ec-172">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="563ec-173">次へ</span><span class="sxs-lookup"><span data-stu-id="563ec-173">Next</span></span>](adding-a-controller.md)
