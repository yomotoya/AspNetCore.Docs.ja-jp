---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: "手順 5: ビジネス ロジック |Microsoft ドキュメント"
author: JoeStagner
description: "このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 5 部では、いくつかのビジネス ロジックを追加します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e205788e05a2ad94d86d4847c11c40898b1c3113
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-5-business-logic"></a>手順 5: ビジネス ロジック
====================
によって[行える](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。
> 
> このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 5 部では、いくつかのビジネス ロジックを追加します。


## <a id="_Toc260221671"></a>一部のビジネス ロジックを追加します。

当社の web サイトにアクセスするたびに使用できる、ショッピングします。 閲覧者を参照して、登録またはログインしていない場合でも、ショッピング カートにアイテムを追加することができます。 チェック アウトする準備ができたらが与えられます認証のオプションをしない場合はまだメンバーができるアカウントを作成します。

これは、匿名の状態から「ユーザーの登録済み」状態に、ショッピング カートを変換するロジックを実装する必要がありますが、ことを意味します。

みましょう「クラス」という名前のディレクトリを作成、フォルダーを右クリックし、MyShoppingCart.cs をという名前の新しい"Class"ファイルを作成します。

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

既に触れましたが MyShoppingCart.aspx ページを実装するクラスを拡張して、私たちはこれを使用して実行します。NET の強力な「部分的なクラス」を構築します。

次のような MyShoppingCart.aspx.cf ファイルの生成された呼び出しが表示されます。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

"Partial"キーワードの使用に注意してください。

生成したクラス ファイルは、次のようになります。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

このファイルにも、部分的なキーワードを追加することで実装がマージされます。

新しいクラス ファイルは、次のようになります。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

このクラスに追加する最初のメソッドは、"AddItem"メソッドです。 これは、ユーザーが、製品の一覧と製品の詳細ページに「アートを追加」へのリンクをクリックしたときに最終的に呼び出されるメソッドです。

使用して、、次を追加、ページの上部にあるステートメントです。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

MyShoppingCart クラスにこのメソッドを追加します。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

かどうか、項目は既に、カートを表示するのに LINQ to Entities を使用します。 場合は、アイテムの注文数量を更新、それ以外の場合新しいエントリを作成する、選んだ項目の

このメソッドを呼び出すためには、このメソッドをクラスだけでなく、現在の項目が追加された後に = カートを買い物を表示するための AddToCart.aspx ページを実装します。

ソリューション エクスプ ローラーでソリューション名を右クリックし、追加し、新しいページを既に行って AddToCart.aspx という名前です。

実装と低のストック問題などの中間結果を表示おこのページを使用できますが、ページが実際には、レンダリングしますが、ではなく"Add"ロジックを呼び出すし、リダイレクトします。

これを実現する、ページに、次のコードを追加しますお\_Load イベント。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

QueryString パラメーターと、クラスの AddItem メソッドを呼び出してから、ショッピング カートに追加する製品を取得することに注意してください。

発生したエラーがないと想定し、完全を実装します。 次に後ページに制御が渡されます。 必要がありますエラーが発生する場合、例外がスローされます。

現在を実装していないグローバル エラー ハンドラーのため、この例外は、アプリケーションで処理されないが、私たちはこの問題を解決直後。

Debug.Fail() (を介して使用できるステートメントを使用しても注意してください。`using System.Diagnostics;)`

デバッガー内のアプリケーションが実行されている、このメソッドを指定するエラー メッセージと共にアプリケーションの状態に関する情報を含む詳細なダイアログが表示されます。

Debug.Fail() ステートメントを実稼働環境で実行されている場合は無視されます。

ショッピング カート クラス名"GetShoppingCartId"のメソッドを呼び出す前のコードで注意してください。

次のように、メソッドを実装するコードを追加します。

更新プログラムとチェック アウトのボタンと、カート「合計」を表示おラベルも追加されたことに注意してください。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

このショッピング カートにアイテムを追加できますようになりましたが、製品を追加した後、カートを表示するためのロジックを実装していないこと。

そのため、MyShoppingCart.aspx ページで追加 EntityDataSource コントロールと GridVire コントロールとおりです。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

カートの更新ボタンをダブルクリックしてしたり、マークアップ内の宣言で指定されている click イベント ハンドラーを生成できるように、デザイナーでフォームを呼び出します。

後で詳細を実装しますが、これはお知らせアプリケーション ビルドし実行、エラーが発生しません。

アプリケーションを実行し、ショッピング カートにアイテムを追加するときにこれが表示されます。

![](tailspin-spyworks-part-5/_static/image2.jpg)

次の 3 つのカスタム列を実装することで、"default"のグリッド表示から傾いたりおに注意してください。

1 つは、編集可能、数量の「バインド」フィールドには。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

"次へ計算された"列、行アイテムの合計 (を順序付ける数量回アイテム コスト) を表示します。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

最後に、ユーザーがショッピング グラフから項目を削除する必要があることを示すために使用する チェック ボックス コントロールを含むカスタムの列があります。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

ご覧のように、合計行がでは空の順序は注文合計を計算するいくつかのロジックを追加します。

"GetTotal"メソッド、MyShoppingCart クラスにまずを実装します。

MyShoppingCart.cs ファイルでは、次のコードを追加します。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

その後、ページ \_Load イベント ハンドラーが、GetTotal メソッドを呼び出してことがあります。 同時に、ショッピング カートが空かどうかである場合は、必要に応じて表示を調整してテストを追加します。

これで、ショッピング カートの内容が空の場合これが表示されます。

![](tailspin-spyworks-part-5/_static/image4.jpg)

合計が表示されていない場合。

![](tailspin-spyworks-part-5/_static/image5.jpg)

ただし、このページはまだ完了しません。

削除のマークされた項目を削除して、一部が変更された可能性がグリッドに、ユーザーが、新しい数量の値を決定することにより、ショッピング カートを再計算する追加のロジックが必要ですが。

ユーザーがアイテムの削除をマークするときに、ケースを処理する MyShoppingCart.cs に、ショッピング カート クラスに"RemoveItem"メソッドを追加することができます。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

今すぐみましょう ad ユーザーが GridView で注文する品質を単に変更するときに、状況を処理するメソッド。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

インプレースの削除と更新プログラムの基本的な機能では、実際には、データベースに、ショッピング カートを更新するロジックを実装おことができます。 (で MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

このメソッドが 2 つのパラメーターを期待していることに注意します。 1 は、ショッピング カート Id で、もう 1 つはユーザー定義型のオブジェクトの配列です。

ユーザー インターフェイスについて、ロジックの依存関係を最小限にとどめる GridView コントロールに直接アクセスする必要がある、メソッドを使用しないで、ショッピング カートのアイテムをコードに渡すに使用できるデータ構造を定義したのでです。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

MyShoppingCart.aspx.cs ファイルで使用できますこの構造体、更新ボタンの Click イベント ハンドラーでとおり。 に加えて、買い物カゴの更新を更新してカートの合計金額にも注意してください。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

次のコード行の特定の目的に注意してください。

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() は MyShoppingCart.aspx.cs で次のように実装が特殊なヘルパー関数です。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

これにより、クリーン、GridView コントロールのバインド要素の値にアクセスします。 ユーザーの「アイテムの削除」チェック ボックス コントロールがバインドされていないために、FindControl() メソッドを使用してアクセスしてみましょう。

プロジェクトの開発では、この段階では、お支払い処理を実装する準備がされています。

みましょうそう前に、Visual Studio を使用して、メンバーシップ データベースを生成し、ユーザーのメンバーシップのリポジトリを追加します。

>[!div class="step-by-step"]
[前へ](tailspin-spyworks-part-4.md)
[次へ](tailspin-spyworks-part-6.md)
