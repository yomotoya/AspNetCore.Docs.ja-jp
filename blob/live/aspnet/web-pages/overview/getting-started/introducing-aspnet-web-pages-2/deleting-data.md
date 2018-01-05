---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: "ASP.NET Web Pages の概要 - データベースのデータの削除 |Microsoft ドキュメント"
author: tfitzmac
description: "このチュートリアルでは、個々 のデータベース エントリを削除する方法を示します。 ASP.NET Web Pa. 内のデータベース データの更新で、系列を修了を想定しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 5bc92b5d40e7a55dcd730d552c71031d913b277e
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2018
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="a648e-104">ASP.NET Web ページの概要 - データベースのデータを削除します。</span><span class="sxs-lookup"><span data-stu-id="a648e-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="a648e-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a648e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a648e-106">このチュートリアルでは、個々 のデータベース エントリを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a648e-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="a648e-107">を通じてシリーズを完了すると想定[データベースのデータを更新する ASP.NET Web Pages で](updating-data.md)です。</span><span class="sxs-lookup"><span data-stu-id="a648e-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="a648e-108">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="a648e-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="a648e-109">レコードの一覧から個々 のレコードを選択する方法です。</span><span class="sxs-lookup"><span data-stu-id="a648e-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="a648e-110">データベースから 1 つのレコードを削除する方法です。</span><span class="sxs-lookup"><span data-stu-id="a648e-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="a648e-111">フォームで、特定のボタンがクリックされたことを確認する方法です。</span><span class="sxs-lookup"><span data-stu-id="a648e-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="a648e-112">説明されている機能/テクノロジ:</span><span class="sxs-lookup"><span data-stu-id="a648e-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="a648e-113">`WebGrid`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="a648e-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="a648e-114">SQL`Delete`コマンド。</span><span class="sxs-lookup"><span data-stu-id="a648e-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="a648e-115">`Database.Execute` SQL を実行するメソッドを`Delete`コマンド。</span><span class="sxs-lookup"><span data-stu-id="a648e-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="a648e-116">新機能のビルドします。</span><span class="sxs-lookup"><span data-stu-id="a648e-116">What You'll Build</span></span>

<span data-ttu-id="a648e-117">前のチュートリアルでは、既存のデータベース レコードを更新する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="a648e-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="a648e-118">このチュートリアルは、似ていますが、レコードを更新するには、代わりに、それを削除します。</span><span class="sxs-lookup"><span data-stu-id="a648e-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="a648e-119">プロセスは、このチュートリアルに短くなるように単純になりますが削除する点を除いて、ほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="a648e-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="a648e-120">*映画*を更新します ページで、`WebGrid`ヘルパーを表示できるように、**削除**に付随する各ムービーの横にあるリンク、**編集**以前追加したリンクします。</span><span class="sxs-lookup"><span data-stu-id="a648e-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![各ムービーの削除 リンクを表示するビデオ ページ](deleting-data/_static/image1.png)

<span data-ttu-id="a648e-122">同様に編集するをクリックすると、**削除**リンクに移動する異なるページでは、ムービー情報が既にフォームで。</span><span class="sxs-lookup"><span data-stu-id="a648e-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![表示ムービーでビデオ ページを削除します。](deleting-data/_static/image2.png)

<span data-ttu-id="a648e-124">レコードを完全に削除するボタンをクリックすることができますし、します。</span><span class="sxs-lookup"><span data-stu-id="a648e-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="a648e-125">ムービーの一覧に、[削除] リンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="a648e-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="a648e-126">追加することで開始、**削除**へのリンク、`WebGrid`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="a648e-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="a648e-127">このリンクがに似ていますが、**編集**リンク前のチュートリアルで追加します。</span><span class="sxs-lookup"><span data-stu-id="a648e-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="a648e-128">開く、 *Movies.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a648e-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="a648e-129">変更、`WebGrid`列を追加して、ページの本文内のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="a648e-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="a648e-130">変更後のマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="a648e-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="a648e-131">新しい列では、この 1 つです。</span><span class="sxs-lookup"><span data-stu-id="a648e-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="a648e-132">グリッドの構成方法、**編集**列は、グリッドで一番左と**削除**列は右端にあります。</span><span class="sxs-lookup"><span data-stu-id="a648e-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="a648e-133">(後にコンマがあります、`Year`ここで、列ことに注意していない場合にします)。これらのリンク列の移動先に関して特別なところを使用する必要があるし、互いの横にある同様の容易に配置する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a648e-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="a648e-134">この場合、これらは個別に取得を混在させることが困難にします。</span><span class="sxs-lookup"><span data-stu-id="a648e-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![編集と詳細へのリンクをムービー ページがいるいない並べて表示するマーク](deleting-data/_static/image3.png)

<span data-ttu-id="a648e-136">新しい列は、リンクを示しています (`<a>`要素)"Delete"と表示されました。</span><span class="sxs-lookup"><span data-stu-id="a648e-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="a648e-137">リンクのターゲット (その`href`属性) は、コードでこの URL のようなものに最終的に解決される、`id`各ムービーのさまざまな値。</span><span class="sxs-lookup"><span data-stu-id="a648e-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="a648e-138">このリンクをという名前のページを呼び出す*DeleteMovie*を選択したムービーの ID を渡します。</span><span class="sxs-lookup"><span data-stu-id="a648e-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="a648e-139">このチュートリアルはありませんについて詳しくは、このリンクの構築方法とほぼ同じになっているため、**編集**リンク前のチュートリアル ([データベースのデータを更新する ASP.NET Web Pages で](updating-data.md))。</span><span class="sxs-lookup"><span data-stu-id="a648e-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="a648e-140">ページの削除 を作成します。</span><span class="sxs-lookup"><span data-stu-id="a648e-140">Creating the Delete Page</span></span>

<span data-ttu-id="a648e-141">対象となるページを作成できるようになりました、**削除**グリッド内のリンク。</span><span class="sxs-lookup"><span data-stu-id="a648e-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a648e-142">**重要な**最初を削除するレコードを選択して、プロセスを確認する別のページとボタンを使用しての手法がセキュリティの非常に重要です。</span><span class="sxs-lookup"><span data-stu-id="a648e-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="a648e-143">前のチュートリアルで読んだとして行う*任意*web サイトへの変更の種類にする必要があります*常に*行うフォームを使用して&mdash;は、HTTP POST 操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="a648e-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="a648e-144">を加えた場合に (つまり、GET 操作を使用) のリンクをクリックするだけで、サイトを変更すること人でした、サイトに対して単純な要求を行うし、データを削除します。</span><span class="sxs-lookup"><span data-stu-id="a648e-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="a648e-145">でも、検索エンジンのクローラー サイト インデックス作成を行うことは、次のリンクだけでデータを誤って削除可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a648e-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="a648e-146">アプリでは、ユーザー レコードを変更することができます、ときにする必要があるかを編集するためのユーザーにレコードを表示します。</span><span class="sxs-lookup"><span data-stu-id="a648e-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="a648e-147">レコードを削除するのには、この手順をスキップしたくなる場合があります。</span><span class="sxs-lookup"><span data-stu-id="a648e-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="a648e-148">ただしその手順をスキップしません。</span><span class="sxs-lookup"><span data-stu-id="a648e-148">Don't skip that step, though.</span></span> <span data-ttu-id="a648e-149">(これはもユーザーのレコードを確認し、これらの目的は、レコードを削除していることを確認するために役立ちます)。</span><span class="sxs-lookup"><span data-stu-id="a648e-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="a648e-150">後続のチュートリアルのセットでは、ユーザーがレコードを削除する前にログインする必要がありますので、ログインの機能を追加する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a648e-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="a648e-151">という名前のページを作成する*DeleteMovie.cshtml*が次のマークアップ ファイルを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a648e-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="a648e-152">同様には、このマークアップ、 *EditMovie*ページ、テキスト ボックスを使用する代わりにことを除いて (`<input type="text">`)、マークアップを含む`<span>`要素。</span><span class="sxs-lookup"><span data-stu-id="a648e-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="a648e-153">何もここで編集するのには。</span><span class="sxs-lookup"><span data-stu-id="a648e-153">There's nothing here to edit.</span></span> <span data-ttu-id="a648e-154">行う必要があるすべては、ことができるようにするため、右のムービーを削除していることを確認して、ムービーの詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="a648e-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="a648e-155">マークアップには、ユーザーが、ムービーの一覧ページに戻りますできるリンクが既に含まれています。</span><span class="sxs-lookup"><span data-stu-id="a648e-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="a648e-156">同様、 *EditMovie*  ページで、選択したムービーの ID が非表示のフィールドに格納します。</span><span class="sxs-lookup"><span data-stu-id="a648e-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="a648e-157">(として渡されるページに、まずクエリ文字列値です。)`Html.ValidationSummary`妥当性確認エラーを表示するための呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="a648e-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="a648e-158">ここでは、エラーは、ページに映画の ID が渡されなかったことまたはムービー ID が無効である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a648e-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="a648e-159">この状況でビデオを選択せずこのページのユーザーが実行された場合に発生する可能性があります、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="a648e-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="a648e-160">ボタンのキャプション**削除ムービー**に設定されている、name 属性と`buttonDelete`です。</span><span class="sxs-lookup"><span data-stu-id="a648e-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="a648e-161">`name`属性は、フォームを送信するボタンを識別するコードで使用されます。</span><span class="sxs-lookup"><span data-stu-id="a648e-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="a648e-162">1)、ムービーの詳細を読んで、ページが最初に表示されるときにコードを記述し、2) 実際には、ユーザーがボタンをクリックしたときに、ムービーを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a648e-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="a648e-163">1 つのムービーの読み取りにコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a648e-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="a648e-164">上部にある、 *DeleteMovie.cshtml*  ページで、次のコード ブロックを追加します。</span><span class="sxs-lookup"><span data-stu-id="a648e-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="a648e-165">このマークアップは、対応するコードと同じ、 *EditMovie*ページ。</span><span class="sxs-lookup"><span data-stu-id="a648e-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="a648e-166">クエリ文字列からムービー ID を取得し、データベースからレコードを読み取るための ID を使用します。</span><span class="sxs-lookup"><span data-stu-id="a648e-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="a648e-167">コードには検証テストが含まれています (`IsInt()`と`row != null`) ページに渡されるムービー ID が有効であるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="a648e-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="a648e-168">このコードは、最初に実行、ページにのみ実行する必要がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a648e-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="a648e-169">ユーザーがクリックしたときに、ムービーのレコードをデータベースからを再読み込みしたくない、**削除ムービー**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a648e-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="a648e-170">そのため、コードと表示されているテスト内ではビデオを読めない`if(!IsPost)` &mdash; 、*かどうか、要求は post 操作 (フォームの送信)*です。</span><span class="sxs-lookup"><span data-stu-id="a648e-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="a648e-171">選択したムービーを削除するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a648e-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="a648e-172">ユーザーがボタンをクリックしたときに、ムービーを削除するには、次のコードの右中かっこの内側を追加、`@`ブロック。</span><span class="sxs-lookup"><span data-stu-id="a648e-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="a648e-173">このコードは、既存のレコードを更新するためのコードに似た簡単です。</span><span class="sxs-lookup"><span data-stu-id="a648e-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="a648e-174">コードは、基本的には、SQL を実行`Delete`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="a648e-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="a648e-175">同様、 *EditMovie*  ページで、コードは、`if(IsPost)`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="a648e-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="a648e-176">このとき、`if()`条件は、もう少し複雑になります。</span><span class="sxs-lookup"><span data-stu-id="a648e-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="a648e-177">ここで、2 つの条件があります。</span><span class="sxs-lookup"><span data-stu-id="a648e-177">There are two conditions here.</span></span> <span data-ttu-id="a648e-178">1 つあるページが送信される、前に説明したようには、 &mdash; `if(IsPost)`です。</span><span class="sxs-lookup"><span data-stu-id="a648e-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="a648e-179">2 番目の条件が`!Request["buttonDelete"].IsEmpty()`、要求という名前のオブジェクトが含まれることを意味`buttonDelete`です。</span><span class="sxs-lookup"><span data-stu-id="a648e-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="a648e-180">確かに、どのボタン フォームの送信のテストの間接的な手段を勧めします。</span><span class="sxs-lookup"><span data-stu-id="a648e-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="a648e-181">フォームには、複数の submit ボタンが含まれる、要求でクリックしてされたボタンの名前のみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a648e-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="a648e-182">そのため、論理的には、要求で特定のボタンの名前が表示されます&mdash;で述べたように、コードでは、そのボタンが空でない場合、または&mdash;フォームを送信するボタンです。</span><span class="sxs-lookup"><span data-stu-id="a648e-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="a648e-183">`&&`演算子手段「と」(論理 AND)。</span><span class="sxs-lookup"><span data-stu-id="a648e-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="a648e-184">そのため、全体`if`条件がしています.</span><span class="sxs-lookup"><span data-stu-id="a648e-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="a648e-185">*この要求は、post (初回要求されません)*</span><span class="sxs-lookup"><span data-stu-id="a648e-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="a648e-186">AND</span><span class="sxs-lookup"><span data-stu-id="a648e-186">AND</span></span>  
  
<span data-ttu-id="a648e-187">`buttonDelete`*ボ**タンがフォームを送信するボタンをクリックします。*</span><span class="sxs-lookup"><span data-stu-id="a648e-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="a648e-188">そのため (実際には、このページ) でのこのフォームにボタンが 1 つだけが含まれていますの追加のテスト`buttonDelete`は技術的には必要ありません。</span><span class="sxs-lookup"><span data-stu-id="a648e-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="a648e-189">それでもは、データが完全に削除する操作を実行しようとしています。</span><span class="sxs-lookup"><span data-stu-id="a648e-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="a648e-190">したがって、ユーザーが要求して明示的に場合にのみ、操作を実行することができるだけに必ずたいです。</span><span class="sxs-lookup"><span data-stu-id="a648e-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="a648e-191">たとえば、このページを後で展開し、その他のボタンを追加します。</span><span class="sxs-lookup"><span data-stu-id="a648e-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="a648e-192">場合にのみのムービーを削除するコードの実行後も、 `buttonDelete` button がクリックしてされました。</span><span class="sxs-lookup"><span data-stu-id="a648e-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="a648e-193">同様、 *EditMovie*  ページで、非表示のフィールドから ID を取得して、SQL コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a648e-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="a648e-194">構文、`Delete`ステートメントは。</span><span class="sxs-lookup"><span data-stu-id="a648e-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="a648e-195">含めることが重要、`WHERE`句と、id です。</span><span class="sxs-lookup"><span data-stu-id="a648e-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="a648e-196">WHERE 句を省略すると*テーブル内のすべてのレコードが削除されます*です。</span><span class="sxs-lookup"><span data-stu-id="a648e-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="a648e-197">説明したよう、SQL コマンドにプレース ホルダーを使用して ID 値を渡します。</span><span class="sxs-lookup"><span data-stu-id="a648e-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="a648e-198">ムービーの削除プロセスのテスト</span><span class="sxs-lookup"><span data-stu-id="a648e-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="a648e-199">これで次のようにテストすることができます。</span><span class="sxs-lookup"><span data-stu-id="a648e-199">Now you can test.</span></span> <span data-ttu-id="a648e-200">実行、*映画* ページで、をクリックして**削除**ムービーの横にあります。</span><span class="sxs-lookup"><span data-stu-id="a648e-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="a648e-201">ときに、 *DeleteMovie*ページが表示されたら、をクリックして**削除ムービー**です。</span><span class="sxs-lookup"><span data-stu-id="a648e-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![ムービーの削除 ボタンが強調表示されているとムービーのページを削除します。](deleting-data/_static/image4.png)

<span data-ttu-id="a648e-203">ボタンをクリックすると、コードは、ムービーを削除し、ムービーの一覧を戻します。</span><span class="sxs-lookup"><span data-stu-id="a648e-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="a648e-204">削除したムービーを検索することがありますが削除されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a648e-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="a648e-205">次へ直近の見通し</span><span class="sxs-lookup"><span data-stu-id="a648e-205">Coming Up Next</span></span>

<span data-ttu-id="a648e-206">次のチュートリアルでは、一般的な外観とレイアウト、サイト上のすべてのページを指定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a648e-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="a648e-207">ムービーのページ (更新と削除のリンク) の完全な一覧</span><span class="sxs-lookup"><span data-stu-id="a648e-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="a648e-208">DeleteMovie ページの完全な一覧</span><span class="sxs-lookup"><span data-stu-id="a648e-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="a648e-209">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="a648e-209">Additional Resources</span></span>

- [<span data-ttu-id="a648e-210">Razor 構文を使用して ASP.NET Web プログラミングの概要</span><span class="sxs-lookup"><span data-stu-id="a648e-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="a648e-211">[SQL の DELETE ステートメント](http://www.w3schools.com/sql/sql_delete.asp)W3Schools サイト</span><span class="sxs-lookup"><span data-stu-id="a648e-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a648e-212">[前へ](updating-data.md)
[次へ](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="a648e-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
