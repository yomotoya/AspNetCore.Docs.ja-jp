---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: ASP.NET Web API 2.1 の BSON サポート |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7c5e763d92295a83b3431e9ec6e305b07f8a64d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370495"
---
<a name="bson-support-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 の BSON サポート
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

Web API 2.1 の BSON サポートを紹介します。 このトピックでは、Web API コント ローラー (サーバー側) と .NET クライアント アプリでは、BSON を使用する方法を示します。

## <a name="what-is-bson"></a>BSON とは何ですか。

[BSON](http://bsonspec.org/)はバイナリ シリアル化形式です。 "BSON"は"Binary JSON"を表しますが、BSON と JSON はまったく違う方法でシリアル化します。 BSON は、「JSON のような」、オブジェクトが JSON のような名前と値のペアとして表されるためです。 JSON とは異なり、数値データ型がバイト、文字列ではないとして格納されます。

BSON は、軽量、簡単にスキャンする、エンコードとデコードに高速に設計されています。

- BSON は JSON のサイズと同等です。 データによっては、BSON ペイロードは JSON ペイロードより小さいか大きいする可能性があります。 イメージ ファイルなどのバイナリ データをシリアル化するため、BSON は、バイナリ データは base64 でエンコードされたがないため、JSON よりも小さい。
- BSON ドキュメントは、簡単にスキャンするため、パーサーは、それらをデコードすることがなく要素をスキップできますので長のフィールドでは要素が付きます。
- エンコードおよびデコードは数値データ型がない文字列、数値として格納されているため、効率的です。

.NET クライアント アプリなどのネイティブ クライアントは、BSON を使用して、JSON や XML などのテキスト ベース形式の代わりに活用できます。 ブラウザー クライアントはおそらく json では、引き続き使用ためにする JavaScript は、JSON ペイロードに直接変換できます。

さいわい、Web API を使用して[コンテンツ ネゴシエーション](content-negotiation.md)ので、API が両方の形式をサポートして、クライアントを選択します。

## <a name="enabling-bson-on-the-server"></a>サーバー上の BSON を有効にします。

Web API 構成を追加、 **BsonMediaTypeFormatter**フォーマッタのコレクションにします。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

これで、クライアントは、"application/bson"を要求する Web API は BSON フォーマッタに使用します。

BSON を他のメディアの種類に関連付ける、SupportedMediaTypes コレクションに追加します。 次のコードでは、サポートされているメディアの種類を"application/vnd.contoso"を追加します。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>例 HTTP セッション

この例では、単純な Web API コント ローラーと次のモデル クラスを使用します。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

クライアントは、次の HTTP 要求を送信可能性があります。

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

応答を次に示します。

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

ここでのバイナリ データに置き換えました&quot;.&quot;文字。 Fiddler の表示から生の 16 進値の次のスクリーン ショット。

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>BSON HttpClient の併用

.NET クライアント アプリは BSON フォーマッタを使用できます**HttpClient**します。 詳細については**HttpClient**を参照してください[Web API から、.NET クライアントを呼び出す](../advanced/calling-a-web-api-from-a-net-client.md)します。

次のコードは、BSON を受け取り、応答で BSON ペイロードを逆シリアル化する GET 要求を送信します。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

サーバーから BSON を要求するには、"application/bson"に Accept ヘッダーを設定します。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

応答本文を逆シリアル化を使用して、 **BsonMediaTypeFormatter**します。 このフォーマッタでない既定のフォーマッタのコレクションのため、応答本文を読み取るときに指定する必要は。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

次の例では、BSON を含む POST 要求を送信する方法を示します。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

このコードの多くは、前の例と同じです。 しかし、 **PostAsync**メソッドでは、指定**BsonMediaTypeFormatter**フォーマッタと。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>最上位レベルのプリミティブ型をシリアル化します。

すべての BSON ドキュメントでは、キー/値ペアの一覧を示します。BSON 仕様では、整数や文字列などの単一の raw 値をシリアル化するための構文が定義されていません。

このような制限を回避する、 **BsonMediaTypeFormatter**プリミティブ型を特殊なケースとして扱われます。 をシリアル化する前に、値に変換しますキー/値ペア"Value"キーを使用します。 たとえば、API コント ローラーは、整数を返します。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

をシリアル化する前に BSON フォーマッタは、これを次のキー/値ペアに変換します。

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

逆シリアル化するときに、フォーマッタは、元の値に、データを変換します。 ただし、異なる BSON パーサーを使用するクライアントは、web API は、生の値を返す場合、このケースを処理する必要があります。 一般に、生の値ではなく、構造化データを返すことを検討してください。

## <a name="additional-resources"></a>その他のリソース

[Web API の BSON サンプル](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[メディア フォーマッタ](media-formatters.md)
