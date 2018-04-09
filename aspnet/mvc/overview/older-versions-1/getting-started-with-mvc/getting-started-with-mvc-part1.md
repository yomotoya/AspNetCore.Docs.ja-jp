---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: ASP.NET mvc |Microsoft ドキュメント
author: shanselman
description: これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みする単純な web アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="2f336-104">ASP.NET mvc</span><span class="sxs-lookup"><span data-stu-id="2f336-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="2f336-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="2f336-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="2f336-106">このチュートリアルでは使用可能な場合、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)です。</span><span class="sxs-lookup"><span data-stu-id="2f336-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="2f336-107">新しいチュートリアルでは、ASP.NET MVC 5 は、このチュートリアルを超える多くの機能強化を提供を使用します。</span><span class="sxs-lookup"><span data-stu-id="2f336-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="2f336-108">これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="2f336-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="2f336-109">データベースから読み取り/書き込みを単純な web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="2f336-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="2f336-110">参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。</span><span class="sxs-lookup"><span data-stu-id="2f336-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="2f336-111">ASP.NET MVC Web アプリケーションを使用して最初を加えてみましょう[Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/)です。</span><span class="sxs-lookup"><span data-stu-id="2f336-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="2f336-112">少しのムービー リスト アプリケーションをお知らせを作成し、映画を一覧表示するしましょう。</span><span class="sxs-lookup"><span data-stu-id="2f336-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="2f336-113">新機能のビルドします。</span><span class="sxs-lookup"><span data-stu-id="2f336-113">What You'll Build</span></span>

<span data-ttu-id="2f336-114">アプリケーションのビルドの 2 つのスクリーン ショットを次に示します。</span><span class="sxs-lookup"><span data-stu-id="2f336-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="2f336-115">さまざまな列を持つムービーの単純なテーブルがあります。</span><span class="sxs-lookup"><span data-stu-id="2f336-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="2f336-116">[![ムービーの一覧]-[Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2f336-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="2f336-117">ムービーの一覧に追加できるように、フォームの作成が必要があります。</span><span class="sxs-lookup"><span data-stu-id="2f336-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="2f336-118">[![-ムービーを作成するには、Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="2f336-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="2f336-119">スキルの学習</span><span class="sxs-lookup"><span data-stu-id="2f336-119">Skills You'll Learn</span></span>

<span data-ttu-id="2f336-120">このチュートリアルでは、Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="2f336-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="2f336-121">について説明します。</span><span class="sxs-lookup"><span data-stu-id="2f336-121">You'll learn:</span></span>

- <span data-ttu-id="2f336-122">新しい ASP.NET MVC プロジェクトを作成する方法</span><span class="sxs-lookup"><span data-stu-id="2f336-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="2f336-123">SQL Server の新しいデータベースを作成する方法</span><span class="sxs-lookup"><span data-stu-id="2f336-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="2f336-124">ASP.NET MVC のコント ローラーとビューを作成する方法</span><span class="sxs-lookup"><span data-stu-id="2f336-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="2f336-125">データ取得して表示する方法</span><span class="sxs-lookup"><span data-stu-id="2f336-125">How to retrieve and display data</span></span>
- <span data-ttu-id="2f336-126">データを編集し、データの検証を有効にする方法</span><span class="sxs-lookup"><span data-stu-id="2f336-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="2f336-127">データベース スキーマを更新する方法</span><span class="sxs-lookup"><span data-stu-id="2f336-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="2f336-128">開始するには</span><span class="sxs-lookup"><span data-stu-id="2f336-128">Get Started</span></span>

<span data-ttu-id="2f336-129">Visual Web Developer 2010 Express (名前を付けますが"VWD"今後) と新しいプロジェクトを選択を実行して、スタート画面から、開始します。</span><span class="sxs-lookup"><span data-stu-id="2f336-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="2f336-130">Visual Web Developer は、IDE、または開発者の統合環境です。</span><span class="sxs-lookup"><span data-stu-id="2f336-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="2f336-131">Microsoft Word を使用してドキュメントを作成するのにのと同じようにアプリケーションを作成するのに、IDE を使用します。</span><span class="sxs-lookup"><span data-stu-id="2f336-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="2f336-132">だけでなく、ファイルの選択を使用してもでしたメニューを使用できるさまざまなオプションを示す上部にあるツールバー |新規のプロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="2f336-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="2f336-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="2f336-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="2f336-134">初めてアプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="2f336-134">Creating Your First Application</span></span>

<span data-ttu-id="2f336-135">Visual Basic または Visual c# を使用してアプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="2f336-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="2f336-136">ここでは、選択 Visual C# の場合、左側の「ASP.NET MVC 2 Web アプリケーション」を選択し、</span><span class="sxs-lookup"><span data-stu-id="2f336-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="2f336-137">「映画」をプロジェクトに名前し、[ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2f336-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="2f336-138">[![新しいプロジェクト](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="2f336-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="2f336-139">右側にあるすべてのファイルとフォルダーを表示、アプリケーションで、ソリューション エクスプ ローラーです。</span><span class="sxs-lookup"><span data-stu-id="2f336-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="2f336-140">大規模なウィンドウの中間には、コードを編集し、ほとんどの時間を費やす場所です。</span><span class="sxs-lookup"><span data-stu-id="2f336-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="2f336-141">Visual Studio では、作成した ASP.NET MVC プロジェクトの既定のテンプレートが使用されるため、何もせず作業アプリケーションが現在ある!</span><span class="sxs-lookup"><span data-stu-id="2f336-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="2f336-142">これは、単純な"Hello World!</span><span class="sxs-lookup"><span data-stu-id="2f336-142">This is a simple "Hello World!</span></span> <span data-ttu-id="2f336-143">プロジェクト、これは、アプリケーションを起動するに適してします。</span><span class="sxs-lookup"><span data-stu-id="2f336-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="2f336-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="2f336-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="2f336-145">ツールバーに「再生」ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="2f336-145">Select the "play" button to the toolbar.</span></span>

![デバッグの開始](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="2f336-147">緑色の矢印は、プログラムをコンパイルし、web ブラウザーで、アプリケーションを起動する右 をポイントすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2f336-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="2f336-148">*注: する代わりに、キーボードの f5 キーを押すか、選択デバッグ-&gt;[デバッグ] メニューからデバッグを開始します。*</span><span class="sxs-lookup"><span data-stu-id="2f336-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="2f336-149">開発 web サーバーを起動し、(はありません構成や手動手順がこれを有効にするために必要)、web アプリケーションを実行する Visual Web Developer は、なります。</span><span class="sxs-lookup"><span data-stu-id="2f336-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="2f336-150">ブラウザーを起動し、アプリケーションのホーム ページを参照するように構成されます。</span><span class="sxs-lookup"><span data-stu-id="2f336-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="2f336-151">下"localhost"、「example.com」のようなものでありブラウザーのアドレス バーに表示されるに注意してください。-ここでは、作成したアプリケーションを実行している自分のローカル コンピューターを常に localhost がポイントするためです。</span><span class="sxs-lookup"><span data-stu-id="2f336-151">Notice below that the address bar of the browser says "localhost", and not something like example.com. That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="2f336-152">[![ホーム ページ](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="2f336-152">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="2f336-153">すぐは、この既定のテンプレートは、する 2 つのページにアクセスし、基本的なログイン ページを示します。</span><span class="sxs-lookup"><span data-stu-id="2f336-153">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="2f336-154">このアプリケーションの動作を変更して、プロセスで ASP.NET MVC について少し説明しましょう。</span><span class="sxs-lookup"><span data-stu-id="2f336-154">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="2f336-155">ブラウザーを閉じて、いくつかのコードを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="2f336-155">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2f336-156">次へ</span><span class="sxs-lookup"><span data-stu-id="2f336-156">Next</span></span>](getting-started-with-mvc-part2.md)
