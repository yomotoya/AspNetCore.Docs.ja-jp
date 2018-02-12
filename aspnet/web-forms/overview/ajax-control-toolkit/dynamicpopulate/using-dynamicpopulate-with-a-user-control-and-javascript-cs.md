---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: "ユーザー コントロールと JavaScript (c#) DynamicPopulate を使って |Microsoft ドキュメント"
author: wenz
description: "ASP.NET AJAX コントロールのツールキットで DynamicPopulate コントロールは、メソッドを呼び出す web サービス (ページ) と、t の対象のコントロールに、結果の値を設定しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d98177561b72ffbe05455f785e156f91e450d36
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>ユーザー コントロールと JavaScript (c#) に DynamicPopulate を使用します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> ASP.NET AJAX コントロールのツールキットで DynamicPopulate コントロールは、メソッドを呼び出す web サービス (ページ) と、ターゲット コントロール ページで、ページの更新せずに、結果の値を入力します。 カスタムのクライアント側 JavaScript コードを使用してカタログの作成をトリガーすることもできます。 ただし、extender は、ユーザー コントロールに存在する場合に実行される特別な注意があります。


## <a name="overview"></a>概要

`DynamicPopulate` ASP.NET AJAX コントロール Toolkit でコントロールのメソッドを呼び出す web サービス (ページ) と [ページの更新なし] ページでターゲット コントロールに、結果の値を設定します。 カスタムのクライアント側 JavaScript コードを使用してカタログの作成をトリガーすることもできます。 ただし、extender は、ユーザー コントロールに存在する場合に実行される特別な注意があります。

## <a name="steps"></a>手順

まず、ASP.NET Web サービスによって呼び出されるメソッドを実装する必要があります、`DynamicPopulateExtender`コントロール。 Web サービス メソッドを実装する`getDate()`と呼ばれる、文字列型の引数を 1 つ指定する必要が`contextKey`、ので、`DynamicPopulate`コントロールは、web サービスの呼び出しごとに 2 つのコンテキスト情報を送信します。 次のコードは (ファイル`DynamicPopulate.cs.asmx`) 3 つの形式のいずれかで現在の日付を取得します。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

次の手順で、新しいユーザー コントロールを作成します (`.ascx`ファイル)、最初の行で、次の宣言を表します。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

A &lt; `label` &gt;サーバーから取得したデータを表示する要素が使用されます。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

また、ユーザー コントロール ファイル内を使用します次の 3 つのオプション ボタン、web サービスによってサポートされる 3 つの可能な日付形式のいずれかを表すごとに 1 つ。 ラジオ ボタンのいずれかで、ユーザーがクリックすると、ブラウザーは次のような JavaScript コードを実行します。

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

このコードにアクセスする、 `DynamicPopulateExtender` (奇妙な ID を心配しないでまだ、これについては、後で) し、データを動的な作成をトリガーします。 現在のラジオ ボタンのコンテキストで`this.value`はその値を指す`format1`、`format2`または`format3`正確にどのような web メソッドが必要です。

ユーザー コントロールにまだないですだけが、`DynamicPopulateExtender`オプション ボタン web サービスからへのリンク コントロール。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

コントロールで使用される奇妙な ID を再度確認可能性があります:`mcd1$myDate`の代わりに`myDate`です。 以前は、JavaScript コード`mcd1_dpe1`にアクセスする、`DynamicPopulateExtender`の代わりに`dpe1`です。この名前付け方法を使用する場合に特別な要件を`DynamicPopulateExtender`ユーザー コントロール内で。 さらに、正常に動作させる特定の方法でユーザー コントロールを埋め込む必要があります。 新しい ASP.NET ページを作成し、実装した、ユーザー コントロールのタグ プレフィックスを登録します。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

次に、ASP.NET AJAX を含めます`ScriptManager`新しいページ上のコントロール。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

最後に、ユーザー コントロールをページに追加します。 のみを設定する必要があるその`ID`属性 (および`runat="server"`もちろん、) も、特定の名前に設定する必要があるが、:`mcd1`ので、これは JavaScript を使用してそれにアクセスするユーザー コントロール内で使用されるプレフィックス。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

以上です。 ページが期待どおりに動作します。 ユーザーがラジオ ボタンの 1 つクリックすると、、Toolkit でコントロールが web サービスを呼び出すと、目的の形式で、現在の日付を表示します。


[![ユーザー コントロールでのラジオ ボタンが存在します。](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

ユーザー コントロールでのラジオ ボタンが存在する ([フルサイズのイメージを表示するをクリックして](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

>[!div class="step-by-step"]
[前へ](dynamically-populating-a-control-using-javascript-code-cs.md)
[次へ](dynamically-populating-a-control-vb.md)
