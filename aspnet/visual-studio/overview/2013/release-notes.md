---
uid: visual-studio/overview/2013/release-notes
title: "ASP.NET および Web Tools for Visual Studio 2013 のリリース ノート |Microsoft ドキュメント"
author: microsoft
description: "このドキュメントでは、ASP.NET および Web ツールの Visual Studio 2013 のリリースについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 10835c39d3bca752ed3068a23fecaaab56449e41
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET および Web Tools for Visual Studio 2013 のリリース ノート
====================
によって[Microsoft](https://github.com/microsoft)

> このドキュメントでは、ASP.NET および Web ツールの Visual Studio 2013 のリリースについて説明します。


## <a name="contents"></a>目次

- [インストールに関する注意事項](#TOC1)
- [ドキュメント](#TOC2)
- [ソフトウェアの要件](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>ASP.NET および Web Tools for Visual Studio 2013 の新機能

- [1 つの ASP.NET](#TOC6)
- [新しい Web プロジェクトのエクスペリエンス](#newproj)
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
- [ASP.NET Id](#TOC8)
- [Microsoft OWIN コンポーネント](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [ASP.NET アプリを中断します。](#TOC15)
- [既知の問題と重大な変更](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>インストールに関する注意事項

ASP.NET および Visual Studio 2013 用 Web ツールがメインのインストーラーにバンドルおよびダウンロードできます[ここ](https://www.asp.net/downloads)です。

<a id="TOC2"></a>
## <a name="documentation"></a>ドキュメント

チュートリアルおよび Visual Studio 2013 用 ASP.NET および Web ツールに関するその他の情報は、 [ASP.NET web サイト](https://www.asp.net/)です。

<a id="TOC4"></a>
## <a name="software-requirements"></a>ソフトウェア要件

ASP.NET および Web ツール Visual Studio 2013 が必要です。

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>ASP.NET および Web Tools for Visual Studio 2013 の新機能

次のセクションでは、リリースで導入された機能について説明します。

<a id="TOC6"></a>
## <a name="one-aspnet"></a>1 つの ASP.NET

Visual Studio 2013 のリリースで混在したりしたい場合に一致を簡単にできるように、ASP.NET テクノロジを使用する場合の動作を統合するための手段に移動しました。 たとえば、MVC を使用して、プロジェクトを開始および簡単に後で、プロジェクトに Web フォーム ページを追加したり、Web フォーム プロジェクトでの Web Api のスキャフォールディングです。 1 つの ASP.NET はやすくするための ASP.NET でお使いの場合、操作を実行する開発者です。 選択するどのようなテクノロジに関係なく、1 つの ASP.NET の信頼されている、基になる framework でビルドする信頼を持つことができます。

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>新しい Web プロジェクトのエクスペリエンス

Visual Studio 2013 で新しい web プロジェクトを作成する場合の動作を拡張しています。 **新しい ASP.NET Web プロジェクト**ダイアログ テクノロジ (Web フォーム、MVC、Web API) の任意の組み合わせを構成する、認証オプションを構成し、単体テスト プロジェクトを追加、プロジェクトの種類を選択することができます。

![新しい ASP.NET プロジェクト](release-notes/_static/image1.png)

新しいダイアログ ボックスでは、多くのテンプレートの既定の認証オプションを変更することができます。 たとえば、ASP.NET Web フォーム プロジェクトを作成するときに、次のオプションのいずれかを選択できます。

- 認証なし
- 個々 のユーザー アカウント (ASP.NET メンバーシップまたはのソーシャル プロバイダー ログ)
- 組織アカウント (インターネット アプリケーションでは Active Directory)
- Windows 認証 (イントラネット アプリケーションの Active Directory)

![認証オプション](release-notes/_static/image2.png)

Web プロジェクトを作成するための新しいプロセスの詳細については、次を参照してください。 [Visual Studio 2013 で ASP.NET Web プロジェクトの作成](creating-web-projects-in-visual-studio.md)です。 新しい認証オプションの詳細については、次を参照してください。 [ASP.NET Identity](#TOC8)このドキュメントで後述します。

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET のスキャフォールディング

ASP.NET のスキャフォールディングとは、ASP.NET Web アプリケーションのコード生成フレームワークです。 により、簡単にデータ モデルと連携するプロジェクトに定型コードを追加します。

Visual Studio の以前のバージョンでは、スキャフォールディングは、ASP.NET MVC プロジェクトに制限されていました。 Visual Studio 2013 で Web フォームを含む、すべての ASP.NET プロジェクトのスキャフォールディングを使用できます。 Visual Studio 2013 はサポートされていません生成のページは、Web フォーム プロジェクトがプロジェクトに MVC 依存関係を追加することで、Web フォームのスキャフォールディングを使用することもできます。 Web フォームのページを生成するためのサポートは、今後の更新で追加されます。

スキャフォールディングを使用する場合は、必要なすべての依存関係がプロジェクトにインストールされていることを確認します。 たとえば、ASP.NET Web フォーム プロジェクトを開始して、スキャフォールディングを使用して Web API コント ローラーを追加した場合、必要な NuGet パッケージと参照をプロジェクトに自動的に追加されます。

MVC のスキャフォールディングを Web フォーム プロジェクトに追加するには、追加、**スキャフォールディングされた新しい項目**選択**MVC 5 の依存関係**ダイアログ ウィンドウでします。 MVC; スキャフォールディングの 2 つのオプションがあります。最小限かつ完全します。 最低限を選択した場合は、NuGet パッケージの管理と ASP.NET MVC の参照のみがプロジェクトに追加されます。 [完全] オプションを選択した場合、MVC プロジェクトの必要なコンテンツ ファイルだけでなく、最小の依存関係が追加されます。

非同期コント ローラーをスキャフォールディングのサポートは、Entity Framework 6 から新しい非同期機能を使用します。

詳細な情報およびチュートリアルについては、次を参照してください。 [ASP.NET スキャフォールディング概要](aspnet-scaffolding-overview.md)です。

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>ブラウザー リンク: ブラウザーと Visual Studio の間の SignalR チャネル

新しい[Browser Link](using-browser-link.md)機能では、複数のブラウザーを Visual Studio に接続し、ツールバーのボタンをクリックするだけで更新したりすることができます。 モバイル エミュレーターを含め、開発サイトを複数のブラウザーを接続し、同時にすべて更新する更新のすべてのブラウザーをクリックできます。 Browser Link では、開発者がブラウザー リンク拡張を書き込むための API も公開します。

![](release-notes/_static/image3.png)

ブラウザー リンク API を利用する開発者を有効にするには、Visual Studio と接続されている任意のブラウザー間の境界を越える非常に高度なシナリオを作成することなります。 Web Essentials では、Visual Studio とブラウザーの開発ツール、リモートのモバイル エミュレーターと、はるかに多くの制御間統合エクスペリエンスを提供する API を活用します。

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web エディターの機能強化

Visual Studio 2013 には、web アプリケーションで Razor ファイルや HTML ファイルに新しい HTML エディターが含まれています。 新しい HTML エディターは、HTML5 に基づいて 1 つの統一されたスキーマを提供します。 自動の中かっこの補完、jQuery UI、および AngularJS IntelliSense の属性、属性 IntelliSense のグループ化、ID とクラス名、Intellisense、パフォーマンスの向上、書式設定を含むその他の改善があるとスマート タグです。

次のスクリーン ショットでは、HTML エディターのブートス トラップ属性 IntelliSense の使用方法を示します。

![HTML エディターの Intellisense](release-notes/_static/image4.png)

Visual Studio 2013 には、両方の CoffeeScript とに組み込まれているエディターも付属しています。 LESS エディター、CSS エディターから便利な機能がすべて付属しており mixins および変数の特定の Intellisense の少ないすべてのドキュメントに、@importチェーン。

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Visual Studio での azure App Service Web アプリのサポート

Azure SDK for .NET 2.2 で Visual Studio 2013 で使用できます**サーバー エクスプ ローラー**リモートの web アプリと直接通信します。 Azure アカウントにサインインすることができます新しい web アプリを作成する、アプリの構成、リアルタイムのログ、および詳細を表示します。 SDK 2.2 後すぐに聞こえるがリリースされたら、Azure でリモートでデバッグ モードで実行することができます。 Azure App Service Web Apps の新機能のほとんどは、現在のリリースの Azure SDK for .NET をインストールすると、Visual Studio 2012 で動作もします。

詳細については、次のリソースを参照してください。

- [Azure App service の ASP.NET web アプリを作成します。](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/)
- [Visual Studio を使用して Azure App service web アプリをトラブルシューティングします。](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Web の発行の機能強化

Visual Studio 2013 には、新機能および強化された Web 発行機能が含まれています。 次に、その一部を示します。

- 簡単に[Web.config ファイルの暗号化を自動化](https://go.microsoft.com/fwlink/?LinkId=325529)です。 (このリンクし、次の 2 つ) をポイント 10/17 の 1 日の終わりまで使用できない可能性がある MSDN のドキュメントです。
- 簡単に[配置時に、アプリケーションをオフラインの取得を自動化](https://go.microsoft.com/fwlink/?LinkId=325530)です。
- Web デプロイを構成する[最終変更日ではなくファイルのチェックサムを使用して](https://go.microsoft.com/fwlink/?LinkId=325531)ファイルは、サーバーにコピーするかを決定するためです。
- 迅速に FTP を使用しているか、ファイル システムのメソッドを公開するときにだけでなく Web 配置で個々 の選択したファイル (Web.config を含む) を発行します。

ASP.NET web 配置の詳細については、次を参照してください。 [、ASP.NET サイト](https://go.microsoft.com/fwlink/?LinkId=322027)です。

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 にはで詳細に説明される新機能の豊富なセットが含まれます[NuGet 2.7 リリース ノート](http://docs.nuget.org/docs/release-notes/nuget-2.7)です。

このバージョンの NuGet では、NuGet のパッケージ復元機能は、パッケージをダウンロードするための明示的な同意を提供する必要性も削除されます。 NuGet をインストールすることで同意 (NuGet の環境設定 ダイアログに関連付けられているチェック ボックスと) が付与されました。 ここでパッケージの復元は、既定では単に動作します。

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web フォーム

### <a name="one-aspnet"></a>1 つの ASP.NET

Web フォーム プロジェクト テンプレートは、新しい 1 つの ASP.NET エクスペリエンスとシームレスに統合します。 Web フォーム プロジェクトに MVC と Web API がサポートされており、1 つの ASP.NET プロジェクトの作成ウィザードを使用して認証を構成することができますを追加することができます。 詳細については、次を参照してください。 [Visual Studio 2013 で ASP.NET Web プロジェクトの作成](creating-web-projects-in-visual-studio.md)です。

### <a name="aspnet-identity"></a>ASP.NET Identity

Web フォーム プロジェクト テンプレートでは、新しい ASP.NET Identity framework をサポートします。 さらに、テンプレートは、Web フォーム イントラネット プロジェクトの作成をサポートするようになりました。 詳細については、次を参照してください。[認証方法](creating-web-projects-in-visual-studio.md#auth)で**Visual Studio 2013 で ASP.NET Web プロジェクトの作成**です。

### <a name="bootstrap"></a>ブートス トラップ

Web フォーム テンプレートを使用して[ブートス トラップ](http://twitter.github.io/bootstrap/)光沢のあるや応答性のルック アンド フィールを簡単にカスタマイズすることができますを提供します。 詳細については、次を参照してください。 [Visual Studio 2013 の web プロジェクト テンプレートでブートス トラップ](creating-web-projects-in-visual-studio.md#bootstrap)です。

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>1 つの ASP.NET

Web の MVC プロジェクト テンプレートは、新しい 1 つの ASP.NET エクスペリエンスとシームレスに統合します。 MVC プロジェクトをカスタマイズし、1 つの ASP.NET プロジェクトの作成ウィザードを使用して認証を構成することができます。 ASP.NET MVC 5 に入門チュートリアルはあります[ASP.NET MVC 5 の概要](../../../mvc/overview/getting-started/introduction/getting-started.md)です。

MVC 5 に MVC 4 プロジェクトをアップグレードする方法については、次を参照してください。 [ASP.NET MVC 5 と Web API 2 に、ASP.NET MVC 4 と Web API プロジェクトをアップグレードする方法](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)です。

### <a name="aspnet-identity"></a>ASP.NET Identity

ASP.NET の Id を使用して認証と id 管理には、MVC プロジェクト テンプレートが更新されました。 提供され、Facebook、Google の認証と新しいメンバーシップ API のチュートリアルが見つかります[Facebook、Google OAuth2 および OpenID サイン オンで、ASP.NET MVC 5 アプリを作成](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)と[認証で ASP.NET MVC アプリケーションを作成し、SQL DB し、Azure App Service に展開](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)です。

### <a name="bootstrap"></a>ブートス トラップ

使用する MVC プロジェクト テンプレートが更新されました[ブートス トラップ](http://getbootstrap.com/)光沢のあるや応答性のルック アンド フィールを簡単にカスタマイズすることができますを提供します。 詳細については、次を参照してください。 [Visual Studio 2013 の web プロジェクト テンプレートでブートス トラップ](creating-web-projects-in-visual-studio.md#bootstrap)です。

### <a name="authentication-filters"></a>認証フィルター

認証フィルターは、新しい ASP.NET MVC パイプラインでの承認フィルターの前に実行し、認証ロジックごとのアクションを指定することを ASP.NET MVC でのフィルターの種類ごとのコント ローラー、またはすべてのコント ローラーに対してグローバルにします。 認証フィルターは、要求に資格情報を処理し、対応するプリンシパルを提供します。 認証フィルターは、未承認の要求に応答認証チャレンジを追加することもできます。

### <a name="filter-overrides"></a>上書きをフィルター処理します。

オーバーライド フィルターを指定して特定のアクション メソッドまたはコント ローラーに適用するフィルターをオーバーライドすることができますようになりました。 オーバーライド フィルターは、特定のスコープ (アクションまたはコント ローラー) で実行しないでフィルターの種類のセットを指定します。 これにより、グローバルに適用が、特定のアクションまたはコント ローラーに適用されない特定のグローバル フィルターを除外するフィルターを構成することができます。

### <a name="attribute-routing"></a>属性のルーティング

ASP.NET MVC が Tim McCall の作成者によって貢献感謝の属性がルーティングをサポートして今すぐ[http://attributerouting.net](http://attributerouting.net)です。 属性のルーティングは、アクションとコント ローラーに注釈を付けることによって、ルートを指定できます。

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>属性のルーティング

ASP.NET Web API が Tim McCall の作成者によって貢献感謝の属性がルーティングをサポートして今すぐ[http://attributerouting.net](http://attributerouting.net)です。 属性のルーティングは、次のようにコント ローラー、アクションに注釈を付けるして、Web API ルートを指定できます。

[!code-csharp[Main](release-notes/samples/sample1.cs)]

属性がルーティングすること、Uri より詳細に制御での web API。 たとえば、単一の API コント ローラーを使用してリソースの階層を簡単に定義できます。

[!code-csharp[Main](release-notes/samples/sample2.cs)]

省略可能なパラメーター、既定値、およびルート制約を指定する便利な構文を提供する属性のルーティングも。

[!code-csharp[Main](release-notes/samples/sample3.cs)]

属性のルーティングの詳細については、次を参照してください。 [Web API 2 での属性のルーティング](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md)です。

### <a name="oauth-20"></a>OAuth 2.0

Web API およびシングル ページ アプリケーション プロジェクト テンプレートでは、OAuth 2.0 を使用して認証できるようになりました。 OAuth 2.0 は、保護されたリソースへのクライアント アクセスを承認するためのフレームワークです。 この機能は、さまざまなブラウザーやモバイル デバイスを含む、クライアントに対して機能します。

OAuth 2.0 のサポートは、新しいセキュリティ ミドルウェア ベアラ認証のため、Microsoft OWIN コンポーネントによって提供され、承認サーバーの役割の実装に基づいています。 また、クライアントは、Azure Active Directory または Windows Server 2012 R2 での ADFS など、組織の承認サーバーを使用して承認できます。

### <a name="odata-improvements"></a>OData の機能強化

**$Select、$ のサポートを展開し、$batch、$value**

ASP.NET Web API OData の $select を完全にサポートになりました、$ 順に展開し、$value です。 バッチ処理と変更セットの処理要求の $batch を使用することもできます。

$Select および $expand オプションを展開します。 OData エンドポイントから返されるデータの形状を変更することができます。 詳細については、次を参照してください。[概要 $select および $expand Web API OData のサポートの拡張](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md)です。

**強化された機能拡張**

OData フォーマッタは、拡張可能なようになりました。 Atom エントリ メタデータを追加して名前付きストリームとメディア リンク エントリのサポート、インスタンス注釈を追加するか、およびリンクを生成する方法をカスタマイズできます。

**型のないサポート**

CLR 型、エンティティ型を定義することがなく OData サービスを構築できます。 代わりに、OData コント ローラーはまたは実行のインスタンスを返す**IEdmObject**、どの OData フォーマッタは、シリアル化/逆シリアル化します。

**既存のモデルを再利用します。**

既存 entity data model (EDM) を既にある場合は、今すぐ再利用できることを直接、新しいものを作成する代わりにします。 たとえば、Entity Framework を使用している場合は、EDM の EF を構築するを使用できます。

### <a name="request-batching"></a>バッチ処理を要求します。

ネットワーク トラフィックを削減でき、少ない chatty なユーザー インターフェイス、滑らかに、1 つの HTTP POST 要求に複数の操作を結合する要求がバッチ処理します。 ASP.NET Web API では、要求がバッチ処理のいくつかの方法をサポートしているようになりました。

- OData サービスの $batch エンドポイントを使用します。
- 1 つの MIME マルチパート要求に複数の要求をパッケージ化します。
- バッチ処理のカスタム書式指定を使用します。

バッチ要求を有効にするには、Web API の構成にバッチ処理のハンドラーを持つルートを追加します。

[!code-csharp[Main](release-notes/samples/sample4.cs)]

制御することもできるかどうかを要求または順番に、または任意の順序で実行します。

### <a name="portable-aspnet-web-api-client"></a>ポータブル ASP.NET Web API のクライアント

Windows ストアおよび Windows Phone 8 アプリケーション間で動作するポータブル クラス ライブラリを作成する ASP.NET Web API のクライアントを使えるようになりました。 クライアントとサーバー間で共有できるポータブル フォーマッタを作成することもできます。

### <a name="improved-testability"></a>強化されたテストの容易性

Web API 2 は非常に簡単に単体テスト、API コント ローラー。 だけで、要求メッセージと構成 API コント ローラーのインスタンスを作成し、テストするアクション メソッドを呼び出します。 よくを模擬表示、 **UrlHelper**のリンクの生成を実行するアクション メソッドのクラスです。

### <a name="ihttpactionresult"></a>IHttpActionResult

今すぐ、Web API のアクション メソッドの結果をカプセル化する IHttpActionResult を実装できます。 Web API のアクション メソッドから返される、IHttpActionResult は、結果の応答メッセージを生成するために、ASP.NET Web API ランタイムによって実行されます。 単位を簡単に Web API 操作から返される、IHttpActionResult、Web API の実装をテストします。 都合のよいとき IHttpActionResult 実装の数が特定の状態コードを返すための結果を含むボックスから提供されたコンテンツまたはコンテンツ ネゴシエート応答形式になります。

### <a name="httprequestcontext"></a>HttpRequestContext

新しい**HttpRequestContext**要求に関連付けられていますが、すぐに利用できません要求から状態を追跡します。 たとえば、使用することができます、 **HttpRequestContext**ルート データ要求、クライアント証明書に関連付けられているプリンシパルを取得する、 **UrlHelper**と仮想パスのルートです。 簡単に作成することができます、 **HttpRequestContext**単体テストのためです。

要求のプリンシパルはではなく、要求に応じてフローため**Thread.CurrentPrincipal**プリンシパルが Web API パイプラインにある間、要求の有効期間全体で使用できる、します。

### <a name="cors"></a>CORS

Brock Allen から別の優れた投稿、感謝 ASP.NET 完全がサポートされましたクロス オリジン要求の共有 (CORS)。

ブラウザーのセキュリティは、web ページが別のドメインに AJAX 要求を行うことを防止します。 [CORS](http://www.w3.org/TR/cors/) W3C 標準により、同じオリジンのポリシーを緩和するサーバーです。 CORS を使用して、サーバー明示的に許可できますいくつかのクロス オリジン要求中に、他のユーザーを拒否します。

Web API 2 は、プレフライト要求の自動処理を含む、CORS をサポートしています。 詳細については、次を参照してください。 [ASP.NET Web API でのクロス オリジン要求の有効化](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md)です。

### <a name="authentication-filters"></a>認証フィルター

認証フィルターは、新しい ASP.NET Web API パイプラインでの承認フィルターの前に実行し、認証ロジックごとのアクションを指定することを ASP.NET Web API 内のフィルターの種類ごとのコント ローラー、またはすべてのコント ローラーに対してグローバルにします。 認証フィルターは、要求に資格情報を処理し、対応するプリンシパルを提供します。 認証フィルターは、未承認の要求に応答認証チャレンジを追加することもできます。

### <a name="filter-overrides"></a>上書きをフィルター処理します。

オーバーライド フィルターを指定して特定のアクション メソッドまたはコント ローラーに適用するフィルターをオーバーライドすることができますようになりました。 オーバーライド フィルターは、特定のスコープ (アクションまたはコント ローラー) で実行しないでフィルターの種類のセットを指定します。 これにより、グローバルのフィルターを追加するが、特定のアクションまたはコント ローラーから一部を除外することができます。

### <a name="owin-integration"></a>OWIN の統合

ASP.NET Web API 今すぐ完全 OWIN をサポート、OWIN 対応のホストで実行することができます。 含まれています、 **HostAuthenticationFilter** OWIN 認証システムとの統合を提供します。

OWIN の統合により、自己 SignalR など他の OWIN ミドルウェアと共にプロセスを独自の Web API をホストすることができます。 詳細については、次を参照してください。 [Self-Host ASP.NET Web API を使用する OWIN](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)です。

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

次のセクションでは、SignalR 2.0 の機能について説明します。

- [OWIN 上に構築されました。](#builtonowin)
- [MapHubs と MapConnection、MapSignalR](#MapSignalR)
- [クロス ドメインのサポート](#crossdomain)
- [iOS および Android MonoTouch および MonoDroid によるサポートします。](#mobile)
- [ポータブル .NET クライアント](#portable)
- [自己ホストの新しいパッケージ](#selfhost)
- [旧バージョンと互換性のあるサーバーのサポート](#backwardcompat)
- [.NET 4.0 をサーバー サポートを削除](#remove40)
- [クライアントとグループの一覧にメッセージを送信](#messagelist)
- [特定のユーザーにメッセージを送信します。](#sendtouser)
- [エラー処理サポートの向上](#errorhandling)
- [簡単に単体テストのハブ](#unittesting)
- [JavaScript のエラー処理](#javascripterror)

SignalR 2.0 を 1.x の既存のプロジェクトをアップグレードする方法の例は、次を参照してください。 [、SignalR のアップグレード 1.x プロジェクト](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md)です。

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>OWIN 上に構築されました。

SignalR 2.0 に完全に構築された[OWIN (.NET の Open Web Interface)](http://owin.org/)です。 この変更と、セットアップ プロセス SignalR web ホストおよび自己ホスト型の SignalR アプリケーション間でより一貫性が、API の変更の数も必要です。

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs と MapConnection、MapSignalR

OWIN 標準と互換性のため、これらのメソッドが名前に変更されました`MapSignalR`です。 `MapSignalR`パラメーターは、すべてのハブをマップすることがなく呼び出されます (として`MapHubs`はバージョン 1.x) 以外の場合は個別にマップする**PersistentConnection**オブジェクト、型パラメーターと、URL の拡張機能としての接続として接続の種類を指定する、最初の引数。

`MapSignalR` Owin スタートアップ クラスでメソッドが呼び出されます。 Visual Studio 2013 には、Owin スタートアップ クラスの新しいテンプレートが含まれていますこのテンプレートを使用するには、次の操作を行います。

1. プロジェクトを右クリックします。
2. 選択**追加**、**新しい項目の追加.**
3. 選択**Owin スタートアップ クラス**です。 新しいクラスの名前を**Startup.cs**です。

**Web アプリケーション、** Owin スタートアップ クラスを含む、`MapSignalR`次のように、メソッドは Owin の起動プロセスのエントリを使用して、Web.Config ファイルのアプリケーション設定 ノード内に追加されます。

**セルフホスト アプリケーション**、スタートアップ クラスは、の型パラメーターとして渡される、`WebApp.Start`メソッドです。

**ハブとの SignalR 接続とのマッピング (web アプリケーションのグローバルなアプリケーション ファイル) から 1.x:** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**ハブおよび SignalR 2.0 (Owin 起動クラス ファイルの場合) からの接続のマッピング。** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

**セルフホスト アプリケーション**、スタートアップ クラスは、の型パラメーターとして渡される、`WebApp.Start`メソッドを次のようにします。

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>クロス ドメインのサポート

SignalR で 1.x では、クロス ドメイン要求が単一 EnableCrossDomain フラグによって制御されます。 このフラグは、両方の JSONP および CORS 要求を制御します。 すべての CORS のサポートをより柔軟に行う、SignalR のサーバー コンポーネントから削除されました (JavaScript lients まだ CORS 通常、ブラウザーがサポートすることが検出された場合) を使用し、新しい OWIN ミドルウェアをこれらのシナリオをサポートするために使用されました。

SignalR 2.0 では JSONP の場合は、クライアントで (古いブラウザーでは、クロス ドメイン要求をサポート) で必要設定を明示的に有効にする必要があります`EnableJSONP`上、`HubConfiguration`オブジェクトを`true`、次のようにします。 CORS より安全であるために、既定では、JSONP が無効です。

SignalR 2.0 では、新しい CORS ミドルウェアを追加するには、追加、`Microsoft.Owin.Cors`ライブラリ プロジェクト、および呼び出しを`UseCors`SignalR ミドルウェアは、以下のセクションで示すようにする前にします。

**Microsoft.Owin.Cors をプロジェクトに追加する**: このライブラリをインストールするには、パッケージ マネージャー コンソールで、次のコマンドを実行します。

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

このコマンドは、2.0.0 を追加、プロジェクトにパッケージのバージョン。

**通話 UseCors**

次のコード スニペットは、SignalR にクロス ドメインの接続を実装する方法を示す 1.x および 2.0。

**SignalR でクロス ドメイン要求を実装する (グローバルなアプリケーション ファイル) から 1.x**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**SignalR 2.0 (c# コード ファイル) からのクロス ドメイン要求を実装します。**

次のコードでは、SignalR 2.0 プロジェクトで CORS または JSONP を有効にする方法を示します。 このコード サンプルでは使用`Map`と`RunSignalR`の代わりに`MapSignalR`CORS のサポートを必要とする SignalR 要求に対してのみ、CORS ミドルウェアが実行されるように、(で指定されたパスにすべてのトラフィックではなく`MapSignalR`)。`Map`アプリケーション全体に対してではなく、特定の URL プレフィックスを実行する必要があるその他のすべてのミドルウェアのも使用できます。

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS および Android MonoTouch および MonoDroid によるサポートします。

IOS および Android のクライアントから MonoTouch と MonoDroid のコンポーネントを使用してサポートが追加されて、 [Xamarin ライブラリ](https://xamarin.com/)です。 それらの使用方法の詳細については、次を参照してください。[を使用して Xamarin コンポーネント](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln)です。 これらのコンポーネントがで使用できる、 [Xamarin ストア](https://store.xamarin.com/)SignalR RTW リリースが使用可能な場合です。

<a id="portable"></a>### ポータブル .NET クライアント

効率的に Silverlight を WinRT、クロスプラット フォーム開発を容易にし、Windows Phone クライアントは、次のプラットフォームをサポートする 1 つのポータブル .NET クライアントに置き換えられています。

- NET 4.5
- Silverlight 5
- WinRT (Windows ストア アプリ用 .NET)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>自己ホストの新しいパッケージ

SignalR (プロセスでホストされている SignalR アプリケーションまたはその他のアプリケーションではなく、web サーバーでホストされている) が自己ホストを開始するが容易に NuGet パッケージはようになりました。 SignalR でビルドされた自己ホスト プロジェクトをアップグレードする 1.x では、Microsoft.AspNet.SignalR.Owin パッケージを削除して、Microsoft.AspNet.SignalR.SelfHost パッケージを追加します。 自己ホスト パッケージの概要の詳細については、次を参照してください。[チュートリアル: 自己ホスト SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)です。

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>旧バージョンと互換性のあるサーバーのサポート

以前のバージョンの SignalR、SignalR パッケージをクライアントで使用されると、一致させるために必要なサーバーのバージョン。 更新するが困難になるシック クライアント アプリケーションをサポートするために SignalR 2.0 サポート古いクライアントで新しいサーバー バージョンを使用します。 **注: SignalR 2.0 では比較的新しいクライアントで以前のバージョンでビルドされたサーバーはサポートされません。**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>.NET 4.0 をサーバー サポートを削除

SignalR 2.0 では、.NET 4.0 でのサーバーの相互運用性のサポートを削除します。 SignalR 2.0 サーバーでは、.NET 4.5 を使用する必要があります。 SignalR 2.0 用の .NET 4.0 クライアントは引き続きです。

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>クライアントとグループの一覧にメッセージを送信

SignalR 2.0 では、クライアントとグループ Id の一覧を使用してメッセージを送信することはできます。 次のコード スニペットでは、これを行う方法を示します。

**クライアントと PersistentConnection を使用して、グループの一覧にメッセージを送信**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**クライアントとハブを使用してグループの一覧にメッセージを送信**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>特定のユーザーにメッセージを送信します。

この機能により、ユーザー、ユーザー Id を指定する新しいインターフェイス IUserIdProvider を介して、IRequest に基づいて。

**IUserIdProvider インターフェイス**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

既定では、ユーザー名とユーザーの IPrincipal.Identity.Name を使用する実装があります。

ハブで、新しい API を使用してこれらのユーザーにメッセージを送信することができます。

**Clients.User API を使用します。**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>エラー処理サポートの向上

ユーザーがスローできるようになりました**HubException**任意ハブ呼び出しからです。 コンス トラクター、 **HubException**文字列メッセージおよびオブジェクトを受け取ってできる追加のエラー データ。 SignalR では、例外を自動でシリアル化し、拒否/失敗のハブ メソッド呼び出しに使用される場所をクライアントに送信します。

**詳細なハブの例外を表示する**設定に影響していない**HubException**ことはありません。 または、クライアントに送信されることが常に送信します。

**サーバー側コード、HubException をクライアントに送信中のデモンストレーション**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**JavaScript クライアント コードが、サーバーから送信された HubException への応答のデモンストレーション**

[!code-html[Main](release-notes/samples/sample16.html)]

**.NET クライアントをサーバーから送信された HubException への応答を示すコード**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>簡単に単体テストのハブ

SignalR 2.0 には、というインターフェイスが含まれています。`IHubCallerConnectionContext`モック クライアント側の呼び出しを作成するのが容易ハブにします。 次のコード スニペットは、このインターフェイスを使用して一般的なテスト ハーネスを示す[xUnit.net](https://github.com/xunit/xunit)と[moq](https://code.google.com/p/moq/)です。

**使用した単体テスト SignalR xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**使用した単体テスト SignalR moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>JavaScript のエラー処理

SignalR 2.0 では、すべての JavaScript エラー処理のコールバックは、未加工の文字列ではなく JavaScript エラー オブジェクトを返します。 これによりより詳細な情報、エラー ハンドラーにフローする SignalR できます。 内部例外を取得することができます、`source`エラーのプロパティです。

**Start.Fail 例外を処理する JavaScript クライアント コード**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>新規の ASP.NET メンバーシップ システム

ASP.NET Identity は、ASP.NET アプリケーションの新しいメンバーシップ システムです。 ASP.NET Identity しやすいアプリケーションのデータとユーザー固有のプロファイル データを統合します。 ASP.NET Id では、アプリケーションでユーザー プロファイルの永続化モデルを選択することもできます。 SQL Server データベースまたは Azure ストレージ テーブルなどの NoSQL データ ストアを含む別のデータ ストアでデータを保存することができます。 詳細については、次を参照してください。[個々 のユーザー アカウント](creating-web-projects-in-visual-studio.md#indauth)で**Visual Studio 2013 で ASP.NET Web プロジェクトの作成**です。

### <a name="claims-based-authentication"></a>クレーム ベースの認証

ASP.NET は、ユーザーの id が、信頼できる発行者からのクレームのセットとして表されるクレーム ベース認証をサポートします。 ユーザーが認証されたユーザー名とパスワード、アプリケーション データベースで保守管理を使用してまたはのソーシャル id プロバイダーを使用して指定できます (例: Microsoft アカウント、Facebook、Google、Twitter)、または Azure Active Directory を介して組織アカウントを使用して、またはActive Directory フェデレーション サービス (ADFS)。

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Azure Active Directory および Windows Server Active Directory との統合

認証を Azure Active Directory または Windows Server ・ Active Directory (AD) を使用する ASP.NET プロジェクトを作成できます。 詳細については、次を参照してください。[組織アカウント](creating-web-projects-in-visual-studio.md#orgauth)で**Visual Studio 2013 で ASP.NET Web プロジェクトの作成**です。

### <a name="owin-integration"></a>OWIN の統合

ASP.NET 認証は、任意の OWIN ベースのホストで使用できますの OWIN ミドルウェアに基づいています。 OWIN の詳細については、次を参照してください。 [Microsoft OWIN コンポーネント](#TOC7)セクションです。

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Microsoft OWIN コンポーネント

[.NET 用 Web インターフェイスを開く](http://owin.org/)(OWIN) .NET web サーバーと web アプリケーション間で抽象型を定義します。 OWIN では、web アプリケーション ホストにとらわれないを行う、サーバーから web アプリケーションを分離します。 たとえば、IIS で OWIN ベースの web アプリケーションをホストしたり、カスタム プロセス内で自己ホストできます。

Microsoft OWIN コンポーネント (Katana プロジェクトとも呼ばれます) で導入された変更には、新しいサーバーとホストのコンポーネント、新しいヘルパー ライブラリとミドルウェア、および新しい認証ミドルウェアが含まれます。

OWIN および Katana の詳細については、次を参照してください。 [OWIN および Katana 新](../../../aspnet/overview/owin-and-katana/index.md)です。

**注: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)アプリケーションは、IIS クラシック モードで実行できません。 統合モードで実行する必要があります。**

**注: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)完全信頼でアプリケーションを実行する必要があります。**

### <a name="new-servers-and-hosts"></a>新しいサーバーとホスト

このリリースでは、新しいコンポーネントは、自己ホストのシナリオを有効にする追加されました。 これらのコンポーネントには、次の NuGet パッケージが含まれます。

- **Microsoft.Owin.Host.HttpListener**です。 使用する OWIN サーバーは、 **HttpListener**を HTTP 要求をリッスンし、OWIN パイプラインに指示します。
- **Microsoft.Owin.Hosting**自己コンソール アプリケーションまたは Windows サービスなど、カスタムのプロセスで OWIN パイプラインをホストしたい開発者向けのライブラリを提供します。
- **OwinHost**です。 ラップするスタンドアロンの実行可能ファイルを提供`Microsoft.Owin.Hosting`自己をカスタム ホスト アプリケーションを記述しなくても、OWIN パイプラインをホストすることができます。

さらに、`Microsoft.Owin.Host.SystemWeb`パッケージにヒントを指定してミドルウェアを今すぐ有効に、 **SystemWeb**サーバー、特定の ASP.NET パイプライン ステージ中に、ミドルウェアを呼び出す必要があります。 この機能は、認証ミドルウェアは、早い段階での ASP.NET パイプラインを実行する必要がありますに特に便利です。

### <a name="helper-libraries-and-middleware"></a>ヘルパー ライブラリとミドルウェア

OWIN 仕様からの機能と種類の定義のみを使用する OWIN コンポーネントを記述することができますが、新しい`Microsoft.Owin`パッケージは、抽象化のわかりやすいセットを提供します。 このパッケージは以前のいくつかのパッケージを組み合わせたもの (例: `Owin.Extensions`、 `Owin.Types`) し、簡単に使用できる他の OWIN コンポーネントで適切に構成された、1 つのオブジェクト モデルをします。 実際には、Microsoft OWIN コンポーネントの大部分は、このパッケージを使用するようになりました。

> [!NOTE]
> [OWIN](http://www.owin.org)アプリケーションは、IIS クラシック モードで実行できません。 統合モードで実行する必要があります。

> [!NOTE]
> [OWIN](http://www.owin.org)完全信頼でアプリケーションを実行する必要があります。

このリリースでは、ミドルウェアで実行中の OWIN アプリケーションを検証するとエラーを調査に役立つエラー ページ ミドルウェアが含まれます Microsoft.Owin.Diagnostics パッケージのも含まれます。

### <a name="authentication-components"></a>認証コンポーネント

次の認証コンポーネントを利用できます。

- **Microsoft.Owin.Security.ActiveDirectory**です。 内部設置型またはクラウド ベースのディレクトリ サービスを使用して認証を有効にします。
- **Microsoft.Owin.Security.Cookies** cookie を使用して認証を有効します。 このパッケージが以前に名前付き`Microsoft.Owin.Security.Forms`します。
- **Microsoft.Owin.Security.Facebook** Facebook の OAuth ベースのサービスを使用して認証を有効します。
- **Microsoft.Owin.Security.Google** Google の OpenID ベースのサービスを使用して認証を有効します。
- **Microsoft.Owin.Security.Jwt** JWT トークンを使用して認証を有効します。
- **Microsoft.Owin.Security.MicrosoftAccount** Microsoft アカウントを使用して認証を有効します。
- **Microsoft.Owin.Security.OAuth**です。 ベアラー トークンを認証するため、OAuth 承認サーバーだけでなくミドルウェアを提供します。
- **Microsoft.Owin.Security.Twitter** Twitter の OAuth ベースのサービスを使用して認証を有効します。

このリリースにも含まれています、`Microsoft.Owin.Cors`パッケージで、HTTP のクロス オリジン要求を処理するためのミドルウェアが含まれています。

> [!NOTE]
> JWT の署名のサポートは Visual Studio 2013 の最終バージョンで削除されました。

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

新機能および Entity Framework 6 の他の変更の一覧は、次を参照してください。 [Entity Framework のバージョン履歴](https://msdn.com/data/jj574253)です。

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 には、次の新機能が含まれています。

- タブの編集をサポートします。 Preivously、**ドキュメントのフォーマット**コマンド、自動インデント、および自動の Visual Studio での書式設定が正しく動作しなかったを使用する場合、**タブの保持**オプション。 この変更は、Visual Studio の書式設定 タブの Razor コードの書式設定を修正します。
- リンクを生成するときに URL 書き換えルールをサポートします。
- セキュリティ透過的な属性の削除。
 > [!NOTE]
 > これは、重大な変更と Razor 3 互換性のない MVC4 以前のバージョンと Razor 2 は MVC5 MVC5 に対してコンパイルされたアセンブリとの互換性はありません。

プレリリース版から Visual Studio 2013 で修正された razor 3 問題はあります[ここ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0)です。

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.NET アプリを中断します。

ASP.NET アプリの中断は、ユーザー エクスペリエンスと多数の 1 台のコンピューター上の ASP.NET のサイトをホストするための経済モデルを大幅に変更する .NET Framework 4.5.1 における革新的な機能です。 詳細については、次を参照してください。 [ASP.NET App Suspend – 応答共有 .NET web ホスティング](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx)です。

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

このセクションでは、Visual Studio 2013 用 ASP.NET および Web ツールの変更点と既知の問題について説明します。

### <a name="nuget"></a>NuGet

- [SLN ファイルを使用するときにモノラルで新しいパッケージの復元は機能しません](https://nuget.codeplex.com/workitem/3596)– 今後 nuget.exe ダウンロードで修正されると[NuGet.CommandLine パッケージ](http://www.nuget.org/packages/NuGet.CommandLine/)を更新します。
- [Wix プロジェクトで新しいパッケージの復元は機能しません](https://nuget.codeplex.com/workitem/3598)– 今後 nuget.exe ダウンロードで修正されると[NuGet.CommandLine パッケージ](http://www.nuget.org/packages/NuGet.CommandLine/)を更新します。
- [自動のパッケージ復元は、ソリューション フォルダーの下のプロジェクトのそれでも](https://nuget.codeplex.com/workitem/3625)– NuGet 2.8 で修正される予定です。

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)`返さない`IQueryable<T>`おのサポートが追加されると、常に`$select`と`$expand`です。

    以前サンプル`ODataQueryOptions<T>`から戻り値を常にキャスト`ApplyTo`に`IQueryable<T>`です。 以前この以前するクエリのオプションのための方法でサポート (`$filter`、 `$orderby`、 `$skip`、 `$top`) クエリの形状を変更しないでください。 これで、サポートされています`$select`と`$expand`からの戻り値`ApplyTo`されません`IQueryable<T>`常にします。

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    前述の例とサンプル コードを使用している場合、クライアントが送信されなかった場合操作は続行されます`$select`と`$expand`です。 ただし、サポートする場合`$select`と`$expand`にそのコードを変更する必要があります。

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url または RequestContext.Url が null で、バッチ要求時に**

    バッチ処理のシナリオで**UrlHelper**からアクセスする場合は null は**Request.Url**または**RequestContext.Url**です。

    この問題はここで現在追跡: [BatchRequestContext.Url が要求をバッチ処理の null](http://aspnetwebstack.codeplex.com/workitem/1301)です。

    この問題の回避策はの新しいインスタンスを作成する**UrlHelper**、次の例のようにします。

    **UrlHelper の新しいインスタンスを作成します。**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. 使用すると MVC5 と OrgAuth、AntiForgerToken 検証を実行するビューがある場合、ビューにデータを投稿すると、次のエラーに遭遇した可能性があります。

    **エラー**:

    *'/' アプリケーションでのサーバー エラーです。*

    *型 'http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier' または 'http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider' の要求は、指定した ClaimsIdentity に存在しませんでした。クレーム ベース認証を使用した偽造防止トークンのサポートを有効にするには、構成されている要求プロバイダーが提供している生成 ClaimsIdentity インスタンスでこれらの要求の両方をしてくださいを確認します。代わりに、構成されている要求プロバイダーは、一意の識別子として、異なる要求種類を使用する場合は、静的プロパティ AntiForgeryConfig.UniqueClaimTypeIdentifier を設定して構成できます。*

    **回避策**:

    その修正 Global.asax で、次の行を追加します。

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    これは、次のリリースの修正されます。
2. MVC5 に MVC4 アプリケーションをアップグレードした後は、ソリューションをビルドし、これを起動します。 次のエラーが表示されます。

    [A][B]System.Web.WebPages.Razor.Configuration.HostSection に System.Web.WebPages.Razor.Configuration.HostSection をキャストすることはできません。 タイプ A の発生元 'System.Web.WebPages.Razor、バージョン 2.0.0.0、Culture = neutral, PublicKeyToken = 31bf3856ad364e35 を =' コンテキストの位置には、' Default' で' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll' です。 タイプ B の発生元 'System.Web.WebPages.Razor, Version 3.0.0.0]、[カルチャを = = neutral, PublicKeyToken = 31bf3856ad364e35' コンテキストの位置には、' Default' で' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll' です。

    上記のエラーを修正するには、開きます*すべて*プロジェクトとは、次の Web.config ファイル (Views フォルダーにあるものを含む)。

    1. 「5.0.0.0」の"System.Web.Mvc"のバージョン「4.0.0.0」のすべての項目を更新します。
    2. "System.Web.Helpers"のバージョン「2.0.0.0」のすべての項目を更新&quot;System.Web.WebPages&quot;と&quot;System.Web.WebPages.Razor&quot; 「3.0.0.0」を

    など、上記の変更を加えた後アセンブリ バインドは次のようになります。

    [!code-xml[Main](release-notes/samples/sample24.xml)]

    MVC 5 に MVC 4 プロジェクトをアップグレードする方法については、次を参照してください。 [ASP.NET MVC 5 と Web API 2 に、ASP.NET MVC 4 と Web API プロジェクトをアップグレードする方法](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)です。
3. 、JQuery 検証の控えめなクライアント側の検証を使用しているときに、検証メッセージが正しくない場合もあります型の HTML input 要素を = 'number' です。 検証エラーに対して必要な値 (「[Age] フィールドが必要」) を表示、適切なメッセージの有効な数値が必要であるではなく無効な数値が入力されている場合。

    この問題は一般的に生じるスキャフォールディング コードで作成および編集ビューで整数のプロパティを使用してモデルのです。

    この問題を回避するからエディター ヘルパーを変更します。

    `@Html.EditorFor(person => person.Age)`

    移動先:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 は、部分的に信頼をサポートしていません。 プロジェクトの MVC または WebAPI バイナリへのリンクを削除する必要があります、 [SecurityTransparent](https://msdn.microsoft.com/en-us/library/system.security.securitytransparentattribute.aspx)属性および[AllowPartiallyTrustedCallers](https://msdn.microsoft.com/en-us/library/system.security.allowpartiallytrustedcallersattribute.aspx)属性。 これらの属性を削除するには、次のようにコンパイラ エラーがなくなります。

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > ただし、操作の副作用は、同じアプリケーションで 4.0 および 5.0 のアセンブリを使用することはできません。 5.0 に更新し、すべての必要があります。

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Facebook の承認で SPA テンプレート可能性がありますが不安定になる IE、web サイトがイントラネット ゾーンでホストされているときに

SPA テンプレートは、Facebook での外部ログを提供します。 テンプレートを使用して作成されたプロジェクトは、ローカルで実行中は、サインインする場合がありますに IE がクラッシュします。

解決方法 : 

1. インターネット ゾーンの web サイトのホストまたは

2. IE 以外のブラウザーでシナリオをテストします。

### <a name="web-forms-scaffolding"></a>Web フォームのスキャフォールディング

Web フォームのスキャフォールディングは VS2013 から削除されましたし、Visual Studio に対する今後の更新で使用できます。 ただし、MVC 依存関係を追加し、MVC のスキャフォールディングを生成して、Web フォーム プロジェクト内のスキャフォールディングを使用できます。 プロジェクトは、Web フォームと MVC の組み合わせが格納されます。

MVC プロジェクトを追加する、Web フォーム、新しいスキャフォールディングされた項目を追加および選択**MVC 5 の依存関係**です。 最小またはスクリプトなどのコンテンツ ファイルのすべてを必要とするかどうかに応じて完全のいずれかを選択します。 次に、コント ローラーとビューを作成し、プロジェクトの MVC のスキャフォールディングされた項目を追加します。

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC と Web API のスキャフォールディング - HTTP 404 Not Found エラー

スキャフォールディングされた項目をプロジェクトに追加するときにエラーが発生した場合、プロジェクトは不整合な状態になることができます。 いくつかのスキャフォールディングをで行われた変更はロールバックされますが、インストールされている NuGet パッケージなど、他の変更はロールバックされません。 ルーティングの構成の変更がロールバックされた場合、ユーザーはスキャフォールディングされた項目に移動するときに HTTP 404 エラーが表示されます。

対応策 :

- MVC のこのエラーを解決する新しいスキャフォールディングされた項目を追加し、MVC 5 の依存関係の選択 (最小または完全のいずれか)。 このプロセスでは、プロジェクトに追加のすべての必要な変更します。
- Web api には、このエラーを解決します。

    1. WebApiConfig クラスをプロジェクトに追加します。

        [!code-csharp[Main](release-notes/samples/sample25.cs)]

        [!code-vb[Main](release-notes/samples/sample26.vb)]
    2. アプリケーションで WebApiConfig.Register を構成\_Global.asax で次のようにメソッドを開始します。

        [!code-csharp[Main](release-notes/samples/sample27.cs)]

        [!code-vb[Main](release-notes/samples/sample28.vb)]
