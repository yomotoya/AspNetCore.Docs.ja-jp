---
title: ASP.NET Core での Windows 認証を構成します。
author: scottaddie
description: ASP.NET core で IIS Express、IIS、HTTP.sys は、および WebListener を使用して Windows 認証を構成する方法について説明します。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/01/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 342759a6ff4b5679e0d54c979188ae66d339562d
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121298"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>ASP.NET Core での Windows 認証を構成します。

作成者: [Steve Smith](https://ardalis.com)、[Scott Addie](https://twitter.com/Scott_Addie)

Windows 認証は、IIS でホストされている ASP.NET Core アプリ用に構成できます[HTTP.sys](xref:fundamentals/servers/httpsys)、または[WebListener](xref:fundamentals/servers/weblistener)します。

## <a name="windows-authentication"></a>Windows 認証

Windows 認証は、ASP.NET Core アプリのユーザーを認証するオペレーティング システムに依存します。 ユーザーを識別するために Active Directory ドメインの id またはその他の Windows アカウントを使用して、企業ネットワークで、サーバーの実行時に、Windows 認証を使用できます。 Windows 認証は、同じ Windows ドメインにユーザー、クライアント アプリケーション、および web サーバーが属している、イントラネット環境に最適です。

[Windows 認証の詳細について説明し、IIS のインストール、](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)します。

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>ASP.NET Core アプリで Windows 認証を有効にします。

Visual Studio Web アプリケーション テンプレートは、Windows 認証をサポートするために構成できます。

### <a name="use-the-windows-authentication-app-template"></a>Windows 認証アプリ テンプレートを使用します。

Visual Studio:

1. 新しい ASP.NET Core Web アプリケーションを作成します。
1. テンプレートの一覧から Web アプリケーションを選択します。
1. 選択、**認証の変更**ボタンをクリックし、選択**Windows 認証**します。

アプリを実行します。 上部にある ユーザー名が表示されます、アプリの右。

![Windows 認証のブラウザーのスクリーン ショット](windowsauth/_static/browser-screenshot.png)

IIS Express を使用して開発作業では、テンプレートは、Windows 認証を使用するために必要なすべての構成を提供します。 次のセクションでは、Windows 認証用の ASP.NET Core アプリを手動で構成する方法を示します。

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Windows と匿名認証用 visual Studio の設定

Visual Studio プロジェクト**プロパティ**ページの**デバッグ** タブでは、Windows 認証と匿名認証 チェック ボックスが用意されています。

![強調表示されている認証オプションを使用して、Windows 認証ブラウザーのスクリーン ショット](windowsauth/_static/vs-auth-property-menu.png)

これら 2 つのプロパティを構成する代わりに、 *launchSettings.json*ファイル。

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>IIS での Windows 認証を有効にします。

IIS を使用して、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)ASP.NET Core アプリをホストします。 Windows 認証は、アプリケーションではなく、IIS で構成されます。 次のセクションでは、IIS マネージャーを使用して、Windows 認証を使用する ASP.NET Core アプリを構成する方法を示します。

### <a name="iis-configuration"></a>IIS 構成

Windows 認証の IIS の役割サービスを有効にします。 詳細については、次を参照してください。 [(手順 2 参照)、IIS の役割サービスで Windows 認証を有効にする](xref:host-and-deploy/iis/index#iis-configuration)します。

IIS 統合ミドルウェアは、既定で自動的に要求の認証に構成されます。 詳細については、次を参照してください。 [ASP.NET Core の IIS と Windows ホスト: IIS のオプション (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options)します。

ASP.NET Core モジュールは、既定では、アプリに Windows 認証トークンを転送するように構成されます。 詳細については、次を参照してください。 [ASP.NET Core モジュール構成リファレンス: aspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)します。

### <a name="create-a-new-iis-site"></a>新しい IIS サイトを作成します。

名前とフォルダーを指定し、新しいアプリケーション プールを作成することを許可します。

### <a name="customize-authentication"></a>認証をカスタマイズします。

サイトの認証機能を開きます。

![IIS 認証 メニュー](windowsauth/_static/iis-authentication-menu.png)

匿名認証を無効にして、Windows 認証を有効にします。

![IIS 認証の設定](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>IIS サイトのフォルダーにプロジェクトを発行します。

目的のフォルダーにアプリを発行する Visual Studio または .NET Core CLI を使用して。

![Visual Studio の発行 ダイアログ](windowsauth/_static/vs-publish-app.png)

詳細については[IIS への発行](xref:host-and-deploy/iis/index)します。

Windows 認証が動作していることを確認するアプリを起動します。

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a>HTTP.sys を使用して Windows 認証を有効にします。

使用することができますが、Kestrel が Windows 認証をサポートしていない[HTTP.sys](xref:fundamentals/servers/httpsys) Windows で自己ホスト型のシナリオをサポートします。 次の例は、Windows 認証を使用した HTTP.sys を使用するアプリの web ホストを構成します。

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> HTTP.sys では、Kerberos 認証プロトコルを使用したカーネル モード認証に処理が委任されます。 Kerberos および HTTP.sys ではユーザー モード認証がサポートされていません。 Active Directory から取得され、クライアントによって、ユーザーを認証するサーバーに転送される Kerberos トークン/チケットを暗号化解除するには、コンピューター アカウントを使用する必要があります。 アプリのユーザーではなく、ホストのサービス プリンシパル名 (SPN) を登録します。

> [!NOTE]
> HTTP.sys は、Nano Server バージョン 1709 以降でサポートされていません。 Nano Server の Windows 認証と HTTP.sys を使用する、 [(microsoft/windowsservercore) の Server Core コンテナー](https://hub.docker.com/r/microsoft/windowsservercore/)します。 Server Core の詳細については、次を参照してください。 [Windows Server の Server Core インストール オプションとは何ですか?](/windows-server/administration/server-core/what-is-server-core)します。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a>WebListener での Windows 認証を有効にします。

使用することができますが、Kestrel が Windows 認証をサポートしていない[WebListener](xref:fundamentals/servers/weblistener) Windows で自己ホスト型のシナリオをサポートします。 次の例は、Windows 認証で WebListener を使用するアプリの web ホストを構成します。

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> WebListener では、Kerberos 認証プロトコルを使用したカーネル モード認証に処理が委任されます。 Kerberos および WebListener ではユーザー モード認証がサポートされていません。 Active Directory から取得され、クライアントによって、ユーザーを認証するサーバーに転送される Kerberos トークン/チケットを暗号化解除するには、コンピューター アカウントを使用する必要があります。 アプリのユーザーではなく、ホストのサービス プリンシパル名 (SPN) を登録します。

::: moniker-end

## <a name="work-with-windows-authentication"></a>Windows 認証を使用します。

匿名アクセスの構成の状態にする方法が決定します、`[Authorize]`と`[AllowAnonymous]`属性は、アプリで使用します。 次の 2 つのセクションでは、匿名アクセスの許可されていないと、許可されている構成の状態を処理する方法について説明します。

### <a name="disallow-anonymous-access"></a>匿名アクセスを禁止します。

Windows 認証が有効になっており、匿名アクセスが無効になっているときに、`[Authorize]`と`[AllowAnonymous]`属性は影響ありません。 IIS サイト (または HTTP.sys または WebListener サーバー) を匿名アクセスを許可しないように構成する場合、要求がアプリに到達しません。 このため、`[AllowAnonymous]`属性には適用されません。

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

ASP.NET Core では、権限借用を実装しません。 アプリのアプリ プールまたはプロセス id を使用して、すべての要求 id をアプリケーションで実行されます。 明示的に、ユーザーに代わって操作を実行する必要がある場合を使用して、`WindowsIdentity.RunImpersonated`します。 このコンテキストで 1 つのアクションを実行し、コンテキストを閉じます。

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

なお`RunImpersonated`非同期操作をサポートしていないし、複雑なシナリオでは使用しないでください。 全体要求またはミドルウェアのチェーンをラッピングされていないサポートなど、お勧めします。
