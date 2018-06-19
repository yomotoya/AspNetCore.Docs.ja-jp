---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: ASP.NET Web API 2.1 で BSON サポート |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2018
ms.locfileid: "27984584"
---
<a name="bson-support-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 で BSON サポート
====================
によって[Mike Wasson](https://github.com/MikeWasson)

Web API 2.1 には、BSON のサポートが導入されています。 このトピックでは、Web API コント ローラー (サーバー側) と .NET クライアント アプリで BSON を使用する方法を示します。

## <a name="what-is-bson"></a>BSON とは何ですか。

[BSON](http://bsonspec.org/)バイナリのシリアル化形式です。 "BSON"は"Binary JSON"を表しますが、BSON と JSON がまったく違う方法でシリアル化します。 BSON は「JSON のような」、オブジェクトが JSON のような名前と値のペアとして表されるためです。 JSON とは異なり、数値データ型がバイト、文字列ではありませんとして格納されます。

BSON は、軽量、簡単には、scan、エンコードとデコードに高速されるようにするために設計されました。

- BSON は JSON のサイズと同等です。 データによっては、BSON ペイロードは、JSON ペイロードより小さいか大きいする可能性があります。 イメージ ファイルなど、バイナリ データをシリアル化するため、バイナリ データが base64 でエンコードされたため BSON は JSON よりも小さいです。
- BSON ドキュメントは、スキャン、パーサーはそれらをデコードすることがなく要素をスキップすることができますので、長さフィールドを持つ要素が前付くので簡単です。
- エンコードとデコードは、数値データ型がない文字列、数値として格納されているため効率的、です。

.NET クライアント アプリなどのネイティブ クライアントは、BSON を使用して、JSON または XML などのテキスト ベース形式の代わりに活用できます。 ブラウザー クライアント用可能性がありますする JSON のオプションを使用するので、JavaScript が JSON ペイロードを直接変換できます。

幸いにも、Web API を使用して[コンテンツ ネゴシエーション](content-negotiation.md)API の両方の形式をサポートし、クライアントを選択できるよう、します。

## <a name="enabling-bson-on-the-server"></a>サーバー上の BSON を有効にします。

構成では、Web API、追加、 **BsonMediaTypeFormatter**フォーマッタのコレクション。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

現在、クライアントは、「application/bson」を要求する Web API には、BSON フォーマッタが使用されます。

BSON を他のメディア タイプに関連付ける、SupportedMediaTypes コレクションに追加します。 次のコードでは、サポートされているメディアの種類に"application/vnd.contoso"を追加します。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>HTTP セッションの例

この例では、単純な Web API コント ローラーと次のモデル クラスを使用します。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

クライアントは、次の HTTP 要求を送信可能性があります。

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

応答を次に示します。

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

ここでのバイナリ データは置き換えた&quot;.&quot;文字です。 Fiddler の番組を生の 16 進値の次のスクリーン ショット。

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>BSON HttpClient の併用

.NET クライアント アプリで BSON フォーマッタを使用することができます**HttpClient**です。 詳細については**HttpClient**を参照してください[Web API から、.NET クライアントを呼び出す](../advanced/calling-a-web-api-from-a-net-client.md)です。

次のコードとを受け取ってする BSON、応答で BSON ペイロードを逆シリアル化 GET 要求を送信します。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

BSON を要求するには、サーバーから、するには、「application/bson」に Accept ヘッダーを設定します。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

応答本文を逆シリアル化を使用して、 **BsonMediaTypeFormatter**です。 このフォーマッタが既定のフォーマッタのコレクションで応答本体を読み取るときに指定する必要があるためです。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

次の例では、BSON を含む POST 要求を送信する方法を示します。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

このコードの多くは、前の例と同じです。 しかし、 **PostAsync**メソッドを指定して**BsonMediaTypeFormatter**フォーマッタとして。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>最上位のプリミティブ型をシリアル化します。

BSON のすべてのドキュメントでは、キー/値ペアの一覧を示します。BSON 仕様では、整数や文字列などの単一の raw 値をシリアル化するための構文は定義されていません。

このような制限を回避する、 **BsonMediaTypeFormatter**は特殊なケースとしてプリミティブ型を処理します。 をシリアル化する前に、値に変換キー/値ペア"Value"キーにします。 たとえば、API コント ローラーは、整数を返します。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

をシリアル化する前に BSON フォーマッタは、これを次のキー/値ペアに変換します。

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

逆シリアル化するときに、フォーマッタは、元の値にデータを変換します。 ただし、異なる BSON パーサーを使用するクライアントは、web API は、生の値を返す場合、このケースを処理する必要があります。 一般に、生の値ではなく、構造化データを返すことを考慮する必要があります。

## <a name="additional-resources"></a>その他のリソース

[Web API BSON サンプル](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[メディア フォーマッタ](media-formatters.md)
