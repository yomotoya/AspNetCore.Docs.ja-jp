---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: "SignalR 固定接続の認証と承認 (SignalR 1.x) |Microsoft ドキュメント"
author: pfletcher
description: "このトピックでは、永続的な接続の承認を適用する方法について説明します。 概要については、SignalR アプリケーションへのセキュリティの統合しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 4c036ddf1e20e3a3be7b043d90b594292013f6c2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="807dd-104">SignalR 固定接続の認証と承認 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="807dd-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="807dd-105">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="807dd-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="807dd-106">このトピックでは、永続的な接続の承認を適用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="807dd-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="807dd-107">SignalR アプリケーションへのセキュリティの統合の詳細については、次を参照してください。[セキュリティの概要](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="807dd-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="807dd-108">承認を適用します。</span><span class="sxs-lookup"><span data-stu-id="807dd-108">Enforce authorization</span></span>

<span data-ttu-id="807dd-109">使用する場合は、承認規則を適用する、 [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)オーバーライドする必要があります、`AuthorizeRequest`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="807dd-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="807dd-110">使用することはできません、`Authorize`永続的な接続の属性です。</span><span class="sxs-lookup"><span data-stu-id="807dd-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="807dd-111">`AuthorizeRequest`メソッドが要求された操作を実行するユーザーが許可されていることを確認するすべての要求の前に、SignalR フレームワークによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="807dd-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="807dd-112">`AuthorizeRequest`メソッドは、クライアントからは呼び出されません以外の場合は、アプリケーションの標準の認証メカニズムにより、ユーザーを認証する代わりに、します。</span><span class="sxs-lookup"><span data-stu-id="807dd-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="807dd-113">次の例では、認証されたユーザーに要求を制限する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="807dd-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="807dd-114">AuthorizeRequest メソッドの任意のカスタマイズされた承認ロジックを追加することができます。など、ユーザーが特定のロールに属しているかどうかを確認しています。</span><span class="sxs-lookup"><span data-stu-id="807dd-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
