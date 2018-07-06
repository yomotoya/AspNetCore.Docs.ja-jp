---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: ASP.NET MVC コント ローラーの概要 (c#) |Microsoft Docs
author: StephenWalther
description: このチュートリアルで Stephen Walther がわかる ASP.NET MVC コント ローラー。 新しいコント ローラーを作成し、さまざまな種類のアクション res を返す方法を学習します.
ms.author: aspnetcontent
ms.date: 02/16/2008
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: f8ff1ed19e6357a9a4e98f755a677d05ae5c2ec4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821723"
---
<a name="aspnet-mvc-controller-overview-c"></a>ASP.NET MVC コント ローラーの概要 (c#)
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> このチュートリアルで Stephen Walther がわかる ASP.NET MVC コント ローラー。 新しいコント ローラーを作成し、さまざまな種類のアクションの結果を返す方法を学習します。


このチュートリアルでは、ASP.NET MVC コント ローラー、コント ローラーのアクションおよびアクションの結果のトピックについて説明します。 このチュートリアルを完了すると、コント ローラーを使用して ASP.NET MVC web サイトを訪問者と対話する方法を制御する方法を理解できます。

## <a name="understanding-controllers"></a>コント ローラーの説明

MVC コント ローラーは、ASP.NET MVC の web サイトに対して行われた要求に応答します。 各ブラウザーの要求は、特定のコント ローラーにマップされます。 たとえば、お使いのブラウザーのアドレス バーに次の URL を入力してください。

`http://localhost/Product/Index/3`

この場合は、「productcontroller」という名前のコント ローラーが呼び出されます。 「Productcontroller」はブラウザー要求に対する応答を生成しますします。 たとえば、コント ローラーは、ブラウザーに特定のビューを返す可能性があります。 またはコント ローラーは別のコント ローラーにユーザーをリダイレクトする可能性があります。

1 を一覧表示するには、「productcontroller」という名前のシンプルなコント ローラーにはが含まれています。

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

リスト 1 からわかるように、コント ローラー、クラス (Visual Basic .NET または c# のクラス) だけです。 コント ローラーは、基本の System.Web.Mvc.Controller クラスから派生したクラスです。 コント ローラーが無料でいくつかの便利なメソッドを継承するコント ローラーは、この基本クラスから継承、するため (これらのメソッドで説明する時点)。

## <a name="understanding-controller-actions"></a>コント ローラー アクションを理解します。

コント ローラーは、コント ローラー アクションを公開します。 アクションは、ブラウザーのアドレス バーで、特定の URL を入力するときに呼び出されるコント ローラーのメソッドです。 たとえば、次の URL の要求を行うことを考えてみましょう。

`http://localhost/Product/Index/3`

この場合、Index() メソッドは、「productcontroller」クラスで呼び出されます。 Index() メソッドは、コント ローラー アクションの例を示します。

コント ローラーのアクションは、コント ローラー クラスのパブリック メソッドである必要があります。 C# のメソッド、既定では、プライベート メソッドです。 コント ローラー クラスに追加したすべてのパブリック メソッドがコント ローラーのアクションとして自動的に公開されることを実現 (する必要がありますこれについて注意が必要ですので、コント ローラーのアクションは、ブラウザーのアドレス バーに適切な URL を入力するだけで、世界中のだれでも呼び出すことができます)。

コント ローラーのアクションで満たす必要があるいくつかの追加要件があります。 コント ローラーのアクションとして使用されるメソッドをオーバー ロードできません。 さらに、コント ローラーのアクションは、静的メソッドをすることはできません。 それ以外には、コント ローラーのアクションとしてほとんどすべてのメソッドを使用できます。

## <a name="understanding-action-results"></a>アクションの結果を理解します。

コント ローラーのアクションを返しますと呼ばれるものを*アクション結果*します。 アクションの結果は、コント ローラーのアクションはブラウザーの要求に応答を返します。

ASP.NET MVC フレームワークでは、アクションの結果などのいくつかの種類をサポートします。

1. ViewResult - 表します HTML とマークアップ。
2. EmptyResult のない結果を表します。
3. RedirectResult - は、新しい URL へのリダイレクトを表します。
4. JsonResult - は、AJAX アプリケーションで使用できる JavaScript Object Notation 結果を表します。
5. JavaScriptResult - JavaScript のスクリプトを表します。
6. ContentResult - は、テキストの結果を表します。
7. FileContentResult - は、ダウンロード可能な (バイナリ コンテンツ) を含むファイルを表します。
8. FilePathResult - は、ダウンロード可能な (パス) を使用してファイルを表します。
9. FileStreamResult - は、(ファイル ストリーム) をダウンロード可能なファイルを表します。

これらのアクション結果のすべては、基本の ActionResult クラスから継承します。

ほとんどの場合は、コント ローラーのアクションは、ViewResult を返します。 たとえば、リスト 2 でインデックスのコント ローラー アクションは、ViewResult を返します。

**2 - Controllers\BookController.cs を一覧表示します。**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

アクション、ViewResult が戻るとき、ブラウザーに HTML が返されます。 リスト 2 で Index() メソッドは、ブラウザーにインデックスをという名前のビューを返します。

リスト 2 で Index() アクションが、ViewResult() を返さないことに注意してください。 代わりに、コント ローラーの基本クラスの View() メソッドが呼び出されます。 通常、アクションの結果を直接返すはされません。 代わりに、コント ローラーの基本クラスの次のメソッドのいずれかを呼び出します。

1. 表示 - ViewResult アクションの結果を返します。
2. リダイレクト - RedirectResult アクションの結果を返します。
3. RedirectToAction - RedirectToRouteResult アクションの結果を返します。
4. RedirectToRoute - RedirectToRouteResult アクションの結果を返します。
5. Json - JsonResult アクションの結果を返します。
6. JavaScriptResult - は、JavaScriptResult を返します。
7. コンテンツ - ContentResult アクションの結果を返します。
8. ファイル - FileContentResult、FilePathResult、またはパラメーターに応じて FileStreamResult がメソッドに渡されるを返します。

そのため、ブラウザーにビューを返す場合には、View() メソッドを呼び出します。 1 つのコント ローラー アクションからユーザーをリダイレクトする場合は、RedirectToAction() メソッドを呼び出します。 たとえば、リスト 3 の Details() アクション ビューを表示します。 またはユーザー Id パラメーターが値を持つかどうかに応じて Index() アクションにリダイレクトします。

**3 - CustomerController.cs を一覧表示します。**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

ContentResult アクションの結果は特殊です。 ContentResult アクションの結果を使用して、プレーン テキストとしてアクションの結果を返すことができます。 たとえば、リスト 4 Index() メソッドは、HTML ではなく、プレーン テキストとして、メッセージを返します。

**4 - Controllers\StatusController.cs を一覧表示します。**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

StatusController.Index() アクションが呼び出されたときに、ビューは返されません。 代わりに、未加工のテキスト"Hello World!" ブラウザーに返されます。

コント ローラーのアクションは、日付や整数 - の結果などのアクションの結果のないであるを返す場合は、結果が自動的にラップされた ContentResult でし。 たとえば、リスト 5 WorkController の Index() アクションが呼び出されたときに、日付が返されます ContentResult として自動的に。

**5 - WorkController.cs を一覧表示します。**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

リスト 5 Index() アクションは、DateTime オブジェクトを返します。 ASP.NET MVC フレームワークでは、DateTime オブジェクトを文字列に変換し、ContentResult で DateTime 値を自動的にラップします。 ブラウザーは、日付と時刻をプレーン テキストとして受信します。

## <a name="summary"></a>まとめ

このチュートリアルの目的は、ASP.NET MVC コント ローラー、コント ローラー アクション、コント ローラー アクションの結果の概念を紹介しました。 最初のセクションでは、ASP.NET MVC プロジェクトに新しいコント ローラーを追加する方法について説明しました。 次に、コント ローラーのメソッドを公開する方法を学習しました。 コント ローラー アクションとして、世界に公開されます。 最後に、コント ローラー アクションから返されるアクションの結果の種類を説明します。 具体的には、コント ローラー アクションから ViewResult、RedirectToActionResult、および ContentResult を返す方法を説明します。

> [!div class="step-by-step"]
> [前へ](creating-an-action-vb.md)
> [次へ](creating-custom-routes-cs.md)
