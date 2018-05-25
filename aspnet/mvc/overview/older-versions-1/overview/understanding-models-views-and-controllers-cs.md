---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Understanding モデル、ビュー、およびコント ローラー (c#) |Microsoft ドキュメント
author: StephenWalther
description: モデル、ビュー、およびコント ローラーに関する混乱しますか。 このチュートリアルでは Stephen Walther について説明しています、ASP.NET MVC アプリケーションのさまざまな部分です。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c9a0cbbf6f786944d7892fbb14859939f21bdd5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="understanding-models-views-and-controllers-c"></a><span data-ttu-id="b0867-104">Understanding モデル、ビュー、およびコント ローラー (c#)</span><span class="sxs-lookup"><span data-stu-id="b0867-104">Understanding Models, Views, and Controllers (C#)</span></span>
====================
<span data-ttu-id="b0867-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="b0867-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="b0867-106">モデル、ビュー、およびコント ローラーに関する混乱しますか。</span><span class="sxs-lookup"><span data-stu-id="b0867-106">Confused about Models, Views, and Controllers?</span></span> <span data-ttu-id="b0867-107">このチュートリアルでは Stephen Walther について説明しています、ASP.NET MVC アプリケーションのさまざまな部分です。</span><span class="sxs-lookup"><span data-stu-id="b0867-107">In this tutorial, Stephen Walther introduces you to the different parts of an ASP.NET MVC application.</span></span>


<span data-ttu-id="b0867-108">このチュートリアルは、ASP.NET MVC の概要を示しますモデル、ビュー、およびコント ローラーです。</span><span class="sxs-lookup"><span data-stu-id="b0867-108">This tutorial provides you with a high-level overview of ASP.NET MVC models, views, and controllers.</span></span> <span data-ttu-id="b0867-109">つまり、M について説明します '、V'、および C' ASP.NET MVC でします。</span><span class="sxs-lookup"><span data-stu-id="b0867-109">In other words, it explains the M', V', and C' in ASP.NET MVC.</span></span>

<span data-ttu-id="b0867-110">このチュートリアルを読むと、後に、ASP.NET MVC アプリケーションのさまざまな部分がどのように連携を理解する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0867-110">After reading this tutorial, you should understand how the different parts of an ASP.NET MVC application work together.</span></span> <span data-ttu-id="b0867-111">ASP.NET MVC アプリケーションのアーキテクチャの違い、ASP.NET Web フォーム アプリケーションまたは Active Server Pages アプリケーションも理解する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0867-111">You should also understand how the architecture of an ASP.NET MVC application differs from an ASP.NET Web Forms application or Active Server Pages application.</span></span>

## <a name="the-sample-aspnet-mvc-application"></a><span data-ttu-id="b0867-112">サンプルの ASP.NET MVC アプリケーション</span><span class="sxs-lookup"><span data-stu-id="b0867-112">The Sample ASP.NET MVC Application</span></span>

<span data-ttu-id="b0867-113">ASP.NET MVC Web アプリケーションを作成するための既定の Visual Studio テンプレートには、ASP.NET MVC アプリケーションのさまざまな部分を理解するために使用する非常に簡単なサンプル アプリケーションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b0867-113">The default Visual Studio template for creating ASP.NET MVC Web Applications includes an extremely simple sample application that can be used to understand the different parts of an ASP.NET MVC application.</span></span> <span data-ttu-id="b0867-114">このチュートリアルではこの単純なアプリケーションのことを利用します。</span><span class="sxs-lookup"><span data-stu-id="b0867-114">We take advantage of this simple application in this tutorial.</span></span>

<span data-ttu-id="b0867-115">MVC テンプレートを使用して Visual Studio 2008 を起動して、新しい ASP.NET MVC アプリケーションを作成して、新規プロジェクト ファイル メニュー オプションを選択すると、(図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="b0867-115">You create a new ASP.NET MVC application with the MVC template by launching Visual Studio 2008 and selecting the menu option File, New Project (see Figure 1).</span></span> <span data-ttu-id="b0867-116">新しいプロジェクト] ダイアログで [プロジェクトの種類 (Visual Basic または C# の場合)、好みのプログラミング言語を選択し、[ **ASP.NET MVC Web アプリケーション**テンプレート] の下。</span><span class="sxs-lookup"><span data-stu-id="b0867-116">In the New Project dialog, select your favorite programming language under Project Types (Visual Basic or C#) and select **ASP.NET MVC Web Application** under Templates.</span></span> <span data-ttu-id="b0867-117">[Ok] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b0867-117">Click the OK button.</span></span>


<span data-ttu-id="b0867-118">[![新しいプロジェクト ダイアログ ボックス](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b0867-118">[![New Project Dialog](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)</span></span>

<span data-ttu-id="b0867-119">**図 01**: 新しいプロジェクト ダイアログ ボックス ([フルサイズのイメージを表示するをクリックして](understanding-models-views-and-controllers-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b0867-119">**Figure 01**: New Project Dialog ([Click to view full-size image](understanding-models-views-and-controllers-cs/_static/image2.png))</span></span>


<span data-ttu-id="b0867-120">新しい ASP.NET MVC アプリケーションを作成するときに、**単体テスト プロジェクトの作成**ダイアログが表示されます (図 2)。</span><span class="sxs-lookup"><span data-stu-id="b0867-120">When you create a new ASP.NET MVC application, the **Create Unit Test Project** dialog appears (see Figure 2).</span></span> <span data-ttu-id="b0867-121">このダイアログ ボックスでは、ASP.NET MVC アプリケーションのテスト用のソリューションで別のプロジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="b0867-121">This dialog enables you to create a separate project in your solution for testing your ASP.NET MVC application.</span></span> <span data-ttu-id="b0867-122">オプションを選択**単体テスト プロジェクトを作成できません** をクリックし、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b0867-122">Select the option **No, do not create a unit test project** and click the **OK** button.</span></span>


<span data-ttu-id="b0867-123">[![単体テスト ダイアログ ボックスを作成します。](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b0867-123">[![Create Unit Test Dialog](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)</span></span>

<span data-ttu-id="b0867-124">**図 02**: 単体テストの作成 ダイアログ ボックス ([フルサイズのイメージを表示するをクリックして](understanding-models-views-and-controllers-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="b0867-124">**Figure 02**: Create Unit Test Dialog ([Click to view full-size image](understanding-models-views-and-controllers-cs/_static/image4.png))</span></span>


<span data-ttu-id="b0867-125">新しい ASP.NET MVC アプリケーションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-125">After the new ASP.NET MVC application is created.</span></span> <span data-ttu-id="b0867-126">いくつかのフォルダーとファイル、ソリューション エクスプ ローラー ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-126">You will see several folders and files in the Solution Explorer window.</span></span> <span data-ttu-id="b0867-127">具体的には、モデル、ビュー、およびコント ローラーという 3 つのフォルダーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-127">In particular, you'll see three folders named Models, Views, and Controllers.</span></span> <span data-ttu-id="b0867-128">フォルダー名から推測することがあります、これらのフォルダーは、モデル、ビュー、およびコント ローラーを実装するためのファイルを格納します。</span><span class="sxs-lookup"><span data-stu-id="b0867-128">As you might guess from the folder names, these folders contain the files for implementing models, views, and controllers.</span></span>

<span data-ttu-id="b0867-129">Controllers フォルダーを展開する場合は、AccountController.cs をという名前のファイルおよび HomeController.cs をという名前のファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-129">If you expand the Controllers folder, you should see a file named AccountController.cs and a file named HomeController.cs.</span></span> <span data-ttu-id="b0867-130">Views フォルダーを展開する場合は、アカウント、ホームおよび Shared という名前の 3 つのサブフォルダーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-130">If you expand the Views folder, you should see three subfolders named Account, Home and Shared.</span></span> <span data-ttu-id="b0867-131">ホーム フォルダーを展開する場合は、About.aspx および Index.aspx (図 3 を参照してください) という 2 つの追加のファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-131">If you expand the Home folder, you'll see two additional files named About.aspx and Index.aspx (see Figure 3).</span></span> <span data-ttu-id="b0867-132">これらのファイルは、既定の ASP.NET MVC テンプレートに含まれているサンプル アプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="b0867-132">These files make up the sample application included with the default ASP.NET MVC template.</span></span>


<span data-ttu-id="b0867-133">[![ソリューション エクスプ ローラー ウィンドウ](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="b0867-133">[![The Solution Explorer Window](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)</span></span>

<span data-ttu-id="b0867-134">**図 03**:、ソリューション エクスプ ローラー ウィンドウ ([フルサイズのイメージを表示するをクリックして](understanding-models-views-and-controllers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b0867-134">**Figure 03**: The Solution Explorer Window ([Click to view full-size image](understanding-models-views-and-controllers-cs/_static/image6.png))</span></span>


<span data-ttu-id="b0867-135">メニュー オプションを選択して、サンプル アプリケーションを実行することができます**デバッグ、デバッグの開始**です。</span><span class="sxs-lookup"><span data-stu-id="b0867-135">You can run the sample application by selecting the menu option **Debug, Start Debugging**.</span></span> <span data-ttu-id="b0867-136">または、F5 キーを押してことができます。</span><span class="sxs-lookup"><span data-stu-id="b0867-136">Alternatively, you can press the F5 key.</span></span>

<span data-ttu-id="b0867-137">最初に、ASP.NET アプリケーションを実行すると、デバッグ モードを有効にすることをお勧めの図 4 では、ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-137">When you first run an ASP.NET application, the dialog in Figure 4 appears that recommends that you enable debug mode.</span></span> <span data-ttu-id="b0867-138">[Ok] ボタンをクリックし、そのアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="b0867-138">Click the OK button and the application will run.</span></span>


<span data-ttu-id="b0867-139">[![デバッグの有効になっていません ダイアログ](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="b0867-139">[![Debugging Not Enabled dialog](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)</span></span>

<span data-ttu-id="b0867-140">**図 04**: ダイアログのデバッグが無効 ([フルサイズのイメージを表示するをクリックして](understanding-models-views-and-controllers-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="b0867-140">**Figure 04**: Debugging Not Enabled dialog ([Click to view full-size image](understanding-models-views-and-controllers-cs/_static/image8.png))</span></span>


<span data-ttu-id="b0867-141">ASP.NET MVC アプリケーションを実行すると、Visual Studio は、web ブラウザーでアプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="b0867-141">When you run an ASP.NET MVC application, Visual Studio launches the application in your web browser.</span></span> <span data-ttu-id="b0867-142">サンプル アプリケーションは、2 つのページで構成されます: インデックス ページと [バージョン情報] ページ。</span><span class="sxs-lookup"><span data-stu-id="b0867-142">The sample application consists of only two pages: the Index page and the About page.</span></span> <span data-ttu-id="b0867-143">アプリケーションの起動時、(図 5 を参照してください)、インデックス ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-143">When the application first starts, the Index page appears (see Figure 5).</span></span> <span data-ttu-id="b0867-144">上部のメニューのリンクをクリックして、[バージョン情報] ページに移動することができます、アプリケーションの権限です。</span><span class="sxs-lookup"><span data-stu-id="b0867-144">You can navigate to the About page by clicking the menu link at the top right of the application.</span></span>


<span data-ttu-id="b0867-145">[![[インデックス] ページ](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="b0867-145">[![The Index Page](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)</span></span>

<span data-ttu-id="b0867-146">**図 05**:、インデックス ページ ([フルサイズのイメージを表示するをクリックして](understanding-models-views-and-controllers-cs/_static/image11.png))</span><span class="sxs-lookup"><span data-stu-id="b0867-146">**Figure 05**: The Index Page ([Click to view full-size image](understanding-models-views-and-controllers-cs/_static/image11.png))</span></span>


<span data-ttu-id="b0867-147">お使いのブラウザーのアドレス バーに Url に注意してください。</span><span class="sxs-lookup"><span data-stu-id="b0867-147">Notice the URLs in the address bar of your browser.</span></span> <span data-ttu-id="b0867-148">たとえば、バージョン情報] メニューの [リンクをクリックするとブラウザーのアドレス バーに URL を変更**ホーム//について**です。</span><span class="sxs-lookup"><span data-stu-id="b0867-148">For example, when you click the About menu link, the URL in the browser address bar changes to **/Home/About**.</span></span>

<span data-ttu-id="b0867-149">ブラウザー ウィンドウを閉じるし、Visual Studio に戻り、パス ホーム約/ファイルを検索することはできません。</span><span class="sxs-lookup"><span data-stu-id="b0867-149">If you close the browser window and return to Visual Studio, you won't be able to find a file with the path Home/About.</span></span> <span data-ttu-id="b0867-150">ファイルが存在しません。</span><span class="sxs-lookup"><span data-stu-id="b0867-150">The files don't exist.</span></span> <span data-ttu-id="b0867-151">次のとおりですか。</span><span class="sxs-lookup"><span data-stu-id="b0867-151">How is this possible?</span></span>

## <a name="a-url-does-not-equal-a-page"></a><span data-ttu-id="b0867-152">URL がページに等しくないです。</span><span class="sxs-lookup"><span data-stu-id="b0867-152">A URL Does Not Equal a Page</span></span>

<span data-ttu-id="b0867-153">従来の ASP.NET Web フォーム アプリケーションまたは Active Server Pages アプリケーションをビルドするときに、URL とページ間の一対一の対応。</span><span class="sxs-lookup"><span data-stu-id="b0867-153">When you build a traditional ASP.NET Web Forms application or an Active Server Pages application, there is a one-to-one correspondence between a URL and a page.</span></span> <span data-ttu-id="b0867-154">サーバーから SomePage.aspx をという名前のページを要求する場合がよりありますページ SomePage.aspx をという名前のディスク上。</span><span class="sxs-lookup"><span data-stu-id="b0867-154">If you request a page named SomePage.aspx from the server, then there had better be a page on disk named SomePage.aspx.</span></span> <span data-ttu-id="b0867-155">見た目を取得する SomePage.aspx ファイルが存在しない場合**ページが見つかりません - 404**エラーです。</span><span class="sxs-lookup"><span data-stu-id="b0867-155">If the SomePage.aspx file does not exist, you get an ugly **404 - Page Not Found** error.</span></span>

<span data-ttu-id="b0867-156">ASP.NET MVC アプリケーションを構築するときにこれに対しはありません、ブラウザーのアドレス バーに入力した URL と、アプリケーション内にあるファイルの対応。</span><span class="sxs-lookup"><span data-stu-id="b0867-156">When building an ASP.NET MVC application, in contrast, there is no correspondence between the URL that you type into your browser's address bar and the files that you find in your application.</span></span> <span data-ttu-id="b0867-157">ASP.NET MVC アプリケーションでは、URL は、ディスク上のページではなくコント ローラーのアクションに対応します。</span><span class="sxs-lookup"><span data-stu-id="b0867-157">In an ASP.NET MVC application, a URL corresponds to a controller action instead of a page on disk.</span></span>

<span data-ttu-id="b0867-158">従来の ASP.NET または ASP アプリケーションでは、ブラウザーの要求がページにマップされます。</span><span class="sxs-lookup"><span data-stu-id="b0867-158">In a traditional ASP.NET or ASP application, browser requests are mapped to pages.</span></span> <span data-ttu-id="b0867-159">ASP.NET MVC アプリケーションでこれに対し、ブラウザーの要求にマップされますコント ローラーのアクション。</span><span class="sxs-lookup"><span data-stu-id="b0867-159">In an ASP.NET MVC application, in contrast, browser requests are mapped to controller actions.</span></span> <span data-ttu-id="b0867-160">ASP.NET Web フォーム アプリケーションは、コンテンツを中心としました。</span><span class="sxs-lookup"><span data-stu-id="b0867-160">An ASP.NET Web Forms application is content-centric.</span></span> <span data-ttu-id="b0867-161">これに対し、ASP.NET MVC アプリケーションは、中心のアプリケーション ロジックです。</span><span class="sxs-lookup"><span data-stu-id="b0867-161">An ASP.NET MVC application, in contrast, is application logic centric.</span></span>

## <a name="understanding-aspnet-routing"></a><span data-ttu-id="b0867-162">ASP.NET ルーティング</span><span class="sxs-lookup"><span data-stu-id="b0867-162">Understanding ASP.NET Routing</span></span>

<span data-ttu-id="b0867-163">ブラウザー要求を取得という ASP.NET フレームワークの機能を通じて、コント ローラー アクションにマップされている*ASP.NET ルーティング*です。</span><span class="sxs-lookup"><span data-stu-id="b0867-163">A browser request gets mapped to a controller action through a feature of the ASP.NET framework called *ASP.NET Routing*.</span></span> <span data-ttu-id="b0867-164">ASP.NET ルーティングが、ASP.NET MVC フレームワークによって使用される*ルート*コント ローラー アクションへの着信要求します。</span><span class="sxs-lookup"><span data-stu-id="b0867-164">ASP.NET Routing is used by the ASP.NET MVC framework to *route* incoming requests to controller actions.</span></span>

<span data-ttu-id="b0867-165">ASP.NET ルーティングでは、ルート テーブルを使用して、受信要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="b0867-165">ASP.NET Routing uses a route table to handle incoming requests.</span></span> <span data-ttu-id="b0867-166">Web アプリケーションが初めて起動したときに、このルート テーブルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-166">This route table is created when your web application first starts.</span></span> <span data-ttu-id="b0867-167">ルート テーブルは、Global.asax ファイルで設定します。</span><span class="sxs-lookup"><span data-stu-id="b0867-167">The route table is setup in the Global.asax file.</span></span> <span data-ttu-id="b0867-168">既定の MVC Global.asax ファイルが 1 のリストに含まれます。</span><span class="sxs-lookup"><span data-stu-id="b0867-168">The default MVC Global.asax file is contained in Listing 1.</span></span>

<span data-ttu-id="b0867-169">**1 - Global.asax を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="b0867-169">**Listing 1 - Global.asax**</span></span>

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

<span data-ttu-id="b0867-170">ASP.NET アプリケーション最初の開始時、アプリケーション\_Start() メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-170">When an ASP.NET application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="b0867-171">リストの 1 では、このメソッドは、RegisterRoutes() し RegisterRoutes() メソッドは、既定のルート テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="b0867-171">In Listing 1, this method calls the RegisterRoutes() method and the RegisterRoutes() method creates the default route table.</span></span>

<span data-ttu-id="b0867-172">既定のルート テーブルは、1 つのルートで構成されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-172">The default route table consists of one route.</span></span> <span data-ttu-id="b0867-173">この既定のルートでは、すべての着信要求を 3 つのセグメント (URL セグメントとは何もスラッシュの間) に分割します。</span><span class="sxs-lookup"><span data-stu-id="b0867-173">This default route breaks all incoming requests into three segments (a URL segment is anything between forward slashes).</span></span> <span data-ttu-id="b0867-174">最初のセグメントは、コント ローラー名にマップされて、2 番目のセグメントは、アクション名にマップ、および最後のセグメントが id。 をという名前のアクションに渡されるパラメーターにマップされています。</span><span class="sxs-lookup"><span data-stu-id="b0867-174">The first segment is mapped to a controller name, the second segment is mapped to an action name, and the final segment is mapped to a parameter passed to the action named Id.</span></span>

<span data-ttu-id="b0867-175">たとえば、次の URL を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="b0867-175">For example, consider the following URL:</span></span>

<span data-ttu-id="b0867-176">/製品/詳細/3</span><span class="sxs-lookup"><span data-stu-id="b0867-176">/Product/Details/3</span></span>

<span data-ttu-id="b0867-177">次のように 3 つのパラメーターには、この URL が解析されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-177">This URL is parsed into three parameters like this:</span></span>

<span data-ttu-id="b0867-178">コント ローラー製品を =</span><span class="sxs-lookup"><span data-stu-id="b0867-178">Controller = Product</span></span>

<span data-ttu-id="b0867-179">アクションの詳細を =</span><span class="sxs-lookup"><span data-stu-id="b0867-179">Action = Details</span></span>

<span data-ttu-id="b0867-180">Id = 3</span><span class="sxs-lookup"><span data-stu-id="b0867-180">Id = 3</span></span>

<span data-ttu-id="b0867-181">Global.asax ファイルで定義されている既定のルートには、次の 3 つのすべてのパラメーターの既定値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b0867-181">The Default route defined in the Global.asax file includes default values for all three parameters.</span></span> <span data-ttu-id="b0867-182">既定値コント ローラーはホーム、既定のアクションでは、インデックス、および既定の Id は空の文字列。</span><span class="sxs-lookup"><span data-stu-id="b0867-182">The default Controller is Home, the default Action is Index, and the default Id is an empty string.</span></span> <span data-ttu-id="b0867-183">これらの既定値を念頭には、次の URL を解析する方法について考えてみます。</span><span class="sxs-lookup"><span data-stu-id="b0867-183">With these defaults in mind, consider how the following URL is parsed:</span></span>

<span data-ttu-id="b0867-184">/従業員</span><span class="sxs-lookup"><span data-stu-id="b0867-184">/Employee</span></span>

<span data-ttu-id="b0867-185">次のように 3 つのパラメーターには、この URL が解析されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-185">This URL is parsed into three parameters like this:</span></span>

<span data-ttu-id="b0867-186">コント ローラーの従業員を =</span><span class="sxs-lookup"><span data-stu-id="b0867-186">Controller = Employee</span></span>

<span data-ttu-id="b0867-187">アクション インデックスを =</span><span class="sxs-lookup"><span data-stu-id="b0867-187">Action = Index</span></span>

<span data-ttu-id="b0867-188">Id =</span><span class="sxs-lookup"><span data-stu-id="b0867-188">Id = ��</span></span>

<span data-ttu-id="b0867-189">最後に、任意の URL を指定せずに ASP.NET MVC アプリケーションを開くかどうか (たとえば、 `http://localhost`) 次のように URL が解析結果。</span><span class="sxs-lookup"><span data-stu-id="b0867-189">Finally, if you open an ASP.NET MVC Application without supplying any URL (for example, `http://localhost`) then the URL is parsed like this:</span></span>

<span data-ttu-id="b0867-190">コント ローラー ホーム =</span><span class="sxs-lookup"><span data-stu-id="b0867-190">Controller = Home</span></span>

<span data-ttu-id="b0867-191">アクション インデックスを =</span><span class="sxs-lookup"><span data-stu-id="b0867-191">Action = Index</span></span>

<span data-ttu-id="b0867-192">Id =</span><span class="sxs-lookup"><span data-stu-id="b0867-192">Id = ��</span></span>

<span data-ttu-id="b0867-193">HomeController クラスに対する Index() アクションに、要求がルーティングされます。</span><span class="sxs-lookup"><span data-stu-id="b0867-193">The request is routed to the Index() action on the HomeController class.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="b0867-194">コント ローラーの説明</span><span class="sxs-lookup"><span data-stu-id="b0867-194">Understanding Controllers</span></span>

<span data-ttu-id="b0867-195">コント ローラーは、ユーザーが、MVC アプリケーションと対話する方法を制御します。</span><span class="sxs-lookup"><span data-stu-id="b0867-195">A controller is responsible for controlling the way that a user interacts with an MVC application.</span></span> <span data-ttu-id="b0867-196">コント ローラーには、ASP.NET MVC アプリケーションのフロー制御ロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b0867-196">A controller contains the flow control logic for an ASP.NET MVC application.</span></span> <span data-ttu-id="b0867-197">コント ローラーでは、ユーザーがブラウザーの要求を行うときに、ユーザーに返送するには、どのような応答を決定します。</span><span class="sxs-lookup"><span data-stu-id="b0867-197">A controller determines what response to send back to a user when a user makes a browser request.</span></span>

<span data-ttu-id="b0867-198">コント ローラーは、クラス (たとえば、Visual Basic または c# クラス) だけです。</span><span class="sxs-lookup"><span data-stu-id="b0867-198">A controller is just a class (for example, a Visual Basic or C# class).</span></span> <span data-ttu-id="b0867-199">ASP.NET MVC アプリケーションの例には、コント ローラーのフォルダーにある HomeController.cs をという名前のコント ローラーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b0867-199">The sample ASP.NET MVC application includes a controller named HomeController.cs located in the Controllers folder.</span></span> <span data-ttu-id="b0867-200">HomeController.cs ファイルの内容は、リスト 2 で再現します。</span><span class="sxs-lookup"><span data-stu-id="b0867-200">The content of the HomeController.cs file is reproduced in Listing 2.</span></span>

<span data-ttu-id="b0867-201">**2 - HomeController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="b0867-201">**Listing 2 - HomeController.cs**</span></span>

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

<span data-ttu-id="b0867-202">HomeController が Index() および About() という名前の 2 つのメソッドを持つことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b0867-202">Notice that the HomeController has two methods named Index() and About().</span></span> <span data-ttu-id="b0867-203">これら 2 つのメソッドは、コント ローラーによって公開されている 2 つのアクションに対応します。</span><span class="sxs-lookup"><span data-stu-id="b0867-203">These two methods correspond to the two actions exposed by the controller.</span></span> <span data-ttu-id="b0867-204">URL/Home/インデックス、HomeController.Index() メソッドを呼び出しに関する URL/Home/HomeController.About() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b0867-204">The URL /Home/Index invokes the HomeController.Index() method and the URL /Home/About invokes the HomeController.About() method.</span></span>

<span data-ttu-id="b0867-205">コント ローラー内のすべてのパブリック メソッドは、コント ローラーのアクションとして公開されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-205">Any public method in a controller is exposed as a controller action.</span></span> <span data-ttu-id="b0867-206">これについて注意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0867-206">You need to be careful about this.</span></span> <span data-ttu-id="b0867-207">これは、ブラウザーに正しい URL を入力して、インターネットにアクセス権を持つすべてのユーザーが、コント ローラーに含まれているすべてのパブリック メソッドを起動できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="b0867-207">This means that any public method contained in a controller can be invoked by anyone with access to the Internet by entering the right URL into a browser.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="b0867-208">ビューについて</span><span class="sxs-lookup"><span data-stu-id="b0867-208">Understanding Views</span></span>

<span data-ttu-id="b0867-209">HomeController クラス、Index() と About()、によって公開されている 2 つのコント ローラーのアクション、ビューはどちらも返します。</span><span class="sxs-lookup"><span data-stu-id="b0867-209">The two controller actions exposed by the HomeController class, Index() and About(), both return a view.</span></span> <span data-ttu-id="b0867-210">ビューには、HTML マークアップと、ブラウザーに送信されるコンテンツが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b0867-210">A view contains the HTML markup and content that is sent to the browser.</span></span> <span data-ttu-id="b0867-211">ビューは、ASP.NET MVC アプリケーションを使用する場合、ページの相当します。</span><span class="sxs-lookup"><span data-stu-id="b0867-211">A view is the equivalent of a page when working with an ASP.NET MVC application.</span></span>

<span data-ttu-id="b0867-212">適切な場所で、ビューを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0867-212">You must create your views in the right location.</span></span> <span data-ttu-id="b0867-213">HomeController.Index() アクションには、次のパスにあるビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-213">The HomeController.Index() action returns a view located at the following path:</span></span>

<span data-ttu-id="b0867-214">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="b0867-214">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="b0867-215">HomeController.About() アクションには、次のパスにあるビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-215">The HomeController.About() action returns a view located at the following path:</span></span>

<span data-ttu-id="b0867-216">\Views\Home\About.aspx</span><span class="sxs-lookup"><span data-stu-id="b0867-216">\Views\Home\About.aspx</span></span>

<span data-ttu-id="b0867-217">一般に、コント ローラーのアクションのビューを取得するには、場合、コント ローラーと同じ名前のビュー フォルダーにサブフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="b0867-217">In general, if you want to return a view for a controller action, then you need to create a subfolder in the Views folder with the same name as your controller.</span></span> <span data-ttu-id="b0867-218">サブフォルダー内には、コント ローラーのアクションと同じ名前の .aspx ファイルを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0867-218">Within the subfolder, you must create an .aspx file with the same name as the controller action.</span></span>

<span data-ttu-id="b0867-219">3 の一覧で、ファイルには、About.aspx ビューが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b0867-219">The file in Listing 3 contains the About.aspx view.</span></span>

<span data-ttu-id="b0867-220">**3 - About.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="b0867-220">**Listing 3 - About.aspx**</span></span>

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

<span data-ttu-id="b0867-221">リスト 3 の最初の行を無視する場合、ビューの残りの部分のほとんどは標準の HTML で構成されます。</span><span class="sxs-lookup"><span data-stu-id="b0867-221">If you ignore the first line in Listing 3, most of the rest of the view consists of standard HTML.</span></span> <span data-ttu-id="b0867-222">ここにする任意の HTML を入力して、ビューの内容を変更できます。</span><span class="sxs-lookup"><span data-stu-id="b0867-222">You can modify the contents of the view by entering any HTML that you want here.</span></span>

<span data-ttu-id="b0867-223">ビューは、Active Server Pages または ASP.NET Web フォームでのページによく似ています。</span><span class="sxs-lookup"><span data-stu-id="b0867-223">A view is very similar to a page in Active Server Pages or ASP.NET Web Forms.</span></span> <span data-ttu-id="b0867-224">ビューには、HTML コンテンツおよびスクリプトを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="b0867-224">A view can contain HTML content and scripts.</span></span> <span data-ttu-id="b0867-225">スクリプトは、プログラミング言語 (たとえば、c# または Visual Basic .NET)、使い慣れた .NET で記述できます。</span><span class="sxs-lookup"><span data-stu-id="b0867-225">You can write the scripts in your favorite .NET programming language (for example, C# or Visual Basic .NET).</span></span> <span data-ttu-id="b0867-226">スクリプトを使用して、データベースのデータなどの動的なコンテンツを表示します。</span><span class="sxs-lookup"><span data-stu-id="b0867-226">You use scripts to display dynamic content such as database data.</span></span>

## <a name="understanding-models"></a><span data-ttu-id="b0867-227">Understanding モデル</span><span class="sxs-lookup"><span data-stu-id="b0867-227">Understanding Models</span></span>

<span data-ttu-id="b0867-228">コント ローラーを説明したおよびビューについて説明してきました。</span><span class="sxs-lookup"><span data-stu-id="b0867-228">We have discussed controllers and we have discussed views.</span></span> <span data-ttu-id="b0867-229">最後のトピックで説明する必要がありますが、モデルです。</span><span class="sxs-lookup"><span data-stu-id="b0867-229">The last topic that we need to discuss is models.</span></span> <span data-ttu-id="b0867-230">MVC モデルとは何ですか。</span><span class="sxs-lookup"><span data-stu-id="b0867-230">What is an MVC model?</span></span>

<span data-ttu-id="b0867-231">MVC モデルには、すべてのビューまたはコント ローラーに含まれていないこと、アプリケーション ロジックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="b0867-231">An MVC model contains all of your application logic that is not contained in a view or a controller.</span></span> <span data-ttu-id="b0867-232">モデルには、すべてのアプリケーションのビジネス ロジック、検証ロジック、およびデータベース アクセス ロジックを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0867-232">The model should contain all of your application business logic, validation logic, and database access logic.</span></span> <span data-ttu-id="b0867-233">たとえば、データベースにアクセスする、Microsoft の Entity Framework を使用している場合、Models フォルダーに、Entity Framework のクラス (.edmx ファイル) を作成です。</span><span class="sxs-lookup"><span data-stu-id="b0867-233">For example, if you are using the Microsoft Entity Framework to access your database, then you would create your Entity Framework classes (your .edmx file) in the Models folder.</span></span>

<span data-ttu-id="b0867-234">ビューには、ユーザー インターフェイスの生成に関連するロジックのみを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0867-234">A view should contain only logic related to generating the user interface.</span></span> <span data-ttu-id="b0867-235">コント ローラーには、右側のビューを取得または別のアクション (フロー制御) にユーザーをリダイレクトするために必要なロジックの最低限ののみを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0867-235">A controller should only contain the bare minimum of logic required to return the right view or redirect the user to another action (flow control).</span></span> <span data-ttu-id="b0867-236">他のすべては、モデルに含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0867-236">Everything else should be contained in the model.</span></span>

<span data-ttu-id="b0867-237">一般に、fat モデルおよびスキニー コント ローラーに限り必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0867-237">In general, you should strive for fat models and skinny controllers.</span></span> <span data-ttu-id="b0867-238">コント ローラーのメソッドは、数行のコードのみを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0867-238">Your controller methods should contain only a few lines of code.</span></span> <span data-ttu-id="b0867-239">コント ローラーのアクションが fat すぎるを取得する場合、Models フォルダーに新しいクラスをロジックの移動を検討してください。</span><span class="sxs-lookup"><span data-stu-id="b0867-239">If a controller action gets too fat, then you should consider moving the logic out to a new class in the Models folder.</span></span>

## <a name="summary"></a><span data-ttu-id="b0867-240">概要</span><span class="sxs-lookup"><span data-stu-id="b0867-240">Summary</span></span>

<span data-ttu-id="b0867-241">このチュートリアルは、web アプリケーション、ASP.NET MVC のさまざまな部分の高レベルな概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="b0867-241">This tutorial provided you with a high level overview of the different parts of an ASP.NET MVC web application.</span></span> <span data-ttu-id="b0867-242">特定のコント ローラーのアクションをブラウザーの入力方向の要求がどのように ASP.NET ルーティング マップについて学習しました。</span><span class="sxs-lookup"><span data-stu-id="b0867-242">You learned how ASP.NET Routing maps incoming browser requests to particular controller actions.</span></span> <span data-ttu-id="b0867-243">コント ローラーがブラウザーにビューを返す方法を調整する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="b0867-243">You learned how controllers orchestrate how views are returned to the browser.</span></span> <span data-ttu-id="b0867-244">最後に、モデルがアプリケーションの業務、検証、およびデータベース アクセス ロジックを含める方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="b0867-244">Finally, you learned how models contain application business, validation, and database access logic.</span></span>
