---
title: ASP.NET Core の概要
author: rick-anderson
description: インターネットに接続された最新のクラウド ベース アプリケーションを構築するための、クロス プラットフォームで高パフォーマンスのオープン ソース フレームワークである ASP.NET Core について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2019
uid: index
ms.openlocfilehash: e7c81ff82e5206a5aca217417f6cb1c339d72e89
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341408"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="5daba-103">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="5daba-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="5daba-104">著者: [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="5daba-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="5daba-105">ASP.NET Core は、インターネットに接続された最新のクラウド ベース アプリケーションを構築するための、クロス プラットフォームで高パフォーマンスの[オープン ソース](https://github.com/aspnet/home) フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="5daba-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="5daba-106">ASP.NET Core では次のことができます。</span><span class="sxs-lookup"><span data-stu-id="5daba-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="5daba-107">Web アプリ、Web サービス、[IoT](https://www.microsoft.com/internet-of-things/) アプリ、モバイル バックエンドを構築する。</span><span class="sxs-lookup"><span data-stu-id="5daba-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="5daba-108">Windows、macOS、Linux で好みの開発ツールを使う。</span><span class="sxs-lookup"><span data-stu-id="5daba-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="5daba-109">クラウドまたはオンプレミスに展開する。</span><span class="sxs-lookup"><span data-stu-id="5daba-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="5daba-110">[.NET Core または .NET Framework](/dotnet/articles/standard/choosing-core-framework-server) 上で実行する。</span><span class="sxs-lookup"><span data-stu-id="5daba-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="5daba-111">ASP.NET Core を使う理由</span><span class="sxs-lookup"><span data-stu-id="5daba-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="5daba-112">何百万人もの開発者が、これまで、そして現在も、Web アプリの作成に [ASP.NET 4.x](/aspnet/overview) を使っています。</span><span class="sxs-lookup"><span data-stu-id="5daba-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="5daba-113">ASP.NET Core は ASP.NET 4.x を設計し直したものであり、無駄のないモジュール形式のフレームワークになるようにアーキテクチャが変更されています。</span><span class="sxs-lookup"><span data-stu-id="5daba-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="5daba-114">ASP.NET Core MVC を使って Web API と Web UI を構築する</span><span class="sxs-lookup"><span data-stu-id="5daba-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="5daba-115">ASP.NET Core MVC は、[Web API](xref:tutorials/first-web-api) と [Web アプリ](xref:tutorials/razor-pages/index)を構築する機能を備えています。</span><span class="sxs-lookup"><span data-stu-id="5daba-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="5daba-116">[モデル ビュー コントローラー (MVC) パターン](xref:mvc/overview)は、Web API と Web アプリをテスト可能にするのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="5daba-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="5daba-117">[Razor ページ](xref:razor-pages/index)はページ ベースのプログラミング モデルであり、Web UI の開発を容易にし、生産性を高めます。</span><span class="sxs-lookup"><span data-stu-id="5daba-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="5daba-118">[Razor マークアップ](xref:mvc/views/razor)では、[Razor ページ](xref:razor-pages/index)および [MVC ビュー](xref:mvc/views/overview)用に生産性の高い構文が提供されます。</span><span class="sxs-lookup"><span data-stu-id="5daba-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="5daba-119">[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)を使うと、Razor ファイルでの HTML 要素の作成とレンダリングに、サーバー側コードを組み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="5daba-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="5daba-120">[複数のデータ形式とコンテンツ ネゴシエーション](xref:web-api/advanced/formatting)の組み込みサポートにより、Web API はブラウザーやモバイル デバイスなどのさまざまなクライアントと接続できます。</span><span class="sxs-lookup"><span data-stu-id="5daba-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="5daba-121">[モデル バインド](xref:mvc/models/model-binding)は、HTTP 要求からアクション メソッドのパラメーターにデータを自動的にマップします。</span><span class="sxs-lookup"><span data-stu-id="5daba-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="5daba-122">[モデル検証](xref:mvc/models/validation)では、クライアント側とサーバー側の検証が自動的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="5daba-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="5daba-123">クライアント側の開発</span><span class="sxs-lookup"><span data-stu-id="5daba-123">Client-side development</span></span>

<span data-ttu-id="5daba-124">ASP.NET Core は、人気のあるクライアント側のフレームワークとライブラリ ([Angular](xref:spa/angular)、[React](xref:spa/react)、[Bootstrap](https://getbootstrap.com/) など) をシームレスに統合します。</span><span class="sxs-lookup"><span data-stu-id="5daba-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="5daba-125">詳しくは、「[クライアント側の開発](xref:client-side/index)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="5daba-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="5daba-126">.NET Framework を対象とする ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5daba-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="5daba-127">ASP.NET Core 2.x は、.NET Core または .NET Framework を対象にすることができます。</span><span class="sxs-lookup"><span data-stu-id="5daba-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="5daba-128">.NET Framework を対象とする ASP.NET Core アプリはクロスプラットフォームではありません&mdash;Windows でのみ実行されます。</span><span class="sxs-lookup"><span data-stu-id="5daba-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="5daba-129">一般に、ASP.NET Core 2.x は [.NET Standard](/dotnet/standard/net-standard) ライブラリで構成されています。</span><span class="sxs-lookup"><span data-stu-id="5daba-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="5daba-130">.NET Standard 2.0 で記述されたアプリは、.NET Standard 2.0 がサポートされていればどこでも実行できます。</span><span class="sxs-lookup"><span data-stu-id="5daba-130">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="5daba-131">ASP.NET Core 2.x は、.NET Standard 2.0 との互換性を持つ .NET Framework バージョンにおいてサポートされています。</span><span class="sxs-lookup"><span data-stu-id="5daba-131">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="5daba-132">.NET framework 4.7.1 以降を強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5daba-132">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="5daba-133">.NET Framework 4.6.1 以降。</span><span class="sxs-lookup"><span data-stu-id="5daba-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="5daba-134">ASP.NET Core 3.0 以降は、.NET Core でのみ実行されます。</span><span class="sxs-lookup"><span data-stu-id="5daba-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="5daba-135">この変更に関する詳細については、「[ASP.NET Core 3.0 で導入される変更について](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5daba-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="5daba-136">.NET Core を対象とする利点はいくつかあり、リリースのたびにその利点が増えています。</span><span class="sxs-lookup"><span data-stu-id="5daba-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="5daba-137">.NET Framework 経由による .NET Core には次のような利点があります。</span><span class="sxs-lookup"><span data-stu-id="5daba-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="5daba-138">クロスプラットフォームである。</span><span class="sxs-lookup"><span data-stu-id="5daba-138">Cross-platform.</span></span> <span data-ttu-id="5daba-139">macOS、Linux、Windows で実行できる。</span><span class="sxs-lookup"><span data-stu-id="5daba-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="5daba-140">パフォーマンスの向上</span><span class="sxs-lookup"><span data-stu-id="5daba-140">Improved performance</span></span>
* <span data-ttu-id="5daba-141">side-by-side でのバージョン管理</span><span class="sxs-lookup"><span data-stu-id="5daba-141">Side-by-side versioning</span></span>
* <span data-ttu-id="5daba-142">新しい API</span><span class="sxs-lookup"><span data-stu-id="5daba-142">New APIs</span></span>
* <span data-ttu-id="5daba-143">ソースを開く</span><span class="sxs-lookup"><span data-stu-id="5daba-143">Open source</span></span>

<span data-ttu-id="5daba-144">.NET Framework と .NET Core の間にある API のギャップを埋めるため、鋭意作業中です。</span><span class="sxs-lookup"><span data-stu-id="5daba-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="5daba-145">[Windows 互換機能パック](/dotnet/core/porting/windows-compat-pack)により、多くの Windows 限定の API が .NET Core で利用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="5daba-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="5daba-146">このような API は .NET Core 1.x で利用できませんでした。</span><span class="sxs-lookup"><span data-stu-id="5daba-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="5daba-147">サンプルをダウンロードする方法</span><span class="sxs-lookup"><span data-stu-id="5daba-147">How to download a sample</span></span>

<span data-ttu-id="5daba-148">多くの記事やチュートリアルにサンプル コードへのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5daba-148">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="5daba-149">[ASP.NET リポジトリの zip ファイルをダウンロード](https://codeload.github.com/aspnet/Docs/zip/master)します。</span><span class="sxs-lookup"><span data-stu-id="5daba-149">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="5daba-150">*Docs-master.zip* ファイルを解凍します。</span><span class="sxs-lookup"><span data-stu-id="5daba-150">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="5daba-151">サンプル リンクの URL を使って、サンプル ディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="5daba-151">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="5daba-152">サンプル コードのプリプロセッサ ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="5daba-152">Preprocessor directives in sample code</span></span>

<span data-ttu-id="5daba-153">複数のシナリオを示すため、サンプル アプリでは `#define` と `#if-#else/#elif-#endif` の C# ステートメントを使用してさまざまなサンプル コードのセクションを選択してコンパイルし、実行します。</span><span class="sxs-lookup"><span data-stu-id="5daba-153">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` C# statements to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="5daba-154">このアプローチを活用するサンプルでは、C# ファイルの上部にある `#define` ステートメントを、実行するシナリオに関連付けられたシンボルに設定します。</span><span class="sxs-lookup"><span data-stu-id="5daba-154">For those samples that make use of this approach, set the `#define` statement at the top of the C# files to the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="5daba-155">一部のサンプルでは、シナリオを実行するために複数のファイルの上部でシンボルを設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5daba-155">Some samples require setting the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="5daba-156">たとえば、次の `#define` のシンボル一覧は、4 つのシナリオが使用可能である (シンボルごとに 1 つのシナリオ) ことを示しています。</span><span class="sxs-lookup"><span data-stu-id="5daba-156">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="5daba-157">現在のサンプル構成では `TemplateCode` のシナリオが実行されます。</span><span class="sxs-lookup"><span data-stu-id="5daba-157">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="5daba-158">`ExpandDefault` のシナリオを実行するサンプルを変更するには、`ExpandDefault` のシンボルを定義し、残りのシンボルをコメント アウトしたままにします。</span><span class="sxs-lookup"><span data-stu-id="5daba-158">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="5daba-159">[C# プリプロセッサ ディレクティブ](/dotnet/csharp/language-reference/preprocessor-directives/)を使用してコードのセクションを選択的にコンパイルする方法の詳細については、「[#define (C# リファレンス)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define)」および「[#if (C# リファレンス)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5daba-159">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="5daba-160">サンプル コードのリージョン</span><span class="sxs-lookup"><span data-stu-id="5daba-160">Regions in sample code</span></span>

<span data-ttu-id="5daba-161">一部のサンプル アプリには、[#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) と [#end-region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) の C# ステートメントに囲まれたコードのセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5daba-161">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# statements.</span></span> <span data-ttu-id="5daba-162">ドキュメントのビルド システムによって、レンダリングされたドキュメントのトピックにこのリージョンが挿入されます。</span><span class="sxs-lookup"><span data-stu-id="5daba-162">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="5daba-163">リージョン名には通常、"snippet" という単語が含まれています。</span><span class="sxs-lookup"><span data-stu-id="5daba-163">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="5daba-164">次の例は `snippet_FilterInCode` という名前のリージョンを示しています。</span><span class="sxs-lookup"><span data-stu-id="5daba-164">The following example shows a region named `snippet_FilterInCode`:</span></span>

```csharp
#region snippet_FilterInCode
WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
        logging.AddFilter("System", LogLevel.Debug)
            .AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Trace))
            .Build();
#endregion
```

<span data-ttu-id="5daba-165">先述の C# コード スニペットは、トピックのマークダウン ファイルの次の行で示されています。</span><span class="sxs-lookup"><span data-stu-id="5daba-165">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_FilterInCode)]
```

<span data-ttu-id="5daba-166">コードを囲む `#region` と `#endregion` のステートメントは安全に無視 (または削除) することができます。</span><span class="sxs-lookup"><span data-stu-id="5daba-166">You may safely ignore (or remove) the `#region` and `#endregion` statements that surround the code.</span></span> <span data-ttu-id="5daba-167">トピックで説明されているサンプル シナリオを実行する予定がある場合は、これらのステートメント内のコードを変更しないでください。</span><span class="sxs-lookup"><span data-stu-id="5daba-167">Don't alter the code within these statements if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="5daba-168">他のシナリオを試す場合は、自由にコードを変更できます。</span><span class="sxs-lookup"><span data-stu-id="5daba-168">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="5daba-169">詳細については、「[Contribute to the ASP.NET documentation: Code snippets (ASP.NET に貢献する: コード スニペット)](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5daba-169">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5daba-170">次の手順</span><span class="sxs-lookup"><span data-stu-id="5daba-170">Next steps</span></span>

<span data-ttu-id="5daba-171">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="5daba-171">For more information, see the following resources:</span></span>

* [<span data-ttu-id="5daba-172">Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="5daba-172">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="5daba-173">ASP.NET Core の基礎</span><span class="sxs-lookup"><span data-stu-id="5daba-173">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="5daba-174">[週 1 回の ASP.NET Community Standup](https://live.asp.net/) では、チームの進行状況とプランが報告され、</span><span class="sxs-lookup"><span data-stu-id="5daba-174">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="5daba-175">新しいブログやサード パーティ製ソフトウェアが取り上げられています。</span><span class="sxs-lookup"><span data-stu-id="5daba-175">It features new blogs and third-party software.</span></span>
