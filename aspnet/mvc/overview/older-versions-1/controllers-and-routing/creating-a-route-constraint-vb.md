---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: ルート制約 (VB) を作成する |Microsoft Docs
author: StephenWalther
description: このチュートリアルでは、Stephen Walther は、正規表現のルート制約を作成して、ブラウザーが一致するルートを要求する方法を制御する方法について説明します。
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 0a8e540a00d852d5b710bfdbf63a68f6e6d280ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832173"
---
<a name="creating-a-route-constraint-vb"></a>ルート制約 (VB) を作成します。
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> このチュートリアルでは、Stephen Walther は、正規表現のルート制約を作成して、ブラウザーが一致するルートを要求する方法を制御する方法について説明します。


ルート制約を使用すると、特定のルートに一致するブラウザーの要求を制限できます。 正規表現を使用して、ルート制約を指定することができます。

たとえば、ルート、Global.asax ファイルでリスト 1 で定義したとします。

**1 - Global.asax.vb を一覧表示します。**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

1 を一覧表示するには、製品をという名前のルートが含まれています。 製品のルートを使用して、リスト 2 に含まれる「productcontroller」ブラウザー要求をマップすることができます。

**2 - Controllers\ProductController.vb を一覧表示します。**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

製品のコント ローラーによって公開される Details() アクションが productId という 1 つのパラメーターを受け入れることに注意してください。 このパラメーターは、整数パラメーターです。

リスト 1 で定義されているルートは、次の Url のいずれかに一致します。

- /製品/23
- /製品/7

残念ながら、ルートには、次の Url は一致も。

- /製品/とおりです。
- /製品/apple

Details() アクションには、整数パラメーターが必要ですが、ため、整数値以外のものを含む要求を行うと、エラーが発生します。 たとえば、URL/Product/apple をお使いのブラウザーに入力した場合は、図 1 エラー ページを表示がされます。


[![[新しいプロジェクト] ダイアログ ボックス](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**図 01**: 展開のページが表示 ([フルサイズの画像を表示する をクリックします](creating-a-route-constraint-vb/_static/image2.png))。


本当にする内容は、のみ一致する適切な整数 productId を含む Url です。 ルートに一致する Url を制限するのにルートを定義するときに制約を使用できます。 リスト 3 で修正された製品のルートには、整数にのみ一致する正規表現の制約が含まれています。

**3 - Global.asax.vb を一覧表示します。**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

正規表現 \d+ では、1 つまたは複数の整数と一致します。 この制約により、次の Url と一致する製品ルート。

- /製品/3
- /製品/8999

次の Url ではないです。

- /製品/apple
- /製品

別のルートでこれらのブラウザー要求を処理します。 または、一致のルートがない場合、*リソースが見つかりませんでした*エラーが返されます。

> [!div class="step-by-step"]
> [前へ](creating-custom-routes-vb.md)
> [次へ](creating-a-custom-route-constraint-vb.md)
