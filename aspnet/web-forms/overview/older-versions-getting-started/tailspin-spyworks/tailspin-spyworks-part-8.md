---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: "手順 8: 最終的なページ、例外処理、および結論 |Microsoft ドキュメント"
author: JoeStagner
description: "このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 8 の一部では、ページ、および例外に関する、連絡先ページを追加しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 0dd1717ff1051f18a78fe77402c7603008b9b486
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>手順 8: 最終的なページ、例外処理、および結論
====================
によって[行える](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。
> 
> このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 8 の一部では、ページ、および例外処理についての連絡先のページを追加します。 これは、系列の最後です。


## <a id="_Toc260221680"></a>ページ (ASP.NET から電子メールを送信する) にお問い合わせください。

ContactUs.aspx をという名前の新しいページを作成します。

デザイナーを使用して、次の形式を ToolkitScriptManager、AjaxdControlToolkit からエディター コントロールなど、特別な注意事項を作成します。 。

![](tailspin-spyworks-part-8/_static/image1.jpg)

分離コード ファイルでクリック イベント ハンドラーを生成し、連絡先に関する情報を電子メールとして送信するメソッドを実装するには、"Submit"ボタンをダブルクリックします。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

このコードでは、web.config ファイルにメールを送信するのに使用する SMTP サーバーを指定する構成セクションのエントリが含まれている必要があります。

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>ページについて

AboutUs.aspx をという名前のページを作成し、すべてのコンテンツを追加します。

## <a id="_Toc260221682"></a>グローバル例外ハンドラー

最後に、例外をスローして、アプリケーション全体で、そのコールド不測の事態も web アプリケーションでは原因が未処理の例外。

Web サイトの訪問者に表示されるハンドルされない例外作成することはありません。

![](tailspin-spyworks-part-8/_static/image2.jpg)

大きな引っかきユーザー エクスペリエンスをされているとは別の未処理の例外にセキュリティ上の問題ことができます。

この問題を解決するためには、グローバル例外ハンドラーを実装します。

これを行うには、Global.asax ファイルを開き、次の事前に生成されたイベント ハンドラーに注意してください。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

アプリケーションを実装するコードを追加\_エラー ハンドラーを次のようにします。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

ソリューションに Error.aspx をという名前のページを追加し、このマークアップのスニペットを追加します。

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

ページで今すぐ\_要求オブジェクトから、エラー メッセージがイベント ハンドラーの抽出をロードします。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>結論

ASP.NET WebForms 簡単これまで見てきたなどのデータベース アクセス、メンバーシップ、AJAX、高度な web サイトを作成します。 非常に短時間です。

おそらくこのチュートリアルが提供されているツールを独自の ASP.NET WebForms アプリケーションの構築を開始する必要があります!

>[!div class="step-by-step"]
[前へ](tailspin-spyworks-part-7.md)
