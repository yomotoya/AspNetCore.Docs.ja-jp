---
uid: mvc/overview/getting-started/introduction/adding-search
title: 検索 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 797384fb04d3ec6fa618ac1848a7efd548b30dbf
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362579"
---
<a name="search"></a><span data-ttu-id="ee098-102">検索</span><span class="sxs-lookup"><span data-stu-id="ee098-102">Search</span></span>
====================
<span data-ttu-id="ee098-103">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="ee098-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="ee098-104">Search メソッドとの検索ビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="ee098-104">Adding a Search Method and Search View</span></span>

<span data-ttu-id="ee098-105">このセクションでは、検索機能を追加します、`Index`できるようにするアクション メソッドがムービーをジャンルで名前を検索します。</span><span class="sxs-lookup"><span data-stu-id="ee098-105">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="updating-the-index-form"></a><span data-ttu-id="ee098-106">インデックスのフォームを更新しています</span><span class="sxs-lookup"><span data-stu-id="ee098-106">Updating the Index Form</span></span>

<span data-ttu-id="ee098-107">更新することで開始、`Index`を既存のアクション メソッド`MoviesController`クラス。</span><span class="sxs-lookup"><span data-stu-id="ee098-107">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="ee098-108">次のコードに示します。</span><span class="sxs-lookup"><span data-stu-id="ee098-108">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="ee098-109">最初の行、`Index`メソッドは、次を作成します。 [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) 、ムービーを選択するクエリ。</span><span class="sxs-lookup"><span data-stu-id="ee098-109">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="ee098-110">クエリでは、この時点では、定義されているが、まだデータベースに対して実行されていません。</span><span class="sxs-lookup"><span data-stu-id="ee098-110">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="ee098-111">場合、`searchString`パラメーターには文字列が含まれています、ムービー クエリが、次のコードを使用して、検索文字列の値にフィルターを変更します。</span><span class="sxs-lookup"><span data-stu-id="ee098-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="ee098-112">上の `s => s.Title` コードは[ラムダ式](https://msdn.microsoft.com/library/bb397687.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="ee098-112">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="ee098-113">ラムダは、メソッド ベースで使用[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)などの標準クエリ演算子メソッドの引数としてクエリ、[場所](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx)上記のコードで使用されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="ee098-113">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="ee098-114">定義されているとき、またはなどのメソッドを呼び出すことによって変更されるときに LINQ クエリは実行されません`Where`または`OrderBy`します。</span><span class="sxs-lookup"><span data-stu-id="ee098-114">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="ee098-115">代わりに、クエリの実行は延期されます、つまり、その具体値が実際に反復されるまで、式の評価が遅れること、または[ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ee098-115">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="ee098-116">`Search`サンプルでは、クエリがで実行、 *Index.cshtml*ビュー。</span><span class="sxs-lookup"><span data-stu-id="ee098-116">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="ee098-117">クエリの遅延実行の詳細については、「[クエリの実行](https://msdn.microsoft.com/library/bb738633.aspx)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ee098-117">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="ee098-118">[Contains](https://msdn.microsoft.com/library/bb155125.aspx)メソッドが、データベースでは、c# コードではなく上で実行します。</span><span class="sxs-lookup"><span data-stu-id="ee098-118">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="ee098-119">データベースに対して、 [Contains](https://msdn.microsoft.com/library/bb155125.aspx)にマップ[SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx)、これは大文字と小文字を区別しません。</span><span class="sxs-lookup"><span data-stu-id="ee098-119">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="ee098-120">更新することができますので、`Index`ユーザーに、フォームを表示するビュー。</span><span class="sxs-lookup"><span data-stu-id="ee098-120">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="ee098-121">アプリケーションを実行しに移動します */ムービー/インデックス*します。</span><span class="sxs-lookup"><span data-stu-id="ee098-121">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="ee098-122">`?searchString=ghost` などのクエリ文字列を URL に追加します。</span><span class="sxs-lookup"><span data-stu-id="ee098-122">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="ee098-123">フィルターされたムービーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee098-123">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="ee098-125">シグネチャを変更する場合、`Index`メソッドという名前のパラメーターに`id`、`id`パラメーターが一致、`{id}`既定値のプレース ホルダーで一連のルーティング、*アプリ\_作成できます。RouteConfig.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ee098-125">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="ee098-126">元の`Index`次のようなメソッド。</span><span class="sxs-lookup"><span data-stu-id="ee098-126">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="ee098-127">変更された`Index`メソッドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ee098-127">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="ee098-128">これで、クエリ文字列の値ではなく、ルート データ (URL セグメント) として検索タイトルを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="ee098-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="ee098-129">ただし、ユーザーがムービーを検索するたびに URL の変更を求めることはできません。</span><span class="sxs-lookup"><span data-stu-id="ee098-129">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="ee098-130">これを追加しますに役立つ UI でムービーをフィルターしています。</span><span class="sxs-lookup"><span data-stu-id="ee098-130">So now you you'll add UI to help them filter movies.</span></span> <span data-ttu-id="ee098-131">シグネチャを変更した場合、`Index`ルート バインド ID パラメーターを渡す方法をテストする方法を変更することができるように、`Index`という名前の文字列パラメーターを受け取ります`searchString`:</span><span class="sxs-lookup"><span data-stu-id="ee098-131">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="ee098-132">開く、 *Views\Movies\Index.cshtml*ファイル、および後だけ`@Html.ActionLink("Create New", "Create")`、以下に示す形式のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="ee098-132">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="ee098-133">`Html.BeginForm`ヘルパーの作成開始`<form>`タグ。</span><span class="sxs-lookup"><span data-stu-id="ee098-133">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="ee098-134">`Html.BeginForm`ヘルパーがユーザーをクリックして、フォームを送信すると、それ自体に投稿するためのフォーム、**フィルター**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee098-134">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="ee098-135">Visual Studio 2013 では、表示およびファイルの表示を編集するときに便利な機能強化があります。</span><span class="sxs-lookup"><span data-stu-id="ee098-135">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="ee098-136">アプリケーションを実行するビュー ファイルを開き、Visual Studio 2013 は、ビューを表示する適切なコント ローラー アクション メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ee098-136">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="ee098-137">インデックス ビュー (上記の図で示す) のように、Visual Studio で開く、Ctr f5 キーまたは f5 キーを押してアプリケーションを実行してから、ムービーを検索 をタップします。</span><span class="sxs-lookup"><span data-stu-id="ee098-137">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="ee098-138">ない`HttpPost`のオーバー ロード、`Index`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ee098-138">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="ee098-139">不要になった、メソッドは、アプリケーションの状態を変更されていないため、データをフィルターするだけです。</span><span class="sxs-lookup"><span data-stu-id="ee098-139">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="ee098-140">以下の `HttpPost Index` メソッドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="ee098-140">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="ee098-141">その場合は、アクション呼び出し元が一致、`HttpPost Index`メソッド、および`HttpPost Index`メソッドは、次の図に示すように実行が。</span><span class="sxs-lookup"><span data-stu-id="ee098-141">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="ee098-143">ただし、この `HttpPost` バージョンの `Index` メソッドを追加しても、実装方法は制限されます。</span><span class="sxs-lookup"><span data-stu-id="ee098-143">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="ee098-144">たとえば、特定の検索をブックマークするか、友だちにリンクを送信し、友だちがそれをクリックしてムービーのフィルターされた同じリストを表示できるようにするとします。</span><span class="sxs-lookup"><span data-stu-id="ee098-144">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="ee098-145">HTTP POST 要求の URL は、GET 要求 (localhost:xxxxx/ムービー/インデックス) の URL と同じ--、URL 自体で検索情報がないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ee098-145">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="ee098-146">右ここで、検索文字列の情報がサーバーに送信、フォーム フィールドの値として。</span><span class="sxs-lookup"><span data-stu-id="ee098-146">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="ee098-147">つまり、ブックマークまたは URL で友人に送信するには、その検索情報をキャプチャすることはできません。</span><span class="sxs-lookup"><span data-stu-id="ee098-147">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="ee098-148">ソリューションは、のオーバー ロードを使用する`BeginForm`POST 要求が URL に検索情報を追加する必要がありにルーティングする必要がありますを指定する、`HttpGet`のバージョン、`Index`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ee098-148">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="ee098-149">既存のパラメーターのない`BeginForm`メソッドを次のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="ee098-149">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="ee098-151">今すぐ検索を送信するときに、URL には検索のクエリ文字列が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ee098-151">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="ee098-152">`HttpPost Index` メソッドがある場合でも、検索時には `HttpGet Index` アクション メソッドにも移動します。</span><span class="sxs-lookup"><span data-stu-id="ee098-152">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="ee098-154">ジャンルによる検索の追加</span><span class="sxs-lookup"><span data-stu-id="ee098-154">Adding Search by Genre</span></span>

<span data-ttu-id="ee098-155">追加した場合、`HttpPost`のバージョン、`Index`メソッド、今すぐ削除しています。</span><span class="sxs-lookup"><span data-stu-id="ee098-155">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="ee098-156">次に、ユーザーがムービー ジャンルによる検索できるようにするための機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="ee098-156">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="ee098-157">`Index` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ee098-157">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="ee098-158">このバージョンの`Index`メソッドは、追加のパラメーターが namely`movieGenre`します。</span><span class="sxs-lookup"><span data-stu-id="ee098-158">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="ee098-159">最初の数行のコードを作成、`List`データベースからムービー ジャンルを保持するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="ee098-159">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="ee098-160">次のコードは、データベースからすべてのジャンルを取得する LINQ クエリです。</span><span class="sxs-lookup"><span data-stu-id="ee098-160">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="ee098-161">コードを使用して、`AddRange`メソッドがジェネリックの`List`個々 のすべてのジャンルを一覧に追加するコレクション。</span><span class="sxs-lookup"><span data-stu-id="ee098-161">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="ee098-162">(なし、`Distinct`修飾子は、重複するジャンルが追加されます: たとえば、コメディ サンプルでは 2 回追加されます)。</span><span class="sxs-lookup"><span data-stu-id="ee098-162">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="ee098-163">コードでジャンルのリストを格納し、`ViewBag.MovieGenre`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="ee098-163">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="ee098-164">カテゴリ データ (このようなムービー ジャンルの) として格納する、 [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx)オブジェクト、 `ViewBag`、MVC アプリケーションの一般的なアプローチには、ドロップダウン ボックスでカテゴリ データにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="ee098-164">Storing category data (such a movie genre's) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="ee098-165">次のコードを確認する方法を示しています、`movieGenre`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="ee098-165">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="ee098-166">空ではない、コードをさらには、指定されたジャンルを選択したムービーを制限するムービーのクエリを制約します。</span><span class="sxs-lookup"><span data-stu-id="ee098-166">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="ee098-167">既に説明したように、クエリで実行していないデータ ベース ムービー リストが反復処理されるまで (後のビューで動作する、`Index`アクション メソッドを返します)。</span><span class="sxs-lookup"><span data-stu-id="ee098-167">As stated previously, the query is not run on the data base until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="ee098-168">ジャンルによる検索をサポートするために、インデックス ビューにマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="ee098-168">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="ee098-169">追加、`Html.DropDownList`ヘルパーが、 *Views\Movies\Index.cshtml*直前に、ファイル、`TextBox`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="ee098-169">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="ee098-170">完成したマークアップは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="ee098-170">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="ee098-171">次のコード。</span><span class="sxs-lookup"><span data-stu-id="ee098-171">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="ee098-172">パラメーター"MovieGenre"キーを提供する、`DropDownList`を検索するためのヘルパーを`IEnumerable<SelectListItem>`で、`ViewBag`します。</span><span class="sxs-lookup"><span data-stu-id="ee098-172">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="ee098-173">`ViewBag`がアクション メソッドで設定されます。</span><span class="sxs-lookup"><span data-stu-id="ee098-173">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="ee098-174">オプション ラベルは、"All"パラメーター。</span><span class="sxs-lookup"><span data-stu-id="ee098-174">The parameter "All" provides an option label.</span></span> <span data-ttu-id="ee098-175">お使いのブラウザーでその選択を検査する場合、"value"属性が空であることがわかります。</span><span class="sxs-lookup"><span data-stu-id="ee098-175">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="ee098-176">コント ローラーのみがフィルターから`if`文字列ではありません`null`または空の値を送信する、空`movieGenre`すべてのジャンルを示しています。</span><span class="sxs-lookup"><span data-stu-id="ee098-176">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="ee098-177">既定で選択するオプションを設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="ee098-177">You can also set an option to be selected by default.</span></span> <span data-ttu-id="ee098-178">コント ローラーのコードを変更すると、既定のオプションとして「コメディ」場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="ee098-178">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="ee098-179">アプリケーションを実行しを参照する */ムービー/インデックス*します。</span><span class="sxs-lookup"><span data-stu-id="ee098-179">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="ee098-180">ジャンル、映画の名称、および両方の条件は、検索を実行してください。</span><span class="sxs-lookup"><span data-stu-id="ee098-180">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="ee098-181">このセクションでは、検索アクション メソッドとユーザーがムービーのタイトルとジャンルで検索できるようにするビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="ee098-181">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="ee098-182">次のセクションでは、プロパティを追加する方法について説明します、`Movie`モデルおよびテスト データベースが自動的に作成するには初期化子を追加する方法。</span><span class="sxs-lookup"><span data-stu-id="ee098-182">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ee098-183">[前へ](examining-the-edit-methods-and-edit-view.md)
> [次へ](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="ee098-183">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
