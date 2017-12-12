---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: "ASP.NET MVC の実行プロセスを理解する |Microsoft ドキュメント"
author: microsoft
description: "ASP.NET MVC フレームワークがステップ バイ ステップ ブラウザー要求を処理する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>ASP.NET MVC の実行プロセスを理解します。
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET MVC フレームワークがステップ バイ ステップ ブラウザー要求を処理する方法について説明します。


ASP.NET MVC ベースの Web アプリケーションへの要求が最初に通過、 **UrlRoutingModule** HTTP モジュールであるオブジェクト。 このモジュールは要求を解析し、ルートの選択を実行します。 **UrlRoutingModule**オブジェクトが現在の要求と一致する最初のルート オブジェクトを選択します。 (ルート オブジェクトを実装するクラスは、 **RouteBase**のインスタンスでは通常、**ルート**クラスです)。ルートと一致しない場合、 **UrlRoutingModule**オブジェクトは、何も実行でき、要求を代替で通常の ASP.NET または IIS の要求を処理します。

選択されている**ルート**オブジェクト、 **UrlRoutingModule**オブジェクトを取得、 **IRouteHandler**オブジェクトに関連付けられている、**ルート**オブジェクト。 通常、MVC アプリケーションでこのインスタンスになります。 の**MvcRouteHandler**です。 **IRouteHandler**インスタンスを作成、 **IHttpHandler**オブジェクトを渡します。、 **IHttpContext**オブジェクト。 既定では、 **IHttpHandler** MVC は、のインスタンス、 **MvcHandler**オブジェクト。 **MvcHandler**オブジェクトが、最終的には、要求を処理するコント ローラーを選択します。

> [!NOTE]
> IIS 7.0 で ASP.NET MVC Web アプリケーションを実行すると、ファイル名拡張子の MVC プロジェクトの必要はありません。 ただし、IIS 6.0 では、ハンドラーが必要に ASP.NET ISAPI DLL には、.mvc ファイル名拡張子をマップすることです。


モジュールとハンドラーは、ASP.NET MVC フレームワークのエントリ ポイントがします。 次の操作を実行します。

- MVC Web アプリケーションで適切なコント ローラーを選択します。
- 特定のコント ローラー インスタンスを取得します。
- コント ローラーを呼び出して**Execute**メソッドです。

MVC Web プロジェクトの実行の段階を次に示します。

- アプリケーションの最初の要求を受信します。 

    - Global.asax ファイルで**ルート**オブジェクトに追加されます、 **RouteTable**オブジェクト。
- ルーティングを実行します。 

    - **UrlRoutingModule** 、最初に一致するモジュールを使用して**ルート**内のオブジェクト、 **RouteTable**を作成するコレクション、 **RouteData**オブジェクトを作成し、使用して、 **RequestContext** (**IHttpContext**) オブジェクト。
- MVC 要求ハンドラーを作成します。 

    - **MvcRouteHandler**オブジェクトのインスタンスを作成する、 **MvcHandler**クラスを渡します。、 **RequestContext**インスタンス。
- コント ローラーを作成します。 

    - **MvcHandler**オブジェクトが使用、 **RequestContext**を識別するインスタンス、 **IControllerFactory**オブジェクト (通常のインスタンス、 **DefaultControllerFactory**クラス) を使用して、コント ローラー インスタンスを作成します。
- Execute コント ローラーで、 **MvcHandler**インスタンスが s コント ローラーを呼び出す**Execute**メソッドです。 |
- アクションを起動します。 

    - ほとんどのコント ローラーが継承、**コント ローラー**基本クラスです。 それを実行するコント ローラーの**ControllerActionInvoker**コント ローラーに関連付けられているオブジェクトを呼び出すには、コント ローラー クラスのアクション メソッドを決定し、そのメソッドを呼び出すとします。
- 結果を実行します。 

    - 一般的なアクション メソッドでは、ユーザー入力を受信、適切な応答データを準備、および結果型を返すことによって、結果を実行可能性があります。 実行できる組み込みの結果型は次のとおりです: **ViewResult** (ビューを表示して最もよく使用される結果型は、)、 **RedirectToRouteResult**、 **RedirectResult**、 **ContentResult**、 **JsonResult**、および**EmptyResult**です。
