---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'パート 8: ショッピング カートと Ajax 更新 |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 8 では、ショッピング カートと Ajax 更新について説明します。
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: cab338e56505c453532a26d794eb7bf4e94555a9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832327"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>パート 8: ショッピング カートと Ajax 更新
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。  
>   
> MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。  
>   
> このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 8 では、ショッピング カートと Ajax 更新について説明します。


ユーザー登録を行わない、カートにアルバムを配置する を許可しますが、完全なチェック アウトをゲストとして登録する必要があります。 買い物とチェック アウト プロセスが 2 つのコント ローラーに区切られる: 匿名で、カートにアイテムを追加できるように、ShoppingCart コント ローラーとチェック アウト プロセスを処理するチェック アウト コント ローラー。 このセクションでショッピング カートの開始をし、次のセクションで、清算処理をビルドします。

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>カート、順序、および OrderDetail モデル クラスを追加します。

ショッピング カートとチェック アウト プロセスにより、いくつかの新しいクラスを使用します。 Models フォルダーを右クリックし、カート クラス (Cart.cs) を次のコードを追加します。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

このクラスは、レコード Id プロパティの [キー] 属性を除く、これまでに使用した他のユーザーに非常に似ています。 カートのアイテムは、匿名ショッピングを許可する CartID という名前の文字列識別子がテーブルにレコード Id を名前付き整数型のプライマリ キーが含まれています。 慣例により、Entity Framework Code First には、ことカートをという名前のテーブルの主キーは CartId または ID のいずれかになりますが簡単にオーバーライドできますを注釈またはコードを使用して必要な場合が想定しています。 使用する方法、シンプルな規約で Entity Framework Code First に合ったときの例は、これが私たちを受けなくが不要になったとき。

次に、次のコードで Order クラス (Order.cs) を追加します。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

このクラスは、注文の概要と配信の情報を追跡します。 **まだコンパイルされません**まだ作成していないクラスに依存する OrderDetails ナビゲーション プロパティを持つため、します。 今すぐ追加することでクラスという名前の OrderDetail.cs、次のコードを追加することを修正しましょう。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

これら新しいモデルなどのクラスも、DbSet を公開する Dbset を含める MusicStoreEntities クラスに 1 つの最後の更新を行います&lt;アーティスト&gt;します。 更新された MusicStoreEntities クラスとして表示されます以下です。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>ショッピング カートのビジネス ロジックを管理します。

次に、Models フォルダーに ShoppingCart クラスを作成します。 ShoppingCart モデルでは、カート テーブルにデータ アクセスを処理します。 さらに、追加すると、ショッピング カートから項目を削除するビジネス ロジックを処理します。

一時的な一意の識別子 (GUID、またはグローバルに一意の識別子を使用して) ユーザーを割り当てることが買い物かごにアイテムを追加するためだけのアカウントにサインアップするユーザーを必要とするのでない、ショッピング カートにアクセスするとき。 ASP.NET セッション クラスを使用して、この ID を格納します。

*注: ASP.NET セッションは、サイトを離れた後の有効期限をユーザーに固有の情報を格納する便利な場所です。セッション状態の不正使用では、大きいサイトのパフォーマンスに影響を持つことができます、明るい使用はデモンストレーションのためにも機能します。*

ShoppingCart クラスは、次のメソッドを公開します。

**AddToCart**アルバムをパラメーターとして受け取り、ユーザーのカートに追加します。 カートの表では、各アルバムの数量を追跡しているために、必要な場合は、新しい行を作成またはだけ場合は、ユーザーが既に 1 つのコピーのアルバムを注文数量をインクリメントするためのロジックが含まれます。

**RemoveFromCart**アルバムの ID を受け取り、ユーザーのカートから削除されます。 ユーザーは、カゴにアルバムのコピーを 1 つのみがある、行が削除されます。

**EmptyCart**ユーザーのショッピング カートからすべての項目を削除します。

**GetCartItems** CartItems の表示または処理するための一覧を取得します。

**GetCount**を取得するユーザーがショッピング カートに入っているアルバムの合計数。

**GetTotal**カート内のすべての項目の合計コストを計算します。

**CreateOrder**ショッピング カートをチェック アウト フェーズ中に、注文に変換します。

**GetCart**カート オブジェクトを取得するコント ローラーを使用する静的メソッドです。 使用して、 **GetCartId**ユーザーのセッションから、CartId の読み取りを処理するメソッド。 GetCartId メソッドでは、ユーザーの CartId ユーザーのセッションから読み取ることができるように、HttpContextBase が必要です。

ここでは、完全な**ShoppingCart クラス**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

ショッピング カート コント ローラーは、複雑な情報をビュー モデル オブジェクトに正しくマップされないと通信する必要があります。 ありません。 ビューに合わせて、モデルを変更します。モデル クラスには、ユーザー インターフェイスではなく、ドメインを表す必要があります。 ストア マネージャー、ボックスの一覧についてで行ったように、管理が困難で取得 ViewBag を使用して、多くの情報を渡しますが、ViewBag クラスを使用して、ビューに、情報を渡す 1 つのソリューションになります。

これに対する解決策は、使用する、 *ViewModel*パターン。 このパターンを使用する場合、特定のビューのシナリオ用に最適化されたと、ビュー テンプレートで必要な動的値/コンテンツ プロパティを公開する厳密に型指定されたクラスを作成します。 コント ローラー クラスを設定し、使用するには、このビュー テンプレートにこれらのビューに最適化されたクラスを渡します。 これにより、タイプ セーフ、コンパイル時のチェック、およびエディター IntelliSense 内でのテンプレートを表示できます。

ショッピング カート コント ローラーで使用するための 2 つのビュー モデルを作成します。 ShoppingCartViewModel はユーザーのショッピング カートの内容を保持し、ユーザーが何かを削除すると確認情報の表示に使用される、ShoppingCartRemoveViewModelカート。

わかりやすくするために、プロジェクトのルートでは、新しいビューモデル フォルダーを作成しましょう。 プロジェクトを右クリックし、追加の選択/新規フォルダー。

![](mvc-music-store-part-8/_static/image1.jpg)

ビューモデル フォルダーの名前を付けます。

![](mvc-music-store-part-8/_static/image1.png)

次に、ビューモデル フォルダーで ShoppingCartViewModel クラスを追加します。 2 つのプロパティがあります: カートのアイテムと、カート内のすべての項目の合計価格を保持するために 10 進値の一覧。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

これで、ShoppingCartRemoveViewModel をビューモデル フォルダーに次の 4 つのプロパティに追加します。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>買い物カ ゴ コント ローラー

ショッピング カートのコント ローラーは、次の 3 つの主な目的を持つ: カートから項目を削除する、カートにアイテムを表示する、カートにアイテムを追加します。 行う 3 つのクラスの使用先ほど作成した: ShoppingCartViewModel、ShoppingCartRemoveViewModel、および ShoppingCart します。 ように、StoreController と StoreManagerController MusicStoreEntities のインスタンスを保持するフィールドを追加します。

空のコント ローラー テンプレートを使用してプロジェクトに新しいショッピング カートのコント ローラーを追加します。

![](mvc-music-store-part-8/_static/image2.png)

完全な ShoppingCart コント ローラーを次に示します。 インデックスとコント ローラーの追加のアクションの使用率は見覚えがあります。 削除と CartSummary コント ローラー アクションは、次のセクションで後ほど説明しますが、2 つの特殊なケースを処理します。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>JQuery で Ajax の更新

次に、ShoppingCartViewModel を厳密に型指定し、同じ方法を使用して、リスト ビュー テンプレートを使用するショッピング カートのインデックス ページを作成します。

![](mvc-music-store-part-8/_static/image3.png)

ただし、カートから項目を削除するのに、Html.ActionLink を使用する代わりには、HTML クラス RemoveLink がこのビュー内のすべてのリンクのクリック イベントを「結び付ける」jQuery を使用します。 フォームの送信ではなくこの click イベント ハンドラーだけ、AJAX コールバックを RemoveFromCart コント ローラー アクションになります。 JSON のシリアル化された結果を返す、RemoveFromCart、jQuery のコールバックは、次を解析し、jQuery を使用してページに 4 つの簡単な更新プログラムを実行します。

- 1. 一覧から削除したアルバムを削除します。
- 2. ヘッダーのカート数を更新します。
- 3. 更新メッセージをユーザーに表示します。
- 4. カートの総額を更新します。

削除のシナリオは、インデックス ビュー内で、Ajax コールバックが処理される、ため RemoveFromCart アクションの追加のビューは不要です。 /ShoppingCart/Index ビューの完全なコードを次に示します。

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

このアプリケーションをテストするためには、このショッピング カートにアイテムを追加できる必要があります。 更新、**ストアの詳細**をビューに含め、"カートに追加 ボタンをクリックします。 いくつかのアルバムの追加情報が追加されましたが含めることができますに思いますが、このビューを最後に更新しましたので: ジャンル、アーティスト、価格、およびアルバム アート。 更新されたストアの詳細ビューのコードは、次に示すように表示されます。

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

今すぐクリックして、ストアを追加して、アルバムを削除して、ショッピング カートからテストできます。 アプリケーションを実行し、ストアのインデックスを参照します。

![](mvc-music-store-part-8/_static/image4.png)

次に、アルバムの一覧を表示するジャンルをクリックします。

![](mvc-music-store-part-8/_static/image5.png)

アルバム タイトルに今すぐクリックしてには、"カートに追加 ボタンを含む、更新されたアルバムの詳細ビューが表示されます。

![](mvc-music-store-part-8/_static/image6.png)

カートに追加"ボタンをクリックすると、ショッピング カートの一覧で、ショッピング カートのインデックス ビューが表示されます。

![](mvc-music-store-part-8/_static/image7.png)

ショッピング カートを読み込んだら、Ajax の更新、ショッピング カートを表示する、カートのリンクから削除 をクリックできます。

![](mvc-music-store-part-8/_static/image8.png)

ショッピング カートのアイテムをカートに追加する登録されていないユーザーが作業を構築しました。 次のセクションで登録し、チェック アウト プロセスを完了することができます。


> [!div class="step-by-step"]
> [前へ](mvc-music-store-part-7.md)
> [次へ](mvc-music-store-part-9.md)
