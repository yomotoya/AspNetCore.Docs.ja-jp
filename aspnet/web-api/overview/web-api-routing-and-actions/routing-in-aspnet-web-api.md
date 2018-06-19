---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: ASP.NET Web API でルーティング |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26509261"
---
<a name="routing-in-aspnet-web-api"></a>ASP.NET Web API でのルーティング
====================
によって[Mike Wasson](https://github.com/MikeWasson)

この記事では、ASP.NET Web API がコント ローラーに HTTP 要求をルーティングする方法について説明します。

> [!NOTE]
> ASP.NET MVC について理解する場合は、Web API ルーティングは、MVC ルーティングするためによく似ています。 主な違いは、Web API メソッドを使用して、HTTP、URI パスではなく、操作を選択します。 また、Web API での MVC スタイルのルーティングを使用することができます。 この記事は、ASP.NET MVC の知識を想定していません。


## <a name="routing-tables"></a>ルーティング テーブル

ASP.NET Web API で、*コント ローラー* HTTP 要求を処理するクラスです。 コント ローラーのパブリック メソッドが呼び出されます*アクション メソッド*または単に*アクション*です。 Web API フレームワークでは、要求を受信すると、アクションに要求をルーティングします。

フレームワークを使用して呼び出すアクションを確認するのには*ルーティング テーブル*です。 Web API の Visual Studio プロジェクト テンプレートには、既定のルートを作成します。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

このルートがアプリに配置されます WebApiConfig.cs ファイルで定義されている\_開始ディレクトリ。

![](routing-in-aspnet-web-api/_static/image1.png)

詳細については、 **WebApiConfig**クラスを参照してください[ASP.NET Web API の構成](../advanced/configuring-aspnet-web-api.md)です。

自己 Web API をホストしている場合に直接ルーティング テーブルを設定する必要があります、 **HttpSelfHostConfiguration**オブジェクト。 詳細については、次を参照してください。 [Web API の自己ホスト](../older-versions/self-host-a-web-api.md)です。

ルーティング テーブル内の各エントリが含まれています、*ルート テンプレート*です。 Web API の既定のルート テンプレートは&quot;api/{controller}/{id}&quot;です。 このテンプレートで&quot;api&quot;は、リテラルのパス セグメント、および {controller} と {id} プレース ホルダー変数します。

Web API フレームワークでは、HTTP 要求を受信、ルーティング テーブルにルート テンプレートのいずれかに対して URI と照合しようとします。 ルートが一致しない場合、クライアントは、404 エラーを受け取ります。 たとえば、次の Uri は、既定のルートを一致します。

- /api および連絡先
- /api/contacts/1
- /api/products/gizmo1

ただし、次の URI が一致しないがないため、 &quot;api&quot;セグメント。

- /連絡先/1

> [!NOTE]
> ルートに"api"を使用する理由は、ASP.NET MVC ルーティングとの衝突を避けるためにです。 そうすることができます&quot;連絡先/&quot; 、MVC コント ローラーに移動し、 &quot;api/連絡先&quot;Web API コント ローラーに移動します。 もちろん、この規則がたくない場合は、既定のルート テーブルを変更できます。

一致するルートが見つかると、Web API は、コント ローラーとアクションを選択します。

- Web API を追加するコント ローラーを検索するには、&quot;コント ローラー&quot;の値に、 *{controller}* 変数。
- アクションを検索するには、Web API HTTP メソッドと調べ操作の対応する HTTP メソッド名で始まる名前を検索します。 たとえば、GET 要求で Web API 検索で始まるアクション&quot;を取得しています.&quot;など&quot;GetContact&quot;または&quot;GetAllContacts&quot;です。 この規則は、取得、POST、PUT、および削除メソッドにのみ適用されます。 その他の HTTP メソッドを有効にするには、コント ローラーの属性を使用します。 その例が後で表示されます。
- ルート テンプレートでは、その他のプレース ホルダー変数など *{id},* アクション パラメーターにマップされます。

例を見てみましょう。 次のコント ローラーを定義するとします。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

一部可能な HTTP 要求、ごとに呼び出されるアクションを次に示します。

| HTTP メソッド | URI のパス | 操作 | パラメーター |
| --- | --- | --- | --- |
| GET | api/製品 | GetAllProducts | *(なし)* |
| GET | api/製品/4 | GetProductById | 4 |
| Del | api/製品/4 | DeleteProduct | 4 |
| 投稿 | api/製品 | *(一致なし)* |  |

注意して、 *{id}* 、URI のセグメントは存在する場合にマップされて、 *id*アクションのパラメーターです。 この例では、コント ローラーを持つ 2 つの GET メソッドを定義、 *id*パラメーター、パラメーターなしのいずれか。

また、注意してください、コント ローラーが定義されていないため、POST 要求に失敗する、 &quot;Post しています.&quot;メソッドです。

## <a name="routing-variations"></a>ルーティングのバリエーション

前のセクションでは、ASP.NET Web api ルーティングの基本的なメカニズムについて説明します。 このセクションでは、いくつかのバリエーションについて説明します。

### <a name="http-methods"></a>HTTP メソッド

HTTP メソッドの名前付け規則を使用する代わりにことが明示的に指定するアクションの HTTP メソッドをアクション メソッドを修飾することによって、 **HttpGet**、 **HttpPut**、 **HttpPost**、または**HttpDelete**属性。

次の例では、FindProduct メソッドは、GET 要求にマップされます。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

アクションに対して複数の HTTP メソッドを許可する、または GET、PUT、POST、および DELETE 以外の HTTP メソッドを使用して、 **AcceptVerbs**属性は、HTTP メソッドの一覧です。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>アクション名をルーティング

Web API では、既定のルーティング テンプレートで、HTTP メソッドを使用して、操作を選択します。 ただし、URI でアクション名が含まれているルートを作成することもできます。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

このルート テンプレートで、 *{controller}* パラメーター名、コント ローラー アクション メソッド。 ルーティングのこのスタイルでは、許可される HTTP メソッドを指定するのに属性を使用します。 たとえば、コント ローラーに次のメソッドがあるとします。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

この場合、「api/製品/詳細/1」の GET 要求は、Details メソッドにマップします。 このスタイルのルーティングは ASP.NET MVC に似ており、RPC スタイルの API の適切な場合があります。

使用して、アクション名をオーバーライドすることができます、 **ActionName**属性。 次の例では、2 つのアクションにマップされる&quot;api/製品/縮小版/*id*です。GET をでいずれか、他の POST をサポートしています。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>アクション以外

メソッドを防ぐため、アクションとして呼び出されてを使用して、 **NonAction**属性。 これは、通知 framework メソッドは、アクションではない場合でも、ルーティングの規則はそれ以外の場合と一致します。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>関連項目

このトピックでは、ルーティングの概要ビューが用意されています。 詳細については、次を参照してください。[ルーティングとアクションの選択](routing-and-action-selection.md)、を正確におよび方法について説明フレームワークをルート URI と一致する、コント ローラーを選択に呼び出すアクションを選択します。
