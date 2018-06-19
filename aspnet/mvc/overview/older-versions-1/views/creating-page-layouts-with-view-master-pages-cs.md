---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: ビュー マスター ページ (c#) のページ レイアウトの作成 |Microsoft ドキュメント
author: microsoft
description: このチュートリアルでは、ビュー マスター ページを利用して、アプリケーションで複数のページに共通のページ レイアウトを作成する方法を学習します。 使用することができます、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 82500a311e1110713a60604027d018ba16539b65
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871247"
---
<a name="creating-page-layouts-with-view-master-pages-c"></a><span data-ttu-id="cef8e-104">ビュー マスター ページ (c#) のページ レイアウトの作成</span><span class="sxs-lookup"><span data-stu-id="cef8e-104">Creating Page Layouts with View Master Pages (C#)</span></span>
====================
<span data-ttu-id="cef8e-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="cef8e-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="cef8e-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="cef8e-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> <span data-ttu-id="cef8e-107">このチュートリアルでは、ビュー マスター ページを利用して、アプリケーションで複数のページに共通のページ レイアウトを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-107">In this tutorial, you learn how to create a common page layout for multiple pages in your application by taking advantage of view master pages.</span></span> <span data-ttu-id="cef8e-108">たとえば、ビュー マスター ページを使用する 2 つの列のページ レイアウトを定義し、2 つの列レイアウトのすべての web アプリケーションでページを使用します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-108">You can use a view master page, for example, to define a two-column page layout and use the two-column layout for all of the pages in your web application.</span></span>


## <a name="creating-page-layouts-with-view-master-pages"></a><span data-ttu-id="cef8e-109">ビュー マスター ページのページ レイアウトの作成</span><span class="sxs-lookup"><span data-stu-id="cef8e-109">Creating Page Layouts with View Master Pages</span></span>

<span data-ttu-id="cef8e-110">このチュートリアルでは、ビュー マスター ページを利用して、アプリケーションで複数のページに共通のページ レイアウトを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-110">In this tutorial, you learn how to create a common page layout for multiple pages in your application by taking advantage of view master pages.</span></span> <span data-ttu-id="cef8e-111">たとえば、ビュー マスター ページを使用する 2 つの列のページ レイアウトを定義し、2 つの列レイアウトのすべての web アプリケーションでページを使用します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-111">You can use a view master page, for example, to define a two-column page layout and use the two-column layout for all of the pages in your web application.</span></span>

<span data-ttu-id="cef8e-112">利用できますビューのマスター ページ、アプリケーションで複数のページ間での一般的なコンテンツを共有します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-112">You also can take advantage of view master pages to share common content across multiple pages in your application.</span></span> <span data-ttu-id="cef8e-113">たとえば、ビュー マスター ページで、web サイトのロゴ、ナビゲーション リンク、および著作権情報の提供情報を配置できます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-113">For example, you can place your website logo, navigation links, and banner advertisements in a view master page.</span></span> <span data-ttu-id="cef8e-114">このように、アプリケーション内の各ページはこのコンテンツを自動的に表示します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-114">That way, every page in your application would display this content automatically.</span></span>

<span data-ttu-id="cef8e-115">このチュートリアルでは、新しいビュー マスター ページを作成し、新しいビューに基づいてコンテンツ ページ、マスター ページを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-115">In this tutorial, you learn how to create a new view master page and create a new view content page based on the master page.</span></span>

### <a name="creating-a-view-master-page"></a><span data-ttu-id="cef8e-116">ビュー マスター ページの作成</span><span class="sxs-lookup"><span data-stu-id="cef8e-116">Creating a View Master Page</span></span>

<span data-ttu-id="cef8e-117">2 つの列のレイアウトを定義するビュー マスター ページの作成を始めます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-117">Let's start by creating a view master page that defines a two-column layout.</span></span> <span data-ttu-id="cef8e-118">追加する新しいビュー マスター ページ、MVC プロジェクトを \shared フォルダーを右クリックしてメニュー オプションを選択する**追加]、[新しい項目の**を選択して、 **MVC ビュー マスター ページ**テンプレート (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="cef8e-118">You add a new view master page to an MVC project by right-clicking the Views\Shared folder, selecting the menu option **Add, New Item**, and selecting the **MVC View Master Page** template (see Figure 1).</span></span>


<span data-ttu-id="cef8e-119">[![ビュー マスター ページを追加します。](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cef8e-119">[![Adding a view master page](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)</span></span>

<span data-ttu-id="cef8e-120">**図 01**: ビュー マスター ページを追加する ([フルサイズのイメージを表示するをクリックして](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="cef8e-120">**Figure 01**: Adding a view master page ([Click to view full-size image](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))</span></span>


<span data-ttu-id="cef8e-121">アプリケーションでは、1 つ以上のビュー マスター ページを作成できます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-121">You can create more than one view master page in an application.</span></span> <span data-ttu-id="cef8e-122">各ビュー マスター ページには、別のページ レイアウトを定義できます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-122">Each view master page can define a different page layout.</span></span> <span data-ttu-id="cef8e-123">たとえば、2 つの列のレイアウトに特定のページおよびその他のページ 3 列のレイアウトをする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cef8e-123">For example, you might want certain pages to have a two-column layout and other pages to have a three-column layout.</span></span>

<span data-ttu-id="cef8e-124">ビュー マスター ページの外観標準の ASP.NET MVC ビューとよく似ています。</span><span class="sxs-lookup"><span data-stu-id="cef8e-124">A view master page looks very much like a standard ASP.NET MVC view.</span></span> <span data-ttu-id="cef8e-125">ただし、通常のビューとは異なりビュー マスター ページに含まれる 1 つまたは複数`<asp:ContentPlaceHolder>`タグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-125">However, unlike a normal view, a view master page contains one or more `<asp:ContentPlaceHolder>` tags.</span></span> <span data-ttu-id="cef8e-126">`<contentplaceholder>`タグはコンテンツの個別のページでオーバーライド可能なマスター ページの領域を示すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-126">The `<contentplaceholder>` tags are used to mark the areas of the master page that can be overridden in an individual content page.</span></span>

<span data-ttu-id="cef8e-127">たとえば、ビュー マスター ページ 1 の一覧表示するのには、2 列のレイアウトを定義します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-127">For example, the view master page in Listing 1 defines a two-column layout.</span></span> <span data-ttu-id="cef8e-128">2 つが含まれている`<contentplaceholder>`タグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-128">It contains two `<contentplaceholder>` tags.</span></span> <span data-ttu-id="cef8e-129">1 つ`<ContentPlaceHolder>`列ごとにします。</span><span class="sxs-lookup"><span data-stu-id="cef8e-129">One `<ContentPlaceHolder>` for each column.</span></span>

<span data-ttu-id="cef8e-130">**1 – を一覧表示します。 `Views\Shared\Site.master`**</span><span class="sxs-lookup"><span data-stu-id="cef8e-130">**Listing 1 – `Views\Shared\Site.master`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

<span data-ttu-id="cef8e-131">マスター ページ 1 の一覧表示するには 2 つのビューの本文`<div>`2 つの列に対応するタグです。</span><span class="sxs-lookup"><span data-stu-id="cef8e-131">The body of the view master page in Listing 1 contains two `<div>` tags that correspond to the two columns.</span></span> <span data-ttu-id="cef8e-132">カスケード スタイル シート列クラスは、両方に適用`<div>`タグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-132">The Cascading Style Sheet column class is applied to both `<div>` tags.</span></span> <span data-ttu-id="cef8e-133">このクラスは、マスター ページの上部で宣言されているスタイル シートで定義されます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-133">This class is defined in the style sheet declared at the top of the master page.</span></span> <span data-ttu-id="cef8e-134">デザイン ビューに切り替えることによって、ビュー マスター ページのレンダリング方法をプレビューすることができます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-134">You can preview how the view master page will be rendered by switching to Design view.</span></span> <span data-ttu-id="cef8e-135">ソース コード エディターの左下に [デザイン] タブをクリックして (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="cef8e-135">Click the Design tab at the bottom-left of the source code editor (see Figure 2).</span></span>


<span data-ttu-id="cef8e-136">[![デザイナー内のマスター ページをプレビューします。](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="cef8e-136">[![Previewing a master page in the designer](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)</span></span>

<span data-ttu-id="cef8e-137">**図 02**: マスター ページ、デザイナーでのプレビュー ([フルサイズのイメージを表示するをクリックして](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="cef8e-137">**Figure 02**: Previewing a master page in the designer ([Click to view full-size image](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))</span></span>


### <a name="creating-a-view-content-page"></a><span data-ttu-id="cef8e-138">ビューのコンテンツ ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-138">Creating a View Content Page</span></span>

<span data-ttu-id="cef8e-139">ビュー マスター ページを作成した後は、1 つまたは複数のビューに、ビュー マスター ページに基づくコンテンツ ページを作成できます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-139">After you create a view master page, you can create one or more view content pages based on the view master page.</span></span> <span data-ttu-id="cef8e-140">Views \home フォルダーを右クリックして、Home コント ローラーのインデックス ビュー コンテンツ ページを作成するを選択すると**追加、新しい項目の**を選択すると、 **MVC ビューのコンテンツ ページ**テンプレートを入力します。Index.aspx、名前をクリックし、**追加**(図 3 を参照してください) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cef8e-140">For example, you can create an Index view content page for the Home controller by right-clicking the Views\Home folder, selecting **Add, New Item**, selecting the **MVC View Content Page** template, entering the name Index.aspx, and clicking the **Add** button (see Figure 3).</span></span>


<span data-ttu-id="cef8e-141">[![ビューのコンテンツ ページを追加します。](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="cef8e-141">[![Adding a view content page](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)</span></span>

<span data-ttu-id="cef8e-142">**図 03**: ビューのコンテンツ ページを追加する ([フルサイズのイメージを表示するをクリックして](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="cef8e-142">**Figure 03**: Adding a view content page ([Click to view full-size image](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))</span></span>


<span data-ttu-id="cef8e-143">[追加] ボタンをクリックした後、新しいダイアログが表示されますビューのコンテンツ ページに関連付けられたビュー マスター ページを選択することができます (図 4 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="cef8e-143">After you click the Add button, a new dialog appears that enables you to select a view master page to associate with the view content page (see Figure 4).</span></span> <span data-ttu-id="cef8e-144">前のセクションで作成した Site.master ビュー マスター ページに移動することができます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-144">You can navigate to the Site.master view master page that we created in the previous section.</span></span>


<span data-ttu-id="cef8e-145">[![マスター ページの選択](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="cef8e-145">[![Selecting a master page](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)</span></span>

<span data-ttu-id="cef8e-146">**図 04**: マスター ページの選択 ([フルサイズのイメージを表示するをクリックして](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="cef8e-146">**Figure 04**: Selecting a master page ([Click to view full-size image](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))</span></span>


<span data-ttu-id="cef8e-147">Site.master マスター ページに基づいて、新しいビュー コンテンツ ページを作成した後は、2 の一覧でファイルを取得します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-147">After you create a new view content page based on the Site.master master page, you get the file in Listing 2.</span></span>

<span data-ttu-id="cef8e-148">**2 – を一覧表示します。 `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="cef8e-148">**Listing 2 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

<span data-ttu-id="cef8e-149">このビューに表示される、`<asp:Content>`のそれぞれに対応するタグ、`<asp:ContentPlaceHolder>`ビュー マスター ページ内のタグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-149">Notice that this view contains a `<asp:Content>` tag that corresponds to each of the `<asp:ContentPlaceHolder>` tags in the view master page.</span></span> <span data-ttu-id="cef8e-150">各`<asp:Content>`タグには、特定を指す ContentPlaceHolderID 属性が含まれています。`<asp:ContentPlaceHolder>`オーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="cef8e-150">Each `<asp:Content>` tag includes a ContentPlaceHolderID attribute that points to the particular `<asp:ContentPlaceHolder>` that it overrides.</span></span>

<span data-ttu-id="cef8e-151">さらに、2 のリスト内のコンテンツ ビュー ページは含まれていないこと、通常のタグと終了 HTML タグのいずれかに注意してください。</span><span class="sxs-lookup"><span data-stu-id="cef8e-151">Notice, furthermore, that the content view page in Listing 2 does not contain any of the normal opening and closing HTML tags.</span></span> <span data-ttu-id="cef8e-152">たとえばを含まないタグと終了`<html>`または`<head>`タグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-152">For example, it does not contain the opening and closing `<html>` or `<head>` tags.</span></span> <span data-ttu-id="cef8e-153">ビュー マスター ページでは、通常タグと終了タグのすべてが含まれます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-153">All of the normal opening and closing tags are contained in the view master page.</span></span>

<span data-ttu-id="cef8e-154">ビューのコンテンツ ページに表示するすべてのコンテンツ内に配置する必要があります、`<asp:Content>`タグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-154">Any content that you want to display in a view content page must be placed within a `<asp:Content>` tag.</span></span> <span data-ttu-id="cef8e-155">場合は、任意の HTML またはこれらのタグの外部では、他のコンテンツを配置すると、ページを表示しようとしたときにエラーが取得されます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-155">If you place any HTML or other content outside of these tags, then you will get an error when you attempt to view the page.</span></span>

<span data-ttu-id="cef8e-156">オーバーライドする必要はありませんすべて`<asp:ContentPlaceHolder>`コンテンツ ビュー ページで、マスター ページからのタグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-156">You don't need to override every `<asp:ContentPlaceHolder>` tag from a master page in a content view page.</span></span> <span data-ttu-id="cef8e-157">のみをオーバーライドする必要があります、`<asp:ContentPlaceHolder>`特定のコンテンツのタグで置換するときにタグ付けします。</span><span class="sxs-lookup"><span data-stu-id="cef8e-157">You only need to override a `<asp:ContentPlaceHolder>` tag when you want to replace the tag with particular content.</span></span>

<span data-ttu-id="cef8e-158">たとえば、3 の一覧表示するインデックス ビューには 2 つのみ`<asp:Content>`タグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-158">For example, the modified Index view in Listing 3 contains only two `<asp:Content>` tags.</span></span> <span data-ttu-id="cef8e-159">各、`<asp:Content>`タグには、いくつかのテキストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="cef8e-159">Each of the `<asp:Content>` tags includes some text.</span></span>

<span data-ttu-id="cef8e-160">**3 – を一覧表示します。 `Views\Home\Index.aspx (modified)`**</span><span class="sxs-lookup"><span data-stu-id="cef8e-160">**Listing 3 – `Views\Home\Index.aspx (modified)`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

<span data-ttu-id="cef8e-161">3 の一覧表示するビューが要求されたときに、図 5 にページを表示します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-161">When the view in Listing 3 is requested, it renders the page in Figure 5.</span></span> <span data-ttu-id="cef8e-162">ビューが 2 つの列を含むページを表示することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="cef8e-162">Notice that the view renders a page with two columns.</span></span> <span data-ttu-id="cef8e-163">注意してください、さらに、ビューのマスター ページからコンテンツを含むビューのコンテンツ ページの内容がマージされたこと</span><span class="sxs-lookup"><span data-stu-id="cef8e-163">Notice, furthermore, that the content from the view content page is merged with the content from the view master page</span></span>


<span data-ttu-id="cef8e-164">[![インデックス ビューのコンテンツ ページ](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="cef8e-164">[![The Index view content page](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)</span></span>

<span data-ttu-id="cef8e-165">**図 05**:、インデックス ビューのコンテンツ ページ ([フルサイズのイメージを表示するをクリックして](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="cef8e-165">**Figure 05**: The Index view content page ([Click to view full-size image](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))</span></span>


### <a name="modifying-view-master-page-content"></a><span data-ttu-id="cef8e-166">ビュー マスター ページの内容を変更します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-166">Modifying View Master Page Content</span></span>

<span data-ttu-id="cef8e-167">ほとんどが発生した問題の 1 つがマスター ページの表示を操作するときにすぐには別のビューのコンテンツ ページが要求されたときに、マスター ページ コンテンツの表示を変更した場合の問題です。</span><span class="sxs-lookup"><span data-stu-id="cef8e-167">One issue that you encounter almost immediately when working with view master pages is the problem of modifying view master page content when different view content pages are requested.</span></span> <span data-ttu-id="cef8e-168">たとえば、web アプリケーションに固有のタイトルがで各ページを希望します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-168">For example, you want each page in your web application to have a unique title.</span></span> <span data-ttu-id="cef8e-169">ただし、ビュー マスター ページでは、ビューのコンテンツ ページではなく、タイトルが宣言されています。</span><span class="sxs-lookup"><span data-stu-id="cef8e-169">However, the title is declared in the view master page and not in the view content page.</span></span> <span data-ttu-id="cef8e-170">そのため、各ビューのコンテンツ ページのページのタイトルをカスタマイズする方法ですか?</span><span class="sxs-lookup"><span data-stu-id="cef8e-170">So, how do you customize the page title for each view content page?</span></span>

<span data-ttu-id="cef8e-171">ビューのコンテンツ ページで表示されるタイトルを変更できる 2 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="cef8e-171">There are two ways that you can modify the title displayed by a view content page.</span></span> <span data-ttu-id="cef8e-172">ページ タイトルの title 属性を最初に、割り当てることができます、`<%@ page %>`ビューのコンテンツ ページの上部にあるディレクティブが宣言されています。</span><span class="sxs-lookup"><span data-stu-id="cef8e-172">First, you can assign a page title to the title attribute of the `<%@ page %>` directive declared at the top of a view content page.</span></span> <span data-ttu-id="cef8e-173">たとえば、インデックス ビューに、ページ タイトル「スーパー優れた web サイト」を割り当てる場合は、インデックス ビューの上部にある次のディレクティブを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-173">For example, if you want to assign the page title "Super Great Website" to the Index view, then you can include the following directive at the top of the Index view:</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

<span data-ttu-id="cef8e-174">インデックス ビューは、ブラウザーに表示される、目的のタイトルは、ブラウザーのタイトル バーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-174">When the Index view is rendered to the browser, the desired title appears in the browser title bar:</span></span>


<span data-ttu-id="cef8e-175">[![ブラウザーのタイトル バー](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="cef8e-175">[![Browser title bar](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)</span></span>


<span data-ttu-id="cef8e-176">マスター ビュー ページを使用するタイトル属性の順序で満たす必要がある 1 つの重要な要件です。</span><span class="sxs-lookup"><span data-stu-id="cef8e-176">There is one important requirement that a master view page must satisfy in order for the title attribute to work.</span></span> <span data-ttu-id="cef8e-177">ビュー マスター ページを含める必要があります、`<head runat="server">`通常ではなくタグ`<head>`ヘッダーのタグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-177">The view master page must contain a `<head runat="server">` tag instead of a normal `<head>` tag for its header.</span></span> <span data-ttu-id="cef8e-178">場合、`<head>`タグは、runat は含まれません ="server"属性タイトルは表示されません。</span><span class="sxs-lookup"><span data-stu-id="cef8e-178">If the `<head>` tag does not include the runat="server" attribute then the title won't appear.</span></span> <span data-ttu-id="cef8e-179">マスター ページが含まれていますが、必要な既定のビュー`<head runat="server">`タグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-179">The default view master page includes the required `<head runat="server">` tag.</span></span>

<span data-ttu-id="cef8e-180">個々 のビューのコンテンツ ページからマスター ページのコンテンツを変更する他の方法で変更する地域をラップする、`<asp:ContentPlaceHolder>`タグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-180">An alternative approach to modifying master page content from an individual view content page is to wrap the region that you want to modify in a `<asp:ContentPlaceHolder>` tag.</span></span> <span data-ttu-id="cef8e-181">たとえば、タイトル、だけでなく、マスター ビュー ページによってレンダリングされるメタ タグを変更することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="cef8e-181">For example, imagine that you want to change not only the title, but also the meta tags, rendered by a master view page.</span></span> <span data-ttu-id="cef8e-182">マスター ビュー ページ 4 の一覧表示するのには、`<asp:ContentPlaceHolder>`タグ内でその`<head>`タグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-182">The master view page in Listing 4 contains a `<asp:ContentPlaceHolder>` tag within its `<head>` tag.</span></span>

<span data-ttu-id="cef8e-183">**4 – を一覧表示します。 `Views\Shared\Site2.master`**</span><span class="sxs-lookup"><span data-stu-id="cef8e-183">**Listing 4 – `Views\Shared\Site2.master`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

<span data-ttu-id="cef8e-184">注意して、`<asp:ContentPlaceHolder>`リスト 4 内のタグには、既定のコンテンツが含まれています。 既定のタイトルと既定のメタ タグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-184">Notice that the `<asp:ContentPlaceHolder>` tag in Listing 4 includes default content: a default title and default meta tags.</span></span> <span data-ttu-id="cef8e-185">これをオーバーライドしない場合は`<asp:ContentPlaceHolder>`タグを個々 のビューのコンテンツ ページで既定のコンテンツが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cef8e-185">If you don't override this `<asp:ContentPlaceHolder>` tag in an individual view content page, then the default content will be displayed.</span></span>

<span data-ttu-id="cef8e-186">リスト 5 内のコンテンツ ビュー ページよりも優先、`<asp:ContentPlaceHolder>`カスタム タイトルとのカスタム メタ タグを表示するためにタグ。</span><span class="sxs-lookup"><span data-stu-id="cef8e-186">The content view page in Listing 5 overrides the `<asp:ContentPlaceHolder>` tag in order to display a custom title and custom meta tags.</span></span>

<span data-ttu-id="cef8e-187">**5 – を一覧表示します。 `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="cef8e-187">**Listing 5 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a><span data-ttu-id="cef8e-188">まとめ</span><span class="sxs-lookup"><span data-stu-id="cef8e-188">Summary</span></span>

<span data-ttu-id="cef8e-189">このチュートリアルは、マスター ページを表示し、コンテンツ ページを表示するための基本的な概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-189">This tutorial provided you with a basic introduction to view master pages and view content pages.</span></span> <span data-ttu-id="cef8e-190">新しいビュー マスター ページを作成し、それらに基づくコンテンツ ページの表示を作成する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="cef8e-190">You learned how to create new view master pages and create view content pages based on them.</span></span> <span data-ttu-id="cef8e-191">特定のビューのコンテンツ ページからビュー マスター ページの内容を変更する方法をも検討します。</span><span class="sxs-lookup"><span data-stu-id="cef8e-191">We also examined how you can modify the content of a view master page from a particular view content page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cef8e-192">[前へ](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [次へ](passing-data-to-view-master-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="cef8e-192">[Previous](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
[Next](passing-data-to-view-master-pages-cs.md)</span></span>
