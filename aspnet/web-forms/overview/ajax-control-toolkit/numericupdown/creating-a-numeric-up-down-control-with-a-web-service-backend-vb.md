---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: "Web サービスのバックエンド (VB) で、数値のアップダウン コントロールを作成する |Microsoft ドキュメント"
author: wenz
description: "チェック ボックスに値を入力するユーザーを待つ代わりに、数値のアップダウン コントロール (Windows と他のオペレーティング システムに存在する場合) はより多くの c を生じる可能性があります."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ceefd6c18761c2abe3f3a4298d340642a0951d6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Web サービス バックエンド (VB) と数値のアップダウン コントロールを作成します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> チェック ボックスに値を入力するユーザーを待つ代わりに、数値のアップダウン コントロール (Windows と他のオペレーティング システムに存在する場合) を生じる可能性が増えるにつれて快適です。 既定では、NumericUpDown コントロール常にだけ増加または減少値 1 が web サービスの証明をより柔軟です。


## <a name="overview"></a>概要

チェック ボックスに値を入力するユーザーを待つ代わりに、数値のアップダウン コントロール (Windows と他のオペレーティング システムに存在する場合) を生じる可能性が増えるにつれて快適です。 既定では、`NumericUpDown`柔軟性を証明する web サービスが、常にコントロールを拡大または値を 1 ずつ減少します。

## <a name="steps"></a>手順

ASP.NET AJAX コントロールのツールキットに含まれています、`NumericUpDown`テキスト ボックスに 2 つのボタンが自動的に追加されるエクステンダー: を減らしたりのいずれかの値を増やすことのいずれか。 ただし、コントロールは、web サービスの呼び出し (またはページのメソッドの呼び出し) をもサポートします。 ときに、または下のボタンをクリックすると、JavaScript コードは、web サーバーに接続して、あるメソッドを実行します。 メソッドのシグネチャは、次のいずれかを示します。

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

`current`引数は、現在の値、テキスト ボックスに、`tag`属性のプロパティとして設定できる追加のコンテキスト データ、`NumericUpDown`エクステンダー (ただし、必須ではありません)。

このサンプルでは、数値のアップダウン コントロールいてはいけないのみを許可する 2 の累乗値: 1、2、4、8、16、32、64、およびなどです。 したがって、ユーザーが値を大きくときに実行されるメソッドには、倍精度浮動小数点、古い値です。 必要があります。その他のメソッドは、2 つのによって値を分割する必要があります。 したがって、完全な web サービスを次に示します。

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

最後に、新しい ASP.NET ページを作成します。 必要がある通常どおり、`ScriptManager`コントロール、`TextBox`コントロールと`NumericUpDownExtender`コントロール。 後者には web サービス情報を提供する必要があります。

- `ServiceDownMethod`web メソッドまたはメソッドのページを下の名前
- `ServiceDownPath`下向きサービス メソッドを使用して web サービスへのパスページ メソッドを使用している場合は、省略します。
- `ServiceUpMethod`web メソッドまたはメソッドをページ上の名前
- `ServiceUpPath`サービス メソッドを使用して web サービスへのパスページ メソッドを使用している場合は、省略します。

ページの完全なマークアップを次に示します。

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

ページを実行する場合は、どのようにテキスト ボックスに値常に 2 倍に上のボタンをクリックして、下のボタンをクリックすると、半分になりますに注意してください。


[![2 の累乗の数値のみが表示されます。](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

2 の累乗の数値のみが表示されます ([フルサイズのイメージを表示するをクリックして](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

>[!div class="step-by-step"]
[前へ](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
