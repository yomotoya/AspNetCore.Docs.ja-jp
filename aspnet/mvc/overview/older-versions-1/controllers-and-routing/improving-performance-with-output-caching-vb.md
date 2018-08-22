---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
title: キャッシュ (VB) を出力によるパフォーマンスの向上 |Microsoft Docs
author: microsoft
description: このチュートリアルでは、出力キャッシュを活用して、ASP.NET MVC web アプリケーションのパフォーマンスを大幅に向上する方法を説明します。 あなたが。。。
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 0e7b4d85-2c46-4eaf-b6a8-6cd566a67334
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
msc.type: authoredcontent
ms.openlocfilehash: 9402d7e053ef11eeefa92d112b05ec255d5ec6f7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823966"
---
<a name="improving-performance-with-output-caching-vb"></a><span data-ttu-id="659c7-104">出力 (VB) のキャッシュによるパフォーマンスの向上</span><span class="sxs-lookup"><span data-stu-id="659c7-104">Improving Performance with Output Caching (VB)</span></span>
====================
<span data-ttu-id="659c7-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="659c7-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="659c7-106">このチュートリアルでは、出力キャッシュを活用して、ASP.NET MVC web アプリケーションのパフォーマンスを大幅に向上する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="659c7-106">In this tutorial, you learn how you can dramatically improve the performance of your ASP.NET MVC web applications by taking advantage of output caching.</span></span> <span data-ttu-id="659c7-107">同じコンテンツは、新しいユーザーがアクションを呼び出すたびに作成する必要はありません、コント ローラー アクションから返される結果をキャッシュする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="659c7-107">You learn how to cache the result returned from a controller action so that the same content does not need to be created each and every time a new user invokes the action.</span></span>


<span data-ttu-id="659c7-108">このチュートリアルの目的では、出力キャッシュを活用して、ASP.NET MVC アプリケーションのパフォーマンスを大幅に向上する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="659c7-108">The goal of this tutorial is to explain how you can dramatically improve the performance of an ASP.NET MVC application by taking advantage of the output cache.</span></span> <span data-ttu-id="659c7-109">出力キャッシュでは、コント ローラーのアクションによって返されるコンテンツをキャッシュすることができます。</span><span class="sxs-lookup"><span data-stu-id="659c7-109">The output cache enables you to cache the content returned by a controller action.</span></span> <span data-ttu-id="659c7-110">これにより、同じコンテンツは同じコント ローラー アクションが呼び出されるたびに生成する必要ありません。</span><span class="sxs-lookup"><span data-stu-id="659c7-110">That way, the same content does not need to be generated each and every time the same controller action is invoked.</span></span>

<span data-ttu-id="659c7-111">たとえば、ASP.NET MVC アプリケーションがインデックスをという名前のビューでデータベースのレコードの一覧を表示することに想像してください。</span><span class="sxs-lookup"><span data-stu-id="659c7-111">Imagine, for example, that your ASP.NET MVC application displays a list of database records in a view named Index.</span></span> <span data-ttu-id="659c7-112">通常、ユーザーは、インデックス ビューを返すコント ローラー アクションを呼び出すたびにデータベースのレコード セットする必要がありますデータベースから取得、データベース クエリを実行しています。</span><span class="sxs-lookup"><span data-stu-id="659c7-112">Normally, each and every time that a user invokes the controller action that returns the Index view, the set of database records must be retrieved from the database by executing a database query.</span></span>

<span data-ttu-id="659c7-113">場合を出力キャッシュを利用する一方では、すべてのユーザーが同じコント ローラー アクションを呼び出すたびに、データベース クエリの実行を回避できます。</span><span class="sxs-lookup"><span data-stu-id="659c7-113">If, on the other hand, you take advantage of the output cache then you can avoid executing a database query every time any user invokes the same controller action.</span></span> <span data-ttu-id="659c7-114">ビューは、コント ローラー アクションから再生成されることがなくキャッシュから取得できます。</span><span class="sxs-lookup"><span data-stu-id="659c7-114">The view can be retrieved from the cache instead of being regenerated from the controller action.</span></span> <span data-ttu-id="659c7-115">サーバー上で動作する冗長な実行を回避するキャッシュ有効。</span><span class="sxs-lookup"><span data-stu-id="659c7-115">Caching enables you to avoid performing redundant work on the server.</span></span>

#### <a name="enabling-output-caching"></a><span data-ttu-id="659c7-116">出力キャッシュを有効にします。</span><span class="sxs-lookup"><span data-stu-id="659c7-116">Enabling Output Caching</span></span>

<span data-ttu-id="659c7-117">追加して、出力キャッシュを有効にすると、 &lt;OutputCache&gt;属性の個々 のコント ローラー アクションまたはコント ローラー全体クラスのいずれかにします。</span><span class="sxs-lookup"><span data-stu-id="659c7-117">You enable output caching by adding an &lt;OutputCache&gt; attribute to either an individual controller action or an entire controller class.</span></span> <span data-ttu-id="659c7-118">たとえば、リスト 1 で、コント ローラーは、Index() という名前のアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="659c7-118">For example, the controller in Listing 1 exposes an action named Index().</span></span> <span data-ttu-id="659c7-119">Index() アクションの出力は、10 秒間キャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="659c7-119">The output of the Index() action is cached for 10 seconds.</span></span>

<span data-ttu-id="659c7-120">**1 – Controllers\HomeController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="659c7-120">**Listing 1 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample1.vb)]


<span data-ttu-id="659c7-121">ASP.NET MVC のベータ バージョンでは、出力キャッシュは機能しませんように URL [ http://www.MySite.com/](http://www.mysite.com/)します。</span><span class="sxs-lookup"><span data-stu-id="659c7-121">In the Beta versions of ASP.NET MVC, output caching does not work for a URL like [http://www.MySite.com/](http://www.mysite.com/).</span></span> <span data-ttu-id="659c7-122">代わりに、ような URL を入力する必要があります[ http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index)します。</span><span class="sxs-lookup"><span data-stu-id="659c7-122">Instead, you must enter a URL like [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index).</span></span>


<span data-ttu-id="659c7-123">リストの 1 の Index() アクションの出力は 10 秒間キャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="659c7-123">In Listing 1, the output of the Index() action is cached for 10 seconds.</span></span> <span data-ttu-id="659c7-124">場合は、大幅に長くキャッシュ期間を指定できます。</span><span class="sxs-lookup"><span data-stu-id="659c7-124">If you prefer, you can specify a much longer cache duration.</span></span> <span data-ttu-id="659c7-125">たとえば、1 日のコント ローラー アクションの出力をキャッシュする場合できます指定する必要が 86400 秒のキャッシュ有効期間 (60 秒\*60 分\*24 時間)。</span><span class="sxs-lookup"><span data-stu-id="659c7-125">For example, if you want to cache the output of a controller action for one day then you can specify a cache duration of 86400 seconds (60 seconds \* 60 minutes \* 24 hours).</span></span>

<span data-ttu-id="659c7-126">指定した時間にわたって保証がそのコンテンツはキャッシュされません。</span><span class="sxs-lookup"><span data-stu-id="659c7-126">There is no guarantee that content will be cached for the amount of time that you specify.</span></span> <span data-ttu-id="659c7-127">メモリ リソースが不足になると、キャッシュは削除のコンテンツを自動的に開始します。</span><span class="sxs-lookup"><span data-stu-id="659c7-127">When memory resources become low, the cache starts evicting content automatically.</span></span>

<span data-ttu-id="659c7-128">リスト 1 で、Home コント ローラーは、リスト 2 で、インデックス ビューを返します。</span><span class="sxs-lookup"><span data-stu-id="659c7-128">The Home controller in Listing 1 returns the Index view in Listing 2.</span></span> <span data-ttu-id="659c7-129">このビューに関する特別なものがあります。</span><span class="sxs-lookup"><span data-stu-id="659c7-129">There is nothing special about this view.</span></span> <span data-ttu-id="659c7-130">インデックス ビューには、現在の時刻だけが表示されます (図 1 参照)。</span><span class="sxs-lookup"><span data-stu-id="659c7-130">The Index view simply displays the current time (see Figure 1).</span></span>

<span data-ttu-id="659c7-131">**2 – Views\Home\Index.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="659c7-131">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](improving-performance-with-output-caching-vb/samples/sample2.aspx)]

<span data-ttu-id="659c7-132">**図 1 – キャッシュのインデックス ビュー**</span><span class="sxs-lookup"><span data-stu-id="659c7-132">**Figure 1 – Cached Index view**</span></span>

![clip_image002](improving-performance-with-output-caching-vb/_static/image1.jpg)

<span data-ttu-id="659c7-134">お使いのブラウザーのアドレス バーに URL/Home/インデックスを入力し、時間インデックス ビューに表示し、繰り返し、お使いのブラウザーの更新/再読み込み ボタンを押すを複数回 Index() アクションを起動する場合は、10 秒間は変更されません。</span><span class="sxs-lookup"><span data-stu-id="659c7-134">If you invoke the Index() action multiple times by entering the URL /Home/Index in the address bar of your browser and hitting the Refresh/Reload button in your browser repeatedly, then the time displayed by the Index view won't change for 10 seconds.</span></span> <span data-ttu-id="659c7-135">ビューがキャッシュされるため、同じ時間が表示されます。</span><span class="sxs-lookup"><span data-stu-id="659c7-135">The same time is displayed because the view is cached.</span></span>

<span data-ttu-id="659c7-136">アプリケーションにアクセスするすべてのユーザーに対して同じビューがキャッシュされていることを理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="659c7-136">It is important to understand that the same view is cached for everyone who visits your application.</span></span> <span data-ttu-id="659c7-137">Index() アクションを起動するすべてのユーザーにより、インデックス ビューの場合は、同じキャッシュされたバージョンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="659c7-137">Anyone who invokes the Index() action will get the same cached version of the Index view.</span></span> <span data-ttu-id="659c7-138">つまり、インデックス ビューを提供する web サーバーを実行する必要がある作業量が大幅に減ります。</span><span class="sxs-lookup"><span data-stu-id="659c7-138">This means that the amount of work that the web server must perform to serve the Index view is dramatically reduced.</span></span>

<span data-ttu-id="659c7-139">リスト 2 で、ビューは、非常に単純な処理を実行するのには発生します。</span><span class="sxs-lookup"><span data-stu-id="659c7-139">The view in Listing 2 happens to be doing something really simple.</span></span> <span data-ttu-id="659c7-140">ビューには、現在の時刻のみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="659c7-140">The view just displays the current time.</span></span> <span data-ttu-id="659c7-141">ただし、同様の一連のデータベース レコードを表示するビューを簡単にキャッシュできます。</span><span class="sxs-lookup"><span data-stu-id="659c7-141">However, you could just as easily cache a view that displays a set of database records.</span></span> <span data-ttu-id="659c7-142">その場合は、データベース レコードのセットでは、ビューを返すコント ローラー アクションが呼び出されるたびに、データベースから取得する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="659c7-142">In that case, the set of database records would not need to be retrieved from the database each and every time the controller action that returns the view is invoked.</span></span> <span data-ttu-id="659c7-143">キャッシュと、web サーバーとデータベース サーバーの両方を実行する必要がある作業の量を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="659c7-143">Caching can reduce the amount of work that both your web server and database server must perform.</span></span>

<span data-ttu-id="659c7-144">ページを使用しない&lt;% @ OutputCache %&gt; MVC ビューでディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="659c7-144">Don't use the page &lt;%@ OutputCache %&gt; directive in an MVC view.</span></span> <span data-ttu-id="659c7-145">このディレクティブは、Web フォームの世界から漏れると、ASP.NET MVC アプリケーションでは使用しない必要があります。</span><span class="sxs-lookup"><span data-stu-id="659c7-145">This directive is bleeding over from the Web Forms world and should not be used in an ASP.NET MVC application.</span></span> 

#### <a name="where-content-is-cached"></a><span data-ttu-id="659c7-146">コンテンツがキャッシュされています。</span><span class="sxs-lookup"><span data-stu-id="659c7-146">Where Content is Cached</span></span>

<span data-ttu-id="659c7-147">既定で使用すると、 &lt;OutputCache&gt;属性、コンテンツが 3 つの場所にキャッシュされている: web サーバー、すべてのプロキシ サーバーと web ブラウザー。</span><span class="sxs-lookup"><span data-stu-id="659c7-147">By default, when you use the &lt;OutputCache&gt; attribute, content is cached in three locations: the web server, any proxy servers, and the web browser.</span></span> <span data-ttu-id="659c7-148">Location プロパティを変更することによってコンテンツがキャッシュされているに正確に制御することができます、 &lt;OutputCache&gt;属性。</span><span class="sxs-lookup"><span data-stu-id="659c7-148">You can control exactly where content is cached by modifying the Location property of the &lt;OutputCache&gt; attribute.</span></span>

<span data-ttu-id="659c7-149">Location プロパティは、次の値のいずれかに設定できます。</span><span class="sxs-lookup"><span data-stu-id="659c7-149">You can set the Location property to any one of the following values:</span></span>

> <span data-ttu-id="659c7-150">·任意</span><span class="sxs-lookup"><span data-stu-id="659c7-150">· Any</span></span>
> 
> <span data-ttu-id="659c7-151">·クライアント</span><span class="sxs-lookup"><span data-stu-id="659c7-151">· Client</span></span>
> 
> <span data-ttu-id="659c7-152">·ダウン ストリーム</span><span class="sxs-lookup"><span data-stu-id="659c7-152">· Downstream</span></span>
> 
> <span data-ttu-id="659c7-153">·サーバー</span><span class="sxs-lookup"><span data-stu-id="659c7-153">· Server</span></span>
> 
> <span data-ttu-id="659c7-154">·[なし]</span><span class="sxs-lookup"><span data-stu-id="659c7-154">· None</span></span>
> 
> <span data-ttu-id="659c7-155">·ServerAndClient</span><span class="sxs-lookup"><span data-stu-id="659c7-155">· ServerAndClient</span></span>


<span data-ttu-id="659c7-156">既定では、Location プロパティ値を持ちます。</span><span class="sxs-lookup"><span data-stu-id="659c7-156">By default, the Location property has the value Any.</span></span> <span data-ttu-id="659c7-157">ただし、キャッシュ、ブラウザーでのみ、またはサーバー上でのみにすることがありますもあります。</span><span class="sxs-lookup"><span data-stu-id="659c7-157">However, there are situations in which you might want to cache only on the browser or only on the server.</span></span> <span data-ttu-id="659c7-158">たとえば、ユーザーごとに合わせてカスタマイズされた情報をキャッシュする場合は必要がありますいないサーバー上の情報をキャッシュしました。</span><span class="sxs-lookup"><span data-stu-id="659c7-158">For example, if you are caching information that is personalized for each user then you should not cache the information on the server.</span></span> <span data-ttu-id="659c7-159">別のユーザーに異なる情報を表示する場合は、クライアント上にのみ情報をキャッシュする必要があります。</span><span class="sxs-lookup"><span data-stu-id="659c7-159">If you are displaying different information to different users then you should cache the information only on the client.</span></span>

<span data-ttu-id="659c7-160">たとえば、リスト 3 のコント ローラーを現在のユーザー名を返す GetName() という名前のアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="659c7-160">For example, the controller in Listing 3 exposes an action named GetName() that returns the current user name.</span></span> <span data-ttu-id="659c7-161">ジャック、web サイトにログインし、GetName() アクションを呼び出す場合、アクションの文字列を返します"やあ Jack"。</span><span class="sxs-lookup"><span data-stu-id="659c7-161">If Jack logs into the website and invokes the GetName() action then the action returns the string "Hi Jack".</span></span> <span data-ttu-id="659c7-162">その後、Jill は、web サイトにログインし、GetName() アクションを呼び出します場合、して彼女も表示されます"やあ Jack"の文字列。</span><span class="sxs-lookup"><span data-stu-id="659c7-162">If, subsequently, Jill logs into the website and invokes the GetName() action then she also will get the string "Hi Jack".</span></span> <span data-ttu-id="659c7-163">文字列は、回線のモジュラー ジャックが最初にコント ローラー アクションを起動した後、すべてのユーザーの web サーバーでキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="659c7-163">The string is cached on the web server for all users after Jack initially invokes the controller action.</span></span>

<span data-ttu-id="659c7-164">**3 – Controllers\BadUserController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="659c7-164">**Listing 3 – Controllers\BadUserController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample3.vb)]

<span data-ttu-id="659c7-165">ほとんどの場合、リスト 3 のコント ローラーでは、使用する方法は機能しません。</span><span class="sxs-lookup"><span data-stu-id="659c7-165">Most likely, the controller in Listing 3 does not work the way that you want.</span></span> <span data-ttu-id="659c7-166">Jill に"やあ Jack"のメッセージを表示しません。</span><span class="sxs-lookup"><span data-stu-id="659c7-166">You don't want to display the message "Hi Jack" to Jill.</span></span>

<span data-ttu-id="659c7-167">サーバー キャッシュのパーソナライズされたコンテンツをキャッシュしない必要があります。</span><span class="sxs-lookup"><span data-stu-id="659c7-167">You should never cache personalized content in the server cache.</span></span> <span data-ttu-id="659c7-168">ただし、パフォーマンスを向上させるために、ブラウザーのキャッシュでパーソナライズされたコンテンツをキャッシュする場合があります。</span><span class="sxs-lookup"><span data-stu-id="659c7-168">However, you might want to cache the personalized content in the browser cache to improve performance.</span></span> <span data-ttu-id="659c7-169">ブラウザーでコンテンツをキャッシュすると、ユーザーが同じコント ローラー アクションを複数回起動、コンテンツは、サーバーではなく、ブラウザーのキャッシュから取得できます。</span><span class="sxs-lookup"><span data-stu-id="659c7-169">If you cache content in the browser, and a user invokes the same controller action multiple times, then the content can be retrieved from the browser cache instead of the server.</span></span>

<span data-ttu-id="659c7-170">リスト 4 変更後のコント ローラーは、GetName() アクションの出力をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="659c7-170">The modified controller in Listing 4 caches the output of the GetName() action.</span></span> <span data-ttu-id="659c7-171">ただし、ブラウザーでのみ、サーバーではなく、コンテンツがキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="659c7-171">However, the content is cached only on the browser and not on the server.</span></span> <span data-ttu-id="659c7-172">これにより、複数のユーザーが GetName() メソッドを呼び出すときに、各ユーザーは、自分のユーザー名と他のユーザーのユーザー名を取得します。</span><span class="sxs-lookup"><span data-stu-id="659c7-172">That way, when multiple users invoke the GetName() method, each person gets their own user name and not another person's user name.</span></span>

<span data-ttu-id="659c7-173">**4 – Controllers\UserController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="659c7-173">**Listing 4 – Controllers\UserController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample4.vb)]

<span data-ttu-id="659c7-174">なお、 &lt;OutputCache&gt;属性リスト 4 OutputCacheLocation.Client 値に設定された場所プロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="659c7-174">Notice that the &lt;OutputCache&gt; attribute in Listing 4 includes a Location property set to the value OutputCacheLocation.Client.</span></span> <span data-ttu-id="659c7-175">&lt;OutputCache&gt;属性には、NoStore プロパティも含まれています。</span><span class="sxs-lookup"><span data-stu-id="659c7-175">The &lt;OutputCache&gt; attribute also includes a NoStore property.</span></span> <span data-ttu-id="659c7-176">NoStore プロパティは、キャッシュされたコンテンツの永続的なコピーを保存しないようにすることのプロキシ サーバーとブラウザーに通知するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="659c7-176">The NoStore property is used to inform proxy servers and browsers that they should not store a permanent copy of the cached content.</span></span>

#### <a name="varying-the-output-cache"></a><span data-ttu-id="659c7-177">出力キャッシュを変更</span><span class="sxs-lookup"><span data-stu-id="659c7-177">Varying the Output Cache</span></span>

<span data-ttu-id="659c7-178">状況によっては、まったく同じコンテンツの別のキャッシュされたバージョンを必要があります。</span><span class="sxs-lookup"><span data-stu-id="659c7-178">In some situations, you might want different cached versions of the very same content.</span></span> <span data-ttu-id="659c7-179">たとえば、マスター/詳細ページを作成することに想像してください。</span><span class="sxs-lookup"><span data-stu-id="659c7-179">Imagine, for example, that you are creating a master/detail page.</span></span> <span data-ttu-id="659c7-180">マスター ページには、ムービーのタイトルの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="659c7-180">The master page displays a list of movie titles.</span></span> <span data-ttu-id="659c7-181">タイトルをクリックすると、選択したムービーの詳細を取得します。</span><span class="sxs-lookup"><span data-stu-id="659c7-181">When you click a title, you get details for the selected movie.</span></span>

<span data-ttu-id="659c7-182">詳細ページをキャッシュする場合、同じムービーの詳細が表示されますどの映画をクリックします。</span><span class="sxs-lookup"><span data-stu-id="659c7-182">If you cache the details page, then the details for the same movie will be displayed no matter which movie you click.</span></span> <span data-ttu-id="659c7-183">最初のユーザーが選択した最初のムービーが将来のすべてのユーザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="659c7-183">The first movie selected by the first user will be displayed to all future users.</span></span>

<span data-ttu-id="659c7-184">この問題を解決するには、VaryByParam プロパティを活用して、 &lt;OutputCache&gt;属性。</span><span class="sxs-lookup"><span data-stu-id="659c7-184">You can fix this problem by taking advantage of the VaryByParam property of the &lt;OutputCache&gt; attribute.</span></span> <span data-ttu-id="659c7-185">このプロパティでは、フォーム パラメーターとまったく同じコンテンツの別のキャッシュされたバージョンを作成できます。 または、クエリ文字列パラメーターによって異なります。</span><span class="sxs-lookup"><span data-stu-id="659c7-185">This property enables you to create different cached versions of the very same content when a form parameter or query string parameter varies.</span></span>

<span data-ttu-id="659c7-186">たとえば、リスト 5 コント ローラーは、Master() Details() という 2 つのアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="659c7-186">For example, the controller in Listing 5 exposes two actions named Master() and Details().</span></span> <span data-ttu-id="659c7-187">Master() アクションはムービーのタイトルの一覧を返し、Details() アクションは、選択したムービーの詳細を返します。</span><span class="sxs-lookup"><span data-stu-id="659c7-187">The Master() action returns a list of movie titles and the Details() action returns the details for the selected movie.</span></span>

<span data-ttu-id="659c7-188">**5 – Controllers\MoviesController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="659c7-188">**Listing 5 – Controllers\MoviesController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample5.vb)]

<span data-ttu-id="659c7-189">Master() アクションには、値を持つ VaryByParam プロパティが含まれています。"none"です。</span><span class="sxs-lookup"><span data-stu-id="659c7-189">The Master() action includes a VaryByParam property with the value "none".</span></span> <span data-ttu-id="659c7-190">アクションが呼び出された Master() マスターの場合は、同じキャッシュされたバージョンの表示とが返されます。</span><span class="sxs-lookup"><span data-stu-id="659c7-190">When the Master() action is invoked, the same cached version of the Master view is returned.</span></span> <span data-ttu-id="659c7-191">パラメーターは、フォームのパラメーターやクエリ文字列には、(図 2 参照) が無視されます。</span><span class="sxs-lookup"><span data-stu-id="659c7-191">Any form parameters or query string parameters are ignored (see Figure 2).</span></span>

<span data-ttu-id="659c7-192">**図 2 –/Movies/Master ビュー**</span><span class="sxs-lookup"><span data-stu-id="659c7-192">**Figure 2 – The /Movies/Master view**</span></span>

![clip_image004](improving-performance-with-output-caching-vb/_static/image2.jpg)

<span data-ttu-id="659c7-194">**図 3 –/ムービー/詳細ビュー**</span><span class="sxs-lookup"><span data-stu-id="659c7-194">**Figure 3 – The /Movies/Details view**</span></span>

![clip_image006](improving-performance-with-output-caching-vb/_static/image3.jpg)

<span data-ttu-id="659c7-196">Details() アクションには、"Id"値を持つ VaryByParam プロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="659c7-196">The Details() action includes a VaryByParam property with the value "Id".</span></span> <span data-ttu-id="659c7-197">Id パラメーターのさまざまな値は、コント ローラー アクションに渡される、別のキャッシュされたバージョンの詳細ビューが生成されます。</span><span class="sxs-lookup"><span data-stu-id="659c7-197">When different values of the Id parameter are passed to the controller action, different cached versions of the Details view are generated.</span></span>

<span data-ttu-id="659c7-198">以上のキャッシュで VaryByParam プロパティの結果を使用することを理解して重要でない less をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="659c7-198">It is important to understand that using the VaryByParam property results in more caching and not less.</span></span> <span data-ttu-id="659c7-199">Id パラメーターの各バージョンでは別のキャッシュされたバージョンの詳細ビューが作成されます。</span><span class="sxs-lookup"><span data-stu-id="659c7-199">A different cached version of the Details view is created for each different version of the Id parameter.</span></span>

<span data-ttu-id="659c7-200">VaryByParam プロパティは、次の値に設定できます。</span><span class="sxs-lookup"><span data-stu-id="659c7-200">You can set the VaryByParam property to the following values:</span></span>

> <span data-ttu-id="659c7-201">\* = フォームまたはクエリ文字列パラメーターが変化するたびに、別のキャッシュされたバージョンを作成します。</span><span class="sxs-lookup"><span data-stu-id="659c7-201">\* = Create a different cached version whenever a form or query string parameter varies.</span></span>
> 
> <span data-ttu-id="659c7-202">none = しない別のキャッシュされたバージョンを作成します。</span><span class="sxs-lookup"><span data-stu-id="659c7-202">none = Never create different cached versions</span></span>
> 
> <span data-ttu-id="659c7-203">パラメーターのリストをセミコロン、リスト内のフォームまたはクエリ文字列パラメーターのいずれかによって異なりますたびに異なるキャッシュされたバージョンの作成を =</span><span class="sxs-lookup"><span data-stu-id="659c7-203">Semicolon list of parameters = Create different cached versions whenever any of the form or query string parameters in the list varies</span></span>


#### <a name="creating-a-cache-profile"></a><span data-ttu-id="659c7-204">キャッシュ プロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="659c7-204">Creating a Cache Profile</span></span>

<span data-ttu-id="659c7-205">プロパティを変更して出力キャッシュのプロパティを構成する代わりに、 &lt;OutputCache&gt;属性に、web の構成 (web.config) ファイルでキャッシュ プロファイルを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="659c7-205">As an alternative to configuring output cache properties by modifying properties of the &lt;OutputCache&gt; attribute, you can create a cache profile in the web configuration (web.config) file.</span></span> <span data-ttu-id="659c7-206">Web 構成ファイルでキャッシュ プロファイルを作成するには、重要な利点がいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="659c7-206">Creating a cache profile in the web configuration file offers a couple of important advantages.</span></span>

<span data-ttu-id="659c7-207">最初に、出力キャッシュを web 構成ファイルに構成すると、コント ローラー アクションが 1 つの場所にコンテンツをキャッシュする方法を制御できます。</span><span class="sxs-lookup"><span data-stu-id="659c7-207">First, by configuring output caching in the web configuration file, you can control how controller actions cache content in one central location.</span></span> <span data-ttu-id="659c7-208">1 つのキャッシュ プロファイルを作成し、いくつかのコント ローラーまたはコント ローラー アクションにプロファイルを適用することができます。</span><span class="sxs-lookup"><span data-stu-id="659c7-208">You can create one cache profile and apply the profile to several controllers or controller actions.</span></span>

<span data-ttu-id="659c7-209">次に、アプリケーションを再コンパイルしなくても、web 構成ファイルを変更できます。</span><span class="sxs-lookup"><span data-stu-id="659c7-209">Second, you can modify the web configuration file without recompiling your application.</span></span> <span data-ttu-id="659c7-210">実稼働環境に既にデプロイされているアプリケーションのキャッシュを無効にする必要がある場合、web 構成ファイルで定義されているキャッシュ プロファイルだけ変更できます。</span><span class="sxs-lookup"><span data-stu-id="659c7-210">If you need to disable caching for an application that has already been deployed to production, then you can simply modify the cache profiles defined in the web configuration file.</span></span> <span data-ttu-id="659c7-211">Web 構成ファイルへの変更が自動的に検出され、適用します。</span><span class="sxs-lookup"><span data-stu-id="659c7-211">Any changes to the web configuration file will be detected automatically and applied.</span></span>

<span data-ttu-id="659c7-212">たとえば、&lt;キャッシュ&gt;リスト 6 web 構成セクションが Cache1Hour をという名前のキャッシュ プロファイルを定義します。</span><span class="sxs-lookup"><span data-stu-id="659c7-212">For example, the &lt;caching&gt; web configuration section in Listing 6 defines a cache profile named Cache1Hour.</span></span> <span data-ttu-id="659c7-213">&lt;キャッシュ&gt;セクションで囲む必要があります、 &lt;system.web&gt; web 構成ファイルのセクション。</span><span class="sxs-lookup"><span data-stu-id="659c7-213">The &lt;caching&gt; section must appear within the &lt;system.web&gt; section of a web configuration file.</span></span>

<span data-ttu-id="659c7-214">**6 – web.config のキャッシュ セクションの一覧を表示します。**</span><span class="sxs-lookup"><span data-stu-id="659c7-214">**Listing 6 – Caching section for web.config**</span></span>

[!code-xml[Main](improving-performance-with-output-caching-vb/samples/sample6.xml)]

<span data-ttu-id="659c7-215">リスト 7 でコント ローラーはコント ローラー アクションとに Cache1Hour プロファイルを適用する方法を示しています、 &lt;OutputCache&gt;属性。</span><span class="sxs-lookup"><span data-stu-id="659c7-215">The controller in Listing 7 illustrates how you can apply the Cache1Hour profile to a controller action with the &lt;OutputCache&gt; attribute.</span></span>

<span data-ttu-id="659c7-216">**7 – Controllers\ProfileController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="659c7-216">**Listing 7 – Controllers\ProfileController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample7.vb)]

<span data-ttu-id="659c7-217">リスト 7 でコント ローラーによって公開される Index() アクションを呼び出す場合と同じ時間が 1 時間返されます。</span><span class="sxs-lookup"><span data-stu-id="659c7-217">If you invoke the Index() action exposed by the controller in Listing 7 then the same time will be returned for 1 hour.</span></span>

#### <a name="summary"></a><span data-ttu-id="659c7-218">まとめ</span><span class="sxs-lookup"><span data-stu-id="659c7-218">Summary</span></span>

<span data-ttu-id="659c7-219">出力キャッシュと、ASP.NET MVC アプリケーションのパフォーマンスを大幅に向上させるの非常に簡単なメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="659c7-219">Output caching provides you with a very easy method of dramatically improving the performance of your ASP.NET MVC applications.</span></span> <span data-ttu-id="659c7-220">このチュートリアルで使用する方法を学習しました、 &lt;OutputCache&gt;属性をコント ローラー アクションの出力をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="659c7-220">In this tutorial, you learned how to use the &lt;OutputCache&gt; attribute to cache the output of controller actions.</span></span> <span data-ttu-id="659c7-221">プロパティを変更する方法も学習しました、 &lt;OutputCache&gt;コンテンツがキャッシュを取得する方法を変更する期間と VaryByParam プロパティなどの属性。</span><span class="sxs-lookup"><span data-stu-id="659c7-221">You also learned how to modify properties of the &lt;OutputCache&gt; attribute such as the Duration and VaryByParam properties to modify how content gets cached.</span></span> <span data-ttu-id="659c7-222">最後に、web 構成ファイルでキャッシュ プロファイルを定義する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="659c7-222">Finally, you learned how to define cache profiles in the web configuration file.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="659c7-223">[前へ](understanding-action-filters-vb.md)
> [次へ](adding-dynamic-content-to-a-cached-page-vb.md)</span><span class="sxs-lookup"><span data-stu-id="659c7-223">[Previous](understanding-action-filters-vb.md)
[Next](adding-dynamic-content-to-a-cached-page-vb.md)</span></span>
