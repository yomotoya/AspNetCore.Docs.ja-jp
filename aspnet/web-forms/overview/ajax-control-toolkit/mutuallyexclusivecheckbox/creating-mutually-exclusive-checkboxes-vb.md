---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: 相互に排他的なチェック ボックス (VB) を作成 |Microsoft ドキュメント
author: wenz
description: '一連のオプションの 1 つのみが選択されるときに、オプション ボタンが通常使用されます。 ただし、欠点がある: グループ内の 1 つのオプション ボタンを選択すると、.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: cdb93a080fb62cfdc7e3ff0604a1447e2bb99071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a>相互に排他的なチェック ボックス (VB) を作成します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> 一連のオプションの 1 つのみが選択されるときに、オプション ボタンが通常使用されます。 ただし、欠点がある: グループ内の 1 つのオプション ボタンを選択するはすべてのオプション ボタンをクリックしてオフにします。 チェック ボックスは、ただしは相互に排他的でない、いつでもチェックできます。 このチュートリアルでは、両方のアプローチの最適な: チェック ボックスは相互に排他的です。


## <a name="overview"></a>概要

一連のオプションの 1 つのみが選択されるときに、オプション ボタンが通常使用されます。 ただし、欠点がある: グループ内の 1 つのオプション ボタンを選択するはすべてのオプション ボタンをクリックしてオフにします。 チェック ボックスは、ただしは相互に排他的でない、いつでもチェックできます。 このチュートリアルでは、両方のアプローチの最適な: チェック ボックスは相互に排他的です。

## <a name="steps"></a>手順

ASP.NET AJAX コントロール Toolkit には、MutuallyExclusiveCheckBox extender が含まれています。 これにより、プログラマは、グループ名にすべてのチェック ボックスを割り当てる (`Key`属性)。 同じグループ内のすべてのチェック ボックスは、1 つだけを一度に 1 つ選択する可能性があります。

新しい ASP.NET ページ上の 2 つのチェック ボックスを配置することから始めましょう。 以上、存在することができますですが、プリンシパルを示すために十分 2 つの。

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

両方のチェック ボックス、ページで MutuallyExclusiveCheckBoxExtender コントロールに配置されている必要があります。 両方のキー属性は、HTML ラジオ ボタンの要素の属性は、所属するグループを示すと同じである必要があります値と同様に、同じ値を持つ必要があります。 Extender の TargetControlID プロパティは、チェック ボックスの ID を指します。

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

最後に、ASP.NET AJAX を含める`ScriptManager`ASP.NET AJAX コントロール Toolkit のすべての要素で必要となります。

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

保存し、ページの実行: を確認し、両方のチェック ボックスをオフにただしない時に両方のチェック ボックス チェックできます。


[![一度に 1 つだけのチェック ボックスを選択することができます。](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

一度に 1 つだけのチェック ボックスを選択することができます ([フルサイズのイメージを表示するをクリックして](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](creating-mutually-exclusive-checkboxes-cs.md)
