---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'ASP.NET Web API で HTML フォームのデータを送信する: フォーム エンコード データ |Microsoft ドキュメント'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506941"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>ASP.NET Web API で HTML フォームのデータを送信する: フォーム エンコード データ
====================
によって[Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>パート 1: フォーム エンコード データ

この記事では、Web API コント ローラーへのフォーム エンコード データを投稿する方法を示します。

- [HTML フォームの概要](#overview_of_html_forms)
- [複合型を送信します。](#sending_complex_types)
- [AJAX を使用してフォームのデータを送信します。](#sending_form_data_via_ajax)
- [単純型を送信します。](#sending_simple_types)

> [!NOTE]
> [完成したプロジェクトをダウンロード](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007)です。


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>HTML フォームの概要

HTML フォームの使用は、GET か、または、サーバーにデータを送信する POST。 **メソッド**の属性、**フォーム**要素は、HTTP メソッド。

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

既定の方法は GET です。 フォームで使用する場合の取得、データがクエリ文字列としての URI でエンコードされたフォームです。 フォームは POST を使用している場合、フォーム データが要求本文に配置されます。 データの送信を**enctype**属性が要求本文の形式を指定します。

| enctype | 説明 |
| --- | --- |
| application/x-www-form-urlencoded | フォームのデータは、URI クエリ文字列のような名前と値のペアとしてエンコードされます。 これは、POST の既定の形式です。 |
| マルチパート フォーム データ | フォームのデータは、マルチパート MIME メッセージとしてエンコードされます。 サーバーにファイルをアップロードする場合は、この形式を使用します。 |

この記事のパート 1 x-www-form-urlencoded 形式を見ます。 [第 2 部](sending-html-form-data-part-2.md)マルチパート MIME について説明します。

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>複合型を送信します。

通常、複数のフォーム コントロールから取得した値から成る複合型が送信されます。 ステータスの更新を表す次のモデルを考慮してください。

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

受け付ける、Web API コント ローラーをここでは、 `Update` POST 要求を介してオブジェクト。

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> このコント ローラーを使用して[アクション ベースのルーティング](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name)ルート テンプレートは、 &quot;api/{controller}/{controller}/{id}&quot;です。 クライアントは、データを送信&quot;/api/updates/complex&quot;です。


これでユーザー状態の更新を送信するための HTML フォームを作成してみましょう。

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

注意して、**アクション**フォーム上の属性は、コント ローラー アクションの URI。 いくつかの値の入力にフォームを次に示します。

![](sending-html-form-data-part-1/_static/image1.png)

ユーザーには、送信がクリックすると、ブラウザー HTTP 要求を送信すると、次のような。

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

要求本文に名前/値のペアとして書式設定されたフォームのデータが含まれていることを確認します。 Web API がのインスタンスに名前/値ペアを自動的に変換、`Update`クラスです。

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>AJAX を使用してフォームのデータを送信します。

ユーザーは、フォームを送信するときに、ブラウザーは現在のページから移動し、応答メッセージの本文を表示します。 応答が HTML ページの場合は問題ありません。 Web API、ただし、応答本文は通常か空であるか、JSON などの構造化データが含まれています。 その場合は、方が有意義ページが応答を処理できるように、AJAX を使用して、フォーム データが要求を送信します。

次のコードでは、jQuery を使用してデータをフォームを投稿する方法を示します。

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery**送信**関数は、新しい関数にフォームのアクションを置き換えます。 これには、[送信] ボタンの既定の動作がオーバーライドされます。 **シリアル化**関数は、名前/値ペアにフォームのデータをシリアル化します。 フォームのデータをサーバーに送信する`$.post()`です。

要求が完了したら、`.success()`または`.error()`ハンドラーでは、ユーザーに適切なメッセージが表示されます。

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>単純型を送信します。

前のセクションで、Web API は、モデル クラスのインスタンスに逆シリアル化する複合型を送信します。 文字列などの単純型を送信することもできます。

> [!NOTE]
> 単純型を送信する前に、複合型で値の代わりに折り返しを検討してください。 これは、利点を得られるモデルの検証のサーバー側でと、必要な場合は、モデルを拡張するが容易です。


単純型を送信する基本的な手順は、同じ値が 2 つの微妙な違いがあります。 最初に、コント ローラーにする必要がありますを装飾するパラメーター名の**FromBody**属性。

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

既定では、Web API は、要求の URI から単純型を取得しようとします。 **FromBody**属性は Web API 要求の本文から値を読み取ることを示します。

> [!NOTE]
> Web API、応答本文最大で何度か読み取る、アクションの 1 つのパラメーターは要求の本体からのもののみです。 要求本文から複数の値を取得する必要がある場合は、複合型を定義します。


次に、クライアントは、次の形式の値を送信する必要があります。

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

具体的には、名前/値ペアの名前の部分は単純型の空にする必要があります。 すべてのブラウザーが HTML フォームは、これをサポートしますが、次のようにスクリプトでこの形式を作成します。

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

サンプル フォームを次に示します。

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

フォーム値を送信するスクリプトを次に示します。 前のスクリプトから唯一の違いに渡される引数、**投稿**関数。

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

同じアプローチを使用して、単純型の配列を送信できます。

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>その他のリソース

[パート 2: ファイルのアップロードとマルチパート MIME](sending-html-form-data-part-2.md)
