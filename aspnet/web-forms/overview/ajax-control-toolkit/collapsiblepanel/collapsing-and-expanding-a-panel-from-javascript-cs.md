---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: 折りたたみと展開 (c#) の JavaScript からパネル |Microsoft Docs
author: wenz
description: ASP.NET AJAX Control Toolkit の CollapsiblePanel コントロール パネルの拡張し、その内容を折りたたむし、展開する機能が提供されますをしています.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 325194fc1f980714ea2fc26cfaa13d86ae9d2f89
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835260"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>折りたたみと展開 (c#) の JavaScript からパネル
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> ASP.NET AJAX Control Toolkit の CollapsiblePanel コントロールはパネルを拡張し、その内容を折りたたむし、もう一度展開する機能を提供します。 これら 2 つのアクションは、カスタム JavaScript コードからトリガーすることもできます。


## <a name="overview"></a>概要

ASP.NET AJAX Control Toolkit の CollapsiblePanel コントロールはパネルを拡張し、その内容を折りたたむし、もう一度展開する機能を提供します。 これら 2 つのアクションは、カスタム JavaScript コードからトリガーすることもできます。

## <a name="steps"></a>手順

まず、新しい ASP.NET ページを作成し、含める、`ScriptManager`内、`<form>`要素。 これは、Control Toolkit に必要な ASP.NET AJAX ライブラリを読み込みます。

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

次に、折りたたみ/展開効果がわかるようにテキスト パネルを作成します。

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

ご覧のように、パネルは、CSS クラスを次に示します (と基本的に背景色とパネルの幅を定義します) を参照します。

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

`CollapsiblePanelExtender`コントロールに必要な`TargetControlID`ツールキットは、どのパネルを折りたたむか、要求時に展開を認識できるように属性します。

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

残念ながら、エクステンダー現在公開しません特定の API、パネルの展開または折りたたみが一部の文書化されていないメソッドが操作を行います。 まず、クライアント側の JavaScript を折りたたんだり展開したり、パネルの内容をトリガーし、ページに 3 つの HTML ボタンを追加します。

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

クライアント側の JavaScript コード (を使い始める`<script type="text/javascript">`)、`$find()`メソッドを使用する必要がありますにアクセスする、`CollapsiblePanelExtender`します。 `$find("cpe")` 参照を返します。 そこから、特定のメソッドは、手元のタスクを解決します。

メソッド (拡大) を開くため、パネルが呼び出されます`_doOpen()`; 次のコードで実装、`doOpen()`最初のボタンがクリックされたときに、関数が呼び出されます。

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

閉じる、または、パネルを折りたたみ、`_doClose()`メソッドを実行する必要があります。 したがって、ユーザーは、2 番目のボタンをクリックすると、次の JavaScript コードが呼び出されます。

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

3 番目のボタンは、パネルの状態を切り替えます。 から折りたたまれているを展開すると、その逆。 `CollapsiblePanelExtender`公開、`toggle()`はまさにメソッド: パネルの状態を反転させます。 別の方法もあります (によって内部的に使用される、`toggle()`メソッド):`get_Collapsed()`のメソッド、`CollapsiblePanelExtender()`パネルが折りたたまれているかどうかどうかがわかります。 いずれかを展開し、この関数の戻り値の値に応じて、パネルは、(`_doOpen()`メソッド)、または折りたたまれている (`_doClose()`) メソッド。

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![3 番目のボタンは、パネルの状態を変更しますから一番前と展開のために折りたたむ。](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

3 番目のボタンは、パネルの状態を変更: から一番前と展開のために折りたたむ ([フルサイズの画像を表示する をクリックします](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [次へ](collapsing-and-expanding-a-panel-from-javascript-vb.md)
