---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: "ASP.NET および Web ツール 2013.1 for Visual Studio 2012 のリリース ノート |Microsoft ドキュメント"
author: microsoft
description: "このドキュメントでは、Visual Studio 2012 用の ASP.NET および Web ツール 2013.1 のリリースについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>ASP.NET および Web ツール 2013.1 for Visual Studio 2012 のリリース ノート
====================
によって[Microsoft](https://github.com/microsoft)

> このドキュメントでは、Visual Studio 2012 用の ASP.NET および Web ツール 2013.1 のリリースについて説明します。


## <a name="contents"></a>目次

- [インストールに関する注意事項](#install)
- [ソフトウェアの要件](#requirements)
- ASP.NET および Web ツール 2013.1 for Visual Studio 2012 の新機能

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

        - [MVC と Web API のスキャフォールディング - HTTP 404 Not Found エラー](#404issue)
        - [Visual Studio Express 2012 for Web がスキャフォールディングされた項目を追加した後の動作を停止します。](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [サーバー エラーが発生する参照または f5 キーを持つ cshtml ファイルを表示します。](#browseissue)
        - [Url 書き換えと Tilde(~)](#rewriteissue)
    - [テンプレート](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>インストールに関する注意事項

[インストール](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids)ASP.NET および Web Tools for Visual Studio 2012 2013.1 です。

<a id="requirements"></a>
## <a name="software-requirements"></a>ソフトウェア要件

Visual Studio 2012 または Visual Studio Express 2012 for Web のいずれかが必要です。

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>ASP.NET および Web ツール 2013.1 for Visual Studio 2012 の新機能

<a id="bootstrap"></a>
### <a name="bootstrap"></a>ブートス トラップ

MVC 5 コント ローラーとビューをスキャフォールディングする際、ビューのマークアップを使用して[ブートス トラップ](http://getbootstrap.com/)です。

<a id="templates"></a>
### <a name="templates"></a>テンプレート

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>ASP.NET MVC 5 テンプレート

新しい MVC 5 のテンプレートが追加されました。 最新の MVC 5 の NuGet パッケージが参照され、スキャフォールディングを使用して、コント ローラーとビューを追加することができます。

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>ASP.NET Web API 2 のテンプレート

新しい Web API 2 のテンプレートが追加されました。 最新の Web API 2 の NuGet パッケージが参照され、スキャフォールディングを使用して、コント ローラーとビューを追加することができます。

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>項目テンプレート

MVC 5 ビュー、Web ページ (Razor 3)、および Web API 2 コント ローラー用の新しい項目テンプレートが追加されました。 プロジェクトに新しい項目の追加中に NuGet パッケージの関連する、インストールされます。

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework を使用して、MVC または Web API のコント ローラーをスキャフォールディングする際、Framework 6 を使用します。 Entity Framework の詳細については、次を参照してください。、 [Entity Framework のバージョン履歴](https://msdn.com/data/jj574253)です。

ダウンロードして、Visual Studio 2012 用の Entity Framework 6 のツールをインストールすることができます。 参照してください、 [Entity Framework の入手](https://msdn.com/data/ee712906#tooling)です。

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET のスキャフォールディング

ASP.NET のスキャフォールディングとは、ASP.NET Web アプリケーションのコード生成フレームワークです。 により、簡単にデータ モデルと連携するプロジェクトに定型コードを追加します。

Visual Studio の以前のバージョンでは、スキャフォールディングは、ASP.NET MVC プロジェクトに制限されていました。 この更新で、Web フォームを含む、すべての ASP.NET プロジェクトのスキャフォールディングを使用できます。 この更新プログラムが、Web フォーム プロジェクトの生成のページをサポートしていませんが、プロジェクトに MVC 依存関係を追加することで、Web フォームのスキャフォールディングを使用することもできます。 Web フォームのページを生成するためのサポートは、今後の更新で追加されます。

スキャフォールディングを使用する場合は、必要なすべての依存関係がプロジェクトにインストールされていることを確認します。 たとえば、ASP.NET Web フォーム プロジェクトを開始して、スキャフォールディングを使用して Web API コント ローラーを追加した場合、必要な NuGet パッケージと参照をプロジェクトに自動的に追加されます。

MVC のスキャフォールディングを Web フォーム プロジェクトに追加するには、追加、**スキャフォールディングされた新しい項目**選択**MVC 5 の依存関係**ダイアログ ウィンドウでします。 MVC; スキャフォールディングの 2 つのオプションがあります。最小限かつ完全します。 最低限を選択した場合は、NuGet パッケージの管理と ASP.NET MVC の参照のみがプロジェクトに追加されます。 [完全] オプションを選択した場合、MVC プロジェクトの必要なコンテンツ ファイルだけでなく、最小の依存関係が追加されます。

非同期コント ローラーをスキャフォールディングのサポートは、Entity Framework 6 から新しい非同期機能を使用します。

詳細な情報およびチュートリアルについては、次を参照してください。 [ASP.NET スキャフォールディング概要](../2013/aspnet-scaffolding-overview.md)です。 これらのチュートリアルを Visual Studio 2013、スキャフォールディングを表示することも ASP.NET および Web ツール 2013.1 for Visual Studio 2012 に適用します。

<a id="razor"></a>
### <a name="razor-editor"></a>Razor エディター

この更新で、Visual Studio 2012 今すぐ Razor 3 ツール/が編集をサポートします。

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 にはで詳細に説明される新機能の豊富なセットが含まれます[NuGet 2.7 リリース ノート](http://docs.nuget.org/docs/release-notes/nuget-2.7)です。

このバージョンの NuGet では、不足しているパッケージを復元する NuGet を明示的に許可するユーザーの必要性を削除します。 NuGet 2.7、ユーザーを暗黙的にインストールするときに自動的に足りないパッケージを復元することに同意します。 ユーザーは、Visual Studio で NuGet 設定によるパッケージの復元から明示的に選択できます。 この変更は、パッケージの復元の動作方法を簡略化します。

## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET のスキャフォールディング

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC と Web API のスキャフォールディング - HTTP 404 Not Found エラー

スキャフォールディングされた項目をプロジェクトに追加するときにエラーが発生した場合、プロジェクトは不整合な状態になることができます。 いくつかのスキャフォールディングをで行われた変更はロールバックされますが、インストールされている NuGet パッケージなど、他の変更はロールバックされません。 ルーティングの構成の変更がロールバックされた場合、ユーザーはスキャフォールディングされた項目に移動するときに HTTP 404 エラーが表示されます。

MVC のこのエラーを解決する新しいスキャフォールディングされた項目を追加し、MVC 5 の依存関係の選択 (最小または完全のいずれか)。 このプロセスでは、プロジェクトに追加のすべての必要な変更します。

Web api には、このエラーを解決します。

1. 次の WebApiConfig クラスをプロジェクトに追加します。

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. アプリケーションで WebApiConfig.Register を構成\_Global.asax で次のようにメソッドを開始します。

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 for Web がスキャフォールディングされた項目を追加した後の動作を停止します。

Entity Framework (Web API 2 コント ローラーなどのアクション、Entity Framework を使用) とスキャフォールディング項目の追加後に Visual Studio Express 2012 for Web が動作しなくなった場合は Visual Studio Express は、アセンブリのネイティブ イメージの読み込みに失敗したこと可能性System.Web.Extensions に依存します。

この問題を解決するには、System.Web.Extensions の MSIL のイメージを使用する Visual Studio Express を構成します。

1. 管理者モードでコマンド プロンプトを開きます。
2. %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE または %programfiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (64 ビット Windows) 用に移動します。
3. VWDExpress.exe.config をテキスト エディターで開きます。
4. 次の行を追加、&lt;構成&gt;/&lt;ランタイム&gt;要素。  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Visual Studio の再起動は Express 2012 for Web です。

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Cshtml ファイル withBrowse WithorF5causes サーバー エラーを表示します。

Visual Studio 2012 (または Visual Studio 2012 MVC 5 の場合、作成されたプロジェクトを Visual Studio 2013 で開きます) で MVC 5 のプロジェクトを作成し、Browse With または f5 キーを使用して、cshtml ファイルを表示しようとすると、ときにエラーが発生状態 - を**でサーバー エラー'/' アプリケーション**です。 サーバーに移動しようとしました。`http://localhost:XXXX/Views/../XXXX.cshtml`

この問題を解決するには、変更、**開始動作**に、プロジェクトで設定**特定のページ**です。 ページの値を指定する必要はありません。

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

この変更を加えたら、f5 キーを選択すると、アプリケーションのルートに移動した (`http://localhost:XXXX`)。 この動作は Visual Studio 2013 での MVC 5 プロジェクトの動作と同じ場所、**現在のページ**設定が開いているページを起動します。

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Url 書き換えと Tilde(~)

ASP.NET Razor 3 または ASP.NET MVC 5 にアップグレードすると、tilde(~) 表記が正しく動作しなく URL 書き換えを使用している場合。 URL 書き換えなど HTML 要素の tilde(~) 表記に影響&lt;A/&gt;、&lt;スクリプト/&gt;、&lt;リンク/&gt;、され、その結果、チルダが不要になったルート ディレクトリにマップします。

たとえば、要求を書き直してください**asp.net/content**に**asp.net**、href 属性に&lt;A href ="~/content/"/&gt;に解決される**/content/コンテンツ/**の代わりに **/**です。 この変更を抑制するには設定、 **IIS\_WasUrlRewritten**を各 Web ページまたは false コンテキスト**アプリケーション\_BeginRequest** Global.asax でします。

<a id="templateissue"></a>
### <a name="templates"></a>テンプレート

"構成 Web [url] の ASP.NET 4.5 に失敗しました"を示すエラー メッセージを表示するときに ASP.NET MVC プロジェクトを作成するには、Windows 8.1 または Windows Server 2012 R2、Visual Studio の Visual Studio 2012 で

![構成エラー](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

これらのリリースの Windows にインストールされているときに、Visual Studio 2012 が ASP.NET 4.5 機能を有効いないために、このエラーが表示されます。 ASP.NET 4.5 を有効にするに記載の手順を実行[Windows の機能のオンまたはオフ](https://windows.microsoft.com/windows-8/turn-windows-features-on-off)です。

![Windows の機能の無効を切り替える](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

また、コマンドラインを使用して ASP.NET 4.5 を有効にできます。

1. 管理者モードでコマンド プロンプトを開きます。
2. ASP.NET 4.5 を有効にするのには、次のコマンドを実行します。  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
