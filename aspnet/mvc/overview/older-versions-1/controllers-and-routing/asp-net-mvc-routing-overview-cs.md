---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: ASP.NET MVC ルーティング概要 (C#) |Microsoft Docs
author: StephenWalther
description: このチュートリアルでは、Stephen Walther は、ASP.NET MVC フレームワークがコント ローラー アクションへのブラウザーの要求をマップする方法を示します。
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 188490c5ca075710dcbdcd1c325808f7c1d383bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826957"
---
<a name="aspnet-mvc-routing-overview-c"></a>ASP.NET MVC ルーティング概要 (C#)
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> このチュートリアルでは、Stephen Walther は、ASP.NET MVC フレームワークがコント ローラー アクションへのブラウザーの要求をマップする方法を示します。


このチュートリアルと呼ばれるすべての ASP.NET MVC アプリケーションの重要な機能に導入された*ASP.NET ルーティング*します。 ASP.NET ルーティング モジュールは、特定の MVC コント ローラー アクションへの受信ブラウザー要求をマッピングします。 このチュートリアルの目的は、標準的なルート テーブルがコント ローラー アクションへの要求をマップする方法を理解します。

## <a name="using-the-default-route-table"></a>既定のルート テーブルを使用します。

新しい ASP.NET MVC アプリケーションを作成するときに ASP.NET のルーティングを使用するアプリケーションが既に構成されています。 ASP.NET ルーティングは、2 つの場所でのセットアップです。

最初に、ASP.NET ルーティングは、アプリケーションの Web 構成ファイル (web.config) で有効です。 ルーティングに関連する構成ファイルで次の 4 つのセクションがあります: system.web.httpModules セクション、system.web.httpHandlers セクション、system.webserver.modules セクションおよび system.webserver.handlers セクション。 これらのセクションではないルーティングは動作しないために、これらのセクションを削除しないように注意します。

次に、そして何よりも、ルート テーブルは、アプリケーションの Global.asax ファイルに作成されます。 Global.asax ファイルは、ASP.NET アプリケーションのライフ サイクル イベントのイベント ハンドラーを含む特殊なファイルです。 ルート テーブルは、アプリケーションの開始イベント中に作成されます。

リスト 1 でファイルには、ASP.NET MVC アプリケーションの既定の Global.asax ファイルが含まれています。

**1 - Global.asax.cs の一覧を表示します。**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

MVC アプリケーションを最初の起動時、アプリケーション\_Start() メソッドが呼び出されます。 このメソッドは、さらに、RegisterRoutes() メソッドを呼び出します。 RegisterRoutes() メソッドは、ルート テーブルを作成します。

既定のルーティング テーブルには、Default という 1 つのルートが含まれています。 既定のルートがコント ローラー名、コント ローラーのアクションへの URL は、2 番目のセグメントおよびという名前のパラメーターを 3 番目のセグメントに URL の最初のセグメントをマップ**id**します。

Web ブラウザーのアドレス バーには、次の URL を入力することを想像してください。

/Home/Index/3

既定のルートは、この URL を次のパラメーターにマッピングされます。

- controller = Home

- action = Index

- id = 3

URL/Home、インデックス、3 を要求するときに、次のコードが実行されます。

HomeController.Index(3)

既定のルートには、次の 3 つのすべてのパラメーターの既定値が含まれています。 コント ローラーを指定しない場合、コント ローラー パラメーターの既定値、値**ホーム**します。 アクション パラメーターの値に既定値アクションを指定しない場合**インデックス**します。 最後に、id を指定しない場合は、id パラメーターが空の文字列に既定値です。

既定のルートがコント ローラー アクションへの Url をマップする方法の例をいくつか見てみましょう。 ブラウザーのアドレス バーには、次の URL を入力することを想像してください。

/Home

既定ルート パラメーターの既定値のためこの URL を入力する、HomeController クラスの Index() メソッドを呼び出せるリスト 2 でします。

**2 - HomeController.cs を一覧表示します。**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

リスト 2 では、HomeController クラスに Index() という名前の id。 1 つのパラメーターを受け取るという名前のメソッドが含まれます。URL/Home はにより、Id パラメーターの値として空の文字列で呼び出せる Index() メソッドです。

URL/Home には、MVC フレームワークがコント ローラー アクションを呼び出すことにより、リスト 3 のテンプレートを使用するクラスの Index() メソッドもと一致します。

**3 - HomeController.cs (パラメーターなしでの Index アクション) を一覧表示します。**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

Index() メソッドは、リスト 3 では、すべてのパラメーターは受け入れられません。 URL/Home をこの Index() メソッドが呼び出されるとなります。 また、URL /Home/Index/3 は、(Id は無視されます) このメソッドを呼び出します。

URL/Home には、リスト 4 HomeController クラスの Index() メソッドもと一致します。

**4 - HomeController.cs (null 許容パラメーターを使用してインデックスの操作) を一覧表示します。**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

Index() メソッドではリスト 4、1 つの整数パラメーターがあります。 パラメーターは (Null 値を持つことができます)、null 許容パラメーターでは、エラーを発生させず、Index() を呼び出すことができます。

最後に、Id パラメーターから例外が発生した URL/Home でリスト 5 で Index() メソッドを呼び出す*ない*null 許容パラメーター。 Index() メソッドを呼び出すしようとした場合、エラーが発生した図 1 に表示されます。

**5 - HomeController.cs (Id パラメーターを使用してインデックスの操作) を一覧表示します。**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


[![パラメーターの値が必要とするコント ローラー アクションの呼び出し](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)

**図 01**: パラメーターの値が必要とするコント ローラー アクションの呼び出し ([フルサイズの画像を表示する をクリックします](asp-net-mvc-routing-overview-cs/_static/image2.png))。


URL /Home/Index/3 は一方で、リスト 5 でインデックスのコント ローラー アクションでうまくいきます。 要求/Home/Index/3 とにより、3 の値を持つ Id パラメーターで呼び出される Index() メソッドです。

## <a name="summary"></a>まとめ

このチュートリアルの目的は、ASP.NET ルーティングの概要を提供することでした。 新しい ASP.NET MVC アプリケーションで得られる既定のルーティング テーブルを調査します。 既定のルートがコント ローラー アクションへの Url をマップする方法を学習しました。

> [!div class="step-by-step"]
> [次へ](understanding-action-filters-cs.md)
