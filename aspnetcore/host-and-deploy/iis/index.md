---
title: IIS を使用した Windows での ASP.NET Core のホスト
author: guardrex
description: Windows Server インターネット インフォメーション サービス (IIS) での ASP.NET Core アプリをホストする方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
uid: host-and-deploy/iis/index
ms.openlocfilehash: f35fbbbf7d04b041565e76d3cc6b9822f1056e50
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824540"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>IIS を使用した Windows での ASP.NET Core のホスト

執筆者: [Luke Latham](https://github.com/guardrex)、[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>サポートされるオペレーティング システム

次のオペレーティング システムがサポートされています。

* Windows 7 以降
* Windows Server 2008 R2 以降

[HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (以前の名称は [WebListener](xref:fundamentals/servers/weblistener)) が、IIS を含むリバース プロキシ構成で動作しません。 [Kestrel サーバー](xref:fundamentals/servers/kestrel)を使用します。

## <a name="application-configuration"></a>アプリケーション構成

### <a name="enable-the-iisintegration-components"></a>IISIntegration コンポーネントを有効にする

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

一般的な *Program.cs* は [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。 `CreateDefaultBuilder` は [Kestrel](xref:fundamentals/servers/kestrel) を Web サーバーとして構成し、ベース パスとポートを [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)に構成することで、IIS 統合を有効にします。

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

ASP.NET Core モジュールは、バックエンド プロセスに割り当てる動的なポートを生成します。 `CreateDefaultBuilder` は [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) メソッドを呼び出します。このメソッドは、動的なポートを取得すると共に、`http://localhost:{dynamicPort}/` でリッスンするように Kestrel を構成します。 これにより、`UseUrls` または [Kestrel の Listen API](xref:fundamentals/servers/kestrel#endpoint-configuration) の呼び出しなど、その他の URL 構成がオーバーライドされます。 そのため、モジュールを使用するときに、`UseUrls` または Kestrel の `Listen` API の呼び出しは必要ありません。 `UseUrls` または `Listen` が呼び出されると、IIS なしでアプリを実行するときに指定されたポートで Kestrel が受信を待機します。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

アプリの依存関係に [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) パッケージへの依存関係を含めます。 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) 拡張メソッドを [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) に追加して、IIS 統合ミドルウェアを使用します。

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) と [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) の両方が必要です。 `UseIISIntegration` を呼び出すコードはコードの移植性に影響しません。 アプリが IIS の背後で実行されていない場合 (たとえば、アプリが Kestrel で直接実行されている場合)、`UseIISIntegration` は機能しません。

ASP.NET Core モジュールは、バックエンド プロセスに割り当てる動的なポートを生成します。 `UseIISIntegration` メソッドは、この動的なポートを取得すると共に、`http://locahost:{dynamicPort}/` をリッスンするように Kestrel を構成します。 これにより、`UseUrls` の呼び出しなど、その他の URL 構成がオーバーライドされます。 そのため、モジュールを使用するときに、`UseUrls` の呼び出しは必要ありません。 `UseUrls` が呼び出されると、IIS なしでアプリを実行するときに指定されたポートで Kestrel が受信を待機します。

ASP.NET Core 1.0 アプリ内で `UseUrls` 呼び出される場合、モジュールに構成されたポートが上書きされないように、 **を呼び出す**前に`UseIISIntegration`呼び出されます。 この呼び出しの順序は ASP.NET Core 1.1 では必要ありません。モジュール設定は `UseUrls` をオーバーライドするからです。

---

ホスティングの詳細については、「[Hosting in ASP.NET Core](xref:fundamentals/host/index)」(ASP.NET Core でのホスティング) を参照してください。

### <a name="iis-options"></a>IIS のオプション

[IIS](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) のオプションを構成するには、IISOptions のサービス構成を [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices) に含めます。 次の例では、`HttpContext.Connection.ClientCertificate` を入力するためのクライアント証明書のアプリへの転送は無効になります。

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| オプション                         | 既定値 | 設定 |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | `true` の場合、IIS 統合ミドルウェアが、[Windows 認証](xref:security/authentication/windowsauth)によって `HttpContext.User` を設定します。 `false` の場合、ミドルウェアは `HttpContext.User` の ID を提供するだけで、`AuthenticationScheme` によって明示的に要求されたときに課題に応答します。 `AutomaticAuthentication` を機能させるためには、IIS で Windows 認証を有効にする必要があります。 詳細については、「[Windows 認証](xref:security/authentication/windowsauth)」のトピックを参照してください。 |
| `AuthenticationDisplayName`    | `null`  | ログイン ページでユーザーに表示名が表示されるように設定します。 |
| `ForwardClientCertificate`     | `true`  | `true` の場合、`MS-ASPNETCORE-CLIENTCERT` 要求ヘッダーが指定されていると、`HttpContext.Connection.ClientCertificate` が設定されます。 |

### <a name="proxy-server-and-load-balancer-scenarios"></a>プロキシ サーバーとロード バランサーのシナリオ

要求が発生したスキーム (HTTP/HTTPS) とリモート IP アドレスを転送するために、Forwarded Headers Middleware を構成する IIS 統合ミドルウェアと、ASP.NET Core モジュールが構成されます。 追加のプロキシ サーバーとロード バランサーの背後でホストされているアプリでは、追加の構成が必要になる場合があります。 詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。

### <a name="webconfig-file"></a>web.config ファイル

*web.config* ファイルでは、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を設定します。 *web.config* ファイルの作成、変換、および公開は、プロジェクトの公開時に、MSBuild ターゲット (`_TransformWebConfig`) によって処理されます。 このターゲットは、Web SDK ターゲット (`Microsoft.NET.Sdk.Web`) に存在します。 SDK は、プロジェクト ファイルの先頭で設定されています。

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

プロジェクトに *web.config* ファイルが含まれていない場合、そのファイルは [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を構成するための正しい *processPath* と *arguments* を使用して作成され、[発行された出力](xref:host-and-deploy/directory-structure)に移行されます。

プロジェクトに *web.config* ファイルが含まれていない場合、そのファイルは ASP.NET Core モジュールを構成するための正しい *processPath* と *arguments* を使用して作成され、発行された出力に移行されます。 変換によりファイル内の IIS 構成の設定が変わることはありません。

*web.config* ファイルは、アクティブな IIS モジュールを制御する追加の IIS 構成設定を提供する可能性があります。 ASP.NET Core アプリを使用して要求を処理できる IIS モジュールの詳細については、[IIS モジュール](xref:host-and-deploy/iis/modules)のトピックを参照してください。

Web SDK によって *web.config* ファイルが変換されないようにするため、**\<IsTransformWebConfigDisabled>** プロパティをプロジェクト ファイルで使用します。

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Web SDK ファイルの変換を無効にすると、 *processPath*と*引数*開発者によって手動で設定する必要があります。 詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。

### <a name="webconfig-file-location"></a>web.config ファイルの場所

IIS と Kestrel サーバーの間にリバース プロキシを作成するには、展開されるアプリのコンテンツ ルート パス (通常は、アプリ ベース パス) に *web.config* ファイルが存在する必要があります。 これは、IIS に提供される web サイトの物理パスと同じ場所です。 *Web.config* ファイルは、Web 配置を使用して複数のアプリの発行を有効にするため、アプリのルートで必要です。

アプリの物理パスには、*\<assembly>.runtimeconfig.json*、*\<assembly>.xml* (XML ドキュメントのコメント)、*\<assembly>.deps.json* などの機密性の高いファイルが存在します。 *web.config* ファイルが存在し、サイトは通常どおり起動した場合、IIS は、これらの機密性の高いファイルが要求された場合にファイルを提供しません。 *web.config*ファイルが存在しないか、不適切な名前が付けられているか、または通常の起動用にサイトを構成できない場合、IIS が機密性の高いファイルを公開する可能性があります。

***web.config*ファイルは、展開環境に常に存在し、適切な名前が付けられ、通常の起動用にサイトを構成できる必要があります。*web.config* ファイルを運用環境の展開から削除しないでください。**

## <a name="iis-configuration"></a>IIS 構成

**Windows Server オペレーティング システム**

**Web サーバー (IIS)** サーバーの役割を有効にし、役割のサービスを設定します。

1. **[管理]** メニューから**役割と機能の追加**ウィザードを使用するか、**サーバー マネージャー**にあるリンクを使用します。 **[サーバーの役割]** のステップで、**[Web サーバー (IIS)]** チェック ボックスをオンにします。

   ![[サーバーの役割の選択] のステップで Web サーバー IIS の役割を選択します。](index/_static/server-roles-ws2016.png)

1. **[機能]** のステップの後に、**[役割サービスの]** のステップで Web サーバー (IIS) を読み込みます。 希望する IIS の役割サービスを選択するか、既定の役割サービスをそのまま使用します。

   ![[役割サービスの選択] のステップで既定の役割サービスを選択します。](index/_static/role-services-ws2016.png)

   **Windows 認証 (任意)**  
   Windows 認証を有効にするには、**[Web サーバー]** > **[セキュリティ]** ノードを展開します。 **[Windows 認証]** 機能を選択します。 詳細については、「[Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)」および「[Configure Windows authentication](xref:security/authentication/windowsauth)」(Windows 認証の構成) を参照してください。

   **Websocket (省略可能)**  
   WebSockets は、ASP.NET Core 1.1 以降でサポートされています。 Websocket を有効にするには、**[Web サーバー]** > **[アプリケーション開発]** ノードを展開します。 **[WebSocket プロトコル]** 機能を選択します。 詳細については、[WebSockets](xref:fundamentals/websockets) に関するページを参照してください。

1. **[確認]** のステップを経て Web サーバーの役割とサービスをインストールします。 **Web サーバー (IIS)** の役割をインストールした後、サーバーと IIS の再起動は必要ありません。

**Windows デスクトップ オペレーティング システム**

**[IIS 管理コンソール]** と **[World Wide Web サービス]** を有効にします。

1. **[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。

1. **インターネット インフォメーション サービス (IIS)** ノードを開きます。 **[Web 管理ツール]** ノードを開きます。

1. **[IIS 管理コンソール]** チェック ボックスをオンにします。

1. **[World Wide Web サービス]** チェック ボックスをオンにします。

1. **[World Wide Web サービス]** の既定の機能をそのまま使用するか、IIS 機能をカスタマイズします。

   **Windows 認証 (任意)**  
   Windows 認証を有効にするには、**[World Wide Web サービス]** > **[セキュリティ]** ノードを展開します。 **[Windows 認証]** 機能を選択します。 詳細については、「[Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)」および「[Configure Windows authentication](xref:security/authentication/windowsauth)」(Windows 認証の構成) を参照してください。

   **Websocket (省略可能)**  
   WebSockets は、ASP.NET Core 1.1 以降でサポートされています。 Websocket を有効にするには、**[World Wide Web サービス]** > **[アプリケーション開発機能]** ノードを展開します。 **[WebSocket プロトコル]** 機能を選択します。 詳細については、[WebSockets](xref:fundamentals/websockets) に関するページを参照してください。

1. IIS のインストールに再起動が必要な場合は、システムを再起動します。

![[Windows の機能] で [IIS 管理コンソール] と [World Wide Web サービス] を選択します。](index/_static/windows-features-win10.png)

---

## <a name="install-the-net-core-hosting-bundle"></a>.NET Core ホスティング バンドルのインストール

1. ホスティング システムに *.NET Core ホスティング バンドル*をインストールします。 このバンドルをインストールすることで、.NET Core ランタイム、.NET Core ライブラリ、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。 このモジュールは、IIS と Kestrel サーバーの間にリバース プロキシを作成します。 システムにインターネット接続が設定されていない場合は、.NET Core ホスティング バンドルをインストールする前に、[Microsoft Visual C++ 2015 再頒布可能パッケージ](https://www.microsoft.com/download/details.aspx?id=53840)を入手してインストールしてください。

   1. [.NET の「All Downloads」のページ](https://www.microsoft.com/net/download/all)に移動します。
   1. テーブルの **[Runtime]** 列で、一覧 (**X.Y Runtime (vX.Y.Z) のダウンロード**) からプレビューではない最新の .NET Core ランタイムを選択します。 最新のランタイムには、**[Current]** (最新) ラベルがあります。 プレビュー ソフトウェアと連携しない限り、そのリンク テキストで "Preview" という単語または "rc" (リリース候補) を回避します。
   1. **Windows** の .NET Core のランタイムのダウンロード ページで、**ホスティング バンドル インストーラー**のリンクを選択して、*.NET Core ホスティング バンドル* インストーラーをダウンロードします。
   1. サーバーでインストーラーを実行します。

   **重要**。 ホスティング バンドルが IIS の前にインストールされている場合、バンドルのインストールを修復する必要があります。 IIS をインストールした後に、ホスティング バンドル インストーラーをもう一度実行します。
   
   インストーラーが x86 パッケージを x64 OS 上にインストールしないようにするために、スイッチ `OPT_NO_X86=1` を使用して管理者のコマンド プロンプトからインストーラーを実行します。

1. システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行します。 IIS を再起動すると、インストーラーによって行われたシステム パスへの変更が取得されます。

> [!NOTE]
> IIS 共有構成の詳細については、「[ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)」 (IIS 共有構成の ASP.NET Core モジュール) を参照してください。

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Visual Studio で発行する場合の Web 配置のインストール

[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用してサーバーにアプリを展開する場合、Web 配置の最新バージョンをサーバーにインストールします。 Web 配置をインストールするには、[Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) を使用するか、[Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=43717)からインストーラーを取得します。 WebPI を使用することをお勧めします。 WebPI は、スタンドアロンのセットアップとホスティング プロバイダー向けの構成を提供します。

## <a name="create-the-iis-site"></a>IIS サイトを作成する

1. ホスト システムで、アプリの公開フォルダーとファイルを格納するフォルダーを作成します。 アプリの展開レイアウトについては、「[ディレクトリ構造](xref:host-and-deploy/directory-structure)」のトピックを参照してください。

1. 新しいフォルダー内で、stdout ログが有効になっている場合に ASP.NET Core Module stdout ログを保持するための *logs* フォルダーを作成します。 ペイロードに *logs* フォルダーを含めてアプリを配置する場合は、この手順をスキップします。 プロジェクトがローカルでビルドされるときに、MSBuild を有効にして、*logs* フォルダーを自動的に作成する手順については、「[ディレクトリ構造](xref:host-and-deploy/directory-structure)」のトピックを参照してください。

   > [!IMPORTANT]
   > stdout ログは、アプリケーションの起動エラーのトラブルシューティングにのみ使用します。 日常的なアプリのログ記録に、stdout ログを使用しないでください。 ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。 アプリケーション プールは、ログが書き込まれる場所への書き込みアクセス権を持っている必要があります。 ログの場所へのパス上のフォルダーがすべて存在する必要があります。 stdout ログの詳細については、「[Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)」(ログの作成とリダイレクト) を参照してください。 ASP.NET Core アプリケーションのログ記録の詳細については、「[ログ](xref:fundamentals/logging/index)」のトピックを参照してください。

1. **IIS マネージャー**の **[接続]** パネルでサーバーのノードを開きます。 **[サイト]** フォルダーを右クリックします。 コンテキスト メニューで **[Web サイトの追加]** を選択します。

1. **[サイト名]** を指定し、**[物理パス]** にはアプリの配置フォルダーを設定します。 **[バインド]** の構成を指定して **[OK]** を選択し、Web サイトを作成します。

   ![[Web サイトの追加] のステップでサイト名、物理パス、ホスト名を指定します。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > 最上位のワイルドカードのバインド (`http://*:80/` と `http://+:80`) は使用しては**いけません**。 最上位のワイルドカードのバインドは、セキュリティの脆弱性に対してアプリを切り開くことができます。 これは、強力と脆弱の両方のワイルドカードに適用されます。 ワイルドカードではなく、明示的なホスト名を使用します。 全体の親ドメインを制御する場合、サブドメイン ワイルドカード バインド (たとえば、`*.mysub.com`) にこのセキュリティ リスクはありません (脆弱である `*.com` とは対照的)。 詳細については、[rfc7230 セクション-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) を参照してください。

1. サーバーのノードでは、**[アプリケーション プール]** を選択します。

1. サイトのアプリケーション プールを右クリックし、コンテキスト メニューから **[基本設定]** を選択します。

1. 
  **[アプリケーション プールの編集]** ウィンドウで、**[.NET CLR バージョン]** を **[マネージド コードなし]** に設定します。

   ![.NET CLR バージョンとして [マネージド コードなし] を設定します。](index/_static/edit-apppool-ws2016.png)

    ASP.NET Core は、別個のプロセスで実行され、ランタイムを管理します。 ASP.NET Core を使用するためにデスクトップ CLR を読み込む必要はありません。 
  **[.NET CLR バージョン]** の **[マネージド コードなし]** への設定は、任意です。

1. プロセス モデル ID に適切なアクセス許可があることを確認します。

   アプリ プールの既定の ID (**[プロセス モデル]** > **[ID]**) を **ApplicationPoolIdentity** から別の ID に変更した場合は、アプリのフォルダー、データベース、その他の必要なリソースにアクセスするために要求されるアクセス許可が新しい ID に設定されていることを確認します。 たとえば、アプリケーション プールには、アプリがファイルの読み取りおよび書き込みを行うフォルダーへの読み取りおよび書き込みアクセスが必要です。

**Windows 認証の構成 (任意)**  
詳細については、「[Windows 認証を構成する](xref:security/authentication/windowsauth)」を参照してください。

## <a name="deploy-the-app"></a>アプリを配置する

ホスト システム上に作成したフォルダーにアプリを展開します。 [Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)は、推奨される展開のメカニズムです。

### <a name="web-deploy-with-visual-studio"></a>Visual Studio からのアプリの展開

Web 配置に使用する発行プロファイルの作成方法については、「[Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)」 (ASP.NET Core アプリ展開用の Visual Studio の発行プロファイル) のトピックを参照してください。 ホスティング プロバイダーから発行プロファイルまたは発行プロファイルを作成するためのサポートが提供されている場合は、そのプロファイルをダウンロードし、Visual Studio の **[発行]** ダイアログを使用してインポートします。

![[発行] ダイアログ ページ](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Visual Studio 外部での Web 配置

Visual Studio の外部で、コマンド ラインから [Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用することもできます。 詳細については、[Web 配置ツール](/iis/publish/using-web-deploy/use-the-web-deployment-tool)に関するページを参照してください。

### <a name="alternatives-to-web-deploy"></a>Web 配置の代替手段

手動コピー、Xcopy、Robocopy、PowerShell などの任意の方法を使用して、ホスティング システムにアプリを移動します。

IIS への ASP.NET Core の展開の詳細については、「[IIS 管理者用の展開リソース](#deployment-resources-for-iis-administrators)」を参照してください。

## <a name="browse-the-website"></a>Web サイトを閲覧する

![Microsoft Edge ブラウザーに IIS のスタートアップ ページが読み込まれています。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>ロックされた展開ファイル

アプリが実行中は、展開フォルダー内のファイルがロックされます。 展開中にロックされたファイルを上書きすることはできません。 展開でロックされているファイルをリリースするには、次の**いずれか**の方法を使用してアプリ プールを停止します。

* Web 配置を使用し、プロジェクト ファイル内の `Microsoft.NET.Sdk.Web` を参照して停止します。 *app_offline.htm* ファイルは、Web アプリのディレクトリのルートに配置されます。 ファイルが存在する場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、展開中に *app_offline.htm* ファイルを提供します。 詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)」を参照してください。
* サーバー上の IIS マネージャーで手動によりアプリ プールを停止します。
* PowerShell を使用して、アプリ プールを停止して再起動します (PowerShell 5 以降が必要)。

  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

## <a name="data-protection"></a>データの保護

[ASP.NET Core データ保護スタック](xref:security/data-protection/index)は、認証で使用されるミドルウェアを含め、いくつかの [ASP.NET Core](xref:fundamentals/middleware/index) ミドルウェアによって使用されます。 データ保護 API がユーザーのコードから呼び出されない場合でも、配置スクリプトを使用するかユーザー コードで、永続的な暗号化[キー ストア](xref:security/data-protection/implementation/key-management)を作成するようにデータ保護を構成する必要があります。 データ保護を構成しない場合、既定でキーはメモリ内に保持され、アプリが再起動すると破棄されます。

キーリングがメモリに格納されている場合、アプリを再起動すると次のことが行われます。

* すべての cookie ベースの認証トークンは無効になります。 
* ユーザーは、次回の要求時に再度サインインする必要があります。 
* キーリングで保護されているデータは、いずれも復号化できなくなります。 これには、[CSRF トークン](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)と [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata) が含まれます。

キーリングを保持するために IIS でのデータ保護を構成するには、次の**いずれか**の方法を使用する必要があります。

* **データ保護のレジストリ キーを作成する**

  ASP.NET Core アプリで使用されるデータ保護キーは、アプリ外部のレジストリに格納されます。 特定のアプリのキーを保持するには、アプリ プール用のレジストリ キーを作成する必要があります。

  非 Web ファーム IIS のスタンドアロン インストールの場合、ASP.NET Core アプリで使用するアプリ プールごとに、[データ保護の PowerShell スクリプト Provision-AutoGenKeys.ps1](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) を使用できます。 このスクリプトは、HKLM レジストリにレジストリ キーを作成します。そのキーは、アプリのアプリ プールのワーカー プロセス アカウントのみがアクセスできます。 キーは保存時に、DPAPI を使用して、コンピューター全体に適用するキーによって暗号化されます。

  Web ファームのシナリオでは、UNC パスを使用してデータ保護キー リングを格納するようにアプリを構成できます。 既定では、データ保護キーは暗号化されません。 ネットワーク共有のファイルのアクセス許可が、アプリを実行する Windows アカウントに限定されていることを確認します。 保存時のキーを保護するために、X509 証明書を使用することができます。 ユーザーが証明書をアップデートできるメカニズムを検討している場合、ユーザーの信頼できる証明書ストアに証明書を配置し、ユーザーのアプリが実行されるすべてのコンピューターで証明書を利用できるようにします。 詳細については、「[ASP.NET Core データ保護の構成](xref:security/data-protection/configuration/overview)」を参照してください。

* **ユーザー プロファイルを読み込むための IIS アプリケーション プールを構成する**

  この設定は、アプリ プールの **[詳細設定]** の **[プロセス モデル]** セクションにあります。 [ユーザー プロファイルの読み込み] を `True` に設定します。 これによりキーはユーザー プロファイル ディレクトリに格納され、DPAPI を使用して、アプリ プールで使うユーザー アカウントに固有のキーによって保護されます。

* **ファイル システムをキー リング ストアとして使用する**

  [ファイル システムをキー リング ストアとして使用](xref:security/data-protection/configuration/overview)するようにアプリ コードを調整します。 X509 証明書を使用してキー リングを保護し、その証明書が信頼された証明書であることを確認します。 証明書が自己署名されている場合は、信頼されたルート ストアにその証明書を配置します。

  Web ファームで IIS を使用する場合:

  * すべてのコンピューターがアクセスできるファイル共有を使用します。
  * X509 証明書を各コンピューターに配置します。 [コード内にデータ保護](xref:security/data-protection/configuration/overview)を構成します。

* **コンピューター全体に適用するデータ保護ポリシーを設定する**

  データ保護システムでは、データ保護 API を使用するすべてのアプリに対して、[コンピューター全体に適用する既定のポリシー](xref:security/data-protection/configuration/machine-wide-policy)を設定するためのサポートは限定的です。 詳細については、[データ保護](xref:security/data-protection/index)に関するドキュメントを参照してください。

## <a name="sub-application-configuration"></a>サブアプリケーション構成

ルート アプリの下に追加したサブアプリに、ハンドラーとして ASP.NET Core モジュールを含めることはできません。 このモジュールをサブアプリの *web.config* ファイルにハンドラーとして追加すると、サブアプリを閲覧しようとすると、エラーのある構成ファイルを参照する *500.19 内部サーバー エラー* が返されます。

次の例は、ASP.NET Core サブアプリの発行済み *web.config* ファイルを示しています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

ASP.NET Core アプリの下に ASP.NET Core 以外のサブアプリをホストする場合は、サブアプリの *web.config* ファイルにある継承されたハンドラーを明示的に削除します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

ASP.NET Core モジュールを構成する方法の詳細については、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」と「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。

## <a name="configuration-of-iis-with-webconfig"></a>web.config による IIS の構成

IIS の構成は、リバース プロキシ構成に適用されるこれらの IIS 機能の *web.config*の **\<system.webServer>** セクションの影響を受けます。 IIS が動的な圧縮を使用するようにサーバー レベルで構成されている場合、アプリの *web.config* ファイルで **\<urlCompression>** 要素を使用して無効にすることができます。

詳細については、[\<system.webServer> の構成リファレンス](/iis/configuration/system.webServer/)、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」および「[IIS モジュールと ASP.NET Core](xref:host-and-deploy/iis/modules)」を参照してください。 分離されたアプリ プール (IIS 10.0 以降でサポートされています) で実行する個別アプリに対して環境変数を設定するには、IIS のリファレンス ドキュメントで、[環境変数\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。

## <a name="configuration-sections-of-webconfig"></a>web.config の構成のセクション

*web.config* の ASP.NET 4.x アプリの構成セクションは、ASP.NET Core アプリの構成では使用されません。

* **\<system.web >**
* **\<appSettings>**
* **\<connectionStrings>**
* **\<location>**

ASP.NET Core アプリは、他の構成プロバイダーを使用して構成されます。 詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。

## <a name="application-pools"></a>アプリケーション プール

サーバーで複数の Web サイトをホストする場合は、アプリをそれぞれ専用のアプリ プールで実行して、アプリを相互に分離することをお勧めします。 IIS の **[Web サイトの追加]** ダイアログはこの構成の既定です。 **[サイト名]** を指定すると、入力したテキストが自動的に **[アプリケーション プール]** テキストボックスに設定されます。 サイトが追加されるときに、そのサイト名を使用して新しいアプリ プールが作成されます。

## <a name="application-pool-identity"></a>アプリケーション プール ID

アプリ プール ID アカウントを使用すると、ドメインやローカル アカウントを作成して管理する必要なく、一意のアカウントでアプリを実行できます。 IIS 8.0 以降の IIS 管理者ワーカー プロセス (WAS) は、新しいアプリ プールの名前で仮想アカウントを作成し、既定によってアプリ プールのワーカー プロセスをこのアカウントで実行します。 IIS 管理コンソールにあるアプリ プールの **[詳細設定]** で、**ID** が **ApplicationPoolIdentity** を使用するように設定されていることを確認します。

![アプリケーション プールの [詳細設定] ダイアログ](index/_static/apppool-identity.png)

IIS 管理プロセスは、Windows セキュリティ システムでのアプリ プールの名前を使用して、セキュリティで保護された識別子を作成します。 この ID を使用してリソースを保護することができます。 ただし、この ID は実際のユーザー アカウントではなく、Windows ユーザーの管理コンソールに表示されません。

アプリに対する IIS ワーカー プロセスのアクセス許可を昇格させる必要がある場合は、アプリを含むディレクトリのアクセス制御リスト (ACL) を変更します。

1. エクスプローラーを開き、そのディレクトリに移動します。

1. そのディレクトリを右クリックし、**[プロパティ]** を選択します。

1. **[セキュリティ]** タブの **[編集]** ボタンを選択し、**[追加]** ボタンをクリックします。

1. **[場所]** ボタンを選択し、システムが選択されていることを確認します。

1. **[選択するオブジェクト名を入力してください]** 領域に、「**IIS AppPool\\<app_pool_name>**」と入力します。 **[名前の確認]** を選択します。 *DefaultAppPool* で、**IIS AppPool\DefaultAppPool** を使用して名前を確認します。 **[名前の確認]** ボタンが選択されているときには、**DefaultAppPool** の値が、オブジェクト名領域に表示されます。 オブジェクト名の領域に直接アプリケーション プール名を入力することはできません。 オブジェクト名を確認するときには、**IIS AppPool\\<app_pool_name>** の形式を使用します。

   ![アプリ フォルダーのユーザーまたはグループのダイアログを選択します。[名前の確認] を選択する前に、オブジェクト名領域で "DefaultAppPool" のアプリ名が "IIS AppPool\" に適用されます。](index/_static/select-users-or-groups-1.png)

1. **[OK]** を選択します。

   ![アプリ フォルダーのユーザーまたはグループのダイアログを選択します。[名前の確認] を選択した後に、オブジェクト名 "DefaultAppPool" がオブジェクト名領域に表示されます。](index/_static/select-users-or-groups-2.png)

1. 読み取り &amp; 実行アクセス許可は、既定で付与される必要があります。 必要に応じて、追加のアクセス許可を提供します。

**ICACLS** ツールを使用してコマンド プロンプトでアクセス許可を付与することもできます。 たとえば、*DefaultAppPool* を使用する場合、次のコマンドを使用します。

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

詳細については、「[icacls](/windows-server/administration/windows-commands/icacls)」のトピックを参照してください。

## <a name="deployment-resources-for-iis-administrators"></a>IIS 管理者用の展開リソース

IIS ドキュメントでの IIS の詳細について説明します。  
[IIS ドキュメント ](/iis)

.NET Core アプリの展開モデルについて説明します。  
[.NET Core アプリケーションの展開](/dotnet/core/deploying/)

Kestrel Web サーバーが IIS または IIS Express をリバース プロキシ サーバーとして使用できるようにするための ASP.NET Core モジュールについて説明します。  
[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)

ASP.NET Core アプリをホストするための ASP.NET Core モジュールを構成する方法について説明します。  
[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)

発行された ASP.NET Core アプリのディレクトリ構造について説明します。  
[ディレクトリの構造](xref:host-and-deploy/directory-structure)

ASP.NET Core アプリに対してアクティブおよび非アクティブな IIS モジュールについて、さらに IIS モジュールの管理方法についてを把握します。  
[IIS モジュール](xref:host-and-deploy/iis/troubleshoot)

ASP.NET Core アプリの IIS 展開に関する問題を診断する方法について説明します。  
[トラブルシューティング](xref:host-and-deploy/iis/troubleshoot)

IIS で ASP.NET Core アプリをホストする場合の一般的なエラーを識別します。  
[Azure App Service と IIS の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a>その他の技術情報

* [ASP.NET Core の概要](xref:index)
* [Microsoft IIS 公式サイト](https://www.iis.net/)
* [Windows Server テクニカル コンテンツ ライブラリ](/windows-server/windows-server)
