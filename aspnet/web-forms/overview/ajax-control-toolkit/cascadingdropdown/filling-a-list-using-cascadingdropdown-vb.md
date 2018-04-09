---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: CascadingDropDown (VB) を使用して、リストを入力 |Microsoft ドキュメント
author: wenz
description: CascadingDropDown コントロール、AJAX コントロール Toolkit でコントロールを拡張する DropDownList anoth 内の値が 1 つの DropDownList 負荷の変更に関連付けられているようにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: e488a30443970d5e2ce825abd96d8e4a027585d1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a>CascadingDropDown (VB) を使用して、リストを入力
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。 (たとえば、1 つのリストが状態、私たちの一覧を提供し、[次へ] の一覧は状態にある主要都市で埋められます)。解決するために最初のチャレンジが実際にこのコントロールを使用して、ドロップダウン リストです。


## <a name="overview"></a>概要

AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。 (たとえば、1 つのリストが状態、私たちの一覧を提供し、[次へ] の一覧は状態にある主要都市で埋められます)。解決するために最初のチャレンジが実際にこのコントロールを使用して、ドロップダウン リストです。

## <a name="steps"></a>手順

ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

その後、DropDownList コントロールが必要です。

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

この一覧 CascadingDropDown extender が追加されます。 一覧に表示されるエントリの一覧を返すし、web サービスへの非同期要求を送信します。 これを行うには、次 CascadingDropDown 属性を設定する必要があります。

- `ServicePath`: 一覧のエントリを提供する web サービスの URL
- `ServiceMethod`: Web メソッドの一覧のエントリを提供します。
- `TargetControlID`。 ドロップダウン リストの ID
- `Category`: カテゴリについては、web メソッドが呼び出されたときに送信されます。
- `PromptText`: サーバーから非同期的にリストのデータを読み込むときに表示されるテキスト

ここでは、マークアップを`CascadingDropDown`要素。 C# および VB の唯一の違いは、関連付けられている web サービスの名前を示します。

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

JavaScript コード、 `CascadingDropDown` extender は、次のシグネチャを持つ web サービス メソッドを呼び出します。

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

重要な側面は、メソッドの型の配列を返す必要があるように`CascadingDropDownNameValue`(ASP.NET AJAX コントロール Toolkit により定義) です。 `CascadingDropDownNameValue`一覧のエントリのテキストとし、その値を指定する必要があります、最初のコンス トラクターと同様に`<option value="VALUE">NAME</option>`HTML での操作とします。 サンプル データの一部を次に示します。

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

ブラウザーでページの読み込みと、次の 3 つの仕入先を格納するリストがトリガーされます。


[![一覧が自動的に入力します。](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

一覧が自動的に入力 ([フルサイズのイメージを表示するをクリックして](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](using-auto-postback-with-cascadingdropdown-cs.md)
> [次へ](using-cascadingdropdown-with-a-database-vb.md)
