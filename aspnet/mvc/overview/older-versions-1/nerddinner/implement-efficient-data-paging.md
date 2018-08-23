---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: 効率的なデータ ページングを実装する |Microsoft Docs
author: microsoft
description: 手順 8 では、dinners 一度に数千ものを表示するには、代わりで今後 10 の dinners を表示しますがのみように、/Dinners URL にページング サポートを追加する方法を示します.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 2bef690355cd1f89a15a67f0c49775296d551136
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833344"
---
<a name="implement-efficient-data-paging"></a><span data-ttu-id="d1223-103">効率的なデータ ページングを実装します。</span><span class="sxs-lookup"><span data-stu-id="d1223-103">Implement Efficient Data Paging</span></span>
====================
<span data-ttu-id="d1223-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d1223-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="d1223-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="d1223-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="d1223-106">これは、無料の手順 8 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。</span><span class="sxs-lookup"><span data-stu-id="d1223-106">This is step 8 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="d1223-107">手順 8 では、dinners 一度に数千ものを表示するには、代わりには、のみ - 一度に 10 個の今後の dinners を表示し、戻るページし、SEO フレンドリな方法で、一覧全体を転送するエンドユーザーを許可するありますようにする、/Dinners URL にページング サポートを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d1223-107">Step 8 shows how to add paging support to our /Dinners URL so that instead of displaying 1000s of dinners at once, we'll only display 10 upcoming dinners at a time - and allow end-users to page back and forward through the entire list in an SEO friendly way.</span></span>
> 
> <span data-ttu-id="d1223-108">次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="d1223-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-8-paging-support"></a><span data-ttu-id="d1223-109">NerdDinner 手順 8: ページングのサポート</span><span class="sxs-lookup"><span data-stu-id="d1223-109">NerdDinner Step 8: Paging Support</span></span>

<span data-ttu-id="d1223-110">私たちのサイトが成功した場合は、何千もの今後の dinners があります。</span><span class="sxs-lookup"><span data-stu-id="d1223-110">If our site is successful, it will have thousands of upcoming dinners.</span></span> <span data-ttu-id="d1223-111">UI がすべてこれらの dinners の処理を拡張し、それらを参照することができます、かどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1223-111">We need to make sure that our UI scales to handle all of these dinners, and allows users to browse them.</span></span> <span data-ttu-id="d1223-112">これを有効にするのには、ページング サポートを追加します、 */Dinners*のみが数千の dinners を表示するのにはの代わりに URL を一度に - 10 今後 dinners を表示し、戻るページおよびでは、すべてのリストを転送するエンドユーザーを許可します。SEO のわかりやすい方法です。</span><span class="sxs-lookup"><span data-stu-id="d1223-112">To enable this, we'll add paging support to our */Dinners* URL so that instead of displaying 1000s of dinners at once, we'll only display 10 upcoming dinners at a time - and allow end-users to page back and forward through the entire list in an SEO friendly way.</span></span>

### <a name="index-action-method-recap"></a><span data-ttu-id="d1223-113">Index() アクション メソッドの要約</span><span class="sxs-lookup"><span data-stu-id="d1223-113">Index() Action Method Recap</span></span>

<span data-ttu-id="d1223-114">Index() アクション メソッド、DinnersController クラス内で現在は次のような。</span><span class="sxs-lookup"><span data-stu-id="d1223-114">The Index() action method within our DinnersController class currently looks like below:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

<span data-ttu-id="d1223-115">要求はに対して行われた場合、 */Dinners* URL、今後のすべて dinners の一覧を取得し、それらのすべての一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="d1223-115">When a request is made to the */Dinners* URL, it retrieves a list of all upcoming dinners and then renders a listing of all of them out:</span></span>

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a><span data-ttu-id="d1223-116">Understanding IQuerable&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="d1223-116">Understanding IQuerable&lt;T&gt;</span></span>

<span data-ttu-id="d1223-117">*IQueryable&lt;T&gt;* は .NET 3.5 の一部として、LINQ で導入されたインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="d1223-117">*IQueryable&lt;T&gt;* is an interface that was introduced with LINQ as part of .NET 3.5.</span></span> <span data-ttu-id="d1223-118">活用ページング サポートを実装するために実行できる強力な「遅延実行」のシナリオを可能になります。</span><span class="sxs-lookup"><span data-stu-id="d1223-118">It enables powerful "deferred execution" scenarios that we can take advantage of to implement paging support.</span></span>

<span data-ttu-id="d1223-119">IQueryable を返すことのときは必ず DinnerRepository で&lt;Dinner&gt; FindUpcomingDinners() メソッドからシーケンス。</span><span class="sxs-lookup"><span data-stu-id="d1223-119">In our DinnerRepository we are returning an IQueryable&lt;Dinner&gt; sequence from our FindUpcomingDinners() method:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

<span data-ttu-id="d1223-120">対象の IQueryable&lt;Dinner&gt; FindUpcomingDinners() メソッドによって返されるオブジェクトは、LINQ to SQL を使用して、データベースから Dinner オブジェクトを取得するクエリをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="d1223-120">The IQueryable&lt;Dinner&gt; object returned by our FindUpcomingDinners() method encapsulates a query to retrieve Dinner objects from our database using LINQ to SQL.</span></span> <span data-ttu-id="d1223-121">重要なは、か ToList() メソッドを呼び出すことで、クエリ内のデータをアクセス/反復処理するまでのデータベースに対してクエリを実行しません。</span><span class="sxs-lookup"><span data-stu-id="d1223-121">Importantly, it won't execute the query against the database until we attempt to access/iterate over the data in the query, or until we call the ToList() method on it.</span></span> <span data-ttu-id="d1223-122">この FindUpcomingDinners() メソッドを呼び出すコードは、IQueryable に追加の「チェーン」操作/フィルターを追加する必要に応じて選択できます&lt;Dinner&gt;オブジェクト クエリを実行する前にします。</span><span class="sxs-lookup"><span data-stu-id="d1223-122">The code calling our FindUpcomingDinners() method can optionally choose to add additional "chained" operations/filters to the IQueryable&lt;Dinner&gt; object before executing the query.</span></span> <span data-ttu-id="d1223-123">LINQ to SQL は、データが要求されたときに、データベースに対して結合のクエリを実行するのに十分なスマートでは、します。</span><span class="sxs-lookup"><span data-stu-id="d1223-123">LINQ to SQL is then smart enough to execute the combined query against the database when the data is requested.</span></span>

<span data-ttu-id="d1223-124">返される対象の IQueryable を「スキップ」と「実行」の追加の演算子が適用されるページング ロジックを実装するために、DinnersController の Index() アクション メソッドを更新しましたできます&lt;Dinner&gt;に ToList() を呼び出す前にシーケンス。</span><span class="sxs-lookup"><span data-stu-id="d1223-124">To implement paging logic we can update our DinnersController's Index() action method so that it applies additional "Skip" and "Take" operators to the returned IQueryable&lt;Dinner&gt; sequence before calling ToList() on it:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

<span data-ttu-id="d1223-125">上記のコードでは、データベース内の最初の 10 の今後 dinners をスキップし、戻る 20 dinners を返します。</span><span class="sxs-lookup"><span data-stu-id="d1223-125">The above code skips over the first 10 upcoming dinners in the database, and then returns back 20 dinners.</span></span> <span data-ttu-id="d1223-126">LINQ to SQL では、– SQL データベースと web サーバーではなくロジックをスキップしています。 これを実行する最適化された SQL クエリを構築できるほどスマートです。</span><span class="sxs-lookup"><span data-stu-id="d1223-126">LINQ to SQL is smart enough to construct an optimized SQL query that performs this skipping logic in the SQL database – and not in the web-server.</span></span> <span data-ttu-id="d1223-127">これは、データベースに何百万もの今後の Dinners がある場合でも、必要な 10 のみは (ので効率的でスケーラブルな) この要求の一部として取得することを意味します。</span><span class="sxs-lookup"><span data-stu-id="d1223-127">This means that even if we have millions of upcoming Dinners in the database, only the 10 we want will be retrieved as part of this request (making it efficient and scalable).</span></span>

### <a name="adding-a-page-value-to-the-url"></a><span data-ttu-id="d1223-128">URL に「ページ」値を追加します。</span><span class="sxs-lookup"><span data-stu-id="d1223-128">Adding a "page" value to the URL</span></span>

<span data-ttu-id="d1223-129">特定のページ範囲をハードコーディングするのではなく、Url にユーザーを要求しているどの Dinner 範囲を示す「ページ」パラメーターを含めてがいいでしょう。</span><span class="sxs-lookup"><span data-stu-id="d1223-129">Instead of hard-coding a specific page range, we'll want our URLs to include a "page" parameter that indicates which Dinner range a user is requesting.</span></span>

#### <a name="using-a-querystring-value"></a><span data-ttu-id="d1223-130">クエリ文字列値を使用します。</span><span class="sxs-lookup"><span data-stu-id="d1223-130">Using a Querystring value</span></span>

<span data-ttu-id="d1223-131">次のコードでは、クエリ文字列パラメーターをサポートし、Url などを有効にする、Index() アクション メソッドの更新を示します */Dinners でしょうかページ 2 =*:。</span><span class="sxs-lookup"><span data-stu-id="d1223-131">The code below demonstrates how we can update our Index() action method to support a querystring parameter and enable URLs like */Dinners?page=2*:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

<span data-ttu-id="d1223-132">上記の Index() アクション メソッドには、「ページ」という名前のパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="d1223-132">The Index() action method above has a parameter named "page".</span></span> <span data-ttu-id="d1223-133">パラメーターが null 許容の整数として宣言されている (どのような int ですか? を示します)。</span><span class="sxs-lookup"><span data-stu-id="d1223-133">The parameter is declared as a nullable integer (that is what int? indicates).</span></span> <span data-ttu-id="d1223-134">つまり、 */Dinners でしょうか。 ページ 2 =* URL パラメーター値として渡されるには、"2"の値になります。</span><span class="sxs-lookup"><span data-stu-id="d1223-134">This means that the */Dinners?page=2* URL will cause a value of "2" to be passed as the parameter value.</span></span> <span data-ttu-id="d1223-135">*/Dinners* (クエリ文字列値) のない URL が渡される null 値になります。</span><span class="sxs-lookup"><span data-stu-id="d1223-135">The */Dinners* URL (without a querystring value) will cause a null value to be passed.</span></span>

<span data-ttu-id="d1223-136">ページ サイズ (この場合は 10 行) をページの値を乗算するをスキップする数の dinners を判断することは。</span><span class="sxs-lookup"><span data-stu-id="d1223-136">We are multiplying the page value by the page size (in this case 10 rows) to determine how many dinners to skip over.</span></span> <span data-ttu-id="d1223-137">使用している、 [c# null「結合」演算子 (?)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) null 許容型を扱う場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="d1223-137">We are using the [C# null "coalescing" operator (??)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) which is useful when dealing with nullable types.</span></span> <span data-ttu-id="d1223-138">上記のコードは、ページのパラメーターが null の場合に、ページの 0 の値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="d1223-138">The code above assigns page the value of 0 if the page parameter is null.</span></span>

#### <a name="using-embedded-url-values"></a><span data-ttu-id="d1223-139">埋め込まれた URL 値を使用してください。</span><span class="sxs-lookup"><span data-stu-id="d1223-139">Using Embedded URL values</span></span>

<span data-ttu-id="d1223-140">クエリ文字列値を使用する代わりには、埋め込む自体は実際の URL 内にあるページ パラメーターになります。</span><span class="sxs-lookup"><span data-stu-id="d1223-140">An alternative to using a querystring value would be to embed the page parameter within the actual URL itself.</span></span> <span data-ttu-id="d1223-141">例: */Dinners/Page/2*または */Dinners/2*します。</span><span class="sxs-lookup"><span data-stu-id="d1223-141">For example: */Dinners/Page/2* or */Dinners/2*.</span></span> <span data-ttu-id="d1223-142">ASP.NET MVC には、このようなシナリオをサポートしやすい強力な URL ルーティング エンジンが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d1223-142">ASP.NET MVC includes a powerful URL routing engine that makes it easy to support scenarios like this.</span></span>

<span data-ttu-id="d1223-143">受信 URL または URL の形式、コント ローラー クラスまたはアクション メソッドをマップするカスタムのルーティング規則を登録することができます。</span><span class="sxs-lookup"><span data-stu-id="d1223-143">We can register custom routing rules that map any incoming URL or URL format to any controller class or action method we want.</span></span> <span data-ttu-id="d1223-144">To do 必要がありますは、このプロジェクト内で、Global.asax ファイルを開くことだけです。</span><span class="sxs-lookup"><span data-stu-id="d1223-144">All we need to-do is to open the Global.asax file within our project:</span></span>

![](implement-efficient-data-paging/_static/image2.png)

<span data-ttu-id="d1223-145">ルートの最初の呼び出しのような MapRoute() ヘルパー メソッドを使用して、新しいマッピング ルールを登録します。MapRoute() 下:</span><span class="sxs-lookup"><span data-stu-id="d1223-145">And then register a new mapping rule using the MapRoute() helper method like the first call to routes.MapRoute() below:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

<span data-ttu-id="d1223-146">上記の"UpcomingDinners"をという名前の新しいルーティング規則が登録されます。</span><span class="sxs-lookup"><span data-stu-id="d1223-146">Above we are registering a new routing rule named "UpcomingDinners".</span></span> <span data-ttu-id="d1223-147">URL の形式であることを示すことは"Dinners/ページ/{ページ}": {ページ} は、URL に埋め込まれたパラメーター値。</span><span class="sxs-lookup"><span data-stu-id="d1223-147">We are indicating it has the URL format "Dinners/Page/{page}" – where {page} is a parameter value embedded within the URL.</span></span> <span data-ttu-id="d1223-148">MapRoute() メソッドの 3 番目のパラメーターは、DinnersController クラス Index() アクション メソッドには、この形式に一致する Url をマップする必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="d1223-148">The third parameter to the MapRoute() method indicates that we should map URLs that match this format to the Index() action method on the DinnersController class.</span></span>

<span data-ttu-id="d1223-149">点を除いて、URL とクエリ文字列ではないから、「ページ」パラメーターになるようになりました、Querystring シナリオ – 以前まったく同じ Index() コードを使用できます。</span><span class="sxs-lookup"><span data-stu-id="d1223-149">We can use the exact same Index() code we had before with our Querystring scenario – except now our "page" parameter will come from the URL and not the querystring:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

<span data-ttu-id="d1223-150">ここでときに、アプリケーションを実行し、入力と */Dinners*最初の 10 個の今後の dinners を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="d1223-150">And now when we run the application and type in */Dinners* we'll see the first 10 upcoming dinners:</span></span>

![](implement-efficient-data-paging/_static/image3.png)

<span data-ttu-id="d1223-151">入力 */Dinners/Page/1* dinners の次のページを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="d1223-151">And when we type in */Dinners/Page/1* we'll see the next page of dinners:</span></span>

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a><span data-ttu-id="d1223-152">ページ ナビゲーション UI を追加します。</span><span class="sxs-lookup"><span data-stu-id="d1223-152">Adding page navigation UI</span></span>

<span data-ttu-id="d1223-153">Dinner データを簡単にスキップできるように、ビュー テンプレート内の"next"および「前」のナビゲーション UI を実装するために、ページングのシナリオを完了する最後の手順になります。</span><span class="sxs-lookup"><span data-stu-id="d1223-153">The last step to complete our paging scenario will be to implement "next" and "previous" navigation UI within our view template to enable users to easily skip over the Dinner data.</span></span>

<span data-ttu-id="d1223-154">これを正しく実装するに必要になります Dinners の総数をデータベースにわかっているにも方法は複数のデータ ページと解釈されます。</span><span class="sxs-lookup"><span data-stu-id="d1223-154">To implement this correctly, we'll need to know the total number of Dinners in the database, as well as how many pages of data this translates to.</span></span> <span data-ttu-id="d1223-155">現在要求されている「ページ」値が先頭または、データの末尾かどうかを計算し、表示を切り替える適宜「前」と"次へ の UI を非表示にし、必要になります。</span><span class="sxs-lookup"><span data-stu-id="d1223-155">We'll then need to calculate whether the currently requested "page" value is at the beginning or end of the data, and show or hide the "previous" and "next" UI accordingly.</span></span> <span data-ttu-id="d1223-156">このロジックを実装、Index() アクション メソッド内で可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d1223-156">We could implement this logic within our Index() action method.</span></span> <span data-ttu-id="d1223-157">また、プロジェクトを再利用可能な複数の方法でこのロジックをカプセル化するヘルパー クラスを追加できます。</span><span class="sxs-lookup"><span data-stu-id="d1223-157">Alternatively we can add a helper class to our project that encapsulates this logic in a more re-usable way.</span></span>

<span data-ttu-id="d1223-158">一覧から派生した単純な"PaginatedList"ヘルパー クラスを次に示します&lt;T&gt;コレクション クラスに組み込まれて、.NET Framework です。</span><span class="sxs-lookup"><span data-stu-id="d1223-158">Below is a simple "PaginatedList" helper class that derives from the List&lt;T&gt; collection class built-into the .NET Framework.</span></span> <span data-ttu-id="d1223-159">IQueryable データの任意のシーケンスを改ページを使用できる再利用可能なコレクション クラスを実装しています。</span><span class="sxs-lookup"><span data-stu-id="d1223-159">It implements a re-usable collection class that can be used to paginate any sequence of IQueryable data.</span></span> <span data-ttu-id="d1223-160">アプリケーションで NerdDinner した IQueryable 経由で動作&lt;Dinner&gt;結果がされる可能性がありますには簡単に IQueryable に対して&lt;製品&gt;または IQueryable&lt;顧客&gt;他のアプリケーション シナリオになります。</span><span class="sxs-lookup"><span data-stu-id="d1223-160">In our NerdDinner application we'll have it work over IQueryable&lt;Dinner&gt; results, but it could just as easily be used against IQueryable&lt;Product&gt; or IQueryable&lt;Customer&gt; results in other application scenarios:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

<span data-ttu-id="d1223-161">上の例が計算され、"PageIndex"、"PageSize"、"TotalCount"および"TotalPages"などの公開のプロパティの します。</span><span class="sxs-lookup"><span data-stu-id="d1223-161">Notice above how it calculates and then exposes properties like "PageIndex", "PageSize", "TotalCount", and "TotalPages".</span></span> <span data-ttu-id="d1223-162">ヘルパーの 2 つのプロパティ"HasPreviousPage"と"HasNextPage"コレクション内のデータのページが先頭または元のシーケンスの末尾にあるかどうかを示すも公開します。</span><span class="sxs-lookup"><span data-stu-id="d1223-162">It also then exposes two helper properties "HasPreviousPage" and "HasNextPage" that indicate whether the page of data in the collection is at the beginning or end of the original sequence.</span></span> <span data-ttu-id="d1223-163">上記のコードを実行する - Dinner オブジェクトの合計数の数を取得する最初の 2 つの SQL クエリとなります (これには、オブジェクトを返さない – ではなく整数を返す"SELECT COUNT"ステートメントを実行します)、2 番目の行だけを取得するにはデータは、データベースのデータの現在のページから必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1223-163">The above code will cause two SQL queries to be run - the first to retrieve the count of the total number of Dinner objects (this doesn't return the objects – rather it performs a "SELECT COUNT" statement that returns an integer), and the second to retrieve just the rows of data we need from our database for the current page of data.</span></span>

<span data-ttu-id="d1223-164">今後、PaginatedList を作成する、DinnersController.Index() ヘルパー メソッドを更新できるし&lt;Dinner&gt; DinnerRepository.FindUpcomingDinners() から、発生して、ビュー テンプレートに渡すこと。</span><span class="sxs-lookup"><span data-stu-id="d1223-164">We can then update our DinnersController.Index() helper method to create a PaginatedList&lt;Dinner&gt; from our DinnerRepository.FindUpcomingDinners() result, and pass it to our view template:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

<span data-ttu-id="d1223-165">ViewPage から継承する \Views\Dinners\Index.aspx ビュー テンプレートを更新することができますし&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; ViewPage ではなく&lt;IEnumerable&lt;Dinner&gt;&gt;、または次または前のナビゲーション UI を非表示に、ビュー テンプレートの下部に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d1223-165">We can then update the \Views\Dinners\Index.aspx view template to inherit from ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt;&gt; instead of ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;, and then add the following code to the bottom of our view-template to show or hide next and previous navigation UI:</span></span>

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

<span data-ttu-id="d1223-166">このハイパーリンクを生成する Html.RouteLink() のヘルパー メソッドを使って方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="d1223-166">Notice above how we are using the Html.RouteLink() helper method to generate our hyperlinks.</span></span> <span data-ttu-id="d1223-167">このメソッドは、以前に使用した Html.ActionLink() ヘルパー メソッドに似ています。</span><span class="sxs-lookup"><span data-stu-id="d1223-167">This method is similar to the Html.ActionLink() helper method we've used previously.</span></span> <span data-ttu-id="d1223-168">違いは、"UpcomingDinners"ルーティング規則、Global.asax ファイル内でセットアップを使用して URL を生成することです。</span><span class="sxs-lookup"><span data-stu-id="d1223-168">The difference is that we are generating the URL using the "UpcomingDinners" routing rule we setup within our Global.asax file.</span></span> <span data-ttu-id="d1223-169">これにより、Url、形式を持つ、Index() アクション メソッドを生成します: */Dinners/ページ/{ページ}* -{ページ} の値が提供するという上記現在 PageIndex に基づいて変数は、場所。</span><span class="sxs-lookup"><span data-stu-id="d1223-169">This ensures that we'll generate URLs to our Index() action method that have the format: */Dinners/Page/{page}* – where the {page} value is a variable we are providing above based on the current PageIndex.</span></span>

<span data-ttu-id="d1223-170">これで、アプリケーションを実行するともう一度おいでください、一度に 10 個の dinners、ブラウザーで。</span><span class="sxs-lookup"><span data-stu-id="d1223-170">And now when we run our application again we'll see 10 dinners at a time in our browser:</span></span>

![](implement-efficient-data-paging/_static/image5.png)

<span data-ttu-id="d1223-171">またある&lt; &lt; &lt;と&gt; &gt; &gt;転送をスキップし、旧バージョンとを使用して、データをエンジンにアクセスできる Url を検索できるようにするページの下部にある UI のナビゲーション。</span><span class="sxs-lookup"><span data-stu-id="d1223-171">We also have &lt;&lt;&lt; and &gt;&gt;&gt; navigation UI at the bottom of the page that allows us to skip forwards and backwards over our data using search engine accessible URLs:</span></span>

![](implement-efficient-data-paging/_static/image6.png)

| <span data-ttu-id="d1223-172">**側のトピック: IQueryable の影響を理解する&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="d1223-172">**Side Topic: Understanding the implications of IQueryable&lt;T&gt;**</span></span> |
| --- |
| <span data-ttu-id="d1223-173">IQueryable&lt;T&gt;により、遅延実行の興味深いシナリオのさまざまな強力な機能は、(ページングおよびコンポジションのように基づくクエリ)。</span><span class="sxs-lookup"><span data-stu-id="d1223-173">IQueryable&lt;T&gt; is a very powerful feature that enables a variety of interesting deferred execution scenarios (like paging and composition based queries).</span></span> <span data-ttu-id="d1223-174">として、強力な機能はすべて、使用する方法を慎重にしてはいないに悪用されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d1223-174">As with all powerful features, you want to be careful with how you use it and make sure it is not abused.</span></span> <span data-ttu-id="d1223-175">IQueryable を返すことを認識することが重要&lt;T&gt;リポジトリからの結果により、呼び出し元のコードは、連結演算子のメソッドに追加し、そのため、最終的なクエリの実行に参加します。</span><span class="sxs-lookup"><span data-stu-id="d1223-175">It is important to recognize that returning an IQueryable&lt;T&gt; result from your repository enables calling code to append on chained operator methods to it, and so participate in the ultimate query execution.</span></span> <span data-ttu-id="d1223-176">この機能は、呼び出し元のコードを提供したくないかどうかは、返す必要があります IList をバックアップ&lt;T&gt;または IEnumerable&lt;T&gt;が既に実行しているクエリの結果を含んだ結果。</span><span class="sxs-lookup"><span data-stu-id="d1223-176">If you do not want to provide calling code this ability, then you should return back IList&lt;T&gt; or IEnumerable&lt;T&gt; results - which contain the results of a query that has already executed.</span></span> <span data-ttu-id="d1223-177">改ページ調整シナリオをリポジトリ メソッドが呼び出される実際のデータの改ページ調整ロジックをプッシュする必要がこれは。</span><span class="sxs-lookup"><span data-stu-id="d1223-177">For pagination scenarios this would require you to push the actual data pagination logic into the repository method being called.</span></span> <span data-ttu-id="d1223-178">このシナリオでは、返される、PaginatedList がいずれかの署名、FindUpcomingDinners() finder メソッドを更新可能性があります: PaginatedList&lt; Dinner&gt; FindUpcomingDinners (pageIndex の int、int pageSize) {} 返された場合は、IList をバックアップまたは&lt;Dinner&gt;、Dinners の合計数を返す"totalCount"out パラメーターを使用して: IList&lt;Dinner&gt; FindUpcomingDinners (pageIndex の int、int totalCount アウトの int pageSize) {}</span><span class="sxs-lookup"><span data-stu-id="d1223-178">In this scenario we might update our FindUpcomingDinners() finder method to have a signature that either returned a PaginatedList: PaginatedList&lt; Dinner&gt; FindUpcomingDinners(int pageIndex, int pageSize) { } Or return back an IList&lt;Dinner&gt;, and use a "totalCount" out param to return the total count of Dinners: IList&lt;Dinner&gt; FindUpcomingDinners(int pageIndex, int pageSize, out int totalCount) { }</span></span> |

### <a name="next-step"></a><span data-ttu-id="d1223-179">次の手順</span><span class="sxs-lookup"><span data-stu-id="d1223-179">Next Step</span></span>

<span data-ttu-id="d1223-180">これで、アプリケーションへの認証と承認のサポートを追加する方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="d1223-180">Let's now look at how we can add authentication and authorization support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d1223-181">[前へ](re-use-ui-using-master-pages-and-partials.md)
> [次へ](secure-applications-using-authentication-and-authorization.md)</span><span class="sxs-lookup"><span data-stu-id="d1223-181">[Previous](re-use-ui-using-master-pages-and-partials.md)
[Next](secure-applications-using-authentication-and-authorization.md)</span></span>
