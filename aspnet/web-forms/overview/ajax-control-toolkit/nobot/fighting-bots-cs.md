---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Bot (c#) に対抗 |Microsoft ドキュメント
author: wenz
description: 自動のボット雇い迷惑メール、ユーザーが介入せずコメント フォームの送信と、web ログやその他の web サイトを設置します。 ASP.NET AJAX Con で NoBot 制御しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 1ea3aaa5461c2f58a927ae975568f18a34a4729b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872014"
---
<a name="fighting-bots-c"></a>戦闘 Bot (c#)
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)

> 自動のボット雇い迷惑メール、ユーザーが介入せずコメント フォームの送信と、web ログやその他の web サイトを設置します。 ASP.NET AJAX コントロールのツールキットで NoBot 管理は、具体的には、bot 対策に役立ちます。


## <a name="overview"></a>概要

自動のボット雇い迷惑メール、ユーザーが介入せずコメント フォームの送信と、web ログやその他の web サイトを設置します。 ASP.NET AJAX コントロールのツールキットで NoBot 管理は、具体的には、bot 対策に役立ちます。

## <a name="steps"></a>手順

Bot を阻止する 1 つの一般的な方法では、コンピューターおよび人間間隔を通知するな Captcha 完全に自動化されたパブリック チューリング テストを使用します。 チューリング テストがもともとテスト通信パートナーは人間またはコンピューターがあるかどうかを決定する必要な他のユーザー場所です。 Web、CAPTCHA 通常構成一部ゆがんでの文字を使用してイメージをします。 概念は人間のみが、イメージ上の文字を読み取ることができますに対しです OCR アルゴリズムは失敗します。

いくつかの長所と短所、この方法にが、これの詳細については、このチュートリアルの範囲を超えては。 ただしが、同様のアプローチを提供する ASP.NET AJAX コントロール Toolkit でコントロール:`NoBot`です。 場所と見なされます成功ほとんど迷惑メールの送信試行ブログなどの web サイトで、特によく運賃は行われず、し、CAPTCHA よりを克服する方が簡単ですがは非常に簡単に使用される、`NoBot`制御を行うことができます。

`NoBot` これらの条件の少なくとも 1 つが満たされる場合は、現在の ASP.NET web フォームのポストバックを途中受信します。

- ブラウザーが JavaScript パズルを解く失敗 (たとえば JavaScript が非アクティブ化時に)
- ユーザーには、高速にフォームが送信されました
- クライアントの IP アドレスでは、一定期間で多くの場合、フォームが送信されます。

これらの条件を確認するために、`NoBot`コントロールには、これらの属性 (すべての省略可能) が必要です。

- `ResponseMinimumDelaySeconds` 最小限のポストバック間隔 (秒)
- `CutoffWindowSeconds` 1 つの IP からのポストバックがメジャーとなる時間間隔の長さ
- `CutoffMaximumInstances` 1 時間間隔の秒の最大量

ポストバック間で次のマークアップ要求するには、少なくとも 2 秒が経過し、5 つのポストバックがあること、または 30 秒間隔に含まれる小さい。

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

通常どおりを含めるように、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、コントロール ツールキットを使用できるようにします。

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

ほとんどの確認のため`NoBot`を行ってこれらの検証の結果を確認する必要がありますが、サーバー側で発生します。 呼び出すことによってこれ行う`NoBot`の`IsValid()`メソッドです。 1 つの引数がある (として、`out`パラメーター/`ByRef`パラメーター) が型である`NoBotState`です。 チェックが失敗したときに、文字列形式には理由が含まれますと`Valid`それ以外の場合。 次のコードによるとメッセージを出力する`NoBot`'s が発生します。

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

最後に、フォームを送信して、メッセージを出力する label 要素が必要し、し終わったら!

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

このスクリプトを実行し、JavaScript を非アクティブ化または最初の 2 秒以内に、フォームを送信または送信する際、フォーム 30 秒以内に 7 回、エラー メッセージが表示されます。 ただし、このコントロールは慎重に使用すると、ユーザーの約 90 ~ 95% があるためアクティブ化された JavaScript、にしたがって 5 ~ 10% のユーザー失敗`NoBot`'s をテストします。


[![このエラー メッセージを bot によって発生する可能性があります。](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)

このエラー メッセージを bot によって発生する可能性があります ([フルサイズのイメージを表示するをクリックして](fighting-bots-cs/_static/image3.png))

> [!div class="step-by-step"]
> [次へ](fighting-bots-vb.md)
