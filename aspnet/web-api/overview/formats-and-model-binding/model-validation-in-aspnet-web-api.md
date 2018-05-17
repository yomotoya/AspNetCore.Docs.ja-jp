---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: モデルの ASP.NET Web API での検証 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 409a91eceb8baa48a7dded1b850d59a27cec2c60
ms.sourcegitcommit: 5ae0c125ee3bbd324edef3818d1d160f4dd84602
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
---
<a name="model-validation-in-aspnet-web-api"></a>ASP.NET Web API でのモデルの検証
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

クライアントは、web API へのデータを送信するときに多くの場合、検証するデータ処理を実行する前にします。 この記事に、モデルの注釈を付け、データ検証のため、注釈を使用して、web API で検証エラーの処理方法を示します。

## <a name="data-annotations"></a>データの注釈

ASP.NET Web API から属性を使用することができます、 [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations)名前空間をモデルに検証規則のプロパティを設定します。 次のようなモデルを考慮してください。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

ASP.NET MVC のモデルの検証を使用した場合、使い慣れたこれがはずです。 **必要**属性を表示する、`Name`プロパティを null することはできません。 **範囲**いる属性という`Weight`0 ~ 999 でなければなりません。

クライアントが次の JSON 表現で POST 要求を送信するとします。

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

クライアントが含まれていないことを確認できます、`Name`としてマークされているプロパティが必要です。 Web API に JSON を変換するときに、`Product`インスタンスを検証して、`Product`検証の属性に対してです。 コント ローラー動作では、モデルが有効かどうかを確認できます。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

モデルの検証では、クライアントのデータを安全なことは保証されません。 追加の検証は、アプリケーションの他のレイヤーで必要な可能性があります。 (たとえば、データ層可能性があります外部キー制約を適用します。)チュートリアル[Entity Framework と Web API を使用して](../data/using-web-api-with-entity-framework/part-1.md)これらの問題について説明します。

**「過少転記」**: 過少ポストされた内容の場合は、クライアントがいくつかのプロパティを残します。 たとえば、クライアントは、次に送信します。

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

ここでは、クライアントがの値を指定しなかった`Price`または`Weight`です。 JSON フォーマッタは、0、不足しているプロパティを既定値を割り当てます。

![](model-validation-in-aspnet-web-api/_static/image1.png)

モデルの状態は無効は、これらのプロパティの有効な値は 0 です。 これは問題であるかどうかは、シナリオによって異なります。 たとえば、操作では、更新、可能性がある"zero"と"されていません を区別するには 値を設定するクライアントに強制するように、プロパティの null 値を許容し、設定、**必要**属性。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"過剰ポスティング**: クライアントが送信も*詳細*予想以上にデータ。 例えば:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

ここでは、JSON がの存在しないプロパティ ("Color") が含まれます、`Product`モデル。 この例では、JSON フォーマッタでは、単にこの値を無視します。 (XML フォーマッタは同じです。)過剰な投稿では、モデルにプロパティが読み取り専用にしようとしたものがある場合に問題が発生します。 例えば:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

ユーザーを更新しない、`IsAdmin`プロパティ自体を管理者に昇格させると! 最も安全な方法は、送信するクライアントが許可されていると一致するモデル クラスを使用するは。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Brad Wilson のブログの投稿"[入力検証とします。ASP.NET MVC での検証をモデル](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"過少ポストされた内容と過剰ポストされた内容の詳細がします。 投稿は、ASP.NET MVC 2 については、問題が Web API にも関係します。


## <a name="handling-validation-errors"></a>検証エラーの処理

Web API は、検証が失敗したときに、クライアントにエラーを自動的に返しません。 モデルの状態を確認し、適切に対応するコント ローラー アクションの責任です。

コント ローラーのアクションが呼び出される前に、モデルの状態を確認するアクション フィルターを作成することもできます。 次のコード例に示します。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

モデルの検証に失敗した場合、このフィルターには、検証エラーを含む HTTP 応答が返されます。 その場合は、コント ローラーのアクションは呼び出されません。

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

すべての Web API コント ローラーにフィルターを適用するフィルターのインスタンスに追加、 **HttpConfiguration.Filters**構成中にコレクション。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

個々 のコント ローラーまたはコント ローラーのアクションを属性としてフィルターを設定することもできます。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
