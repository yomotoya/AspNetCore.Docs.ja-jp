---
title: "ASP.NET MVC を持つコアを EF Core - データ モデルの 10 の 5"
author: tdykstra
description: "このチュートリアルでは、複数のエンティティとリレーションシップを追加し、書式設定、検証、およびデータベース マッピング規則を指定することによって、データ モデルをカスタマイズします。"
keywords: "ASP.NET Core、Entity Framework のコア データ注釈"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 0dd63913-a041-48b6-96a4-3aeaedbdf5d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: ad34a86c90c06dcddeeba7a0deba95f8057b4513
ms.sourcegitcommit: def90564eff4adfeed0a8e511e4c201b040e9a5e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/26/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a><span data-ttu-id="145dd-104">EF コア ASP.NET Core MVC のチュートリアル (10 の 5) に、複雑なデータ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="145dd-104">Creating a complex data model - EF Core with ASP.NET Core MVC tutorial (5 of 10)</span></span>

<span data-ttu-id="145dd-105">によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="145dd-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="145dd-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="145dd-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="145dd-107">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。</span><span class="sxs-lookup"><span data-stu-id="145dd-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="145dd-108">前のチュートリアルでは、3 つのエンティティで構成されている単純なデータ モデルを使用していた。</span><span class="sxs-lookup"><span data-stu-id="145dd-108">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="145dd-109">このチュートリアルでは複数のエンティティとリレーションシップを追加し、書式設定、検証、およびデータベース マッピング規則を指定することによって、データ モデルをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="145dd-109">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="145dd-110">完了したら、エンティティ クラスは、次の図に示す完成したデータ モデルを構成します。</span><span class="sxs-lookup"><span data-stu-id="145dd-110">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![エンティティの図](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="145dd-112">属性を使用して、データ モデルをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="145dd-112">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="145dd-113">このセクションでは、書式設定、検証、およびデータベース マッピング規則を指定する属性を使用して、データ モデルをカスタマイズする方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-113">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="145dd-114">作成する次のセクションでのいくつか追加することによって完全な学校のデータ モデル属性をクラスは、既に作成して、モデル内の残りのエンティティ型の新しいクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="145dd-114">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="145dd-115">データ型の属性</span><span class="sxs-lookup"><span data-stu-id="145dd-115">The DataType attribute</span></span>

<span data-ttu-id="145dd-116">学生の登録日のすべての web ページ現在表示、日付と時刻が、このフィールドの関心のあるは、日付。</span><span class="sxs-lookup"><span data-stu-id="145dd-116">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="145dd-117">データの注釈属性を使用することができますいずれかのコード変更データを表示するすべてのビューの表示形式を修正します。</span><span class="sxs-lookup"><span data-stu-id="145dd-117">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="145dd-118">属性を追加することを実行する方法の例を参照する、`EnrollmentDate`プロパティに、`Student`クラスです。</span><span class="sxs-lookup"><span data-stu-id="145dd-118">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="145dd-119">*Models/Student.cs*、追加、`using`のステートメント、`System.ComponentModel.DataAnnotations`名前空間を追加および`DataType`と`DisplayFormat`属性を`EnrollmentDate`プロパティ、次の例で示すように。</span><span class="sxs-lookup"><span data-stu-id="145dd-119">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

<span data-ttu-id="145dd-120">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]</span><span class="sxs-lookup"><span data-stu-id="145dd-120">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]</span></span>

<span data-ttu-id="145dd-121">`DataType`属性を使用してデータベースの組み込み型よりも特定のデータ型を指定します。</span><span class="sxs-lookup"><span data-stu-id="145dd-121">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="145dd-122">ここでのみが必要を追跡する、日付、日付と時刻がありません。</span><span class="sxs-lookup"><span data-stu-id="145dd-122">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="145dd-123">`DataType`日付、時刻、PhoneNumber、通貨、EmailAddress などの多くのデータ型の列挙体を提供します。</span><span class="sxs-lookup"><span data-stu-id="145dd-123">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="145dd-124">`DataType`属性も自動的に機能を提供する型固有のアプリケーションを有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="145dd-124">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="145dd-125">たとえば、`mailto:`のリンクを作成できます`DataType.EmailAddress`、日付選択を指定することができます、 `DataType.Date` HTML5 をサポートするブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="145dd-125">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="145dd-126">`DataType`属性は、HTML 5 を出力`data-`HTML 5 ブラウザーを理解することができます (データと読みます dash) の属性です。</span><span class="sxs-lookup"><span data-stu-id="145dd-126">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="145dd-127">`DataType`属性は、いずれかの検証を渡さないようにします。</span><span class="sxs-lookup"><span data-stu-id="145dd-127">The `DataType` attributes do not provide any validation.</span></span>

<span data-ttu-id="145dd-128">`DataType.Date`表示される日付の形式を指定しません。</span><span class="sxs-lookup"><span data-stu-id="145dd-128">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="145dd-129">既定では、サーバーの CultureInfo に基づく既定の形式に従ってデータ フィールドが表示されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-129">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="145dd-130">`DisplayFormat`属性を使用して、日付の書式を明示的に指定します。</span><span class="sxs-lookup"><span data-stu-id="145dd-130">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="145dd-131">`ApplyFormatInEditMode`設定では、書式設定も適用されることを編集するためのテキスト ボックスが表示されたら、値を指定します。</span><span class="sxs-lookup"><span data-stu-id="145dd-131">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="145dd-132">(たくない一部のフィールドなどの通貨値の可能性がありますしないこと、テキスト ボックス内の通貨記号を編集するためです。)</span><span class="sxs-lookup"><span data-stu-id="145dd-132">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="145dd-133">使用することができます、`DisplayFormat`自体が、属性が使用することをお勧めでは一般に、`DataType`も属性します。</span><span class="sxs-lookup"><span data-stu-id="145dd-133">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="145dd-134">`DataType`属性を画面に表示する方法ではなく、データのセマンティクスを伝達し、次の利点を得られないことを示します`DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="145dd-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="145dd-135">ブラウザーが HTML5 機能を有効にすることができます (たとえば、予定表コントロールでは、ロケールに応じた通貨記号、電子メールへのリンクを表示する、いくつかのクライアント側入力検証などです。)。</span><span class="sxs-lookup"><span data-stu-id="145dd-135">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="145dd-136">既定では、ブラウザーは、ロケールに基づく正しい形式を使用してデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="145dd-136">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="145dd-137">詳細については、次を参照してください。、 [\<入力 > タグ ヘルパーのドキュメント](../../mvc/views/working-with-forms.md#the-input-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="145dd-137">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="145dd-138">受講者インデックス ページを再度実行し、登録の日付の時刻が表示されていないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="145dd-138">Run the Students Index page again and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="145dd-139">学生のモデルを使用する任意のビューの場合は true。 同じになります。</span><span class="sxs-lookup"><span data-stu-id="145dd-139">The same will be true for any view that uses the Student model.</span></span>

![受講者時刻なしの日付が表示されたページをインデックスします。](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="145dd-141">StringLength 属性</span><span class="sxs-lookup"><span data-stu-id="145dd-141">The StringLength attribute</span></span>

<span data-ttu-id="145dd-142">データ検証ルールおよび属性を使用して検証エラー メッセージを指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="145dd-142">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="145dd-143">`StringLength`属性は、データベースの最大長を設定し、クライアント側とサーバー側の提供 ASP.NET MVC の検証。</span><span class="sxs-lookup"><span data-stu-id="145dd-143">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="145dd-144">この属性には、文字列の長さを指定することも、最小値データベース スキーマに影響がありません。</span><span class="sxs-lookup"><span data-stu-id="145dd-144">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="145dd-145">ユーザーが名前の 50 個を超える文字を入力しないことを確認するとします。</span><span class="sxs-lookup"><span data-stu-id="145dd-145">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="145dd-146">この制限を追加する追加`StringLength`属性を`LastName`と`FirstMidName`プロパティ、次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="145dd-146">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

<span data-ttu-id="145dd-147">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]</span><span class="sxs-lookup"><span data-stu-id="145dd-147">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]</span></span>

<span data-ttu-id="145dd-148">`StringLength`属性は、名前に空白文字を入力するからユーザーを妨げることはありません。</span><span class="sxs-lookup"><span data-stu-id="145dd-148">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="145dd-149">使用することができます、`RegularExpression`入力に制限を適用する属性。</span><span class="sxs-lookup"><span data-stu-id="145dd-149">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="145dd-150">たとえば、次のコードには、最初の文字が大文字で指定し、残りの文字は英文字でが必要です。</span><span class="sxs-lookup"><span data-stu-id="145dd-150">For example the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

<span data-ttu-id="145dd-151">`MaxLength`属性と同様の機能を提供する、`StringLength`属性が、クライアント側では提供しません検証します。</span><span class="sxs-lookup"><span data-stu-id="145dd-151">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="145dd-152">データベース モデルがデータベース スキーマの変更を必要とするように変更されました。</span><span class="sxs-lookup"><span data-stu-id="145dd-152">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="145dd-153">追加したデータベースにアプリケーション UI を使用してデータを失うことがなく、スキーマを更新するのにには移行を使用します。</span><span class="sxs-lookup"><span data-stu-id="145dd-153">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="145dd-154">変更を保存し、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="145dd-154">Save your changes and build the project.</span></span> <span data-ttu-id="145dd-155">プロジェクト フォルダー内のコマンド ウィンドウを開くし、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="145dd-155">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="145dd-156">`migrations add`コマンドをデータ損失が発生する可能性があります、変更は、最大の長さの 2 つの列より短いために警告します。</span><span class="sxs-lookup"><span data-stu-id="145dd-156">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="145dd-157">移行という名前のファイルを作成する*\<タイムスタンプ > _MaxLengthOnNames.cs*です。</span><span class="sxs-lookup"><span data-stu-id="145dd-157">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="145dd-158">このファイルには内のコードが含まれています、`Up`メソッドを現在のデータ モデルと一致するデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="145dd-158">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="145dd-159">`database update`コマンドは、そのコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="145dd-159">The `database update` command ran that code.</span></span>

<span data-ttu-id="145dd-160">移行ファイル名にプレフィックスのタイムスタンプは、Entity Framework によって、移行を並べ替えに使用されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-160">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="145dd-161">データベースの更新コマンドを実行する前に複数の移行を作成することができ、移行のすべてが作成された順序で適用されます、します。</span><span class="sxs-lookup"><span data-stu-id="145dd-161">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="145dd-162">作成 ページを実行し、50 文字以下のいずれかの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="145dd-162">Run the Create page, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="145dd-163">作成 をクリックすると、クライアント側の検証は、エラー メッセージを示します。</span><span class="sxs-lookup"><span data-stu-id="145dd-163">When you click Create, client side validation shows an error message.</span></span>

![学生のインデックス文字列の長さエラーを示すページ](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="145dd-165">列の属性</span><span class="sxs-lookup"><span data-stu-id="145dd-165">The Column attribute</span></span>

<span data-ttu-id="145dd-166">クラスとプロパティをデータベースにマップする方法を制御するのに属性を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="145dd-166">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="145dd-167">名前を使用するいたと仮定します`FirstMidName`最初の-名前のフィールドのフィールドには、ミドル ネームが含まれる場合もあるためです。</span><span class="sxs-lookup"><span data-stu-id="145dd-167">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="145dd-168">選択した名前を指定するデータベース列が、`FirstName`データベースに対してアドホック クエリを記述するユーザーがその名前に慣れているため、します。</span><span class="sxs-lookup"><span data-stu-id="145dd-168">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="145dd-169">このマッピングを作成するには、使用することができます、`Column`属性。</span><span class="sxs-lookup"><span data-stu-id="145dd-169">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="145dd-170">`Column`属性を指定するデータベースが作成されるとき、列の`Student`をマップするテーブル、`FirstMidName`プロパティの名前は`FirstName`します。</span><span class="sxs-lookup"><span data-stu-id="145dd-170">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="145dd-171">つまり、ときに、コードを指す`Student.FirstMidName`、データから得られますまたはで更新される、`FirstName`の列、`Student`テーブル。</span><span class="sxs-lookup"><span data-stu-id="145dd-171">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="145dd-172">列名を指定しない場合、プロパティ名と同じ名前が付与されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-172">If you don't specify column names, they are given the same name as the property name.</span></span>

<span data-ttu-id="145dd-173">*Student.cs*ファイルに追加し、`using`の声明`System.ComponentModel.DataAnnotations.Schema`に列名の属性を追加し、`FirstMidName`プロパティ、次の強調表示されたコードに示すように。</span><span class="sxs-lookup"><span data-stu-id="145dd-173">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

<span data-ttu-id="145dd-174">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]</span><span class="sxs-lookup"><span data-stu-id="145dd-174">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]</span></span>

<span data-ttu-id="145dd-175">追加、`Column`属性がサポートするモデルを変更、 `SchoolContext`、ので、データベースと一致しません。</span><span class="sxs-lookup"><span data-stu-id="145dd-175">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="145dd-176">変更を保存し、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="145dd-176">Save your changes and build the project.</span></span> <span data-ttu-id="145dd-177">プロジェクト フォルダー内のコマンド ウィンドウを開くし、別の移行を作成する次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="145dd-177">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="145dd-178">**SQL Server オブジェクト エクスプ ローラー**をダブルクリックして、Student テーブル デザイナーを開き、**学生**テーブル。</span><span class="sxs-lookup"><span data-stu-id="145dd-178">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![移行した後で SSOX students テーブル](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="145dd-180">最初の 2 つの移行を適用する前に、名前の列は、nvarchar (max) 型のでした。</span><span class="sxs-lookup"><span data-stu-id="145dd-180">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="145dd-181">これは nvarchar (50) し、列名は、firstname FirstMidName から変更されました。</span><span class="sxs-lookup"><span data-stu-id="145dd-181">They are now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="145dd-182">次のセクションでのすべてのエンティティ クラスの作成を完了する前にコンパイルしようとすると、コンパイラ エラーが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="145dd-182">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="145dd-183">学生エンティティの最終変更</span><span class="sxs-lookup"><span data-stu-id="145dd-183">Final changes to the Student entity</span></span>

![学生エンティティ](complex-data-model/_static/student-entity.png)

<span data-ttu-id="145dd-185">*Models/Student.cs*、次のコードで以前に追加したコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="145dd-185">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="145dd-186">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-186">The changes are highlighted.</span></span>

<span data-ttu-id="145dd-187">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]</span><span class="sxs-lookup"><span data-stu-id="145dd-187">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]</span></span>

### <a name="the-required-attribute"></a><span data-ttu-id="145dd-188">必須の属性</span><span class="sxs-lookup"><span data-stu-id="145dd-188">The Required attribute</span></span>

<span data-ttu-id="145dd-189">`Required`属性は、名前のプロパティの必須フィールドです。</span><span class="sxs-lookup"><span data-stu-id="145dd-189">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="145dd-190">`Required`値の型などの非 null 許容の型の属性は必要ありません (DateTime、int、double、float などです。)。</span><span class="sxs-lookup"><span data-stu-id="145dd-190">The `Required` attribute is not needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="145dd-191">Null にすることはできません型は、必要なフィールドとして自動的に扱われます。</span><span class="sxs-lookup"><span data-stu-id="145dd-191">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="145dd-192">削除することで、`Required`属性し、の最小の長さのパラメーターに置き換えて、`StringLength`属性。</span><span class="sxs-lookup"><span data-stu-id="145dd-192">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="145dd-193">表示属性</span><span class="sxs-lookup"><span data-stu-id="145dd-193">The Display attribute</span></span>

<span data-ttu-id="145dd-194">`Display`属性、テキスト ボックスのキャプションにするように指定「姓」、"Last Name"、「完全名」、およびプロパティ名の代わりに「登録日」(を単語に分割する領域を持つなし) 各インスタンスにします。</span><span class="sxs-lookup"><span data-stu-id="145dd-194">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="145dd-195">計算 FullName プロパティ</span><span class="sxs-lookup"><span data-stu-id="145dd-195">The FullName calculated property</span></span>

<span data-ttu-id="145dd-196">`FullName`その他の 2 つのプロパティを連結することによって作成される値を返す計算されるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="145dd-196">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="145dd-197">存在し、get アクセサーだけ、したがって`FullName`データベース内の列が生成されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-197">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="145dd-198">Instructor エンティティを作成します。</span><span class="sxs-lookup"><span data-stu-id="145dd-198">Create the Instructor Entity</span></span>

![Instructor エンティティ](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="145dd-200">作成*Models/Instructor.cs*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="145dd-200">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

<span data-ttu-id="145dd-201">[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]</span><span class="sxs-lookup"><span data-stu-id="145dd-201">[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]</span></span>

<span data-ttu-id="145dd-202">いくつかのプロパティは、Student とインストラクター エンティティで同じことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="145dd-202">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="145dd-203">[実装の継承](inheritance.md)このシリーズの後でチュートリアルでは、冗長性を回避するのには、このコードをリファクターします。</span><span class="sxs-lookup"><span data-stu-id="145dd-203">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="145dd-204">作成することも、1 行に複数の属性を配置することができます、`HireDate`次のように属性します。</span><span class="sxs-lookup"><span data-stu-id="145dd-204">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="145dd-205">CourseAssignments と OfficeAssignment ナビゲーションのプロパティ</span><span class="sxs-lookup"><span data-stu-id="145dd-205">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="145dd-206">`CourseAssignments`と`OfficeAssignment`プロパティは、ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="145dd-206">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="145dd-207">インストラクターは任意の数のコースを教えることができますので、`CourseAssignments`コレクションとして定義されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-207">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="145dd-208">ナビゲーション プロパティは、複数のエンティティを保持できる、その型が一覧にエントリを追加、削除すると、更新できるにあります。</span><span class="sxs-lookup"><span data-stu-id="145dd-208">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="145dd-209">指定できます`ICollection<T>`またはなど型`List<T>`または`HashSet<T>`です。</span><span class="sxs-lookup"><span data-stu-id="145dd-209">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="145dd-210">指定した場合`ICollection<T>`、EF の作成、`HashSet<T>`既定のコレクション。</span><span class="sxs-lookup"><span data-stu-id="145dd-210">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="145dd-211">これらは、なぜ理由`CourseAssignment`エンティティが、多対多リレーションシップに関するセクションで後述します。</span><span class="sxs-lookup"><span data-stu-id="145dd-211">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="145dd-212">Contoso 大学のビジネス ルールの状態、インストラクターが最大で 1 つのオフィスを持つことができますのみため、`OfficeAssignment`プロパティは 1 つの OfficeAssignment エンティティ (を office が割り当てられていない場合は null にすることがあります) を保持します。</span><span class="sxs-lookup"><span data-stu-id="145dd-212">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="145dd-213">OfficeAssignment エンティティを作成します。</span><span class="sxs-lookup"><span data-stu-id="145dd-213">Create the OfficeAssignment entity</span></span>

![OfficeAssignment エンティティ](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="145dd-215">作成*Models/OfficeAssignment.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="145dd-215">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

<span data-ttu-id="145dd-216">[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]</span><span class="sxs-lookup"><span data-stu-id="145dd-216">[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]</span></span>

### <a name="the-key-attribute"></a><span data-ttu-id="145dd-217">キー属性</span><span class="sxs-lookup"><span data-stu-id="145dd-217">The Key attribute</span></span>

<span data-ttu-id="145dd-218">リレーションシップがある 0 または 1 を 1 つ、インストラクターと OfficeAssignment エンティティの間です。</span><span class="sxs-lookup"><span data-stu-id="145dd-218">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="145dd-219">オフィス割り当てのみに、割り当てられているインストラクターに関連してあり、したがって、主キーも Instructor エンティティへの外部のキー。</span><span class="sxs-lookup"><span data-stu-id="145dd-219">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="145dd-220">Entity Framework は、自動的に、その名前が ID または classnameID 名前付け規則に従っていないためこのエンティティの主キーとして InstructorID を認識できません。</span><span class="sxs-lookup"><span data-stu-id="145dd-220">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="145dd-221">したがって、`Key`属性は、キーとして識別するために使用します。</span><span class="sxs-lookup"><span data-stu-id="145dd-221">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="145dd-222">使用することも、`Key`エンティティが、独自のプライマリ キーを持っていて classnameID または ID 以外何か、プロパティの名前を指定の属性</span><span class="sxs-lookup"><span data-stu-id="145dd-222">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="145dd-223">既定では、列はリレーションシップ用のため、EF はデータベースによって生成された非としてキーを扱います。</span><span class="sxs-lookup"><span data-stu-id="145dd-223">By default EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="145dd-224">講師ナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="145dd-224">The Instructor navigation property</span></span>

<span data-ttu-id="145dd-225">Instructor エンティティに null 許容`OfficeAssignment`ナビゲーション プロパティ (インストラクター オフィス割り当てがあるない) ため、OfficeAssignment エンティティが、null 非許容と`Instructor`ナビゲーション プロパティ (ため、オフィス割り当てできません講師--がないと存在`InstructorID`が null 非許容の)。</span><span class="sxs-lookup"><span data-stu-id="145dd-225">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="145dd-226">Instructor エンティティに関連する OfficeAssignment エンティティがある場合、各エンティティは、ナビゲーション プロパティで、もう 1 つへの参照があります。</span><span class="sxs-lookup"><span data-stu-id="145dd-226">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="145dd-227">配置すること、 `[Required]` 、インストラクター ナビゲーション プロパティに関連のインストラクターである必要がありますがこれを行うためがないことを指定する属性、 `InstructorID` (これは、このテーブルへのキーではも) 外部キーが null 非許容されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-227">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="145dd-228">Course エンティティを変更します。</span><span class="sxs-lookup"><span data-stu-id="145dd-228">Modify the Course Entity</span></span>

![Course エンティティ](complex-data-model/_static/course-entity.png)

<span data-ttu-id="145dd-230">*Models/Course.cs*、次のコードで以前に追加したコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="145dd-230">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="145dd-231">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-231">The changes are highlighted.</span></span>

<span data-ttu-id="145dd-232">[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]</span><span class="sxs-lookup"><span data-stu-id="145dd-232">[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]</span></span>

<span data-ttu-id="145dd-233">Course エンティティは、外部キーのプロパティを持つ`DepartmentID`が、関連する部門エンティティとそれを`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="145dd-233">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="145dd-234">Entity Framework は、関連エンティティのナビゲーション プロパティがある場合、データ モデルに外部キーのプロパティを追加する必要ありません。</span><span class="sxs-lookup"><span data-stu-id="145dd-234">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="145dd-235">EF は自動的には、必要に応じて、データベース内の外部キーを作成し、作成[プロパティをシャドウ](https://docs.microsoft.com/ef/core/modeling/shadow-properties)にします。</span><span class="sxs-lookup"><span data-stu-id="145dd-235">EF automatically creates foreign keys in the database wherever they are needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="145dd-236">簡素化されより効率的な更新プログラムを必ずデータ モデルの外部キーを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="145dd-236">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="145dd-237">たとえば、コースを編集するエンティティをフェッチするときに、Department エンティティが null、読み込まない場合ので、course エンティティを更新するときに必要があります、Department エンティティを最初に取得します。</span><span class="sxs-lookup"><span data-stu-id="145dd-237">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="145dd-238">ときに外部キー プロパティ`DepartmentID`が含まれるデータ モデルで更新する前に、Department エンティティをフェッチする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="145dd-238">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="145dd-239">DatabaseGenerated 属性</span><span class="sxs-lookup"><span data-stu-id="145dd-239">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="145dd-240">`DatabaseGenerated`属性が、`None`上のパラメーター、`CourseID`プロパティは、主キーの値は、ユーザーによって提供されるではなくデータベースによって生成されたを指定します。</span><span class="sxs-lookup"><span data-stu-id="145dd-240">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="145dd-241">既定では、Entity Framework は、主キーの値がデータベースによって生成されると仮定します。</span><span class="sxs-lookup"><span data-stu-id="145dd-241">By default, the Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="145dd-242">ほとんどのシナリオに必要な要素です。</span><span class="sxs-lookup"><span data-stu-id="145dd-242">That's what you want in most scenarios.</span></span> <span data-ttu-id="145dd-243">ただし、コースのエンティティを 1 つの部門別の部門の 2000年シリーズの一連の 1000 ようコースのユーザーが指定した番号を使用し、します。</span><span class="sxs-lookup"><span data-stu-id="145dd-243">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="145dd-244">`DatabaseGenerated`属性を使用して日付を記録するために使用するデータベース列の場合、行の作成または更新されるように、既定値を生成することもできます。</span><span class="sxs-lookup"><span data-stu-id="145dd-244">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="145dd-245">詳細については、次を参照してください。[生成プロパティ](https://docs.microsoft.com/ef/core/modeling/generated-properties)です。</span><span class="sxs-lookup"><span data-stu-id="145dd-245">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="145dd-246">外部キー プロパティとナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="145dd-246">Foreign key and navigation properties</span></span>

<span data-ttu-id="145dd-247">外部キーのプロパティと、Course エンティティでのナビゲーション プロパティは、次のリレーションシップを反映します。</span><span class="sxs-lookup"><span data-stu-id="145dd-247">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="145dd-248">コースを 1 つの部門に割り当てられるがあるため、`DepartmentID`外部キーと`Department`上記の理由によりナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="145dd-248">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="145dd-249">コースに受講者に、登録済みの任意の数を持つことができますので、`Enrollments`ナビゲーション プロパティがコレクション。</span><span class="sxs-lookup"><span data-stu-id="145dd-249">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="145dd-250">コースに複数のインストラクターを教育する可能性がありますので、`CourseAssignments`ナビゲーション プロパティがコレクション (型`CourseAssignment`については説明[後](#many-to-many-relationships))。</span><span class="sxs-lookup"><span data-stu-id="145dd-250">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="145dd-251">部門 エンティティを作成します。</span><span class="sxs-lookup"><span data-stu-id="145dd-251">Create the Department entity</span></span>

![部門 エンティティ](complex-data-model/_static/department-entity.png)


<span data-ttu-id="145dd-253">作成*Models/Department.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="145dd-253">Create *Models/Department.cs* with the following code:</span></span>

<span data-ttu-id="145dd-254">[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]</span><span class="sxs-lookup"><span data-stu-id="145dd-254">[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="145dd-255">列の属性</span><span class="sxs-lookup"><span data-stu-id="145dd-255">The Column attribute</span></span>

<span data-ttu-id="145dd-256">以前を使用して、`Column`列名のマッピングを変更する属性。</span><span class="sxs-lookup"><span data-stu-id="145dd-256">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="145dd-257">部門 エンティティのコードで、 `Column` SQL データ型マッピングの列を定義するときは、データベースで SQL Server の money 型の使用ができるように変更する属性が使用されています。</span><span class="sxs-lookup"><span data-stu-id="145dd-257">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="145dd-258">列マッピングは一般的に必要ですが、Entity Framework のプロパティを定義する CLR 型に基づく適切な SQL Server データ型の選択されるためです。</span><span class="sxs-lookup"><span data-stu-id="145dd-258">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="145dd-259">CLR `decimal` SQL Server へのマップ」と入力`decimal`型です。</span><span class="sxs-lookup"><span data-stu-id="145dd-259">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="145dd-260">ここでは、列に通貨の値が保持していると、money データ型は方が適していることを知る。</span><span class="sxs-lookup"><span data-stu-id="145dd-260">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="145dd-261">外部キー プロパティとナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="145dd-261">Foreign key and navigation properties</span></span>

<span data-ttu-id="145dd-262">外部キー プロパティとナビゲーション プロパティは、次のリレーションシップを反映します。</span><span class="sxs-lookup"><span data-stu-id="145dd-262">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="145dd-263">部門、管理者がない可能性があり、管理者は、常に、インストラクター。</span><span class="sxs-lookup"><span data-stu-id="145dd-263">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="145dd-264">したがって、`InstructorID`プロパティが Instructor エンティティへの外部キーとして含まれると、後に疑問符 () が追加、`int`タイプの null 値許容とプロパティを指定します。</span><span class="sxs-lookup"><span data-stu-id="145dd-264">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="145dd-265">ナビゲーション プロパティの名前が`Administrator`Instructor エンティティを保持しているが、します。</span><span class="sxs-lookup"><span data-stu-id="145dd-265">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="145dd-266">部門は、コースのナビゲーション プロパティがあるために、複数のコースにすることがあります。</span><span class="sxs-lookup"><span data-stu-id="145dd-266">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="145dd-267">慣例により、Entity Framework null 非許容の外部キーおよび多対多リレーションシップの連鎖削除を有効にします。</span><span class="sxs-lookup"><span data-stu-id="145dd-267">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="145dd-268">これは、結果、循環 cascade delete ルールは、移行を追加しようとすると、例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="145dd-268">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="145dd-269">たとえば、Department.InstructorID プロパティは、null 値許容と定義されていないのに、EF よう、部門ではないどのような動作を削除すると、インストラクターを削除する連鎖削除規則に構成します。</span><span class="sxs-lookup"><span data-stu-id="145dd-269">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which is not what you want to have happen.</span></span> <span data-ttu-id="145dd-270">ビジネス ルールが必要な場合、`InstructorID`プロパティを null 非許容、ステートメントを使用して、次 fluent API をリレーションシップで連鎖削除を無効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="145dd-270">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="145dd-271">登録のエンティティを変更します。</span><span class="sxs-lookup"><span data-stu-id="145dd-271">Modify the Enrollment entity</span></span>

![登録エンティティ](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="145dd-273">*Models/Enrollment.cs*、次のコードで以前に追加したコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="145dd-273">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

<span data-ttu-id="145dd-274">[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]</span><span class="sxs-lookup"><span data-stu-id="145dd-274">[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="145dd-275">外部キー プロパティとナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="145dd-275">Foreign key and navigation properties</span></span>

<span data-ttu-id="145dd-276">外部キーのプロパティとナビゲーション プロパティは、次のリレーションシップを反映します。</span><span class="sxs-lookup"><span data-stu-id="145dd-276">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="145dd-277">登録レコードが、1 つのコースがあるため、`CourseID`外部キーのプロパティと`Course`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="145dd-277">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="145dd-278">登録レコードが 1 つの受講者には、`StudentID`外部キーのプロパティと`Student`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="145dd-278">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="145dd-279">多対多リレーションシップ</span><span class="sxs-lookup"><span data-stu-id="145dd-279">Many-to-Many Relationships</span></span>

<span data-ttu-id="145dd-280">Student とコースのエンティティ間に多対多のリレーションシップがあるし、登録エンティティが、多対多の結合テーブルとして機能*ペイロードを持つ*データベースにします。</span><span class="sxs-lookup"><span data-stu-id="145dd-280">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="145dd-281">"ペイロード"で、Enrollment テーブルに (この場合は、主キーおよびグレード プロパティ) 内の結合テーブルの外部キーだけでなく、追加のデータが含まれているを意味します。</span><span class="sxs-lookup"><span data-stu-id="145dd-281">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="145dd-282">次の図は、エンティティの図でこれらのリレーションシップがどのようにを示します。</span><span class="sxs-lookup"><span data-stu-id="145dd-282">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="145dd-283">(この図は、EF の Entity Framework のパワー ツールを使用して生成された 6.x 以外の場合は、チュートリアルの一部ではない、ダイアグラムを作成、使用されているだけを例としては、ここです)。</span><span class="sxs-lookup"><span data-stu-id="145dd-283">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![受講者コース多対多の関係](complex-data-model/_static/student-course.png)

<span data-ttu-id="145dd-285">各リレーションシップの線は、もう一方の端、一対多のリレーションシップを示す一方の端と、アスタリスク (*) で 1 がします。</span><span class="sxs-lookup"><span data-stu-id="145dd-285">Each relationship line has a 1 at one end and an asterisk (*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="145dd-286">場合は、Enrollment テーブルには、評価情報が含まれていなかった、CourseID と学生 Id の 2 つの外部キーを保持することはのみです。</span><span class="sxs-lookup"><span data-stu-id="145dd-286">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="145dd-287">その場合は、なりますペイロードせず、多対多の結合テーブル (または純粋な結合テーブル)、データベースにします。</span><span class="sxs-lookup"><span data-stu-id="145dd-287">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="145dd-288">インストラクターとコース エンティティはそのような多対多リレーションシップを持ち、ペイロードしないテーブルの結合として機能するエンティティ クラスを作成するのには、次の手順。</span><span class="sxs-lookup"><span data-stu-id="145dd-288">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="145dd-289">(EF 6.x サポートされていますが、多対多のリレーションシップ、EF コアの暗黙の結合テーブルはありません。</span><span class="sxs-lookup"><span data-stu-id="145dd-289">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core does not.</span></span> <span data-ttu-id="145dd-290">詳細については、次を参照してください、 [EF コア GitHub リポジトリにディスカッション](https://github.com/aspnet/EntityFramework/issues/1368)。)。</span><span class="sxs-lookup"><span data-stu-id="145dd-290">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span> 

## <a name="the-courseassignment-entity"></a><span data-ttu-id="145dd-291">CourseAssignment エンティティ</span><span class="sxs-lookup"><span data-stu-id="145dd-291">The CourseAssignment entity</span></span>

![CourseAssignment エンティティ](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="145dd-293">作成*Models/CourseAssignment.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="145dd-293">Create *Models/CourseAssignment.cs* with the following code:</span></span>

<span data-ttu-id="145dd-294">[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]</span><span class="sxs-lookup"><span data-stu-id="145dd-294">[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]</span></span>

### <a name="join-entity-names"></a><span data-ttu-id="145dd-295">エンティティ名を参加させる</span><span class="sxs-lookup"><span data-stu-id="145dd-295">Join entity names</span></span>

<span data-ttu-id="145dd-296">講師-コース多対多リレーションシップのデータベースに必要な結合テーブルを保持しているエンティティ セットで表されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-296">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="145dd-297">結合のエンティティの名前を が一般的`EntityName1EntityName2`、ここではある`CourseInstructor`です。</span><span class="sxs-lookup"><span data-stu-id="145dd-297">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="145dd-298">ただし、リレーションシップを説明する名前を選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="145dd-298">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="145dd-299">データ モデルでは、簡単な起動し、拡大、いいえペイロード結合を頻繁にペイロードを後で取得します。</span><span class="sxs-lookup"><span data-stu-id="145dd-299">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="145dd-300">エンティティのわかりやすい名前で開始する場合は、後で、名前を変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="145dd-300">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="145dd-301">理想的には、結合エンティティは、ビジネス ドメイン内、自身の自然な (場合によって 1 つの単語) 名があります。</span><span class="sxs-lookup"><span data-stu-id="145dd-301">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="145dd-302">たとえば、ブックと顧客は、評価を通じてリンクでした。</span><span class="sxs-lookup"><span data-stu-id="145dd-302">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="145dd-303">このリレーションシップの`CourseAssignment`よりの方が適しています`CourseInstructor`です。</span><span class="sxs-lookup"><span data-stu-id="145dd-303">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="145dd-304">複合キー</span><span class="sxs-lookup"><span data-stu-id="145dd-304">Composite key</span></span>

<span data-ttu-id="145dd-305">外部キーがない null 許容、まとめて一意にテーブルの各行を識別する、独立したプライマリ キーの必要はありません。</span><span class="sxs-lookup"><span data-stu-id="145dd-305">Since the foreign keys are not nullable and together uniquely identify each row of the table, there is no need for a separate primary key.</span></span> <span data-ttu-id="145dd-306">*InstructorID*と*CourseID*プロパティは複合主キーとして機能する必要があります。</span><span class="sxs-lookup"><span data-stu-id="145dd-306">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="145dd-307">EF に複合主キーを識別する唯一の方法を使用して、 *fluent API* (属性を使用して、そのことはできません)。</span><span class="sxs-lookup"><span data-stu-id="145dd-307">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="145dd-308">次のセクションで、複合主キーを構成する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-308">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="145dd-309">複合キーにより、複数の行、1 つのコースおよび講師が 1 つに対して複数の行を保持できますが、することは いる同じインストラクターとコースに複数の行。</span><span class="sxs-lookup"><span data-stu-id="145dd-309">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="145dd-310">`Enrollment`結合エンティティは、この種の重複も有効であるために自身のプライマリ キーを定義します。</span><span class="sxs-lookup"><span data-stu-id="145dd-310">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="145dd-311">このような重複を防ぐためには、外部のキー フィールドで一意のインデックスを追加または構成することが`Enrollment`に複合主キーのような`CourseAssignment`します。</span><span class="sxs-lookup"><span data-stu-id="145dd-311">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="145dd-312">詳細については、次を参照してください。[インデックス](https://docs.efproject.net/en/latest/modeling/indexes.html)です。</span><span class="sxs-lookup"><span data-stu-id="145dd-312">For more information, see [Indexes](https://docs.efproject.net/en/latest/modeling/indexes.html).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="145dd-313">データベース コンテキストを更新します。</span><span class="sxs-lookup"><span data-stu-id="145dd-313">Update the database context</span></span>

<span data-ttu-id="145dd-314">次の強調表示されたコードを追加、 *Data/SchoolContext.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="145dd-314">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

<span data-ttu-id="145dd-315">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]</span><span class="sxs-lookup"><span data-stu-id="145dd-315">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]</span></span>

<span data-ttu-id="145dd-316">このコードは、新しいエンティティを追加し、CourseAssignment エンティティの複合主キーを構成します。</span><span class="sxs-lookup"><span data-stu-id="145dd-316">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="145dd-317">属性に fluent API の代替手段</span><span class="sxs-lookup"><span data-stu-id="145dd-317">Fluent API alternative to attributes</span></span>

<span data-ttu-id="145dd-318">内のコード、`OnModelCreating`のメソッド、`DbContext`クラスの使用、 *fluent API* EF の動作を構成します。</span><span class="sxs-lookup"><span data-stu-id="145dd-318">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="145dd-319">API が呼び出されます"fluent"からこの例のように、1 つのステートメントには一連のメソッド呼び出しをまとめてを組み合わせるで多くの場合、使用されているため、 [EF の主要なマニュアル](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="145dd-319">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="145dd-320">このチュートリアルで使用している fluent API のみデータベース マッピング属性を使用して行うことはできません。</span><span class="sxs-lookup"><span data-stu-id="145dd-320">In this tutorial you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="145dd-321">ただし、ほとんどの書式、検証、および属性を使用して行うことができますマッピング規則を指定するのに fluent API を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="145dd-321">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="145dd-322">などのいくつかの属性`MinimumLength`fluent API では適用できません。</span><span class="sxs-lookup"><span data-stu-id="145dd-322">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="145dd-323">前に述べたよう`MinimumLength`スキーマを変更しないクライアントとサーバー側の検証規則のみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-323">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="145dd-324">"クリーンな状態です"そのエンティティ クラスを維持できるように排他的 fluent API の使用を好む開発者</span><span class="sxs-lookup"><span data-stu-id="145dd-324">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="145dd-325">をしたいが存在している fluent API を使用してのみ実行できるいくつかのカスタマイズ属性および fluent API が混在することができますが、一般に推奨これら 2 つの方法のいずれかを選択し、使用可能な一貫している限りです。</span><span class="sxs-lookup"><span data-stu-id="145dd-325">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="145dd-326">両方を使用して行う場合は、競合がある場所に Fluent API が属性をオーバーライドする注意してください。</span><span class="sxs-lookup"><span data-stu-id="145dd-326">If you do use both, note that wherever there is a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="145dd-327">Fluent API と属性の詳細については、次を参照してください。[構成の方法](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="145dd-327">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="145dd-328">エンティティの図の関係を示す</span><span class="sxs-lookup"><span data-stu-id="145dd-328">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="145dd-329">次の図は、Entity Framework のパワー ツールが、完成した School モデルを作成するダイアグラムを示します。</span><span class="sxs-lookup"><span data-stu-id="145dd-329">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![エンティティの図](complex-data-model/_static/diagram.png)

<span data-ttu-id="145dd-331">一対多リレーションシップの線以外 (1 ~ \*)、インストラクターと OfficeAssignment エンティティと 0-または-1-対多リレーションシップの線の間の 0 または 1 を 1 つのリレーションシップの線 (1 対 0..1) をここで確認できます (0..1 対 *) の間で、インストラクターと Department エンティティ。</span><span class="sxs-lookup"><span data-stu-id="145dd-331">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="145dd-332">データベースにテスト データをシードします。</span><span class="sxs-lookup"><span data-stu-id="145dd-332">Seed the Database with Test Data</span></span>

<span data-ttu-id="145dd-333">コードで置き換え、 *Data/DbInitializer.cs*を作成した新しいエンティティのシードにデータを提供するために次のコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="145dd-333">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

<span data-ttu-id="145dd-334">[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]</span><span class="sxs-lookup"><span data-stu-id="145dd-334">[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]</span></span>

<span data-ttu-id="145dd-335">最初のチュートリアルで説明したとおり、このコードのほとんどは単に新しいエンティティ オブジェクトを作成し、プロパティをテストするために必要に応じてにサンプル データを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="145dd-335">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="145dd-336">多対多のリレーションシップの処理方法に注意してください: コード内のエンティティを作成することでリレーションシップを作成する、`Enrollments`と`CourseAssignment`エンティティ セットに参加します。</span><span class="sxs-lookup"><span data-stu-id="145dd-336">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="145dd-337">移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="145dd-337">Add a migration</span></span>

<span data-ttu-id="145dd-338">変更を保存し、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="145dd-338">Save your changes and build the project.</span></span> <span data-ttu-id="145dd-339">プロジェクト フォルダー内のコマンド ウィンドウを開き、入力、`migrations add`コマンド (しない update database コマンドが、まだ)。</span><span class="sxs-lookup"><span data-stu-id="145dd-339">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="145dd-340">データ損失の可能性に関する警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-340">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="145dd-341">実行しようとする場合、`database update`コマンドの時点で (これを行わないまだ) 次のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="145dd-341">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="145dd-342">"FK_dbo FOREIGN KEY 制約と競合して ALTER TABLE ステートメント。Course_dbo です。Department_DepartmentID"です。</span><span class="sxs-lookup"><span data-stu-id="145dd-342">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="145dd-343">競合が発生したデータベース"ContosoUniversity"テーブル"dbo します。部門"、列 'DepartmentID' です。</span><span class="sxs-lookup"><span data-stu-id="145dd-343">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="145dd-344">既存のデータと移行を実行するときに、外部キー制約を満たすためにデータベースにスタブ データを挿入する必要があります。</span><span class="sxs-lookup"><span data-stu-id="145dd-344">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="145dd-345">生成されたコードに、`Up`メソッドは、Course テーブルに null 非許容 DepartmentID 外部キーを追加します。</span><span class="sxs-lookup"><span data-stu-id="145dd-345">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="145dd-346">既にある場合の行 Course テーブルに、コードを実行すると、 `AddColumn` SQL Server が null にすることはできません、列内の値を認識していないために、操作が失敗します。</span><span class="sxs-lookup"><span data-stu-id="145dd-346">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="145dd-347">このチュートリアルでは、新しいデータベースでして移行を実行しますが、実稼働アプリケーションでは、既存のデータを処理する次の手順を実行する方法の例を表示するため、移行を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="145dd-347">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="145dd-348">この移行は、新しい列に、既定値を提供するコードを変更する必要がある既存のデータを操作および明細部分を作成するには、既定の部門として機能するには、"Temp"という名前です。</span><span class="sxs-lookup"><span data-stu-id="145dd-348">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="145dd-349">その結果、既存のコース行はすべてに関連する後に"Temp"部門、`Up`メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="145dd-349">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="145dd-350">開く、 *{timestamp}_ComplexDataModel.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="145dd-350">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span> 

* <span data-ttu-id="145dd-351">Course テーブルに、[DepartmentID] 列を追加するコードの行をコメントにします。</span><span class="sxs-lookup"><span data-stu-id="145dd-351">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  <span data-ttu-id="145dd-352">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]</span><span class="sxs-lookup"><span data-stu-id="145dd-352">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]</span></span>

* <span data-ttu-id="145dd-353">Department テーブルを作成するコードの後に、次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="145dd-353">Add the following highlighted code after the code that creates the Department table:</span></span>

  <span data-ttu-id="145dd-354">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="145dd-354">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="145dd-355">実稼働アプリケーションでは、コードまたは部門の行を追加して、コースの行を新しい部門行に関連するスクリプトを記述するとします。</span><span class="sxs-lookup"><span data-stu-id="145dd-355">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="145dd-356">不要になった"Temp"部門または Course.DepartmentID 列での既定値です。</span><span class="sxs-lookup"><span data-stu-id="145dd-356">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="145dd-357">変更を保存し、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="145dd-357">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="145dd-358">接続文字列を変更し、データベースを更新</span><span class="sxs-lookup"><span data-stu-id="145dd-358">Change the connection string and update the database</span></span>

<span data-ttu-id="145dd-359">今すぐに新しいコードがある、`DbInitializer`シード エンティティのデータを新しいを空のデータベースに追加するクラスです。</span><span class="sxs-lookup"><span data-stu-id="145dd-359">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="145dd-360">EF 新しい空のデータベースを作成するために、内の接続文字列でデータベースの名前を変更する*される appsettings.json* ContosoUniversity3 を使用しているコンピューターで使用していない別の名前。</span><span class="sxs-lookup"><span data-stu-id="145dd-360">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="145dd-361">変更を保存*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="145dd-361">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="145dd-362">データベース名を変更する代わりに、データベースを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="145dd-362">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="145dd-363">使用して**SQL Server オブジェクト エクスプ ローラー** (SSOX) または`database drop`CLI コマンド。</span><span class="sxs-lookup"><span data-stu-id="145dd-363">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="145dd-364">データベース名を変更または、データベースを削除した後、実行、`database update`コマンドをコマンド ウィンドウで、移行を実行します。</span><span class="sxs-lookup"><span data-stu-id="145dd-364">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="145dd-365">アプリケーションを実行、`DbInitializer.Initialize`メソッドを実行し、新しいデータベースを設定します。</span><span class="sxs-lookup"><span data-stu-id="145dd-365">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="145dd-366">以前に行ったよう、SSOX でデータベースを開き、展開、**テーブル**ノードをすべてのテーブルが作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="145dd-366">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="145dd-367">(以前の状態から開く SSOX があるを場合更新 をクリックします。)</span><span class="sxs-lookup"><span data-stu-id="145dd-367">(If you still have SSOX open from the earlier time, click the Refresh button.)</span></span>

![SSOX 内のテーブル](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="145dd-369">データベースのシードを設定する初期化子のコードをトリガーするアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="145dd-369">Run the application to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="145dd-370">右クリックし、 **CourseAssignment**テーブルを選択して**ビュー データ**にデータが入っていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="145dd-370">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![SSOX で CourseAssignment データ](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="145dd-372">概要</span><span class="sxs-lookup"><span data-stu-id="145dd-372">Summary</span></span>

<span data-ttu-id="145dd-373">複雑なデータ モデルと対応するデータベースがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="145dd-373">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="145dd-374">次のチュートリアルでは、学びます関連のデータにアクセスする方法の詳細。</span><span class="sxs-lookup"><span data-stu-id="145dd-374">In the following tutorial, you'll learn more about how to access related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="145dd-375">[前へ](migrations.md)
[次へ](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="145dd-375">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>  
