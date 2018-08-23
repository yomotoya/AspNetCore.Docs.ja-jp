---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'パート 9: 登録と精算 |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 9 では、登録と精算について説明します。
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: b3babf1d935774b0ef93d6ab02c8b295998f8afc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835616"
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="1d594-104">パート 9: 登録と精算</span><span class="sxs-lookup"><span data-stu-id="1d594-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="1d594-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="1d594-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="1d594-106">MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。</span><span class="sxs-lookup"><span data-stu-id="1d594-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="1d594-107">MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。</span><span class="sxs-lookup"><span data-stu-id="1d594-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="1d594-108">このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="1d594-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="1d594-109">パート 9 では、登録と精算について説明します。</span><span class="sxs-lookup"><span data-stu-id="1d594-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="1d594-110">このセクションでを作成します、CheckoutController 買い物客のアドレスと支払い情報を収集します。</span><span class="sxs-lookup"><span data-stu-id="1d594-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="1d594-111">ユーザーはこのコント ローラーが承認を要求するために、チェックインする前に、サイトを登録することが求められます。</span><span class="sxs-lookup"><span data-stu-id="1d594-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="1d594-112">ユーザーは、「チェック アウト」ボタンをクリックして、ショッピング カートからチェック アウト プロセスに移動されます。</span><span class="sxs-lookup"><span data-stu-id="1d594-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="1d594-113">ユーザーがログインしていない場合に要求されます。</span><span class="sxs-lookup"><span data-stu-id="1d594-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="1d594-114">ログインが成功すると、ユーザーに、アドレスと支払いのビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d594-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="1d594-115">フォームの入力し、は注文を送信するに表示されます、注文確認画面。</span><span class="sxs-lookup"><span data-stu-id="1d594-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="1d594-116">存在しない注文かに属していない注文を表示しようとすると、エラー ビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d594-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="1d594-117">ショッピング カートを移行します。</span><span class="sxs-lookup"><span data-stu-id="1d594-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="1d594-118">ショッピングのプロセスは、匿名がチェック アウト ボタンをクリックすると、登録する必要があり、ログインします。</span><span class="sxs-lookup"><span data-stu-id="1d594-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="1d594-119">ユーザーは、訪問に登録またはログインを完了すると、ショッピング カートの内容をユーザーに関連付ける必要があります。 間、ショッピング カート情報を維持することが期待されます。</span><span class="sxs-lookup"><span data-stu-id="1d594-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="1d594-120">これは、ShoppingCart クラスが既にユーザー名で現在のカート内のすべての項目を関連付けるメソッドを持つようには、実際には非常に単純です。</span><span class="sxs-lookup"><span data-stu-id="1d594-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="1d594-121">ユーザーが登録またはログインを完了すると、このメソッドを呼び出すだけ必要になります。</span><span class="sxs-lookup"><span data-stu-id="1d594-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="1d594-122">開く、 **AccountController**メンバーシップと承認を設定しているときに追加したクラス。</span><span class="sxs-lookup"><span data-stu-id="1d594-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="1d594-123">追加、次の MigrateShoppingCart メソッドを追加し、MvcMusicStore.Models を参照するステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d594-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="1d594-124">次に示すよう、ユーザーが確認されると、MigrateShoppingCart を呼び出すログオン後のアクションを次に、変更するには。</span><span class="sxs-lookup"><span data-stu-id="1d594-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="1d594-125">ユーザー アカウントが正常に作成された直後後にアクションを投稿レジスタに同じ変更を行います。</span><span class="sxs-lookup"><span data-stu-id="1d594-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="1d594-126">これでは、匿名ショッピング カートが自動的に登録に成功したか、ログイン時にユーザー アカウントに転送するようになりました。</span><span class="sxs-lookup"><span data-stu-id="1d594-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="1d594-127">CheckoutController を作成します。</span><span class="sxs-lookup"><span data-stu-id="1d594-127">Creating the CheckoutController</span></span>

<span data-ttu-id="1d594-128">Controllers フォルダーを右クリックし、空のコント ローラー テンプレートを使用して CheckoutController という名前のプロジェクトに新しいコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="1d594-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="1d594-129">最初に、ユーザーのチェック アウトする前に登録する必要がコント ローラーのクラス宣言の上の Authorize 属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="1d594-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="1d594-130">*注: これは以前、StoreManagerController に行われた変更に似ていますが、Authorize 属性のユーザーが管理者ロールであることが必要な場合。チェック アウト コント ローラーで要求しているユーザーがログインする管理者である必要はありませんが。*</span><span class="sxs-lookup"><span data-stu-id="1d594-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="1d594-131">わかりやすくは、私たちはこのチュートリアルでは支払い情報を扱うはありません。</span><span class="sxs-lookup"><span data-stu-id="1d594-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="1d594-132">代わりに、ユーザーにキャンペーン コードを使用して、チェック アウトできるようにしています。</span><span class="sxs-lookup"><span data-stu-id="1d594-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="1d594-133">プロモーションをという名前の定数を使用してこのプロモーション コードを保存します。</span><span class="sxs-lookup"><span data-stu-id="1d594-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="1d594-134">StoreController のように storeDB をという名前の MusicStoreEntities クラスのインスタンスを保持するフィールドを宣言します。</span><span class="sxs-lookup"><span data-stu-id="1d594-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="1d594-135">作成するには、MusicStoreEntities クラスの使用を使用して、追加する必要があります。 MvcMusicStore.Models 名前空間のステートメント。</span><span class="sxs-lookup"><span data-stu-id="1d594-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="1d594-136">チェック アウト コント ローラーの上部には、以下が表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d594-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="1d594-137">CheckoutController は、次のコント ローラー アクションが適用されます。</span><span class="sxs-lookup"><span data-stu-id="1d594-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="1d594-138">**AddressAndPayment (GET メソッド)** ユーザーの情報を入力するためのフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d594-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="1d594-139">**AddressAndPayment (POST メソッド)** 入力を検証し、注文を処理します。</span><span class="sxs-lookup"><span data-stu-id="1d594-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="1d594-140">**完全な**清算処理をユーザーが正常に終了した後に表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d594-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="1d594-141">このビューは、確認として、ユーザーの注文番号に含まれます。</span><span class="sxs-lookup"><span data-stu-id="1d594-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="1d594-142">まず、AddressAndPayment にしましょう (これは、コント ローラーを作成したときに生成された) インデックス コント ローラー アクションの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="1d594-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="1d594-143">このコント ローラー アクションでは、モデル情報は必要ありませんので、チェック アウト フォームだけ表示します。</span><span class="sxs-lookup"><span data-stu-id="1d594-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="1d594-144">AddressAndPayment POST メソッドは、同じパターンに従って、StoreManagerController で使用: これは、フォームの送信に同意し、注文を完了しようし、失敗した場合、フォームが再表示します。</span><span class="sxs-lookup"><span data-stu-id="1d594-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="1d594-145">注文の検証要件を満たしているフォームの入力の検証後、直接プロモーションのフォーム値がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="1d594-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="1d594-146">すべてが正しい順序で更新された情報を保存しますと仮定すると、注文プロセスを完了し、完全なアクションにリダイレクト ShoppingCart オブジェクトに伝えます。</span><span class="sxs-lookup"><span data-stu-id="1d594-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="1d594-147">チェック アウト プロセスの正常に完了したら、ユーザーは、完全なコント ローラー アクションにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="1d594-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="1d594-148">このアクションでは、順序が確認メッセージとして注文番号を表示する前に実際にログインしたユーザーに属しているはことを検証する簡単なチェックを実行します。</span><span class="sxs-lookup"><span data-stu-id="1d594-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="1d594-149">*注: エラー ビューが自動的に作成お/Views/Shared フォルダーにプロジェクトを開始したとき。*</span><span class="sxs-lookup"><span data-stu-id="1d594-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="1d594-150">完全な CheckoutController コードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="1d594-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="1d594-151">AddressAndPayment ビューの追加</span><span class="sxs-lookup"><span data-stu-id="1d594-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="1d594-152">ここで、AddressAndPayment ビューを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="1d594-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="1d594-153">AddressAndPayment コント ローラーのアクションのいずれかを右クリックし、次に示すように注文として厳密に型指定し、テンプレートの編集を使用して AddressAndPayment をという名前のビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="1d594-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="1d594-154">このビューにより、2 つのこれまで見てきた StoreManagerEdit ビューの作成中に手法の使用します。</span><span class="sxs-lookup"><span data-stu-id="1d594-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="1d594-155">Html.EditorForModel() 順序モデルのフォーム フィールドの表示を使用して、</span><span class="sxs-lookup"><span data-stu-id="1d594-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="1d594-156">検証属性を持つ、Order クラスを使用して検証規則を使用します</span><span class="sxs-lookup"><span data-stu-id="1d594-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="1d594-157">まず、Html.EditorForModel()、プロモーション コードの後に、追加のテキスト ボックスを使用するフォームのコードを更新します。</span><span class="sxs-lookup"><span data-stu-id="1d594-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="1d594-158">AddressAndPayment ビューの完全なコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="1d594-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="1d594-159">注文の検証規則を定義します。</span><span class="sxs-lookup"><span data-stu-id="1d594-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="1d594-160">できたので、ビューを設定すると、私たちは検証ルールをモデルの設定、順序、アルバムのモデルの以前に行ったよう。</span><span class="sxs-lookup"><span data-stu-id="1d594-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="1d594-161">Models フォルダーを右クリックし、注文をという名前のクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="1d594-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="1d594-162">アルバム用に以前使用した検証属性、だけでなくをも使用正規ユーザーの電子メール アドレスを検証します。</span><span class="sxs-lookup"><span data-stu-id="1d594-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="1d594-163">しようとしてが欠落しているフォームを送信するか、無効な情報がクライアント側の検証を使用してエラー メッセージを表示するようになりました。</span><span class="sxs-lookup"><span data-stu-id="1d594-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="1d594-164">さて、チェック アウト プロセスの困難な作業のほとんど完了しました[完了] に、いくつかのオッズ and 端があるだけです。</span><span class="sxs-lookup"><span data-stu-id="1d594-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="1d594-165">2 つの単純なビューを追加する必要があり、ログイン プロセス中にカートの内容のハンドオフに対処する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d594-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="1d594-166">チェック アウト完了ビューの追加</span><span class="sxs-lookup"><span data-stu-id="1d594-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="1d594-167">だけ、順序の ID を表示する必要がある、チェック アウトの完全なビューは非常に単純で、</span><span class="sxs-lookup"><span data-stu-id="1d594-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="1d594-168">完全なコント ローラーのアクションを右クリックし、int として厳密に型指定された完了という名前のビューの追加</span><span class="sxs-lookup"><span data-stu-id="1d594-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="1d594-169">これで次に示すよう、注文 ID を表示するコードの表示を更新します。</span><span class="sxs-lookup"><span data-stu-id="1d594-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="1d594-170">エラー ビューを更新しています</span><span class="sxs-lookup"><span data-stu-id="1d594-170">Updating The Error view</span></span>

<span data-ttu-id="1d594-171">既定のテンプレートにはが再利用できる別の場所、サイトのように、共有ビュー フォルダーでビュー エラーにはが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1d594-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="1d594-172">このエラーの表示では、非常に単純なエラーを含んでいるし、レイアウト、私たちのサイトを使用してしないため、更新します。</span><span class="sxs-lookup"><span data-stu-id="1d594-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="1d594-173">これは、一般的なエラー ページであるため、コンテンツは非常に単純です。</span><span class="sxs-lookup"><span data-stu-id="1d594-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="1d594-174">ユーザーがそれらのアクションをもう一度お試したい場合は、履歴に前のページに移動するには、メッセージとのリンクを含めます。</span><span class="sxs-lookup"><span data-stu-id="1d594-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> <span data-ttu-id="1d594-175">[前へ](mvc-music-store-part-8.md)
> [次へ](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="1d594-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
