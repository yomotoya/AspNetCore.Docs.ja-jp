---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
title: JavaScript インジェクション攻撃を防ぐ (c#) |Microsoft Docs
author: StephenWalther
description: JavaScript インジェクション攻撃およびクロス サイト スクリプティング攻撃が発生しないようにします。 このチュートリアルでは、Stephen Walther は、方法を簡単に de について説明しています.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: d0136da6-81a4-4815-b002-baa84744c09e
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
msc.type: authoredcontent
ms.openlocfilehash: 77d0f0346e9eff756cd74c64c310918f3c367ab1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826896"
---
<a name="preventing-javascript-injection-attacks-c"></a>JavaScript インジェクション攻撃を防ぐ (c#)
====================
によって[Stephen Walther](https://github.com/StephenWalther)

[PDF のダウンロード](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_CS.pdf)

> JavaScript インジェクション攻撃およびクロス サイト スクリプティング攻撃が発生しないようにします。 このチュートリアルでは、Stephen Walther は、これらの種類の HTML コンテンツをエンコードして攻撃を簡単に倒す方法について説明します。


このチュートリアルの目的では、ASP.NET MVC アプリケーションで JavaScript インジェクション攻撃を防止する方法について説明します。 このチュートリアルでは、web サイト、JavaScript インジェクション攻撃を防御する 2 つの方法について説明します。 表示データをエンコードすることによって JavaScript インジェクション攻撃を回避する方法について説明します。 そのまま使用するデータをエンコードすることによって JavaScript インジェクション攻撃を回避する方法を説明します。

## <a name="what-is-a-javascript-injection-attack"></a>JavaScript インジェクション攻撃を受けるとは何ですか。

ユーザー入力をそのまま使用し、ユーザー入力を再表示すると、JavaScript インジェクション攻撃、web サイトを開きます。 JavaScript インジェクション攻撃にさらされる具体的なアプリケーションを調べてみましょう。

顧客フィードバック web サイトを作成することを想像してください (図 1 参照)。 Web サイトにアクセスし、製品を使用した経験のフィードバックを入力できます。 顧客は、ユーザーのフィードバックを送信するときに、フィードバックはフィードバック ページに再表示されます。


[![カスタマー フィードバックの web サイト](preventing-javascript-injection-attacks-cs/_static/image2.png)](preventing-javascript-injection-attacks-cs/_static/image1.png)

**図 01**: カスタマー フィードバックの web サイト ([フルサイズの画像を表示する をクリックします](preventing-javascript-injection-attacks-cs/_static/image3.png))。


顧客フィードバック web サイトを使用して、`controller`リスト 1 でします。 これは、`controller`という名前の 2 つのアクションを含む`Index()`と`Create()`します。

**1 – を一覧表示します。 `HomeController.cs`**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample1.cs)]

`Index()`メソッドが表示されます、`Index`ビュー。 このメソッドは以前、お客様のフィードバックにはすべて、 `Index` (LINQ to SQL クエリを使用して) データベースからのフィードバックを取得して表示します。

`Create()`メソッドは、新しいフィードバック項目を作成し、それをデータベースに追加します。 渡される、顧客がフォームに入力したメッセージ、`Create()`メッセージ パラメーターのメソッド。 フィードバック項目が作成され、メッセージは、フィードバック項目に割り当てられる`Message`プロパティ。 フィードバック項目が使用してデータベースに送信される、`DataContext.SubmitChanges()`メソッドの呼び出し。 最後に、訪問者にリダイレクトされる、`Index`ビューのすべてのフィードバックが表示されます。

`Index`リスト 2 でビューが含まれています。

**2 – を一覧表示します。 `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample2.aspx)]

`Index`ビューでは 2 つのセクション。 最上部のセクションには、実際の顧客のフィードバック フォームが含まれています。 下部のセクションには、For が含まれています.ループをすべての以前の顧客フィードバック項目をループし、各フィードバック項目の EntryDate とメッセージのプロパティが表示されます。

顧客フィードバック web サイトは、単純な web サイトです。 残念ながら、web サイトは JavaScript インジェクション攻撃を開いています。

お客様のフィードバック フォームに次のテキストを入力することを想像してください。

[!code-html[Main](preventing-javascript-injection-attacks-cs/samples/sample3.html)]

このテキストは、警告メッセージ ボックスを表示する JavaScript スクリプトを表します。 このスクリプトを送信するフィードバックにだれかが後のフォームのメッセージ<em>Boo!</em>すべてのユーザー アクセス、カスタマー フィードバックの web サイト、将来 (図 2 参照) されるたびに表示されます。


[![JavaScript インジェクション](preventing-javascript-injection-attacks-cs/_static/image5.png)](preventing-javascript-injection-attacks-cs/_static/image4.png)

**図 02**: JavaScript インジェクション ([フルサイズの画像を表示する をクリックします](preventing-javascript-injection-attacks-cs/_static/image6.png))。


ここで、JavaScript インジェクション攻撃への初期応答には、無関心可能性があります。 JavaScript インジェクション攻撃がの型だけであると思うかもしれません*ねらい*攻撃です。 JavaScript インジェクション攻撃を受けるをコミットすることによってはだれ何か本当に有害なことができますと思われる場合があります。

残念ながら、ハッカーはいくつか実行実際には、web サイトに JavaScript を挿入し、実に悪質なものです。 JavaScript インジェクション攻撃を受けるを使用して、クロスサイト スクリプティング (XSS) 攻撃を実行することができます。 クロスサイト スクリプティング攻撃では、ユーザーの機密情報を盗むし、別の web サイトに情報を送信します。

たとえば、ハッカーは、他のユーザーからのブラウザーの cookie の値を盗み出す JavaScript インジェクション攻撃を受けるを使用できます。 -パスワード、クレジット_カード番号、または – の社会保障番号などの機密情報がブラウザーの cookie に格納されている場合、ハッカーはこの情報の盗用、JavaScript インジェクション攻撃を使用できます。 または、ユーザーが JavaScript 攻撃では、侵害された場合のページに含まれる、ハッカーは、挿入された JavaScript を使用して、フォームのデータを取得し、別の web サイトに送信し、フォーム フィールドに機密情報を入力した場合。

*怖くてお答えください*します。 JavaScript インジェクション攻撃を真剣に受け止めてし、ユーザーの機密情報を保護します。 次の 2 つのセクションでは、JavaScript インジェクション攻撃から ASP.NET MVC アプリケーションを保護するために使用できる 2 つの手法について説明します。

## <a name="approach-1-html-encode-in-the-view"></a>方法 1: ビュー内の HTML エンコードします。

JavaScript インジェクション攻撃を防ぐの簡単な方法では HTML を 1 つは、ビュー内のデータを再表示するときに、web サイトのユーザーが入力した任意のデータをエンコードします。 更新された`Index`リスト 3 でビューがこの方法に従います。

**リスト 3. – `Index.aspx` (HTML でエンコードされた)**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample4.aspx)]

注意の値`feedback.Message`html エンコード前に、次のコードで、値が表示されます。

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample5.aspx)]

これは平均値を HTML 文字列をエンコードするでしょうか。 文字列をエンコードする HTML と危険な文字など`<`と`>`など HTML エンティティ参照に置き換え`&lt;`と`&gt;`します。 したがって、文字列`<script>alert("Boo!")</script>`html エンコードされる場合に変換を取得`&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`します。 エンコードされた文字列は、ブラウザーによって解釈される場合は、JavaScript スクリプトとして実行されなくなります。 代わりに、図 3 害のないページを取得します。


[![JavaScript の敗北攻撃](preventing-javascript-injection-attacks-cs/_static/image8.png)](preventing-javascript-injection-attacks-cs/_static/image7.png)

**図 03**: 敗北 JavaScript 攻撃 ([フルサイズの画像を表示する をクリックします](preventing-javascript-injection-attacks-cs/_static/image9.png))。


インシデントを`Index`リスト 3 での値のみを表示します。`feedback.Message`はエンコードされます。 値`feedback.EntryDate`がエンコードされていません。 のみ、ユーザーによって入力されたデータをエンコードする必要があります。 EntryDate の値が、コント ローラーで生成されたためする必要がありますを HTML をエンコードしません。 この値。

## <a name="approach-2-html-encode-in-the-controller"></a>方法 2: コント ローラー内の HTML エンコードします。

HTML のできる HTML ビューでデータを表示すると、データをエンコードではなく、データベースにデータを送信する直前にデータをエンコードします。 この 2 つ目のアプローチがの場合に行われています、`controller`リスト 4。

**リスト 4 – `HomeController.cs` (HTML でエンコードされた)**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample6.cs)]

メッセージの値が HTML エンコード値がデータベースに送信される前に、`Create()`アクション。 ビューでは、メッセージが再表示されますと、メッセージが HTML エンコードされたメッセージに挿入された任意の JavaScript は実行されません。

通常、この 2 つ目の方法でこのチュートリアルで説明した最初のアプローチのどちらを優先する必要があります。 この 2 つ目のアプローチの問題は、最終的に HTML エンコードされたデータ、データベース内です。 つまり、データベースのデータには、奇妙な探し求めている文字ダーティになった。

なぜこれが正しくないでしょうか。 Web ページ以外の方法で、データベースのデータを表示する必要がある場合は、問題があります。 たとえば、Windows フォーム アプリケーションで、データを表示することができます簡単になります。

## <a name="summary"></a>まとめ

このチュートリアルの目的は、JavaScript インジェクション攻撃の取引関係に関する恐ろしいでした。 このチュートリアルには、JavaScript インジェクション攻撃に対して、ASP.NET MVC アプリケーションを守るための 2 つの方法が説明されている: いずれかの HTML を実行できます送信されたユーザーをエンコードするか、ビュー内のデータは HTML ユーザー送信のエンコード、コント ローラー内のデータ。

> [!div class="step-by-step"]
> [前へ](authenticating-users-with-windows-authentication-cs.md)
> [次へ](authenticating-users-with-forms-authentication-vb.md)
