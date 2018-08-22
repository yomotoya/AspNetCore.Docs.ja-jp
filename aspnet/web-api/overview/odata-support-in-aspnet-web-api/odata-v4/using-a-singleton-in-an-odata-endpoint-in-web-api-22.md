---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Web API 2.2 を使用して OData v4 でシングルトンを作成する |Microsoft Docs
author: rick-anderson
description: このトピックでは、Web API 2.2 で OData エンドポイントでシングルトンを定義する方法を示します。
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7562a90ae34b216dca2dd3cf541d086585735212
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833306"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Web API 2.2 を使用して OData v4 でシングルトンを作成します。
====================
Zoe ルオ語で

> これまでは、エンティティは、エンティティ セット内にカプセル化された場合にのみアクセスできます。 OData v4、シングルトンおよびコンテインメント、web Api 2.2 をサポートする 2 つの追加オプションを提供します。


この記事では、Web API 2.2 で OData エンドポイントでシングルトンを定義する方法を示します。 どのようなシングルトン、およびこれを使用する利点については、次を参照してください。[シングルトンを使用して、特別なエンティティを定義する](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx)します。 Web api OData V4 エンドポイントを作成するを参照してください。[作成 OData v4 エンドポイントを使用して ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)します。 

次のデータ モデルを使用して Web API プロジェクトでシングルトンを作成します。

![データ モデル](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

という名前の単一`Umbrella`の種類に基づいて定義されます`Company`、およびエンティティ セットの名前付き`Employees`の種類に基づいて定義されます`Employee`します。

このチュートリアルで使用するソリューションをからダウンロードできます[CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)します。

## <a name="define-the-data-model"></a>データ モデルを定義する

1. CLR 型を定義します。

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. CLR 型に基づく EDM モデルを生成します。

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    ここでは、`builder.Singleton<Company>("Umbrella")`という名前のシングルトンを作成するモデル ビルダーに指示`Umbrella`EDM モデル。

    生成されるメタデータは、次のようになります。

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    メタデータから確認できます、ナビゲーション プロパティ`Company`で、`Employees`エンティティ セットは、シングルトンにバインドされて`Umbrella`します。 バインディングはによって自動的に行われます`ODataConventionModelBuilder`、のでのみ`Umbrella`が、`Company`型。 使用することができます、モデル内であいまいさがある場合`HasSingletonBinding`シングルトン; にナビゲーション プロパティを明示的にバインドするには`HasSingletonBinding`を使用して同じ効果があります、 `Singleton` CLR 型の定義内の属性。

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>単一のコント ローラーを定義します。

単一のコント ローラーが継承、EntitySet のコント ローラーのような`ODataController`、単一のコント ローラー名を指定する必要があります`[singletonName]Controller`します。

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

さまざまな種類の要求を処理するためには、アクションをコント ローラーに事前定義する必要があります。 **属性ルーティング**web Api 2.2 で既定で有効です。 たとえば、処理クエリを実行するアクションを定義する`Revenue`から`Company`属性ルーティングを使用して、次を使用します。

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

各アクションの属性を定義する場合は、次の操作の定義だけ[OData ルーティング規約](../odata-routing-conventions.md)します。 キーが、単一のクエリを実行するために必要ないため、単一のコント ローラーで定義されたアクションは entityset コント ローラーで定義されたアクションとは若干異なります。

リファレンスについては、単一のコント ローラーのすべてのアクション定義のメソッド シグネチャは、以下に示します。

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

基本的に、これはサービス側で行う必要があるすべてです。 [サンプル プロジェクト](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)すべてのソリューションおよびシングルトンを使用する方法を示す OData クライアント コードが含まれています。 クライアントは次の手順に従って構築[OData v4 クライアント アプリを作成する](create-an-odata-v4-client-app.md)します。

. 

*Leo Hu に改訂された記事については、元のコンテンツいただきありがとうございます。*
