---
title: "検証の追加"
author: rick-anderson
description: "Razor ページに検証を追加する方法について説明します"
keywords: "ASP.NET Core,検証,DataAnnotations,Razor,Razor ページ"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 9a822457d1581a70d59c553eb28133815f395d7d
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="adding-validation-to-a-razor-page"></a><span data-ttu-id="a0147-104">Razor ページに検証を追加する</span><span class="sxs-lookup"><span data-stu-id="a0147-104">Adding validation to a Razor Page</span></span>

<span data-ttu-id="a0147-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a0147-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a0147-106">ここでは、`Movie` モデルに検証ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="a0147-106">In this section validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="a0147-107">検証規則は、ユーザーがムービーを作成または編集するときに、いつでも適用できます。</span><span class="sxs-lookup"><span data-stu-id="a0147-107">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="a0147-108">検証</span><span class="sxs-lookup"><span data-stu-id="a0147-108">Validation</span></span>

<span data-ttu-id="a0147-109">ソフトウェア開発には、[DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself" (繰り返しを避けること)) という重要な理念があります。</span><span class="sxs-lookup"><span data-stu-id="a0147-109">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="a0147-110">Razor ページでは、機能を一度規定したら、アプリ全体に反映する開発を推奨しています。</span><span class="sxs-lookup"><span data-stu-id="a0147-110">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="a0147-111">DRY によって、アプリのコード量を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="a0147-111">DRY can help reduce the amount of code in an app.</span></span> <span data-ttu-id="a0147-112">DRY を実施すると、コードのエラーが発生する可能性が低くなり、テストと保守が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="a0147-112">DRY makes the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="a0147-113">Razor ページと Entity Framework が提供している検証のサポートは、DRY 原則の好例です。</span><span class="sxs-lookup"><span data-stu-id="a0147-113">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="a0147-114">検証規則は、1 つの場所 (モデル クラス内) で宣言的に規定され、アプリの任意の場所で適用されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-114">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

### <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="a0147-115">検証規則をムービー モデルを追加する</span><span class="sxs-lookup"><span data-stu-id="a0147-115">Adding validation rules to the movie model</span></span>

<span data-ttu-id="a0147-116">*Movie.cs* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="a0147-116">Open the *Movie.cs* file.</span></span> <span data-ttu-id="a0147-117">[DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) には、クラスまたはプロパティに宣言的に適用される組み込みの検証属性セットがあります。</span><span class="sxs-lookup"><span data-stu-id="a0147-117">[DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) provides a built-in set of validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="a0147-118">また、DataAnnotations には、検証設定を支援し、検証を行わない `DataType` のような書式設定属性もあります。</span><span class="sxs-lookup"><span data-stu-id="a0147-118">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide validation.</span></span>

<span data-ttu-id="a0147-119">`Required`、`StringLength`、`RegularExpression`、および `Range` 検証属性を利用するように `Movie` クラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="a0147-119">Update the `Movie` class to take advantage of the `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="a0147-120">検証属性で、モデルのプロパティに適用されている動作を指定します。</span><span class="sxs-lookup"><span data-stu-id="a0147-120">Validation attributes specify behavior that is enforced on model properties.</span></span> <span data-ttu-id="a0147-121">`Required` および `MinimumLength` 属性は、プロパティに値が必要であることを示します。ただし、検証制約を満たすためにユーザーが空白を入力することは禁止されていません。</span><span class="sxs-lookup"><span data-stu-id="a0147-121">The `Required` and `MinimumLength` attributes indicates that a property must have a value; but nothing prevents a user from entering white space to satisfy the validation constraint.</span></span> <span data-ttu-id="a0147-122">`RegularExpression` 属性は、入力できる文字を制限するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-122">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="a0147-123">上記のコードで、`Genre` と `Rating` は、文字のみを使用する必要があります (空白、数字、特殊文字は使用できません)。</span><span class="sxs-lookup"><span data-stu-id="a0147-123">In the preceding code, `Genre` and `Rating` must use only letters (white space, numbers and special characters are not allowed).</span></span> <span data-ttu-id="a0147-124">`Range` 属性は、指定した範囲内に値を制限します。</span><span class="sxs-lookup"><span data-stu-id="a0147-124">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="a0147-125">`StringLength` 属性は文字列の最大長を設定します。必要に応じて、最短長も設定できます。</span><span class="sxs-lookup"><span data-stu-id="a0147-125">The `StringLength` attribute sets the maximum length of a string, and optionally the minimum length.</span></span> <span data-ttu-id="a0147-126">[[値の型]](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (`decimal`、`int`、`float`、`DateTime` など) は本来は必須ではなく、`[Required]` 属性を必要としていません。</span><span class="sxs-lookup"><span data-stu-id="a0147-126">[Value types](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="a0147-127">ASP.NET Core で検証規則を自動的に適用すると、アプリをより堅牢にすることができます。</span><span class="sxs-lookup"><span data-stu-id="a0147-127">Having validation rules automatically enforced by ASP.NET Core helps make an app more robust.</span></span> <span data-ttu-id="a0147-128">モデルに自動検証を適用すると、新しいコードを追加したときに適用を思い出す必要がないので、アプリの保護に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="a0147-128">Automatic validation on models helps protect the app because you don't have to remember to apply them when new code is added.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="a0147-129">Razor ページの検証エラー UI</span><span class="sxs-lookup"><span data-stu-id="a0147-129">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="a0147-130">アプリを実行し、[Pages/Movies]/(ページ/ムービー/) に移動します。</span><span class="sxs-lookup"><span data-stu-id="a0147-130">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="a0147-131">**[Create New]\(新規作成\)** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="a0147-131">Select the **Create New** link.</span></span> <span data-ttu-id="a0147-132">いくつか無効な値を入力してフォームを設定します。</span><span class="sxs-lookup"><span data-stu-id="a0147-132">Complete the form with some invalid values.</span></span> <span data-ttu-id="a0147-133">jQuery クライアント側検証がエラーを検出すると、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-133">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![複数 jQuery クライアント側検証エラーが表示されたムービー ビュー フォーム](validation/_static/val.png)

> [!NOTE]
> <span data-ttu-id="a0147-135">`Price` フィールドに小数点またはコンマを入力することはできない場合があります。</span><span class="sxs-lookup"><span data-stu-id="a0147-135">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="a0147-136">小数点にコンマ (",") を使用し、英語 (米国) 以外の日付形式を使用する英語以外のロケールの [jQuery 検証](https://jqueryvalidation.org/)をサポートするには、アプリをグローバル化する手順を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0147-136">To support [jQuery validation](https://jqueryvalidation.org/) in non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="a0147-137">詳しくは、「[その他の技術情報](#additional-resources)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a0147-137">See [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="a0147-138">ここでは、単に 10 のような整数を入力します。</span><span class="sxs-lookup"><span data-stu-id="a0147-138">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="a0147-139">無効な値を含む各フィールドに、検証エラー メッセージが自動的に表示されることがわかります。</span><span class="sxs-lookup"><span data-stu-id="a0147-139">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="a0147-140">エラーは、(JavaScript と jQuery を使用している) クライアント側とサーバー側 (ユーザーが JavaScript を無効にしている場合) の両方に適用されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-140">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="a0147-141">大きな利点は、[Create]/(作成/) ページまたは [Edit]/(編集/) ページの変更が**必要ない**ことです。</span><span class="sxs-lookup"><span data-stu-id="a0147-141">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="a0147-142">DataAnnotations がモデルに適用されたときに、検証 UI は有効になっています。</span><span class="sxs-lookup"><span data-stu-id="a0147-142">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="a0147-143">このチュートリアルで作成した Razor ページでは、(`Movie` モデル クラスのプロパティで検証属性を使用して) 検証規則が自動的に選択されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-143">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="a0147-144">[Edit]/(編集/) ページを使用して検証をテストします。同じ検証が適用されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-144">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="a0147-145">クライアント側の検証エラーがなくなるまで、フォーム データはサーバーにポストされません。</span><span class="sxs-lookup"><span data-stu-id="a0147-145">The form data is not posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="a0147-146">次のいずれかまたは複数の方法で、フォーム データがポストされていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a0147-146">Verify form data is not posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="a0147-147">`OnPostAsync` メソッドにブレークポイントを設定します。</span><span class="sxs-lookup"><span data-stu-id="a0147-147">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="a0147-148">フォームを送信します (**[Create]/(作成/)** または **[Save]/(保存/)** を選択します)。</span><span class="sxs-lookup"><span data-stu-id="a0147-148">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="a0147-149">ブレークポイントがヒットすることはありません。</span><span class="sxs-lookup"><span data-stu-id="a0147-149">The break point is never hit.</span></span>
* <span data-ttu-id="a0147-150">[Fiddler ツール](http://www.telerik.com/fiddler)を使用します。</span><span class="sxs-lookup"><span data-stu-id="a0147-150">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="a0147-151">ブラウザー開発者向けツールを使用して、ネットワーク トラフィックを監視します。</span><span class="sxs-lookup"><span data-stu-id="a0147-151">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="a0147-152">サーバー側の検証</span><span class="sxs-lookup"><span data-stu-id="a0147-152">Server-side validation</span></span>

<span data-ttu-id="a0147-153">ブラウザーで JavaScript が無効にされている場合、エラーがあるフォームを送信すると、サーバーにポストされます。</span><span class="sxs-lookup"><span data-stu-id="a0147-153">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="a0147-154">必要に応じて、サーバー側の検証をテストします。</span><span class="sxs-lookup"><span data-stu-id="a0147-154">Optional, test server-side validation:</span></span>

* <span data-ttu-id="a0147-155">ブラウザーで JavaScript を無効にします。</span><span class="sxs-lookup"><span data-stu-id="a0147-155">Disable JavaScript in the browser.</span></span> <span data-ttu-id="a0147-156">ブラウザーで JavaScript を無効にすることができない場合は、別のブラウザーを試してください。</span><span class="sxs-lookup"><span data-stu-id="a0147-156">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="a0147-157">[Create] または [Edit] ページの `OnPostAsync` メソッドにブレークポイントを設定します。</span><span class="sxs-lookup"><span data-stu-id="a0147-157">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="a0147-158">検証エラーがあるフォームを送信します。</span><span class="sxs-lookup"><span data-stu-id="a0147-158">Submit a form with validation errors.</span></span>
* <span data-ttu-id="a0147-159">モデルの状態が無効であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a0147-159">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="a0147-160">次のコードは、以前のチュートリアルでスキャフォールディング処理した *Create.cshtml* ページの一部です。</span><span class="sxs-lookup"><span data-stu-id="a0147-160">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="a0147-161">[Create] または [Edit] ページで、最初のフォームを表示し、エラーのイベント時にフォームを再表示するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-161">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="a0147-162">[入力タグ ヘルパー](xref:mvc/views/working-with-forms)は [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 属性を使用し、クライアント側で jQuery 検証に必要な HTML 属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="a0147-162">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="a0147-163">[検証タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-tag-helpers)には検証エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-163">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="a0147-164">詳しくは、[検証に関する記事](xref:mvc/models/validation)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a0147-164">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="a0147-165">[Create] ページと [Edit] ページ内に検証規則はありません。</span><span class="sxs-lookup"><span data-stu-id="a0147-165">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="a0147-166">検証規則とエラー文字列は、`Movie` クラスでのみ指定されています。</span><span class="sxs-lookup"><span data-stu-id="a0147-166">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="a0147-167">これらの検証規則は、`Movie` モデルを編集する Razor ページに自動的に適用されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-167">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="a0147-168">検証ロジックの変更が必要な場合は、モデルでのみ変更します。</span><span class="sxs-lookup"><span data-stu-id="a0147-168">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="a0147-169">検証は常にアプリケーション全体に適用されます (検証ロジックは 1 か所で定義されます)。</span><span class="sxs-lookup"><span data-stu-id="a0147-169">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="a0147-170">検証が 1 か所なので、コードが整理された状態を保ち、保守と更新が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="a0147-170">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="a0147-171">DataType 属性の使用</span><span class="sxs-lookup"><span data-stu-id="a0147-171">Using DataType Attributes</span></span>

<span data-ttu-id="a0147-172">`Movie` クラスを調べます。</span><span class="sxs-lookup"><span data-stu-id="a0147-172">Examine the `Movie` class.</span></span> <span data-ttu-id="a0147-173">`System.ComponentModel.DataAnnotations` 名前空間には、組み込みの検証属性セットに加え、書式設定の属性もあります。</span><span class="sxs-lookup"><span data-stu-id="a0147-173">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="a0147-174">`DataType` 属性は、`ReleaseDate` および `Price` プロパティに適用されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-174">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="a0147-175">`DataType` 属性は、ビュー エンジンに対して、データの書式設定のヒントのみを提供します (また、URL の場合に `<a>`、電子メールの場合に `<a href="mailto:EmailAddress.com">` などの属性を提供します)。</span><span class="sxs-lookup"><span data-stu-id="a0147-175">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="a0147-176">`RegularExpression` 属性は、データの書式を検証するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-176">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="a0147-177">`DataType` 属性は、データベースの組み込み型よりも具体的なデータ型を指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-177">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="a0147-178">`DataType` 属性は、検証属性ではありません。</span><span class="sxs-lookup"><span data-stu-id="a0147-178">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="a0147-179">サンプル アプリケーションでは、日付のみが表示され、時刻は表示されません。</span><span class="sxs-lookup"><span data-stu-id="a0147-179">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="a0147-180">`DataType` 列挙型は、Date、Time、PhoneNumber、Currency、EmailAddress など、多くの型のために用意されています。</span><span class="sxs-lookup"><span data-stu-id="a0147-180">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="a0147-181">また、`DataType` 属性を使用して、アプリケーションで型固有の機能を自動的に提供することもできます。</span><span class="sxs-lookup"><span data-stu-id="a0147-181">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="a0147-182">たとえば、`DataType.EmailAddress` に対して `mailto:` リンクを作成できます。</span><span class="sxs-lookup"><span data-stu-id="a0147-182">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="a0147-183">HTML 5 をサポートしているブラウザーでは、`DataType.Date` に日付セレクターを提供できます。</span><span class="sxs-lookup"><span data-stu-id="a0147-183">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="a0147-184">`DataType` 属性は、HTML 5 ブラウザーが使用する HTML 5 `data-` ("データ ダッシュ" と読みます) 属性を出力します。</span><span class="sxs-lookup"><span data-stu-id="a0147-184">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="a0147-185">`DataType` 属性は、検証を**提供していません**。</span><span class="sxs-lookup"><span data-stu-id="a0147-185">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="a0147-186">`DataType.Date` は、表示される日付の書式を指定しません。</span><span class="sxs-lookup"><span data-stu-id="a0147-186">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="a0147-187">既定で、日付フィールドはサーバーの `CultureInfo` に基づき、既定の書式に従って表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-187">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="a0147-188">`DisplayFormat` 属性は、日付の書式を明示的に指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-188">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="a0147-189">`ApplyFormatInEditMode` 設定では、編集対象の値を表示するときに適用する必要がある書式設定を指定します。</span><span class="sxs-lookup"><span data-stu-id="a0147-189">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="a0147-190">一部のフィールドでは、この動作が不要なことがあります。</span><span class="sxs-lookup"><span data-stu-id="a0147-190">You might not want that behavior for some fields.</span></span> <span data-ttu-id="a0147-191">たとえば、通貨値の場合、編集 UI で通貨記号が不要なことがあります。</span><span class="sxs-lookup"><span data-stu-id="a0147-191">For example, in currency values, you probably do not want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="a0147-192">`DisplayFormat` 属性を単独で使用できますが、一般的に、`DataType` 属性を使用することが推奨されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-192">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="a0147-193">`DataType` 属性は、画面でのレンダリング方法とは異なり、データのセマンティクスを伝達します。また、DisplayFormat にはない、次の利点があります。</span><span class="sxs-lookup"><span data-stu-id="a0147-193">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="a0147-194">ブラウザーは HTML5 機能を有効にすることができます (たとえば、カレンダー コントロール、ロケールに適した通貨記号、電子メール リンクを表示するときなど)。</span><span class="sxs-lookup"><span data-stu-id="a0147-194">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="a0147-195">ブラウザーの既定では、ロケールに基づいて正しい書式を使ってデータがレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="a0147-195">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="a0147-196">`DataType` 属性を使用すると、ASP.NET Core フレームワークで、適切なフィールド テンプレートを選択し、データをレンダリングすることができます。</span><span class="sxs-lookup"><span data-stu-id="a0147-196">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="a0147-197">`DisplayFormat` を単独で使用する場合、文字列テンプレートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-197">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="a0147-198">注: jQuery の検証は、`Range` 属性と `DateTime` では機能しません。</span><span class="sxs-lookup"><span data-stu-id="a0147-198">Note: jQuery validation does not work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="a0147-199">たとえば、次のコードでは、指定した範囲内の日付であっても、クライアント側の検証エラーが常に表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0147-199">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="a0147-200">一般的に、モデルに日付をハードコーディングしてコンパイルすることは推奨されません。そのため、`Range` 属性と `DateTime` の使用は推奨されません。</span><span class="sxs-lookup"><span data-stu-id="a0147-200">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="a0147-201">次のコードは、1 行で複数の属性を組み合わせる例です。</span><span class="sxs-lookup"><span data-stu-id="a0147-201">The following code shows combining attributes on one line:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

## <a name="additional-resources"></a><span data-ttu-id="a0147-202">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="a0147-202">Additional resources</span></span>

* [<span data-ttu-id="a0147-203">フォームの操作</span><span class="sxs-lookup"><span data-stu-id="a0147-203">Working with Forms</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="a0147-204">グローバライズとローカライズ</span><span class="sxs-lookup"><span data-stu-id="a0147-204">Globalization and localization</span></span>](xref:fundamentals/localization)
* [<span data-ttu-id="a0147-205">Tag Helpers の概要</span><span class="sxs-lookup"><span data-stu-id="a0147-205">Introduction to Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="a0147-206">タグ ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="a0147-206">Authoring Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)

>[!div class="step-by-step"]
<span data-ttu-id="a0147-207">[前: 新しいフィールドの追加](xref:tutorials/razor-pages/new-field)
[次: ファイルのアップロード](xref:tutorials/razor-pages/uploading-files)</span><span class="sxs-lookup"><span data-stu-id="a0147-207">[Previous: Adding a new field](xref:tutorials/razor-pages/new-field)
[Next: Uploading files](xref:tutorials/razor-pages/uploading-files)</span></span>
