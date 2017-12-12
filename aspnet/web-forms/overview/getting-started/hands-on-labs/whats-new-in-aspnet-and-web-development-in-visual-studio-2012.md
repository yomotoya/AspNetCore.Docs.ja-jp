---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: "ASP.NET および Visual Studio 2012 での Web 開発の新機能 |Microsoft ドキュメント"
author: rick-anderson
description: "新しいバージョンの Visual Studio には、さまざまな Web テクノロジを使用する場合、エクスペリエンスとパフォーマンスを向上させることに重点を置いた機能強化が導入されています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: e57f1200aaa207c9109f2832cbf88629ed385bb5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a><span data-ttu-id="8f6e2-103">ASP.NET および Visual Studio 2012 での Web 開発の新機能</span><span class="sxs-lookup"><span data-stu-id="8f6e2-103">What's New in ASP.NET and Web Development in Visual Studio 2012</span></span>
====================
<span data-ttu-id="8f6e2-104">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="8f6e2-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="8f6e2-105">新しいバージョンの Visual Studio には、さまざまな Web テクノロジを使用する場合、エクスペリエンスとパフォーマンスを向上させることに重点を置いた機能強化が導入されています。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-105">The new version of Visual Studio introduces a number of enhancements focused on improving the experience and performance when working with Web technologies.</span></span> <span data-ttu-id="8f6e2-106">CSS、JavaScript と HTML の visual Studio エディターは、IntelliSense や自動インデントなど、最もでオンデマンド コード補助機能の多くを完全に刷新されました。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-106">Visual Studio Editors for CSS, JavaScript and HTML have been completely revamped to include many of the most in-demand code aids, such as IntelliSense and automatic indentation.</span></span> <span data-ttu-id="8f6e2-107">パフォーマンスに関してバンドルと縮小が統合されたページを簡単に削減を組み込みの機能の読み込み時にします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-107">Regarding performance, bundling and minification are now integrated as built-in features to easily reduce page load time.</span></span>
> 
> <span data-ttu-id="8f6e2-108">Visual Studio では、web サイトの最新のテクノロジを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-108">Visual Studio enables you to work with the latest website technologies.</span></span> <span data-ttu-id="8f6e2-109">クロス ブラウザー CSS3 スニペットを使用するも、新しい HTML5 要素と機能の利点を活かしながら、サイトがクライアントのプラットフォームに関係なく動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-109">You can use cross-browser CSS3 Snippets to make sure your site works regardless of the client platform while also taking advantage of the new HTML5 elements and features.</span></span>
> 
> <span data-ttu-id="8f6e2-110">作成および JavaScript コードのプロファイリングは、この Visual Studio のバージョンを簡単にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-110">Writing and profiling JavaScript code should be easier with this Visual Studio version.</span></span> <span data-ttu-id="8f6e2-111">IntelliSense リストでは、XML ドキュメントとナビゲーションの統合の機能は、JavaScript コードにできます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-111">IntelliSense lists, integrated XML documentation and navigation features are now available for JavaScript code.</span></span> <span data-ttu-id="8f6e2-112">すぐに JavaScript カタログがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-112">You now have the JavaScript catalog at your fingertips.</span></span> <span data-ttu-id="8f6e2-113">さらに、スクリプトで ECMAScript5 コンプライアンスを確認し、早い段階で構文エラーを検出できます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-113">Additionally, you can check ECMAScript5 compliance with your scripts and detect syntax errors at an early stage.</span></span>
> 
> <span data-ttu-id="8f6e2-114">最後はさらに、このバージョンの Visual Studio は、組み込みのバンドルと縮小を実装します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-114">Last but not least, this Visual Studio version implements built-in bundling and minification.</span></span> <span data-ttu-id="8f6e2-115">スタイル シート、スクリプト ファイルがパックし、サイトを高速実行するように圧縮します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-115">Your script files and style sheets will be packed and compressed so that the site performs faster.</span></span>
> 
> <span data-ttu-id="8f6e2-116">このラボには、機能強化と軽微な変更を元のフォルダーで提供されるサンプル Web アプリケーションに適用することで前に説明した新しい機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="8f6e2-117">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-117">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="8f6e2-118">目的</span><span class="sxs-lookup"><span data-stu-id="8f6e2-118">Objectives</span></span>

<span data-ttu-id="8f6e2-119">このハンズ オン ラボで学習する方法。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-119">In this hands on lab, you will learn how to:</span></span>

- <span data-ttu-id="8f6e2-120">CSS エディターの新機能と機能強化を使用します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-120">Use the new features and improvements in the CSS editor</span></span>
- <span data-ttu-id="8f6e2-121">HTML エディターの新機能と機能強化を使用します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-121">Use the new features and improvements in the HTML editor</span></span>
- <span data-ttu-id="8f6e2-122">JavaScript エディターの新機能と機能強化を使用します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-122">Use the new features and improvements in the JavaScript editor</span></span>
- <span data-ttu-id="8f6e2-123">構成および使用のバンドルと縮小</span><span class="sxs-lookup"><span data-stu-id="8f6e2-123">Configure and use bundling and minification</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="8f6e2-124">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="8f6e2-124">Prerequisites</span></span>

- <span data-ttu-id="8f6e2-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="8f6e2-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (用セットアップ スクリプトに Windows 8 および Windows Server 2008 R2 に既にインストールされている)</span><span class="sxs-lookup"><span data-stu-id="8f6e2-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (for setup scripts - already installed on Windows 8 and Windows Server 2008 R2)</span></span>
- <span data-ttu-id="8f6e2-127">[Internet Explorer 10](https://windows.microsoft.com/en-US/internet-explorer/products/ie/home)または HTML5 対応ブラウザー</span><span class="sxs-lookup"><span data-stu-id="8f6e2-127">[Internet Explorer 10](https://windows.microsoft.com/en-US/internet-explorer/products/ie/home) - or an HTML5 compliant browser</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="8f6e2-128">演習</span><span class="sxs-lookup"><span data-stu-id="8f6e2-128">Exercises</span></span>

<span data-ttu-id="8f6e2-129">このハンズ オン ラボには、次の実習が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-129">This hands on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="8f6e2-130">CSS エディターの新機能はどのような演習 1 です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-130">Exercise 1: What's New in the CSS Editor</span></span>](#Exercise1)
2. [<span data-ttu-id="8f6e2-131">新しい HTML エディターではどのような演習 2 です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-131">Exercise 2: What's New in the HTML Editor</span></span>](#Exercise2)
3. [<span data-ttu-id="8f6e2-132">手順 3: は、JavaScript エディターの新機能</span><span class="sxs-lookup"><span data-stu-id="8f6e2-132">Exercise 3: What's New in the JavaScript Editor</span></span>](#Exercise3)
4. [<span data-ttu-id="8f6e2-133">手順 4: バンドルと縮小</span><span class="sxs-lookup"><span data-stu-id="8f6e2-133">Exercise 4: Bundling and Minification</span></span>](#Exercise4)

<span data-ttu-id="8f6e2-134">この演習を完了する時間を推定: **60 分**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-134">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a><span data-ttu-id="8f6e2-135">CSS エディターの新機能はどのような演習 1 です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-135">Exercise 1: What's New in the CSS Editor</span></span>

<span data-ttu-id="8f6e2-136">Web 開発者は、CSS の編集に関連する問題の多くと理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-136">Web developers should be familiar with many of the difficulties related to CSS editing.</span></span> <span data-ttu-id="8f6e2-137">CSS スタイルの最大の問題の 1 つは、クロス ブラウザーの互換性です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-137">One of the biggest issues of CSS styling is cross-browser compatibility.</span></span> <span data-ttu-id="8f6e2-138">多くの場合、スタイルを適用すると、サイトに、わかりますをさまざまな参照別のブラウザーまたはデバイスで開く場合行われます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-138">It often happens that, after applying styles to your site, you notice that it looks different if you open it in another browser or device.</span></span> <span data-ttu-id="8f6e2-139">そのため、それらに注意して、最後に、1 つのブラウザーで作業を行うときに分割されるその他の視覚的な問題を修正するのにかなりの時間を費やす可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-139">Therefore, you may spend a considerable time on fixing those visual issues to realize that, when you finally make it work in one browser, it is broken in the others.</span></span>

<span data-ttu-id="8f6e2-140">Visual Studio には、開発者にアクセスし、作業、CSS スタイル シートを効果的に整理に役立つ機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-140">Visual Studio now includes features that help developers access, work and organize CSS style sheets effectively.</span></span> <span data-ttu-id="8f6e2-141">この演習で効果的な組織や edition の新機能および css3 用のコード スニペットのクロス ブラウザーの互換性が満たされます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-141">Throughout this exercise, you will meet the new features for an effective organization and edition, as well as the CSS3 Code Snippets for cross-browser compatibility.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a><span data-ttu-id="8f6e2-142">タスク 1 - エディターの新機能</span><span class="sxs-lookup"><span data-stu-id="8f6e2-142">Task 1 - New Editor Features</span></span>

<span data-ttu-id="8f6e2-143">このタスクは、CSS エディターの新しい機能を検出します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-143">In this task, you will discover the new features of the CSS Editor.</span></span> <span data-ttu-id="8f6e2-144">この新しいエディターには、新しいスマート インデント、強化されたコードのコメントおよび IntelliSense の拡張の一覧を活用して生産性を向上するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-144">This new editor will help you increase your productivity by taking advantage of the new smart indentation, the improved code comments and the enhanced IntelliSense list.</span></span>

1. <span data-ttu-id="8f6e2-145">開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**にソリューションがある、 **Source\WhatsNewASPNET**このラボのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-145">Start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="8f6e2-146">ソリューション エクスプ ローラーで開く、 **Site.css**の下にあるファイル、**スタイル**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-146">In Solution Explorer, open the **Site.css** file located under the **Styles** folder.</span></span> <span data-ttu-id="8f6e2-147">確認、**テキスト エディター**ツールは、ツールバーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-147">Make sure the **Text Editor** tools are visible on the toolbar.</span></span> <span data-ttu-id="8f6e2-148">実行するには、選択、**ビュー** | **ツールバー**メニュー オプションとチェック、**テキスト エディター**オプション。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-148">To do that, select the **View** | **Toolbars** menu option, and check the **Text Editor** options.</span></span> <span data-ttu-id="8f6e2-149">表示になります、この新しいバージョンでは、以降、**コメント**ボタン (![コメント ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png)) および**コメント解除**ボタン (![のコメントを解除-ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png))、CSS エディターのも有効にします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-149">You will notice that, since this new version, the **Comment** button (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) and the **Uncomment** button (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) ) are also enabled for the CSS editor.</span></span>

    <span data-ttu-id="8f6e2-150">![エディターと CSS ツールを有効にする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "エディターと CSS ツールを有効にします。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-150">![Enabling Editor and CSS Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Enabling Editor and CSS Tools")</span></span>

    <span data-ttu-id="8f6e2-151">*エディターと CSS ツールを有効にします。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-151">*Enabling Editor and CSS Tools*</span></span>
3. <span data-ttu-id="8f6e2-152">コードをスクロールし、任意の CSS クラスの定義を選択します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-152">Scroll the code and select any CSS class definition.</span></span> <span data-ttu-id="8f6e2-153">をクリックして、**コメント**(![コメント ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png)) 選択した行をコメントするボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-153">Click the **Comment** (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) button to comment the selected lines.</span></span> <span data-ttu-id="8f6e2-154">[] をクリックして、**コメント解除**(![のコメントを解除](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) 変更を元に戻すボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-154">Then, click the **Uncomment** (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) button to undo the changes.</span></span>
4. <span data-ttu-id="8f6e2-155">をクリックして、**折りたたむ**(![折りたたむ](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png)) と**展開**(![展開](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png)) テキストの左余白のボタンがあります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-155">Click the **Collapse** (![collapse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) and **Expand** (![expand](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) buttons located on the left margin of the text.</span></span> <span data-ttu-id="8f6e2-156">クリーナー ビューを使用しないスタイルを隠すことができるようになりましたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-156">Notice that you can now hide the styles you don't use to have a cleaner view.</span></span>

    <span data-ttu-id="8f6e2-157">![CSS クラスの折りたたみ](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "折りたたみ CSS クラス")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-157">![Collapsing CSS classes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Collapsing CSS classes")</span></span>

    <span data-ttu-id="8f6e2-158">*CSS クラスの折りたたみ*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-158">*Collapsing CSS classes*</span></span>
5. <span data-ttu-id="8f6e2-159">スマート インデント機能が有効になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-159">Make sure that the smart indentation feature is enabled.</span></span> <span data-ttu-id="8f6e2-160">選択、**ツール** | **オプション**メニュー オプションをクリックし、**テキスト エディター** | **CSS**  | **書式**画面の左側のウィンドウ内のページです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-160">Select the **Tools** | **Options** menu option, and then select the **Text Editor** | **CSS** | **Formatting** page in the left pane of the screen.</span></span> <span data-ttu-id="8f6e2-161">チェック、**階層インデント**オプション。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-161">Check the **Hierarchical indentation** option.</span></span>

    <span data-ttu-id="8f6e2-162">![階層構造のインデントを有効にする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "階層インデントを有効にします。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-162">![Enabling hierarchical indentation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Enabling hierarchical indentation")</span></span>

    <span data-ttu-id="8f6e2-163">*階層構造のインデントを有効にします。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-163">*Enabling hierarchical indentation*</span></span>
6. <span data-ttu-id="8f6e2-164">メイン クラス定義 (.main) を見つけて、div 要素にスタイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-164">Locate the main class definition (.main) and append a style to the div elements.</span></span> <span data-ttu-id="8f6e2-165">コードは、配置を一目で、親クラスを検索するユーザーを支援することで、自動的に表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-165">You will notice that the code aligns automatically, helping users to find the parent classes at a glance.</span></span>

    <span data-ttu-id="8f6e2-166">CSS</span><span class="sxs-lookup"><span data-stu-id="8f6e2-166">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    <span data-ttu-id="8f6e2-167">![Css 階層的な配置](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "CSS 内の階層の配置")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-167">![Hierarchical alignment in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchical alignment in CSS")</span></span>

    <span data-ttu-id="8f6e2-168">*CSS 内の階層の配置*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-168">*Hierarchical alignment in CSS*</span></span>
7. <span data-ttu-id="8f6e2-169">内**.main div**クラスの末尾にカーソルを置いて**境界線: 0 px;**とキーを押します**Enter** IntelliSense の一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-169">Inside **.main div** class, locate the cursor at the end of **border: 0px;** and press **Enter** to display the IntelliSense list.</span></span> <span data-ttu-id="8f6e2-170">入力を開始**上部**し、一覧がフィルター処理する方法を入力するように注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-170">Start typing **top** and notice how the list is filtered as you type.</span></span> <span data-ttu-id="8f6e2-171">一覧を含む要素が表示されます**上部**という単語の一部に (Visual Studio の以前のバージョンでは、一覧は、項目でフィルター処理を*開始*という用語と)。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-171">The list will display the elements that contain **top** at any part of the word (In prior versions of Visual Studio, the list is filtered by the items that *begin* with the term).</span></span>

    <span data-ttu-id="8f6e2-172">![Css IntelliSense の機能強化](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "css IntelliSense の機能強化")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-172">![IntelliSense enhancements in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense enhancements in CSS")</span></span>

    <span data-ttu-id="8f6e2-173">*Css IntelliSense の機能強化*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-173">*IntelliSense enhancements in CSS*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a><span data-ttu-id="8f6e2-174">タスク 2 - カラー ピッカー</span><span class="sxs-lookup"><span data-stu-id="8f6e2-174">Task 2 - The Color Picker</span></span>

<span data-ttu-id="8f6e2-175">このタスクでは、Visual Studio IntelliSense に統合されて、新しい CSS カラー ピッカーを検出します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-175">In this task, you will discover the new CSS Color Picker integrated into Visual Studio IntelliSense.</span></span>

1. <span data-ttu-id="8f6e2-176">**Site.css、**ヘッダー クラス定義 (.header) を見つけて、カーソルを置き、横に**背景色**属性間、 &quot;:&quot;と&quot; #&quot;の文字のコード行を**です。**</span><span class="sxs-lookup"><span data-stu-id="8f6e2-176">In **Site.css,** locate the header class definition (.header) and place the cursor next to **background-color** attribute, between the &quot;:&quot; and &quot;#&quot; characters on that line of code **.**</span></span>

    <span data-ttu-id="8f6e2-177">![カーソルを検索する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "カーソルを検索します。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-177">![Locating the cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Locating the cursor")</span></span>

    <span data-ttu-id="8f6e2-178">*カーソルを検索します。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-178">*Locating the cursor*</span></span>
2. <span data-ttu-id="8f6e2-179">削除、**コロン**(:) し、もう一度表示するように、カラー ピッカーを記述します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-179">Delete the **colon** (:) and write it again to display the color picker.</span></span> <span data-ttu-id="8f6e2-180">最初の色が表示されますが、サイトの最も一般的な色であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-180">Notice that the first colors you will see are the most frequent colors of your site.</span></span> <span data-ttu-id="8f6e2-181">白い色をクリックした場合、HTML のカラー コード (#fff) は、スタイル シートの現在のカラー コードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-181">If you click the white color, its HTML color code (#fff) will replace the current color code in the stylesheet.</span></span>

    <span data-ttu-id="8f6e2-182">![カラー ピッカー](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "カラー ピッカー")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-182">![Color picker](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Color picker")</span></span>

    <span data-ttu-id="8f6e2-183">*カラー ピッカー*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-183">*Color picker*</span></span>
3. <span data-ttu-id="8f6e2-184">キーを押して、**展開**(![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) 色のグラデーションを表示し、別の色を選択するグラデーションのカーソルをドラッグするには、カラー ピッカーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-184">Press the **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) button on the color picker to display the color gradient, and then drag the gradient cursor to select a different color.</span></span> <span data-ttu-id="8f6e2-185">その後をクリックして、**スポイト**ボタンをクリックし、画面の任意の色を選択します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-185">After that, click the **Eyedropper** button and select any color from the screen.</span></span> <span data-ttu-id="8f6e2-186">カーソルを移動するときに、背景色の値が動的に変更することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-186">Notice that background color value changes dynamically while you move the cursor.</span></span>

    <span data-ttu-id="8f6e2-187">![グラデーション ピッカー](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "グラデーション ピッカー")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-187">![Color picker gradient](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Color picker gradient")</span></span>

    <span data-ttu-id="8f6e2-188">*グラデーション ピッカー*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-188">*Color picker gradient*</span></span>
4. <span data-ttu-id="8f6e2-189">**不透明度**スライダー、セレクターを不透明度を減らすには、バーの中央に移動します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-189">In the **Opacity** slider, move the selector to the center of the bar to reduce the opacity.</span></span> <span data-ttu-id="8f6e2-190">背景色の値は、RGBA にスケールを今すぐ変更に注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-190">Notice that background-color value now changes its scale to RGBA.</span></span>

    <span data-ttu-id="8f6e2-191">![カラー ピッカーの不透明度](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "カラー ピッカーの不透明度")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-191">![Color picker Opacity](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Color picker Opacity")</span></span>

    <span data-ttu-id="8f6e2-192">*カラー ピッカーの不透明度*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-192">*Color picker Opacity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f6e2-193">CSS3 で RGBA (赤、緑、青、Alpha) 色定義では、1 つの項目の色の不透明度の値を定義することができます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-193">The RGBA (Red, Green, Blue, Alpha) color definition in CSS3 enables you to define the color opacity value for a single item.</span></span> <span data-ttu-id="8f6e2-194">異なり**不透明度 -**のような CSS 属性 **-**  RGBA 色は、最新のブラウザーと互換性があります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-194">Unlike **opacity -** a similar CSS attribute **-** RGBA colors are also compatible with the latest browsers.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a><span data-ttu-id="8f6e2-195">タスク 3 - CSS 互換性のあるコード スニペット</span><span class="sxs-lookup"><span data-stu-id="8f6e2-195">Task 3 - CSS Compatible Code Snippets</span></span>

<span data-ttu-id="8f6e2-196">このタスクでは、web サイトでいくつかの機能を実装するためにクロス ブラウザーの互換性のある CSS3 スニペットを使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-196">In this task, you will learn how to use cross-browser compatible CSS3 snippets in order to implement some features in your website.</span></span>

1. <span data-ttu-id="8f6e2-197">**Site.css**ファイルを調べ、**ヘッダー** CSS クラス定義 (.header) および下にカーソルを置き、  **/\*罫線 radius\* /**新しいスニペットを追加するプレース ホルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-197">In the **Site.css** file, locate the **header** CSS class definition (.header) and place the cursor below the **/\*border radius\*/** placeholder to add a new snippet.</span></span> <span data-ttu-id="8f6e2-198">キーを押して**Enter** 、IntelliSense の一覧を表示し**radius**一覧をフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-198">Press **Enter** to display the IntelliSense list and type **radius** to filter the list.</span></span> <span data-ttu-id="8f6e2-199">選択、**罫線 radius**二重のクリックで、一覧からオプションをクリックして、**タブ**スニペットを挿入するキー。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-199">Select the **border-radius** option from the list with a double click, and then press the **TAB** key to insert the snippet.</span></span> <span data-ttu-id="8f6e2-200">Radius サイズを入力し、ピクセルとキーを押して**Enter**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-200">Then, type a radius size in pixels and press **Enter**.</span></span> <span data-ttu-id="8f6e2-201">たとえば、入力**15px**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-201">For instance, type **15px**.</span></span>

    <span data-ttu-id="8f6e2-202">スニペットによって追加された CSS3 属性は、Mozilla WebKit ベースのブラウザーなど、ほとんどの HTML5 対応ブラウザーに境界線を曲線で表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-202">The CSS3 attributes added by the snippet will render rounded borders in most HTML5 compliance browsers, including Mozilla and WebKit-based browsers.</span></span>

    <span data-ttu-id="8f6e2-203">![罫線は、radius スニペットを使用して](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "罫線 radius スニペットを使用して")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-203">![Using a border-radius snippet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Using a border-radius snippet")</span></span>

    <span data-ttu-id="8f6e2-204">*罫線は、radius スニペットを使用してください。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-204">*Using a border-radius snippet*</span></span>
2. <span data-ttu-id="8f6e2-205">同じ**罫線**スニペット ページのスタイル (.page) にします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-205">Apply the same **border** snippets in the page style (.page).</span></span>

    <span data-ttu-id="8f6e2-206">CSS</span><span class="sxs-lookup"><span data-stu-id="8f6e2-206">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. <span data-ttu-id="8f6e2-207">キーを押して**f5 キーを押して**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-207">Press **F5** to run the solution.</span></span> <span data-ttu-id="8f6e2-208">つまり各ページようになりましたが、丸めた罫線に注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-208">Notice that each page now has rounded borders.</span></span>

    <span data-ttu-id="8f6e2-209">![角を丸く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "角を丸く")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-209">![Rounded corners](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Rounded corners")</span></span>

    <span data-ttu-id="8f6e2-210">*角が丸い*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-210">*Rounded corners*</span></span>
4. <span data-ttu-id="8f6e2-211">ブラウザーを閉じて、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-211">Close the browser and return to Visual Studio.</span></span>
5. <span data-ttu-id="8f6e2-212">開く、 **Custom.css**の下にあるファイル、**スタイル**フォルダー内のカーソルを置く**div.images ul li img**クラス定義です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-212">Open the **Custom.css** file located under the **Styles** folder and place the cursor inside **div.images ul li img** class definition.</span></span>
6. <span data-ttu-id="8f6e2-213">IntelliSense の一覧を表示する enter キーを押します型**ボックス シャドウ**キーを押すと、**タブ**クラス定義の内部既定シャドウ コード スニペットを挿入するには、2 回のキー。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-213">Press enter to display the IntelliSense list, type **box-shadow** and press the **TAB** key twice to insert the default shadow code snippet inside the class definition.</span></span> <span data-ttu-id="8f6e2-214">シャドウの値に設定**10 px 10 px 5 px #888**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-214">Set the shadow values to **10px 10px 5px #888**.</span></span> <span data-ttu-id="8f6e2-215">次に、入力**罫線 radius**およびコード スニペットを挿入します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-215">Then, type **border-radius** and insert the code snippet.</span></span> <span data-ttu-id="8f6e2-216">型**15px**半径のサイズとキーを押してを設定する**ENTER**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-216">Type **15px** to set radius size and press **ENTER**.</span></span>

    <span data-ttu-id="8f6e2-217">![シャドウと角を丸く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "シャドウ付き角を丸く")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-217">![Rounded corners with shadow](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Rounded corners with shadow")</span></span>

    <span data-ttu-id="8f6e2-218">*シャドウと角の丸い*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-218">*Rounded corners with shadow*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f6e2-219">この時点では、(プロパティ、webkit、o) Mozilla をサポートするために、対応するプレフィックスと Webkit (Chrome、Safari、Konkeror) ブラウザーでシャドウ属性が挿入されます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-219">At this moment, the shadow attribute is inserted with the corresponding prefix (moz, webkit, o) to support Mozilla and Webkit (Chrome, Safari, Konkeror) browsers.</span></span>
7. <span data-ttu-id="8f6e2-220">新しいクラスを作成**div.images ul li img:hover**下、 **div.images ul li img**クラス定義にカーソルを置き、かっこで囲んで**です。**</span><span class="sxs-lookup"><span data-stu-id="8f6e2-220">Create a new class **div.images ul li img:hover** below the **div.images ul li img** class definition and place the cursor inside the brackets **.**</span></span>

    <span data-ttu-id="8f6e2-221">CSS</span><span class="sxs-lookup"><span data-stu-id="8f6e2-221">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. <span data-ttu-id="8f6e2-222">型**変換**キーを押すと、**タブ**変換スニペットを挿入するために 2 回のキー。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-222">Type **transform** and press the **TAB** key twice in order to insert the transform snippet.</span></span> <span data-ttu-id="8f6e2-223">次に、入力**rotate(-15deg)**イメージが置かれているときに、回転の角度の値を変更します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-223">Then, enter **rotate(-15deg)** to change the rotation angle value when images are hovered.</span></span>

    <span data-ttu-id="8f6e2-224">CSS</span><span class="sxs-lookup"><span data-stu-id="8f6e2-224">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. <span data-ttu-id="8f6e2-225">キーを押して**f5 キーを押して**ソリューションを実行し、CSS3 ページを参照します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-225">Press **F5** to run the solution and browse to the CSS3 page.</span></span> <span data-ttu-id="8f6e2-226">イメージがコーナーが丸くし、シャドウをボックスに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-226">Notice that the images have rounded corners and box shadows.</span></span> <span data-ttu-id="8f6e2-227">イメージ上にマウスを移動、回転させることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-227">Hover the mouse over the images and watch them rotate.</span></span>

    <span data-ttu-id="8f6e2-228">![イメージの回転のスニペットを変換](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "イメージを回転変換スニペット")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-228">![Transform snippet rotating an image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transform snippet rotating an image")</span></span>

    <span data-ttu-id="8f6e2-229">*イメージを回転スニペットを変換します。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-229">*Transform snippet rotating an image*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f6e2-230">Internet Explorer 10 を使用するいるし、シャドウを表示することはできません、ドキュメント モードが IE10 標準に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-230">If you are using Internet Explorer 10 and cannot see the shadows, make sure the document mode is set to IE10 standards.</span></span> <span data-ttu-id="8f6e2-231">キーを押して**F12**を Internet Explorer developer tools を開き、をクリックして**ドキュメント モード**IE10 標準に変更します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-231">Press **F12** to open Internet Explorer developer tools and click **Document Mode** to change to IE10 standards.</span></span>

    ![に関する-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a><span data-ttu-id="8f6e2-233">新しい HTML エディターではどのような演習 2 です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-233">Exercise 2: What's New in the HTML Editor</span></span>

<span data-ttu-id="8f6e2-234">Visual Studio には、改善の HTML エディターがあります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-234">Visual Studio has an improved HTML editor.</span></span> <span data-ttu-id="8f6e2-235">このバージョンに含まれる拡張機能の一部は、HTML ドキュメント、HTML5 スニペット、HTML の開始と終了タグに一致する、および HTML の検証のスマート インデントです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-235">Some of the enhancements included in this version are smart indentation in HTML documents, HTML5 snippets, HTML start and end tag matching, and HTML validation.</span></span> <span data-ttu-id="8f6e2-236">この演習では、マークアップを web サイトで作業するときに、これらの変更が、習熟を向上する方法がわかります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-236">Throughout this exercise, you will see how these changes improve your fluency when working in the website markup.</span></span>

<span data-ttu-id="8f6e2-237">CSS エディターのように、HTML エディターも強化されました。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-237">Like the CSS editor, the HTML editor was also improved.</span></span> <span data-ttu-id="8f6e2-238">これらの機能強化のほとんどは、Web 開発者の作業を容易にする小規模なものです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-238">Most of these improvements are small ones that make the Web developer's life easier.</span></span> <span data-ttu-id="8f6e2-239">複数のコード スニペットは、HTML5、スマート インデントは、編集と、HTML ドキュメントの DOCTYPE を対象とする検証がこれらの機能強化の一部である場合、開始タグと終了タグを照合のなどです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-239">Things like more code snippets for HTML5, smart indentation, matching start and end tags when editing and validation targeting the HTML document DOCTYPE are some of these improvements.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a><span data-ttu-id="8f6e2-240">タスク 1 - 強化された DOCTYPE の検証</span><span class="sxs-lookup"><span data-stu-id="8f6e2-240">Task 1 - Improved DOCTYPE Validation</span></span>

<span data-ttu-id="8f6e2-241">HTML エディターになりました、ページの DOCTYPE をチェックするよう場合でも、マスター ページで、定義があります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-241">The HTML editor now has the ability to check the DOCTYPE of your page, even though the definition might be in the master page.</span></span> <span data-ttu-id="8f6e2-242">によっては、ページの DOCTYPE、HTML エディター正しい一連の規則を使用して検証されますが一覧とフィルター IntelliSense DOCTYPE 要素を検討します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-242">Depending on the DOCTYPE of your page, the HTML editor will validate with the correct set of rules and will filter the IntelliSense list considering the DOCTYPE elements.</span></span>

<span data-ttu-id="8f6e2-243">このタスクでは、それに応じて HTML エディターの動作の変化を表示するページの DOCTYPE を変更します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-243">In this task, you will change the DOCTYPE of a page to see how the HTML editor behavior changes accordingly.</span></span>

1. <span data-ttu-id="8f6e2-244">開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**にソリューションがある、 **Source\WhatsNewASPNET**このラボのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-244">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="8f6e2-245">開く、 **Site.Master**ページ。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-245">Open the **Site.Master** page.</span></span>
3. <span data-ttu-id="8f6e2-246">検証のツールバーには、ターゲット スキーマに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-246">Notice the Target Schema for Validation Toolbar.</span></span> <span data-ttu-id="8f6e2-247">HTML エディターの動作 (検証、IntelliSense など) は、選択されている Doctype に合わせて正しく変更されます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-247">The way the HTML editor behaves (Validation, IntelliSense, etc.) will properly change to fit the Doctype selected.</span></span>

    <span data-ttu-id="8f6e2-248">![HTML ソースの編集 ツールバーに Doctype を使用して](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "HTML ソースの編集 ツールバーに Doctype を使用します。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-248">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="8f6e2-249">*Doctype を使用して、HTML ソースの編集ツールバー*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-249">*Use Doctype in HTML Source Editing toolbar*</span></span>
4. <span data-ttu-id="8f6e2-250">HTML 4.01 にターゲット スキーマを変更します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-250">Change the Target Schema to HTML 4.01.</span></span>

    <span data-ttu-id="8f6e2-251">![HTML ソースの編集 ツールバーに Doctype を変更する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "HTML ソースの編集 ツールバーに Doctype を変更します。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-251">![Changing Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Changing Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="8f6e2-252">*HTML ソースの編集 ツールバーに Doctype を変更します。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-252">*Changing Doctype in HTML Source Editing toolbar*</span></span>
5. <span data-ttu-id="8f6e2-253">下にあるカーソルを置き、**本文**要素、および HTML5 の要素の名前を入力する (たとえば、**ビデオ**)。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-253">Place the cursor under the **body** element, and start typing the name of an HTML5 element (for example, **video**).</span></span> <span data-ttu-id="8f6e2-254">要素が IntelliSense リストで使用できないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-254">Notice that the element is not available in the IntelliSense list.</span></span>

    <span data-ttu-id="8f6e2-255">![HTML5 要素が表示されていない](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 要素が表示されていません。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-255">![HTML5 elements not listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elements not listed")</span></span>

    <span data-ttu-id="8f6e2-256">*HTML5 要素が表示されていません。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-256">*HTML5 elements not listed*</span></span>
6. <span data-ttu-id="8f6e2-257">DOCTYPE をピッキング検証ツールバーのターゲット スキーマへの変更を元に戻す: ドロップダウン リストから XHTML5 です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-257">Undo the changes to the Target Schema for Validation Toolbar, picking DOCTYPE: XHTML5 from the dropdown list.</span></span>

    <span data-ttu-id="8f6e2-258">![HTML ソースの編集 ツールバーに Doctype を使用して](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "HTML ソースの編集 ツールバーに Doctype を使用します。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-258">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="8f6e2-259">*Doctype の HTML ソースの編集ツールバーをリセットします。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-259">*Reset Doctype in HTML Source Editing toolbar*</span></span>
7. <span data-ttu-id="8f6e2-260">下にあるカーソルを置き、**本文**HTML5 要素をもう一度入力する要素と (たとえば、**ビデオ**)。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-260">Place the cursor under the **body** element and start typing an HTML5 element again (For example, like **video**).</span></span> <span data-ttu-id="8f6e2-261">HTML5 要素は IntelliSense リストで使用できるようになりましたことに注目してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-261">Notice that the HTML5 elements are now available in the IntelliSense list.</span></span>

    <span data-ttu-id="8f6e2-262">![HTML5 要素が表示される](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 要素が表示されます。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-262">![HTML5 elements being listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elements being listed")</span></span>

    <span data-ttu-id="8f6e2-263">*HTML5 要素が表示されます。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-263">*HTML5 elements being listed*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a><span data-ttu-id="8f6e2-264">タスク 2 - 開始/終了タグを自動更新</span><span class="sxs-lookup"><span data-stu-id="8f6e2-264">Task 2 - Start/End Tags Automatic Update</span></span>

<span data-ttu-id="8f6e2-265">Visual Studio では、開く、または相互に一致するように編集している要素のタグを閉じる HTML 今すぐ更新します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-265">Visual Studio now updates the HTML opening or closing tags of the element that you are editing to match each other.</span></span> <span data-ttu-id="8f6e2-266">この新しい機能には、HTML タグを編集するときに生産性が向上します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-266">This new feature will improve your productivity when editing HTML tags.</span></span>

1. <span data-ttu-id="8f6e2-267">**Default.aspx**  ページで、追加、 **H3**タイトル (たとえば、Visual Studio 2012 もの!) を持つ要素。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-267">On the **Default.aspx** page, add an **H3** element with a title (for example, Visual Studio 2012 Rocks!).</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. <span data-ttu-id="8f6e2-268">変更、 **H3**タグおよび種類**H2**または**H1 です。**</span><span class="sxs-lookup"><span data-stu-id="8f6e2-268">Change the **H3** tag and type **H2** or **H1.**</span></span>

    <span data-ttu-id="8f6e2-269">終了タグが自動的に更新することを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-269">Notice that the end tag automatically updates.</span></span> <span data-ttu-id="8f6e2-270">開始タグを更新するそれに応じてすぎるを表示する終了タグを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-270">You can also modify the end tag to see that the start tag updates accordingly too.</span></span>

    <span data-ttu-id="8f6e2-271">![終了タグの更新プログラムの自動](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "終了タグの自動更新")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-271">![Automatic update of the end tag](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatic update of the end tag")</span></span>

    <span data-ttu-id="8f6e2-272">*終了タグの自動更新*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-272">*Automatic update of the end tag*</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a><span data-ttu-id="8f6e2-273">タスク 3 - 新しい HTML5 コード スニペット</span><span class="sxs-lookup"><span data-stu-id="8f6e2-273">Task 3 - New HTML5 Code Snippets</span></span>

<span data-ttu-id="8f6e2-274">Visual Studio には、いくつかの HTML5 のコード スニペットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-274">Visual Studio now includes several HTML5 code snippets.</span></span> <span data-ttu-id="8f6e2-275">このタスクでは、これらのスニペットの一部を使用します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-275">In this task, you will use some of these snippets.</span></span>

1. <span data-ttu-id="8f6e2-276">という名前の新しいフォルダーを追加**オーディオ**web サイト フォルダーのルートにします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-276">Add a new folder named **audio** to the root of the web site folder.</span></span> <span data-ttu-id="8f6e2-277">Windows エクスプ ローラーを開きに任意のオーディオ ファイルをコピー、**オーディオ**のフォルダー、 **WhatsNewASPNET.sln**ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-277">Open Windows Explorer and copy any audio file into the **audio** folder of the **WhatsNewASPNET.sln** solution.</span></span>
2. <span data-ttu-id="8f6e2-278">**Default.aspx** Web11 ものの下でカーソルを検索 ページで、!!</span><span class="sxs-lookup"><span data-stu-id="8f6e2-278">In the **Default.aspx** page, locate the cursor under the Web11 Rocks!!</span></span> <span data-ttu-id="8f6e2-279">ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-279">Header.</span></span> <span data-ttu-id="8f6e2-280">型**オーディオ**TAB キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-280">Type **audio** and press the TAB key.</span></span>

    <span data-ttu-id="8f6e2-281">新しい HTML エディターには、HTML5 コンテンツのコード スニペットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-281">The new HTML editor includes code snippets for HTML5 content.</span></span> <span data-ttu-id="8f6e2-282">適切な DOCTYPE 定義を使用して HTML5 スニペットを有効にしてください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-282">Remember to use the proper DOCTYPE definition to enable the HTML5 snippets.</span></span>

    <span data-ttu-id="8f6e2-283">![HTML5 のコード スニペットの挿入](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "HTML5 コード スニペットの挿入")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-283">![Inserting HTML5 Code Snippets](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserting HTML5 Code Snippets")</span></span>

    <span data-ttu-id="8f6e2-284">*HTML5 のコード スニペットの挿入*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-284">*Inserting HTML5 Code Snippets*</span></span>
3. <span data-ttu-id="8f6e2-285">既存のオーディオ ファイルを指すオーディオのソースを更新します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-285">Update the audio source to point to an existing audio file.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > <span data-ttu-id="8f6e2-286">オーディオ ファイルをソリューションに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-286">You will need to add the audio file to the solution.</span></span>
4. <span data-ttu-id="8f6e2-287">キーを押して**f5 キーを押して**サイトを実行して、オーディオを再生します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-287">Press **F5** to run the site and play the audio.</span></span>

    <span data-ttu-id="8f6e2-288">![オーディオ コントロールを実行している](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "オーディオ コントロールの実行")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-288">![Running the audio control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Running the audio control")</span></span>

    <span data-ttu-id="8f6e2-289">*オーディオ コントロールの実行*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-289">*Running the audio control*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f6e2-290">ビデオ、図など、Visual Studio に含まれている複数のスニペットを試すこともできます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-290">You can also try more snippets included in Visual Studio, such as video, figure, etc.</span></span>
5. <span data-ttu-id="8f6e2-291">ここで、コントロールを挿入するページの一部で再試行してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-291">Now, try to insert a control in some part of the page.</span></span> <span data-ttu-id="8f6e2-292">挿入しようとするなど、 **GridView**コントロールが入力する代わりに **&lt;Gri、**の入力を開始 **&lt;GV**。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-292">For example, try to insert a **GridView** control, but instead of typing **&lt;Gri,** start typing **&lt;GV**.</span></span> <span data-ttu-id="8f6e2-293">IntelliSense の一覧を示す通知、 **asp: GridView**コントロール。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-293">Notice that the IntelliSense list shows the **asp:GridView** control.</span></span>

    <span data-ttu-id="8f6e2-294">HTML エディターでの IntelliSense は、タイトル文字種の検索と部分一致する (要素を取得するすべての用語を含む) を提供します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-294">IntelliSense in the HTML Editor now provides title-casing search, as well as partial matching (retrieving all elements that contains the term).</span></span>

    <span data-ttu-id="8f6e2-295">![IntelliSense リストを GridView を挿入する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "IntelliSense リストを GridView の挿入")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-295">![Inserting a GridView with IntelliSense lists](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserting a GridView with IntelliSense lists")</span></span>

    <span data-ttu-id="8f6e2-296">*IntelliSense リストを GridView の挿入*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-296">*Inserting a GridView with IntelliSense lists*</span></span>

    <span data-ttu-id="8f6e2-297">入力した場合**&lt;グリッド**の語句に一致するすべてのアイテムが表示されますが、Visual Studio の候補が表示されます、 **gridview**コントロール。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-297">If you type **&lt;grid** you will get all the items that match the term, but Visual Studio will suggest the **gridview** control:</span></span>

    <span data-ttu-id="8f6e2-298">![IntelliSense リストおよび部分的な一致を GridView を挿入する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "IntelliSense リストおよび部分的な一致を GridView の挿入")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-298">![Inserting a GridView with IntelliSense lists and partial matching](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserting a GridView with IntelliSense lists and partial matching")</span></span>

    <span data-ttu-id="8f6e2-299">*IntelliSense リストおよび部分的な一致を GridView の挿入*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-299">*Inserting a GridView with IntelliSense lists and partial matching*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a><span data-ttu-id="8f6e2-300">タスク 4 - スマート タグを HTML エディター</span><span class="sxs-lookup"><span data-stu-id="8f6e2-300">Task 4 - HTML Editor Smart Tags</span></span>

<span data-ttu-id="8f6e2-301">別の向上、HTML エディターでは、スマート タグの機能です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-301">Another improvement in the HTML Editor is the Smart Tags feature.</span></span> <span data-ttu-id="8f6e2-302">スマート タグしやすいように、コントロールごとに一般的なや反復的な開発タスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-302">Smart tags make it easy to perform common or repetitive development tasks on a per-control basis.</span></span> <span data-ttu-id="8f6e2-303">この機能は既にできた HTML デザイナーでは HTML エディターでありません。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-303">This feature was already available in the HTML Designer, but not in the HTML Editor.</span></span>

1. <span data-ttu-id="8f6e2-304">開いている**Site.Master**を検索し、 **asp: メニュー**要素。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-304">Open **Site.Master** and locate the **asp:Menu** element.</span></span> <span data-ttu-id="8f6e2-305">カーソルを置き、開始タグと要素 - の下部に表示される小さなグリフがクリックして [スマート タスク] メニューを開きますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-305">Place the cursor on the start tag and notice that the small glyph displayed at the bottom of the element - click it to open the smart tasks menu.</span></span> <span data-ttu-id="8f6e2-306">メニュー コントロールに関連するいくつかのタスクにすばやくアクセスがあることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-306">Notice that you have quick access to some tasks related to the Menu control.</span></span>

    <span data-ttu-id="8f6e2-307">![メニュー コントロールのタスクのスマート](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "メニュー コントロールのタスクのスマート")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-307">![Smart tasks for the Menu control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart tasks for the Menu control")</span></span>

    <span data-ttu-id="8f6e2-308">*メニュー コントロールのスマート タスク*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-308">*Smart tasks for the Menu control*</span></span>

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a><span data-ttu-id="8f6e2-309">タスク 5 - スマート インデント</span><span class="sxs-lookup"><span data-stu-id="8f6e2-309">Task 5 - Smart Indentation</span></span>

<span data-ttu-id="8f6e2-310">読み取り可能なコードを保持する入れ子になった要素をインデント html 形式でのベスト プラクティスの 1 つです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-310">One of the best practices in HTML is indenting the nested elements to keep the code readable.</span></span> <span data-ttu-id="8f6e2-311">Visual Studio 2012 をエディターに自動的にインデントを設定、要素、コードを記述するときにわかります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-311">In Visual Studio 2012, you will notice that the editor automatically indents the elements while you are writing the code.</span></span>

> [!NOTE]
> <span data-ttu-id="8f6e2-312">Visual Studio の以前のバージョンではスマート インデントが使用可能な HTML エディターではなく、XML エディターにします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-312">In previous version of Visual Studio, smart indentation was available in the XML editor but not in the HTML editor.</span></span>


1. <span data-ttu-id="8f6e2-313">HTML エディターでインデント構成がスマート インデントに設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-313">Make sure that the Indenting configuration on the HTML Editor is set to Smart Indentation.</span></span> <span data-ttu-id="8f6e2-314">実行するには、選択、**ツール |オプション**メニュー オプションを選択し、**テキスト エディター |HTML |タブ**画面の左側のウィンドウ内のページです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-314">To do that, select the **Tools | Options** menu option and then select the **Text Editor | HTML | Tabs** page in the left pane of the screen.</span></span> <span data-ttu-id="8f6e2-315">スマート インデント オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-315">Select the Smart indentation option.</span></span>

    <span data-ttu-id="8f6e2-316">![HTML エディターの設定](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML エディターの設定")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-316">![HTML Editor settings](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML Editor settings")</span></span>

    <span data-ttu-id="8f6e2-317">*HTML エディターの設定*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-317">*HTML Editor settings*</span></span>
2. <span data-ttu-id="8f6e2-318">**Default.aspx**  ページで、オーディオの要素の下のすべてのコンテンツを削除します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-318">On the **Default.aspx** page, remove all the content under the audio element.</span></span>
3. <span data-ttu-id="8f6e2-319">開始の末尾にカーソルを置き**オーディオ**要素とヒット**ENTER**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-319">Place the cursor at the end of the opening **audio** element and hit **ENTER**.</span></span>

    <span data-ttu-id="8f6e2-320">新しいカーソルの位置が、追加のインデント レベルを持つことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-320">Notice that the new position of cursor has an additional indentation level.</span></span>

    <span data-ttu-id="8f6e2-321">![スマート インデント、HTML エディターで](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "スマート インデント、HTML エディター")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-321">![Smart indentation in the HTML Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart indentation in the HTML Editor")</span></span>

    <span data-ttu-id="8f6e2-322">*スマート インデント、HTML エディター*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-322">*Smart indentation in the HTML Editor*</span></span>
4. <span data-ttu-id="8f6e2-323">内容を削除するか、閉じるでオーディオのタグを復元**Default.aspx**なし、変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-323">Restore the audio tag with the content you have removed, or close **Default.aspx** without saving the changes.</span></span>

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a><span data-ttu-id="8f6e2-324">タスク 6: ユーザー コントロールに抽出</span><span class="sxs-lookup"><span data-stu-id="8f6e2-324">Task 6 - Extract to User Control</span></span>

<span data-ttu-id="8f6e2-325">関数の場合、コードの一部を抽出するなど、Visual Studio に含まれるリファクタリング ツールは、優れた機能を改善し、既存のコードのリファクタリングを容易にします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-325">The Refactoring tools included in Visual Studio, such as extracting a portion of code to a function, are great features that facilitate the improvement and the refactoring the existing code.</span></span> <span data-ttu-id="8f6e2-326">ASP.NET ページの対応するユーザー コントロールへの HTML コードの抽出になります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-326">The counterpart for ASP.NET pages would be the extraction of HTML code to a User Control.</span></span> <span data-ttu-id="8f6e2-327">手動で行うと、新しいユーザー コントロールを作成する、コード セクションをユーザー コントロールに移動、ユーザー コントロールのタグ プレフィックスを登録および、最後に、ページ上のユーザー コントロールをインスタンス化するように、いくつかの手順が行われます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-327">Doing it manually would involve several steps, like creating a new User Control, moving the code section to the User Control, registering a tag prefix for the User Control, and, finally, instantiating the User Control on the pages.</span></span> <span data-ttu-id="8f6e2-328">ここで、新しい*ユーザー コントロールに抽出*ツールが自動的にこれらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-328">Now, the new *Extract to User Control* tool automatically performs all those steps for you.</span></span>

<span data-ttu-id="8f6e2-329">このタスクでは、選択したコードから新しいユーザー コントロールを生成するのにユーザー コントロールのコンテキストの操作に新しい展開を使用します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-329">In this task, you will use the new Extract to User Control contextual operation to generate a new user control from the selected code.</span></span>

1. <span data-ttu-id="8f6e2-330">**Default.aspx** ] ページで、[、 **H2**と**オーディオ**要素。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-330">On the **Default.aspx** page, select the **H2** and **audio** elements.</span></span>
2. <span data-ttu-id="8f6e2-331">右クリックし、**ユーザー コントロールに抽出**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-331">Right click and select **Extract to User Control**.</span></span>

    <span data-ttu-id="8f6e2-332">![ユーザー コントロールのメニュー オプションに抽出](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "ユーザー コントロールのメニュー オプションに抽出")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-332">![Extract to User Control menu option](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Extract to User Control menu option")</span></span>

    <span data-ttu-id="8f6e2-333">*ユーザー コントロールのメニュー オプションに抽出します。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-333">*Extract to User Control menu option*</span></span>
3. <span data-ttu-id="8f6e2-334">新しいユーザー コントロールの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-334">Type a name for the new user control.</span></span> <span data-ttu-id="8f6e2-335">たとえば、 **Jukebox.ascx**、クリックして**[ok]**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-335">For instance, **Jukebox.ascx**, and then click **OK**.</span></span>

    <span data-ttu-id="8f6e2-336">![展開されたユーザー コントロールを保存](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "展開されたユーザー コントロールの保存")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-336">![Saving the extracted user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Saving the extracted user control")</span></span>

    <span data-ttu-id="8f6e2-337">*展開されたユーザー コントロールの保存*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-337">*Saving the extracted user control*</span></span>
4. <span data-ttu-id="8f6e2-338">ユーザー コントロールに抽出された、選択したコードと、選択したコードの元の場所は、新しいユーザー コントロールのインスタンスに置き換えられましたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-338">Notice that the selected code was extracted to a user control and the original location of the selected code was replaced with an instance of the new user control.</span></span>

    <span data-ttu-id="8f6e2-339">![ページが自動的に新しいユーザー コントロールを使用して更新](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "ページは、新しいユーザー コントロールを使用して自動的に更新")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-339">![Page automatically updated to use the new user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page automatically updated to use the new user control")</span></span>

    <span data-ttu-id="8f6e2-340">*ページは、新しいユーザー コントロールを使用して自動的に更新*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-340">*Page automatically updated to use the new user control*</span></span>
5. <span data-ttu-id="8f6e2-341">キーを押して**f5 キーを押して**ページを実行し、コントロールが動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-341">Press **F5** to run the page and verify that the control works.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a><span data-ttu-id="8f6e2-342">手順 3: は、JavaScript エディターの新機能</span><span class="sxs-lookup"><span data-stu-id="8f6e2-342">Exercise 3: What's New in the JavaScript Editor</span></span>

<span data-ttu-id="8f6e2-343">作成や JavaScript コードを編集が簡単な作業では、特に開始されたときに、アプリケーション サイズを大きく気づいたら長いファイルを処理する場合や数百ある関数</span><span class="sxs-lookup"><span data-stu-id="8f6e2-343">Writing or editing JavaScript code is not an easy task, especially when your application starts to grow in size and you find yourself dealing with long files and hundreds of functions.</span></span> <span data-ttu-id="8f6e2-344">スクリプトの開発者は、通常、コードの読みやすさを維持し、ファイル間で移動する特別な作業を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-344">Script developers usually have to do some extra work to maintain code legibility and navigate across files.</span></span> <span data-ttu-id="8f6e2-345">スクリプトのナビゲーションで jQuery と同様に JavaScript ライブラリの長いので、コード自体課題となっています。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-345">With the inclusion of JavaScript libraries like jQuery, script navigation has become a challenge itself because of the code length.</span></span>

<span data-ttu-id="8f6e2-346">Visual Studio には、コード モードにアクセスして整理する約束を JavaScript エディターが更新されます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-346">Visual Studio has renewed the JavaScript editor with the promise to make the code mode accessible and organized.</span></span> <span data-ttu-id="8f6e2-347">JavaScript エディターで c# または VB のエディターで既に存在していた Visual Studio の多くの機能が実装されるようになりました。 定義へ移動、自動インデント、ドキュメント、および記述するときに検証します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-347">Many Visual Studio features that already existed in C# or VB editors are now implemented in the JavaScript editor: Go To Definition, automatic indentation, documentation and validation when you are writing.</span></span> <span data-ttu-id="8f6e2-348">書き換えられた IntelliSense リストで、JavaScript 関数のカタログをすぐに利用できるがあります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-348">With the renewed IntelliSense list you will have the JavaScript function catalog at your fingertips.</span></span>

<span data-ttu-id="8f6e2-349">この演習では、JavaScript エディターの機能強化および新機能の一部を学習します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-349">In this exercise, you will learn some of the new features and improvements of JavaScript editor.</span></span> <span data-ttu-id="8f6e2-350">サンプル ファイルを参照し、検出できるは、JavaScript プログラミング Visual Studio 2012 内でより効率的な新しい特徴の各します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-350">You will browse sample files and discover each of the new characteristics that will make your JavaScript programming more efficient within Visual Studio 2012.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a><span data-ttu-id="8f6e2-351">タスク 1 - JavaScript エディターの新機能</span><span class="sxs-lookup"><span data-stu-id="8f6e2-351">Task 1 - JavaScript Editor New Features</span></span>

<span data-ttu-id="8f6e2-352">このタスクでは、コードを整理し、ユーザー エクスペリエンスの改善に集中新しい JavaScript エディター機能の一部を説明します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-352">This task will introduce you to some of the new JavaScript editor features, which focus on organizing your code and bringing a better user experience.</span></span>

1. <span data-ttu-id="8f6e2-353">開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**にソリューションがある、 **Source\WhatsNewASPNET**このラボのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-353">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="8f6e2-354">キーを押して**f5 キーを押して**アプリケーションを実行するには、次のナビゲーション バーでの JavaScript リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-354">Press **F5** to run the application, then click the JavaScript link in the navigation bar.</span></span> <span data-ttu-id="8f6e2-355">ページを更新、いくつかの時刻と確認方法はカウンターがインクリメントします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-355">Refresh the page several times and check how the counter increments.</span></span>

    <span data-ttu-id="8f6e2-356">![ページ カウンター](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "ページ カウンター")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-356">![Page counter](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page counter")</span></span>

    <span data-ttu-id="8f6e2-357">*ページ カウンター*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-357">*Page counter*</span></span>
3. <span data-ttu-id="8f6e2-358">ブラウザーを終了し、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-358">Close the browser and go back to Visual Studio.</span></span>
4. <span data-ttu-id="8f6e2-359">開く、 **JavaScript.aspx**ページし、検索、 **&lt;スクリプト&gt;**ブロック (下図参照)。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-359">Open the **JavaScript.aspx** page and locate the **&lt;script&gt;** block (shown below).</span></span>

    <span data-ttu-id="8f6e2-360">次のコードを格納する HTML5 のローカル ストレージを使用して、 *pageLoadCount*ページが現在のユーザーがアクセスした回数を格納する変数。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-360">The following code uses HTML5 local storage to store a *pageLoadCount* variable that stores the number of times the page has been visited by the current user.</span></span> <span data-ttu-id="8f6e2-361">ローカル ストレージは、標準の HTML5 で導入されたクライアント側のキー値データベースです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-361">Local Storage is a client-side key-value database introduced with the HTML5 standard.</span></span> <span data-ttu-id="8f6e2-362">データは、ユーザーのブラウザー内のローカルのコンピューターに保存されます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-362">The data is saved on the local machine, inside the user's browser.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="8f6e2-363">DOCTYPE の次の手順に進む前に XHTML5 に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-363">Ensure the DOCTYPE is set to XHTML5 before proceeding with the next steps.</span></span>
5. <span data-ttu-id="8f6e2-364">コードを編集して、JavaScript 用の IntelliSense がローカル記憶域、およびその内部メソッドなどの HTML5 の機能が含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-364">Edit the code and notice that IntelliSense for JavaScript includes HTML5 features, like local storage, and their inner methods.</span></span>

    <span data-ttu-id="8f6e2-365">![JavaScript での HTML5 の JavaScript 機能が](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "JavaScript での HTML5 の JavaScript 機能")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-365">![HTML5 JavaScript features in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript features in JavaScript")</span></span>

    <span data-ttu-id="8f6e2-366">*JavaScript での HTML5 の JavaScript 機能*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-366">*HTML5 JavaScript features in JavaScript*</span></span>
6. <span data-ttu-id="8f6e2-367">任意の始め角かっこをクリックして (**{**)、スクリプトからのコードし、角かっこが強調表示されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-367">Click any opening bracket (**{**) from the scripting code and notice that the brackets are highlighted.</span></span>

    <span data-ttu-id="8f6e2-368">![角かっこが強調表示されている](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "角かっこが強調表示されます")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-368">![Brackets are highlighted](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Brackets are highlighted")</span></span>

    <span data-ttu-id="8f6e2-369">*角かっこが強調表示されます。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-369">*Brackets are highlighted*</span></span>
7. <span data-ttu-id="8f6e2-370">関数のコメントを解除**testAutoAlign()** (3 つの行を選択し、使用することができます**CTRL** + **K**です。**CTRL** + **U**) 関数のコード内にカーソルを検索します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-370">Uncomment the function **testAutoAlign()** (select the three lines and you can use **CTRL** + **K**; **CTRL** + **U**) and locate the cursor inside the function code.</span></span> <span data-ttu-id="8f6e2-371">2 番目の行の追加に enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-371">Press enter to append a second line.</span></span> <span data-ttu-id="8f6e2-372">コードは、今すぐ通知**揃え**と**自動インデント**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-372">Notice that the code is now **aligned** and **auto-indented**.</span></span>

    <span data-ttu-id="8f6e2-373">![JavaScript コードは自動配置](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript コードは自動配置")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-373">![JavaScript code is auto aligned](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript code is auto aligned")</span></span>

    <span data-ttu-id="8f6e2-374">*JavaScript コードは自動配置*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-374">*JavaScript code is auto aligned*</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a><span data-ttu-id="8f6e2-375">作業 2: JavaScript の検証</span><span class="sxs-lookup"><span data-stu-id="8f6e2-375">Task 2 - Validating JavaScript</span></span>

<span data-ttu-id="8f6e2-376">このタスクは、ECMAScript5 標準の新しい JavaScript の検証を検出します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-376">In this task, you will discover the new JavaScript validation for the ECMAScript5 standard.</span></span> <span data-ttu-id="8f6e2-377">この機能はサイトに展開する前にスクリプトの問題を防止しながら、準拠の JavaScript コードを記述するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-377">This feature will help you to write compliant JavaScript code, while preventing scripting issues before site deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="8f6e2-378">Visual Studio 2010 では、Visual Studio 2012 ECMAScript5 コンプライアンスを提供しますが、ECMAStript3 コンプライアンスが実装されています。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-378">Visual Studio 2010 implemented ECMAStript3 compliance, while Visual Studio 2012 provides ECMAScript5 compliance.</span></span>


1. <span data-ttu-id="8f6e2-379">開いている**ECMA5script5.js**の下にある、 **Scripts\custom**プロジェクト フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-379">Open **ECMA5script5.js** located under the **Scripts\custom** project folder.</span></span> <span data-ttu-id="8f6e2-380">ECMAScript5 標準の検証をテストします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-380">You will now test validation for ECMAScript5 standard.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    <span data-ttu-id="8f6e2-381">チェック アウトすることができます、 &quot; **を使用して厳密な** &quot; ECMAScript5 を使用すると、ファイルの最初の行に方向**厳格モード**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-381">You can check out the &quot; **use strict** &quot; direction in the first line of the file, which enables ECMAScript5 **strict mode**.</span></span> <span data-ttu-id="8f6e2-382">このモードは、過去のエディションでは、あいまいさを明確化し、オブジェクトのプロパティの get アクセス操作子および set アクセス操作子、JSON、およびより完全なリフレクションのライブラリのサポートなど、一部の新機能を追加する言語のサブセットで構成されます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-382">This mode consists in a subset of the language that clarifies ambiguities from the past edition, and adds some new features, such as getters and setters, library support for JSON, and more complete reflection on object properties.</span></span>
2. <span data-ttu-id="8f6e2-383">開く、**エラー一覧**開いていない場合 (**ビュー**メニュー |**エラー一覧**)。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-383">Open the **Error List** if not already opened (**View** menu | **Error List**).</span></span> <span data-ttu-id="8f6e2-384">通知、**関数**宣言の下線が付けられます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-384">Notice the **function** declaration is underlined.</span></span> <span data-ttu-id="8f6e2-385">これは、言語構造内にネストできません標準関数 ECMA5 であるためです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-385">This is because in ECMA5 standard functions cannot be nested inside language structures.</span></span> <span data-ttu-id="8f6e2-386">エラーが発生する次の一覧は、警告の詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-386">In the error list below you will see the warning details.</span></span>

    <span data-ttu-id="8f6e2-387">![JavaScript の検証エラー メッセージ](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript の検証エラー メッセージ")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-387">![JavaScript validation error message](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript validation error message")</span></span>

    <span data-ttu-id="8f6e2-388">*JavaScript の検証エラー メッセージ*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-388">*JavaScript validation error message*</span></span>
3. <span data-ttu-id="8f6e2-389">コメント アウト、 **&quot;を使用して厳密な&quot;**方向とエラーが非表示になりますが、警告まま残っていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-389">Comment out the **&quot;use strict&quot;** direction and notice that errors disappear, but the warnings remain.</span></span>
4. <span data-ttu-id="8f6e2-390">ファイルの最後の行でのような任意の文字列に書き込む**&quot;テスト&quot;** (引用符は文字列であることを示すを含む)。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-390">In the last line of the file, write any string like **&quot;test&quot;** (include the quotation marks to indicate it is as string).</span></span> <span data-ttu-id="8f6e2-391">IntelliSense の一覧を表示し、選択する文字列の横にあるピリオドを書き込み、**トリム**オプション。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-391">Write a period next to the string to display the IntelliSense list, and select the **trim** option.</span></span>

    <span data-ttu-id="8f6e2-392">ECMAScript5 標準で文字列値および変数も文字列の方法があります定義、トリミング、大文字、検索し、置換と同じようにします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-392">In ECMAScript5 standard, string values and variables also have string methods defined, like trim, uppercase, search and replace.</span></span>

    <span data-ttu-id="8f6e2-393">![JavaScript での IntelliSense リスト](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "javascript IntelliSense の一覧")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-393">![IntelliSense list in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense list in JavaScript")</span></span>

    <span data-ttu-id="8f6e2-394">*JavaScript での IntelliSense リスト*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-394">*IntelliSense list in JavaScript*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a><span data-ttu-id="8f6e2-395">タスク 3 - JavaScript 用の XML ドキュメント</span><span class="sxs-lookup"><span data-stu-id="8f6e2-395">Task 3 - XML Documentation for JavaScript</span></span>

<span data-ttu-id="8f6e2-396">このタスクでは、JavaScript での XML ドキュメントの Visual Studio の機能を調査します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-396">In this task, you will explore Visual Studio features for XML documentation in JavaScript.</span></span> <span data-ttu-id="8f6e2-397">JavaScript IntelliSense の一覧の各関数の XML ドキュメントの表示が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-397">You will see the JavaScript IntelliSense list now shows the XML documentation of each function.</span></span> <span data-ttu-id="8f6e2-398">さらに、JavaScript でナビゲーション機能は、ことがわかります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-398">Additionally, you will discover the navigation feature in JavaScript.</span></span>

1. <span data-ttu-id="8f6e2-399">開いている**XMLDoc.js**にあるファイル**スクリプトまたはカスタム**プロジェクト フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-399">Open **XMLDoc.js** file located in **Scripts/custom** project folder.</span></span> <span data-ttu-id="8f6e2-400">このファイルには、JavaScript 関数の各 XML ドキュメントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-400">This file contains XML documentation on each of the JavaScript functions.</span></span>

    <span data-ttu-id="8f6e2-401">![JavaScript XML ドキュメントが IntelliSense に統合された](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML ドキュメントが IntelliSense に統合")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-401">![JavaScript XML documentation integrated to IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentation integrated to IntelliSense")</span></span>

    <span data-ttu-id="8f6e2-402">*IntelliSense に統合された JavaScript XML ドキュメント*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-402">*JavaScript XML documentation integrated to IntelliSense*</span></span>
2. <span data-ttu-id="8f6e2-403">下**追加**関数**XMLDoc.js**ファイル、という名前の新しい関数を作成する**テスト**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-403">Below **add** function in **XMLDoc.js** file, create a new function named **test**.</span></span>
3. <span data-ttu-id="8f6e2-404">**テスト**関数を呼び出して、**乗算**2 つのパラメーターを受け取る関数。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-404">In the **test** function, call the **multiply** function that receives two parameters.</span></span> <span data-ttu-id="8f6e2-405">ツールヒント ボックスが表示されていることを確認、**乗算**ドキュメントに機能します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-405">Notice the tooltip box is showing the **multiply** function documentation.</span></span>

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    <span data-ttu-id="8f6e2-406">![JavaScript 関数の XML ドキュメント](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "JavaScript 関数の XML ドキュメント")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-406">![XML documentation for JavaScript functions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML documentation for JavaScript functions")</span></span>

    <span data-ttu-id="8f6e2-407">*JavaScript 関数の XML ドキュメント*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-407">*XML documentation for JavaScript functions*</span></span>
4. <span data-ttu-id="8f6e2-408">関数呼び出しステートメントと種類を完了する、*ドット*を開くには、返される値の IntelliSense の一覧です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-408">Complete the function call statement and type a *dot* to open the IntelliSense list on the returned value.</span></span> <span data-ttu-id="8f6e2-409">Visual Studio が検出することを確認、**返す**ドキュメントには、数値として値を扱う方法の値。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-409">Notice that Visual Studio is detecting the **return** value in the documentation, treating the value as a number.</span></span>

    <span data-ttu-id="8f6e2-410">![戻り値の型の XML ドキュメント](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "戻り値の型の XML ドキュメント")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-410">![XML documentation for return types](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML documentation for return types")</span></span>

    <span data-ttu-id="8f6e2-411">*戻り値の型の XML ドキュメント*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-411">*XML documentation for return types*</span></span>
5. <span data-ttu-id="8f6e2-412">関数を追加するための呼び出しを挿入します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-412">Now, insert a call to add function.</span></span> <span data-ttu-id="8f6e2-413">JavaScript エディターが今すぐ関数のオーバー ロードをサポートしていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-413">Notice that the JavaScript editor now supports function overloads.</span></span> <span data-ttu-id="8f6e2-414">関数名を記述するときに、ドキュメントで指定された使用可能なオーバー ロードのいずれかを選択することができます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-414">When you write a function name, you will be able to select any of the available overloads specified in the documentation.</span></span>

    <span data-ttu-id="8f6e2-415">![オーバー ロードでは XML ドキュメント](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "オーバー ロードでは XML ドキュメント")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-415">![XML documentation for overloads](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML documentation for overloads")</span></span>

    <span data-ttu-id="8f6e2-416">*オーバー ロードでは XML ドキュメント*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-416">*XML documentation for overloads*</span></span>
6. <span data-ttu-id="8f6e2-417">開いている**GotoDefinition.js**ファイルし、検索、 **$().html()**関数呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-417">Open **GotoDefinition.js** file and locate the **$().html()** function call.</span></span> <span data-ttu-id="8f6e2-418">カーソルを置いて**html**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-418">Locate the cursor on **html**.</span></span>
7. <span data-ttu-id="8f6e2-419">キーを押して**F12**定義に移動します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-419">Press **F12** and navigate to the definition.</span></span> <span data-ttu-id="8f6e2-420">今すぐアクセスしを使用せず、JavaScript コードを参照することができますに注意してください、**検索**ツールです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-420">Notice you can now access and browse your JavaScript code without using the **Find** tool.</span></span>
8. <span data-ttu-id="8f6e2-421">コード ファイルの下部にある署名ブロックの前に、jQuery 命令にカーソルを配置します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-421">Locate the cursor on the jQuery instruction prior to the signature block at the bottom of the code file.</span></span> <span data-ttu-id="8f6e2-422">キーを押して**F12**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-422">Press **F12**.</span></span> <span data-ttu-id="8f6e2-423">JQuery ライブラリ ファイルに移動します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-423">You will navigate to the jQuery library file.</span></span> <span data-ttu-id="8f6e2-424">使用して jQuery ファイルを移動することもわかります**F12**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-424">Notice you can also navigate across the jQuery files using **F12**.</span></span>

    <span data-ttu-id="8f6e2-425">![JQuery の定義に移動する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "jQuery 定義に移動します。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-425">![Navigating to jQuery definitions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigating to jQuery definitions")</span></span>

    <span data-ttu-id="8f6e2-426">*JQuery の定義に移動します。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-426">*Navigating to jQuery definitions*</span></span>

> [!NOTE]
> <span data-ttu-id="8f6e2-427">GotoDefinition.js は、ファイルを保存する前に構文エラーがないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-427">Make sure that GotoDefinition.js has no syntax errors before saving the file.</span></span>


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a><span data-ttu-id="8f6e2-428">手順 4: バンドルと縮小</span><span class="sxs-lookup"><span data-stu-id="8f6e2-428">Exercise 4: Bundling and Minification</span></span>

<span data-ttu-id="8f6e2-429">何回かは、web サイトを含める JavaScript や CSS の 1 つ以上のファイルですか。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-429">How many times do your websites include more than one JavaScript or CSS file?</span></span> <span data-ttu-id="8f6e2-430">これは、そこバンドルと縮小で役立つファイル サイズを小さくし、高速に実行するサイトを非常に一般的なシナリオです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-430">This is a very common scenario where bundling and minification can help to reduce the file size and make the site perform faster.</span></span> <span data-ttu-id="8f6e2-431">ASP.NET 4.5 の新機能でバンドルは、JS または CSS ファイルのセットを 1 つの要素にパックされ、縮小する (必須ではない空白を削除する、コメントを削除する、識別子を減らすこと) のコンテンツのサイズを軽減します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-431">The new bundling feature in ASP.NET 4.5 packs a set of JS or CSS files into a single element, and reduces its size by minifying the content ( i.e. removing not required blank spaces, removing comments, reducing identifiers ).</span></span>

<span data-ttu-id="8f6e2-432">バンドルと縮小 ASP.NET 4.5 では実行時に、プロセスを (たとえば IE、Mozilla など)、ユーザー エージェントを識別するし、そのため、(たとえば、削除 stuff Mozilla 固有であるユーザーのブラウザーを対象として、圧縮が向上するためときに、要求が送られた IE から)。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-432">Bundling and minification in ASP.NET 4.5 is performed at runtime, so that the process can identify the user agent (for example IE, Mozilla, etc), and thus, improve the compression by targeting the user browser (for instance, removing stuff that is Mozilla specific when the request comes from IE).</span></span>

<span data-ttu-id="8f6e2-433">この演習では、有効にして ASP.NET 4.5 でのバンドルと縮小のさまざまな種類の使用方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-433">In this exercise, you will learn how to enable and use the different types of bundling and minification in ASP.NET 4.5.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a><span data-ttu-id="8f6e2-434">タスク 1 - バンドルのインストールと NuGet パッケージの縮小</span><span class="sxs-lookup"><span data-stu-id="8f6e2-434">Task 1 - Installing the Bundling and Minification Package from NuGet</span></span>

1. <span data-ttu-id="8f6e2-435">開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**にソリューションがある、 **Source\WhatsNewASPNET**このラボのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-435">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="8f6e2-436">NuGet パッケージ マネージャー コンソールを開きます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-436">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="8f6e2-437">これを行うには、メニューを使用して**ビュー** | **その他のウィンドウ** | **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-437">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>

    <span data-ttu-id="8f6e2-438">![パッケージ マネージャー file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole を開く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "パッケージ マネージャー コンソールを開く")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-438">![Opening the package manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Opening the package manager console")</span></span>

    <span data-ttu-id="8f6e2-439">*パッケージ マネージャー コンソールを開く*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-439">*Opening the package manager console*</span></span>
3. <span data-ttu-id="8f6e2-440">**Package Manager Console**型**Install-package Microsoft.Web.Optimization**とキーを押します**ENTER**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-440">In the **Package Manager Console,** type **Install-Package Microsoft.Web.Optimization** and press **ENTER**.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a><span data-ttu-id="8f6e2-441">タスク 2 - 既定のバンドル</span><span class="sxs-lookup"><span data-stu-id="8f6e2-441">Task 2 - Default Bundles</span></span>

<span data-ttu-id="8f6e2-442">バンドルと縮小を使用する最も簡単な方法は、既定のバンドルを有効にするのにです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-442">The simplest way to use bundling and minification is to enable the default bundles.</span></span> <span data-ttu-id="8f6e2-443">このメソッドは、規則を使用して、フォルダー内の JS および CSS ファイルのバンドルと縮小されたバージョンを参照できます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-443">This method uses conventions to let you reference the bundled and minified version for the JS and CSS files in a folder.</span></span>

<span data-ttu-id="8f6e2-444">このタスクでは、有効にしてバンドルと縮小された JS および CSS ファイルを参照し、結果の出力を表示する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-444">In this task, you will learn how to enable and reference the bundled and minified JS and CSS files and view the resulting output.</span></span>

1. <span data-ttu-id="8f6e2-445">開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**にソリューションがある、 **Source\WhatsNewASPNET**このラボのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-445">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="8f6e2-446">**ソリューション エクスプ ローラー**、展開、**スタイル**、 **Scripts\custom**と**Scripts\bundle**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-446">In the **Solution Explorer**, expand the **Styles**, **Scripts\custom** and **Scripts\bundle** folders.</span></span>

    <span data-ttu-id="8f6e2-447">アプリケーションが 1 つ以上の CSS および JS ファイルを使用することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-447">Notice that the application is using more than one CSS and JS file.</span></span>

    <span data-ttu-id="8f6e2-448">![アプリケーション内の複数のスタイル シートと JavaScript ファイル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "アプリケーション内の複数のスタイル シートと JavaScript ファイル")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-448">![Multiple Stylesheets and JavaScript files in the application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Multiple Stylesheets and JavaScript files in the application")</span></span>

    <span data-ttu-id="8f6e2-449">*アプリケーションで複数のスタイル シートおよび JavaScript ファイル*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-449">*Multiple Stylesheets and JavaScript files in the application*</span></span>
3. <span data-ttu-id="8f6e2-450">開く、 **Global.asax.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-450">Open the **Global.asax.cs** file.</span></span>

    <span data-ttu-id="8f6e2-451">注意して、新しい**Microsoft.Web.Optimization**名前空間は、ファイルの先頭にコメント アウトされています。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-451">Notice that the new **Microsoft.Web.Optimization** namespace is commented out at the beginning of the file.</span></span> <span data-ttu-id="8f6e2-452">使用して、コメントを解除バンドルと縮小の機能をインクルードします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-452">Uncomment the using directive to include the bundling and minification features.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. <span data-ttu-id="8f6e2-453">検索、**アプリケーション\_開始**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-453">Locate the **Application\_Start** method.</span></span>

    <span data-ttu-id="8f6e2-454">次のスニペットに示すようには、このメソッドで EnableDefaultBundles 呼び出しをコメント解除します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-454">In this method, uncomment the EnableDefaultBundles call as shown in the snippet below.</span></span> <span data-ttu-id="8f6e2-455">これにより、そのフォルダーへのパスを使用して、フォルダー内の CSS ファイルのバンドル コレクションを参照すると、 &quot;CSS&quot;または&quot;JS&quot;サフィックス。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-455">This enables us to reference a bundled collection of CSS files in a folder by using the path to that folder, plus the &quot;CSS&quot; or the &quot;JS&quot; suffix.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. <span data-ttu-id="8f6e2-456">開く、 **Optimization.aspx**ファイルし、のコンテンツ コントロールを探して**HeadContent**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-456">Open the **Optimization.aspx** file and locate the content control for **HeadContent**.</span></span>

    <span data-ttu-id="8f6e2-457">CSS ファイルと参照されているタグが 1 つ JS ファイルに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-457">Notice the CSS files and the JS files have a single referenced tag.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > <span data-ttu-id="8f6e2-458">このコードは、デモの目的でです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-458">This code is for demo purposes.</span></span> <span data-ttu-id="8f6e2-459">理想的には、Site.Master ファイル内のバンドルを参照します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-459">Ideally, you will reference the bundles in the Site.Master file.</span></span> <span data-ttu-id="8f6e2-460">このサンプル コードで表示されます、バンドルされているファイルの一部もによって参照されている Site.Master ファイル冗長この最後の参照を作成します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-460">In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.</span></span>
6. <span data-ttu-id="8f6e2-461">リンクが内のバンドルの規則を使用していることを確認、 **href**スタイルと Scripts\custom からすべての CSS または JS ファイルを取得する属性フォルダーそれぞれします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-461">Notice that the links are using the bundling conventions in the **href** attribute to get all the CSS or JS files from the Styles and Scripts\custom folder respectively.</span></span>

    <span data-ttu-id="8f6e2-462">パスを使用することができます**スクリプト/カスタム/JS**をバンドルして、内のすべての JS ファイルを縮小する次のように、**スクリプトまたはカスタム**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-462">You can use the path **Scripts/custom/JS** as shown below to bundle and minify all the JS files inside a **Scripts/custom** folder.</span></span> <span data-ttu-id="8f6e2-463">これは、既定のバンドルで既定の動作です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-463">This is the default behavior with the default bundles.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. <span data-ttu-id="8f6e2-464">開く、 **Styles\Site.css**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-464">Open the **Styles\Site.css** file.</span></span>

    <span data-ttu-id="8f6e2-465">元の CSS ファイルには、インデントされたコード、空白文字と、ファイルを大きくコメントが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-465">Notice that the original CSS file contains indented code, blank spaces and comments that enlarge the file.</span></span> <span data-ttu-id="8f6e2-466">(また、JavaScript ファイルが含まれていますの空白文字とコメント)。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-466">(Also the JavaScript file contains blank spaces and comments).</span></span>

    <span data-ttu-id="8f6e2-467">![スクリプト フォルダーにファイルを元の CSS のいずれかの](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "スクリプト フォルダーにファイルを元の CSS のいずれか")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-467">![One of the original CSS files in the Scripts folder](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "One of the original CSS files in the Scripts folder")</span></span>

    <span data-ttu-id="8f6e2-468">*Scripts フォルダー内の元の CSS ファイルのいずれか*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-468">*One of the original CSS files in the Scripts folder*</span></span>
8. <span data-ttu-id="8f6e2-469">キーを押して**f5 キーを押して**アプリケーションを実行しに移動する、**最適化**ページ。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-469">Press **F5** to run the application and navigate to the **Optimization** page.</span></span>
9. <span data-ttu-id="8f6e2-470">をクリックして、 **CSS バンドル**のリンクをダウンロードしてファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-470">Click on the **CSS Bundle** link to download and open the file.</span></span>

    <span data-ttu-id="8f6e2-471">縮小されたバンドルされているファイルをチェック アウトします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-471">Check out the minified bundled file.</span></span> <span data-ttu-id="8f6e2-472">わかりますすべての空白文字、コメントとインデント文字が削除されているより小さなファイルを生成します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-472">You will notice that all the blank spaces, comments and indentation characters have been removed, generating a smaller file.</span></span>

    <span data-ttu-id="8f6e2-473">![CSS ファイルをバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "バンドルされた CSS ファイル")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-473">![Bundled CSS files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Bundled CSS files")</span></span>

    <span data-ttu-id="8f6e2-474">*CSS、バンドルされるファイル*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-474">*Bundled CSS files*</span></span>
10. <span data-ttu-id="8f6e2-475">クリックし、 **JS バンドル**バンドルされている JavaScript ファイルを開くためのリンク。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-475">Now click the **JS Bundle** link to open the JavaScript bundled file.</span></span> <span data-ttu-id="8f6e2-476">警告エクスプ ローラーを安全に無視することができます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-476">You can safely disregard the explorer warning.</span></span> <span data-ttu-id="8f6e2-477">JavaScript ファイルに注意してください、**カスタム**フォルダーもバンドルし、縮小します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-477">Notice the JavaScript files under the **custom** folder are also bundled and minified.</span></span>

    <span data-ttu-id="8f6e2-478">![JavaScript ファイルをバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "バンドルされている JavaScript ファイル")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-478">![Bundled JavaScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Bundled JavaScript files")</span></span>

    <span data-ttu-id="8f6e2-479">*バンドルされている JavaScript ファイル*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-479">*Bundled JavaScript files*</span></span>

    <span data-ttu-id="8f6e2-480">ASP.NET の以前のバージョンでは、はるかに複雑なはしました CSS または JS ファイルの圧縮を有効にします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-480">Enabling compression for CSS or JS files was much more complicated in previous ASP.NET version.</span></span> <span data-ttu-id="8f6e2-481">ここで、説明したようするだけで 1 つの行を追加、 *Global.asax*ファイルをバンドルするには、有効にして、サイトから、バンドルされているファイルを参照します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-481">Now, as you have seen, you just need to add one line in the *Global.asax* file to enable bundling, and then reference the bundled files from your site.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a><span data-ttu-id="8f6e2-482">タスク 3 - 静的バンドル</span><span class="sxs-lookup"><span data-stu-id="8f6e2-482">Task 3 - Static Bundles</span></span>

<span data-ttu-id="8f6e2-483">静的バンドル アプローチでは、バンドル、参照、および使用される縮小メソッドへのファイルのセットをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-483">The static bundle approach allows you to customize the set of files to bundle, the reference and the minification method that will be used.</span></span>

<span data-ttu-id="8f6e2-484">このタスクでは、ファイルをバンドルして縮小の特定のセットを定義する静的バンドルを構成します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-484">In this task, you will configure a static bundle to define a specific set of files to bundle and minify.</span></span>

1. <span data-ttu-id="8f6e2-485">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-485">Close the browser.</span></span>
2. <span data-ttu-id="8f6e2-486">開く、 **Global.asax.cs**ファイルし、検索、**アプリケーション\_開始**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-486">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
3. <span data-ttu-id="8f6e2-487">次のコードに示すように、静的なバンドル コードのコメントを解除します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-487">Uncomment the static bundle code as shown in the code below.</span></span>

    <span data-ttu-id="8f6e2-488">参照される静的バンドルを定義する、 &quot; **~/StaticBundle** &quot;仮想パスと使用**JsMinify**用に指定されたすべてのファイルのサイズを縮小します**AddFile**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-488">You are defining a static bundle that will be referenced with the &quot;**~/StaticBundle**&quot; virtual path and use **JsMinify** for minification of all the specified files with the **AddFile** method.</span></span> <span data-ttu-id="8f6e2-489">最後に、静的のバンドルを追加する、 **BundleTable**有効にするとします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-489">Finally, you are adding the static bundle to the **BundleTable** and enabling it.</span></span>

    <span data-ttu-id="8f6e2-490">ファイルが同じ位置に固定; に存在しないことに注意してください。これは、既定のバンドル経由のもう 1 つの利点です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-490">Notice that the files are not located in the same place; this is another advantage over the default bundling.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. <span data-ttu-id="8f6e2-491">開く、 **Optimization.aspx**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-491">Open the **Optimization.aspx** file.</span></span>

    <span data-ttu-id="8f6e2-492">注意してへのリンク**静的 JS バンドル**Global.asax.cs ファイルで、静的なバンドルの構成時に宣言されているパスを使用して: **/StaticBundle**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-492">Notice that the link to **Static JS Bundle** is using the path you have declared when you configured the static bundle in the Global.asax.cs file: **/StaticBundle**.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. <span data-ttu-id="8f6e2-493">キーを押して**f5 キーを押して**アプリケーションを実行しに移動する、**最適化**ページ。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-493">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
6. <span data-ttu-id="8f6e2-494">をクリックして、**静的 JS バンドル**ファイルを開くためのリンク。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-494">Click on the **Static JS Bundle** link to open the file.</span></span>

    <span data-ttu-id="8f6e2-495">静的バンドル ファイル パスの下で構成されているすべての JavaScript ファイルの出力、縮小されたバンドルの JavaScript ファイル&quot;/StaticBundle&quot;です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-495">Notice that the minified bundled JavaScript file is the output for all the JavaScript files configured in the static bundle file under the path &quot;/StaticBundle&quot;.</span></span>

    <span data-ttu-id="8f6e2-496">![静的な JavaScript ファイルのバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "静的 JavaScript ファイルのバンドル")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-496">![Static JavaScript files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Static JavaScript files bundle")</span></span>

    <span data-ttu-id="8f6e2-497">*静的な JavaScript ファイルをバンドルします。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-497">*Static JavaScript files bundle*</span></span>
7. <span data-ttu-id="8f6e2-498">ブラウザーを閉じて、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-498">Close the browser and return to Visual Studio.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a><span data-ttu-id="8f6e2-499">タスク 4 - 動的フォルダー バンドル</span><span class="sxs-lookup"><span data-stu-id="8f6e2-499">Task 4 - Dynamic Folder Bundles</span></span>

<span data-ttu-id="8f6e2-500">このタスクでは、動的フォルダー バンドルを構成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-500">In this task, you will learn how to configure dynamic folder bundles.</span></span> <span data-ttu-id="8f6e2-501">動的バンドルの電源は、JavaScript にコンパイルされる言語で、静的な JavaScript だけでなく、その他のファイルを含めるし、できるためのバンドルを実行する前に何らかの処理を必要です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-501">The power of dynamic bundling is that you can include static JavaScript, as well as other files in languages that compiles into JavaScript, and thus, require some processing before the bundling is executed.</span></span>

<span data-ttu-id="8f6e2-502">この例では、使用する方法を学習します、 **DynamicFolderBundle** CofeeScript で記述されたファイルの動的なバンドルを作成するクラス。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-502">In this example, you will learn how to use the **DynamicFolderBundle** class to create a dynamic bundle for files written in CofeeScript.</span></span> <span data-ttu-id="8f6e2-503">CofeeScript は、JavaScript にコンパイルし、JavaScript コードを記述、JavaScript の簡潔さと読みやすさの向上の単純な構文を提供するプログラミング言語です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-503">CofeeScript is a programming language that compiles into JavaScript and provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability.</span></span>

1. <span data-ttu-id="8f6e2-504">開く、 **Global.asax.cs**ファイルし、検索、**アプリケーション\_開始**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-504">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
2. <span data-ttu-id="8f6e2-505">次のコードに示すように、動的なバンドル コードのコメントを解除します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-505">Uncomment the dynamic bundle code as shown in the code below.</span></span>

    <span data-ttu-id="8f6e2-506">使用する動的フォルダー バンドルを定義する、 **CoffeeMinify**を使用したファイルにのみ適用されるカスタムの縮小プロセッサ、 &quot; **.coffee** &quot;拡張機能 (CoffeeScript ファイル)。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-506">You are defining a dynamic folder bundle that will use the **CoffeeMinify** custom minification processor that will only apply to the files with the &quot;**.coffee**&quot; extension (CoffeeScript files).</span></span> <span data-ttu-id="8f6e2-507">同様に、フォルダー内にバンドルするファイルを選択し、検索パターンを使用することができますに注意してください '\*.coffee' です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-507">Notice that you can use a search pattern to select the files to bundle within a folder, like '\*.coffee'.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. <span data-ttu-id="8f6e2-508">NuGet パッケージ マネージャー コンソールを開きます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-508">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="8f6e2-509">これを行うには、メニューを使用して**ビュー** | **その他のウィンドウ** | **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-509">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>
4. <span data-ttu-id="8f6e2-510">**Package Manager Console**型**Install-package CoffeeSharp**とキーを押します**ENTER**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-510">In the **Package Manager Console,** type **Install-Package CoffeeSharp** and press **ENTER**.</span></span>
5. <span data-ttu-id="8f6e2-511">クリックして、 **すべてのファイル**ボタンをクリックして、**ソリューション エクスプ ローラー**ウィンドウ</span><span class="sxs-lookup"><span data-stu-id="8f6e2-511">Click the **Show All Files** button in the **Solution Explorer** window</span></span>

    <span data-ttu-id="8f6e2-512">![すべてのファイルを示す](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "すべてのファイルを表示")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-512">![Showing all files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Showing all files")</span></span>

    <span data-ttu-id="8f6e2-513">*すべてのファイルを表示*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-513">*Showing all files*</span></span>
6. <span data-ttu-id="8f6e2-514">右クリックして、 **CoffeeMinify.cs**ファイルで、**ソリューション エクスプ ローラー**選択**プロジェクトに含める**</span><span class="sxs-lookup"><span data-stu-id="8f6e2-514">Right click the **CoffeeMinify.cs** file in the **Solution Explorer** and select **Include in Project**</span></span>

    <span data-ttu-id="8f6e2-515">![CoffeeMinify.cs ファイルをプロジェクトに含める](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs ファイルをプロジェクトに含める")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-515">![Include the CoffeeMinify.cs file in the project](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Include the CoffeeMinify.cs file in the project")</span></span>

    <span data-ttu-id="8f6e2-516">*CoffeeMinify.cs ファイルをプロジェクトに含める*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-516">*Include the CoffeeMinify.cs file in the project*</span></span>
7. <span data-ttu-id="8f6e2-517">開く、 **CoffeeMinify.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-517">Open the **CoffeeMinify.cs** file.</span></span>

    <span data-ttu-id="8f6e2-518">このクラスは、JavaScript の出力 CoffeeScript コードのコンパイルから結果を縮小する JsMinify から継承します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-518">This class inherits from JsMinify to minify the JavaScript output resulting from the CoffeeScript code compilation.</span></span> <span data-ttu-id="8f6e2-519">最初に、JavaScript コードを生成する CoffeeScript コンパイラを呼び出すし、その、結果のコードを縮小する JsMinify.Process メソッドにします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-519">It calls the CoffeeScript compiler to generate the JavaScript code first, and then it sends it to the JsMinify.Process method to minify the resulting code.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. <span data-ttu-id="8f6e2-520">開く、 **Script1.coffee**と**Script2.coffee**ファイルから、**スクリプト/バンドル**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-520">Open the **Script1.coffee** and **Script2.coffee** files from the **Scripts/bundle** folder.</span></span>

    <span data-ttu-id="8f6e2-521">これらのファイルは、CoffeeMinify クラスとのバンドルを実行中にコンパイルする CoffeScript コードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-521">These files will include the CoffeScript code to be compiled while performing the bundling with the CoffeeMinify class.</span></span>

    <span data-ttu-id="8f6e2-522">わかりやすくするための目的で提供されている CoffeeScript ファイルは CoffeeScript サンプル コードのみを含みます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-522">For simplicity purposes, the CoffeeScript files provided are only including CoffeeScript sample code.</span></span> <span data-ttu-id="8f6e2-523">JsMinify プロセスでは、コメントは除外します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-523">The comments are excluded by the JsMinify process.</span></span>

    <span data-ttu-id="8f6e2-524">![CoffeeScript ファイル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript ファイル")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-524">![CoffeeScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript files")</span></span>

    <span data-ttu-id="8f6e2-525">*CoffeeScript ファイル*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-525">*CoffeeScript files*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f6e2-526">[CofeeScript](https://github.com/jashkenas/coffeescript/)の JavaScript コードを記述、JavaScript の簡潔さと読みやすさの向上だけでなく配列の理解とパターンに一致するようにその他の機能を追加する単純な構文を提供します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability, as well as adding other features like array comprehension and pattern matching.</span></span>
9. <span data-ttu-id="8f6e2-527">開く、 **Optimization.aspx**ファイルし、バンドルのリンクを検索します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-527">Open the **Optimization.aspx** file and locate the bundle links.</span></span>

    <span data-ttu-id="8f6e2-528">注意してへのリンク**動的 JS バンドル**を参照している、**スクリプト/バンドル**フォルダーを使用して、 **コーヒー/**動的フォルダー バンドルの構成されているサフィックス。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-528">Notice that the link to **Dynamic JS Bundle** is referencing the **Scripts/bundle** folder by using the **/Coffee** suffix you configured for the dynamic folder bundle.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. <span data-ttu-id="8f6e2-529">キーを押して**f5 キーを押して**アプリケーションを実行しに移動する、**最適化**ページ。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-529">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
11. <span data-ttu-id="8f6e2-530">をクリックして、**動的 JS バンドル**生成されたファイルを開くためのリンク。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-530">Click on the **Dynamic JS Bundle** link to open the generated file.</span></span>

    <span data-ttu-id="8f6e2-531">このバンドルに含まれているコンテンツのみを含む通知**.coffee**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-531">Notice that the content that was included in this bundle only contains **.coffee** files.</span></span> <span data-ttu-id="8f6e2-532">CoffeeScript コードが JavaScript にコンパイルされたこと、およびコメント アウトされた行が削除されてもわかります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-532">You can also see that the CoffeeScript code was compiled to JavaScript and the commented-out lines has been removed.</span></span>

    <span data-ttu-id="8f6e2-533">![動的な JS ファイルをバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "動的 JS ファイル バンドル")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-533">![Dynamic JS files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Dynamic JS files bundle")</span></span>

    <span data-ttu-id="8f6e2-534">*動的な JS ファイルをバンドルします。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-534">*Dynamic JS files bundle*</span></span>

> [!NOTE]
> <span data-ttu-id="8f6e2-535">Windows Azure Web サイトの次にこのアプリケーションを展開するさらに、[付録 b: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-535">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="8f6e2-536">概要</span><span class="sxs-lookup"><span data-stu-id="8f6e2-536">Summary</span></span>

<span data-ttu-id="8f6e2-537">このラボでは、ASP.NET で新機能と Visual Studio 2012 での Web 開発と Visual Studio 2012 で、さまざまな拡張機能の活用方法を理解するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-537">This lab helps you to understand what New in ASP.NET and Web Development in Visual Studio 2012 is and how to take advantage of the variety of enhancements in Visual Studio 2012.</span></span>

<span data-ttu-id="8f6e2-538">このハンズオン ラボを完了すると、Visual Studio 2012 のエディターで CSS、JavaScript と HTML の新機能と機能強化を使用する方法を習得がします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-538">By completing this Hands-On Lab, you have learnt how to use the new features and improvements in Visual Studio 2012 Editors for CSS, JavaScript and HTML.</span></span> <span data-ttu-id="8f6e2-539">さらに、Visual Studio 2012 で、組み込みのバンドルと縮小がどのように実装する方法を習得がします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-539">In addition, you have learnt how Visual Studio 2012 implements built-in bundling and minification.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="8f6e2-540">付録 a: をインストールする Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="8f6e2-540">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="8f6e2-541">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="8f6e2-541">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="8f6e2-542">次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-542">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="8f6e2-543">移動して[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-543">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="8f6e2-544">または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; *Visual Studio Express 2012 for Web と Windows Azure SDK*&quot;です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-544">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="8f6e2-545">をクリックして**を今すぐインストール**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-545">Click on **Install Now**.</span></span> <span data-ttu-id="8f6e2-546">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-546">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="8f6e2-547">1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-547">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="8f6e2-548">![Visual Studio Express インストール](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-548">![Install Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="8f6e2-549">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-549">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="8f6e2-550">すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-550">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    <span data-ttu-id="8f6e2-552">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-552">*Accepting the license terms*</span></span>
5. <span data-ttu-id="8f6e2-553">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-553">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    <span data-ttu-id="8f6e2-555">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-555">*Installation progress*</span></span>
6. <span data-ttu-id="8f6e2-556">インストールが完了したらをクリックして**完了**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-556">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    <span data-ttu-id="8f6e2-558">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-558">*Installation completed*</span></span>
7. <span data-ttu-id="8f6e2-559">をクリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-559">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="8f6e2-560">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-560">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web タイルを](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    <span data-ttu-id="8f6e2-562">*VS Express Web タイルを*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-562">*VS Express for Web tile*</span></span>

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="8f6e2-563">付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-563">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="8f6e2-564">この付録では、Windows Azure 管理ポータルから新しい web サイトを作成し、Windows Azure によって提供される Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを公開する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-564">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="8f6e2-565">タスク 1 - Windows から新しい Web サイトを作成する Azure ポータル</span><span class="sxs-lookup"><span data-stu-id="8f6e2-565">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="8f6e2-566">移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-566">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f6e2-567">Windows Azure 無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増大に応じて拡大縮小できます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-567">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="8f6e2-568">サインアップする[ここ](http://aka.ms/aspnet-hol-azure)です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-568">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="8f6e2-569">![Windows Azure ポータルにログオンする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Windows Azure ポータルにログオンします。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-569">![Log on to Windows Azure portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="8f6e2-570">*Windows Azure 管理ポータルにログオンします。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-570">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="8f6e2-571">をクリックして**新規**コマンド バーでします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-571">Click **New** on the command bar.</span></span>

    <span data-ttu-id="8f6e2-572">![新しい Web サイトを作成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-572">![Creating a new Web Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="8f6e2-573">*新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-573">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="8f6e2-574">をクリックして**コンピューティング** | **Web サイト**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-574">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="8f6e2-575">選択し、**簡易作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-575">Then select **Quick Create** option.</span></span> <span data-ttu-id="8f6e2-576">新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-576">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f6e2-577">Windows Azure Web サイトは、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-577">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="8f6e2-578">簡易作成 オプションでは、完成した web アプリケーションを Windows Azure Web サイトからポータル外を展開することができます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-578">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="8f6e2-579">データベースを設定するための手順は含まれません。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-579">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="8f6e2-580">![簡易作成を使用して新しい Web サイトを作成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "簡易作成を使用して新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-580">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="8f6e2-581">*簡易作成を使用して新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-581">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="8f6e2-582">新しいまで待つ**Web サイト**を作成します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-582">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="8f6e2-583">Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-583">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="8f6e2-584">新しい Web サイトが動作していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-584">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="8f6e2-585">![新しい web サイトを参照して](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "新しい web サイトを参照")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-585">![Browsing to the new web site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="8f6e2-586">*新しい web サイトを参照*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-586">*Browsing to the new web site*</span></span>

    <span data-ttu-id="8f6e2-587">![Web サイトを実行している](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "を実行している Web サイト")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-587">![Web site running](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Web site running")</span></span>

    <span data-ttu-id="8f6e2-588">*実行している web サイト*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-588">*Web site running*</span></span>
6. <span data-ttu-id="8f6e2-589">ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-589">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="8f6e2-590">![Web サイトの管理ページを開く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "web サイトの管理ページを開く")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-590">![Opening the web site management pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="8f6e2-591">*Web サイトの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-591">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="8f6e2-592">**ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-592">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f6e2-593">*発行プロファイル*すべてのパブリケーションを有効になっているメソッドの各 Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-593">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="8f6e2-594">発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-594">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="8f6e2-595">**Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Windows Azure の web サイトに web アプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="8f6e2-596">![発行プロファイルを web サイトをダウンロードする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "発行プロファイルを web サイトをダウンロードします。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-596">![Downloading the web site publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="8f6e2-597">*発行プロファイルを Web サイトをダウンロードします。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-597">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="8f6e2-598">既知の場所に発行プロファイル ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-598">Download the publish profile file to a known location.</span></span> <span data-ttu-id="8f6e2-599">さらにこの練習では、このファイルを使用して、Visual Studio から web アプリケーション、Windows Azure Web サイトを発行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-599">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="8f6e2-600">![発行プロファイル ファイルを保存](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "発行プロファイルの保存")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-600">![Saving the publish profile file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Saving the publish profile")</span></span>

    <span data-ttu-id="8f6e2-601">*発行プロファイル ファイルを保存します。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-601">*Saving the publish profile file*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="8f6e2-602">タスク 2 - データベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="8f6e2-602">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="8f6e2-603">アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-603">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="8f6e2-604">SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-604">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="8f6e2-605">SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-605">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="8f6e2-606">SQL データベース サーバーを表示するには、Windows Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-606">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="8f6e2-607">使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-607">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="8f6e2-608">注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-608">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="8f6e2-609">データベースを作成しない、まだと後の段階で作成されます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-609">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="8f6e2-610">![SQL データベース サーバーのダッシュ ボード](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL データベース サーバーのダッシュ ボード")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-610">![SQL Database Server Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="8f6e2-611">*SQL データベース サーバーのダッシュ ボード*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-611">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="8f6e2-612">次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-612">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="8f6e2-613">実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-613">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes.</span></span> <span data-ttu-id="8f6e2-614">ルールの名前を入力し、クリックして、 ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png)ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-614">Enter a name for the rule and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) button.</span></span>

    ![クライアントの IP アドレスを追加します。](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    <span data-ttu-id="8f6e2-616">*クライアントの IP アドレスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-616">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="8f6e2-617">1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-617">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![変更を確認します。](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    <span data-ttu-id="8f6e2-619">*変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-619">*Confirm Changes*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="8f6e2-620">タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="8f6e2-620">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="8f6e2-621">ASP.NET MVC 4 ソリューションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-621">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="8f6e2-622">**ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-622">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="8f6e2-623">![アプリケーションの発行](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-623">![Publishing the Application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publishing the Application")</span></span>

    <span data-ttu-id="8f6e2-624">*Web サイトの発行*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-624">*Publishing the web site*</span></span>
2. <span data-ttu-id="8f6e2-625">最初の作業を保存した発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-625">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="8f6e2-626">![発行プロファイルをインポートする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "発行プロファイルをインポートします。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-626">![Importing the publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importing the publish profile")</span></span>

    <span data-ttu-id="8f6e2-627">*発行プロファイルのインポート*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-627">*Importing publish profile*</span></span>
3. <span data-ttu-id="8f6e2-628">をクリックして**への接続検証**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-628">Click **Validate Connection**.</span></span> <span data-ttu-id="8f6e2-629">検証が完了したらクリックして**次**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-629">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f6e2-630">検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-630">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="8f6e2-631">![接続の検証](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "接続の検証")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-631">![Validating connection](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validating connection")</span></span>

    <span data-ttu-id="8f6e2-632">*接続の検証*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-632">*Validating connection*</span></span>
4. <span data-ttu-id="8f6e2-633">**設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-633">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="8f6e2-634">![Web 配置の構成](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web 配置の構成")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-634">![Web deploy configuration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy configuration")</span></span>

    <span data-ttu-id="8f6e2-635">*Web 配置の構成*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-635">*Web deploy configuration*</span></span>
5. <span data-ttu-id="8f6e2-636">次のように、データベースの接続を構成します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-636">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="8f6e2-637">**サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:*プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-637">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="8f6e2-638">**ユーザー名**サーバー管理者のログイン名を入力します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-638">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="8f6e2-639">**パスワード**サーバー管理者のログイン パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-639">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="8f6e2-640">たとえば、新しいデータベース名を入力: *MVC4SampleDB*です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-640">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="8f6e2-641">![対象の接続文字列を構成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "対象の接続文字列を構成します。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-641">![Configuring destination connection string](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="8f6e2-642">*対象の接続文字列を構成します。*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-642">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="8f6e2-643">次に、 **[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-643">Then click **OK**.</span></span> <span data-ttu-id="8f6e2-644">データベースをクリックを作成するように求められたら**はい**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-644">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="8f6e2-645">![データベースを作成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "データベース文字列を作成します。")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-645">![Creating the database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creating the database string")</span></span>

    <span data-ttu-id="8f6e2-646">*データベースの作成*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-646">*Creating the database*</span></span>
7. <span data-ttu-id="8f6e2-647">接続の既定のテキスト ボックス内では、Windows azure SQL データベースへの接続に使用する接続文字列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-647">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="8f6e2-648">その後、 **[次へ]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-648">Then click **Next**.</span></span>

    <span data-ttu-id="8f6e2-649">![SQL データベースを指す接続文字列](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "SQL データベースを指す接続文字列")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-649">![Connection string pointing to SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="8f6e2-650">*SQL データベースを指す接続文字列*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-650">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="8f6e2-651">**プレビュー** ] ページで [**発行**です。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-651">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="8f6e2-652">![Web アプリケーションの発行](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "web アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-652">![Publishing the web application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publishing the web application")</span></span>

    <span data-ttu-id="8f6e2-653">*Web アプリケーションの発行*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-653">*Publishing the web application*</span></span>
9. <span data-ttu-id="8f6e2-654">発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="8f6e2-654">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="8f6e2-655">![アプリケーションが Windows Azure に発行](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "アプリケーションが Windows Azure に発行")</span><span class="sxs-lookup"><span data-stu-id="8f6e2-655">![Application published to Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="8f6e2-656">*Windows Azure に発行されるアプリケーション*</span><span class="sxs-lookup"><span data-stu-id="8f6e2-656">*Application published to Windows Azure*</span></span>
