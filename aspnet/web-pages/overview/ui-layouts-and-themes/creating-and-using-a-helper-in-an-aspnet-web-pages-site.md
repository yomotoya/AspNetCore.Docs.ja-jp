---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: 作成して、ヘルパーを使用して ASP.NET web Pages (Razor) サイト |Microsoft Docs
author: tfitzmac
description: この記事では、ASP.NET Web Pages (Razor) の web サイトでヘルパーを作成する方法について説明します。 コードとパフォーマンスにマークアップを含む再利用可能なコンポーネントをヘルパーには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5e73beabf973a20335112a0d7dc5ecb6b8817744
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394613"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="07846-104">ASP.NET Web Pages (Razor) サイトで作成およびヘルパー</span><span class="sxs-lookup"><span data-stu-id="07846-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="07846-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="07846-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="07846-106">この記事では、ASP.NET Web Pages (Razor) の web サイトでヘルパーを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="07846-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="07846-107">A*ヘルパー*はコードと面倒か複雑になる可能性があるタスクを実行するためのマークアップを含む再利用可能なコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="07846-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="07846-108">**学習内容。**</span><span class="sxs-lookup"><span data-stu-id="07846-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="07846-109">作成して単純なヘルパーを使用する方法。</span><span class="sxs-lookup"><span data-stu-id="07846-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="07846-110">この記事で導入された ASP.NET 機能を次に示します。</span><span class="sxs-lookup"><span data-stu-id="07846-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="07846-111">`@helper`構文。</span><span class="sxs-lookup"><span data-stu-id="07846-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="07846-112">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="07846-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="07846-113">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="07846-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="07846-114">このチュートリアルは、ASP.NET Web Pages 2 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="07846-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="07846-115">ヘルパーの概要</span><span class="sxs-lookup"><span data-stu-id="07846-115">Overview of Helpers</span></span>

<span data-ttu-id="07846-116">サイト内のさまざまなページで、同じタスクを実行する必要がある場合は、ヘルパーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="07846-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="07846-117">ASP.NET Web ページには、さまざまなヘルパーが含まれています。 また、ダウンロードしてインストールをさらに多くがあります。</span><span class="sxs-lookup"><span data-stu-id="07846-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="07846-118">(ASP.NET Web Pages での組み込みのヘルパーの一覧が記載されて、 [ASP.NET API クイック リファレンス](https://go.microsoft.com/fwlink/?LinkId=202907))。ニーズを満たす既存のヘルパーの場合は、独自のヘルパーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="07846-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="07846-119">ヘルパーを使用して、複数のページ、一般的なコードのブロックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="07846-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="07846-120">ページに多くの場合する通常の段落とは別に設定されている注項目を作成するとします。</span><span class="sxs-lookup"><span data-stu-id="07846-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="07846-121">としてメモを作成するなど、`<div>`要素を境界線付きのボックスとしてスタイルが適用されます。</span><span class="sxs-lookup"><span data-stu-id="07846-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="07846-122">メモを表示するたびに、この同じマークアップをページに追加ではなく、ヘルパーとしてマークアップをパッケージ化することができます。</span><span class="sxs-lookup"><span data-stu-id="07846-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="07846-123">メモに 1 行のコードを挿入して必要な任意の場所。</span><span class="sxs-lookup"><span data-stu-id="07846-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="07846-124">このようなヘルパーを使用しては各ページでにより、コードと簡素化され、読みやすくします。</span><span class="sxs-lookup"><span data-stu-id="07846-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="07846-125">簡単に、サイトを維持するために、ノートを検索する方法を変更する必要がある場合は、1 か所でマークアップを変更できるためです。</span><span class="sxs-lookup"><span data-stu-id="07846-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="07846-126">ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="07846-126">Creating a Helper</span></span>

<span data-ttu-id="07846-127">この手順では、前述のように、メモを作成するヘルパーを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="07846-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="07846-128">これは単純な例ですが、カスタム ヘルパーは、マークアップと必要な ASP.NET コードを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="07846-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="07846-129">という名前のフォルダーを作成、web サイトのルート フォルダーに*アプリ\_コード*します。</span><span class="sxs-lookup"><span data-stu-id="07846-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="07846-130">これは、ヘルパーなどのコンポーネントのコードを配置する ASP.NET の予約済みフォルダーの名前です。</span><span class="sxs-lookup"><span data-stu-id="07846-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="07846-131">*アプリ\_コード*フォルダーを作成する新しい *.cshtml*ファイルし、名前を付けます*MyHelpers.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="07846-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="07846-132">次のように、既存のコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="07846-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="07846-133">コードを使用して、`@helper`という名前の新しいヘルパーを宣言する構文`MakeNote`します。</span><span class="sxs-lookup"><span data-stu-id="07846-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="07846-134">この特定のヘルパーには、という名前のパラメーターを渡すことができます`content`テキストとマークアップの組み合わせを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="07846-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="07846-135">ヘルパーにメモの本文を使用した文字列を挿入する、`@content`変数。</span><span class="sxs-lookup"><span data-stu-id="07846-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="07846-136">ファイルの名前はことに注意してください。 *MyHelpers.cshtml*、ヘルパーがという名前が`MakeNote`します。</span><span class="sxs-lookup"><span data-stu-id="07846-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="07846-137">複数のカスタム ヘルパーは、1 つのファイルに配置できます。</span><span class="sxs-lookup"><span data-stu-id="07846-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="07846-138">ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="07846-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="07846-139">ページで、ヘルパーの使用</span><span class="sxs-lookup"><span data-stu-id="07846-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="07846-140">呼ばれる新しい空のファイルを作成、ルート フォルダーで*TestHelper.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="07846-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="07846-141">次のコードをファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="07846-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="07846-142">作成したヘルパーを呼び出すには、次のように使用します。`@`ヘルパーの名前と場所、ヘルパーは、ドットのファイル名が続きます。</span><span class="sxs-lookup"><span data-stu-id="07846-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="07846-143">(複数のフォルダーがある場合、*アプリ\_コード*フォルダー、構文を使用する`@FolderName.FileName.HelperName`入れ子になったフォルダー レベルのいずれかのヘルパーを呼び出す)。</span><span class="sxs-lookup"><span data-stu-id="07846-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="07846-144">かっこ内の引用符で追加したテキストは、ヘルパーは、web ページの注意事項の一部として表示されるテキストです。</span><span class="sxs-lookup"><span data-stu-id="07846-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="07846-145">ページを保存し、ブラウザーで実行します。</span><span class="sxs-lookup"><span data-stu-id="07846-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="07846-146">ヘルパー、ヘルパーが呼び出されている右注項目を生成する: 2 つの段落の間。</span><span class="sxs-lookup"><span data-stu-id="07846-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![ヘルパーが、指定したテキストを囲むボックスを配置するマークアップを生成する方法と、ブラウザーでページを示すスクリーン ショット。](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="07846-148">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="07846-148">Additional Resources</span></span>


<span data-ttu-id="07846-149">[Razor ヘルパーとしての水平メニュー](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341)します。</span><span class="sxs-lookup"><span data-stu-id="07846-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="07846-150">Mike 教皇によるブログ エントリでは、マークアップ、CSS、およびコードを使用するヘルパーと水平方向のメニューを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="07846-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="07846-151">[For WebMatrix and ASP.NET MVC3 ページ ヘルパーを ASP.NET Web での HTML5 の活用](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="07846-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="07846-152">Sam Abraham によるブログ エントリは、HTML5 を表示するヘルパーを示しています。`Canvas`要素。</span><span class="sxs-lookup"><span data-stu-id="07846-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="07846-153">[間の差@Helpersと@FunctionsWebMatrix で](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix)します。</span><span class="sxs-lookup"><span data-stu-id="07846-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="07846-154">Mike Brind によるブログ エントリについて説明します`@helper`構文と`@function`構文と使用すると、それぞれを使用します。</span><span class="sxs-lookup"><span data-stu-id="07846-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
