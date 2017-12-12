---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: "ASP.NET Web Pages (Razor) FAQ |Microsoft ドキュメント"
author: tfitzmac
description: "この記事は、WebMatrix と ASP.NET Web Pages (Razor) に関するいくつかのよく寄せられる質問を一覧表示します。 ソフトウェアのバージョンが、チュートリアル ASP.NET Web Pages で使用 (R..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 7f6dc3b56a33bcbe3e1e4086681ca1ba76d7d153
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages (Razor) FAQ
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix は推奨されなくなりました統合開発環境として ASP.NET Web Pages のです。 使用して[Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio)または[Visual Studio Code](https://code.visualstudio.com/)です。
>
> この記事は、WebMatrix と ASP.NET Web Pages (Razor) に関するいくつかのよく寄せられる質問を一覧表示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2、WebMatrix 2、および Visual Studio 2012 でも動作します。


- [ASP.NET Web ページ、ASP.NET Web フォーム、ASP.NET MVC の違いは何ですか。](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Web ページを使用するために WebMatrix ですか。](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [ASP.NET Web フォーム ページのコントロールを Web ページを使用できますか。](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [WebMatrix を使用せず、ASP.NET Web Pages サイトを配置できますか。](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [ログインをサポートするために、WebSecurity ヘルパーを使用する必要がありますは?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [ASP.NET Web Pages は HTML5 をサポートしますか。](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Web ページで JavaScript と jQuery を使用できますか。](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [その他のリソース](#AdditionalResources)

エラーおよびその他の問題に関する質問については、次を参照してください。、 [ASP.NET Web Pages (Razor) トラブルシューティング ガイド](https://go.microsoft.com/fwlink/?LinkId=253001)です。

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>ASP.NET Web ページ、ASP.NET Web フォーム、ASP.NET MVC の違いは何ですか。

3 つすべては、動的な web アプリケーションを作成するための ASP.NET テクノロジです。

- ASP.NET Web ページは、HTML ページ、および機能の簡単、軽量構文を動的 (サーバー側) コードとデータベースへのアクセスを追加することについて説明します。
- ASP.NET Web フォームは、ページのオブジェクト モデルと従来のウィンドウ タイプ コントロール (ボタン、リストなど) に基づいています。 Web フォームでは、クライアント ベース (Windows フォーム) の開発で作業したユーザーになじみのあるイベントに基づくモデルを使用します。
- ASP.NET MVC では、ASP.NET のモデル ビュー コント ローラー パターンを実装します。 「関心の分離」が強調 (処理、データ、および UI レイヤー)。

次の 3 つすべてのフレームワークは完全にサポートし、ASP.NET チームによって開発に進みます。 一般に、使用するには、どのフレームワークの選択によって異なります、バック グラウンドで ASP.NET のエクスペリエンスとします。

具体的には、ASP.NET Web ページは、ページにサーバーの処理を追加する HTML を既に知っているユーザーを簡単に設計されました。 受講者、趣味はプログラミングの経験となっている人一般に、適切な選択であります。 ASP.NET 以外の web テクノロジの使用経験を持つ開発者に適してことができます。

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Web ページを使用するために WebMatrix ですか。

いいえ。 WebMatrix は推奨されなくなりました統合開発環境として ASP.NET Web Pages のです。 使用して[Visual Studio](program-asp-net-web-pages-in-visual-studio.md)または[Visual Studio Code](https://code.visualstudio.com/)です。

使用して個別のコンポーネント製品をインストールするには Visual Studio または Visual Studio のコードを使用しない場合は、 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)です。 次の製品が必要です。

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 をインストールします (同様に、ASP.NET Web Pages framework)
- IIS Express (web サーバー)
- Microsoft SQL Server Compact 4.0 (データベース)

テキスト エディターを使用して編集する*.cshtml* (または*.vbhtml*) ページ。

SQL Server Compact データベースの管理 (*.sdf*ファイル) を含まないツールは少し難しくなります。 管理するための visual Studio containds ツール*.sdf*データベース。 多くの SQL Server 管理タスクを実行するコードでの SQL コマンドを実行することもできます。

テストする*.cshtml*ページ統合開発環境 (IDE) を使用せず、展開したりできますライブ サーバーにします。 (を参照してください[WebMatrix を使用せず、ASP.NET Web Pages サイトを展開できますか](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))。

### <a name="running-iis-express-without-using-an-ide"></a>IIS Express、IDE を使用せずに実行します。

Web サーバーとしてコンピューターに IIS Express をインストールする場合は、ページをテストすることを使用できます。 コマンドラインからの IIS Express の実行し、特定のポート番号を関連付けることができます。 要求するときに、そのポートを指定する*.cshtml*お使いのブラウザー内のファイルです。

Windows では、管理者特権でコマンド プロンプトを開くし、変更*C:\Program Files\IIS Express です。* (64 ビット システムでは、フォルダーを使用して*C:\Program Files (x86) \IIS Express です)。*サイトに実際のパスを使用して、次のコマンドを入力します。

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

他のプロセスによって予約されていない任意のポート番号を使用することができます。 (1024 より上のポート番号は、通常無料)。`path`値、web サイト フォルダーのパスを使用して場所、 *.cshtml*ファイルします。

ページに提供する IIS Express をセットアップするには、このコマンドを実行した後は、ブラウザーを開きを参照、 *.cshtml*ファイル。 次のように URL を使用します。

`http://localhost:35896/default.cshtml`

IIS Express のコマンド ライン オプションについては、次のように入力します。`iisexpress.exe /?`コマンドライン。

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>ASP.NET Web フォーム ページのコントロールを Web ページを使用できますか。

いいえ。 Web フォーム コントロールと同様に、 [ チェック ボックス](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.checkbox)コントロール、[検証コントロール](https://msdn.microsoft.com/en-us/library/bwd43d0x)、および[GridView](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview) Web フォーム ページでのみ動作を制御 (*.aspx*ファイル)。 これらのコントロールでは、Web フォーム ページのフレームワークが必要です。

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>WebMatrix を使用せず、ASP.NET Web Pages サイトを配置できますか。

はい。 (通常 FTP を使用して、サーバーに web サイトのファイルを手動でコピーすることができます。 手動でコピーを実行する場合は、SQL Server Compact (データベース) をサポートするファイルをコピーする必要があります。 詳細については、ブログ記事を参照してください。[ツールを使用せずに Web ページを展開するアプリケーション](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317)です。

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>ログインをサポートするために、WebSecurity ヘルパーを使用する必要がありますは?

いいえ。 `SimpleMembership` ASP.NET Web Pages の一部であるプロバイダーが 1 つのオプションです。 ASP.NET (Web フォームでの操作に使用した可能性があります) の一部であるセキュリティ プロバイダーも使用できます。 たとえば、できます認証を使用するフォーム ASP.NET Web Pages で Web フォームのと同様です。 使用する方法の 1 つの例ではフォーム認証、Microsoft サポートの記事を参照してください[How To Implement Forms-Based 認証を使用して C# .NET、ASP.NET アプリケーションで](https://support.microsoft.com/kb/301240)です。 単純な例をダウンロードするを参照してください。[の ASP.NET のバージョン"ログイン&amp;パスワード](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm)です。

Windows 認証を使用する方法については、ブログの投稿を参照してください。[を使用して Windows 認証では、ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298)です。

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>ASP.NET Web Pages は HTML5 をサポートしますか。

はい。 ASP.NET Web Pages を作成するページ (*.cshtml*または*.vbhtml*ページ) も、ページが表示される前に、サーバー上で実行されるコードが含まれる HTML ページは基本的にします。 HTML5 の要素を使用して、ユーザーのブラウザーが HTML5 をサポートしている限り、 *.cshtml*または*.vbhtml*ページ。

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Web ページで JavaScript と jQuery を使用できますか。

もちろん、できます。 ASP.NET Web Pages を作成するページ (*.cshtml*または*.vbhtml*ページ) にサーバー コードで HTML ページだけです。 そのため、何も行うことができます、標準の HTML ページに JavaScript または jQuery を使用して行うことができますも、 *.cshtml*または*.vbhtml*ページ。

**スターター サイト**WebMatrix でテンプレートには、多く jQuery ライブラリにはが含まれています。 そのテンプレートを使用して、サイトを作成する場合、*スクリプト*フォルダーには、jQuery コア ライブラリが含まれています (*jquery 1.6.2.js)*および jQuery 検証のためのライブラリ (*jquery.validate.js*, などです。)。

ASP.NET Web Pages を jQuery を使用する方法を説明した一部のブログ投稿を次に示します。

- [ASP.NET Web Pages に WebMatrix を使用して jQuery 適合度を追加する](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/)Rachel Appel によって
- [5 分: WebMatrix + jQuery UI + json + jQuery テンプレート](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/)Jonas Eriksson によって
- [WebMatrix および jQuery フォーム](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms)Mike Brind によって

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>その他のリソース


[ASP.NET Web Pages (Razor) トラブルシューティング ガイド](https://go.microsoft.com/fwlink/?LinkId=253001)

[WebMatrix と ASP.NET Web Pages のフォーラム](https://forums.asp.net/1224.aspx/1?WebMatrix)ASP.NET web サイト
