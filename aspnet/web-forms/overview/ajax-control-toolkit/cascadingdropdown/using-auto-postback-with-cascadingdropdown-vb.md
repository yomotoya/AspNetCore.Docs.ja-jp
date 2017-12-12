---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: "CascadingDropDown (VB) での自動ポストバックの使用 |Microsoft ドキュメント"
author: wenz
description: "CascadingDropDown コントロール、AJAX コントロール Toolkit でコントロールを拡張する DropDownList anoth 内の値が 1 つの DropDownList 負荷の変更に関連付けられているようにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: f8059f44b4efbf59ebe7b3d2fd5400e886642a90
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a>CascadingDropDown (VB) での自動ポストバックの使用
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)

> AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。 ただし、ASP CascadingDropDown コントロールを使用する場合。NET の DropDownList のコントロールの AutoPostBack 機能は動作しません、自体 (不要な) のポストバックを生成、一覧に非同期的にデータを読み込むためです。 一部の JavaScript コードでは、この特殊効果を回避できます。


## <a name="overview"></a>概要

AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。 (たとえば、1 つのリストが状態、私たちの一覧を提供し、[次へ] の一覧は状態にある主要都市で埋められます)。ただし、ASP CascadingDropDown コントロールを使用する場合。NET の DropDownList のコントロールの AutoPostBack 機能は動作しません、自体 (不要な) のポストバックを生成、一覧に非同期的にデータを読み込むためです。 一部の JavaScript コードでは、この特殊効果を回避できます。

## <a name="steps"></a>手順

ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、 &lt; `form` &gt;要素)。

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

その後、DropDownList コントロールが必要です。

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

このリストの CascadingDropDown extender が追加され、web サービスの URL とメソッドの情報。

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

CascadingDropDown extender し、非同期的に呼び出します次のメソッド シグネチャを持つ web サービス。

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

メソッドは、型 CascadingDropDown 値の配列を返します。 リスト項目のキャプションとし、値型のコンス トラクターが最初に必要です (HTML`value`属性)。

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

ブラウザーでページの読み込みがいっぱいに 3 つのベンダーと、ドロップダウン リスト、もう 1 つが既に選択されています。 また、ASP.NET、定義、 `__doPostBack()` JavaScript メソッドです。 ページが読み込まれると、この JavaScript の呼び出しがのみ内にある要素には、ドロップダウン リストに追加されます。 リスト内の要素がない場合、コントロール ツールキットが現在を読み込んで、JavaScript コードが、タイムアウト時間を使用し、0.5 秒でもう一度試みます。

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

これにより、ポストバックは、一覧内に実際に要素が、ユーザーはエントリを選択するときにのみ実行されます。


[![ポストバック リスト要素を選択すると、します。](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)

ポストバック リスト要素を選択すると、([フルサイズのイメージを表示するをクリックして](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))

>[!div class="step-by-step"]
[前へ](presetting-list-entries-with-cascadingdropdown-vb.md)
