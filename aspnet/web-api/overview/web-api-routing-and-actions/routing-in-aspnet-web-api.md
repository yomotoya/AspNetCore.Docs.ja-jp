---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: ASP.NET Web API でのルーティング |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207304"
---
<a name="routing-in-aspnet-web-api"></a>ASP.NET Web API でのルーティング
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

この記事では、ASP.NET Web API がコント ローラーに HTTP 要求をルーティングする方法について説明します。

> [!NOTE]
> ASP.NET MVC について理解する場合は、Web API のルーティングは、MVC ルーティングによく似ています。 主な違いは、Web API が URI パスではなく、HTTP 動詞を使用して、アクションを選択することです。 MVC スタイルの Web API のルーティングを使用することもできます。 この記事では、ASP.NET MVC の知識を前提としてはいません。

## <a name="routing-tables"></a>ルーティング テーブル

ASP.NET Web api で、*コント ローラー*は HTTP 要求を処理するクラスです。 コント ローラーのパブリック メソッドが呼び出されます*アクション メソッド*または単に*アクション*します。 Web API フレームワークは、要求を受信すると、アクションに要求をルーティングします。

呼び出すアクションを確認するのには、framework は、*ルーティング テーブル*します。 Web API 用の Visual Studio プロジェクト テンプレートは、既定のルートを作成します。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

このルートが定義されている、 *WebApiConfig.cs*で配置されているファイル、*アプリ\_開始*ディレクトリ。

![](routing-in-aspnet-web-api/_static/image1.png)

詳細については、`WebApiConfig`クラスを参照してください[ASP.NET Web API の構成](../advanced/configuring-aspnet-web-api.md)します。

Web API を自己ホストする場合は、上で直接ルーティング テーブルを設定する必要があります、`HttpSelfHostConfiguration`オブジェクト。 詳細については、次を参照してください。 [Web API を自己ホスト](../older-versions/self-host-a-web-api.md)します。

ルーティング テーブル各エントリが含まれる、*ルート テンプレート*します。 Web API の既定のルート テンプレートは&quot;api/{controller}/{id}&quot;します。 このテンプレートで&quot;api&quot;はリテラルのパス セグメント、および {controller} と {id} はプレース ホルダー変数です。

Web API フレームワークでは、HTTP 要求を受信、ルーティング テーブルにルート テンプレートのいずれかに対して URI と照合しようとします。 ルートが一致しない場合、クライアントは、404 エラーを受け取ります。 たとえば、次の Uri では、既定のルートと一致します。

- api/連絡先
- /api/contacts/1
- /api/products/gizmo1

ただし、次の URI が一致しないがないため、 &quot;api&quot;セグメント。

- /連絡先/1

> [!NOTE]
> ルートで"api"を使用する理由は、ASP.NET MVC ルーティングとの衝突を避けるためです。 そうすることがあることができます&quot;連絡先/&quot; 、MVC コント ローラーに移動し、 &quot;api/連絡先&quot;Web API コント ローラーに移動します。 もちろん、この規則に満足できない場合は、既定のルーティング テーブルを変更できます。

一致するルートが見つかったら、Web API コント ローラーとアクションを選択します。

- Web API を追加、コント ローラーを検索する&quot;コント ローラー&quot;の値に、 *{controller}* 変数。
- アクションを検索するには、Web API は、HTTP 動詞を見てし、その HTTP 動詞名で始まる名前、アクションを探します。 たとえば、GET 要求で Web API を探しますアクションが付いた&quot;取得&quot;など&quot;GetContact&quot;または&quot;GetAllContacts&quot;します。 この規則は、取得、POST、PUT、DELETE、HEAD、オプション、および PATCH の動詞にのみ適用されます。 属性をコント ローラーを使用して、他の HTTP 動詞を有効にすることができます。 後で例を見ていきます。
- ルート テンプレートでは、その他のプレース ホルダー変数など *{id},* アクション パラメーターにマップされます。

例を見てみましょう。 次のコント ローラーを定義するとします。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

いくつか考えられる HTTP 要求、ごとに呼び出されるアクションと共に以下に示します。

| HTTP 動詞 | URI のパス | アクション | パラメーター |
| --- | --- | --- | --- |
| GET | api/製品 | GetAllProducts | *(なし)* |
| GET | api/製品/4 | GetProductById | 4 |
| Del | api/製品/4 | DeleteProduct | 4 |
| POST | api/製品 | *(一致なし)* |  |

なお、 *{id}* 、URI のセグメントは存在する場合にマップされます、 *id*アクションのパラメーター。 この例では、コント ローラーが 1、2 つの GET メソッドを定義します。、 *id*パラメーター、パラメーターなしのもう 1 つ。

また、コント ローラーを一切定義しませんので、POST 要求が失敗する注、&quot;投稿しています.&quot;メソッド。

## <a name="routing-variations"></a>ルーティングのバリエーション

前のセクションでは、ASP.NET Web API の基本的なルーティング メカニズムについて説明します。 このセクションでは、いくつかのバリエーションについて説明します。

### <a name="http-verbs"></a>HTTP 動詞

HTTP 動詞の名前付け規則を使用する代わりに、次の属性のいずれかのアクション メソッドを修飾することによって、アクションの HTTP 動詞を明示的に指定できます。

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

次の例では、`FindProduct`メソッドが GET 要求にマップされます。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

アクション、複数の HTTP 動詞を許可するまたは以外、GET、PUT、POST、DELETE、HEAD、オプション、および PATCH HTTP 動詞を許可するのを使用して、`[AcceptVerbs]`属性には、HTTP 動詞のリストを取得します。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>アクション名によるルーティング

既定のルーティング テンプレートでは、Web API は HTTP 動詞を使用して、アクションを選択します。 ただし、アクション名が URI に含まれている場所のルートを作成することもできます。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

このルート テンプレートで、 *{action}* パラメーター名、コント ローラー アクション メソッド。 このスタイルのルーティングでは、属性を使用して、使用できる HTTP 動詞を指定します。 たとえば、コント ローラーに次のメソッドがあるとします。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

「Api/製品/詳細/1」の GET 要求をここにマップ、`Details`メソッド。 このスタイルのルーティングは、ASP.NET MVC でのような RPC スタイルの API の適切な場合があります。

アクション名を使用してオーバーライドすることができます、`[ActionName]`属性。 次の例では、2 つのアクションにマップされる&quot;api/製品/サムネイル/*id*します。GET をサポートしている 1 つと、その他の投稿をサポートしています。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>非アクション

メソッドがアクションとして呼び出されることを防ぐために使用して、`[NonAction]`属性。 これに通知フレームワーク メソッドは、アクションではない場合でも、ルーティングの規則はそれ以外の場合と一致します。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>関連項目

このトピックでは、ルーティングの概要が提供されています。 詳細については、次を参照してください。[ルーティングとアクションの選択](routing-and-action-selection.md)、が正確におよび方法について説明フレームワーク ルートへの URI と一致する、コント ローラーを選択します。 次に呼び出すアクションを選択します。
