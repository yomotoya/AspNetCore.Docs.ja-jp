---
title: Razor コンポーネントのホスティング モデル
author: guardrex
description: クライアント側 Blazor とサーバー側の ASP.NET Core で Razor コンポーネント ホスティング モデルを理解します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: razor-components/hosting-models
ms.openlocfilehash: 8ed70117c94bf1a3e4c208f70310bbf0473bae44
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515650"
---
# <a name="razor-components-hosting-models"></a><span data-ttu-id="0afb9-103">Razor コンポーネントのホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="0afb9-103">Razor Components hosting models</span></span>

<span data-ttu-id="0afb9-104">によって[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="0afb9-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="0afb9-105">Razor のコンポーネントは web framework がクライアント側で実行するように設計上のブラウザーで、 [WebAssembly](http://webassembly.org/)-ベースの .NET ランタイム (*Blazor*) または ASP.NET Core でのサーバー側 (*ASP.NET Core の Razorコンポーネント*)。</span><span class="sxs-lookup"><span data-stu-id="0afb9-105">Razor Components is a web framework designed to run client-side in the browser on a [WebAssembly](http://webassembly.org/)-based .NET runtime (*Blazor*) or server-side in ASP.NET Core (*ASP.NET Core Razor Components*).</span></span> <span data-ttu-id="0afb9-106">ホスティング モデルでは、アプリおよびコンポーネントのモデルに関係なく*は同じまま*します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-106">Regardless of the hosting model, the app and component models *remain the same*.</span></span> <span data-ttu-id="0afb9-107">この記事では、使用可能なホスティング モデルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-107">This article discusses the available hosting models:</span></span>

* [<span data-ttu-id="0afb9-108">クライアント側 Blazor</span><span class="sxs-lookup"><span data-stu-id="0afb9-108">Client-side Blazor</span></span>](#client-side-hosting-model)
* [<span data-ttu-id="0afb9-109">サーバー側 ASP.NET Core Razor Components</span><span class="sxs-lookup"><span data-stu-id="0afb9-109">Server-side ASP.NET Core Razor Components</span></span>](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a><span data-ttu-id="0afb9-110">クライアント側のホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="0afb9-110">Client-side hosting model</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="0afb9-111">Blazor のプリンシパルのホスティング モデルは、WebAssembly のブラウザーで実行されているクライアント側です。</span><span class="sxs-lookup"><span data-stu-id="0afb9-111">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="0afb9-112">Blazor アプリ、その依存関係、.NET ランタイムがブラウザーにダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="0afb9-112">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="0afb9-113">アプリがブラウザー UI スレッド上で直接実行されます。</span><span class="sxs-lookup"><span data-stu-id="0afb9-113">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="0afb9-114">UI の更新とイベントの処理は、同じプロセス内で発生します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-114">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="0afb9-115">アプリの資産は、静的ファイルを web サーバーまたは静的なコンテンツをクライアントに提供できるサービスとしてデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="0afb9-115">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor クライアント側:Blazor アプリは、ブラウザー内での UI スレッドで実行されます。](hosting-models/_static/client-side.png)

<span data-ttu-id="0afb9-117">クライアント側のホスティング モデルを使用して Blazor アプリを作成するには、次のテンプレートのいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-117">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="0afb9-118">**Blazor** ([dotnet の新しい blazor](/dotnet/core/tools/dotnet-new))&ndash;一連の静的ファイルとしてデプロイします。</span><span class="sxs-lookup"><span data-stu-id="0afb9-118">**Blazor** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="0afb9-119">**(ASP.NET Core のホストされる) Blazor** ([dotnet の新しい blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; ASP.NET Core サーバーによってホストされています。</span><span class="sxs-lookup"><span data-stu-id="0afb9-119">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="0afb9-120">ASP.NET Core アプリでは、クライアントに Blazor アプリが機能します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-120">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="0afb9-121">クライアント側の Blazor アプリは、web API の呼び出しを使用してネットワーク経由でサーバーと対話できるまたは[SignalR](xref:signalr/introduction)します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-121">The client-side Blazor app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="0afb9-122">テンプレートが含まれる、 *components.webassembly.js*を処理するスクリプト。</span><span class="sxs-lookup"><span data-stu-id="0afb9-122">The templates include the *components.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="0afb9-123">.NET ランタイムで、アプリとアプリの依存関係をダウンロードしています。</span><span class="sxs-lookup"><span data-stu-id="0afb9-123">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="0afb9-124">アプリを実行するランタイムを初期化します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-124">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="0afb9-125">クライアント側のホスティング モデルでは、いくつかの利点を提供します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-125">The client-side hosting model offers several benefits.</span></span> <span data-ttu-id="0afb9-126">クライアント側 Blazor:</span><span class="sxs-lookup"><span data-stu-id="0afb9-126">Client-side Blazor:</span></span>

* <span data-ttu-id="0afb9-127">.NET サーバー側の依存関係がありません。</span><span class="sxs-lookup"><span data-stu-id="0afb9-127">Has no .NET server-side dependency.</span></span>
* <span data-ttu-id="0afb9-128">クライアント リソースと機能を完全に活用します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-128">Fully leverages client resources and capabilities.</span></span>
* <span data-ttu-id="0afb9-129">オフロードは、サーバーからクライアントに機能します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-129">Offloads work from the server to the client.</span></span>
* <span data-ttu-id="0afb9-130">オフライン シナリオをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="0afb9-130">Supports offline scenarios.</span></span>

<span data-ttu-id="0afb9-131">クライアント側のホストには欠点があります。</span><span class="sxs-lookup"><span data-stu-id="0afb9-131">There are downsides to client-side hosting.</span></span> <span data-ttu-id="0afb9-132">クライアント側 Blazor:</span><span class="sxs-lookup"><span data-stu-id="0afb9-132">Client-side Blazor:</span></span>

* <span data-ttu-id="0afb9-133">アプリをブラウザーの機能に制限されます。</span><span class="sxs-lookup"><span data-stu-id="0afb9-133">Restricts the app to the capabilities of the browser.</span></span>
* <span data-ttu-id="0afb9-134">対応クライアントのハードウェアとソフトウェア (たとえば、WebAssembly サポート) が必要です。</span><span class="sxs-lookup"><span data-stu-id="0afb9-134">Requires capable client hardware and software (for example, WebAssembly support).</span></span>
* <span data-ttu-id="0afb9-135">より大きなダウンロード サイズと読み込み時間長くアプリ。</span><span class="sxs-lookup"><span data-stu-id="0afb9-135">Has a larger download size and longer app load time.</span></span>
* <span data-ttu-id="0afb9-136">.NET ランタイムおよびツールのサポートを成熟させる必要がある (の制限など、 [.NET Standard](/dotnet/standard/net-standard)サポートおよびデバッグ)。</span><span class="sxs-lookup"><span data-stu-id="0afb9-136">Has less mature .NET runtime and tooling support (for example, limitations in [.NET Standard](/dotnet/standard/net-standard) support and debugging).</span></span>

## <a name="server-side-hosting-model"></a><span data-ttu-id="0afb9-137">サーバー側のホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="0afb9-137">Server-side hosting model</span></span>

<span data-ttu-id="0afb9-138">ASP.NET Core の Razor コンポーネント サーバー側のホスティング モデルでは、アプリは、ASP.NET Core アプリ内からサーバーで実行されます。</span><span class="sxs-lookup"><span data-stu-id="0afb9-138">With the ASP.NET Core Razor Components server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="0afb9-139">UI の更新、イベント処理、および JavaScript の呼び出しが経由で処理を[SignalR](xref:signalr/introduction)接続します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-139">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![ASP.NET Core Razor コンポーネント サーバー側:ブラウザーでは、SignalR 接続経由でサーバー (ASP.NET Core アプリの内部でホストされている) アプリと対話します。](hosting-models/_static/server-side.png)

<span data-ttu-id="0afb9-141">サーバー側のホスティング モデルを使用して Razor コンポーネント アプリを作成するには、ASP.NET Core を使用して**Razor コンポーネント**テンプレート ([dotnet の新しい razorcomponents](/dotnet/core/tools/dotnet-new))。</span><span class="sxs-lookup"><span data-stu-id="0afb9-141">To create a Razor Components app using the server-side hosting model, use the ASP.NET Core **Razor Components** template ([dotnet new razorcomponents](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="0afb9-142">ASP.NET Core アプリは、Razor コンポーネント サーバー側のアプリをホストし、クライアントが接続する SignalR エンドポイントを設定します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-142">The ASP.NET Core app hosts the Razor Components server-side app and sets up the SignalR endpoint where clients connect.</span></span> <span data-ttu-id="0afb9-143">ASP.NET Core アプリを参照して、アプリの`Startup`クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-143">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="0afb9-144">サーバー側コンポーネントの Razor サービス。</span><span class="sxs-lookup"><span data-stu-id="0afb9-144">Server-side Razor Components services.</span></span>
* <span data-ttu-id="0afb9-145">要求パイプラインを処理するアプリです。</span><span class="sxs-lookup"><span data-stu-id="0afb9-145">The app to the request handling pipeline.</span></span>

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

<span data-ttu-id="0afb9-146">*Components.server.js*スクリプト&dagger;クライアント接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-146">The *components.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="0afb9-147">永続化および (たとえば、失われたネットワーク接続の場合は) 発生時に必要なアプリの状態を復元するアプリの役目です。</span><span class="sxs-lookup"><span data-stu-id="0afb9-147">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="0afb9-148">サーバー側のホスティング モデルでは、いくつかの利点を提供します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-148">The server-side hosting model offers several benefits.</span></span> <span data-ttu-id="0afb9-149">Razor のサーバー側コンポーネント:</span><span class="sxs-lookup"><span data-stu-id="0afb9-149">Server-side Razor Components:</span></span>

* <span data-ttu-id="0afb9-150">クライアント側の Blazor アプリよりも大幅に小さくアプリのサイズを持ち、はるかに高速に読み込みます。</span><span class="sxs-lookup"><span data-stu-id="0afb9-150">Have a significantly smaller app size than a client-side Blazor app and load much faster.</span></span>
* <span data-ttu-id="0afb9-151">含む任意の .NET Core の互換性のある Api を使用して、サーバー機能の完全な利用できます。</span><span class="sxs-lookup"><span data-stu-id="0afb9-151">Can take full advantage of server capabilities, including using any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="0afb9-152">既存の .NET tooling、デバッグなどが期待どおりに動作するため、サーバー上で .NET Core で実行します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-152">Run on .NET Core on the server, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="0afb9-153">シン クライアントと連動 (たとえば、WebAssembly とリソースをサポートしないブラウザーがデバイスを制限する)。</span><span class="sxs-lookup"><span data-stu-id="0afb9-153">Works with thin clients (for example, browsers that don't support WebAssembly and resource constrained devices).</span></span>
* <span data-ttu-id="0afb9-154">.NET/C#アプリのコンポーネントのコードを含む、コード ベースは、クライアントに提供されません。</span><span class="sxs-lookup"><span data-stu-id="0afb9-154">.NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="0afb9-155">サーバー側のホストには欠点があります。</span><span class="sxs-lookup"><span data-stu-id="0afb9-155">There are downsides to server-side hosting.</span></span> <span data-ttu-id="0afb9-156">Razor のサーバー側コンポーネント:</span><span class="sxs-lookup"><span data-stu-id="0afb9-156">Server-side Razor Components:</span></span>

* <span data-ttu-id="0afb9-157">待機時間があります。すべてのユーザー操作には、ネットワーク ホップが必要があります。</span><span class="sxs-lookup"><span data-stu-id="0afb9-157">Have higher latency: Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="0afb9-158">オフライン サポートを提供しません。クライアント接続に失敗した場合、アプリは稼働を停止します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-158">Offer no offline support: If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="0afb9-159">スケーラビリティが低下します。サーバーは、複数のクライアント接続を管理し、クライアントの状態を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0afb9-159">Have reduced scalability: The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="0afb9-160">アプリを処理するために、ASP.NET Core サーバーが必要です。</span><span class="sxs-lookup"><span data-stu-id="0afb9-160">Require an ASP.NET Core server to serve the app.</span></span> <span data-ttu-id="0afb9-161">(たとえば、CDN) からサーバーなしの展開は、ことはできません。</span><span class="sxs-lookup"><span data-stu-id="0afb9-161">Deployment without a server (for example, from a CDN) isn't possible.</span></span>

<span data-ttu-id="0afb9-162">&dagger;*Components.server.js*スクリプトは、次のパスに発行: *bin/{デバッグ |リリース}/{ターゲット フレームワーク}/publish/{アプリケーション名}。アプリ/dist/フレームワーク (_f)* します。</span><span class="sxs-lookup"><span data-stu-id="0afb9-162">&dagger;The *components.server.js* script is published to the following path: *bin/{Debug|Release}/{TARGET FRAMEWORK}/publish/{APPLICATION NAME}.App/dist/_framework*.</span></span>
