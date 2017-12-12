---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: "ポストバックの併用 ReorderList (c#) |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロールのツールキットで ReorderList コントロール一覧に表示される、ユーザーがドラッグ アンド ドロップで並べ替えることができます。 一覧の順序が変更されるたびに、po しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 5f5c5e253f6d85203a488152c5ad908157e6ee0c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-postbacks-with-reorderlist-c"></a>ポストバックの併用 ReorderList (c#)
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> AJAX コントロールのツールキットで ReorderList コントロール一覧に表示される、ユーザーがドラッグ アンド ドロップで並べ替えることができます。 一覧の順序が変更されるたびにポストバックの変更のサーバーに通知するものとします。


## <a name="overview"></a>概要

`ReorderList` AJAX コントロール Toolkit でコントロール一覧に表示される、ユーザーがドラッグ アンド ドロップで並べ替えることができます。 一覧の順序が変更されるたびにポストバックの変更のサーバーに通知するものとします。

## <a name="steps"></a>手順

いくつかの可能なデータ ソースが、`ReorderList`コントロール。 使用する 1 つは、`XmlDataSource`コントロール。

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

この XML をバインドするために、`ReorderList`コントロールと有効にするポストバックで、次の属性を設定する必要があります。

- `DataSourceID`: データ ソースの ID
- `SortOrderField`: プロパティを並べ替える
- `AllowReorder`: リストの要素の順序を変更するユーザーを許可するか
- `PostBackOnReorder`: かどうかを、リストが再配置されるたびに、ポストバックを作成するには

コントロールの適切なマークアップを次に示します。

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

内で、`ReorderList`を使用してコントロール、データ ソースから特定のデータをバインドすることがあります、`Eval()`メソッド。

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

ページで任意の位置にあるラベルは最後の順番変更が発生したときに情報を保持します。

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

このラベルは、ポストバックを処理、サーバー側コード内のテキストが入力されます。

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

最後に、ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`ページにコントロールを配置する必要があります。

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![ポストバックをトリガーするそれぞれの並べ替え](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

ポストバックをトリガーするそれぞれの並べ替え ([フルサイズのイメージを表示するをクリックして](using-postbacks-with-reorderlist-cs/_static/image3.png))

>[!div class="step-by-step"]
[次へ](drag-and-drop-via-reorderlist-cs.md)
