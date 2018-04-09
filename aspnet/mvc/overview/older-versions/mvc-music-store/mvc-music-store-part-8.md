---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: '手順 8: ショッピング カート Ajax の更新と |Microsoft ドキュメント'
author: jongalloway
description: このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 8 の一部では、Ajax の更新でショッピング カートについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>手順 8: ショッピング カートの Ajax の更新
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。  
>   
> MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。  
>   
> このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 8 の一部では、Ajax の更新でショッピング カートについて説明します。


登録、せずカゴにアルバムを配置するユーザーを使用するをおは完全なチェック アウトをゲストとして登録する必要があります。 2 つのコント ローラーに、ショッピングおよびチェック アウト プロセスが区切られる: 匿名で、カートにアイテムを追加できるようにする ShoppingCart コント ローラーとチェック アウト プロセスを処理するチェック アウト コント ローラー。 このセクションのショッピング カートで始まるその後、次のセクションで、チェック アウト処理をビルドします。

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>カート、順序、および OrderDetail モデル クラスを追加します。

ショッピング カートおよび支払いプロセスにより、いくつかの新しいクラスを使用します。 モデル フォルダーを右クリックし、次のコードを持つカート クラス (Cart.cs) を追加します。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

このクラスは、RecordId プロパティの [キー] 属性を除くをこれまで使用して他のユーザーに非常に似ています。 買い物カゴの品目が匿名買い物を許可する CartID をという名前の文字列識別子がテーブルには、RecordId をという名前の整数主キーが含まれています。 慣例により、Entity Framework コードで最初には、ことカートをという名前のテーブルの主キーは CartId または ID のいずれかになりますが簡単にオーバーライドできますを注釈またはコードを使用してたい場合が期待しています。 これを使用する方法、単純な規則で Entity Framework コード優先 us, に合わせて、場合の例ですがおいるによって制約されないことがない場合にあります。

次に、次のコードで Order クラス (Order.cs) を追加します。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

このクラスは、注文の概要および配信の情報を追跡します。 **これはまだコンパイルされません**おをまだ作成していないクラスに依存する OrderDetails ナビゲーション プロパティがあるため、します。 今すぐ追加することで、クラスという名前の OrderDetail.cs、次のコードを追加することを修正してみましょう。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

最後の 1 つの更新に DbSet も含む、新しいそれらモデル クラスを公開する DbSets を含める MusicStoreEntities クラスを作成します&lt;アーティスト&gt;です。 更新された MusicStoreEntities クラスとして表示されます以下です。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>ショッピング カートのビジネス ロジックを管理します。

次に、Models フォルダー ShoppingCart クラスが作成されます。 ShoppingCart モデルでは、買い物カゴ テーブルへのデータ アクセスを処理します。 さらに、追加すると、ショッピング カートから項目を削除するビジネス ロジックを処理します。

ショッピング カートにアイテムを追加するだけアカウントにサインアップするユーザーに要求するのでない、割り当てることで、ユーザー、一時的な一意の識別子 (GUID、またはグローバルに一意の識別子を使用)、ショッピング カートをアクセスしたときにします。 ASP.NET セッション クラスを使用してこの ID を格納します。

*メモ: ASP.NET セッションは、サイトに移動して、有効期限が切れるユーザー固有の情報を格納する便利な場所です。セッション状態の誤用はパフォーマンスに影響を持つ大規模なサイトで、ライト使用はデモンストレーションのためにも機能します。*

ShoppingCart クラスは、次のメソッドを公開します。

**AddToCart**アルバムをパラメーターとしては、ユーザーのカートに追加します。 カート テーブルは、各アルバムの数量を追跡しているために、必要な場合は、新しい行を作成またはだけ場合は、ユーザーが既に 1 つはアルバムのコピーを注文数量をインクリメントするためのロジックが含まれます。

**RemoveFromCart**アルバムの ID を受け取り、ユーザーのカートからは削除されます。 ユーザーには、カートのアルバムのコピーを 1 つしかありませんでしたがある、行が削除されます。

**EmptyCart**ユーザーのショッピング カートからすべての項目を削除します。

**GetCartItems** CartItems の表示や処理用の一覧を取得します。

**GetCount**を取得するユーザーがショッピング カートに入っているアルバムの合計数。

**GetTotal**カート内のすべての項目の総コストを計算します。

**CreateOrder**順にチェック アウト フェーズ中に、ショッピング カートを変換します。

**GetCart**カート オブジェクトを取得する、コント ローラーを使用する静的メソッドです。 使用して、 **GetCartId**ユーザーのセッションから、CartId の読み取りを処理するメソッド。 GetCartId メソッドでは、ユーザーのセッションからユーザーの CartId が読み取ることができるように、HttpContextBase が必要です。

ここでは、完全な**ShoppingCart クラス**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

買い物カ ゴ コント ローラーは、いくつか複雑な情報をそのビューに、モデル オブジェクトに完全に対応していないを通信する必要があります。 このビューに合わせて、モデルを変更したくありません。モデルのクラスには、ユーザー インターフェイスではなく、ドメインを表す必要があります。 ストア マネージャー ドロップダウンについてでは、使用しましたが、管理が困難で取得 ViewBag 経由で大量の情報を渡す ViewBag クラスを使用して、ビューに、情報を渡す 1 つのソリューションになります。

この解決方法は、使用する、 *ViewModel*パターン。 このパターンを使用する場合、特定のビューのシナリオ用に最適化されていると、テンプレートの表示に必要な動的な値/コンテンツのプロパティを公開する厳密に型指定されたクラスを作成します。 コント ローラー クラスでは設定し、ビュー用に最適化されたこれらのクラスを使用するには、このビュー テンプレートに渡すことができます。 これにより、タイプ セーフ、コンパイル時のチェック、および IntelliSense ビュー テンプレート内のエディターです。

2 つのビュー モデルを使用して、ショッピング カートのコント ローラーに作成します。 ShoppingCartViewModel がユーザーのショッピング カートの内容を保持して、ユーザーが何かを削除すると、確認情報を表示に使用される、ShoppingCartRemoveViewModelカートです。

わかりやすくするために、プロジェクトのルートには、新しい ViewModels フォルダーを作成してみましょう。 プロジェクトを右クリックし、追加 を選択/新規フォルダーです。

![](mvc-music-store-part-8/_static/image1.jpg)

ViewModels フォルダーの名前を指定します。

![](mvc-music-store-part-8/_static/image1.png)

次に、ViewModels フォルダーに ShoppingCartViewModel クラスを追加します。 2 つのプロパティがある: カート項目、およびすべてのアイテムの合計金額をカートに保持するために 10 進値の一覧です。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

次の 4 つのプロパティで、ViewModels フォルダーに、ShoppingCartRemoveViewModel を追加します。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>買い物カ ゴ コント ローラー

ショッピング カート コント ローラーが次の 3 つの主な目的: カートから項目を削除する、カートにアイテムを表示するアイテムをカートに追加します。 これは、3 つのクラスを使用してため先ほど作成した: ShoppingCartViewModel、ShoppingCartRemoveViewModel、および ShoppingCart です。 StoreController、StoreManagerController、MusicStoreEntities のインスタンスを保持するためのフィールドを追加します。

空のコント ローラーのテンプレートを使用してプロジェクトに新しい Shopping Cart コント ローラーを追加します。

![](mvc-music-store-part-8/_static/image2.png)

完全な ShoppingCart コント ローラーを次に示します。 インデックスとコント ローラーの追加のアクションは非常に使いやすくなります。 削除と CartSummary コント ローラーのアクションは、次のセクションで取り上げる 2 つの特殊なケースを処理します。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>JQuery での Ajax の更新

次に、ShoppingCartViewModel を厳密に型指定、リスト ビューを前に、と同じ方法を使用してテンプレートを使用して、ショッピング カートのインデックス ページを作成します。

![](mvc-music-store-part-8/_static/image3.png)

ただし、カートから項目を削除するのに、Html.ActionLink を使用する代わりに使用されます jQuery RemoveLink HTML クラスである必要が、このビュー内のすべてのリンクのクリック イベントを「接続」。 フォームの送信ではなくこの click イベント ハンドラーだけと、AJAX コールバック、RemoveFromCart コント ローラーのアクションになります。 RemoveFromCart 返します JSON シリアル化の結果、jQuery、コールバックは、次を解析し、jQuery を使用して、ページに次の 4 つのクイック更新を実行します。

- 1. 一覧から削除したアルバムを削除します。
- 2. ヘッダーのカート数を更新します。
- 3. 更新メッセージをユーザーに表示します。
- 4. カート合計価格を更新します。

削除シナリオは、インデックス ビュー内で、Ajax コールバックによって処理されているが、以降 RemoveFromCart アクションの追加のビューは不要です。 /ShoppingCart/Index ビューの完全なコードを次に示します。

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

アウトこれをテストするためには、このショッピング カートにアイテムを追加することである必要があります。 更新されます、**ストアの詳細**ビューに含め、"カートに追加 ボタンをクリックします。 おが、アルバムの追加情報が追加されました。 これの一部を含めることで思いますが、このビューを最後に更新しましたので: ジャンル、アーティスト、価格、およびアルバム アート。 更新されたコードのストアの詳細表示では、次のように表示されます。

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

今すぐストアをクリックし、テストを追加して、このショッピング カートからアルバムを削除するおできます。 アプリケーションを実行し、ストア インデックスを参照します。

![](mvc-music-store-part-8/_static/image4.png)

次に、アルバムの一覧を表示するジャンルをクリックします。

![](mvc-music-store-part-8/_static/image5.png)

これで、アルバム タイトルをクリックすると、アルバムの詳細表示を更新"カートに追加 ボタンを含むを示します。

![](mvc-music-store-part-8/_static/image6.png)

ショッピング カート概要リスト、ショッピング カート インデックス ビューを示して"カートに追加 ボタンをクリックします。

![](mvc-music-store-part-8/_static/image7.png)

ショッピング カートを読み込むには、ショッピング カートに Ajax の更新を表示する、カートのリンクから削除 をクリックすることができます。

![](mvc-music-store-part-8/_static/image8.png)

ショッピング カートをカートにアイテムを追加する登録されていないユーザーが作業を開発しています。 次のセクションに登録をチェック アウト プロセスを完了することができますをします。


> [!div class="step-by-step"]
> [前へ](mvc-music-store-part-7.md)
> [次へ](mvc-music-store-part-9.md)
