---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: '繰り返し #3 – フォーム検証を追加する (c#) |Microsoft Docs'
author: microsoft
description: 3 番目のイテレーションでは、基本的なフォーム検証を追加します。 ユーザーは、必要なフォームのフィールドを完了しなくても、フォームを送信できないようにようにします。 私たちも emai を検証しています.
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 532a5849007a5e8301da7c2801b553b9148679c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828148"
---
<a name="iteration-3--add-form-validation-c"></a><span data-ttu-id="78b1f-105">繰り返し #3 – フォーム検証を追加する (c#)</span><span class="sxs-lookup"><span data-stu-id="78b1f-105">Iteration #3 – Add form validation (C#)</span></span>
====================
<span data-ttu-id="78b1f-106">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="78b1f-106">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="78b1f-107">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="78b1f-107">Download Code</span></span>](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> <span data-ttu-id="78b1f-108">3 番目のイテレーションでは、基本的なフォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-108">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="78b1f-109">ユーザーは、必要なフォームのフィールドを完了しなくても、フォームを送信できないようにようにします。</span><span class="sxs-lookup"><span data-stu-id="78b1f-109">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="78b1f-110">また、電子メール アドレスと電話番号を検証しました。</span><span class="sxs-lookup"><span data-stu-id="78b1f-110">We also validate email addresses and phone numbers.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a><span data-ttu-id="78b1f-111">連絡先管理 ASP.NET MVC アプリケーション (c#) の構築</span><span class="sxs-lookup"><span data-stu-id="78b1f-111">Building a Contact Management ASP.NET MVC Application (C#)</span></span>
  

<span data-ttu-id="78b1f-112">このチュートリアル シリーズでは、開始から終了に全体連絡先管理アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-112">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="78b1f-113">Contact Manager アプリケーションは、ユーザーの一覧については店舗連絡先情報の名前、電話番号、電子メール アドレスにするようにことができます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-113">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="78b1f-114">複数のイテレーションにおける、アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-114">We build the application over multiple iterations.</span></span> <span data-ttu-id="78b1f-115">反復処理ごとに、アプリケーション徐々 に向上します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-115">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="78b1f-116">この複数のイテレーションのアプローチの目的は、各変更の理由を理解するためです。</span><span class="sxs-lookup"><span data-stu-id="78b1f-116">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="78b1f-117">繰り返し #1 - は、アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-117">Iteration #1 - Create the application.</span></span> <span data-ttu-id="78b1f-118">最初のイテレーションを作成、連絡先マネージャー最も簡単な方法で考えられるします。</span><span class="sxs-lookup"><span data-stu-id="78b1f-118">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="78b1f-119">基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="78b1f-119">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="78b1f-120">繰り返し #2 - は、素敵に見えるアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-120">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="78b1f-121">このイテレーションで、アプリケーションの見た目を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。</span><span class="sxs-lookup"><span data-stu-id="78b1f-121">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="78b1f-122">繰り返し #3 - フォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-122">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="78b1f-123">3 番目のイテレーションでは、基本的なフォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-123">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="78b1f-124">ユーザーは、必要なフォームのフィールドを完了しなくても、フォームを送信できないようにようにします。</span><span class="sxs-lookup"><span data-stu-id="78b1f-124">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="78b1f-125">また、電子メール アドレスと電話番号を検証しました。</span><span class="sxs-lookup"><span data-stu-id="78b1f-125">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="78b1f-126">繰り返し #4 - は、アプリケーションを疎結合を作成します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-126">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="78b1f-127">この 3 番目のイテレーションで、保守し、Contact Manager アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかの利点を実行します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-127">In this third iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="78b1f-128">たとえば、Repository パターンと依存関係の注入パターンを使用するようにアプリケーションをリファクタリングします。</span><span class="sxs-lookup"><span data-stu-id="78b1f-128">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="78b1f-129">繰り返し #5 - 単体テストを作成します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-129">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="78b1f-130">5 番目のイテレーションでアプリケーションと簡単に維持単体テストを追加して変更します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-130">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="78b1f-131">データ モデル クラスの模擬テストを実行し、コント ローラーと検証ロジックの単体テストをビルドします。</span><span class="sxs-lookup"><span data-stu-id="78b1f-131">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="78b1f-132">繰り返し #6 - は、テスト駆動開発を使用します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-132">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="78b1f-133">この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加には、まず単体テストの記述、単体テストに対してコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-133">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="78b1f-134">このイテレーションは、連絡先グループを追加します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-134">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="78b1f-135">繰り返し #7 - Ajax 機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-135">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="78b1f-136">7 番目のイテレーションで改良、応答性と、アプリケーションのパフォーマンスの Ajax のサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-136">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>


## <a name="this-iteration"></a><span data-ttu-id="78b1f-137">このイテレーション</span><span class="sxs-lookup"><span data-stu-id="78b1f-137">This Iteration</span></span>

<span data-ttu-id="78b1f-138">Contact Manager アプリケーションの 2 番目のイテレーションでは、基本的なフォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-138">In this second iteration of the Contact Manager application, we add basic form validation.</span></span> <span data-ttu-id="78b1f-139">により、ユーザーが必要なフォーム フィールドの値を入力しなくても、連絡先を送信できないようにします。</span><span class="sxs-lookup"><span data-stu-id="78b1f-139">We prevent people from submitting a contact without entering values for required form fields.</span></span> <span data-ttu-id="78b1f-140">また、電話番号、電子メール アドレスが (図 1 参照) を検証しました。</span><span class="sxs-lookup"><span data-stu-id="78b1f-140">We also validate phone numbers and email addresses (see Figure 1).</span></span>


<span data-ttu-id="78b1f-141">[![[新しいプロジェクト] ダイアログ ボックス](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="78b1f-141">[![The New Project dialog box](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)</span></span>

<span data-ttu-id="78b1f-142">**図 01**: 検証を使用して、フォーム ([フルサイズの画像を表示する をクリックします](iteration-3-add-form-validation-cs/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="78b1f-142">**Figure 01**: A form with validation ([Click to view full-size image](iteration-3-add-form-validation-cs/_static/image2.png))</span></span>


<span data-ttu-id="78b1f-143">このイテレーションでは、コント ローラー アクションに直接検証ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-143">In this iteration, we add the validation logic directly to the controller actions.</span></span> <span data-ttu-id="78b1f-144">一般に、これは、ASP.NET MVC アプリケーションに検証を追加することをお勧めの方法ではありません。</span><span class="sxs-lookup"><span data-stu-id="78b1f-144">In general, this is not the recommended way to add validation to an ASP.NET MVC application.</span></span> <span data-ttu-id="78b1f-145">別のアプリケーションの検証ロジックを配置するほうが効果的[サービス層](http://martinfowler.com/eaaCatalog/serviceLayer.html)します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-145">A better approach is to place an application s validation logic in a separate [service layer](http://martinfowler.com/eaaCatalog/serviceLayer.html).</span></span> <span data-ttu-id="78b1f-146">次のイテレーションは、アプリケーションを保守しやすくなりますように Contact Manager アプリケーションをリファクタリングします。</span><span class="sxs-lookup"><span data-stu-id="78b1f-146">In the next iteration, we refactor the Contact Manager application to make the application more maintainable.</span></span>

<span data-ttu-id="78b1f-147">このイテレーションで、簡単に記述するすべての検証コードを手動で。</span><span class="sxs-lookup"><span data-stu-id="78b1f-147">In this iteration, to keep things simple, we write all of the validation code by hand.</span></span> <span data-ttu-id="78b1f-148">自分たちの検証コードを記述する、する代わりに、検証フレームワークの利点を取得できます。 します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-148">Instead of writing the validation code ourselves, we could take advantage of a validation framework.</span></span> <span data-ttu-id="78b1f-149">たとえば、ASP.NET MVC アプリケーションの検証ロジックを実装するために、Microsoft エンタープライズ ライブラリ検証アプリケーション ブロック (VAB) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-149">For example, you can use the Microsoft Enterprise Library Validation Application Block (VAB) to implement the validation logic for your ASP.NET MVC application.</span></span> <span data-ttu-id="78b1f-150">詳細については、検証アプリケーション ブロックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="78b1f-150">To learn more about the Validation Application Block, see:</span></span>

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a><span data-ttu-id="78b1f-151">ビューを作成する検証の追加</span><span class="sxs-lookup"><span data-stu-id="78b1f-151">Adding Validation to the Create View</span></span>

<span data-ttu-id="78b1f-152">検証ロジックを作成ビューに追加することで開始 s を使用できます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-152">Let s start by adding validation logic to the Create view.</span></span> <span data-ttu-id="78b1f-153">さいわい、Visual Studio を使用したビューを作成する、生成されたため、ビューを作成する既に含まれていますすべての検証メッセージを表示するために必要なユーザー インターフェイス ロジック。</span><span class="sxs-lookup"><span data-stu-id="78b1f-153">Fortunately, because we generated the Create view with Visual Studio, the Create view already contains all of the necessary user interface logic to display validation messages.</span></span> <span data-ttu-id="78b1f-154">リスト 1 で、作成ビューが含まれています。</span><span class="sxs-lookup"><span data-stu-id="78b1f-154">The Create view is contained in Listing 1.</span></span>

<span data-ttu-id="78b1f-155">**Listing 1 - \Views\Contact\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="78b1f-155">**Listing 1 - \Views\Contact\Create.aspx**</span></span>

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

<span data-ttu-id="78b1f-156">HTML フォーム上にすぐに表示される Html.ValidationSummary() ヘルパー メソッドの呼び出しに注意してください。</span><span class="sxs-lookup"><span data-stu-id="78b1f-156">Notice the call to the Html.ValidationSummary() helper method that appears immediately above the HTML form.</span></span> <span data-ttu-id="78b1f-157">検証エラー メッセージがある場合、このメソッドは箇条書きリストに検証メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-157">If there are validation error messages, then this method displays validation messages in a bulleted list.</span></span>

<span data-ttu-id="78b1f-158">通知、さらに、各フォーム フィールドの横に表示される Html.ValidationMessage() への呼び出し。</span><span class="sxs-lookup"><span data-stu-id="78b1f-158">Notice, furthermore, the calls to Html.ValidationMessage() that appear next to each form field.</span></span> <span data-ttu-id="78b1f-159">ValidationMessage() ヘルパーには、個別の検証エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-159">The ValidationMessage() helper displays an individual validation error message.</span></span> <span data-ttu-id="78b1f-160">リストの 1 の場合は、検証エラーがある場合にアスタリスクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-160">In the case of Listing 1, an asterisk is displayed when there is a validation error.</span></span>

<span data-ttu-id="78b1f-161">最後に、ヘルパーによって表示されるプロパティに関連付けられている検証エラーがある場合に Html.TextBox() ヘルパーによってカスケード スタイル シートのクラスが自動的にレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="78b1f-161">Finally, the Html.TextBox() helper automatically renders a Cascading Style Sheet class when there is a validation error associated with the property displayed by the helper.</span></span> <span data-ttu-id="78b1f-162">Html.TextBox() ヘルパーという名前のクラスをレンダリングする**入力検証エラー**します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-162">The Html.TextBox() helper renders a class named **input-validation-error**.</span></span>

<span data-ttu-id="78b1f-163">新しい ASP.NET MVC アプリケーションを作成するときに Site.css という名前のスタイル シートがコンテンツのフォルダーに自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-163">When you create a new ASP.NET MVC application, a style sheet named Site.css is created in the Content folder automatically.</span></span> <span data-ttu-id="78b1f-164">このスタイル シートには、次の検証エラー メッセージの外観に関連する CSS クラスの定義が含まれています。</span><span class="sxs-lookup"><span data-stu-id="78b1f-164">This style sheet contains the following definitions for CSS classes related to the appearance of validation error messages:</span></span>

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

<span data-ttu-id="78b1f-165">フィールドの検証エラー クラスは、Html.ValidationMessage() ヘルパーによってレンダリングされる出力のスタイル設定に使用されます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-165">The field-validation-error class is used to style the output rendered by the Html.ValidationMessage() helper.</span></span> <span data-ttu-id="78b1f-166">入力検証エラー クラスは、Html.TextBox() ヘルパーによって表示されるテキスト ボックス (入力) のスタイル設定に使用されます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-166">The input-validation-error class is used to style the textbox (input) rendered by the Html.TextBox() helper.</span></span> <span data-ttu-id="78b1f-167">検証の概要-エラー クラスは、Html.ValidationSummary() ヘルパーによって表示される順序なしリストをスタイル設定に使用されます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-167">The validation-summary-errors class is used to style the unordered list rendered by the Html.ValidationSummary() helper.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="78b1f-168">検証エラー メッセージの外観をカスタマイズするには、このセクションで説明されているスタイル シートのクラスを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-168">You can modify the style sheet classes described in this section to customize the appearance of validation error messages.</span></span>


## <a name="adding-validation-logic-to-the-create-action"></a><span data-ttu-id="78b1f-169">検証ロジックを追加する、アクションの作成</span><span class="sxs-lookup"><span data-stu-id="78b1f-169">Adding Validation Logic to the Create Action</span></span>

<span data-ttu-id="78b1f-170">現在のところ、作成ビューは検証エラー メッセージをすべてのメッセージを生成するためのロジックを作成していないために表示されません。</span><span class="sxs-lookup"><span data-stu-id="78b1f-170">Right now, the Create view never displays validation error messages because we have not written the logic to generate any messages.</span></span> <span data-ttu-id="78b1f-171">検証エラー メッセージを表示するためには、ModelState にエラー メッセージを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="78b1f-171">In order to display validation error messages, you need to add the error messages to ModelState.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="78b1f-172">UpdateModel() メソッドのエラー メッセージに追加 ModelState 自動的にフォーム フィールドの値をプロパティに割り当てエラーがある場合にします。</span><span class="sxs-lookup"><span data-stu-id="78b1f-172">The UpdateModel() method adds error messages to ModelState automatically when there is an error assigning the value of a form field to a property.</span></span> <span data-ttu-id="78b1f-173">たとえば、"apple"の文字列を DateTime 値を受け入れる BirthDate プロパティに割り当てるしようとした場合、UpdateModel() メソッドに追加しますエラー ModelState。</span><span class="sxs-lookup"><span data-stu-id="78b1f-173">For example, if you attempt to assign the string "apple" to a BirthDate property that accepts DateTime values, then the UpdateModel() method adds an error to ModelState.</span></span>


<span data-ttu-id="78b1f-174">リスト 2 で修正された Create() メソッドには、新しい連絡先は、データベースに挿入される前に、Contact クラスのプロパティを検証する新しいセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="78b1f-174">The modified Create() method in Listing 2 contains a new section that validates the properties of the Contact class before the new contact is inserted into the database.</span></span>

<span data-ttu-id="78b1f-175">**2 - Controllers\ContactController.cs (検証で作成) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="78b1f-175">**Listing 2 - Controllers\ContactController.cs (Create with validation)**</span></span>

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

<span data-ttu-id="78b1f-176">検証のセクションでは、次の 4 つの個別の検証規則を適用します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-176">The validate section enforces four distinct validation rules:</span></span>

- <span data-ttu-id="78b1f-177">FirstName プロパティ、長さが 0 より大きい必要があります (およびスペースのみが構成することができません)</span><span class="sxs-lookup"><span data-stu-id="78b1f-177">The FirstName property must have a length greater than zero (and it cannot consist solely of spaces)</span></span>
- <span data-ttu-id="78b1f-178">LastName プロパティの長さが 0 より大きい必要があります (およびスペースのみが構成することができません)</span><span class="sxs-lookup"><span data-stu-id="78b1f-178">The LastName property must have a length greater than zero (and it cannot consist solely of spaces)</span></span>
- <span data-ttu-id="78b1f-179">電話プロパティに値が設定されている場合 (長さが 0 より大きい値を持つ) 電話プロパティ正規表現と一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="78b1f-179">If the Phone property has a value (has a length greater than 0) then the Phone property must match a regular expression.</span></span>
- <span data-ttu-id="78b1f-180">電子メール プロパティに値が設定されている場合 (長さが 0 より大きい値を持つ) し、電子メールのプロパティが正規表現に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="78b1f-180">If the Email property has a value (has a length greater than 0) then the Email property must match a regular expression.</span></span>

<span data-ttu-id="78b1f-181">検証規則違反がある場合に、エラー メッセージが AddModelError() メソッドのヘルプで ModelState に追加されます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-181">When there is a validation rule violation, an error message is added to ModelState with the help of the AddModelError() method.</span></span> <span data-ttu-id="78b1f-182">ModelState にメッセージを追加する場合は、プロパティの名前と、検証エラー メッセージのテキストを提供します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-182">When you add a message to ModelState, you provide the name of a property and the text of a validation error message.</span></span> <span data-ttu-id="78b1f-183">このエラー メッセージは、Html.ValidationSummary() および Html.ValidationMessage() ヘルパー メソッドによって、ビューに表示されます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-183">This error message is displayed in the view by the Html.ValidationSummary() and Html.ValidationMessage() helper methods.</span></span>

<span data-ttu-id="78b1f-184">検証規則が実行された後は、ModelState の IsValid プロパティがチェックします。</span><span class="sxs-lookup"><span data-stu-id="78b1f-184">After the validation rules are executed, the IsValid property of ModelState is checked.</span></span> <span data-ttu-id="78b1f-185">ModelState に検証エラー メッセージが追加されたときに、IsValid プロパティが false を返します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-185">The IsValid property returns false when any validation error messages have been added to ModelState.</span></span> <span data-ttu-id="78b1f-186">検証に失敗した場合は、エラー メッセージを使用したフォームを作成するが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-186">If validation fails, the Create form is redisplayed with the error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="78b1f-187">正規表現のリポジトリから電話番号、電子メール アドレスを検証するための正規表現が表示されました [*http://regexlib.com*](http://regexlib.com)</span><span class="sxs-lookup"><span data-stu-id="78b1f-187">I got the regular expressions for validating the phone number and email address from the regular expression repository at [*http://regexlib.com*](http://regexlib.com)</span></span>


## <a name="adding-validation-logic-to-the-edit-action"></a><span data-ttu-id="78b1f-188">編集の操作に検証ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-188">Adding Validation Logic to the Edit Action</span></span>

<span data-ttu-id="78b1f-189">Edit() アクションでは、連絡先を更新します。</span><span class="sxs-lookup"><span data-stu-id="78b1f-189">The Edit() action updates a Contact.</span></span> <span data-ttu-id="78b1f-190">Edit() アクションは、Create() アクションと正確に同じ検証を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="78b1f-190">The Edit() action needs to perform exactly the same validation as the Create() action.</span></span> <span data-ttu-id="78b1f-191">同じ検証コードを複製するのではなく Create() と Edit() の両方のアクションが同じ検証メソッドを呼び出せるように、連絡先のコント ローラーをリファクタリングする必要があります。</span><span class="sxs-lookup"><span data-stu-id="78b1f-191">Instead of duplicating the same validation code, we should refactor the Contact controller so that both the Create() and Edit() actions call the same validation method.</span></span>

<span data-ttu-id="78b1f-192">変更後にお問い合わせくださいコント ローラー クラスは、リスト 3 に含まれます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-192">The modified Contact controller class is contained in Listing 3.</span></span> <span data-ttu-id="78b1f-193">このクラスには、Create() と Edit() の両方のアクション内で呼び出される新しい ValidateContact() メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="78b1f-193">This class has a new ValidateContact() method that is called within both the Create() and Edit() actions.</span></span>

<span data-ttu-id="78b1f-194">**3 - Controllers\ContactController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="78b1f-194">**Listing 3 - Controllers\ContactController.cs**</span></span>

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a><span data-ttu-id="78b1f-195">まとめ</span><span class="sxs-lookup"><span data-stu-id="78b1f-195">Summary</span></span>

<span data-ttu-id="78b1f-196">このイテレーションでは、Contact Manager アプリケーションを基本的なフォーム検証を追加しました。</span><span class="sxs-lookup"><span data-stu-id="78b1f-196">In this iteration, we added basic form validation to our Contact Manager application.</span></span> <span data-ttu-id="78b1f-197">この検証ロジックでは、ユーザーが新しい連絡先を送信したり、FirstName および LastName プロパティの値を指定せず、既存の連絡先を編集できないようにします。</span><span class="sxs-lookup"><span data-stu-id="78b1f-197">Our validation logic prevents users from submitting a new contact or editing an existing contact without supplying values for the FirstName and LastName properties.</span></span> <span data-ttu-id="78b1f-198">さらに、ユーザーは、有効な電話番号と電子メール アドレスを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="78b1f-198">Furthermore, users must supply valid phone numbers and email addresses.</span></span>

<span data-ttu-id="78b1f-199">このイテレーションで最も簡単な方法で、Contact Manager アプリケーションに検証ロジックをしました。</span><span class="sxs-lookup"><span data-stu-id="78b1f-199">In this iteration, we added the validation logic to our Contact Manager application in the easiest way possible.</span></span> <span data-ttu-id="78b1f-200">ただしに、コント ローラー ロジック、検証ロジックを混在させる問題が発生私たちにとって長期的にします。</span><span class="sxs-lookup"><span data-stu-id="78b1f-200">However, mixing our validation logic into our controller logic will create problems for us in the long term.</span></span> <span data-ttu-id="78b1f-201">アプリケーションの保守し、時間の経過と共に変更をより困難になります。</span><span class="sxs-lookup"><span data-stu-id="78b1f-201">Our application will be more difficult to maintain and modify over time.</span></span>

<span data-ttu-id="78b1f-202">次のイテレーションで、検証ロジックとデータベース アクセス ロジックをコント ローラーからにリファクタリングされます。</span><span class="sxs-lookup"><span data-stu-id="78b1f-202">In the next iteration, we will refactor our validation logic and database access logic out of our controllers.</span></span> <span data-ttu-id="78b1f-203">柔軟に結合し、保守性がアプリケーションを作成することを有効にするソフトウェア設計の原則をいくつかの利点がご説明しましょう。</span><span class="sxs-lookup"><span data-stu-id="78b1f-203">We'll take advantage of several software design principles to enable us to create a more loosely coupled, and more maintainable, application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="78b1f-204">[前へ](iteration-2-make-the-application-look-nice-cs.md)
> [次へ](iteration-4-make-the-application-loosely-coupled-cs.md)</span><span class="sxs-lookup"><span data-stu-id="78b1f-204">[Previous](iteration-2-make-the-application-look-nice-cs.md)
[Next](iteration-4-make-the-application-loosely-coupled-cs.md)</span></span>
