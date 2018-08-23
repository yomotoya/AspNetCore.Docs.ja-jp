---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: ASP.NET Web API 2 のメディア フォーマッタ |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 7b7ba2fb3f1bba0447e700c84a017266cba305e6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832401"
---
<a name="media-formatters-in-aspnet-web-api-2"></a>ASP.NET Web API 2 のメディア フォーマッタ
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

このチュートリアルでは、ASP.NET Web API で他のメディア形式をサポートする方法を示します。

## <a name="internet-media-types"></a>インターネット メディアの種類

MIME の種類とも呼ばれます。 メディアの種類は、データの一部の形式を識別します。 HTTP では、メディアの種類には、メッセージ本文の形式について説明します。 メディアの種類は、2 つの文字列、種類、およびサブタイプで構成されます。 例えば:

- text/html
- image/png
- application/json

HTTP メッセージにエンティティ ボディが含まれている場合、Content-type ヘッダーには、メッセージ本文の形式を指定します。 これは、受信側に、メッセージ本文の内容を解析する方法を指示します。

たとえば、HTTP 応答に PNG イメージが含まれている場合、応答は、次のヘッダーがあります。

[!code-console[Main](media-formatters/samples/sample1.cmd)]

クライアントは、要求メッセージを送信するときに、Accept ヘッダーに含めることができます。 Accept ヘッダーでは、サーバーを使用してメディアの種類、クライアント、サーバーを指示します。 例えば:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

このヘッダーは、HTML、XHTML、または XML のいずれかに、クライアントが要求しているサーバーに指示します。

Web API のシリアル化し、HTTP メッセージ本文を逆シリアル化、メディアの種類を決定します。 Web API が XML、JSON、BSON、および url エンコード フォーム データの組み込みサポートと、追加のメディアの種類をサポートするには記述することで、*メディア フォーマッタ*します。

メディア フォーマッタを作成するには、これらのクラスのいずれかから派生します。

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx)します。 このクラスは非同期の読み取りと書き込みを行うメソッドです。
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx)します。 このクラスから派生**MediaTypeFormatter**が、同期読み取り/書き込みのメソッドを使用します。

派生する**BufferedMediaTypeFormatter**の非同期コードではありませんが、I/O 中に呼び出し元のスレッドをブロックできますをも意味があるため、単純なは。

## <a name="example-creating-a-csv-media-formatter"></a>例: CSV のメディア フォーマッタを作成します。

次の例は、コンマ区切り値 (CSV) 形式に Product オブジェクトをシリアル化できるメディア タイプ フォーマッタの一覧です。 この例は、このチュートリアルで定義されている製品の種類を使用して[そのサポートの CRUD 操作を作成するには、Web API](../older-versions/creating-a-web-api-that-supports-crud-operations.md)します。 Product オブジェクトの定義を次に示します。

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

CSV フォーマッタを実装するから派生したクラスを定義**BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

コンス トラクターに、フォーマッタでサポートされるメディアの種類を追加します。 この例では、フォーマッタは、1 つのメディアの種類をサポートしています&quot;テキスト/csv&quot;:。

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

上書き、**のオーバーライド**をフォーマッタの種類を示すためにメソッドがシリアル化できます。

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

この例では、フォーマッタをシリアル化できるシングル`Product`オブジェクトのコレクションおよび`Product`オブジェクト。

同様に、オーバーライド、 **CanReadType**をフォーマッタの種類を示すためにメソッドの逆シリアル化できます。 この例では、フォーマッタはサポートされない逆シリアル化、ため、メソッドは単に返します**false**します。

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

最後に、オーバーライド、 **WriteToStream**メソッド。 このメソッドでは、ストリームに書き込むことで、型をシリアル化します。 フォーマッタで逆シリアル化をサポートする場合は上書きも、 **ReadFromStream**メソッド。

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Web API パイプラインへのメディア フォーマッタの追加

メディア タイプ フォーマッタを Web API パイプラインに追加するには、使用、**フォーマッタ**プロパティを**HttpConfiguration**オブジェクト。

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>文字エン コード

必要に応じて、メディア フォーマッタは、utf-8 または ISO 8859-1 などの複数の文字エンコーディングをサポートできます。

コンス トラクターの 1 つ以上追加[System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx)型を**SupportedEncodings**コレクション。 既定の最初のエンコーディングを配置します。

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

**WriteToStream**と**ReadFromStream**メソッドを呼び出して[MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx)優先文字エン コードを選択します。 このメソッドでは、サポートされているエンコーディングのリストに照らして要求ヘッダーと一致します。 使用して、返された**エンコード**を読み取るか、ストリームからの書き込み。

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
