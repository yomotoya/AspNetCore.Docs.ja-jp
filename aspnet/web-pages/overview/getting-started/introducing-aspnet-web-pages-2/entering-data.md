---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: ASP.NET Web Pages の概要 - データベースのデータを入力するフォームを使用して |Microsoft Docs
author: tfitzmac
description: このチュートリアルでは、入力フォームを作成し、取得するフォームからデータベース テーブルに ASP.NET Web Pages (... を使用するときにデータを入力する方法
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: 313fd6ff70af540d4dd749734f50e3fc24be0d29
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835762"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a><span data-ttu-id="26cdb-103">ASP.NET Web Pages の概要 - フォームを使用してデータベースのデータを入力します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-103">Introducing ASP.NET Web Pages - Entering Database Data by Using Forms</span></span>
====================
<span data-ttu-id="26cdb-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="26cdb-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="26cdb-105">このチュートリアルでは、入力フォームを作成し、取得するフォームからデータベース テーブルに ASP.NET Web Pages (Razor) を使用するときにデータを入力する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-105">This tutorial shows you how to create an entry form and then enter the data that you get from the form into a database table when you use ASP.NET Web Pages (Razor).</span></span> <span data-ttu-id="26cdb-106">を通じてシリーズを完了したと想定して[基本の HTML フォームの ASP.NET Web Pages で](https://go.microsoft.com/fwlink/?LinkId=251581)します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-106">It assumes you have completed the series through [Basics of HTML Forms in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581).</span></span>
> 
> <span data-ttu-id="26cdb-107">学習内容。</span><span class="sxs-lookup"><span data-stu-id="26cdb-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="26cdb-108">入力フォームを処理する方法の詳細。</span><span class="sxs-lookup"><span data-stu-id="26cdb-108">More about how to process entry forms.</span></span>
> - <span data-ttu-id="26cdb-109">データベースのデータ (挿入) を追加する方法。</span><span class="sxs-lookup"><span data-stu-id="26cdb-109">How to add (insert) data in a database.</span></span>
> - <span data-ttu-id="26cdb-110">ユーザーがフォーム (ユーザー入力を検証する方法) で必要な値を入力したかどうかを確認する方法。</span><span class="sxs-lookup"><span data-stu-id="26cdb-110">How to make sure that users have entered a required value in a form (how to validate user input).</span></span>
> - <span data-ttu-id="26cdb-111">検証エラーを表示する方法。</span><span class="sxs-lookup"><span data-stu-id="26cdb-111">How to display validation errors.</span></span>
> - <span data-ttu-id="26cdb-112">現在のページから別のページに移動する方法。</span><span class="sxs-lookup"><span data-stu-id="26cdb-112">How to jump to another page from the current page.</span></span>
>   
> 
> <span data-ttu-id="26cdb-113">説明した機能/テクノロジ:</span><span class="sxs-lookup"><span data-stu-id="26cdb-113">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="26cdb-114">`Database.Execute` メソッド。</span><span class="sxs-lookup"><span data-stu-id="26cdb-114">The `Database.Execute` method.</span></span>
> - <span data-ttu-id="26cdb-115">SQL`Insert Into`ステートメント</span><span class="sxs-lookup"><span data-stu-id="26cdb-115">The SQL `Insert Into` statement</span></span>
> - <span data-ttu-id="26cdb-116">`Validation`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="26cdb-116">The `Validation` helper.</span></span>
> - <span data-ttu-id="26cdb-117">`Response.Redirect` メソッド。</span><span class="sxs-lookup"><span data-stu-id="26cdb-117">The `Response.Redirect` method.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="26cdb-118">構築します</span><span class="sxs-lookup"><span data-stu-id="26cdb-118">What You'll Build</span></span>

<span data-ttu-id="26cdb-119">チュートリアルでは、前のデータベースを作成する方法を示しましたが、WebMatrix での作業で直接、データベースを編集することによってデータベースのデータを入力した、**データベース**ワークスペース。</span><span class="sxs-lookup"><span data-stu-id="26cdb-119">In the tutorial earlier that showed you how to create a database, you entered database data by editing the database directly in WebMatrix, working in the **Database** workspace.</span></span> <span data-ttu-id="26cdb-120">ほとんどのアプリでないが、データベースにデータを格納する実用的な方法。</span><span class="sxs-lookup"><span data-stu-id="26cdb-120">In most apps, that's not a practical way to put data into the database, though.</span></span> <span data-ttu-id="26cdb-121">したがって、このチュートリアルでは、または他のデータを入力し、データベースに保存できる web ベースのインターフェイスを作成します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-121">So in this tutorial, you'll create a web-based interface that lets you or anyone enter data and save it to the database.</span></span>

<span data-ttu-id="26cdb-122">新しいムービーを入力するページを作成します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-122">You'll create a page where you can enter new movies.</span></span> <span data-ttu-id="26cdb-123">ページは、ムービーのタイトル、ジャンル、および年を入力するフィールド (テキスト ボックス) が含まれる入力フォームが含まれます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-123">The page will contain an entry form that has fields (text boxes) where you can enter a movie title, genre, and year.</span></span> <span data-ttu-id="26cdb-124">ページは、このページのようになります。</span><span class="sxs-lookup"><span data-stu-id="26cdb-124">The page will look like this page:</span></span>

![ブラウザーで映画の追加 ページ](entering-data/_static/image1.png)

<span data-ttu-id="26cdb-126">テキスト ボックスに HTML なります`<input>`などこのマークアップの要素を検索します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-126">The text boxes will be HTML `<input>` elements that will look like this markup:</span></span>

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a><span data-ttu-id="26cdb-127">エントリの基本的なフォームを作成します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-127">Creating the Basic Entry Form</span></span>

<span data-ttu-id="26cdb-128">という名前のページを作成する*AddMovie.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-128">Create a page named *AddMovie.cshtml*.</span></span>

<span data-ttu-id="26cdb-129">新機能を次のマークアップ ファイルで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-129">Replace what's in the file with the following markup.</span></span> <span data-ttu-id="26cdb-130">他のものを上書き間もなく、上部にあるコード ブロックを追加します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-130">Overwrite everything; you'll add a code block at the top shortly.</span></span>

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

<span data-ttu-id="26cdb-131">この例では、フォームを作成する一般的な HTML を示します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-131">This example shows typical HTML for creating a form.</span></span> <span data-ttu-id="26cdb-132">使用して`<input>`テキスト ボックスおよび [送信] ボタンの要素。</span><span class="sxs-lookup"><span data-stu-id="26cdb-132">It uses `<input>` elements for the text boxes and for the submit button.</span></span> <span data-ttu-id="26cdb-133">テキスト ボックスのキャプションが標準を使用して作成された`<label>`要素。</span><span class="sxs-lookup"><span data-stu-id="26cdb-133">The captions for the text boxes are created by using standard `<label>` elements.</span></span> <span data-ttu-id="26cdb-134">`<fieldset>`と`<legend>`要素がフォーム上の便利なボックスを配置します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-134">The `<fieldset>` and `<legend>` elements put a nice box around the form.</span></span>

<span data-ttu-id="26cdb-135">このページで、`<form>`要素を使用して`post`の値として、`method`属性。</span><span class="sxs-lookup"><span data-stu-id="26cdb-135">Notice that in this page, the `<form>` element uses `post` as the value for the `method` attribute.</span></span> <span data-ttu-id="26cdb-136">前のチュートリアルで使用するフォームを作成した、`get`メソッド。</span><span class="sxs-lookup"><span data-stu-id="26cdb-136">In the previous tutorial, you created a form that used the `get` method.</span></span> <span data-ttu-id="26cdb-137">フォームでは、値をサーバーに送信される、要求に変更をしないようにしなかったので、そうでした。</span><span class="sxs-lookup"><span data-stu-id="26cdb-137">That was correct, because although the form submitted values to the server, the request did not make any changes.</span></span> <span data-ttu-id="26cdb-138">もしすべては、さまざまな方法でデータをフェッチをしました。</span><span class="sxs-lookup"><span data-stu-id="26cdb-138">All it did was fetch data in different ways.</span></span> <span data-ttu-id="26cdb-139">ただし、このページに*は*変更-新しいデータベース レコードを追加しようとしています。</span><span class="sxs-lookup"><span data-stu-id="26cdb-139">However, in this page you *will* make changes—you're going to add new database records.</span></span> <span data-ttu-id="26cdb-140">したがって、このフォームを使用する必要があります、`post`メソッド。</span><span class="sxs-lookup"><span data-stu-id="26cdb-140">Therefore, this form should use the `post` method.</span></span> <span data-ttu-id="26cdb-141">(間の違いについての詳細は`GET`と`POST`、操作を参照してください、[GET、POST、および HTTP 動詞の安全性](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety)前のチュートリアルの補足記事「)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-141">(For more about the difference between `GET` and `POST` operations, see the[GET, POST, and HTTP Verb Safety](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) sidebar in the previous tutorial.)</span></span>

<span data-ttu-id="26cdb-142">各テキスト ボックスにメモを`name`要素 (`title`、 `genre`、 `year`)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-142">Note that each text box has a `name` element (`title`, `genre`, `year`).</span></span> <span data-ttu-id="26cdb-143">前のチュートリアルで説明するようにこれらの名前は、後で、ユーザーの入力を取得できるように、これらの名前を必要なため重要です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-143">As you saw in the previous tutorial, these names are important because you must have those names so you can get the user's input later.</span></span> <span data-ttu-id="26cdb-144">任意の名前を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-144">You can use any names.</span></span> <span data-ttu-id="26cdb-145">お勧めする際に役立つ意味のある名を使用してどのようなデータを使用しているを注意してください。</span><span class="sxs-lookup"><span data-stu-id="26cdb-145">It's helpful to use meaningful names that help you remember what data you're working with.</span></span>

<span data-ttu-id="26cdb-146">`value`の各属性`<input>`要素には Razor コードが含まれています (たとえば、 `Request.Form["title"]`)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-146">The `value` attribute of each `<input>` element contains a bit of Razor code (for example, `Request.Form["title"]`).</span></span> <span data-ttu-id="26cdb-147">フォームが送信された後は、このトリックのバージョンを (ある場合)、テキスト ボックスに入力された値を保持するために、前のチュートリアルで学習します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-147">You learned a version of this trick in the previous tutorial to preserve the value entered into the text box (if any) after the form has been submitted.</span></span>

## <a name="getting-the-form-values"></a><span data-ttu-id="26cdb-148">フォーム値を取得します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-148">Getting the Form Values</span></span>

<span data-ttu-id="26cdb-149">次に、フォームを処理するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-149">Next, you add code that processes the form.</span></span> <span data-ttu-id="26cdb-150">アウトラインで、次を行います。</span><span class="sxs-lookup"><span data-stu-id="26cdb-150">In outline, you'll do the following:</span></span>

1. <span data-ttu-id="26cdb-151">ページがポストされているかどうかを確認 (送信) します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-151">Check whether the page is being posted (was submitted).</span></span> <span data-ttu-id="26cdb-152">コードを実行するページが最初に実行されるときではなく、ユーザーは、ボタンをクリックした場合にのみです。</span><span class="sxs-lookup"><span data-stu-id="26cdb-152">You want to your code to run only when users have clicked the button, not when the page first runs.</span></span>
2. <span data-ttu-id="26cdb-153">ユーザーがテキスト ボックスに入力した値を取得します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-153">Get the values that the user entered into the text boxes.</span></span> <span data-ttu-id="26cdb-154">この場合、形式が使用されているので、`POST`からフォーム値を取得する、動詞、`Request.Form`コレクション。</span><span class="sxs-lookup"><span data-stu-id="26cdb-154">In this case, because the form is using the `POST` verb, you get the form values from the `Request.Form` collection.</span></span>
3. <span data-ttu-id="26cdb-155">新しいレコードとして値を挿入、*映画*データベース テーブル。</span><span class="sxs-lookup"><span data-stu-id="26cdb-155">Insert the values as a new record in the *Movies* database table.</span></span>

<span data-ttu-id="26cdb-156">ファイルの上部にある次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-156">At the top of the file, add the following code:</span></span>

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

<span data-ttu-id="26cdb-157">変数を作成する最初の数行 (`title`、 `genre`、および`year`) テキスト ボックスから値を保持します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-157">The first few lines create variables (`title`, `genre`, and `year`) to hold the values from the text boxes.</span></span> <span data-ttu-id="26cdb-158">行`if(IsPost)`により、変数が設定されていることを確認して*のみ*ユーザーがクリックすると、**映画の追加**ボタン-は、ときに、フォームが投稿されました。</span><span class="sxs-lookup"><span data-stu-id="26cdb-158">The line `if(IsPost)` makes sure that the variables are set *only* when users click the **Add Movie** button — that is, when the form has been posted.</span></span>

<span data-ttu-id="26cdb-159">ような式を使用して、テキスト ボックスの値を取得する前のチュートリアルで説明するように`Request.Form["name"]`ここで、*名前*の名前を指定します、`<input>`要素。</span><span class="sxs-lookup"><span data-stu-id="26cdb-159">As you saw in an earlier tutorial, you get the value of a text box by using an expression like `Request.Form["name"]`, where *name* is the name of the `<input>` element.</span></span>

<span data-ttu-id="26cdb-160">変数の名前 (`title`、 `genre`、および`year`) は任意です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-160">The names of the variables (`title`, `genre`, and `year`) are arbitrary.</span></span> <span data-ttu-id="26cdb-161">などの名前に割り当てた`<input>`要素、好きなデザインに呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-161">Like the names that you assign to `<input>` elements, you can call them anything you like.</span></span> <span data-ttu-id="26cdb-162">(変数の名前の名前属性と一致する必要はありません`<input>`フォーム上の要素)。同様、`<input>`要素が含まれているデータが反映される変数名を使用することをお勧めは。</span><span class="sxs-lookup"><span data-stu-id="26cdb-162">(The names of the variables don't have to match the name attributes of `<input>` elements on the form.) But as with the `<input>` elements, it's a good idea to use variable names that reflect the data that they contain.</span></span> <span data-ttu-id="26cdb-163">コードを記述するときに一貫した名前やすくを使用しているデータを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="26cdb-163">When you write code, consistent names make it easier for you to remember what data you're working with.</span></span>

## <a name="adding-data-to-the-database"></a><span data-ttu-id="26cdb-164">データベースへのデータの追加</span><span class="sxs-lookup"><span data-stu-id="26cdb-164">Adding Data to the Database</span></span>

<span data-ttu-id="26cdb-165">コードでブロックするだけで、追加された*内*右中かっこ ( `}` ) の`if`ブロック (だけでなく、コード ブロック) 内で、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-165">In the code block you just added, just *inside* the closing brace ( `}` ) of the `if` block (not just inside the code block), add the following code:</span></span>

[!code-csharp[Main](entering-data/samples/sample3.cs)]

<span data-ttu-id="26cdb-166">この例では、データをフェッチしたり表示を前のチュートリアルで使用したコードに似ています。</span><span class="sxs-lookup"><span data-stu-id="26cdb-166">This example is similar to the code you used in a previous tutorial to fetch and display data.</span></span> <span data-ttu-id="26cdb-167">始まる行`db =`以前のように、データベースと次の行として定義します SQL ステートメントでは、もう一度開く前に説明しました。</span><span class="sxs-lookup"><span data-stu-id="26cdb-167">The line that starts with `db =` opens the database, like before, and the next line defines a SQL statement, again as you saw before.</span></span> <span data-ttu-id="26cdb-168">ただし、この時間を定義する SQL`Insert Into`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="26cdb-168">However, this time it defines a SQL `Insert Into` statement.</span></span> <span data-ttu-id="26cdb-169">次の例の一般的な構文を示しています、`Insert Into`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="26cdb-169">The following example shows the general syntax of the `Insert Into` statement:</span></span>

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

<span data-ttu-id="26cdb-170">つまりに挿入するテーブルには、挿入する列を一覧表示し、および挿入する値を一覧表示を指定します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-170">In other words, you specify the table to insert into, then list the columns to insert into, and then list the values to insert.</span></span> <span data-ttu-id="26cdb-171">(する前に述べたように、SQL は、大文字小文字を区別しない、一部の人には、コマンドを読みやすくキーワードが大文字に変換)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-171">(As noted before, SQL is not case sensitive but some people capitalize the keywords to make it easier to read the command.)</span></span>

<span data-ttu-id="26cdb-172">挿入する列は、コマンドに既に表示されて、`(Title, Genre, Year)`します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-172">The columns that you're inserting into are already listed in the command — `(Title, Genre, Year)`.</span></span> <span data-ttu-id="26cdb-173">興味深い部分にテキスト ボックスから値を取得する方法、`VALUES`コマンドの一部です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-173">The interesting part is how you get the values from the text boxes into the `VALUES` part of the command.</span></span> <span data-ttu-id="26cdb-174">実際の値の代わりに`@0`、 `@1`、および`@2`、プレース ホルダーはもちろんです。</span><span class="sxs-lookup"><span data-stu-id="26cdb-174">Instead of actual values, you see `@0`, `@1`, and `@2`, which are of course placeholders.</span></span> <span data-ttu-id="26cdb-175">コマンドを実行すると (上、`db.Execute`行)、テキスト ボックスから取得した値を渡します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-175">When you run the command (on the `db.Execute` line), you pass the values that you got from the text boxes.</span></span>

<span data-ttu-id="26cdb-176">**重要**。</span><span class="sxs-lookup"><span data-stu-id="26cdb-176">**Important!**</span></span> <span data-ttu-id="26cdb-177">SQL ステートメント内のユーザーがオンラインに入力したデータが含まれることが唯一の方法は、次に示すよう、プレース ホルダーを使用する、(`VALUES(@0, @1, @2)`)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-177">Remember that the only way you should ever include data entered online by a user in a SQL statement is to use placeholders, as you see here (`VALUES(@0, @1, @2)`).</span></span> <span data-ttu-id="26cdb-178">SQL ステートメントにユーザー入力を連結する場合を手動で開く、SQL インジェクション攻撃をで説明したよう[フォームの基本 ASP.NET Web Pages で](https://go.microsoft.com/fwlink/?LinkId=251581)(前のチュートリアル)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-178">If you concatenate user input into a SQL statement, you open yourself to a SQL injection attack, as explained in [Form Basics in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581) (the previous tutorial).</span></span>

<span data-ttu-id="26cdb-179">まだ内、`if`ブロックで、次の行を追加、`db.Execute`行。</span><span class="sxs-lookup"><span data-stu-id="26cdb-179">Still inside the `if` block, add the following line after the `db.Execute` line:</span></span>

[!code-css[Main](entering-data/samples/sample4.css)]

<span data-ttu-id="26cdb-180">この行を (リダイレクト) のジャンプに新しいムービーは、データベースに挿入されていますが後、*映画*ページ入力したムービーを表示できるようにします。</span><span class="sxs-lookup"><span data-stu-id="26cdb-180">After the new movie has been inserted into the database, this line jumps you (redirects) to the *Movies* page so you can see the movie you just entered.</span></span> <span data-ttu-id="26cdb-181">`~` 「Web サイトのルートです」を意味する演算子。</span><span class="sxs-lookup"><span data-stu-id="26cdb-181">The `~` operator means "root of the website."</span></span> <span data-ttu-id="26cdb-182">(、`~`演算子は一般的に HTML ではなく、ASP.NET ページでのみ動作します)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-182">(The `~` operator works only in ASP.NET pages, not in HTML generally.)</span></span>

<span data-ttu-id="26cdb-183">完全なコード ブロックは、この例のようになります。</span><span class="sxs-lookup"><span data-stu-id="26cdb-183">The complete code block looks like this example:</span></span>

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a><span data-ttu-id="26cdb-184">(これまで) の挿入 コマンドをテストします。</span><span class="sxs-lookup"><span data-stu-id="26cdb-184">Testing the Insert Command (So Far)</span></span>

<span data-ttu-id="26cdb-185">まだ、完了していませんをテストする良い機会です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-185">You're not done yet, but now is a good time to test.</span></span>

<span data-ttu-id="26cdb-186">WebMatrix 内のファイルのツリー ビューで右クリックし、 *AddMovie.cshtml*ページし、クリックして**ブラウザーで起動**します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-186">In the tree view of files in WebMatrix, right-click the *AddMovie.cshtml* page and then click **Launch in browser**.</span></span>

![ブラウザーで映画の追加 ページ](entering-data/_static/image2.png)

<span data-ttu-id="26cdb-188">(ブラウザーの別のページと最終的する場合は、URL があることを確認してください`http://localhost:nnnnn/AddMovie`) ここで、 *nnnnn*を使用するポート番号です)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-188">(If you end up with a different page in the browser, make sure that the URL is `http://localhost:nnnnn/AddMovie`), where *nnnnn* is the port number that you're using.)</span></span>

<span data-ttu-id="26cdb-189">エラー ページを取得しましたか。</span><span class="sxs-lookup"><span data-stu-id="26cdb-189">Did you get an error page?</span></span> <span data-ttu-id="26cdb-190">そうである場合は、よくお読みし、コードを前に示したがどのような正確に見えることかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-190">If so, read it carefully and make sure that the code looks exactly what was listed earlier.</span></span>

<span data-ttu-id="26cdb-191">ムービーをフォームに入力&mdash;たとえば、"市民 Kane"、「ドラマ」および「1941」を使用します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-191">Enter a movie in the form &mdash; for example, use "Citizen Kane", "Drama", and "1941".</span></span> <span data-ttu-id="26cdb-192">(またはすべて)。クリックして**追加ムービー**します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-192">(Or whatever.) Then click **Add Movie**.</span></span>

<span data-ttu-id="26cdb-193">すべてうまくいけば場合に、リダイレクトされます、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="26cdb-193">If all goes well, you're redirected to the *Movies* page.</span></span> <span data-ttu-id="26cdb-194">新しいムービーが表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-194">Make sure that your new movie is listed.</span></span>

![新しく表示ムービー ページは、ムービーを追加します。](entering-data/_static/image3.png)

## <a name="validating-user-input"></a><span data-ttu-id="26cdb-196">ユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="26cdb-196">Validating User Input</span></span>

<span data-ttu-id="26cdb-197">戻り、 *AddMovie*ページ、または、もう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-197">Go back to the *AddMovie* page, or run it again.</span></span> <span data-ttu-id="26cdb-198">この時間がもう 1 つのムービーをタイトルのみを入力します。&mdash;たとえば、"the で Rain の ' を家する"を入力します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-198">Enter another movie, but this time, enter only the title &mdash; for example, enter "Singin' in the Rain".</span></span> <span data-ttu-id="26cdb-199">クリックして**追加ムービー**します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-199">Then click **Add Movie**.</span></span>

<span data-ttu-id="26cdb-200">リダイレクトされます、*映画*ページをもう一度です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-200">You're redirected to the *Movies* page again.</span></span> <span data-ttu-id="26cdb-201">新しいビデオを見つけることができますは完了しません。</span><span class="sxs-lookup"><span data-stu-id="26cdb-201">You can find the new movie, but it's incomplete.</span></span>

![ムービーをページが表示された新しいムービーにはいくつかの値がありません。](entering-data/_static/image4.png)

<span data-ttu-id="26cdb-203">作成したときに、*映画*テーブル、明示的にそれほどある null のフィールドがありませんでしたが。</span><span class="sxs-lookup"><span data-stu-id="26cdb-203">When you created the *Movies* table, you explicitly said that none of the fields could be null.</span></span> <span data-ttu-id="26cdb-204">ここで新しい映画は、入力フォームがあり、空白のフィールドを終了しています。</span><span class="sxs-lookup"><span data-stu-id="26cdb-204">Here you have an entry form for new movies, and you're leaving fields blank.</span></span> <span data-ttu-id="26cdb-205">エラーです。</span><span class="sxs-lookup"><span data-stu-id="26cdb-205">That's an error.</span></span>

<span data-ttu-id="26cdb-206">ここでは、データベースを生成していない実際に (または*スロー*) エラー。</span><span class="sxs-lookup"><span data-stu-id="26cdb-206">In this case, the database didn't actually raise (or *throw*) an error.</span></span> <span data-ttu-id="26cdb-207">そのため、ジャンルまたは年を指定しなかった内のコード、 *AddMovie*ページには、いわゆるとしてそれらの値が扱われます*空の文字列*。</span><span class="sxs-lookup"><span data-stu-id="26cdb-207">You didn't supply a genre or year, so the code in the *AddMovie* page treated those values as so-called *empty strings*.</span></span> <span data-ttu-id="26cdb-208">ときに、SQL`Insert Into`コマンドが実行された、ジャンル、年フィールドは、有用なデータがありませんでしたが、null でした。</span><span class="sxs-lookup"><span data-stu-id="26cdb-208">When the SQL `Insert Into` command ran, the genre and year fields didn't have useful data in them, but they weren't null.</span></span>

<span data-ttu-id="26cdb-209">当然ながら、ユーザーがデータベースに半空ムービー情報を入力できるようにしたくないです。</span><span class="sxs-lookup"><span data-stu-id="26cdb-209">Obviously, you don't want to let users enter half-empty movie information into the database.</span></span> <span data-ttu-id="26cdb-210">このソリューションでは、ユーザーの入力を検証します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-210">The solution is to validate the user's input.</span></span> <span data-ttu-id="26cdb-211">最初に、検証だけになります、ユーザーにすべてのフィールドの値が入力したことを確認します (空の文字列を含むなしには、)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-211">Initially, the validation will simply make sure that the user has entered a value for all of the fields (that is, that none of them contains an empty string).</span></span>

> [!TIP]
> 
> <span data-ttu-id="26cdb-212">**Null および空の文字列**</span><span class="sxs-lookup"><span data-stu-id="26cdb-212">**Null and Empty Strings**</span></span>
> 
> <span data-ttu-id="26cdb-213">"No value"の概念を区別してプログラミングでは、</span><span class="sxs-lookup"><span data-stu-id="26cdb-213">In programming, there's a distinction between different notions of "no value."</span></span> <span data-ttu-id="26cdb-214">一般に、値は*null*しない設定またはした任意の方法で初期化される場合。</span><span class="sxs-lookup"><span data-stu-id="26cdb-214">In general, a value is *null* if it has never been set or initialized in any way.</span></span> <span data-ttu-id="26cdb-215">これに対し、文字データ (文字列) が必要とする変数に設定することができます、*空の文字列*します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-215">In contrast, a variable that expects character data (strings) can be set to an *empty string*.</span></span> <span data-ttu-id="26cdb-216">その場合は、値が null です。これはだけ明示的に設定されて文字の長さが 0 の文字列にします。</span><span class="sxs-lookup"><span data-stu-id="26cdb-216">In that case, the value is not null; it's just been explicitly set to a string of characters whose length is zero.</span></span> <span data-ttu-id="26cdb-217">これら 2 つのステートメントでは、差分を表示します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-217">These two statements show the difference:</span></span>
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> <span data-ttu-id="26cdb-218">より複雑に少しが重要な点は`null`未確定の状態の並べ替えを表します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-218">It's a little more complicated than that, but the important point is that `null` represents a sort of undetermined state.</span></span>
> 
> <span data-ttu-id="26cdb-219">値が null の場合と、空の文字列だけが正確に理解しておく必要です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-219">Now and then it's important to understand exactly when a value is null and when it's just an empty string.</span></span> <span data-ttu-id="26cdb-220">コードで、 *AddMovie*  ページを使用して、テキスト ボックスの値を取得する`Request.Form["title"]`という具合です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-220">In the code for the *AddMovie* page, you get the values of the text boxes by using `Request.Form["title"]` and so on.</span></span> <span data-ttu-id="26cdb-221">ときに、ページ最初の実行 (前に、ボタンをクリックすると)、値の`Request.Form["title"]`が null です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-221">When the page first runs (before you click the button), the value of `Request.Form["title"]` is null.</span></span> <span data-ttu-id="26cdb-222">フォームを送信するときに、`Request.Form["title"]`の値を取得、`title`テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="26cdb-222">But when you submit the form, `Request.Form["title"]` gets the value of the `title` text box.</span></span> <span data-ttu-id="26cdb-223">、明確ではありませんが、空のテキスト ボックスが null 以外ではないです。空の文字列だけがあります。</span><span class="sxs-lookup"><span data-stu-id="26cdb-223">It's not obvious, but an empty text box is not null; it just has an empty string in it.</span></span> <span data-ttu-id="26cdb-224">したがって、ボタンに応答コードを実行する をクリックして、`Request.Form["title"]`空の文字列が含まれる。</span><span class="sxs-lookup"><span data-stu-id="26cdb-224">So when the code runs in response to the button click, `Request.Form["title"]` has an empty string in it.</span></span>
> 
> <span data-ttu-id="26cdb-225">この区別が重要な理由</span><span class="sxs-lookup"><span data-stu-id="26cdb-225">Why is this distinction important?</span></span> <span data-ttu-id="26cdb-226">作成したときに、*映画*テーブル、明示的にそれほどある null のフィールドがありませんでしたが。</span><span class="sxs-lookup"><span data-stu-id="26cdb-226">When you created the *Movies* table, you explicitly said that none of the fields could be null.</span></span> <span data-ttu-id="26cdb-227">ここで新しい映画は、入力フォームがあり、空白のフィールドを終了しています。</span><span class="sxs-lookup"><span data-stu-id="26cdb-227">But here you have an entry form for new movies, and you're leaving fields blank.</span></span> <span data-ttu-id="26cdb-228">ジャンルまたは年の値がない新しいムービーを保存しようとしたときから不満があがるにデータベースが予測されるは。</span><span class="sxs-lookup"><span data-stu-id="26cdb-228">You would reasonably expect the database to complain when you tried to save new movies that didn't have values for genre or year.</span></span> <span data-ttu-id="26cdb-229">ポイントですが、&mdash;場合でも、これらのテキスト ボックスを空白のままにすると、値 null は空の文字列です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-229">But that's the point &mdash; even if you leave those text boxes blank, the values aren't null; they're empty strings.</span></span> <span data-ttu-id="26cdb-230">これらの列は空のデータベースに新しいムービーを保存できますしたらその結果、&mdash;いない null です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-230">As a result, you're able to save new movies to the database with these columns empty &mdash; but not null!</span></span> <span data-ttu-id="26cdb-231">&mdash; 値。</span><span class="sxs-lookup"><span data-stu-id="26cdb-231">&mdash; values.</span></span> <span data-ttu-id="26cdb-232">そのため、ユーザーがユーザーの入力の検証を行うには空の文字列を送信しないかどうかを確認する必要あります。</span><span class="sxs-lookup"><span data-stu-id="26cdb-232">Therefore, you have to make sure that users don't submit an empty string, which you can do by validating the user's input.</span></span>


### <a name="the-validation-helper"></a><span data-ttu-id="26cdb-233">検証ヘルパー</span><span class="sxs-lookup"><span data-stu-id="26cdb-233">The Validation Helper</span></span>

<span data-ttu-id="26cdb-234">ASP.NET Web ページには、ヘルパーが含まれています。 &mdash; 、`Validation`ヘルパー&mdash;ことをユーザーが要件を満たすデータを入力するかどうかを確認に使用することができます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-234">ASP.NET Web Pages includes a helper &mdash; the `Validation` helper &mdash; that you can use to make sure that users enter data that meets your requirements.</span></span> <span data-ttu-id="26cdb-235">`Validation`ヘルパーが組み込まれている ASP.NET Web ページには、NuGet では、前のチュートリアルで Gravatar ヘルパーをインストールする方法を使用してパッケージとしてインストールする必要はありませんので、ヘルパーのいずれか。</span><span class="sxs-lookup"><span data-stu-id="26cdb-235">The `Validation` helper is one of the helpers that's built in to ASP.NET Web Pages, so you don't have to install it as a package by using NuGet, the way you installed the Gravatar helper in an earlier tutorial.</span></span>

<span data-ttu-id="26cdb-236">ユーザーの入力を検証するには、次を行います。</span><span class="sxs-lookup"><span data-stu-id="26cdb-236">To validate the user's input, you'll do the following:</span></span>

- <span data-ttu-id="26cdb-237">コードを使用すると、ページ上のテキスト ボックス内の値を必要とすることを指定できます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-237">Use code to specify that you want to require values in the text boxes on the page.</span></span>
- <span data-ttu-id="26cdb-238">すべてが正しく検証する場合にのみ、ムービー情報をデータベースに追加するコードにテストを配置します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-238">Put a test into the code so that the movie information is added to the database only if everything validates properly.</span></span>
- <span data-ttu-id="26cdb-239">エラー メッセージを表示するマークアップにコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-239">Add code into the markup to display error messages.</span></span>

<span data-ttu-id="26cdb-240">コード ブロックで、 *AddMovie*  ページで、右上変数の宣言の前に、上部にある次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-240">In the code block in the *AddMovie* page, right up at the top before the variable declarations, add the following code:</span></span>

[!code-csharp[Main](entering-data/samples/sample7.cs)]

<span data-ttu-id="26cdb-241">呼び出す`Validation.RequireField`フィールドごとに 1 回 (`<input>`要素) のエントリを必要とします。</span><span class="sxs-lookup"><span data-stu-id="26cdb-241">You call `Validation.RequireField` once for each field (`<input>` element) where you want to require an entry.</span></span> <span data-ttu-id="26cdb-242">ここに表示するように、各呼び出しのカスタム エラー メッセージを追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-242">You can also add a custom error message for each call, like you see here.</span></span> <span data-ttu-id="26cdb-243">(私たちは変化がどのような配置を示すためにメッセージです)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-243">(We varied the messages just to show that you can put anything you like there.)</span></span>

<span data-ttu-id="26cdb-244">問題がある場合は、新しいムービー情報、データベースに挿入されるようにします。</span><span class="sxs-lookup"><span data-stu-id="26cdb-244">If there's a problem, you want to prevent the new movie information from being inserted into the database.</span></span> <span data-ttu-id="26cdb-245">`if(IsPost)`ブロック`&&`をテストするもう 1 つの条件を追加する (論理 AND)`Validation.IsValid()`します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-245">In the `if(IsPost)` block, use `&&` (logical AND) to add another condition that tests `Validation.IsValid()`.</span></span> <span data-ttu-id="26cdb-246">完了したら、全体`if(IsPost)`ブロックがこのコードのようになります。</span><span class="sxs-lookup"><span data-stu-id="26cdb-246">When you're done, the whole `if(IsPost)` block looks like this code:</span></span>

[!code-csharp[Main](entering-data/samples/sample8.cs)]

<span data-ttu-id="26cdb-247">使用して登録されているフィールドのいずれかの検証エラーがある場合、`Validation`ヘルパー、`Validation.IsValid`メソッドが false を返します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-247">If there's a validation error with any of the fields that you registered by using the `Validation` helper, the `Validation.IsValid` method returns false.</span></span> <span data-ttu-id="26cdb-248">その場合は、そのブロック内のコードの実行、データベースに無効なムービー エントリが挿入されないようにします。</span><span class="sxs-lookup"><span data-stu-id="26cdb-248">And in that case, none of the code in that block will run, so no invalid movie entries will be inserted into the database.</span></span> <span data-ttu-id="26cdb-249">もちろんできませんにリダイレクトと、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="26cdb-249">And of course you're not redirected to the *Movies* page.</span></span>

<span data-ttu-id="26cdb-250">検証コードを含む、完全なコード ブロックは、この例のようになります。</span><span class="sxs-lookup"><span data-stu-id="26cdb-250">The complete code block, including the validation code, now looks like this example:</span></span>

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a><span data-ttu-id="26cdb-251">検証エラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-251">Displaying Validation Errors</span></span>

<span data-ttu-id="26cdb-252">最後の手順では、すべてのエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-252">The last step is to display any error messages.</span></span> <span data-ttu-id="26cdb-253">概要については、またはその両方を表示または各検証エラーに対して個別のメッセージを表示できます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-253">You can display individual messages for each validation error, or you can display a summary, or both.</span></span> <span data-ttu-id="26cdb-254">このチュートリアルで行う両方動作を確認できるようにします。</span><span class="sxs-lookup"><span data-stu-id="26cdb-254">For this tutorial, you'll do both so that you can see how it works.</span></span>

<span data-ttu-id="26cdb-255">それぞれの横にある`<input>`に検証する要素では、呼び出し、`Html.ValidationMessage`メソッドの名前を渡すと、`<input>`に検証する要素。</span><span class="sxs-lookup"><span data-stu-id="26cdb-255">Next to each `<input>` element that you're validating, call the `Html.ValidationMessage` method and pass it the name of the `<input>` element you're validating.</span></span> <span data-ttu-id="26cdb-256">配置する、`Html.ValidationMessage`メソッドの右側に表示されるエラー メッセージをします。</span><span class="sxs-lookup"><span data-stu-id="26cdb-256">You put the `Html.ValidationMessage` method right where you want the error message to appear.</span></span> <span data-ttu-id="26cdb-257">ページが実行すると、`Html.ValidationMessage`メソッドのレンダリングを`<span>`要素の検証エラーが出ます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-257">When the page runs, the `Html.ValidationMessage` method renders a `<span>` element where the validation error will go.</span></span> <span data-ttu-id="26cdb-258">(エラーがない場合、`<span>`要素が表示されますでそのテキストがない)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-258">(If there's no error, the `<span>` element is rendered, but there's no text in it.)</span></span>

<span data-ttu-id="26cdb-259">含むように、ページのマークアップを変更、 `Html.ValidationMessage` 、3 つのメソッド`<input>` ページで、要素などの次の例。</span><span class="sxs-lookup"><span data-stu-id="26cdb-259">Change the markup in the page so that it includes an `Html.ValidationMessage` method for each of the three `<input>` elements on the page, like this example:</span></span>

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

<span data-ttu-id="26cdb-260">概要の動作を確認するも、次のマークアップを追加し、コードの直後に、`<h1>Add a Movie</h1>`ページ上の要素。</span><span class="sxs-lookup"><span data-stu-id="26cdb-260">To see how the summary works, also add the following markup and code right after the `<h1>Add a Movie</h1>` element on the page:</span></span>

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

<span data-ttu-id="26cdb-261">既定で、`Html.ValidationSummary`メソッドは、リストのすべての検証メッセージを表示します (、`<ul>`内にある要素を`<div>`要素)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-261">By default, the `Html.ValidationSummary` method displays all the validation messages in a list (a `<ul>` element that's inside a `<div>` element).</span></span> <span data-ttu-id="26cdb-262">同様、`Html.ValidationMessage`メソッド、検証の概要のマークアップが常にレンダリングされます。 エラーがない場合は、リスト項目は表示されません。</span><span class="sxs-lookup"><span data-stu-id="26cdb-262">As with the `Html.ValidationMessage` method, the markup for the validation summary is always rendered; if there are no errors, no list items are rendered.</span></span>

<span data-ttu-id="26cdb-263">使用しての代わりに検証メッセージを表示する別の方法は、概要、`Html.ValidationMessage`各フィールドに固有のエラーを表示するメソッド。</span><span class="sxs-lookup"><span data-stu-id="26cdb-263">The summary can be an alternative way to display validation messages instead of by using the `Html.ValidationMessage` method to display each field-specific error.</span></span> <span data-ttu-id="26cdb-264">または、概要と詳細の両方を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-264">Or you can use both a summary and the details.</span></span> <span data-ttu-id="26cdb-265">使用することができます、`Html.ValidationSummary`メソッドは一般的なエラーの表示を使用して、個々`Html.ValidationMessage`呼び出しの詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-265">Or you can use the `Html.ValidationSummary` method to display a generic error and then use individual `Html.ValidationMessage` calls to display details.</span></span>

<span data-ttu-id="26cdb-266">[完了] ページは、この例のようになります。</span><span class="sxs-lookup"><span data-stu-id="26cdb-266">The complete page now looks like this example:</span></span>

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

<span data-ttu-id="26cdb-267">それで完了です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-267">That's it.</span></span> <span data-ttu-id="26cdb-268">ムービーを追加する 1 つまたは複数のフィールドを除外して、ページをテストできます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-268">You can now test the page by adding a movie but leaving out one or more of the fields.</span></span> <span data-ttu-id="26cdb-269">この場合、表示、次のエラーを参照してください。</span><span class="sxs-lookup"><span data-stu-id="26cdb-269">When you do, you see the following error display:</span></span>

![検証エラー メッセージを示すビデオ ページを追加します。](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a><span data-ttu-id="26cdb-271">検証エラー メッセージのスタイル設定</span><span class="sxs-lookup"><span data-stu-id="26cdb-271">Styling the Validation Error Messages</span></span>

<span data-ttu-id="26cdb-272">エラーのメッセージがあるが本当に負って非常にうまくことを確認できます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-272">You can see that there are error messages, but they don't really stand out very well.</span></span> <span data-ttu-id="26cdb-273">ただし、エラー メッセージのスタイルを設定する簡単な方法があります。</span><span class="sxs-lookup"><span data-stu-id="26cdb-273">There's an easy way to style the error messages, though.</span></span>

<span data-ttu-id="26cdb-274">によって表示される個別のエラー メッセージのスタイルを設定する`Html.ValidationMessage`、という名前の CSS スタイル クラスを作成する`field-validation-error`します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-274">To style the individual error messages that are displayed by `Html.ValidationMessage`, create a CSS style class named `field-validation-error`.</span></span> <span data-ttu-id="26cdb-275">検証の概要の外観を定義するには、という名前の CSS スタイル クラスを作成`validation-summary-errors`です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-275">To define the look for the validation summary, create a CSS style class named `validation-summary-errors`.</span></span>

<span data-ttu-id="26cdb-276">この手法の動作を確認するには追加、`<style>`内の要素、`<head>`ページのセクション。</span><span class="sxs-lookup"><span data-stu-id="26cdb-276">To see how this technique works, add a `<style>` element inside the `<head>` section of the page.</span></span> <span data-ttu-id="26cdb-277">という名前のスタイル クラスを定義し、`field-validation-error`と`validation-summary-errors`次の規則が含まれています。</span><span class="sxs-lookup"><span data-stu-id="26cdb-277">Then define style classes named `field-validation-error` and `validation-summary-errors` that contain the following rules:</span></span>

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

<span data-ttu-id="26cdb-278">別のスタイル情報を通常おそらく配置 *.css*ファイル、わかりやすくするために配置できます ページでここでは。</span><span class="sxs-lookup"><span data-stu-id="26cdb-278">Normally you'd probably put style information into a separate *.css* file, but for simplicity you can put them in the page for now.</span></span> <span data-ttu-id="26cdb-279">(この一連のチュートリアルの後半では、個別に CSS 規則を移動します *.css*ファイルです)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-279">(Later in this tutorial set, you'll move the CSS rules to a separate *.css* file.)</span></span>

<span data-ttu-id="26cdb-280">検証エラーがある場合、`Html.ValidationMessage`メソッドのレンダリングを`<span>`要素が含まれる`class="field-validation-error"`します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-280">If there's a validation error, the `Html.ValidationMessage` method renders a `<span>` element that includes `class="field-validation-error"`.</span></span> <span data-ttu-id="26cdb-281">そのクラスのスタイル定義を追加すると、メッセージがどのように構成できます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-281">By adding a style definition for that class, you can configure what the message looks like.</span></span> <span data-ttu-id="26cdb-282">エラーがある場合、`ValidationSummary`メソッドは動的に同様に、属性をレンダリング`class="validation-summary-errors"`します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-282">If there are errors, the `ValidationSummary` method likewise dynamically renders the attribute `class="validation-summary-errors"`.</span></span>

<span data-ttu-id="26cdb-283">ページを再度実行し、意図的に 2 つのフィールドのままにします。</span><span class="sxs-lookup"><span data-stu-id="26cdb-283">Run the page again and deliberately leave out a couple of the fields.</span></span> <span data-ttu-id="26cdb-284">エラーがより顕著に表れます。</span><span class="sxs-lookup"><span data-stu-id="26cdb-284">The errors are now more noticeable.</span></span> <span data-ttu-id="26cdb-285">(実際がいる overdone、しかし、何ができるかを示すためにです)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-285">(In fact, they're overdone, but that's just to show what you can do.)</span></span>

![スタイルが設定されている検証エラーを示すビデオ ページを追加します。](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a><span data-ttu-id="26cdb-287">ムービー ページへのリンクの追加</span><span class="sxs-lookup"><span data-stu-id="26cdb-287">Adding a Link to the Movies Page</span></span>

<span data-ttu-id="26cdb-288">最後の 1 つの手順が簡単に取得するようにすること、 *AddMovie*オリジナルのムービーの一覧からページ。</span><span class="sxs-lookup"><span data-stu-id="26cdb-288">One final step is to make it convenient to get to the *AddMovie* page from the original movie listing.</span></span>

<span data-ttu-id="26cdb-289">開く、*映画*ページをもう一度です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-289">Open the *Movies* page again.</span></span> <span data-ttu-id="26cdb-290">終了後`</div>`タグに続く、`WebGrid`ヘルパーでは、次のマークアップを追加。</span><span class="sxs-lookup"><span data-stu-id="26cdb-290">After the closing `</div>` tag that follows the `WebGrid` helper, add the following markup:</span></span>

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

<span data-ttu-id="26cdb-291">ASP.NET を解釈する前に説明するように、 `~` web サイトのルートとして演算子。</span><span class="sxs-lookup"><span data-stu-id="26cdb-291">As you saw before, ASP.NET interprets the `~` operator as the root of the website.</span></span> <span data-ttu-id="26cdb-292">使用する必要はありません、`~`は演算子のマークアップを使用することが`<a href="./AddMovie">Add a movie</a>`または HTML で認識されるパスを定義するその他の何らかの方法です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-292">You don't have to use the `~` operator; you could use the markup `<a href="./AddMovie">Add a movie</a>` or some other way to define the path that HTML understands.</span></span> <span data-ttu-id="26cdb-293">`~`演算子は適切な一般的なアプローチ、Razor ページへのリンクを作成するときに、サイトをより柔軟なため、リンクに移動しても、サブフォルダーに、現在のページを移動する場合、 *AddMovie*ページ。</span><span class="sxs-lookup"><span data-stu-id="26cdb-293">But the `~` operator is a good general approach when you create links for Razor pages, because it makes the site more flexible — if you move the current page to a subfolder, the link will still go to the *AddMovie* page.</span></span> <span data-ttu-id="26cdb-294">(ことに注意して、`~`演算子でのみ機能 *.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="26cdb-294">(Remember that the `~` operator only works in *.cshtml* pages.</span></span> <span data-ttu-id="26cdb-295">ASP.NET が理解できるようにが標準の HTML ではありません)。</span><span class="sxs-lookup"><span data-stu-id="26cdb-295">ASP.NET understands it, but it's not standard HTML.)</span></span>

<span data-ttu-id="26cdb-296">完了したら、実行、*映画*ページ。</span><span class="sxs-lookup"><span data-stu-id="26cdb-296">When you're done, run the *Movies* page.</span></span> <span data-ttu-id="26cdb-297">このページのようになります。</span><span class="sxs-lookup"><span data-stu-id="26cdb-297">It will look like this page:</span></span>

!['追加映画' ページへのリンクを含むムービー ページ](entering-data/_static/image7.png)

<span data-ttu-id="26cdb-299">をクリックして、**ビデオを追加する**に移動するかどうかを確認するリンク、 *AddMovie*ページ。</span><span class="sxs-lookup"><span data-stu-id="26cdb-299">Click the **Add a movie** link to make sure that it goes to the *AddMovie* page.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="26cdb-300">次回について</span><span class="sxs-lookup"><span data-stu-id="26cdb-300">Coming Up Next</span></span>

<span data-ttu-id="26cdb-301">次のチュートリアルでは、ユーザーが既にデータベース内にあるデータを編集できるようにする方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="26cdb-301">In the next tutorial, you'll learn how to let users edit data that's already in the database.</span></span>

## <a name="complete-listing-for-addmovie-page"></a><span data-ttu-id="26cdb-302">AddMovie ページ全体を示します</span><span class="sxs-lookup"><span data-stu-id="26cdb-302">Complete Listing for AddMovie Page</span></span>

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="26cdb-303">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="26cdb-303">Additional Resources</span></span>

- [<span data-ttu-id="26cdb-304">Razor 構文を使用して ASP.NET Web プログラミングの概要</span><span class="sxs-lookup"><span data-stu-id="26cdb-304">Introduction to ASP.NET Web Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=202890)
- <span data-ttu-id="26cdb-305">[ステートメントに SQL 挿入](http://www.w3schools.com/sql/sql_insert.asp)W3Schools サイト</span><span class="sxs-lookup"><span data-stu-id="26cdb-305">[SQL INSERT INTO Statement](http://www.w3schools.com/sql/sql_insert.asp) on the W3Schools site</span></span>
- <span data-ttu-id="26cdb-306">[ページのサイトを ASP.NET Web でユーザー入力の検証](https://go.microsoft.com/fwlink/?LinkId=253002)です。</span><span class="sxs-lookup"><span data-stu-id="26cdb-306">[Validating User Input in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=253002).</span></span> <span data-ttu-id="26cdb-307">作業の詳細については、`Validation`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="26cdb-307">More information about working with the `Validation` helper.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="26cdb-308">[前へ](form-basics.md)
> [次へ](updating-data.md)</span><span class="sxs-lookup"><span data-stu-id="26cdb-308">[Previous](form-basics.md)
[Next](updating-data.md)</span></span>
