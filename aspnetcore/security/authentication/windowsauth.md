---
title: "ASP.NET Core で Windows 認証を構成します。"
author: ardalis
description: "この記事では、IIS Express、IIS、HTTP.sys および WebListener を使用して、ASP.NET Core で Windows 認証を構成する方法について説明します。"
keywords: "ASP.NET Core、Windows 認証、Authorize attribute、AllowAnonymous 属性"
ms.author: riande
manager: wpickett
ms.date: 10/24/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: 703f924d049a267cb8bee22e63e6669b13099c53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a>ASP.NET Core アプリケーションの Windows 認証を構成します。

によって[Steve Smith](https://ardalis.com)と[Scott Addie](https://twitter.com/Scott_Addie)

Windows 認証は、IIS でホストされている ASP.NET Core アプリケーションに対して設定できる[HTTP.sys](xref:fundamentals/servers/httpsys)、または[WebListener](xref:fundamentals/servers/weblistener)です。

## <a name="what-is-windows-authentication"></a>Windows 認証とは何ですか。

Windows 認証は、ASP.NET Core アプリケーションのユーザーを認証するオペレーティング システムに依存します。 ユーザーを識別する Active Directory ドメインの id またはその他の Windows アカウントを使用して、企業ネットワークで実行されると、サーバーは、Windows 認証を使用することができます。 Windows 認証は、同じ Windows ドメインにユーザー、クライアント アプリケーションおよび web サーバーが属している、イントラネット環境に最も適しています。

[Windows 認証と IIS のインストールの詳細について](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)です。

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>ASP.NET Core アプリケーションの Windows 認証を有効にします。

Visual Studio Web アプリケーション テンプレートは、Windows 認証をサポートするために構成されていることができます。

### <a name="use-the-windows-authentication-app-template"></a>Windows 認証アプリ テンプレートを使用します。

Visual Studio で。
1. 新しい ASP.NET Core Web アプリケーションを作成します。 
1. テンプレートの一覧から Web アプリケーションを選択します。
1. 選択、**認証の変更**ボタンをクリックして**Windows 認証**です。 

アプリを実行します。 上部にある ユーザー名が表示されるアプリの右。

![Windows 認証のブラウザーのスクリーン ショット](windowsauth/_static/browser-screenshot.png)

IIS Express を使用して開発作業では、テンプレートは、Windows 認証を使用するために必要なすべての構成を提供します。 次のセクションでは、Windows 認証用の ASP.NET Core アプリを手動で構成する方法を示します。

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Windows および匿名認証用 visual Studio の設定

Visual Studio プロジェクト**プロパティ**ページの**デバッグ** タブは、Windows 認証と匿名認証のチェック ボックスを表示します。

![Windows 認証のブラウザーのスクリーン ショット](windowsauth/_static/vs-auth-property-menu.png)

これら 2 つのプロパティを構成する代わりに、 *launchSettings.json*ファイル。

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>IIS での Windows 認証を有効にします。

IIS を使用して、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)ASP.NET Core アプリケーションをホストするには、(ANCM)。 ANCM フロー Windows 認証を IIS に既定でします。 Windows 認証の構成については、アプリケーション プロジェクトではなく、IIS 内で実行します。 次のセクションでは、IIS マネージャーを使用して、Windows 認証を使用する ASP.NET Core アプリケーションを構成する方法を示します。

### <a name="create-a-new-iis-site"></a>新しい IIS サイトを作成します。

名前とフォルダーを指定し、新しいアプリケーション プールを作成することを許可します。

### <a name="customize-authentication"></a>認証をカスタマイズします。

サイトの [認証] メニューを開きます。

![IIS 認証 メニュー](windowsauth/_static/iis-authentication-menu.png)

匿名認証を無効にして、Windows 認証を有効にします。

![IIS の認証設定](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>IIS サイトのフォルダーにプロジェクトを発行します。

Visual Studio または .NET Core CLI を使用して、インストール先フォルダーをアプリの発行します。

![Visual Studio 発行のダイアログ ボックス](windowsauth/_static/vs-publish-app.png)

詳細については[を IIS に発行](xref:publishing/iis)です。

Windows 認証が動作していることを確認するアプリを起動します。

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a>HTTP.sys や WebListener で Windows 認証を有効にします。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel が Windows 認証をサポートしていないは使用できます[HTTP.sys](xref:fundamentals/servers/httpsys) Windows では自己ホスト型のシナリオをサポートするためにします。 次の例は、Windows 認証での HTTP.sys を使用するアプリの web ホストを構成します。

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Kestrel が Windows 認証をサポートしていないは使用できます[WebListener](xref:fundamentals/servers/weblistener) Windows では自己ホスト型のシナリオをサポートするためにします。 次の例は、Windows 認証で WebListener を使用するアプリの web ホストを構成します。

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a>Windows 認証を使用します。

匿名アクセスの構成の状態を決定する方法、`[Authorize]`と`[AllowAnonymous]`属性は、アプリで使用します。 次の 2 つのセクションでは、匿名アクセスの許可されていないと許可されている構成の状態を処理する方法について説明します。

### <a name="disallow-anonymous-access"></a>匿名アクセスを許可しません。

Windows 認証が有効になっており、匿名アクセスが無効になっているときに、`[Authorize]`と`[AllowAnonymous]`属性が影響を与えるありません。 IIS サイト (または HTTP.sys または WebListener サーバー) への匿名アクセスを許可しないように構成する場合、要求がアプリに到達しません。 このため、`[AllowAnonymous]`属性は適用されません。

### <a name="allow-anonymous-access"></a>匿名アクセスを許可します。

Windows 認証と匿名アクセスの両方が有効になっているときに使用して、`[Authorize]`と`[AllowAnonymous]`属性。 `[Authorize]`属性では、Windows 認証を必要と本当にアプリの部分をセキュリティで保護することができます。 `[AllowAnonymous]`属性のオーバーライド`[Authorize]`属性の匿名アクセスを許可するアプリ内で使用します。 参照してください[単純な承認](xref:security/authorization/simple)属性の使用方法の詳細。

ASP.NET Core で 2.x、`[Authorize]`属性で追加の構成が必要です。 *Startup.cs* Windows 認証の匿名要求を身にします。 推奨される構成によって若干異なります、web サーバーが使用されています。

#### <a name="iis"></a>IIS

IIS を使用する場合に、次を追加、`ConfigureServices`メソッド。 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

HTTP.sys を使用する場合に、次の追加、`ConfigureServices`メソッド。

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>偽装

ASP.NET Core は、権限借用を実装していません。 アプリは、アプリケーション プールまたはプロセス id を使用して、すべての要求のアプリケーション id で実行されます。 ユーザーの代理としてアクションを明示的に実行する必要がある場合を使用して`WindowsIdentity.RunImpersonated`です。 このコンテキストで 1 つのアクションを実行し、コンテキストを閉じます。

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

なお`RunImpersonated`非同期操作をサポートしないし、複雑なシナリオで使用しないでください。 全体の要求またはミドルウェア チェーンをラッピングされていないはサポートされてなど、ことをお勧めします。

---
