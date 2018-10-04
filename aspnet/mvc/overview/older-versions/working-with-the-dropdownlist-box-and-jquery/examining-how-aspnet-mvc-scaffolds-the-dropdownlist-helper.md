---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: ASP.NET MVC で DropDownList ヘルパーをスキャフォールディングする方法を調べること |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 542790b7f475cc641ed26ff3187c25c25118e0ed
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577848"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="4d190-102">ASP.NET MVC で DropDownList ヘルパーをスキャフォールディングする方法を調べる</span><span class="sxs-lookup"><span data-stu-id="4d190-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="4d190-103">によって[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="4d190-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="4d190-104">**ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*クリックしてフォルダー**コント ローラーの追加**します。</span><span class="sxs-lookup"><span data-stu-id="4d190-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="4d190-105">コント ローラー **StoreManagerController**します。</span><span class="sxs-lookup"><span data-stu-id="4d190-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="4d190-106">オプションを設定、**コント ローラーの追加**ダイアログの次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="4d190-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="4d190-107">編集、 *StoreManager\Index.cshtml*表示および削除`AlbumArtUrl`します。</span><span class="sxs-lookup"><span data-stu-id="4d190-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="4d190-108">削除する`AlbumArtUrl`は、プレゼンテーションを読みやすくします。</span><span class="sxs-lookup"><span data-stu-id="4d190-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="4d190-109">完成したコードを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="4d190-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="4d190-110">開く、 *Controllers\StoreManagerController.cs*ファイルを見つけて、`Index`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4d190-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="4d190-111">追加、`OrderBy`句のため、アルバムは価格で並べ替えられます。</span><span class="sxs-lookup"><span data-stu-id="4d190-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="4d190-112">完全なコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="4d190-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="4d190-113">価格で並べ替えが容易にできるようにデータベースへの変更をテストします。</span><span class="sxs-lookup"><span data-stu-id="4d190-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="4d190-114">編集、テスト メソッドを作成すると、保存されたデータを最初に表示されます、低価格を使用できます。</span><span class="sxs-lookup"><span data-stu-id="4d190-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="4d190-115">開く、 *StoreManager\Edit.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4d190-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="4d190-116">凡例のタグの直後後には、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="4d190-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="4d190-117">次のコードは、この変更のコンテキストを示しています。</span><span class="sxs-lookup"><span data-stu-id="4d190-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="4d190-118">`AlbumId`アルバムのレコードを変更するが必要です。</span><span class="sxs-lookup"><span data-stu-id="4d190-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="4d190-119">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="4d190-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="4d190-120">選択する、**管理者**をリンクし、選択、**新規作成**新しいアルバムを作成するリンク。</span><span class="sxs-lookup"><span data-stu-id="4d190-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="4d190-121">アルバム情報の保存を確認します。</span><span class="sxs-lookup"><span data-stu-id="4d190-121">Verify the album information was saved.</span></span> <span data-ttu-id="4d190-122">アルバムを編集し、加えた変更が永続化されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4d190-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="4d190-123">アルバム スキーマ</span><span class="sxs-lookup"><span data-stu-id="4d190-123">The Album Schema</span></span>

<span data-ttu-id="4d190-124">`StoreManager` MVC スキャフォールディング メカニズムによって作成されたコント ローラーは、ミュージック ストア データベースのアルバムの CRUD (作成、読み取り、更新、削除) へのアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="4d190-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="4d190-125">アルバム情報のスキーマは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="4d190-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="4d190-126">`Albums`アルバムのジャンルと説明は、テーブルが格納されていないへの外部キーを格納、`Genres`テーブル。</span><span class="sxs-lookup"><span data-stu-id="4d190-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="4d190-127">`Genres`テーブルには、ジャンル名と説明が含まれています。</span><span class="sxs-lookup"><span data-stu-id="4d190-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="4d190-128">同様に、`Albums`テーブルには、アルバム アーティスト名がへの外部キーが含まれていない、`Artists`テーブル。</span><span class="sxs-lookup"><span data-stu-id="4d190-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="4d190-129">`Artists`テーブルには、アーティスト名が含まれています。</span><span class="sxs-lookup"><span data-stu-id="4d190-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="4d190-130">内のデータを確認する場合、`Albums`テーブル、各行への外部キーが含まれています。 参照できます、`Genres`テーブルと外部キーを、`Artists`テーブル。</span><span class="sxs-lookup"><span data-stu-id="4d190-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="4d190-131">次の図の表示から一部のテーブル データ、`Albums`テーブル。</span><span class="sxs-lookup"><span data-stu-id="4d190-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="4d190-132">HTML Select タグ</span><span class="sxs-lookup"><span data-stu-id="4d190-132">The HTML Select Tag</span></span>

<span data-ttu-id="4d190-133">HTML`<select>`要素 (HTML によって作成された[DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)ヘルパー) (ジャンルのリスト) などの値の一覧を表示するために使用します。</span><span class="sxs-lookup"><span data-stu-id="4d190-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="4d190-134">編集フォームでは、現在の値がわかっている場合、選択リストを表示できます、現在の値。</span><span class="sxs-lookup"><span data-stu-id="4d190-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="4d190-135">見たこの以前に選択した値を設定すると**コメディ**します。</span><span class="sxs-lookup"><span data-stu-id="4d190-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="4d190-136">選択リストは、カテゴリまたは外部キーのデータを表示するために最適です。</span><span class="sxs-lookup"><span data-stu-id="4d190-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="4d190-137">`<select>`ジャンルの外部キーの要素には、可能なジャンル名の一覧が表示されますが、フォームを保存すると、ジャンル プロパティは、ジャンル外部キーの値、表示されているジャンル名ではなくで更新されます。</span><span class="sxs-lookup"><span data-stu-id="4d190-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="4d190-138">次の図では、選択されたジャンルは**Disco** 、アーティスト、 **Donna 夏**します。</span><span class="sxs-lookup"><span data-stu-id="4d190-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="4d190-139">スキャフォールディングされたコードを ASP.NET MVC の確認</span><span class="sxs-lookup"><span data-stu-id="4d190-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="4d190-140">開く、 *Controllers\StoreManagerController.cs*ファイルを見つけて、`HTTP GET Create`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4d190-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="4d190-141">`Create`メソッドは、2 つ追加[SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx)オブジェクトを`ViewBag`ジャンルの情報を格納する 1 つ、アーティスト情報が含まれる 1 つ。</span><span class="sxs-lookup"><span data-stu-id="4d190-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="4d190-142">[SelectList](https://msdn.microsoft.com/library/dd505286.aspx)上記で使用するコンス トラクター オーバー ロードは 3 つの引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="4d190-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="4d190-143">*項目*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx)リスト内の項目を格納します。</span><span class="sxs-lookup"><span data-stu-id="4d190-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="4d190-144">上記の例では、ジャンルの一覧がによって返される`db.Genres`します。</span><span class="sxs-lookup"><span data-stu-id="4d190-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="4d190-145">*dataValueField*: でプロパティの名前、 **IEnumerable**キーの値を含む一覧。</span><span class="sxs-lookup"><span data-stu-id="4d190-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="4d190-146">上記の例で`GenreId`と`ArtistId`します。</span><span class="sxs-lookup"><span data-stu-id="4d190-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="4d190-147">*dataTextField*: でプロパティの名前、 **IEnumerable**一覧を表示する情報を格納します。</span><span class="sxs-lookup"><span data-stu-id="4d190-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="4d190-148">アーティストとジャンルのテーブルの両方で、`name`フィールドを使用します。</span><span class="sxs-lookup"><span data-stu-id="4d190-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="4d190-149">開く、 *Views\StoreManager\Create.cshtml*ファイルを調べ、 `Html.DropDownList` genre フィールドのマークアップをヘルパー。</span><span class="sxs-lookup"><span data-stu-id="4d190-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="4d190-150">ビューを作成するには、最初の行を示しています、`Album`モデル。</span><span class="sxs-lookup"><span data-stu-id="4d190-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="4d190-151">`Create`上記の方法、モデルが渡されなかったので、ビューを取得、 **null** `Album`モデル。</span><span class="sxs-lookup"><span data-stu-id="4d190-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="4d190-152">この時点で作成する新しいアルバムがある存在しないこと、`Album`のデータ。</span><span class="sxs-lookup"><span data-stu-id="4d190-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="4d190-153">[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)上記のオーバー ロードが、モデルにバインドするフィールドの名前を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="4d190-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="4d190-154">この名前を検索するも使用、 **ViewBag**オブジェクトを含む、 [SelectList](https://msdn.microsoft.com/library/dd505286.aspx)オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4d190-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="4d190-155">このオーバー ロードを使用する必要がある名前、 **ViewBag SelectList**オブジェクト`GenreId`します。</span><span class="sxs-lookup"><span data-stu-id="4d190-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="4d190-156">2 番目のパラメーター (`String.Empty`) は、項目が選択されていない場合に表示されるテキストです。</span><span class="sxs-lookup"><span data-stu-id="4d190-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="4d190-157">これこそ新しいアルバムを作成するときに、次のものが必要です。</span><span class="sxs-lookup"><span data-stu-id="4d190-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="4d190-158">2 番目のパラメーターを削除してから、次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="4d190-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="4d190-159">選択リストのサンプルでは、最初の要素または岩を既定値は。</span><span class="sxs-lookup"><span data-stu-id="4d190-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="4d190-160">確認、`HTTP POST Create`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4d190-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="4d190-161">このオーバー ロード、`Create`メソッドは、`album`ポストされたフォーム値からは、ASP.NET MVC モデル バインド システムによって作成されたオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4d190-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="4d190-162">モデルの状態が有効であり、データベース エラーがない場合に、新しいアルバムを送信するときに、データベースに新しいアルバムが追加されます。</span><span class="sxs-lookup"><span data-stu-id="4d190-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="4d190-163">次の図は、新しいアルバムの作成を示しています。</span><span class="sxs-lookup"><span data-stu-id="4d190-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="4d190-164">使用することができます、 [fiddler ツール](http://www.fiddler2.com/fiddler2/)ポストされたフォーム値を確認するその ASP.NET MVC のモデル バインドを使用してアルバム オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="4d190-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="4d190-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png)。</span><span class="sxs-lookup"><span data-stu-id="4d190-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="4d190-166">ViewBag SelectList 作成のリファクタリング</span><span class="sxs-lookup"><span data-stu-id="4d190-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="4d190-167">両方の`Edit`メソッドと`HTTP POST Create`メソッドを設定するのと同じコードがある、 **SelectList**で、 **ViewBag**します。</span><span class="sxs-lookup"><span data-stu-id="4d190-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="4d190-168">精神[ドライ](http://en.wikipedia.org/wiki/Don't_repeat_yourself)、このコードのリファクタリングしました。</span><span class="sxs-lookup"><span data-stu-id="4d190-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="4d190-169">します。 このソフトウェアの使用が後でコードをリファクタリングします。</span><span class="sxs-lookup"><span data-stu-id="4d190-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="4d190-170">ジャンル、アーティストを追加する新しいメソッドを作成**SelectList**を**ViewBag**します。</span><span class="sxs-lookup"><span data-stu-id="4d190-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="4d190-171">設定 2 つの行を置き換える、`ViewBag`のそれぞれで、`Create`と`Edit`メソッドを呼び出して、`SetGenreArtistViewBag`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4d190-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="4d190-172">完成したコードを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="4d190-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="4d190-173">新しいアルバムを作成し、作業、変更を確認するアルバムを編集します。</span><span class="sxs-lookup"><span data-stu-id="4d190-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="4d190-174">DropDownList を SelectList を明示的に渡す</span><span class="sxs-lookup"><span data-stu-id="4d190-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="4d190-175">次の ASP.NET MVC のスキャフォールディングを使用して作成された create と edit ビュー **DropDownList**オーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="4d190-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="4d190-176">`DropDownList`作成ビューのマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="4d190-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="4d190-177">`ViewBag`プロパティを`SelectList`という`GenreId`、 **DropDownList**ヘルパーを使用して、 `GenreId` **SelectList**で、 **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="4d190-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="4d190-178">次に**DropDownList** 、オーバー ロード、`SelectList`が明示的に渡されます。</span><span class="sxs-lookup"><span data-stu-id="4d190-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="4d190-179">開く、 *Views\StoreManager\Edit.cshtml*ファイルを開き、変更、 **DropDownList**に明示的に渡す呼び出し、 **SelectList**、上記のオーバー ロードを使用します。</span><span class="sxs-lookup"><span data-stu-id="4d190-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="4d190-180">これは、ジャンル カテゴリを行います。</span><span class="sxs-lookup"><span data-stu-id="4d190-180">Do this for the Genre category.</span></span> <span data-ttu-id="4d190-181">完成したコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="4d190-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="4d190-182">アプリケーションを実行し、をクリックして、**管理者**リンク、Jazz アルバムに移動し、選択して、**編集**リンク。</span><span class="sxs-lookup"><span data-stu-id="4d190-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="4d190-183">現在選択されているジャンルとして Jazz ではなく、岩が表示されます。</span><span class="sxs-lookup"><span data-stu-id="4d190-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="4d190-184">ときに、文字列引数 (バインドするプロパティ) および**SelectList**オブジェクトが同じ名前を持つ、選択した値は使用されません。</span><span class="sxs-lookup"><span data-stu-id="4d190-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="4d190-185">ブラウザーの既定の最初の要素に指定された選択した値がない場合に、 **SelectList**(これは**Rock**上記の例では)。</span><span class="sxs-lookup"><span data-stu-id="4d190-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="4d190-186">これは、既知の制限、 **DropDownList**ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="4d190-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="4d190-187">開く、 *Controllers\StoreManagerController.cs*ファイルし、変更、 **SelectList**オブジェクト名を`Genres`と`Artists`します。</span><span class="sxs-lookup"><span data-stu-id="4d190-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="4d190-188">完成したコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="4d190-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="4d190-189">各カテゴリの ID だけが含まれているジャンル、アーティスト名は、カテゴリのわかりやすい名前です。</span><span class="sxs-lookup"><span data-stu-id="4d190-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="4d190-190">先ほどのリファクタリングは、支払われます。</span><span class="sxs-lookup"><span data-stu-id="4d190-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="4d190-191">変更する代わりに、 **ViewBag** 4 つの方法で変更されたに分離、`SetGenreArtistViewBag`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4d190-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="4d190-192">変更、 **DropDownList**の作成を呼び出すし、新しいビューの編集**SelectList**名。</span><span class="sxs-lookup"><span data-stu-id="4d190-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="4d190-193">編集ビューの新しいマークアップは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="4d190-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="4d190-194">ビューを作成するには、空の文字列、SelectList の最初の項目が表示されないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4d190-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="4d190-195">新しいアルバムを作成し、作業、変更を確認するアルバムを編集します。</span><span class="sxs-lookup"><span data-stu-id="4d190-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="4d190-196">編集のコードをテストするには、ロック以外のジャンルのアルバムを選択します。</span><span class="sxs-lookup"><span data-stu-id="4d190-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="4d190-197">DropDownList ヘルパーをビュー モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="4d190-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="4d190-198">という名前のビューモデル フォルダーで新しいクラスを作成`AlbumSelectListViewModel`です。</span><span class="sxs-lookup"><span data-stu-id="4d190-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="4d190-199">コードに置き換えます、`AlbumSelectListViewModel`を次のクラス。</span><span class="sxs-lookup"><span data-stu-id="4d190-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="4d190-200">`AlbumSelectListViewModel`コンス トラクターは、アルバム、アーティスト、ジャンルのリストを受け取り、アルバムを含むオブジェクトを作成して、`SelectList`ジャンル、アーティストの。</span><span class="sxs-lookup"><span data-stu-id="4d190-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="4d190-201">プロジェクトをビルドするため、`AlbumSelectListViewModel`は次の手順で、ビューを作成したときに使用できます。</span><span class="sxs-lookup"><span data-stu-id="4d190-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="4d190-202">追加、`EditVM`メソッドを`StoreManagerController`します。</span><span class="sxs-lookup"><span data-stu-id="4d190-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="4d190-203">完成したコードを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="4d190-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="4d190-204">右クリックして`AlbumSelectListViewModel`を選択します**解決**、し**MvcMusicStore.ViewModels; を使用して**します。</span><span class="sxs-lookup"><span data-stu-id="4d190-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="4d190-205">または、次を追加するステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="4d190-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="4d190-206">右クリックして`EditVM`選択**ビューの追加**します。</span><span class="sxs-lookup"><span data-stu-id="4d190-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="4d190-207">次に示すオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="4d190-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="4d190-208">選択**追加**の内容を置き換え、 *Views\StoreManager\EditVM.cshtml*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="4d190-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="4d190-209">`EditVM`マークアップは、元によく似た`Edit`次の例外を使用するマークアップ。</span><span class="sxs-lookup"><span data-stu-id="4d190-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="4d190-210">モデルのプロパティ、`Edit`の形式のビューは`model.property`(たとえば、 `model.Title` )。</span><span class="sxs-lookup"><span data-stu-id="4d190-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="4d190-211">モデルのプロパティ、`EditVm`の形式のビューは`model.Album.property`(たとえば、 `model.Album.Title`)。</span><span class="sxs-lookup"><span data-stu-id="4d190-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="4d190-212">です、`EditVM`ビューのコンテナーが渡されます、`Album`ではなく、`Album`ように、`Edit`ビュー。</span><span class="sxs-lookup"><span data-stu-id="4d190-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="4d190-213">**DropDownList**されませんが、ビュー モデルから 2 番目のパラメーター、 **ViewBag**します。</span><span class="sxs-lookup"><span data-stu-id="4d190-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="4d190-214">**BeginForm**でヘルパー、`EditVM`に明示的に投稿を表示、`Edit`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="4d190-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="4d190-215">投稿することでバックアップ、`Edit`アクションにしなくても書き込み、`HTTP POST EditVM`アクションと再利用できる、 `HTTP POST` `Edit`アクション。</span><span class="sxs-lookup"><span data-stu-id="4d190-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="4d190-216">アプリケーションを実行し、アルバムを編集します。</span><span class="sxs-lookup"><span data-stu-id="4d190-216">Run the application and edit an album.</span></span> <span data-ttu-id="4d190-217">使用する URL を変更する`EditVM`します。</span><span class="sxs-lookup"><span data-stu-id="4d190-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="4d190-218">フィールドを変更して、ヒット、**保存**ボタン コードの動作を検証します。</span><span class="sxs-lookup"><span data-stu-id="4d190-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="4d190-219">どちらのアプローチを使用する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="4d190-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="4d190-220">表示されるすべての 3 つのアプローチは許容します。</span><span class="sxs-lookup"><span data-stu-id="4d190-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="4d190-221">多くの開発者が好む explictily パスに、`SelectList`を`DropDownList`を使用して、`ViewBag`します。</span><span class="sxs-lookup"><span data-stu-id="4d190-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="4d190-222">このアプローチでは、コレクションのより適切な名前を使用する柔軟がもたらさの長所があります。</span><span class="sxs-lookup"><span data-stu-id="4d190-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="4d190-223">1 つの注意事項は、名前を`ViewBag SelectList`モデルのプロパティと同じ名前のオブジェクトします。</span><span class="sxs-lookup"><span data-stu-id="4d190-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="4d190-224">ViewModel のアプローチを好む開発者もいます。</span><span class="sxs-lookup"><span data-stu-id="4d190-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="4d190-225">他のユーザーより詳細なマークアップを考慮し、HTML ViewModel アプローチの欠点を生成します。</span><span class="sxs-lookup"><span data-stu-id="4d190-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="4d190-226">このセクションでは、3 つのアプローチを使用してのについてを説明しましたが、 **DropDownList**カテゴリ データ。</span><span class="sxs-lookup"><span data-stu-id="4d190-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="4d190-227">次のセクションでは、新しいカテゴリを追加する方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="4d190-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4d190-228">[前へ](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [次へ](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="4d190-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
