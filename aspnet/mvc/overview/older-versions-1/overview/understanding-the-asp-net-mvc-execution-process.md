---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: ASP.NET MVC 実行プロセスの理解 |Microsoft Docs
author: microsoft
description: ASP.NET MVC フレームワークがステップ バイ ステップのブラウザー要求を処理する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: d94fcedb6e0ec6134bdbd69729abf1f875fac420
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378272"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>ASP.NET MVC 実行プロセスを理解します。
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET MVC フレームワークがステップ バイ ステップのブラウザー要求を処理する方法について説明します。


ASP.NET MVC ベースの Web アプリケーションへの要求が最初に通過、 **UrlRoutingModule**オブジェクトで、HTTP モジュールです。 このモジュールは、要求を解析し、ルートの選択を実行します。 **UrlRoutingModule**オブジェクトが現在の要求と一致する最初のルート オブジェクトを選択します。 (ルート オブジェクトを実装するクラスは、 **RouteBase**と通常のインスタンスでは、**ルート**クラスです)。ルートが一致しない場合、 **UrlRoutingModule**オブジェクトは、何も実行でき、フォールバックする通常の ASP.NET または IIS 要求処理要求。

選択されている**ルート**オブジェクト、 **UrlRoutingModule**オブジェクトを取得、 **IRouteHandler**オブジェクトに関連付けられている、**ルート**オブジェクト。 通常、MVC アプリケーションのこのインスタンスになります。 の**MvcRouteHandler**します。 **IRouteHandler**インスタンスを作成、 **IHttpHandler**オブジェクトを渡します、 **IHttpContext**オブジェクト。 既定で、 **IHttpHandler** MVC がのインスタンス、 **MvcHandler**オブジェクト。 **MvcHandler**オブジェクトが、最終的には、要求を処理するコント ローラーを選択します。

> [!NOTE]
> IIS 7.0 で ASP.NET MVC Web アプリケーションを実行すると、ファイル名拡張子の MVC プロジェクトの必要はありません。 ただし、IIS 6.0 では、ハンドラーが必要です、ASP.NET ISAPI DLL には、.mvc ファイル名拡張子をマップすること。


モジュールとハンドラーは、ASP.NET MVC フレームワークのエントリ ポイントです。 次の操作を実行します。

- MVC Web アプリケーションでは、適切なコント ローラーを選択します。
- 特定のコント ローラー インスタンスを取得します。
- コント ローラーを呼び出す**Execute**メソッド。

MVC Web プロジェクトの実行の段階を次に示します。

- アプリケーションの最初の要求を受信します。 

    - Global.asax ファイルで**ルート**オブジェクトに追加されます、 **RouteTable**オブジェクト。
- ルーティングを実行します。 

    - **UrlRoutingModule**最初に一致するモジュールを使用して**ルート**オブジェクト、 **RouteTable**コレクションを作成する、 **RouteData**使用して、作成、オブジェクト、 **RequestContext** (**IHttpContext**) オブジェクト。
- MVC 要求ハンドラーを作成します。 

    - **MvcRouteHandler**オブジェクトのインスタンスを作成する、 **MvcHandler**クラスを渡します、 **RequestContext**インスタンス。
- コント ローラーを作成します。 

    - **MvcHandler**オブジェクトで使用、 **RequestContext**インスタンスを識別するために、 **IControllerFactory**オブジェクト (通常のインスタンス、 **DefaultControllerFactory**クラス) を使用して、コント ローラー インスタンスを作成します。
- Execute のコント ローラー -、 **MvcHandler**インスタンスが s コント ローラーを呼び出す**Execute**メソッド。 |
- アクションを呼び出す 

    - 継承するほとんどのコント ローラー、**コント ローラー**基本クラス。 それを実行するコント ローラー、 **ControllerActionInvoker**コント ローラーに関連付けられているオブジェクトが呼び出すために、コント ローラー クラスのアクション メソッドを決定し、そのメソッドを呼び出します。
- 結果を実行します。 

    - 一般的なアクション メソッドでは、ユーザー入力を受け取る、適切な応答データを準備、および結果型を返すことによって、結果を実行可能性があります。 実行できる組み込みの結果型は、次を含める: **ViewResult** (ビューをレンダリングし、は、最もよく使用される結果型です)、 **RedirectToRouteResult**、 **RedirectResult**、 **ContentResult**、 **JsonResult**、および**EmptyResult**します。
