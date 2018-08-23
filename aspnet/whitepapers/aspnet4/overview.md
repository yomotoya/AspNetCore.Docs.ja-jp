---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 および Visual Studio 2010 Web 開発の概要 |Microsoft Docs
author: rick-anderson
description: このドキュメントは、ASP.NET と Visual Studio 2010 での.net Framework 4 に含まれている数多くの新しい機能の概要を提供します。
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 775286df610df9040cbf04125b1742b6befa055b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828026"
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a><span data-ttu-id="f7641-103">ASP.NET 4 および Visual Studio 2010 Web 開発の概要</span><span class="sxs-lookup"><span data-stu-id="f7641-103">ASP.NET 4 and Visual Studio 2010 Web Development Overview</span></span>
====================
> <span data-ttu-id="f7641-104">このドキュメントは、ASP.NET と Visual Studio 2010 での.net Framework 4 に含まれている数多くの新しい機能の概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="f7641-104">This document provides an overview of many of the new features for ASP.NET that are included in the.NET Framework 4 and in Visual Studio 2010.</span></span>
> 
> [<span data-ttu-id="f7641-105">このホワイト ペーパーをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="f7641-105">Download This Whitepaper</span></span>](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


<span data-ttu-id="f7641-106">**目次**</span><span class="sxs-lookup"><span data-stu-id="f7641-106">**Contents**</span></span>

<span data-ttu-id="f7641-107">**[コア サービス](#0.2__Toc253429238 "_Toc253429238")**</span><span class="sxs-lookup"><span data-stu-id="f7641-107">**[Core Services](#0.2__Toc253429238 "_Toc253429238")**</span></span>  
[<span data-ttu-id="f7641-108">Web.config ファイルがリファクタリング</span><span class="sxs-lookup"><span data-stu-id="f7641-108">Web.config File Refactoring</span></span>](#0.2__Toc253429239 "_Toc253429239")  
[<span data-ttu-id="f7641-109">拡張可能な出力キャッシュ</span><span class="sxs-lookup"><span data-stu-id="f7641-109">Extensible Output Caching</span></span>](#0.2__Toc253429240 "_Toc253429240")  
[<span data-ttu-id="f7641-110">Web アプリケーションの自動開始</span><span class="sxs-lookup"><span data-stu-id="f7641-110">Auto-Start Web Applications</span></span>](#0.2__Toc253429241 "_Toc253429241")  
[<span data-ttu-id="f7641-111">永続的なリダイレクト ページ</span><span class="sxs-lookup"><span data-stu-id="f7641-111">Permanently Redirecting a Page</span></span>](#0.2__Toc253429242 "_Toc253429242")  
[<span data-ttu-id="f7641-112">セッション状態を圧縮</span><span class="sxs-lookup"><span data-stu-id="f7641-112">Shrinking Session State</span></span>](#0.2__Toc253429243 "_Toc253429243")  
[<span data-ttu-id="f7641-113">許容される Url の範囲を拡大</span><span class="sxs-lookup"><span data-stu-id="f7641-113">Expanding the Range of Allowable URLs</span></span>](#0.2__Toc253429244 "_Toc253429244")  
[<span data-ttu-id="f7641-114">拡張可能な要求の検証</span><span class="sxs-lookup"><span data-stu-id="f7641-114">Extensible Request Validation</span></span>](#0.2__Toc253429245 "_Toc253429245")  
[<span data-ttu-id="f7641-115">オブジェクト キャッシュとキャッシュの機能拡張オブジェクト</span><span class="sxs-lookup"><span data-stu-id="f7641-115">Object Caching and Object Caching Extensibility</span></span>](#0.2__Toc253429246 "_Toc253429246")  
[<span data-ttu-id="f7641-116">拡張可能な HTML、URL、および HTTP ヘッダーのエンコーディング</span><span class="sxs-lookup"><span data-stu-id="f7641-116">Extensible HTML, URL, and HTTP Header Encoding</span></span>](#0.2__Toc253429247 "_Toc253429247")  
[<span data-ttu-id="f7641-117">1 つのワーカー プロセス内の個々 のアプリケーションのパフォーマンスを監視</span><span class="sxs-lookup"><span data-stu-id="f7641-117">Performance Monitoring for Individual Applications in a Single Worker Process</span></span>](#0.2__Toc253429248 "_Toc253429248")  
[<span data-ttu-id="f7641-118">Multi-Targeting</span><span class="sxs-lookup"><span data-stu-id="f7641-118">Multi-Targeting</span></span>](#0.2__Toc253429249 "_Toc253429249")

<span data-ttu-id="f7641-119">**[Ajax](#0.2__Toc253429250 "_Toc253429250")**</span><span class="sxs-lookup"><span data-stu-id="f7641-119">**[Ajax](#0.2__Toc253429250 "_Toc253429250")**</span></span>  
[<span data-ttu-id="f7641-120">含まれる Web フォームと MVC の jQuery</span><span class="sxs-lookup"><span data-stu-id="f7641-120">jQuery Included with Web Forms and MVC</span></span>](#0.2__Toc253429251 "_Toc253429251")  
[<span data-ttu-id="f7641-121">コンテンツ配信ネットワーク サポート</span><span class="sxs-lookup"><span data-stu-id="f7641-121">Content Delivery Network Support</span></span>](#0.2__Toc253429252 "_Toc253429252")  
[<span data-ttu-id="f7641-122">Scriptmanager コントロールの明示的なスクリプト</span><span class="sxs-lookup"><span data-stu-id="f7641-122">ScriptManager Explicit Scripts</span></span>](#0.2__Toc253429253 "_Toc253429253")

<span data-ttu-id="f7641-123">**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**</span><span class="sxs-lookup"><span data-stu-id="f7641-123">**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**</span></span>  
[<span data-ttu-id="f7641-124">Page.MetaKeywords Page.MetaDescription プロパティとメタ タグを設定</span><span class="sxs-lookup"><span data-stu-id="f7641-124">Setting Meta Tags with the Page.MetaKeywords and Page.MetaDescription Properties</span></span>](#0.2__Toc253429257 "_Toc253429257")  
[<span data-ttu-id="f7641-125">個々 のコントロール ビュー ステートを有効にする</span><span class="sxs-lookup"><span data-stu-id="f7641-125">Enabling View State for Individual Controls</span></span>](#0.2__Toc253429258 "_Toc253429258")  
[<span data-ttu-id="f7641-126">ブラウザーの機能への変更</span><span class="sxs-lookup"><span data-stu-id="f7641-126">Changes to Browser Capabilities</span></span>](#0.2__Toc253429259 "_Toc253429259")  
[<span data-ttu-id="f7641-127">ASP.NET 4 でのルーティング</span><span class="sxs-lookup"><span data-stu-id="f7641-127">Routing in ASP.NET 4</span></span>](#0.2__Toc253429260 "_Toc253429260")  
[<span data-ttu-id="f7641-128">クライアント Id を設定</span><span class="sxs-lookup"><span data-stu-id="f7641-128">Setting Client IDs</span></span>](#0.2__Toc253429261 "_Toc253429261")  
[<span data-ttu-id="f7641-129">データ コントロールで行の選択を保持する</span><span class="sxs-lookup"><span data-stu-id="f7641-129">Persisting Row Selection in Data Controls</span></span>](#0.2__Toc253429262 "_Toc253429262")  
[<span data-ttu-id="f7641-130">ASP.NET のグラフ コントロール</span><span class="sxs-lookup"><span data-stu-id="f7641-130">ASP.NET Chart Control</span></span>](#0.2__Toc253429263 "_Toc253429263")  
[<span data-ttu-id="f7641-131">による QueryExtender コントロールでデータをフィルター処理</span><span class="sxs-lookup"><span data-stu-id="f7641-131">Filtering Data with the QueryExtender Control</span></span>](#0.2__Toc253429264 "_Toc253429264")  
[<span data-ttu-id="f7641-132">Html エンコードのコード式</span><span class="sxs-lookup"><span data-stu-id="f7641-132">Html Encoded Code Expressions</span></span>](#0.2__Toc253429265 "_Toc253429265")  
[<span data-ttu-id="f7641-133">テンプレートの変更をプロジェクト</span><span class="sxs-lookup"><span data-stu-id="f7641-133">Project Template Changes</span></span>](#0.2__Toc253429266 "_Toc253429266")  
[<span data-ttu-id="f7641-134">CSS の改良点</span><span class="sxs-lookup"><span data-stu-id="f7641-134">CSS Improvements</span></span>](#0.2__Toc253429267 "_Toc253429267")  
[<span data-ttu-id="f7641-135">Div を非表示フィールドの周囲の要素を非表示</span><span class="sxs-lookup"><span data-stu-id="f7641-135">Hiding div Elements Around Hidden Fields</span></span>](#0.2__Toc253429268 "_Toc253429268")  
[<span data-ttu-id="f7641-136">テンプレート化されたコントロールの外側のテーブルをレンダリング</span><span class="sxs-lookup"><span data-stu-id="f7641-136">Rendering an Outer Table for Templated Controls</span></span>](#0.2__Toc253429269 "_Toc253429269")  
[<span data-ttu-id="f7641-137">ListView コントロールの機能強化</span><span class="sxs-lookup"><span data-stu-id="f7641-137">ListView Control Enhancements</span></span>](#0.2__Toc253429270 "_Toc253429270")  
[<span data-ttu-id="f7641-138">CheckBoxList、RadioButtonList コントロールの強化</span><span class="sxs-lookup"><span data-stu-id="f7641-138">CheckBoxList and RadioButtonList Control Enhancements</span></span>](#0.2__Toc253429271 "_Toc253429271")  
[<span data-ttu-id="f7641-139">メニュー コントロールの機能強化</span><span class="sxs-lookup"><span data-stu-id="f7641-139">Menu Control Improvements</span></span>](#0.2__Toc253429272 "_Toc253429272")  
[<span data-ttu-id="f7641-140">ウィザードおよび CreateUserWizard コントロール 56</span><span class="sxs-lookup"><span data-stu-id="f7641-140">Wizard and CreateUserWizard Controls 56</span></span>](#0.2__Toc253429273 "_Toc253429273")

<span data-ttu-id="f7641-141">**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**</span><span class="sxs-lookup"><span data-stu-id="f7641-141">**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**</span></span>  
[<span data-ttu-id="f7641-142">領域のサポート</span><span class="sxs-lookup"><span data-stu-id="f7641-142">Areas Support</span></span>](#0.2__Toc253429275 "_Toc253429275")  
[<span data-ttu-id="f7641-143">データ注釈属性の検証サポート</span><span class="sxs-lookup"><span data-stu-id="f7641-143">Data-Annotation Attribute Validation Support</span></span>](#0.2__Toc253429276 "_Toc253429276")  
[<span data-ttu-id="f7641-144">テンプレート化されたヘルパー</span><span class="sxs-lookup"><span data-stu-id="f7641-144">Templated Helpers</span></span>](#0.2__Toc253429277 "_Toc253429277")

<span data-ttu-id="f7641-145">**[動的データ](#0.2__Toc253429278 "_Toc253429278")**</span><span class="sxs-lookup"><span data-stu-id="f7641-145">**[Dynamic Data](#0.2__Toc253429278 "_Toc253429278")**</span></span>  
[<span data-ttu-id="f7641-146">既存のプロジェクトに対して動的なデータを有効にする</span><span class="sxs-lookup"><span data-stu-id="f7641-146">Enabling Dynamic Data for Existing Projects</span></span>](#0.2__Toc253429279 "_Toc253429279")  
[<span data-ttu-id="f7641-147">DynamicDataManager コントロールの宣言型構文</span><span class="sxs-lookup"><span data-stu-id="f7641-147">Declarative DynamicDataManager Control Syntax</span></span>](#0.2__Toc253429280 "_Toc253429280")  
[<span data-ttu-id="f7641-148">エンティティ テンプレート</span><span class="sxs-lookup"><span data-stu-id="f7641-148">Entity Templates</span></span>](#0.2__Toc253429281 "_Toc253429281")  
[<span data-ttu-id="f7641-149">Url および電子メール アドレスの新しいフィールド テンプレート</span><span class="sxs-lookup"><span data-stu-id="f7641-149">New Field Templates for URLs and E-mail Addresses</span></span>](#0.2__Toc253429282 "_Toc253429282")  
[<span data-ttu-id="f7641-150">DynamicHyperLink コントロールへのリンクを作成する</span><span class="sxs-lookup"><span data-stu-id="f7641-150">Creating Links with the DynamicHyperLink Control</span></span>](#0.2__Toc253429283 "_Toc253429283")  
[<span data-ttu-id="f7641-151">データ モデルにおける継承のサポート</span><span class="sxs-lookup"><span data-stu-id="f7641-151">Support for Inheritance in the Data Model</span></span>](#0.2__Toc253429284 "_Toc253429284")  
[<span data-ttu-id="f7641-152">多対多のリレーションシップ (Entity Framework) のサポート</span><span class="sxs-lookup"><span data-stu-id="f7641-152">Support for Many-to-Many Relationships (Entity Framework Only)</span></span>](#0.2__Toc253429285 "_Toc253429285")  
[<span data-ttu-id="f7641-153">コントロールの表示と列挙のサポートを新しい属性</span><span class="sxs-lookup"><span data-stu-id="f7641-153">New Attributes to Control Display and Support Enumerations</span></span>](#0.2__Toc253429286 "_Toc253429286")  
[<span data-ttu-id="f7641-154">フィルターのサポートが強化されて</span><span class="sxs-lookup"><span data-stu-id="f7641-154">Enhanced Support for Filters</span></span>](#0.2__Toc253429287 "_Toc253429287")

<span data-ttu-id="f7641-155">**[Visual Studio 2010 Web 開発の改良](#0.2__Toc253429288 "_Toc253429288")**</span><span class="sxs-lookup"><span data-stu-id="f7641-155">**[Visual Studio 2010 Web Development Improvements](#0.2__Toc253429288 "_Toc253429288")**</span></span>  
[<span data-ttu-id="f7641-156">CSS に関する互換性の改善</span><span class="sxs-lookup"><span data-stu-id="f7641-156">Improved CSS Compatibility</span></span>](#0.2__Toc253429289 "_Toc253429289")  
[<span data-ttu-id="f7641-157">HTML および JavaScript スニペット</span><span class="sxs-lookup"><span data-stu-id="f7641-157">HTML and JavaScript Snippets</span></span>](#0.2__Toc253429290 "_Toc253429290")  
[<span data-ttu-id="f7641-158">JavaScript IntelliSense の機能強化</span><span class="sxs-lookup"><span data-stu-id="f7641-158">JavaScript IntelliSense Enhancements</span></span>](#0.2__Toc253429291 "_Toc253429291")

<span data-ttu-id="f7641-159">**[Visual Studio 2010 でアプリケーションのデプロイを web](#0.2__Toc253429292 "_Toc253429292")**</span><span class="sxs-lookup"><span data-stu-id="f7641-159">**[Web Application Deployment with Visual Studio 2010](#0.2__Toc253429292 "_Toc253429292")**</span></span>  
[<span data-ttu-id="f7641-160">Web パッケージ</span><span class="sxs-lookup"><span data-stu-id="f7641-160">Web Packaging</span></span>](#0.2__Toc253429293 "_Toc253429293")  
[<span data-ttu-id="f7641-161">Web.config 変換</span><span class="sxs-lookup"><span data-stu-id="f7641-161">Web.config Transformation</span></span>](#0.2__Toc253429294 "_Toc253429294")  
[<span data-ttu-id="f7641-162">データベース配置</span><span class="sxs-lookup"><span data-stu-id="f7641-162">Database Deployment</span></span>](#0.2__Toc253429295 "_Toc253429295")  
[<span data-ttu-id="f7641-163">1 回のクリックは、Web アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="f7641-163">One-Click Publish for Web Applications</span></span>](#0.2__Toc253429296 "_Toc253429296")  
[<span data-ttu-id="f7641-164">Resources</span><span class="sxs-lookup"><span data-stu-id="f7641-164">Resources</span></span>](#0.2__Toc253429297 "_Toc253429297")

<span data-ttu-id="f7641-165">**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**</span><span class="sxs-lookup"><span data-stu-id="f7641-165">**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**</span></span>

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a><span data-ttu-id="f7641-166">コア サービス</span><span class="sxs-lookup"><span data-stu-id="f7641-166">Core Services</span></span>

<span data-ttu-id="f7641-167">ASP.NET 4 では、さまざまな出力のキャッシュおよびセッション状態の記憶域などの ASP.NET コア サービスを改善する機能が導入されています。</span><span class="sxs-lookup"><span data-stu-id="f7641-167">ASP.NET 4 introduces a number of features that improve core ASP.NET services such as output caching and session-state storage.</span></span>

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a><span data-ttu-id="f7641-168">Web.config ファイルのリファクタリング</span><span class="sxs-lookup"><span data-stu-id="f7641-168">Web.config File Refactoring</span></span>

<span data-ttu-id="f7641-169">`Web.config` Web アプリケーションが大きく成長、.NET Framework のいくつか過去のリリースで、Ajax など、新しい機能が追加されているように構成を含んでいるファイル ルーティング、および IIS 7 との統合。</span><span class="sxs-lookup"><span data-stu-id="f7641-169">The `Web.config` file that contains the configuration for a Web application has grown considerably over the past few releases of the .NET Framework as new features have been added, such as Ajax, routing, and integration with IIS 7.</span></span> <span data-ttu-id="f7641-170">これはで、構成、または Visual Studio などのツールを使用せずに新しい Web アプリケーションを起動するは困難です。</span><span class="sxs-lookup"><span data-stu-id="f7641-170">This has made it harder to configure or start new Web applications without a tool like Visual Studio.</span></span> <span data-ttu-id="f7641-171">.NET Framework 4 では、主要な構成要素を移動されていますが、`machine.config`ファイル、およびアプリケーションがこれらの設定を継承するようになりました。</span><span class="sxs-lookup"><span data-stu-id="f7641-171">In .the NET Framework 4, the major configuration elements have been moved to the `machine.config` file, and applications now inherit these settings.</span></span> <span data-ttu-id="f7641-172">これにより、`Web.config`を空にするかにのみ、次の行を Visual Studio のアプリケーションが対象とする framework のバージョンを指定します。 これを含めることが、ASP.NET 4 アプリケーション内のファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-172">This allows the `Web.config` file in ASP.NET 4 applications either to be empty or to contain just the following lines, which specify for Visual Studio what version of the framework the application is targeting:</span></span>

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a><span data-ttu-id="f7641-173">拡張可能な出力キャッシュ</span><span class="sxs-lookup"><span data-stu-id="f7641-173">Extensible Output Caching</span></span>

<span data-ttu-id="f7641-174">ASP.NET 1.0 がリリースされた時刻、以降は、出力キャッシュは、ページ、コントロール、および HTTP 応答の生成された出力をメモリに格納する開発者に有効に。</span><span class="sxs-lookup"><span data-stu-id="f7641-174">Since the time that ASP.NET 1.0 was released, output caching has enabled developers to store the generated output of pages, controls, and HTTP responses in memory.</span></span> <span data-ttu-id="f7641-175">以降の Web 要求で ASP.NET 使用できるコンテンツより迅速にゼロからの出力を再生成する代わりに、メモリから生成された出力を取得することによって。</span><span class="sxs-lookup"><span data-stu-id="f7641-175">On subsequent Web requests, ASP.NET can serve content more quickly by retrieving the generated output from memory instead of regenerating the output from scratch.</span></span> <span data-ttu-id="f7641-176">ただし、このアプローチには、制限、生成されたコンテンツは常にメモリに格納されるが、大量のトラフィックが発生しているサーバーで出力キャッシュによって消費されるメモリは、Web アプリケーションの他の部分からのメモリ要求と競合ことができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-176">However, this approach has a limitation — generated content always has to be stored in memory, and on servers that are experiencing heavy traffic, the memory consumed by output caching can compete with memory demands from other portions of a Web application.</span></span>

<span data-ttu-id="f7641-177">ASP.NET 4 では、1 つまたは複数のカスタム出力キャッシュ プロバイダーを構成することができますを出力キャッシュの機能拡張ポイントを追加します。</span><span class="sxs-lookup"><span data-stu-id="f7641-177">ASP.NET 4 adds an extensibility point to output caching that enables you to configure one or more custom output-cache providers.</span></span> <span data-ttu-id="f7641-178">出力キャッシュ プロバイダーは、任意のストレージ メカニズムを使用して、HTML コンテンツを保持することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-178">Output-cache providers can use any storage mechanism to persist HTML content.</span></span> <span data-ttu-id="f7641-179">これにより、多様な永続化メカニズムは、ローカルまたはリモートのディスクを含めることができます、クラウドの記憶域のカスタム出力キャッシュ プロバイダーを作成することにより、なり、分散キャッシュ エンジン。</span><span class="sxs-lookup"><span data-stu-id="f7641-179">This makes it possible to create custom output-cache providers for diverse persistence mechanisms, which can include local or remote disks, cloud storage, and distributed cache engines.</span></span>

<span data-ttu-id="f7641-180">新しいから派生するクラスとしてカスタム出力キャッシュ プロバイダーを作成する*System.Web.Caching.OutputCacheProvider*型。</span><span class="sxs-lookup"><span data-stu-id="f7641-180">You create a custom output-cache provider as a class that derives from the new *System.Web.Caching.OutputCacheProvider* type.</span></span> <span data-ttu-id="f7641-181">プロバイダーを構成することができますし、`Web.config`新しいを使用してファイル*プロバイダー*のサブセクション、 *outputCache*要素は、次の例に示すように。</span><span class="sxs-lookup"><span data-stu-id="f7641-181">You can then configure the provider in the `Web.config` file by using the new *providers* subsection of the *outputCache* element, as shown in the following example:</span></span>

[!code-xml[Main](overview/samples/sample2.xml)]

<span data-ttu-id="f7641-182">ASP.NET 4 では、すべての HTTP 応答で既定で表示するページ、およびコントロールの前の例に示すように、メモリ内の出力キャッシュを使用している、 *defaultProvider* AspNetInternalProvider に属性を設定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-182">By default in ASP.NET 4, all HTTP responses, rendered pages, and controls use the in-memory output cache, as shown in the previous example, where the *defaultProvider* attribute is set to AspNetInternalProvider.</span></span> <span data-ttu-id="f7641-183">別のプロバイダー名を指定して Web アプリケーションの使用、既定の出力キャッシュ プロバイダーを変更する*defaultProvider*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-183">You can change the default output-cache provider used for a Web application by specifying a different provider name for *defaultProvider*.</span></span>

<span data-ttu-id="f7641-184">さらに、コントロールおよび要求ごとに別の出力キャッシュ プロバイダーを選択できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-184">In addition, you can select different output-cache providers per control and per request.</span></span> <span data-ttu-id="f7641-185">別の Web ユーザー コントロールの別の出力キャッシュ プロバイダーを選択する最も簡単な方法は、new を使用して、宣言を行う、 *providerName*コントロール ディレクティブでは、次の例に示すように属性します。</span><span class="sxs-lookup"><span data-stu-id="f7641-185">The easiest way to choose a different output-cache provider for different Web user controls is to do so declaratively by using the new *providerName* attribute in a control directive, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample3.aspx)]

<span data-ttu-id="f7641-186">HTTP 要求の別の出力キャッシュ プロバイダーを指定するには、少し作業が必要です。</span><span class="sxs-lookup"><span data-stu-id="f7641-186">Specifying a different output cache provider for an HTTP request requires a little more work.</span></span> <span data-ttu-id="f7641-187">新しい上書きプロバイダーを宣言によって指定するには、代わりに*GetOuputCacheProviderName*メソッドで、`Global.asax`ファイルをプログラムで特定の要求に使用するプロバイダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-187">Instead of declaratively specifying the provider, you override the new *GetOuputCacheProviderName* method in the `Global.asax` file to programmatically specify which provider to use for a specific request.</span></span> <span data-ttu-id="f7641-188">その方法を次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-188">The following example shows how to do this.</span></span>

[!code-csharp[Main](overview/samples/sample4.cs)]

<span data-ttu-id="f7641-189">ASP.NET 4 を出力キャッシュ プロバイダー機能拡張の追加により、Web サイトのようになりましたよりアグレッシブなとより高度な出力キャッシュの戦略を追求できることにします。</span><span class="sxs-lookup"><span data-stu-id="f7641-189">With the addition of output-cache provider extensibility to ASP.NET 4, you can now pursue more aggressive and more intelligent output-caching strategies for your Web sites.</span></span> <span data-ttu-id="f7641-190">たとえば、ディスク上のトラフィックの削減を取得するページをキャッシュ中には、メモリ内のサイトの「トップ 10」ページをキャッシュすることはようになりました。</span><span class="sxs-lookup"><span data-stu-id="f7641-190">For example, it is now possible to cache the "Top 10" pages of a site in memory, while caching pages that get lower traffic on disk.</span></span> <span data-ttu-id="f7641-191">または、キャッシュ、レンダリングされるページのすべての変更によって組み合わせは、フロント エンド Web サーバーのメモリ使用量の負荷を軽減するために、分散キャッシュを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-191">Alternatively, you can cache every vary-by combination for a rendered page, but use a distributed cache so that the memory consumption is offloaded from front-end Web servers.</span></span>

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a><span data-ttu-id="f7641-192">Web アプリケーションの自動開始</span><span class="sxs-lookup"><span data-stu-id="f7641-192">Auto-Start Web Applications</span></span>

<span data-ttu-id="f7641-193">一部の Web アプリケーションは、大量のデータを読み込んだり、最初の要求を処理する前に処理負荷のかかる初期化を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-193">Some Web applications need to load large amounts of data or perform expensive initialization processing before serving the first request.</span></span> <span data-ttu-id="f7641-194">以前のバージョンの ASP.NET では、このような状況にする必要がありました ASP.NET アプリケーションを「起こす」し、時に初期化コードを実行するカスタム方法を考案、*アプリケーション\_ロード*メソッドで、 `Global.asax`ファイルです。</span><span class="sxs-lookup"><span data-stu-id="f7641-194">In earlier versions of ASP.NET, for these situations you had to devise custom approaches to "wake up" an ASP.NET application and then run initialization code during the *Application\_Load* method in the `Global.asax` file.</span></span>

<span data-ttu-id="f7641-195">という名前の新しいスケーラビリティ機能*自動開始*ことアドレスこのシナリオは、使用可能な直接 ASP.NET 4 で実行すると Windows Server 2008 R2 に IIS 7.5 です。</span><span class="sxs-lookup"><span data-stu-id="f7641-195">A new scalability feature named *auto-start* that directly addresses this scenario is available when ASP.NET 4 runs on IIS 7.5 on Windows Server 2008 R2.</span></span> <span data-ttu-id="f7641-196">自動開始機能は、アプリケーション プールの開始、ASP.NET アプリケーションの初期化、および HTTP 要求の受け入れのための制御方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="f7641-196">The auto-start feature provides a controlled approach for starting up an application pool, initializing an ASP.NET application, and then accepting HTTP requests.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="f7641-197">Iis 7.5、IIS アプリケーション ウォーム アップ モジュール</span><span class="sxs-lookup"><span data-stu-id="f7641-197">IIS Application Warm-Up Module for IIS 7.5</span></span>
> 
> <span data-ttu-id="f7641-198">For IIS 7.5、IIS チームはアプリケーションのウォーム アップ モジュールの最初のベータ版のテスト バージョンに公開しました。</span><span class="sxs-lookup"><span data-stu-id="f7641-198">The IIS team has released the first beta test version of the Application Warm-Up Module for IIS 7.5.</span></span> <span data-ttu-id="f7641-199">これにより、前に説明したよりもずっと簡単にアプリケーションを準備します。</span><span class="sxs-lookup"><span data-stu-id="f7641-199">This makes warming up your applications even easier than previously described.</span></span> <span data-ttu-id="f7641-200">カスタム コードを記述する代わりに、Web アプリケーションがネットワークからの要求を受け入れる前に実行するリソースの Url を指定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-200">Instead of writing custom code, you specify the URLs of resources to execute before the Web application accepts requests from the network.</span></span> <span data-ttu-id="f7641-201">このウォーム アップは、IIS サービスの起動処理中に発生します (として IIS アプリケーション プールを構成した場合*AlwaysRunning*) して、IIS ワーカー プロセスが再利用されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-201">This warm-up occurs during startup of the IIS service (if you configured the IIS application pool as *AlwaysRunning*) and when an IIS worker process recycles.</span></span> <span data-ttu-id="f7641-202">、リサイクル中には、古い IIS ワーカー プロセスは、アプリケーションの中断なしまたは unprimed キャッシュのための他の問題が発生するように、新しく作成されるワーカー プロセスが、ウォームが完全にまで要求を実行し続けます。</span><span class="sxs-lookup"><span data-stu-id="f7641-202">During recycle, the old IIS worker process continues to execute requests until the newly spawned worker process is fully warmed up, so that applications experience no interruptions or other issues due to unprimed caches.</span></span> <span data-ttu-id="f7641-203">このモジュールがバージョン 2.0 以降、ASP.NET の任意のバージョンで動作するに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f7641-203">Note that this module works with any version of ASP.NET, starting with version 2.0.</span></span>
> 
> <span data-ttu-id="f7641-204">詳細については、次を参照してください。 [Application Warm-Up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) 、IIS.net Web サイト。</span><span class="sxs-lookup"><span data-stu-id="f7641-204">For more information, see [Application Warm-Up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) on the IIS.net Web site.</span></span> <span data-ttu-id="f7641-205">ウォーム アップ機能を使用する方法について説明するチュートリアルでは、次を参照してください。[入門 IIS 7.5 アプリケーション ウォーム アップ モジュール](https://www.iis.net/learn/manage)、IIS.net Web サイト。</span><span class="sxs-lookup"><span data-stu-id="f7641-205">For a walkthrough that illustrates how to use the warm-up feature, see [Getting Started with the IIS 7.5 Application Warm-Up Module](https://www.iis.net/learn/manage) on the IIS.net Web site.</span></span>


<span data-ttu-id="f7641-206">Auto-start 機能を使用して、IIS 管理者では、次の構成を使用して自動的に起動する IIS 7.5 におけるアプリケーション プールを設定、`applicationHost.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-206">To use the auto-start feature, an IIS administrator sets an application pool in IIS 7.5 to be automatically started by using the following configuration in the `applicationHost.config` file:</span></span>

[!code-xml[Main](overview/samples/sample5.xml)]

<span data-ttu-id="f7641-207">次の構成を使用して自動的に開始する個々 のアプリケーションを指定するため、1 つのアプリケーション プールは、複数のアプリケーションを含めることができます、`applicationHost.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-207">Because a single application pool can contain multiple applications, you specify individual applications to be automatically started by using the following configuration in the `applicationHost.config` file:</span></span>

[!code-xml[Main](overview/samples/sample6.xml)]

<span data-ttu-id="f7641-208">IIS 7.5 サーバーがコールド起動の場合、または個別のアプリケーション プールがリサイクルされるときに、IIS 7.5 は、情報を使用して、`applicationHost.config`ファイルが自動的に開始する Web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="f7641-208">When an IIS 7.5 server is cold-started or when an individual application pool is recycled, IIS 7.5 uses the information in the `applicationHost.config` file to determine which Web applications need to be automatically started.</span></span> <span data-ttu-id="f7641-209">自動開始をマークされている各アプリケーションの IIS7.5 は ASP.NET 4 をアプリケーション一時的に受け入れません状態でアプリケーションを起動する HTTP 要求に、要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="f7641-209">For each application that is marked for auto-start, IIS7.5 sends a request to ASP.NET 4 to start the application in a state during which the application temporarily does not accept HTTP requests.</span></span> <span data-ttu-id="f7641-210">ASP.NET がによって定義された型をインスタンス化がこの状態にあるときに、 *serviceAutoStartProvider* (されている前の例で示した) 属性とそのパブリック エントリ ポイントへの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="f7641-210">When it is in this state, ASP.NET instantiates the type defined by the *serviceAutoStartProvider* attribute (as shown in the previous example) and calls into its public entry point.</span></span>

<span data-ttu-id="f7641-211">実装することで、必要なエントリ ポイントで管理された自動開始の種類を作成する、 *IProcessHostPreloadClient*インターフェイスを次の例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="f7641-211">You create a managed auto-start type with the necessary entry point by implementing the *IProcessHostPreloadClient* interface, as shown in the following example:</span></span>

[!code-csharp[Main](overview/samples/sample7.cs)]

<span data-ttu-id="f7641-212">初期化後にコードを実行、*プリロード*メソッドとメソッドから制御が戻る、ASP.NET アプリケーションに要求を処理する準備ができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-212">After your initialization code runs in the *Preload* method and the method returns, the ASP.NET application is ready to process requests.</span></span>

<span data-ttu-id="f7641-213">.5 IIS および ASP.NET 4 に自動開始の追加により、最初の HTTP 要求を処理する前に、負荷の高いアプリケーションの初期化を実行するための適切に定義された方法があるようになりました。</span><span class="sxs-lookup"><span data-stu-id="f7641-213">With the addition of auto-start to IIS .5 and ASP.NET 4, you now have a well-defined approach for performing expensive application initialization prior to processing the first HTTP request.</span></span> <span data-ttu-id="f7641-214">たとえば、新しい自動開始機能を使用して、アプリケーションを初期化して、アプリケーションが初期化され、HTTP トラフィックを受け入れる準備ができてであるロード バランサーを通知することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-214">For example, you can use the new auto-start feature to initialize an application and then signal a load-balancer that the application was initialized and ready to accept HTTP traffic.</span></span>

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a><span data-ttu-id="f7641-215">完全にページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="f7641-215">Permanently Redirecting a Page</span></span>

<span data-ttu-id="f7641-216">検索エンジンに古くなったリンクの累積が発生する可能性があります時間の経過と共にページおよびその他のコンテンツの周囲を移動する Web アプリケーションで一般的です。</span><span class="sxs-lookup"><span data-stu-id="f7641-216">It is common practice in Web applications to move pages and other content around over time, which can lead to an accumulation of stale links in search engines.</span></span> <span data-ttu-id="f7641-217">ASP.NET では、開発者がこれまで古い Url への要求を使用して処理を使用して、 *Response.Redirect*新しい URL に要求を転送するメソッド。</span><span class="sxs-lookup"><span data-stu-id="f7641-217">In ASP.NET, developers have traditionally handled requests to old URLs by using by using the *Response.Redirect* method to forward a request to the new URL.</span></span> <span data-ttu-id="f7641-218">ただし、*リダイレクト*メソッドは、ユーザーが、古い Url にアクセスしようとしています。 ときに、余分な http ラウンド トリップ結果、HTTP 302 Found (一時的なリダイレクト) 応答を発行します。</span><span class="sxs-lookup"><span data-stu-id="f7641-218">However, the *Redirect* method issues an HTTP 302 Found (temporary redirect) response, which results in an extra HTTP round trip when users attempt to access the old URLs.</span></span>

<span data-ttu-id="f7641-219">ASP.NET 4 に新しく追加*RedirectPermanent*ヘルパー メソッドで簡単に HTTP 301 の問題を次の例のように、応答を完全に移動します。</span><span class="sxs-lookup"><span data-stu-id="f7641-219">ASP.NET 4 adds a new *RedirectPermanent* helper method that makes it easy to issue HTTP 301 Moved Permanently responses, as in the following example:</span></span>

[!code-csharp[Main](overview/samples/sample8.cs)]

<span data-ttu-id="f7641-220">検索エンジンと永続的なリダイレクトを認識するその他のユーザー エージェントは、一時的なリダイレクトのブラウザーによって行われた不要なラウンド トリップを排除すると、コンテンツに関連付けられている新しい URL に格納されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-220">Search engines and other user agents that recognize permanent redirects will store the new URL that is associated with the content, which eliminates the unnecessary round trip made by the browser for temporary redirects.</span></span>

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a><span data-ttu-id="f7641-221">セッション状態を圧縮します。</span><span class="sxs-lookup"><span data-stu-id="f7641-221">Shrinking Session State</span></span>

<span data-ttu-id="f7641-222">ASP.NET Web ファーム全体でセッション状態を格納するための 2 つの既定オプションを提供します。 セッション状態プロバイダーをアウト プロセス セッション状態サーバーを呼び出すと、Microsoft SQL Server データベースにデータを格納するセッション状態プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="f7641-222">ASP.NET provides two default options for storing session state across a Web farm: a session-state provider that invokes an out-of-process session-state server, and a session-state provider that stores data in a Microsoft SQL Server database.</span></span> <span data-ttu-id="f7641-223">両方のオプションは、Web アプリケーションのワーカー プロセスの外部の状態情報を格納するが含まれる、ために、セッション状態をリモート ストレージに送信される前にシリアル化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-223">Because both options involve storing state information outside a Web application's worker process, session state has to be serialized before it is sent to remote storage.</span></span> <span data-ttu-id="f7641-224">によって、開発者がセッション状態の保存情報の量、シリアル化されたデータのサイズが非常に大きく拡張可能です。</span><span class="sxs-lookup"><span data-stu-id="f7641-224">Depending on how much information a developer saves in session state, the size of the serialized data can grow quite large.</span></span>

<span data-ttu-id="f7641-225">ASP.NET 4 では、アウト プロセス セッション状態プロバイダーの両方の種類の新しい圧縮オプションを紹介します。</span><span class="sxs-lookup"><span data-stu-id="f7641-225">ASP.NET 4 introduces a new compression option for both kinds of out-of-process session-state providers.</span></span> <span data-ttu-id="f7641-226">ときに、 *compressionEnabled*に次の例に示すように構成オプションが設定されている*true*ASP.NET は圧縮 (および圧縮解除) .NETFrameworkを使用してシリアル化されたセッションの状態*System.IO.Compression.GZipStream*クラス。</span><span class="sxs-lookup"><span data-stu-id="f7641-226">When the *compressionEnabled* configuration option shown in the following example is set to *true*, ASP.NET will compress (and decompress) serialized session state by using the .NET Framework *System.IO.Compression.GZipStream* class.</span></span>

[!code-xml[Main](overview/samples/sample9.xml)]

<span data-ttu-id="f7641-227">単純に新しい属性の追加、`Web.config`ファイル、Web サーバーの予備の CPU サイクルを持つアプリケーションは、シリアル化されたセッション状態データのサイズが大幅に削減を実現できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-227">With the simple addition of the new attribute to the `Web.config` file, applications with spare CPU cycles on Web servers can realize substantial reductions in the size of serialized session-state data.</span></span>

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a><span data-ttu-id="f7641-228">許容される Url の範囲を拡大</span><span class="sxs-lookup"><span data-stu-id="f7641-228">Expanding the Range of Allowable URLs</span></span>

<span data-ttu-id="f7641-229">ASP.NET 4 では、アプリケーションの Url のサイズを拡大するための新しいオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="f7641-229">ASP.NET 4 introduces new options for expanding the size of application URLs.</span></span> <span data-ttu-id="f7641-230">以前のバージョンの ASP.NET では、260 文字まで、NTFS ファイル パスの上限に基づく URL パスの長さを制限します。</span><span class="sxs-lookup"><span data-stu-id="f7641-230">Previous versions of ASP.NET constrained URL path lengths to 260 characters, based on the NTFS file-path limit.</span></span> <span data-ttu-id="f7641-231">ASP.NET 4 である (増減) この制限として、アプリケーションの適切なオプションを使用して 2 つの新しい*httpRuntime*構成属性。</span><span class="sxs-lookup"><span data-stu-id="f7641-231">In ASP.NET 4, you have the option to increase (or decrease) this limit as appropriate for your applications, using two new *httpRuntime* configuration attributes.</span></span> <span data-ttu-id="f7641-232">次の例では、これらの新しい属性を示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-232">The following example shows these new attributes.</span></span>

[!code-xml[Main](overview/samples/sample10.xml)]

<span data-ttu-id="f7641-233">長いまたは短いパス (プロトコル、サーバー名、およびクエリ文字列が含まれていない URL の一部) を許可するのには、変更、 *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* 属性。</span><span class="sxs-lookup"><span data-stu-id="f7641-233">To allow longer or shorter paths (the portion of the URL that does not include protocol, server name, and query string), modify the *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* attribute.</span></span> <span data-ttu-id="f7641-234">長いまたは短いクエリ文字列を許可するには、値を変更、 *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* 属性。</span><span class="sxs-lookup"><span data-stu-id="f7641-234">To allow longer or shorter query strings, modify the value of the *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* attribute.</span></span>

<span data-ttu-id="f7641-235">ASP.NET 4 では、URL の文字のチェックで使用される文字を構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="f7641-235">ASP.NET 4 also enables you to configure the characters that are used by the URL character check.</span></span> <span data-ttu-id="f7641-236">ASP.NET では、URL のパス部分では無効な文字が検出されると、要求は拒否され、HTTP 400 エラーを発行します。</span><span class="sxs-lookup"><span data-stu-id="f7641-236">When ASP.NET finds an invalid character in the path portion of a URL, it rejects the request and issues an HTTP 400 error.</span></span> <span data-ttu-id="f7641-237">ASP.NET の以前のバージョンでは、URL の文字のチェックは、文字の固定セットに制限されていました。</span><span class="sxs-lookup"><span data-stu-id="f7641-237">In previous versions of ASP.NET, the URL character checks were limited to a fixed set of characters.</span></span> <span data-ttu-id="f7641-238">ASP.NET 4 では、新しい有効な文字のセットをカスタマイズできます*requestPathInvalidChars*の属性、 *httpRuntime*構成要素を次の例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="f7641-238">In ASP.NET 4, you can customize the set of valid characters using the new *requestPathInvalidChars* attribute of the *httpRuntime* configuration element, as shown in the following example:</span></span>

[!code-xml[Main](overview/samples/sample11.xml)]

<span data-ttu-id="f7641-239">既定で、 <em>requestPathInvalidChars</em>属性が無効として 8 文字を定義します。</span><span class="sxs-lookup"><span data-stu-id="f7641-239">By default, the <em>requestPathInvalidChars</em> attribute defines eight characters as invalid.</span></span> <span data-ttu-id="f7641-240">(に割り当てられている文字列で<em>requestPathInvalidChars</em>既定<em>、</em>より小さい (&lt;)、大 (&gt;)、アンパサンドと (&amp;) 文字は、エンコードされたため、`Web.config`ファイルは、XML ファイルです)。無効な文字のセットは、必要に応じてカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="f7641-240">(In the string that is assigned to <em>requestPathInvalidChars</em> by default<em>,</em>the less than (&lt;), greater than (&gt;), and ampersand (&amp;) characters are encoded, because the `Web.config` file is an XML file.) You can customize the set of invalid characters as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="f7641-241">ASP.NET 4 は、無効な URL 文字は、IETF の RFC 2396 で定義されているため、常に 0x00 ~ 0x1F の ASCII の範囲内の文字が含まれている URL パスを拒否するメモ ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt))。</span><span class="sxs-lookup"><span data-stu-id="f7641-241">Note ASP.NET 4 always rejects URL paths that contain characters in the ASCII range of 0x00 to 0x1F, because those are invalid URL characters as defined in RFC 2396 of the IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)).</span></span> <span data-ttu-id="f7641-242">IIS 6 を実行するバージョンの Windows Server または以降では、http.sys プロトコルのデバイス ドライバーに自動的に拒否 Url でこれらの文字。</span><span class="sxs-lookup"><span data-stu-id="f7641-242">On versions of Windows Server that run IIS 6 or higher, the http.sys protocol device driver automatically rejects URLs with these characters.</span></span>


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a><span data-ttu-id="f7641-243">拡張可能な要求の検証</span><span class="sxs-lookup"><span data-stu-id="f7641-243">Extensible Request Validation</span></span>

<span data-ttu-id="f7641-244">ASP.NET 要求の検証は、クロス サイト スクリプティング (XSS) 攻撃によく使用される文字列の入力方向の HTTP 要求データを検索します。</span><span class="sxs-lookup"><span data-stu-id="f7641-244">ASP.NET request validation searches incoming HTTP request data for strings that are commonly used in cross-site scripting (XSS) attacks.</span></span> <span data-ttu-id="f7641-245">XSS の潜在的な文字列が見つかった場合、要求の検証は疑いのある文字列のフラグを設定し、エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="f7641-245">If potential XSS strings are found, request validation flags the suspect string and returns an error.</span></span> <span data-ttu-id="f7641-246">組み込みの要求の検証は、XSS 攻撃で使用される最も一般的な文字列を見つけた場合にのみ、エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="f7641-246">The built-in request validation returns an error only when it finds the most common strings used in XSS attacks.</span></span> <span data-ttu-id="f7641-247">偽陽性が多すぎる、XSS 検証をより積極的な実行する前の試行が発生しました。</span><span class="sxs-lookup"><span data-stu-id="f7641-247">Previous attempts to make the XSS validation more aggressive resulted in too many false positives.</span></span> <span data-ttu-id="f7641-248">ただし、お客様は、XSS を意図的に緩和する逆にすることがありますかより積極的な要求の検証は、特定のページまたは特定の種類の要求を確認します。 する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-248">However, customers might want request validation that is more aggressive, or conversely might want to intentionally relax XSS checks for specific pages or for specific types of requests.</span></span>

<span data-ttu-id="f7641-249">ASP.NET 4 の要求の検証機能は拡張可能な要求のカスタム検証ロジックを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f7641-249">In ASP.NET 4, the request validation feature has been made extensible so that you can use custom request-validation logic.</span></span> <span data-ttu-id="f7641-250">要求の検証を拡張する新しいから派生したクラスを作成する*System.Web.Util.RequestValidator*型とするアプリケーションの構成 (で、 *httpRuntime* の`Web.config`ファイル) にカスタムの型を使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-250">To extend request validation, you create a class that derives from the new *System.Web.Util.RequestValidator* type, and you configure the application (in the *httpRuntime* section of the `Web.config` file) to use the custom type.</span></span> <span data-ttu-id="f7641-251">次の例では、要求のカスタム検証クラスを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-251">The following example shows how to configure a custom request-validation class:</span></span>

[!code-xml[Main](overview/samples/sample12.xml)]

<span data-ttu-id="f7641-252">新しい*requestValidationType*属性には、カスタム要求の検証を提供するクラスを指定する標準 .NET Framework 型識別子文字列が必要です。</span><span class="sxs-lookup"><span data-stu-id="f7641-252">The new *requestValidationType* attribute requires a standard .NET Framework type identifier string that specifies the class that provides custom request validation.</span></span> <span data-ttu-id="f7641-253">要求ごとには、ASP.NET は、受信 HTTP 要求データの各部分を処理するカスタム型を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f7641-253">For each request, ASP.NET invokes the custom type to process each piece of incoming HTTP request data.</span></span> <span data-ttu-id="f7641-254">受信 URL、すべての HTTP ヘッダー (クッキーとカスタム ヘッダーの両方)、およびエンティティ本体は、次の例のような要求のカスタム検証クラスの使用可能なすべての。</span><span class="sxs-lookup"><span data-stu-id="f7641-254">The incoming URL, all the HTTP headers (both cookies and custom headers), and the entity body are all available for inspection by a custom request validation class like that shown in the following example:</span></span>

[!code-csharp[Main](overview/samples/sample13.cs)]

<span data-ttu-id="f7641-255">HTTP の受信データの一部を検査しない場合、要求の検証のクラスに戻ることが ASP.NET 既定の要求の検証呼び出すだけで実行できるように*ベース。IsValidRequestString します。*</span><span class="sxs-lookup"><span data-stu-id="f7641-255">For cases where you do not want to inspect a piece of incoming HTTP data, the request-validation class can fall back to let the ASP.NET default request validation run by simply calling *base.IsValidRequestString.*</span></span>

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a><span data-ttu-id="f7641-256">オブジェクト キャッシュとキャッシュの機能拡張オブジェクト</span><span class="sxs-lookup"><span data-stu-id="f7641-256">Object Caching and Object Caching Extensibility</span></span>

<span data-ttu-id="f7641-257">最初のリリース以降、ASP.NET の強力なメモリ内オブジェクト キャッシュが含まれる (*System.Web.Caching.Cache*)。</span><span class="sxs-lookup"><span data-stu-id="f7641-257">Since its first release, ASP.NET has included a powerful in-memory object cache (*System.Web.Caching.Cache*).</span></span> <span data-ttu-id="f7641-258">キャッシュの実装は普及非 Web アプリケーションで使用されています。</span><span class="sxs-lookup"><span data-stu-id="f7641-258">The cache implementation has been so popular that it has been used in non-Web applications.</span></span> <span data-ttu-id="f7641-259">ただしへの参照を含むように Windows フォームや WPF アプリケーションの面倒は`System.Web.dll`ASP.NET オブジェクト キャッシュを使用することができるようにするだけです。</span><span class="sxs-lookup"><span data-stu-id="f7641-259">However, it is awkward for a Windows Forms or WPF application to include a reference to `System.Web.dll` just to be able to use the ASP.NET object cache.</span></span>

<span data-ttu-id="f7641-260">キャッシュで使用できるようにすべてのアプリケーションは、.NET Framework 4 は、新しいアセンブリを新しい名前空間、一部の基本型および実装をキャッシュする具象型について説明します。</span><span class="sxs-lookup"><span data-stu-id="f7641-260">To make caching available for all applications, the .NET Framework 4 introduces a new assembly, a new namespace, some base types, and a concrete caching implementation.</span></span> <span data-ttu-id="f7641-261">新しい`System.Runtime.Caching.dll`アセンブリには、新しいキャッシュ API が含まれている、 *System.Runtime.Caching*名前空間。</span><span class="sxs-lookup"><span data-stu-id="f7641-261">The new `System.Runtime.Caching.dll` assembly contains a new caching API in the *System.Runtime.Caching* namespace.</span></span> <span data-ttu-id="f7641-262">名前空間には、クラスの 2 つのコア セットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f7641-262">The namespace contains two core sets of classes:</span></span>

- <span data-ttu-id="f7641-263">任意の種類のカスタム キャッシュ実装を構築するための基盤を提供する抽象型。</span><span class="sxs-lookup"><span data-stu-id="f7641-263">Abstract types that provide the foundation for building any type of custom cache implementation.</span></span>
- <span data-ttu-id="f7641-264">具体的なメモリ内オブジェクト キャッシュ実装 (、 *System.Runtime.Caching.MemoryCache*クラス)。</span><span class="sxs-lookup"><span data-stu-id="f7641-264">A concrete in-memory object cache implementation (the *System.Runtime.Caching.MemoryCache* class).</span></span>

<span data-ttu-id="f7641-265">新しい*MemoryCache*クラスは、ASP.NET キャッシュで密接にモデル化し、ASP.NET と多くの内部キャッシュ エンジン ロジックを共有します。</span><span class="sxs-lookup"><span data-stu-id="f7641-265">The new *MemoryCache* class is modeled closely on the ASP.NET cache, and it shares much of the internal cache engine logic with ASP.NET.</span></span> <span data-ttu-id="f7641-266">パブリックのキャッシュ Api *System.Runtime.Caching* 、ASP.NET を使用している場合に、カスタムのキャッシュの開発をサポートするために更新されました*キャッシュ*オブジェクトでなじみのある概念を検索は、新しい Api です。</span><span class="sxs-lookup"><span data-stu-id="f7641-266">Although the public caching APIs in *System.Runtime.Caching* have been updated to support development of custom caches, if you have used the ASP.NET *Cache* object, you will find familiar concepts in the new APIs.</span></span>

<span data-ttu-id="f7641-267">新しいの詳細については*MemoryCache*クラスと基本 Api をサポートしているドキュメント全体を必要となります。</span><span class="sxs-lookup"><span data-stu-id="f7641-267">An in-depth discussion of the new *MemoryCache* class and supporting base APIs would require an entire document.</span></span> <span data-ttu-id="f7641-268">ただし、次の例では、使用する、新しいキャッシュ API のしくみの概要を把握できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-268">However, the following example gives you an idea of how the new cache API works.</span></span> <span data-ttu-id="f7641-269">依存することがなく、Windows フォーム アプリケーションの上の例が書き込まれた`System.Web.dll`します。</span><span class="sxs-lookup"><span data-stu-id="f7641-269">The example was written for a Windows Forms application, without any dependency on `System.Web.dll`.</span></span>

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a><span data-ttu-id="f7641-270">拡張可能な HTML、URL、および HTTP ヘッダーのエンコード</span><span class="sxs-lookup"><span data-stu-id="f7641-270">Extensible HTML, URL, and HTTP Header Encoding</span></span>

<span data-ttu-id="f7641-271">ASP.NET 4 では、次の一般的なテキスト エンコーディング タスクのカスタムのエンコーディング ルーチンを作成できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-271">In ASP.NET 4, you can create custom encoding routines for the following common text-encoding tasks:</span></span>

- <span data-ttu-id="f7641-272">HTML エンコードします。</span><span class="sxs-lookup"><span data-stu-id="f7641-272">HTML encoding.</span></span>
- <span data-ttu-id="f7641-273">URL エンコードします。</span><span class="sxs-lookup"><span data-stu-id="f7641-273">URL encoding.</span></span>
- <span data-ttu-id="f7641-274">HTML 属性エンコード。</span><span class="sxs-lookup"><span data-stu-id="f7641-274">HTML attribute encoding.</span></span>
- <span data-ttu-id="f7641-275">送信 HTTP ヘッダーをエンコードします。</span><span class="sxs-lookup"><span data-stu-id="f7641-275">Encoding outbound HTTP headers.</span></span>

<span data-ttu-id="f7641-276">カスタム エンコーダーを作成するには、新しいから派生することによって*System.Web.Util.HttpEncoder*にカスタムの型を使用する型と ASP.NET を構成し、 *httpRuntime*のセクション、`Web.config`ファイルとして次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-276">You can create a custom encoder by deriving from the new *System.Web.Util.HttpEncoder* type and then configuring ASP.NET to use the custom type in the *httpRuntime* section of the `Web.config` file, as shown in the following example:</span></span>

[!code-xml[Main](overview/samples/sample15.xml)]

<span data-ttu-id="f7641-277">ASP.NET が自動的にエンコードのメソッドを公開するたびに、カスタム エンコード実装を呼び出すカスタム エンコーダーを構成した後、 *System.Web.HttpUtility*または*System.Web.HttpServerUtility*クラスと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="f7641-277">After a custom encoder has been configured, ASP.NET automatically calls the custom encoding implementation whenever public encoding methods of the *System.Web.HttpUtility* or *System.Web.HttpServerUtility* classes are called.</span></span> <span data-ttu-id="f7641-278">これには、Web 開発チームの 1 つの部分を積極的な文字エンコーディング、Web の開発チームの残りの部分がパブリックの ASP.NET Api のエンコードを使用して引き続きを実装するカスタム エンコーダーを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-278">This lets one part of a Web development team create a custom encoder that implements aggressive character encoding, while the rest of the Web development team continues to use the public ASP.NET encoding APIs.</span></span> <span data-ttu-id="f7641-279">カスタム エンコーダーを一元的に構成することによって、 *httpRuntime*要素、カスタム エンコーダーでエンコード Api パブリック ASP.NET からのすべてのテキスト エンコーディングの呼び出しがルーティングされる保証されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-279">By centrally configuring a custom encoder in the *httpRuntime* element, you are guaranteed that all text-encoding calls from the public ASP.NET encoding APIs are routed through the custom encoder.</span></span>

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a><span data-ttu-id="f7641-280">1 つのワーカー プロセス内の個々 のアプリケーションのパフォーマンスの監視</span><span class="sxs-lookup"><span data-stu-id="f7641-280">Performance Monitoring for Individual Applications in a Single Worker Process</span></span>

<span data-ttu-id="f7641-281">1 台のサーバーをホストできる Web サイトの数を増やすためには、多くのホスト側は、1 つのワーカー プロセスで複数の ASP.NET アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="f7641-281">In order to increase the number of Web sites that can be hosted on a single server, many hosters run multiple ASP.NET applications in a single worker process.</span></span> <span data-ttu-id="f7641-282">ただし、複数のアプリケーションは、1 つの共有ワーカー プロセスを使用すると、問題が発生している個々 のアプリケーションを識別するためにサーバー管理者のため困難ですが。</span><span class="sxs-lookup"><span data-stu-id="f7641-282">However, if multiple applications use a single shared worker process, it is difficult for server administrators to identify an individual application that is experiencing problems.</span></span>

<span data-ttu-id="f7641-283">ASP.NET 4 では、CLR で導入された新しいリソースの監視の機能を活用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-283">ASP.NET 4 leverages new resource-monitoring functionality introduced by the CLR.</span></span> <span data-ttu-id="f7641-284">この機能を有効にするには、次の XML 構成スニペットを追加することができます、`aspnet.config`構成ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-284">To enable this functionality, you can add the following XML configuration snippet to the `aspnet.config` configuration file.</span></span>

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> <span data-ttu-id="f7641-285">注、`aspnet.config`ファイルは、.NET Framework がインストールされているディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="f7641-285">Note The `aspnet.config` file is in the directory where the .NET Framework is installed.</span></span> <span data-ttu-id="f7641-286">`Web.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-286">It is not the `Web.config` file.</span></span>


<span data-ttu-id="f7641-287">ときに、 *appDomainResourceMonitoring*機能が有効になって、2 つの新しいパフォーマンス カウンターは、[ASP.NET アプリケーション] のパフォーマンス カテゴリで利用可能な: *% プロセッサ時間のマネージ*と*マネージ メモリの使用*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-287">When the *appDomainResourceMonitoring* feature has been enabled, two new performance counters are available in the "ASP.NET Applications" performance category: *% Managed Processor Time* and *Managed Memory Used*.</span></span> <span data-ttu-id="f7641-288">これらのパフォーマンス カウンターの両方は、推定の CPU 時間と個別の ASP.NET アプリケーションのマネージ メモリの使用率を追跡するために新しい CLR アプリケーション ドメインのリソースの管理機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-288">Both of these performance counters use the new CLR application-domain resource management feature to track estimated CPU time and managed memory utilization of individual ASP.NET applications.</span></span> <span data-ttu-id="f7641-289">その結果、ASP.NET 4 で管理者ようになりましたがある 1 つのワーカー プロセスで実行されている個々 のアプリケーションのリソースの消費をより詳細に表示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-289">As a result, with ASP.NET 4, administrators now have a more granular view into the resource consumption of individual applications running in a single worker process.</span></span>

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a><span data-ttu-id="f7641-290">マルチ ターゲット</span><span class="sxs-lookup"><span data-stu-id="f7641-290">Multi-Targeting</span></span>

<span data-ttu-id="f7641-291">.NET Framework の特定のバージョンを対象とするアプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-291">You can create an application that targets a specific version of the .NET Framework.</span></span> <span data-ttu-id="f7641-292">ASP.NET 4 では、新しい属性で、*コンパイル*の要素、`Web.config`ファイルでは、.NET Framework 4 以降をターゲットすることができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-292">In ASP.NET 4, a new attribute in the *compilation* element of the `Web.config` file lets you target the .NET Framework 4 and later.</span></span> <span data-ttu-id="f7641-293">明示的に、.NET Framework 4 を対象とする場合とで省略可能な要素を含める場合は、`Web.config`ファイルのエントリなど*system.codedom*、これらの要素は、.NET Framework 4 に対して適切である必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-293">If you explicitly target the .NET Framework 4, and if you include optional elements in the `Web.config` file such as the entries for *system.codedom*, these elements must be correct for the .NET Framework 4.</span></span> <span data-ttu-id="f7641-294">(.NET Framework 4 を明示的にターゲットはない場合、ターゲット フレームワークは内のエントリがないことから推論されます、`Web.config`ファイルです)。</span><span class="sxs-lookup"><span data-stu-id="f7641-294">(If you do not explicitly target the .NET Framework 4, the target framework is inferred from the lack of an entry in the `Web.config` file.)</span></span>

<span data-ttu-id="f7641-295">次の例では、使用、 *targetFramework*属性、*コンパイル*の要素、`Web.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-295">The following example shows the use of the *targetFramework* attribute in the *compilation* element of the `Web.config` file.</span></span>

[!code-xml[Main](overview/samples/sample17.xml)]

<span data-ttu-id="f7641-296">対象となる .NET Framework の特定のバージョンについては、次に注意してください。</span><span class="sxs-lookup"><span data-stu-id="f7641-296">Note the following about targeting a specific version of the .NET Framework:</span></span>

- <span data-ttu-id="f7641-297">.NET Framework 4 アプリケーション プールでは、ASP.NET のビルド システムが想定、.NET Framework 4 をターゲットとして場合、`Web.config`ファイルは含まれません、 *targetFramework*属性場合は、`Web.config`ファイルが見つかりません。</span><span class="sxs-lookup"><span data-stu-id="f7641-297">In a .NET Framework 4 application pool, the ASP.NET build system assumes the .NET Framework 4 as a target if the `Web.config` file does not include the *targetFramework* attribute or if the `Web.config` file is missing.</span></span> <span data-ttu-id="f7641-298">(、.NET Framework 4 で実行されるようにアプリケーションをコーディングに変更する必要があります)。</span><span class="sxs-lookup"><span data-stu-id="f7641-298">(You might have to make coding changes to your application to make it run under the .NET Framework 4.)</span></span>
- <span data-ttu-id="f7641-299">必要な場合、 *targetFramework*属性、場合に、 *system.codeDom*で要素が定義されている、`Web.config`ファイルでは、このファイルは、.NET Framework 4 の適切なエントリを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-299">If you do include the *targetFramework* attribute, and if the *system.codeDom* element is defined in the `Web.config` file, this file must contain the correct entries for the .NET Framework 4.</span></span>
- <span data-ttu-id="f7641-300">使用する場合、 *aspnet\_コンパイラ*(ビルド環境など)、アプリケーションをプリコンパイルするコマンドでの正しいバージョンを使用する必要があります、 *aspnet\_コンパイラ*ターゲット フレームワークのコマンド。</span><span class="sxs-lookup"><span data-stu-id="f7641-300">If you are using the *aspnet\_compiler* command to precompile your application (such as in a build environment), you must use the correct version of the *aspnet\_compiler* command for the target framework.</span></span> <span data-ttu-id="f7641-301">.NET Framework 3.5 およびそれ以前のバージョン用にコンパイルすると、.NET Framework 2.0 (%windir%\microsoft.net\framework\v2.0.50727) の同梱されているコンパイラを使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-301">Use the compiler that shipped with the .NET Framework 2.0 (%WINDIR%\Microsoft.NET\Framework\v2.0.50727) to compile for the .NET Framework 3.5 and earlier versions.</span></span> <span data-ttu-id="f7641-302">.NET Framework 4 でに付属するコンパイラを使用すると、そのフレームワークを使用するか、以降のバージョンを使用して作成されたアプリケーションをコンパイルできます。</span><span class="sxs-lookup"><span data-stu-id="f7641-302">Use the compiler that ships with the .NET Framework 4 to compile applications created using that framework or using later versions.</span></span>
- <span data-ttu-id="f7641-303">実行時に、コンパイラは、コンピューターにインストールされている最新のフレームワーク アセンブリを使用 (とそのため、GAC 内)。</span><span class="sxs-lookup"><span data-stu-id="f7641-303">At run time, the compiler uses the latest framework assemblies that are installed on the computer (and therefore in the GAC).</span></span> <span data-ttu-id="f7641-304">フレームワークに更新プログラムを後で行われた場合 (仮想的なバージョン 4.1 がインストールされてなど)、場合でも、framework の新しいバージョンで機能を使用することができます、 *targetFramework*属性は、古いバージョンを対象と(4.0 など)。</span><span class="sxs-lookup"><span data-stu-id="f7641-304">If an update is made later to the framework (for example, a hypothetical version 4.1 is installed), you will be able to use features in the newer version of the framework even though the *targetFramework* attribute targets a lower version (such as 4.0).</span></span> <span data-ttu-id="f7641-305">(ただし、デザイン時に Visual Studio 2010 または使用すると、 *aspnet\_コンパイラ*コマンド、フレームワークの最新の機能を使用すると、コンパイラ エラーが発生)。</span><span class="sxs-lookup"><span data-stu-id="f7641-305">(However, at design time in Visual Studio 2010 or when you use the *aspnet\_compiler* command, using newer features of the framework will cause compiler errors).</span></span>

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a><span data-ttu-id="f7641-306">Ajax</span><span class="sxs-lookup"><span data-stu-id="f7641-306">Ajax</span></span>

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a><span data-ttu-id="f7641-307">含まれる Web フォームと MVC の jQuery</span><span class="sxs-lookup"><span data-stu-id="f7641-307">jQuery Included with Web Forms and MVC</span></span>

<span data-ttu-id="f7641-308">Web フォームと MVC の Visual Studio テンプレートには、オープン ソースの jQuery ライブラリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f7641-308">The Visual Studio templates for both Web Forms and MVC include the open-source jQuery library.</span></span> <span data-ttu-id="f7641-309">新しい web サイトまたはプロジェクトを作成するときに、次の 3 つのファイルを含む Scripts フォルダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-309">When you create a new website or project, a Scripts folder containing the following 3 files is created:</span></span>

- <span data-ttu-id="f7641-310">jQuery-1.4.1.js –、人間が判読できる、unminified、jQuery ライブラリのバージョン。</span><span class="sxs-lookup"><span data-stu-id="f7641-310">jQuery-1.4.1.js – The human-readable, unminified version of the jQuery library.</span></span>
- <span data-ttu-id="f7641-311">jQuery-14.1.min.js – jQuery ライブラリの縮小されたバージョン。</span><span class="sxs-lookup"><span data-stu-id="f7641-311">jQuery-14.1.min.js – The minified version of the jQuery library.</span></span>
- <span data-ttu-id="f7641-312">jQuery の 1.4.1-vsdoc.js – jQuery ライブラリの Intellisense ドキュメント ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-312">jQuery-1.4.1-vsdoc.js – The Intellisense documentation file for the jQuery library.</span></span>

<span data-ttu-id="f7641-313">アプリケーションの開発中には、jQuery の unminified バージョンを含めます。</span><span class="sxs-lookup"><span data-stu-id="f7641-313">Include the unminified version of jQuery while developing an application.</span></span> <span data-ttu-id="f7641-314">実稼働アプリケーションの jQuery の縮小されたバージョンが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f7641-314">Include the minified version of jQuery for production applications.</span></span>

<span data-ttu-id="f7641-315">たとえば、次の Web フォーム ページは、jQuery を使用して、黄色のフォーカスがあるときに、ASP.NET の TextBox コントロールの背景色を変更する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="f7641-315">For example, the following Web Forms page illustrates how you can use jQuery to change the background color of ASP.NET TextBox controls to yellow when they have focus.</span></span>

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a><span data-ttu-id="f7641-316">Content Delivery Network のサポート</span><span class="sxs-lookup"><span data-stu-id="f7641-316">Content Delivery Network Support</span></span>

<span data-ttu-id="f7641-317">Microsoft Ajax Content Delivery Network (CDN) では、Web アプリケーションに ASP.NET Ajax および jQuery スクリプトを簡単に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-317">The Microsoft Ajax Content Delivery Network (CDN) enables you to easily add ASP.NET Ajax and jQuery scripts to your Web applications.</span></span> <span data-ttu-id="f7641-318">JQuery ライブラリを追加するだけで使用を開始するなど、`<script>`ページを Ajax.microsoft.com を指すようにタグ。</span><span class="sxs-lookup"><span data-stu-id="f7641-318">For example, you can start using the jQuery library simply by adding a `<script>` tag to your page that points to Ajax.microsoft.com like this:</span></span>

[!code-html[Main](overview/samples/sample19.html)]

<span data-ttu-id="f7641-319">Microsoft Ajax CDN を利用して、Ajax アプリケーションのパフォーマンスを大幅に向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-319">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="f7641-320">Microsoft Ajax CDN の内容は、世界各地に配置されたサーバーにキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="f7641-320">The contents of the Microsoft Ajax CDN are cached on servers located around the world.</span></span> <span data-ttu-id="f7641-321">さらに、Microsoft Ajax CDN は、別のドメインに配置されている Web サイトのキャッシュされた JavaScript ファイルを再利用するブラウザーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-321">In addition, the Microsoft Ajax CDN enables browsers to reuse cached JavaScript files for Web sites that are located in different domains.</span></span>

<span data-ttu-id="f7641-322">Microsoft Ajax Content Delivery Network は、Secure Sockets Layer を使用して web ページを使用する必要がある場合に、SSL (HTTPS) をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="f7641-322">The Microsoft Ajax Content Delivery Network supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="f7641-323">CDN が利用できない場合は、フォールバックを実装します。</span><span class="sxs-lookup"><span data-stu-id="f7641-323">Implement a fallback when the CDN is unavailable.</span></span> <span data-ttu-id="f7641-324">フォールバックをテストします。</span><span class="sxs-lookup"><span data-stu-id="f7641-324">Test the fallback.</span></span>

<span data-ttu-id="f7641-325">Microsoft Ajax CDN の概要の詳細については、次の web サイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f7641-325">To learn more about the Microsoft Ajax CDN, visit the following website:</span></span>

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

<span data-ttu-id="f7641-326">ASP.NET の scriptmanager コントロールは、Microsoft Ajax CDN をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f7641-326">The ASP.NET ScriptManager supports the Microsoft Ajax CDN.</span></span> <span data-ttu-id="f7641-327">1 つのプロパティの設定、EnableCdn プロパティだけでは、CDN からすべての ASP.NET フレームワークの JavaScript ファイルを取得できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-327">Simply by setting one property, the EnableCdn property, you can retrieve all ASP.NET framework JavaScript files from the CDN:</span></span>

[!code-aspx[Main](overview/samples/sample20.aspx)]

<span data-ttu-id="f7641-328">EnableCdn プロパティを true に設定した後、ASP.NET フレームワークがそのすべての ASP.NET フレームワークの JavaScript ファイルを検証し、UpdatePanel を使用するすべての JavaScript ファイルを含む CDN から取得されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-328">After you set the EnableCdn property to the value true, the ASP.NET framework will retrieve all ASP.NET framework JavaScript files from the CDN including all JavaScript files used for validation and the UpdatePanel.</span></span> <span data-ttu-id="f7641-329">この 1 つのプロパティを設定すると、web アプリケーションのパフォーマンスに大きな影響を及ぼす場合があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-329">Setting this one property can have a dramatic impact on the performance of your web application.</span></span>

<span data-ttu-id="f7641-330">WebResource 属性を使用して、独自の JavaScript ファイルの CDN パスを設定できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-330">You can set the CDN path for your own JavaScript files by using the WebResource attribute.</span></span> <span data-ttu-id="f7641-331">新しい CdnPath プロパティは、EnableCdn プロパティ値の true に設定するときに使用する CDN へのパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-331">The new CdnPath property specifies the path to the CDN used when you set the EnableCdn property to the value true:</span></span>

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a><span data-ttu-id="f7641-332">Scriptmanager コントロールの明示的なスクリプト</span><span class="sxs-lookup"><span data-stu-id="f7641-332">ScriptManager Explicit Scripts</span></span>

<span data-ttu-id="f7641-333">以前は、ASP.NET ScriptManger を使用している場合する必要がある全体のモノリシック ASP.NET Ajax ライブラリを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="f7641-333">In the past, if you used the ASP.NET ScriptManger then you were required to load the entire monolithic ASP.NET Ajax Library.</span></span> <span data-ttu-id="f7641-334">新しい ScriptManager.AjaxFrameworkMode プロパティを利用して、ASP.NET Ajax ライブラリのコンポーネントが読み込まれる正確に制御し、必要な ASP.NET Ajax ライブラリのコンポーネントのみをロードできます。</span><span class="sxs-lookup"><span data-stu-id="f7641-334">By taking advantage of the new ScriptManager.AjaxFrameworkMode property, you can control exactly which components of the ASP.NET Ajax Library are loaded and load only the components of the ASP.NET Ajax Library that you need.</span></span>

<span data-ttu-id="f7641-335">ScriptManager.AjaxFrameworkMode プロパティは、次の値に設定できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-335">The ScriptManager.AjaxFrameworkMode property can be set to the following values:</span></span>

- <span data-ttu-id="f7641-336">-有効にするには、ScriptManager コントロールに自動的に結合したスクリプト ファイルの各コア フレームワーク スクリプト (従来の動作) である MicrosoftAjax.js スクリプト ファイルが含まれているを指定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-336">Enabled -- Specifies that the ScriptManager control automatically includes the MicrosoftAjax.js script file, which is a combined script file of every core framework script (legacy behavior).</span></span>
- <span data-ttu-id="f7641-337">-無効にするには、Microsoft Ajax スクリプトのすべての機能が無効になっていると、ScriptManager コントロールは、スクリプトを自動的に参照していないことを指定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-337">Disabled -- Specifies that all Microsoft Ajax script features are disabled and that the ScriptManager control does not reference any scripts automatically.</span></span>
- <span data-ttu-id="f7641-338">--明示的なページに必要な個別のフレームワーク コア スクリプト ファイルへのスクリプト参照を明示的に含めるが、各スクリプト ファイルに必要な依存関係への参照を含めることを指定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-338">Explicit -- Specifies that you will explicitly include script references to individual framework core script file that your page requires, and that you will include references to the dependencies that each script file requires.</span></span>

<span data-ttu-id="f7641-339">たとえば、AjaxFrameworkMode プロパティ値を明示的に設定する場合は、必要な特定の ASP.NET Ajax コンポーネントのスクリプトを指定できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-339">For example, if you set the AjaxFrameworkMode property to the value Explicit then you can specify the particular ASP.NET Ajax component scripts that you need:</span></span>

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a><span data-ttu-id="f7641-340">Web フォーム</span><span class="sxs-lookup"><span data-stu-id="f7641-340">Web Forms</span></span>

<span data-ttu-id="f7641-341">Web フォームは、ASP.NET 1.0 のリリース以降では、ASP.NET core の機能をしました。</span><span class="sxs-lookup"><span data-stu-id="f7641-341">Web Forms has been a core feature in ASP.NET since the release of ASP.NET 1.0.</span></span> <span data-ttu-id="f7641-342">ASP.NET 4 で、次のようには、この領域で多くの機能強化がされています。</span><span class="sxs-lookup"><span data-stu-id="f7641-342">Many enhancements have been in this area for ASP.NET 4, including the following:</span></span>

- <span data-ttu-id="f7641-343">設定する機能*メタ*タグ。</span><span class="sxs-lookup"><span data-stu-id="f7641-343">The ability to set *meta* tags.</span></span>
- <span data-ttu-id="f7641-344">ビュー ステートを細かく制御します。</span><span class="sxs-lookup"><span data-stu-id="f7641-344">More control over view state.</span></span>
- <span data-ttu-id="f7641-345">ブラウザーの機能を使用する簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="f7641-345">Easier ways to work with browser capabilities.</span></span>
- <span data-ttu-id="f7641-346">ASP.NET が Web フォームによるルーティングを使用するためのサポート。</span><span class="sxs-lookup"><span data-stu-id="f7641-346">Support for using ASP.NET routing with Web Forms.</span></span>
- <span data-ttu-id="f7641-347">生成された Id をより詳細に制御します。</span><span class="sxs-lookup"><span data-stu-id="f7641-347">More control over generated IDs.</span></span>
- <span data-ttu-id="f7641-348">データ コントロールで選択された行を保持できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f7641-348">The ability to persist selected rows in data controls.</span></span>
- <span data-ttu-id="f7641-349">さらにレンダリングされる HTML の制御、 *FormView*と*ListView*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-349">More control over rendered HTML in the *FormView* and *ListView* controls.</span></span>
- <span data-ttu-id="f7641-350">データ ソース コントロールのフィルター処理のサポート。</span><span class="sxs-lookup"><span data-stu-id="f7641-350">Filtering support for data source controls.</span></span>

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a><span data-ttu-id="f7641-351">Page.MetaKeywords Page.MetaDescription プロパティとメタ タグを設定</span><span class="sxs-lookup"><span data-stu-id="f7641-351">Setting Meta Tags with the Page.MetaKeywords and Page.MetaDescription Properties</span></span>

<span data-ttu-id="f7641-352">ASP.NET 4 で追加する 2 つのプロパティ、*ページ*クラス、 *MetaKeywords*と*MetaDescription*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-352">ASP.NET 4 adds two properties to the *Page* class, *MetaKeywords* and *MetaDescription*.</span></span> <span data-ttu-id="f7641-353">これら 2 つのプロパティを表す対応する*メタ*ページで、次の例に示すようにタグを付けます。</span><span class="sxs-lookup"><span data-stu-id="f7641-353">These two properties represent corresponding *meta* tags in your page, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample23.aspx)]

<span data-ttu-id="f7641-354">これら 2 つのプロパティは、同じ動作方法、ページの*タイトル*プロパティには。</span><span class="sxs-lookup"><span data-stu-id="f7641-354">These two properties work the same way that the page's *Title* property does.</span></span> <span data-ttu-id="f7641-355">これらには、これらの規則に従ってください。</span><span class="sxs-lookup"><span data-stu-id="f7641-355">They follow these rules:</span></span>

1. <span data-ttu-id="f7641-356">ある場合ありません*メタ*でタグ付け、*ヘッド*プロパティ名と一致する要素 (名前は、= の"keywords" *Page.MetaKeywords*と名前の"description"を=*Page.MetaDescription*、つまり、これらのプロパティが設定されていない)、*メタ*タグは、表示される場合は、ページに追加されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-356">If there are no *meta* tags in the *head* element that match the property names (that is, name="keywords" for *Page.MetaKeywords* and name="description" for *Page.MetaDescription*, meaning that these properties have not been set), the *meta* tags will be added to the page when it is rendered.</span></span>
2. <span data-ttu-id="f7641-357">既にある場合*メタ*get および set メソッドの既存のタグの内容としてにこれらの名前を持つタグは、これらのプロパティの動作します。</span><span class="sxs-lookup"><span data-stu-id="f7641-357">If there are already *meta* tags with these names, these properties act as get and set methods for the contents of the existing tags.</span></span>

<span data-ttu-id="f7641-358">これらのプロパティを設定するには、データベースまたはその他のソースからのコンテンツを入手することができ、対象を記述するには、動的にタグを設定することができます、実行時に特定のページです。</span><span class="sxs-lookup"><span data-stu-id="f7641-358">You can set these properties at run time, which lets you get the content from a database or other source, and which lets you set the tags dynamically to describe what a particular page is for.</span></span>

<span data-ttu-id="f7641-359">設定することも、*キーワード*と*説明*プロパティで、 *@ Page*次の例のように、Web フォーム ページのマークアップの上部にあるディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="f7641-359">You can also set the *Keywords* and *Description* properties in the *@ Page* directive at the top of the Web Forms page markup, as in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample24.aspx)]

<span data-ttu-id="f7641-360">これよりも優先されます、*メタ*タグ ページで既に宣言されている内容 (ある場合)。</span><span class="sxs-lookup"><span data-stu-id="f7641-360">This will override the *meta* tag contents (if any) already declared in the page.</span></span>

<span data-ttu-id="f7641-361">説明の内容*メタ*タグは Google でのプレビューを一覧表示する検索を向上させるために使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-361">The contents of the description *meta* tag are used for improving search listing previews in Google.</span></span> <span data-ttu-id="f7641-362">(詳細については、次を参照してください[メタ説明の改訂にスニペットを向上させる](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html)Google web マスターの中央のブログです。)。Google、Windows Live Search では、任意のキーワードの内容は使用しないで、他の検索エンジンの可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-362">(For details, see [Improve snippets with a meta description makeover](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) on the Google Webmaster Central blog.) Google and Windows Live Search do not use the contents of the keywords for anything, but other search engines might.</span></span> <span data-ttu-id="f7641-363">詳細については、次を参照してください。[メタ キーワード アドバイス](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php)、検索エンジンのガイドの Web サイト。</span><span class="sxs-lookup"><span data-stu-id="f7641-363">For more information, see [Meta Keywords Advice](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) on the Search Engine Guide Web site.</span></span>

<span data-ttu-id="f7641-364">これらの新しいプロパティは、単純な機能が、これらを手動で追加するための要件から、または作成するコードの記述から保存した、*メタ*タグ。</span><span class="sxs-lookup"><span data-stu-id="f7641-364">These new properties are a simple feature, but they save you from the requirement to add these manually or from writing your own code to create the *meta* tags.</span></span>

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a><span data-ttu-id="f7641-365">個々 のコントロール ビュー ステートを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f7641-365">Enabling View State for Individual Controls</span></span>

<span data-ttu-id="f7641-366">既定では、アプリケーションに必要でない場合でも、ページ上の各コントロールがそのビュー ステートを可能性のある格納する結果と、ページのビュー ステートが有効にします。</span><span class="sxs-lookup"><span data-stu-id="f7641-366">By default, view state is enabled for the page, with the result that each control on the page potentially stores view state even if it is not required for the application.</span></span> <span data-ttu-id="f7641-367">ページが生成し、ページをクライアントに送信し、ポストバックにかかる時間の増加は、マークアップでビュー ステート データが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f7641-367">View state data is included in the markup that a page generates and increases the amount of time it takes to send a page to the client and post it back.</span></span> <span data-ttu-id="f7641-368">複数のビュー ステートを格納すると、パフォーマンスが著しく低下が生じる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-368">Storing more view state than is necessary can cause significant performance degradation.</span></span> <span data-ttu-id="f7641-369">以前のバージョンの ASP.NET では、開発者は、ページ サイズを軽減するために、個々 のコントロールのビューステートを無効には、個々 のコントロールを明示的に行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-369">In earlier versions of ASP.NET, developers could disable view state for individual controls in order to reduce page size, but had to do so explicitly for individual controls.</span></span> <span data-ttu-id="f7641-370">Web サーバー コントロールには、ASP.NET 4 で、*に ViewStateMode*プロパティ ビュー ステートを既定で無効にして、ページが必要なコントロールに対してのみ有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-370">In ASP.NET 4, Web server controls include a *ViewStateMode* property that lets you disable view state by default and then enable it only for the controls that require it in the page.</span></span>

<span data-ttu-id="f7641-371">*に ViewStateMode*プロパティには次の 3 つの値を持つ列挙体:*有効*、*無効*、および*継承*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-371">The *ViewStateMode* property takes an enumeration that has three values: *Enabled*, *Disabled*, and *Inherit*.</span></span> <span data-ttu-id="f7641-372">*有効になっている*有効に設定されているすべての子コントロールのそのコントロールの状態を表示する*継承*ことか、または何も設定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-372">*Enabled* enables view state for that control and for any child controls that are set to *Inherit* or that have nothing set.</span></span> <span data-ttu-id="f7641-373">*無効になっている*無効にしますが、状態を表示し、*継承*コントロールが使用することを指定します、*に ViewStateMode*親コントロールから設定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-373">*Disabled* disables view state, and *Inherit* specifies that the control uses the *ViewStateMode* setting from the parent control.</span></span>

<span data-ttu-id="f7641-374">次の例は、どのように*に ViewStateMode*プロパティは。</span><span class="sxs-lookup"><span data-stu-id="f7641-374">The following example shows how the *ViewStateMode* property works.</span></span> <span data-ttu-id="f7641-375">マークアップとコードを次のページのコントロールの値が含まれています、*に ViewStateMode*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="f7641-375">The markup and code for the controls in the following page includes values for the *ViewStateMode* property:</span></span>

[!code-aspx[Main](overview/samples/sample25.aspx)]

<span data-ttu-id="f7641-376">ご覧のように、コードには、PlaceHolder1 コントロールのビューステートが無効にします。</span><span class="sxs-lookup"><span data-stu-id="f7641-376">As you can see, the code disables view state for the PlaceHolder1 control.</span></span> <span data-ttu-id="f7641-377">子 label1 コントロールは、このプロパティの値を継承 (*継承*の既定値は、*に ViewStateMode*コントロールの) ビューステートしたがってを保存しないとします。</span><span class="sxs-lookup"><span data-stu-id="f7641-377">The child label1 control inherits this property value (*Inherit* is the default value for *ViewStateMode* for controls.) and therefore saves no view state.</span></span> <span data-ttu-id="f7641-378">PlaceHolder2 コントロールで*に ViewStateMode*に設定されている*有効*のため、label2 は、このプロパティを継承およびビューステートを保存します。</span><span class="sxs-lookup"><span data-stu-id="f7641-378">In the PlaceHolder2 control, *ViewStateMode* is set to *Enabled*, so label2 inherits this property and saves view state.</span></span> <span data-ttu-id="f7641-379">ページが最初に読み込まれたときに、*テキスト*両方のプロパティ*ラベル*コントロールは、文字列"[DynamicValue]"に設定されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-379">When the page is first loaded, the *Text* property of both *Label* controls is set to the string "[DynamicValue]".</span></span>

<span data-ttu-id="f7641-380">これらの設定の効果は、ページが初めて読み込まれる、ブラウザーで次の出力で表示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-380">The effect of these settings is that when the page loads the first time, the following output is displayed in the browser:</span></span>

<span data-ttu-id="f7641-381">無効になっています。 `: [DynamicValue]`</span><span class="sxs-lookup"><span data-stu-id="f7641-381">Disabled `: [DynamicValue]`</span></span>

<span data-ttu-id="f7641-382">有効になります。`[DynamicValue]`</span><span class="sxs-lookup"><span data-stu-id="f7641-382">Enabled:`[DynamicValue]`</span></span>

<span data-ttu-id="f7641-383">ポストバック後に、次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-383">After a postback, however, the following output is displayed:</span></span>

<span data-ttu-id="f7641-384">無効になっています。 `: [DeclaredValue]`</span><span class="sxs-lookup"><span data-stu-id="f7641-384">Disabled `: [DeclaredValue]`</span></span>

<span data-ttu-id="f7641-385">有効になります。`[DynamicValue]`</span><span class="sxs-lookup"><span data-stu-id="f7641-385">Enabled:`[DynamicValue]`</span></span>

<span data-ttu-id="f7641-386">Label1 コントロール (が*に ViewStateMode*値に設定されて*無効*) がコードで設定されていた値を保持していません。</span><span class="sxs-lookup"><span data-stu-id="f7641-386">The label1 control (whose *ViewStateMode* value is set to *Disabled*) has not preserved the value that it was set to in code.</span></span> <span data-ttu-id="f7641-387">ただし、label2 制御 (が*に ViewStateMode*値に設定されて*有効*) の状態を保持しています。</span><span class="sxs-lookup"><span data-stu-id="f7641-387">However, the label2 control (whose *ViewStateMode* value is set to *Enabled*) has preserved its state.</span></span>

<span data-ttu-id="f7641-388">設定することも*に ViewStateMode*で、 *@ Page*ディレクティブを次の例のように。</span><span class="sxs-lookup"><span data-stu-id="f7641-388">You can also set *ViewStateMode* in the *@ Page* directive, as in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample26.aspx)]

<span data-ttu-id="f7641-389">*ページ*クラスは単なる別のコントロールは、ページの他のすべてのコントロールの親コントロールとして機能します。</span><span class="sxs-lookup"><span data-stu-id="f7641-389">The *Page* class is just another control; it acts as the parent control for all the other controls in the page.</span></span> <span data-ttu-id="f7641-390">既定値*に ViewStateMode*は*有効*のインスタンスの*ページ*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-390">The default value of *ViewStateMode* is *Enabled* for instances of *Page*.</span></span> <span data-ttu-id="f7641-391">コントロールが既定になるため*継承*、コントロールが継承、*有効*プロパティ値を設定しない限り*に ViewStateMode*ページまたはコントロールのレベル。</span><span class="sxs-lookup"><span data-stu-id="f7641-391">Because controls default to *Inherit*, controls will inherit the *Enabled* property value unless you set *ViewStateMode* at page or control level.</span></span>

<span data-ttu-id="f7641-392">値、*に ViewStateMode*プロパティは、ビュー状態を維持する場合にのみかどうかを決定します。、 *EnableViewState*プロパティに設定されて*true*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-392">The value of the *ViewStateMode* property determines if view state is maintained only if the *EnableViewState* property is set to *true*.</span></span> <span data-ttu-id="f7641-393">場合、 *EnableViewState*プロパティに設定されて*false*、ビュー ステートは保持されない場合でも*に ViewStateMode*に設定されている*有効*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-393">If the *EnableViewState* property is set to *false*, view state will not be maintained even if *ViewStateMode* is set to *Enabled*.</span></span>

<span data-ttu-id="f7641-394">この機能は適切な使い方は*ContentPlaceHolder*設定することができます、マスター ページのコントロール*に ViewStateMode*に*無効になっている*マスターのページし、それを有効にします。個別に*ContentPlaceHolder*順番を必要とするコントロールを格納しているコントロールの状態を表示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-394">A good use for this feature is with *ContentPlaceHolder* controls in master pages, where you can set *ViewStateMode* to *Disabled* for the master page and then enable it individually for *ContentPlaceHolder* controls that in turn contain controls that require view state.</span></span>

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a><span data-ttu-id="f7641-395">ブラウザーの機能への変更</span><span class="sxs-lookup"><span data-stu-id="f7641-395">Changes to Browser Capabilities</span></span>

<span data-ttu-id="f7641-396">ASP.NET と呼ばれる機能を使用して、サイトを参照するユーザーを使用しているブラウザーの機能を決定する*ブラウザー機能*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-396">ASP.NET determines the capabilities of the browser that a user is using to browse your site by using a feature called *browser capabilities*.</span></span> <span data-ttu-id="f7641-397">ブラウザーの機能がによって表される、 *HttpBrowserCapabilities*オブジェクト (によって公開されている、 *Request.Browser*プロパティ)。</span><span class="sxs-lookup"><span data-stu-id="f7641-397">Browser capabilities are represented by the *HttpBrowserCapabilities* object (exposed by the *Request.Browser* property).</span></span> <span data-ttu-id="f7641-398">たとえば、使用することができます、 *HttpBrowserCapabilities*オブジェクトの種類と現在のブラウザーのバージョンが JavaScript の特定のバージョンをサポートするかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="f7641-398">For example, you can use the *HttpBrowserCapabilities* object to determine whether the type and version of the current browser supports a particular version of JavaScript.</span></span> <span data-ttu-id="f7641-399">または、使用することができます、 *HttpBrowserCapabilities*要求がモバイル デバイスから送られたかどうかを判断するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f7641-399">Or, you can use the *HttpBrowserCapabilities* object to determine whether the request originated from a mobile device.</span></span>

<span data-ttu-id="f7641-400">*HttpBrowserCapabilities*オブジェクトは、一連のブラウザー定義ファイルによって駆動されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-400">The *HttpBrowserCapabilities* object is driven by a set of browser definition files.</span></span> <span data-ttu-id="f7641-401">これらのファイルには、特定のブラウザーの機能に関する情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f7641-401">These files contain information about the capabilities of particular browsers.</span></span> <span data-ttu-id="f7641-402">ASP.NET 4 で最近導入されたブラウザーと Google Chrome、研究などモーション BlackBerry スマート フォンと Apple iPhone でのデバイスに関する情報を含むように、これらのブラウザー定義ファイルが更新されました。</span><span class="sxs-lookup"><span data-stu-id="f7641-402">In ASP.NET 4, these browser definition files have been updated to contain information about recently introduced browsers and devices such as Google Chrome, Research in Motion BlackBerry smartphones, and Apple iPhone.</span></span>

<span data-ttu-id="f7641-403">次の一覧は、新しいブラウザー定義ファイルを示しています。</span><span class="sxs-lookup"><span data-stu-id="f7641-403">The following list shows new browser definition files:</span></span>

- <span data-ttu-id="f7641-404">*blackberry.browser*</span><span class="sxs-lookup"><span data-stu-id="f7641-404">*blackberry.browser*</span></span>
- <span data-ttu-id="f7641-405">*chrome.browser*</span><span class="sxs-lookup"><span data-stu-id="f7641-405">*chrome.browser*</span></span>
- <span data-ttu-id="f7641-406">*Default.browser*</span><span class="sxs-lookup"><span data-stu-id="f7641-406">*Default.browser*</span></span>
- <span data-ttu-id="f7641-407">*firefox.browser*</span><span class="sxs-lookup"><span data-stu-id="f7641-407">*firefox.browser*</span></span>
- <span data-ttu-id="f7641-408">*gateway.browser*</span><span class="sxs-lookup"><span data-stu-id="f7641-408">*gateway.browser*</span></span>
- <span data-ttu-id="f7641-409">*generic.browser*</span><span class="sxs-lookup"><span data-stu-id="f7641-409">*generic.browser*</span></span>
- <span data-ttu-id="f7641-410">*ie.browser*</span><span class="sxs-lookup"><span data-stu-id="f7641-410">*ie.browser*</span></span>
- <span data-ttu-id="f7641-411">*iemobile.browser*</span><span class="sxs-lookup"><span data-stu-id="f7641-411">*iemobile.browser*</span></span>
- <span data-ttu-id="f7641-412">*iphone.browser*</span><span class="sxs-lookup"><span data-stu-id="f7641-412">*iphone.browser*</span></span>
- <span data-ttu-id="f7641-413">*opera.browser*</span><span class="sxs-lookup"><span data-stu-id="f7641-413">*opera.browser*</span></span>
- <span data-ttu-id="f7641-414">*safari.browser*</span><span class="sxs-lookup"><span data-stu-id="f7641-414">*safari.browser*</span></span>

#### <a name="using-browser-capabilities-providers"></a><span data-ttu-id="f7641-415">ブラウザー機能のプロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-415">Using Browser Capabilities Providers</span></span>

<span data-ttu-id="f7641-416">Asp.net version 3.5 Service Pack 1 では、次のように、ブラウザーを含む機能を定義できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-416">In ASP.NET version 3.5 Service Pack 1, you can define the capabilities that a browser has in the following ways:</span></span>

- <span data-ttu-id="f7641-417">コンピューター レベルでは、作成または更新を`.browser`次のフォルダーに XML ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-417">At the computer level, you create or update a `.browser` XML file in the following folder:</span></span>

- [!code-console[Main](overview/samples/sample27.cmd)]

- <span data-ttu-id="f7641-418">ブラウザーの機能を定義した後、次のコマンドに、Visual Studio コマンド プロンプトからブラウザー機能のアセンブリを再構築し、GAC に追加するにを実行します。</span><span class="sxs-lookup"><span data-stu-id="f7641-418">After you define the browser capability, you run the following command from the Visual Studio Command Prompt in order to rebuild the browser capabilities assembly and add it to the GAC:</span></span>

- [!code-console[Main](overview/samples/sample28.cmd)]

- <span data-ttu-id="f7641-419">個々 のアプリケーションの作成、`.browser`アプリケーションのファイル`App_Browsers`フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f7641-419">For an individual application, you create a `.browser` file in the application's `App_Browsers` folder.</span></span>

<span data-ttu-id="f7641-420">これらの方法では、XML ファイルを変更する必要があり、コンピューター レベルの変更、aspnet を実行した後、アプリケーションを再起動する必要があります\_regbrowsers.exe プロセス。</span><span class="sxs-lookup"><span data-stu-id="f7641-420">These approaches require you to change XML files, and for computer-level changes, you must restart the application after you run the aspnet\_regbrowsers.exe process.</span></span>

<span data-ttu-id="f7641-421">ASP.NET 4 と呼ばれる機能が含まれています*ブラウザー機能プロバイダー*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-421">ASP.NET 4 includes a feature referred to as *browser capabilities providers*.</span></span> <span data-ttu-id="f7641-422">名前からわかるように、ブラウザーの機能を決定するのに、独自のコードが使用できるプロバイダーを構築するこのできます。</span><span class="sxs-lookup"><span data-stu-id="f7641-422">As the name suggests, this lets you build a provider that in turn lets you use your own code to determine browser capabilities.</span></span>

<span data-ttu-id="f7641-423">実際には、開発者多くの場合を定義しないでカスタム ブラウザーの機能です。</span><span class="sxs-lookup"><span data-stu-id="f7641-423">In practice, developers often do not define custom browser capabilities.</span></span> <span data-ttu-id="f7641-424">ブラウザー ファイルの更新プログラム、プロセスは非常に複雑で、更新することが難しいとの XML 構文`.browser`ファイルは、定義を使用して複雑になることができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-424">Browser files are hard to update, the process for updating them is fairly complicated, and the XML syntax for `.browser` files can be complex to use and define.</span></span> <span data-ttu-id="f7641-425">何がこのプロセスがより簡単とは、一般的なブラウザーの定義の構文がある場合または最新のブラウザーの定義、またはそのようなデータベース、Web サービスも含まれているデータベース。</span><span class="sxs-lookup"><span data-stu-id="f7641-425">What would make this process much easier is if there were a common browser definition syntax, or a database that contained up-to-date browser definitions, or even a Web service for such a database.</span></span> <span data-ttu-id="f7641-426">新しいブラウザーの機能のプロバイダー機能により、これらのシナリオ実現してサード パーティ製の開発者向け。</span><span class="sxs-lookup"><span data-stu-id="f7641-426">The new browser capabilities providers feature makes these scenarios possible and practical for third-party developers.</span></span>

<span data-ttu-id="f7641-427">新しい ASP.NET 4 のブラウザー機能のプロバイダー機能を使用するための 2 つの主な方法: ASP.NET ブラウザー機能の定義の機能を拡張するか、完全に置換することです。</span><span class="sxs-lookup"><span data-stu-id="f7641-427">There are two main approaches for using the new ASP.NET 4 browser capabilities provider feature: extending the ASP.NET browser capabilities definition functionality, or totally replacing it.</span></span> <span data-ttu-id="f7641-428">次のセクションではの機能を置き換える方法と、それを拡張する方法最初について説明します。</span><span class="sxs-lookup"><span data-stu-id="f7641-428">The following sections describe first how to replace the functionality, and then how to extend it.</span></span>

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a><span data-ttu-id="f7641-429">ASP.NET ブラウザー機能の機能を置き換える</span><span class="sxs-lookup"><span data-stu-id="f7641-429">Replacing the ASP.NET Browser Capabilities Functionality</span></span>

<span data-ttu-id="f7641-430">ASP.NET ブラウザー機能の定義機能を完全に置き換えるには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="f7641-430">To replace the ASP.NET browser capabilities definition functionality completely, follow these steps:</span></span>

1. <span data-ttu-id="f7641-431">派生するプロバイダー クラスを作成*HttpCapabilitiesProvider*をオーバーライドして、 *GetBrowserCapabilities*次の例のように、メソッド。</span><span class="sxs-lookup"><span data-stu-id="f7641-431">Create a provider class that derives from *HttpCapabilitiesProvider* and that overrides the *GetBrowserCapabilities* method, as in the following example:</span></span> 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    <span data-ttu-id="f7641-432">この例では、コードが新たに作成*HttpBrowserCapabilities*オブジェクト、MyCustomBrowser にその機能を設定してブラウザーという名前の機能のみを指定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-432">The code in this example creates a new *HttpBrowserCapabilities* object, specifying only the capability named browser and setting that capability to MyCustomBrowser.</span></span>
2. <span data-ttu-id="f7641-433">アプリケーション プロバイダーに登録します。</span><span class="sxs-lookup"><span data-stu-id="f7641-433">Register the provider with the application.</span></span> 

    <span data-ttu-id="f7641-434">アプリケーションとプロバイダーを使用するために追加する必要があります、*プロバイダー*属性を*browserCaps*セクション、`Web.config`または`Machine.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-434">In order to use a provider with an application, you must add the *provider* attribute to the *browserCaps* section in the `Web.config` or `Machine.config` files.</span></span> <span data-ttu-id="f7641-435">(プロバイダーの属性を定義することもできます、*場所*要素の特定のモバイル デバイス用のフォルダーなどのアプリケーションで特定のディレクトリ)。次の例は、設定する方法を示します、*プロバイダー*構成ファイル内の属性。</span><span class="sxs-lookup"><span data-stu-id="f7641-435">(You can also define the provider attributes in a *location* element for specific directories in application, such as in a folder for a specific mobile device.) The following example shows how to set the *provider* attribute in a configuration file:</span></span>

    [!code-xml[Main](overview/samples/sample30.xml)]

    <span data-ttu-id="f7641-436">新しいブラウザー機能の定義を登録する別の方法は、次の例に示すようにコードを使用するは。</span><span class="sxs-lookup"><span data-stu-id="f7641-436">Another way to register the new browser capability definition is to use code, as shown in the following example:</span></span>

    [!code-csharp[Main](overview/samples/sample31.cs)]

    <span data-ttu-id="f7641-437">このコードを実行する必要があります、*アプリケーション\_開始*のイベント、`Global.asax`ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-437">This code must run in the *Application\_Start* event of the `Global.asax` file.</span></span> <span data-ttu-id="f7641-438">何らかの変更、 *BrowserCapabilitiesProvider*クラスは、キャッシュが、解決の有効な状態に残っているかどうかを確認するために、アプリケーション内のコードの実行前に行う必要があります*HttpCapabilitiesBase*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f7641-438">Any change to the *BrowserCapabilitiesProvider* class must occur before any code in the application executes, in order to make sure that the cache remains in a valid state for the resolved *HttpCapabilitiesBase* object.</span></span>

#### <a name="caching-the-httpbrowsercapabilities-object"></a><span data-ttu-id="f7641-439">HttpBrowserCapabilities オブジェクトのキャッシュ</span><span class="sxs-lookup"><span data-stu-id="f7641-439">Caching the HttpBrowserCapabilities Object</span></span>

<span data-ttu-id="f7641-440">前の例は、コードは、カスタム プロバイダーを取得するために呼び出すたびに実行ことである 1 つの問題、 *HttpBrowserCapabilities*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f7641-440">The preceding example has one problem, which is that the code would run each time the custom provider is invoked in order to get the *HttpBrowserCapabilities* object.</span></span> <span data-ttu-id="f7641-441">これでは、複数回を各要求中に発生します。</span><span class="sxs-lookup"><span data-stu-id="f7641-441">This can happen multiple times during each request.</span></span> <span data-ttu-id="f7641-442">例では、プロバイダーのコードは行いません多くします。</span><span class="sxs-lookup"><span data-stu-id="f7641-442">In the example, the code for the provider does not do much.</span></span> <span data-ttu-id="f7641-443">ただし、取得する場合は、カスタム プロバイダーのコードで、重要な作業を実行するため、 *HttpBrowserCapabilities*オブジェクト、パフォーマンスに影響することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-443">However, if the code in your custom provider performs significant work in order to get the *HttpBrowserCapabilities* object, this can affect performance.</span></span> <span data-ttu-id="f7641-444">これが事態を防ぐためにキャッシュすることができます、 *HttpBrowserCapabilities*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f7641-444">To prevent this from happening, you can cache the *HttpBrowserCapabilities* object.</span></span> <span data-ttu-id="f7641-445">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="f7641-445">Follow these steps:</span></span>

1. <span data-ttu-id="f7641-446">派生するクラスを作成*HttpCapabilitiesProvider*などの次の例 1。</span><span class="sxs-lookup"><span data-stu-id="f7641-446">Create a class that derives from *HttpCapabilitiesProvider*, like the one in the following example:</span></span> 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    <span data-ttu-id="f7641-447">例では、コードでは、カスタムの BuildCacheKey メソッドを呼び出して、キャッシュ キーを生成し、カスタム GetCacheTime メソッドを呼び出すことでキャッシュする時間の長さを取得します。</span><span class="sxs-lookup"><span data-stu-id="f7641-447">In the example, the code generates a cache key by calling a custom BuildCacheKey method, and it gets the length of time to cache by calling a custom GetCacheTime method.</span></span> <span data-ttu-id="f7641-448">コードは、解決された追加*HttpBrowserCapabilities*キャッシュするオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f7641-448">The code then adds the resolved *HttpBrowserCapabilities* object to the cache.</span></span> <span data-ttu-id="f7641-449">オブジェクトをキャッシュから取得しで再利用する後続の要求が、カスタム プロバイダーの使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-449">The object can be retrieved from the cache and reused on subsequent requests that make use of the custom provider.</span></span>
2. <span data-ttu-id="f7641-450">前の手順で説明されているアプリケーションでプロバイダーを登録します。</span><span class="sxs-lookup"><span data-stu-id="f7641-450">Register the provider with the application as described in the preceding procedure.</span></span>

#### <a name="extending-aspnet-browser-capabilities-functionality"></a><span data-ttu-id="f7641-451">ASP.NET ブラウザー機能の機能を拡張します。</span><span class="sxs-lookup"><span data-stu-id="f7641-451">Extending ASP.NET Browser Capabilities Functionality</span></span>

<span data-ttu-id="f7641-452">前のセクションには、新たに作成する方法が説明されている*HttpBrowserCapabilities* ASP.NET 4 でのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f7641-452">The previous section described how to create a new *HttpBrowserCapabilities* object in ASP.NET 4.</span></span> <span data-ttu-id="f7641-453">ASP.NET で既にできるものの新しいブラウザーの機能の定義を追加することで、ASP.NET ブラウザー機能の機能を拡張することもできます。</span><span class="sxs-lookup"><span data-stu-id="f7641-453">You can also extend the ASP.NET browser capabilities functionality by adding new browser capabilities definitions to those that are already in ASP.NET.</span></span> <span data-ttu-id="f7641-454">XML のブラウザー定義を使用せず、これを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-454">You can do this without using the XML browser definitions.</span></span> <span data-ttu-id="f7641-455">次の手順に示す方法。</span><span class="sxs-lookup"><span data-stu-id="f7641-455">The following procedure shows how.</span></span>

1. <span data-ttu-id="f7641-456">派生するクラスを作成*HttpCapabilitiesEvaluator*をオーバーライドして、 *GetBrowserCapabilities*メソッドを次の例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="f7641-456">Create a class that derives from *HttpCapabilitiesEvaluator* and that overrides the *GetBrowserCapabilities* method, as shown in the following example:</span></span> 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    <span data-ttu-id="f7641-457">まず、このコードは、ブラウザーを識別しようとするのに ASP.NET ブラウザー機能の機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-457">This code first uses the ASP.NET browser capabilities functionality to try to identify the browser.</span></span> <span data-ttu-id="f7641-458">ただし、ブラウザーが指定されていない場合、情報に基づいて、要求で定義されている (つまり場合、*ブラウザー*のプロパティ、 *HttpBrowserCapabilities*オブジェクトが文字列"Unknown")、コードでは、カスタム プロバイダー (MyBrowserCapabilitiesEvaluator)、ブラウザーを識別するためにします。</span><span class="sxs-lookup"><span data-stu-id="f7641-458">However, if no browser is identified based on the information defined in the request (that is, if the *Browser* property of the *HttpBrowserCapabilities* object is the string "Unknown"), the code calls the custom provider (MyBrowserCapabilitiesEvaluator) to identify the browser.</span></span>
2. <span data-ttu-id="f7641-459">前の例」の説明に従って、アプリケーションにプロバイダーを登録します。</span><span class="sxs-lookup"><span data-stu-id="f7641-459">Register the provider with the application as described in the previous example.</span></span>

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a><span data-ttu-id="f7641-460">既存の機能の定義に新しい機能を追加してブラウザーの機能の機能を拡張します。</span><span class="sxs-lookup"><span data-stu-id="f7641-460">Extending Browser Capabilities Functionality by Adding New Capabilities to Existing Capabilities Definitions</span></span>

<span data-ttu-id="f7641-461">に加えて、カスタムのブラウザー定義プロバイダーを作成して、新しいブラウザーの定義を動的に作成するのには、追加の機能を備えた既存のブラウザー定義を拡張できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-461">In addition to creating a custom browser definition provider and to dynamically creating new browser definitions, you can extend existing browser definitions with additional capabilities.</span></span> <span data-ttu-id="f7641-462">これにより、目的の近くにあるが、いくつかの機能のみがない定義を使用できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-462">This lets you use a definition that is close to what you want but lacks only a few capabilities.</span></span> <span data-ttu-id="f7641-463">そのためには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="f7641-463">To do this, use the following steps.</span></span>

1. <span data-ttu-id="f7641-464">派生するクラスを作成*HttpCapabilitiesEvaluator*をオーバーライドして、 *GetBrowserCapabilities*メソッドを次の例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="f7641-464">Create a class that derives from *HttpCapabilitiesEvaluator* and that overrides the *GetBrowserCapabilities* method, as shown in the following example:</span></span> 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    <span data-ttu-id="f7641-465">コード例は、既存の ASP.NET を拡張*HttpCapabilitiesEvaluator*クラスを取得するため、 *HttpBrowserCapabilities*次のコードを使用して現在の要求の定義に一致するオブジェクト:</span><span class="sxs-lookup"><span data-stu-id="f7641-465">The example code extends the existing ASP.NET *HttpCapabilitiesEvaluator* class and gets the *HttpBrowserCapabilities* object that matches the current request definition by using the following code:</span></span>

    [!code-csharp[Main](overview/samples/sample35.cs)]

    <span data-ttu-id="f7641-466">コードでは追加、またはこのブラウザーの機能を変更できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-466">The code can then add or modify a capability for this browser.</span></span> <span data-ttu-id="f7641-467">新しいブラウザーの機能を指定する 2 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="f7641-467">There are two ways to specify a new browser capability:</span></span>

    - <span data-ttu-id="f7641-468">キー/値ペアを追加、 *IDictionary*オブジェクトによって公開される、*機能*のプロパティ、 *HttpCapabilitiesBase*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f7641-468">Add a key/value pair to the *IDictionary* object that is exposed by the *Capabilities* property of the *HttpCapabilitiesBase* object.</span></span> <span data-ttu-id="f7641-469">前の例では、マルチタッチをという名前の値を持つ機能を追加するコードを*true*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-469">In the previous example, the code adds a capability named MultiTouch with a value of *true*.</span></span>
    - <span data-ttu-id="f7641-470">既存のプロパティの設定、 *HttpCapabilitiesBase*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f7641-470">Set existing properties of the *HttpCapabilitiesBase* object.</span></span> <span data-ttu-id="f7641-471">前の例では、コードを設定、*フレーム*プロパティを*true*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-471">In the previous example, the code sets the *Frames* property to *true*.</span></span> <span data-ttu-id="f7641-472">このプロパティはアクセサーを単に、 *IDictionary*オブジェクトによって公開される、*機能*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="f7641-472">This property is simply an accessor for the *IDictionary* object that is exposed by the *Capabilities* property.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="f7641-473">このモデルは任意のプロパティに適用されます*HttpBrowserCapabilities*、コントロール アダプターを含むです。</span><span class="sxs-lookup"><span data-stu-id="f7641-473">Note This model applies to any property of *HttpBrowserCapabilities*, including control adapters.</span></span>
2. <span data-ttu-id="f7641-474">前の手順で説明されているアプリケーションで、プロバイダーを登録します。</span><span class="sxs-lookup"><span data-stu-id="f7641-474">Register the provider with the application as described in the earlier procedure.</span></span>

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a><span data-ttu-id="f7641-475">ASP.NET 4 でのルーティング</span><span class="sxs-lookup"><span data-stu-id="f7641-475">Routing in ASP.NET 4</span></span>

<span data-ttu-id="f7641-476">ASP.NET 4 では、Web フォームによるルーティングを使用するための組み込みサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="f7641-476">ASP.NET 4 adds built-in support for using routing with Web Forms.</span></span> <span data-ttu-id="f7641-477">要求の物理ファイルにマップされていない Url のルーティングは、そのまま使用するアプリケーションを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-477">Routing lets you configure an application to accept request URLs that do not map to physical files.</span></span> <span data-ttu-id="f7641-478">代わりに、ルーティングを使用する、ユーザーにわかりやすいと、アプリケーションの検索エンジン最適化 (SEO) に役立つことができます、Url を定義します。</span><span class="sxs-lookup"><span data-stu-id="f7641-478">Instead, you can use routing to define URLs that are meaningful to users and that can help with search-engine optimization (SEO) for your application.</span></span> <span data-ttu-id="f7641-479">たとえば、既存のアプリケーションの製品カテゴリを表示するページの URL は、次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="f7641-479">For example, the URL for a page that displays product categories in an existing application might look like the following example:</span></span>

[!code-console[Main](overview/samples/sample36.cmd)]

<span data-ttu-id="f7641-480">ルーティングを使用するには、同じ情報を表示するために、次の URL を受け入れるようにアプリケーションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-480">By using routing, you can configure the application to accept the following URL to render the same information:</span></span>

[!code-console[Main](overview/samples/sample37.cmd)]

<span data-ttu-id="f7641-481">ルーティングは、ASP.NET 3.5 SP1 以降で使用されました。</span><span class="sxs-lookup"><span data-stu-id="f7641-481">Routing has been available starting with ASP.NET 3.5 SP1.</span></span> <span data-ttu-id="f7641-482">(ASP.NET 3.5 SP1 でのルーティングを使用する方法の例は、エントリを参照してください。 [WebForms でルーティングを使用して](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "このエントリのタイトル。")</span><span class="sxs-lookup"><span data-stu-id="f7641-482">(For an example of how to use routing in ASP.NET 3.5 SP1, see the entry [Using Routing With WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "Title of this entry.")</span></span> <span data-ttu-id="f7641-483">Phil Haack のブログです。)ただし、ASP.NET 4 が含まれています、ルーティングを使用するより簡単にするいくつかの機能には次を含みます。</span><span class="sxs-lookup"><span data-stu-id="f7641-483">on Phil Haack's blog.) However, ASP.NET 4 includes some features that make it easier to use routing, including the following:</span></span>

- <span data-ttu-id="f7641-484">*ある PageRouteHandler*はルートを定義するときに使用する単純な HTTP ハンドラー クラス。</span><span class="sxs-lookup"><span data-stu-id="f7641-484">The *PageRouteHandler* class, which is a simple HTTP handler that you use when you define routes.</span></span> <span data-ttu-id="f7641-485">クラスは、要求にルーティングするページにデータを渡します。</span><span class="sxs-lookup"><span data-stu-id="f7641-485">The class passes data to the page that the request is routed to.</span></span>
- <span data-ttu-id="f7641-486">新しいプロパティ*HttpRequest.RequestContext*と*Page.RouteData* (これは、プロキシを*HttpRequest.RequestContext.RouteData*オブジェクト)。</span><span class="sxs-lookup"><span data-stu-id="f7641-486">The new properties *HttpRequest.RequestContext* and *Page.RouteData* (which is a proxy for the *HttpRequest.RequestContext.RouteData* object).</span></span> <span data-ttu-id="f7641-487">これらのプロパティを簡単に、ルートから渡される情報にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="f7641-487">These properties make it easier to access information that is passed from the route.</span></span>
- <span data-ttu-id="f7641-488">定義されている次新しい式ビルダー、 *System.Web.Compilation.RouteUrlExpressionBuilder*と*System.Web.Compilation.RouteValueExpressionBuilder*:</span><span class="sxs-lookup"><span data-stu-id="f7641-488">The following new expression builders, which are defined in *System.Web.Compilation.RouteUrlExpressionBuilder* and *System.Web.Compilation.RouteValueExpressionBuilder*:</span></span>
- <span data-ttu-id="f7641-489">*RouteUrl*、ASP.NET サーバー コントロール内のルート URL に対応する URL を作成する簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="f7641-489">*RouteUrl*, which provides a simple way to create a URL that corresponds to a route URL within an ASP.NET server control.</span></span>
- <span data-ttu-id="f7641-490">*RouteValue*からの情報を抽出する簡単な方法を提供する、 *RouteContext*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f7641-490">*RouteValue*, which provides a simple way to extract information from the *RouteContext* object.</span></span>
- <span data-ttu-id="f7641-491">*RouteParameter*クラスは、簡単に含まれるデータを渡す、 *RouteContext*オブジェクト データ ソース コントロールのクエリを (のような[ *FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).</span><span class="sxs-lookup"><span data-stu-id="f7641-491">The *RouteParameter* class, which makes it easier to pass data contained in a *RouteContext* object to a query for a data source control (similar to [*FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).</span></span>

#### <a name="routing-for-web-forms-pages"></a><span data-ttu-id="f7641-492">Web フォーム ページのルーティング</span><span class="sxs-lookup"><span data-stu-id="f7641-492">Routing for Web Forms Pages</span></span>

<span data-ttu-id="f7641-493">次の例は、新しいを使用して Web フォームのルートを定義する方法を示します*MapPageRoute*のメソッド、*ルート*クラス。</span><span class="sxs-lookup"><span data-stu-id="f7641-493">The following example shows how to define a Web Forms route by using the new *MapPageRoute* method of the *Route* class:</span></span>

[!code-csharp[Main](overview/samples/sample38.cs)]

<span data-ttu-id="f7641-494">ASP.NET 4 では、 *MapPageRoute*メソッド。</span><span class="sxs-lookup"><span data-stu-id="f7641-494">ASP.NET 4 introduces the *MapPageRoute* method.</span></span> <span data-ttu-id="f7641-495">次の例は前の例で示した SearchRoute 定義と同じですが、使用、*ある PageRouteHandler*クラス。</span><span class="sxs-lookup"><span data-stu-id="f7641-495">The following example is equivalent to the SearchRoute definition shown in the previous example, but uses the *PageRouteHandler* class.</span></span>

[!code-csharp[Main](overview/samples/sample39.cs)]

<span data-ttu-id="f7641-496">コード例では、物理ページにルートをマップする (最初のルート上でに`~/search.aspx`)。</span><span class="sxs-lookup"><span data-stu-id="f7641-496">The code in the example maps the route to a physical page (in the first route, to `~/search.aspx`).</span></span> <span data-ttu-id="f7641-497">最初のルート定義は、searchterm をという名前のパラメーターを URL から抽出し、ページに渡される必要があることにも指定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-497">The first route definition also specifies that the parameter named searchterm should be extracted from the URL and passed to the page.</span></span>

<span data-ttu-id="f7641-498">*MapPageRoute*メソッドは、次のメソッドのオーバー ロードをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="f7641-498">The *MapPageRoute* method supports the following method overloads:</span></span>

- <span data-ttu-id="f7641-499">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess)*</span><span class="sxs-lookup"><span data-stu-id="f7641-499">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess)*</span></span>
- <span data-ttu-id="f7641-500">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults)*</span><span class="sxs-lookup"><span data-stu-id="f7641-500">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults)*</span></span>
- <span data-ttu-id="f7641-501">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults, RouteValueDictionary constraints)*</span><span class="sxs-lookup"><span data-stu-id="f7641-501">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults, RouteValueDictionary constraints)*</span></span>

<span data-ttu-id="f7641-502">*CheckPhysicalUrlAccess*パラメーターは、ルートにルーティングされている物理ページの セキュリティ アクセス許可を確認する必要があるかどうかを指定します (この場合は、完成しました) と、着信 URL のアクセス許可 (この場合、検索/{searchterm})。</span><span class="sxs-lookup"><span data-stu-id="f7641-502">The *checkPhysicalUrlAccess* parameter specifies whether the route should check the security permissions for the physical page being routed to (in this case, search.aspx) and the permissions on the incoming URL (in this case, search/{searchterm}).</span></span> <span data-ttu-id="f7641-503">場合の値*checkPhysicalUrlAccess*は*false*、着信 URL のアクセス許可のみがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="f7641-503">If the value of *checkPhysicalUrlAccess* is *false*, only the permissions of the incoming URL will be checked.</span></span> <span data-ttu-id="f7641-504">これらのアクセス許可が定義されている、`Web.config`ファイルに次のように設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-504">These permissions are defined in the `Web.config` file using settings such as the following:</span></span>

[!code-xml[Main](overview/samples/sample40.xml)]

<span data-ttu-id="f7641-505">物理ページに、構成の例ではアクセスが拒否されました`search.aspx`管理者の役割ではお客様を除くすべてのユーザー。</span><span class="sxs-lookup"><span data-stu-id="f7641-505">In the example configuration, access is denied to the physical page `search.aspx` for all users except those who are in the admin role.</span></span> <span data-ttu-id="f7641-506">ときに、 *checkPhysicalUrlAccess*にパラメーターが設定されている*true* (既定値は)、物理ページ完成しましたので、URL/search/{searchterm} にアクセスする管理者ユーザーのみが許可されてそのロールのユーザーに制限されています。</span><span class="sxs-lookup"><span data-stu-id="f7641-506">When the *checkPhysicalUrlAccess* parameter is set to *true* (which is its default value), only admin users are allowed to access the URL /search/{searchterm}, because the physical page search.aspx is restricted to users in that role.</span></span> <span data-ttu-id="f7641-507">場合*checkPhysicalUrlAccess*に設定されている*false*と前の例で示すように、サイトが構成されている、認証済みのすべてのユーザーは URL/search/{searchterm} へのアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="f7641-507">If *checkPhysicalUrlAccess* is set to *false* and the site is configured as shown in the previous example, all authenticated users are allowed to access the URL /search/{searchterm}.</span></span>

#### <a name="reading-routing-information-in-a-web-forms-page"></a><span data-ttu-id="f7641-508">ルーティング情報を Web フォーム ページの読み取り</span><span class="sxs-lookup"><span data-stu-id="f7641-508">Reading Routing Information in a Web Forms Page</span></span>

<span data-ttu-id="f7641-509">Web フォームの物理的なページのコードでのルーティングが URL から抽出される情報にアクセスすることができます (または別のオブジェクトに追加したその他の情報、 *RouteData*オブジェクト) 2 つの新しいプロパティを使用して: *HttpRequest.RequestContext*と*Page.RouteData*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-509">In the code of the Web Forms physical page, you can access the information that routing has extracted from the URL (or other information that another object has added to the *RouteData* object) by using two new properties: *HttpRequest.RequestContext* and *Page.RouteData*.</span></span> <span data-ttu-id="f7641-510">(*Page.RouteData*ラップ*HttpRequest.RequestContext.RouteData*)。次の例は、使用する方法を示します*Page.RouteData*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-510">(*Page.RouteData* wraps *HttpRequest.RequestContext.RouteData*.) The following example shows how to use *Page.RouteData*.</span></span>

[!code-csharp[Main](overview/samples/sample41.cs)]

<span data-ttu-id="f7641-511">コードは、前の例のルートで定義されている searchterm パラメーターには、渡された値を抽出します。</span><span class="sxs-lookup"><span data-stu-id="f7641-511">The code extracts the value that was passed for the searchterm parameter, as defined in the example route earlier.</span></span> <span data-ttu-id="f7641-512">次の要求 URL を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="f7641-512">Consider the following request URL:</span></span>

[!code-console[Main](overview/samples/sample42.cmd)]

<span data-ttu-id="f7641-513">この要求が行われる場合でレンダリングされます"scott"という単語、`search.aspx`ページ。</span><span class="sxs-lookup"><span data-stu-id="f7641-513">When this request is made, the word "scott" would be rendered in the `search.aspx` page.</span></span>

#### <a name="accessing-routing-information-in-markup"></a><span data-ttu-id="f7641-514">マークアップでのルーティング情報にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="f7641-514">Accessing Routing Information in Markup</span></span>

<span data-ttu-id="f7641-515">前のセクションで説明されているメソッドは、Web フォーム ページのコードでルート データを取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-515">The method described in the previous section shows how to get route data in code in a Web Forms page.</span></span> <span data-ttu-id="f7641-516">同じ情報にアクセスするためのマークアップ内の式を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="f7641-516">You can also use expressions in markup that give you access to the same information.</span></span> <span data-ttu-id="f7641-517">式ビルダーは、宣言型コードを操作する強力で洗練された方法です。</span><span class="sxs-lookup"><span data-stu-id="f7641-517">Expression builders are a powerful and elegant way to work with declarative code.</span></span> <span data-ttu-id="f7641-518">(詳細については、エントリを参照してください[Express 自分でカスタム式ビルダー](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) Phil Haack のブログです。)。</span><span class="sxs-lookup"><span data-stu-id="f7641-518">(For more information, see the entry [Express Yourself With Custom Expression Builders](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) on Phil Haack's blog.)</span></span>

<span data-ttu-id="f7641-519">ASP.NET 4 には、Web フォームのルーティング用の 2 つの新しい式ビルダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f7641-519">ASP.NET 4 includes two new expression builders for Web Forms routing.</span></span> <span data-ttu-id="f7641-520">次の例では、それらを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-520">The following example shows how to use them.</span></span>

[!code-aspx[Main](overview/samples/sample43.aspx)]

<span data-ttu-id="f7641-521">この例で、 *RouteUrl*ルート パラメーターに基づいている URL を定義する式を使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-521">In the example, the *RouteUrl* expression is used to define a URL that is based on a route parameter.</span></span> <span data-ttu-id="f7641-522">これは、マークアップに完全な URL をハードコーディングする手間が省けます、このリンクの変更を必要とせず、URL の構造を後で変更することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-522">This saves you from having to hard-code the complete URL into the markup, and lets you change the URL structure later without requiring any change to this link.</span></span>

<span data-ttu-id="f7641-523">以前に定義されているルートに基づいて、このマークアップは、次の URL が生成されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-523">Based on the route defined earlier, this markup generates the following URL:</span></span>

[!code-console[Main](overview/samples/sample44.cmd)]

<span data-ttu-id="f7641-524">ASP.NET は、適切なルートを自動的に動作 (正しい URL を生成、)、入力パラメーターに基づきます。</span><span class="sxs-lookup"><span data-stu-id="f7641-524">ASP.NET automatically works out the correct route (that is, it generates the correct URL) based on the input parameters.</span></span> <span data-ttu-id="f7641-525">式で使用するルートを指定するため、ルート名を含めることもできます。</span><span class="sxs-lookup"><span data-stu-id="f7641-525">You can also include a route name in the expression, which lets you specify a route to use.</span></span>

<span data-ttu-id="f7641-526">次の例は、使用する方法を示します、 *RouteValue*式。</span><span class="sxs-lookup"><span data-stu-id="f7641-526">The following example shows how to use the *RouteValue* expression.</span></span>

[!code-aspx[Main](overview/samples/sample45.aspx)]

<span data-ttu-id="f7641-527">このコントロールを含むページを実行すると、ラベルに値"scott"が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-527">When the page that contains this control runs, the value "scott" is displayed in the label.</span></span>

<span data-ttu-id="f7641-528">*RouteValue*式では、マークアップでは、ルート データを使用する単純なより複雑な Page.RouteData["x を使用することを避ける"] マークアップ構文です。</span><span class="sxs-lookup"><span data-stu-id="f7641-528">The *RouteValue* expression makes it simple to use route data in markup, and it avoids having to work with the more complex Page.RouteData["x"] syntax in markup.</span></span>

#### <a name="using-route-data-for-data-source-control-parameters"></a><span data-ttu-id="f7641-529">ルート データ データ ソース コントロールのパラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-529">Using Route Data for Data Source Control Parameters</span></span>

<span data-ttu-id="f7641-530">*RouteParameter*クラスでは、ルート データをデータ ソース コントロール内のクエリのパラメーター値として指定することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-530">The *RouteParameter* class lets you specify route data as a parameter value for queries in a data source control.</span></span> <span data-ttu-id="f7641-531">これは、 [works はほぼ同じように、](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)クラスに、次の例に示すように。</span><span class="sxs-lookup"><span data-stu-id="f7641-531">It [works much like the](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) class, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample46.aspx)]

<span data-ttu-id="f7641-532">ルート パラメーターの searchterm の値が使用しての例では、@companynameパラメーター、<em>選択</em>ステートメント。</span><span class="sxs-lookup"><span data-stu-id="f7641-532">In this case, the value of the route parameter searchterm will be used for the @companyname parameter in the <em>Select</em> statement.</span></span>

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a><span data-ttu-id="f7641-533">クライアント Id の設定</span><span class="sxs-lookup"><span data-stu-id="f7641-533">Setting Client IDs</span></span>

<span data-ttu-id="f7641-534">新しい*ClientIDMode*プロパティが ASP.NET では、長期にわたる問題に対処 namely コントロールを作成する方法、 *id*をレンダリングする要素の属性。</span><span class="sxs-lookup"><span data-stu-id="f7641-534">The new *ClientIDMode* property addresses a long-standing issue in ASP.NET, namely how controls create the *id* attribute for elements that they render.</span></span> <span data-ttu-id="f7641-535">把握、 *id*レンダリングされる要素の属性は、アプリケーションには、これらの要素を参照するクライアント スクリプトが含まれている場合に重要です。</span><span class="sxs-lookup"><span data-stu-id="f7641-535">Knowing the *id* attribute for rendered elements is important if your application includes client script that references these elements.</span></span>

<span data-ttu-id="f7641-536">*Id*に基づいて Web サーバー コントロールのレンダリングされる HTML 属性を生成、 *ClientID*コントロールのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f7641-536">The *id* attribute in HTML that is rendered for Web server controls is generated based on the *ClientID* property of the control.</span></span> <span data-ttu-id="f7641-537">ASP.NET 4 で生成するためのアルゴリズムまで、 *id*属性を*ClientID*プロパティは、ID のように、繰り返し使用されるコントロールの場合、(ある場合)、名前付けコンテナーを連結するようにしていますデータ コントロール)、プレフィックスとシーケンシャル番号を追加します。</span><span class="sxs-lookup"><span data-stu-id="f7641-537">Until ASP.NET 4, the algorithm for generating the *id* attribute from the *ClientID* property has been to concatenate the naming container (if any) with the ID, and in the case of repeated controls (as in data controls), to add a prefix and a sequential number.</span></span> <span data-ttu-id="f7641-538">コントロール Id は、予測可能なクライアント スクリプトで参照するが困難でしたので、アルゴリズムが常に、ページ内のコントロールの Id が一意であることを保証これが、中が発生しました。</span><span class="sxs-lookup"><span data-stu-id="f7641-538">While this has always guaranteed that the IDs of controls in the page are unique, the algorithm has resulted in control IDs that were not predictable, and were therefore difficult to reference in client script.</span></span>

<span data-ttu-id="f7641-539">新しい*ClientIDMode*プロパティでは、コントロールのクライアント ID が生成される方法により正確に指定することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-539">The new *ClientIDMode* property lets you specify more precisely how the client ID is generated for controls.</span></span> <span data-ttu-id="f7641-540">設定することができます、 *ClientIDMode*ページなど、あらゆるコントロールのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f7641-540">You can set the *ClientIDMode* property for any control, including for the page.</span></span> <span data-ttu-id="f7641-541">設定できる次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f7641-541">Possible settings are the following:</span></span>

- <span data-ttu-id="f7641-542">*AutoID* – これは、生成するためのアルゴリズムを*ClientID*旧バージョンの ASP.NET で使用されていたプロパティ値。</span><span class="sxs-lookup"><span data-stu-id="f7641-542">*AutoID* – This is equivalent to the algorithm for generating *ClientID* property values that was used in earlier versions of ASP.NET.</span></span>
- <span data-ttu-id="f7641-543">*静的*– ことを指定します、 *ClientID*値には、親の名前付けコンテナーの Id を連結することがなく ID と同じになります。</span><span class="sxs-lookup"><span data-stu-id="f7641-543">*Static* – This specifies that the *ClientID* value will be the same as the ID without concatenating the IDs of parent naming containers.</span></span> <span data-ttu-id="f7641-544">これは、Web ユーザー コントロールに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="f7641-544">This can be useful in Web user controls.</span></span> <span data-ttu-id="f7641-545">Web ユーザー コントロールなさまざまなページで、別のコンテナー コントロールで配置されているので、難しいことがありますを使用するコントロールのクライアント スクリプトを記述、 *AutoID*アルゴリズムする ID 値を予測することはできませんので、.</span><span class="sxs-lookup"><span data-stu-id="f7641-545">Because a Web user control can be located on different pages and in different container controls, it can be difficult to write client script for controls that use the *AutoID* algorithm because you cannot predict what the ID values will be.</span></span>
- <span data-ttu-id="f7641-546">*予測可能な*– このオプションは、主に繰り返しのテンプレートを使用するデータ コントロールで使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-546">*Predictable* – This option is primarily for use in data controls that use repeating templates.</span></span> <span data-ttu-id="f7641-547">生成されたが、コントロールの名前付けコンテナーの ID プロパティを連結*ClientID*値には"ctlxxx"などの文字列が含まれていません。</span><span class="sxs-lookup"><span data-stu-id="f7641-547">It concatenates the ID properties of the control's naming containers, but generated *ClientID* values do not contain strings like "ctlxxx".</span></span> <span data-ttu-id="f7641-548">この設定は機能と組み合わせて、 *ClientIDRowSuffix*コントロールのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f7641-548">This setting works in conjunction with the *ClientIDRowSuffix* property of the control.</span></span> <span data-ttu-id="f7641-549">設定する、 *ClientIDRowSuffix*プロパティをデータ フィールドの名前とそのフィールドの値が生成されるサフィックスとして使用*ClientID*値。</span><span class="sxs-lookup"><span data-stu-id="f7641-549">You set the *ClientIDRowSuffix* property to the name of a data field, and the value of that field is used as the suffix for the generated *ClientID* value.</span></span> <span data-ttu-id="f7641-550">通常としてデータ レコードの主キーを使用すると、 *ClientIDRowSuffix*値。</span><span class="sxs-lookup"><span data-stu-id="f7641-550">Typically you would use the primary key of a data record as the *ClientIDRowSuffix* value.</span></span>
- <span data-ttu-id="f7641-551">*継承*– この設定は、コントロールの既定の動作は、コントロールの ID の生成がその親と同じであることを指定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-551">*Inherit* – This setting is the default behavior for controls; it specifies that a control's ID generation is the same as its parent.</span></span>

<span data-ttu-id="f7641-552">設定することができます、 *ClientIDMode*ページ レベルのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f7641-552">You can set the *ClientIDMode* property at the page level.</span></span> <span data-ttu-id="f7641-553">これは、既定値を定義*ClientIDMode*現在のページのすべてのコントロールの値。</span><span class="sxs-lookup"><span data-stu-id="f7641-553">This defines the default *ClientIDMode* value for all controls in the current page.</span></span>

<span data-ttu-id="f7641-554">既定の*ClientIDMode*ページ レベルの値は*AutoID*と既定*ClientIDMode*コントロール レベルの値である*継承*.</span><span class="sxs-lookup"><span data-stu-id="f7641-554">The default *ClientIDMode* value at the page level is *AutoID*, and the default *ClientIDMode* value at the control level is *Inherit*.</span></span> <span data-ttu-id="f7641-555">その結果、コードで、このプロパティを任意の場所設定はできませんと、すべてのコントロールは既定、 *AutoID*アルゴリズム。</span><span class="sxs-lookup"><span data-stu-id="f7641-555">As a result, if you do not set this property anywhere in your code, all controls will default to the *AutoID* algorithm.</span></span>

<span data-ttu-id="f7641-556">ページ レベルの値で設定する、 *@ Page*ディレクティブは、次の例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="f7641-556">You set the page-level value in the *@ Page* directive, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample47.aspx)]

<span data-ttu-id="f7641-557">設定することも、 *ClientIDMode* (コンピューター) をコンピューター レベルまたはアプリケーション レベルで、構成ファイル内の値。</span><span class="sxs-lookup"><span data-stu-id="f7641-557">You can also set the *ClientIDMode* value in the configuration file, either at the computer (machine) level or at the application level.</span></span> <span data-ttu-id="f7641-558">これは、既定値を定義*ClientIDMode*アプリケーション内のすべてのページのすべてのコントロールを設定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-558">This defines the default *ClientIDMode* setting for all controls in all pages in the application.</span></span> <span data-ttu-id="f7641-559">コンピューター レベルで、値を設定する場合、既定値を定義します。 *ClientIDMode*そのコンピューターですべての Web サイトを設定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-559">If you set the value at the computer level, it defines the default *ClientIDMode* setting for all Web sites on that computer.</span></span> <span data-ttu-id="f7641-560">次の例は、 *ClientIDMode*構成ファイルで設定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-560">The following example shows the *ClientIDMode* setting in the configuration file:</span></span>

[!code-xml[Main](overview/samples/sample48.xml)]

<span data-ttu-id="f7641-561">値を前に説明したように、 *ClientID*プロパティは、コントロールの親の名前付けコンテナーから派生します。</span><span class="sxs-lookup"><span data-stu-id="f7641-561">As noted earlier, the value of the *ClientID* property is derived from the naming container for a control's parent.</span></span> <span data-ttu-id="f7641-562">マスター ページを使用する場合など、いくつかのシナリオでコントロールが最終的に Id を持つもので、次のようにレンダリングされる HTML:</span><span class="sxs-lookup"><span data-stu-id="f7641-562">In some scenarios, such as when you are using master pages, controls can end up with IDs like those in the following rendered HTML:</span></span>

[!code-html[Main](overview/samples/sample49.html)]

<span data-ttu-id="f7641-563">場合でも、*入力*マークアップに示される要素 (から、 *テキスト ボックス*コントロール) 2 つの名前付けコンテナーは、ページの深さ (入れ子になった*ContentPlaceholder*コントロール)、マスター ページが処理されるため、最終結果は、次のようにコントロール ID です。</span><span class="sxs-lookup"><span data-stu-id="f7641-563">Even though the *input* element shown in the markup (from a *TextBox* control) is only two naming containers deep in the page (the nested *ContentPlaceholder* controls), because of the way master pages are processed, the end result is a control ID like the following:</span></span>

[!code-console[Main](overview/samples/sample50.cmd)]

<span data-ttu-id="f7641-564">この ID は ページで、一意であることが保証されますが、ほとんどの目的の時間が不必要にします。</span><span class="sxs-lookup"><span data-stu-id="f7641-564">This ID is guaranteed to be unique in the page, but is unnecessarily long for most purposes.</span></span> <span data-ttu-id="f7641-565">レンダリングの ID の長さを短くと、ID を生成する方法をより細かく制御することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="f7641-565">Imagine that you want to reduce the length of the rendered ID, and to have more control over how the ID is generated.</span></span> <span data-ttu-id="f7641-566">(たとえば、する"ctlxxx"プレフィックスを削除します。)これを実現する最も簡単な方法は、設定して、 *ClientIDMode*プロパティを次の例で示した。</span><span class="sxs-lookup"><span data-stu-id="f7641-566">(For example, you want to eliminate "ctlxxx" prefixes.) The easiest way to achieve this is by setting the *ClientIDMode* property as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample51.aspx)]

<span data-ttu-id="f7641-567">このサンプルで、 *ClientIDMode*プロパティに設定されて*静的*最も外側の*NamingPanel*要素、*予測可能*内部の*NamingControl*要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-567">In this sample, the *ClientIDMode* property is set to *Static* for the outermost *NamingPanel* element, and set to *Predictable* for the inner *NamingControl* element.</span></span> <span data-ttu-id="f7641-568">これらの設定と、次のマークアップ (ページとマスター ページの残りの部分は、前の例のように同じであると見なされます)。</span><span class="sxs-lookup"><span data-stu-id="f7641-568">These settings result in the following markup (the rest of the page and the master page is assumed to be the same as in the previous example):</span></span>

[!code-html[Main](overview/samples/sample52.html)]

<span data-ttu-id="f7641-569">*静的*設定は、最も外側にある、そのすべてのコントロールの名前付けの階層をリセットする際の影響*NamingPanel*要素がなくなると、 *ContentPlaceHolder*と*マスター* Id 生成された id。 から</span><span class="sxs-lookup"><span data-stu-id="f7641-569">The *Static* setting has the effect of resetting the naming hierarchy for any controls inside the outermost *NamingPanel* element, and of eliminating the *ContentPlaceHolder* and *MasterPage* IDs from the generated ID.</span></span> <span data-ttu-id="f7641-570">(、*名前*レンダリングする要素の属性が影響を受ける、イベントの通常の ASP.NET 機能が保持されるため、状態を表示します)。マークアップを移動する場合でも、名前付けの階層をリセットする際の副作用は、 *NamingPanel*要素を異なる*ContentPlaceholder*コントロール、レンダリングされたクライアント Id は変わりません。</span><span class="sxs-lookup"><span data-stu-id="f7641-570">(The *name* attribute of rendered elements is unaffected, so the normal ASP.NET functionality is retained for events, view state, and so on.) A side effect of resetting the naming hierarchy is that even if you move the markup for the *NamingPanel* elements to a different *ContentPlaceholder* control, the rendered client IDs remain the same.</span></span>

> [!NOTE]
> <span data-ttu-id="f7641-571">レンダリングされたコントロールの Id が一意であることを確認することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f7641-571">Note It is up to you to make sure that the rendered control IDs are unique.</span></span> <span data-ttu-id="f7641-572">そうでない場合は、クライアントなど、個々 の HTML 要素の一意の Id を必要とする任意の機能を壊すことできます*document.getElementById*関数。</span><span class="sxs-lookup"><span data-stu-id="f7641-572">If they are not, it can break any functionality that requires unique IDs for individual HTML elements, such as the client *document.getElementById* function.</span></span>


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a><span data-ttu-id="f7641-573">データ バインド コントロールで予測可能なクライアント Id を作成します。</span><span class="sxs-lookup"><span data-stu-id="f7641-573">Creating Predictable Client IDs in Data-Bound Controls</span></span>

<span data-ttu-id="f7641-574">*ClientID*の従来のアルゴリズムによってデータ連結リスト コントロール内のコントロールは、生成される値を実際に予測可能でないです。</span><span class="sxs-lookup"><span data-stu-id="f7641-574">The *ClientID* values that are generated for controls in a data-bound list control by the legacy algorithm can be long and are not really predictable.</span></span> <span data-ttu-id="f7641-575">*ClientIDMode*機能を使用して、詳細にどのようにこれらの Id を生成を制御できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-575">The *ClientIDMode* functionality can help you have more control over how these IDs are generated.</span></span>

<span data-ttu-id="f7641-576">次の例のマークアップが含まれています、 *ListView*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-576">The markup in the following example includes a *ListView* control:</span></span>

[!code-aspx[Main](overview/samples/sample53.aspx)]

<span data-ttu-id="f7641-577">前の例では、 *ClientIDMode*と*RowClientIDRowSuffix*マークアップでプロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-577">In the previous example, the *ClientIDMode* and *RowClientIDRowSuffix* properties are set in markup.</span></span> <span data-ttu-id="f7641-578">*ClientIDRowSuffix*プロパティは、データ バインド コントロールでのみ使用できを使用するコントロールに応じてでその動作が異なります。</span><span class="sxs-lookup"><span data-stu-id="f7641-578">The *ClientIDRowSuffix* property can be used only in data-bound controls, and its behavior differs depending on which control you are using.</span></span> <span data-ttu-id="f7641-579">違いは、これらは。</span><span class="sxs-lookup"><span data-stu-id="f7641-579">The differences are these:</span></span>

- <span data-ttu-id="f7641-580">*GridView*コントロール-指定できます 1 つまたは複数の列の名前、データ ソースで、クライアント Id を作成する実行時に結合されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-580">*GridView* control — You can specify the name of one or more columns in the data source, which are combined at run time to create the client IDs.</span></span> <span data-ttu-id="f7641-581">例では、設定した場合の*RowClientIDRowSuffix* "ProductName、ProductId"をレンダリングする要素は、次のような形式になっているために、コントロール Id:</span><span class="sxs-lookup"><span data-stu-id="f7641-581">For example, if you set *RowClientIDRowSuffix* to "ProductName, ProductId", control IDs for rendered elements will have a format like the following:</span></span>

- [!code-console[Main](overview/samples/sample54.cmd)]

- <span data-ttu-id="f7641-582">*ListView*コントロール-クライアント ID に追加されるデータ ソースの 1 つの列を指定することができます</span><span class="sxs-lookup"><span data-stu-id="f7641-582">*ListView* control — You can specify a single column in the data source that is appended to the client ID.</span></span> <span data-ttu-id="f7641-583">たとえば、設定した場合*ClientIDRowSuffix* "ProductName"に表示されるコントロール Id の次のような形式はなります。</span><span class="sxs-lookup"><span data-stu-id="f7641-583">For example, if you set *ClientIDRowSuffix* to "ProductName", the rendered control IDs will have a format like the following:</span></span>

- [!code-console[Main](overview/samples/sample55.cmd)]

- <span data-ttu-id="f7641-584">ここで、末尾の 1 は現在のデータ項目の製品 ID から派生します。</span><span class="sxs-lookup"><span data-stu-id="f7641-584">In this case the trailing 1 is derived from the product ID of the current data item.</span></span>

- <span data-ttu-id="f7641-585">*Repeater*コントロール-このコントロールはサポートしていません、 *ClientIDRowSuffix*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="f7641-585">*Repeater* control— This control does not support the *ClientIDRowSuffix* property.</span></span> <span data-ttu-id="f7641-586">*Repeater*コントロール、現在の行のインデックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-586">In a *Repeater* control, the index of the current row is used.</span></span> <span data-ttu-id="f7641-587">ClientIDMode を使用すると =「予測可能」、 *Repeater*を制御する次の形式になっているクライアント Id が生成されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-587">When you use ClientIDMode="Predictable" with a *Repeater* control, client IDs are generated that have the following format:</span></span>

- [!code-console[Main](overview/samples/sample56.cmd)]

- <span data-ttu-id="f7641-588">末尾の 0 は、現在の行のインデックスです。</span><span class="sxs-lookup"><span data-stu-id="f7641-588">The trailing 0 is the index of the current row.</span></span>

<span data-ttu-id="f7641-589">*FormView*と*DetailsView*コントロール表示しない複数の行をサポートしていないため、 *ClientIDRowSuffix*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="f7641-589">The *FormView* and *DetailsView* controls do not display multiple rows, so they do not support the *ClientIDRowSuffix* property.</span></span>

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a><span data-ttu-id="f7641-590">データ コントロールで保持する行の選択</span><span class="sxs-lookup"><span data-stu-id="f7641-590">Persisting Row Selection in Data Controls</span></span>

<span data-ttu-id="f7641-591">*GridView*と*ListView*コントロールを使用することができますが、行の選択ができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-591">The *GridView* and *ListView* controls can let users select a row.</span></span> <span data-ttu-id="f7641-592">ASP.NET の以前のバージョンでの選択はページで行のインデックスに基づいています。</span><span class="sxs-lookup"><span data-stu-id="f7641-592">In previous versions of ASP.NET, selection has been based on the row index on the page.</span></span> <span data-ttu-id="f7641-593">たとえば、1 ページ目では、3 番目の項目を選択して、2 ページ目に移動する場合、そのページでは、3 番目の項目が選択されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-593">For example, if you select the third item on page 1 and then move to page 2, the third item on that page is selected.</span></span>

<span data-ttu-id="f7641-594">永続化された選択は、.NET Framework 3.5 SP1 での動的なデータ プロジェクトでのみ最初にサポートされていました。</span><span class="sxs-lookup"><span data-stu-id="f7641-594">Persisted selection was initially supported only in Dynamic Data projects in the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="f7641-595">この機能を有効にすると、現在の選択項目は、項目のデータ キーに基づきます。</span><span class="sxs-lookup"><span data-stu-id="f7641-595">When this feature is enabled, the current selected item is based on the data key for the item.</span></span> <span data-ttu-id="f7641-596">これは、ページ 1 では、3 番目の行を選択してページ 2 に移動する場合は nothing が選択されているページ 2 ことを意味します。</span><span class="sxs-lookup"><span data-stu-id="f7641-596">This means that if you select the third row on page 1 and move to page 2, nothing is selected on page 2.</span></span> <span data-ttu-id="f7641-597">1 ページに戻ると、3 番目の行が選択されたままです。</span><span class="sxs-lookup"><span data-stu-id="f7641-597">When you move back to page 1, the third row is still selected.</span></span> <span data-ttu-id="f7641-598">永続化された選択範囲がサポートされているようになりました、 *GridView*と*ListView*コントロールを使用してすべてのプロジェクトで、 *EnablePersistedSelection*プロパティのように、次の例:</span><span class="sxs-lookup"><span data-stu-id="f7641-598">Persisted selection is now supported for the *GridView* and *ListView* controls in all projects by using the *EnablePersistedSelection* property, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a><span data-ttu-id="f7641-599">ASP.NET のグラフ コントロール</span><span class="sxs-lookup"><span data-stu-id="f7641-599">ASP.NET Chart Control</span></span>

<span data-ttu-id="f7641-600">ASP.NET*グラフ*コントロールは、.NET Framework でのデータ視覚化製品を拡張します。</span><span class="sxs-lookup"><span data-stu-id="f7641-600">The ASP.NET *Chart* control expands the data-visualization offerings in the .NET Framework.</span></span> <span data-ttu-id="f7641-601">使用して、*グラフ*コントロール、複雑な統計や財務分析のための直感的で視覚的に説得力のあるグラフがある ASP.NET ページを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-601">Using the *Chart* control, you can create ASP.NET pages that have intuitive and visually compelling charts for complex statistical or financial analysis.</span></span> <span data-ttu-id="f7641-602">ASP.NET*グラフ*コントロール アドオンとして .NET Framework version 3.5 SP1 のリリースに導入され、は、.NET Framework 4 のリリースの一部です。</span><span class="sxs-lookup"><span data-stu-id="f7641-602">The ASP.NET *Chart* control was introduced as an add-on to the .NET Framework version 3.5 SP1 release and is part of the .NET Framework 4 release.</span></span>

<span data-ttu-id="f7641-603">コントロールには、次の機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f7641-603">The control includes the following features:</span></span>

- <span data-ttu-id="f7641-604">35 種類のグラフ</span><span class="sxs-lookup"><span data-stu-id="f7641-604">35 distinct chart types.</span></span>
- <span data-ttu-id="f7641-605">グラフ領域、タイトル、凡例、および注釈の無制限の数。</span><span class="sxs-lookup"><span data-stu-id="f7641-605">An unlimited number of chart areas, titles, legends, and annotations.</span></span>
- <span data-ttu-id="f7641-606">さまざまなすべてのグラフ要素の外観設定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-606">A wide variety of appearance settings for all chart elements.</span></span>
- <span data-ttu-id="f7641-607">ほとんどのグラフの種類の 3-D のサポート。</span><span class="sxs-lookup"><span data-stu-id="f7641-607">3-D support for most chart types.</span></span>
- <span data-ttu-id="f7641-608">スマート データ ラベルをデータ ポイントの周りに自動的に調整できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-608">Smart data labels that can automatically fit around data points.</span></span>
- <span data-ttu-id="f7641-609">背景の縞模様、スケールの区切り、対数スケール化します。</span><span class="sxs-lookup"><span data-stu-id="f7641-609">Strip lines, scale breaks, and logarithmic scaling.</span></span>
- <span data-ttu-id="f7641-610">データの分析と変換に使用できる 50 を超える財務式および統計式。</span><span class="sxs-lookup"><span data-stu-id="f7641-610">More than 50 financial and statistical formulas for data analysis and transformation.</span></span>
- <span data-ttu-id="f7641-611">単純なバインド、およびグラフ データの操作。</span><span class="sxs-lookup"><span data-stu-id="f7641-611">Simple binding and manipulation of chart data.</span></span>
- <span data-ttu-id="f7641-612">日付、時刻、通貨などの一般的なデータ形式をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f7641-612">Support for common data formats such as dates, times, and currency.</span></span>
- <span data-ttu-id="f7641-613">対話機能とクライアントを含む、イベント ドリブンのカスタマイズのサポートは、Ajax を使用してイベントをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f7641-613">Support for interactivity and event-driven customization, including client click events using Ajax.</span></span>
- <span data-ttu-id="f7641-614">状態管理。</span><span class="sxs-lookup"><span data-stu-id="f7641-614">State management.</span></span>
- <span data-ttu-id="f7641-615">バイナリ ストリーム転送。</span><span class="sxs-lookup"><span data-stu-id="f7641-615">Binary streaming.</span></span>

<span data-ttu-id="f7641-616">次の図は、ASP.NET グラフ コントロールによって生成される財務グラフの例を紹介します。</span><span class="sxs-lookup"><span data-stu-id="f7641-616">The following figures show examples of financial charts that are produced by the ASP.NET Chart control.</span></span>

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

<span data-ttu-id="f7641-617">図 2: ASP.NET のグラフ コントロールの例</span><span class="sxs-lookup"><span data-stu-id="f7641-617">Figure 2: ASP.NET Chart control examples</span></span>

<span data-ttu-id="f7641-618">ASP.NET グラフ コントロールを使用する方法の例についてをサンプル コードをダウンロード、 [Microsoft グラフ コントロールのサンプル環境](https://go.microsoft.com/fwlink/?LinkId=128300)MSDN Web サイトのページ。</span><span class="sxs-lookup"><span data-stu-id="f7641-618">For more examples of how to use the ASP.NET Chart control, download the sample code on the [Samples Environment for Microsoft Chart Controls](https://go.microsoft.com/fwlink/?LinkId=128300) page on the MSDN Web site.</span></span> <span data-ttu-id="f7641-619">コンテンツにコミュニティの他のサンプルを見つけることができます、 [Chart Control Forum](https://go.microsoft.com/fwlink/?LinkId=128713)します。</span><span class="sxs-lookup"><span data-stu-id="f7641-619">You can find more samples of community content at the [Chart Control Forum](https://go.microsoft.com/fwlink/?LinkId=128713).</span></span>

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a><span data-ttu-id="f7641-620">ASP.NET ページにグラフ コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="f7641-620">Adding the Chart Control to an ASP.NET Page</span></span>

<span data-ttu-id="f7641-621">次の例は、追加する方法を示します、*グラフ*マークアップを使用して、ASP.NET ページを制御します。</span><span class="sxs-lookup"><span data-stu-id="f7641-621">The following example shows how to add a *Chart* control to an ASP.NET page by using markup.</span></span> <span data-ttu-id="f7641-622">この例で、*グラフ*コントロールが静的なデータ ポイントの縦棒グラフを生成します。</span><span class="sxs-lookup"><span data-stu-id="f7641-622">In the example, the *Chart* control produces a column chart for static data points.</span></span>

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a><span data-ttu-id="f7641-623">3-D グラフの使用</span><span class="sxs-lookup"><span data-stu-id="f7641-623">Using 3-D Charts</span></span>

<span data-ttu-id="f7641-624">*グラフ*コントロールが含まれています、 *ChartAreas*コレクションを含めることができる*ChartArea*グラフ エリアの特性を定義するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f7641-624">The *Chart* control contains a *ChartAreas* collection, which can contain *ChartArea* objects that define characteristics of chart areas.</span></span> <span data-ttu-id="f7641-625">たとえば、使用して 3-D グラフ エリアを使用する、 *Area3DStyle*例を次のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f7641-625">For example, to use 3-D for a chart area, use the *Area3DStyle* property as in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample59.aspx)]

<span data-ttu-id="f7641-626">次の図は、4 つの系列を 3-D グラフを示しています、*バー*グラフの種類。</span><span class="sxs-lookup"><span data-stu-id="f7641-626">The figure below shows a 3-D chart with four series of the *Bar* chart type.</span></span>

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

<span data-ttu-id="f7641-627">図 3: 3-D 横棒グラフ</span><span class="sxs-lookup"><span data-stu-id="f7641-627">Figure 3: 3-D Bar Chart</span></span>

#### <a name="using-scale-breaks-and-logarithmic-scales"></a><span data-ttu-id="f7641-628">スケール区切り、対数スケールを使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-628">Using Scale Breaks and Logarithmic Scales</span></span>

<span data-ttu-id="f7641-629">対数スケールのスケール区切りは、洗練されたグラフを追加する、2 つの追加方法です。</span><span class="sxs-lookup"><span data-stu-id="f7641-629">Scale breaks and logarithmic scales are two additional ways to add sophistication to the chart.</span></span> <span data-ttu-id="f7641-630">これらの機能は、グラフ領域内の各軸に固有です。</span><span class="sxs-lookup"><span data-stu-id="f7641-630">These features are specific to each axis in a chart area.</span></span> <span data-ttu-id="f7641-631">たとえば、グラフ領域の主軸の Y 軸にこれらの機能を使用する次のように使用します。、 *AxisY.IsLogarithmic*と*ScaleBreakStyle*でプロパティを*ChartArea*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f7641-631">For example, to use these features on the primary Y axis of a chart area, use the *AxisY.IsLogarithmic* and *ScaleBreakStyle* properties in a *ChartArea* object.</span></span> <span data-ttu-id="f7641-632">次のスニペットでは、主軸の Y 軸のスケール区切りを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-632">The following snippet shows how to use scale breaks on the primary Y axis.</span></span>

[!code-aspx[Main](overview/samples/sample60.aspx)]

<span data-ttu-id="f7641-633">次の図は、スケール区切りを有効になっていると、Y 軸を示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-633">The figure below shows the Y axis with scale breaks enabled.</span></span>

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

<span data-ttu-id="f7641-634">図 4: スケール区切り</span><span class="sxs-lookup"><span data-stu-id="f7641-634">Figure 4: Scale Breaks</span></span>

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a><span data-ttu-id="f7641-635">による QueryExtender コントロールでデータをフィルター処理</span><span class="sxs-lookup"><span data-stu-id="f7641-635">Filtering Data with the QueryExtender Control</span></span>

<span data-ttu-id="f7641-636">データ駆動型 Web ページを作成する開発者向けの非常に一般的なタスクでは、データをフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="f7641-636">A very common task for developers who create data-driven Web pages is to filter data.</span></span> <span data-ttu-id="f7641-637">これは、従来によって実行されたビルド*場所*句では、データ ソース コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-637">This traditionally has been performed by building *Where* clauses in data source controls.</span></span> <span data-ttu-id="f7641-638">この方法は複雑になることができ、場合によっては、*場所*構文では、基になるデータベースのすべての機能を利用することはできません。</span><span class="sxs-lookup"><span data-stu-id="f7641-638">This approach can be complicated, and in some cases the *Where* syntax does not let you take advantage of the full functionality of the underlying database.</span></span>

<span data-ttu-id="f7641-639">新しいフィルターを容易にする*による QueryExtender*コントロールが ASP.NET 4 で追加されました。</span><span class="sxs-lookup"><span data-stu-id="f7641-639">To make filtering easier, a new *QueryExtender* control has been added in ASP.NET 4.</span></span> <span data-ttu-id="f7641-640">このコントロールに追加できます*EntityDataSource*または*LinqDataSource*これらのコントロールによって返されるデータをフィルター処理するためにコントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-640">This control can be added to *EntityDataSource* or *LinqDataSource* controls in order to filter the data returned by these controls.</span></span> <span data-ttu-id="f7641-641">*による QueryExtender*コントロールは、LINQ では、非常に効率的な操作の結果 ページにデータが送信される前に、データベース サーバーで、フィルターが適用されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-641">Because the *QueryExtender* control relies on LINQ, the filter is applied on the database server before the data is sent to the page, which results in very efficient operations.</span></span>

<span data-ttu-id="f7641-642">*による QueryExtender*コントロールは、さまざまなフィルター オプションをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="f7641-642">The *QueryExtender* control supports a variety of filter options.</span></span> <span data-ttu-id="f7641-643">次のセクションでは、これらのオプションについて説明し、その使用方法の例を示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-643">The following sections describe these options and provide examples of how to use them.</span></span>

#### <a name="search"></a><span data-ttu-id="f7641-644">検索</span><span class="sxs-lookup"><span data-stu-id="f7641-644">Search</span></span>

<span data-ttu-id="f7641-645">検索オプションで、*による QueryExtender*コントロールは、指定したフィールドに検索を実行します。</span><span class="sxs-lookup"><span data-stu-id="f7641-645">For the search option, the *QueryExtender* control performs a search in specified fields.</span></span> <span data-ttu-id="f7641-646">次の例では、コントロールは TextBoxSearch コントロールし、その内容の検索に入力したテキストを使用して、`ProductName`と`Supplier.CompanyName`から返されるデータの列、 *LinqDataSource*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-646">In the following example, the control uses the text that is entered in the TextBoxSearch control and searches for its contents in the `ProductName` and `Supplier.CompanyName` columns in the data that is returned from the *LinqDataSource* control.</span></span>

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a><span data-ttu-id="f7641-647">範囲</span><span class="sxs-lookup"><span data-stu-id="f7641-647">Range</span></span>

<span data-ttu-id="f7641-648">範囲のオプションは、検索オプションに似ていますが、範囲を定義する値のペアを指定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-648">The range option is similar to the search option, but specifies a pair of values to define the range.</span></span> <span data-ttu-id="f7641-649">次の例では、*による QueryExtender*検索を制御、`UnitPrice`から返されるデータの列、 *LinqDataSource*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-649">In the following example, the *QueryExtender* control searches the `UnitPrice` column in the data returned from the *LinqDataSource* control.</span></span> <span data-ttu-id="f7641-650">範囲は、ページ上の TextBoxFrom と TextBoxTo コントロールから読み取られます。</span><span class="sxs-lookup"><span data-stu-id="f7641-650">The range is read from the TextBoxFrom and TextBoxTo controls on the page.</span></span>

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a><span data-ttu-id="f7641-651">PropertyExpression</span><span class="sxs-lookup"><span data-stu-id="f7641-651">PropertyExpression</span></span>

<span data-ttu-id="f7641-652">プロパティ式のオプションを使用して、プロパティの値の比較を定義できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-652">The property expression option lets you define a comparison to a property value.</span></span> <span data-ttu-id="f7641-653">式が評価された場合*true*、検査されているデータが返されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-653">If the expression evaluates to *true*, the data that is being examined is returned.</span></span> <span data-ttu-id="f7641-654">次の例では、*による QueryExtender*コントロール内のデータを比較することでデータをフィルター処理、`Discontinued`ページ上の CheckBoxDiscontinued コントロールからの列には、値。</span><span class="sxs-lookup"><span data-stu-id="f7641-654">In the following example, the *QueryExtender* control filters data by comparing the data in the `Discontinued` column to the value from the CheckBoxDiscontinued control on the page.</span></span>

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a><span data-ttu-id="f7641-655">CustomExpression</span><span class="sxs-lookup"><span data-stu-id="f7641-655">CustomExpression</span></span>

<span data-ttu-id="f7641-656">最後で使用するカスタム式を指定することができます、*による QueryExtender*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-656">Finally, you can specify a custom expression to use with the *QueryExtender* control.</span></span> <span data-ttu-id="f7641-657">このオプションにより、カスタム フィルターのロジックを定義するページの関数を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-657">This option lets you call a function in the page that defines custom filter logic.</span></span> <span data-ttu-id="f7641-658">次の例は、宣言でカスタム式を指定する方法を示します、*による QueryExtender*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-658">The following example shows how to declaratively specify a custom expression in the *QueryExtender* control.</span></span>

[!code-aspx[Main](overview/samples/sample64.aspx)]

<span data-ttu-id="f7641-659">次の例は、カスタム関数によって呼び出される、*による QueryExtender*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-659">The following example shows the custom function that is invoked by the *QueryExtender* control.</span></span> <span data-ttu-id="f7641-660">含むデータベース クエリを使用する代わりに、ここで、*場所*句、コードを使用して LINQ クエリ、データをフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="f7641-660">In this case, instead of using a database query that includes a *Where* clause, the code uses a LINQ query to filter the data.</span></span>

[!code-csharp[Main](overview/samples/sample65.cs)]

<span data-ttu-id="f7641-661">これらの例で使用されている 1 つだけの式の表示、*による QueryExtender*一度にコントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-661">These examples show only one expression being used in the *QueryExtender* control at a time.</span></span> <span data-ttu-id="f7641-662">ただし、内部の複数の式を含めることができます、*による QueryExtender*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-662">However, you can include multiple expressions inside the *QueryExtender* control.</span></span>

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a><span data-ttu-id="f7641-663">Html エンコードのコード式</span><span class="sxs-lookup"><span data-stu-id="f7641-663">Html Encoded Code Expressions</span></span>

<span data-ttu-id="f7641-664">使用してに大きく依存します (特に、ASP.NET MVC) の一部の ASP.NET サイト`<%` =  `expression %>`構文 (「コード ナゲット」と呼ばれる多くの場合)、応答にいくつかのテキストを書き込む。</span><span class="sxs-lookup"><span data-stu-id="f7641-664">Some ASP.NET sites (especially with ASP.NET MVC) rely heavily on using `<%`= `expression %>` syntax (often called "code nuggets") to write some text to the response.</span></span> <span data-ttu-id="f7641-665">コード式を使用する場合は、HTML エンコード (クロス サイト スクリプティング) XSS 攻撃できるページを開いたままに、ユーザーから、テキストの場合は、テキストを入力し忘れないように簡単です。</span><span class="sxs-lookup"><span data-stu-id="f7641-665">When you use code expressions, it is easy to forget to HTML-encode the text, If the text comes from user input, it can leave pages open to an XSS (Cross Site Scripting) attack.</span></span>

<span data-ttu-id="f7641-666">ASP.NET 4 には、式のコードは、次の新しい構文が導入されています。</span><span class="sxs-lookup"><span data-stu-id="f7641-666">ASP.NET 4 introduces the following new syntax for code expressions:</span></span>

[!code-aspx[Main](overview/samples/sample66.aspx)]

<span data-ttu-id="f7641-667">この構文では、応答に書き込むときに、既定では HTML エンコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-667">This syntax uses HTML encoding by default when writing to the response.</span></span> <span data-ttu-id="f7641-668">この新しい式は、次を効果的に変換します。</span><span class="sxs-lookup"><span data-stu-id="f7641-668">This new expression effectively translates to the following:</span></span>

[!code-aspx[Main](overview/samples/sample67.aspx)]

<span data-ttu-id="f7641-669">たとえば、 &lt;%: 要求 ["UserInput"] %&gt; HTML エンコードの値を実行します*要求 ["UserInput"]* します。</span><span class="sxs-lookup"><span data-stu-id="f7641-669">For example, &lt;%: Request["UserInput"] %&gt; performs HTML encoding on the value of *Request["UserInput"]*.</span></span>

<span data-ttu-id="f7641-670">この機能の目的は、新しい構文を使用する各手順で決定する強制されないように、古い構文のすべてのインスタンスを置換することを可能にです。</span><span class="sxs-lookup"><span data-stu-id="f7641-670">The goal of this feature is to make it possible to replace all instances of the old syntax with the new syntax so that you are not forced to decide at every step which one to use.</span></span> <span data-ttu-id="f7641-671">ただしが出力されるテキストを HTML にするものではまたは既にエンコードされている場合にダブル エンコードする場合これ可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-671">However, there are cases in which the text being output is meant to be HTML or is already encoded, in which case this could lead to double encoding.</span></span>

<span data-ttu-id="f7641-672">ASP.NET 4 のような場合には、新しいインターフェイスが導入されています*IHtmlString*、具体的な実装と共に*HtmlString*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-672">For those cases, ASP.NET 4 introduces a new interface, *IHtmlString*, along with a concrete implementation, *HtmlString*.</span></span> <span data-ttu-id="f7641-673">これらの型のインスタンスでは、そのため、値がされないこと HTML エンコードされたもう一度と、戻り値は既に正しくエンコードされて (またはそれ以外の場合)、HTML として表示するためを指定できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-673">Instances of these types let you indicate that the return value is already properly encoded (or otherwise examined) for displaying as HTML, and that therefore the value should not be HTML-encoded again.</span></span> <span data-ttu-id="f7641-674">たとえば、次することはできません (とは) でエンコードされた HTML:</span><span class="sxs-lookup"><span data-stu-id="f7641-674">For example, the following should not be (and is not) HTML encoded:</span></span>

[!code-aspx[Main](overview/samples/sample68.aspx)]

<span data-ttu-id="f7641-675">ASP.NET MVC 2 のヘルパー メソッドは、ASP.NET 4 を実行しているときに更新されるのでない二重エンコードされる場合は、この新しい構文を使用するが、唯一されています。</span><span class="sxs-lookup"><span data-stu-id="f7641-675">ASP.NET MVC 2 helper methods have been updated to work with this new syntax so that they are not double encoded, but only when you are running ASP.NET 4.</span></span> <span data-ttu-id="f7641-676">この新しい構文では、ASP.NET 3.5 SP1 を使用してアプリケーションを実行するときに機能しません。</span><span class="sxs-lookup"><span data-stu-id="f7641-676">This new syntax does not work when you run an application using ASP.NET 3.5 SP1.</span></span>

<span data-ttu-id="f7641-677">XSS 攻撃からの保護は保証されないことに留意してください。</span><span class="sxs-lookup"><span data-stu-id="f7641-677">Keep in mind that this does not guarantee protection from XSS attacks.</span></span> <span data-ttu-id="f7641-678">たとえば、引用符ではありません。 属性値を使用する HTML は、引き続き受けやすいユーザー入力を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-678">For example, HTML that uses attribute values that are not in quotation marks can contain user input that is still susceptible.</span></span> <span data-ttu-id="f7641-679">ASP.NET コントロールと ASP.NET MVC ヘルパーの出力常に含まれている属性値、引用符では、推奨される方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="f7641-679">Note that the output of ASP.NET controls and ASP.NET MVC helpers always includes attribute values in quotation marks, which is the recommended approach.</span></span>

<span data-ttu-id="f7641-680">同様に、この構文では、ユーザー入力に基づいて JavaScript 文字列を作成する場合など、JavaScript のエンコーディングは行いません。</span><span class="sxs-lookup"><span data-stu-id="f7641-680">Likewise, this syntax does not perform JavaScript encoding, such as when you create a JavaScript string based on user input.</span></span>

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a><span data-ttu-id="f7641-681">プロジェクト テンプレートの変更</span><span class="sxs-lookup"><span data-stu-id="f7641-681">Project Template Changes</span></span>

<span data-ttu-id="f7641-682">以前のバージョンの ASP.NET では、Visual Studio を使用して新しい Web サイト プロジェクトまたは Web アプリケーション プロジェクトを作成する場合、結果として得られるプロジェクトが含まれますのみ、Default.aspx ページで、既定の`Web.config`ファイル、および`App_Data`では、次に示すように、フォルダー図:</span><span class="sxs-lookup"><span data-stu-id="f7641-682">In earlier versions of ASP.NET, when you use Visual Studio to create a new Web Site project or Web Application project, the resulting projects contain only a Default.aspx page, a default `Web.config` file, and the `App_Data` folder, as shown in the following illustration:</span></span>

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

<span data-ttu-id="f7641-683">Visual Studio には、すべての次の図に示すようにファイルが含まれない空の Web サイト プロジェクト型もサポートしています。</span><span class="sxs-lookup"><span data-stu-id="f7641-683">Visual Studio also supports an Empty Web Site project type, which contains no files at all, as shown in the following figure:</span></span>

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

<span data-ttu-id="f7641-684">初心者にとって非常に指針はほとんどありません、運用環境の Web アプリケーションを構築する方法になります。</span><span class="sxs-lookup"><span data-stu-id="f7641-684">The result is that for the beginner, there is very little guidance on how to build a production Web application.</span></span> <span data-ttu-id="f7641-685">そのため、ASP.NET 4 は、次の 3 つの新しいテンプレート、空の Web アプリケーション プロジェクトと Web アプリケーションと Web サイト プロジェクトに対して 1 つずつについて説明します。</span><span class="sxs-lookup"><span data-stu-id="f7641-685">Therefore, ASP.NET 4 introduces three new templates, one for an empty Web application project, and one each for a Web Application and Web Site project.</span></span>

#### <a name="empty-web-application-template"></a><span data-ttu-id="f7641-686">空の Web アプリケーション テンプレート</span><span class="sxs-lookup"><span data-stu-id="f7641-686">Empty Web Application Template</span></span>

<span data-ttu-id="f7641-687">名前からわかるように、空の Web アプリケーション テンプレートはたしか Web アプリケーション プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="f7641-687">As the name suggests, the Empty Web Application template is a stripped-down Web Application project.</span></span> <span data-ttu-id="f7641-688">このプロジェクト テンプレートは、次の図に示すように、Visual Studio の新しいプロジェクト ダイアログ ボックスから選択します。</span><span class="sxs-lookup"><span data-stu-id="f7641-688">You select this project template from the Visual Studio New Project dialog box, as shown in the following figure:</span></span>

[![](overview/_static/image7.png)](overview/_static/image6.png)

<span data-ttu-id="f7641-689">([フルサイズの画像を表示する をクリックします](overview/_static/image8.png))。</span><span class="sxs-lookup"><span data-stu-id="f7641-689">([Click to view full-size image](overview/_static/image8.png))</span></span>

<span data-ttu-id="f7641-690">空の ASP.NET Web アプリケーションを作成するときに、Visual Studio には、次のフォルダー レイアウトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-690">When you create an Empty ASP.NET Web Application, Visual Studio creates the following folder layout:</span></span>

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

<span data-ttu-id="f7641-691">これは、例外が 1 つの ASP.NET の以前のバージョンから空の Web サイトのレイアウトに似ています。</span><span class="sxs-lookup"><span data-stu-id="f7641-691">This is similar to the Empty Web Site layout from earlier versions of ASP.NET, with one exception.</span></span> <span data-ttu-id="f7641-692">Visual Studio 2010 では、空の Web アプリケーションと空の Web サイト プロジェクトが含まれている、次の最小限`Web.config`Visual Studio でプロジェクトのターゲット フレームワークを識別するために使用される情報を含むファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-692">In Visual Studio 2010, Empty Web Application and Empty Web Site projects contain the following minimal `Web.config` file that contains information used by Visual Studio to identify the framework that the project is targeting:</span></span>

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

<span data-ttu-id="f7641-693">これを行わない*targetFramework*プロパティ、Visual Studio の既定値は、古いアプリケーションを開くときに、互換性を維持するために、.NET Framework 2.0 を対象とします。</span><span class="sxs-lookup"><span data-stu-id="f7641-693">Without this *targetFramework* property, Visual Studio defaults to targeting the .NET Framework 2.0 in order to preserve compatibility when opening older applications.</span></span>

#### <a name="web-application-and-web-site-project-templates"></a><span data-ttu-id="f7641-694">Web アプリケーションや Web サイト プロジェクト テンプレート</span><span class="sxs-lookup"><span data-stu-id="f7641-694">Web Application and Web Site Project Templates</span></span>

<span data-ttu-id="f7641-695">その他の 2 つ新しいプロジェクト テンプレートが Visual Studio 2010 に付属するには、大幅な変更が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f7641-695">The other two new project templates that are shipped with Visual Studio 2010 contain major changes.</span></span> <span data-ttu-id="f7641-696">次の図は、新しい Web アプリケーション プロジェクトを作成するときに作成されるプロジェクト レイアウトを示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-696">The following figure shows the project layout that is created when you create a new Web Application project.</span></span> <span data-ttu-id="f7641-697">(Web サイト プロジェクトのレイアウトは、事実上同じです)。</span><span class="sxs-lookup"><span data-stu-id="f7641-697">(The layout for a Web Site project is virtually identical.)</span></span>

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

<span data-ttu-id="f7641-698">プロジェクトには、多数以前のバージョンで作成されていないファイルにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f7641-698">The project includes a number of files that were not created in earlier versions.</span></span> <span data-ttu-id="f7641-699">さらに、新しい Web アプリケーション プロジェクトは、すぐに新しいアプリケーションへのアクセスをセキュリティで保護することで開始することができますが、基本メンバーシップ機能で構成されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-699">In addition, the new Web Application project is configured with basic membership functionality, which lets you quickly get started in securing access to the new application.</span></span> <span data-ttu-id="f7641-700">、これらを含めることにより、`Web.config`ファイルの新しいプロジェクトにはメンバーシップ、ロール、およびプロファイルを構成するために使用するエントリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f7641-700">Because of this inclusion, the `Web.config` file for the new project includes entries that are used to configure membership, roles, and profiles.</span></span> <span data-ttu-id="f7641-701">次の例は、`Web.config`新しい Web アプリケーション プロジェクトのファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-701">The following example shows the `Web.config` file for a new Web Application project.</span></span> <span data-ttu-id="f7641-702">(この場合、 *roleManager*は無効です)。</span><span class="sxs-lookup"><span data-stu-id="f7641-702">(In this case, *roleManager* is disabled.)</span></span>

[![](overview/_static/image13.png)](overview/_static/image12.png)

<span data-ttu-id="f7641-703">([フルサイズの画像を表示する をクリックします](overview/_static/image14.png))。</span><span class="sxs-lookup"><span data-stu-id="f7641-703">([Click to view full-size image](overview/_static/image14.png))</span></span>

<span data-ttu-id="f7641-704">プロジェクトは、2 つ目も含まれています。`Web.config`ファイル、`Account`ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="f7641-704">The project also contains a second `Web.config` file in the `Account` directory.</span></span> <span data-ttu-id="f7641-705">2 番目の構成ファイルは、ログに記録されないユーザーで ChangePassword.aspx のページへのアクセスをセキュリティで保護する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="f7641-705">The second configuration file provides a way to secure access to the ChangePassword.aspx page for non-logged in users.</span></span> <span data-ttu-id="f7641-706">次の例は、2 つ目の内容を示しています。`Web.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-706">The following example shows the contents of the second `Web.config` file.</span></span>

![](overview/_static/image15.png)

<span data-ttu-id="f7641-707">既定では、新しいプロジェクト テンプレートで作成されたページには、以前のバージョンよりもより多くのコンテンツも含まれます。</span><span class="sxs-lookup"><span data-stu-id="f7641-707">The pages created by default in the new project templates also contain more content than in previous versions.</span></span> <span data-ttu-id="f7641-708">プロジェクトには、既定のマスター ページと CSS ファイルが含まれていて、既定のページ (Default.aspx) が既定でマスター ページを使用するよう構成します。</span><span class="sxs-lookup"><span data-stu-id="f7641-708">The project contains a default master page and CSS file, and the default page (Default.aspx) is configured to use the master page by default.</span></span> <span data-ttu-id="f7641-709">初めての Web アプリケーションまたは Web サイトを実行すると、既定のページ (ホーム) を機能が既にになります。</span><span class="sxs-lookup"><span data-stu-id="f7641-709">The result is that when you run the Web application or Web site for the first time, the default (home) page is already functional.</span></span> <span data-ttu-id="f7641-710">実際には、新しい MVC アプリケーションを起動するときに表示する既定のページに似ています。</span><span class="sxs-lookup"><span data-stu-id="f7641-710">In fact, it is similar to the default page you see when you start up a new MVC application.</span></span>

[![](overview/_static/image17.png)](overview/_static/image16.png)

<span data-ttu-id="f7641-711">([フルサイズの画像を表示する をクリックします](overview/_static/image18.png))。</span><span class="sxs-lookup"><span data-stu-id="f7641-711">([Click to view full-size image](overview/_static/image18.png))</span></span>

<span data-ttu-id="f7641-712">これらの変更をプロジェクト テンプレートの目的は、新しい Web アプリケーションの構築を開始する方法のガイダンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="f7641-712">The intention of these changes to the project templates is to provide guidance on how to start building a new Web application.</span></span> <span data-ttu-id="f7641-713">意味的に正しく、厳密な XHTML 1.0 に準拠したマークアップと CSS を使用して指定されているレイアウト、テンプレート内のページは ASP.NET 4 Web アプリケーションを構築するためのベスト プラクティスを表します。</span><span class="sxs-lookup"><span data-stu-id="f7641-713">With semantically correct, strict XHTML 1.0-compliant markup and with layout that is specified using CSS, the pages in the templates represent best practices for building ASP.NET 4 Web applications.</span></span> <span data-ttu-id="f7641-714">既定のページには、簡単にカスタマイズ可能な 2 つの列のレイアウトもがあります。</span><span class="sxs-lookup"><span data-stu-id="f7641-714">The default pages also have a two-column layout that you can easily customize.</span></span>

<span data-ttu-id="f7641-715">たとえば、場合を考えます新しい Web アプリケーションの一部の色を変更し、My ASP.NET Application ロゴの代わりに、会社のロゴを挿入します。</span><span class="sxs-lookup"><span data-stu-id="f7641-715">For example, imagine that for a new Web Application you want to change some of the colors and insert your company logo in place of the My ASP.NET Application logo.</span></span> <span data-ttu-id="f7641-716">新しいディレクトリを作成するには、`Content`ロゴ イメージを格納します。</span><span class="sxs-lookup"><span data-stu-id="f7641-716">To do this, you create a new directory under `Content` to store your logo image:</span></span>

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

<span data-ttu-id="f7641-717">ページには、イメージを追加するを開く、`Site.Master`ファイル、My ASP.NET Application テキストが定義され、置き換え、それを検索、*イメージ*要素が*src*属性がこの新しいロゴに設定次の例に示すの画像:</span><span class="sxs-lookup"><span data-stu-id="f7641-717">To add the image to the page, you then open the `Site.Master` file, find where the My ASP.NET Application text is defined, and replace it with an *image* element whose *src* attribute is set to the new logo image, as in the following example:</span></span>

[![](overview/_static/image21.png)](overview/_static/image20.png)

<span data-ttu-id="f7641-718">([フルサイズの画像を表示する をクリックします](overview/_static/image22.png))。</span><span class="sxs-lookup"><span data-stu-id="f7641-718">([Click to view full-size image](overview/_static/image22.png))</span></span>

<span data-ttu-id="f7641-719">Site.css ファイルに移動し、ページの背景色とのヘッダーを変更する CSS クラスの定義を変更します。</span><span class="sxs-lookup"><span data-stu-id="f7641-719">You can then go into the Site.css file and modify CSS class definitions to change the background color of the page as well as that of the header.</span></span>

<span data-ttu-id="f7641-720">これらの変更の結果を非常に簡単にカスタマイズされたホーム ページを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-720">The result of these changes is that you can display a customized home page with very little effort:</span></span>

[![](overview/_static/image24.png)](overview/_static/image23.png)

<span data-ttu-id="f7641-721">([フルサイズの画像を表示する をクリックします](overview/_static/image25.png))。</span><span class="sxs-lookup"><span data-stu-id="f7641-721">([Click to view full-size image](overview/_static/image25.png))</span></span>

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a><span data-ttu-id="f7641-722">CSS の改良点</span><span class="sxs-lookup"><span data-stu-id="f7641-722">CSS Improvements</span></span>

<span data-ttu-id="f7641-723">ASP.NET 4 での作業の主要な分野の 1 つは、最新の HTML 標準に準拠している HTML のレンダリングを支援するようにしています。</span><span class="sxs-lookup"><span data-stu-id="f7641-723">One of the major areas of work in ASP.NET 4 has been to help render HTML that is compliant with the latest HTML standards.</span></span> <span data-ttu-id="f7641-724">これには、ASP.NET Web サーバー コントロールで CSS スタイルを使用する方法の変更が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f7641-724">This includes changes to how ASP.NET Web server controls use CSS styles.</span></span>

#### <a name="compatibility-setting-for-rendering"></a><span data-ttu-id="f7641-725">レンダリングの互換性設定</span><span class="sxs-lookup"><span data-stu-id="f7641-725">Compatibility Setting for Rendering</span></span>

<span data-ttu-id="f7641-726">Web アプリケーションまたは Web サイトは、.NET Framework 4 を対象とした場合、既定、 *controlRenderingCompatibilityVersion*の属性、*ページ*要素が「4.0」に設定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-726">By default, when a Web application or Web site targets the .NET Framework 4, the *controlRenderingCompatibilityVersion* attribute of the *pages* element is set to "4.0".</span></span> <span data-ttu-id="f7641-727">この要素がコンピューター レベルで定義されている`Web.config`ファイルし、既定ですべての ASP.NET 4 アプリケーションに適用されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-727">This element is defined in the machine-level `Web.config` file and by default applies to all ASP.NET 4 applications:</span></span>

[!code-xml[Main](overview/samples/sample69.xml)]

<span data-ttu-id="f7641-728">値は、 *controlRenderingCompatibility*は潜在的な新しいバージョンの定義は今後のリリースにより、文字列です。</span><span class="sxs-lookup"><span data-stu-id="f7641-728">The value for *controlRenderingCompatibility* is a string, which allows potential new version definitions in future releases.</span></span> <span data-ttu-id="f7641-729">現在のリリースでは、次の値がこのプロパティのサポートされています。</span><span class="sxs-lookup"><span data-stu-id="f7641-729">In the current release, the following values are supported for this property:</span></span>

- <span data-ttu-id="f7641-730">"3.5".</span><span class="sxs-lookup"><span data-stu-id="f7641-730">"3.5".</span></span> <span data-ttu-id="f7641-731">この設定は、従来のレンダリングとマークアップを示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-731">This setting indicates legacy rendering and markup.</span></span> <span data-ttu-id="f7641-732">コントロールによってレンダリングされるマークアップが 100% の旧バージョンと互換性のあるの設定、 *xhtmlConformance*プロパティが受け入れられます。</span><span class="sxs-lookup"><span data-stu-id="f7641-732">Markup rendered by controls is 100% backward compatible, and the setting of the *xhtmlConformance* property is honored.</span></span>
- <span data-ttu-id="f7641-733">"4.0".</span><span class="sxs-lookup"><span data-stu-id="f7641-733">"4.0".</span></span> <span data-ttu-id="f7641-734">プロパティにこの設定がある場合は、ASP.NET Web サーバー コントロールは、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="f7641-734">If the property has this setting, ASP.NET Web server controls do the following:</span></span>
- <span data-ttu-id="f7641-735">*XhtmlConformance*プロパティが常に"Strict"として扱われます。</span><span class="sxs-lookup"><span data-stu-id="f7641-735">The *xhtmlConformance* property is always treated as "Strict".</span></span> <span data-ttu-id="f7641-736">その結果、コントロールは、XHTML 1.0 Strict マークアップをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="f7641-736">As a result, controls render XHTML 1.0 Strict markup.</span></span>
- <span data-ttu-id="f7641-737">不要になった非入力コントロールを無効にするには、無効なスタイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-737">Disabling non-input controls no longer renders invalid styles.</span></span>
- <span data-ttu-id="f7641-738">*div* CSS 規則のユーザーが作成したこれらの影響を与えないように、非表示フィールドを囲む要素はスタイリングようになりました。</span><span class="sxs-lookup"><span data-stu-id="f7641-738">*div* elements around hidden fields are now styled so they do not interfere with user-created CSS rules.</span></span>
- <span data-ttu-id="f7641-739">メニュー コントロールは、意味的に正しいとアクセシビリティのガイドラインに準拠しているマークアップをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="f7641-739">Menu controls render markup that is semantically correct and compliant with accessibility guidelines.</span></span>
- <span data-ttu-id="f7641-740">検証コントロールでは、インライン スタイルはレンダリングされません。</span><span class="sxs-lookup"><span data-stu-id="f7641-740">Validation controls do not render inline styles.</span></span>
- <span data-ttu-id="f7641-741">以前の境界線を描画するコントロール =「0」(ASP.NET から派生したコントロール*テーブル*制御、および ASP.NET*イメージ*コントロール) この属性は表示されなくします。</span><span class="sxs-lookup"><span data-stu-id="f7641-741">Controls that previously rendered border="0" (controls that derive from the ASP.NET *Table* control, and the ASP.NET *Image* control) no longer render this attribute.</span></span>

#### <a name="disabling-controls"></a><span data-ttu-id="f7641-742">コントロールを無効にします。</span><span class="sxs-lookup"><span data-stu-id="f7641-742">Disabling Controls</span></span>

<span data-ttu-id="f7641-743">ASP.NET 3.5 SP1 およびそれ以前のバージョンでは、フレームワークがレンダリング、*無効になっている*そのコントロールの HTML マークアップ属性*有効*プロパティに設定*false*。</span><span class="sxs-lookup"><span data-stu-id="f7641-743">In ASP.NET 3.5 SP1 and earlier versions, the framework renders the *disabled* attribute in the HTML markup for any control whose *Enabled* property set to *false*.</span></span> <span data-ttu-id="f7641-744">ただし、HTML 4.01 仕様のみに従って*入力*要素は、この属性を持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-744">However, according to the HTML 4.01 specification, only *input* elements should have this attribute.</span></span>

<span data-ttu-id="f7641-745">ASP.NET 4 で設定することができます、 *controlRenderingCompatabilityVersion*プロパティを次の例のように「3.5」。</span><span class="sxs-lookup"><span data-stu-id="f7641-745">In ASP.NET 4, you can set the *controlRenderingCompatabilityVersion* property to "3.5", as in the following example:</span></span>

[!code-xml[Main](overview/samples/sample70.xml)]

<span data-ttu-id="f7641-746">用のマークアップを作成する場合があります、*ラベル*コントロールを無効にすると、次のようなコントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-746">You might create markup for a *Label* control like the following, which disables the control:</span></span>

[!code-aspx[Main](overview/samples/sample71.aspx)]

<span data-ttu-id="f7641-747">*ラベル*コントロールは、次の HTML をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="f7641-747">The *Label* control would render the following HTML:</span></span>

[!code-html[Main](overview/samples/sample72.html)]

<span data-ttu-id="f7641-748">ASP.NET 4 で設定することができます、 *controlRenderingCompatabilityVersion* 「4.0」にします。</span><span class="sxs-lookup"><span data-stu-id="f7641-748">In ASP.NET 4, you can set the *controlRenderingCompatabilityVersion* to "4.0".</span></span> <span data-ttu-id="f7641-749">その場合は、そのレンダリングを制御するだけ*入力*要素が表示されます、*無効になっている*属性の場合に、コントロールの*有効*プロパティに設定されて*false*.</span><span class="sxs-lookup"><span data-stu-id="f7641-749">In that case, only controls that render *input* elements will render a *disabled* attribute when the control's *Enabled* property is set to *false*.</span></span> <span data-ttu-id="f7641-750">HTML にレンダリングされないコントロール*入力*要素をレンダリングする*クラス*無効になっているコントロールの外観を定義に使用できる CSS クラスを参照する属性。</span><span class="sxs-lookup"><span data-stu-id="f7641-750">Controls that do not render HTML *input* elements instead render a *class* attribute that references a CSS class that you can use to define a disabled look for the control.</span></span> <span data-ttu-id="f7641-751">たとえば、*ラベル*前の例に示すようにコントロールが次のマークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="f7641-751">For example, the *Label* control shown in the earlier example would generate the following markup:</span></span>

[!code-html[Main](overview/samples/sample73.html)]

<span data-ttu-id="f7641-752">このコントロールに指定するクラスの既定値は、"aspNetDisabled"です。</span><span class="sxs-lookup"><span data-stu-id="f7641-752">The default value for the class that specified for this control is "aspNetDisabled".</span></span> <span data-ttu-id="f7641-753">ただし、この既定値を変更するには、静的な*DisabledCssClass*の静的プロパティ、 *WebControl*クラス。</span><span class="sxs-lookup"><span data-stu-id="f7641-753">However, you can change this default value by setting the static *DisabledCssClass* static property of the *WebControl* class.</span></span> <span data-ttu-id="f7641-754">コントロール開発者は、特定のコントロールを使用する動作も定義できますを使用して、 *SupportsDisabledAttribute*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="f7641-754">For control developers, the behavior to use for a specific control can also be defined using the *SupportsDisabledAttribute* property.</span></span>

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a><span data-ttu-id="f7641-755">Div を非表示フィールドの周囲の要素を非表示</span><span class="sxs-lookup"><span data-stu-id="f7641-755">Hiding div Elements Around Hidden Fields</span></span>

<span data-ttu-id="f7641-756">ASP.NET 2.0 およびそれ以降のバージョンがシステムに固有の非表示フィールドを表示 (など、*隠し*ビュー状態情報を格納するための要素) 内で*div* XHTML 標準に準拠するために要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-756">ASP.NET 2.0 and later versions render system-specific hidden fields (such as the *hidden* element used to store view state information) inside *div* element in order to comply with the XHTML standard.</span></span> <span data-ttu-id="f7641-757">ただし、この問題が発生 CSS 規則の影響を与える場合*div*ページ上の要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-757">However, this can cause a problem when a CSS rule affects *div* elements on a page.</span></span> <span data-ttu-id="f7641-758">たとえば、ページに表示されている 1 ピクセルの行でそのよう非表示に約*div*要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-758">For example, it can result in a one-pixel line appearing in the page around hidden *div* elements.</span></span> <span data-ttu-id="f7641-759">ASP.NET 4 で*div* ASP.NET によって生成された非表示フィールドを囲む要素は、次の例のように CSS クラスの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="f7641-759">In ASP.NET 4, *div* elements that enclose the hidden fields generated by ASP.NET add a CSS class reference as in the following example:</span></span>

[!code-html[Main](overview/samples/sample74.html)]

<span data-ttu-id="f7641-760">のみに適用される CSS クラスを定義することができますし、*隠し*次の例のように、ASP.NET によって生成される要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-760">You can then define a CSS class that applies only to the *hidden* elements that are generated by ASP.NET, as in the following example:</span></span>

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a><span data-ttu-id="f7641-761">テンプレート化されたコントロールの外側のテーブルを表示</span><span class="sxs-lookup"><span data-stu-id="f7641-761">Rendering an Outer Table for Templated Controls</span></span>

<span data-ttu-id="f7641-762">既定では、テンプレートをサポートする次の ASP.NET Web サーバー コントロールが、インライン スタイルを適用するために使用する外部テーブルで自動的にラップします。</span><span class="sxs-lookup"><span data-stu-id="f7641-762">By default, the following ASP.NET Web server controls that support templates are automatically wrapped in an outer table that is used to apply inline styles:</span></span>

- <span data-ttu-id="f7641-763">*FormView*</span><span class="sxs-lookup"><span data-stu-id="f7641-763">*FormView*</span></span>
- <span data-ttu-id="f7641-764">*Login*</span><span class="sxs-lookup"><span data-stu-id="f7641-764">*Login*</span></span>
- <span data-ttu-id="f7641-765">*PasswordRecovery*</span><span class="sxs-lookup"><span data-stu-id="f7641-765">*PasswordRecovery*</span></span>
- <span data-ttu-id="f7641-766">*ChangePassword*</span><span class="sxs-lookup"><span data-stu-id="f7641-766">*ChangePassword*</span></span>
- <span data-ttu-id="f7641-767">*ウィザード*</span><span class="sxs-lookup"><span data-stu-id="f7641-767">*Wizard*</span></span>
- <span data-ttu-id="f7641-768">*CreateUserWizard*</span><span class="sxs-lookup"><span data-stu-id="f7641-768">*CreateUserWizard*</span></span>

<span data-ttu-id="f7641-769">という名前の新しいプロパティ*RenderOuterTable*マークアップから削除する外部テーブルは、これらのコントロールに追加されました。</span><span class="sxs-lookup"><span data-stu-id="f7641-769">A new property named *RenderOuterTable* has been added to these controls that allows the outer table to be removed from the markup.</span></span> <span data-ttu-id="f7641-770">たとえば、次の例を*FormView*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-770">For example, consider the following example of a *FormView* control:</span></span>

[!code-aspx[Main](overview/samples/sample76.aspx)]

<span data-ttu-id="f7641-771">このマークアップは、HTML テーブルを含むページには、次の出力を表示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-771">This markup renders the following output to the page, which includes an HTML table:</span></span>

[!code-html[Main](overview/samples/sample77.html)]

<span data-ttu-id="f7641-772">テーブルが表示されていることを防ぐために設定することができます、 *FormView*コントロールの*RenderOuterTable*例を次のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f7641-772">To prevent the table from being rendered, you can set the *FormView* control's *RenderOuterTable* property, as in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample78.aspx)]

<span data-ttu-id="f7641-773">前の例では、次の出力、なしのレンダリング、*テーブル*、 *tr*、および*td*要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-773">The previous example renders the following output, without the *table*, *tr*, and *td* elements:</span></span>

> <span data-ttu-id="f7641-774">Content</span><span class="sxs-lookup"><span data-stu-id="f7641-774">Content</span></span>


<span data-ttu-id="f7641-775">この機能強化できますやすくスタイル、CSS を使用してコントロールのコンテンツのため、コントロールによって予期しないタグは表示されません。</span><span class="sxs-lookup"><span data-stu-id="f7641-775">This enhancement can make it easier to style the content of the control with CSS, because no unexpected tags are rendered by the control.</span></span>

> [!NOTE]
> <span data-ttu-id="f7641-776">注この変更が不要になったため、デザイナーの Visual Studio 2010 でのオート フォーマット関数のサポートを無効にする*テーブル*オート フォーマット オプションによって生成されるスタイル属性をホストできる要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-776">Note This change disables support for the auto-format function in the Visual Studio 2010 designer, because there is no longer a *table* element that can host style attributes that are generated by the auto-format option.</span></span>


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a><span data-ttu-id="f7641-777">ListView コントロールの機能強化</span><span class="sxs-lookup"><span data-stu-id="f7641-777">ListView Control Enhancements</span></span>

<span data-ttu-id="f7641-778">*ListView*コントロールを ASP.NET 4 で使いやすくなりました。</span><span class="sxs-lookup"><span data-stu-id="f7641-778">The *ListView* control has been made easier to use in ASP.NET 4.</span></span> <span data-ttu-id="f7641-779">コントロールの以前のバージョンの既知の ID を持つサーバー コントロールに含まれているレイアウト テンプレートを指定することが必要です。</span><span class="sxs-lookup"><span data-stu-id="f7641-779">The earlier version of the control required that you specify a layout template that contained a server control with a known ID.</span></span> <span data-ttu-id="f7641-780">次のマークアップを使用する方法の典型的な例を示しています、 *ListView* ASP.NET 3.5 のコントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-780">The following markup shows a typical example of how to use the *ListView* control in ASP.NET 3.5.</span></span>

[!code-aspx[Main](overview/samples/sample79.aspx)]

<span data-ttu-id="f7641-781">ASP.NET 4 で、 *ListView*コントロールでは、レイアウト テンプレートは必要はありません。</span><span class="sxs-lookup"><span data-stu-id="f7641-781">In ASP.NET 4, the *ListView* control does not require a layout template.</span></span> <span data-ttu-id="f7641-782">前の例に示すようにマークアップは、次のマークアップで置換できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-782">The markup shown in the previous example can be replaced with the following markup:</span></span>

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a><span data-ttu-id="f7641-783">CheckBoxList、RadioButtonList コントロールの強化</span><span class="sxs-lookup"><span data-stu-id="f7641-783">CheckBoxList and RadioButtonList Control Enhancements</span></span>

<span data-ttu-id="f7641-784">ASP.NET 3.5 では、レイアウトを指定することができます、 *CheckBoxList*と*RadioButtonList*次の 2 つの設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-784">In ASP.NET 3.5, you can specify layout for the *CheckBoxList* and *RadioButtonList* using the following two settings:</span></span>

- <span data-ttu-id="f7641-785">*フロー*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-785">*Flow*.</span></span> <span data-ttu-id="f7641-786">コントロールのレンダリング*span*そのコンテンツを格納する要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-786">The control renders *span* elements to contain its content.</span></span>
- <span data-ttu-id="f7641-787">*テーブル*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-787">*Table*.</span></span> <span data-ttu-id="f7641-788">コントロールのレンダリングを*テーブル*そのコンテンツを格納する要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-788">The control renders a *table* element to contain its content.</span></span>

<span data-ttu-id="f7641-789">次の例は、これらのコントロールのマークアップを示しています。</span><span class="sxs-lookup"><span data-stu-id="f7641-789">The following example shows markup for each of these controls.</span></span>

[!code-aspx[Main](overview/samples/sample81.aspx)]

<span data-ttu-id="f7641-790">既定では、コントロールをレンダリング HTML、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="f7641-790">By default, the controls render HTML similar to the following:</span></span>

[!code-html[Main](overview/samples/sample82.html)]

<span data-ttu-id="f7641-791">これらのコントロールには、意味的に正しくの HTML を表示するために、項目の一覧が含まれているために、HTML リストを使用してその内容をレンダリングする必要があります (*li*) 要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-791">Because these controls contain lists of items, to render semantically correct HTML, they should render their contents using HTML list (*li*) elements.</span></span> <span data-ttu-id="f7641-792">これにより、簡単に支援のテクノロジを使用して Web ページが読み取られ、コントロールを簡単にスタイルが CSS を使用しているユーザー。</span><span class="sxs-lookup"><span data-stu-id="f7641-792">This makes it easier for users who read Web pages using assistive technology, and makes the controls easier to style using CSS.</span></span>

<span data-ttu-id="f7641-793">ASP.NET 4 で、 *CheckBoxList*と*RadioButtonList*コントロールに次の新しい値をサポートする、*この*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="f7641-793">In ASP.NET 4, the *CheckBoxList* and *RadioButtonList* controls support the following new values for the *RepeatLayout* property:</span></span>

- <span data-ttu-id="f7641-794">*OrderedList* – としてコンテンツがレンダリングされる*li*内の要素、 *ol*要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-794">*OrderedList* – The content is rendered as *li* elements within an *ol* element.</span></span>
- <span data-ttu-id="f7641-795">*UnorderedList* – としてコンテンツがレンダリングされる*li*内の要素を*ul*要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-795">*UnorderedList* – The content is rendered as *li* elements within a *ul* element.</span></span>

<span data-ttu-id="f7641-796">次の例では、これらの新しい値を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-796">The following example shows how to use these new values.</span></span>

[!code-aspx[Main](overview/samples/sample83.aspx)]

<span data-ttu-id="f7641-797">上記のマークアップでは、次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-797">The preceding markup generates the following HTML:</span></span>

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> <span data-ttu-id="f7641-798">設定するかどうかに注意してください*この*に*OrderedList*または*UnorderedList*、 *RepeatDirection*プロパティが使用できなくできますとは。マークアップまたはコード内でプロパティが設定されている場合は、実行時に例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="f7641-798">Note If you set *RepeatLayout* to *OrderedList* or *UnorderedList*, the *RepeatDirection* property can no longer be used and will throw an exception at run time if the property has been set within your markup or code.</span></span> <span data-ttu-id="f7641-799">プロパティがない値 CSS を代わりを使用してこれらのコントロールのレイアウトが定義されているためです。</span><span class="sxs-lookup"><span data-stu-id="f7641-799">The property would have no value because the visual layout of these controls is defined using CSS instead.</span></span>


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a><span data-ttu-id="f7641-800">メニュー コントロールの機能強化</span><span class="sxs-lookup"><span data-stu-id="f7641-800">Menu Control Improvements</span></span>

<span data-ttu-id="f7641-801">ASP.NET 4 では、前に、*メニュー*コントロールは、一連の HTML テーブルを表示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-801">Before ASP.NET 4, the *Menu* control rendered a series of HTML tables.</span></span> <span data-ttu-id="f7641-802">これにより、インライン プロパティの設定の外部での CSS スタイルを適用する複数が困難し、ユーザー補助の標準に準拠してでしたも。</span><span class="sxs-lookup"><span data-stu-id="f7641-802">This made it more difficult to apply CSS styles outside of setting inline properties and was also not compliant with accessibility standards.</span></span>

<span data-ttu-id="f7641-803">ASP.NET 4 で、コントロールはこれで順不同のリストとリストの要素で構成されるセマンティック マークアップを使用して HTML をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="f7641-803">In ASP.NET 4, the control now renders HTML using semantic markup that consists of an unordered list and list elements.</span></span> <span data-ttu-id="f7641-804">次の例では、ASP.NET ページ用のマークアップを示しています、*メニュー*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-804">The following example shows markup in an ASP.NET page for the *Menu* control.</span></span>

[!code-aspx[Main](overview/samples/sample85.aspx)]

<span data-ttu-id="f7641-805">コントロールが、次の HTML を生成するページが表示される、(、 *onclick*コードがわかりやすくするために省略されています)。</span><span class="sxs-lookup"><span data-stu-id="f7641-805">When the page renders, the control produces the following HTML (the *onclick* code has been omitted for clarity):</span></span>

[!code-html[Main](overview/samples/sample86.html)]

<span data-ttu-id="f7641-806">レンダリング機能強化に加え、フォーカス管理を使用して、メニューのキーボード ナビゲーションを向上しています。</span><span class="sxs-lookup"><span data-stu-id="f7641-806">In addition to rendering improvements, keyboard navigation of the menu has been improved using focus management.</span></span> <span data-ttu-id="f7641-807">ときに、*メニュー*コントロールがフォーカスを取得、方向キーを使用して要素を移動することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-807">When the *Menu* control gets focus, you can use the arrow keys to navigate elements.</span></span> <span data-ttu-id="f7641-808">*メニュー*コントロールはもアクセス可能なリッチ インターネット applications (ARIA) の役割をアタッチし、follo 属性[翼、](http://www.w3.org/TR/wai-aria-practices/#menu "メニュー ARIA ガイドライン")を向上させるためアクセシビリティ。</span><span class="sxs-lookup"><span data-stu-id="f7641-808">The *Menu* control also now attaches accessible rich internet applications (ARIA) roles and attributes follo[wing the](http://www.w3.org/TR/wai-aria-practices/#menu "Menu ARIA guidelines")for improved accessibility.</span></span>

<span data-ttu-id="f7641-809">上部にあるスタイル ブロックに沿ってレンダリングされる HTML 要素ではなく、ページのでは、メニュー コントロールのスタイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-809">Styles for the menu control are rendered in a style block at the top of the page, rather than in line with the rendered HTML elements.</span></span> <span data-ttu-id="f7641-810">新しい設定することができる場合、コントロールのスタイル設定を完全に制御を実行するには、 *IncludeStyleBlock*プロパティを*false*、スタイル ブロックが出力されない場合。</span><span class="sxs-lookup"><span data-stu-id="f7641-810">If you want to take full control over the styling for the control, you can set the new *IncludeStyleBlock* property to *false*, in which case the style block is not emitted.</span></span> <span data-ttu-id="f7641-811">このプロパティを使用する方法の 1 つ、メニューの外観を設定する、Visual Studio デザイナーで、オート フォーマットの機能を使用することです。</span><span class="sxs-lookup"><span data-stu-id="f7641-811">One way to use this property is to use the auto-format feature in the Visual Studio designer to set the appearance of the menu.</span></span> <span data-ttu-id="f7641-812">ページの実行、ページのソースを開く、および外部 CSS ファイルに表示されるスタイルのブロックをコピーし、ことができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-812">You can then run the page, open the page source, and then copy the rendered style block to an external CSS file.</span></span> <span data-ttu-id="f7641-813">Visual Studio で、スタイルと設定を元に戻す*IncludeStyleBlock*に*false*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-813">In Visual Studio, undo the styling and set *IncludeStyleBlock* to *false*.</span></span> <span data-ttu-id="f7641-814">外部スタイル シートのスタイルを使用して、メニューの外観を定義することになります。</span><span class="sxs-lookup"><span data-stu-id="f7641-814">The result is that the menu appearance is defined using styles in an external style sheet.</span></span>

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a><span data-ttu-id="f7641-815">ウィザードおよび CreateUserWizard コントロール</span><span class="sxs-lookup"><span data-stu-id="f7641-815">Wizard and CreateUserWizard Controls</span></span>

<span data-ttu-id="f7641-816">ASP.NET*ウィザード*と*CreateUserWizard*コントロールがレンダリングする HTML を定義するためのテンプレートをサポートします。</span><span class="sxs-lookup"><span data-stu-id="f7641-816">The ASP.NET *Wizard* and *CreateUserWizard* controls support templates that let you define the HTML that they render.</span></span> <span data-ttu-id="f7641-817">(*CreateUserWizard*から派生した*ウィザード*)。次の例では、完全なテンプレートのマークアップ*CreateUserWizard*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-817">(*CreateUserWizard* derives from *Wizard*.) The following example shows the markup for a fully templated *CreateUserWizard* control:</span></span>

[!code-aspx[Main](overview/samples/sample87.aspx)]

<span data-ttu-id="f7641-818">コントロールは、次のような HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-818">The control renders HTML similar to the following:</span></span>

[!code-html[Main](overview/samples/sample88.html)]

<span data-ttu-id="f7641-819">ASP.NET 3.5 sp1 では、テンプレートの内容を変更できますが、まだ制限されている場合の出力を制御、*ウィザード*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-819">In ASP.NET 3.5 SP1, although you can change the template contents, you still have limited control over the output of the *Wizard* control.</span></span> <span data-ttu-id="f7641-820">ASP.NET 4 で作成することができます、 *LayoutTemplate*テンプレートおよび insert*プレース ホルダー* (予約済みの名前を使用) を制御する方法を指定する、*ウィザード コントロール*をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="f7641-820">In ASP.NET 4, you can create a *LayoutTemplate* template and insert *PlaceHolder* controls (using reserved names) to specify how you want the *Wizard control* to render.</span></span> <span data-ttu-id="f7641-821">次の例では、これは示しています。</span><span class="sxs-lookup"><span data-stu-id="f7641-821">The following example shows this:</span></span>

[!code-aspx[Main](overview/samples/sample89.aspx)]

<span data-ttu-id="f7641-822">例では、名前付きプレース ホルダーで、次が含まれています、 *LayoutTemplate*要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-822">The example contains the following named placeholders in the *LayoutTemplate* element:</span></span>

- <span data-ttu-id="f7641-823">*headerPlaceholder* – の内容で置き換えられます、実行時に、*使って HeaderTemplate*要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-823">*headerPlaceholder* – At run time, this is replaced by the contents of the *HeaderTemplate* element.</span></span>
- <span data-ttu-id="f7641-824">*sideBarPlaceholder* – の内容で置き換えられます、実行時に、 *SideBarTemplate*要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-824">*sideBarPlaceholder* – At run time, this is replaced by the contents of the *SideBarTemplate* element.</span></span>
- <span data-ttu-id="f7641-825">*wizardStepPlaceHolder* – の内容で置き換えられます、実行時に、 *WizardStepTemplate*要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-825">*wizardStepPlaceHolder* – At run time, this is replaced by the contents of the *WizardStepTemplate* element.</span></span>
- <span data-ttu-id="f7641-826">*navigationPlaceholder* – 実行時に定義されているナビゲーション テンプレートで置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="f7641-826">*navigationPlaceholder* – At run time, this is replaced by any navigation templates that you have defined.</span></span>

<span data-ttu-id="f7641-827">プレース ホルダーを使用する例のマークアップでは、次の HTML をレンダリングします (実際には、テンプレートで定義されているコンテンツ) なし。</span><span class="sxs-lookup"><span data-stu-id="f7641-827">The markup in the example that uses placeholders renders the following HTML (without the content actually defined in the templates):</span></span>

[!code-html[Main](overview/samples/sample90.html)]

<span data-ttu-id="f7641-828">ここで定義されていないユーザーの唯一の HTML は、 *span*要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-828">The only HTML that is now not user-defined is a *span* element.</span></span> <span data-ttu-id="f7641-829">(後のリリースでは、予想も、 *span*要素は表示されません)。これでは、によって生成されるほぼすべてのコンテンツを完全に制御を提供する、*ウィザード*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-829">(We anticipate that in future releases, even the *span* element will not be rendered.) This now gives you full control over virtually all the content that is generated by the *Wizard* control.</span></span>

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a><span data-ttu-id="f7641-830">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f7641-830">ASP.NET MVC</span></span>

<span data-ttu-id="f7641-831">ASP.NET MVC は、2009 年 3 月で、アドオン、フレームワークとして、ASP.NET 3.5 SP1 を導入されました。</span><span class="sxs-lookup"><span data-stu-id="f7641-831">ASP.NET MVC was introduced as an add-on framework to ASP.NET 3.5 SP1 in March 2009.</span></span> <span data-ttu-id="f7641-832">Visual Studio 2010 には、ASP.NET MVC 2 にには新しい機能や機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f7641-832">Visual Studio 2010 includes ASP.NET MVC 2, which includes new features and capabilities.</span></span>

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a><span data-ttu-id="f7641-833">領域のサポート</span><span class="sxs-lookup"><span data-stu-id="f7641-833">Areas Support</span></span>

<span data-ttu-id="f7641-834">領域使用するグループのコント ローラーとビューを他のセクションから相対的に分離で大規模なアプリケーションのセクションにできます。</span><span class="sxs-lookup"><span data-stu-id="f7641-834">Areas let you group controllers and views into sections of a large application in relative isolation from other sections.</span></span> <span data-ttu-id="f7641-835">各領域は、メイン アプリケーションによって参照し、可能な個別の ASP.NET MVC プロジェクトとして実装できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-835">Each area can be implemented as a separate ASP.NET MVC project that can then be referenced by the main application.</span></span> <span data-ttu-id="f7641-836">これにより、大規模なアプリケーションをビルドするときの複雑さし、1 つのアプリケーションで連携して動作する複数のチームで簡単になります。</span><span class="sxs-lookup"><span data-stu-id="f7641-836">This helps manage complexity when you build a large application and makes it easier for multiple teams to work together on a single application.</span></span>

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a><span data-ttu-id="f7641-837">データ注釈属性の検証のサポート</span><span class="sxs-lookup"><span data-stu-id="f7641-837">Data-Annotation Attribute Validation Support</span></span>

<span data-ttu-id="f7641-838">*DataAnnotations*属性を使用して、メタデータ属性を使用して、モデルに検証ロジックを適用できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-838">*DataAnnotations* attributes let you attach validation logic to a model by using metadata attributes.</span></span> <span data-ttu-id="f7641-839">*DataAnnotations*属性は、ASP.NET 3.5 SP1 での ASP.NET Dynamic Data で導入されました。</span><span class="sxs-lookup"><span data-stu-id="f7641-839">*DataAnnotations* attributes were introduced in ASP.NET Dynamic Data in ASP.NET 3.5 SP1.</span></span> <span data-ttu-id="f7641-840">これらの属性は、既定のモデル バインダーに統合されており、ユーザー入力を検証するメタデータ ドリブン手段を提供します。</span><span class="sxs-lookup"><span data-stu-id="f7641-840">These attributes have been integrated into the default model binder and provide a metadata-driven means to validate user input.</span></span>

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a><span data-ttu-id="f7641-841">テンプレート化されたヘルパー</span><span class="sxs-lookup"><span data-stu-id="f7641-841">Templated Helpers</span></span>

<span data-ttu-id="f7641-842">テンプレート化されたヘルパーは、関連付けが自動的に編集することができ、データ型を持つテンプレートを表示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-842">Templated helpers let you automatically associate edit and display templates with data types.</span></span> <span data-ttu-id="f7641-843">たとえばの日付の選択の UI 要素が自動的に表示されることを指定するテンプレート ヘルパーを使用することができます、 *System.DateTime*値。</span><span class="sxs-lookup"><span data-stu-id="f7641-843">For example, you can use a template helper to specify that a date-picker UI element is automatically rendered for a *System.DateTime* value.</span></span> <span data-ttu-id="f7641-844">ASP.NET Dynamic Data でのフィールド テンプレートに似ています。</span><span class="sxs-lookup"><span data-stu-id="f7641-844">This is similar to field templates in ASP.NET Dynamic Data.</span></span>

<span data-ttu-id="f7641-845">*Html.EditorFor*と*Html.DisplayFor*ヘルパー メソッドが複数のプロパティでも複雑なオブジェクト型の標準的なデータを表示、組み込みサポートを備えています。</span><span class="sxs-lookup"><span data-stu-id="f7641-845">The *Html.EditorFor* and *Html.DisplayFor* helper methods have built-in support for rendering standard data types as well as complex objects with multiple properties.</span></span> <span data-ttu-id="f7641-846">などのデータ注釈属性を適用することができますもレンダリングをカスタマイズする*DisplayName*と*ScaffoldColumn*を*ViewModel*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f7641-846">They also customize rendering by letting you apply data-annotation attributes like *DisplayName* and *ScaffoldColumn* to the *ViewModel* object.</span></span>

<span data-ttu-id="f7641-847">多くの場合、さらに UI ヘルパーからの出力をカスタマイズして、生成されたものを完全に制御します。</span><span class="sxs-lookup"><span data-stu-id="f7641-847">Often you want to customize the output from UI helpers even further and have complete control over what is generated.</span></span> <span data-ttu-id="f7641-848">*Html.EditorFor*と*Html.DisplayFor*ヘルパー メソッドをサポートしてこれをオーバーライドできる外部テンプレートとコントロール レンダリングされる出力を定義できるテンプレート メカニズムを使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-848">The *Html.EditorFor* and *Html.DisplayFor* helper methods support this using a templating mechanism that lets you define external templates that can override and control the output rendered.</span></span> <span data-ttu-id="f7641-849">クラスのテンプレートを個別に表示できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-849">The templates can be rendered individually for a class.</span></span>

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a><span data-ttu-id="f7641-850">動的データ</span><span class="sxs-lookup"><span data-stu-id="f7641-850">Dynamic Data</span></span>

<span data-ttu-id="f7641-851">動的なデータが導入された 2008 年の半ばに .NET Framework 3.5 SP1 のリリースでします。</span><span class="sxs-lookup"><span data-stu-id="f7641-851">Dynamic Data was introduced in the .NET Framework 3.5 SP1 release in mid-2008.</span></span> <span data-ttu-id="f7641-852">この機能は、次を含む、データ駆動型アプリケーションを作成するための多くの拡張機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="f7641-852">This feature provides many enhancements for creating data-driven applications, including the following:</span></span>

- <span data-ttu-id="f7641-853">データ ドリブンの Web サイトをすばやく構築するための RAD エクスペリエンス。</span><span class="sxs-lookup"><span data-stu-id="f7641-853">A RAD experience for quickly building a data-driven Web site.</span></span>
- <span data-ttu-id="f7641-854">データ モデルで定義された制約に基づいている自動検証します。</span><span class="sxs-lookup"><span data-stu-id="f7641-854">Automatic validation that is based on constraints defined in the data model.</span></span>
- <span data-ttu-id="f7641-855">内のフィールドに対して生成されるマークアップを簡単に変更する機能、 *GridView*と*DetailsView*動的データ プロジェクトの一部であるフィールド テンプレートを使用してコントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-855">The ability to easily change the markup that is generated for fields in the *GridView* and *DetailsView* controls by using field templates that are part of your Dynamic Data project.</span></span>

> [!NOTE]
> <span data-ttu-id="f7641-856">注詳細についてを参照してください、[動的データ ドキュメント](https://msdn.microsoft.com/library/cc488545.aspx)MSDN ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="f7641-856">Note For more information, see the [Dynamic Data documentation](https://msdn.microsoft.com/library/cc488545.aspx) in the MSDN Library.</span></span>


<span data-ttu-id="f7641-857">ASP.NET 4 では、データ駆動型 Web サイトをすばやく構築するためのさらに多くの電力を開発者に提供する動的なデータが強化されました。</span><span class="sxs-lookup"><span data-stu-id="f7641-857">For ASP.NET 4, Dynamic Data has been enhanced to give developers even more power for quickly building data-driven Web sites.</span></span>

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a><span data-ttu-id="f7641-858">既存のプロジェクトの動的なデータの有効化</span><span class="sxs-lookup"><span data-stu-id="f7641-858">Enabling Dynamic Data for Existing Projects</span></span>

<span data-ttu-id="f7641-859">.NET Framework 3.5 SP1 に同梱されている動的データ機能では、次などの新機能になります。</span><span class="sxs-lookup"><span data-stu-id="f7641-859">Dynamic Data features that shipped in the .NET Framework 3.5 SP1 brought new features such as the following:</span></span>

- <span data-ttu-id="f7641-860">フィールド テンプレート – これらは、データ種類ベースのテンプレートのデータ バインド コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-860">Field templates – These provide data-type-based templates for data-bound controls.</span></span> <span data-ttu-id="f7641-861">フィールド テンプレートには、各フィールドのテンプレートのフィールドを使用するよりもデータ コントロールの外観をカスタマイズする簡単なことができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-861">Field templates provide a simpler way to customize the look of data controls than using template fields for each field.</span></span>
- <span data-ttu-id="f7641-862">検証: 動的なデータでのデータ クラスに属性を使用して、必要なフィールド、範囲のチェック、型チェック、照合、正規表現パターンのような一般的なシナリオの検証とカスタム検証を指定するは。</span><span class="sxs-lookup"><span data-stu-id="f7641-862">Validation – Dynamic Data lets you use attributes on data classes to specify validation for common scenarios like required fields, range checking, type checking, pattern matching using regular expressions, and custom validation.</span></span> <span data-ttu-id="f7641-863">データ コントロールで検証が適用されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-863">Validation is enforced by the data controls.</span></span>

<span data-ttu-id="f7641-864">ただし、これらの機能には、次の要件がありました。</span><span class="sxs-lookup"><span data-stu-id="f7641-864">However, these features had the following requirements:</span></span>

- <span data-ttu-id="f7641-865">データ アクセス層は、Entity Framework や LINQ to SQL に基づく必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-865">The data-access layer had to be based on Entity Framework or LINQ to SQL.</span></span>
- <span data-ttu-id="f7641-866">唯一のデータ ソースのこれらの機能がサポートされているコントロール、 *EntityDataSource*または*LinqDataSource*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-866">The only data source controls supported for these features were the *EntityDataSource* or *LinqDataSource* controls.</span></span>
- <span data-ttu-id="f7641-867">機能が、機能をサポートするために必要なすべてのファイルを確保するために、動的なデータまたは動的データ エンティティのテンプレートを使用して作成した Web プロジェクトが必要です。</span><span class="sxs-lookup"><span data-stu-id="f7641-867">The features required a Web project that had been created using the Dynamic Data or Dynamic Data Entities templates in order to have all the files that were required to support the feature.</span></span>

<span data-ttu-id="f7641-868">ASP.NET 4 での動的なデータのサポートの主な目的では、ASP.NET アプリケーションの動的なデータの新しい機能が有効にします。</span><span class="sxs-lookup"><span data-stu-id="f7641-868">A major goal of Dynamic Data support in ASP.NET 4 is to enable the new functionality of Dynamic Data for any ASP.NET application.</span></span> <span data-ttu-id="f7641-869">次の例では、マークアップを利用して動的データ機能の既存のページでコントロールを示しています。</span><span class="sxs-lookup"><span data-stu-id="f7641-869">The following example shows markup for controls that can take advantage of Dynamic Data functionality in an existing page.</span></span>

[!code-aspx[Main](overview/samples/sample91.aspx)]

<span data-ttu-id="f7641-870">ページのコードでこれらのコントロールの動的なデータ サポートを有効にするには次のコードを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-870">In the code for the page, the following code must be added in order to enable Dynamic Data support for these controls:</span></span>

[!code-csharp[Main](overview/samples/sample92.cs)]

<span data-ttu-id="f7641-871">ときに、 *GridView*コントロールが編集モードでは、入力されたデータが適切な形式である動的なデータが自動的に検証します。</span><span class="sxs-lookup"><span data-stu-id="f7641-871">When the *GridView* control is in edit mode, Dynamic Data automatically validates that the data entered is in the proper format.</span></span> <span data-ttu-id="f7641-872">そうでない場合、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-872">If it is not, an error message is displayed.</span></span>

<span data-ttu-id="f7641-873">この機能は、その他の特典も用意されています。 の値を挿入モードの既定値を指定できるなどです。</span><span class="sxs-lookup"><span data-stu-id="f7641-873">This functionality also provides other benefits, such as being able to specify default values for insert mode.</span></span> <span data-ttu-id="f7641-874">動的なデータがない場合、フィールドの既定値を実装するにはイベントへのアタッチ、コントロールの検索 (を使用して*FindControl*)、その値を設定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-874">Without Dynamic Data, to implement a default value for a field, you must attach to an event, locate the control (using *FindControl*), and set its value.</span></span> <span data-ttu-id="f7641-875">ASP.NET 4 で、 *EnableDynamicData*呼び出しは、この例で示すように、オブジェクトのいずれかのフィールドの既定値を渡すことができますが、2 番目のパラメーターをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="f7641-875">In ASP.NET 4, the *EnableDynamicData* call supports a second parameter that lets you pass default values for any field on the object, as shown in this example:</span></span>

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a><span data-ttu-id="f7641-876">DynamicDataManager コントロールの宣言構文</span><span class="sxs-lookup"><span data-stu-id="f7641-876">Declarative DynamicDataManager Control Syntax</span></span>

<span data-ttu-id="f7641-877">*DynamicDataManager*コントロールは、宣言によって構成できるようにするために強化されていますのコードだけではなく、ASP.NET では、ほとんどのコントロールと同様です。</span><span class="sxs-lookup"><span data-stu-id="f7641-877">The *DynamicDataManager* control has been enhanced so that you can configure it declaratively, as with most controls in ASP.NET, instead of only in code.</span></span> <span data-ttu-id="f7641-878">マークアップを*DynamicDataManager*コントロールは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f7641-878">The markup for the *DynamicDataManager* control looks like the following example:</span></span>

[!code-aspx[Main](overview/samples/sample94.aspx)]

<span data-ttu-id="f7641-879">このマークアップにより、動的データで参照されている GridView1 コントロールの動作、 *DataControls*のセクション、 *DynamicDataManager*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-879">This markup enables Dynamic Data behavior for the GridView1 control that is referenced in the *DataControls* section of the *DynamicDataManager* control.</span></span>

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a><span data-ttu-id="f7641-880">エンティティ テンプレート</span><span class="sxs-lookup"><span data-stu-id="f7641-880">Entity Templates</span></span>

<span data-ttu-id="f7641-881">エンティティ テンプレートは、新しいカスタム ページを作成することがなくデータのレイアウトをカスタマイズする方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="f7641-881">Entity templates offer a new way to customize the layout of data without requiring you to create a custom page.</span></span> <span data-ttu-id="f7641-882">テンプレートの使用のページ、 *FormView*コントロール (の代わりに、 *DetailsView*コントロールが動的なデータの以前のバージョンのページ テンプレートで使用) および*DynamicEntity*エンティティ テンプレートをレンダリングするコントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-882">Page templates use the *FormView* control (instead of the *DetailsView* control, as used in page templates in earlier versions of Dynamic Data) and the *DynamicEntity* control to render Entity templates.</span></span> <span data-ttu-id="f7641-883">これにより、動的なデータによって表示されるマークアップの詳細に制御できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f7641-883">This gives you more control over the markup that is rendered by Dynamic Data.</span></span>

<span data-ttu-id="f7641-884">次に、エンティティのテンプレートを含む新しいプロジェクト ディレクトリのレイアウトを示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-884">The following list shows the new project directory layout that contains entity templates:</span></span>

[!code-console[Main](overview/samples/sample95.cmd)]

<span data-ttu-id="f7641-885">`EntityTemplate`ディレクトリには、データ モデル オブジェクトを表示する方法のテンプレートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f7641-885">The `EntityTemplate` directory contains templates for how to display data model objects.</span></span> <span data-ttu-id="f7641-886">使用して既定では、オブジェクトの表示、`Default.ascx`テンプレートによって作成されたマークアップのような外観のマークアップを提供する、 *DetailsView* ASP.NET 3.5 SP1 での Dynamic Data で使用されるコントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-886">By default, objects are rendered by using the `Default.ascx` template, which provides markup that looks just like the markup created by the *DetailsView* control used by Dynamic Data in ASP.NET 3.5 SP1.</span></span> <span data-ttu-id="f7641-887">次の例のマークアップを示しています、`Default.ascx`コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-887">The following example shows the markup for the `Default.ascx` control:</span></span>

[!code-aspx[Main](overview/samples/sample96.aspx)]

<span data-ttu-id="f7641-888">サイト全体の外観を変更するのには、既定のテンプレートを編集できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-888">The default templates can be edited to change the look and feel for the entire site.</span></span> <span data-ttu-id="f7641-889">表示、編集、および挿入操作のテンプレートがあります。</span><span class="sxs-lookup"><span data-stu-id="f7641-889">There are templates for display, edit, and insert operations.</span></span> <span data-ttu-id="f7641-890">新しいテンプレート オブジェクトの種類を 1 つだけの外観を変更するにがデータ オブジェクトの名前に基づいて追加されたことができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-890">New templates can be added based on the name of the data object in order to change the look and feel of just one type of object.</span></span> <span data-ttu-id="f7641-891">たとえば、次のテンプレートを追加できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-891">For example, you can add the following template:</span></span>

[!code-console[Main](overview/samples/sample97.cmd)]

<span data-ttu-id="f7641-892">テンプレートには、次のマークアップが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f7641-892">The template might contain the following markup:</span></span>

[!code-aspx[Main](overview/samples/sample98.aspx)]

<span data-ttu-id="f7641-893">新しいエンティティ テンプレートが新しいを使用して、ページに表示される*DynamicEntity*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-893">The new entity templates are displayed on a page by using the new *DynamicEntity* control.</span></span> <span data-ttu-id="f7641-894">実行時に、このコントロールは、エンティティ テンプレートの内容に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="f7641-894">At run time, this control is replaced with the contents of the entity template.</span></span> <span data-ttu-id="f7641-895">次のマークアップに示す、 *FormView*を制御、`Detail.aspx`エンティティ テンプレートを使用するページのテンプレート。</span><span class="sxs-lookup"><span data-stu-id="f7641-895">The following markup shows the *FormView* control in the `Detail.aspx` page template that uses the entity template.</span></span> <span data-ttu-id="f7641-896">通知、 *DynamicEntity*マークアップ内の要素。</span><span class="sxs-lookup"><span data-stu-id="f7641-896">Notice the *DynamicEntity* element in the markup.</span></span>

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a><span data-ttu-id="f7641-897">Url および電子メール アドレスの新しいフィールド テンプレート</span><span class="sxs-lookup"><span data-stu-id="f7641-897">New Field Templates for URLs and Email Addresses</span></span>

<span data-ttu-id="f7641-898">ASP.NET 4 では、2 つの新しい組み込みフィールド テンプレート`EmailAddress.ascx`と`Url.ascx`します。</span><span class="sxs-lookup"><span data-stu-id="f7641-898">ASP.NET 4 introduces two new built-in field templates, `EmailAddress.ascx` and `Url.ascx`.</span></span> <span data-ttu-id="f7641-899">これらのテンプレートとしてマークされているフィールドの使用は*EmailAddress*または*Url*で、 *DataType*属性。</span><span class="sxs-lookup"><span data-stu-id="f7641-899">These templates are used for fields that are marked as *EmailAddress* or *Url* with the *DataType* attribute.</span></span> <span data-ttu-id="f7641-900">*EmailAddress*オブジェクトを使用して作成されるハイパーリンクとして、フィールドが表示されます、 *mailto:* プロトコル。</span><span class="sxs-lookup"><span data-stu-id="f7641-900">For *EmailAddress* objects, the field is displayed as a hyperlink that is created by using the *mailto:* protocol.</span></span> <span data-ttu-id="f7641-901">ユーザーがリンクをクリック、ユーザーの電子メール クライアントを開くし、スケルトン メッセージを作成します。</span><span class="sxs-lookup"><span data-stu-id="f7641-901">When users click the link, it opens the user's email client and creates a skeleton message.</span></span> <span data-ttu-id="f7641-902">オブジェクトとして型指定された*Url*は通常のハイパーリンクとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-902">Objects typed as *Url* are displayed as ordinary hyperlinks.</span></span>

<span data-ttu-id="f7641-903">次の例では、フィールドがマークする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f7641-903">The following example shows how fields would be marked.</span></span>

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a><span data-ttu-id="f7641-904">DynamicHyperLink コントロールへのリンクを作成します。</span><span class="sxs-lookup"><span data-stu-id="f7641-904">Creating Links with the DynamicHyperLink Control</span></span>

<span data-ttu-id="f7641-905">動的データでは、エンドユーザーが Web サイトにアクセスするときに表示される Url を制御するには、.NET Framework 3.5 SP1 で追加された新しいルーティング機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-905">Dynamic Data uses the new routing feature that was added in the .NET Framework 3.5 SP1 to control the URLs that end users see when they access the Web site.</span></span> <span data-ttu-id="f7641-906">新しい*DynamicHyperLink*コントロールにより、簡単に動的データ サイト内のページへのリンクを構築できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-906">The new *DynamicHyperLink* control makes it easy to build links to pages in a Dynamic Data site.</span></span> <span data-ttu-id="f7641-907">次の例は、使用する方法を示します、 *DynamicHyperLink*コントロール。</span><span class="sxs-lookup"><span data-stu-id="f7641-907">The following example shows how to use the *DynamicHyperLink* control:</span></span>

[!code-aspx[Main](overview/samples/sample101.aspx)]

<span data-ttu-id="f7641-908">このマークアップのリスト ページを指すリンクが作成、`Products`で定義されているルートに基づいて、テーブル、`Global.asax`ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-908">This markup creates a link that points to the List page for the `Products` table based on routes that are defined in the `Global.asax` file.</span></span> <span data-ttu-id="f7641-909">コントロールは、自動的に既定のテーブル名に基づく動的なデータ ページを使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-909">The control automatically uses the default table name that the Dynamic Data page is based on.</span></span>

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a><span data-ttu-id="f7641-910">データ モデルにおける継承のサポート</span><span class="sxs-lookup"><span data-stu-id="f7641-910">Support for Inheritance in the Data Model</span></span>

<span data-ttu-id="f7641-911">Entity Framework と LINQ to SQL は、そのデータ モデルで継承をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f7641-911">Both the Entity Framework and LINQ to SQL support inheritance in their data models.</span></span> <span data-ttu-id="f7641-912">この例を持つデータベースである可能性があります、`InsurancePolicy`テーブル。</span><span class="sxs-lookup"><span data-stu-id="f7641-912">An example of this might be a database that has an `InsurancePolicy` table.</span></span> <span data-ttu-id="f7641-913">含まれる場合も`CarPolicy`と`HousePolicy`と同じフィールドを持つテーブルを`InsurancePolicy`しより多くのフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="f7641-913">It might also contain `CarPolicy` and `HousePolicy` tables that have the same fields as `InsurancePolicy` and then add more fields.</span></span> <span data-ttu-id="f7641-914">データ モデルで継承されたオブジェクトを理解し、継承されたテーブルのスキャフォールディングをサポートするために、動的なデータが変更されました。</span><span class="sxs-lookup"><span data-stu-id="f7641-914">Dynamic Data has been modified to understand inherited objects in the data model and to support scaffolding for the inherited tables.</span></span>

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a><span data-ttu-id="f7641-915">多対多のリレーションシップ (Entity Framework) のサポート</span><span class="sxs-lookup"><span data-stu-id="f7641-915">Support for Many-to-Many Relationships (Entity Framework Only)</span></span>

<span data-ttu-id="f7641-916">Entity Framework では、テーブルのリレーションシップをコレクションとして公開することで実装されている間に多対多リレーションシップの豊富なサポート、*エンティティ*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f7641-916">The Entity Framework has rich support for many-to-many relationships between tables, which is implemented by exposing the relationship as a collection on an *Entity* object.</span></span> <span data-ttu-id="f7641-917">新しい`ManyToMany.ascx`と`ManyToMany_Edit.ascx`を表示して、多対多のリレーションシップに関係するデータの編集をサポートするフィールド テンプレートが追加されました。</span><span class="sxs-lookup"><span data-stu-id="f7641-917">New `ManyToMany.ascx` and `ManyToMany_Edit.ascx` field templates have been added to provide support for displaying and editing data that is involved in many-to-many relationships.</span></span>

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a><span data-ttu-id="f7641-918">コントロールの表示と列挙のサポートを新しい属性</span><span class="sxs-lookup"><span data-stu-id="f7641-918">New Attributes to Control Display and Support Enumerations</span></span>

<span data-ttu-id="f7641-919">*DisplayAttribute*フィールドの表示方法を細かく制御に追加されました。</span><span class="sxs-lookup"><span data-stu-id="f7641-919">The *DisplayAttribute* has been added to give you additional control over how fields are displayed.</span></span> <span data-ttu-id="f7641-920">*DisplayName*以前のバージョン フィールドのキャプションとして使用される名前を変更することを許可されている動的データの属性。</span><span class="sxs-lookup"><span data-stu-id="f7641-920">The *DisplayName* attribute in earlier versions of Dynamic Data allowed you to change the name that is used as a caption for a field.</span></span> <span data-ttu-id="f7641-921">新しい*DisplayAttribute*クラスを使用して、順序のフィールドを表示およびフィルターとしてフィールドを使用するかどうかなどのフィールドを表示するための他のオプションを指定できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-921">The new *DisplayAttribute* class lets you specify more options for displaying a field, such as the order in which a field is displayed and whether a field will be used as a filter.</span></span> <span data-ttu-id="f7641-922">属性は、独立したコントロールのラベルに使用される名前のも用意されています、 *GridView*コントロールで使用される名前、 *DetailsView*の透かしが使用されると、フィールドのヘルプ テキストを制御します。、。フィールド (フィールドは、テキスト入力を受け入れる) 場合。</span><span class="sxs-lookup"><span data-stu-id="f7641-922">The attribute also provides independent control of the name used for the labels in a *GridView* control, the name used in a *DetailsView* control, the help text for the field, and the watermark used for the field (if the field accepts text input).</span></span>

<span data-ttu-id="f7641-923">*EnumDataTypeAttribute*列挙体にフィールドをマップできるようにクラスが追加されました。</span><span class="sxs-lookup"><span data-stu-id="f7641-923">The *EnumDataTypeAttribute* class has been added to let you map fields to enumerations.</span></span> <span data-ttu-id="f7641-924">フィールドにこの属性を適用する場合は、列挙型を指定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-924">When you apply this attribute to a field, you specify an enumeration type.</span></span> <span data-ttu-id="f7641-925">動的なデータは、新しい`Enumeration.ascx`フィールド テンプレートを表示および編集する列挙値の UI を作成します。</span><span class="sxs-lookup"><span data-stu-id="f7641-925">Dynamic Data uses the new `Enumeration.ascx` field template to create UI for displaying and editing enumeration values.</span></span> <span data-ttu-id="f7641-926">テンプレートは、列挙の名前に、データベースから値をマップします。</span><span class="sxs-lookup"><span data-stu-id="f7641-926">The template maps the values from the database to the names in the enumeration.</span></span>

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a><span data-ttu-id="f7641-927">フィルターのサポートの強化</span><span class="sxs-lookup"><span data-stu-id="f7641-927">Enhanced Support for Filters</span></span>

<span data-ttu-id="f7641-928">動的データ 1.0 は、ブール型の列と外部キー列の組み込みのフィルターに付属します。</span><span class="sxs-lookup"><span data-stu-id="f7641-928">Dynamic Data 1.0 shipped with built-in filters for Boolean columns and foreign-key columns.</span></span> <span data-ttu-id="f7641-929">フィルターでは、表示されているかどうか、またはどのような順序では表示の指定は許可しませんでした。</span><span class="sxs-lookup"><span data-stu-id="f7641-929">The filters did not allow you to specify whether they were displayed or in what order they were displayed.</span></span> <span data-ttu-id="f7641-930">新しい*DisplayAttribute*属性のアドレスを両方提供することでこれらの問題をフィルターとして列を表示するかどうかを制御し、どのような順序で表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7641-930">The new *DisplayAttribute* attribute addresses both of these issues by giving you control over whether a column is displayed as a filter and in what order it will be displayed.</span></span>

<span data-ttu-id="f7641-931">追加の機能強化がフィルター処理のサポートがされている[新しいを使用するように記述](#0.2__QueryExtender "_QueryExtender") Web フォームの機能です。</span><span class="sxs-lookup"><span data-stu-id="f7641-931">An additional enhancement is that filtering support has been re[written to use the new](#0.2__QueryExtender "_QueryExtender") feature of Web Forms.</span></span> <span data-ttu-id="f7641-932">これにより、フィルターで使用されるデータ ソース コントロールの知識がなくても、フィルターを作成できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-932">This lets you create filters without requiring knowledge of the data source control that the filters will be used with.</span></span> <span data-ttu-id="f7641-933">これらの拡張機能とフィルターもなっているコントロールのテンプレートに新しいものを追加できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-933">Along with these extensions, filters have also been turned into template controls, which allows you to add new ones.</span></span> <span data-ttu-id="f7641-934">最後に、 *DisplayAttribute*前に説明したクラスが同じで、オーバーライドできる既定のフィルターを使用する*UIHint*オーバーライドする列の既定のフィールド テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="f7641-934">Finally, the *DisplayAttribute* class mentioned earlier allows the default filter to be overridden, in the same way that *UIHint* allows the default field template for a column to be overridden.</span></span>

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a><span data-ttu-id="f7641-935">Visual Studio 2010 Web 開発機能の強化</span><span class="sxs-lookup"><span data-stu-id="f7641-935">Visual Studio 2010 Web Development Improvements</span></span>

<span data-ttu-id="f7641-936">大きい CSS 互換性、HTML および ASP.NET のマークアップ スニペットと新しい動的な IntelliSense の JavaScript による生産性の向上のため、Visual Studio 2010 での web 開発が強化されました。</span><span class="sxs-lookup"><span data-stu-id="f7641-936">Web development in Visual Studio 2010 has been enhanced for greater CSS compatibility, increased productivity through HTML and ASP.NET markup snippets and new dynamic IntelliSense JavaScript.</span></span>

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a><span data-ttu-id="f7641-937">強化された CSS 互換性</span><span class="sxs-lookup"><span data-stu-id="f7641-937">Improved CSS Compatibility</span></span>

<span data-ttu-id="f7641-938">Visual Studio 2010 で Visual Web Developer デザイナーは CSS 2.1 標準への準拠を向上させるために更新されました。</span><span class="sxs-lookup"><span data-stu-id="f7641-938">The Visual Web Developer designer in Visual Studio 2010 has been updated to improve CSS 2.1 standards compliance.</span></span> <span data-ttu-id="f7641-939">デザイナーは、HTML ソースの整合性を維持し、Visual Studio の以前のバージョンよりも堅牢です。</span><span class="sxs-lookup"><span data-stu-id="f7641-939">The designer better preserves integrity of the HTML source and is more robust than in previous versions of Visual Studio.</span></span> <span data-ttu-id="f7641-940">内部的には、アーキテクチャの機能強化も行われました将来的で、表示、レイアウト、および保守しやすくなります。</span><span class="sxs-lookup"><span data-stu-id="f7641-940">Under the hood, architectural improvements have also been made that will better enable future enhancements in rendering, layout, and serviceability.</span></span>

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a><span data-ttu-id="f7641-941">HTML および JavaScript のスニペット</span><span class="sxs-lookup"><span data-stu-id="f7641-941">HTML and JavaScript Snippets</span></span>

<span data-ttu-id="f7641-942">HTML エディターで IntelliSense オートコンプリート タグ名。</span><span class="sxs-lookup"><span data-stu-id="f7641-942">In the HTML editor, IntelliSense auto-completes tag names.</span></span> <span data-ttu-id="f7641-943">IntelliSense スニペット機能が自動補完し、タグ全体。</span><span class="sxs-lookup"><span data-stu-id="f7641-943">The IntelliSense Snippets feature auto-completes entire tags and more.</span></span> <span data-ttu-id="f7641-944">Visual Studio 2010 では、IntelliSense スニペットは、javascript、c# および Visual Basic、Visual Studio の以前のバージョンでサポートされていたと共にサポートされます。</span><span class="sxs-lookup"><span data-stu-id="f7641-944">In Visual Studio 2010, IntelliSense snippets are supported for JavaScript, alongside C# and Visual Basic, which were supported in earlier versions of Visual Studio.</span></span>

<span data-ttu-id="f7641-945">Visual Studio 2010 に役立つ、オート コンプリート一般的な ASP.NET タグと HTML タグ、必要な属性を含む 200 を超えるのスニペットが含まれています (など、runat ="server") と共通の属性をタグに特定 (など*ID*、 *DataSourceID*、 *ControlToValidate*、および*テキスト*)。</span><span class="sxs-lookup"><span data-stu-id="f7641-945">Visual Studio 2010 includes over 200 snippets that help you auto-complete common ASP.NET and HTML tags, including required attributes (such as runat="server") and common attributes specific to a tag (such as *ID*, *DataSourceID*, *ControlToValidate*, and *Text*).</span></span>

<span data-ttu-id="f7641-946">他のスニペットをダウンロードすることも一般的なタスクのユーザーまたはチームを使用するマークアップのブロックをカプセル化する、独自のスニペットを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-946">You can download additional snippets, or you can write your own snippets that encapsulate the blocks of markup that you or your team use for common tasks.</span></span>

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a><span data-ttu-id="f7641-947">JavaScript IntelliSense の機能強化</span><span class="sxs-lookup"><span data-stu-id="f7641-947">JavaScript IntelliSense Enhancements</span></span>

<span data-ttu-id="f7641-948">Visual 2010 では、JavaScript IntelliSense は再設計されましたより豊富な編集エクスペリエンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="f7641-948">In Visual 2010, JavaScript IntelliSense has been redesigned to provide an even richer editing experience.</span></span> <span data-ttu-id="f7641-949">IntelliSense は動的に生成されたメソッドによってなどのオブジェクト認識される*registerNamespace*およびその他の JavaScript フレームワークで使用される同様の手法です。</span><span class="sxs-lookup"><span data-stu-id="f7641-949">IntelliSense now recognizes objects that have been dynamically generated by methods such as *registerNamespace* and by similar techniques used by other JavaScript frameworks.</span></span> <span data-ttu-id="f7641-950">スクリプトの大規模なライブラリを分析して、ほとんどまたはまったく処理の遅延で IntelliSense を表示するパフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="f7641-950">Performance has been improved to analyze large libraries of script and to display IntelliSense with little or no processing delay.</span></span> <span data-ttu-id="f7641-951">ほぼすべてのサード パーティ製ライブラリをサポートし、多様なコーディングのスタイルをサポートするために、互換性を大幅に増加されています。</span><span class="sxs-lookup"><span data-stu-id="f7641-951">Compatibility has been dramatically increased to support nearly all third-party libraries and to support diverse coding styles.</span></span> <span data-ttu-id="f7641-952">入力して、IntelliSense によってすぐに活用できるように、ドキュメントのコメントは解析ようになりました。</span><span class="sxs-lookup"><span data-stu-id="f7641-952">Documentation comments are now parsed as you type and are immediately leveraged by IntelliSense.</span></span>

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a><span data-ttu-id="f7641-953">Visual Studio 2010 で web アプリケーションのデプロイ</span><span class="sxs-lookup"><span data-stu-id="f7641-953">Web Application Deployment with Visual Studio 2010</span></span>

<span data-ttu-id="f7641-954">ASP.NET 開発者は、Web アプリケーションをデプロイ、多くの場合、検索するなど、次の問題に遭遇します。</span><span class="sxs-lookup"><span data-stu-id="f7641-954">When ASP.NET developers deploy a Web application, they often find that they encounter issues such as the following:</span></span>

- <span data-ttu-id="f7641-955">共有ホスティング サイトにデプロイすると、時間がかかり、FTP などのテクノロジが必要です。</span><span class="sxs-lookup"><span data-stu-id="f7641-955">Deploying to a shared hosting site requires technologies such as FTP, which can be slow.</span></span> <span data-ttu-id="f7641-956">さらに、手動でデータベースを構成する SQL スクリプトの実行などのタスクを実行する必要があり、アプリケーションとして仮想ディレクトリのフォルダーの構成などの IIS 設定を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-956">In addition, you must manually perform tasks such as running SQL scripts to configure a database and you must change IIS settings, such as configuring a virtual directory folder as an application.</span></span>
- <span data-ttu-id="f7641-957">Web アプリケーションのファイルを展開するだけでなく、エンタープライズ環境で管理者頻繁にする必要があります変更 ASP.NET 構成ファイルと IIS の設定。</span><span class="sxs-lookup"><span data-stu-id="f7641-957">In an enterprise environment, in addition to deploying the Web application files, administrators frequently must modify ASP.NET configuration files and IIS settings.</span></span> <span data-ttu-id="f7641-958">データベース管理者は、一連の実行中のアプリケーション データベースを取得する SQL スクリプトを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-958">Database administrators must run a series of SQL scripts to get the application database running.</span></span> <span data-ttu-id="f7641-959">このようなインストールは多くの労力で、多くの場合、完了するには時間がかかると、慎重に文書化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-959">Such installations are labor intensive, often take hours to complete, and must be carefully documented.</span></span>

<span data-ttu-id="f7641-960">Visual Studio 2010 には、これらの問題に対処して、シームレスに Web アプリケーションをデプロイするためのテクノロジが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f7641-960">Visual Studio 2010 includes technologies that address these issues and that let you seamlessly deploy Web applications.</span></span> <span data-ttu-id="f7641-961">これらのテクノロジの 1 つは、IIS Web 配置ツール (MsDeploy.exe) です。</span><span class="sxs-lookup"><span data-stu-id="f7641-961">One of these technologies is the IIS Web Deployment Tool (MsDeploy.exe).</span></span>

<span data-ttu-id="f7641-962">Visual Studio 2010 で web デプロイ機能には、次の主要な領域があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-962">Web deployment features in Visual Studio 2010 include the following major areas:</span></span>

- <span data-ttu-id="f7641-963">Web のパッケージ化</span><span class="sxs-lookup"><span data-stu-id="f7641-963">Web packaging</span></span>
- <span data-ttu-id="f7641-964">Web.config 変換</span><span class="sxs-lookup"><span data-stu-id="f7641-964">Web.config transformation</span></span>
- <span data-ttu-id="f7641-965">データベースの配置</span><span class="sxs-lookup"><span data-stu-id="f7641-965">Database deployment</span></span>
- <span data-ttu-id="f7641-966">1 回のクリックは、Web アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="f7641-966">One-click publish for Web applications</span></span>

<span data-ttu-id="f7641-967">次のセクションでは、これらの機能について詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="f7641-967">The following sections provide details about these features.</span></span>

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a><span data-ttu-id="f7641-968">Web のパッケージ化</span><span class="sxs-lookup"><span data-stu-id="f7641-968">Web Packaging</span></span>

<span data-ttu-id="f7641-969">Visual Studio 2010 では、MSDeploy ツールを使用して、圧縮 (.zip) ファイルと呼ばれる、アプリケーションを作成、 *Web パッケージ*します。</span><span class="sxs-lookup"><span data-stu-id="f7641-969">Visual Studio 2010 uses the MSDeploy tool to create a compressed (.zip) file for your application, which is referred to as a *Web package*.</span></span> <span data-ttu-id="f7641-970">パッケージ ファイルには、アプリケーションと、次のコンテンツに関するメタデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f7641-970">The package file contains metadata about your application plus the following content:</span></span>

- <span data-ttu-id="f7641-971">IIS の設定、アプリケーション プールの設定や、エラー ページの設定が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f7641-971">IIS settings, which includes application pool settings, error page settings, and so on.</span></span>
- <span data-ttu-id="f7641-972">実際 Web コンテンツ、Web ページやユーザー コントロール、静的コンテンツ (イメージや HTML ファイル) などが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f7641-972">The actual Web content, which includes Web pages, user controls, static content (images and HTML files), and so on.</span></span>
- <span data-ttu-id="f7641-973">SQL Server データベース スキーマとデータ。</span><span class="sxs-lookup"><span data-stu-id="f7641-973">SQL Server database schemas and data.</span></span>
- <span data-ttu-id="f7641-974">セキュリティ証明書は、GAC、レジストリの設定でインストールするコンポーネント。</span><span class="sxs-lookup"><span data-stu-id="f7641-974">Security certificates, components to install in the GAC, registry settings, and so on.</span></span>

<span data-ttu-id="f7641-975">Web パッケージを任意のサーバーにコピーされ、IIS マネージャーを使用して手動でインストールされていることができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-975">A Web package can be copied to any server and then installed manually by using IIS Manager.</span></span> <span data-ttu-id="f7641-976">または、配置の自動化のコマンド ライン コマンドを使用して、または配置 Api を使用してなるパッケージをインストールすることができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-976">Alternatively, for automated deployment, the package can be installed by using command-line commands or by using deployment APIs.</span></span>

<span data-ttu-id="f7641-977">Visual Studio 2010 は、組み込みの MSBuild タスクと Web のパッケージを作成するターゲットを提供します。</span><span class="sxs-lookup"><span data-stu-id="f7641-977">Visual Studio 2010 provides built in MSBuild tasks and targets to create Web packages.</span></span> <span data-ttu-id="f7641-978">詳細については、次を参照してください。 [ASP.NET Web アプリケーション プロジェクトの配置の概要](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx)MSDN Web サイトと[10 + 20 の理由がなぜ Web パッケージを作成する必要があります](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html)Vishal Joshi's ブログ。</span><span class="sxs-lookup"><span data-stu-id="f7641-978">For more information, see [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) on the MSDN Web site and [10 + 20 reasons why you should create a Web Package](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) on Vishal Joshi's blog.</span></span>

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a><span data-ttu-id="f7641-979">Web.config 変換</span><span class="sxs-lookup"><span data-stu-id="f7641-979">Web.config Transformation</span></span>

<span data-ttu-id="f7641-980">Visual Studio 2010 を紹介する Web アプリケーションの配置の[XML Document Transform (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)、変換できる機能は、`Web.config`ファイルの開発設定の運用環境の設定をします。</span><span class="sxs-lookup"><span data-stu-id="f7641-980">For Web application deployment, Visual Studio 2010 introduces [XML Document Transform (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), which is a feature that lets you transform a `Web.config` file from development settings to production settings.</span></span> <span data-ttu-id="f7641-981">変換の設定がという名前の変換ファイルで指定された`web.debug.config`、`web.release.config`など。</span><span class="sxs-lookup"><span data-stu-id="f7641-981">Transformation settings are specified in transform files named `web.debug.config`, `web.release.config`, and so on.</span></span> <span data-ttu-id="f7641-982">(これらのファイルの名前は一致 MSBuild 構成です)。変換ファイルには、展開済みにする必要のある変更のみが含まれています。`Web.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7641-982">(The names of these files match MSBuild configurations.) A transform file includes just the changes that you need to make to a deployed `Web.config` file.</span></span> <span data-ttu-id="f7641-983">単純な構文を使用して変更を指定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-983">You specify the changes by using simple syntax.</span></span>

<span data-ttu-id="f7641-984">次の例の一部を示しています、`web.release.config`リリース構成のデプロイのファイルを生成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-984">The following example shows a portion of a `web.release.config` file that might be produced for deployment of your release configuration.</span></span> <span data-ttu-id="f7641-985">指定する展開時の例では置換キーワードを指定します、 *connectionString*内のノード、`Web.config`例では、記載されている値を持つファイルが置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="f7641-985">The Replace keyword in the example specifies that during deployment the *connectionString* node in the `Web.config` file will be replaced with the values that are listed in the example.</span></span>

[!code-xml[Main](overview/samples/sample102.xml)]

<span data-ttu-id="f7641-986">詳細については、次を参照してください[Web アプリケーション プロジェクトの配置の Web.config 変換構文](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx)msdn <a id="0.2_a"> </a> Web サイトと[Web 展開: Web.Config 変換](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)。Vishal Joshi's ブログ。</span><span class="sxs-lookup"><span data-stu-id="f7641-986">For more information, see [Web.config Transformation Syntax for Web Application Project Deployment](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) on the MSDN <a id="0.2_a"></a> Web site and[Web Deployment: Web.Config Transformation](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) on Vishal Joshi's blog.</span></span>

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a><span data-ttu-id="f7641-987">データベースの配置</span><span class="sxs-lookup"><span data-stu-id="f7641-987">Database Deployment</span></span>

<span data-ttu-id="f7641-988">Visual Studio 2010 の展開パッケージには、SQL Server データベースへの依存関係を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-988">A Visual Studio 2010 deployment package can include dependencies on SQL Server databases.</span></span> <span data-ttu-id="f7641-989">パッケージの定義の一部として、ソース データベースの接続文字列を指定します。</span><span class="sxs-lookup"><span data-stu-id="f7641-989">As part of the package definition, you provide the connection string for your source database.</span></span> <span data-ttu-id="f7641-990">Web パッケージを作成するときに、Visual Studio 2010 は、データベース スキーマとデータでは、必要に応じて、SQL スクリプトの作成し、パッケージに追加します。</span><span class="sxs-lookup"><span data-stu-id="f7641-990">When you create the Web package, Visual Studio 2010 creates SQL scripts for the database schema and optionally for the data, and then adds these to the package.</span></span> <span data-ttu-id="f7641-991">カスタムの SQL スクリプトを提供し、サーバーで実行する必要がありますシーケンスを指定できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-991">You can also provide custom SQL scripts and specify the sequence in which they should run on the server.</span></span> <span data-ttu-id="f7641-992">デプロイ時に、ターゲット サーバーの適切な接続文字列を指定します。展開プロセスは、この接続文字列を使用してデータベース スキーマを作成し、データを追加するスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="f7641-992">At deployment time, you provide a connection string that is appropriate for the target server; the deployment process then uses this connection string to run the scripts that create the database schema and add the data.</span></span>

<span data-ttu-id="f7641-993">さらに、1 回のクリックを使用して、発行、アプリケーションがリモート共有ホスティング サイトに発行されると、データベースを直接発行する配置を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="f7641-993">In addition, by using one-click publish, you can configure deployment to publish your database directly when the application is published to a remote shared hosting site.</span></span> <span data-ttu-id="f7641-994">詳細については、次を参照してください。[方法: Web アプリケーション プロジェクトでのデータベース配置](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx)MSDN Web サイトと[VS 2010 を使用したデータベースの配置](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi's ブログ。</span><span class="sxs-lookup"><span data-stu-id="f7641-994">For more information, see [How to: Deploy a Database With a Web Application Project](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) on the MSDN Web site and [Database Deployment with VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) on Vishal Joshi's blog.</span></span>

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a><span data-ttu-id="f7641-995">1 回のクリックは、Web アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="f7641-995">One-Click Publish for Web Applications</span></span>

<span data-ttu-id="f7641-996">Visual Studio 2010 では、リモート サーバーに Web アプリケーションを発行する IIS のリモート管理サービスを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="f7641-996">Visual Studio 2010 also lets you use the IIS remote management service to publish a Web application to a remote server.</span></span> <span data-ttu-id="f7641-997">ホスティング アカウントまたはサーバーをテストまたはステージング サーバーには、発行プロファイルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-997">You can create a publish profile for your hosting account or for testing servers or staging servers.</span></span> <span data-ttu-id="f7641-998">各プロファイルは、適切な資格情報を安全に保存できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-998">Each profile can save appropriate credentials securely.</span></span> <span data-ttu-id="f7641-999">ターゲットのいずれかに展開することができますし、Web の 1 回のクリックを使用して 1 回のクリックでのサーバーは、ツールバーを公開します。</span><span class="sxs-lookup"><span data-stu-id="f7641-999">You can then deploy to any of the target servers with one click by using the Web one-click publish toolbar.</span></span> <span data-ttu-id="f7641-1000">Visual Studio 2010 で MSBuild のコマンドラインを使用して公開することもできます。</span><span class="sxs-lookup"><span data-stu-id="f7641-1000">With Visual Studio 2010, you can also publish by using the MSBuild command line.</span></span> <span data-ttu-id="f7641-1001">これにより、発行を継続的インテグレーション モデルに含めるチーム ビルド環境を構成できます。</span><span class="sxs-lookup"><span data-stu-id="f7641-1001">This lets you configure your team build environment to include publishing in a continuous-integration model.</span></span>

<span data-ttu-id="f7641-1002">詳細については、次を参照してください。[方法: Web アプリケーション プロジェクトを使用して 1 回のクリックの 発行および Web デプロイのデプロイ](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx)MSDN Web サイトと[1 クリックで VS 2010 での発行を Web](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) Vishal Joshi's ブログ。</span><span class="sxs-lookup"><span data-stu-id="f7641-1002">For more information, see [How to: Deploy a Web Application Project Using One-Click Publish and Web Deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) on the MSDN Web site and [Web 1-Click Publish with VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) on Vishal Joshi's blog.</span></span> <span data-ttu-id="f7641-1003">Visual Studio 2010 で Web アプリケーションのデプロイに関するビデオ プレゼンテーションを表示するを参照してください。 [Web 開発者プレビューには、VS 2010](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) Vishal Joshi's ブログ。</span><span class="sxs-lookup"><span data-stu-id="f7641-1003">To view video presentations about Web application deployment in Visual Studio 2010, see [VS 2010 for Web Developer Previews](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) on Vishal Joshi's blog.</span></span>

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a><span data-ttu-id="f7641-1004">リソース</span><span class="sxs-lookup"><span data-stu-id="f7641-1004">Resources</span></span>

<span data-ttu-id="f7641-1005">次の Web サイトでは、ASP.NET 4 および Visual Studio 2010 に関する追加情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="f7641-1005">The following Web sites provide additional information about ASP.NET 4 and Visual Studio 2010.</span></span>

- <span data-ttu-id="f7641-1006">[ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) -MSDN Web サイト上の ASP.NET 4 用の公式ドキュメント。</span><span class="sxs-lookup"><span data-stu-id="f7641-1006">[ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) — The official documentation for ASP.NET 4 on the MSDN Web site.</span></span>
- <span data-ttu-id="f7641-1007">[https://www.asp.net/](https://www.asp.net/) -ASP.NET チームの Web サイト。</span><span class="sxs-lookup"><span data-stu-id="f7641-1007">[https://www.asp.net/](https://www.asp.net/) — The ASP.NET team's own Web site.</span></span>
- <span data-ttu-id="f7641-1008">[https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) [ASP.NET 動的データのコンテンツ マップ](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx)-ASP.NET チーム サイトと ASP.NET Dynamic Data の公式ドキュメントでのオンライン リソースです。</span><span class="sxs-lookup"><span data-stu-id="f7641-1008">[https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) and [ASP.NET Dynamic Data Content Map](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) — Online resources on the ASP.NET team site and in the official documentation for ASP.NET Dynamic Data.</span></span>
- <span data-ttu-id="f7641-1009">[https://www.asp.net/ajax/](../../ajax/index.md) -メインの Web リソースを ASP.NET Ajax 開発します。</span><span class="sxs-lookup"><span data-stu-id="f7641-1009">[https://www.asp.net/ajax/](../../ajax/index.md) — The main Web resource for ASP.NET Ajax development.</span></span>
- <span data-ttu-id="f7641-1010">[https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — Visual Web Developer チーム ブログ Visual Studio 2010 の機能に関する情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f7641-1010">[https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — The Visual Web Developer Team blog, which includes information about features in Visual Studio 2010.</span></span>
- <span data-ttu-id="f7641-1011">[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) -ASP.NET のプレビュー リリースのメインの Web リソース。</span><span class="sxs-lookup"><span data-stu-id="f7641-1011">[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) — The main Web resource for preview releases of ASP.NET.</span></span>

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a><span data-ttu-id="f7641-1012">免責情報</span><span class="sxs-lookup"><span data-stu-id="f7641-1012">Disclaimer</span></span>

<span data-ttu-id="f7641-1013">このドキュメントは暫定版であり、ここで説明するソフトウェアの最終的な商業リリースより前に大幅に変更される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-1013">This is a preliminary document and may be changed substantially prior to final commercial release of the software described herein.</span></span>

<span data-ttu-id="f7641-1014">このドキュメントに記載されている情報は、説明されている問題に関する Microsoft Corporation の発行日時点における最新の見解を表します。</span><span class="sxs-lookup"><span data-stu-id="f7641-1014">The information contained in this document represents the current view of Microsoft Corporation on the issues discussed as of the date of publication.</span></span> <span data-ttu-id="f7641-1015">マイクロソフトは変化する市場の状況に対応する必要があるため、本ドキュメントをマイクロソフトの確約と解釈することはできず、発行日以降は、提示されている情報の正確性を保証できません。</span><span class="sxs-lookup"><span data-stu-id="f7641-1015">Because Microsoft must respond to changing market conditions, it should not be interpreted to be a commitment on the part of Microsoft, and Microsoft cannot guarantee the accuracy of any information presented after the date of publication.</span></span>

<span data-ttu-id="f7641-1016">このホワイト ペーパーは、情報提供のみを目的としています。</span><span class="sxs-lookup"><span data-stu-id="f7641-1016">This White Paper is for informational purposes only.</span></span> <span data-ttu-id="f7641-1017">マイクロソフトは、このドキュメント内の情報に関して、明示的、暗黙的、または法的ないかなる保証も行いません。</span><span class="sxs-lookup"><span data-stu-id="f7641-1017">MICROSOFT MAKES NO WARRANTIES, EXPRESS, IMPLIED OR STATUTORY, AS TO THE INFORMATION IN THIS DOCUMENT.</span></span>

<span data-ttu-id="f7641-1018">お客様ご自身の責任において、適用されるすべての著作権関連法規に従ったご使用を願います。</span><span class="sxs-lookup"><span data-stu-id="f7641-1018">Complying with all applicable copyright laws is the responsibility of the user.</span></span> <span data-ttu-id="f7641-1019">このドキュメントのいかなる部分も、米国 Microsoft Corporation の書面による許諾を受けることなく、その目的を問わず、どのような形態であっても、複製または譲渡することは禁じられています。ここでいう形態とは、複写や記録など、電子的な、または物理的なすべての手段を含みます。ただしこれは、著作権法上のお客様の権利を制限するものではありません。</span><span class="sxs-lookup"><span data-stu-id="f7641-1019">Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.</span></span>

<span data-ttu-id="f7641-1020">マイクロソフトは、このドキュメントに記載されている内容に関し、特許、特許申請、商標、著作権、またはその他の無体財産権を有する場合があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-1020">Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document.</span></span> <span data-ttu-id="f7641-1021">別途マイクロソフトのライセンス契約上に明示の規定のない限り、このドキュメントはこれらの特許、商標、著作権、またはその他の無体財産権に関する権利をお客様に許諾するものではありません。</span><span class="sxs-lookup"><span data-stu-id="f7641-1021">Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.</span></span>

<span data-ttu-id="f7641-1022">明記されない限り、会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、イベント実在架空のものと関連会社、組織、製品、ドメイン名、電子メールをアドレス、ロゴ、人物、場所、またはイベントは、または推論する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-1022">Unless otherwise noted, the example companies, organizations, products, domain names, email addresses, logos, people, places and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, email address, logo, person, place or event is intended or should be inferred.</span></span>

<span data-ttu-id="f7641-1023">© 2009 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="f7641-1023">© 2009 Microsoft Corporation.</span></span> <span data-ttu-id="f7641-1024">All rights reserved.</span><span class="sxs-lookup"><span data-stu-id="f7641-1024">All rights reserved.</span></span>

<span data-ttu-id="f7641-1025">Microsoft および Windows は、米国および他の国における Microsoft Corporation の登録商標または商標です。</span><span class="sxs-lookup"><span data-stu-id="f7641-1025">Microsoft and Windows are either registered trademarks or trademarks of Microsoft Corporation in the United States and/or other countries.</span></span>

<span data-ttu-id="f7641-1026">本ドキュメントで説明する実際の製造元および製品名は、それぞれの所有者の商標である場合があります。</span><span class="sxs-lookup"><span data-stu-id="f7641-1026">The names of actual companies and products mentioned herein may be the trademarks of their respective owners.</span></span>
