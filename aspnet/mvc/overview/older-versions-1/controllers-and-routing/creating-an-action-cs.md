---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: アクションを作成する (c#) |Microsoft Docs
author: microsoft
description: ASP.NET MVC のコント ローラーに新しいアクションを追加する方法について説明します。 メソッドを操作するための要件について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: d6f981201b93e3650b18f112dd9330f1470aa573
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372499"
---
<a name="creating-an-action-c"></a>アクションを作成する (c#)
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET MVC のコント ローラーに新しいアクションを追加する方法について説明します。 メソッドを操作するための要件について説明します。


このチュートリアルの目的では、新しいコント ローラーのアクションを作成する方法について説明します。 アクション メソッドの要件について説明します。 メソッドがアクションとして公開されないようにする方法を学びます。

## <a name="adding-an-action-to-a-controller"></a>コント ローラー アクションの追加

コント ローラーに新しいアクションを追加するには、コント ローラーに新しいメソッドを追加します。 たとえば、リスト 1 で、コント ローラーには、Index() という名前のアクションと SayHello() という名前のアクションが含まれています。 どちらの方法は、アクションとして公開されます。

**1 - controllers \homecontroller.cs を一覧表示します。**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

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

コント ローラー クラスにパブリック メソッドを作成する必要があると、コント ローラーのアクションとしてメソッドを公開する場合は、[NonAction] 属性を使用して呼び出されているからメソッドを防ぐことができます。 たとえば、リスト 2 でコント ローラーには、[NonAction] 属性で装飾した CompanySecrets() をという名前のパブリック メソッドが含まれています。

**2 - Controllers\WorkController.cs を一覧表示します。**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

CompanySecrets() コント ローラーのアクションを起動するには、お使いのブラウザーのアドレス バーに/Work/CompanySecrets を入力しようとした場合は、図 1 で、エラー メッセージが表示されます。


[![NonAction メソッドを呼び出す](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**図 01**: NonAction メソッドを呼び出す ([フルサイズの画像を表示する をクリックします](creating-an-action-cs/_static/image2.png))。

> [!div class="step-by-step"]
> [前へ](creating-a-controller-cs.md)
> [次へ](asp-net-mvc-routing-overview-vb.md)
