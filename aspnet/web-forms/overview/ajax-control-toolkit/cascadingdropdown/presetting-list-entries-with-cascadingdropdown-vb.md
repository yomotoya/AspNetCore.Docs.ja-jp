---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: 一覧のエントリ CascadingDropDown (VB) を事前に設定する |Microsoft ドキュメント
author: wenz
description: CascadingDropDown コントロール、AJAX コントロール Toolkit でコントロールを拡張する DropDownList anoth 内の値が 1 つの DropDownList 負荷の変更に関連付けられているようにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: f74e6ac80b756240870d9406a03db11c610093aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a>一覧のエントリ CascadingDropDown (VB) を事前に設定します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)

> AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。 少しのコードがデータが動的に読み込まれた後にリスト要素があらかじめ選択されていることもできます。


## <a name="overview"></a>概要

AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。 (たとえば、1 つのリストが状態、私たちの一覧を提供し、[次へ] の一覧は状態にある主要都市で埋められます)。少しのコードがデータが動的に読み込まれた後にリスト要素があらかじめ選択されていることもできます。

## <a name="steps"></a>手順

ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

その後、DropDownList コントロールが必要です。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

このリストの CascadingDropDown extender が追加され、web サービスの URL とメソッドの情報。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

CascadingDropDown extender し、非同期的に呼び出します次のメソッド シグネチャを持つ web サービス。

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

メソッドは、型 CascadingDropDown 値の配列を返します。 リスト項目のキャプションとし、値型のコンス トラクターが最初に必要です (HTML`value`属性)。 3 番目の引数が設定されている場合、true、リストに要素が自動的に、ブラウザーで選択します。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

ブラウザーでページの読み込みがいっぱいに 3 つのベンダーと、ドロップダウン リスト、もう 1 つが既に選択されています。


[![リストが入力され、自動的に事前に選択](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)

リストが入力され、自動的に事前に選択された ([フルサイズのイメージを表示するをクリックして](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](using-cascadingdropdown-with-a-database-vb.md)
> [次へ](using-auto-postback-with-cascadingdropdown-vb.md)
