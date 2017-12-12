---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: "Twitter の ASP.NET Web Pages でヘルパー |Microsoft ドキュメント"
author: tfitzmac
description: "このトピックおよびアプリケーションは、Twitter Helper を WebMatrix 3 プロジェクトに追加する方法を示します。 Twitter のヘルパー コードを含むし、ヘルパーを呼び出す方法を示しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="d6de1-104">Twitter でヘルパーを ASP.NET Web ページ</span><span class="sxs-lookup"><span data-stu-id="d6de1-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="d6de1-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d6de1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d6de1-106">このトピックおよびアプリケーションは、Twitter Helper を WebMatrix 3 プロジェクトに追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d6de1-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="d6de1-107">Twitter のヘルパー コードを含むし、ヘルパー メソッドを呼び出す方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="d6de1-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="d6de1-108">Twitter.cshtml ファイルのこのコードは、によって開発された**Tian パン**Microsoft のです。</span><span class="sxs-lookup"><span data-stu-id="d6de1-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d6de1-109">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="d6de1-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d6de1-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d6de1-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="d6de1-111">このチュートリアルは、ASP.NET Web Pages 2 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="d6de1-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="d6de1-112">はじめに</span><span class="sxs-lookup"><span data-stu-id="d6de1-112">Introduction</span></span>

<span data-ttu-id="d6de1-113">このトピックでは、Twitter Helper をアプリケーションに追加して、ヘルパー メソッドの呼び出しに Razor 構文を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d6de1-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="d6de1-114">Twitter Helper を使用を簡単に Twitter ボタンと、アプリケーションでウィジェットを反映します。</span><span class="sxs-lookup"><span data-stu-id="d6de1-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="d6de1-115">ユーザーのタイムラインをハッシュタグの検索結果などの Twitter ウィジェットを使用する必要がありますまずを作成する、 [twitter ウィジェット](https://twitter.com/settings/widgets)です。</span><span class="sxs-lookup"><span data-stu-id="d6de1-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="d6de1-116">独自のウィジェットを作成した後は、ウィジェット id を受け取ります。このウィジェット id は、ウィジェットを表示するヘルパー メソッドを呼び出すときにパラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="d6de1-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="d6de1-117">このトピックは、Twitter API のバージョン 1.1 に記述されています。</span><span class="sxs-lookup"><span data-stu-id="d6de1-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="d6de1-118">直接 Twitter ヘルパー コードを追加すると、プロジェクトに、Twitter API が変更された場合、ヘルパー コードを更新できます。</span><span class="sxs-lookup"><span data-stu-id="d6de1-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="d6de1-119">WebMatrix のインストール方法の詳細については、次を参照してください。 [Introducing ASP.NET Web Pages 2 - はじめ](../getting-started/introducing-aspnet-web-pages-2/getting-started.md)です。</span><span class="sxs-lookup"><span data-stu-id="d6de1-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="d6de1-120">Twitter Helper プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="d6de1-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="d6de1-121">Twitter Helper を追加するには、最初に、という名前のフォルダーを追加**アプリ\_コード**をプロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="d6de1-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="d6de1-122">という名前のファイルを作成し、 **Twitter.cshtml**です。</span><span class="sxs-lookup"><span data-stu-id="d6de1-122">Then, create a file named **Twitter.cshtml**.</span></span>

![App_Code フォルダー](twitter-helper/_static/image1.png)

<span data-ttu-id="d6de1-124">Twitter.cshtml の既定のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d6de1-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="d6de1-125">Web ページから Twitter メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="d6de1-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="d6de1-126">次の例では、プロジェクト内のページから Twitter のヘルパー メソッドを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d6de1-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="d6de1-127">プロジェクトではするパラメーター値は、お客様のニーズに関連する値に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d6de1-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="d6de1-128">指定されたウィジェット id を使用すると、メソッドの動作を調べるがプロジェクト用の独自のウィジェットを生成します。</span><span class="sxs-lookup"><span data-stu-id="d6de1-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="d6de1-129">すべての次に示すパラメーターが必要です。</span><span class="sxs-lookup"><span data-stu-id="d6de1-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="d6de1-130">省略可能なパラメーターを使用するには、ボタンまたはウィジェットの表示方法をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="d6de1-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="d6de1-131">たとえば、以下のボタンに従うには、ユーザー名のみが必要ですが、フォローの番号を含めるし、ボタンと言語のサイズを指定する方法を方法の例に示します。</span><span class="sxs-lookup"><span data-stu-id="d6de1-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="d6de1-132">結果を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d6de1-132">See the results</span></span>

<span data-ttu-id="d6de1-133">上記のコードには、次のボタンとウィジェットが生成されます。</span><span class="sxs-lookup"><span data-stu-id="d6de1-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="d6de1-134">これらのボタンとウィジェットが完全に機能、スクリーン ショットではありません。</span><span class="sxs-lookup"><span data-stu-id="d6de1-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="d6de1-135">言語パラメーター設定されているためスペイン語で以下のボタンが表示されます**es**です。</span><span class="sxs-lookup"><span data-stu-id="d6de1-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="d6de1-136">次のボタン</span><span class="sxs-lookup"><span data-stu-id="d6de1-136">Follow Button</span></span>

<span data-ttu-id="d6de1-137">[次の@aspnet)](https://twitter.com/aspnet)<script>! (d、s、id) 関数 {var js、fjs d.getElementsByTagName(s) [0], p =/^http:/.test(d.location) を = ですか?'http': 'https';場合 (! d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore (js、fjs);}}(ドキュメント、'script'、' twitter wjs');</script></span><span class="sxs-lookup"><span data-stu-id="d6de1-137">[Follow @aspnet)](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script></span></span>

### <a name="tweet-button"></a><span data-ttu-id="d6de1-138">ツイート ボタン</span><span class="sxs-lookup"><span data-stu-id="d6de1-138">Tweet Button</span></span>

<span data-ttu-id="d6de1-139">[ツイート](https://twitter.com/share)<script>! (d、s、id) 関数 {var js、fjs d.getElementsByTagName(s) [0], p =/^http:/.test(d.location) を = ですか?'http': 'https';場合 (! d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore (js、fjs);}}(ドキュメント、'script'、' twitter wjs');</script></span><span class="sxs-lookup"><span data-stu-id="d6de1-139">[Tweet](https://twitter.com/share)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script></span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="d6de1-140">ユーザー タイムライン (プロファイル)</span><span class="sxs-lookup"><span data-stu-id="d6de1-140">User Timeline (Profile)</span></span>

<span data-ttu-id="d6de1-141">[によってツイート@aspnet ](https://twitter.com/aspnet) <script>! (d、s、id) 関数 {var js、fjs d.getElementsByTagName(s) [0], p =/^http:/.test(d.location) を = ですか?'http': 'https';場合 (! d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +"://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js、fjs);}}(ドキュメント、"script"、"twitter wjs") です。</script></span><span class="sxs-lookup"><span data-stu-id="d6de1-141">[Tweets by @aspnet](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>

### <a name="favorites"></a><span data-ttu-id="d6de1-142">お気に入り</span><span class="sxs-lookup"><span data-stu-id="d6de1-142">Favorites</span></span>

<span data-ttu-id="d6de1-143">[お気に入りツイート@Microsoft ](https://twitter.com/Microsoft/favorites) <script>! (d、s、id) 関数 {var js、fjs d.getElementsByTagName(s) [0], p =/^http:/.test(d.location) を = ですか?'http': 'https';場合 (! d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +"://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js、fjs);}}(ドキュメント、"script"、"twitter wjs") です。</script></span><span class="sxs-lookup"><span data-stu-id="d6de1-143">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>

### <a name="list"></a><span data-ttu-id="d6de1-144">リスト</span><span class="sxs-lookup"><span data-stu-id="d6de1-144">List</span></span>

<span data-ttu-id="d6de1-145">[ツイート@Microsoft/MS\_コンシューマー\_バンド](https://twitter.com/microsoft/ms-consumer-brands/)<script>! (d、s、id) 関数 {var js、fjs d.getElementsByTagName(s) [0], p =/^http:/.test(d.location) を = ですか?'http': 'https';場合 (! d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +"://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js、fjs);}}(ドキュメント、"script"、"twitter wjs") です。</script></span><span class="sxs-lookup"><span data-stu-id="d6de1-145">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>

### <a name="search"></a><span data-ttu-id="d6de1-146">検索</span><span class="sxs-lookup"><span data-stu-id="d6de1-146">Search</span></span>

<span data-ttu-id="d6de1-147">[に関するツイート&quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>! (d、s、id) 関数 {var js、fjs d.getElementsByTagName(s) [0], p =/^http:/.test(d.location) を = ですか?'http': 'https';場合 (! d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +"://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js、fjs);}}(ドキュメント、"script"、"twitter wjs") です。</script></span><span class="sxs-lookup"><span data-stu-id="d6de1-147">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>
