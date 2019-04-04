---
title: ASP.NET Core での Windows 認証を構成します。
author: scottaddie
description: ASP.NET core で IIS Express、IIS、および HTTP.sys を使用して Windows 認証を構成する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 02/25/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 15fc41efba77f88fc8129f875b85836ac1b5f886
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833697"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>ASP.NET Core での Windows 認証を構成します。

によって[Scott Addie](https://twitter.com/Scott_Addie)と[Luke Latham](https://github.com/guardrex)

[Windows 認証](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)でホストされている ASP.NET Core アプリ用に構成できます[IIS](xref:host-and-deploy/iis/index)または[HTTP.sys](xref:fundamentals/servers/httpsys)します。

Windows 認証は、ASP.NET Core アプリのユーザーを認証するオペレーティング システムに依存します。 ユーザーを識別するために Active Directory ドメインの id または Windows アカウントを使用して、企業ネットワークで、サーバーの実行時に、Windows 認証を使用できます。 Windows 認証は、同じ Windows ドメインに属しているユーザー、クライアント アプリ、および web サーバーのイントラネット環境に最適です。

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>ASP.NET Core アプリで Windows 認証を有効にします。

**Web アプリケーション**Windows 認証をサポートする Visual Studio または .NET Core CLI を使用して利用可能なテンプレートを構成することができます。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a>新しいプロジェクトに Windows 認証アプリ テンプレートを使用

Visual Studio:

1. 新規作成**ASP.NET Core Web アプリケーション**します。
1. 選択**Web アプリケーション**テンプレートの一覧から。
1. 選択、**認証の変更**ボタンをクリックし、選択**Windows 認証**します。

アプリを実行します。 ユーザー名は、レンダリングされたアプリのユーザー インターフェイスに表示されます。

### <a name="manual-configuration-for-an-existing-project"></a>既存のプロジェクトの手動構成

プロジェクトのプロパティは、Windows 認証を有効にして、匿名認証を無効化を使用します。

1. Visual studio のプロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**プロパティ**します。
1. **[デバッグ]** タブを選択します。
1. チェック ボックスをオフ**匿名認証を有効にする**します。
1. チェック ボックスをオン**Windows 認証を有効にする**します。

プロパティを構成する代わりに、`iisSettings`のノード、 *launchSettings.json*ファイル。

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

使用して、 **Windows 認証**アプリ テンプレート。

実行、[新しい dotnet](/dotnet/core/tools/dotnet-new)コマンドと、`webapp`引数 (ASP.NET Core Web アプリ) と`--auth Windows`スイッチします。

```console
dotnet new webapp --auth Windows
```

---

既存のプロジェクトを変更する場合は、プロジェクト ファイルにはへのパッケージ参照が含まれていることを確認、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)**または**、 [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet パッケージ。

## <a name="enable-windows-authentication-with-iis"></a>IIS での Windows 認証を有効にします。

IIS を使用して、 [ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)ASP.NET Core アプリをホストします。 使用した IIS の Windows 認証が構成されている、 *web.config*ファイル。 以下のセクションで表示する方法。

* ローカルの提供*web.config*ファイルをアプリが展開されると、サーバーで Windows 認証をアクティブにします。
* IIS マネージャーを使用して、構成、 *web.config*サーバーに既に展開されている ASP.NET Core アプリのファイル。

### <a name="iis-configuration"></a>IIS 構成

これをいない場合は、ASP.NET Core アプリをホストする IIS を有効にします。 詳細については、「 <xref:host-and-deploy/iis/index> 」を参照してください。

Windows 認証の IIS の役割サービスを有効にします。 詳細については、[(手順 2 参照)、IIS の役割サービスで Windows 認証を有効にする](xref:host-and-deploy/iis/index#iis-configuration)を参照してください。

IIS 統合ミドルウェアは、既定で自動的に要求の認証に構成されます。 詳細については、次を参照してください。 [ASP.NET Core の IIS と Windows ホスト。IIS のオプション (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options)します。

ASP.NET Core モジュールは、既定では、アプリに Windows 認証トークンを転送するように構成されます。 詳細については、次を参照してください。 [ASP.NET Core モジュール構成リファレンス。AspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)します。

### <a name="create-a-new-iis-site"></a>新しい IIS サイトを作成します。

名前とフォルダーを指定し、新しいアプリケーション プールを作成することを許可します。

### <a name="enable-windows-authentication-for-the-app-in-iis"></a>IIS でアプリケーションの Windows 認証を有効にします。

使用**か**の次の方法。

* [アプリを発行する前に開発側の構成](#development-side-configuration-with-a-local-webconfig-file)(*推奨*)
* [アプリを発行した後、サーバー側の構成](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a>ローカルの web.config ファイルを使用して開発側の構成

次の手順に従います**する前に**する[発行し、プロジェクトを配置](#publish-and-deploy-your-project-to-the-iis-site-folder)します。

次の追加*web.config*ファイルをプロジェクトのルート。

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

プロジェクトが SDK によって公開されると (なし、`<IsTransformWebConfigDisabled>`プロパティに設定`true`プロジェクト ファイル内)、公開された*web.config*ファイルが含まれています、`<location><system.webServer><security><authentication>`セクション。 詳細については、`<IsTransformWebConfigDisabled>`プロパティを参照してください<xref:host-and-deploy/iis/index#webconfig-file>します。

#### <a name="server-side-configuration-with-the-iis-manager"></a>IIS マネージャーでサーバー側の構成

次の手順に従います**後**する[発行し、プロジェクトを配置](#publish-and-deploy-your-project-to-the-iis-site-folder)します。

1. IIS マネージャーで、下にある IIS サイトを選択して、**サイト**のノード、**接続**サイドバーです。
1. ダブルクリック**認証**で、 **IIS**領域。
1. 選択**匿名認証**します。 選択**を無効にする**で、**アクション**サイドバーです。
1. 選択**Windows 認証**します。 選択**を有効にする**で、**アクション**サイドバーです。

これらのアクションが実行したときに、IIS マネージャーは、アプリを変更します*web.config*ファイル。 A`<system.webServer><security><authentication>`ノードが更新された設定を使用した追加`anonymousAuthentication`と`windowsAuthentication`:

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

`<system.webServer>`セクションに追加、 *web.config*ファイルを IIS マネージャーでは、アプリの外部で`<location>`は .NET Core SDK により、アプリが公開されるときに追加されたセクションです。 外部のセクションが追加されるため、`<location>`ノード、いずれかで、設定が継承されます[サブ アプリ](xref:host-and-deploy/iis/index#sub-applications)現在のアプリにします。 継承を防ぐためには、移動、追加した`<security>`内のセクション、`<location><system.webServer>`セクション、SDK が提供されます。

IIS マネージャーを使用するには、IIS の構成を追加する、影響を受けるのみアプリの*web.config*サーバー上のファイル。 場合、アプリの後続の配置は、サーバーの設定を上書き可能性があります、サーバーのコピーの*web.config*はプロジェクトの置き換え*web.config*ファイル。 使用**か**の設定を管理する次の方法。

* 設定をリセットする IIS マネージャーを使用して、 *web.config*ファイルについては、展開で、ファイルが上書きされます。
* 追加、 *web.config ファイル*アプリの設定でローカルにします。 詳細については、次を参照してください。、[開発側の構成](#development-side-configuration-with-a-local-webconfig-file)セクション。

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a>発行し、IIS サイトのフォルダーにプロジェクトを配置

Visual Studio または .NET Core CLI を使用して、発行し、目的のフォルダーに、アプリをデプロイします。

IIS でホストの詳細については、発行、および配置では、次のトピックを参照してください。

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

Windows 認証が動作していることを確認するアプリを起動します。

## <a name="enable-windows-authentication-with-httpsys"></a>HTTP.sys を使用して Windows 認証を有効にします。

使用することができますが、Kestrel が Windows 認証をサポートしていない[HTTP.sys](xref:fundamentals/servers/httpsys) Windows で自己ホスト型のシナリオをサポートします。 次の例は、Windows 認証を使用した HTTP.sys を使用するアプリの web ホストを構成します。

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> HTTP.sys では、Kerberos 認証プロトコルを使用したカーネル モード認証に処理が委任されます。 Kerberos および HTTP.sys ではユーザー モード認証がサポートされていません。 Active Directory から取得され、クライアントによって、ユーザーを認証するサーバーに転送される Kerberos トークン/チケットを暗号化解除するには、コンピューター アカウントを使用する必要があります。 アプリのユーザーではなく、ホストのサービス プリンシパル名 (SPN) を登録します。

> [!NOTE]
> HTTP.sys は、Nano Server バージョン 1709 以降でサポートされていません。 Nano Server の Windows 認証と HTTP.sys を使用する、 [(microsoft/windowsservercore) の Server Core コンテナー](https://hub.docker.com/r/microsoft/windowsservercore/)します。 Server Core の詳細については、[Windows Server の Server Core インストール オプションとは何ですか?](/windows-server/administration/server-core/what-is-server-core)を参照してください。

## <a name="work-with-windows-authentication"></a>Windows 認証を使用します。

匿名アクセスの構成の状態にする方法が決定します、`[Authorize]`と`[AllowAnonymous]`属性は、アプリで使用します。 次の 2 つのセクションでは、匿名アクセスの許可されていないと、許可されている構成の状態を処理する方法について説明します。

### <a name="disallow-anonymous-access"></a>匿名アクセスを禁止します。

Windows 認証が有効になっており、匿名アクセスが無効になっているときに、`[Authorize]`と`[AllowAnonymous]`属性は影響ありません。 IIS サイト (または HTTP.sys) を匿名アクセスを許可しないように構成する場合、要求がアプリに到達しません。 このため、`[AllowAnonymous]`属性には適用されません。

### <a name="allow-anonymous-access"></a>匿名アクセスを許可します。

Windows 認証と匿名アクセスの両方が有効になっているときに使用して、`[Authorize]`と`[AllowAnonymous]`属性。 `[Authorize]`属性では、本当に Windows 認証を必要と、アプリの情報を保護できます。 `[AllowAnonymous]`属性のオーバーライド`[Authorize]`属性の匿名アクセスを許可するアプリ内で使用します。 参照してください[単純な承認](xref:security/authorization/simple)属性の使用方法の詳細。

ASP.NET core 2.x、`[Authorize]`属性に追加の構成が必要です*Startup.cs*匿名要求を Windows 認証チャレンジを。 推奨される構成によって若干異なります、web サーバーが使用されています。

> [!NOTE]
> 既定では、ページにアクセスするための承認を持たないユーザーには、空の HTTP 403 応答が表示されます。 [StatusCodePages ミドルウェア](xref:fundamentals/error-handling#configure-status-code-pages)「アクセスが拒否されました」のより優れたエクスペリエンスをユーザーに提供するように構成できます。

#### <a name="iis"></a>IIS

IIS を使用する場合に、次を追加、`ConfigureServices`メソッド。

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

HTTP.sys を使用する場合に、次を追加、`ConfigureServices`メソッド。

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>偽装

ASP.NET Core では、権限借用を実装しません。 アプリは、アプリ プールまたはプロセス id を使用して、すべての要求をアプリの id で実行します。 明示的に、ユーザーに代わって操作を実行する必要がある場合を使用して、 [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*)で、[ターミナル インライン ミドルウェア](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)で`Startup.Configure`します。 このコンテキストで 1 つのアクションを実行し、コンテキストを閉じます。

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` 非同期操作をサポートしていないし、複雑なシナリオでは使用しないでください。 全体要求またはミドルウェアのチェーンをラッピングされていないサポートなど、お勧めします。

### <a name="claims-transformations"></a>要求の変換

IIS のインプロセス モードでホストする場合<xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*>されていないユーザーを初期化するために内部的に呼び出されます。 そのため、<xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>実装のすべての認証は既定でアクティブにした後、要求を変換するために使用します。 詳細については、インプロセスをホストする場合は、クレームの変換をアクティブにするためのコード例を参照してください。<xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>します。
