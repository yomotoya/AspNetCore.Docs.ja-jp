---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: "コンテンツの ASP.NET Web API でネゴシエーション |Microsoft ドキュメント"
author: MikeWasson
description: "ASP.NET Web API HTTP コンテンツ ネゴシエーションを実装する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="content-negotiation-in-aspnet-web-api"></a>ASP.NET Web API でコンテンツ ネゴシエーション
====================
によって[Mike Wasson](https://github.com/MikeWasson)

この記事では、ASP.NET Web API がコンテンツ ネゴシエーションを実装する方法について説明します。

HTTP の仕様 (RFC 2616)「使用可能な複数の表現が存在する場合は、指定された応答の最適な形式を選択した場合のプロセスです」としてコンテンツ ネゴシエーションを定義します。 HTTP でコンテンツ ネゴシエーションを主要なメカニズムは、これらの要求ヘッダーです。

- **受信:**どのメディアの種類は"application/json に"など、応答として受け入れ可能な"application/xml、"またはカスタムのメディアの種類など、 &quot;application/vnd.example+xml&quot;
- **受け入れる Charset:**どの文字セットは許容できますが、utf-8 または ISO 8859-1 などです。
- **Accept-encoding:**コンテンツのエンコード方式は gzip など、許容されます。
- **ブラウザーの言語:**優先される自然言語など"en-us"です。

サーバーは、HTTP 要求の他の部分にも確認できます。 たとえば、要求に X-要求-とヘッダーが含まれている場合、AJAX 要求を示すサーバーが既定値を JSON に Accept ヘッダーが存在しない場合。

この記事では、Web API が Accept、Accept-charset ヘッダーを使用する方法を紹介します。 (この時点ではありません Accept-encoding、Accept-language の組み込みのサポート。)

## <a name="serialization"></a>シリアル化

Web API コント ローラーには、CLR 型として、リソースが返された場合、パイプラインは、戻り値をシリアル化し、HTTP 応答の本文に書き込みます。

たとえば、次のコント ローラーのアクションがあるとします。

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

クライアントは、この HTTP 要求を送信可能性があります。

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

応答として、サーバーが送信可能性があります。

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

この例で、クライアント要求、JSON、Javascript、または「すべて」(\*/\*)。 応答の JSON 表現のサーバー、`Product`オブジェクト。 応答の Content-type ヘッダーが設定されていることを確認&quot;アプリケーション/json&quot;です。

コント ローラーを返すことも、 **HttpResponseMessage**オブジェクト。 応答本文の CLR オブジェクトを指定するには、呼び出し、 **CreateResponse**拡張メソッド。

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

このオプションでは、応答の詳細についてより詳細に制御をできます。 HTTP ヘッダーを追加、状態コードを設定するなどです。

リソースをシリアル化するオブジェクトが呼び出されると、*メディア フォーマッタ*です。 メディア フォーマッタから派生して、 **MediaTypeFormatter**クラスです。 Web API は、XML および JSON 用メディア フォーマッタを提供し、その他のメディアの種類をサポートするためにカスタム フォーマッタを作成することができます。 カスタム フォーマッタを記述する方法については、次を参照してください。[メディア フォーマッタ](media-formatters.md)です。

## <a name="how-content-negotiation-works"></a>コンテンツ ネゴシエーションのしくみ

最初に、パイプラインの取得、 **IContentNegotiator**からサービス、 **HttpConfiguration**オブジェクト。 メディアのフォーマッタの一覧も取得、 **HttpConfiguration.Formatters**コレクション。

次に、パイプラインを呼び出す**IContentNegotiatior.Negotiate**を渡して、します。

- シリアル化するオブジェクトの種類
- メディア フォーマッタのコレクション
- HTTP 要求

**Negotiate**メソッドは 2 つの情報を返します。

- どのフォーマッタを使用するには
- 応答のメディアの種類

フォーマッタが見つからなかった場合、 **Negotiate**メソッドを返します。 **null**、およびクライアント渡さ HTTP エラー 406 (受容不可)。

次のコードは、コント ローラーを直接呼び出す方法コンテンツ ネゴシエーションを示しています。

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

このコードは、パイプラインを自動的には、何をします。

## <a name="default-content-negotiator"></a>既定のコンテンツ ネゴシエーター

**DefaultContentNegotiator**クラスの既定の実装を提供する**IContentNegotiator**です。 いくつかの条件を使用したフォーマッタを選択します。

最初に、フォーマッタは、型をシリアル化できる必要があります。 これは呼び出すことによって検証**MediaTypeFormatter.CanWriteType**です。

次に、コンテンツ ネゴシエーターを使用して、各フォーマッタを調べ、どのようにに適合する HTTP 要求を評価します。 一致を評価するには、コンテンツ ネゴシエーター 2 つの点を調べ、フォーマッタ。

- **SupportedMediaTypes**コレクションで、サポートされているメディアの種類の一覧が含まれています。 コンテンツ ネゴシエーターでは、この一覧に対して、要求の Accept ヘッダーを照合します。 Accept ヘッダーが範囲を含めることができますに注意してください。 たとえば、テキストと一致するは「テキスト/プレーン」/\*または\* /\*です。
- **MediaTypeMappings**のリストを含むコレクション**MediaTypeMapping**オブジェクト。 **MediaTypeMapping**クラスには、メディアの種類が HTTP 要求を一致するように汎用的な方法が用意されています。 たとえば、カスタム HTTP ヘッダーを特定のメディアの種類にマップことでした。

複数ある場合と一致する、最高品質係数 wins と一致します。 例:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

この例では、アプリケーション/json に暗黙的な品質ファクター 1.0 がアプリケーションまたは xml 上で優先されるようにします。

一致が見つからない場合、コンテンツ ネゴシエーターは、存在する場合、要求本文のメディアの種類で一致を試みます。 たとえば、要求に JSON データが含まれている場合、コンテンツ ネゴシエーターを JSON フォーマッタを探します。

一致が見つからない、コンテンツ ネゴシエーターは単に型をシリアル化できる最初のフォーマッタを取得します。

## <a name="selecting-a-character-encoding"></a>文字エンコーディングを選択します。

フォーマッタが選択されているコンテンツ ネゴシエーターが選択、最適な文字エン コードを見て、 **SupportedEncodings**プロパティをフォーマッタおよび (存在する場合)、要求で、Accept-charset ヘッダーと一致することです。
