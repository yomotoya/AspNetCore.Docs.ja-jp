---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: (C#) ReorderList でポストバックを使用 |Microsoft Docs
author: wenz
description: ReorderList コントロール、AJAX Control Toolkit では、ユーザーがドラッグ アンド ドロップを使用して並べ替えることができる一覧を提供します。 一覧の順序が変更されるたびに、po.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 4510adc4ecf6928863b035d0afe8d008968d25b0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820105"
---
<a name="using-postbacks-with-reorderlist-c"></a>(C#) ReorderList でポストバックを使用
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> ReorderList コントロール、AJAX Control Toolkit では、ユーザーがドラッグ アンド ドロップを使用して並べ替えることができる一覧を提供します。 一覧の順序が変更されるたびにポストバックの変更のサーバーに通知する必要があります。


## <a name="overview"></a>概要

`ReorderList` AJAX Control Toolkit でコントロールには、ユーザーがドラッグ アンド ドロップを使用して並べ替えることができる一覧が用意されています。 一覧の順序が変更されるたびにポストバックの変更のサーバーに通知する必要があります。

## <a name="steps"></a>手順

いくつかのデータ ソースが、`ReorderList`コントロール。 1 つは、使用する、`XmlDataSource`コントロール。

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

この XML にバインドするために、`ReorderList`コントロールと有効にするポストバックでは、次の属性を設定する必要があります。

- `DataSourceID`: データ ソースの ID
- `SortOrderField`プロパティを並べ替えるには
- `AllowReorder`: リストの要素の順序を変更するユーザーを許可するか
- `PostBackOnReorder`: リストが再配置されるたびに、ポストバックを作成するか

コントロールの適切なマークアップを次に示します。

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

内で、`ReorderList`を使用して、コントロール、データ ソースから特定のデータを連結することも、`Eval()`メソッド。

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

ページで、任意の位置にラベルは最後の順序変更が発生したときに情報を保持します。

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

このラベルは、ポストバックを処理、サーバー側コード内のテキストが入力されます。

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

最後に、ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`ページにコントロールを配置する必要があります。

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![ポストバックをトリガーするそれぞれの並べ替え](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

ポストバックをトリガーするそれぞれの並べ替え ([フルサイズの画像を表示する をクリックします](using-postbacks-with-reorderlist-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [次へ](drag-and-drop-via-reorderlist-cs.md)
