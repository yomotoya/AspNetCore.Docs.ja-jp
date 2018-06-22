---
title: ASP.NET Core SignalR でサポートされているプラットフォーム
author: rachelappel
description: ASP.NET Core SignalR でサポートされるプラットフォーム
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/supported-platforms
ms.openlocfilehash: 26365bf62ac935eda4ab119a834e753ba40e6123
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274335"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="e9af3-103">ASP.NET Core SignalR でサポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="e9af3-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="e9af3-104">サーバーのシステム要件</span><span class="sxs-lookup"><span data-stu-id="e9af3-104">Server system requirements</span></span>

<span data-ttu-id="e9af3-105">ASP.NET Core の SignalR には、ASP.NET Core をサポートしている任意のサーバー プラットフォームがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="e9af3-105">SignalR for ASP.NET Core supports any server platform ASP.NET Core supports.</span></span>

## <a name="client-system-requirements"></a><span data-ttu-id="e9af3-106">クライアントのシステム要件</span><span class="sxs-lookup"><span data-stu-id="e9af3-106">Client system requirements</span></span>

### <a name="browser-support"></a><span data-ttu-id="e9af3-107">ブラウザーのサポート</span><span class="sxs-lookup"><span data-stu-id="e9af3-107">Browser support</span></span>

<span data-ttu-id="e9af3-108">ASP.NET Core JavaScript クライアント用 SignalR には、次のブラウザーがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="e9af3-108">The SignalR for ASP.NET Core JavaScript client supports the following browsers:</span></span>

| <span data-ttu-id="e9af3-109">ブラウザー</span><span class="sxs-lookup"><span data-stu-id="e9af3-109">Browser</span></span> | <span data-ttu-id="e9af3-110">Version</span><span class="sxs-lookup"><span data-stu-id="e9af3-110">Version</span></span> |
| ------- | ------- |
| <span data-ttu-id="e9af3-111">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="e9af3-111">Microsoft Internet Explorer</span></span> | <span data-ttu-id="e9af3-112">11</span><span class="sxs-lookup"><span data-stu-id="e9af3-112">11</span></span> |
| <span data-ttu-id="e9af3-113">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="e9af3-113">Microsoft Edge</span></span> | <span data-ttu-id="e9af3-114">現在の</span><span class="sxs-lookup"><span data-stu-id="e9af3-114">current</span></span> |
| <span data-ttu-id="e9af3-115">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="e9af3-115">Mozilla Firefox</span></span> | <span data-ttu-id="e9af3-116">現在の</span><span class="sxs-lookup"><span data-stu-id="e9af3-116">current</span></span> |
| <span data-ttu-id="e9af3-117">Google Chrome です。Android が含まれています</span><span class="sxs-lookup"><span data-stu-id="e9af3-117">Google Chrome; includes Android</span></span> | <span data-ttu-id="e9af3-118">現在の</span><span class="sxs-lookup"><span data-stu-id="e9af3-118">current</span></span> |
| <span data-ttu-id="e9af3-119">Safari です。iOS が含まれています</span><span class="sxs-lookup"><span data-stu-id="e9af3-119">Safari; includes iOS</span></span> | <span data-ttu-id="e9af3-120">現在の</span><span class="sxs-lookup"><span data-stu-id="e9af3-120">current</span></span> |
 
### <a name="net-client-support"></a><span data-ttu-id="e9af3-121">.NET クライアントのサポート</span><span class="sxs-lookup"><span data-stu-id="e9af3-121">.NET Client support</span></span>

<span data-ttu-id="e9af3-122">任意のサーバーは、ASP.NET Core でサポートされているプラットフォーム。</span><span class="sxs-lookup"><span data-stu-id="e9af3-122">Any server platform supported by ASP.NET Core.</span></span> <span data-ttu-id="e9af3-123">IIS を使用して、Windows Server 2012 またはそれ以上 Websocket トランスポート IIS 8.0 以上必要です。</span><span class="sxs-lookup"><span data-stu-id="e9af3-123">When using IIS, the WebSockets transport requires IIS 8.0 or higher, on Windows Server 2012 or higher.</span></span> <span data-ttu-id="e9af3-124">他のトランスポートは、すべてのプラットフォームでサポートされます。</span><span class="sxs-lookup"><span data-stu-id="e9af3-124">Other transports are supported on all platforms.</span></span>