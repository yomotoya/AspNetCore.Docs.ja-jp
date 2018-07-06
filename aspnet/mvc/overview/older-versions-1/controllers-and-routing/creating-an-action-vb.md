---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: アクションを作成する (VB) |Microsoft Docs
author: microsoft
description: ASP.NET MVC のコント ローラーに新しいアクションを追加する方法について説明します。 メソッドを操作するための要件について説明します。
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 6ec69f5aa9f7a789cb533a8dc04dca952adf35d0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824047"
---
<a name="creating-an-action-vb"></a>アクションを作成する (VB)
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET MVC のコント ローラーに新しいアクションを追加する方法について説明します。 メソッドを操作するための要件について説明します。


このチュートリアルの目的では、新しいコント ローラーのアクションを作成する方法について説明します。 アクション メソッドの要件について説明します。 メソッドがアクションとして公開されないようにする方法を学びます。

## <a name="adding-an-action-to-a-controller"></a>コント ローラー アクションの追加

コント ローラーに新しいアクションを追加するには、コント ローラーに新しいメソッドを追加します。 たとえば、リスト 1 で、コント ローラーには、Index() という名前のアクションと SayHello() という名前のアクションが含まれています。 どちらの方法は、アクションとして公開されます。

**1 - Controllers\HomeController.vb を一覧表示します。**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

メソッドは、アクションとして universe に公開するために、特定の要件を満たす必要があります。

- メソッドは、パブリックである必要があります。
- メソッドは、静的メソッドにすることはできません。
- メソッドは、拡張メソッドにすることはできません。
- メソッドは、コンス トラクター、getter、または set アクセス操作子にすることはできません。
- メソッドは、オープン ジェネリック型を含めることはできません。
- メソッドは、コント ローラーの基本クラスのメソッドではありません。
- メソッドを含めることはできません**ref**または**アウト**パラメーター。

コント ローラー アクションの戻り値の型に制限がないことに注意してください。 コント ローラー アクションには、文字列、DateTime、void、Random クラスのインスタンスを返すことができます。 ASP.NET MVC フレームワークはアクションの結果を文字列にではない戻り値の型を変換し、ブラウザーに文字列を表示します。

コント ローラーにこれらの要件に違反しない任意のメソッドを追加すると、メソッドは、コント ローラーのアクションとして公開されます。 ここでも注意します。 コント ローラーのアクションは、インターネットに接続されているすべてのユーザーが呼び出すことができます。 DeleteMyWebsite() コント ローラーのアクションをたとえば、作成、操作を行います。

## <a name="preventing-a-public-method-from-being-invoked"></a>パブリック メソッドが呼び出されていることを防ぐ

コント ローラー クラスにパブリック メソッドを作成する必要があるかどうかとコント ローラーのアクションとしてメソッドを公開する、メソッドを使用して呼び出されているように、 &lt;NonAction&gt;属性。 たとえば、リスト 2 でコント ローラーで装飾した CompanySecrets() をという名前のパブリック メソッドが含まれます。、 &lt;NonAction&gt;属性。

**2 - Controllers\WorkController.vb を一覧表示します。**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

CompanySecrets() コント ローラーのアクションを起動するには、お使いのブラウザーのアドレス バーに/Work/CompanySecrets を入力しようとした場合は、図 1 で、エラー メッセージが表示されます。


[![NonAction メソッドを呼び出す](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**図 01**: NonAction メソッドを呼び出す ([フルサイズの画像を表示する をクリックします](creating-an-action-vb/_static/image2.png))。

> [!div class="step-by-step"]
> [前へ](creating-a-controller-vb.md)
> [次へ](aspnet-mvc-controllers-overview-cs.md)
