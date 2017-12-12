---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: "ASP.NET MVC コント ローラーの概要 (c#) |Microsoft ドキュメント"
author: StephenWalther
description: "このチュートリアルでは Stephen Walther 紹介 ASP.NET MVC のコント ローラーにします。 新しいコント ローラーを作成してさまざまな種類のアクション res を返す方法を説明したとしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 9e4ca745fa068b1813e01b131d53a0199cc47d5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-controller-overview-c"></a>ASP.NET MVC コント ローラーの概要 (c#)
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> このチュートリアルでは Stephen Walther 紹介 ASP.NET MVC のコント ローラーにします。 新しいコント ローラーを作成してさまざまな種類のアクションの結果を返す方法を学びます。


このチュートリアルでは、ASP.NET MVC コント ローラー、コント ローラーのアクションおよびアクションの結果のトピックについて説明します。 このチュートリアルを完了すると、コント ローラーを使用して、訪問者が ASP.NET MVC の web サイトと対話する方法を制御する方法を理解できます。

## <a name="understanding-controllers"></a>コント ローラーの説明

MVC コント ローラーは、ASP.NET MVC の web サイトに対して行われた要求に応答して行います。 各ブラウザーの要求は、特定のコント ローラーにマップされます。 たとえば、お使いのブラウザーのアドレス バーに次の URL を入力してください。

`http://localhost/Product/Index/3`

この場合、ProductController をという名前のコント ローラーが呼び出されます。 ProductController をブラウザーの要求に対する応答を生成します。 たとえば、コント ローラーは、ブラウザーに特定のビューを返す可能性があります。 またはコント ローラーは別のコント ローラーにユーザーをリダイレクトする可能性があります。

1 を一覧表示するには、ProductController をという名前の単純なコント ローラーが含まれます。

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

一覧表示する 1 からわかるように、コント ローラー、クラス (Visual Basic .NET や c# のクラス) だけです。 コント ローラーは、基本 System.Web.Mvc.Controller クラスから派生したクラスです。 コント ローラーは、この基本クラスから継承、ためにと、コント ローラーが無料にいくつかの便利なメソッドを継承 (触れこれらのメソッドはすぐに)。

## <a name="understanding-controller-actions"></a>コント ローラーのアクションを理解します。

コント ローラーは、コント ローラーのアクションを公開します。 アクションは、ブラウザーのアドレス バーで、特定の URL を入力するときに呼び出されるコント ローラーのメソッドです。 たとえば、次の URL に対する要求を行うとします。

`http://localhost/Product/Index/3`

この場合、Index() メソッドは、ProductController クラスで呼び出されます。 Index() メソッドは、コント ローラーのアクションの例を示します。

コント ローラーのアクションは、コント ローラー クラスのパブリック メソッドである必要があります。 既定には、c# メソッドでは、プライベート メソッドです。 コント ローラー クラスに追加するすべてのパブリック メソッドがコント ローラーのアクションとして自動的に公開されることに注意して (必要がありますに関する注意してこのコント ローラーのアクションは、右側の URL をブラウザーのアドレス バーに入力するだけで、universe 内の誰でも呼び出すことができますので)。

コント ローラーのアクションを満たす必要のあるいくつかの追加要件があります。 コント ローラーのアクションとして使用されるメソッドをオーバー ロードすることはできません。 さらに、コント ローラーのアクションは、静的メソッドをすることはできません。 それを除けば、コント ローラーのアクションとしてほとんどすべてのメソッドを使用できます。

## <a name="understanding-action-results"></a>アクションの結果を理解します。

コント ローラーのアクションを返しますと呼ばれるもの、*アクション結果*です。 アクションの結果は、コント ローラーのアクションはブラウザーの要求に応答を返します。

ASP.NET MVC フレームワークには、いくつか含むアクションの結果の種類がサポートされています。

1. ViewResult - を表します HTML およびマークアップ。
2. EmptyResult のない結果を表します。
3. RedirectResult - は、新しい URL へのリダイレクトを表します。
4. JsonResult - は、AJAX アプリケーションで使用できる JavaScript Object Notation 結果を表します。
5. JavaScriptResult - は、JavaScript のスクリプトを表します。
6. ContentResult - は、テキストの結果を表します。
7. FileContentResult - は、(バイナリ コンテンツ) のダウンロード可能なファイルを表します。
8. FilePathResult - は、(パス) のダウンロード可能なファイルを表します。
9. FileStreamResult - は、(ファイル ストリーム) のダウンロード可能なファイルを表します。

これらのアクション結果のすべては、基本の ActionResult クラスから継承されます。

ほとんどの場合は、コント ローラーのアクションは、ViewResult を返します。 たとえば、インデックス コント ローラーのアクションを一覧表示する 2 では、ViewResult を返します。

**2 - Controllers\BookController.cs を一覧表示します。**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

操作が戻るとき、ViewResult、ブラウザーに HTML が返されます。 Index() メソッドを一覧表示する 2 では、ブラウザーにインデックスをという名前のビューを返します。

リスト 2 で Index() アクションが、ViewResult() を返さないことに注意してください。 代わりに、コント ローラーの基本クラスの View() メソッドが呼び出されます。 通常を返さないアクション結果直接です。 代わりに、コント ローラーの基本クラスの次のメソッドのいずれかを呼び出します。

1. ビュー - ViewResult アクションの結果を返します。
2. リダイレクト - RedirectResult アクションの結果を返します。
3. RedirectToAction - RedirectToRouteResult アクションの結果を返します。
4. RedirectToRoute - RedirectToRouteResult アクションの結果を返します。
5. Json では、JsonResult アクションの結果を返します。
6. JavaScriptResult -、JavaScriptResult を返します。
7. コンテンツ - ContentResult アクションの結果を返します。
8. ファイルのメソッドに渡される FileContentResult、FilePathResult、またはパラメーターに応じて FileStreamResult を返します。

そのため、ブラウザーにビューを返す場合には、View() メソッドを呼び出します。 1 つのコント ローラーのアクションからユーザーをリダイレクトする場合は、RedirectToAction() メソッドを呼び出します。 たとえば、Details() 操作リスト 3 のビューが表示されます。 または Id パラメーターの値がかどうかに応じて、Index() アクションにはユーザーをリダイレクトします。

**3 - CustomerController.cs を一覧表示します。**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

ContentResult アクションの結果は特殊です。 ContentResult アクションの結果を使用して、プレーン テキストとしてアクションの結果を返すことができます。 たとえば、リスト 4 Index() メソッドは、プレーン テキストとして、および HTML ではなく、メッセージを返します。

**4 - Controllers\StatusController.cs を一覧表示します。**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

StatusController.Index() アクションが呼び出されたときに、表示は返されません。 代わりに、生のテキスト"Hello World!" ブラウザーに返されます。

コント ローラーのアクション結果を返します、たとえば、アクションの結果のないである、日付や整数の場合、結果にラップされて、ContentResult 自動的にします。 たとえば、5 の一覧で WorkController の Index() アクションが呼び出されたときに日付が返されます、ContentResult として自動的に。

**5 - WorkController.cs を一覧表示します。**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

Index() アクション 5 の一覧表示するのには、DateTime オブジェクトを返します。 ASP.NET MVC フレームワークでは、DateTime オブジェクトを文字列に変換し、ContentResult で自動的に DateTime 値をラップします。 ブラウザーは、日付と時刻をプレーン テキストとして受信します。

## <a name="summary"></a>概要

このチュートリアルの目的は、ASP.NET MVC コント ローラー、コント ローラーのアクション、コント ローラー アクションの結果の概念を紹介することでした。 最初のセクションでは、ASP.NET MVC プロジェクトを新しいコント ローラーを追加する方法について学習しました。 コント ローラーのパブリック メソッドを次に、学習したコント ローラーのアクションとして、universe に公開されます。 最後に、コント ローラーのアクションから返されるアクションの結果の種類を説明します。 具体的には、コント ローラーのアクションから ViewResult、RedirectToActionResult、および ContentResult を返す方法を説明します。

>[!div class="step-by-step"]
[前へ](creating-an-action-vb.md)
[次へ](creating-custom-routes-cs.md)
