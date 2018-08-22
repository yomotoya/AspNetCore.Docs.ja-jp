---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: コンテンツ ネゴシエーションを ASP.NET Web API の |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API で HTTP コンテンツ ネゴシエーションの実装方法について説明します。
ms.author: riande
ms.date: 05/20/2012
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: e936bdfa52f786ec86d3e84eac3cd644225b6f92
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827316"
---
<a name="content-negotiation-in-aspnet-web-api"></a>ASP.NET Web API でコンテンツ ネゴシエーション
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

この記事では、ASP.NET Web API でコンテンツ ネゴシエーションの実装方法について説明します。

HTTP の仕様 (RFC 2616) は、「複数の表現が使用できる場合に、指定された応答の最適な表現を選択した場合のプロセスです。」とコンテンツ ネゴシエーションを定義します。 Http コンテンツ ネゴシエーションを主要なメカニズムは、これらの要求ヘッダーです。

- **受信:** するメディアの種類が"application/json に"など、応答として受け入れ可能な"application/xml、"またはカスタムのメディアの種類など、 &quot;application/vnd.example+xml&quot;
- **受け入れる-文字セット:** 文字セットが utf-8 または ISO 8859-1 など、許容されます。
- **Accept-encoding:** コンテンツのエンコード方式が受け入れ可能で gzip など。
- **Accept Language:** 、使用する自然言語など"英語-米国"。

サーバーは、HTTP 要求の他の部分でも確認できます。 たとえば、要求に X-要求-でヘッダーが含まれている場合、AJAX 要求を示すサーバー可能性があります既定値を JSON に Accept ヘッダーがない場合。

この記事では、Web API で Accept、Accept-charset ヘッダーを使用する方法を紹介します。 (この時点ではありません Accept-encoding、Accept-language の組み込みサポート。)

## <a name="serialization"></a>シリアル化

Web API コント ローラーには、CLR 型としてのリソースが返された場合、パイプラインは、戻り値をシリアル化し、HTTP 応答の本文に書き込みます。

たとえば、次のコント ローラー アクションがあるとします。

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

クライアントでは、この HTTP 要求を送信可能性があります。

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

応答では、サーバーが送信可能性があります。

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

JSON、Javascript、またはその「何も」この例では、クライアントが要求した (\*/\*)。 サーバー応答の JSON 表現の`Product`オブジェクト。 通知に応答の Content-type ヘッダーが設定されている&quot;、application/json&quot;します。

コント ローラーを返すことも、 **HttpResponseMessage**オブジェクト。 応答本文の CLR オブジェクトを指定するには、呼び出し、 **CreateResponse**拡張メソッド。

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

このオプションでは、応答の詳細についてより詳細に制御をできます。 ステータス コードを設定し、HTTP のヘッダーを追加するなどできます。

リソースをシリアル化するオブジェクトが呼び出されると、*メディア フォーマッタ*します。 派生してメディア フォーマッタ、 **MediaTypeFormatter**クラス。 Web API は、XML および JSON 用メディア フォーマッタを提供し、その他のメディアの種類をサポートするために、カスタム フォーマッタを作成することができます。 カスタム フォーマッタを作成する方法の詳細については、次を参照してください。[メディア フォーマッタ](media-formatters.md)します。

## <a name="how-content-negotiation-works"></a>コンテンツ ネゴシエーションのしくみ

最初に、パイプラインの取得、 **IContentNegotiator**からサービス、 **HttpConfiguration**オブジェクト。 メディア フォーマッタの一覧を取得また、 **HttpConfiguration.Formatters**コレクション。

次に、パイプラインを呼び出す**IContentNegotiatior.Negotiate**で渡し。

- シリアル化するオブジェクトの種類
- メディア フォーマッタのコレクション
- HTTP 要求

**ネゴシエート**メソッドは 2 つの情報を返します。

- どのフォーマッタを使用するには
- 応答のメディアの種類

フォーマッタが見つからない場合、**ネゴシエート**メソッドを返します。 **null**、およびクライアント渡さ HTTP エラー 406 (Not Acceptable)。

次のコードは、どのコント ローラーを直接呼び出すできますコンテンツ ネゴシエーションを示しています。

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

このコードは、パイプラインを自動的には、何をします。

## <a name="default-content-negotiator"></a>既定のコンテンツ ネゴシエーター

**DefaultContentNegotiator**クラスの既定の実装を提供する**IContentNegotiator**します。 いくつかの条件を使用して、フォーマッタを選択します。

最初に、フォーマッタは、型をシリアル化できる必要があります。 これは、呼び出すことによって確認が**MediaTypeFormatter.CanWriteType**します。

次に、コンテンツ ネゴシエーターを使用して、各フォーマッタを調べ、HTTP 要求が一致する度合いを評価します。 一致を評価するには、コンテンツ ネゴシエーターを次に 2 つのことで、フォーマッタで

- **SupportedMediaTypes**コレクションで、サポートされているメディアの種類の一覧が含まれています。 コンテンツ ネゴシエーターは、要求の Accept ヘッダーに対しては、この一覧の一致を試みます。 Accept ヘッダーが範囲を含めることができますに注意してください。 たとえば、"text/plain"にはテキストと一致する/\*または\* /\*します。
- **MediaTypeMappings**の一覧を含むコレクションを**MediaTypeMapping**オブジェクト。 **MediaTypeMapping**クラスには、メディアの種類が HTTP 要求を一致するように、一般的な方法が用意されています。 たとえば、カスタム HTTP ヘッダーを特定のメディアの種類にマップことでした。

複数ある場合は一致すると、最高の品質ファクター wins と一致します。 例えば:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

この例では、application/json は、application/xml が優先されるために暗黙の品質ファクター 1.0 が。

一致が見つからない場合、コンテンツ ネゴシエーターは存在する場合、要求本文のメディアの種類に一致するように試みます。 たとえば、要求に JSON データが含まれている場合、コンテンツ ネゴシエーターは、JSON フォーマッタを探します。

引き続き、一致がない、コンテンツ ネゴシエーターは単に型をシリアル化できる最初のフォーマッタを取得します。

## <a name="selecting-a-character-encoding"></a>文字エンコーディングを選択します。

コンテンツ ネゴシエーターが調べることで最適な文字エンコーディングを選択、フォーマッタを選択した後、 **SupportedEncodings**プロパティをフォーマッタおよび (存在する場合)、要求の Accept-charset ヘッダーに対して照合します。
