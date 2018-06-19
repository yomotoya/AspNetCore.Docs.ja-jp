---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: ルート制約 (VB) を作成する |Microsoft ドキュメント
author: StephenWalther
description: このチュートリアルでは、Stephen Walther は、正規表現によるルート制約を作成することで、ブラウザーが一致のルートを要求する方法を制御する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f50b371ac679218b06c4848e6d33516d29d3a82
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871221"
---
<a name="creating-a-route-constraint-vb"></a>ルート制約 (VB) を作成します。
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> このチュートリアルでは、Stephen Walther は、正規表現によるルート制約を作成することで、ブラウザーが一致のルートを要求する方法を制御する方法について説明します。


ルート制約を使用すると、特定のルートに一致するブラウザーの要求を制限できます。 ルート制約を指定するのに正規表現を使用することができます。

たとえば、ルート、Global.asax ファイルで一覧表示する 1 で定義したとします。

**1 - Global.asax.vb を一覧表示します。**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

1 を一覧表示するには、製品をという名前のルートが含まれます。 製品のルートを使用して、2 のリストに含まれている ProductController にブラウザーの要求をマップすることができます。

**2 - Controllers\ProductController.vb を一覧表示します。**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

製品のコント ローラーによって公開される Details() アクションが productId という 1 つのパラメーターを受け入れることに注意してください。 このパラメーターは、整数値です。

1 のリストで定義されたルートは、次の Url のいずれかと一致します。

- /Product/23
- /製品/7

残念ながら、ルートには、次の Url は一致も。

- /Product/blah
- /Product/apple

Details() アクションには、整数パラメーターが必要ですが、ためにの整数値以外のものを含む要求を行うとエラーが発生します。 など、ブラウザーに URL/Product/apple を入力する場合は、図 1 にエラー ページが取得されます。


[![[新しいプロジェクト] ダイアログ ボックス](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**図 01**: 分解ページが表示されて ([フルサイズのイメージを表示するをクリックして](creating-a-route-constraint-vb/_static/image2.png))


何かを行うには、適切な整数 productId を含んでいる Url とのみ一致です。 ルートに一致する Url を制限するのにルートを定義するときに制約を使用できます。 リスト 3 に変更された製品ルートには、整数とのみ一致する正規表現の制約が含まれています。

**3 - Global.asax.vb を一覧表示します。**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

正規表現 \d+ では、1 つ以上の整数と一致します。 この制約により、次の Url に一致する製品ルート。

- /Product/3
- /Product/8999

ただし、次の Url ではありません。

- /Product/apple
- /製品

これらのブラウザーの要求を別のルートによって処理されますか、一致のルートがない場合、*リソースが見つかりませんでした*エラーが返されます。

> [!div class="step-by-step"]
> [前へ](creating-custom-routes-vb.md)
> [次へ](creating-a-custom-route-constraint-vb.md)
