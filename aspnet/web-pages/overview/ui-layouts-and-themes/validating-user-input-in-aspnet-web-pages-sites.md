---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: ページ (Razor) サイトの ASP.NET Web におけるユーザー入力の検証 |Microsoft ドキュメント
author: tfitzmac
description: ユーザーから取得した情報を検証する方法を説明&mdash;は、確認するユーザー入力を有効な html 形式で情報でのフォーム、名前を付けて.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 34f703e6db70ac79c22f4a50d4cfd4e2326b4c74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="af2b0-103">ASP.NET Web Pages (Razor) サイトにおけるユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="af2b0-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="af2b0-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="af2b0-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="af2b0-105">ユーザーから取得した情報を検証する方法を説明&mdash;は、確認するユーザー入力を有効な html 形式で情報フォームの ASP.NET Web Pages (Razor) サイトにします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="af2b0-106">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="af2b0-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="af2b0-107">ユーザーの入力になっていることを確認する方法では、定義されている検証条件と一致します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="af2b0-108">すべての検証テストが成功するかどうかを判断する方法。</span><span class="sxs-lookup"><span data-stu-id="af2b0-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="af2b0-109">検証エラーを表示する方法 (およびそれらの書式を設定する方法)。</span><span class="sxs-lookup"><span data-stu-id="af2b0-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="af2b0-110">ユーザーから直接取得しないデータを検証する方法。</span><span class="sxs-lookup"><span data-stu-id="af2b0-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="af2b0-111">これらは、ASP.NET のアーティクルで導入された概念をプログラミングします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="af2b0-112">`Validation`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="af2b0-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="af2b0-113">`Html.ValidationSummary`と`Html.ValidationMessage`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="af2b0-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="af2b0-114">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="af2b0-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="af2b0-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="af2b0-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="af2b0-116">このチュートリアルは、ASP.NET Web Pages 2 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="af2b0-117">この記事は、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="af2b0-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="af2b0-118">ユーザー入力の検証の概要</span><span class="sxs-lookup"><span data-stu-id="af2b0-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="af2b0-119">ユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="af2b0-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="af2b0-120">クライアント側の検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="af2b0-121">検証エラーの書式設定</span><span class="sxs-lookup"><span data-stu-id="af2b0-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="af2b0-122">ユーザーから直接取得しないデータの検証</span><span class="sxs-lookup"><span data-stu-id="af2b0-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="af2b0-123">ユーザー入力の検証の概要</span><span class="sxs-lookup"><span data-stu-id="af2b0-123">Overview of User Input Validation</span></span>

<span data-ttu-id="af2b0-124">ページで情報を入力するユーザーを依頼するかどうかは — たとえば、フォームに — が入力する値が有効であるかどうかを確認することが重要です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="af2b0-125">たとえば、重要な情報が欠落しているフォームを処理します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="af2b0-126">ユーザーは、HTML フォームに値を入力、入力値は文字列です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="af2b0-127">多くの場合、必要な値は整数や日付のような他のデータ型です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="af2b0-128">したがっても、ユーザーが入力した値を正しく、適切なデータ型に変換できるかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="af2b0-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="af2b0-129">値に特定の制限を設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="af2b0-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="af2b0-130">ユーザーは、整数値を正しく入力して、場合でもはたとえば、値が特定の範囲内にあるかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="af2b0-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![CSS スタイル クラスを使用する検証エラー](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="af2b0-132">**重要な**セキュリティの重要なユーザー入力の検証もします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="af2b0-133">ユーザーがフォームに入力できる値を制限すると、サイトのセキュリティを損なう可能性を値を入力できます誰か可能性を低くします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="af2b0-134">ユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="af2b0-134">Validating User Input</span></span>

<span data-ttu-id="af2b0-135">ASP.NET Web Pages 2 で使用できます、`Validator`ユーザー入力をテストするためのヘルパー。</span><span class="sxs-lookup"><span data-stu-id="af2b0-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="af2b0-136">基本的な方法は、次の操作です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="af2b0-137">どの入力を検証する要素 (フィールド) を決定します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="af2b0-138">通常の値を検証する`<input>`フォーム内の要素。</span><span class="sxs-lookup"><span data-stu-id="af2b0-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="af2b0-139">ただし、ことをお勧めも入力をすべての入力を検証するように制約付きの要素から取得した、 `<select>`  ボックスの一覧です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="af2b0-140">これにより、ユーザーがページ上のコントロールをバイパスおよびフォームを送信しないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="af2b0-141">ページのコードでのメソッドを使用して要素を入力ごとに個別の検証チェックを追加、`Validation`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="af2b0-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="af2b0-142">必須フィールドを確認するには、使用`Validation.RequireField(field, [error message])`(用、個々 のフィールド) または`Validation.RequireFields(field1, field2, ...))`(のフィールドの一覧)。</span><span class="sxs-lookup"><span data-stu-id="af2b0-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="af2b0-143">その他の種類の検証では、使用`Validation.Add(field, ValidationType)`です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="af2b0-144">`ValidationType`、これらのオプションを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="af2b0-144">For `ValidationType`, you can use these options:</span></span>

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
3. <span data-ttu-id="af2b0-145">チェックして、検証が渡されるかどうかを確認して、ページが送信されると、 `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="af2b0-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="af2b0-146">検証エラーがある場合は、通常のページの処理をスキップします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="af2b0-147">たとえば、ページの目的がデータベースを更新する場合は、しないをするまでを行うすべての検証エラーが修正されました。</span><span class="sxs-lookup"><span data-stu-id="af2b0-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="af2b0-148">検証エラーがある場合はエラーでメッセージを表示、ページのマークアップを使用して`Html.ValidationSummary`または`Html.ValidationMessage`、またはその両方です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="af2b0-149">次の例では、次の手順について説明するページを示します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="af2b0-150">検証の動作を確認するには、このページを実行し、意図的に間違いを犯さないです。</span><span class="sxs-lookup"><span data-stu-id="af2b0-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="af2b0-151">たとえば、ここでは、ページの外観を忘れた場合、コースの名前を入力する入力した場合、無効な日付を入力するとします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![表示されたページの検証エラー](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="af2b0-153">クライアント側の検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="af2b0-154">ユーザーがページを送信後に既定では、ユーザー入力の検証: サーバー コードで、検証の実行は、します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="af2b0-155">この方法の欠点は、あるユーザーかわからないページを送信した後までエラー活かせることです。</span><span class="sxs-lookup"><span data-stu-id="af2b0-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="af2b0-156">フォームは、long または複合型には、ページの送信後にのみ、エラーを報告できるかどうをユーザーに便利です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="af2b0-157">クライアント スクリプトで検証を実行するサポートを追加できます。</span><span class="sxs-lookup"><span data-stu-id="af2b0-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="af2b0-158">その場合は、検証は、ユーザーがブラウザーで作業を実行します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="af2b0-159">たとえば、値が整数を指定する必要がありますを指定するとします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="af2b0-160">整数以外の値を入力すると、ユーザーが、入力フィールドのままとすぐに、エラーが報告されます。</span><span class="sxs-lookup"><span data-stu-id="af2b0-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="af2b0-161">ユーザーは、それらに便利です、即時のフィードバックを取得します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="af2b0-162">クライアント ベースの検証は、複数のエラーを修正するフォームを送信するユーザーが持っている回数を減らすこともできます。</span><span class="sxs-lookup"><span data-stu-id="af2b0-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="af2b0-163">クライアント側の検証を使用する場合でも検証はサーバー コードでも、常に実行されます。</span><span class="sxs-lookup"><span data-stu-id="af2b0-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="af2b0-164">ユーザー クライアント ベースの検証をバイパスする場合に、サーバー コードの検証を実行するはセキュリティのメジャー、です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="af2b0-165">ページで、次の JavaScript ライブラリを登録します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="af2b0-166">ライブラリの 2 つのコンテンツ配信ネットワーク (CDN) から読み込み可能なため、コンピューターまたはサーバー上に必ずしも必要はありません。</span><span class="sxs-lookup"><span data-stu-id="af2b0-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="af2b0-167">ただしのローカル コピーが必要*jquery.validate.unobtrusive.js*です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="af2b0-168">既に動作していない WebMatrix テンプレートを使用する (と同様に**スターター サイト**) ライブラリが含まれている、基になっている Web Pages サイトを作成する**スターター サイト**です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="af2b0-169">コピーし、 *.js*ファイルを現在のサイトです。</span><span class="sxs-lookup"><span data-stu-id="af2b0-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="af2b0-170">マークアップでは、次の情報を検証している、各要素に対して呼び出しを追加して`Validation.For(field)`です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="af2b0-171">このメソッドは、クライアント側の検証で使用される属性を出力します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="af2b0-172">(実際の JavaScript コードの出力ではなく、メソッドがなどの属性を出力`data-val-...`です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="af2b0-173">これらの属性控えめなクライアントの検証をサポートを作業を行うには jQuery を使用します。)</span><span class="sxs-lookup"><span data-stu-id="af2b0-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="af2b0-174">次のページでは、前の例にクライアントの検証機能を追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="af2b0-175">すべての検証チェックがクライアント上で実行します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="af2b0-176">具体的には、データ型の検証 (整数、日付、およびなど) は、クライアントで実行しないでください。</span><span class="sxs-lookup"><span data-stu-id="af2b0-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="af2b0-177">次のチェックは、クライアントとサーバーの両方で動作します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="af2b0-178">この例では、有効な日付のテストは、クライアント コードで機能しません。</span><span class="sxs-lookup"><span data-stu-id="af2b0-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="af2b0-179">ただし、テストは、サーバー コードで実行されます。</span><span class="sxs-lookup"><span data-stu-id="af2b0-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="af2b0-180">検証エラーの書式設定</span><span class="sxs-lookup"><span data-stu-id="af2b0-180">Formatting Validation Errors</span></span>

<span data-ttu-id="af2b0-181">予約済みの名前は次の CSS クラスを定義することによって検証エラーを表示する方法を制御できます。</span><span class="sxs-lookup"><span data-stu-id="af2b0-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="af2b0-182">`field-validation-error`。</span><span class="sxs-lookup"><span data-stu-id="af2b0-182">`field-validation-error`.</span></span> <span data-ttu-id="af2b0-183">出力を定義、`Html.ValidationMessage`メソッドはエラーを表示するいるとします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="af2b0-184">`field-validation-valid`。</span><span class="sxs-lookup"><span data-stu-id="af2b0-184">`field-validation-valid`.</span></span> <span data-ttu-id="af2b0-185">出力を定義、`Html.ValidationMessage`メソッドはエラーがないとします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="af2b0-186">`input-validation-error`。</span><span class="sxs-lookup"><span data-stu-id="af2b0-186">`input-validation-error`.</span></span> <span data-ttu-id="af2b0-187">定義する方法`<input>`要素は、エラーがある場合にレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="af2b0-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="af2b0-188">(このクラスを使用して、背景色を設定するなど、&lt;入力&gt;要素の値が有効でない場合は異なる色にします)。この CSS クラスは、ASP.NET Web Pages 2) の「クライアントの検証中にのみ使用されます。</span><span class="sxs-lookup"><span data-stu-id="af2b0-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="af2b0-189">`input-validation-valid`。</span><span class="sxs-lookup"><span data-stu-id="af2b0-189">`input-validation-valid`.</span></span> <span data-ttu-id="af2b0-190">外観を定義`<input>`要素エラーがない場合。</span><span class="sxs-lookup"><span data-stu-id="af2b0-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="af2b0-191">`validation-summary-errors`。</span><span class="sxs-lookup"><span data-stu-id="af2b0-191">`validation-summary-errors`.</span></span> <span data-ttu-id="af2b0-192">出力を定義、`Html.ValidationSummary`メソッドがエラーの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="af2b0-193">`validation-summary-valid`。</span><span class="sxs-lookup"><span data-stu-id="af2b0-193">`validation-summary-valid`.</span></span> <span data-ttu-id="af2b0-194">出力を定義、`Html.ValidationSummary`メソッドはエラーがないとします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="af2b0-195">次`<style>`ブロックのエラー条件のルールを示しています。</span><span class="sxs-lookup"><span data-stu-id="af2b0-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="af2b0-196">記事の前半の例のページにこのスタイル ブロックを含めると、エラーの表示は次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="af2b0-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![CSS スタイル クラスを使用する検証エラー](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="af2b0-198">ASP.NET Web Pages 2 でないクライアントの検証を使用している場合、CSS クラス、`<input>`要素 (`input-validation-error`と`input-validation-valid`何も影響がありません。</span><span class="sxs-lookup"><span data-stu-id="af2b0-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="af2b0-199">静的および動的なエラーの表示</span><span class="sxs-lookup"><span data-stu-id="af2b0-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="af2b0-200">CSS 規則には、ペアなど`validation-summary-errors`と`validation-summary-valid`です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="af2b0-201">これらのペアを使用して、両方の条件の規則を定義できます: エラーが発生し、"normal"(エラーではない) 状態です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="af2b0-202">エラーがない場合でも、エラーの表示のマークアップが常に表示されることを理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="af2b0-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="af2b0-203">たとえば、ページがある場合、`Html.ValidationSummary`マークアップ内のメソッド、ページのソースには、次のマークアップ ページが最初に要求されたときにもします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="af2b0-204">言い換えると、`Html.ValidationSummary`メソッドは常に表示、`<div>`要素と一覧は、エラー リストが空の場合でもします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="af2b0-205">同様に、`Html.ValidationMessage`メソッドは常に表示、`<span>`要素を個々 のフィールド エラー、エラーが発生しなかった場合でものプレース ホルダーとして。</span><span class="sxs-lookup"><span data-stu-id="af2b0-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="af2b0-206">状況によっては、エラー メッセージを表示するページのフローとなる可能性が要素を移動するページにします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="af2b0-207">最後に CSS ルール`-valid`この問題を防ぐことができます、レイアウトを定義できます。</span><span class="sxs-lookup"><span data-stu-id="af2b0-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="af2b0-208">たとえば、定義`field-validation-error`と`field-validation-valid`固定するには両方が同じサイズです。</span><span class="sxs-lookup"><span data-stu-id="af2b0-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="af2b0-209">このように、フィールドの表示領域は静的であり、エラー メッセージが表示されている場合、ページのフローは変更されません。</span><span class="sxs-lookup"><span data-stu-id="af2b0-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="af2b0-210">ユーザーから直接取得しないデータの検証</span><span class="sxs-lookup"><span data-stu-id="af2b0-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="af2b0-211">HTML フォームから直接取得しない情報を検証がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="af2b0-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="af2b0-212">典型的な例では、次の例のように、クエリ文字列の値が渡される場所のページを示します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="af2b0-213">確認するこの例では、ページに渡される値 (ここでは、の値の 1022 `classid`) が有効です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="af2b0-214">直接使用することはできません、`Validation`この検証を実行するためのヘルパー。</span><span class="sxs-lookup"><span data-stu-id="af2b0-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="af2b0-215">ただし、検証エラー メッセージを表示する機能など、検証システムの他の機能を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="af2b0-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="af2b0-216">**重要な**から取得する値を常に検証*任意*フォーム フィールドの値、クエリ文字列値、および cookie の値を含むソース。</span><span class="sxs-lookup"><span data-stu-id="af2b0-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="af2b0-217">(おそらく悪意のある目的の場合) のこれらの値を変更するユーザーを簡単になります。</span><span class="sxs-lookup"><span data-stu-id="af2b0-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="af2b0-218">したがって、アプリケーションを保護するためにこれらの値を確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="af2b0-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="af2b0-219">次の例では、クエリ文字列で渡される値を検証する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="af2b0-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="af2b0-220">コードでは、値が空でないことと、整数であることをテストします。</span><span class="sxs-lookup"><span data-stu-id="af2b0-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="af2b0-221">要求がフォームの送信ではない場合に、テストが実行されるように注意してください (`if(!IsPost)`)。</span><span class="sxs-lookup"><span data-stu-id="af2b0-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="af2b0-222">このテストは、ページが要求されると、最初の時間を渡す場合しますが、いない場合、要求フォームの送信。</span><span class="sxs-lookup"><span data-stu-id="af2b0-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="af2b0-223">このエラーを表示することができますに追加するエラー検証エラーの一覧を呼び出して`Validation.AddFormError("message")`です。</span><span class="sxs-lookup"><span data-stu-id="af2b0-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="af2b0-224">ページへの呼び出しが含まれている場合、`Html.ValidationSummary`メソッド、エラーがユーザー入力の検証エラーと同じように、表示されます。</span><span class="sxs-lookup"><span data-stu-id="af2b0-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="af2b0-225">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="af2b0-225">Additional Resources</span></span>

[<span data-ttu-id="af2b0-226">ASP.NET Web Pages サイトでの HTML フォームの使用</span><span class="sxs-lookup"><span data-stu-id="af2b0-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
