---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: アクション (VB) の作成 |Microsoft ドキュメント
author: microsoft
description: ASP.NET MVC のコント ローラーに新しいアクションを追加する方法を説明します。 メソッドをアクションの要件について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: c77e4738444c61d60bdd78a50b36f98be41fc271
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867893"
---
<a name="creating-an-action-vb"></a>アクション (VB) の作成
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET MVC のコント ローラーに新しいアクションを追加する方法を説明します。 メソッドをアクションの要件について説明します。


このチュートリアルの目的では、新しいコント ローラーのアクションを作成する方法について説明します。 アクション メソッドの要件について説明します。 メソッドがアクションとして公開されないようにする方法についても説明します。

## <a name="adding-an-action-to-a-controller"></a>コント ローラーにアクションを追加します。

コント ローラーに新しいアクションを追加するには、コント ローラーに新しいメソッドを追加します。 たとえば、コント ローラー 1 の一覧表示するには、Index() をという名前のアクションと SayHello() をという名前のアクションが含まれます。 どちらの方法は、操作として公開されます。

**1 - Controllers\HomeController.vb を一覧表示します。**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

メソッドは、アクションとして universe に公開するために、特定の要件を満たす必要があります。

- メソッドはパブリックである必要があります。
- メソッドは、静的メソッドにすることはできません。
- メソッドは、拡張メソッドにすることはできません。
- メソッドは、コンス トラクター、getter、または set アクセス操作子にすることはできません。
- メソッドは、オープン ジェネリック型を持つことはできません。
- メソッドはコント ローラーの基本クラスのメソッドではありません。
- メソッドを含めることはできません**ref**または**アウト**パラメーター。

コント ローラーのアクションの戻り値の型に関する制限がないことに注意してください。 コント ローラーのアクションは、文字列、DateTime のインスタンス Random クラス、または void を返すことができます。 ASP.NET MVC フレームワークはアクションの結果を文字列にではない戻り値の型に変換し、ブラウザーに文字列を表示します。

これらの要件をコント ローラーが違反していない任意のメソッドを追加すると、メソッドは、コント ローラーのアクションとして公開されます。 ここで注意してください。 コント ローラーのアクションは、インターネットに接続されているすべてのユーザーが呼び出すことができます。 、たとえば、作成しないで DeleteMyWebsite() コント ローラーのアクション。

## <a name="preventing-a-public-method-from-being-invoked"></a>パブリック メソッドが呼び出されていることを妨げてください。

かどうかは、コント ローラー クラスにパブリック メソッドを作成する必要があります、およびコント ローラーのアクションとしてメソッドを公開するメソッドを使用して呼び出されているように、 &lt;NonAction&gt;属性。 たとえば、2 の一覧で、コント ローラーで装飾されて CompanySecrets() をという名前のパブリック メソッドが含まれます。、 &lt;NonAction&gt;属性。

**2 - Controllers\WorkController.vb を一覧表示します。**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

/Work/CompanySecrets をお使いのブラウザーのアドレス バーに入力して CompanySecrets() コント ローラーのアクションを呼び出すしようとする場合は、図 1 に、エラー メッセージを表示します。


[![NonAction メソッドを呼び出す](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**図 01**: NonAction メソッドを呼び出す ([フルサイズのイメージを表示するをクリックして](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [前へ](creating-a-controller-vb.md)
> [次へ](aspnet-mvc-controllers-overview-cs.md)
