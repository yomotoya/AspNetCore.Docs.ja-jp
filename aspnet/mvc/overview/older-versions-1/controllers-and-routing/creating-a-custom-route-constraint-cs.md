---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: "カスタム ルート制約 (c#) を作成する |Microsoft ドキュメント"
author: StephenWalther
description: "Stephen Walther では、カスタム ルート制約を作成する方法について説明します。 単純な実装により、ルートを防止するカスタムの制約に一致する w."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: c31ba3382b9dbe22a6826b9f858944c223efdd9d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-route-constraint-c"></a>カスタム ルート制約 (c#) を作成します。
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther では、カスタム ルート制約を作成する方法について説明します。 ルートがリモート コンピューターからブラウザーの要求が行われたときに一致することを防止する単純なカスタム制約を実装します。


このチュートリアルの目的では、カスタム ルート制約を作成する方法を示しています。 カスタム ルート制約では、ルートがいくつかのカスタム条件が一致しない限りに一致することを防止することができます。

このチュートリアルでは、Localhost ルート制約を作成します。 ローカル ホスト ルートの制約には、ローカル コンピューターからの要求のみと一致します。 インターネットからリモート要求は一致していません。

カスタム ルート制約を実装するには、IRouteConstraint インターフェイスを実装します。 これは、1 つのメソッドを記述する非常に単純なインターフェイスです。

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

メソッドは、ブール値を返します。 場合は false を返す、制約に関連付けられているルートがブラウザーの要求に一致しません。

Localhost の制約は、1 のリストに含まれます。

**1 - LocalhostConstraint.cs を一覧表示します。**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

HttpRequest クラスによって公開される IsLocal プロパティのリスト 1 の制約を活用します。 このプロパティは、要求の IP アドレスが、127.0.0.1、または要求の ip アドレスがサーバーの IP アドレスと同じ場合に true を返します。

Global.asax ファイルで定義されているルート内のユーザー設定の制約を使用するとします。 Global.asax ファイルを一覧表示する 2 では、ローカル サーバーから要求を作成する場合を除き、[管理] ページを要求しないようにすべてのユーザー Localhost 制約を使用します。 たとえば、リモート サーバーから行われたときに、/Admin/DeleteAll の要求は失敗します。

**2 - Global.asax を一覧表示します。**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

Localhost の制約は、管理者ルートの定義で使用されます。 このルートは、リモート ブラウザーの要求で一致しません。 したがって、Global.asax で定義されている他のルートが同じ要求に一致があります。 制約により、特定のルートが要求に一致して、いないすべてのルートが Global.asax ファイルで定義されていることを理解しておく必要があります。

既定のルート コメント アウトされています、Global.asax ファイルを一覧表示する 2 からに注意してください。 既定のルートを含める場合、既定のルートは、管理コント ローラーに対する要求を一致します。 その場合は、場合でも、その要求が管理者ルートを一致せず、リモート ユーザーは管理コント ローラーのアクションを呼び出すまだでした。

>[!div class="step-by-step"]
[前へ](creating-a-route-constraint-cs.md)
[次へ](asp-net-mvc-controller-overview-vb.md)
