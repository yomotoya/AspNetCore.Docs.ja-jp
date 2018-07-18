---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'ハンズ オン ラボ: Visual Studio 2013 Web ツール |Microsoft Docs'
author: rick-anderson
description: Visual Studio には、優れた開発環境です。NET ベースの Windows と web プロジェクト。 簡単に使用できる強力なテキスト エディターが含まれています.
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 9553d3129ff4c942eacbba628d1daaf6c4e33635
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807529"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="d18bf-104">ハンズ オン ラボ: Visual Studio 2013 Web ツール</span><span class="sxs-lookup"><span data-stu-id="d18bf-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="d18bf-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="d18bf-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="d18bf-106">Web のキャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="d18bf-107">Visual Studio には、優れた開発環境です。NET ベースの Windows と web プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="d18bf-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="d18bf-108">これには、プロジェクトなしのスタンドアロン ファイルの編集を簡単に使用できる強力なテキスト エディターが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="d18bf-109">Visual Studio は、各ファイルを編集すると、全機能装備の解析ツリーを保持します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="d18bf-110">これにより、Visual Studio 開発体験をはるかに高速より快適比類のないオート コンプリートとドキュメントに基づくアクションを提供します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="d18bf-111">これらの機能は、HTML および CSS のドキュメントでは特に強力です。</span><span class="sxs-lookup"><span data-stu-id="d18bf-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="d18bf-112">すべてこの電源は、拡張機能、強力な新しい機能によって、ニーズに合わせてのエディターの拡張を簡単に使用できます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="d18bf-113">Web Essentials は、Visual Studio に拡張を (主に) web 関連のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="d18bf-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="d18bf-114">(特に、CSS) の新しい IntelliSense 入力候補、ブラウザー リンクの新機能、自動の多くが含まれています JSHint の JavaScript ファイル、HTML、CSS、および最新の web 開発に不可欠な他の多くの機能の新しい警告します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="d18bf-115">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="d18bf-116">概要</span><span class="sxs-lookup"><span data-stu-id="d18bf-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="d18bf-117">目的</span><span class="sxs-lookup"><span data-stu-id="d18bf-117">Objectives</span></span>

<span data-ttu-id="d18bf-118">このハンズオン ラボでは、学習する方法。</span><span class="sxs-lookup"><span data-stu-id="d18bf-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="d18bf-119">豊富な HTML5 コード スニペットや Zen コーディングなど、Web Essentials に含まれる HTML エディターの新機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="d18bf-120">カラー ピッカーやブラウザー マトリックス ツールヒントなど、Web Essentials に含まれる CSS エディターの新機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="d18bf-121">すべての HTML 要素を抽出するファイルと IntelliSense などの Web Essentials に含まれる JavaScript エディターの新機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="d18bf-122">ブラウザーとブラウザー リンクを使用して Visual Studio 間の Exchange データ</span><span class="sxs-lookup"><span data-stu-id="d18bf-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="d18bf-123">前提条件</span><span class="sxs-lookup"><span data-stu-id="d18bf-123">Prerequisites</span></span>

<span data-ttu-id="d18bf-124">このハンズオン ラボを完了する、次が必要。</span><span class="sxs-lookup"><span data-stu-id="d18bf-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="d18bf-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/)以上</span><span class="sxs-lookup"><span data-stu-id="d18bf-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="d18bf-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="d18bf-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="d18bf-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="d18bf-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="d18bf-128">セットアップ</span><span class="sxs-lookup"><span data-stu-id="d18bf-128">Setup</span></span>

<span data-ttu-id="d18bf-129">このハンズオン ラボの演習を実行するためには、まず環境を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d18bf-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="d18bf-130">Windows エクスプ ローラー ウィンドウを開き、ラボを参照**ソース**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="d18bf-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="d18bf-131">右クリック**Setup.cmd**選択**管理者として実行**環境を構成すると、このラボの Visual Studio のコード スニペットがインストールのセットアップ プロセスを起動します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="d18bf-132">ユーザー アカウント制御ダイアログ ボックスが表示されている場合は、続行するアクションを確認します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="d18bf-133">セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="d18bf-134">コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="d18bf-134">Using the Code Snippets</span></span>

<span data-ttu-id="d18bf-135">ラボ ドキュメント全体を通じて、コード ブロックを挿入するよう指示されます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="d18bf-136">便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを避けるために Visual Studio 2013 内からアクセスできるとして提供されます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="d18bf-137">ソリューションでは個々 の演習を伴います、**開始**を使用すると、各演習を他のユーザーとは無関係に練習のフォルダー。</span><span class="sxs-lookup"><span data-stu-id="d18bf-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="d18bf-138">演習の中に追加されるコード スニペットはこれらのスターティング ソリューションが表示されないし、演習を完了するまで動作しない可能性がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d18bf-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="d18bf-139">演習では、ソース コード内でも表示されます、**エンド**結果から、対応する演習の手順を実行するコードと Visual Studio ソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="d18bf-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="d18bf-140">このハンズオン ラボを使用すると、追加のヘルプが必要な場合は、これらのソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="d18bf-141">演習</span><span class="sxs-lookup"><span data-stu-id="d18bf-141">Exercises</span></span>

<span data-ttu-id="d18bf-142">このハンズオン ラボには、次の演習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="d18bf-143">Browser Link と Web Essentials の使用</span><span class="sxs-lookup"><span data-stu-id="d18bf-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="d18bf-144">コード スニペットと IntelliSense の活用</span><span class="sxs-lookup"><span data-stu-id="d18bf-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="d18bf-145">Visual Studio を初めて起動すると、定義済みの設定のコレクションの 1 つを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d18bf-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="d18bf-146">定義済みの各コレクションは、特定の開発スタイルに一致するように設計されていて、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペット、およびダイアログ ボックスのオプションを決定します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="d18bf-147">このラボの手順を使用する場合は、Visual Studio で特定のタスクを実行するために必要な操作を記述する、**汎用開発設定**コレクション。</span><span class="sxs-lookup"><span data-stu-id="d18bf-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="d18bf-148">開発環境のさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。</span><span class="sxs-lookup"><span data-stu-id="d18bf-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="d18bf-149">Browser Link と Web Essentials 演習 1: を使用します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="d18bf-150">**Web Essentials**はさまざまな web 開発体験をはるかに高速より快適に焦点を合わせた、最新の web 開発用の便利な機能を追加する Visual Studio 拡張機能です。</span><span class="sxs-lookup"><span data-stu-id="d18bf-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="d18bf-151">Web Essentials は、Visual Studio の拡張機能ギャラリーからインストールできます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="d18bf-152">**Browser Link**は Visual Studio、web アプリケーションの間でデータを交換するには、Visual Studio IDE と任意の開いているブラウザー間のチャネルを提供する Visual Studio 2013 に含まれる新機能です。</span><span class="sxs-lookup"><span data-stu-id="d18bf-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="d18bf-153">Web Essentials は、DOM オブジェクト モデルと、ブラウザーから直接 web ページの CSS スタイルを操作するためのツールを Browser Link を拡張します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="d18bf-154">この演習で一部の機能でサポートされている探索は**Web Essentials**と**Browser Link**簡単なクイズ ページを強化するためにします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="d18bf-155">タスク 1 - 複数のブラウザーでプロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="d18bf-156">このタスクでは、クロス ブラウザー テスト用の便利なは、一度に複数のブラウザーで実行する web アプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="d18bf-157">開いている**Microsoft Visual Studio**します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="d18bf-158">**ファイル**メニューの**開く |プロジェクト/ソリューションしています.** を見つけて**Ex1 WorkingwithBrowserLinkandWebEssentials\Begin**で、**ソース**ラボ (C:\WebCampsTK\HOL\VSWebTooling\Source) のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d18bf-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="d18bf-159">選択**Begin.sln**をクリック**開く**です。</span><span class="sxs-lookup"><span data-stu-id="d18bf-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="d18bf-160">Visual Studio のツールバーで、[ブラウザー] メニューを展開し、選択**を参照しています.**.</span><span class="sxs-lookup"><span data-stu-id="d18bf-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="d18bf-161">![ブラウザーのメニュー オプション](visual-studio-2013-web-tools/_static/image1.png "ブラウザーのメニューで で [参照]")</span><span class="sxs-lookup"><span data-stu-id="d18bf-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="d18bf-162">*ブラウザーのメニュー オプション*</span><span class="sxs-lookup"><span data-stu-id="d18bf-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="d18bf-163">**Browse With**ダイアログ ボックスの両方をオン**Google Chrome**と**Internet Explorer**押したまま、 **CTRL**キーおよびをクリックして**既定値として設定**です。</span><span class="sxs-lookup"><span data-stu-id="d18bf-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="d18bf-164">![ダイアログ ボックスで [参照]](visual-studio-2013-web-tools/_static/image2.png "ダイアログ ボックスで [参照]")</span><span class="sxs-lookup"><span data-stu-id="d18bf-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="d18bf-165">*複数の既定のブラウザーを選択します。*</span><span class="sxs-lookup"><span data-stu-id="d18bf-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="d18bf-166">Google Chrome や Internet Explorer の両方が既定のブラウザーとしては表示されます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="d18bf-167">をクリックして**キャンセル**ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="d18bf-168">![Google Chrome と既定のブラウザーとして Internet Explorer](visual-studio-2013-web-tools/_static/image3.png "Google Chrome や Internet Explorer の既定のブラウザー")</span><span class="sxs-lookup"><span data-stu-id="d18bf-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="d18bf-169">*Google Chrome と既定のブラウザーとして Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="d18bf-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d18bf-170">既定のブラウザーを構成した後、**複数のブラウザー**ブラウザー メニューのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="d18bf-171">![複数のブラウザー](visual-studio-2013-web-tools/_static/image4.png "複数のブラウザー")</span><span class="sxs-lookup"><span data-stu-id="d18bf-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="d18bf-172">キーを押して**CTRL** + **F5**デバッグなしでアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="d18bf-173">両方のブラウザー ウィンドウが開いたときに、更新プログラムを同時に両方のブラウザーで表示するには、上記のうちの 1 つを配置します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="d18bf-174">ブラウザーでは、薄い青色の四角形を持つ web ページを表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d18bf-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="d18bf-175">![上の 1 つのブラウザーを配置する](visual-studio-2013-web-tools/_static/image5.png "上、もう 1 つのブラウザーを配置します。")</span><span class="sxs-lookup"><span data-stu-id="d18bf-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="d18bf-176">*上の 1 つのブラウザーを配置します。*</span><span class="sxs-lookup"><span data-stu-id="d18bf-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="d18bf-177">ブラウザーを閉じないでください。</span><span class="sxs-lookup"><span data-stu-id="d18bf-177">Do not close the browsers.</span></span> <span data-ttu-id="d18bf-178">次のタスクでは、それらを使用します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="d18bf-179">タスク 2 - HTML 要素を作成する場合、コーディングを使用して Zen</span><span class="sxs-lookup"><span data-stu-id="d18bf-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="d18bf-180">**Zen コーディング**エディターは、高速の HTML、XML、XSL (またはその他の構造化されたコード形式) 用のプラグインのコーディングおよび編集します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="d18bf-181">このプラグインのコアは、HTML コードに - CSS セレクターのように式を拡張できるようにする強力な省略形エンジンです。</span><span class="sxs-lookup"><span data-stu-id="d18bf-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="d18bf-182">Zen コーディングは、HTML、CSS を使用してスタイルのセレクターの構文を記述する高速の方法です。</span><span class="sxs-lookup"><span data-stu-id="d18bf-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="d18bf-183">この演習では、質問のオプションを表す HTML ボタンを生成するのに Web Essentials によって提供される Zen コーディング機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="d18bf-184">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="d18bf-185">開く、 **Index.cshtml**にあるファイル、**ビュー** | **ホーム**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="d18bf-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="d18bf-186">置換、  **&lt;!--TODO: ここでオプションを追加する&gt;** コメントを次のコード、およびキーを押して**タブ**。</span><span class="sxs-lookup"><span data-stu-id="d18bf-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="d18bf-187">コードは、HTML に拡張する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d18bf-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="d18bf-188">![HTML を拡張](visual-studio-2013-web-tools/_static/image6.png "HTML の展開")</span><span class="sxs-lookup"><span data-stu-id="d18bf-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="d18bf-189">*展開された HTML*</span><span class="sxs-lookup"><span data-stu-id="d18bf-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d18bf-190">Zen コーディングの構文の詳細については、次を参照してください。[記事](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="d18bf-191">をクリックして、**リンクされたブラウザーを更新**両方のブラウザーを更新するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="d18bf-192">![リンクされたブラウザーを更新](visual-studio-2013-web-tools/_static/image7.png "リンクされたブラウザーを更新")</span><span class="sxs-lookup"><span data-stu-id="d18bf-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="d18bf-193">*リンクされたブラウザーを更新します。*</span><span class="sxs-lookup"><span data-stu-id="d18bf-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="d18bf-194">![Internet Explorer - ページは 4 つのボタンで更新](visual-studio-2013-web-tools/_static/image8.png "4 つのボタンで Internet Explorer - ページの更新")</span><span class="sxs-lookup"><span data-stu-id="d18bf-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="d18bf-195">*4 つのボタンで Internet Explorer - ページの更新*</span><span class="sxs-lookup"><span data-stu-id="d18bf-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="d18bf-196">![Google Chrome のページは 4 つのボタンで更新](visual-studio-2013-web-tools/_static/image9.png "4 つのボタンで Google Chrome のページの更新")</span><span class="sxs-lookup"><span data-stu-id="d18bf-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="d18bf-197">*Google Chrome の 4 つのボタンのページの更新*</span><span class="sxs-lookup"><span data-stu-id="d18bf-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="d18bf-198">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="d18bf-199">ページにボタンを追加したが、テンプレートの質問を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d18bf-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="d18bf-200">これを行うと呼ばれる Web Essentials の新機能を使用します**Lorem Ipsum ジェネレーター**します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="d18bf-201">検索、 **div**を持つ要素、**クラス**属性**前面**します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="d18bf-202">最初の子要素として次のコードを追加、 **div**、キーを押します**タブ**します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="d18bf-203">コードは、HTML に拡張する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d18bf-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="d18bf-204">![Lorem Ipsum の自動生成された](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum の自動生成")</span><span class="sxs-lookup"><span data-stu-id="d18bf-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="d18bf-205">*Lorem Ipsum の自動生成*</span><span class="sxs-lookup"><span data-stu-id="d18bf-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d18bf-206">Zen コーディングの一部として、HTML エディターで直接 Lorem Ipsum のコードを生成できます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="d18bf-207">入力するだけ**lorem**ヒットと**タブ**し、30 Lorem Ipsum のテキストが挿入されます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="d18bf-208">たとえば、</span><span class="sxs-lookup"><span data-stu-id="d18bf-208">E.g.</span></span> <span data-ttu-id="d18bf-209">*lorem10* Lorem Ipsum の 10 単語を挿入します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="d18bf-210">もう 1 つの新しい機能と呼ばれる Web Essentials を使用して、質問の上部にあるロゴを追加する**Lorem ピクセル ジェネレーター**します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="d18bf-211">最初の子要素として次のコードを追加、 **div**を持つ要素**コンテナー**として**クラス**値、およびキーを押して**タブ**します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="d18bf-212">コードを HTML に展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d18bf-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="d18bf-213">![Lorem ピクセルの自動生成された](visual-studio-2013-web-tools/_static/image11.png "Lorem ピクセルの自動生成")</span><span class="sxs-lookup"><span data-stu-id="d18bf-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="d18bf-214">*Lorem ピクセルの自動生成*</span><span class="sxs-lookup"><span data-stu-id="d18bf-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d18bf-215">Zen コーディングの一部として、HTML エディターで直接 Lorem ピクセルのコードを生成することもできます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="d18bf-216">入力**pix の動物 200 x 200**ヒットと**タブ**と**img**動物の 200 x 200 イメージにタグが挿入されます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="d18bf-217">詳細についてを参照してください[Lorem ピクセル](http://www.lorempixel.com)します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="d18bf-218">をクリックして、**リンクされたブラウザーを更新**両方のブラウザーを更新するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="d18bf-219">![Internet Explorer - 自動生成されたイメージとテキスト](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - 自動生成されたイメージとテキスト")</span><span class="sxs-lookup"><span data-stu-id="d18bf-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="d18bf-220">*Internet Explorer - 自動生成されたイメージとテキスト*</span><span class="sxs-lookup"><span data-stu-id="d18bf-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="d18bf-221">![Google Chrome の自動生成されたイメージとテキスト](visual-studio-2013-web-tools/_static/image13.png "Google Chrome の自動生成されたイメージとテキスト")</span><span class="sxs-lookup"><span data-stu-id="d18bf-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="d18bf-222">*Google Chrome の自動生成されたイメージとテキスト*</span><span class="sxs-lookup"><span data-stu-id="d18bf-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d18bf-223">コード スニペットを追加するときに、イメージがランダムに選択、ために、ブラウザーに表示されるイメージが異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="d18bf-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="d18bf-224">ブラウザーを閉じないでください。</span><span class="sxs-lookup"><span data-stu-id="d18bf-224">Do not close the browsers.</span></span> <span data-ttu-id="d18bf-225">次のタスクでは、それらを使用します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="d18bf-226">タスク 3 - スタイル プロパティを更新しています</span><span class="sxs-lookup"><span data-stu-id="d18bf-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="d18bf-227">このタスクでは、ブラウザー リンクを使用して**検査モード**特定の DOM 要素が生成される場所の正確な場所を検出し、Web で提供されるカラー ピッカーを使用してその要素の色のプロパティを更新する機能Essentials。</span><span class="sxs-lookup"><span data-stu-id="d18bf-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="d18bf-228">Internet Explorer のブラウザーでキーを押して**CTRL** + **ALT** + **は**検査モードを有効にします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="d18bf-229">薄い青の枠線の上にポインターを移動し をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="d18bf-230">![薄い青の枠線にポインターを移動](visual-studio-2013-web-tools/_static/image14.png "明るい青色の枠線にポインターを移動")</span><span class="sxs-lookup"><span data-stu-id="d18bf-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="d18bf-231">*薄い青の枠線にポインターを移動*</span><span class="sxs-lookup"><span data-stu-id="d18bf-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="d18bf-232">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="d18bf-233">Visual Studio の HTML エディターで、ブラウザーで選択した HTML 要素を選択することも注目してください。</span><span class="sxs-lookup"><span data-stu-id="d18bf-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="d18bf-234">![Visual Studio の HTML エディターで選択されている HTML 要素](visual-studio-2013-web-tools/_static/image15.png "Visual Studio の HTML エディターで選択されている HTML 要素")</span><span class="sxs-lookup"><span data-stu-id="d18bf-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="d18bf-235">*Visual Studio の HTML エディターで選択されている HTML 要素*</span><span class="sxs-lookup"><span data-stu-id="d18bf-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="d18bf-236">今すぐ更新する、**前面**選択した要素のスタイルを変更するために CSS クラス。</span><span class="sxs-lookup"><span data-stu-id="d18bf-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="d18bf-237">これを行うには、キーを押します**CTRL** + **、** を開く、**移動**検索ボックス。</span><span class="sxs-lookup"><span data-stu-id="d18bf-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="d18bf-238">型**site.css**キーを押します**ENTER**ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="d18bf-239">![Site.css ファイルを開く](visual-studio-2013-web-tools/_static/image16.png "Site.css ファイルを開いています")</span><span class="sxs-lookup"><span data-stu-id="d18bf-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="d18bf-240">*Site.css ファイルを開いています*</span><span class="sxs-lookup"><span data-stu-id="d18bf-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="d18bf-241">キーを押して**CTRL** + **F**と種類 **.flip コンテナー .front** CSS セレクターを検索します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="d18bf-242">カラー ピッカーを開きますクラスの枠線のプロパティで明るい青い四角形をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="d18bf-243">![カラー ピッカーを開いて](visual-studio-2013-web-tools/_static/image17.png "カラー ピッカーを開く")</span><span class="sxs-lookup"><span data-stu-id="d18bf-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="d18bf-244">*カラー ピッカーを開く*</span><span class="sxs-lookup"><span data-stu-id="d18bf-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="d18bf-245">シェブロンをクリックして、カラー ピッカーを展開し、新しい色を選択します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="d18bf-246">![カラー ピッカーの展開](visual-studio-2013-web-tools/_static/image18.png "カラー ピッカーの展開")</span><span class="sxs-lookup"><span data-stu-id="d18bf-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="d18bf-247">*カラー ピッカーの展開*</span><span class="sxs-lookup"><span data-stu-id="d18bf-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="d18bf-248">キーを押して**CTRL** + **ALT** + **ENTER**リンクされたブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="d18bf-249">Internet Explorer に切り替えるし、罫線の色がどのように変更されたかに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d18bf-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="d18bf-250">![Internet Explorer - 更新された境界線の色](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - 更新された境界線の色")</span><span class="sxs-lookup"><span data-stu-id="d18bf-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="d18bf-251">*Internet Explorer - 更新された境界線の色*</span><span class="sxs-lookup"><span data-stu-id="d18bf-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="d18bf-252">Google Chrome に切り替えるし、罫線の色がどのように変更されたかに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d18bf-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="d18bf-253">![Google Chrome - 更新された境界線の色](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - 更新された境界線の色")</span><span class="sxs-lookup"><span data-stu-id="d18bf-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="d18bf-254">*Google Chrome - 更新された境界線の色*</span><span class="sxs-lookup"><span data-stu-id="d18bf-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="d18bf-255">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="d18bf-256">末尾に移動して、 **Site.css**ファイルとキーを押して**CTRL** + **F**を検索する、 **.btn**セレクター。</span><span class="sxs-lookup"><span data-stu-id="d18bf-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="d18bf-257">注意、 **webkit の境界線の radius**プロパティは、緑色の下線が引かれます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="d18bf-258">![btn セレクターの webkit の境界線の半径プロパティ](visual-studio-2013-web-tools/_static/image21.png "btn セレクターの webkit の境界線の半径プロパティ")</span><span class="sxs-lookup"><span data-stu-id="d18bf-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="d18bf-259">*btn セレクターの webkit の境界線の半径プロパティ*</span><span class="sxs-lookup"><span data-stu-id="d18bf-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="d18bf-260">キャレットを置き、 **webkit-罫線-radius**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="d18bf-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="d18bf-261">青い線は、プロパティの最初の単語の最初の文字の下に表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d18bf-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="d18bf-262">これは、**スマート タグ**します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="d18bf-263">キーを押して**CTRL** + **します。**</span><span class="sxs-lookup"><span data-stu-id="d18bf-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="d18bf-264">推奨事項 メニューを開き、をクリックする**標準プロパティ (radius 境界線) が不足している追加**します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="d18bf-265">![追加の標準プロパティの提案が不足している](visual-studio-2013-web-tools/_static/image22.png "追加の修正候補の標準プロパティがありません")</span><span class="sxs-lookup"><span data-stu-id="d18bf-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="d18bf-266">*追加の修正候補の標準プロパティがありません。*</span><span class="sxs-lookup"><span data-stu-id="d18bf-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="d18bf-267">**境界の半径**プロパティは、CSS 規則を自動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="d18bf-268">![追加標準プロパティが欠落して](visual-studio-2013-web-tools/_static/image23.png "追加標準プロパティが見つかりません")</span><span class="sxs-lookup"><span data-stu-id="d18bf-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="d18bf-269">*追加標準プロパティが見つかりません*</span><span class="sxs-lookup"><span data-stu-id="d18bf-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="d18bf-270">ポインターを置く、**境界の半径**プロパティを表示、**ブラウザー マトリックス ツールヒント**します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="d18bf-271">**ブラウザー マトリックス ツールヒント**各ブラウザーで、プロパティの可用性を示します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="d18bf-272">![ブラウザーのマトリックス ツールヒント](visual-studio-2013-web-tools/_static/image24.png "ブラウザー マトリックス ツールヒント")</span><span class="sxs-lookup"><span data-stu-id="d18bf-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="d18bf-273">*ブラウザーのマトリックスのツールヒント*</span><span class="sxs-lookup"><span data-stu-id="d18bf-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="d18bf-274">注意の値、**境界の半径**プロパティがまだ下線が引かれました。</span><span class="sxs-lookup"><span data-stu-id="d18bf-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="d18bf-275">警告メッセージが表示される値の上にポインターを移動します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="d18bf-276">![境界の半径プロパティ値の警告](visual-studio-2013-web-tools/_static/image25.png "境界の半径プロパティ値の警告")</span><span class="sxs-lookup"><span data-stu-id="d18bf-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="d18bf-277">*境界の半径プロパティ値の警告*</span><span class="sxs-lookup"><span data-stu-id="d18bf-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="d18bf-278">単位を削除、**境界の半径**プロパティ値、ツールヒントで示されているとします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="d18bf-279">として**境界の半径**コーナーは、削除することができます丸みのある方法の境界を定義するための標準プロパティは、 **webkit-罫線-radius**プロパティと、CSS 規則からの値。</span><span class="sxs-lookup"><span data-stu-id="d18bf-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="d18bf-280">キャレットを置き、**ワード ラップ**プロパティとスマート タグは、以下も表示されることがわかります。</span><span class="sxs-lookup"><span data-stu-id="d18bf-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="d18bf-281">メニューを開き、をクリックして**不足しているベンダーの仕様を追加**します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="d18bf-282">![不足しているベンダーの仕様の修正候補を追加](visual-studio-2013-web-tools/_static/image26.png "不足しているベンダーの仕様の修正候補の追加")</span><span class="sxs-lookup"><span data-stu-id="d18bf-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="d18bf-283">*不足しているベンダーの仕様の修正候補を追加します。*</span><span class="sxs-lookup"><span data-stu-id="d18bf-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="d18bf-284">**Ms 折り返し**プロパティは、CSS 規則を自動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="d18bf-285">![ベンダー固有のプロパティが追加](visual-studio-2013-web-tools/_static/image27.png "ベンダー固有のプロパティを追加")</span><span class="sxs-lookup"><span data-stu-id="d18bf-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="d18bf-286">*ベンダー固有のプロパティを追加*</span><span class="sxs-lookup"><span data-stu-id="d18bf-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="d18bf-287">タスク 4 - ブラウザーから HTML コードの更新</span><span class="sxs-lookup"><span data-stu-id="d18bf-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="d18bf-288">このタスクでは、ブラウザー リンクを使用して**デザイン モード**ブラウザーからの DOM オブジェクトを編集し、Visual Studio での HTML ソース ファイルに変更を転送する機能。</span><span class="sxs-lookup"><span data-stu-id="d18bf-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="d18bf-289">Google Chrome でキーを押して**CTRL** + **ALT** + **D**デザイン モードを有効にします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="d18bf-290">ポインターを置く、 **Lorem Ipsum dolor sit amet**ラベルを付け、をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="d18bf-291">![質問を編集](visual-studio-2013-web-tools/_static/image28.png "質問の編集")</span><span class="sxs-lookup"><span data-stu-id="d18bf-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="d18bf-292">*質問の編集*</span><span class="sxs-lookup"><span data-stu-id="d18bf-292">*Editing the question*</span></span>
3. <span data-ttu-id="d18bf-293">カーソルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-293">A cursor should appear.</span></span> <span data-ttu-id="d18bf-294">元のテキストを置換*どのそのような長い質問を書き込むときにしますか?*、しキーを押します**ESC**デザイン モードを終了します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="d18bf-295">![編集の質問](visual-studio-2013-web-tools/_static/image29.png "質問の編集")</span><span class="sxs-lookup"><span data-stu-id="d18bf-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="d18bf-296">*質問の編集*</span><span class="sxs-lookup"><span data-stu-id="d18bf-296">*Question edited*</span></span>
4. <span data-ttu-id="d18bf-297">Visual Studio に戻り、開いているスイッチ**Index.cshtml**開いていない場合、します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="d18bf-298">注意しての内部テキ スト、 **&lt;p&gt;** 要素が更新されました。</span><span class="sxs-lookup"><span data-stu-id="d18bf-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="d18bf-299">![HTML ページで更新された質問](visual-studio-2013-web-tools/_static/image30.png "HTML ページに最新の質問")</span><span class="sxs-lookup"><span data-stu-id="d18bf-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="d18bf-300">*HTML ページで、更新された質問*</span><span class="sxs-lookup"><span data-stu-id="d18bf-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="d18bf-301">タスク 5 - 変更履歴の SEO に関連する警告</span><span class="sxs-lookup"><span data-stu-id="d18bf-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="d18bf-302">**検索エンジンの最適化**(SEO) とは、結果の検索エンジンの一覧で web サイトのランクを与えるをするプロセスです。</span><span class="sxs-lookup"><span data-stu-id="d18bf-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="d18bf-303">サイトの順位が高いほど、記載されているより一貫した多くの訪問者、サイトは、その検索エンジンから取得されます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="d18bf-304">Web Essentials には、HTML を検査する分析ツールが組み込まれています、問題のレポートは検出し、それを修正するアシスタンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="d18bf-305">移動して、**ビュー**メニューをクリックします**エラー一覧**を開く、**エラー一覧**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="d18bf-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="d18bf-306">![エラーをビューに一覧表示メニューの](visual-studio-2013-web-tools/_static/image31.png "表示 メニューのエラー一覧")</span><span class="sxs-lookup"><span data-stu-id="d18bf-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="d18bf-307">*エラーをビューに一覧表示メニュー*</span><span class="sxs-lookup"><span data-stu-id="d18bf-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="d18bf-308">通知を SEO 警告があることに注意してください、 **&lt;メタ&gt;** タグ ページの説明がありません。</span><span class="sxs-lookup"><span data-stu-id="d18bf-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="d18bf-309">これを修正する SEO 警告エントリをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="d18bf-310">![エラー一覧ウィンドウ](visual-studio-2013-web-tools/_static/image32.png "エラー一覧ウィンドウ")</span><span class="sxs-lookup"><span data-stu-id="d18bf-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="d18bf-311">*エラー一覧ウィンドウ*</span><span class="sxs-lookup"><span data-stu-id="d18bf-311">*Error List window*</span></span>
3. <span data-ttu-id="d18bf-312">**Web Essentials**ダイアログ ボックスで、をクリックして**はい**説明を挿入する&lt;メタ&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="d18bf-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="d18bf-313">![Web Essentials ダイアログ ボックス](visual-studio-2013-web-tools/_static/image33.png "Web Essentials ダイアログ ボックス")</span><span class="sxs-lookup"><span data-stu-id="d18bf-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="d18bf-314">*Web Essentials ダイアログ ボックス*</span><span class="sxs-lookup"><span data-stu-id="d18bf-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="d18bf-315">用のエディター **\_Layout.cshtml**開きます、 **&lt;メタ&gt;** にタグが自動的に追加、**ヘッド**のセクション、HTML ファイルです。</span><span class="sxs-lookup"><span data-stu-id="d18bf-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="d18bf-316">![自動的に追加されます (_l) ページに Meta タグ](visual-studio-2013-web-tools/_static/image34.png "(_l) ページに自動的に追加のメタ タグ")</span><span class="sxs-lookup"><span data-stu-id="d18bf-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="d18bf-317">*自動的に追加のメタ タグ\_レイアウト ページ*</span><span class="sxs-lookup"><span data-stu-id="d18bf-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="d18bf-318">値を変更、**コンテンツ**属性を*GeekQuiz*ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="d18bf-319">手順 2: をコード スニペットと IntelliSense の利用</span><span class="sxs-lookup"><span data-stu-id="d18bf-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="d18bf-320">Web essentials、余分な機能を持つ、HTML エディターが拡張されました。</span><span class="sxs-lookup"><span data-stu-id="d18bf-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="d18bf-321">この演習では、web アプリケーションの開発に役立つものをいくつかの新しい機能が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="d18bf-322">タスク 1 - HTML ドキュメントで IntelliSense の使用</span><span class="sxs-lookup"><span data-stu-id="d18bf-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="d18bf-323">このタスクに表示される最初の新機能と呼びます**動的 IntelliSense**します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="d18bf-324">動的 IntelliSense では、その他のタグとは使用可能な id を推論する属性を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="d18bf-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="d18bf-325">このタスクでは、ラベルと、入力フィールドが含まれた新しい HTML フォーム要素を作成します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="d18bf-326">追加し、**の**属性に、入力に連結するラベルをスコープ内の入力の id に基づく IntelliSense による候補が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="d18bf-327">開いている**Visual Studio Express 2013 for Web**と**Begin.sln**ソリューション、**ソース/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/開始**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="d18bf-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="d18bf-328">または、前の手順で取得したソリューションを続行できます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="d18bf-329">**ソリューション エクスプ ローラー**、オープン、 **Index.cshtml**にあるファイル、**ビュー** | **ホーム**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="d18bf-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="d18bf-330">内で、次の形式を追加、 **&lt;セクション&gt;** 要素。</span><span class="sxs-lookup"><span data-stu-id="d18bf-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="d18bf-331">(コード スニペット - *VisualStudio2013WebTooling* - *Ex2* - *フォーム*)</span><span class="sxs-lookup"><span data-stu-id="d18bf-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="d18bf-332">入力タグは、フィールドのいくつかの説明のラベルによって先行されなければなりません。</span><span class="sxs-lookup"><span data-stu-id="d18bf-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="d18bf-333">入力タグの前に次のラベルを追加します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="d18bf-334">**の**の属性を**&lt;ラベル&gt;** にラベルをフォーム要素がバインドされているを指定します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="d18bf-335">属性の値は、関連する要素の id と同じでなければなりません。</span><span class="sxs-lookup"><span data-stu-id="d18bf-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="d18bf-336">追加、**の**属性を**&lt;ラベル&gt;** 要素。</span><span class="sxs-lookup"><span data-stu-id="d18bf-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="d18bf-337">次の図に示すように、&quot;名前&quot;値が表示されます、[IntelliSense] ボックスで、同じスコープ内の要素の id に基づく (囲んでいる**&lt;フォーム&gt;**)。</span><span class="sxs-lookup"><span data-stu-id="d18bf-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="d18bf-338">![IntelliSense で、id を示す](visual-studio-2013-web-tools/_static/image35.png "IntelliSense 内の id を表示")</span><span class="sxs-lookup"><span data-stu-id="d18bf-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="d18bf-339">*IntelliSense 内の id を表示*</span><span class="sxs-lookup"><span data-stu-id="d18bf-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="d18bf-340">最近追加された削除**&lt;フォーム&gt;** 要素とそのコンテンツ。</span><span class="sxs-lookup"><span data-stu-id="d18bf-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="d18bf-341">タスク 2 - を使用して HTML コード スニペット</span><span class="sxs-lookup"><span data-stu-id="d18bf-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="d18bf-342">HTML5 では、25 を超える新しいセマンティック タグが導入されました。</span><span class="sxs-lookup"><span data-stu-id="d18bf-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="d18bf-343">Visual Studio が IntelliSense でこれらのタグのサポートに既に存在しますが、Visual Studio 2013 では、高速化し、新しいコード スニペットを追加することでマークアップを記述しやすくなります。 します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="d18bf-344">これらのタグが複雑でない場合、これらにはの正しいコーデック フォールバックを追加するなど、いくつかの小さな細部について、*オーディオ*タグ。</span><span class="sxs-lookup"><span data-stu-id="d18bf-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="d18bf-345">このタスクでは、オーディオ タグの HTML コード スニペットが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="d18bf-346">**Index.cshtml**ファイルに、入力 **&lt;aud**内で、 **&lt;セクション&gt;** 要素の次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="d18bf-347">![要素を挿入するオーディオ](visual-studio-2013-web-tools/_static/image36.png "オーディオの要素を挿入")</span><span class="sxs-lookup"><span data-stu-id="d18bf-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="d18bf-348">*オーディオの要素を挿入*</span><span class="sxs-lookup"><span data-stu-id="d18bf-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="d18bf-349">キーを押して**タブ**2 回ページで、次のコードを追加する方法に注意してください。 および上にカーソルが置かれます、 **src**最初のソースの属性です。</span><span class="sxs-lookup"><span data-stu-id="d18bf-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="d18bf-350">キーを押して、**タブ**キー 2 回、コード スニペットが挿入されます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="d18bf-351">オーディオのスニペットの標準的な使用法を示しています、*オーディオ*サポートの強化を 2 つのソース ファイルのタグ。</span><span class="sxs-lookup"><span data-stu-id="d18bf-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="d18bf-352">2 番目の行を削除し、WebCampsTV Katana 表示するには、次のリンクで最初の行のソースを更新: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3)します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="d18bf-353">結果として得られるコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="d18bf-354">ソース ファイル*KatanaProject.mp3*例として使用されます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="d18bf-355">たい場合は、別のソースを使用できます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="d18bf-356">キーを押して**CTRL** + **S**ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="d18bf-357">キーを押して**CTRL** + **F5**アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="d18bf-358">オーディオ プレーヤーをアプリケーションに追加されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d18bf-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="d18bf-359">![Internet Explorer でのオーディオ プレーヤー](visual-studio-2013-web-tools/_static/image37.png "Internet Explorer でのオーディオ プレーヤー")</span><span class="sxs-lookup"><span data-stu-id="d18bf-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="d18bf-360">*Internet Explorer でのオーディオ プレーヤー*</span><span class="sxs-lookup"><span data-stu-id="d18bf-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="d18bf-361">![Google Chrome でオーディオ プレーヤー](visual-studio-2013-web-tools/_static/image38.png "Google Chrome でオーディオ プレーヤー")</span><span class="sxs-lookup"><span data-stu-id="d18bf-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="d18bf-362">*Google Chrome でオーディオ プレーヤー*</span><span class="sxs-lookup"><span data-stu-id="d18bf-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="d18bf-363">ブラウザーを閉じないでください。</span><span class="sxs-lookup"><span data-stu-id="d18bf-363">Do not close the browsers.</span></span> <span data-ttu-id="d18bf-364">次のタスクでは、それらを使用します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="d18bf-365">タスク 3 - JavaScript のドキュメントで IntelliSense の使用</span><span class="sxs-lookup"><span data-stu-id="d18bf-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="d18bf-366">Web Essentials 2013 では、スタイル シートと HTML ページは、Id 名とクラス名の一覧を生成します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="d18bf-367">このタスクでは、それらのリストが Web Essentials 2013 での JavaScript IntelliSense のサポートを改善する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="d18bf-368">**Index.cshtml**ファイルを定義する次のコードを追加、**スクリプト**JavaScript コードのタグ。</span><span class="sxs-lookup"><span data-stu-id="d18bf-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="d18bf-369">内の次のコードを追加、**スクリプト**準備完了コールバック関数を定義するタグ。</span><span class="sxs-lookup"><span data-stu-id="d18bf-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="d18bf-370">(コード スニペット - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="d18bf-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="d18bf-371">キャレットを置き、**スクリプト**タグとキーを押して**CTRL** + **します。**</span><span class="sxs-lookup"><span data-stu-id="d18bf-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="d18bf-372">提案メニューを開きます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="d18bf-373">クリックして**ファイルに抽出**します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="d18bf-374">![ファイルの提案に抽出する JavaScript](visual-studio-2013-web-tools/_static/image39.png "JavaScript ファイルの提案に抽出します。")</span><span class="sxs-lookup"><span data-stu-id="d18bf-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="d18bf-375">*JavaScript は、ファイルの提案に抽出します。*</span><span class="sxs-lookup"><span data-stu-id="d18bf-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="d18bf-376">**名前を付けて保存**ウィンドウで、**スクリプト**フォルダー、ファイルの名前**init.js** をクリック**保存**です。</span><span class="sxs-lookup"><span data-stu-id="d18bf-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="d18bf-377">![名前を付けて保存ウィンドウ](visual-studio-2013-web-tools/_static/image40.png "名前を付けて保存ウィンドウ")</span><span class="sxs-lookup"><span data-stu-id="d18bf-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="d18bf-378">*名前を付けて保存ウィンドウ*</span><span class="sxs-lookup"><span data-stu-id="d18bf-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d18bf-379">**Init.js**ファイルが作成され、スクリプトの内容が、ファイルに移動します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="d18bf-380">![含まれるコンテンツと共に作成 Init.js ファイル](visual-studio-2013-web-tools/_static/image41.png "Init.js ファイルが含まれているコンテンツの作成")</span><span class="sxs-lookup"><span data-stu-id="d18bf-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="d18bf-381">*含まれるコンテンツと共に作成 Init.js ファイル*</span><span class="sxs-lookup"><span data-stu-id="d18bf-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="d18bf-382">開く、 **Index.cshtml**ファイルを開き、script タグをへの参照に置き換えられたことを確認、 **init.js**ファイル。</span><span class="sxs-lookup"><span data-stu-id="d18bf-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="d18bf-383">![Html の参照を Init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js html リファレンス")</span><span class="sxs-lookup"><span data-stu-id="d18bf-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="d18bf-384">*Init.js html リファレンス*</span><span class="sxs-lookup"><span data-stu-id="d18bf-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="d18bf-385">移動して、**ソリューション エクスプ ローラー**ことに注意して、 **init.js**ファイルは、ソリューションに自動的に含まれていた。</span><span class="sxs-lookup"><span data-stu-id="d18bf-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="d18bf-386">![ソリューションに含まれる Init.js ファイル](visual-studio-2013-web-tools/_static/image43.png "Init.js ファイルがソリューションに含まれる")</span><span class="sxs-lookup"><span data-stu-id="d18bf-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="d18bf-387">*ソリューションに含まれる Init.js ファイル*</span><span class="sxs-lookup"><span data-stu-id="d18bf-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="d18bf-388">戻り、 **init.js**更新ファイルを**準備**コールバック関数。</span><span class="sxs-lookup"><span data-stu-id="d18bf-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="d18bf-389">渡される関数コールバック定義内で*準備*、特定のクラスの属性によってすべての要素を取得する次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="d18bf-390">キーを押して**CTRL** + **領域**内の引用符の間、 **getElementsByClassName**関数呼び出し。</span><span class="sxs-lookup"><span data-stu-id="d18bf-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="d18bf-391">![GetElementsByClassName 関数の IntelliSense を示す](visual-studio-2013-web-tools/_static/image44.png "getElementsByClassName 関数を示す IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="d18bf-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="d18bf-392">*GetElementsByClassName 関数の IntelliSense の表示*</span><span class="sxs-lookup"><span data-stu-id="d18bf-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d18bf-393">プロジェクトのスタイル シートで定義されたクラスが IntelliSense に表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="d18bf-394">次のコードを使用して作成する行を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="d18bf-395">後ろにカーソルを置きます**au**で引用符で囲んで、 **getElementsByTagName**関数およびキーを押して**CTRL** + **領域**.</span><span class="sxs-lookup"><span data-stu-id="d18bf-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="d18bf-396">![GetElementByTagName メソッドの IntelliSense を示す](visual-studio-2013-web-tools/_static/image45.png "getElementByTagName メソッド用の IntelliSense の表示")</span><span class="sxs-lookup"><span data-stu-id="d18bf-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="d18bf-397">*GetElementsByTagName メソッドを示す IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="d18bf-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="d18bf-398">選択**&quot;オーディオ&quot;** キーを押して、一覧から**ENTER**します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="d18bf-399">結果を次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="d18bf-400">![オーディオの要素を取得する](visual-studio-2013-web-tools/_static/image46.png "オーディオ要素を取得します。")</span><span class="sxs-lookup"><span data-stu-id="d18bf-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="d18bf-401">*オーディオの要素を取得します。*</span><span class="sxs-lookup"><span data-stu-id="d18bf-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="d18bf-402">**ソリューション エクスプ ローラー**を右クリックし、 **init.js**ファイル、**スクリプト**フォルダーと選択**アンミニファイ JavaScript ファイル**から、**Web Essentials**メニュー。</span><span class="sxs-lookup"><span data-stu-id="d18bf-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="d18bf-403">![JavaScript ファイルを縮小](visual-studio-2013-web-tools/_static/image47.png "アンミニファイ JavaScript ファイル")</span><span class="sxs-lookup"><span data-stu-id="d18bf-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="d18bf-404">*JavaScript ファイルを縮小します。*</span><span class="sxs-lookup"><span data-stu-id="d18bf-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="d18bf-405">ソース ファイルの変更をクリックすると、自動縮小を有効にするように求められたら**はい**です。</span><span class="sxs-lookup"><span data-stu-id="d18bf-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="d18bf-406">![自動縮小の警告を有効にする](visual-studio-2013-web-tools/_static/image48.png "自動縮小の警告を有効にします。")</span><span class="sxs-lookup"><span data-stu-id="d18bf-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="d18bf-407">*自動縮小の警告を有効にします。*</span><span class="sxs-lookup"><span data-stu-id="d18bf-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d18bf-408">**Init.min.js**が作成され、追加の依存関係として、 **init.js**ファイル。</span><span class="sxs-lookup"><span data-stu-id="d18bf-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="d18bf-409">![作成した Init.min.js ファイル](visual-studio-2013-web-tools/_static/image49.png "Init.min.js ファイルの作成")</span><span class="sxs-lookup"><span data-stu-id="d18bf-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="d18bf-410">*Init.min.js ファイルの作成*</span><span class="sxs-lookup"><span data-stu-id="d18bf-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="d18bf-411">開く、 **init.min.js**ファイルを開き、ファイルを縮小ことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d18bf-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="d18bf-412">![ファイルの内容を Init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js ファイルの内容")</span><span class="sxs-lookup"><span data-stu-id="d18bf-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="d18bf-413">*Init.min.js ファイルの内容*</span><span class="sxs-lookup"><span data-stu-id="d18bf-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="d18bf-414">**Init.js**ファイルに、次の次のコードを追加、 **getElementsByTagName**関数呼び出しにオーディオのすべての要素を再生します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="d18bf-415">(コード スニペット - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="d18bf-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="d18bf-416">クリックして**CTRL** + **S**ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="d18bf-417">縮小されたファイルが既に開かれているために、ソース エディター以外でファイルが変更されたことを通知するダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d18bf-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="d18bf-418">**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-418">Click **Yes**.</span></span>

    <span data-ttu-id="d18bf-419">![Microsoft Visual Studio 警告](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio 警告")</span><span class="sxs-lookup"><span data-stu-id="d18bf-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="d18bf-420">*Microsoft Visual Studio 警告*</span><span class="sxs-lookup"><span data-stu-id="d18bf-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="d18bf-421">戻り、 **init.min.js**ファイルを新しいコードで、ファイルが更新されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="d18bf-422">![更新された Init.min.js ファイル](visual-studio-2013-web-tools/_static/image52.png "Init.min.js ファイルが更新されました")</span><span class="sxs-lookup"><span data-stu-id="d18bf-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="d18bf-423">*Init.min.js ファイルが更新されました*</span><span class="sxs-lookup"><span data-stu-id="d18bf-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="d18bf-424">をクリックして、**ブラウザー リンクの更新**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d18bf-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="d18bf-425">両方のブラウザーが更新されると、前のタスクで学習したオーディオ プレーヤーは自動的に再生を開始します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="d18bf-426">![ビューに含まれるオーディオ プレーヤー](visual-studio-2013-web-tools/_static/image53.png "ビューに含まれるオーディオ プレーヤー")</span><span class="sxs-lookup"><span data-stu-id="d18bf-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="d18bf-427">*ビューに含まれるオーディオ プレーヤー*</span><span class="sxs-lookup"><span data-stu-id="d18bf-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="d18bf-428">まとめ</span><span class="sxs-lookup"><span data-stu-id="d18bf-428">Summary</span></span>

<span data-ttu-id="d18bf-429">このハンズオン ラボについて説明した方法。</span><span class="sxs-lookup"><span data-stu-id="d18bf-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="d18bf-430">豊富な HTML5 コード スニペットや Zen コーディングなど、Web Essentials に含まれる HTML エディターの新機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="d18bf-431">カラー ピッカーやブラウザー マトリックス ツールヒントなど、Web Essentials に含まれる CSS エディターの新機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="d18bf-432">すべての HTML 要素を抽出するファイルと IntelliSense などの Web Essentials に含まれる JavaScript エディターの新機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="d18bf-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="d18bf-433">ブラウザーとブラウザー リンクを使用して Visual Studio 間の Exchange データ</span><span class="sxs-lookup"><span data-stu-id="d18bf-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
