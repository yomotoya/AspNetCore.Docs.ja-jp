---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: "手順 9: 登録とチェック アウト |Microsoft ドキュメント"
author: jongalloway
description: "このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 一部 9 では、登録およびチェック アウトを説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 799985f889b1063c53df7bce365ae3989809ba79
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="908dc-104">手順 9: 登録とチェック アウト</span><span class="sxs-lookup"><span data-stu-id="908dc-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="908dc-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="908dc-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="908dc-106">MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="908dc-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="908dc-107">MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。</span><span class="sxs-lookup"><span data-stu-id="908dc-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="908dc-108">このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="908dc-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="908dc-109">一部 9 では、登録およびチェック アウトを説明します。</span><span class="sxs-lookup"><span data-stu-id="908dc-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="908dc-110">このセクションでお今後を作成する買い物のアドレスと支払い情報を収集する CheckoutController です。</span><span class="sxs-lookup"><span data-stu-id="908dc-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="908dc-111">ユーザーこのコント ローラーが承認を必要とするようにチェック アウトするの前に、サイトを登録することが求められます。</span><span class="sxs-lookup"><span data-stu-id="908dc-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="908dc-112">ユーザーは、「チェック アウト」ボタンをクリックしてショッピング カートに入ってからチェック アウト プロセスに移動されます。</span><span class="sxs-lookup"><span data-stu-id="908dc-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="908dc-113">ユーザーがログインしていない場合に求められます。</span><span class="sxs-lookup"><span data-stu-id="908dc-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="908dc-114">正常にログイン、ユーザー、アドレスと支払ビュー、表示されます。</span><span class="sxs-lookup"><span data-stu-id="908dc-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="908dc-115">フォームをいっぱいになり、注文を送信するが後に、表示されます、注文確認画面。</span><span class="sxs-lookup"><span data-stu-id="908dc-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="908dc-116">存在しない順序かに属していない注文を表示しようとすると、エラー ビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="908dc-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="908dc-117">ショッピング カートを移行します。</span><span class="sxs-lookup"><span data-stu-id="908dc-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="908dc-118">ショッピング プロセスが、匿名ユーザーがチェック アウト ボタンをクリックすると、ときに、登録する必要がありますとログインします。</span><span class="sxs-lookup"><span data-stu-id="908dc-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="908dc-119">ユーザーは、登録またはログインを完了すると、ユーザーに、ショッピング カート情報を関連付ける必要がありますが、間、ショッピング カート情報を維持するを必要とします。</span><span class="sxs-lookup"><span data-stu-id="908dc-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="908dc-120">これは実際には、単純な ShoppingCart クラスには既にユーザー名で現在カート内のすべての項目を関連付けるメソッド。</span><span class="sxs-lookup"><span data-stu-id="908dc-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="908dc-121">ユーザーが登録またはログインを完了すると、このメソッドを呼び出す必要がありますがだけです。</span><span class="sxs-lookup"><span data-stu-id="908dc-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="908dc-122">開く、 **AccountController**おメンバーシップと承認を設定しているときに追加したクラスです。</span><span class="sxs-lookup"><span data-stu-id="908dc-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="908dc-123">追加、次の MigrateShoppingCart メソッドを追加し、MvcMusicStore.Models を参照するステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="908dc-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="908dc-124">次のように、ユーザーが確認されると、MigrateShoppingCart を呼び出すログオン後のアクションを次に、変更するには。</span><span class="sxs-lookup"><span data-stu-id="908dc-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="908dc-125">ユーザー アカウントが正常に作成されるとすぐにアクションを投稿レジスタに同じ変更を行います。</span><span class="sxs-lookup"><span data-stu-id="908dc-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="908dc-126">です - 匿名のショッピング カートは自動的に登録に成功したか、ログイン時にユーザー アカウントに転送するようになりました。</span><span class="sxs-lookup"><span data-stu-id="908dc-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="908dc-127">CheckoutController を作成します。</span><span class="sxs-lookup"><span data-stu-id="908dc-127">Creating the CheckoutController</span></span>

<span data-ttu-id="908dc-128">コント ローラーのフォルダーを右クリックし、新しいコント ローラーを空のコント ローラーのテンプレートを使用して CheckoutController をという名前のプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="908dc-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="908dc-129">、、コント ローラー クラスの宣言の上にチェック アウトする前に登録するユーザーに要求する承認属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="908dc-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="908dc-130">*注: これは以前に行った、StoreManagerController への変更に似ていますが、Authorize attribute のユーザーが管理者ロールであることが必要な場合は。チェック アウト コント ローラーでおいるを必要とするユーザーがログインする管理者であることが必要なされません。*</span><span class="sxs-lookup"><span data-stu-id="908dc-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="908dc-131">わかりやすくするためは、おはこのチュートリアルでは支払情報を扱うはありません。</span><span class="sxs-lookup"><span data-stu-id="908dc-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="908dc-132">代わりに、ユーザーがチェック アウト プロモーション コードを使用できるようにしています。</span><span class="sxs-lookup"><span data-stu-id="908dc-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="908dc-133">PromoCode を名前付き定数を使用してこのプロモーション コードを格納します。</span><span class="sxs-lookup"><span data-stu-id="908dc-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="908dc-134">StoreController と同様に、storeDB をという名前の MusicStoreEntities クラスのインスタンスを保持するためのフィールドを宣言します。</span><span class="sxs-lookup"><span data-stu-id="908dc-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="908dc-135">作成するために MusicStoreEntities クラスを使用して、使用して、追加する必要があります。 MvcMusicStore.Models 名前空間のステートメント。</span><span class="sxs-lookup"><span data-stu-id="908dc-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="908dc-136">下のチェック アウト コント ローラーの上部に表示されます。</span><span class="sxs-lookup"><span data-stu-id="908dc-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="908dc-137">CheckoutController は、次のコント ローラーのアクションが適用されます。</span><span class="sxs-lookup"><span data-stu-id="908dc-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="908dc-138">**AddressAndPayment (GET メソッド)**ユーザーに、それらの情報の入力を許可するフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="908dc-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="908dc-139">**AddressAndPayment (POST メソッド)**入力を検証し、注文を処理します。</span><span class="sxs-lookup"><span data-stu-id="908dc-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="908dc-140">**完全な**ユーザーには、チェック アウト処理が正常に完了した後に表示されます。</span><span class="sxs-lookup"><span data-stu-id="908dc-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="908dc-141">このビューは、確認として、ユーザーの注文番号に含まれます。</span><span class="sxs-lookup"><span data-stu-id="908dc-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="908dc-142">まず、みましょう AddressAndPayment する (これは、コント ローラーを作成したときに生成された) インデックス コント ローラー アクションの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="908dc-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="908dc-143">このコント ローラーのアクションでは、モデル情報をする必要がないため、チェック アウト形式であるだけ表示します。</span><span class="sxs-lookup"><span data-stu-id="908dc-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="908dc-144">当社 AddressAndPayment POST メソッドは、同じパターンに従う、StoreManagerController で使用: フォームの送信に同意し、注文を完了しようして失敗した場合は、フォームが表示されます再です。</span><span class="sxs-lookup"><span data-stu-id="908dc-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="908dc-145">フォームの入力を検証する、検証要件を満たしている注文に対して後直接 PromoCode フォーム値が確認されます。</span><span class="sxs-lookup"><span data-stu-id="908dc-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="908dc-146">すべてが正しい順序で更新された情報を保存しますと仮定した場合、注文プロセスを完了し、完全なアクションにリダイレクト ShoppingCart オブジェクトに伝えます。</span><span class="sxs-lookup"><span data-stu-id="908dc-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="908dc-147">チェック アウト プロセスの正常完了時に、ユーザーは、完全なコント ローラー アクションにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="908dc-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="908dc-148">この操作では、順序が、確認メッセージと注文番号を表示する前に実際にログインしたユーザーに属しているはことを検証する簡単なチェックを実行します。</span><span class="sxs-lookup"><span data-stu-id="908dc-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="908dc-149">*注: エラー ビュー自動的に作成された us/Views/Shared フォルダーにプロジェクトを開始したとき。*</span><span class="sxs-lookup"><span data-stu-id="908dc-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="908dc-150">CheckoutController の完全なコードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="908dc-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="908dc-151">AddressAndPayment ビューの追加</span><span class="sxs-lookup"><span data-stu-id="908dc-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="908dc-152">ここで、AddressAndPayment ビューを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="908dc-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="908dc-153">1 つを右クリックし、AddressAndPayment コント ローラーのアクションを次に示すように順序として厳密に型指定し、テンプレートの編集を使用している AddressAndPayment をという名前のビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="908dc-153">Right-click on one of the the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="908dc-154">このビューにより、2 つの StoreManagerEdit ビューの作成中に見てお手法の使用します。</span><span class="sxs-lookup"><span data-stu-id="908dc-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="908dc-155">Html.EditorForModel() を使用して、注文のモデルのフォーム フィールドを表示するには</span><span class="sxs-lookup"><span data-stu-id="908dc-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="908dc-156">検証属性を持つ、Order クラスを使用する検証規則を活用し、</span><span class="sxs-lookup"><span data-stu-id="908dc-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="908dc-157">まず、Html.EditorForModel()、プロモーション コードの後に追加のテキスト ボックスを使用するフォームのコードを更新します。</span><span class="sxs-lookup"><span data-stu-id="908dc-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="908dc-158">AddressAndPayment ビューの完全なコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="908dc-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="908dc-159">注文の検証規則を定義します。</span><span class="sxs-lookup"><span data-stu-id="908dc-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="908dc-160">これで、ビューの設定おは検証ルールをモデルの設定、順序以前と同様アルバムのモデルの。</span><span class="sxs-lookup"><span data-stu-id="908dc-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="908dc-161">モデル フォルダーを右クリックし、注文をという名前のクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="908dc-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="908dc-162">属性に加えて、検証、アルバムの以前を使用したも使用する正規表現、ユーザーの電子メール アドレスを検証します。</span><span class="sxs-lookup"><span data-stu-id="908dc-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's e-mail address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="908dc-163">しようとしてが欠落しているフォームを送信または、無効な情報がクライアント側の検証を使用してエラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="908dc-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="908dc-164">これで、チェック アウト プロセスの面倒な作業のほとんどに実行しました。[完了] に、いくつかのオッズ and 端があるだけです。</span><span class="sxs-lookup"><span data-stu-id="908dc-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="908dc-165">2 つの単純なビューを追加する必要があり、ハンドオフ カート情報、ログイン プロセス中に対処する必要があります。</span><span class="sxs-lookup"><span data-stu-id="908dc-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="908dc-166">チェック アウトの完全なビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="908dc-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="908dc-167">注文の ID を表示するだけで済みますチェック アウトの完全なビューが非常に単純ですが、</span><span class="sxs-lookup"><span data-stu-id="908dc-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="908dc-168">完全なコント ローラーのアクションを右クリックし、int として厳密に型指定の完了をという名前のビューの追加</span><span class="sxs-lookup"><span data-stu-id="908dc-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="908dc-169">これで次のように、注文 ID を表示するコードの表示を更新します。</span><span class="sxs-lookup"><span data-stu-id="908dc-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="908dc-170">エラー ビューを更新</span><span class="sxs-lookup"><span data-stu-id="908dc-170">Updating The Error view</span></span>

<span data-ttu-id="908dc-171">既定のテンプレートにはが再利用できる他の場所、サイトのように、共有ビュー フォルダーに、エラー ビューが含まれます。</span><span class="sxs-lookup"><span data-stu-id="908dc-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="908dc-172">このエラー ビューは、非常に単純なエラーが含まれ、それが更新されますので、サイトのレイアウトを使用しません。</span><span class="sxs-lookup"><span data-stu-id="908dc-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="908dc-173">これは汎用エラー ページであるため、コンテンツは非常に単純です。</span><span class="sxs-lookup"><span data-stu-id="908dc-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="908dc-174">ユーザーがそのアクションを再試行する場合は、履歴に前のページに移動するには、メッセージとのリンクを含めます。</span><span class="sxs-lookup"><span data-stu-id="908dc-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


>[!div class="step-by-step"]
<span data-ttu-id="908dc-175">[前へ](mvc-music-store-part-8.md)
[次へ](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="908dc-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
