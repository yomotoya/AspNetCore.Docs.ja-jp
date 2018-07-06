---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Web Pages (Razor) FAQ |Microsoft Docs
author: tfitzmac
description: この記事では、WebMatrix と ASP.NET Web Pages (Razor) についてよく寄せられる質問を示します。 ソフトウェアのバージョンが、チュートリアル ASP.NET Web Pages で使用 (R...
ms.author: aspnetcontent
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 1f97056c952757ea2d009eaca57d904791e80ca3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814237"
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages (Razor) FAQ
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix は推奨されなくの統合開発環境として ASP.NET Web Pages の。 使用[Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio)または[Visual Studio Code](https://code.visualstudio.com/)します。
>
> この記事では、WebMatrix と ASP.NET Web Pages (Razor) についてよく寄せられる質問を示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2、WebMatrix 2 では、Visual Studio 2012 とも動作します。


- [ASP.NET Web ページ、ASP.NET Web フォーム、ASP.NET MVC の違いは何ですか。](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Web ページを使用するために WebMatrix ですか。](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Web ページ上の ASP.NET Web フォーム コントロールを使用できますか。](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [WebMatrix を使用せず、ASP.NET Web Pages サイトを展開できますか。](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [WebSecurity ヘルパーを使用してログインをサポートするには](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [ASP.NET Web Pages は HTML5 をサポートしますか。](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Web ページで JavaScript と jQuery を使用できますか。](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [その他のリソース](#AdditionalResources)

エラーおよびその他の問題に関する質問については、次を参照してください。、 [ASP.NET Web Pages (Razor) トラブルシューティング ガイド](https://go.microsoft.com/fwlink/?LinkId=253001)します。

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>ASP.NET Web ページ、ASP.NET Web フォーム、ASP.NET MVC の違いは何ですか。

3 つすべてが、動的な web アプリケーションを作成するための ASP.NET のテクノロジです。

- ASP.NET Web ページは、HTML ページ、および機能の単純で軽量構文への動的 (サーバー側) のコードとデータベースへのアクセスの追加について説明します。
- ASP.NET Web フォームは、ページのオブジェクト モデルと従来のウィンドウの種類のコントロール (ボタン、リストなど) に基づきます。 Web フォームでは、クライアント ベース (Windows フォーム) の開発に携わっていればユーザーになじみのあるイベント ベースのモデルを使用します。
- ASP.NET MVC では、ASP.NET のモデル-ビュー-コント ローラー パターンを実装します。 「関心の分離」が強調 (処理、データ、および UI レイヤー)。

3 つすべてのフレームワークが完全にサポートされ、ASP.NET チームで開発し続けます。 使用するには、どのフレームワークの選択が、バック グラウンドに依存する一般に、ASP.NET を使用したエクスペリエンスとします。

具体的には、ASP.NET Web ページは、ページにサーバーの処理を追加する HTML を既に知っている人が簡単に設計されました。 学生、趣味のユーザーを一般にプログラミングするは初めての適切な選択肢になります。 ASP.NET 以外の web テクノロジの経験がある開発者のための適切な選択ことができます。

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Web ページを使用するために WebMatrix ですか。

いいえ。 WebMatrix は推奨されなくの統合開発環境として ASP.NET Web Pages の。 使用[Visual Studio](program-asp-net-web-pages-in-visual-studio.md)または[Visual Studio Code](https://code.visualstudio.com/)します。

使用して個別のコンポーネント製品をインストールするには Visual Studio または Visual Studio Code を使用しない場合は、 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)します。 次の製品が必要です。

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (これも、ASP.NET Web ページ フレームワークがインストールされます)
- IIS Express (web サーバー)
- Microsoft SQL Server Compact 4.0 (データベース)

テキスト エディターを使用して、編集 *.cshtml* (または *.vbhtml*) ページ。

SQL Server Compact データベースの管理 (*.sdf*ファイル)、ツールが若干困難なし。 管理するための visual Studio containds ツール *.sdf*データベース。 多くの SQL Server 管理タスクを実行するコードで SQL コマンドを実行することもできます。

テストする *.cshtml*ページ統合開発環境 (IDE) を使用せず、展開するライブ サーバーにします。 (を参照してください[WebMatrix を使用せず、ASP.NET Web Pages サイトをデプロイできますか?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>IIS Express IDE を使用せずに実行

Web サーバーとしてコンピューターに IIS Express をインストールする場合は、ページをテストするを使用できます。 コマンドラインから IIS Express を実行し、特定のポート番号を関連付けることができます。 要求すると、そのポートを指定する *.cshtml*お使いのブラウザー内のファイル。

Windows で管理者特権を持つコマンド プロンプトを開きに変更*C:\Program Files\IIS Express。* (64 ビット システムでは、フォルダーを使用して*C:\Program Files (x86) \IIS Express)。* 実際のパス、サイトを使用して、次のコマンドを入力します。

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

他のプロセスで既に予約されていない任意のポート番号を使用することができます。 (1024 より大きいポート番号は、通常は無料)。`path`値には、web サイト フォルダーのパスを使用して、場所、 *.cshtml*ファイルします。

ページを処理するために IIS Express をセットアップするには、このコマンドを実行した後は、ブラウザーを開きを参照、 *.cshtml*ファイル。 次のような URL を使用します。

`http://localhost:35896/default.cshtml`

IIS Express のコマンド ライン オプションについては、次のように入力します。`iisexpress.exe /?`コマンドライン。

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Web ページ上の ASP.NET Web フォーム コントロールを使用できますか。

いいえ。 Web フォーム コントロールのように、[チェック ボックスをオン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox)コントロール、[検証コントロール](https://msdn.microsoft.com/library/bwd43d0x)、および[GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) Web フォーム ページでのみ動作の制御 (*.aspx*ファイル)。 これらのコントロールには、Web フォーム ページのフレームワークが必要です。

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>WebMatrix を使用せず、ASP.NET Web Pages サイトを展開できますか。

はい。 (通常は FTP を使用して) をサーバーに web サイトのファイルを手動でコピーすることができます。 手動でコピーを実行する場合は、SQL Server Compact (データベース) をサポートするファイルをコピーする必要があります。 詳細については、ブログ記事を参照してください。[ツールを使用せずに Web ページを展開するアプリケーション](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317)します。

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>WebSecurity ヘルパーを使用してログインをサポートするには

いいえ。 `SimpleMembership` ASP.NET Web ページの一部であるプロバイダーの 1 つです。 Asp.net (Web フォームでの操作に使用した可能性があります) の一部であるセキュリティ プロバイダーも使用できます。 たとえば、できます認証を使用するフォーム ASP.NET Web Pages で Web フォームと同様です。 使用する方法の 1 つの例ではフォーム認証、Microsoft サポート記事を参照してください[How To Implement Forms-Based 認証を使用して C# .NET、ASP.NET アプリケーションで](https://support.microsoft.com/kb/301240)します。 簡単な例をダウンロードするには、次を参照してください。[の ASP.NET のバージョン"ログイン&amp;パスワード](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm)します。

Windows 認証を使用する方法については、ブログの投稿を参照してください。[を使用して Windows 認証 ASP.NET Web Pages で](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298)します。

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>ASP.NET Web Pages は HTML5 をサポートしますか。

はい。 ASP.NET Web ページを作成するページ (*.cshtml*または *.vbhtml*ページ) も、ページがレンダリングされる前に、サーバーで実行されるコードが含まれている HTML ページは基本的にします。 HTML5 要素を使用して、ユーザーのブラウザーは HTML5 をサポートする限り、 *.cshtml*または *.vbhtml*ページ。

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Web ページで JavaScript と jQuery を使用できますか。

もちろん、できます。 ASP.NET Web ページを作成するページ (*.cshtml*または *.vbhtml*ページ) にサーバー コードで HTML ページだけです。 そのため、JavaScript または jQuery を使用して通常の HTML ページで実行できる何も行うことができますも、 *.cshtml*または *.vbhtml*ページ。

**スターター サイト**WebMatrix でテンプレートには、さまざまな jQuery ライブラリが含まれています。 そのテンプレートを使用してサイトを作成する場合、*スクリプト*フォルダーには、jQuery のコア ライブラリが含まれています (*jquery 1.6.2.js)* と jQuery の検証用ライブラリ (*jquery.validate.js*など。)。

JQuery と ASP.NET Web Pages を使用する方法を説明したいくつかのブログの投稿を次に示します。

- [JQuery した場合の利点を追加するには、ASP.NET Web Pages に WebMatrix を使用した](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/)Rachel appel
- [5 分: WebMatrix の jQuery UI + json + jQuery テンプレート](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/)Jonas Eriksson によって
- [WebMatrix および jQuery フォーム](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms)Mike Brind によって

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>その他のリソース


[ASP.NET Web ページ (Razor) トラブルシューティング ガイド](https://go.microsoft.com/fwlink/?LinkId=253001)

[WebMatrix と ASP.NET Web Pages のフォーラム](https://forums.asp.net/1224.aspx/1?WebMatrix)ASP.NET web サイト
