---
title: ASP.NET Core MVC でのモデルの検証
author: tdykstra
description: ASP.NET Core MVC でのモデルの検証について説明します。
ms.author: riande
ms.date: 07/31/2018
uid: mvc/models/validation
ms.openlocfilehash: f407903577e40b6501737ef5b78d90e1e3e60c06
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2018
ms.locfileid: "39378668"
---
# <a name="model-validation-in-aspnet-core-mvc"></a><span data-ttu-id="c0863-103">ASP.NET Core MVC でのモデルの検証</span><span class="sxs-lookup"><span data-stu-id="c0863-103">Model validation in ASP.NET Core MVC</span></span>

<span data-ttu-id="c0863-104">作成者: [Rachel Appel](https://github.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="c0863-104">By [Rachel Appel](https://github.com/rachelappel)</span></span>

## <a name="introduction-to-model-validation"></a><span data-ttu-id="c0863-105">モデル検証の概要</span><span class="sxs-lookup"><span data-stu-id="c0863-105">Introduction to model validation</span></span>

<span data-ttu-id="c0863-106">アプリでは、データベースにデータを格納する前に、データを検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-106">Before an app stores data in a database, the app must validate the data.</span></span> <span data-ttu-id="c0863-107">データにセキュリティ上の脅威がないかどうかを確認し、種類とサイズが適切に設定されていることを検証しなければなりません。また、ご自身のルールに準拠している必要もあります。</span><span class="sxs-lookup"><span data-stu-id="c0863-107">Data must be checked for potential security threats, verified that it's appropriately formatted by type and size, and it must conform to your rules.</span></span> <span data-ttu-id="c0863-108">検証を実装するのは冗長で面倒な場合がありますが、必要不可欠です。</span><span class="sxs-lookup"><span data-stu-id="c0863-108">Validation is necessary although it can be redundant and tedious to implement.</span></span> <span data-ttu-id="c0863-109">MVC では、検証はクライアントとサーバーの両方で発生します。</span><span class="sxs-lookup"><span data-stu-id="c0863-109">In MVC, validation happens on both the client and server.</span></span>

<span data-ttu-id="c0863-110">さいわい、.NET では検証が検証属性に抽象化されています。</span><span class="sxs-lookup"><span data-stu-id="c0863-110">Fortunately, .NET has abstracted validation into validation attributes.</span></span> <span data-ttu-id="c0863-111">これらの属性には検証コードが含まれているため、開発者が記述しなければならないコードの量は少なくて済みます。</span><span class="sxs-lookup"><span data-stu-id="c0863-111">These attributes contain validation code, thereby reducing the amount of code you must write.</span></span>

<span data-ttu-id="c0863-112">[GitHub のサンプルを表示またはダウンロードしてください](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)。</span><span class="sxs-lookup"><span data-stu-id="c0863-112">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).</span></span>

## <a name="validation-attributes"></a><span data-ttu-id="c0863-113">検証属性</span><span class="sxs-lookup"><span data-stu-id="c0863-113">Validation Attributes</span></span>

<span data-ttu-id="c0863-114">検証属性はモデルの検証を構成する方法であり、概念的にはデータベース テーブルでのフィールドの検証に似ています。</span><span class="sxs-lookup"><span data-stu-id="c0863-114">Validation attributes are a way to configure model validation so it's similar conceptually to validation on fields in database tables.</span></span> <span data-ttu-id="c0863-115">これには、データ型の割り当てや必須フィールドなどの制約が含まれます。</span><span class="sxs-lookup"><span data-stu-id="c0863-115">This includes constraints such as assigning data types or required fields.</span></span> <span data-ttu-id="c0863-116">その他の種類の検証としては、データへのパターンの適用があります。クレジット カード、電話番号、メール アドレスなどのビジネス ルールを強制するためのものです。</span><span class="sxs-lookup"><span data-stu-id="c0863-116">Other types of validation include applying patterns to data to enforce business rules, such as a credit card, phone number, or email address.</span></span> <span data-ttu-id="c0863-117">検証属性を使うと、これらの要件の適用がはるかに単純で簡単になります。</span><span class="sxs-lookup"><span data-stu-id="c0863-117">Validation attributes make enforcing these requirements much simpler and easier to use.</span></span>

<span data-ttu-id="c0863-118">次に示すのは、映画やテレビ番組に関する情報を格納するアプリの注釈付き `Movie` モデルです。</span><span class="sxs-lookup"><span data-stu-id="c0863-118">Below is an annotated `Movie` model from an app that stores information about movies and TV shows.</span></span> <span data-ttu-id="c0863-119">ほとんどのプロパティは必須であり、一部の文字列プロパティには長さの要件があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-119">Most of the properties are required and several string properties have length requirements.</span></span> <span data-ttu-id="c0863-120">さらに、`Price` プロパティには 0 から $999.99 までの数値範囲制限が、カスタム検証属性と共に適用されています。</span><span class="sxs-lookup"><span data-stu-id="c0863-120">Additionally, there's a numeric range restriction in place for the `Price` property from 0 to $999.99, along with a custom validation attribute.</span></span>

[!code-csharp[](validation/sample/Movie.cs?range=6-29)]

<span data-ttu-id="c0863-121">モデルに目を通すだけでこのアプリのデータについてのルールがわかり、コードの保守が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="c0863-121">Simply reading through the model reveals the rules about data for this app, making it easier to maintain the code.</span></span> <span data-ttu-id="c0863-122">いくつかの一般的な組み込み検証属性を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c0863-122">Below are several popular built-in validation attributes:</span></span>

* <span data-ttu-id="c0863-123">`[CreditCard]`: プロパティがクレジット カード形式であることを検証します。</span><span class="sxs-lookup"><span data-stu-id="c0863-123">`[CreditCard]`: Validates the property has a credit card format.</span></span>

* <span data-ttu-id="c0863-124">`[Compare]`: モデルの 2 つのプロパティが一致することを検証します。</span><span class="sxs-lookup"><span data-stu-id="c0863-124">`[Compare]`: Validates two properties in a model match.</span></span>

* <span data-ttu-id="c0863-125">`[EmailAddress]`: プロパティが電子メール形式であることを検証します。</span><span class="sxs-lookup"><span data-stu-id="c0863-125">`[EmailAddress]`: Validates the property has an email format.</span></span>

* <span data-ttu-id="c0863-126">`[Phone]`: プロパティが電話番号形式であることを検証します。</span><span class="sxs-lookup"><span data-stu-id="c0863-126">`[Phone]`: Validates the property has a telephone format.</span></span>

* <span data-ttu-id="c0863-127">`[Range]`: プロパティの値が特定の範囲内であることを検証します。</span><span class="sxs-lookup"><span data-stu-id="c0863-127">`[Range]`: Validates the property value falls within the given range.</span></span>

* <span data-ttu-id="c0863-128">`[RegularExpression]`: データが指定した正規表現と一致することを検証します。</span><span class="sxs-lookup"><span data-stu-id="c0863-128">`[RegularExpression]`: Validates that the data matches the specified regular expression.</span></span>

* <span data-ttu-id="c0863-129">`[Required]`: プロパティを必須にします。</span><span class="sxs-lookup"><span data-stu-id="c0863-129">`[Required]`: Makes a property required.</span></span>

* <span data-ttu-id="c0863-130">`[StringLength]`: 文字列プロパティが指定した最大長以下であることを検証します。</span><span class="sxs-lookup"><span data-stu-id="c0863-130">`[StringLength]`: Validates that a string property has at most the given maximum length.</span></span>

* <span data-ttu-id="c0863-131">`[Url]`: プロパティが URL 形式であることを検証します。</span><span class="sxs-lookup"><span data-stu-id="c0863-131">`[Url]`: Validates the property has a URL format.</span></span>

<span data-ttu-id="c0863-132">MVC は、検証のために `ValidationAttribute` から派生した任意の属性をサポートします。</span><span class="sxs-lookup"><span data-stu-id="c0863-132">MVC supports any attribute that derives from `ValidationAttribute` for validation purposes.</span></span> <span data-ttu-id="c0863-133">[System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) 名前空間には多くの便利な検証属性が含まれています。</span><span class="sxs-lookup"><span data-stu-id="c0863-133">Many useful validation attributes can be found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace.</span></span>

<span data-ttu-id="c0863-134">組み込みの属性で提供されるものより多くの機能が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-134">There may be instances where you need more features than built-in attributes provide.</span></span> <span data-ttu-id="c0863-135">そのようなときは、`ValidationAttribute` から派生するか、モデルを変更して `IValidatableObject` を実装することにより、カスタム検証属性を作成できます。</span><span class="sxs-lookup"><span data-stu-id="c0863-135">For those times, you can create custom validation attributes by deriving from `ValidationAttribute` or changing your model to implement `IValidatableObject`.</span></span>

## <a name="notes-on-the-use-of-the-required-attribute"></a><span data-ttu-id="c0863-136">Required 属性の使用に関する注意事項</span><span class="sxs-lookup"><span data-stu-id="c0863-136">Notes on the use of the Required attribute</span></span>

<span data-ttu-id="c0863-137">null 非許容の[値の型](/dotnet/csharp/language-reference/keywords/value-types) (`decimal`、`int`、`float`、`DateTime` など) は本質的に必須であり、`Required` 属性を必要としません。</span><span class="sxs-lookup"><span data-stu-id="c0863-137">Non-nullable [value types](/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span> <span data-ttu-id="c0863-138">アプリは、`Required` とマークされている null 非許容型に対して、サーバー側の検証チェックを実行しません。</span><span class="sxs-lookup"><span data-stu-id="c0863-138">The app performs no server-side validation checks for non-nullable types that are marked `Required`.</span></span>

<span data-ttu-id="c0863-139">MVC モデル バインドは、検証および検証属性には関わりがありませんが、null 非許容型に対して欠落値または空白文字を含むフォーム フィールドの送信を却下します。</span><span class="sxs-lookup"><span data-stu-id="c0863-139">MVC model binding, which isn't concerned with validation and validation attributes, rejects a form field submission containing a missing value or whitespace for a non-nullable type.</span></span> <span data-ttu-id="c0863-140">ターゲット プロパティに `BindRequired` 属性がない場合、モデル バインドは null 非許容型の欠落データを無視し、そのフォーム フィールドは受信フォーム データからなくなります。</span><span class="sxs-lookup"><span data-stu-id="c0863-140">In the absence of a `BindRequired` attribute on the target property, model binding ignores missing data for non-nullable types, where the form field is absent from the incoming form data.</span></span>

<span data-ttu-id="c0863-141">[BindRequired 属性](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (「[属性を使用してモデル バインドの動作をカスタマイズする](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)」も参照) は、フォーム データが完全であることを保証するのに便利です。</span><span class="sxs-lookup"><span data-stu-id="c0863-141">The [BindRequired attribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (also see [Customize model binding behavior with attributes](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) is useful to ensure form data is complete.</span></span> <span data-ttu-id="c0863-142">プロパティに適用すると、モデル バインド システムはそのプロパティの値を必須にします。</span><span class="sxs-lookup"><span data-stu-id="c0863-142">When applied to a property, the model binding system requires a value for that property.</span></span> <span data-ttu-id="c0863-143">型に適用すると、モデル バインド システムはその型のすべてのプロパティに対して値を必須にします。</span><span class="sxs-lookup"><span data-stu-id="c0863-143">When applied to a type, the model binding system requires values for all of the properties of that type.</span></span>

<span data-ttu-id="c0863-144">[Nullable\<T> 型](/dotnet/csharp/programming-guide/nullable-types/) (例: `decimal?`、`System.Nullable<decimal>`) を使い、それを `Required` とマークすると、プロパティが標準の null 許容型 (例: `string`) の場合と同様に、サーバー側の検証チェックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="c0863-144">When you use a [Nullable\<T> type](/dotnet/csharp/programming-guide/nullable-types/) (for example, `decimal?` or `System.Nullable<decimal>`) and mark it `Required`, a server-side validation check is performed as if the property were a standard nullable type (for example, a `string`).</span></span>

<span data-ttu-id="c0863-145">クライアント側検証では、`Required` とマークされているモデル プロパティに対応するフォーム フィールドの値、および `Required` とマークされていない null 非許容型のプロパティの値を必須にします。</span><span class="sxs-lookup"><span data-stu-id="c0863-145">Client-side validation requires a value for a form field that corresponds to a model property that you've marked `Required` and for a non-nullable type property that you haven't marked `Required`.</span></span> <span data-ttu-id="c0863-146">`Required` を使って、クライアント側検証のエラー メッセージを制御できます。</span><span class="sxs-lookup"><span data-stu-id="c0863-146">`Required` can be used to control the client-side validation error message.</span></span>

## <a name="model-state"></a><span data-ttu-id="c0863-147">モデルの状態</span><span class="sxs-lookup"><span data-stu-id="c0863-147">Model State</span></span>

<span data-ttu-id="c0863-148">モデルの状態は、送信された HTML フォーム値での検証エラーを表します。</span><span class="sxs-lookup"><span data-stu-id="c0863-148">Model state represents validation errors in submitted HTML form values.</span></span>

<span data-ttu-id="c0863-149">MVC は、エラー数の最大数 (既定値は 200) に達するまで、フィールドの検証を続けます。</span><span class="sxs-lookup"><span data-stu-id="c0863-149">MVC will continue validating fields until reaches the maximum number of errors (200 by default).</span></span> <span data-ttu-id="c0863-150">*Startup.cs* ファイルの `ConfigureServices` メソッドに次のコードを挿入することで、この値を構成できます。</span><span class="sxs-lookup"><span data-stu-id="c0863-150">You can configure this number by inserting the following code into the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a><span data-ttu-id="c0863-151">モデルの状態エラーの処理</span><span class="sxs-lookup"><span data-stu-id="c0863-151">Handling Model State Errors</span></span>

<span data-ttu-id="c0863-152">モデル検証は各コントローラー アクションが呼び出される前に行われます。`ModelState.IsValid` を検査し、適切に対処するのはアクション メソッドの仕事です。</span><span class="sxs-lookup"><span data-stu-id="c0863-152">Model validation occurs prior to each controller action being invoked, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="c0863-153">多くの場合において適切な対応は、エラー応答を返すことであり、モデルの検証が失敗した理由を詳しく示すのが理想的です。</span><span class="sxs-lookup"><span data-stu-id="c0863-153">In many cases, the appropriate reaction is to return an error response, ideally detailing the reason why model validation failed.</span></span>

<span data-ttu-id="c0863-154">一部のアプリでは、モデル検証エラーを処理するとき、標準的な規則に従います。その場合、そのようなポリシーを実装する場所としてフィルターが適していることがあります。</span><span class="sxs-lookup"><span data-stu-id="c0863-154">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a filter may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="c0863-155">モデルの状態が有効なとき、および無効なときにアクションがどのように動作するかテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-155">You should test how your actions behave with valid and invalid model states.</span></span>

## <a name="manual-validation"></a><span data-ttu-id="c0863-156">手動検証</span><span class="sxs-lookup"><span data-stu-id="c0863-156">Manual validation</span></span>

<span data-ttu-id="c0863-157">モデル バインドと検証が完了した後、その一部を繰り返すことが必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-157">After model binding and validation are complete, you may want to repeat parts of it.</span></span> <span data-ttu-id="c0863-158">たとえば、整数が想定されているフィールドにユーザーがテキストを入力したかもしれない場合や、モデルのプロパティの値を計算する必要がある場合などです。</span><span class="sxs-lookup"><span data-stu-id="c0863-158">For example, a user may have entered text in a field expecting an integer, or you may need to compute a value for a model's property.</span></span>

<span data-ttu-id="c0863-159">検証の手動実行が必要になることがあります。</span><span class="sxs-lookup"><span data-stu-id="c0863-159">You may need to run validation manually.</span></span> <span data-ttu-id="c0863-160">その場合は、次に示すように `TryValidateModel` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="c0863-160">To do so, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a><span data-ttu-id="c0863-161">カスタム検証</span><span class="sxs-lookup"><span data-stu-id="c0863-161">Custom validation</span></span>

<span data-ttu-id="c0863-162">検証属性の機能は、ほとんどの検証のニーズに対応します。</span><span class="sxs-lookup"><span data-stu-id="c0863-162">Validation attributes work for most validation needs.</span></span> <span data-ttu-id="c0863-163">しかし、中にはご自身のビジネスに特化した検証ルールもあります。</span><span class="sxs-lookup"><span data-stu-id="c0863-163">However, some validation rules are specific to your business.</span></span> <span data-ttu-id="c0863-164">そのルールは、必須フィールドや値範囲の確認などの一般的なデータ検証方法ではない場合があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-164">Your rules might not be common data validation techniques such as ensuring a field is required or that it conforms to a range of values.</span></span> <span data-ttu-id="c0863-165">そのような場合は、カスタム検証属性が優れた解決策です。</span><span class="sxs-lookup"><span data-stu-id="c0863-165">For these scenarios, custom validation attributes are a great solution.</span></span> <span data-ttu-id="c0863-166">MVC では独自のカスタム検証属性を簡単に作成できます。</span><span class="sxs-lookup"><span data-stu-id="c0863-166">Creating your own custom validation attributes in MVC is easy.</span></span> <span data-ttu-id="c0863-167">`ValidationAttribute` から継承し、`IsValid` メソッドをオーバーライドするだけです。</span><span class="sxs-lookup"><span data-stu-id="c0863-167">Just inherit from the `ValidationAttribute`, and override the `IsValid` method.</span></span> <span data-ttu-id="c0863-168">`IsValid` メソッドは 2 つのパラメーターを受け取ります。1 番目は *value* という名前のオブジェクトで、2 番目は *validationContext* という名前の `ValidationContext` オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="c0863-168">The `IsValid` method accepts two parameters, the first is an object named *value* and the second is a `ValidationContext` object named *validationContext*.</span></span> <span data-ttu-id="c0863-169">*Value* は、カスタム検証メソッドで検証するフィールドの実際の値を示します。</span><span class="sxs-lookup"><span data-stu-id="c0863-169">*Value* refers to the actual value from the field that your custom validator is validating.</span></span>

<span data-ttu-id="c0863-170">次のサンプルのビジネス ルールでは、ユーザーは 1960 年より後にリリースされた映画のジャンルを *Classic* に設定できないことになっています。</span><span class="sxs-lookup"><span data-stu-id="c0863-170">In the following sample, a business rule states that users may not set the genre to *Classic* for a movie released after 1960.</span></span> <span data-ttu-id="c0863-171">`[ClassicMovie]` 属性は最初にジャンルをチェックし、それが Classic である場合、次にリリース日をチェックして、それが 1960 年以降であるかを確認します。</span><span class="sxs-lookup"><span data-stu-id="c0863-171">The `[ClassicMovie]` attribute checks the genre first, and if it's a classic, then it checks the release date to see that it's later than 1960.</span></span> <span data-ttu-id="c0863-172">1960 年より後にリリースされている場合、検証は失敗します。</span><span class="sxs-lookup"><span data-stu-id="c0863-172">If it's released after 1960, validation fails.</span></span> <span data-ttu-id="c0863-173">この属性には、データの検証に使うことができる、年を表す整数パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="c0863-173">The attribute accepts an integer parameter representing the year that you can use to validate data.</span></span> <span data-ttu-id="c0863-174">次のように、属性のコンストラクターでパラメーターの値をキャプチャすることができます。</span><span class="sxs-lookup"><span data-stu-id="c0863-174">You can capture the value of the parameter in the attribute's constructor, as shown here:</span></span>

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=9-28)]

<span data-ttu-id="c0863-175">上の `movie` 変数は、フォーム送信からのデータを検証のために格納している `Movie` オブジェクトを表します。</span><span class="sxs-lookup"><span data-stu-id="c0863-175">The `movie` variable above represents a `Movie` object that contains the data from the form submission to validate.</span></span> <span data-ttu-id="c0863-176">この例では、検証コードはルールに従って `ClassicMovieAttribute` クラスの `IsValid` メソッドで日付とジャンルをチェックします。</span><span class="sxs-lookup"><span data-stu-id="c0863-176">In this case, the validation code checks the date and genre in the `IsValid` method of the `ClassicMovieAttribute` class as per the rules.</span></span> <span data-ttu-id="c0863-177">検証に成功すると、`IsValid` によって `ValidationResult.Success` コードが返されます。</span><span class="sxs-lookup"><span data-stu-id="c0863-177">Upon successful validation ,`IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="c0863-178">検証に失敗すると、`ValidationResult` が次のエラー メッセージとともに返されます。</span><span class="sxs-lookup"><span data-stu-id="c0863-178">When validation fails, a `ValidationResult` with an error message is returned:</span></span>

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=55-58)]

<span data-ttu-id="c0863-179">ユーザーが `Genre` フィールドを変更してフォームを送信すると、`ClassicMovieAttribute` の `IsValid` メソッドは映画が Classic かどうかを検証します。</span><span class="sxs-lookup"><span data-stu-id="c0863-179">When a user modifies the `Genre` field and submits the form, the `IsValid` method of the `ClassicMovieAttribute` will verify whether the movie is a classic.</span></span> <span data-ttu-id="c0863-180">他の組み込み属性と同じように、`ClassicMovieAttribute` を `ReleaseDate` などのプロパティに適用し、検証が確実に行われるようにします (前のコード例を参照)。</span><span class="sxs-lookup"><span data-stu-id="c0863-180">Like any built-in attribute, apply the `ClassicMovieAttribute` to a property such as `ReleaseDate` to ensure validation happens, as shown in the previous code sample.</span></span> <span data-ttu-id="c0863-181">この例は `Movie` 型でのみ機能するので、次の段落で示すように、`IValidatableObject` を使うのがさらによい方法です。</span><span class="sxs-lookup"><span data-stu-id="c0863-181">Since the example works only with `Movie` types, a better option is to use `IValidatableObject` as shown in the following paragraph.</span></span>

<span data-ttu-id="c0863-182">または、`IValidatableObject` インターフェイスに `Validate` メソッドを実装することで、これと同じコードをモデルに配置することもできます。</span><span class="sxs-lookup"><span data-stu-id="c0863-182">Alternatively, this same code could be placed in the model by implementing the `Validate` method on the `IValidatableObject` interface.</span></span> <span data-ttu-id="c0863-183">カスタム検証属性は個別のプロパティを検証する場合に使えるのに対し、`IValidatableObject` の実装は、ここで示すようにクラス レベルの検証を実装するために使うことができます。</span><span class="sxs-lookup"><span data-stu-id="c0863-183">While custom validation attributes work well for validating individual properties, implementing `IValidatableObject` can be used to implement class-level validation as seen here.</span></span>

[!code-csharp[](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a><span data-ttu-id="c0863-184">クライアント側の検証</span><span class="sxs-lookup"><span data-stu-id="c0863-184">Client side validation</span></span>

<span data-ttu-id="c0863-185">クライアント側の検証は、ユーザーにとって非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="c0863-185">Client side validation is a great convenience for users.</span></span> <span data-ttu-id="c0863-186">他の方法を使用した場合に必要な、サーバーへのラウンド トリップを待つ時間が節約できます。</span><span class="sxs-lookup"><span data-stu-id="c0863-186">It saves time they would otherwise spend waiting for a round trip to the server.</span></span> <span data-ttu-id="c0863-187">ビジネス的に表現すると、1 回だけ見ればわずかな時間でも、それが毎日何百回も積み重なると、膨大な時間、費用、フラストレーションになります。</span><span class="sxs-lookup"><span data-stu-id="c0863-187">In business terms, even a few fractions of seconds multiplied hundreds of times each day adds up to be a lot of time, expense, and frustration.</span></span> <span data-ttu-id="c0863-188">簡単かつ迅速な検証は、ユーザーの作業をいっそう効率的にし、入力と出力の品質が向上します。</span><span class="sxs-lookup"><span data-stu-id="c0863-188">Straightforward and immediate validation enables users to work more efficiently and produce better quality input and output.</span></span>

<span data-ttu-id="c0863-189">クライアント側検証が動作するには、ここで示すように、適切な JavaScript スクリプト参照をビューに設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-189">You must have a view with the proper JavaScript script references in place for client side validation to work as you see here.</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

<span data-ttu-id="c0863-190">[jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) スクリプトは、人気のある [jQuery Validate](https://jqueryvalidation.org/) プラグインを基に作成された Microsoft のカスタム フロントエンド ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="c0863-190">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="c0863-191">jQuery Unobtrusive Validation を使わないと、同じ検証ロジックを 2 か所でコーディングする必要があります。1 つはモデル プロパティでのサーバー側検証属性で、もう 1 つはクライアント側スクリプトです (jQuery Validate の [`validate()`](https://jqueryvalidation.org/validate/) メソッドの例を見ると、これがどれほど複雑になるかがわかります)。</span><span class="sxs-lookup"><span data-stu-id="c0863-191">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server side validation attributes on model properties, and then again in client side scripts (the examples for jQuery Validate's [`validate()`](https://jqueryvalidation.org/validate/) method shows how complex this could become).</span></span> <span data-ttu-id="c0863-192">代わりに、MVC の[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)および [HTML ヘルパー](xref:mvc/views/overview)では、モデル プロパティの検証属性と型メタデータを使って、検証の必要なフォーム要素に HTML 5 の [data- 属性](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)をレンダリングできます。</span><span class="sxs-lookup"><span data-stu-id="c0863-192">Instead, MVC's [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) are able to use the validation attributes and type metadata from model properties to render HTML 5 [data- attributes](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) in the form elements that need validation.</span></span> <span data-ttu-id="c0863-193">MVC は、組み込み属性とカスタム属性の両方に対して、`data-` 属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="c0863-193">MVC generates the `data-` attributes for both built-in and custom attributes.</span></span> <span data-ttu-id="c0863-194">その後、jQuery Unobtrusive Validation は `data-` 属性を解析し、ロジックを jQuery Validate に渡して、クライアントにサーバー側検証ロジックを実質的に "コピー" します。</span><span class="sxs-lookup"><span data-stu-id="c0863-194">jQuery Unobtrusive Validation then parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server side validation logic to the client.</span></span> <span data-ttu-id="c0863-195">次に示すように、関連するタグ ヘルパーを使って、クライアントで検証エラーを表示できます。</span><span class="sxs-lookup"><span data-stu-id="c0863-195">You can display validation errors on the client using the relevant tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

<span data-ttu-id="c0863-196">上記のタグ ヘルパーは、以下の HTML をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="c0863-196">The tag helpers above render the HTML below.</span></span> <span data-ttu-id="c0863-197">HTML 出力の `data-` 属性が、`ReleaseDate` プロパティの検証属性に対応していることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="c0863-197">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="c0863-198">下の `data-val-required` 属性には、ユーザーがリリース日フィールドを入力していない場合に表示するエラー メッセージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c0863-198">The `data-val-required` attribute below contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="c0863-199">jQuery Unobtrusive Validation はこの値を jQuery Validate の [`required()`](https://jqueryvalidation.org/required-method/) メソッドに渡し、このメソッドは付随する **\<span>** 要素にそのメッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="c0863-199">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

<span data-ttu-id="c0863-200">クライアント側検証は、フォームが有効になるまで送信を許可しません。</span><span class="sxs-lookup"><span data-stu-id="c0863-200">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="c0863-201">送信ボタンをクリックすると、フォームの送信またはエラー メッセージの表示を行う JavaScript が実行されます。</span><span class="sxs-lookup"><span data-stu-id="c0863-201">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="c0863-202">MVC はプロパティの .NET データ型に基づいて型属性の値を特定しますが、場合によってはこの値は `[DataType]` 属性を使ってオーバーライドされています。</span><span class="sxs-lookup"><span data-stu-id="c0863-202">MVC determines type attribute values based on the .NET data type of a property, possibly overridden using `[DataType]` attributes.</span></span> <span data-ttu-id="c0863-203">基本 `[DataType]` 属性では、実際のサーバー側検証は行われません。</span><span class="sxs-lookup"><span data-stu-id="c0863-203">The base `[DataType]` attribute does no real server-side validation.</span></span> <span data-ttu-id="c0863-204">ブラウザーでは独自のエラー メッセージが選択され、自由にエラーが表示されますが、jQuery Validation Unobtrusive パッケージではメッセージをオーバーライドし、他と整合性のあるメッセージを表示できます。</span><span class="sxs-lookup"><span data-stu-id="c0863-204">Browsers choose their own error messages and display those errors as they wish, however the jQuery Validation Unobtrusive package can override the messages and display them consistently with others.</span></span> <span data-ttu-id="c0863-205">これが最もよくわかるのは、ユーザーが `[EmailAddress]` などの `[DataType]` サブクラスを適用した場合です。</span><span class="sxs-lookup"><span data-stu-id="c0863-205">This happens most obviously when users apply `[DataType]` subclasses such as `[EmailAddress]`.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="c0863-206">動的なフォームに検証を追加する</span><span class="sxs-lookup"><span data-stu-id="c0863-206">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="c0863-207">jQuery Unobtrusive Validation はページが初めて読み込まれるときに検証ロジックとパラメーターを jQuery Validate に渡すので、動的に生成されるフォームでは、検証は自動的には行われません。</span><span class="sxs-lookup"><span data-stu-id="c0863-207">Because jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads, dynamically generated forms won't automatically exhibit validation.</span></span> <span data-ttu-id="c0863-208">代わりに、作成直後に動的フォームを解析するよう、jQuery Unobtrusive Validation に指示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-208">Instead, you must tell jQuery Unobtrusive Validation to parse the dynamic form immediately after creating it.</span></span> <span data-ttu-id="c0863-209">たとえば、次のコードは、AJAX によって追加されるフォームでクライアント側検証を設定する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="c0863-209">For example, the code below shows how you might set up client side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

<span data-ttu-id="c0863-210">`$.validator.unobtrusive.parse()` メソッドには、その引数の 1 つで jQuery セレクターを指定します。</span><span class="sxs-lookup"><span data-stu-id="c0863-210">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="c0863-211">このメソッドは、そのセレクター内のフォームの `data-` 属性を解析するよう jQuery Unobtrusive Validation に指示します。</span><span class="sxs-lookup"><span data-stu-id="c0863-211">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="c0863-212">その後、これらの属性の値が jQuery Validate プラグインに渡され、必要なクライアント側検証ルールがフォームで示されます。</span><span class="sxs-lookup"><span data-stu-id="c0863-212">The values of those attributes are then passed to the jQuery Validate plugin so that the form exhibits the desired client side validation rules.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="c0863-213">動的なコントロールに検証を追加する</span><span class="sxs-lookup"><span data-stu-id="c0863-213">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="c0863-214">`<input/>` や `<select/>` などの個別のコントロールが動的に生成されるときに、フォームの検証ルールを更新することもできます。</span><span class="sxs-lookup"><span data-stu-id="c0863-214">You can also update the validation rules on a form when individual controls, such as `<input/>`s and `<select/>`s, are dynamically generated.</span></span> <span data-ttu-id="c0863-215">これらの要素のセレクターを `parse()` メソッドに直接渡すことはできません。格納しているフォームが既に解析されていて更新されないためです。</span><span class="sxs-lookup"><span data-stu-id="c0863-215">You cannot pass selectors for these elements to the `parse()` method directly because the surrounding form has already been parsed and won't update.</span></span> <span data-ttu-id="c0863-216">代わりに、まず既存の検証データを削除した後、次に示すようにフォーム全体を再解析します。</span><span class="sxs-lookup"><span data-stu-id="c0863-216">Instead, you first remove the existing validation data, then reparse the entire form, as shown below:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a><span data-ttu-id="c0863-217">IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="c0863-217">IClientModelValidator</span></span>

<span data-ttu-id="c0863-218">カスタム属性のクライアント側ロジックを作成することができ、[jquery 検証](http://jqueryvalidation.org/documentation/)へのアダプターを作成する [Unobtrusive Validation](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) はそれを検証の一部としてクライアントで自動的に実行します。</span><span class="sxs-lookup"><span data-stu-id="c0863-218">You may create client side logic for your custom attribute, and [unobtrusive validation](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) which creates an adapter to [jquery validation](http://jqueryvalidation.org/documentation/) will execute it on the client for you automatically as part of validation.</span></span> <span data-ttu-id="c0863-219">最初に、次に示すように `IClientModelValidator` インターフェイスを実装することで、追加される data- 属性を制御します。</span><span class="sxs-lookup"><span data-stu-id="c0863-219">The first step is to control what data- attributes are added by implementing the `IClientModelValidator` interface as shown here:</span></span>

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

<span data-ttu-id="c0863-220">このインターフェイスを実装する属性は、生成されるフィールドに HTML 属性を追加できます。</span><span class="sxs-lookup"><span data-stu-id="c0863-220">Attributes that implement this interface can add HTML attributes to generated fields.</span></span> <span data-ttu-id="c0863-221">`ReleaseDate` 要素の出力を調べると、前の例と似た HTML であることがわかりますが、ここでは、`IClientModelValidator` の `AddValidation` メソッドで定義された `data-val-classicmovie` 属性があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-221">Examining the output for the `ReleaseDate` element reveals HTML that's similar to the previous example, except now there's a `data-val-classicmovie` attribute that was defined in the `AddValidation` method of `IClientModelValidator`.</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

<span data-ttu-id="c0863-222">Unobtrusive Validation は、`data-` 属性のデータを使ってエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="c0863-222">Unobtrusive validation uses the data in the `data-` attributes to display error messages.</span></span> <span data-ttu-id="c0863-223">ただし、jQuery が規則やメッセージを認識するのは、jQuery の `validator` オブジェクトにそれらが追加されてからです。</span><span class="sxs-lookup"><span data-stu-id="c0863-223">However, jQuery doesn't know about rules or messages until you add them to jQuery's `validator` object.</span></span> <span data-ttu-id="c0863-224">次の例でこれを示します。ここでは、カスタムの `classicmovie` クライアント検証メソッドが、jQuery `validator` オブジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="c0863-224">This is shown in the following example, which adds a custom `classicmovie` client validation method to the jQuery `validator` object.</span></span> <span data-ttu-id="c0863-225">`unobtrusive.adapters.add` メソッドの説明については、[ASP.NET MVC での控え目なクライアント検証](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="c0863-225">For an explanation of the `unobtrusive.adapters.add` method, see [Unobtrusive Client Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html).</span></span>

[!code-javascript[](validation/sample/Views/Movies/Create.cshtml?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="c0863-226">上のコードでは、`classicmovie` メソッドが映画のリリース日のクライアント側検証を実行しています。</span><span class="sxs-lookup"><span data-stu-id="c0863-226">With the preceding code, the `classicmovie` method performs client-side validation on the movie release date.</span></span> <span data-ttu-id="c0863-227">メソッドが `false` を返す場合、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c0863-227">The error message displays if the method returns `false`.</span></span>

## <a name="remote-validation"></a><span data-ttu-id="c0863-228">リモート検証</span><span class="sxs-lookup"><span data-stu-id="c0863-228">Remote validation</span></span>

<span data-ttu-id="c0863-229">リモート検証は、クライアント上のデータをサーバー上のデータに対して検証する必要があるときに使う優れた機能です。</span><span class="sxs-lookup"><span data-stu-id="c0863-229">Remote validation is a great feature to use when you need to validate data on the client against data on the server.</span></span> <span data-ttu-id="c0863-230">たとえば、アプリでメール アドレスやユーザー名が既に使われているかどうかを検証する必要があり、そのために大量のデータをクエリする必要があるような場合です。</span><span class="sxs-lookup"><span data-stu-id="c0863-230">For example, your app may need to verify whether an email or user name is already in use, and it must query a large amount of data to do so.</span></span> <span data-ttu-id="c0863-231">1 つまたは少数のフィールドを検証するために大きなデータ セットをダウンロードするのでは、消費するリソースが多すぎます。</span><span class="sxs-lookup"><span data-stu-id="c0863-231">Downloading large sets of data for validating one or a few fields consumes too many resources.</span></span> <span data-ttu-id="c0863-232">機密情報が漏えいする可能性もあります。</span><span class="sxs-lookup"><span data-stu-id="c0863-232">It may also expose sensitive information.</span></span> <span data-ttu-id="c0863-233">代わりの方法として、ラウンド トリップ要求を行って、フィールドを検証します。</span><span class="sxs-lookup"><span data-stu-id="c0863-233">An alternative is to make a round-trip request to validate a field.</span></span>

<span data-ttu-id="c0863-234">リモート検証は、2 ステップのプロセスで実装できます。</span><span class="sxs-lookup"><span data-stu-id="c0863-234">You can implement remote validation in a two step process.</span></span> <span data-ttu-id="c0863-235">最初に、`[Remote]` 属性でモデルに注釈を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-235">First, you must annotate your model with the `[Remote]` attribute.</span></span> <span data-ttu-id="c0863-236">`[Remote]` 属性には複数のオーバーロードが指定できるので、それを使って、クライアント側の JavaScript が適切なコードを呼び出すようにします。</span><span class="sxs-lookup"><span data-stu-id="c0863-236">The `[Remote]` attribute accepts multiple overloads you can use to direct client side JavaScript to the appropriate code to call.</span></span> <span data-ttu-id="c0863-237">次の例では、`Users` コントローラーの `VerifyEmail` アクション メソッドを指し示しています。</span><span class="sxs-lookup"><span data-stu-id="c0863-237">The example below points to the `VerifyEmail` action method of the `Users` controller.</span></span>

[!code-csharp[](validation/sample/User.cs?range=7-8)]

<span data-ttu-id="c0863-238">2 番目のステップでは、`[Remote]` 属性で定義されている対応するアクション メソッドに、検証コードを配置します。</span><span class="sxs-lookup"><span data-stu-id="c0863-238">The second step is putting the validation code in the corresponding action method as defined in the `[Remote]` attribute.</span></span> <span data-ttu-id="c0863-239">Jquery Validate の[リモート](https://jqueryvalidation.org/remote-method/) メソッドに関するドキュメントによれば、サーバーの応答は次のいずれかの JSON 文字列である必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-239">According to the jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method documentation, the server response must be a JSON string that's either:</span></span>

* <span data-ttu-id="c0863-240">有効な要素の場合は `"true"`。</span><span class="sxs-lookup"><span data-stu-id="c0863-240">`"true"` for valid elements.</span></span>
* <span data-ttu-id="c0863-241">無効な要素の場合は `"false"`、`undefined`、または `null` (既定のエラー メッセージを使用)。</span><span class="sxs-lookup"><span data-stu-id="c0863-241">`"false"`, `undefined`, or `null` for invalid elements, using the default error message.</span></span>

<span data-ttu-id="c0863-242">サーバーの応答が文字列 (例: `"That name is already taken, try peter123 instead"`) である場合、文字列は既定の文字列の代わりにカスタム エラー メッセージとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="c0863-242">If the server response is a string (for example, `"That name is already taken, try peter123 instead"`), the string is displayed as a custom error message in place of the default string.</span></span>

<span data-ttu-id="c0863-243">次に示すように、`VerifyEmail` メソッドの定義はこれらの規則に従います。</span><span class="sxs-lookup"><span data-stu-id="c0863-243">The definition of the `VerifyEmail` method follows these rules, as shown below.</span></span> <span data-ttu-id="c0863-244">このメソッドは、メール アドレスが使われている場合は検証エラー メッセージを返し、メール アドレスが空いている場合は `true` を返します。また、結果を `JsonResult` オブジェクトにラップします。</span><span class="sxs-lookup"><span data-stu-id="c0863-244">It returns a validation error message if the email is taken, or `true` if the email is free, and wraps the result in a `JsonResult` object.</span></span> <span data-ttu-id="c0863-245">クライアント側では、返された値を使って、続行するか、必要な場合はエラーを表示できます。</span><span class="sxs-lookup"><span data-stu-id="c0863-245">The client side can then use the returned value to proceed or display the error if needed.</span></span>

[!code-csharp[](validation/sample/UsersController.cs?range=19-28)]

<span data-ttu-id="c0863-246">ユーザーがメール アドレスを入力すると、ビューの JavaScript はリモート呼び出しを行って、そのメール アドレスが使われているかどうかを確認し、使われている場合はエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="c0863-246">Now when users enter an email, JavaScript in the view makes a remote call to see if that email has been taken and, if so, displays the error message.</span></span> <span data-ttu-id="c0863-247">使われていない場合は、ユーザーは普通にフォームを送信できます。</span><span class="sxs-lookup"><span data-stu-id="c0863-247">Otherwise, the user can submit the form as usual.</span></span>

<span data-ttu-id="c0863-248">`[Remote]` 属性の `AdditionalFields` プロパティは、フィールドの組み合わせをサーバー上のデータに対して検証するときに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="c0863-248">The `AdditionalFields` property of the `[Remote]` attribute is useful for validating combinations of fields against data on the server.</span></span> <span data-ttu-id="c0863-249">たとえば、上の `User` モデルに `FirstName` および `LastName` という名前の 2 つの追加プロパティがある場合、その名前のペアを使う既存ユーザーがいないことを確認したい場合があるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="c0863-249">For example, if the `User` model from above had two additional properties called `FirstName` and `LastName`, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="c0863-250">次のコードで示すように、新しいプロパティを定義します。</span><span class="sxs-lookup"><span data-stu-id="c0863-250">You define the new properties as shown in the following code:</span></span>

[!code-csharp[](validation/sample/User.cs?range=10-13)]

<span data-ttu-id="c0863-251">`AdditionalFields` を文字列 `"FirstName"` および `"LastName"` に明示的に設定することもできますが、[`nameof`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) 演算子をこのように使うと、後のリファクタリングが容易になります。</span><span class="sxs-lookup"><span data-stu-id="c0863-251">`AdditionalFields` could've been set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) operator like this simplifies later refactoring.</span></span> <span data-ttu-id="c0863-252">検証を実行するアクション メソッドには、`FirstName` の値と `LastName` の値のそれぞれに対応する 2 つの引数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-252">The action method to perform the validation must then accept two arguments, one for the value of `FirstName` and one for the value of `LastName`.</span></span>

[!code-csharp[](validation/sample/UsersController.cs?range=30-39)]

<span data-ttu-id="c0863-253">ここで、ユーザーが姓と名を入力すると、JavaScript は以下のことを行います。</span><span class="sxs-lookup"><span data-stu-id="c0863-253">Now when users enter a first and last name, JavaScript:</span></span>

* <span data-ttu-id="c0863-254">リモート呼び出しを行って、その名前のペアが使われているかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="c0863-254">Makes a remote call to see if that pair of names has been taken.</span></span>
* <span data-ttu-id="c0863-255">ペアが使われている場合は、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c0863-255">If the pair has been taken, an error message is displayed.</span></span>
* <span data-ttu-id="c0863-256">使われていない場合は、ユーザーはフォームを送信できます。</span><span class="sxs-lookup"><span data-stu-id="c0863-256">If not taken, the user can submit the form.</span></span>

<span data-ttu-id="c0863-257">複数の追加フィールドを `[Remote]` 属性で検証する必要がある場合は、コンマ区切りのリストとして提供します。</span><span class="sxs-lookup"><span data-stu-id="c0863-257">If you need to validate two or more additional fields with the `[Remote]` attribute, you provide them as a comma-delimited list.</span></span> <span data-ttu-id="c0863-258">たとえば、`MiddleName` プロパティをモデルに追加するには、`[Remote]` 属性を次のコードのように設定します。</span><span class="sxs-lookup"><span data-stu-id="c0863-258">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following code:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="c0863-259">他の属性引数と同じように、`AdditionalFields` も定数式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-259">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="c0863-260">したがって、[補間文字列](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings)を使ったり、[`string.Join()`](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) を呼び出して `AdditionalFields` を初期化したりすることはできません。</span><span class="sxs-lookup"><span data-stu-id="c0863-260">Therefore, you must not use an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings) or call [`string.Join()`](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) to initialize `AdditionalFields`.</span></span> <span data-ttu-id="c0863-261">`[Remote]` 属性に新しいフィールドを追加するごとに、対応するコントローラー アクション メソッドに別の引数を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0863-261">For every additional field that you add to the `[Remote]` attribute, you must add another argument to the corresponding controller action method.</span></span>
