---
title: Razor Components の概要
author: guardrex
description: ASP.NET Core アプリ内に .NET を使った対話型のクライアント側 Web UI を構築する方法である、ASP.NET Core Razor Components について調べます。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/index
ms.openlocfilehash: 8b2e87fe856598a5ac231e3bc1d413957829b448
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2019
ms.locfileid: "58751013"
---
# <a name="introduction-to-razor-components"></a><span data-ttu-id="90fc2-103">Razor Components の概要</span><span class="sxs-lookup"><span data-stu-id="90fc2-103">Introduction to Razor Components</span></span>

<span data-ttu-id="90fc2-104">作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="90fc2-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="90fc2-105">*Razor Components* は、.NET を使った対話型のクライアント側 Web UI を構築するための方法です。</span><span class="sxs-lookup"><span data-stu-id="90fc2-105">*Razor Components* is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="90fc2-106">JavaScript の代わりに C# を使って、多機能な対話型 UI を構築します。</span><span class="sxs-lookup"><span data-stu-id="90fc2-106">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="90fc2-107">すべて .NET で記述された、サーバー側とクライアント側アプリのロジックを共有します。</span><span class="sxs-lookup"><span data-stu-id="90fc2-107">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="90fc2-108">モバイル ブラウザーを含めた広範なブラウザーのサポートのために、HTML および CSS として UI をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="90fc2-108">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="90fc2-109">Razor Components は、ほとんどのアプリで必要となる次のコア機能をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="90fc2-109">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="90fc2-110">パラメーター</span><span class="sxs-lookup"><span data-stu-id="90fc2-110">Parameters</span></span>
* <span data-ttu-id="90fc2-111">イベント処理</span><span class="sxs-lookup"><span data-stu-id="90fc2-111">Event handling</span></span>
* <span data-ttu-id="90fc2-112">データ バインディング</span><span class="sxs-lookup"><span data-stu-id="90fc2-112">Data binding</span></span>
* <span data-ttu-id="90fc2-113">ルーティング</span><span class="sxs-lookup"><span data-stu-id="90fc2-113">Routing</span></span>
* <span data-ttu-id="90fc2-114">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="90fc2-114">Dependency injection</span></span>
* <span data-ttu-id="90fc2-115">レイアウト</span><span class="sxs-lookup"><span data-stu-id="90fc2-115">Layouts</span></span>
* <span data-ttu-id="90fc2-116">テンプレート</span><span class="sxs-lookup"><span data-stu-id="90fc2-116">Templating</span></span>
* <span data-ttu-id="90fc2-117">カスケード値</span><span class="sxs-lookup"><span data-stu-id="90fc2-117">Cascading values</span></span>

<span data-ttu-id="90fc2-118">Razor Components では、UI の更新を適用する方法からコンポーネントのレンダリング ロジックが分離されます。</span><span class="sxs-lookup"><span data-stu-id="90fc2-118">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="90fc2-119">.NET Core 3.0 の ASP.NET Core Razor Component には、ASP.NET Core アプリでサーバー上の Razor Components をホストするためのサポートが追加されています。</span><span class="sxs-lookup"><span data-stu-id="90fc2-119">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="90fc2-120">UI の更新は SignalR 接続を介して処理されます。</span><span class="sxs-lookup"><span data-stu-id="90fc2-120">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="90fc2-121">ランタイムは:</span><span class="sxs-lookup"><span data-stu-id="90fc2-121">The runtime:</span></span>

* <span data-ttu-id="90fc2-122">ブラウザーからサーバーへの UI イベントの送信を処理します。</span><span class="sxs-lookup"><span data-stu-id="90fc2-122">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="90fc2-123">サーバーから送信された UI の更新を、コンポーネントの実行後にブラウザーに適用します。</span><span class="sxs-lookup"><span data-stu-id="90fc2-123">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="90fc2-124">ブラウザーと通信するために Razor Components で使われる接続は、JavaScript 相互運用の呼び出しを処理するためにも使われます。</span><span class="sxs-lookup"><span data-stu-id="90fc2-124">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Razor Components では、サーバー上で .NET コードが実行され、SignalR 接続を介してクライアント上のドキュメント オブジェクト モデルとのやりとりが行われます。](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="90fc2-126">詳細については、「<xref:razor-components/hosting-models#server-side-hosting-model>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="90fc2-126">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span>

<span data-ttu-id="90fc2-127">*Blazor* は、Razor Components の実験的なクライアント側のホスティング モデルです。</span><span class="sxs-lookup"><span data-stu-id="90fc2-127">*Blazor* is the experimental client-side hosting model of Razor Components.</span></span> <span data-ttu-id="90fc2-128">Blazor は、オープンな Web 標準を使ってブラウザー内の .NET 上で実行され、プラグインもコード トランスパイルも必要ありません。</span><span class="sxs-lookup"><span data-stu-id="90fc2-128">Blazor runs on .NET in the browser using open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="90fc2-129">詳細については、次のトピックを参照してください。 <xref:spa/blazor/index> および <xref:razor-components/hosting-models#client-side-hosting-model></span><span class="sxs-lookup"><span data-stu-id="90fc2-129">For more information, see <xref:spa/blazor/index> and <xref:razor-components/hosting-models#client-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="90fc2-130">コンポーネント</span><span class="sxs-lookup"><span data-stu-id="90fc2-130">Components</span></span>

<span data-ttu-id="90fc2-131">*Razor Component* は、ページ、ダイアログ、データ入力フォームなど、UI の一部です。</span><span class="sxs-lookup"><span data-stu-id="90fc2-131">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="90fc2-132">コンポーネントではユーザー イベントが処理され、柔軟な UI のレンダリング ロジックが定義されます。</span><span class="sxs-lookup"><span data-stu-id="90fc2-132">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="90fc2-133">コンポーネントは、入れ子にしたり再利用したりできます。</span><span class="sxs-lookup"><span data-stu-id="90fc2-133">Components can be nested and reused.</span></span>

<span data-ttu-id="90fc2-134">コンポーネントは .NET アセンブリに組み込まれた .NET クラスであり、NuGet パッケージとして共有したり配布したりできます。</span><span class="sxs-lookup"><span data-stu-id="90fc2-134">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="90fc2-135">クラスは通常、Razor マークアップ ページの形式で、*.razor* ファイル拡張子と共に記述します。</span><span class="sxs-lookup"><span data-stu-id="90fc2-135">The class is normally written in the form of a Razor markup page with a *.razor* file extension.</span></span>

<span data-ttu-id="90fc2-136">[Razor](xref:mvc/views/razor) とは、HTML マークアップを C# コードに結合するための構文です。</span><span class="sxs-lookup"><span data-stu-id="90fc2-136">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="90fc2-137">Razor は開発者の生産性向上を目的に設計されています。これにより開発者は [IntelliSense](/visualstudio/ide/using-intellisense) サポートを使用して同じファイル内でマークアップと C# を切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="90fc2-137">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="90fc2-138">Razor Pages と MVC ビューでも Razor が使われます。</span><span class="sxs-lookup"><span data-stu-id="90fc2-138">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="90fc2-139">要求/応答モデルを中心に構築される Razor Pages や MVC ビューとは異なり、コンポーネントは特に UI コンポジションを処理するために使われます。</span><span class="sxs-lookup"><span data-stu-id="90fc2-139">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="90fc2-140">Razor Components は特にクライアント側の UI ロジックとコンポジションのために使えます。</span><span class="sxs-lookup"><span data-stu-id="90fc2-140">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="90fc2-141">次のマークアップは、Razor ファイルに含まれるカスタム ダイアログ コンポーネントの例です (*DialogComponent.razor*)。</span><span class="sxs-lookup"><span data-stu-id="90fc2-141">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.razor*):</span></span>

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

<span data-ttu-id="90fc2-142">このコンポーネントをアプリ内の他の場所で使用する場合、IntelliSense の構文およびパラメーター補完により開発を迅速化できます。</span><span class="sxs-lookup"><span data-stu-id="90fc2-142">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="90fc2-143">コンポーネントは、"*レンダリング ツリー*" と呼ばれるブラウザー DOM のメモリ内表現としてレンダリングされ、柔軟かつ効率的な方法で UI を更新するために使えるようになります。</span><span class="sxs-lookup"><span data-stu-id="90fc2-143">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="90fc2-144">JavaScript 相互運用</span><span class="sxs-lookup"><span data-stu-id="90fc2-144">JavaScript interop</span></span>

<span data-ttu-id="90fc2-145">サード パーティ製の JavaScript ライブラリおよびブラウザーの API を必要とするアプリのために、コンポーネントは JavaScript と相互運用します。</span><span class="sxs-lookup"><span data-stu-id="90fc2-145">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="90fc2-146">コンポーネントでは、JavaScript で使用できるライブラリまたは API はいずれも使用することができます。</span><span class="sxs-lookup"><span data-stu-id="90fc2-146">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="90fc2-147">C# コードによる JavaScript コードの呼び出し、および JavaScript コードによる C# コードの呼び出しを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="90fc2-147">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="90fc2-148">詳細については、[JavaScript 相互運用](xref:razor-components/javascript-interop)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="90fc2-148">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="90fc2-149">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="90fc2-149">Additional resources</span></span>

* <xref:spa/blazor/index>
* [<span data-ttu-id="90fc2-150">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="90fc2-150">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="90fc2-151">C# のガイド</span><span class="sxs-lookup"><span data-stu-id="90fc2-151">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="90fc2-152">HTML</span><span class="sxs-lookup"><span data-stu-id="90fc2-152">HTML</span></span>](https://www.w3.org/html/)
