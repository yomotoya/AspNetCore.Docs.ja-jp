---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'パート 5: ビジネス ロジック |Microsoft Docs'
author: JoeStagner
description: このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 5 では、いくつかのビジネス ロジックを追加します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: 719d56e0764e2f66b8813c9487119bbc700d738c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377987"
---
<a name="part-5-business-logic"></a>パート 5: ビジネス ロジック
====================
によって[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力でスケーラブルなアプリケーションを作成するはどの非常に単純なを示します。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアを構築する方法を示します。
> 
> このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 5 では、いくつかのビジネス ロジックを追加します。


## <a id="_Toc260221671"></a>  一部のビジネス ロジックを追加します。

経験ショッピング web サイトにアクセスするたびに使用する必要があります。 訪問者を参照して、登録またはログインしていない場合でも、ショッピング カートにアイテムを追加することになります。 認証するオプションを指定するをチェック アウトの準備ができたら、それ以外の場合まだメンバーができるアカウントを作成する.

これは、ショッピング カートを匿名の状態から「ユーザーの登録済み」状態に変換するロジックを実装する必要ことを意味します。

みましょう「クラス」という名前のディレクトリを作成し、フォルダーを右クリックし、MyShoppingCart.cs をという名前の新しい「クラス」ファイルの作成

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

前述したよう MyShoppingCart.aspx ページを実装するクラスを拡張しましたと私たちはこれを使用します。NET の強力な「部分クラス」のコンス トラクター。

MyShoppingCart.aspx.cf ファイルの生成された呼び出しは次のようです。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

"Partial"キーワードの使用に注意してください。

生成したばかりのクラス ファイルは、次のように検索します。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

実装も、このファイルに部分的なキーワードを追加することでマージされます。

新しいクラス ファイルは、次のようになります。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

クラスに追加する最初のメソッドは、"AddItem"方法です。 これは、ユーザーが、製品リストと製品の詳細ページに「アートを追加」リンクをクリックすると最終的に呼び出されるメソッド。

使用して、次を追加、ページの上部にあるステートメント。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

MyShoppingCart クラスにこのメソッドを追加します。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

カートにアイテムが既にかどうかは LINQ to Entities を使用しています。 それ以外の場合、選択した項目の新しいエントリを作る場合、そのため、アイテムの注文数量を更新、

このメソッドを呼び出すためには、このメソッドをクラスだけでなく、現在の項目が追加された後に = カートをショッピングを表示する AddToCart.aspx ページを実装します。

ソリューション エクスプ ローラーでソリューション名を右クリックし、追加し、新しいページが以前に行ったように、AddToCart.aspx をという名前です。

実装に在庫不足の問題などのように中間結果を表示このページを使用できますが、ページが実際には、レンダリングしますが、ではなく、"Add"ロジックを呼び出すし、リダイレクトします。

これを実現する、ページに、次のコードを追加します\_Load イベント。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

クエリ文字列パラメーターと、クラスの AddItem メソッドを呼び出してから、ショッピング カートに追加する製品取得することに注意してください。

発生したエラーは想定されません後ページを次に実装は完全に制御が渡されます。 エラーがある必要がある場合は、例外をスローします。

現在まだ実装していないグローバル エラー ハンドラーのため、この例外は、アプリケーションでハンドルされないがまもなく修正は。

Debug.Fail() (を介して使用できるステートメントを使用しても注意してください。 `using System.Diagnostics;)`

アプリケーションがデバッガー内部で実行されている、このメソッドを指定するエラー メッセージと共にアプリケーションの状態に関する情報を含む詳細なダイアログが表示されます。

Debug.Fail() ステートメントを運用環境で実行されている場合は無視されます。

上で、ショッピング カート クラス名"GetShoppingCartId"メソッドの呼び出しのコードことがわかります。

次のように、メソッドを実装するコードを追加します。

更新プログラムとチェック アウトのボタンと「合計」カートの内容を表示しますラベルも追加したことに注意してください。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

このショッピング カートにアイテムを追加することができますようになりましたが、製品を追加した後、カートを表示するためのロジックを実装していません。

、MyShoppingCart.aspx ページでこれを追加、EntityDataSource コントロールと GridVire コントロールには、次のようにします。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

カートの更新 ボタンをダブルクリックして、マークアップで宣言で指定されているクリック イベント ハンドラーを生成できるように、デザイナーでフォームを呼び出します。

後で詳細を実装しますが、お知らせビルドおよび実行のエラーのないアプリケーションではこれを行います。

アプリケーションを実行し、ショッピング カートにアイテムを追加するときにこれが表示されます。

![](tailspin-spyworks-part-5/_static/image2.jpg)

3 つのカスタム列を実装することで、「既定」のグリッド表示から逸脱ことに注意してください。

1 つは、編集可能な数量の「バインド」フィールドには。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

(注文する数量回アイテム コスト) 行アイテムの合計を表示する「計算」の列を次に示します。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

最後に、ユーザーがショッピングのグラフから、アイテムを削除することを示すために使用する チェック ボックス コントロールを含むカスタムの列があります。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

ご覧のように、合計行がでは空の順序は注文合計を計算するいくつかのロジックを追加します。

まず"GetTotal"メソッド MyShoppingCart クラスに実装します。

MyShoppingCart.cs ファイルでは、次のコードを追加します。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

次のページに\_Load イベント ハンドラーが、GetTotal メソッドを呼び出すことができます。 同時に、ショッピング カートが空かどうかである場合、表示を調整してテストを追加します。

これで、ショッピング カートの内容が空の場合これが表示されます。

![](tailspin-spyworks-part-5/_static/image4.jpg)

そうでない場合、合計がわかります。

![](tailspin-spyworks-part-5/_static/image5.jpg)

ただし、このページはまだ完了しません。

追加のロジックを削除対象としてマークされた項目を削除して、一部が変更されたグリッドで、ユーザーが、新しい数量の値を決定することにより、ショッピング カートを再計算する必要があります。

ユーザーがアイテムの削除をマークするときに、ケースを処理する MyShoppingCart.cs でショッピング カート クラスに"RemoveItem"メソッドを追加することができます。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

今すぐみましょう ad ユーザーは、GridView で注文する品質を単に変更されたときに、状況を処理するメソッド。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

場所の削除と更新プログラムの基本的な機能実際には、データベース内のショッピング カートを更新するロジックを実装できます。 (で MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

このメソッドが 2 つのパラメーターを期待していることがわかります。 ショッピング カート Id 1 つは、他のユーザー定義型のオブジェクトの配列。

ユーザー インターフェイスの仕様に、ロジックの依存関係を最小限にとどめる GridView コントロールに直接アクセスする必要がある方法では、せず、コードをショッピング カートのアイテムを渡す使用できるデータ構造を定義しました。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

MyShoppingCart.aspx.cs ファイルで使用できますこの構造体の Update ボタンの Click イベント ハンドラーでとおり。 に加えて、カートの更新を更新してカート合計も注意してください。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

次のコード行で特に重要な注意してください。

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() は、次のように MyShoppingCart.aspx.cs で私たちが実装する特殊なヘルパー関数です。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

これは、クリーン、GridView コントロールのバインド要素の値にアクセスする方法を提供します。 ユーザー「アイテムの削除」のチェック ボックス コントロールがバインドされていないために、FindControl() メソッドを使用してアクセスします。

この段階で、プロジェクトの開発、清算処理を実装する準備ができていますされています。

そってしましょう前に、Visual Studio を使用して、メンバーシップ データベースを生成し、ユーザーのメンバーシップのリポジトリを追加します。

> [!div class="step-by-step"]
> [前へ](tailspin-spyworks-part-4.md)
> [次へ](tailspin-spyworks-part-6.md)
