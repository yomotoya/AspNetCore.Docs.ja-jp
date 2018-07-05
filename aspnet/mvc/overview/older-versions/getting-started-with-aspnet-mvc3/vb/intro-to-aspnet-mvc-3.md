---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: ASP.NET MVC 3 (VB) の概要 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 1a870f10d39980335c7acb4a543907fecd408eb2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372354"
---
<a name="intro-to-aspnet-mvc-3-vb"></a><span data-ttu-id="782d8-103">ASP.NET MVC 3 (VB) の概要</span><span class="sxs-lookup"><span data-stu-id="782d8-103">Intro to ASP.NET MVC 3 (VB)</span></span>
====================
<span data-ttu-id="782d8-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="782d8-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="782d8-105">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="782d8-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="782d8-106">始める前に、以下の前提条件がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="782d8-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="782d8-107">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="782d8-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="782d8-108">または、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="782d8-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="782d8-109">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="782d8-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="782d8-110">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="782d8-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="782d8-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="782d8-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="782d8-112">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="782d8-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="782d8-113">VB.NET のソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="782d8-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="782d8-114">[VB.NET のバージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。</span><span class="sxs-lookup"><span data-stu-id="782d8-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="782d8-115">C# を使用する場合に切り替えて、 [c# バージョン](../cs/intro-to-aspnet-mvc-3.md)このチュートリアルの。</span><span class="sxs-lookup"><span data-stu-id="782d8-115">If you prefer C#, switch to the [C# version](../cs/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="782d8-116">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="782d8-116">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="782d8-117">始める前に、以下の前提条件がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="782d8-117">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="782d8-118">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="782d8-118">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="782d8-119">または、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="782d8-119">Alternatively, you can individually install the prerequisites using the following links:</span></span>

- [<span data-ttu-id="782d8-120">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="782d8-120">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [<span data-ttu-id="782d8-121">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="782d8-121">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- <span data-ttu-id="782d8-122">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="782d8-122">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>

<span data-ttu-id="782d8-123">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="782d8-123">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>

<span data-ttu-id="782d8-124">VB ソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="782d8-124">A Visual Web Developer project with VB source code is available to accompany this topic.</span></span> <span data-ttu-id="782d8-125">[ここで、VB バージョンのダウンロード](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824)します。</span><span class="sxs-lookup"><span data-stu-id="782d8-125">[Download the VB version here](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824).</span></span> <span data-ttu-id="782d8-126">CSharp を使用する場合に切り替えて、 [CSharp バージョン](../cs/intro-to-aspnet-mvc-3.md)このチュートリアルの。</span><span class="sxs-lookup"><span data-stu-id="782d8-126">If you prefer CSharp, switch to the [CSharp version](../cs/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="782d8-127">構築します</span><span class="sxs-lookup"><span data-stu-id="782d8-127">What You'll Build</span></span>

<span data-ttu-id="782d8-128">作成、編集、およびデータベースからムービーを一覧表示をサポートする簡単なムービー リスト アプリケーションを実装します。</span><span class="sxs-lookup"><span data-stu-id="782d8-128">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="782d8-129">アプリケーションをビルドの 2 つのスクリーン ショットを次に示します。</span><span class="sxs-lookup"><span data-stu-id="782d8-129">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="782d8-130">データベースからムービーの一覧を表示するページが含まれています。</span><span class="sxs-lookup"><span data-stu-id="782d8-130">It includes a page that displays a list of movies from a database:</span></span>

<span data-ttu-id="782d8-131">[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="782d8-131">[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)</span></span>

<span data-ttu-id="782d8-132">アプリケーションでは、追加、編集、および個別の詳細を参照するくださいと同様に、ムービーを削除することもできます。</span><span class="sxs-lookup"><span data-stu-id="782d8-132">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="782d8-133">すべてのデータ入力シナリオには、データベースに格納されたデータが正しいことを確認する検証が含まれます。</span><span class="sxs-lookup"><span data-stu-id="782d8-133">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

<span data-ttu-id="782d8-134">[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="782d8-134">[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="782d8-135">学習内容</span><span class="sxs-lookup"><span data-stu-id="782d8-135">Skills You'll Learn</span></span>

<span data-ttu-id="782d8-136">学習内容を次に示します。</span><span class="sxs-lookup"><span data-stu-id="782d8-136">Here's what you'll learn:</span></span>

- <span data-ttu-id="782d8-137">新しい ASP.NET MVC プロジェクトを作成する方法</span><span class="sxs-lookup"><span data-stu-id="782d8-137">How to create a new ASP.NET MVC project</span></span>
- <span data-ttu-id="782d8-138">Entity Framework code first を使用して新しいデータベースを作成する方法</span><span class="sxs-lookup"><span data-stu-id="782d8-138">How to create a new database using Entity Framework code-first</span></span>
- <span data-ttu-id="782d8-139">ASP.NET MVC のコント ローラーとビューを作成する方法</span><span class="sxs-lookup"><span data-stu-id="782d8-139">How to create ASP.NET MVC controllers and views</span></span>
- <span data-ttu-id="782d8-140">データ取得して表示する方法</span><span class="sxs-lookup"><span data-stu-id="782d8-140">How to retrieve and display data</span></span>
- <span data-ttu-id="782d8-141">データを編集し、データの検証を有効にする方法</span><span class="sxs-lookup"><span data-stu-id="782d8-141">How to edit data and enable data validation</span></span>

## <a name="getting-started"></a><span data-ttu-id="782d8-142">作業の開始</span><span class="sxs-lookup"><span data-stu-id="782d8-142">Getting Started</span></span>

<span data-ttu-id="782d8-143">Visual Web Developer 2010 Express (略しての"VWD") を実行して起動し、選択**新しいプロジェクト**から、**開始**ページ。</span><span class="sxs-lookup"><span data-stu-id="782d8-143">Start by running Visual Web Developer 2010 Express ("VWD" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="782d8-144">Visual Web Developer は、IDE、または統合開発環境です。</span><span class="sxs-lookup"><span data-stu-id="782d8-144">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="782d8-145">Microsoft Word を使用してドキュメントを記述するのにのと同じようにアプリケーションを作成、IDE を使用します。</span><span class="sxs-lookup"><span data-stu-id="782d8-145">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="782d8-146">Visual Web Developer を使用できるさまざまなオプションを示す上部のツールバーがあります。</span><span class="sxs-lookup"><span data-stu-id="782d8-146">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="782d8-147">また、IDE でタスクを実行する別の方法を提供するメニューがあります。</span><span class="sxs-lookup"><span data-stu-id="782d8-147">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="782d8-148">(選択ではなく、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **の新しいプロジェクト**.)</span><span class="sxs-lookup"><span data-stu-id="782d8-148">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="782d8-149">最初のアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="782d8-149">Creating Your First Application</span></span>

<span data-ttu-id="782d8-150">プログラミング言語として Visual Basic または Visual c# のいずれかの好みを使用してアプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="782d8-150">You can create applications using your choice of either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="782d8-151">このチュートリアルでは、左側で、Visual Basic を選択し、選択**ASP.NET MVC 3 Web アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="782d8-151">For this tutorial, select Visual Basic on the left, then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="782d8-152">クリックして、プロジェクトに"MvcMovie" **OK**します。</span><span class="sxs-lookup"><span data-stu-id="782d8-152">Name your project "MvcMovie" and then click **OK**.</span></span>

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="782d8-154">**新しい ASP.NET MVC 3 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="782d8-154">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="782d8-155">まま**Razor**既定のビュー エンジンとして。</span><span class="sxs-lookup"><span data-stu-id="782d8-155">Leave **Razor** as the default view engine.</span></span>

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

<span data-ttu-id="782d8-157">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="782d8-157">Click **OK**.</span></span> <span data-ttu-id="782d8-158">Visual Web Developer では、作成した ASP.NET MVC プロジェクトの既定のテンプレートが使用されるので何もせず、実用的なアプリケーションを今すぐ必要する!</span><span class="sxs-lookup"><span data-stu-id="782d8-158">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="782d8-159">これは、単純です"Hello World!"</span><span class="sxs-lookup"><span data-stu-id="782d8-159">This is a simple "Hello World!"</span></span> <span data-ttu-id="782d8-160">プロジェクト、およびそのアプリケーションにお勧めです。</span><span class="sxs-lookup"><span data-stu-id="782d8-160">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="782d8-161">**[デバッグ]** メニューの **[デバッグの開始]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="782d8-161">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image11.png)

<span data-ttu-id="782d8-162">デバッグを開始するキーボード ショートカットは、f5 キーであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="782d8-162">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="782d8-163">F5 キーは、Visual Web Developer を開発 web サーバーを起動し、web アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="782d8-163">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="782d8-164">VWD は、ブラウザーを起動し、アプリケーションのホーム ページを開きます。</span><span class="sxs-lookup"><span data-stu-id="782d8-164">VWD then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="782d8-165">ブラウザーのアドレス バーが表示されますが`localhost`ようなものではありません`example.com`します。</span><span class="sxs-lookup"><span data-stu-id="782d8-165">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="782d8-166">だ`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。</span><span class="sxs-lookup"><span data-stu-id="782d8-166">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="782d8-167">VWD が web プロジェクトを実行すると、プロジェクトのランダムなポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="782d8-167">When VWD runs a web project, a random port is used for the project.</span></span> <span data-ttu-id="782d8-168">次の図では、ランダムのポート番号は、43246 です。</span><span class="sxs-lookup"><span data-stu-id="782d8-168">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="782d8-169">プロジェクトでは、別のポート番号は使用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="782d8-169">Your project will probably use a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image12.png)

<span data-ttu-id="782d8-170">この既定のテンプレートはすぐする 2 つのページにアクセスして、基本的なログイン ページを示します。</span><span class="sxs-lookup"><span data-stu-id="782d8-170">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="782d8-171">このアプリケーションの動作を変更して、プロセスで ASP.NET MVC についてもう少し説明します。</span><span class="sxs-lookup"><span data-stu-id="782d8-171">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="782d8-172">ブラウザーを閉じて、いくつかのコードを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="782d8-172">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="782d8-173">次へ</span><span class="sxs-lookup"><span data-stu-id="782d8-173">Next</span></span>](adding-a-controller.md)
