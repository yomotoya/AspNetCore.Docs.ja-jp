---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: "Visual Studio 2012 で Page Inspector を使用して |Microsoft ドキュメント"
author: rick-anderson
description: "このハンズオン ラボでは、新しいツールを見つけて Visual Studio - Page Inspector で web ページの問題を修正することがわかります。 Page Inspector は、新しいツールその b."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 1a9e093faae2cea1c27c582e22aebc908f78addb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-page-inspector-in-visual-studio-2012"></a><span data-ttu-id="6489f-104">Visual Studio 2012 での Page Inspector の使用</span><span class="sxs-lookup"><span data-stu-id="6489f-104">Using Page Inspector in Visual Studio 2012</span></span>
====================
<span data-ttu-id="6489f-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="6489f-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="6489f-106">このハンズオン ラボでは、新しいツールを見つけて Visual Studio - Page Inspector で web ページの問題を修正することがわかります。</span><span class="sxs-lookup"><span data-stu-id="6489f-106">In this Hands-on Lab, you will discover a new tool to find and fix web page issues in Visual Studio - the Page Inspector.</span></span>
> 
> <span data-ttu-id="6489f-107">Page Inspector は、ブラウザー診断ツールを Visual Studio に表示し、ブラウザー、ASP.NET、およびソース コードの間で統合されたエクスペリエンスを提供する新しいツールです。</span><span class="sxs-lookup"><span data-stu-id="6489f-107">Page Inspector is a new tool that brings browser diagnostics tools to Visual Studio and provides an integrated experience among the browser, ASP.NET, and source code.</span></span> <span data-ttu-id="6489f-108">Visual Studio IDE 内で直接、web ページ (HTML、Web フォーム、ASP.NET MVC または Web ページ) を表示し、ソース コードと、結果の出力の両方を調べることができます。</span><span class="sxs-lookup"><span data-stu-id="6489f-108">It renders a web page (HTML, Web Forms, ASP.NET MVC, or Web Pages) directly within the Visual Studio IDE and lets you examine both the source code and the resulting output.</span></span> <span data-ttu-id="6489f-109">Page Inspector を使用すると、web サイトを簡単に展開し、迅速に、ゼロからページを構築および問題を診断できます。</span><span class="sxs-lookup"><span data-stu-id="6489f-109">Page Inspector enables you to easily decompose a website, rapidly build pages from the ground up, and quickly diagnose issues.</span></span>
> 
> <span data-ttu-id="6489f-110">最近では ASP.NET MVC と WebForms などの適切なタイミングで柔軟でスケーラブルな web サイトを作成する Web フレームワークの数があります。</span><span class="sxs-lookup"><span data-stu-id="6489f-110">Nowadays we have a number of Web frameworks that create flexible and scalable websites in a timely manner, such as ASP.NET MVC and WebForms.</span></span> <span data-ttu-id="6489f-111">その一方で、取得が困難になります IDE がテンプレートに基づくページや動的コンテンツでデザイナーのビューをサポートしていないため、ページ上の問題を検索します。</span><span class="sxs-lookup"><span data-stu-id="6489f-111">On the other hand, it gets harder to find issues on the pages because the IDE does not support the designer view in template-based pages and dynamic content.</span></span> <span data-ttu-id="6489f-112">したがって、これらの web サイトをユーザーに表示される方法を参照するブラウザーで開く必要です。</span><span class="sxs-lookup"><span data-stu-id="6489f-112">Therefore, these websites have to be opened in a browser to see how they appear to a user.</span></span>
> 
> <span data-ttu-id="6489f-113">Web 開発者は、外部ツールを使用して、ブラウザーで定期的に実行している問題を検出します。</span><span class="sxs-lookup"><span data-stu-id="6489f-113">Web developers use external tools to find issues that regularly run in the browser.</span></span> <span data-ttu-id="6489f-114">次に、IDE に戻る、修復を開始します。</span><span class="sxs-lookup"><span data-stu-id="6489f-114">Then, they return to the IDE and start fixing.</span></span> <span data-ttu-id="6489f-115">これは、前後 IDE、ブラウザー、およびプロファイリング ツールの間のアクティビティ、効率的なことができ、新規に展開とキャッシュの問題を再現するたびに、クリーニングが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="6489f-115">This back and forth activity among the IDE, the browser and the profiling tools can be inefficient, and sometimes requires a fresh deployment and cache cleaning each time you want to reproduce an issue.</span></span>
> 
> <span data-ttu-id="6489f-116">Page Inspector は、の機能の結合セットを使用して両方の長所をまとめることによってクライアント (ブラウザー ツール) と、サーバー (ASP.NET およびソース コード) の間での Web 開発のギャップを結びます。</span><span class="sxs-lookup"><span data-stu-id="6489f-116">Page Inspector bridges a gap in Web development between the client (browser tools) and the server (ASP.NET and source code) by bringing together the best of both worlds using a combined set of features.</span></span>
> 
> <span data-ttu-id="6489f-117">Page Inspector を使用して確認できます (サーバー側のコードを含む)、ソース ファイルの要素がブラウザーにレンダリングされる HTML マークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="6489f-117">Using Page Inspector, you can see which elements in the source files (including server-side code) have produced the HTML markup to be rendered in the browser.</span></span> <span data-ttu-id="6489f-118">Page Inspector を使用して、CSS のプロパティと、ブラウザーで直ちに反映された変更を表示する DOM 要素の属性を変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="6489f-118">Page Inspector also lets you modify CSS properties and DOM element attributes to see the changes reflected immediately in the browser.</span></span>
> 
> <span data-ttu-id="6489f-119">このハンズオン ラボは、Page Inspector の機能について説明して、Web アプリケーションで問題の解決を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="6489f-119">This hands-on lab will walk you through the Page Inspector features and show you how you can use them to fix issues in Web applications.</span></span> <span data-ttu-id="6489f-120">**このラボには、類似のフローを使用して、さまざまなテクノロジを対象とする 2 つの手順が含まれています。ASP.NET MVC 開発者の場合は、次の手順のいずれかです。2 WebForms 開発者フォロー練習をする場合は**します。</span><span class="sxs-lookup"><span data-stu-id="6489f-120">**This lab contains two exercises using similar flows but targeting different technologies. If you are an ASP.NET MVC Developer, follow exercise one; if you are a WebForms developer follow exercise two**.</span></span>
> 
> <span data-ttu-id="6489f-121">このラボには、機能強化と軽微な変更を元のフォルダーで提供されるサンプル Web アプリケーションに適用することで前に説明した新しい機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="6489f-121">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="6489f-122">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)です。</span><span class="sxs-lookup"><span data-stu-id="6489f-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="6489f-123">目的</span><span class="sxs-lookup"><span data-stu-id="6489f-123">Objectives</span></span>

<span data-ttu-id="6489f-124">このハンズオン ラボでは学習する方法。</span><span class="sxs-lookup"><span data-stu-id="6489f-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="6489f-125">Page Inspector を使用して Web サイトを分解します。</span><span class="sxs-lookup"><span data-stu-id="6489f-125">Decompose a Web Site using Page Inspector</span></span>
- <span data-ttu-id="6489f-126">検査し、Page Inspector で CSS スタイルの変更のプレビュー</span><span class="sxs-lookup"><span data-stu-id="6489f-126">Inspect and preview CSS styles changes with Page Inspector</span></span>
- <span data-ttu-id="6489f-127">検出して Page Inspector を使用して、web ページでの問題を修正します。</span><span class="sxs-lookup"><span data-stu-id="6489f-127">Detect and fix issues in your web pages using Page Inspector</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="6489f-128">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="6489f-128">Prerequisites</span></span>

<span data-ttu-id="6489f-129">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="6489f-129">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="6489f-130">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="6489f-130">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="6489f-131">Internet Explorer 9 以降</span><span class="sxs-lookup"><span data-stu-id="6489f-131">Internet Explorer 9 or higher</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="6489f-132">演習</span><span class="sxs-lookup"><span data-stu-id="6489f-132">Exercises</span></span>

<span data-ttu-id="6489f-133">このハンズオン ラボには、次の実習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="6489f-133">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="6489f-134">手順 1: ASP.NET MVC プロジェクトで Page Inspector を使用します。</span><span class="sxs-lookup"><span data-stu-id="6489f-134">Exercise 1: Using Page Inspector in ASP.NET MVC Projects</span></span>](#Exercise1)
2. [<span data-ttu-id="6489f-135">手順 2: WebForms プロジェクトで Page Inspector を使用します。</span><span class="sxs-lookup"><span data-stu-id="6489f-135">Exercise 2: Using Page Inspector in WebForms Projects</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="6489f-136">各手順は、他のユーザーとは無関係に各手順に従うことができるようにすると、作業の開始フォルダーにある、開始のソリューションを伴います。</span><span class="sxs-lookup"><span data-stu-id="6489f-136">Each exercise is accompanied by a starting solution, located in the Begin folder of the exercise, that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="6489f-137">演習では、ソース コード、内部は、対応する手順」の手順を完了して得た結果コードと Visual Studio ソリューションを含む終了フォルダーもがあります。</span><span class="sxs-lookup"><span data-stu-id="6489f-137">Inside the source code for an exercise, you will also find an End folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="6489f-138">このハンズオン ラボを使用すると、追加のヘルプを必要がある場合は、これらのソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="6489f-138">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


<span data-ttu-id="6489f-139">この演習を完了する時間を推定: **30 分**です。</span><span class="sxs-lookup"><span data-stu-id="6489f-139">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a><span data-ttu-id="6489f-140">手順 1: ASP.NET MVC プロジェクトで Page Inspector を使用します。</span><span class="sxs-lookup"><span data-stu-id="6489f-140">Exercise 1: Using Page Inspector in ASP.NET MVC Projects</span></span>

<span data-ttu-id="6489f-141">この演習では、プレビューをデバッグする方法を学習します、 **ASP.NET MVC 4**ソリューションを使用して**Page Inspector**です。</span><span class="sxs-lookup"><span data-stu-id="6489f-141">In this exercise, you will learn how to preview and debug an **ASP.NET MVC 4** solution using **Page Inspector**.</span></span> <span data-ttu-id="6489f-142">最初に、プロセスのデバッグ、Web を促進する機能を説明するツールの簡単な膝を実行します。</span><span class="sxs-lookup"><span data-stu-id="6489f-142">First, you will perform a brief lap around the tool to learn the features that facilitate the Web debugging process.</span></span> <span data-ttu-id="6489f-143">次に、スタイルの問題を含む web ページで作業します。</span><span class="sxs-lookup"><span data-stu-id="6489f-143">Then, you will work in a web page that contains styling issues.</span></span> <span data-ttu-id="6489f-144">Page Inspector を使用して、問題を生成するソース コードを検索し、その修正方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="6489f-144">You will learn how to use Page Inspector to find the source code that generates the issue and fix it.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a><span data-ttu-id="6489f-145">タスク 1 - Page Inspector の探索</span><span class="sxs-lookup"><span data-stu-id="6489f-145">Task 1 - Exploring Page Inspector</span></span>

<span data-ttu-id="6489f-146">このタスクでは、フォト ギャラリーを表示する ASP.NET MVC 4 プロジェクトのコンテキストで Page Inspector を使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="6489f-146">In this task, you will learn how to use the Page Inspector in the context of an ASP.NET MVC 4 project that shows a photo gallery.</span></span>

1. <span data-ttu-id="6489f-147">開く、**開始**ソリューションにある**ソース/Ex1-MVC4/開始/**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="6489f-147">Open the **Begin** solution located at **Source/Ex1-MVC4/Begin/** folder.</span></span>

    1. <span data-ttu-id="6489f-148">いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行前にします。</span><span class="sxs-lookup"><span data-stu-id="6489f-148">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6489f-149">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="6489f-149">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="6489f-150">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="6489f-150">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="6489f-151">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="6489f-151">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6489f-152">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="6489f-152">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6489f-153">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="6489f-153">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6489f-154">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="6489f-154">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="6489f-155">ソリューション エクスプ ローラーで**Index.cshtml**下を表示、 **/ビュー/ホーム**プロジェクト フォルダーを右クリックして、選択**Page Inspector で表示**です。</span><span class="sxs-lookup"><span data-stu-id="6489f-155">In the Solution Explorer, locate **Index.cshtml** view under the **/Views/Home** project folder, right-click it and select **View in Page Inspector**.</span></span>

    <span data-ttu-id="6489f-156">![Page Inspector 内でプレビューするファイルを選択する](using-page-inspector-in-visual-studio-2012/_static/image1.png "Page Inspector 内でプレビューするファイルを選択します。")</span><span class="sxs-lookup"><span data-stu-id="6489f-156">![Selecting a file to preview in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image1.png "Selecting a file to preview in Page Inspector")</span></span>

    <span data-ttu-id="6489f-157">*Page Inspector 内でプレビューするファイルを選択します。*</span><span class="sxs-lookup"><span data-stu-id="6489f-157">*Selecting a file to preview in Page Inspector*</span></span>
3. <span data-ttu-id="6489f-158">Page Inspector ウィンドウに表示されます、*ホーム/インデックス*URL ソースを選択したビューにマップします。</span><span class="sxs-lookup"><span data-stu-id="6489f-158">The Page Inspector window will show the */Home/Index* URL mapped to the source View you selected.</span></span>

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    <span data-ttu-id="6489f-160">*Page Inspector で最初にお問い合わせください。*</span><span class="sxs-lookup"><span data-stu-id="6489f-160">*The first contact with Page Inspector*</span></span>

    <span data-ttu-id="6489f-161">Page Inspector ツールは、Visual Studio 環境内に統合されています。</span><span class="sxs-lookup"><span data-stu-id="6489f-161">The Page Inspector tool is integrated in your Visual Studio environment.</span></span> <span data-ttu-id="6489f-162">インスペクターには、強力な HTML プロファイラーと共にの組み込みブラウザーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6489f-162">The inspector contains an embedded browser, together with a powerful HTML profiler.</span></span> <span data-ttu-id="6489f-163">注意してください、ページの外観を確認するようにソリューションを実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="6489f-163">Notice that you do not have to run the solution to see how your pages look.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6489f-164">Page Inspector のブラウザーの幅が開かれるページの幅よりも低い場合は表示されませんページ適切です。</span><span class="sxs-lookup"><span data-stu-id="6489f-164">When the width of Page Inspector browser is less than the width of the opened page, you will not see the page properly.</span></span> <span data-ttu-id="6489f-165">その場合は、Page Inspector の幅を調整します。</span><span class="sxs-lookup"><span data-stu-id="6489f-165">If that happens, adjust the width of the Page Inspector.</span></span>
4. <span data-ttu-id="6489f-166">クリックして、**ファイル**Page Inspector 内でのタブです。</span><span class="sxs-lookup"><span data-stu-id="6489f-166">Click the **Files** tab in Page Inspector.</span></span>

    <span data-ttu-id="6489f-167">インデックス ページを作成するすべてのソース ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-167">You will see all the source files that are composing the Index page.</span></span> <span data-ttu-id="6489f-168">この機能は、部分ビューとテンプレートを使用している場合は特に、ひとめですべての要素を識別に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="6489f-168">This feature helps to identify all the elements at a glance, especially when you are working with partial views and templates.</span></span> <span data-ttu-id="6489f-169">開くことができますも各ファイルのリンクをクリックする場合に注意してください。</span><span class="sxs-lookup"><span data-stu-id="6489f-169">Notice that you can also open each of the files if you click the links.</span></span>

    ![[ファイル] タブ](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    <span data-ttu-id="6489f-171">*[ファイル] タブ*</span><span class="sxs-lookup"><span data-stu-id="6489f-171">*The Files tab*</span></span>
5. <span data-ttu-id="6489f-172">クリックして、**検査モードを切り替える**タブの左側にあるボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-172">Click the **Toggle Inspection Mode** button, located at the left of the tabs.</span></span>

    <span data-ttu-id="6489f-173">このツールを使用して、ページのいずれかの要素を選択して、HTML および Razor コードを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6489f-173">This tool will let you select any element of the page and see its HTML and Razor code.</span></span>

    ![トグル検査モード ボタン](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    <span data-ttu-id="6489f-175">*トグル検査モード ボタン*</span><span class="sxs-lookup"><span data-stu-id="6489f-175">*Toggle Inspection Mode button*</span></span>
6. <span data-ttu-id="6489f-176">Page Inspector のブラウザーでは、ページの要素に対するマウス ポインターを移動します。</span><span class="sxs-lookup"><span data-stu-id="6489f-176">In the Page Inspector browser, move the mouse pointer over the page elements.</span></span> <span data-ttu-id="6489f-177">表示するページの任意の部分にマウス ポインターを移動するときに、要素の型が表示され、対応するソース マークアップまたはコードが Visual Studio エディターで強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-177">While you move the mouse pointer over any part of the rendered page, the element type is displayed and the corresponding source markup or code is highlighted in the Visual Studio editor.</span></span>

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    <span data-ttu-id="6489f-179">*検査モードの動作*</span><span class="sxs-lookup"><span data-stu-id="6489f-179">*Inspection mode in action*</span></span>

    > [!NOTE]
    > <span data-ttu-id="6489f-180">Page Inspector ウィンドウを最大化しないで、またはソース コードを示す プレビュー タブを表示することはできません。</span><span class="sxs-lookup"><span data-stu-id="6489f-180">Do not maximize the Page Inspector window or you will not be able to see the preview tab showing the source code.</span></span> <span data-ttu-id="6489f-181">最大化されたときに、Page Inspector 内で要素をクリックすると、選択範囲のソース コードが表示されますが、Page Inspector ウィンドウを非表示になります。</span><span class="sxs-lookup"><span data-stu-id="6489f-181">If you click the element in Page Inspector when it is maximized, the source code of the selection will appear but it will hide the Page Inspector window.</span></span>

    <span data-ttu-id="6489f-182">注意する場合、 **Index.cshtml**ファイルが表示されます、選択した要素を生成するソース コードの部分が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-182">If you pay attention to the **Index.cshtml** file, you will notice that the portion of source code that generates the selected element is highlighted.</span></span> <span data-ttu-id="6489f-183">この機能は、コードにアクセスするダイレクトと高速の方法を提供、長いソース ファイルの編集を容易にします。</span><span class="sxs-lookup"><span data-stu-id="6489f-183">This feature facilitates the editing of long source files, providing a direct and fast way to access the code.</span></span>

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    <span data-ttu-id="6489f-185">*要素の検査*</span><span class="sxs-lookup"><span data-stu-id="6489f-185">*Inspecting elements*</span></span>
7. <span data-ttu-id="6489f-186">クリックして、**検査モードを切り替える**ボタン (![Page Inspector のブラウザーでレンダリングされた HTML コードを表示する HTML タブを選択します](using-page-inspector-in-visual-studio-2012/_static/image7.png "Page Inspector のブラウザーでレンダリングされた HTML コードを表示する HTML タブを選択します。")</span><span class="sxs-lookup"><span data-stu-id="6489f-186">Click the **Toggle Inspection Mode** button (![Select the HTML tab to display the HTML code rendered in the Page Inspector browser.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Select the HTML tab to display the HTML code rendered in the Page Inspector browser.")</span></span> <span data-ttu-id="6489f-187">)、カーソルを無効にします。</span><span class="sxs-lookup"><span data-stu-id="6489f-187">) to disable the cursor.</span></span>
8. <span data-ttu-id="6489f-188">選択、 **HTML** Page Inspector のブラウザーでレンダリングされた HTML コードを表示するタブです。</span><span class="sxs-lookup"><span data-stu-id="6489f-188">Select the **HTML** tab to display the HTML code rendered in the Page Inspector browser.</span></span>
9. <span data-ttu-id="6489f-189">HTML マークアップで Koala リンクのリスト アイテムをクリックして選択します。</span><span class="sxs-lookup"><span data-stu-id="6489f-189">In the HTML markup, locate the list item with the Koala link and select it.</span></span>

    <span data-ttu-id="6489f-190">コードを選択すると、対応する出力はブラウザーでは強調表示に自動的に注目してください。</span><span class="sxs-lookup"><span data-stu-id="6489f-190">Notice that when you select the code, the corresponding output is automatically highlighted in the browser.</span></span> <span data-ttu-id="6489f-191">この機能は HTML ブロックをページに表示する方法を表示すると便利です。</span><span class="sxs-lookup"><span data-stu-id="6489f-191">This feature is useful to see how an HTML block is rendered on the page.</span></span>

    <span data-ttu-id="6489f-192">![ページで選択する HTML 要素](using-page-inspector-in-visual-studio-2012/_static/image8.png " ページで選択する HTML 要素")</span><span class="sxs-lookup"><span data-stu-id="6489f-192">![Selecting HTML element in the page](using-page-inspector-in-visual-studio-2012/_static/image8.png "Selecting HTML element in the page")</span></span>

    <span data-ttu-id="6489f-193">*ページの HTML 要素の選択*</span><span class="sxs-lookup"><span data-stu-id="6489f-193">*Selecting HTML element in the page*</span></span>
10. <span data-ttu-id="6489f-194">をクリックして、**検査モードを切り替える** ボタンを有効にする*検査モード*ナビゲーション バー をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-194">Click the **Toggle Inspection Mode** button to enable *Inspection Mode* and click the navigation bar.</span></span> <span data-ttu-id="6489f-195">スタイル ウィンドウ内の HTML コードの右上には、選択した要素に適用される CSS スタイルを使用して一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-195">On the right of the HTML code, in the Styles pane, you will see a list with the CSS styles applied to the selected element.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6489f-196">Page Inspector を開くことも、ヘッダーがサイトのレイアウトの一部であるため、 \_Layout.cshtml ファイルとコードのセグメントが影響を強調表示します。</span><span class="sxs-lookup"><span data-stu-id="6489f-196">Since the header is a part of the site layout, Page Inspector will also open \_Layout.cshtml file and highlight the segment of code affected.</span></span>

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    <span data-ttu-id="6489f-198">*スタイルを検出すると、選択した要素のソース ファイル*</span><span class="sxs-lookup"><span data-stu-id="6489f-198">*Discovering styles and source files of a selected element*</span></span>
11. <span data-ttu-id="6489f-199">有効になっている、トグル検査ポインターでは、青の機能を備えたバーの下のマウス ポインターを移動を半分の円をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-199">With the toggle inspection pointer enabled, move the mouse pointer below the blue featured bar and click the half circle.</span></span>

    <span data-ttu-id="6489f-200">![要素の選択](using-page-inspector-in-visual-studio-2012/_static/image10.png "要素の選択")</span><span class="sxs-lookup"><span data-stu-id="6489f-200">![Selecting an element](using-page-inspector-in-visual-studio-2012/_static/image10.png "Selecting an element")</span></span>

    <span data-ttu-id="6489f-201">*要素の選択*</span><span class="sxs-lookup"><span data-stu-id="6489f-201">*Selecting an element*</span></span>
12. <span data-ttu-id="6489f-202">スタイルのウィンドウに、**背景イメージ**項目の下にある、 **.main コンテンツ**グループ。</span><span class="sxs-lookup"><span data-stu-id="6489f-202">In the Styles pane, locate the **background-image** item under the **.main-content** group.</span></span> <span data-ttu-id="6489f-203">**オフにして**、**背景イメージ**何が起こるかを確認します。</span><span class="sxs-lookup"><span data-stu-id="6489f-203">**Uncheck** the **background-image** and see what happens.</span></span> <span data-ttu-id="6489f-204">ブラウザーで変更がすぐに反映され、円が非表示のことがわかります。</span><span class="sxs-lookup"><span data-stu-id="6489f-204">You will notice that the browser will reflect the changes immediately and the circle is hidden.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6489f-205">ページ インスペクターのスタイル タブを適用する変更は、元のスタイル シートには影響しません。</span><span class="sxs-lookup"><span data-stu-id="6489f-205">The changes you apply on the Page Inspector Styles tab do not affect the original stylesheet.</span></span> <span data-ttu-id="6489f-206">スタイルをオフにしたり、回数だけ、したいが、ページを更新した後、復元するには、その値を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="6489f-206">You can uncheck styles or change their values as many times as you want, but they will be restored after refreshing the page.</span></span>

    <span data-ttu-id="6489f-207">![有効にして、CSS スタイルを無効化](using-page-inspector-in-visual-studio-2012/_static/image11.png "の有効化と CSS スタイルを無効にします。")</span><span class="sxs-lookup"><span data-stu-id="6489f-207">![Enabling and disabling CSS styles](using-page-inspector-in-visual-studio-2012/_static/image11.png "Enabling and disabling CSS styles")</span></span>

    <span data-ttu-id="6489f-208">*有効にして、CSS スタイルを無効化*</span><span class="sxs-lookup"><span data-stu-id="6489f-208">*Enabling and disabling CSS styles*</span></span>
13. <span data-ttu-id="6489f-209">をクリックして、'**ここにロゴ**' 検査モードを使用して、ヘッダーのテキスト。</span><span class="sxs-lookup"><span data-stu-id="6489f-209">Now, click the '**your logo here**' text on the header using the inspection mode.</span></span>
14. <span data-ttu-id="6489f-210">**スタイル** タブで、検索、**フォント サイズ**CSS 属性の下にある、 **.site タイトル**グループ。</span><span class="sxs-lookup"><span data-stu-id="6489f-210">In the **Styles** tab, locate the **font-size** CSS attribute under the **.site-title** group.</span></span> <span data-ttu-id="6489f-211">属性の値をダブルクリックし、2.3 em 値の置換**3 em**、キーを押します**ENTER**です。</span><span class="sxs-lookup"><span data-stu-id="6489f-211">Double-click the attribute value and replace the 2.3 em value with **3 em**, and then press **ENTER**.</span></span> <span data-ttu-id="6489f-212">タイトルに大きく見えることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6489f-212">Notice that the title looks bigger.</span></span>

    <span data-ttu-id="6489f-213">![Page Inspector 内で CSS の値を変更する](using-page-inspector-in-visual-studio-2012/_static/image12.png "Page Inspector 内で変更する CSS の値")</span><span class="sxs-lookup"><span data-stu-id="6489f-213">![Changing CSS values in the Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image12.png "Changing CSS values in the Page Inspector")</span></span>

    <span data-ttu-id="6489f-214">*Page Inspector 内で CSS の値を変更します。*</span><span class="sxs-lookup"><span data-stu-id="6489f-214">*Changing CSS values in the Page Inspector*</span></span>
15. <span data-ttu-id="6489f-215">クリックして、**トレース スタイル** タブ、Page Inspector の右側のウィンドウであります。</span><span class="sxs-lookup"><span data-stu-id="6489f-215">Click the **Trace Styles** tab, located in the right pane of Page Inspector.</span></span> <span data-ttu-id="6489f-216">これは、属性の名前順で、選択範囲に適用されるすべてのスタイルを表示する別の方法です。</span><span class="sxs-lookup"><span data-stu-id="6489f-216">This is an alternative way to see all the styles applied to the selection, ordered by attribute name.</span></span>

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    <span data-ttu-id="6489f-218">*選択した要素の CSS スタイルのトレース*</span><span class="sxs-lookup"><span data-stu-id="6489f-218">*CSS styles tracing of the selected element*</span></span>
16. <span data-ttu-id="6489f-219">Page Inspector のもう 1 つの機能は、レイアウト ペインです。</span><span class="sxs-lookup"><span data-stu-id="6489f-219">Another feature of Page Inspector is the Layout pane.</span></span> <span data-ttu-id="6489f-220">検査モードを使用して、ナビゲーション バーを選択し、をクリックして、**レイアウト**右側のウィンドウ タブ。</span><span class="sxs-lookup"><span data-stu-id="6489f-220">Using the inspection mode, select the navigation bar and then click the **Layout** tab on the right pane.</span></span> <span data-ttu-id="6489f-221">選択した要素の正確なサイズだけでなく、オフセット、余白、パディング、および境界線のサイズが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-221">You will see the exact size of the selected element, as well as its offset, margin, padding and border size.</span></span> <span data-ttu-id="6489f-222">このビューから値を変更することも確認します。</span><span class="sxs-lookup"><span data-stu-id="6489f-222">Notice that you can also modify the values from this view.</span></span>

    <span data-ttu-id="6489f-223">![Page Inspector 内で要素のレイアウト](using-page-inspector-in-visual-studio-2012/_static/image14.png "Page Inspector 内で要素のレイアウト")</span><span class="sxs-lookup"><span data-stu-id="6489f-223">![Element layout in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image14.png "Element layout in Page Inspector")</span></span>

    <span data-ttu-id="6489f-224">*Page Inspector 内で要素のレイアウト*</span><span class="sxs-lookup"><span data-stu-id="6489f-224">*Element layout in Page Inspector*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a><span data-ttu-id="6489f-225">タスク 2 - 検索して、フォト ギャラリーでのスタイルの問題の解決</span><span class="sxs-lookup"><span data-stu-id="6489f-225">Task 2 - Finding and Fixing Style Issues in the Photo Gallery</span></span>

<span data-ttu-id="6489f-226">以前のバージョンの Visual Studio で Web ページの問題を診断するにはどうは?</span><span class="sxs-lookup"><span data-stu-id="6489f-226">How would you diagnose Web pages issues with previous versions of Visual Studio?</span></span> <span data-ttu-id="6489f-227">可能性の高い経験のある web に Internet Explorer Developer Tools または Firebug など、Visual Studio IDE の外部で実行するデバッグ ツールとしています。</span><span class="sxs-lookup"><span data-stu-id="6489f-227">You are likely familiar with web debugging tools that run outside the Visual Studio IDE, like Internet Explorer Developer Tools or Firebug.</span></span> <span data-ttu-id="6489f-228">ブラウザーでのみ認識、HTML スクリプトとスタイル、基になるフレームワークがレンダリングされる HTML を生成中にします。</span><span class="sxs-lookup"><span data-stu-id="6489f-228">Browsers only understand HTML, scripting and styles, while an underlying framework generates the HTML that will be rendered.</span></span> <span data-ttu-id="6489f-229">そのため、多くの場合、サイト全体のように web ページの外観を確認するを展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6489f-229">For that reason, you often need to deploy the whole site to see how web pages look like.</span></span>

<span data-ttu-id="6489f-230">検出し、web サイトで、問題を解決するには、次の手順をに従ってする必要がある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="6489f-230">You had probably followed these steps when you wanted to detect and fix an issue in your web site:</span></span>

1. <span data-ttu-id="6489f-231">Visual studio でソリューションを実行するか、web サーバー上のページを展開します。</span><span class="sxs-lookup"><span data-stu-id="6489f-231">Run the Solution from Visual Studio, or deploy the page on the web server.</span></span>
2. <span data-ttu-id="6489f-232">ブラウザーで、開発者ツールを使用すると、または単に、ソース コードとスタイルを開きますしようとする問題を一致を開きます。</span><span class="sxs-lookup"><span data-stu-id="6489f-232">In the browser, open the developer tools you use, or simply open the source code and the styles and try to match the issue.</span></span> <span data-ttu-id="6489f-233">ファイルを検索する、関連するを使用した、&quot;検索&quot;または&quot;ファイルで検索&quot;スタイル クラスの名前で機能します。</span><span class="sxs-lookup"><span data-stu-id="6489f-233">To find the files involved, you would have used the &quot;Search&quot; or &quot;Search in files&quot; features with the name of the style classes.</span></span>
3. <span data-ttu-id="6489f-234">エラーが検出されると、Web ブラウザーとサーバーを停止します。</span><span class="sxs-lookup"><span data-stu-id="6489f-234">Once the error is detected, stop the Web browser and the server.</span></span>
4. <span data-ttu-id="6489f-235">ブラウザーのキャッシュを消去します。</span><span class="sxs-lookup"><span data-stu-id="6489f-235">Clear the browser cache.</span></span>
5. <span data-ttu-id="6489f-236">修正を適用する Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="6489f-236">Return to Visual Studio to apply a fix.</span></span> <span data-ttu-id="6489f-237">テストするすべての手順を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="6489f-237">Repeat all the steps to test.</span></span>

<span data-ttu-id="6489f-238">ASP.NET MVC 4 の実際の WYSIWYG がないため、スタイルの問題のほとんどが後の段階で実行されているまたは web アプリケーションの展開後に検出されました。</span><span class="sxs-lookup"><span data-stu-id="6489f-238">As there is no real WYSIWYG in ASP.NET MVC 4, most of the style issues are detected on a later stage, after running or deploying the web application.</span></span> <span data-ttu-id="6489f-239">ここで、Page Inspector でソリューションを実行しなくても、任意のページをプレビューすることができます。</span><span class="sxs-lookup"><span data-stu-id="6489f-239">Now, with Page Inspector, it is possible to preview any page without running the solution.</span></span>

<span data-ttu-id="6489f-240">このタスクでは、Page inspector を使用し、アプリケーションをフォト ギャラリーのいくつかの問題を修正します。</span><span class="sxs-lookup"><span data-stu-id="6489f-240">In this task, you will use the Page inspector and fix some issues the Photo Gallery application.</span></span>

1. <span data-ttu-id="6489f-241">Page Inspector を使用して探します、**登録**と**ログイン**ヘッダーの左側にあるリンクします。</span><span class="sxs-lookup"><span data-stu-id="6489f-241">Using Page Inspector, locate the **Register** and the **Log in** links at the left side of the header.</span></span>

    <span data-ttu-id="6489f-242">リンクは、右側の必要な場所には表示されませんし、箇条書きリストのように表示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6489f-242">Notice that the links are not displayed at the expected place on the right, and they are shown like a bulleted list.</span></span> <span data-ttu-id="6489f-243">それに応じてスタイルを変更して、右へのリンクを整列するされます。</span><span class="sxs-lookup"><span data-stu-id="6489f-243">You will now align the links to the right and restyle them accordingly.</span></span>

    <span data-ttu-id="6489f-244">![リンクで、登録とログを検索する](using-page-inspector-in-visual-studio-2012/_static/image15.png "へのリンクで、登録とログを検索します。")</span><span class="sxs-lookup"><span data-stu-id="6489f-244">![Locating the Register and Log in links](using-page-inspector-in-visual-studio-2012/_static/image15.png "Locating the Register and Log in links")</span></span>

    <span data-ttu-id="6489f-245">*リンクで、登録とログを検索します。*</span><span class="sxs-lookup"><span data-stu-id="6489f-245">*Locating the Register and Log in links*</span></span>
2. <span data-ttu-id="6489f-246">選択されている検査モードの切り替え、閉じるをではなく、そのコードを開くための登録のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-246">With Toggle Inspection Mode selected, click close to, but not on, the Register link to open its code.</span></span>

    <span data-ttu-id="6489f-247">リンクのソース コードがあることを確認、  **\_LoginPartial.cshtml**ファイル、Index.cshtml いないも\_Layout.cshtml は、最初に探すこともできます。</span><span class="sxs-lookup"><span data-stu-id="6489f-247">Notice that the source code of the links is located in the **\_LoginPartial.cshtml** file, not the Index.cshtml nor the \_Layout.cshtml, which are the places you might look in first place.</span></span> <span data-ttu-id="6489f-248">正しいソース ファイルに直接配置されています。</span><span class="sxs-lookup"><span data-stu-id="6489f-248">You have been placed directly in the correct source file.</span></span>
3. <span data-ttu-id="6489f-249">**スタイル** タブを特定し、をクリックして、  **<section> #login</section>** 項目は、これらのリンクを HTML のコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="6489f-249">In the **Styles** tab, locate and click the **<section> #login</section>** item, which is the HTML container for these links.</span></span>

    <span data-ttu-id="6489f-250">注意して、 **#login**スタイルで自動的にある**Site.css**  をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-250">Notice that the **#login** style is automatically located in **Site.css** after you click.</span></span> <span data-ttu-id="6489f-251">さらに、コードは、ここで強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-251">Moreover, the code is now highlighted.</span></span>

    <span data-ttu-id="6489f-252">![CSS スタイルの選択](using-page-inspector-in-visual-studio-2012/_static/image16.png "CSS スタイルの選択")</span><span class="sxs-lookup"><span data-stu-id="6489f-252">![Selecting the CSS styles](using-page-inspector-in-visual-studio-2012/_static/image16.png "Selecting the CSS styles")</span></span>

    <span data-ttu-id="6489f-253">*CSS スタイルの選択*</span><span class="sxs-lookup"><span data-stu-id="6489f-253">*Selecting the CSS styles*</span></span>
4. <span data-ttu-id="6489f-254">コメントを解除、**テキスト配置**属性の開始タグと終了文字を削除することで強調表示されたコードと保存、 **Site.css**ファイル。</span><span class="sxs-lookup"><span data-stu-id="6489f-254">Uncomment the **text-align** attribute in the highlighted code by removing the opening and closing characters and save the **Site.css** file.</span></span>

    <span data-ttu-id="6489f-255">Page Inspector は、現在のページを構成するすべてのファイルを認識しており、これらのファイルのいずれかが変更を検出できます。</span><span class="sxs-lookup"><span data-stu-id="6489f-255">Page Inspector is aware of all the different files that compose the current page, and it can detect when any of these files change.</span></span> <span data-ttu-id="6489f-256">ブラウザーでは、現在のページがソース ファイルと同期されるたびにユーザーに警告します。</span><span class="sxs-lookup"><span data-stu-id="6489f-256">It alerts you whenever the current page in browser is not in sync with the source files.</span></span>
5. <span data-ttu-id="6489f-257">Page Inspector のブラウザーでは、ページを再度読み込んで、アドレス バーの下にあるバーをクリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-257">In the Page Inspector browser, click the bar located below the address bar to reload the page.</span></span>

    ![ページの再読み込み](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    <span data-ttu-id="6489f-259">*ページの再読み込み*</span><span class="sxs-lookup"><span data-stu-id="6489f-259">*Reloading the page*</span></span>

    <span data-ttu-id="6489f-260">リンクようになりました、右側にあるが、まだ箇条書きリストのように見えます。</span><span class="sxs-lookup"><span data-stu-id="6489f-260">The links are now at the right, but they still look like a bulleted list.</span></span> <span data-ttu-id="6489f-261">ここで、箇条書きを削除し、リンクを水平方向に整列します。</span><span class="sxs-lookup"><span data-stu-id="6489f-261">Now, you will remove the bullets and align the links horizontally.</span></span>

    ![更新されたページ](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    <span data-ttu-id="6489f-263">*更新されたページ*</span><span class="sxs-lookup"><span data-stu-id="6489f-263">*Updated page*</span></span>
6. <span data-ttu-id="6489f-264">いずれかを選択して検査モードを使用して、  **&lt;li&gt;** を含む項目、&quot;登録&quot;と&quot;ログイン&quot;リンクします。</span><span class="sxs-lookup"><span data-stu-id="6489f-264">Using the inspection mode, select any of the **&lt;li&gt;** items that contain the &quot;Register&quot; and &quot;Log in&quot; links.</span></span> <span data-ttu-id="6489f-265">次に、をクリックして、 **&lt;セクション&gt;#login**にアクセスする項目**Styles.css**コード。</span><span class="sxs-lookup"><span data-stu-id="6489f-265">Then, click the **&lt;section&gt; #login** item to access **Styles.css** code.</span></span>

    <span data-ttu-id="6489f-266">![スタイルを検索する](using-page-inspector-in-visual-studio-2012/_static/image19.png "スタイルを検索します。")</span><span class="sxs-lookup"><span data-stu-id="6489f-266">![Finding the style](using-page-inspector-in-visual-studio-2012/_static/image19.png "Finding the style")</span></span>

    <span data-ttu-id="6489f-267">*スタイルを検索します。*</span><span class="sxs-lookup"><span data-stu-id="6489f-267">*Finding the style*</span></span>
7. <span data-ttu-id="6489f-268">**Style.css**、用のコードのコメントを解除**#login li**項目。</span><span class="sxs-lookup"><span data-stu-id="6489f-268">In **Style.css**, uncomment the code for **#login li** items.</span></span> <span data-ttu-id="6489f-269">追加するスタイルの項目を非表示にされ、アイテムの水平方向に表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-269">The style you are adding will hide the bullet and display the items horizontally.</span></span>

    <span data-ttu-id="6489f-270">![スタイルの変更、ログイン リンク](using-page-inspector-in-visual-studio-2012/_static/image20.png "ログイン リンク スタイルの変更")</span><span class="sxs-lookup"><span data-stu-id="6489f-270">![Restyling the login links](using-page-inspector-in-visual-studio-2012/_static/image20.png "Restyling the login links")</span></span>

    <span data-ttu-id="6489f-271">*ログイン リンク スタイルの変更*</span><span class="sxs-lookup"><span data-stu-id="6489f-271">*Restyling the login links*</span></span>
8. <span data-ttu-id="6489f-272">保存**Style.css**ファイルおよびページの再読み込みするアドレスの下にあるバーを 1 回クリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-272">Save **Style.css** file and click once on the bar located below the address to reload the page.</span></span> <span data-ttu-id="6489f-273">リンクが正しく表示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6489f-273">Notice that the links appear correctly.</span></span>

    <span data-ttu-id="6489f-274">![リンクが右側に整列](using-page-inspector-in-visual-studio-2012/_static/image21.png "へのリンクが、右側に配置")</span><span class="sxs-lookup"><span data-stu-id="6489f-274">![Links aligned to the right side](using-page-inspector-in-visual-studio-2012/_static/image21.png "Links aligned to the right side")</span></span>

    <span data-ttu-id="6489f-275">*右揃えで配置へのリンク*</span><span class="sxs-lookup"><span data-stu-id="6489f-275">*Links aligned to the right side*</span></span>
9. <span data-ttu-id="6489f-276">最後に、ヘッダーのタイトルを変更します。</span><span class="sxs-lookup"><span data-stu-id="6489f-276">Finally, you will change the header title.</span></span> <span data-ttu-id="6489f-277">検査モードを使用 をクリックして**ここにロゴ**テキストと生成されたソース コードを取得します。</span><span class="sxs-lookup"><span data-stu-id="6489f-277">Use the inspection mode to click **your logo here** text and get to the source code that generates it.</span></span>
10. <span data-ttu-id="6489f-278">使用するようになりました **\_Layout.cshtml**、置換 '**ここにロゴ**'にテキスト'**フォト ギャラリー**' です。</span><span class="sxs-lookup"><span data-stu-id="6489f-278">Now you are in **\_Layout.cshtml**, replace '**your logo here**' text with '**Photo Gallery**'.</span></span> <span data-ttu-id="6489f-279">保存して Page Inspector のブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="6489f-279">Save and update the Page Inspector browser.</span></span>

    <span data-ttu-id="6489f-280">![新しいタイトルを割り当てる](using-page-inspector-in-visual-studio-2012/_static/image22.png "新しいタイトルを割り当てる")</span><span class="sxs-lookup"><span data-stu-id="6489f-280">![Assigning a new title](using-page-inspector-in-visual-studio-2012/_static/image22.png "Assigning a new title")</span></span>

    <span data-ttu-id="6489f-281">*新しいタイトルを割り当てる*</span><span class="sxs-lookup"><span data-stu-id="6489f-281">*Assigning a new title*</span></span>

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    <span data-ttu-id="6489f-283">*フォト ギャラリー ページが更新されました*</span><span class="sxs-lookup"><span data-stu-id="6489f-283">*Photo Gallery page updated*</span></span>
11. <span data-ttu-id="6489f-284">最後に、方法を選択、 **PhotoGallery**プロジェクトとキーを押して**f5 キーを押して**アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="6489f-284">Finally, selet the **PhotoGallery** project and press **F5** to run the app.</span></span> <span data-ttu-id="6489f-285">チェック アウトすべて変更が想定どおりに動作します。</span><span class="sxs-lookup"><span data-stu-id="6489f-285">Check out all the changes work as expected.</span></span>

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a><span data-ttu-id="6489f-286">手順 2: WebForms プロジェクトで Page Inspector を使用します。</span><span class="sxs-lookup"><span data-stu-id="6489f-286">Exercise 2: Using Page Inspector in WebForms Projects</span></span>

<span data-ttu-id="6489f-287">この演習では、プレビューして Page Inspector を使用して WebForms ソリューションをデバッグする方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="6489f-287">In this exercise, you will learn how to preview and debug a WebForms solution using Page Inspector.</span></span> <span data-ttu-id="6489f-288">最初はプロセスのデバッグ、Web を容易に Page Inspector の機能を説明するツールの簡単な膝を行います。</span><span class="sxs-lookup"><span data-stu-id="6489f-288">You will first perform a brief lap around the tool to learn the Page Inspector features that facilitate the Web debugging process.</span></span> <span data-ttu-id="6489f-289">次に、スタイルの問題を含む web ページで作業します。</span><span class="sxs-lookup"><span data-stu-id="6489f-289">Then, you will work in a web page that contains styling issues.</span></span> <span data-ttu-id="6489f-290">Page Inspector を使用して、問題を生成するソース コードを検索し、その修正方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="6489f-290">You will learn how to use Page Inspector to find the source code that generates the issue and fix it.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a><span data-ttu-id="6489f-291">タスク 1 - Page Inspector の探索</span><span class="sxs-lookup"><span data-stu-id="6489f-291">Task 1 - Exploring Page Inspector</span></span>

<span data-ttu-id="6489f-292">このタスクでは、フォト ギャラリーを表示する WebForms プロジェクトのコンテキストで Page Inspector の機能を使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="6489f-292">In this task, you will learn how to use the Page Inspector features in the context of a WebForms project that shows a photo gallery.</span></span>

1. <span data-ttu-id="6489f-293">開く、**開始**ソリューションにある**ソース/Ex2-WebForms/開始/**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="6489f-293">Open the **Begin** solution located at **Source/Ex2-WebForms/Begin/** folder.</span></span>

    1. <span data-ttu-id="6489f-294">いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行前にします。</span><span class="sxs-lookup"><span data-stu-id="6489f-294">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6489f-295">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="6489f-295">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="6489f-296">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="6489f-296">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="6489f-297">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="6489f-297">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6489f-298">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="6489f-298">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6489f-299">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="6489f-299">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6489f-300">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="6489f-300">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="6489f-301">ソリューション エクスプ ローラーで**Default.aspx**  ページで、右クリックし、選択**Page Inspector で表示**です。</span><span class="sxs-lookup"><span data-stu-id="6489f-301">In the Solution Explorer, locate **Default.aspx** page, right-click it and select **View in Page Inspector**.</span></span>

    <span data-ttu-id="6489f-302">![Page Inspector で Default.aspx を開く](using-page-inspector-in-visual-studio-2012/_static/image24.png "Page Inspector で Default.aspx を開く")</span><span class="sxs-lookup"><span data-stu-id="6489f-302">![Opening Default.aspx with Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image24.png "Opening Default.aspx with Page Inspector")</span></span>

    <span data-ttu-id="6489f-303">*Page Inspector で Default.aspx を開く*</span><span class="sxs-lookup"><span data-stu-id="6489f-303">*Opening Default.aspx with Page Inspector*</span></span>
3. <span data-ttu-id="6489f-304">Default.aspx ページ インスペクター ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-304">The Page Inspector window will show Default.aspx.</span></span>

    <span data-ttu-id="6489f-305">![Page Inspector 内で Default.aspx を表示する](using-page-inspector-in-visual-studio-2012/_static/image25.png "Page Inspector 内で Default.aspx を表示します。")</span><span class="sxs-lookup"><span data-stu-id="6489f-305">![Viewing Default.aspx in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image25.png "Viewing Default.aspx in Page Inspector")</span></span>

    <span data-ttu-id="6489f-306">*Page Inspector 内で Default.aspx を表示します。*</span><span class="sxs-lookup"><span data-stu-id="6489f-306">*Viewing Default.aspx in Page Inspector*</span></span>

    <span data-ttu-id="6489f-307">Page Inspector ツールは、Visual Studio 環境内に統合されています。</span><span class="sxs-lookup"><span data-stu-id="6489f-307">The Page Inspector tool is integrated in your Visual Studio environment.</span></span> <span data-ttu-id="6489f-308">インスペクターには、選択したコードを表示する強力な HTML プロファイラーと共にの組み込みブラウザーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6489f-308">The inspector contains an embedded browser, together with a powerful HTML profiler that will show the selected code.</span></span> <span data-ttu-id="6489f-309">注意してください、ページの外観を確認するようにソリューションを実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="6489f-309">Notice that you do not have to run the solution to see how your pages look.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6489f-310">Page Inspector のブラウザーの幅が開かれるページの幅よりも低い場合は表示されませんページ適切です。</span><span class="sxs-lookup"><span data-stu-id="6489f-310">When the width of Page Inspector browser is less than the width of the opened page, you will not see the page properly.</span></span> <span data-ttu-id="6489f-311">その場合は、Page Inspector の幅を調整します。</span><span class="sxs-lookup"><span data-stu-id="6489f-311">If that happens, adjust the width of the Page Inspector.</span></span>
4. <span data-ttu-id="6489f-312">クリックして、**ファイル**Page Inspector 内でのタブです。</span><span class="sxs-lookup"><span data-stu-id="6489f-312">Click the **Files** tab in Page Inspector.</span></span>

    <span data-ttu-id="6489f-313">表示された既定のページを作成するすべてのソース ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-313">You will see all the source files that are composing the rendered Default page.</span></span> <span data-ttu-id="6489f-314">これは、ユーザー コントロールとマスター ページを使用している場合は特に、ひとめですべての要素を識別する便利な機能です。</span><span class="sxs-lookup"><span data-stu-id="6489f-314">This is a useful feature to identify all the elements at a glance, especially when you are working with User Controls and Master Pages.</span></span> <span data-ttu-id="6489f-315">各ファイルに移動することも確認します。</span><span class="sxs-lookup"><span data-stu-id="6489f-315">Notice that you can also navigate to each of the files.</span></span>

    <span data-ttu-id="6489f-316">![[ファイル] タブ](using-page-inspector-in-visual-studio-2012/_static/image26.png "[ファイル] タブ")</span><span class="sxs-lookup"><span data-stu-id="6489f-316">![The Files tab](using-page-inspector-in-visual-studio-2012/_static/image26.png "The Files tab")</span></span>

    <span data-ttu-id="6489f-317">*[ファイル] タブ*</span><span class="sxs-lookup"><span data-stu-id="6489f-317">*The Files tab*</span></span>
5. <span data-ttu-id="6489f-318">クリックして、**検査モードを切り替える**タブの左側にあるボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-318">Click the **Toggle Inspection Mode** button, located at the left of the tabs.</span></span>

    <span data-ttu-id="6489f-319">このツールを使用して、ページのいずれかの要素を選択して、HTML コードおよび .aspx ソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6489f-319">This tool will let you select any element of the page and see its HTML code and .aspx source.</span></span>

    <span data-ttu-id="6489f-320">![トグル検査モード ボタン](using-page-inspector-in-visual-studio-2012/_static/image27.png "検査モードの切り替えボタン")</span><span class="sxs-lookup"><span data-stu-id="6489f-320">![Toggle Inspection Mode button](using-page-inspector-in-visual-studio-2012/_static/image27.png "Toggle Inspection Mode button")</span></span>

    <span data-ttu-id="6489f-321">*トグル検査モード ボタン*</span><span class="sxs-lookup"><span data-stu-id="6489f-321">*Toggle Inspection Mode button*</span></span>
6. <span data-ttu-id="6489f-322">Page Inspector のブラウザーでは、ページ要素にマウスを移動します。</span><span class="sxs-lookup"><span data-stu-id="6489f-322">In the Page Inspector browser, move the mouse over the page elements.</span></span> <span data-ttu-id="6489f-323">表示するページの任意の部分にマウス ポインターを移動するときに、要素の型が表示され、対応するソース マークアップまたはコードが Visual Studio エディターで強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-323">While you move the mouse pointer over any part of the rendered page, the element type is displayed and the corresponding source markup or code is highlighted in the Visual Studio editor.</span></span>

    <span data-ttu-id="6489f-324">![アクションで検査モード](using-page-inspector-in-visual-studio-2012/_static/image28.png "検査モードの動作")</span><span class="sxs-lookup"><span data-stu-id="6489f-324">![Inspection mode in action](using-page-inspector-in-visual-studio-2012/_static/image28.png "Inspection mode in action")</span></span>

    <span data-ttu-id="6489f-325">*検査モードの動作*</span><span class="sxs-lookup"><span data-stu-id="6489f-325">*Inspection mode in action*</span></span>

    > [!NOTE]
    > <span data-ttu-id="6489f-326">Page Inspector ウィンドウを最大化しないで、またはソース コードを示す プレビュー タブを表示することはできません。</span><span class="sxs-lookup"><span data-stu-id="6489f-326">Do not maximize the Page Inspector window or you will not be able to see the preview tab showing the source code.</span></span> <span data-ttu-id="6489f-327">最大化されたときに、Page Inspector 内で要素をクリックすると、選択範囲のソース コードが表示されますが、Page Inspector ウィンドウを非表示になります。</span><span class="sxs-lookup"><span data-stu-id="6489f-327">If you click the element in Page Inspector when it is maximized, the source code of the selection will appear but it will hide the Page Inspector window.</span></span>

    <span data-ttu-id="6489f-328">場合に注意を払う**Default.aspx**ファイルが表示されます、選択した要素を生成するソース コードの部分が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-328">If you pay attention to **Default.aspx** file, you will notice that the portion of source code that generates the selected element is highlighted.</span></span> <span data-ttu-id="6489f-329">この機能には、長いソース ファイル、コードにアクセスするダイレクトと高速の方法を提供のエディションが容易になります。</span><span class="sxs-lookup"><span data-stu-id="6489f-329">This feature facilitates the edition of long source files, providing a direct and fast way to access the code.</span></span>

    <span data-ttu-id="6489f-330">![要素を検査](using-page-inspector-in-visual-studio-2012/_static/image29.png "要素を検査")</span><span class="sxs-lookup"><span data-stu-id="6489f-330">![Inspecting elements](using-page-inspector-in-visual-studio-2012/_static/image29.png "Inspecting elements")</span></span>

    <span data-ttu-id="6489f-331">*要素の検査*</span><span class="sxs-lookup"><span data-stu-id="6489f-331">*Inspecting elements*</span></span>
7. <span data-ttu-id="6489f-332">クリックして、**検査モードを切り替える**ボタン (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser] 。(using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser です。")</span><span class="sxs-lookup"><span data-stu-id="6489f-332">Click the **Toggle Inspection Mode** button (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.")</span></span> <span data-ttu-id="6489f-333">)、カーソルを無効にする、Page Inspector のタブにあります。</span><span class="sxs-lookup"><span data-stu-id="6489f-333">), located in Page Inspector tabs, to disable the cursor.</span></span>
8. <span data-ttu-id="6489f-334">選択、 **HTML** Page Inspector のブラウザーでレンダリングされた HTML コードを表示するタブです。</span><span class="sxs-lookup"><span data-stu-id="6489f-334">Select the **HTML** tab to display the HTML code rendered in the Page Inspector browser.</span></span>
9. <span data-ttu-id="6489f-335">HTML のコードで Koala リンクのリスト アイテムをクリックして選択します。</span><span class="sxs-lookup"><span data-stu-id="6489f-335">In the HTML code, locate the list item with the Koala link and select it.</span></span>

    <span data-ttu-id="6489f-336">コードを選択すると、対応する出力が自動的に強調表示されているブラウザーであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6489f-336">Notice that when you select the code, the corresponding output is automatically highlighted browser.</span></span> <span data-ttu-id="6489f-337">この機能は HTML ブロックをページに表示する方法を表示すると便利です。</span><span class="sxs-lookup"><span data-stu-id="6489f-337">This feature is useful to see how an HTML block is rendered on the page.</span></span>

    <span data-ttu-id="6489f-338">![HTML 要素の選択 ページで](using-page-inspector-in-visual-studio-2012/_static/image31.png " ページで、HTML 要素の選択")</span><span class="sxs-lookup"><span data-stu-id="6489f-338">![Selecting an HTML element in the page](using-page-inspector-in-visual-studio-2012/_static/image31.png "Selecting an HTML element in the page")</span></span>

    <span data-ttu-id="6489f-339">*ページの HTML 要素の選択*</span><span class="sxs-lookup"><span data-stu-id="6489f-339">*Selecting an HTML element in the page*</span></span>
10. <span data-ttu-id="6489f-340">をクリックして、**検査モードを切り替える** ボタンを有効にする*検査モード*ナビゲーション バー をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-340">Click the **Toggle Inspection Mode** button to enable *Inspection Mode* and click the navigation bar.</span></span> <span data-ttu-id="6489f-341">スタイル ウィンドウ内の HTML コードの右上には、選択した要素に適用される CSS スタイルを使用して一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-341">On the right of the HTML code, in the Styles pane, you will see a list with the CSS styles applied to the selected element.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6489f-342">ヘッダーがサイトのレイアウトの一部であるため、Page Inspector も Site.Master ファイルを開き、影響を受けるコードのセグメントを強調表示します。</span><span class="sxs-lookup"><span data-stu-id="6489f-342">since the header is a part of the site layout, Page Inspector will also open Site.Master file and highlight the segment of code affected.</span></span>

    <span data-ttu-id="6489f-343">![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "スタイルを検出すると、選択した要素のソース ファイル")</span><span class="sxs-lookup"><span data-stu-id="6489f-343">![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "Discovering styles and source files of a selected element")</span></span>

    <span data-ttu-id="6489f-344">*スタイルを検出すると、選択した要素のソース ファイル*</span><span class="sxs-lookup"><span data-stu-id="6489f-344">*Discovering styles and source files of a selected element*</span></span>
11. <span data-ttu-id="6489f-345">有効になっている、トグル検査ポインターでは、メニュー バーの下のマウス ポインターを移動を空白の半分の円をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-345">With the toggle inspection pointer enabled, move the mouse pointer below the menu bar and click the blank half circle.</span></span>

    <span data-ttu-id="6489f-346">![要素の選択](using-page-inspector-in-visual-studio-2012/_static/image33.png "要素の選択")</span><span class="sxs-lookup"><span data-stu-id="6489f-346">![Selecting an element](using-page-inspector-in-visual-studio-2012/_static/image33.png "Selecting an element")</span></span>

    <span data-ttu-id="6489f-347">*要素の選択*</span><span class="sxs-lookup"><span data-stu-id="6489f-347">*Selecting an element*</span></span>
12. <span data-ttu-id="6489f-348">スタイルのウィンドウに、**背景イメージ**項目の下にある、 **.main コンテンツ**グループ。</span><span class="sxs-lookup"><span data-stu-id="6489f-348">In the Styles pane, locate the **background-image** item under the **.main-content** group.</span></span> <span data-ttu-id="6489f-349">**オフにして**、**背景イメージ**何が起こるかを確認します。</span><span class="sxs-lookup"><span data-stu-id="6489f-349">**Uncheck** the **background-image** and see what happens.</span></span> <span data-ttu-id="6489f-350">ブラウザーで変更がすぐに反映され、円が非表示のことがわかります。</span><span class="sxs-lookup"><span data-stu-id="6489f-350">You will notice that the browser will reflect the changes immediately and the circle is hidden.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6489f-351">ページ インスペクターのスタイル タブを適用する変更は、元のスタイル シートには影響しません。</span><span class="sxs-lookup"><span data-stu-id="6489f-351">The changes you apply on the Page Inspector Styles tab do not affect the original stylesheet.</span></span> <span data-ttu-id="6489f-352">スタイルをオフにしたり、回数だけ、したいが、ページを更新した後、復元するには、その値を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="6489f-352">You can uncheck styles or change their values as many times as you want, but they will be restored after refreshing the page.</span></span>

    <span data-ttu-id="6489f-353">![有効にして、CSS styles2 を無効化](using-page-inspector-in-visual-studio-2012/_static/image34.png "の有効化と CSS スタイルを無効にします。")</span><span class="sxs-lookup"><span data-stu-id="6489f-353">![Enabling and disabling CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "Enabling and disabling CSS styles")</span></span>

    <span data-ttu-id="6489f-354">*有効にして、CSS スタイルを無効化*</span><span class="sxs-lookup"><span data-stu-id="6489f-354">*Enabling and disabling CSS styles*</span></span>
13. <span data-ttu-id="6489f-355">をクリックして、'**、** **ここにロゴ '**検査モードを使用して、ヘッダーのテキスト。</span><span class="sxs-lookup"><span data-stu-id="6489f-355">Now, click the '**your** **logo here'** text on the header using the inspection mode.</span></span>
14. <span data-ttu-id="6489f-356">**スタイル** タブで、検索、**フォント サイズ**CSS 属性の下にある、 **.site タイトル**グループ。</span><span class="sxs-lookup"><span data-stu-id="6489f-356">In the **Styles** tab, locate the **font-size** CSS attribute under the **.site-title** group.</span></span> <span data-ttu-id="6489f-357">値を編集するには、1 回の属性をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-357">Double-click the attribute once to edit its value.</span></span> <span data-ttu-id="6489f-358">置換、2.3em 値**3em**、ENTER キーを押します。</span><span class="sxs-lookup"><span data-stu-id="6489f-358">Replace the 2.3em value with **3em**, and then press ENTER.</span></span> <span data-ttu-id="6489f-359">タイトルに大きく見えることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6489f-359">Notice that the title looks bigger.</span></span>

    <span data-ttu-id="6489f-360">![ページ Inspector2 CSS 値を変更する](using-page-inspector-in-visual-studio-2012/_static/image35.png "Page Inspector 内で変更する CSS の値")</span><span class="sxs-lookup"><span data-stu-id="6489f-360">![Changing CSS values in the Page Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "Changing CSS values in the Page Inspector")</span></span>

    <span data-ttu-id="6489f-361">*Page Inspector 内で CSS の値を変更します。*</span><span class="sxs-lookup"><span data-stu-id="6489f-361">*Changing CSS values in the Page Inspector*</span></span>
15. <span data-ttu-id="6489f-362">クリックして、**トレース スタイル** タブ、Page Inspector の右側のウィンドウであります。</span><span class="sxs-lookup"><span data-stu-id="6489f-362">Click the **Trace Styles** tab, located in the right pane of Page Inspector.</span></span> <span data-ttu-id="6489f-363">これは、属性の名前順で、選択範囲に適用されるすべてのスタイルを表示する別の方法です。</span><span class="sxs-lookup"><span data-stu-id="6489f-363">This is an alternative way to see all the styles applied to the selection, ordered by attribute name.</span></span>

    <span data-ttu-id="6489f-364">![選択した要素の CSS スタイル トレース](using-page-inspector-in-visual-studio-2012/_static/image36.png "選択した要素の CSS スタイルのトレース")</span><span class="sxs-lookup"><span data-stu-id="6489f-364">![CSS styles tracing of the selected element](using-page-inspector-in-visual-studio-2012/_static/image36.png "CSS styles tracing of the selected element")</span></span>

    <span data-ttu-id="6489f-365">*選択した要素の CSS スタイルのトレース*</span><span class="sxs-lookup"><span data-stu-id="6489f-365">*CSS styles tracing of the selected element*</span></span>
16. <span data-ttu-id="6489f-366">Page Inspector のもう 1 つの機能は、レイアウト ペインです。</span><span class="sxs-lookup"><span data-stu-id="6489f-366">Another feature of Page Inspector is the Layout pane.</span></span> <span data-ttu-id="6489f-367">検査モードを使用して、ナビゲーション バーを選択し、をクリックして、**レイアウト**右側のウィンドウ タブ。</span><span class="sxs-lookup"><span data-stu-id="6489f-367">Using the inspection mode, select the navigation bar and then click the **Layout** tab on the right pane.</span></span> <span data-ttu-id="6489f-368">選択した要素の正確なサイズだけでなく、オフセット、余白、パディング、および境界線のサイズが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-368">You will see the exact size of the selected element, as well as its offset, margin, padding and border size.</span></span> <span data-ttu-id="6489f-369">このビューから値を変更することも確認します。</span><span class="sxs-lookup"><span data-stu-id="6489f-369">Notice that you can also modify the values from this view.</span></span>

    <span data-ttu-id="6489f-370">![Page Inspector 内で要素のレイアウト](using-page-inspector-in-visual-studio-2012/_static/image37.png "Page Inspector 内で要素のレイアウト")</span><span class="sxs-lookup"><span data-stu-id="6489f-370">![Element layout in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image37.png "Element layout in Page Inspector")</span></span>

    <span data-ttu-id="6489f-371">*Page Inspector 内で要素のレイアウト*</span><span class="sxs-lookup"><span data-stu-id="6489f-371">*Element layout in Page Inspector*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a><span data-ttu-id="6489f-372">タスク 2 - 検索して、フォト ギャラリーでのスタイルの問題の解決</span><span class="sxs-lookup"><span data-stu-id="6489f-372">Task 2 - Finding and Fixing Style Issues in the Photo Gallery</span></span>

<span data-ttu-id="6489f-373">以前のバージョンの Visual Studio で Web ページの問題を診断するにはどうは?</span><span class="sxs-lookup"><span data-stu-id="6489f-373">How would you diagnose Web pages issues with previous versions of Visual Studio?</span></span> <span data-ttu-id="6489f-374">可能性の高い経験のある web に Internet Explorer Developer Tools または Firebug など、Visual Studio IDE の外部で実行するデバッグ ツールとしています。</span><span class="sxs-lookup"><span data-stu-id="6489f-374">You are likely familiar with web debugging tools that run outside the Visual Studio IDE, like Internet Explorer Developer Tools or Firebug.</span></span> <span data-ttu-id="6489f-375">ブラウザーでのみ認識、HTML スクリプトとスタイル、基になるフレームワークがレンダリングされる HTML を生成中にします。</span><span class="sxs-lookup"><span data-stu-id="6489f-375">Browsers only understand HTML, scripting and styles, while an underlying framework generates the HTML that will be rendered.</span></span> <span data-ttu-id="6489f-376">そのため、多くの場合、サイト全体のように web ページの外観を確認するを展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6489f-376">For that reason, you often need to deploy the whole site to see how web pages look like.</span></span>

<span data-ttu-id="6489f-377">検出し、web サイトで、問題を解決するには、次の手順をに従ってする必要がある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="6489f-377">You had probably followed these steps when you wanted to detect and fix an issue in your web site:</span></span>

1. <span data-ttu-id="6489f-378">Visual studio でソリューションを実行するか、web サーバー上のページを展開します。</span><span class="sxs-lookup"><span data-stu-id="6489f-378">Run the Solution from Visual Studio, or deploy the page on the web server.</span></span>
2. <span data-ttu-id="6489f-379">ブラウザーで、開発者ツールを使用すると、または単に、ソース コードとスタイルを開きますしようとする問題を一致を開きます。</span><span class="sxs-lookup"><span data-stu-id="6489f-379">In the browser, open the developer tools you use, or simply open the source code and the styles and try to match the issue.</span></span> <span data-ttu-id="6489f-380">ファイルを検索する、関連するを使用した、&quot;検索&quot;または&quot;ファイルで検索&quot;スタイル クラスの名前で機能します。</span><span class="sxs-lookup"><span data-stu-id="6489f-380">To find the files involved, you'd have used the &quot;Search&quot; or &quot;Search in files&quot; features with the name of the style classes.</span></span>
3. <span data-ttu-id="6489f-381">エラーが検出されると、Web ブラウザーとサーバーを停止します。</span><span class="sxs-lookup"><span data-stu-id="6489f-381">Once the error is detected, stop the Web browser and the server.</span></span>
4. <span data-ttu-id="6489f-382">ブラウザーのキャッシュを消去します。</span><span class="sxs-lookup"><span data-stu-id="6489f-382">Clear the browser cache.</span></span>
5. <span data-ttu-id="6489f-383">修正を適用する Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="6489f-383">Return to Visual Studio to apply a fix.</span></span> <span data-ttu-id="6489f-384">テストするすべての手順を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="6489f-384">Repeat all the steps to test.</span></span>

<span data-ttu-id="6489f-385">いいえ ASP.NET WebForms の実数 WYSIWYG が、いくつかのスタイルの問題を実行しているか、展開した後後の段階で検出されました。</span><span class="sxs-lookup"><span data-stu-id="6489f-385">As there is no real WYSIWYG in ASP.NET WebForms, some style issues are detected on a later stage, after running or deploying.</span></span> <span data-ttu-id="6489f-386">ここで、Page Inspector でソリューションを実行しなくても、任意のページをプレビューすることができます。</span><span class="sxs-lookup"><span data-stu-id="6489f-386">Now, with Page Inspector, it is possible to preview any page without running the solution.</span></span>

<span data-ttu-id="6489f-387">このタスクでは、Page inspector を使用して、フォト ギャラリーのアプリケーションのいくつかの問題を修正するためです。</span><span class="sxs-lookup"><span data-stu-id="6489f-387">In this task, you will use the Page inspector for fixing some issues of the Photo Gallery application.</span></span> <span data-ttu-id="6489f-388">次の手順では、検出し、ヘッダーにいくつかの単純なスタイルの問題を迅速に解決します。</span><span class="sxs-lookup"><span data-stu-id="6489f-388">In the following steps, you will detect and quickly fix some simple styling issue in the header.</span></span>

1. <span data-ttu-id="6489f-389">検索ページの検査を使用して、**登録**と**ログで**ヘッダーの左側にリンクします。</span><span class="sxs-lookup"><span data-stu-id="6489f-389">Using Page Inspection, locate the **Register** and the **Log In** links at the left side of the header.</span></span>

    <span data-ttu-id="6489f-390">右側の必要な場所にリンクが表示されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6489f-390">Notice that the link is not displayed at the expected place on the right.</span></span> <span data-ttu-id="6489f-391">それに応じてスタイルを変更して、右へのリンクを整列するされます。</span><span class="sxs-lookup"><span data-stu-id="6489f-391">You will now align the link to the right and restyle it accordingly.</span></span>

    <span data-ttu-id="6489f-392">![左側に配置されているリンク ログイン](using-page-inspector-in-visual-studio-2012/_static/image38.png "左側に配置されているリンク ログイン")</span><span class="sxs-lookup"><span data-stu-id="6489f-392">![Log in link positioned on the left](using-page-inspector-in-visual-studio-2012/_static/image38.png "Log in link positioned on the left")</span></span>

    <span data-ttu-id="6489f-393">*左側に配置されているログのリンク*</span><span class="sxs-lookup"><span data-stu-id="6489f-393">*Log In link positioned on the left*</span></span>
2. <span data-ttu-id="6489f-394">選択されている検査モードの切り替え、ログ内のリンクを開くには、コードを選択します。</span><span class="sxs-lookup"><span data-stu-id="6489f-394">With Toggle Inspection Mode selected, select the Log In link to open its code.</span></span>

    <span data-ttu-id="6489f-395">リンクのソース コードにある通知、 **Site.Master**ファイル、いない場所である、Default.aspx ページで最初に探すことになる以外の場合は、正しいソース ファイルで直接に配置します。</span><span class="sxs-lookup"><span data-stu-id="6489f-395">Notice that the link source code is located in the **Site.Master** file, not in the Default.aspx page which is the place you might look in first place; you have been placed directly in the correct source file.</span></span>
3. <span data-ttu-id="6489f-396">**スタイル** タブを特定し、をクリックして、 **&lt;セクション&gt;#login**項目は、これらのリンクを HTML のコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="6489f-396">In the **Styles** tab, locate and click the **&lt;section&gt; #login** item, which is the HTML container for these links.</span></span>

    <span data-ttu-id="6489f-397">注意して、 **#login**スタイルで自動的にある**Site.css**  をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-397">Notice that the **#login** style is automatically located in **Site.css** after you click.</span></span> <span data-ttu-id="6489f-398">さらに、コードは、ここで強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-398">Moreover, the code is now highlighted.</span></span>

    <span data-ttu-id="6489f-399">![CSS スタイルの選択](using-page-inspector-in-visual-studio-2012/_static/image39.png "CSS スタイルの選択")</span><span class="sxs-lookup"><span data-stu-id="6489f-399">![Selecting the CSS styles](using-page-inspector-in-visual-studio-2012/_static/image39.png "Selecting the CSS styles")</span></span>

    <span data-ttu-id="6489f-400">*CSS スタイルの選択*</span><span class="sxs-lookup"><span data-stu-id="6489f-400">*Selecting the CSS styles*</span></span>
4. <span data-ttu-id="6489f-401">コメントを解除、**テキスト配置**属性の開始タグと終了文字を削除することで強調表示されたコードと保存、 **Site.css**ファイル。</span><span class="sxs-lookup"><span data-stu-id="6489f-401">Uncomment the **text-align** attribute in the highlighted code by removing the opening and closing characters and save the **Site.css** file.</span></span>

    <span data-ttu-id="6489f-402">Page Inspector は、現在のページを構成するすべてのファイルを認識しており、これらのファイルのいずれかが変更を検出できます。</span><span class="sxs-lookup"><span data-stu-id="6489f-402">Page Inspector is aware of all the different files that compose the current page, and it can detect when any of these files change.</span></span> <span data-ttu-id="6489f-403">ブラウザーでは、現在のページがソース ファイルと同期されるたびにユーザーに警告します。</span><span class="sxs-lookup"><span data-stu-id="6489f-403">It alerts you whenever the current page in browser is not in sync with the source files.</span></span>
5. <span data-ttu-id="6489f-404">Page Inspector のブラウザーでは、変更を保存し、ページの再読み込みするには、アドレス バーの下にあるバーをクリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-404">In the Page Inspector browser, click the bar located below the address bar to save the changes and reload the page.</span></span>

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    <span data-ttu-id="6489f-406">*ページの再読み込み*</span><span class="sxs-lookup"><span data-stu-id="6489f-406">*Reloading the page*</span></span>

    <span data-ttu-id="6489f-407">リンクようになりました、右側にあるが、まだ箇条書きリストのように見えます。</span><span class="sxs-lookup"><span data-stu-id="6489f-407">The links are now at the right, but they still look like a bulleted list.</span></span> <span data-ttu-id="6489f-408">ここで、箇条書きを削除し、リンクを水平方向に整列します。</span><span class="sxs-lookup"><span data-stu-id="6489f-408">Now, you will remove the bullets and align the links horizontally.</span></span>

    ![更新されたページ](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    <span data-ttu-id="6489f-410">*更新されたページ*</span><span class="sxs-lookup"><span data-stu-id="6489f-410">*Updated page*</span></span>
6. <span data-ttu-id="6489f-411">いずれかを選択して検査モードを使用して、  **&lt;li&gt;** を含む項目、&quot;登録&quot;と&quot;ログイン&quot;リンクします。</span><span class="sxs-lookup"><span data-stu-id="6489f-411">Using the inspection mode, select any of the **&lt;li&gt;** items that contain the &quot;Register&quot; and &quot;Log in&quot; links.</span></span> <span data-ttu-id="6489f-412">次に、をクリックして、 **&lt;セクション&gt;#login**にアクセスする項目**Styles.css**コード。</span><span class="sxs-lookup"><span data-stu-id="6489f-412">Then, click the **&lt;section&gt; #login** item to access **Styles.css** code.</span></span>

    <span data-ttu-id="6489f-413">![スタイルを検索する](using-page-inspector-in-visual-studio-2012/_static/image42.png "スタイルを検索します。")</span><span class="sxs-lookup"><span data-stu-id="6489f-413">![Finding the style](using-page-inspector-in-visual-studio-2012/_static/image42.png "Finding the style")</span></span>

    <span data-ttu-id="6489f-414">*スタイルを検索します。*</span><span class="sxs-lookup"><span data-stu-id="6489f-414">*Finding the style*</span></span>
7. <span data-ttu-id="6489f-415">**Style.css**、用のコードのコメントを解除**#login li**項目。</span><span class="sxs-lookup"><span data-stu-id="6489f-415">In **Style.css**, uncomment the code for **#login li** items.</span></span> <span data-ttu-id="6489f-416">追加するスタイルの項目を非表示にされ、アイテムの水平方向に表示されます。</span><span class="sxs-lookup"><span data-stu-id="6489f-416">The style you are adding will hide the bullet and display the items horizontally.</span></span>

    <span data-ttu-id="6489f-417">![スタイルの変更、ログイン リンク](using-page-inspector-in-visual-studio-2012/_static/image43.png "ログイン リンク スタイルの変更")</span><span class="sxs-lookup"><span data-stu-id="6489f-417">![Restyling the login links](using-page-inspector-in-visual-studio-2012/_static/image43.png "Restyling the login links")</span></span>

    <span data-ttu-id="6489f-418">*ログイン リンク スタイルの変更*</span><span class="sxs-lookup"><span data-stu-id="6489f-418">*Restyling the login links*</span></span>
8. <span data-ttu-id="6489f-419">保存**Style.css**ファイルおよびページの再読み込みするアドレスの下にあるバーを 1 回クリックします。</span><span class="sxs-lookup"><span data-stu-id="6489f-419">Save **Style.css** file and click once on the bar located below the address to reload the page.</span></span> <span data-ttu-id="6489f-420">リンクが正しく表示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6489f-420">Notice that the links appear correctly.</span></span>

    <span data-ttu-id="6489f-421">![リンクが右側に整列](using-page-inspector-in-visual-studio-2012/_static/image44.png "へのリンクが、右側に配置")</span><span class="sxs-lookup"><span data-stu-id="6489f-421">![Links aligned to the right side](using-page-inspector-in-visual-studio-2012/_static/image44.png "Links aligned to the right side")</span></span>

    <span data-ttu-id="6489f-422">*右揃えで配置へのリンク*</span><span class="sxs-lookup"><span data-stu-id="6489f-422">*Links aligned to the right side*</span></span>
9. <span data-ttu-id="6489f-423">最後に、ヘッダーのタイトルを変更します。</span><span class="sxs-lookup"><span data-stu-id="6489f-423">Finally, you will change the header title.</span></span> <span data-ttu-id="6489f-424">検索する代わりに、'**ここにロゴ '**すべてのファイル内のテキストでは、検査モードを使用してテキストをクリックし、生成されたソース コードを取得します。</span><span class="sxs-lookup"><span data-stu-id="6489f-424">Instead of searching for the '**your logo here'** text in all the files, use the inspection mode to click the text and get to source code that generates it.</span></span>

    <span data-ttu-id="6489f-425">![サイトのタイトルを検索する](using-page-inspector-in-visual-studio-2012/_static/image45.png "サイトのタイトルを検索します。")</span><span class="sxs-lookup"><span data-stu-id="6489f-425">![Finding the site title](using-page-inspector-in-visual-studio-2012/_static/image45.png "Finding the site title")</span></span>

    <span data-ttu-id="6489f-426">*サイトのタイトルを検索します。*</span><span class="sxs-lookup"><span data-stu-id="6489f-426">*Finding the site title*</span></span>
10. <span data-ttu-id="6489f-427">使用するようになりました**Site.Master**、置換、'**ここにロゴ**'にテキスト'**フォト ギャラリー**' です。</span><span class="sxs-lookup"><span data-stu-id="6489f-427">Now you are in **Site.Master**, replace the '**your logo here**' text with '**Photo Gallery**'.</span></span> <span data-ttu-id="6489f-428">保存して Page Inspector のブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="6489f-428">Save and update the Page Inspector browser.</span></span>

    <span data-ttu-id="6489f-429">![フォト ギャラリー ページで更新](using-page-inspector-in-visual-studio-2012/_static/image46.png "更新フォト ギャラリー ページ")</span><span class="sxs-lookup"><span data-stu-id="6489f-429">![Photo Gallery page updated](using-page-inspector-in-visual-studio-2012/_static/image46.png "Photo Gallery page updated")</span></span>

    <span data-ttu-id="6489f-430">*フォト ギャラリー ページが更新されました*</span><span class="sxs-lookup"><span data-stu-id="6489f-430">*Photo Gallery page updated*</span></span>
11. <span data-ttu-id="6489f-431">最後にキーを押して**f5 キーを押して**アプリを実行する、チェック アウトすべて変更が想定どおりに動作します。</span><span class="sxs-lookup"><span data-stu-id="6489f-431">Finally press **F5** to run the app the check out all the changes work as expected.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="6489f-432">概要</span><span class="sxs-lookup"><span data-stu-id="6489f-432">Summary</span></span>

<span data-ttu-id="6489f-433">このハンズオン ラボを完了すると、Page Inspector を使用して、再構築し、ブラウザーで Web サイトを実行することがなく、Web アプリケーションをプレビューする方法を習得がします。</span><span class="sxs-lookup"><span data-stu-id="6489f-433">By completing this Hands-On Lab, you have learnt how to use Page Inspector to preview your Web application without having to rebuild and run the Web site in a browser.</span></span> <span data-ttu-id="6489f-434">さらに、すばやく発見し、ソース コードに表示される出力から直接アクセスすることでバグを修正する方法を習得がします。</span><span class="sxs-lookup"><span data-stu-id="6489f-434">In addition, you have learnt how to quickly find and fix bugs by accessing directly from the rendered output to the source code.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="6489f-435">付録 a: をインストールする Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="6489f-435">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="6489f-436">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="6489f-436">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="6489f-437">次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。</span><span class="sxs-lookup"><span data-stu-id="6489f-437">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="6489f-438">移動して[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。</span><span class="sxs-lookup"><span data-stu-id="6489f-438">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="6489f-439">または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; *Visual Studio Express 2012 for Web と Windows Azure SDK*&quot;です。</span><span class="sxs-lookup"><span data-stu-id="6489f-439">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="6489f-440">をクリックして**を今すぐインストール**です。</span><span class="sxs-lookup"><span data-stu-id="6489f-440">Click on **Install Now**.</span></span> <span data-ttu-id="6489f-441">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="6489f-441">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="6489f-442">1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="6489f-442">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="6489f-443">![Visual Studio Express インストール](using-page-inspector-in-visual-studio-2012/_static/image47.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="6489f-443">![Install Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="6489f-444">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="6489f-444">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="6489f-445">すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="6489f-445">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    <span data-ttu-id="6489f-447">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="6489f-447">*Accepting the license terms*</span></span>
5. <span data-ttu-id="6489f-448">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="6489f-448">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    <span data-ttu-id="6489f-450">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="6489f-450">*Installation progress*</span></span>
6. <span data-ttu-id="6489f-451">インストールが完了したらをクリックして**完了**です。</span><span class="sxs-lookup"><span data-stu-id="6489f-451">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    <span data-ttu-id="6489f-453">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="6489f-453">*Installation completed*</span></span>
7. <span data-ttu-id="6489f-454">をクリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="6489f-454">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="6489f-455">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="6489f-455">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web タイルを](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    <span data-ttu-id="6489f-457">*VS Express Web タイルを*</span><span class="sxs-lookup"><span data-stu-id="6489f-457">*VS Express for Web tile*</span></span>
