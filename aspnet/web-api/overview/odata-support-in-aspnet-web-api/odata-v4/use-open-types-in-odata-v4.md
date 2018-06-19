---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: ASP.NET web API OData v4 の種類を開く |Microsoft ドキュメント
author: microsoft
description: OData v4、オープン型の種類の定義で宣言されているプロパティに加え、動的なプロパティを含む stuctured 型です。 開く...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fe67b9a11a82b55d5f3e0e5f1b0cee10a58833d2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
ms.locfileid: "29153273"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>ASP.NET web API OData v4 の種類を開く
====================
によって[Microsoft](https://github.com/microsoft)

> OData v4 に、*オープン型*種類の定義で宣言されているプロパティに加え、動的なプロパティを含む stuctured 型です。 オープン型では、データ モデルに柔軟性を追加できます。 このチュートリアルでは、ASP.NET Web API OData のオープン型の使用方法を示します。
> 
> このチュートリアルでは、ASP.NET Web API で OData エンドポイントを作成する方法を既に把握している前提としています。 いない場合は、最初にお読み[OData v4 エンドポイントを作成する](create-an-odata-v4-endpoint.md)最初。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - Web API OData 5.3
> - OData v4


最初に、いくつかの OData 用語:

- エンティティの種類: キーを使用して構造化された型。
- 複合型: キーがない場合は構造化型です。
- オープン型: 動的なプロパティを持つ型。 エンティティ型と複合型の両方を開くことができます。

動的プロパティの値は、プリミティブ型、複合型、または列挙型を指定できます。または、それらの型のいずれかのコレクション。 オープン型の詳細については、次を参照してください。、 [OData v4 仕様](http://www.odata.org/documentation/odata-version-4-0/)です。

## <a name="install-the-web-odata-libraries"></a>Web OData ライブラリをインストールします。

NuGet パッケージ マネージャーを使用して、最新の Web API OData ライブラリをインストールします。 [パッケージ マネージャー コンソール] ウィンドウで。

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>CLR 型を定義します。

CLR 型として EDM モデルを定義することで開始します。

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Entity Data Model (EDM) を作成すると、

- `Category`列挙型です。
- `Address`複合型です。 (がない、キー、エンティティ型ではありません。)
- `Customer`エンティティ型。 (キーを持つ)。
- `Press`開いている複合型です。
- `Book`開いているエンティティ型。

オープン型を作成するには、CLR 型が型のプロパティをいる必要があります`IDictionary<string, object>`、動的なプロパティを保持します。

## <a name="build-the-edm-model"></a>EDM モデルを構築します。

使用する場合**ODataConventionModelBuilder** 、EDM を作成する`Press`と`Book`の有無に基づいて、オープン型として自動的に追加されて、`IDictionary<string, object>`プロパティです。

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

ビルドすることも、EDM、明示的に使用する**ため**です。

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>OData コント ローラーを追加します。

次に、OData コント ローラーを追加します。 このチュートリアルでは、簡略化されたコント ローラーだけでサポート GET および POST 要求、およびをエンティティを格納する、インメモリ リストを使用して使用します。

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

注意して最初`Book`インスタンスには、動的なプロパティがありません。 2 番目`Book`インスタンスには、次の動的なプロパティ。

- 「公開」プリミティブ型。
- "Authors"プリミティブ型のコレクション。
- 「カテゴリー」: 列挙型のコレクション。

また、`Press`そのプロパティ`Book`インスタンスには、次の動的なプロパティ。

- 「ブログ」プリミティブ型。
- "Address": Complex type

## <a name="query-the-metadata"></a>メタデータのクエリ

OData メタデータ ドキュメントを取得する GET 要求を送信`~/$metadata`です。 応答本文は、次のようになります。

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

メタデータ ドキュメントからは、ことを確認できます。

- `Book`と`Press`の値の型、`OpenType`属性が true です。 `Customer`と`Address`型はこの属性がありません。
- `Book`エンティティ型が宣言されている 3 つのプロパティ: ISBN、タイトル、およびキーを押します。 OData メタデータを含まない、 `Book.Properties` CLR クラスからプロパティです。
- 同様に、`Press`複合型が宣言された 2 つのプロパティ: 名前とカテゴリ。 メタデータを含まない、 `Press.DynamicProperties` CLR クラスからプロパティです。

## <a name="query-an-entity"></a>エンティティのクエリ

ISBN を book と等しい「978-0-7356-7942-9」を取得する GET 要求を送信`~/Books('978-0-7356-7942-9')`です。 応答本文は、次のようになります。 (読みやすくするためにインデントされます。)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

動的プロパティはインラインで宣言されたプロパティに含まれて、ことに注意してください。

## <a name="post-an-entity"></a>エンティティの投稿

Book エンティティを追加するに POST 要求を送信`~/Books`です。 クライアントは、要求のペイロードで動的なプロパティを設定できます。

要求の例を次に示します。 "Price"と「公開済み」のプロパティに注意してください。

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

コント ローラーのメソッドにブレークポイントを設定する場合、Web API にこれらのプロパティが追加されたことを確認できます、`Properties`ディクショナリ。

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>その他のリソース

[OData オープン型のサンプル](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
