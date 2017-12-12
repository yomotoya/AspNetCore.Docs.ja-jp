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
<a name="part-9-registration-and-checkout"></a>手順 9: 登録とチェック アウト
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。  
>   
> MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。  
>   
> このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 一部 9 では、登録およびチェック アウトを説明します。


このセクションでお今後を作成する買い物のアドレスと支払い情報を収集する CheckoutController です。 ユーザーこのコント ローラーが承認を必要とするようにチェック アウトするの前に、サイトを登録することが求められます。

ユーザーは、「チェック アウト」ボタンをクリックしてショッピング カートに入ってからチェック アウト プロセスに移動されます。

![](mvc-music-store-part-9/_static/image1.jpg)

ユーザーがログインしていない場合に求められます。

![](mvc-music-store-part-9/_static/image1.png)

正常にログイン、ユーザー、アドレスと支払ビュー、表示されます。

![](mvc-music-store-part-9/_static/image2.png)

フォームをいっぱいになり、注文を送信するが後に、表示されます、注文確認画面。

![](mvc-music-store-part-9/_static/image3.png)

存在しない順序かに属していない注文を表示しようとすると、エラー ビューが表示されます。

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>ショッピング カートを移行します。

ショッピング プロセスが、匿名ユーザーがチェック アウト ボタンをクリックすると、ときに、登録する必要がありますとログインします。 ユーザーは、登録またはログインを完了すると、ユーザーに、ショッピング カート情報を関連付ける必要がありますが、間、ショッピング カート情報を維持するを必要とします。

これは実際には、単純な ShoppingCart クラスには既にユーザー名で現在カート内のすべての項目を関連付けるメソッド。 ユーザーが登録またはログインを完了すると、このメソッドを呼び出す必要がありますがだけです。

開く、 **AccountController**おメンバーシップと承認を設定しているときに追加したクラスです。 追加、次の MigrateShoppingCart メソッドを追加し、MvcMusicStore.Models を参照するステートメントを使用します。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

次のように、ユーザーが確認されると、MigrateShoppingCart を呼び出すログオン後のアクションを次に、変更するには。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

ユーザー アカウントが正常に作成されるとすぐにアクションを投稿レジスタに同じ変更を行います。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

です - 匿名のショッピング カートは自動的に登録に成功したか、ログイン時にユーザー アカウントに転送するようになりました。

## <a name="creating-the-checkoutcontroller"></a>CheckoutController を作成します。

コント ローラーのフォルダーを右クリックし、新しいコント ローラーを空のコント ローラーのテンプレートを使用して CheckoutController をという名前のプロジェクトに追加します。

![](mvc-music-store-part-9/_static/image5.png)

、、コント ローラー クラスの宣言の上にチェック アウトする前に登録するユーザーに要求する承認属性を追加します。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*注: これは以前に行った、StoreManagerController への変更に似ていますが、Authorize attribute のユーザーが管理者ロールであることが必要な場合は。チェック アウト コント ローラーでおいるを必要とするユーザーがログインする管理者であることが必要なされません。*

わかりやすくするためは、おはこのチュートリアルでは支払情報を扱うはありません。 代わりに、ユーザーがチェック アウト プロモーション コードを使用できるようにしています。 PromoCode を名前付き定数を使用してこのプロモーション コードを格納します。

StoreController と同様に、storeDB をという名前の MusicStoreEntities クラスのインスタンスを保持するためのフィールドを宣言します。 作成するために MusicStoreEntities クラスを使用して、使用して、追加する必要があります。 MvcMusicStore.Models 名前空間のステートメント。 下のチェック アウト コント ローラーの上部に表示されます。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

CheckoutController は、次のコント ローラーのアクションが適用されます。

**AddressAndPayment (GET メソッド)**ユーザーに、それらの情報の入力を許可するフォームが表示されます。

**AddressAndPayment (POST メソッド)**入力を検証し、注文を処理します。

**完全な**ユーザーには、チェック アウト処理が正常に完了した後に表示されます。 このビューは、確認として、ユーザーの注文番号に含まれます。

まず、みましょう AddressAndPayment する (これは、コント ローラーを作成したときに生成された) インデックス コント ローラー アクションの名前を変更します。 このコント ローラーのアクションでは、モデル情報をする必要がないため、チェック アウト形式であるだけ表示します。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

当社 AddressAndPayment POST メソッドは、同じパターンに従う、StoreManagerController で使用: フォームの送信に同意し、注文を完了しようして失敗した場合は、フォームが表示されます再です。

フォームの入力を検証する、検証要件を満たしている注文に対して後直接 PromoCode フォーム値が確認されます。 すべてが正しい順序で更新された情報を保存しますと仮定した場合、注文プロセスを完了し、完全なアクションにリダイレクト ShoppingCart オブジェクトに伝えます。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

チェック アウト プロセスの正常完了時に、ユーザーは、完全なコント ローラー アクションにリダイレクトされます。 この操作では、順序が、確認メッセージと注文番号を表示する前に実際にログインしたユーザーに属しているはことを検証する簡単なチェックを実行します。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*注: エラー ビュー自動的に作成された us/Views/Shared フォルダーにプロジェクトを開始したとき。*

CheckoutController の完全なコードは次のとおりです。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>AddressAndPayment ビューの追加

ここで、AddressAndPayment ビューを作成してみましょう。 1 つを右クリックし、AddressAndPayment コント ローラーのアクションを次に示すように順序として厳密に型指定し、テンプレートの編集を使用している AddressAndPayment をという名前のビューを追加します。

![](mvc-music-store-part-9/_static/image6.png)

このビューにより、2 つの StoreManagerEdit ビューの作成中に見てお手法の使用します。

- Html.EditorForModel() を使用して、注文のモデルのフォーム フィールドを表示するには
- 検証属性を持つ、Order クラスを使用する検証規則を活用し、

まず、Html.EditorForModel()、プロモーション コードの後に追加のテキスト ボックスを使用するフォームのコードを更新します。 AddressAndPayment ビューの完全なコードは、以下に示します。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>注文の検証規則を定義します。

これで、ビューの設定おは検証ルールをモデルの設定、順序以前と同様アルバムのモデルの。 モデル フォルダーを右クリックし、注文をという名前のクラスを追加します。 属性に加えて、検証、アルバムの以前を使用したも使用する正規表現、ユーザーの電子メール アドレスを検証します。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

しようとしてが欠落しているフォームを送信または、無効な情報がクライアント側の検証を使用してエラー メッセージが表示されます。

![](mvc-music-store-part-9/_static/image7.png)

これで、チェック アウト プロセスの面倒な作業のほとんどに実行しました。[完了] に、いくつかのオッズ and 端があるだけです。 2 つの単純なビューを追加する必要があり、ハンドオフ カート情報、ログイン プロセス中に対処する必要があります。

## <a name="adding-the-checkout-complete-view"></a>チェック アウトの完全なビューを追加します。

注文の ID を表示するだけで済みますチェック アウトの完全なビューが非常に単純ですが、 完全なコント ローラーのアクションを右クリックし、int として厳密に型指定の完了をという名前のビューの追加

![](mvc-music-store-part-9/_static/image8.png)

これで次のように、注文 ID を表示するコードの表示を更新します。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>エラー ビューを更新

既定のテンプレートにはが再利用できる他の場所、サイトのように、共有ビュー フォルダーに、エラー ビューが含まれます。 このエラー ビューは、非常に単純なエラーが含まれ、それが更新されますので、サイトのレイアウトを使用しません。

これは汎用エラー ページであるため、コンテンツは非常に単純です。 ユーザーがそのアクションを再試行する場合は、履歴に前のページに移動するには、メッセージとのリンクを含めます。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


>[!div class="step-by-step"]
[前へ](mvc-music-store-part-8.md)
[次へ](mvc-music-store-part-10.md)
