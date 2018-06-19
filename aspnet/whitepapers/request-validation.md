---
uid: whitepapers/request-validation
title: 要求の検証 - スクリプト攻撃の防止 |Microsoft ドキュメント
author: rick-anderson
description: このペーパーでは、ここで、既定では、アプリケーションが回避エンコードされていない HTML コンテンツして処理から ASP.NET の要求の検証機能について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0b24fe2193d2c7a858667505bad9ed0b1d70a328
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
ms.locfileid: "28883556"
---
<a name="request-validation---preventing-script-attacks"></a>要求の検証 - スクリプト攻撃の防止
====================
> このホワイト ペーパーでは、ここで、既定では、アプリケーションが回避サーバーに送信されたエンコードされていない HTML コンテンツを処理から ASP.NET の要求の検証機能について説明します。 HTML のデータを安全に処理するアプリケーションは設計されて 場合は、この要求の検証機能を無効にすることができます。
> 
> ASP.NET 1.1 および ASP.NET 2.0 に適用されます。


要求の検証、ASP.NET のバージョン 1.1 では、以降の機能は、サーバーがコンテンツを含むエンコードされていない HTML を受け入れることを防止します。 この機能は、クライアント スクリプト コードまたは HTML 知らずにサーバーに送信、格納、し、他のユーザーに提示し、いくつかのスクリプト インジェクション攻撃を防ぐために設計されています。 すべての入力データを検証することと、適切なときに HTML エンコードをまだ強くお勧めします。

たとえば、ユーザーの電子メール アドレスと、データベースでの電子メール アドレスのストアを要求する Web ページを作成します。 If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded. ASP.NET の要求の検証機能は、問題が起こらないこれを防止します。

## <a name="why-this-feature-is-useful"></a>この機能は役立ちます理由

多くのサイトは、単純なスクリプト インジェクション攻撃に対して開いていることに対応していません。 このような攻撃の目的は、HTML を表示することによって、サイトの内容を改ざんまたは可能性のあるハッカーのサイトにユーザーをリダイレクトするクライアント スクリプトを実行するには、スクリプト インジェクション攻撃、Web 開発者必要がありますどうしは競合している問題です。

スクリプト インジェクション攻撃は、すべての web 開発者の ASP.NET、ASP、またはその他の web 開発テクノロジを使用しているかどうかです。

事前対応的に、ASP.NET 要求の検証機能は、開発者は、そのコンテンツを許可することにした場合を除き、サーバーによって処理される、エンコードされていない HTML コンテンツを許可しないことによってこれらの攻撃を防ぎます。

## <a name="what-to-expect-error-page"></a>予想される結果: エラー ページ

以下のスクリーン ショットは、いくつかのサンプルの ASP.NET コードを示しています。

![](request-validation/_static/image1.png)

これにより、コードを実行して、テキスト ボックスにいくつかのテキストを入力することができる、単純なページで、ボタンをクリックし、ラベル コントロールでテキストを表示します。

![](request-validation/_static/image2.png)

ただし、JavaScript をなどが`<script>alert("hello!")</script>`を入力して送信された例外になります。

![](request-validation/_static/image3.png)

エラー メッセージの状態を '危険性のある Request.Form 値が検出された' し、詳細は何が発生したと動作を変更する方法を正確に説明を提供します。 例:

要求の検証が危険性のあるクライアントの入力値を検出し、要求の処理は中止されました。 この値は、クロスサイト スクリプティング攻撃など、アプリケーションのセキュリティを侵害しようである可能性があります。 要求の検証を無効に設定できます`validateRequest=false`ページ ディレクティブまたは構成セクション。 ただし、アプリケーション明示的にチェックするすべての入力ケースでこのを強くお勧めします。

## <a name="disabling-request-validation-on-a-page"></a>ページの要求の検証を無効にします。

設定する必要があります ページで要求の検証を無効にする、`validateRequest`ページ ディレクティブの属性`false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> コンテンツは、ページに送信する要求の検証を無効にすると、そのコンテンツを確実にページの開発者の責任においてが正しくエンコードまたは処理をお勧めします。

## <a name="disabling-request-validation-for-your-application"></a>アプリケーションの要求の検証を無効にします。

アプリケーションの要求の検証を無効にする必要がありますを変更または、アプリケーションの Web.config ファイルを作成しのクロス属性を設定、`<pages />`セクション`false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

サーバー上のすべてのアプリケーションの要求の検証を無効にする場合は、Machine.config ファイルには、この変更を行うことができます。

> [!CAUTION]
> コンテンツがアプリケーションに送信する要求の検証を無効にすると、アプリケーション開発者がそのコンテンツを確認する必要が正しくエンコードまたは処理をお勧めします。

次のコードは、要求の検証をオフに変更されます。

![](request-validation/_static/image4.png)

ここで、次の JavaScript は、テキスト ボックスに入力された`<script>alert("hello!")</script>`結果になります。

![](request-validation/_static/image5.png)

これが発生しないように、要求の検証を無効になっていると、お必要がありますを HTML エンコード、コンテンツ。

## <a name="how-to-html-encode-content"></a>方法を HTML コンテンツをエンコード

要求の検証を無効にした場合は、将来使用するために格納されるコンテンツの HTML エンコードすることお勧めします。 HTML エンコードは自動的に置き換える '&lt;'または'&gt;' (およびその他のいくつかの記号) と、対応する HTML でエンコードされた表現です。 たとえば、'&lt;'は置き換え'&amp;lt;' と '&gt;'は置き換え'&amp;gt;'。 ブラウザーでは、これらの特別なコードを使用して、表示を '&lt;'または'&gt;' ブラウザーにします。

コンテンツを使用して、サーバーに簡単に HTML エンコードする、 `Server.HtmlEncode(string)` API です。 コンテンツすることもできます簡単に HTML デコード、つまり、標準を使用して HTML に戻す、`Server.HtmlDecode(string)`メソッドです。

![](request-validation/_static/image6.png)

その結果。

![](request-validation/_static/image7.png)
