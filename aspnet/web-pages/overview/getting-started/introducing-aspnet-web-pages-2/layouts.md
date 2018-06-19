---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: ASP.NET Web Pages の概要 - 一貫したレイアウトを作成 |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルでは、レイアウトを使用して ASP.NET Web Pages を使用するサイトのページに対して一貫した外観を作成する方法を示します。 完了すると想定します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: c2d5c4d8ed8a71979c16d484ab90d283a45de537
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899815"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="bfd66-104">ASP.NET Web Pages の一貫したレイアウトの作成の概要</span><span class="sxs-lookup"><span data-stu-id="bfd66-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="bfd66-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bfd66-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="bfd66-106">このチュートリアルを使用する方法を示します*レイアウト*を ASP.NET Web Pages を使用するサイト ページの一貫した外観を作成します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="bfd66-107">を通じてシリーズを完了すると想定[データベースのデータを削除する ASP.NET Web Pages で](https://go.microsoft.com/fwlink/?LinkId=251584)です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="bfd66-108">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="bfd66-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="bfd66-109">どのようなレイアウト ページです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-109">What a layout page is.</span></span>
> - <span data-ttu-id="bfd66-110">動的なコンテンツとレイアウト ページを結合する方法。</span><span class="sxs-lookup"><span data-stu-id="bfd66-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="bfd66-111">レイアウト ページに値を渡す方法。</span><span class="sxs-lookup"><span data-stu-id="bfd66-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="bfd66-112">レイアウトについて</span><span class="sxs-lookup"><span data-stu-id="bfd66-112">About Layouts</span></span>

<span data-ttu-id="bfd66-113">これまでに作成したページがすべて完了、スタンドアロンのページです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="bfd66-114">同じサイトに属しているが、共通の要素または標準の外観はありません。</span><span class="sxs-lookup"><span data-stu-id="bfd66-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="bfd66-115">ほとんどのサイトには、一貫した外観とレイアウトができます。</span><span class="sxs-lookup"><span data-stu-id="bfd66-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="bfd66-116">たとえばに移動する場合、 [Microsoft.com/web](https://www.microsoft.com/web/)サイトし、少し調べて、すべてのページが全体的なレイアウトをビジュアル テーマに従うことを参照してください。</span><span class="sxs-lookup"><span data-stu-id="bfd66-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![ヘッダーのナビゲーション領域、コンテンツ領域、およびページ フッターのレイアウトを示す Microsoft.com/web サイト ページ](layouts/_static/image1.png)

<span data-ttu-id="bfd66-118">*非効率的な*このレイアウトを作成する方法はそれぞれ、ページのヘッダー、ナビゲーション バー、およびフッターを個別に定義することです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="bfd66-119">複製するのではするマークアップを同じたびにします。</span><span class="sxs-lookup"><span data-stu-id="bfd66-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="bfd66-120">変更を加える必要がある場合 (たとえば、更新、フッター)、各ページを個別に変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bfd66-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="bfd66-121">ような場合は*レイアウト ページ*取得します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="bfd66-122">ASP.NET Web ページで、サイト上のページの全体的なコンテナーを提供するレイアウト ページを定義できます。</span><span class="sxs-lookup"><span data-stu-id="bfd66-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="bfd66-123">たとえば、[レイアウト] ページでは、ヘッダーのナビゲーション領域およびフッターを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="bfd66-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="bfd66-124">[レイアウト] ページには、メイン コンテンツの行き先のプレース ホルダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bfd66-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="bfd66-125">マークアップおよびコード ページのみを含む個々 のコンテンツ ページを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="bfd66-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="bfd66-126">コンテンツ ページがなくても完全な HTML ページです。必要なしないであっても、`<body>`要素。</span><span class="sxs-lookup"><span data-stu-id="bfd66-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="bfd66-127">コンテンツを表示するレイアウト ページを ASP.NET に指示するコードの行もあります。</span><span class="sxs-lookup"><span data-stu-id="bfd66-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="bfd66-128">このリレーションシップのしくみをほぼ表示される画像を次に示します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-128">Here's a picture that shows roughly how this relationship works:</span></span>

![2 つのコンテンツ ページと適合するレイアウト ページを表示する概念図](layouts/_static/image2.png)

<span data-ttu-id="bfd66-130">この連携で理解しやすい操作で表示される場合。</span><span class="sxs-lookup"><span data-stu-id="bfd66-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="bfd66-131">このチュートリアルでは、レイアウトを使用する、映画ページを変更します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="bfd66-132">レイアウト ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-132">Adding a Layout Page</span></span>

<span data-ttu-id="bfd66-133">ヘッダー、フッター、およびメイン コンテンツ領域での通常のページ レイアウトを定義するレイアウト ページを作成して開始します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="bfd66-134">WebPagesMovies サイト内には、名前付き CSHTML ページを追加します。  *\_Layout.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="bfd66-135">先頭にアンダー スコア ( `_` ) 文字は重要です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="bfd66-136">ページの名前にアンダー スコアである場合、ASP.NET がブラウザーにそのページを送信しません直接です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="bfd66-137">この規則では、ユーザーは直接要求できることはできませんが、サイトに必要なページを定義できます。</span><span class="sxs-lookup"><span data-stu-id="bfd66-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="bfd66-138">ページのコンテンツを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bfd66-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="bfd66-139">このマークアップを使用する HTML だけことがわかります`<div>`では、ページと 1 つ以上 3 つのセクションを定義する要素`<div>`3 つのセクションを保持する要素。</span><span class="sxs-lookup"><span data-stu-id="bfd66-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="bfd66-140">フッターには Razor コードのビットが含まれています:`@DateTime.Now.Year`ページ内の場所に現在の年を表示します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="bfd66-141">名前付きスタイル シートへのリンクがあることに注意してください。 *Movies.css*です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="bfd66-142">スタイル シートとは、要素の物理的なレイアウトの詳細を定義します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="bfd66-143">すぐに作成します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-143">You'll create that in a moment.</span></span>

<span data-ttu-id="bfd66-144">こののみ通常とは異なる機能 *\_Layout.cshtml*ページは、`@Render.Body()`行です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="bfd66-145">このレイアウトが別のページとマージされるときに、コンテンツの行き先プレース ホルダーです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="bfd66-146">.Css ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-146">Adding a .css File</span></span>

<span data-ttu-id="bfd66-147">カスケード スタイル シート (CSS) の規則を使用するは、ページ要素の実際の配置 (つまり、外観) を定義することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="bfd66-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="bfd66-148">作成するように、 *.css*を新しいレイアウトの規則を持つファイルです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="bfd66-149">WebMatrix でサイトのルートを選択します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="bfd66-150">次に、**ファイル** タブ、リボンの下の矢印をクリックして、**新規**ボタンをクリックし、をクリックして**新しいフォルダー**です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![リボンの 新規 '新しいフォルダー' オプション。](layouts/_static/image3.png)

<span data-ttu-id="bfd66-152">新しいフォルダーの名前を付けます*スタイル*です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-152">Name the new folder *Styles*.</span></span>

![新しいフォルダー 'スタイル' の名前付け](layouts/_static/image4.png)

<span data-ttu-id="bfd66-154">新しい内部*スタイル*フォルダー、という名前のファイルを作成する*Movies.css*です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Movies.css ファイルの新規作成](layouts/_static/image5.png)

<span data-ttu-id="bfd66-156">新しい内容を置き換える *.css*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="bfd66-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="bfd66-157">2 つの点に注意してくださいを除く、CSS 規則の大部分を言うされません。</span><span class="sxs-lookup"><span data-stu-id="bfd66-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="bfd66-158">1 つは、フォントとサイズを設定するだけでなく、使用しているルール絶対配置ヘッダー、フッター、およびメイン コンテンツ領域の場所を確立するためにです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="bfd66-159">かどうかは、CSS で配置する新しいしたらを読み取ることができます、 [CSS 配置](http://www.w3schools.com/css/css_positioning.asp)W3Schools サイトのチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="bfd66-160">もう 1 つは、下部、元々 のスタイル ルールがコピーされるしたおで定義されている個別に注意してください、 *Movies.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bfd66-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="bfd66-161">これらの規則が使用されていた、[を表示するデータを使用して ASP.NET Web Pages の概要](https://go.microsoft.com/fwlink/?LinkId=251580)するのには、チュートリアル、`WebGrid`ヘルパーがテーブルにストライプ化を追加するマークアップを表示します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="bfd66-162">(使用する場合、 *.css*ファイル スタイル定義がサイト全体のスタイル ルールをも配置する場合があります)。</span><span class="sxs-lookup"><span data-stu-id="bfd66-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="bfd66-163">レイアウトを使用するビデオ ファイルの更新</span><span class="sxs-lookup"><span data-stu-id="bfd66-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="bfd66-164">今すぐ新しいレイアウトを使用するようにサイト内の既存のファイルを更新することができます。</span><span class="sxs-lookup"><span data-stu-id="bfd66-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="bfd66-165">開く、 *Movies.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bfd66-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="bfd66-166">上部にある、コードの最初の行として以下を追加します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="bfd66-167">ページは、この方法を今すぐ開始します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="bfd66-168">この 1 行のコードが ASP.NET に指示する場合、*映画*でマージするか、ページが実行される、  *\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bfd66-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="bfd66-169">以降、 *Movies.cshtml*ファイルは今すぐレイアウト ページを使用して、マークアップからを削除することができます、 *Movies.cshtml*によって対処がページ、  *\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bfd66-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="bfd66-170">取り出して、 `<!DOCTYPE>`、 `<html>`、および`<body>`タグと終了タグ。</span><span class="sxs-lookup"><span data-stu-id="bfd66-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="bfd66-171">全体を取り出して`<head>`要素とその内容それらのルールを取得した後に、グリッドのスタイル ルールを含んでいます、 *.css*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bfd66-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="bfd66-172">中は、変更、既存`<h1>`要素を`<h2>`要素; が、`<h1>`レイアウト ページを既に内の要素。</span><span class="sxs-lookup"><span data-stu-id="bfd66-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="bfd66-173">変更、 `<h2>` 「リスト映画」するテキスト。</span><span class="sxs-lookup"><span data-stu-id="bfd66-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="bfd66-174">通常は、コンテンツ ページでこれらの種類の変更を有効にする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="bfd66-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="bfd66-175">レイアウト ページで、サイトを起動すると、まず、これらすべての要素のコンテンツ ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="bfd66-176">ここでは、ただし、変換しているスタンドアロン ページ クリーンアップのビットは、レイアウトを使用している 1 つにします。</span><span class="sxs-lookup"><span data-stu-id="bfd66-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="bfd66-177">、が完了したら、 *Movies.cshtml*ページは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="bfd66-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="bfd66-178">レイアウトのテスト</span><span class="sxs-lookup"><span data-stu-id="bfd66-178">Testing the Layout</span></span>

<span data-ttu-id="bfd66-179">レイアウトの外観を確認できます。</span><span class="sxs-lookup"><span data-stu-id="bfd66-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="bfd66-180">WebMatrix でを右クリックし、 *Movies.cshtml*ページし、選択**ブラウザーで起動**です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="bfd66-181">ブラウザーでは、ページを表示するときは、このページのようになります。</span><span class="sxs-lookup"><span data-stu-id="bfd66-181">When the browser displays the page, it looks like this page:</span></span>

![レイアウトを使用して表示されるビデオ ページ](layouts/_static/image6.png)

<span data-ttu-id="bfd66-183">ASP.NET に Movies.cshtml ページの内容とマージ、  *\_Layout.cshtml*右のページ、`RenderBody`メソッドはします。</span><span class="sxs-lookup"><span data-stu-id="bfd66-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="bfd66-184">そしてもちろん、  *\_Layout.cshtml*ページを参照する、 *.css*ページの外観を定義するファイル。</span><span class="sxs-lookup"><span data-stu-id="bfd66-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="bfd66-185">レイアウトを使用する AddMovie ページの更新</span><span class="sxs-lookup"><span data-stu-id="bfd66-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="bfd66-186">レイアウトの実際の利点は、ことできますで使用するすべてのページで、サイトです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="bfd66-187">開く、 *AddMovie.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="bfd66-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="bfd66-188">留意することがあります、 *AddMovie.cshtml*  ページでは、検証エラー メッセージの外観を定義することで、CSS ルールがいくつかを最初からです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="bfd66-189">あるため、 *.css*ファイルを今すぐサイトをそれらの規則を移動することができます、 *.css*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bfd66-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="bfd66-190">削除してから、 *AddMovie.cshtml*ファイルの下部に追加して、 *Movies.css*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bfd66-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="bfd66-191">次の規則を移動します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="bfd66-192">これで、同じ種類の変更の*AddMovie.cshtml*の*Movies.cshtml* -追加`Layout="~/_Layout.cshtml;`とは無関係な HTML マークアップを削除します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="bfd66-193">`<h1>` 要素を `<h2>` に変更します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="bfd66-194">完了したら、ページはこの例のようになります。</span><span class="sxs-lookup"><span data-stu-id="bfd66-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="bfd66-195">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-195">Run the page.</span></span> <span data-ttu-id="bfd66-196">これで次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="bfd66-196">Now it looks like this illustration:</span></span>

!['映画' ページの追加、レイアウトを使用してレンダリング](layouts/_static/image7.png)

<span data-ttu-id="bfd66-198">サイト内のページに同様に変更する — *EditMovie.cshtml*と*DeleteMovie.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="bfd66-199">ただし、実行する前にことも、もう少し柔軟なレイアウトに別の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="bfd66-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="bfd66-200">[レイアウト] ページにタイトル情報を渡す.</span><span class="sxs-lookup"><span data-stu-id="bfd66-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="bfd66-201"> *\_Layout.cshtml*作成したページには、 `<title>` "マイ ムービー Site"に設定されている要素です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="bfd66-202">ほとんどのブラウザーでは、タブのテキストとしてこの要素の内容を表示します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-202">Most browsers display the content of this element as the text on a tab:</span></span>

![ページの&lt;タイトル&gt;ブラウザー タブで表示される要素](layouts/_static/image8.png)

<span data-ttu-id="bfd66-204">このタイトル情報はジェネリックです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-204">This title information is generic.</span></span> <span data-ttu-id="bfd66-205">現在のページに詳細を指定するタイトルのテキストを表示するとします。</span><span class="sxs-lookup"><span data-stu-id="bfd66-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="bfd66-206">(タイトルのテキストにも使用検索エンジンによって、ページの内容を判断します。)同様に、コンテンツ ページから情報を渡すことができます*Movies.cshtml*または*AddMovie.cshtml*へしを使用して、レイアウト ページ、レイアウト ページをカスタマイズするには、その情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="bfd66-207">開く、 *Movies.cshtml*ページをもう一度です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="bfd66-208">上部のコードでは、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="bfd66-209">`Page`オブジェクトは、すべての利用 *.cshtml*ページ、つまり、この目的では、ページとレイアウトの間で情報を共有します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="bfd66-210">開く、<em>\_Layout.cshtml</em>ページ。</span><span class="sxs-lookup"><span data-stu-id="bfd66-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="bfd66-211">変更、`<title>`要素のため、このマークアップのようなことを検索します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="bfd66-212">処理します。 このコードの表示、`Page.Title`プロパティ ページで、その場所にすぐにします。</span><span class="sxs-lookup"><span data-stu-id="bfd66-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="bfd66-213">実行、 *Movies.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="bfd66-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="bfd66-214">この時点のブラウザー タブが表示の値として渡される内容`Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="bfd66-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![動的に作成されるタイトルを表示、ブラウザーのタブ](layouts/_static/image9.png)

<span data-ttu-id="bfd66-216">場合は、ブラウザーでページ ソースを表示します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="bfd66-217">`<title>`要素としてレンダリング`<title>List Movies</title>`です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="bfd66-218">**Page オブジェクト**</span><span class="sxs-lookup"><span data-stu-id="bfd66-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="bfd66-219">機能は便利です`Page`は動的オブジェクトには、-、`Title`プロパティは、固定または予約名ではありません。</span><span class="sxs-lookup"><span data-stu-id="bfd66-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="bfd66-220">使用することができます*任意*の値の名前、`Page`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="bfd66-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="bfd66-221">たとえば、でしたように簡単に渡されたタイトルという名前のプロパティを使用して、`Page.CurrentName`または`Page.MyPage`です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="bfd66-222">唯一の制限は、名前がどのようなプロパティの名前を付けるの通常の規則に従うことです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="bfd66-223">(たとえば、名前に使用できませんスペースです。)</span><span class="sxs-lookup"><span data-stu-id="bfd66-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="bfd66-224">使用して任意の数の値を渡すことができます、`Page`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="bfd66-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="bfd66-225">ようなものを使用して値を渡すことが、レイアウト ページにムービー情報を渡す場合は、`Page.MovieTitle`と`Page.Genre`と`Page.MovieYear`です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="bfd66-226">(または情報を格納するために発明する他の任意の名前)。唯一の要件: これはおそらく明白な — コンテンツ ページとページ レイアウト ページ内の同じ名前を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bfd66-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="bfd66-227">使用して渡す情報、`Page`オブジェクトは、レイアウト ページに表示するテキストだけに制限はありません。</span><span class="sxs-lookup"><span data-stu-id="bfd66-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="bfd66-228">レイアウト ページに値を渡すことができ、レイアウト ページ内のコードが、ページのセクションを表示するかどうかを決定する値を使用して、どのような *.css*を使用するファイル。</span><span class="sxs-lookup"><span data-stu-id="bfd66-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="bfd66-229">値を渡す、`Page`オブジェクトは、他の値と同じようにコードで使用します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="bfd66-230">値がコンテンツ ページで発生し、レイアウト ページに渡されるのみです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="bfd66-231">開く、 *AddMovie.cshtml*ページし、のタイトルを提供するコードの先頭に行を追加、 *AddMovie.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="bfd66-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="bfd66-232">実行、 *AddMovie.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="bfd66-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="bfd66-233">新しいタイトルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bfd66-233">You see the new title there:</span></span>

![動的に作成 '追加映画' タイトルを表示、ブラウザーのタブ](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="bfd66-235">レイアウトを使用して残りのページを更新</span><span class="sxs-lookup"><span data-stu-id="bfd66-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="bfd66-236">今すぐ新しいレイアウトを使用するよう、サイト内の残りのページを完了できます。</span><span class="sxs-lookup"><span data-stu-id="bfd66-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="bfd66-237">開いている*EditMovie.cshtml*と*DeleteMovie.cshtml*で有効にして、それぞれに同じ変更を行います。</span><span class="sxs-lookup"><span data-stu-id="bfd66-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="bfd66-238">[レイアウト] ページにリンクされているコードの行を追加します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="bfd66-239">ページのタイトルを設定する行を追加します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="bfd66-240">または</span><span class="sxs-lookup"><span data-stu-id="bfd66-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="bfd66-241">余分なすべての HTML マークアップを削除する — 基本的には、内にあるビットのみのままにして、`<body>`要素 (および上部にあるコード ブロック) します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="bfd66-242">変更、`<h1>`要素に、`<h2>`要素。</span><span class="sxs-lookup"><span data-stu-id="bfd66-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="bfd66-243">これらの変更を加えたときに、それぞれをテストし、正しく表示されないことと、タイトルが正しいことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="bfd66-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="bfd66-244">分離考えレイアウト ページの概要</span><span class="sxs-lookup"><span data-stu-id="bfd66-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="bfd66-245">このチュートリアルで作成した、  *\_Layout.cshtml*ページ、使用、`RenderBody`別のページからコンテンツをマージするメソッド。</span><span class="sxs-lookup"><span data-stu-id="bfd66-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="bfd66-246">Web ページのレイアウトを使用するための基本的なパターンです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="bfd66-247">レイアウト ページでは、ここに対応していないする機能があります。</span><span class="sxs-lookup"><span data-stu-id="bfd66-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="bfd66-248">たとえば、レイアウト ページを入れ子にすることができます — 1 つのレイアウト ページをさらに参照別できます。</span><span class="sxs-lookup"><span data-stu-id="bfd66-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="bfd66-249">入れ子になったレイアウトをさまざまなレイアウトを必要とするサイトのサブセクションで作業する場合に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="bfd66-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="bfd66-250">追加のメソッドを使用することもできます (たとえば、 `RenderSection`) を設定するレイアウト ページのセクションをという名前です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="bfd66-251">レイアウト ページの組み合わせと *.css*ファイルは強力です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="bfd66-252">次のチュートリアルの系列が表示されます、WebMatrix で作成してに基づいてサイト、*テンプレート*、されているサイトを提供することで構築済みの機能です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="bfd66-253">テンプレートを使用するレイアウト ページ、見栄えが良くして、メニューなどの機能があるサイトを作成する CSS の適切な使い方です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="bfd66-254">ページのレイアウトと CSS を使用する機能を示す、テンプレートに基づくサイトのホーム ページのスクリーン ショットを次に示します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![ヘッダーのナビゲーション領域、コンテンツ領域、省略可能なセクション、およびログインのリンクを示す WebMatrix サイト テンプレートによって作成されたレイアウト](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="bfd66-256">ムービーのページ (レイアウト ページを使用して更新) の完全な一覧</span><span class="sxs-lookup"><span data-stu-id="bfd66-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="bfd66-257">[完了] ページ (レイアウトの更新) ビデオ ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="bfd66-258">Delete ムービーのページ (レイアウトの更新) の完了 ページの一覧</span><span class="sxs-lookup"><span data-stu-id="bfd66-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="bfd66-259">(レイアウトの更新) の編集ビデオ ページの 完了 ページの一覧</span><span class="sxs-lookup"><span data-stu-id="bfd66-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="bfd66-260">次へ直近の見通し</span><span class="sxs-lookup"><span data-stu-id="bfd66-260">Coming Up Next</span></span>

<span data-ttu-id="bfd66-261">次のチュートリアルでは、すべてのユーザーが表示できるように、サイトをインターネットに公開する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bfd66-262">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="bfd66-262">Additional Resources</span></span>

- <span data-ttu-id="bfd66-263">[一貫性のある参照を作成する](https://go.microsoft.com/fwlink/?LinkID=202891)-レイアウトの操作をいくつかの詳細を提供するアーティクルです。</span><span class="sxs-lookup"><span data-stu-id="bfd66-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="bfd66-264">また、レイアウト ページを表示または一部のコンテンツを非表示に値を渡す方法も説明します。</span><span class="sxs-lookup"><span data-stu-id="bfd66-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="bfd66-265">[入れ子になったと Razor レイアウト ページ](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor)— Mike Brind ブログをレイアウト ページを入れ子にする方法の例です。</span><span class="sxs-lookup"><span data-stu-id="bfd66-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="bfd66-266">(ページのダウンロードが含まれています)。</span><span class="sxs-lookup"><span data-stu-id="bfd66-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bfd66-267">[前へ](deleting-data.md)
> [次へ](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="bfd66-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
