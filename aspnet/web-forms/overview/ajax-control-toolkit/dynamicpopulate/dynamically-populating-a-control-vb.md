---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: コントロール (VB) を動的に作成する |Microsoft Docs
author: wenz
description: ASP.NET AJAX Control Toolkit で DynamicPopulate コントロールは、web サービス (またはページ メソッド) を呼び出すし、t のターゲット コントロールに、結果の値を入力しています.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: cda816cab99867aac8770c420cab7a78ba699e4e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836204"
---
<a name="dynamically-populating-a-control-vb"></a>コントロール (VB) を動的に作成します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> ASP.NET AJAX Control Toolkit で DynamicPopulate コントロールは、web サービス (またはページ メソッド) を呼び出すし、ページで、ページを更新せず、ターゲット コントロールに、結果の値を設定します。


## <a name="overview"></a>概要

`DynamicPopulate` ASP.NET AJAX Control Toolkit のコントロールが web サービス (またはページ メソッド) を呼び出し、ターゲット コントロール ページで、ページ更新せずに、結果の値を入力します。 このチュートリアルでは、これを設定する方法を示します。

## <a name="steps"></a>手順

まず、ASP.NET Web サービスによって呼び出されるメソッドを実装する必要があります`DynamicPopulate`します。 Web サービス クラスに必要な`ScriptService`内で定義されている属性`Microsoft.Web.Script.Services`。 それ以外の場合、ASP.NET AJAX はにさらに必要な web サービスのクライアント側の JavaScript プロキシを作成することはできません`DynamicPopulate`します。

Web メソッドと呼ばれる、文字列型の 1 つの引数を想定する必要があります`contextKey`、ので、`DynamicPopulate`コントロールが web サービスの呼び出しごとに 2 つのコンテキスト情報を送信します。 次の web サービスによって表される形式で現在の日付を返します、`contextKey`引数。

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

Web サービスとして保存されます`DynamicPopulate.vb.asmx`します。 また、実装すること、`getDate()`メソッドでは、実際の ASP.NET ページ内でページ メソッドとして、`DynamicPopulate`コントロール。

次の手順では、新しい ASP.NET ファイルを作成します。 最初の手順を含めるには常に、よう、 `ScriptManager` Control Toolkit の作業を行うと ASP.NET AJAX ライブラリを読み込むには、現在のページで。

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

ラベル コントロールを追加し、(たとえば、同じ名前の HTML コントロールを使用して、または&lt; `asp:Label`  / &gt; web コントロール)、web サービス呼び出しの結果は後で説明します。

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

(HTML コントロール、サーバーへのポストバックは必要ありませんので) として、HTML ボタンは、動的な作成をトリガーを使用しています。

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

最後に、必要があります、`DynamicPopulateExtender`ワイヤ処理を制御します。 次の属性が設定されます (、明らかなものとは別に`ID`と`runat` = `"server"`)。

- `TargetControlID` web サービスの呼び出しから結果を格納する場所
- `ServicePath` web サービスへのパス (ページ メソッドを使用する場合は省略)
- `ServiceMethod` web メソッドまたはページ メソッドの名前
- `ContextKey` web サービスに送信されるコンテキスト情報
- `PopulateTriggerControlID` web サービスの呼び出しをトリガーする要素
- `ClearContentsDuringUpdate` web サービスの呼び出し中にターゲット要素を空にするかどうか

ご覧のように、コントロールには、いくつかの情報が必要なが非常に簡単には所定の位置に配置することで、すべてのもの。 マークアップを次に示します、`DynamicPopulateExtender`現在のシナリオでのコントロール。

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

ASP.NET ページをブラウザーで実行して、; ボタンをクリックします現在の日付が月-日-年の形式で表示されます。


[![ボタンをクリックして、サーバーから日付を取得します。](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

ボタンをクリックしますが、サーバーから日付を取得します ([フルサイズの画像を表示する をクリックします](dynamically-populating-a-control-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [次へ](dynamically-populating-a-control-using-javascript-code-vb.md)
