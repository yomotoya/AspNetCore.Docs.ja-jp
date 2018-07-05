---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: ASP.NET および Web Tools 2013.1 for Visual Studio 2012 のリリース ノート |Microsoft Docs
author: microsoft
description: このドキュメントでは、ASP.NET および Web Tools 2013.1 for Visual Studio 2012 のリリースについて説明します。
ms.author: aspnetcontent
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 33577dec7278694f932ac7eded84359baacf65bc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829102"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>ASP.NET と Web Tools 2013.1 for Visual Studio 2012 のリリース ノート
====================
によって[Microsoft](https://github.com/microsoft)

> このドキュメントでは、ASP.NET および Web Tools 2013.1 for Visual Studio 2012 のリリースについて説明します。


## <a name="contents"></a>目次

- [インストールに関する注記](#install)
- [ソフトウェアの要件](#requirements)
- ASP.NET および Web Tools 2013.1 for Visual Studio 2012 の新機能

    - [Bootstrap](#bootstrap)
    - [テンプレート](#templates)

        - [ASP.NET MVC 5 テンプレート](#mvc5template)
        - [ASP.NET Web API 2 のテンプレート](#apitemplate)
        - [項目テンプレート](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.NET のスキャフォールディング](#scaffold)
    - [Razor エディター](#razor)
    - [NuGet 2.7](#nuget)
- 既知の問題と重大な変更

    - [ASP.NET のスキャフォールディング](#issuescaffolding)

        - [MVC と Web API スキャフォールティング - HTTP 404 Not Found エラー](#404issue)
        - [Visual Studio Express 2012 for Web スキャフォールディングされた項目を追加した後の作業を停止します](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [サーバー エラーが発生 cshtml ファイルで参照または f5 キーを表示します。](#browseissue)
        - [Url の書き換えと Tilde(~)](#rewriteissue)
    - [テンプレート](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>インストールに関する注記

[インストール](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids)ASP.NET および Web Tools 2013.1 for Visual Studio 2012。

<a id="requirements"></a>
## <a name="software-requirements"></a>ソフトウェア要件

Visual Studio 2012 または Visual Studio Express 2012 for Web のいずれかが必要です。

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>ASP.NET および Web Tools 2013.1 for Visual Studio 2012 の新機能

<a id="bootstrap"></a>
### <a name="bootstrap"></a>ブートス トラップ

MVC 5 コント ローラーとビューをスキャフォールディングするときに、ビューのマークアップを使用して[ブートス トラップ](http://getbootstrap.com/)します。

<a id="templates"></a>
### <a name="templates"></a>テンプレート

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>ASP.NET MVC 5 テンプレート

新しい MVC 5 テンプレートを追加しました。 最新の MVC 5 の NuGet パッケージが参照され、スキャフォールディングを使用して、コント ローラーとビューを追加できます。

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>ASP.NET Web API 2 のテンプレート

新しい Web API 2 のテンプレートを追加しました。 最新の Web API 2 の NuGet パッケージが参照され、スキャフォールディングを使用して、コント ローラーとビューを追加できます。

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>項目テンプレート

MVC 5 ビュー、Web ページ (Razor 3)、および Web API 2 コント ローラーの新しい項目テンプレートを追加しました。 プロジェクトに新しい項目を追加するときに、関連する NuGet パッケージがインストールします。

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework を使用して、MVC または Web API のコント ローラーをスキャフォールディングするときに、Framework 6 を使用します。 Entity Framework の詳細については、次を参照してください。、 [Entity Framework のバージョン履歴](https://msdn.com/data/jj574253)します。

ダウンロードして、Visual Studio 2012 用の Entity Framework 6 Tools をインストールすることができますも。 参照してください、 [Entity Framework の入手](https://msdn.com/data/ee712906#tooling)します。

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET のスキャフォールディング

ASP.NET のスキャフォールディングは、ASP.NET Web アプリケーションのコード生成フレームワークです。 これにより、簡単にデータ モデルを操作するプロジェクトに定型コードを追加できます。

Visual Studio の以前のバージョンでは、スキャフォールディングは、ASP.NET MVC プロジェクトに制限されていました。 この更新で、Web フォームを含む、任意の ASP.NET プロジェクトのスキャフォールディングを使用できます。 この更新プログラムが Web フォーム プロジェクトでは、ページの生成をサポートしていませんが、MVC 依存関係をプロジェクトに追加することで、Web フォームでスキャフォールディングを使用することもできます。 Web フォームのページを生成するためのサポートは今後の更新で追加されます。

スキャフォールディングを使用して、必要なすべての依存関係がプロジェクトにインストールされていることができます。 たとえば、ASP.NET Web フォーム プロジェクトを開始し、スキャフォールディングを使用して Web API コント ローラーを追加すると場合、必要な NuGet パッケージと参照をプロジェクトに自動的に追加されます。

Web フォーム プロジェクトには、MVC のスキャフォールディングを追加するには、追加、**スキャフォールディングされた新しい項目**選択**MVC 5 依存関係**ダイアログ ウィンドウでします。 MVC; をスキャフォールディングするための 2 つのオプションがあります。最小限かつ完全です。 最低限を選択した場合は、NuGet パッケージと ASP.NET MVC の参照のみがプロジェクトに追加されます。 すべてのオプションを選択した場合、MVC プロジェクトに必要なコンテンツ ファイルと、最小の依存関係が追加されます。

非同期コント ローラーをスキャフォールディングするためのサポートは、Entity Framework 6 からの新しい非同期機能を使用します。

詳細とチュートリアルについては、次を参照してください。 [ASP.NET スキャフォールディング概要](../2013/aspnet-scaffolding-overview.md)します。 これらのチュートリアルは、Visual Studio 2013 では、スキャフォールディングを説明しますも ASP.NET と Visual Studio 2012 for Web Tools 2013.1 に適用されます。

<a id="razor"></a>
### <a name="razor-editor"></a>Razor エディター

この更新で、Visual Studio 2012 サポート Razor 3 ツール/編集します。

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 にはで詳細に説明されている新機能の豊富なセットが含まれます[NuGet 2.7 リリース ノート](http://docs.nuget.org/docs/release-notes/nuget-2.7)します。

このバージョンの NuGet では、不足しているパッケージを復元する NuGet を明示的に許可するユーザーの必要性を削除します。 NuGet 2.7 をインストールするときに自動的に不足しているパッケージを復元するユーザーが暗黙的に同意します。 ユーザーは、Visual Studio で NuGet の設定を使用してパッケージの復元から明示的に除外できます。 この変更は、パッケージ復元のしくみを簡略化します。

## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET のスキャフォールディング

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC と Web API スキャフォールティング - HTTP 404 Not Found エラー

スキャフォールディングされた項目をプロジェクトに追加するときにエラーが発生した場合は、プロジェクトは、不整合な状態になることは勧めします。 一部のスキャフォールディングが行われた変更はロールバックされますが、インストール済みの NuGet パッケージなどの他の変更はロールバックされません。 ルーティング構成の変更がロールバックされた場合、ユーザーはスキャフォールディングされた項目に移動するときに、HTTP 404 エラーを受信します。

MVC のこのエラーを修正する新しくスキャフォールディングした項目を追加し、MVC 5 依存関係を選択します (最小または完全のいずれか)。 このプロセスでは、プロジェクトに追加のすべての必要な変更します。

Web api には、このエラーを解決します。

1. 次の WebApiConfig クラスをプロジェクトに追加します。

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. WebApiConfig.Register をアプリケーションで構成\_Global.asax でメソッドを次のように開始します。

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 for Web スキャフォールディングされた項目を追加した後の作業を停止します

Visual Studio Express 2012 for Web は、Entity Framework (Web API 2 コント ローラー アクション、Entity Framework を使用) などでスキャフォールディングされた項目を追加した後の作業を停止する場合は、Visual Studio Express は、アセンブリのネイティブ イメージの読み込みに失敗したことが可能System.Web.Extensions に依存します。

この問題を解決するには、System.Web.Extensions の MSIL イメージを使用する、Visual Studio Express を構成します。

1. 管理者モードでコマンド プロンプトを開きます。
2. %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE または %programfiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (の 64 ビット Windows) に移動します。
3. VWDExpress.exe.config をテキスト エディターで開きます。
4. 下にある次の行を追加、&lt;構成&gt;/&lt;ランタイム&gt;要素。  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. For Web Express 2012 を Visual Studio を再起動します。

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Cshtml ファイル withBrowse WithorF5causes サーバー エラーを表示します。

-を示すエラーが表示されます、MVC 5 プロジェクトを Visual Studio 2012 (または Visual Studio 2013 で作成された Visual Studio 2012、MVC 5 プロジェクトで開く) で作成 cshtml ファイルを参照または f5 キーを使用して表示しようとすると、**でサーバー エラー'/' アプリケーション**します。 サーバーに移動しようとしました。 `http://localhost:XXXX/Views/../XXXX.cshtml`

この問題を解決するには、変更、**開始動作**、プロジェクトに設定**特定のページ**。 ページの値を指定する必要はありません。

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

この変更を加えたら、f5 キーを選択すると、アプリケーションのルートに移動します (`http://localhost:XXXX`)。 この動作は、Visual Studio 2013 での MVC 5 プロジェクトの動作と同じ場所、**現在のページ**設定は、開いているページを起動します。

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Url の書き換えと Tilde(~)

ASP.NET Razor 3 または ASP.NET MVC 5 にアップグレードすると、tilde(~) 表記法が正しく動作しなく URL の書き換えを使用している場合。 URL 書き換えの HTML 要素の tilde(~) 表記はなどに影響&lt;A/&gt;、&lt;スクリプト/&gt;、&lt;リンク/&gt;、し、その結果、チルダが不要になったルート ディレクトリにマップします。

要求を書き換える場合など、 **asp.net/content**に**asp.net**、href 属性に&lt;A href ="~/content/"/&gt;に解決される **/content/コンテンツ/** の代わりに **/** します。 この変更を抑制するのに設定することができます、 **IIS\_WasUrlRewritten**を各 Web ページまたは false にコンテキスト**アプリケーション\_BeginRequest** Global.asax でします。

<a id="templateissue"></a>
### <a name="templates"></a>テンプレート

作成すると ASP.NET MVC プロジェクトでは、Windows 8.1 または Windows Server 2012 R2 の Visual Studio の Visual Studio 2012 には、"Configuring Web [url] の ASP.NET 4.5 に失敗しました。"を示すエラー メッセージが表示されます。

![構成エラー](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Visual Studio 2012 ではこれらのリリースの Windows をインストールしていることを ASP.NET 4.5 機能有効になりませんので、このエラーが表示されます。 ASP.NET 4.5 を有効にする」に記載の手順に従います[オンまたはオフにする Windows 機能](https://windows.microsoft.com/windows-8/turn-windows-features-on-off)します。

![Windows の機能を無効を切り替える](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

または、コマンドラインを使用して ASP.NET 4.5 を有効にできます。

1. 管理者モードでコマンド プロンプトを開きます。
2. ASP.NET 4.5 を有効にするのには、次のコマンドを実行します。  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
