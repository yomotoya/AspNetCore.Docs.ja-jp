---
title: ASP.NET Core の概要
author: rick-anderson
description: インターネットに接続された最新のクラウド ベース アプリケーションを構築するための、クロス プラットフォームで高パフォーマンスのオープン ソース フレームワークである ASP.NET Core について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: index
ms.openlocfilehash: fcd95b88b970073f4d7eddf89729683d18be449d
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090655"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="06364-103">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="06364-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="06364-104">著者: [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="06364-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="06364-105">ASP.NET Core は、インターネットに接続された最新のクラウド ベース アプリケーションを構築するための、クロス プラットフォームで高パフォーマンスの[オープン ソース](https://github.com/aspnet/home) フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="06364-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="06364-106">ASP.NET Core では次のことができます。</span><span class="sxs-lookup"><span data-stu-id="06364-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="06364-107">Web アプリ、Web サービス、[IoT](https://www.microsoft.com/internet-of-things/) アプリ、モバイル バックエンドを構築する。</span><span class="sxs-lookup"><span data-stu-id="06364-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="06364-108">Windows、macOS、Linux で好みの開発ツールを使う。</span><span class="sxs-lookup"><span data-stu-id="06364-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="06364-109">クラウドまたはオンプレミスに展開する。</span><span class="sxs-lookup"><span data-stu-id="06364-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="06364-110">[.NET Core または .NET Framework](/dotnet/articles/standard/choosing-core-framework-server) 上で実行する。</span><span class="sxs-lookup"><span data-stu-id="06364-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="06364-111">ASP.NET Core を使う理由</span><span class="sxs-lookup"><span data-stu-id="06364-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="06364-112">何百万人もの開発者が、これまで、そして現在も、Web アプリの作成に [ASP.NET 4.x](/aspnet/overview) を使っています。</span><span class="sxs-lookup"><span data-stu-id="06364-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="06364-113">ASP.NET Core は ASP.NET 4.x を設計し直したものであり、無駄のないモジュール形式のフレームワークになるようにアーキテクチャが変更されています。</span><span class="sxs-lookup"><span data-stu-id="06364-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="06364-114">ASP.NET Core MVC を使って Web API と Web UI を構築する</span><span class="sxs-lookup"><span data-stu-id="06364-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="06364-115">ASP.NET Core MVC は、[Web API](xref:tutorials/index#build-web-apis) と [Web アプリ](xref:tutorials/index#build-web-apps)を構築する機能を備えています。</span><span class="sxs-lookup"><span data-stu-id="06364-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/index#build-web-apis) and [web apps](xref:tutorials/index#build-web-apps):</span></span>

* <span data-ttu-id="06364-116">[モデル ビュー コントローラー (MVC) パターン](xref:mvc/overview)は、Web API と Web アプリを[テスト可能](xref:test/index)にするのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="06364-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](xref:test/index).</span></span>
* <span data-ttu-id="06364-117">[Razor ページ](xref:razor-pages/index) (ASP.NET Core 2.0 の新機能) はページ ベースのプログラミング モデルであり、Web UI の開発を容易にし、生産性を高めます。</span><span class="sxs-lookup"><span data-stu-id="06364-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="06364-118">[Razor マークアップ](xref:mvc/views/razor)では、[Razor ページ](xref:razor-pages/index)および [MVC ビュー](xref:mvc/views/overview)用に生産性の高い構文が提供されます。</span><span class="sxs-lookup"><span data-stu-id="06364-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="06364-119">[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)を使うと、Razor ファイルでの HTML 要素の作成とレンダリングに、サーバー側コードを組み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="06364-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="06364-120">[複数のデータ形式とコンテンツ ネゴシエーション](xref:web-api/advanced/formatting)の組み込みサポートにより、Web API はブラウザーやモバイル デバイスなどのさまざまなクライアントと接続できます。</span><span class="sxs-lookup"><span data-stu-id="06364-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="06364-121">[モデル バインド](xref:mvc/models/model-binding)は、HTTP 要求からアクション メソッドのパラメーターにデータを自動的にマップします。</span><span class="sxs-lookup"><span data-stu-id="06364-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="06364-122">[モデル検証](xref:mvc/models/validation)では、クライアント側とサーバー側の検証が自動的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="06364-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="06364-123">クライアント側の開発</span><span class="sxs-lookup"><span data-stu-id="06364-123">Client-side development</span></span>

<span data-ttu-id="06364-124">ASP.NET Core は、人気のあるクライアント側のフレームワークとライブラリ ([Angular](xref:spa/angular)、[React](xref:spa/react)、[Bootstrap](https://getbootstrap.com/) など) をシームレスに統合します。</span><span class="sxs-lookup"><span data-stu-id="06364-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="06364-125">詳しくは、「[クライアント側の開発](xref:client-side/index)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="06364-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="06364-126">.NET Framework を対象とする ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06364-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="06364-127">ASP.NET Core は、.NET Core または .NET Framework を対象にすることができます。</span><span class="sxs-lookup"><span data-stu-id="06364-127">ASP.NET Core can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="06364-128">.NET Framework を対象とする ASP.NET Core アプリはクロスプラットフォームではありません&mdash;Windows でのみ実行されます。</span><span class="sxs-lookup"><span data-stu-id="06364-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="06364-129">ASP.NET Core の .NET Framework を対象とするためのサポートを削除するプランはありません。</span><span class="sxs-lookup"><span data-stu-id="06364-129">There are no plans to remove support for targeting .NET Framework in ASP.NET Core.</span></span> <span data-ttu-id="06364-130">一般に、ASP.NET Core は [.NET Standard](/dotnet/standard/net-standard) ライブラリで構成されています。</span><span class="sxs-lookup"><span data-stu-id="06364-130">Generally, ASP.NET Core is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="06364-131">.NET Standard 2.0 で記述されたアプリは、.NET Standard 2.0 がサポートされていればどこでも実行できます。</span><span class="sxs-lookup"><span data-stu-id="06364-131">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="06364-132">ASP.NET Core 2.x は、.NET Standard 2.0 との互換性を持つ .NET Framework バージョンにおいてサポートされています。</span><span class="sxs-lookup"><span data-stu-id="06364-132">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="06364-133">.NET framework 4.7.1 以降を強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="06364-133">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="06364-134">.NET Framework 4.6.1 以降。</span><span class="sxs-lookup"><span data-stu-id="06364-134">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="06364-135">.NET Core を対象とする利点はいくつかあり、リリースのたびにその利点が増えています。</span><span class="sxs-lookup"><span data-stu-id="06364-135">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="06364-136">.NET Framework 経由による .NET Core には次のような利点があります。</span><span class="sxs-lookup"><span data-stu-id="06364-136">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="06364-137">クロスプラットフォームである。</span><span class="sxs-lookup"><span data-stu-id="06364-137">Cross-platform.</span></span> <span data-ttu-id="06364-138">macOS、Linux、Windows で実行できる。</span><span class="sxs-lookup"><span data-stu-id="06364-138">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="06364-139">パフォーマンスの向上</span><span class="sxs-lookup"><span data-stu-id="06364-139">Improved performance</span></span>
* <span data-ttu-id="06364-140">side-by-side でのバージョン管理</span><span class="sxs-lookup"><span data-stu-id="06364-140">Side-by-side versioning</span></span>
* <span data-ttu-id="06364-141">新しい API</span><span class="sxs-lookup"><span data-stu-id="06364-141">New APIs</span></span>
* <span data-ttu-id="06364-142">ソースを開く</span><span class="sxs-lookup"><span data-stu-id="06364-142">Open source</span></span>

<span data-ttu-id="06364-143">.NET Framework と .NET Core の間にある API のギャップを埋めるため、鋭意作業中です。</span><span class="sxs-lookup"><span data-stu-id="06364-143">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="06364-144">[Windows 互換機能パック](/dotnet/core/porting/windows-compat-pack)により、多くの Windows 限定の API が .NET Core で利用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="06364-144">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="06364-145">このような API は .NET Core 1.x で利用できませんでした。</span><span class="sxs-lookup"><span data-stu-id="06364-145">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="next-steps"></a><span data-ttu-id="06364-146">次の手順</span><span class="sxs-lookup"><span data-stu-id="06364-146">Next steps</span></span>

<span data-ttu-id="06364-147">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="06364-147">For more information, see the following resources:</span></span>

* [<span data-ttu-id="06364-148">Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="06364-148">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="06364-149">ASP.NET Core チュートリアル</span><span class="sxs-lookup"><span data-stu-id="06364-149">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="06364-150">ASP.NET Core の基礎</span><span class="sxs-lookup"><span data-stu-id="06364-150">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="06364-151">[週 1 回の ASP.NET Community Standup](https://live.asp.net/) では、チームの進行状況とプランが報告され、</span><span class="sxs-lookup"><span data-stu-id="06364-151">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="06364-152">新しいブログやサード パーティ製ソフトウェアが取り上げられています。</span><span class="sxs-lookup"><span data-stu-id="06364-152">It features new blogs and third-party software.</span></span>
