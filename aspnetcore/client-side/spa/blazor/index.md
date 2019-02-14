---
title: Blazor の概要
author: guardrex
description: WebAssembly を使ってブラウザー内で実行される対話型のクライアント側アプリを、 .NET を使って構築するための新しい方法である、ASP.NET Core Blazor について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: spa/blazor/index
ms.openlocfilehash: 0d22365701a4fc1857582c13459280e50d59c858
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159599"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="c5a87-103">Blazor の概要</span><span class="sxs-lookup"><span data-stu-id="c5a87-103">Introduction to Blazor</span></span>

<span data-ttu-id="c5a87-104">作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c5a87-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="c5a87-105">Blazor は、.NET を使って対話型のクライアント側 Web アプリを構築するための、シングルページ アプリ フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="c5a87-105">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="c5a87-106">Blazor ではオープンな Web 標準が使われ、プラグインもコード トランスパイルも必要ありません。</span><span class="sxs-lookup"><span data-stu-id="c5a87-106">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="c5a87-107">Blazor はモバイル ブラウザーも含めて、すべての最新の Web ブラウザーで動作します。</span><span class="sxs-lookup"><span data-stu-id="c5a87-107">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="c5a87-108">クライアント側の Web 開発のためにブラウザー内で .NET を使うことには、多くの利点があります。</span><span class="sxs-lookup"><span data-stu-id="c5a87-108">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="c5a87-109">**C# 言語**:JavaScript ではなく C# でコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="c5a87-109">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="c5a87-110">**.NET エコシステム**:.NET ライブラリの既存のエコシステムを活用します。</span><span class="sxs-lookup"><span data-stu-id="c5a87-110">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="c5a87-111">**フルスタック開発**:サーバーとクライアント側のロジックを共有します。</span><span class="sxs-lookup"><span data-stu-id="c5a87-111">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="c5a87-112">**スピードとスケーラビリティ**:NET はパフォーマンス、信頼性、セキュリティを重視して構築されました。</span><span class="sxs-lookup"><span data-stu-id="c5a87-112">**Speed and scalability**: NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="c5a87-113">**業界をリードするツール**: Windows、Linux、macOS 上の Visual Studio を使って生産性を維持します。</span><span class="sxs-lookup"><span data-stu-id="c5a87-113">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="c5a87-114">**安定性と一貫性**:多機能で使いやすい安定した言語、フレームワーク、およびツールの共通セットに基づいて構築します。</span><span class="sxs-lookup"><span data-stu-id="c5a87-114">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="c5a87-115">[WebAssembly](http://webassembly.org) (略称 *wasm*) によって、Web ブラウザー内で .NET コードを実行することが可能になります。</span><span class="sxs-lookup"><span data-stu-id="c5a87-115">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="c5a87-116">WebAssembly はオープンな Web 標準であり、プラグインを使わずに Web ブラウザー内でサポートされます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-116">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="c5a87-117">WebAssembly は、ダウンロードを高速化し実行速度を最大限に高めるために最適化されたコンパクトなバイトコード形式です。</span><span class="sxs-lookup"><span data-stu-id="c5a87-117">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="c5a87-118">WebAssembly コードを使用すると、JavaScript 相互運用を介してブラウザーのすべての機能にアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-118">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="c5a87-119">それと同時に、WebAssembly を介して実行される .NET コードは、クライアント コンピューター上での悪意のあるアクションを禁止するために JavaScript と同じ信頼されたサンドボックス内で実行されます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-119">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor では、WebAssembly を使用してブラウザー内で .NET コードが実行されます。](index/_static/blazor.png)

<span data-ttu-id="c5a87-121">Blazor アプリをビルドしてブラウザーで実行する場合: </span><span class="sxs-lookup"><span data-stu-id="c5a87-121">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="c5a87-122">C# コード ファイルと Razor ファイルが .NET アセンブリにコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-122">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="c5a87-123">そのアセンブリと .NET ランタイムがブラウザーにダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-123">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="c5a87-124">Blazor により .NET ランタイムがブートストラップされ、アプリのアセンブリを読み込むようにそのランタイムが構成されます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-124">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="c5a87-125">ドキュメント オブジェクト モデル (DOM) 操作とブラウザー API の呼び出しは、JavaScript 相互運用を介して Blazor ランタイムによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-125">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="c5a87-126">Blazor は、ほとんどのアプリで必要となる次のコア機能をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="c5a87-126">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="c5a87-127">パラメーター</span><span class="sxs-lookup"><span data-stu-id="c5a87-127">Parameters</span></span>
* <span data-ttu-id="c5a87-128">イベント処理</span><span class="sxs-lookup"><span data-stu-id="c5a87-128">Event handling</span></span>
* <span data-ttu-id="c5a87-129">データ バインディング</span><span class="sxs-lookup"><span data-stu-id="c5a87-129">Data binding</span></span>
* <span data-ttu-id="c5a87-130">ルーティング</span><span class="sxs-lookup"><span data-stu-id="c5a87-130">Routing</span></span>
* <span data-ttu-id="c5a87-131">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="c5a87-131">Dependency injection</span></span>
* <span data-ttu-id="c5a87-132">レイアウト</span><span class="sxs-lookup"><span data-stu-id="c5a87-132">Layouts</span></span>
* <span data-ttu-id="c5a87-133">テンプレート</span><span class="sxs-lookup"><span data-stu-id="c5a87-133">Templating</span></span>
* <span data-ttu-id="c5a87-134">カスケード値</span><span class="sxs-lookup"><span data-stu-id="c5a87-134">Cascading values</span></span>

<span data-ttu-id="c5a87-135">ダウンロードされるアプリのサイズを小さくするため、アプリが[中間言語 (IL) リンカー](xref:host-and-deploy/razor-components/configure-linker)によって発行されるときに、アプリから未使用コードが除去されます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-135">To reduce the size of the downloaded app unused code stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/razor-components/configure-linker).</span></span>

<span data-ttu-id="c5a87-136">Blazor は、Razor Components 用のクライアント側のホスティング モデルです。</span><span class="sxs-lookup"><span data-stu-id="c5a87-136">Blazor is the client-side hosting model for Razor Components.</span></span> <span data-ttu-id="c5a87-137">Razor Components ではコンポーネントのレンダリング ロジックが UI の更新の適用方法から切り離されているため、Razor Components をホストする方法には柔軟性があります。</span><span class="sxs-lookup"><span data-stu-id="c5a87-137">Because Razor Components decouple a component's rendering logic from how UI updates are applied, there's flexibility in how Razor Components can be hosted.</span></span> <span data-ttu-id="c5a87-138">ASP.NET Core Razor Component を使って、サーバー上の Razor Components を ASP.NET Core アプリでホストします。UI の更新は SignalR 接続を介して処理されます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-138">Use ASP.NET Core Razor Components to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="c5a87-139">詳細については、「<xref:razor-components/hosting-models#server-side-hosting-model>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c5a87-139">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span> 

## <a name="components"></a><span data-ttu-id="c5a87-140">コンポーネント</span><span class="sxs-lookup"><span data-stu-id="c5a87-140">Components</span></span>

<span data-ttu-id="c5a87-141">*Razor Component* は、ページ、ダイアログ、データ入力フォームなど、UI の一部です。</span><span class="sxs-lookup"><span data-stu-id="c5a87-141">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="c5a87-142">コンポーネントではユーザー イベントが処理され、柔軟な UI のレンダリング ロジックが定義されます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-142">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="c5a87-143">コンポーネントは、入れ子にしたり再利用したりできます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-143">Components can be nested and reused.</span></span>

<span data-ttu-id="c5a87-144">コンポーネントは .NET アセンブリに組み込まれた .NET クラスであり、NuGet パッケージとして共有したり配布したりできます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-144">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="c5a87-145">このクラスは、Razor マークアップ ページの形式 (*.cshtml*) で記述することも、C# クラス (*.cs*) として記述することもできます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-145">The class can either be written in the form of a Razor markup page (*.cshtml*) or as a C# class (*.cs*).</span></span>

<span data-ttu-id="c5a87-146">[Razor](xref:mvc/views/razor) とは、HTML マークアップを C# コードに結合するための構文です。</span><span class="sxs-lookup"><span data-stu-id="c5a87-146">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="c5a87-147">Razor は開発者の生産性向上を目的に設計されています。これにより開発者は [IntelliSense](/visualstudio/ide/using-intellisense) サポートを使用して同じファイル内でマークアップと C# を切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-147">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="c5a87-148">Razor Pages と MVC ビューでも Razor が使われます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-148">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="c5a87-149">要求/応答モデルを中心に構築される Razor Pages や MVC ビューとは異なり、コンポーネントは特に UI コンポジションを処理するために使われます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-149">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="c5a87-150">Razor Components は特にクライアント側の UI ロジックとコンポジションのために使えます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-150">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="c5a87-151">次のマークアップは、Razor ファイル (*DialogComponent.cshtml*) に含まれるカスタム ダイアログ コンポーネントの例です。</span><span class="sxs-lookup"><span data-stu-id="c5a87-151">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="c5a87-152">このコンポーネントをアプリ内の他の場所で使用する場合、IntelliSense の構文およびパラメーター補完により開発を迅速化できます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-152">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="c5a87-153">コンポーネントは、"*レンダリング ツリー*" と呼ばれるブラウザー DOM のメモリ内表現としてレンダリングされ、柔軟かつ効率的な方法で UI を更新するために使えるようになります。</span><span class="sxs-lookup"><span data-stu-id="c5a87-153">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="c5a87-154">JavaScript 相互運用</span><span class="sxs-lookup"><span data-stu-id="c5a87-154">JavaScript interop</span></span>

<span data-ttu-id="c5a87-155">サード パーティ製の JavaScript ライブラリおよびブラウザーの API を必要とするアプリのために、Blazor は JavaScript と相互運用します。</span><span class="sxs-lookup"><span data-stu-id="c5a87-155">For apps that require third-party JavaScript libraries and browser APIs, Blazor interoperates with JavaScript.</span></span> <span data-ttu-id="c5a87-156">コンポーネントでは、JavaScript で使用できるライブラリまたは API はいずれも使用することができます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-156">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="c5a87-157">C# コードによる JavaScript コードの呼び出し、および JavaScript コードによる C# コードの呼び出しを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-157">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="c5a87-158">詳細については、[JavaScript 相互運用](xref:razor-components/javascript-interop)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c5a87-158">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="c5a87-159">コードの共有と .NET Standard</span><span class="sxs-lookup"><span data-stu-id="c5a87-159">Code sharing and .NET Standard</span></span>

<span data-ttu-id="c5a87-160">アプリでは、既存の [.NET Standard](/dotnet/standard/net-standard) ライブラリを参照および使用することができます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-160">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="c5a87-161">.NET Standard は、.NET 実装全体で共通した .NET API の標準仕様です。</span><span class="sxs-lookup"><span data-stu-id="c5a87-161">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="c5a87-162">.NET Standard 2.0 以降がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="c5a87-162">.NET Standard 2.0 or higher is supported.</span></span> <span data-ttu-id="c5a87-163">Web ブラウザー内で適用できない API (たとえば、ファイル システムにアクセスする機能、ソケットを開く機能、スレッド機能など) からは、<xref:System.PlatformNotSupportedException> がスローされます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-163">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="c5a87-164">.NET Standard クラス ライブラリは、サーバー コード全体で共有、およびブラウザー ベースのアプリ内で共有することができます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-164">.NET Standard class libraries can be shared across server code and in browser-based apps.</span></span>

## <a name="optimization"></a><span data-ttu-id="c5a87-165">最適化</span><span class="sxs-lookup"><span data-stu-id="c5a87-165">Optimization</span></span>

<span data-ttu-id="c5a87-166">ペイロードのサイズは、アプリの使用性に関する重要なパフォーマンス要因です。</span><span class="sxs-lookup"><span data-stu-id="c5a87-166">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="c5a87-167">Blazor では、ダウンロード時間を短縮するためにペイロードのサイズが最適化されます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-167">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="c5a87-168">.NET アセンブリの未使用部分がビルド処理中に削除されます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-168">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="c5a87-169">HTTP 応答が圧縮されます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-169">HTTP responses are compressed.</span></span>
* <span data-ttu-id="c5a87-170">.NET ランタイムとアセンブリがブラウザーにキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-170">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="c5a87-171">[サーバー側の Razor Components](xref:razor-components/index) では、.NET アセンブリ、アプリのアセンブリ、およびランタイムをサーバー側で維持することにより、Blazor よりもさらにペイロードのサイズが縮小されます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-171">[Server-side Razor Components](xref:razor-components/index) provides an even smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="c5a87-172">Razor Components アプリでは、マークアップ ファイルと静的資産のみがクライアントに提供されます。</span><span class="sxs-lookup"><span data-stu-id="c5a87-172">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c5a87-173">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="c5a87-173">Additional resources</span></span>

* <xref:razor-components/index>
* [<span data-ttu-id="c5a87-174">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="c5a87-174">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="c5a87-175">C# のガイド</span><span class="sxs-lookup"><span data-stu-id="c5a87-175">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="c5a87-176">HTML</span><span class="sxs-lookup"><span data-stu-id="c5a87-176">HTML</span></span>](https://www.w3.org/html/)
