---
title: サーバー側 Blazor をホストおよび展開する
author: guardrex
description: ASP.NET Core を使用して Blazor サーバー側アプリをホストおよびデプロイする方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 940020ee44d72d50395aad64bc924413c1bbecfb
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614714"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="f1af7-103">サーバー側 Blazor をホストおよび展開する</span><span class="sxs-lookup"><span data-stu-id="f1af7-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="f1af7-104">作成者: [Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com)、[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f1af7-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="f1af7-105">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="f1af7-105">Host configuration values</span></span>

<span data-ttu-id="f1af7-106">[サーバー側のホスティング モデル](xref:blazor/hosting-models#server-side-hosting-model)を使用するサーバー側アプリでは、[汎用ホスト構成値](xref:fundamentals/host/generic-host#host-configuration)を受け入れることができます。</span><span class="sxs-lookup"><span data-stu-id="f1af7-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="f1af7-107">配置</span><span class="sxs-lookup"><span data-stu-id="f1af7-107">Deployment</span></span>

<span data-ttu-id="f1af7-108">[サーバー側のホスティング モデル](xref:blazor/hosting-models#server-side-hosting-model)では、Blazor はサーバー上で ASP.NET Core アプリ内から実行されます。</span><span class="sxs-lookup"><span data-stu-id="f1af7-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="f1af7-109">UI の更新、イベント処理、JavaScript の呼び出しは、[SignalR](xref:signalr/introduction) 接続経由で処理されます。</span><span class="sxs-lookup"><span data-stu-id="f1af7-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="f1af7-110">アプリは、発行された出力に ASP.NET Core アプリと共に含まれており、2 つのアプリが一緒に展開されます。</span><span class="sxs-lookup"><span data-stu-id="f1af7-110">The app is included with the ASP.NET Core app in the published output, and the two apps are deployed together.</span></span> <span data-ttu-id="f1af7-111">ASP.NET Core アプリをホストできる Web サーバーが必要です。</span><span class="sxs-lookup"><span data-stu-id="f1af7-111">A web server that's capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="f1af7-112">サーバー側の展開の場合、Visual Studio には **Blazor コンポーネント** プロジェクト テンプレートが含まれています ([dotnet new](/dotnet/core/tools/dotnet-new) コマンドを使用する場合は `razorcomponents` テンプレート)。</span><span class="sxs-lookup"><span data-stu-id="f1af7-112">For a server-side deployment, Visual Studio includes the **Razor Components** project template (`razorcomponents` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="f1af7-113">ASP.NET Core アプリでのホストと展開の詳細については、「<xref:host-and-deploy/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f1af7-113">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="f1af7-114">Azure App Service の展開については、「<xref:tutorials/publish-to-azure-webapp-using-vs>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f1af7-114">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>
