---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: JavaScript インジェクション攻撃 (VB) |Microsoft ドキュメント
author: StephenWalther
description: JavaScript インジェクション攻撃やクロスサイト スクリプティング攻撃が発生しないようにします。 このチュートリアルでは、Stephen Walther は、する方法に簡単に de について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: cb19236b22abd455472621ce74a8cddf9752d6c5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="preventing-javascript-injection-attacks-vb"></a>JavaScript インジェクション攻撃 (VB) の防止
====================
によって[Stephen Walther](https://github.com/StephenWalther)

[PDF をダウンロードします。](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> JavaScript インジェクション攻撃やクロスサイト スクリプティング攻撃が発生しないようにします。 このチュートリアルでは、Stephen Walther は、この種の攻撃、html エンコード、コンテンツを簡単に低下する方法について説明します。


このチュートリアルの目的では、ASP.NET MVC アプリケーションの JavaScript インジェクション攻撃を防止する方法について説明します。 このチュートリアルでは、JavaScript インジェクション攻撃から web サイトを保護することに 2 つの方法について説明します。 表示するデータをエンコードすることによって JavaScript インジェクション攻撃を防止する方法を学びます。 使用すること、データをエンコードすることによって JavaScript インジェクション攻撃を防止する方法についても説明します。

## <a name="what-is-a-javascript-injection-attack"></a>JavaScript インジェクション攻撃とは何ですか。

ユーザー入力をそのまま使用し、ユーザー入力を再表示すると、JavaScript インジェクション攻撃に対して、web サイトを開きます。 JavaScript インジェクション攻撃にさらされる具体的なアプリケーションを調べてみましょう。

カスタマー フィードバックの web サイトが作成されたことを想像してください (図 1 を参照してください)。 顧客では、web サイトにアクセスでき、製品を使用しての経験に関するフィードバックを入力することができます。 お客様のフィードバックを送信すると、フィードバックはフィードバック ページに再表示されます。


[![カスタマー フィードバックの web サイト](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**図 01**: カスタマー フィードバックの web サイト ([フルサイズのイメージを表示するをクリックして](preventing-javascript-injection-attacks-vb/_static/image3.png))


カスタマー フィードバックの web サイトを使用して、`controller`リスト 1 にします。 これは、`controller`という 2 つのアクションを含む`Index()`と`Create()`です。

**1 – を一覧表示します。 `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

`Index()`メソッドが表示されます、`Index`ビュー。 このメソッドはすべてに、お客様からのフィードバックの前の`Index`(LINQ to SQL クエリを使用して) データベースからのフィードバックを取得して表示します。

`Create()`メソッドは、新しい項目のフィードバックを作成し、データベースに追加します。 顧客がフォームに入力するメッセージが渡される、`Create()`メッセージ パラメーターのメソッドです。 フィードバック項目が作成され、メッセージは、フィードバック項目に割り当てられる`Message`プロパティです。 フィードバック項目がデータベースに送信される、`DataContext.SubmitChanges()`メソッドの呼び出しです。 訪問者が最後に、リダイレクトは、`Index`ビューのすべてのフィードバックが表示されます。

`Index`ビューが一覧表示する 2 に含まれています。

**2 – を一覧表示します。 `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

`Index`ビューには 2 つのセクションです。 上部のセクションには、実際のお客様のフィードバック フォームが含まれています。 下部のセクションには、For が含まれています.すべての以前の顧客からのフィードバック項目をループし、フィードバックの各項目に対して EntryDate とメッセージ プロパティを表示する各ループします。

カスタマー フィードバックの web サイトは、単純な web サイトです。 残念ながら、web サイトは、JavaScript インジェクション攻撃にさらさです。

顧客フィードバック フォームに次のテキストを入力することを想像してください。

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

このテキストは、警告メッセージ ボックスを表示する JavaScript のスクリプトを表します。 このスクリプトを送信するフィードバックにユーザーが後のフォームのメッセージ<em>Boo!</em>すべてのユーザー、顧客フィードバック web サイトにアクセス、将来 (図 2 を参照) するたびに表示されます。


[![JavaScript インジェクション](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**図 02**: JavaScript インジェクション ([フルサイズのイメージを表示するをクリックして](preventing-javascript-injection-attacks-vb/_static/image6.png))


ここで、JavaScript インジェクション攻撃への初期応答には、apathy 可能性があります。 JavaScript インジェクション攻撃がの型だけであると思われる場合があります*改変*攻撃です。 いる誰は何でも実行できます evil 本当に JavaScript インジェクション攻撃をコミットすることによってと思われる場合があります。

残念ながら、ハッカー行えるいくつか実際には、web サイトに JavaScript を挿入し、実に悪質なものです。 JavaScript インジェクション攻撃を使用して、クロスサイト スクリプト (XSS) 攻撃を実行することができます。 クロスサイト スクリプティング攻撃では、ユーザーの機密情報を盗むし、別の web サイトに情報を送信します。

たとえば、ハッカーは、他のユーザーからブラウザーの cookie の値を盗むために JavaScript インジェクション攻撃を使用できます。 -パスワード、クレジット_カード番号、または – の社会保障番号などの機密情報がブラウザーの cookie に格納されている場合、ハッカーはこの情報を盗むために JavaScript インジェクション攻撃を使用できます。 または、ユーザーが JavaScript 攻撃では、侵害された場合、ページ内にハッカーは、挿入された JavaScript を使用してフォーム データを取得し、別の web サイトに送信し、フォーム フィールドの機密情報を入力した場合。

*心配お答えください*です。 JavaScript インジェクション攻撃を真剣にして、ユーザーの機密情報を保護します。 次の 2 つのセクションでは、ASP.NET MVC アプリケーションの JavaScript インジェクション攻撃を防御するために使用できる 2 つの方法について説明します。

## <a name="approach-1-html-encode-in-the-view"></a>アプローチ 1: ビューでの HTML エンコードします。

1 つの JavaScript インジェクション攻撃を回避するという簡単な方法を HTML には、ビュー内のデータを再表示するときに、web サイトのユーザーが入力した任意のデータをエンコードします。 更新された`Index`ビューを一覧表示する 3 でこの方法に依存します。

**3 – を一覧表示する`Index.aspx`(HTML エンコード)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

注意して、値の`feedback.Message`HTML エンコードされる値が次のコードに表示される前に。

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

新機能の平均を HTML エンコード文字列ですか? 文字列をエンコードする HTML と危険ななどの文字`<`と`>`など、HTML エンティティの参照に置き換え`&lt;`と`&gt;`です。 ときに、文字列`<script>alert("Boo!")</script>`html エンコードされる場合に変換を取得`&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`です。 エンコードされた文字列は、ブラウザーによって解釈される場合は、JavaScript スクリプトとして実行されなくなります。 代わりに、図 3 に無害なページを取得します。


[![敗北 JavaScript 攻撃](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**図 03**: 敗北 JavaScript 攻撃 ([フルサイズのイメージを表示するをクリックして](preventing-javascript-injection-attacks-vb/_static/image9.png))


注意、`Index`リスト 3 での値のみを表示します。`feedback.Message`はエンコードされます。 値`feedback.EntryDate`がエンコードされていません。 のみ、ユーザーが入力したデータをエンコードする必要があります。 EntryDate の値が生成されるため、コント ローラーで、しない必要がありますを HTML エンコードしてこの値。

## <a name="approach-2-html-encode-in-the-controller"></a>方法 2: コント ローラー内の HTML エンコードします。

HTML のできる HTML ビューで、データを表示するには、データをエンコード、代わりに、データベースへのデータを送信する直前にデータをエンコードします。 場合、この 2 番目の方法が実行される、 `controller` 4 の一覧表示します。

**4 – を一覧表示する`HomeController.cs`(HTML エンコード)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

メッセージの値が HTML エンコードされる値は内でデータベースに送信する前に、`Create()`アクション。 ビューでは、メッセージが再表示されますと、メッセージが HTML でエンコードされたメッセージに挿入された任意の JavaScript は実行されません。

通常、この 2 番目の方法でこのチュートリアルで説明した最初の方法をお勧めします。 この 2 つ目の方法を使った問題は、最終的に HTML エンコードされたデータ、データベース内です。 つまり、データベースのデータは奇妙な探し求めている文字ダーティになった。

なぜこれが正しくないか。 Web ページ以外のもので、データベースのデータを表示する必要が生じた場合は、問題があります。 たとえば、データには、Windows フォーム アプリケーションで不要になった簡単に表示できます。

## <a name="summary"></a>まとめ

このチュートリアルの目的は、JavaScript インジェクション攻撃の見込顧客のコンピューターを保護するにはでした。 このチュートリアルには、JavaScript インジェクション攻撃に対して、ASP.NET MVC アプリケーションを守るための 2 つの方法が説明されている: いずれかの HTML を作成することができます送信されたユーザーをエンコードするか、ビュー内のデータを HTML 送信されたユーザーをエンコード コント ローラー内のデータ。

> [!div class="step-by-step"]
> [前へ](authenticating-users-with-windows-authentication-vb.md)
