---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: HTML エディター コントロールの使用方法 (VB) |Microsoft Docs
author: microsoft
description: HTMLEditor は、ASP.NET AJAX コントロールを簡単に作成し、ツールバーのボタンを使用して HTML コンテンツを編集することができます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b3882f631799eb99f3c63da89097daba1fca3ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401968"
---
<a name="how-do-i-use-the-html-editor-control-vb"></a>HTML エディター コントロールの使用方法 (VB)
====================
によって[Microsoft](https://github.com/microsoft)

> HTMLEditor は、ASP.NET AJAX コントロールを簡単に作成し、ツールバーのボタンを使用して HTML コンテンツを編集することができます。


このチュートリアルの目的は、AJAX Control Toolkit に含まれている HTML エディター コントロールの概要を提供するためにです。 HTML エディターには、リンクを追加するには、テキストの配置を変更するイメージの追加、フォント サイズを変更する、フォントを選択すると、背景色を変更する、前景色の色を変更するためのオプションと切り取りを実行するには、コピー、および貼り付けの操作 (図 1 参照)。


[![HTML エディター](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**図 01**:、HTML エディター ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-html-editor-control-vb/_static/image2.png))。


HTML エディターでは、デザイン モードを使用してコンテンツを入力できます。 または、HTML を直接入力することができます。 HTML コンテンツをプレビューするオプションも提供されます (図 2 参照)。


[![デザイン、HTML、およびプレビュー ボタン](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**図 02**: デザイン、HTML、およびプレビュー ボタン ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-html-editor-control-vb/_static/image4.png))。


このチュートリアルでは、HTML エディターを表示する方法、HTML エディターで表示されるツールバーのボタンをカスタマイズする方法、およびクロス サイト スクリプティング攻撃を回避する方法について説明します。

## <a name="displaying-the-html-editor"></a>HTML エディターを表示します。

HTML エディターを使用するには、ASP.NET ページで、前に、ページに ScriptManager コントロールを追加する必要があります。 ScriptManager コントロールは、Visual Studio または Visual Web Developer Express ツールボックスの [AJAX Extensions] タブの下に配置します。

ページ上の他のコントロールの前にページの上部にある、ScriptManager コントロールを配置する必要があります。 たとえば、することができます下に配置すぐに開始サーバー側&lt;フォーム&gt;タグ。

AJAX Control Toolkit のコントロールの残りの部分を使用して、ツールボックスには、HTML エディター コントロールがあります。 エディター コントロールという名前です (図 3 を参照してください)。


[![HTML エディター コントロール](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**図 03**:、HTML エディター コード ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-html-editor-control-vb/_static/image6.png))。


HTML エディターをページにドラッグした後は、プロパティ シートでそのプロパティを設定できます。 たとえば、通常 Width および Height プロパティを設定する必要があります。 1 を一覧表示するには、HTML エディターを含む ASP.NET ページのソースが含まれています。

**1 - SimpleEditor.aspx を一覧表示します。**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

リスト 1 で、ページには、HTML エディター コントロール、Button コントロール、およびリテラル コントロールが含まれています。 リテラル コントロールの HTML エディターの内容が表示されます、ボタンをクリックすると (図 4 参照)。


[![HTML エディターでフォームを送信します。](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**図 04**: HTML エディターでフォームを送信する ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-html-editor-control-vb/_static/image8.png))。


HTML エディターのコンテンツ プロパティは、HTML エディターに入力した HTML コンテンツの検索に使用されます。 この HTML コンテンツが JavaScript を含めることができますに注意します。 次のセクションでは、JavaScript インジェクション攻撃を防止する方法について説明します。

## <a name="customizing-the-html-editor-toolbar"></a>HTML エディターのツールバーをカスタマイズします。

正確にボタンをカスタマイズすることができます、エディターに表示されます。 たとえばをユーザーが HTML エディターを HTML モードに切り替えることを防ぐために、[HTML] タブを削除する場合があります。 または、ユーザーがフォーラムに過度に大きなテキストを作成できないようにするには、フォント サイズ ドロップダウン リストを削除したい場合があります (図 5 参照) を post メッセージします。


[![カスタマイズされた HTML エディター](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**図 05**: A HTML エディターのカスタマイズ ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-html-editor-control-vb/_static/image10.png))。


ツール バー ボタンをカスタマイズするには、新しい HTML エディターのエディターの基本クラスから派生します。 たとえば、リスト 2 でのカスタム エディターには、太字と斜体のツール バー ボタンにはのみが含まれます。 その他のすべてのツール バー ボタンが削除されました。 さらに、エディターの下部にある HTML タブがなくなりました (ただし、デザインおよびプレビュー タブが残っています)。

**2 - アプリの一覧を表示する\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

アプリに、リスト 2 で、クラスを追加する必要があります\_コード フォルダーのクラスが自動的にコンパイルされるようにします。 場合、アプリ\_コード フォルダーは、web サイトに存在しませんし、フォルダーを追加することだけです。

カスタム エディターを作成した後追加できますが、ASP.NET ページに同じ方法で、通常の HTML エディター (リスト 3 参照) を追加すると。

**3 - ShowCustomEditor.aspx を一覧表示します。**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>クロス サイト スクリプティング (XSS) 攻撃の回避

ユーザーからの入力をそのまま使用し、web サイトにその入力を再表示すると、可能性のあるクロスサイト スクリプティング (XSS) 攻撃に web サイトを開きます。 理論上は、悪意のあるハッカーは、入力が再表示と実行される JavaScript コードを送信でした。 JavaScript は、ユーザーのパスワードやその他の機密情報の盗用される可能性があります。

通常、html web ページに表示する前に、ユーザーから取得する任意の入力をエンコード XSS 攻撃を倒すことができます。 ただし、HTML 出力を HTML エディターのエンコードはのみエンコードしない&lt;スクリプト&gt;タグも、すべての HTML タグをエンコードことはできます。 つまり、すべての種類のフォント、フォント サイズ、および背景色などの書式設定が失われます。

パスワード、クレジット_カード番号、社会保障番号のなどのユーザーから機密情報を収集している場合、HTML エディターでのユーザーから取得したコンテンツのエンコードされていないいない表示されます。 HTML コンテンツを再表示できませんまたは HTML コンテンツを送信していますの状況でのみ、HTML エディターを使用して、信頼されたパーティで web サイトにする必要があります。

たとえば、ブログのアプリケーションを作成することに想像してください。 このような状況で理にかなってブログの投稿を作成するときに、HTML エディターを使用します。 ブログの投稿を送信した 1 つのみと、悪意のある JavaScript の送信が自分で信頼の。 ただし、匿名ユーザーがコメントの投稿を許可する場合は、HTML エディターを使用しても意味は行いません。 ユーザーがパスワードなどの機密情報を送信する場合に特に注意が必要があります。 場合によっては、悪意のあるユーザーは、パスワードを盗むの適切な JavaScript を含むコメントを投稿する可能性があります。

## <a name="summary"></a>まとめ

このチュートリアルでは、AJAX Control Toolkit に含まれる HTML エディター コントロールの概要を簡単にいました。 HTML エディターを使用して、ユーザーからの豊富なコンテンツを受け取るし、サーバーにコンテンツを送信する方法を学習しました。 HTML エディターによって表示されるツールバー ボタンをカスタマイズする方法についても説明しました。 最後に、HTML エディターを使用して、可能性のある悪意のある入力をそのまま使用する場合、クロスサイト スクリプティング攻撃を回避する方法を学習しました。

> [!div class="step-by-step"]
> [前へ](how-do-i-use-the-html-editor-control-cs.md)
