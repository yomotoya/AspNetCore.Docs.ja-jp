---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: ASP.NET Web API でのモデル検証 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/20/2012
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: be681a97a65a95a6bb83196d29c60ae750a523a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820014"
---
<a name="model-validation-in-aspnet-web-api"></a>ASP.NET Web API でモデルの検証
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

クライアントは、web API へのデータを送信するときに多くの場合、検証するデータ、処理を実行する前にします。 この記事では、モデルの注釈を設定、データの検証のため、注釈を使用して、web API で検証エラーを処理する方法を示しています。

## <a name="data-annotations"></a>データの注釈

ASP.NET Web api からの属性を使用することができます、 [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations)名前空間をモデルに検証規則のプロパティを設定します。 次のようなモデルを検討してください。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

ASP.NET MVC のモデルの検証を使用している場合この使い慣れたになります。 **必要**属性という、`Name`プロパティを null にすることはできません。 **範囲**属性という`Weight`0 ~ 999 にする必要があります。

クライアントが次の JSON 表現を含む POST 要求を送信するとします。

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

クライアントが含まれていないことを確認できます、`Name`としてマークされているプロパティが必要です。 Web API に JSON を変換するときに、`Product`インスタンス、検証、`Product`検証属性に対して。 コント ローラー アクションでは、モデルが有効かどうかを確認できます。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

モデルの検証では、クライアント データが安全であるは保証されません。 追加の検証は、アプリケーションの他のレイヤーで必要な場合があります。 (たとえば、データ層可能性があります外部キー制約を適用します。)このチュートリアル[Entity Framework で Web API を使用して](../data/using-web-api-with-entity-framework/part-1.md)これらの問題のいくつか解説します。

**「過小投稿」**: 過小転記クライアントがいくつかのプロパティを離れるときの動作。 たとえば、クライアントは、次に送信します。

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

ここでは、クライアントがの値を指定しなかった`Price`または`Weight`します。 JSON フォーマッタは、0 不足しているプロパティからの既定値を割り当てます。

![](model-validation-in-aspnet-web-api/_static/image1.png)

これらのプロパティの有効な値は 0 ですので、モデルの状態は有効です。 これは問題であるかどうかは、シナリオによって異なります。 たとえば、更新操作では、可能性がある「0」と「設定なし」とを区別 値を設定するクライアントを強制的に設定して null 許容にプロパティを作成、**必要**属性。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**「オーバーポスティング」**: クライアントが送信も*詳細*が予想よりもデータ。 例えば:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

JSON に存在しないプロパティ ("Color") が含まれていますここでは、`Product`モデル。 この場合は、JSON フォーマッタは、この値を無視するだけです。 (XML フォーマッタは同じです。)オーバーポスティング攻撃では、モデルに読み取り専用にしようとしたプロパティが設定されている場合に問題が発生します。 例えば:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

ユーザーを更新しない、`IsAdmin`プロパティ自体を管理者に昇格させるとします。 最も安全な戦略を送信するクライアントが許可されていると完全に一致するモデル クラスを使用することです。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Brad Wilson のブログの投稿"[入力の検証とします。ASP.NET MVC でのモデル検証](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"が過少投稿およびオーバーポスティング攻撃の詳細。 投稿は、ASP.NET MVC 2 の詳細については、問題は、Web API にも関連します。


## <a name="handling-validation-errors"></a>検証エラーの処理

Web API は、検証が失敗したときに、クライアントにエラーを自動的に返しません。 モデルの状態を確認し、適切に対応するコント ローラー アクションの責任です。

コント ローラー アクションが呼び出される前に、モデルの状態を確認するアクション フィルターを作成することもできます。 次のコード例に示します。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

モデルの検証に失敗した場合、このフィルターは、検証エラーを含む HTTP 応答を返します。 その場合は、コント ローラー アクションは呼び出されません。

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

すべての Web API コント ローラーにこのフィルターを適用するフィルターのインスタンスを追加、 **HttpConfiguration.Filters**構成中にコレクション。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

別のオプションでは、個々 のコント ローラーまたはコント ローラー アクションに属性として、フィルターを設定します。

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
