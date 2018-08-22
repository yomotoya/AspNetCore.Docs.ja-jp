---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET および Web Tools for Visual Studio 2013 リリース ノート |Microsoft Docs
author: microsoft
description: このドキュメントでは、ASP.NET および Web Tools for Visual Studio 2013 のリリースについて説明します。
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 44ab88b61a96235da27ff41d6b649bfd7fce3e38
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823903"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET および Web Tools for Visual Studio 2013 リリース ノート
====================
によって[Microsoft](https://github.com/microsoft)

> このドキュメントでは、ASP.NET および Web Tools for Visual Studio 2013 のリリースについて説明します。


## <a name="contents"></a>目次

- [インストールに関する注記](#TOC1)
- [ドキュメント](#TOC2)
- [ソフトウェアの要件](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>ASP.NET および Web Tools for Visual Studio 2013 の新機能

- [1 つの ASP.NET](#TOC6)
- [新しい Web プロジェクト エクスペリエンス](#newproj)
- [ASP.NET のスキャフォールディング](#scaffold)
- [ブラウザー リンク](#browser-link)
- [Visual Studio Web エディターの機能強化](#web-editor)
- [Visual Studio での azure App Service Web アプリのサポート](#waws)
- [Web の発行の機能強化](#publish)
- [NuGet 2.7](#nuget)
- [ASP.NET Web フォーム](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Microsoft OWIN コンポーネント](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [ASP.NET アプリを中断します。](#TOC15)
- [既知の問題と重大な変更](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>インストールに関する注記

ASP.NET および Web Tools for Visual Studio 2013 でメインのインストーラーにバンドルされて、ダウンロードできます[ここ](https://www.asp.net/downloads)します。

<a id="TOC2"></a>
## <a name="documentation"></a>ドキュメント

チュートリアルおよび Visual Studio 2013 の ASP.NET と Web ツールの詳細については、その他の情報は、 [ASP.NET web サイト](https://www.asp.net/)します。

<a id="TOC4"></a>
## <a name="software-requirements"></a>ソフトウェア要件

ASP.NET と Web ツール Visual Studio 2013 が必要です。

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>ASP.NET および Web Tools for Visual Studio 2013 の新機能

次のセクションでは、リリースで導入された機能について説明します。

<a id="TOC6"></a>
## <a name="one-aspnet"></a>1 つの ASP.NET

Visual Studio 2013 のリリースでは、ASP.NET のテクノロジを使用することができますを混在させるし、必要なものと同じに簡単に使用できるエクスペリエンスを統合するための手段を思い出させています。 たとえば、MVC を使用してプロジェクトを開始し簡単に後で、プロジェクトに Web フォーム ページを追加したり、Web フォーム プロジェクトで Web Api をスキャフォールディングできます。 1 つの ASP.NET はしやすく開発者は ASP.NET で使い慣れた、ことを行います。 どのようなテクノロジを選択に関係なく 1 つの ASP.NET の信頼された、基になる framework でビルドする信頼ことができます。

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>新しい Web プロジェクト エクスペリエンス

Visual Studio 2013 で新しい web プロジェクトの作成の強化されています。 **新しい ASP.NET Web プロジェクト**ダイアログし、テクノロジ (Web フォーム、MVC、Web API) の任意の組み合わせを構成、認証のオプションを構成して、単体テスト プロジェクトを追加、プロジェクトの種類を選択することができます。

![新しい ASP.NET プロジェクト](release-notes/_static/image1.png)

新しいダイアログでは、テンプレートの多くの既定の認証オプションを変更することができます。 たとえば、ASP.NET Web フォーム プロジェクトを作成するときに、次のオプションのいずれかを選択できます。

- 認証なし
- 個々 のユーザー アカウント (ASP.NET メンバーシップまたはソーシャル プロバイダー ログ)
- 組織アカウント (インターネット アプリケーションでは Active Directory)
- Windows 認証 (イントラネット アプリケーションで Active Directory)

![認証オプション](release-notes/_static/image2.png)

Web プロジェクトを作成するための新しいプロセスの詳細については、次を参照してください。 [Visual Studio 2013 で ASP.NET Web プロジェクトの作成](creating-web-projects-in-visual-studio.md)です。 新しい認証オプションの詳細については、次を参照してください。 [ASP.NET Identity](#TOC8)このドキュメントで後述します。

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET のスキャフォールディング

ASP.NET のスキャフォールディングは、ASP.NET Web アプリケーションのコード生成フレームワークです。 これにより、簡単にデータ モデルを操作するプロジェクトに定型コードを追加できます。

Visual Studio の以前のバージョンでは、スキャフォールディングは、ASP.NET MVC プロジェクトに制限されていました。 Visual Studio 2013 では、Web フォームを含む、任意の ASP.NET プロジェクトのスキャフォールディングを使用できます。 Visual Studio 2013 はサポートされていないページを生成する Web フォーム プロジェクトの場合は、MVC 依存関係をプロジェクトに追加することで、Web フォームでスキャフォールディングを使用することもできます。 Web フォームのページを生成するためのサポートは今後の更新で追加されます。

スキャフォールディングを使用して、必要なすべての依存関係がプロジェクトにインストールされていることができます。 たとえば、ASP.NET Web フォーム プロジェクトを開始し、スキャフォールディングを使用して Web API コント ローラーを追加すると場合、必要な NuGet パッケージと参照をプロジェクトに自動的に追加されます。

Web フォーム プロジェクトには、MVC のスキャフォールディングを追加するには、追加、**スキャフォールディングされた新しい項目**選択**MVC 5 依存関係**ダイアログ ウィンドウでします。 MVC; をスキャフォールディングするための 2 つのオプションがあります。最小限かつ完全です。 最低限を選択した場合は、NuGet パッケージと ASP.NET MVC の参照のみがプロジェクトに追加されます。 すべてのオプションを選択した場合、MVC プロジェクトに必要なコンテンツ ファイルと、最小の依存関係が追加されます。

非同期コント ローラーをスキャフォールディングするためのサポートは、Entity Framework 6 からの新しい非同期機能を使用します。

詳細とチュートリアルについては、次を参照してください。 [ASP.NET スキャフォールディング概要](aspnet-scaffolding-overview.md)します。

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>ブラウザー リンク – ブラウザーと Visual Studio の間の SignalR チャネル

新しい[Browser Link](using-browser-link.md)機能では、Visual Studio に複数のブラウザーを接続し、ツールバーのボタンをクリックしてすべての更新することができます。 モバイル エミュレーターの場合を含め、開発サイトに複数のブラウザーを接続でき、同時にすべての更新、更新をすべてのブラウザーをクリックします。 ブラウザー リンクには、ブラウザー リンク拡張機能を記述する開発者を有効にするための API も公開します。

![](release-notes/_static/image3.png)

ブラウザー リンク API を活用するために開発者を有効にするには、Visual Studio と接続されている任意のブラウザー間の境界を越える非常に高度なシナリオを作成することなります。 Web Essentials では、Visual Studio とブラウザーの開発者ツール、リモート モバイル エミュレーターと、はるかに多くの制御の統合されたエクスペリエンスを作成する API を利用します。

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web エディターの機能強化

Visual Studio 2013 には、web アプリケーションでの Razor ファイルや HTML ファイルに新しい HTML エディターが含まれています。 新しい HTML エディターは、HTML5 に基づいて 1 つの統一されたスキーマを提供します。 自動かっこ挿入、jQuery UI および IntelliSense の属性、属性 IntelliSense のグループ化、ID とクラス名、Intellisense、書式設定のパフォーマンスを高めるための他の強化、AngularJS とスマート タグ。

次のスクリーン ショットでは、HTML エディターでのブートス トラップ属性 IntelliSense の使用方法を示します。

![HTML エディターの Intellisense](release-notes/_static/image4.png)

Visual Studio 2013 には、両方 CoffeeScript エディターが組み込まれているとも付属しています。 LESS エディター、CSS エディターからすべての優れた機能が付属し、変数、mixin の特定の Intellisense を少ないすべてのドキュメントでは、@importチェーン。

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Visual Studio での azure App Service Web アプリのサポート

Azure SDK for .NET 2.2 で Visual Studio 2013 で使用できます**サーバー エクスプ ローラー**リモートの web アプリと直接対話します。 Azure アカウントにサインインして新しい web アプリを作成、アプリを構成するか、リアルタイムのログと詳細を表示できます。 SDK 2.2 後すぐに予定されているがリリースされたら、Azure でリモートでデバッグ モードで実行することができます。 Azure App Service Web Apps の新しい機能のほとんどは、現在のリリースの Azure SDK for .NET をインストールすると、Visual Studio 2012 で動作も。

詳細については、次のリソースを参照してください。

- [Azure App Service で ASP.NET web アプリを作成します。](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Visual Studio を使用した Azure App Service での Web アプリのトラブルシューティング](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Web の発行の機能強化

Visual Studio 2013 には、新規および強化された Web の発行の機能が含まれています。 その一部を次に示します。

- 簡単に[Web.config ファイルの暗号化を自動化](https://go.microsoft.com/fwlink/?LinkId=325529)します。 (このリンクし、次の 2 つ) をポイント 10/17 日の終わりまで使用できない可能性がある MSDN のドキュメント。
- 簡単に[デプロイ中にオフライン アプリケーションの自動化](https://go.microsoft.com/fwlink/?LinkId=325530)します。
- Web デプロイを構成する[ファイルのチェックサムを使用して、最終変更日ではなく](https://go.microsoft.com/fwlink/?LinkId=325531)ファイルは、サーバーにコピーするかを決定するため。
- すばやく、FTP を使用しているか、ファイル システム メソッドを公開するときにだけでなく Web 配置には、個々 の選択したファイル (Web.config を含む) を発行します。

ASP.NET web 配置の詳細については、次を参照してください。 [ASP.NET サイト](https://go.microsoft.com/fwlink/?LinkId=322027)します。

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 にはで詳細に説明されている新機能の豊富なセットが含まれます[NuGet 2.7 リリース ノート](http://docs.nuget.org/docs/release-notes/nuget-2.7)します。

このバージョンの NuGet では、NuGet のパッケージ復元機能パッケージをダウンロードするための明示的な同意を提供する必要性も削除されます。 NuGet をインストールすることで同意 (と NuGet の設定のダイアログ ボックスで、対応するチェック ボックス) が付与されました。 今すぐパッケージの復元は、既定では単に動作します。

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web フォーム

### <a name="one-aspnet"></a>1 つの ASP.NET

Web フォーム プロジェクト テンプレートは、1 つの ASP.NET の新しいエクスペリエンスとシームレスに統合します。 MVC と Web API のサポート、Web フォーム プロジェクトを 1 つの ASP.NET プロジェクトの作成ウィザードを使用して認証を構成することができますを追加することができます。 詳細については、次を参照してください。 [Visual Studio 2013 で ASP.NET Web プロジェクトの作成](creating-web-projects-in-visual-studio.md)です。

### <a name="aspnet-identity"></a>ASP.NET Identity

Web フォーム プロジェクト テンプレートでは、新しい ASP.NET Identity フレームワークをサポートします。 さらに、テンプレートは、イントラネットの Web フォーム プロジェクトの作成をサポートするようになりました。 詳細については、次を参照してください。[認証方法](creating-web-projects-in-visual-studio.md#auth)で**Visual Studio 2013 で ASP.NET Web プロジェクトの作成**です。

### <a name="bootstrap"></a>ブートス トラップ

Web フォーム テンプレートを使用して、[ブートス トラップ](http://twitter.github.io/bootstrap/)や応答性のスマートなルック アンド フィールを簡単にカスタマイズできるを提供します。 詳細については、次を参照してください。 [Visual Studio 2013 web プロジェクト テンプレートを使用すると、ブートス トラップ](creating-web-projects-in-visual-studio.md#bootstrap)します。

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>1 つの ASP.NET

Web MVC プロジェクト テンプレートは、1 つの ASP.NET の新しいエクスペリエンスとシームレスに統合します。 MVC プロジェクトをカスタマイズし、1 つの ASP.NET プロジェクトの作成ウィザードを使用して認証を構成します。 ASP.NET MVC 5 の入門のチュートリアルをご覧[ASP.NET MVC 5 の概要](../../../mvc/overview/getting-started/introduction/getting-started.md)します。

MVC 5、MVC 4 プロジェクトをアップグレードする方法については、次を参照してください。 [ASP.NET MVC 5 と Web API 2 に、ASP.NET MVC 4 と Web API プロジェクトをアップグレードする方法](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)します。

### <a name="aspnet-identity"></a>ASP.NET Identity

ASP.NET Identity を使用して認証と id 管理には、MVC プロジェクト テンプレートが更新されました。 Facebook や Google の認証と新しいメンバーシップ API を備えたチュートリアルをご覧[Facebook と Google の OAuth2 や OpenID サインオンでの ASP.NET MVC 5 アプリケーションの作成](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)と[認証を使用した ASP.NET MVC アプリを作成し、SQL DB と Azure App Service へのデプロイ](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)します。

### <a name="bootstrap"></a>ブートス トラップ

使用する MVC プロジェクト テンプレートが更新されました[ブートス トラップ](http://getbootstrap.com/)や応答性のスマートなルック アンド フィールを簡単にカスタマイズできるを提供します。 詳細については、次を参照してください。 [Visual Studio 2013 web プロジェクト テンプレートを使用すると、ブートス トラップ](creating-web-projects-in-visual-studio.md#bootstrap)します。

### <a name="authentication-filters"></a>認証フィルター

認証フィルターは、新しい種類のフィルターを ASP.NET MVC パイプライン内の承認フィルターの前に実行し、認証ロジックごとのアクションを指定することは ASP.NET MVC でコント ローラーごとまたはすべてのコント ローラーに対してグローバルにします。 認証フィルターは、要求に資格情報を処理し、対応するプリンシパルを指定します。 認証フィルターは、未承認の要求に応答認証チャレンジを追加することもできます。

### <a name="filter-overrides"></a>フィルターのオーバーライド

オーバーライド フィルターを指定することによって、特定のアクション メソッドまたはコント ローラーに適用するフィルターをオーバーライドすることができますようになりました。 オーバーライド フィルターは、特定のスコープ (アクションまたはコント ローラー) で実行するかのフィルターの種類のセットを指定します。 これにより、グローバルに適用されますが、特定のアクションまたはコント ローラーに適用することから特定のグローバル フィルターを除外するフィルターを構成することができます。

### <a name="attribute-routing"></a>属性ルーティング

ASP.NET MVC が Tim McCall の作成者によって投稿物に協力してくれた、属性のルーティングをサポートして今すぐ[ http://attributerouting.net](http://attributerouting.net)します。 属性ルーティングでは、アクションとコント ローラーに注釈を付けることによって、ルートを指定できます。

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>属性ルーティング

ASP.NET Web API が Tim McCall の作成者によって投稿物に協力してくれた、属性のルーティングをサポートして今すぐ[ http://attributerouting.net](http://attributerouting.net)します。 属性ルーティングでこのようなコント ローラー、アクションに注釈を付けることによって、Web API ルートを指定できます。

[!code-csharp[Main](release-notes/samples/sample1.cs)]

属性ルーティングでは、Uri の制御、web API でします。 たとえば、リソース階層が単一の API コント ローラーを使用して簡単に定義できます。

[!code-csharp[Main](release-notes/samples/sample2.cs)]

属性ルーティングも省略可能なパラメーター、既定値、およびルート制約を指定する便利な構文を提供します。

[!code-csharp[Main](release-notes/samples/sample3.cs)]

属性ルーティングの詳細については、次を参照してください。[属性は、Web API 2 でルーティング](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md)します。

### <a name="oauth-20"></a>OAuth 2.0

Web API と Single Page Application プロジェクト テンプレートでは、OAuth 2.0 を使用して認証できるようになりました。 OAuth 2.0 は、保護されたリソースへのクライアント アクセスを承認するためのフレームワークです。 クライアントのブラウザーやモバイル デバイスなどのさまざまな動作します。

OAuth 2.0 のサポートは、新しいセキュリティ ミドルウェア ベアラー認証のため、Microsoft OWIN コンポーネントによって提供され、承認サーバーの役割の実装に基づいています。 また、クライアントは、Azure Active Directory または Windows Server 2012 R2 で ADFS などの組織の承認サーバーを使用して承認できます。

### <a name="odata-improvements"></a>OData の機能強化

**$Batch と $value、$select、$ のサポートを展開します。**

ASP.NET Web API OData の $select を完全にサポートになりました、$ の展開、$value します。 バッチ処理と変更セットの処理要求の $batch を使用することもできます。

$Select および $expand オプションを展開し、OData エンドポイントから返されるデータの形状を変更するようにします。 詳細については、次を参照してください。 [Introducing $select および $expand は、Web API odata のサポートを拡張](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md)します。

**強化された機能拡張**

OData フォーマッタは、拡張可能なようになりました。 Atom エントリのメタデータを追加して名前付きストリームとメディア リンク エントリをサポートして、インスタンス注釈を追加するか、およびリンクの生成方法をカスタマイズできます。

**型のないサポート**

エンティティ型の CLR 型を定義することがなく OData サービスを構築できます。 代わりに、OData コント ローラーがかかることができますかのインスタンスを返す**IEdmObject**、どの OData フォーマッタは、シリアル化/逆シリアル化します。

**既存のモデルを再利用します。**

既存 entity data model (EDM) を既にある場合するようになりました再利用できる、直接新しいゲートウェイを作成する代わりにします。 たとえば、Entity Framework を使用している場合は、EF によって構築する EDM を使用できます。

### <a name="request-batching"></a>バッチ処理を要求します。

要求がバッチ処理を少ない頻度の高いユーザー インターフェイスを滑らかにネットワーク トラフィックを削減し、1 つの HTTP POST 要求に複数の操作を結合します。 ASP.NET Web API では、要求がバッチ処理のいくつかの方法をサポートしているようになりました。

- OData サービスの $batch エンドポイントを使用します。
- 1 つの MIME マルチパート要求に複数の要求をパッケージ化します。
- バッチ処理のカスタム書式指定を使用します。

要求のバッチ処理を有効にするには、だけの Web API 構成にバッチ処理のハンドラーを持つルートを追加します。

[!code-csharp[Main](release-notes/samples/sample4.cs)]

制御することもできるかどうかを要求または順番に、または任意の順序で実行します。

### <a name="portable-aspnet-web-api-client"></a>移植可能な ASP.NET Web API クライアント

ASP.NET Web API のクライアントを使用して、Windows ストアおよび Windows Phone 8 アプリケーション間で動作するポータブル クラス ライブラリを作成することができますようになりました。 クライアントとサーバー間で共有できる移植可能なフォーマッタを作成することもできます。

### <a name="improved-testability"></a>強化されたテストの容易性

Web API 2 で、API コント ローラーをテスト ユニットに簡単になります。 だけで、要求メッセージと構成では、API コント ローラーをインスタンス化してテストするアクション メソッドを呼び出します。 簡単にたとえるなら、 **UrlHelper**のリンクの生成を実行するアクション メソッドのクラス。

### <a name="ihttpactionresult"></a>IHttpActionResult

今すぐ、Web API アクション メソッドの結果をカプセル化する IHttpActionResult を実装できます。 Web API アクション メソッドから返される、IHttpActionResult は、結果の応答メッセージを生成するために、ASP.NET Web API ランタイムによって実行されます。 単位を簡略化する Web API アクションから返される、IHttpActionResult、Web API の実装をテストします。 参考までに特定のステータス コードを返すため結果を含むさまざまな IHttpActionResult 実装が提供されている、コンテンツまたはコンテンツ ネゴシエーションの応答を書式設定します。

### <a name="httprequestcontext"></a>HttpRequestContext

新しい**HttpRequestContext**は要求に関連付けられていますが、要求からすぐにご利用いただけません状態を追跡します。 たとえば、使用することができます、 **HttpRequestContext**ルート データ要求、クライアント証明書に関連付けられたプリンシパルを取得する、 **UrlHelper**と仮想パスのルート。 簡単に作成することができます、 **HttpRequestContext**単体テストのためです。

要求のプリンシパルが使用する代わりに、要求と共に送られるため、 **Thread.CurrentPrincipal**、Web API パイプラインでは、プリンシパルに、要求の有効期間全体で使用できるがようになりました。

### <a name="cors"></a>CORS

Brock Allen から別の優れた投稿に協力してくれた ASP.NET 完全サポート クロス オリジン要求の共有 (CORS) します。

ブラウザーのセキュリティは、Web ページが別のドメインに AJAX 要求を行うことを防止します。 [CORS](http://www.w3.org/TR/cors/) W3C 標準で、サーバーによる同一オリジン ポリシーの緩和にできるです。 CORS を使用することによって、不明なリクエストは拒否しながら、一部のクロス オリジン要求のみを明示的に許可できるようになります。

プレフライト要求の自動処理を含む CORS web API 2 をサポートします。 詳細については、次を参照してください。 [ASP.NET Web API でのクロス オリジン要求を有効にする](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md)します。

### <a name="authentication-filters"></a>認証フィルター

認証フィルターは、新しい ASP.NET Web API を ASP.NET Web API パイプラインで承認フィルターの前に実行し、認証ロジックごとのアクションを指定することでフィルターの種類をコント ローラーごとまたはすべてのコント ローラーに対してグローバルにします。 認証フィルターは、要求に資格情報を処理し、対応するプリンシパルを指定します。 認証フィルターは、未承認の要求に応答認証チャレンジを追加することもできます。

### <a name="filter-overrides"></a>フィルターのオーバーライド

オーバーライド フィルターを指定することによって、特定のアクション メソッドまたはコント ローラーに適用するフィルターをオーバーライドすることができますようになりました。 オーバーライド フィルターは、特定のスコープ (アクションまたはコント ローラー) で実行しないでくださいフィルターの種類のセットを指定します。 これにより、グローバル フィルターを追加し、特定のアクションまたはコント ローラーから一部を除外することができます。

### <a name="owin-integration"></a>OWIN の統合

ASP.NET Web API 今すぐ完全 OWIN をサポートの OWIN 対応のホストで実行することができます。 含まれていますが、 **HostAuthenticationFilter** OWIN 認証システムとの統合を提供します。

OWIN の統合、SignalR など、他の OWIN ミドルウェアと共に独自のプロセスで Web API を自己ホストできます。 詳細については、次を参照してください。 [Self-Host ASP.NET Web API を使用して OWIN](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)します。

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

次のセクションでは、SignalR 2.0 の機能について説明します。

- [OWIN 上に構築されました。](#builtonowin)
- [MapHubs および MapConnection が MapSignalR](#MapSignalR)
- [クロス ドメインのサポート](#crossdomain)
- [MonoTouch と MonoDroid による iOS および Android をサポートします。](#mobile)
- [ポータブル .NET クライアント](#portable)
- [新しいパッケージを自己ホスト](#selfhost)
- [旧バージョンと互換性のあるサーバーのサポート](#backwardcompat)
- [.NET 4.0 用のサーバーのサポートが削除されました](#remove40)
- [クライアントとグループの一覧にメッセージを送信します。](#messagelist)
- [特定のユーザーにメッセージを送信します。](#sendtouser)
- [優れたエラー処理のサポート](#errorhandling)
- [簡単に単体テストのハブ](#unittesting)
- [JavaScript のエラー処理](#javascripterror)

SignalR 2.0 に既存の 1.x プロジェクトをアップグレードする方法の例は、次を参照してください。[アップグレード、SignalR 1.x プロジェクト](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md)します。

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>OWIN 上に構築されました。

SignalR 2.0 に完全に構築された[OWIN (.NET の Open Web Interface)](http://owin.org/)します。 この変更は、SignalR のセットアップ プロセスは、web ホストおよび自己ホスト型の SignalR アプリケーション間でより一貫性がさまざまな API の変更点にも必要です。

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs および MapConnection が MapSignalR

OWIN 規格と互換性のため、これらのメソッドが変更されています`MapSignalR`します。 `MapSignalR` パラメーターはすべてのハブをマップせずに呼び出されます (として`MapHubs`はバージョン 1.x) は個別にマップする**PersistentConnection**オブジェクト、型のパラメーターとして、接続 URL の拡張機能として、接続の種類を指定する、最初の引数。

`MapSignalR` Owin startup クラスでメソッドが呼び出されます。 Visual Studio 2013 には、Owin スタートアップ クラスの新しいテンプレートが含まれていますこのテンプレートを使用するには、次の操作を行います。

1. プロジェクトを右クリックします。
2. 選択**追加**、**新しい項目.**
3. 選択**Owin Startup クラス**します。 新しいクラスの名前**Startup.cs**します。

**Web アプリケーション、** Owin スタートアップ クラスを含む、`MapSignalR`次に示すメソッドは、Web.Config ファイルのアプリケーション設定 ノード内のエントリを使用して、Owin のスタートアップ プロセスに追加されます。

**アプリケーションを自己ホスト型**、Startup クラスがの型パラメーターとして渡される、`WebApp.Start`メソッド。

**ハブと SignalR の接続のマッピング (web アプリケーションのグローバルなアプリケーション ファイル) から 1.x:** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**ハブと SignalR 2.0 (Owin Startup クラス ファイルの場合) からの接続をマッピングします。** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

**アプリケーションを自己ホスト型**、Startup クラスがの型パラメーターとして渡される、`WebApp.Start`メソッドは、次のようです。

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>クロス ドメインのサポート

SignalR では 1.x では、クロス ドメイン要求が 1 つの EnableCrossDomain フラグによって制御されます。 このフラグ JSONP と CORS の両方の要求を制御します。 すべての CORS をサポートする柔軟性を高め、SignalR のサーバー コンポーネントから削除されました (JavaScript lients まだ CORS 通常、ブラウザーがサポートすることが検出された場合) を使用し、新しい OWIN ミドルウェアをこれらのシナリオをサポートするために使用しました。

SignalR 2.0 では、クライアントの場合の JSONP を必須 (古いブラウザーでは、クロス ドメイン要求をサポート) を設定を明示的に有効にする必要があります`EnableJSONP`上、`HubConfiguration`オブジェクトを`true`以下に示すようにします。 CORS より安全であるために、既定では、JSONP が無効です。

SignalR 2.0 では、新しい CORS ミドルウェアを追加するには、追加、`Microsoft.Owin.Cors`呼び出しをプロジェクトにライブラリ`UseCors`SignalR ミドルウェアは、以下のセクションで示すようにする前にします。

**Microsoft.Owin.Cors をプロジェクトに追加する**: このライブラリをインストールするには、パッケージ マネージャー コンソールで、次のコマンドを実行します。

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

このコマンドは、2.0.0 を追加、プロジェクトにパッケージのバージョン。

**UseCors を呼び出す**

次のコード スニペットは、SignalR のドメイン間の接続を実装する方法を示します 1.x と 2.0。

**SignalR でクロス ドメイン要求を実装する (グローバル アプリケーション ファイル) から 1.x**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**SignalR 2.0 (c# コード ファイル) からのクロス ドメイン要求を実装します。**

次のコードでは、SignalR 2.0 プロジェクトで CORS または JSONP を有効にする方法を示します。 このコード サンプルを使用して`Map`と`RunSignalR`の代わりに`MapSignalR`、CORS ミドルウェアは、CORS のサポートを必要とする SignalR 要求に対してのみが実行されるように、(で指定されたパスにすべてのトラフィックではなく`MapSignalR`)。`Map`アプリケーション全体に対してではなく、特定の URL プレフィックスでは、実行する必要があるその他のミドルウェアにも使用できます。

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>MonoTouch と MonoDroid による iOS および Android をサポートします。

IOS と Android のクライアントから MonoTouch と MonoDroid コンポーネントを使用してサポートが追加されました、 [Xamarin ライブラリ](https://xamarin.com/)します。 それらを使用する方法の詳細については、次を参照してください。 [Xamarin コンポーネントを使用して](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln)します。 これらのコンポーネントがで提供されます、 [Xamarin Store](https://store.xamarin.com/) SignalR RTW リリースが使用可能な場合。

<a id="portable"></a> ### ポータブル .NET クライアント

Silverlight を WinRT のクロス プラットフォーム開発を容易にし、Windows Phone クライアント、次のプラットフォームをサポートする 1 つのポータブル .NET クライアントに置き換えられました。

- NET 4.5
- Silverlight 5
- WinRT (Windows ストア アプリ用 .NET)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>新しいパッケージを自己ホスト

SignalR セルフホスト (プロセスでホストされている SignalR アプリケーションまたは他のアプリケーションではなく、web サーバーでホストされている) を使用しやすく NuGet パッケージはようになりました。 SignalR を使ってビルドされた自己ホスト プロジェクトをアップグレードする 1.x では、Microsoft.AspNet.SignalR.Owin のパッケージを削除し、Microsoft.AspNet.SignalR.SelfHost パッケージを追加します。 自己ホスト パッケージの概要については、次を参照してください。[チュートリアル: SignalR セルフホスト](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)します。

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>旧バージョンと互換性のあるサーバーのサポート

以前のバージョンの SignalR、SignalR パッケージがクライアントで使用されると同一であるために必要なサーバーのバージョン。 シック クライアント アプリケーションを更新することは困難をサポートするために SignalR 2.0 サポート古いクライアントで新しいサーバー バージョンを使用します。 **注: SignalR 2.0 は新しいクライアントと以前のバージョンで構築されたサーバーをサポートしていません。**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>.NET 4.0 用のサーバーのサポートが削除されました

SignalR 2.0 では、.NET 4.0 でのサーバーの相互運用性のサポートを削除します。 SignalR 2.0 サーバーでは、.NET 4.5 を使用する必要があります。 SignalR 2.0 for .NET 4.0 クライアントは引き続きします。

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>クライアントとグループの一覧にメッセージを送信します。

SignalR 2.0 では、クライアントとグループ Id の一覧を使用してメッセージを送信することはできます。 次のコード スニペットでは、これを行う方法を示します。

**クライアントと PersistentConnection を使用してグループの一覧にメッセージを送信します。**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**クライアントとハブを使用してグループの一覧にメッセージを送信します。**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>特定のユーザーにメッセージを送信します。

この機能により、ユーザー Id を指定するユーザー IUserIdProvider 新しいインターフェイスを使用して、IRequest に基づいて。

**IUserIdProvider インターフェイス**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

既定では、ユーザー名とユーザーの IPrincipal.Identity.Name を使用する実装があります。

ハブで、新しい API を使用してこれらのユーザーにメッセージを送信するようになります。

**Clients.User API を使用します。**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>優れたエラー処理のサポート

ユーザーがスローできるようになりました**HubException**任意のハブ呼び出しから。 コンス トラクター、 **HubException**文字列メッセージとオブジェクトにかかる追加のエラー データ。 SignalR は、例外を自動シリアル化し、拒否/失敗のハブ メソッドの呼び出しに使用されますをクライアントに送信します。

**ハブの詳細な例外表示**設定に影響していない**HubException**ことはありません。 または、クライアントに送信されることは常に送信します。

**サーバー側コードのデモを HubException をクライアントに送信します。**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**JavaScript クライアント コードが、サーバーから送信された HubException への応答のデモ**

[!code-html[Main](release-notes/samples/sample16.html)]

**.NET クライアントをサーバーから送信された HubException への応答を示すコード**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>簡単に単体テストのハブ

SignalR 2.0 にはというインターフェイスが含まれています`IHubCallerConnectionContext`hubs モックのクライアント側の呼び出しの作成が簡単です。 人気のあるテスト ハーネスでのこのインターフェイスの使用方法を示す次のコード スニペット[xUnit.net](https://github.com/xunit/xunit)と[moq](https://code.google.com/p/moq/)します。

**単体テストの xUnit.net で SignalR**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**単体テストの moq と SignalR**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>JavaScript のエラー処理

SignalR 2.0 では、JavaScript エラー処理のすべてのコールバックは、未加工の文字列ではなく JavaScript エラー オブジェクトを返します。 これによりより豊富な情報、エラー ハンドラーにフローする SignalR ができます。 内部例外を取得することができます、`source`エラーのプロパティ。

**Start.Fail 例外を処理する JavaScript クライアント コード**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>新しい ASP.NET メンバーシップ システム

ASP.NET Identity は、ASP.NET アプリケーションの新しいメンバーシップ システムです。 ASP.NET Identity を簡単にアプリケーション データとユーザー固有のプロファイル データを統合します。 ASP.NET Identity では、アプリケーションでユーザー プロファイルの永続化モデルを選択することもできます。 SQL Server データベースまたは Azure Storage のテーブルなどの NoSQL データ ストアなど、別のデータ ストアにデータを保存することができます。 詳細については、次を参照してください。[個々 のユーザー アカウント](creating-web-projects-in-visual-studio.md#indauth)で**Visual Studio 2013 で ASP.NET Web プロジェクトの作成**です。

### <a name="claims-based-authentication"></a>クレーム ベースの認証

ASP.NET では、ユーザーの id が信頼された発行者からのクレームのセットとして表されるクレーム ベース認証できるようになりました。 ユーザーは認証されたユーザー名と、アプリケーション データベースに保存されるパスワードを使用してまたはソーシャル id プロバイダーを使用できます (例: Microsoft アカウント、Facebook、Google、Twitter)、または Azure Active Directory を介して組織のアカウントを使用して、またはActive Directory Federation Services (ADFS)。

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Azure Active Directory および Windows Server Active Directory との統合

Azure Active Directory または Windows Server ・ Active Directory (AD) 認証を使用する ASP.NET プロジェクトを作成できます。 詳細については、次を参照してください。[組織アカウント](creating-web-projects-in-visual-studio.md#orgauth)で**Visual Studio 2013 で ASP.NET Web プロジェクトの作成**です。

### <a name="owin-integration"></a>OWIN の統合

ASP.NET 認証は、任意の OWIN ベースのホストで使用できる、OWIN ミドルウェアに基づいています。 OWIN の詳細については、次を参照してください。 [Microsoft OWIN コンポーネント](#TOC7)セクション。

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Microsoft OWIN コンポーネント

[.NET 用 Web インターフェイスを開き](http://owin.org/)(OWIN) .NET web サーバーおよび web アプリケーション間の抽象化を定義します。 OWIN は、web アプリケーション ホストに依存しないのため、サーバーから web アプリケーションを分離します。 たとえば、IIS の OWIN ベースの web アプリケーションをホストしたり、カスタム プロセスでセルフホストできます。

Microsoft OWIN コンポーネント (Katana プロジェクトとも呼ばれます) で導入された変更には、新しいサーバーとホストのコンポーネント、新しいヘルパー ライブラリとミドルウェア、および新しい認証ミドルウェアが含まれます。

OWIN と Katana の詳細については、次を参照してください。 [OWIN と Katana の新](../../../aspnet/overview/owin-and-katana/index.md)します。

**注: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) IIS クラシック モードでアプリケーションを実行できません。 これらは、統合モードで実行する必要があります。**

**注: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)完全信頼でアプリケーションを実行する必要があります。**

### <a name="new-servers-and-hosts"></a>新しいサーバーとホスト

このリリースでは、新しいコンポーネントは、自己ホスト シナリオを有効に追加されました。 これらのコンポーネントには、次の NuGet パッケージが含まれます。

- **Microsoft.Owin.Host.HttpListener**します。 使用する OWIN サーバーは、 **HttpListener**を HTTP 要求をリッスンし、OWIN パイプラインに指示します。
- **Microsoft.Owin.Hosting**にコンソール アプリケーションまたは Windows サービスなどのカスタムのプロセスで OWIN パイプラインをセルフホストする開発者のライブラリを提供します。
- **OwinHost**します。 ラップするスタンドアロン実行可能ファイルを提供します。`Microsoft.Owin.Hosting`し、カスタム ホスト アプリケーションを記述することなく、OWIN パイプラインをセルフホストすることができます。

さらに、`Microsoft.Owin.Host.SystemWeb`パッケージには、ミドルウェアにヒントを提供するようになりましたができるように、 **SystemWeb**サーバー、特定の ASP.NET パイプライン ステージの間に、ミドルウェアを呼び出す必要があることを示します。 この機能は、認証ミドルウェアは、ASP.NET パイプラインの早い段階で実行する必要がありますに特に便利です。

### <a name="helper-libraries-and-middleware"></a>ヘルパー ライブラリとミドルウェア

OWIN 仕様から関数と型の定義のみを使用して、OWIN コンポーネントを記述できますが、新しい`Microsoft.Owin`パッケージはユーザーにわかりやすい一連の抽象化を提供します。 このパッケージは、いくつかの以前のパッケージを結合 (例: `Owin.Extensions`、 `Owin.Types`) し、簡単に使用できるその他の OWIN コンポーネントで適切に構成された 1 つのオブジェクト モデルにします。 実際には、Microsoft OWIN コンポーネントの大半は、このパッケージを使用するようになりました。

> [!NOTE]
> [OWIN](http://www.owin.org) IIS クラシック モードでアプリケーションを実行できません。 これらは、統合モードで実行する必要があります。

> [!NOTE]
> [OWIN](http://www.owin.org)完全信頼でアプリケーションを実行する必要があります。

このリリースには、ミドルウェアを実行中の OWIN アプリケーションを検証するとエラーの調査に役立つエラー ページ ミドルウェアを含む Microsoft.Owin.Diagnostics パッケージも含まれています。

### <a name="authentication-components"></a>認証コンポーネント

次の認証コンポーネントは使用できます。

- **Microsoft.Owin.Security.ActiveDirectory**します。 オンプレミスまたはクラウド ベースのディレクトリ サービスを使用して認証を有効にします。
- **Microsoft.Owin.Security.Cookies** cookie を使用して認証を有効にします。 このパッケージが以前という名前だった`Microsoft.Owin.Security.Forms`します。
- **Microsoft.Owin.Security.Facebook** Facebook の OAuth ベースのサービスを使用して認証を有効にします。
- **Microsoft.Owin.Security.Google** Google の OpenID ベースのサービスを使用して認証を有効にします。
- **Microsoft.Owin.Security.Jwt** JWT トークンを使用して認証を有効にします。
- **Microsoft.Owin.Security.MicrosoftAccount** Microsoft アカウントを使用して認証を有効にします。
- **Microsoft.Owin.Security.OAuth**します。 ベアラー トークンを認証するためには、OAuth 承認サーバーだけでなく、ミドルウェアを提供します。
- **Microsoft.Owin.Security.Twitter** Twitter の OAuth ベースのサービスを使用して認証を有効にします。

このリリースでは、`Microsoft.Owin.Cors`パッケージで、クロスオリジン HTTP 要求を処理するためのミドルウェアが含まれています。

> [!NOTE]
> JWT に署名するためのサポートは Visual Studio 2013 の最終バージョンで削除されました。

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

新機能および Entity Framework 6 の他の変更の一覧は、次を参照してください。 [Entity Framework のバージョン履歴](https://msdn.com/data/jj574253)します。

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 には、次の新機能が含まれています。

- タブの編集をサポートします。 Preivously、**ドキュメントのフォーマット**コマンド、自動インデント、および自動 Visual Studio で書式設定が正しく動作しなかったを使用する場合、**タブの保持**オプション。 この変更は、Visual Studio の書式設定 タブの Razor コードの書式設定を修正します。
- リンクを生成するときに URL 書き換えルールをサポートします。
- セキュリティ透過的な属性の削除。
  > [!NOTE]
  > これは重大な変更し、Razor 3 互換性のない MVC4、以前のバージョンと Razor 2 は MVC5 または MVC5 に対してコンパイルされたアセンブリとの互換性はありません。

プレリリース版から Visual Studio 2013 で修正された razor 3 問題はあります[ここ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0)します。

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.NET アプリを中断します。

ASP.NET アプリの中断は、ユーザー エクスペリエンスと多数の 1 台のコンピューター上の ASP.NET のサイトをホストするための経済的なモデルを大幅に変更する .NET Framework 4.5.1 における革新的機能です。 詳細については、次を参照してください。 [ASP.NET App Suspend – 応答性の高い共有 .NET web ホスティング](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx)します。

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

このセクションでは、Visual Studio 2013 の既知の問題と、ASP.NET と Web ツールにおける重大な変更について説明します。

### <a name="nuget"></a>NuGet

- [SLN ファイルを使用する場合、Mono で新しいパッケージの復元が機能しません](https://nuget.codeplex.com/workitem/3596)– 今後 nuget.exe をダウンロードで修正される予定と[NuGet.CommandLine パッケージ](http://www.nuget.org/packages/NuGet.CommandLine/)を更新します。
- [Wix プロジェクトで新しいパッケージの復元は機能しません](https://nuget.codeplex.com/workitem/3598)– 今後 nuget.exe をダウンロードで修正される予定と[NuGet.CommandLine パッケージ](http://www.nuget.org/packages/NuGet.CommandLine/)を更新します。
- [自動パッケージの復元では、ソリューション フォルダーの下のプロジェクトが機能しません](https://nuget.codeplex.com/workitem/3625)– NuGet 2.8 で修正される予定です。

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` 返さない`IQueryable<T>`常に、サポートが追加されました`$select`と`$expand`します。

    以前のサンプル`ODataQueryOptions<T>`から戻り値を常にキャストされた`ApplyTo`に`IQueryable<T>`します。 以前このため、クエリ オプションを以前方法でサポート (`$filter`、 `$orderby`、 `$skip`、 `$top`)、クエリの形状を変更しないでください。 これでサポートされています`$select`と`$expand`からの戻り値`ApplyTo`されません`IQueryable<T>`常にします。

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    前のサンプル コードを使用している場合、引き続き動作、クライアントが送信されなかった場合`$select`と`$expand`します。 ただし、サポートする場合`$select`と`$expand`にそのコードを変更する必要があります。

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url または RequestContext.Url が null のバッチ要求中に**

    バッチ処理のシナリオで**UrlHelper**がからアクセスする場合は null **Request.Url**または**RequestContext.Url**します。

    この問題はここで現在追跡: [BatchRequestContext.Url が要求をバッチ処理の null](http://aspnetwebstack.codeplex.com/workitem/1301)します。

    この問題の回避の新しいインスタンスを作成するには**UrlHelper**、次の例。

    **UrlHelper の新しいインスタンスを作成します。**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. MVC5 と OrgAuth を使用しているビュー AntiForgerToken 検証を行うことがある場合は、使用している場合、ビューにデータを投稿すると、次のエラーを遭遇する可能性があります。

    **エラー**:

    *'/' アプリケーションでは、サーバー エラーです。*

    <em>種類の要求 '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'または'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' が指定した ClaimsIdentity に存在しませんでした。クレーム ベース認証を使用した偽造防止トークンのサポートを有効にするには、構成されている要求プロバイダーが提供している生成 ClaimsIdentity インスタンスでこれらの要求の両方をくださいを確認します。代わりに、構成されている要求プロバイダーは、一意の識別子としてさまざまなクレームの種類を使用する場合は、AntiForgeryConfig.UniqueClaimTypeIdentifier 静的プロパティを設定して構成できます。</em>

    **回避策**:

    その修正 Global.asax で、次の行を追加します。

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    これについては、次のリリースを修正します。
2. MVC5 に MVC4 アプリケーションをアップグレードした後は、ソリューションをビルドし、これを起動します。 次のエラーが表示されます。

    [A][B]System.Web.WebPages.Razor.Configuration.HostSection に System.Web.WebPages.Razor.Configuration.HostSection をキャストすることはできません。 タイプ A 'System.Web.WebPages.Razor、バージョン 2.0.0.0、カルチャを = = neutral, PublicKeyToken = 31bf3856ad364e35' 場所' Default' のコンテキストで ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'。 タイプ b 'System.Web.WebPages.Razor、バージョン 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 を =' 場所' Default' のコンテキストで ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'。

    上記のエラーを修正するには、開きます*すべて*プロジェクトとは、次の Web.config ファイル (Views フォルダーにあるものを含む)。

   1. 「5.0.0.0」の"System.Web.Mvc"のバージョン「4.0.0.0」のすべての出現箇所を更新します。
   2. バージョン「2.0.0.0」を"System.Web.Helpers"の出現をすべて更新&quot;System.Web.WebPages&quot;と&quot;System.Web.WebPages.Razor&quot; 「3.0.0.0」を

      たとえば、上記の変更を行った後にこのようアセンブリ バインドになります。

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      MVC 5、MVC 4 プロジェクトをアップグレードする方法については、次を参照してください。 [ASP.NET MVC 5 と Web API 2 に、ASP.NET MVC 4 と Web API プロジェクトをアップグレードする方法](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)します。
3. Jquery Unobtrusive Validation をクライアント側の検証を使用する場合、検証メッセージが正しくない場合があります型の HTML input 要素を = 'number' です。 検証エラーが必須の値 (「[Age] フィールドが必要です」) が表示されます、正しいメッセージの有効な数値が必要であるのではなく無効な数値を入力する場合。

    Create と Edit ビューで整数のプロパティを持つモデルのこの問題が検出スキャフォールディングされたコードで一般的です。

    この問題を回避するからエディター ヘルパーを変更します。

    `@Html.EditorFor(person => person.Age)`

    移動先:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 には、部分信頼がサポートされていません。 MVC または web Api のバイナリにリンクするプロジェクトを削除する必要があります、 [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx)属性と[AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx)属性。 これらの属性を削除するには、次のようにコンパイラ エラーがなくなります。

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > ただし、このことの副作用は、同じアプリケーションで 4.0、5.0 のアセンブリを使用することはできません。 5.0 にそれらのすべてを更新する必要があります。

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Facebook の承認を持つ SPA テンプレート、web サイトがイントラネット ゾーンでホストされている IE で不安定になる可能性があります。

SPA テンプレートは、外部で Facebook ログインを提供します。 テンプレートで作成されたプロジェクトはローカルで実行してサインイン発生に IE がクラッシュします。

解決方法 : 

1. インターネット ゾーンでの web サイトのホストまたは

2. IE 以外のブラウザーでは、シナリオをテストします。

### <a name="web-forms-scaffolding"></a>Web フォームのスキャフォールディング

Web フォームのスキャフォールディングでは、VS2013 からが削除され、Visual Studio に対する今後の更新で提供されます。 ただし、MVC 依存関係を追加して、MVC のスキャフォールディングを生成する、Web フォーム プロジェクト内でスキャフォールディングを引き続き使用することができます。 プロジェクトは、Web フォームと MVC の組み合わせが格納されます。

MVC は、Web フォーム プロジェクトに追加するに新しくスキャフォールディングした項目を追加および選択**MVC 5 依存関係**します。 最小またはスクリプトなどのコンテンツ ファイルのすべてを必要とするかどうかに応じて完全のいずれかを選択します。 次に、プロジェクト内のビューとコント ローラーを作成する MVC のスキャフォールディングされた項目を追加します。

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC と Web API スキャフォールティング - HTTP 404 Not Found エラー

スキャフォールディングされた項目をプロジェクトに追加するときにエラーが発生した場合は、プロジェクトは、不整合な状態になることが勧めします。 一部のスキャフォールディングが行われた変更はロールバックされますが、インストール済みの NuGet パッケージなどの他の変更はロールバックされません。 ルーティング構成の変更がロールバックされた場合、ユーザーはスキャフォールディングされた項目に移動するときに、HTTP 404 エラーを受信します。

回避策:

- MVC のこのエラーを修正する新しくスキャフォールディングした項目を追加し、MVC 5 依存関係を選択します (最小または完全のいずれか)。 このプロセスでは、プロジェクトに追加のすべての必要な変更します。
- Web api には、このエラーを解決します。

  1. プロジェクトには、WebApiConfig クラスを追加します。

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. WebApiConfig.Register をアプリケーションで構成\_Global.asax でメソッドを次のように開始します。

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
