---
title: "IIS を使用した Windows での ASP.NET Core のホスト"
author: guardrex
description: "Windows Server インターネット インフォメーション サービス (IIS) での ASP.NET Core アプリをホストする方法を説明します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: 1df438af2394f41b686413cd1ce5ad73a9416ec5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>IIS を使用した Windows での ASP.NET Core のホスト

執筆者: [Luke Latham](https://github.com/guardrex)、[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>サポートされるオペレーティング システム

次のオペレーティング システムがサポートされています。

* Windows 7 およびそれ以降
* Windows Server 2008 R2 およびそれ以降&#8224;

&#8224;概念的には、このドキュメントで説明する IIS 構成は、Nano Server IIS で ASP.NET Core アプリをホストする場合にも当てはまりますが、詳細な手順については「[Nano Server の ASP.NET Core と IIS](xref:tutorials/nano-server)」を参照してください。

[HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (以前の名称は [WebListener](xref:fundamentals/servers/weblistener)) が、IIS を含むリバース プロキシ構成で動作しません。 [Kestrel サーバー](xref:fundamentals/servers/kestrel)を使用します。

## <a name="iis-configuration"></a>IIS 構成

**Web サーバー (IIS)** の役割を有効にし、役割のサービスを設定します。

### <a name="windows-desktop-operating-systems"></a>Windows デスクトップ オペレーティング システム

**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。 **[インターネット インフォメーション サービス]** と **[Web 管理ツール]** のグループを開きます。 **[IIS 管理コンソール]** チェック ボックスをオンにします。 **[World Wide Web サービス]** チェック ボックスをオンにします。 **[World Wide Web サービス]** の既定の機能をそのまま使用するか、IIS 機能をカスタマイズします。

![[Windows の機能] で [IIS 管理コンソール] と [World Wide Web サービス] を選択します。](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Windows Server オペレーティング システム

サーバー オペレーティング システムでは、**[管理]** メニューから**役割と機能の追加**ウィザードを使用するか、**サーバー マネージャー**にあるリンクを使用します。 **[サーバーの役割]** のステップで、**[Web サーバー (IIS)]** チェック ボックスをオンにします。

![[サーバーの役割の選択] のステップで Web サーバー IIS の役割を選択します。](index/_static/server-roles-ws2016.png)

**[役割サービス]** のステップで、希望する IIS の役割サービスを選択するか、既定の役割サービスをそのまま使用します。

![[役割サービスの選択] のステップで既定の役割サービスを選択します。](index/_static/role-services-ws2016.png)

**[確認]** のステップを経て Web サーバーの役割とサービスをインストールします。 Web サーバー (IIS) の役割をインストールした後、サーバーと IIS の再起動は必要ありません。

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>.NET Core Windows Server ホスティング バンドルのインストール

1. ホスティング システムに [.NET Core Windows Server ホスティング バンドル](https://aka.ms/dotnetcore-2-windowshosting)をインストールします。 このバンドルをインストールすることで、.NET Core ランタイム、.NET Core ライブラリ、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。 このモジュールは、IIS と Kestrel サーバーの間にリバース プロキシを作成します。 システムにインターネット接続が設定されていない場合は、.NET Core Windows Server ホスティング バンドルをインストールする前に、[Microsoft Visual C++ 2015 再頒布可能パッケージ](https://www.microsoft.com/download/details.aspx?id=53840)を入手してインストールしてください。

2. システムを再起動するか、コマンド プロンプトから **net stop was /y** の後に続けて **net start w3svc** を実行して、システム PATH への変更を適用します。

> [!NOTE]
> IIS 共有構成の詳細については、「[ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)」 (IIS 共有構成の ASP.NET Core モジュール) を参照してください。

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Visual Studio で発行する場合の Web 配置のインストール

[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用してサーバーにアプリを展開する場合、Web 配置の最新バージョンをサーバーにインストールします。 Web 配置をインストールするには、[Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) を使用するか、[Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=43717)からインストーラーを取得します。 WebPI を使用することをお勧めします。 WebPI は、スタンドアロンのセットアップとホスティング プロバイダー向けの構成を提供します。

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

`UseKestrel` と `UseIISIntegration` の両方が必要です。 `UseIISIntegration` を呼び出すコードはコードの移植性に影響しません。 アプリが IIS の背後で実行されていない場合 (たとえば、アプリが Kestrel で直接実行されている場合)、`UseIISIntegration` は機能しません。

---

ホスティングの詳細については、「[Hosting in ASP.NET Core](xref:fundamentals/hosting)」(ASP.NET Core でのホスティング) を参照してください。

### <a name="iis-options"></a>IIS のオプション

IIS オプションを構成するには、`IISOptions` 用のサービス構成を `ConfigureServices` に含めます。

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

*web.config* ファイルの主な目的は、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を構成することです。 必要に応じて、追加の IIS の構成設定を指定することもできます。 *web.config* の作成、変換、発行は .NET Core Web SDK (`Microsoft.NET.Sdk.Web`) で処理されます。 SDK は、プロジェクト ファイル `<Project Sdk="Microsoft.NET.Sdk.Web">` の先頭で設定されています。 SDK によって *web.config* ファイルが変換されないようにするため、`true` に設定した **\<IsTransformWebConfigDisabled>** プロパティをプロジェクト ファイルに追加します。

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

プロジェクトに *web.config* ファイルが含まれている場合、そのファイルは [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を構成するための正しい *processPath* と *arguments* を使用して変換され、[発行された出力](xref:host-and-deploy/directory-structure)に移行されます。 変換によりファイル内の IIS 構成の設定が変わることはありません。

### <a name="webconfig-location"></a>web.config の場所

.NET Core アプリは、IIS と Kestrel サーバー間のリバース プロキシによってホストされます。 リバース プロキシを作成するためには、展開されるアプリのコンテンツ ルート パス (通常は、アプリ ベース パス) に *web.config* ファイルが存在する必要があります。これは IIS に対して指定される Web サイトの物理パスです。 *Web.config* ファイルは、Web 配置を使用して複数のアプリの発行を有効にするため、アプリのルートで必要です。

アプリの物理パスには、*\<assembly_name>.runtimeconfig.json*、*\<assembly_name>.xml* (XML ドキュメントのコメント)、*\<assembly_name>.deps.json* などのサブフォルダーを含め、機密性の高いファイルが存在します。 *web.config* ファイルが存在し、サイトを構成している場合、IIS がこれらの機密性の高いファイルが使用されるのを防止します。 **したがって、*web.config* ファイルを誤って名前変更したり展開から削除したりしないようにすることが重要です。**

## <a name="create-the-iis-website"></a>IIS Web サイトを作成する

1. [ディレクトリ構造](xref:host-and-deploy/directory-structure)に関するページで説明されている、アプリの公開フォルダーとファイルを格納するためのフォルダーを、ターゲットの IIS システムで作成します。

2. そのフォルダー内で、stdout ログが有効になっている場合に stdout ログを保持するための *logs* フォルダーを作成します。 ペイロードに *logs* フォルダーを含めてアプリを配置する場合は、この手順をスキップします。 MSBuild で *logs* フォルダーを作成する手順については、[ディレクトリ構造](xref:host-and-deploy/directory-structure)に関するトピックを参照してください。

3. **IIS マネージャー**で新しい Web サイトを作成します。 **[サイト名]** を指定し、**[物理パス]** にはアプリの配置フォルダーを設定します。 **[バインド]** の構成を指定して Web サイトを作成します。

4. アプリケーション プールを **[マネージ コードなし]** に設定します。 ASP.NET Core は、別個のプロセスで実行され、ランタイムを管理します。

5. **[Web サイトの追加]** ウィンドウを開きます。

   ![サイトのコンテキスト メニューで [Web サイトの追加] を選択します。](index/_static/add-website-context-menu-ws2016.png)

6. Web サイトを構成します。

   ![[Web サイトの追加] のステップでサイト名、物理パス、ホスト名を指定します。](index/_static/add-website-ws2016.png)

7. **[アプリケーション プール]** パネルで、**[アプリケーション プールの編集]** ウィンドウを開きます。そのためには、Web サイトのアプリ プール上で右クリックし、ポップアップ メニューから **[基本設定]** を選択します。

   ![アプリケーション プールのコンテキスト メニューから [基本設定] を選択します。](index/_static/apppools-basic-settings-ws2016.png)

8. **[.NET CLR バージョン]** を **[マネージ コードなし]** に設定します。

   ![.NET CLR バージョンとして [マネージ コードなし] を設定します。](index/_static/edit-apppool-ws2016.png)
     
    注: **[.NET CLR バージョン]** の **[マネージ コードなし]** への設定は、任意です。 ASP.NET Core を使用するためにデスクトップ CLR を読み込む必要はありません。

9. プロセス モデル ID に適切なアクセス許可があることを確認します。

   アプリ プールの既定の ID (**[プロセス モデル]** > **[ID]**) を **ApplicationPoolIdentity** から別の ID に変更した場合は、アプリのフォルダー、データベース、その他の必要なリソースにアクセスするために要求されるアクセス許可が新しい ID に設定されていることを確認します。
   
## <a name="deploy-the-app"></a>アプリを配置する

ターゲット IIS システム上に作成したフォルダーにアプリを展開します。 [Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)は、推奨される展開のメカニズムです。

配置用に発行したアプリが実行されていないことを確認します。 アプリが実行中は、*publish* フォルダー内のファイルがロックされます。 ロックされたファイルを上書きすることはできません。 展開でロックされているファイルをリリースするには、次の方法でアプリ プールを停止します。

* サーバー上の IIS マネージャーで手動により停止します。
* Web 配置を使用し、プロジェクト ファイル内の `Microsoft.NET.Sdk.Web` を参照して停止します。 *app_offline.htm* ファイルは、Web アプリのディレクトリのルートに配置されます。 ファイルが存在する場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、展開中に *app_offline.htm* ファイルを提供します。 詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#appofflinehtm)」を参照してください。
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

### <a name="web-deploy-with-visual-studio"></a>Visual Studio からのアプリの展開

Web 配置に使用する発行プロファイルの作成方法については、「[Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)」 (ASP.NET Core アプリ展開用の Visual Studio の発行プロファイル) のトピックを参照してください。 ホスティング プロバイダーから発行プロファイルまたは発行プロファイルを作成するためのサポートが提供されている場合は、そのプロファイルをダウンロードし、Visual Studio の **[発行]** ダイアログを使用してインポートします。

![[発行] ダイアログ ページ](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Visual Studio 外部での Web 配置

Visual Studio の外部で、コマンド ラインから [Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用することもできます。 詳細については、[Web 配置ツール](/iis/publish/using-web-deploy/use-the-web-deployment-tool)に関するページを参照してください。

### <a name="alternatives-to-web-deploy"></a>Web 配置の代替手段

Xcopy、Robocopy、PowerShell などの任意の方法を使用して、ホスティング システムにアプリを移動します。 Visual Studio ユーザーは、[発行サンプル](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md)を利用できます。

## <a name="browse-the-website"></a>Web サイトを閲覧する

![Microsoft Edge ブラウザーに IIS のスタートアップ ページが読み込まれています。](index/_static/browsewebsite.png)

## <a name="data-protection"></a>データの保護

データ保護は、認証で使用されるものを含め、いくつかの ASP.NET ミドルウェアによって使用されます。 データ保護 API がユーザーのコードから呼び出されない場合でも、配置スクリプトを使用するかユーザー コードで、永続的なキー ストアを作成するようにデータ保護を構成する必要があります。 データ保護を構成しない場合、既定でキーはメモリ内に保持され、アプリが再起動すると破棄されます。

キーリングがメモリに格納されている場合、アプリを再起動すると次のことが行われます。

* すべてのフォーム認証トークンは無効になります。 
* ユーザーは、次回の要求時に再度サインインする必要があります。 
* キーリングで保護されているデータは、いずれも復号化できなくなります。

IIS でのデータ保護を構成するには、次の**いずれか**の方法を使用する必要があります。

### <a name="create-a-data-protection-registry-hive"></a>データ保護のレジストリ ハイブを作成する

ASP.NET アプリで使用されるデータ保護キーは、アプリ外部のレジストリ ハイブに格納されます。 特定のアプリのキーを保持するには、アプリ プール用のレジストリ ハイブを作成する必要があります。

非 Web ファーム IIS のスタンドアロン インストールの場合、ASP.NET Core アプリで使用するアプリ プールごとに、[データ保護の PowerShell スクリプト Provision-AutoGenKeys.ps1](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) を使用できます。 このスクリプトは、HKLM レジストリに特別なレジストリ キーを作成します。そのキーは、ワーカー プロセス アカウントに対してのみ、ACL に追加されます。 キーは保存時に、DPAPI を使用して、コンピューター全体に適用するキーによって暗号化されます。

Web ファームのシナリオでは、UNC パスを使用してデータ保護キー リングを格納するようにアプリを構成できます。 既定では、データ保護キーは暗号化されません。 このような共有のファイルのアクセス許可が、アプリを実行する Windows アカウントに限定されていることを確認します。 また、保存時のキーを保護するために、X509 証明書を使用することもできます。 ユーザーが証明書をアップデートできるメカニズムを検討している場合、ユーザーの信頼できる証明書ストアに証明書を配置し、ユーザーのアプリが実行されるすべてのコンピューターで証明書を利用できるようにします。 詳細については、「[データ保護の構成](xref:security/data-protection/configuration/overview)」を参照してください。

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a>ユーザー プロファイルを読み込むように IIS アプリケーション プールを構成する

この設定は、アプリ プールの **[詳細設定]** の **[プロセス モデル]** セクションにあります。 [ユーザー プロファイルの読み込み] を `True` に設定します。 これによりキーはユーザー プロファイル ディレクトリに格納され、DPAPI を使用して、アプリ プールで使うユーザー アカウントに固有のキーによって保護されます。

### <a name="use-the-file-system-as-a-key-ring-store"></a>ファイル システムをキー リング ストアとして使用する

[ファイル システムをキー リング ストアとして使用](xref:security/data-protection/configuration/overview)するようにアプリ コードを調整します。 X509 証明書を使用してキー リングを保護し、その証明書が信頼された証明書であることを確認します。 証明書が自己署名証明書である場合は、信頼されたルート ストアにその証明書を配置します。

Web ファームで IIS を使用する場合:

* すべてのコンピューターがアクセスできるファイル共有を使用します。
* X509 証明書を各コンピューターに配置します。 [コード内にデータ保護](xref:security/data-protection/configuration/overview)を構成します。

### <a name="set-a-machine-wide-policy-for-data-protection"></a>コンピューター全体に適用するデータ保護ポリシーを設定する

データ保護システムでは、データ保護 API を使用するすべてのアプリに対して、[コンピューター全体に適用する既定のポリシー](xref:security/data-protection/configuration/machine-wide-policy)を設定するためのサポートは限定的です。 詳細については、[データ保護](xref:security/data-protection/index)に関するドキュメントを参照してください。

## <a name="configuration-of-sub-applications"></a>サブアプリケーションの構成

ルート アプリの下に追加したサブアプリに、ハンドラーとして ASP.NET Core モジュールを含めることはできません。 このモジュールをサブアプリの *web.config* ファイルにハンドラーとして追加すると、サブアプリを閲覧しようとすると、エラーのある構成ファイルを参照する 500.19 (内部サーバー エラー) が返されます。 次の例は、ASP.NET Core サブアプリの発行済み *web.config* ファイルの内容を示しています。

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
      <remove name="aspNetCore"/>
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

IIS の構成は、リバース プロキシ構成に適用されるこれらの IIS 機能の *web.config*の **\<system.webServer>** セクションの影響を受けます。 IIS が動的な圧縮を使用するようにシステム レベルで構成されている場合、アプリの *web.config* ファイルで **\<urlCompression>** 要素を使用して、アプリに対してその設定を無効にすることができます。 詳細については、[\<system.webServer> の構成リファレンス](https://docs.microsoft.com/iis/configuration/system.webServer/)、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」および「[IIS モジュールと ASP.NET Core の使用](xref:host-and-deploy/iis/modules)」を参照してください。 分離されたアプリ プール (IIS 10.0 以降でサポートされています) で実行する個別アプリに対して環境変数を設定する必要がある場合は、IIS のリファレンス ドキュメントで、[環境変数\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。

## <a name="configuration-sections-of-webconfig"></a>web.config の構成のセクション

*web.config* の ASP.NET フレームワーク アプリの構成セクションは、ASP.NET Core アプリの構成では使用されません。

* **\<system.web >**
* **\<appSettings>**
* **\<connectionStrings>**
* **\<location>**

ASP.NET Core アプリは、他の構成プロバイダーを使用して構成されます。 詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。

## <a name="application-pools"></a>アプリケーション プール

1 つのシステムで複数の Web サイトをホストする場合は、アプリをそれぞれ専用のアプリ プールで実行して、アプリを相互に分離します。 IIS の **[Web サイトの追加]** ダイアログはこの構成の既定です。 **[サイト名]** を指定すると、入力したテキストが自動的に **[アプリケーション プール]** テキストボックスに設定されます。 Web サイトを追加するときに、そのサイト名を使用して新しいアプリ プールが作成されます。

## <a name="application-pool-identity"></a>アプリケーション プール ID

アプリ プール ID アカウントを使用すると、ドメインやローカル アカウントを作成して管理する必要なく、一意のアカウントでアプリを実行できます。 IIS 8.0 以降の IIS 管理者ワーカー プロセス (WAS) は、新しいアプリ プールの名前で仮想アカウントを作成し、既定によってアプリ プールのワーカー プロセスをこのアカウントで実行します。 IIS 管理コンソールにあるアプリ プールの **[詳細設定]** で、**ID** が **ApplicationPoolIdentity** を使用するように設定されていることを確認します。

![アプリケーション プールの [詳細設定] ダイアログ](index/_static/apppool-identity.png)

IIS 管理プロセスは、Windows セキュリティ システムでのアプリ プールの名前を使用して、セキュリティで保護された識別子を作成します。 この ID を使用してリソースを保護することができます。ただし、この ID は実際のユーザー アカウントではなく、Windows ユーザーの管理コンソールに表示されません。

アプリに対する IIS ワーカー プロセスのアクセス許可を昇格させる必要がある場合は、アプリを含むディレクトリのアクセス制御リスト (ACL) を変更します。

1. エクスプローラーを開き、そのディレクトリに移動します。

2. そのディレクトリを右クリックし、**[プロパティ]** を選択します。

3. **[セキュリティ]** タブの **[編集]** ボタンを選択し、**[追加]** ボタンをクリックします。

4. **[場所]** ボタンを選択し、システムが選択されていることを確認します。

5. **[選択するオブジェクト名を入力してください]** テキストボックスに「**IIS AppPool\DefaultAppPool**」と入力します。

   ![アプリ フォルダーの [ユーザーまたはグループの選択] ダイアログ](index/_static/select-users-or-groups-1.png)

6. **[名前の確認]** を選択します。 **[OK]** を選択します。

   ![アプリ フォルダーの [ユーザーまたはグループの選択] ダイアログ](index/_static/select-users-or-groups-2.png)

これは、**ICACLS** ツールを使用してコマンド プロンプトからも実行できます。

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a>その他の技術情報

* [IIS での ASP.NET Core のトラブルシューティング](xref:host-and-deploy/iis/troubleshoot)
* [Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)
* [ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)
* [IIS モジュールと ASP.NET Core の使用](xref:host-and-deploy/iis/modules)
* [ASP.NET Core の概要](../index.md)
* [Microsoft IIS 公式サイト](https://www.iis.net/)
* [Microsoft TechNet ライブラリ: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
