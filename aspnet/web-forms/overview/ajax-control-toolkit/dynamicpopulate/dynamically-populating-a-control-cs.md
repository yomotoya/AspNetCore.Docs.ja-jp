---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: (C#) コントロールを動的に作成する |Microsoft Docs
author: wenz
description: ASP.NET AJAX Control Toolkit で DynamicPopulate コントロールは、web サービス (またはページ メソッド) を呼び出すし、t のターゲット コントロールに、結果の値を入力しています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a78e2800b119db61965f9922ba99a2f90e6be948
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827328"
---
<a name="dynamically-populating-a-control-c"></a>(C#) コントロールを動的に作成します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)

> ASP.NET AJAX Control Toolkit で DynamicPopulate コントロールは、web サービス (またはページ メソッド) を呼び出すし、ページで、ページを更新せず、ターゲット コントロールに、結果の値を設定します。


## <a name="overview"></a>概要

`DynamicPopulate` ASP.NET AJAX Control Toolkit のコントロールが web サービス (またはページ メソッド) を呼び出し、ターゲット コントロール ページで、ページ更新せずに、結果の値を入力します。 このチュートリアルでは、これを設定する方法を示します。

## <a name="steps"></a>手順

まず、ASP.NET Web サービスによって呼び出されるメソッドを実装する必要があります`DynamicPopulate`します。 Web サービス クラスに必要な`ScriptService`内で定義されている属性`Microsoft.Web.Script.Services`。 それ以外の場合、ASP.NET AJAX はにさらに必要な web サービスのクライアント側の JavaScript プロキシを作成することはできません`DynamicPopulate`します。

Web メソッドと呼ばれる、文字列型の 1 つの引数を想定する必要があります`contextKey`、ので、`DynamicPopulate`コントロールが web サービスの呼び出しごとに 2 つのコンテキスト情報を送信します。 次の web サービスによって表される形式で現在の日付を返します、`contextKey`引数。

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

Web サービスとして保存されます`DynamicPopulate.cs.asmx`します。 また、実装すること、`getDate()`メソッドでは、実際の ASP.NET ページ内でページ メソッドとして、`DynamicPopulate`コントロール。

次の手順では、新しい ASP.NET ファイルを作成します。 最初の手順を含めるには常に、よう、 `ScriptManager` Control Toolkit の作業を行うと ASP.NET AJAX ライブラリを読み込むには、現在のページで。

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

ラベル コントロールを追加し、(たとえば、同じ名前の HTML コントロールを使用して、または&lt; `asp:Label`  / &gt; web コントロール)、web サービス呼び出しの結果は後で説明します。

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

(HTML コントロール、サーバーへのポストバックは必要ありませんので) として、HTML ボタンは、動的な作成をトリガーを使用しています。

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

最後に、必要があります、`DynamicPopulateExtender`ワイヤ処理を制御します。 次の属性が設定されます (、明らかなものとは別に`ID`と`runat` = `"server"`)。

- `TargetControlID` web サービスの呼び出しから結果を格納する場所
- `ServicePath` web サービスへのパス (ページ メソッドを使用する場合は省略)
- `ServiceMethod` web メソッドまたはページ メソッドの名前
- `ContextKey` web サービスに送信されるコンテキスト情報
- `PopulateTriggerControlID` web サービスの呼び出しをトリガーする要素
- `ClearContentsDuringUpdate` web サービスの呼び出し中にターゲット要素を空にするかどうか

ご覧のように、コントロールには、いくつかの情報が必要なが非常に簡単には所定の位置に配置することで、すべてのもの。 マークアップを次に示します、`DynamicPopulateExtender`現在のシナリオでのコントロール。

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

ASP.NET ページをブラウザーで実行して、; ボタンをクリックします現在の日付が月-日-年の形式で表示されます。


[![ボタンをクリックして、サーバーから日付を取得します。](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)

ボタンをクリックしますが、サーバーから日付を取得します ([フルサイズの画像を表示する をクリックします](dynamically-populating-a-control-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [次へ](dynamically-populating-a-control-using-javascript-code-cs.md)
