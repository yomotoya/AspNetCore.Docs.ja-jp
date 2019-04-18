---
title: ASP.NET Core での Blazor の概要
author: guardrex
description: ASP.NET Core アプリ内に .NET を使った対話型のクライアント側 Web UI を構築する方法である、ASP.NET Core Blazor について調べます。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/15/2019
uid: blazor/index
ms.openlocfilehash: a5b12a5c5c10a74ab192d0d67a2ba9a5617232c7
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614598"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="5f04a-103">Blazor の概要</span><span class="sxs-lookup"><span data-stu-id="5f04a-103">Introduction to Blazor</span></span>

<span data-ttu-id="5f04a-104">作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5f04a-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="razor-components"></a><span data-ttu-id="5f04a-105">Razor Components</span><span class="sxs-lookup"><span data-stu-id="5f04a-105">Razor Components</span></span>

<span data-ttu-id="5f04a-106">Blazor のサーバー側ホスティング モデルである *Razor Components* は、.NET を使った対話型のクライアント側 Web UI を構築するための方法です。</span><span class="sxs-lookup"><span data-stu-id="5f04a-106">The server-side hosting model of Blazor, *Razor Components*, is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="5f04a-107">JavaScript の代わりに C# を使って、多機能な対話型 UI を構築します。</span><span class="sxs-lookup"><span data-stu-id="5f04a-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="5f04a-108">すべて .NET で記述された、サーバー側とクライアント側アプリのロジックを共有します。</span><span class="sxs-lookup"><span data-stu-id="5f04a-108">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="5f04a-109">モバイル ブラウザーを含めた広範なブラウザーのサポートのために、HTML および CSS として UI をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="5f04a-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="5f04a-110">Razor Components は、ほとんどのアプリで必要となる次のコア機能をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="5f04a-110">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="5f04a-111">パラメーター</span><span class="sxs-lookup"><span data-stu-id="5f04a-111">Parameters</span></span>
* <span data-ttu-id="5f04a-112">イベント処理</span><span class="sxs-lookup"><span data-stu-id="5f04a-112">Event handling</span></span>
* <span data-ttu-id="5f04a-113">データ バインディング</span><span class="sxs-lookup"><span data-stu-id="5f04a-113">Data binding</span></span>
* <span data-ttu-id="5f04a-114">ルーティング</span><span class="sxs-lookup"><span data-stu-id="5f04a-114">Routing</span></span>
* <span data-ttu-id="5f04a-115">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="5f04a-115">Dependency injection</span></span>
* <span data-ttu-id="5f04a-116">レイアウト</span><span class="sxs-lookup"><span data-stu-id="5f04a-116">Layouts</span></span>
* <span data-ttu-id="5f04a-117">テンプレート</span><span class="sxs-lookup"><span data-stu-id="5f04a-117">Templating</span></span>
* <span data-ttu-id="5f04a-118">カスケード値</span><span class="sxs-lookup"><span data-stu-id="5f04a-118">Cascading values</span></span>

<span data-ttu-id="5f04a-119">Razor Components では、UI の更新を適用する方法からコンポーネントのレンダリング ロジックが分離されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-119">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="5f04a-120">.NET Core 3.0 の ASP.NET Core Razor Component には、ASP.NET Core アプリでサーバー上の Razor Components をホストするためのサポートが追加されています。</span><span class="sxs-lookup"><span data-stu-id="5f04a-120">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="5f04a-121">UI の更新は SignalR 接続を介して処理されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-121">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="5f04a-122">ランタイムは:</span><span class="sxs-lookup"><span data-stu-id="5f04a-122">The runtime:</span></span>

* <span data-ttu-id="5f04a-123">ブラウザーからサーバーへの UI イベントの送信を処理します。</span><span class="sxs-lookup"><span data-stu-id="5f04a-123">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="5f04a-124">サーバーから送信された UI の更新を、コンポーネントの実行後にブラウザーに適用します。</span><span class="sxs-lookup"><span data-stu-id="5f04a-124">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="5f04a-125">ブラウザーと通信するために Razor Components で使われる接続は、JavaScript 相互運用の呼び出しを処理するためにも使われます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-125">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Razor Components では、サーバー上で .NET コードが実行され、SignalR 接続を介してクライアント上のドキュメント オブジェクト モデルとのやりとりが行われます。](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="5f04a-127">詳細については、「<xref:blazor/hosting-models#server-side-hosting-model>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5f04a-127">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="5f04a-128">コンポーネント</span><span class="sxs-lookup"><span data-stu-id="5f04a-128">Components</span></span>

<span data-ttu-id="5f04a-129">*Razor Component* は、ページ、ダイアログ、データ入力フォームなど、UI の一部です。</span><span class="sxs-lookup"><span data-stu-id="5f04a-129">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="5f04a-130">コンポーネントではユーザー イベントが処理され、柔軟な UI のレンダリング ロジックが定義されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-130">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="5f04a-131">コンポーネントは、入れ子にしたり再利用したりできます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-131">Components can be nested and reused.</span></span>

<span data-ttu-id="5f04a-132">コンポーネントは .NET アセンブリに組み込まれた .NET クラスであり、NuGet パッケージとして共有したり配布したりできます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-132">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="5f04a-133">クラスは通常、*.razor* ファイル名拡張子を使った Razor マークアップ ページ (Razor Components)、または *.cshtml* ファイル名拡張子を使った Razor マークアップ ページ (Blazor) の形式で記述されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-133">The class is normally written in the form of a Razor markup page with a *.razor* file extension (Razor Components) or Razor markup page with a *.cshtml* file extension (Blazor).</span></span>

<span data-ttu-id="5f04a-134">[Razor](xref:mvc/views/razor) とは、HTML マークアップを C# コードに結合するための構文です。</span><span class="sxs-lookup"><span data-stu-id="5f04a-134">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="5f04a-135">Razor は開発者の生産性向上を目的に設計されています。これにより開発者は [IntelliSense](/visualstudio/ide/using-intellisense) サポートを使用して同じファイル内でマークアップと C# を切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-135">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="5f04a-136">Razor Pages と MVC ビューでも Razor が使われます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-136">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="5f04a-137">要求/応答モデルを中心に構築される Razor Pages や MVC ビューとは異なり、コンポーネントは特に UI コンポジションを処理するために使われます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-137">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="5f04a-138">Razor Components は特にクライアント側の UI ロジックとコンポジションのために使えます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-138">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="5f04a-139">次のマークアップは、カスタム ダイアログ コンポーネントの例です。</span><span class="sxs-lookup"><span data-stu-id="5f04a-139">The following markup is an example of a custom dialog component:</span></span>

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

<span data-ttu-id="5f04a-140">このコンポーネントをアプリ内の他の場所で使用する場合、IntelliSense の構文およびパラメーター補完により開発を迅速化できます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-140">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="5f04a-141">コンポーネントは、"*レンダリング ツリー*" と呼ばれるブラウザー DOM のメモリ内表現としてレンダリングされ、柔軟かつ効率的な方法で UI を更新するために使えるようになります。</span><span class="sxs-lookup"><span data-stu-id="5f04a-141">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="5f04a-142">JavaScript 相互運用</span><span class="sxs-lookup"><span data-stu-id="5f04a-142">JavaScript interop</span></span>

<span data-ttu-id="5f04a-143">サード パーティ製の JavaScript ライブラリおよびブラウザーの API を必要とするアプリのために、コンポーネントは JavaScript と相互運用します。</span><span class="sxs-lookup"><span data-stu-id="5f04a-143">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="5f04a-144">コンポーネントでは、JavaScript で使用できるライブラリまたは API はいずれも使用することができます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-144">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="5f04a-145">C# コードによる JavaScript コードの呼び出し、および JavaScript コードによる C# コードの呼び出しを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-145">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="5f04a-146">詳細については、[JavaScript 相互運用](xref:blazor/javascript-interop)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="5f04a-146">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="5f04a-147">コードの共有と .NET Standard</span><span class="sxs-lookup"><span data-stu-id="5f04a-147">Code sharing and .NET Standard</span></span>

<span data-ttu-id="5f04a-148">アプリでは、既存の [.NET Standard](/dotnet/standard/net-standard) ライブラリを参照および使用することができます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-148">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="5f04a-149">.NET Standard は、.NET 実装全体で共通した .NET API の標準仕様です。</span><span class="sxs-lookup"><span data-stu-id="5f04a-149">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="5f04a-150">Blazor では .NET Standard 2.0 が実装されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-150">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="5f04a-151">Web ブラウザー内で適用できない API (たとえば、ファイル システムにアクセスする機能、ソケットを開く機能、スレッド機能など) からは、<xref:System.PlatformNotSupportedException> がスローされます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-151">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="5f04a-152">.NET Standard のクラス ライブラリは、Blazor、.NET Framework、.NET Core、Xamarin、Mono、Unity など、さまざまな .NET プラットフォーム全体で共有することができます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-152">.NET Standard class libraries can be shared across different .NET platforms, like Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="blazor"></a><span data-ttu-id="5f04a-153">Blazor</span><span class="sxs-lookup"><span data-stu-id="5f04a-153">Blazor</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="5f04a-154">Blazor は、.NET を使って対話型のクライアント側 Web アプリを構築するための、シングルページ アプリ フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="5f04a-154">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="5f04a-155">Blazor ではオープンな Web 標準が使われ、プラグインもコード トランスパイルも必要ありません。</span><span class="sxs-lookup"><span data-stu-id="5f04a-155">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="5f04a-156">Blazor はモバイル ブラウザーも含めて、すべての最新の Web ブラウザーで動作します。</span><span class="sxs-lookup"><span data-stu-id="5f04a-156">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="5f04a-157">クライアント側の Web 開発のためにブラウザー内で .NET を使うことには、多くの利点があります。</span><span class="sxs-lookup"><span data-stu-id="5f04a-157">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="5f04a-158">**C# 言語**:JavaScript ではなく C# でコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="5f04a-158">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="5f04a-159">**.NET エコシステム**:.NET ライブラリの既存のエコシステムを活用します。</span><span class="sxs-lookup"><span data-stu-id="5f04a-159">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="5f04a-160">**フルスタック開発**:サーバーとクライアント側のロジックを共有します。</span><span class="sxs-lookup"><span data-stu-id="5f04a-160">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="5f04a-161">**スピードとスケーラビリティ**: NET はパフォーマンス、信頼性、セキュリティを重視して構築されました。</span><span class="sxs-lookup"><span data-stu-id="5f04a-161">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="5f04a-162">**業界をリードするツール**: Windows、Linux、macOS 上の Visual Studio を使って生産性を維持します。</span><span class="sxs-lookup"><span data-stu-id="5f04a-162">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="5f04a-163">**安定性と一貫性**:多機能で使いやすい安定した言語、フレームワーク、およびツールの共通セットに基づいて構築します。</span><span class="sxs-lookup"><span data-stu-id="5f04a-163">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="5f04a-164">[WebAssembly](http://webassembly.org) (略称 *wasm*) によって、Web ブラウザー内で .NET コードを実行することが可能になります。</span><span class="sxs-lookup"><span data-stu-id="5f04a-164">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="5f04a-165">WebAssembly はオープンな Web 標準であり、プラグインを使わずに Web ブラウザー内でサポートされます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-165">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="5f04a-166">WebAssembly は、ダウンロードを高速化し実行速度を最大限に高めるために最適化されたコンパクトなバイトコード形式です。</span><span class="sxs-lookup"><span data-stu-id="5f04a-166">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="5f04a-167">WebAssembly コードを使用すると、JavaScript 相互運用を介してブラウザーのすべての機能にアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-167">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="5f04a-168">それと同時に、WebAssembly を介して実行される .NET コードは、クライアント コンピューター上での悪意のあるアクションを禁止するために JavaScript と同じ信頼されたサンドボックス内で実行されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-168">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor では、WebAssembly を使用してブラウザー内で .NET コードが実行されます。](index/_static/blazor.png)

<span data-ttu-id="5f04a-170">Blazor アプリをビルドしてブラウザーで実行する場合: </span><span class="sxs-lookup"><span data-stu-id="5f04a-170">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="5f04a-171">C# コード ファイルと Razor ファイルが .NET アセンブリにコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-171">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="5f04a-172">そのアセンブリと .NET ランタイムがブラウザーにダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-172">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="5f04a-173">Blazor により .NET ランタイムがブートストラップされ、アプリのアセンブリを読み込むようにそのランタイムが構成されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-173">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="5f04a-174">ドキュメント オブジェクト モデル (DOM) 操作とブラウザー API の呼び出しは、JavaScript 相互運用を介して Blazor ランタイムによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-174">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="5f04a-175">Blazor は、ほとんどのアプリで必要となる次のコア機能をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="5f04a-175">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="5f04a-176">パラメーター</span><span class="sxs-lookup"><span data-stu-id="5f04a-176">Parameters</span></span>
* <span data-ttu-id="5f04a-177">イベント処理</span><span class="sxs-lookup"><span data-stu-id="5f04a-177">Event handling</span></span>
* <span data-ttu-id="5f04a-178">データ バインディング</span><span class="sxs-lookup"><span data-stu-id="5f04a-178">Data binding</span></span>
* <span data-ttu-id="5f04a-179">ルーティング</span><span class="sxs-lookup"><span data-stu-id="5f04a-179">Routing</span></span>
* <span data-ttu-id="5f04a-180">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="5f04a-180">Dependency injection</span></span>
* <span data-ttu-id="5f04a-181">レイアウト</span><span class="sxs-lookup"><span data-stu-id="5f04a-181">Layouts</span></span>
* <span data-ttu-id="5f04a-182">テンプレート</span><span class="sxs-lookup"><span data-stu-id="5f04a-182">Templating</span></span>
* <span data-ttu-id="5f04a-183">カスケード値</span><span class="sxs-lookup"><span data-stu-id="5f04a-183">Cascading values</span></span>

<span data-ttu-id="5f04a-184">ダウンロードされるアプリのサイズを小さくするため、アプリが[中間言語 (IL) リンカー](xref:host-and-deploy/blazor/configure-linker)によって発行されるときに、アプリから未使用コードが除去されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-184">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="5f04a-185">Blazor のクライアント側は、クライアント側のホスティング モデルです。</span><span class="sxs-lookup"><span data-stu-id="5f04a-185">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="5f04a-186">Blazor ではコンポーネントのレンダリング ロジックが UI の更新の適用方法から切り離されているため、Blazor をホストする方法には柔軟性があります。</span><span class="sxs-lookup"><span data-stu-id="5f04a-186">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="5f04a-187">ASP.NET Core [Razor Component](#razor-components) を使って、サーバー上の Razor Components を ASP.NET Core アプリでホストします。UI の更新は SignalR 接続を介して処理されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-187">Use ASP.NET Core [Razor Components](#razor-components) to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="5f04a-188">詳細については、「<xref:blazor/hosting-models#server-side-hosting-model>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5f04a-188">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span> 

<span data-ttu-id="5f04a-189">ペイロードのサイズは、アプリの使用性に関する重要なパフォーマンス要因です。</span><span class="sxs-lookup"><span data-stu-id="5f04a-189">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="5f04a-190">Blazor では、ダウンロード時間を短縮するためにペイロードのサイズが最適化されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-190">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="5f04a-191">.NET アセンブリの未使用部分がビルド処理中に削除されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-191">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="5f04a-192">HTTP 応答が圧縮されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-192">HTTP responses are compressed.</span></span>
* <span data-ttu-id="5f04a-193">.NET ランタイムとアセンブリがブラウザーにキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-193">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="5f04a-194">[Razor Components](#razor-components) では、.NET アセンブリ、アプリのアセンブリ、およびランタイムをサーバー側で維持することにより、Blazor よりもペイロードのサイズが縮小されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-194">[Razor Components](#razor-components) provides a smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="5f04a-195">Razor Components アプリでは、マークアップ ファイルと静的資産のみがクライアントに提供されます。</span><span class="sxs-lookup"><span data-stu-id="5f04a-195">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5f04a-196">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="5f04a-196">Additional resources</span></span>

* [<span data-ttu-id="5f04a-197">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="5f04a-197">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="5f04a-198">C# のガイド</span><span class="sxs-lookup"><span data-stu-id="5f04a-198">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="5f04a-199">HTML</span><span class="sxs-lookup"><span data-stu-id="5f04a-199">HTML</span></span>](https://www.w3.org/html/)
