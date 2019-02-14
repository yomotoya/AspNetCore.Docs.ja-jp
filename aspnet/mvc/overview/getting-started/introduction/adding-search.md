---
uid: mvc/overview/getting-started/introduction/adding-search
title: 検索 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/17/2019
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: ada125c917656f3a83524ff39e53b4cfc041a497
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248382"
---
<a name="search"></a><span data-ttu-id="87436-102">検索</span><span class="sxs-lookup"><span data-stu-id="87436-102">Search</span></span>
====================

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="87436-103">Search メソッドとの検索ビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="87436-103">Adding a Search Method and Search View</span></span>

<span data-ttu-id="87436-104">このセクションでは、検索機能を追加します、`Index`できるようにするアクション メソッドがムービーをジャンルで名前を検索します。</span><span class="sxs-lookup"><span data-stu-id="87436-104">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87436-105">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="87436-105">Prerequisites</span></span>

<span data-ttu-id="87436-106">このセクションのスクリーン ショットを一致させるのには、(f5 キーを押して) アプリケーションを実行し、データベースに次の動画を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="87436-106">To match this section's screenshots, you need to run the application (F5) and add the following movies to the database.</span></span>

| <span data-ttu-id="87436-107">タイトル</span><span class="sxs-lookup"><span data-stu-id="87436-107">Title</span></span> | <span data-ttu-id="87436-108">リリース日</span><span class="sxs-lookup"><span data-stu-id="87436-108">Release Date</span></span> | <span data-ttu-id="87436-109">Genre</span><span class="sxs-lookup"><span data-stu-id="87436-109">Genre</span></span> | <span data-ttu-id="87436-110">価格</span><span class="sxs-lookup"><span data-stu-id="87436-110">Price</span></span> |
| ----- | ------------ | ----- | ----- |
| <span data-ttu-id="87436-111">Ghostbusters</span><span class="sxs-lookup"><span data-stu-id="87436-111">Ghostbusters</span></span> | <span data-ttu-id="87436-112">6/8/1984</span><span class="sxs-lookup"><span data-stu-id="87436-112">6/8/1984</span></span> | <span data-ttu-id="87436-113">コメディ</span><span class="sxs-lookup"><span data-stu-id="87436-113">Comedy</span></span> | <span data-ttu-id="87436-114">6.99</span><span class="sxs-lookup"><span data-stu-id="87436-114">6.99</span></span> |
| <span data-ttu-id="87436-115">Ghostbusters II</span><span class="sxs-lookup"><span data-stu-id="87436-115">Ghostbusters II</span></span> | <span data-ttu-id="87436-116">6/16/1989</span><span class="sxs-lookup"><span data-stu-id="87436-116">6/16/1989</span></span> | <span data-ttu-id="87436-117">コメディ</span><span class="sxs-lookup"><span data-stu-id="87436-117">Comedy</span></span> | <span data-ttu-id="87436-118">6.99</span><span class="sxs-lookup"><span data-stu-id="87436-118">6.99</span></span> |
| <span data-ttu-id="87436-119">Apes の地球</span><span class="sxs-lookup"><span data-stu-id="87436-119">Planet of the Apes</span></span> | <span data-ttu-id="87436-120">3/27/1986</span><span class="sxs-lookup"><span data-stu-id="87436-120">3/27/1986</span></span> | <span data-ttu-id="87436-121">アクション</span><span class="sxs-lookup"><span data-stu-id="87436-121">Action</span></span> | <span data-ttu-id="87436-122">5.99</span><span class="sxs-lookup"><span data-stu-id="87436-122">5.99</span></span> |


## <a name="updating-the-index-form"></a><span data-ttu-id="87436-123">インデックスのフォームを更新しています</span><span class="sxs-lookup"><span data-stu-id="87436-123">Updating the Index Form</span></span>

<span data-ttu-id="87436-124">更新することで開始、`Index`を既存のアクション メソッド`MoviesController`クラス。</span><span class="sxs-lookup"><span data-stu-id="87436-124">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="87436-125">次のコードに示します。</span><span class="sxs-lookup"><span data-stu-id="87436-125">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="87436-126">最初の行、`Index`メソッドは、次を作成します。 [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) 、ムービーを選択するクエリ。</span><span class="sxs-lookup"><span data-stu-id="87436-126">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="87436-127">クエリでは、この時点では、定義されているが、まだデータベースに対して実行されていません。</span><span class="sxs-lookup"><span data-stu-id="87436-127">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="87436-128">場合、`searchString`パラメーターには文字列が含まれています、ムービー クエリが、次のコードを使用して、検索文字列の値にフィルターを変更します。</span><span class="sxs-lookup"><span data-stu-id="87436-128">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="87436-129">上の `s => s.Title` コードは[ラムダ式](https://msdn.microsoft.com/library/bb397687.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="87436-129">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="87436-130">ラムダは、メソッド ベースで使用[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)などの標準クエリ演算子メソッドの引数としてクエリ、[場所](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx)上記のコードで使用されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="87436-130">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="87436-131">定義されているとき、またはなどのメソッドを呼び出すことによって変更されるときに LINQ クエリは実行されません`Where`または`OrderBy`します。</span><span class="sxs-lookup"><span data-stu-id="87436-131">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="87436-132">代わりに、クエリの実行は延期されます、つまり、その具体値が実際に反復されるまで、式の評価が遅れること、または[ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="87436-132">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="87436-133">`Search`サンプルでは、クエリがで実行、 *Index.cshtml*ビュー。</span><span class="sxs-lookup"><span data-stu-id="87436-133">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="87436-134">クエリの遅延実行の詳細については、「[クエリの実行](https://msdn.microsoft.com/library/bb738633.aspx)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="87436-134">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="87436-135">[Contains](https://msdn.microsoft.com/library/bb155125.aspx)メソッドが、データベースでは、c# コードではなく上で実行します。</span><span class="sxs-lookup"><span data-stu-id="87436-135">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="87436-136">データベースに対して、 [Contains](https://msdn.microsoft.com/library/bb155125.aspx)にマップ[SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx)、これは大文字と小文字を区別しません。</span><span class="sxs-lookup"><span data-stu-id="87436-136">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="87436-137">更新することができますので、`Index`ユーザーに、フォームを表示するビュー。</span><span class="sxs-lookup"><span data-stu-id="87436-137">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="87436-138">アプリケーションを実行しに移動します */ムービー/インデックス*します。</span><span class="sxs-lookup"><span data-stu-id="87436-138">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="87436-139">`?searchString=ghost` などのクエリ文字列を URL に追加します。</span><span class="sxs-lookup"><span data-stu-id="87436-139">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="87436-140">フィルターされたムービーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="87436-140">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="87436-142">シグネチャを変更する場合、`Index`メソッドという名前のパラメーターに`id`、`id`パラメーターが一致、`{id}`既定値のプレース ホルダーで一連のルーティング、*アプリ\_作成できます。RouteConfig.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="87436-142">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="87436-143">元の`Index`次のようなメソッド。</span><span class="sxs-lookup"><span data-stu-id="87436-143">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="87436-144">変更された`Index`メソッドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="87436-144">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="87436-145">これで、クエリ文字列の値ではなく、ルート データ (URL セグメント) として検索タイトルを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="87436-145">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="87436-146">ただし、ユーザーがムービーを検索するたびに URL の変更を求めることはできません。</span><span class="sxs-lookup"><span data-stu-id="87436-146">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="87436-147">そのため、ここでは UI を追加して、ムービーをフィルターできるようにします。</span><span class="sxs-lookup"><span data-stu-id="87436-147">So now you'll add UI to help them filter movies.</span></span> <span data-ttu-id="87436-148">シグネチャを変更した場合、`Index`ルート バインド ID パラメーターを渡す方法をテストする方法を変更することができるように、`Index`という名前の文字列パラメーターを受け取ります`searchString`:</span><span class="sxs-lookup"><span data-stu-id="87436-148">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="87436-149">開く、 *Views\Movies\Index.cshtml*ファイル、および後だけ`@Html.ActionLink("Create New", "Create")`、以下に示す形式のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="87436-149">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="87436-150">`Html.BeginForm`ヘルパーの作成開始`<form>`タグ。</span><span class="sxs-lookup"><span data-stu-id="87436-150">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="87436-151">`Html.BeginForm`ヘルパーがユーザーをクリックして、フォームを送信すると、それ自体に投稿するためのフォーム、**フィルター**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="87436-151">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="87436-152">Visual Studio 2013 では、表示およびファイルの表示を編集するときに便利な機能強化があります。</span><span class="sxs-lookup"><span data-stu-id="87436-152">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="87436-153">アプリケーションを実行するビュー ファイルを開き、Visual Studio 2013 は、ビューを表示する適切なコント ローラー アクション メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="87436-153">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="87436-154">インデックス ビュー (上記の図で示す) のように、Visual Studio で開く、Ctr f5 キーまたは f5 キーを押してアプリケーションを実行してから、ムービーを検索 をタップします。</span><span class="sxs-lookup"><span data-stu-id="87436-154">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="87436-155">ない`HttpPost`のオーバー ロード、`Index`メソッド。</span><span class="sxs-lookup"><span data-stu-id="87436-155">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="87436-156">不要になった、メソッドは、アプリケーションの状態を変更されていないため、データをフィルターするだけです。</span><span class="sxs-lookup"><span data-stu-id="87436-156">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="87436-157">以下の `HttpPost Index` メソッドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="87436-157">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="87436-158">その場合は、アクション呼び出し元が一致、`HttpPost Index`メソッド、および`HttpPost Index`メソッドは、次の図に示すように実行が。</span><span class="sxs-lookup"><span data-stu-id="87436-158">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="87436-160">ただし、この `HttpPost` バージョンの `Index` メソッドを追加しても、実装方法は制限されます。</span><span class="sxs-lookup"><span data-stu-id="87436-160">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="87436-161">たとえば、特定の検索をブックマークするか、友だちにリンクを送信し、友だちがそれをクリックしてムービーのフィルターされた同じリストを表示できるようにするとします。</span><span class="sxs-lookup"><span data-stu-id="87436-161">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="87436-162">HTTP POST 要求の URL は、GET 要求 (localhost:xxxxx/ムービー/インデックス) の URL と同じ--、URL 自体で検索情報がないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="87436-162">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="87436-163">右ここで、検索文字列の情報がサーバーに送信、フォーム フィールドの値として。</span><span class="sxs-lookup"><span data-stu-id="87436-163">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="87436-164">つまり、ブックマークまたは URL で友人に送信するには、その検索情報をキャプチャすることはできません。</span><span class="sxs-lookup"><span data-stu-id="87436-164">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="87436-165">ソリューションは、のオーバー ロードを使用する`BeginForm`POST 要求が URL に検索情報を追加する必要がありにルーティングする必要がありますを指定する、`HttpGet`のバージョン、`Index`メソッド。</span><span class="sxs-lookup"><span data-stu-id="87436-165">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="87436-166">既存のパラメーターのない`BeginForm`メソッドを次のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="87436-166">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="87436-168">今すぐ検索を送信するときに、URL には検索のクエリ文字列が含まれます。</span><span class="sxs-lookup"><span data-stu-id="87436-168">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="87436-169">`HttpPost Index` メソッドがある場合でも、検索時には `HttpGet Index` アクション メソッドにも移動します。</span><span class="sxs-lookup"><span data-stu-id="87436-169">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="87436-171">ジャンルによる検索の追加</span><span class="sxs-lookup"><span data-stu-id="87436-171">Adding Search by Genre</span></span>

<span data-ttu-id="87436-172">追加した場合、`HttpPost`のバージョン、`Index`メソッド、今すぐ削除しています。</span><span class="sxs-lookup"><span data-stu-id="87436-172">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="87436-173">次に、ユーザーがムービー ジャンルによる検索できるようにするための機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="87436-173">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="87436-174">
  `Index\` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="87436-174">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="87436-175">このバージョンの`Index`メソッドは、追加のパラメーターが namely`movieGenre`します。</span><span class="sxs-lookup"><span data-stu-id="87436-175">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="87436-176">最初の数行のコードを作成、`List`データベースからムービー ジャンルを保持するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="87436-176">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="87436-177">次のコードは、データベースからすべてのジャンルを取得する LINQ クエリです。</span><span class="sxs-lookup"><span data-stu-id="87436-177">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="87436-178">コードを使用して、`AddRange`メソッドがジェネリックの`List`個々 のすべてのジャンルを一覧に追加するコレクション。</span><span class="sxs-lookup"><span data-stu-id="87436-178">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="87436-179">(なし、`Distinct`修飾子は、重複するジャンルが追加されます: たとえば、コメディ サンプルでは 2 回追加されます)。</span><span class="sxs-lookup"><span data-stu-id="87436-179">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="87436-180">コードでジャンルのリストを格納し、`ViewBag.MovieGenre`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="87436-180">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="87436-181">カテゴリ データ (このようなムービー ジャンル) として格納する、 [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx)オブジェクト、 `ViewBag`、MVC アプリケーションの一般的なアプローチには、ドロップダウン ボックスでカテゴリ データにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="87436-181">Storing category data (such a movie genres) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="87436-182">次のコードを確認する方法を示しています、`movieGenre`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="87436-182">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="87436-183">空ではない、コードをさらには、指定されたジャンルを選択したムービーを制限するムービーのクエリを制約します。</span><span class="sxs-lookup"><span data-stu-id="87436-183">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="87436-184">既に説明したように、クエリで実行していないデータベース ムービー リストが反復処理されるまで (後のビューで動作する、`Index`アクション メソッドを返します)。</span><span class="sxs-lookup"><span data-stu-id="87436-184">As stated previously, the query is not run on the database until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="87436-185">ジャンルによる検索をサポートするために、インデックス ビューにマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="87436-185">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="87436-186">追加、`Html.DropDownList`ヘルパーが、 *Views\Movies\Index.cshtml*直前に、ファイル、`TextBox`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="87436-186">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="87436-187">完成したマークアップは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="87436-187">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="87436-188">次のコード。</span><span class="sxs-lookup"><span data-stu-id="87436-188">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="87436-189">パラメーター"MovieGenre"キーを提供する、`DropDownList`を検索するためのヘルパーを`IEnumerable<SelectListItem>`で、`ViewBag`します。</span><span class="sxs-lookup"><span data-stu-id="87436-189">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="87436-190">`ViewBag`がアクション メソッドで設定されます。</span><span class="sxs-lookup"><span data-stu-id="87436-190">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="87436-191">オプション ラベルは、"All"パラメーター。</span><span class="sxs-lookup"><span data-stu-id="87436-191">The parameter "All" provides an option label.</span></span> <span data-ttu-id="87436-192">お使いのブラウザーでその選択を検査する場合、"value"属性が空であることがわかります。</span><span class="sxs-lookup"><span data-stu-id="87436-192">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="87436-193">コント ローラーのみがフィルターから`if`文字列ではありません`null`または空の値を送信する、空`movieGenre`すべてのジャンルを示しています。</span><span class="sxs-lookup"><span data-stu-id="87436-193">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="87436-194">既定で選択するオプションを設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="87436-194">You can also set an option to be selected by default.</span></span> <span data-ttu-id="87436-195">コント ローラーのコードを変更すると、既定のオプションとして「コメディ」場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="87436-195">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="87436-196">アプリケーションを実行しを参照する */ムービー/インデックス*します。</span><span class="sxs-lookup"><span data-stu-id="87436-196">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="87436-197">ジャンル、映画の名称、および両方の条件は、検索を実行してください。</span><span class="sxs-lookup"><span data-stu-id="87436-197">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="87436-198">このセクションでは、検索アクション メソッドとユーザーがムービーのタイトルとジャンルで検索できるようにするビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="87436-198">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="87436-199">次のセクションでは、プロパティを追加する方法について説明します、`Movie`モデルおよびテスト データベースが自動的に作成するには初期化子を追加する方法。</span><span class="sxs-lookup"><span data-stu-id="87436-199">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="87436-200">[前へ](examining-the-edit-methods-and-edit-view.md)
> [次へ](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="87436-200">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
