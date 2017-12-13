---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: "によるパフォーマンスの向上の出力キャッシュ (c#) |Microsoft ドキュメント"
author: microsoft
description: "このチュートリアルでは、出力キャッシュを活用して、ASP.NET MVC web アプリケーションのパフォーマンスを大幅に向上する方法を学習します。 あなたが。。。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 47f0aa976c5876991ccc2406fb8f7402e59ec556
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="improving-performance-with-output-caching-c"></a><span data-ttu-id="a9821-104">出力キャッシュ (c#) によるパフォーマンスの向上</span><span class="sxs-lookup"><span data-stu-id="a9821-104">Improving Performance with Output Caching (C#)</span></span>
====================
<span data-ttu-id="a9821-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a9821-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a9821-106">このチュートリアルでは、出力キャッシュを活用して、ASP.NET MVC web アプリケーションのパフォーマンスを大幅に向上する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="a9821-106">In this tutorial, you learn how you can dramatically improve the performance of your ASP.NET MVC web applications by taking advantage of output caching.</span></span> <span data-ttu-id="a9821-107">同じコンテンツは、新しいユーザーがアクションを呼び出すたびに作成する必要はありません、コント ローラーのアクションから返される結果をキャッシュする方法について学びます。</span><span class="sxs-lookup"><span data-stu-id="a9821-107">You learn how to cache the result returned from a controller action so that the same content does not need to be created each and every time a new user invokes the action.</span></span>


<span data-ttu-id="a9821-108">このチュートリアルの目的では、出力キャッシュを活用して、ASP.NET MVC アプリケーションのパフォーマンスを大幅に向上する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a9821-108">The goal of this tutorial is to explain how you can dramatically improve the performance of an ASP.NET MVC application by taking advantage of the output cache.</span></span> <span data-ttu-id="a9821-109">出力キャッシュには、コント ローラーのアクションによって返されるコンテンツをキャッシュすることができます。</span><span class="sxs-lookup"><span data-stu-id="a9821-109">The output cache enables you to cache the content returned by a controller action.</span></span> <span data-ttu-id="a9821-110">このように、同じコンテンツは、同じコント ローラー アクションが呼び出されるたびに生成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a9821-110">That way, the same content does not need to be generated each and every time the same controller action is invoked.</span></span>

<span data-ttu-id="a9821-111">たとえば、ASP.NET MVC アプリケーションがインデックスをという名前のビューでデータベース レコードの一覧を表示することに想像してください。</span><span class="sxs-lookup"><span data-stu-id="a9821-111">Imagine, for example, that your ASP.NET MVC application displays a list of database records in a view named Index.</span></span> <span data-ttu-id="a9821-112">通常、ユーザーが、インデックス ビューを返すコント ローラーのアクションを呼び出すたびにデータベース レコードのセットする必要がありますデータベースから取得する、データベース クエリを実行しています。</span><span class="sxs-lookup"><span data-stu-id="a9821-112">Normally, each and every time that a user invokes the controller action that returns the Index view, the set of database records must be retrieved from the database by executing a database query.</span></span>

<span data-ttu-id="a9821-113">その一方で、使用すると、出力キャッシュの場合は、すべてのユーザーが同じコント ローラー アクションを呼び出すたびに、データベース クエリの実行を回避できます。</span><span class="sxs-lookup"><span data-stu-id="a9821-113">If, on the other hand, you take advantage of the output cache then you can avoid executing a database query every time any user invokes the same controller action.</span></span> <span data-ttu-id="a9821-114">ビューは、コント ローラーのアクションから再生成されることがなくキャッシュから取得できます。</span><span class="sxs-lookup"><span data-stu-id="a9821-114">The view can be retrieved from the cache instead of being regenerated from the controller action.</span></span> <span data-ttu-id="a9821-115">サーバー上で動作する冗長な実行を回避するキャッシュを有効します。</span><span class="sxs-lookup"><span data-stu-id="a9821-115">Caching enables you to avoid performing redundant work on the server.</span></span>

## <a name="enabling-output-caching"></a><span data-ttu-id="a9821-116">出力キャッシュの有効化</span><span class="sxs-lookup"><span data-stu-id="a9821-116">Enabling Output Caching</span></span>

<span data-ttu-id="a9821-117">出力は、個々 のコント ローラー アクションまたは全体のコント ローラー クラスに [OutputCache] 属性を追加してキャッシュを有効にします。</span><span class="sxs-lookup"><span data-stu-id="a9821-117">You enable output caching by adding an [OutputCache] attribute to either an individual controller action or an entire controller class.</span></span> <span data-ttu-id="a9821-118">たとえば、リスト 1 のコント ローラーは、Index() をという名前のアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="a9821-118">For example, the controller in Listing 1 exposes an action named Index().</span></span> <span data-ttu-id="a9821-119">Index() アクションの出力は、10 秒間キャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="a9821-119">The output of the Index() action is cached for 10 seconds.</span></span>

<span data-ttu-id="a9821-120">**1 – controllers \homecontroller.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a9821-120">**Listing 1 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

<span data-ttu-id="a9821-121">ASP.NET MVC のベータ版で出力キャッシュが機能しないような URL [http://www.MySite.com/](http://www.mysite.com/)です。</span><span class="sxs-lookup"><span data-stu-id="a9821-121">In the Beta versions of ASP.NET MVC, output caching does not work for a URL like [http://www.MySite.com/](http://www.mysite.com/).</span></span> <span data-ttu-id="a9821-122">代わりに、ような URL を入力する必要があります[http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index)です。</span><span class="sxs-lookup"><span data-stu-id="a9821-122">Instead, you must enter a URL like [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index).</span></span> 

<span data-ttu-id="a9821-123">リストの 1 の Index() アクションの出力が 10 秒間キャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="a9821-123">In Listing 1, the output of the Index() action is cached for 10 seconds.</span></span> <span data-ttu-id="a9821-124">場合は、かなり長くキャッシュの存続期間を指定できます。</span><span class="sxs-lookup"><span data-stu-id="a9821-124">If you prefer, you can specify a much longer cache duration.</span></span> <span data-ttu-id="a9821-125">たとえば、1 日のコント ローラーのアクションの出力をキャッシュする場合を指定できます (86400 秒) のキャッシュの存続期間 (60 秒\*60 分\*24 時間)。</span><span class="sxs-lookup"><span data-stu-id="a9821-125">For example, if you want to cache the output of a controller action for one day then you can specify a cache duration of 86400 seconds (60 seconds \* 60 minutes \* 24 hours).</span></span>

<span data-ttu-id="a9821-126">指定した時間分保証コンテンツはキャッシュされません。</span><span class="sxs-lookup"><span data-stu-id="a9821-126">There is no guarantee that content will be cached for the amount of time that you specify.</span></span> <span data-ttu-id="a9821-127">メモリ リソースが不足になると、キャッシュは削除のコンテンツを自動的に起動します。</span><span class="sxs-lookup"><span data-stu-id="a9821-127">When memory resources become low, the cache starts evicting content automatically.</span></span>

<span data-ttu-id="a9821-128">1 の一覧で、Home コント ローラーは、2 の一覧表示するインデックス ビューを返します。</span><span class="sxs-lookup"><span data-stu-id="a9821-128">The Home controller in Listing 1 returns the Index view in Listing 2.</span></span> <span data-ttu-id="a9821-129">このビューに関する特別なはありません。</span><span class="sxs-lookup"><span data-stu-id="a9821-129">There is nothing special about this view.</span></span> <span data-ttu-id="a9821-130">インデックス ビューには、単に、現在の時刻が表示されます (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a9821-130">The Index view simply displays the current time (see Figure 1).</span></span>

<span data-ttu-id="a9821-131">**2 – Views\Home\Index.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a9821-131">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

<span data-ttu-id="a9821-132">**図 1 – キャッシュのインデックス ビュー**</span><span class="sxs-lookup"><span data-stu-id="a9821-132">**Figure 1 – Cached Index view**</span></span>

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

<span data-ttu-id="a9821-134">お使いのブラウザーのアドレス バーに URL/Home/インデックスを入力し、インデックス ビューによって表示される時間し、繰り返し、お使いのブラウザーの更新/再読み込み ボタンを押すで複数回 Index() アクションを起動する場合は、10 秒間に変更されません。</span><span class="sxs-lookup"><span data-stu-id="a9821-134">If you invoke the Index() action multiple times by entering the URL /Home/Index in the address bar of your browser and hitting the Refresh/Reload button in your browser repeatedly, then the time displayed by the Index view won't change for 10 seconds.</span></span> <span data-ttu-id="a9821-135">ビューがキャッシュされるため、同時が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a9821-135">The same time is displayed because the view is cached.</span></span>

<span data-ttu-id="a9821-136">同じビューが、アプリケーションにアクセスしたすべてのキャッシュされていることを理解しておく必要です。</span><span class="sxs-lookup"><span data-stu-id="a9821-136">It is important to understand that the same view is cached for everyone who visits your application.</span></span> <span data-ttu-id="a9821-137">Index() アクションを起動したすべてのユーザーをインデックス ビューの場合は、同じキャッシュされたバージョンとなります。</span><span class="sxs-lookup"><span data-stu-id="a9821-137">Anyone who invokes the Index() action will get the same cached version of the Index view.</span></span> <span data-ttu-id="a9821-138">これは、インデックス ビューを提供する web サーバーが行う作業の量が大幅に削減することを意味します。</span><span class="sxs-lookup"><span data-stu-id="a9821-138">This means that the amount of work that the web server must perform to serve the Index view is dramatically reduced.</span></span>

<span data-ttu-id="a9821-139">ビューを一覧表示する 2 は、操作を行っている実に簡単に発生します。</span><span class="sxs-lookup"><span data-stu-id="a9821-139">The view in Listing 2 happens to be doing something really simple.</span></span> <span data-ttu-id="a9821-140">ビューには、現在の時刻だけが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a9821-140">The view just displays the current time.</span></span> <span data-ttu-id="a9821-141">ただし、簡単にキャッシュを一連のデータベース レコードを表示するビューと同じようにできます。</span><span class="sxs-lookup"><span data-stu-id="a9821-141">However, you could just as easily cache a view that displays a set of database records.</span></span> <span data-ttu-id="a9821-142">その場合は、データベース レコードのセットでは、ビューを返しますコント ローラーのアクションが呼び出されるたびに、データベースから取得する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a9821-142">In that case, the set of database records would not need to be retrieved from the database each and every time the controller action that returns the view is invoked.</span></span> <span data-ttu-id="a9821-143">キャッシュと、web サーバーおよびデータベース サーバーの両方が実行する必要がある作業の量を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="a9821-143">Caching can reduce the amount of work that both your web server and database server must perform.</span></span>

<span data-ttu-id="a9821-144">ページを使用しない&lt;% @ OutputCache %&gt; MVC ビュー内のディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="a9821-144">Don't use the page &lt;%@ OutputCache %&gt; directive in an MVC view.</span></span> <span data-ttu-id="a9821-145">このディレクティブは、Web フォームの世界から漏れると、ASP.NET MVC アプリケーションでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="a9821-145">This directive is bleeding over from the Web Forms world and should not be used in an ASP.NET MVC application.</span></span>

## <a name="where-content-is-cached"></a><span data-ttu-id="a9821-146">コンテンツがキャッシュされています。</span><span class="sxs-lookup"><span data-stu-id="a9821-146">Where Content is Cached</span></span>

<span data-ttu-id="a9821-147">既定では、[OutputCache] 属性を使用するときにコンテンツがキャッシュに 3 つの場所: web サーバー、すべてのプロキシ サーバーと web ブラウザー。</span><span class="sxs-lookup"><span data-stu-id="a9821-147">By default, when you use the [OutputCache] attribute, content is cached in three locations: the web server, any proxy servers, and the web browser.</span></span> <span data-ttu-id="a9821-148">[OutputCache] 属性の場所のプロパティを変更することによってコンテンツがキャッシュされているに正確に制御できます。</span><span class="sxs-lookup"><span data-stu-id="a9821-148">You can control exactly where content is cached by modifying the Location property of the [OutputCache] attribute.</span></span>

<span data-ttu-id="a9821-149">次の値のいずれかに、Location プロパティを設定できます。</span><span class="sxs-lookup"><span data-stu-id="a9821-149">You can set the Location property to any one of the following values:</span></span>

> <span data-ttu-id="a9821-150">·任意</span><span class="sxs-lookup"><span data-stu-id="a9821-150">· Any</span></span>
> 
> <span data-ttu-id="a9821-151">·クライアント</span><span class="sxs-lookup"><span data-stu-id="a9821-151">· Client</span></span>
> 
> <span data-ttu-id="a9821-152">·ダウン ストリーム</span><span class="sxs-lookup"><span data-stu-id="a9821-152">· Downstream</span></span>
> 
> <span data-ttu-id="a9821-153">·サーバー</span><span class="sxs-lookup"><span data-stu-id="a9821-153">· Server</span></span>
> 
> <span data-ttu-id="a9821-154">·[なし]</span><span class="sxs-lookup"><span data-stu-id="a9821-154">· None</span></span>
> 
> <span data-ttu-id="a9821-155">·ServerAndClient</span><span class="sxs-lookup"><span data-stu-id="a9821-155">· ServerAndClient</span></span>


<span data-ttu-id="a9821-156">既定では、Location プロパティに値 Any です。</span><span class="sxs-lookup"><span data-stu-id="a9821-156">By default, the Location property has the value Any.</span></span> <span data-ttu-id="a9821-157">ただし、ブラウザーでのみ、またはサーバー上でのみキャッシュにすることがありますもあります。</span><span class="sxs-lookup"><span data-stu-id="a9821-157">However, there are situations in which you might want to cache only on the browser or only on the server.</span></span> <span data-ttu-id="a9821-158">たとえば、ユーザーごとに個人用に設定された情報をキャッシュする場合は必要がありますいないサーバー上の情報をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="a9821-158">For example, if you are caching information that is personalized for each user then you should not cache the information on the server.</span></span> <span data-ttu-id="a9821-159">ユーザーごとに異なる情報を表示する場合は、クライアント上にのみ情報をキャッシュする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9821-159">If you are displaying different information to different users then you should cache the information only on the client.</span></span>

<span data-ttu-id="a9821-160">たとえば、リスト 3 でのコント ローラーを現在のユーザー名を返す GetName() をという名前のアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="a9821-160">For example, the controller in Listing 3 exposes an action named GetName() that returns the current user name.</span></span> <span data-ttu-id="a9821-161">回線のモジュラー ジャックが、web サイトにログインすると、GetName() アクションを呼び出しますアクションの文字列を返します「やあジャック」です。</span><span class="sxs-lookup"><span data-stu-id="a9821-161">If Jack logs into the website and invokes the GetName() action then the action returns the string "Hi Jack".</span></span> <span data-ttu-id="a9821-162">後で、Jill は、web サイトにログインし、GetName() アクションが呼び出されます場合、し、彼女はもは文字列を取得「やあジャック」。</span><span class="sxs-lookup"><span data-stu-id="a9821-162">If, subsequently, Jill logs into the website and invokes the GetName() action then she also will get the string "Hi Jack".</span></span> <span data-ttu-id="a9821-163">文字列は、回線のモジュラー ジャックが最初にコント ローラーのアクションを起動した後、すべてのユーザー用の web サーバーにキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="a9821-163">The string is cached on the web server for all users after Jack initially invokes the controller action.</span></span>

<span data-ttu-id="a9821-164">**3 – Controllers\BadUserController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a9821-164">**Listing 3 – Controllers\BadUserController.cs**</span></span>

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

<span data-ttu-id="a9821-165">ほとんどの場合、リスト 3 でのコント ローラーでは、使用する方法は機能しません。</span><span class="sxs-lookup"><span data-stu-id="a9821-165">Most likely, the controller in Listing 3 does not work the way that you want.</span></span> <span data-ttu-id="a9821-166">Jill に「やあジャック」メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="a9821-166">You don't want to display the message "Hi Jack" to Jill.</span></span>

<span data-ttu-id="a9821-167">サーバーのキャッシュ内の個人用に設定されたコンテンツをキャッシュすることはありません。</span><span class="sxs-lookup"><span data-stu-id="a9821-167">You should never cache personalized content in the server cache.</span></span> <span data-ttu-id="a9821-168">ただし、パフォーマンスを向上させるためにブラウザー キャッシュ内の個人用に設定されたコンテンツをキャッシュすることができます。</span><span class="sxs-lookup"><span data-stu-id="a9821-168">However, you might want to cache the personalized content in the browser cache to improve performance.</span></span> <span data-ttu-id="a9821-169">ブラウザーで、コンテンツをキャッシュする、ユーザーは、同じコント ローラー アクションを複数回呼び出す場合は、コンテンツは、サーバーではなく、ブラウザーのキャッシュから取得できます。</span><span class="sxs-lookup"><span data-stu-id="a9821-169">If you cache content in the browser, and a user invokes the same controller action multiple times, then the content can be retrieved from the browser cache instead of the server.</span></span>

<span data-ttu-id="a9821-170">リスト 4 に変更されたコント ローラーは、GetName() アクションの出力をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="a9821-170">The modified controller in Listing 4 caches the output of the GetName() action.</span></span> <span data-ttu-id="a9821-171">ただし、ブラウザーでのみ、サーバーではなく、コンテンツがキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="a9821-171">However, the content is cached only on the browser and not on the server.</span></span> <span data-ttu-id="a9821-172">このように、複数のユーザーが GetName() メソッドを呼び出すときは、各ユーザーは、ユーザー名と他のユーザーのユーザー名を取得します。</span><span class="sxs-lookup"><span data-stu-id="a9821-172">That way, when multiple users invoke the GetName() method, each person gets their own user name and not another person's user name.</span></span>

<span data-ttu-id="a9821-173">**4 – Controllers\UserController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a9821-173">**Listing 4 – Controllers\UserController.cs**</span></span>

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

<span data-ttu-id="a9821-174">4 の一覧で [OutputCache] 属性が OutputCacheLocation.Client 値に設定された場所プロパティが含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a9821-174">Notice that the [OutputCache] attribute in Listing 4 includes a Location property set to the value OutputCacheLocation.Client.</span></span> <span data-ttu-id="a9821-175">[OutputCache] 属性には、NoStore プロパティも含まれています。</span><span class="sxs-lookup"><span data-stu-id="a9821-175">The [OutputCache] attribute also includes a NoStore property.</span></span> <span data-ttu-id="a9821-176">NoStore プロパティは、キャッシュされたコンテンツの永続的なコピーを格納しないようにすることのプロキシ サーバーとブラウザーを通知するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a9821-176">The NoStore property is used to inform proxy servers and browser that they should not store a permanent copy of the cached content.</span></span>

## <a name="varying-the-output-cache"></a><span data-ttu-id="a9821-177">出力キャッシュの変更</span><span class="sxs-lookup"><span data-stu-id="a9821-177">Varying the Output Cache</span></span>

<span data-ttu-id="a9821-178">状況によっては、非常に同じコンテンツの別のキャッシュされたバージョンを必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9821-178">In some situations, you might want different cached versions of the very same content.</span></span> <span data-ttu-id="a9821-179">たとえば、マスター/詳細ページを作成することに想像してください。</span><span class="sxs-lookup"><span data-stu-id="a9821-179">Imagine, for example, that you are creating a master/detail page.</span></span> <span data-ttu-id="a9821-180">マスター ページには、ムービーのタイトルの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a9821-180">The master page displays a list of movie titles.</span></span> <span data-ttu-id="a9821-181">タイトルをクリックすると、選択したムービーの詳細を取得します。</span><span class="sxs-lookup"><span data-stu-id="a9821-181">When you click a title, you get details for the selected movie.</span></span>

<span data-ttu-id="a9821-182">詳細ページをキャッシュする場合、同じ映画の詳細が表示 ムービーに関係なく をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a9821-182">If you cache the details page, then the details for the same movie will be displayed no matter which movie you click.</span></span> <span data-ttu-id="a9821-183">最初のユーザーが選択した最初のムービーが将来のすべてのユーザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="a9821-183">The first movie selected by the first user will be displayed to all future users.</span></span>

<span data-ttu-id="a9821-184">この問題を修正するには、[OutputCache] 属性の VaryByParam プロパティを活用します。</span><span class="sxs-lookup"><span data-stu-id="a9821-184">You can fix this problem by taking advantage of the VaryByParam property of the [OutputCache] attribute.</span></span> <span data-ttu-id="a9821-185">このプロパティでは、フォームのパラメーターのときに非常に同じコンテンツの別のキャッシュされたバージョンを作成できます。 または、クエリ文字列パラメーターが異なります。</span><span class="sxs-lookup"><span data-stu-id="a9821-185">This property enables you to create different cached versions of the very same content when a form parameter or query string parameter varies.</span></span>

<span data-ttu-id="a9821-186">たとえば、5 の一覧で、コント ローラーは、Master() および Details() という名前の 2 つのアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="a9821-186">For example, the controller in Listing 5 exposes two actions named Master() and Details().</span></span> <span data-ttu-id="a9821-187">Master() アクションはムービーのタイトルの一覧を返し、Details() アクションは、選択したムービーの詳細を返します。</span><span class="sxs-lookup"><span data-stu-id="a9821-187">The Master() action returns a list of movie titles and the Details() action returns the details for the selected movie.</span></span>

<span data-ttu-id="a9821-188">**5 – Controllers\MoviesController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a9821-188">**Listing 5 – Controllers\MoviesController.cs**</span></span>

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

<span data-ttu-id="a9821-189">Master() アクションには、値を持つ VaryByParam プロパティが含まれています。"none"です。</span><span class="sxs-lookup"><span data-stu-id="a9821-189">The Master() action includes a VaryByParam property with the value "none".</span></span> <span data-ttu-id="a9821-190">アクションが呼び出されると、Master() マスターの場合は、同じキャッシュされたバージョンの表示とが返されます。</span><span class="sxs-lookup"><span data-stu-id="a9821-190">When the Master() action is invoked, the same cached version of the Master view is returned.</span></span> <span data-ttu-id="a9821-191">パラメーターは、フォームのパラメーターまたはクエリ文字列には、(図 2 を参照) が無視されます。</span><span class="sxs-lookup"><span data-stu-id="a9821-191">Any form parameters or query string parameters are ignored (see Figure 2).</span></span>

<span data-ttu-id="a9821-192">**図 2 –/Movies/Master ビュー**</span><span class="sxs-lookup"><span data-stu-id="a9821-192">**Figure 2 – The /Movies/Master view**</span></span>

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

<span data-ttu-id="a9821-194">**図 3 –/ビデオ/詳細ビュー**</span><span class="sxs-lookup"><span data-stu-id="a9821-194">**Figure 3 – The /Movies/Details view**</span></span>

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

<span data-ttu-id="a9821-196">Details() アクションには、"Id"の値を持つ VaryByParam プロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a9821-196">The Details() action includes a VaryByParam property with the value "Id".</span></span> <span data-ttu-id="a9821-197">Id パラメーターの異なる値は、コント ローラーのアクションに渡され、詳細ビューの別のキャッシュされたバージョンが生成されます。</span><span class="sxs-lookup"><span data-stu-id="a9821-197">When different values of the Id parameter are passed to the controller action, different cached versions of the Details view are generated.</span></span>

<span data-ttu-id="a9821-198">詳細キャッシュでは、VaryByParam プロパティ結果を使用することを理解して重要でない小さいをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a9821-198">It is important to understand that using the VaryByParam property results in more caching and not less.</span></span> <span data-ttu-id="a9821-199">別のキャッシュされたバージョンの詳細ビューの Id パラメーターの各バージョンが作成されます。</span><span class="sxs-lookup"><span data-stu-id="a9821-199">A different cached version of the Details view is created for each different version of the Id parameter.</span></span>

<span data-ttu-id="a9821-200">VaryByParam プロパティは、次の値を設定することができます。</span><span class="sxs-lookup"><span data-stu-id="a9821-200">You can set the VaryByParam property to the following values:</span></span>

> <span data-ttu-id="a9821-201">\*= フォームまたはクエリ文字列パラメーターが変化するたびに、別のキャッシュされたバージョンを作成します。</span><span class="sxs-lookup"><span data-stu-id="a9821-201">\* = Create a different cached version whenever a form or query string parameter varies.</span></span>
> 
> <span data-ttu-id="a9821-202">none = しない別のキャッシュされたバージョンを作成します。</span><span class="sxs-lookup"><span data-stu-id="a9821-202">none = Never create different cached versions</span></span>
> 
> <span data-ttu-id="a9821-203">セミコロンのパラメーター リスト内のフォームまたはクエリ文字列パラメーターのいずれかによって異なりますされるたびに作成のさまざまなキャッシュされたバージョンを =</span><span class="sxs-lookup"><span data-stu-id="a9821-203">Semicolon list of parameters = Create different cached versions whenever any of the form or query string parameters in the list varies</span></span>


## <a name="creating-a-cache-profile"></a><span data-ttu-id="a9821-204">キャッシュ プロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="a9821-204">Creating a Cache Profile</span></span>

<span data-ttu-id="a9821-205">[OutputCache] 属性のプロパティを変更することで出力キャッシュのプロパティを構成する代わりに、web の構成 (web.config) ファイルでキャッシュ プロファイルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="a9821-205">As an alternative to configuring output cache properties by modifying properties of the [OutputCache] attribute, you can create a cache profile in the web configuration (web.config) file.</span></span> <span data-ttu-id="a9821-206">Web 構成ファイルでキャッシュ プロファイルを作成するには、重要な利点のいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="a9821-206">Creating a cache profile in the web configuration file offers a couple of important advantages.</span></span>

<span data-ttu-id="a9821-207">最初に、web 構成ファイルで出力キャッシュを構成すると、コント ローラーのアクションが 1 つの場所にコンテンツをキャッシュする方法を制御できます。</span><span class="sxs-lookup"><span data-stu-id="a9821-207">First, by configuring output caching in the web configuration file, you can control how controller actions cache content in one central location.</span></span> <span data-ttu-id="a9821-208">1 つのキャッシュ プロファイルを作成し、いくつかのコント ローラーまたはコント ローラーのアクションにプロファイルを適用できます。</span><span class="sxs-lookup"><span data-stu-id="a9821-208">You can create one cache profile and apply the profile to several controllers or controller actions.</span></span>

<span data-ttu-id="a9821-209">次に、アプリケーションを再コンパイルしなくても web 構成ファイルを変更できます。</span><span class="sxs-lookup"><span data-stu-id="a9821-209">Second, you can modify the web configuration file without recompiling your application.</span></span> <span data-ttu-id="a9821-210">実稼働環境に既に配置されているアプリケーションのキャッシュを無効にする必要がある場合、web 構成ファイルで定義されているキャッシュ プロファイルだけで変更できます。</span><span class="sxs-lookup"><span data-stu-id="a9821-210">If you need to disable caching for an application that has already been deployed to production, then you can simply modify the cache profiles defined in the web configuration file.</span></span> <span data-ttu-id="a9821-211">Web 構成ファイルへの変更が自動的に検出され、適用します。</span><span class="sxs-lookup"><span data-stu-id="a9821-211">Any changes to the web configuration file will be detected automatically and applied.</span></span>

<span data-ttu-id="a9821-212">たとえば、&lt;キャッシュ&gt;リスト 6 での web 構成セクションを Cache1Hour をという名前のキャッシュ プロファイルを定義します。</span><span class="sxs-lookup"><span data-stu-id="a9821-212">For example, the &lt;caching&gt; web configuration section in Listing 6 defines a cache profile named Cache1Hour.</span></span> <span data-ttu-id="a9821-213">&lt;キャッシュ&gt;内のセクションに表示されます、 &lt;system.web&gt; web 構成ファイルのセクションです。</span><span class="sxs-lookup"><span data-stu-id="a9821-213">The &lt;caching&gt; section must appear within the &lt;system.web&gt; section of a web configuration file.</span></span>

<span data-ttu-id="a9821-214">**6 – web.config のキャッシュ セクションを一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a9821-214">**Listing 6 – Caching section for web.config**</span></span>

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

<span data-ttu-id="a9821-215">一覧表示する 7 でのコント ローラーは、[OutputCache] 属性でコント ローラーのアクションに Cache1Hour プロファイルを適用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="a9821-215">The controller in Listing 7 illustrates how you can apply the Cache1Hour profile to a controller action with the [OutputCache] attribute.</span></span>

<span data-ttu-id="a9821-216">**7 – Controllers\ProfileController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a9821-216">**Listing 7 – Controllers\ProfileController.cs**</span></span>

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

<span data-ttu-id="a9821-217">リスト 7 でのコント ローラーによって公開される Index() アクションを呼び出す場合、同時にが 1 時間に返されます。</span><span class="sxs-lookup"><span data-stu-id="a9821-217">If you invoke the Index() action exposed by the controller in Listing 7 then the same time will be returned for 1 hour.</span></span>

## <a name="summary"></a><span data-ttu-id="a9821-218">概要</span><span class="sxs-lookup"><span data-stu-id="a9821-218">Summary</span></span>

<span data-ttu-id="a9821-219">出力キャッシュ、ASP.NET MVC アプリケーションのパフォーマンスを大幅に向上させるの非常に簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="a9821-219">Output caching provides you with a very easy method of dramatically improving the performance of your ASP.NET MVC applications.</span></span> <span data-ttu-id="a9821-220">このチュートリアルでは、[OutputCache] 属性を使用して、コント ローラー アクションの出力をキャッシュする方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="a9821-220">In this tutorial, you learned how to use the [OutputCache] attribute to cache the output of controller actions.</span></span> <span data-ttu-id="a9821-221">また、コンテンツがキャッシュを取得する方法を変更する期間と VaryByParam プロパティなど、その [OutputCache] 属性のプロパティを変更する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="a9821-221">You also learned how to modify properties of the [OutputCache] attribute such as the Duration and VaryByParam properties to modify how content gets cached.</span></span> <span data-ttu-id="a9821-222">最後に、web 構成ファイルでキャッシュ プロファイルを定義する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="a9821-222">Finally, you learned how to define cache profiles in the web configuration file.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a9821-223">[前へ](understanding-action-filters-cs.md)
[次へ](adding-dynamic-content-to-a-cached-page-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a9821-223">[Previous](understanding-action-filters-cs.md)
[Next](adding-dynamic-content-to-a-cached-page-cs.md)</span></span>
