---
uid: signalr/overview/security/persistent-connection-authorization
title: SignalR 永続的な接続の認証と承認 |Microsoft Docs
author: pfletcher
description: このトピックでは、永続的な接続の承認を適用する方法を説明します。 概要については、SignalR アプリケーションでは、セキュリティと統合しています.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: bbbcb5593fb265eca4fb261d378532047d53e674
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287391"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="39d7b-104">SignalR 永続的な接続の認証と承認</span><span class="sxs-lookup"><span data-stu-id="39d7b-104">Authentication and Authorization for SignalR Persistent Connections</span></span>
====================
<span data-ttu-id="39d7b-105">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="39d7b-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="39d7b-106">このトピックでは、永続的な接続の承認を適用する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="39d7b-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="39d7b-107">SignalR アプリケーションへのセキュリティの統合の詳細については、次を参照してください。[セキュリティの概要](introduction-to-security.md)します。</span><span class="sxs-lookup"><span data-stu-id="39d7b-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="39d7b-108">このトピックで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="39d7b-108">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="39d7b-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="39d7b-109">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="39d7b-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="39d7b-110">.NET 4.5</span></span>
> - <span data-ttu-id="39d7b-111">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="39d7b-111">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="39d7b-112">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="39d7b-112">Previous versions of this topic</span></span>
>
> <span data-ttu-id="39d7b-113">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="39d7b-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="39d7b-114">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="39d7b-114">Questions and comments</span></span>
>
> <span data-ttu-id="39d7b-115">このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。</span><span class="sxs-lookup"><span data-stu-id="39d7b-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="39d7b-116">チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。</span><span class="sxs-lookup"><span data-stu-id="39d7b-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="39d7b-117">承認を適用します。</span><span class="sxs-lookup"><span data-stu-id="39d7b-117">Enforce authorization</span></span>

<span data-ttu-id="39d7b-118">使用する場合は、承認規則を適用する、 [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)オーバーライドする必要があります、`AuthorizeRequest`メソッド。</span><span class="sxs-lookup"><span data-stu-id="39d7b-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="39d7b-119">使用することはできません、`Authorize`永続的な接続を持つ属性です。</span><span class="sxs-lookup"><span data-stu-id="39d7b-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="39d7b-120">`AuthorizeRequest`メソッドは要求ごとに、要求された操作を実行するユーザーが許可されていることを確認する前に SignalR フレームワークによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="39d7b-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="39d7b-121">`AuthorizeRequest`メソッドは、クライアントからは呼び出されません。 代わりに、アプリケーションの標準的な認証メカニズムを通じてユーザーを認証します。</span><span class="sxs-lookup"><span data-stu-id="39d7b-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="39d7b-122">次の例では、認証されたユーザーへの要求に制限する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="39d7b-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="39d7b-123">AuthorizeRequest メソッドで、カスタマイズされた承認ロジックを追加することができます。次のように、ユーザーが特定のロールに属しているかどうかを確認しています。</span><span class="sxs-lookup"><span data-stu-id="39d7b-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
