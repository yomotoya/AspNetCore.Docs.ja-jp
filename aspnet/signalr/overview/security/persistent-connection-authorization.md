---
uid: signalr/overview/security/persistent-connection-authorization
title: "SignalR 固定接続の認証と承認 |Microsoft ドキュメント"
author: pfletcher
description: "このトピックでは、永続的な接続の承認を適用する方法について説明します。 概要については、SignalR アプリケーションへのセキュリティの統合しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: d559cfa21f6444b2361fd003b9ce3d2c9c6c57a4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="f8339-104">SignalR 固定接続の認証と承認</span><span class="sxs-lookup"><span data-stu-id="f8339-104">Authentication and Authorization for SignalR Persistent Connections</span></span>
====================
<span data-ttu-id="f8339-105">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f8339-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f8339-106">このトピックでは、永続的な接続の承認を適用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f8339-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="f8339-107">SignalR アプリケーションへのセキュリティの統合の詳細については、次を参照してください。[セキュリティの概要](introduction-to-security.md)です。</span><span class="sxs-lookup"><span data-stu-id="f8339-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="f8339-108">このトピックで使用されているソフトウェア バージョン</span><span class="sxs-lookup"><span data-stu-id="f8339-108">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="f8339-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f8339-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="f8339-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f8339-110">.NET 4.5</span></span>
> - <span data-ttu-id="f8339-111">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="f8339-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="f8339-112">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="f8339-112">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="f8339-113">SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="f8339-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="f8339-114">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="f8339-114">Questions and comments</span></span>
> 
> <span data-ttu-id="f8339-115">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="f8339-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f8339-116">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="f8339-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="f8339-117">承認を適用します。</span><span class="sxs-lookup"><span data-stu-id="f8339-117">Enforce authorization</span></span>

<span data-ttu-id="f8339-118">使用する場合は、承認規則を適用する、 [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)オーバーライドする必要があります、`AuthorizeRequest`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="f8339-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="f8339-119">使用することはできません、`Authorize`永続的な接続の属性です。</span><span class="sxs-lookup"><span data-stu-id="f8339-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="f8339-120">`AuthorizeRequest`メソッドが要求された操作を実行するユーザーが許可されていることを確認するすべての要求の前に、SignalR フレームワークによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f8339-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="f8339-121">`AuthorizeRequest`メソッドは、クライアントからは呼び出されません以外の場合は、アプリケーションの標準の認証メカニズムにより、ユーザーを認証する代わりに、します。</span><span class="sxs-lookup"><span data-stu-id="f8339-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="f8339-122">次の例では、認証されたユーザーに要求を制限する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f8339-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="f8339-123">AuthorizeRequest メソッドの任意のカスタマイズされた承認ロジックを追加することができます。など、ユーザーが特定のロールに属しているかどうかを確認しています。</span><span class="sxs-lookup"><span data-stu-id="f8339-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
