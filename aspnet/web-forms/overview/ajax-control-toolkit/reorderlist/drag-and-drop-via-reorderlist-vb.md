---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: ドラッグ アンド ドロップ ReorderList (VB) 経由で |Microsoft ドキュメント
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 99f47b969dc75efeec8485254d311c93dc0b5d35
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="drag-and-drop-via-reorderlist-vb"></a>ドラッグ アンド ドロップ ReorderList (VB) を使用して
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> AJAX コントロールのツールキットで ReorderList コントロール一覧に表示される、ユーザーがドラッグ アンド ドロップで並べ替えることができます。 リストの現在の順序は、サーバー上に保持いてはいけない。


## <a name="overview"></a>概要

`ReorderList` AJAX コントロール Toolkit でコントロール一覧に表示される、ユーザーがドラッグ アンド ドロップで並べ替えることができます。 リストの現在の順序は、サーバー上に保持いてはいけない。

## <a name="steps"></a>手順

`ReorderList`コントロールは、データベースから、リストへのデータ バインディングをサポートします。 何より、データ ストアに、リスト要素の順序変更を書き込んでいますもサポートします。

このサンプルでは、データ ストアとして Microsoft SQL Server 2005 Express Edition を使用します。 データベースは、express エディションを含む、Visual Studio のインストールの省略可能な (および空き) 部分です。 個別のダウンロードとして入手できますも[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)です。 このサンプルのものと、SQL Server 2005 Express Edition のインスタンスが呼び出される`SQLEXPRESS`web サーバーと同じコンピューター上に存在し、これは既定の設定にもします。 セットアップが異なっている場合は、データベースの接続情報を調整する必要です。

データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) )。 サーバーへの接続をダブルクリックして`Databases`新しいデータベースを作成し、(を右クリックして`New Database`) と呼ばれる`Tutorials`です。

このデータベースでと呼ばれる新しいテーブルを作成`AJAX`次の 4 つの列を含む。

- `id` (主キー、整数、id は not NULL)
- `char` (char (1)、NULL)
- `description` (varchar(50), NULL)
- `position` (int, NULL)


[![AJAX のテーブルのレイアウト](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

AJAX のテーブルのレイアウト ([フルサイズのイメージを表示するをクリックして](drag-and-drop-via-reorderlist-vb/_static/image3.png))


次に、いくつかの値をテーブルを入力します。 なお、`position`列は、要素の並べ替え順序を保持します。


[![AJAX テーブル内の最初のデータ](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

AJAX テーブル内の最初のデータ ([フルサイズのイメージを表示するをクリックして](drag-and-drop-via-reorderlist-vb/_static/image6.png))


次の手順を生成する必要があります、`SqlDataSource`新しいデータベースとそのテーブルとの通信を制御します。 データ ソースをサポートする必要があります、`SELECT`と`UPDATE`SQL コマンド。 リストの要素の順序が変更された後で、ときに、`ReorderList`コントロールは、データ ソースの 2 つの値を自動的に送信する`Update`コマンド: 新しい位置と、要素の ID。 そのため、データ ソースのニーズ、`<UpdateParameters>`これら 2 つの値についてのセクション。

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

`ReorderList`コントロールは、次の属性を設定する必要があります。

- `AllowReorder`: かどうか、一覧の項目を並べ替えることができます
- `DataSourceID`: データ ソースの ID
- `DataKeyField`: データ ソースの主キー列の名前
- `SortOrderField`: データ ソースを提供する列、リスト項目の並べ替え順序

`<DragHandleTemplate>`と`<ItemTemplate>`セクションでは、リストのレイアウトを微調整することができます。 また、データ バインドが可能なを使用して、`Eval()`メソッドを次に示すよう。

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

次の CSS スタイル情報 (で参照されている、`<DragHandleTemplate>`のセクションで、`ReorderList`コントロール)、マウス ポインターをドラッグ ハンドル上に置くことと適切に変わることを確認します。

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

最後に、`ScriptManager`コントロール、ページを ASP.NET AJAX を初期化します。

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

ブラウザーでこの例を実行し、少しリスト項目を再配置します。 次に、ページの再読み込みや、データベースを参照してください。 変更後の位置が維持されているしの値でも反映されます、`position`ことと、データベース内の列せず、すべてのコードは、マークアップを使用してだけです。


[![新しい一覧項目の順序に従って、データベースで変更データ](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

新しいリストに従って、データベースで変更データ項目の順序 ([フルサイズのイメージを表示するをクリックして](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [前へ](using-postbacks-with-reorderlist-vb.md)
