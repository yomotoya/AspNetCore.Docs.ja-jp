---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'ASP.NET Web API で HTML フォーム データを送信する: url エンコード フォーム データ |Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/15/2012
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2d01212cc408f8bb66fa3103464c9a1f7a1e21c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834809"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>ASP.NET Web API で HTML フォーム データを送信する: url エンコード フォーム データ
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>パート 1: url エンコード フォーム データ

この記事では、Web API コント ローラーに url エンコード フォーム データをポストする方法を示します。

- [HTML フォームの概要](#overview_of_html_forms)
- [複合型を送信します。](#sending_complex_types)
- [AJAX を使用してフォーム データを送信します。](#sending_form_data_via_ajax)
- [単純型を送信します。](#sending_simple_types)

> [!NOTE]
> [完成したプロジェクトをダウンロード](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007)します。


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>HTML フォームの概要

HTML フォームの使用か、取得、または、サーバーにデータを送信する投稿します。 **メソッド**の属性、**フォーム**要素は、HTTP メソッド。

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

既定のメソッドには GET です。 フォームで使用する場合の取得、データがクエリ文字列としての URI でエンコードされたフォーム。 フォーム POST を使用する場合は、フォーム データが要求本文に配置されます。 データの送信、 **enctype**属性が要求本文の形式を指定します。

| enctype | 説明 |
| --- | --- |
| application/x-www-form-urlencoded | フォーム データは、URI クエリ文字列のような名前と値のペアとしてエンコードされます。 これは、投稿の既定の形式です。 |
| マルチパート/フォーム データ | フォーム データは、マルチパート MIME メッセージとしてエンコードされます。 サーバーにファイルをアップロードする場合は、この形式を使用します。 |

この記事の第 1 部では、x-www-form-urlencoded 形式で検索します。 [第 2 部](sending-html-form-data-part-2.md)マルチパート MIME について説明します。

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>複合型を送信します。

通常は、複数のフォーム コントロールから取得した値から成る複合型を送信します。 ステータスの更新を表す次のモデルを検討してください。

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

受け取る Web API コント ローラーを次に示します、 `Update` POST 経由でのオブジェクト。

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> このコント ローラーを使用して[アクション ベースのルーティング](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name)でルート テンプレートがあるため、 &quot;api/{controller}/{action}/{id}&quot;します。 クライアントは、データをポスト&quot;/api/updates/complex&quot;します。


これでユーザー状態の更新を送信するための HTML フォームを作成してみましょう。

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

注意、**アクション**フォーム上の属性は、コント ローラー アクションの URI。 フォームに入力されたいくつかの値を次に示します。

![](sending-html-form-data-part-1/_static/image1.png)

ユーザーは、送信をクリックすると、ブラウザーが送信 HTTP 要求を次のような。

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

要求本文がフォーム データには、名前/値ペアとして書式設定が含まれていることを確認します。 Web API がのインスタンスに自動的に名前/値ペアを変換、`Update`クラス。

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>AJAX を使用してフォーム データを送信します。

ユーザーは、フォームを送信して、ブラウザーが現在のページから離れた応答メッセージの本文を表示します。 応答が HTML ページは、[ok] です。 Web API、ただし、応答本文は通常、いずれかを空や JSON などの構造化データが含まれています。 その場合は、ページは、応答を処理できるように、AJAX を使用してフォーム データが要求を送信する際になります。

次のコードでは、jQuery を使用してフォーム データを投稿する方法を示します。

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery**送信**関数は、新しい関数にフォームのアクションを置き換えます。 これには、[送信] ボタンの既定の動作がオーバーライドされます。 **シリアル化**関数は、名前/値ペアにフォーム データをシリアル化します。 サーバーにフォーム データを送信する呼び出し`$.post()`します。

要求が完了したら、`.success()`または`.error()`ハンドラーでは、ユーザーに適切なメッセージが表示されます。

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>単純型を送信します。

前のセクションでは、複合型は、Web API は、モデル クラスのインスタンスに逆シリアル化を送信しました。 文字列などの単純型を送信することもできます。

> [!NOTE]
> 単純型を送信する前に、代わりに、複合型で値をラッピング検討してください。 これはサーバー側では、モデルの検証のメリットを提供し、必要な場合に、モデルを拡張しやすきます。


単純型を送信する基本的な手順は同じですが 2 つの微妙な違いがあります。 最初に、コント ローラーにする必要がありますを装飾するパラメーター名の**FromBody**属性。

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

既定では、Web API は、要求 URI からの単純型の取得を試みます。 **FromBody**属性値の読み取り、要求本文から Web API に指示します。

> [!NOTE]
> Web API を読み取り、応答本文、多くても 1 回のみ、アクションの 1 つのパラメーターは、要求本文から取得できます。 要求本文から複数の値を取得する必要がある場合は、複合型を定義します。


次に、クライアントは、次の形式で値を送信する必要があります。

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

具体的には、名前/値ペアの名前の部分は単純型の空にする必要があります。 すべてのブラウザーが HTML フォームは、これをサポートしますが、次のようにスクリプトでこの形式を作成します。

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

フォームの例を次に示します。

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

フォームの値を送信するスクリプトを次に示します。 前のスクリプトからの唯一の違いに渡される引数、**投稿**関数。

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

同じアプローチを使用して、単純型の配列を送信できます。

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>その他のリソース

[パート 2: ファイルのアップロードとマルチパート MIME](sending-html-form-data-part-2.md)
