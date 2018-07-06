---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Visual Studio 2012 で Page Inspector の使用 |Microsoft Docs
author: rick-anderson
description: このハンズオン ラボでは、検索して、Visual Studio で Page Inspector で web ページの問題を修正する新しいツールを検出します。 Page Inspector は、新しいツール b.
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: ac945a23dc6ef060340320d047f13c8e81057138
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833673"
---
<a name="using-page-inspector-in-visual-studio-2012"></a><span data-ttu-id="00c9b-104">Visual Studio 2012 で Page Inspector の使用</span><span class="sxs-lookup"><span data-stu-id="00c9b-104">Using Page Inspector in Visual Studio 2012</span></span>
====================
<span data-ttu-id="00c9b-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="00c9b-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="00c9b-106">このハンズオン ラボでは、検索して、Visual Studio で Page Inspector で web ページの問題を修正する新しいツールを検出します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-106">In this Hands-on Lab, you will discover a new tool to find and fix web page issues in Visual Studio - the Page Inspector.</span></span>
> 
> <span data-ttu-id="00c9b-107">Page Inspector は、Visual Studio には、ブラウザー診断ツールと、ブラウザー、ASP.NET、およびソース コードの間で統合されたエクスペリエンスを提供する新しいツールです。</span><span class="sxs-lookup"><span data-stu-id="00c9b-107">Page Inspector is a new tool that brings browser diagnostics tools to Visual Studio and provides an integrated experience among the browser, ASP.NET, and source code.</span></span> <span data-ttu-id="00c9b-108">Visual Studio IDE 内で直接、web ページ (HTML、Web フォーム、ASP.NET MVC、または Web ページ) を表示し、ソース コードと結果の出力の両方を調べることができます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-108">It renders a web page (HTML, Web Forms, ASP.NET MVC, or Web Pages) directly within the Visual Studio IDE and lets you examine both the source code and the resulting output.</span></span> <span data-ttu-id="00c9b-109">Page Inspector を使用すると、web サイトを簡単に分解、迅速に、ゼロからページをビルドおよび問題をすばやく診断できます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-109">Page Inspector enables you to easily decompose a website, rapidly build pages from the ground up, and quickly diagnose issues.</span></span>
> 
> <span data-ttu-id="00c9b-110">最近では ASP.NET MVC と WebForms などの適切なタイミングで柔軟かつスケーラブルな web サイトを作成する Web フレームワークの番号があります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-110">Nowadays we have a number of Web frameworks that create flexible and scalable websites in a timely manner, such as ASP.NET MVC and WebForms.</span></span> <span data-ttu-id="00c9b-111">その一方で、困難に達して IDE がテンプレートに基づくページと動的なコンテンツのデザイナー ビューをサポートしていないため、ページ上の問題を検索します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-111">On the other hand, it gets harder to find issues on the pages because the IDE does not support the designer view in template-based pages and dynamic content.</span></span> <span data-ttu-id="00c9b-112">そのため、これらの web サイトをユーザーに表示される方法を表示するブラウザーで開く必要です。</span><span class="sxs-lookup"><span data-stu-id="00c9b-112">Therefore, these websites have to be opened in a browser to see how they appear to a user.</span></span>
> 
> <span data-ttu-id="00c9b-113">Web 開発者は、外部ツールを使用して、定期的に実行して、ブラウザーでの問題を見つけます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-113">Web developers use external tools to find issues that regularly run in the browser.</span></span> <span data-ttu-id="00c9b-114">次に、IDE に戻るし、修復を開始します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-114">Then, they return to the IDE and start fixing.</span></span> <span data-ttu-id="00c9b-115">これを行き来アクティビティ IDE、ブラウザー、およびプロファイリング ツールの間で、効率的なことができるし、新規に展開とキャッシュの問題を再現するたびにクリーニングが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-115">This back and forth activity among the IDE, the browser and the profiling tools can be inefficient, and sometimes requires a fresh deployment and cache cleaning each time you want to reproduce an issue.</span></span>
> 
> <span data-ttu-id="00c9b-116">Page Inspector は、の機能の組み合わせを使用して両方の長所を統合することによって、クライアント (ブラウザー ツール) とサーバー (ASP.NET およびソース コード) の間の Web 開発のギャップを橋渡しします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-116">Page Inspector bridges a gap in Web development between the client (browser tools) and the server (ASP.NET and source code) by bringing together the best of both worlds using a combined set of features.</span></span>
> 
> <span data-ttu-id="00c9b-117">Page Inspector を使用して、ブラウザーにレンダリングされる HTML マークアップを生成した (サーバー側コードを含む)、ソース ファイルの要素を確認できます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-117">Using Page Inspector, you can see which elements in the source files (including server-side code) have produced the HTML markup to be rendered in the browser.</span></span> <span data-ttu-id="00c9b-118">Page Inspector を使用して、CSS のプロパティと、ブラウザーで直ちに反映された変更を表示する DOM 要素の属性を変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-118">Page Inspector also lets you modify CSS properties and DOM element attributes to see the changes reflected immediately in the browser.</span></span>
> 
> <span data-ttu-id="00c9b-119">このハンズオン ラボは、Page Inspector の機能について説明し、それらを使用して Web アプリケーションで問題を修正する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-119">This hands-on lab will walk you through the Page Inspector features and show you how you can use them to fix issues in Web applications.</span></span> <span data-ttu-id="00c9b-120">**このラボには、2 つの手順ようなフローを使用して、さまざまなテクノロジを対象とするにはが含まれています。ASP.NET MVC 開発者の場合は、次の演習 1 つです。WebForms 開発者に従って演習 2 の場合**します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-120">**This lab contains two exercises using similar flows but targeting different technologies. If you are an ASP.NET MVC Developer, follow exercise one; if you are a WebForms developer follow exercise two**.</span></span>
> 
> <span data-ttu-id="00c9b-121">このラボでは、拡張機能とソース フォルダーにサンプル Web アプリケーションに軽微な変更を適用することで以前に説明する新機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-121">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="00c9b-122">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="00c9b-123">目的</span><span class="sxs-lookup"><span data-stu-id="00c9b-123">Objectives</span></span>

<span data-ttu-id="00c9b-124">このハンズオン ラボでは、学習する方法。</span><span class="sxs-lookup"><span data-stu-id="00c9b-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="00c9b-125">Page Inspector を使用して Web サイトを分解します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-125">Decompose a Web Site using Page Inspector</span></span>
- <span data-ttu-id="00c9b-126">検査し、Page Inspector で CSS スタイルの変更のプレビュー</span><span class="sxs-lookup"><span data-stu-id="00c9b-126">Inspect and preview CSS styles changes with Page Inspector</span></span>
- <span data-ttu-id="00c9b-127">検出し、Page Inspector を使用して、web ページで問題を修正</span><span class="sxs-lookup"><span data-stu-id="00c9b-127">Detect and fix issues in your web pages using Page Inspector</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="00c9b-128">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="00c9b-128">Prerequisites</span></span>

<span data-ttu-id="00c9b-129">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="00c9b-129">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="00c9b-130">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="00c9b-130">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="00c9b-131">Internet Explorer 9 以降</span><span class="sxs-lookup"><span data-stu-id="00c9b-131">Internet Explorer 9 or higher</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="00c9b-132">演習</span><span class="sxs-lookup"><span data-stu-id="00c9b-132">Exercises</span></span>

<span data-ttu-id="00c9b-133">このハンズオン ラボには、次の演習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-133">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="00c9b-134">手順 1: ASP.NET MVC プロジェクトで Page Inspector の使用</span><span class="sxs-lookup"><span data-stu-id="00c9b-134">Exercise 1: Using Page Inspector in ASP.NET MVC Projects</span></span>](#Exercise1)
2. [<span data-ttu-id="00c9b-135">手順 2: WebForms プロジェクトで Page Inspector の使用</span><span class="sxs-lookup"><span data-stu-id="00c9b-135">Exercise 2: Using Page Inspector in WebForms Projects</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="00c9b-136">各演習は、各演習を他のユーザーとは別にすることができますが、この演習の Begin フォルダーにあるソリューションを伴います。</span><span class="sxs-lookup"><span data-stu-id="00c9b-136">Each exercise is accompanied by a starting solution, located in the Begin folder of the exercise, that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="00c9b-137">演習のソース コード内で、対応する手順」の手順を完了に起因するコードを Visual Studio ソリューションを格納した End フォルダーを検索することもされます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-137">Inside the source code for an exercise, you will also find an End folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="00c9b-138">このハンズオン ラボを使用すると、追加のヘルプが必要な場合は、これらのソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-138">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


<span data-ttu-id="00c9b-139">この演習の所要時間を推定: **30 分**します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-139">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a><span data-ttu-id="00c9b-140">手順 1: ASP.NET MVC プロジェクトで Page Inspector の使用</span><span class="sxs-lookup"><span data-stu-id="00c9b-140">Exercise 1: Using Page Inspector in ASP.NET MVC Projects</span></span>

<span data-ttu-id="00c9b-141">この演習では、プレビュー、およびデバッグする方法について説明します、 **ASP.NET MVC 4**ソリューションを使用して**Page Inspector**します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-141">In this exercise, you will learn how to preview and debug an **ASP.NET MVC 4** solution using **Page Inspector**.</span></span> <span data-ttu-id="00c9b-142">まず、簡単な簡単なツールについては、Web のプロセスのデバッグを容易にする機能を実行します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-142">First, you will perform a brief lap around the tool to learn the features that facilitate the Web debugging process.</span></span> <span data-ttu-id="00c9b-143">次に、スタイルの問題を含む web ページで作業します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-143">Then, you will work in a web page that contains styling issues.</span></span> <span data-ttu-id="00c9b-144">Page Inspector を使用して、問題を生成するソース コードを検索し、その修正方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-144">You will learn how to use Page Inspector to find the source code that generates the issue and fix it.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a><span data-ttu-id="00c9b-145">タスク 1 - Page Inspector の調査</span><span class="sxs-lookup"><span data-stu-id="00c9b-145">Task 1 - Exploring Page Inspector</span></span>

<span data-ttu-id="00c9b-146">このタスクでは、フォト ギャラリーを表示する ASP.NET MVC 4 プロジェクトのコンテキストで Page Inspector を使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-146">In this task, you will learn how to use the Page Inspector in the context of an ASP.NET MVC 4 project that shows a photo gallery.</span></span>

1. <span data-ttu-id="00c9b-147">開く、**開始**ソリューションがある**ソース/Ex1-MVC4/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="00c9b-147">Open the **Begin** solution located at **Source/Ex1-MVC4/Begin/** folder.</span></span>

   1. <span data-ttu-id="00c9b-148">いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行する前にします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-148">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="00c9b-149">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-149">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="00c9b-150">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="00c9b-150">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="00c9b-151">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-151">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="00c9b-152">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="00c9b-152">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="00c9b-153">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-153">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="00c9b-154">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-154">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="00c9b-155">ソリューション エクスプ ローラーで検索**Index.cshtml**下の表示、 **/ビュー/ホーム**プロジェクト フォルダーで、右クリックし、選択**Page Inspector で表示**します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-155">In the Solution Explorer, locate **Index.cshtml** view under the **/Views/Home** project folder, right-click it and select **View in Page Inspector**.</span></span>

    <span data-ttu-id="00c9b-156">![Page Inspector 内でプレビューするファイルを選択する](using-page-inspector-in-visual-studio-2012/_static/image1.png "Page Inspector 内でプレビューするファイルを選択します。")</span><span class="sxs-lookup"><span data-stu-id="00c9b-156">![Selecting a file to preview in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image1.png "Selecting a file to preview in Page Inspector")</span></span>

    <span data-ttu-id="00c9b-157">*Page Inspector 内でプレビューするファイルを選択します。*</span><span class="sxs-lookup"><span data-stu-id="00c9b-157">*Selecting a file to preview in Page Inspector*</span></span>
3. <span data-ttu-id="00c9b-158">Page Inspector のウィンドウが表示されます、 */ホーム/インデックス*URL ソースを選択したビューにマップします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-158">The Page Inspector window will show the */Home/Index* URL mapped to the source View you selected.</span></span>

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    <span data-ttu-id="00c9b-160">*Page Inspector で最初にお問い合わせください。*</span><span class="sxs-lookup"><span data-stu-id="00c9b-160">*The first contact with Page Inspector*</span></span>

    <span data-ttu-id="00c9b-161">Page Inspector ツールは、Visual Studio 環境で統合されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-161">The Page Inspector tool is integrated in your Visual Studio environment.</span></span> <span data-ttu-id="00c9b-162">インスペクターには、強力な HTML プロファイラーと共に、埋め込まれたブラウザーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="00c9b-162">The inspector contains an embedded browser, together with a powerful HTML profiler.</span></span> <span data-ttu-id="00c9b-163">注意してください、ページの外観を確認するソリューションを実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="00c9b-163">Notice that you do not have to run the solution to see how your pages look.</span></span>

    > [!NOTE]
    > <span data-ttu-id="00c9b-164">Page Inspector のブラウザーの幅が開かれているページの幅未満の場合は表示されませんページ適切です。</span><span class="sxs-lookup"><span data-stu-id="00c9b-164">When the width of Page Inspector browser is less than the width of the opened page, you will not see the page properly.</span></span> <span data-ttu-id="00c9b-165">このような場合は、Page Inspector の幅を調整します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-165">If that happens, adjust the width of the Page Inspector.</span></span>
4. <span data-ttu-id="00c9b-166">をクリックして、**ファイル**Page Inspector 内でのタブ。</span><span class="sxs-lookup"><span data-stu-id="00c9b-166">Click the **Files** tab in Page Inspector.</span></span>

    <span data-ttu-id="00c9b-167">インデックス ページを作成するすべてのソース ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-167">You will see all the source files that are composing the Index page.</span></span> <span data-ttu-id="00c9b-168">この機能は、部分ビューとテンプレートを使用している場合は特に、ひとめですべての要素を識別するために役立ちます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-168">This feature helps to identify all the elements at a glance, especially when you are working with partial views and templates.</span></span> <span data-ttu-id="00c9b-169">開くことができますも、ファイルの各リンクをクリックする場合に注意してください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-169">Notice that you can also open each of the files if you click the links.</span></span>

    ![[ファイル] タブ](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    <span data-ttu-id="00c9b-171">*[ファイル] タブ*</span><span class="sxs-lookup"><span data-stu-id="00c9b-171">*The Files tab*</span></span>
5. <span data-ttu-id="00c9b-172">をクリックして、**検査モードの切り替え**タブの左側にあるボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-172">Click the **Toggle Inspection Mode** button, located at the left of the tabs.</span></span>

    <span data-ttu-id="00c9b-173">このツールを使用すると、ページの任意の要素を選択し、HTML および Razor コードを参照してください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-173">This tool will let you select any element of the page and see its HTML and Razor code.</span></span>

    ![トグル検査モード ボタン](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    <span data-ttu-id="00c9b-175">*トグル検査モード ボタン*</span><span class="sxs-lookup"><span data-stu-id="00c9b-175">*Toggle Inspection Mode button*</span></span>
6. <span data-ttu-id="00c9b-176">Page Inspector のブラウザーでページの要素に対するマウス ポインターを移動します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-176">In the Page Inspector browser, move the mouse pointer over the page elements.</span></span> <span data-ttu-id="00c9b-177">表示するページの任意の部分にマウス ポインターを移動するときに、要素の型が表示され、対応するソース マークアップまたはコードが Visual Studio エディターで強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-177">While you move the mouse pointer over any part of the rendered page, the element type is displayed and the corresponding source markup or code is highlighted in the Visual Studio editor.</span></span>

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    <span data-ttu-id="00c9b-179">*検査モードの動作*</span><span class="sxs-lookup"><span data-stu-id="00c9b-179">*Inspection mode in action*</span></span>

    > [!NOTE]
    > <span data-ttu-id="00c9b-180">Page Inspector ウィンドウを最大化しないか、ソース コードを示す プレビュー タブを表示することはできません。</span><span class="sxs-lookup"><span data-stu-id="00c9b-180">Do not maximize the Page Inspector window or you will not be able to see the preview tab showing the source code.</span></span> <span data-ttu-id="00c9b-181">最大化されたときに、Page Inspector 内で要素をクリックすると、選択範囲のソース コードが表示されますが、Page Inspector ウィンドウを非表示になります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-181">If you click the element in Page Inspector when it is maximized, the source code of the selection will appear but it will hide the Page Inspector window.</span></span>

    <span data-ttu-id="00c9b-182">注意する場合、 **Index.cshtml**ファイルが表示されます、選択した要素を生成するソース コードの部分が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-182">If you pay attention to the **Index.cshtml** file, you will notice that the portion of source code that generates the selected element is highlighted.</span></span> <span data-ttu-id="00c9b-183">この機能では、コードにアクセスする直接的および高速の方法を提供する長いソース ファイルの編集が容易になります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-183">This feature facilitates the editing of long source files, providing a direct and fast way to access the code.</span></span>

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    <span data-ttu-id="00c9b-185">*要素の検査*</span><span class="sxs-lookup"><span data-stu-id="00c9b-185">*Inspecting elements*</span></span>
7. <span data-ttu-id="00c9b-186">クリックして、**検査モードを切り替える**ボタン (![Page Inspector のブラウザーでレンダリングされた HTML コードを表示する HTML タブを選択します.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Page Inspector のブラウザーでレンダリングされた HTML コードを表示する HTML タブを選択します.")</span><span class="sxs-lookup"><span data-stu-id="00c9b-186">Click the **Toggle Inspection Mode** button (![Select the HTML tab to display the HTML code rendered in the Page Inspector browser.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Select the HTML tab to display the HTML code rendered in the Page Inspector browser.")</span></span> <span data-ttu-id="00c9b-187">) 、カーソルを無効にします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-187">) to disable the cursor.</span></span>
8. <span data-ttu-id="00c9b-188">選択、 **HTML** Page Inspector のブラウザーでレンダリングされた HTML コードを表示するタブ。</span><span class="sxs-lookup"><span data-stu-id="00c9b-188">Select the **HTML** tab to display the HTML code rendered in the Page Inspector browser.</span></span>
9. <span data-ttu-id="00c9b-189">HTML マークアップで Koala リンクのリスト アイテムを見つけて選択します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-189">In the HTML markup, locate the list item with the Koala link and select it.</span></span>

    <span data-ttu-id="00c9b-190">コードを選択すると、対応する出力は、ブラウザーでは強調表示に自動的に注目してください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-190">Notice that when you select the code, the corresponding output is automatically highlighted in the browser.</span></span> <span data-ttu-id="00c9b-191">この機能は、HTML ブロックをページに表示する方法を確認するのに便利です。</span><span class="sxs-lookup"><span data-stu-id="00c9b-191">This feature is useful to see how an HTML block is rendered on the page.</span></span>

    <span data-ttu-id="00c9b-192">![ページの HTML 要素を選択する](using-page-inspector-in-visual-studio-2012/_static/image8.png "ページ内の選択の HTML 要素")</span><span class="sxs-lookup"><span data-stu-id="00c9b-192">![Selecting HTML element in the page](using-page-inspector-in-visual-studio-2012/_static/image8.png "Selecting HTML element in the page")</span></span>

    <span data-ttu-id="00c9b-193">*ページの HTML 要素の選択*</span><span class="sxs-lookup"><span data-stu-id="00c9b-193">*Selecting HTML element in the page*</span></span>
10. <span data-ttu-id="00c9b-194">をクリックして、**検査モードの切り替え**有効にするボタン*検査モード*ナビゲーション バーをクリックします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-194">Click the **Toggle Inspection Mode** button to enable *Inspection Mode* and click the navigation bar.</span></span> <span data-ttu-id="00c9b-195">スタイルのウィンドウで、HTML コードの右側に、選択した要素に適用される CSS スタイルを使用して一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-195">On the right of the HTML code, in the Styles pane, you will see a list with the CSS styles applied to the selected element.</span></span>

    > [!NOTE]
    > <span data-ttu-id="00c9b-196">Page Inspector は開くことも、ヘッダーは、サイトのレイアウトの一部であるため\_Layout.cshtml ファイルとコードのセグメントが影響を強調表示します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-196">Since the header is a part of the site layout, Page Inspector will also open \_Layout.cshtml file and highlight the segment of code affected.</span></span>

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    <span data-ttu-id="00c9b-198">*スタイルと、選択した要素のソース ファイルを検出します。*</span><span class="sxs-lookup"><span data-stu-id="00c9b-198">*Discovering styles and source files of a selected element*</span></span>
11. <span data-ttu-id="00c9b-199">切り替え検査ポインターを有効になっている、おすすめ青いバーの下にマウス ポインターを移動し、半分の円をクリックします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-199">With the toggle inspection pointer enabled, move the mouse pointer below the blue featured bar and click the half circle.</span></span>

    <span data-ttu-id="00c9b-200">![要素の選択](using-page-inspector-in-visual-studio-2012/_static/image10.png "要素の選択")</span><span class="sxs-lookup"><span data-stu-id="00c9b-200">![Selecting an element](using-page-inspector-in-visual-studio-2012/_static/image10.png "Selecting an element")</span></span>

    <span data-ttu-id="00c9b-201">*要素の選択*</span><span class="sxs-lookup"><span data-stu-id="00c9b-201">*Selecting an element*</span></span>
12. <span data-ttu-id="00c9b-202">スタイルのウィンドウで検索、**背景イメージ**] の [、 **.main コンテンツ**グループ。</span><span class="sxs-lookup"><span data-stu-id="00c9b-202">In the Styles pane, locate the **background-image** item under the **.main-content** group.</span></span> <span data-ttu-id="00c9b-203">**オフに**、**背景イメージ**みてください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-203">**Uncheck** the **background-image** and see what happens.</span></span> <span data-ttu-id="00c9b-204">ブラウザーで、変更がすぐに反映されますを非表示には、円が表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-204">You will notice that the browser will reflect the changes immediately and the circle is hidden.</span></span>

    > [!NOTE]
    > <span data-ttu-id="00c9b-205">ページのインスペクターのスタイル タブに適用する変更は、元のスタイル シートには影響しません。</span><span class="sxs-lookup"><span data-stu-id="00c9b-205">The changes you apply on the Page Inspector Styles tab do not affect the original stylesheet.</span></span> <span data-ttu-id="00c9b-206">スタイルをオフにしたり、何度でも、ページの更新後に復元されますが、値を変更できます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-206">You can uncheck styles or change their values as many times as you want, but they will be restored after refreshing the page.</span></span>

    <span data-ttu-id="00c9b-207">![有効にして、CSS スタイルを無効化](using-page-inspector-in-visual-studio-2012/_static/image11.png "の有効化と CSS スタイルを無効にします。")</span><span class="sxs-lookup"><span data-stu-id="00c9b-207">![Enabling and disabling CSS styles](using-page-inspector-in-visual-studio-2012/_static/image11.png "Enabling and disabling CSS styles")</span></span>

    <span data-ttu-id="00c9b-208">*有効にして、CSS スタイルを無効化*</span><span class="sxs-lookup"><span data-stu-id="00c9b-208">*Enabling and disabling CSS styles*</span></span>
13. <span data-ttu-id="00c9b-209">をクリックして、'**貴社のロゴ**' 検査モードを使用して、ヘッダーのテキスト。</span><span class="sxs-lookup"><span data-stu-id="00c9b-209">Now, click the '**your logo here**' text on the header using the inspection mode.</span></span>
14. <span data-ttu-id="00c9b-210">**スタイル** タブで、検索、**フォント サイズ**CSS 属性、 **.site タイトル**グループ。</span><span class="sxs-lookup"><span data-stu-id="00c9b-210">In the **Styles** tab, locate the **font-size** CSS attribute under the **.site-title** group.</span></span> <span data-ttu-id="00c9b-211">属性の値をダブルクリックし、2.3 em 値の置換**3 em**、キーを押しますと **」と入力**します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-211">Double-click the attribute value and replace the 2.3 em value with **3 em**, and then press **ENTER**.</span></span> <span data-ttu-id="00c9b-212">タイトルが大きいことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-212">Notice that the title looks bigger.</span></span>

    <span data-ttu-id="00c9b-213">![Page Inspector 内での CSS 値を変更する](using-page-inspector-in-visual-studio-2012/_static/image12.png "Page Inspector 内で値を変更する CSS")</span><span class="sxs-lookup"><span data-stu-id="00c9b-213">![Changing CSS values in the Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image12.png "Changing CSS values in the Page Inspector")</span></span>

    <span data-ttu-id="00c9b-214">*Page Inspector 内での CSS 値を変更します。*</span><span class="sxs-lookup"><span data-stu-id="00c9b-214">*Changing CSS values in the Page Inspector*</span></span>
15. <span data-ttu-id="00c9b-215">をクリックして、**トレース スタイル** タブで、Page Inspector の右側のウィンドウにあります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-215">Click the **Trace Styles** tab, located in the right pane of Page Inspector.</span></span> <span data-ttu-id="00c9b-216">これは、属性の名前順で、選択範囲に適用されるすべてのスタイルを表示する別の方法です。</span><span class="sxs-lookup"><span data-stu-id="00c9b-216">This is an alternative way to see all the styles applied to the selection, ordered by attribute name.</span></span>

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    <span data-ttu-id="00c9b-218">*選択した要素の CSS スタイルのトレース*</span><span class="sxs-lookup"><span data-stu-id="00c9b-218">*CSS styles tracing of the selected element*</span></span>
16. <span data-ttu-id="00c9b-219">Page Inspector のもう 1 つの機能は、レイアウト ペインです。</span><span class="sxs-lookup"><span data-stu-id="00c9b-219">Another feature of Page Inspector is the Layout pane.</span></span> <span data-ttu-id="00c9b-220">検査モードを使用して、ナビゲーション バーを選択し、クリックして、**レイアウト**右側のウィンドウ タブ。</span><span class="sxs-lookup"><span data-stu-id="00c9b-220">Using the inspection mode, select the navigation bar and then click the **Layout** tab on the right pane.</span></span> <span data-ttu-id="00c9b-221">選択した要素の正確なサイズと、オフセット、余白、パディングおよび境界線のサイズが表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-221">You will see the exact size of the selected element, as well as its offset, margin, padding and border size.</span></span> <span data-ttu-id="00c9b-222">このビューから値を変更することも注目してください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-222">Notice that you can also modify the values from this view.</span></span>

    <span data-ttu-id="00c9b-223">![Page Inspector 内で要素のレイアウト](using-page-inspector-in-visual-studio-2012/_static/image14.png "Page Inspector 内で要素のレイアウト")</span><span class="sxs-lookup"><span data-stu-id="00c9b-223">![Element layout in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image14.png "Element layout in Page Inspector")</span></span>

    <span data-ttu-id="00c9b-224">*Page Inspector 内で要素のレイアウト*</span><span class="sxs-lookup"><span data-stu-id="00c9b-224">*Element layout in Page Inspector*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a><span data-ttu-id="00c9b-225">タスク 2 - 検索して、フォト ギャラリーでスタイルの問題を修正</span><span class="sxs-lookup"><span data-stu-id="00c9b-225">Task 2 - Finding and Fixing Style Issues in the Photo Gallery</span></span>

<span data-ttu-id="00c9b-226">Visual Studio の以前のバージョンとの Web ページの問題を診断するにはでしょうか。</span><span class="sxs-lookup"><span data-stu-id="00c9b-226">How would you diagnose Web pages issues with previous versions of Visual Studio?</span></span> <span data-ttu-id="00c9b-227">なじみの web デバッグ ツール、Internet Explorer 開発者ツールや Firebug など、Visual Studio IDE の外部で実行するとしています。</span><span class="sxs-lookup"><span data-stu-id="00c9b-227">You are likely familiar with web debugging tools that run outside the Visual Studio IDE, like Internet Explorer Developer Tools or Firebug.</span></span> <span data-ttu-id="00c9b-228">ブラウザーでのみ認識、HTML スクリプトとスタイル中、基になるフレームワークには、レンダリングされる HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-228">Browsers only understand HTML, scripting and styles, while an underlying framework generates the HTML that will be rendered.</span></span> <span data-ttu-id="00c9b-229">そのため、多くの場合、サイト全体のように web ページの外観を確認するを展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-229">For that reason, you often need to deploy the whole site to see how web pages look like.</span></span>

<span data-ttu-id="00c9b-230">検出し、web サイトの問題を修正しました。 必要な場合に、次の手順が後にいた可能性があります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-230">You had probably followed these steps when you wanted to detect and fix an issue in your web site:</span></span>

1. <span data-ttu-id="00c9b-231">Visual studio でソリューションを実行または web サーバー上のページをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-231">Run the Solution from Visual Studio, or deploy the page on the web server.</span></span>
2. <span data-ttu-id="00c9b-232">ブラウザーで開発者ツールを使用または単にソース コードを開き、スタイルしようとする、問題の一致を開きます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-232">In the browser, open the developer tools you use, or simply open the source code and the styles and try to match the issue.</span></span> <span data-ttu-id="00c9b-233">使用を関連するファイルを検索する、&quot;検索&quot;または&quot;ファイルで検索&quot;スタイル クラスの名前で機能します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-233">To find the files involved, you would have used the &quot;Search&quot; or &quot;Search in files&quot; features with the name of the style classes.</span></span>
3. <span data-ttu-id="00c9b-234">エラーが検出されると、Web ブラウザーとサーバーを停止します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-234">Once the error is detected, stop the Web browser and the server.</span></span>
4. <span data-ttu-id="00c9b-235">ブラウザーのキャッシュを消去します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-235">Clear the browser cache.</span></span>
5. <span data-ttu-id="00c9b-236">修正を適用する Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-236">Return to Visual Studio to apply a fix.</span></span> <span data-ttu-id="00c9b-237">テストするすべての手順を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-237">Repeat all the steps to test.</span></span>

<span data-ttu-id="00c9b-238">ASP.NET MVC 4 の実際の WYSIWYG が存在しないためスタイルの問題のほとんどを実行しているまたは web アプリケーションを展開した後、後の段階で検出されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-238">As there is no real WYSIWYG in ASP.NET MVC 4, most of the style issues are detected on a later stage, after running or deploying the web application.</span></span> <span data-ttu-id="00c9b-239">ここで Page Inspector、ソリューションを実行することがなく任意のページをプレビューできることです。</span><span class="sxs-lookup"><span data-stu-id="00c9b-239">Now, with Page Inspector, it is possible to preview any page without running the solution.</span></span>

<span data-ttu-id="00c9b-240">このタスクでは、Page inspector を使用し、フォト ギャラリーのアプリケーションのいくつかの問題を修正します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-240">In this task, you will use the Page inspector and fix some issues the Photo Gallery application.</span></span>

1. <span data-ttu-id="00c9b-241">Page Inspector を使用して探します、**登録**と**ログイン**ヘッダーの左側にあるリンクです。</span><span class="sxs-lookup"><span data-stu-id="00c9b-241">Using Page Inspector, locate the **Register** and the **Log in** links at the left side of the header.</span></span>

    <span data-ttu-id="00c9b-242">リンクは、右側に期待どおりの場所に表示されませんが箇条書きリストのように表示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-242">Notice that the links are not displayed at the expected place on the right, and they are shown like a bulleted list.</span></span> <span data-ttu-id="00c9b-243">それに応じてスタイルを変更して、右へのリンクを整列するされます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-243">You will now align the links to the right and restyle them accordingly.</span></span>

    <span data-ttu-id="00c9b-244">![リンクで、登録とログを検索する](using-page-inspector-in-visual-studio-2012/_static/image15.png "リンクで、登録とログを検索します。")</span><span class="sxs-lookup"><span data-stu-id="00c9b-244">![Locating the Register and Log in links](using-page-inspector-in-visual-studio-2012/_static/image15.png "Locating the Register and Log in links")</span></span>

    <span data-ttu-id="00c9b-245">*リンクで、登録とログを検索します。*</span><span class="sxs-lookup"><span data-stu-id="00c9b-245">*Locating the Register and Log in links*</span></span>
2. <span data-ttu-id="00c9b-246">選択されている検査モードの切り替えに近いではなく、そのコードを開くための登録リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-246">With Toggle Inspection Mode selected, click close to, but not on, the Register link to open its code.</span></span>

    <span data-ttu-id="00c9b-247">通知のリンクのソース コードに配置されている、  **\_LoginPartial.cshtml**ファイル、Index.cshtml いないも\_Layout.cshtml は、最初に確認することがあります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-247">Notice that the source code of the links is located in the **\_LoginPartial.cshtml** file, not the Index.cshtml nor the \_Layout.cshtml, which are the places you might look in first place.</span></span> <span data-ttu-id="00c9b-248">適切なソース ファイルに直接配置されています。</span><span class="sxs-lookup"><span data-stu-id="00c9b-248">You have been placed directly in the correct source file.</span></span>
3. <span data-ttu-id="00c9b-249">**スタイル** タブを見つけて、クリックして、 **<section> #login</section>** 項目で、これらのリンクの HTML コンテナーです。</span><span class="sxs-lookup"><span data-stu-id="00c9b-249">In the **Styles** tab, locate and click the **<section> #login</section>** item, which is the HTML container for these links.</span></span>

    <span data-ttu-id="00c9b-250">いることを確認、 **#login**スタイルが自動的に**Site.css**  をクリック後します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-250">Notice that the **#login** style is automatically located in **Site.css** after you click.</span></span> <span data-ttu-id="00c9b-251">さらに、コードは強調表示されたようになりました。</span><span class="sxs-lookup"><span data-stu-id="00c9b-251">Moreover, the code is now highlighted.</span></span>

    <span data-ttu-id="00c9b-252">![CSS スタイルの選択](using-page-inspector-in-visual-studio-2012/_static/image16.png "CSS スタイルの選択")</span><span class="sxs-lookup"><span data-stu-id="00c9b-252">![Selecting the CSS styles](using-page-inspector-in-visual-studio-2012/_static/image16.png "Selecting the CSS styles")</span></span>

    <span data-ttu-id="00c9b-253">*CSS スタイルの選択*</span><span class="sxs-lookup"><span data-stu-id="00c9b-253">*Selecting the CSS styles*</span></span>
4. <span data-ttu-id="00c9b-254">コメントを解除、**テキスト配置**属性の開始タグと終了文字を削除することで強調表示されたコードと保存、 **Site.css**ファイル。</span><span class="sxs-lookup"><span data-stu-id="00c9b-254">Uncomment the **text-align** attribute in the highlighted code by removing the opening and closing characters and save the **Site.css** file.</span></span>

    <span data-ttu-id="00c9b-255">Page Inspector は、現在のページを構成するすべてのファイルを認識し、これらのファイルのいずれかが変更を検出できます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-255">Page Inspector is aware of all the different files that compose the current page, and it can detect when any of these files change.</span></span> <span data-ttu-id="00c9b-256">ブラウザーで現在のページがソース ファイルと同期されるたびに、警告されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-256">It alerts you whenever the current page in browser is not in sync with the source files.</span></span>
5. <span data-ttu-id="00c9b-257">Page Inspector のブラウザーでページを再読み込みアドレス バーの下にあるバーをクリックします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-257">In the Page Inspector browser, click the bar located below the address bar to reload the page.</span></span>

    ![ページの再読み込み](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    <span data-ttu-id="00c9b-259">*ページの再読み込み*</span><span class="sxs-lookup"><span data-stu-id="00c9b-259">*Reloading the page*</span></span>

    <span data-ttu-id="00c9b-260">リンクは、右側にある、ようになりましたも箇条書きリストのように見えます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-260">The links are now at the right, but they still look like a bulleted list.</span></span> <span data-ttu-id="00c9b-261">次に、行頭文字を削除し、リンクを左右に配置します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-261">Now, you will remove the bullets and align the links horizontally.</span></span>

    ![更新されたページ](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    <span data-ttu-id="00c9b-263">*更新されたページ*</span><span class="sxs-lookup"><span data-stu-id="00c9b-263">*Updated page*</span></span>
6. <span data-ttu-id="00c9b-264">いずれかを選択して、検査モードを使用して、 **&lt;li&gt;** 項目が含まれている、&quot;登録&quot;と&quot;ログイン&quot;リンク。</span><span class="sxs-lookup"><span data-stu-id="00c9b-264">Using the inspection mode, select any of the **&lt;li&gt;** items that contain the &quot;Register&quot; and &quot;Log in&quot; links.</span></span> <span data-ttu-id="00c9b-265">をクリックし、 **&lt;セクション&gt;#login**項目へのアクセスを**Styles.css**コード。</span><span class="sxs-lookup"><span data-stu-id="00c9b-265">Then, click the **&lt;section&gt; #login** item to access **Styles.css** code.</span></span>

    <span data-ttu-id="00c9b-266">![スタイルの検索](using-page-inspector-in-visual-studio-2012/_static/image19.png "のスタイルの検索")</span><span class="sxs-lookup"><span data-stu-id="00c9b-266">![Finding the style](using-page-inspector-in-visual-studio-2012/_static/image19.png "Finding the style")</span></span>

    <span data-ttu-id="00c9b-267">*スタイルの検索*</span><span class="sxs-lookup"><span data-stu-id="00c9b-267">*Finding the style*</span></span>
7. <span data-ttu-id="00c9b-268">**Style.css**、コードをコメント解除します **#login li**項目。</span><span class="sxs-lookup"><span data-stu-id="00c9b-268">In **Style.css**, uncomment the code for **#login li** items.</span></span> <span data-ttu-id="00c9b-269">追加するスタイルは行頭文字を非表示にして、水平方向に項目を表示します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-269">The style you are adding will hide the bullet and display the items horizontally.</span></span>

    <span data-ttu-id="00c9b-270">![ログインのリンク スタイルの変更](using-page-inspector-in-visual-studio-2012/_static/image20.png "ログインのリンク スタイルの変更")</span><span class="sxs-lookup"><span data-stu-id="00c9b-270">![Restyling the login links](using-page-inspector-in-visual-studio-2012/_static/image20.png "Restyling the login links")</span></span>

    <span data-ttu-id="00c9b-271">*ログインのリンク スタイルの変更*</span><span class="sxs-lookup"><span data-stu-id="00c9b-271">*Restyling the login links*</span></span>
8. <span data-ttu-id="00c9b-272">保存**Style.css**ファイルを開き、ページを再読み込みするアドレスの下にあるバーを 1 回クリックします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-272">Save **Style.css** file and click once on the bar located below the address to reload the page.</span></span> <span data-ttu-id="00c9b-273">リンクが正しく表示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-273">Notice that the links appear correctly.</span></span>

    <span data-ttu-id="00c9b-274">![リンクが右側に揃えて配置](using-page-inspector-in-visual-studio-2012/_static/image21.png "へのリンクが右側に配置")</span><span class="sxs-lookup"><span data-stu-id="00c9b-274">![Links aligned to the right side](using-page-inspector-in-visual-studio-2012/_static/image21.png "Links aligned to the right side")</span></span>

    <span data-ttu-id="00c9b-275">*右揃えで配置へのリンク*</span><span class="sxs-lookup"><span data-stu-id="00c9b-275">*Links aligned to the right side*</span></span>
9. <span data-ttu-id="00c9b-276">最後に、ヘッダーのタイトルを変更します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-276">Finally, you will change the header title.</span></span> <span data-ttu-id="00c9b-277">検査モードを使用 をクリックして**貴社のロゴ**テキストと生成されたソース コードを取得します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-277">Use the inspection mode to click **your logo here** text and get to the source code that generates it.</span></span>
10. <span data-ttu-id="00c9b-278">使用するようになりました **\_Layout.cshtml**、置き換える '**貴社のロゴ**'テキスト'**フォト ギャラリー**'。</span><span class="sxs-lookup"><span data-stu-id="00c9b-278">Now you are in **\_Layout.cshtml**, replace '**your logo here**' text with '**Photo Gallery**'.</span></span> <span data-ttu-id="00c9b-279">保存して Page Inspector のブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-279">Save and update the Page Inspector browser.</span></span>

    <span data-ttu-id="00c9b-280">![新しいタイトルを割り当てる](using-page-inspector-in-visual-studio-2012/_static/image22.png "新しいタイトルを割り当てる")</span><span class="sxs-lookup"><span data-stu-id="00c9b-280">![Assigning a new title](using-page-inspector-in-visual-studio-2012/_static/image22.png "Assigning a new title")</span></span>

    <span data-ttu-id="00c9b-281">*新しいタイトルを割り当てる*</span><span class="sxs-lookup"><span data-stu-id="00c9b-281">*Assigning a new title*</span></span>

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    <span data-ttu-id="00c9b-283">*フォト ギャラリー ページの更新*</span><span class="sxs-lookup"><span data-stu-id="00c9b-283">*Photo Gallery page updated*</span></span>
11. <span data-ttu-id="00c9b-284">最後に、完了、 **PhotoGallery**プロジェクト キーを押します**f5 キーを押して**アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-284">Finally, selet the **PhotoGallery** project and press **F5** to run the app.</span></span> <span data-ttu-id="00c9b-285">チェック アウトすべて予想どおりに変更します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-285">Check out all the changes work as expected.</span></span>

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a><span data-ttu-id="00c9b-286">手順 2: WebForms プロジェクトで Page Inspector の使用</span><span class="sxs-lookup"><span data-stu-id="00c9b-286">Exercise 2: Using Page Inspector in WebForms Projects</span></span>

<span data-ttu-id="00c9b-287">この演習では、プレビュー、Page Inspector を使用して、WebForms ソリューションをデバッグする方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-287">In this exercise, you will learn how to preview and debug a WebForms solution using Page Inspector.</span></span> <span data-ttu-id="00c9b-288">については、Page Inspector の機能を Web のプロセスのデバッグを容易にするツールの簡単な概要をまず実行します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-288">You will first perform a brief lap around the tool to learn the Page Inspector features that facilitate the Web debugging process.</span></span> <span data-ttu-id="00c9b-289">次に、スタイルの問題を含む web ページで作業します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-289">Then, you will work in a web page that contains styling issues.</span></span> <span data-ttu-id="00c9b-290">Page Inspector を使用して、問題を生成するソース コードを検索し、その修正方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-290">You will learn how to use Page Inspector to find the source code that generates the issue and fix it.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a><span data-ttu-id="00c9b-291">タスク 1 - Page Inspector の調査</span><span class="sxs-lookup"><span data-stu-id="00c9b-291">Task 1 - Exploring Page Inspector</span></span>

<span data-ttu-id="00c9b-292">このタスクでは、フォト ギャラリーを表示するための WebForms プロジェクトのコンテキストで Page Inspector の機能を使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-292">In this task, you will learn how to use the Page Inspector features in the context of a WebForms project that shows a photo gallery.</span></span>

1. <span data-ttu-id="00c9b-293">開く、**開始**ソリューションがある**ソース/Ex2-WebForms/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="00c9b-293">Open the **Begin** solution located at **Source/Ex2-WebForms/Begin/** folder.</span></span>

   1. <span data-ttu-id="00c9b-294">いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行する前にします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-294">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="00c9b-295">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-295">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="00c9b-296">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="00c9b-296">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="00c9b-297">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-297">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="00c9b-298">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="00c9b-298">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="00c9b-299">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-299">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="00c9b-300">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-300">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="00c9b-301">ソリューション エクスプ ローラーで検索**Default.aspx**  ページで、右クリックし、選択**Page Inspector で表示**します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-301">In the Solution Explorer, locate **Default.aspx** page, right-click it and select **View in Page Inspector**.</span></span>

    <span data-ttu-id="00c9b-302">![Page Inspector を開くと Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image24.png "Page Inspector で Default.aspx を開く")</span><span class="sxs-lookup"><span data-stu-id="00c9b-302">![Opening Default.aspx with Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image24.png "Opening Default.aspx with Page Inspector")</span></span>

    <span data-ttu-id="00c9b-303">*Page Inspector で Default.aspx を開く*</span><span class="sxs-lookup"><span data-stu-id="00c9b-303">*Opening Default.aspx with Page Inspector*</span></span>
3. <span data-ttu-id="00c9b-304">Page Inspector ウィンドウは、Default.aspx に表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-304">The Page Inspector window will show Default.aspx.</span></span>

    <span data-ttu-id="00c9b-305">![Page Inspector 内で Default.aspx を表示する](using-page-inspector-in-visual-studio-2012/_static/image25.png "Page Inspector 内で Default.aspx を表示します。")</span><span class="sxs-lookup"><span data-stu-id="00c9b-305">![Viewing Default.aspx in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image25.png "Viewing Default.aspx in Page Inspector")</span></span>

    <span data-ttu-id="00c9b-306">*Page Inspector 内で Default.aspx を表示します。*</span><span class="sxs-lookup"><span data-stu-id="00c9b-306">*Viewing Default.aspx in Page Inspector*</span></span>

    <span data-ttu-id="00c9b-307">Page Inspector ツールは、Visual Studio 環境で統合されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-307">The Page Inspector tool is integrated in your Visual Studio environment.</span></span> <span data-ttu-id="00c9b-308">インスペクターには、選択したコードを示す強力な HTML プロファイラーと共に、埋め込まれたブラウザーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="00c9b-308">The inspector contains an embedded browser, together with a powerful HTML profiler that will show the selected code.</span></span> <span data-ttu-id="00c9b-309">注意してください、ページの外観を確認するソリューションを実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="00c9b-309">Notice that you do not have to run the solution to see how your pages look.</span></span>

    > [!NOTE]
    > <span data-ttu-id="00c9b-310">Page Inspector のブラウザーの幅が開かれているページの幅未満の場合は表示されませんページ適切です。</span><span class="sxs-lookup"><span data-stu-id="00c9b-310">When the width of Page Inspector browser is less than the width of the opened page, you will not see the page properly.</span></span> <span data-ttu-id="00c9b-311">このような場合は、Page Inspector の幅を調整します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-311">If that happens, adjust the width of the Page Inspector.</span></span>
4. <span data-ttu-id="00c9b-312">をクリックして、**ファイル**Page Inspector 内でのタブ。</span><span class="sxs-lookup"><span data-stu-id="00c9b-312">Click the **Files** tab in Page Inspector.</span></span>

    <span data-ttu-id="00c9b-313">表示された既定のページを作成するすべてのソース ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-313">You will see all the source files that are composing the rendered Default page.</span></span> <span data-ttu-id="00c9b-314">これは、ユーザー コントロールとマスター ページを使用している場合は特に、ひとめですべての要素を識別するために便利な機能です。</span><span class="sxs-lookup"><span data-stu-id="00c9b-314">This is a useful feature to identify all the elements at a glance, especially when you are working with User Controls and Master Pages.</span></span> <span data-ttu-id="00c9b-315">各ファイルに移動することも確認します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-315">Notice that you can also navigate to each of the files.</span></span>

    <span data-ttu-id="00c9b-316">![[ファイル] タブ](using-page-inspector-in-visual-studio-2012/_static/image26.png "[ファイル] タブ")</span><span class="sxs-lookup"><span data-stu-id="00c9b-316">![The Files tab](using-page-inspector-in-visual-studio-2012/_static/image26.png "The Files tab")</span></span>

    <span data-ttu-id="00c9b-317">*[ファイル] タブ*</span><span class="sxs-lookup"><span data-stu-id="00c9b-317">*The Files tab*</span></span>
5. <span data-ttu-id="00c9b-318">をクリックして、**検査モードの切り替え**タブの左側にあるボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-318">Click the **Toggle Inspection Mode** button, located at the left of the tabs.</span></span>

    <span data-ttu-id="00c9b-319">このツールを使用すると、ページの任意の要素を選択し、その HTML コードおよび .aspx ソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-319">This tool will let you select any element of the page and see its HTML code and .aspx source.</span></span>

    <span data-ttu-id="00c9b-320">![トグル検査モード ボタン](using-page-inspector-in-visual-studio-2012/_static/image27.png "検査モードの切り替えボタン")</span><span class="sxs-lookup"><span data-stu-id="00c9b-320">![Toggle Inspection Mode button](using-page-inspector-in-visual-studio-2012/_static/image27.png "Toggle Inspection Mode button")</span></span>

    <span data-ttu-id="00c9b-321">*トグル検査モード ボタン*</span><span class="sxs-lookup"><span data-stu-id="00c9b-321">*Toggle Inspection Mode button*</span></span>
6. <span data-ttu-id="00c9b-322">Page Inspector のブラウザーでは、ページ要素にマウスを移動します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-322">In the Page Inspector browser, move the mouse over the page elements.</span></span> <span data-ttu-id="00c9b-323">表示するページの任意の部分にマウス ポインターを移動するときに、要素の型が表示され、対応するソース マークアップまたはコードが Visual Studio エディターで強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-323">While you move the mouse pointer over any part of the rendered page, the element type is displayed and the corresponding source markup or code is highlighted in the Visual Studio editor.</span></span>

    <span data-ttu-id="00c9b-324">![アクションで検査モード](using-page-inspector-in-visual-studio-2012/_static/image28.png "アクションで検査モード")</span><span class="sxs-lookup"><span data-stu-id="00c9b-324">![Inspection mode in action](using-page-inspector-in-visual-studio-2012/_static/image28.png "Inspection mode in action")</span></span>

    <span data-ttu-id="00c9b-325">*検査モードの動作*</span><span class="sxs-lookup"><span data-stu-id="00c9b-325">*Inspection mode in action*</span></span>

    > [!NOTE]
    > <span data-ttu-id="00c9b-326">Page Inspector ウィンドウを最大化しないか、ソース コードを示す プレビュー タブを表示することはできません。</span><span class="sxs-lookup"><span data-stu-id="00c9b-326">Do not maximize the Page Inspector window or you will not be able to see the preview tab showing the source code.</span></span> <span data-ttu-id="00c9b-327">最大化されたときに、Page Inspector 内で要素をクリックすると、選択範囲のソース コードが表示されますが、Page Inspector ウィンドウを非表示になります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-327">If you click the element in Page Inspector when it is maximized, the source code of the selection will appear but it will hide the Page Inspector window.</span></span>

    <span data-ttu-id="00c9b-328">場合に注意を払う**Default.aspx**ファイルが表示されます、選択した要素を生成するソース コードの部分が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-328">If you pay attention to **Default.aspx** file, you will notice that the portion of source code that generates the selected element is highlighted.</span></span> <span data-ttu-id="00c9b-329">この機能には、長いソース ファイル、コードにアクセスする直接的および高速の方法を提供するのエディションが容易になります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-329">This feature facilitates the edition of long source files, providing a direct and fast way to access the code.</span></span>

    <span data-ttu-id="00c9b-330">![要素を検査](using-page-inspector-in-visual-studio-2012/_static/image29.png "要素の検査")</span><span class="sxs-lookup"><span data-stu-id="00c9b-330">![Inspecting elements](using-page-inspector-in-visual-studio-2012/_static/image29.png "Inspecting elements")</span></span>

    <span data-ttu-id="00c9b-331">*要素の検査*</span><span class="sxs-lookup"><span data-stu-id="00c9b-331">*Inspecting elements*</span></span>
7. <span data-ttu-id="00c9b-332">クリックして、**検査モードを切り替える**ボタン (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser です.")</span><span class="sxs-lookup"><span data-stu-id="00c9b-332">Click the **Toggle Inspection Mode** button (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.")</span></span> <span data-ttu-id="00c9b-333">) 、カーソルを無効にする、Page Inspector のタブにあります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-333">), located in Page Inspector tabs, to disable the cursor.</span></span>
8. <span data-ttu-id="00c9b-334">選択、 **HTML** Page Inspector のブラウザーでレンダリングされた HTML コードを表示するタブ。</span><span class="sxs-lookup"><span data-stu-id="00c9b-334">Select the **HTML** tab to display the HTML code rendered in the Page Inspector browser.</span></span>
9. <span data-ttu-id="00c9b-335">HTML のコードで Koala リンクのリスト アイテムを見つけて選択します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-335">In the HTML code, locate the list item with the Koala link and select it.</span></span>

    <span data-ttu-id="00c9b-336">コードを選択すると、対応する出力が自動的に強調表示されているブラウザーであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-336">Notice that when you select the code, the corresponding output is automatically highlighted browser.</span></span> <span data-ttu-id="00c9b-337">この機能は、HTML ブロックをページに表示する方法を確認するのに便利です。</span><span class="sxs-lookup"><span data-stu-id="00c9b-337">This feature is useful to see how an HTML block is rendered on the page.</span></span>

    <span data-ttu-id="00c9b-338">![HTML 要素の選択 ページで](using-page-inspector-in-visual-studio-2012/_static/image31.png "ページの HTML 要素の選択")</span><span class="sxs-lookup"><span data-stu-id="00c9b-338">![Selecting an HTML element in the page](using-page-inspector-in-visual-studio-2012/_static/image31.png "Selecting an HTML element in the page")</span></span>

    <span data-ttu-id="00c9b-339">*ページの HTML 要素の選択*</span><span class="sxs-lookup"><span data-stu-id="00c9b-339">*Selecting an HTML element in the page*</span></span>
10. <span data-ttu-id="00c9b-340">をクリックして、**検査モードの切り替え**有効にするボタン*検査モード*ナビゲーション バーをクリックします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-340">Click the **Toggle Inspection Mode** button to enable *Inspection Mode* and click the navigation bar.</span></span> <span data-ttu-id="00c9b-341">スタイルのウィンドウで、HTML コードの右側に、選択した要素に適用される CSS スタイルを使用して一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-341">On the right of the HTML code, in the Styles pane, you will see a list with the CSS styles applied to the selected element.</span></span>

    > [!NOTE]
    > <span data-ttu-id="00c9b-342">ヘッダーがサイトのレイアウトの一部であるため、Page Inspector も Site.Master ファイルを開き、影響を受けるコードのセグメントを強調表示します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-342">since the header is a part of the site layout, Page Inspector will also open Site.Master file and highlight the segment of code affected.</span></span>

    <span data-ttu-id="00c9b-343">![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "スタイルと、選択した要素のソース ファイルを検出します。")</span><span class="sxs-lookup"><span data-stu-id="00c9b-343">![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "Discovering styles and source files of a selected element")</span></span>

    <span data-ttu-id="00c9b-344">*スタイルと、選択した要素のソース ファイルを検出します。*</span><span class="sxs-lookup"><span data-stu-id="00c9b-344">*Discovering styles and source files of a selected element*</span></span>
11. <span data-ttu-id="00c9b-345">切り替え検査ポインターを有効になっている、メニュー バーの下にマウス ポインターを移動し、空白の半分の円をクリックします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-345">With the toggle inspection pointer enabled, move the mouse pointer below the menu bar and click the blank half circle.</span></span>

    <span data-ttu-id="00c9b-346">![要素の選択](using-page-inspector-in-visual-studio-2012/_static/image33.png "要素の選択")</span><span class="sxs-lookup"><span data-stu-id="00c9b-346">![Selecting an element](using-page-inspector-in-visual-studio-2012/_static/image33.png "Selecting an element")</span></span>

    <span data-ttu-id="00c9b-347">*要素の選択*</span><span class="sxs-lookup"><span data-stu-id="00c9b-347">*Selecting an element*</span></span>
12. <span data-ttu-id="00c9b-348">スタイルのウィンドウで検索、**背景イメージ**] の [、 **.main コンテンツ**グループ。</span><span class="sxs-lookup"><span data-stu-id="00c9b-348">In the Styles pane, locate the **background-image** item under the **.main-content** group.</span></span> <span data-ttu-id="00c9b-349">**オフに**、**背景イメージ**みてください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-349">**Uncheck** the **background-image** and see what happens.</span></span> <span data-ttu-id="00c9b-350">ブラウザーで、変更がすぐに反映されますを非表示には、円が表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-350">You will notice that the browser will reflect the changes immediately and the circle is hidden.</span></span>

    > [!NOTE]
    > <span data-ttu-id="00c9b-351">ページのインスペクターのスタイル タブに適用する変更は、元のスタイル シートには影響しません。</span><span class="sxs-lookup"><span data-stu-id="00c9b-351">The changes you apply on the Page Inspector Styles tab do not affect the original stylesheet.</span></span> <span data-ttu-id="00c9b-352">スタイルをオフにしたり、何度でも、ページの更新後に復元されますが、値を変更できます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-352">You can uncheck styles or change their values as many times as you want, but they will be restored after refreshing the page.</span></span>

    <span data-ttu-id="00c9b-353">![有効にして、CSS styles2 を無効化](using-page-inspector-in-visual-studio-2012/_static/image34.png "の有効化と CSS スタイルを無効にします。")</span><span class="sxs-lookup"><span data-stu-id="00c9b-353">![Enabling and disabling CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "Enabling and disabling CSS styles")</span></span>

    <span data-ttu-id="00c9b-354">*有効にして、CSS スタイルを無効化*</span><span class="sxs-lookup"><span data-stu-id="00c9b-354">*Enabling and disabling CSS styles*</span></span>
13. <span data-ttu-id="00c9b-355">をクリックして、'**、** **ロゴ '** 検査モードを使用して、ヘッダーのテキスト。</span><span class="sxs-lookup"><span data-stu-id="00c9b-355">Now, click the '**your** **logo here'** text on the header using the inspection mode.</span></span>
14. <span data-ttu-id="00c9b-356">**スタイル** タブで、検索、**フォント サイズ**CSS 属性、 **.site タイトル**グループ。</span><span class="sxs-lookup"><span data-stu-id="00c9b-356">In the **Styles** tab, locate the **font-size** CSS attribute under the **.site-title** group.</span></span> <span data-ttu-id="00c9b-357">その値を編集するには、1 回の属性をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-357">Double-click the attribute once to edit its value.</span></span> <span data-ttu-id="00c9b-358">値を置換、2.3em **3em**、し、ENTER キーを押します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-358">Replace the 2.3em value with **3em**, and then press ENTER.</span></span> <span data-ttu-id="00c9b-359">タイトルが大きいことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-359">Notice that the title looks bigger.</span></span>

    <span data-ttu-id="00c9b-360">![ページ Inspector2 CSS 値を変更する](using-page-inspector-in-visual-studio-2012/_static/image35.png "Page Inspector 内で値を変更する CSS")</span><span class="sxs-lookup"><span data-stu-id="00c9b-360">![Changing CSS values in the Page Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "Changing CSS values in the Page Inspector")</span></span>

    <span data-ttu-id="00c9b-361">*Page Inspector 内での CSS 値を変更します。*</span><span class="sxs-lookup"><span data-stu-id="00c9b-361">*Changing CSS values in the Page Inspector*</span></span>
15. <span data-ttu-id="00c9b-362">をクリックして、**トレース スタイル** タブで、Page Inspector の右側のウィンドウにあります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-362">Click the **Trace Styles** tab, located in the right pane of Page Inspector.</span></span> <span data-ttu-id="00c9b-363">これは、属性の名前順で、選択範囲に適用されるすべてのスタイルを表示する別の方法です。</span><span class="sxs-lookup"><span data-stu-id="00c9b-363">This is an alternative way to see all the styles applied to the selection, ordered by attribute name.</span></span>

    <span data-ttu-id="00c9b-364">![選択した要素の CSS スタイルのトレース](using-page-inspector-in-visual-studio-2012/_static/image36.png "選択した要素の CSS スタイルのトレース")</span><span class="sxs-lookup"><span data-stu-id="00c9b-364">![CSS styles tracing of the selected element](using-page-inspector-in-visual-studio-2012/_static/image36.png "CSS styles tracing of the selected element")</span></span>

    <span data-ttu-id="00c9b-365">*選択した要素の CSS スタイルのトレース*</span><span class="sxs-lookup"><span data-stu-id="00c9b-365">*CSS styles tracing of the selected element*</span></span>
16. <span data-ttu-id="00c9b-366">Page Inspector のもう 1 つの機能は、レイアウト ペインです。</span><span class="sxs-lookup"><span data-stu-id="00c9b-366">Another feature of Page Inspector is the Layout pane.</span></span> <span data-ttu-id="00c9b-367">検査モードを使用して、ナビゲーション バーを選択し、クリックして、**レイアウト**右側のウィンドウ タブ。</span><span class="sxs-lookup"><span data-stu-id="00c9b-367">Using the inspection mode, select the navigation bar and then click the **Layout** tab on the right pane.</span></span> <span data-ttu-id="00c9b-368">選択した要素の正確なサイズと、オフセット、余白、パディングおよび境界線のサイズが表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-368">You will see the exact size of the selected element, as well as its offset, margin, padding and border size.</span></span> <span data-ttu-id="00c9b-369">このビューから値を変更することも注目してください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-369">Notice that you can also modify the values from this view.</span></span>

    <span data-ttu-id="00c9b-370">![Page Inspector 内で要素のレイアウト](using-page-inspector-in-visual-studio-2012/_static/image37.png "Page Inspector 内で要素のレイアウト")</span><span class="sxs-lookup"><span data-stu-id="00c9b-370">![Element layout in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image37.png "Element layout in Page Inspector")</span></span>

    <span data-ttu-id="00c9b-371">*Page Inspector 内で要素のレイアウト*</span><span class="sxs-lookup"><span data-stu-id="00c9b-371">*Element layout in Page Inspector*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a><span data-ttu-id="00c9b-372">タスク 2 - 検索して、フォト ギャラリーでスタイルの問題を修正</span><span class="sxs-lookup"><span data-stu-id="00c9b-372">Task 2 - Finding and Fixing Style Issues in the Photo Gallery</span></span>

<span data-ttu-id="00c9b-373">Visual Studio の以前のバージョンとの Web ページの問題を診断するにはでしょうか。</span><span class="sxs-lookup"><span data-stu-id="00c9b-373">How would you diagnose Web pages issues with previous versions of Visual Studio?</span></span> <span data-ttu-id="00c9b-374">なじみの web デバッグ ツール、Internet Explorer 開発者ツールや Firebug など、Visual Studio IDE の外部で実行するとしています。</span><span class="sxs-lookup"><span data-stu-id="00c9b-374">You are likely familiar with web debugging tools that run outside the Visual Studio IDE, like Internet Explorer Developer Tools or Firebug.</span></span> <span data-ttu-id="00c9b-375">ブラウザーでのみ認識、HTML スクリプトとスタイル中、基になるフレームワークには、レンダリングされる HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-375">Browsers only understand HTML, scripting and styles, while an underlying framework generates the HTML that will be rendered.</span></span> <span data-ttu-id="00c9b-376">そのため、多くの場合、サイト全体のように web ページの外観を確認するを展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-376">For that reason, you often need to deploy the whole site to see how web pages look like.</span></span>

<span data-ttu-id="00c9b-377">検出し、web サイトの問題を修正しました。 必要な場合に、次の手順が後にいた可能性があります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-377">You had probably followed these steps when you wanted to detect and fix an issue in your web site:</span></span>

1. <span data-ttu-id="00c9b-378">Visual studio でソリューションを実行または web サーバー上のページをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-378">Run the Solution from Visual Studio, or deploy the page on the web server.</span></span>
2. <span data-ttu-id="00c9b-379">ブラウザーで開発者ツールを使用または単にソース コードを開き、スタイルしようとする、問題の一致を開きます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-379">In the browser, open the developer tools you use, or simply open the source code and the styles and try to match the issue.</span></span> <span data-ttu-id="00c9b-380">使用を関連するファイルを検索する、&quot;検索&quot;または&quot;ファイルで検索&quot;スタイル クラスの名前で機能します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-380">To find the files involved, you'd have used the &quot;Search&quot; or &quot;Search in files&quot; features with the name of the style classes.</span></span>
3. <span data-ttu-id="00c9b-381">エラーが検出されると、Web ブラウザーとサーバーを停止します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-381">Once the error is detected, stop the Web browser and the server.</span></span>
4. <span data-ttu-id="00c9b-382">ブラウザーのキャッシュを消去します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-382">Clear the browser cache.</span></span>
5. <span data-ttu-id="00c9b-383">修正を適用する Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="00c9b-383">Return to Visual Studio to apply a fix.</span></span> <span data-ttu-id="00c9b-384">テストするすべての手順を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-384">Repeat all the steps to test.</span></span>

<span data-ttu-id="00c9b-385">いいえ、ASP.NET WebForms で実際に WYSIWYG が、いくつかのスタイルの問題が後の段階で実行中または展開後に検出されました。</span><span class="sxs-lookup"><span data-stu-id="00c9b-385">As there is no real WYSIWYG in ASP.NET WebForms, some style issues are detected on a later stage, after running or deploying.</span></span> <span data-ttu-id="00c9b-386">ここで Page Inspector、ソリューションを実行することがなく任意のページをプレビューできることです。</span><span class="sxs-lookup"><span data-stu-id="00c9b-386">Now, with Page Inspector, it is possible to preview any page without running the solution.</span></span>

<span data-ttu-id="00c9b-387">このタスクでは、フォト ギャラリーのアプリケーションのいくつかの問題を修正するために、Page inspector を使用します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-387">In this task, you will use the Page inspector for fixing some issues of the Photo Gallery application.</span></span> <span data-ttu-id="00c9b-388">次の手順では、検出し、ヘッダーに何らかの単純なスタイルの問題を迅速に修正します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-388">In the following steps, you will detect and quickly fix some simple styling issue in the header.</span></span>

1. <span data-ttu-id="00c9b-389">検索ページの検査を使用して、**登録**と**ログで**ヘッダーの左側にあるリンクです。</span><span class="sxs-lookup"><span data-stu-id="00c9b-389">Using Page Inspection, locate the **Register** and the **Log In** links at the left side of the header.</span></span>

    <span data-ttu-id="00c9b-390">右側の期待どおりの場所で、リンクが表示されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-390">Notice that the link is not displayed at the expected place on the right.</span></span> <span data-ttu-id="00c9b-391">それに応じてスタイルを変更して、右へのリンクを整列するされます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-391">You will now align the link to the right and restyle it accordingly.</span></span>

    <span data-ttu-id="00c9b-392">![左側に配置されているリンク ログイン](using-page-inspector-in-visual-studio-2012/_static/image38.png "左側に配置されているリンクにログイン")</span><span class="sxs-lookup"><span data-stu-id="00c9b-392">![Log in link positioned on the left](using-page-inspector-in-visual-studio-2012/_static/image38.png "Log in link positioned on the left")</span></span>

    <span data-ttu-id="00c9b-393">*左側に配置されているログのリンク*</span><span class="sxs-lookup"><span data-stu-id="00c9b-393">*Log In link positioned on the left*</span></span>
2. <span data-ttu-id="00c9b-394">選択されている検査モードの切り替え、そのコードを開くためのログでリンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-394">With Toggle Inspection Mode selected, select the Log In link to open its code.</span></span>

    <span data-ttu-id="00c9b-395">リンクのソース コードに配置されている通知、 **Site.Master**最初に探すことになる場所ですが、Default.aspx ページでは適切なソース ファイルで直接に配置したせずファイル。</span><span class="sxs-lookup"><span data-stu-id="00c9b-395">Notice that the link source code is located in the **Site.Master** file, not in the Default.aspx page which is the place you might look in first place; you have been placed directly in the correct source file.</span></span>
3. <span data-ttu-id="00c9b-396">**スタイル** タブを見つけて、クリックして、 **&lt;セクション&gt;#login**項目で、これらのリンクの HTML コンテナーです。</span><span class="sxs-lookup"><span data-stu-id="00c9b-396">In the **Styles** tab, locate and click the **&lt;section&gt; #login** item, which is the HTML container for these links.</span></span>

    <span data-ttu-id="00c9b-397">いることを確認、 **#login**スタイルが自動的に**Site.css**  をクリック後します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-397">Notice that the **#login** style is automatically located in **Site.css** after you click.</span></span> <span data-ttu-id="00c9b-398">さらに、コードは強調表示されたようになりました。</span><span class="sxs-lookup"><span data-stu-id="00c9b-398">Moreover, the code is now highlighted.</span></span>

    <span data-ttu-id="00c9b-399">![CSS スタイルの選択](using-page-inspector-in-visual-studio-2012/_static/image39.png "CSS スタイルの選択")</span><span class="sxs-lookup"><span data-stu-id="00c9b-399">![Selecting the CSS styles](using-page-inspector-in-visual-studio-2012/_static/image39.png "Selecting the CSS styles")</span></span>

    <span data-ttu-id="00c9b-400">*CSS スタイルの選択*</span><span class="sxs-lookup"><span data-stu-id="00c9b-400">*Selecting the CSS styles*</span></span>
4. <span data-ttu-id="00c9b-401">コメントを解除、**テキスト配置**属性の開始タグと終了文字を削除することで強調表示されたコードと保存、 **Site.css**ファイル。</span><span class="sxs-lookup"><span data-stu-id="00c9b-401">Uncomment the **text-align** attribute in the highlighted code by removing the opening and closing characters and save the **Site.css** file.</span></span>

    <span data-ttu-id="00c9b-402">Page Inspector は、現在のページを構成するすべてのファイルを認識し、これらのファイルのいずれかが変更を検出できます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-402">Page Inspector is aware of all the different files that compose the current page, and it can detect when any of these files change.</span></span> <span data-ttu-id="00c9b-403">ブラウザーで現在のページがソース ファイルと同期されるたびに、警告されます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-403">It alerts you whenever the current page in browser is not in sync with the source files.</span></span>
5. <span data-ttu-id="00c9b-404">Page Inspector のブラウザーでは、変更を保存し、ページを再読み込みするには、アドレス バーの下にあるバーをクリックします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-404">In the Page Inspector browser, click the bar located below the address bar to save the changes and reload the page.</span></span>

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    <span data-ttu-id="00c9b-406">*ページの再読み込み*</span><span class="sxs-lookup"><span data-stu-id="00c9b-406">*Reloading the page*</span></span>

    <span data-ttu-id="00c9b-407">リンクは、右側にある、ようになりましたも箇条書きリストのように見えます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-407">The links are now at the right, but they still look like a bulleted list.</span></span> <span data-ttu-id="00c9b-408">次に、行頭文字を削除し、リンクを左右に配置します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-408">Now, you will remove the bullets and align the links horizontally.</span></span>

    ![更新されたページ](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    <span data-ttu-id="00c9b-410">*更新されたページ*</span><span class="sxs-lookup"><span data-stu-id="00c9b-410">*Updated page*</span></span>
6. <span data-ttu-id="00c9b-411">いずれかを選択して、検査モードを使用して、 **&lt;li&gt;** 項目が含まれている、&quot;登録&quot;と&quot;ログイン&quot;リンク。</span><span class="sxs-lookup"><span data-stu-id="00c9b-411">Using the inspection mode, select any of the **&lt;li&gt;** items that contain the &quot;Register&quot; and &quot;Log in&quot; links.</span></span> <span data-ttu-id="00c9b-412">をクリックし、 **&lt;セクション&gt;#login**項目へのアクセスを**Styles.css**コード。</span><span class="sxs-lookup"><span data-stu-id="00c9b-412">Then, click the **&lt;section&gt; #login** item to access **Styles.css** code.</span></span>

    <span data-ttu-id="00c9b-413">![スタイルの検索](using-page-inspector-in-visual-studio-2012/_static/image42.png "のスタイルの検索")</span><span class="sxs-lookup"><span data-stu-id="00c9b-413">![Finding the style](using-page-inspector-in-visual-studio-2012/_static/image42.png "Finding the style")</span></span>

    <span data-ttu-id="00c9b-414">*スタイルの検索*</span><span class="sxs-lookup"><span data-stu-id="00c9b-414">*Finding the style*</span></span>
7. <span data-ttu-id="00c9b-415">**Style.css**、コードをコメント解除します **#login li**項目。</span><span class="sxs-lookup"><span data-stu-id="00c9b-415">In **Style.css**, uncomment the code for **#login li** items.</span></span> <span data-ttu-id="00c9b-416">追加するスタイルは行頭文字を非表示にして、水平方向に項目を表示します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-416">The style you are adding will hide the bullet and display the items horizontally.</span></span>

    <span data-ttu-id="00c9b-417">![ログインのリンク スタイルの変更](using-page-inspector-in-visual-studio-2012/_static/image43.png "ログインのリンク スタイルの変更")</span><span class="sxs-lookup"><span data-stu-id="00c9b-417">![Restyling the login links](using-page-inspector-in-visual-studio-2012/_static/image43.png "Restyling the login links")</span></span>

    <span data-ttu-id="00c9b-418">*ログインのリンク スタイルの変更*</span><span class="sxs-lookup"><span data-stu-id="00c9b-418">*Restyling the login links*</span></span>
8. <span data-ttu-id="00c9b-419">保存**Style.css**ファイルを開き、ページを再読み込みするアドレスの下にあるバーを 1 回クリックします。</span><span class="sxs-lookup"><span data-stu-id="00c9b-419">Save **Style.css** file and click once on the bar located below the address to reload the page.</span></span> <span data-ttu-id="00c9b-420">リンクが正しく表示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="00c9b-420">Notice that the links appear correctly.</span></span>

    <span data-ttu-id="00c9b-421">![リンクが右側に揃えて配置](using-page-inspector-in-visual-studio-2012/_static/image44.png "へのリンクが右側に配置")</span><span class="sxs-lookup"><span data-stu-id="00c9b-421">![Links aligned to the right side](using-page-inspector-in-visual-studio-2012/_static/image44.png "Links aligned to the right side")</span></span>

    <span data-ttu-id="00c9b-422">*右揃えで配置へのリンク*</span><span class="sxs-lookup"><span data-stu-id="00c9b-422">*Links aligned to the right side*</span></span>
9. <span data-ttu-id="00c9b-423">最後に、ヘッダーのタイトルを変更します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-423">Finally, you will change the header title.</span></span> <span data-ttu-id="00c9b-424">検索する代わりに、'**貴社のロゴ '** すべてのファイル内のテキストでは、検査モードを使用してテキストをクリックし、生成されたソース コードを取得します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-424">Instead of searching for the '**your logo here'** text in all the files, use the inspection mode to click the text and get to source code that generates it.</span></span>

    <span data-ttu-id="00c9b-425">![サイトのタイトルを検索](using-page-inspector-in-visual-studio-2012/_static/image45.png "サイトのタイトルの検索")</span><span class="sxs-lookup"><span data-stu-id="00c9b-425">![Finding the site title](using-page-inspector-in-visual-studio-2012/_static/image45.png "Finding the site title")</span></span>

    <span data-ttu-id="00c9b-426">*サイトのタイトルの検索*</span><span class="sxs-lookup"><span data-stu-id="00c9b-426">*Finding the site title*</span></span>
10. <span data-ttu-id="00c9b-427">使用するようになりました**Site.Master**、置換、'**貴社のロゴ**'テキスト'**フォト ギャラリー**'。</span><span class="sxs-lookup"><span data-stu-id="00c9b-427">Now you are in **Site.Master**, replace the '**your logo here**' text with '**Photo Gallery**'.</span></span> <span data-ttu-id="00c9b-428">保存して Page Inspector のブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-428">Save and update the Page Inspector browser.</span></span>

    <span data-ttu-id="00c9b-429">![フォト ギャラリーのページ更新](using-page-inspector-in-visual-studio-2012/_static/image46.png "フォト ギャラリー ページの更新")</span><span class="sxs-lookup"><span data-stu-id="00c9b-429">![Photo Gallery page updated](using-page-inspector-in-visual-studio-2012/_static/image46.png "Photo Gallery page updated")</span></span>

    <span data-ttu-id="00c9b-430">*フォト ギャラリー ページの更新*</span><span class="sxs-lookup"><span data-stu-id="00c9b-430">*Photo Gallery page updated*</span></span>
11. <span data-ttu-id="00c9b-431">最後にキーを押して**F5**アプリに期待どおりに変更をすべてチェックを実行します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-431">Finally press **F5** to run the app the check out all the changes work as expected.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="00c9b-432">まとめ</span><span class="sxs-lookup"><span data-stu-id="00c9b-432">Summary</span></span>

<span data-ttu-id="00c9b-433">このハンズオン ラボを完了するが、Page Inspector を使用して、再構築し、ブラウザーで Web サイトを実行することがなく、Web アプリケーションをプレビューする方法を習得します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-433">By completing this Hands-On Lab, you have learnt how to use Page Inspector to preview your Web application without having to rebuild and run the Web site in a browser.</span></span> <span data-ttu-id="00c9b-434">さらに、すばやく発見し、ソース コードに表示される出力から直接アクセスすることでバグを修正する方法を習得しました。</span><span class="sxs-lookup"><span data-stu-id="00c9b-434">In addition, you have learnt how to quickly find and fix bugs by accessing directly from the rendered output to the source code.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="00c9b-435">付録 a: Installing Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="00c9b-435">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="00c9b-436">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="00c9b-436">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="00c9b-437">次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-437">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="00c9b-438">移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-438">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="00c9b-439">または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-439">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="00c9b-440">をクリックして**を今すぐインストール**します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-440">Click on **Install Now**.</span></span> <span data-ttu-id="00c9b-441">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-441">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="00c9b-442">1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-442">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="00c9b-443">![Visual Studio Express のインストール](using-page-inspector-in-visual-studio-2012/_static/image47.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="00c9b-443">![Install Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="00c9b-444">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="00c9b-444">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="00c9b-445">すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-445">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    <span data-ttu-id="00c9b-447">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="00c9b-447">*Accepting the license terms*</span></span>
5. <span data-ttu-id="00c9b-448">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-448">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    <span data-ttu-id="00c9b-450">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="00c9b-450">*Installation progress*</span></span>
6. <span data-ttu-id="00c9b-451">インストールが完了したら、クリックして**完了**します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-451">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    <span data-ttu-id="00c9b-453">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="00c9b-453">*Installation completed*</span></span>
7. <span data-ttu-id="00c9b-454">クリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="00c9b-454">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="00c9b-455">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="00c9b-455">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web のタイル](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    <span data-ttu-id="00c9b-457">*VS Express for Web のタイル*</span><span class="sxs-lookup"><span data-stu-id="00c9b-457">*VS Express for Web tile*</span></span>
