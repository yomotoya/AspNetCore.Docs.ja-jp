---
title: Razor Components の概要
author: guardrex
description: Blazor を探索します。これは C#/Razor や HTML を使用する .NET Web フレームワークであり WebAssembly を使用してブラウザー内で実行されます。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/index
ms.openlocfilehash: 0f7a4b2ec404b6ceec2e643fea9e0635de6ad716
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667940"
---
# <a name="introduction-to-razor-components"></a><span data-ttu-id="8c261-103">Razor Components の概要</span><span class="sxs-lookup"><span data-stu-id="8c261-103">Introduction to Razor Components</span></span>

<span data-ttu-id="8c261-104">作成者: [Steve Sanderson](http://blog.stevensanderson.com)、[Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8c261-104">By [Steve Sanderson](http://blog.stevensanderson.com), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="8c261-105">*ASP.NET Core Razor Components* はサーバー側において ASP.NET Core で実行されます。一方、*Blazor* (クライアント側で実行される Razor Components) は C#/Razor や HTML を使用する実験的な .NET Web フレームワークであり、WebAssembly を使用してブラウザー内で実行されます。</span><span class="sxs-lookup"><span data-stu-id="8c261-105">*ASP.NET Core Razor Components* executes server-side in ASP.NET Core, while *Blazor* (Razor Components executed client-side) is an experimental .NET web framework using C#/Razor and HTML that runs in the browser with WebAssembly.</span></span> <span data-ttu-id="8c261-106">Blazor では、クライアント上で .NET を使用するクライアント側 Web UI フレームワークの利点がすべて提供されます。</span><span class="sxs-lookup"><span data-stu-id="8c261-106">Blazor provides all of the benefits of a client-side web UI framework using .NET on the client.</span></span>

<span data-ttu-id="8c261-107">Web 開発は長年にわたって多くの点で改善されてきましたが、最新の Web アプリの構築にはまだ課題があります。</span><span class="sxs-lookup"><span data-stu-id="8c261-107">Web development has improved in many ways over the years, but building modern web apps still poses challenges.</span></span> <span data-ttu-id="8c261-108">ブラウザー内で .NET を使用すると、開発の簡略化や生産性向上に役立つ次のような多くの利点が得られます。</span><span class="sxs-lookup"><span data-stu-id="8c261-108">Using .NET in the browser offers many advantages that can help make web development easier and more productive:</span></span>

* <span data-ttu-id="8c261-109">**安定性と一貫性**: .NET では、安定性に優れ機能が豊富で使いやすい、標準化されたプログラミング フレームワークがプラットフォームを超えて実現されます。</span><span class="sxs-lookup"><span data-stu-id="8c261-109">**Stability and consistency**: .NET provides standardized programming frameworks across platforms that are stable, feature-rich, and easy to use.</span></span>
* <span data-ttu-id="8c261-110">**最新の革新的な言語**: .NET 言語は、革新的な新しい言語機能によって常に向上し続けています。</span><span class="sxs-lookup"><span data-stu-id="8c261-110">**Modern innovative languages**: .NET languages are constantly improving with innovative new language features.</span></span>
* <span data-ttu-id="8c261-111">**業界をリードするツール**: Visual Studio 製品ファミリでは、Windows、Linux、および macOS のプラットフォームを超えて非常にすばらしい .NET 開発エクスペリエンスが提供されています。</span><span class="sxs-lookup"><span data-stu-id="8c261-111">**Industry-leading tools**: The Visual Studio product family provides a fantastic .NET development experience across platforms on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="8c261-112">**速度とスケーラビリティ**: .NET では、アプリ開発のためのパフォーマンス、信頼性、およびセキュリティにおける強固な歴史があります。</span><span class="sxs-lookup"><span data-stu-id="8c261-112">**Speed and scalability**: .NET has a strong history of performance, reliability, and security for app development.</span></span> <span data-ttu-id="8c261-113">フル スタック ソリューションとして .NET を使用することにより、信頼性が高くセキュリティで保護された高速のアプリをさらに容易に構築できるようになります。</span><span class="sxs-lookup"><span data-stu-id="8c261-113">Using .NET as a full-stack solution makes it easier to build fast, reliable, and secure apps.</span></span>
* <span data-ttu-id="8c261-114">**既存のスキルを活用するフルスタック開発**: C#/Razor 開発者は、既に身に付けている C#/Razor スキルを使用してクライアント側コードを記述し、サーバーおよびクライアント側ロジックをアプリ間で共有します。</span><span class="sxs-lookup"><span data-stu-id="8c261-114">**Full-stack development that leverages existing skills**: C#/Razor developers use their existing C#/Razor skills to write client-side code and share server and client-side logic among apps.</span></span>
* <span data-ttu-id="8c261-115">**広範なブラウザー サポート**: Razor Components では、通常のマークアップおよび JavaScript として UI がレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="8c261-115">**Wide browser support**: Razor Components render the UI as ordinary markup and JavaScript.</span></span> <span data-ttu-id="8c261-116">Blazor は、オープンな Web 標準を使用してブラウザー内の .NET 上で実行され、プラグインもコード トランスパイルも必要ありません。</span><span class="sxs-lookup"><span data-stu-id="8c261-116">Blazor runs on .NET in the browser using open web standards with no plugins and no code transpilation.</span></span> <span data-ttu-id="8c261-117">Blazor はモバイル ブラウザーも含めて、すべての最新の Web ブラウザーで動作します。</span><span class="sxs-lookup"><span data-stu-id="8c261-117">Blazor works in all modern web browsers, including mobile browsers.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="8c261-118">ホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="8c261-118">Hosting models</span></span>

### <a name="server-side-hosting-model"></a><span data-ttu-id="8c261-119">サーバー側のホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="8c261-119">Server-side hosting model</span></span>

<span data-ttu-id="8c261-120">Razor Components ではコンポーネントのレンダリング ロジックが UI の更新の適用方法から切り離されているため、Razor Components をホストする方法には柔軟性があります。</span><span class="sxs-lookup"><span data-stu-id="8c261-120">Because Razor Components decouple a component's rendering logic from how the UI updates are applied, there is flexibility in how Razor Components can be hosted.</span></span> <span data-ttu-id="8c261-121">.NET Core 3.0 の ASP.NET Core Razor Component には、ASP.NET Core アプリによってサーバー上で Razor Components をホストするためのサポートが追加されています。この場合、すべての UI の更新が SignalR 接続を介して処理されます。</span><span class="sxs-lookup"><span data-stu-id="8c261-121">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app where all UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="8c261-122">ランタイムでは、ブラウザーからサーバーへの UI イベントの送信が処理され、次に、コンポーネントの実行後、サーバーによってブラウザーに返送された UI の更新が適用されます。</span><span class="sxs-lookup"><span data-stu-id="8c261-122">The runtime handles sending UI events from the browser to the server and then applies UI updates sent by the server back to the browser after running the components.</span></span> <span data-ttu-id="8c261-123">JavaScript 相互運用の呼び出しの処理にも同じ接続が使用されます。</span><span class="sxs-lookup"><span data-stu-id="8c261-123">The same connection is also used to handle JavaScript interop calls.</span></span>

![Razor Components では、サーバー上で .NET コードが実行され、SignalR 接続を介してクライアント上のドキュメント オブジェクト モデルとのやりとりが行われます。](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="8c261-125">詳細については、「<xref:razor-components/hosting-models#server-side-hosting-model>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8c261-125">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span>

### <a name="client-side-hosting-model"></a><span data-ttu-id="8c261-126">クライアント側のホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="8c261-126">Client-side hosting model</span></span>

<span data-ttu-id="8c261-127">比較的新しいテクノロジである [WebAssembly](http://webassembly.org) (*wasm* と略される) によって、Web ブラウザー内で .NET コードを実行することが可能になります。</span><span class="sxs-lookup"><span data-stu-id="8c261-127">Running .NET code inside web browsers is made possible by a relatively new technology, [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="8c261-128">WebAssembly はオープンな Web 標準であり、プラグインを使わずに Web ブラウザー内でサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8c261-128">WebAssembly is an open web standard and is supported in web browsers without plugins.</span></span> <span data-ttu-id="8c261-129">WebAssembly は、ダウンロードを高速化し実行速度を最大限に高めるために最適化されたコンパクトなバイトコード形式です。</span><span class="sxs-lookup"><span data-stu-id="8c261-129">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="8c261-130">WebAssembly コードを使用すると、JavaScript 相互運用を介してブラウザーのすべての機能にアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="8c261-130">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="8c261-131">それと同時に、WebAssembly コードは、クライアント コンピューター上での悪意のあるアクションを禁止するために JavaScript と同じ信頼されたサンドボックス内で実行されます。</span><span class="sxs-lookup"><span data-stu-id="8c261-131">At the same time, WebAssembly code runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor では、WebAssembly を使用してブラウザー内で .NET コードが実行されます。](index/_static/blazor.png)

<span data-ttu-id="8c261-133">Blazor アプリをビルドしてブラウザーで実行する場合: </span><span class="sxs-lookup"><span data-stu-id="8c261-133">When a Blazor app is built and run in a browser:</span></span>

1. <span data-ttu-id="8c261-134">C# コード ファイルと Razor ファイルが .NET アセンブリにコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="8c261-134">C# code files and Razor files are compiled into .NET assemblies.</span></span>
1. <span data-ttu-id="8c261-135">そのアセンブリと .NET ランタイムがブラウザーにダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="8c261-135">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
1. <span data-ttu-id="8c261-136">Blazor では JavaScript を使用して .NET ランタイムがブートス トラップされると共に、必要なアセンブリ参照を読み込むようにランタイムが構成されます。</span><span class="sxs-lookup"><span data-stu-id="8c261-136">Blazor uses JavaScript to bootstrap the .NET runtime and configures the runtime to load required assembly references.</span></span> <span data-ttu-id="8c261-137">ドキュメント オブジェクト モデル (DOM) 操作とブラウザー API 呼び出しは、JavaScript 相互運用を介して Blazor ランタイムによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="8c261-137">Document object model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interoperability.</span></span>

<span data-ttu-id="8c261-138">WebAssembly をサポートしていない、より古いブラウザーをサポートするには、ASP.NET Core Razor Components[ のサーバー側ホスティング モデル](#server-side-hosting-model)を使用します。</span><span class="sxs-lookup"><span data-stu-id="8c261-138">To support older browsers that don't support WebAssembly, you can use the ASP.NET Core Razor Components [server-side hosting model](#server-side-hosting-model).</span></span>

<span data-ttu-id="8c261-139">詳細については、「<xref:razor-components/hosting-models#client-side-hosting-model>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8c261-139">For more information, see <xref:razor-components/hosting-models#client-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="8c261-140">コンポーネント</span><span class="sxs-lookup"><span data-stu-id="8c261-140">Components</span></span>

<span data-ttu-id="8c261-141">アプリは "*コンポーネント*" を使用してビルドされます。</span><span class="sxs-lookup"><span data-stu-id="8c261-141">Apps are built with *components*.</span></span> <span data-ttu-id="8c261-142">コンポーネントとはページ、ダイアログ、データ エントリ フォームなど UI の一部です。</span><span class="sxs-lookup"><span data-stu-id="8c261-142">A component is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="8c261-143">コンポーネントは入れ子にしたり、再利用したり、プロジェクト間で共有したりできます。</span><span class="sxs-lookup"><span data-stu-id="8c261-143">Components can be nested, reused, and shared between projects.</span></span>

<span data-ttu-id="8c261-144">"*コンポーネント*" は .NET クラスです。</span><span class="sxs-lookup"><span data-stu-id="8c261-144">A *component* is a .NET class.</span></span> <span data-ttu-id="8c261-145">このクラスは C# クラス (*\*.cs*) として直接書き込むことも、より一般的な方法として Razor マークアップ ページの形式 (*\*.cshtml*) で書き込むこともできます。</span><span class="sxs-lookup"><span data-stu-id="8c261-145">The class can either be written directly, as a C# class (*\*.cs*), or more commonly in the form of a Razor markup page (*\*.cshtml*).</span></span>

<span data-ttu-id="8c261-146">[Razor](/aspnet/core/mvc/views/razor) とは、HTML マークアップを C# コードに結合するための構文です。</span><span class="sxs-lookup"><span data-stu-id="8c261-146">[Razor](/aspnet/core/mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="8c261-147">Razor は開発者の生産性向上を目的に設計されています。これにより開発者は [IntelliSense](/visualstudio/ide/using-intellisense) サポートを使用して同じファイル内でマークアップと C# を切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="8c261-147">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="8c261-148">次のマークアップは、Razor ファイル (*DialogComponent.cshtml*) に含まれる基本的なカスタム ダイアログ コンポーネントの例です。</span><span class="sxs-lookup"><span data-stu-id="8c261-148">The following markup is an example of a basic custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

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

<span data-ttu-id="8c261-149">このコンポーネントをアプリ内の他の場所で使用する場合、IntelliSense の構文およびパラメーター補完により開発を迅速化できます。</span><span class="sxs-lookup"><span data-stu-id="8c261-149">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="8c261-150">コンポーネントに対しては次の処理が行えます。</span><span class="sxs-lookup"><span data-stu-id="8c261-150">Components can be:</span></span>

* <span data-ttu-id="8c261-151">入れ子にする。</span><span class="sxs-lookup"><span data-stu-id="8c261-151">Nested.</span></span>
* <span data-ttu-id="8c261-152">Razor (*\*.cshtml*) または C# (*\*.cs*) コードを使用して作成する。</span><span class="sxs-lookup"><span data-stu-id="8c261-152">Created with Razor (*\*.cshtml*) or C# (*\*.cs*) code.</span></span>
* <span data-ttu-id="8c261-153">クラス ライブラリを介して共有する。</span><span class="sxs-lookup"><span data-stu-id="8c261-153">Shared via class libraries.</span></span>
* <span data-ttu-id="8c261-154">ブラウザー DOM を必要とせずに単体テストを行う。</span><span class="sxs-lookup"><span data-stu-id="8c261-154">Unit tested without requiring a browser DOM.</span></span>

## <a name="infrastructure"></a><span data-ttu-id="8c261-155">インフラストラクチャ</span><span class="sxs-lookup"><span data-stu-id="8c261-155">Infrastructure</span></span>

<span data-ttu-id="8c261-156">Razor Components には、ほとんどのアプリで必要となる次のコア機能が用意されています。</span><span class="sxs-lookup"><span data-stu-id="8c261-156">Razor Components offers the core facilities that most apps require, including:</span></span>

* <span data-ttu-id="8c261-157">レイアウト</span><span class="sxs-lookup"><span data-stu-id="8c261-157">Layouts</span></span>
* <span data-ttu-id="8c261-158">ルーティング</span><span class="sxs-lookup"><span data-stu-id="8c261-158">Routing</span></span>
* <span data-ttu-id="8c261-159">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="8c261-159">Dependency injection</span></span>

<span data-ttu-id="8c261-160">これらの機能はすべて必要に応じて設定できます。</span><span class="sxs-lookup"><span data-stu-id="8c261-160">All of these features are optional.</span></span> <span data-ttu-id="8c261-161">これらの機能のいずれか 1 つがアプリで使用されていない場合、実装は、[中間言語 (IL) リンカー](xref:host-and-deploy/razor-components/configure-linker)によって発行されたとき、アプリから除去されます。</span><span class="sxs-lookup"><span data-stu-id="8c261-161">When one of these features isn't used in an app, the implementation is stripped out of the app when published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/razor-components/configure-linker).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="8c261-162">コードの共有と .NET Standard</span><span class="sxs-lookup"><span data-stu-id="8c261-162">Code sharing and .NET Standard</span></span>

<span data-ttu-id="8c261-163">アプリでは、既存の [.NET Standard](/dotnet/standard/net-standard) ライブラリを参照および使用することができます。</span><span class="sxs-lookup"><span data-stu-id="8c261-163">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="8c261-164">.NET Standard は、.NET 実装全体で共通した .NET API の標準仕様です。</span><span class="sxs-lookup"><span data-stu-id="8c261-164">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="8c261-165">.NET Standard 2.0 以降がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="8c261-165">.NET Standard 2.0 or higher is supported.</span></span> <span data-ttu-id="8c261-166">Web ブラウザー内で適用できない API (たとえば、ファイル システムにアクセスする機能、ソケットを開く機能、スレッド機能など) からは、[PlatformNotSupportedException](/dotnet/api/system.platformnotsupportedexception) がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8c261-166">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw [PlatformNotSupportedException](/dotnet/api/system.platformnotsupportedexception).</span></span> <span data-ttu-id="8c261-167">.NET Standard クラス ライブラリは、サーバー コード全体で共有、およびブラウザー ベースのアプリ内で共有することができます。</span><span class="sxs-lookup"><span data-stu-id="8c261-167">.NET Standard class libraries can be shared across server code and in browser-based apps.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="8c261-168">JavaScript 相互運用</span><span class="sxs-lookup"><span data-stu-id="8c261-168">JavaScript interop</span></span>

<span data-ttu-id="8c261-169">サード パーティ製の JavaScript ライブラリおよびブラウザーの API を必要とするアプリのために、WebAssembly は JavaScript と相互運用できるように設計されています。</span><span class="sxs-lookup"><span data-stu-id="8c261-169">For apps that require third-party JavaScript libraries and browser APIs, WebAssembly is designed to interoperate with JavaScript.</span></span> <span data-ttu-id="8c261-170">Razor Components では、JavaScript で使用できるライブラリまたは API はいずれも使用することができます。</span><span class="sxs-lookup"><span data-stu-id="8c261-170">Razor Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="8c261-171">C# コードによる JavaScript コードの呼び出し、および JavaScript コードによる C# コードの呼び出しを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="8c261-171">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="8c261-172">詳細については、[JavaScript 相互運用](xref:razor-components/javascript-interop)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8c261-172">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="optimization"></a><span data-ttu-id="8c261-173">最適化</span><span class="sxs-lookup"><span data-stu-id="8c261-173">Optimization</span></span>

<span data-ttu-id="8c261-174">クライアント側アプリの場合は、ペイロードのサイズが重要です。</span><span class="sxs-lookup"><span data-stu-id="8c261-174">For client-side apps, payload size is critical.</span></span> <span data-ttu-id="8c261-175">Blazor では、ダウンロード時間を短縮できるようにペイロードのサイズが最適化されます。</span><span class="sxs-lookup"><span data-stu-id="8c261-175">Blazor optimizes payload size to reduce download times.</span></span> <span data-ttu-id="8c261-176">たとえば、ビルド プロセス中に .NET アセンブリの未使用の部分が削除され、HTTP 応答が圧縮され、.NET ランタイムとアセンブリがブラウザーでキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="8c261-176">For example, unused parts of .NET assemblies are removed during the build process, HTTP responses are compressed, and the .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="8c261-177">Razor Components では、.NET アセンブリ、アプリのアセンブリ、およびランタイムをサーバー側で維持することにより、Blazor よりもさらにペイロードのサイズが縮小されます。</span><span class="sxs-lookup"><span data-stu-id="8c261-177">Razor Components provides an even smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="8c261-178">Razor Components アプリでは、クライアントに対してマークアップ、スクリプト、スタイル シートのみが提供されます。</span><span class="sxs-lookup"><span data-stu-id="8c261-178">Razor Components apps only serve markup, script, and stylesheets to clients.</span></span>

## <a name="deployment"></a><span data-ttu-id="8c261-179">配置</span><span class="sxs-lookup"><span data-stu-id="8c261-179">Deployment</span></span>

<span data-ttu-id="8c261-180">Blazor を使用すると、純粋なスタンドアロン クライアント側アプリをビルドすることも、サーバー アプリとクライアント アプリの両方を含むフル スタック ASP.NET Core アプリをビルドすることもできます。</span><span class="sxs-lookup"><span data-stu-id="8c261-180">Use Blazor to build a pure standalone client-side app or a full-stack ASP.NET Core app that contains both server and client apps:</span></span>

* <span data-ttu-id="8c261-181">**クライアント側スタンドアロン アプリ**では、Blazor アプリは静的ファイルのみが格納される *dist* フォルダーにコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="8c261-181">In a **standalone client-side app**, the Blazor app is compiled into a *dist* folder that only contains static files.</span></span> <span data-ttu-id="8c261-182">Azure App Service、GitHub ページ、IIS (静的ファイル サーバーとして構成されている)、Node.js サーバーなど、さまざまなサーバーおよびサービス上で、それらのファイルをホストすることができます。</span><span class="sxs-lookup"><span data-stu-id="8c261-182">The files can be hosted on Azure App Service, GitHub Pages, IIS (configured as a static file server), Node.js servers, and many other servers and services.</span></span> <span data-ttu-id="8c261-183">運用環境のサーバー上では、.NET は必要はありません。</span><span class="sxs-lookup"><span data-stu-id="8c261-183">.NET isn't required on the server in production.</span></span>
* <span data-ttu-id="8c261-184">**フル スタック ASP.NET Core アプリ**では、サーバー アプリとクライアント アプリの間でコードを共有することができます。</span><span class="sxs-lookup"><span data-stu-id="8c261-184">In a **full-stack ASP.NET Core app**, code can be shared between server and client apps.</span></span> <span data-ttu-id="8c261-185">結果として生成される ASP.NET Core Razor Components アプリ (クライアント側 UI とその他のサーバー側 API エンドポイントに対応する) をビルドし、それを、ASP.NET Core によってサポートされる任意のクラウドまたはオンプレミスのホストに展開することができます。</span><span class="sxs-lookup"><span data-stu-id="8c261-185">The resulting ASP.NET Core Razor Components app, which serves the client-side UI and other server-side API endpoints, can be built and deployed to any cloud or on-premise host supported by ASP.NET Core.</span></span>

## <a name="suggest-a-feature-or-file-a-bug-report"></a><span data-ttu-id="8c261-186">機能の提案またはバグ報告の提出</span><span class="sxs-lookup"><span data-stu-id="8c261-186">Suggest a feature or file a bug report</span></span>

<span data-ttu-id="8c261-187">提案またはバグ報告の提出を行うには、[懸案事項を開いてください](https://github.com/aspnet/AspNetCore/issues/new)。</span><span class="sxs-lookup"><span data-stu-id="8c261-187">To make suggestions and file bug reports, please [open an issue](https://github.com/aspnet/AspNetCore/issues/new).</span></span> <span data-ttu-id="8c261-188">一般的なヘルプを取得する場合や、コミュニティから回答を得る場合は、[Gitter](https://gitter.im/aspnet/Blazor) での会話に参加してください。</span><span class="sxs-lookup"><span data-stu-id="8c261-188">For general help and to get answers from the community, join the conversation on [Gitter](https://gitter.im/aspnet/Blazor).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8c261-189">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="8c261-189">Additional resources</span></span>

* [<span data-ttu-id="8c261-190">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="8c261-190">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="8c261-191">C# のガイド</span><span class="sxs-lookup"><span data-stu-id="8c261-191">C# Guide</span></span>](/dotnet/csharp/)
* [<span data-ttu-id="8c261-192">Razor</span><span class="sxs-lookup"><span data-stu-id="8c261-192">Razor</span></span>](/aspnet/core/mvc/views/razor)
* [<span data-ttu-id="8c261-193">HTML</span><span class="sxs-lookup"><span data-stu-id="8c261-193">HTML</span></span>](https://www.w3.org/html/)
