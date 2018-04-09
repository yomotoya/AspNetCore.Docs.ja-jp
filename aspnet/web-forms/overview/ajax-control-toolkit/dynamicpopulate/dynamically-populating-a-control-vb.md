---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: コントロール (VB) を動的に作成する |Microsoft ドキュメント
author: wenz
description: ASP.NET AJAX コントロールのツールキットで DynamicPopulate コントロールは、メソッドを呼び出す web サービス (ページ) と、t の対象のコントロールに、結果の値を設定しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e2031a80be71a406e632955583d83920dd0f3ef7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-populating-a-control-vb"></a>コントロール (VB) を動的に作成します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> ASP.NET AJAX コントロールのツールキットで DynamicPopulate コントロールは、メソッドを呼び出す web サービス (ページ) と、ターゲット コントロール ページで、ページの更新せずに、結果の値を入力します。


## <a name="overview"></a>概要

`DynamicPopulate` ASP.NET AJAX コントロール Toolkit でコントロールのメソッドを呼び出す web サービス (ページ) と [ページの更新なし] ページでターゲット コントロールに、結果の値を設定します。 このチュートリアルでは、これを設定する方法を示します。

## <a name="steps"></a>手順

まず、ASP.NET Web サービスによって呼び出されるメソッドを実装する必要があります`DynamicPopulate`です。 Web サービス クラスに必要な`ScriptService`内で定義されている属性`Microsoft.Web.Script.Services`です。 それ以外の場合 ASP.NET AJAX は順番に必要な web サービスのクライアント側の JavaScript プロキシを作成できません`DynamicPopulate`です。

Web メソッドと呼ばれる、文字列型の 1 つの引数を期待する必要があります`contextKey`、ので、`DynamicPopulate`コントロールは、web サービスの呼び出しごとに 2 つのコンテキスト情報を送信します。 次の web サービスでは、現在の日付を返しますで表される形式で、`contextKey`引数。

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

Web サービスとして保存し、`DynamicPopulate.vb.asmx`です。 また、実装する可能性があります、`getDate()`メソッドを使用して実際の ASP.NET ページ内でページ メソッドとして、`DynamicPopulate`コントロール。

次の手順では、新しい ASP.NET ファイルを作成します。 最初の手順を含めるには常に、よう、 `ScriptManager` ASP.NET AJAX ライブラリを読み込むとコントロール Toolkit 動作させるには、現在のページで。

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

ラベル コントロールを追加し、(たとえば、同じ名前の HTML コントロールを使用して、または&lt; `asp:Label`  / &gt; web コントロール) web サービス呼び出しの結果は後で説明します。

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

(、HTML コントロールとして、サーバーへのポストバックが必要ないため)、HTML のボタンは、カタログを動的に作成をトリガーする使用されます。

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

最後に、必要があります、`DynamicPopulateExtender`作業のネットワーク上のコントロールです。 次の属性が設定されます (とは別に、自明`ID`と`runat` = `"server"`)。

- `TargetControlID` web サービスの呼び出しから結果を配置します。
- `ServicePath` web サービスへのパス (ページ メソッドを使用する場合は省略)
- `ServiceMethod` web メソッドまたはページのメソッドの名前
- `ContextKey` web サービスに送信されるコンテキスト情報
- `PopulateTriggerControlID` web サービスの呼び出しをトリガーする要素
- `ClearContentsDuringUpdate` web サービスの呼び出し中にターゲット要素を空にするかどうか

ご覧のように、コントロールには、いくつかの情報が必要なが所定の位置に配置することで、すべて非常に簡単です。 ここでは、マークアップを`DynamicPopulateExtender`現在のシナリオでの制御。

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

ASP.NET ページをブラウザーで実行し、をクリックします。現在の日付が月-日-年の形式で表示されます。


[![ボタンをクリックして、サーバーから日付を取得します。](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

ボタンをクリックして、サーバーから日付を取得する ([フルサイズのイメージを表示するをクリックして](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [次へ](dynamically-populating-a-control-using-javascript-code-vb.md)
