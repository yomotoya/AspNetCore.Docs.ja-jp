---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'ハンズ オン ラボ: Visual Studio 2013 の Web Tools |Microsoft ドキュメント'
author: rick-anderson
description: Visual Studio とは、優れた開発環境です。NET ベースの Windows と web プロジェクトです。 簡単に使用できる強力なテキスト エディターが含まれています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="72c4b-104">ハンズ オン ラボ: Visual Studio 2013 の Web ツール</span><span class="sxs-lookup"><span data-stu-id="72c4b-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="72c4b-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="72c4b-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="72c4b-106">Web キャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="72c4b-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="72c4b-107">Visual Studio とは、優れた開発環境です。NET ベースの Windows と web プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="72c4b-108">これには、プロジェクトなしスタンドアロン ファイルを編集して簡単に使用できる強力なテキスト エディターが含まれます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="72c4b-109">Visual Studio は、各ファイルを編集すると、フル機能の解析ツリーを保持します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="72c4b-110">これにより、Visual Studio をはるかに高速でより快適な開発作業をするときにこれまでにないオートコンプリートとドキュメントに基づくアクションを提供します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="72c4b-111">これらの機能は、HTML および CSS のドキュメントで特に強力です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="72c4b-112">この電源のすべても、ニーズに合わせて編集者には、強力な新機能を拡張する簡素化する、拡張機能の使用可能なできます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="72c4b-113">Web Essentials は、(ほとんどの場合) web 関連の機能強化 Visual Studio のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="72c4b-114">(CSS) のでは特に新しい IntelliSense 入力候補、Browser Link の新機能、および自動の多くが含まれている JSHint の JavaScript ファイル、HTML、CSS、および最新の web 開発に不可欠なその他の多くの機能に対する新しい警告します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="72c4b-115">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="72c4b-116">概要</span><span class="sxs-lookup"><span data-stu-id="72c4b-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="72c4b-117">目的</span><span class="sxs-lookup"><span data-stu-id="72c4b-117">Objectives</span></span>

<span data-ttu-id="72c4b-118">このハンズオン ラボでは学習する方法。</span><span class="sxs-lookup"><span data-stu-id="72c4b-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="72c4b-119">豊富な HTML5 のコード スニペットと全角コーディングなど Web Essentials に含まれる HTML エディターの新機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="72c4b-120">カラー ピッカーやブラウザー マトリックス ツールヒントなどの Web Essentials に含まれる CSS エディターの新機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="72c4b-121">すべての HTML 要素の IntelliSense およびファイルへの展開などの Web Essentials に含まれる JavaScript エディターの新機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="72c4b-122">ブラウザーとブラウザー リンクを使用して Visual Studio の間での Exchange データ</span><span class="sxs-lookup"><span data-stu-id="72c4b-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="72c4b-123">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="72c4b-123">Prerequisites</span></span>

<span data-ttu-id="72c4b-124">このハンズオン ラボを完了する、次が必要。</span><span class="sxs-lookup"><span data-stu-id="72c4b-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="72c4b-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/)以上</span><span class="sxs-lookup"><span data-stu-id="72c4b-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="72c4b-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="72c4b-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="72c4b-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="72c4b-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="72c4b-128">セットアップ</span><span class="sxs-lookup"><span data-stu-id="72c4b-128">Setup</span></span>

<span data-ttu-id="72c4b-129">このハンズオン ラボでは、演習を実行するのには、最初に、環境を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="72c4b-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="72c4b-130">Windows エクスプ ローラー ウィンドウを開き、ラボを参照**ソース**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="72c4b-131">右クリック**Setup.cmd**選択**管理者として実行**を環境を構成して、このラボ用の Visual Studio のコード スニペットをインストールするセットアップ プロセスを起動します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="72c4b-132">ユーザー アカウント制御ダイアログ ボックスが表示されている場合は、続行するアクションを確認します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="72c4b-133">セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="72c4b-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="72c4b-134">コード スニペットを使用します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-134">Using the Code Snippets</span></span>

<span data-ttu-id="72c4b-135">ラボ ドキュメント全体にわたって、コード ブロックを挿入するように指示されます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="72c4b-136">便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを回避する Visual Studio 2013 内からアクセスできるとして提供されます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="72c4b-137">各演習にある開始ソリューションが組み込まれた、**開始**個別に各手順をたどることによってこの作業のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="72c4b-138">実習では、追加のコード スニペットがこれらのソリューションの開始から不足しており、演習を完了するまで動作しない可能性がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="72c4b-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="72c4b-139">演習では、ソース コード、内部も表示されます、**終了**を対応する手順の手順を実行した結果のコードの Visual Studio ソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="72c4b-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="72c4b-140">このハンズオン ラボを使用すると、追加のヘルプを必要がある場合は、これらのソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="72c4b-141">演習</span><span class="sxs-lookup"><span data-stu-id="72c4b-141">Exercises</span></span>

<span data-ttu-id="72c4b-142">このハンズオン ラボには、次の実習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="72c4b-143">Browser Link と Web Essentials の使用</span><span class="sxs-lookup"><span data-stu-id="72c4b-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="72c4b-144">コード スニペットと IntelliSense の利用</span><span class="sxs-lookup"><span data-stu-id="72c4b-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="72c4b-145">Visual Studio を初めて起動するときに定義済みの設定のコレクションの 1 つを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="72c4b-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="72c4b-146">定義済みの各コレクションは、特定の開発スタイルと一致するものではまた、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペットでは、およびダイアログ ボックスのオプションを判断します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="72c4b-147">このラボの手順を使用する場合は、Visual Studio での特定のタスクを実行に必要な操作を記述する、**全般的な開発設定**コレクション。</span><span class="sxs-lookup"><span data-stu-id="72c4b-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="72c4b-148">開発環境に合わせてさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。</span><span class="sxs-lookup"><span data-stu-id="72c4b-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="72c4b-149">Browser Link と Web Essentials 演習 1: を使用します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="72c4b-150">**Web Essentials**は、さまざまな最新の web 開発では、はるかに高速でより快適な web 開発作業を行うに焦点を合わせた便利な機能を追加する Visual Studio 拡張機能です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="72c4b-151">Visual Studio の拡張機能ギャラリーから Web Essentials をインストールすることができます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="72c4b-152">**Browser Link** web アプリケーションと Visual Studio の間でデータを交換するには、Visual Studio IDE と任意の開いているブラウザー間のチャネルを提供する Visual Studio 2013 に含まれる新機能です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="72c4b-153">Web Essentials は、DOM オブジェクト モデルと、ブラウザーから直接 web ページの CSS スタイルを操作するツールで Browser Link を拡張します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="72c4b-154">この演習では調査でサポートされる機能のいくつか**Web Essentials**と**Browser Link**クイズの単純なページを強化するためにします。</span><span class="sxs-lookup"><span data-stu-id="72c4b-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="72c4b-155">タスク 1 - 複数のブラウザーでプロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="72c4b-156">このタスクでは、クロス ブラウザー テスト用に便利ですが、一度に複数のブラウザーで実行する web アプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="72c4b-157">開いている**Microsoft Visual Studio**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="72c4b-158">**ファイル**メニューの**開く |プロジェクト/ソリューションしています.**を見つけて**Ex1 WorkingwithBrowserLinkandWebEssentials\Begin**で、**ソース**ラボ (C:\WebCampsTK\HOL\VSWebTooling\Source) のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="72c4b-159">選択**Begin.sln**をクリック**開く**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="72c4b-160">Visual Studio ツールバーで、[ブラウザー] メニューを展開し、選択**を参照しています.**.</span><span class="sxs-lookup"><span data-stu-id="72c4b-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="72c4b-161">![メニュー オプション browse With](visual-studio-2013-web-tools/_static/image1.png "ブラウザー メニュー... 参照")</span><span class="sxs-lookup"><span data-stu-id="72c4b-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="72c4b-162">*Browse With メニュー オプション*</span><span class="sxs-lookup"><span data-stu-id="72c4b-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="72c4b-163">**Browse With**ダイアログ ボックスの両方をオン**Google Chrome**と**Internet Explorer**押したまま、 **CTRL**キーおよびをクリックして**既定値として設定**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="72c4b-164">![ダイアログ ボックスで [参照]](visual-studio-2013-web-tools/_static/image2.png "ダイアログ ボックスで [参照]")</span><span class="sxs-lookup"><span data-stu-id="72c4b-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="72c4b-165">*複数の既定ブラウザーを選択します。*</span><span class="sxs-lookup"><span data-stu-id="72c4b-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="72c4b-166">既定のブラウザーとして Google Chrome と Internet Explorer の両方が表示されます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="72c4b-167">をクリックして**キャンセル**ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="72c4b-168">![Google Chrome と既定のブラウザーとして Internet Explorer](visual-studio-2013-web-tools/_static/image3.png "Google Chrome と Internet Explorer の既定のブラウザー")</span><span class="sxs-lookup"><span data-stu-id="72c4b-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="72c4b-169">*Google Chrome と既定のブラウザーとして Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="72c4b-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="72c4b-170">既定のブラウザーを構成した後、**複数のブラウザー**ブラウザー メニューのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="72c4b-171">![複数のブラウザー](visual-studio-2013-web-tools/_static/image4.png "複数のブラウザー")</span><span class="sxs-lookup"><span data-stu-id="72c4b-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="72c4b-172">キーを押して**ctrl キーを押し** + **f5 キーを押して**デバッグを使わないアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="72c4b-173">両方のブラウザー ウィンドウを開く場合は、同時に両方のブラウザーで、更新プログラムを確認するために上、他のうちの 1 つを配置します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="72c4b-174">ブラウザーには、明るい青色の四角形と web ページを表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="72c4b-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="72c4b-175">![上の 1 つのブラウザーを配置する](visual-studio-2013-web-tools/_static/image5.png "上、他の 1 つのブラウザーを配置します。")</span><span class="sxs-lookup"><span data-stu-id="72c4b-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="72c4b-176">*上下に並べて配置する 1 つのブラウザー*</span><span class="sxs-lookup"><span data-stu-id="72c4b-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="72c4b-177">ブラウザーを閉じないでください。</span><span class="sxs-lookup"><span data-stu-id="72c4b-177">Do not close the browsers.</span></span> <span data-ttu-id="72c4b-178">次のタスクでは、それらを使用します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="72c4b-179">タスク 2 - HTML 要素を作成するコードを使用して全角</span><span class="sxs-lookup"><span data-stu-id="72c4b-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="72c4b-180">**全角コーディング**エディターは、高速な HTML、XML、XSL (またはその他の構造化されたコード形式) 用のプラグインのコーディングおよび編集します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="72c4b-181">このプラグインのコアとは、HTML コードに - CSS セレクターのように式を展開することができますが、強力な省略形エンジンです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="72c4b-182">全角のコーディングは、HTML、CSS を使用するスタイルのセレクターの構文を記述する高速な方法です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="72c4b-183">この演習では機能を使用する、全角コーディング Web Essentials によって提供される質問のオプションを表す HTML ボタンを生成します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="72c4b-184">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="72c4b-185">開く、 **Index.cshtml**にあるファイル、**ビュー** | **ホーム**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="72c4b-186">置換、  **&lt;!--TODO: ここでのオプションを追加&gt;**にコメントを次のコード、およびキーを押して**タブ**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="72c4b-187">コードは、HTML を拡張する必要があります。</span><span class="sxs-lookup"><span data-stu-id="72c4b-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="72c4b-188">![HTML 拡張](visual-studio-2013-web-tools/_static/image6.png "HTML の展開")</span><span class="sxs-lookup"><span data-stu-id="72c4b-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="72c4b-189">*展開された HTML*</span><span class="sxs-lookup"><span data-stu-id="72c4b-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="72c4b-190">全角のコーディング構文の詳細については、次を参照してください。[記事](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="72c4b-191">クリックして、**リンクされているブラウザーを更新**両方のブラウザーを更新するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="72c4b-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="72c4b-192">![リンクされているブラウザーを更新](visual-studio-2013-web-tools/_static/image7.png "リンクされているブラウザーを更新")</span><span class="sxs-lookup"><span data-stu-id="72c4b-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="72c4b-193">*リンクされているブラウザーを更新します。*</span><span class="sxs-lookup"><span data-stu-id="72c4b-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="72c4b-194">![Internet Explorer のページは 4 つのボタンで更新](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer のページは 4 つのボタンで更新")</span><span class="sxs-lookup"><span data-stu-id="72c4b-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="72c4b-195">*Internet Explorer のページは 4 つのボタンで更新*</span><span class="sxs-lookup"><span data-stu-id="72c4b-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="72c4b-196">![Google Chrome のページは 4 つのボタンで更新](visual-studio-2013-web-tools/_static/image9.png "Google Chrome のページは 4 つのボタンで更新")</span><span class="sxs-lookup"><span data-stu-id="72c4b-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="72c4b-197">*Google Chrome のページは 4 つのボタンで更新*</span><span class="sxs-lookup"><span data-stu-id="72c4b-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="72c4b-198">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="72c4b-199">ページに、ボタンを追加したが、テンプレート質問を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="72c4b-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="72c4b-200">これを行うで使用する新しい機能と呼ばれる Web Essentials **Lorem Ipsum ジェネレーター**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="72c4b-201">検索、 **div**を持つ要素、**クラス**属性**前面**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="72c4b-202">最初の子要素として次のコードを追加、 **div**、キーを押します**タブ**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="72c4b-203">コードは、HTML を拡張する必要があります。</span><span class="sxs-lookup"><span data-stu-id="72c4b-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="72c4b-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span><span class="sxs-lookup"><span data-stu-id="72c4b-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="72c4b-205">*Lorem Ipsum 自動生成されました。*</span><span class="sxs-lookup"><span data-stu-id="72c4b-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="72c4b-206">全角のコーディングの一環として、HTML エディターで直接 Lorem Ipsum コードを生成できます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="72c4b-207">入力するだけ**lorem**ヒットと**タブ**し、30 Lorem Ipsum テキストが挿入されます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="72c4b-208">たとえば、</span><span class="sxs-lookup"><span data-stu-id="72c4b-208">E.g.</span></span> <span data-ttu-id="72c4b-209">*lorem10* 10 Lorem Ipsum 単語を挿入します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="72c4b-210">Web essentials と呼ばれる別の新機能を使用して、質問の上部にロゴを追加する**Lorem ピクセル ジェネレーター**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="72c4b-211">最初の子要素として次のコードを追加、 **div**を持つ要素**コンテナー**として**クラス**値、およびキーを押して**タブ**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="72c4b-212">コードは、HTML を展開します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="72c4b-213">![Lorem ピクセル自動生成された](visual-studio-2013-web-tools/_static/image11.png "Lorem ピクセルの自動生成")</span><span class="sxs-lookup"><span data-stu-id="72c4b-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="72c4b-214">*Lorem ピクセルの自動生成*</span><span class="sxs-lookup"><span data-stu-id="72c4b-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="72c4b-215">全角のコーディングの一環として、HTML エディターで直接 Lorem ピクセルのコードを生成することもできます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="72c4b-216">入力するだけ**pix 200 x 200-動物**とヒット**タブ**と**img**動物の 200 x 200 イメージからのタグが挿入されます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="72c4b-217">詳細についてを参照してください[Lorem ピクセル](http://www.lorempixel.com)です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="72c4b-218">クリックして、**リンクされているブラウザーを更新**両方のブラウザーを更新するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="72c4b-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="72c4b-219">![Internet Explorer の自動生成されたイメージとテキスト](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer の自動生成されたイメージとテキスト")</span><span class="sxs-lookup"><span data-stu-id="72c4b-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="72c4b-220">*Internet Explorer の自動生成されたイメージとテキスト*</span><span class="sxs-lookup"><span data-stu-id="72c4b-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="72c4b-221">![Google Chrome の自動生成されたイメージとテキスト](visual-studio-2013-web-tools/_static/image13.png "Google Chrome の自動生成されたイメージとテキスト")</span><span class="sxs-lookup"><span data-stu-id="72c4b-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="72c4b-222">*Google Chrome の自動生成されたイメージとテキスト*</span><span class="sxs-lookup"><span data-stu-id="72c4b-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="72c4b-223">コード スニペットを追加するときに、イメージをランダムに選択、ためのブラウザーで表示されるイメージが異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="72c4b-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="72c4b-224">ブラウザーを閉じないでください。</span><span class="sxs-lookup"><span data-stu-id="72c4b-224">Do not close the browsers.</span></span> <span data-ttu-id="72c4b-225">次のタスクでは、それらを使用します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="72c4b-226">タスク 3 - スタイル プロパティの更新</span><span class="sxs-lookup"><span data-stu-id="72c4b-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="72c4b-227">このタスクでは、ブラウザーのリンクを使用して**検査モード**特定の DOM 要素が生成される正確な場所を検出し、Web で提供されるカラー ピッカーを使用してその要素の色のプロパティを更新する機能Essentials です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="72c4b-228">Internet Explorer ブラウザーでキーを押して**ctrl キーを押し** + **ALT** + **すれば**検査モードを有効にします。</span><span class="sxs-lookup"><span data-stu-id="72c4b-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="72c4b-229">明るい青色の枠線でポインターを移動し、をクリックします。</span><span class="sxs-lookup"><span data-stu-id="72c4b-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="72c4b-230">![明るい青色の枠線にポインターを移動](visual-studio-2013-web-tools/_static/image14.png "明るい青色の枠線にポインターを移動")</span><span class="sxs-lookup"><span data-stu-id="72c4b-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="72c4b-231">*明るい青色の枠線にポインターを移動*</span><span class="sxs-lookup"><span data-stu-id="72c4b-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="72c4b-232">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="72c4b-233">Visual Studio HTML エディターで、ブラウザーで選択されている HTML 要素を選択するも注目してください。</span><span class="sxs-lookup"><span data-stu-id="72c4b-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="72c4b-234">![Visual Studio HTML エディターで選択されている HTML 要素](visual-studio-2013-web-tools/_static/image15.png "Visual Studio HTML エディターで選択されている HTML 要素")</span><span class="sxs-lookup"><span data-stu-id="72c4b-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="72c4b-235">*Visual Studio HTML エディターで選択されている HTML 要素*</span><span class="sxs-lookup"><span data-stu-id="72c4b-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="72c4b-236">今すぐ更新するが、**前面**選択した要素のスタイルを変更するために CSS クラス。</span><span class="sxs-lookup"><span data-stu-id="72c4b-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="72c4b-237">これを行うには、キーを押します。 **CTRL** + **、**を開くには、**へ移動**検索ボックス。</span><span class="sxs-lookup"><span data-stu-id="72c4b-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="72c4b-238">型**site.css**とキーを押します**ENTER**ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="72c4b-239">![Site.css ファイルを開く](visual-studio-2013-web-tools/_static/image16.png "Site.css ファイルを開く")</span><span class="sxs-lookup"><span data-stu-id="72c4b-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="72c4b-240">*Site.css ファイルを開く*</span><span class="sxs-lookup"><span data-stu-id="72c4b-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="72c4b-241">キーを押して**CTRL** + **F**および種類**.flip コンテナー .front** CSS セレクターを検索します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="72c4b-242">クラスを開くには、カラー ピッカーの枠線のプロパティで明るい青色の四角形をクリックします。</span><span class="sxs-lookup"><span data-stu-id="72c4b-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="72c4b-243">![カラー ピッカーを開いて](visual-studio-2013-web-tools/_static/image17.png "カラー ピッカーを開く")</span><span class="sxs-lookup"><span data-stu-id="72c4b-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="72c4b-244">*カラー ピッカーを開く*</span><span class="sxs-lookup"><span data-stu-id="72c4b-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="72c4b-245">シェブロンをクリックして、カラー ピッカーを展開し、新しい色を選択します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="72c4b-246">![カラー ピッカーを展開する](visual-studio-2013-web-tools/_static/image18.png "カラー ピッカーを展開します。")</span><span class="sxs-lookup"><span data-stu-id="72c4b-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="72c4b-247">*カラー ピッカーを展開します。*</span><span class="sxs-lookup"><span data-stu-id="72c4b-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="72c4b-248">キーを押して**ctrl キーを押し** + **alt キーを押し** + **ENTER**にリンクされているブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="72c4b-249">Internet Explorer に切り替え、罫線の色がどのように変更されたかに注意してください。</span><span class="sxs-lookup"><span data-stu-id="72c4b-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="72c4b-250">![Internet Explorer の更新の罫線の色](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer の更新の罫線の色")</span><span class="sxs-lookup"><span data-stu-id="72c4b-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="72c4b-251">*インターネット エクスプ ローラー - 更新の罫線の色*</span><span class="sxs-lookup"><span data-stu-id="72c4b-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="72c4b-252">Google Chrome に切り替え、罫線の色がどのように変更されたかに注意してください。</span><span class="sxs-lookup"><span data-stu-id="72c4b-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="72c4b-253">![Google Chrome の更新の罫線の色](visual-studio-2013-web-tools/_static/image20.png "Google Chrome の更新の罫線の色")</span><span class="sxs-lookup"><span data-stu-id="72c4b-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="72c4b-254">*Google Chrome の更新の罫線の色*</span><span class="sxs-lookup"><span data-stu-id="72c4b-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="72c4b-255">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="72c4b-256">末尾に移動して、 **Site.css**ファイルとキーを押して**CTRL** + **F**を検索する、 **.btn**セレクター。</span><span class="sxs-lookup"><span data-stu-id="72c4b-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="72c4b-257">注意して、 **- webkit 罫線半径**プロパティは、緑色の下線が引かれます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="72c4b-258">![btn セレクターの - webkit 罫線半径プロパティ](visual-studio-2013-web-tools/_static/image21.png "btn セレクターの - webkit 罫線半径プロパティ")</span><span class="sxs-lookup"><span data-stu-id="72c4b-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="72c4b-259">*btn セレクターの - webkit 罫線半径プロパティ*</span><span class="sxs-lookup"><span data-stu-id="72c4b-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="72c4b-260">内のキャレットを配置、 **- webkit 罫線半径**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="72c4b-261">プロパティの最初の単語の最初の文字の下にある青い線が表示されます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="72c4b-262">これは、**スマート タグ**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="72c4b-263">キーを押して**CTRL** + **です。**</span><span class="sxs-lookup"><span data-stu-id="72c4b-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="72c4b-264">提案メニューを開き、をクリックする**(境界線 radius) の標準的なプロパティがありません追加**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="72c4b-265">![追加の標準的なプロパティの修正案がありません](visual-studio-2013-web-tools/_static/image22.png "追加の標準的なプロパティの修正案がありません")</span><span class="sxs-lookup"><span data-stu-id="72c4b-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="72c4b-266">*追加の標準的なプロパティの修正案がありません。*</span><span class="sxs-lookup"><span data-stu-id="72c4b-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="72c4b-267">**罫線 radius**プロパティは、CSS 規則を自動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="72c4b-268">![標準的なプロパティが追加されたありません](visual-studio-2013-web-tools/_static/image23.png "追加標準的なプロパティがありません")</span><span class="sxs-lookup"><span data-stu-id="72c4b-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="72c4b-269">*追加する標準的なプロパティがありません。*</span><span class="sxs-lookup"><span data-stu-id="72c4b-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="72c4b-270">ポインターを移動、**罫線 radius**プロパティを表示、**ブラウザー マトリックス ツールヒント**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="72c4b-271">**ブラウザー マトリックス ツールヒント**各ブラウザーで、プロパティの可用性を示します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="72c4b-272">![ブラウザーのマトリックス ツールヒント](visual-studio-2013-web-tools/_static/image24.png "ブラウザー マトリックスのツールヒント")</span><span class="sxs-lookup"><span data-stu-id="72c4b-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="72c4b-273">*ブラウザーのマトリックスのツールヒント*</span><span class="sxs-lookup"><span data-stu-id="72c4b-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="72c4b-274">注意しての値、**罫線 radius**プロパティがまだ下線付きです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="72c4b-275">警告メッセージを表示する値を超える、ポインターを移動します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="72c4b-276">![罫線は、radius プロパティ値の警告](visual-studio-2013-web-tools/_static/image25.png "border-radius プロパティ値の警告")</span><span class="sxs-lookup"><span data-stu-id="72c4b-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="72c4b-277">*罫線は、radius プロパティ値の警告*</span><span class="sxs-lookup"><span data-stu-id="72c4b-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="72c4b-278">ユニットを削除して、**罫線 radius**ツールヒントによって指定されたプロパティの値。</span><span class="sxs-lookup"><span data-stu-id="72c4b-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="72c4b-279">として**罫線 radius**コーナーは、削除することができます丸みのある方法の境界を定義するための標準的なプロパティ、 **- webkit 罫線半径**プロパティと、CSS 規則からの値。</span><span class="sxs-lookup"><span data-stu-id="72c4b-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="72c4b-280">内のキャレットを配置、**ワード ラップ**プロパティとがスマート タグは、以下も表示されます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="72c4b-281">メニューを開き、をクリックして**不足しているベンダー固有の追加**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="72c4b-282">![不足している仕入先の具体的な候補を追加](visual-studio-2013-web-tools/_static/image26.png "不足している仕入先の具体的な修正候補の追加")</span><span class="sxs-lookup"><span data-stu-id="72c4b-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="72c4b-283">*不足している仕入先の具体的な修正候補を追加します。*</span><span class="sxs-lookup"><span data-stu-id="72c4b-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="72c4b-284">**Ms 折り返し**プロパティは、CSS 規則を自動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="72c4b-285">![ベンダー固有のプロパティが追加](visual-studio-2013-web-tools/_static/image27.png "ベンダー固有のプロパティを追加")</span><span class="sxs-lookup"><span data-stu-id="72c4b-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="72c4b-286">*ベンダー固有のプロパティを追加*</span><span class="sxs-lookup"><span data-stu-id="72c4b-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="72c4b-287">タスク 4 - ブラウザーからの HTML コードの更新</span><span class="sxs-lookup"><span data-stu-id="72c4b-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="72c4b-288">このタスクでは、ブラウザーのリンクを使用して**デザイン モード**ブラウザーからの DOM オブジェクトを編集し、Visual Studio での HTML ソース ファイルに変更を転送する機能。</span><span class="sxs-lookup"><span data-stu-id="72c4b-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="72c4b-289">Google Chrome でキーを押して**CTRL** + **ALT** + **D**デザイン モードを有効にします。</span><span class="sxs-lookup"><span data-stu-id="72c4b-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="72c4b-290">ポインターを移動、 **Lorem Ipsum dolor sit amet**にラベルを付けるし、をクリックします。</span><span class="sxs-lookup"><span data-stu-id="72c4b-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="72c4b-291">![質問を編集](visual-studio-2013-web-tools/_static/image28.png "質問の編集")</span><span class="sxs-lookup"><span data-stu-id="72c4b-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="72c4b-292">*質問の編集*</span><span class="sxs-lookup"><span data-stu-id="72c4b-292">*Editing the question*</span></span>
3. <span data-ttu-id="72c4b-293">カーソルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-293">A cursor should appear.</span></span> <span data-ttu-id="72c4b-294">元のテキストを置換*どのそのような長い質問を書き込むときにしますか?*、キーを押します**ESC**デザイン モードを終了します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="72c4b-295">![編集質問](visual-studio-2013-web-tools/_static/image29.png "質問の編集")</span><span class="sxs-lookup"><span data-stu-id="72c4b-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="72c4b-296">*質問の編集*</span><span class="sxs-lookup"><span data-stu-id="72c4b-296">*Question edited*</span></span>
4. <span data-ttu-id="72c4b-297">Visual Studio に戻り、開いているスイッチ**Index.cshtml**開いていない場合、します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="72c4b-298">注意しての内部テキ スト、 **&lt;p&gt;**要素が更新されました。</span><span class="sxs-lookup"><span data-stu-id="72c4b-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="72c4b-299">![HTML ページに更新された質問](visual-studio-2013-web-tools/_static/image30.png "HTML ページに最新の質問")</span><span class="sxs-lookup"><span data-stu-id="72c4b-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="72c4b-300">*HTML ページに更新された質問*</span><span class="sxs-lookup"><span data-stu-id="72c4b-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="72c4b-301">タスク 5 - 変更履歴の SEO に関連する警告</span><span class="sxs-lookup"><span data-stu-id="72c4b-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="72c4b-302">**検索エンジン最適化**(SEO) は、結果の検索エンジンの一覧に高いランクが web サイトを行うプロセスです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="72c4b-303">サイトが順位付けが高いほどより常に表示されている複数訪問者サイトは、その検索エンジンから取得されます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="72c4b-304">Web Essentials には、HTML を調査する分析ツールが組み込まれており、レポートの問題を見つけて対処支援を提供します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="72c4b-305">移動して、**ビュー**メニューをクリックして**エラー一覧**を開くには、**エラー一覧**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="72c4b-306">![エラーをビューに一覧表示メニューの [](visual-studio-2013-web-tools/_static/image31.png "表示] メニューのエラー一覧")</span><span class="sxs-lookup"><span data-stu-id="72c4b-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="72c4b-307">*エラーをビューに一覧表示メニュー*</span><span class="sxs-lookup"><span data-stu-id="72c4b-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="72c4b-308">通知を SEO 警告があることに注意してください、 **&lt;メタ&gt;**タグ ページの説明がありません。</span><span class="sxs-lookup"><span data-stu-id="72c4b-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="72c4b-309">これを修正する SEO 警告エントリをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="72c4b-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="72c4b-310">![エラー一覧ウィンドウ](visual-studio-2013-web-tools/_static/image32.png "エラー一覧ウィンドウ")</span><span class="sxs-lookup"><span data-stu-id="72c4b-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="72c4b-311">*エラー一覧ウィンドウ*</span><span class="sxs-lookup"><span data-stu-id="72c4b-311">*Error List window*</span></span>
3. <span data-ttu-id="72c4b-312">**Web Essentials**ダイアログ ボックスで、をクリックして**はい**説明を挿入する&lt;メタ&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="72c4b-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="72c4b-313">![Web Essentials ダイアログ ボックス](visual-studio-2013-web-tools/_static/image33.png "Web Essentials ダイアログ ボックス")</span><span class="sxs-lookup"><span data-stu-id="72c4b-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="72c4b-314">*Web Essentials ダイアログ ボックス*</span><span class="sxs-lookup"><span data-stu-id="72c4b-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="72c4b-315">用のエディター **\_Layout.cshtml**開きます、 **&lt;メタ&gt;**にタグが自動的に追加、**ヘッド**のセクション、HTML ファイルです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="72c4b-316">![_Layout ページに自動的に追加のメタ タグ](visual-studio-2013-web-tools/_static/image34.png "_Layout ページに自動的に追加のメタ タグ")</span><span class="sxs-lookup"><span data-stu-id="72c4b-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="72c4b-317">*自動的に追加のメタ タグ\_レイアウト ページ*</span><span class="sxs-lookup"><span data-stu-id="72c4b-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="72c4b-318">値を変更、**コンテンツ**属性を*GeekQuiz*ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="72c4b-319">手順 2: コード スニペットおよび活用 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="72c4b-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="72c4b-320">Web Essentials の追加機能を持つ、HTML エディターが拡張されました。</span><span class="sxs-lookup"><span data-stu-id="72c4b-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="72c4b-321">この演習では、web アプリケーションの開発には便利な一部の新機能が表示されます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="72c4b-322">タスク 1 - HTML ドキュメントで IntelliSense の使用</span><span class="sxs-lookup"><span data-stu-id="72c4b-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="72c4b-323">このタスクに表示される最初の新機能と呼びます**動的 IntelliSense**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="72c4b-324">動的 IntelliSense では、他のタグとは使用可能な id を推測する属性を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="72c4b-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="72c4b-325">このタスクでは、ラベルと入力フィールドを含む新しい HTML フォーム要素を作成します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="72c4b-326">追加してから、**の**属性に、入力にバインドするラベルをスコープ内の入力の id に基づいて、intellisense による候補が表示されます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="72c4b-327">開いている**Visual Studio Express 2013 for Web**と**Begin.sln**にソリューションがある、**ソース/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/開始**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="72c4b-328">代わりに、前の手順で取得した、ソリューションを続行できます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="72c4b-329">**ソリューション エクスプ ローラー**を開き、 **Index.cshtml**にあるファイル、**ビュー** | **ホーム**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="72c4b-330">内の次の形式を追加、 **&lt;セクション&gt;**要素。</span><span class="sxs-lookup"><span data-stu-id="72c4b-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="72c4b-331">(コード スニペットの*VisualStudio2013WebTooling* - *Ex2* - *フォーム*)</span><span class="sxs-lookup"><span data-stu-id="72c4b-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="72c4b-332">入力タグは、フィールドのいくつかの説明ラベルによって先行されなければなりません。</span><span class="sxs-lookup"><span data-stu-id="72c4b-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="72c4b-333">入力タグの前に次のラベルを追加します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="72c4b-334">**の**の属性、 **&lt;ラベル&gt;**ラベルをフォーム要素の連結先を指定します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="72c4b-335">属性の値は、関連する要素の id と同じにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="72c4b-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="72c4b-336">追加、**の**属性を**&lt;ラベル&gt;**要素。</span><span class="sxs-lookup"><span data-stu-id="72c4b-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="72c4b-337">次の図に示すように、&quot;名前&quot;値がポップアップ、[IntelliSense] ボックスで、同じスコープ内の要素の id に基づいた (囲んでいる**&lt;フォーム&gt;**)。</span><span class="sxs-lookup"><span data-stu-id="72c4b-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="72c4b-338">![IntelliSense で id を示す](visual-studio-2013-web-tools/_static/image35.png "IntelliSense での id を表示")</span><span class="sxs-lookup"><span data-stu-id="72c4b-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="72c4b-339">*IntelliSense での id を表示*</span><span class="sxs-lookup"><span data-stu-id="72c4b-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="72c4b-340">削除、新しく追加された**&lt;フォーム&gt;**要素とそのコンテンツ。</span><span class="sxs-lookup"><span data-stu-id="72c4b-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="72c4b-341">作業 2: を使用して HTML コード スニペット</span><span class="sxs-lookup"><span data-stu-id="72c4b-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="72c4b-342">HTML5 では、25 台を超える新しいセマンティック タグが導入されました。</span><span class="sxs-lookup"><span data-stu-id="72c4b-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="72c4b-343">Visual Studio は、これらのタグに対する IntelliSense サポートを既に持ってけれども、Visual Studio 2013 では、高速化し、新しいコード スニペットを追加することでマークアップを記述を簡単になります。</span><span class="sxs-lookup"><span data-stu-id="72c4b-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="72c4b-344">ただし、これらのタグは、複雑で、付属の正しいコーデック フォールバックを追加するなど、いくつかの小さな微妙な*オーディオ*タグ。</span><span class="sxs-lookup"><span data-stu-id="72c4b-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="72c4b-345">このタスクでは、オーディオのタグの HTML コード スニペットが表示されます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="72c4b-346">**Index.cshtml**ファイルに、入力 **&lt;aud**内、 **&lt;セクション&gt;**要素の次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="72c4b-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="72c4b-347">![Audio 要素を挿入する](visual-studio-2013-web-tools/_static/image36.png "audio 要素を挿入します。")</span><span class="sxs-lookup"><span data-stu-id="72c4b-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="72c4b-348">*Audio 要素を挿入します。*</span><span class="sxs-lookup"><span data-stu-id="72c4b-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="72c4b-349">キーを押して**タブ**2 回ページで、次のコードを追加する方法に注意してください。 および上にカーソルが置かれます、 **src**最初のソースの属性です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="72c4b-350">キーを押して、**タブ**キーを 2 回、コード スニペットを挿入します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="72c4b-351">オーディオのスニペットの標準的使用方法を示しています、*オーディオ*サポートの強化の 2 つのソース ファイルでのタグ。</span><span class="sxs-lookup"><span data-stu-id="72c4b-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="72c4b-352">2 番目の行を削除および WebCampsTV Katana を表示する次のリンクを使用して、1 行目のソースの更新: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3)です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="72c4b-353">結果のコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="72c4b-354">ソース ファイル*KatanaProject.mp3*を例として使用します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="72c4b-355">たい場合は、別のソースを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="72c4b-356">キーを押して**CTRL** + **S**ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="72c4b-357">キーを押して**CTRL** + **f5 キーを押して**アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="72c4b-358">オーディオ プレーヤーがアプリケーションに追加されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="72c4b-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="72c4b-359">![Internet Explorer でのオーディオ プレーヤー](visual-studio-2013-web-tools/_static/image37.png "Internet Explorer でのオーディオ プレーヤー")</span><span class="sxs-lookup"><span data-stu-id="72c4b-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="72c4b-360">*Internet Explorer でのオーディオ プレーヤー*</span><span class="sxs-lookup"><span data-stu-id="72c4b-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="72c4b-361">![Google Chrome でオーディオ プレーヤー](visual-studio-2013-web-tools/_static/image38.png "Google Chrome でオーディオ プレーヤー")</span><span class="sxs-lookup"><span data-stu-id="72c4b-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="72c4b-362">*Google Chrome でオーディオ プレーヤー*</span><span class="sxs-lookup"><span data-stu-id="72c4b-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="72c4b-363">ブラウザーを閉じないでください。</span><span class="sxs-lookup"><span data-stu-id="72c4b-363">Do not close the browsers.</span></span> <span data-ttu-id="72c4b-364">次のタスクでは、それらを使用します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="72c4b-365">タスク 3 - JavaScript ドキュメントで IntelliSense の使用</span><span class="sxs-lookup"><span data-stu-id="72c4b-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="72c4b-366">Web Essentials 2013 とスタイル シートおよび HTML ページは、Id 名とクラス名の一覧を生成します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="72c4b-367">このタスクでは、それらのリストが Web Essentials 2013 での JavaScript IntelliSense のサポートを向上させる方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="72c4b-368">**Index.cshtml**ファイルに定義する次のコードを追加、**スクリプト**JavaScript コードのタグ。</span><span class="sxs-lookup"><span data-stu-id="72c4b-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="72c4b-369">次のコードを追加、**スクリプト**タグ準備完了コールバック関数を定義します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="72c4b-370">(コード スニペットの*VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="72c4b-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="72c4b-371">内のキャレットを配置、**スクリプト**タグとキーを押して**CTRL** + **です。**</span><span class="sxs-lookup"><span data-stu-id="72c4b-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="72c4b-372">提案メニューを開きます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="72c4b-373">をクリックして**ファイルに抽出**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="72c4b-374">![ファイルの修正案に JavaScript が抽出](visual-studio-2013-web-tools/_static/image39.png "ファイル修正案を JavaScript の抽出")</span><span class="sxs-lookup"><span data-stu-id="72c4b-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="72c4b-375">*ファイルの修正案を JavaScript を抽出します。*</span><span class="sxs-lookup"><span data-stu-id="72c4b-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="72c4b-376">**名前を付けて保存**ウィンドウで、**スクリプト**フォルダー、ファイルの名前**init.js** をクリック**保存**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="72c4b-377">![名前を付けて保存ウィンドウ](visual-studio-2013-web-tools/_static/image40.png "名前を付けて保存ウィンドウ")</span><span class="sxs-lookup"><span data-stu-id="72c4b-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="72c4b-378">*名前を付けて保存ウィンドウ*</span><span class="sxs-lookup"><span data-stu-id="72c4b-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="72c4b-379">**Init.js**ファイルが作成され、スクリプトの内容が、ファイルに移動します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="72c4b-380">![Init.js ファイルが含まれているコンテンツと共に作成](visual-studio-2013-web-tools/_static/image41.png "Init.js ファイルが含まれているコンテンツと共に作成")</span><span class="sxs-lookup"><span data-stu-id="72c4b-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="72c4b-381">*含まれるコンテンツと共に作成 Init.js ファイル*</span><span class="sxs-lookup"><span data-stu-id="72c4b-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="72c4b-382">開く、 **Index.cshtml**ファイルし、スクリプト タグがへの参照に置き換えられますことを確認して、 **init.js**ファイル。</span><span class="sxs-lookup"><span data-stu-id="72c4b-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="72c4b-383">![Html の参照を Init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js html の参照")</span><span class="sxs-lookup"><span data-stu-id="72c4b-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="72c4b-384">*Init.js html の参照*</span><span class="sxs-lookup"><span data-stu-id="72c4b-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="72c4b-385">移動して、**ソリューション エクスプ ローラー**ことを確認し、 **init.js**ファイルは、ソリューションに自動的に含まれていました。</span><span class="sxs-lookup"><span data-stu-id="72c4b-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="72c4b-386">![ソリューションに含まれる Init.js ファイル](visual-studio-2013-web-tools/_static/image43.png "Init.js ファイルがソリューションに含まれる")</span><span class="sxs-lookup"><span data-stu-id="72c4b-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="72c4b-387">*ソリューションに含まれる Init.js ファイル*</span><span class="sxs-lookup"><span data-stu-id="72c4b-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="72c4b-388">戻り、 **init.js**ファイルを更新する、**準備**関数のコールバック。</span><span class="sxs-lookup"><span data-stu-id="72c4b-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="72c4b-389">渡される関数コールバック定義の内部*準備*、特定のクラス属性によってすべての要素を取得する次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="72c4b-390">キーを押して**CTRL** + **領域**内の引用符で囲んで、 **getElementsByClassName**関数呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="72c4b-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="72c4b-391">![GetElementsByClassName 関数の IntelliSense を示す](visual-studio-2013-web-tools/_static/image44.png "getElementsByClassName 関数用の IntelliSense を表示")</span><span class="sxs-lookup"><span data-stu-id="72c4b-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="72c4b-392">*GetElementsByClassName 関数の IntelliSense を表示*</span><span class="sxs-lookup"><span data-stu-id="72c4b-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="72c4b-393">IntelliSense が、プロジェクトのスタイル シートで定義されているクラスを示して ことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="72c4b-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="72c4b-394">次のコードで作成した行を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="72c4b-395">位置の後にカーソルを置く**オーストラリア**で引用符で囲んで、 **getElementsByTagName**関数とキーを押して**ctrl キーを押し** + **領域**.</span><span class="sxs-lookup"><span data-stu-id="72c4b-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="72c4b-396">![GetElementByTagName メソッドの IntelliSense を示す](visual-studio-2013-web-tools/_static/image45.png "getElementByTagName メソッド用の IntelliSense を表示")</span><span class="sxs-lookup"><span data-stu-id="72c4b-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="72c4b-397">*GetElementsByTagName メソッドの IntelliSense を表示*</span><span class="sxs-lookup"><span data-stu-id="72c4b-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="72c4b-398">選択**&quot;オーディオ&quot;**キーを押して、一覧から**ENTER**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="72c4b-399">結果を次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="72c4b-400">![オーディオの要素を取得する](visual-studio-2013-web-tools/_static/image46.png "オーディオ要素を取得します。")</span><span class="sxs-lookup"><span data-stu-id="72c4b-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="72c4b-401">*オーディオの要素を取得します。*</span><span class="sxs-lookup"><span data-stu-id="72c4b-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="72c4b-402">**ソリューション エクスプ ローラー**を右クリックし、 **init.js**ファイルを**スクリプト**フォルダーと選択**JavaScript の縮小ファイル**から、**Web Essentials**メニュー。</span><span class="sxs-lookup"><span data-stu-id="72c4b-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="72c4b-403">![JavaScript ファイルを縮小](visual-studio-2013-web-tools/_static/image47.png "JavaScript の縮小ファイル")</span><span class="sxs-lookup"><span data-stu-id="72c4b-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="72c4b-404">*JavaScript ファイルを縮小します。*</span><span class="sxs-lookup"><span data-stu-id="72c4b-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="72c4b-405">ソース ファイルの変更をクリックすると、自動縮小を有効にするように求められたら**はい**です。</span><span class="sxs-lookup"><span data-stu-id="72c4b-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="72c4b-406">![自動縮小の警告を有効にする](visual-studio-2013-web-tools/_static/image48.png "自動縮小の警告を有効にします。")</span><span class="sxs-lookup"><span data-stu-id="72c4b-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="72c4b-407">*自動縮小の警告を有効にします。*</span><span class="sxs-lookup"><span data-stu-id="72c4b-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="72c4b-408">**Init.min.js**が作成され、a の依存関係として追加されて、 **init.js**ファイル。</span><span class="sxs-lookup"><span data-stu-id="72c4b-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="72c4b-409">![作成された Init.min.js ファイル](visual-studio-2013-web-tools/_static/image49.png "Init.min.js ファイルの作成")</span><span class="sxs-lookup"><span data-stu-id="72c4b-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="72c4b-410">*作成された Init.min.js ファイル*</span><span class="sxs-lookup"><span data-stu-id="72c4b-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="72c4b-411">開く、 **init.min.js**ファイルし、ファイルを縮小ことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="72c4b-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="72c4b-412">![ファイルの内容を Init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js ファイルの内容")</span><span class="sxs-lookup"><span data-stu-id="72c4b-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="72c4b-413">*Init.min.js ファイルの内容*</span><span class="sxs-lookup"><span data-stu-id="72c4b-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="72c4b-414">**Init.js**ファイルに次の次のコードを追加、 **getElementsByTagName**関数の呼び出しをすべてのオーディオ要素を再生します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="72c4b-415">(コード スニペットの*VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="72c4b-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="72c4b-416">をクリックして**CTRL** + **S**ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="72c4b-417">縮小されたファイルは既に開かれているため、ファイルがソース エディターの外部で変更されたことを示すダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="72c4b-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="72c4b-418">**[はい]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="72c4b-418">Click **Yes**.</span></span>

    <span data-ttu-id="72c4b-419">![Microsoft Visual Studio 警告](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio の警告")</span><span class="sxs-lookup"><span data-stu-id="72c4b-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="72c4b-420">*Microsoft Visual Studio の警告*</span><span class="sxs-lookup"><span data-stu-id="72c4b-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="72c4b-421">戻り、 **init.min.js**ファイルを新しいコードで、ファイルが更新されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="72c4b-422">![更新 Init.min.js ファイル](visual-studio-2013-web-tools/_static/image52.png "Init.min.js ファイルが更新されました")</span><span class="sxs-lookup"><span data-stu-id="72c4b-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="72c4b-423">*Init.min.js ファイルが更新されました*</span><span class="sxs-lookup"><span data-stu-id="72c4b-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="72c4b-424">クリックして、**ブラウザー リンク更新**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="72c4b-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="72c4b-425">両方のブラウザーが更新される前の実習で学習したオーディオ プレーヤーは自動的に再生を開始します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="72c4b-426">![ビューに含まれるオーディオ プレーヤー](visual-studio-2013-web-tools/_static/image53.png "ビューに含まれるオーディオ プレーヤー")</span><span class="sxs-lookup"><span data-stu-id="72c4b-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="72c4b-427">*ビューに含まれるオーディオ プレーヤー*</span><span class="sxs-lookup"><span data-stu-id="72c4b-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="72c4b-428">まとめ</span><span class="sxs-lookup"><span data-stu-id="72c4b-428">Summary</span></span>

<span data-ttu-id="72c4b-429">このハンズオン ラボを完了して学習した方法。</span><span class="sxs-lookup"><span data-stu-id="72c4b-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="72c4b-430">豊富な HTML5 のコード スニペットと全角コーディングなど Web Essentials に含まれる HTML エディターの新機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="72c4b-431">カラー ピッカーやブラウザー マトリックス ツールヒントなどの Web Essentials に含まれる CSS エディターの新機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="72c4b-432">すべての HTML 要素の IntelliSense およびファイルへの展開などの Web Essentials に含まれる JavaScript エディターの新機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="72c4b-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="72c4b-433">ブラウザーとブラウザー リンクを使用して Visual Studio の間での Exchange データ</span><span class="sxs-lookup"><span data-stu-id="72c4b-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
