---
title: ASP.NET Core での Windows 認証を構成します。
author: scottaddie
description: ASP.NET Core での IIS と HTTP.sys は Windows 認証を構成する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 05/29/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 9dfff5dcba409ddca7e05c771b864ab121e0ea85
ms.sourcegitcommit: 06c4f2910dd54ded25e1b8750e09c66578748bc9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/30/2019
ms.locfileid: "66395922"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>ASP.NET Core での Windows 認証を構成します。

によって[Scott Addie](https://twitter.com/Scott_Addie)と[Luke Latham](https://github.com/guardrex)

[Windows 認証](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)でホストされている ASP.NET Core アプリ用に構成できます[IIS](xref:host-and-deploy/iis/index)または[HTTP.sys](xref:fundamentals/servers/httpsys)します。

Windows 認証は、ASP.NET Core アプリのユーザーを認証するオペレーティング システムに依存します。 ユーザーを識別するために Active Directory ドメインの id または Windows アカウントを使用して、企業ネットワークで、サーバーの実行時に、Windows 認証を使用できます。 Windows 認証は、同じ Windows ドメインに属しているユーザー、クライアント アプリ、および web サーバーのイントラネット環境に最適です。

## <a name="launch-settings-debugger"></a>起動設定 (デバッガー)

起動設定の構成にのみ影響、 *Properties/launchSettings.json*ファイルし、Windows 認証のため、IIS または HTTP.sys サーバーを構成しません。 サーバーの構成については、 [IIS または HTTP.sys の認証サービスを有効にする](#authentication-services-for-iis-or-httpsys)セクション。

**Web アプリケーション**を更新する Windows 認証をサポートする Visual Studio または .NET Core CLI を使用して利用可能なテンプレートを構成することができます、 *Properties/launchSettings.json*ファイル自動的に。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

**新しいプロジェクト**

1. 新しいプロジェクトを作成します。
1. **[ASP.NET Core Web アプリケーション]** を選択します。 **[次へ]** を選択します。
1. 名前を入力、**プロジェクト名**フィールド。 確認、**場所**エントリが正しいか、プロジェクトの場所を指定します。 **[作成]** を選択します。
1. 選択**変更** **認証**します。
1. **認証の変更**ウィンドウで、 **Windows 認証**します。 **[OK]** を選択します。
1. **[Web アプリケーション]** を選択します。
1. **[作成]** を選択します。

アプリを実行します。 ユーザー名は、レンダリングされたアプリのユーザー インターフェイスに表示されます。

**既存のプロジェクト**

プロジェクトのプロパティは、Windows 認証を有効にして、匿名認証を無効にします。

1. **ソリューション エクスプローラー**でプロジェクトを右クリックして、 **[プロパティ]** を選択します。
1. **[デバッグ]** タブを選択します。
1. チェック ボックスをオフ**匿名認証を有効にする**します。
1. チェック ボックスをオン**Windows 認証を有効にする**します。
1. 保存して、プロパティ ページを閉じます。

プロパティを構成する代わりに、`iisSettings`のノード、 *launchSettings.json*ファイル。

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code / .NET Core CLI](#tab/visual-studio-code+netcore-cli)

**新しいプロジェクト**

実行、[新しい dotnet](/dotnet/core/tools/dotnet-new)コマンドと、`webapp`引数 (ASP.NET Core Web アプリ) と`--auth Windows`スイッチします。

```console
dotnet new webapp --auth Windows
```

**既存のプロジェクト**

更新プログラム、`iisSettings`のノード、 *launchSettings.json*ファイル。

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

既存のプロジェクトを変更する場合は、プロジェクト ファイルにはへのパッケージ参照が含まれていることを確認、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)**または**、 [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet パッケージ。

## <a name="authentication-services-for-iis-or-httpsys"></a>IIS または HTTP.sys の認証サービス

ホスティングのシナリオに応じてのガイダンスに従って**か**、 [IIS](#iis)セクション**または** [HTTP.sys](#httpsys)セクション。

### <a name="iis"></a>IIS

認証サービスを呼び出すことによって追加<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>(<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName>名前空間) で`Startup.ConfigureServices`:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

IIS を使用して、 [ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)ASP.NET Core アプリをホストします。 使用した IIS の Windows 認証が構成されている、 *web.config*ファイル。 以下のセクションで表示する方法。

* ローカルの提供*web.config*ファイルをアプリが展開されると、サーバーで Windows 認証をアクティブにします。
* IIS マネージャーを使用して、構成、 *web.config*サーバーに既に展開されている ASP.NET Core アプリのファイル。

これをいない場合は、ASP.NET Core アプリをホストする IIS を有効にします。 詳細については、「 <xref:host-and-deploy/iis/index> 」を参照してください。

Windows 認証の IIS の役割サービスを有効にします。 詳細については、[(手順 2 参照)、IIS の役割サービスで Windows 認証を有効にする](xref:host-and-deploy/iis/index#iis-configuration)を参照してください。

[IIS 統合ミドルウェア](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)既定で自動的に要求の認証に構成されます。 詳細については、次を参照してください。 [ASP.NET Core の IIS と Windows ホスト。IIS のオプション (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options)します。

ASP.NET Core モジュールは、既定では、アプリに Windows 認証トークンを転送するように構成されます。 詳細については、次を参照してください。 [ASP.NET Core モジュール構成リファレンス。AspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)します。

使用**か**の次の方法。

* **発行して、プロジェクトを配置する前に**次の追加*web.config*ファイルをプロジェクトのルート。

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  プロジェクトが .NET Core SDK によって公開されると (なし、`<IsTransformWebConfigDisabled>`プロパティに設定`true`プロジェクト ファイル内)、公開された*web.config*ファイルが含まれています、`<location><system.webServer><security><authentication>`セクション。 詳細については、`<IsTransformWebConfigDisabled>`プロパティを参照してください<xref:host-and-deploy/iis/index#webconfig-file>します。

* **発行し、プロジェクトを配置した後**サーバー側の構成、IIS マネージャーでを実行します。

  1. IIS マネージャーで、下にある IIS サイトを選択して、**サイト**のノード、**接続**サイドバーです。
  1. ダブルクリック**認証**で、 **IIS**領域。
  1. 選択**匿名認証**します。 選択**を無効にする**で、**アクション**サイドバーです。
  1. 選択**Windows 認証**します。 選択**を有効にする**で、**アクション**サイドバーです。

  これらのアクションが実行したときに、IIS マネージャーは、アプリを変更します*web.config*ファイル。 A`<system.webServer><security><authentication>`ノードが更新された設定を使用した追加`anonymousAuthentication`と`windowsAuthentication`:

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  `<system.webServer>`セクションに追加、 *web.config*ファイルを IIS マネージャーでは、アプリの外部で`<location>`は .NET Core SDK により、アプリが公開されるときに追加されたセクションです。 外部のセクションが追加されるため、`<location>`ノード、いずれかで、設定が継承されます[サブ アプリ](xref:host-and-deploy/iis/index#sub-applications)現在のアプリにします。 継承を防ぐためには、移動、追加した`<security>`内のセクション、 `<location><system.webServer>` .NET Core SDK が提供されているセクション。

  IIS マネージャーを使用するには、IIS の構成を追加する、影響を受けるのみアプリの*web.config*サーバー上のファイル。 場合、アプリの後続の配置は、サーバーの設定を上書き可能性があります、サーバーのコピーの*web.config*はプロジェクトの置き換え*web.config*ファイル。 使用**か**の設定を管理する次の方法。

  * 設定をリセットする IIS マネージャーを使用して、 *web.config*ファイルについては、展開で、ファイルが上書きされます。
  * 追加、 *web.config ファイル*アプリの設定でローカルにします。

### <a name="httpsys"></a>HTTP.sys

[Kestrel](xref:fundamentals/servers/kestrel) 、Windows 認証をサポートしていないを使用することができます[HTTP.sys](xref:fundamentals/servers/httpsys) Windows で自己ホスト型のシナリオをサポートします。

認証サービスを呼び出すことによって追加<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>(<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName>名前空間) で`Startup.ConfigureServices`:

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

Windows 認証を使用した HTTP.sys を使用するアプリの web ホストを構成する (*Program.cs*)。 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName>名前空間。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> HTTP.sys では、Kerberos 認証プロトコルを使用したカーネル モード認証に処理が委任されます。 Kerberos および HTTP.sys ではユーザー モード認証がサポートされていません。 Active Directory から取得され、クライアントによって、ユーザーを認証するサーバーに転送される Kerberos トークン/チケットを暗号化解除するには、コンピューター アカウントを使用する必要があります。 アプリのユーザーではなく、ホストのサービス プリンシパル名 (SPN) を登録します。

> [!NOTE]
> HTTP.sys は、Nano Server バージョン 1709 以降でサポートされていません。 Nano Server の Windows 認証と HTTP.sys を使用する、 [(microsoft/windowsservercore) の Server Core コンテナー](https://hub.docker.com/r/microsoft/windowsservercore/)します。 Server Core の詳細については、[Windows Server の Server Core インストール オプションとは何ですか?](/windows-server/administration/server-core/what-is-server-core)を参照してください。

## <a name="authorize-users"></a>ユーザーを認証します。

匿名アクセスの構成の状態にする方法が決定します、`[Authorize]`と`[AllowAnonymous]`属性は、アプリで使用します。 次の 2 つのセクションでは、匿名アクセスの許可されていないと、許可されている構成の状態を処理する方法について説明します。

### <a name="disallow-anonymous-access"></a>匿名アクセスを禁止します。

Windows 認証が有効になっており、匿名アクセスが無効になっているときに、`[Authorize]`と`[AllowAnonymous]`属性は影響ありません。 匿名アクセスを禁止する IIS サイトを構成する場合、要求がアプリに到達しません。 このため、`[AllowAnonymous]`属性には適用されません。

### <a name="allow-anonymous-access"></a>匿名アクセスを許可します。

Windows 認証と匿名アクセスの両方が有効になっているときに使用して、`[Authorize]`と`[AllowAnonymous]`属性。 `[Authorize]`属性では、認証を必要とするアプリのエンドポイントをセキュリティで保護できます。 `[AllowAnonymous]`属性のオーバーライド、`[Authorize]`への匿名アクセスを許可するアプリ内の属性。 属性の使用方法の詳細を参照してください。<xref:security/authorization/simple>します。

> [!NOTE]
> 既定では、ページにアクセスするための承認を持たないユーザーには、空の HTTP 403 応答が表示されます。 [StatusCodePages ミドルウェア](xref:fundamentals/error-handling#usestatuscodepages)「アクセスが拒否されました」のより優れたエクスペリエンスをユーザーに提供するように構成できます。

## <a name="impersonation"></a>偽装

ASP.NET Core では、権限借用を実装しません。 アプリは、アプリ プールまたはプロセス id を使用して、すべての要求をアプリの id で実行します。 使用の場合は、アプリは、ユーザーの代理としてアクションを実行する必要があります、 [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*)で、[ターミナル インライン ミドルウェア](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)で`Startup.Configure`します。 このコンテキストで 1 つのアクションを実行し、コンテキストを閉じます。

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` 非同期操作をサポートしていないし、複雑なシナリオでは使用しないでください。 全体要求またはミドルウェアのチェーンをラッピングされていないサポートなど、お勧めします。

## <a name="claims-transformations"></a>要求の変換

IIS のインプロセス モードでホストする場合<xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*>されていないユーザーを初期化するために内部的に呼び出されます。 そのため、認証のたびに要求を変換するための <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 実装は既定で有効になっていません。 詳細については、インプロセスをホストする場合は、クレームの変換をアクティブにするためのコード例を参照してください。<xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>します。

## <a name="additional-resources"></a>その他の技術情報

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
