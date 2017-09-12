---
title: "ASP.NET Core で Windows 認証を構成します。"
author: ardalis
description: "ASP.NET Core で Windows 認証を構成する方法"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: aa401f956d74680efd3964203af3e8866b129887
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>ASP.NET Core で Windows 認証を構成します。

によって[Steve Smith](https://ardalis.com)

IIS または WebListener でホストされている ASP.NET Core アプリケーションの Windows 認証を構成することができます。

## <a name="what-is-windows-authentication"></a>Windows 認証とは

Windows 認証は、ASP.NET Core アプリケーションのユーザーを認証するオペレーティング システムに依存します。 ユーザーを識別する Active Directory ドメインの id またはその他の Windows アカウントを使用して、企業ネットワークで実行されると、サーバーは、Windows 認証を使用することができます。 Windows 認証は、ユーザー、クライアント アプリケーションおよび web サーバーを同じ Windows ドメインに属している、イントラネット環境に適した認証最善のセキュリティで保護された形式です。

[詳細については、Windows 認証と IIS のインストール](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)です。

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a>ASP.NET Core アプリケーションで Windows 認証を有効にします。

Visual Studio Web アプリケーション テンプレートは、Windows 認証をサポートするために構成されていることができます。

### <a name="using-the-windows-authentication-app-template"></a>Windows 認証アプリ テンプレートを使用します。

Visual Studio で。
* 新しい ASP.NET Core Web アプリケーションを作成します。 
* テンプレートの一覧から Web アプリケーションを選択します。
* 認証の変更 ボタンを選択し、選択**Windows 認証**です。 

アプリを実行します。 上部にある ユーザー名が表示されるアプリの右。

![Windows 認証のブラウザーのスクリーン ショット](windowsauth/_static/browser-screenshot.png)

IIS Express を使用して開発作業では、テンプレートは、Windows 認証を使用するために必要なすべての構成を提供します。 次のセクションでは、Windows 認証を手動で ASP.NET Core アプリケーションを構成する方法を示します。

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Windows および匿名認証用 visual Studio の設定

Visual Studio のプロパティ ページで、デバッグ タブは、Windows 認証と匿名認証のチェック ボックスを提供します。

![Windows 認証のブラウザーのスクリーン ショット](windowsauth/_static/vs-auth-property-menu.png)

これらのプロパティを構成することも、`launchSettings.json`ファイル。

```json
{
  "iisSettings": {
    "windowsAuthentication": true,
    "anonymousAuthentication": false,
    "iisExpress": {
      "applicationUrl": "http://localhost:52171/",
      "sslPort": 0
    }
  } // additional options trimmed
}
```

## <a name="enabling-windows-authentication-with-iis"></a>IIS での Windows 認証を有効にします。

IIS を使用して、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)ASP.NET Core アプリケーションをホストするには、(ANCM)。 ANCM フロー Windows 認証を IIS に既定でします。 Windows 認証の構成については、アプリケーション プロジェクトではなく、IIS 内で実行します。 次のセクションでは、IIS マネージャーを使用して、Windows 認証を使用する ASP.NET Core アプリケーションを構成する方法を示します。

### <a name="create-a-new-iis-site"></a>新しい IIS サイトを作成します。

名前とフォルダーを指定し、新しいアプリケーション プールを作成することを許可します。

### <a name="customize-authentication"></a>認証をカスタマイズします。

サイトの [認証] メニューを開きます。

![IIS 認証 メニュー](windowsauth/_static/iis-authentication-menu.png)

匿名認証を無効にして、Windows 認証を有効にします。

![IIS の認証設定](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>IIS サイトのフォルダーにプロジェクトを発行します。

Visual Studio または .NET Core CLI を使用して*発行*アプリ インストール先フォルダーをします。

![Visual Studio 発行のダイアログ ボックス](windowsauth/_static/vs-publish-app.png)

詳細については[を IIS に発行](xref:publishing/iis)です。

Windows 認証が動作していることを確認するアプリを起動します。

## <a name="enabling-windows-authentication-with-weblistener"></a>WebListener で Windows 認証を有効にします。

Kestrel が Windows 認証をサポートしていないは使用できます[WebListener](xref:fundamentals/servers/weblistener) Windows では自己ホスト型のシナリオをサポートするためにします。 次の例は、Windows 認証で WebListener を使用するアプリの web ホストを構成します。

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseWebListener(options =>
            {
                options.ListenerSettings.Authentication.Schemes = 
                    AuthenticationSchemes.Negotiate | AuthenticationSchemes.NTLM;
                options.ListenerSettings.Authentication.AllowAnonymous = false;
            })
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

## <a name="working-with-windows-authentication"></a>Windows 認証の使用

使用することができます、アプリが Windows 認証と匿名アクセスを使用する場合、``[Authorize]``と``[AllowAnonymous]``属性。 アプリを必要としない匿名の有効な操作を持たない``[Authorize]``以外の場合は、アプリは認証を要求として扱われます、匿名の要求は拒否されます。 IIS サイトが構成されている場合に、注意してください**いない**匿名アクセスを許可する、``[AllowAnonymous]``属性は**いない**匿名の要求を許可します。 ``[AllowAnonymous]``属性のオーバーライド``[Authorize]``属性の匿名アクセスを許可するアプリ内で使用します。

### <a name="impersonation"></a>偽装

ASP.NET Core は、権限借用を実装していません。 アプリは、アプリケーション プールまたはプロセス id を使用して、すべての要求のアプリケーション id で実行されます。 ユーザーの代理としてアクションを明示的に実行する必要がある場合を使用して``WindowsIdentity.RunImpersonated``です。 このコンテキストで 1 つのアクションを実行し、コンテキストを閉じます。 なお``RunImpersonated``非同期をサポートしておらず、複雑なシナリオでは使用できません。 たとえば、全体の要求またはミドルウェア チェーンをラッピングは not サポートまたはことをお勧めします。
