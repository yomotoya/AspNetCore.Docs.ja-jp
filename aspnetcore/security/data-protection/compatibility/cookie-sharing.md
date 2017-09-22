---
title: "アプリケーション間で cookie を共有"
author: rick-anderson
description: 
keywords: "ASP.NET Core,ASP.NET,cookies,Interop,cookie 共有"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9a7aae45-8e21-4c54-950c-3c29df6c1082
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: dbf52b0a990a3627b8eded22db033c45d51ba6ad
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="sharing-cookies-between-applications"></a><span data-ttu-id="fbea0-103">アプリケーション間で cookie を共有</span><span class="sxs-lookup"><span data-stu-id="fbea0-103">Sharing cookies between applications</span></span>

<span data-ttu-id="fbea0-104">Web サイト一般的で構成されている多数の個々 の web アプリケーション、すべての作業まとめて harmoniously です。</span><span class="sxs-lookup"><span data-stu-id="fbea0-104">Web sites commonly consist of many individual web applications, all working together harmoniously.</span></span> <span data-ttu-id="fbea0-105">アプリケーション開発者は、良好なシングル サインオン エクスペリエンスを提供する場合、必要があります多くの場合、すべての別の web アプリケーション、サイト内で相互の認証チケットを共有します。</span><span class="sxs-lookup"><span data-stu-id="fbea0-105">If an application developer wants to provide a good single-sign-on experience, they'll often need all of the different web applications within the site to share authentication tickets between each other.</span></span>

<span data-ttu-id="fbea0-106">このシナリオをサポートするには、データ保護スタックは、共有 Katana cookie 認証および ASP.NET Core cookie 認証チケットを許可します。</span><span class="sxs-lookup"><span data-stu-id="fbea0-106">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

## <a name="sharing-authentication-cookies-between-applications"></a><span data-ttu-id="fbea0-107">アプリケーション間での認証クッキーの共有</span><span class="sxs-lookup"><span data-stu-id="fbea0-107">Sharing authentication cookies between applications</span></span>

<span data-ttu-id="fbea0-108">2 つの異なる ASP.NET Core アプリケーション間での認証クッキーを共有するには、アプリケーションごとに次のように cookie を共有する必要がありますを構成します。</span><span class="sxs-lookup"><span data-stu-id="fbea0-108">To share authentication cookies between two different ASP.NET Core applications, configure each application that should share cookies as follows.</span></span>

<span data-ttu-id="fbea0-109">Cookie のデータ保護サービスをセットアップする CookieAuthenticationOptions と ASP.NET を一致するように AuthenticationScheme メソッドの使用を構成する 4.X です。</span><span class="sxs-lookup"><span data-stu-id="fbea0-109">In your configure method use the CookieAuthenticationOptions to set up the data protection service for cookies and the AuthenticationScheme to match ASP.NET 4.X.</span></span>

<span data-ttu-id="fbea0-110">Id を使用する: 場合</span><span class="sxs-lookup"><span data-stu-id="fbea0-110">If you're using identity:</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
   {
       options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
       var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
       options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
       options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
   });
   ```

<span data-ttu-id="fbea0-111">Cookie を直接使用する: 場合</span><span class="sxs-lookup"><span data-stu-id="fbea0-111">If you're using cookies directly:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
   {
       DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
   });
   ```
   
<span data-ttu-id="fbea0-112">`DataProtectionProvider`が必要です、 `Microsoft.AspNetCore.DataProtection.Extensions` NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="fbea0-112">The `DataProtectionProvider` requires the `Microsoft.AspNetCore.DataProtection.Extensions` NuGet package.</span></span>

<span data-ttu-id="fbea0-113">この方法で使用する場合、DirectoryInfo は具体的には認証 cookie 用に確保キー記憶域の場所を指す必要があります。</span><span class="sxs-lookup"><span data-stu-id="fbea0-113">When used in this manner, the DirectoryInfo should point to a key storage location specifically set aside for authentication cookies.</span></span> <span data-ttu-id="fbea0-114">Cookie 認証ミドルウェアがであると、DataProtectionProvider の明示的に指定された実装を使用して、アプリケーションの他の部分で使用するデータ保護システムから分離します。</span><span class="sxs-lookup"><span data-stu-id="fbea0-114">The cookie authentication middleware will use the explicitly provided implementation of the DataProtectionProvider, which is now isolated from the data protection system used by other parts of the application.</span></span> <span data-ttu-id="fbea0-115">アプリケーション名は無視されます (意図的には、ペイロードを共有する複数のアプリケーションを取得しようとしているため)。</span><span class="sxs-lookup"><span data-stu-id="fbea0-115">The application name is ignored (intentionally so, since you're trying to get multiple applications to share payloads).</span></span>

>[!WARNING]
><span data-ttu-id="fbea0-116">キーは、残りの部分にあるように暗号化されるように、DataProtectionProvider を構成する必要があります、次の例です。</span><span class="sxs-lookup"><span data-stu-id="fbea0-116">You should consider configuring the DataProtectionProvider such that keys are encrypted at rest, as in the below example.</span></span>
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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a><span data-ttu-id="fbea0-117">ASP.NET との間の認証クッキーを共有 4.x および ASP.NET Core アプリケーション</span><span class="sxs-lookup"><span data-stu-id="fbea0-117">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core applications</span></span>

<span data-ttu-id="fbea0-118">Katana cookie 認証ミドルウェアを使用する ASP.NET 4.x アプリケーションは、ASP.NET Core cookie 認証ミドルウェアと互換性がある認証クッキーを生成するように構成できます。</span><span class="sxs-lookup"><span data-stu-id="fbea0-118">ASP.NET 4.x applications which use Katana cookie authentication middleware can be configured to generate authentication cookies which are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="fbea0-119">これにより、サイト全体で滑らかなシングル サインオン エクスペリエンスを提供しながら段階的な部分、大規模なサイトの個々 のアプリケーションをアップグレードできます。</span><span class="sxs-lookup"><span data-stu-id="fbea0-119">This allows upgrading a large site's individual applications piecemeal while still providing a smooth single sign on experience across the site.</span></span>

>[!TIP]
> <span data-ttu-id="fbea0-120">既存のアプリケーションが、プロジェクトの Startup.Auth.cs で UseCookieAuthentication への呼び出しの存在によって Katana cookie 認証ミドルウェアを使用するかどうかを判断できます。</span><span class="sxs-lookup"><span data-stu-id="fbea0-120">You can tell if your existing application uses Katana cookie authentication middleware by the existence of a call to UseCookieAuthentication in your project's Startup.Auth.cs.</span></span> <span data-ttu-id="fbea0-121">ASP.NET 4.x web アプリケーション プロジェクトでは、Visual Studio 2013 で作成され、後で、既定では、Katana cookie 認証ミドルウェアを使用しています。</span><span class="sxs-lookup"><span data-stu-id="fbea0-121">ASP.NET 4.x web application projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="fbea0-122">ASP.NET 4.x アプリケーションが .NET Framework 4.5.1 を対象する必要があります。 または以降では、それ以外の場合、必要な NuGet パッケージはインストールできません。</span><span class="sxs-lookup"><span data-stu-id="fbea0-122">Your ASP.NET 4.x application must target .NET Framework 4.5.1 or higher, otherwise the necessary NuGet packages will fail to install.</span></span>

<span data-ttu-id="fbea0-123">ASP.NET 4.x アプリケーションおよび ASP.NET Core アプリケーションの間の認証クッキーを共有するには、前述のように ASP.NET Core アプリケーションを構成し、以下の手順に従って、ASP.NET 4.x アプリケーションを構成すます。</span><span class="sxs-lookup"><span data-stu-id="fbea0-123">To share authentication cookies between your ASP.NET 4.x applications and your ASP.NET Core applications, configure the ASP.NET Core application as stated above, then configure your ASP.NET 4.x applications by following the steps below.</span></span>

1.  <span data-ttu-id="fbea0-124">ASP.NET 4.x アプリケーションのそれぞれに Microsoft.Owin.Security.Interop パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="fbea0-124">Install the package Microsoft.Owin.Security.Interop into each of your ASP.NET 4.x applications.</span></span>

2.   <span data-ttu-id="fbea0-125">Startup.Auth.cs では、一般に、次のようになります UseCookieAuthentication への呼び出しを見つけます。</span><span class="sxs-lookup"><span data-stu-id="fbea0-125">In Startup.Auth.cs, locate the call to UseCookieAuthentication, which will generally look like the following.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  <span data-ttu-id="fbea0-126">UseCookieAuthentication への呼び出しを次のように変更をキー記憶域の場所に初期化されている DataProtectionProvider のインスタンスを作成して、ASP.NET Core cookie 認証ミドルウェアで使用される名前と一致する CookieName を変更し、チャンク形式は互換性があるために、相互運用機能 ChunkingCookieManager に CookieManager を設定します。</span><span class="sxs-lookup"><span data-stu-id="fbea0-126">Modify the call to UseCookieAuthentication as follows, changing the CookieName to match the name used by the ASP.NET Core cookie authentication middleware, providing an instance of a DataProtectionProvider that has been initialized to a key storage location, and set CookieManager to interop ChunkingCookieManager so the chunking format is compatible.</span></span>

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
    <span data-ttu-id="fbea0-127">ASP.NET Core アプリケーションを指しているし、同じ設定を使用して構成する必要がありますを同じ記憶域の場所を指す、DirectoryInfo があります。</span><span class="sxs-lookup"><span data-stu-id="fbea0-127">The DirectoryInfo has to point to the same storage location that you pointed your ASP.NET Core application to and should be configured using the same settings.</span></span>

<span data-ttu-id="fbea0-128">ASP.NET 4.x および ASP.NET Core アプリケーションが認証 cookie を共有するように構成されてようになりました。</span><span class="sxs-lookup"><span data-stu-id="fbea0-128">The ASP.NET 4.x and ASP.NET Core applications are now configured to share authentication cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="fbea0-129">各アプリケーションの id システムは、同じユーザー データベースを指すことを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fbea0-129">You'll need to make sure that the identity system for each application is pointed at the same user database.</span></span> <span data-ttu-id="fbea0-130">それ以外の場合 id システムであっても、そのデータベース内の情報に対する認証クッキーの情報と一致するとき、実行時にエラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="fbea0-130">Otherwise the identity system will produce failures at runtime when it tries to match the information in the authentication cookie against the information in its database.</span></span>
