---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: CascadingDropDown (c#) で一覧のエントリを事前設定する |Microsoft Docs
author: wenz
description: CascadingDropDown コントロール、AJAX Control Toolkit では、anoth 内の値が 1 つの DropDownList の読み込みの変更に関連付けられているように DropDownList コントロールを拡張しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 56093ad098d69039aafa9b4e6ba4e4c2ad91e49b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362000"
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a>CascadingDropDown (c#) で一覧のエントリを事前に設定します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)

> CascadingDropDown コントロール、AJAX Control Toolkit では、もう 1 つの DropDownList の値が 1 つの DropDownList の読み込みの変更に関連付けられているように、DropDownList コントロールを拡張します。 ほんの少しのコード リストの要素は、データが動的に読み込まれると既に選択されてことができます。


## <a name="overview"></a>概要

CascadingDropDown コントロール、AJAX Control Toolkit では、もう 1 つの DropDownList の値が 1 つの DropDownList の読み込みの変更に関連付けられているように、DropDownList コントロールを拡張します。 (たとえば、1 つのリストは、私たちの状態の一覧を提供します。 し、[次へ] の一覧は、その状態の主な都市で埋められます)。ほんの少しのコード リストの要素は、データが動的に読み込まれると既に選択されてことができます。

## <a name="steps"></a>手順

ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

次に、DropDownList コントロールが必要です。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

このリストの CascadingDropDown エクステンダーが追加になり、web サービスの URL とメソッドの情報を提供します。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

CascadingDropDown エクステンダー非同期的に呼び出して、次のメソッド シグネチャを持つ web サービス。

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

CascadingDropDown 値の型の配列を返します。 リスト項目のキャプションとし、値型のコンス トラクターが最初に必要です (HTML`value`属性)。 3 番目の引数が設定されている場合は true、リストに要素が、ブラウザーで自動的に選択します。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

ブラウザーでページの読み込みが挿入されますをドロップダウン リストに 3 つのベンダーでは、あらかじめ選択されている 2 つ目。


[![リストが入力され、自動的にあらかじめ選択されています](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)

リストが入力され、自動的にあらかじめ選択されています ([フルサイズの画像を表示する をクリックします](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](using-cascadingdropdown-with-a-database-cs.md)
> [次へ](using-auto-postback-with-cascadingdropdown-cs.md)
