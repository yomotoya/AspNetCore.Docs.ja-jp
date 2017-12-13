---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: "作成して、ヘルパーを使用して ASP.NET web Pages (Razor) サイト |Microsoft ドキュメント"
author: tfitzmac
description: "この記事では、ASP.NET Web Pages (Razor) の web サイトにヘルパーを作成する方法について説明します。 コードとパフォーマンスにマークアップを含む再使用可能なコンポーネントをヘルパーには."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="a212c-104">ASP.NET Web Pages (Razor) サイトで作成およびヘルパー</span><span class="sxs-lookup"><span data-stu-id="a212c-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="a212c-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a212c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a212c-106">この記事では、ASP.NET Web Pages (Razor) の web サイトにヘルパーを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a212c-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="a212c-107">A*ヘルパー*コードと面倒か複雑になる可能性があるタスクを実行するマークアップを含む再使用可能なコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="a212c-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="a212c-108">**学習する内容。**</span><span class="sxs-lookup"><span data-stu-id="a212c-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="a212c-109">作成して単純なヘルパーを使用する方法。</span><span class="sxs-lookup"><span data-stu-id="a212c-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="a212c-110">アーティクルで導入された ASP.NET 機能を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a212c-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="a212c-111">`@helper`構文です。</span><span class="sxs-lookup"><span data-stu-id="a212c-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a212c-112">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="a212c-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a212c-113">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="a212c-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="a212c-114">このチュートリアルは、ASP.NET Web Pages 2 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="a212c-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="a212c-115">ヘルパーの概要</span><span class="sxs-lookup"><span data-stu-id="a212c-115">Overview of Helpers</span></span>

<span data-ttu-id="a212c-116">サイト内のさまざまなページで、同じタスクを実行する必要がある場合は、ヘルパーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="a212c-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="a212c-117">ASP.NET Web ページには、ヘルパーの多くが含まれていて、ダウンロードしてインストールすることが多くがあります。</span><span class="sxs-lookup"><span data-stu-id="a212c-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="a212c-118">(ASP.NET Web Pages での組み込みのヘルパーの一覧が記載されて、 [ASP.NET API クイック リファレンス](https://go.microsoft.com/fwlink/?LinkId=202907))。お客様のニーズを満たしていない既存のヘルパーの場合は、独自のヘルパーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="a212c-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="a212c-119">ヘルパーを使用して、複数のページにわたって共通コード ブロックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="a212c-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="a212c-120">ページに多くの場合とは別に標準の段落に設定されている注項目を作成するとします。</span><span class="sxs-lookup"><span data-stu-id="a212c-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="a212c-121">としてメモを作成するなど、`<div>`要素をボックスに罫線のスタイルがします。</span><span class="sxs-lookup"><span data-stu-id="a212c-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="a212c-122">メモを表示するたびに、ページにこの同じマークアップを追加するのではなく、ヘルパーとしてマークアップをパッケージ化することができます。</span><span class="sxs-lookup"><span data-stu-id="a212c-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="a212c-123">メモに 1 行のコードを挿入することができますし、必要な任意の場所。</span><span class="sxs-lookup"><span data-stu-id="a212c-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="a212c-124">次のようなヘルパーの使用は、ページの各により、コードと簡素化されを読みやすくします。</span><span class="sxs-lookup"><span data-stu-id="a212c-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="a212c-125">これもやすく、サイトを維持するために、ノートを検索する方法を変更する必要がある場合は、1 か所でマークアップを変更できるためです。</span><span class="sxs-lookup"><span data-stu-id="a212c-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="a212c-126">ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="a212c-126">Creating a Helper</span></span>

<span data-ttu-id="a212c-127">この手順では、上記のメモを作成するヘルパーを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a212c-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="a212c-128">これは単純な例ですが、カスタム ヘルパーは、マークアップとする必要がある ASP.NET コードを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="a212c-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="a212c-129">という名前のフォルダーを作成、web サイトのルート フォルダーに*アプリ\_コード*です。</span><span class="sxs-lookup"><span data-stu-id="a212c-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="a212c-130">これは、ヘルパーなどのコンポーネントのコードを配置できる ASP.NET で予約されたフォルダーの名前です。</span><span class="sxs-lookup"><span data-stu-id="a212c-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="a212c-131">*アプリ\_コード*フォルダーを作成、新しい*.cshtml*ファイルし、名前を付けます*MyHelpers.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="a212c-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="a212c-132">次のように、既存のコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a212c-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="a212c-133">コードを使用して、`@helper`という名前の新しいヘルパーを宣言する構文`MakeNote`です。</span><span class="sxs-lookup"><span data-stu-id="a212c-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="a212c-134">この特定のヘルパーには、という名前のパラメーターを渡すことができます`content`テキストとマークアップの組み合わせを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="a212c-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="a212c-135">ヘルパーが注本文を使用して、文字列を挿入、`@content`変数。</span><span class="sxs-lookup"><span data-stu-id="a212c-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="a212c-136">ファイルの名前はことに注意してください。 *MyHelpers.cshtml*、ヘルパーがという名前が、`MakeNote`です。</span><span class="sxs-lookup"><span data-stu-id="a212c-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="a212c-137">1 つのファイルに複数のカスタム ヘルパーを配置することができます。</span><span class="sxs-lookup"><span data-stu-id="a212c-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="a212c-138">ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="a212c-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="a212c-139">ページで、ヘルパーの使用</span><span class="sxs-lookup"><span data-stu-id="a212c-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="a212c-140">ルート フォルダーと呼ばれる新しい空のファイルを作成する*TestHelper.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="a212c-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="a212c-141">次のコードをファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="a212c-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="a212c-142">作成するヘルパーを呼び出すには、次のように使用します。`@`ここで、ヘルパーは、ドット、ファイル名および順ヘルパー名。</span><span class="sxs-lookup"><span data-stu-id="a212c-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="a212c-143">(複数のフォルダーが含まれていた、*アプリ\_コード*フォルダー、構文を使用する可能性があります`@FolderName.FileName.HelperName`内でいずれかのヘルパーを呼び出す入れ子フォルダー レベル)。</span><span class="sxs-lookup"><span data-stu-id="a212c-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="a212c-144">かっこで囲まれた引用符で追加したテキストは、ヘルパーは、ノートの web ページの一部として表示されるテキストです。</span><span class="sxs-lookup"><span data-stu-id="a212c-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="a212c-145">ページを保存し、ブラウザーで実行します。</span><span class="sxs-lookup"><span data-stu-id="a212c-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="a212c-146">ヘルパーはヘルパーが呼び出されている右注項目を生成します。 2 つの段落です。</span><span class="sxs-lookup"><span data-stu-id="a212c-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![ブラウザーとヘルパーが指定されたテキストを囲むボックスを配置するマークアップを生成する方法でページを示すスクリーン ショット。](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="a212c-148">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="a212c-148">Additional Resources</span></span>


<span data-ttu-id="a212c-149">[Razor ヘルパーとしての水平方向のメニュー](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341)です。</span><span class="sxs-lookup"><span data-stu-id="a212c-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="a212c-150">Mike 教皇によるブログ エントリでは、マークアップ、CSS、およびコードを使用するヘルパーと水平方向のメニューを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a212c-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="a212c-151">[For WebMatrix and ASP.NET MVC3 ページ ヘルパーを活用する ASP.NET Web での HTML5](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="a212c-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="a212c-152">Sam 西によるブログ エントリは、HTML5 をレンダリングするヘルパーを示しています。`Canvas`要素。</span><span class="sxs-lookup"><span data-stu-id="a212c-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="a212c-153">[違い@Helpersと@FunctionsWebMatrix で](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix)です。</span><span class="sxs-lookup"><span data-stu-id="a212c-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="a212c-154">Mike Brind によるブログ エントリについて説明します`@helper`構文と`@function`構文と、それぞれを使用します。</span><span class="sxs-lookup"><span data-stu-id="a212c-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
