---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: "ASP.NET MVC での DropDownList ヘルパーの scaffolds 方法を調べて |Microsoft ドキュメント"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: abd9b5c09e942b966eb3eaaebe1b315c30b8e0c0
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="49827-102">ASP.NET MVC での DropDownList ヘルパーの scaffolds 方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="49827-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="49827-103">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="49827-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="49827-104">**ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*クリックしてフォルダー**コント ローラーの追加**です。</span><span class="sxs-lookup"><span data-stu-id="49827-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="49827-105">コント ローラーの名前を付けます**StoreManagerController**です。</span><span class="sxs-lookup"><span data-stu-id="49827-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="49827-106">オプションを設定、**コント ローラーの追加**ダイアログの次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="49827-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="49827-107">編集、 *StoreManager\Index.cshtml*表示および削除`AlbumArtUrl`です。</span><span class="sxs-lookup"><span data-stu-id="49827-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="49827-108">削除する`AlbumArtUrl`は、プレゼンテーションを読みやすくします。</span><span class="sxs-lookup"><span data-stu-id="49827-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="49827-109">完成したコードを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="49827-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="49827-110">開く、 *Controllers\StoreManagerController.cs*ファイルを見つけて、`Index`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="49827-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="49827-111">追加、`OrderBy`句価格別、アルバムにソートされるようにします。</span><span class="sxs-lookup"><span data-stu-id="49827-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="49827-112">完全なコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="49827-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="49827-113">価格を順に並べ替えが容易にできるようにデータベースへの変更をテストします。</span><span class="sxs-lookup"><span data-stu-id="49827-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="49827-114">編集をテストするメソッドを作成するを使用して低価格、保存したデータを最初に表示されます。</span><span class="sxs-lookup"><span data-stu-id="49827-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="49827-115">開く、 *StoreManager\Edit.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="49827-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="49827-116">凡例のタグの直後に次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="49827-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="49827-117">次のコードは、この変更のコンテキストを示しています。</span><span class="sxs-lookup"><span data-stu-id="49827-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="49827-118">`AlbumId`アルバム レコードに変更を加えるために必要なです。</span><span class="sxs-lookup"><span data-stu-id="49827-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="49827-119">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="49827-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="49827-120">選択、 **Admin**をリンクし、選択、**新規作成**新しいアルバムの作成へのリンク。</span><span class="sxs-lookup"><span data-stu-id="49827-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="49827-121">アルバム情報の保存を確認します。</span><span class="sxs-lookup"><span data-stu-id="49827-121">Verify the album information was saved.</span></span> <span data-ttu-id="49827-122">アルバムを編集して、行った変更は永続化を確認してください。</span><span class="sxs-lookup"><span data-stu-id="49827-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="49827-123">アルバム スキーマ</span><span class="sxs-lookup"><span data-stu-id="49827-123">The Album Schema</span></span>

<span data-ttu-id="49827-124">`StoreManager` MVC のスキャフォールディング メカニズムによって作成されたコント ローラーは、音楽ストア データベースのアルバムを CRUD (Create、Read、Update、Delete) のアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="49827-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="49827-125">アルバム情報のスキーマを次に示します。</span><span class="sxs-lookup"><span data-stu-id="49827-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="49827-126">`Albums`アルバム ジャンルと説明は、テーブルが格納されていないへの外部キーを格納、`Genres`テーブル。</span><span class="sxs-lookup"><span data-stu-id="49827-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="49827-127">`Genres`テーブルには、ジャンルの名前と説明が含まれています。</span><span class="sxs-lookup"><span data-stu-id="49827-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="49827-128">同様に、`Albums`アルバム アーティスト名がへの外部キー テーブルが含まれていない、`Artists`テーブル。</span><span class="sxs-lookup"><span data-stu-id="49827-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="49827-129">`Artists`テーブルには、アーティスト名が含まれています。</span><span class="sxs-lookup"><span data-stu-id="49827-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="49827-130">内のデータを確認する場合、`Albums`テーブル、行ごとへの外部キーが含まれています。 参照ことができます、`Genres`テーブルと外部キーを、`Artists`テーブル。</span><span class="sxs-lookup"><span data-stu-id="49827-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="49827-131">次の図がいくつかのテーブル データを表示する、`Albums`テーブル。</span><span class="sxs-lookup"><span data-stu-id="49827-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="49827-132">HTML 選択タグ</span><span class="sxs-lookup"><span data-stu-id="49827-132">The HTML Select Tag</span></span>

<span data-ttu-id="49827-133">HTML`<select>`要素 (HTML によって作成された[DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)ヘルパー) (ジャンルの一覧) などの値の完全な一覧を表示するために使用します。</span><span class="sxs-lookup"><span data-stu-id="49827-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="49827-134">編集フォームは、現在の値がわかっている場合、選択リストを表示できます、現在の値。</span><span class="sxs-lookup"><span data-stu-id="49827-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="49827-135">見たこの以前に選択した値を設定すると**コメディ**です。</span><span class="sxs-lookup"><span data-stu-id="49827-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="49827-136">選択リストは、カテゴリ、または外部キーのデータを表示するために最適です。</span><span class="sxs-lookup"><span data-stu-id="49827-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="49827-137">`<select>`ジャンルの外部キーの要素には、考えられるジャンル名の一覧が表示されますが、ジャンル プロパティは、ジャンル外部キー値を持つ、ジャンルの表示名ではなく更新は、フォームを保存するときにします。</span><span class="sxs-lookup"><span data-stu-id="49827-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="49827-138">選択した genre は、次の図で**Disco** 、アーティスト、 **Donna 夏**です。</span><span class="sxs-lookup"><span data-stu-id="49827-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="49827-139">コードをスキャフォールディングされた ASP.NET MVC を確認します。</span><span class="sxs-lookup"><span data-stu-id="49827-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="49827-140">開く、 *Controllers\StoreManagerController.cs*ファイルを見つけて、`HTTP GET Create`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="49827-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="49827-141">`Create`メソッドは、2 つ追加[SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx)オブジェクトを`ViewBag`、ジャンル情報が含まれる 1 つとアーティスト情報が含まれるいずれか。</span><span class="sxs-lookup"><span data-stu-id="49827-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="49827-142">[SelectList](https://msdn.microsoft.com/library/dd505286.aspx)上記で使用するコンス トラクター オーバー ロードは、3 つの引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="49827-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="49827-143">*項目*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx)リスト内の項目を格納します。</span><span class="sxs-lookup"><span data-stu-id="49827-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="49827-144">上記の例では、ジャンルの一覧がによって返される`db.Genres`です。</span><span class="sxs-lookup"><span data-stu-id="49827-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="49827-145">*dataValueField*: でプロパティの名前、 **IEnumerable**キーの値を含む一覧。</span><span class="sxs-lookup"><span data-stu-id="49827-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="49827-146">上記の例で`GenreId`と`ArtistId`です。</span><span class="sxs-lookup"><span data-stu-id="49827-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="49827-147">*dataTextField*: でプロパティの名前、 **IEnumerable**を表示する情報を含むリスト。</span><span class="sxs-lookup"><span data-stu-id="49827-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="49827-148">アーティスト、ジャンル テーブルで、`name`フィールドを使用します。</span><span class="sxs-lookup"><span data-stu-id="49827-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="49827-149">開く、 *Views\StoreManager\Create.cshtml*ファイルを確認、 `Html.DropDownList` genre フィールドのヘルパー マークアップ。</span><span class="sxs-lookup"><span data-stu-id="49827-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="49827-150">ビューを作成するには、最初の行を示しています、`Album`モデル。</span><span class="sxs-lookup"><span data-stu-id="49827-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="49827-151">`Create`上記の方法、モデルが渡されなかったため、ビューを取得、 **null** `Album`モデル。</span><span class="sxs-lookup"><span data-stu-id="49827-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="49827-152">この時点では作成する新しいアルバムのでこのいずれかの`Album`データ。</span><span class="sxs-lookup"><span data-stu-id="49827-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="49827-153">[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)上に示したオーバー ロードは、モデルにバインドするフィールドの名を取得します。</span><span class="sxs-lookup"><span data-stu-id="49827-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="49827-154">使用してこの名前を探して、 **ViewBag**オブジェクトを含む、 [SelectList](https://msdn.microsoft.com/library/dd505286.aspx)オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="49827-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="49827-155">このオーバー ロードを使用する必要がある名前、 **ViewBag SelectList**オブジェクト`GenreId`です。</span><span class="sxs-lookup"><span data-stu-id="49827-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="49827-156">2 番目のパラメーター (`String.Empty`) 項目が選択されていないときに表示されるテキストです。</span><span class="sxs-lookup"><span data-stu-id="49827-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="49827-157">これは、正確に新しいアルバムを作成するときに、次の新機能が必要です。</span><span class="sxs-lookup"><span data-stu-id="49827-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="49827-158">2 番目のパラメーターを削除して、次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="49827-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="49827-159">選択リストのサンプルの最初の要素、またはロックに既定値は。</span><span class="sxs-lookup"><span data-stu-id="49827-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="49827-160">確認する、`HTTP POST Create`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="49827-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="49827-161">このオーバー ロード、`Create`メソッドは、`album`ポストされたフォーム値からは、ASP.NET MVC モデル バインド システムで作成されたオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="49827-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="49827-162">モデルの状態が有効では、データベース エラーがない場合に、新しいアルバムを送信するときに新しいアルバムがデータベースに追加されます。</span><span class="sxs-lookup"><span data-stu-id="49827-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="49827-163">次の図は、新しいアルバムの作成を示しています。</span><span class="sxs-lookup"><span data-stu-id="49827-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="49827-164">使用することができます、 [fiddler ツール](http://www.fiddler2.com/fiddler2/)ポストされたフォーム値を確認する ASP.NET MVC モデル バインディングを使用してアルバム オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="49827-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="49827-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png)。</span><span class="sxs-lookup"><span data-stu-id="49827-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="49827-166">ViewBag SelectList 作成のリファクタリング</span><span class="sxs-lookup"><span data-stu-id="49827-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="49827-167">両方の`Edit`メソッドおよび`HTTP POST Create`メソッドを設定するのと同じコードがある、 **SelectList**で、 **ViewBag**です。</span><span class="sxs-lookup"><span data-stu-id="49827-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="49827-168">基本[ドライ](http://en.wikipedia.org/wiki/Don't_repeat_yourself)、このコードをリファクタリングことができます。</span><span class="sxs-lookup"><span data-stu-id="49827-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="49827-169">します。 このソフトウェアの使用が後でコードをリファクタリングします。</span><span class="sxs-lookup"><span data-stu-id="49827-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="49827-170">ジャンル、アーティストを追加する新しいメソッドを作成する**SelectList**を**ViewBag**です。</span><span class="sxs-lookup"><span data-stu-id="49827-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="49827-171">設定する 2 つの行を置き換える、`ViewBag`内の各、`Create`と`Edit`メソッドを呼び出して、`SetGenreArtistViewBag`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="49827-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="49827-172">完成したコードを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="49827-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="49827-173">新しいアルバムの作成および変更に問題を確認するアルバムを編集します。</span><span class="sxs-lookup"><span data-stu-id="49827-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="49827-174">次のドロップダウン リストに、SelectList を明示的に渡すこと</span><span class="sxs-lookup"><span data-stu-id="49827-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="49827-175">次の ASP.NET MVC のスキャフォールディングを使用して作成されたを作成および編集ビュー **DropDownList**オーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="49827-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="49827-176">`DropDownList`作成ビューのマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="49827-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="49827-177">`ViewBag`プロパティを`SelectList`という`GenreId`、 **DropDownList**ヘルパーを使用して、 `GenreId` **SelectList**で、 **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="49827-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="49827-178">次に**DropDownList**過負荷、`SelectList`に明示的に渡されます。</span><span class="sxs-lookup"><span data-stu-id="49827-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="49827-179">開く、 *Views\StoreManager\Edit.cshtml*ファイルを開き、変更、 **DropDownList**で明示的に渡すへの呼び出し、 **SelectList**、上記のオーバー ロードを使用します。</span><span class="sxs-lookup"><span data-stu-id="49827-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="49827-180">このジャンル カテゴリに対して行います。</span><span class="sxs-lookup"><span data-stu-id="49827-180">Do this for the Genre category.</span></span> <span data-ttu-id="49827-181">完成したコードは、次に示します。</span><span class="sxs-lookup"><span data-stu-id="49827-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="49827-182">アプリケーションを実行し、をクリックして、 **Admin**をリンクし、ジャズ アルバムに移動し、選択、**編集**リンクします。</span><span class="sxs-lookup"><span data-stu-id="49827-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="49827-183">現在選択されているジャンルとしてジャズではなく、ロックが表示されます。</span><span class="sxs-lookup"><span data-stu-id="49827-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="49827-184">ときに、文字列引数 (にバインドするプロパティ) および**SelectList**オブジェクトが同じ名前を持つ、選択した値は使用されません。</span><span class="sxs-lookup"><span data-stu-id="49827-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="49827-185">ブラウザーが既定の最初の要素に指定された選択した値がない場合に、 **SelectList**(これは**ロック**上記の例で)。</span><span class="sxs-lookup"><span data-stu-id="49827-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="49827-186">これは、既知の制限事項、 **DropDownList**ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="49827-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="49827-187">開く、 *Controllers\StoreManagerController.cs*ファイルし、変更、 **SelectList**オブジェクト名を`Genres`と`Artists`です。</span><span class="sxs-lookup"><span data-stu-id="49827-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="49827-188">完成したコードは、次に示します。</span><span class="sxs-lookup"><span data-stu-id="49827-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="49827-189">ジャンル、アーティスト名は、各カテゴリの ID だけを含めるようにのカテゴリより優れた名です。</span><span class="sxs-lookup"><span data-stu-id="49827-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="49827-190">オフを有料リファクタリング前いました。</span><span class="sxs-lookup"><span data-stu-id="49827-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="49827-191">変更する代わりに、 **ViewBag** 4 つの方法で変更されたに分離、`SetGenreArtistViewBag`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="49827-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="49827-192">変更、 **DropDownList**の作成を呼び出すし、新しいビューの編集**SelectList**名。</span><span class="sxs-lookup"><span data-stu-id="49827-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="49827-193">新しいマークアップ編集ビューを次に示します。</span><span class="sxs-lookup"><span data-stu-id="49827-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="49827-194">ビューを作成するには、空の文字列に、SelectList の最初の項目が表示されないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="49827-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="49827-195">新しいアルバムの作成および変更に問題を確認するアルバムを編集します。</span><span class="sxs-lookup"><span data-stu-id="49827-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="49827-196">編集のコードをテストするには、ロック以外ジャンルのアルバムを選択します。</span><span class="sxs-lookup"><span data-stu-id="49827-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="49827-197">DropDownList ヘルパーのビュー モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="49827-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="49827-198">名前が付いた ViewModels フォルダーで新しいクラスを作成`AlbumSelectListViewModel`です。</span><span class="sxs-lookup"><span data-stu-id="49827-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="49827-199">コードで置き換え、`AlbumSelectListViewModel`を次のクラス。</span><span class="sxs-lookup"><span data-stu-id="49827-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="49827-200">`AlbumSelectListViewModel`コンス トラクターは、アルバム、アーティスト、ジャンルの一覧を受け取り、アルバムを含むオブジェクトを作成し、`SelectList`ジャンル、アーティストのです。</span><span class="sxs-lookup"><span data-stu-id="49827-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="49827-201">プロジェクトをビルドしますので、`AlbumSelectListViewModel`は次の手順で、ビューを作成した場合に使用できます。</span><span class="sxs-lookup"><span data-stu-id="49827-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="49827-202">追加、`EditVM`メソッドを`StoreManagerController`です。</span><span class="sxs-lookup"><span data-stu-id="49827-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="49827-203">完成したコードを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="49827-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="49827-204">右クリックして`AlbumSelectListViewModel`**を解決する**、し**MvcMusicStore.ViewModels; を使用して**です。</span><span class="sxs-lookup"><span data-stu-id="49827-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="49827-205">次を追加する代わりに、ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="49827-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="49827-206">右クリックして`EditVM`選択**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="49827-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="49827-207">次に示すオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="49827-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="49827-208">選択**追加**の内容を置き換える、 *Views\StoreManager\EditVM.cshtml*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="49827-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="49827-209">`EditVM`マークアップは、元によく似た`Edit`マークアップで、次の例外。</span><span class="sxs-lookup"><span data-stu-id="49827-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="49827-210">モデルのプロパティの`Edit`ビュー形式は次の`model.property`(たとえば、 `model.Title` )。</span><span class="sxs-lookup"><span data-stu-id="49827-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="49827-211">モデルのプロパティの`EditVm`ビュー形式は次の`model.Album.property`(たとえば、 `model.Album.Title`)。</span><span class="sxs-lookup"><span data-stu-id="49827-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="49827-212">`EditVM`メソッドから制御がコンテナー`Album`ではなく、`Album`ように、`Edit`ビュー。</span><span class="sxs-lookup"><span data-stu-id="49827-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="49827-213">**DropDownList**ないビュー モデルから 2 番目のパラメーターを取得、 **ViewBag**です。</span><span class="sxs-lookup"><span data-stu-id="49827-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="49827-214">**BeginForm**ヘルパーを`EditVM`に明示的に投稿を表示、`Edit`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="49827-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="49827-215">投稿し、バックアップ、`Edit`アクション、する必要はない書き込み、`HTTP POST EditVM`アクションを再利用できると、 `HTTP POST` `Edit`アクション。</span><span class="sxs-lookup"><span data-stu-id="49827-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="49827-216">アプリケーションを実行し、アルバムを編集します。</span><span class="sxs-lookup"><span data-stu-id="49827-216">Run the application and edit an album.</span></span> <span data-ttu-id="49827-217">使用する URL を変更する`EditVM`です。</span><span class="sxs-lookup"><span data-stu-id="49827-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="49827-218">フィールドを変更して、ヒット、**保存**ボタン コードが動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="49827-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="49827-219">どの方法を使用する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="49827-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="49827-220">表示されるすべての 3 つの方法は、承認します。</span><span class="sxs-lookup"><span data-stu-id="49827-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="49827-221">Explictily パスに多くの開発者が必要に応じて、`SelectList`を`DropDownList`を使用して、`ViewBag`です。</span><span class="sxs-lookup"><span data-stu-id="49827-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="49827-222">この方法より適切なコレクションの名前を使用する柔軟性を提供するという追加の利点があります。</span><span class="sxs-lookup"><span data-stu-id="49827-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="49827-223">1 つの注意点は、名前を付けることはできません、`ViewBag SelectList`モデル プロパティと同じ名前のオブジェクトします。</span><span class="sxs-lookup"><span data-stu-id="49827-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="49827-224">一部の開発者は、ViewModel アプローチを選択します。</span><span class="sxs-lookup"><span data-stu-id="49827-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="49827-225">他のユーザーより詳細なマークアップを考慮し、欠点は、ViewModel アプローチの HTML を生成します。</span><span class="sxs-lookup"><span data-stu-id="49827-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="49827-226">このセクションを使用する 3 つの方法を学習おしました、 **DropDownList**カテゴリ データを使用します。</span><span class="sxs-lookup"><span data-stu-id="49827-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="49827-227">次のセクションでは、新しいカテゴリを追加する方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="49827-227">In the next section, we'll show how to add a new category.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="49827-228">[前へ](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[次へ](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="49827-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
