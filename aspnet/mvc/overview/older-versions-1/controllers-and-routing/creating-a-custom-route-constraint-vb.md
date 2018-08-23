---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: カスタム ルート制約 (VB) を作成する |Microsoft Docs
author: StephenWalther
description: Stephen Walther では、カスタム ルート制約を作成する方法を示します。 単純な実装のルートがされたりすることを防止するカスタムの制約に一致する w.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: d088380152adcb025857176b4396cab48fa64b66
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835780"
---
<a name="creating-a-custom-route-constraint-vb"></a>カスタム ルート制約 (VB) を作成します。
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther では、カスタム ルート制約を作成する方法を示します。 ルートがリモート コンピューターからブラウザーの要求が行われたときに一致することを防止する単純なカスタム制約を実装します。


このチュートリアルの目的は、カスタム ルート制約を作成する方法を示すです。 カスタム ルート制約では、ルートがいくつかのカスタム条件が一致しない限り、一致することを防止することができます。

このチュートリアルでは、Localhost のルート制約を作成します。 Localhost のルート制約では、ローカル コンピューターからの要求のみと一致します。 インターネット経由でのリモート要求は一致していません。

カスタム ルート制約を実装するには、また、IRouteConstraint インターフェイスを実装します。 これは、1 つのメソッドを記述する非常にシンプルなインターフェイスです。

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

メソッドは、ブール値を返します。 False を返す場合、制約に関連付けられているルートは、ブラウザーの要求に一致しません。

リスト 1 で、Localhost の制約が含まれています。

**1 - LocalhostConstraint.vb を一覧表示します。**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

リスト 1 で制約では、HttpRequest クラスによって公開される IsLocal プロパティを利用します。 このプロパティは、要求の IP アドレスが、127.0.0.1、または要求の ip アドレスがサーバーの IP アドレスと同じ場合に true を返します。

Global.asax ファイルで定義されているルート内のカスタムの制約を使用するとします。 リスト 2 で Global.asax ファイルでは、Localhost の制約を使用して、ローカル サーバーから要求を行った場合を除き、[管理] ページを要求できないようにします。 たとえば、リモート サーバーから行われたときに、/Admin/DeleteAll の要求は失敗します。

**2 - Global.asax を一覧表示します。**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Localhost の制約は、管理者のルートの定義に使用されます。 このルートは、リモートのブラウザー要求によってとも一致しません。 ただし、Global.asax で定義されている他のルートが同じ要求を一致することに注意してください。 制約は、要求を照合から特定のルートを防止し、いないすべてのルートが Global.asax ファイルで定義されていることを理解しておく必要があります。

既定のルート コメント アウトされているリスト 2 で、Global.asax ファイルからに注意してください。 既定のルートを含める場合、既定のルートは、管理コントローラの要求と一致は。 その場合は、管理者ルートは一致しません、要求でも、リモート ユーザーは管理者コント ローラーのアクションを呼び出しても場合があります。

> [!div class="step-by-step"]
> [前へ](creating-a-route-constraint-vb.md)
