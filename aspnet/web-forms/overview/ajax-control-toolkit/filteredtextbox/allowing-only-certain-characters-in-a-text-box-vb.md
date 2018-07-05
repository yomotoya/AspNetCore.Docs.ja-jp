---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: テキスト ボックス (VB) で特定の文字のみを許可する |Microsoft Docs
author: wenz
description: ASP.NET 検証コントロールは、ユーザー入力で特定の文字だけが許可されることを保証できます。 ただしこれができない無効な入力からユーザー.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: e44b69a4f7d46f1f1278f7de07a2e6c025f5c316
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386761"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>テキスト ボックス (VB) で特定の文字のみを許可します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> ASP.NET 検証コントロールは、ユーザー入力で特定の文字だけが許可されることを保証できます。 ただしこのままでも、ユーザーから無効な文字を入力し、フォームを送信しようとしています。


## <a name="overview"></a>概要

ASP.NET 検証コントロールは、ユーザー入力で特定の文字だけが許可されることを保証できます。 ただしこのままでも、ユーザーから無効な文字を入力し、フォームを送信しようとしています。

## <a name="steps"></a>手順

ASP.NET AJAX Control Toolkit に含まれています、`FilteredTextBox`をテキスト ボックスを拡張するコントロール。 アクティブ化される、文字の特定のセットのみがフィールドに入力可能性があります。

これを機能させるには、まず必要があります通常どおり、ASP.NET AJAX `ScriptManager` ASP.NET AJAX Control Toolkit で使用されるも JavaScript ライブラリが読み込まれます。

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

次に、テキスト ボックス必要があります。

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

最後に、`FilteredTextBoxExtender`ユーザーが入力できる文字の制限の制御を行います。 最初に、設定、`TargetControlID`属性を`ID`の`TextBox`コントロール。 使用可能ないずれかを選択し、`FilterType`値。

- `Custom` 既定値です。有効な文字の一覧を提供する必要があります。
- `LowercaseLetters` 小文字のみを使用
- `Numbers` 数字のみ
- `UppercaseLetters` 大文字のみ

場合、`Custom FilterType`を使用する、`ValidChars`プロパティが設定されていないと、入力可能性がありますの文字の一覧を提供する必要があります。 方法: テキスト ボックスにテキストを貼り付けるしようとすると、すべての無効な文字が削除されます。

マークアップを次に示します、`FilteredTextBoxExtender`桁のみを実行できるコントロール (何も必要であったと考えられる`FilterType="Numbers"`)。

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

JavaScript が有効になっている場合は、文字を入力しようと複数のページを実行するには、アプリケーションは動作しません。ただし、桁の数字は、ページに表示されます。 しかし注意保護`FilteredTextBox`を提供しますが堅牢な: 場合 JavaScript が有効になっている、ASP などの追加の検証方法を使用する必要があるために、テキスト ボックスで、すべてのデータを入力します。NET の検証コントロール。


[![数字のみを入力することがあります。](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

数字のみを入力することがあります ([フルサイズの画像を表示する をクリックします](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](allowing-only-certain-characters-in-a-text-box-cs.md)
