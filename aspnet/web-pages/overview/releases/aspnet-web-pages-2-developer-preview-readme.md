---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: ASP.NET Web ページ 2 開発者プレビュー ReadMe |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 09/14/2011
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 0a89216c0f65d49d00c96a27a5ab33e3872f9bc5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818347"
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web ページ 2 開発者プレビュー ReadMe
====================
によって[Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web ページ 2 開発者プレビュー ReadMe

2011 年 9 月 14日

### <a name="contents"></a>目次

#### <a id="_Toc303701284"></a>  インストールに関する注記

Web ページ 2 開発者プレビューをインストールするには、これらのオプションがあります。

- WebMatrix 2 ベータ版を使用してインストール、 [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=226883)します。 WebMatrix は、ASP.NET Web ページを含む無料の web 開発ツールのセットです。 詳細については、インストールのセクションを参照してください。 [ASP.NET Web ページ 2 開発者プレビューのベスト機能](https://go.microsoft.com/fwlink/?LinkID=227824)します。

- 使用して直接 Web ページ 2 開発者プレビューをインストール、[ダウンロード リンク](https://go.microsoft.com/fwlink/?LinkID=226335)します。 メモ帳などのエディター、テキストを使用して Web ページ アプリケーションを作成する場合は、このアプローチを使用します。 Web ページ 2 アプリケーションを実行するのには、IIS Express 7.5 が必要です。 (これは、WebMatrix で自動的に含まれています)IIS Express を使用して Web ページのページをテストする方法に関するヒントを参照してください、補足記事の作成とテスト ASP.NET ページを使用して、独自テキスト エディター [WebMatrix と ASP.NET Web Pages の概要](https://go.microsoft.com/fwlink/?LinkId=202889)します。

ASP.NET Web Pages 2 Developer Preview がインストールできるし、サイド バイ サイドでを実行できる ASP.NET Web ページ 1。 <a id="a"></a>詳細については、「実行されている Web ページ アプリケーション - サイド」セクションを参照してください[Web ページ 2 開発者プレビューのベスト機能](https://go.microsoft.com/fwlink/?LinkID=227824)します。

#### <a id="_Toc303701285"></a>  ドキュメント

チュートリアルとその他の情報については、ASP.NET Web Pages は、ASP.NET web サイトの Web ページのページで使用可能な ([https://www.asp.net/web-pages/](../../index.md))。 新しい機能と Web ページ 2 の機能強化については、次を参照してください。 [Web ページ 2 開発者プレビューのベスト機能](https://go.microsoft.com/fwlink/?LinkID=227824)します。

#### <a id="_Toc303701286"></a>  サポート

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> これは、プレビュー リリースであり、正式にサポートされていません。 このリリースの操作について質問等がございましたら、ASP.NET Web Pages のフォーラムに投稿 ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) )、ASP.NET コミュニティのメンバーが頻繁に、非公式のサポートを提供することができます。

#### <a id="_Toc303701287"></a>  ソフトウェアの要件

ASP.NET Web ページ 2 では、.NET Framework 4 が必要です。 .NET Framework 4.5 Developer Preview のリリースでも機能します。

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>修正プログラム、既知の問題、および重大な変更

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **\*メソッド (たとえば、IsDateTime) ここで正しい値を返すすべてのカルチャ。** などのメソッド*IsDateTime*以前に返した*false*ときに戻っている必要があります*true*に以前のカルチャ固有のチェックを実行していたためです。 これらのメソッドが現在のカルチャを考慮に入れてに修正されました。 これは重大な変更です。アプリケーションが、以前の動作に依存している場合は中断されます。
- **Href メソッドの動作が変更されました。** 以前は、Href("~/SomeFile") を呼び出すと、現在実行中のファイルの相対 URL が返されます。 今すぐ Href("~/SomeFile") は、アプリケーションのルートからの絶対パスを常に返します。 ほとんどの場合、この動作は戻り値の相違点をことはありません。 特定の Ajax シナリオの解決に変更されました。 たとえば、次のコード例を考えてみます。 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    このコード以前に解決 Images/Logo.jpg、そのページに Ajax 要求に対して正しいこれがなります。 ルートに解決するようになりましたが、(/MySite/Images/Logo.jpg)。
- **HttpContext.RedirectLocal メソッドが変更された**します。 このメソッドは、現在のアプリケーションの Url のみを受け入れるようになりました。 完全修飾 Url は拒否されます。
- **ModelState.IsValid メソッドが必要になりましたを最初に検証を呼び出す**します。 新しい入力の検証メソッドを使用するアプリケーションを変換しを呼び出している場合、 *ModelState.IsValid*メソッドを呼び出す必要があるようになりました*Validation.Validate*事前します。 たとえば、このパターンに従うようになりましたする必要があります。 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  ただしを新しい入力の検証メソッドを使用する場合を使用しない推奨*ModelState.IsValid*します。 代わりに、このようなコードを構造します。 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **Internet Explorer 7 と Internet Explorer 8 では、クライアント側の検証は機能しません**します。 クライアント側の検証は機能しません 1.6.2、jQuery と互換性がないのため、既定のプロジェクト テンプレートに含まれています。 (サーバー側の検証動作します。)。

#### <a id="_Toc303701289"></a>  免責事項

© 2011 Microsoft Corporation. All rights reserved. このドキュメントが提供される"としてのです"。 情報および見解 URL やその他のインターネット Web サイトの参照を含め、このドキュメントでは、予告なく変更可能性があります。 お客様は、その使用に関するリスクを負うものとします。
