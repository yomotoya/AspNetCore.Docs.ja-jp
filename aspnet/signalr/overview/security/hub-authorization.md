---
uid: signalr/overview/security/hub-authorization
title: SignalR ハブの認証と承認 |Microsoft Docs
author: pfletcher
description: このトピックでは、どのユーザーまたはロールがハブ メソッドにアクセスできる制限する方法について説明します。 ソフトウェアのバージョンは、このトピックの「Visual Studio 2013 .NET 4.5 SignalR ve をを使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 6d351542a3238cbb8168ac20bcba559551837351
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361944"
---
<a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="3c2dc-104">SignalR ハブの認証と承認</span><span class="sxs-lookup"><span data-stu-id="3c2dc-104">Authentication and Authorization for SignalR Hubs</span></span>
====================
<span data-ttu-id="3c2dc-105">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3c2dc-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3c2dc-106">このトピックでは、どのユーザーまたはロールがハブ メソッドにアクセスできる制限する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-106">This topic describes how to restrict which users or roles can access hub methods.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="3c2dc-107">このトピックで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="3c2dc-107">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="3c2dc-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3c2dc-108">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="3c2dc-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3c2dc-109">.NET 4.5</span></span>
> - <span data-ttu-id="3c2dc-110">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="3c2dc-110">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="3c2dc-111">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="3c2dc-111">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="3c2dc-112">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="3c2dc-113">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="3c2dc-113">Questions and comments</span></span>
> 
> <span data-ttu-id="3c2dc-114">このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="3c2dc-115">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="3c2dc-116">概要</span><span class="sxs-lookup"><span data-stu-id="3c2dc-116">Overview</span></span>

<span data-ttu-id="3c2dc-117">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="3c2dc-118">属性を承認します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="3c2dc-119">すべてのハブの認証が必要です。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="3c2dc-120">カスタマイズされた承認</span><span class="sxs-lookup"><span data-stu-id="3c2dc-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="3c2dc-121">クライアントに認証情報を渡す</span><span class="sxs-lookup"><span data-stu-id="3c2dc-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="3c2dc-122">.NET クライアントの認証オプション</span><span class="sxs-lookup"><span data-stu-id="3c2dc-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="3c2dc-123">フォーム認証 cookie</span><span class="sxs-lookup"><span data-stu-id="3c2dc-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="3c2dc-124">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="3c2dc-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="3c2dc-125">接続ヘッダー</span><span class="sxs-lookup"><span data-stu-id="3c2dc-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="3c2dc-126">証明書</span><span class="sxs-lookup"><span data-stu-id="3c2dc-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="3c2dc-127">属性を承認します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-127">Authorize attribute</span></span>

<span data-ttu-id="3c2dc-128">SignalR の提供、 [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)ハブまたはメソッドへのアクセスを持つユーザーまたはロールを指定する属性。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="3c2dc-129">この属性にある、`Microsoft.AspNet.SignalR`名前空間。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="3c2dc-130">適用する、`Authorize`属性をハブまたはハブ内の特定のメソッドのいずれか。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="3c2dc-131">適用すると、 `Authorize` hub クラスでは、指定された承認要件を属性のすべてのハブ メソッドに適用されます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="3c2dc-132">このトピックでは、適用できる承認要件のさまざまな種類の例を示します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="3c2dc-133">なし、`Authorize`属性は、接続されたクライアントがハブのすべてのパブリック メソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="3c2dc-134">Web アプリケーションで"Admin"をという名前のロールを定義している場合は、次のコードでのハブをそのロール内のユーザーのみがアクセスできることを指定できます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="3c2dc-135">または、すべてのユーザーに提供される 1 つのメソッドと 2 番目のメソッドのみが利用できる認証されたユーザーは、次に示すようにハブが含まれているを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="3c2dc-136">次の例は、さまざまな承認シナリオに対処します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="3c2dc-137">`[Authorize]` -認証されたユーザーのみ</span><span class="sxs-lookup"><span data-stu-id="3c2dc-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="3c2dc-138">`[Authorize(Roles = "Admin,Manager")]` – 指定したロールのユーザーを認証されたのみ</span><span class="sxs-lookup"><span data-stu-id="3c2dc-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="3c2dc-139">`[Authorize(Users = "user1,user2")]` – 指定されたユーザー名を持つユーザーを認証されたのみ</span><span class="sxs-lookup"><span data-stu-id="3c2dc-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="3c2dc-140">`[Authorize(RequireOutgoing=false)]` – だけで認証されたユーザーは、ハブで呼び出すことができますが、サーバーからクライアントに返送の呼び出しによる制限はありません、承認など、特定のユーザーだけがメッセージを送信できますが、他のすべてのユーザー メッセージが表示されることができます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="3c2dc-141">RequireOutgoing プロパティは、ハブ内の個人メソッドではなく、全体のハブにのみ適用できます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="3c2dc-142">RequireOutgoing が false に設定されていない場合は、サーバーから承認要件を満たすユーザーのみが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="3c2dc-143">すべてのハブの認証が必要です。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-143">Require authentication for all hubs</span></span>

<span data-ttu-id="3c2dc-144">アプリケーションで呼び出すことによってすべてのハブおよびハブ メソッドの認証を要求できます、 [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx)メソッド、アプリケーションの起動時にします。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="3c2dc-145">複数のハブおよびすべての認証の要件を適用するした場合、このメソッドを使用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="3c2dc-146">この方法では、ロール、ユーザー、または送信の承認の要件を指定できません。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="3c2dc-147">ハブ メソッドへのアクセスが認証されたユーザーに制限のみ指定することができます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="3c2dc-148">ただし、ハブまたはその他の要件を指定するメソッドに、Authorize 属性を適用できます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="3c2dc-149">属性で指定したすべての要件は、認証の基本的な要件に追加されます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="3c2dc-150">次の例では、すべてのハブ メソッドを認証されたユーザーに制限するスタートアップ ファイルを示します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="3c2dc-151">呼び出す場合、`RequireAuthentication()`メソッド SignalR 要求の処理が完了したら、SignalR がスローされます、`InvalidOperationException`例外。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="3c2dc-152">SignalR は、パイプラインが呼び出された後、モジュールを HubPipeline に追加することはできませんので、この例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="3c2dc-153">前の例では、呼び出しを示しています、`RequireAuthentication`メソッドで、`Configuration`メソッドの最初の要求を処理する前に 1 回実行されます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="3c2dc-154">カスタマイズされた承認</span><span class="sxs-lookup"><span data-stu-id="3c2dc-154">Customized authorization</span></span>

<span data-ttu-id="3c2dc-155">派生したクラスを作成するには承認を決定する方法をカスタマイズする必要がある場合`AuthorizeAttribute`をオーバーライドし、 [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx)メソッド。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="3c2dc-156">要求ごとには、SignalR は、ユーザーに承認要求を完了するかどうかを判断するには、このメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="3c2dc-157">オーバーライドされたメソッドで、承認のシナリオに必要なロジックを提供します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="3c2dc-158">次の例では、クレーム ベース id 経由で認証を実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="3c2dc-159">クライアントに認証情報を渡す</span><span class="sxs-lookup"><span data-stu-id="3c2dc-159">Pass authentication information to clients</span></span>

<span data-ttu-id="3c2dc-160">クライアントで実行されるコードで認証情報を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="3c2dc-161">クライアントでメソッドの呼び出し時に、必要な情報を渡します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="3c2dc-162">たとえば、チャット アプリケーションの方法でしたをパラメーターとして渡す、メッセージを投稿する人のユーザー名次に示すよう。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="3c2dc-163">または、次に示すように認証情報を表し、そのオブジェクトをパラメーターとして渡すオブジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="3c2dc-164">悪意のあるユーザーがそのクライアントからの要求を模倣するために使用できますよう他のクライアントに 1 つのクライアント接続 id を渡してください。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="3c2dc-165">.NET クライアントの認証オプション</span><span class="sxs-lookup"><span data-stu-id="3c2dc-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="3c2dc-166">認証されたユーザーを制限するハブとの対話、により、コンソール アプリなどの .NET クライアントがある場合は、cookie、connection ヘッダー。 または、証明書で認証資格情報を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="3c2dc-167">このセクションの例では、ユーザーを認証するため、これらのさまざまなメソッドを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="3c2dc-168">完全に機能する SignalR アプリケーションではありません。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="3c2dc-169">SignalR を使って .NET クライアントの詳細については、次を参照してください。[ハブ API ガイド - .NET クライアント](../guide-to-the-api/hubs-api-guide-net-client.md)します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="3c2dc-170">クッキー</span><span class="sxs-lookup"><span data-stu-id="3c2dc-170">Cookie</span></span>

<span data-ttu-id="3c2dc-171">ASP.NET フォーム認証を使用するハブを操作すると、.NET クライアント、接続の認証クッキーを手動で設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="3c2dc-172">Cookie を追加する、`CookieContainer`プロパティを[HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx)オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="3c2dc-173">次の例では、web ページから、認証 cookie を取得し、接続をそのクッキーを追加します。 コンソール アプリを示します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="3c2dc-174">コンソール アプリに資格情報を投稿する<strong>www.contoso.com/RemoteLogin</strong>次のコード ビハインド ファイルを含む空のページを参照する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-174">The console app posts the credentials to <strong>www.contoso.com/RemoteLogin</strong> which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="3c2dc-175">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="3c2dc-175">Windows authentication</span></span>

<span data-ttu-id="3c2dc-176">Windows 認証を使用する場合を使用して現在のユーザーの資格情報を渡すことができます、[される DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="3c2dc-177">される DefaultCredentials の値には、接続の資格情報を設定します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="3c2dc-178">接続ヘッダー</span><span class="sxs-lookup"><span data-stu-id="3c2dc-178">Connection header</span></span>

<span data-ttu-id="3c2dc-179">アプリケーションが cookie を使用していない場合は、ユーザー情報を接続ヘッダーで渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="3c2dc-180">たとえば、接続ヘッダーでトークンを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="3c2dc-181">次に、ハブでは、ユーザーのトークンを確認します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="3c2dc-182">証明書</span><span class="sxs-lookup"><span data-stu-id="3c2dc-182">Certificate</span></span>

<span data-ttu-id="3c2dc-183">ユーザーを確認するクライアント証明書を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="3c2dc-184">接続を作成するときに、証明書を追加します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="3c2dc-185">次の例では、接続にクライアント証明書を追加する方法のみを示しています。完全なコンソール アプリは表示されません。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="3c2dc-186">使用して、 [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx)クラス、証明書を作成するいくつかの方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="3c2dc-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
