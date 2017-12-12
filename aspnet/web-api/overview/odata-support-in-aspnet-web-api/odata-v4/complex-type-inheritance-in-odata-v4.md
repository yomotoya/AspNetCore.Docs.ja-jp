---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: "ASP.NET web API OData v4 の複合型の継承 |Microsoft ドキュメント"
author: microsoft
description: "OData v4 仕様では、複合型を別の複合型から継承できます。 (複合型は、キーがない場合は構造化型です)。Web API しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>ASP.NET web API OData v4 の複合型の継承
====================
によって[Microsoft](https://github.com/microsoft)

> OData v4 に従って[仕様](http://www.odata.org/documentation/odata-version-4-0/)、複合型が別の複合型から継承できます。 (A*複雑な*型は、キーのない構造化された型です)。Web API OData 5.3 は、複合型の継承をサポートします。
> 
> このトピックでは、複雑な継承の種類で entity data model (EDM) をビルドする方法を示します。 完全なソース コードでは、次を参照してください。[複合型の継承のサンプルの OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)です。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - Web API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>モデル階層

複合型の継承を示すためには、次のクラス階層を使用します。

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`抽象の複合型。 `Rectangle`、 `Triangle`、および`Circle`複合型から派生`Shape`、および`RoundRectangle`から派生した`Rectangle`です。 `Window`エンティティ型は、含む、`Shape`インスタンス。

ここでは、これらの型を定義する CLR クラスです。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>EDM モデルを構築します。

使用することができます、EDM を作成する**ODataConventionModelBuilder**CLR 型から継承関係を推測します。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

ビルドすることも、EDM、明示的に使用する**ため**です。 これにより、多くのコードを受け取りますが、EDM より詳細に制御できます。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

これら 2 つの例では、同じ EDM スキーマを作成します。

## <a name="metadata-document"></a>メタデータ ドキュメント

ここでは、OData メタデータ ドキュメント、複合型の継承を示すです。

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

メタデータ ドキュメントからは、ことを確認できます。

- `Shape`複合型は抽象クラスです。
- `Rectangle`、 `Triangle`、および`Circle`複合型が基本型を含める`Shape`です。
- `RoundRectangle`型に基本型が、`Rectangle`です。

## <a name="casting-complex-types"></a>複合型のキャスト

複合型のキャストはサポートされています。 たとえば、次のクエリのキャスト、`Shape`を`Rectangle`です。

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

応答ペイロードを次に示します。

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
