---
uid: signalr/overview/older-versions/hub-authorization
title: SignalR ハブの認証と承認 (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: このトピックでは、どのユーザーまたはロールがハブ メソッドにアクセスできる制限する方法について説明します。
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 1bb10a49a0d783300c145c30ad09e31f8e6055d6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808280"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="b9413-103">SignalR ハブの認証と承認 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="b9413-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>
====================
<span data-ttu-id="b9413-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b9413-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b9413-105">このトピックでは、どのユーザーまたはロールがハブ メソッドにアクセスできる制限する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b9413-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>


## <a name="overview"></a><span data-ttu-id="b9413-106">概要</span><span class="sxs-lookup"><span data-stu-id="b9413-106">Overview</span></span>

<span data-ttu-id="b9413-107">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="b9413-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="b9413-108">属性を承認します。</span><span class="sxs-lookup"><span data-stu-id="b9413-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="b9413-109">すべてのハブの認証が必要です。</span><span class="sxs-lookup"><span data-stu-id="b9413-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="b9413-110">カスタマイズされた承認</span><span class="sxs-lookup"><span data-stu-id="b9413-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="b9413-111">クライアントに認証情報を渡す</span><span class="sxs-lookup"><span data-stu-id="b9413-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="b9413-112">.NET クライアントの認証オプション</span><span class="sxs-lookup"><span data-stu-id="b9413-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="b9413-113">フォーム認証 cookie</span><span class="sxs-lookup"><span data-stu-id="b9413-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="b9413-114">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="b9413-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="b9413-115">接続ヘッダー</span><span class="sxs-lookup"><span data-stu-id="b9413-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="b9413-116">証明書</span><span class="sxs-lookup"><span data-stu-id="b9413-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="b9413-117">属性を承認します。</span><span class="sxs-lookup"><span data-stu-id="b9413-117">Authorize attribute</span></span>

<span data-ttu-id="b9413-118">SignalR の提供、 [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)ハブまたはメソッドへのアクセスを持つユーザーまたはロールを指定する属性。</span><span class="sxs-lookup"><span data-stu-id="b9413-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="b9413-119">この属性にある、`Microsoft.AspNet.SignalR`名前空間。</span><span class="sxs-lookup"><span data-stu-id="b9413-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="b9413-120">適用する、`Authorize`属性をハブまたはハブ内の特定のメソッドのいずれか。</span><span class="sxs-lookup"><span data-stu-id="b9413-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="b9413-121">適用すると、 `Authorize` hub クラスでは、指定された承認要件を属性のすべてのハブ メソッドに適用されます。</span><span class="sxs-lookup"><span data-stu-id="b9413-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="b9413-122">適用できる承認要件のさまざまな種類は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="b9413-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="b9413-123">なし、`Authorize`属性、ハブのすべてのパブリック メソッドは、ハブに接続されているクライアントで使用します。</span><span class="sxs-lookup"><span data-stu-id="b9413-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="b9413-124">Web アプリケーションで"Admin"をという名前のロールを定義している場合は、次のコードでのハブをそのロール内のユーザーのみがアクセスできることを指定できます。</span><span class="sxs-lookup"><span data-stu-id="b9413-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="b9413-125">または、すべてのユーザーに提供される 1 つのメソッドと 2 番目のメソッドのみが利用できる認証されたユーザーは、次に示すようにハブが含まれているを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="b9413-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="b9413-126">次の例は、さまざまな承認シナリオに対処します。</span><span class="sxs-lookup"><span data-stu-id="b9413-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="b9413-127">`[Authorize]` -認証されたユーザーのみ</span><span class="sxs-lookup"><span data-stu-id="b9413-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="b9413-128">`[Authorize(Roles = "Admin,Manager")]` – 指定したロールのユーザーを認証されたのみ</span><span class="sxs-lookup"><span data-stu-id="b9413-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="b9413-129">`[Authorize(Users = "user1,user2")]` – 指定されたユーザー名を持つユーザーを認証されたのみ</span><span class="sxs-lookup"><span data-stu-id="b9413-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="b9413-130">`[Authorize(RequireOutgoing=false)]` – だけで認証されたユーザーは、ハブで呼び出すことができますが、サーバーからクライアントに返送の呼び出しによる制限はありません、承認など、特定のユーザーだけがメッセージを送信できますが、他のすべてのユーザー メッセージが表示されることができます。</span><span class="sxs-lookup"><span data-stu-id="b9413-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="b9413-131">RequireOutgoing プロパティは、ハブ内の個人メソッドではなく、全体のハブにのみ適用できます。</span><span class="sxs-lookup"><span data-stu-id="b9413-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="b9413-132">RequireOutgoing が false に設定されていない場合は、サーバーから承認要件を満たすユーザーのみが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b9413-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="b9413-133">すべてのハブの認証が必要です。</span><span class="sxs-lookup"><span data-stu-id="b9413-133">Require authentication for all hubs</span></span>

<span data-ttu-id="b9413-134">アプリケーションで呼び出すことによってすべてのハブおよびハブ メソッドの認証を要求できます、 [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx)メソッド、アプリケーションの起動時にします。</span><span class="sxs-lookup"><span data-stu-id="b9413-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="b9413-135">複数のハブおよびすべての認証の要件を適用するした場合、このメソッドを使用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="b9413-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="b9413-136">この方法では、ロール、ユーザー、または送信の承認を指定できません。</span><span class="sxs-lookup"><span data-stu-id="b9413-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="b9413-137">ハブ メソッドへのアクセスが認証されたユーザーに制限のみ指定することができます。</span><span class="sxs-lookup"><span data-stu-id="b9413-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="b9413-138">ただし、ハブまたはその他の要件を指定するメソッドに、Authorize 属性を適用できます。</span><span class="sxs-lookup"><span data-stu-id="b9413-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="b9413-139">属性で指定する必要は、認証の基本的な要件に加えて適用されます。</span><span class="sxs-lookup"><span data-stu-id="b9413-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="b9413-140">次の例では、すべてのハブ メソッドを認証されたユーザーに制限する Global.asax ファイルを示します。</span><span class="sxs-lookup"><span data-stu-id="b9413-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="b9413-141">呼び出す場合、`RequireAuthentication()`メソッド SignalR 要求の処理が完了したら、SignalR がスローされます、`InvalidOperationException`例外。</span><span class="sxs-lookup"><span data-stu-id="b9413-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="b9413-142">パイプラインが呼び出された後、モジュールを HubPipeline に追加することはできませんので、この例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="b9413-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="b9413-143">前の例では、呼び出しを示しています、`RequireAuthentication`メソッドで、`Application_Start`メソッドの最初の要求を処理する前に 1 回実行されます。</span><span class="sxs-lookup"><span data-stu-id="b9413-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="b9413-144">カスタマイズされた承認</span><span class="sxs-lookup"><span data-stu-id="b9413-144">Customized authorization</span></span>

<span data-ttu-id="b9413-145">派生したクラスを作成するには承認を決定する方法をカスタマイズする必要がある場合`AuthorizeAttribute`をオーバーライドし、 [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx)メソッド。</span><span class="sxs-lookup"><span data-stu-id="b9413-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="b9413-146">このメソッドは、ユーザーに承認要求を完了するかどうかを判断するには、各要求に対して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b9413-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="b9413-147">オーバーライドされたメソッドで、承認のシナリオに必要なロジックを提供します。</span><span class="sxs-lookup"><span data-stu-id="b9413-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="b9413-148">次の例では、クレーム ベース id 経由で認証を実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b9413-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="b9413-149">クライアントに認証情報を渡す</span><span class="sxs-lookup"><span data-stu-id="b9413-149">Pass authentication information to clients</span></span>

<span data-ttu-id="b9413-150">クライアントで実行されるコードで認証情報を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b9413-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="b9413-151">クライアントでメソッドの呼び出し時に、必要な情報を渡します。</span><span class="sxs-lookup"><span data-stu-id="b9413-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="b9413-152">たとえば、チャット アプリケーションの方法でしたをパラメーターとして渡す、メッセージを投稿する人のユーザー名次に示すよう。</span><span class="sxs-lookup"><span data-stu-id="b9413-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="b9413-153">または、次に示すように認証情報を表し、そのオブジェクトをパラメーターとして渡すオブジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="b9413-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="b9413-154">悪意のあるユーザーがそのクライアントからの要求を模倣するために使用できますよう他のクライアントに 1 つのクライアント接続 id を渡してください。</span><span class="sxs-lookup"><span data-stu-id="b9413-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="b9413-155">.NET クライアントの認証オプション</span><span class="sxs-lookup"><span data-stu-id="b9413-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="b9413-156">認証されたユーザーを制限するハブとの対話、により、コンソール アプリなどの .NET クライアントがある場合は、cookie、connection ヘッダー。 または、証明書で認証資格情報を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="b9413-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="b9413-157">このセクションの例では、ユーザーを認証するため、これらのさまざまなメソッドを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b9413-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="b9413-158">完全に機能する SignalR アプリケーションではありません。</span><span class="sxs-lookup"><span data-stu-id="b9413-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="b9413-159">SignalR を使って .NET クライアントの詳細については、次を参照してください。[ハブ API ガイド - .NET クライアント](../guide-to-the-api/hubs-api-guide-net-client.md)します。</span><span class="sxs-lookup"><span data-stu-id="b9413-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="b9413-160">クッキー</span><span class="sxs-lookup"><span data-stu-id="b9413-160">Cookie</span></span>

<span data-ttu-id="b9413-161">ASP.NET フォーム認証を使用するハブを操作すると、.NET クライアント、接続の認証クッキーを手動で設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b9413-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="b9413-162">Cookie を追加する、`CookieContainer`プロパティを[HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx)オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b9413-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="b9413-163">次の例では、web ページから、認証 cookie を取得し、接続をそのクッキーを追加します。 コンソール アプリを示します。</span><span class="sxs-lookup"><span data-stu-id="b9413-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="b9413-164">URL`https://www.contoso.com/RemoteLogin`で例を作成する必要のある web ページ。</span><span class="sxs-lookup"><span data-stu-id="b9413-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="b9413-165">ページは、ポストされたユーザー名とパスワードを取得し、資格情報でユーザーにログインしようとしています。</span><span class="sxs-lookup"><span data-stu-id="b9413-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="b9413-166">コンソール アプリはでした次のコード ビハインド ファイルを含む空のページを参照している www.contoso.com/RemoteLogin に資格情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="b9413-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="b9413-167">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="b9413-167">Windows authentication</span></span>

<span data-ttu-id="b9413-168">Windows 認証を使用する場合を使用して現在のユーザーの資格情報を渡すことができます、[される DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="b9413-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="b9413-169">される DefaultCredentials の値には、接続の資格情報を設定します。</span><span class="sxs-lookup"><span data-stu-id="b9413-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="b9413-170">接続ヘッダー</span><span class="sxs-lookup"><span data-stu-id="b9413-170">Connection header</span></span>

<span data-ttu-id="b9413-171">アプリケーションが cookie を使用していない場合は、ユーザー情報を接続ヘッダーで渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="b9413-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="b9413-172">たとえば、接続ヘッダーでトークンを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="b9413-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="b9413-173">次に、ハブでは、ユーザーのトークンを確認します。</span><span class="sxs-lookup"><span data-stu-id="b9413-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="b9413-174">証明書</span><span class="sxs-lookup"><span data-stu-id="b9413-174">Certificate</span></span>

<span data-ttu-id="b9413-175">ユーザーを確認するクライアント証明書を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="b9413-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="b9413-176">接続を作成するときに、証明書を追加します。</span><span class="sxs-lookup"><span data-stu-id="b9413-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="b9413-177">次の例では、接続にクライアント証明書を追加する方法のみを示しています。完全なコンソール アプリは表示されません。</span><span class="sxs-lookup"><span data-stu-id="b9413-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="b9413-178">使用して、 [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx)クラス、証明書を作成するいくつかの方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="b9413-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
