---
uid: whitepapers/request-validation
title: 要求の検証 - スクリプト攻撃の防止 |Microsoft Docs
author: rick-anderson
description: このホワイト ペーパーでは、ここで、既定では、アプリケーションはできませんがエンコードされていない HTML コンテンツして処理する ASP.NET の要求の検証機能について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: ''
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 783fb1ae27d88f9c6d6d3484d26d3e206e7f2fba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372930"
---
<a name="request-validation---preventing-script-attacks"></a>要求の検証 - スクリプト攻撃を防ぐ
====================
> このホワイト ペーパーでは、ここで、既定では、アプリケーションが回避エンコードされていない HTML コンテンツがサーバーに送信を処理する ASP.NET の要求の検証機能について説明します。 アプリケーションは HTML のデータを安全に処理するように設計されている場合は、この要求の検証機能を無効にできます。
> 
> ASP.NET 1.1 および ASP.NET 2.0 に適用されます。


要求の検証は ASP.NET のバージョン 1.1 では、以降の機能は、サーバーがコンテナーのコンテンツのエンコードされていない HTML を受け入れることを防ぎます。 この機能は、クライアント スクリプト コードや HTML 知らないうちに、サーバーに送信された、保存、およびできる他のユーザーに提示されますいくつかのスクリプト インジェクション攻撃を防ぐために設計されています。 すべての入力データを検証して、HTML エンコード時に適切なことをまだ強くお勧めします。

たとえば、ユーザーの電子メール アドレスと、ストア データベースでの電子メール アドレスを要求する Web ページを作成します。 ユーザーが入力した場合&lt;スクリプト&gt;アラート (「こんにちは from script」)&lt;/script&gt; 、有効な電子メール アドレスの代わりにそのデータが表示されたら、このスクリプトを実行する、コンテンツが正しくエンコードされていません。 ASP.NET の要求の検証機能が行われてからこれを防止します。

## <a name="why-this-feature-is-useful"></a>この機能が便利な理由

多くのサイトは、単純なスクリプト インジェクション攻撃に対して開いていることに対応していません。 これらの攻撃の目的は、HTML を表示することで、サイトの内容を改ざんしたり、可能性のあるハッカーのサイトにユーザーをリダイレクトするクライアント スクリプトを実行するには、スクリプト インジェクション攻撃、Web 開発者に取り組む必要がある問題です。

スクリプト インジェクション攻撃は、ASP.NET、ASP、またはその他の web 開発テクノロジを使用しているかどうか、すべての web 開発者の問題です。

ASP.NET 要求の検証機能は、事前にそのコンテンツを許可する、開発者が決定しない限り、サーバーによって処理されるエンコードされていない HTML コンテンツを許可しないことによってこれらの攻撃を防ぎます。

## <a name="what-to-expect-error-page"></a>予想される結果: エラー ページ

以下のスクリーン ショットは、いくつかのサンプル ASP.NET コードを示しています。

![](request-validation/_static/image1.png)

このコードの結果を実行して、テキスト ボックスにテキストを入力できるようにする単純なページで、ボタンをクリックし、ラベル コントロールでテキストを表示します。

![](request-validation/_static/image2.png)

ただし、JavaScript をなどが`<script>alert("hello!")</script>`入力し、例外が発生しましたが送信します。

![](request-validation/_static/image3.png)

エラー メッセージの状態を '可能性のある危険な Request.Form 値が検出された' し、詳細は何が発生したと動作を変更する方法を正確に説明を提供します。 例えば:

要求の検証には、危険性のあるクライアントの入力値が検出され、要求の処理は中止されました。 この値は、クロスサイト スクリプティング攻撃などのアプリケーションのセキュリティを侵害する試行を示す可能性があります。 要求の検証を無効に設定できます`validateRequest=false`Page ディレクティブまたは構成セクション。 ただし、アプリケーションに明示的にチェックするすべての入力ケースでこのを強くお勧めします。

## <a name="disabling-request-validation-on-a-page"></a>ページの要求の検証を無効にします。

設定する必要があります ページの要求の検証を無効にする、`validateRequest`する Page ディレクティブの属性`false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> ページにコンテンツを送信できる要求の検証を無効にすると、そのコンテンツを確認するページの開発者の責任が正しくエンコードまたは処理をお勧めします。

## <a name="disabling-request-validation-for-your-application"></a>アプリケーションの要求の検証を無効にします。

アプリケーションの要求の検証を無効にする必要があります変更またはアプリケーションの Web.config ファイルを作成しての validateRequest 属性を設定、`<pages />`セクション`false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

サーバー上のすべてのアプリケーションの要求の検証を無効にする場合は、Machine.config ファイルには、この変更を行うことができます。

> [!CAUTION]
> コンテンツをアプリケーションに送信できる要求の検証を無効にすると、そのコンテンツを確実にアプリケーション開発者の責任が正しくエンコードまたは処理をお勧めします。

次のコードは、要求の検証を無効に変更されます。

![](request-validation/_static/image4.png)

次の JavaScript は、テキスト ボックスに入力した場合は、今すぐ`<script>alert("hello!")</script>`結果になります。

![](request-validation/_static/image5.png)

これをになっている要求の検証を防ぐには、html コンテンツでエンコードします必要があります。

## <a name="how-to-html-encode-content"></a>Html コンテンツをエンコード方法

要求の検証を無効にした場合は、将来使用するために格納されるコンテンツの HTML エンコードすることをお勧めします。 HTML エンコーディングが自動的に置き換え、'&lt;'または'&gt;' (とその他のいくつかの記号) で、対応する HTML でエンコードされた表現。 たとえば、'&lt;'に置換されます'&amp;lt;' と '&gt;'に置換されます'&amp;gt;'。 ブラウザーでは、これらの特殊なコードを使用して、表示を '&lt;'または'&gt;' ブラウザーにします。

コンテンツを使用してサーバーに簡単に HTML エンコードすることができます、 `Server.HtmlEncode(string)` API。 コンテンツできますも簡単に HTML でデコードされては、標準の HTML を使用してに戻す、`Server.HtmlDecode(string)`メソッド。

![](request-validation/_static/image6.png)

その結果。

![](request-validation/_static/image7.png)
