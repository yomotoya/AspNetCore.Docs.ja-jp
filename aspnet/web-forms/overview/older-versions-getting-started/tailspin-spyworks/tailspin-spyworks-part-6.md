---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: "パート 6: ASP.NET メンバーシップ |Microsoft ドキュメント"
author: JoeStagner
description: "このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 6 部では、ASP.NET メンバーシップを追加します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: efb0e2bed1172f42c7f1539f016fba305c47e3eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-6-aspnet-membership"></a>パート 6: ASP.NET メンバーシップ
====================
によって[行える](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。
> 
> このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 6 部では、ASP.NET メンバーシップを追加します。


## <a id="_Toc260221672"></a>ASP.NET メンバーシップの使用

![](tailspin-spyworks-part-6/_static/image1.png)

[セキュリティ] をクリックします。

![](tailspin-spyworks-part-6/_static/image1.jpg)

フォーム認証が使用されていることを確認してください。

![](tailspin-spyworks-part-6/_static/image2.jpg)

"Create User"リンクを使用すると、数人のユーザーを作成できます。

![](tailspin-spyworks-part-6/_static/image3.jpg)

完了したら、ソリューション エクスプ ローラー ウィンドウを参照してくださいし、ビューを更新します。

![](tailspin-spyworks-part-6/_static/image2.png)

なお、ASPNETDB です。MDF 正常に用意されています。 このファイルには、メンバーシップと同様に、ASP.NET のコア サービスをサポートするためにテーブルが含まれています。

今すぐチェック アウト プロセスの実装を開始できます。

まず CheckOut.aspx ページを作成します。

CheckOut.aspx ページへのアクセスを制限は、ユーザーとログイン ページにログインしていないリダイレクトのユーザーに記録するためにログオンしているユーザーに利用可能なのみする必要があります。

これを行う、次の web.config ファイルの configuration セクションに追加します。

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

ASP.NET Web フォーム アプリケーションのテンプレートは自動的に、web.config ファイルに認証セクションを追加し、既定のログイン ページを確立します。

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Login.aspx 分離コード ファイルをユーザーがログインすると、匿名のショッピング カートを移行するお変更する必要があります。 ページを変更する\_読み込みイベントは次のようにします。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

新しくログイン済みユーザー セッション名を設定および MyShoppingCart クラス MigrateCart メソッドを呼び出すことによって、ユーザーのショッピング カートに一時的なセッション id を変更する次のように「当てはまります」イベント ハンドラーを追加します。 (.Cs ファイルで実装される)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

次のように、MigrateCart() メソッドを実装します。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

Checkout.aspx で使用されます、EntityDataSource および GridView 当社のチェック アウト ページで、買い物カゴのページで行ったようになります。

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

ユーザーの GridView コントロールが MyList を名前付き"ondatabound"イベント ハンドラーを指定することに注意してください\_RowDataBound では次のような場合は、そのイベント ハンドラーを実装します。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

このメソッドが、集計の途中経過、ショッピング カートと各行がバインドされて GridView の一番下の行を更新します。

この段階で配置するための「確認」プレゼンテーションが実装されました。

ページに数行のコードを追加することで空カート シナリオを処理してみましょう\_Load イベント。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

ユーザーが「送信」ボタンをクリックして送信ボタンの Click イベント ハンドラーで、次のコードは実行されます。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

注文の送信プロセスの「肉」MyShoppingCart クラスの SubmitOrder() メソッドの実装を開始します。

SubmitOrder を行います。

- ショッピング カートにすべての行項目を行い、新しい注文レコードと関連付けられている OrderDetails レコードの作成に使用します。
- 出荷日を計算します。
- ショッピング カートをオフにします。


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

このサンプル アプリケーションの目的で、出荷日を現在の日付の 2 日を追加するだけで計算されます。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

今すぐアプリケーションを実行して、開始から終了までショッピング プロセスをテストすることが許可されます。

>[!div class="step-by-step"]
[前へ](tailspin-spyworks-part-5.md)
[次へ](tailspin-spyworks-part-7.md)
