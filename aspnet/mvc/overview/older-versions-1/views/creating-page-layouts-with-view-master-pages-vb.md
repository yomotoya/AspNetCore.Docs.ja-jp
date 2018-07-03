---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: ビュー マスター ページ (VB) でページ レイアウトを作成 |Microsoft Docs
author: microsoft
description: このチュートリアルでは、ビュー マスター ページを利用して、アプリケーションで複数のページの一般的なページ レイアウトを作成する方法について説明します。 使用することができますをしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: eb3fb0759ac6511b1d60045a653f509df7810255
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375503"
---
<a name="creating-page-layouts-with-view-master-pages-vb"></a><span data-ttu-id="80eb9-104">ビュー マスター ページ (VB) でページ レイアウトを作成</span><span class="sxs-lookup"><span data-stu-id="80eb9-104">Creating Page Layouts with View Master Pages (VB)</span></span>
====================
<span data-ttu-id="80eb9-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="80eb9-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="80eb9-106">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="80eb9-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> <span data-ttu-id="80eb9-107">このチュートリアルでは、ビュー マスター ページを利用して、アプリケーションで複数のページの一般的なページ レイアウトを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-107">In this tutorial, you learn how to create a common page layout for multiple pages in your application by taking advantage of view master pages.</span></span> <span data-ttu-id="80eb9-108">たとえば、ビュー マスター ページを使用する 2 つの列 ページのレイアウトを定義して、web アプリケーション内のページのすべての 2 つの列のレイアウトを使用します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-108">You can use a view master page, for example, to define a two-column page layout and use the two-column layout for all of the pages in your web application.</span></span>


## <a name="creating-page-layouts-with-view-master-pages"></a><span data-ttu-id="80eb9-109">ビュー マスター ページとページ レイアウトを作成します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-109">Creating Page Layouts with View Master Pages</span></span>

<span data-ttu-id="80eb9-110">このチュートリアルでは、ビュー マスター ページを利用して、アプリケーションで複数のページの一般的なページ レイアウトを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-110">In this tutorial, you learn how to create a common page layout for multiple pages in your application by taking advantage of view master pages.</span></span> <span data-ttu-id="80eb9-111">たとえば、ビュー マスター ページを使用する 2 つの列 ページのレイアウトを定義して、web アプリケーション内のページのすべての 2 つの列のレイアウトを使用します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-111">You can use a view master page, for example, to define a two-column page layout and use the two-column layout for all of the pages in your web application.</span></span>

<span data-ttu-id="80eb9-112">利用したもできるビューのマスター ページ、アプリケーションで複数のページ間で共通のコンテンツを共有します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-112">You also can take advantage of view master pages to share common content across multiple pages in your application.</span></span> <span data-ttu-id="80eb9-113">たとえば、ビュー マスター ページで、web サイトのロゴ、ナビゲーション リンク、およびバナー広告を配置できます。</span><span class="sxs-lookup"><span data-stu-id="80eb9-113">For example, you can place your website logo, navigation links, and banner advertisements in a view master page.</span></span> <span data-ttu-id="80eb9-114">これにより、アプリケーション内の各ページはこのコンテンツを自動的に表示します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-114">That way, every page in your application would display this content automatically.</span></span>

<span data-ttu-id="80eb9-115">このチュートリアルでは、新しいビュー マスター ページを作成し、マスター ページに基づいて、新しいビュー コンテンツ ページを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-115">In this tutorial, you learn how to create a new view master page and create a new view content page based on the master page.</span></span>

### <a name="creating-a-view-master-page"></a><span data-ttu-id="80eb9-116">ビュー マスター ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-116">Creating a View Master Page</span></span>

<span data-ttu-id="80eb9-117">2 つの列のレイアウトを定義するビュー マスター ページを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="80eb9-117">Let's start by creating a view master page that defines a two-column layout.</span></span> <span data-ttu-id="80eb9-118">追加する新しいビュー マスター ページ、MVC プロジェクトを views \shared フォルダーを右クリックしてメニュー オプションを選択する**追加]、[新しい項目の**、MVC ビュー マスター ページ テンプレートを選択して (図 1 参照)。</span><span class="sxs-lookup"><span data-stu-id="80eb9-118">You add a new view master page to an MVC project by right-clicking the Views\Shared folder, selecting the menu option **Add, New Item**, and selecting the  MVC View Master Page template (see Figure 1).</span></span>


<span data-ttu-id="80eb9-119">[![ビュー マスター ページを追加します。](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="80eb9-119">[![Adding a view master page](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)</span></span>

<span data-ttu-id="80eb9-120">**図 01**: ビュー マスター ページを追加する ([フルサイズの画像を表示する をクリックします](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="80eb9-120">**Figure 01**: Adding a view master page ([Click to view full-size image](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))</span></span>


<span data-ttu-id="80eb9-121">アプリケーションでは、1 つ以上のビュー マスター ページを作成できます。</span><span class="sxs-lookup"><span data-stu-id="80eb9-121">You can create more than one view master page in an application.</span></span> <span data-ttu-id="80eb9-122">各ビュー マスター ページには、別のページ レイアウトを定義できます。</span><span class="sxs-lookup"><span data-stu-id="80eb9-122">Each view master page can define a different page layout.</span></span> <span data-ttu-id="80eb9-123">たとえば、特定のページが 2 つの列のレイアウトおよびその他のページ 3 列のレイアウトをたい場合があります。</span><span class="sxs-lookup"><span data-stu-id="80eb9-123">For example, you might want certain pages to have a two-column layout and other pages to have a three-column layout.</span></span>

<span data-ttu-id="80eb9-124">ビュー マスター ページの外観を標準の ASP.NET MVC ビューとよく似ています。</span><span class="sxs-lookup"><span data-stu-id="80eb9-124">A view master page looks very much like a standard ASP.NET MVC view.</span></span> <span data-ttu-id="80eb9-125">ただし、通常のビューとは異なり、ビュー マスター ページには 1 つまたは複数`<asp:ContentPlaceHolder>`タグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-125">However, unlike a normal view, a view master page contains one or more `<asp:ContentPlaceHolder>` tags.</span></span> <span data-ttu-id="80eb9-126">`<contentplaceholder>`タグを使用して、個々 のコンテンツ ページでオーバーライドできるマスター ページの領域をマークします。</span><span class="sxs-lookup"><span data-stu-id="80eb9-126">The `<contentplaceholder>` tags are used to mark the areas of the master page that can be overridden in an individual content page.</span></span>

<span data-ttu-id="80eb9-127">ビュー マスター ページ リスト 1 では、2 つの列のレイアウトを定義します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-127">For example, the view master page in Listing 1 defines a two-column layout.</span></span> <span data-ttu-id="80eb9-128">2 つが含まれている`<contentplaceholder>`タグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-128">It contains two `<contentplaceholder>` tags.</span></span> <span data-ttu-id="80eb9-129">1 つ`<ContentPlaceHolder>`列ごとにします。</span><span class="sxs-lookup"><span data-stu-id="80eb9-129">One `<ContentPlaceHolder>` for each column.</span></span>

<span data-ttu-id="80eb9-130">**1 – を一覧表示します。 `Views\Shared\Site.master`**</span><span class="sxs-lookup"><span data-stu-id="80eb9-130">**Listing 1 – `Views\Shared\Site.master`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

<span data-ttu-id="80eb9-131">リスト 1 でマスター ページには 2 つのビューの本文`<div>`2 つの列に対応するタグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-131">The body of the view master page in Listing 1 contains two `<div>` tags that correspond to the two columns.</span></span> <span data-ttu-id="80eb9-132">列のカスケード スタイル シートのクラスは両方に適用`<div>`タグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-132">The Cascading Style Sheet column class is applied to both `<div>` tags.</span></span> <span data-ttu-id="80eb9-133">このクラスは、マスター ページの上部で宣言されているスタイル シートで定義されます。</span><span class="sxs-lookup"><span data-stu-id="80eb9-133">This class is defined in the style sheet declared at the top of the master page.</span></span> <span data-ttu-id="80eb9-134">デザイン ビューに切り替えることで、ビュー マスター ページを表示する方法をプレビューすることができます。</span><span class="sxs-lookup"><span data-stu-id="80eb9-134">You can preview how the view master page will be rendered by switching to Design view.</span></span> <span data-ttu-id="80eb9-135">ソース コード エディターの左下にある [デザイン] タブをクリックします (図 2 参照)。</span><span class="sxs-lookup"><span data-stu-id="80eb9-135">Click the Design tab at the bottom-left of the source code editor (see Figure 2).</span></span>


<span data-ttu-id="80eb9-136">[![デザイナーでマスター ページのプレビュー](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="80eb9-136">[![Previewing a master page in the designer](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)</span></span>

<span data-ttu-id="80eb9-137">**図 02**: マスター ページ、デザイナーのプレビュー ([フルサイズの画像を表示する をクリックします](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="80eb9-137">**Figure 02**: Previewing a master page in the designer ([Click to view full-size image](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))</span></span>


### <a name="creating-a-view-content-page"></a><span data-ttu-id="80eb9-138">ビュー コンテンツ ページの作成</span><span class="sxs-lookup"><span data-stu-id="80eb9-138">Creating a View Content Page</span></span>

<span data-ttu-id="80eb9-139">ビュー マスター ページを作成した後は、1 つまたは複数のビューにビュー マスター ページに基づくコンテンツ ページを作成できます。</span><span class="sxs-lookup"><span data-stu-id="80eb9-139">After you create a view master page, you can create one or more view content pages based on the view master page.</span></span> <span data-ttu-id="80eb9-140">Views \home フォルダーを右クリックして、ホーム コント ローラーの Index ビュー コンテンツ ページを作成するなど、選択**追加、新しい項目の**選択、 **MVC ビュー コンテンツ ページ**入力テンプレート(図 3 参照) にボタン名 Index.aspx、したりの追加 をクリックします。</span><span class="sxs-lookup"><span data-stu-id="80eb9-140">For example, you can create an Index view content page for the Home controller by right-clicking the Views\Home folder, selecting **Add, New Item**, selecting the **MVC View Content Page** template, entering the name Index.aspx, and clicking the Add button (see Figure 3).</span></span>


<span data-ttu-id="80eb9-141">[![ビュー コンテンツ ページを追加します。](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="80eb9-141">[![Adding a view content page](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)</span></span>

<span data-ttu-id="80eb9-142">**図 03**: ビュー コンテンツ ページの追加 ([フルサイズの画像を表示する をクリックします](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))。</span><span class="sxs-lookup"><span data-stu-id="80eb9-142">**Figure 03**: Adding a view content page ([Click to view full-size image](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))</span></span>


<span data-ttu-id="80eb9-143">[追加] ボタンをクリックした後はビュー コンテンツ ページに関連付けられたビュー マスター ページを選択することができますの新しいダイアログが表示されます (図 4 参照)。</span><span class="sxs-lookup"><span data-stu-id="80eb9-143">After you click the Add button, a new dialog appears that enables you to select a view master page to associate with the view content page (see Figure 4).</span></span> <span data-ttu-id="80eb9-144">前のセクションで作成した Site.master ビュー マスター ページに移動することができます。</span><span class="sxs-lookup"><span data-stu-id="80eb9-144">You can navigate to the Site.master view master page that we created in the previous section.</span></span>


<span data-ttu-id="80eb9-145">[![マスター ページの選択](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="80eb9-145">[![Selecting a master page](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)</span></span>

<span data-ttu-id="80eb9-146">**図 04**: マスター ページの選択 ([フルサイズの画像を表示する をクリックします](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))。</span><span class="sxs-lookup"><span data-stu-id="80eb9-146">**Figure 04**: Selecting a master page ([Click to view full-size image](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))</span></span>


<span data-ttu-id="80eb9-147">Site.master マスター ページに基づいて、新しいビュー コンテンツ ページを作成した後は、リスト 2 でファイルを取得します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-147">After you create a new view content page based on the Site.master master page, you get the file in Listing 2.</span></span>

<span data-ttu-id="80eb9-148">**2 – を一覧表示します。 `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="80eb9-148">**Listing 2 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

<span data-ttu-id="80eb9-149">このビューが含まれていますが、`<asp:Content>`のそれぞれに対応するタグ、`<asp:ContentPlaceHolder>`ビュー マスター ページ内のタグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-149">Notice that this view contains a `<asp:Content>` tag that corresponds to each of the `<asp:ContentPlaceHolder>` tags in the view master page.</span></span> <span data-ttu-id="80eb9-150">各`<asp:Content>`タグには、特定を指す ContentPlaceHolderID 属性が含まれています。 `<asp:ContentPlaceHolder>` 、オーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="80eb9-150">Each `<asp:Content>` tag includes a ContentPlaceHolderID attribute that points to the particular `<asp:ContentPlaceHolder>` that it overrides.</span></span>

<span data-ttu-id="80eb9-151">さらに、リスト 2 でのコンテンツ ビュー ページは含まれていないこと、通常の開始タグと HTML 終了タグのいずれかに注意してください。</span><span class="sxs-lookup"><span data-stu-id="80eb9-151">Notice, furthermore, that the content view page in Listing 2 does not contain any of the normal opening and closing HTML tags.</span></span> <span data-ttu-id="80eb9-152">たとえば、含まないタグと終了`<html>`または`<head>`タグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-152">For example, it does not contain the opening and closing `<html>` or `<head>` tags.</span></span> <span data-ttu-id="80eb9-153">ビュー マスター ページのすべての標準タグと終了タグに格納されます。</span><span class="sxs-lookup"><span data-stu-id="80eb9-153">All of the normal opening and closing tags are contained in the view master page.</span></span>

<span data-ttu-id="80eb9-154">内のビュー コンテンツ ページに表示するコンテンツを配置する必要があります、`<asp:Content>`タグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-154">Any content that you want to display in a view content page must be placed within a `<asp:Content>` tag.</span></span> <span data-ttu-id="80eb9-155">任意の HTML またはこれらのタグの外部では、その他のコンテンツを配置する場合、ページを表示しようとしたときにエラーを取得は。</span><span class="sxs-lookup"><span data-stu-id="80eb9-155">If you place any HTML or other content outside of these tags, then you will get an error when you attempt to view the page.</span></span>

<span data-ttu-id="80eb9-156">オーバーライドする必要はありませんすべて`<asp:ContentPlaceHolder>`コンテンツ ビュー ページでマスター ページからのタグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-156">You don't need to override every `<asp:ContentPlaceHolder>` tag from a master page in a content view page.</span></span> <span data-ttu-id="80eb9-157">だけをオーバーライドする必要があります、`<asp:ContentPlaceHolder>`特定のコンテンツのタグで置換するときにタグ付けします。</span><span class="sxs-lookup"><span data-stu-id="80eb9-157">You only need to override a `<asp:ContentPlaceHolder>` tag when you want to replace the tag with particular content.</span></span>

<span data-ttu-id="80eb9-158">たとえば、リスト 3 に変更したのインデックス ビューには 2 つのみ`<asp:Content>`タグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-158">For example, the modified Index view in Listing 3 contains only two `<asp:Content>` tags.</span></span> <span data-ttu-id="80eb9-159">各、`<asp:Content>`タグにいくつかのテキストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="80eb9-159">Each of the `<asp:Content>` tags includes some text.</span></span>

<span data-ttu-id="80eb9-160">**3 – を一覧表示します。 `Views\Home\Index.aspx (modified)`**</span><span class="sxs-lookup"><span data-stu-id="80eb9-160">**Listing 3 – `Views\Home\Index.aspx (modified)`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

<span data-ttu-id="80eb9-161">リスト 3 でビューが要求されると、図 5 ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-161">When the view in Listing 3 is requested, it renders the page in Figure 5.</span></span> <span data-ttu-id="80eb9-162">ビューが 2 つの列でページを表示することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="80eb9-162">Notice that the view renders a page with two columns.</span></span> <span data-ttu-id="80eb9-163">さらに、ビュー マスター ページからコンテンツを持つビュー コンテンツ ページからコンテンツがマージされることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="80eb9-163">Notice, furthermore, that the content from the view content page is merged with the content from the view master page.</span></span>


<span data-ttu-id="80eb9-164">[![インデックス ビューのコンテンツ ページ](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="80eb9-164">[![The Index view content page](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)</span></span>

<span data-ttu-id="80eb9-165">**図 05**: インデックス ビューのコンテンツ ページ ([フルサイズの画像を表示する をクリックします](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))。</span><span class="sxs-lookup"><span data-stu-id="80eb9-165">**Figure 05**: The Index view content page ([Click to view full-size image](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))</span></span>


### <a name="modifying-view-master-page-content"></a><span data-ttu-id="80eb9-166">ビュー マスター ページのコンテンツを変更します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-166">Modifying View Master Page Content</span></span>

<span data-ttu-id="80eb9-167">ビュー マスター ページのコンテンツを別のビュー コンテンツ ページが要求されたときに変更の問題としては、マスター ページの表示を使用する場合にすぐにほぼが発生した問題の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="80eb9-167">One issue that you encounter almost immediately when working with view master pages is the problem of modifying view master page content when different view content pages are requested.</span></span> <span data-ttu-id="80eb9-168">たとえば、各ページで web アプリケーションに固有のタイトルがします。</span><span class="sxs-lookup"><span data-stu-id="80eb9-168">For example, you want each page in your web application to have a unique title.</span></span> <span data-ttu-id="80eb9-169">ただし、ビュー マスター ページで、ビューのコンテンツ ページではなく、タイトルが宣言されています。</span><span class="sxs-lookup"><span data-stu-id="80eb9-169">However, the title is declared in the view master page and not in the view content page.</span></span> <span data-ttu-id="80eb9-170">そのため、各ビューのコンテンツ ページのページ タイトルをカスタマイズする方法でしょうか。</span><span class="sxs-lookup"><span data-stu-id="80eb9-170">So, how do you customize the page title for each view content page?</span></span>

<span data-ttu-id="80eb9-171">ビューのコンテンツ ページが表示されるタイトルを変更できる 2 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="80eb9-171">There are two ways that you can modify the title displayed by a view content page.</span></span> <span data-ttu-id="80eb9-172">最初に、ページのタイトルの title 属性を割り当てることができます、`<%@ page %>`ビュー コンテンツ ページの上部にあるディレクティブが宣言されています。</span><span class="sxs-lookup"><span data-stu-id="80eb9-172">First, you can assign a page title to the title attribute of the `<%@ page %>` directive declared at the top of a view content page.</span></span> <span data-ttu-id="80eb9-173">たとえば、インデックス ビューにページ タイトル「スーパー優れた web サイト」を割り当てる場合は、Index ビューの上部にある次のディレクティブを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="80eb9-173">For example, if you want to assign the page title "Super Great Website" to the Index view, then you can include the following directive at the top of the Index view:</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

<span data-ttu-id="80eb9-174">インデックス ビューがブラウザーに表示されると、ブラウザーのタイトル バーに目的のタイトルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80eb9-174">When the Index view is rendered to the browser, the desired title appears in the browser title bar:</span></span>


<span data-ttu-id="80eb9-175">[![ブラウザーのタイトル バー](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="80eb9-175">[![Browser title bar](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)</span></span>


<span data-ttu-id="80eb9-176">マスター ビュー ページが動作する、title 属性の順序で満たす必要がある 1 つの重要な要件があります。</span><span class="sxs-lookup"><span data-stu-id="80eb9-176">There is one important requirement that a master view page must satisfy in order for the title attribute to work.</span></span> <span data-ttu-id="80eb9-177">ビュー マスター ページに含める必要があります、`<head runat="server">`タグは、通常ではなく`<head>`のヘッダーのタグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-177">The view master page must contain a `<head runat="server">` tag instead of a normal `<head>` tag for its header.</span></span> <span data-ttu-id="80eb9-178">場合、`<head>`タグに runat は含まれません ="server"属性、タイトルは表示されません。</span><span class="sxs-lookup"><span data-stu-id="80eb9-178">If the `<head>` tag does not include the runat="server" attribute then the title won't appear.</span></span> <span data-ttu-id="80eb9-179">マスター ページが含まれていますが、必要な既定のビュー`<head runat="server">`タグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-179">The default view master page includes the required `<head runat="server">` tag.</span></span>

<span data-ttu-id="80eb9-180">変更する地域をラップするという、個々 のビュー コンテンツ ページからマスター ページのコンテンツを変更する方法、`<asp:ContentPlaceHolder>`タグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-180">An alternative approach to modifying master page content from an individual view content page is to wrap the region that you want to modify in a `<asp:ContentPlaceHolder>` tag.</span></span> <span data-ttu-id="80eb9-181">たとえば、タイトルだけでなく、マスター ビュー ページで表示される、メタ タグを変更することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="80eb9-181">For example, imagine that you want to change not only the title, but also the meta tags, rendered by a master view page.</span></span> <span data-ttu-id="80eb9-182">リスト 4 のマスター ビュー ページには、`<asp:ContentPlaceHolder>`タグ内でその`<head>`タグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-182">The master view page in Listing 4 contains a `<asp:ContentPlaceHolder>` tag within its `<head>` tag.</span></span>

<span data-ttu-id="80eb9-183">**4 – を一覧表示します。 `Views\Shared\Site2.master`**</span><span class="sxs-lookup"><span data-stu-id="80eb9-183">**Listing 4 – `Views\Shared\Site2.master`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

<span data-ttu-id="80eb9-184">注意、`<asp:ContentPlaceHolder>`リスト 4 のタグには、既定のコンテンツが含まれています: 既定のタイトルと既定のメタ タグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-184">Notice that the `<asp:ContentPlaceHolder>` tag in Listing 4 includes default content: a default title and default meta tags.</span></span> <span data-ttu-id="80eb9-185">これを上書きしない場合`<asp:ContentPlaceHolder>`タグを個々 のビューのコンテンツ ページで、既定のコンテンツが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80eb9-185">If you don't override this `<asp:ContentPlaceHolder>` tag in an individual view content page, then the default content will be displayed.</span></span>

<span data-ttu-id="80eb9-186">リスト 5 でコンテンツの表示 ページの上書き、`<asp:ContentPlaceHolder>`カスタム タイトルとカスタム メタ タグを表示するにはタグ。</span><span class="sxs-lookup"><span data-stu-id="80eb9-186">The content view page in Listing 5 overrides the `<asp:ContentPlaceHolder>` tag in order to display a custom title and custom meta tags.</span></span>

<span data-ttu-id="80eb9-187">**5 – を一覧表示します。 `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="80eb9-187">**Listing 5 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a><span data-ttu-id="80eb9-188">まとめ</span><span class="sxs-lookup"><span data-stu-id="80eb9-188">Summary</span></span>

<span data-ttu-id="80eb9-189">このチュートリアルは、マスター ページを表示し、コンテンツ ページを表示するための基本的な概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-189">This tutorial provided you with a basic introduction to view master pages and view content pages.</span></span> <span data-ttu-id="80eb9-190">新しいビュー マスター ページを作成し、それらに基づくコンテンツ ページの表示を作成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="80eb9-190">You learned how to create new view master pages and create view content pages based on them.</span></span> <span data-ttu-id="80eb9-191">特定のビュー コンテンツ ページからビュー マスター ページの内容を変更する方法についても確認します。</span><span class="sxs-lookup"><span data-stu-id="80eb9-191">We also examined how you can modify the content of a view master page from a particular view content page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="80eb9-192">[前へ](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [次へ](passing-data-to-view-master-pages-vb.md)</span><span class="sxs-lookup"><span data-stu-id="80eb9-192">[Previous](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
[Next](passing-data-to-view-master-pages-vb.md)</span></span>
