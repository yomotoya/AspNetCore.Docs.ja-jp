---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: ドラッグ アンド ドロップ ReorderList (VB) を使用して |Microsoft Docs
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: d8b10ecd4c061082aba5cefe9d92b8616e9f40e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389164"
---
<a name="drag-and-drop-via-reorderlist-vb"></a>ドラッグ アンド ドロップで ReorderList (VB)
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> ReorderList コントロール、AJAX Control Toolkit では、ユーザーがドラッグ アンド ドロップを使用して並べ替えることができる一覧を提供します。 リストの現在の順序は、サーバーに保存されます。


## <a name="overview"></a>概要

`ReorderList` AJAX Control Toolkit でコントロールには、ユーザーがドラッグ アンド ドロップを使用して並べ替えることができる一覧が用意されています。 リストの現在の順序は、サーバーに保存されます。

## <a name="steps"></a>手順

`ReorderList`コントロールが一覧に、データベースからデータをバインディングをサポートします。 何よりすばらしいは、データ ストアにリストの要素の順序の変更の書き込みもサポートしています。

このサンプルでは、データ ストアとして Microsoft SQL Server 2005 Express Edition を使用します。 データベースは、express edition を含め、Visual Studio のインストールのオプション (および無料) 部分です。 個別のダウンロードとして入手[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。 このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。 場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。

データベースを設定する最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) )。 サーバーへの接続をダブルクリックして`Databases`新しいデータベースを作成し、(右クリックし、 `New Database`) と呼ばれる`Tutorials`します。

という新しいテーブルを作成すると、このデータベースに`AJAX`次の 4 つの列を含む。

- `id` (プライマリ キー、整数、identity、いない NULL)
- `char` (char (1)、NULL)
- `description` (varchar (50)、NULL)
- `position` (int, NULL)


[![AJAX のテーブルのレイアウト](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

AJAX のテーブルのレイアウト ([フルサイズの画像を表示する をクリックします](drag-and-drop-via-reorderlist-vb/_static/image3.png))。


次に、表に、いくつかの値を設定します。 なお、`position`列は、要素の並べ替え順序を保持します。


[![AJAX の表に、初期データ](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

AJAX の表に、初期データ ([フルサイズの画像を表示する をクリックします](drag-and-drop-via-reorderlist-vb/_static/image6.png))。


次の手順を生成する必要があります、`SqlDataSource`新しいデータベースとそのテーブルでの通信を制御します。 データ ソースをサポートする必要があります、`SELECT`と`UPDATE`SQL コマンド。 リストの要素の順序が変更された後で、ときに、`ReorderList`コントロールは、データ ソースの 2 つの値を自動的に送信する`Update`コマンド: 新しい位置と、要素の ID。 そのため、データ ソースのニーズ、`<UpdateParameters>`これら 2 つの値のセクション。

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

`ReorderList`コントロールは、次の属性を設定する必要があります。

- `AllowReorder`: かどうか、リスト項目を並べ替えることができます
- `DataSourceID`: データ ソースの ID
- `DataKeyField`: データ ソースに主キー列の名前
- `SortOrderField`リスト項目の並べ替え順序を提供するデータ ソース列

`<DragHandleTemplate>`と`<ItemTemplate>`セクションでは、リストのレイアウトを微調整することができます。 また、データ バインドが可能なを使用して、`Eval()`メソッド、ご覧のとおり。

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

次の CSS スタイル情報 (で参照されている、`<DragHandleTemplate>`のセクション、`ReorderList`コントロール) により、マウス ポインターをドラッグ ハンドルに置いたときに適切に変更します。

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

最後に、`ScriptManager`コントロールがページの ASP.NET AJAX を初期化します。

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

ブラウザーでこの例を実行し、リスト項目を少し変更します。 次に、ページを再度読み込んでか、データベースを見てください。 変更後の位置が保持されているし、は、値でも反映されます、`position`データベース内の列のマークアップを使用して、任意のコードは一切だけです。


[![新しい一覧項目の順序に従って、データベースで変更データ](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

新しい一覧に従って、データベースで変更データ項目の順序 ([フルサイズの画像を表示する をクリックします](drag-and-drop-via-reorderlist-vb/_static/image9.png))。

> [!div class="step-by-step"]
> [前へ](using-postbacks-with-reorderlist-vb.md)
