---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: ユーザー コントロールと (VB) を JavaScript で DynamicPopulate を使用する |Microsoft Docs
author: wenz
description: ASP.NET AJAX Control Toolkit で DynamicPopulate コントロールは、web サービス (またはページ メソッド) を呼び出すし、t のターゲット コントロールに、結果の値を入力しています.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: a275eed17552d26b63f98762c6c870bd53dd455d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836817"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a>ユーザー コントロールと (VB) を JavaScript で DynamicPopulate を使用します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)

> ASP.NET AJAX Control Toolkit で DynamicPopulate コントロールは、web サービス (またはページ メソッド) を呼び出すし、ページで、ページを更新せず、ターゲット コントロールに、結果の値を設定します。 カスタムのクライアント側の JavaScript コードを使用して、カタログの作成をトリガーすることもできます。 ただし、エクステンダーは、ユーザー コントロールが存在するときに実行される特別な注意があります。


## <a name="overview"></a>概要

`DynamicPopulate` ASP.NET AJAX Control Toolkit のコントロールが web サービス (またはページ メソッド) を呼び出し、ターゲット コントロール ページで、ページ更新せずに、結果の値を入力します。 カスタムのクライアント側の JavaScript コードを使用して、カタログの作成をトリガーすることもできます。 ただし、エクステンダーは、ユーザー コントロールが存在するときに実行される特別な注意があります。

## <a name="steps"></a>手順

まず、ASP.NET Web サービスによって呼び出されるメソッドを実装する必要があります、`DynamicPopulateExtender`コントロール。 Web サービス メソッドを実装する`getDate()`と呼ばれる、文字列型の 1 つの引数が予想される`contextKey`、ので、`DynamicPopulate`コントロールが web サービスの呼び出しごとに 2 つのコンテキスト情報を送信します。 コードを次に示します (ファイル`DynamicPopulate.vb.asmx`) 3 つの形式のいずれかで現在の日付を取得します。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

次の手順で新しいユーザー コントロールを作成します (`.ascx`ファイル)、その 1 行目で次の宣言を表します。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

A &lt; `label` &gt;要素を使用して、サーバーから取得したデータを表示します。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

また、ユーザー コントロール ファイル内を使用します次の 3 つのラジオ ボタン、それぞれが表す web サービスでサポートされる 3 つの可能な日付形式のいずれか。 ユーザーは、ラジオ ボタンのいずれかをクリックすると、ブラウザーは次のような JavaScript コードを実行します。

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

このコードにアクセスする、 `DynamicPopulateExtender` (奇妙な ID を心配しないでまだ、これについては、後で)、データの動的作成をトリガーします。 現在のラジオ ボタンのコンテキストで`this.value`はその値を指す`format1`、`format2`または`format3`正確にどのような web メソッドが必要です。

まだユーザー コントロールで不足しているですだけが、`DynamicPopulateExtender`オプション ボタン web サービスからへのリンク コントロール。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

奇妙に、コントロールで使用される ID を再度確認可能性があります:`mcd1$myDate`の代わりに`myDate`します。 以前は、使用される JavaScript コード`mcd1_dpe1`にアクセスする、`DynamicPopulateExtender`の代わりに`dpe1`します。この名前付け方法を使用する場合に特別な要件を`DynamicPopulateExtender`ユーザー コントロール内で。 さらに、正常に動作させる特定の方法でユーザー コントロールを埋め込むことがあります。 新しい ASP.NET ページを作成し、実装したユーザー コントロールのタグ プレフィックスを登録します。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

次に、ASP.NET AJAX を含めます`ScriptManager`新しいページ上のコントロール。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

最後に、ユーザー コントロールをページに追加します。 のみを設定する必要があるその`ID`属性 (と`runat="server"`、もちろん)、特定の名前に設定する必要も:`mcd1`ので、これは、JavaScript を使用してアクセスするユーザー コントロール内で使用されるプレフィックス。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

以上です。 ページが期待どおりに動作します。 ユーザーがラジオ ボタンの 1 つクリックすると、Toolkit でコントロールが web サービスを呼び出すと、目的の形式で、現在の日付を表示します。


[![ラジオ ボタンがユーザー コントロール内に存在します。](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)

ユーザー コントロールでオプション ボタンが存在する ([フルサイズの画像を表示する をクリックします](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](dynamically-populating-a-control-using-javascript-code-vb.md)
