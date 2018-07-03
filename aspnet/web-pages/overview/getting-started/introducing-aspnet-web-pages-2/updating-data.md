---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: ASP.NET Web Pages の概要 - データベースのデータの更新 |Microsoft Docs
author: tfitzmac
description: このチュートリアルでは、ASP.NET Web Pages (Razor) を使用する場合は、(変更)、既存のデータベース エントリを更新する方法を示します。 シリーズを完了したと想定して th.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 259034a795b9dff7001165a182bc0e84bb690491
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377059"
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a><span data-ttu-id="ee805-104">ASP.NET Web ページの概要 - データベースのデータを更新しています</span><span class="sxs-lookup"><span data-stu-id="ee805-104">Introducing ASP.NET Web Pages - Updating Database Data</span></span>
====================
<span data-ttu-id="ee805-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ee805-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ee805-106">このチュートリアルでは、ASP.NET Web Pages (Razor) を使用する場合は、(変更)、既存のデータベース エントリを更新する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ee805-106">This tutorial shows you how to update (change) an existing database entry when you use ASP.NET Web Pages (Razor).</span></span> <span data-ttu-id="ee805-107">を通じてシリーズを完了したと想定して[入力データを使用してフォームを使用して ASP.NET Web Pages](entering-data.md)します。</span><span class="sxs-lookup"><span data-stu-id="ee805-107">It assumes you have completed the series through [Entering Data by Using Forms Using ASP.NET Web Pages](entering-data.md).</span></span>
> 
> <span data-ttu-id="ee805-108">学習内容。</span><span class="sxs-lookup"><span data-stu-id="ee805-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="ee805-109">内の個々 のレコードを選択する方法、`WebGrid`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="ee805-109">How to select an individual record in the `WebGrid` helper.</span></span>
> - <span data-ttu-id="ee805-110">データベースから 1 つのレコードを読み取る方法。</span><span class="sxs-lookup"><span data-stu-id="ee805-110">How to read a single record from a database.</span></span>
> - <span data-ttu-id="ee805-111">データベース レコードの値を持つフォームをプリロードする方法。</span><span class="sxs-lookup"><span data-stu-id="ee805-111">How to preload a form with values from the database record.</span></span>
> - <span data-ttu-id="ee805-112">データベースの既存のレコードを更新する方法。</span><span class="sxs-lookup"><span data-stu-id="ee805-112">How to update an existing record in a database.</span></span>
> - <span data-ttu-id="ee805-113">表示する ページで情報を格納する方法。</span><span class="sxs-lookup"><span data-stu-id="ee805-113">How to store information in the page without displaying it.</span></span>
> - <span data-ttu-id="ee805-114">非表示フィールドを使用して情報を格納する方法。</span><span class="sxs-lookup"><span data-stu-id="ee805-114">How to use a hidden field to store information.</span></span>
>   
> 
> <span data-ttu-id="ee805-115">説明した機能/テクノロジ:</span><span class="sxs-lookup"><span data-stu-id="ee805-115">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="ee805-116">`WebGrid`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="ee805-116">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="ee805-117">SQL`Update`コマンド。</span><span class="sxs-lookup"><span data-stu-id="ee805-117">The SQL `Update` command.</span></span>
> - <span data-ttu-id="ee805-118">`Database.Execute` メソッド。</span><span class="sxs-lookup"><span data-stu-id="ee805-118">The `Database.Execute` method.</span></span>
> - <span data-ttu-id="ee805-119">フィールドを非表示 (`<input type="hidden">`)。</span><span class="sxs-lookup"><span data-stu-id="ee805-119">Hidden fields (`<input type="hidden">`).</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="ee805-120">構築します</span><span class="sxs-lookup"><span data-stu-id="ee805-120">What You'll Build</span></span>

<span data-ttu-id="ee805-121">前のチュートリアルでは、データベースにレコードを追加する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="ee805-121">In the previous tutorial, you learned how to add a record to a database.</span></span> <span data-ttu-id="ee805-122">ここでは、編集するためのレコードを表示する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="ee805-122">Here, you'll learn how to display a record for editing.</span></span> <span data-ttu-id="ee805-123">*映画*を更新します ページで、`WebGrid`ヘルパーを表示できるように、**編集**各ムービーの横にあるリンク。</span><span class="sxs-lookup"><span data-stu-id="ee805-123">In the *Movies* page, you'll update the `WebGrid` helper so that it displays an **Edit** link next to each movie:</span></span>

![各ムービーの [編集] リンクを含む WebGrid の表示します。](updating-data/_static/image1.png)

<span data-ttu-id="ee805-125">クリックすると、**編集**リンクに移動する別のページでムービー情報が既にフォームで。</span><span class="sxs-lookup"><span data-stu-id="ee805-125">When you click the **Edit** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![編集するムービーを表示するムービー ページを編集します。](updating-data/_static/image2.png)

<span data-ttu-id="ee805-127">任意の値を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="ee805-127">You can change any of the values.</span></span> <span data-ttu-id="ee805-128">変更を送信するときにページ内のコードは、データベースを更新し、ムービーの一覧に戻ります。</span><span class="sxs-lookup"><span data-stu-id="ee805-128">When you submit the changes, the code in the page updates the database and takes you back to the movie listing.</span></span>

<span data-ttu-id="ee805-129">プロセスのこの部分の動作と同様にほぼ、 *AddMovie.cshtml*のため、このチュートリアルの多くはなじみのある、前のチュートリアルで作成したページです。</span><span class="sxs-lookup"><span data-stu-id="ee805-129">This part of the process works almost exactly like the *AddMovie.cshtml* page you created in the previous tutorial, so much of this tutorial will be familiar.</span></span>

<span data-ttu-id="ee805-130">個々 のムービーを編集する方法を実装することがいくつかの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="ee805-130">There are several ways you could implement a way to edit an individual movie.</span></span> <span data-ttu-id="ee805-131">アプローチには、簡単に実装するために、わかりやすくするために選択されました。</span><span class="sxs-lookup"><span data-stu-id="ee805-131">The approach shown was chosen because it's easy to implement and easy to understand.</span></span>

## <a name="adding-an-edit-link-to-the-movie-listing"></a><span data-ttu-id="ee805-132">ムービーの一覧を編集 リンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="ee805-132">Adding an Edit Link to the Movie Listing</span></span>

<span data-ttu-id="ee805-133">更新を開始する、*映画*ページも一覧表示する各ムービーが含まれるように、**編集**リンク。</span><span class="sxs-lookup"><span data-stu-id="ee805-133">To begin, you'll update the *Movies* page so that each movie listing also contains an **Edit** link.</span></span>

<span data-ttu-id="ee805-134">開く、 *Movies.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ee805-134">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="ee805-135">ページの本文には、変更、`WebGrid`列を追加してマークアップ。</span><span class="sxs-lookup"><span data-stu-id="ee805-135">In the body of the page, change the `WebGrid` markup by adding a column.</span></span> <span data-ttu-id="ee805-136">変更後のマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="ee805-136">Here's the modified markup:</span></span>

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

<span data-ttu-id="ee805-137">新しい列では、この 1 つです。</span><span class="sxs-lookup"><span data-stu-id="ee805-137">The new column is this one:</span></span>

[!code-html[Main](updating-data/samples/sample2.html)]

<span data-ttu-id="ee805-138">このコラムのポイントは、リンクを表示するのには (`<a>`要素)「編集」と表示されました。</span><span class="sxs-lookup"><span data-stu-id="ee805-138">The point of this column is to show a link (`<a>` element) whose text says "Edit".</span></span> <span data-ttu-id="ee805-139">次のページを実行すると、次のようなリンクを作成するには必要になると、`id`各ムービーのさまざまな値。</span><span class="sxs-lookup"><span data-stu-id="ee805-139">What we're after is to create a link that looks like the following when the page runs, with the `id` value different for each movie:</span></span>

[!code-css[Main](updating-data/samples/sample3.css)]

<span data-ttu-id="ee805-140">このリンクをという名前のページを呼び出す*EditMovie*、クエリ文字列を渡すと`?id=7`そのページにします。</span><span class="sxs-lookup"><span data-stu-id="ee805-140">This link will invoke a page named *EditMovie*, and it will pass the query string `?id=7` to that page.</span></span>

<span data-ttu-id="ee805-141">新しい列の構文は少し複雑になりますが、いくつかの要素をまとめた、ためにのみです。</span><span class="sxs-lookup"><span data-stu-id="ee805-141">The syntax for the new column might look a bit complex, but that's only because it puts together several elements.</span></span> <span data-ttu-id="ee805-142">各要素は簡単です。</span><span class="sxs-lookup"><span data-stu-id="ee805-142">Each individual element is straightforward.</span></span> <span data-ttu-id="ee805-143">集中する場合だけ、`<a>`要素では、このマークアップを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ee805-143">If you concentrate on just the `<a>` element, you see this markup:</span></span>

[!code-html[Main](updating-data/samples/sample4.html)]

<span data-ttu-id="ee805-144">グリッドのしくみについていくつかのバック グラウンド: グリッド、データベースのレコードを 1 つの行を表示して、データベース レコードの各フィールドの列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee805-144">Some background about how the grid works: the grid displays rows, one for each database record, and it displays columns for each field in the database record.</span></span> <span data-ttu-id="ee805-145">グリッドの各行が構築されるときに、`item`オブジェクトには、その行のデータベース レコード (アイテム) が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ee805-145">While each grid row is being constructed, the `item` object contains the database record (item) for that row.</span></span> <span data-ttu-id="ee805-146">これによって、その行のデータを取得するためのコードで方法がわかります。</span><span class="sxs-lookup"><span data-stu-id="ee805-146">This arrangement gives you a way in code to get at the data for that row.</span></span> <span data-ttu-id="ee805-147">ここです: 式`item.ID`が現在のデータベースのアイテムの ID 値を取得します。</span><span class="sxs-lookup"><span data-stu-id="ee805-147">That's what you see here: the expression `item.ID` is getting the ID value of the current database item.</span></span> <span data-ttu-id="ee805-148">取得できます (タイトル、ジャンル、または年) のデータベースの値のいずれかと同じ方法を使用して`item.Title`、 `item.Genre`、または`item.Year`します。</span><span class="sxs-lookup"><span data-stu-id="ee805-148">You could get any of the database values (title, genre, or year) the same way by using `item.Title`, `item.Genre`, or `item.Year`.</span></span>

<span data-ttu-id="ee805-149">式`"~/EditMovie?id=@item.ID`ターゲット URL のハード コーディングされた一部の結合 (`~/EditMovie?id=`) 動的に派生この ID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ee805-149">The expression `"~/EditMovie?id=@item.ID` combines the hard-coded part of the target URL (`~/EditMovie?id=`) with this dynamically derived ID.</span></span> <span data-ttu-id="ee805-150">(表示、`~`演算子は、前のチュートリアルでは現在の web サイトのルートを表す ASP.NET 演算子)。</span><span class="sxs-lookup"><span data-stu-id="ee805-150">(You saw the `~` operator in the previous tutorial; it's an ASP.NET operator that represents the current website root.)</span></span>

<span data-ttu-id="ee805-151">結果は、列内のマークアップのこの部分だけが生成されること、次のマークアップのようなもの実行時に。</span><span class="sxs-lookup"><span data-stu-id="ee805-151">The result is that this part of the markup in the column simply produces something like the following markup at run time:</span></span>

[!code-xml[Main](updating-data/samples/sample5.xml)]

<span data-ttu-id="ee805-152">実際の値では当然ながら、`id`行ごとに異なります。</span><span class="sxs-lookup"><span data-stu-id="ee805-152">Naturally, the actual value of `id` will be different for each row.</span></span>

## <a name="creating-a-custom-display-for-a-grid-column"></a><span data-ttu-id="ee805-153">グリッド列のカスタムの表示を作成します。</span><span class="sxs-lookup"><span data-stu-id="ee805-153">Creating a Custom Display for a Grid Column</span></span>

<span data-ttu-id="ee805-154">グリッド列にバックアップするようになりました。</span><span class="sxs-lookup"><span data-stu-id="ee805-154">Now back to the grid column.</span></span> <span data-ttu-id="ee805-155">3 つの列は、最初にユーザーが、グリッドが表示されるデータ値のみ (タイトル、ジャンル、および年)。</span><span class="sxs-lookup"><span data-stu-id="ee805-155">The three columns you originally had in the grid displayed only data values (title, genre, and year).</span></span> <span data-ttu-id="ee805-156">この表示を指定されたデータベース列の名前を渡すことによって&mdash;など`grid.Column("Title")`します。</span><span class="sxs-lookup"><span data-stu-id="ee805-156">You specified this display by passing the name of the database column &mdash; for example, `grid.Column("Title")`.</span></span>

<span data-ttu-id="ee805-157">この新しい**編集**リンク列は異なります。</span><span class="sxs-lookup"><span data-stu-id="ee805-157">This new **Edit** link column is different.</span></span> <span data-ttu-id="ee805-158">渡そうとしている列名を指定するには、代わりに、`format`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="ee805-158">Instead of specifying a column name, you're passing a `format` parameter.</span></span> <span data-ttu-id="ee805-159">このパラメーターを使用して、マークアップを定義できますが、`WebGrid`と共にヘルパーがレンダリングされます、`item`太字または緑として列のデータを表示またはした形式で、必要な値。</span><span class="sxs-lookup"><span data-stu-id="ee805-159">This parameter lets you define markup that the `WebGrid` helper will render along with the `item` value to display the column data as bold or green or in whatever format that you want.</span></span> <span data-ttu-id="ee805-160">たとえば、タイトルを太字で表示する場合は、この例のように列を作成します。</span><span class="sxs-lookup"><span data-stu-id="ee805-160">For example, if you wanted the title to appear bold, you could create a column like this example:</span></span>

[!code-html[Main](updating-data/samples/sample6.html)]

<span data-ttu-id="ee805-161">(さまざまな`@`中の文字、`format`プロパティは、マークアップとコード値間の遷移をマークします)。</span><span class="sxs-lookup"><span data-stu-id="ee805-161">(The various `@` characters you see in the `format` property mark the transition between markup and a code value.)</span></span>

<span data-ttu-id="ee805-162">について知っておくと、`format`プロパティは、理解しやすいですか、新しい**編集**リンク列をまとめます。</span><span class="sxs-lookup"><span data-stu-id="ee805-162">Once you know about the `format` property, it's easier to understand how the new **Edit** link column is put together:</span></span>

[!code-html[Main](updating-data/samples/sample7.html)]

<span data-ttu-id="ee805-163">列で構成されます*のみ*のリンクをレンダリングするマークアップをさらにいくつかの情報 (ID) をから抽出された行のデータベース レコードです。</span><span class="sxs-lookup"><span data-stu-id="ee805-163">The column consists *only* of the markup that renders the link, plus some information (the ID) that's extracted from the database record for the row.</span></span>

> [!TIP]
> 
> <span data-ttu-id="ee805-164">**名前付きパラメーターとメソッドの位置指定パラメーター**</span><span class="sxs-lookup"><span data-stu-id="ee805-164">**Named Parameters and Positional Parameters for a Method**</span></span>
> 
> <span data-ttu-id="ee805-165">何度もメソッドを呼び出すし、パラメーターが渡されたした、単純に掲載するコンマで区切られたパラメーター値。</span><span class="sxs-lookup"><span data-stu-id="ee805-165">Many times when you've called a method and passed parameters to it, you've simply listed the parameter values separated by commas.</span></span> <span data-ttu-id="ee805-166">ここでは例をいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="ee805-166">Here are a couple of examples:</span></span>
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> <span data-ttu-id="ee805-167">このコードは、最初に説明しましたが、各ケースでは、特定の順序でメソッドにパラメーターを渡しているときに、問題を伝え&mdash;具体的には、パラメーターがそのメソッドで定義されている順序。</span><span class="sxs-lookup"><span data-stu-id="ee805-167">We didn't mention the issue when you first saw this code, but in each case, you're passing parameters to the methods in a specific order &mdash; namely, the order in which the parameters are defined in that method.</span></span> <span data-ttu-id="ee805-168">`db.Execute`と`Validation.RequireFields`、渡す値の順序を混在している場合は、ページを実行すると、エラー メッセージまたは少なくともいくつかの予期しない結果を取得ができます。</span><span class="sxs-lookup"><span data-stu-id="ee805-168">For `db.Execute` and `Validation.RequireFields`, if you mixed up the order of the values you pass, you'd get an error message when the page runs, or at least some strange results.</span></span> <span data-ttu-id="ee805-169">明らかにのパラメーターを渡す順序を理解する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ee805-169">Clearly, you have to know the order to pass the parameters in.</span></span> <span data-ttu-id="ee805-170">(WebMatrix で、IntelliSense 知るのに役立つ名前、種類、およびパラメーターの順序を把握します。)</span><span class="sxs-lookup"><span data-stu-id="ee805-170">(In WebMatrix, IntelliSense can help you learn figure out the name, type, and order of the parameters.)</span></span>
> 
> <span data-ttu-id="ee805-171">使用することができますの順序で値の受け渡しする代わりに、*名前付きパラメーター*します。</span><span class="sxs-lookup"><span data-stu-id="ee805-171">As an alternative to passing values in order, you can use *named parameters*.</span></span> <span data-ttu-id="ee805-172">(を使用すると呼ばれる、順序でパラメーターを渡す*位置指定パラメーター*)。名前付きパラメーターは、明示的に含めるパラメーターの名前の値を渡すときにします。</span><span class="sxs-lookup"><span data-stu-id="ee805-172">(Passing parameters in order is known as using *positional parameters*.) For named parameters, you explicitly include the name of the parameter when passing its value.</span></span> <span data-ttu-id="ee805-173">使用して名前付きパラメーター既に何度もこれらのチュートリアル。</span><span class="sxs-lookup"><span data-stu-id="ee805-173">You've used named parameters already a number of times in these tutorials.</span></span> <span data-ttu-id="ee805-174">例えば:</span><span class="sxs-lookup"><span data-stu-id="ee805-174">For example:</span></span>
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> <span data-ttu-id="ee805-175">と、呼び出し</span><span class="sxs-lookup"><span data-stu-id="ee805-175">and</span></span>
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> <span data-ttu-id="ee805-176">名前付きパラメーターは、メソッドは、多くのパラメーターを受け取る場合は特に、2 つの状況で便利です。</span><span class="sxs-lookup"><span data-stu-id="ee805-176">Named parameters are handy for a couple of situations, especially when a method takes many parameters.</span></span> <span data-ttu-id="ee805-177">1 つは、1 つまたは 2 つのパラメーターを渡すに渡す値がパラメーター リスト内の最初の位置の間ではないです。</span><span class="sxs-lookup"><span data-stu-id="ee805-177">One is when you want to pass only one or two parameters, but the values you want to pass are not among the first positions in the parameter list.</span></span> <span data-ttu-id="ee805-178">別の状況に最も適した順序でパラメーターを渡すことによって、コードを読みやすくするときです。</span><span class="sxs-lookup"><span data-stu-id="ee805-178">Another situation is when you want to make your code more readable by passing the parameters in the order that makes the most sense to you.</span></span>
> 
> <span data-ttu-id="ee805-179">当然ながら、名前付きパラメーターを使用するパラメーターの名前を認識する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ee805-179">Obviously, to use named parameters, you have to know the names of the parameters.</span></span> <span data-ttu-id="ee805-180">WebMatrix IntelliSense できます*表示*する、名前が、できません現在情報を入力できます。</span><span class="sxs-lookup"><span data-stu-id="ee805-180">WebMatrix IntelliSense can *show* you the names, but it cannot currently fill them in for you.</span></span>


## <a name="creating-the-edit-page"></a><span data-ttu-id="ee805-181">[編集] ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="ee805-181">Creating the Edit Page</span></span>

<span data-ttu-id="ee805-182">作成できるようになりました、 *EditMovie*ページ。</span><span class="sxs-lookup"><span data-stu-id="ee805-182">Now you can create the *EditMovie* page.</span></span> <span data-ttu-id="ee805-183">ユーザーがクリックすると、**編集**リンクでは、このページの最終的なします。</span><span class="sxs-lookup"><span data-stu-id="ee805-183">When users click the **Edit** link, they'll end up on this page.</span></span>

<span data-ttu-id="ee805-184">という名前のページを作成する*EditMovie.cshtml*と新機能を次のマークアップ ファイルで置換します。</span><span class="sxs-lookup"><span data-stu-id="ee805-184">Create a page named *EditMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

<span data-ttu-id="ee805-185">このマークアップとコードにあるような*AddMovie*ページ。</span><span class="sxs-lookup"><span data-stu-id="ee805-185">This markup and code is similar to what you have in the *AddMovie* page.</span></span> <span data-ttu-id="ee805-186">[送信] ボタンのテキストにわずかな違いがあります。</span><span class="sxs-lookup"><span data-stu-id="ee805-186">There's a small difference in the text for the submit button.</span></span> <span data-ttu-id="ee805-187">同様、 *AddMovie*ページでは、`Html.ValidationSummary`呼び出しが発生した場合に検証エラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="ee805-187">As with the *AddMovie* page, there's an `Html.ValidationSummary` call that will display validation errors if there are any.</span></span> <span data-ttu-id="ee805-188">呼び出しを除外しているこの時間`Validation.Message`検証の概要でエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee805-188">This time we're leaving out calls to `Validation.Message`, since errors will be displayed in the validation summary.</span></span> <span data-ttu-id="ee805-189">前述の前のチュートリアルでは、検証の概要と、個々 のエラー メッセージをさまざまな組み合わせで使用できます。</span><span class="sxs-lookup"><span data-stu-id="ee805-189">As noted in the previous tutorial, you can use the validation summary and the individual error messages in various combinations.</span></span>

<span data-ttu-id="ee805-190">もう一度ことに注意して、`method`の属性、`<form>`要素に設定されて`post`します。</span><span class="sxs-lookup"><span data-stu-id="ee805-190">Notice again that the `method` attribute of the `<form>` element is set to `post`.</span></span> <span data-ttu-id="ee805-191">同様、 *AddMovie.cshtml*  ページで、このページに変更を加えるデータベース。</span><span class="sxs-lookup"><span data-stu-id="ee805-191">As with the *AddMovie.cshtml* page, this page makes changes to the database.</span></span> <span data-ttu-id="ee805-192">そのため、このフォームを実行する必要があります、`POST`操作。</span><span class="sxs-lookup"><span data-stu-id="ee805-192">Therefore, this form should perform a `POST` operation.</span></span> <span data-ttu-id="ee805-193">(の違いの詳細について`GET`と`POST`、操作を参照してください、 [GET、POST、および HTTP 動詞の安全性](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety)HTML フォームのチュートリアルではサイドバーです)。</span><span class="sxs-lookup"><span data-stu-id="ee805-193">(For more about the difference between `GET` and `POST` operations, see the [GET, POST, and HTTP Verb Safety](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) sidebar in the tutorial on HTML forms.)</span></span>

<span data-ttu-id="ee805-194">前のチュートリアルで示した、 `value` Razor コードでそれらをプリロードするために、テキスト ボックスの属性が設定されています。</span><span class="sxs-lookup"><span data-stu-id="ee805-194">As you saw in an earlier tutorial, the `value` attributes of the text boxes are being set with Razor code in order to preload them.</span></span> <span data-ttu-id="ee805-195">今回は、ただし、使用しているような変数`title`と`genre`の代わりにそのタスク`Request.Form["title"]`:</span><span class="sxs-lookup"><span data-stu-id="ee805-195">This time, though, you're using variables like `title` and `genre` for that task instead of `Request.Form["title"]`:</span></span>

`<input type="text" name="title" value="@title" />`

<span data-ttu-id="ee805-196">としてする前に、このマークアップは事前に読み込むムービーの値を持つテキスト ボックスの値。</span><span class="sxs-lookup"><span data-stu-id="ee805-196">As before, this markup will preload the text box values with the movie values.</span></span> <span data-ttu-id="ee805-197">使用する代わりにこの変数を使用する便利な理由はすぐに表示されます、`Request`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="ee805-197">You'll see in a moment why it's handy to use variables this time instead of using the `Request` object.</span></span>

<span data-ttu-id="ee805-198">`<input type="hidden">`このページの要素。</span><span class="sxs-lookup"><span data-stu-id="ee805-198">There's also a `<input type="hidden">` element on this page.</span></span> <span data-ttu-id="ee805-199">この要素は、ページで表示されるようにすることがなく、映画 ID を格納します。</span><span class="sxs-lookup"><span data-stu-id="ee805-199">This element stores the movie ID without making it visible on the page.</span></span> <span data-ttu-id="ee805-200">クエリ文字列値を使用して、ページに、ID が渡される最初に (`?id=7`または URL に似ています)。</span><span class="sxs-lookup"><span data-stu-id="ee805-200">The ID is initially passed to the page by using a query string value (`?id=7` or similar in the URL).</span></span> <span data-ttu-id="ee805-201">非表示フィールドに ID 値を配置することで行うことができます使用可能なであることを確認して、フォームが送信されるときに、ページを起動した元の URL へのアクセスがある不要になった場合でもです。</span><span class="sxs-lookup"><span data-stu-id="ee805-201">By putting the ID value into a hidden field, you can make sure that it's available when the form is submitted, even if you no longer have access to the original URL that the page was invoked with.</span></span>

<span data-ttu-id="ee805-202">異なり、 *AddMovie*ページのコードを*EditMovie*ページが 2 つの異なる機能を持っています。</span><span class="sxs-lookup"><span data-stu-id="ee805-202">Unlike the *AddMovie* page, the code for the *EditMovie* page has two distinct functions.</span></span> <span data-ttu-id="ee805-203">最初に、ページが表示されるときに、最初の関数は (と*のみ*し)、コードは、クエリ文字列から映画 ID を取得します。</span><span class="sxs-lookup"><span data-stu-id="ee805-203">The first function is that when the page is displayed for the first time (and *only* then), the code gets the movie ID from the query string.</span></span> <span data-ttu-id="ee805-204">コードでは、ID、データベースから対応するムービーを読み取り、表示 (事前に読み込む)、テキスト ボックスに。</span><span class="sxs-lookup"><span data-stu-id="ee805-204">The code then uses the ID to read the corresponding movie out of the database and display (preload) it in the text boxes.</span></span>

<span data-ttu-id="ee805-205">ユーザーがクリックしたときに 2 番目の関数は、**変更の送信**ボタン、テキスト ボックスの値を読み取るし、それらを検証するコードには。</span><span class="sxs-lookup"><span data-stu-id="ee805-205">The second function is that when the user clicks the **Submit Changes** button, the code has to read the values of the text boxes and validate them.</span></span> <span data-ttu-id="ee805-206">コードは、新しい値を持つデータベース アイテムの更新にもあります。</span><span class="sxs-lookup"><span data-stu-id="ee805-206">The code also has to update the database item with the new values.</span></span> <span data-ttu-id="ee805-207">この手法で見たように、レコードの追加と同様に*AddMovie*します。</span><span class="sxs-lookup"><span data-stu-id="ee805-207">This technique is similar to adding a record, as you saw in *AddMovie*.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="ee805-208">1 つのムービーを読み取るコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="ee805-208">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="ee805-209">最初の関数を実行するには、ページの上部にこのコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="ee805-209">To perform the first function, add this code to the top of the page:</span></span>

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

<span data-ttu-id="ee805-210">このコードのほとんどが開始されるブロックの内部`if(!IsPost)`します。</span><span class="sxs-lookup"><span data-stu-id="ee805-210">Most of this code is inside a block that starts `if(!IsPost)`.</span></span> <span data-ttu-id="ee805-211">`!`式のことを意味するために、演算子が"not"を意味*場合、この要求が post 送信*、流の間接的な手段である*かどうか、この要求はこのページが実行されて初めて*。</span><span class="sxs-lookup"><span data-stu-id="ee805-211">The `!` operator means "not," so the expression means *if this request is not a post submission*, which is an indirect way of saying *if this request is the first time that this page has been run*.</span></span> <span data-ttu-id="ee805-212">前述のように、このコードを実行する必要があります*のみ*初めてページを実行します。</span><span class="sxs-lookup"><span data-stu-id="ee805-212">As noted earlier, this code should run *only* the first time the page runs.</span></span> <span data-ttu-id="ee805-213">コードを囲みしていない場合`if(!IsPost)`、かどうか、ページを起動すると、初めてたびに実行またはボタンへの応答で次のようにクリックします。 これは。</span><span class="sxs-lookup"><span data-stu-id="ee805-213">If you didn't enclose the code in `if(!IsPost)`, it would run every time the page is invoked, whether the first time or in response to a button click.</span></span>

<span data-ttu-id="ee805-214">コードが含まれていますが、`else`この時間をブロックします。</span><span class="sxs-lookup"><span data-stu-id="ee805-214">Notice that the code includes an `else` block this time.</span></span> <span data-ttu-id="ee805-215">紹介と言った`if`もテストする条件が true でない場合は、代替のコードを実行するブロック。</span><span class="sxs-lookup"><span data-stu-id="ee805-215">As we said when we introduced `if` blocks, sometimes you want to run alternative code if the condition you're testing isn't true.</span></span> <span data-ttu-id="ee805-216">ここです。</span><span class="sxs-lookup"><span data-stu-id="ee805-216">That's the case here.</span></span> <span data-ttu-id="ee805-217">(ページに渡された ID に問題はありません) の場合は、条件が成功した場合は、データベースから行を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="ee805-217">If the condition passes (that is, if the ID passed to the page is ok), you read a row from the database.</span></span> <span data-ttu-id="ee805-218">ただし、条件に合格しなかった場合、`else`ブロックが実行され、コードは、エラー メッセージを設定します。</span><span class="sxs-lookup"><span data-stu-id="ee805-218">However, if the condition doesn't pass, the `else` block runs and the code sets an error message.</span></span>

## <a name="validating-a-value-passed-to-the-page"></a><span data-ttu-id="ee805-219">ページに渡される値を検証します。</span><span class="sxs-lookup"><span data-stu-id="ee805-219">Validating a Value Passed to the Page</span></span>

<span data-ttu-id="ee805-220">コードを使用して`Request.QueryString["id"]`ページに渡される ID を取得します。</span><span class="sxs-lookup"><span data-stu-id="ee805-220">The code uses `Request.QueryString["id"]` to get the ID that's passed to the page.</span></span> <span data-ttu-id="ee805-221">コードは、ID の値が渡された実際に確認してください。</span><span class="sxs-lookup"><span data-stu-id="ee805-221">The code makes sure that a value was actually passed for the ID.</span></span> <span data-ttu-id="ee805-222">値が渡されなかった場合、コードは、検証エラーを設定します。</span><span class="sxs-lookup"><span data-stu-id="ee805-222">If no value was passed, the code sets a validation error.</span></span>

<span data-ttu-id="ee805-223">このコードは、情報を検証する別の方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="ee805-223">This code shows a different way to validate information.</span></span> <span data-ttu-id="ee805-224">前のチュートリアルで使用した、`Validation`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="ee805-224">In the previous tutorial, you worked with the `Validation` helper.</span></span> <span data-ttu-id="ee805-225">を検証するフィールドを登録して、ASP.NET は、自動的に検証をでしたし、を使用してエラーを表示`Html.ValidationMessage`と`Html.ValidationSummary`します。</span><span class="sxs-lookup"><span data-stu-id="ee805-225">You registered fields to validate, and ASP.NET automatically did the validation and displayed errors by using `Html.ValidationMessage` and `Html.ValidationSummary`.</span></span> <span data-ttu-id="ee805-226">この場合、ただし、実際には検証するユーザー入力します。</span><span class="sxs-lookup"><span data-stu-id="ee805-226">In this case, however, you're not really validating user input.</span></span> <span data-ttu-id="ee805-227">代わりに、別の場所から、ページに渡された値を検証しています。</span><span class="sxs-lookup"><span data-stu-id="ee805-227">Instead, you're validating a value that was passed to the page from elsewhere.</span></span> <span data-ttu-id="ee805-228">`Validation`をヘルパーではありません。</span><span class="sxs-lookup"><span data-stu-id="ee805-228">The `Validation` helper doesn't do that for you.</span></span>

<span data-ttu-id="ee805-229">そのため、値をチェックする、自分でテストすることで`if(!Request.QueryString["ID"].IsEmpty()`)。</span><span class="sxs-lookup"><span data-stu-id="ee805-229">Therefore, you check the value yourself, by testing it with `if(!Request.QueryString["ID"].IsEmpty()`).</span></span> <span data-ttu-id="ee805-230">使用して、エラーを表示するには問題がある場合`Html.ValidationSummary`で行ったように、`Validation`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="ee805-230">If there's a problem, you can display the error by using `Html.ValidationSummary`, as you did with the `Validation` helper.</span></span> <span data-ttu-id="ee805-231">そのために呼び出す`Validation.AddFormError`を表示するメッセージを渡します。</span><span class="sxs-lookup"><span data-stu-id="ee805-231">To do that, you call `Validation.AddFormError` and pass it a message to display.</span></span> <span data-ttu-id="ee805-232">`Validation.AddFormError` 組み込みメソッドを既に使い慣れている検証システム結び付いたカスタム メッセージを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="ee805-232">`Validation.AddFormError` is a built-in method that lets you define custom messages that tie in with the validation system you're already familiar with.</span></span> <span data-ttu-id="ee805-233">(このチュートリアルの後半で説明しますこの検証プロセスをより堅牢にする方法について。)</span><span class="sxs-lookup"><span data-stu-id="ee805-233">(Later in this tutorial we'll talk about how to make this validation process a little more robust.)</span></span>

<span data-ttu-id="ee805-234">映画の ID があることを確認したら、コードは、データベースは、1 つのデータベースのアイテムのみを検索してを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="ee805-234">After making sure that there's an ID for the movie, the code reads the database, looking for only a single database item.</span></span> <span data-ttu-id="ee805-235">(おそらくデータベース操作の一般的なパターンが気付き: データベースを開き、SQL ステートメントを定義して、ステートメントを実行します)。この時点では、SQL`Select`ステートメントが含まれる`WHERE ID = @0`します。</span><span class="sxs-lookup"><span data-stu-id="ee805-235">(You probably have noticed the general pattern for database operations: open the database, define a SQL statement, and run the statement.) This time, the SQL `Select` statement includes `WHERE ID = @0`.</span></span> <span data-ttu-id="ee805-236">ID は一意であり、1 つのレコードが返されます。</span><span class="sxs-lookup"><span data-stu-id="ee805-236">Because the ID is unique, only one record can be returned.</span></span>

<span data-ttu-id="ee805-237">使用して、クエリが実行される`db.QuerySingle`(いない`db.Query`ムービーの一覧を使用すると、)、コードに結果を格納して、`row`変数。</span><span class="sxs-lookup"><span data-stu-id="ee805-237">The query is performed by using `db.QuerySingle` (not `db.Query`, as you used for the movie listing), and the code puts the result into the `row` variable.</span></span> <span data-ttu-id="ee805-238">名前`row`は任意です。 どのような変数を名前ことができます。</span><span class="sxs-lookup"><span data-stu-id="ee805-238">The name `row` is arbitrary; you can name the variables anything you like.</span></span> <span data-ttu-id="ee805-239">上部にある初期化された変数は、これらの値は、テキスト ボックスに表示できるように、映画の詳細が入力されます。</span><span class="sxs-lookup"><span data-stu-id="ee805-239">The variables initialized at the top are then filled with the movie details so that these values can be displayed in the text boxes.</span></span>

## <a name="testing-the-edit-page-so-far"></a><span data-ttu-id="ee805-240">テストの編集 ページ (これまでに)</span><span class="sxs-lookup"><span data-stu-id="ee805-240">Testing the Edit Page (So Far)</span></span>

<span data-ttu-id="ee805-241">ページをテストしたい場合は、実行、*映画*ページし、をクリックして、**編集**任意のビデオの横にあるリンクです。</span><span class="sxs-lookup"><span data-stu-id="ee805-241">If you'd like to test your page, run the *Movies* page now and click an **Edit** link next to any movie.</span></span> <span data-ttu-id="ee805-242">表示されます、 *EditMovie*の詳細 ページで選択したムービーを記入します。</span><span class="sxs-lookup"><span data-stu-id="ee805-242">You'll see the *EditMovie* page with the details filled in for the movie you selected:</span></span>

![編集するムービーを表示するムービー ページを編集します。](updating-data/_static/image3.png)

<span data-ttu-id="ee805-244">ページの URL がようなものが含まれることに注意してください。 `?id=10` (またはその他の番号)。</span><span class="sxs-lookup"><span data-stu-id="ee805-244">Notice that the URL of the page includes something like `?id=10` (or some other number).</span></span> <span data-ttu-id="ee805-245">これまでにテストが終了する**編集**内のリンク、*ムービー*作業、ページが、クエリ文字列から ID を読み取って動作は、データベースが 1 つのムービーのレコードを取得するクエリを実行しているページします。</span><span class="sxs-lookup"><span data-stu-id="ee805-245">So far you've tested that **Edit** links in the *Movie* page work, that your page is reading the ID from the query string, and that the database query to get a single movie record is working.</span></span>

<span data-ttu-id="ee805-246">ムービーの情報を変更できますをクリックすると何も起こりません**変更の送信**します。</span><span class="sxs-lookup"><span data-stu-id="ee805-246">You can change the movie information, but nothing happens when you click **Submit Changes**.</span></span>

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a><span data-ttu-id="ee805-247">ユーザーの変更と、ムービーを更新するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="ee805-247">Adding Code to Update the Movie with the User's Changes</span></span>

<span data-ttu-id="ee805-248">*EditMovie.cshtml*ファイルで、(変更の保存)、2 番目の関数を実装するには、中かっこの内側は、次のコードを追加、`@`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="ee805-248">In the *EditMovie.cshtml* file, to implement the second function (saving changes), add the following code just inside the closing brace of the `@` block.</span></span> <span data-ttu-id="ee805-249">(コードを配置する正確な場所がわからない場合に進めるよう、[ムービーの編集 ページの完全なコード](#Complete_Page_Listing_for_EditMovie)このチュートリアルの最後に表示される)。</span><span class="sxs-lookup"><span data-stu-id="ee805-249">(If you're not sure exactly where to put the code, you can look at the [complete code listing for the Edit Movie page](#Complete_Page_Listing_for_EditMovie) that appears at the end of this tutorial.)</span></span>

[!code-csharp[Main](updating-data/samples/sample12.cs)]

<span data-ttu-id="ee805-250">このマークアップとコードが内のコードに似ていますが、もう一度*AddMovie*します。</span><span class="sxs-lookup"><span data-stu-id="ee805-250">Again, this markup and code is similar to the code in *AddMovie*.</span></span> <span data-ttu-id="ee805-251">コードは、`if(IsPost)`ブロック、ユーザーがクリックした場合にのみ、このコードが実行されるので、**変更を送信**ボタン&mdash;ときに、場合にのみ)、フォームがポストされました。</span><span class="sxs-lookup"><span data-stu-id="ee805-251">The code is in an `if(IsPost)` block, because this code runs only when the user clicks the **Submit Changes** button &mdash; that is, when (and only when) the form has been posted.</span></span> <span data-ttu-id="ee805-252">この場合、使用していないようなテスト`if(IsPost && Validation.IsValid())`: を使用して両方のテストをいない結合します。</span><span class="sxs-lookup"><span data-stu-id="ee805-252">In this case, you're not using a test like `if(IsPost && Validation.IsValid())`— that is, you're not combining both tests by using AND.</span></span> <span data-ttu-id="ee805-253">このページでは、まず決定フォームの送信があるかどうか (`if(IsPost)`)、しかし、フィールドの検証を登録します。</span><span class="sxs-lookup"><span data-stu-id="ee805-253">In this page, you first determine whether there's a form submission (`if(IsPost)`), and only then register the fields for validation.</span></span> <span data-ttu-id="ee805-254">検証結果をテストできます (`if(Validation.IsValid()`)。</span><span class="sxs-lookup"><span data-stu-id="ee805-254">Then you can test the validation results (`if(Validation.IsValid()`).</span></span> <span data-ttu-id="ee805-255">フローがでよりも若干異なりますが、 *AddMovie.cshtml*  ページが、効果は同じです。</span><span class="sxs-lookup"><span data-stu-id="ee805-255">The flow is slightly different than in the *AddMovie.cshtml* page, but the effect is the same.</span></span>

<span data-ttu-id="ee805-256">使用して、テキスト ボックスの値を取得する`Request.Form["title"]`と、その他の同様のコード`<input>`要素。</span><span class="sxs-lookup"><span data-stu-id="ee805-256">You get the values of the text boxes by using `Request.Form["title"]` and similar code for the other `<input>` elements.</span></span> <span data-ttu-id="ee805-257">今回は、コードを取得する映画 ID 非表示フィールドからに注目してください (`<input type="hidden">`)。</span><span class="sxs-lookup"><span data-stu-id="ee805-257">Notice that this time, the code gets the movie ID out of the hidden field (`<input type="hidden">`).</span></span> <span data-ttu-id="ee805-258">ときに、ページでは、最初に実行、コードは、クエリ文字列から ID を受け取りました。</span><span class="sxs-lookup"><span data-stu-id="ee805-258">When the page ran the first time, the code got the ID out of the query string.</span></span> <span data-ttu-id="ee805-259">クエリ文字列は以来何らかの方法で変更された場合に、表示されたムービーの ID を取得しているかどうかを確認する非表示フィールドから値を取得します。</span><span class="sxs-lookup"><span data-stu-id="ee805-259">You get the value from the hidden field to make sure that you're getting the ID of the movie that was originally displayed, in case the query string was somehow altered since then.</span></span>

<span data-ttu-id="ee805-260">非常に重要な違い、 *AddMovie*コードとこのコードは、このコードでは、SQL を使用することを`Update`ステートメントの代わりに、`Insert Into`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="ee805-260">The really important difference between the *AddMovie* code and this code is that in this code you use the SQL `Update` statement instead of the `Insert Into` statement.</span></span> <span data-ttu-id="ee805-261">次の例は、SQL の構文を示します`Update`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="ee805-261">The following example shows the syntax of the SQL `Update` statement:</span></span>

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

<span data-ttu-id="ee805-262">任意の順序ですべての列を指定して、必ずしもでは、中にすべての列を更新する必要はありません、`Update`操作。</span><span class="sxs-lookup"><span data-stu-id="ee805-262">You can specify any columns in any order, and you don't necessarily have to update every column during an `Update` operation.</span></span> <span data-ttu-id="ee805-263">(、新しいレコードとレコードを保存するは有効にしての許可されていないため、ID 自体を更新することはできません、`Update`操作します)。</span><span class="sxs-lookup"><span data-stu-id="ee805-263">(You cannot update the ID itself, because that would in effect save the record as a new record, and that's not allowed for an `Update` operation.)</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ee805-264">**重要な**、 `Where` id 句は非常に重要ではデータベースがデータベースを知っている方法であるためですを更新するレコード。</span><span class="sxs-lookup"><span data-stu-id="ee805-264">**Important** The `Where` clause with the ID is very important, because that's how the database knows which database record you want to update.</span></span> <span data-ttu-id="ee805-265">のままにする場合、`Where`句では、データベースを更新*すべて*データベース内のレコード。</span><span class="sxs-lookup"><span data-stu-id="ee805-265">If you left off the `Where` clause, the database would update *every* record in the database.</span></span> <span data-ttu-id="ee805-266">ほとんどの場合、障害になります。</span><span class="sxs-lookup"><span data-stu-id="ee805-266">In most cases, that would be a disaster.</span></span>


<span data-ttu-id="ee805-267">コードでは、更新する値は、プレース ホルダーを使用して、SQL ステートメントに渡されます。</span><span class="sxs-lookup"><span data-stu-id="ee805-267">In the code, the values to update are passed to the SQL statement by using placeholders.</span></span> <span data-ttu-id="ee805-268">前に付いているものを繰り返す: セキュリティ上の理由から、*のみ*SQL ステートメントに値を渡すプレース ホルダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="ee805-268">To repeat what we've said before: for security reasons, *only* use placeholders to pass values to a SQL statement.</span></span>

<span data-ttu-id="ee805-269">コードを使用した後`db.Execute`を実行する、`Update`変更内容を表示、一覧のページに戻るステートメントにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="ee805-269">After the code uses `db.Execute` to run the `Update` statement, it redirects back to the listing page, where you can see the changes.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="ee805-270">**別の SQL ステートメントでは、さまざまな方法**</span><span class="sxs-lookup"><span data-stu-id="ee805-270">**Different SQL Statements, Different Methods**</span></span>
> 
> <span data-ttu-id="ee805-271">お気付きかもしれません方法は少し異なりますを使用して、別の SQL ステートメントを実行することです。</span><span class="sxs-lookup"><span data-stu-id="ee805-271">You might have noticed that you use slightly different methods to run different SQL statements.</span></span> <span data-ttu-id="ee805-272">実行する、`Select`複数のレコード可能性のあることを返します。 クエリで使用する、`Query`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ee805-272">To run a `Select` query that potentially returns multiple records, you use the `Query` method.</span></span> <span data-ttu-id="ee805-273">実行する、`Select`がわかっているクエリが 1 つだけデータベースのアイテムを返すを使用する、`QuerySingle`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ee805-273">To run a `Select` query that you know will return only one database item, you use the `QuerySingle` method.</span></span> <span data-ttu-id="ee805-274">使用する変更をデータベースのアイテムを返さないコマンドを実行する、`Execute`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ee805-274">To run commands that make changes but that don't return database items, you use the `Execute` method.</span></span>
> 
> <span data-ttu-id="ee805-275">間の差で既に説明したようにさまざまな方法を異なる結果が返されるそれぞれのために存在する必要が`Query`と`QuerySingle`します。</span><span class="sxs-lookup"><span data-stu-id="ee805-275">You have to have different methods because each of them returns different results, as you saw already in the difference between `Query` and `QuerySingle`.</span></span> <span data-ttu-id="ee805-276">(、`Execute`メソッドで値も返します実際に&mdash;コマンドによって影響を受けた行の数、&mdash;するした無視していましたがこれまでは)。</span><span class="sxs-lookup"><span data-stu-id="ee805-276">(The `Execute` method actually returns a value also &mdash; namely, the number of database rows that were affected by the command &mdash; but you've been ignoring that so far.)</span></span>
> 
> <span data-ttu-id="ee805-277">もちろん、`Query`メソッドは、1 つだけデータベースの行を返す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ee805-277">Of course, the `Query` method might return only one database row.</span></span> <span data-ttu-id="ee805-278">ただし、ASP.NET で常の結果が処理、`Query`コレクションとしてのメソッド。</span><span class="sxs-lookup"><span data-stu-id="ee805-278">However, ASP.NET always treats the results of the `Query` method as a collection.</span></span> <span data-ttu-id="ee805-279">メソッドに 1 行だけが返される場合でも、その 1 つの行をコレクションから抽出する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ee805-279">Even if the method returns just one row, you have to extract that single row from the collection.</span></span> <span data-ttu-id="ee805-280">状況ではそのため、場所を*知る*1 行のみが返されます、これは、ビットを使用する方が便利`QuerySingle`します。</span><span class="sxs-lookup"><span data-stu-id="ee805-280">Therefore, in situations where you *know* you'll get back only one row, it's a bit more convenient to use `QuerySingle`.</span></span>
> 
> <span data-ttu-id="ee805-281">特定の種類のデータベース操作を実行するその他のいくつかの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="ee805-281">There are a few other methods that perform specific types of database operations.</span></span> <span data-ttu-id="ee805-282">データベースのメソッドの一覧を見つけることができます、 [ASP.NET Web Pages API のクイック リファレンス](../../api-reference/asp-net-web-pages-api-reference.md#Data)します。</span><span class="sxs-lookup"><span data-stu-id="ee805-282">You can find a listing of database methods in the [ASP.NET Web Pages API Quick Reference](../../api-reference/asp-net-web-pages-api-reference.md#Data).</span></span>


## <a name="making-validation-for-the-id-more-robust"></a><span data-ttu-id="ee805-283">信頼性の高い ID より検証を行う</span><span class="sxs-lookup"><span data-stu-id="ee805-283">Making Validation for the ID More Robust</span></span>

<span data-ttu-id="ee805-284">初めてページが実行されるように取得する映画 ID、クエリ文字列から、データベースからそのムービーを取得するを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ee805-284">The first time that the page runs, you get the movie ID from the query string so that you can go get that movie from the database.</span></span> <span data-ttu-id="ee805-285">行ったは実際にあったことを確認します。 このコードを使用して行った、値を確認します。</span><span class="sxs-lookup"><span data-stu-id="ee805-285">You made sure that there actually was a value to go look for, which you did by using this code:</span></span>

[!code-csharp[Main](updating-data/samples/sample13.cs)]

<span data-ttu-id="ee805-286">このコードを使用したことを確認するユーザーを取得する場合、 *EditMovies*  ページでムービーを選択せず、*映画* ページで、わかりやすいエラー メッセージが表示されるページ。</span><span class="sxs-lookup"><span data-stu-id="ee805-286">You used this code to make sure that if a user gets to the *EditMovies* page without first selecting a movie in the *Movies* page, the page would display a user-friendly error message.</span></span> <span data-ttu-id="ee805-287">(それ以外の場合、ユーザーはエラーが表示と混同する多くの場合。)</span><span class="sxs-lookup"><span data-stu-id="ee805-287">(Otherwise, users would see an error that would probably just confuse them.)</span></span>

<span data-ttu-id="ee805-288">ただし、この検証は非常に堅牢なはありません。</span><span class="sxs-lookup"><span data-stu-id="ee805-288">However, this validation isn't very robust.</span></span> <span data-ttu-id="ee805-289">ページは、これらのエラーと共に呼び出すことも可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ee805-289">The page might also be invoked with these errors:</span></span>

- <span data-ttu-id="ee805-290">ID では、数値はありません。</span><span class="sxs-lookup"><span data-stu-id="ee805-290">The ID isn't a number.</span></span> <span data-ttu-id="ee805-291">たとえばのような URL を使用して、ページを呼び出すことができます`http://localhost:nnnnn/EditMovie?id=abc`します。</span><span class="sxs-lookup"><span data-stu-id="ee805-291">For example, the page could be invoked with a URL like `http://localhost:nnnnn/EditMovie?id=abc`.</span></span>
- <span data-ttu-id="ee805-292">ID は、の数値が存在しないムービーを参照して (たとえば、 `http://localhost:nnnnn/EditMovie?id=100934`)。</span><span class="sxs-lookup"><span data-stu-id="ee805-292">The ID is a number, but it references a movie that doesn't exist (for example, `http://localhost:nnnnn/EditMovie?id=100934`).</span></span>

<span data-ttu-id="ee805-293">これらの Url は、実行に起因するエラーが発生する関心がある場合、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="ee805-293">If you're curious to see the errors that result from these URLs, run the *Movies* page.</span></span> <span data-ttu-id="ee805-294">ビデオの編集を選択しの URL を変更、 *EditMovie*英字を含む URL にページ ID で、または存在しない映画の ID。</span><span class="sxs-lookup"><span data-stu-id="ee805-294">Select a movie to edit, and then change the URL of the *EditMovie* page to a URL that contains an alphabetic ID or the ID of a non-existent movie.</span></span>

<span data-ttu-id="ee805-295">これは何か。</span><span class="sxs-lookup"><span data-stu-id="ee805-295">So what should you do?</span></span> <span data-ttu-id="ee805-296">最初の修正プログラムでは、ことだけでなく、ID に渡されるページが、ID が整数であるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="ee805-296">The first fix is to make sure that not only is an ID passed to the page, but that the ID is an integer.</span></span> <span data-ttu-id="ee805-297">コードを変更、`!IsPost`テストをこの例のようになります。</span><span class="sxs-lookup"><span data-stu-id="ee805-297">Change the code for the `!IsPost` test to look like this example:</span></span>

[!code-csharp[Main](updating-data/samples/sample14.cs)]

<span data-ttu-id="ee805-298">2 番目の条件を追加した、`IsEmpty`にリンクされているテスト`&&`(論理 AND)。</span><span class="sxs-lookup"><span data-stu-id="ee805-298">You've added a second condition to the `IsEmpty` test, linked with `&&` (logical AND):</span></span>

[!code-csharp[Main](updating-data/samples/sample15.cs)]

<span data-ttu-id="ee805-299">わかる場合があります、 [ASP.NET Web Pages のプログラミングの概要](../introducing-razor-syntax-c.md)などのメソッドを示すチュートリアル`AsBool`、`AsInt`文字の文字列を他のデータ型に変換します。</span><span class="sxs-lookup"><span data-stu-id="ee805-299">You might remember from the [Introduction to ASP.NET Web Pages Programming](../introducing-razor-syntax-c.md) tutorial that methods like `AsBool` an `AsInt` convert a character string to some other data type.</span></span> <span data-ttu-id="ee805-300">`IsInt`メソッド (など、他のユーザーと`IsBool`と`IsDateTime`) と似ています。</span><span class="sxs-lookup"><span data-stu-id="ee805-300">The `IsInt` method (and others, like `IsBool` and `IsDateTime`) are similar.</span></span> <span data-ttu-id="ee805-301">テストでのみ、かどうかを*できます*実際には、変換を実行せず、文字列に変換します。</span><span class="sxs-lookup"><span data-stu-id="ee805-301">However, they test only whether you *can* convert the string, without actually performing the conversion.</span></span> <span data-ttu-id="ee805-302">基本的に指示しているので、ここでする*クエリ文字列の値を整数に変換できる場合.*.</span><span class="sxs-lookup"><span data-stu-id="ee805-302">So here you're essentially saying *If the query string value can be converted to an integer ...*.</span></span>

<span data-ttu-id="ee805-303">その他の潜在的な問題は映画が存在しないを探しています。</span><span class="sxs-lookup"><span data-stu-id="ee805-303">The other potential problem is looking for a movie that doesn't exist.</span></span> <span data-ttu-id="ee805-304">映画を取得するコードは、このコードのようになります。</span><span class="sxs-lookup"><span data-stu-id="ee805-304">The code to get a movie looks like this code:</span></span>

[!code-csharp[Main](updating-data/samples/sample16.cs)]

<span data-ttu-id="ee805-305">渡す場合、`movieId`値を`QuerySingle`何も返されない実際のムービーに対応しないメソッドをおよび、後のステートメント (たとえば、 `title=row.Title`) エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ee805-305">If you pass a `movieId` value to the `QuerySingle` method that doesn't correspond to an actual movie, nothing is returned and the statements that follow (for example, `title=row.Title`) result in errors.</span></span>

<span data-ttu-id="ee805-306">もう一度が簡単に解決します。</span><span class="sxs-lookup"><span data-stu-id="ee805-306">Again there's an easy fix.</span></span> <span data-ttu-id="ee805-307">場合、`db.QuerySingle`メソッドには、結果は返されません、`row`変数は null になります。</span><span class="sxs-lookup"><span data-stu-id="ee805-307">If the `db.QuerySingle` method returns no results, the `row` variable will be null.</span></span> <span data-ttu-id="ee805-308">チェックできるようにするかどうか、`row`からその値を取得しようとする前に、変数が null です。</span><span class="sxs-lookup"><span data-stu-id="ee805-308">So you can check whether the `row` variable is null before you try to get values from it.</span></span> <span data-ttu-id="ee805-309">次のコードを追加、`if`の値を取得するステートメントの周囲、`row`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="ee805-309">The following code adds an `if` block around the statements that get the values out of the `row` object:</span></span>

[!code-csharp[Main](updating-data/samples/sample17.cs)]

<span data-ttu-id="ee805-310">これら 2 つの追加の検証テストより堅牢ながページになります。</span><span class="sxs-lookup"><span data-stu-id="ee805-310">With these two additional validation tests, the page becomes more bullet-proof.</span></span> <span data-ttu-id="ee805-311">完全なコード、`!IsPost`ブランチではこの例のようになります。</span><span class="sxs-lookup"><span data-stu-id="ee805-311">The complete code for the `!IsPost` branch now looks like this example:</span></span>

[!code-csharp[Main](updating-data/samples/sample18.cs)]

<span data-ttu-id="ee805-312">私たちことに注意もう一度このタスクの良い使用、`else`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="ee805-312">We'll note once more that this task is a good use for an `else` block.</span></span> <span data-ttu-id="ee805-313">テストが合格しなかった場合、`else`ブロックがエラー メッセージを設定します。</span><span class="sxs-lookup"><span data-stu-id="ee805-313">If the tests don't pass, the `else` blocks set error messages.</span></span>

## <a name="adding-a-link-to-return-to-the-movies-page"></a><span data-ttu-id="ee805-314">ムービー ページに戻るためのリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="ee805-314">Adding a Link to Return to the Movies Page</span></span>

<span data-ttu-id="ee805-315">最終で役立つ詳細は、リンクを追加するに戻す、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="ee805-315">A final and helpful detail is to add a link back to the *Movies* page.</span></span> <span data-ttu-id="ee805-316">イベントの通常フローでユーザーが始まり、*映画*ページし、をクリックして、**編集**リンク。</span><span class="sxs-lookup"><span data-stu-id="ee805-316">In the ordinary flow of events, users will start at the *Movies* page and click an **Edit** link.</span></span> <span data-ttu-id="ee805-317">それらになります、 *EditMovie*  ページで、ムービーを編集し、ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee805-317">That brings them to the *EditMovie* page, where they can edit the movie and click the button.</span></span> <span data-ttu-id="ee805-318">コードによって変更が処理した後にリダイレクト、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="ee805-318">After the code processes the change, it redirects back to the *Movies* page.</span></span>

<span data-ttu-id="ee805-319">ただし、</span><span class="sxs-lookup"><span data-stu-id="ee805-319">However:</span></span>

- <span data-ttu-id="ee805-320">ユーザーは何も変更しないように指定します。</span><span class="sxs-lookup"><span data-stu-id="ee805-320">The user might decide not to change anything.</span></span>
- <span data-ttu-id="ee805-321">クリックしてせず、このページにユーザーを受けている可能性があります、**編集**のリンクを*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="ee805-321">The user might have gotten to this page without first clicking an **Edit** link in the *Movies* page.</span></span>

<span data-ttu-id="ee805-322">どちらにしても、メインの一覧を返すことを簡単にします。</span><span class="sxs-lookup"><span data-stu-id="ee805-322">Either way, you want to make it easy for them to return to the main listing.</span></span> <span data-ttu-id="ee805-323">簡単な修正は&mdash;終了した直後に次のマークアップを追加`</form>`マークアップ タグ。</span><span class="sxs-lookup"><span data-stu-id="ee805-323">It's an easy fix &mdash; add the following markup just after the closing `</form>` tag in the markup:</span></span>

[!code-html[Main](updating-data/samples/sample19.html)]

<span data-ttu-id="ee805-324">このマークアップのと同じ構文を使用して、`<a>`他の場所で見た要素。</span><span class="sxs-lookup"><span data-stu-id="ee805-324">This markup uses the same syntax for an `<a>` element that you've seen elsewhere.</span></span> <span data-ttu-id="ee805-325">URL が含まれます`~`「web サイトのルートです」を意味するには。</span><span class="sxs-lookup"><span data-stu-id="ee805-325">The URL includes `~` to mean "root of the website."</span></span>

## <a name="testing-the-movie-update-process"></a><span data-ttu-id="ee805-326">ムービーの更新プロセスのテスト</span><span class="sxs-lookup"><span data-stu-id="ee805-326">Testing the Movie Update Process</span></span>

<span data-ttu-id="ee805-327">今すぐテストすることができます。</span><span class="sxs-lookup"><span data-stu-id="ee805-327">Now you can test.</span></span> <span data-ttu-id="ee805-328">実行、*映画*ページ、およびクリックして**編集**ムービーの横にあります。</span><span class="sxs-lookup"><span data-stu-id="ee805-328">Run the *Movies* page, and click **Edit** next to a movie.</span></span> <span data-ttu-id="ee805-329">ときに、 *EditMovie*ムービー をクリックして変更を加えるページが表示されたら、**変更の送信**します。</span><span class="sxs-lookup"><span data-stu-id="ee805-329">When the *EditMovie* page appears, make changes to the movie and click **Submit Changes**.</span></span> <span data-ttu-id="ee805-330">ムービーの一覧が表示されたら、変更内容が表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ee805-330">When the movie listing appears, make sure that your changes are shown.</span></span>

<span data-ttu-id="ee805-331">検証が動作していることを確認するのにはクリックして**編集**もう 1 つのムービーの。</span><span class="sxs-lookup"><span data-stu-id="ee805-331">To make sure that validation is working, click **Edit** for another movie.</span></span> <span data-ttu-id="ee805-332">取得するときに、 *EditMovie*  ページで、クリア、**ジャンル**フィールド (または**年**フィールド、またはその両方) の変更を送信しようとします。</span><span class="sxs-lookup"><span data-stu-id="ee805-332">When you get to the *EditMovie* page, clear the **Genre** field (or **Year** field, or both) and try to submit your changes.</span></span> <span data-ttu-id="ee805-333">想像どおり、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee805-333">You'll see an error, as you'd expect:</span></span>

![検証エラーを示すビデオ ページを編集します。](updating-data/_static/image4.png)

<span data-ttu-id="ee805-335">をクリックして、**ムービーの一覧に戻る**に戻って変更を中止へのリンク、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="ee805-335">Click the **Return to movie listing** link to abandon your changes and return to the *Movies* page.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="ee805-336">次回について</span><span class="sxs-lookup"><span data-stu-id="ee805-336">Coming Up Next</span></span>

<span data-ttu-id="ee805-337">次のチュートリアルでは、ムービーのレコードを削除する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="ee805-337">In the next tutorial, you'll see how to delete a movie record.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a><span data-ttu-id="ee805-338">(リンクの編集と更新) ムービー ページ全体を示します</span><span class="sxs-lookup"><span data-stu-id="ee805-338">Complete Listing for Movie Page (Updated with Edit Links)</span></span>

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a><span data-ttu-id="ee805-339">ムービーの編集 ページの一覧のページを完了します。</span><span class="sxs-lookup"><span data-stu-id="ee805-339">Complete Page Listing for Edit Movie Page</span></span>

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="ee805-340">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="ee805-340">Additional Resources</span></span>

- [<span data-ttu-id="ee805-341">Razor 構文を使用して ASP.NET Web プログラミングの概要</span><span class="sxs-lookup"><span data-stu-id="ee805-341">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../../getting-started/introducing-razor-syntax-c.md)
- <span data-ttu-id="ee805-342">[SQL UPDATE ステートメント](http://www.w3schools.com/sql/sql_update.asp)W3Schools サイト</span><span class="sxs-lookup"><span data-stu-id="ee805-342">[SQL UPDATE Statement](http://www.w3schools.com/sql/sql_update.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ee805-343">[前へ](entering-data.md)
> [次へ](deleting-data.md)</span><span class="sxs-lookup"><span data-stu-id="ee805-343">[Previous](entering-data.md)
[Next](deleting-data.md)</span></span>
