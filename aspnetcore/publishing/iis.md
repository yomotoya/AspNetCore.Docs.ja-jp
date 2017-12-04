---
title: "IIS を使用した Windows での ASP.NET Core のホスト"
author: guardrex
description: "Windows Server インターネット インフォメーション サービス (IIS) の構成と ASP.NET Core アプリケーションの展開について説明します。"
keywords: "ASP.NET Core, インターネット インフォメーション サービス, iis, windows server, ホスティング バンドル, asp.net core モジュール, web 配置"
ms.author: riande
manager: wpickett
ms.date: 03/13/2017
ms.topic: article
ms.assetid: a4449ad3-5bad-410c-afa7-dc32d832b552
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/iis
ms.openlocfilehash: 7eb1537df47fcf0b24db2a7d843b655a6f6f8f21
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/29/2017
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>IIS を使用した Windows での ASP.NET Core のホスト

執筆者: [Luke Latham](https://github.com/guardrex)、[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>サポートされるオペレーティング システム

次のオペレーティング システムがサポートされています。

* Windows 7 およびそれ以降
* Windows Server 2008 R2 およびそれ以降&#8224;

&#8224;概念的には、このドキュメントで説明する IIS 構成は、Nano Server IIS で ASP.NET Core アプリケーションをホストする場合にも当てはまりますが、詳細な手順については「[ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server)」 (IIS を使用した Nano Server での ASP.NET Core) を参照してください。

[HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (以前の名称は [WebListener](xref:fundamentals/servers/weblistener)) が、IIS を含むリバース プロキシ構成で動作しません。 [Kestrel サーバー](xref:fundamentals/servers/kestrel)を使用する必要があります。

## <a name="iis-configuration"></a>IIS 構成

**Web サーバー (IIS)** の役割を有効にし、役割のサービスを設定します。

### <a name="windows-desktop-operating-systems"></a>Windows デスクトップ オペレーティング システム

**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。 **[インターネット インフォメーション サービス]** と **[Web 管理ツール]** のグループを開きます。 **[IIS 管理コンソール]** チェック ボックスをオンにします。 **[World Wide Web サービス]** チェック ボックスをオンにします。 **[World Wide Web サービス]** の既定の機能をそのまま使用するか、ニーズに合わせて IIS 機能をカスタマイズします。

![[Windows の機能] で [IIS 管理コンソール] と [World Wide Web サービス] を選択します。](iis/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Windows Server オペレーティング システム

サーバー オペレーティング システムでは、**[管理]** メニューから**役割と機能の追加**ウィザードを使用するか、**サーバー マネージャー**にあるリンクを使用します。 **[サーバーの役割]** のステップで、**[Web サーバー (IIS)]** チェック ボックスをオンにします。

![[サーバーの役割の選択] のステップで Web サーバー IIS の役割を選択します。](iis/_static/server-roles-ws2016.png)

**[役割サービス]** のステップで、希望する IIS の役割サービスを選択するか、既定の役割サービスをそのまま使用します。

![[役割サービスの選択] のステップで既定の役割サービスを選択します。](iis/_static/role-services-ws2016.png)

**[確認]** のステップを経て Web サーバーの役割とサービスをインストールします。 Web サーバー (IIS) の役割をインストールした後、サーバーと IIS の再起動は必要ありません。

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>.NET Core Windows Server ホスティング バンドルのインストール

1. ホスティング システムに [.NET Core Windows Server ホスティング バンドル](https://aka.ms/dotnetcore-2-windowshosting)をインストールします。 このバンドルをインストールすることで、.NET Core ランタイム、.NET Core ライブラリ、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。 このモジュールは、IIS と Kestrel サーバーの間にリバース プロキシを作成します。 システムにインターネット接続が設定されていない場合は、.NET Core Windows Server ホスティング バンドルをインストールする前に、[Microsoft Visual C++ 2015 再頒布可能パッケージ](https://www.microsoft.com/download/details.aspx?id=53840)を入手してインストールしてください。

2. システムを再起動するか、コマンド プロンプトから **net stop was /y** の後に続けて **net start w3svc** を実行して、システム PATH への変更を適用します。

> [!NOTE]
> IIS 共有構成を使用する場合は、「[ASP.NET Core Module with IIS Shared Configuration](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)」 (IIS 共有構成の ASP.NET Core モジュール) を参照してください。

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Visual Studio で発行する場合の Web 配置のインストール

[Visual Studio](https://www.visualstudio.com/vs/) の [Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用してアプリケーションを展開する場合は、ホスティング システムに最新バージョンの Web 配置をインストールします。 Web 配置をインストールするには、[Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) を使用するか、[Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=43717)からインストーラーを取得することができます。 WebPI を使用することをお勧めします。 WebPI は、スタンドアロンのセットアップとホスティング プロバイダー向けの構成を提供します。

## <a name="application-configuration"></a>アプリケーション構成

### <a name="enabling-the-iisintegration-components"></a>IISIntegration コンポーネントを有効にする

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

一般的な *Program.cs* は [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。 `CreateDefaultBuilder` は [Kestrel](xref:fundamentals/servers/kestrel) を Web サーバーとして構成し、ベース パスとポートを [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)に構成することで、IIS 統合を有効にします。

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

アプリの依存関係に [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) パッケージへの依存関係を含めます。 *UseIISIntegration* 拡張メソッドを *WebHostBuilder* に追加して、IIS 統合ミドルウェアをアプリに組み込みます。

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

`UseKestrel` と `UseIISIntegration` の両方が必要です。 *UseIISIntegration()* を呼び出すコードはコードの移植性に影響しません。 アプリが IIS の背後で実行されていない場合 (たとえば、アプリが Kestrel で直接実行されている場合)、`UseIISIntegration` は機能しません。

---

ホスティングの詳細については、「[Hosting in ASP.NET Core](xref:fundamentals/hosting)」(ASP.NET Core でのホスティング) を参照してください。

### <a name="iis-options"></a>IIS のオプション

*IISIntegration* サービスのオプションを構成するには、*IISOptions* のサービス構成を *ConfigureServices*に含めます。

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| オプション                         | 既定値 | 設定 |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | `true` の場合、認証ミドルウェアが `HttpContext.User` を設定し、一般的な課題に応答します。 `false` の場合、認証ミドルウェアは ID (`HttpContext.User`) を提供するだけで、`AuthenticationScheme` によって明示的に要求されたときに課題に応答します。 `AutomaticAuthentication` を機能させるためには、IIS で Windows 認証を有効にする必要があります。 |
| `AuthenticationDisplayName`    | `null`  | ログイン ページでユーザーに表示名が表示されるように設定します。 |
| `ForwardClientCertificate`     | `true`  | `true` の場合、`MS-ASPNETCORE-CLIENTCERT` 要求ヘッダーが指定されていると、`HttpContext.Connection.ClientCertificate` が設定されます。 |

### <a name="webconfig"></a>web.config

*web.config* ファイルでは、主に ASP.NET Core モジュールを設定します。 必要に応じて、追加の IIS の構成設定を指定することもできます。 *web.config* の作成、変換、発行は .NET Core Web SDK (`Microsoft.NET.Sdk.Web`) で処理されます。 SDK は、プロジェクト ファイル (*.csproj*) の先頭 (`<Project Sdk="Microsoft.NET.Sdk.Web">`) で設定されています。 SDK によって *web.config* ファイルが変換されないようにするため、`true` に設定した **\<IsTransformWebConfigDisabled>** プロパティをプロジェクト ファイルに追加します。

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

プロジェクトに *web.config* ファイルが存在せず、*dotnet publish* または Visual Studio の公開機能を使用して発行する場合、ファイルは[発行された出力](xref:hosting/directory-structure)内に自動的に作成されます。 プロジェクトに *web.config* ファイルが含まれている場合、そのファイルは [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を構成するための正しい *processPath* と *arguments* を使用して変換され、発行された出力に移行されます。 ファイルに含めた IIS 構成の設定は、変換後も変わりません。

## <a name="create-the-iis-website"></a>IIS Web サイトを作成する

1. [ディレクトリ構造](xref:hosting/directory-structure)に関するページで説明されている、アプリの公開フォルダーとファイルを格納するためのフォルダーを、ターゲットの IIS システムで作成します。

2. 作成したフォルダー内に、stdout ログを保存するための *logs* フォルダーを作成します (起動問題の解決のためにログ作成を有効にする場合)。 ペイロードに *logs* フォルダーを含めてアプリケーションを配置する場合は、このステップをスキップすることができます。 [フォルダーの自動作成には未解決の問題](https://github.com/aspnet/AspNetCoreModule/issues/30)があります。 MSBuild で *log* フォルダーを自動作成するには、プロジェクト ファイルに次の `Target` を追加します。

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
     <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
     <MakeDir Directories="$(PublishUrl)logs" Condition="!Exists('$(PublishUrl)logs')" />
   </Target>
   ```

3. **IIS マネージャー**で新しい Web サイトを作成します。 **[サイト名]** を指定し、**[物理パス]** には作成したアプリの配置フォルダーを設定します。 **[バインド]** の構成を指定して Web サイトを作成します。

4. アプリケーション プールを **[マネージ コードなし]** に設定します。 ASP.NET Core は、別個のプロセスで実行され、ランタイムを管理します。

5. **[Web サイトの追加]** ウィンドウを開きます。

   ![サイトのコンテキスト メニューで [Web サイトの追加] をクリックします。](iis/_static/add-website-context-menu-ws2016.png)

6. Web サイトを構成します。

   ![[Web サイトの追加] のステップでサイト名、物理パス、ホスト名を指定します。](iis/_static/add-website-ws2016.png)

7. **[アプリケーション プール]** パネルで、**[アプリケーション プールの編集]** ウィンドウを開きます。そのためには、Web サイトのアプリ プール上で右クリックし、ポップアップ メニューから **[基本設定]** を選択します。

   ![アプリケーション プールのコンテキスト メニューから [基本設定] を選択します。](iis/_static/apppools-basic-settings-ws2016.png)

8. **[.NET CLR バージョン]** を **[マネージ コードなし]** に設定します。

   ![.NET CLR バージョンとして [マネージ コードなし] を設定します。](iis/_static/edit-apppool-ws2016.png)
     
    注: **[.NET CLR バージョン]** の **[マネージ コードなし]** への設定は、任意です。 ASP.NET Core を使用するためにデスクトップ CLR を読み込む必要はありません。

9. プロセス モデル ID に適切なアクセス許可があることを確認します。

   アプリ プールの既定の ID (**[プロセス モデル]** > **[ID]**) を **ApplicationPoolIdentity** から別の ID に変更した場合は、アプリのフォルダー、データベース、その他の必要なリソースにアクセスするために要求されるアクセス許可が新しい ID に設定されていることを確認します。
   
## <a name="deploy-the-application"></a>アプリケーションを展開する
ターゲット IIS システム上に作成したフォルダーにアプリケーションを展開します。 [Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)は、推奨される展開のメカニズムです。 Web 配置に代わる方法は次のとおりです。

配置用に発行したアプリが実行されていないことを確認します。 アプリが実行中は、*publish* フォルダー内のファイルがロックされます。 ロックされているファイルはコピーできないため、配置は行われません。

### <a name="web-deploy-with-visual-studio"></a>Visual Studio からのアプリの展開
Web 配置に使用する発行プロファイルの作成方法については、「[Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps](xref:publishing/web-publishing-vs#publish-profiles)」 (ASP.NET Core アプリを展開するために Visual Studio と MSBuild 用の発行プロファイルを作成する) のトピックを参照してください。 ホスティング プロバイダーから発行プロファイルまたは発行プロファイルを作成するためのサポートが提供されている場合は、そのプロファイルをダウンロードし、Visual Studio の **[発行]** ダイアログを使用してインポートします。

![[発行] ダイアログ ページ](iis/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Visual Studio 外部での Web 配置
Visual Studio の外部で、コマンド ラインから [Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用することもできます。 詳細については、[Web 配置ツール](/iis/publish/using-web-deploy/use-the-web-deployment-tool)に関するページを参照してください。

### <a name="alternatives-to-web-deploy"></a>Web 配置の代替手段
Web 配置を使用しないか、Visual Studio を使用しない場合は、複数の方法のいずれかを使用して、Xcopy、Robocopy、PowerShell などのホスティング システムにアプリケーションを移行できます。 Visual Studio ユーザーは、[発行サンプル](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md)を利用できます。

## <a name="browse-the-website"></a>Web サイトを閲覧する
![Microsoft Edge ブラウザーに IIS のスタートアップ ページが読み込まれています。](iis/_static/browsewebsite.png)
   
> [!WARNING]
> .NET Core アプリは、IIS と Kestrel サーバー間のリバース プロキシによってホストされます。 リバース プロキシを作成するためには、展開されるアプリケーションのコンテンツ ルート パス (通常は、アプリ ベース パス) に *web.config* ファイルが存在する必要があります。これは IIS に対して指定される Web サイトの物理パスです。 アプリの物理パスには、*my_application.runtimeconfig.json*、*my_application.xml* (XML ドキュメントのコメント)、*my_application.deps.json*などのサブフォルダーを含め、機密性の高いファイルが存在します。 Kestrel に対するリバース プロキシを作成するためには *web.config* ファイルが必要であり、そのため IIS は機密性の高いこれらのファイルやその他のファイルを処理できません。 **したがって、*web.config* ファイルを誤って名前変更したり展開から削除したりしないようにすることが重要です。**

## <a name="data-protection"></a>データの保護

ASP.NET Core アプリケーションは、次の場合にキーリングをメモリに格納します。

* Web サイトが IIS の背後でホストされている。
* データ保護スタックが、永続的なストアにキーリングを格納するように構成されていない。

キーリングがメモリに格納されている場合、アプリを再起動すると次のことが行われます。

* すべてのフォーム認証トークンは無効になります。 
* ユーザーは、次回の要求時に再ログインする必要があります。 
* キーリングで保護されていたデータは保護されなくなります。

> [!WARNING]
> データ保護は、認証で使用されるものを含め、いくつかの ASP.NET ミドルウェアによって使用されます。 独自のコードからデータ保護 API を呼び出さない場合でも、配置スクリプトを使用して、またはコード内にデータ保護を構成する必要があります。 データ保護を構成しない場合、既定でキーはメモリ内に保持され、アプリが再起動すると破棄されます。 再起動すると、Cookie 認証ミドルウェアによって作成された Cookie は無効になり、ユーザーは再度ログインする必要があります。

IIS でのデータ保護を構成するには、次のいずれかの方法を使用する必要があります。

* [powershell スクリプト](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1)を実行し、適切なレジストリ エントリ (たとえば `.\Provision-AutoGenKeys.ps1 DefaultAppPool`) を作成します。 これによりキーはレジストリに格納され、DPAPI を使用して、コンピューター全体に適用するキーによって保護されます。
* ユーザー プロファイルを読み込むための IIS アプリケーション プールを構成します。 この設定は、アプリケーション プールの **[詳細設定]** の **[プロセス モデル]** セクションにあります。 **[ユーザー プロファイルの読み込み]** を `True` に設定します。 これによりキーはユーザー プロファイル ディレクトリに格納され、DPAPI を使用して、アプリ プールで使うユーザー アカウントに固有のキーによって保護されます。
* [ファイル システムをキー リング ストアとして使用](xref:security/data-protection/configuration/overview)するようにアプリ コードを調整します。 X509 証明書を使用してキー リングを保護し、それが信頼された証明書であることを確認します。 証明書が自己署名証明書である場合は、信頼されたルート ストアに配置する必要があります。

Web ファームで IIS を使用する場合:

* すべてのコンピューターがアクセスできるファイル共有を使用します。
* X509 証明書を各コンピューターに配置します。 [コード内にデータ保護](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview)を構成します。

### <a name="1-create-a-data-protection-registry-hive"></a>1.データ保護のレジストリ ハイブを作成する

ASP.NET アプリケーションで使用されるデータ保護キーは、アプリケーション外部のレジストリ ハイブに格納されます。 特定のアプリケーションのキーを保持するには、アプリケーションのアプリ プール用のレジストリ ハイブを作成する必要があります。

IIS のスタンドアロン インストールの場合、ASP.NET Core アプリで使用するアプリ プールごとに、[データ保護の PowerShell スクリプト Provision-AutoGenKeys.ps1](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) を使用できます。 このスクリプトは、HKLM レジストリに特別なレジストリ キーを作成します。そのキーは、ワーカー プロセス アカウントに対してのみ、ACL に追加されます。 キーは DPAPI を使用して保存時に暗号化されます。

Web ファームのシナリオでは、UNC パスを使用してデータ保護キー リングを格納するようにアプリを構成できます。 既定では、データ保護キーは暗号化されません。 このような共有のファイル アクセス許可は、アプリを実行する Windows アカウントに限定されている必要があります。 また、X509 証明書を使用して保存時のキーを保護するように選択することもできます。 ユーザーが証明書をアップデートできるメカニズムを検討している場合、ユーザーの信頼できる証明書ストアに証明書を配置し、ユーザーのアプリが実行されるすべてのコンピューターで証明書を利用できるようにします。 詳細については、「[データ保護の構成](xref:security/data-protection/configuration/overview)」を参照してください。

### <a name="2-configure-the-iis-application-pool-to-load-the-user-profile"></a>2.ユーザー プロファイルを読み込むように IIS アプリケーション プールを構成する

この設定は、アプリ プールの **[詳細設定]** の **[プロセス モデル]** セクションにあります。 [ユーザー プロファイルの読み込み] を `True` に設定します。 これによりキーはユーザー プロファイル ディレクトリに格納され、DPAPI を使用して、アプリ プールで使うユーザー アカウントに固有のキーによって保護されます。

### <a name="3-machine-wide-policy-for-data-protection"></a>3.コンピューター全体に適用するデータ保護ポリシー

データ保護システムでは、データ保護 API を使用するすべてのアプリに対して、[コンピューター全体に適用する既定のポリシー](xref:security/data-protection/configuration/machine-wide-policy)を設定するためのサポートは限定的です。 詳細については、[データ保護](xref:security/data-protection/index)に関するドキュメントを参照してください。

## <a name="configuration-of-sub-applications"></a>サブアプリケーションの構成

ルート アプリケーションの下に追加したサブアプリケーションに、ハンドラーとして ASP.NET Core モジュールを含めることはできません。 サブアプリケーションの *web.config* ファイルにこのモジュールをハンドラーとして追加した場合、サブアプリを閲覧しようとすると、エラーのある構成ファイルを参照する 500.19 (内部サーバー エラー) が返されます。 次の例は、ASP.NET Core サブアプリの発行済み *web.config* ファイルの内容を示しています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

ASP.NET Core アプリの下に ASP.NET Core 以外のサブアプリをホストする場合は、サブアプリの *web.config* ファイルにある継承されたハンドラーを明示的に削除する必要があります。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

*web.config* ファイルを使用して ASP.NET Core モジュールを構成する方法の詳細については、「[Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module)」 (ASP.NET Core モジュールの概要) のトピックと「[ASP.NET Core モジュール構成リファレンス](xref:hosting/aspnet-core-module)」を参照してください。

## <a name="configuration-of-iis-with-webconfig"></a>web.config による IIS の構成

リバース プロキシ構成に適用される IIS 機能については、*web.config*の `<system.webServer>` セクションも、IIS の構成に影響します。 たとえば、IIS が動的な圧縮を使用するようにシステム レベルで構成されている場合でも、アプリの *web.config* ファイルで `<urlCompression>` 要素を使用して、アプリに対してその設定を無効にすることができます。 詳細については、[`<system.webServer>` の構成リファレンス](https://docs.microsoft.com/iis/configuration/system.webServer/)、「[ASP.NET Core モジュール構成の参照](xref:hosting/aspnet-core-module)」および「[ASP.NET Core で IIS のモジュールの使用](xref:hosting/iis-modules)」を参照してください。 分離されたアプリケーション プール (IIS 10.0 以降でサポートされています) で実行する個別アプリに対して環境変数を設定する必要がある場合は、IIS のリファレンス ドキュメントで、[環境変数\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。

## <a name="configuration-sections-of-webconfig"></a>web.config の構成のセクション

`<system.web>`、`<appSettings>`、`<connectionStrings>`、`<location>` の各要素を使用して *web.config* で構成される .NET Framework アプリケーションとは異なり、ASP.NET Core アプリは、その他の構成プロバイダーを使用して構成されます。 詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。

## <a name="application-pools"></a>アプリケーション プール

1 つのシステムで複数の Web サイトをホストする場合は、アプリをそれぞれ専用のアプリケーション プールで実行して、アプリを相互に分離する必要があります。 これは、IIS の **[Web サイトの追加]** ダイアログの既定の設定です。 **[サイト名]** を指定すると、入力したテキストが自動的に **[アプリケーション プール]** テキストボックスに設定されます。 Web サイトを追加するときに、そのサイト名を使用して新しいアプリケーション プールが作成されます。

## <a name="application-pool-identity"></a>アプリケーション プール ID

アプリケーション プール ID アカウントを使用すると、ドメインやローカル アカウントを作成して管理する必要なく、一意のアカウントでアプリケーションを実行できます。 IIS 8.0 以降の IIS 管理者ワーカー プロセス (WAS) は、新しいアプリケーション プールの名前で仮想アカウントを作成し、既定によってアプリ プールのワーカー プロセスをこのアカウントで実行します。 IIS 管理コンソールにあるアプリ プールの **[詳細設定]** で、次の画像に示すように、**ID** が **ApplicationPoolIdentity** を使用するように設定されていることを確認します。

![アプリケーション プールの [詳細設定] ダイアログ](iis/_static/apppool-identity.png)

IIS 管理プロセスは、Windows セキュリティ システムでのアプリ プールの名前を使用して、セキュリティで保護された識別子を作成します。 この ID を使用してリソースを保護することができます。ただし、この ID は実際のユーザー アカウントではなく、Windows ユーザーの管理コンソールに表示されません。

アプリに対する IIS ワーカー プロセスのアクセス許可を昇格させる必要がある場合は、アプリケーションを含むディレクトリのアクセス制御リスト (ACL) を変更する必要があります。

1. エクスプローラーを開き、そのディレクトリに移動します。

2. ディレクトリ上で右クリックし、**[プロパティ]** をクリックします。

3. **[セキュリティ]** タブの **[編集]** ボタンをクリックし、**[追加]** ボタンをクリックします。

4. **[場所]** ボタンをクリックし、使用しているシステムを選択します。

5. **[選択するオブジェクト名を入力してください]** テキストボックスに「**IIS AppPool\DefaultAppPool**」と入力します。

  ![アプリケーション フォルダーの [ユーザーまたはグループの選択] ダイアログ](iis/_static/select-users-or-groups-1.png)

6. **[名前の確認]** ボタンをクリックし、**[OK]** をクリックします。

  ![アプリケーション フォルダーの [ユーザーまたはグループの選択] ダイアログ](iis/_static/select-users-or-groups-2.png)

この操作は、コマンド プロンプトから **ICACLS** ツールを使用して行うこともできます。

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="troubleshooting-tips"></a>トラブルシューティングのヒント

IIS 展開の問題を診断する方法:

* ブラウザーの出力を調べます。
* **イベント ビューアー**でシステムの**アプリケーション** ログを調べます。
* `stdout` ログを有効にします。 **ASP.NET Core モジュール**のログは、*web.config*で `<aspNetCore>`要素の *stdoutLogFile* 属性に指定したパスにあります。この属性の値で指定されたパスにあるすべてのフォルダーが、展開環境に存在する必要があります。 また、*stdoutLogEnabled="true"*を設定する必要があります。 `Microsoft.NET.Sdk.Web` SDK を使用して *web.config* ファイルを作成するアプリでは、既定により *stdoutLogEnabled* が *false* に設定されるため、`stdout` のログ記録を有効にするには、*web.config* ファイルを手動で作成するか、ファイルを編集する必要があります。

一般的なエラーのいくつかは、*startupTimeLimit* (既定値: 120 秒) と *startupRetryCount* (既定値: 2) が経過するまで、ブラウザー、アプリケーション ログ、ASP.NET Core モジュールのログに表示されません。 したがって、モジュールがアプリのプロセスを開始できなかったと判断する前に、完全に 6 分が経過するまで待ってください。

アプリが正常に動作しているかどうかを決定する 1 つの簡単な方法は、アプリを Kestrel で直接実行することです。 アプリがフレームワークに依存する展開 (FDD) として発行された場合は、展開フォルダー (アプリの IIS 物理パス) の `dotnet my_application.dll` を実行します。 アプリが自己完結型の展開 (SCD) として発行された場合は、展開フォルダーにあるアプリケーションの実行可能ファイル (`my_application.exe`) をコマンド プロンプトから直接実行します。 Kestrel が既定のポート 5000 でリッスンしている場合は、`http://localhost:5000/` でアプリを閲覧できます。 アプリが通常は Kestrel エンドポイント アドレスで応答する場合、問題は IIS、ASP.NET Core モジュール、Kestrel の構成に関連している可能性が高く、アプリ自体に関連する可能性は低くなります。

Kestrel サーバーに対する IIS リバース プロキシが正常に動作しているかどうかを確認する 1 つの方法は、[静的ファイル ミドルウェア](xref:fundamentals/static-files)を使用して、*wwwroot* にあるアプリの静的ファイルのスタイルシート、スクリプト、または画像に対する簡単な静的ファイル要求を実行することです。 アプリの静的ファイルを取得できたにもかかわらず、MVC ビューおよびその他のエンドポイントでエラーになる場合は、問題が IIS、ASP.NET Core モジュール、Kestrel 構成に関連している可能性は低く、アプリ自体に関連している可能性が高くなります (たとえば、MVC ルーティングや内部サーバー エラー 500)。

Kestrel が IIS の背後で正常に開始される一方、ローカルで正常に実行できたアプリをシステムで実行できない場合は、*web.config* に、`ASPNETCORE_ENVIRONMENT` を `Development` に設定する環境変数を一時的に追加することができます。 これにより、アプリの起動時に環境をオーバーライドしない限り、アプリをシステムで実行するときに[開発者例外ページ](xref:fundamentals/error-handling)を表示できます。 `ASPNETCORE_ENVIRONMENT` の環境変数をこのように設定する方法は、インターネットに公開されないステージング システムやテスト用システムについてのみ推奨される方法です。 目的が完了したら、必ず *web.config* ファイルから環境変数を削除してください。 *web.config* を使用してリバース プロキシ用の環境変数を設定する方法の詳細については、「[environmentVariables child element of aspNetCore](xref:hosting/aspnet-core-module#setting-environment-variables)」を参照してください。

ほとんどの場合、アプリケーションのログ記録を有効にすると、アプリまたはリバース プロキシに関する問題のトラブルシューティングに役立ちます。 詳細については、[ログ記録](xref:fundamentals/logging/index)に関する説明を参照してください。

最後に紹介するトラブルシューティングのヒントは、開発用コンピューター上の .NET Core SDK またはアプリ内のパッケージ バージョンのいずれかをアップグレード後に実行が失敗するアプリに関するものです。 場合によっては、パッケージに統一性がないと、メジャー アップグレード実行時にアプリが破壊されることがあります。 このような問題の大部分は、次の方法で解決できます。まず、プロジェクトの `bin` フォルダーと `obj` フォルダーを削除し、`%UserProfile%\.nuget\packages\` と `%LocalAppData%\Nuget\v3-cache` にあるパッケージのキャッシュをクリアします。その後、プロジェクトを復元し、システムで以前の展開が完全に削除されたことを確認してから、アプリを再展開します。

> [!TIP]
> パッケージのキャッシュをクリアする便利な方法として、[NuGet.org](https://www.nuget.org/) から *NuGet.exe* ツールを入手し、システム PATH に追加して、コマンド プロンプトから `nuget locals all -clear` を実行します。 *NuGet.exe* を入手せず、コマンド プロンプトから `dotnet nuget locals all --clear` コマンドを実行することもできます。

## <a name="common-errors"></a>一般的なエラー

次の一覧に含まれていないエラーもあります。 この一覧にないエラーが発生した場合は、以下のコメント セクションに詳細なエラー メッセージを記録してください。

### <a name="installer-unable-to-obtain-vc-redistributable"></a>インストーラーが VC++ 再頒布可能パッケージを取得できない

* **インストーラー例外:** 0x80072efd または 0x80072f76 - 特定できないエラー

* **インストーラー ログ例外&#8224;:** エラー 0x80072efd または 0x80072f76: Failed to execute EXE package (EXE パッケージを実行できませんでした)

  &#8224;ログの場所は C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log です。

トラブルシューティング:

* サーバー ホスティング バンドルのインストール時にインストーラーがインターネットにアクセスできない場合、インストーラーは *Microsoft Visual C++ 2015 再頒布可能パッケージ*を取得できず、この例外が発生します。 インストーラーは [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)から入手できます。 インストーラーが失敗した場合、フレームワークに依存する展開 (FDD) をホストするために必要な .NET Core ランタイムを受け取れない可能性があります。 FDD をホストする場合は、[プログラムと機能] でランタイムがインストールされていることを確認してください。 ランタイム インストーラーは [.NET ダウンロード](https://www.microsoft.com/net/download/core)のページから入手できます。 ランタイムのインストール後、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。

### <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>OS のアップグレードによって 32 ビット ASP.NET Core モジュールが削除された

* **アプリケーション ログ:** モジュール DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** を読み込めませんでした。 このデータはエラーです。

トラブルシューティング:

* **C:\Windows\SysWOW64\inetsrv** ディレクトリにある OS ファイルでないファイルは、OS アップグレード時に保持されません。 OS アップグレードより前に ASP.NET Core モジュールをインストールしていた場合、OS アップグレード後に 32 ビット モードで AppPool を実行しようとすると、この問題が発生します。 OS アップグレード後に ASP.NET Core モジュールを修復してください。 「[.NET Core Windows Server ホスティング バンドルのインストール](#install-the-net-core-windows-server-hosting-bundle)」を参照してください。 インストーラーを実行するときに **[修復]** を選択します。

### <a name="platform-conflicts-with-rid"></a>プラットフォームが RID と競合している

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ', ErrorCode = '0x80004005 : ff. (物理ルート 'C:{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"C:\{PATH}\my_application.{exe|dll}" ' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : ff)

* **ASP.NET Core モジュールのログ:** ハンドルされない例外: System.BadImageFormatException: Could not load file or assembly 'my_application.dll'. (ファイルまたはアセンブリ 'my_application.dll' が読み込めませんでした) 正しくない形式のプログラムを読み込もうとしました。

トラブルシューティング:

* Kestrel でアプリケーションをローカルに実行できることを確認します。 プロセスのエラーは、アプリケーション内の問題の結果である可能性があります。 詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。

* RID と競合する `<PlatformTarget>` を *.csproj* に設定していないことを確認します。 たとえば、`x86` の `<PlatformTarget>`を指定した場合は、*dotnet publish -c Release -r win10-x64* を使用するか、*.csproj*で `<RuntimeIdentifiers>` を `win10-x64`に設定するかを問わず、`win10-x64` の RID を使用して発行しないでください。 プロジェクトは警告やエラーにならずに発行されますが、上記のログに記録されたシステムの例外によって失敗します。

* Azure Apps 展開で、アプリケーションをアップグレードして新しいアセンブリを展開しようとしたときにこの例外が発生した場合は、以前の展開からすべてのファイルを手動で削除してください。 アップグレードしたアプリを展開するとき、互換性のないアセンブリが残っていると、`System.BadImageFormatException` 例外が発生します。

### <a name="uri-endpoint-wrong-or-stopped-website"></a>URI のエンドポイントが間違っているか、Web サイトが停止している

* **ブラウザー:** ERR_CONNECTION_REFUSED

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング:

* アプリケーションに対して正しい URI エンドポイントを使用していることを確認します。 バインドを確認します。

* IIS Web サイトが *[停止]* 状態でないことを確認します。

### <a name="corewebengine-or-w3svc-server-features-disabled"></a>CoreWebEngine または W3SVC サーバー機能が無効

* **OS の例外:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module. (ASP.NET Core モジュールを使用するには、IIS 7.0 CoreWebEngine および W3SVC の機能をインストールする必要があります。)

トラブルシューティング:

* 適切な役割と機能を有効にしていることを確認します。 「[IIS 構成](#iis-configuration)」を参照してください。

### <a name="incorrect-website-physical-path-or-application-missing"></a>Web サイト物理パスが間違っているか、アプリケーションがない

* **ブラウザー:** 403 許可されていません - アクセスが拒否されました **--または--** 403.14 許可されていません - Web サーバーは、このディレクトリの内容の一覧を表示しないように構成されています。

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング:

* IIS Web サイトの**基本設定**と物理アプリケーション フォルダーを確認します。 アプリケーションが IIS Web サイトの**物理パス**にあるフォルダー内に配置されていることを確認します。

### <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>役割が正しくない、モジュールがインストールされていない、または不適切なアクセス許可

* **ブラウザー:** 500.19 内部サーバー エラー - ページに関連する構成データが無効であるため、要求されたページにアクセスできません。

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング:

* 適切な役割を有効にしていることを確認します。 「[IIS 構成](#iis-configuration)」を参照してください。

* **[プログラムと機能]** をチェックし、**Microsoft ASP.NET Core モジュール**がインストールされていることを確認します。 インストールされているプログラムの一覧に **Microsoft ASP.NET Core モジュール**が表示されない場合は、モジュールをインストールします。 「[.NET Core Windows Server ホスティング バンドルのインストール](#install-the-net-core-windows-server-hosting-bundle)」を参照してください。

* **[アプリケーション プール]**、**[プロセス モデル]**、**[ID]** が **ApplicationPoolIdentity** に設定されていることを確認します。または、アプリの展開フォルダーにアクセスするための正しいアクセス許可がカスタム ID に設定されていることを確認します。

### <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>processPath の誤り、PATH 変数の欠如、ホスティング バンドルが未インストール、システムまたは IIS が再起動されていない、VC++ 再頒布可能パッケージが未インストール、dotnet.exe アクセス違反

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\my_application.exe" ', ErrorCode = '0x80070002 : 0. (物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '".\my_application.exe" ' でプロセスを開始できませんでした。ErrorCode = '0x80070002 : 0)

* **ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です

トラブルシューティング:

* Kestrel でアプリケーションをローカルに実行できることを確認します。 プロセスのエラーは、アプリケーション内の問題の結果である可能性があります。 詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。

* *web.config* で `<aspNetCore>` 要素の *processPath* 属性を調べ、フレームワークに依存する展開 (FDD) を示す *dotnet*、または自己完結型の展開 (SCD) を示す *.\my_application.exe* になっていることを確認します。

* FDD の場合、PATH 設定で *dotnet.exe* にアクセスできていない可能性があります。 *C:\Program Files\dotnet\* がシステムの PATH 設定に含まれていることを確認します。

* FDD では、アプリケーション プールのユーザー ID で *dotnet.exe* にアクセスできていない可能性があります。 AppPool ユーザー ID に、*C:\Program Files\dotnet* ディレクトリへのアクセス許可が設定されていることを確認します。 *C:\Program Files\dotnet* とアプリケーション ディレクトリに、AppPool ユーザー ID に対する拒否ルールが構成されていないことを確認します。

* FDD を配置し、IIS を再起動せずに .NET Core をインストールした可能性があります。 サーバーを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。

* ホスト システム .NET Core ランタイムをインストールせずに、FDD を配置した可能性があります。 FDD を配置しようとしていて、.NET Core ランタイムをインストールしていない場合は、**.NET Core Windows Server ホスティング バンドル インストーラー**をシステムで実行します。 「[.NET Core Windows Server ホスティング バンドルのインストール](#install-the-net-core-windows-server-hosting-bundle)」を参照してください。 インターネットに接続せずに、.NET Core ランタイムをシステムにインストールする場合は、[.NET ダウンロード](https://www.microsoft.com/net/download/core)のページからランタイムを入手し、ホスティング バンドル インストーラーを実行して ASP.NET Core モジュールをインストールします。 インストールを完了するために、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。

* FDD を配置し、システム/IIS を再起動せずに .NET Core をインストールした可能性があります。 システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。

* FDD を配置し、*Microsoft Visual C++ 2015 再頒布可能パッケージ (x64)* がシステムにインストールされていない可能性があります。 インストーラーは [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)から入手できます。

### <a name="incorrect-arguments-of-aspnetcore-element"></a>\<aspNetCore\> 要素の引数の誤り

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081. (物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"dotnet" .\my_application.dll' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : 80008081)

* **ASP.NET Core モジュールのログ:** The application to execute does not exist: 'PATH\my_application.dll' (実行するアプリケーションが存在しません: 'PATH\my_application.dll')

トラブルシューティング:

* Kestrel でアプリケーションをローカルに実行できることを確認します。 プロセスのエラーは、アプリケーション内の問題の結果である可能性があります。 詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。

* *web.config* で `<aspNetCore>` 要素の *arguments* 属性を調べ、次のいずれかになっていることを確認します。(a) フレームワークに依存する展開 (FDD) の場合は *.\my_applciation.dll*、または (b) 自己完結型の展開 (SCD) の場合は、未指定の空の文字列 (*arguments=""*) か、アプリの引数のリスト (*arguments="arg1, arg2, ..."*)。

### <a name="missing-net-framework-version"></a>.NET Framework バージョンの欠落

* **ブラウザー:** 502.3 ゲートウェイが正しくありません - There was a connection error while trying to route the request. (要求のルーティング中に接続エラーが発生しました。)

* **アプリケーション ログ:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081. (ErrorCode = 物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"dotnet" .\my_application.dll' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : 80008081)

* **ASP.NET Core モジュールのログ:** メソッド、ファイル、またはアセンブリの欠如の例外。 例外で特定されたメソッド、ファイル、またはアセンブリは、.NET Framework のメソッド、ファイル、またはアセンブリです。

トラブルシューティング:

* システムにない .NET Framework のバージョンをインストールします。

* フレームワークに依存する展開 (FDD) では、正しいランタイムがシステムにインストールされていることを確認します。 プロジェクトを 1.1 から 2.0 にアップグレードしてからホスティング システムに展開し、この例外を受け取った場合は、ホスティング システムに 2.0 のフレームワークをインストールします。

### <a name="stopped-application-pool"></a>アプリケーション プールの停止

* **ブラウザー:** 503 サービスを利用できません

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング

* アプリケーション プールが *[停止]* 状態でないことを確認します。

### <a name="iis-integration-middleware-not-implemented"></a>IIS 統合ミドルウェアが未実装

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not response or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4' (物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"C:\{PATH}\my_application.{exe|dll}" ' でプロセスを作成しましたが、クラッシュしたか、応答しなかったか、指定されたポート '{PORT}' でリッスンしていません。ErrorCode = '0x800705b4')

* **ASP.NET Core モジュールのログ:** ログ ファイルが作成され、正常動作を示しています。

トラブルシューティング

* Kestrel でアプリをローカルに実行できることを確認します。 プロセスのエラーは、アプリ内の問題の結果である可能性があります。 詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。

* アプリケーションの *WebHostBuilder()* (ASP.NET Core 1.x) で *.UseIISIntegration()* メソッドを呼び出して IIS 統合ミドルウェアを正しく参照していることを、あるいは `CreateDefaultBuilder` メソッド (ASP.NET Core 2.x) を使用していることを確認します。 詳細については、「[ASP.NET Core でのホスティング](xref:fundamentals/hosting)」を参照してください。

### <a name="sub-application-includes-a-handlers-section"></a>サブアプリケーションに \<handlers\> セクションが含まれている

* **ブラウザー:** HTTP エラー 500.19 - 内部サーバー エラー

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルが作成され、ルート アプリケーションの正常動作を示しています。 サブアプリケーションにログ ファイルは作成されません。

トラブルシューティング

* サブアプリの *web.config* ファイルに `<handlers>` セクションが含まれていないことを確認します。

### <a name="application-configuration-general-issue"></a>アプリケーション構成の一般的な問題

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not response or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4' (物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"C:\{PATH}\my_application.{exe|dll}" ' でプロセスを作成しましたが、クラッシュしたか、応答しなかったか、指定されたポート '{PORT}' でリッスンしていません。ErrorCode = '0x800705b4')

* **ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です

トラブルシューティング

* この一般例外はプロセスを開始できなかったことを示し、ほとんどの場合はアプリケーション構成の問題が原因です。 [ディレクトリ構造](xref:hosting/directory-structure)に関する説明を参照して、アプリの展開済みファイルとフォルダーが適切であること、アプリの構成ファイルが存在し、そのファイルにアプリと環境に応じた正しい設定が含まれていることを確認します。 詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。

## <a name="resources"></a>リソース

* [ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core モジュール構成リファレンス](xref:hosting/aspnet-core-module)
* [IIS モジュールと ASP.NET Core の使用](xref:hosting/iis-modules)
* [ASP.NET Core の概要](../index.md)
* [Microsoft IIS 公式サイト](https://www.iis.net/)
* [Microsoft TechNet ライブラリ: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
