---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: ASP.NET mvc 4 |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルは Visual Studio 2013 を使用して、ここで使用可能な場合は、更新されたバージョンです。 新しいチュートリアルを使用して ASP.NET MVC 5 は、t に対して多くの機能強化を提供しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 519bac22ba2607931c5f3123b9b567859a2b3d1c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-4"></a><span data-ttu-id="f39f2-104">ASP.NET mvc 4</span><span class="sxs-lookup"><span data-stu-id="f39f2-104">Intro to ASP.NET MVC 4</span></span>
====================
<span data-ttu-id="f39f2-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="f39f2-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="f39f2-106">このチュートリアルでは使用可能な場合、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)です。</span><span class="sxs-lookup"><span data-stu-id="f39f2-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="f39f2-107">新しいチュートリアルでは、ASP.NET MVC 5 は、このチュートリアルを超える多くの機能強化を提供を使用します。</span><span class="sxs-lookup"><span data-stu-id="f39f2-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> <span data-ttu-id="f39f2-108">このチュートリアルが Microsoft を使用して ASP.NET MVC 4 Web アプリケーションの構築の基礎を教える[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)または Visual Web Developer 2010 Express Service Pack 1。</span><span class="sxs-lookup"><span data-stu-id="f39f2-108">This tutorial will teach you the basics of building an ASP.NET MVC 4 Web application using Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) or Visual Web Developer 2010 Express Service Pack 1.</span></span> <span data-ttu-id="f39f2-109">チュートリアルを完了できるものをインストールする必要はありませんが、visual Studio 2012 がお勧めします。</span><span class="sxs-lookup"><span data-stu-id="f39f2-109">Visual Studio 2012 is recommended, you won't need to install anything to complete the tutorial.</span></span> <span data-ttu-id="f39f2-110">Visual Studio 2010 を使用している場合は、以下のコンポーネントをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f39f2-110">If you are using Visual Studio 2010 you must install the components below.</span></span> <span data-ttu-id="f39f2-111">次のリンクをクリックしてそれらのすべてをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="f39f2-111">You can install all of them by clicking the following links:</span></span>
> 
> - [<span data-ttu-id="f39f2-112">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="f39f2-112">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="f39f2-113">ASP.NET MVC 4 の WPI インストーラー</span><span class="sxs-lookup"><span data-stu-id="f39f2-113">WPI installer for ASP.NET MVC 4</span></span>](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [<span data-ttu-id="f39f2-114">LocalDB</span><span class="sxs-lookup"><span data-stu-id="f39f2-114">LocalDB</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [<span data-ttu-id="f39f2-115">SSDT</span><span class="sxs-lookup"><span data-stu-id="f39f2-115">SSDT</span></span>](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> <span data-ttu-id="f39f2-116">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、インストール、 [ASP.NET MVC 4 の WPI インストーラー](https://go.microsoft.com/fwlink/?LinkId=243392)と: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)</span><span class="sxs-lookup"><span data-stu-id="f39f2-116">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the [WPI installer for ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) and the: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)</span></span>
> 
> <span data-ttu-id="f39f2-117">C# ソース コードの Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f39f2-117">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="f39f2-118">[C# バージョンをダウンロード](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip)です。</span><span class="sxs-lookup"><span data-stu-id="f39f2-118">[Download the C# version](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).</span></span>
> 
> <span data-ttu-id="f39f2-119">このチュートリアルでは、Visual Studio でアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="f39f2-119">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="f39f2-120">利用できるアプリケーション、インターネット経由でホスティング プロバイダーに展開することで。</span><span class="sxs-lookup"><span data-stu-id="f39f2-120">You can also make the application available over the Internet by deploying it to a hosting provider.</span></span> <span data-ttu-id="f39f2-121">マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、[試用アカウントを Windows Azure の無料](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)です。</span><span class="sxs-lookup"><span data-stu-id="f39f2-121">Microsoft offers free web hosting for up to 10 web sites in a [free Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="f39f2-122">Windows Azure Web サイトへの Visual Studio web プロジェクトを配置する方法については、次を参照してください。[を作成すると、ASP.NET web サイトと Visual Studio での SQL データベースを展開](https://docs.microsoft.com/dotnet/azure/)です。</span><span class="sxs-lookup"><span data-stu-id="f39f2-122">For information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create and deploy an ASP.NET web site and SQL Database with Visual Studio](https://docs.microsoft.com/dotnet/azure/).</span></span> <span data-ttu-id="f39f2-123">そのチュートリアルでは、Entity Framework Code First Migrations を使用して Windows Azure SQL データベース (以前の SQL Azure) に、SQL Server データベースを展開する方法も示します。</span><span class="sxs-lookup"><span data-stu-id="f39f2-123">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Windows Azure SQL Database (formerly SQL Azure).</span></span>
> 
> <span data-ttu-id="f39f2-124">このチュートリアルは、Rick Anderson によって書き込まれました ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="f39f2-124">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ).</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="f39f2-125">新機能のビルドします。</span><span class="sxs-lookup"><span data-stu-id="f39f2-125">What You'll Build</span></span>

> [!NOTE]
> <span data-ttu-id="f39f2-126">このチュートリアルでは使用可能な場合、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)です。</span><span class="sxs-lookup"><span data-stu-id="f39f2-126">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="f39f2-127">新しいチュートリアルでは、ASP.NET MVC 5 は、このチュートリアルを超える多くの機能強化を提供を使用します。</span><span class="sxs-lookup"><span data-stu-id="f39f2-127">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>


<span data-ttu-id="f39f2-128">作成、編集、検索、およびデータベースからムービーの一覧表示をサポートする単純なムービー リスト アプリケーション実装します。</span><span class="sxs-lookup"><span data-stu-id="f39f2-128">You'll implement a simple movie-listing application that supports creating, editing, searching and listing movies from a database.</span></span> <span data-ttu-id="f39f2-129">アプリケーションのビルドの 2 つのスクリーン ショットを次に示します。</span><span class="sxs-lookup"><span data-stu-id="f39f2-129">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="f39f2-130">データベースからムービーの一覧を表示するページが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f39f2-130">It includes a page that displays a list of movies from a database:</span></span>

![](intro-to-aspnet-mvc-4/_static/image1.png)

<span data-ttu-id="f39f2-131">アプリケーションでは、追加、編集、およびムービー、だけでなく 1 つずつに関する詳細を参照してくださいを削除することもできます。</span><span class="sxs-lookup"><span data-stu-id="f39f2-131">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="f39f2-132">データ エントリのすべてのシナリオには、データベースに格納されたデータが正しいことを確認する検証が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f39f2-132">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a><span data-ttu-id="f39f2-133">作業の開始</span><span class="sxs-lookup"><span data-stu-id="f39f2-133">Getting Started</span></span>

<span data-ttu-id="f39f2-134">Visual Studio Express 2012 または Visual Web Developer 2010 Express を実行して開始します。</span><span class="sxs-lookup"><span data-stu-id="f39f2-134">Start by running Visual Studio Express 2012 or Visual Web Developer 2010 Express.</span></span> <span data-ttu-id="f39f2-135">この系列を Visual Studio Express 2012 がスクリーン ショットのほとんどは、Visual Studio 2010 SP1、Visual Studio 2012、Visual Studio Express 2012 または Visual Web Developer 2010 Express を使用してこのチュートリアルを完了できます。</span><span class="sxs-lookup"><span data-stu-id="f39f2-135">Most of the screen shots in this series use Visual Studio Express 2012, but you can complete this tutorial with Visual Studio 2010/SP1, Visual Studio 2012, Visual Studio Express 2012 or Visual Web Developer 2010 Express.</span></span> <span data-ttu-id="f39f2-136">選択**新しいプロジェクト**から、**開始**ページ。</span><span class="sxs-lookup"><span data-stu-id="f39f2-136">Select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="f39f2-137">Visual Studio は、IDE、または統合開発環境です。</span><span class="sxs-lookup"><span data-stu-id="f39f2-137">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="f39f2-138">Microsoft Word を使用してドキュメントを作成するのにのと同じようにアプリケーションを作成するのに、IDE を使用します。</span><span class="sxs-lookup"><span data-stu-id="f39f2-138">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="f39f2-139">Visual Studio を使用できるさまざまなオプションを示す上部にツールバーがあります。</span><span class="sxs-lookup"><span data-stu-id="f39f2-139">In Visual Studio there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="f39f2-140">IDE でのタスクを実行する別の方法を提供するメニューもあります。</span><span class="sxs-lookup"><span data-stu-id="f39f2-140">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="f39f2-141">(選択する代わりに、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **新しいプロジェクト**.)</span><span class="sxs-lookup"><span data-stu-id="f39f2-141">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="f39f2-142">初めてアプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="f39f2-142">Creating Your First Application</span></span>

<span data-ttu-id="f39f2-143">プログラミング言語と Visual Basic または Visual c# を使用してアプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="f39f2-143">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="f39f2-144">左側の Visual c# を選択し、 **ASP.NET MVC 4 Web Application**です。</span><span class="sxs-lookup"><span data-stu-id="f39f2-144">Select Visual C# on the left and then select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="f39f2-145">プロジェクトの名前を付けます&quot;MvcMovie&quot;  をクリックし、 **ok**です。</span><span class="sxs-lookup"><span data-stu-id="f39f2-145">Name your project &quot;MvcMovie&quot; and then click **OK**.</span></span>

![](intro-to-aspnet-mvc-4/_static/image4.png)

<span data-ttu-id="f39f2-146">**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="f39f2-146">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="f39f2-147">ままにして**Razor**既定のビュー エンジンとして。</span><span class="sxs-lookup"><span data-stu-id="f39f2-147">Leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-4/_static/image5.png)

<span data-ttu-id="f39f2-148">**[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f39f2-148">Click **OK**.</span></span> <span data-ttu-id="f39f2-149">Visual Studio では、作成した ASP.NET MVC プロジェクトの既定のテンプレートが使用されるため、何もせず作業アプリケーションが現在ある!</span><span class="sxs-lookup"><span data-stu-id="f39f2-149">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="f39f2-150">これは、単純な&quot;Hello World!&quot;プロジェクト、およびそのアプリケーションを起動するに適しています。</span><span class="sxs-lookup"><span data-stu-id="f39f2-150">This is a simple &quot;Hello World!&quot; project, and it's a good place to start your application.</span></span>

![](intro-to-aspnet-mvc-4/_static/image6.png)

<span data-ttu-id="f39f2-151">**[デバッグ]** メニューの **[デバッグの開始]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f39f2-151">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-4/_static/image7.png)

<span data-ttu-id="f39f2-152">デバッグを開始するキーボード ショートカットは、f5 キーであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f39f2-152">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="f39f2-153">F5 キーでは、IIS Express を起動し、web アプリケーションを実行する Visual Studio が発生します。</span><span class="sxs-lookup"><span data-stu-id="f39f2-153">F5 causes Visual Studio to start IIS Express and run your web application.</span></span> <span data-ttu-id="f39f2-154">Visual Studio は、ブラウザーを起動し、アプリケーションのホーム ページが開きます。</span><span class="sxs-lookup"><span data-stu-id="f39f2-154">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="f39f2-155">ブラウザーのアドレス バーに表示される`localhost`などのメカニズムではありませんし`example.com`です。</span><span class="sxs-lookup"><span data-stu-id="f39f2-155">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="f39f2-156">これはため`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。</span><span class="sxs-lookup"><span data-stu-id="f39f2-156">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="f39f2-157">Visual Studio web プロジェクトの実行時に、ランダムなポートが、web サーバーに使用されます。</span><span class="sxs-lookup"><span data-stu-id="f39f2-157">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f39f2-158">次の図では、ポート番号は、41788 です。</span><span class="sxs-lookup"><span data-stu-id="f39f2-158">In the image below, the port number is 41788.</span></span> <span data-ttu-id="f39f2-159">アプリケーションを実行するときに、別のポート番号をおそらく表示されます。</span><span class="sxs-lookup"><span data-stu-id="f39f2-159">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-4/_static/image8.png)

<span data-ttu-id="f39f2-160">すぐこの既定テンプレートを使用して自宅、連絡先、についてのページです。</span><span class="sxs-lookup"><span data-stu-id="f39f2-160">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="f39f2-161">また、登録および、ログ記録のサポートを提供し、Facebook、Twitter へのリンクします。</span><span class="sxs-lookup"><span data-stu-id="f39f2-161">It also provides support to register and log in, and links to Facebook and Twitter.</span></span> <span data-ttu-id="f39f2-162">次の手順では、このアプリケーションの動作を変更し、ASP.NET MVC について少し説明します。</span><span class="sxs-lookup"><span data-stu-id="f39f2-162">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="f39f2-163">ブラウザーを閉じて、いくつかのコードを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f39f2-163">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f39f2-164">次へ</span><span class="sxs-lookup"><span data-stu-id="f39f2-164">Next</span></span>](adding-a-controller.md)
