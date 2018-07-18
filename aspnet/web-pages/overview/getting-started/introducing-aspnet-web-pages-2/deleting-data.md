---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: ASP.NET Web Pages の概要 - データベースのデータの削除 |Microsoft Docs
author: tfitzmac
description: このチュートリアルでは、個々 のデータベース エントリを削除する方法を示します。 ASP.NET Web Pa. 内のデータベース データの更新をシリーズを完了したと想定して.
ms.author: aspnetcontent
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 234b5f99c5d5f580316204c88ea1ab8c1269d452
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815357"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="06e9f-104">ASP.NET Web ページの概要 - データベースのデータを削除します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="06e9f-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="06e9f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="06e9f-106">このチュートリアルでは、個々 のデータベース エントリを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="06e9f-107">を通じてシリーズを完了したと想定して[データベースのデータを更新する ASP.NET Web Pages で](updating-data.md)します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="06e9f-108">学習内容。</span><span class="sxs-lookup"><span data-stu-id="06e9f-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="06e9f-109">レコードの一覧から個々 のレコードを選択する方法。</span><span class="sxs-lookup"><span data-stu-id="06e9f-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="06e9f-110">データベースから 1 つのレコードを削除する方法。</span><span class="sxs-lookup"><span data-stu-id="06e9f-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="06e9f-111">フォームで、特定のボタンがクリックしてされたことを確認する方法。</span><span class="sxs-lookup"><span data-stu-id="06e9f-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="06e9f-112">説明した機能/テクノロジ:</span><span class="sxs-lookup"><span data-stu-id="06e9f-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="06e9f-113">`WebGrid`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="06e9f-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="06e9f-114">SQL`Delete`コマンド。</span><span class="sxs-lookup"><span data-stu-id="06e9f-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="06e9f-115">`Database.Execute` SQL を実行するメソッドを`Delete`コマンド。</span><span class="sxs-lookup"><span data-stu-id="06e9f-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="06e9f-116">構築します</span><span class="sxs-lookup"><span data-stu-id="06e9f-116">What You'll Build</span></span>

<span data-ttu-id="06e9f-117">前のチュートリアルでは、既存のデータベース レコードを更新する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="06e9f-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="06e9f-118">このチュートリアルは、似ていますが、レコードを更新するには、代わりに、それを削除します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="06e9f-119">プロセスは、このチュートリアルは、短縮されるためにより単純に、削除がある点がほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="06e9f-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="06e9f-120">*映画*を更新します ページで、`WebGrid`ヘルパーを表示できるように、**削除**に付属する各ムービーの横にあるリンク、**編集**先ほど追加したリンク。</span><span class="sxs-lookup"><span data-stu-id="06e9f-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![各ムービーの削除 リンクを示すビデオ ページ](deleting-data/_static/image1.png)

<span data-ttu-id="06e9f-122">編集をクリックすると、**削除**リンクに移動する別のページでムービー情報が既にフォームで。</span><span class="sxs-lookup"><span data-stu-id="06e9f-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![ムービー表示ビデオ ページを削除します。](deleting-data/_static/image2.png)

<span data-ttu-id="06e9f-124">レコードを完全に削除するボタンをクリックできます。</span><span class="sxs-lookup"><span data-stu-id="06e9f-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="06e9f-125">ムービーの一覧に、[削除] リンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="06e9f-126">追加することから始めます、**削除**へのリンク、`WebGrid`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="06e9f-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="06e9f-127">このリンクと似ています、**編集**リンクが前のチュートリアルで追加されました。</span><span class="sxs-lookup"><span data-stu-id="06e9f-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="06e9f-128">開く、 *Movies.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="06e9f-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="06e9f-129">変更、`WebGrid`列を追加してページの本文内のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="06e9f-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="06e9f-130">変更後のマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="06e9f-131">新しい列では、この 1 つです。</span><span class="sxs-lookup"><span data-stu-id="06e9f-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="06e9f-132">グリッドの構成方法、**編集**列がグリッドの左端と**削除**列は最も右にあります。</span><span class="sxs-lookup"><span data-stu-id="06e9f-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="06e9f-133">(コンマの後に、`Year`列、いることを確認していない場合)。これらのリンク列の移動先に関する特別なものを使用する必要があるし、互いの横にあるように簡単に配置する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="06e9f-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="06e9f-134">この場合は、混同を取得するは困難にする別です。</span><span class="sxs-lookup"><span data-stu-id="06e9f-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![編集と詳細のリンクを含む動画ページのマークを表示して相互の横にあるいません。](deleting-data/_static/image3.png)

<span data-ttu-id="06e9f-136">新しい列には、リンクが表示されます (`<a>`要素)"Delete"と表示されました。</span><span class="sxs-lookup"><span data-stu-id="06e9f-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="06e9f-137">リンクのターゲット (その`href`属性) で、この URL のように最終的に解決されるコードは、`id`各ムービーのさまざまな値。</span><span class="sxs-lookup"><span data-stu-id="06e9f-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="06e9f-138">このリンクをという名前のページを呼び出す*DeleteMovie*を選択したムービーの ID を渡します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="06e9f-139">このチュートリアルには触れませんが、このリンクの構築方法の詳細とほぼ同じである、**編集**、前のチュートリアルからのリンク ([データベースのデータを更新する ASP.NET Web Pages で](updating-data.md))。</span><span class="sxs-lookup"><span data-stu-id="06e9f-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="06e9f-140">Delete ページの作成</span><span class="sxs-lookup"><span data-stu-id="06e9f-140">Creating the Delete Page</span></span>

<span data-ttu-id="06e9f-141">対象となるページを作成できるようになりました、**削除**グリッドのリンク。</span><span class="sxs-lookup"><span data-stu-id="06e9f-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="06e9f-142">**重要な**最初を削除するレコードを選択して、プロセスを確認する別のページとボタンを使用しての手法はセキュリティのきわめて重要です。</span><span class="sxs-lookup"><span data-stu-id="06e9f-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="06e9f-143">前のチュートリアルで読んだとを行う*任意*web サイトに変更する必要があります*常に*行うフォームを使用して&mdash;は、HTTP POST 操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="06e9f-144">した場合 (つまり、GET 操作を使用) のリンクをクリックするだけで、サイトを変更することも場合、人でした、サイトに対して単純な要求を行うし、データを削除します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="06e9f-145">でも、サイトのインデックス作成は、検索エンジン クローラーでは、次のリンクをするだけでデータを誤って削除する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="06e9f-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="06e9f-146">アプリでは、ユーザーがレコードを変更することができます、ときにも編集するため、ユーザーに、レコードを提示するがあります。</span><span class="sxs-lookup"><span data-stu-id="06e9f-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="06e9f-147">レコードを削除するのには、この手順を省略したくなる場合があります。</span><span class="sxs-lookup"><span data-stu-id="06e9f-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="06e9f-148">ただし、その手順をスキップするはありません。</span><span class="sxs-lookup"><span data-stu-id="06e9f-148">Don't skip that step, though.</span></span> <span data-ttu-id="06e9f-149">(これはも、レコードをことを目的とするレコードを削除しようとしていることを確認します。 ユーザーに便利です)。</span><span class="sxs-lookup"><span data-stu-id="06e9f-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="06e9f-150">後続のチュートリアル セットでは、ユーザーがレコードを削除する前にログインする必要がありますので、ログイン機能を追加する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="06e9f-151">という名前のページを作成する*DeleteMovie.cshtml*と新機能を次のマークアップ ファイルで置換します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="06e9f-152">このマークアップは似ています、 *EditMovie*テキスト ボックスを使用する代わりに、ページ (`<input type="text">`)、マークアップを含む`<span>`要素。</span><span class="sxs-lookup"><span data-stu-id="06e9f-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="06e9f-153">編集するのには、ここ何があります。</span><span class="sxs-lookup"><span data-stu-id="06e9f-153">There's nothing here to edit.</span></span> <span data-ttu-id="06e9f-154">できるようにするため、適切なムービーを削除しようとしていることを確認して、映画の詳細を表示するだけです。</span><span class="sxs-lookup"><span data-stu-id="06e9f-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="06e9f-155">マークアップには、リンク、ムービーの一覧ページに戻り、ユーザーにはが既に含まれています。</span><span class="sxs-lookup"><span data-stu-id="06e9f-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="06e9f-156">*EditMovie*  ページで、選択したムービーの ID は非表示フィールドに格納されます。</span><span class="sxs-lookup"><span data-stu-id="06e9f-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="06e9f-157">(として渡されるページに、最初に、クエリ文字列値。)`Html.ValidationSummary`呼び出しする検証エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="06e9f-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="06e9f-158">この場合、エラーは、ページにムービー ID が渡されなかったことや、映画 ID が無効である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="06e9f-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="06e9f-159">このページでムービーを選択せずに実行だれかが場合に、このような状況が発生する可能性があります、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="06e9f-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="06e9f-160">ボタンのキャプション**削除ムービー**、name 属性に設定し、 `buttonDelete`。</span><span class="sxs-lookup"><span data-stu-id="06e9f-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="06e9f-161">`name`属性は、フォームを送信するボタンを識別するために、コードで使用されます。</span><span class="sxs-lookup"><span data-stu-id="06e9f-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="06e9f-162">1) ページが初めて表示したとき、ムービーの詳細を確認するコードを記述し、ボタンをクリックすると、2) 実際には、ムービーを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="06e9f-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="06e9f-163">1 つのムービーを読み取るコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="06e9f-164">上部にある、 *DeleteMovie.cshtml*ページで、次のコード ブロックを追加します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="06e9f-165">このマークアップは、対応するコードと同じ、 *EditMovie*ページ。</span><span class="sxs-lookup"><span data-stu-id="06e9f-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="06e9f-166">クエリ文字列からムービー ID を取得し、ID を使用して、データベースからレコードを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="06e9f-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="06e9f-167">コードには検証テストが含まれています (`IsInt()`と`row != null`) ページに渡される映画 ID が有効であるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="06e9f-168">このコードは、最初に、ページの実行にのみ実行する必要がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="06e9f-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="06e9f-169">ユーザーがクリックしたときに、データベースからムービー レコードを再読み込みしたくない、**削除ムービー**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="06e9f-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="06e9f-170">したがって、ムービーがあることを示すテスト内で読み取るコード`if(!IsPost)`&mdash;つまり*要求が post 操作 (フォームの送信) ではないかどうか*します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="06e9f-171">選択したムービーを削除するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="06e9f-172">ユーザーがボタンをクリックすると、ムービーを削除するには、中かっこの内側は、次のコードを追加、`@`ブロック。</span><span class="sxs-lookup"><span data-stu-id="06e9f-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="06e9f-173">このコードは、既存のレコードを更新するためのコードに似ていますが、簡単ですが。</span><span class="sxs-lookup"><span data-stu-id="06e9f-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="06e9f-174">コードは、基本的に、SQL を実行`Delete`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="06e9f-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="06e9f-175">*EditMovie*  ページで、コードは、`if(IsPost)`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="06e9f-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="06e9f-176">今回は、`if()`条件は、もう少し複雑になります。</span><span class="sxs-lookup"><span data-stu-id="06e9f-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="06e9f-177">ここで、2 つの条件があります。</span><span class="sxs-lookup"><span data-stu-id="06e9f-177">There are two conditions here.</span></span> <span data-ttu-id="06e9f-178">1 つが前に説明したように、ページの送信されていることは&mdash;`if(IsPost)`します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="06e9f-179">2 番目の条件が`!Request["buttonDelete"].IsEmpty()`、要求という名前のオブジェクトが含まれることを意味`buttonDelete`します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="06e9f-180">確かに、フォームを送信するボタンのテストの間接的な手段になります。</span><span class="sxs-lookup"><span data-stu-id="06e9f-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="06e9f-181">フォームには、複数の submit ボタンが含まれる、要求でクリックしてされたボタンの名前のみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="06e9f-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="06e9f-182">そのため、論理的にはかどうか、特定のボタンの名前は、要求に表示されます。&mdash;コードでは、そのボタンが空でない場合に説明したように、または&mdash;フォームを送信するボタンです。</span><span class="sxs-lookup"><span data-stu-id="06e9f-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="06e9f-183">`&&`演算子手段「と」(論理 AND)。</span><span class="sxs-lookup"><span data-stu-id="06e9f-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="06e9f-184">そのため、全体`if`条件がしています.</span><span class="sxs-lookup"><span data-stu-id="06e9f-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="06e9f-185">*この要求が post (初回要求しない)*</span><span class="sxs-lookup"><span data-stu-id="06e9f-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="06e9f-186">AND</span><span class="sxs-lookup"><span data-stu-id="06e9f-186">AND</span></span>  
  
<span data-ttu-id="06e9f-187">** `buttonDelete`*ボタンがフォームを送信するボタン。*</span><span class="sxs-lookup"><span data-stu-id="06e9f-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="06e9f-188">そのため (実際には、このページ) でこのフォームにボタンが 1 つが含まれていますの追加のテスト`buttonDelete`は技術的には必要ありません。</span><span class="sxs-lookup"><span data-stu-id="06e9f-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="06e9f-189">ただし、データが完全に削除する操作を実行しようとしています。</span><span class="sxs-lookup"><span data-stu-id="06e9f-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="06e9f-190">これに限り、ユーザーが要求して明示的に場合にのみ、操作を実行していることを確認にします。</span><span class="sxs-lookup"><span data-stu-id="06e9f-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="06e9f-191">たとえば、後からこのページを拡大し、その他のボタンを追加するとします。</span><span class="sxs-lookup"><span data-stu-id="06e9f-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="06e9f-192">場合にのみのムービーを削除するコードの実行後も、`buttonDelete`ボタンがクリックされました。</span><span class="sxs-lookup"><span data-stu-id="06e9f-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="06e9f-193">*EditMovie*  ページで、非表示フィールドから ID を取得し、SQL コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="06e9f-194">構文、`Delete`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="06e9f-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="06e9f-195">含めるには必須では、`WHERE`句と ID</span><span class="sxs-lookup"><span data-stu-id="06e9f-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="06e9f-196">WHERE 句を省略すると*テーブル内のすべてのレコードが削除されます*します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="06e9f-197">説明したように、SQL コマンドにプレース ホルダーを使用して ID 値を渡します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="06e9f-198">ムービーの削除プロセスをテストします。</span><span class="sxs-lookup"><span data-stu-id="06e9f-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="06e9f-199">今すぐテストすることができます。</span><span class="sxs-lookup"><span data-stu-id="06e9f-199">Now you can test.</span></span> <span data-ttu-id="06e9f-200">実行、*映画*ページ、およびクリックして**削除**ムービーの横にあります。</span><span class="sxs-lookup"><span data-stu-id="06e9f-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="06e9f-201">ときに、 *DeleteMovie*ページが表示されたら、をクリックして**削除ムービー**します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![ムービーの削除 ボタンが強調表示されているビデオ ページを削除します。](deleting-data/_static/image4.png)

<span data-ttu-id="06e9f-203">ボタンをクリックすると、コードは、ムービーを削除し、ムービーの一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="06e9f-204">削除されたムービーを検索できますが、削除されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="06e9f-205">次回について</span><span class="sxs-lookup"><span data-stu-id="06e9f-205">Coming Up Next</span></span>

<span data-ttu-id="06e9f-206">次のチュートリアルでは、一般的な外観とレイアウト、サイト上のすべてのページを指定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="06e9f-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="06e9f-207">ムービーのページ (更新と削除のリンク) の完全な一覧</span><span class="sxs-lookup"><span data-stu-id="06e9f-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="06e9f-208">DeleteMovie ページ全体を示します</span><span class="sxs-lookup"><span data-stu-id="06e9f-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="06e9f-209">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="06e9f-209">Additional Resources</span></span>

- [<span data-ttu-id="06e9f-210">Razor 構文を使用して ASP.NET Web プログラミングの概要</span><span class="sxs-lookup"><span data-stu-id="06e9f-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="06e9f-211">[SQL の DELETE ステートメント](http://www.w3schools.com/sql/sql_delete.asp)W3Schools サイト</span><span class="sxs-lookup"><span data-stu-id="06e9f-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="06e9f-212">[前へ](updating-data.md)
> [次へ](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="06e9f-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
