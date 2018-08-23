---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: ASP.NET Web Pages の概要 - 一貫性のあるレイアウトを作成する |Microsoft Docs
author: tfitzmac
description: このチュートリアルでは、レイアウトを使用して ASP.NET Web Pages を使用しているサイトで、ページの一貫性のある外観を作成する方法を示します。 完了したと想定して、.
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 8f3d9e8a6f6a0179224e18faf11db3dc1510a095
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831886"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="92c9a-104">ASP.NET Web ページの概要 - 一貫性のあるレイアウトを作成します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="92c9a-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="92c9a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="92c9a-106">このチュートリアルは、使用する方法を示します*レイアウト*ASP.NET Web Pages を使用するサイトで、ページの一貫性のある外観を作成します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="92c9a-107">を通じてシリーズを完了したと想定して[データベースのデータを削除する ASP.NET Web Pages で](https://go.microsoft.com/fwlink/?LinkId=251584)します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="92c9a-108">学習内容。</span><span class="sxs-lookup"><span data-stu-id="92c9a-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="92c9a-109">どのようなレイアウト ページは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="92c9a-109">What a layout page is.</span></span>
> - <span data-ttu-id="92c9a-110">動的なコンテンツとレイアウト ページを結合する方法。</span><span class="sxs-lookup"><span data-stu-id="92c9a-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="92c9a-111">レイアウト ページに値を渡す方法。</span><span class="sxs-lookup"><span data-stu-id="92c9a-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="92c9a-112">レイアウトについて</span><span class="sxs-lookup"><span data-stu-id="92c9a-112">About Layouts</span></span>

<span data-ttu-id="92c9a-113">これまでに作成したページがすべて完了したら、スタンドアロンのページ。</span><span class="sxs-lookup"><span data-stu-id="92c9a-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="92c9a-114">すべて、同じサイトに属するが、共通の要素または標準の外観がないです。</span><span class="sxs-lookup"><span data-stu-id="92c9a-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="92c9a-115">ほとんどのサイトには一貫性のある外観とレイアウトをが。</span><span class="sxs-lookup"><span data-stu-id="92c9a-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="92c9a-116">たとえばに移動する場合、 [Microsoft.com/web](https://www.microsoft.com/web/)サイト、少し調べてみると、すべてのページが全体的なレイアウトをビジュアル テーマを従うことを参照してください。</span><span class="sxs-lookup"><span data-stu-id="92c9a-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Microsoft.com/web サイトのページ ヘッダー、ナビゲーション領域、コンテンツ領域、およびフッターのレイアウトを示す](layouts/_static/image1.png)

<span data-ttu-id="92c9a-118">*非効率的な*このレイアウトを作成する方法は各ページで、ヘッダー、ナビゲーション バーで、およびフッターを個別に定義することです。</span><span class="sxs-lookup"><span data-stu-id="92c9a-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="92c9a-119">複製するのではする同じマークアップを毎回。</span><span class="sxs-lookup"><span data-stu-id="92c9a-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="92c9a-120">何かを変更する場合 (たとえば、更新、フッター)、各ページを個別に変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="92c9a-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="92c9a-121">ここ*レイアウト ページ*取得します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="92c9a-122">ASP.NET Web ページで、サイトのページの全体のコンテナーを提供するレイアウト ページを定義できます。</span><span class="sxs-lookup"><span data-stu-id="92c9a-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="92c9a-123">たとえば、レイアウト ページでは、ヘッダー、ナビゲーションの領域およびフッターを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="92c9a-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="92c9a-124">レイアウト ページには、メイン コンテンツの行き先プレース ホルダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="92c9a-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="92c9a-125">マークアップおよびコード ページのみを含めることが個々 のコンテンツ ページを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="92c9a-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="92c9a-126">コンテンツ ページは、完全な HTML ページを使用する必要はありません。必要すらありません、`<body>`要素。</span><span class="sxs-lookup"><span data-stu-id="92c9a-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="92c9a-127">コンテンツを表示するレイアウト ページを ASP.NET に指示するコードの行もあります。</span><span class="sxs-lookup"><span data-stu-id="92c9a-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="92c9a-128">このリレーションシップのしくみをほぼ表示する図を次に示します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-128">Here's a picture that shows roughly how this relationship works:</span></span>

![2 つのコンテンツ ページと適合するレイアウト ページを表示する概念図](layouts/_static/image2.png)

<span data-ttu-id="92c9a-130">これは、簡単に動作を確認するときに理解できます。</span><span class="sxs-lookup"><span data-stu-id="92c9a-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="92c9a-131">このチュートリアルでは、映画のページ レイアウトを使用するを変更します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="92c9a-132">レイアウト ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-132">Adding a Layout Page</span></span>

<span data-ttu-id="92c9a-133">ヘッダー、フッター、メイン コンテンツ領域と一般的なページ レイアウトを定義するレイアウト ページの作成から始めます。</span><span class="sxs-lookup"><span data-stu-id="92c9a-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="92c9a-134">WebPagesMovies サイトの追加という名前の CSHTML ページ *\_Layout.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="92c9a-135">先頭のアンダー スコア ( `_` ) 文字は重要です。</span><span class="sxs-lookup"><span data-stu-id="92c9a-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="92c9a-136">ページの名前がアンダー スコアで始まる場合は、ASP.NET がブラウザーにそのページを送信しません直接。</span><span class="sxs-lookup"><span data-stu-id="92c9a-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="92c9a-137">この規則では、ユーザーが直接要求したりすることはできませんが、サイトに必要なページを定義できます。</span><span class="sxs-lookup"><span data-stu-id="92c9a-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="92c9a-138">次のように、ページのコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="92c9a-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="92c9a-139">ご覧のように、このマークアップを使用するように HTML`<div>`複数ページに 1 を加えたで 3 つのセクションを定義する要素`<div>`3 つのセクションを保持する要素。</span><span class="sxs-lookup"><span data-stu-id="92c9a-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="92c9a-140">フッターには Razor コードが含まれています:`@DateTime.Now.Year`ページ内の場所に現在の年を表示します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="92c9a-141">という名前のスタイル シートへのリンクがあることに注意してください。 *Movies.css*します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="92c9a-142">スタイル シートは、要素の物理的なレイアウトの詳細を定義します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="92c9a-143">すぐに作成します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-143">You'll create that in a moment.</span></span>

<span data-ttu-id="92c9a-144">これでのみ通常とは異なる機能 *\_Layout.cshtml*ページは、`@Render.Body()`行。</span><span class="sxs-lookup"><span data-stu-id="92c9a-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="92c9a-145">このレイアウトを結合する別のページで、コンテンツのどこのプレース ホルダーです。</span><span class="sxs-lookup"><span data-stu-id="92c9a-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="92c9a-146">.Css ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-146">Adding a .css File</span></span>

<span data-ttu-id="92c9a-147">カスケード スタイル シート (CSS) ルールを使用することのページ要素の実際の配置 (つまり、外観) を定義することをお勧めです。</span><span class="sxs-lookup"><span data-stu-id="92c9a-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="92c9a-148">作成できるように、 *.css*新しいレイアウトの規則を含むファイル。</span><span class="sxs-lookup"><span data-stu-id="92c9a-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="92c9a-149">WebMatrix で、サイトのルートを選択します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="92c9a-150">次に、**ファイル** タブのリボンの下の矢印をクリックします。、**新規**ボタンをクリックし、 をクリックし、**新しいフォルダー**。</span><span class="sxs-lookup"><span data-stu-id="92c9a-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![リボンの [新規] [新しいフォルダー] オプション。](layouts/_static/image3.png)

<span data-ttu-id="92c9a-152">新しいフォルダーの名前*スタイル*します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-152">Name the new folder *Styles*.</span></span>

![新しいフォルダー 'スタイル' の名前付け](layouts/_static/image4.png)

<span data-ttu-id="92c9a-154">新しい内部*スタイル*フォルダー、という名前のファイルを作成する*Movies.css*します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![新しい Movies.css ファイルを作成します。](layouts/_static/image5.png)

<span data-ttu-id="92c9a-156">新しい内容を置き換える *.css*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="92c9a-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="92c9a-157">以外に 2 つのことに注意してください。 これらの CSS ルールの詳細については示さことはありません。</span><span class="sxs-lookup"><span data-stu-id="92c9a-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="92c9a-158">1 つは、フォントとサイズを設定するだけでなく、使用しているルール絶対配置ヘッダー、フッター、およびメイン コンテンツ領域の場所を確立するためにです。</span><span class="sxs-lookup"><span data-stu-id="92c9a-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="92c9a-159">読むことができるかどうか、新しい CSS の配置にしたら、 [CSS 配置](http://www.w3schools.com/css/css_positioning.asp)W3Schools サイトのチュートリアル。</span><span class="sxs-lookup"><span data-stu-id="92c9a-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="92c9a-160">もう 1 つがで個別に定義されている下部で、コピーした元々 のスタイル ルールに注意してください、 *Movies.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="92c9a-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="92c9a-161">これらの規則が使用されていた、[を表示するデータを使用して ASP.NET Web Pages の概要](https://go.microsoft.com/fwlink/?LinkId=251580)させるチュートリアル、`WebGrid`ヘルパーがストライプをテーブルに追加するマークアップをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="92c9a-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="92c9a-162">(使用する場合、 *.css*ファイル スタイルの定義がサイト全体のスタイル ルールにも配置する場合があります)。</span><span class="sxs-lookup"><span data-stu-id="92c9a-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="92c9a-163">レイアウトを使用するビデオ ファイルの更新</span><span class="sxs-lookup"><span data-stu-id="92c9a-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="92c9a-164">これで、新しいレイアウトを使用するサイト内の既存のファイルを更新できます。</span><span class="sxs-lookup"><span data-stu-id="92c9a-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="92c9a-165">開く、 *Movies.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="92c9a-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="92c9a-166">上部にある、コードの最初の行として以下を追加します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="92c9a-167">ページは、この方法を今すぐ開始します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="92c9a-168">この 1 行のコードが ASP.NET に指示する場合、*映画*ページが実行されるとにマージするか、  *\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="92c9a-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="92c9a-169">以降、 *Movies.cshtml*ファイルはこれでレイアウト ページを使用して、マークアップからを削除することができます、 *Movies.cshtml*によって対処がページ、  *\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="92c9a-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="92c9a-170">`<!DOCTYPE>`、 `<html>`、および`<body>`開始タグと終了します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="92c9a-171">全体を`<head>`要素とその内容これらのルールを取得したため、グリッドのスタイル ルールが含まれています、 *.css*ファイル。</span><span class="sxs-lookup"><span data-stu-id="92c9a-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="92c9a-172">ついでに、変更、既存`<h1>`要素を`<h2>`要素; が、`<h1>`レイアウト ページを既に要素。</span><span class="sxs-lookup"><span data-stu-id="92c9a-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="92c9a-173">変更、 `<h2>` "一覧 Movies"するテキスト。</span><span class="sxs-lookup"><span data-stu-id="92c9a-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="92c9a-174">通常は、コンテンツ ページでこの種の変更を有効にする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="92c9a-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="92c9a-175">レイアウト ページ サイトを開始するときに、まず、これらすべての要素のコンテンツ ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="92c9a-176">ここでは、ただし、クリーンアップの一部があるために、レイアウトを使用する 1 つに、スタンドアロン ページを変換しています。</span><span class="sxs-lookup"><span data-stu-id="92c9a-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="92c9a-177">操作を完了するときに、 *Movies.cshtml*ページは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="92c9a-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="92c9a-178">レイアウトのテスト</span><span class="sxs-lookup"><span data-stu-id="92c9a-178">Testing the Layout</span></span>

<span data-ttu-id="92c9a-179">これで、レイアウトがどのようにを確認できます。</span><span class="sxs-lookup"><span data-stu-id="92c9a-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="92c9a-180">WebMatrix で、右クリックし、 *Movies.cshtml*  ページ**ブラウザーで起動**します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="92c9a-181">ブラウザーでは、ページを表示するときは、このページのようになります。</span><span class="sxs-lookup"><span data-stu-id="92c9a-181">When the browser displays the page, it looks like this page:</span></span>

![レイアウトを使用して表示されるビデオ ページ](layouts/_static/image6.png)

<span data-ttu-id="92c9a-183">ASP.NET に Movies.cshtml ページのコンテンツをマージ、  *\_Layout.cshtml*適切な場所のページ、`RenderBody`メソッドします。</span><span class="sxs-lookup"><span data-stu-id="92c9a-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="92c9a-184">もちろん、  *\_Layout.cshtml*ページを参照する、 *.css*ページの外観を定義するファイル。</span><span class="sxs-lookup"><span data-stu-id="92c9a-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="92c9a-185">レイアウトを使用する AddMovie ページを更新しています</span><span class="sxs-lookup"><span data-stu-id="92c9a-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="92c9a-186">レイアウトの実際の利点は、ことできますで使用するすべてのページで、サイトです。</span><span class="sxs-lookup"><span data-stu-id="92c9a-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="92c9a-187">開く、 *AddMovie.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="92c9a-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="92c9a-188">覚えている可能性があります、 *AddMovie.cshtml*  ページでは、検証エラー メッセージの外観を定義するために、いくつかの CSS 規則を最初から。</span><span class="sxs-lookup"><span data-stu-id="92c9a-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="92c9a-189">あるので、 *.css*これで、サイトのファイルは、それらの規則を移動する、 *.css*ファイル。</span><span class="sxs-lookup"><span data-stu-id="92c9a-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="92c9a-190">削除、 *AddMovie.cshtml*の下部に追加ファイルを開き、 *Movies.css*ファイル。</span><span class="sxs-lookup"><span data-stu-id="92c9a-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="92c9a-191">次の規則を移動します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="92c9a-192">次に、同じ種類の変更を加えます*AddMovie.cshtml*の*Movies.cshtml* -追加`Layout="~/_Layout.cshtml;`および削除するには余分な HTML マークアップ。</span><span class="sxs-lookup"><span data-stu-id="92c9a-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="92c9a-193">`<h1>` 要素を `<h2>` に変更します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="92c9a-194">完了したら、ページはこの例のようになります。</span><span class="sxs-lookup"><span data-stu-id="92c9a-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="92c9a-195">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-195">Run the page.</span></span> <span data-ttu-id="92c9a-196">これで次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="92c9a-196">Now it looks like this illustration:</span></span>

![レイアウトを使用してレンダリングの映画の追加 ページ](layouts/_static/image7.png)

<span data-ttu-id="92c9a-198">サイト内のページに同様に変更する — *EditMovie.cshtml*と*DeleteMovie.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="92c9a-199">ただし、実行する前に、なりますが、もう少し柔軟なレイアウトに別の変更を加えることができます。</span><span class="sxs-lookup"><span data-stu-id="92c9a-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="92c9a-200">レイアウト ページにタイトル情報を渡す</span><span class="sxs-lookup"><span data-stu-id="92c9a-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="92c9a-201">*\_Layout.cshtml*作成したページには、 `<title>` "My ムービー Site"に設定されている要素。</span><span class="sxs-lookup"><span data-stu-id="92c9a-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="92c9a-202">ほとんどのブラウザー タブのテキストとしてこの要素の内容を表示します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-202">Most browsers display the content of this element as the text on a tab:</span></span>

![ページの&lt;タイトル&gt;ブラウザー タブで表示される要素](layouts/_static/image8.png)

<span data-ttu-id="92c9a-204">このタイトル情報は、ジェネリックです。</span><span class="sxs-lookup"><span data-stu-id="92c9a-204">This title information is generic.</span></span> <span data-ttu-id="92c9a-205">現在のページに詳細を指定するタイトル テキストをたいとします。</span><span class="sxs-lookup"><span data-stu-id="92c9a-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="92c9a-206">(タイトルのテキストはでも使用検索エンジンによって、ページの項目を判断します。)ようなコンテンツ ページから情報を渡すことができます*Movies.cshtml*または*AddMovie.cshtml*レイアウト ページで、し、レイアウト ページをカスタマイズするには、その情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="92c9a-207">開く、 *Movies.cshtml*ページをもう一度です。</span><span class="sxs-lookup"><span data-stu-id="92c9a-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="92c9a-208">上部にあるコードでは、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="92c9a-209">`Page`オブジェクトがすべてで使用可能な *.cshtml*ページ、つまり、この目的は、ページとレイアウト情報を共有します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="92c9a-210">開く、<em>\_Layout.cshtml</em>ページ。</span><span class="sxs-lookup"><span data-stu-id="92c9a-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="92c9a-211">変更、`<title>`要素のため、it がこのマークアップのようになります。</span><span class="sxs-lookup"><span data-stu-id="92c9a-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="92c9a-212">このコードの表示内容に関係なく、`Page.Title`プロパティ ページで、その場所で右。</span><span class="sxs-lookup"><span data-stu-id="92c9a-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="92c9a-213">実行、 *Movies.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="92c9a-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="92c9a-214">今度の値として渡されると、ブラウザー タブが表示されます`Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="92c9a-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![ブラウザーのタブが動的に作成されるタイトルを表示](layouts/_static/image9.png)

<span data-ttu-id="92c9a-216">する場合は、ブラウザーでページ ソースを表示します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="92c9a-217">確認、`<title>`要素としてレンダリング`<title>List Movies</title>`します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="92c9a-218">**Page オブジェクト**</span><span class="sxs-lookup"><span data-stu-id="92c9a-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="92c9a-219">便利な機能の`Page`は動的オブジェクトである —、`Title`プロパティは、固定または予約名ではありません。</span><span class="sxs-lookup"><span data-stu-id="92c9a-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="92c9a-220">使用することができます*任意*の値の名前、`Page`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="92c9a-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="92c9a-221">たとえば、でしたように簡単に渡したことタイトルという名前のプロパティを使用して`Page.CurrentName`または`Page.MyPage`します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="92c9a-222">唯一の制限は、名前が、どのようなプロパティの名前を指定できますの通常の規則に従います。</span><span class="sxs-lookup"><span data-stu-id="92c9a-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="92c9a-223">(たとえば、名前できませんにスペースを含める。)</span><span class="sxs-lookup"><span data-stu-id="92c9a-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="92c9a-224">使用して任意の数の値を渡すことができます、`Page`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="92c9a-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="92c9a-225">レイアウト ページにムービー情報を渡す場合は、ようなものを使用して値を渡すこともできます`Page.MovieTitle`と`Page.Genre`と`Page.MovieYear`します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="92c9a-226">(または、情報を格納するために作成する他の任意の名前。)唯一の要件: これは明らかな可能性があります-コンテンツ ページとレイアウト ページ内の同じ名前を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="92c9a-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="92c9a-227">使用して渡す情報、`Page`オブジェクトは、レイアウト ページに表示するテキストだけに限定されません。</span><span class="sxs-lookup"><span data-stu-id="92c9a-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="92c9a-228">レイアウト ページに値を渡すことができ、レイアウト ページのコードが、ページのセクションを表示するかどうかを決定する値を使用できますし、どのような *.css*を使用するファイル。</span><span class="sxs-lookup"><span data-stu-id="92c9a-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="92c9a-229">値を渡す、`Page`オブジェクトが値をその他のようなコードで使用します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="92c9a-230">値が [コンテンツ] ページで生成され、レイアウト ページに渡されるだけです。</span><span class="sxs-lookup"><span data-stu-id="92c9a-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="92c9a-231">開く、 *AddMovie.cshtml*ページし、のタイトルを提供するコードの先頭に行を追加、 *AddMovie.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="92c9a-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="92c9a-232">実行、 *AddMovie.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="92c9a-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="92c9a-233">新しいタイトルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="92c9a-233">You see the new title there:</span></span>

![動的に作成された '追加映画' タイトルを表示するブラウザー タブ](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="92c9a-235">レイアウトを使用して、残りのページを更新しています</span><span class="sxs-lookup"><span data-stu-id="92c9a-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="92c9a-236">今すぐ新しいレイアウトを使用するよう、サイトの残りのページを完了できます。</span><span class="sxs-lookup"><span data-stu-id="92c9a-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="92c9a-237">開いている*EditMovie.cshtml*と*DeleteMovie.cshtml*で有効にして、それぞれに同じ変更を行います。</span><span class="sxs-lookup"><span data-stu-id="92c9a-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="92c9a-238">レイアウト ページにリンクされているコード行を追加します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="92c9a-239">ページのタイトルを設定する行を追加します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="92c9a-240">または</span><span class="sxs-lookup"><span data-stu-id="92c9a-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="92c9a-241">すべての余分な HTML マークアップを削除する、基本的には、内部にあるビットのみのままに、`<body>`要素 (および、上部にあるコード ブロック)。</span><span class="sxs-lookup"><span data-stu-id="92c9a-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="92c9a-242">変更、`<h1>`要素に、`<h2>`要素。</span><span class="sxs-lookup"><span data-stu-id="92c9a-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="92c9a-243">これらの変更を行ったときに、各テストし、正しく表示されないこと、およびタイトルが正しいかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="92c9a-244">レイアウト ページの分離アイデア</span><span class="sxs-lookup"><span data-stu-id="92c9a-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="92c9a-245">このチュートリアルで作成した、  *\_Layout.cshtml*ページし、使用、`RenderBody`メソッドを別のページからコンテンツをマージします。</span><span class="sxs-lookup"><span data-stu-id="92c9a-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="92c9a-246">Web ページのレイアウトを使用するための基本的なパターンです。</span><span class="sxs-lookup"><span data-stu-id="92c9a-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="92c9a-247">レイアウト ページには、ここで説明していない追加の機能があります。</span><span class="sxs-lookup"><span data-stu-id="92c9a-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="92c9a-248">たとえば、レイアウト ページを入れ子にすることができます: 1 つのレイアウト ページがさらに参照別です。</span><span class="sxs-lookup"><span data-stu-id="92c9a-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="92c9a-249">入れ子になったレイアウトは、さまざまなレイアウトを必要とするサイトのサブセクションを使用している場合に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="92c9a-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="92c9a-250">追加のメソッドを使用することもできます (たとえば、 `RenderSection`) という名前のセクションでは、レイアウト ページを設定します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="92c9a-251">レイアウト ページの組み合わせと *.css*ファイルは強力です。</span><span class="sxs-lookup"><span data-stu-id="92c9a-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="92c9a-252">次のチュートリアル シリーズで後ほど、WebMatrix でサイトを作成できます、に基づいて、*テンプレート*を持つサイトを提供する事前構築済みの機能です。</span><span class="sxs-lookup"><span data-stu-id="92c9a-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="92c9a-253">テンプレートは、レイアウト ページと CSS を美しく、メニューなどの機能がある、サイトを作成する適切な使い方を使用します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="92c9a-254">レイアウト ページと CSS を使用する機能を示す、テンプレートに基づくサイトのホーム ページのスクリーン ショットを次に示します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![ヘッダー、ナビゲーション領域、コンテンツ領域、省略可能なセクション、およびログインのリンクを示す WebMatrix サイト テンプレートによって作成されたレイアウト](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="92c9a-256">ムービーのページ (レイアウト ページを使用して更新) の完全な一覧</span><span class="sxs-lookup"><span data-stu-id="92c9a-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="92c9a-257">[完了] ページ (レイアウトの更新) ムービー ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="92c9a-258">(レイアウトの更新) 削除ムービー ページの完了 ページの一覧</span><span class="sxs-lookup"><span data-stu-id="92c9a-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="92c9a-259">ムービー ページ (レイアウトの更新) の完了 ページの一覧</span><span class="sxs-lookup"><span data-stu-id="92c9a-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="92c9a-260">次回について</span><span class="sxs-lookup"><span data-stu-id="92c9a-260">Coming Up Next</span></span>

<span data-ttu-id="92c9a-261">次のチュートリアルでは、すべてのユーザーが参照できるように、サイトをインターネットに公開する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="92c9a-262">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="92c9a-262">Additional Resources</span></span>

- <span data-ttu-id="92c9a-263">[一貫性のある検索を作成する](https://go.microsoft.com/fwlink/?LinkID=202891)-レイアウトの操作をさらに詳細を提供する記事です。</span><span class="sxs-lookup"><span data-stu-id="92c9a-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="92c9a-264">表示とコンテンツの一部を非表示のレイアウト ページに値を渡す方法も説明します。</span><span class="sxs-lookup"><span data-stu-id="92c9a-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="92c9a-265">[入れ子になったと Razor レイアウト ページ](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor)— Mike Brind ブログ レイアウト ページを入れ子にする方法の例です。</span><span class="sxs-lookup"><span data-stu-id="92c9a-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="92c9a-266">(ページのダウンロードが含まれています)。</span><span class="sxs-lookup"><span data-stu-id="92c9a-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="92c9a-267">[前へ](deleting-data.md)
> [次へ](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="92c9a-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
