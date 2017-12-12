---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: "(C#) パスワードの強度をテスト |Microsoft ドキュメント"
author: wenz
description: "パスワードが要求されて、どこにでもほぼレイジー ユーザーから傾向を容易に実行を中断する単純なパスワードを選択できるようにします。 ASP で PasswordStrength コントロールです。N.."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: eda7baae1833b074ba34d8f10fa434df14cc592e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="testing-the-strength-of-a-password-c"></a>テスト (c#) パスワードの強度
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)

> パスワードが要求されて、どこにでもほぼレイジー ユーザーから傾向を容易に実行を中断する単純なパスワードを選択できるようにします。 ASP.NET AJAX コントロールのツールキットで PasswordStrength コントロールは、適切な方法は、パスワードが確認できます。


## <a name="overview"></a>概要

パスワードが要求されて、どこにでもほぼレイジー ユーザーから傾向を容易に実行を中断する単純なパスワードを選択できるようにします。 `PasswordStrength` ASP.NET AJAX コントロール Toolkit でコントロールを適切な方法は、パスワードが確認できます。

## <a name="steps"></a>手順

`PasswordStrength`コントロールがテキスト ボックスを拡張し、パスワードが満足できるかどうかを確認します。 さまざまな属性のオプションが提供しています次にそれらの一部を示します。

- `MinimumNumericCharacters`パスワードに必要な数字の最小数
- `MinimumSymbolCharacters`パスワードに必要な記号文字 (文字と数字ではない) の最小数
- `PreferredPasswordLength`パスワードの最小の長さ
- `RequiresUpperAndLowerCaseCharacters`かどうか、パスワードは、大文字と小文字の両方を使用する必要があります。

`StrengthIndicatorType`にテキストとして、パスワードの強度を表示する方法を説明 (値`"Text"`) または進行状況バーの一種として (値`"BarIndicator"`)。 `DisplayPosition`情報が表示される場所を構成する属性。 ASP.NET AJAX を含む、完全な例を次に示します`ScriptManager`コントロール、`PasswordStrength`コントロール、そしてもちろん、テキスト ボックス、ユーザーがパスワードを入力可能性があります。 デモについては、ここでは、後者フォーム フィールドを通常のテキスト フィールド、パスワード フィールドではありません、入力中の開発時に認識できるようにします。

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

ページを実行し、休暇から入力: 英小文字、大文字、数字および記号を入力した後、パスワードと見なされるに分割できないのみです。


[![今すぐパスワードをお勧め (非常に)](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)

(パスワード非常) に優れているようになりました ([フルサイズのイメージを表示するをクリックして](testing-the-strength-of-a-password-cs/_static/image3.png))

>[!div class="step-by-step"]
[次へ](testing-the-strength-of-a-password-vb.md)
