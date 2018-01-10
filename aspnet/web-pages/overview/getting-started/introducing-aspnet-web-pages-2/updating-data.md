---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: "ASP.NET Web Pages の概要 - データベースのデータの更新 |Microsoft ドキュメント"
author: tfitzmac
description: "このチュートリアルでは、ASP.NET Web Pages (Razor) を使用する場合は、(変更)、既存のデータベース エントリを更新する方法を示します。 系列を完了すると想定 th しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: b016231975bf8d359f4c390b0b478edc383117d4
ms.sourcegitcommit: df2157ae9aeea0075772719c29784425c783e82a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2018
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a><span data-ttu-id="8ca18-104">ASP.NET Web ページの概要 - データベースのデータの更新</span><span class="sxs-lookup"><span data-stu-id="8ca18-104">Introducing ASP.NET Web Pages - Updating Database Data</span></span>
====================
<span data-ttu-id="8ca18-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8ca18-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8ca18-106">このチュートリアルでは、ASP.NET Web Pages (Razor) を使用する場合は、(変更)、既存のデータベース エントリを更新する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-106">This tutorial shows you how to update (change) an existing database entry when you use ASP.NET Web Pages (Razor).</span></span> <span data-ttu-id="8ca18-107">を通じてシリーズを完了すると想定[データの入力を使用してフォームを使用して ASP.NET Web Pages](entering-data.md)です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-107">It assumes you have completed the series through [Entering Data by Using Forms Using ASP.NET Web Pages](entering-data.md).</span></span>
> 
> <span data-ttu-id="8ca18-108">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="8ca18-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="8ca18-109">個々 のレコードを選択する方法、`WebGrid`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="8ca18-109">How to select an individual record in the `WebGrid` helper.</span></span>
> - <span data-ttu-id="8ca18-110">データベースから 1 つのレコードを読み取る方法です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-110">How to read a single record from a database.</span></span>
> - <span data-ttu-id="8ca18-111">データベース レコードの値を持つフォームをプリロードする方法です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-111">How to preload a form with values from the database record.</span></span>
> - <span data-ttu-id="8ca18-112">データベースの既存のレコードを更新する方法です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-112">How to update an existing record in a database.</span></span>
> - <span data-ttu-id="8ca18-113">表示する ページで情報を格納する方法。</span><span class="sxs-lookup"><span data-stu-id="8ca18-113">How to store information in the page without displaying it.</span></span>
> - <span data-ttu-id="8ca18-114">隠しフィールドを使用して情報を格納する方法です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-114">How to use a hidden field to store information.</span></span>
>   
> 
> <span data-ttu-id="8ca18-115">説明されている機能/テクノロジ:</span><span class="sxs-lookup"><span data-stu-id="8ca18-115">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="8ca18-116">`WebGrid`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="8ca18-116">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="8ca18-117">SQL`Update`コマンド。</span><span class="sxs-lookup"><span data-stu-id="8ca18-117">The SQL `Update` command.</span></span>
> - <span data-ttu-id="8ca18-118">`Database.Execute` メソッド。</span><span class="sxs-lookup"><span data-stu-id="8ca18-118">The `Database.Execute` method.</span></span>
> - <span data-ttu-id="8ca18-119">フィールドを非表示 (`<input type="hidden">`)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-119">Hidden fields (`<input type="hidden">`).</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="8ca18-120">新機能のビルドします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-120">What You'll Build</span></span>

<span data-ttu-id="8ca18-121">前のチュートリアルでは、データベースにレコードを追加する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="8ca18-121">In the previous tutorial, you learned how to add a record to a database.</span></span> <span data-ttu-id="8ca18-122">ここでは、編集するためのレコードを表示する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-122">Here, you'll learn how to display a record for editing.</span></span> <span data-ttu-id="8ca18-123">*映画*を更新します ページで、`WebGrid`ヘルパーを表示できるように、**編集**各ムービーの横にあるリンク。</span><span class="sxs-lookup"><span data-stu-id="8ca18-123">In the *Movies* page, you'll update the `WebGrid` helper so that it displays an **Edit** link next to each movie:</span></span>

![各ムービーの [編集] リンクを含む WebGrid を表示します。](updating-data/_static/image1.png)

<span data-ttu-id="8ca18-125">クリックすると、**編集**リンクに移動する異なるページでは、ムービー情報が既にフォームで。</span><span class="sxs-lookup"><span data-stu-id="8ca18-125">When you click the **Edit** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![ムービーを編集することを示すビデオ ページを編集します。](updating-data/_static/image2.png)

<span data-ttu-id="8ca18-127">任意の値を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-127">You can change any of the values.</span></span> <span data-ttu-id="8ca18-128">変更を送信するときにページ内のコードはデータベースを更新し、ムービーの一覧に戻ることができます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-128">When you submit the changes, the code in the page updates the database and takes you back to the movie listing.</span></span>

<span data-ttu-id="8ca18-129">このプロセスの一部の動作と同様にほぼ、 *AddMovie.cshtml*のため、このチュートリアルの多くはなじみのある、前のチュートリアルで作成したページ。</span><span class="sxs-lookup"><span data-stu-id="8ca18-129">This part of the process works almost exactly like the *AddMovie.cshtml* page you created in the previous tutorial, so much of this tutorial will be familiar.</span></span>

<span data-ttu-id="8ca18-130">個々 のムービーを編集する方法を実装することがいくつかの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-130">There are several ways you could implement a way to edit an individual movie.</span></span> <span data-ttu-id="8ca18-131">アプローチには、簡単に実装し、わかりやすいになっているために選ばれました。</span><span class="sxs-lookup"><span data-stu-id="8ca18-131">The approach shown was chosen because it's easy to implement and easy to understand.</span></span>

## <a name="adding-an-edit-link-to-the-movie-listing"></a><span data-ttu-id="8ca18-132">ムービーの一覧を編集 リンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-132">Adding an Edit Link to the Movie Listing</span></span>

<span data-ttu-id="8ca18-133">更新を開始する、*映画*ページも一覧表示する各ムービーが含まれるように、**編集**リンクします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-133">To begin, you'll update the *Movies* page so that each movie listing also contains an **Edit** link.</span></span>

<span data-ttu-id="8ca18-134">開く、 *Movies.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8ca18-134">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="8ca18-135">ページの本文に次のように変更します。、`WebGrid`列を追加することでマークアップ。</span><span class="sxs-lookup"><span data-stu-id="8ca18-135">In the body of the page, change the `WebGrid` markup by adding a column.</span></span> <span data-ttu-id="8ca18-136">変更後のマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-136">Here's the modified markup:</span></span>

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

<span data-ttu-id="8ca18-137">新しい列では、この 1 つです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-137">The new column is this one:</span></span>

[!code-html[Main](updating-data/samples/sample2.html)]

<span data-ttu-id="8ca18-138">この列のポイントは、リンクを表示する (`<a>`要素)"Edit"と表示されました。</span><span class="sxs-lookup"><span data-stu-id="8ca18-138">The point of this column is to show a link (`<a>` element) whose text says "Edit".</span></span> <span data-ttu-id="8ca18-139">ページを実行すると、次のように検索するリンクを作成するは私たちの後で、`id`各ムービーのさまざまな値。</span><span class="sxs-lookup"><span data-stu-id="8ca18-139">What we're after is to create a link that looks like the following when the page runs, with the `id` value different for each movie:</span></span>

[!code-css[Main](updating-data/samples/sample3.css)]

<span data-ttu-id="8ca18-140">このリンクをという名前のページを呼び出す*EditMovie*、これは、クエリ文字列を渡すと`?id=7`そのページにします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-140">This link will invoke a page named *EditMovie*, and it will pass the query string `?id=7` to that page.</span></span>

<span data-ttu-id="8ca18-141">新しい列の構文は、少し複雑になりますが、いくつかの要素をまとめてそのためにのみです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-141">The syntax for the new column might look a bit complex, but that's only because it puts together several elements.</span></span> <span data-ttu-id="8ca18-142">各要素は簡単です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-142">Each individual element is straightforward.</span></span> <span data-ttu-id="8ca18-143">集中する場合だけ、`<a>`要素では、このマークアップを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8ca18-143">If you concentrate on just the `<a>` element, you see this markup:</span></span>

[!code-html[Main](updating-data/samples/sample4.html)]

<span data-ttu-id="8ca18-144">グリッドの動作に関するいくつかのバック グラウンド: グリッドには、各データベース レコードのいずれかの行が表示されます。 および、データベースのレコードに各フィールドの列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-144">Some background about how the grid works: the grid displays rows, one for each database record, and it displays columns for each field in the database record.</span></span> <span data-ttu-id="8ca18-145">グリッドの各行が構築されるときに、`item`オブジェクトには、その行のデータベースのレコード (アイテム) が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8ca18-145">While each grid row is being constructed, the `item` object contains the database record (item) for that row.</span></span> <span data-ttu-id="8ca18-146">これによって、その行のデータを取得するためのコードのことができます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-146">This arrangement gives you a way in code to get at the data for that row.</span></span> <span data-ttu-id="8ca18-147">ここに表示される: 式`item.ID`が現在のデータベース アイテムの ID 値を取得します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-147">That's what you see here: the expression `item.ID` is getting the ID value of the current database item.</span></span> <span data-ttu-id="8ca18-148">可能性があります (タイトル、ジャンル、または年) のデータベースの値のいずれかと同じ方法を使用して取得`item.Title`、 `item.Genre`、または`item.Year`です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-148">You could get any of the database values (title, genre, or year) the same way by using `item.Title`, `item.Genre`, or `item.Year`.</span></span>

<span data-ttu-id="8ca18-149">式`"~/EditMovie?id=@item.ID`ターゲット URL のハード コーディングされた部品を結合 (`~/EditMovie?id=`) 動的に派生この ID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-149">The expression `"~/EditMovie?id=@item.ID` combines the hard-coded part of the target URL (`~/EditMovie?id=`) with this dynamically derived ID.</span></span> <span data-ttu-id="8ca18-150">(説明しました、`~`演算子が前のチュートリアルでは、ASP.NET の操作を現在の web サイトのルートを表すです)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-150">(You saw the `~` operator in the previous tutorial; it's an ASP.NET operator that represents the current website root.)</span></span>

<span data-ttu-id="8ca18-151">結果は、列内のマークアップのこの部分だけが生成されること、次のマークアップのようなもの実行時に。</span><span class="sxs-lookup"><span data-stu-id="8ca18-151">The result is that this part of the markup in the column simply produces something like the following markup at run time:</span></span>

[!code-xml[Main](updating-data/samples/sample5.xml)]

<span data-ttu-id="8ca18-152">実際の値に必然的に、`id`行ごとに異なります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-152">Naturally, the actual value of `id` will be different for each row.</span></span>

## <a name="creating-a-custom-display-for-a-grid-column"></a><span data-ttu-id="8ca18-153">グリッド列のカスタムの表示を作成します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-153">Creating a Custom Display for a Grid Column</span></span>

<span data-ttu-id="8ca18-154">これで、グリッドの列にバックアップします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-154">Now back to the grid column.</span></span> <span data-ttu-id="8ca18-155">3 つの列最初にユーザーが、グリッドが表示されるデータ値のみ (タイトル、ジャンル、および year) です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-155">The three columns you originally had in the grid displayed only data values (title, genre, and year).</span></span> <span data-ttu-id="8ca18-156">この表示を指定されたデータベース列の名前を渡すことによって&mdash;など`grid.Column("Title")`です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-156">You specified this display by passing the name of the database column &mdash; for example, `grid.Column("Title")`.</span></span>

<span data-ttu-id="8ca18-157">この新しい**編集**リンク列は異なります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-157">This new **Edit** link column is different.</span></span> <span data-ttu-id="8ca18-158">列名を指定するには、代わりに渡す、`format`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="8ca18-158">Instead of specifying a column name, you're passing a `format` parameter.</span></span> <span data-ttu-id="8ca18-159">このパラメーターを使用して、マークアップを定義できますを`WebGrid`ヘルパーがと共に表示されます、`item`太字または緑として列のデータを表示したり、どのような形式を必要する値。</span><span class="sxs-lookup"><span data-stu-id="8ca18-159">This parameter lets you define markup that the `WebGrid` helper will render along with the `item` value to display the column data as bold or green or in whatever format that you want.</span></span> <span data-ttu-id="8ca18-160">たとえば、タイトルを太字で表示する場合は、この例のように列を作成できます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-160">For example, if you wanted the title to appear bold, you could create a column like this example:</span></span>

[!code-html[Main](updating-data/samples/sample6.html)]

<span data-ttu-id="8ca18-161">(さまざまな`@`中の文字、`format`プロパティは、マークアップとコード値間の遷移をマークします)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-161">(The various `@` characters you see in the `format` property mark the transition between markup and a code value.)</span></span>

<span data-ttu-id="8ca18-162">知っておくと、`format`プロパティは、理解しやすいだ方法、新しい**編集**リンク列が一緒に配置されます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-162">Once you know about the `format` property, it's easier to understand how the new **Edit** link column is put together:</span></span>

[!code-html[Main](updating-data/samples/sample7.html)]

<span data-ttu-id="8ca18-163">列で構成されます*のみ*のリンクを表示するマークアップをさらにいくつかの情報 (ID) をから抽出された行のデータベース レコードです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-163">The column consists *only* of the markup that renders the link, plus some information (the ID) that's extracted from the database record for the row.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="8ca18-164">**名前付きパラメーターとメソッドの位置指定パラメーター**</span><span class="sxs-lookup"><span data-stu-id="8ca18-164">**Named Parameters and Positional Parameters for a Method**</span></span>
> 
> <span data-ttu-id="8ca18-165">何度もしたメソッドを呼び出してに渡されるパラメーターするだけに表示されることコンマで区切られたパラメーター値。</span><span class="sxs-lookup"><span data-stu-id="8ca18-165">Many times when you've called a method and passed parameters to it, you've simply listed the parameter values separated by commas.</span></span> <span data-ttu-id="8ca18-166">ここでは例をいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-166">Here are a couple of examples:</span></span>
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> <span data-ttu-id="8ca18-167">このコードは、最初に説明しましたが、各ケースでは、特定の順序のメソッドにパラメーターを渡すしているときに、問題を触れません&mdash;パラメーターがそのメソッドで定義されている順序つまり、します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-167">We didn't mention the issue when you first saw this code, but in each case, you're passing parameters to the methods in a specific order &mdash; namely, the order in which the parameters are defined in that method.</span></span> <span data-ttu-id="8ca18-168">`db.Execute`と`Validation.RequireFields`、渡す値の順序を混在している場合は、ページを実行すると、エラー メッセージまたは少なくともいくつか予期しない結果を得られます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-168">For `db.Execute` and `Validation.RequireFields`, if you mixed up the order of the values you pass, you'd get an error message when the page runs, or at least some strange results.</span></span> <span data-ttu-id="8ca18-169">明確でパラメーターを渡す順序を知る必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-169">Clearly, you have to know the order to pass the parameters in.</span></span> <span data-ttu-id="8ca18-170">(WebMatrix、IntelliSense できますを見つけ出します名前、型、およびパラメーターの順序を学習する。)</span><span class="sxs-lookup"><span data-stu-id="8ca18-170">(In WebMatrix, IntelliSense can help you learn figure out the name, type, and order of the parameters.)</span></span>
> 
> <span data-ttu-id="8ca18-171">使用して順序の値を渡す代わりに、*名前付きパラメーター*です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-171">As an alternative to passing values in order, you can use *named parameters*.</span></span> <span data-ttu-id="8ca18-172">(を使用すると呼ばれる、順序でパラメーターを渡す*位置指定パラメーター*)。名前付きパラメーターは、明示的に名を含めて、パラメーターの値を渡すときにします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-172">(Passing parameters in order is known as using *positional parameters*.) For named parameters, you explicitly include the name of the parameter when passing its value.</span></span> <span data-ttu-id="8ca18-173">使用した名前付きパラメーター既に何度もこれらのチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-173">You've used named parameters already a number of times in these tutorials.</span></span> <span data-ttu-id="8ca18-174">例:</span><span class="sxs-lookup"><span data-stu-id="8ca18-174">For example:</span></span>
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> <span data-ttu-id="8ca18-175">と、呼び出し</span><span class="sxs-lookup"><span data-stu-id="8ca18-175">and</span></span>
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> <span data-ttu-id="8ca18-176">名前付きパラメーターは、メソッドは、多くのパラメーターを受け取る場合は特に、いくつかのような状況で便利です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-176">Named parameters are handy for a couple of situations, especially when a method takes many parameters.</span></span> <span data-ttu-id="8ca18-177">1 つは、1 つまたは 2 つのパラメーターを渡す必要に渡す値がパラメーター リスト内の最初の位置の間ではないです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-177">One is when you want to pass only one or two parameters, but the values you want to pass are not among the first positions in the parameter list.</span></span> <span data-ttu-id="8ca18-178">別の状況は、自分に最も適した順序でパラメーターを渡すことによって、コードを読みやすくする場合です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-178">Another situation is when you want to make your code more readable by passing the parameters in the order that makes the most sense to you.</span></span>
> 
> <span data-ttu-id="8ca18-179">当然ながら、名前付きパラメーターを使用するのにパラメーターの名前を認識する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-179">Obviously, to use named parameters, you have to know the names of the parameters.</span></span> <span data-ttu-id="8ca18-180">WebMatrix IntelliSense できます*表示*する、名前が、できません現在情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-180">WebMatrix IntelliSense can *show* you the names, but it cannot currently fill them in for you.</span></span>


## <a name="creating-the-edit-page"></a><span data-ttu-id="8ca18-181">[編集] ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-181">Creating the Edit Page</span></span>

<span data-ttu-id="8ca18-182">作成できるようになりました、 *EditMovie*ページ。</span><span class="sxs-lookup"><span data-stu-id="8ca18-182">Now you can create the *EditMovie* page.</span></span> <span data-ttu-id="8ca18-183">ユーザーがクリックして、**編集**、リンクすることになりますこのページにします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-183">When users click the **Edit** link, they'll end up on this page.</span></span>

<span data-ttu-id="8ca18-184">という名前のページを作成する*EditMovie.cshtml*が次のマークアップ ファイルを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-184">Create a page named *EditMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

<span data-ttu-id="8ca18-185">このマークアップおよびコードある場合に似ていますが、 *AddMovie*ページ。</span><span class="sxs-lookup"><span data-stu-id="8ca18-185">This markup and code is similar to what you have in the *AddMovie* page.</span></span> <span data-ttu-id="8ca18-186">[送信] ボタンのテキストで小さな違いがあります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-186">There's a small difference in the text for the submit button.</span></span> <span data-ttu-id="8ca18-187">同様、 *AddMovie*  ページでは、`Html.ValidationSummary`呼び出しをいずれかを使用する必要がある場合、検証エラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-187">As with the *AddMovie* page, there's an `Html.ValidationSummary` call that will display validation errors if there are any.</span></span> <span data-ttu-id="8ca18-188">この時点への呼び出しを除外しているお`Validation.Message`検証概要にエラーが表示されるため、します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-188">This time we're leaving out calls to `Validation.Message`, since errors will be displayed in the validation summary.</span></span> <span data-ttu-id="8ca18-189">述べたように前のチュートリアルでは、検証の概要と、個々 のエラー メッセージをさまざまな組み合わせで使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-189">As noted in the previous tutorial, you can use the validation summary and the individual error messages in various combinations.</span></span>

<span data-ttu-id="8ca18-190">もう一度ことに注意して、`method`の属性、`<form>`要素に設定されている`post`です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-190">Notice again that the `method` attribute of the `<form>` element is set to `post`.</span></span> <span data-ttu-id="8ca18-191">同様、 *AddMovie.cshtml*  ページで、このページでは、変更をデータベースにします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-191">As with the *AddMovie.cshtml* page, this page makes changes to the database.</span></span> <span data-ttu-id="8ca18-192">そのため、このフォームを実行する必要があります、`POST`操作します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-192">Therefore, this form should perform a `POST` operation.</span></span> <span data-ttu-id="8ca18-193">(の違いの詳細について`GET`と`POST`、操作を参照してください、 [GET、POST、および HTTP 動詞の安全性](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety)HTML フォームのチュートリアルではサイドバーです)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-193">(For more about the difference between `GET` and `POST` operations, see the [GET, POST, and HTTP Verb Safety](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) sidebar in the tutorial on HTML forms.)</span></span>

<span data-ttu-id="8ca18-194">以前のチュートリアルで説明したとおり、`value`テキスト ボックスの属性はそれらをプリロードするために Razor コードで設定されています。</span><span class="sxs-lookup"><span data-stu-id="8ca18-194">As you saw in an earlier tutorial, the `value` attributes of the text boxes are being set with Razor code in order to preload them.</span></span> <span data-ttu-id="8ca18-195">このとき、ただし、使用しているような変数`title`と`genre`の代わりにそのタスクの`Request.Form["title"]`:</span><span class="sxs-lookup"><span data-stu-id="8ca18-195">This time, though, you're using variables like `title` and `genre` for that task instead of `Request.Form["title"]`:</span></span>

`<input type="text" name="title" value="@title" />`

<span data-ttu-id="8ca18-196">としてする前に、このマークアップが事前に読み込むビデオの値を持つテキスト ボックスの値。</span><span class="sxs-lookup"><span data-stu-id="8ca18-196">As before, this markup will preload the text box values with the movie values.</span></span> <span data-ttu-id="8ca18-197">変数を使用するこの時間を使用せずに便利な理由はすぐに表示されます、`Request`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="8ca18-197">You'll see in a moment why it's handy to use variables this time instead of using the `Request` object.</span></span>

<span data-ttu-id="8ca18-198">`<input type="hidden">`のこのページの要素。</span><span class="sxs-lookup"><span data-stu-id="8ca18-198">There's also a `<input type="hidden">` element on this page.</span></span> <span data-ttu-id="8ca18-199">この要素は、ページに表示させず、ムービーの ID を格納します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-199">This element stores the movie ID without making it visible on the page.</span></span> <span data-ttu-id="8ca18-200">クエリ文字列の値を使用して、ページに、ID が渡される最初に (`?id=7`または URL に類似)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-200">The ID is initially passed to the page by using a query string value (`?id=7` or similar in the URL).</span></span> <span data-ttu-id="8ca18-201">非表示のフィールドに ID 値を配置することで行うことができますを使用すること、フォームが送信されるときにページが呼び出された元の URL へのアクセスがある不要になった場合でもです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-201">By putting the ID value into a hidden field, you can make sure that it's available when the form is submitted, even if you no longer have access to the original URL that the page was invoked with.</span></span>

<span data-ttu-id="8ca18-202">異なり、 *AddMovie*ページで、コードを*EditMovie*ページが 2 つの異なる関数を持っています。</span><span class="sxs-lookup"><span data-stu-id="8ca18-202">Unlike the *AddMovie* page, the code for the *EditMovie* page has two distinct functions.</span></span> <span data-ttu-id="8ca18-203">最初の関数となるは、ページが最初に表示されます (および*のみ*し)、コードは、クエリ文字列からムービー ID を取得します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-203">The first function is that when the page is displayed for the first time (and *only* then), the code gets the movie ID from the query string.</span></span> <span data-ttu-id="8ca18-204">次に、ID を使用して、データベースから対応するムービーを読み取り、表示 (プリロード) に、テキスト ボックスになります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-204">The code then uses the ID to read the corresponding movie out of the database and display (preload) it in the text boxes.</span></span>

<span data-ttu-id="8ca18-205">2 番目の関数となるは、ユーザーがクリックした、**変更を送信**ボタン、コードが、テキスト ボックスの値を読み取るし、それを検証します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-205">The second function is that when the user clicks the **Submit Changes** button, the code has to read the values of the text boxes and validate them.</span></span> <span data-ttu-id="8ca18-206">コードは、新しい値を持つデータベース アイテムの更新にもあります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-206">The code also has to update the database item with the new values.</span></span> <span data-ttu-id="8ca18-207">説明したとおり、この手法は、レコードを追加するよう*AddMovie*です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-207">This technique is similar to adding a record, as you saw in *AddMovie*.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="8ca18-208">1 つのムービーの読み取りにコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-208">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="8ca18-209">最初の関数を実行するには、ページの最上位にこのコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-209">To perform the first function, add this code to the top of the page:</span></span>

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

<span data-ttu-id="8ca18-210">このコードのほとんどが始まるブロックの内側は`if(!IsPost)`します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-210">Most of this code is inside a block that starts `if(!IsPost)`.</span></span> <span data-ttu-id="8ca18-211">`!`ため、式の意味に、演算子が"not"を意味*場合、この要求が post 送信*、ことわざの間接的な手段である*かどうか、この要求はこのページが実行されて初めて*です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-211">The `!` operator means "not," so the expression means *if this request is not a post submission*, which is an indirect way of saying *if this request is the first time that this page has been run*.</span></span> <span data-ttu-id="8ca18-212">前述のように、このコードを実行する必要があります*のみ*初めてページを実行します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-212">As noted earlier, this code should run *only* the first time the page runs.</span></span> <span data-ttu-id="8ca18-213">内のコードを囲むしなかった場合`if(!IsPost)`はたびに、ページが呼び出されるかどうかを初めて実行、または次のボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-213">If you didn't enclose the code in `if(!IsPost)`, it would run every time the page is invoked, whether the first time or in response to a button click.</span></span>

<span data-ttu-id="8ca18-214">コードを含む通知、`else`この時間をブロックします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-214">Notice that the code includes an `else` block this time.</span></span> <span data-ttu-id="8ca18-215">既に述べたように導入された`if`もテストする条件が true でない場合は、代替コードを実行するブロック。</span><span class="sxs-lookup"><span data-stu-id="8ca18-215">As we said when we introduced `if` blocks, sometimes you want to run alternative code if the condition you're testing isn't true.</span></span> <span data-ttu-id="8ca18-216">ここで、ケースです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-216">That's the case here.</span></span> <span data-ttu-id="8ca18-217">(つまり、ページに渡された ID に問題はありません) 場合は、条件が成功した場合は、データベースから行を読み取る。</span><span class="sxs-lookup"><span data-stu-id="8ca18-217">If the condition passes (that is, if the ID passed to the page is ok), you read a row from the database.</span></span> <span data-ttu-id="8ca18-218">ただし、条件が満たさない場合、`else`ブロックが実行され、コードが、エラー メッセージを設定します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-218">However, if the condition doesn't pass, the `else` block runs and the code sets an error message.</span></span>

## <a name="validating-a-value-passed-to-the-page"></a><span data-ttu-id="8ca18-219">ページに渡される値を検証します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-219">Validating a Value Passed to the Page</span></span>

<span data-ttu-id="8ca18-220">コードを使用して`Request.QueryString["id"]`ページに渡される ID を取得します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-220">The code uses `Request.QueryString["id"]` to get the ID that's passed to the page.</span></span> <span data-ttu-id="8ca18-221">コードにより、ID の値が渡された実際には</span><span class="sxs-lookup"><span data-stu-id="8ca18-221">The code makes sure that a value was actually passed for the ID.</span></span> <span data-ttu-id="8ca18-222">値が渡されなかった場合、コードは、検証エラーを設定します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-222">If no value was passed, the code sets a validation error.</span></span>

<span data-ttu-id="8ca18-223">このコードは、情報を検証する別の方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="8ca18-223">This code shows a different way to validate information.</span></span> <span data-ttu-id="8ca18-224">使用していた以前のチュートリアルでは、`Validation`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="8ca18-224">In the previous tutorial, you worked with the `Validation` helper.</span></span> <span data-ttu-id="8ca18-225">を検証するフィールドを登録して、ASP.NET は自動的には、検証し、を使用してエラーを表示`Html.ValidationMessage`と`Html.ValidationSummary`です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-225">You registered fields to validate, and ASP.NET automatically did the validation and displayed errors by using `Html.ValidationMessage` and `Html.ValidationSummary`.</span></span> <span data-ttu-id="8ca18-226">この場合、ただし、するしている実際にはユーザー入力を検証します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-226">In this case, however, you're not really validating user input.</span></span> <span data-ttu-id="8ca18-227">代わりに、別の場所からのページに渡された値を検証しています。</span><span class="sxs-lookup"><span data-stu-id="8ca18-227">Instead, you're validating a value that was passed to the page from elsewhere.</span></span> <span data-ttu-id="8ca18-228">`Validation`ヘルパーしないを自動的に行うことです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-228">The `Validation` helper doesn't do that for you.</span></span>

<span data-ttu-id="8ca18-229">そのため、値をチェックする、自分で使用してテスト`if(!Request.QueryString["ID"].IsEmpty()`)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-229">Therefore, you check the value yourself, by testing it with `if(!Request.QueryString["ID"].IsEmpty()`).</span></span> <span data-ttu-id="8ca18-230">使用して、エラーを表示するには問題がある場合`Html.ValidationSummary`で行ったよう、`Validation`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="8ca18-230">If there's a problem, you can display the error by using `Html.ValidationSummary`, as you did with the `Validation` helper.</span></span> <span data-ttu-id="8ca18-231">呼び出す`Validation.AddFormError`を表示するメッセージを渡します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-231">To do that, you call `Validation.AddFormError` and pass it a message to display.</span></span> <span data-ttu-id="8ca18-232">`Validation.AddFormError`組み込みのメソッドに慣れている既に検証システムに関連するカスタム メッセージを定義することができますです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-232">`Validation.AddFormError` is a built-in method that lets you define custom messages that tie in with the validation system you're already familiar with.</span></span> <span data-ttu-id="8ca18-233">(このチュートリアルの後半で取り上げます、もう少し堅牢なこの検証プロセスを実行する方法について。)</span><span class="sxs-lookup"><span data-stu-id="8ca18-233">(Later in this tutorial we'll talk about how to make this validation process a little more robust.)</span></span>

<span data-ttu-id="8ca18-234">ムービーの ID があることを確認したら、コードは、データベースは、1 つのデータベースのアイテムのみを検索してを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-234">After making sure that there's an ID for the movie, the code reads the database, looking for only a single database item.</span></span> <span data-ttu-id="8ca18-235">(データベース操作の一般的なパターンがおそらく気付き: データベースを開き、SQL ステートメントを定義して、ステートメントを実行します)。この時点では、SQL`Select`ステートメントが含まれる`WHERE ID = @0`です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-235">(You probably have noticed the general pattern for database operations: open the database, define a SQL statement, and run the statement.) This time, the SQL `Select` statement includes `WHERE ID = @0`.</span></span> <span data-ttu-id="8ca18-236">ID は一意であり、1 つのレコードが返されます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-236">Because the ID is unique, only one record can be returned.</span></span>

<span data-ttu-id="8ca18-237">使用して、クエリが実行されます`db.QuerySingle`(いない`db.Query`ムービーの一覧に使用されるため、)、コードに結果を格納して、`row`変数。</span><span class="sxs-lookup"><span data-stu-id="8ca18-237">The query is performed by using `db.QuerySingle` (not `db.Query`, as you used for the movie listing), and the code puts the result into the `row` variable.</span></span> <span data-ttu-id="8ca18-238">名前`row`は任意です。 変数をどのような名前ができます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-238">The name `row` is arbitrary; you can name the variables anything you like.</span></span> <span data-ttu-id="8ca18-239">上部に初期化される変数は、これらの値は、テキスト ボックスに表示できるように、ムービーの詳細で埋められます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-239">The variables initialized at the top are then filled with the movie details so that these values can be displayed in the text boxes.</span></span>

## <a name="testing-the-edit-page-so-far"></a><span data-ttu-id="8ca18-240">テストの編集 ページ (これまで)</span><span class="sxs-lookup"><span data-stu-id="8ca18-240">Testing the Edit Page (So Far)</span></span>

<span data-ttu-id="8ca18-241">ページをテストする場合は、実行、*映画*今すぐ ページをクリックして、**編集**任意のビデオの横にあるリンクします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-241">If you'd like to test your page, run the *Movies* page now and click an **Edit** link next to any movie.</span></span> <span data-ttu-id="8ca18-242">表示されます、 *EditMovie*の詳細ページに入力します。 選択したムービー。</span><span class="sxs-lookup"><span data-stu-id="8ca18-242">You'll see the *EditMovie* page with the details filled in for the movie you selected:</span></span>

![ムービーを編集することを示すビデオ ページを編集します。](updating-data/_static/image3.png)

<span data-ttu-id="8ca18-244">ページの URL がのようなものが含まれることに注意してください。 `?id=10` (またはその他の番号)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-244">Notice that the URL of the page includes something like `?id=10` (or some other number).</span></span> <span data-ttu-id="8ca18-245">これまでにテストを実施する**編集**でリンク、*ムービー*作業、ページが、クエリ文字列から ID を読み取って、動作、データベースが 1 つのムービーのレコードを取得するクエリを実行しているページします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-245">So far you've tested that **Edit** links in the *Movie* page work, that your page is reading the ID from the query string, and that the database query to get a single movie record is working.</span></span>

<span data-ttu-id="8ca18-246">ムービー情報を変更することができますをクリックすると何も起こりません**変更を送信**です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-246">You can change the movie information, but nothing happens when you click **Submit Changes**.</span></span>

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a><span data-ttu-id="8ca18-247">ユーザーの変更と、ムービーを更新するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-247">Adding Code to Update the Movie with the User's Changes</span></span>

<span data-ttu-id="8ca18-248">*EditMovie.cshtml*ファイルを 2 番目の関数 (変更の保存) を実装するには、次のコードを追加の右中かっこの内側、`@`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-248">In the *EditMovie.cshtml* file, to implement the second function (saving changes), add the following code just inside the closing brace of the `@` block.</span></span> <span data-ttu-id="8ca18-249">(コードを配置する正確な場所がわからない場合を表示できる、[ムービーの編集 ページの完全なコード](#Complete_Page_Listing_for_EditMovie)このチュートリアルの最後に表示される)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-249">(If you're not sure exactly where to put the code, you can look at the [complete code listing for the Edit Movie page](#Complete_Page_Listing_for_EditMovie) that appears at the end of this tutorial.)</span></span>

[!code-csharp[Main](updating-data/samples/sample12.cs)]

<span data-ttu-id="8ca18-250">このマークアップおよびコードが内のコードに似ていますが、もう一度*AddMovie*です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-250">Again, this markup and code is similar to the code in *AddMovie*.</span></span> <span data-ttu-id="8ca18-251">コードに、`if(IsPost)`ブロック、ユーザーがクリックした場合にのみ、このコードが実行されるため、**変更を送信**ボタン&mdash;は、ときに、場合にのみ) フォームがポストされました。</span><span class="sxs-lookup"><span data-stu-id="8ca18-251">The code is in an `if(IsPost)` block, because this code runs only when the user clicks the **Submit Changes** button &mdash; that is, when (and only when) the form has been posted.</span></span> <span data-ttu-id="8ca18-252">この場合、使用していないようなテスト`if(IsPost && Validation.IsValid())`— はないと組み合わせている両方のテストを使用しています。</span><span class="sxs-lookup"><span data-stu-id="8ca18-252">In this case, you're not using a test like `if(IsPost && Validation.IsValid())`— that is, you're not combining both tests by using AND.</span></span> <span data-ttu-id="8ca18-253">このページで確認するフォームの送信があるかどうか (`if(IsPost)`)、のみし、検証のためのフィールドを登録します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-253">In this page, you first determine whether there's a form submission (`if(IsPost)`), and only then register the fields for validation.</span></span> <span data-ttu-id="8ca18-254">検証結果をテストすることができますし、(`if(Validation.IsValid()`)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-254">Then you can test the validation results (`if(Validation.IsValid()`).</span></span> <span data-ttu-id="8ca18-255">フローが若干異なりますで、 *AddMovie.cshtml*  ページが、効果は同じです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-255">The flow is slightly different than in the *AddMovie.cshtml* page, but the effect is the same.</span></span>

<span data-ttu-id="8ca18-256">使用して、テキスト ボックスの値を取得する`Request.Form["title"]`と、他の同様のコード`<input>`要素。</span><span class="sxs-lookup"><span data-stu-id="8ca18-256">You get the values of the text boxes by using `Request.Form["title"]` and similar code for the other `<input>` elements.</span></span> <span data-ttu-id="8ca18-257">今回は、コードを取得するムービー ID 隠しフィールド外に注意してください (`<input type="hidden">`)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-257">Notice that this time, the code gets the movie ID out of the hidden field (`<input type="hidden">`).</span></span> <span data-ttu-id="8ca18-258">ときに、ページでは、最初に実行、コードは、クエリ文字列から ID を受け取りました。</span><span class="sxs-lookup"><span data-stu-id="8ca18-258">When the page ran the first time, the code got the ID out of the query string.</span></span> <span data-ttu-id="8ca18-259">クエリ文字列は以来何らかの理由で変更された場合に、最初に表示されていた、ムービーの ID を取得しているかどうかを確認する隠しフィールドから値を取得します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-259">You get the value from the hidden field to make sure that you're getting the ID of the movie that was originally displayed, in case the query string was somehow altered since then.</span></span>

<span data-ttu-id="8ca18-260">非常に重要な違い、 *AddMovie*コードとこのコードは、このコードでは、SQL を使用することを`Update`ステートメントの代わりに、`Insert Into`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-260">The really important difference between the *AddMovie* code and this code is that in this code you use the SQL `Update` statement instead of the `Insert Into` statement.</span></span> <span data-ttu-id="8ca18-261">次の例は、SQL の構文を示しています。`Update`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="8ca18-261">The following example shows the syntax of the SQL `Update` statement:</span></span>

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

<span data-ttu-id="8ca18-262">任意の順序ですべての列を指定でき、必ずしもでは、中にすべての列を更新する必要はありません、`Update`操作します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-262">You can specify any columns in any order, and you don't necessarily have to update every column during an `Update` operation.</span></span> <span data-ttu-id="8ca18-263">(新しいレコードとしてレコードを保存するが有効の許可されていないため自体、ID を更新することはできません、`Update`操作します)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-263">(You cannot update the ID itself, because that would in effect save the record as a new record, and that's not allowed for an `Update` operation.)</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="8ca18-264">**重要な**、`Where`データベースがどのデータベースを認識する方法は、ID を持つ句が非常に重要なレコードを更新します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-264">**Important** The `Where` clause with the ID is very important, because that's how the database knows which database record you want to update.</span></span> <span data-ttu-id="8ca18-265">中断した場合、`Where`句、データベースが更新*すべて*データベースに記録します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-265">If you left off the `Where` clause, the database would update *every* record in the database.</span></span> <span data-ttu-id="8ca18-266">ほとんどの場合、災害になります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-266">In most cases, that would be a disaster.</span></span>


<span data-ttu-id="8ca18-267">コードでは、更新する値は、プレース ホルダーを使用して、SQL ステートメントに渡されます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-267">In the code, the values to update are passed to the SQL statement by using placeholders.</span></span> <span data-ttu-id="8ca18-268">前に述べたしたを繰り返す: セキュリティ上の理由から、*のみ*SQL ステートメントに値を渡すプレース ホルダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-268">To repeat what we've said before: for security reasons, *only* use placeholders to pass values to a SQL statement.</span></span>

<span data-ttu-id="8ca18-269">コードを使用して後`db.Execute`を実行する、`Update`変更を確認できる、一覧のページに戻るステートメントにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-269">After the code uses `db.Execute` to run the `Update` statement, it redirects back to the listing page, where you can see the changes.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="8ca18-270">**別の SQL ステートメント、さまざまな方法**</span><span class="sxs-lookup"><span data-stu-id="8ca18-270">**Different SQL Statements, Different Methods**</span></span>
> 
> <span data-ttu-id="8ca18-271">お気付き少し異なる方法を使用して、別の SQL ステートメントを実行することです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-271">You might have noticed that you use slightly different methods to run different SQL statements.</span></span> <span data-ttu-id="8ca18-272">実行する、`Select`クエリの可能性がありますを返します。 複数のレコードで使用する、`Query`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-272">To run a `Select` query that potentially returns multiple records, you use the `Query` method.</span></span> <span data-ttu-id="8ca18-273">実行する、`Select`がわかっているクエリが 1 つだけデータベースのアイテムを返すを使用する、`QuerySingle`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-273">To run a `Select` query that you know will return only one database item, you use the `QuerySingle` method.</span></span> <span data-ttu-id="8ca18-274">使用する変更を加えることが、データベースのアイテムを返さないコマンドを実行する、`Execute`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-274">To run commands that make changes but that don't return database items, you use the `Execute` method.</span></span>
> 
> <span data-ttu-id="8ca18-275">間の違いで既に説明したようにさまざまな方法を異なる結果が返されるそれぞれのために存在する必要が`Query`と`QuerySingle`です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-275">You have to have different methods because each of them returns different results, as you saw already in the difference between `Query` and `QuerySingle`.</span></span> <span data-ttu-id="8ca18-276">(、`Execute`メソッド実際に値を返しますも&mdash;コマンドによって影響を受けた行の数、つまり&mdash;するしたされて無視をこれまではします)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-276">(The `Execute` method actually returns a value also &mdash; namely, the number of database rows that were affected by the command &mdash; but you've been ignoring that so far.)</span></span>
> 
> <span data-ttu-id="8ca18-277">もちろん、`Query`メソッドは、1 つだけデータベースの行を返す場合があります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-277">Of course, the `Query` method might return only one database row.</span></span> <span data-ttu-id="8ca18-278">ただし、ASP.NET 常には処理の結果、`Query`コレクションとしてのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-278">However, ASP.NET always treats the results of the `Query` method as a collection.</span></span> <span data-ttu-id="8ca18-279">メソッドが 1 行だけを返す場合でもその 1 つの行をコレクションから抽出する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-279">Even if the method returns just one row, you have to extract that single row from the collection.</span></span> <span data-ttu-id="8ca18-280">状況でこのため、場所する*知る*1 行のみが返されます、これは使いやすいビット`QuerySingle`です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-280">Therefore, in situations where you *know* you'll get back only one row, it's a bit more convenient to use `QuerySingle`.</span></span>
> 
> <span data-ttu-id="8ca18-281">特定の種類のデータベース操作を実行するその他のいくつかの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-281">There are a few other methods that perform specific types of database operations.</span></span> <span data-ttu-id="8ca18-282">データベースのメソッドの一覧を見つけることができます、 [ASP.NET Web Pages API のクイック リファレンス](../../api-reference/asp-net-web-pages-api-reference.md#Data)です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-282">You can find a listing of database methods in the [ASP.NET Web Pages API Quick Reference](../../api-reference/asp-net-web-pages-api-reference.md#Data).</span></span>


## <a name="making-validation-for-the-id-more-robust"></a><span data-ttu-id="8ca18-283">堅牢な ID の複数の検証を行う</span><span class="sxs-lookup"><span data-stu-id="8ca18-283">Making Validation for the ID More Robust</span></span>

<span data-ttu-id="8ca18-284">ページの実行、初めて ID を取得したムービー クエリ文字列からそのムービーをデータベースから取得を移動できるようにします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-284">The first time that the page runs, you get the movie ID from the query string so that you can go get that movie from the database.</span></span> <span data-ttu-id="8ca18-285">実際にはこのコードを使用して行った、検索する値を確認しました。</span><span class="sxs-lookup"><span data-stu-id="8ca18-285">You made sure that there actually was a value to go look for, which you did by using this code:</span></span>

[!code-csharp[Main](updating-data/samples/sample13.cs)]

<span data-ttu-id="8ca18-286">確認して、ユーザーを取得する場合にこのコードを使用した、 *EditMovies*  ページでビデオを選択せず、*映画* ページで、わかりやすいエラー メッセージが表示されるページです。</span><span class="sxs-lookup"><span data-stu-id="8ca18-286">You used this code to make sure that if a user gets to the *EditMovies* page without first selecting a movie in the *Movies* page, the page would display a user-friendly error message.</span></span> <span data-ttu-id="8ca18-287">(それ以外の場合、ユーザーが参照と混同する多くの場合、エラー)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-287">(Otherwise, users would see an error that would probably just confuse them.)</span></span>

<span data-ttu-id="8ca18-288">ただし、この検証は、堅牢性はありません。</span><span class="sxs-lookup"><span data-stu-id="8ca18-288">However, this validation isn't very robust.</span></span> <span data-ttu-id="8ca18-289">ページは、これらのエラーと共にも呼び出される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-289">The page might also be invoked with these errors:</span></span>

- <span data-ttu-id="8ca18-290">ID では、数値はありません。</span><span class="sxs-lookup"><span data-stu-id="8ca18-290">The ID isn't a number.</span></span> <span data-ttu-id="8ca18-291">たとえばのように URL を使用して、ページを呼び出すことができます`http://localhost:nnnnn/EditMovie?id=abc`です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-291">For example, the page could be invoked with a URL like `http://localhost:nnnnn/EditMovie?id=abc`.</span></span>
- <span data-ttu-id="8ca18-292">ID は、数値が存在しないムービーを参照して (たとえば、 `http://localhost:nnnnn/EditMovie?id=100934`)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-292">The ID is a number, but it references a movie that doesn't exist (for example, `http://localhost:nnnnn/EditMovie?id=100934`).</span></span>

<span data-ttu-id="8ca18-293">これらの Url は、実行に起因するエラーを表示する興味があるなら、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="8ca18-293">If you're curious to see the errors that result from these URLs, run the *Movies* page.</span></span> <span data-ttu-id="8ca18-294">ビデオの編集を選択しの URL を変更、 *EditMovie*英字を含む URL へのページングを ID または存在しない映画の ID。</span><span class="sxs-lookup"><span data-stu-id="8ca18-294">Select a movie to edit, and then change the URL of the *EditMovie* page to a URL that contains an alphabetic ID or the ID of a non-existent movie.</span></span>

<span data-ttu-id="8ca18-295">これは何か。</span><span class="sxs-lookup"><span data-stu-id="8ca18-295">So what should you do?</span></span> <span data-ttu-id="8ca18-296">最初の解決策では、ことだけでなく、ID に渡される、ページが、ID が整数であるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-296">The first fix is to make sure that not only is an ID passed to the page, but that the ID is an integer.</span></span> <span data-ttu-id="8ca18-297">コードを変更、`!IsPost`テストをこの例のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-297">Change the code for the `!IsPost` test to look like this example:</span></span>

[!code-csharp[Main](updating-data/samples/sample14.cs)]

<span data-ttu-id="8ca18-298">2 番目の条件を追加した、`IsEmpty`にリンクされているテスト`&&`(論理 AND)。</span><span class="sxs-lookup"><span data-stu-id="8ca18-298">You've added a second condition to the `IsEmpty` test, linked with `&&` (logical AND):</span></span>

[!code-csharp[Main](updating-data/samples/sample15.cs)]

<span data-ttu-id="8ca18-299">留意することがあります、 [ASP.NET Web Pages のプログラミングの概要](../introducing-razor-syntax-c.md)などのメソッドを説明するチュートリアル`AsBool`、`AsInt`文字の文字列をその他の任意のデータ型に変換します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-299">You might remember from the [Introduction to ASP.NET Web Pages Programming](../introducing-razor-syntax-c.md) tutorial that methods like `AsBool` an `AsInt` convert a character string to some other data type.</span></span> <span data-ttu-id="8ca18-300">`IsInt`メソッド (など、他のユーザーおよび`IsBool`と`IsDateTime`) と似ています。</span><span class="sxs-lookup"><span data-stu-id="8ca18-300">The `IsInt` method (and others, like `IsBool` and `IsDateTime`) are similar.</span></span> <span data-ttu-id="8ca18-301">ただし、それらをテストのみかどうかを*できます*実際には、変換を実行せず、文字列に変換します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-301">However, they test only whether you *can* convert the string, without actually performing the conversion.</span></span> <span data-ttu-id="8ca18-302">基本的に指示しているので、ここで、*クエリ文字列の値を整数に変換できる場合.*.</span><span class="sxs-lookup"><span data-stu-id="8ca18-302">So here you're essentially saying *If the query string value can be converted to an integer ...*.</span></span>

<span data-ttu-id="8ca18-303">他の潜在的な問題はムービーが存在しないを探しています。</span><span class="sxs-lookup"><span data-stu-id="8ca18-303">The other potential problem is looking for a movie that doesn't exist.</span></span> <span data-ttu-id="8ca18-304">ムービーを取得するコードは、このコードのようになります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-304">The code to get a movie looks like this code:</span></span>

[!code-csharp[Main](updating-data/samples/sample16.cs)]

<span data-ttu-id="8ca18-305">渡す場合、`movieId`値を`QuerySingle`実際ムービーに対応していないメソッドの場合は、何も行われませんが返され、ステートメントをに従って (たとえば、 `title=row.Title`) にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-305">If you pass a `movieId` value to the `QuerySingle` method that doesn't correspond to an actual movie, nothing is returned and the statements that follow (for example, `title=row.Title`) result in errors.</span></span>

<span data-ttu-id="8ca18-306">もう一度が簡単に解決します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-306">Again there's an easy fix.</span></span> <span data-ttu-id="8ca18-307">場合、`db.QuerySingle`メソッドには、結果が返されない、`row`変数は null になります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-307">If the `db.QuerySingle` method returns no results, the `row` variable will be null.</span></span> <span data-ttu-id="8ca18-308">チェックできるようにするかどうか、`row`からその値を取得しようとする前に、変数が null です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-308">So you can check whether the `row` variable is null before you try to get values from it.</span></span> <span data-ttu-id="8ca18-309">次のコードを追加、`if`の値を取得するステートメントの周囲、`row`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="8ca18-309">The following code adds an `if` block around the statements that get the values out of the `row` object:</span></span>

[!code-csharp[Main](updating-data/samples/sample17.cs)]

<span data-ttu-id="8ca18-310">これら 2 つの追加の検証テストより堅牢ながページになります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-310">With these two additional validation tests, the page becomes more bullet-proof.</span></span> <span data-ttu-id="8ca18-311">完全なコード、`!IsPost`ブランチは次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-311">The complete code for the `!IsPost` branch now looks like this example:</span></span>

[!code-csharp[Main](updating-data/samples/sample18.cs)]

<span data-ttu-id="8ca18-312">おわかりますもう一度このタスクは、の良い使用、`else`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-312">We'll note once more that this task is a good use for an `else` block.</span></span> <span data-ttu-id="8ca18-313">テストが成功しない場合、`else`ブロックがエラー メッセージを設定します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-313">If the tests don't pass, the `else` blocks set error messages.</span></span>

## <a name="adding-a-link-to-return-to-the-movies-page"></a><span data-ttu-id="8ca18-314">映画のページに戻るためのリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-314">Adding a Link to Return to the Movies Page</span></span>

<span data-ttu-id="8ca18-315">リンクを追加するのには、最終で役立つ詳細に戻る、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="8ca18-315">A final and helpful detail is to add a link back to the *Movies* page.</span></span> <span data-ttu-id="8ca18-316">通常のフロー イベント、ユーザーがで開始されます、*映画*ページ、をクリックし、**編集**リンクします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-316">In the ordinary flow of events, users will start at the *Movies* page and click an **Edit** link.</span></span> <span data-ttu-id="8ca18-317">表示される、 *EditMovie*  ページで、ムービーを編集し、ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-317">That brings them to the *EditMovie* page, where they can edit the movie and click the button.</span></span> <span data-ttu-id="8ca18-318">戻るリダイレクト、コードが、変更を処理した後、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="8ca18-318">After the code processes the change, it redirects back to the *Movies* page.</span></span>

<span data-ttu-id="8ca18-319">ただし。</span><span class="sxs-lookup"><span data-stu-id="8ca18-319">However:</span></span>

- <span data-ttu-id="8ca18-320">ユーザー情報は一切変更しないように指定します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-320">The user might decide not to change anything.</span></span>
- <span data-ttu-id="8ca18-321">クリックしてせず、このページにユーザーを受けている可能性があります、**編集**内のリンク、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="8ca18-321">The user might have gotten to this page without first clicking an **Edit** link in the *Movies* page.</span></span>

<span data-ttu-id="8ca18-322">メインの一覧を返すことを簡単にするどちらにしても、します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-322">Either way, you want to make it easy for them to return to the main listing.</span></span> <span data-ttu-id="8ca18-323">簡単な解決策は&mdash;終了の直後に、次のマークアップを追加`</form>`マークアップ タグ。</span><span class="sxs-lookup"><span data-stu-id="8ca18-323">It's an easy fix &mdash; add the following markup just after the closing `</form>` tag in the markup:</span></span>

[!code-html[Main](updating-data/samples/sample19.html)]

<span data-ttu-id="8ca18-324">このマークアップの同じ構文を使用して、`<a>`要素を別の場所を見てきました。</span><span class="sxs-lookup"><span data-stu-id="8ca18-324">This markup uses the same syntax for an `<a>` element that you've seen elsewhere.</span></span> <span data-ttu-id="8ca18-325">URL が含まれます`~`「web サイトのルートです」ことを意味するには。</span><span class="sxs-lookup"><span data-stu-id="8ca18-325">The URL includes `~` to mean "root of the website."</span></span>

## <a name="testing-the-movie-update-process"></a><span data-ttu-id="8ca18-326">ムービーの更新プロセスのテスト</span><span class="sxs-lookup"><span data-stu-id="8ca18-326">Testing the Movie Update Process</span></span>

<span data-ttu-id="8ca18-327">これで次のようにテストすることができます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-327">Now you can test.</span></span> <span data-ttu-id="8ca18-328">実行、*映画* ページで、をクリックして**編集**ムービーの横にあります。</span><span class="sxs-lookup"><span data-stu-id="8ca18-328">Run the *Movies* page, and click **Edit** next to a movie.</span></span> <span data-ttu-id="8ca18-329">ときに、 *EditMovie*ページが表示されたら、および変更する、ムービーをクリックして**変更を送信**です。</span><span class="sxs-lookup"><span data-stu-id="8ca18-329">When the *EditMovie* page appears, make changes to the movie and click **Submit Changes**.</span></span> <span data-ttu-id="8ca18-330">ムービーの一覧が表示される場合、変更内容が表示されているかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-330">When the movie listing appears, make sure that your changes are shown.</span></span>

<span data-ttu-id="8ca18-331">検証が動作していることを確認して、をクリックして**編集**別ムービーにします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-331">To make sure that validation is working, click **Edit** for another movie.</span></span> <span data-ttu-id="8ca18-332">取得するときに、 *EditMovie*  ページで、クリア、**ジャンル**フィールド (または**年**フィールド、またはその両方) の変更を送信しようとします。</span><span class="sxs-lookup"><span data-stu-id="8ca18-332">When you get to the *EditMovie* page, clear the **Genre** field (or **Year** field, or both) and try to submit your changes.</span></span> <span data-ttu-id="8ca18-333">予想できるように、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-333">You'll see an error, as you'd expect:</span></span>

![検証エラーを示すビデオ ページを編集します。](updating-data/_static/image4.png)

<span data-ttu-id="8ca18-335">をクリックして、**ムービーの一覧に戻る**のリンクを変更を破棄してに戻り、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="8ca18-335">Click the **Return to movie listing** link to abandon your changes and return to the *Movies* page.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="8ca18-336">次へ直近の見通し</span><span class="sxs-lookup"><span data-stu-id="8ca18-336">Coming Up Next</span></span>

<span data-ttu-id="8ca18-337">次のチュートリアルでは、ムービーのレコードを削除する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8ca18-337">In the next tutorial, you'll see how to delete a movie record.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a><span data-ttu-id="8ca18-338">(リンクの編集と更新された) ビデオ ページの完全な一覧</span><span class="sxs-lookup"><span data-stu-id="8ca18-338">Complete Listing for Movie Page (Updated with Edit Links)</span></span>

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a><span data-ttu-id="8ca18-339">ムービーの編集 ページのページの一覧を完了します。</span><span class="sxs-lookup"><span data-stu-id="8ca18-339">Complete Page Listing for Edit Movie Page</span></span>

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="8ca18-340">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="8ca18-340">Additional Resources</span></span>

- [<span data-ttu-id="8ca18-341">Razor 構文を使用して ASP.NET Web プログラミングの概要</span><span class="sxs-lookup"><span data-stu-id="8ca18-341">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../../getting-started/introducing-razor-syntax-c.md)
- <span data-ttu-id="8ca18-342">[SQL UPDATE ステートメント](http://www.w3schools.com/sql/sql_update.asp)W3Schools サイト</span><span class="sxs-lookup"><span data-stu-id="8ca18-342">[SQL UPDATE Statement](http://www.w3schools.com/sql/sql_update.asp) on the W3Schools site</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8ca18-343">[前へ](entering-data.md)
[次へ](deleting-data.md)</span><span class="sxs-lookup"><span data-stu-id="8ca18-343">[Previous](entering-data.md)
[Next](deleting-data.md)</span></span>
