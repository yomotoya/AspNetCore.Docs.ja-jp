---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: ASP.NET Web API を使用した OData v4 の種類を開く |Microsoft Docs
author: microsoft
description: OData v4 のオープン型は、型定義で宣言されている任意のプロパティだけでなく、動的プロパティを含む stuctured 型です。 開く...
ms.author: aspnetcontent
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 560d47e0dc451847311eb9e2327190eed2209546
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832271"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>ASP.NET Web API を使用した OData v4 の種類を開く
====================
によって[Microsoft](https://github.com/microsoft)

> OData v4 の*オープン型*は型定義で宣言されている任意のプロパティだけでなく、動的プロパティを含む stuctured 型です。 オープン型では、データ モデルに柔軟性を追加できます。 このチュートリアルでは、ASP.NET Web API OData でオープン型を使用する方法を示します。
> 
> このチュートリアルでは、ASP.NET Web API で OData エンドポイントを作成する方法を既に把握している前提としています。 そうでない場合は、を読み取ることによって開始[OData v4 エンドポイントを作成](create-an-odata-v4-endpoint.md)最初。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - Web API OData 5.3
> - OData v4


まず、OData の用語。

- エンティティの種類: キーを持つ構造化型。
- 複合型: キーのない構造化型。
- オープン型: 動的プロパティを持つ型。 エンティティ型と複合型の両方を開くことができます。

動的なプロパティの値は、プリミティブ型、複合型、または列挙型を指定できます。または、それらの型のいずれかのコレクション。 オープン型の詳細については、次を参照してください。、 [OData v4 仕様](http://www.odata.org/documentation/odata-version-4-0/)します。

## <a name="install-the-web-odata-libraries"></a>Web の OData ライブラリをインストールします。

NuGet パッケージ マネージャーを使用して、最新の Web API OData ライブラリをインストールします。 [パッケージ マネージャー コンソール] ウィンドウで。

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>CLR 型を定義します。

CLR 型として EDM モデルを定義することで開始します。

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Entity Data Model (EDM) の作成時に

- `Category` 列挙型です。
- `Address` 複合型。 (がない、キー、ため、エンティティ型ではありません。)
- `Customer` エンティティ型。 (キーを持つ)。
- `Press` 開いている複雑な型です。
- `Book` オープンなエンティティ型。

オープン型を作成するには、CLR 型が型のプロパティをいる必要があります`IDictionary<string, object>`、動的なプロパティが格納されます。

## <a name="build-the-edm-model"></a>EDM モデルを構築します。

使用する場合**ODataConventionModelBuilder** 、EDM を作成する`Press`と`Book`の有無に基づいて、オープン型として自動的に追加されますが、`IDictionary<string, object>`プロパティ。

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

ビルドすることも、EDM に明示的を使用して**に**します。

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>OData コント ローラーを追加します。

次に、OData コント ローラーを追加します。 このチュートリアルでは、だけでサポートを取得および POST 要求、しするエンティティを格納するメモリ内のリストを使用して簡略化されたコント ローラーを使用します。

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

いることを確認最初`Book`インスタンスに動的プロパティがありません。 2 番目の`Book`インスタンスには、次の動的プロパティ。

- 「発行」: プリミティブ型
- "Authors": プリミティブ型のコレクション
- 「カテゴリー」: 列挙型のコレクション。

また、`Press`プロパティを`Book`インスタンスには、次の動的プロパティ。

- 「ブログ」: プリミティブ型
- "Address": Complex type

## <a name="query-the-metadata"></a>メタデータをクエリします。

OData メタデータ ドキュメントを取得する GET 要求を送信`~/$metadata`します。 応答本文は、次のようになります。

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

メタデータ ドキュメントをことを確認できます。

- `Book`と`Press`の値の型、`OpenType`属性は true。 `Customer`と`Address`型は、この属性を必要はありません。
- `Book`エンティティ型が宣言された 3 つのプロパティ: ISBN、タイトル、およびキーを押します。 OData メタデータは含まれません、 `Book.Properties` CLR クラスからプロパティ。
- 同様に、`Press`複合型が宣言された 2 つのプロパティ: 名前とカテゴリ。 メタデータは含まれません、 `Press.DynamicProperties` CLR クラスからプロパティ。

## <a name="query-an-entity"></a>エンティティを照会します。

書籍と ISBN と等しい「978-0-7356-7942-9」を取得するに GET 要求を送信`~/Books('978-0-7356-7942-9')`します。 応答本文は、次のようになります。 (読みやすくするためにインデントします。)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

動的プロパティはインラインで宣言されたプロパティであることに注目してください。

## <a name="post-an-entity"></a>エンティティを投稿します。

書籍エンティティを追加するに POST 要求を送信`~/Books`します。 クライアントは、要求ペイロード内の動的プロパティを設定できます。

要求の例を次に示します。 "Price"と「発行済み」のプロパティに注意してください。

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

コント ローラー メソッドにブレークポイントを設定する場合、Web API がこれらのプロパティを追加することを確認できます、`Properties`ディクショナリ。

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>その他のリソース

[OData のオープン型のサンプル](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
