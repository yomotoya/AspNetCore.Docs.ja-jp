---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: HTML エディター コントロールの使用方法 (VB) |Microsoft ドキュメント
author: microsoft
description: HTMLEditor は、ASP.NET AJAX コントロールを簡単に作成し、ツールバーのボタンを使用して HTML コンテンツを編集することができます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 4833949a54fa9ae12eaf7b596a5fe1ddfd1f7b7a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873301"
---
<a name="how-do-i-use-the-html-editor-control-vb"></a>HTML エディター コントロールの使用方法 (VB)
====================
によって[Microsoft](https://github.com/microsoft)

> HTMLEditor は、ASP.NET AJAX コントロールを簡単に作成し、ツールバーのボタンを使用して HTML コンテンツを編集することができます。


このチュートリアルの目的は、AJAX コントロールのツールキットに含まれている HTML エディター コントロールの概要が提供するためです。 HTML エディターには、イメージを追加するテキストの配置を変更する、リンクを追加するフォント サイズを変更する、フォントを選択すると、背景色を変更する、前景の色を変更するためのオプションが含まれています、切り取りを実行するには、コピー、貼り付けの操作 (図 1 を参照してください)。


[![HTML エディター](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**図 01**:、HTML エディター ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-html-editor-control-vb/_static/image2.png))


HTML エディターでは、デザイン モードを使用してコンテンツを入力できます。 または、HTML を直接入力することができます。 HTML コンテンツをプレビューするためのオプションも提供されます (図 2 を参照してください)。


[![デザイン、HTML、およびプレビュー ボタン](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**図 02**: ボタンのデザイン、HTML、およびプレビュー ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-html-editor-control-vb/_static/image4.png))


このチュートリアルでは、HTML エディターを表示する方法、HTML エディターで、表示されるツールバーのボタンをカスタマイズする方法、およびクロス サイト スクリプト攻撃を回避する方法を学習します。

## <a name="displaying-the-html-editor"></a>HTML エディターを表示します。

HTML エディターを使用するには、ASP.NET ページで、前に、ページに ScriptManager コントロールを追加する必要があります。 ScriptManager コントロールは、Visual Studio または Visual Web Developer Express ツールボックスの [AJAX Extensions] タブの下にあります。

ページの他のコントロールの前にページの上部にある ScriptManager コントロールを配置する必要があります。 などに配置してすぐに始めサーバー側の下&lt;フォーム&gt;タグ。

HTML エディター コントロールは、他の AJAX コントロール Toolkit コントロールのツールボックスに配置されます。 エディター コントロールという名前です (図 3 を参照してください)。


[![HTML エディター コントロール](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**図 03**:、HTML エディター コントロール ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-html-editor-control-vb/_static/image6.png))


HTML エディターをページにドラッグした後は、プロパティ シートでそのプロパティを設定できます。 たとえば、通常する幅と高さのプロパティを設定します。 1 を一覧表示するには、HTML エディターを含む ASP.NET ページのソースが含まれます。

**1 - SimpleEditor.aspx を一覧表示します。**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

1 のリスト内のページには、HTML エディター コントロール、ボタン コントロール、およびリテラルのコントロールが含まれています。 Literal コントロールに HTML エディターの内容が表示されるボタンをクリックすると (図 4 を参照してください)。


[![HTML エディターでフォームを送信します。](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**図 04**: HTML エディターでフォームを送信する ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-html-editor-control-vb/_static/image8.png))


HTML エディターの Content プロパティを使用して、HTML エディターに入力された HTML コンテンツを取得します。 この HTML コンテンツが JavaScript を含めることができますに注意してください。 次のセクションでは、JavaScript インジェクション攻撃を防止する方法について説明します。

## <a name="customizing-the-html-editor-toolbar"></a>HTML エディター ツールバーをカスタマイズします。

正確にボタンをカスタマイズすることができます、エディターに表示されます。 たとえば、ユーザーが HTML モードに HTML エディターを切り替えることを防止する [HTML] タブを削除する可能性があります。 または、ユーザーがフォーラムに過度に大きなテキストを作成することを防止する、フォント サイズのドロップダウン リストを削除する場合があります (図 5 を参照してください) を post メッセージします。


[![カスタマイズされた HTML エディター](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**図 05**: A HTML エディターのカスタマイズ ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-html-editor-control-vb/_static/image10.png))


ツールバーのボタンをカスタマイズするには、新しい HTML エディターのエディターの基本クラスから派生します。 たとえば、カスタム エディター一覧表示する 2 にはでは、太字や斜体のツール バー ボタンにはのみが含まれます。 その他のすべてのツール バー ボタンが削除されました。 さらに、[HTML] タブは、エディターの下部から削除されました (ただし、デザインおよびプレビュー タブは引き続き行わ)。

**2 - アプリを一覧表示する\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

アプリを一覧表示する 2 クラスに追加する必要があります\_コード フォルダーのクラスを自動的にコンパイルできるようにします。 場合、アプリ\_コード フォルダーは、web サイトに存在しませんし、単に、フォルダーを追加することができます。

カスタム エディターを作成した後することができますに追加 ASP.NET ページと同様に、標準の HTML エディター (3 のリストを参照) を追加するとします。

**3 - ShowCustomEditor.aspx を一覧表示します。**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>クロス サイト スクリプト (XSS) 攻撃の回避

ユーザーからの入力を受け付ける、web サイトでその入力を再表示して、ときに可能性のあるクロスサイト スクリプト (XSS) 攻撃に web サイトを開きます。 理論上は、悪意のあるハッカーは、入力が再表示されるときに実行される JavaScript コードを送信でした。 JavaScript は、ユーザーのパスワードや他の機密情報を盗むために使用できます。

通常は、HTML web ページに表示する前に、ユーザーから取得する任意の入力をエンコーディング XSS 攻撃が損なわためことができます。 ただし、HTML、HTML エディターの出力のエンコードはのみエンコードされない&lt;スクリプト&gt;タグも、すべての HTML タグをエンコードことができます。 つまり、すべての種類のフォント、フォント サイズ、および背景色などの書式設定が失われるとします。

パスワード、クレジット_カード番号や社会保障番号 - などのユーザーの機密情報を収集している場合は、HTML エディターを使用して、ユーザーから取得されたエンコードされていないコンテンツをいない表示しています。 HTML コンテンツを再表示されませんまたは HTML コンテンツが送信される場合にのみ、HTML エディターを使用すると、信頼されたパーティから web サイトにする必要があります。

たとえば、ブログ アプリケーションを作成している想像してみてください。 このような状況は、ブログの投稿を作成するときに、HTML エディターを使用します。 ブログの投稿を送信した 1 つだけと、悪意のある JavaScript の送信が自分自身を信頼する多くの場合、します。 ただし、コメントを投稿する匿名ユーザーを許可する場合に、HTML エディターを使用する意味は行いません。 ユーザーがパスワードなどの機密情報を送信する場合に特に注意する必要があります。 可能性のある、悪意のあるユーザーはパスワードを盗むの右側の JavaScript を含むコメントを投稿する可能性があります。

## <a name="summary"></a>まとめ

このチュートリアルでは、AJAX コントロールのツールキットに含まれている HTML エディター コントロールの概要で提供されました。 HTML エディターを使用して、ユーザーからの豊富なコンテンツを受け取るし、サーバーにコンテンツを送信する方法について学習しました。 HTML エディターによって表示されるツールバーのボタンをカスタマイズする方法についても説明しました。 最後に、悪意のある入力を受け付ける HTML エディターを使用する場合、クロスサイト スクリプティング攻撃を回避する方法を学習します。

> [!div class="step-by-step"]
> [前へ](how-do-i-use-the-html-editor-control-cs.md)
