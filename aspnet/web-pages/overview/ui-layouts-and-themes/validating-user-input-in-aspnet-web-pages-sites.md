---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: ページ (Razor) サイトを ASP.NET Web でユーザー入力の検証 |Microsoft Docs
author: tfitzmac
description: この記事では、ユーザーから取得した情報を検証する方法をについて説明します&mdash;は、ユーザーが入力されることを確認して有効する HTML 内の情報でのフォーム、名前を付けて.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 2a35e9895c5b711d5c6c5544987f99fe7e2e0085
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368228"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="dceec-103">ASP.NET Web Pages (Razor) サイトでユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="dceec-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="dceec-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dceec-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="dceec-105">この記事では、ユーザーから取得した情報を検証する方法をについて説明します&mdash;は、ユーザーが入力されることを確認して有効する HTML 内の情報フォームの ASP.NET Web Pages (Razor) サイトでします。</span><span class="sxs-lookup"><span data-stu-id="dceec-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="dceec-106">学習内容。</span><span class="sxs-lookup"><span data-stu-id="dceec-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="dceec-107">ユーザーの入力になっていることを確認する方法では、定義した条件と一致します。</span><span class="sxs-lookup"><span data-stu-id="dceec-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="dceec-108">すべての検証テストに合格したかどうかを判断する方法。</span><span class="sxs-lookup"><span data-stu-id="dceec-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="dceec-109">検証エラーを表示する方法 (およびそれらの書式を設定する方法)。</span><span class="sxs-lookup"><span data-stu-id="dceec-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="dceec-110">ユーザーから直接提供しないデータを検証する方法。</span><span class="sxs-lookup"><span data-stu-id="dceec-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="dceec-111">これらは、プログラミングの概念は、情報の記事で導入された ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="dceec-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="dceec-112">`Validation`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="dceec-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="dceec-113">`Html.ValidationSummary`と`Html.ValidationMessage`メソッド。</span><span class="sxs-lookup"><span data-stu-id="dceec-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="dceec-114">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="dceec-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="dceec-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="dceec-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="dceec-116">このチュートリアルは、ASP.NET Web Pages 2 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="dceec-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="dceec-117">この記事は、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="dceec-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="dceec-118">ユーザー入力の検証の概要</span><span class="sxs-lookup"><span data-stu-id="dceec-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="dceec-119">ユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="dceec-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="dceec-120">クライアント側の検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="dceec-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="dceec-121">書式設定の検証エラー</span><span class="sxs-lookup"><span data-stu-id="dceec-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="dceec-122">ユーザーから直接提供しないデータの検証</span><span class="sxs-lookup"><span data-stu-id="dceec-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="dceec-123">ユーザー入力の検証の概要</span><span class="sxs-lookup"><span data-stu-id="dceec-123">Overview of User Input Validation</span></span>

<span data-ttu-id="dceec-124">ページの情報を入力するユーザーを依頼するかどうか: などをフォームに: が入力する値が有効であることを確認することが重要です。</span><span class="sxs-lookup"><span data-stu-id="dceec-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="dceec-125">たとえば、重要な情報が不足しているフォームを処理したくないです。</span><span class="sxs-lookup"><span data-stu-id="dceec-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="dceec-126">ユーザーは、HTML フォームに値を入力、入力値は文字列になります。</span><span class="sxs-lookup"><span data-stu-id="dceec-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="dceec-127">多くの場合、必要な値は整数や日付などの他のデータ型です。</span><span class="sxs-lookup"><span data-stu-id="dceec-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="dceec-128">そのため、また、ユーザーが入力した値を正しく、適切なデータ型に変換できるかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dceec-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="dceec-129">特定の制限は、値にもあります。</span><span class="sxs-lookup"><span data-stu-id="dceec-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="dceec-130">場合でも、ユーザーが正しく、整数の入力はたとえば、値が特定の範囲内にあるかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dceec-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![CSS スタイル クラスを使用する検証エラー](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="dceec-132">**重要な**セキュリティの重要なもユーザー入力を検証します。</span><span class="sxs-lookup"><span data-stu-id="dceec-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="dceec-133">ユーザーがフォームに入力できる値を制限する場合は、サイトのセキュリティを脅かす可能性のある値を入力だれかができます可能性を低くします。</span><span class="sxs-lookup"><span data-stu-id="dceec-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="dceec-134">ユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="dceec-134">Validating User Input</span></span>

<span data-ttu-id="dceec-135">ASP.NET Web ページ 2 では、使用することができます、`Validator`ユーザー入力をテストするためのヘルパー。</span><span class="sxs-lookup"><span data-stu-id="dceec-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="dceec-136">基本的な方法は、次の操作です。</span><span class="sxs-lookup"><span data-stu-id="dceec-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="dceec-137">検証する要素 (フィールド) を入力するかを決定します。</span><span class="sxs-lookup"><span data-stu-id="dceec-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="dceec-138">通常の値を検証する`<input>`フォーム内の要素。</span><span class="sxs-lookup"><span data-stu-id="dceec-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="dceec-139">ただし、制約付きの要素のように由来するすべての入力の検証は、入力もをお勧めは、`<select>`一覧。</span><span class="sxs-lookup"><span data-stu-id="dceec-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="dceec-140">これにより、ユーザーがページ上のコントロールをバイパスおよびフォームを送信しないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="dceec-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="dceec-141">ページのコードでのメソッドを使用して要素を入力ごとに個別の検証チェックを追加、`Validation`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="dceec-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="dceec-142">必須フィールドを確認するには、使用`Validation.RequireField(field, [error message])`(個々 のフィールド) に対するまたは`Validation.RequireFields(field1, field2, ...))`(のフィールドの一覧)。</span><span class="sxs-lookup"><span data-stu-id="dceec-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="dceec-143">その他の種類の検証では、使用`Validation.Add(field, ValidationType)`します。</span><span class="sxs-lookup"><span data-stu-id="dceec-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="dceec-144">`ValidationType`、これらのオプションを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="dceec-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. <span data-ttu-id="dceec-145">ページが送信されたときにチェックして、検証に合格するかどうかを確認`Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="dceec-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="dceec-146">検証エラーがある場合は、通常のページの処理をスキップします。</span><span class="sxs-lookup"><span data-stu-id="dceec-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="dceec-147">たとえば、ページの目的は、データベースを更新する場合に、しないするまで、すべての検証エラーが修正されました。</span><span class="sxs-lookup"><span data-stu-id="dceec-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="dceec-148">検証エラーがある場合はエラーでメッセージを表示、ページのマークアップを使用して`Html.ValidationSummary`または`Html.ValidationMessage`、またはその両方です。</span><span class="sxs-lookup"><span data-stu-id="dceec-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="dceec-149">次の例では、次の手順を説明するページを示します。</span><span class="sxs-lookup"><span data-stu-id="dceec-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="dceec-150">検証の動作を確認するには、このページを実行し、間違いを意図的にします。</span><span class="sxs-lookup"><span data-stu-id="dceec-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="dceec-151">たとえば、ここでは、ページがどのように入力すると、コース名を入力する忘れた場合、無効な日付を入力するとします。</span><span class="sxs-lookup"><span data-stu-id="dceec-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![レンダリングされたページの検証エラー](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="dceec-153">クライアント側の検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="dceec-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="dceec-154">ユーザーがページを送信した後、既定でユーザー入力の検証-は、検証は、サーバー コードで実行します。</span><span class="sxs-lookup"><span data-stu-id="dceec-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="dceec-155">このアプローチの欠点は、ユーザーがページを送信した後までエラーを作成したことを知ってしないいるです。</span><span class="sxs-lookup"><span data-stu-id="dceec-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="dceec-156">フォームが長であるか、または複雑な場合、ページが送信された後にのみ、エラーを報告できますをユーザーに便利です。</span><span class="sxs-lookup"><span data-stu-id="dceec-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="dceec-157">クライアント スクリプトで検証を実行するサポートを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="dceec-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="dceec-158">その場合は、検証は、ユーザーがブラウザーで作業を実行します。</span><span class="sxs-lookup"><span data-stu-id="dceec-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="dceec-159">たとえば、値が整数を指定する必要がありますを指定するとします。</span><span class="sxs-lookup"><span data-stu-id="dceec-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="dceec-160">整数以外の値を入力すると、ユーザーが、入力フィールドを離れると、すぐに、エラーが報告されます。</span><span class="sxs-lookup"><span data-stu-id="dceec-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="dceec-161">ユーザーにとって便利なは、即時のフィードバックを取得します。</span><span class="sxs-lookup"><span data-stu-id="dceec-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="dceec-162">クライアント ベースの検証をユーザーが持つ複数のエラーを修正するフォームを送信する時間の数を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="dceec-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="dceec-163">クライアント側の検証を使用する場合でも、検証はサーバー コードでも、常に実行されます。</span><span class="sxs-lookup"><span data-stu-id="dceec-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="dceec-164">ユーザーがクライアント ベースの検証をバイパスする場合に、セキュリティ対策はサーバー コードで検証を実行します。</span><span class="sxs-lookup"><span data-stu-id="dceec-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="dceec-165">ページで、次の JavaScript ライブラリを登録します。</span><span class="sxs-lookup"><span data-stu-id="dceec-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="dceec-166">ライブラリの 2 つの方法は、コンピューターまたはサーバー上にしなくても、コンテンツ配信ネットワーク (CDN) から読み込み可能なです。</span><span class="sxs-lookup"><span data-stu-id="dceec-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="dceec-167">ただしのローカル コピーが必要*jquery.validate.unobtrusive.js*します。</span><span class="sxs-lookup"><span data-stu-id="dceec-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="dceec-168">かどうかを既に使用していない WebMatrix テンプレート (など**スターター サイト**) ライブラリが含まれているに基づいている Web ページ サイトを作成する**スターター サイト**します。</span><span class="sxs-lookup"><span data-stu-id="dceec-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="dceec-169">コピーし、 *.js*ファイルを現在のサイト。</span><span class="sxs-lookup"><span data-stu-id="dceec-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="dceec-170">マークアップでは、次のように検証しているの各要素に対して呼び出しを追加`Validation.For(field)`します。</span><span class="sxs-lookup"><span data-stu-id="dceec-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="dceec-171">このメソッドは、クライアント側の検証で使用される属性を出力します。</span><span class="sxs-lookup"><span data-stu-id="dceec-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="dceec-172">(実際の JavaScript コードの出力ではなく、メソッドはなどの属性を出力`data-val-...`します。</span><span class="sxs-lookup"><span data-stu-id="dceec-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="dceec-173">これらの属性控えめなクライアントの検証をサポートする jQuery を使用して作業を行います。)</span><span class="sxs-lookup"><span data-stu-id="dceec-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="dceec-174">次のページでは、前の例にクライアントの検証機能を追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="dceec-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="dceec-175">すべての検証チェックがクライアントで実行します。</span><span class="sxs-lookup"><span data-stu-id="dceec-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="dceec-176">具体的には、データ型の検証 (整数、日付、およびなど) は、クライアントで実行しないでください。</span><span class="sxs-lookup"><span data-stu-id="dceec-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="dceec-177">次のチェックは、クライアントとサーバーの両方で機能します。</span><span class="sxs-lookup"><span data-stu-id="dceec-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="dceec-178">この例では有効な日付のテストは、クライアント コードで機能しません。</span><span class="sxs-lookup"><span data-stu-id="dceec-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="dceec-179">ただし、サーバー コードで、テストが実行されます。</span><span class="sxs-lookup"><span data-stu-id="dceec-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="dceec-180">書式設定の検証エラー</span><span class="sxs-lookup"><span data-stu-id="dceec-180">Formatting Validation Errors</span></span>

<span data-ttu-id="dceec-181">予約済みの名前は次の CSS クラスを定義することで検証エラーを表示する方法を制御できます。</span><span class="sxs-lookup"><span data-stu-id="dceec-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="dceec-182">`field-validation-error`。</span><span class="sxs-lookup"><span data-stu-id="dceec-182">`field-validation-error`.</span></span> <span data-ttu-id="dceec-183">出力を定義、`Html.ValidationMessage`メソッドのエラーを表示している場合。</span><span class="sxs-lookup"><span data-stu-id="dceec-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="dceec-184">`field-validation-valid`。</span><span class="sxs-lookup"><span data-stu-id="dceec-184">`field-validation-valid`.</span></span> <span data-ttu-id="dceec-185">出力を定義、`Html.ValidationMessage`メソッドのエラーがない場合。</span><span class="sxs-lookup"><span data-stu-id="dceec-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="dceec-186">`input-validation-error`。</span><span class="sxs-lookup"><span data-stu-id="dceec-186">`input-validation-error`.</span></span> <span data-ttu-id="dceec-187">定義する方法`<input>`エラーがある場合に要素が表示されます。</span><span class="sxs-lookup"><span data-stu-id="dceec-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="dceec-188">(の背景色を設定するこのクラスを使用するなど、&lt;入力&gt;要素を別の色を使用している場合、その値が無効です)。この CSS クラスは、(ASP.NET Web ページ 2) でクライアントの検証中にのみ使用されます。</span><span class="sxs-lookup"><span data-stu-id="dceec-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="dceec-189">`input-validation-valid`。</span><span class="sxs-lookup"><span data-stu-id="dceec-189">`input-validation-valid`.</span></span> <span data-ttu-id="dceec-190">外観を定義`<input>`要素エラーがない場合。</span><span class="sxs-lookup"><span data-stu-id="dceec-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="dceec-191">`validation-summary-errors`。</span><span class="sxs-lookup"><span data-stu-id="dceec-191">`validation-summary-errors`.</span></span> <span data-ttu-id="dceec-192">出力を定義、`Html.ValidationSummary`メソッドがエラーの一覧を表示することができます。</span><span class="sxs-lookup"><span data-stu-id="dceec-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="dceec-193">`validation-summary-valid`。</span><span class="sxs-lookup"><span data-stu-id="dceec-193">`validation-summary-valid`.</span></span> <span data-ttu-id="dceec-194">出力を定義、`Html.ValidationSummary`メソッドのエラーがない場合。</span><span class="sxs-lookup"><span data-stu-id="dceec-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="dceec-195">次`<style>`ブロックには、エラー条件のルールが表示されます。</span><span class="sxs-lookup"><span data-stu-id="dceec-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="dceec-196">このスタイルのブロックを含めるには、記事の前半で例のページで、エラーの表示は次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="dceec-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![CSS スタイル クラスを使用する検証エラー](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="dceec-198">に対して、CSS クラスを ASP.NET Web ページ 2 で、クライアントの検証を使用していない場合、`<input>`要素 (`input-validation-error`と`input-validation-valid`影響はありません。</span><span class="sxs-lookup"><span data-stu-id="dceec-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="dceec-199">静的および動的なエラーの表示</span><span class="sxs-lookup"><span data-stu-id="dceec-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="dceec-200">CSS 規則などはペアになって`validation-summary-errors`と`validation-summary-valid`します。</span><span class="sxs-lookup"><span data-stu-id="dceec-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="dceec-201">これらのペアを使用して、両方の条件の規則を定義できます。 エラー状態と、"normal"(エラーではない) 条件。</span><span class="sxs-lookup"><span data-stu-id="dceec-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="dceec-202">エラーがない場合でも、エラーの表示のマークアップが常に表示されることを理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="dceec-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="dceec-203">たとえば、次のページがある、`Html.ValidationSummary`メソッドのマークアップで、ページのソースには、次のマークアップ、ページが初めて要求された場合でも。</span><span class="sxs-lookup"><span data-stu-id="dceec-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="dceec-204">つまり、`Html.ValidationSummary`メソッドは常に表示、`<div>`要素と一覧は、エラーの一覧が空の場合でも。</span><span class="sxs-lookup"><span data-stu-id="dceec-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="dceec-205">同様に、`Html.ValidationMessage`メソッドは常に表示、`<span>`要素エラーがない場合でも、個々 のフィールド エラーのプレース ホルダーとして。</span><span class="sxs-lookup"><span data-stu-id="dceec-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="dceec-206">場合によっては、エラー メッセージを表示するページのリフローして内を移動するページの要素が発生することができます。</span><span class="sxs-lookup"><span data-stu-id="dceec-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="dceec-207">CSS 規則で終わる`-valid`この問題を回避するのに役立つレイアウトを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="dceec-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="dceec-208">たとえば、定義`field-validation-error`と`field-validation-valid`両方に同じ固定サイズです。</span><span class="sxs-lookup"><span data-stu-id="dceec-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="dceec-209">これにより、フィールドの表示領域は静的であり、エラー メッセージが表示されている場合、ページ フローは変更されません。</span><span class="sxs-lookup"><span data-stu-id="dceec-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="dceec-210">ユーザーから直接提供しないデータの検証</span><span class="sxs-lookup"><span data-stu-id="dceec-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="dceec-211">HTML フォームから直接取得しない情報を検証するがある場合があります。</span><span class="sxs-lookup"><span data-stu-id="dceec-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="dceec-212">典型的な例では、値が次の例のように、クエリ文字列で渡されるページを示します。</span><span class="sxs-lookup"><span data-stu-id="dceec-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="dceec-213">確認するこの例では、ページに渡される値 (ここでは、の値の 1022 `classid`) は有効です。</span><span class="sxs-lookup"><span data-stu-id="dceec-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="dceec-214">直接使用することはできません、`Validation`この検証を実行するためのヘルパー。</span><span class="sxs-lookup"><span data-stu-id="dceec-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="dceec-215">ただし、検証エラー メッセージを表示する機能など、検証システムの他の機能を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="dceec-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="dceec-216">**重要な**から取得した値を常に検証*任意*フォーム フィールドの値、クエリ文字列値、cookie の値を含むソース。</span><span class="sxs-lookup"><span data-stu-id="dceec-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="dceec-217">(おそらくの悪意のある目的で) これらの値を変更するユーザーを簡単になります。</span><span class="sxs-lookup"><span data-stu-id="dceec-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="dceec-218">したがって、アプリケーションを保護するためにこれらの値を確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dceec-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="dceec-219">次の例では、クエリ文字列で渡される値を検証する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="dceec-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="dceec-220">コードでは、値が空でないことと、これは、整数であることをテストします。</span><span class="sxs-lookup"><span data-stu-id="dceec-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="dceec-221">要求はフォームの送信時に、テストの実行に注意してください (`if(!IsPost)`)。</span><span class="sxs-lookup"><span data-stu-id="dceec-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="dceec-222">このテストは、ページが要求されると、初めてを渡す場合しますが、ない場合、要求フォームの送信です。</span><span class="sxs-lookup"><span data-stu-id="dceec-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="dceec-223">このエラーを表示することができますを追加するエラーの検証エラーの一覧に`Validation.AddFormError("message")`します。</span><span class="sxs-lookup"><span data-stu-id="dceec-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="dceec-224">ページへの呼び出しが含まれている場合、`Html.ValidationSummary`メソッド、エラーがユーザー入力の検証エラーと同じように、表示されます。</span><span class="sxs-lookup"><span data-stu-id="dceec-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="dceec-225">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="dceec-225">Additional Resources</span></span>

[<span data-ttu-id="dceec-226">ASP.NET Web Pages サイトでの HTML フォームの操作</span><span class="sxs-lookup"><span data-stu-id="dceec-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
