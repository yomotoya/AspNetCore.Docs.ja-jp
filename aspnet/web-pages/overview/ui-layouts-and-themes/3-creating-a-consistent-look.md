---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: ページ (Razor) サイトの ASP.NET Web で一貫したレイアウトを作成 |Microsoft ドキュメント
author: tfitzmac
description: これをサイトの web ページを作成する方が効率的にするには、するには、web サイト、および c を (ヘッダーやフッター) などのコンテンツの再利用可能なブロックを作成しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/10/2014
ms.topic: article
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 2c7631017f7c0fb31f43320c2ab78baddd87b516
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530241"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="bd1b3-103">ASP.NET Web Pages (Razor) のサイトで一貫したレイアウトを作成します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-103">Creating a Consistent Layout in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="bd1b3-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bd1b3-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="bd1b3-105">この記事使用方法を説明するレイアウト ページの ASP.NET Web Pages (Razor) web サイト (ヘッダーやフッター) などのコンテンツの再利用可能なブロックを作成して、サイトのすべてのページで一貫した外観を作成します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-105">This article explains how you can use layout pages in an ASP.NET Web Pages (Razor) website to create reusable blocks of content (like headers and footers) and to create a consistent look for all the pages in the site.</span></span>
> 
> <span data-ttu-id="bd1b3-106">**学習する内容。**</span><span class="sxs-lookup"><span data-stu-id="bd1b3-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="bd1b3-107">ヘッダーやフッターなどのコンテンツの再利用可能なブロックを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-107">How to create reusable blocks of content like headers and footers.</span></span>
> - <span data-ttu-id="bd1b3-108">レイアウトを使用して、サイト内のすべてのページの一貫した外観を作成する方法。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-108">How to create a consistent look for all the pages in your site using a layout.</span></span>
> - <span data-ttu-id="bd1b3-109">レイアウト ページに、実行時にデータを渡す方法。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-109">How to pass data at run time to a layout page.</span></span>
> 
> <span data-ttu-id="bd1b3-110">アーティクルで導入された ASP.NET 機能を次に示します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="bd1b3-111">コンテンツ ブロック、複数のページに挿入される HTML 形式のコンテンツを含むファイルです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-111">Content blocks, which are files that contain HTML-formatted content to be inserted in multiple pages.</span></span>
> - <span data-ttu-id="bd1b3-112">レイアウト ページ、web サイト上のページで共有できる HTML 形式のコンテンツが含まれるページします。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-112">Layout pages, which are pages that contain HTML-formatted content that can be shared by pages on the website.</span></span>
> - <span data-ttu-id="bd1b3-113">`RenderPage`、 `RenderBody`、および`RenderSection`メソッドで、ページの要素を挿入する場所を ASP.NET に指示します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-113">The `RenderPage`, `RenderBody`, and `RenderSection` methods, which tell ASP.NET where to insert page elements.</span></span>
> - <span data-ttu-id="bd1b3-114">`PageData`コンテンツ ブロック レイアウト ページの間でデータを共有することができますをディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-114">The `PageData` dictionary that lets you share data between content blocks and layout pages.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bd1b3-115">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="bd1b3-115">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="bd1b3-116">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="bd1b3-116">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="bd1b3-117">このチュートリアルは、ASP.NET Web Pages 2 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-117">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-layout-pages"></a><span data-ttu-id="bd1b3-118">レイアウト ページの概要</span><span class="sxs-lookup"><span data-stu-id="bd1b3-118">About Layout Pages</span></span>

<span data-ttu-id="bd1b3-119">多くの web サイトでは、ヘッダーとフッター、またはログオンしているユーザーに通知するボックスのように、すべてのページに表示されているコンテンツが存在します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-119">Many websites have content that's displayed on every page, like a header and footer, or a box that tells users that they're logged in.</span></span> <span data-ttu-id="bd1b3-120">ASP.NET では、テキスト、マークアップ、および標準の web ページと同じように、コードを含めることができるコンテンツ ブロックを別のファイルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-120">ASP.NET lets you create a separate file with a content block that can contain text, markup, and code, just like a regular web page.</span></span> <span data-ttu-id="bd1b3-121">情報を表示するサイトの他のページで、コンテンツ ブロックを挿入できます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-121">You can then insert the content block in other pages on the site where you want the information to appear.</span></span> <span data-ttu-id="bd1b3-122">このようにコピーして、すべてのページに同じコンテンツを貼り付ける必要はありません。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-122">That way you don't have to copy and paste the same content into every page.</span></span> <span data-ttu-id="bd1b3-123">次のような一般的なコンテンツの作成も簡単、サイトを更新できます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-123">Creating common content like this also makes it easier to update your site.</span></span> <span data-ttu-id="bd1b3-124">コンテンツを変更する必要があります、1 つのファイルだけを更新できるし、変更はすべての場所で反映し、コンテンツが挿入されました。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-124">If you need to change the content, you can just update a single file, and the changes are then reflected everywhere the content has been inserted.</span></span>

<span data-ttu-id="bd1b3-125">次の図は、コンテンツが作業をブロックする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-125">The following diagram shows how content blocks work.</span></span> <span data-ttu-id="bd1b3-126">ASP.NET がポイントでコンテンツのブロックを挿入するブラウザーでは、web サーバーからページを要求するときにここで、`RenderPage`メイン ページでメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-126">When a browser requests a page from the web server, ASP.NET inserts the content blocks at the point where the `RenderPage` method is called in the main page.</span></span> <span data-ttu-id="bd1b3-127">完成した (マージ) ページがブラウザーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-127">The finished (merged) page is then sent to the browser.</span></span>

![RenderPage メソッドが、現在のページに、参照先のページを挿入する方法を示す概念図です。](3-creating-a-consistent-look/_static/image1.jpg)

<span data-ttu-id="bd1b3-129">この手順では、個別のファイル内にある 2 つのコンテンツ ブロック (ヘッダーおよびフッター) を参照するページを作成します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-129">In this procedure, you'll create a page that references two content blocks (a header and a footer) that are located in separate files.</span></span> <span data-ttu-id="bd1b3-130">サイト内の任意のページで、これらの同じコンテンツ ブロックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-130">You can use these same content blocks in any page in your site.</span></span> <span data-ttu-id="bd1b3-131">完了したら、次のようにページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-131">When you're done, you'll get a page like this:</span></span>

![RenderPage メソッドへの呼び出しを含むページを実行して得た結果ブラウザーでページを示すスクリーン ショット。](3-creating-a-consistent-look/_static/image2.jpg)

1. <span data-ttu-id="bd1b3-133">という名前のファイルを作成、web サイトのルート フォルダーに*Index.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-133">In the root folder of your website, create a file named *Index.cshtml*.</span></span>
2. <span data-ttu-id="bd1b3-134">既存のマークアップを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-134">Replace the existing markup with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. <span data-ttu-id="bd1b3-135">ルート フォルダーにという名前のフォルダーを作成する*Shared*です。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-135">In the root folder, create a folder named *Shared*.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bd1b3-136">という名前のフォルダー内の web ページ間で共有されるファイルを保存することを共通*Shared*です。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-136">It's common practice to store files that are shared among web pages in a folder named *Shared*.</span></span>
4. <span data-ttu-id="bd1b3-137">*Shared*フォルダー、という名前のファイルを作成する *\_Header.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-137">In the *Shared* folder, create a file named *\_Header.cshtml*.</span></span>
5. <span data-ttu-id="bd1b3-138">既存の内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-138">Replace any existing content with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    <span data-ttu-id="bd1b3-139">ファイル名は *\_Header.cshtml*、アンダー スコア (\_) をプレフィックスとして。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-139">Notice that the file name is *\_Header.cshtml*, with an underscore (\_) as a prefix.</span></span> <span data-ttu-id="bd1b3-140">ASP.NET は、その名前がアンダー スコアで始まる場合、ブラウザーにページを送信しません。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-140">ASP.NET won't send a page to the browser if its name starts with an underscore.</span></span> <span data-ttu-id="bd1b3-141">これはユーザーが要求する (誤ってまたはそれ以外の場合) これらのページ直接することを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-141">This prevents people from requesting (inadvertently or otherwise) these pages directly.</span></span> <span data-ttu-id="bd1b3-142">ユーザーがこれらのページ &#8212; を要求できるかないためには、コンテンツ ブロックがある名前のページにアンダー スコアを使用することをお勧めそれらは、他のページに挿入する厳密に存在します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-142">It's a good idea to use an underscore to name pages that have content blocks in them, because you don't really want users to be able to request these pages &#8212; they exist strictly to be inserted into other pages.</span></span>
6. <span data-ttu-id="bd1b3-143">*共有*フォルダー、という名前のファイルを作成する *\_Footer.cshtml*コンテンツを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-143">In the *Shared* folder, create a file named *\_Footer.cshtml* and replace the content with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. <span data-ttu-id="bd1b3-144">*Index.cshtml*  ページで、次の 2 つの呼び出しを追加、`RenderPage`メソッドを次に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-144">In the *Index.cshtml* page, add two calls to the `RenderPage` method, as shown here:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    <span data-ttu-id="bd1b3-145">これは、web ページに、コンテンツ ブロックを挿入する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-145">This shows how to insert a content block into a web page.</span></span> <span data-ttu-id="bd1b3-146">呼び出す、`RenderPage`メソッドをその時点で挿入する内容を持つファイルの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-146">You call the `RenderPage` method and pass it the name of the file whose contents you want to insert at that point.</span></span> <span data-ttu-id="bd1b3-147">ここでは、内容を挿入している、  *\_Header.cshtml*と *\_Footer.cshtml*へのファイル、 *Index.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-147">Here, you're inserting the contents of the *\_Header.cshtml* and *\_Footer.cshtml* files into the *Index.cshtml* file.</span></span>
8. <span data-ttu-id="bd1b3-148">実行、 *Index.cshtml*ブラウザーのページです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-148">Run the *Index.cshtml* page in a browser.</span></span> <span data-ttu-id="bd1b3-149">(WebMatrix での**ファイル**] ワークスペースで、ファイルを右クリックし、[**ブラウザーで起動**)。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-149">(In WebMatrix, in the **Files** workspace, right-click the file and then select **Launch in browser**.)</span></span>
9. <span data-ttu-id="bd1b3-150">ブラウザーでページ ソースを表示します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-150">In the browser, view the page source.</span></span> <span data-ttu-id="bd1b3-151">(たとえば、Internet Explorer で、ページを右クリックし、をクリックして**ソースの表示**)。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-151">(For example, in Internet Explorer, right-click the page and then click **View Source**.)</span></span>

    <span data-ttu-id="bd1b3-152">これにより、ブラウザーでは、インデックス ページのマークアップを組み合わせて、コンテンツ ブロックに送信される web ページのマークアップを表示できます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-152">This lets you see the web page markup that's sent to the browser, which combines the index page markup with the content blocks.</span></span> <span data-ttu-id="bd1b3-153">次の例では、について表示されるページのソース*Index.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-153">The following example shows the page source that's rendered for *Index.cshtml*.</span></span> <span data-ttu-id="bd1b3-154">呼び出し`RenderPage`に挿入した*Index.cshtml*ヘッダーとページ フッターのファイルの実際の内容に置き換えられています。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-154">The calls to `RenderPage` that you inserted into *Index.cshtml* have been replaced with the actual contents of the header and footer files.</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a><span data-ttu-id="bd1b3-155">レイアウト ページを使用して一貫した外観を作成します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-155">Creating a Consistent Look Using Layout Pages</span></span>

<span data-ttu-id="bd1b3-156">これまでは、簡単に複数のページに同じコンテンツを含めることを見てきました。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-156">So far you've seen that it's easy to include the same content on multiple pages.</span></span> <span data-ttu-id="bd1b3-157">サイトの一貫した外観を作成するための構造化されたアプローチでは、レイアウト ページを使用します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-157">A more structured approach to creating a consistent look for a site is to use layout pages.</span></span> <span data-ttu-id="bd1b3-158">レイアウト ページでは、web ページの構造を定義していますが、実際の内容が含まれていません。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-158">A layout page defines the structure of a web page, but doesn't contain any actual content.</span></span> <span data-ttu-id="bd1b3-159">レイアウト ページを作成した後、コンテンツが含まれ、それらをレイアウト ページにリンクする web ページを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-159">After you've created a layout page, you can create web pages that contain the content and then link them to the layout page.</span></span> <span data-ttu-id="bd1b3-160">これらのページが表示される場合は、レイアウト ページに従ってフォーマットされます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-160">When these pages are displayed, they'll be formatted according to the layout page.</span></span> <span data-ttu-id="bd1b3-161">(この意味で、レイアウト ページとして動作は他のページで定義されているコンテンツのテンプレートの種類です。)</span><span class="sxs-lookup"><span data-stu-id="bd1b3-161">(In this sense, a layout page acts as a kind of template for content that's defined in other pages.)</span></span>

<span data-ttu-id="bd1b3-162">呼び出しが含まれていますが、任意の HTML ページと同じようには、レイアウト ページ、`RenderBody`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-162">The layout page is just like any HTML page, except that it contains a call to the `RenderBody` method.</span></span> <span data-ttu-id="bd1b3-163">位置、`RenderBody`レイアウト ページのメソッドは、コンテンツ ページの情報が含まれるを決定します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-163">The position of the `RenderBody` method in the layout page determines where the information from the content page will be included.</span></span>

<span data-ttu-id="bd1b3-164">次の図は、コンテンツ ページが表示され、レイアウト ページは、完成した web ページを生成するために実行時に結合されます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-164">The following diagram shows how content pages and layout pages are combined at run time to produce the finished web page.</span></span> <span data-ttu-id="bd1b3-165">ブラウザーでは、コンテンツ ページを要求します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-165">The browser requests a content page.</span></span> <span data-ttu-id="bd1b3-166">コンテンツ ページでは、ページの構造を使用するレイアウト ページを指定することでコードを持っています。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-166">The content page has code in it that specifies the layout page to use for the page's structure.</span></span> <span data-ttu-id="bd1b3-167">ポイントでコンテンツを挿入レイアウト ページで、場所、`RenderBody`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-167">In the layout page, the content is inserted at the point where the `RenderBody` method is called.</span></span> <span data-ttu-id="bd1b3-168">コンテンツ ブロックも挿入可能、レイアウト ページを呼び出して、`RenderPage`メソッド、前のセクションで行った方法です。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-168">Content blocks can also be inserted into the layout page by calling the `RenderPage` method, the way you did in the previous section.</span></span> <span data-ttu-id="bd1b3-169">Web ページが完了したら、ブラウザーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-169">When the web page is complete, it's sent to the browser.</span></span>

![RenderBody メソッドへの呼び出しを含むページを実行して得た結果ブラウザーでページを示すスクリーン ショット。](3-creating-a-consistent-look/_static/image3.jpg)

<span data-ttu-id="bd1b3-171">次の手順では、ページとリンクのコンテンツ ページをレイアウトを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-171">The following procedure shows how to create a layout page and link content pages to it.</span></span>

1. <span data-ttu-id="bd1b3-172">*Shared*という名前のファイルの作成、web サイトのフォルダー  *\_Layout1.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-172">In the *Shared* folder of your website, create a file named *\_Layout1.cshtml*.</span></span>
2. <span data-ttu-id="bd1b3-173">既存の内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-173">Replace any existing content with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    <span data-ttu-id="bd1b3-174">使用する、`RenderPage`コンテンツ ブロックを挿入するレイアウト ページ内のメソッドです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-174">You use the `RenderPage` method in a layout page to insert content blocks.</span></span> <span data-ttu-id="bd1b3-175">レイアウト ページは、1 つだけの呼び出しを含めることができます、`RenderBody`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-175">A layout page can contain only one call to the `RenderBody` method.</span></span>
3. <span data-ttu-id="bd1b3-176">*Shared*フォルダー、という名前のファイルを作成する *\_Header2.cshtml*既存のコンテンツを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-176">In the *Shared* folder, create a file named *\_Header2.cshtml* and replace any existing content with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. <span data-ttu-id="bd1b3-177">ルート フォルダーに新しいフォルダーを作成し、名前*スタイル*です。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-177">In the root folder, create a new folder and name it *Styles*.</span></span>
5. <span data-ttu-id="bd1b3-178">*スタイル*フォルダー、という名前のファイルを作成*Site.css*し、次のスタイル定義を追加します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-178">In the *Styles* folder, create a file named *Site.css* and add the following style definitions:</span></span>

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    <span data-ttu-id="bd1b3-179">これらのスタイルの定義は、レイアウト ページでスタイル シートを使用する方法を表示するためだけここでは。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-179">These style definitions are here only to show how style sheets can be used with layout pages.</span></span> <span data-ttu-id="bd1b3-180">する場合は、これらの要素に独自のスタイルを定義できます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-180">If you want, you can define your own styles for these elements.</span></span>
6. <span data-ttu-id="bd1b3-181">ルート フォルダーにという名前のファイルを作成する*Content1.cshtml*既存のコンテンツを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-181">In the root folder, create a file named *Content1.cshtml* and replace any existing content with the following:</span></span>

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    <span data-ttu-id="bd1b3-182">これは、レイアウト ページを使用するページです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-182">This is a page that will use a layout page.</span></span> <span data-ttu-id="bd1b3-183">ページの上部にあるコード ブロックでは、このコンテンツを書式設定に使用するレイアウト ページを示します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-183">The code block at the top of the page indicates which layout page to use to format this content.</span></span>
7. <span data-ttu-id="bd1b3-184">実行*Content1.cshtml*ブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-184">Run *Content1.cshtml* in a browser.</span></span> <span data-ttu-id="bd1b3-185">表示されたページは、形式を使用し、スタイル シートがで定義されている *\_Layout1.cshtml*およびで定義されているテキスト (コンテンツ) *Content1.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-185">The rendered page uses the format and style sheet defined in *\_Layout1.cshtml* and the text (content) defined in *Content1.cshtml*.</span></span>

    ![[イメージ]](3-creating-a-consistent-look/_static/image4.jpg)

    <span data-ttu-id="bd1b3-187">同じレイアウト ページを共有することができますし、追加のコンテンツ ページを作成する 6 の手順を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-187">You can repeat step 6 to create additional content pages that can then share the same layout page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bd1b3-188">フォルダー内のすべてのコンテンツ ページの同じページにレイアウトを自動的に使用できるように、サイトを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-188">You can set up your site so that you can automatically use the same layout page for all the content pages in a folder.</span></span> <span data-ttu-id="bd1b3-189">詳細については、「[サイト全体の動作をカスタマイズする ASP.NET Web Pages の](https://go.microsoft.com/fwlink/?LinkId=202906)します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-189">For details, see [Customizing Site-Wide Behavior for ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906).</span></span>

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a><span data-ttu-id="bd1b3-190">複数のコンテンツ セクション レイアウト ページのデザイン</span><span class="sxs-lookup"><span data-stu-id="bd1b3-190">Designing Layout Pages That Have Multiple Content Sections</span></span>

<span data-ttu-id="bd1b3-191">コンテンツ ページは、置き換え可能なコンテンツを持つ複数の領域のレイアウトを使用する場合に便利ですは複数のセクションを設定できます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-191">A content page can have multiple sections, which is useful if you want to use layouts that have multiple areas with replaceable content.</span></span> <span data-ttu-id="bd1b3-192">コンテンツ ページで、各セクションに一意の名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-192">In the content page, you give each section a unique name.</span></span> <span data-ttu-id="bd1b3-193">(既定のセクションの左無名です)。レイアウト ページでは追加、`RenderBody`名前のない (既定) のセクションが表示される場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-193">(The default section is left unnamed.) In the layout page, you add a `RenderBody` method to specify where the unnamed (default) section should appear.</span></span> <span data-ttu-id="bd1b3-194">追加する別`RenderSection`名前付きセクションを個別に表示するためにメソッドです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-194">You then add separate `RenderSection` methods in order to render named sections individually.</span></span>

<span data-ttu-id="bd1b3-195">次の図は、ASP.NET が複数のセクションに分割されたコンテンツを処理する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-195">The following diagram shows how ASP.NET handles content that's divided into multiple sections.</span></span> <span data-ttu-id="bd1b3-196">各名前付きセクションでは、コンテンツ ページのセクションでブロックに含まれます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-196">Each named section is contained in a section block in the content page.</span></span> <span data-ttu-id="bd1b3-197">(名前が付けられますが`Header`と`List`の例です)。フレームワークが挿入ポイントで、レイアウト ページのコンテンツ セクションで、`RenderSection`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-197">(They're named `Header` and `List` in the example.) The framework inserts content section into the layout page at the point where the `RenderSection` method is called.</span></span> <span data-ttu-id="bd1b3-198">名前のない (既定) のセクションは、ポイントに挿入場所、`RenderBody`前述のメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-198">The unnamed (default) section is inserted at the point where the `RenderBody` method is called, as you saw earlier.</span></span>

![RenderSection メソッドが、現在のページに参照セクションを挿入する方法を示す概念図です。](3-creating-a-consistent-look/_static/image5.jpg)

<span data-ttu-id="bd1b3-200">この手順では、複数のコンテンツ セクションのあるコンテンツ ページを作成する方法と複数のコンテンツ セクションをサポートするレイアウト ページを使用して表示する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-200">This procedure shows how to create a content page that has multiple content sections and how to render it using a layout page that supports multiple content sections.</span></span>

1. <span data-ttu-id="bd1b3-201">*Shared*フォルダー、という名前のファイルを作成する *\_Layout2.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-201">In the *Shared* folder, create a file named *\_Layout2.cshtml*.</span></span>
2. <span data-ttu-id="bd1b3-202">既存の内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-202">Replace any existing content with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    <span data-ttu-id="bd1b3-203">使用する、`RenderSection`ヘッダーとリストの両方のセクションを表示するメソッド。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-203">You use the `RenderSection` method to render both the header and list sections.</span></span>
3. <span data-ttu-id="bd1b3-204">ルート フォルダーにという名前のファイルを作成する*Content2.cshtml*既存のコンテンツを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-204">In the root folder, create a file named *Content2.cshtml* and replace any existing content with the following:</span></span>

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    <span data-ttu-id="bd1b3-205">このコンテンツ ページには、ページの上部にあるコード ブロックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-205">This content page contains a code block at the top of the page.</span></span> <span data-ttu-id="bd1b3-206">各名前付きセクションでは、セクション ブロックに含まれます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-206">Each named section is contained in a section block.</span></span> <span data-ttu-id="bd1b3-207">ページの残りの部分には、既定の (名前のない) のコンテンツ セクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-207">The rest of the page contains the default (unnamed) content section.</span></span>
4. <span data-ttu-id="bd1b3-208">実行*Content2.cshtml*ブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-208">Run *Content2.cshtml* in a browser.</span></span>

    ![RenderSection メソッドへの呼び出しを含むページを実行して得た結果ブラウザーでページを示すスクリーン ショット。](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a><span data-ttu-id="bd1b3-210">省略可能なコンテンツ セクションを行う</span><span class="sxs-lookup"><span data-stu-id="bd1b3-210">Making Content Sections Optional</span></span>

<span data-ttu-id="bd1b3-211">通常、コンテンツ ページで作成するためのセクションでは、セクションでは、レイアウト ページで定義されていると一致する必要です。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-211">Normally, the sections that you create in a content page have to match sections that are defined in the layout page.</span></span> <span data-ttu-id="bd1b3-212">次のいずれかが発生した場合、エラーが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-212">You can get errors if any of the following occur:</span></span>

- <span data-ttu-id="bd1b3-213">コンテンツ ページには、レイアウト ページの該当するセクションを持たないセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-213">The content page contains a section that has no corresponding section in the layout page.</span></span>
- <span data-ttu-id="bd1b3-214">[レイアウト] ページには、対象のコンテンツがセクションが含まれていません。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-214">The layout page contains a section for which there's no content.</span></span>
- <span data-ttu-id="bd1b3-215">[レイアウト] ページには、同じセクションを 2 回以上表示しようとするメソッドの呼び出しが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-215">The layout page includes method calls that try to render the same section more than once.</span></span>

<span data-ttu-id="bd1b3-216">ただし、レイアウト ページで省略可能にするのには、セクションを宣言することで、名前付きセクションのこの動作をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-216">However, you can override this behavior for a named section by declaring the section to be optional in the layout page.</span></span> <span data-ttu-id="bd1b3-217">これは、操作により、複数のコンテンツ ページ、レイアウト ページを共有することができますが、特定のセクションのコンテンツがないこともありますを定義できます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-217">This lets you define multiple content pages that can share a layout page but that might or might not have content for a specific section.</span></span>

1. <span data-ttu-id="bd1b3-218">開いている*Content2.cshtml*し、次のセクションを削除します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-218">Open *Content2.cshtml* and remove the following section:</span></span>

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. <span data-ttu-id="bd1b3-219">ページを保存し、ブラウザーで実行します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-219">Save the page and then run it in a browser.</span></span> <span data-ttu-id="bd1b3-220">コンテンツ ページ、レイアウト ページ、つまりヘッダー セクションで定義されているセクションのコンテンツは提供されないため、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-220">An error message is displayed, because the content page doesn't provide content for a section defined in the layout page, namely the header section.</span></span>

    ![ページを実行する場合に発生するエラーを示すスクリーン ショットが RenderSection メソッドを呼び出しますが、対応するセクションが指定されていません。](3-creating-a-consistent-look/_static/image7.jpg)
3. <span data-ttu-id="bd1b3-222">*Shared*フォルダーを開き、  *\_Layout2.cshtml*ページし、この行に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-222">In the *Shared* folder, open the *\_Layout2.cshtml* page and replace this line:</span></span>

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    <span data-ttu-id="bd1b3-223">次のコードでは。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-223">with the following code:</span></span>

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    <span data-ttu-id="bd1b3-224">代わりに、次のコードのブロックは、同じ結果を生成前のコード行を置き換える可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-224">As an alternative, you could replace the previous line of code with the following code block, which produces the same results:</span></span>

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. <span data-ttu-id="bd1b3-225">実行、 *Content2.cshtml*ブラウザーをもう一度のページです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-225">Run the *Content2.cshtml* page in a browser again.</span></span> <span data-ttu-id="bd1b3-226">(このページをブラウザーで開くもあれば、するのみ更新できます。)このページが表示エラーは、ヘッダーを持たない場合でも。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-226">(If you still have this page open in the browser, you can just refresh it.) This time the page is displayed with no error, even though it has no header.</span></span>

## <a name="passing-data-to-layout-pages"></a><span data-ttu-id="bd1b3-227">レイアウト ページにデータを渡す</span><span class="sxs-lookup"><span data-stu-id="bd1b3-227">Passing Data to Layout Pages</span></span>

<span data-ttu-id="bd1b3-228">レイアウト ページを参照する必要のあるコンテンツ ページで定義されているデータがあります。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-228">You might have data defined in the content page that you need to refer to in a layout page.</span></span> <span data-ttu-id="bd1b3-229">場合は、レイアウト ページに、コンテンツ ページからデータを渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-229">If so, you need to pass the data from the content page to the layout page.</span></span> <span data-ttu-id="bd1b3-230">ユーザーのログイン状態を表示するなど、ユーザー入力に基づいてコンテンツ領域を非表示にする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-230">For example, you might want to display the login status of a user, or you might want to show or hide content areas based on user input.</span></span>

<span data-ttu-id="bd1b3-231">レイアウト ページには、コンテンツ ページからデータを渡すするに値を配置することができます、`PageData`コンテンツ ページのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-231">To pass data from a content page to a layout page, you can put values into the `PageData` property of the content page.</span></span> <span data-ttu-id="bd1b3-232">`PageData`プロパティがページ間で通過するデータを保持する名前/値ペアのコレクション。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-232">The `PageData` property is a collection of name/value pairs that hold the data that you want to pass between pages.</span></span> <span data-ttu-id="bd1b3-233">レイアウト ページは、値のうち、読み取ることができます、`PageData`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-233">In the layout page, you can then read values out of the `PageData` property.</span></span>

<span data-ttu-id="bd1b3-234">別のダイアグラムを次に示します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-234">Here's another diagram.</span></span> <span data-ttu-id="bd1b3-235">ASP.NET の使用方法を示しますこのいずれか、`PageData`プロパティに値を渡すコンテンツ ページからページをレイアウトします。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-235">This one shows how ASP.NET can use the `PageData` property to pass values from a content page to the layout page.</span></span> <span data-ttu-id="bd1b3-236">ASP.NET web ページの作成の開始時に作成、`PageData`コレクション。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-236">When ASP.NET begins building the web page, it creates the `PageData` collection.</span></span> <span data-ttu-id="bd1b3-237">データを配置するコードを記述するコンテンツ ページで、`PageData`コレクション。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-237">In the content page, you write code to put data in the `PageData` collection.</span></span> <span data-ttu-id="bd1b3-238">値が、`PageData`コレクションは、コンテンツ ページの他のセクションで、またはコンテンツ ブロックを追加してもアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-238">Values in the `PageData` collection can also be accessed by other sections in the content page or by additional content blocks.</span></span>

![コンテンツ ページが PageData ディクショナリを設定およびレイアウト ページにその情報を渡す方法を示す概念図です。](3-creating-a-consistent-look/_static/image8.jpg)

<span data-ttu-id="bd1b3-240">次の手順では、レイアウト ページに、コンテンツ ページからデータを渡す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-240">The following procedure shows how to pass data from a content page to a layout page.</span></span> <span data-ttu-id="bd1b3-241">ページの実行時に、ユーザー、レイアウト ページで定義されている一覧を表示または非表示を切り替えるボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-241">When the page runs, it displays a button that lets the user hide or show a list that's defined in the layout page.</span></span> <span data-ttu-id="bd1b3-242">ユーザーは、ボタンをクリックして、内 (ブール値) を true または false 値のセット、`PageData`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-242">When users click the button, it sets a true/false (Boolean) value in the `PageData` property.</span></span> <span data-ttu-id="bd1b3-243">レイアウト ページは、その値を読み込んで、これが false の場合は、一覧を非表示にします。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-243">The layout page reads that value, and if it's false, hides the list.</span></span> <span data-ttu-id="bd1b3-244">値はでも使用コンテンツ ページを表示するかどうかを決定する、**リストの非表示に**ボタンまたは**リストの表示**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-244">The value is also used in the content page to determine whether to display the **Hide List** button or the **Show List** button.</span></span>

![[イメージ]](3-creating-a-consistent-look/_static/image9.jpg)

1. <span data-ttu-id="bd1b3-246">ルート フォルダーにという名前のファイルを作成する*Content3.cshtml*既存のコンテンツを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-246">In the root folder, create a file named *Content3.cshtml* and replace any existing content with the following:</span></span>

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    <span data-ttu-id="bd1b3-247">コード内のデータの 2 つの部分を格納する、`PageData`プロパティ &#8212; タイトル web ページと true または false に設定すると、一覧を表示するかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-247">The code stores two pieces of data in the `PageData` property &#8212; the title of the web page and true or false to specify whether to display a list.</span></span>

    <span data-ttu-id="bd1b3-248">ASP.NET を使うと条件付きでコード ブロックを使用して、ページに HTML マークアップを格納できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-248">Notice that ASP.NET lets you put HTML markup into the page conditionally using a code block.</span></span> <span data-ttu-id="bd1b3-249">たとえば、`if/else`ページの本文内のブロックを決定かどうかに応じてを表示するには、どのフォーム`PageData["ShowList"]`が設定を true にします。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-249">For example, the `if/else` block in the body of the page determines which form to display depending on whether `PageData["ShowList"]` is set to true.</span></span>
2. <span data-ttu-id="bd1b3-250">*Shared*フォルダー、という名前のファイルを作成する *\_Layout3.cshtml*既存のコンテンツを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-250">In the *Shared* folder, create a file named *\_Layout3.cshtml* and replace any existing content with the following:</span></span>

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    <span data-ttu-id="bd1b3-251">レイアウト ページには内の式が含まれています、`<title>`からタイトルの値を取得する要素、`PageData`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-251">The layout page includes an expression in the `<title>` element that gets the title value from the `PageData` property.</span></span> <span data-ttu-id="bd1b3-252">また、使用、`ShowList`の値、`PageData`リスト コンテンツ ブロックを表示するかどうかを決定するプロパティです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-252">It also uses the `ShowList` value of the `PageData` property to determine whether to display the list content block.</span></span>
3. <span data-ttu-id="bd1b3-253">*Shared*フォルダー、という名前のファイルを作成する *\_List.cshtml*既存のコンテンツを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-253">In the *Shared* folder, create a file named *\_List.cshtml* and replace any existing content with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. <span data-ttu-id="bd1b3-254">実行、 *Content3.cshtml*ブラウザーのページです。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-254">Run the *Content3.cshtml* page in a browser.</span></span> <span data-ttu-id="bd1b3-255">ページの左側に表示される一覧で、ページが表示されます、**リストの非表示に**下部にあるボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-255">The page is displayed with the list visible on the left side of the page and a **Hide List** button at the bottom.</span></span>

    ![ページ スクリーン ショットを含むリストとボタンの一覧を非表示にする と表示されています。](3-creating-a-consistent-look/_static/image10.jpg)
5. <span data-ttu-id="bd1b3-257">をクリックして **ボックスの一覧を非表示に**です。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-257">Click **Hide List**.</span></span> <span data-ttu-id="bd1b3-258">リストが消え、ボタンに変わります**リストの表示**です。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-258">The list disappears and the button changes to **Show List**.</span></span>

    ![リストとボタンの一覧を表示する と表示されているが含まれていません ページを示すスクリーン ショット。](3-creating-a-consistent-look/_static/image11.jpg)
6. <span data-ttu-id="bd1b3-260">クリックして、**リストの表示** ボタン、および一覧が再び表示されます。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-260">Click the **Show List** button, and the list is displayed again.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bd1b3-261">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="bd1b3-261">Additional Resources</span></span>


[<span data-ttu-id="bd1b3-262">ASP.NET Web ページのサイト全体の動作をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="bd1b3-262">Customizing Site-Wide Behavior for ASP.NET Web Pages</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
