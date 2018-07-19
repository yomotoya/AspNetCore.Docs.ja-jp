---
title: ASP.NET Core SignalR で認証と承認
author: tdykstra
description: ASP.NET Core SignalR での認証と承認を使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/authn-and-authz
ms.openlocfilehash: fceae37ce53a0d5a219e6dc466e9cc6df0277494
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123773"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="96fc9-103">ASP.NET Core SignalR で認証と承認</span><span class="sxs-lookup"><span data-stu-id="96fc9-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="96fc9-104">によって[Andrew Stanton-nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="96fc9-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="96fc9-105">[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="96fc9-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="96fc9-106">SignalR ハブに接続するユーザーを認証します。</span><span class="sxs-lookup"><span data-stu-id="96fc9-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="96fc9-107">SignalR で使用できる[ASP.NET Core 認証](xref:security/authentication/index)接続ごとにユーザーを関連付ける。</span><span class="sxs-lookup"><span data-stu-id="96fc9-107">SignalR can be used with [ASP.NET Core Authentication](xref:security/authentication/index) to associate a user with each connection.</span></span> <span data-ttu-id="96fc9-108">ハブの認証データをからアクセスできる、 [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="96fc9-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="96fc9-109">認証で許可されるユーザーに関連付けられているすべての接続でメソッドを呼び出すハブ (を参照してください[ユーザーと SignalR でグループ管理](xref:signalr/groups)詳細については)。</span><span class="sxs-lookup"><span data-stu-id="96fc9-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="96fc9-110">複数の接続は、1 人のユーザーを関連付けることができます。</span><span class="sxs-lookup"><span data-stu-id="96fc9-110">Multiple connections may be associated with a single user.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="96fc9-111">Cookie 認証</span><span class="sxs-lookup"><span data-stu-id="96fc9-111">Cookie authentication</span></span>

<span data-ttu-id="96fc9-112">ブラウザー ベースのアプリでは、cookie 認証は、SignalR 接続を自動的にフローする既存のユーザーの資格情報をできます。</span><span class="sxs-lookup"><span data-stu-id="96fc9-112">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="96fc9-113">クライアントのブラウザーを使用する場合、追加の構成は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="96fc9-113">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="96fc9-114">アプリに、ユーザーがログインに SignalR 接続は、この認証を自動的に継承します。</span><span class="sxs-lookup"><span data-stu-id="96fc9-114">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="96fc9-115">アプリはのみ、ブラウザー クライアントからユーザーを認証する必要がない限り、cookie 認証はお勧めしません。</span><span class="sxs-lookup"><span data-stu-id="96fc9-115">Cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="96fc9-116">使用する場合、 [.NET クライアント](xref:signalr/dotnet-client)、`Cookies`でプロパティを構成することができます、 `.WithUrl` cookie を提供するために呼び出し。</span><span class="sxs-lookup"><span data-stu-id="96fc9-116">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="96fc9-117">ただし、.NET クライアントからの cookie 認証を使用するには、クッキーの認証データを交換するための API を提供するアプリが必要です。</span><span class="sxs-lookup"><span data-stu-id="96fc9-117">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="96fc9-118">ベアラー トークン認証</span><span class="sxs-lookup"><span data-stu-id="96fc9-118">Bearer token authentication</span></span>

<span data-ttu-id="96fc9-119">ベアラー トークン認証は、クライアントのブラウザー以外のクライアントを使用する場合に推奨されるアプローチです。</span><span class="sxs-lookup"><span data-stu-id="96fc9-119">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span> <span data-ttu-id="96fc9-120">この方法では、クライアントは、サーバーを検証し、ユーザーを識別するために使用するアクセス トークンを提供します。</span><span class="sxs-lookup"><span data-stu-id="96fc9-120">In this approach, the client provides an access token that the server validates and uses to identify the user.</span></span> <span data-ttu-id="96fc9-121">ベアラー トークン認証の詳細については、このドキュメントの範囲を超えては。</span><span class="sxs-lookup"><span data-stu-id="96fc9-121">The details of bearer token authentication are beyond the scope of this document.</span></span> <span data-ttu-id="96fc9-122">使用してベアラー トークン認証を構成サーバーで、 [JWT ベアラー ミドルウェア](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)します。</span><span class="sxs-lookup"><span data-stu-id="96fc9-122">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="96fc9-123">JavaScript クライアントで、トークンを提供できますを使用して、 [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication)オプション。</span><span class="sxs-lookup"><span data-stu-id="96fc9-123">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="96fc9-124">.NET クライアントである類似した[AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication)トークンを構成するために使用できるプロパティ。</span><span class="sxs-lookup"><span data-stu-id="96fc9-124">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => _myAccessToken;
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="96fc9-125">指定したアクセス トークンの関数は、前に呼び出されます**すべて**SignalR による HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="96fc9-125">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="96fc9-126">(ための接続中に、有効期限が切れる可能性があります) は、接続を維持するために、トークンを更新する必要があるかどうか、この関数内から行うし、更新されたトークンが返されます。</span><span class="sxs-lookup"><span data-stu-id="96fc9-126">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="96fc9-127">標準の web Api では、ベアラー トークンは、HTTP ヘッダーで送信されます。</span><span class="sxs-lookup"><span data-stu-id="96fc9-127">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="96fc9-128">ただし、SignalR では、一部のトランスポートを使用する場合は、ブラウザーでこれらのヘッダーを設定することはありません。</span><span class="sxs-lookup"><span data-stu-id="96fc9-128">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="96fc9-129">WebSockets および Server-Sent イベントを使用する場合は、トークンが、クエリ文字列パラメーターとして送信されます。</span><span class="sxs-lookup"><span data-stu-id="96fc9-129">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="96fc9-130">サーバーでこれをサポートするためには、追加の構成が必要です。</span><span class="sxs-lookup"><span data-stu-id="96fc9-130">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="windows-authentication"></a><span data-ttu-id="96fc9-131">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="96fc9-131">Windows authentication</span></span>

<span data-ttu-id="96fc9-132">場合[Windows 認証](xref:security/authentication/windowsauth)が構成されているアプリに、SignalR は、その id を使用して、ハブをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="96fc9-132">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="96fc9-133">ただし、個々 のユーザーにメッセージを送信するには、カスタムのユーザーの ID プロバイダーを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="96fc9-133">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="96fc9-134">Windows 認証システムは、SignalR を使用してユーザー名を確認する"Name Identifier"要求を提供しないためにです。</span><span class="sxs-lookup"><span data-stu-id="96fc9-134">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="96fc9-135">実装する新しいクラスを追加`IUserIdProvider`し、識別子として使用するユーザーから、要求の 1 つを取得します。</span><span class="sxs-lookup"><span data-stu-id="96fc9-135">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="96fc9-136">たとえば、"Name"要求を使用する (これは、フォーム内の Windows ユーザー名`[Domain]\[Username]`)、次のクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="96fc9-136">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

```csharp
public class NameUserIdProvider : IUserIdProvider
{
    public string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Name)?.Value;
    }
}
```

<span data-ttu-id="96fc9-137">なく`ClaimTypes.Name`、任意の値を使用することができます、 `User` (など、Windows SID 識別子など)。</span><span class="sxs-lookup"><span data-stu-id="96fc9-137">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="96fc9-138">選択した値は、システム内のすべてのユーザーの間で一意でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="96fc9-138">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="96fc9-139">それ以外の場合、1 人のユーザー宛てのメッセージは、別のユーザーに陥る可能性があります。</span><span class="sxs-lookup"><span data-stu-id="96fc9-139">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="96fc9-140">このコンポーネントの登録、`Startup.ConfigureServices`メソッド**後**への呼び出し `.AddSignalR`</span><span class="sxs-lookup"><span data-stu-id="96fc9-140">Register this component in your `Startup.ConfigureServices` method **after** the call to `.AddSignalR`</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="96fc9-141">設定して、.NET クライアントで Windows 認証を有効にする必要があります、 [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="96fc9-141">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="96fc9-142">Windows 認証は、Microsoft Internet Explorer または Microsoft Edge を使用する場合にのみ、ブラウザー クライアントでサポートされます。</span><span class="sxs-lookup"><span data-stu-id="96fc9-142">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="96fc9-143">アクセスのハブおよびハブ メソッドのユーザーを認証します。</span><span class="sxs-lookup"><span data-stu-id="96fc9-143">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="96fc9-144">既定では、認証されていないユーザーによってハブのすべてのメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="96fc9-144">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="96fc9-145">認証を必要とするためには、適用、 [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute)ハブに属性します。</span><span class="sxs-lookup"><span data-stu-id="96fc9-145">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="96fc9-146">コンス トラクターの引数とのプロパティを使用することができます、`[Authorize]`特定に一致する唯一のユーザーへのアクセス制限を属性[承認ポリシー](xref:security/authorization/policies)します。</span><span class="sxs-lookup"><span data-stu-id="96fc9-146">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="96fc9-147">たとえばと呼ばれるカスタム承認ポリシーがある場合`MyAuthorizationPolicy`そのポリシーに一致するユーザーのみが、次のコードを使用してハブにアクセスできることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="96fc9-147">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

<span data-ttu-id="96fc9-148">個々 のハブ メソッドを持つことができます、`[Authorize]`属性も適用されます。</span><span class="sxs-lookup"><span data-stu-id="96fc9-148">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="96fc9-149">現在のユーザーが、メソッドに適用されるポリシーに一致しない場合は、呼び出し元にエラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="96fc9-149">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class ChatHub: Hub
{
    public async Task Send(string message)
    {
        // ... send a message to all users ...
    }

    [Authorize("Administrators")]
    public void BanUser(string userName)
    {
        // ... ban a user from the chat room (something only Administrators can do) ...
    }
}
```
