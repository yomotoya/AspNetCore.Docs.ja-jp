---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: "イテレーション 3 – 追加のフォーム検証 (VB) |Microsoft ドキュメント"
author: microsoft
description: "3 番目のイテレーションは、基本フォーム検証を追加します。 おは人が必要なフォームのフィールドを完了しなくても、フォームを送信することを防ぐ。 Emai も検証する."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: e9ed182fb58addd8c5dadbe6e3d09c391840ca00
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="iteration-3--add-form-validation-vb"></a><span data-ttu-id="c5983-105">イテレーション 3 – 追加のフォーム検証 (VB)</span><span class="sxs-lookup"><span data-stu-id="c5983-105">Iteration #3 – Add form validation (VB)</span></span>
====================
<span data-ttu-id="c5983-106">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c5983-106">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="c5983-107">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="c5983-107">Download Code</span></span>](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> <span data-ttu-id="c5983-108">3 番目のイテレーションは、基本フォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="c5983-108">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="c5983-109">おは人が必要なフォームのフィールドを完了しなくても、フォームを送信することを防ぐ。</span><span class="sxs-lookup"><span data-stu-id="c5983-109">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="c5983-110">私たちも電子メール アドレスと電話番号を検証します。</span><span class="sxs-lookup"><span data-stu-id="c5983-110">We also validate email addresses and phone numbers.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a><span data-ttu-id="c5983-111">連絡先管理 ASP.NET MVC アプリケーション (VB) のビルド</span><span class="sxs-lookup"><span data-stu-id="c5983-111">Building a Contact Management ASP.NET MVC Application (VB)</span></span>
  

<span data-ttu-id="c5983-112">この一連のチュートリアルは、連絡先管理アプリケーション全体が開始されてから完了するを構築します。</span><span class="sxs-lookup"><span data-stu-id="c5983-112">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="c5983-113">お問い合わせのマネージャー アプリケーションでは、人のユーザーの一覧については使用すると、連絡先情報の名前、電話番号、電子メール アドレスを格納できます。</span><span class="sxs-lookup"><span data-stu-id="c5983-113">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="c5983-114">私たちは、複数のイテレーションにおける、アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="c5983-114">We build the application over multiple iterations.</span></span> <span data-ttu-id="c5983-115">各イテレーションで、アプリケーション、徐々 に向上します。</span><span class="sxs-lookup"><span data-stu-id="c5983-115">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="c5983-116">この複数のイテレーション アプローチの目的は、各変更の理由を理解するためです。</span><span class="sxs-lookup"><span data-stu-id="c5983-116">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="c5983-117">イテレーション 1 には、アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="c5983-117">Iteration #1 - Create the application.</span></span> <span data-ttu-id="c5983-118">最初のイテレーションでお連絡先のマネージャー最も簡単な方法で可能なを作成します。</span><span class="sxs-lookup"><span data-stu-id="c5983-118">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="c5983-119">基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="c5983-119">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="c5983-120">イテレーション 2 では、素敵に見えるアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="c5983-120">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="c5983-121">このイテレーションで、アプリケーションの外観を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。</span><span class="sxs-lookup"><span data-stu-id="c5983-121">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="c5983-122">イテレーション 3 - フォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="c5983-122">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="c5983-123">3 番目のイテレーションは、基本フォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="c5983-123">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="c5983-124">おは人が必要なフォームのフィールドを完了しなくても、フォームを送信することを防ぐ。</span><span class="sxs-lookup"><span data-stu-id="c5983-124">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="c5983-125">私たちも電子メール アドレスと電話番号を検証します。</span><span class="sxs-lookup"><span data-stu-id="c5983-125">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="c5983-126">4: イテレーションは、疎結合アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="c5983-126">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="c5983-127">この 3 番目のイテレーションで利用の保守し、連絡先のマネージャー アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかのです。</span><span class="sxs-lookup"><span data-stu-id="c5983-127">In this third iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="c5983-128">たとえば、リポジトリ パターンと依存関係の挿入のパターンを使用するようにアプリケーションをリファクターします。</span><span class="sxs-lookup"><span data-stu-id="c5983-128">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="c5983-129">イテレーション #5 - 単体テストを作成します。</span><span class="sxs-lookup"><span data-stu-id="c5983-129">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="c5983-130">5 番目のイテレーションでおやすく、アプリケーションを維持し、単体テストを追加して変更できます。</span><span class="sxs-lookup"><span data-stu-id="c5983-130">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="c5983-131">データ モデル クラスを模擬表示し、コント ローラーと検証ロジックの単体テストをビルドします。</span><span class="sxs-lookup"><span data-stu-id="c5983-131">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="c5983-132">イテレーション 6 - テスト駆動開発を使用します。</span><span class="sxs-lookup"><span data-stu-id="c5983-132">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="c5983-133">この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加おには、まず単体テストを記述し、単体テストに対してコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="c5983-133">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="c5983-134">このイテレーションは、連絡先グループを追加します。</span><span class="sxs-lookup"><span data-stu-id="c5983-134">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="c5983-135">イテレーション #7 - Ajax 機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="c5983-135">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="c5983-136">7 番目のイテレーションでお、応答性およびパフォーマンスの向上、アプリケーションの Ajax のサポートを追加することで。</span><span class="sxs-lookup"><span data-stu-id="c5983-136">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>


## <a name="this-iteration"></a><span data-ttu-id="c5983-137">このイテレーション</span><span class="sxs-lookup"><span data-stu-id="c5983-137">This Iteration</span></span>

<span data-ttu-id="c5983-138">連絡先のマネージャー アプリケーションの 2 番目のイテレーションは、基本フォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="c5983-138">In this second iteration of the Contact Manager application, we add basic form validation.</span></span> <span data-ttu-id="c5983-139">ユーザーは、必要なフォーム フィールドの値を入力することがなく、連絡先を送信してからようにします。</span><span class="sxs-lookup"><span data-stu-id="c5983-139">We prevent people from submitting a contact without entering values for required form fields.</span></span> <span data-ttu-id="c5983-140">私たちは、電話番号、電子メール アドレス (図 1 を参照してください) にも検証します。</span><span class="sxs-lookup"><span data-stu-id="c5983-140">We also validate phone numbers and email addresses (see Figure 1).</span></span>


<span data-ttu-id="c5983-141">[![[新しいプロジェクト] ダイアログ ボックス](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c5983-141">[![The New Project dialog box](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)</span></span>

<span data-ttu-id="c5983-142">**図 01**: 検証とフォーム ([フルサイズのイメージを表示するをクリックして](iteration-3-add-form-validation-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="c5983-142">**Figure 01**: A form with validation ([Click to view full-size image](iteration-3-add-form-validation-vb/_static/image2.png))</span></span>


<span data-ttu-id="c5983-143">このイテレーションに、コント ローラーのアクションに直接検証ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="c5983-143">In this iteration, we add the validation logic directly to the controller actions.</span></span> <span data-ttu-id="c5983-144">通常、これは、ASP.NET MVC アプリケーションに検証を追加することをお勧めではありません。</span><span class="sxs-lookup"><span data-stu-id="c5983-144">In general, this is not the recommended way to add validation to an ASP.NET MVC application.</span></span> <span data-ttu-id="c5983-145">適切な方法は、別のアプリケーションの検証ロジックを配置する[サービス層](http://martinfowler.com/eaaCatalog/serviceLayer.html)です。</span><span class="sxs-lookup"><span data-stu-id="c5983-145">A better approach is to place an application s validation logic in a separate [service layer](http://martinfowler.com/eaaCatalog/serviceLayer.html).</span></span> <span data-ttu-id="c5983-146">次のイテレーションでは、アプリケーションの保守が簡単にする連絡先のマネージャー アプリケーションをリファクタリングします。</span><span class="sxs-lookup"><span data-stu-id="c5983-146">In the next iteration, we refactor the Contact Manager application to make the application more maintainable.</span></span>

<span data-ttu-id="c5983-147">このイテレーションで煩雑にならないように、記述のすべての検証コードを手動でします。</span><span class="sxs-lookup"><span data-stu-id="c5983-147">In this iteration, to keep things simple, we write all of the validation code by hand.</span></span> <span data-ttu-id="c5983-148">検証コードを自分で記述する代わりには、検証フレームワークの利点おがかかります。</span><span class="sxs-lookup"><span data-stu-id="c5983-148">Instead of writing the validation code ourselves, we could take advantage of a validation framework.</span></span> <span data-ttu-id="c5983-149">たとえば、ASP.NET MVC アプリケーションの検証ロジックを実装するのに Microsoft エンタープライズ ライブラリ検証アプリケーション ブロック (VAB) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="c5983-149">For example, you can use the Microsoft Enterprise Library Validation Application Block (VAB) to implement the validation logic for your ASP.NET MVC application.</span></span> <span data-ttu-id="c5983-150">検証アプリケーション ブロックに関する詳細についてを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c5983-150">To learn more about the Validation Application Block, see:</span></span>

[<span data-ttu-id="c5983-151">*http://msdn.microsoft.com/library/dd203099.aspx*</span><span class="sxs-lookup"><span data-stu-id="c5983-151">*http://msdn.microsoft.com/library/dd203099.aspx*</span></span>](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a><span data-ttu-id="c5983-152">ビューを作成する検証の追加</span><span class="sxs-lookup"><span data-stu-id="c5983-152">Adding Validation to the Create View</span></span>

<span data-ttu-id="c5983-153">ビューを作成するに検証ロジックを追加することによって開始 s を使用できます。</span><span class="sxs-lookup"><span data-stu-id="c5983-153">Let s start by adding validation logic to the Create view.</span></span> <span data-ttu-id="c5983-154">さいわい、ビューを作成する Visual Studio で生成されたため、ビューを作成する既に含まれていますすべての検証メッセージを表示するために必要なユーザー インターフェイス ロジック。</span><span class="sxs-lookup"><span data-stu-id="c5983-154">Fortunately, because we generated the Create view with Visual Studio, the Create view already contains all of the necessary user interface logic to display validation messages.</span></span> <span data-ttu-id="c5983-155">ビューを作成するは、1 のリストに含まれます。</span><span class="sxs-lookup"><span data-stu-id="c5983-155">The Create view is contained in Listing 1.</span></span>

<span data-ttu-id="c5983-156">**Listing 1 - \Views\Contact\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="c5983-156">**Listing 1 - \Views\Contact\Create.aspx**</span></span>

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

<span data-ttu-id="c5983-157">フィールドの検証エラーのクラスは Html.ValidationMessage() ヘルパーによって表示される出力のスタイル設定に使用されます。</span><span class="sxs-lookup"><span data-stu-id="c5983-157">The field-validation-error class is used to style the output rendered by the Html.ValidationMessage() helper.</span></span> <span data-ttu-id="c5983-158">入力の検証エラーのクラスは Html.TextBox() ヘルパーによって表示されるテキスト ボックス (入力) のスタイル設定に使用されます。</span><span class="sxs-lookup"><span data-stu-id="c5983-158">The input-validation-error class is used to style the textbox (input) rendered by the Html.TextBox() helper.</span></span> <span data-ttu-id="c5983-159">検証の概要-エラー クラスを使用して、Html.ValidationSummary() ヘルパーによって表示される順序なしリストのスタイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="c5983-159">The validation-summary-errors class is used to style the unordered list rendered by the Html.ValidationSummary() helper.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c5983-160">検証エラー メッセージの外観をカスタマイズするには、このセクションで説明されているスタイル シートのクラスを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="c5983-160">You can modify the style sheet classes described in this section to customize the appearance of validation error messages.</span></span>


## <a name="adding-validation-logic-to-the-create-action"></a><span data-ttu-id="c5983-161">検証ロジックを追加する、操作の作成</span><span class="sxs-lookup"><span data-stu-id="c5983-161">Adding Validation Logic to the Create Action</span></span>

<span data-ttu-id="c5983-162">今すぐ,、ビューを作成するは、すべてのメッセージを生成するロジックがない書き込まため決して検証エラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="c5983-162">Right now, the Create view never displays validation error messages because we have not written the logic to generate any messages.</span></span> <span data-ttu-id="c5983-163">検証エラー メッセージを表示するためには、ModelState にエラー メッセージを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c5983-163">In order to display validation error messages, you need to add the error messages to ModelState.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c5983-164">UpdateModel() メソッドのエラー メッセージに追加 ModelState 自動的に、プロパティに、フォーム フィールドの値を割り当てることでエラーがある場合にします。</span><span class="sxs-lookup"><span data-stu-id="c5983-164">The UpdateModel() method adds error messages to ModelState automatically when there is an error assigning the value of a form field to a property.</span></span> <span data-ttu-id="c5983-165">たとえば、"apple"の文字列を DateTime 値を受け入れる BirthDate プロパティに割り当てるしようとする場合、UpdateModel() メソッド エラーを追加 ModelState です。</span><span class="sxs-lookup"><span data-stu-id="c5983-165">For example, if you attempt to assign the string "apple" to a BirthDate property that accepts DateTime values, then the UpdateModel() method adds an error to ModelState.</span></span>


<span data-ttu-id="c5983-166">リスト 2 で修正 Create() メソッドには、新しい連絡先は、データベースに挿入される前に、連絡先クラスのプロパティを検証するための新しいセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c5983-166">The modified Create() method in Listing 2 contains a new section that validates the properties of the Contact class before the new contact is inserted into the database.</span></span>

<span data-ttu-id="c5983-167">**2 - Controllers\ContactController.vb (検証で作成する) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="c5983-167">**Listing 2 - Controllers\ContactController.vb (Create with validation)**</span></span>

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

<span data-ttu-id="c5983-168">検証のセクションでは、次の 4 つの個別の検証規則を適用します。</span><span class="sxs-lookup"><span data-stu-id="c5983-168">The validate section enforces four distinct validation rules:</span></span>

- <span data-ttu-id="c5983-169">FirstName プロパティの長さが 0 より大きい必要があります (とスペースだけので構成されていることはできません)</span><span class="sxs-lookup"><span data-stu-id="c5983-169">The FirstName property must have a length greater than zero (and it cannot consist solely of spaces)</span></span>
- <span data-ttu-id="c5983-170">LastName プロパティの長さが 0 より大きい必要があります (とスペースだけので構成されていることはできません)</span><span class="sxs-lookup"><span data-stu-id="c5983-170">The LastName property must have a length greater than zero (and it cannot consist solely of spaces)</span></span>
- <span data-ttu-id="c5983-171">Phone プロパティの値が (長さが 0 より大きい値を持つ) し、Phone プロパティは、正規表現と一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c5983-171">If the Phone property has a value (has a length greater than 0) then the Phone property must match a regular expression.</span></span>
- <span data-ttu-id="c5983-172">電子メール プロパティの値が (長さが 0 より大きい値を持つ)、Email プロパティは、正規表現と一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c5983-172">If the Email property has a value (has a length greater than 0) then the Email property must match a regular expression.</span></span>

<span data-ttu-id="c5983-173">検証規則の違反がある場合は、エラー メッセージが AddModelError() メソッドを利用して ModelState に追加されます。</span><span class="sxs-lookup"><span data-stu-id="c5983-173">When there is a validation rule violation, an error message is added to ModelState with the help of the AddModelError() method.</span></span> <span data-ttu-id="c5983-174">ModelState にメッセージを追加する場合は、プロパティの名前と、検証エラー メッセージのテキストを提供します。</span><span class="sxs-lookup"><span data-stu-id="c5983-174">When you add a message to ModelState, you provide the name of a property and the text of a validation error message.</span></span> <span data-ttu-id="c5983-175">このエラー メッセージは、Html.ValidationSummary() および Html.ValidationMessage() ヘルパー メソッドによって、ビューに表示されます。</span><span class="sxs-lookup"><span data-stu-id="c5983-175">This error message is displayed in the view by the Html.ValidationSummary() and Html.ValidationMessage() helper methods.</span></span>

<span data-ttu-id="c5983-176">検証規則が実行された後の ModelState IsValid プロパティがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="c5983-176">After the validation rules are executed, the IsValid property of ModelState is checked.</span></span> <span data-ttu-id="c5983-177">ModelState に検証エラー メッセージが追加されたときに、IsValid プロパティが false を返します。</span><span class="sxs-lookup"><span data-stu-id="c5983-177">The IsValid property returns false when any validation error messages have been added to ModelState.</span></span> <span data-ttu-id="c5983-178">検証に失敗した場合は、エラー メッセージにフォームを作成するが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="c5983-178">If validation fails, the Create form is redisplayed with the error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c5983-179">正規表現のリポジトリからの電話番号と電子メール アドレスを検証するための正規表現が[ *http://regexlib.com*](http://regexlib.com)</span><span class="sxs-lookup"><span data-stu-id="c5983-179">I got the regular expressions for validating the phone number and email address from the regular expression repository at [*http://regexlib.com*](http://regexlib.com)</span></span>


## <a name="adding-validation-logic-to-the-edit-action"></a><span data-ttu-id="c5983-180">編集の操作に検証ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="c5983-180">Adding Validation Logic to the Edit Action</span></span>

<span data-ttu-id="c5983-181">Edit() 操作では、連絡先を更新します。</span><span class="sxs-lookup"><span data-stu-id="c5983-181">The Edit() action updates a Contact.</span></span> <span data-ttu-id="c5983-182">Edit() アクションは、Create() アクションと正確に同じ検証を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c5983-182">The Edit() action needs to perform exactly the same validation as the Create() action.</span></span> <span data-ttu-id="c5983-183">同じ検証コードを複製するには、代わりに、Create() および Edit() 操作の両方が同じ検証メソッドを呼び出すように連絡先コント ローラーをリファクターお必要があります。</span><span class="sxs-lookup"><span data-stu-id="c5983-183">Instead of duplicating the same validation code, we should refactor the Contact controller so that both the Create() and Edit() actions call the same validation method.</span></span>

<span data-ttu-id="c5983-184">変更された連絡先のコント ローラー クラスは、リスト 3 に含まれます。</span><span class="sxs-lookup"><span data-stu-id="c5983-184">The modified Contact controller class is contained in Listing 3.</span></span> <span data-ttu-id="c5983-185">このクラスには、Create() と Edit() アクション内で呼び出される新しい ValidateContact() メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="c5983-185">This class has a new ValidateContact() method that is called within both the Create() and Edit() actions.</span></span>

<span data-ttu-id="c5983-186">**3 - Controllers\ContactController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="c5983-186">**Listing 3 - Controllers\ContactController.vb**</span></span>

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a><span data-ttu-id="c5983-187">まとめ</span><span class="sxs-lookup"><span data-stu-id="c5983-187">Summary</span></span>

<span data-ttu-id="c5983-188">このイテレーションでは、この連絡先のマネージャー アプリケーションに基本的な形式の検証を追加しました。</span><span class="sxs-lookup"><span data-stu-id="c5983-188">In this iteration, we added basic form validation to our Contact Manager application.</span></span> <span data-ttu-id="c5983-189">この検証ロジックでは、ユーザーの新しい連絡先を送信または FirstName および LastName から構成プロパティの値を指定せず、既存の連絡先を編集できなくなります。</span><span class="sxs-lookup"><span data-stu-id="c5983-189">Our validation logic prevents users from submitting a new contact or editing an existing contact without supplying values for the FirstName and LastName properties.</span></span> <span data-ttu-id="c5983-190">さらに、ユーザーは、有効な電話番号と電子メール アドレスを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c5983-190">Furthermore, users must supply valid phone numbers and email addresses.</span></span>

<span data-ttu-id="c5983-191">このイテレーションにロジックを追加した、検証、連絡先のマネージャー アプリケーションに最も簡単な方法でします。</span><span class="sxs-lookup"><span data-stu-id="c5983-191">In this iteration, we added the validation logic to our Contact Manager application in the easiest way possible.</span></span> <span data-ttu-id="c5983-192">ただし、コント ローラー ロジックに、検証ロジックを混在させるによって問題が発生の長期的なです。</span><span class="sxs-lookup"><span data-stu-id="c5983-192">However, mixing our validation logic into our controller logic will create problems for us in the long term.</span></span> <span data-ttu-id="c5983-193">アプリケーションは管理や時間の経過と共に変更が困難になります。</span><span class="sxs-lookup"><span data-stu-id="c5983-193">Our application will be more difficult to maintain and modify over time.</span></span>

<span data-ttu-id="c5983-194">次のイテレーションで、検証ロジックとデータベース アクセス ロジックをコント ローラー外にリファクタリングがします。</span><span class="sxs-lookup"><span data-stu-id="c5983-194">In the next iteration, we will refactor our validation logic and database access logic out of our controllers.</span></span> <span data-ttu-id="c5983-195">より柔軟に結合し、保守性がアプリケーションを作成することを有効にするソフトウェア設計の原則をいくつかの利点をみましょう。</span><span class="sxs-lookup"><span data-stu-id="c5983-195">We'll take advantage of several software design principles to enable us to create a more loosely coupled, and more maintainable, application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c5983-196">[前へ](iteration-2-make-the-application-look-nice-vb.md)
[次へ](iteration-4-make-the-application-loosely-coupled-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c5983-196">[Previous](iteration-2-make-the-application-look-nice-vb.md)
[Next](iteration-4-make-the-application-loosely-coupled-vb.md)</span></span>
