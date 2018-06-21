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
ms.openlocfilehash: 7f8ccff7e3da93d6e617505ac93fafc3a82ed880
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252010"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="4319f-103">ASP.NET Core でのホスティング</span><span class="sxs-lookup"><span data-stu-id="4319f-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="4319f-104">.NET アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="4319f-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="4319f-105">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="4319f-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="4319f-106">次の 2 つのホスト API を使用できます。</span><span class="sxs-lookup"><span data-stu-id="4319f-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="4319f-107">[Web ホスト](xref:fundamentals/host/web-host) &ndash; Web アプリのホスティングに適しています。</span><span class="sxs-lookup"><span data-stu-id="4319f-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="4319f-108">[汎用ホスト](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 以降)&ndash; Web 以外のアプリ (バックグラウンド タスクを実行するアプリなど) のホスティングに適しています。</span><span class="sxs-lookup"><span data-stu-id="4319f-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="4319f-109">将来のリリースでは、汎用ホストが Web アプリを含むすべての種類のアプリのホスティングに適するようになります。</span><span class="sxs-lookup"><span data-stu-id="4319f-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="4319f-110">Web ホストは最終的に汎用ホストに置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="4319f-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="4319f-111">ASP.NET Core *Web アプリ*をホスティングする場合は、[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) に基づいた Web ホストを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4319f-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="4319f-112">*Web アプリ以外*をホスティングする場合は、[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) に基づいた汎用ホストを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4319f-112">For hosting *non-web apps*, developers should use the Generic Host based on [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span></span>
