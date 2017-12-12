---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: "テキスト ボックス (VB) 内の特定の文字のみを許可する |Microsoft ドキュメント"
author: wenz
description: "ASP.NET の検証コントロールは、ユーザー入力の特定の文字のみが許可されることを確認できます。 ただしこのまだでも、ユーザー入力が無効です."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: b41ec1dfda5d85c625026e1f1e1ecd7e190ee3ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>テキスト ボックス (VB) 内の特定の文字のみを許可します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> ASP.NET の検証コントロールは、ユーザー入力の特定の文字のみが許可されることを確認できます。 ただしこれができないユーザーが無効な文字を入力し、フォームを送信しようとしています。


## <a name="overview"></a>概要

ASP.NET の検証コントロールは、ユーザー入力の特定の文字のみが許可されることを確認できます。 ただしこれができないユーザーが無効な文字を入力し、フォームを送信しようとしています。

## <a name="steps"></a>手順

ASP.NET AJAX コントロールのツールキットに含まれています、`FilteredTextBox`をテキスト ボックスを拡張するコントロール。 アクティブ化されると、フィールドに文字の特定のセットのみを入力することがあります。

これを行うには、まず必要がありますいつものように ASP.NET AJAX `ScriptManager` ASP.NET AJAX コントロール Toolkit で使用されるも JavaScript ライブラリが読み込まれます。

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

次に、テキスト ボックスは必要があります。

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

最後に、`FilteredTextBoxExtender`コントロールは、ユーザーが入力できる文字の制限の対処します。 まず、設定、`TargetControlID`属性を`ID`の`TextBox`コントロール。 次に、使用可能ないずれかを選択`FilterType`値。

- `Custom`既定値です。有効な文字の一覧を指定する必要があります。
- `LowercaseLetters`小文字のみ
- `Numbers`数字のみ
- `UppercaseLetters`大文字の使用のみ

場合、`Custom FilterType`を使用する、`ValidChars`プロパティが設定して型を指定することがありますの文字の一覧を提供する必要があります。 方法: テキスト ボックスにテキストを貼り付けるしようとすると、すべての無効な文字が削除されます。

マークアップをここでは、`FilteredTextBoxExtender`桁の数字のみを実行できるコントロール (も必要であったと考えられるもの`FilterType="Numbers"`)。

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

ページを起動して、JavaScript が有効になっている場合、文字を入力してくださいは機能しません。ただし、桁の数字は、ページに表示されます。 注意して、保護`FilteredTextBox`提供強固ではありません: 場合は JavaScript が有効になっている、検証を追加する方法、つまり ASP を使用する必要があるために、テキスト ボックスで、すべてのデータを入力します。NET の検証コントロール。


[![数字のみを入力することがあります。](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

数字のみを入力することがあります ([フルサイズのイメージを表示するをクリックして](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

>[!div class="step-by-step"]
[前へ](allowing-only-certain-characters-in-a-text-box-cs.md)
