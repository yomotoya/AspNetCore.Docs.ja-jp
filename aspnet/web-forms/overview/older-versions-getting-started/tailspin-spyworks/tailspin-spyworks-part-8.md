---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'パート 8: 最終ページ、例外処理、およびまとめ |Microsoft Docs'
author: JoeStagner
description: このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 8 では、ページ、および例外に関する、連絡先ページを追加します.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 4fb0147a5cd8621e5341e4995960adf2f00fffc0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804718"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>パート 8: 最終ページ、例外処理、およびまとめ
====================
によって[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力でスケーラブルなアプリケーションを作成するはどの非常に単純なを示します。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアを構築する方法を示します。
> 
> このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 8 では、ページ、および例外処理についての連絡のページを追加します。 これは、シリーズのまとめです。


## <a id="_Toc260221680"></a>  ページ (ASP.NET から電子メールを送信する) にお問い合わせください。

ContactUs.aspx をという名前の新しいページを作成します。

デザイナーを使用して、ToolkitScriptManager、AjaxdControlToolkit からエディター コントロールなど、特別な注意事項は次の形式を作成します。 .

![](tailspin-spyworks-part-8/_static/image1.jpg)

分離コード ファイルで、クリック イベント ハンドラーを生成し、連絡先情報を電子メールとして送信するメソッドを実装する [送信] ボタンをダブルクリックします。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

このコードでは、web.config ファイルにメールを送信するのに使用する SMTP サーバーを指定する構成セクションのエントリが含まれている必要があります。

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  ページについて

AboutUs.aspx という名前のページを作成し、任意のコンテンツを追加します。

## <a id="_Toc260221682"></a>  グローバル例外ハンドラー

最後に、アプリケーション全体では、例外をスローしましたが、不測の事態をコールドも web アプリケーションでは原因が未処理の例外。

未処理の例外の web サイトの訪問者に表示されることはありません必要があります。

![](tailspin-spyworks-part-8/_static/image2.jpg)

悲惨なユーザー エクスペリエンスとは別のハンドルされない例外は、セキュリティ上の問題をこともできます。

この問題を解決するためには、グローバル例外ハンドラーを実装します。

これを行うには、Global.asax ファイルを開き、次の事前生成されたイベント ハンドラーに注意してください。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

アプリケーションを実装するコードを追加\_エラー ハンドラーとして次のとおりです。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

ソリューションに Error.aspx をという名前のページを追加し、次のマークアップ スニペットを追加します。

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

ページで今すぐ\_要求オブジェクトからイベント ハンドラーの抽出、エラー メッセージを読み込みます。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  まとめ

ASP.NET WebForms 簡単見たなどのデータベース アクセス、メンバーシップ、AJAX、高度な web サイトを作成します。 非常に短時間です。

このチュートリアルが願って、独自の ASP.NET WebForms アプリケーションの構築を開始する必要があるツールです。

> [!div class="step-by-step"]
> [前へ](tailspin-spyworks-part-7.md)
