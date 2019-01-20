---
title: ASP.NET Core MVC でのモデルの検証
author: tdykstra
description: ASP.NET Core MVC でのモデルの検証について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 01/14/2019
uid: mvc/models/validation
ms.openlocfilehash: 7c8255097dfc72480794930ebe4d6cb568edbd7c
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396195"
---
# <a name="model-validation-in-aspnet-core-mvc"></a><span data-ttu-id="7ae95-103">ASP.NET Core MVC でのモデルの検証</span><span class="sxs-lookup"><span data-stu-id="7ae95-103">Model validation in ASP.NET Core MVC</span></span>

<span data-ttu-id="7ae95-104">作成者: [Rachel Appel](https://github.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="7ae95-104">By [Rachel Appel](https://github.com/rachelappel)</span></span>

## <a name="introduction-to-model-validation"></a><span data-ttu-id="7ae95-105">モデル検証の概要</span><span class="sxs-lookup"><span data-stu-id="7ae95-105">Introduction to model validation</span></span>

<span data-ttu-id="7ae95-106">アプリでは、データベースにデータを格納する前に、データを検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-106">Before an app stores data in a database, the app must validate the data.</span></span> <span data-ttu-id="7ae95-107">データにセキュリティ上の脅威がないかどうかを確認し、種類とサイズが適切に設定されていることを検証しなければなりません。また、ご自身のルールに準拠している必要もあります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-107">Data must be checked for potential security threats, verified that it's appropriately formatted by type and size, and it must conform to your rules.</span></span> <span data-ttu-id="7ae95-108">検証を実装するのは冗長で面倒な場合がありますが、必要不可欠です。</span><span class="sxs-lookup"><span data-stu-id="7ae95-108">Validation is necessary although it can be redundant and tedious to implement.</span></span> <span data-ttu-id="7ae95-109">MVC では、検証はクライアントとサーバーの両方で発生します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-109">In MVC, validation happens on both the client and server.</span></span>

<span data-ttu-id="7ae95-110">さいわい、.NET では検証が検証属性に抽象化されています。</span><span class="sxs-lookup"><span data-stu-id="7ae95-110">Fortunately, .NET has abstracted validation into validation attributes.</span></span> <span data-ttu-id="7ae95-111">これらの属性には検証コードが含まれているため、開発者が記述しなければならないコードの量は少なくて済みます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-111">These attributes contain validation code, thereby reducing the amount of code you must write.</span></span>

<span data-ttu-id="7ae95-112">ASP.NET Core 2.2 以降の ASP.NET Core ランタイムでは、特定のモデル グラフが検証を必要としていないことを判断できる場合、検証がショートサーキット (スキップ) されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-112">In ASP.NET Core 2.2 and later, the ASP.NET Core runtime short-circuits (skips) validation if it can determine that a given model graph doesn't require validation.</span></span> <span data-ttu-id="7ae95-113">関連付けられた検証を持つことができない、または持っていないモデルを検証する場合、検証のスキップによりパフォーマンスを大幅に向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-113">Skipping validation can provide significant performance improvements when validating models that cannot or do not have any associated validators.</span></span> <span data-ttu-id="7ae95-114">スキップされた検証には、プリミティブ型のコレクション (`byte[]`、`string[]`、`Dictionary<string, string>` など) や、検証コントロールのない複雑なオブジェクト グラフなどのオブジェクトが含まれます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-114">The skipped validation includes objects such as collections of primitives (`byte[]`, `string[]`, `Dictionary<string, string>`, etc.), or complex object graphs without any validators.</span></span>

<span data-ttu-id="7ae95-115">[GitHub のサンプルを表示またはダウンロードしてください](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)。</span><span class="sxs-lookup"><span data-stu-id="7ae95-115">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).</span></span>

## <a name="validation-attributes"></a><span data-ttu-id="7ae95-116">検証属性</span><span class="sxs-lookup"><span data-stu-id="7ae95-116">Validation Attributes</span></span>

<span data-ttu-id="7ae95-117">検証属性はモデルの検証を構成する方法であり、概念的にはデータベース テーブルでのフィールドの検証に似ています。</span><span class="sxs-lookup"><span data-stu-id="7ae95-117">Validation attributes are a way to configure model validation so it's similar conceptually to validation on fields in database tables.</span></span> <span data-ttu-id="7ae95-118">これには、データ型の割り当てや必須フィールドなどの制約が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-118">This includes constraints such as assigning data types or required fields.</span></span> <span data-ttu-id="7ae95-119">その他の種類の検証としては、データへのパターンの適用があります。クレジット カード、電話番号、メール アドレスなどのビジネス ルールを強制するためのものです。</span><span class="sxs-lookup"><span data-stu-id="7ae95-119">Other types of validation include applying patterns to data to enforce business rules, such as a credit card, phone number, or email address.</span></span> <span data-ttu-id="7ae95-120">検証属性を使うと、これらの要件の適用がはるかに単純で簡単になります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-120">Validation attributes make enforcing these requirements much simpler and easier to use.</span></span>

<span data-ttu-id="7ae95-121">検証属性は、次のようにプロパティ レベルで指定されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-121">Validation attributes are specified at the property level:</span></span> 

```csharp
[Required]
public string MyProperty { get; set; }
```

<span data-ttu-id="7ae95-122">次に示すのは、映画やテレビ番組に関する情報を格納するアプリの注釈付き `Movie` モデルです。</span><span class="sxs-lookup"><span data-stu-id="7ae95-122">Below is an annotated `Movie` model from an app that stores information about movies and TV shows.</span></span> <span data-ttu-id="7ae95-123">ほとんどのプロパティは必須であり、一部の文字列プロパティには長さの要件があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-123">Most of the properties are required and several string properties have length requirements.</span></span> <span data-ttu-id="7ae95-124">さらに、`Price` プロパティには 0 から $999.99 までの数値範囲制限が、カスタム検証属性と共に適用されています。</span><span class="sxs-lookup"><span data-stu-id="7ae95-124">Additionally, there's a numeric range restriction in place for the `Price` property from 0 to $999.99, along with a custom validation attribute.</span></span>

[!code-csharp[](validation/sample/Movie.cs?range=6-29)]

<span data-ttu-id="7ae95-125">モデルに目を通すだけでこのアプリのデータについてのルールがわかり、コードの保守が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-125">Simply reading through the model reveals the rules about data for this app, making it easier to maintain the code.</span></span> <span data-ttu-id="7ae95-126">いくつかの一般的な組み込み検証属性を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-126">Below are several popular built-in validation attributes:</span></span>

* <span data-ttu-id="7ae95-127">`[CreditCard]`:プロパティがクレジット カード形式であることを検証します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-127">`[CreditCard]`: Validates the property has a credit card format.</span></span>

* <span data-ttu-id="7ae95-128">`[Compare]`:モデルの 2 つのプロパティが一致することを検証します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-128">`[Compare]`: Validates two properties in a model match.</span></span>

* <span data-ttu-id="7ae95-129">`[EmailAddress]`:プロパティが電子メール形式であることを検証します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-129">`[EmailAddress]`: Validates the property has an email format.</span></span>

* <span data-ttu-id="7ae95-130">`[Phone]`:プロパティが電話番号形式であることを検証します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-130">`[Phone]`: Validates the property has a telephone format.</span></span>

* <span data-ttu-id="7ae95-131">`[Range]`:プロパティの値が特定の範囲内であることを検証します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-131">`[Range]`: Validates the property value falls within the given range.</span></span>

* <span data-ttu-id="7ae95-132">`[RegularExpression]`:データが指定した正規表現と一致することを検証します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-132">`[RegularExpression]`: Validates that the data matches the specified regular expression.</span></span>

* <span data-ttu-id="7ae95-133">`[Required]`:プロパティを必須にします。</span><span class="sxs-lookup"><span data-stu-id="7ae95-133">`[Required]`: Makes a property required.</span></span>

* <span data-ttu-id="7ae95-134">`[StringLength]`:文字列プロパティが指定した最大長以下であることを検証します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-134">`[StringLength]`: Validates that a string property has at most the given maximum length.</span></span>

* <span data-ttu-id="7ae95-135">`[Url]`:プロパティが URL 形式であることを検証します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-135">`[Url]`: Validates the property has a URL format.</span></span>

<span data-ttu-id="7ae95-136">MVC は、検証のために `ValidationAttribute` から派生した任意の属性をサポートします。</span><span class="sxs-lookup"><span data-stu-id="7ae95-136">MVC supports any attribute that derives from `ValidationAttribute` for validation purposes.</span></span> <span data-ttu-id="7ae95-137">[System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) 名前空間には多くの便利な検証属性が含まれています。</span><span class="sxs-lookup"><span data-stu-id="7ae95-137">Many useful validation attributes can be found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace.</span></span>

<span data-ttu-id="7ae95-138">組み込みの属性で提供されるものより多くの機能が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-138">There may be instances where you need more features than built-in attributes provide.</span></span> <span data-ttu-id="7ae95-139">そのようなときは、`ValidationAttribute` から派生するか、モデルを変更して `IValidatableObject` を実装することにより、カスタム検証属性を作成できます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-139">For those times, you can create custom validation attributes by deriving from `ValidationAttribute` or changing your model to implement `IValidatableObject`.</span></span>

## <a name="notes-on-the-use-of-the-required-attribute"></a><span data-ttu-id="7ae95-140">Required 属性の使用に関する注意事項</span><span class="sxs-lookup"><span data-stu-id="7ae95-140">Notes on the use of the Required attribute</span></span>

<span data-ttu-id="7ae95-141">null 非許容の[値の型](/dotnet/csharp/language-reference/keywords/value-types) (`decimal`、`int`、`float`、`DateTime` など) は本質的に必須であり、`Required` 属性を必要としません。</span><span class="sxs-lookup"><span data-stu-id="7ae95-141">Non-nullable [value types](/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span> <span data-ttu-id="7ae95-142">アプリは、`Required` とマークされている null 非許容型に対して、サーバー側の検証チェックを実行しません。</span><span class="sxs-lookup"><span data-stu-id="7ae95-142">The app performs no server-side validation checks for non-nullable types that are marked `Required`.</span></span>

<span data-ttu-id="7ae95-143">MVC モデル バインドは、検証および検証属性には関わりがありませんが、null 非許容型に対して欠落値または空白文字を含むフォーム フィールドの送信を却下します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-143">MVC model binding, which isn't concerned with validation and validation attributes, rejects a form field submission containing a missing value or whitespace for a non-nullable type.</span></span> <span data-ttu-id="7ae95-144">ターゲット プロパティに `BindRequired` 属性がない場合、モデル バインドは null 非許容型の欠落データを無視し、そのフォーム フィールドは受信フォーム データからなくなります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-144">In the absence of a `BindRequired` attribute on the target property, model binding ignores missing data for non-nullable types, where the form field is absent from the incoming form data.</span></span>

<span data-ttu-id="7ae95-145">[BindRequired 属性](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (「<xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes>」も参照) は、フォーム データが完全であることを保証するのに便利です。</span><span class="sxs-lookup"><span data-stu-id="7ae95-145">The [BindRequired attribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (also see <xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes>) is useful to ensure form data is complete.</span></span> <span data-ttu-id="7ae95-146">プロパティに適用すると、モデル バインド システムはそのプロパティの値を必須にします。</span><span class="sxs-lookup"><span data-stu-id="7ae95-146">When applied to a property, the model binding system requires a value for that property.</span></span> <span data-ttu-id="7ae95-147">型に適用すると、モデル バインド システムはその型のすべてのプロパティに対して値を必須にします。</span><span class="sxs-lookup"><span data-stu-id="7ae95-147">When applied to a type, the model binding system requires values for all of the properties of that type.</span></span>

<span data-ttu-id="7ae95-148">[Nullable\<T> 型](/dotnet/csharp/programming-guide/nullable-types/) (例: `decimal?`、`System.Nullable<decimal>`) を使い、それを `Required` とマークすると、プロパティが標準の null 許容型 (例: `string`) の場合と同様に、サーバー側の検証チェックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-148">When you use a [Nullable\<T> type](/dotnet/csharp/programming-guide/nullable-types/) (for example, `decimal?` or `System.Nullable<decimal>`) and mark it `Required`, a server-side validation check is performed as if the property were a standard nullable type (for example, a `string`).</span></span>

<span data-ttu-id="7ae95-149">クライアント側検証では、`Required` とマークされているモデル プロパティに対応するフォーム フィールドの値、および `Required` とマークされていない null 非許容型のプロパティの値を必須にします。</span><span class="sxs-lookup"><span data-stu-id="7ae95-149">Client-side validation requires a value for a form field that corresponds to a model property that you've marked `Required` and for a non-nullable type property that you haven't marked `Required`.</span></span> <span data-ttu-id="7ae95-150">`Required` を使って、クライアント側検証のエラー メッセージを制御できます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-150">`Required` can be used to control the client-side validation error message.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="top-level-node-validation"></a><span data-ttu-id="7ae95-151">最上位ノードの検証</span><span class="sxs-lookup"><span data-stu-id="7ae95-151">Top-level node validation</span></span>

<span data-ttu-id="7ae95-152">最上位ノードには次が含まれています。</span><span class="sxs-lookup"><span data-stu-id="7ae95-152">Top-level nodes include:</span></span>

* <span data-ttu-id="7ae95-153">アクションのパラメーター</span><span class="sxs-lookup"><span data-stu-id="7ae95-153">Action parameters</span></span>
* <span data-ttu-id="7ae95-154">コントローラーのプロパティ</span><span class="sxs-lookup"><span data-stu-id="7ae95-154">Controller properties</span></span>
* <span data-ttu-id="7ae95-155">ページ ハンドラーのパラメーター</span><span class="sxs-lookup"><span data-stu-id="7ae95-155">Page handler parameters</span></span>
* <span data-ttu-id="7ae95-156">ページ モデルのプロパティ</span><span class="sxs-lookup"><span data-stu-id="7ae95-156">Page model properties</span></span>

<span data-ttu-id="7ae95-157">モデルのパラメーター検証に加え、モデルが関連付けられた最上位ノードが検証されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-157">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="7ae95-158">サンプル アプリからの次の例では、`VerifyPhone` メソッドでは <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> を使用し、フォームの Phone フィールドのユーザー データを検証します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-158">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the user data in the Phone field of a form:</span></span>

[!code-csharp[](validation/sample/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="7ae95-159">最上位ノードでは、検証属性と共に <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> を使用できます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-159">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="7ae95-160">サンプル アプリからの次の例では、`CheckAge` メソッドによって、フォームの送信時、クエリ文字列から `age` パラメーターを関連付ける必要があることが指定されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-160">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/sample/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="7ae95-161">[年齢確認] ページ (*CheckAge.cshtml*) には 2 つのフォームがあります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-161">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="7ae95-162">最初のフォームでは、`Age` の値 `99` がクエリ文字列 `https://localhost:5001/Users/CheckAge?Age=99` として送信されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-162">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="7ae95-163">クエリ文字列の正しく書式設定された `age` パラメーターが送信されると、フォームの有効性が確認されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-163">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="7ae95-164">[年齢確認] ページの 2 番目のフォームでは、要求本文で `Age` 値が送信され、検証は不合格となります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-164">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="7ae95-165">`age` パラメーターはクエリ文字列で渡される必要があるため、バインドが失敗します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-165">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="7ae95-166">検証は既定で有効になっており、<xref:Microsoft.AspNetCore.Mvc.MvcOptions> の <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> プロパティによって制御されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-166">Validation is enabled by default and controlled by the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property of <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span> <span data-ttu-id="7ae95-167">最上位ノードの検証を無効にするには、MVC オプション (`Startup.ConfigureServices`) で `AllowValidatingTopLevelNodes` を `false` に設定します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-167">To disable top-level node validation, set `AllowValidatingTopLevelNodes` to `false` in MVC options (`Startup.ConfigureServices`):</span></span>

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

::: moniker-end

## <a name="model-state"></a><span data-ttu-id="7ae95-168">モデルの状態</span><span class="sxs-lookup"><span data-stu-id="7ae95-168">Model State</span></span>

<span data-ttu-id="7ae95-169">モデルの状態は、送信された HTML フォーム値での検証エラーを表します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-169">Model state represents validation errors in submitted HTML form values.</span></span>

<span data-ttu-id="7ae95-170">MVC は、エラー数の最大数 (既定値は 200) に達するまで、フィールドの検証を続けます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-170">MVC will continue validating fields until it reaches the maximum number of errors (200 by default).</span></span> <span data-ttu-id="7ae95-171">この数は、`Startup.ConfigureServices` の次のコードを使用して構成します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-171">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors)]

## <a name="handle-model-state-errors"></a><span data-ttu-id="7ae95-172">モデルの状態エラーの処理</span><span class="sxs-lookup"><span data-stu-id="7ae95-172">Handle Model State errors</span></span>

<span data-ttu-id="7ae95-173">モデルの検証は、コントローラー アクションの実行前に発生します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-173">Model validation occurs before the execution of a controller action.</span></span> <span data-ttu-id="7ae95-174">このアクションは、`ModelState.IsValid` を検査し、適切な対処を行います。</span><span class="sxs-lookup"><span data-stu-id="7ae95-174">It's the action's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="7ae95-175">多くの場合において適切な対応は、エラー応答を返すことであり、モデルの検証が失敗した理由を詳しく示すのが理想的です。</span><span class="sxs-lookup"><span data-stu-id="7ae95-175">In many cases, the appropriate reaction is to return an error response, ideally detailing the reason why model validation failed.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7ae95-176">Web API コントローラーで `[ApiController]` 属性を使用して `ModelState.IsValid` が `false` と評価されると、問題の詳細を含む自動的な HTTP 400 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-176">When `ModelState.IsValid` evaluates to `false` in web API controllers using the `[ApiController]` attribute, an automatic HTTP 400 response containing issue details is returned.</span></span> <span data-ttu-id="7ae95-177">詳細については、「[自動的な HTTP 400 応答](xref:web-api/index#automatic-http-400-responses)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7ae95-177">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

::: moniker-end

<span data-ttu-id="7ae95-178">一部のアプリでは、モデル検証エラーを処理するとき、標準的な規則に従います。その場合、そのようなポリシーを実装する場所としてフィルターが適していることがあります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-178">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a filter may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="7ae95-179">モデルの状態が有効なとき、および無効なときにアクションがどのように動作するかテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-179">You should test how your actions behave with valid and invalid model states.</span></span>

## <a name="manual-validation"></a><span data-ttu-id="7ae95-180">手動検証</span><span class="sxs-lookup"><span data-stu-id="7ae95-180">Manual validation</span></span>

<span data-ttu-id="7ae95-181">モデル バインドと検証が完了した後、その一部を繰り返すことが必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-181">After model binding and validation are complete, you may want to repeat parts of it.</span></span> <span data-ttu-id="7ae95-182">たとえば、整数が想定されているフィールドにユーザーがテキストを入力したかもしれない場合や、モデルのプロパティの値を計算する必要がある場合などです。</span><span class="sxs-lookup"><span data-stu-id="7ae95-182">For example, a user may have entered text in a field expecting an integer, or you may need to compute a value for a model's property.</span></span>

<span data-ttu-id="7ae95-183">検証の手動実行が必要になることがあります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-183">You may need to run validation manually.</span></span> <span data-ttu-id="7ae95-184">その場合は、次に示すように `TryValidateModel` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-184">To do so, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/MoviesController.cs?name=snippet_TryValidateModel)]

## <a name="custom-validation"></a><span data-ttu-id="7ae95-185">カスタム検証</span><span class="sxs-lookup"><span data-stu-id="7ae95-185">Custom validation</span></span>

<span data-ttu-id="7ae95-186">検証属性の機能は、ほとんどの検証のニーズに対応します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-186">Validation attributes work for most validation needs.</span></span> <span data-ttu-id="7ae95-187">しかし、中にはご自身のビジネスに特化した検証ルールもあります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-187">However, some validation rules are specific to your business.</span></span> <span data-ttu-id="7ae95-188">そのルールは、必須フィールドや値範囲の確認などの一般的なデータ検証方法ではない場合があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-188">Your rules might not be common data validation techniques such as ensuring a field is required or that it conforms to a range of values.</span></span> <span data-ttu-id="7ae95-189">そのような場合は、カスタム検証属性が優れた解決策です。</span><span class="sxs-lookup"><span data-stu-id="7ae95-189">For these scenarios, custom validation attributes are a great solution.</span></span> <span data-ttu-id="7ae95-190">MVC では独自のカスタム検証属性を簡単に作成できます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-190">Creating your own custom validation attributes in MVC is easy.</span></span> <span data-ttu-id="7ae95-191">`ValidationAttribute` から継承し、`IsValid` メソッドをオーバーライドするだけです。</span><span class="sxs-lookup"><span data-stu-id="7ae95-191">Just inherit from the `ValidationAttribute`, and override the `IsValid` method.</span></span> <span data-ttu-id="7ae95-192">`IsValid` メソッドは 2 つのパラメーターを受け取ります。1 番目は *value* という名前のオブジェクトで、2 番目は *validationContext* という名前の `ValidationContext` オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="7ae95-192">The `IsValid` method accepts two parameters, the first is an object named *value* and the second is a `ValidationContext` object named *validationContext*.</span></span> <span data-ttu-id="7ae95-193">*Value* は、カスタム検証メソッドで検証するフィールドの実際の値を示します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-193">*Value* refers to the actual value from the field that your custom validator is validating.</span></span>

<span data-ttu-id="7ae95-194">次のサンプルのビジネス ルールでは、ユーザーは 1960 年より後にリリースされた映画のジャンルを *Classic* に設定できないことになっています。</span><span class="sxs-lookup"><span data-stu-id="7ae95-194">In the following sample, a business rule states that users may not set the genre to *Classic* for a movie released after 1960.</span></span> <span data-ttu-id="7ae95-195">`[ClassicMovie]` 属性は最初にジャンルをチェックし、それが Classic である場合、次にリリース日をチェックして、それが 1960 年以降であるかを確認します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-195">The `[ClassicMovie]` attribute checks the genre first, and if it's a classic, then it checks the release date to see that it's later than 1960.</span></span> <span data-ttu-id="7ae95-196">1960 年より後にリリースされている場合、検証は失敗します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-196">If it's released after 1960, validation fails.</span></span> <span data-ttu-id="7ae95-197">この属性には、データの検証に使うことができる、年を表す整数パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-197">The attribute accepts an integer parameter representing the year that you can use to validate data.</span></span> <span data-ttu-id="7ae95-198">次のように、属性のコンストラクターでパラメーターの値をキャプチャすることができます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-198">You can capture the value of the parameter in the attribute's constructor, as shown here:</span></span>

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="7ae95-199">上の `movie` 変数は、フォーム送信からのデータを検証のために格納している `Movie` オブジェクトを表します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-199">The `movie` variable above represents a `Movie` object that contains the data from the form submission to validate.</span></span> <span data-ttu-id="7ae95-200">この例では、検証コードはルールに従って `ClassicMovieAttribute` クラスの `IsValid` メソッドで日付とジャンルをチェックします。</span><span class="sxs-lookup"><span data-stu-id="7ae95-200">In this case, the validation code checks the date and genre in the `IsValid` method of the `ClassicMovieAttribute` class as per the rules.</span></span> <span data-ttu-id="7ae95-201">検証に成功すると、`IsValid` によって `ValidationResult.Success` コードが返されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-201">Upon successful validation ,`IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="7ae95-202">検証に失敗すると、`ValidationResult` が次のエラー メッセージとともに返されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-202">When validation fails, a `ValidationResult` with an error message is returned:</span></span>

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?name=snippet_GetErrorMessage)]

<span data-ttu-id="7ae95-203">ユーザーが `Genre` フィールドを変更してフォームを送信すると、`ClassicMovieAttribute` の `IsValid` メソッドは映画が Classic かどうかを検証します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-203">When a user modifies the `Genre` field and submits the form, the `IsValid` method of the `ClassicMovieAttribute` will verify whether the movie is a classic.</span></span> <span data-ttu-id="7ae95-204">他の組み込み属性と同じように、`ClassicMovieAttribute` を `ReleaseDate` などのプロパティに適用し、検証が確実に行われるようにします (前のコード例を参照)。</span><span class="sxs-lookup"><span data-stu-id="7ae95-204">Like any built-in attribute, apply the `ClassicMovieAttribute` to a property such as `ReleaseDate` to ensure validation happens, as shown in the previous code sample.</span></span> <span data-ttu-id="7ae95-205">この例は `Movie` 型でのみ機能するので、次の段落で示すように、`IValidatableObject` を使うのがさらによい方法です。</span><span class="sxs-lookup"><span data-stu-id="7ae95-205">Since the example works only with `Movie` types, a better option is to use `IValidatableObject` as shown in the following paragraph.</span></span>

<span data-ttu-id="7ae95-206">または、`IValidatableObject` インターフェイスに `Validate` メソッドを実装することで、これと同じコードをモデルに配置することもできます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-206">Alternatively, this same code could be placed in the model by implementing the `Validate` method on the `IValidatableObject` interface.</span></span> <span data-ttu-id="7ae95-207">カスタム検証属性は個別のプロパティを検証する場合に使えるのに対し、`IValidatableObject` の実装は、ここで示すようにクラス レベルの検証を実装するために使うことができます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-207">While custom validation attributes work well for validating individual properties, implementing `IValidatableObject` can be used to implement class-level validation as seen here.</span></span>

[!code-csharp[](validation/sample/MovieIValidatable.cs?name=snippet_Validate)]

## <a name="client-side-validation"></a><span data-ttu-id="7ae95-208">クライアント側の検証</span><span class="sxs-lookup"><span data-stu-id="7ae95-208">Client side validation</span></span>

<span data-ttu-id="7ae95-209">クライアント側の検証は、ユーザーにとって非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="7ae95-209">Client side validation is a great convenience for users.</span></span> <span data-ttu-id="7ae95-210">他の方法を使用した場合に必要な、サーバーへのラウンド トリップを待つ時間が節約できます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-210">It saves time they would otherwise spend waiting for a round trip to the server.</span></span> <span data-ttu-id="7ae95-211">ビジネス的に表現すると、1 回だけ見ればわずかな時間でも、それが毎日何百回も積み重なると、膨大な時間、費用、フラストレーションになります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-211">In business terms, even a few fractions of seconds multiplied hundreds of times each day adds up to be a lot of time, expense, and frustration.</span></span> <span data-ttu-id="7ae95-212">簡単かつ迅速な検証は、ユーザーの作業をいっそう効率的にし、入力と出力の品質が向上します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-212">Straightforward and immediate validation enables users to work more efficiently and produce better quality input and output.</span></span>

<span data-ttu-id="7ae95-213">クライアント側検証が動作するには、ここで示すように、適切な JavaScript スクリプト参照をビューに設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-213">You must have a view with the proper JavaScript script references in place for client-side validation to work as you see here.</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

<span data-ttu-id="7ae95-214">[jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) スクリプトは、人気のある [jQuery Validate](https://jqueryvalidation.org/) プラグインを基に作成された Microsoft のカスタム フロントエンド ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="7ae95-214">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="7ae95-215">jQuery Unobtrusive Validation を使わないと、同じ検証ロジックを 2 か所でコーディングする必要があります。1 つはモデル プロパティでのサーバー側検証属性で、もう 1 つはクライアント側スクリプトです (jQuery Validate の [`validate()`](https://jqueryvalidation.org/validate/) メソッドの例を見ると、これがどれほど複雑になるかがわかります)。</span><span class="sxs-lookup"><span data-stu-id="7ae95-215">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server side validation attributes on model properties, and then again in client side scripts (the examples for jQuery Validate's [`validate()`](https://jqueryvalidation.org/validate/) method shows how complex this could become).</span></span> <span data-ttu-id="7ae95-216">代わりに、MVC の[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)および [HTML ヘルパー](xref:mvc/views/overview)では、モデル プロパティの検証属性と型メタデータを使って、検証の必要なフォーム要素に HTML 5 の [data- 属性](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)をレンダリングできます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-216">Instead, MVC's [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) are able to use the validation attributes and type metadata from model properties to render HTML 5 [data- attributes](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) in the form elements that need validation.</span></span> <span data-ttu-id="7ae95-217">MVC は、組み込み属性とカスタム属性の両方に対して、`data-` 属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-217">MVC generates the `data-` attributes for both built-in and custom attributes.</span></span> <span data-ttu-id="7ae95-218">その後、jQuery Unobtrusive Validation は `data-` 属性を解析し、ロジックを jQuery Validate に渡して、クライアントにサーバー側検証ロジックを実質的に "コピー" します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-218">jQuery Unobtrusive Validation then parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server side validation logic to the client.</span></span> <span data-ttu-id="7ae95-219">次に示すように、関連するタグ ヘルパーを使って、クライアントで検証エラーを表示できます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-219">You can display validation errors on the client using the relevant tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="7ae95-220">上記のタグ ヘルパーは、以下の HTML をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="7ae95-220">The tag helpers above render the HTML below.</span></span> <span data-ttu-id="7ae95-221">HTML 出力の `data-` 属性が、`ReleaseDate` プロパティの検証属性に対応していることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="7ae95-221">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="7ae95-222">下の `data-val-required` 属性には、ユーザーがリリース日フィールドを入力していない場合に表示するエラー メッセージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="7ae95-222">The `data-val-required` attribute below contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="7ae95-223">jQuery Unobtrusive Validation はこの値を jQuery Validate の [`required()`](https://jqueryvalidation.org/required-method/) メソッドに渡し、このメソッドは付随する **\<span>** 要素にそのメッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-223">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

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

<span data-ttu-id="7ae95-224">クライアント側検証は、フォームが有効になるまで送信を許可しません。</span><span class="sxs-lookup"><span data-stu-id="7ae95-224">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="7ae95-225">送信ボタンをクリックすると、フォームの送信またはエラー メッセージの表示を行う JavaScript が実行されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-225">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="7ae95-226">MVC はプロパティの .NET データ型に基づいて型属性の値を特定しますが、場合によってはこの値は `[DataType]` 属性を使ってオーバーライドされています。</span><span class="sxs-lookup"><span data-stu-id="7ae95-226">MVC determines type attribute values based on the .NET data type of a property, possibly overridden using `[DataType]` attributes.</span></span> <span data-ttu-id="7ae95-227">基本 `[DataType]` 属性では、実際のサーバー側検証は行われません。</span><span class="sxs-lookup"><span data-stu-id="7ae95-227">The base `[DataType]` attribute does no real server-side validation.</span></span> <span data-ttu-id="7ae95-228">ブラウザーでは独自のエラー メッセージが選択され、自由にエラーが表示されますが、jQuery Validation Unobtrusive パッケージではメッセージをオーバーライドし、他と整合性のあるメッセージを表示できます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-228">Browsers choose their own error messages and display those errors as they wish, however the jQuery Validation Unobtrusive package can override the messages and display them consistently with others.</span></span> <span data-ttu-id="7ae95-229">これが最もよくわかるのは、ユーザーが `[EmailAddress]` などの `[DataType]` サブクラスを適用した場合です。</span><span class="sxs-lookup"><span data-stu-id="7ae95-229">This happens most obviously when users apply `[DataType]` subclasses such as `[EmailAddress]`.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="7ae95-230">動的なフォームに検証を追加する</span><span class="sxs-lookup"><span data-stu-id="7ae95-230">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="7ae95-231">jQuery Unobtrusive Validation はページが初めて読み込まれるときに検証ロジックとパラメーターを jQuery Validate に渡すので、動的に生成されるフォームでは、検証は自動的には行われません。</span><span class="sxs-lookup"><span data-stu-id="7ae95-231">Because jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads, dynamically generated forms won't automatically exhibit validation.</span></span> <span data-ttu-id="7ae95-232">代わりに、作成直後に動的フォームを解析するよう、jQuery Unobtrusive Validation に指示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-232">Instead, you must tell jQuery Unobtrusive Validation to parse the dynamic form immediately after creating it.</span></span> <span data-ttu-id="7ae95-233">たとえば、次のコードは、AJAX によって追加されるフォームでクライアント側検証を設定する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="7ae95-233">For example, the code below shows how you might set up client side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="7ae95-234">`$.validator.unobtrusive.parse()` メソッドには、その引数の 1 つで jQuery セレクターを指定します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-234">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="7ae95-235">このメソッドは、そのセレクター内のフォームの `data-` 属性を解析するよう jQuery Unobtrusive Validation に指示します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-235">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="7ae95-236">その後、これらの属性の値が jQuery Validate プラグインに渡され、必要なクライアント側検証ルールがフォームで示されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-236">The values of those attributes are then passed to the jQuery Validate plugin so that the form exhibits the desired client side validation rules.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="7ae95-237">動的なコントロールに検証を追加する</span><span class="sxs-lookup"><span data-stu-id="7ae95-237">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="7ae95-238">`<input/>` や `<select/>` などの個別のコントロールが動的に生成されるときに、フォームの検証ルールを更新することもできます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-238">You can also update the validation rules on a form when individual controls, such as `<input/>`s and `<select/>`s, are dynamically generated.</span></span> <span data-ttu-id="7ae95-239">これらの要素のセレクターを `parse()` メソッドに直接渡すことはできません。格納しているフォームが既に解析されていて更新されないためです。</span><span class="sxs-lookup"><span data-stu-id="7ae95-239">You cannot pass selectors for these elements to the `parse()` method directly because the surrounding form has already been parsed and won't update.</span></span> <span data-ttu-id="7ae95-240">代わりに、まず既存の検証データを削除した後、次に示すようにフォーム全体を再解析します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-240">Instead, you first remove the existing validation data, then reparse the entire form, as shown below:</span></span>

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

## <a name="iclientmodelvalidator"></a><span data-ttu-id="7ae95-241">IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="7ae95-241">IClientModelValidator</span></span>

<span data-ttu-id="7ae95-242">カスタム属性のクライアント側ロジックを作成することができ、[jquery 検証](http://jqueryvalidation.org/documentation/)へのアダプターを作成する [Unobtrusive Validation](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) はそれを検証の一部としてクライアントで自動的に実行します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-242">You may create client side logic for your custom attribute, and [unobtrusive validation](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) which creates an adapter to [jquery validation](http://jqueryvalidation.org/documentation/) will execute it on the client for you automatically as part of validation.</span></span> <span data-ttu-id="7ae95-243">最初に、次に示すように `IClientModelValidator` インターフェイスを実装することで、追加される data- 属性を制御します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-243">The first step is to control what data- attributes are added by implementing the `IClientModelValidator` interface as shown here:</span></span>

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?name=snippet_AddValidation)]

<span data-ttu-id="7ae95-244">このインターフェイスを実装する属性は、生成されるフィールドに HTML 属性を追加できます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-244">Attributes that implement this interface can add HTML attributes to generated fields.</span></span> <span data-ttu-id="7ae95-245">`ReleaseDate` 要素の出力を調べると、前の例と似た HTML であることがわかりますが、ここでは、`IClientModelValidator` の `AddValidation` メソッドで定義された `data-val-classicmovie` 属性があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-245">Examining the output for the `ReleaseDate` element reveals HTML that's similar to the previous example, except now there's a `data-val-classicmovie` attribute that was defined in the `AddValidation` method of `IClientModelValidator`.</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

<span data-ttu-id="7ae95-246">Unobtrusive Validation は、`data-` 属性のデータを使ってエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-246">Unobtrusive validation uses the data in the `data-` attributes to display error messages.</span></span> <span data-ttu-id="7ae95-247">ただし、jQuery が規則やメッセージを認識するのは、jQuery の `validator` オブジェクトにそれらが追加されてからです。</span><span class="sxs-lookup"><span data-stu-id="7ae95-247">However, jQuery doesn't know about rules or messages until you add them to jQuery's `validator` object.</span></span> <span data-ttu-id="7ae95-248">次の例でこれを示します。ここでは、カスタムの `classicmovie` クライアント検証メソッドが、jQuery `validator` オブジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-248">This is shown in the following example, which adds a custom `classicmovie` client validation method to the jQuery `validator` object.</span></span> <span data-ttu-id="7ae95-249">`unobtrusive.adapters.add` メソッドの説明については、[ASP.NET MVC での控え目なクライアント検証](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7ae95-249">For an explanation of the `unobtrusive.adapters.add` method, see [Unobtrusive Client Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html).</span></span>

[!code-javascript[](validation/sample/Views/Movies/Create.cshtml?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="7ae95-250">上のコードでは、`classicmovie` メソッドが映画のリリース日のクライアント側検証を実行しています。</span><span class="sxs-lookup"><span data-stu-id="7ae95-250">With the preceding code, the `classicmovie` method performs client-side validation on the movie release date.</span></span> <span data-ttu-id="7ae95-251">メソッドが `false` を返す場合、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-251">The error message displays if the method returns `false`.</span></span>

## <a name="remote-validation"></a><span data-ttu-id="7ae95-252">リモート検証</span><span class="sxs-lookup"><span data-stu-id="7ae95-252">Remote validation</span></span>

<span data-ttu-id="7ae95-253">リモート検証は、クライアント上のデータをサーバー上のデータに対して検証する必要があるときに使う優れた機能です。</span><span class="sxs-lookup"><span data-stu-id="7ae95-253">Remote validation is a great feature to use when you need to validate data on the client against data on the server.</span></span> <span data-ttu-id="7ae95-254">たとえば、アプリでメール アドレスやユーザー名が既に使われているかどうかを検証する必要があり、そのために大量のデータをクエリする必要があるような場合です。</span><span class="sxs-lookup"><span data-stu-id="7ae95-254">For example, your app may need to verify whether an email or user name is already in use, and it must query a large amount of data to do so.</span></span> <span data-ttu-id="7ae95-255">1 つまたは少数のフィールドを検証するために大きなデータ セットをダウンロードするのでは、消費するリソースが多すぎます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-255">Downloading large sets of data for validating one or a few fields consumes too many resources.</span></span> <span data-ttu-id="7ae95-256">機密情報が漏えいする可能性もあります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-256">It may also expose sensitive information.</span></span> <span data-ttu-id="7ae95-257">代わりの方法として、ラウンド トリップ要求を行って、フィールドを検証します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-257">An alternative is to make a round-trip request to validate a field.</span></span>

<span data-ttu-id="7ae95-258">リモート検証は、2 ステップのプロセスで実装できます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-258">You can implement remote validation in a two step process.</span></span> <span data-ttu-id="7ae95-259">最初に、`[Remote]` 属性でモデルに注釈を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-259">First, you must annotate your model with the `[Remote]` attribute.</span></span> <span data-ttu-id="7ae95-260">`[Remote]` 属性には複数のオーバーロードが指定できるので、それを使って、クライアント側の JavaScript が適切なコードを呼び出すようにします。</span><span class="sxs-lookup"><span data-stu-id="7ae95-260">The `[Remote]` attribute accepts multiple overloads you can use to direct client side JavaScript to the appropriate code to call.</span></span> <span data-ttu-id="7ae95-261">次の例では、`Users` コントローラーの `VerifyEmail` アクション メソッドを指し示しています。</span><span class="sxs-lookup"><span data-stu-id="7ae95-261">The example below points to the `VerifyEmail` action method of the `Users` controller.</span></span>

[!code-csharp[](validation/sample/User.cs?name=snippet_UserEmailProperty)]

<span data-ttu-id="7ae95-262">2 番目のステップでは、`[Remote]` 属性で定義されている対応するアクション メソッドに、検証コードを配置します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-262">The second step is putting the validation code in the corresponding action method as defined in the `[Remote]` attribute.</span></span> <span data-ttu-id="7ae95-263">Jquery Validate の[リモート](https://jqueryvalidation.org/remote-method/) メソッドに関するドキュメントによれば、サーバーの応答は次のいずれかの JSON 文字列である必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-263">According to the jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method documentation, the server response must be a JSON string that's either:</span></span>

* <span data-ttu-id="7ae95-264">有効な要素の場合は `"true"`。</span><span class="sxs-lookup"><span data-stu-id="7ae95-264">`"true"` for valid elements.</span></span>
* <span data-ttu-id="7ae95-265">無効な要素の場合は `"false"`、`undefined`、または `null` (既定のエラー メッセージを使用)。</span><span class="sxs-lookup"><span data-stu-id="7ae95-265">`"false"`, `undefined`, or `null` for invalid elements, using the default error message.</span></span>

<span data-ttu-id="7ae95-266">サーバーの応答が文字列 (例: `"That name is already taken, try peter123 instead"`) である場合、文字列は既定の文字列の代わりにカスタム エラー メッセージとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-266">If the server response is a string (for example, `"That name is already taken, try peter123 instead"`), the string is displayed as a custom error message in place of the default string.</span></span>

<span data-ttu-id="7ae95-267">次に示すように、`VerifyEmail` メソッドの定義はこれらの規則に従います。</span><span class="sxs-lookup"><span data-stu-id="7ae95-267">The definition of the `VerifyEmail` method follows these rules, as shown below.</span></span> <span data-ttu-id="7ae95-268">このメソッドは、メール アドレスが使われている場合は検証エラー メッセージを返し、メール アドレスが空いている場合は `true` を返します。また、結果を `JsonResult` オブジェクトにラップします。</span><span class="sxs-lookup"><span data-stu-id="7ae95-268">It returns a validation error message if the email is taken, or `true` if the email is free, and wraps the result in a `JsonResult` object.</span></span> <span data-ttu-id="7ae95-269">クライアント側では、返された値を使って、続行するか、必要な場合はエラーを表示できます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-269">The client side can then use the returned value to proceed or display the error if needed.</span></span>

[!code-csharp[](validation/sample/UsersController.cs?name=snippet_VerifyEmail)]

<span data-ttu-id="7ae95-270">ユーザーがメール アドレスを入力すると、ビューの JavaScript はリモート呼び出しを行って、そのメール アドレスが使われているかどうかを確認し、使われている場合はエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-270">Now when users enter an email, JavaScript in the view makes a remote call to see if that email has been taken and, if so, displays the error message.</span></span> <span data-ttu-id="7ae95-271">使われていない場合は、ユーザーは普通にフォームを送信できます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-271">Otherwise, the user can submit the form as usual.</span></span>

<span data-ttu-id="7ae95-272">`[Remote]` 属性の `AdditionalFields` プロパティは、フィールドの組み合わせをサーバー上のデータに対して検証するときに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-272">The `AdditionalFields` property of the `[Remote]` attribute is useful for validating combinations of fields against data on the server.</span></span> <span data-ttu-id="7ae95-273">たとえば、上の `User` モデルに `FirstName` および `LastName` という名前の 2 つの追加プロパティがある場合、その名前のペアを使う既存ユーザーがいないことを確認したい場合があるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="7ae95-273">For example, if the `User` model from above had two additional properties called `FirstName` and `LastName`, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="7ae95-274">次のコードで示すように、新しいプロパティを定義します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-274">You define the new properties as shown in the following code:</span></span>

[!code-csharp[](validation/sample/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="7ae95-275">`AdditionalFields` を文字列 `"FirstName"` および `"LastName"` に明示的に設定することもできますが、[`nameof`](/dotnet/csharp/language-reference/keywords/nameof) 演算子をこのように使うと、後のリファクタリングが容易になります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-275">`AdditionalFields` could've been set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator like this simplifies later refactoring.</span></span> <span data-ttu-id="7ae95-276">検証を実行するアクション メソッドには、`FirstName` の値と `LastName` の値のそれぞれに対応する 2 つの引数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-276">The action method to perform the validation must then accept two arguments, one for the value of `FirstName` and one for the value of `LastName`.</span></span>

[!code-csharp[](validation/sample/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="7ae95-277">ここで、ユーザーが姓と名を入力すると、JavaScript は以下のことを行います。</span><span class="sxs-lookup"><span data-stu-id="7ae95-277">Now when users enter a first and last name, JavaScript:</span></span>

* <span data-ttu-id="7ae95-278">リモート呼び出しを行って、その名前のペアが使われているかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-278">Makes a remote call to see if that pair of names has been taken.</span></span>
* <span data-ttu-id="7ae95-279">ペアが使われている場合は、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-279">If the pair has been taken, an error message is displayed.</span></span>
* <span data-ttu-id="7ae95-280">使われていない場合は、ユーザーはフォームを送信できます。</span><span class="sxs-lookup"><span data-stu-id="7ae95-280">If not taken, the user can submit the form.</span></span>

<span data-ttu-id="7ae95-281">複数の追加フィールドを `[Remote]` 属性で検証する必要がある場合は、コンマ区切りのリストとして提供します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-281">If you need to validate two or more additional fields with the `[Remote]` attribute, you provide them as a comma-delimited list.</span></span> <span data-ttu-id="7ae95-282">たとえば、`MiddleName` プロパティをモデルに追加するには、`[Remote]` 属性を次のコードのように設定します。</span><span class="sxs-lookup"><span data-stu-id="7ae95-282">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following code:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="7ae95-283">他の属性引数と同じように、`AdditionalFields` も定数式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-283">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="7ae95-284">したがって、[補間文字列](/dotnet/csharp/language-reference/keywords/interpolated-strings)を使ったり、<xref:System.String.Join*> を呼び出して `AdditionalFields` を初期化したりすることはできません。</span><span class="sxs-lookup"><span data-stu-id="7ae95-284">Therefore, you must not use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span> <span data-ttu-id="7ae95-285">`[Remote]` 属性に新しいフィールドを追加するごとに、対応するコントローラー アクション メソッドに別の引数を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ae95-285">For every additional field that you add to the `[Remote]` attribute, you must add another argument to the corresponding controller action method.</span></span>
