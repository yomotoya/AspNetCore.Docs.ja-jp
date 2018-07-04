---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: SignalR 永続的な接続の認証と承認 (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: このトピックでは、永続的な接続の承認を適用する方法を説明します。 概要については、SignalR アプリケーションでは、セキュリティと統合しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 471bce03a37a401c2afe3735afef6f838555cc26
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365111"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="08676-104">SignalR 永続的な接続の認証と承認 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="08676-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="08676-105">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="08676-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="08676-106">このトピックでは、永続的な接続の承認を適用する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="08676-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="08676-107">SignalR アプリケーションへのセキュリティの統合の詳細については、次を参照してください。[セキュリティの概要](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="08676-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="08676-108">承認を適用します。</span><span class="sxs-lookup"><span data-stu-id="08676-108">Enforce authorization</span></span>

<span data-ttu-id="08676-109">使用する場合は、承認規則を適用する、 [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)オーバーライドする必要があります、`AuthorizeRequest`メソッド。</span><span class="sxs-lookup"><span data-stu-id="08676-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="08676-110">使用することはできません、`Authorize`永続的な接続を持つ属性です。</span><span class="sxs-lookup"><span data-stu-id="08676-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="08676-111">`AuthorizeRequest`メソッドは要求ごとに、要求された操作を実行するユーザーが許可されていることを確認する前に SignalR フレームワークによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="08676-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="08676-112">`AuthorizeRequest`メソッドは、クライアントからは呼び出されません。 代わりに、アプリケーションの標準的な認証メカニズムを通じてユーザーを認証します。</span><span class="sxs-lookup"><span data-stu-id="08676-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="08676-113">次の例では、認証されたユーザーへの要求に制限する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="08676-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="08676-114">AuthorizeRequest メソッドで、カスタマイズされた承認ロジックを追加することができます。次のように、ユーザーが特定のロールに属しているかどうかを確認しています。</span><span class="sxs-lookup"><span data-stu-id="08676-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
