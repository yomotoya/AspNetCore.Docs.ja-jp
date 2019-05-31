---
title: サーバー側 Blazor をホストおよび展開する
author: guardrex
description: ASP.NET Core を使用して Blazor サーバー側アプリをホストおよびデプロイする方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/26/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8e44be09a4cceba2509f3e86abf3ce5fd2d077bd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887767"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="576d7-103">サーバー側 Blazor をホストおよび展開する</span><span class="sxs-lookup"><span data-stu-id="576d7-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="576d7-104">作成者: [Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com)、[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="576d7-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="576d7-105">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="576d7-105">Host configuration values</span></span>

<span data-ttu-id="576d7-106">[サーバー側のホスティング モデル](xref:blazor/hosting-models#server-side)を使用するサーバー側アプリでは、[汎用ホスト構成値](xref:fundamentals/host/generic-host#host-configuration)を受け入れることができます。</span><span class="sxs-lookup"><span data-stu-id="576d7-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="576d7-107">配置</span><span class="sxs-lookup"><span data-stu-id="576d7-107">Deployment</span></span>

<span data-ttu-id="576d7-108">[サーバー側のホスティング モデル](xref:blazor/hosting-models#server-side)では、Blazor はサーバー上で ASP.NET Core アプリ内から実行されます。</span><span class="sxs-lookup"><span data-stu-id="576d7-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="576d7-109">UI の更新、イベント処理、JavaScript の呼び出しは、[SignalR](xref:signalr/introduction) 接続経由で処理されます。</span><span class="sxs-lookup"><span data-stu-id="576d7-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="576d7-110">ASP.NET Core アプリをホストできる Web サーバーが必要です。</span><span class="sxs-lookup"><span data-stu-id="576d7-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="576d7-111">Visual Studio には **Blazor (サーバー側)** プロジェクト テンプレートが含まれています ([dotnet new](/dotnet/core/tools/dotnet-new) コマンドを使用する場合は `blazorserverside` テンプレート)。</span><span class="sxs-lookup"><span data-stu-id="576d7-111">Visual Studio includes the **Blazor (server-side)** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

## <a name="additional-resources"></a><span data-ttu-id="576d7-112">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="576d7-112">Additional resources</span></span>

* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="576d7-113">Azure App Service に ASP.NET Core プレビュー リリースを展開する</span><span class="sxs-lookup"><span data-stu-id="576d7-113">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
