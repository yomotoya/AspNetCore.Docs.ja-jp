---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: ASP.NET MVC ルーティング概要 (VB) |Microsoft Docs
author: StephenWalther
description: このチュートリアルでは、Stephen Walther は、ASP.NET MVC フレームワークがコント ローラー アクションへのブラウザーの要求をマップする方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 0f078455397014ba1bcddaeece6dea7737f33710
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384158"
---
<a name="aspnet-mvc-routing-overview-vb"></a>ASP.NET MVC ルーティング概要 (VB)
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> このチュートリアルでは、Stephen Walther は、ASP.NET MVC フレームワークがコント ローラー アクションへのブラウザーの要求をマップする方法を示します。


このチュートリアルと呼ばれるすべての ASP.NET MVC アプリケーションの重要な機能に導入された*ASP.NET ルーティング*します。 ASP.NET ルーティング モジュールは、特定の MVC コント ローラー アクションへの受信ブラウザー要求をマッピングします。 このチュートリアルの目的は、標準的なルート テーブルがコント ローラー アクションへの要求をマップする方法を理解します。

## <a name="using-the-default-route-table"></a>既定のルート テーブルを使用します。

新しい ASP.NET MVC アプリケーションを作成するときに ASP.NET のルーティングを使用するアプリケーションが既に構成されています。 ASP.NET ルーティングは、2 つの場所でのセットアップです。

最初に、ASP.NET ルーティングは、アプリケーションの Web 構成ファイル (web.config) で有効です。 ルーティングに関連する構成ファイルで次の 4 つのセクションがあります: system.web.httpModules セクション、system.web.httpHandlers セクション、system.webserver.modules セクションおよび system.webserver.handlers セクション。 これらのセクションではないルーティングは動作しないために、これらのセクションを削除しないように注意します。

次に、そして何よりも、ルート テーブルは、アプリケーションの Global.asax ファイルに作成されます。 Global.asax ファイルは、ASP.NET アプリケーションのライフ サイクル イベントのイベント ハンドラーを含む特殊なファイルです。 ルート テーブルは、アプリケーションの開始イベント中に作成されます。

リスト 1 でファイルには、ASP.NET MVC アプリケーションの既定の Global.asax ファイルが含まれています。

**1 - Global.asax.vb を一覧表示します。**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

MVC アプリケーションを最初の起動時、アプリケーション\_Start() メソッドが呼び出されます。 このメソッドは、さらに、RegisterRoutes() メソッドを呼び出します。 RegisterRoutes() メソッドは、ルート テーブルを作成します。

既定のルーティング テーブルには、Default という 1 つのルートが含まれています。 既定のルートがコント ローラー名、コント ローラーのアクションへの URL は、2 番目のセグメントおよびという名前のパラメーターを 3 番目のセグメントに URL の最初のセグメントをマップ**id**します。

Web ブラウザーのアドレス バーには、次の URL を入力することを想像してください。

/Home/Index/3

既定のルートは、この URL を次のパラメーターにマッピングされます。

- コント ローラー = ホーム

- アクション インデックスを =

- id = 3

URL/Home、インデックス、3 を要求するときに、次のコードが実行されます。

HomeController.Index(3)

既定のルートには、次の 3 つのすべてのパラメーターの既定値が含まれています。 コント ローラーを指定しない場合、コント ローラー パラメーターの既定値、値**ホーム**します。 アクション パラメーターの値に既定値アクションを指定しない場合**インデックス**します。 最後に、id を指定しない場合は、id パラメーターが空の文字列に既定値です。

既定のルートがコント ローラー アクションへの Url をマップする方法の例をいくつか見てみましょう。 ブラウザーのアドレス バーには、次の URL を入力することを想像してください。

/ホーム

既定ルート パラメーターの既定値のためこの URL を入力する、HomeController クラスの Index() メソッドを呼び出せるリスト 2 でします。

**2 - HomeController.vb を一覧表示します。**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

リスト 2 では、HomeController クラスに Index() という名前の id。 1 つのパラメーターを受け取るという名前のメソッドが含まれます。URL/Home はにより呼び出される値は、Id パラメーターの値として何も Index() メソッドです。

URL/Home には、MVC フレームワークがコント ローラー アクションを呼び出すことにより、リスト 3 のテンプレートを使用するクラスの Index() メソッドもと一致します。

**3 - HomeController.vb (パラメーターなしでの Index アクション) を一覧表示します。**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

Index() メソッドは、リスト 3 では、すべてのパラメーターは受け入れられません。 URL/Home をこの Index() メソッドが呼び出されるとなります。 また、URL/Home、インデックス、3 は、(Id は無視されます) このメソッドを呼び出します。

URL/Home には、リスト 4 HomeController クラスの Index() メソッドもと一致します。

**4 - HomeController.vb (null 許容パラメーターを使用してインデックスの操作) を一覧表示します。**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

Index() メソッドではリスト 4、1 つの整数パラメーターがあります。 パラメーターは null 許容パラメーター (値を持つことは Nothing) では、エラーを発生させず、Index() を呼び出すことができます。

最後に、Id パラメーターから例外が発生した URL/Home でリスト 5 で Index() メソッドを呼び出す*ない*null 許容パラメーター。 Index() メソッドを呼び出すしようとした場合、エラーが発生した図 1 に表示されます。

**5 - HomeController.vb (Id パラメーターを使用してインデックスの操作) を一覧表示します。**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


[![パラメーターの値が必要とするコント ローラー アクションの呼び出し](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)

**図 01**: パラメーターの値が必要とするコント ローラー アクションの呼び出し ([フルサイズの画像を表示する をクリックします](asp-net-mvc-routing-overview-vb/_static/image2.png))。


URL/Home/インデックス/3 は一方で、リスト 5 でインデックスのコント ローラー アクションでうまくいきます。 要求/Home/Index/3 とにより、3 の値を持つ Id パラメーターで呼び出される Index() メソッドです。

## <a name="summary"></a>まとめ

このチュートリアルの目的は、ASP.NET ルーティングの概要を提供することでした。 新しい ASP.NET MVC アプリケーションで得られる既定のルーティング テーブルを調査します。 既定のルートがコント ローラー アクションへの Url をマップする方法を学習しました。

> [!div class="step-by-step"]
> [前へ](creating-an-action-cs.md)
> [次へ](understanding-action-filters-vb.md)
