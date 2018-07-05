---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter ヘルパーと ASP.NET Web Pages |Microsoft Docs
author: tfitzmac
description: このトピックで、アプリケーションは、Twitter ヘルパーを WebMatrix 3 プロジェクトに追加する方法を示します。 Twitter ヘルパー コードが含まれ、ヘルパーを呼び出す方法を示しています.
ms.author: aspnetcontent
ms.date: 02/07/2014
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 1824b677a7ba96ea6fc5119610725a30d472764e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817656"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="2adbe-104">Twitter ヘルパーと ASP.NET Web ページ</span><span class="sxs-lookup"><span data-stu-id="2adbe-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="2adbe-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2adbe-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2adbe-106">このトピックで、アプリケーションは、Twitter ヘルパーを WebMatrix 3 プロジェクトに追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2adbe-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="2adbe-107">Twitter ヘルパー コードが含まれ、ヘルパー メソッドを呼び出す方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="2adbe-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="2adbe-108">Twitter.cshtml ファイル用のこのコードは、開発された**Tian パン**Microsoft の。</span><span class="sxs-lookup"><span data-stu-id="2adbe-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2adbe-109">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="2adbe-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2adbe-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="2adbe-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="2adbe-111">このチュートリアルは、ASP.NET Web Pages 2 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="2adbe-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="2adbe-112">はじめに</span><span class="sxs-lookup"><span data-stu-id="2adbe-112">Introduction</span></span>

<span data-ttu-id="2adbe-113">このトピックでは、アプリケーションに Twitter ヘルパーを追加して、ヘルパー メソッドを呼び出す Razor 構文を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2adbe-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="2adbe-114">Twitter ヘルパーでは、簡単に Twitter ボタンと、アプリケーションでウィジェットを組み込めます。</span><span class="sxs-lookup"><span data-stu-id="2adbe-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="2adbe-115">ユーザーのタイムラインや、ハッシュタグの検索結果などの Twitter ウィジェットを使用する必要があります最初に作成する、 [twitter ウィジェット](https://twitter.com/settings/widgets)します。</span><span class="sxs-lookup"><span data-stu-id="2adbe-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="2adbe-116">ウィジェットを作成した後は、ウィジェット id を受け取ります。ウィジェットを表示するヘルパー メソッドを呼び出すときに、このウィジェット id をパラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="2adbe-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="2adbe-117">このトピックでは、Twitter API のバージョン 1.1 用に記述されています。</span><span class="sxs-lookup"><span data-stu-id="2adbe-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="2adbe-118">直接 Twitter ヘルパー コードを追加すると、プロジェクトに、Twitter API が変更された場合、ヘルパー コードを更新できます。</span><span class="sxs-lookup"><span data-stu-id="2adbe-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="2adbe-119">WebMatrix のインストール方法の詳細については、次を参照してください。 [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md)します。</span><span class="sxs-lookup"><span data-stu-id="2adbe-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="2adbe-120">Twitter ヘルパーをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="2adbe-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="2adbe-121">Twitter ヘルパーを追加するには、最初に、という名前のフォルダーを追加**アプリ\_コード**をプロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="2adbe-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="2adbe-122">という名前のファイルを作成し、 **Twitter.cshtml**します。</span><span class="sxs-lookup"><span data-stu-id="2adbe-122">Then, create a file named **Twitter.cshtml**.</span></span>

![App_Code フォルダー](twitter-helper/_static/image1.png)

<span data-ttu-id="2adbe-124">Twitter.cshtml の既定のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2adbe-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="2adbe-125">Web ページから Twitter メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="2adbe-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="2adbe-126">次の例では、プロジェクトで、ページから Twitter ヘルパー メソッドを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2adbe-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="2adbe-127">プロジェクトのニーズに関連する値を持つパラメーターの値を置換するは。</span><span class="sxs-lookup"><span data-stu-id="2adbe-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="2adbe-128">指定されたウィジェット id を使用するには、メソッドの動作を探索するが、プロジェクトの独自のウィジェットを生成します。</span><span class="sxs-lookup"><span data-stu-id="2adbe-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="2adbe-129">すべての以下のパラメーターが必要です。</span><span class="sxs-lookup"><span data-stu-id="2adbe-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="2adbe-130">省略可能なパラメーターは、ボタンまたはウィジェットの表示方法をカスタマイズに使用されます。</span><span class="sxs-lookup"><span data-stu-id="2adbe-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="2adbe-131">たとえば、次のボタンにするには、ユーザー名のみが必要ですが、フォロワー数を含めるし、ボタンと言語のサイズを指定する方法する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2adbe-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="2adbe-132">結果を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2adbe-132">See the results</span></span>

<span data-ttu-id="2adbe-133">上記のコードには、次のボタンとウィジェットが生成されます。</span><span class="sxs-lookup"><span data-stu-id="2adbe-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="2adbe-134">これらのボタンとウィジェットは完全に機能、スクリーン ショットではありません。</span><span class="sxs-lookup"><span data-stu-id="2adbe-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="2adbe-135">言語パラメーター設定されているために、スペイン語で以下のボタンが表示されます**es**します。</span><span class="sxs-lookup"><span data-stu-id="2adbe-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="2adbe-136">次のボタン</span><span class="sxs-lookup"><span data-stu-id="2adbe-136">Follow Button</span></span>

<span data-ttu-id="2adbe-137">[次の@aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="2adbe-137">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="2adbe-138">ツイート ボタン</span><span class="sxs-lookup"><span data-stu-id="2adbe-138">Tweet Button</span></span>

<span data-ttu-id="2adbe-139">[ツイート](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="2adbe-139">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="2adbe-140">ユーザー タイムライン (プロファイル)</span><span class="sxs-lookup"><span data-stu-id="2adbe-140">User Timeline (Profile)</span></span>

<span data-ttu-id="2adbe-141">[別のツイート @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="2adbe-141">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="2adbe-142">お気に入り</span><span class="sxs-lookup"><span data-stu-id="2adbe-142">Favorites</span></span>

<span data-ttu-id="2adbe-143">[ツイートのお気に入り @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="2adbe-143">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="2adbe-144">リスト</span><span class="sxs-lookup"><span data-stu-id="2adbe-144">List</span></span>

<span data-ttu-id="2adbe-145">[ツイート@Microsoft/MS\_コンシューマー\_バンド](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="2adbe-145">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="2adbe-146">検索</span><span class="sxs-lookup"><span data-stu-id="2adbe-146">Search</span></span>

<span data-ttu-id="2adbe-147">[についてツイート&quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="2adbe-147">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
