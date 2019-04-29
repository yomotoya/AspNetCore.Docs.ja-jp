---
title: ASP.NET Core での Blazor の概要
author: guardrex
description: ASP.NET Core アプリ内に .NET を使った対話型のクライアント側 Web UI を構築する方法である、ASP.NET Core Blazor について調べます。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982998"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="3de76-103">Blazor の概要</span><span class="sxs-lookup"><span data-stu-id="3de76-103">Introduction to Blazor</span></span>

<span data-ttu-id="3de76-104">作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3de76-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3de76-105">Blazor にようこそ!</span><span class="sxs-lookup"><span data-stu-id="3de76-105">Welcome to Blazor!</span></span>

<span data-ttu-id="3de76-106">.NET を使った対話型のクライアント側 Web UI を構築します。</span><span class="sxs-lookup"><span data-stu-id="3de76-106">Build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="3de76-107">JavaScript の代わりに C# を使って、多機能な対話型 UI を構築します。</span><span class="sxs-lookup"><span data-stu-id="3de76-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="3de76-108">.NET で記述された、サーバー側とクライアント側のアプリのロジックを共有します。</span><span class="sxs-lookup"><span data-stu-id="3de76-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="3de76-109">モバイル ブラウザーを含めた広範なブラウザーのサポートのために、HTML および CSS として UI をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="3de76-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="3de76-110">Blazor は、ほとんどのアプリで必要となる次のコア シナリオをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="3de76-110">Blazor supports core scenarios required by most apps:</span></span>

* <span data-ttu-id="3de76-111">パラメーター</span><span class="sxs-lookup"><span data-stu-id="3de76-111">Parameters</span></span>
* <span data-ttu-id="3de76-112">イベント処理</span><span class="sxs-lookup"><span data-stu-id="3de76-112">Event handling</span></span>
* <span data-ttu-id="3de76-113">データ バインディング</span><span class="sxs-lookup"><span data-stu-id="3de76-113">Data binding</span></span>
* <span data-ttu-id="3de76-114">ルーティング</span><span class="sxs-lookup"><span data-stu-id="3de76-114">Routing</span></span>
* <span data-ttu-id="3de76-115">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="3de76-115">Dependency injection</span></span>
* <span data-ttu-id="3de76-116">レイアウト</span><span class="sxs-lookup"><span data-stu-id="3de76-116">Layouts</span></span>
* <span data-ttu-id="3de76-117">テンプレート</span><span class="sxs-lookup"><span data-stu-id="3de76-117">Templates</span></span>
* <span data-ttu-id="3de76-118">カスケード値</span><span class="sxs-lookup"><span data-stu-id="3de76-118">Cascading values</span></span>

## <a name="components"></a><span data-ttu-id="3de76-119">コンポーネント</span><span class="sxs-lookup"><span data-stu-id="3de76-119">Components</span></span>

<span data-ttu-id="3de76-120">Blazor の*コンポーネント*とはページ、ダイアログ、データ エントリ フォームなど UI の要素です。</span><span class="sxs-lookup"><span data-stu-id="3de76-120">A *component* in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="3de76-121">コンポーネントではユーザー イベントが処理され、柔軟な UI のレンダリング ロジックが定義されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-121">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="3de76-122">コンポーネントは、入れ子にしたり再利用したりできます。</span><span class="sxs-lookup"><span data-stu-id="3de76-122">Components can be nested and reused.</span></span>

<span data-ttu-id="3de76-123">コンポーネントは .NET アセンブリに組み込まれた .NET クラスであり、NuGet パッケージとして共有したり配布したりできます。</span><span class="sxs-lookup"><span data-stu-id="3de76-123">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="3de76-124">コンポーネント クラスは通常、Razor マークアップ ページの形式で、*.razor* ファイル拡張子で記述されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-124">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span> <span data-ttu-id="3de76-125">Blazor のコンポーネントは、Razor コンポーネントと呼ばれることもあります。</span><span class="sxs-lookup"><span data-stu-id="3de76-125">Components in Blazor are sometimes referred to as Razor components.</span></span>

<span data-ttu-id="3de76-126">[Razor](xref:mvc/views/razor) とは、HTML マークアップを C# コードに結合するための構文です。</span><span class="sxs-lookup"><span data-stu-id="3de76-126">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="3de76-127">Razor は開発者の生産性向上を目的に設計されています。これにより開発者は [IntelliSense](/visualstudio/ide/using-intellisense) サポートを使用して同じファイル内でマークアップと C# を切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="3de76-127">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="3de76-128">Razor Pages と MVC ビューでも Razor が使われます。</span><span class="sxs-lookup"><span data-stu-id="3de76-128">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="3de76-129">要求/応答モデルを中心に構築される Razor Pages や MVC ビューとは異なり、コンポーネントは特に UI コンポジションを処理するために使われます。</span><span class="sxs-lookup"><span data-stu-id="3de76-129">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="3de76-130">Razor コンポーネントは特にクライアント側の UI ロジックとコンポジションのために使えます。</span><span class="sxs-lookup"><span data-stu-id="3de76-130">Razor components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="3de76-131">次のマークアップは、カスタム ダイアログ コンポーネントの例です。</span><span class="sxs-lookup"><span data-stu-id="3de76-131">The following markup is an example of a custom dialog component:</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="3de76-132">このコンポーネントをアプリ内の他の場所で使用すると、[Visual Studio](https://visualstudio.microsoft.com/vs/) の IntelliSense の構文およびパラメーター補完により開発を迅速化できます。</span><span class="sxs-lookup"><span data-stu-id="3de76-132">When this component is used elsewhere in the app, IntelliSense in [Visual Studio](https://visualstudio.microsoft.com/vs/) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="3de76-133">コンポーネントは、"*レンダリング ツリー*" と呼ばれるブラウザー DOM のメモリ内表現としてレンダリングされ、柔軟かつ効率的な方法で UI を更新するために使えるようになります。</span><span class="sxs-lookup"><span data-stu-id="3de76-133">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="3de76-134">サーバー側 Blazor</span><span class="sxs-lookup"><span data-stu-id="3de76-134">Blazor server-side</span></span>

<span data-ttu-id="3de76-135">Blazor では、UI の更新プログラムを適用する方法からコンポーネントのレンダリング ロジックが分離されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-135">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="3de76-136">サーバー側 Blazor では、ASP.NET Core アプリでサーバー上の Razor コンポーネントをホストするためのサポートが提供されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-136">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="3de76-137">UI の更新は SignalR 接続を介して処理されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-137">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="3de76-138">ランタイムは:</span><span class="sxs-lookup"><span data-stu-id="3de76-138">The runtime:</span></span>

* <span data-ttu-id="3de76-139">ブラウザーからサーバーへの UI イベントの送信を処理します。</span><span class="sxs-lookup"><span data-stu-id="3de76-139">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="3de76-140">サーバーから送信された UI の更新を、コンポーネントの実行後にブラウザーに適用します。</span><span class="sxs-lookup"><span data-stu-id="3de76-140">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="3de76-141">ブラウザーと通信するために Blazor のサーバー側で使われる接続は、JavaScript 相互運用の呼び出しを処理するためにも使われます。</span><span class="sxs-lookup"><span data-stu-id="3de76-141">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![サーバー側 Blazor では、サーバー上で .NET コードが実行され、SignalR 接続を介してクライアント上のドキュメント オブジェクト モデルとのやりとりが行われます](index/_static/blazor-server-side.png)

<span data-ttu-id="3de76-143">詳細については、「<xref:blazor/hosting-models#server-side>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3de76-143">For more information, see <xref:blazor/hosting-models#server-side>.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="3de76-144">クライアント側 Blazor</span><span class="sxs-lookup"><span data-stu-id="3de76-144">Blazor client-side</span></span>

<span data-ttu-id="3de76-145">クライアント側 Blazor は、.NET を使って対話型のクライアント側 Web アプリを構築するための、シングルページ アプリ フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="3de76-145">Blazor client-side is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="3de76-146">クライアント側 Blazor ではオープンな Web 標準が使われ、プラグインもコード トランスパイルも必要ありません。</span><span class="sxs-lookup"><span data-stu-id="3de76-146">Blazor client-side uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="3de76-147">クライアント側 Blazor はモバイル ブラウザーも含めて、すべての最新の Web ブラウザーで動作します。</span><span class="sxs-lookup"><span data-stu-id="3de76-147">Blazor client-side works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="3de76-148">クライアント側の Web 開発のためにブラウザー内で .NET を使うことには、多くの利点があります。</span><span class="sxs-lookup"><span data-stu-id="3de76-148">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="3de76-149">**C# 言語**:JavaScript ではなく C# でコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="3de76-149">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="3de76-150">**.NET エコシステム**:.NET ライブラリの既存のエコシステムを活用します。</span><span class="sxs-lookup"><span data-stu-id="3de76-150">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="3de76-151">**フルスタック開発**:サーバーとクライアント側のロジックを共有します。</span><span class="sxs-lookup"><span data-stu-id="3de76-151">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="3de76-152">**スピードとスケーラビリティ**: NET はパフォーマンス、信頼性、セキュリティを重視して構築されました。</span><span class="sxs-lookup"><span data-stu-id="3de76-152">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="3de76-153">**業界をリードするツール**: Windows、Linux、macOS 上の Visual Studio を使って生産性を維持します。</span><span class="sxs-lookup"><span data-stu-id="3de76-153">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="3de76-154">**安定性と一貫性**:多機能で使いやすい安定した言語、フレームワーク、およびツールの共通セットに基づいて構築します。</span><span class="sxs-lookup"><span data-stu-id="3de76-154">**Stability and consistency**:  Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="3de76-155">[WebAssembly](http://webassembly.org) (略称 *wasm*) によって、Web ブラウザー内で .NET コードを実行することが可能になります。</span><span class="sxs-lookup"><span data-stu-id="3de76-155">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="3de76-156">WebAssembly はオープンな Web 標準であり、プラグインを使わずに Web ブラウザー内でサポートされます。</span><span class="sxs-lookup"><span data-stu-id="3de76-156">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="3de76-157">WebAssembly は、ダウンロードを高速化し実行速度を最大限に高めるために最適化されたコンパクトなバイトコード形式です。</span><span class="sxs-lookup"><span data-stu-id="3de76-157">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="3de76-158">WebAssembly コードを使用すると、JavaScript 相互運用を介してブラウザーのすべての機能にアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="3de76-158">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="3de76-159">それと同時に、WebAssembly を介して実行される .NET コードは、クライアント コンピューター上での悪意のあるアクションを禁止するために JavaScript と同じ信頼されたサンドボックス内で実行されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-159">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![クライアント側 Blazor は WebAssembly を使用してブラウザーで .NET コードを実行します。](index/_static/blazor-client-side.png)

<span data-ttu-id="3de76-161">クライアント側 Blazor アプリをビルドしてブラウザーで実行する場合: </span><span class="sxs-lookup"><span data-stu-id="3de76-161">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="3de76-162">C# コード ファイルと Razor ファイルが .NET アセンブリにコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="3de76-162">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="3de76-163">そのアセンブリと .NET ランタイムがブラウザーにダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="3de76-163">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="3de76-164">クライアント側 Blazor により .NET ランタイムがブートストラップされ、アプリのアセンブリを読み込むようにそのランタイムが構成されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-164">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="3de76-165">ドキュメント オブジェクト モデル (DOM) 操作とブラウザー API の呼び出しは、JavaScript 相互運用を介してクライアント側 Blazor ランタイムによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-165">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor client-side runtime via JavaScript interop.</span></span>

<span data-ttu-id="3de76-166">ダウンロードされるアプリのサイズを小さくするため、アプリが[中間言語 (IL) リンカー](xref:host-and-deploy/blazor/configure-linker)によって発行されるときに、アプリから未使用コードが除去されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-166">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="3de76-167">Blazor のクライアント側は、クライアント側のホスティング モデルです。</span><span class="sxs-lookup"><span data-stu-id="3de76-167">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="3de76-168">Blazor ではコンポーネントのレンダリング ロジックが UI の更新の適用方法から切り離されているため、Blazor をホストする方法には柔軟性があります。</span><span class="sxs-lookup"><span data-stu-id="3de76-168">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="3de76-169">[クライアント側 Blazor](#blazor-server-side) を使って、サーバー上の Blazor を ASP.NET Core アプリでホストします。UI の更新は SignalR 接続を介して処理されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-169">Use [Blazor server-side](#blazor-server-side) to host Blazor on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="3de76-170">詳細については、「<xref:blazor/hosting-models#server-side>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3de76-170">For more information, see <xref:blazor/hosting-models#server-side>.</span></span> 

<span data-ttu-id="3de76-171">ペイロードのサイズは、アプリの使用性に関する重要なパフォーマンス要因です。</span><span class="sxs-lookup"><span data-stu-id="3de76-171">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="3de76-172">クライアント側 Blazor では、ダウンロード時間を短縮するためにペイロードのサイズが最適化されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-172">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="3de76-173">.NET アセンブリの未使用部分がビルド処理中に削除されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-173">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="3de76-174">HTTP 応答が圧縮されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-174">HTTP responses are compressed.</span></span>
* <span data-ttu-id="3de76-175">.NET ランタイムとアセンブリがブラウザーにキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="3de76-175">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="3de76-176">[サーバー側 Blazor](#blazor-server-side) では、.NET アセンブリ、アプリのアセンブリ、およびランタイムをサーバー側で維持することにより、クライアント側 Blazor よりもペイロードのサイズが縮小されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-176">[Blazor server-side](#blazor-server-side) provides a smaller payload size than Blazor client-side by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="3de76-177">サーバー側 Blazor アプリでは、マークアップ ファイルと静的資産のみがクライアントに提供されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-177">Blazor server-side apps only serve markup files and static assets to clients.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="3de76-178">JavaScript 相互運用</span><span class="sxs-lookup"><span data-stu-id="3de76-178">JavaScript interop</span></span>

<span data-ttu-id="3de76-179">サード パーティ製の JavaScript ライブラリおよびブラウザーの API を必要とするアプリのために、コンポーネントは JavaScript と相互運用します。</span><span class="sxs-lookup"><span data-stu-id="3de76-179">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="3de76-180">コンポーネントでは、JavaScript で使用できるライブラリまたは API はいずれも使用することができます。</span><span class="sxs-lookup"><span data-stu-id="3de76-180">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="3de76-181">C# コードによる JavaScript コードの呼び出し、および JavaScript コードによる C# コードの呼び出しを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="3de76-181">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="3de76-182">詳細については、[JavaScript 相互運用](xref:blazor/javascript-interop)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3de76-182">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="3de76-183">コードの共有と .NET Standard</span><span class="sxs-lookup"><span data-stu-id="3de76-183">Code sharing and .NET Standard</span></span>

<span data-ttu-id="3de76-184">アプリでは、既存の [.NET Standard](/dotnet/standard/net-standard) ライブラリを参照および使用することができます。</span><span class="sxs-lookup"><span data-stu-id="3de76-184">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="3de76-185">.NET Standard は、.NET 実装全体で共通した .NET API の標準仕様です。</span><span class="sxs-lookup"><span data-stu-id="3de76-185">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="3de76-186">Blazor では .NET Standard 2.0 が実装されます。</span><span class="sxs-lookup"><span data-stu-id="3de76-186">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="3de76-187">Web ブラウザー内で適用できない API (たとえば、ファイル システムにアクセスする機能、ソケットを開く機能、スレッド機能など) からは、<xref:System.PlatformNotSupportedException> がスローされます。</span><span class="sxs-lookup"><span data-stu-id="3de76-187">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="3de76-188">.NET Standard のクラス ライブラリは、Blazor、.NET Framework、.NET Core、Xamarin、Mono、Unity など、さまざまな .NET プラットフォーム全体で共有することができます。</span><span class="sxs-lookup"><span data-stu-id="3de76-188">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3de76-189">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="3de76-189">Additional resources</span></span>

* [<span data-ttu-id="3de76-190">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="3de76-190">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="3de76-191">C# のガイド</span><span class="sxs-lookup"><span data-stu-id="3de76-191">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="3de76-192">HTML</span><span class="sxs-lookup"><span data-stu-id="3de76-192">HTML</span></span>](https://www.w3.org/html/)
