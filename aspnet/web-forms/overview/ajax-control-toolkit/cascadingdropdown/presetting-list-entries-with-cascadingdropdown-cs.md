---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: "一覧のエントリ CascadingDropDown (c#) を事前に設定する |Microsoft ドキュメント"
author: wenz
description: "CascadingDropDown コントロール、AJAX コントロール Toolkit でコントロールを拡張する DropDownList anoth 内の値が 1 つの DropDownList 負荷の変更に関連付けられているようにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: d86c5887a8b175a60c32a0970482266b7d690162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a>一覧のエントリ CascadingDropDown (c#) を事前に設定します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)

> AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。 少しのコードがデータが動的に読み込まれた後にリスト要素があらかじめ選択されていることもできます。


## <a name="overview"></a>概要

AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。 (たとえば、1 つのリストが状態、私たちの一覧を提供し、[次へ] の一覧は状態にある主要都市で埋められます)。少しのコードがデータが動的に読み込まれた後にリスト要素があらかじめ選択されていることもできます。

## <a name="steps"></a>手順

ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

その後、DropDownList コントロールが必要です。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

このリストの CascadingDropDown extender が追加され、web サービスの URL とメソッドの情報。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

CascadingDropDown extender し、非同期的に呼び出します次のメソッド シグネチャを持つ web サービス。

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

メソッドは、型 CascadingDropDown 値の配列を返します。 リスト項目のキャプションとし、値型のコンス トラクターが最初に必要です (HTML`value`属性)。 3 番目の引数が設定されている場合、true、リストに要素が自動的に、ブラウザーで選択します。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

ブラウザーでページの読み込みがいっぱいに 3 つのベンダーと、ドロップダウン リスト、もう 1 つが既に選択されています。


[![リストが入力され、自動的に事前に選択](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)

リストが入力され、自動的に事前に選択された ([フルサイズのイメージを表示するをクリックして](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))

>[!div class="step-by-step"]
[前へ](using-cascadingdropdown-with-a-database-cs.md)
[次へ](using-auto-postback-with-cascadingdropdown-cs.md)
