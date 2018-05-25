---
title: ASP.NET Core でのホスティング
author: guardrex
description: アプリの起動と有効期間の管理を担当する、ASP.NET Core Web ホストと .NET 汎用ホストについて説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="98df9-103">ASP.NET Core でのホスティング</span><span class="sxs-lookup"><span data-stu-id="98df9-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="98df9-104">.NET アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="98df9-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="98df9-105">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="98df9-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="98df9-106">次の 2 つのホスト API を使用できます。</span><span class="sxs-lookup"><span data-stu-id="98df9-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="98df9-107">[Web ホスト](xref:fundamentals/host/web-host) &ndash; Web アプリのホスティングに適しています。</span><span class="sxs-lookup"><span data-stu-id="98df9-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="98df9-108">[汎用ホスト](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 以降)&ndash; Web 以外のアプリ (バックグラウンド タスクを実行するアプリなど) のホスティングに適しています。</span><span class="sxs-lookup"><span data-stu-id="98df9-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="98df9-109">将来のリリースでは、汎用ホストが Web アプリを含むすべての種類のアプリのホスティングに適するようになります。</span><span class="sxs-lookup"><span data-stu-id="98df9-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="98df9-110">Web ホストは最終的に汎用ホストに置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="98df9-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="98df9-111">現時点で ASP.NET Core アプリをホスティングする場合は、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) に基づいた [Web ホスト](xref:fundamentals/host/web-host)を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="98df9-111">At this time, developers should use the [Web Host](xref:fundamentals/host/web-host) based on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) for hosting ASP.NET Core apps.</span></span>
