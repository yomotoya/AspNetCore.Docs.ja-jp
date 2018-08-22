---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: ASP.NET と Visual Studio 2012 での Web 開発における新 |Microsoft Docs
author: rick-anderson
description: 新しいバージョンの Visual Studio には、さまざまな Web テクノロジを使用する場合、エクスペリエンスとパフォーマンスの向上に重点を置いての機能強化が導入されています.
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 263a6e0aed51a681193333b53eff8f03847fc3aa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826751"
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a><span data-ttu-id="dba43-103">ASP.NET と Visual Studio 2012 での Web 開発における新します。</span><span class="sxs-lookup"><span data-stu-id="dba43-103">What's New in ASP.NET and Web Development in Visual Studio 2012</span></span>
====================
<span data-ttu-id="dba43-104">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="dba43-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="dba43-105">新しいバージョンの Visual Studio には、多数の Web テクノロジを使用する場合、エクスペリエンスとパフォーマンスの向上に重点を置いた機能が導入されています。</span><span class="sxs-lookup"><span data-stu-id="dba43-105">The new version of Visual Studio introduces a number of enhancements focused on improving the experience and performance when working with Web technologies.</span></span> <span data-ttu-id="dba43-106">CSS、JavaScript と HTML の visual Studio エディターは、IntelliSense や自動インデントなど、最も、オンデマンドでコード補助機能の多くを完全に刷新されました。</span><span class="sxs-lookup"><span data-stu-id="dba43-106">Visual Studio Editors for CSS, JavaScript and HTML have been completely revamped to include many of the most in-demand code aids, such as IntelliSense and automatic indentation.</span></span> <span data-ttu-id="dba43-107">パフォーマンスに関してバンドルと縮小は統合されました ページを簡単に削減する組み込み機能の読み込み時間と。</span><span class="sxs-lookup"><span data-stu-id="dba43-107">Regarding performance, bundling and minification are now integrated as built-in features to easily reduce page load time.</span></span>
> 
> <span data-ttu-id="dba43-108">Visual Studio では、最新の web サイト テクノロジと連携することができます。</span><span class="sxs-lookup"><span data-stu-id="dba43-108">Visual Studio enables you to work with the latest website technologies.</span></span> <span data-ttu-id="dba43-109">クロスブラウザー CSS3 のスニペットを使用すると、新しい HTML5 の要素と機能の活用しながら、サイトがクライアント プラットフォームに関係なく動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="dba43-109">You can use cross-browser CSS3 Snippets to make sure your site works regardless of the client platform while also taking advantage of the new HTML5 elements and features.</span></span>
> 
> <span data-ttu-id="dba43-110">書き込みと JavaScript コードのプロファイリングは、このバージョンの Visual Studio で簡単になります。</span><span class="sxs-lookup"><span data-stu-id="dba43-110">Writing and profiling JavaScript code should be easier with this Visual Studio version.</span></span> <span data-ttu-id="dba43-111">IntelliSense リストでは、XML ドキュメントとナビゲーション機能の統合は現在、JavaScript コードにはできます。</span><span class="sxs-lookup"><span data-stu-id="dba43-111">IntelliSense lists, integrated XML documentation and navigation features are now available for JavaScript code.</span></span> <span data-ttu-id="dba43-112">指先ひとつで、JavaScript のカタログがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="dba43-112">You now have the JavaScript catalog at your fingertips.</span></span> <span data-ttu-id="dba43-113">さらに、各自のスクリプトで ECMAScript5 のコンプライアンスを確認し、早い段階で構文エラーを検出できます。</span><span class="sxs-lookup"><span data-stu-id="dba43-113">Additionally, you can check ECMAScript5 compliance with your scripts and detect syntax errors at an early stage.</span></span>
> 
> <span data-ttu-id="dba43-114">最後に、しかし大事なこのバージョンの Visual Studio は、組み込みのバンドルと縮小を実装します。</span><span class="sxs-lookup"><span data-stu-id="dba43-114">Last but not least, this Visual Studio version implements built-in bundling and minification.</span></span> <span data-ttu-id="dba43-115">スタイル シート、スクリプト ファイルをパックし、圧縮、サイトが高速に実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="dba43-115">Your script files and style sheets will be packed and compressed so that the site performs faster.</span></span>
> 
> <span data-ttu-id="dba43-116">このラボでは、拡張機能とソース フォルダーにサンプル Web アプリケーションに軽微な変更を適用することで以前に説明する新機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="dba43-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="dba43-117">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)します。</span><span class="sxs-lookup"><span data-stu-id="dba43-117">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="dba43-118">目的</span><span class="sxs-lookup"><span data-stu-id="dba43-118">Objectives</span></span>

<span data-ttu-id="dba43-119">このハンズ オン ラボでは、学習する方法。</span><span class="sxs-lookup"><span data-stu-id="dba43-119">In this hands on lab, you will learn how to:</span></span>

- <span data-ttu-id="dba43-120">CSS エディターの新機能と機能強化を使用します。</span><span class="sxs-lookup"><span data-stu-id="dba43-120">Use the new features and improvements in the CSS editor</span></span>
- <span data-ttu-id="dba43-121">HTML エディターで新しい機能と機能強化を使用します。</span><span class="sxs-lookup"><span data-stu-id="dba43-121">Use the new features and improvements in the HTML editor</span></span>
- <span data-ttu-id="dba43-122">新しい機能と機能強化を使用して、JavaScript エディター</span><span class="sxs-lookup"><span data-stu-id="dba43-122">Use the new features and improvements in the JavaScript editor</span></span>
- <span data-ttu-id="dba43-123">構成および使用のバンドルと縮小</span><span class="sxs-lookup"><span data-stu-id="dba43-123">Configure and use bundling and minification</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="dba43-124">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="dba43-124">Prerequisites</span></span>

- <span data-ttu-id="dba43-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="dba43-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="dba43-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (セットアップ スクリプト - Windows 8 および Windows Server 2008 R2 に既にインストールされている) に使用</span><span class="sxs-lookup"><span data-stu-id="dba43-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (for setup scripts - already installed on Windows 8 and Windows Server 2008 R2)</span></span>
- <span data-ttu-id="dba43-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home)または HTML5 準拠のブラウザー</span><span class="sxs-lookup"><span data-stu-id="dba43-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - or an HTML5 compliant browser</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="dba43-128">演習</span><span class="sxs-lookup"><span data-stu-id="dba43-128">Exercises</span></span>

<span data-ttu-id="dba43-129">このハンズオン ラボでは、次の演習が含まれています。</span><span class="sxs-lookup"><span data-stu-id="dba43-129">This hands on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="dba43-130">CSS エディターの新機能がどのような演習 1 です。</span><span class="sxs-lookup"><span data-stu-id="dba43-130">Exercise 1: What's New in the CSS Editor</span></span>](#Exercise1)
2. [<span data-ttu-id="dba43-131">手順 2: 新機能については、HTML エディターです。</span><span class="sxs-lookup"><span data-stu-id="dba43-131">Exercise 2: What's New in the HTML Editor</span></span>](#Exercise2)
3. [<span data-ttu-id="dba43-132">手順 3: 新機能については、JavaScript エディターです。</span><span class="sxs-lookup"><span data-stu-id="dba43-132">Exercise 3: What's New in the JavaScript Editor</span></span>](#Exercise3)
4. [<span data-ttu-id="dba43-133">手順 4: バンドルと縮小</span><span class="sxs-lookup"><span data-stu-id="dba43-133">Exercise 4: Bundling and Minification</span></span>](#Exercise4)

<span data-ttu-id="dba43-134">この演習の所要時間を推定: **60 分**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-134">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a><span data-ttu-id="dba43-135">CSS エディターの新機能がどのような演習 1 です。</span><span class="sxs-lookup"><span data-stu-id="dba43-135">Exercise 1: What's New in the CSS Editor</span></span>

<span data-ttu-id="dba43-136">Web 開発者は、CSS の編集に関連する問題の多くをよく知って必要があります。</span><span class="sxs-lookup"><span data-stu-id="dba43-136">Web developers should be familiar with many of the difficulties related to CSS editing.</span></span> <span data-ttu-id="dba43-137">CSS スタイルの最も大きな問題の 1 つは、ブラウザーの間の互換性です。</span><span class="sxs-lookup"><span data-stu-id="dba43-137">One of the biggest issues of CSS styling is cross-browser compatibility.</span></span> <span data-ttu-id="dba43-138">多くの場合、スタイルを適用すると、サイト、わかります表示が異なること別のブラウザーまたはデバイスで開く場合行われます。</span><span class="sxs-lookup"><span data-stu-id="dba43-138">It often happens that, after applying styles to your site, you notice that it looks different if you open it in another browser or device.</span></span> <span data-ttu-id="dba43-139">そのため、その視覚的な問題、最後に 1 つのブラウザーで動作することを行ったときに壊れている、他のユーザーに認識する修正にかなりの時間を費やす可能性があります。</span><span class="sxs-lookup"><span data-stu-id="dba43-139">Therefore, you may spend a considerable time on fixing those visual issues to realize that, when you finally make it work in one browser, it is broken in the others.</span></span>

<span data-ttu-id="dba43-140">Visual Studio には、開発者は、アクセス、動作、および CSS スタイル シートを効果的に整理に役立つ機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="dba43-140">Visual Studio now includes features that help developers access, work and organize CSS style sheets effectively.</span></span> <span data-ttu-id="dba43-141">この演習での 有効な組織や edition の新機能とブラウザー間で互換性の CSS3 のコード スニペットを満たします。</span><span class="sxs-lookup"><span data-stu-id="dba43-141">Throughout this exercise, you will meet the new features for an effective organization and edition, as well as the CSS3 Code Snippets for cross-browser compatibility.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a><span data-ttu-id="dba43-142">タスク 1 - エディターの新機能</span><span class="sxs-lookup"><span data-stu-id="dba43-142">Task 1 - New Editor Features</span></span>

<span data-ttu-id="dba43-143">このタスクでは、CSS エディターの新しい機能を検出します。</span><span class="sxs-lookup"><span data-stu-id="dba43-143">In this task, you will discover the new features of the CSS Editor.</span></span> <span data-ttu-id="dba43-144">この新しいエディターを使用すると、新しいスマート インデント、改善されたコードのコメントおよび強化された IntelliSense リストを活用して生産性を向上できます。</span><span class="sxs-lookup"><span data-stu-id="dba43-144">This new editor will help you increase your productivity by taking advantage of the new smart indentation, the improved code comments and the enhanced IntelliSense list.</span></span>

1. <span data-ttu-id="dba43-145">開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**ソリューション、 **Source\WhatsNewASPNET**このラボのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="dba43-145">Start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dba43-146">ソリューション エクスプ ローラーで開く、 **Site.css**下にあるファイル、**スタイル**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="dba43-146">In Solution Explorer, open the **Site.css** file located under the **Styles** folder.</span></span> <span data-ttu-id="dba43-147">必ず、**テキスト エディター**ツールは、ツールバーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-147">Make sure the **Text Editor** tools are visible on the toolbar.</span></span> <span data-ttu-id="dba43-148">そのためには次のように選択します。、**ビュー** | **ツールバー**メニュー オプション、およびチェック、**テキスト エディター**オプション。</span><span class="sxs-lookup"><span data-stu-id="dba43-148">To do that, select the **View** | **Toolbars** menu option, and check the **Text Editor** options.</span></span> <span data-ttu-id="dba43-149">以降、この新しいバージョンでは、いることを確認は、**コメント**ボタン (![コメント ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png)) および**コメントを解除します**ボタン (![のコメントを解除します-ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png))、CSS エディターのも有効にします。</span><span class="sxs-lookup"><span data-stu-id="dba43-149">You will notice that, since this new version, the **Comment** button (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) and the **Uncomment** button (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) ) are also enabled for the CSS editor.</span></span>

    <span data-ttu-id="dba43-150">![エディターと CSS ツールを有効にする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "エディターと CSS ツールを有効にします。")</span><span class="sxs-lookup"><span data-stu-id="dba43-150">![Enabling Editor and CSS Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Enabling Editor and CSS Tools")</span></span>

    <span data-ttu-id="dba43-151">*エディターと CSS ツールを有効にします。*</span><span class="sxs-lookup"><span data-stu-id="dba43-151">*Enabling Editor and CSS Tools*</span></span>
3. <span data-ttu-id="dba43-152">コードをスクロールし、任意の CSS クラスの定義を選択します。</span><span class="sxs-lookup"><span data-stu-id="dba43-152">Scroll the code and select any CSS class definition.</span></span> <span data-ttu-id="dba43-153">をクリックして、**コメント**(![コメント ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png)) ボタンを選択した行をコメントします。</span><span class="sxs-lookup"><span data-stu-id="dba43-153">Click the **Comment** (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) button to comment the selected lines.</span></span> <span data-ttu-id="dba43-154">をクリックし、**コメントを解除します**(![のコメントを解除ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png))、変更を元に戻すボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="dba43-154">Then, click the **Uncomment** (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) button to undo the changes.</span></span>
4. <span data-ttu-id="dba43-155">をクリックして、**折りたたむ**(![折りたたむ](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png)) と**展開**(![展開](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png)) ボタンがテキストの左余白にあります。</span><span class="sxs-lookup"><span data-stu-id="dba43-155">Click the **Collapse** (![collapse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) and **Expand** (![expand](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) buttons located on the left margin of the text.</span></span> <span data-ttu-id="dba43-156">クリーナー ビューを使用しないスタイルを隠すようになりましたことができることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-156">Notice that you can now hide the styles you don't use to have a cleaner view.</span></span>

    <span data-ttu-id="dba43-157">![CSS クラスの折りたたみ](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "折りたたみの CSS クラス")</span><span class="sxs-lookup"><span data-stu-id="dba43-157">![Collapsing CSS classes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Collapsing CSS classes")</span></span>

    <span data-ttu-id="dba43-158">*CSS クラスの折りたたみ*</span><span class="sxs-lookup"><span data-stu-id="dba43-158">*Collapsing CSS classes*</span></span>
5. <span data-ttu-id="dba43-159">スマート インデント機能が有効になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="dba43-159">Make sure that the smart indentation feature is enabled.</span></span> <span data-ttu-id="dba43-160">選択、**ツール** | **オプション**メニュー オプション、および選択し、**テキスト エディター** | **CSS**  | **書式**画面の左側のウィンドウでページ。</span><span class="sxs-lookup"><span data-stu-id="dba43-160">Select the **Tools** | **Options** menu option, and then select the **Text Editor** | **CSS** | **Formatting** page in the left pane of the screen.</span></span> <span data-ttu-id="dba43-161">チェック、**階層インデント**オプション。</span><span class="sxs-lookup"><span data-stu-id="dba43-161">Check the **Hierarchical indentation** option.</span></span>

    <span data-ttu-id="dba43-162">![階層インデントを有効にする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "階層インデントを有効にします。")</span><span class="sxs-lookup"><span data-stu-id="dba43-162">![Enabling hierarchical indentation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Enabling hierarchical indentation")</span></span>

    <span data-ttu-id="dba43-163">*階層インデントを有効にします。*</span><span class="sxs-lookup"><span data-stu-id="dba43-163">*Enabling hierarchical indentation*</span></span>
6. <span data-ttu-id="dba43-164">メイン クラスの定義 (.main) を検索し、div 要素にスタイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="dba43-164">Locate the main class definition (.main) and append a style to the div elements.</span></span> <span data-ttu-id="dba43-165">コード配置の概要、親クラスを検索するユーザーで自動的に表示されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-165">You will notice that the code aligns automatically, helping users to find the parent classes at a glance.</span></span>

    <span data-ttu-id="dba43-166">CSS</span><span class="sxs-lookup"><span data-stu-id="dba43-166">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    <span data-ttu-id="dba43-167">![CSS で階層的な配置](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "CSS 内の階層の配置")</span><span class="sxs-lookup"><span data-stu-id="dba43-167">![Hierarchical alignment in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchical alignment in CSS")</span></span>

    <span data-ttu-id="dba43-168">*CSS で階層的な配置*</span><span class="sxs-lookup"><span data-stu-id="dba43-168">*Hierarchical alignment in CSS*</span></span>
7. <span data-ttu-id="dba43-169">内で **.main div**クラスの末尾にカーソルを置いて**border: 0 px;** キーを押します **」と入力**IntelliSense の一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="dba43-169">Inside **.main div** class, locate the cursor at the end of **border: 0px;** and press **Enter** to display the IntelliSense list.</span></span> <span data-ttu-id="dba43-170">入力を開始**上部**に入力すると、一覧のフィルター方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-170">Start typing **top** and notice how the list is filtered as you type.</span></span> <span data-ttu-id="dba43-171">リストが含まれている要素が表示されます**上部**という単語の任意の部分で (Visual Studio の以前のバージョンでは、リストは項目でフィルター処理を*開始*用語で)。</span><span class="sxs-lookup"><span data-stu-id="dba43-171">The list will display the elements that contain **top** at any part of the word (In prior versions of Visual Studio, the list is filtered by the items that *begin* with the term).</span></span>

    <span data-ttu-id="dba43-172">![CSS で IntelliSense の機能強化](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "CSS で IntelliSense の機能強化")</span><span class="sxs-lookup"><span data-stu-id="dba43-172">![IntelliSense enhancements in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense enhancements in CSS")</span></span>

    <span data-ttu-id="dba43-173">*CSS で IntelliSense の機能強化*</span><span class="sxs-lookup"><span data-stu-id="dba43-173">*IntelliSense enhancements in CSS*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a><span data-ttu-id="dba43-174">タスク 2 - カラー ピッカー</span><span class="sxs-lookup"><span data-stu-id="dba43-174">Task 2 - The Color Picker</span></span>

<span data-ttu-id="dba43-175">このタスクでは、Visual Studio の IntelliSense 統合され、新しい CSS カラー ピッカーを検出します。</span><span class="sxs-lookup"><span data-stu-id="dba43-175">In this task, you will discover the new CSS Color Picker integrated into Visual Studio IntelliSense.</span></span>

1. <span data-ttu-id="dba43-176">**Site.css、** ヘッダーのクラス定義 (.header) を見つけて横にカーソルを置き**背景色**属性は、間、 &quot;:&quot;と&quot; #&quot;文字コードの行に**します。**</span><span class="sxs-lookup"><span data-stu-id="dba43-176">In **Site.css,** locate the header class definition (.header) and place the cursor next to **background-color** attribute, between the &quot;:&quot; and &quot;#&quot; characters on that line of code **.**</span></span>

    <span data-ttu-id="dba43-177">![カーソルを検索する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "カーソルを検索します。")</span><span class="sxs-lookup"><span data-stu-id="dba43-177">![Locating the cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Locating the cursor")</span></span>

    <span data-ttu-id="dba43-178">*カーソルを検索します。*</span><span class="sxs-lookup"><span data-stu-id="dba43-178">*Locating the cursor*</span></span>
2. <span data-ttu-id="dba43-179">削除、**コロン**(:)、カラー ピッカーを表示するには、もう一度記述します。</span><span class="sxs-lookup"><span data-stu-id="dba43-179">Delete the **colon** (:) and write it again to display the color picker.</span></span> <span data-ttu-id="dba43-180">最初の色が表示されますが、サイトの最も一般的な色に注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-180">Notice that the first colors you will see are the most frequent colors of your site.</span></span> <span data-ttu-id="dba43-181">白のカラーをクリックした場合、HTML のカラー コード (#fff) は、スタイル シートでは、現在のカラー コードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="dba43-181">If you click the white color, its HTML color code (#fff) will replace the current color code in the stylesheet.</span></span>

    <span data-ttu-id="dba43-182">![カラー ピッカー](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "カラー ピッカー")</span><span class="sxs-lookup"><span data-stu-id="dba43-182">![Color picker](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Color picker")</span></span>

    <span data-ttu-id="dba43-183">*カラー ピッカー*</span><span class="sxs-lookup"><span data-stu-id="dba43-183">*Color picker*</span></span>
3. <span data-ttu-id="dba43-184">キーを押して、**展開**(![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) 色のグラデーションを表示し、別の色を選択するグラデーションのカーソルをドラッグするには、カラー ピッカーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="dba43-184">Press the **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) button on the color picker to display the color gradient, and then drag the gradient cursor to select a different color.</span></span> <span data-ttu-id="dba43-185">その後、をクリックして、**スポイト**ボタンをクリックし、画面から色を選択します。</span><span class="sxs-lookup"><span data-stu-id="dba43-185">After that, click the **Eyedropper** button and select any color from the screen.</span></span> <span data-ttu-id="dba43-186">カーソルを移動するときに、背景色の値が動的に変更することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-186">Notice that background color value changes dynamically while you move the cursor.</span></span>

    <span data-ttu-id="dba43-187">![グラデーション ピッカー](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "グラデーション ピッカー")</span><span class="sxs-lookup"><span data-stu-id="dba43-187">![Color picker gradient](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Color picker gradient")</span></span>

    <span data-ttu-id="dba43-188">*グラデーション ピッカー*</span><span class="sxs-lookup"><span data-stu-id="dba43-188">*Color picker gradient*</span></span>
4. <span data-ttu-id="dba43-189">**不透明度**スライダー、セレクターを不透明度を減らすバーの中央に移動します。</span><span class="sxs-lookup"><span data-stu-id="dba43-189">In the **Opacity** slider, move the selector to the center of the bar to reduce the opacity.</span></span> <span data-ttu-id="dba43-190">背景色の値は、RGBA にそのスケールを今すぐ変更に注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-190">Notice that background-color value now changes its scale to RGBA.</span></span>

    <span data-ttu-id="dba43-191">![カラー ピッカーの不透明度](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "不透明度のカラー ピッカー")</span><span class="sxs-lookup"><span data-stu-id="dba43-191">![Color picker Opacity](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Color picker Opacity")</span></span>

    <span data-ttu-id="dba43-192">*不透明度のカラー ピッカー*</span><span class="sxs-lookup"><span data-stu-id="dba43-192">*Color picker Opacity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dba43-193">CSS3 で RGBA (赤、緑、青、アルファ) の色の定義では、1 つの項目の色の不透明度値を定義することができます。</span><span class="sxs-lookup"><span data-stu-id="dba43-193">The RGBA (Red, Green, Blue, Alpha) color definition in CSS3 enables you to define the color opacity value for a single item.</span></span> <span data-ttu-id="dba43-194">異なり**不透明度 -** のような CSS 属性**-** RGBA 色は、最新のブラウザーと互換性があります。</span><span class="sxs-lookup"><span data-stu-id="dba43-194">Unlike **opacity -** a similar CSS attribute **-** RGBA colors are also compatible with the latest browsers.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a><span data-ttu-id="dba43-195">タスク 3 - CSS 互換性のあるコード スニペット</span><span class="sxs-lookup"><span data-stu-id="dba43-195">Task 3 - CSS Compatible Code Snippets</span></span>

<span data-ttu-id="dba43-196">このタスクでは、web サイトで一部の機能を実装するためにクロス ブラウザーの互換性のある CSS3 のスニペットを使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="dba43-196">In this task, you will learn how to use cross-browser compatible CSS3 snippets in order to implement some features in your website.</span></span>

1. <span data-ttu-id="dba43-197">**Site.css**を探し、ファイル、**ヘッダー** CSS クラスの定義 (.header) と、以下のカーソルを置き、  **/\*境界の半径\*/** プレース ホルダーを新しいスニペットを追加します。</span><span class="sxs-lookup"><span data-stu-id="dba43-197">In the **Site.css** file, locate the **header** CSS class definition (.header) and place the cursor below the **/\*border radius\*/** placeholder to add a new snippet.</span></span> <span data-ttu-id="dba43-198">キーを押して **」と入力**、IntelliSense の一覧を表示し**radius**一覧をフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="dba43-198">Press **Enter** to display the IntelliSense list and type **radius** to filter the list.</span></span> <span data-ttu-id="dba43-199">選択、**境界の半径**ダブル クリックで、一覧からオプションし、キーを押します、**タブ**スニペットを挿入するキー。</span><span class="sxs-lookup"><span data-stu-id="dba43-199">Select the **border-radius** option from the list with a double click, and then press the **TAB** key to insert the snippet.</span></span> <span data-ttu-id="dba43-200">Radius のサイズを入力し、ピクセルとキーを押して**Enter**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-200">Then, type a radius size in pixels and press **Enter**.</span></span> <span data-ttu-id="dba43-201">たとえば、入力**15px**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-201">For instance, type **15px**.</span></span>

    <span data-ttu-id="dba43-202">スニペットによって追加される CSS3 属性は、角の丸い境界線などの Mozilla ブラウザーの WebKit ベース HTML5 準拠のほとんどのブラウザーにレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="dba43-202">The CSS3 attributes added by the snippet will render rounded borders in most HTML5 compliance browsers, including Mozilla and WebKit-based browsers.</span></span>

    <span data-ttu-id="dba43-203">![境界の半径のスニペットを使用して](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "境界の半径のスニペットを使用して")</span><span class="sxs-lookup"><span data-stu-id="dba43-203">![Using a border-radius snippet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Using a border-radius snippet")</span></span>

    <span data-ttu-id="dba43-204">*境界の半径のスニペットを使用してください。*</span><span class="sxs-lookup"><span data-stu-id="dba43-204">*Using a border-radius snippet*</span></span>
2. <span data-ttu-id="dba43-205">同じ**境界線**スニペットでは、ページのスタイル (.page)。</span><span class="sxs-lookup"><span data-stu-id="dba43-205">Apply the same **border** snippets in the page style (.page).</span></span>

    <span data-ttu-id="dba43-206">CSS</span><span class="sxs-lookup"><span data-stu-id="dba43-206">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. <span data-ttu-id="dba43-207">キーを押して**F5**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dba43-207">Press **F5** to run the solution.</span></span> <span data-ttu-id="dba43-208">各ページようになりましたが丸い境界線に注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-208">Notice that each page now has rounded borders.</span></span>

    <span data-ttu-id="dba43-209">![角が丸く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "角を丸く")</span><span class="sxs-lookup"><span data-stu-id="dba43-209">![Rounded corners](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Rounded corners")</span></span>

    <span data-ttu-id="dba43-210">*角が丸い*</span><span class="sxs-lookup"><span data-stu-id="dba43-210">*Rounded corners*</span></span>
4. <span data-ttu-id="dba43-211">ブラウザーを閉じて、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="dba43-211">Close the browser and return to Visual Studio.</span></span>
5. <span data-ttu-id="dba43-212">開く、 **Custom.css**下にあるファイル、**スタイル**フォルダー内でカーソルを置き**div.images ul li img**クラスの定義。</span><span class="sxs-lookup"><span data-stu-id="dba43-212">Open the **Custom.css** file located under the **Styles** folder and place the cursor inside **div.images ul li img** class definition.</span></span>
6. <span data-ttu-id="dba43-213">Enter キーを押して IntelliSense リストを表示型**ボックス シャドウ**キーを押すと、**タブ**クラス定義内の既定のシャドウ コード スニペットを挿入するには、2 回のキー。</span><span class="sxs-lookup"><span data-stu-id="dba43-213">Press enter to display the IntelliSense list, type **box-shadow** and press the **TAB** key twice to insert the default shadow code snippet inside the class definition.</span></span> <span data-ttu-id="dba43-214">シャドウの値に設定**10px 10px 5 px #888**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-214">Set the shadow values to **10px 10px 5px #888**.</span></span> <span data-ttu-id="dba43-215">次に、入力**境界の半径**とコード スニペットを挿入します。</span><span class="sxs-lookup"><span data-stu-id="dba43-215">Then, type **border-radius** and insert the code snippet.</span></span> <span data-ttu-id="dba43-216">型**15px**半径のサイズとキーを押してを設定する**ENTER**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-216">Type **15px** to set radius size and press **ENTER**.</span></span>

    <span data-ttu-id="dba43-217">![シャドウを伴って角を丸く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "シャドウを伴って角を丸く")</span><span class="sxs-lookup"><span data-stu-id="dba43-217">![Rounded corners with shadow](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Rounded corners with shadow")</span></span>

    <span data-ttu-id="dba43-218">*影付きの角が丸い*</span><span class="sxs-lookup"><span data-stu-id="dba43-218">*Rounded corners with shadow*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dba43-219">この時点で、対応するプレフィックス (moz、webkit、o) Mozilla をサポートするために、Webkit (Chrome、Safari、Konkeror) ブラウザーとシャドウ属性が挿入されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-219">At this moment, the shadow attribute is inserted with the corresponding prefix (moz, webkit, o) to support Mozilla and Webkit (Chrome, Safari, Konkeror) browsers.</span></span>
7. <span data-ttu-id="dba43-220">新しいクラスを作成**div.images ul li img:hover**下、 **div.images ul li img**クラスの定義と、角かっこ内でカーソルを置き**します。**</span><span class="sxs-lookup"><span data-stu-id="dba43-220">Create a new class **div.images ul li img:hover** below the **div.images ul li img** class definition and place the cursor inside the brackets **.**</span></span>

    <span data-ttu-id="dba43-221">CSS</span><span class="sxs-lookup"><span data-stu-id="dba43-221">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. <span data-ttu-id="dba43-222">型**変換**キーを押すと、**タブ**変換スニペットを挿入するために 2 回のキー。</span><span class="sxs-lookup"><span data-stu-id="dba43-222">Type **transform** and press the **TAB** key twice in order to insert the transform snippet.</span></span> <span data-ttu-id="dba43-223">次に、入力**rotate(-15deg)** イメージが置かれているときに、回転の角度の値を変更します。</span><span class="sxs-lookup"><span data-stu-id="dba43-223">Then, enter **rotate(-15deg)** to change the rotation angle value when images are hovered.</span></span>

    <span data-ttu-id="dba43-224">CSS</span><span class="sxs-lookup"><span data-stu-id="dba43-224">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. <span data-ttu-id="dba43-225">キーを押して**F5**ソリューションを実行し、CSS3 のページを参照します。</span><span class="sxs-lookup"><span data-stu-id="dba43-225">Press **F5** to run the solution and browse to the CSS3 page.</span></span> <span data-ttu-id="dba43-226">イメージがコーナーが丸くし、影のボックスに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-226">Notice that the images have rounded corners and box shadows.</span></span> <span data-ttu-id="dba43-227">イメージの上にマウスを移動し、回転させることを確認します。</span><span class="sxs-lookup"><span data-stu-id="dba43-227">Hover the mouse over the images and watch them rotate.</span></span>

    <span data-ttu-id="dba43-228">![スニペットのイメージの回転の変換](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "イメージの回転変換スニペット")</span><span class="sxs-lookup"><span data-stu-id="dba43-228">![Transform snippet rotating an image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transform snippet rotating an image")</span></span>

    <span data-ttu-id="dba43-229">*スニペットのイメージの回転の変換します。*</span><span class="sxs-lookup"><span data-stu-id="dba43-229">*Transform snippet rotating an image*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dba43-230">Internet Explorer 10 を使用しているし、影を表示することはできませんの場合は、ドキュメント モードが IE10 標準に設定されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-230">If you are using Internet Explorer 10 and cannot see the shadows, make sure the document mode is set to IE10 standards.</span></span> <span data-ttu-id="dba43-231">キーを押して**F12**を Internet Explorer 開発者ツールを開き、をクリックして**ドキュメント モード**IE10 標準に変更します。</span><span class="sxs-lookup"><span data-stu-id="dba43-231">Press **F12** to open Internet Explorer developer tools and click **Document Mode** to change to IE10 standards.</span></span>

    ![についてのご](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a><span data-ttu-id="dba43-233">手順 2: 新機能については、HTML エディターです。</span><span class="sxs-lookup"><span data-stu-id="dba43-233">Exercise 2: What's New in the HTML Editor</span></span>

<span data-ttu-id="dba43-234">Visual Studio では、HTML エディターの強化には。</span><span class="sxs-lookup"><span data-stu-id="dba43-234">Visual Studio has an improved HTML editor.</span></span> <span data-ttu-id="dba43-235">このバージョンに含まれる拡張機能の一部は、HTML ドキュメント、HTML5 のスニペット、HTML の開始と終了タグが一致する、および HTML 検証のスマート インデントです。</span><span class="sxs-lookup"><span data-stu-id="dba43-235">Some of the enhancements included in this version are smart indentation in HTML documents, HTML5 snippets, HTML start and end tag matching, and HTML validation.</span></span> <span data-ttu-id="dba43-236">この演習では、web サイトのマークアップで作業するときに、これらの変更が、自在に操作できることを改善する方法がわかります。</span><span class="sxs-lookup"><span data-stu-id="dba43-236">Throughout this exercise, you will see how these changes improve your fluency when working in the website markup.</span></span>

<span data-ttu-id="dba43-237">CSS エディターのように、HTML エディターも強化されました。</span><span class="sxs-lookup"><span data-stu-id="dba43-237">Like the CSS editor, the HTML editor was also improved.</span></span> <span data-ttu-id="dba43-238">これらの機能強化のほとんどは、Web 開発者の作業を簡単にする小規模なものです。</span><span class="sxs-lookup"><span data-stu-id="dba43-238">Most of these improvements are small ones that make the Web developer's life easier.</span></span> <span data-ttu-id="dba43-239">さらにコード スニペットは、html5、スマート インデントは、編集、および HTML ドキュメントの DOCTYPE を対象とする検証がこれらの機能強化の一部である場合の開始および終了タグに一致するなど。</span><span class="sxs-lookup"><span data-stu-id="dba43-239">Things like more code snippets for HTML5, smart indentation, matching start and end tags when editing and validation targeting the HTML document DOCTYPE are some of these improvements.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a><span data-ttu-id="dba43-240">タスク 1 - 強化された DOCTYPE の検証</span><span class="sxs-lookup"><span data-stu-id="dba43-240">Task 1 - Improved DOCTYPE Validation</span></span>

<span data-ttu-id="dba43-241">HTML エディターでは、マスター ページで、定義としても、今すぐ、ページの DOCTYPE を確認する機能があります。</span><span class="sxs-lookup"><span data-stu-id="dba43-241">The HTML editor now has the ability to check the DOCTYPE of your page, even though the definition might be in the master page.</span></span> <span data-ttu-id="dba43-242">によって、ページの DOCTYPE、HTML エディターは適切な一連の規則の検証し、DOCTYPE 要素を検討して IntelliSense の一覧をフィルター処理されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-242">Depending on the DOCTYPE of your page, the HTML editor will validate with the correct set of rules and will filter the IntelliSense list considering the DOCTYPE elements.</span></span>

<span data-ttu-id="dba43-243">このタスクでは、HTML エディターの動作を適宜変更する方法を表示するページの DOCTYPE が変わります。</span><span class="sxs-lookup"><span data-stu-id="dba43-243">In this task, you will change the DOCTYPE of a page to see how the HTML editor behavior changes accordingly.</span></span>

1. <span data-ttu-id="dba43-244">これを開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**ソリューション、 **Source\WhatsNewASPNET**このラボのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="dba43-244">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dba43-245">開く、 **Site.Master**ページ。</span><span class="sxs-lookup"><span data-stu-id="dba43-245">Open the **Site.Master** page.</span></span>
3. <span data-ttu-id="dba43-246">検証のツールバーには、ターゲット スキーマに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-246">Notice the Target Schema for Validation Toolbar.</span></span> <span data-ttu-id="dba43-247">HTML エディターの動作 (検証、IntelliSense など) は、Doctype の選択に合わせて正しく変更されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-247">The way the HTML editor behaves (Validation, IntelliSense, etc.) will properly change to fit the Doctype selected.</span></span>

    <span data-ttu-id="dba43-248">![Doctype を使用して、HTML ソースの編集 ツールバーで](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "HTML ソースの編集 ツールバーで使用して Doctype")</span><span class="sxs-lookup"><span data-stu-id="dba43-248">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="dba43-249">*HTML ソースの編集 ツールバーに Doctype を使用します。*</span><span class="sxs-lookup"><span data-stu-id="dba43-249">*Use Doctype in HTML Source Editing toolbar*</span></span>
4. <span data-ttu-id="dba43-250">HTML 4.01 にターゲット スキーマを変更します。</span><span class="sxs-lookup"><span data-stu-id="dba43-250">Change the Target Schema to HTML 4.01.</span></span>

    <span data-ttu-id="dba43-251">![HTML ソースの編集 ツールバーに Doctype を変更する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "HTML ソースの編集 ツールバーで Doctype の変更")</span><span class="sxs-lookup"><span data-stu-id="dba43-251">![Changing Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Changing Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="dba43-252">*HTML ソースの編集 ツールバーで Doctype の変更*</span><span class="sxs-lookup"><span data-stu-id="dba43-252">*Changing Doctype in HTML Source Editing toolbar*</span></span>
5. <span data-ttu-id="dba43-253">カーソルを置き、**本文**要素、および HTML5 の要素の名前を入力する (たとえば、**ビデオ**)。</span><span class="sxs-lookup"><span data-stu-id="dba43-253">Place the cursor under the **body** element, and start typing the name of an HTML5 element (for example, **video**).</span></span> <span data-ttu-id="dba43-254">要素が、IntelliSense リストで使用できないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-254">Notice that the element is not available in the IntelliSense list.</span></span>

    <span data-ttu-id="dba43-255">![HTML5 の要素が表示されていない](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 の要素が表示されていません。")</span><span class="sxs-lookup"><span data-stu-id="dba43-255">![HTML5 elements not listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elements not listed")</span></span>

    <span data-ttu-id="dba43-256">*HTML5 の要素が表示されていません。*</span><span class="sxs-lookup"><span data-stu-id="dba43-256">*HTML5 elements not listed*</span></span>
6. <span data-ttu-id="dba43-257">検証のツールバーで、DOCTYPE の選択のターゲット スキーマへの変更を元に戻す: ドロップダウン リストから XHTML5 します。</span><span class="sxs-lookup"><span data-stu-id="dba43-257">Undo the changes to the Target Schema for Validation Toolbar, picking DOCTYPE: XHTML5 from the dropdown list.</span></span>

    <span data-ttu-id="dba43-258">![Doctype を使用して、HTML ソースの編集 ツールバーで](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "HTML ソースの編集 ツールバーで使用して Doctype")</span><span class="sxs-lookup"><span data-stu-id="dba43-258">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="dba43-259">*Doctype の HTML ソースの編集 ツールバーをリセットします。*</span><span class="sxs-lookup"><span data-stu-id="dba43-259">*Reset Doctype in HTML Source Editing toolbar*</span></span>
7. <span data-ttu-id="dba43-260">下にカーソルを置き、**本文**HTML5 の要素をもう一度入力する要素と (たとえば、**ビデオ**)。</span><span class="sxs-lookup"><span data-stu-id="dba43-260">Place the cursor under the **body** element and start typing an HTML5 element again (For example, like **video**).</span></span> <span data-ttu-id="dba43-261">HTML5 の要素は、IntelliSense リストで使用できるようになりましたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-261">Notice that the HTML5 elements are now available in the IntelliSense list.</span></span>

    <span data-ttu-id="dba43-262">![HTML5 の要素が表示される](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 の要素が一覧表示すること")</span><span class="sxs-lookup"><span data-stu-id="dba43-262">![HTML5 elements being listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elements being listed")</span></span>

    <span data-ttu-id="dba43-263">*HTML5 の要素が一覧表示すること*</span><span class="sxs-lookup"><span data-stu-id="dba43-263">*HTML5 elements being listed*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a><span data-ttu-id="dba43-264">タスク 2 - 開始/終了タグの自動更新</span><span class="sxs-lookup"><span data-stu-id="dba43-264">Task 2 - Start/End Tags Automatic Update</span></span>

<span data-ttu-id="dba43-265">Visual Studio は、今すぐ開く、または終了タグが相互に一致する編集している要素の HTML を更新します。</span><span class="sxs-lookup"><span data-stu-id="dba43-265">Visual Studio now updates the HTML opening or closing tags of the element that you are editing to match each other.</span></span> <span data-ttu-id="dba43-266">HTML タグを編集するときに、この新しい機能では、生産性が向上します。</span><span class="sxs-lookup"><span data-stu-id="dba43-266">This new feature will improve your productivity when editing HTML tags.</span></span>

1. <span data-ttu-id="dba43-267">**Default.aspx**ページで、追加、 **H3**タイトル (たとえば、Visual Studio 2012 Rocks!) を持つ要素。</span><span class="sxs-lookup"><span data-stu-id="dba43-267">On the **Default.aspx** page, add an **H3** element with a title (for example, Visual Studio 2012 Rocks!).</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. <span data-ttu-id="dba43-268">変更、 **H3**タグと型**H2**または**H1 します。**</span><span class="sxs-lookup"><span data-stu-id="dba43-268">Change the **H3** tag and type **H2** or **H1.**</span></span>

    <span data-ttu-id="dba43-269">終了タグが自動的に更新することを確認します。</span><span class="sxs-lookup"><span data-stu-id="dba43-269">Notice that the end tag automatically updates.</span></span> <span data-ttu-id="dba43-270">開始タグを更新するそれに応じてすぎるを表示する終了タグを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="dba43-270">You can also modify the end tag to see that the start tag updates accordingly too.</span></span>

    <span data-ttu-id="dba43-271">![終了タグの自動更新](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "終了タグの自動更新")</span><span class="sxs-lookup"><span data-stu-id="dba43-271">![Automatic update of the end tag](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatic update of the end tag")</span></span>

    <span data-ttu-id="dba43-272">*終了タグの自動更新*</span><span class="sxs-lookup"><span data-stu-id="dba43-272">*Automatic update of the end tag*</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a><span data-ttu-id="dba43-273">タスク 3 - 新しい HTML5 コード スニペット</span><span class="sxs-lookup"><span data-stu-id="dba43-273">Task 3 - New HTML5 Code Snippets</span></span>

<span data-ttu-id="dba43-274">Visual Studio には、いくつかの HTML5 のコード スニペットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="dba43-274">Visual Studio now includes several HTML5 code snippets.</span></span> <span data-ttu-id="dba43-275">このタスクでは、これらのスニペットの一部を使用します。</span><span class="sxs-lookup"><span data-stu-id="dba43-275">In this task, you will use some of these snippets.</span></span>

1. <span data-ttu-id="dba43-276">という名前の新しいフォルダーを追加**オーディオ**web サイト フォルダーのルートにします。</span><span class="sxs-lookup"><span data-stu-id="dba43-276">Add a new folder named **audio** to the root of the web site folder.</span></span> <span data-ttu-id="dba43-277">Windows エクスプ ローラーを開きにオーディオ ファイルをコピー、**オーディオ**のフォルダー、 **WhatsNewASPNET.sln**ソリューション。</span><span class="sxs-lookup"><span data-stu-id="dba43-277">Open Windows Explorer and copy any audio file into the **audio** folder of the **WhatsNewASPNET.sln** solution.</span></span>
2. <span data-ttu-id="dba43-278">**Default.aspx**  ページで、Web11 Rocks でカーソルを置いてください。</span><span class="sxs-lookup"><span data-stu-id="dba43-278">In the **Default.aspx** page, locate the cursor under the Web11 Rocks!!</span></span> <span data-ttu-id="dba43-279">ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="dba43-279">Header.</span></span> <span data-ttu-id="dba43-280">型**オーディオ**TAB キーを押します。</span><span class="sxs-lookup"><span data-stu-id="dba43-280">Type **audio** and press the TAB key.</span></span>

    <span data-ttu-id="dba43-281">新しい HTML エディターには、HTML5 のコンテンツのコード スニペットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="dba43-281">The new HTML editor includes code snippets for HTML5 content.</span></span> <span data-ttu-id="dba43-282">適切な DOCTYPE 定義を使用して、HTML5 のスニペットを有効にしてください。</span><span class="sxs-lookup"><span data-stu-id="dba43-282">Remember to use the proper DOCTYPE definition to enable the HTML5 snippets.</span></span>

    <span data-ttu-id="dba43-283">![HTML5 のコード スニペットの挿入](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "HTML5 コード スニペットの挿入")</span><span class="sxs-lookup"><span data-stu-id="dba43-283">![Inserting HTML5 Code Snippets](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserting HTML5 Code Snippets")</span></span>

    <span data-ttu-id="dba43-284">*HTML5 のコード スニペットの挿入*</span><span class="sxs-lookup"><span data-stu-id="dba43-284">*Inserting HTML5 Code Snippets*</span></span>
3. <span data-ttu-id="dba43-285">既存のオーディオ ファイルをポイントするオーディオ ソースを更新します。</span><span class="sxs-lookup"><span data-stu-id="dba43-285">Update the audio source to point to an existing audio file.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > <span data-ttu-id="dba43-286">オーディオ ファイルをソリューションに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dba43-286">You will need to add the audio file to the solution.</span></span>
4. <span data-ttu-id="dba43-287">キーを押して**F5**サイトを実行し、オーディオを再生します。</span><span class="sxs-lookup"><span data-stu-id="dba43-287">Press **F5** to run the site and play the audio.</span></span>

    <span data-ttu-id="dba43-288">![オーディオ コントロールを実行している](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "オーディオ コントロールの実行")</span><span class="sxs-lookup"><span data-stu-id="dba43-288">![Running the audio control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Running the audio control")</span></span>

    <span data-ttu-id="dba43-289">*オーディオ コントロールの実行*</span><span class="sxs-lookup"><span data-stu-id="dba43-289">*Running the audio control*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dba43-290">ビデオや図など、Visual Studio に含まれる複数のスニペットを試すこともできます。</span><span class="sxs-lookup"><span data-stu-id="dba43-290">You can also try more snippets included in Visual Studio, such as video, figure, etc.</span></span>
5. <span data-ttu-id="dba43-291">ここで、ページの一部のコントロールを挿入してみてください。</span><span class="sxs-lookup"><span data-stu-id="dba43-291">Now, try to insert a control in some part of the page.</span></span> <span data-ttu-id="dba43-292">挿入しようとするなど、 **GridView**コントロールが入力する代わりに**&lt;合わせる、** の入力を開始 **&lt;GV**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-292">For example, try to insert a **GridView** control, but instead of typing **&lt;Gri,** start typing **&lt;GV**.</span></span> <span data-ttu-id="dba43-293">IntelliSense の一覧を示していますが、 **: asp GridView**コントロール。</span><span class="sxs-lookup"><span data-stu-id="dba43-293">Notice that the IntelliSense list shows the **asp:GridView** control.</span></span>

    <span data-ttu-id="dba43-294">HTML エディターでの IntelliSense は、タイトルの大文字と小文字の検索と部分一致する (要素を取得するすべての用語を含む) に現在提供します。</span><span class="sxs-lookup"><span data-stu-id="dba43-294">IntelliSense in the HTML Editor now provides title-casing search, as well as partial matching (retrieving all elements that contains the term).</span></span>

    <span data-ttu-id="dba43-295">![IntelliSense のリストを使用した GridView を挿入する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "IntelliSense リストを使用した GridView を挿入します。")</span><span class="sxs-lookup"><span data-stu-id="dba43-295">![Inserting a GridView with IntelliSense lists](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserting a GridView with IntelliSense lists")</span></span>

    <span data-ttu-id="dba43-296">*IntelliSense のリストを使用した GridView を挿入します。*</span><span class="sxs-lookup"><span data-stu-id="dba43-296">*Inserting a GridView with IntelliSense lists*</span></span>

    <span data-ttu-id="dba43-297">入力した場合**&lt;グリッド**という用語に一致するすべての項目が表示されますが、Visual Studio をお勧めしますが、 **gridview**コントロール。</span><span class="sxs-lookup"><span data-stu-id="dba43-297">If you type **&lt;grid** you will get all the items that match the term, but Visual Studio will suggest the **gridview** control:</span></span>

    <span data-ttu-id="dba43-298">![IntelliSense リスト部分に一致すると、GridView を挿入する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "IntelliSense リスト部分に一致すると、GridView を挿入します。")</span><span class="sxs-lookup"><span data-stu-id="dba43-298">![Inserting a GridView with IntelliSense lists and partial matching](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserting a GridView with IntelliSense lists and partial matching")</span></span>

    <span data-ttu-id="dba43-299">*IntelliSense リスト部分に一致すると、GridView を挿入します。*</span><span class="sxs-lookup"><span data-stu-id="dba43-299">*Inserting a GridView with IntelliSense lists and partial matching*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a><span data-ttu-id="dba43-300">タスク 4 - HTML エディターのスマート タグ</span><span class="sxs-lookup"><span data-stu-id="dba43-300">Task 4 - HTML Editor Smart Tags</span></span>

<span data-ttu-id="dba43-301">別の機能強化、HTML エディターでは、スマート タグ機能です。</span><span class="sxs-lookup"><span data-stu-id="dba43-301">Another improvement in the HTML Editor is the Smart Tags feature.</span></span> <span data-ttu-id="dba43-302">スマート タグを簡単にコントロールごとに一般的なや反復的な開発タスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="dba43-302">Smart tags make it easy to perform common or repetitive development tasks on a per-control basis.</span></span> <span data-ttu-id="dba43-303">この機能が既に使用できる HTML デザイナーで、HTML エディターでありません。</span><span class="sxs-lookup"><span data-stu-id="dba43-303">This feature was already available in the HTML Designer, but not in the HTML Editor.</span></span>

1. <span data-ttu-id="dba43-304">開いている**Site.Master**探し、 **asp: メニュー**要素。</span><span class="sxs-lookup"><span data-stu-id="dba43-304">Open **Site.Master** and locate the **asp:Menu** element.</span></span> <span data-ttu-id="dba43-305">カーソルを置き、開始タグと - 要素の下部に表示される小さいグリフをクリックして、[スマート タスク] メニューを開くことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-305">Place the cursor on the start tag and notice that the small glyph displayed at the bottom of the element - click it to open the smart tasks menu.</span></span> <span data-ttu-id="dba43-306">メニュー コントロールに関連するいくつかのタスクにすばやくアクセスすることがわかります。</span><span class="sxs-lookup"><span data-stu-id="dba43-306">Notice that you have quick access to some tasks related to the Menu control.</span></span>

    <span data-ttu-id="dba43-307">![メニュー コントロールのタスクのスマート](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "メニュー コントロールのタスクのスマート")</span><span class="sxs-lookup"><span data-stu-id="dba43-307">![Smart tasks for the Menu control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart tasks for the Menu control")</span></span>

    <span data-ttu-id="dba43-308">*メニュー コントロールのスマート タスク*</span><span class="sxs-lookup"><span data-stu-id="dba43-308">*Smart tasks for the Menu control*</span></span>

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a><span data-ttu-id="dba43-309">タスク 5 - スマート インデント</span><span class="sxs-lookup"><span data-stu-id="dba43-309">Task 5 - Smart Indentation</span></span>

<span data-ttu-id="dba43-310">読み取り可能なコードを保持する入れ子になった要素をインデント HTML でのベスト プラクティスの 1 つです。</span><span class="sxs-lookup"><span data-stu-id="dba43-310">One of the best practices in HTML is indenting the nested elements to keep the code readable.</span></span> <span data-ttu-id="dba43-311">Visual Studio 2012 で、コードを記述するときに、エディターがその要素を自動的にインデントすることがわかります。</span><span class="sxs-lookup"><span data-stu-id="dba43-311">In Visual Studio 2012, you will notice that the editor automatically indents the elements while you are writing the code.</span></span>

> [!NOTE]
> <span data-ttu-id="dba43-312">Visual Studio の以前のバージョンではスマート インデントされた使用可能な HTML エディターではなく、XML エディターにします。</span><span class="sxs-lookup"><span data-stu-id="dba43-312">In previous version of Visual Studio, smart indentation was available in the XML editor but not in the HTML editor.</span></span>


1. <span data-ttu-id="dba43-313">スマート インデントに HTML エディターでインデント構成が設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="dba43-313">Make sure that the Indenting configuration on the HTML Editor is set to Smart Indentation.</span></span> <span data-ttu-id="dba43-314">そのためには次のように選択します、**ツール |。オプション**メニュー オプションと選択し、**テキスト エディター |HTML |タブ**画面の左側のウィンドウでページ。</span><span class="sxs-lookup"><span data-stu-id="dba43-314">To do that, select the **Tools | Options** menu option and then select the **Text Editor | HTML | Tabs** page in the left pane of the screen.</span></span> <span data-ttu-id="dba43-315">スマート インデント オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="dba43-315">Select the Smart indentation option.</span></span>

    <span data-ttu-id="dba43-316">![HTML エディター設定](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML エディターの設定")</span><span class="sxs-lookup"><span data-stu-id="dba43-316">![HTML Editor settings](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML Editor settings")</span></span>

    <span data-ttu-id="dba43-317">*HTML エディターの設定*</span><span class="sxs-lookup"><span data-stu-id="dba43-317">*HTML Editor settings*</span></span>
2. <span data-ttu-id="dba43-318">**Default.aspx**  ページで、オーディオの要素の下のすべてのコンテンツを削除します。</span><span class="sxs-lookup"><span data-stu-id="dba43-318">On the **Default.aspx** page, remove all the content under the audio element.</span></span>
3. <span data-ttu-id="dba43-319">開始の末尾にカーソルを置き**オーディオ**要素とヒット**ENTER**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-319">Place the cursor at the end of the opening **audio** element and hit **ENTER**.</span></span>

    <span data-ttu-id="dba43-320">新しいカーソルの位置、インデントの追加レベルであることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-320">Notice that the new position of cursor has an additional indentation level.</span></span>

    <span data-ttu-id="dba43-321">![スマート インデント HTML エディターで](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "スマート インデント、HTML エディター")</span><span class="sxs-lookup"><span data-stu-id="dba43-321">![Smart indentation in the HTML Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart indentation in the HTML Editor")</span></span>

    <span data-ttu-id="dba43-322">*HTML エディターでのスマート インデント*</span><span class="sxs-lookup"><span data-stu-id="dba43-322">*Smart indentation in the HTML Editor*</span></span>
4. <span data-ttu-id="dba43-323">オーディオ コンテンツを削除するか、閉じるタグを復元**Default.aspx**せず、変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="dba43-323">Restore the audio tag with the content you have removed, or close **Default.aspx** without saving the changes.</span></span>

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a><span data-ttu-id="dba43-324">タスク 6: ユーザー コントロールへの抽出</span><span class="sxs-lookup"><span data-stu-id="dba43-324">Task 6 - Extract to User Control</span></span>

<span data-ttu-id="dba43-325">抽出する関数の場合、コードの一部など、Visual Studio に含まれる、リファクタリング ツールを改善し、既存のコードのリファクタリングを容易にするための優れた機能です。</span><span class="sxs-lookup"><span data-stu-id="dba43-325">The Refactoring tools included in Visual Studio, such as extracting a portion of code to a function, are great features that facilitate the improvement and the refactoring the existing code.</span></span> <span data-ttu-id="dba43-326">ASP.NET ページの対応するユーザー コントロールへの HTML コードの抽出となります。</span><span class="sxs-lookup"><span data-stu-id="dba43-326">The counterpart for ASP.NET pages would be the extraction of HTML code to a User Control.</span></span> <span data-ttu-id="dba43-327">手動で行うと、新しいユーザー コントロールの作成、ユーザー コントロールに、コードのセクションを移動、ユーザー コントロールのタグ プレフィックスを登録および、最後に、ページ上のユーザー コントロールをインスタンス化するように、いくつかの手順は必要があります。</span><span class="sxs-lookup"><span data-stu-id="dba43-327">Doing it manually would involve several steps, like creating a new User Control, moving the code section to the User Control, registering a tag prefix for the User Control, and, finally, instantiating the User Control on the pages.</span></span> <span data-ttu-id="dba43-328">ここで、新しい*ユーザー コントロールに展開*ツールが自動的にこれらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="dba43-328">Now, the new *Extract to User Control* tool automatically performs all those steps for you.</span></span>

<span data-ttu-id="dba43-329">このタスクでは、選択したコードから新しいユーザー コントロールを生成するのにコンテキストの操作をユーザー コントロールに新しい展開を使用します。</span><span class="sxs-lookup"><span data-stu-id="dba43-329">In this task, you will use the new Extract to User Control contextual operation to generate a new user control from the selected code.</span></span>

1. <span data-ttu-id="dba43-330">**Default.aspx** ] ページで、[、 **H2**と**オーディオ**要素。</span><span class="sxs-lookup"><span data-stu-id="dba43-330">On the **Default.aspx** page, select the **H2** and **audio** elements.</span></span>
2. <span data-ttu-id="dba43-331">右クリックし、選択**ユーザー コントロールに展開**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-331">Right click and select **Extract to User Control**.</span></span>

    <span data-ttu-id="dba43-332">![ユーザー コントロール メニュー オプションに抽出](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "ユーザー コントロール メニュー オプションに抽出します。")</span><span class="sxs-lookup"><span data-stu-id="dba43-332">![Extract to User Control menu option](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Extract to User Control menu option")</span></span>

    <span data-ttu-id="dba43-333">*ユーザー コントロール メニュー オプションに抽出します。*</span><span class="sxs-lookup"><span data-stu-id="dba43-333">*Extract to User Control menu option*</span></span>
3. <span data-ttu-id="dba43-334">新しいユーザー コントロールの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="dba43-334">Type a name for the new user control.</span></span> <span data-ttu-id="dba43-335">たとえば、 **Jukebox.ascx**、 をクリックし、 **OK**。</span><span class="sxs-lookup"><span data-stu-id="dba43-335">For instance, **Jukebox.ascx**, and then click **OK**.</span></span>

    <span data-ttu-id="dba43-336">![抽出されたユーザー コントロールを保存](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "抽出されたユーザー コントロールを保存しています")</span><span class="sxs-lookup"><span data-stu-id="dba43-336">![Saving the extracted user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Saving the extracted user control")</span></span>

    <span data-ttu-id="dba43-337">*抽出されたユーザー コントロールを保存しています*</span><span class="sxs-lookup"><span data-stu-id="dba43-337">*Saving the extracted user control*</span></span>
4. <span data-ttu-id="dba43-338">ユーザー コントロールに、選択したコードの抽出、選択したコードの元の場所は、新しいユーザー コントロールのインスタンスに置き換えられたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-338">Notice that the selected code was extracted to a user control and the original location of the selected code was replaced with an instance of the new user control.</span></span>

    <span data-ttu-id="dba43-339">![新しいユーザー コントロールを使用するページが自動的に更新](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "ページは、新しいユーザー コントロールを使用して自動的に更新")</span><span class="sxs-lookup"><span data-stu-id="dba43-339">![Page automatically updated to use the new user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page automatically updated to use the new user control")</span></span>

    <span data-ttu-id="dba43-340">*新しいユーザー コントロールを使用するページが自動的に更新*</span><span class="sxs-lookup"><span data-stu-id="dba43-340">*Page automatically updated to use the new user control*</span></span>
5. <span data-ttu-id="dba43-341">キーを押して**F5**ページを実行し、コントロールが動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="dba43-341">Press **F5** to run the page and verify that the control works.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a><span data-ttu-id="dba43-342">手順 3: 新機能については、JavaScript エディターです。</span><span class="sxs-lookup"><span data-stu-id="dba43-342">Exercise 3: What's New in the JavaScript Editor</span></span>

<span data-ttu-id="dba43-343">書き込みまたは JavaScript コードの編集が簡単な作業では、特に、アプリケーションの起動時にサイズの増加や長いファイルを扱うと数百の関数の自分で検索する場合。</span><span class="sxs-lookup"><span data-stu-id="dba43-343">Writing or editing JavaScript code is not an easy task, especially when your application starts to grow in size and you find yourself dealing with long files and hundreds of functions.</span></span> <span data-ttu-id="dba43-344">通常、スクリプトの開発者は、コードの読みやすさを維持し、ファイルをナビゲートする特別な作業を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="dba43-344">Script developers usually have to do some extra work to maintain code legibility and navigate across files.</span></span> <span data-ttu-id="dba43-345">スクリプトのナビゲーションで jQuery などの JavaScript ライブラリのコードの長さのため自体課題となっています。</span><span class="sxs-lookup"><span data-stu-id="dba43-345">With the inclusion of JavaScript libraries like jQuery, script navigation has become a challenge itself because of the code length.</span></span>

<span data-ttu-id="dba43-346">Visual Studio には、アクセス可能で、構成されたコードのモードにする約束を JavaScript エディターが更新されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-346">Visual Studio has renewed the JavaScript editor with the promise to make the code mode accessible and organized.</span></span> <span data-ttu-id="dba43-347">JavaScript エディターで c# または VB のエディターで既に存在していた多くの Visual Studio の機能が実装されるようになりました。 定義へ移動、自動インデント、ドキュメント、および記述するときに検証します。</span><span class="sxs-lookup"><span data-stu-id="dba43-347">Many Visual Studio features that already existed in C# or VB editors are now implemented in the JavaScript editor: Go To Definition, automatic indentation, documentation and validation when you are writing.</span></span> <span data-ttu-id="dba43-348">更新された IntelliSense リストでは、指先ひとつで JavaScript 関数のカタログをことができます。</span><span class="sxs-lookup"><span data-stu-id="dba43-348">With the renewed IntelliSense list you will have the JavaScript function catalog at your fingertips.</span></span>

<span data-ttu-id="dba43-349">この演習では、JavaScript エディターの機能強化と新機能の一部を説明します。</span><span class="sxs-lookup"><span data-stu-id="dba43-349">In this exercise, you will learn some of the new features and improvements of JavaScript editor.</span></span> <span data-ttu-id="dba43-350">サンプル ファイルを参照し、それぞれが、JavaScript プログラミングの Visual Studio 2012 内でより効率的なように新しい特性を検出します。</span><span class="sxs-lookup"><span data-stu-id="dba43-350">You will browse sample files and discover each of the new characteristics that will make your JavaScript programming more efficient within Visual Studio 2012.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a><span data-ttu-id="dba43-351">タスク 1 - JavaScript エディターの新機能</span><span class="sxs-lookup"><span data-stu-id="dba43-351">Task 1 - JavaScript Editor New Features</span></span>

<span data-ttu-id="dba43-352">このタスクでは、コードを整理し、優れたユーザー エクスペリエンスを集中新しい JavaScript エディター機能をいくつか紹介します。</span><span class="sxs-lookup"><span data-stu-id="dba43-352">This task will introduce you to some of the new JavaScript editor features, which focus on organizing your code and bringing a better user experience.</span></span>

1. <span data-ttu-id="dba43-353">これを開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**ソリューション、 **Source\WhatsNewASPNET**このラボのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="dba43-353">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dba43-354">キーを押して**F5**アプリケーションを実行し、ナビゲーション バーで JavaScript リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="dba43-354">Press **F5** to run the application, then click the JavaScript link in the navigation bar.</span></span> <span data-ttu-id="dba43-355">ページを更新、いくつかの時間とチェックする方法はカウンターがインクリメントします。</span><span class="sxs-lookup"><span data-stu-id="dba43-355">Refresh the page several times and check how the counter increments.</span></span>

    <span data-ttu-id="dba43-356">![ページ カウンター](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "ページ カウンター")</span><span class="sxs-lookup"><span data-stu-id="dba43-356">![Page counter](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page counter")</span></span>

    <span data-ttu-id="dba43-357">*ページ カウンター*</span><span class="sxs-lookup"><span data-stu-id="dba43-357">*Page counter*</span></span>
3. <span data-ttu-id="dba43-358">ブラウザーを閉じて、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="dba43-358">Close the browser and go back to Visual Studio.</span></span>
4. <span data-ttu-id="dba43-359">開く、 **JavaScript.aspx**ページし、検索、 **&lt;スクリプト&gt;** ブロック (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="dba43-359">Open the **JavaScript.aspx** page and locate the **&lt;script&gt;** block (shown below).</span></span>

    <span data-ttu-id="dba43-360">次のコードを格納する HTML5 ローカル ストレージを使用して、 *pageLoadCount*ページが現在のユーザーがアクセスした回数の合計を格納する変数です。</span><span class="sxs-lookup"><span data-stu-id="dba43-360">The following code uses HTML5 local storage to store a *pageLoadCount* variable that stores the number of times the page has been visited by the current user.</span></span> <span data-ttu-id="dba43-361">ローカル ストレージは、標準の HTML5 で導入されたクライアント側のキー値データベースです。</span><span class="sxs-lookup"><span data-stu-id="dba43-361">Local Storage is a client-side key-value database introduced with the HTML5 standard.</span></span> <span data-ttu-id="dba43-362">データは、ユーザーのブラウザー内で、ローカル コンピューターに保存されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-362">The data is saved on the local machine, inside the user's browser.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="dba43-363">DOCTYPE 次の手順に進む前に XHTML5 に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="dba43-363">Ensure the DOCTYPE is set to XHTML5 before proceeding with the next steps.</span></span>
5. <span data-ttu-id="dba43-364">コードを編集して、JavaScript 用 IntelliSense がローカル ストレージは、その内部メソッドなどの HTML5 機能が含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-364">Edit the code and notice that IntelliSense for JavaScript includes HTML5 features, like local storage, and their inner methods.</span></span>

    <span data-ttu-id="dba43-365">![JavaScript での HTML5 の JavaScript 機能](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "JavaScript での HTML5 の JavaScript 機能")</span><span class="sxs-lookup"><span data-stu-id="dba43-365">![HTML5 JavaScript features in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript features in JavaScript")</span></span>

    <span data-ttu-id="dba43-366">*JavaScript での HTML5 の JavaScript 機能*</span><span class="sxs-lookup"><span data-stu-id="dba43-366">*HTML5 JavaScript features in JavaScript*</span></span>
6. <span data-ttu-id="dba43-367">任意の始め角かっこをクリックして (**{**) から、スクリプト コードし、角かっこが強調表示されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-367">Click any opening bracket (**{**) from the scripting code and notice that the brackets are highlighted.</span></span>

    <span data-ttu-id="dba43-368">![角かっこが強調表示されている](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "角かっこが強調表示されます")</span><span class="sxs-lookup"><span data-stu-id="dba43-368">![Brackets are highlighted](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Brackets are highlighted")</span></span>

    <span data-ttu-id="dba43-369">*角かっこが強調表示されます。*</span><span class="sxs-lookup"><span data-stu-id="dba43-369">*Brackets are highlighted*</span></span>
7. <span data-ttu-id="dba43-370">関数をコメント解除します**testAutoAlign()** (3 つの行を選択し、使用することができます**CTRL** + **K**;**CTRL** + **U**) し、関数コード内にカーソルを探します。</span><span class="sxs-lookup"><span data-stu-id="dba43-370">Uncomment the function **testAutoAlign()** (select the three lines and you can use **CTRL** + **K**; **CTRL** + **U**) and locate the cursor inside the function code.</span></span> <span data-ttu-id="dba43-371">2 番目の行を追加して enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="dba43-371">Press enter to append a second line.</span></span> <span data-ttu-id="dba43-372">これで、コードが通知**揃え**と**自動インデント**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-372">Notice that the code is now **aligned** and **auto-indented**.</span></span>

    <span data-ttu-id="dba43-373">![JavaScript コードは自動配置](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript コードは自動配置")</span><span class="sxs-lookup"><span data-stu-id="dba43-373">![JavaScript code is auto aligned](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript code is auto aligned")</span></span>

    <span data-ttu-id="dba43-374">*JavaScript コードは自動配置*</span><span class="sxs-lookup"><span data-stu-id="dba43-374">*JavaScript code is auto aligned*</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a><span data-ttu-id="dba43-375">タスク 2 - JavaScript の検証</span><span class="sxs-lookup"><span data-stu-id="dba43-375">Task 2 - Validating JavaScript</span></span>

<span data-ttu-id="dba43-376">このタスクでは、標準の ECMAScript5 の新しい JavaScript 検証を検出します。</span><span class="sxs-lookup"><span data-stu-id="dba43-376">In this task, you will discover the new JavaScript validation for the ECMAScript5 standard.</span></span> <span data-ttu-id="dba43-377">この機能はサイトに展開する前にスクリプトの問題を防止しながら、準拠の JavaScript コードを記述するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="dba43-377">This feature will help you to write compliant JavaScript code, while preventing scripting issues before site deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="dba43-378">Visual Studio 2010 では、Visual Studio 2012 ECMAScript5 のコンプライアンスを提供しますが、ECMAStript3 のコンプライアンスが実装されています。</span><span class="sxs-lookup"><span data-stu-id="dba43-378">Visual Studio 2010 implemented ECMAStript3 compliance, while Visual Studio 2012 provides ECMAScript5 compliance.</span></span>


1. <span data-ttu-id="dba43-379">開いている**ECMA5script5.js**の下にある、 **Scripts\custom**プロジェクト フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="dba43-379">Open **ECMA5script5.js** located under the **Scripts\custom** project folder.</span></span> <span data-ttu-id="dba43-380">今すぐ ECMAScript5 の標準の検証をテストします。</span><span class="sxs-lookup"><span data-stu-id="dba43-380">You will now test validation for ECMAScript5 standard.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    <span data-ttu-id="dba43-381">チェック アウトすることができます、 &quot; **strict 使用** &quot; ECMAScript5 できると、ファイルの最初の行方向**厳格モード**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-381">You can check out the &quot; **use strict** &quot; direction in the first line of the file, which enables ECMAScript5 **strict mode**.</span></span> <span data-ttu-id="dba43-382">このモードでは、過去のエディションからあいまいさを明確化し、オブジェクトのプロパティの getter と setter、JSON、およびより完全なリフレクション ライブラリのサポートなど、いくつかの新機能を追加する言語のサブセットで構成されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-382">This mode consists in a subset of the language that clarifies ambiguities from the past edition, and adds some new features, such as getters and setters, library support for JSON, and more complete reflection on object properties.</span></span>
2. <span data-ttu-id="dba43-383">開く、**エラー一覧**開いていない場合 (**ビュー**メニュー |**エラー一覧**)。</span><span class="sxs-lookup"><span data-stu-id="dba43-383">Open the **Error List** if not already opened (**View** menu | **Error List**).</span></span> <span data-ttu-id="dba43-384">通知、**関数**宣言の下線が引かれます。</span><span class="sxs-lookup"><span data-stu-id="dba43-384">Notice the **function** declaration is underlined.</span></span> <span data-ttu-id="dba43-385">これは、ため、ECMA5 で標準的な関数内にネストできません言語構造体です。</span><span class="sxs-lookup"><span data-stu-id="dba43-385">This is because in ECMA5 standard functions cannot be nested inside language structures.</span></span> <span data-ttu-id="dba43-386">エラーが発生する次の一覧に警告の詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-386">In the error list below you will see the warning details.</span></span>

    <span data-ttu-id="dba43-387">![JavaScript の検証エラー メッセージ](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript の検証エラー メッセージ")</span><span class="sxs-lookup"><span data-stu-id="dba43-387">![JavaScript validation error message](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript validation error message")</span></span>

    <span data-ttu-id="dba43-388">*JavaScript の検証エラー メッセージ*</span><span class="sxs-lookup"><span data-stu-id="dba43-388">*JavaScript validation error message*</span></span>
3. <span data-ttu-id="dba43-389">コメント アウト、  **&quot;strict 使用&quot;** 方向とエラーが非表示になりますが、警告のままにすることがわかります。</span><span class="sxs-lookup"><span data-stu-id="dba43-389">Comment out the **&quot;use strict&quot;** direction and notice that errors disappear, but the warnings remain.</span></span>
4. <span data-ttu-id="dba43-390">ファイルの最後の行のような任意の文字列を書き込む**&quot;テスト&quot;** (文字列であることを示す引用符を含む)。</span><span class="sxs-lookup"><span data-stu-id="dba43-390">In the last line of the file, write any string like **&quot;test&quot;** (include the quotation marks to indicate it is as string).</span></span> <span data-ttu-id="dba43-391">IntelliSense の一覧を表示し、選択する文字列の横にあるピリオドを書き込み、**トリミング**オプション。</span><span class="sxs-lookup"><span data-stu-id="dba43-391">Write a period next to the string to display the IntelliSense list, and select the **trim** option.</span></span>

    <span data-ttu-id="dba43-392">ECMAScript5 の標準で文字列値および変数もメソッドを持つ文字列定義、トリミング、大文字、検索し、置換と同様にします。</span><span class="sxs-lookup"><span data-stu-id="dba43-392">In ECMAScript5 standard, string values and variables also have string methods defined, like trim, uppercase, search and replace.</span></span>

    <span data-ttu-id="dba43-393">![JavaScript での IntelliSense リスト](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "javascript IntelliSense の一覧")</span><span class="sxs-lookup"><span data-stu-id="dba43-393">![IntelliSense list in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense list in JavaScript")</span></span>

    <span data-ttu-id="dba43-394">*Javascript IntelliSense の一覧*</span><span class="sxs-lookup"><span data-stu-id="dba43-394">*IntelliSense list in JavaScript*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a><span data-ttu-id="dba43-395">タスク 3 - JavaScript の XML ドキュメント</span><span class="sxs-lookup"><span data-stu-id="dba43-395">Task 3 - XML Documentation for JavaScript</span></span>

<span data-ttu-id="dba43-396">このタスクでは、JavaScript での XML ドキュメントの Visual Studio の機能を説明します。</span><span class="sxs-lookup"><span data-stu-id="dba43-396">In this task, you will explore Visual Studio features for XML documentation in JavaScript.</span></span> <span data-ttu-id="dba43-397">JavaScript IntelliSense の一覧では、各関数の XML ドキュメントに表示されますが表示されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-397">You will see the JavaScript IntelliSense list now shows the XML documentation of each function.</span></span> <span data-ttu-id="dba43-398">さらに、JavaScript でのナビゲーション機能が検出されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-398">Additionally, you will discover the navigation feature in JavaScript.</span></span>

1. <span data-ttu-id="dba43-399">開いている**XMLDoc.js**にあるファイル**スクリプト/カスタム**プロジェクト フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="dba43-399">Open **XMLDoc.js** file located in **Scripts/custom** project folder.</span></span> <span data-ttu-id="dba43-400">このファイルには、JavaScript 関数の各 XML ドキュメントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="dba43-400">This file contains XML documentation on each of the JavaScript functions.</span></span>

    <span data-ttu-id="dba43-401">![JavaScript XML ドキュメントが IntelliSense に統合された](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML ドキュメントに IntelliSense が統合されています。")</span><span class="sxs-lookup"><span data-stu-id="dba43-401">![JavaScript XML documentation integrated to IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentation integrated to IntelliSense")</span></span>

    <span data-ttu-id="dba43-402">*IntelliSense に統合された JavaScript XML ドキュメント*</span><span class="sxs-lookup"><span data-stu-id="dba43-402">*JavaScript XML documentation integrated to IntelliSense*</span></span>
2. <span data-ttu-id="dba43-403">下**追加**関数**XMLDoc.js**ファイル、という名前の新しい関数を作成**テスト**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-403">Below **add** function in **XMLDoc.js** file, create a new function named **test**.</span></span>
3. <span data-ttu-id="dba43-404">**テスト**関数を呼び出し、**乗算**を 2 つのパラメーターを受け取る関数。</span><span class="sxs-lookup"><span data-stu-id="dba43-404">In the **test** function, call the **multiply** function that receives two parameters.</span></span> <span data-ttu-id="dba43-405">ツールヒント ボックスが表示されていることを確認、**乗算**関数のドキュメント。</span><span class="sxs-lookup"><span data-stu-id="dba43-405">Notice the tooltip box is showing the **multiply** function documentation.</span></span>

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    <span data-ttu-id="dba43-406">![JavaScript 関数の XML ドキュメント](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "JavaScript 関数の XML ドキュメント")</span><span class="sxs-lookup"><span data-stu-id="dba43-406">![XML documentation for JavaScript functions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML documentation for JavaScript functions")</span></span>

    <span data-ttu-id="dba43-407">*JavaScript 関数の XML ドキュメント*</span><span class="sxs-lookup"><span data-stu-id="dba43-407">*XML documentation for JavaScript functions*</span></span>
4. <span data-ttu-id="dba43-408">種類と関数の呼び出しステートメントの完了を*ドット*戻り値に対して、IntelliSense の一覧を開きます。</span><span class="sxs-lookup"><span data-stu-id="dba43-408">Complete the function call statement and type a *dot* to open the IntelliSense list on the returned value.</span></span> <span data-ttu-id="dba43-409">Visual Studio の検出に注意してください、**返す**値、ドキュメントでは、数値として処理されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-409">Notice that Visual Studio is detecting the **return** value in the documentation, treating the value as a number.</span></span>

    <span data-ttu-id="dba43-410">![戻り値の型の XML ドキュメント](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "戻り値の型の XML ドキュメント")</span><span class="sxs-lookup"><span data-stu-id="dba43-410">![XML documentation for return types](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML documentation for return types")</span></span>

    <span data-ttu-id="dba43-411">*戻り値の型の XML ドキュメント*</span><span class="sxs-lookup"><span data-stu-id="dba43-411">*XML documentation for return types*</span></span>
5. <span data-ttu-id="dba43-412">ここで、関数を追加する呼び出しを挿入します。</span><span class="sxs-lookup"><span data-stu-id="dba43-412">Now, insert a call to add function.</span></span> <span data-ttu-id="dba43-413">JavaScript エディターが今すぐ関数のオーバー ロードをサポートしていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-413">Notice that the JavaScript editor now supports function overloads.</span></span> <span data-ttu-id="dba43-414">関数名を記述するときに、ドキュメントで指定された使用可能なオーバー ロードのいずれかを選択することができます。</span><span class="sxs-lookup"><span data-stu-id="dba43-414">When you write a function name, you will be able to select any of the available overloads specified in the documentation.</span></span>

    <span data-ttu-id="dba43-415">![XML ドキュメントのオーバー ロードを](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "オーバー ロードの XML ドキュメント")</span><span class="sxs-lookup"><span data-stu-id="dba43-415">![XML documentation for overloads](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML documentation for overloads")</span></span>

    <span data-ttu-id="dba43-416">*オーバー ロードの XML ドキュメント*</span><span class="sxs-lookup"><span data-stu-id="dba43-416">*XML documentation for overloads*</span></span>
6. <span data-ttu-id="dba43-417">開いている**GotoDefinition.js**ファイルし、検索、 **$().html()** 関数呼び出し。</span><span class="sxs-lookup"><span data-stu-id="dba43-417">Open **GotoDefinition.js** file and locate the **$().html()** function call.</span></span> <span data-ttu-id="dba43-418">カーソルを置いて**html**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-418">Locate the cursor on **html**.</span></span>
7. <span data-ttu-id="dba43-419">キーを押して**F12**定義に移動します。</span><span class="sxs-lookup"><span data-stu-id="dba43-419">Press **F12** and navigate to the definition.</span></span> <span data-ttu-id="dba43-420">アクセスしを使用せず、JavaScript コードを閲覧できますに注意してください、**検索**ツール。</span><span class="sxs-lookup"><span data-stu-id="dba43-420">Notice you can now access and browse your JavaScript code without using the **Find** tool.</span></span>
8. <span data-ttu-id="dba43-421">コード ファイルの下部にある署名ブロックの前に、jQuery の命令にカーソルを見つけます。</span><span class="sxs-lookup"><span data-stu-id="dba43-421">Locate the cursor on the jQuery instruction prior to the signature block at the bottom of the code file.</span></span> <span data-ttu-id="dba43-422">キーを押して**F12**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-422">Press **F12**.</span></span> <span data-ttu-id="dba43-423">JQuery ライブラリ ファイルに移動します。</span><span class="sxs-lookup"><span data-stu-id="dba43-423">You will navigate to the jQuery library file.</span></span> <span data-ttu-id="dba43-424">使用して、jQuery ファイル間で移動することもできますに注意してください。 **F12**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-424">Notice you can also navigate across the jQuery files using **F12**.</span></span>

    <span data-ttu-id="dba43-425">![JQuery の定義に移動する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "jQuery の定義に移動します。")</span><span class="sxs-lookup"><span data-stu-id="dba43-425">![Navigating to jQuery definitions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigating to jQuery definitions")</span></span>

    <span data-ttu-id="dba43-426">*JQuery の定義に移動します。*</span><span class="sxs-lookup"><span data-stu-id="dba43-426">*Navigating to jQuery definitions*</span></span>

> [!NOTE]
> <span data-ttu-id="dba43-427">GotoDefinition.js ファイルを保存する前に構文エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="dba43-427">Make sure that GotoDefinition.js has no syntax errors before saving the file.</span></span>


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a><span data-ttu-id="dba43-428">手順 4: バンドルと縮小</span><span class="sxs-lookup"><span data-stu-id="dba43-428">Exercise 4: Bundling and Minification</span></span>

<span data-ttu-id="dba43-429">何回か、web サイトは 1 つ以上の JavaScript または CSS ファイルか。</span><span class="sxs-lookup"><span data-stu-id="dba43-429">How many times do your websites include more than one JavaScript or CSS file?</span></span> <span data-ttu-id="dba43-430">これは、バンドルと縮小に役立つファイル サイズを小さくし、高速で実行するサイトをごく一般的なシナリオです。</span><span class="sxs-lookup"><span data-stu-id="dba43-430">This is a very common scenario where bundling and minification can help to reduce the file size and make the site perform faster.</span></span> <span data-ttu-id="dba43-431">ASP.NET 4.5 の新機能でバンドルは、JS または CSS ファイルのセットを 1 つの要素にパックし、(必須ではない空白を削除する、コメントを削除する、識別子の削減) コンテンツを縮小してサイズを縮小します。</span><span class="sxs-lookup"><span data-stu-id="dba43-431">The new bundling feature in ASP.NET 4.5 packs a set of JS or CSS files into a single element, and reduces its size by minifying the content ( i.e. removing not required blank spaces, removing comments, reducing identifiers ).</span></span>

<span data-ttu-id="dba43-432">プロセスは (たとえば IE、Mozilla など)、ユーザー エージェントを識別して、そのため、(たとえば、削除するとか色々 な処理は Mozilla の特定のユーザーのブラウザーを対象とすることによって、圧縮が向上できるように、実行時に実行はバンドルと縮小 ASP.NET 4.5 でときに、要求元が IE)。</span><span class="sxs-lookup"><span data-stu-id="dba43-432">Bundling and minification in ASP.NET 4.5 is performed at runtime, so that the process can identify the user agent (for example IE, Mozilla, etc), and thus, improve the compression by targeting the user browser (for instance, removing stuff that is Mozilla specific when the request comes from IE).</span></span>

<span data-ttu-id="dba43-433">この演習では、有効にして ASP.NET 4.5 でのバンドルと縮小のさまざまな種類を使用する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="dba43-433">In this exercise, you will learn how to enable and use the different types of bundling and minification in ASP.NET 4.5.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a><span data-ttu-id="dba43-434">タスク 1 - バンドルをインストールし、NuGet からパッケージを縮小</span><span class="sxs-lookup"><span data-stu-id="dba43-434">Task 1 - Installing the Bundling and Minification Package from NuGet</span></span>

1. <span data-ttu-id="dba43-435">これを開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**ソリューション、 **Source\WhatsNewASPNET**このラボのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="dba43-435">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dba43-436">NuGet パッケージ マネージャー コンソールを開きます。</span><span class="sxs-lookup"><span data-stu-id="dba43-436">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="dba43-437">これを行うには、メニューを使用して**ビュー** | **その他の Windows** | **パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-437">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>

    <span data-ttu-id="dba43-438">![開くパッケージ マネージャーの file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "パッケージ マネージャー コンソールを開く")</span><span class="sxs-lookup"><span data-stu-id="dba43-438">![Opening the package manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Opening the package manager console")</span></span>

    <span data-ttu-id="dba43-439">*パッケージ マネージャー コンソールを開く*</span><span class="sxs-lookup"><span data-stu-id="dba43-439">*Opening the package manager console*</span></span>
3. <span data-ttu-id="dba43-440">**パッケージ マネージャー コンソールで、** 型**Install-package Microsoft.Web.Optimization**キーを押します**ENTER**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-440">In the **Package Manager Console,** type **Install-Package Microsoft.Web.Optimization** and press **ENTER**.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a><span data-ttu-id="dba43-441">タスク 2 - 既定のバンドル</span><span class="sxs-lookup"><span data-stu-id="dba43-441">Task 2 - Default Bundles</span></span>

<span data-ttu-id="dba43-442">既定のバンドルはバンドルと縮小を使用する最も簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="dba43-442">The simplest way to use bundling and minification is to enable the default bundles.</span></span> <span data-ttu-id="dba43-443">このメソッドでは、規則を使用して、フォルダー内の JS、CSS ファイルのバンドルと縮小されたバージョンを参照することができます。</span><span class="sxs-lookup"><span data-stu-id="dba43-443">This method uses conventions to let you reference the bundled and minified version for the JS and CSS files in a folder.</span></span>

<span data-ttu-id="dba43-444">このタスクでは、有効にしてバンドルおよび縮小された JS、CSS ファイルを参照して、結果の出力を表示する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="dba43-444">In this task, you will learn how to enable and reference the bundled and minified JS and CSS files and view the resulting output.</span></span>

1. <span data-ttu-id="dba43-445">これを開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**ソリューション、 **Source\WhatsNewASPNET**このラボのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="dba43-445">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dba43-446">**ソリューション エクスプ ローラー**、展開、**スタイル**、 **Scripts\custom**と**Scripts\bundle**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="dba43-446">In the **Solution Explorer**, expand the **Styles**, **Scripts\custom** and **Scripts\bundle** folders.</span></span>

    <span data-ttu-id="dba43-447">アプリケーションが 1 つ以上の CSS と JS ファイルを使用することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-447">Notice that the application is using more than one CSS and JS file.</span></span>

    <span data-ttu-id="dba43-448">![アプリケーションの複数のスタイル シートおよび JavaScript ファイル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "アプリケーションで複数のスタイル シートおよび JavaScript ファイル")</span><span class="sxs-lookup"><span data-stu-id="dba43-448">![Multiple Stylesheets and JavaScript files in the application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Multiple Stylesheets and JavaScript files in the application")</span></span>

    <span data-ttu-id="dba43-449">*アプリケーションで複数のスタイル シートおよび JavaScript ファイル*</span><span class="sxs-lookup"><span data-stu-id="dba43-449">*Multiple Stylesheets and JavaScript files in the application*</span></span>
3. <span data-ttu-id="dba43-450">開く、 **Global.asax.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="dba43-450">Open the **Global.asax.cs** file.</span></span>

    <span data-ttu-id="dba43-451">注意して、新しい**Microsoft.Web.Optimization**名前空間が、ファイルの先頭にコメント アウトされています。</span><span class="sxs-lookup"><span data-stu-id="dba43-451">Notice that the new **Microsoft.Web.Optimization** namespace is commented out at the beginning of the file.</span></span> <span data-ttu-id="dba43-452">使用して、コメントを解除します。 バンドルと縮小の機能をインクルードします。</span><span class="sxs-lookup"><span data-stu-id="dba43-452">Uncomment the using directive to include the bundling and minification features.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. <span data-ttu-id="dba43-453">検索、**アプリケーション\_開始**メソッド。</span><span class="sxs-lookup"><span data-stu-id="dba43-453">Locate the **Application\_Start** method.</span></span>

    <span data-ttu-id="dba43-454">このメソッドは、次のスニペットに示すように EnableDefaultBundles 呼び出しをコメント解除します。</span><span class="sxs-lookup"><span data-stu-id="dba43-454">In this method, uncomment the EnableDefaultBundles call as shown in the snippet below.</span></span> <span data-ttu-id="dba43-455">これにより、そのフォルダーのパスを使用してフォルダー内の CSS ファイルのバンドル コレクションを参照するだけでなく、 &quot;CSS&quot;または&quot;JS&quot;サフィックス。</span><span class="sxs-lookup"><span data-stu-id="dba43-455">This enables us to reference a bundled collection of CSS files in a folder by using the path to that folder, plus the &quot;CSS&quot; or the &quot;JS&quot; suffix.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. <span data-ttu-id="dba43-456">開く、 **Optimization.aspx**ファイルし、のコンテンツ コントロールを探して**HeadContent**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-456">Open the **Optimization.aspx** file and locate the content control for **HeadContent**.</span></span>

    <span data-ttu-id="dba43-457">CSS ファイルと 1 つの参照されているタグがある JS ファイルに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dba43-457">Notice the CSS files and the JS files have a single referenced tag.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > <span data-ttu-id="dba43-458">このコードは、デモの目的では。</span><span class="sxs-lookup"><span data-stu-id="dba43-458">This code is for demo purposes.</span></span> <span data-ttu-id="dba43-459">理想的には、Site.Master ファイル内のバンドルを参照します。</span><span class="sxs-lookup"><span data-stu-id="dba43-459">Ideally, you will reference the bundles in the Site.Master file.</span></span> <span data-ttu-id="dba43-460">このサンプル コードで紹介するバンドル ファイルの一部もによって参照されている、Site.Master ファイル冗長この最後の参照を作成します。</span><span class="sxs-lookup"><span data-stu-id="dba43-460">In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.</span></span>
6. <span data-ttu-id="dba43-461">リンクがバンドル内の規則を使用していることに注意してください、 **href**スタイルと Scripts\custom からすべての CSS または JS ファイルを取得する属性フォルダーそれぞれします。</span><span class="sxs-lookup"><span data-stu-id="dba43-461">Notice that the links are using the bundling conventions in the **href** attribute to get all the CSS or JS files from the Styles and Scripts\custom folder respectively.</span></span>

    <span data-ttu-id="dba43-462">パスを使用することができます**スクリプト/カスタム/JS**バンドルし、縮小内のすべての JS ファイルに次のような**スクリプト/カスタム**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="dba43-462">You can use the path **Scripts/custom/JS** as shown below to bundle and minify all the JS files inside a **Scripts/custom** folder.</span></span> <span data-ttu-id="dba43-463">これは、既定のバンドルと既定の動作です。</span><span class="sxs-lookup"><span data-stu-id="dba43-463">This is the default behavior with the default bundles.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. <span data-ttu-id="dba43-464">開く、 **Styles\Site.css**ファイル。</span><span class="sxs-lookup"><span data-stu-id="dba43-464">Open the **Styles\Site.css** file.</span></span>

    <span data-ttu-id="dba43-465">元の CSS ファイルには、インデントされたコード、空白、およびファイルを拡大するコメントが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="dba43-465">Notice that the original CSS file contains indented code, blank spaces and comments that enlarge the file.</span></span> <span data-ttu-id="dba43-466">(また、JavaScript ファイルが含まれています空白とコメント)。</span><span class="sxs-lookup"><span data-stu-id="dba43-466">(Also the JavaScript file contains blank spaces and comments).</span></span>

    <span data-ttu-id="dba43-467">![スクリプト フォルダーにファイルを元の CSS のいずれかの](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "スクリプト フォルダーにファイルを元の CSS のいずれか")</span><span class="sxs-lookup"><span data-stu-id="dba43-467">![One of the original CSS files in the Scripts folder](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "One of the original CSS files in the Scripts folder")</span></span>

    <span data-ttu-id="dba43-468">*Scripts フォルダーでは、元の CSS ファイルのいずれか*</span><span class="sxs-lookup"><span data-stu-id="dba43-468">*One of the original CSS files in the Scripts folder*</span></span>
8. <span data-ttu-id="dba43-469">キーを押して**F5**アプリケーションを実行しに移動する、**最適化**ページ。</span><span class="sxs-lookup"><span data-stu-id="dba43-469">Press **F5** to run the application and navigate to the **Optimization** page.</span></span>
9. <span data-ttu-id="dba43-470">をクリックして、 **CSS バンドル**へのリンクをダウンロードして、ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="dba43-470">Click on the **CSS Bundle** link to download and open the file.</span></span>

    <span data-ttu-id="dba43-471">縮小されたバンドルされているファイルを確認します。</span><span class="sxs-lookup"><span data-stu-id="dba43-471">Check out the minified bundled file.</span></span> <span data-ttu-id="dba43-472">表示されます、空白、コメントおよびインデント文字をすべて削除されていること、サイズの小さいファイルを生成します。</span><span class="sxs-lookup"><span data-stu-id="dba43-472">You will notice that all the blank spaces, comments and indentation characters have been removed, generating a smaller file.</span></span>

    <span data-ttu-id="dba43-473">![CSS ファイルをバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "バンドルされている CSS ファイル")</span><span class="sxs-lookup"><span data-stu-id="dba43-473">![Bundled CSS files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Bundled CSS files")</span></span>

    <span data-ttu-id="dba43-474">*バンドルの CSS ファイル*</span><span class="sxs-lookup"><span data-stu-id="dba43-474">*Bundled CSS files*</span></span>
10. <span data-ttu-id="dba43-475">クリックし、 **JS バンドル**バンドルされている JavaScript ファイルを開くためのリンク。</span><span class="sxs-lookup"><span data-stu-id="dba43-475">Now click the **JS Bundle** link to open the JavaScript bundled file.</span></span> <span data-ttu-id="dba43-476">エクスプ ローラーの警告は無視して構いません。</span><span class="sxs-lookup"><span data-stu-id="dba43-476">You can safely disregard the explorer warning.</span></span> <span data-ttu-id="dba43-477">JavaScript ファイルに注意してください、**カスタム**フォルダーもバンドルおよび縮小します。</span><span class="sxs-lookup"><span data-stu-id="dba43-477">Notice the JavaScript files under the **custom** folder are also bundled and minified.</span></span>

    <span data-ttu-id="dba43-478">![JavaScript ファイルをバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "バンドルされている JavaScript ファイル")</span><span class="sxs-lookup"><span data-stu-id="dba43-478">![Bundled JavaScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Bundled JavaScript files")</span></span>

    <span data-ttu-id="dba43-479">*バンドルの JavaScript ファイル*</span><span class="sxs-lookup"><span data-stu-id="dba43-479">*Bundled JavaScript files*</span></span>

    <span data-ttu-id="dba43-480">CSS または JS ファイルの圧縮を有効にするは、ASP.NET の以前のバージョンではるかに複雑ですが。</span><span class="sxs-lookup"><span data-stu-id="dba43-480">Enabling compression for CSS or JS files was much more complicated in previous ASP.NET version.</span></span> <span data-ttu-id="dba43-481">ここで説明したようにするだけ 1 つの行を追加、 *Global.asax*ファイルのバンドル化、有効にして、サイトからバンドルのファイルを参照します。</span><span class="sxs-lookup"><span data-stu-id="dba43-481">Now, as you have seen, you just need to add one line in the *Global.asax* file to enable bundling, and then reference the bundled files from your site.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a><span data-ttu-id="dba43-482">タスク 3 - 静的バンドル</span><span class="sxs-lookup"><span data-stu-id="dba43-482">Task 3 - Static Bundles</span></span>

<span data-ttu-id="dba43-483">バンドルの静的なアプローチでは、バンドル、参照および使用される縮小メソッドへのファイルのセットをカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="dba43-483">The static bundle approach allows you to customize the set of files to bundle, the reference and the minification method that will be used.</span></span>

<span data-ttu-id="dba43-484">このタスクでは、バンドルし、縮小するファイルの特定のセットを定義する静的バンドルを構成します。</span><span class="sxs-lookup"><span data-stu-id="dba43-484">In this task, you will configure a static bundle to define a specific set of files to bundle and minify.</span></span>

1. <span data-ttu-id="dba43-485">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="dba43-485">Close the browser.</span></span>
2. <span data-ttu-id="dba43-486">開く、 **Global.asax.cs**ファイルし、検索、**アプリケーション\_開始**メソッド。</span><span class="sxs-lookup"><span data-stu-id="dba43-486">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
3. <span data-ttu-id="dba43-487">次のコードに示すように、静的バンドル コードをコメント解除します。</span><span class="sxs-lookup"><span data-stu-id="dba43-487">Uncomment the static bundle code as shown in the code below.</span></span>

    <span data-ttu-id="dba43-488">参照される静的バンドルを定義する、 &quot; **~/StaticBundle** &quot;仮想パスと使用**JsMinify**で指定されたすべてのファイルの縮小を**AddFile**メソッド。</span><span class="sxs-lookup"><span data-stu-id="dba43-488">You are defining a static bundle that will be referenced with the &quot;**~/StaticBundle**&quot; virtual path and use **JsMinify** for minification of all the specified files with the **AddFile** method.</span></span> <span data-ttu-id="dba43-489">最後に、静的バンドルを追加する場合は、 **BundleTable**有効化します。</span><span class="sxs-lookup"><span data-stu-id="dba43-489">Finally, you are adding the static bundle to the **BundleTable** and enabling it.</span></span>

    <span data-ttu-id="dba43-490">ファイルが、同じ場所にいないにあることに注意してください。これは、既定のバンドルをもう 1 つの利点です。</span><span class="sxs-lookup"><span data-stu-id="dba43-490">Notice that the files are not located in the same place; this is another advantage over the default bundling.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. <span data-ttu-id="dba43-491">開く、 **Optimization.aspx**ファイル。</span><span class="sxs-lookup"><span data-stu-id="dba43-491">Open the **Optimization.aspx** file.</span></span>

    <span data-ttu-id="dba43-492">注意へのリンク**静的 JS バンドル**Global.asax.cs ファイルの静的なバンドルの構成時に宣言されているパスを使用して: **/StaticBundle**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-492">Notice that the link to **Static JS Bundle** is using the path you have declared when you configured the static bundle in the Global.asax.cs file: **/StaticBundle**.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. <span data-ttu-id="dba43-493">キーを押して**F5**アプリケーションを実行しに移動する、**最適化**ページ。</span><span class="sxs-lookup"><span data-stu-id="dba43-493">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
6. <span data-ttu-id="dba43-494">をクリックして、**静的 JS バンドル**ファイルを開くためのリンク。</span><span class="sxs-lookup"><span data-stu-id="dba43-494">Click on the **Static JS Bundle** link to open the file.</span></span>

    <span data-ttu-id="dba43-495">パスの下の静的なバンドル ファイルで構成されているすべての JavaScript ファイルの出力は、縮小されたバンドルの JavaScript ファイル&quot;/StaticBundle&quot;します。</span><span class="sxs-lookup"><span data-stu-id="dba43-495">Notice that the minified bundled JavaScript file is the output for all the JavaScript files configured in the static bundle file under the path &quot;/StaticBundle&quot;.</span></span>

    <span data-ttu-id="dba43-496">![静的な JavaScript ファイルのバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "静的 JavaScript ファイルのバンドル")</span><span class="sxs-lookup"><span data-stu-id="dba43-496">![Static JavaScript files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Static JavaScript files bundle")</span></span>

    <span data-ttu-id="dba43-497">*静的な JavaScript ファイルをバンドルします。*</span><span class="sxs-lookup"><span data-stu-id="dba43-497">*Static JavaScript files bundle*</span></span>
7. <span data-ttu-id="dba43-498">ブラウザーを閉じて、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="dba43-498">Close the browser and return to Visual Studio.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a><span data-ttu-id="dba43-499">タスク 4 - 動的フォルダー バンドル</span><span class="sxs-lookup"><span data-stu-id="dba43-499">Task 4 - Dynamic Folder Bundles</span></span>

<span data-ttu-id="dba43-500">このタスクでは、動的フォルダー バンドルを構成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="dba43-500">In this task, you will learn how to configure dynamic folder bundles.</span></span> <span data-ttu-id="dba43-501">動的バンドルの電源は、こと、JavaScript にコンパイルされる言語で、静的な JavaScript だけでなく、他のファイルを含めるし、そのため、バンドルが実行される前に、何らかの処理が必要ですが。</span><span class="sxs-lookup"><span data-stu-id="dba43-501">The power of dynamic bundling is that you can include static JavaScript, as well as other files in languages that compiles into JavaScript, and thus, require some processing before the bundling is executed.</span></span>

<span data-ttu-id="dba43-502">この例を使用する方法について説明します、 **DynamicFolderBundle** CofeeScript で記述されたファイルの動的なバンドルを作成するクラス。</span><span class="sxs-lookup"><span data-stu-id="dba43-502">In this example, you will learn how to use the **DynamicFolderBundle** class to create a dynamic bundle for files written in CofeeScript.</span></span> <span data-ttu-id="dba43-503">CofeeScript は、JavaScript にコンパイルし、JavaScript コードの記述が JavaScript の簡潔さと読みやすさを向上させるの単純な構文を提供するプログラミング言語です。</span><span class="sxs-lookup"><span data-stu-id="dba43-503">CofeeScript is a programming language that compiles into JavaScript and provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability.</span></span>

1. <span data-ttu-id="dba43-504">開く、 **Global.asax.cs**ファイルし、検索、**アプリケーション\_開始**メソッド。</span><span class="sxs-lookup"><span data-stu-id="dba43-504">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
2. <span data-ttu-id="dba43-505">次のコードに示すように動的なバンドル コードをコメント解除します。</span><span class="sxs-lookup"><span data-stu-id="dba43-505">Uncomment the dynamic bundle code as shown in the code below.</span></span>

    <span data-ttu-id="dba43-506">使用する動的フォルダー バンドルを定義する、 **CoffeeMinify**でファイルにのみ適用されるカスタムの縮小プロセッサ、 &quot; \***.coffee** &quot;拡張機能 (CoffeeScript ファイル)。</span><span class="sxs-lookup"><span data-stu-id="dba43-506">You are defining a dynamic folder bundle that will use the **CoffeeMinify** custom minification processor that will only apply to the files with the &quot;**.coffee**&quot; extension (CoffeeScript files).</span></span> <span data-ttu-id="dba43-507">通知など、フォルダー内にバンドルするファイルを選択し、検索パターンを使用することができます '\**.coffee '。</span><span class="sxs-lookup"><span data-stu-id="dba43-507">Notice that you can use a search pattern to select the files to bundle within a folder, like '\*.coffee'.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. <span data-ttu-id="dba43-508">NuGet パッケージ マネージャー コンソールを開きます。</span><span class="sxs-lookup"><span data-stu-id="dba43-508">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="dba43-509">これを行うには、メニューを使用して**ビュー** | **その他の Windows** | **パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-509">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>
4. <span data-ttu-id="dba43-510">**パッケージ マネージャー コンソールで、** 型**Install-package CoffeeSharp**キーを押します**ENTER**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-510">In the **Package Manager Console,** type **Install-Package CoffeeSharp** and press **ENTER**.</span></span>
5. <span data-ttu-id="dba43-511">をクリックして、 **すべてのファイル**ボタン、**ソリューション エクスプ ローラー**ウィンドウ</span><span class="sxs-lookup"><span data-stu-id="dba43-511">Click the **Show All Files** button in the **Solution Explorer** window</span></span>

    <span data-ttu-id="dba43-512">![すべてのファイルを示す](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "すべてのファイルを表示")</span><span class="sxs-lookup"><span data-stu-id="dba43-512">![Showing all files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Showing all files")</span></span>

    <span data-ttu-id="dba43-513">*すべてのファイルを表示*</span><span class="sxs-lookup"><span data-stu-id="dba43-513">*Showing all files*</span></span>
6. <span data-ttu-id="dba43-514">右クリックして、 **CoffeeMinify.cs**ファイル、**ソリューション エクスプ ローラー**選択**プロジェクトに含める**</span><span class="sxs-lookup"><span data-stu-id="dba43-514">Right click the **CoffeeMinify.cs** file in the **Solution Explorer** and select **Include in Project**</span></span>

    <span data-ttu-id="dba43-515">![CoffeeMinify.cs ファイルをプロジェクトに含める](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs ファイルをプロジェクトに含める")</span><span class="sxs-lookup"><span data-stu-id="dba43-515">![Include the CoffeeMinify.cs file in the project](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Include the CoffeeMinify.cs file in the project")</span></span>

    <span data-ttu-id="dba43-516">*CoffeeMinify.cs ファイルをプロジェクトに含める*</span><span class="sxs-lookup"><span data-stu-id="dba43-516">*Include the CoffeeMinify.cs file in the project*</span></span>
7. <span data-ttu-id="dba43-517">開く、 **CoffeeMinify.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="dba43-517">Open the **CoffeeMinify.cs** file.</span></span>

    <span data-ttu-id="dba43-518">このクラスは、JavaScript の出力、CoffeeScript コードのコンパイルから結果を縮小する JsMinify から継承します。</span><span class="sxs-lookup"><span data-stu-id="dba43-518">This class inherits from JsMinify to minify the JavaScript output resulting from the CoffeeScript code compilation.</span></span> <span data-ttu-id="dba43-519">最初に、JavaScript コードを生成する CoffeeScript コンパイラを呼び出すし、結果のコードを縮小する JsMinify.Process メソッドに送信、し。</span><span class="sxs-lookup"><span data-stu-id="dba43-519">It calls the CoffeeScript compiler to generate the JavaScript code first, and then it sends it to the JsMinify.Process method to minify the resulting code.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. <span data-ttu-id="dba43-520">開く、 **Script1.coffee**と**Script2.coffee**ファイルから、**スクリプト/バンドル**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="dba43-520">Open the **Script1.coffee** and **Script2.coffee** files from the **Scripts/bundle** folder.</span></span>

    <span data-ttu-id="dba43-521">これらのファイルは、CoffeeMinify クラスを使用してバンドルを実行中にコンパイルする CoffeScript コードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="dba43-521">These files will include the CoffeScript code to be compiled while performing the bundling with the CoffeeMinify class.</span></span>

    <span data-ttu-id="dba43-522">わかりやすくするための目的で提供されている CoffeeScript ファイルは CoffeeScript のサンプル コードのみを含むです。</span><span class="sxs-lookup"><span data-stu-id="dba43-522">For simplicity purposes, the CoffeeScript files provided are only including CoffeeScript sample code.</span></span> <span data-ttu-id="dba43-523">コメントは、JsMinify プロセスによって除外されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-523">The comments are excluded by the JsMinify process.</span></span>

    <span data-ttu-id="dba43-524">![CoffeeScript ファイル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript ファイル")</span><span class="sxs-lookup"><span data-stu-id="dba43-524">![CoffeeScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript files")</span></span>

    <span data-ttu-id="dba43-525">*CoffeeScript ファイル*</span><span class="sxs-lookup"><span data-stu-id="dba43-525">*CoffeeScript files*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dba43-526">[CofeeScript](https://github.com/jashkenas/coffeescript/)の JavaScript コードを記述、JavaScript の簡潔さと読みやすさ、強化だけでなく配列理解してパターン マッチングのようなその他の機能を追加する単純な構文を提供します。</span><span class="sxs-lookup"><span data-stu-id="dba43-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability, as well as adding other features like array comprehension and pattern matching.</span></span>
9. <span data-ttu-id="dba43-527">開く、 **Optimization.aspx**ファイルし、バンドルのリンクを探します。</span><span class="sxs-lookup"><span data-stu-id="dba43-527">Open the **Optimization.aspx** file and locate the bundle links.</span></span>

    <span data-ttu-id="dba43-528">注意してへのリンク**動的 JS バンドル**を参照して、**スクリプト/バンドル**フォルダーを使用して、 **コーヒー/** 動的フォルダー バンドルの構成されているサフィックス。</span><span class="sxs-lookup"><span data-stu-id="dba43-528">Notice that the link to **Dynamic JS Bundle** is referencing the **Scripts/bundle** folder by using the **/Coffee** suffix you configured for the dynamic folder bundle.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. <span data-ttu-id="dba43-529">キーを押して**F5**アプリケーションを実行しに移動する、**最適化**ページ。</span><span class="sxs-lookup"><span data-stu-id="dba43-529">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
11. <span data-ttu-id="dba43-530">をクリックして、**動的 JS バンドル**生成されたファイルを開くためのリンク。</span><span class="sxs-lookup"><span data-stu-id="dba43-530">Click on the **Dynamic JS Bundle** link to open the generated file.</span></span>

    <span data-ttu-id="dba43-531">このバンドルに含まれているコンテンツのみを含む通知 \***.coffee**ファイル。</span><span class="sxs-lookup"><span data-stu-id="dba43-531">Notice that the content that was included in this bundle only contains **.coffee** files.</span></span> <span data-ttu-id="dba43-532">CoffeeScript コードが JavaScript にコンパイルされたされ、コメント アウトされた行が削除されたこともわかります。</span><span class="sxs-lookup"><span data-stu-id="dba43-532">You can also see that the CoffeeScript code was compiled to JavaScript and the commented-out lines has been removed.</span></span>

    <span data-ttu-id="dba43-533">![動的な JS ファイルをバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "動的 JS ファイルのバンドル")</span><span class="sxs-lookup"><span data-stu-id="dba43-533">![Dynamic JS files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Dynamic JS files bundle")</span></span>

    <span data-ttu-id="dba43-534">*動的な JS ファイルのバンドル*</span><span class="sxs-lookup"><span data-stu-id="dba43-534">*Dynamic JS files bundle*</span></span>

> [!NOTE]
> <span data-ttu-id="dba43-535">また、Windows Azure Web サイトの次に、このアプリケーションを展開できます[付録 b: 発行 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)します。</span><span class="sxs-lookup"><span data-stu-id="dba43-535">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="dba43-536">まとめ</span><span class="sxs-lookup"><span data-stu-id="dba43-536">Summary</span></span>

<span data-ttu-id="dba43-537">このラボでは、ASP.NET の新機能と Visual Studio 2012 での Web 開発と Visual Studio 2012 で、さまざまな機能強化の活用方法を理解するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="dba43-537">This lab helps you to understand what New in ASP.NET and Web Development in Visual Studio 2012 is and how to take advantage of the variety of enhancements in Visual Studio 2012.</span></span>

<span data-ttu-id="dba43-538">このハンズオン ラボを完了するが、Visual Studio 2012 のエディターで CSS、JavaScript と HTML の新しい機能と機能強化を使用する方法を習得します。</span><span class="sxs-lookup"><span data-stu-id="dba43-538">By completing this Hands-On Lab, you have learnt how to use the new features and improvements in Visual Studio 2012 Editors for CSS, JavaScript and HTML.</span></span> <span data-ttu-id="dba43-539">さらに、Visual Studio 2012 で組み込みのバンドルと縮小を実装する方法を習得があります。</span><span class="sxs-lookup"><span data-stu-id="dba43-539">In addition, you have learnt how Visual Studio 2012 implements built-in bundling and minification.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="dba43-540">付録 a: Installing Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="dba43-540">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="dba43-541">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="dba43-541">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="dba43-542">次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。</span><span class="sxs-lookup"><span data-stu-id="dba43-542">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="dba43-543">移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。</span><span class="sxs-lookup"><span data-stu-id="dba43-543">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="dba43-544">または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。</span><span class="sxs-lookup"><span data-stu-id="dba43-544">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="dba43-545">をクリックして**を今すぐインストール**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-545">Click on **Install Now**.</span></span> <span data-ttu-id="dba43-546">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="dba43-546">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="dba43-547">1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="dba43-547">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="dba43-548">![Visual Studio Express のインストール](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="dba43-548">![Install Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="dba43-549">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="dba43-549">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="dba43-550">すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="dba43-550">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    <span data-ttu-id="dba43-552">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="dba43-552">*Accepting the license terms*</span></span>
5. <span data-ttu-id="dba43-553">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="dba43-553">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    <span data-ttu-id="dba43-555">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="dba43-555">*Installation progress*</span></span>
6. <span data-ttu-id="dba43-556">インストールが完了したら、クリックして**完了**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-556">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    <span data-ttu-id="dba43-558">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="dba43-558">*Installation completed*</span></span>
7. <span data-ttu-id="dba43-559">クリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="dba43-559">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="dba43-560">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="dba43-560">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web のタイル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    <span data-ttu-id="dba43-562">*VS Express for Web のタイル*</span><span class="sxs-lookup"><span data-stu-id="dba43-562">*VS Express for Web tile*</span></span>

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="dba43-563">付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="dba43-563">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="dba43-564">この付録では、Windows Azure 管理ポータルから新しい web サイトを作成して Windows Azure によって提供される、Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを発行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="dba43-564">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="dba43-565">タスク 1 - 新しい Web サイトを作成する、Windows から Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dba43-565">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="dba43-566">移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="dba43-566">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dba43-567">Windows Azure を無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増加に応じてスケールできます。</span><span class="sxs-lookup"><span data-stu-id="dba43-567">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="dba43-568">サインアップする[ここ](http://aka.ms/aspnet-hol-azure)します。</span><span class="sxs-lookup"><span data-stu-id="dba43-568">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="dba43-569">![Windows Azure ポータルにログオン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Windows Azure ポータルにログオン")</span><span class="sxs-lookup"><span data-stu-id="dba43-569">![Log on to Windows Azure portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="dba43-570">*Windows Azure 管理ポータルにログオン*</span><span class="sxs-lookup"><span data-stu-id="dba43-570">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="dba43-571">クリックして**新規**コマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="dba43-571">Click **New** on the command bar.</span></span>

    <span data-ttu-id="dba43-572">![新しい Web サイトを作成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="dba43-572">![Creating a new Web Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="dba43-573">*新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="dba43-573">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="dba43-574">クリックして**コンピューティング** | **Web サイト**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-574">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="dba43-575">選び**簡易作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="dba43-575">Then select **Quick Create** option.</span></span> <span data-ttu-id="dba43-576">新しい web サイトの URL を指定し、をクリックして**Web サイトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="dba43-576">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dba43-577">Windows Azure の Web サイトには、制御および管理できる、クラウドで実行されている web アプリケーション、ホストです。</span><span class="sxs-lookup"><span data-stu-id="dba43-577">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="dba43-578">簡易作成 オプションを使用すると、完成した web アプリケーションを Windows Azure の Web サイトから、ポータル外からデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="dba43-578">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="dba43-579">データベースを設定するための手順は含まれません。</span><span class="sxs-lookup"><span data-stu-id="dba43-579">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="dba43-580">![簡易作成を使用して新しい Web サイトを作成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "簡易作成を使用して新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="dba43-580">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="dba43-581">*簡易作成を使用して新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="dba43-581">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="dba43-582">新しいまで待つ**Web サイト**が作成されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-582">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="dba43-583">Web サイトを作成した後は、下のリンクをクリックして、 **URL**列。</span><span class="sxs-lookup"><span data-stu-id="dba43-583">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="dba43-584">新しい Web サイトが動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="dba43-584">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="dba43-585">![新しい web サイトを参照](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "新しい web サイトを参照")</span><span class="sxs-lookup"><span data-stu-id="dba43-585">![Browsing to the new web site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="dba43-586">*新しい web サイトを参照*</span><span class="sxs-lookup"><span data-stu-id="dba43-586">*Browsing to the new web site*</span></span>

    <span data-ttu-id="dba43-587">![Web サイトが実行されている](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "実行している Web サイト")</span><span class="sxs-lookup"><span data-stu-id="dba43-587">![Web site running](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Web site running")</span></span>

    <span data-ttu-id="dba43-588">*実行している web サイト*</span><span class="sxs-lookup"><span data-stu-id="dba43-588">*Web site running*</span></span>
6. <span data-ttu-id="dba43-589">ポータルに戻り、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="dba43-589">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="dba43-590">![Web サイトの管理ページを開く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "web サイトの管理ページを開く")</span><span class="sxs-lookup"><span data-stu-id="dba43-590">![Opening the web site management pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="dba43-591">*Web サイトの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="dba43-591">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="dba43-592">**ダッシュ ボード**ページの**概要**セクションで、、**発行プロファイルのダウンロード**リンク。</span><span class="sxs-lookup"><span data-stu-id="dba43-592">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dba43-593">*発行プロファイル*すべてのパブリケーションを有効になっているメソッドごとに Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="dba43-593">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="dba43-594">発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベースの文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="dba43-594">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="dba43-595">**Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートと、発行プロファイルのこれらのプログラムの構成を自動化するにはWindows Azure の web サイトへの web アプリケーションを発行しています。</span><span class="sxs-lookup"><span data-stu-id="dba43-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="dba43-596">![発行プロファイルのダウンロード web サイト](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "発行プロファイルの web サイトのダウンロード")</span><span class="sxs-lookup"><span data-stu-id="dba43-596">![Downloading the web site publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="dba43-597">*発行プロファイルの Web サイトのダウンロード*</span><span class="sxs-lookup"><span data-stu-id="dba43-597">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="dba43-598">既知の場所に発行プロファイル ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="dba43-598">Download the publish profile file to a known location.</span></span> <span data-ttu-id="dba43-599">さらにこの演習ではこのファイルを使用して、Visual Studio から Windows Azure Web サイトに web アプリケーションを発行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-599">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="dba43-600">![発行プロファイル ファイルを保存する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "発行プロファイルを保存しています")</span><span class="sxs-lookup"><span data-stu-id="dba43-600">![Saving the publish profile file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Saving the publish profile")</span></span>

    <span data-ttu-id="dba43-601">*発行プロファイル ファイルを保存しています*</span><span class="sxs-lookup"><span data-stu-id="dba43-601">*Saving the publish profile file*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="dba43-602">タスク 2 - データベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="dba43-602">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="dba43-603">アプリケーションが SQL Server を使用する場合のデータベースは SQL Database サーバーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dba43-603">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="dba43-604">SQL Server を使用しない単純なアプリケーションをデプロイする場合は、このタスクをスキップする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="dba43-604">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="dba43-605">SQL Database サーバーは、アプリケーション データベースを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dba43-605">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="dba43-606">Windows Azure 管理ポータル内のサブスクリプションから SQL Database サーバーを表示する**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-606">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="dba43-607">使用して 1 つを作成するには作成したサーバーがない、**追加**コマンド バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="dba43-607">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="dba43-608">メモ、**サーバー名および URL、管理者のログイン名とパスワード**次のタスクで使用すると、します。</span><span class="sxs-lookup"><span data-stu-id="dba43-608">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="dba43-609">データベースを作成しない、まだ、後の段階でそれが作成されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-609">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="dba43-610">![SQL データベース サーバーのダッシュ ボード](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL データベース サーバーのダッシュ ボード")</span><span class="sxs-lookup"><span data-stu-id="dba43-610">![SQL Database Server Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="dba43-611">*SQL データベース サーバーのダッシュ ボード*</span><span class="sxs-lookup"><span data-stu-id="dba43-611">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="dba43-612">次のタスクでは、サーバーの一覧で、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-612">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="dba43-613">次のようにクリックします**構成**、から IP アドレスを選択して**現在のクライアント IP アドレス**で貼り付けます、**開始 IP アドレス**と**終了 IP アドレス。** テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="dba43-613">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes.</span></span> <span data-ttu-id="dba43-614">ルールの名前を入力し、クリックして、 ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png)ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="dba43-614">Enter a name for the rule and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) button.</span></span>

    ![クライアント IP アドレスを追加します。](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    <span data-ttu-id="dba43-616">*クライアント IP アドレスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="dba43-616">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="dba43-617">1 回、**クライアント IP アドレス**が許可された IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="dba43-617">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![変更を確認します。](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    <span data-ttu-id="dba43-619">*変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="dba43-619">*Confirm Changes*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="dba43-620">タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="dba43-620">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="dba43-621">ASP.NET MVC 4 のソリューションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="dba43-621">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="dba43-622">**ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-622">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="dba43-623">![アプリケーションの発行](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="dba43-623">![Publishing the Application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publishing the Application")</span></span>

    <span data-ttu-id="dba43-624">*Web サイトの発行*</span><span class="sxs-lookup"><span data-stu-id="dba43-624">*Publishing the web site*</span></span>
2. <span data-ttu-id="dba43-625">最初のタスクで保存した発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="dba43-625">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="dba43-626">![発行プロファイルをインポートする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "発行プロファイルをインポートします。")</span><span class="sxs-lookup"><span data-stu-id="dba43-626">![Importing the publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importing the publish profile")</span></span>

    <span data-ttu-id="dba43-627">*発行プロファイルのインポート*</span><span class="sxs-lookup"><span data-stu-id="dba43-627">*Importing publish profile*</span></span>
3. <span data-ttu-id="dba43-628">クリックして**接続の検証**です。</span><span class="sxs-lookup"><span data-stu-id="dba43-628">Click **Validate Connection**.</span></span> <span data-ttu-id="dba43-629">検証が完了したら、クリックして**次**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-629">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dba43-630">接続の検証ボタンの横に表示される緑のチェック マークが表示されたら、検証が完了しました。</span><span class="sxs-lookup"><span data-stu-id="dba43-630">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="dba43-631">![接続の検証](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "接続の検証")</span><span class="sxs-lookup"><span data-stu-id="dba43-631">![Validating connection](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validating connection")</span></span>

    <span data-ttu-id="dba43-632">*接続の検証*</span><span class="sxs-lookup"><span data-stu-id="dba43-632">*Validating connection*</span></span>
4. <span data-ttu-id="dba43-633">**設定**ページの**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックします (つまり**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="dba43-633">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="dba43-634">![Web 配置の構成](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web 配置の構成")</span><span class="sxs-lookup"><span data-stu-id="dba43-634">![Web deploy configuration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy configuration")</span></span>

    <span data-ttu-id="dba43-635">*Web 配置の構成*</span><span class="sxs-lookup"><span data-stu-id="dba43-635">*Web deploy configuration*</span></span>
5. <span data-ttu-id="dba43-636">データベース接続を次のように構成します。</span><span class="sxs-lookup"><span data-stu-id="dba43-636">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="dba43-637">**サーバー名**SQL Database サーバーの URL を使用して、入力、 *tcp:* プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="dba43-637">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="dba43-638">**ユーザー名**サーバー管理者ログイン名を入力します。</span><span class="sxs-lookup"><span data-stu-id="dba43-638">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="dba43-639">**パスワード**サーバー管理者ログイン パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="dba43-639">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="dba43-640">たとえば、新しいデータベース名を入力: *MVC4SampleDB*します。</span><span class="sxs-lookup"><span data-stu-id="dba43-640">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="dba43-641">![ターゲットの接続文字列を構成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "ターゲットの接続文字列を構成します。")</span><span class="sxs-lookup"><span data-stu-id="dba43-641">![Configuring destination connection string](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="dba43-642">*ターゲットの接続文字列を構成します。*</span><span class="sxs-lookup"><span data-stu-id="dba43-642">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="dba43-643">次に、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="dba43-643">Then click **OK**.</span></span> <span data-ttu-id="dba43-644">データベースのクリックを作成するように求められたら**はい**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-644">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="dba43-645">![データベースを作成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "データベース文字列を作成します。")</span><span class="sxs-lookup"><span data-stu-id="dba43-645">![Creating the database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creating the database string")</span></span>

    <span data-ttu-id="dba43-646">*データベースの作成*</span><span class="sxs-lookup"><span data-stu-id="dba43-646">*Creating the database*</span></span>
7. <span data-ttu-id="dba43-647">Windows azure SQL Database への接続に使用する接続文字列は、接続の既定のテキスト ボックス内に表示されます。</span><span class="sxs-lookup"><span data-stu-id="dba43-647">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="dba43-648">その後、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="dba43-648">Then click **Next**.</span></span>

    <span data-ttu-id="dba43-649">![SQL データベースを指す接続文字列](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "SQL データベースを指す接続文字列")</span><span class="sxs-lookup"><span data-stu-id="dba43-649">![Connection string pointing to SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="dba43-650">*SQL データベースを指す接続文字列*</span><span class="sxs-lookup"><span data-stu-id="dba43-650">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="dba43-651">**プレビュー** ] ページで [**発行**します。</span><span class="sxs-lookup"><span data-stu-id="dba43-651">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="dba43-652">![Web アプリケーションの発行](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "web アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="dba43-652">![Publishing the web application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publishing the web application")</span></span>

    <span data-ttu-id="dba43-653">*Web アプリケーションの発行*</span><span class="sxs-lookup"><span data-stu-id="dba43-653">*Publishing the web application*</span></span>
9. <span data-ttu-id="dba43-654">発行プロセスが完了すると、既定のブラウザーが発行した web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="dba43-654">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="dba43-655">![Windows Azure にアプリケーションが発行される](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "アプリケーションは Windows Azure に発行")</span><span class="sxs-lookup"><span data-stu-id="dba43-655">![Application published to Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="dba43-656">*Windows Azure に発行されたアプリケーション*</span><span class="sxs-lookup"><span data-stu-id="dba43-656">*Application published to Windows Azure*</span></span>
