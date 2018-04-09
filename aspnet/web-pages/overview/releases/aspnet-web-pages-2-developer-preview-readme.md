---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: ASP.NET Web Pages 2 開発者プレビューのリリース ノート |Microsoft ドキュメント
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 1a43b2b12af9cd223d8a3622239743f7c431f617
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 開発者プレビューのリリース ノート
====================
によって[Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 開発者プレビューのリリース ノート

2011 年 9 月 14日

### <a name="contents"></a>目次

#### <a id="_Toc303701284"></a>  インストールに関する注意事項

Web ページ 2 Developer Preview をインストールするには、これらのオプションがあります。

- WebMatrix 2 のベータ版を使用してインストール、 [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=226883)です。 WebMatrix は、ASP.NET Web Pages を含んだ無料の web 開発ツールのセットです。 詳細については、インストールに関するセクションを参照してください。[上部機能によって、ASP.NET Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824)です。

- 使用して直接 Web Pages 2 Developer Preview をインストール、[ダウンロード リンク](https://go.microsoft.com/fwlink/?LinkID=226335)です。 メモ帳などのエディター、テキストを使用して Web Pages アプリケーションを作成する場合は、このアプローチを使用します。 Web Pages 2 アプリケーションを実行するのには、IIS Express 7.5 が必要です。 (これは WebMatrix で自動的に含まれる) です。IIS Express を使用する Web ページのページをテストする方法に関するヒントを参照して、サイドバー「を作成し、テスト ASP.NET ページを使用して、独自テキスト エディター」 [WebMatrix と ASP.NET Web Pages の概要](https://go.microsoft.com/fwlink/?LinkId=202889)です。

ASP.NET Web Pages 2 Developer Preview をインストールしてサイド バイ サイドで実行できる ASP.NET Web ページの 1。 <a id="a"></a>詳細についてを参照してください「実行されている Web ページ アプリケーション サイド バイ サイド」で[上部機能によって、Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824)です。

#### <a id="_Toc303701285"></a>  ドキュメント

チュートリアルおよびその他の情報については、ASP.NET Web Pages は、ASP.NET web サイトの Web ページのページで使用可能な ([https://www.asp.net/web-pages/](../../index.md))。 新機能および Web Pages 2 での機能強化については、次を参照してください。[上部機能によって、Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824)です。

#### <a id="_Toc303701286"></a>  サポート

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> これは、プレビュー リリースであり、公式にサポートされていません。 このリリースの操作についての質問がある場合、ASP.NET Web Pages のフォーラムに投稿 ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ) では、ASP.NET コミュニティのメンバーは頻繁に、非公式のサポートを提供することができます。

#### <a id="_Toc303701287"></a>  ソフトウェアの要件

ASP.NET Web Pages 2 には、.NET Framework 4 が必要です。 .NET Framework 4.5 Developer Preview のリリースでも動作します。

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>修正プログラム、既知の問題、および重大な変更

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **\*メソッド (たとえば、IsDateTime) ここで正しい値を返す、すべてのカルチャ。** などのメソッド*IsDateTime*以前に返された*false*ときに戻っている必要があります*true*に以前のカルチャ固有のチェックを実行していたためです。 これらのメソッドは、現在のカルチャを考慮に入れる修正されています。 これは、互換性に影響する変更です。アプリケーションは、従来の動作に依存、分割されます。
- **Href メソッドの動作が変更されました。** 以前は、Href("~/SomeFile") を呼び出すには、現在実行中のファイルに相対 URL は返します。 今すぐ Href("~/SomeFile") は、アプリケーションのルートからの絶対パスを常に返します。 ほとんどの場合、この動作は、戻り値の違いを作成しません。 この変更は、特定の Ajax シナリオを修正しました。 たとえば、次のコード例を考えてみます。 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    このコード以前に解決される Images/Logo.jpg、そのページに Ajax 要求に対して正しいとなります。 ルートを解決するようになりましたが、(/MySite/Images/Logo.jpg)。
- **HttpContext.RedirectLocal メソッドが変更された**です。 このメソッドは、現在のアプリケーションに対する相対 Url のみを受け入れるようになりました。 完全修飾 Url は拒否されます。
- **ModelState.IsValid メソッド今すぐする必要がありますまず Validate を呼び出して**です。 新しい入力の検証メソッドを使用するアプリケーションを変換してを呼び出している場合、 *ModelState.IsValid*メソッド、ここで呼び出す必要があります*Validation.Validate*事前です。 たとえば、このパターンを今すぐに従う必要があります。 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  ただし、ことをお勧めする場合、新しい入力の検証メソッドを使用するを使用しないで*ModelState.IsValid*です。 代わりに、次のようにコードを構造化します。 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **Internet Explorer 7 および Internet Explorer 8 では、クライアント側検証は機能しません**です。 クライアント側の検証は機能しません jQuery 1.6.2 でした、との非互換性のため、既定のプロジェクト テンプレートに含まれています。 (サーバー側の検証は動作します。)。

#### <a id="_Toc303701289"></a>  免責事項

© 2011 Microsoft Corporation. All rights reserved. このドキュメントは提供される"としてでは、します"。 情報および見解 URL および他のインターネット Web サイトの参照を含む、このドキュメントでは、通知なく変更可能性があります。 お客様は、その使用に関するリスクを負うものとします。
