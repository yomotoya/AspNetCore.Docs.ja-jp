---
title: "ASP.NET Core の概要"
author: rick-anderson
description: "インターネットに接続された最新のクラウド ベース アプリケーションを構築するための、クロス プラットフォームで高パフォーマンスのオープン ソース フレームワークである ASP.NET Core について説明します。"
manager: wpickett
ms.author: riande
ms.date: 02/28/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: index
ms.openlocfilehash: 103b7862900e08488dcc0f5fc78c08fefcfa17f3
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="0cddf-103">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="0cddf-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="0cddf-104">著者: [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="0cddf-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="0cddf-105">ASP.NET Core は、インターネットに接続された最新のクラウド ベース アプリケーションを構築するための、クロス プラットフォームで高パフォーマンスの[オープン ソース](https://github.com/aspnet/home) フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="0cddf-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="0cddf-106">ASP.NET Core では次のことができます。</span><span class="sxs-lookup"><span data-stu-id="0cddf-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="0cddf-107">Web アプリ、Web サービス、[IoT](https://www.microsoft.com/internet-of-things/) アプリ、モバイル バックエンドを構築する。</span><span class="sxs-lookup"><span data-stu-id="0cddf-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="0cddf-108">Windows、macOS、Linux で好みの開発ツールを使う。</span><span class="sxs-lookup"><span data-stu-id="0cddf-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="0cddf-109">クラウドまたはオンプレミスに展開する。</span><span class="sxs-lookup"><span data-stu-id="0cddf-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="0cddf-110">[.NET Core または .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) 上で実行する。</span><span class="sxs-lookup"><span data-stu-id="0cddf-110">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="0cddf-111">ASP.NET Core を使う理由</span><span class="sxs-lookup"><span data-stu-id="0cddf-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="0cddf-112">何百万人もの開発者が、これまで、そして現在も、Web アプリの作成に [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) を使っています。</span><span class="sxs-lookup"><span data-stu-id="0cddf-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="0cddf-113">ASP.NET Core は ASP.NET 4.x を設計し直したものであり、無駄のないモジュール形式のフレームワークになるようにアーキテクチャが変更されています。</span><span class="sxs-lookup"><span data-stu-id="0cddf-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

<span data-ttu-id="0cddf-114">ASP.NET Core の利点は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="0cddf-114">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="0cddf-115">Web UI と Web API を構築するプロセスの統一。</span><span class="sxs-lookup"><span data-stu-id="0cddf-115">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="0cddf-116">[最新のクライアント側フレームワーク](xref:client-side/index)と開発ワークフローの統合。</span><span class="sxs-lookup"><span data-stu-id="0cddf-116">Integration of [modern, client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="0cddf-117">クラウド対応で環境ベースの[構成システム](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="0cddf-117">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration/index).</span></span>
* <span data-ttu-id="0cddf-118">組み込まれている[依存性の注入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="0cddf-118">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="0cddf-119">軽量で[高パフォーマンス](https://github.com/aspnet/benchmarks)のモジュール化された HTTP 要求パイプライン。</span><span class="sxs-lookup"><span data-stu-id="0cddf-119">A lightweight, [high-performance](https://github.com/aspnet/benchmarks), and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="0cddf-120">[IIS](xref:host-and-deploy/iis/index)、[Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache)、[Docker](xref:host-and-deploy/docker/index) でホストする、または独自のプロセスで自己ホストする機能。</span><span class="sxs-lookup"><span data-stu-id="0cddf-120">Ability to host on [IIS](xref:host-and-deploy/iis/index), [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), [Docker](xref:host-and-deploy/docker/index), or self-host in your own process.</span></span>
* <span data-ttu-id="0cddf-121">[.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) に対応する場合は、アプリのサイド バイ サイド バージョン管理をサポート。</span><span class="sxs-lookup"><span data-stu-id="0cddf-121">Side-by-side app versioning when targeting [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
* <span data-ttu-id="0cddf-122">最新の Web 開発を簡単にするツール。</span><span class="sxs-lookup"><span data-stu-id="0cddf-122">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="0cddf-123">Windows、macOS、Linux でビルドおよび実行する機能。</span><span class="sxs-lookup"><span data-stu-id="0cddf-123">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="0cddf-124">オープン ソースで[コミュニティ重視](https://live.asp.net/)。</span><span class="sxs-lookup"><span data-stu-id="0cddf-124">Open-source and [community-focused](https://live.asp.net/).</span></span>

<span data-ttu-id="0cddf-125">ASP.NET Core は、[NuGet](https://www.nuget.org/) パッケージとして完全に提供されます。</span><span class="sxs-lookup"><span data-stu-id="0cddf-125">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="0cddf-126">NuGet パッケージを使用することにより、必要な依存関係だけを含むようにアプリを最適化できます。</span><span class="sxs-lookup"><span data-stu-id="0cddf-126">Using NuGet packages allows you to optimize your app to include only the necessary dependencies.</span></span> <span data-ttu-id="0cddf-127">実際に、.NET Core に対応した ASP.NET Core 2.x アプリで必要なのは、[1 つの NuGet パッケージ](xref:fundamentals/metapackage)だけです。</span><span class="sxs-lookup"><span data-stu-id="0cddf-127">In fact, ASP.NET Core 2.x apps targeting .NET Core only require a [single NuGet package](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="0cddf-128">小さいアプリ領域の利点には、セキュリティの強化、サービスの削減、パフォーマンスの向上などがあります。</span><span class="sxs-lookup"><span data-stu-id="0cddf-128">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="0cddf-129">ASP.NET Core MVC を使って Web API と Web UI を構築する</span><span class="sxs-lookup"><span data-stu-id="0cddf-129">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="0cddf-130">ASP.NET Core MVC は、[Web API](xref:tutorials/index#build-web-apis) と [Web アプリ](xref:tutorials/index#build-web-apps)を構築する機能を備えています。</span><span class="sxs-lookup"><span data-stu-id="0cddf-130">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/index#build-web-apis) and [web apps](xref:tutorials/index#build-web-apps):</span></span>

* <span data-ttu-id="0cddf-131">[モデル ビュー コントローラー (MVC) パターン](xref:mvc/overview)は、Web API と Web アプリを[テスト可能](testing/index.md)にするのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="0cddf-131">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="0cddf-132">[Razor ページ](xref:mvc/razor-pages/index) (ASP.NET Core 2.0 の新機能) はページ ベースのプログラミング モデルであり、Web UI の開発を容易にし、生産性を高めます。</span><span class="sxs-lookup"><span data-stu-id="0cddf-132">[Razor Pages](xref:mvc/razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="0cddf-133">[Razor マークアップ](xref:mvc/views/razor)では、[Razor ページ](xref:mvc/razor-pages/index)および [MVC ビュー](xref:mvc/views/overview)用に生産性の高い構文が提供されます。</span><span class="sxs-lookup"><span data-stu-id="0cddf-133">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:mvc/razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="0cddf-134">[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)を使うと、Razor ファイルでの HTML 要素の作成とレンダリングに、サーバー側コードを組み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="0cddf-134">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="0cddf-135">[複数のデータ形式とコンテンツ ネゴシエーション](mvc/models/formatting.md)の組み込みサポートにより、Web API はブラウザーやモバイル デバイスなどのさまざまなクライアントと接続できます。</span><span class="sxs-lookup"><span data-stu-id="0cddf-135">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="0cddf-136">[モデル バインド](xref:mvc/models/model-binding)は、HTTP 要求からアクション メソッドのパラメーターにデータを自動的にマップします。</span><span class="sxs-lookup"><span data-stu-id="0cddf-136">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="0cddf-137">[モデル検証](xref:mvc/models/validation)は、クライアント側とサーバー側の検証を自動的に実行します。</span><span class="sxs-lookup"><span data-stu-id="0cddf-137">[Model validation](xref:mvc/models/validation) automatically performs client- and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="0cddf-138">クライアント側の開発</span><span class="sxs-lookup"><span data-stu-id="0cddf-138">Client-side development</span></span>

<span data-ttu-id="0cddf-139">ASP.NET Core は、人気のあるクライアント側のフレームワークとライブラリ ([Angular](xref:spa/angular)、[React](xref:spa/react)、[Bootstrap](xref:client-side/bootstrap) など) をシームレスに統合します。</span><span class="sxs-lookup"><span data-stu-id="0cddf-139">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="0cddf-140">詳しくは、「[クライアント側の開発](xref:client-side/index)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0cddf-140">For more information, see [Client-side development](xref:client-side/index).</span></span>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="0cddf-141">.NET Framework を対象とする ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0cddf-141">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="0cddf-142">ASP.NET Core は、.NET Core または .NET Framework を対象にすることができます。</span><span class="sxs-lookup"><span data-stu-id="0cddf-142">ASP.NET Core can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="0cddf-143">.NET Framework を対象とする ASP.NET Core アプリはクロスプラットフォームではありません&mdash;Windows でのみ実行されます。</span><span class="sxs-lookup"><span data-stu-id="0cddf-143">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="0cddf-144">ASP.NET Core の .NET Framework を対象とするためのサポートを削除するプランはありません。</span><span class="sxs-lookup"><span data-stu-id="0cddf-144">There are no plans to remove support for targeting .NET Framework in ASP.NET Core.</span></span> <span data-ttu-id="0cddf-145">一般に、ASP.NET Core は [.NET Standard](/dotnet/standard/net-standard) ライブラリで構成されています。</span><span class="sxs-lookup"><span data-stu-id="0cddf-145">Generally, ASP.NET Core is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="0cddf-146">.NET Standard 2.0 で記述されたアプリは、.NET Standard 2.0 がサポートされていればどこでも実行できます。</span><span class="sxs-lookup"><span data-stu-id="0cddf-146">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="0cddf-147">.NET Core を対象とする利点はいくつかあり、リリースのたびにその利点が増えています。</span><span class="sxs-lookup"><span data-stu-id="0cddf-147">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="0cddf-148">.NET Framework 経由による .NET Core には次のような利点があります。</span><span class="sxs-lookup"><span data-stu-id="0cddf-148">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="0cddf-149">クロスプラットフォームである。</span><span class="sxs-lookup"><span data-stu-id="0cddf-149">Cross-platform.</span></span> <span data-ttu-id="0cddf-150">macOS、Linux、Windows で実行できる。</span><span class="sxs-lookup"><span data-stu-id="0cddf-150">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="0cddf-151">パフォーマンスの向上</span><span class="sxs-lookup"><span data-stu-id="0cddf-151">Improved performance</span></span>
* <span data-ttu-id="0cddf-152">side-by-side でのバージョン管理</span><span class="sxs-lookup"><span data-stu-id="0cddf-152">Side-by-side versioning</span></span>
* <span data-ttu-id="0cddf-153">新しい API</span><span class="sxs-lookup"><span data-stu-id="0cddf-153">New APIs</span></span>
* <span data-ttu-id="0cddf-154">ソースを開く</span><span class="sxs-lookup"><span data-stu-id="0cddf-154">Open source</span></span>

<span data-ttu-id="0cddf-155">.NET Framework と .NET Core の間にある API のギャップを埋めるため、鋭意作業中です。</span><span class="sxs-lookup"><span data-stu-id="0cddf-155">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="0cddf-156">[Windows 互換機能パック](/dotnet/core/porting/windows-compat-pack)により、多くの Windows 限定の API が .NET Core で利用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="0cddf-156">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="0cddf-157">このような API は .NET Core 1.x で利用できませんでした。</span><span class="sxs-lookup"><span data-stu-id="0cddf-157">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0cddf-158">次の手順</span><span class="sxs-lookup"><span data-stu-id="0cddf-158">Next steps</span></span>

<span data-ttu-id="0cddf-159">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0cddf-159">For more information, see the following resources:</span></span>

* [<span data-ttu-id="0cddf-160">Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="0cddf-160">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="0cddf-161">ASP.NET Core チュートリアル</span><span class="sxs-lookup"><span data-stu-id="0cddf-161">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="0cddf-162">ASP.NET Core の基礎</span><span class="sxs-lookup"><span data-stu-id="0cddf-162">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="0cddf-163">[週 1 回の ASP.NET Community Standup](https://live.asp.net/) では、チームの進行状況とプランが報告され、</span><span class="sxs-lookup"><span data-stu-id="0cddf-163">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="0cddf-164">新しいブログやサード パーティ製ソフトウェアが取り上げられています。</span><span class="sxs-lookup"><span data-stu-id="0cddf-164">It features new blogs and third-party software.</span></span>
