---
title: "ASP.NET Core MVC でのモデルの検証"
author: rick-anderson
description: "ASP.NET Core MVC でのモデルの検証について説明します。"
keywords: "ASP.NET Core、MVC での検証"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3a8676dd-7ed8-4a05-bca2-44e288ab99ee
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: be130c24f5baf643a4c9493a33ec45bdd4cc66ed
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a><span data-ttu-id="71f47-104">ASP.NET Core MVC でのモデル検証の概要</span><span class="sxs-lookup"><span data-stu-id="71f47-104">Introduction to model validation in ASP.NET Core MVC</span></span>

<span data-ttu-id="71f47-105">によって[Rachel Appel](https://github.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="71f47-105">By [Rachel Appel](https://github.com/rachelappel)</span></span>

## <a name="introduction-to-model-validation"></a><span data-ttu-id="71f47-106">モデルの検証の概要</span><span class="sxs-lookup"><span data-stu-id="71f47-106">Introduction to model validation</span></span>

<span data-ttu-id="71f47-107">アプリは、データベースのデータを保存する前に、アプリは、データを検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="71f47-107">Before an app stores data in a database, the app must validate the data.</span></span> <span data-ttu-id="71f47-108">潜在的なセキュリティ上の脅威は、種類とサイズで適切に書式設定し、ルールに従ってくださいことを確認のためには、データをチェックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="71f47-108">Data must be checked for potential security threats, verified that it is appropriately formatted by type and size, and it must conform to your rules.</span></span> <span data-ttu-id="71f47-109">冗長と実装が煩雑でできますが、検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="71f47-109">Validation is necessary although it can be redundant and tedious to implement.</span></span> <span data-ttu-id="71f47-110">MVC では、検証は、クライアントとサーバーの両方で発生します。</span><span class="sxs-lookup"><span data-stu-id="71f47-110">In MVC, validation happens on both the client and server.</span></span>

<span data-ttu-id="71f47-111">さいわい、.NET には、検証属性に検証が抽象化します。</span><span class="sxs-lookup"><span data-stu-id="71f47-111">Fortunately, .NET has abstracted validation into validation attributes.</span></span> <span data-ttu-id="71f47-112">これらの属性には、検証コード記述するコードの量を減らすにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="71f47-112">These attributes contain validation code, thereby reducing the amount of code you must write.</span></span>

<span data-ttu-id="71f47-113">[GitHub からサンプルのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)です。</span><span class="sxs-lookup"><span data-stu-id="71f47-113">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).</span></span>

## <a name="validation-attributes"></a><span data-ttu-id="71f47-114">検証属性</span><span class="sxs-lookup"><span data-stu-id="71f47-114">Validation Attributes</span></span>

<span data-ttu-id="71f47-115">検証属性は、データベース テーブル内のフィールドの検証に概念的に似ていますので、モデルの検証を構成する方法です。</span><span class="sxs-lookup"><span data-stu-id="71f47-115">Validation attributes are a way to configure model validation so it's similar conceptually to validation on fields in database tables.</span></span> <span data-ttu-id="71f47-116">これには、データ型または必要なフィールドの割り当てなどの制約が含まれます。</span><span class="sxs-lookup"><span data-stu-id="71f47-116">This includes constraints such as assigning data types or required fields.</span></span> <span data-ttu-id="71f47-117">その他の種類検証にはでは、電子メール アドレスまたは電話番号、クレジット カードなど、ビジネス ルールを適用するデータにパターンを適用することが含まれます。</span><span class="sxs-lookup"><span data-stu-id="71f47-117">Other types of validation include applying patterns to data to enforce business rules, such as a credit card, phone number, or email address.</span></span> <span data-ttu-id="71f47-118">検証属性は、大幅に簡素化され、使いやすく、これらの要件を適用することを確認します。</span><span class="sxs-lookup"><span data-stu-id="71f47-118">Validation attributes make enforcing these requirements much simpler and easier to use.</span></span>

<span data-ttu-id="71f47-119">以下は、注釈付き`Movie`ムービーやテレビ番組に関する情報を格納するアプリケーションからモデル。</span><span class="sxs-lookup"><span data-stu-id="71f47-119">Below is an annotated `Movie` model from an app that stores information about movies and TV shows.</span></span> <span data-ttu-id="71f47-120">ほとんどのプロパティが必要ないくつかの文字列プロパティの長さの要件があります。</span><span class="sxs-lookup"><span data-stu-id="71f47-120">Most of the properties are required and several string properties have length requirements.</span></span> <span data-ttu-id="71f47-121">さらのために数値の範囲の制限がある、`Price`プロパティを 0 から $999.99、カスタム検証属性と共ににします。</span><span class="sxs-lookup"><span data-stu-id="71f47-121">Additionally, there is a numeric range restriction in place for the `Price` property from 0 to $999.99, along with a custom validation attribute.</span></span>

<span data-ttu-id="71f47-122">[!code-csharp[Main](validation/sample/Movie.cs?range=6-31)]</span><span class="sxs-lookup"><span data-stu-id="71f47-122">[!code-csharp[Main](validation/sample/Movie.cs?range=6-31)]</span></span>

<span data-ttu-id="71f47-123">モデル全体を読むだけで、コードを保守しやすいように、このアプリのデータについてのルールが表示されます。</span><span class="sxs-lookup"><span data-stu-id="71f47-123">Simply reading through the model reveals the rules about data for this app, making it easier to maintain the code.</span></span> <span data-ttu-id="71f47-124">いくつかの一般的な組み込みの検証属性を次に示します。</span><span class="sxs-lookup"><span data-stu-id="71f47-124">Below are several popular built-in validation attributes:</span></span>

* <span data-ttu-id="71f47-125">`[CreditCard]`: 検証プロパティは、クレジット カード形式です。</span><span class="sxs-lookup"><span data-stu-id="71f47-125">`[CreditCard]`: Validates the property has a credit card format.</span></span>

* <span data-ttu-id="71f47-126">`[Compare]`: モデルの検索一致で 2 つのプロパティを検証します。</span><span class="sxs-lookup"><span data-stu-id="71f47-126">`[Compare]`: Validates two properties in a model match.</span></span>

* <span data-ttu-id="71f47-127">`[EmailAddress]`: 検証電子メール形式を、プロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="71f47-127">`[EmailAddress]`: Validates the property has an email format.</span></span>

* <span data-ttu-id="71f47-128">`[Phone]`: 検証プロパティは、電話形式です。</span><span class="sxs-lookup"><span data-stu-id="71f47-128">`[Phone]`: Validates the property has a telephone format.</span></span>

* <span data-ttu-id="71f47-129">`[Range]`: 特定の範囲内でプロパティの値が検証されます。</span><span class="sxs-lookup"><span data-stu-id="71f47-129">`[Range]`: Validates the property value falls within the given range.</span></span>

* <span data-ttu-id="71f47-130">`[RegularExpression]`: データが指定された正規表現と一致することを検証します。</span><span class="sxs-lookup"><span data-stu-id="71f47-130">`[RegularExpression]`: Validates that the data matches the specified regular expression.</span></span>

* <span data-ttu-id="71f47-131">`[Required]`: 必要なプロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="71f47-131">`[Required]`: Makes a property required.</span></span>

* <span data-ttu-id="71f47-132">`[StringLength]`: 文字列プロパティに指定された最大長でが多くてことを検証します。</span><span class="sxs-lookup"><span data-stu-id="71f47-132">`[StringLength]`: Validates that a string property has at most the given maximum length.</span></span>

* <span data-ttu-id="71f47-133">`[Url]`: を検証、プロパティには、URL の形式です。</span><span class="sxs-lookup"><span data-stu-id="71f47-133">`[Url]`: Validates the property has a URL format.</span></span>

<span data-ttu-id="71f47-134">MVC から派生した任意の属性をサポートしている`ValidationAttribute`検証のためです。</span><span class="sxs-lookup"><span data-stu-id="71f47-134">MVC supports any attribute that derives from `ValidationAttribute` for validation purposes.</span></span> <span data-ttu-id="71f47-135">多くの便利な検証属性は含まれて、 [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations)名前空間。</span><span class="sxs-lookup"><span data-stu-id="71f47-135">Many useful validation attributes can be found in the [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) namespace.</span></span>

<span data-ttu-id="71f47-136">組み込みの属性が用意されてより多くの機能が必要になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="71f47-136">There may be instances where you need more features than built-in attributes provide.</span></span> <span data-ttu-id="71f47-137">派生することによってカスタム検証属性を作成する場合、それらの時間`ValidationAttribute`を実装する、モデルの変更または`IValidatableObject`です。</span><span class="sxs-lookup"><span data-stu-id="71f47-137">For those times, you can create custom validation attributes by deriving from `ValidationAttribute` or changing your model to implement `IValidatableObject`.</span></span>

## <a name="model-state"></a><span data-ttu-id="71f47-138">モデルの状態</span><span class="sxs-lookup"><span data-stu-id="71f47-138">Model State</span></span>

<span data-ttu-id="71f47-139">モデルの状態では、送信された HTML フォームの値で検証エラーを表します。</span><span class="sxs-lookup"><span data-stu-id="71f47-139">Model state represents validation errors in submitted HTML form values.</span></span>

<span data-ttu-id="71f47-140">MVC は引き続きに達するまでフィールドの検証エラー (既定で 200) の最大数。</span><span class="sxs-lookup"><span data-stu-id="71f47-140">MVC will continue validating fields until reaches the maximum number of errors (200 by default).</span></span> <span data-ttu-id="71f47-141">この数を構成するには次のコードを挿入することで、`ConfigureServices`メソッドで、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="71f47-141">You can configure this number by inserting the following code into the `ConfigureServices` method in the *Startup.cs* file:</span></span>

<span data-ttu-id="71f47-142">[!code-csharp[Main](validation/sample/Startup.cs?range=27)]</span><span class="sxs-lookup"><span data-stu-id="71f47-142">[!code-csharp[Main](validation/sample/Startup.cs?range=27)]</span></span>

## <a name="handling-model-state-errors"></a><span data-ttu-id="71f47-143">処理のモデル状態エラー</span><span class="sxs-lookup"><span data-stu-id="71f47-143">Handling Model State Errors</span></span>

<span data-ttu-id="71f47-144">モデルの検証が呼び出される、各コント ローラー アクションの前に発生しを検査するアクション メソッドの責任である`ModelState.IsValid`して適切に対処します。</span><span class="sxs-lookup"><span data-stu-id="71f47-144">Model validation occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="71f47-145">多くの場合は、適切な反応をなんらかの種類のモデルの検証が失敗した理由の詳細を示す理想的には、エラー応答を返します。</span><span class="sxs-lookup"><span data-stu-id="71f47-145">In many cases, the appropriate reaction is to return some kind of error response, ideally detailing the reason why model validation failed.</span></span>

<span data-ttu-id="71f47-146">ケース フィルターがこのようなポリシーを実装する適切な場所をする可能性があります、モデルの検証エラーを処理するための標準的な規則に準拠するアプリの一部を選択します。</span><span class="sxs-lookup"><span data-stu-id="71f47-146">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a filter may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="71f47-147">有効および無効なモデルの状態を持つユーザーの操作の動作をテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="71f47-147">You should test how your actions behave with valid and invalid model states.</span></span>

## <a name="manual-validation"></a><span data-ttu-id="71f47-148">手動検証</span><span class="sxs-lookup"><span data-stu-id="71f47-148">Manual validation</span></span>

<span data-ttu-id="71f47-149">モデル バインディングと検証の完了後にその一部を繰り返すこともできます。</span><span class="sxs-lookup"><span data-stu-id="71f47-149">After model binding and validation are complete, you may want to repeat parts of it.</span></span> <span data-ttu-id="71f47-150">たとえば、ユーザーが正しくありませんテキスト、整数を指定してください フィールドにまたはモデルのプロパティの値を計算する必要があります。</span><span class="sxs-lookup"><span data-stu-id="71f47-150">For example, a user may have entered text in a field expecting an integer, or you may need to compute a value for a model's property.</span></span>

<span data-ttu-id="71f47-151">検証を手動で実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="71f47-151">You may need to run validation manually.</span></span> <span data-ttu-id="71f47-152">これを行うには、呼び出し、`TryValidateModel`メソッドを次のように。</span><span class="sxs-lookup"><span data-stu-id="71f47-152">To do so, call the `TryValidateModel` method, as shown here:</span></span>

<span data-ttu-id="71f47-153">[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]</span><span class="sxs-lookup"><span data-stu-id="71f47-153">[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]</span></span>

## <a name="custom-validation"></a><span data-ttu-id="71f47-154">カスタム検証</span><span class="sxs-lookup"><span data-stu-id="71f47-154">Custom validation</span></span>

<span data-ttu-id="71f47-155">検証属性の機能のほとんどの検証要件に対応します。</span><span class="sxs-lookup"><span data-stu-id="71f47-155">Validation attributes work for most validation needs.</span></span> <span data-ttu-id="71f47-156">ただし、いくつかの検証規則とは違うだけのフィールドを確保するなどの汎用的なデータ検証が必要なまたは値の範囲に準拠するいると、ビジネスに固有です。</span><span class="sxs-lookup"><span data-stu-id="71f47-156">However, some validation rules are specific to your business, as they're not just generic data validation such as ensuring a field is required or that it conforms to a range of values.</span></span> <span data-ttu-id="71f47-157">これらのシナリオでは、カスタム検証属性は、優れたソリューションです。</span><span class="sxs-lookup"><span data-stu-id="71f47-157">For these scenarios, custom validation attributes are a great solution.</span></span> <span data-ttu-id="71f47-158">MVC での独自のカスタム検証属性の作成は簡単です。</span><span class="sxs-lookup"><span data-stu-id="71f47-158">Creating your own custom validation attributes in MVC is easy.</span></span> <span data-ttu-id="71f47-159">継承した、 `ValidationAttribute`、オーバーライド、`IsValid`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="71f47-159">Just inherit from the `ValidationAttribute`, and override the `IsValid` method.</span></span> <span data-ttu-id="71f47-160">`IsValid`メソッドは、次の 2 つのパラメーターを受け取り、1 つは、という名前のオブジェクトを*値*、2 番目の`ValidationContext`という名前のオブジェクト*validationContext*です。</span><span class="sxs-lookup"><span data-stu-id="71f47-160">The `IsValid` method accepts two parameters, the first is an object named *value* and the second is a `ValidationContext` object named *validationContext*.</span></span> <span data-ttu-id="71f47-161">*値*カスタム検証を検証するフィールドの実際の値を参照します。</span><span class="sxs-lookup"><span data-stu-id="71f47-161">*Value* refers to the actual value from the field that your custom validator is validating.</span></span>

<span data-ttu-id="71f47-162">次の例で、ビジネス ルールで制限ユーザーが設定されていないこと、ジャンル*クラシック*1960年以降後にリリースされたムービーにします。</span><span class="sxs-lookup"><span data-stu-id="71f47-162">In the following sample, a business rule states that users may not set the genre to *Classic* for a movie released after 1960.</span></span> <span data-ttu-id="71f47-163">`[ClassicMovie]`属性はまず、ジャンルをチェックし、従来型である場合、チェックをリリース日を 1960年よりも後であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="71f47-163">The `[ClassicMovie]` attribute checks the genre first, and if it is a classic, then it checks the release date to see that it is later than 1960.</span></span> <span data-ttu-id="71f47-164">これは、1960年後にリリースされた、検証が失敗します。</span><span class="sxs-lookup"><span data-stu-id="71f47-164">If it is released after 1960, validation fails.</span></span> <span data-ttu-id="71f47-165">この属性は、データの検証に使用できる暦を表す整数パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="71f47-165">The attribute accepts an integer parameter representing the year that you can use to validate data.</span></span> <span data-ttu-id="71f47-166">次のように、属性のコンス トラクターのパラメーターの値をキャプチャすることができます。</span><span class="sxs-lookup"><span data-stu-id="71f47-166">You can capture the value of the parameter in the attribute's constructor, as shown here:</span></span>

<span data-ttu-id="71f47-167">[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-28)]</span><span class="sxs-lookup"><span data-stu-id="71f47-167">[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-28)]</span></span>

<span data-ttu-id="71f47-168">`movie`上を表す変数、`Movie`を検証するフォームの送信からのデータを格納しているオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="71f47-168">The `movie` variable above represents a `Movie` object that contains the data from the form submission to validate.</span></span> <span data-ttu-id="71f47-169">検証コードが日とでジャンルをチェックするこの例では、`IsValid`のメソッド、`ClassicMovieAttribute`規則に従ってクラスです。</span><span class="sxs-lookup"><span data-stu-id="71f47-169">In this case, the validation code checks the date and genre in the `IsValid` method of the `ClassicMovieAttribute` class as per the rules.</span></span> <span data-ttu-id="71f47-170">検証成功時に`IsValid`を返します、`ValidationResult.Success`コード、および検証に失敗したときに、`ValidationResult`エラー メッセージを使用します。</span><span class="sxs-lookup"><span data-stu-id="71f47-170">Upon successful validation `IsValid` returns a `ValidationResult.Success` code, and when validation fails, a `ValidationResult` with an error message.</span></span> <span data-ttu-id="71f47-171">ユーザーが変更すると、`Genre`フィールドに、フォームを送信して、`IsValid`のメソッド、`ClassicMovieAttribute`ムービーが、従来型であるかどうかを確認してください。</span><span class="sxs-lookup"><span data-stu-id="71f47-171">When a user modifies the `Genre` field and submits the form, the `IsValid` method of the `ClassicMovieAttribute` will verify whether the movie is a classic.</span></span> <span data-ttu-id="71f47-172">すべての組み込みの属性と同様に適用、`ClassicMovieAttribute`などのプロパティに`ReleaseDate`前のコード例で示すように、検証が発生したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="71f47-172">Like any built-in attribute, apply the `ClassicMovieAttribute` to a property such as `ReleaseDate` to ensure validation happens, as shown in the previous code sample.</span></span> <span data-ttu-id="71f47-173">のみ行われますので`Movie`を使用する型の場合よりよい方法は`IValidatableObject`次の段落に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="71f47-173">Since the example works only with `Movie` types, a better option is to use `IValidatableObject` as shown in the following paragraph.</span></span>

<span data-ttu-id="71f47-174">また、この同じコードでしたに配置するモデルを実装して、`Validate`メソッドを`IValidatableObject`インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="71f47-174">Alternatively, this same code could be placed in the model by implementing the `Validate` method on the `IValidatableObject` interface.</span></span> <span data-ttu-id="71f47-175">実装するカスタム検証属性は、個々 のプロパティを検証するため、動作、`IValidatableObject`を次に示すようにクラス レベルの検証を実装するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="71f47-175">While custom validation attributes work well for validating individual properties, implementing `IValidatableObject` can be used to implement class-level validation as seen here.</span></span>

<span data-ttu-id="71f47-176">[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=33-41)]</span><span class="sxs-lookup"><span data-stu-id="71f47-176">[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=33-41)]</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="71f47-177">クライアント側の検証</span><span class="sxs-lookup"><span data-stu-id="71f47-177">Client side validation</span></span>

<span data-ttu-id="71f47-178">クライアント側の検証は、ユーザーにとって非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="71f47-178">Client side validation is a great convenience for users.</span></span> <span data-ttu-id="71f47-179">保存されますがそれ以外の場合に費やす時間のラウンド トリップを待機しているサーバー。</span><span class="sxs-lookup"><span data-stu-id="71f47-179">It saves time they would otherwise spend waiting for a round trip to the server.</span></span> <span data-ttu-id="71f47-180">ビジネス用語として、何百もの時間を乗算した値の秒の小数部をもいくつか追加最大多数の時間、費用やフラストレーションは解消します。</span><span class="sxs-lookup"><span data-stu-id="71f47-180">In business terms, even a few fractions of seconds multiplied hundreds of times each day adds up to be a lot of time, expense, and frustration.</span></span> <span data-ttu-id="71f47-181">簡単かつ迅速検証より効率的に作業しより優れた品質の入力し、出力を生成することができます。</span><span class="sxs-lookup"><span data-stu-id="71f47-181">Straightforward and immediate validation enables users to work more efficiently and produce better quality input and output.</span></span>

<span data-ttu-id="71f47-182">次のように動作するクライアント側の検証のために、適切な JavaScript のスクリプト参照では、ビューが必要です。</span><span class="sxs-lookup"><span data-stu-id="71f47-182">You must have a view with the proper JavaScript script references in place for client side validation to work as you see here.</span></span>

<span data-ttu-id="71f47-183">[!code-html[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]</span><span class="sxs-lookup"><span data-stu-id="71f47-183">[!code-html[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]</span></span>

<span data-ttu-id="71f47-184">[!code-html[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="71f47-184">[!code-html[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]</span></span>

<span data-ttu-id="71f47-185">MVC では、モデルのプロパティからの型のメタデータの他の検証属性を使用してデータを検証し、JavaScript を使用してエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="71f47-185">MVC uses validation attributes in addition to type metadata from model properties to validate data and display any error messages using JavaScript.</span></span> <span data-ttu-id="71f47-186">MVC を使用してモデルからのフォーム要素を表示するために使用すると[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)または[HTML ヘルパー](xref:mvc/views/overview)は HTML 5、追加[データ属性](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)として、検証を必要のあるフォーム要素で次に示します。</span><span class="sxs-lookup"><span data-stu-id="71f47-186">When you use MVC to render form elements from a model using [Tag Helpers](xref:mvc/views/tag-helpers/intro) or [HTML helpers](xref:mvc/views/overview) it will add HTML 5 [data- attributes](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) in the form elements that need validation, as shown below.</span></span> <span data-ttu-id="71f47-187">MVC の生成、`data-`組み込みとカスタムの両方の属性の属性です。</span><span class="sxs-lookup"><span data-stu-id="71f47-187">MVC generates the `data-` attributes for both built-in and custom attributes.</span></span> <span data-ttu-id="71f47-188">次に示すように、関連するタグ ヘルパーを使用して、クライアントで検証エラーを表示できます。</span><span class="sxs-lookup"><span data-stu-id="71f47-188">You can display validation errors on the client using the relevant tag helpers as shown here:</span></span>

<span data-ttu-id="71f47-189">[!code-html[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]</span><span class="sxs-lookup"><span data-stu-id="71f47-189">[!code-html[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]</span></span>

<span data-ttu-id="71f47-190">上記のタグ ヘルパーは、以下の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="71f47-190">The tag helpers above render the HTML below.</span></span> <span data-ttu-id="71f47-191">注意して、 `data-` html 属性の出力の検証属性に対応している、`ReleaseDate`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="71f47-191">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="71f47-192">`data-val-required`下の属性には、[リリース日] フィールドに、ユーザーを満たさないし、同時にそのメッセージが表示する場合に表示するエラー メッセージが含まれています。`<span>`要素。</span><span class="sxs-lookup"><span data-stu-id="71f47-192">The `data-val-required` attribute below contains an error message to display if the user doesn't fill in the release date field, and that message displays in the accompanying `<span>` element.</span></span>

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

<span data-ttu-id="71f47-193">クライアント側の検証では、形式が有効になるまで、送信ができなくなります。</span><span class="sxs-lookup"><span data-stu-id="71f47-193">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="71f47-194">[送信] ボタンは、フォームを送信するか、エラー メッセージを表示する JavaScript を実行します。</span><span class="sxs-lookup"><span data-stu-id="71f47-194">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="71f47-195">MVC は、.NET データ型に基づく可能性のあるオーバーライドを使用して、プロパティの型の属性値を決定`[DataType]`属性。</span><span class="sxs-lookup"><span data-stu-id="71f47-195">MVC determines type attribute values based on the .NET data type of a property, possibly overridden using `[DataType]` attributes.</span></span> <span data-ttu-id="71f47-196">基本`[DataType]`属性では、実際のサーバー側の検証は行われません。</span><span class="sxs-lookup"><span data-stu-id="71f47-196">The base `[DataType]` attribute does no real server-side validation.</span></span> <span data-ttu-id="71f47-197">ブラウザーでは、独自のエラー メッセージを選択し、jQuery 検証控えめなパッケージが、メッセージを上書きし、それらを他のユーザーと一貫して表示ただし、希望するが、それらのエラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="71f47-197">Browsers choose their own error messages and display those errors however they wish, however the jQuery Validation Unobtrusive package can override the messages and display them consistently with others.</span></span> <span data-ttu-id="71f47-198">ユーザーを適用すると最も明らかにこのような`[DataType]`などサブクラス`[EmailAddress]`です。</span><span class="sxs-lookup"><span data-stu-id="71f47-198">This happens most obviously when users apply `[DataType]` subclasses such as `[EmailAddress]`.</span></span>

## <a name="iclientmodelvalidator"></a><span data-ttu-id="71f47-199">IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="71f47-199">IClientModelValidator</span></span>

<span data-ttu-id="71f47-200">カスタム属性では、クライアント側のロジックを作成することがありますと[控えめな検証](http://jqueryvalidation.org/documentation/)に対するクライアントの検証の一部として自動的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="71f47-200">You may create client side logic for your custom attribute, and [unobtrusive validation](http://jqueryvalidation.org/documentation/) will execute it on the client for you automatically as part of validation.</span></span> <span data-ttu-id="71f47-201">どのようなデータの属性を実装することによって追加を制御するには、まず、`IClientModelValidator`インターフェイスの次のようにします。</span><span class="sxs-lookup"><span data-stu-id="71f47-201">The first step is to control what data- attributes are added by implementing the `IClientModelValidator` interface as shown here:</span></span>

<span data-ttu-id="71f47-202">[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]</span><span class="sxs-lookup"><span data-stu-id="71f47-202">[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]</span></span>

<span data-ttu-id="71f47-203">このインターフェイスを実装する属性は、生成されたフィールドに HTML 属性を追加できます。</span><span class="sxs-lookup"><span data-stu-id="71f47-203">Attributes that implement this interface can add HTML attributes to generated fields.</span></span> <span data-ttu-id="71f47-204">出力を調べて、`ReleaseDate`要素は、ここでは、前の例では、次のような HTML を表示、`data-val-classicmovie`属性で定義されている、`AddValidation`メソッドの`IClientModelValidator`です。</span><span class="sxs-lookup"><span data-stu-id="71f47-204">Examining the output for the `ReleaseDate` element reveals HTML that is similar to the previous example, except now there is a `data-val-classicmovie` attribute that was defined in the `AddValidation` method of `IClientModelValidator`.</span></span>

```html
<input class="form-control" type="datetime"
data-val="true"
data-val-classicmovie="Classic movies must have a release year earlier than 1960."
data-val-classicmovie-year="1960"
data-val-required="The ReleaseDate field is required."
id="ReleaseDate" name="ReleaseDate" value="" />
```

<span data-ttu-id="71f47-205">控えめな検証でデータを使用して、`data-`エラー メッセージを表示する属性。</span><span class="sxs-lookup"><span data-stu-id="71f47-205">Unobtrusive validation uses the data in the `data-` attributes to display error messages.</span></span> <span data-ttu-id="71f47-206">ただし、jQuery のルールが把握して、jQuery を追加するまでメッセージ`validator`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="71f47-206">However, jQuery doesn't know about rules or messages until you add them to jQuery's `validator` object.</span></span> <span data-ttu-id="71f47-207">これは、という名前のメソッドを追加する次の例に示されて`classicmovie`jQuery にカスタムのクライアント検証コードを含む`validator`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="71f47-207">This is shown in the example below that adds a method named `classicmovie` containing custom client validation code to the jQuery `validator` object.</span></span>

<span data-ttu-id="71f47-208">[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]</span><span class="sxs-lookup"><span data-stu-id="71f47-208">[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]</span></span>

<span data-ttu-id="71f47-209">JQuery なりました検証コードが false を返す場合に表示するエラー メッセージと同様に、カスタムの JavaScript の検証を実行するには、情報。</span><span class="sxs-lookup"><span data-stu-id="71f47-209">Now jQuery has the information to execute the custom JavaScript validation as well as the error message to display if that validation code returns false.</span></span>

## <a name="remote-validation"></a><span data-ttu-id="71f47-210">リモート検証</span><span class="sxs-lookup"><span data-stu-id="71f47-210">Remote validation</span></span>

<span data-ttu-id="71f47-211">リモート検証は、サーバー上のデータを使用してクライアント上のデータを検証する必要があるときに使用する優れた機能です。</span><span class="sxs-lookup"><span data-stu-id="71f47-211">Remote validation is a great feature to use when you need to validate data on the client against data on the server.</span></span> <span data-ttu-id="71f47-212">たとえば、アプリは、電子メールまたはユーザー名は既に使用中でと、そのためにはデータの大量のクエリを実行する必要があるかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="71f47-212">For example, your app may need to verify whether an email or user name is already in use, and it must query a large amount of data to do so.</span></span> <span data-ttu-id="71f47-213">ダウンロード大きなデータ セットのいずれかを検証するためや、いくつかのフィールドの多くのリソースを消費します。</span><span class="sxs-lookup"><span data-stu-id="71f47-213">Downloading large sets of data for validating one or a few fields consumes too many resources.</span></span> <span data-ttu-id="71f47-214">機密情報を公開する場合もします。</span><span class="sxs-lookup"><span data-stu-id="71f47-214">It may also expose sensitive information.</span></span> <span data-ttu-id="71f47-215">代わりにラウンドト リップを要求すると、フィールドの検証を開始します。</span><span class="sxs-lookup"><span data-stu-id="71f47-215">An alternative is to make a round-trip request to validate a field.</span></span>

<span data-ttu-id="71f47-216">2 つの手順では、リモート検証を実装できます。</span><span class="sxs-lookup"><span data-stu-id="71f47-216">You can implement remote validation in a two step process.</span></span> <span data-ttu-id="71f47-217">最初を使用したモデルを付ける必要があります、`[Remote]`属性。</span><span class="sxs-lookup"><span data-stu-id="71f47-217">First, you must annotate your model with the `[Remote]` attribute.</span></span> <span data-ttu-id="71f47-218">`[Remote]`属性は複数のオーバー ロードを呼び出して、適切なコードをクライアント側 JavaScript を直接行うこともできますを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="71f47-218">The `[Remote]` attribute accepts multiple overloads you can use to direct client side JavaScript to the appropriate code to call.</span></span> <span data-ttu-id="71f47-219">例が指す、`VerifyEmail`のアクション メソッド、`Users`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="71f47-219">The example points to the `VerifyEmail` action method of the `Users` controller.</span></span>

<span data-ttu-id="71f47-220">[!code-csharp[Main](validation/sample/User.cs?range=5-9)]</span><span class="sxs-lookup"><span data-stu-id="71f47-220">[!code-csharp[Main](validation/sample/User.cs?range=5-9)]</span></span>

<span data-ttu-id="71f47-221">定義されている、2 番目の手順が、対応するアクション メソッドに検証コードを配置する、`[Remote]`属性。</span><span class="sxs-lookup"><span data-stu-id="71f47-221">The second step is putting the validation code in the corresponding action method as defined in the `[Remote]` attribute.</span></span> <span data-ttu-id="71f47-222">返します、`JsonResult`クライアント側が続行または一時停止、および必要な場合にエラーが表示するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="71f47-222">It returns a `JsonResult` that the client side can use to proceed or pause and display an error if needed.</span></span>

<span data-ttu-id="71f47-223">[!code-none[Main](validation/sample/UsersController.cs?range=19-28)]</span><span class="sxs-lookup"><span data-stu-id="71f47-223">[!code-none[Main](validation/sample/UsersController.cs?range=19-28)]</span></span>

<span data-ttu-id="71f47-224">これで、ビューでの JavaScript が確認の電子メールを取得したかどうかにリモート呼び出しを行うユーザーは、電子メール アドレスを入力、ときに、その場合は、エラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="71f47-224">Now when users enter an email, JavaScript in the view makes a remote call to see if that email has been taken, and if so, then displays the error message.</span></span> <span data-ttu-id="71f47-225">それ以外の場合、ユーザーは通常どおり、フォームを送信できます。</span><span class="sxs-lookup"><span data-stu-id="71f47-225">Otherwise, the user can submit the form as usual.</span></span>
