---
title: "EF Core - データ モデル - 8 の 5 と razor ページ"
author: rick-anderson
description: "このチュートリアルでは、複数のエンティティとリレーションシップを追加し、書式設定、検証、およびデータベース マッピング規則を指定することによって、データ モデルをカスタマイズします。"
keywords: "ASP.NET Core、Entity Framework のコア データ注釈"
ms.author: riande
manager: wpickett
ms.date: 10/25/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: c2761f79fa4836d29541526782969bb0fd47778b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/02/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a><span data-ttu-id="ee1d4-104">EF コア Razor ページのチュートリアル (5/8) に、複雑なデータ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-104">Creating a complex data model - EF Core with Razor Pages tutorial (5 of 8)</span></span>

<span data-ttu-id="ee1d4-105">によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ee1d4-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="ee1d4-106">前のチュートリアルは、3 つのエンティティで構成されている基本的なデータ モデルと連携しています。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-106">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="ee1d4-107">このチュートリアルでは。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-107">In this tutorial:</span></span>

* <span data-ttu-id="ee1d4-108">複数のエンティティとリレーションシップが追加されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-108">More entities and relationships are added.</span></span>
* <span data-ttu-id="ee1d4-109">データ モデルをカスタマイズするには、書式設定、検証、およびデータベース マッピング規則を指定します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-109">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="ee1d4-110">完成したデータ モデルのエンティティ クラスは、次の図に表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-110">The entity classes for the completed data model is shown in the following illustration:</span></span>

![エンティティの図](complex-data-model/_static/diagram.png)

<span data-ttu-id="ee1d4-112">問題を解決できない場合に実行する場合は、ダウンロード、[この段階でのアプリを完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex)です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-112">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="ee1d4-113">データ モデルとその属性をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-113">Customize the data model with attributes</span></span>

<span data-ttu-id="ee1d4-114">このセクションで、データ モデルは、属性の使用をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-114">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="ee1d4-115">データ型の属性</span><span class="sxs-lookup"><span data-stu-id="ee1d4-115">The DataType attribute</span></span>

<span data-ttu-id="ee1d4-116">現在、学生のページには、登録日の時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-116">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="ee1d4-117">通常、のみ日付と時刻ではなく、日付フィールドが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-117">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="ee1d4-118">更新*Models/Student.cs*次のようにコードを強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-118">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="ee1d4-119">[DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1)属性は、データベースの組み込み型よりも特定するデータ型を指定します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-119">The [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="ee1d4-120">ここでは、日付のみを表示するか、日付と時刻がありません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-120">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="ee1d4-121">[DataType 列挙](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1)日付、時刻、PhoneNumber、通貨、EmailAddress など、多くのデータ型を提供します。`DataType`属性も自動的に機能を提供する型固有のアプリを有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-121">The [DataType Enumeration](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="ee1d4-122">例:</span><span class="sxs-lookup"><span data-stu-id="ee1d4-122">For example:</span></span>

* <span data-ttu-id="ee1d4-123">`mailto:`リンクが自動的に作成`DataType.EmailAddress`です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-123">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="ee1d4-124">日付の選択が提供される`DataType.Date`ほとんどのブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-124">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="ee1d4-125">`DataType`属性は、HTML 5 を出力`data-`HTML 5 ブラウザーを使用する (と読みますデータ dash) の属性です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-125">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="ee1d4-126">`DataType`属性は、検証を渡さないようにします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-126">The `DataType` attributes do not provide validation.</span></span>

<span data-ttu-id="ee1d4-127">`DataType.Date` は、表示される日付の書式を指定しません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-127">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="ee1d4-128">既定では、基に、サーバーの既定の形式に従って日付フィールドを表示[CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support)です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-128">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="ee1d4-129">`DisplayFormat` 属性は、日付の書式を明示的に指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-129">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="ee1d4-130">`ApplyFormatInEditMode`を書式設定する必要がありますにも適用編集 UI 設定を指定します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-130">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="ee1d4-131">一部のフィールドを使用しないでください`ApplyFormatInEditMode`です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-131">Some fields should not use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="ee1d4-132">たとえば、通貨記号必要があります通常に含めません編集ボックス。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-132">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="ee1d4-133">`DisplayFormat`属性は、単独で使用することができます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-133">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="ee1d4-134">一般に、使用することをお勧め、`DataType`属性が、`DisplayFormat`属性。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-134">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="ee1d4-135">`DataType`属性は、画面に表示する方法ではなく、データのセマンティクスが示されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-135">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="ee1d4-136">`DataType`属性では使用できない、次の利点を提供する`DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="ee1d4-136">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="ee1d4-137">ブラウザーには、HTML5 機能が有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-137">The browser can enable HTML5 features.</span></span> <span data-ttu-id="ee1d4-138">たとえば、予定表コントロール、ロケールに応じた通貨記号、電子メールのリンク、クライアント側の入力の検証などを表示します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-138">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="ee1d4-139">既定では、ブラウザーは、ロケールに基づく正しい形式を使用してデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-139">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="ee1d4-140">詳細については、次を参照してください。、 [\<入力 > タグ ヘルパーのドキュメント](xref:mvc/views/working-with-forms#the-input-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-140">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="ee1d4-141">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-141">Run the app.</span></span> <span data-ttu-id="ee1d4-142">受講者インデックス ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-142">Navigate to the Students Index page.</span></span> <span data-ttu-id="ee1d4-143">時刻は表示されません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-143">Times are no longer displayed.</span></span> <span data-ttu-id="ee1d4-144">使用するすべてのビュー、`Student`モデルには、時刻のない日付が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-144">Every view that uses the `Student` model displays the date without time.</span></span>

![受講者時刻なしの日付が表示されたページをインデックスします。](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="ee1d4-146">StringLength 属性</span><span class="sxs-lookup"><span data-stu-id="ee1d4-146">The StringLength attribute</span></span>

<span data-ttu-id="ee1d4-147">データの検証規則と検証エラー メッセージは、属性で指定できます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-147">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="ee1d4-148">[StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1)属性は、データ フィールドで許可される文字の最小値と最大の長さを指定します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-148">The [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="ee1d4-149">`StringLength`属性には、クライアント側およびサーバー側の検証も用意されています。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-149">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="ee1d4-150">最小値は、データベース スキーマには影響を与えません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-150">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="ee1d4-151">更新プログラム、`Student`を次のコード モデル。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-151">Update the `Student` model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="ee1d4-152">上記のコードでは、50 個の文字に名前を制限します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-152">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="ee1d4-153">`StringLength`属性を防ぐユーザー名を空白文字を入力します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-153">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="ee1d4-154">[正規表現](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1)属性は、制限を適用する入力に使用します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-154">The [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="ee1d4-155">たとえば、次のコードには、最初の文字が大文字で指定し、残りの文字は英文字でが必要です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-155">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

<span data-ttu-id="ee1d4-156">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-156">Run the app:</span></span>

* <span data-ttu-id="ee1d4-157">学生のページに移動します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-157">Navigate to the Students page.</span></span>
* <span data-ttu-id="ee1d4-158">選択**新規作成**、50 文字より長い名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-158">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="ee1d4-159">選択**作成**クライアント側の検証エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-159">Select **Create**, client-side validation shows an error message.</span></span>

![学生のインデックス文字列の長さエラーを示すページ](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="ee1d4-161">**SQL Server オブジェクト エクスプ ローラー** (SSOX) をダブルクリックして、Student テーブル デザイナーを開き、**学生**テーブル。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-161">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![移行する前に SSOX で students テーブル](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="ee1d4-163">上記の図に、スキーマを`Student`テーブル。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-163">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="ee1d4-164">型名フィールドである`nvarchar(MAX)`DB での移行が実行されていないためです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-164">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="ee1d4-165">実行すると移行は、このチュートリアルの後半で、名前フィールドは次のようになります。`nvarchar(50)`です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-165">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="ee1d4-166">列の属性</span><span class="sxs-lookup"><span data-stu-id="ee1d4-166">The Column attribute</span></span>

<span data-ttu-id="ee1d4-167">属性は、クラスとプロパティをデータベースにマップする方法を制御できます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="ee1d4-168">このセクションで、`Column`の名前にマップするための属性、 `FirstMidName` db では、"firstname"のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-168">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="ee1d4-169">列名のモデルのプロパティ名が使用されるデータベースの作成時に (する場合を除く、`Column`属性を使用)。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-169">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="ee1d4-170">`Student`モデルの使用`FirstMidName`最初の-名前のフィールドのフィールドには、ミドル ネームが含まれる場合もあるためです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="ee1d4-171">更新プログラム、 *Student.cs*を次の強調表示されたコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-171">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="ee1d4-172">前の変更に`Student.FirstMidName`アプリをマップ、`FirstName`の列、`Student`テーブル。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-172">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="ee1d4-173">追加、`Column`属性がサポートするモデルを変更、`SchoolContext`です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-173">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="ee1d4-174">モデルのバックアップ、`SchoolContext`データベースと一致しません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-174">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="ee1d4-175">移行を適用する前に、アプリを実行した場合は、次の例外が生成されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-175">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```
<span data-ttu-id="ee1d4-176">DB の更新。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-176">To update the DB:</span></span>

* <span data-ttu-id="ee1d4-177">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-177">Build the project.</span></span>
* <span data-ttu-id="ee1d4-178">プロジェクト フォルダーで、コマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-178">Open a command window in the project folder.</span></span> <span data-ttu-id="ee1d4-179">新しい移行を作成し、データベースを更新するには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-179">Enter the following commands to create a new migration and update the DB:</span></span>

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

<span data-ttu-id="ee1d4-180">`dotnet ef migrations add ColumnFirstName`コマンドには、次の警告メッセージが生成されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-180">The `dotnet ef migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="ee1d4-181">名前フィールドは、ようになりましたので、警告が生成 50 文字に制限されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-181">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="ee1d4-182">Db 名には、50 個を超える文字が含まれていた、最後の文字を 51 が失われます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-182">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="ee1d4-183">アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-183">Test the app.</span></span>

<span data-ttu-id="ee1d4-184">SSOX では、Student テーブルを開きます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-184">Open the Student table in SSOX:</span></span>

![移行した後で SSOX students テーブル](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="ee1d4-186">型の名前の列がされた移行が適用される前に、 [nvarchar (max)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-186">Before migration was applied, the name columns were of type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="ee1d4-187">名前の列は、今すぐ`nvarchar(50)`です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-187">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="ee1d4-188">列名がから変更`FirstMidName`に`FirstName`です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-188">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="ee1d4-189">次のセクションでアプリケーションをビルドするいくつかの段階でコンパイラ エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-189">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="ee1d4-190">手順では、アプリをビルドするときに指定します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-190">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="ee1d4-191">学生のエンティティの更新</span><span class="sxs-lookup"><span data-stu-id="ee1d4-191">Student entity update</span></span>

![学生エンティティ](complex-data-model/_static/student-entity.png)

<span data-ttu-id="ee1d4-193">更新*Models/Student.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-193">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="ee1d4-194">必須の属性</span><span class="sxs-lookup"><span data-stu-id="ee1d4-194">The Required attribute</span></span>

<span data-ttu-id="ee1d4-195">`Required`属性は、名前のプロパティの必須フィールドです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-195">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="ee1d4-196">`Required`値の型などの非 null 許容の型の属性は必要ありません (`DateTime`、 `int`、 `double`, などです。)。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-196">The `Required` attribute is not needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="ee1d4-197">Null にすることはできません型は、必要なフィールドとして自動的に扱われます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-197">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="ee1d4-198">`Required`に最小の長さのパラメーターに属性を置き換えることが、`StringLength`属性。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-198">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="ee1d4-199">表示属性</span><span class="sxs-lookup"><span data-stu-id="ee1d4-199">The Display attribute</span></span>

<span data-ttu-id="ee1d4-200">`Display`属性は、テキスト ボックスのキャプションにする必要があります「姓」、"Last Name"、「完全名」および「登録日」を指定します</span><span class="sxs-lookup"><span data-stu-id="ee1d4-200">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="ee1d4-201">既定のキャプションには、たとえば"Lastname と"単語を分割空白がないです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-201">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="ee1d4-202">計算 FullName プロパティ</span><span class="sxs-lookup"><span data-stu-id="ee1d4-202">The FullName calculated property</span></span>

<span data-ttu-id="ee1d4-203">`FullName`その他の 2 つのプロパティを連結することによって作成される値を返す計算されるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-203">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="ee1d4-204">`FullName`設定された場合、get アクセサーだけを持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-204">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="ee1d4-205">いいえ`FullName`列が、データベース内に作成します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-205">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="ee1d4-206">Instructor エンティティを作成します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-206">Create the Instructor Entity</span></span>

![Instructor エンティティ](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="ee1d4-208">作成*Models/Instructor.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-208">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="ee1d4-209">いくつかのプロパティは、同じことに注意してください、`Student`と`Instructor`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-209">Notice that several properties are the same in the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="ee1d4-210">チュートリアルでは、継承を実装するこのシリーズの後で、このコードは、重複を排除するリファクターされています。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-210">In the Implementing Inheritance tutorial later in this series, this code is refactored to eliminate the redundancy.</span></span>

<span data-ttu-id="ee1d4-211">複数の属性は、1 つの行に配置できます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-211">Multiple attributes can be on one line.</span></span> <span data-ttu-id="ee1d4-212">`HireDate`属性は次のように記述された可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-212">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="ee1d4-213">CourseAssignments と OfficeAssignment ナビゲーションのプロパティ</span><span class="sxs-lookup"><span data-stu-id="ee1d4-213">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="ee1d4-214">`CourseAssignments`と`OfficeAssignment`プロパティは、ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-214">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="ee1d4-215">インストラクターは任意の数のコースを教えることができますので、`CourseAssignments`コレクションとして定義されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-215">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="ee1d4-216">場合は、ナビゲーション プロパティは、複数のエンティティを保持します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-216">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="ee1d4-217">場所エントリを追加、削除、更新できるリストの種類があります。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-217">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="ee1d4-218">ナビゲーション プロパティの型は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-218">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="ee1d4-219">場合`ICollection<T>`を指定すると、EF コアを作成、`HashSet<T>`既定のコレクション。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-219">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="ee1d4-220">`CourseAssignment`エンティティが、多対多リレーションシップのセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-220">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="ee1d4-221">Contoso 大学のビジネス ルールをインストラクターが最大で 1 つのオフィスを持つことができる状態です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-221">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="ee1d4-222">`OfficeAssignment`プロパティは、1 つを保持`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-222">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="ee1d4-223">`OfficeAssignment`office が割り当てられていない場合は null です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-223">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="ee1d4-224">OfficeAssignment エンティティを作成します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-224">Create the OfficeAssignment entity</span></span>

![OfficeAssignment エンティティ](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="ee1d4-226">作成*Models/OfficeAssignment.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-226">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="ee1d4-227">キー属性</span><span class="sxs-lookup"><span data-stu-id="ee1d4-227">The Key attribute</span></span>

<span data-ttu-id="ee1d4-228">`[Key]`属性の識別に使用、プロパティ、PK (主キー) としてプロパティ名が何か classnameID または ID 以外</span><span class="sxs-lookup"><span data-stu-id="ee1d4-228">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="ee1d4-229">0 または 1 を 1 つの関係がある、`Instructor`と`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-229">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="ee1d4-230">Office の代入はのみに割り当てられているインストラクターに関連して存在します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-230">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="ee1d4-231">`OfficeAssignment` PK も、外部キー (FK) に、`Instructor`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-231">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="ee1d4-232">EF コアを自動的に認識できない`InstructorID`の主キーとして`OfficeAssignment`のため。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-232">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="ee1d4-233">`InstructorID`ID または classnameID 名前付け規則に従っていません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-233">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="ee1d4-234">したがって、`Key`属性を使用して識別`InstructorID`PK として。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-234">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="ee1d4-235">既定では、EF Core では、列はリレーションシップ用のため、キーをデータベースによって生成された非として扱います。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-235">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="ee1d4-236">講師ナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="ee1d4-236">The Instructor navigation property</span></span>

<span data-ttu-id="ee1d4-237">`OfficeAssignment`のナビゲーション プロパティ、`Instructor`エンティティが null 許容ため。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-237">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="ee1d4-238">参照型 (などのクラスは、null を許容) にします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-238">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="ee1d4-239">講師がオフィス割り当てをいない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-239">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="ee1d4-240">`OfficeAssignment`エンティティには、null 非許容`Instructor`ナビゲーション プロパティのため。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-240">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="ee1d4-241">`InstructorID`null 非許容です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-241">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="ee1d4-242">オフィス割り当ては、あるインストラクターなしに存在できません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-242">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="ee1d4-243">ときに、`Instructor`エンティティに関連する`OfficeAssignment`エンティティ、各エンティティのナビゲーション プロパティで、もう 1 つを参照しています。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-243">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="ee1d4-244">`[Required]`に対して属性を適用できませんでした、`Instructor`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-244">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="ee1d4-245">上記のコードでは、関連するインストラクターが存在する必要がありますを指定します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-245">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="ee1d4-246">上記のコードは必要ありませんので、 `InstructorID` (これは主キーではも) 外部キーが null 非許容されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-246">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="ee1d4-247">Course エンティティを変更します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-247">Modify the Course Entity</span></span>

![Course エンティティ](complex-data-model/_static/course-entity.png)

<span data-ttu-id="ee1d4-249">更新*Models/Course.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-249">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="ee1d4-250">`Course`エンティティ プロパティが含まれる外部キー (FK)`DepartmentID`です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-250">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="ee1d4-251">`DepartmentID`関連するを指す`Department`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-251">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="ee1d4-252">`Course`エンティティには、`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-252">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="ee1d4-253">EF コアは、モデルに関連するエンティティのナビゲーション プロパティがある場合、データ モデルの外部キー プロパティを必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-253">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="ee1d4-254">EF コアを使用、データベースには、必要に応じての FKs が自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-254">EF Core automatically creates FKs in the database wherever they are needed.</span></span> <span data-ttu-id="ee1d4-255">EF コア作成[プロパティをシャドウ](https://docs.microsoft.com/ef/core/modeling/shadow-properties)自動的に作成された FKs 用です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-255">EF Core creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="ee1d4-256">データ モデル内に、外部キーを含めると、簡単かつ効率的に更新を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-256">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="ee1d4-257">たとえば、モデル、FK プロパティ`DepartmentID`は*いない*含まれています。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-257">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="ee1d4-258">ときにコース エンティティは編集にフェッチされます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-258">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="ee1d4-259">`Department`エンティティが明示的に読み込まれている場合は null です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-259">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="ee1d4-260">Course エンティティを更新する、`Department`エンティティをフェッチ最初必要があります。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-260">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="ee1d4-261">ときに、外部キー プロパティ`DepartmentID`が含まれるデータ モデルでフェッチする必要はありません、`Department`更新の前にエンティティです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-261">When the FK property `DepartmentID` is included in the data model, there is no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="ee1d4-262">DatabaseGenerated 属性</span><span class="sxs-lookup"><span data-stu-id="ee1d4-262">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="ee1d4-263">`[DatabaseGenerated(DatabaseGeneratedOption.None)]`を主キーは、アプリケーションによって提供されるではなくデータベースによって生成される、属性を指定します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-263">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="ee1d4-264">既定では、EF コアは、DB で主キー値が生成されると仮定します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-264">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="ee1d4-265">DB には主キーが生成される値は、最善の方法では、通常、します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-265">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="ee1d4-266">`Course` PK がユーザーのエンティティを指定します</span><span class="sxs-lookup"><span data-stu-id="ee1d4-266">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="ee1d4-267">たとえば、数値演算する部署の 1000 シリーズ、英語版の部門の 2000年シリーズなどのコース数。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-267">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="ee1d4-268">`DatabaseGenerated`属性を使用して既定値を生成することもできます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-268">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="ee1d4-269">たとえば、DB では、行が作成または更新された日時を記録する日付フィールドを自動的に生成できます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-269">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="ee1d4-270">詳細については、次を参照してください。[生成プロパティ](https://docs.microsoft.com/ef/core/modeling/generated-properties)です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-270">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="ee1d4-271">外部キー プロパティとナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="ee1d4-271">Foreign key and navigation properties</span></span>

<span data-ttu-id="ee1d4-272">外部キー (FK) プロパティとナビゲーション プロパティで、`Course`エンティティは、次のリレーションシップを反映します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-272">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="ee1d4-273">コースを 1 つの部門に割り当てられるがあるため、 `DepartmentID` FK と`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-273">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="ee1d4-274">コースに受講者に、登録済みの任意の数を持つことができますので、`Enrollments`ナビゲーション プロパティがコレクション。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-274">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="ee1d4-275">複数のインストラクターを習得するは、コースのため、`CourseAssignments`ナビゲーション プロパティがコレクション。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-275">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="ee1d4-276">`CourseAssignment`説明は[後](#many-to-many-relationships)です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-276">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="ee1d4-277">部門 エンティティを作成します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-277">Create the Department entity</span></span>

![部門 エンティティ](complex-data-model/_static/department-entity.png)

<span data-ttu-id="ee1d4-279">作成*Models/Department.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-279">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="ee1d4-280">列の属性</span><span class="sxs-lookup"><span data-stu-id="ee1d4-280">The Column attribute</span></span>

<span data-ttu-id="ee1d4-281">以前、`Column`列名のマッピングを変更する属性が使用されています。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-281">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="ee1d4-282">コードで、 `Department` 、エンティティ、 `Column` SQL データ型マッピングを変更する属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-282">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="ee1d4-283">`Budget` Db では、SQL Server の money 型を使用して列を定義します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-283">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="ee1d4-284">列マッピングは通常必要ありません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-284">Column mapping is generally not required.</span></span> <span data-ttu-id="ee1d4-285">EF コアは、一般に、プロパティの CLR 型に基づく適切な SQL Server データ型を選択します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-285">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="ee1d4-286">CLR `decimal` SQL Server へのマップ」と入力`decimal`型です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-286">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="ee1d4-287">`Budget`通貨の場合は、money データ型は通貨に適しているとします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-287">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="ee1d4-288">外部キー プロパティとナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="ee1d4-288">Foreign key and navigation properties</span></span>

<span data-ttu-id="ee1d4-289">FK およびナビゲーションのプロパティは、次のリレーションシップを反映します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-289">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="ee1d4-290">部門では、可能性があります。 または管理者がない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-290">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="ee1d4-291">管理者は、常に、インストラクターです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-291">An administrator is always an instructor.</span></span> <span data-ttu-id="ee1d4-292">したがって、`InstructorID`プロパティに外部キーとして含まれる、`Instructor`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-292">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="ee1d4-293">ナビゲーション プロパティの名前が`Administrator`を保持するが、`Instructor`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-293">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="ee1d4-294">上記のコードでは疑問符 (?) では、プロパティが null 許容型を指定します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-294">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="ee1d4-295">部門は、コースのナビゲーション プロパティがあるために、複数のコースにすることがあります。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-295">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="ee1d4-296">注: 規則では、EF コアに、null 非許容 FKs および多対多リレーションシップの連鎖削除ができます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-296">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="ee1d4-297">連鎖削除は、循環 cascade delete ルールになります。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-297">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="ee1d4-298">循環 cascade は、移行が追加されたときに、規則の例外を削除します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-298">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="ee1d4-299">たとえば場合、`Department.InstructorID`プロパティが null 許容型として定義されていません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-299">For example, if the `Department.InstructorID` property was not defined as nullable:</span></span>

* <span data-ttu-id="ee1d4-300">EF コアは、部門が削除されたときに、インストラクターを削除する連鎖削除規則を構成します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-300">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="ee1d4-301">部門が削除されたときに、インストラクターを削除することが意図した動作ではないです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-301">Deleting the instructor when the department is deleted is not the intended behavior.</span></span>

<span data-ttu-id="ee1d4-302">ビジネス ルールが必要な場合、`InstructorID`プロパティが非 null 許容にする次 fluent API ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-302">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="ee1d4-303">上記のコードには、部門インストラクター リレーションシップで連鎖削除が無効にします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-303">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="ee1d4-304">登録のエンティティを更新します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-304">Update the Enrollment entity</span></span>

<span data-ttu-id="ee1d4-305">登録レコードは、1 つのコースを受講者によって実行されるです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-305">An enrollment record is for a one course taken by one student.</span></span>

![登録エンティティ](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="ee1d4-307">更新*Models/Enrollment.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-307">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="ee1d4-308">外部キー プロパティとナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="ee1d4-308">Foreign key and navigation properties</span></span>

<span data-ttu-id="ee1d4-309">外部キー プロパティとナビゲーション プロパティは、次のリレーションシップを反映します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-309">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="ee1d4-310">登録レコードが 1 つのコースには、 `CourseID` FK プロパティおよび`Course`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-310">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="ee1d4-311">登録レコードが 1 つの受講者には、 `StudentID` FK プロパティおよび`Student`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-311">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="ee1d4-312">多対多リレーションシップ</span><span class="sxs-lookup"><span data-stu-id="ee1d4-312">Many-to-Many Relationships</span></span>

<span data-ttu-id="ee1d4-313">多対多の関係がある、`Student`と`Course`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-313">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="ee1d4-314">`Enrollment`エンティティが、多対多の結合テーブルとして機能*ペイロードを持つ*データベースにします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-314">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="ee1d4-315">"ペイロード"を持つことを意味、`Enrollment`テーブルには FKs だけでなく、結合されたテーブルの追加のデータが含まれています (この場合、主キーと`Grade`)。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-315">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="ee1d4-316">次の図は、エンティティの図でこれらのリレーションシップがどのようにを示します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-316">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="ee1d4-317">(この図は、EF の EF パワー ツールを使用して生成された 6.x です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-317">(This diagram was generated using EF Power Tools for EF 6.x.</span></span> <span data-ttu-id="ee1d4-318">図の作成は、チュートリアルの一部です。)</span><span class="sxs-lookup"><span data-stu-id="ee1d4-318">Creating the diagram isn't part of the tutorial.)</span></span>

![受講者コース多対多の関係](complex-data-model/_static/student-course.png)

<span data-ttu-id="ee1d4-320">各リレーションシップの線は、もう一方の端、一対多のリレーションシップを示す一方の端と、アスタリスク (*) で 1 がします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-320">Each relationship line has a 1 at one end and an asterisk (*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="ee1d4-321">場合、`Enrollment`テーブルに評価情報が含まれていなかった、2 つの FKs を含めることはのみ (`CourseID`と`StudentID`)。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-321">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="ee1d4-322">ペイロードせず、多対多の結合テーブルは、純粋な結合テーブル (PJT) と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-322">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="ee1d4-323">`Instructor`と`Course`エンティティが純粋な結合テーブルを使用して多対多リレーションシップを設定します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-323">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="ee1d4-324">注: EF 6.x をサポートする多対多のリレーションシップが EF コアの暗黙の結合テーブルはありません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-324">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core does not.</span></span> <span data-ttu-id="ee1d4-325">詳細については、次を参照してください。[多対多リレーションシップ EF コア 2.0 で](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/)です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-325">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="ee1d4-326">CourseAssignment エンティティ</span><span class="sxs-lookup"><span data-stu-id="ee1d4-326">The CourseAssignment entity</span></span>

![CourseAssignment エンティティ](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="ee1d4-328">作成*Models/CourseAssignment.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-328">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="ee1d4-329">講師-コース</span><span class="sxs-lookup"><span data-stu-id="ee1d4-329">Instructor-to-Courses</span></span>

![講師-コース m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="ee1d4-331">講師-コース多対多リレーションシップ:</span><span class="sxs-lookup"><span data-stu-id="ee1d4-331">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="ee1d4-332">エンティティ セットで表される必要がある結合テーブルが必要です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-332">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="ee1d4-333">純粋な結合テーブル (テーブル ペイロードなし) です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-333">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="ee1d4-334">一般に結合エンティティの名前を付けます`EntityName1EntityName2`です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-334">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="ee1d4-335">たとえば、このパターンを使用して、インストラクター-コース結合テーブルは`CourseInstructor`します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-335">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="ee1d4-336">ただし、リレーションシップを説明する名前の使用をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-336">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="ee1d4-337">データ モデルでは、簡単なを起動し、拡張します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-337">Data models start out simple and grow.</span></span> <span data-ttu-id="ee1d4-338">いいえペイロード結合 (PJTs) は、頻繁に進化ペイロードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-338">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="ee1d4-339">使用して開始エンティティのわかりやすい名前、名前は、結合テーブルが変更されたときに変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-339">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="ee1d4-340">理想的には、結合エンティティは、ビジネス ドメイン内、自身の自然な (場合によって 1 つの単語) 名があります。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-340">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="ee1d4-341">たとえば、ブックと顧客は、評価と呼ばれる結合のエンティティにリンクでした。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-341">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="ee1d4-342">講師-コース多対多リレーションシップの`CourseAssignment`が優先`CourseInstructor`です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-342">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="ee1d4-343">複合キー</span><span class="sxs-lookup"><span data-stu-id="ee1d4-343">Composite key</span></span>

<span data-ttu-id="ee1d4-344">FKs は null を許容できません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-344">FKs are not nullable.</span></span> <span data-ttu-id="ee1d4-345">2 つの FKs `CourseAssignment` (`InstructorID`と`CourseID`) まとめての各行を一意に識別、`CourseAssignment`テーブル。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-345">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="ee1d4-346">`CourseAssignment`専用 PK. を必要としません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-346">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="ee1d4-347">`InstructorID`と`CourseID`プロパティが複合 PK. として機能</span><span class="sxs-lookup"><span data-stu-id="ee1d4-347">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="ee1d4-348">EF コアにも、複合主キーを指定する唯一の方法は、 *fluent API*です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-348">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="ee1d4-349">次のセクションは、複合 PK を構成する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-349">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="ee1d4-350">複合キーことを確認します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-350">The composite key ensures:</span></span>

* <span data-ttu-id="ee1d4-351">複数の行は、1 つのコースの許可されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-351">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="ee1d4-352">複数の行は、1 つのインストラクターが許可されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-352">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="ee1d4-353">同じインストラクターとコースに複数の行を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-353">Multiple rows for the same instructor and course is not allowed.</span></span>

<span data-ttu-id="ee1d4-354">`Enrollment`結合エンティティでは、この種の重複も有効であるため、独自の主キーを定義します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-354">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="ee1d4-355">このような重複を防ぐためには。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-355">To prevent such duplicates:</span></span>

* <span data-ttu-id="ee1d4-356">外部キーの各フィールドに一意のインデックスを追加または</span><span class="sxs-lookup"><span data-stu-id="ee1d4-356">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="ee1d4-357">構成`Enrollment`に複合主キーのような`CourseAssignment`します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-357">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="ee1d4-358">詳細については、次を参照してください。[インデックス](https://docs.microsoft.com/ef/core/modeling/indexes)です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-358">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="ee1d4-359">DB コンテキストを更新します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-359">Update the DB context</span></span>

<span data-ttu-id="ee1d4-360">次の強調表示されたコードを追加*Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="ee1d4-360">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="ee1d4-361">上記のコードは、新しいエンティティを追加し、構成、`CourseAssignment`エンティティの複合 PK.</span><span class="sxs-lookup"><span data-stu-id="ee1d4-361">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="ee1d4-362">属性に fluent API の代替手段</span><span class="sxs-lookup"><span data-stu-id="ee1d4-362">Fluent API alternative to attributes</span></span>

<span data-ttu-id="ee1d4-363">`OnModelCreating` 、上記のメソッドのコードでは、 *fluent API* EF の主な動作を構成します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-363">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="ee1d4-364">API は一連のメソッド呼び出しに 1 つのステートメントを組み合わせるで多くの場合、使用されているために、"fluent"と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-364">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="ee1d4-365">[次のコード](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)fluent API の例に示します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-365">The [following code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="ee1d4-366">このチュートリアルでは、属性を持つことはできません DB の割り当てにのみ、fluent API を使用します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-366">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="ee1d4-367">ただし、fluent API では、ほとんどの書式、検証、および属性を持つを実行するマッピング規則を指定できます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-367">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="ee1d4-368">などのいくつかの属性`MinimumLength`fluent API では適用できません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-368">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="ee1d4-369">`MinimumLength`スキーマを変更しない、最小の長さの検証規則のみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-369">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="ee1d4-370">"クリーンな状態です"そのエンティティ クラスを維持できるように排他的 fluent API の使用を好む開発者</span><span class="sxs-lookup"><span data-stu-id="ee1d4-370">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="ee1d4-371">属性および fluent API を混在させることができます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-371">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="ee1d4-372">構成できるのみ、fluent API で (複合主キーの指定) があります。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-372">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="ee1d4-373">属性によってのみ実行できるいくつかの構成 (`MinimumLength`)。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-373">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="ee1d4-374">Fluent API または属性を使用する方法を推奨します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-374">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="ee1d4-375">これら 2 つの方法のいずれかを選択します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-375">Choose one of these two approaches.</span></span>
* <span data-ttu-id="ee1d4-376">選択した方法を可能な限り一貫して使用します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-376">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="ee1d4-377">こので使用される属性のいくつかのチュートリアルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-377">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="ee1d4-378">のみの検証 (たとえば、 `MinimumLength`)。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-378">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="ee1d4-379">EF のコア構成のみ (たとえば、 `HasKey`)。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-379">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="ee1d4-380">検証と EF コア構成 (たとえば、 `[StringLength(50)]`)。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-380">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="ee1d4-381">Fluent API と属性の詳細については、次を参照してください。[構成の方法](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-381">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="ee1d4-382">エンティティの図の関係を示す</span><span class="sxs-lookup"><span data-stu-id="ee1d4-382">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="ee1d4-383">次の図では、EF パワー ツールが、完成した School モデルを作成するダイアグラムを表示します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-383">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![エンティティの図](complex-data-model/_static/diagram.png)

<span data-ttu-id="ee1d4-385">前の図を示しています。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-385">The preceding diagram shows:</span></span>

* <span data-ttu-id="ee1d4-386">いくつかの一対多リレーションシップの線 (1 ~ \*)。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-386">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="ee1d4-387">間での 0 または 1 を 1 つのリレーションシップの線 (1 対 0..1)、`Instructor`と`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-387">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="ee1d4-388">0-または-1-対多リレーションシップの線 (0..1 対 *) の間、`Instructor`と`Department`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-388">The zero-or-one-to-many relationship line (0..1 to *) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="ee1d4-389">テスト データを使用して DB をシードします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-389">Seed the DB with Test Data</span></span>

<span data-ttu-id="ee1d4-390">コードを更新*Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="ee1d4-390">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="ee1d4-391">上記のコードは、新しいエンティティのシードのデータを提供します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-391">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="ee1d4-392">このコードのほとんどは、新しいエンティティ オブジェクトを作成し、サンプル データを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-392">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="ee1d4-393">サンプル データは、テストに使用されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-393">The sample data is used for testing.</span></span> <span data-ttu-id="ee1d4-394">上記のコードでは、次の多対多リレーションシップを作成します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-394">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

<span data-ttu-id="ee1d4-395">注: [EF コア 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap)をサポートする[データ シード](https://github.com/aspnet/EntityFrameworkCore/issues/629)です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-395">Note: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) will support [data seeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="ee1d4-396">移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-396">Add a migration</span></span>

<span data-ttu-id="ee1d4-397">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-397">Build the project.</span></span> <span data-ttu-id="ee1d4-398">プロジェクト フォルダーに、コマンド ウィンドウを開き、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-398">Open a command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="ee1d4-399">上記のコマンドは、データ損失の可能性に関する警告を表示します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-399">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="ee1d4-400">場合、`database update`コマンドを実行する、次のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-400">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="ee1d4-401">移行実行すると、既存のデータと外部キー制約が既存のデータで満たされていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-401">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="ee1d4-402">このチュートリアルでは、外部キー制約違反がないため、新しいデータベースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-402">For this tutorial, a new DB is created, so there are no FK constraint violations.</span></span> <span data-ttu-id="ee1d4-403">参照してください[従来のデータと外部キー制約を修正して](#fk)の現在のデータベースで外部キー違反を修正する方法についてはします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-403">See [Fixing foreign key constraints with legacy data](#fk) for instructions on how to fix the FK violations on the current DB.</span></span>

## <a name="change-the-connection-string-and-update-the-db"></a><span data-ttu-id="ee1d4-404">接続文字列を変更し、DB の更新</span><span class="sxs-lookup"><span data-stu-id="ee1d4-404">Change the connection string and update the DB</span></span>

<span data-ttu-id="ee1d4-405">更新されたコード`DbInitializer`シード データの新しいエンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-405">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="ee1d4-406">強制的に新しい空のデータベースを作成する EF コア。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-406">To force EF Core to create a new empty DB:</span></span>

* <span data-ttu-id="ee1d4-407">DB の接続文字列名を変更*される appsettings.json* ContosoUniversity3 にします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-407">Change the DB connection string name in *appsettings.json* to ContosoUniversity3.</span></span> <span data-ttu-id="ee1d4-408">新しい名前は、コンピューターで使用されていない名前にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-408">The new name must be a name that hasn't been used on the computer.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* <span data-ttu-id="ee1d4-409">または、DB を使用して、削除します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-409">Alternatively, delete the DB using:</span></span>

    * <span data-ttu-id="ee1d4-410">**SQL Server オブジェクト エクスプ ローラー** (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-410">**SQL Server Object Explorer** (SSOX).</span></span>
    * <span data-ttu-id="ee1d4-411">`database drop` CLI コマンド。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-411">The `database drop` CLI command:</span></span>

   ```console
   dotnet ef database drop
   ```

<span data-ttu-id="ee1d4-412">実行`database update`コマンド ウィンドウで。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-412">Run `database update` in the command window:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="ee1d4-413">上記のコマンドは、すべての移行を実行します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-413">The preceding command runs all the migrations.</span></span>

<span data-ttu-id="ee1d4-414">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-414">Run the app.</span></span> <span data-ttu-id="ee1d4-415">アプリの実行を実行している、`DbInitializer.Initialize`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-415">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="ee1d4-416">`DbInitializer.Initialize`新しいデータベースを設定します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-416">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="ee1d4-417">SSOX でデータベースを開きます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-417">Open the DB in SSOX:</span></span>

* <span data-ttu-id="ee1d4-418">展開して、**テーブル**ノード。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-418">Expand the **Tables** node.</span></span> <span data-ttu-id="ee1d4-419">作成されたテーブルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-419">The created tables are displayed.</span></span>
* <span data-ttu-id="ee1d4-420">SSOX が以前に開かれた場合にクリックして、**更新**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-420">If SSOX was opened previously, click the **Refresh** button.</span></span>

![SSOX 内のテーブル](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="ee1d4-422">確認、 **CourseAssignment**テーブル。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-422">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="ee1d4-423">右クリックし、 **CourseAssignment**テーブルを選択して**ビュー データ**です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-423">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="ee1d4-424">確認、 **CourseAssignment**テーブルにデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-424">Verify the **CourseAssignment** table contains data.</span></span>

![SSOX で CourseAssignment データ](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a><span data-ttu-id="ee1d4-426">従来のデータと外部キー制約を修正します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-426">Fixing foreign key constraints with legacy data</span></span>

<span data-ttu-id="ee1d4-427">このセクションではオプションです。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-427">This section is optional.</span></span>

<span data-ttu-id="ee1d4-428">移行実行すると、既存のデータと外部キー制約が既存のデータで満たされていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-428">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="ee1d4-429">運用データには、既存のデータを移行する手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-429">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="ee1d4-430">このセクションでは、外部キー制約違反の修正の例を示します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-430">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="ee1d4-431">バックアップがない場合のこれらのコード変更を行わない。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-431">Don't make these code changes without a backup.</span></span> <span data-ttu-id="ee1d4-432">前のセクションを完了し、データベースを更新する場合、これらのコード変更を行わない。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-432">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="ee1d4-433">*{Timestamp}_ComplexDataModel.cs*ファイルには、次のコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-433">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="ee1d4-434">上記のコードを追加、null 非許容`DepartmentID`に FK、`Course`テーブル。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-434">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="ee1d4-435">前のチュートリアル DB には内の行が含まれています`Course`ので、移行によってそのテーブルを更新できません。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-435">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="ee1d4-436">させる、`ComplexDataModel`と既存のデータの移行作業します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-436">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="ee1d4-437">新しい列を提供するコードを変更 (`DepartmentID`)、既定値です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-437">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="ee1d4-438">既定の部門として機能するには、"Temp"という名前の偽部門を作成します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-438">Create a fake department named "Temp" to act as the default department.</span></span>

### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="ee1d4-439">外部キー制約を修正します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-439">Fix the foreign key constraints</span></span>

<span data-ttu-id="ee1d4-440">更新プログラム、`ComplexDataModel`クラス`Up`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-440">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="ee1d4-441">開く、 *{timestamp}_ComplexDataModel.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-441">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="ee1d4-442">追加するコードの行をコメント、`DepartmentID`列を`Course`テーブル。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-442">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="ee1d4-443">次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-443">Add the following highlighted code.</span></span> <span data-ttu-id="ee1d4-444">新しいコードに記述した後、`.CreateTable( name: "Department"`ブロック。[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="ee1d4-444">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="ee1d4-445">既存の前の変更と`Course`後に"Temp"部門に関連する行、 `ComplexDataModel` `Up`メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-445">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="ee1d4-446">運用アプリは。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-446">A production app would:</span></span>

* <span data-ttu-id="ee1d4-447">コードまたは追加するためのスクリプトを含める`Department`行および関連`Course`を新しい行`Department`行です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-447">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="ee1d4-448">"Temp"部門やの既定値は使用しない`Course.DepartmentID `です。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-448">Would not use the "Temp" department or the default value for `Course.DepartmentID `.</span></span>

<span data-ttu-id="ee1d4-449">次のチュートリアルでは、関連するデータについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ee1d4-449">The next tutorial covers related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ee1d4-450">[前へ](xref:data/ef-rp/migrations)
[次へ](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="ee1d4-450">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>