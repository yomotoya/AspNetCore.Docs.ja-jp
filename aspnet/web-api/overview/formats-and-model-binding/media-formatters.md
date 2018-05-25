---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: ASP.NET Web API 2 に、メディア フォーマッタ |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 1cb1c7e0f832a0a0160276fbd41facc017e2ae3e
ms.sourcegitcommit: 50d40c83fa641d283c097f986dde5341ebe1b44c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/22/2018
---
<a name="media-formatters-in-aspnet-web-api-2"></a>ASP.NET Web API 2 に、メディア フォーマッタ
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

このチュートリアルでは、ASP.NET Web API で他のメディア形式をサポートする方法を示します。

## <a name="internet-media-types"></a>インターネットのメディアの種類

MIME の種類とも呼ばれる、メディアの種類では、一部のデータの形式を識別します。 HTTP では、メディアの種類は、メッセージ本文の形式を記述します。 メディアの種類は、2 つの文字列、型とサブタイプで構成されます。 例えば:

- text/html
- image/png
- application/json

HTTP メッセージにエンティティ ボディが含まれている場合、Content-type ヘッダーは、メッセージ本文の形式を指定します。 これは、受信側に、メッセージ本文の内容を解析する方法を示しています。

たとえば、HTTP 応答 PNG イメージが含まれている場合、応答は、次のヘッダーがあります。

[!code-console[Main](media-formatters/samples/sample1.cmd)]

クライアントは、要求メッセージを送信した場合は、Accept ヘッダーを含めることできます。 Accept ヘッダーでは、サーバーからのメディアの種類、クライアント サーバーがように指示します。 例えば:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

このヘッダーは、HTML、XHTML、または XML のいずれかに、クライアントが要求しているサーバーに指示します。

メディアの種類は、Web API をシリアル化および HTTP メッセージの本文を逆シリアル化する方法を決定します。 Web API XML、JSON、BSON、およびフォーム エンコード データの組み込みサポートがあり、追加のメディアの種類をサポートするには記述して、*メディア フォーマッタ*です。

メディア フォーマッタを作成するには、これらのクラスのいずれかから派生します。

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx)です。 このクラスは非同期の読み取りと書き込みを行うメソッドです。
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx)です。 このクラスから派生**MediaTypeFormatter**が、同期読み取り/書き込みのメソッドを使用します。

派生する**BufferedMediaTypeFormatter**のための非同期コードではありませんが、I/O 中に呼び出し元のスレッドをブロックすることができますも意味が単純になります。

## <a name="example-creating-a-csv-media-formatter"></a>例: CSV メディア フォーマッタを作成します。

次の例では、コンマ区切り値 (CSV) 形式に Product オブジェクトをシリアル化できるメディア タイプ フォーマッタを示します。 この例は、このチュートリアルで定義されている製品の種類を使用して[CRUD 操作をサポートする Web API を作成する](../older-versions/creating-a-web-api-that-supports-crud-operations.md)です。 Product オブジェクトの定義を次に示します。

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

派生するクラスを定義 CSV フォーマッタを実装する**BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

コンス トラクターでは、フォーマッタをサポートしているメディアの種類を追加します。 この例では、フォーマッタは、1 つのメディアの種類をサポートしています&quot;テキスト/csv&quot;:。

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

上書き、 **CanWriteType**フォーマッタ種類を示すためにメソッドがシリアル化できます。

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

この例では、フォーマッタをシリアル化できる単一`Product`オブジェクトのコレクションおよび`Product`オブジェクト。

同様に、オーバーライド、 **CanReadType**フォーマッタ種類を示すためにメソッドを逆シリアル化できます。 この例では、フォーマッタはサポートしていません、逆シリアル化メソッドだけを返すように**false**です。

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

最後に、オーバーライド、 **WriteToStream**メソッドです。 このメソッドは、ストリームに書き込むことで、型をシリアル化します。 フォーマッタでは、逆シリアル化をサポートする場合は上書きも、 **ReadFromStream**メソッドです。

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>メディア フォーマッタを Web API パイプラインに追加します。

使用して、メディア タイプ フォーマッタを Web API パイプラインを追加する、**フォーマッタ**プロパティを**HttpConfiguration**オブジェクト。

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>文字エン コード

必要に応じて、メディア フォーマッタは、utf-8 または ISO 8859-1 などの複数の文字エンコーディングをサポートできます。

コンス トラクターの 1 つ以上追加[System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx)型を**SupportedEncodings**コレクション。 既定の最初のエンコーディングを格納します。

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

**WriteToStream**と**ReadFromStream**メソッドを呼び出して[MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx)優先文字エン コードを選択します。 このメソッドでは、サポートされているエンコーディングの一覧に対して、要求ヘッダーと一致します。 使用して、返された**エンコード**読み取りまたは書き込みストリームをする場合。

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
