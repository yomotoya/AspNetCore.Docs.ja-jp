---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: 評価コントロール (c#) を作成する |Microsoft ドキュメント
author: wenz
description: 多くの web サイト、e コマース、コミュニティ サイトへのユーザーは、レート アーティクルまたは項目を提供します。 通常、いくつかのコーディング作業が必要ですが、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a48cf0ed9402e2875e87ba7bdb76afc5f501a670
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879596"
---
<a name="creating-a-rating-control-c"></a>評価コントロール (c#) を作成します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)

> 多くの web サイト、e コマース、コミュニティ サイトへのユーザーは、レート アーティクルまたは項目を提供します。 通常、いくつかのコーディング作業が必要ですが、コントロール Toolkit、破棄する必要はします。


## <a name="overview"></a>概要

多くの web サイト、e コマース、コミュニティ サイトへのユーザーは、レート アーティクルまたは項目を提供します。 通常、いくつかのコーディング作業が必要ですが、コントロール Toolkit、破棄する必要はします。

## <a name="steps"></a>手順

イメージの 2 つ (以上) の種類を最初に、すべての必要: 塗りつぶしのいずれかを評価し、アイテムを空の評価の 1 つです。 評価の項目は、通常、星または、スマイリーです。 このシナリオでは、このチュートリアルではソース コードのダウンロードの一部として、次の 3 つのファイル、smiley.png と empty.png およびスマイリー done.png を検索します。

新しい ASP.NET ファイルを作成し、追加することで開始、`ScriptManager`を制御します。

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

次に、追加、`Rating`から ASP.NET AJAX コントロール Toolkit コントロール。 次の属性は、この例に設定する必要があります。

- `CurrentRating` 使用する初期の評価
- `MaxRating` 最大年齢区分
- `EmptyStarCssClass` 評価アイテム (星印) は空にするときに使用する CSS クラス
- `FilledStarCssClass` 評価項目 (星印) は、入力時に使用する CSS クラス
- `StarCssClass` 表示状態を使用する CSS クラス
- `WaitingStarCssClass` 星の評価は、サーバーに送信中に使用する CSS クラス

5 つの評価のコントロールを作成するマークアップは、次を [なし] が入力された最初の項目 (スマイリー)。

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

3 つの参照先の CSS クラス必要があります、適切なイメージ ファイルを表示するために CSS を使用して簡単であります。

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

3 つのイメージの高さと幅を指定することは、それ以外の場合が正しく表示を少し messed 確認します。

最後に、評価の結果するユーザーに表示 (か、少なくとも、データベースに保存) します。 したがって評価フォームをサーバーにポスト バックするには、テキスト メッセージと送信ボタンの出力のラベルを追加します。

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

サーバー側のコードでアクセスを使用して評価制御その`ID`し、アクセス、`CurrentRating`プロパティは、この例では 0 ~ 5 の値で、選択した評価項目の数。

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

ページを保存し、ブラウザーに読み込むことです。 (最初は空) の評価の項目をポイントすると、JavaScript の効果が発生します。 評価の変更。 星のセットをクリックすると、現在の評価が保持されます。 最後に、フォームを送信するときに、サーバー側のコードは、選択した評価を出力します。


[![最小限のコードで評価システムを作成します。](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)

最小限のコードで評価システムを作成する ([フルサイズのイメージを表示するをクリックして](creating-a-rating-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [次へ](creating-a-rating-control-vb.md)
