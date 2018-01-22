---
title: "ASP.NET Core MVC でのモデルの検証"
author: rachelappel
description: "ASP.NET Core MVC でのモデルの検証について説明します。"
ms.author: riande
manager: wpickett
ms.date: 12/18/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.openlocfilehash: 91db17e103723ac411a2ad4f3f9549860f250cce
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a><span data-ttu-id="40f6d-103">ASP.NET Core MVC でのモデル検証の概要</span><span class="sxs-lookup"><span data-stu-id="40f6d-103">Introduction to model validation in ASP.NET Core MVC</span></span>

<span data-ttu-id="40f6d-104">によって[Rachel Appel](https://github.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="40f6d-104">By [Rachel Appel](https://github.com/rachelappel)</span></span>

## <a name="introduction-to-model-validation"></a><span data-ttu-id="40f6d-105">モデルの検証の概要</span><span class="sxs-lookup"><span data-stu-id="40f6d-105">Introduction to model validation</span></span>

<span data-ttu-id="40f6d-106">アプリは、データベースのデータを保存する前に、アプリは、データを検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-106">Before an app stores data in a database, the app must validate the data.</span></span> <span data-ttu-id="40f6d-107">潜在的なセキュリティ上の脅威は、種類とサイズで適切に書式設定し、ルールに従ってくださいことを確認のためには、データをチェックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-107">Data must be checked for potential security threats, verified that it is appropriately formatted by type and size, and it must conform to your rules.</span></span> <span data-ttu-id="40f6d-108">冗長と実装が煩雑でできますが、検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-108">Validation is necessary although it can be redundant and tedious to implement.</span></span> <span data-ttu-id="40f6d-109">MVC では、検証は、クライアントとサーバーの両方で発生します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-109">In MVC, validation happens on both the client and server.</span></span>

<span data-ttu-id="40f6d-110">さいわい、.NET には、検証属性に検証が抽象化します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-110">Fortunately, .NET has abstracted validation into validation attributes.</span></span> <span data-ttu-id="40f6d-111">これらの属性には、検証コード記述するコードの量を減らすにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="40f6d-111">These attributes contain validation code, thereby reducing the amount of code you must write.</span></span>

<span data-ttu-id="40f6d-112">[GitHub からサンプルのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-112">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).</span></span>

## <a name="validation-attributes"></a><span data-ttu-id="40f6d-113">検証属性</span><span class="sxs-lookup"><span data-stu-id="40f6d-113">Validation Attributes</span></span>

<span data-ttu-id="40f6d-114">検証属性は、データベース テーブル内のフィールドの検証に概念的に似ていますので、モデルの検証を構成する方法です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-114">Validation attributes are a way to configure model validation so it's similar conceptually to validation on fields in database tables.</span></span> <span data-ttu-id="40f6d-115">これには、データ型または必要なフィールドの割り当てなどの制約が含まれます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-115">This includes constraints such as assigning data types or required fields.</span></span> <span data-ttu-id="40f6d-116">その他の種類検証にはでは、電子メール アドレスまたは電話番号、クレジット カードなど、ビジネス ルールを適用するデータにパターンを適用することが含まれます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-116">Other types of validation include applying patterns to data to enforce business rules, such as a credit card, phone number, or email address.</span></span> <span data-ttu-id="40f6d-117">検証属性は、大幅に簡素化され、使いやすく、これらの要件を適用することを確認します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-117">Validation attributes make enforcing these requirements much simpler and easier to use.</span></span>

<span data-ttu-id="40f6d-118">以下は、注釈付き`Movie`ムービーやテレビ番組に関する情報を格納するアプリケーションからモデル。</span><span class="sxs-lookup"><span data-stu-id="40f6d-118">Below is an annotated `Movie` model from an app that stores information about movies and TV shows.</span></span> <span data-ttu-id="40f6d-119">ほとんどのプロパティが必要ないくつかの文字列プロパティの長さの要件があります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-119">Most of the properties are required and several string properties have length requirements.</span></span> <span data-ttu-id="40f6d-120">さらのために数値の範囲の制限がある、`Price`プロパティを 0 から $999.99、カスタム検証属性と共ににします。</span><span class="sxs-lookup"><span data-stu-id="40f6d-120">Additionally, there is a numeric range restriction in place for the `Price` property from 0 to $999.99, along with a custom validation attribute.</span></span>

[!code-csharp[Main](validation/sample/Movie.cs?range=6-29)]

<span data-ttu-id="40f6d-121">モデル全体を読むだけで、コードを保守しやすいように、このアプリのデータについてのルールが表示されます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-121">Simply reading through the model reveals the rules about data for this app, making it easier to maintain the code.</span></span> <span data-ttu-id="40f6d-122">いくつかの一般的な組み込みの検証属性を次に示します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-122">Below are several popular built-in validation attributes:</span></span>

* <span data-ttu-id="40f6d-123">`[CreditCard]`: 検証プロパティは、クレジット カード形式です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-123">`[CreditCard]`: Validates the property has a credit card format.</span></span>

* <span data-ttu-id="40f6d-124">`[Compare]`: モデルの検索一致で 2 つのプロパティを検証します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-124">`[Compare]`: Validates two properties in a model match.</span></span>

* <span data-ttu-id="40f6d-125">`[EmailAddress]`: 検証電子メール形式を、プロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-125">`[EmailAddress]`: Validates the property has an email format.</span></span>

* <span data-ttu-id="40f6d-126">`[Phone]`: 検証プロパティは、電話形式です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-126">`[Phone]`: Validates the property has a telephone format.</span></span>

* <span data-ttu-id="40f6d-127">`[Range]`: 特定の範囲内でプロパティの値が検証されます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-127">`[Range]`: Validates the property value falls within the given range.</span></span>

* <span data-ttu-id="40f6d-128">`[RegularExpression]`: データが指定された正規表現と一致することを検証します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-128">`[RegularExpression]`: Validates that the data matches the specified regular expression.</span></span>

* <span data-ttu-id="40f6d-129">`[Required]`: 必要なプロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-129">`[Required]`: Makes a property required.</span></span>

* <span data-ttu-id="40f6d-130">`[StringLength]`: 文字列プロパティに指定された最大長でが多くてことを検証します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-130">`[StringLength]`: Validates that a string property has at most the given maximum length.</span></span>

* <span data-ttu-id="40f6d-131">`[Url]`: を検証、プロパティには、URL の形式です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-131">`[Url]`: Validates the property has a URL format.</span></span>

<span data-ttu-id="40f6d-132">MVC から派生した任意の属性をサポートしている`ValidationAttribute`検証のためです。</span><span class="sxs-lookup"><span data-stu-id="40f6d-132">MVC supports any attribute that derives from `ValidationAttribute` for validation purposes.</span></span> <span data-ttu-id="40f6d-133">多くの便利な検証属性は含まれて、 [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations)名前空間。</span><span class="sxs-lookup"><span data-stu-id="40f6d-133">Many useful validation attributes can be found in the [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) namespace.</span></span>

<span data-ttu-id="40f6d-134">組み込みの属性が用意されてより多くの機能が必要になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-134">There may be instances where you need more features than built-in attributes provide.</span></span> <span data-ttu-id="40f6d-135">派生することによってカスタム検証属性を作成する場合、それらの時間`ValidationAttribute`を実装する、モデルの変更または`IValidatableObject`です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-135">For those times, you can create custom validation attributes by deriving from `ValidationAttribute` or changing your model to implement `IValidatableObject`.</span></span>

## <a name="notes-on-the-use-of-the-required-attribute"></a><span data-ttu-id="40f6d-136">必須の属性の使用に関する注意事項</span><span class="sxs-lookup"><span data-stu-id="40f6d-136">Notes on the use of the Required attribute</span></span>

<span data-ttu-id="40f6d-137">null 非許容の[値の型](/dotnet/csharp/language-reference/keywords/value-types) (`decimal`、`int`、`float`、`DateTime` など) は本質的に必須ではなく、`Required` 属性を必要としません。</span><span class="sxs-lookup"><span data-stu-id="40f6d-137">Non-nullable [value types](/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span> <span data-ttu-id="40f6d-138">アプリ マークされている null 非許容の型のサーバー側の検証チェックを実行しない`Required`です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-138">The app performs no server-side validation checks for non-nullable types that are marked `Required`.</span></span>

<span data-ttu-id="40f6d-139">MVC モデル バインディングは、検証と検証の属性を持つ関係はありませんは、不足値または null 非許容の型の空白文字を含むフォーム フィールドの送信を却下します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-139">MVC model binding, which isn't concerned with validation and validation attributes, rejects a form field submission containing a missing value or whitespace for a non-nullable type.</span></span> <span data-ttu-id="40f6d-140">ない場合、`BindRequired`属性モデル バインディング ターゲット プロパティに null 非許容の型の場合、フォーム フィールドが存在しないデータの欠落が無視受信フォーム データからです。</span><span class="sxs-lookup"><span data-stu-id="40f6d-140">In the absence of a `BindRequired` attribute on the target property, model binding ignores missing data for non-nullable types, where the form field is absent from the incoming form data.</span></span>

<span data-ttu-id="40f6d-141">[BindRequired 属性](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute)(も参照してください[属性を持つモデル バインディング動作をカスタマイズする](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) フォームのデータが完全なことを確認すると便利です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-141">The [BindRequired attribute](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (also see [Customize model binding behavior with attributes](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) is useful to ensure form data is complete.</span></span> <span data-ttu-id="40f6d-142">プロパティに適用する場合、モデル バインド システムには、そのプロパティの値が必要です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-142">When applied to a property, the model binding system requires a value for that property.</span></span> <span data-ttu-id="40f6d-143">型に適用する場合、モデル バインド システムのすべての型のプロパティの値が必要です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-143">When applied to a type, the model binding system requires values for all of the properties of that type.</span></span>

<span data-ttu-id="40f6d-144">使用すると、 [Nullable\<T > 型](/dotnet/csharp/programming-guide/nullable-types/)(たとえば、`decimal?`または`System.Nullable<decimal>`) とマークして`Required`プロパティが、標準の null 許容型 (の場合と同様に、サーバー側の検証チェックが実行されます。例では、 `string`)。</span><span class="sxs-lookup"><span data-stu-id="40f6d-144">When you use a [Nullable\<T> type](/dotnet/csharp/programming-guide/nullable-types/) (for example, `decimal?` or `System.Nullable<decimal>`) and mark it `Required`, a server-side validation check is performed as if the property were a standard nullable type (for example, a `string`).</span></span>

<span data-ttu-id="40f6d-145">クライアント側の検証がマークされているモデル プロパティに対応するフォーム フィールドの値が必要です`Required`とマークしていない型の null 非許容のプロパティの`Required`します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-145">Client-side validation requires a value for a form field that corresponds to a model property that you've marked `Required` and for a non-nullable type property that you haven't marked `Required`.</span></span> <span data-ttu-id="40f6d-146">`Required`クライアント側の検証エラー メッセージを制御するために使用します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-146">`Required` can be used to control the client-side validation error message.</span></span>

## <a name="model-state"></a><span data-ttu-id="40f6d-147">モデルの状態</span><span class="sxs-lookup"><span data-stu-id="40f6d-147">Model State</span></span>

<span data-ttu-id="40f6d-148">モデルの状態では、送信された HTML フォームの値で検証エラーを表します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-148">Model state represents validation errors in submitted HTML form values.</span></span>

<span data-ttu-id="40f6d-149">MVC は引き続きに達するまでフィールドの検証エラー (既定で 200) の最大数。</span><span class="sxs-lookup"><span data-stu-id="40f6d-149">MVC will continue validating fields until reaches the maximum number of errors (200 by default).</span></span> <span data-ttu-id="40f6d-150">この数を構成するには次のコードを挿入することで、`ConfigureServices`メソッドで、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="40f6d-150">You can configure this number by inserting the following code into the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a><span data-ttu-id="40f6d-151">処理のモデル状態エラー</span><span class="sxs-lookup"><span data-stu-id="40f6d-151">Handling Model State Errors</span></span>

<span data-ttu-id="40f6d-152">モデルの検証が呼び出される、各コント ローラー アクションの前に発生しを検査するアクション メソッドの責任である`ModelState.IsValid`して適切に対処します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-152">Model validation occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="40f6d-153">多くの場合は、適切な反応をモデルの検証が失敗した理由の詳細を示す理想的には、エラー応答を返します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-153">In many cases, the appropriate reaction is to return an error response, ideally detailing the reason why model validation failed.</span></span>

<span data-ttu-id="40f6d-154">ケース フィルターがこのようなポリシーを実装する適切な場所をする可能性があります、モデルの検証エラーを処理するための標準的な規則に準拠するアプリの一部を選択します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-154">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a filter may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="40f6d-155">有効および無効なモデルの状態を持つユーザーの操作の動作をテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-155">You should test how your actions behave with valid and invalid model states.</span></span>

## <a name="manual-validation"></a><span data-ttu-id="40f6d-156">手動検証</span><span class="sxs-lookup"><span data-stu-id="40f6d-156">Manual validation</span></span>

<span data-ttu-id="40f6d-157">モデル バインディングと検証の完了後にその一部を繰り返すこともできます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-157">After model binding and validation are complete, you may want to repeat parts of it.</span></span> <span data-ttu-id="40f6d-158">たとえば、ユーザーが正しくありませんテキスト、整数を指定してください フィールドにまたはモデルのプロパティの値を計算する必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-158">For example, a user may have entered text in a field expecting an integer, or you may need to compute a value for a model's property.</span></span>

<span data-ttu-id="40f6d-159">検証を手動で実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-159">You may need to run validation manually.</span></span> <span data-ttu-id="40f6d-160">これを行うには、呼び出し、`TryValidateModel`メソッドを次のように。</span><span class="sxs-lookup"><span data-stu-id="40f6d-160">To do so, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a><span data-ttu-id="40f6d-161">カスタム検証</span><span class="sxs-lookup"><span data-stu-id="40f6d-161">Custom validation</span></span>

<span data-ttu-id="40f6d-162">検証属性の機能のほとんどの検証要件に対応します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-162">Validation attributes work for most validation needs.</span></span> <span data-ttu-id="40f6d-163">ただし、いくつかの検証規則は、ビジネスに固有です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-163">However, some validation rules are specific to your business.</span></span> <span data-ttu-id="40f6d-164">ルールできない可能性があります一般的なデータの検証方法など、フィールドのことを確認することが必要か、値の範囲に準拠していること。</span><span class="sxs-lookup"><span data-stu-id="40f6d-164">Your rules might not be common data validation techniques such as ensuring a field is required or that it conforms to a range of values.</span></span> <span data-ttu-id="40f6d-165">これらのシナリオでは、カスタム検証属性は、優れたソリューションです。</span><span class="sxs-lookup"><span data-stu-id="40f6d-165">For these scenarios, custom validation attributes are a great solution.</span></span> <span data-ttu-id="40f6d-166">MVC での独自のカスタム検証属性の作成は簡単です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-166">Creating your own custom validation attributes in MVC is easy.</span></span> <span data-ttu-id="40f6d-167">継承した、 `ValidationAttribute`、オーバーライド、`IsValid`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="40f6d-167">Just inherit from the `ValidationAttribute`, and override the `IsValid` method.</span></span> <span data-ttu-id="40f6d-168">`IsValid`メソッドは、次の 2 つのパラメーターを受け取り、1 つは、という名前のオブジェクトを*値*、2 番目の`ValidationContext`という名前のオブジェクト*validationContext*です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-168">The `IsValid` method accepts two parameters, the first is an object named *value* and the second is a `ValidationContext` object named *validationContext*.</span></span> <span data-ttu-id="40f6d-169">*値*カスタム検証を検証するフィールドの実際の値を参照します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-169">*Value* refers to the actual value from the field that your custom validator is validating.</span></span>

<span data-ttu-id="40f6d-170">次の例で、ビジネス ルールで制限ユーザーが設定されていないこと、ジャンル*クラシック*1960年以降後にリリースされたムービーにします。</span><span class="sxs-lookup"><span data-stu-id="40f6d-170">In the following sample, a business rule states that users may not set the genre to *Classic* for a movie released after 1960.</span></span> <span data-ttu-id="40f6d-171">`[ClassicMovie]`属性はまず、ジャンルをチェックし、従来型である場合、チェックをリリース日を 1960年よりも後であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-171">The `[ClassicMovie]` attribute checks the genre first, and if it is a classic, then it checks the release date to see that it is later than 1960.</span></span> <span data-ttu-id="40f6d-172">これは、1960年後にリリースされた、検証が失敗します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-172">If it is released after 1960, validation fails.</span></span> <span data-ttu-id="40f6d-173">この属性は、データの検証に使用できる暦を表す整数パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-173">The attribute accepts an integer parameter representing the year that you can use to validate data.</span></span> <span data-ttu-id="40f6d-174">次のように、属性のコンス トラクターのパラメーターの値をキャプチャすることができます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-174">You can capture the value of the parameter in the attribute's constructor, as shown here:</span></span>

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

<span data-ttu-id="40f6d-175">`movie`上を表す変数、`Movie`を検証するフォームの送信からのデータを格納しているオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="40f6d-175">The `movie` variable above represents a `Movie` object that contains the data from the form submission to validate.</span></span> <span data-ttu-id="40f6d-176">検証コードが日とでジャンルをチェックするこの例では、`IsValid`のメソッド、`ClassicMovieAttribute`規則に従ってクラスです。</span><span class="sxs-lookup"><span data-stu-id="40f6d-176">In this case, the validation code checks the date and genre in the `IsValid` method of the `ClassicMovieAttribute` class as per the rules.</span></span> <span data-ttu-id="40f6d-177">検証成功時に`IsValid`を返します、`ValidationResult.Success`コード、および検証に失敗したときに、`ValidationResult`エラー メッセージを使用します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-177">Upon successful validation `IsValid` returns a `ValidationResult.Success` code, and when validation fails, a `ValidationResult` with an error message.</span></span> <span data-ttu-id="40f6d-178">ユーザーが変更すると、`Genre`フィールドに、フォームを送信して、`IsValid`のメソッド、`ClassicMovieAttribute`ムービーが、従来型であるかどうかを確認してください。</span><span class="sxs-lookup"><span data-stu-id="40f6d-178">When a user modifies the `Genre` field and submits the form, the `IsValid` method of the `ClassicMovieAttribute` will verify whether the movie is a classic.</span></span> <span data-ttu-id="40f6d-179">すべての組み込みの属性と同様に適用、`ClassicMovieAttribute`などのプロパティに`ReleaseDate`前のコード例で示すように、検証が発生したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-179">Like any built-in attribute, apply the `ClassicMovieAttribute` to a property such as `ReleaseDate` to ensure validation happens, as shown in the previous code sample.</span></span> <span data-ttu-id="40f6d-180">のみ行われますので`Movie`を使用する型の場合よりよい方法は`IValidatableObject`次の段落に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="40f6d-180">Since the example works only with `Movie` types, a better option is to use `IValidatableObject` as shown in the following paragraph.</span></span>

<span data-ttu-id="40f6d-181">また、この同じコードでしたに配置するモデルを実装して、`Validate`メソッドを`IValidatableObject`インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="40f6d-181">Alternatively, this same code could be placed in the model by implementing the `Validate` method on the `IValidatableObject` interface.</span></span> <span data-ttu-id="40f6d-182">実装するカスタム検証属性は、個々 のプロパティを検証するため、動作、`IValidatableObject`を次に示すようにクラス レベルの検証を実装するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-182">While custom validation attributes work well for validating individual properties, implementing `IValidatableObject` can be used to implement class-level validation as seen here.</span></span>

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a><span data-ttu-id="40f6d-183">クライアント側の検証</span><span class="sxs-lookup"><span data-stu-id="40f6d-183">Client side validation</span></span>

<span data-ttu-id="40f6d-184">クライアント側の検証は、ユーザーにとって非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-184">Client side validation is a great convenience for users.</span></span> <span data-ttu-id="40f6d-185">保存されますがそれ以外の場合に費やす時間のラウンド トリップを待機しているサーバー。</span><span class="sxs-lookup"><span data-stu-id="40f6d-185">It saves time they would otherwise spend waiting for a round trip to the server.</span></span> <span data-ttu-id="40f6d-186">ビジネス用語として、何百もの時間を乗算した値の秒の小数部をもいくつか追加最大多数の時間、費用やフラストレーションは解消します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-186">In business terms, even a few fractions of seconds multiplied hundreds of times each day adds up to be a lot of time, expense, and frustration.</span></span> <span data-ttu-id="40f6d-187">簡単かつ迅速検証より効率的に作業しより優れた品質の入力し、出力を生成することができます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-187">Straightforward and immediate validation enables users to work more efficiently and produce better quality input and output.</span></span>

<span data-ttu-id="40f6d-188">次のように動作するクライアント側の検証のために、適切な JavaScript のスクリプト参照では、ビューが必要です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-188">You must have a view with the proper JavaScript script references in place for client side validation to work as you see here.</span></span>

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

<span data-ttu-id="40f6d-189">[JQuery 控えめな検証](https://github.com/aspnet/jquery-validation-unobtrusive)スクリプトは、人気の高い上に構築されるカスタムの Microsoft フロント エンド ライブラリ[jQuery 検証](https://jqueryvalidation.org/)プラグインします。</span><span class="sxs-lookup"><span data-stu-id="40f6d-189">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="40f6d-190">2 つの場所に同じ検証ロジックをコードが jQuery 控えめな検証なし: サーバー側の検証属性、モデルのプロパティの中に 2 回、クライアント側スクリプトでもう (jQuery 検証のための例は、 [ `validate()`](https://jqueryvalidation.org/validate/)メソッドではどの程度複雑なこのなる可能性があります)。</span><span class="sxs-lookup"><span data-stu-id="40f6d-190">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server side validation attributes on model properties, and then again in client side scripts (the examples for jQuery Validate's [`validate()`](https://jqueryvalidation.org/validate/) method shows how complex this could become).</span></span> <span data-ttu-id="40f6d-191">代わりに、MVC の[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)と[HTML ヘルパー](xref:mvc/views/overview)検証属性を使用して HTML 5 を表示するためにモデルのプロパティのメタデータを入力できるようには[データ属性](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)で検証が必要な形式の要素。</span><span class="sxs-lookup"><span data-stu-id="40f6d-191">Instead, MVC's [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) are able to use the validation attributes and type metadata from model properties to render HTML 5 [data- attributes](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) in the form elements that need validation.</span></span> <span data-ttu-id="40f6d-192">MVC の生成、`data-`組み込みとカスタムの両方の属性の属性です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-192">MVC generates the `data-` attributes for both built-in and custom attributes.</span></span> <span data-ttu-id="40f6d-193">jQuery 控えめな検証がアプリケーションを解析し、`data-`属性し、jQuery、変数をクライアントにサーバー側の検証ロジックを効率的に「コピー」を検証するロジックを渡します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-193">jQuery Unobtrusive Validation then parses thes `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server side validation logic to the client.</span></span> <span data-ttu-id="40f6d-194">次に示すように、関連するタグ ヘルパーを使用して、クライアントで検証エラーを表示できます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-194">You can display validation errors on the client using the relevant tag helpers as shown here:</span></span>

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

<span data-ttu-id="40f6d-195">上記のタグ ヘルパーは、以下の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-195">The tag helpers above render the HTML below.</span></span> <span data-ttu-id="40f6d-196">注意して、 `data-` html 属性の出力の検証属性に対応している、`ReleaseDate`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="40f6d-196">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="40f6d-197">`data-val-required`下の属性には、リリース日 フィールドにユーザーを満たさない場合に表示するエラー メッセージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="40f6d-197">The `data-val-required` attribute below contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="40f6d-198">jQuery 検証の控えめな jQuery 検証をこの値を渡します[ `required()` ](https://jqueryvalidation.org/required-method/)付随するでそのメッセージを表示するメソッド **\<span >**要素。</span><span class="sxs-lookup"><span data-stu-id="40f6d-198">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

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

<span data-ttu-id="40f6d-199">クライアント側の検証では、形式が有効になるまで、送信ができなくなります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-199">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="40f6d-200">[送信] ボタンは、フォームを送信するか、エラー メッセージを表示する JavaScript を実行します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-200">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="40f6d-201">MVC は、.NET データ型に基づく可能性のあるオーバーライドを使用して、プロパティの型の属性値を決定`[DataType]`属性。</span><span class="sxs-lookup"><span data-stu-id="40f6d-201">MVC determines type attribute values based on the .NET data type of a property, possibly overridden using `[DataType]` attributes.</span></span> <span data-ttu-id="40f6d-202">基本`[DataType]`属性では、実際のサーバー側の検証は行われません。</span><span class="sxs-lookup"><span data-stu-id="40f6d-202">The base `[DataType]` attribute does no real server-side validation.</span></span> <span data-ttu-id="40f6d-203">ブラウザーでは、独自のエラー メッセージを選択し、jQuery 検証控えめなパッケージが、メッセージを上書きし、それらを他のユーザーと一貫して表示ただし、希望するが、それらのエラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-203">Browsers choose their own error messages and display those errors however they wish, however the jQuery Validation Unobtrusive package can override the messages and display them consistently with others.</span></span> <span data-ttu-id="40f6d-204">ユーザーを適用すると最も明らかにこのような`[DataType]`などサブクラス`[EmailAddress]`です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-204">This happens most obviously when users apply `[DataType]` subclasses such as `[EmailAddress]`.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="40f6d-205">動的なフォームに検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-205">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="40f6d-206">JQuery 控えめな検証に渡すための検証ロジックとパラメーター jQuery 検証ページが初めて読み込まれるときに、動的に生成されたフォームの検証が自動的に発生しません。</span><span class="sxs-lookup"><span data-stu-id="40f6d-206">Because jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads, dynamically generated forms will not automatically exhibit validation.</span></span> <span data-ttu-id="40f6d-207">代わりに、jQuery 作成直後に動的形式の解析を控えめな検証を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-207">Instead, you must tell jQuery Unobtrusive Validation to parse the dynamic form immediately after creating it.</span></span> <span data-ttu-id="40f6d-208">たとえば、次のコードは、AJAX を使用して追加、フォーム上のクライアント側の検証を設定する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="40f6d-208">For example, the code below shows how you might set up client side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
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

<span data-ttu-id="40f6d-209">`$.validator.unobtrusive.parse()`メソッドは 1 つの引数には、jQuery セレクターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-209">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="40f6d-210">このメソッドは、jQuery の解析を控えめな検証を指示、`data-`そのセレクター内のフォームの属性です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-210">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="40f6d-211">これらの属性の値は、フォーム、目的のクライアント側の検証規則の欠落を表すように jQuery 検証プラグインに渡されます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-211">The values of those attributes are then passed to the jQuery Validate plugin so that the form exhibits the desired client side validation rules.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="40f6d-212">検証のダイナミック コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-212">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="40f6d-213">などの個人コントロールときに、フォームに検証規則を更新することも`<input/>`s および`<select/>`s、動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-213">You can also update the validation rules on a form when individual controls, such as `<input/>`s and `<select/>`s, are dynamically generated.</span></span> <span data-ttu-id="40f6d-214">これらの要素のセレクターを渡すことはできません、`parse()`メソッドを直接周囲のフォームが既に解析されているし、更新されないためです。</span><span class="sxs-lookup"><span data-stu-id="40f6d-214">You cannot pass selectors for these elements to the `parse()` method directly because the surrounding form has already been parsed and will not update.</span></span> <span data-ttu-id="40f6d-215">代わりに、まず、既存の検証データを削除して次に示すように、フォーム全体は再解析。</span><span class="sxs-lookup"><span data-stu-id="40f6d-215">Instead, you first remove the existing validation data, then reparse the entire form, as shown below:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        form.removeData("validator")    // Added by the raw jQuery Validate
            .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a><span data-ttu-id="40f6d-216">IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="40f6d-216">IClientModelValidator</span></span>

<span data-ttu-id="40f6d-217">カスタム属性では、クライアント側のロジックを作成することがありますと[控えめな検証](http://jqueryvalidation.org/documentation/)に対するクライアントの検証の一部として自動的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-217">You may create client side logic for your custom attribute, and [unobtrusive validation](http://jqueryvalidation.org/documentation/) will execute it on the client for you automatically as part of validation.</span></span> <span data-ttu-id="40f6d-218">どのようなデータの属性を実装することによって追加を制御するには、まず、`IClientModelValidator`インターフェイスの次のようにします。</span><span class="sxs-lookup"><span data-stu-id="40f6d-218">The first step is to control what data- attributes are added by implementing the `IClientModelValidator` interface as shown here:</span></span>

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

<span data-ttu-id="40f6d-219">このインターフェイスを実装する属性は、生成されたフィールドに HTML 属性を追加できます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-219">Attributes that implement this interface can add HTML attributes to generated fields.</span></span> <span data-ttu-id="40f6d-220">出力を調べて、`ReleaseDate`要素は、ここでは、前の例では、次のような HTML を表示、`data-val-classicmovie`属性で定義されている、`AddValidation`メソッドの`IClientModelValidator`です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-220">Examining the output for the `ReleaseDate` element reveals HTML that is similar to the previous example, except now there is a `data-val-classicmovie` attribute that was defined in the `AddValidation` method of `IClientModelValidator`.</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

<span data-ttu-id="40f6d-221">控えめな検証でデータを使用して、`data-`エラー メッセージを表示する属性。</span><span class="sxs-lookup"><span data-stu-id="40f6d-221">Unobtrusive validation uses the data in the `data-` attributes to display error messages.</span></span> <span data-ttu-id="40f6d-222">ただし、jQuery のルールが把握して、jQuery を追加するまでメッセージ`validator`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="40f6d-222">However, jQuery doesn't know about rules or messages until you add them to jQuery's `validator` object.</span></span> <span data-ttu-id="40f6d-223">これは、という名前のメソッドを追加する次の例に示されて`classicmovie`jQuery にカスタムのクライアント検証コードを含む`validator`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="40f6d-223">This is shown in the example below that adds a method named `classicmovie` containing custom client validation code to the jQuery `validator` object.</span></span>

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

<span data-ttu-id="40f6d-224">JQuery なりました検証コードが false を返す場合に表示するエラー メッセージと同様に、カスタムの JavaScript の検証を実行するには、情報。</span><span class="sxs-lookup"><span data-stu-id="40f6d-224">Now jQuery has the information to execute the custom JavaScript validation as well as the error message to display if that validation code returns false.</span></span>

## <a name="remote-validation"></a><span data-ttu-id="40f6d-225">リモート検証</span><span class="sxs-lookup"><span data-stu-id="40f6d-225">Remote validation</span></span>

<span data-ttu-id="40f6d-226">リモート検証は、サーバー上のデータを使用してクライアント上のデータを検証する必要があるときに使用する優れた機能です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-226">Remote validation is a great feature to use when you need to validate data on the client against data on the server.</span></span> <span data-ttu-id="40f6d-227">たとえば、アプリは、電子メールまたはユーザー名は既に使用中でと、そのためにはデータの大量のクエリを実行する必要があるかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-227">For example, your app may need to verify whether an email or user name is already in use, and it must query a large amount of data to do so.</span></span> <span data-ttu-id="40f6d-228">ダウンロード大きなデータ セットのいずれかを検証するためや、いくつかのフィールドの多くのリソースを消費します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-228">Downloading large sets of data for validating one or a few fields consumes too many resources.</span></span> <span data-ttu-id="40f6d-229">機密情報を公開する場合もします。</span><span class="sxs-lookup"><span data-stu-id="40f6d-229">It may also expose sensitive information.</span></span> <span data-ttu-id="40f6d-230">代わりにラウンドト リップを要求すると、フィールドの検証を開始します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-230">An alternative is to make a round-trip request to validate a field.</span></span>

<span data-ttu-id="40f6d-231">2 つの手順では、リモート検証を実装できます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-231">You can implement remote validation in a two step process.</span></span> <span data-ttu-id="40f6d-232">最初を使用したモデルを付ける必要があります、`[Remote]`属性。</span><span class="sxs-lookup"><span data-stu-id="40f6d-232">First, you must annotate your model with the `[Remote]` attribute.</span></span> <span data-ttu-id="40f6d-233">`[Remote]`属性は複数のオーバー ロードを呼び出して、適切なコードをクライアント側 JavaScript を直接行うこともできますを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-233">The `[Remote]` attribute accepts multiple overloads you can use to direct client side JavaScript to the appropriate code to call.</span></span> <span data-ttu-id="40f6d-234">次の例が指す、`VerifyEmail`のアクション メソッド、`Users`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="40f6d-234">The example below points to the `VerifyEmail` action method of the `Users` controller.</span></span>

[!code-csharp[Main](validation/sample/User.cs?range=7-8)]

<span data-ttu-id="40f6d-235">定義されている、2 番目の手順が、対応するアクション メソッドに検証コードを配置する、`[Remote]`属性。</span><span class="sxs-lookup"><span data-stu-id="40f6d-235">The second step is putting the validation code in the corresponding action method as defined in the `[Remote]` attribute.</span></span> <span data-ttu-id="40f6d-236">JQuery 検証に従って[ `remote()` ](https://jqueryvalidation.org/remote-method/)メソッドのドキュメント。</span><span class="sxs-lookup"><span data-stu-id="40f6d-236">According to the jQuery Validate [`remote()`](https://jqueryvalidation.org/remote-method/) method documentation:</span></span>

> <span data-ttu-id="40f6d-237">Serverside 応答する必要がある JSON 文字列である必要があります`"true"`の有効な要素は、可能であり`"false"`、 `undefined`、または`null`無効な要素は、既定のエラー メッセージを使用します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-237">The serverside response must be a JSON string that must be `"true"` for valid elements, and can be `"false"`, `undefined`, or `null` for invalid elements, using the default error message.</span></span> <span data-ttu-id="40f6d-238">文字列の場合、serverside 応答、リフレッシュ レート。</span><span class="sxs-lookup"><span data-stu-id="40f6d-238">If the serverside response is a string, eg.</span></span> <span data-ttu-id="40f6d-239">`"That name is already taken, try peter123 instead"`、この文字列は、既定値の代わりにカスタム エラー メッセージとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-239">`"That name is already taken, try peter123 instead"`, this string will be displayed as a custom error message in place of the default.</span></span>

<span data-ttu-id="40f6d-240">定義、`VerifyEmail()`に従ってこれらの規則では、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="40f6d-240">The definition of the `VerifyEmail()` method follows these rules, as shown below.</span></span> <span data-ttu-id="40f6d-241">検証エラーを返すメッセージの電子メールが行われた場合、または`true`かどうか、電子メールには、無料で結果をラップ、`JsonResult`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="40f6d-241">It returns a validation error message if the email is taken, or `true` if the email is free, and wraps the result in a `JsonResult` object.</span></span> <span data-ttu-id="40f6d-242">クライアント側では、続行するか、必要な場合は、エラーを表示、返される値を使用できます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-242">The client side can then use the returned value to proceed or display the error if needed.</span></span>

[!code-csharp[Main](validation/sample/UsersController.cs?range=19-28)]

<span data-ttu-id="40f6d-243">ユーザーは、電子メール アドレスを入力、ときに、ビューでの JavaScript がについての電子メールが取得されている場合は、エラー メッセージが表示されますへのリモート呼び出しできます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-243">Now when users enter an email, JavaScript in the view makes a remote call to see if that email has been taken and, if so, displays the error message.</span></span> <span data-ttu-id="40f6d-244">それ以外の場合、ユーザーは通常どおり、フォームを送信できます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-244">Otherwise, the user can submit the form as usual.</span></span>

<span data-ttu-id="40f6d-245">`AdditionalFields`のプロパティ、`[Remote]`属性は、サーバー上のデータに対するフィールドの組み合わせを検証するために役立ちます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-245">The `AdditionalFields` property of the `[Remote]` attribute is useful for validating combinations of fields against data on the server.</span></span> <span data-ttu-id="40f6d-246">たとえば場合、`User`上記のモデルが 2 つの追加のプロパティと呼ばれる`FirstName`と`LastName`、ないことを確認の既存のユーザー既に名のペアリングすることができます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-246">For example, if the `User` model from above had two additional properties called `FirstName` and `LastName`, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="40f6d-247">次のコードに示すように、新しいプロパティを定義します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-247">You define the new properties as shown in the following code:</span></span>

[!code-csharp[Main](validation/sample/User.cs?range=10-13)]

<span data-ttu-id="40f6d-248">`AdditionalFields`でしたに設定されている明示的に文字列`"FirstName"`と`"LastName"`を使用して、 [ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof)リファクタリング後でこのような演算子が簡略化します。</span><span class="sxs-lookup"><span data-stu-id="40f6d-248">`AdditionalFields` could have been set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) operator like this simplifies later refactoring.</span></span> <span data-ttu-id="40f6d-249">検証を実行するアクション メソッドは、2 つの引数の値のいずれかを同意し、必要があります`FirstName`の値のいずれかと`LastName`です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-249">The action method to perform the validation must then accept two arguments, one for the value of `FirstName` and one for the value of `LastName`.</span></span>


[!code-csharp[Main](validation/sample/UsersController.cs?range=30-39)]

<span data-ttu-id="40f6d-250">ここでユーザー入力と姓と名前で、JavaScript:</span><span class="sxs-lookup"><span data-stu-id="40f6d-250">Now when users enter a first and last name, JavaScript:</span></span>

* <span data-ttu-id="40f6d-251">使うリモート呼び出しをそのペアの名前が取得されているかどうかを参照してください。</span><span class="sxs-lookup"><span data-stu-id="40f6d-251">Makes a remote call to see if that pair of names has been taken.</span></span>
* <span data-ttu-id="40f6d-252">ペアを取得した場合、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-252">If the pair has been taken, an error message is displayed.</span></span> 
* <span data-ttu-id="40f6d-253">ない場合は、ユーザーは、フォームを送信できます。</span><span class="sxs-lookup"><span data-stu-id="40f6d-253">If not taken, the user can submit the form.</span></span>

<span data-ttu-id="40f6d-254">2 つ以上のフィールドを検証する必要があるかどうか、`[Remote]`属性として指定することにコンマ区切りの一覧です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-254">If you need to validate two or more additional fields with the `[Remote]` attribute, you provide them as a comma-delimited list.</span></span> <span data-ttu-id="40f6d-255">例については、追加する、`MiddleName`プロパティ、モデルを設定、`[Remote]`属性の次のコードに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="40f6d-255">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following code:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="40f6d-256">`AdditionalFields`、すべての属性の引数と同様に、定数式がある必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-256">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="40f6d-257">そのため、使用しないでください、[補間文字列](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interpolated-strings)呼び出したり[ `string.Join()` ](https://msdn.microsoft.com/en-us/library/system.string.join(v=vs.110).aspx)初期化するために`AdditionalFields`です。</span><span class="sxs-lookup"><span data-stu-id="40f6d-257">Therefore, you must not use an [interpolated string](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interpolated-strings) or call [`string.Join()`](https://msdn.microsoft.com/en-us/library/system.string.join(v=vs.110).aspx) to initialize `AdditionalFields`.</span></span> <span data-ttu-id="40f6d-258">追加するその他のフィールドごとに、`[Remote]`属性に、対応するコント ローラー アクション メソッドに別の引数を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f6d-258">For every additional field that you add to the `[Remote]` attribute, you must add another argument to the corresponding controller action method.</span></span>
