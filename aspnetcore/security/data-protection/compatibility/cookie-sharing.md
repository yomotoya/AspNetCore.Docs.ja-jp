---
title: "アプリケーション間で cookie を共有"
author: rick-anderson
description: "このドキュメントでは、データ保護スタックが ASP.NET との間の認証クッキーの共有をサポートする方法について説明します 4.x および ASP.NET Core アプリケーションです。"
keywords: "ASP.NET Core,ASP.NET,cookies,Interop,cookie 共有"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9a7aae45-8e21-4c54-950c-3c29df6c1082
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: e92cc81f9362787b7b4bfb44ba26b82182826a59
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="sharing-cookies-between-applications"></a>アプリケーション間で cookie を共有

Web サイト一般的で構成されている多数の個々 の web アプリケーション、すべての作業まとめて harmoniously です。 アプリケーション開発者は、良好なシングル サインオン エクスペリエンスを提供する場合、必要があります多くの場合、すべての別の web アプリケーション、サイト内で相互の認証チケットを共有します。

このシナリオをサポートするには、データ保護スタックは、共有 Katana cookie 認証および ASP.NET Core cookie 認証チケットを許可します。

## <a name="sharing-authentication-cookies-between-applications"></a>アプリケーション間での認証クッキーの共有

2 つの異なる ASP.NET Core アプリケーション間での認証クッキーを共有するには、アプリケーションごとに次のように cookie を共有する必要がありますを構成します。

メソッドを構成、ASP.NET と一致する cookie と、AuthenticationScheme データ保護サービスを設定する、CookieAuthenticationOptions を使用して 4.x です。

Id を使用する: 場合

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

Cookie を直接使用する: 場合

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
`DataProtectionProvider`が必要です、 `Microsoft.AspNetCore.DataProtection.Extensions` NuGet パッケージです。

この方法で使用する場合、DirectoryInfo は具体的には認証 cookie 用に確保キー記憶域の場所を指す必要があります。 Cookie 認証ミドルウェアがであると、DataProtectionProvider の明示的に指定された実装を使用して、アプリケーションの他の部分で使用するデータ保護システムから分離します。 アプリケーション名は無視されます (意図的には、ペイロードを共有する複数のアプリケーションを取得しようとしているため)。

>[!WARNING]
>キーは、残りの部分にあるように暗号化されるように、DataProtectionProvider を構成する必要があります、次の例です。
>
>
>  ```csharp
>  app.UseCookieAuthentication(new CookieAuthenticationOptions
>  {
>      DataProtectionProvider = DataProtectionProvider.Create(
>          new DirectoryInfo(@"c:\shared-auth-ticket-keys\"),
>          configure =>
>          {
>              configure.ProtectKeysWithCertificate("thumbprint");
>          })
>  });
>  ```

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a>ASP.NET との間の認証クッキーを共有 4.x および ASP.NET Core アプリケーション

Katana cookie 認証ミドルウェアを使用する ASP.NET 4.x アプリケーションは、ASP.NET Core cookie 認証ミドルウェアと互換性がある認証クッキーを生成するように構成できます。 これにより、サイト全体で滑らかなシングル サインオン エクスペリエンスを提供しながら段階的な部分、大規模なサイトの個々 のアプリケーションをアップグレードできます。

>[!TIP]
> 既存のアプリケーションが、プロジェクトの Startup.Auth.cs で UseCookieAuthentication への呼び出しの存在によって Katana cookie 認証ミドルウェアを使用するかどうかを判断できます。 ASP.NET 4.x web アプリケーション プロジェクトでは、Visual Studio 2013 で作成され、後で、既定では、Katana cookie 認証ミドルウェアを使用しています。

> [!NOTE]
> ASP.NET 4.x アプリケーションが .NET Framework 4.5.1 を対象する必要があります。 または以降では、それ以外の場合、必要な NuGet パッケージはインストールできません。

ASP.NET 4.x アプリケーションおよび ASP.NET Core アプリケーションの間の認証クッキーを共有するには、前述のように ASP.NET Core アプリケーションを構成し、以下の手順に従って、ASP.NET 4.x アプリケーションを構成すます。

1.  ASP.NET 4.x アプリケーションのそれぞれに Microsoft.Owin.Security.Interop パッケージをインストールします。

2.   Startup.Auth.cs では、一般に、次のようになります UseCookieAuthentication への呼び出しを見つけます。

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  UseCookieAuthentication への呼び出しを次のように変更をキー記憶域の場所に初期化されている DataProtectionProvider のインスタンスを作成して、ASP.NET Core cookie 認証ミドルウェアで使用される名前と一致する CookieName を変更し、チャンク形式は互換性があるために、相互運用機能 ChunkingCookieManager に CookieManager を設定します。

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
        CookieName = ".AspNetCore.Cookies",
        // CookieName = ".AspNetCore.ApplicationCookie", (if you're using identity)
        // CookiePath = "...", (if necessary)
        // ...
        TicketDataFormat = new AspNetTicketDataFormat(
            new DataProtectorShim(
                DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
                .CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", "v2"))),
        CookieManager = new ChunkingCookieManager()
    });
    ```
    ASP.NET Core アプリケーションを指しているし、同じ設定を使用して構成する必要がありますを同じ記憶域の場所を指す、DirectoryInfo があります。

ASP.NET 4.x および ASP.NET Core アプリケーションが認証 cookie を共有するように構成されてようになりました。

> [!NOTE]
> 各アプリケーションの id システムは、同じユーザー データベースを指すことを確認する必要があります。 それ以外の場合 id システムであっても、そのデータベース内の情報に対する認証クッキーの情報と一致するとき、実行時にエラーが生成されます。
