---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: ASP.NET MVC 4 の概要 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Visual Studio 2013 を使用して、ここで使用可能な場合は、更新されたバージョン。 新しいチュートリアルでは、t に多くの機能強化を提供する ASP.NET MVC 5 を使用しています.
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: ea3d1517192ded0e5372c49897bb1fec33324b6f
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912398"
---
<a name="intro-to-aspnet-mvc-4"></a><span data-ttu-id="e2be4-104">ASP.NET MVC 4 の概要</span><span class="sxs-lookup"><span data-stu-id="e2be4-104">Intro to ASP.NET MVC 4</span></span>
====================
<span data-ttu-id="e2be4-105">によって[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="e2be4-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="e2be4-106">このチュートリアルでは、使用可能な場合は、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="e2be4-107">新しいチュートリアルでは、このチュートリアルで多くの機能強化を提供する ASP.NET MVC 5 を使用します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
>
> <span data-ttu-id="e2be4-108">このチュートリアルが Microsoft を使用して ASP.NET MVC 4 Web アプリケーションの構築の基礎を講義[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)または Visual Web Developer 2010 Express Service Pack 1。</span><span class="sxs-lookup"><span data-stu-id="e2be4-108">This tutorial will teach you the basics of building an ASP.NET MVC 4 Web application using Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) or Visual Web Developer 2010 Express Service Pack 1.</span></span> <span data-ttu-id="e2be4-109">Visual Studio 2012 がお勧め、チュートリアルを完了する何もインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e2be4-109">Visual Studio 2012 is recommended, you won't need to install anything to complete the tutorial.</span></span> <span data-ttu-id="e2be4-110">Visual Studio 2010 を使用している場合は、以下のコンポーネントをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e2be4-110">If you are using Visual Studio 2010 you must install the components below.</span></span> <span data-ttu-id="e2be4-111">次のリンクをクリックして、それらのすべてをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="e2be4-111">You can install all of them by clicking the following links:</span></span>
>
> - [<span data-ttu-id="e2be4-112">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="e2be4-112">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="e2be4-113">ASP.NET MVC 4 の WPI インストーラー</span><span class="sxs-lookup"><span data-stu-id="e2be4-113">WPI installer for ASP.NET MVC 4</span></span>](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [<span data-ttu-id="e2be4-114">LocalDB</span><span class="sxs-lookup"><span data-stu-id="e2be4-114">LocalDB</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [<span data-ttu-id="e2be4-115">SSDT</span><span class="sxs-lookup"><span data-stu-id="e2be4-115">SSDT</span></span>](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
>
> <span data-ttu-id="e2be4-116">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、インストール、 [ASP.NET MVC 4 の WPI インストーラー](https://go.microsoft.com/fwlink/?LinkId=243392)と: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)</span><span class="sxs-lookup"><span data-stu-id="e2be4-116">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the [WPI installer for ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) and the: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)</span></span>
>
> <span data-ttu-id="e2be4-117">C# ソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="e2be4-117">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="e2be4-118">[C# バージョンをダウンロード](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip)します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-118">[Download the C# version](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).</span></span>
>
> <span data-ttu-id="e2be4-119">チュートリアルでは、Visual Studio でアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-119">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="e2be4-120">利用できるアプリケーション、インターネット経由でホスティング プロバイダーへのデプロイで。</span><span class="sxs-lookup"><span data-stu-id="e2be4-120">You can also make the application available over the Internet by deploying it to a hosting provider.</span></span> <span data-ttu-id="e2be4-121">マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、[無料試用版アカウントを Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-121">Microsoft offers free web hosting for up to 10 web sites in a [free Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="e2be4-122">Visual Studio web プロジェクトを Windows Azure の Web サイトにデプロイする方法については、次を参照してください。[作成し、ASP.NET web サイトと Visual Studio での SQL Database のデプロイ](https://docs.microsoft.com/dotnet/azure/)します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-122">For information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create and deploy an ASP.NET web site and SQL Database with Visual Studio](https://docs.microsoft.com/dotnet/azure/).</span></span> <span data-ttu-id="e2be4-123">そのチュートリアルでは、Entity Framework Code First Migrations を使用して Windows Azure SQL Database (旧 SQL Azure) に、SQL Server データベースをデプロイする方法も示します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-123">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Windows Azure SQL Database (formerly SQL Azure).</span></span>
>
> <span data-ttu-id="e2be4-124">このチュートリアルの執筆者は、Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="e2be4-124">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ).</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="e2be4-125">構築します</span><span class="sxs-lookup"><span data-stu-id="e2be4-125">What You'll Build</span></span>

> [!NOTE]
> <span data-ttu-id="e2be4-126">このチュートリアルでは、使用可能な場合は、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-126">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="e2be4-127">新しいチュートリアルでは、このチュートリアルで多くの機能強化を提供する ASP.NET MVC 5 を使用します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-127">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>


<span data-ttu-id="e2be4-128">作成、編集、検索、およびデータベースからムービーを一覧表示をサポートする単純なムービーの一覧をアプリケーションを実装します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-128">You'll implement a simple movie-listing application that supports creating, editing, searching and listing movies from a database.</span></span> <span data-ttu-id="e2be4-129">アプリケーションをビルドの 2 つのスクリーン ショットを次に示します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-129">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="e2be4-130">データベースからムービーの一覧を表示するページが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e2be4-130">It includes a page that displays a list of movies from a database:</span></span>

![](intro-to-aspnet-mvc-4/_static/image1.png)

<span data-ttu-id="e2be4-131">アプリケーションでは、追加、編集、および個別の詳細を参照するくださいと同様に、ムービーを削除することもできます。</span><span class="sxs-lookup"><span data-stu-id="e2be4-131">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="e2be4-132">すべてのデータ入力シナリオには、データベースに格納されたデータが正しいことを確認する検証が含まれます。</span><span class="sxs-lookup"><span data-stu-id="e2be4-132">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a><span data-ttu-id="e2be4-133">作業の開始</span><span class="sxs-lookup"><span data-stu-id="e2be4-133">Getting Started</span></span>

<span data-ttu-id="e2be4-134">Visual Studio Express 2012 または Visual Web Developer 2010 Express を実行して開始します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-134">Start by running Visual Studio Express 2012 or Visual Web Developer 2010 Express.</span></span> <span data-ttu-id="e2be4-135">このシリーズを Visual Studio Express 2012 がスクリーン ショットのほとんどは、Visual Studio 2010 SP1、Visual Studio 2012、Visual Studio Express 2012 または Visual Web Developer 2010 Express で、このチュートリアルを完了できます。</span><span class="sxs-lookup"><span data-stu-id="e2be4-135">Most of the screen shots in this series use Visual Studio Express 2012, but you can complete this tutorial with Visual Studio 2010/SP1, Visual Studio 2012, Visual Studio Express 2012 or Visual Web Developer 2010 Express.</span></span> <span data-ttu-id="e2be4-136">選択**新しいプロジェクト**から、**開始**ページ。</span><span class="sxs-lookup"><span data-stu-id="e2be4-136">Select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="e2be4-137">Visual Studio は、IDE、または統合開発環境です。</span><span class="sxs-lookup"><span data-stu-id="e2be4-137">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="e2be4-138">Microsoft Word を使用してドキュメントを記述するのにのと同じようにアプリケーションを作成、IDE を使用します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-138">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="e2be4-139">Visual Studio を使用できるさまざまなオプションを示す上部のツールバーがあります。</span><span class="sxs-lookup"><span data-stu-id="e2be4-139">In Visual Studio there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="e2be4-140">また、IDE でタスクを実行する別の方法を提供するメニューがあります。</span><span class="sxs-lookup"><span data-stu-id="e2be4-140">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="e2be4-141">(選択ではなく、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **の新しいプロジェクト**.)</span><span class="sxs-lookup"><span data-stu-id="e2be4-141">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="e2be4-142">最初のアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-142">Creating Your First Application</span></span>

<span data-ttu-id="e2be4-143">プログラミング言語として Visual Basic または Visual C# を使用してアプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="e2be4-143">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="e2be4-144">左側の Visual C# を選択し、 **ASP.NET MVC 4 Web アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-144">Select Visual C# on the left and then select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="e2be4-145">プロジェクトに名前を&quot;MvcMovie&quot; をクリックし、 **OK**します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-145">Name your project &quot;MvcMovie&quot; and then click **OK**.</span></span>

![](intro-to-aspnet-mvc-4/_static/image4.png)

<span data-ttu-id="e2be4-146">**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-146">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="e2be4-147">まま**Razor**既定のビュー エンジンとして。</span><span class="sxs-lookup"><span data-stu-id="e2be4-147">Leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-4/_static/image5.png)

<span data-ttu-id="e2be4-148">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e2be4-148">Click **OK**.</span></span> <span data-ttu-id="e2be4-149">Visual Studio は、何もせず、実用的なアプリケーションが現在あるため、作成した ASP.NET MVC プロジェクトの既定のテンプレートを使用!</span><span class="sxs-lookup"><span data-stu-id="e2be4-149">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="e2be4-150">これは、単純な&quot;Hello World!&quot;プロジェクト、およびそのアプリケーションを起動することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e2be4-150">This is a simple &quot;Hello World!&quot; project, and it's a good place to start your application.</span></span>

![](intro-to-aspnet-mvc-4/_static/image6.png)

<span data-ttu-id="e2be4-151">**[デバッグ]** メニューの **[デバッグの開始]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e2be4-151">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-4/_static/image7.png)

<span data-ttu-id="e2be4-152">デバッグを開始するキーボード ショートカットは、f5 キーであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-152">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="e2be4-153">F5 キーは、Visual Studio IIS Express を起動して、web アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-153">F5 causes Visual Studio to start IIS Express and run your web application.</span></span> <span data-ttu-id="e2be4-154">Visual Studio は、ブラウザーを起動し、アプリケーションのホーム ページを開きます。</span><span class="sxs-lookup"><span data-stu-id="e2be4-154">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="e2be4-155">ブラウザーのアドレス バーが表示されますが`localhost`ようなものではありません`example.com`します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-155">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="e2be4-156">だ`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-156">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="e2be4-157">Visual Studio web プロジェクトを実行すると、web サーバーのランダムなポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="e2be4-157">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e2be4-158">次の図では、ポート番号は、41788 です。</span><span class="sxs-lookup"><span data-stu-id="e2be4-158">In the image below, the port number is 41788.</span></span> <span data-ttu-id="e2be4-159">アプリケーションを実行するときに、別のポート番号をおそらく表示されます。</span><span class="sxs-lookup"><span data-stu-id="e2be4-159">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-4/_static/image8.png)

<span data-ttu-id="e2be4-160">すぐに使えるこの既定テンプレートを使用してホーム、連絡先についてのページ。</span><span class="sxs-lookup"><span data-stu-id="e2be4-160">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="e2be4-161">登録および、ログ記録のサポートを提供し、Facebook と Twitter へのリンクします。</span><span class="sxs-lookup"><span data-stu-id="e2be4-161">It also provides support to register and log in, and links to Facebook and Twitter.</span></span> <span data-ttu-id="e2be4-162">次の手順では、このアプリケーションの動作を変更し、ASP.NET MVC についてもう少し説明します。</span><span class="sxs-lookup"><span data-stu-id="e2be4-162">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="e2be4-163">ブラウザーを閉じて、いくつかのコードを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="e2be4-163">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e2be4-164">次へ</span><span class="sxs-lookup"><span data-stu-id="e2be4-164">Next</span></span>](adding-a-controller.md)
