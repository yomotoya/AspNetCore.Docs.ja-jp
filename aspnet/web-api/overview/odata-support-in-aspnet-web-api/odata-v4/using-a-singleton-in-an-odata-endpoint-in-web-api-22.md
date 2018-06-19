---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Web API 2.2 を使用して OData v4 のシングルトンを作成 |Microsoft ドキュメント
author: rick-anderson
description: このトピックでは、Web API 2.2 で OData エンドポイントでシングルトンを定義する方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508211"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Web API 2.2 を使用して OData v4 のシングルトンを作成します。
====================
ゾーイ Luo によって

> 従来、エンティティは、エンティティ セット内にカプセル化された場合にのみアクセス可能性があります。 OData v4 がシングルトンや含有関係、WebAPI 2.2 をどちらもサポートしている 2 つの追加オプションを提供します。


この記事では、Web API 2.2 で OData エンドポイントでシングルトンを定義する方法を示します。 どのようなシングルトンし、を使用する利点については、次を参照してください。[シングルトンを使用して、特殊なエンティティを定義する](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx)です。 Web api OData V4 エンドポイントを作成するを参照してください。[作成 OData v4 エンドポイントを使用する ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)です。 

次のデータ モデルを使用して、Web API プロジェクトのシングルトンを作成します。

![データ モデル](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

という名前の単一`Umbrella`が定義される種類に基づいて`Company`、および名前付きセットのエンティティ`Employees`の種類に基づいて定義されます`Employee`です。

このチュートリアルで使用されるソリューションからダウンロードできます。 [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)です。

## <a name="define-the-data-model"></a>データ モデルを定義する

1. CLR 型を定義します。

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. CLR 型に基づき、EDM モデルを生成します。

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    ここでは、`builder.Singleton<Company>("Umbrella")`シングルトンという名前を作成するモデル ビルダーに指示`Umbrella`EDM モデル。

    生成されたメタデータは、次のようになります。

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    メタデータからことが分かりますナビゲーション プロパティ`Company`で、`Employees`エンティティ セットは、シングルトンにバインドされて`Umbrella`です。 バインディングはによって自動的に行われます`ODataConventionModelBuilder`、のでのみ`Umbrella`が、`Company`型です。 使用することができます、モデルにあいまいさがある場合`HasSingletonBinding`; シングルトンにナビゲーション プロパティを明示的にバインドするには`HasSingletonBinding`を使用すると同じ効果を持つ、 `Singleton` CLR 型の定義内の属性。

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>単一のコント ローラーを定義します。

EntitySet コント ローラーと同様に、単一コント ローラーがから継承`ODataController`、単一コント ローラー名を指定する必要があります`[singletonName]Controller`です。

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

さまざまな種類の要求を処理するために、コント ローラーで事前に定義するアクションが必要です。 **属性のルーティング**WebApi 2.2 で既定で有効にします。 たとえば、クエリを処理するアクションを定義する`Revenue`から`Company`属性がルーティングを使用して、次を使用します。

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

各アクションの属性を定義する場合は、次の操作を定義するだけ[OData ルーティング規約](../odata-routing-conventions.md)です。 キーがシングルトン クエリを実行するために必要ないため、単一コント ローラーで定義されたアクションは entityset コント ローラーで定義されたアクションとは若干異なります。

リファレンスについては、単一コント ローラー アクション定義のそれぞれのメソッドのシグネチャは、以下に示します。

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

基本的には、これは、サービス側で実行する必要がありますすべてです。 [サンプル プロジェクト](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)のすべてのソリューションと、シングルトンを使用する方法を示す OData クライアント コードが含まれています。 次の手順に従って、クライアントが組み込まれて[OData v4 クライアント アプリを作成する](create-an-odata-v4-client-app.md)です。

。 

*この記事の元のコンテンツ Leo %hu に感謝します。*
