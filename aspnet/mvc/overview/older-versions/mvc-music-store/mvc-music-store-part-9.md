---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'パート 9: 登録と精算 |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 9 では、登録と精算について説明します。
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e521e30cb820d834d57c87538b869861a536657b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812874"
---
<a name="part-9-registration-and-checkout"></a>パート 9: 登録と精算
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。  
>   
> MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。  
>   
> このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 9 では、登録と精算について説明します。


このセクションでを作成します、CheckoutController 買い物客のアドレスと支払い情報を収集します。 ユーザーはこのコント ローラーが承認を要求するために、チェックインする前に、サイトを登録することが求められます。

ユーザーは、「チェック アウト」ボタンをクリックして、ショッピング カートからチェック アウト プロセスに移動されます。

![](mvc-music-store-part-9/_static/image1.jpg)

ユーザーがログインしていない場合に要求されます。

![](mvc-music-store-part-9/_static/image1.png)

ログインが成功すると、ユーザーに、アドレスと支払いのビューが表示されます。

![](mvc-music-store-part-9/_static/image2.png)

フォームの入力し、は注文を送信するに表示されます、注文確認画面。

![](mvc-music-store-part-9/_static/image3.png)

存在しない注文かに属していない注文を表示しようとすると、エラー ビューが表示されます。

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>ショッピング カートを移行します。

ショッピングのプロセスは、匿名がチェック アウト ボタンをクリックすると、登録する必要があり、ログインします。 ユーザーは、訪問に登録またはログインを完了すると、ショッピング カートの内容をユーザーに関連付ける必要があります。 間、ショッピング カート情報を維持することが期待されます。

これは、ShoppingCart クラスが既にユーザー名で現在のカート内のすべての項目を関連付けるメソッドを持つようには、実際には非常に単純です。 ユーザーが登録またはログインを完了すると、このメソッドを呼び出すだけ必要になります。

開く、 **AccountController**メンバーシップと承認を設定しているときに追加したクラス。 追加、次の MigrateShoppingCart メソッドを追加し、MvcMusicStore.Models を参照するステートメントを使用します。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

次に示すよう、ユーザーが確認されると、MigrateShoppingCart を呼び出すログオン後のアクションを次に、変更するには。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

ユーザー アカウントが正常に作成された直後後にアクションを投稿レジスタに同じ変更を行います。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

これでは、匿名ショッピング カートが自動的に登録に成功したか、ログイン時にユーザー アカウントに転送するようになりました。

## <a name="creating-the-checkoutcontroller"></a>CheckoutController を作成します。

Controllers フォルダーを右クリックし、空のコント ローラー テンプレートを使用して CheckoutController という名前のプロジェクトに新しいコント ローラーを追加します。

![](mvc-music-store-part-9/_static/image5.png)

最初に、ユーザーのチェック アウトする前に登録する必要がコント ローラーのクラス宣言の上の Authorize 属性を追加します。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*注: これは以前、StoreManagerController に行われた変更に似ていますが、Authorize 属性のユーザーが管理者ロールであることが必要な場合。チェック アウト コント ローラーで要求しているユーザーがログインする管理者である必要はありませんが。*

わかりやすくは、私たちはこのチュートリアルでは支払い情報を扱うはありません。 代わりに、ユーザーにキャンペーン コードを使用して、チェック アウトできるようにしています。 プロモーションをという名前の定数を使用してこのプロモーション コードを保存します。

StoreController のように storeDB をという名前の MusicStoreEntities クラスのインスタンスを保持するフィールドを宣言します。 作成するには、MusicStoreEntities クラスの使用を使用して、追加する必要があります。 MvcMusicStore.Models 名前空間のステートメント。 チェック アウト コント ローラーの上部には、以下が表示されます。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

CheckoutController は、次のコント ローラー アクションが適用されます。

**AddressAndPayment (GET メソッド)** ユーザーの情報を入力するためのフォームが表示されます。

**AddressAndPayment (POST メソッド)** 入力を検証し、注文を処理します。

**完全な**清算処理をユーザーが正常に終了した後に表示されます。 このビューは、確認として、ユーザーの注文番号に含まれます。

まず、AddressAndPayment にしましょう (これは、コント ローラーを作成したときに生成された) インデックス コント ローラー アクションの名前を変更します。 このコント ローラー アクションでは、モデル情報は必要ありませんので、チェック アウト フォームだけ表示します。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

AddressAndPayment POST メソッドは、同じパターンに従って、StoreManagerController で使用: これは、フォームの送信に同意し、注文を完了しようし、失敗した場合、フォームが再表示します。

注文の検証要件を満たしているフォームの入力の検証後、直接プロモーションのフォーム値がチェックされます。 すべてが正しい順序で更新された情報を保存しますと仮定すると、注文プロセスを完了し、完全なアクションにリダイレクト ShoppingCart オブジェクトに伝えます。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

チェック アウト プロセスの正常に完了したら、ユーザーは、完全なコント ローラー アクションにリダイレクトされます。 このアクションでは、順序が確認メッセージとして注文番号を表示する前に実際にログインしたユーザーに属しているはことを検証する簡単なチェックを実行します。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*注: エラー ビューが自動的に作成お/Views/Shared フォルダーにプロジェクトを開始したとき。*

完全な CheckoutController コードは次のとおりです。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>AddressAndPayment ビューの追加

ここで、AddressAndPayment ビューを作成してみましょう。 AddressAndPayment コント ローラーのアクションのいずれかを右クリックし、次に示すように注文として厳密に型指定し、テンプレートの編集を使用して AddressAndPayment をという名前のビューを追加します。

![](mvc-music-store-part-9/_static/image6.png)

このビューにより、2 つのこれまで見てきた StoreManagerEdit ビューの作成中に手法の使用します。

- Html.EditorForModel() 順序モデルのフォーム フィールドの表示を使用して、
- 検証属性を持つ、Order クラスを使用して検証規則を使用します

まず、Html.EditorForModel()、プロモーション コードの後に、追加のテキスト ボックスを使用するフォームのコードを更新します。 AddressAndPayment ビューの完全なコードは、以下に示します。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>注文の検証規則を定義します。

できたので、ビューを設定すると、私たちは検証ルールをモデルの設定、順序、アルバムのモデルの以前に行ったよう。 Models フォルダーを右クリックし、注文をという名前のクラスを追加します。 アルバム用に以前使用した検証属性、だけでなくをも使用正規ユーザーの電子メール アドレスを検証します。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

しようとしてが欠落しているフォームを送信するか、無効な情報がクライアント側の検証を使用してエラー メッセージを表示するようになりました。

![](mvc-music-store-part-9/_static/image7.png)

さて、チェック アウト プロセスの困難な作業のほとんど完了しました[完了] に、いくつかのオッズ and 端があるだけです。 2 つの単純なビューを追加する必要があり、ログイン プロセス中にカートの内容のハンドオフに対処する必要があります。

## <a name="adding-the-checkout-complete-view"></a>チェック アウト完了ビューの追加

だけ、順序の ID を表示する必要がある、チェック アウトの完全なビューは非常に単純で、 完全なコント ローラーのアクションを右クリックし、int として厳密に型指定された完了という名前のビューの追加

![](mvc-music-store-part-9/_static/image8.png)

これで次に示すよう、注文 ID を表示するコードの表示を更新します。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>エラー ビューを更新しています

既定のテンプレートにはが再利用できる別の場所、サイトのように、共有ビュー フォルダーでビュー エラーにはが含まれます。 このエラーの表示では、非常に単純なエラーを含んでいるし、レイアウト、私たちのサイトを使用してしないため、更新します。

これは、一般的なエラー ページであるため、コンテンツは非常に単純です。 ユーザーがそれらのアクションをもう一度お試したい場合は、履歴に前のページに移動するには、メッセージとのリンクを含めます。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [前へ](mvc-music-store-part-8.md)
> [次へ](mvc-music-store-part-10.md)
