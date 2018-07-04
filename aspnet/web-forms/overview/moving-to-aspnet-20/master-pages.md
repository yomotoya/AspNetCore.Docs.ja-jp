---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: マスター ページの |Microsoft Docs
author: microsoft
description: 成功した Web サイトの主なコンポーネントの 1 つは、一貫したルック アンド フィールです。 Asp.net 1.x では、開発者で、ページの一般的な elem のレプリケーションにユーザー コントロールが使用される.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: b31627fec45f153f5832afa6e317f2dd2b296d02
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382622"
---
<a name="master-pages"></a><span data-ttu-id="e31e3-104">マスター ページ</span><span class="sxs-lookup"><span data-stu-id="e31e3-104">Master Pages</span></span>
====================
<span data-ttu-id="e31e3-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e31e3-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="e31e3-106">成功した Web サイトの主なコンポーネントの 1 つは、一貫したルック アンド フィールです。</span><span class="sxs-lookup"><span data-stu-id="e31e3-106">One of the key components to a successful Web site is a consistent look and feel.</span></span> <span data-ttu-id="e31e3-107">Asp.net 1.x では、開発者で、Web アプリケーション全体で共通のページ要素をレプリケートするユーザー コントロールが使用されます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-107">In ASP.NET 1.x, developers used user controls to replicate common page elements across a Web application.</span></span> <span data-ttu-id="e31e3-108">有効な解決策ですが確かに、ユーザー コントロールを使用する欠点があります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-108">While that is certainly a workable solution, using user controls does have some drawbacks.</span></span> <span data-ttu-id="e31e3-109">たとえば、ユーザー コントロールの位置の変更は、サイト全体で複数のページへの変更が必要です。</span><span class="sxs-lookup"><span data-stu-id="e31e3-109">For example, a change in position of a user control requires a change to multiple pages across a site.</span></span> <span data-ttu-id="e31e3-110">ユーザー コントロールはもページに挿入される後、デザイン ビューでレンダリングされません。</span><span class="sxs-lookup"><span data-stu-id="e31e3-110">User controls are also not rendered in Design view after being inserted on a page.</span></span>


<span data-ttu-id="e31e3-111">成功した Web サイトの主なコンポーネントの 1 つは、一貫したルック アンド フィールです。</span><span class="sxs-lookup"><span data-stu-id="e31e3-111">One of the key components to a successful Web site is a consistent look and feel.</span></span> <span data-ttu-id="e31e3-112">Asp.net 1.x では、開発者で、Web アプリケーション全体で共通のページ要素をレプリケートするユーザー コントロールが使用されます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-112">In ASP.NET 1.x, developers used user controls to replicate common page elements across a Web application.</span></span> <span data-ttu-id="e31e3-113">有効な解決策ですが確かに、ユーザー コントロールを使用する欠点があります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-113">While that is certainly a workable solution, using user controls does have some drawbacks.</span></span> <span data-ttu-id="e31e3-114">たとえば、ユーザー コントロールの位置の変更は、サイト全体で複数のページへの変更が必要です。</span><span class="sxs-lookup"><span data-stu-id="e31e3-114">For example, a change in position of a user control requires a change to multiple pages across a site.</span></span> <span data-ttu-id="e31e3-115">ユーザー コントロールはもページに挿入される後、デザイン ビューでレンダリングされません。</span><span class="sxs-lookup"><span data-stu-id="e31e3-115">User controls are also not rendered in Design view after being inserted on a page.</span></span>

<span data-ttu-id="e31e3-116">ASP.NET 2.0 ではマスター ページとすると、一貫したルック アンド フィールを維持するための手段としてすぐにわかります、マスター ページは、ユーザー コントロールのメソッドより大幅に改善を表します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-116">ASP.NET 2.0 introduces Master pages as a way of maintaining a consistent look and feel, and as you'll soon see, Master pages represent a significant improvement over the user control method.</span></span>

## <a name="why-master-pages"></a><span data-ttu-id="e31e3-117">なぜマスター ページでしょうか。</span><span class="sxs-lookup"><span data-stu-id="e31e3-117">Why Master Pages?</span></span>

<span data-ttu-id="e31e3-118">疑問で ASP.NET 2.0 マスター ページが必要な理由です。</span><span class="sxs-lookup"><span data-stu-id="e31e3-118">You may be wondering why master pages were needed in ASP.NET 2.0.</span></span> <span data-ttu-id="e31e3-119">結局のところ、Web サイト開発者既に使用しているユーザー コントロールで ASP.NET 1.x のページ間のコンテンツ領域を共有します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-119">After all, Web site developers are already using user controls in ASP.NET 1.x to share content areas between pages.</span></span> <span data-ttu-id="e31e3-120">実際には、いくつかの理由がなぜユーザー コントロールは、一般的なレイアウトを作成するために、最適のソリューションがあります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-120">There are actually several reasons why user controls are a less-than-optimal solution for creating a common layout.</span></span>

<span data-ttu-id="e31e3-121">ユーザー コントロールでは、ページ レイアウトを実際には定義はありません。</span><span class="sxs-lookup"><span data-stu-id="e31e3-121">User controls don't actually define page layout.</span></span> <span data-ttu-id="e31e3-122">代わりに、レイアウトと、ページの一部の機能を定義します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-122">Instead, they define the layout and functionality for a portion of a page.</span></span> <span data-ttu-id="e31e3-123">ユーザー コントロールのソリューションの管理の容易性をかなり難しくなりますので、これらの 2 つの違いが重要です。</span><span class="sxs-lookup"><span data-stu-id="e31e3-123">The distinction between these two is important because it makes manageability of a user control solution much more difficult.</span></span> <span data-ttu-id="e31e3-124">たとえば、ページ上のユーザー コントロールの位置を変更する場合は、ユーザー コントロールが表示される実際のページを編集する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-124">For example, when you want to change the position of a user control on your page, you must edit the actual page on which the user control appears.</span></span> <span data-ttu-id="e31e3-125">これが正常にいくつかのページのみがあるが、大規模なサイトですぐにサイト管理悪夢のような場合です。</span><span class="sxs-lookup"><span data-stu-id="e31e3-125">Thats fine if you have only a few pages, but in large sites, it quickly becomes a site management nightmare!</span></span>

<span data-ttu-id="e31e3-126">ユーザー コントロールを使用する一般的なレイアウトを定義するためのもう 1 つは ASP.NET 自体のアーキテクチャを基盤とします。</span><span class="sxs-lookup"><span data-stu-id="e31e3-126">Another drawback of using user controls for defining a common layout is rooted in the architecture of ASP.NET itself.</span></span> <span data-ttu-id="e31e3-127">ユーザー コントロールのパブリック メンバーが変更された場合、すべてのユーザー コントロールを使用するページを再コンパイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="e31e3-127">If any public member of a user control is changed, it requires you to recompile all of the pages that use the user control.</span></span> <span data-ttu-id="e31e3-128">さらに、再度 JIT とは、最初のページにアクセスし、ASP.NET がされます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-128">In turn, ASP.NET will then re-JIT your pages when they are first accessed.</span></span> <span data-ttu-id="e31e3-129">これは、もう一度、非スケーラブル アーキテクチャと大規模なサイトのサイト管理の問題を生成します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-129">This, once again, produces a non-scalable architecture and a site management problem for larger sites.</span></span>

<span data-ttu-id="e31e3-130">これらの問題 (とさらに多く) の両方は、ASP.NET 2.0 のマスター ページで適切に対処できます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-130">Both of these problems (and many more) are nicely addressed by master pages in ASP.NET 2.0.</span></span>

## <a name="how-master-pages-work"></a><span data-ttu-id="e31e3-131">マスター ページの動作</span><span class="sxs-lookup"><span data-stu-id="e31e3-131">How Master Pages Work</span></span>

<span data-ttu-id="e31e3-132">マスター ページは、他のページのテンプレートに似ています。</span><span class="sxs-lookup"><span data-stu-id="e31e3-132">A master page is analogous to a template for other pages.</span></span> <span data-ttu-id="e31e3-133">(メニューの罫線など) は、他のページで共有されるページの要素は、マスター ページに追加されます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-133">Page elements that should be shared among other pages (i.e. menus, borders, etc.) are added to the master page.</span></span> <span data-ttu-id="e31e3-134">新しいページがサイトに追加されると、マスター ページに関連付けることができます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-134">When new pages are added to the site, you can associate them with a master page.</span></span> <span data-ttu-id="e31e3-135">マスター ページに関連付けられているページが呼び出され、**コンテンツ ページ**します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-135">A page that is associated with a master page is called a **content page**.</span></span> <span data-ttu-id="e31e3-136">既定では、コンテンツ ページからマスター ページの外観には移動します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-136">By default, a content page takes on the appearance from the master page.</span></span> <span data-ttu-id="e31e3-137">ただし、マスター ページを作成するときに、[コンテンツ] ページは、独自のコンテンツを置き換えることができるページの部分を定義できます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-137">However, when you create a master page, you can define portions of the page that the content page can replace with its own content.</span></span> <span data-ttu-id="e31e3-138">これらの部分は、ASP.NET 2.0 で導入された新しいコントロールを使用して定義されます。**ContentPlaceHolder**コントロール。</span><span class="sxs-lookup"><span data-stu-id="e31e3-138">These portions are defined using a new control introduced in ASP.NET 2.0; the **ContentPlaceHolder** control.</span></span>

<span data-ttu-id="e31e3-139">マスター ページは、任意の数のプレース ホルダー コントロール (またはまったく) を含めることができます。内のコンテンツ ページで、プレース ホルダー コントロールからコンテンツが表示される**コンテンツ**コントロール、ASP.NET 2.0 のもう 1 つの新しいコントロール。</span><span class="sxs-lookup"><span data-stu-id="e31e3-139">A master page can contain any number of ContentPlaceHolder controls (or none at all.) On the content page, the content from the ContentPlaceHolder controls appears inside of **Content** controls, another new control in ASP.NET 2.0.</span></span> <span data-ttu-id="e31e3-140">既定では、独自のコンテンツを提供できるように、コンテンツ ページのコンテンツ コントロールが空です。</span><span class="sxs-lookup"><span data-stu-id="e31e3-140">By default, the content pages Content controls are empty so that you can provide your own content.</span></span> <span data-ttu-id="e31e3-141">コンテンツ コントロール内のマスター ページからコンテンツを使用する場合は後でこのモジュールに表示されるようにします。</span><span class="sxs-lookup"><span data-stu-id="e31e3-141">If you want to use the content from the master page inside of the Content controls, you can do so as you will see later in this module.</span></span> <span data-ttu-id="e31e3-142">コンテンツ コントロールは、コンテンツ コントロールの ContentPlaceHolderID 属性を使用してプレース ホルダー コントロールにマップされます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-142">The Content control is mapped to the ContentPlaceHolder control via the ContentPlaceHolderID attribute of the Content control.</span></span> <span data-ttu-id="e31e3-143">マスター ページに mainBody と呼ばれるプレース ホルダー コントロールにコンテンツ コントロールをマップに次のコード。</span><span class="sxs-lookup"><span data-stu-id="e31e3-143">The code below maps a Content control to a ContentPlaceHolder control called mainBody on a master page.</span></span>

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> <span data-ttu-id="e31e3-144">ユーザーが他のページの基本クラスとしてマスター ページについて説明しますを聞くことが多くの場合。</span><span class="sxs-lookup"><span data-stu-id="e31e3-144">You will often hear people describe master pages as being a base class for other pages.</span></span> <span data-ttu-id="e31e3-145">これが実際にはありません。</span><span class="sxs-lookup"><span data-stu-id="e31e3-145">Thats actually not true.</span></span> <span data-ttu-id="e31e3-146">マスター ページとコンテンツのページ間のリレーションシップは、継承のいずれかではありません。</span><span class="sxs-lookup"><span data-stu-id="e31e3-146">The relationship between master pages and content pages is not one of inheritance.</span></span>


<span data-ttu-id="e31e3-147">**図 1**表示 Visual Studio 2005 でマスター ページと関連付けられているコンテンツ ページを示しています。</span><span class="sxs-lookup"><span data-stu-id="e31e3-147">**Figure 1** shows a master page and an associated content page as they appear in Visual Studio 2005.</span></span> <span data-ttu-id="e31e3-148">マスター ページと、対応するプレース ホルダー コントロールを確認できますコンテンツ ページのコントロールのコンテンツします。</span><span class="sxs-lookup"><span data-stu-id="e31e3-148">You can see the ContentPlaceHolder control in the master page and the corresponding Content control in the content page.</span></span> <span data-ttu-id="e31e3-149">ContentPlaceHolder 外にあるマスター ページ コンテンツが表示されますが、[コンテンツ] ページでグレーに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e31e3-149">Notice that the master pages content that is outside of the ContentPlaceHolder is visible but grayed out in the content page.</span></span> <span data-ttu-id="e31e3-150">[コンテンツ] ページで、プレース ホルダー内部のコンテンツのみをそれことができます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-150">Only the content inside of the ContentPlaceHolder can be supplanted by the content page.</span></span> <span data-ttu-id="e31e3-151">マスター ページから送信されるその他のすべてのコンテンツは変更できません。</span><span class="sxs-lookup"><span data-stu-id="e31e3-151">All other content that comes from the master page is immutable.</span></span>


![マスター ページとその関連するコンテンツ ページ](master-pages/_static/image1.jpg)

<span data-ttu-id="e31e3-153">**図 1**: マスター ページとその関連するコンテンツ ページ</span><span class="sxs-lookup"><span data-stu-id="e31e3-153">**Figure 1**: A master page and its associated content page</span></span>


## <a name="creating-a-master-page"></a><span data-ttu-id="e31e3-154">マスター ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-154">Creating a Master Page</span></span>

<span data-ttu-id="e31e3-155">新しいマスター ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-155">To create a new master page:</span></span>

1. <span data-ttu-id="e31e3-156">Visual Studio 2005 を開き、新しい Web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-156">Open Visual Studio 2005 and create a new Web site.</span></span>
2. <span data-ttu-id="e31e3-157">新しいファイルをクリックして、ファイルします。</span><span class="sxs-lookup"><span data-stu-id="e31e3-157">Click File, New, File.</span></span>
3. <span data-ttu-id="e31e3-158">ように、新しい項目の追加 ダイアログ ボックスから Master ファイルを選択**図 2**します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-158">Choose Master File from the Add New Item dialog as shown in **figure 2**.</span></span>
4. <span data-ttu-id="e31e3-159">[追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e31e3-159">Click Add.</span></span>


![新しいマスター ページを作成します。](master-pages/_static/image2.jpg)

<span data-ttu-id="e31e3-161">**図 2**: 新しいマスター ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-161">**Figure 2**: Creating a New Master Page</span></span>


<span data-ttu-id="e31e3-162">マスター ページのファイル拡張子は<em>.master</em>します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-162">Notice that the file extension for a master page is <em>.master</em>.</span></span> <span data-ttu-id="e31e3-163">これは、通常のページからマスター ページとは異なる方法のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="e31e3-163">This is one of the ways that a master page differs from an ordinary page.</span></span> <span data-ttu-id="e31e3-164">代わりに、その他の主な違いは、@Pageディレクティブ、マスター ページには、@Masterディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="e31e3-164">The other primary difference is that in lieu of a @Page directive, the master page contains a @Master directive.</span></span> <span data-ttu-id="e31e3-165">先ほど作成した、コードをレビューしてページをソース ビューにマスターの切り替え。</span><span class="sxs-lookup"><span data-stu-id="e31e3-165">Switch to Source View for the master page youve just created and review the code.</span></span>

<span data-ttu-id="e31e3-166">新しいマスター ページは、既定では、1 つのプレース ホルダー コントロールがあります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-166">A new master page will have one ContentPlaceHolder control by default.</span></span> <span data-ttu-id="e31e3-167">ほとんどの場合、方一般的なページ要素をまず作成し、プレース ホルダー コントロールを挿入するカスタム コンテンツが必要な場合。</span><span class="sxs-lookup"><span data-stu-id="e31e3-167">In most cases, it makes more sense to create the common page elements first and then insert ContentPlaceHolder controls where custom content is desired.</span></span> <span data-ttu-id="e31e3-168">その場合は、開発者は、既定のプレース ホルダー コントロールを削除して、ページが作成されると、新しい値を挿入する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-168">In those cases, developers will want to delete the default ContentPlaceHolder control and insert new ones as the page is developed.</span></span> <span data-ttu-id="e31e3-169">ContentPlaceHolder のコントロールがサイズ変更ハンドルが表示されることは事実に関係なくサイズ変更可能ではありません。</span><span class="sxs-lookup"><span data-stu-id="e31e3-169">ContentPlaceHolder controls are not resizable despite the fact that they do display sizing handles.</span></span> <span data-ttu-id="e31e3-170">ContentPlaceHolder のコントロールのサイズが 1 つの例外が含まれている内容に基づいて、自動的にブロック要素内のプレース ホルダー コントロールなどのテーブル セルに配置する場合は、要素のサイズに応じてサイズがします。</span><span class="sxs-lookup"><span data-stu-id="e31e3-170">The ContentPlaceHolder control sizes automatically based upon the content that it contains with one exception; if you place a ContentPlaceHolder control inside of a block element such as a table cell, it will size according to the size of the element.</span></span>

## <a name="lab-1-working-with-master-pages"></a><span data-ttu-id="e31e3-171">ラボ 1 マスター ページの操作</span><span class="sxs-lookup"><span data-stu-id="e31e3-171">Lab 1 Working with Master Pages</span></span>

<span data-ttu-id="e31e3-172">この演習では、新しいマスター ページを作成し、次の 3 つのプレース ホルダー コントロールを定義します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-172">In this lab, you will create a new master page and define three ContentPlaceHolder controls.</span></span> <span data-ttu-id="e31e3-173">新しいコンテンツ ページを作成し、少なくとも 1 つのプレース ホルダー コントロールからのコンテンツを置き換えるはします。</span><span class="sxs-lookup"><span data-stu-id="e31e3-173">You will then create a new Content page and replace the content from at least one of the ContentPlaceHolder controls.</span></span>

1. <span data-ttu-id="e31e3-174">マスター ページを作成し、プレース ホルダー コントロールを挿入します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-174">Create a master page and insert ContentPlaceHolder controls.</span></span> 

    1. <span data-ttu-id="e31e3-175">前述のように、新しいマスター ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-175">Create a new master page as described above.</span></span>
    2. <span data-ttu-id="e31e3-176">既定のプレース ホルダー コントロールを削除します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-176">Delete the default ContentPlaceHolder control.</span></span>
    3. <span data-ttu-id="e31e3-177">コントロールの上位影付きの枠線をクリックして、プレース ホルダー コントロールを選択し、キーボードの DEL キーを押すことによってこれを削除します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-177">Select the ContentPlaceHolder control by clicking the shaded top border of the control and then delete it by hitting the DEL key on your keyboard.</span></span>
    4. <span data-ttu-id="e31e3-178">使用して新しいテーブルを挿入、*ヘッダーと側*テンプレート 3 の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="e31e3-178">Insert a new table using the *Header and side* template as shown in figure 3.</span></span> <span data-ttu-id="e31e3-179">テーブル全体がデザイナーで表示されるよう、幅と高さを 90% に変更します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-179">Change the width and height to 90% each so that the entire table is visible in the designer.</span></span>


![](master-pages/_static/image3.jpg)

<span data-ttu-id="e31e3-180">**図 3**</span><span class="sxs-lookup"><span data-stu-id="e31e3-180">**Figure 3**</span></span>


1. <span data-ttu-id="e31e3-181">テーブルの各セルにカーソルを配置し、設定、 *valign*プロパティを*上部*します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-181">Place the cursor into each cell of the table and set the *valign* property to *top*.</span></span>
2. <span data-ttu-id="e31e3-182">ツールボックスからテーブル (ヘッダー セルです。) の先頭のセルにプレース ホルダー コントロールを挿入します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-182">From the Toolbox, insert a ContentPlaceHolder control in the top cell of the table (the header cell.)</span></span>
3. <span data-ttu-id="e31e3-183">このプレース ホルダー コントロールを挿入すると、図 4 に示すようにページ全体ではほとんどの行の高さはかかることがわかります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-183">When you insert this ContentPlaceHolder control, you will notice that the row height will take up almost the entire page as shown in figure 4.</span></span> <span data-ttu-id="e31e3-184">不要は、この時点でについて注意します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-184">Dont be concerned about that at this point.</span></span>


![空の領域は、プレース ホルダーとして同じセルで、します。](master-pages/_static/image1.gif)

<span data-ttu-id="e31e3-186">**図 4**: 空の領域は、プレース ホルダーとして同じセルで、</span><span class="sxs-lookup"><span data-stu-id="e31e3-186">**Figure 4**: The empty space is in the same cell as the ContentPlaceHolder</span></span>


1. <span data-ttu-id="e31e3-187">その他の 2 つのセルで、プレース ホルダー コントロールを配置します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-187">Place a ContentPlaceHolder control in the other two cells.</span></span> <span data-ttu-id="e31e3-188">他のコントロールのプレース ホルダーが挿入されると、テーブルのセルのサイズ期待どおりにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-188">Once the other ContentPlaceHolder controls have been inserted, the size of the table cells should be as you would expect.</span></span> <span data-ttu-id="e31e3-189">ページに表示するページのようになります**図 5**します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-189">The page should now look like the page shown in **figure 5**.</span></span>


![ContentPlaceHolder のすべてのコントロールを使用してマスターします。](master-pages/_static/image2.gif)

<span data-ttu-id="e31e3-192">**図 5**: ContentPlaceHolder のすべてのコントロールを使用して、マスターです。</span><span class="sxs-lookup"><span data-stu-id="e31e3-192">**Figure 5**: The Master with all ContentPlaceHolder controls.</span></span> <span data-ttu-id="e31e3-193">ヘッダー セルのセルの高さがそのでする必要がありますになったことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e31e3-193">Notice that the cell height for the header cell is now what it should be</span></span>


1. <span data-ttu-id="e31e3-194">それぞれの 3 つのプレース ホルダー コントロールには、好みのテキストを入力します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-194">Enter some text of your choice into each of the three ContentPlaceHolder controls.</span></span>
2. <span data-ttu-id="e31e3-195">Exercise1.master としてマスター ページを保存します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-195">Save the master page as exercise1.master.</span></span>
3. <span data-ttu-id="e31e3-196">新しい Web フォームを作成し、exercise1.master マスター ページと関連付けます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-196">Create a new Web Form and associate it with the exercise1.master master page.</span></span>
4. <span data-ttu-id="e31e3-197">ファイルの Visual Studio 2005 で、新しいファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-197">Select File, New, File in Visual Studio 2005.</span></span>
5. <span data-ttu-id="e31e3-198">選択**Web フォーム**新しい項目の追加 ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="e31e3-198">Select **Web Form** in the Add New Item dialog.</span></span>
6. <span data-ttu-id="e31e3-199">図 6 に示すように、マスター ページを選択チェック ボックスをオンすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-199">Make sure that the Select master page checkbox is checked as shown in figure 6.</span></span>


![新しいコンテンツ ページを追加します。](master-pages/_static/image3.gif)

<span data-ttu-id="e31e3-201">**図 6**: 新しいコンテンツ ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-201">**Figure 6**: Adding a new Content Page</span></span>


1. <span data-ttu-id="e31e3-202">[追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e31e3-202">Click Add.</span></span>
2. <span data-ttu-id="e31e3-203">Exercise1.master の選択でマスター ページのダイアログを選択図 7 に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="e31e3-203">Select exercise1.master in the Select a master page dialog as shown in figure 7.</span></span>
3. <span data-ttu-id="e31e3-204">新しいコンテンツ ページを追加するには、[ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e31e3-204">Click OK to add the new content page.</span></span>

<span data-ttu-id="e31e3-205">マスター ページ上の各プレース ホルダー コントロールのコンテンツ コントロールを 1 つに、Visual Studio で新しいコンテンツ ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-205">The new content page appears in Visual Studio with one Content control for each ContentPlaceHolder control on the master page.</span></span> <span data-ttu-id="e31e3-206">既定では、独自のコンテンツを追加することで、コンテンツ コントロールが空です。</span><span class="sxs-lookup"><span data-stu-id="e31e3-206">By default, the Content controls are empty so that you can add your own content.</span></span> <span data-ttu-id="e31e3-207">マスター ページの ContentPlaceHolder のコントロールのコンテンツを使用すると、単にスマート タグ記号 (コントロールの右上隅にある小さな黒い矢印) をクリックし、選択*マスター コンテンツを既定の*ように、スマート タグから**図 8**します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-207">If youd like for them to use the content from the ContentPlaceHolder control on the master page, simply click on the smart tag symbol (the small black arrow in the upper-right corner of the control) and choose *Default to Masters Content* from the smart tag as shown in **figure 8**.</span></span> <span data-ttu-id="e31e3-208">メニュー項目が変更にこれを行うときに*カスタム コンテンツの作成*です。</span><span class="sxs-lookup"><span data-stu-id="e31e3-208">When you do so, the menu item changes to *Create Custom Content*.</span></span> <span data-ttu-id="e31e3-209">クリックすると、その時点では、その特定のコンテンツ コントロールのカスタム コンテンツを定義することができます、マスター ページからコンテンツを削除します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-209">Clicking it at that point removes the content from the master page allowing you to define custom content for that particular Content control.</span></span>


![既定のマスター ページのコンテンツのコンテンツ コントロールの設定](master-pages/_static/image4.gif)

<span data-ttu-id="e31e3-211">**図 7**: コンテンツ コントロールをマスター ページのコンテンツを既定に設定します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-211">**Figure 7**: Setting a Content Control to Default to the Master Pages Content</span></span>


## <a name="connecting-master-page-and-content-pages"></a><span data-ttu-id="e31e3-212">マスター ページとコンテンツ ページに接続します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-212">Connecting Master Page and Content Pages</span></span>

<span data-ttu-id="e31e3-213">マスター ページとコンテンツのページ間の関連付けは、4 つの方法のいずれかで構成できます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-213">The association between a master page and a content page can be configured in one of four different ways:</span></span>

- <span data-ttu-id="e31e3-214"><strong>MasterPageFile</strong>の属性、@Pageディレクティブ</span><span class="sxs-lookup"><span data-stu-id="e31e3-214">The <strong>MasterPageFile</strong> attribute of the @Page directive</span></span>
- <span data-ttu-id="e31e3-215">設定、 **Page.MasterPageFile**コード内のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="e31e3-215">Setting the **Page.MasterPageFile** property in code.</span></span>
- <span data-ttu-id="e31e3-216">**&lt;ページ&gt;** アプリケーション構成ファイル (web.config で、アプリケーションのルート フォルダー) 内の要素</span><span class="sxs-lookup"><span data-stu-id="e31e3-216">The **&lt;pages&gt;** element in the applications configuration file (web.config in the root folder of the application)</span></span>
- <span data-ttu-id="e31e3-217">**&lt;ページ&gt;** サブフォルダー構成ファイル (web.config のサブフォルダー) 内の要素</span><span class="sxs-lookup"><span data-stu-id="e31e3-217">The **&lt;pages&gt;** element in a subfolders configuration file (web.config in a subfolder)</span></span>

## <a name="masterpagefile-attribute"></a><span data-ttu-id="e31e3-218">MasterPageFile 属性</span><span class="sxs-lookup"><span data-stu-id="e31e3-218">MasterPageFile Attribute</span></span>

<span data-ttu-id="e31e3-219">MasterPageFile 属性を簡単に特定の ASP.NET ページにマスター ページを適用します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-219">The MasterPageFile attribute makes it easy to apply a master page to a particular ASP.NET page.</span></span> <span data-ttu-id="e31e3-220">オンにすると、マスター ページを適用するために使用するメソッドも、**マスター ページの選択**するチェック ボックスは、演習 1 ででした。</span><span class="sxs-lookup"><span data-stu-id="e31e3-220">It is also the method used to apply the master page when you check the **Select Master Page** checkbox as you did in Exercise 1.</span></span>

## <a name="setting-pagemasterpagefile-in-code"></a><span data-ttu-id="e31e3-221">コードで Page.MasterPageFile の設定</span><span class="sxs-lookup"><span data-stu-id="e31e3-221">Setting Page.MasterPageFile in Code</span></span>

<span data-ttu-id="e31e3-222">コードの MasterPageFile プロパティの設定によっては、実行時にコンテンツを特定のマスター ページを適用できます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-222">By setting the MasterPageFile property in code, you can apply a particular master page to your content at runtime.</span></span> <span data-ttu-id="e31e3-223">これは、ユーザー ロールまたはその他のいくつかの条件に基づいて、特定のマスター ページを適用する必要がある場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="e31e3-223">This is useful in cases where you may need to apply a specific master page based upon a users role or some other criteria.</span></span> <span data-ttu-id="e31e3-224">PreInit メソッドでは、MasterPageFile プロパティを設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-224">The MasterPageFile property must be set in the PreInit method.</span></span> <span data-ttu-id="e31e3-225">PreInit メソッドの後に設定されている場合、InvalidOperationException がスローされます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-225">If it is set after the PreInit method, an InvalidOperationException will be thrown.</span></span> <span data-ttu-id="e31e3-226">このプロパティが設定されているページには、コンテンツの必要があります。 ページのコントロールがトップレベル コントロールとしてコントロール。</span><span class="sxs-lookup"><span data-stu-id="e31e3-226">The page on which this property is being set must also have a Content control as the top-level control for the page.</span></span> <span data-ttu-id="e31e3-227">それ以外の場合、MasterPageFile プロパティが設定されている場合、HttpException がスローされます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-227">Otherwise an HttpException will be thrown when the MasterPageFile property is set.</span></span>

## <a name="using-the-ltpagesgt-element"></a><span data-ttu-id="e31e3-228">使用して、&lt;ページ&gt;要素</span><span class="sxs-lookup"><span data-stu-id="e31e3-228">Using the &lt;pages&gt; Element</span></span>

<span data-ttu-id="e31e3-229">MasterPageFile 属性に設定して、ページのマスター ページを構成することができます、&lt;ページ&gt;web.config ファイルの要素。</span><span class="sxs-lookup"><span data-stu-id="e31e3-229">You can configure a master page for your pages by setting the masterPageFile attribute in the &lt;pages&gt; element of the web.config file.</span></span> <span data-ttu-id="e31e3-230">このメソッドを使用する場合は、アプリケーションの構造内の下位の web.config ファイルがこの設定をオーバーライドできることに留意してください。</span><span class="sxs-lookup"><span data-stu-id="e31e3-230">When using this method, keep in mind that web.config files lower in the application structure can override this setting.</span></span> <span data-ttu-id="e31e3-231">任意の MasterPageFile 属性で設定を@Pageディレクティブは、この設定をオーバーライドしても。</span><span class="sxs-lookup"><span data-stu-id="e31e3-231">Any MasterPageFile attribute set in a @Page directive will also override this setting.</span></span> <span data-ttu-id="e31e3-232">使用して、&lt;ページ&gt;要素により、簡単に作成、<em>マスター</em>マスター ページを特定のフォルダーまたはファイルに必要な場合にオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-232">Using the &lt;pages&gt; element makes it simple to create a <em>master</em> master page that can be overridden if necessary in particular folders or files.</span></span>

## <a name="properties-in-master-pages"></a><span data-ttu-id="e31e3-233">マスター ページのプロパティ</span><span class="sxs-lookup"><span data-stu-id="e31e3-233">Properties in Master Pages</span></span>

<span data-ttu-id="e31e3-234">マスター ページは、これらのプロパティをマスター ページ内で公開するだけで、プロパティを公開できます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-234">A master page can expose properties by simply making those properties public within the master page.</span></span> <span data-ttu-id="e31e3-235">たとえば、次のコードでは、SomeProperty という名前のプロパティを定義します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-235">For example, the following code defines a property called SomeProperty:</span></span>

[!code-csharp[Main](master-pages/samples/sample2.cs)]

<span data-ttu-id="e31e3-236">[コンテンツ] ページから SomeProperty プロパティにアクセスするにマスターを使用する必要がありますプロパティようになります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-236">To access the SomeProperty property from the Content page, you will need to use the Master property like so:</span></span>

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a><span data-ttu-id="e31e3-237">入れ子のマスター ページ</span><span class="sxs-lookup"><span data-stu-id="e31e3-237">Nesting Master Pages</span></span>

<span data-ttu-id="e31e3-238">マスター ページは、大規模な Web アプリケーション間で共通のルック アンド フィールを確保するための最適なソリューションです。</span><span class="sxs-lookup"><span data-stu-id="e31e3-238">Master pages are the perfect solution for ensuring a common look and feel across a large Web application.</span></span> <span data-ttu-id="e31e3-239">ただし、大規模なサイトの共有、共通のインターフェイスの特定の部分を他の部分を共有する別のインターフェイスが珍しくないです。</span><span class="sxs-lookup"><span data-stu-id="e31e3-239">However, it's not uncommon to have certain parts of a large site share a common interface while other parts share a different interface.</span></span> <span data-ttu-id="e31e3-240">そのに対処するには、複数のマスター ページは、完全なソリューションです。</span><span class="sxs-lookup"><span data-stu-id="e31e3-240">To address that need, multiple master pages are the perfect solution.</span></span> <span data-ttu-id="e31e3-241">ただし、まだは取り扱いませんファクトの大規模なアプリケーションは、すべてのページ間で共有される特定のコンポーネント (たとえばメニュー) など、特定のセクションでは、サイトの間だけで共有されるその他のコンポーネントがある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-241">However, that still doesn't address the fact that a large application may have certain components (such as a menu, for example) that are shared among all pages and other components that are shared only among certain sections of the site.</span></span> <span data-ttu-id="e31e3-242">そのような状況は、入れ子になったマスター ページは、必要を適切に入力します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-242">For that type of situation, nested master pages fill the need nicely.</span></span> <span data-ttu-id="e31e3-243">で説明したようにマスター ページとコンテンツ ページの標準のマスター ページで構成されます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-243">As you've seen, a normal master page consists of a master page and a content page.</span></span> <span data-ttu-id="e31e3-244">入れ子になったマスター ページの状況では、2 つのマスター ページです。親マスターと子マスターします。</span><span class="sxs-lookup"><span data-stu-id="e31e3-244">In a nested master page situation, there are two master pages; a parent master and a child master.</span></span> <span data-ttu-id="e31e3-245">子のマスター ページは、[コンテンツ] ページでも、そのマスタは親マスター ページ。</span><span class="sxs-lookup"><span data-stu-id="e31e3-245">The child master page is also a content page and its master is the parent master page.</span></span>

<span data-ttu-id="e31e3-246">次の一般的なマスター ページのコードに示します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-246">Here is the code for a typical master page:</span></span>

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

<span data-ttu-id="e31e3-247">入れ子になったマスターの場合、親マスターになります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-247">In a nested master scenario, this would be the parent master.</span></span> <span data-ttu-id="e31e3-248">別のマスター ページは、このページを使用して、そのマスター ページとしては、そのコードは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-248">Another master page would use this page as its master page, and that code would look like this:</span></span>

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

<span data-ttu-id="e31e3-249">このシナリオで子マスターも親マスターのコンテンツ ページに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e31e3-249">Note that in this scenario, the child master is also a content page for the parent master.</span></span> <span data-ttu-id="e31e3-250">すべての子マスターのコンテンツは、親のプレース ホルダー コントロールからそのコンテンツを取得するコンテンツ コントロールの内部が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e31e3-250">All of the child master's content appears inside of a Content control that gets its content from the parent's ContentPlaceHolder control.</span></span>

> [!NOTE]
> <span data-ttu-id="e31e3-251">デザイナー サポートは、入れ子になったマスター ページをご利用いただけません。</span><span class="sxs-lookup"><span data-stu-id="e31e3-251">Designer support is not available for nested master pages.</span></span> <span data-ttu-id="e31e3-252">入れ子になったマスターの使用を開発する場合は、ソース ビューを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e31e3-252">When you are developing using nested masters, you will need to use source view.</span></span>


<span data-ttu-id="e31e3-253">このビデオでは、入れ子になったマスター ページを使用してチュートリアルを説明します。</span><span class="sxs-lookup"><span data-stu-id="e31e3-253">This video shows a walkthrough of using nested master pages.</span></span>


![](master-pages/_static/image1.png)


[<span data-ttu-id="e31e3-254">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="e31e3-254">Open Full-Screen Video</span></span>](master-pages/_static/nested1.wmv)


![マスター ページの選択](master-pages/_static/image4.jpg)

<span data-ttu-id="e31e3-256">**図 8**: マスター ページの選択</span><span class="sxs-lookup"><span data-stu-id="e31e3-256">**Figure 8**: Selecting a Master Page</span></span>
