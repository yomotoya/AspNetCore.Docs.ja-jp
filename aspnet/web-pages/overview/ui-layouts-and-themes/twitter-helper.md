---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter ヘルパーと ASP.NET Web Pages |Microsoft Docs
author: Rick-Anderson
description: このトピックで、アプリケーションは、Twitter ヘルパーを WebMatrix 3 プロジェクトに追加する方法を示します。 Twitter ヘルパー コードが含まれ、ヘルパーを呼び出す方法を示しています.
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: fabe9f2b84d278a766dc8d8b7bfc00e1eb967127
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299431"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="7d790-104">Twitter ヘルパーと ASP.NET Web ページ</span><span class="sxs-lookup"><span data-stu-id="7d790-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="7d790-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7d790-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7d790-106">Twitter ヘルパーは廃止されました。</span><span class="sxs-lookup"><span data-stu-id="7d790-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="7d790-107">最新 engagement ツールの web サイト、Twitter を参照してください。 [web サイトの概要の Twitter](https://developer.twitter.com/en/docs/twitter-for-websites/overview)します。</span><span class="sxs-lookup"><span data-stu-id="7d790-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="7d790-108">このトピックで、アプリケーションは、Twitter ヘルパーを WebMatrix 3 プロジェクトに追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7d790-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="7d790-109">Twitter ヘルパー コードが含まれ、ヘルパー メソッドを呼び出す方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="7d790-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="7d790-110">Twitter.cshtml ファイル用のこのコードは、開発された**Tian パン**Microsoft の。</span><span class="sxs-lookup"><span data-stu-id="7d790-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7d790-111">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="7d790-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7d790-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="7d790-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="7d790-113">このチュートリアルは、ASP.NET Web Pages 2 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="7d790-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="7d790-114">はじめに</span><span class="sxs-lookup"><span data-stu-id="7d790-114">Introduction</span></span>

<span data-ttu-id="7d790-115">このトピックでは、アプリケーションに Twitter ヘルパーを追加して、ヘルパー メソッドを呼び出す Razor 構文を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7d790-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="7d790-116">Twitter ヘルパーでは、簡単に Twitter ボタンと、アプリケーションでウィジェットを組み込めます。</span><span class="sxs-lookup"><span data-stu-id="7d790-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="7d790-117">ユーザーのタイムラインや、ハッシュタグの検索結果などの Twitter ウィジェットを使用する必要があります最初に作成する、 [twitter ウィジェット](https://twitter.com/settings/widgets)します。</span><span class="sxs-lookup"><span data-stu-id="7d790-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="7d790-118">ウィジェットを作成した後は、ウィジェット id を受け取ります。ウィジェットを表示するヘルパー メソッドを呼び出すときに、このウィジェット id をパラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="7d790-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="7d790-119">このトピックでは、Twitter API のバージョン 1.1 用に記述されています。</span><span class="sxs-lookup"><span data-stu-id="7d790-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="7d790-120">直接 Twitter ヘルパー コードを追加すると、プロジェクトに、Twitter API が変更された場合、ヘルパー コードを更新できます。</span><span class="sxs-lookup"><span data-stu-id="7d790-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="7d790-121">WebMatrix のインストール方法の詳細については、次を参照してください。 [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md)します。</span><span class="sxs-lookup"><span data-stu-id="7d790-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="7d790-122">Twitter ヘルパーをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="7d790-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="7d790-123">Twitter ヘルパーを追加するには、最初に、という名前のフォルダーを追加**アプリ\_コード**をプロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="7d790-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="7d790-124">という名前のファイルを作成し、 **Twitter.cshtml**します。</span><span class="sxs-lookup"><span data-stu-id="7d790-124">Then, create a file named **Twitter.cshtml**.</span></span>

![App_Code フォルダー](twitter-helper/_static/image1.png)

<span data-ttu-id="7d790-126">Twitter.cshtml の既定のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7d790-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="7d790-127">Web ページから Twitter メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="7d790-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="7d790-128">次の例では、プロジェクトで、ページから Twitter ヘルパー メソッドを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7d790-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="7d790-129">プロジェクトのニーズに関連する値を持つパラメーターの値を置換するは。</span><span class="sxs-lookup"><span data-stu-id="7d790-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="7d790-130">指定されたウィジェット id を使用するには、メソッドの動作を探索するが、プロジェクトの独自のウィジェットを生成します。</span><span class="sxs-lookup"><span data-stu-id="7d790-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="7d790-131">すべての以下のパラメーターが必要です。</span><span class="sxs-lookup"><span data-stu-id="7d790-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="7d790-132">省略可能なパラメーターは、ボタンまたはウィジェットの表示方法をカスタマイズに使用されます。</span><span class="sxs-lookup"><span data-stu-id="7d790-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="7d790-133">たとえば、次のボタンにするには、ユーザー名のみが必要ですが、フォロワー数を含めるし、ボタンと言語のサイズを指定する方法する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7d790-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="7d790-134">結果を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7d790-134">See the results</span></span>

<span data-ttu-id="7d790-135">上記のコードには、次のボタンとウィジェットが生成されます。</span><span class="sxs-lookup"><span data-stu-id="7d790-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="7d790-136">これらのボタンとウィジェットは完全に機能、スクリーン ショットではありません。</span><span class="sxs-lookup"><span data-stu-id="7d790-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="7d790-137">言語パラメーター設定されているために、スペイン語で以下のボタンが表示されます**es**します。</span><span class="sxs-lookup"><span data-stu-id="7d790-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="7d790-138">次のボタン</span><span class="sxs-lookup"><span data-stu-id="7d790-138">Follow Button</span></span>

<span data-ttu-id="7d790-139">[次の@aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="7d790-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="7d790-140">ツイート ボタン</span><span class="sxs-lookup"><span data-stu-id="7d790-140">Tweet Button</span></span>

<span data-ttu-id="7d790-141">[ツイート](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="7d790-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="7d790-142">ユーザー タイムライン (プロファイル)</span><span class="sxs-lookup"><span data-stu-id="7d790-142">User Timeline (Profile)</span></span>

<span data-ttu-id="7d790-143">[別のツイート @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="7d790-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="7d790-144">お気に入り</span><span class="sxs-lookup"><span data-stu-id="7d790-144">Favorites</span></span>

<span data-ttu-id="7d790-145">[ツイートのお気に入り @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="7d790-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="7d790-146">リスト</span><span class="sxs-lookup"><span data-stu-id="7d790-146">List</span></span>

<span data-ttu-id="7d790-147">[ツイート@Microsoft/MS\_コンシューマー\_バンド](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="7d790-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="7d790-148">検索</span><span class="sxs-lookup"><span data-stu-id="7d790-148">Search</span></span>

<span data-ttu-id="7d790-149">[についてツイート&quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="7d790-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
