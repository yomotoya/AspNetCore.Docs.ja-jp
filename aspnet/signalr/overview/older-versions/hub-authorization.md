---
uid: signalr/overview/older-versions/hub-authorization
title: "SignalR ハブの認証と承認 (SignalR 1.x) |Microsoft ドキュメント"
author: pfletcher
description: "このトピックでは、どのユーザーまたはロールがハブ メソッドにアクセスできる制限する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: d73ab6c9091556a62e5d9475baf67a18e305585f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="b2b58-103">SignalR ハブの認証と承認 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="b2b58-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>
====================
<span data-ttu-id="b2b58-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b2b58-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b2b58-105">このトピックでは、どのユーザーまたはロールがハブ メソッドにアクセスできる制限する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>


## <a name="overview"></a><span data-ttu-id="b2b58-106">概要</span><span class="sxs-lookup"><span data-stu-id="b2b58-106">Overview</span></span>

<span data-ttu-id="b2b58-107">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="b2b58-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="b2b58-108">属性を承認します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="b2b58-109">すべてのハブに対して認証を要求します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="b2b58-110">カスタマイズした承認</span><span class="sxs-lookup"><span data-stu-id="b2b58-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="b2b58-111">クライアントに認証情報を渡す</span><span class="sxs-lookup"><span data-stu-id="b2b58-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="b2b58-112">.NET クライアントの認証オプション</span><span class="sxs-lookup"><span data-stu-id="b2b58-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="b2b58-113">フォーム認証 cookie</span><span class="sxs-lookup"><span data-stu-id="b2b58-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="b2b58-114">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="b2b58-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="b2b58-115">接続ヘッダー</span><span class="sxs-lookup"><span data-stu-id="b2b58-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="b2b58-116">証明書</span><span class="sxs-lookup"><span data-stu-id="b2b58-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="b2b58-117">属性を承認します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-117">Authorize attribute</span></span>

<span data-ttu-id="b2b58-118">SignalR の提供、 [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)ハブまたはメソッドへのアクセスを持つユーザーまたはロールを指定する属性。</span><span class="sxs-lookup"><span data-stu-id="b2b58-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="b2b58-119">この属性にある、`Microsoft.AspNet.SignalR`名前空間。</span><span class="sxs-lookup"><span data-stu-id="b2b58-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="b2b58-120">適用する、`Authorize`属性をハブまたはハブ内の特定のメソッドのいずれか。</span><span class="sxs-lookup"><span data-stu-id="b2b58-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="b2b58-121">適用すると、`Authorize`ハブ クラス、指定した承認要件に属性はすべてのハブ メソッドに適用します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="b2b58-122">適用可能な承認要件の種類は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b2b58-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="b2b58-123">なし、`Authorize`属性、ハブのすべてのパブリック メソッドがハブに接続されているクライアントで使用します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="b2b58-124">Web アプリケーションで"Admin"をという名前のロールを定義している場合は、次のコード ハブをそのロールのユーザーのみがアクセスできることを指定できます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="b2b58-125">または、すべてのユーザーに提供される 1 つのメソッドと 2 番目のメソッドだけに提供される認証されたユーザーは、次に示すようにハブが含まれているを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="b2b58-126">次の例は、さまざまな承認シナリオに対応します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="b2b58-127">`[Authorize]`– 認証されたユーザーのみ</span><span class="sxs-lookup"><span data-stu-id="b2b58-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="b2b58-128">`[Authorize(Roles = "Admin,Manager")]`– 指定したロールのユーザーを認証されたのみ</span><span class="sxs-lookup"><span data-stu-id="b2b58-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="b2b58-129">`[Authorize(Users = "user1,user2")]`– 指定されたユーザー名を持つユーザーを認証されたのみ</span><span class="sxs-lookup"><span data-stu-id="b2b58-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="b2b58-130">`[Authorize(RequireOutgoing=false)]`– だけで認証されたユーザーは、ハブを呼び出すことができますが、サーバーからクライアントに返送の呼び出しによる制限はありません、承認など、特定のユーザーだけがメッセージを送信できますが、他のすべてのユーザー メッセージが表示されることができます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="b2b58-131">RequireOutgoing プロパティは、ハブ内の個人用メソッドではなく、hub 全体にのみ適用できます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="b2b58-132">RequireOutgoing が false に設定されていない、サーバーから承認要件を満たすユーザーのみが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="b2b58-133">すべてのハブに対して認証を要求します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-133">Require authentication for all hubs</span></span>

<span data-ttu-id="b2b58-134">アプリケーションで呼び出すことによってすべてのハブおよびハブ メソッドの認証を要求できます、 [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx)メソッド アプリケーションの起動時にします。</span><span class="sxs-lookup"><span data-stu-id="b2b58-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="b2b58-135">複数のハブがあり、それらのすべての認証要件を強制する場合は、このメソッドを使用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="b2b58-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="b2b58-136">このメソッドを使用して役割、ユーザー、または送信の承認を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="b2b58-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="b2b58-137">のみ、ハブ メソッドへのアクセスが認証されたユーザーに制限されていることを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="b2b58-138">ただし、Authorize 属性には、ハブやその他の要件を指定する方法にも適用できます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="b2b58-139">属性で指定したすべての要件は、認証の基本的な要件に加えて適用されます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="b2b58-140">次の例では、すべてのハブ メソッドを認証されたユーザーに制限が Global.asax ファイルを示します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="b2b58-141">呼び出す場合は、 `RequireAuthentication()` SignalR 要求が処理された後、SignalR をスロー、`InvalidOperationException`例外。</span><span class="sxs-lookup"><span data-stu-id="b2b58-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="b2b58-142">パイプラインを呼び出した後に、モジュールを HubPipeline に追加することはできませんので、この例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="b2b58-143">前の例では、通話、`RequireAuthentication`メソッドで、`Application_Start`最初の要求を処理する前に 1 回実行されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="b2b58-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="b2b58-144">カスタマイズした承認</span><span class="sxs-lookup"><span data-stu-id="b2b58-144">Customized authorization</span></span>

<span data-ttu-id="b2b58-145">派生するクラスを作成するには承認を決定する方法をカスタマイズする必要がある場合`AuthorizeAttribute`をオーバーライドし、 [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx)メソッドです。</span><span class="sxs-lookup"><span data-stu-id="b2b58-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="b2b58-146">このメソッドは、ユーザーに承認要求を完了するかどうかを判断するには、各要求に対して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="b2b58-147">オーバーライドされたメソッドでは、承認のシナリオに必要なロジックを提供します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="b2b58-148">次の例では、クレーム ベース id を介して承認を適用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="b2b58-149">クライアントに認証情報を渡す</span><span class="sxs-lookup"><span data-stu-id="b2b58-149">Pass authentication information to clients</span></span>

<span data-ttu-id="b2b58-150">クライアントで実行されるコードでの認証情報を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b2b58-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="b2b58-151">クライアントで、メソッドを呼び出すときに、必要な情報を渡します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="b2b58-152">たとえば、チャット アプリケーション メソッドでしたをパラメーターとして渡す、メッセージの送信を行う人物のユーザー名次のようにします。</span><span class="sxs-lookup"><span data-stu-id="b2b58-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="b2b58-153">または、次に示すように、認証情報を表し、そのオブジェクトをパラメーターとして渡しオブジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="b2b58-154">悪意のあるユーザーを使用すると、そのクライアントからの要求を模倣するために使用できなかったと他のクライアントに 1 つのクライアント接続 id を渡してください。</span><span class="sxs-lookup"><span data-stu-id="b2b58-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="b2b58-155">.NET クライアントの認証オプション</span><span class="sxs-lookup"><span data-stu-id="b2b58-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="b2b58-156">認証されたユーザーに限られているハブをやり取りするコンソール アプリなどの .NET クライアントがある場合は、cookie、接続のヘッダー、または証明書で認証資格情報を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="b2b58-157">このセクションの例では、これらのさまざまなメソッドを使用してユーザーを認証する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="b2b58-158">SignalR アプリケーションを完全に機能はありません。</span><span class="sxs-lookup"><span data-stu-id="b2b58-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="b2b58-159">SignalR の .NET クライアントの詳細については、次を参照してください。 [Hubs API ガイド - .NET クライアント](../guide-to-the-api/hubs-api-guide-net-client.md)です。</span><span class="sxs-lookup"><span data-stu-id="b2b58-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="b2b58-160">クッキー</span><span class="sxs-lookup"><span data-stu-id="b2b58-160">Cookie</span></span>

<span data-ttu-id="b2b58-161">.NET クライアントは、ASP.NET フォーム認証を使用するハブと対話するとき、は、接続の認証クッキーを手動で設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b2b58-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="b2b58-162">Cookie を追加する、`CookieContainer`プロパティを[HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx)オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b2b58-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="b2b58-163">次の例では、web ページから、認証 cookie を取得し、接続にその cookie を追加するコンソール アプリを示します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="b2b58-164">URL `https://www.contoso.com/RemoteLogin` web ページを作成する必要がありますをする例をポイントします。</span><span class="sxs-lookup"><span data-stu-id="b2b58-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="b2b58-165">ページは、ポストされたユーザー名とパスワードを取得し、資格情報を持つユーザーにログインしようとしています。</span><span class="sxs-lookup"><span data-stu-id="b2b58-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="b2b58-166">コンソール アプリは、次の分離コード ファイルを含む空のページを参照する www.contoso.com/RemoteLogin する資格情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="b2b58-167">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="b2b58-167">Windows authentication</span></span>

<span data-ttu-id="b2b58-168">Windows 認証を使用する場合を使用して現在のユーザーの資格情報を渡すことができます、[される DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx)プロパティです。</span><span class="sxs-lookup"><span data-stu-id="b2b58-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="b2b58-169">される DefaultCredentials の値には、接続の資格情報を設定します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="b2b58-170">接続ヘッダー</span><span class="sxs-lookup"><span data-stu-id="b2b58-170">Connection header</span></span>

<span data-ttu-id="b2b58-171">アプリケーションが cookie を使用していない場合は、接続ヘッダーのユーザー情報を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="b2b58-172">たとえば、接続のヘッダーにトークンを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="b2b58-173">次に、ハブでは、ユーザーのトークンを確認します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="b2b58-174">証明書</span><span class="sxs-lookup"><span data-stu-id="b2b58-174">Certificate</span></span>

<span data-ttu-id="b2b58-175">ユーザーを確認するクライアント証明書を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="b2b58-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="b2b58-176">接続を作成するときに、証明書を追加します。</span><span class="sxs-lookup"><span data-stu-id="b2b58-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="b2b58-177">次の例では、接続にクライアント証明書を追加する方法のみコンソールへのフル アプリケーションは表示されません。</span><span class="sxs-lookup"><span data-stu-id="b2b58-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="b2b58-178">使用して、 [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx)証明書を作成するいくつかの方法を提供するクラス。</span><span class="sxs-lookup"><span data-stu-id="b2b58-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
