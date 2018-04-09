---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: ASP.NET MVC ルーティングの概要 (c#) |Microsoft ドキュメント
author: StephenWalther
description: このチュートリアルでは、Stephen Walther は、ASP.NET MVC フレームワークがコント ローラーのアクションをブラウザーの要求をマップする方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: fa565d2ef253539844f5224df00bdcdc047bb3f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-routing-overview-c"></a>ASP.NET MVC ルーティングの概要 (c#)
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> このチュートリアルでは、Stephen Walther は、ASP.NET MVC フレームワークがコント ローラーのアクションをブラウザーの要求をマップする方法を示します。


このチュートリアルと呼ばれるすべての ASP.NET MVC アプリケーションの重要な特徴に導入する*ASP.NET ルーティング*です。 ASP.NET ルーティング モジュールは、特定の MVC コント ローラー アクションへのブラウザーの入力方向の要求のマッピングを行います。 このチュートリアルの目的は、標準的なルート テーブルがコント ローラーのアクションに要求をマップする方法を理解します。

## <a name="using-the-default-route-table"></a>既定のルート テーブルを使用します。

新しい ASP.NET MVC アプリケーションを作成するときに ASP.NET のルーティングを使用するアプリケーションが既に構成されています。 ASP.NET ルーティングでは、セットアップが 2 つの場所がいます。

最初に、ASP.NET ルーティングが有効になっているアプリケーションの Web 構成ファイル (Web.config ファイル) にします。 ルーティングに関連する構成ファイル内の 4 つのセクションがある: system.web.httpModules セクション、system.web.httpHandlers セクション、system.webserver.modules セクションおよび system.webserver.handlers セクションです。 これらのセクションではなくルーティングが機能しないために、これらのセクションを削除しないように注意します。

次に、し、さらに、アプリケーションの Global.asax ファイルで、ルート テーブルが作成されます。 Global.asax ファイルは、ASP.NET アプリケーションのライフ サイクル イベントのイベント ハンドラーを含む特殊なファイルです。 アプリケーションの開始イベントの中では、ルート テーブルが作成されます。

1 のリスト内のファイルには、ASP.NET MVC アプリケーションの既定の Global.asax ファイルが含まれています。

**1 - Global.asax.cs を一覧表示します。**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

MVC アプリケーション最初の開始時、アプリケーション\_Start() メソッドが呼び出されます。 このメソッドは、さらに、RegisterRoutes() メソッドを呼び出します。 RegisterRoutes() メソッドでは、ルート テーブルを作成します。

既定のルート テーブルには、1 つのルート (Default という名前) が含まれています。 既定のルートがコント ローラー名、コント ローラー アクションへの URL は、2 番目のセグメントおよびという名前のパラメーターを 3 番目のセグメントに URL の最初のセグメントをマップ**id**です。

Web ブラウザーのアドレス バーに次の URL を入力することを想像してください。

/Home/Index/3

既定のルートは、この URL を次のパラメーターにマッピングされます。

- コント ローラー ホーム =

- アクション インデックスを =

- id = 3

URL/Home、インデックス、3 を要求するときに、次のコードは実行されます。

HomeController.Index(3)

既定のルートには、次の 3 つすべてのパラメーターの既定値が含まれています。 コント ローラーを指定しない場合、コント ローラー パラメーターの既定値、値**ホーム**です。 Action パラメーターの値に既定値アクションを指定しない場合**インデックス**です。 最後に、id を指定しない場合、id パラメーターの既定値、空の文字列。

既定のルートが Url をコント ローラーのアクションにマップする方法の例をいくつか見てみましょう。 ブラウザーのアドレス バーに次の URL を入力することを想像してください。

/ホーム

既定ルート パラメーターの既定値のためこの URL を入力すると、Index() メソッドは HomeController クラスのリストの 2 を呼び出せるでします。

**2 - HomeController.cs を一覧表示します。**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

HomeController クラスには 2 の一覧表示するには id。 をという名前の単一パラメーターを受け入れる Index() をという名前のメソッドが含まれていますURL/Home が、Id パラメーターの値として空の文字列で呼び出される Index() メソッドです。

MVC フレームワークがコント ローラーのアクションを呼び出すことにより、URL/Home とも一致リスト 3 のテンプレートを使用するクラスの Index() メソッドです。

**3 - HomeController.cs (パラメーターなしでインデックス操作) を一覧表示します。**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

Index() メソッドを一覧表示する 3 では、パラメーターを受け入れません。 URL/Home をこの Index() メソッドが呼び出されるとなります。 また、URL/Home、インデックス、3 は、(Id は無視されます) このメソッドを呼び出します。

URL/Home では、リスト 4 HomeController クラスの Index() メソッドとも一致します。

**4 - HomeController.cs (null 許容パラメーターを持つインデックスの操作) を一覧表示します。**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

4 の一覧表示するのには、Index() メソッドは、1 つの整数パラメーターを持ちます。 パラメーターは (Null 値を保持できます)、null 許容パラメーターでは、エラーを発生させず、Index() を呼び出すことができます。

最後に、Id パラメーターから例外が発生した URL/Home を一覧表示する 5 で Index() メソッドを呼び出す*は*null 許容パラメーター。 Index() メソッドを呼び出すしようとすると、エラーが発生した図 1 に表示されます。

**5 - HomeController.cs (インデックスの Id パラメーターを持つ操作) を一覧表示します。**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


[![パラメーターの値を期待しているコント ローラー アクションの呼び出し](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)

**図 01**: パラメーターの値を期待しているコント ローラー アクションの呼び出し ([フルサイズのイメージを表示するをクリックして](asp-net-mvc-routing-overview-cs/_static/image2.png))


URL/Home/インデックス/3 一方で、正常に動作だけを一覧表示する 5 でインデックス コント ローラーのアクション。 要求/Home/Index/3 が 3 の値を持つ Id パラメーターを指定して呼び出される Index() メソッドです。

## <a name="summary"></a>まとめ

このチュートリアルの目的は、ASP.NET ルーティングの概要情報を提供しました。 ここには、新しい ASP.NET MVC アプリケーションで得られる既定のルート テーブルが調べられます。 既定のルートが Url をコント ローラーのアクションにマップする方法を学習しました。

> [!div class="step-by-step"]
> [次へ](understanding-action-filters-cs.md)
