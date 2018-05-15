---
title: ASP.NET Core モジュール
author: tdykstra
description: Kestrel Web サーバーが IIS または IIS Express をリバース プロキシ サーバーとして使用できるようにするための ASP.NET Core モジュールについて説明します。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4e842544f861c3704ba7798fa693b36435d54731
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
---
# <a name="aspnet-core-module"></a><span data-ttu-id="61dc9-103">ASP.NET Core モジュール</span><span class="sxs-lookup"><span data-stu-id="61dc9-103">ASP.NET Core Module</span></span>

<span data-ttu-id="61dc9-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl)、[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="61dc9-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="61dc9-105">ASP.NET Core モジュールでは、リバース プロキシの構成で ASP.NET Core アプリを IIS の背後で実行することを許可します。</span><span class="sxs-lookup"><span data-stu-id="61dc9-105">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="61dc9-106">IIS では、高度な Web アプリ セキュリティ機能と管理機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="61dc9-106">IIS provides advanced web app security and manageability features.</span></span>

<span data-ttu-id="61dc9-107">サポートされている Windows バージョン:</span><span class="sxs-lookup"><span data-stu-id="61dc9-107">Supported Windows versions:</span></span>

* <span data-ttu-id="61dc9-108">Windows 7 以降</span><span class="sxs-lookup"><span data-stu-id="61dc9-108">Windows 7 or later</span></span>
* <span data-ttu-id="61dc9-109">Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="61dc9-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="61dc9-110">ASP.NET Core モジュールは、Kestrel でのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="61dc9-110">The ASP.NET Core Module only works with Kestrel.</span></span> <span data-ttu-id="61dc9-111">このモジュールは、[HTTP.sys](xref:fundamentals/servers/httpsys) (旧称 [WebListener](xref:fundamentals/servers/weblistener)) と互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="61dc9-111">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

## <a name="aspnet-core-module-description"></a><span data-ttu-id="61dc9-112">ASP.NET Core モジュールの説明</span><span class="sxs-lookup"><span data-stu-id="61dc9-112">ASP.NET Core Module description</span></span>

<span data-ttu-id="61dc9-113">ASP.NET Core モジュールは、ネイティブな IIS モジュールであり、バックエンドの ASP.NET Core アプリに Web 要求をリダイレクトするために IIS パイプラインにプラグされます。</span><span class="sxs-lookup"><span data-stu-id="61dc9-113">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to redirect web requests to backend ASP.NET Core apps.</span></span> <span data-ttu-id="61dc9-114">Windows 認証などの多くのネイティブなモジュールは、アクティブなままです。</span><span class="sxs-lookup"><span data-stu-id="61dc9-114">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="61dc9-115">このモジュールと共にアクティブな IIS モジュールの詳細については、[IIS モジュール](xref:host-and-deploy/iis/modules)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="61dc9-115">To learn more about IIS modules active with the module, see [IIS modules](xref:host-and-deploy/iis/modules).</span></span>

<span data-ttu-id="61dc9-116">ASP.NET Core アプリは IIS ワーカー プロセスとは独立したプロセスで実行されるため、モジュールもプロセス管理を処理します。</span><span class="sxs-lookup"><span data-stu-id="61dc9-116">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="61dc9-117">モジュールは、最初の要求を受信したときに ASP.NET Core アプリのプロセスを開始し、プロセスがクラッシュした場合はアプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="61dc9-117">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="61dc9-118">この動作は、IIS のインプロセスで実行され、[WAS (Windows プロセス アクティブ化サービス)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) によって管理される ASP.NET 4.x アプリと基本的に同じです。</span><span class="sxs-lookup"><span data-stu-id="61dc9-118">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="61dc9-119">次の図は、IIS (ASP.NET Core モジュール) と ASP.NET Core アプリの間のリレーションシップを示しています。</span><span class="sxs-lookup"><span data-stu-id="61dc9-119">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and ASP.NET Core apps:</span></span>

![ASP.NET Core モジュール](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="61dc9-121">要求は、Web からカーネル モードの HTTP.sys ドライバーに到着します。</span><span class="sxs-lookup"><span data-stu-id="61dc9-121">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="61dc9-122">ドライバーは、Web サイトの構成ポート (通常は 80 (HTTP) または 443 (HTTPS)) で IIS への要求をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="61dc9-122">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="61dc9-123">モジュールでは、アプリのランダムなポートで Kestrel に要求を転送します。このポートは 80/443 ではありません。</span><span class="sxs-lookup"><span data-stu-id="61dc9-123">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="61dc9-124">モジュールが起動時に環境変数を介してポートを指定すると、サーバーは `http://localhost:{port}` をリッスンするように、IIS 統合ミドルウェアによって構成されます。</span><span class="sxs-lookup"><span data-stu-id="61dc9-124">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="61dc9-125">追加のチェックが実行され、モジュールから発生していない要求は拒否されます。</span><span class="sxs-lookup"><span data-stu-id="61dc9-125">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="61dc9-126">モジュールでは HTTPS 転送がサポートされていないため、要求は HTTPS を介して IIS によって受信された場合でも、HTTP を介して転送されます。</span><span class="sxs-lookup"><span data-stu-id="61dc9-126">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="61dc9-127">Kestrel でモジュールから要求を取り込んだ後、要求は ASP.NET Core ミドルウェア パイプラインにプッシュされます。</span><span class="sxs-lookup"><span data-stu-id="61dc9-127">After Kestrel picks up a request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="61dc9-128">ミドルウェア パイプラインは要求を処理し、`HttpContext` インスタンスとしてアプリのロジックに渡します。</span><span class="sxs-lookup"><span data-stu-id="61dc9-128">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="61dc9-129">アプリの応答が IIS に戻され、IIS はその応答を、要求を開始した HTTP クライアントに返します。</span><span class="sxs-lookup"><span data-stu-id="61dc9-129">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="61dc9-130">ASP.NET Core モジュールには、他にも機能があります。</span><span class="sxs-lookup"><span data-stu-id="61dc9-130">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="61dc9-131">モジュールでは次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="61dc9-131">The module can:</span></span>

* <span data-ttu-id="61dc9-132">ワーカー プロセスの環境変数を設定する</span><span class="sxs-lookup"><span data-stu-id="61dc9-132">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="61dc9-133">起動に関する問題をトラブルシューティングするために、Stdout 出力をファイル ストレージに記録する</span><span class="sxs-lookup"><span data-stu-id="61dc9-133">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="61dc9-134">Windows 認証トークンを転送する</span><span class="sxs-lookup"><span data-stu-id="61dc9-134">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="61dc9-135">ASP.NET Core モジュールをインストールして使用する方法</span><span class="sxs-lookup"><span data-stu-id="61dc9-135">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="61dc9-136">ASP.NET Core モジュールをインストールして使用する方法の詳細な手順については、「[IIS による Windows 上のホスト](xref:host-and-deploy/iis/index)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="61dc9-136">For detailed instructions on how to install and use the ASP.NET Core Module, see [Host on Windows with IIS](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="61dc9-137">モジュールを構成する方法については、「[ASP.NET Core モジュール構成の参照](xref:host-and-deploy/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="61dc9-137">For information on configuring the module, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61dc9-138">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="61dc9-138">Additional resources</span></span>

* [<span data-ttu-id="61dc9-139">IIS による Windows 上のホスト</span><span class="sxs-lookup"><span data-stu-id="61dc9-139">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="61dc9-140">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="61dc9-140">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="61dc9-141">ASP.NET Core モジュールの GitHub リポジトリ (ソース コード)</span><span class="sxs-lookup"><span data-stu-id="61dc9-141">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
