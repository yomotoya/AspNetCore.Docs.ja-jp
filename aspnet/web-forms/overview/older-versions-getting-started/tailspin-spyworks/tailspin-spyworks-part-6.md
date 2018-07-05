---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'パート 6: ASP.NET メンバーシップ |Microsoft Docs'
author: JoeStagner
description: このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 6 では、ASP.NET メンバーシップを追加します。
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 303c1edf548db1da9ef61d94bdd8157d6afbb2e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804421"
---
<a name="part-6-aspnet-membership"></a>パート 6: ASP.NET メンバーシップ
====================
によって[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力でスケーラブルなアプリケーションを作成するはどの非常に単純なを示します。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアを構築する方法を示します。
> 
> このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 6 では、ASP.NET メンバーシップを追加します。


## <a id="_Toc260221672"></a>  ASP.NET メンバーシップの使用

![](tailspin-spyworks-part-6/_static/image1.png)

[セキュリティ] をクリックします。

![](tailspin-spyworks-part-6/_static/image1.jpg)

フォーム認証が使用されていることを確認します。

![](tailspin-spyworks-part-6/_static/image2.jpg)

数人のユーザーを作成するのにには、「ユーザーの作成」リンクを使用します。

![](tailspin-spyworks-part-6/_static/image3.jpg)

完了したら、ソリューション エクスプ ローラー ウィンドウを参照してくださいし、表示を更新します。

![](tailspin-spyworks-part-6/_static/image2.png)

なお、ASPNETDB します。MDF の問題が作成されました。 このファイルには、メンバーシップのように ASP.NET のコア サービスをサポートするためにテーブルが含まれています。

これでチェック アウト プロセスの実装を開始できます。

まず CheckOut.aspx ページを作成します。

CheckOut.aspx ページのみへのアクセスは制限がログインしてユーザーとリダイレクトのユーザーがログイン ページにログインしていないためにログオンしているユーザーが使用できる場合があります。

これを行うには次の web.config ファイルの構成セクションを追加します。

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

ASP.NET Web フォーム アプリケーション テンプレートは自動的に、web.config ファイルに、[認証] セクションを追加し、既定のログイン ページが確立されています。

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Login.aspx 分離コード ファイルに、ユーザーがログインすると、匿名のショッピング カートを移行するよう変更する必要があります。 ページを変更する\_読み込みイベントは次のようにします。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

新しくログイン ユーザーに、セッション名を設定し、MyShoppingCart クラス MigrateCart メソッドを呼び出しているユーザーのショッピング カートに一時的なセッション id に変更は、このような「当てはまります」イベント ハンドラーを追加します。 (.Cs ファイルで実装)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

MigrateCart() メソッドの実装では、このような。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

Checkout.aspx で使用します、EntityDataSource と GridView、チェック アウト ページで、ショッピング カート ページで行ったようにします。

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

GridView コントロールが MyList という名前の"ondatabound"イベント ハンドラーを指定することに注意してください\_RowDataBound このような場合は、そのイベント ハンドラーを実装してみましょう。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

このメソッドは、累計のショッピング カートの各行がバインドされているし、GridView の一番下の行を更新を保持します。

この段階で配置する順序を"review"プレゼンテーションを実装しました。

ページに数行のコードを追加することで、空のカート シナリオを処理しましょう\_Load イベント。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

ユーザーが [送信] ボタンをクリックすると、送信ボタンの Click イベント ハンドラーで、次のコードを実行しますが。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

注文の送信処理の「核心部分」MyShoppingCart クラスの SubmitOrder() メソッドで実装することです。

SubmitOrder を行います。

- ショッピング カート内のすべての行項目を実行し、新しい注文レコードと関連付けられている OrderDetails レコードの作成に使用します。
- 出荷日を計算します。
- ショッピング カートをオフにします。


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

このサンプル アプリケーションの目的で 2 日間を現在の日付に追加するだけで出荷日を計算します。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

これでアプリケーションを実行している、最初から最後までショッピングのプロセスをテストすることが許可されます。

> [!div class="step-by-step"]
> [前へ](tailspin-spyworks-part-5.md)
> [次へ](tailspin-spyworks-part-7.md)
