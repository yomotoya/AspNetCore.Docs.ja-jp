---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 および Visual Studio 2010 の Web 開発の概要 |Microsoft ドキュメント
author: rick-anderson
description: このドキュメントでは、.net Framework 4 におけると Visual Studio 2010 に含まれている ASP.NET の新機能の多くの概要を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 6ce52c387ff835eda46bc1882b8b974889e2d4af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
ms.locfileid: "30899044"
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a><span data-ttu-id="c6c2f-103">ASP.NET 4 および Visual Studio 2010 の Web 開発の概要</span><span class="sxs-lookup"><span data-stu-id="c6c2f-103">ASP.NET 4 and Visual Studio 2010 Web Development Overview</span></span>
====================
> <span data-ttu-id="c6c2f-104">このドキュメントでは、.net Framework 4 におけると Visual Studio 2010 に含まれている ASP.NET の新機能の多くの概要を示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-104">This document provides an overview of many of the new features for ASP.NET that are included in the.NET Framework 4 and in Visual Studio 2010.</span></span>
> 
> [<span data-ttu-id="c6c2f-105">このホワイト ペーパーをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-105">Download This Whitepaper</span></span>](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


<span data-ttu-id="c6c2f-106">**目次**</span><span class="sxs-lookup"><span data-stu-id="c6c2f-106">**Contents**</span></span>

<span data-ttu-id="c6c2f-107">**[Core Services](#0.2__Toc253429238 "_Toc253429238")**</span><span class="sxs-lookup"><span data-stu-id="c6c2f-107">**[Core Services](#0.2__Toc253429238 "_Toc253429238")**</span></span>  
[<span data-ttu-id="c6c2f-108">Web.config ファイルがリファクタリング</span><span class="sxs-lookup"><span data-stu-id="c6c2f-108">Web.config File Refactoring</span></span>](#0.2__Toc253429239 "_Toc253429239")  
[<span data-ttu-id="c6c2f-109">拡張可能な出力キャッシュ</span><span class="sxs-lookup"><span data-stu-id="c6c2f-109">Extensible Output Caching</span></span>](#0.2__Toc253429240 "_Toc253429240")  
[<span data-ttu-id="c6c2f-110">Web アプリケーションの自動開始</span><span class="sxs-lookup"><span data-stu-id="c6c2f-110">Auto-Start Web Applications</span></span>](#0.2__Toc253429241 "_Toc253429241")  
[<span data-ttu-id="c6c2f-111">永続的なリダイレクト ページ</span><span class="sxs-lookup"><span data-stu-id="c6c2f-111">Permanently Redirecting a Page</span></span>](#0.2__Toc253429242 "_Toc253429242")  
[<span data-ttu-id="c6c2f-112">セッション状態を圧縮</span><span class="sxs-lookup"><span data-stu-id="c6c2f-112">Shrinking Session State</span></span>](#0.2__Toc253429243 "_Toc253429243")  
[<span data-ttu-id="c6c2f-113">許容される Url の範囲を拡大</span><span class="sxs-lookup"><span data-stu-id="c6c2f-113">Expanding the Range of Allowable URLs</span></span>](#0.2__Toc253429244 "_Toc253429244")  
[<span data-ttu-id="c6c2f-114">拡張可能な要求の検証</span><span class="sxs-lookup"><span data-stu-id="c6c2f-114">Extensible Request Validation</span></span>](#0.2__Toc253429245 "_Toc253429245")  
[<span data-ttu-id="c6c2f-115">オブジェクト キャッシュとキャッシュ機能拡張、オブジェクトを</span><span class="sxs-lookup"><span data-stu-id="c6c2f-115">Object Caching and Object Caching Extensibility</span></span>](#0.2__Toc253429246 "_Toc253429246")  
[<span data-ttu-id="c6c2f-116">拡張可能な HTML、URL、および HTTP ヘッダーのエンコーディング</span><span class="sxs-lookup"><span data-stu-id="c6c2f-116">Extensible HTML, URL, and HTTP Header Encoding</span></span>](#0.2__Toc253429247 "_Toc253429247")  
[<span data-ttu-id="c6c2f-117">1 つのワーカー プロセス内の個々 のアプリケーションのパフォーマンスを監視</span><span class="sxs-lookup"><span data-stu-id="c6c2f-117">Performance Monitoring for Individual Applications in a Single Worker Process</span></span>](#0.2__Toc253429248 "_Toc253429248")  
[<span data-ttu-id="c6c2f-118">Multi-Targeting</span><span class="sxs-lookup"><span data-stu-id="c6c2f-118">Multi-Targeting</span></span>](#0.2__Toc253429249 "_Toc253429249")

<span data-ttu-id="c6c2f-119">**[Ajax](#0.2__Toc253429250 "_Toc253429250")**</span><span class="sxs-lookup"><span data-stu-id="c6c2f-119">**[Ajax](#0.2__Toc253429250 "_Toc253429250")**</span></span>  
[<span data-ttu-id="c6c2f-120">含まれる Web フォームと MVC の jQuery</span><span class="sxs-lookup"><span data-stu-id="c6c2f-120">jQuery Included with Web Forms and MVC</span></span>](#0.2__Toc253429251 "_Toc253429251")  
[<span data-ttu-id="c6c2f-121">コンテンツ配信ネットワーク サポート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-121">Content Delivery Network Support</span></span>](#0.2__Toc253429252 "_Toc253429252")  
[<span data-ttu-id="c6c2f-122">ScriptManager の明示的なスクリプト</span><span class="sxs-lookup"><span data-stu-id="c6c2f-122">ScriptManager Explicit Scripts</span></span>](#0.2__Toc253429253 "_Toc253429253")

<span data-ttu-id="c6c2f-123">**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**</span><span class="sxs-lookup"><span data-stu-id="c6c2f-123">**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**</span></span>  
[<span data-ttu-id="c6c2f-124">Page.MetaKeywords と Page.MetaDescription プロパティをメタ タグを設定</span><span class="sxs-lookup"><span data-stu-id="c6c2f-124">Setting Meta Tags with the Page.MetaKeywords and Page.MetaDescription Properties</span></span>](#0.2__Toc253429257 "_Toc253429257")  
[<span data-ttu-id="c6c2f-125">個々 のコントロール ビュー ステートを有効にする</span><span class="sxs-lookup"><span data-stu-id="c6c2f-125">Enabling View State for Individual Controls</span></span>](#0.2__Toc253429258 "_Toc253429258")  
[<span data-ttu-id="c6c2f-126">ブラウザーの機能に変更</span><span class="sxs-lookup"><span data-stu-id="c6c2f-126">Changes to Browser Capabilities</span></span>](#0.2__Toc253429259 "_Toc253429259")  
[<span data-ttu-id="c6c2f-127">ASP.NET 4 でルーティング</span><span class="sxs-lookup"><span data-stu-id="c6c2f-127">Routing in ASP.NET 4</span></span>](#0.2__Toc253429260 "_Toc253429260")  
[<span data-ttu-id="c6c2f-128">クライアント Id を設定</span><span class="sxs-lookup"><span data-stu-id="c6c2f-128">Setting Client IDs</span></span>](#0.2__Toc253429261 "_Toc253429261")  
[<span data-ttu-id="c6c2f-129">データ コントロールで行の選択を保持する</span><span class="sxs-lookup"><span data-stu-id="c6c2f-129">Persisting Row Selection in Data Controls</span></span>](#0.2__Toc253429262 "_Toc253429262")  
[<span data-ttu-id="c6c2f-130">ASP.NET Chart Control</span><span class="sxs-lookup"><span data-stu-id="c6c2f-130">ASP.NET Chart Control</span></span>](#0.2__Toc253429263 "_Toc253429263")  
[<span data-ttu-id="c6c2f-131">QueryExtender コントロールにデータをフィルタ リング</span><span class="sxs-lookup"><span data-stu-id="c6c2f-131">Filtering Data with the QueryExtender Control</span></span>](#0.2__Toc253429264 "_Toc253429264")  
[<span data-ttu-id="c6c2f-132">コード式に html エンコード</span><span class="sxs-lookup"><span data-stu-id="c6c2f-132">Html Encoded Code Expressions</span></span>](#0.2__Toc253429265 "_Toc253429265")  
[<span data-ttu-id="c6c2f-133">テンプレートの変更をプロジェクト</span><span class="sxs-lookup"><span data-stu-id="c6c2f-133">Project Template Changes</span></span>](#0.2__Toc253429266 "_Toc253429266")  
[<span data-ttu-id="c6c2f-134">CSS の機能強化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-134">CSS Improvements</span></span>](#0.2__Toc253429267 "_Toc253429267")  
[<span data-ttu-id="c6c2f-135">Div 要素を非表示のフィールドを非表示にする</span><span class="sxs-lookup"><span data-stu-id="c6c2f-135">Hiding div Elements Around Hidden Fields</span></span>](#0.2__Toc253429268 "_Toc253429268")  
[<span data-ttu-id="c6c2f-136">テンプレート コントロールの外部テーブルをレンダリング</span><span class="sxs-lookup"><span data-stu-id="c6c2f-136">Rendering an Outer Table for Templated Controls</span></span>](#0.2__Toc253429269 "_Toc253429269")  
[<span data-ttu-id="c6c2f-137">ListView コントロールの機能強化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-137">ListView Control Enhancements</span></span>](#0.2__Toc253429270 "_Toc253429270")  
[<span data-ttu-id="c6c2f-138">CheckBoxList と RadioButtonList コントロールの機能強化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-138">CheckBoxList and RadioButtonList Control Enhancements</span></span>](#0.2__Toc253429271 "_Toc253429271")  
[<span data-ttu-id="c6c2f-139">メニュー コントロールの機能強化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-139">Menu Control Improvements</span></span>](#0.2__Toc253429272 "_Toc253429272")  
[<span data-ttu-id="c6c2f-140">ウィザードおよび CreateUserWizard コントロール 56</span><span class="sxs-lookup"><span data-stu-id="c6c2f-140">Wizard and CreateUserWizard Controls 56</span></span>](#0.2__Toc253429273 "_Toc253429273")

<span data-ttu-id="c6c2f-141">**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**</span><span class="sxs-lookup"><span data-stu-id="c6c2f-141">**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**</span></span>  
[<span data-ttu-id="c6c2f-142">領域サポート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-142">Areas Support</span></span>](#0.2__Toc253429275 "_Toc253429275")  
[<span data-ttu-id="c6c2f-143">データ注釈属性の検証のサポート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-143">Data-Annotation Attribute Validation Support</span></span>](#0.2__Toc253429276 "_Toc253429276")  
[<span data-ttu-id="c6c2f-144">テンプレート化されたヘルパー</span><span class="sxs-lookup"><span data-stu-id="c6c2f-144">Templated Helpers</span></span>](#0.2__Toc253429277 "_Toc253429277")

<span data-ttu-id="c6c2f-145">**[動的データ](#0.2__Toc253429278 "_Toc253429278")**</span><span class="sxs-lookup"><span data-stu-id="c6c2f-145">**[Dynamic Data](#0.2__Toc253429278 "_Toc253429278")**</span></span>  
[<span data-ttu-id="c6c2f-146">既存のプロジェクトの動的なデータの有効化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-146">Enabling Dynamic Data for Existing Projects</span></span>](#0.2__Toc253429279 "_Toc253429279")  
[<span data-ttu-id="c6c2f-147">宣言型 DynamicDataManager コントロール構文</span><span class="sxs-lookup"><span data-stu-id="c6c2f-147">Declarative DynamicDataManager Control Syntax</span></span>](#0.2__Toc253429280 "_Toc253429280")  
[<span data-ttu-id="c6c2f-148">エンティティ テンプレート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-148">Entity Templates</span></span>](#0.2__Toc253429281 "_Toc253429281")  
[<span data-ttu-id="c6c2f-149">Url と電子メール アドレスの新しいフィールド テンプレート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-149">New Field Templates for URLs and E-mail Addresses</span></span>](#0.2__Toc253429282 "_Toc253429282")  
[<span data-ttu-id="c6c2f-150">DynamicHyperLink コントロールとのリンクを作成する</span><span class="sxs-lookup"><span data-stu-id="c6c2f-150">Creating Links with the DynamicHyperLink Control</span></span>](#0.2__Toc253429283 "_Toc253429283")  
[<span data-ttu-id="c6c2f-151">データ モデルにおける継承のサポート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-151">Support for Inheritance in the Data Model</span></span>](#0.2__Toc253429284 "_Toc253429284")  
[<span data-ttu-id="c6c2f-152">多対多のリレーションシップ (Entity Framework) のサポート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-152">Support for Many-to-Many Relationships (Entity Framework Only)</span></span>](#0.2__Toc253429285 "_Toc253429285")  
[<span data-ttu-id="c6c2f-153">コントロールの表示と列挙のサポートを新しい属性</span><span class="sxs-lookup"><span data-stu-id="c6c2f-153">New Attributes to Control Display and Support Enumerations</span></span>](#0.2__Toc253429286 "_Toc253429286")  
[<span data-ttu-id="c6c2f-154">フィルターのサポート強化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-154">Enhanced Support for Filters</span></span>](#0.2__Toc253429287 "_Toc253429287")

<span data-ttu-id="c6c2f-155">**[Visual Studio 2010 Web 開発の改良](#0.2__Toc253429288 "_Toc253429288")**</span><span class="sxs-lookup"><span data-stu-id="c6c2f-155">**[Visual Studio 2010 Web Development Improvements](#0.2__Toc253429288 "_Toc253429288")**</span></span>  
[<span data-ttu-id="c6c2f-156">CSS の互換性を改善</span><span class="sxs-lookup"><span data-stu-id="c6c2f-156">Improved CSS Compatibility</span></span>](#0.2__Toc253429289 "_Toc253429289")  
[<span data-ttu-id="c6c2f-157">HTML および JavaScript スニペット</span><span class="sxs-lookup"><span data-stu-id="c6c2f-157">HTML and JavaScript Snippets</span></span>](#0.2__Toc253429290 "_Toc253429290")  
[<span data-ttu-id="c6c2f-158">JavaScript IntelliSense の機能強化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-158">JavaScript IntelliSense Enhancements</span></span>](#0.2__Toc253429291 "_Toc253429291")

<span data-ttu-id="c6c2f-159">**[Web アプリケーションの配置を Visual Studio 2010](#0.2__Toc253429292 "_Toc253429292")**</span><span class="sxs-lookup"><span data-stu-id="c6c2f-159">**[Web Application Deployment with Visual Studio 2010](#0.2__Toc253429292 "_Toc253429292")**</span></span>  
[<span data-ttu-id="c6c2f-160">パッケージを web</span><span class="sxs-lookup"><span data-stu-id="c6c2f-160">Web Packaging</span></span>](#0.2__Toc253429293 "_Toc253429293")  
[<span data-ttu-id="c6c2f-161">Web.config 変換</span><span class="sxs-lookup"><span data-stu-id="c6c2f-161">Web.config Transformation</span></span>](#0.2__Toc253429294 "_Toc253429294")  
[<span data-ttu-id="c6c2f-162">データベース配置</span><span class="sxs-lookup"><span data-stu-id="c6c2f-162">Database Deployment</span></span>](#0.2__Toc253429295 "_Toc253429295")  
[<span data-ttu-id="c6c2f-163">Web アプリケーション用の 1 回のクリックを公開</span><span class="sxs-lookup"><span data-stu-id="c6c2f-163">One-Click Publish for Web Applications</span></span>](#0.2__Toc253429296 "_Toc253429296")  
[<span data-ttu-id="c6c2f-164">Resources</span><span class="sxs-lookup"><span data-stu-id="c6c2f-164">Resources</span></span>](#0.2__Toc253429297 "_Toc253429297")

<span data-ttu-id="c6c2f-165">**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**</span><span class="sxs-lookup"><span data-stu-id="c6c2f-165">**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**</span></span>

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a><span data-ttu-id="c6c2f-166">コア サービス</span><span class="sxs-lookup"><span data-stu-id="c6c2f-166">Core Services</span></span>

<span data-ttu-id="c6c2f-167">ASP.NET 4 には、いくつかの出力キャッシュおよびセッション状態の記憶域などの ASP.NET コア サービスを向上させる機能が導入されています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-167">ASP.NET 4 introduces a number of features that improve core ASP.NET services such as output caching and session-state storage.</span></span>

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a><span data-ttu-id="c6c2f-168">Web.config ファイルのリファクタリング</span><span class="sxs-lookup"><span data-stu-id="c6c2f-168">Web.config File Refactoring</span></span>

<span data-ttu-id="c6c2f-169">`Web.config`構成を含むファイル、Web アプリケーションが大きく成長過去のいくつかリリースの .NET Framework 経由でように新しい機能が追加されました、Ajax などのルーティング、および IIS 7 との統合。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-169">The `Web.config` file that contains the configuration for a Web application has grown considerably over the past few releases of the .NET Framework as new features have been added, such as Ajax, routing, and integration with IIS 7.</span></span> <span data-ttu-id="c6c2f-170">これはで、構成するか、Visual Studio のようなツールを使用せずに新しい Web アプリケーションを起動するは困難です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-170">This has made it harder to configure or start new Web applications without a tool like Visual Studio.</span></span> <span data-ttu-id="c6c2f-171">.NET Framework 4、主要な構成要素を移動されています、`machine.config`ファイル、およびアプリケーションを今すぐこれらの設定を継承します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-171">In .the NET Framework 4, the major configuration elements have been moved to the `machine.config` file, and applications now inherit these settings.</span></span> <span data-ttu-id="c6c2f-172">これにより、`Web.config`を空にするのにまたは線は次だけ、Visual Studio のアプリケーションが対象とする framework のバージョンを指定します。 これを含む ASP.NET 4 アプリケーション内のファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-172">This allows the `Web.config` file in ASP.NET 4 applications either to be empty or to contain just the following lines, which specify for Visual Studio what version of the framework the application is targeting:</span></span>

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a><span data-ttu-id="c6c2f-173">拡張可能な出力キャッシュ</span><span class="sxs-lookup"><span data-stu-id="c6c2f-173">Extensible Output Caching</span></span>

<span data-ttu-id="c6c2f-174">以来、ASP.NET 1.0 がリリースされた、出力キャッシュが有効になっているページ、コントロール、および HTTP 応答の生成された出力をメモリに格納する開発者です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-174">Since the time that ASP.NET 1.0 was released, output caching has enabled developers to store the generated output of pages, controls, and HTTP responses in memory.</span></span> <span data-ttu-id="c6c2f-175">以降の Web 要求で ASP.NET 提供できますコンテンツより迅速に最初から出力を再生成する代わりに、メモリから生成された出力を取得することで。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-175">On subsequent Web requests, ASP.NET can serve content more quickly by retrieving the generated output from memory instead of regenerating the output from scratch.</span></span> <span data-ttu-id="c6c2f-176">ただし、このアプローチには、制限、生成されるコンテンツは常にメモリに格納されますが、大量のトラフィックが発生しているサーバーに出力キャッシュによって消費されるメモリは Web アプリケーションの他の部分からのメモリの要求に競合します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-176">However, this approach has a limitation — generated content always has to be stored in memory, and on servers that are experiencing heavy traffic, the memory consumed by output caching can compete with memory demands from other portions of a Web application.</span></span>

<span data-ttu-id="c6c2f-177">ASP.NET 4 では、1 つまたは複数のカスタム出力キャッシュ プロバイダーを構成することができますを出力キャッシュの機能拡張ポイントを追加します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-177">ASP.NET 4 adds an extensibility point to output caching that enables you to configure one or more custom output-cache providers.</span></span> <span data-ttu-id="c6c2f-178">出力キャッシュ プロバイダーは、任意のストレージ機構を使用して、HTML コンテンツを保持します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-178">Output-cache providers can use any storage mechanism to persist HTML content.</span></span> <span data-ttu-id="c6c2f-179">これを多様な永続化のメカニズムは、ローカルまたはリモート ディスクを含めることができます、クラウドの記憶域のカスタム出力キャッシュ プロバイダーを作成できるようになり、分散キャッシュ エンジン。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-179">This makes it possible to create custom output-cache providers for diverse persistence mechanisms, which can include local or remote disks, cloud storage, and distributed cache engines.</span></span>

<span data-ttu-id="c6c2f-180">新しいから派生するクラスとしてカスタム出力キャッシュ プロバイダーを作成する*System.Web.Caching.OutputCacheProvider*型です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-180">You create a custom output-cache provider as a class that derives from the new *System.Web.Caching.OutputCacheProvider* type.</span></span> <span data-ttu-id="c6c2f-181">プロバイダーを構成することができますし、`Web.config`新しいを使用してファイル*プロバイダー*のサブセクション、 *outputCache*要素は、次の例で示すように。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-181">You can then configure the provider in the `Web.config` file by using the new *providers* subsection of the *outputCache* element, as shown in the following example:</span></span>

[!code-xml[Main](overview/samples/sample2.xml)]

<span data-ttu-id="c6c2f-182">ASP.NET 4 ですべての HTTP 応答では、既定では、ページを表示し、前の例で示すように、コントロールが、メモリ内の出力キャッシュを使用している、 *defaultProvider* AspNetInternalProvider に属性が設定されています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-182">By default in ASP.NET 4, all HTTP responses, rendered pages, and controls use the in-memory output cache, as shown in the previous example, where the *defaultProvider* attribute is set to AspNetInternalProvider.</span></span> <span data-ttu-id="c6c2f-183">別のプロバイダー名を指定して Web アプリケーションの使用既定の出力キャッシュ プロバイダーを変更することができます*defaultProvider*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-183">You can change the default output-cache provider used for a Web application by specifying a different provider name for *defaultProvider*.</span></span>

<span data-ttu-id="c6c2f-184">さらに、要求ごと、およびコントロールごとに別の出力キャッシュ プロバイダーを選択することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-184">In addition, you can select different output-cache providers per control and per request.</span></span> <span data-ttu-id="c6c2f-185">別の Web ユーザー コントロールの別の出力キャッシュ プロバイダーを選択する最も簡単な方法は、new を使用して、宣言を行う、 *providerName*制御のディレクティブでは、次の例で示すように属性します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-185">The easiest way to choose a different output-cache provider for different Web user controls is to do so declaratively by using the new *providerName* attribute in a control directive, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample3.aspx)]

<span data-ttu-id="c6c2f-186">HTTP 要求に対する別の出力キャッシュ プロバイダーを指定するには、少しの作業が必要です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-186">Specifying a different output cache provider for an HTTP request requires a little more work.</span></span> <span data-ttu-id="c6c2f-187">プロバイダーを宣言によって指定するには、代わりに上書きする新しい*GetOuputCacheProviderName*メソッドで、`Global.asax`ファイルをプログラムで、特定の要求を使用するプロバイダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-187">Instead of declaratively specifying the provider, you override the new *GetOuputCacheProviderName* method in the `Global.asax` file to programmatically specify which provider to use for a specific request.</span></span> <span data-ttu-id="c6c2f-188">その方法を次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-188">The following example shows how to do this.</span></span>

[!code-csharp[Main](overview/samples/sample4.cs)]

<span data-ttu-id="c6c2f-189">ASP.NET 4 に出力キャッシュ プロバイダー拡張機能の追加により、Web サイトのようになりましたより積極的なとより高度な出力キャッシュの戦略を試みることができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-189">With the addition of output-cache provider extensibility to ASP.NET 4, you can now pursue more aggressive and more intelligent output-caching strategies for your Web sites.</span></span> <span data-ttu-id="c6c2f-190">たとえば、ディスク上のトラフィックが低いを取得するページをキャッシュ中に、メモリ内のサイトの「トップ 10」ページをキャッシュすることはようになりました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-190">For example, it is now possible to cache the "Top 10" pages of a site in memory, while caching pages that get lower traffic on disk.</span></span> <span data-ttu-id="c6c2f-191">代わりに、キャッシュ、レンダリングされるページのすべての変更によって組み合わせが、分散キャッシュを使用して、メモリ使用量は、フロント エンド Web サーバーからオフロードされるようにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-191">Alternatively, you can cache every vary-by combination for a rendered page, but use a distributed cache so that the memory consumption is offloaded from front-end Web servers.</span></span>

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a><span data-ttu-id="c6c2f-192">Web アプリケーションの自動開始</span><span class="sxs-lookup"><span data-stu-id="c6c2f-192">Auto-Start Web Applications</span></span>

<span data-ttu-id="c6c2f-193">一部の Web アプリケーションは、大量のデータを読み込んだり、最初の要求を処理する前に処理負荷のかかる初期化を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-193">Some Web applications need to load large amounts of data or perform expensive initialization processing before serving the first request.</span></span> <span data-ttu-id="c6c2f-194">以前のバージョンの ASP.NET では、このような状況にする必要がありました脱するよう、ASP.NET アプリケーションおよび時に初期化コードを実行するカスタムの認証方法を検討、*アプリケーション\_ロード*メソッドで、 `Global.asax`ファイルです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-194">In earlier versions of ASP.NET, for these situations you had to devise custom approaches to "wake up" an ASP.NET application and then run initialization code during the *Application\_Load* method in the `Global.asax` file.</span></span>

<span data-ttu-id="c6c2f-195">という名前の新しいスケーラビリティ機能*自動開始*ことアドレスこのシナリオでは使用可能な直接 ASP.NET 4 で実行すると Windows Server 2008 R2 に IIS 7.5 です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-195">A new scalability feature named *auto-start* that directly addresses this scenario is available when ASP.NET 4 runs on IIS 7.5 on Windows Server 2008 R2.</span></span> <span data-ttu-id="c6c2f-196">Auto-start 機能は、アプリケーション プールを開始、ASP.NET アプリケーションの初期化および HTTP 要求を受け入れるのための制御方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-196">The auto-start feature provides a controlled approach for starting up an application pool, initializing an ASP.NET application, and then accepting HTTP requests.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c6c2f-197">IIS 7.5 用の IIS アプリケーションのウォーム アップ モジュール</span><span class="sxs-lookup"><span data-stu-id="c6c2f-197">IIS Application Warm-Up Module for IIS 7.5</span></span>
> 
> <span data-ttu-id="c6c2f-198">IIS チームには、IIS 7.5 のアプリケーションのウォーム アップ モジュールの最初のベータ版のテスト バージョンがリリースされました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-198">The IIS team has released the first beta test version of the Application Warm-Up Module for IIS 7.5.</span></span> <span data-ttu-id="c6c2f-199">これにより、前に説明したよりも簡単にはアプリケーションを準備します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-199">This makes warming up your applications even easier than previously described.</span></span> <span data-ttu-id="c6c2f-200">カスタム コードを記述する代わりに、Web アプリケーションがネットワークからの要求を受け入れる前に実行するリソースの Url を指定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-200">Instead of writing custom code, you specify the URLs of resources to execute before the Web application accepts requests from the network.</span></span> <span data-ttu-id="c6c2f-201">IIS サービスの起動中に発生したこのウォーム アップ (として IIS アプリケーション プールを構成した場合*AlwaysRunning*) および IIS ワーカー プロセスがリサイクルされるときにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-201">This warm-up occurs during startup of the IIS service (if you configured the IIS application pool as *AlwaysRunning*) and when an IIS worker process recycles.</span></span> <span data-ttu-id="c6c2f-202">、リサイクル中には、以前の IIS ワーカー プロセスは、アプリケーションなしの中断または unprimed キャッシュのためには、その他の問題が発生するように、新しく起動されたワーカー プロセスのウォーム アップ、が完全にするまで要求を実行し続けます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-202">During recycle, the old IIS worker process continues to execute requests until the newly spawned worker process is fully warmed up, so that applications experience no interruptions or other issues due to unprimed caches.</span></span> <span data-ttu-id="c6c2f-203">このモジュールは、ASP.NET、バージョン 2.0 以降の任意のバージョンで動作する注意してください。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-203">Note that this module works with any version of ASP.NET, starting with version 2.0.</span></span>
> 
> <span data-ttu-id="c6c2f-204">詳細については、次を参照してください。 [Application Warm-Up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) IIS.net Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-204">For more information, see [Application Warm-Up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) on the IIS.net Web site.</span></span> <span data-ttu-id="c6c2f-205">ウォーム アップ機能を使用する方法について説明するチュートリアルでは、次を参照してください。 [IIS 7.5 アプリケーション ウォーム アップ モジュールの使用を開始する](https://www.iis.net/learn/manage)IIS.net Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-205">For a walkthrough that illustrates how to use the warm-up feature, see [Getting Started with the IIS 7.5 Application Warm-Up Module](https://www.iis.net/learn/manage) on the IIS.net Web site.</span></span>


<span data-ttu-id="c6c2f-206">自動開始機能を使用して、IIS 管理者では、次の構成を使用して自動的に起動する IIS 7.5 におけるアプリケーション プールを設定、`applicationHost.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-206">To use the auto-start feature, an IIS administrator sets an application pool in IIS 7.5 to be automatically started by using the following configuration in the `applicationHost.config` file:</span></span>

[!code-xml[Main](overview/samples/sample5.xml)]

<span data-ttu-id="c6c2f-207">次の構成を使用して自動的に開始する個々 のアプリケーションを指定する単一のアプリケーション プールには、複数のアプリケーションが含まれていることができます、ため、`applicationHost.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-207">Because a single application pool can contain multiple applications, you specify individual applications to be automatically started by using the following configuration in the `applicationHost.config` file:</span></span>

[!code-xml[Main](overview/samples/sample6.xml)]

<span data-ttu-id="c6c2f-208">IIS 7.5 がで情報を使用して IIS 7.5 サーバーは、コールドによって開始されたときに、または個々 のアプリケーション プールがリサイクルされるときに、`applicationHost.config`ファイルを確認してどの Web アプリケーションは自動的に開始します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-208">When an IIS 7.5 server is cold-started or when an individual application pool is recycled, IIS 7.5 uses the information in the `applicationHost.config` file to determine which Web applications need to be automatically started.</span></span> <span data-ttu-id="c6c2f-209">Auto-start 用にマークされている各アプリケーションの IIS7.5 は ASP.NET 4 アプリケーションを起動するアプリケーション一時的に受け付けない状態で HTTP 要求に要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-209">For each application that is marked for auto-start, IIS7.5 sends a request to ASP.NET 4 to start the application in a state during which the application temporarily does not accept HTTP requests.</span></span> <span data-ttu-id="c6c2f-210">ASP.NET がによって定義された型をインスタンス化でこの状態であるときに、 *serviceAutoStartProvider* (ように、前の例で示した部分) の属性とそのパブリック エントリ ポイントへの呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-210">When it is in this state, ASP.NET instantiates the type defined by the *serviceAutoStartProvider* attribute (as shown in the previous example) and calls into its public entry point.</span></span>

<span data-ttu-id="c6c2f-211">実装することによって必要なエントリ ポイントとマネージの自動開始の種類を作成する、 *、IProcessHostPreloadClient*インターフェイス、次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-211">You create a managed auto-start type with the necessary entry point by implementing the *IProcessHostPreloadClient* interface, as shown in the following example:</span></span>

[!code-csharp[Main](overview/samples/sample7.cs)]

<span data-ttu-id="c6c2f-212">初期化後にコードが実行されます、*プリロード*メソッドおよびメソッドが戻る、ASP.NET アプリケーションが要求を処理できる状態にします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-212">After your initialization code runs in the *Preload* method and the method returns, the ASP.NET application is ready to process requests.</span></span>

<span data-ttu-id="c6c2f-213">.5 IIS および ASP.NET 4 に自動開始の追加により、最初の HTTP 要求を処理する前に、負荷の高いアプリケーションの初期化を実行するための適切に定義されたアプローチがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-213">With the addition of auto-start to IIS .5 and ASP.NET 4, you now have a well-defined approach for performing expensive application initialization prior to processing the first HTTP request.</span></span> <span data-ttu-id="c6c2f-214">たとえば、アプリケーションを初期化して、アプリケーションが初期化され、HTTP トラフィックを受け入れる準備ができているロード バランサーを通知する新しい自動開始機能を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-214">For example, you can use the new auto-start feature to initialize an application and then signal a load-balancer that the application was initialized and ready to accept HTTP traffic.</span></span>

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a><span data-ttu-id="c6c2f-215">永続的なリダイレクト ページ</span><span class="sxs-lookup"><span data-stu-id="c6c2f-215">Permanently Redirecting a Page</span></span>

<span data-ttu-id="c6c2f-216">を超えて繰り返し発生する検索エンジンに古いリンクの基になる時間の経過と共にページおよびその他のコンテンツの周囲を移動する Web アプリケーションでよくあることを勧めします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-216">It is common practice in Web applications to move pages and other content around over time, which can lead to an accumulation of stale links in search engines.</span></span> <span data-ttu-id="c6c2f-217">ASP.NET では、開発者が従来古い Url への要求を使用して処理を使用して、 *Response.Redirect*新しい URL に要求を転送する方法です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-217">In ASP.NET, developers have traditionally handled requests to old URLs by using by using the *Response.Redirect* method to forward a request to the new URL.</span></span> <span data-ttu-id="c6c2f-218">ただし、*リダイレクト*メソッドは、ユーザーが、古い Url にアクセスしようとしています。 余分な http ラウンド トリップにより、HTTP 302 検出 (一時的なリダイレクト) 応答を発行します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-218">However, the *Redirect* method issues an HTTP 302 Found (temporary redirect) response, which results in an extra HTTP round trip when users attempt to access the old URLs.</span></span>

<span data-ttu-id="c6c2f-219">ASP.NET 4 で追加、新しい*RedirectPermanent*容易問題 HTTP 301 にヘルパー メソッドは恒久的次の例のように、応答。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-219">ASP.NET 4 adds a new *RedirectPermanent* helper method that makes it easy to issue HTTP 301 Moved Permanently responses, as in the following example:</span></span>

[!code-csharp[Main](overview/samples/sample8.cs)]

<span data-ttu-id="c6c2f-220">検索エンジンと永続的なリダイレクトを認識できる他のユーザー エージェントは、一時的なリダイレクトのブラウザーによって行われた、不要なラウンド トリップが不要になるため、コンテンツに関連付けられている新しい URL を格納します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-220">Search engines and other user agents that recognize permanent redirects will store the new URL that is associated with the content, which eliminates the unnecessary round trip made by the browser for temporary redirects.</span></span>

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a><span data-ttu-id="c6c2f-221">セッション状態の圧縮</span><span class="sxs-lookup"><span data-stu-id="c6c2f-221">Shrinking Session State</span></span>

<span data-ttu-id="c6c2f-222">ASP.NET には、Web ファーム上でセッション状態を格納するための 2 つの既定オプションが用意されています。 セッション状態プロバイダー、アウト プロセス セッション状態サーバーを呼び出すと、Microsoft SQL Server データベースにデータを格納するセッション状態プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-222">ASP.NET provides two default options for storing session state across a Web farm: a session-state provider that invokes an out-of-process session-state server, and a session-state provider that stores data in a Microsoft SQL Server database.</span></span> <span data-ttu-id="c6c2f-223">両方のオプションは、Web アプリケーションのワーカー プロセスの外部の状態情報を格納する、ために、リモート記憶域に送信される前にシリアル化するセッション状態があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-223">Because both options involve storing state information outside a Web application's worker process, session state has to be serialized before it is sent to remote storage.</span></span> <span data-ttu-id="c6c2f-224">、開発者は、セッション状態で保存される情報量によって、シリアル化データのサイズが非常に大きいまで拡大できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-224">Depending on how much information a developer saves in session state, the size of the serialized data can grow quite large.</span></span>

<span data-ttu-id="c6c2f-225">ASP.NET 4 では、両方のアウト プロセス セッション状態プロバイダーの新しい圧縮オプションが導入されています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-225">ASP.NET 4 introduces a new compression option for both kinds of out-of-process session-state providers.</span></span> <span data-ttu-id="c6c2f-226">ときに、 *compressionEnabled*に次の例に示すように構成オプションが設定されている*true*ASP.NET は圧縮 (および圧縮解除) .NET Frameworkを使用してシリアル化されたセッションの状態*System.IO.Compression.GZipStream*クラスです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-226">When the *compressionEnabled* configuration option shown in the following example is set to *true*, ASP.NET will compress (and decompress) serialized session state by using the .NET Framework *System.IO.Compression.GZipStream* class.</span></span>

[!code-xml[Main](overview/samples/sample9.xml)]

<span data-ttu-id="c6c2f-227">単純な新しい属性を追加すると、`Web.config`ファイル、Web サーバー上の予備の CPU サイクルを持つアプリケーションは、シリアル化されたセッション状態データのサイズを大幅に削減を実現できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-227">With the simple addition of the new attribute to the `Web.config` file, applications with spare CPU cycles on Web servers can realize substantial reductions in the size of serialized session-state data.</span></span>

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a><span data-ttu-id="c6c2f-228">許容される Url の範囲を拡張</span><span class="sxs-lookup"><span data-stu-id="c6c2f-228">Expanding the Range of Allowable URLs</span></span>

<span data-ttu-id="c6c2f-229">ASP.NET 4 には、アプリケーションの Url のサイズを拡大するための新しいオプションが導入されています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-229">ASP.NET 4 introduces new options for expanding the size of application URLs.</span></span> <span data-ttu-id="c6c2f-230">ASP.NET の以前のバージョンには、NTFS ファイル パスの制限に基づく、260 文字までに URL パスの長さが制限されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-230">Previous versions of ASP.NET constrained URL path lengths to 260 characters, based on the NTFS file-path limit.</span></span> <span data-ttu-id="c6c2f-231">ASP.NET 4 がある (増減) この制限として、アプリケーションの適切なオプションを使用して 2 つの新しい*httpRuntime*構成属性。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-231">In ASP.NET 4, you have the option to increase (or decrease) this limit as appropriate for your applications, using two new *httpRuntime* configuration attributes.</span></span> <span data-ttu-id="c6c2f-232">次の例では、これらの新しい属性を示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-232">The following example shows these new attributes.</span></span>

[!code-xml[Main](overview/samples/sample10.xml)]

<span data-ttu-id="c6c2f-233">長いまたは短いパス (プロトコル、サーバー名、およびクエリ文字列を含まない URL の部分) を許可するのには、変更、 *[済みの maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* 属性。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-233">To allow longer or shorter paths (the portion of the URL that does not include protocol, server name, and query string), modify the *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* attribute.</span></span> <span data-ttu-id="c6c2f-234">長いまたは短いクエリ文字列を許可するには、値を変更、 *[済みの maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* 属性。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-234">To allow longer or shorter query strings, modify the value of the *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* attribute.</span></span>

<span data-ttu-id="c6c2f-235">ASP.NET 4 では、URL 文字チェックで使用される文字を構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-235">ASP.NET 4 also enables you to configure the characters that are used by the URL character check.</span></span> <span data-ttu-id="c6c2f-236">ASP.NET には、URL のパス部分の無効な文字が検出されると、これは、要求を拒否し、HTTP 400 エラーを発行します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-236">When ASP.NET finds an invalid character in the path portion of a URL, it rejects the request and issues an HTTP 400 error.</span></span> <span data-ttu-id="c6c2f-237">ASP.NET の以前のバージョンでは、URL の文字のチェックは、文字の固定セットに制限されていました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-237">In previous versions of ASP.NET, the URL character checks were limited to a fixed set of characters.</span></span> <span data-ttu-id="c6c2f-238">ASP.NET 4 では、new を使用して有効な文字のセットをカスタマイズできます*requestPathInvalidChars*の属性、 *httpRuntime*構成要素を次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-238">In ASP.NET 4, you can customize the set of valid characters using the new *requestPathInvalidChars* attribute of the *httpRuntime* configuration element, as shown in the following example:</span></span>

[!code-xml[Main](overview/samples/sample11.xml)]

<span data-ttu-id="c6c2f-239">既定では、 <em>requestPathInvalidChars</em>属性は無効として 8 文字を定義します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-239">By default, the <em>requestPathInvalidChars</em> attribute defines eight characters as invalid.</span></span> <span data-ttu-id="c6c2f-240">(に割り当てられている文字列で<em>requestPathInvalidChars</em>既定では<em>、</em>より小さい (&lt;)、大なり (&gt;)、およびアンパサンド (&amp;) 文字は、エンコードされたため、`Web.config`ファイルは XML ファイルです)。無効な文字のセットは、必要に応じてカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-240">(In the string that is assigned to <em>requestPathInvalidChars</em> by default<em>,</em>the less than (&lt;), greater than (&gt;), and ampersand (&amp;) characters are encoded, because the `Web.config` file is an XML file.) You can customize the set of invalid characters as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="c6c2f-241">ASP.NET 4 は 0x00 ~ 0x1F の ASCII の範囲の文字を含む URL のパスを常に拒否の IETF RFC 2396 で定義されているは URL の無効な文字であるため注意してください ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt))。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-241">Note ASP.NET 4 always rejects URL paths that contain characters in the ASCII range of 0x00 to 0x1F, because those are invalid URL characters as defined in RFC 2396 of the IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)).</span></span> <span data-ttu-id="c6c2f-242">IIS 6 を実行する Windows Server のバージョンにまたは以降では、http.sys プロトコルのデバイス ドライバーに自動的に拒否 Url これらの文字でします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-242">On versions of Windows Server that run IIS 6 or higher, the http.sys protocol device driver automatically rejects URLs with these characters.</span></span>


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a><span data-ttu-id="c6c2f-243">拡張可能な要求の検証</span><span class="sxs-lookup"><span data-stu-id="c6c2f-243">Extensible Request Validation</span></span>

<span data-ttu-id="c6c2f-244">ASP.NET 要求の検証は、クロスサイト スクリプト (XSS) 攻撃によく使用される文字列の入力方向の HTTP 要求データを検索します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-244">ASP.NET request validation searches incoming HTTP request data for strings that are commonly used in cross-site scripting (XSS) attacks.</span></span> <span data-ttu-id="c6c2f-245">潜在的な XSS 文字列が見つかった場合、要求の検証は、問題のある文字列のフラグを設定し、エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-245">If potential XSS strings are found, request validation flags the suspect string and returns an error.</span></span> <span data-ttu-id="c6c2f-246">組み込みの要求の検証は、XSS 攻撃で使用される最も一般的な文字列が見つかった場合にのみ、エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-246">The built-in request validation returns an error only when it finds the most common strings used in XSS attacks.</span></span> <span data-ttu-id="c6c2f-247">誤検知が多すぎます XSS 検証をより積極的に実行する前の試行が発生しました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-247">Previous attempts to make the XSS validation more aggressive resulted in too many false positives.</span></span> <span data-ttu-id="c6c2f-248">ただし、環境では、比較的余裕のない、または逆にすることも XSS を意図的に緩和される要求の検証は、特定のページまたは特定の種類の要求を確認することがあります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-248">However, customers might want request validation that is more aggressive, or conversely might want to intentionally relax XSS checks for specific pages or for specific types of requests.</span></span>

<span data-ttu-id="c6c2f-249">ASP.NET 4 では、要求の検証機能が行われて拡張可能な要求のカスタム検証ロジックを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-249">In ASP.NET 4, the request validation feature has been made extensible so that you can use custom request-validation logic.</span></span> <span data-ttu-id="c6c2f-250">新しいから派生するクラスを作成する要求の検証を拡張する*System.Web.Util.RequestValidator*型とするアプリケーションの構成 (で、 *httpRuntime* のセクション`Web.config`ファイル) をカスタムの型を使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-250">To extend request validation, you create a class that derives from the new *System.Web.Util.RequestValidator* type, and you configure the application (in the *httpRuntime* section of the `Web.config` file) to use the custom type.</span></span> <span data-ttu-id="c6c2f-251">次の例では、要求のカスタム検証クラスを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-251">The following example shows how to configure a custom request-validation class:</span></span>

[!code-xml[Main](overview/samples/sample12.xml)]

<span data-ttu-id="c6c2f-252">新しい*requestValidationType*属性には、標準 .NET Framework 型識別子を指定する文字列をカスタム要求の検証を提供するクラスが必要です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-252">The new *requestValidationType* attribute requires a standard .NET Framework type identifier string that specifies the class that provides custom request validation.</span></span> <span data-ttu-id="c6c2f-253">各要求に対しては、ASP.NET は、受信 HTTP 要求データの各部分を処理するカスタム型を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-253">For each request, ASP.NET invokes the custom type to process each piece of incoming HTTP request data.</span></span> <span data-ttu-id="c6c2f-254">受信 URL、すべての HTTP ヘッダー (クッキーとカスタム ヘッダーの両方)、およびエンティティ ボディはすべて次の例に示すようなカスタム要求の検証クラスによって検査で利用できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-254">The incoming URL, all the HTTP headers (both cookies and custom headers), and the entity body are all available for inspection by a custom request validation class like that shown in the following example:</span></span>

[!code-csharp[Main](overview/samples/sample13.cs)]

<span data-ttu-id="c6c2f-255">入力方向の HTTP データの一部を検査しない場合、要求の検証クラスが切り替えられる ASP.NET 既定の要求の検証だけで呼び出すことによって実行できるようにする*ベースです。IsValidRequestString です。*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-255">For cases where you do not want to inspect a piece of incoming HTTP data, the request-validation class can fall back to let the ASP.NET default request validation run by simply calling *base.IsValidRequestString.*</span></span>

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a><span data-ttu-id="c6c2f-256">オブジェクト キャッシュとキャッシュ機能拡張オブジェクト</span><span class="sxs-lookup"><span data-stu-id="c6c2f-256">Object Caching and Object Caching Extensibility</span></span>

<span data-ttu-id="c6c2f-257">最初のリリース以降、ASP.NET の強力なメモリ内オブジェクト キャッシュが含まれる (*System.Web.Caching.Cache*)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-257">Since its first release, ASP.NET has included a powerful in-memory object cache (*System.Web.Caching.Cache*).</span></span> <span data-ttu-id="c6c2f-258">キャッシュの実装は非常に普及以外の Web アプリケーションで使用されてきました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-258">The cache implementation has been so popular that it has been used in non-Web applications.</span></span> <span data-ttu-id="c6c2f-259">ただし、これはへの参照を含むように Windows フォームや WPF アプリケーションの不適切な`System.Web.dll`ASP.NET オブジェクトのキャッシュを使用できるだけにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-259">However, it is awkward for a Windows Forms or WPF application to include a reference to `System.Web.dll` just to be able to use the ASP.NET object cache.</span></span>

<span data-ttu-id="c6c2f-260">キャッシュ使用できるようにするすべてのアプリケーションに対して、.NET Framework 4 には、新しいアセンブリ、新しい名前空間、いくつかの基本型、および実装をキャッシュする具象型が導入されています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-260">To make caching available for all applications, the .NET Framework 4 introduces a new assembly, a new namespace, some base types, and a concrete caching implementation.</span></span> <span data-ttu-id="c6c2f-261">新しい`System.Runtime.Caching.dll`アセンブリには、新しいキャッシュ API が含まれている、 *System.Runtime.Caching*名前空間。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-261">The new `System.Runtime.Caching.dll` assembly contains a new caching API in the *System.Runtime.Caching* namespace.</span></span> <span data-ttu-id="c6c2f-262">名前空間には、クラスの 2 つのコア セットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-262">The namespace contains two core sets of classes:</span></span>

- <span data-ttu-id="c6c2f-263">カスタムのキャッシュ実装の任意の型を構築するための基盤を提供する抽象型です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-263">Abstract types that provide the foundation for building any type of custom cache implementation.</span></span>
- <span data-ttu-id="c6c2f-264">具体的なメモリ内のオブジェクトのキャッシュ実装 (、 *System.Runtime.Caching.MemoryCache*クラス)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-264">A concrete in-memory object cache implementation (the *System.Runtime.Caching.MemoryCache* class).</span></span>

<span data-ttu-id="c6c2f-265">新しい*MemoryCache* ASP.NET キャッシュで密接にモデル化されたクラスと、ASP.NET とエンジンの内部キャッシュ ロジックの多くを共有します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-265">The new *MemoryCache* class is modeled closely on the ASP.NET cache, and it shares much of the internal cache engine logic with ASP.NET.</span></span> <span data-ttu-id="c6c2f-266">パブリックのキャッシュ Api *System.Runtime.Caching* ASP.NET を使用している場合に、カスタム キャッシュの開発をサポートするために更新された*キャッシュ*オブジェクトでの一般的な概念が見つかります、新しい Api です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-266">Although the public caching APIs in *System.Runtime.Caching* have been updated to support development of custom caches, if you have used the ASP.NET *Cache* object, you will find familiar concepts in the new APIs.</span></span>

<span data-ttu-id="c6c2f-267">新しいの詳しい説明*MemoryCache*はクラスと基本 Api をサポートするドキュメント全体が必要です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-267">An in-depth discussion of the new *MemoryCache* class and supporting base APIs would require an entire document.</span></span> <span data-ttu-id="c6c2f-268">ただし、次の例は、新しいキャッシュ API のしくみの概要を把握します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-268">However, the following example gives you an idea of how the new cache API works.</span></span> <span data-ttu-id="c6c2f-269">例は、任意の依存関係なしの Windows フォーム アプリケーション向けに書かれた`System.Web.dll`です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-269">The example was written for a Windows Forms application, without any dependency on `System.Web.dll`.</span></span>

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a><span data-ttu-id="c6c2f-270">拡張可能な HTML、URL、および HTTP ヘッダーのエンコーディング</span><span class="sxs-lookup"><span data-stu-id="c6c2f-270">Extensible HTML, URL, and HTTP Header Encoding</span></span>

<span data-ttu-id="c6c2f-271">ASP.NET 4 では、次の一般的なテキスト エンコーディング タスクのカスタム エンコード ルーチンを作成できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-271">In ASP.NET 4, you can create custom encoding routines for the following common text-encoding tasks:</span></span>

- <span data-ttu-id="c6c2f-272">HTML エンコード。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-272">HTML encoding.</span></span>
- <span data-ttu-id="c6c2f-273">URL エンコードします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-273">URL encoding.</span></span>
- <span data-ttu-id="c6c2f-274">HTML 属性エンコード。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-274">HTML attribute encoding.</span></span>
- <span data-ttu-id="c6c2f-275">出力方向の HTTP ヘッダーのエンコーディング。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-275">Encoding outbound HTTP headers.</span></span>

<span data-ttu-id="c6c2f-276">カスタム エンコーダーを作成するには、新しいから派生することによって*System.Web.Util.HttpEncoder*にカスタムの型を使用する型と ASP.NET を構成、 *httpRuntime*のセクションで、`Web.config`ファイルとして次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-276">You can create a custom encoder by deriving from the new *System.Web.Util.HttpEncoder* type and then configuring ASP.NET to use the custom type in the *httpRuntime* section of the `Web.config` file, as shown in the following example:</span></span>

[!code-xml[Main](overview/samples/sample15.xml)]

<span data-ttu-id="c6c2f-277">ASP.NET が自動的にエンコーディングのメソッドを公開するたびに、カスタム エンコード実装を呼び出すカスタム エンコーダーを構成した後、 *System.Web.HttpUtility*または*System.Web.HttpServerUtility*クラスと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-277">After a custom encoder has been configured, ASP.NET automatically calls the custom encoding implementation whenever public encoding methods of the *System.Web.HttpUtility* or *System.Web.HttpServerUtility* classes are called.</span></span> <span data-ttu-id="c6c2f-278">これにより、Web 開発チームの Api をエンコードするパブリックの ASP.NET を使用して Web 開発チームの残りの部分がし続けながら、余裕のない文字のエンコードを実装するカスタム エンコーダーを作成する 1 つの部分です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-278">This lets one part of a Web development team create a custom encoder that implements aggressive character encoding, while the rest of the Web development team continues to use the public ASP.NET encoding APIs.</span></span> <span data-ttu-id="c6c2f-279">カスタム エンコーダーを一元的に構成することによって、 *httpRuntime*要素、カスタム エンコーダーでエンコード Api パブリック ASP.NET からのすべてのテキスト エンコーディングの呼び出しがルーティングされる保証されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-279">By centrally configuring a custom encoder in the *httpRuntime* element, you are guaranteed that all text-encoding calls from the public ASP.NET encoding APIs are routed through the custom encoder.</span></span>

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a><span data-ttu-id="c6c2f-280">1 つのワーカー プロセス内の個々 のアプリケーションのパフォーマンスを監視</span><span class="sxs-lookup"><span data-stu-id="c6c2f-280">Performance Monitoring for Individual Applications in a Single Worker Process</span></span>

<span data-ttu-id="c6c2f-281">1 台のサーバーでホストできる Web サイトの数を増やすためには、多くのホスト側は、1 つのワーカー プロセスで複数の ASP.NET アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-281">In order to increase the number of Web sites that can be hosted on a single server, many hosters run multiple ASP.NET applications in a single worker process.</span></span> <span data-ttu-id="c6c2f-282">ただし、複数のアプリケーションでは、1 つの共有ワーカー プロセスを使用する場合は、サーバー管理者の問題が発生している個々 のアプリケーションを識別するが困難です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-282">However, if multiple applications use a single shared worker process, it is difficult for server administrators to identify an individual application that is experiencing problems.</span></span>

<span data-ttu-id="c6c2f-283">ASP.NET 4 では、CLR によって導入された新しいリソース監視機能を活用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-283">ASP.NET 4 leverages new resource-monitoring functionality introduced by the CLR.</span></span> <span data-ttu-id="c6c2f-284">この機能を有効にするには、次の XML 構成スニペットを追加することができます、`aspnet.config`構成ファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-284">To enable this functionality, you can add the following XML configuration snippet to the `aspnet.config` configuration file.</span></span>

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> <span data-ttu-id="c6c2f-285">注、`aspnet.config`ファイルが .NET Framework がインストールされているディレクトリにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-285">Note The `aspnet.config` file is in the directory where the .NET Framework is installed.</span></span> <span data-ttu-id="c6c2f-286">`Web.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-286">It is not the `Web.config` file.</span></span>


<span data-ttu-id="c6c2f-287">ときに、 *appDomainResourceMonitoring*機能が有効になって、2 つの新しいパフォーマンス カウンターは、"ASP.NET Applications"のパフォーマンス カテゴリで利用可能な: *% プロセッサ時間のマネージ*と*マネージ メモリの使用*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-287">When the *appDomainResourceMonitoring* feature has been enabled, two new performance counters are available in the "ASP.NET Applications" performance category: *% Managed Processor Time* and *Managed Memory Used*.</span></span> <span data-ttu-id="c6c2f-288">これら両方のパフォーマンス カウンターは、推定の CPU 時間と個々 の ASP.NET アプリケーションの管理対象のメモリ使用率を追跡するために新しい CLR アプリケーション ドメインのリソース管理機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-288">Both of these performance counters use the new CLR application-domain resource management feature to track estimated CPU time and managed memory utilization of individual ASP.NET applications.</span></span> <span data-ttu-id="c6c2f-289">その結果、ASP.NET 4 で管理者ようになりましたがある 1 つのワーカー プロセスで実行されている個々 のアプリケーションのリソース消費量により詳細なビューです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-289">As a result, with ASP.NET 4, administrators now have a more granular view into the resource consumption of individual applications running in a single worker process.</span></span>

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a><span data-ttu-id="c6c2f-290">マルチ ターゲット</span><span class="sxs-lookup"><span data-stu-id="c6c2f-290">Multi-Targeting</span></span>

<span data-ttu-id="c6c2f-291">.NET Framework の特定のバージョンを対象とするアプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-291">You can create an application that targets a specific version of the .NET Framework.</span></span> <span data-ttu-id="c6c2f-292">ASP.NET 4 で、新しい属性で、*コンパイル*の要素、`Web.config`ファイルでは、.NET Framework 4 以降を対象することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-292">In ASP.NET 4, a new attribute in the *compilation* element of the `Web.config` file lets you target the .NET Framework 4 and later.</span></span> <span data-ttu-id="c6c2f-293">省略可能な要素を含める場合と明示的に、.NET Framework 4 を対象とする場合、`Web.config`ファイルのエントリなど*system.codedom*、これらの要素は、.NET Framework 4 に対して適切である必要があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-293">If you explicitly target the .NET Framework 4, and if you include optional elements in the `Web.config` file such as the entries for *system.codedom*, these elements must be correct for the .NET Framework 4.</span></span> <span data-ttu-id="c6c2f-294">(ターゲット フレームワークは内のエントリがないことから推論する明示的を対象としない、.NET Framework 4 の場合、`Web.config`ファイルです)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-294">(If you do not explicitly target the .NET Framework 4, the target framework is inferred from the lack of an entry in the `Web.config` file.)</span></span>

<span data-ttu-id="c6c2f-295">次の例では、使用、 *targetFramework*属性、*コンパイル*の要素、`Web.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-295">The following example shows the use of the *targetFramework* attribute in the *compilation* element of the `Web.config` file.</span></span>

[!code-xml[Main](overview/samples/sample17.xml)]

<span data-ttu-id="c6c2f-296">.NET Framework の特定のバージョンを対象とする際は、次に注意してください。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-296">Note the following about targeting a specific version of the .NET Framework:</span></span>

- <span data-ttu-id="c6c2f-297">.NET Framework 4 のアプリケーション プールでは、ASP.NET ビルド システムと見なされます、.NET Framework 4 をターゲットとして、`Web.config`ファイルは含まれません、 *targetFramework*属性場合、または、`Web.config`ファイルが見つかりません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-297">In a .NET Framework 4 application pool, the ASP.NET build system assumes the .NET Framework 4 as a target if the `Web.config` file does not include the *targetFramework* attribute or if the `Web.config` file is missing.</span></span> <span data-ttu-id="c6c2f-298">(.NET Framework 4 で実行されるようにアプリケーションをコーディングに変更する必要があります)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-298">(You might have to make coding changes to your application to make it run under the .NET Framework 4.)</span></span>
- <span data-ttu-id="c6c2f-299">必要な場合、 *targetFramework*属性、および場合、 *system.codeDom*で要素が定義されている、`Web.config`ファイル、このファイルは、.NET Framework 4 の正しいエントリを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-299">If you do include the *targetFramework* attribute, and if the *system.codeDom* element is defined in the `Web.config` file, this file must contain the correct entries for the .NET Framework 4.</span></span>
- <span data-ttu-id="c6c2f-300">使用している場合、 *aspnet\_コンパイラ*(ビルド環境など)、アプリケーションをプリコンパイルするコマンドを正しいバージョンを使用する必要があります、 *aspnet\_コンパイラ*ターゲット フレームワークのコマンド。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-300">If you are using the *aspnet\_compiler* command to precompile your application (such as in a build environment), you must use the correct version of the *aspnet\_compiler* command for the target framework.</span></span> <span data-ttu-id="c6c2f-301">.NET Framework 3.5 と以前のバージョンのコンパイルに .NET Framework 2.0 (%windir%\microsoft.net\framework\v2.0.50727) 同梱されているコンパイラを使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-301">Use the compiler that shipped with the .NET Framework 2.0 (%WINDIR%\Microsoft.NET\Framework\v2.0.50727) to compile for the .NET Framework 3.5 and earlier versions.</span></span> <span data-ttu-id="c6c2f-302">.NET Framework 4 で付属しているコンパイラを使用して、そのフレームワークを使用して、またはそれ以降のバージョンを使用して作成されたアプリケーションをコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-302">Use the compiler that ships with the .NET Framework 4 to compile applications created using that framework or using later versions.</span></span>
- <span data-ttu-id="c6c2f-303">コンパイラがコンピューターにインストールされている最新のフレームワーク アセンブリを使用して、実行時に (とそのため、GAC 内)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-303">At run time, the compiler uses the latest framework assemblies that are installed on the computer (and therefore in the GAC).</span></span> <span data-ttu-id="c6c2f-304">フレームワークに更新プログラムを後で行われた場合 (たとえば、仮定バージョン 4.1 がインストールされます)、いなくても、新しいバージョンのフレームワークで機能を使用することができます、 *targetFramework*属性ターゲットを下位バージョン(4.0 など)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-304">If an update is made later to the framework (for example, a hypothetical version 4.1 is installed), you will be able to use features in the newer version of the framework even though the *targetFramework* attribute targets a lower version (such as 4.0).</span></span> <span data-ttu-id="c6c2f-305">(ただし、デザイン時に Visual Studio 2010 または使用すると、 *aspnet\_コンパイラ*コマンド、フレームワークの最新の機能を使用すると、コンパイラ エラーが発生)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-305">(However, at design time in Visual Studio 2010 or when you use the *aspnet\_compiler* command, using newer features of the framework will cause compiler errors).</span></span>

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a><span data-ttu-id="c6c2f-306">Ajax</span><span class="sxs-lookup"><span data-stu-id="c6c2f-306">Ajax</span></span>

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a><span data-ttu-id="c6c2f-307">含まれる Web フォームと MVC の jQuery</span><span class="sxs-lookup"><span data-stu-id="c6c2f-307">jQuery Included with Web Forms and MVC</span></span>

<span data-ttu-id="c6c2f-308">Web フォームと MVC の両方の Visual Studio テンプレートには、オープン ソース jQuery ライブラリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-308">The Visual Studio templates for both Web Forms and MVC include the open-source jQuery library.</span></span> <span data-ttu-id="c6c2f-309">新しい web サイトまたはプロジェクトを作成するときに、次の 3 つのファイルを含むスクリプト フォルダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-309">When you create a new website or project, a Scripts folder containing the following 3 files is created:</span></span>

- <span data-ttu-id="c6c2f-310">jQuery-1.4.1.js –、人間が判読できるは unminified jQuery ライブラリのバージョンです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-310">jQuery-1.4.1.js – The human-readable, unminified version of the jQuery library.</span></span>
- <span data-ttu-id="c6c2f-311">jQuery-14.1.min.js – jQuery ライブラリの縮小されたバージョンです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-311">jQuery-14.1.min.js – The minified version of the jQuery library.</span></span>
- <span data-ttu-id="c6c2f-312">jQuery-1.4.1-vsdoc.js – jQuery ライブラリの Intellisense ドキュメント ファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-312">jQuery-1.4.1-vsdoc.js – The Intellisense documentation file for the jQuery library.</span></span>

<span data-ttu-id="c6c2f-313">アプリケーションの開発中には、jQuery の unminified バージョンを含めます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-313">Include the unminified version of jQuery while developing an application.</span></span> <span data-ttu-id="c6c2f-314">実可動アプリケーション用の jQuery の縮小されたバージョンが含まれます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-314">Include the minified version of jQuery for production applications.</span></span>

<span data-ttu-id="c6c2f-315">たとえば、次の Web フォーム ページは、jQuery を使用して、黄色のフォーカスがあるときに ASP.NET TextBox コントロールの背景色を変更する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-315">For example, the following Web Forms page illustrates how you can use jQuery to change the background color of ASP.NET TextBox controls to yellow when they have focus.</span></span>

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a><span data-ttu-id="c6c2f-316">コンテンツ配信ネットワークのサポート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-316">Content Delivery Network Support</span></span>

<span data-ttu-id="c6c2f-317">Microsoft Ajax コンテンツ配信ネットワーク (CDN) では、Web アプリケーションに ASP.NET Ajax および jQuery スクリプトを簡単に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-317">The Microsoft Ajax Content Delivery Network (CDN) enables you to easily add ASP.NET Ajax and jQuery scripts to your Web applications.</span></span> <span data-ttu-id="c6c2f-318">JQuery ライブラリを追加するだけで使用を開始するなど、`<script>`が指す Ajax.microsoft.com 次のように、ページへのタグ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-318">For example, you can start using the jQuery library simply by adding a `<script>` tag to your page that points to Ajax.microsoft.com like this:</span></span>

[!code-html[Main](overview/samples/sample19.html)]

<span data-ttu-id="c6c2f-319">Microsoft Ajax CDN を利用して、Ajax アプリケーションのパフォーマンスを大幅に向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-319">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="c6c2f-320">Microsoft Ajax CDN の内容は、世界各地に配置されたサーバーでキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-320">The contents of the Microsoft Ajax CDN are cached on servers located around the world.</span></span> <span data-ttu-id="c6c2f-321">さらに、Microsoft Ajax CDN では、ブラウザーは別のドメインに配置されている Web サイトの JavaScript のキャッシュ ファイルを再利用が有効にします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-321">In addition, the Microsoft Ajax CDN enables browsers to reuse cached JavaScript files for Web sites that are located in different domains.</span></span>

<span data-ttu-id="c6c2f-322">Microsoft Ajax Content Delivery Network は、Secure Sockets Layer を使用し、web ページを使用する必要がある場合に、SSL (HTTPS) をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-322">The Microsoft Ajax Content Delivery Network supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="c6c2f-323">CDN が利用できない場合は、フォールバックを実装します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-323">Implement a fallback when the CDN is unavailable.</span></span> <span data-ttu-id="c6c2f-324">フォールバックをテストします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-324">Test the fallback.</span></span>

<span data-ttu-id="c6c2f-325">Microsoft Ajax CDN の詳細については、次の web サイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-325">To learn more about the Microsoft Ajax CDN, visit the following website:</span></span>

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

<span data-ttu-id="c6c2f-326">ASP.NET ScriptManager は、Microsoft Ajax CDN をサポートします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-326">The ASP.NET ScriptManager supports the Microsoft Ajax CDN.</span></span> <span data-ttu-id="c6c2f-327">1 つのプロパティの設定、EnableCdn プロパティだけでは、CDN からすべての ASP.NET フレームワークの JavaScript ファイルを取得できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-327">Simply by setting one property, the EnableCdn property, you can retrieve all ASP.NET framework JavaScript files from the CDN:</span></span>

[!code-aspx[Main](overview/samples/sample20.aspx)]

<span data-ttu-id="c6c2f-328">EnableCdn プロパティを true に設定した後、ASP.NET framework は検証と UpdatePanel 使用されるすべての JavaScript ファイルを含む、CDN からすべての ASP.NET フレームワークの JavaScript ファイルを取得します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-328">After you set the EnableCdn property to the value true, the ASP.NET framework will retrieve all ASP.NET framework JavaScript files from the CDN including all JavaScript files used for validation and the UpdatePanel.</span></span> <span data-ttu-id="c6c2f-329">この 1 つのプロパティを設定すると、web アプリケーションのパフォーマンスに大きな影響があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-329">Setting this one property can have a dramatic impact on the performance of your web application.</span></span>

<span data-ttu-id="c6c2f-330">WebResource 属性を使用して、独自の JavaScript ファイル CDN パスを設定できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-330">You can set the CDN path for your own JavaScript files by using the WebResource attribute.</span></span> <span data-ttu-id="c6c2f-331">新しい CdnPath プロパティは、EnableCdn プロパティを true に設定するときに使用する CDN へのパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-331">The new CdnPath property specifies the path to the CDN used when you set the EnableCdn property to the value true:</span></span>

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a><span data-ttu-id="c6c2f-332">ScriptManager Explicit Scripts</span><span class="sxs-lookup"><span data-stu-id="c6c2f-332">ScriptManager Explicit Scripts</span></span>

<span data-ttu-id="c6c2f-333">以前は、ASP.NET ScriptManger を使用した場合、必要があるモノリシックな ASP.NET Ajax ライブラリ全体を読み込めません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-333">In the past, if you used the ASP.NET ScriptManger then you were required to load the entire monolithic ASP.NET Ajax Library.</span></span> <span data-ttu-id="c6c2f-334">新しい ScriptManager.AjaxFrameworkMode プロパティを利用して、ASP.NET Ajax ライブラリのコンポーネントが読み込まれる完全に制御し、必要な ASP.NET Ajax ライブラリのコンポーネントのみをロードできます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-334">By taking advantage of the new ScriptManager.AjaxFrameworkMode property, you can control exactly which components of the ASP.NET Ajax Library are loaded and load only the components of the ASP.NET Ajax Library that you need.</span></span>

<span data-ttu-id="c6c2f-335">ScriptManager.AjaxFrameworkMode プロパティは、次の値に設定できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-335">The ScriptManager.AjaxFrameworkMode property can be set to the following values:</span></span>

- <span data-ttu-id="c6c2f-336">-有効にするには、ScriptManager コントロールは、これは、結合したスクリプト ファイルのすべてのコア フレームワーク スクリプト (従来の動作)、MicrosoftAjax.js スクリプト ファイルを自動的に含まれていますを指定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-336">Enabled -- Specifies that the ScriptManager control automatically includes the MicrosoftAjax.js script file, which is a combined script file of every core framework script (legacy behavior).</span></span>
- <span data-ttu-id="c6c2f-337">無効になっている--には、すべての Microsoft Ajax スクリプト機能を無効になっていることと、ScriptManager コントロールは任意のスクリプトを自動的に参照していませんことを指定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-337">Disabled -- Specifies that all Microsoft Ajax script features are disabled and that the ScriptManager control does not reference any scripts automatically.</span></span>
- <span data-ttu-id="c6c2f-338">--明示的なを指定します、ページを必要とする個々 の framework core スクリプト ファイルへのスクリプト参照を明示的に含める、および各スクリプト ファイルを必要とする依存関係への参照にが含まれます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-338">Explicit -- Specifies that you will explicitly include script references to individual framework core script file that your page requires, and that you will include references to the dependencies that each script file requires.</span></span>

<span data-ttu-id="c6c2f-339">たとえば、AjaxFrameworkMode プロパティを明示的な値に設定した場合は、必要な特定の ASP.NET Ajax コンポーネントのスクリプトを指定できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-339">For example, if you set the AjaxFrameworkMode property to the value Explicit then you can specify the particular ASP.NET Ajax component scripts that you need:</span></span>

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a><span data-ttu-id="c6c2f-340">Web フォーム</span><span class="sxs-lookup"><span data-stu-id="c6c2f-340">Web Forms</span></span>

<span data-ttu-id="c6c2f-341">Web フォームは、ASP.NET 1.0 のリリース後に ASP.NET で中心的な機能にされました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-341">Web Forms has been a core feature in ASP.NET since the release of ASP.NET 1.0.</span></span> <span data-ttu-id="c6c2f-342">多くの機能強化は、次を含む、ASP.NET 4 用のこの領域にされています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-342">Many enhancements have been in this area for ASP.NET 4, including the following:</span></span>

- <span data-ttu-id="c6c2f-343">設定できる*メタ*タグ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-343">The ability to set *meta* tags.</span></span>
- <span data-ttu-id="c6c2f-344">ビュー ステートより詳細に制御します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-344">More control over view state.</span></span>
- <span data-ttu-id="c6c2f-345">ブラウザーの機能を使用する簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-345">Easier ways to work with browser capabilities.</span></span>
- <span data-ttu-id="c6c2f-346">ASP.NET Web フォームでのルーティングを使用するためのサポート。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-346">Support for using ASP.NET routing with Web Forms.</span></span>
- <span data-ttu-id="c6c2f-347">詳細に制御は、Id を生成します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-347">More control over generated IDs.</span></span>
- <span data-ttu-id="c6c2f-348">データ コントロールで選択された行を保持できるようにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-348">The ability to persist selected rows in data controls.</span></span>
- <span data-ttu-id="c6c2f-349">詳細な制御でレンダリングされる HTML、 *FormView*と*ListView*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-349">More control over rendered HTML in the *FormView* and *ListView* controls.</span></span>
- <span data-ttu-id="c6c2f-350">データ ソース コントロールのサポートをフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-350">Filtering support for data source controls.</span></span>

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a><span data-ttu-id="c6c2f-351">Page.MetaKeywords と Page.MetaDescription プロパティをメタ タグを設定</span><span class="sxs-lookup"><span data-stu-id="c6c2f-351">Setting Meta Tags with the Page.MetaKeywords and Page.MetaDescription Properties</span></span>

<span data-ttu-id="c6c2f-352">ASP.NET 4 で追加する 2 つのプロパティ、*ページ*クラス、 *MetaKeywords*と*MetaDescription*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-352">ASP.NET 4 adds two properties to the *Page* class, *MetaKeywords* and *MetaDescription*.</span></span> <span data-ttu-id="c6c2f-353">対応する 2 つのプロパティを表す*メタ*ページを開き、タグを次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-353">These two properties represent corresponding *meta* tags in your page, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample23.aspx)]

<span data-ttu-id="c6c2f-354">これら 2 つのプロパティは、同じ動作方法 ページの*タイトル*プロパティにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-354">These two properties work the same way that the page's *Title* property does.</span></span> <span data-ttu-id="c6c2f-355">これらのルールに従います。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-355">They follow these rules:</span></span>

1. <span data-ttu-id="c6c2f-356">ある場合ありません*メタ*内にタグを*ヘッド*プロパティ名と一致する要素 (は、名前 =「キーワード」 *Page.MetaKeywords*と名前の「説明」を=*Page.MetaDescription*、これらのプロパティが設定されていないことを意味) では、*メタ*タグは、レポートが表示されるページに追加されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-356">If there are no *meta* tags in the *head* element that match the property names (that is, name="keywords" for *Page.MetaKeywords* and name="description" for *Page.MetaDescription*, meaning that these properties have not been set), the *meta* tags will be added to the page when it is rendered.</span></span>
2. <span data-ttu-id="c6c2f-357">既にある場合*メタ*get し、set メソッド、既存のタグの内容としてにこれらの名前を持つタグは、これらのプロパティの動作です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-357">If there are already *meta* tags with these names, these properties act as get and set methods for the contents of the existing tags.</span></span>

<span data-ttu-id="c6c2f-358">データベースまたはその他のソースからコンテンツを取得することができ、作業内容を動的にタグを設定することができます、実行時にこれらのプロパティを設定するには、特定のページです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-358">You can set these properties at run time, which lets you get the content from a database or other source, and which lets you set the tags dynamically to describe what a particular page is for.</span></span>

<span data-ttu-id="c6c2f-359">設定することも、*キーワード*と*説明*プロパティで、 *@ Page*次の例のように、Web フォーム ページのマークアップの上部にあるディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-359">You can also set the *Keywords* and *Description* properties in the *@ Page* directive at the top of the Web Forms page markup, as in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample24.aspx)]

<span data-ttu-id="c6c2f-360">これよりも優先されます、*メタ*タグ内容 (存在する場合) がページで既に宣言されています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-360">This will override the *meta* tag contents (if any) already declared in the page.</span></span>

<span data-ttu-id="c6c2f-361">説明の内容*メタ*タグは Google でのプレビューを一覧表示する検索を向上させるために使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-361">The contents of the description *meta* tag are used for improving search listing previews in Google.</span></span> <span data-ttu-id="c6c2f-362">(詳細については、「[メタ説明作り直しでスニペットを向上させる](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html)Google web マスター サーバー全体のブログです。)Google や Windows Live Search では、任意のキーワードの内容は使用しないで、その他の検索エンジン可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-362">(For details, see [Improve snippets with a meta description makeover](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) on the Google Webmaster Central blog.) Google and Windows Live Search do not use the contents of the keywords for anything, but other search engines might.</span></span> <span data-ttu-id="c6c2f-363">詳細については、次を参照してください。[メタ キーワード アドバイス](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php)検索エンジンのガイドの Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-363">For more information, see [Meta Keywords Advice](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) on the Search Engine Guide Web site.</span></span>

<span data-ttu-id="c6c2f-364">これらの新しいプロパティは単純な機能が、これらを手動で追加の要件やを作成するコードの記述から保存した、*メタ*タグ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-364">These new properties are a simple feature, but they save you from the requirement to add these manually or from writing your own code to create the *meta* tags.</span></span>

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a><span data-ttu-id="c6c2f-365">個々 のコントロール ビュー ステートを有効にします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-365">Enabling View State for Individual Controls</span></span>

<span data-ttu-id="c6c2f-366">既定では、ビュー ステートは、アプリケーションの必要がない場合でも、ページ上の各コントロールがそのビュー ステートを可能性がある格納する結果のページで有効です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-366">By default, view state is enabled for the page, with the result that each control on the page potentially stores view state even if it is not required for the application.</span></span> <span data-ttu-id="c6c2f-367">ページが生成し、ページをクライアントに送信し、バックアップに要する時間の増加は、マークアップでビュー状態データが含まれます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-367">View state data is included in the markup that a page generates and increases the amount of time it takes to send a page to the client and post it back.</span></span> <span data-ttu-id="c6c2f-368">必要以上のビュー ステートを格納すると、大幅なパフォーマンスの低下を引き起こすことができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-368">Storing more view state than is necessary can cause significant performance degradation.</span></span> <span data-ttu-id="c6c2f-369">以前のバージョンの ASP.NET では、開発者は、ページ サイズを小さくために、個々 のコントロールのビューステートを無効には、個々 のコントロールのために明示的に行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-369">In earlier versions of ASP.NET, developers could disable view state for individual controls in order to reduce page size, but had to do so explicitly for individual controls.</span></span> <span data-ttu-id="c6c2f-370">Web サーバー コントロールには、ASP.NET 4 で、 *ViewStateMode*プロパティを使用する既定のビュー ステートを無効にして、ページで必要とするコントロールに対してのみ有効にします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-370">In ASP.NET 4, Web server controls include a *ViewStateMode* property that lets you disable view state by default and then enable it only for the controls that require it in the page.</span></span>

<span data-ttu-id="c6c2f-371">*ViewStateMode*プロパティを 3 つの値を持つ列挙体には:*有効*、*無効になっている*、および*継承*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-371">The *ViewStateMode* property takes an enumeration that has three values: *Enabled*, *Disabled*, and *Inherit*.</span></span> <span data-ttu-id="c6c2f-372">*有効になっている*有効にし、そのコントロールのすべての子コントロールに設定されている状態の表示*継承*ことか、または何も設定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-372">*Enabled* enables view state for that control and for any child controls that are set to *Inherit* or that have nothing set.</span></span> <span data-ttu-id="c6c2f-373">*無効になっている*ビュー ステートを無効にし、*継承*コントロールが使用するように指定、 *ViewStateMode*親コントロールから設定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-373">*Disabled* disables view state, and *Inherit* specifies that the control uses the *ViewStateMode* setting from the parent control.</span></span>

<span data-ttu-id="c6c2f-374">次の例は、どのように*ViewStateMode*プロパティはします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-374">The following example shows how the *ViewStateMode* property works.</span></span> <span data-ttu-id="c6c2f-375">マークアップと、ページのコントロールを次のコードの値が含まれています、 *ViewStateMode*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-375">The markup and code for the controls in the following page includes values for the *ViewStateMode* property:</span></span>

[!code-aspx[Main](overview/samples/sample25.aspx)]

<span data-ttu-id="c6c2f-376">ご覧のように、コードには、PlaceHolder1 コントロールのビュー ステートが無効にします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-376">As you can see, the code disables view state for the PlaceHolder1 control.</span></span> <span data-ttu-id="c6c2f-377">子 label1 コントロールは、このプロパティの値を継承 (*継承*の既定値は、 *ViewStateMode*コントロールの) しビュー ステートしたがって保存されません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-377">The child label1 control inherits this property value (*Inherit* is the default value for *ViewStateMode* for controls.) and therefore saves no view state.</span></span> <span data-ttu-id="c6c2f-378">PlaceHolder2 コントロールで、 *ViewStateMode*に設定されている*有効*label2 は、このプロパティを継承し、保存状態を表示するため、します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-378">In the PlaceHolder2 control, *ViewStateMode* is set to *Enabled*, so label2 inherits this property and saves view state.</span></span> <span data-ttu-id="c6c2f-379">ページが最初に読み込まれたときに、*テキスト*両方のプロパティ*ラベル*コントロールが、文字列"[DynamicValue]"に設定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-379">When the page is first loaded, the *Text* property of both *Label* controls is set to the string "[DynamicValue]".</span></span>

<span data-ttu-id="c6c2f-380">これらの設定の効果は、ページが初めて読み込まれるときに、次の出力がブラウザーで表示されることです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-380">The effect of these settings is that when the page loads the first time, the following output is displayed in the browser:</span></span>

<span data-ttu-id="c6c2f-381">無効になっています。 `: [DynamicValue]`</span><span class="sxs-lookup"><span data-stu-id="c6c2f-381">Disabled `: [DynamicValue]`</span></span>

<span data-ttu-id="c6c2f-382">有効になります。`[DynamicValue]`</span><span class="sxs-lookup"><span data-stu-id="c6c2f-382">Enabled:`[DynamicValue]`</span></span>

<span data-ttu-id="c6c2f-383">ポストバックされた後に、次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-383">After a postback, however, the following output is displayed:</span></span>

<span data-ttu-id="c6c2f-384">無効になっています。 `: [DeclaredValue]`</span><span class="sxs-lookup"><span data-stu-id="c6c2f-384">Disabled `: [DeclaredValue]`</span></span>

<span data-ttu-id="c6c2f-385">有効になります。`[DynamicValue]`</span><span class="sxs-lookup"><span data-stu-id="c6c2f-385">Enabled:`[DynamicValue]`</span></span>

<span data-ttu-id="c6c2f-386">Label1 コントロール (が*ViewStateMode*に値が設定されている*無効になっている*) がコード内設定されている値を保持していません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-386">The label1 control (whose *ViewStateMode* value is set to *Disabled*) has not preserved the value that it was set to in code.</span></span> <span data-ttu-id="c6c2f-387">ただし、label2 制御 (が*ViewStateMode*に値が設定されている*有効*) の状態を保持しています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-387">However, the label2 control (whose *ViewStateMode* value is set to *Enabled*) has preserved its state.</span></span>

<span data-ttu-id="c6c2f-388">設定することも*ViewStateMode*で、 *@ Page*ディレクティブを次の例のように。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-388">You can also set *ViewStateMode* in the *@ Page* directive, as in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample26.aspx)]

<span data-ttu-id="c6c2f-389">*ページ*クラスは、単なる別のコントロール以外の場合は、ページの他のすべてのコントロールの親コントロールとして機能します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-389">The *Page* class is just another control; it acts as the parent control for all the other controls in the page.</span></span> <span data-ttu-id="c6c2f-390">既定値*ViewStateMode*は*有効*のインスタンスの*ページ*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-390">The default value of *ViewStateMode* is *Enabled* for instances of *Page*.</span></span> <span data-ttu-id="c6c2f-391">コントロールが既定になるため*継承*、コントロールが継承、*有効*プロパティ値を設定しない限り*ViewStateMode*ページまたはコントロール レベル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-391">Because controls default to *Inherit*, controls will inherit the *Enabled* property value unless you set *ViewStateMode* at page or control level.</span></span>

<span data-ttu-id="c6c2f-392">値、 *ViewStateMode*プロパティは、ビュー状態を維持する場合にのみかどうかを決定、*エントリ*プロパティに設定されている*true*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-392">The value of the *ViewStateMode* property determines if view state is maintained only if the *EnableViewState* property is set to *true*.</span></span> <span data-ttu-id="c6c2f-393">場合、*エントリ*プロパティに設定されている*false*、ビュー状態が維持されない場合でも*ViewStateMode*に設定されている*有効*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-393">If the *EnableViewState* property is set to *false*, view state will not be maintained even if *ViewStateMode* is set to *Enabled*.</span></span>

<span data-ttu-id="c6c2f-394">この機能の良い使用*ContentPlaceHolder*マスター ページで、設定できるコントロール*ViewStateMode*に*無効になっている*マスター ページし、有効に個別に*ContentPlaceHolder*順番が必要なコントロールを格納しているコントロールの状態を表示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-394">A good use for this feature is with *ContentPlaceHolder* controls in master pages, where you can set *ViewStateMode* to *Disabled* for the master page and then enable it individually for *ContentPlaceHolder* controls that in turn contain controls that require view state.</span></span>

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a><span data-ttu-id="c6c2f-395">ブラウザーの機能への変更</span><span class="sxs-lookup"><span data-stu-id="c6c2f-395">Changes to Browser Capabilities</span></span>

<span data-ttu-id="c6c2f-396">ASP.NET と呼ばれる機能を使用してサイトを閲覧するユーザーを使用しているブラウザーの機能を決定する*ブラウザーの性能*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-396">ASP.NET determines the capabilities of the browser that a user is using to browse your site by using a feature called *browser capabilities*.</span></span> <span data-ttu-id="c6c2f-397">ブラウザーの機能がによって表される、 *HttpBrowserCapabilities*オブジェクト (によって公開されている、 *Request.Browser*プロパティ)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-397">Browser capabilities are represented by the *HttpBrowserCapabilities* object (exposed by the *Request.Browser* property).</span></span> <span data-ttu-id="c6c2f-398">たとえば、使用することができます、 *HttpBrowserCapabilities*オブジェクト、型と、現在のブラウザーのバージョンが JavaScript の特定のバージョンをサポートするかどうかを判別します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-398">For example, you can use the *HttpBrowserCapabilities* object to determine whether the type and version of the current browser supports a particular version of JavaScript.</span></span> <span data-ttu-id="c6c2f-399">または、使用することができます、 *HttpBrowserCapabilities*モバイル デバイスから、要求が発生したかどうかを決定するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-399">Or, you can use the *HttpBrowserCapabilities* object to determine whether the request originated from a mobile device.</span></span>

<span data-ttu-id="c6c2f-400">*HttpBrowserCapabilities*オブジェクトは、一連のブラウザー定義ファイルによって決まります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-400">The *HttpBrowserCapabilities* object is driven by a set of browser definition files.</span></span> <span data-ttu-id="c6c2f-401">これらのファイルには、特定のブラウザーの機能に関する情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-401">These files contain information about the capabilities of particular browsers.</span></span> <span data-ttu-id="c6c2f-402">ASP.NET 4 に最近導入されたブラウザーと Google Chrome、研究などモーション BlackBerry スマート フォンと Apple の iPhone でのデバイスに関する情報にこれらのブラウザー定義ファイルが更新されました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-402">In ASP.NET 4, these browser definition files have been updated to contain information about recently introduced browsers and devices such as Google Chrome, Research in Motion BlackBerry smartphones, and Apple iPhone.</span></span>

<span data-ttu-id="c6c2f-403">次の一覧は、新しいブラウザー定義ファイルを示しています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-403">The following list shows new browser definition files:</span></span>

- <span data-ttu-id="c6c2f-404">*blackberry.browser*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-404">*blackberry.browser*</span></span>
- <span data-ttu-id="c6c2f-405">*chrome.browser*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-405">*chrome.browser*</span></span>
- <span data-ttu-id="c6c2f-406">*Default.browser*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-406">*Default.browser*</span></span>
- <span data-ttu-id="c6c2f-407">*firefox.browser*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-407">*firefox.browser*</span></span>
- <span data-ttu-id="c6c2f-408">*gateway.browser*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-408">*gateway.browser*</span></span>
- <span data-ttu-id="c6c2f-409">*generic.browser*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-409">*generic.browser*</span></span>
- <span data-ttu-id="c6c2f-410">*ie.browser*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-410">*ie.browser*</span></span>
- <span data-ttu-id="c6c2f-411">*iemobile.browser*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-411">*iemobile.browser*</span></span>
- <span data-ttu-id="c6c2f-412">*iphone.browser*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-412">*iphone.browser*</span></span>
- <span data-ttu-id="c6c2f-413">*opera.browser*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-413">*opera.browser*</span></span>
- <span data-ttu-id="c6c2f-414">*safari.browser*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-414">*safari.browser*</span></span>

#### <a name="using-browser-capabilities-providers"></a><span data-ttu-id="c6c2f-415">ブラウザー機能のプロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-415">Using Browser Capabilities Providers</span></span>

<span data-ttu-id="c6c2f-416">Asp.net version 3.5 Service Pack 1 を次のように、ブラウザーが持つ機能を定義することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-416">In ASP.NET version 3.5 Service Pack 1, you can define the capabilities that a browser has in the following ways:</span></span>

- <span data-ttu-id="c6c2f-417">コンピューター レベルで作成または更新、`.browser`次のフォルダーに XML ファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-417">At the computer level, you create or update a `.browser` XML file in the following folder:</span></span>

- [!code-console[Main](overview/samples/sample27.cmd)]

- <span data-ttu-id="c6c2f-418">ブラウザーの機能を定義した後、次のコマンドに、Visual Studio コマンド プロンプトからブラウザー機能アセンブリを再構築して GAC に追加するためを実行します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-418">After you define the browser capability, you run the following command from the Visual Studio Command Prompt in order to rebuild the browser capabilities assembly and add it to the GAC:</span></span>

- [!code-console[Main](overview/samples/sample28.cmd)]

- <span data-ttu-id="c6c2f-419">個々 のアプリケーションを作成する、`.browser`アプリケーションのファイル`App_Browsers`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-419">For an individual application, you create a `.browser` file in the application's `App_Browsers` folder.</span></span>

<span data-ttu-id="c6c2f-420">これらのアプローチでは、XML ファイルを変更する必要があり、コンピューター レベルの変更の aspnet を実行した後、アプリケーションを再起動する必要があります\_regbrowsers.exe プロセスです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-420">These approaches require you to change XML files, and for computer-level changes, you must restart the application after you run the aspnet\_regbrowsers.exe process.</span></span>

<span data-ttu-id="c6c2f-421">ASP.NET 4 と呼ばれる機能が含まれています。*ブラウザー機能プロバイダー*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-421">ASP.NET 4 includes a feature referred to as *browser capabilities providers*.</span></span> <span data-ttu-id="c6c2f-422">名前からわかるように、これにより、さらに使用できるプロバイダーをビルドするは、ブラウザーの機能を決定するのに、独自のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-422">As the name suggests, this lets you build a provider that in turn lets you use your own code to determine browser capabilities.</span></span>

<span data-ttu-id="c6c2f-423">実際には、開発者頻度を定義しないでカスタム ブラウザーの機能です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-423">In practice, developers often do not define custom browser capabilities.</span></span> <span data-ttu-id="c6c2f-424">ブラウザー ファイルは、更新の場合は、更新することは非常に複雑なプロセスをハードおよびの XML 構文`.browser`ファイルを使用してを定義する複雑になることができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-424">Browser files are hard to update, the process for updating them is fairly complicated, and the XML syntax for `.browser` files can be complex to use and define.</span></span> <span data-ttu-id="c6c2f-425">新機能は、このプロセスをより簡潔とは、一般的なブラウザーの定義の構文があった場合または最新のブラウザーの定義、またはデータベースなどの Web サービスも含まれているデータベース。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-425">What would make this process much easier is if there were a common browser definition syntax, or a database that contained up-to-date browser definitions, or even a Web service for such a database.</span></span> <span data-ttu-id="c6c2f-426">新しいブラウザーの機能のプロバイダーの機能により、これらのシナリオ可能で実用的なサード パーティの開発者向けです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-426">The new browser capabilities providers feature makes these scenarios possible and practical for third-party developers.</span></span>

<span data-ttu-id="c6c2f-427">ASP.NET 4 ブラウザー機能プロバイダーの新しい機能を使用する 2 つの主な方法: ASP.NET ブラウザーの機能の定義機能を拡張するか、完全に置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-427">There are two main approaches for using the new ASP.NET 4 browser capabilities provider feature: extending the ASP.NET browser capabilities definition functionality, or totally replacing it.</span></span> <span data-ttu-id="c6c2f-428">次のセクションでは、最初と方法について説明、機能を置き換えるし、それを拡張する方法です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-428">The following sections describe first how to replace the functionality, and then how to extend it.</span></span>

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a><span data-ttu-id="c6c2f-429">ASP.NET ブラウザー機能の機能を置き換える</span><span class="sxs-lookup"><span data-stu-id="c6c2f-429">Replacing the ASP.NET Browser Capabilities Functionality</span></span>

<span data-ttu-id="c6c2f-430">ASP.NET ブラウザー機能の定義機能を完全に置き換えるには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-430">To replace the ASP.NET browser capabilities definition functionality completely, follow these steps:</span></span>

1. <span data-ttu-id="c6c2f-431">派生するプロバイダー クラスを作成する*HttpCapabilitiesProvider*をオーバーライドして、 *GetBrowserCapabilities*次の例のように、メソッド。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-431">Create a provider class that derives from *HttpCapabilitiesProvider* and that overrides the *GetBrowserCapabilities* method, as in the following example:</span></span> 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    <span data-ttu-id="c6c2f-432">この例では、コードが新たに作成*HttpBrowserCapabilities*オブジェクト、MyCustomBrowser にその機能を設定してブラウザーをという名前の機能のみを指定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-432">The code in this example creates a new *HttpBrowserCapabilities* object, specifying only the capability named browser and setting that capability to MyCustomBrowser.</span></span>
2. <span data-ttu-id="c6c2f-433">プロバイダーは、アプリケーションを登録します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-433">Register the provider with the application.</span></span> 

    <span data-ttu-id="c6c2f-434">追加する必要があります、プロバイダーをアプリケーションで使用するために、*プロバイダー*属性を*browserCaps* 」の「、`Web.config`または`Machine.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-434">In order to use a provider with an application, you must add the *provider* attribute to the *browserCaps* section in the `Web.config` or `Machine.config` files.</span></span> <span data-ttu-id="c6c2f-435">(プロバイダーの属性を定義することも、*場所*アプリケーションでは、特定のモバイル デバイス用のフォルダーなどの特定のディレクトリの要素です)。次の例は、設定する方法を示します、*プロバイダー*構成ファイル内の属性。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-435">(You can also define the provider attributes in a *location* element for specific directories in application, such as in a folder for a specific mobile device.) The following example shows how to set the *provider* attribute in a configuration file:</span></span>

    [!code-xml[Main](overview/samples/sample30.xml)]

    <span data-ttu-id="c6c2f-436">新しいブラウザーの機能の定義を登録する別の方法は、次の例で示すようにコードを使用するのには。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-436">Another way to register the new browser capability definition is to use code, as shown in the following example:</span></span>

    [!code-csharp[Main](overview/samples/sample31.cs)]

    <span data-ttu-id="c6c2f-437">このコードを実行する必要があります、*アプリケーション\_開始*のイベント、`Global.asax`ファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-437">This code must run in the *Application\_Start* event of the `Global.asax` file.</span></span> <span data-ttu-id="c6c2f-438">変更を加えた、 *BrowserCapabilitiesProvider*クラスは、キャッシュが、解決済みの有効な状態に残っているかどうかを確認するために、アプリケーションの任意のコードを実行する前に行う必要があります*HttpCapabilitiesBase*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-438">Any change to the *BrowserCapabilitiesProvider* class must occur before any code in the application executes, in order to make sure that the cache remains in a valid state for the resolved *HttpCapabilitiesBase* object.</span></span>

#### <a name="caching-the-httpbrowsercapabilities-object"></a><span data-ttu-id="c6c2f-439">HttpBrowserCapabilities オブジェクトのキャッシュ</span><span class="sxs-lookup"><span data-stu-id="c6c2f-439">Caching the HttpBrowserCapabilities Object</span></span>

<span data-ttu-id="c6c2f-440">前の例は、コードは、カスタム プロバイダーを取得するために呼び出されるたびに実行ことである 1 つの問題、 *HttpBrowserCapabilities*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-440">The preceding example has one problem, which is that the code would run each time the custom provider is invoked in order to get the *HttpBrowserCapabilities* object.</span></span> <span data-ttu-id="c6c2f-441">これでは、複数回を各要求中に発生します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-441">This can happen multiple times during each request.</span></span> <span data-ttu-id="c6c2f-442">例では、プロバイダーのコードはほど多くありません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-442">In the example, the code for the provider does not do much.</span></span> <span data-ttu-id="c6c2f-443">ただし、カスタム プロバイダーでコードの重要な作業を実行する場合を取得するため、 *HttpBrowserCapabilities*オブジェクト、これはパフォーマンスに影響することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-443">However, if the code in your custom provider performs significant work in order to get the *HttpBrowserCapabilities* object, this can affect performance.</span></span> <span data-ttu-id="c6c2f-444">これを防ぐをキャッシュできます、 *HttpBrowserCapabilities*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-444">To prevent this from happening, you can cache the *HttpBrowserCapabilities* object.</span></span> <span data-ttu-id="c6c2f-445">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-445">Follow these steps:</span></span>

1. <span data-ttu-id="c6c2f-446">派生するクラスを作成する*HttpCapabilitiesProvider*などの次の例 1。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-446">Create a class that derives from *HttpCapabilitiesProvider*, like the one in the following example:</span></span> 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    <span data-ttu-id="c6c2f-447">例では、コードはカスタム BuildCacheKey メソッドを呼び出すことによって、キャッシュ キーを生成し、カスタム GetCacheTime メソッドを呼び出して、キャッシュする時間の長さを取得します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-447">In the example, the code generates a cache key by calling a custom BuildCacheKey method, and it gets the length of time to cache by calling a custom GetCacheTime method.</span></span> <span data-ttu-id="c6c2f-448">次のコードは解決された追加*HttpBrowserCapabilities*キャッシュするオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-448">The code then adds the resolved *HttpBrowserCapabilities* object to the cache.</span></span> <span data-ttu-id="c6c2f-449">オブジェクトをキャッシュから取得しで再利用する後続の要求をカスタム プロバイダーの使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-449">The object can be retrieved from the cache and reused on subsequent requests that make use of the custom provider.</span></span>
2. <span data-ttu-id="c6c2f-450">前の手順で説明されているアプリケーションで、プロバイダーを登録します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-450">Register the provider with the application as described in the preceding procedure.</span></span>

#### <a name="extending-aspnet-browser-capabilities-functionality"></a><span data-ttu-id="c6c2f-451">ASP.NET ブラウザー機能の機能を拡張します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-451">Extending ASP.NET Browser Capabilities Functionality</span></span>

<span data-ttu-id="c6c2f-452">前のセクションには、新しいを作成する方法が説明されている*HttpBrowserCapabilities* ASP.NET 4 のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-452">The previous section described how to create a new *HttpBrowserCapabilities* object in ASP.NET 4.</span></span> <span data-ttu-id="c6c2f-453">ASP.NET に既に含まれている新しいブラウザーの機能の定義を追加することで ASP.NET ブラウザー機能機能を拡張することもできます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-453">You can also extend the ASP.NET browser capabilities functionality by adding new browser capabilities definitions to those that are already in ASP.NET.</span></span> <span data-ttu-id="c6c2f-454">XML のブラウザーの定義を使用せず、これを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-454">You can do this without using the XML browser definitions.</span></span> <span data-ttu-id="c6c2f-455">次の手順に示す方法。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-455">The following procedure shows how.</span></span>

1. <span data-ttu-id="c6c2f-456">派生するクラスを作成する*HttpCapabilitiesEvaluator*をオーバーライドして、 *GetBrowserCapabilities*メソッドを次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-456">Create a class that derives from *HttpCapabilitiesEvaluator* and that overrides the *GetBrowserCapabilities* method, as shown in the following example:</span></span> 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    <span data-ttu-id="c6c2f-457">このコードでは、まず、ASP.NET ブラウザー機能の機能を使用してブラウザーを識別しようとしています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-457">This code first uses the ASP.NET browser capabilities functionality to try to identify the browser.</span></span> <span data-ttu-id="c6c2f-458">ただし、ブラウザーが指定されていない場合、情報に基づいて、要求で定義されている (つまり、*ブラウザー*のプロパティ、 *HttpBrowserCapabilities*オブジェクトは、文字列"Unknown")、コードの呼び出しカスタムのプロバイダー (MyBrowserCapabilitiesEvaluator) をブラウザーを識別します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-458">However, if no browser is identified based on the information defined in the request (that is, if the *Browser* property of the *HttpBrowserCapabilities* object is the string "Unknown"), the code calls the custom provider (MyBrowserCapabilitiesEvaluator) to identify the browser.</span></span>
2. <span data-ttu-id="c6c2f-459">前の例に示すように、プロバイダーをアプリケーションを登録します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-459">Register the provider with the application as described in the previous example.</span></span>

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a><span data-ttu-id="c6c2f-460">ブラウザー機能の機能を拡張する既存の機能の定義に新機能の追加</span><span class="sxs-lookup"><span data-stu-id="c6c2f-460">Extending Browser Capabilities Functionality by Adding New Capabilities to Existing Capabilities Definitions</span></span>

<span data-ttu-id="c6c2f-461">カスタムのブラウザー定義プロバイダーを作成するだけでなく、新しいブラウザーの定義を動的に作成するのには、追加の機能の既存のブラウザー定義を拡張できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-461">In addition to creating a custom browser definition provider and to dynamically creating new browser definitions, you can extend existing browser definitions with additional capabilities.</span></span> <span data-ttu-id="c6c2f-462">これにより、定義対象の近くには、いくつかの機能のみがないを使用できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-462">This lets you use a definition that is close to what you want but lacks only a few capabilities.</span></span> <span data-ttu-id="c6c2f-463">そのためには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-463">To do this, use the following steps.</span></span>

1. <span data-ttu-id="c6c2f-464">派生するクラスを作成する*HttpCapabilitiesEvaluator*をオーバーライドして、 *GetBrowserCapabilities*メソッドを次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-464">Create a class that derives from *HttpCapabilitiesEvaluator* and that overrides the *GetBrowserCapabilities* method, as shown in the following example:</span></span> 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    <span data-ttu-id="c6c2f-465">コード例は、既存の ASP.NET を拡張*HttpCapabilitiesEvaluator*クラスを取得するため、 *HttpBrowserCapabilities*次のコードを使用して現在の要求の定義と一致するオブジェクト:</span><span class="sxs-lookup"><span data-stu-id="c6c2f-465">The example code extends the existing ASP.NET *HttpCapabilitiesEvaluator* class and gets the *HttpBrowserCapabilities* object that matches the current request definition by using the following code:</span></span>

    [!code-csharp[Main](overview/samples/sample35.cs)]

    <span data-ttu-id="c6c2f-466">コードでは追加し、またはこのブラウザーの機能を変更できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-466">The code can then add or modify a capability for this browser.</span></span> <span data-ttu-id="c6c2f-467">これにはブラウザーの新機能を指定する 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-467">There are two ways to specify a new browser capability:</span></span>

    - <span data-ttu-id="c6c2f-468">キー/値ペアを追加、 *IDictionary*によって公開されるオブジェクト、*機能*のプロパティ、 *HttpCapabilitiesBase*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-468">Add a key/value pair to the *IDictionary* object that is exposed by the *Capabilities* property of the *HttpCapabilitiesBase* object.</span></span> <span data-ttu-id="c6c2f-469">前の例では、コードは、機能が追加されて、マルチタッチをという名前の値を持つ*true*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-469">In the previous example, the code adds a capability named MultiTouch with a value of *true*.</span></span>
    - <span data-ttu-id="c6c2f-470">既存のプロパティの設定、 *HttpCapabilitiesBase*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-470">Set existing properties of the *HttpCapabilitiesBase* object.</span></span> <span data-ttu-id="c6c2f-471">前の例では、コードを設定、*フレーム*プロパティを*true*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-471">In the previous example, the code sets the *Frames* property to *true*.</span></span> <span data-ttu-id="c6c2f-472">アクセサーを単に、このプロパティは、 *IDictionary*によって公開されるオブジェクト、*機能*プロパティです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-472">This property is simply an accessor for the *IDictionary* object that is exposed by the *Capabilities* property.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="c6c2f-473">このモデルは、任意のプロパティに適用されます*HttpBrowserCapabilities*、コントロール アダプターを含むです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-473">Note This model applies to any property of *HttpBrowserCapabilities*, including control adapters.</span></span>
2. <span data-ttu-id="c6c2f-474">前の手順で説明されているアプリケーションで、プロバイダーを登録します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-474">Register the provider with the application as described in the earlier procedure.</span></span>

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a><span data-ttu-id="c6c2f-475">ASP.NET 4 でのルーティング</span><span class="sxs-lookup"><span data-stu-id="c6c2f-475">Routing in ASP.NET 4</span></span>

<span data-ttu-id="c6c2f-476">ASP.NET 4 Web フォームでルーティングを使用するための組み込みサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-476">ASP.NET 4 adds built-in support for using routing with Web Forms.</span></span> <span data-ttu-id="c6c2f-477">要求の物理ファイルにマップされていない Url のルーティングは、そのまま使用するアプリケーションを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-477">Routing lets you configure an application to accept request URLs that do not map to physical files.</span></span> <span data-ttu-id="c6c2f-478">代わりに、ルーティングを使用する、ユーザーにわかりやすいとが、アプリケーションの検索エンジン最適化 (SEO) を使用して Url を定義することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-478">Instead, you can use routing to define URLs that are meaningful to users and that can help with search-engine optimization (SEO) for your application.</span></span> <span data-ttu-id="c6c2f-479">たとえば、既存のアプリケーションの製品カテゴリを表示するページの URL は、次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-479">For example, the URL for a page that displays product categories in an existing application might look like the following example:</span></span>

[!code-console[Main](overview/samples/sample36.cmd)]

<span data-ttu-id="c6c2f-480">ルーティングを使用するには、同じ情報を表示するために、次の URL を受け入れるようにアプリケーションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-480">By using routing, you can configure the application to accept the following URL to render the same information:</span></span>

[!code-console[Main](overview/samples/sample37.cmd)]

<span data-ttu-id="c6c2f-481">ルーティングは ASP.NET 3.5 SP1 以降で使用されました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-481">Routing has been available starting with ASP.NET 3.5 SP1.</span></span> <span data-ttu-id="c6c2f-482">(ASP.NET 3.5 SP1 でルーティングを使用する方法の例は、エントリを参照してください。 [WebForms とルーティングを使用して](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "このエントリのタイトル。")</span><span class="sxs-lookup"><span data-stu-id="c6c2f-482">(For an example of how to use routing in ASP.NET 3.5 SP1, see the entry [Using Routing With WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "Title of this entry.")</span></span> <span data-ttu-id="c6c2f-483">Phil Haack のブログです。)ただし、ASP.NET 4 が含まれています、ルーティングを使用するより簡単にするいくつかの機能には、次を含みます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-483">on Phil Haack's blog.) However, ASP.NET 4 includes some features that make it easier to use routing, including the following:</span></span>

- <span data-ttu-id="c6c2f-484">*PageRouteHandler*クラスは、ルートを定義するときに使用する単純な HTTP ハンドラーします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-484">The *PageRouteHandler* class, which is a simple HTTP handler that you use when you define routes.</span></span> <span data-ttu-id="c6c2f-485">クラスに要求をルーティングするページへのデータを渡します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-485">The class passes data to the page that the request is routed to.</span></span>
- <span data-ttu-id="c6c2f-486">新しいプロパティ*HttpRequest.RequestContext*と*Page.RouteData* (これは、プロキシを*HttpRequest.RequestContext.RouteData*オブジェクト)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-486">The new properties *HttpRequest.RequestContext* and *Page.RouteData* (which is a proxy for the *HttpRequest.RequestContext.RouteData* object).</span></span> <span data-ttu-id="c6c2f-487">これらのプロパティをやすくをルートから渡される情報にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-487">These properties make it easier to access information that is passed from the route.</span></span>
- <span data-ttu-id="c6c2f-488">次の新しい式ビルダーで定義されている*System.Web.Compilation.RouteUrlExpressionBuilder*と*System.Web.Compilation.RouteValueExpressionBuilder*:</span><span class="sxs-lookup"><span data-stu-id="c6c2f-488">The following new expression builders, which are defined in *System.Web.Compilation.RouteUrlExpressionBuilder* and *System.Web.Compilation.RouteValueExpressionBuilder*:</span></span>
- <span data-ttu-id="c6c2f-489">*RouteUrl*、ASP.NET サーバー コントロール内のルート URL に対応する URL を作成する簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-489">*RouteUrl*, which provides a simple way to create a URL that corresponds to a route URL within an ASP.NET server control.</span></span>
- <span data-ttu-id="c6c2f-490">*RouteValue*からの情報を抽出する簡単な方法を提供する、 *RouteContext*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-490">*RouteValue*, which provides a simple way to extract information from the *RouteContext* object.</span></span>
- <span data-ttu-id="c6c2f-491">*RouteParameter*を簡単に含まれるデータを渡すにクラス、 *RouteContext*データ ソース コントロールに対するクエリへのオブジェクト (に似て[ *FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).</span><span class="sxs-lookup"><span data-stu-id="c6c2f-491">The *RouteParameter* class, which makes it easier to pass data contained in a *RouteContext* object to a query for a data source control (similar to [*FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).</span></span>

#### <a name="routing-for-web-forms-pages"></a><span data-ttu-id="c6c2f-492">Web フォーム ページのルーティング</span><span class="sxs-lookup"><span data-stu-id="c6c2f-492">Routing for Web Forms Pages</span></span>

<span data-ttu-id="c6c2f-493">次の例は、新しいを使用して Web フォームのルートを定義する方法を示します*MapPageRoute*のメソッド、*ルート*クラス。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-493">The following example shows how to define a Web Forms route by using the new *MapPageRoute* method of the *Route* class:</span></span>

[!code-csharp[Main](overview/samples/sample38.cs)]

<span data-ttu-id="c6c2f-494">ASP.NET 4 では、 *MapPageRoute*メソッドです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-494">ASP.NET 4 introduces the *MapPageRoute* method.</span></span> <span data-ttu-id="c6c2f-495">次の例は、前の例で示す SearchRoute 定義と同じですを使用して、 *PageRouteHandler*クラスです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-495">The following example is equivalent to the SearchRoute definition shown in the previous example, but uses the *PageRouteHandler* class.</span></span>

[!code-csharp[Main](overview/samples/sample39.cs)]

<span data-ttu-id="c6c2f-496">コードの例では、物理ページにルートをマップ (最初のルート上でに`~/search.aspx`)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-496">The code in the example maps the route to a physical page (in the first route, to `~/search.aspx`).</span></span> <span data-ttu-id="c6c2f-497">ルートの最初の定義は、searchterm をという名前のパラメーターを URL から抽出され、ページに渡されることにも指定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-497">The first route definition also specifies that the parameter named searchterm should be extracted from the URL and passed to the page.</span></span>

<span data-ttu-id="c6c2f-498">*MapPageRoute*メソッドは、次のメソッドのオーバー ロードをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-498">The *MapPageRoute* method supports the following method overloads:</span></span>

- <span data-ttu-id="c6c2f-499">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess)*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-499">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess)*</span></span>
- <span data-ttu-id="c6c2f-500">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults)*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-500">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults)*</span></span>
- <span data-ttu-id="c6c2f-501">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults, RouteValueDictionary constraints)*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-501">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults, RouteValueDictionary constraints)*</span></span>

<span data-ttu-id="c6c2f-502">*CheckPhysicalUrlAccess*パラメーターは、ルートにルーティングされている物理ページの セキュリティ アクセス許可を確認するかどうかを指定します (この場合は、完成しました) と、着信 URL のアクセス許可 (この場合、検索/{searchterm})。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-502">The *checkPhysicalUrlAccess* parameter specifies whether the route should check the security permissions for the physical page being routed to (in this case, search.aspx) and the permissions on the incoming URL (in this case, search/{searchterm}).</span></span> <span data-ttu-id="c6c2f-503">場合の値*checkPhysicalUrlAccess*は*false*、着信 URL のアクセス許可だけがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-503">If the value of *checkPhysicalUrlAccess* is *false*, only the permissions of the incoming URL will be checked.</span></span> <span data-ttu-id="c6c2f-504">これらのアクセス許可が定義されている、`Web.config`ファイルの次のように設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-504">These permissions are defined in the `Web.config` file using settings such as the following:</span></span>

[!code-xml[Main](overview/samples/sample40.xml)]

<span data-ttu-id="c6c2f-505">例の構成へのアクセスが拒否物理ページ`search.aspx`管理者の役割であるものを除くすべてのユーザーです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-505">In the example configuration, access is denied to the physical page `search.aspx` for all users except those who are in the admin role.</span></span> <span data-ttu-id="c6c2f-506">ときに、 *checkPhysicalUrlAccess*にパラメーターが設定されている*true* (がその既定値)、物理ページ完成しましたので、URL/search/{searchterm} にアクセスする管理ユーザーのみが許可されてそのロールのユーザーに制限されています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-506">When the *checkPhysicalUrlAccess* parameter is set to *true* (which is its default value), only admin users are allowed to access the URL /search/{searchterm}, because the physical page search.aspx is restricted to users in that role.</span></span> <span data-ttu-id="c6c2f-507">場合*checkPhysicalUrlAccess*に設定されている*false*と前の例で示すように、サイトが構成されている、すべての認証ユーザーが URL/search/{searchterm} のアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-507">If *checkPhysicalUrlAccess* is set to *false* and the site is configured as shown in the previous example, all authenticated users are allowed to access the URL /search/{searchterm}.</span></span>

#### <a name="reading-routing-information-in-a-web-forms-page"></a><span data-ttu-id="c6c2f-508">ルーティング情報を Web フォーム ページの読み取り</span><span class="sxs-lookup"><span data-stu-id="c6c2f-508">Reading Routing Information in a Web Forms Page</span></span>

<span data-ttu-id="c6c2f-509">Web フォームの物理ページのコードでのルーティングが URL から抽出される情報にアクセスすることができます (または別のオブジェクトに追加したその他の情報、 *RouteData*オブジェクト) 2 つの新しいプロパティを使用して: *HttpRequest.RequestContext*と*Page.RouteData*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-509">In the code of the Web Forms physical page, you can access the information that routing has extracted from the URL (or other information that another object has added to the *RouteData* object) by using two new properties: *HttpRequest.RequestContext* and *Page.RouteData*.</span></span> <span data-ttu-id="c6c2f-510">(*Page.RouteData*ラップ*HttpRequest.RequestContext.RouteData*)。次の例は、使用する方法を示しています。 *Page.RouteData*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-510">(*Page.RouteData* wraps *HttpRequest.RequestContext.RouteData*.) The following example shows how to use *Page.RouteData*.</span></span>

[!code-csharp[Main](overview/samples/sample41.cs)]

<span data-ttu-id="c6c2f-511">コードでは、前の例のルートで定義されている searchterm パラメーターに渡された値を抽出します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-511">The code extracts the value that was passed for the searchterm parameter, as defined in the example route earlier.</span></span> <span data-ttu-id="c6c2f-512">次の要求の URL を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-512">Consider the following request URL:</span></span>

[!code-console[Main](overview/samples/sample42.cmd)]

<span data-ttu-id="c6c2f-513">この要求が行われる場合、"scott"はでレンダリングされ、単語、`search.aspx`ページ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-513">When this request is made, the word "scott" would be rendered in the `search.aspx` page.</span></span>

#### <a name="accessing-routing-information-in-markup"></a><span data-ttu-id="c6c2f-514">マークアップでルーティング情報にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-514">Accessing Routing Information in Markup</span></span>

<span data-ttu-id="c6c2f-515">前のセクションで説明されているメソッドは、Web フォーム ページ内のコードのルート データを取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-515">The method described in the previous section shows how to get route data in code in a Web Forms page.</span></span> <span data-ttu-id="c6c2f-516">また、同じ情報にアクセスできるようにマークアップ内の式を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-516">You can also use expressions in markup that give you access to the same information.</span></span> <span data-ttu-id="c6c2f-517">式ビルダーは、宣言型コードを操作する強力で簡潔な方法です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-517">Expression builders are a powerful and elegant way to work with declarative code.</span></span> <span data-ttu-id="c6c2f-518">(詳細については、エントリを参照してください[Express 自分でカスタム式ビルダー](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) Phil Haack のブログです。)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-518">(For more information, see the entry [Express Yourself With Custom Expression Builders](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) on Phil Haack's blog.)</span></span>

<span data-ttu-id="c6c2f-519">ASP.NET 4 Web フォームのルーティング用の 2 つの新しい式ビルダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-519">ASP.NET 4 includes two new expression builders for Web Forms routing.</span></span> <span data-ttu-id="c6c2f-520">次の例では、その使用方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-520">The following example shows how to use them.</span></span>

[!code-aspx[Main](overview/samples/sample43.aspx)]

<span data-ttu-id="c6c2f-521">この例で、 *RouteUrl*ルートのパラメーターに基づいている URL を定義する式を使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-521">In the example, the *RouteUrl* expression is used to define a URL that is based on a route parameter.</span></span> <span data-ttu-id="c6c2f-522">これは、マークアップに完全な URL をハードコーディングしなくて済むためであり、このリンクを変更することがなく、URL の構造を後で変更することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-522">This saves you from having to hard-code the complete URL into the markup, and lets you change the URL structure later without requiring any change to this link.</span></span>

<span data-ttu-id="c6c2f-523">以前に定義されたルートに基づき、このマークアップには、次の URL が生成されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-523">Based on the route defined earlier, this markup generates the following URL:</span></span>

[!code-console[Main](overview/samples/sample44.cmd)]

<span data-ttu-id="c6c2f-524">ASP.NET は、正しいルートを自動的に機能 (正しい URL を生成、)、入力パラメーターに基づきます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-524">ASP.NET automatically works out the correct route (that is, it generates the correct URL) based on the input parameters.</span></span> <span data-ttu-id="c6c2f-525">式では、使用するルートを指定するため、ルート名を含めることもできます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-525">You can also include a route name in the expression, which lets you specify a route to use.</span></span>

<span data-ttu-id="c6c2f-526">次の例を使用する方法を示しています、 *RouteValue*式。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-526">The following example shows how to use the *RouteValue* expression.</span></span>

[!code-aspx[Main](overview/samples/sample45.aspx)]

<span data-ttu-id="c6c2f-527">このコントロールを含むページが実行されると、"scott"の値がラベルに表示されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-527">When the page that contains this control runs, the value "scott" is displayed in the label.</span></span>

<span data-ttu-id="c6c2f-528">*RouteValue*式を簡単にマークアップでは、ルート データを使用しより複雑な Page.RouteData["x を操作することを回避できます"] マークアップの構文。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-528">The *RouteValue* expression makes it simple to use route data in markup, and it avoids having to work with the more complex Page.RouteData["x"] syntax in markup.</span></span>

#### <a name="using-route-data-for-data-source-control-parameters"></a><span data-ttu-id="c6c2f-529">ルート データ データ ソース コントロール パラメーターの使用</span><span class="sxs-lookup"><span data-stu-id="c6c2f-529">Using Route Data for Data Source Control Parameters</span></span>

<span data-ttu-id="c6c2f-530">*RouteParameter*クラスでは、ルート データをデータ ソース コントロール内のクエリのパラメーター値として指定することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-530">The *RouteParameter* class lets you specify route data as a parameter value for queries in a data source control.</span></span> <span data-ttu-id="c6c2f-531">これは、[動作とほぼ同様に、](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)クラスに、次の例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-531">It [works much like the](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) class, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample46.aspx)]

<span data-ttu-id="c6c2f-532">ルート パラメーター searchterm の値が使用しての例では、@companyname内のパラメーター、<em>選択</em>ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-532">In this case, the value of the route parameter searchterm will be used for the @companyname parameter in the <em>Select</em> statement.</span></span>

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a><span data-ttu-id="c6c2f-533">クライアント Id の設定</span><span class="sxs-lookup"><span data-stu-id="c6c2f-533">Setting Client IDs</span></span>

<span data-ttu-id="c6c2f-534">新しい*ClientIDMode*プロパティ ASP.NET では、長年にわたる問題に対処してつまりコントロールを作成する方法、 *id*をレンダリングする要素の属性です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-534">The new *ClientIDMode* property addresses a long-standing issue in ASP.NET, namely how controls create the *id* attribute for elements that they render.</span></span> <span data-ttu-id="c6c2f-535">知ること、 *id*レンダリングされる要素の属性は、アプリケーションには、これらの要素を参照しているクライアント スクリプトが含まれている場合に重要です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-535">Knowing the *id* attribute for rendered elements is important if your application includes client script that references these elements.</span></span>

<span data-ttu-id="c6c2f-536">*Id*に基づいて Web サーバー コントロールのレンダリングされる html 属性を生成、 *ClientID*コントロールのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-536">The *id* attribute in HTML that is rendered for Web server controls is generated based on the *ClientID* property of the control.</span></span> <span data-ttu-id="c6c2f-537">ASP.NET 4 で、アルゴリズムを生成するまで、 *id*属性から、 *ClientID*プロパティが、ID、およびに (ように繰り返し使用されるコントロールの場合、(存在する場合)、名前付けコンテナーを連結するされてデータ コントロール)、プレフィックスと連続する番号を追加します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-537">Until ASP.NET 4, the algorithm for generating the *id* attribute from the *ClientID* property has been to concatenate the naming container (if any) with the ID, and in the case of repeated controls (as in data controls), to add a prefix and a sequential number.</span></span> <span data-ttu-id="c6c2f-538">コントロールがクライアント スクリプトで参照するが困難ためと、予測可能なインポートされなかった Id アルゴリズムが、ページ内のコントロールの Id が一意であることを保証これが常に中が発生しました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-538">While this has always guaranteed that the IDs of controls in the page are unique, the algorithm has resulted in control IDs that were not predictable, and were therefore difficult to reference in client script.</span></span>

<span data-ttu-id="c6c2f-539">新しい*ClientIDMode*プロパティを使用して、具体的には、クライアント ID を生成する方法のコントロールを指定できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-539">The new *ClientIDMode* property lets you specify more precisely how the client ID is generated for controls.</span></span> <span data-ttu-id="c6c2f-540">設定することができます、 *ClientIDMode*ページを含む任意のコントロールのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-540">You can set the *ClientIDMode* property for any control, including for the page.</span></span> <span data-ttu-id="c6c2f-541">使用可能な設定は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-541">Possible settings are the following:</span></span>

- <span data-ttu-id="c6c2f-542">*AutoID* – これは、生成するためのアルゴリズムを*ClientID* ASP.NET の以前のバージョンで使用されたプロパティの値。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-542">*AutoID* – This is equivalent to the algorithm for generating *ClientID* property values that was used in earlier versions of ASP.NET.</span></span>
- <span data-ttu-id="c6c2f-543">*静的*– これを指定する、 *ClientID*値は、親の名前付けコンテナーの Id を連結することがなく ID と同じになります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-543">*Static* – This specifies that the *ClientID* value will be the same as the ID without concatenating the IDs of parent naming containers.</span></span> <span data-ttu-id="c6c2f-544">これは、Web ユーザー コントロールに役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-544">This can be useful in Web user controls.</span></span> <span data-ttu-id="c6c2f-545">Web ユーザー コントロールできますが、別のコンテナー コントロールおよび別のページに配置しているために使用するコントロールのクライアント スクリプトを記述するが困難できます、 *AutoID*アルゴリズム ID の値がどうなるかを予測することはできませんので.</span><span class="sxs-lookup"><span data-stu-id="c6c2f-545">Because a Web user control can be located on different pages and in different container controls, it can be difficult to write client script for controls that use the *AutoID* algorithm because you cannot predict what the ID values will be.</span></span>
- <span data-ttu-id="c6c2f-546">*予測可能な*– このオプションは、主に繰り返しのテンプレートを使用するデータ コントロールで使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-546">*Predictable* – This option is primarily for use in data controls that use repeating templates.</span></span> <span data-ttu-id="c6c2f-547">生成されたが、コントロールの名前付けコンテナーの ID プロパティを連結*ClientID* "ctlxxx"などの文字列を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-547">It concatenates the ID properties of the control's naming containers, but generated *ClientID* values do not contain strings like "ctlxxx".</span></span> <span data-ttu-id="c6c2f-548">この設定は機能と組み合わせて、 *ClientIDRowSuffix*コントロールのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-548">This setting works in conjunction with the *ClientIDRowSuffix* property of the control.</span></span> <span data-ttu-id="c6c2f-549">設定する、 *ClientIDRowSuffix*プロパティをデータ フィールドの名前とそのフィールドの値が生成されるサフィックスとして使用*ClientID*値。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-549">You set the *ClientIDRowSuffix* property to the name of a data field, and the value of that field is used as the suffix for the generated *ClientID* value.</span></span> <span data-ttu-id="c6c2f-550">データのレコードとしての主キーを使用する通常、 *ClientIDRowSuffix*値。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-550">Typically you would use the primary key of a data record as the *ClientIDRowSuffix* value.</span></span>
- <span data-ttu-id="c6c2f-551">*継承*– この設定はコントロールの既定の動作です。 コントロールの ID の生成がその親と同じであることを指定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-551">*Inherit* – This setting is the default behavior for controls; it specifies that a control's ID generation is the same as its parent.</span></span>

<span data-ttu-id="c6c2f-552">設定することができます、 *ClientIDMode*ページ レベルのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-552">You can set the *ClientIDMode* property at the page level.</span></span> <span data-ttu-id="c6c2f-553">既定値を定義*ClientIDMode*現在のページのすべてのコントロールの値。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-553">This defines the default *ClientIDMode* value for all controls in the current page.</span></span>

<span data-ttu-id="c6c2f-554">既定値*ClientIDMode*ページ レベルの値は*AutoID*と既定*ClientIDMode*コントロール レベルの値は*継承*.</span><span class="sxs-lookup"><span data-stu-id="c6c2f-554">The default *ClientIDMode* value at the page level is *AutoID*, and the default *ClientIDMode* value at the control level is *Inherit*.</span></span> <span data-ttu-id="c6c2f-555">その結果、設定しないこのプロパティ任意の場所、コードで、すべてのコントロールは既定の*AutoID*アルゴリズムです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-555">As a result, if you do not set this property anywhere in your code, all controls will default to the *AutoID* algorithm.</span></span>

<span data-ttu-id="c6c2f-556">ページ レベルの値で設定する、 *@ Page*ディレクティブについては、次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-556">You set the page-level value in the *@ Page* directive, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample47.aspx)]

<span data-ttu-id="c6c2f-557">設定することも、 *ClientIDMode*コンピューター (machine) レベルまたはアプリケーション レベルで、構成ファイル内の値。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-557">You can also set the *ClientIDMode* value in the configuration file, either at the computer (machine) level or at the application level.</span></span> <span data-ttu-id="c6c2f-558">既定値を定義*ClientIDMode*アプリケーション内のすべてのページのすべてのコントロールを設定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-558">This defines the default *ClientIDMode* setting for all controls in all pages in the application.</span></span> <span data-ttu-id="c6c2f-559">既定値を定義、コンピューター レベルで、値を設定すると*ClientIDMode*そのコンピューターのすべての Web サイトを設定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-559">If you set the value at the computer level, it defines the default *ClientIDMode* setting for all Web sites on that computer.</span></span> <span data-ttu-id="c6c2f-560">次の例は、 *ClientIDMode*構成ファイルで設定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-560">The following example shows the *ClientIDMode* setting in the configuration file:</span></span>

[!code-xml[Main](overview/samples/sample48.xml)]

<span data-ttu-id="c6c2f-561">既に述べたとおりの値として、 *ClientID*プロパティはコントロールの親の名前付けコンテナーから派生します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-561">As noted earlier, the value of the *ClientID* property is derived from the naming container for a control's parent.</span></span> <span data-ttu-id="c6c2f-562">マスター ページを使用する場合など、一部のシナリオ コントロールが最終的に Id を持つもので、次のようにレンダリングされる HTML:</span><span class="sxs-lookup"><span data-stu-id="c6c2f-562">In some scenarios, such as when you are using master pages, controls can end up with IDs like those in the following rendered HTML:</span></span>

[!code-html[Main](overview/samples/sample49.html)]

<span data-ttu-id="c6c2f-563">場合でも、*入力*マークアップに示される要素 (から、 *テキスト ボックス*コントロール) 2 つの名前付けコンテナーは、ページの深さ (入れ子になった*ContentPlaceholder*コントロール)、マスター ページが処理されるため、最終結果は、次のようにコントロール ID です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-563">Even though the *input* element shown in the markup (from a *TextBox* control) is only two naming containers deep in the page (the nested *ContentPlaceholder* controls), because of the way master pages are processed, the end result is a control ID like the following:</span></span>

[!code-console[Main](overview/samples/sample50.cmd)]

<span data-ttu-id="c6c2f-564">この ID ページで、一意であることが保証しますが、不必要にほとんどの目的の長さ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-564">This ID is guaranteed to be unique in the page, but is unnecessarily long for most purposes.</span></span> <span data-ttu-id="c6c2f-565">レンダリングの ID の長さを短くして ID を生成する方法より細かく制御することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-565">Imagine that you want to reduce the length of the rendered ID, and to have more control over how the ID is generated.</span></span> <span data-ttu-id="c6c2f-566">(たとえば、する"ctlxxx"プレフィックスを排除します。)これを実現する最も簡単な方法は、設定して、 *ClientIDMode*次の例で示すようにプロパティ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-566">(For example, you want to eliminate "ctlxxx" prefixes.) The easiest way to achieve this is by setting the *ClientIDMode* property as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample51.aspx)]

<span data-ttu-id="c6c2f-567">このサンプルでは、 *ClientIDMode*プロパティに設定されている*静的*、最も外側の*NamingPanel*要素、および設定する*Predictable*内部の*NamingControl*要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-567">In this sample, the *ClientIDMode* property is set to *Static* for the outermost *NamingPanel* element, and set to *Predictable* for the inner *NamingControl* element.</span></span> <span data-ttu-id="c6c2f-568">これらの設定 (ページおよびマスター ページの残りの部分は前の例では、同じであると見なされます)、次のマークアップで生成されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-568">These settings result in the following markup (the rest of the page and the master page is assumed to be the same as in the previous example):</span></span>

[!code-html[Main](overview/samples/sample52.html)]

<span data-ttu-id="c6c2f-569">*静的*設定は、最も外側にある、そのすべてのコントロールの名前付けの階層をリセットすることによる影響*NamingPanel*要素、および削除する、 *ContentPlaceHolder*と*MasterPage*生成された ID からの Id</span><span class="sxs-lookup"><span data-stu-id="c6c2f-569">The *Static* setting has the effect of resetting the naming hierarchy for any controls inside the outermost *NamingPanel* element, and of eliminating the *ContentPlaceHolder* and *MasterPage* IDs from the generated ID.</span></span> <span data-ttu-id="c6c2f-570">(、*名前*レンダリングされる要素の属性が影響を受けるので、通常の ASP.NET 機能を保持するイベントの状態を表示し、します)。命名階層をリセットする際の副作用のマークアップを移動する場合でも、 *NamingPanel*を別の要素*ContentPlaceholder*コントロール、レンダリングされたクライアント Id は変わりません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-570">(The *name* attribute of rendered elements is unaffected, so the normal ASP.NET functionality is retained for events, view state, and so on.) A side effect of resetting the naming hierarchy is that even if you move the markup for the *NamingPanel* elements to a different *ContentPlaceholder* control, the rendered client IDs remain the same.</span></span>

> [!NOTE]
> <span data-ttu-id="c6c2f-571">レンダリングされたコントロール Id が一意であるかどうかを確認するかどうかは注意してください。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-571">Note It is up to you to make sure that the rendered control IDs are unique.</span></span> <span data-ttu-id="c6c2f-572">クライアントなど、個々 の HTML 要素の一意の Id を必要とする機能をすべてを壊れる可能性がない場合、*に対して*関数。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-572">If they are not, it can break any functionality that requires unique IDs for individual HTML elements, such as the client *document.getElementById* function.</span></span>


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a><span data-ttu-id="c6c2f-573">データ バインド コントロールでの予測可能なクライアント Id の作成</span><span class="sxs-lookup"><span data-stu-id="c6c2f-573">Creating Predictable Client IDs in Data-Bound Controls</span></span>

<span data-ttu-id="c6c2f-574">*ClientID*レガシ アルゴリズムによってデータ連結リスト コントロール内のコントロールにすることができますに生成される値が長いし、は実際に予測できません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-574">The *ClientID* values that are generated for controls in a data-bound list control by the legacy algorithm can be long and are not really predictable.</span></span> <span data-ttu-id="c6c2f-575">*ClientIDMode*機能をどのようにこれらの Id の生成を制御の詳細があるのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-575">The *ClientIDMode* functionality can help you have more control over how these IDs are generated.</span></span>

<span data-ttu-id="c6c2f-576">次の例では、マークアップが含まれています、 *ListView*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-576">The markup in the following example includes a *ListView* control:</span></span>

[!code-aspx[Main](overview/samples/sample53.aspx)]

<span data-ttu-id="c6c2f-577">前の例で、 *ClientIDMode*と*RowClientIDRowSuffix*プロパティ マークアップで設定されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-577">In the previous example, the *ClientIDMode* and *RowClientIDRowSuffix* properties are set in markup.</span></span> <span data-ttu-id="c6c2f-578">*ClientIDRowSuffix*プロパティは、データ バインド コントロールでのみ使用することができを使用するコントロールに応じてでその動作が異なります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-578">The *ClientIDRowSuffix* property can be used only in data-bound controls, and its behavior differs depending on which control you are using.</span></span> <span data-ttu-id="c6c2f-579">この違いは、これらは。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-579">The differences are these:</span></span>

- <span data-ttu-id="c6c2f-580">*GridView*コントロールなどを指定できます 1 つまたは複数の列の名前、データ ソースでは、クライアント Id を作成する実行時に組み合わされます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-580">*GridView* control — You can specify the name of one or more columns in the data source, which are combined at run time to create the client IDs.</span></span> <span data-ttu-id="c6c2f-581">設定する場合など、 *RowClientIDRowSuffix* "ProductName、ProductId"にレンダリングされる要素は、次のような形式であるために、コントロール Id:</span><span class="sxs-lookup"><span data-stu-id="c6c2f-581">For example, if you set *RowClientIDRowSuffix* to "ProductName, ProductId", control IDs for rendered elements will have a format like the following:</span></span>

- [!code-console[Main](overview/samples/sample54.cmd)]

- <span data-ttu-id="c6c2f-582">*ListView*コントロール-クライアント ID に追加されているデータ ソースで 1 つの列を指定できます</span><span class="sxs-lookup"><span data-stu-id="c6c2f-582">*ListView* control — You can specify a single column in the data source that is appended to the client ID.</span></span> <span data-ttu-id="c6c2f-583">設定する場合など、 *ClientIDRowSuffix* "ProductName"にレンダリングされたコントロール Id は、次のような形式。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-583">For example, if you set *ClientIDRowSuffix* to "ProductName", the rendered control IDs will have a format like the following:</span></span>

- [!code-console[Main](overview/samples/sample55.cmd)]

- <span data-ttu-id="c6c2f-584">ここでは、末尾の 1 は、現在のデータ項目の製品 ID から派生されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-584">In this case the trailing 1 is derived from the product ID of the current data item.</span></span>

- <span data-ttu-id="c6c2f-585">*リピータ*コントロール — このコントロールはサポートされない、 *ClientIDRowSuffix*プロパティです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-585">*Repeater* control— This control does not support the *ClientIDRowSuffix* property.</span></span> <span data-ttu-id="c6c2f-586">*リピータ*コントロール、現在の行のインデックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-586">In a *Repeater* control, the index of the current row is used.</span></span> <span data-ttu-id="c6c2f-587">ClientIDMode を使用する場合に"Predictable"を =、*リピータ*を制御する、次の形式を持つクライアント Id が生成されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-587">When you use ClientIDMode="Predictable" with a *Repeater* control, client IDs are generated that have the following format:</span></span>

- [!code-console[Main](overview/samples/sample56.cmd)]

- <span data-ttu-id="c6c2f-588">末尾の 0 は、現在の行のインデックスです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-588">The trailing 0 is the index of the current row.</span></span>

<span data-ttu-id="c6c2f-589">*FormView*と*DetailsView*コントロール表示しない、複数の行をサポートしていないため、 *ClientIDRowSuffix*プロパティです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-589">The *FormView* and *DetailsView* controls do not display multiple rows, so they do not support the *ClientIDRowSuffix* property.</span></span>

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a><span data-ttu-id="c6c2f-590">データ コントロール内で保持する行の選択</span><span class="sxs-lookup"><span data-stu-id="c6c2f-590">Persisting Row Selection in Data Controls</span></span>

<span data-ttu-id="c6c2f-591">*GridView*と*ListView*コントロールは行を選択してユーザーをさせることができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-591">The *GridView* and *ListView* controls can let users select a row.</span></span> <span data-ttu-id="c6c2f-592">ASP.NET の以前のバージョンではされてに基づいて選択 ページで行インデックスされます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-592">In previous versions of ASP.NET, selection has been based on the row index on the page.</span></span> <span data-ttu-id="c6c2f-593">たとえば、1 ページ目の 3 番目の項目を選択して、2 ページを移動して、そのページで、3 番目の項目が選択されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-593">For example, if you select the third item on page 1 and then move to page 2, the third item on that page is selected.</span></span>

<span data-ttu-id="c6c2f-594">持続する選択が最初に .NET Framework 3.5 SP1 での動的なデータ プロジェクトでのみサポート。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-594">Persisted selection was initially supported only in Dynamic Data projects in the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="c6c2f-595">この機能を有効にすると、現在の選択項目が項目のデータのキーに基づいています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-595">When this feature is enabled, the current selected item is based on the data key for the item.</span></span> <span data-ttu-id="c6c2f-596">これは、1 ページ目の 3 番目の行を選択して、2 ページに移動して場合、nothing が選択されている 2 ページ目にことを意味します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-596">This means that if you select the third row on page 1 and move to page 2, nothing is selected on page 2.</span></span> <span data-ttu-id="c6c2f-597">1 ページに戻ると 3 番目の行が選択されたままです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-597">When you move back to page 1, the third row is still selected.</span></span> <span data-ttu-id="c6c2f-598">持続する選択がサポートされるようになりました、 *GridView*と*ListView*を使用してすべてのプロジェクト内のコントロール、 *EnablePersistedSelection*プロパティのように、次の例:</span><span class="sxs-lookup"><span data-stu-id="c6c2f-598">Persisted selection is now supported for the *GridView* and *ListView* controls in all projects by using the *EnablePersistedSelection* property, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a><span data-ttu-id="c6c2f-599">ASP.NET のグラフ コントロール</span><span class="sxs-lookup"><span data-stu-id="c6c2f-599">ASP.NET Chart Control</span></span>

<span data-ttu-id="c6c2f-600">ASP.NET*グラフ*コントロールは、.NET Framework のデータ ビジュアライゼーション ソリューションを拡張します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-600">The ASP.NET *Chart* control expands the data-visualization offerings in the .NET Framework.</span></span> <span data-ttu-id="c6c2f-601">使用して、*グラフ*コントロール、必要な複雑な統計分析や財務分析の直感的で視覚的に説得力のあるグラフを ASP.NET ページを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-601">Using the *Chart* control, you can create ASP.NET pages that have intuitive and visually compelling charts for complex statistical or financial analysis.</span></span> <span data-ttu-id="c6c2f-602">ASP.NET*グラフ*コントロールは、.NET Framework version 3.5 SP1 のリリースにアドオンとして導入され、.NET Framework 4 のリリースの一部であります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-602">The ASP.NET *Chart* control was introduced as an add-on to the .NET Framework version 3.5 SP1 release and is part of the .NET Framework 4 release.</span></span>

<span data-ttu-id="c6c2f-603">コントロールには、次の機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-603">The control includes the following features:</span></span>

- <span data-ttu-id="c6c2f-604">35 種類のグラフ</span><span class="sxs-lookup"><span data-stu-id="c6c2f-604">35 distinct chart types.</span></span>
- <span data-ttu-id="c6c2f-605">グラフ領域、タイトル、凡例、および注釈の無制限の数。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-605">An unlimited number of chart areas, titles, legends, and annotations.</span></span>
- <span data-ttu-id="c6c2f-606">さまざまなすべてのグラフ要素の外観設定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-606">A wide variety of appearance settings for all chart elements.</span></span>
- <span data-ttu-id="c6c2f-607">ほとんどの種類のグラフの 3-D サポートします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-607">3-D support for most chart types.</span></span>
- <span data-ttu-id="c6c2f-608">スマート データ ラベルをデータ ポイントに関連自動的に合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-608">Smart data labels that can automatically fit around data points.</span></span>
- <span data-ttu-id="c6c2f-609">ストリップ ライン、スケールの区切り線、およびスケーリングの対数。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-609">Strip lines, scale breaks, and logarithmic scaling.</span></span>
- <span data-ttu-id="c6c2f-610">データの分析と変換に使用できる 50 を超える財務式および統計式。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-610">More than 50 financial and statistical formulas for data analysis and transformation.</span></span>
- <span data-ttu-id="c6c2f-611">単純バインディングとグラフのデータを操作します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-611">Simple binding and manipulation of chart data.</span></span>
- <span data-ttu-id="c6c2f-612">日付、時刻、通貨などの一般的なデータ形式をサポートします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-612">Support for common data formats such as dates, times, and currency.</span></span>
- <span data-ttu-id="c6c2f-613">対話機能とクライアントを含む、イベント ドリブンのカスタマイズのサポートは、Ajax を使用してイベントをクリックします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-613">Support for interactivity and event-driven customization, including client click events using Ajax.</span></span>
- <span data-ttu-id="c6c2f-614">状態管理。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-614">State management.</span></span>
- <span data-ttu-id="c6c2f-615">バイナリ ストリーム転送。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-615">Binary streaming.</span></span>

<span data-ttu-id="c6c2f-616">次の図は、ASP.NET のグラフ コントロールによって生成される財務グラフの例を紹介します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-616">The following figures show examples of financial charts that are produced by the ASP.NET Chart control.</span></span>

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

<span data-ttu-id="c6c2f-617">図 2: ASP.NET のグラフ コントロールの例</span><span class="sxs-lookup"><span data-stu-id="c6c2f-617">Figure 2: ASP.NET Chart control examples</span></span>

<span data-ttu-id="c6c2f-618">ASP.NET のグラフ コントロールを使用する方法の例については、サンプル コードをダウンロード、[用 Microsoft グラフ コントロールのサンプル環境](https://go.microsoft.com/fwlink/?LinkId=128300)MSDN Web サイトのページです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-618">For more examples of how to use the ASP.NET Chart control, download the sample code on the [Samples Environment for Microsoft Chart Controls](https://go.microsoft.com/fwlink/?LinkId=128300) page on the MSDN Web site.</span></span> <span data-ttu-id="c6c2f-619">コンテンツ コミュニティの他のサンプルを見つけることができます、 [Chart Control Forum](https://go.microsoft.com/fwlink/?LinkId=128713)です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-619">You can find more samples of community content at the [Chart Control Forum](https://go.microsoft.com/fwlink/?LinkId=128713).</span></span>

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a><span data-ttu-id="c6c2f-620">ASP.NET ページへのグラフ コントロールの追加</span><span class="sxs-lookup"><span data-stu-id="c6c2f-620">Adding the Chart Control to an ASP.NET Page</span></span>

<span data-ttu-id="c6c2f-621">次の例は、追加する方法を示します、*グラフ*マークアップを使用して、ASP.NET ページを制御します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-621">The following example shows how to add a *Chart* control to an ASP.NET page by using markup.</span></span> <span data-ttu-id="c6c2f-622">この例で、*グラフ*コントロールが静的なデータ ポイントの縦棒グラフを生成します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-622">In the example, the *Chart* control produces a column chart for static data points.</span></span>

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a><span data-ttu-id="c6c2f-623">3-D グラフの使用</span><span class="sxs-lookup"><span data-stu-id="c6c2f-623">Using 3-D Charts</span></span>

<span data-ttu-id="c6c2f-624">*グラフ*コントロールに含まれる、 *ChartAreas*含めることができるコレクション*ChartArea*グラフ領域の特性を定義するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-624">The *Chart* control contains a *ChartAreas* collection, which can contain *ChartArea* objects that define characteristics of chart areas.</span></span> <span data-ttu-id="c6c2f-625">たとえば、グラフ領域の 3-D を使用する次のように使用します。、 *Area3DStyle*例を次のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-625">For example, to use 3-D for a chart area, use the *Area3DStyle* property as in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample59.aspx)]

<span data-ttu-id="c6c2f-626">次の図は、4 つの系列を 3-D グラフを示しています、*バー*グラフの種類。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-626">The figure below shows a 3-D chart with four series of the *Bar* chart type.</span></span>

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

<span data-ttu-id="c6c2f-627">図 3: 3-D の横棒グラフ</span><span class="sxs-lookup"><span data-stu-id="c6c2f-627">Figure 3: 3-D Bar Chart</span></span>

#### <a name="using-scale-breaks-and-logarithmic-scales"></a><span data-ttu-id="c6c2f-628">スケール区切り、対数スケールを使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-628">Using Scale Breaks and Logarithmic Scales</span></span>

<span data-ttu-id="c6c2f-629">スケール区切り、対数スケールは、洗練されたグラフを追加する、2 つの追加方法です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-629">Scale breaks and logarithmic scales are two additional ways to add sophistication to the chart.</span></span> <span data-ttu-id="c6c2f-630">これらの機能は、グラフ領域内の各軸に固有です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-630">These features are specific to each axis in a chart area.</span></span> <span data-ttu-id="c6c2f-631">たとえば、グラフ領域の主軸 Y 軸にこれらの機能を使用する次のように使用します。、 *AxisY.IsLogarithmic*と*ScaleBreakStyle*プロパティで、 *ChartArea*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-631">For example, to use these features on the primary Y axis of a chart area, use the *AxisY.IsLogarithmic* and *ScaleBreakStyle* properties in a *ChartArea* object.</span></span> <span data-ttu-id="c6c2f-632">次のスニペットは、主軸の Y 軸のスケール区切りを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-632">The following snippet shows how to use scale breaks on the primary Y axis.</span></span>

[!code-aspx[Main](overview/samples/sample60.aspx)]

<span data-ttu-id="c6c2f-633">次の図は、スケール区切りを有効になっていると、Y 軸を示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-633">The figure below shows the Y axis with scale breaks enabled.</span></span>

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

<span data-ttu-id="c6c2f-634">図 4: スケール区切り</span><span class="sxs-lookup"><span data-stu-id="c6c2f-634">Figure 4: Scale Breaks</span></span>

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a><span data-ttu-id="c6c2f-635">QueryExtender コントロールにデータをフィルター処理</span><span class="sxs-lookup"><span data-stu-id="c6c2f-635">Filtering Data with the QueryExtender Control</span></span>

<span data-ttu-id="c6c2f-636">データ ドリブン Web ページを作成する開発者向けに非常に一般的なタスク データをフィルター処理を開始します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-636">A very common task for developers who create data-driven Web pages is to filter data.</span></span> <span data-ttu-id="c6c2f-637">これは、従来によって実行されたビルド*場所*ソース コントロールのデータ内の句。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-637">This traditionally has been performed by building *Where* clauses in data source controls.</span></span> <span data-ttu-id="c6c2f-638">この方法は複雑になることができ、場合によっては、*場所*構文では、基になるデータベースのすべての機能を活用することはできません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-638">This approach can be complicated, and in some cases the *Where* syntax does not let you take advantage of the full functionality of the underlying database.</span></span>

<span data-ttu-id="c6c2f-639">新しいフィルター容易にする*QueryExtender* ASP.NET 4 でコントロールが追加されました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-639">To make filtering easier, a new *QueryExtender* control has been added in ASP.NET 4.</span></span> <span data-ttu-id="c6c2f-640">このコントロールに追加できます*EntityDataSource*または*LinqDataSource*これらのコントロールによって返されるデータをフィルター処理するためにコントロールできます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-640">This control can be added to *EntityDataSource* or *LinqDataSource* controls in order to filter the data returned by these controls.</span></span> <span data-ttu-id="c6c2f-641">*QueryExtender* LINQ に依存しているコントロール、データが非常に効率的な操作の結果 ページに送信される前に、データベース サーバーで、フィルターが適用されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-641">Because the *QueryExtender* control relies on LINQ, the filter is applied on the database server before the data is sent to the page, which results in very efficient operations.</span></span>

<span data-ttu-id="c6c2f-642">*QueryExtender*コントロールは、さまざまなフィルター オプションをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-642">The *QueryExtender* control supports a variety of filter options.</span></span> <span data-ttu-id="c6c2f-643">次のセクションは、これらのオプションについて説明し、その使用方法の例を紹介します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-643">The following sections describe these options and provide examples of how to use them.</span></span>

#### <a name="search"></a><span data-ttu-id="c6c2f-644">検索</span><span class="sxs-lookup"><span data-stu-id="c6c2f-644">Search</span></span>

<span data-ttu-id="c6c2f-645">検索オプションの場合、 *QueryExtender*コントロールは、指定したフィールドに検索を実行します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-645">For the search option, the *QueryExtender* control performs a search in specified fields.</span></span> <span data-ttu-id="c6c2f-646">次の例では、コントロールを使用し、TextBoxSearch コントロール内でそのコンテンツを検索に入力されたテキスト、`ProductName`と`Supplier.CompanyName`から返されるデータの列、 *LinqDataSource*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-646">In the following example, the control uses the text that is entered in the TextBoxSearch control and searches for its contents in the `ProductName` and `Supplier.CompanyName` columns in the data that is returned from the *LinqDataSource* control.</span></span>

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a><span data-ttu-id="c6c2f-647">範囲</span><span class="sxs-lookup"><span data-stu-id="c6c2f-647">Range</span></span>

<span data-ttu-id="c6c2f-648">範囲のオプションは、検索オプションに似ていますが、範囲を定義する値のペアを指定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-648">The range option is similar to the search option, but specifies a pair of values to define the range.</span></span> <span data-ttu-id="c6c2f-649">次の例で、 *QueryExtender*検索の制御、`UnitPrice`から返されるデータの列、 *LinqDataSource*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-649">In the following example, the *QueryExtender* control searches the `UnitPrice` column in the data returned from the *LinqDataSource* control.</span></span> <span data-ttu-id="c6c2f-650">範囲は、ページ上の TextBoxFrom と TextBoxTo コントロールから読み取られます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-650">The range is read from the TextBoxFrom and TextBoxTo controls on the page.</span></span>

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a><span data-ttu-id="c6c2f-651">PropertyExpression</span><span class="sxs-lookup"><span data-stu-id="c6c2f-651">PropertyExpression</span></span>

<span data-ttu-id="c6c2f-652">プロパティ式のオプションを使用して、プロパティの値の比較を定義できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-652">The property expression option lets you define a comparison to a property value.</span></span> <span data-ttu-id="c6c2f-653">式が評価された場合*true*、検査されているデータが返されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-653">If the expression evaluates to *true*, the data that is being examined is returned.</span></span> <span data-ttu-id="c6c2f-654">次の例で、 *QueryExtender*コントロール内のデータを比較することでデータをフィルター処理、`Discontinued`ページ上の CheckBoxDiscontinued コントロールからの列には、値。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-654">In the following example, the *QueryExtender* control filters data by comparing the data in the `Discontinued` column to the value from the CheckBoxDiscontinued control on the page.</span></span>

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a><span data-ttu-id="c6c2f-655">CustomExpression</span><span class="sxs-lookup"><span data-stu-id="c6c2f-655">CustomExpression</span></span>

<span data-ttu-id="c6c2f-656">最後で使用するカスタム式を指定することができます、 *QueryExtender*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-656">Finally, you can specify a custom expression to use with the *QueryExtender* control.</span></span> <span data-ttu-id="c6c2f-657">このオプションにより、カスタム フィルターのロジックを定義する ページで、関数を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-657">This option lets you call a function in the page that defines custom filter logic.</span></span> <span data-ttu-id="c6c2f-658">次の例は、宣言でカスタム式を指定する方法を示します、 *QueryExtender*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-658">The following example shows how to declaratively specify a custom expression in the *QueryExtender* control.</span></span>

[!code-aspx[Main](overview/samples/sample64.aspx)]

<span data-ttu-id="c6c2f-659">次の例では、ユーザー定義関数によって呼び出される、 *QueryExtender*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-659">The following example shows the custom function that is invoked by the *QueryExtender* control.</span></span> <span data-ttu-id="c6c2f-660">データベースを含むクエリを使用する代わりに、ここで、*場所*句、コードを使用して LINQ クエリ、データをフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-660">In this case, instead of using a database query that includes a *Where* clause, the code uses a LINQ query to filter the data.</span></span>

[!code-csharp[Main](overview/samples/sample65.cs)]

<span data-ttu-id="c6c2f-661">これらの例で使用されている 1 つだけ式を表示する、 *QueryExtender*一度にコントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-661">These examples show only one expression being used in the *QueryExtender* control at a time.</span></span> <span data-ttu-id="c6c2f-662">ただし、内の複数の式を含めることができます、 *QueryExtender*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-662">However, you can include multiple expressions inside the *QueryExtender* control.</span></span>

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a><span data-ttu-id="c6c2f-663">Html エンコードのコード式</span><span class="sxs-lookup"><span data-stu-id="c6c2f-663">Html Encoded Code Expressions</span></span>

<span data-ttu-id="c6c2f-664">使用してに大きく依存します (特に、ASP.NET MVC) の ASP.NET サイト`<%` =  `expression %>`構文 (「コード ナゲット」と呼ばれる多くの場合)、応答にいくつかのテキストを書き込む。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-664">Some ASP.NET sites (especially with ASP.NET MVC) rely heavily on using `<%`= `expression %>` syntax (often called "code nuggets") to write some text to the response.</span></span> <span data-ttu-id="c6c2f-665">コード式を使用する場合は、ことを忘れがちに HTML でエンコードするテキストがユーザーを元の場合は、テキスト入力ですが、これがページを開いておく (クロスサイト スクリプティング) XSS 攻撃に対して簡単です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-665">When you use code expressions, it is easy to forget to HTML-encode the text, If the text comes from user input, it can leave pages open to an XSS (Cross Site Scripting) attack.</span></span>

<span data-ttu-id="c6c2f-666">ASP.NET 4 には、次の新しい構文のコード式が導入されています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-666">ASP.NET 4 introduces the following new syntax for code expressions:</span></span>

[!code-aspx[Main](overview/samples/sample66.aspx)]

<span data-ttu-id="c6c2f-667">この構文は、応答に書き込むときに既定では HTML エンコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-667">This syntax uses HTML encoding by default when writing to the response.</span></span> <span data-ttu-id="c6c2f-668">この新しい式は、次を効果的に変換します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-668">This new expression effectively translates to the following:</span></span>

[!code-aspx[Main](overview/samples/sample67.aspx)]

<span data-ttu-id="c6c2f-669">たとえば、 &lt;%: 要求 ["UserInput"] %&gt; HTML エンコーディングの値を実行*要求 ["UserInput"]* です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-669">For example, &lt;%: Request["UserInput"] %&gt; performs HTML encoding on the value of *Request["UserInput"]*.</span></span>

<span data-ttu-id="c6c2f-670">この機能の目的は、新しい構文を使用する各手順で決定する強制されないように、古い構文のすべてのインスタンスを置換できるようにすることです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-670">The goal of this feature is to make it possible to replace all instances of the old syntax with the new syntax so that you are not forced to decide at every step which one to use.</span></span> <span data-ttu-id="c6c2f-671">ただし、場合があります出力されているテキストが HTML を使用するものではまたは既にエンコードされている場合にダブル エンコードする可能性が。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-671">However, there are cases in which the text being output is meant to be HTML or is already encoded, in which case this could lead to double encoding.</span></span>

<span data-ttu-id="c6c2f-672">ASP.NET 4 のような場合に、新しいインターフェイスが導入されています*IHtmlString*、具体的な実装と共に*HtmlString*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-672">For those cases, ASP.NET 4 introduces a new interface, *IHtmlString*, along with a concrete implementation, *HtmlString*.</span></span> <span data-ttu-id="c6c2f-673">これらの型のインスタンスを示し、こと、戻り値は、既に正しくエンコードされて (またはそれ以外の場合) を HTML として表示するため、値がされないこと HTML エンコードされた再度できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-673">Instances of these types let you indicate that the return value is already properly encoded (or otherwise examined) for displaying as HTML, and that therefore the value should not be HTML-encoded again.</span></span> <span data-ttu-id="c6c2f-674">たとえば、次することはできません (とは) HTML エンコードします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-674">For example, the following should not be (and is not) HTML encoded:</span></span>

[!code-aspx[Main](overview/samples/sample68.aspx)]

<span data-ttu-id="c6c2f-675">ASP.NET MVC 2 のヘルパー メソッドは、ASP.NET 4 を実行しているときにこの新しい構文の動作は二重のエンコードするために更新されています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-675">ASP.NET MVC 2 helper methods have been updated to work with this new syntax so that they are not double encoded, but only when you are running ASP.NET 4.</span></span> <span data-ttu-id="c6c2f-676">この新しい構文では、ASP.NET 3.5 SP1 を使用してアプリケーションを実行するときに機能しません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-676">This new syntax does not work when you run an application using ASP.NET 3.5 SP1.</span></span>

<span data-ttu-id="c6c2f-677">XSS 攻撃から保護は保証されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-677">Keep in mind that this does not guarantee protection from XSS attacks.</span></span> <span data-ttu-id="c6c2f-678">たとえば、引用符に含まれていない属性値を使用する HTML がユーザー入力を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-678">For example, HTML that uses attribute values that are not in quotation marks can contain user input that is still susceptible.</span></span> <span data-ttu-id="c6c2f-679">ASP.NET コントロールや ASP.NET MVC ヘルパーの出力には常が含まれている属性値に引用符では推奨される方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-679">Note that the output of ASP.NET controls and ASP.NET MVC helpers always includes attribute values in quotation marks, which is the recommended approach.</span></span>

<span data-ttu-id="c6c2f-680">同様に、この構文では、ユーザー入力に基づいて JavaScript 文字列を作成する場合など、JavaScript のエンコードは行いません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-680">Likewise, this syntax does not perform JavaScript encoding, such as when you create a JavaScript string based on user input.</span></span>

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a><span data-ttu-id="c6c2f-681">プロジェクト テンプレートの変更</span><span class="sxs-lookup"><span data-stu-id="c6c2f-681">Project Template Changes</span></span>

<span data-ttu-id="c6c2f-682">以前のバージョンの ASP.NET では、Visual Studio を使用して、新しい Web サイト プロジェクトまたは Web アプリケーション プロジェクトを作成するときに結果として得られるプロジェクトでは、Default.aspx ページだけで、既定値`Web.config`ファイル、および`App_Data`では、次に示すように、フォルダー図:</span><span class="sxs-lookup"><span data-stu-id="c6c2f-682">In earlier versions of ASP.NET, when you use Visual Studio to create a new Web Site project or Web Application project, the resulting projects contain only a Default.aspx page, a default `Web.config` file, and the `App_Data` folder, as shown in the following illustration:</span></span>

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

<span data-ttu-id="c6c2f-683">Visual Studio には、空の Web サイト プロジェクトの種類も含まれていないファイルも、次の図に示すようにもサポートされます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-683">Visual Studio also supports an Empty Web Site project type, which contains no files at all, as shown in the following figure:</span></span>

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

<span data-ttu-id="c6c2f-684">結果は、初心者にとってはほとんどのガイダンスが実稼働 Web アプリケーションを構築する方法です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-684">The result is that for the beginner, there is very little guidance on how to build a production Web application.</span></span> <span data-ttu-id="c6c2f-685">そのため、ASP.NET 4 には、次の 3 つの新しいテンプレート、空の Web アプリケーション プロジェクトと 1 つ各プロジェクトの Web アプリケーションと Web サイトが導入されています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-685">Therefore, ASP.NET 4 introduces three new templates, one for an empty Web application project, and one each for a Web Application and Web Site project.</span></span>

#### <a name="empty-web-application-template"></a><span data-ttu-id="c6c2f-686">空の Web アプリケーション テンプレート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-686">Empty Web Application Template</span></span>

<span data-ttu-id="c6c2f-687">名前が示すように、空の Web アプリケーション テンプレートは抑えた Web アプリケーション プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-687">As the name suggests, the Empty Web Application template is a stripped-down Web Application project.</span></span> <span data-ttu-id="c6c2f-688">次の図に示すように、Visual Studio の新しいプロジェクト ダイアログ ボックスからこのプロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-688">You select this project template from the Visual Studio New Project dialog box, as shown in the following figure:</span></span>

[![](overview/_static/image7.png)](overview/_static/image6.png)

<span data-ttu-id="c6c2f-689">([フルサイズのイメージを表示するをクリックして](overview/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="c6c2f-689">([Click to view full-size image](overview/_static/image8.png))</span></span>

<span data-ttu-id="c6c2f-690">空の ASP.NET Web アプリケーションを作成するときに、Visual Studio には、次のフォルダーのレイアウトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-690">When you create an Empty ASP.NET Web Application, Visual Studio creates the following folder layout:</span></span>

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

<span data-ttu-id="c6c2f-691">以前のバージョンの ASP.NET、1 つの例外を空の Web サイトのレイアウトに似ています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-691">This is similar to the Empty Web Site layout from earlier versions of ASP.NET, with one exception.</span></span> <span data-ttu-id="c6c2f-692">Visual Studio 2010 では、空の Web アプリケーションおよび空の Web サイト プロジェクトが含まれている、次の最小限`Web.config`を Visual Studio によって、プロジェクトのターゲット フレームワークの識別に使用される情報を含むファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-692">In Visual Studio 2010, Empty Web Application and Empty Web Site projects contain the following minimal `Web.config` file that contains information used by Visual Studio to identify the framework that the project is targeting:</span></span>

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

<span data-ttu-id="c6c2f-693">指定しない場合*targetFramework*プロパティ、Visual Studio の既定値は、古いバージョンのアプリケーションを開くときに、互換性を保つために、.NET Framework 2.0 を対象とします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-693">Without this *targetFramework* property, Visual Studio defaults to targeting the .NET Framework 2.0 in order to preserve compatibility when opening older applications.</span></span>

#### <a name="web-application-and-web-site-project-templates"></a><span data-ttu-id="c6c2f-694">Web アプリケーションおよび Web サイト プロジェクト テンプレート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-694">Web Application and Web Site Project Templates</span></span>

<span data-ttu-id="c6c2f-695">その他の 2 つ新しいプロジェクト テンプレート Visual Studio 2010 に付属するにはには、主要な変更が含まれて。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-695">The other two new project templates that are shipped with Visual Studio 2010 contain major changes.</span></span> <span data-ttu-id="c6c2f-696">次の図は、新しい Web アプリケーション プロジェクトを作成するときに作成されるプロジェクト レイアウトを示しています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-696">The following figure shows the project layout that is created when you create a new Web Application project.</span></span> <span data-ttu-id="c6c2f-697">(Web サイト プロジェクトのレイアウトは、ほぼ同じです)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-697">(The layout for a Web Site project is virtually identical.)</span></span>

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

<span data-ttu-id="c6c2f-698">プロジェクトには、以前のバージョンで作成されなかったファイルの数が含まれています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-698">The project includes a number of files that were not created in earlier versions.</span></span> <span data-ttu-id="c6c2f-699">さらに、新しい Web アプリケーション プロジェクトが構成に基本メンバーシップ機能は、新しいアプリケーションへのアクセスをセキュリティで保護するを迅速に取得することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-699">In addition, the new Web Application project is configured with basic membership functionality, which lets you quickly get started in securing access to the new application.</span></span> <span data-ttu-id="c6c2f-700">この信頼のため、`Web.config`ファイルの新しいプロジェクトには、メンバーシップ、ロール、およびプロファイルを構成するためのエントリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-700">Because of this inclusion, the `Web.config` file for the new project includes entries that are used to configure membership, roles, and profiles.</span></span> <span data-ttu-id="c6c2f-701">次の例は、`Web.config`新しい Web アプリケーション プロジェクトのファイルです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-701">The following example shows the `Web.config` file for a new Web Application project.</span></span> <span data-ttu-id="c6c2f-702">(この場合、 *roleManager*は無効になります)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-702">(In this case, *roleManager* is disabled.)</span></span>

[![](overview/_static/image13.png)](overview/_static/image12.png)

<span data-ttu-id="c6c2f-703">([フルサイズのイメージを表示するをクリックして](overview/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="c6c2f-703">([Click to view full-size image](overview/_static/image14.png))</span></span>

<span data-ttu-id="c6c2f-704">プロジェクトは、2 つ目も含まれています。`Web.config`ファイルで、`Account`ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-704">The project also contains a second `Web.config` file in the `Account` directory.</span></span> <span data-ttu-id="c6c2f-705">2 番目の構成ファイルでは、ログに記録されないユーザー ChangePassword.aspx のページへのアクセスをセキュリティで保護する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-705">The second configuration file provides a way to secure access to the ChangePassword.aspx page for non-logged in users.</span></span> <span data-ttu-id="c6c2f-706">次の例は、2 番目の内容を示しています。`Web.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-706">The following example shows the contents of the second `Web.config` file.</span></span>

![](overview/_static/image15.png)

<span data-ttu-id="c6c2f-707">既定では、新しいプロジェクト テンプレートで作成されたページには、以前のバージョンよりもより多くのコンテンツも含まれます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-707">The pages created by default in the new project templates also contain more content than in previous versions.</span></span> <span data-ttu-id="c6c2f-708">プロジェクトには、既定のマスター ページ、CSS ファイルが含まれていて、既定では、既定のページ (Default.aspx) がマスター ページを使用するよう構成します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-708">The project contains a default master page and CSS file, and the default page (Default.aspx) is configured to use the master page by default.</span></span> <span data-ttu-id="c6c2f-709">結果は、最初に、Web アプリケーションまたは Web サイトを実行するときに既定値 (ホーム) ページを機能は既にです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-709">The result is that when you run the Web application or Web site for the first time, the default (home) page is already functional.</span></span> <span data-ttu-id="c6c2f-710">実際には、新しい MVC アプリケーションを起動するときに表示する既定のページに似ています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-710">In fact, it is similar to the default page you see when you start up a new MVC application.</span></span>

[![](overview/_static/image17.png)](overview/_static/image16.png)

<span data-ttu-id="c6c2f-711">([フルサイズのイメージを表示するをクリックして](overview/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="c6c2f-711">([Click to view full-size image](overview/_static/image18.png))</span></span>

<span data-ttu-id="c6c2f-712">これらの変更をプロジェクト テンプレートの目的は、新しい Web アプリケーションの構築を開始する方法についてガイダンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-712">The intention of these changes to the project templates is to provide guidance on how to start building a new Web application.</span></span> <span data-ttu-id="c6c2f-713">意味的に正しい、厳密な XHTML 1.0 に準拠したマークアップ、CSS を使用して指定されているレイアウトで、テンプレート内のページは ASP.NET 4 Web アプリケーションを構築するためのベスト プラクティスを表します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-713">With semantically correct, strict XHTML 1.0-compliant markup and with layout that is specified using CSS, the pages in the templates represent best practices for building ASP.NET 4 Web applications.</span></span> <span data-ttu-id="c6c2f-714">既定のページには、簡単にカスタマイズできる、2 列構成レイアウトもがあります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-714">The default pages also have a two-column layout that you can easily customize.</span></span>

<span data-ttu-id="c6c2f-715">たとえば、場合を考えます新しい Web アプリケーションの一部の色を変更し、My ASP.NET アプリケーションのロゴの代わりに、会社のロゴを挿入します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-715">For example, imagine that for a new Web Application you want to change some of the colors and insert your company logo in place of the My ASP.NET Application logo.</span></span> <span data-ttu-id="c6c2f-716">これを行うには、下に新しいディレクトリを作成する`Content`ロゴのイメージを保存します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-716">To do this, you create a new directory under `Content` to store your logo image:</span></span>

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

<span data-ttu-id="c6c2f-717">ページに、イメージを追加するを開く、`Site.Master`マイ ASP.NET アプリケーションのテキストが定義され、コードに置き換えますを検索して、ファイル、*イメージ*要素が*src*新しいロゴに属性が設定されています。次の例のようにの画像:</span><span class="sxs-lookup"><span data-stu-id="c6c2f-717">To add the image to the page, you then open the `Site.Master` file, find where the My ASP.NET Application text is defined, and replace it with an *image* element whose *src* attribute is set to the new logo image, as in the following example:</span></span>

[![](overview/_static/image21.png)](overview/_static/image20.png)

<span data-ttu-id="c6c2f-718">([フルサイズのイメージを表示するをクリックして](overview/_static/image22.png))</span><span class="sxs-lookup"><span data-stu-id="c6c2f-718">([Click to view full-size image](overview/_static/image22.png))</span></span>

<span data-ttu-id="c6c2f-719">Site.css ファイルに移動し、ページの背景色だけでなく、ヘッダーのものを変更する CSS クラスの定義を変更します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-719">You can then go into the Site.css file and modify CSS class definitions to change the background color of the page as well as that of the header.</span></span>

<span data-ttu-id="c6c2f-720">これらの変更の結果を非常に簡単にカスタマイズされたホーム ページを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-720">The result of these changes is that you can display a customized home page with very little effort:</span></span>

[![](overview/_static/image24.png)](overview/_static/image23.png)

<span data-ttu-id="c6c2f-721">([フルサイズのイメージを表示するをクリックして](overview/_static/image25.png))</span><span class="sxs-lookup"><span data-stu-id="c6c2f-721">([Click to view full-size image](overview/_static/image25.png))</span></span>

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a><span data-ttu-id="c6c2f-722">CSS の機能強化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-722">CSS Improvements</span></span>

<span data-ttu-id="c6c2f-723">ASP.NET 4 での作業の主要な領域のいずれかを最新の HTML 標準に準拠した HTML のレンダリングを支援するされました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-723">One of the major areas of work in ASP.NET 4 has been to help render HTML that is compliant with the latest HTML standards.</span></span> <span data-ttu-id="c6c2f-724">これには、ASP.NET Web サーバー コントロールが CSS スタイルを使用する方法の変更が含まれます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-724">This includes changes to how ASP.NET Web server controls use CSS styles.</span></span>

#### <a name="compatibility-setting-for-rendering"></a><span data-ttu-id="c6c2f-725">レンダリングの互換性設定</span><span class="sxs-lookup"><span data-stu-id="c6c2f-725">Compatibility Setting for Rendering</span></span>

<span data-ttu-id="c6c2f-726">既定では、Web アプリケーションまたは Web サイトは、.NET Framework 4 を対象の場合、 *controlRenderingCompatibilityVersion*の属性、*ページ*要素が「4.0」に設定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-726">By default, when a Web application or Web site targets the .NET Framework 4, the *controlRenderingCompatibilityVersion* attribute of the *pages* element is set to "4.0".</span></span> <span data-ttu-id="c6c2f-727">この要素がコンピューター レベルで定義されている`Web.config`ファイルし、既定ですべての ASP.NET 4 アプリケーションに適用されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-727">This element is defined in the machine-level `Web.config` file and by default applies to all ASP.NET 4 applications:</span></span>

[!code-xml[Main](overview/samples/sample69.xml)]

<span data-ttu-id="c6c2f-728">値は、 *controlRenderingCompatibility*潜在的な新しいバージョンの定義今後のリリースでは、文字列です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-728">The value for *controlRenderingCompatibility* is a string, which allows potential new version definitions in future releases.</span></span> <span data-ttu-id="c6c2f-729">現在のリリースでは、次の値がこのプロパティのサポートされています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-729">In the current release, the following values are supported for this property:</span></span>

- <span data-ttu-id="c6c2f-730">"3.5".</span><span class="sxs-lookup"><span data-stu-id="c6c2f-730">"3.5".</span></span> <span data-ttu-id="c6c2f-731">この設定は、従来のレンダリングやマークアップを示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-731">This setting indicates legacy rendering and markup.</span></span> <span data-ttu-id="c6c2f-732">コントロールによって表示されるマークアップが 100% の旧バージョンと互換性のあるの設定、*長期*プロパティが受け入れられます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-732">Markup rendered by controls is 100% backward compatible, and the setting of the *xhtmlConformance* property is honored.</span></span>
- <span data-ttu-id="c6c2f-733">"4.0".</span><span class="sxs-lookup"><span data-stu-id="c6c2f-733">"4.0".</span></span> <span data-ttu-id="c6c2f-734">プロパティにこの設定がある場合は、ASP.NET Web サーバー コントロールは、次を行います。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-734">If the property has this setting, ASP.NET Web server controls do the following:</span></span>
- <span data-ttu-id="c6c2f-735">*長期*プロパティは常に"Strict"として扱われます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-735">The *xhtmlConformance* property is always treated as "Strict".</span></span> <span data-ttu-id="c6c2f-736">その結果、コントロールは、XHTML 1.0 Strict マークアップを表示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-736">As a result, controls render XHTML 1.0 Strict markup.</span></span>
- <span data-ttu-id="c6c2f-737">不要になった非入力コントロールを無効にするには、無効なスタイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-737">Disabling non-input controls no longer renders invalid styles.</span></span>
- <span data-ttu-id="c6c2f-738">*div* CSS 規則をユーザーが作成したこれらの影響を与えないように、非表示フィールドの周りに要素がスタイルようになりました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-738">*div* elements around hidden fields are now styled so they do not interfere with user-created CSS rules.</span></span>
- <span data-ttu-id="c6c2f-739">メニュー コントロールは、意味的に正しく、ユーザ補助ガイドラインに準拠しているマークアップを表示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-739">Menu controls render markup that is semantically correct and compliant with accessibility guidelines.</span></span>
- <span data-ttu-id="c6c2f-740">検証コントロールは、インライン スタイルをレンダリングできません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-740">Validation controls do not render inline styles.</span></span>
- <span data-ttu-id="c6c2f-741">以前の境界線をレンダリングするコントロール =「0」(ASP.NET から派生したコントロール*テーブル*制御、および ASP.NET*イメージ*コントロール) 不要になったこの属性を表示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-741">Controls that previously rendered border="0" (controls that derive from the ASP.NET *Table* control, and the ASP.NET *Image* control) no longer render this attribute.</span></span>

#### <a name="disabling-controls"></a><span data-ttu-id="c6c2f-742">コントロールを無効にします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-742">Disabling Controls</span></span>

<span data-ttu-id="c6c2f-743">フレームワークは ASP.NET 3.5 SP1 およびそれ以前のバージョンで、レンダリング、*無効になっている*そのコントロールの HTML マークアップの属性*有効*プロパティに設定*false*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-743">In ASP.NET 3.5 SP1 and earlier versions, the framework renders the *disabled* attribute in the HTML markup for any control whose *Enabled* property set to *false*.</span></span> <span data-ttu-id="c6c2f-744">ただし、仕様に従った、HTML 4.01、のみ*入力*要素は、この属性を持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-744">However, according to the HTML 4.01 specification, only *input* elements should have this attribute.</span></span>

<span data-ttu-id="c6c2f-745">ASP.NET 4 で設定することができます、 *controlRenderingCompatabilityVersion*を次の例のように「3.5」のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-745">In ASP.NET 4, you can set the *controlRenderingCompatabilityVersion* property to "3.5", as in the following example:</span></span>

[!code-xml[Main](overview/samples/sample70.xml)]

<span data-ttu-id="c6c2f-746">用のマークアップを作成する場合があります、*ラベル*コントロールを無効にすると、次のようなコントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-746">You might create markup for a *Label* control like the following, which disables the control:</span></span>

[!code-aspx[Main](overview/samples/sample71.aspx)]

<span data-ttu-id="c6c2f-747">*ラベル*コントロールは、次の HTML をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-747">The *Label* control would render the following HTML:</span></span>

[!code-html[Main](overview/samples/sample72.html)]

<span data-ttu-id="c6c2f-748">ASP.NET 4 で設定することができます、 *controlRenderingCompatabilityVersion* 「4.0」にします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-748">In ASP.NET 4, you can set the *controlRenderingCompatabilityVersion* to "4.0".</span></span> <span data-ttu-id="c6c2f-749">その場合は、そのレンダリングでのみ制御*入力*要素は表示されます、*無効になっている*属性の場合に、コントロールの*有効*プロパティに設定されている*false*.</span><span class="sxs-lookup"><span data-stu-id="c6c2f-749">In that case, only controls that render *input* elements will render a *disabled* attribute when the control's *Enabled* property is set to *false*.</span></span> <span data-ttu-id="c6c2f-750">HTML をレンダリングできませんコントロール*入力*要素をレンダリング、*クラス*を無効になっているコントロールの外観を定義している CSS クラスを参照する属性。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-750">Controls that do not render HTML *input* elements instead render a *class* attribute that references a CSS class that you can use to define a disabled look for the control.</span></span> <span data-ttu-id="c6c2f-751">たとえば、*ラベル*先ほどの例に示すようにコントロールが、次のマークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-751">For example, the *Label* control shown in the earlier example would generate the following markup:</span></span>

[!code-html[Main](overview/samples/sample73.html)]

<span data-ttu-id="c6c2f-752">このコントロールの指定したクラスの既定値は、"aspNetDisabled"です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-752">The default value for the class that specified for this control is "aspNetDisabled".</span></span> <span data-ttu-id="c6c2f-753">ただし、この既定値を変更するには、静的な*DisabledCssClass*の静的プロパティ、 *WebControl*クラスです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-753">However, you can change this default value by setting the static *DisabledCssClass* static property of the *WebControl* class.</span></span> <span data-ttu-id="c6c2f-754">コントロールの開発者の特定のコントロールを使用する動作も定義できますを使用して、 *SupportsDisabledAttribute*プロパティです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-754">For control developers, the behavior to use for a specific control can also be defined using the *SupportsDisabledAttribute* property.</span></span>

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a><span data-ttu-id="c6c2f-755">Div 要素を非表示のフィールドを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-755">Hiding div Elements Around Hidden Fields</span></span>

<span data-ttu-id="c6c2f-756">ASP.NET 2.0 とそれ以降のバージョンがシステムに固有の非表示フィールドを表示 (など、*隠し*ビューステート情報を格納するための要素) の内部*div* XHTML 標準に準拠するために要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-756">ASP.NET 2.0 and later versions render system-specific hidden fields (such as the *hidden* element used to store view state information) inside *div* element in order to comply with the XHTML standard.</span></span> <span data-ttu-id="c6c2f-757">ただし、この問題が発生 CSS 規則の影響を受ける場合*div*ページ上の要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-757">However, this can cause a problem when a CSS rule affects *div* elements on a page.</span></span> <span data-ttu-id="c6c2f-758">たとえば、発生するページに表示される 1 ピクセル行非表示には約*div*要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-758">For example, it can result in a one-pixel line appearing in the page around hidden *div* elements.</span></span> <span data-ttu-id="c6c2f-759">ASP.NET 4 で*div* ASP.NET によって生成された非表示フィールドを囲む要素は、次の例のように CSS クラスの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-759">In ASP.NET 4, *div* elements that enclose the hidden fields generated by ASP.NET add a CSS class reference as in the following example:</span></span>

[!code-html[Main](overview/samples/sample74.html)]

<span data-ttu-id="c6c2f-760">のみ適用される CSS クラスを定義し、*隠し*次の例のように、ASP.NET によって生成される要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-760">You can then define a CSS class that applies only to the *hidden* elements that are generated by ASP.NET, as in the following example:</span></span>

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a><span data-ttu-id="c6c2f-761">テンプレート コントロールの外部テーブルを表示</span><span class="sxs-lookup"><span data-stu-id="c6c2f-761">Rendering an Outer Table for Templated Controls</span></span>

<span data-ttu-id="c6c2f-762">既定では、テンプレートをサポートする次の ASP.NET Web サーバー コントロールは自動的にインライン スタイルを適用するために使用する外部テーブルでラップされます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-762">By default, the following ASP.NET Web server controls that support templates are automatically wrapped in an outer table that is used to apply inline styles:</span></span>

- <span data-ttu-id="c6c2f-763">*FormView*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-763">*FormView*</span></span>
- <span data-ttu-id="c6c2f-764">*ログイン*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-764">*Login*</span></span>
- <span data-ttu-id="c6c2f-765">*PasswordRecovery*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-765">*PasswordRecovery*</span></span>
- <span data-ttu-id="c6c2f-766">*ChangePassword*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-766">*ChangePassword*</span></span>
- <span data-ttu-id="c6c2f-767">*ウィザード*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-767">*Wizard*</span></span>
- <span data-ttu-id="c6c2f-768">*CreateUserWizard*</span><span class="sxs-lookup"><span data-stu-id="c6c2f-768">*CreateUserWizard*</span></span>

<span data-ttu-id="c6c2f-769">という名前の新しいプロパティ*RenderOuterTable*マークアップから削除する外部テーブルは、これらのコントロールに追加されました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-769">A new property named *RenderOuterTable* has been added to these controls that allows the outer table to be removed from the markup.</span></span> <span data-ttu-id="c6c2f-770">たとえば、次の例の*FormView*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-770">For example, consider the following example of a *FormView* control:</span></span>

[!code-aspx[Main](overview/samples/sample76.aspx)]

<span data-ttu-id="c6c2f-771">このマークアップでは、次の出力を HTML テーブルを含むページに表示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-771">This markup renders the following output to the page, which includes an HTML table:</span></span>

[!code-html[Main](overview/samples/sample77.html)]

<span data-ttu-id="c6c2f-772">テーブルが表示されないようにするには設定、 *FormView*コントロールの*RenderOuterTable*例を次のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-772">To prevent the table from being rendered, you can set the *FormView* control's *RenderOuterTable* property, as in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample78.aspx)]

<span data-ttu-id="c6c2f-773">前の例は、次の出力、なしを表示、*テーブル*、 *tr*、および*td*要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-773">The previous example renders the following output, without the *table*, *tr*, and *td* elements:</span></span>

> <span data-ttu-id="c6c2f-774">Content</span><span class="sxs-lookup"><span data-stu-id="c6c2f-774">Content</span></span>


<span data-ttu-id="c6c2f-775">この機能強化やすくスタイルの css でコントロールのコンテンツにため、コントロールによって予期しないタグは表示されません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-775">This enhancement can make it easier to style the content of the control with CSS, because no unexpected tags are rendered by the control.</span></span>

> [!NOTE]
> <span data-ttu-id="c6c2f-776">注この変更が不要になったために、Visual Studio 2010 デザイナーで自動的に書式設定関数のサポートを無効にする、*テーブル*要素を自動的に書式設定オプションによって生成されたスタイル属性をホストできます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-776">Note This change disables support for the auto-format function in the Visual Studio 2010 designer, because there is no longer a *table* element that can host style attributes that are generated by the auto-format option.</span></span>


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a><span data-ttu-id="c6c2f-777">ListView コントロールの機能強化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-777">ListView Control Enhancements</span></span>

<span data-ttu-id="c6c2f-778">*ListView*コントロールを ASP.NET 4 でより使いやすくなりました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-778">The *ListView* control has been made easier to use in ASP.NET 4.</span></span> <span data-ttu-id="c6c2f-779">コントロールの以前のバージョンの既知の ID を持つサーバー コントロールに含まれるレイアウト テンプレートを指定することが必要な</span><span class="sxs-lookup"><span data-stu-id="c6c2f-779">The earlier version of the control required that you specify a layout template that contained a server control with a known ID.</span></span> <span data-ttu-id="c6c2f-780">次のマークアップを使用する方法の一般的な例を示しています、 *ListView* ASP.NET 3.5 で制御します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-780">The following markup shows a typical example of how to use the *ListView* control in ASP.NET 3.5.</span></span>

[!code-aspx[Main](overview/samples/sample79.aspx)]

<span data-ttu-id="c6c2f-781">ASP.NET 4 で、 *ListView*コントロールでは、レイアウト テンプレートは必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-781">In ASP.NET 4, the *ListView* control does not require a layout template.</span></span> <span data-ttu-id="c6c2f-782">前の例に示すようにマークアップは、次のマークアップで置換できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-782">The markup shown in the previous example can be replaced with the following markup:</span></span>

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a><span data-ttu-id="c6c2f-783">CheckBoxList と RadioButtonList コントロールの機能強化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-783">CheckBoxList and RadioButtonList Control Enhancements</span></span>

<span data-ttu-id="c6c2f-784">ASP.NET 3.5 では、レイアウトを指定することができます、 *CheckBoxList*と*RadioButtonList*次の 2 つの設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-784">In ASP.NET 3.5, you can specify layout for the *CheckBoxList* and *RadioButtonList* using the following two settings:</span></span>

- <span data-ttu-id="c6c2f-785">*フロー*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-785">*Flow*.</span></span> <span data-ttu-id="c6c2f-786">コントロールをレンダリング*span*そのコンテンツを格納する要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-786">The control renders *span* elements to contain its content.</span></span>
- <span data-ttu-id="c6c2f-787">*テーブル*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-787">*Table*.</span></span> <span data-ttu-id="c6c2f-788">コントロールを表示、*テーブル*要素をそのコンテンツが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-788">The control renders a *table* element to contain its content.</span></span>

<span data-ttu-id="c6c2f-789">次の例は、これらのコントロールのマークアップを示しています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-789">The following example shows markup for each of these controls.</span></span>

[!code-aspx[Main](overview/samples/sample81.aspx)]

<span data-ttu-id="c6c2f-790">既定では、コントロールをレンダリング HTML 次のようにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-790">By default, the controls render HTML similar to the following:</span></span>

[!code-html[Main](overview/samples/sample82.html)]

<span data-ttu-id="c6c2f-791">これらのコントロールには、意味的に正しい HTML を表示するために、項目の一覧が含まれているため、前に、HTML の一覧を使用してその内容を表示する必要があります (*li*) 要素です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-791">Because these controls contain lists of items, to render semantically correct HTML, they should render their contents using HTML list (*li*) elements.</span></span> <span data-ttu-id="c6c2f-792">これは、やすく補助的な技術を使用して Web ページを読み取るし、コントロールを簡単に CSS を使用してスタイルをユーザー用です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-792">This makes it easier for users who read Web pages using assistive technology, and makes the controls easier to style using CSS.</span></span>

<span data-ttu-id="c6c2f-793">ASP.NET 4 で、 *CheckBoxList*と*RadioButtonList*コントロールに次の新しい値をサポートする、 *RepeatLayout*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-793">In ASP.NET 4, the *CheckBoxList* and *RadioButtonList* controls support the following new values for the *RepeatLayout* property:</span></span>

- <span data-ttu-id="c6c2f-794">*OrderedList* – として、コンテンツが表示される*li*内の要素、 *ol*要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-794">*OrderedList* – The content is rendered as *li* elements within an *ol* element.</span></span>
- <span data-ttu-id="c6c2f-795">*います*– として、コンテンツが表示される*li*内の要素、 *ul*要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-795">*UnorderedList* – The content is rendered as *li* elements within a *ul* element.</span></span>

<span data-ttu-id="c6c2f-796">次の例では、これらの新しい値を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-796">The following example shows how to use these new values.</span></span>

[!code-aspx[Main](overview/samples/sample83.aspx)]

<span data-ttu-id="c6c2f-797">上記のマークアップは、次の HTML を生成します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-797">The preceding markup generates the following HTML:</span></span>

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> <span data-ttu-id="c6c2f-798">設定するかどうかに注意してください*RepeatLayout*に*OrderedList*または*しています*、 *RepeatDirection*プロパティが使用できなくでき、プロパティが、マークアップやコード内で設定されている場合は、実行時に例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-798">Note If you set *RepeatLayout* to *OrderedList* or *UnorderedList*, the *RepeatDirection* property can no longer be used and will throw an exception at run time if the property has been set within your markup or code.</span></span> <span data-ttu-id="c6c2f-799">プロパティはありません値これらのコントロールのビジュアルなレイアウトでは、代わりに CSS を使用して定義されているためです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-799">The property would have no value because the visual layout of these controls is defined using CSS instead.</span></span>


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a><span data-ttu-id="c6c2f-800">メニュー コントロールの機能強化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-800">Menu Control Improvements</span></span>

<span data-ttu-id="c6c2f-801">ASP.NET 4 の前に、*メニュー*コントロールは、一連の HTML テーブルを表示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-801">Before ASP.NET 4, the *Menu* control rendered a series of HTML tables.</span></span> <span data-ttu-id="c6c2f-802">これにより、インラインのプロパティの設定の外部で CSS スタイルを適用するより困難になることと、ユーザー補助の標準に準拠しているでしたも。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-802">This made it more difficult to apply CSS styles outside of setting inline properties and was also not compliant with accessibility standards.</span></span>

<span data-ttu-id="c6c2f-803">ASP.NET 4 で、コントロールはこれで順序なしリストとリストの要素で構成されるセマンティック マークアップを使用して HTML をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-803">In ASP.NET 4, the control now renders HTML using semantic markup that consists of an unordered list and list elements.</span></span> <span data-ttu-id="c6c2f-804">次の例は、ASP.NET ページ用のマークアップを示しています、*メニュー*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-804">The following example shows markup in an ASP.NET page for the *Menu* control.</span></span>

[!code-aspx[Main](overview/samples/sample85.aspx)]

<span data-ttu-id="c6c2f-805">コントロールが次の HTML を生成するページが表示される、(、 *onclick*コードがわかりやすくするため省略されました)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-805">When the page renders, the control produces the following HTML (the *onclick* code has been omitted for clarity):</span></span>

[!code-html[Main](overview/samples/sample86.html)]

<span data-ttu-id="c6c2f-806">レンダリング機能強化に加え、メニューのキーボード ナビゲーションが改善されましたフォーカス管理を使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-806">In addition to rendering improvements, keyboard navigation of the menu has been improved using focus management.</span></span> <span data-ttu-id="c6c2f-807">ときに、*メニュー*コントロールがフォーカスを取得、方向キーを使用して要素を移動することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-807">When the *Menu* control gets focus, you can use the arrow keys to navigate elements.</span></span> <span data-ttu-id="c6c2f-808">*メニュー*コントロールはまた、アクセス可能なリッチ インターネット アプリケーション (ARIA) ロールを添付し、次の属性[翼、](http://www.w3.org/TR/wai-aria-practices/#menu "メニュー ARIA ガイドライン")改善ユーザー補助機能します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-808">The *Menu* control also now attaches accessible rich internet applications (ARIA) roles and attributes follo[wing the](http://www.w3.org/TR/wai-aria-practices/#menu "Menu ARIA guidelines")for improved accessibility.</span></span>

<span data-ttu-id="c6c2f-809">上部にあるスタイル ブロックに沿ってレンダリングされる HTML 要素ではなく、ページのでは、メニュー コントロールのスタイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-809">Styles for the menu control are rendered in a style block at the top of the page, rather than in line with the rendered HTML elements.</span></span> <span data-ttu-id="c6c2f-810">コントロールのスタイル設定を完全に制御を実行する場合は、設定できます、新しい*IncludeStyleBlock*プロパティを*false*、後者スタイル ブロックは出力されません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-810">If you want to take full control over the styling for the control, you can set the new *IncludeStyleBlock* property to *false*, in which case the style block is not emitted.</span></span> <span data-ttu-id="c6c2f-811">このプロパティを使用する方法の 1 つでは、機能を使用する、自動的に書式設定、Visual Studio デザイナーで、メニューの外観を設定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-811">One way to use this property is to use the auto-format feature in the Visual Studio designer to set the appearance of the menu.</span></span> <span data-ttu-id="c6c2f-812">ことができますし、ページの実行、ページ ソースを開くし、コピー、レンダリングされたスタイル ブロック外部 CSS ファイルにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-812">You can then run the page, open the page source, and then copy the rendered style block to an external CSS file.</span></span> <span data-ttu-id="c6c2f-813">Visual Studio で、スタイル設定および設定を元に戻す*IncludeStyleBlock*に*false*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-813">In Visual Studio, undo the styling and set *IncludeStyleBlock* to *false*.</span></span> <span data-ttu-id="c6c2f-814">外部スタイル シートのスタイルを使用するメニューの外観を定義することになります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-814">The result is that the menu appearance is defined using styles in an external style sheet.</span></span>

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a><span data-ttu-id="c6c2f-815">ウィザードおよび CreateUserWizard コントロール</span><span class="sxs-lookup"><span data-stu-id="c6c2f-815">Wizard and CreateUserWizard Controls</span></span>

<span data-ttu-id="c6c2f-816">ASP.NET*ウィザード*と*CreateUserWizard*コントロールが HTML が表示されるかを定義できるようにするテンプレートをサポートします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-816">The ASP.NET *Wizard* and *CreateUserWizard* controls support templates that let you define the HTML that they render.</span></span> <span data-ttu-id="c6c2f-817">(*CreateUserWizard*から派生した*ウィザード*)。次の例は、完全にテンプレートのマークアップを示しています。 *CreateUserWizard*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-817">(*CreateUserWizard* derives from *Wizard*.) The following example shows the markup for a fully templated *CreateUserWizard* control:</span></span>

[!code-aspx[Main](overview/samples/sample87.aspx)]

<span data-ttu-id="c6c2f-818">コントロールは、次のような HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-818">The control renders HTML similar to the following:</span></span>

[!code-html[Main](overview/samples/sample88.html)]

<span data-ttu-id="c6c2f-819">ASP.NET 3.5 sp1 では、テンプレートの内容を変更できますが、まだ制限されているの出力を制御、*ウィザード*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-819">In ASP.NET 3.5 SP1, although you can change the template contents, you still have limited control over the output of the *Wizard* control.</span></span> <span data-ttu-id="c6c2f-820">ASP.NET 4 で作成することができます、 *LayoutTemplate*テンプレートおよび insert*プレース ホルダー* (予約済みの名前を使用) を制御する方法を指定する、*ウィザード コントロール*をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-820">In ASP.NET 4, you can create a *LayoutTemplate* template and insert *PlaceHolder* controls (using reserved names) to specify how you want the *Wizard control* to render.</span></span> <span data-ttu-id="c6c2f-821">次の例では、これは示しています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-821">The following example shows this:</span></span>

[!code-aspx[Main](overview/samples/sample89.aspx)]

<span data-ttu-id="c6c2f-822">例では、次名前付きのプレース ホルダーにはが含まれています、 *LayoutTemplate*要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-822">The example contains the following named placeholders in the *LayoutTemplate* element:</span></span>

- <span data-ttu-id="c6c2f-823">*headerPlaceholder* – の内容で置き換えられます、実行時に、 *HeaderTemplate*要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-823">*headerPlaceholder* – At run time, this is replaced by the contents of the *HeaderTemplate* element.</span></span>
- <span data-ttu-id="c6c2f-824">*sideBarPlaceholder* – の内容で置き換えられます、実行時に、 *SideBarTemplate*要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-824">*sideBarPlaceholder* – At run time, this is replaced by the contents of the *SideBarTemplate* element.</span></span>
- <span data-ttu-id="c6c2f-825">*wizardStepPlaceHolder* – の内容で置き換えられます、実行時に、 *WizardStepTemplate*要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-825">*wizardStepPlaceHolder* – At run time, this is replaced by the contents of the *WizardStepTemplate* element.</span></span>
- <span data-ttu-id="c6c2f-826">*navigationPlaceholder* –、実行時に定義されているナビゲーション テンプレートで置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-826">*navigationPlaceholder* – At run time, this is replaced by any navigation templates that you have defined.</span></span>

<span data-ttu-id="c6c2f-827">プレース ホルダーを使用しての例では、マークアップでは、次の HTML を表示します (実際には、テンプレートで定義されているコンテンツ) なし。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-827">The markup in the example that uses placeholders renders the following HTML (without the content actually defined in the templates):</span></span>

[!code-html[Main](overview/samples/sample90.html)]

<span data-ttu-id="c6c2f-828">ここで定義されていないユーザーの唯一の HTML が、 *span*要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-828">The only HTML that is now not user-defined is a *span* element.</span></span> <span data-ttu-id="c6c2f-829">(後のリリースでは、予想でも、 *span*要素はレンダリングされません)。今すぐ完全に制御によって生成されるほとんどすべてのコンテンツを*ウィザード*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-829">(We anticipate that in future releases, even the *span* element will not be rendered.) This now gives you full control over virtually all the content that is generated by the *Wizard* control.</span></span>

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a><span data-ttu-id="c6c2f-830">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c6c2f-830">ASP.NET MVC</span></span>

<span data-ttu-id="c6c2f-831">ASP.NET MVC は、2009 年 3 月で、アドオン フレームワークとして、ASP.NET 3.5 SP1 に導入されました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-831">ASP.NET MVC was introduced as an add-on framework to ASP.NET 3.5 SP1 in March 2009.</span></span> <span data-ttu-id="c6c2f-832">Visual Studio 2010 には、ASP.NET MVC 2 には、新機能と機能が含まれていますが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-832">Visual Studio 2010 includes ASP.NET MVC 2, which includes new features and capabilities.</span></span>

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a><span data-ttu-id="c6c2f-833">領域のサポート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-833">Areas Support</span></span>

<span data-ttu-id="c6c2f-834">領域使用するグループのコント ローラーとビューを他のセクションからの相対的な分離で大規模なアプリケーションのセクションにできます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-834">Areas let you group controllers and views into sections of a large application in relative isolation from other sections.</span></span> <span data-ttu-id="c6c2f-835">各領域は、メイン アプリケーションによって、参照できる個別の ASP.NET MVC プロジェクトとして実装できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-835">Each area can be implemented as a separate ASP.NET MVC project that can then be referenced by the main application.</span></span> <span data-ttu-id="c6c2f-836">これにより、大規模なアプリケーションをビルドすると複雑性の管理を支援し、1 つのアプリケーションで連携して動作する複数のチームで簡単になります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-836">This helps manage complexity when you build a large application and makes it easier for multiple teams to work together on a single application.</span></span>

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a><span data-ttu-id="c6c2f-837">データ注釈属性の検証のサポート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-837">Data-Annotation Attribute Validation Support</span></span>

<span data-ttu-id="c6c2f-838">*DataAnnotations*属性を使用して、メタデータ属性を使用して、モデルに検証ロジックを適用できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-838">*DataAnnotations* attributes let you attach validation logic to a model by using metadata attributes.</span></span> <span data-ttu-id="c6c2f-839">*DataAnnotations*属性は ASP.NET 3.5 SP1 での ASP.NET 動的データで導入されました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-839">*DataAnnotations* attributes were introduced in ASP.NET Dynamic Data in ASP.NET 3.5 SP1.</span></span> <span data-ttu-id="c6c2f-840">これらの属性は、既定のモデル バインダーに統合されており、ユーザー入力を検証するメタデータ ドリブン手段を提供します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-840">These attributes have been integrated into the default model binder and provide a metadata-driven means to validate user input.</span></span>

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a><span data-ttu-id="c6c2f-841">テンプレート化されたヘルパー</span><span class="sxs-lookup"><span data-stu-id="c6c2f-841">Templated Helpers</span></span>

<span data-ttu-id="c6c2f-842">テンプレート化されたヘルパーは、自動的に関連付ける編集ができ、データ型を持つテンプレートを表示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-842">Templated helpers let you automatically associate edit and display templates with data types.</span></span> <span data-ttu-id="c6c2f-843">日付選択カレンダー UI 要素が自動的に表示されることを指定するテンプレート ヘルパーを使用するなど、 *System.DateTime*値。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-843">For example, you can use a template helper to specify that a date-picker UI element is automatically rendered for a *System.DateTime* value.</span></span> <span data-ttu-id="c6c2f-844">ASP.NET 動的データのフィールド テンプレートに似ています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-844">This is similar to field templates in ASP.NET Dynamic Data.</span></span>

<span data-ttu-id="c6c2f-845">*Html.EditorFor*と*Html.DisplayFor*ヘルパー メソッドには組み込みサポートがされている型の複数のプロパティと同様に複雑なオブジェクトの標準的なデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-845">The *Html.EditorFor* and *Html.DisplayFor* helper methods have built-in support for rendering standard data types as well as complex objects with multiple properties.</span></span> <span data-ttu-id="c6c2f-846">などのデータ注釈属性を適用することによってレンダリングをカスタマイズするも*DisplayName*と*ScaffoldColumn*を*ViewModel*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-846">They also customize rendering by letting you apply data-annotation attributes like *DisplayName* and *ScaffoldColumn* to the *ViewModel* object.</span></span>

<span data-ttu-id="c6c2f-847">多くの場合、さらに UI ヘルパーからの出力をカスタマイズして、生成されたものを完全に制御します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-847">Often you want to customize the output from UI helpers even further and have complete control over what is generated.</span></span> <span data-ttu-id="c6c2f-848">*Html.EditorFor*と*Html.DisplayFor*ヘルパー メソッドをサポートしてこれをオーバーライドできます外部のテンプレートと表示される出力のコントロールを定義できるテンプレート メカニズムを使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-848">The *Html.EditorFor* and *Html.DisplayFor* helper methods support this using a templating mechanism that lets you define external templates that can override and control the output rendered.</span></span> <span data-ttu-id="c6c2f-849">テンプレートは、クラスに対して個別にレンダリングできます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-849">The templates can be rendered individually for a class.</span></span>

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a><span data-ttu-id="c6c2f-850">動的データ</span><span class="sxs-lookup"><span data-stu-id="c6c2f-850">Dynamic Data</span></span>

<span data-ttu-id="c6c2f-851">動的なデータが導入された 2008 中旬で .NET Framework 3.5 SP1 のリリースでします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-851">Dynamic Data was introduced in the .NET Framework 3.5 SP1 release in mid-2008.</span></span> <span data-ttu-id="c6c2f-852">この機能は、次を含む、データ駆動型アプリケーションを作成するための多くの機能強化を提供します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-852">This feature provides many enhancements for creating data-driven applications, including the following:</span></span>

- <span data-ttu-id="c6c2f-853">データ ドリブン Web サイトの構築をすばやくの RAD エクスペリエンス。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-853">A RAD experience for quickly building a data-driven Web site.</span></span>
- <span data-ttu-id="c6c2f-854">データ モデルで定義された制約に基づく自動検証。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-854">Automatic validation that is based on constraints defined in the data model.</span></span>
- <span data-ttu-id="c6c2f-855">内のフィールドに対して生成されるマークアップを簡単に変更すること、 *GridView*と*DetailsView*動的なデータ プロジェクトの一部であるフィールド テンプレートを使用して制御します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-855">The ability to easily change the markup that is generated for fields in the *GridView* and *DetailsView* controls by using field templates that are part of your Dynamic Data project.</span></span>

> [!NOTE]
> <span data-ttu-id="c6c2f-856">注の詳細についてを参照してください、[動的データ ドキュメント](https://msdn.microsoft.com/library/cc488545.aspx)MSDN ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-856">Note For more information, see the [Dynamic Data documentation](https://msdn.microsoft.com/library/cc488545.aspx) in the MSDN Library.</span></span>


<span data-ttu-id="c6c2f-857">ASP.NET 4 では、データ ドリブン Web サイトをすばやく構築するためのさらに多くの電源を開発者に提供する動的なデータが強化されました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-857">For ASP.NET 4, Dynamic Data has been enhanced to give developers even more power for quickly building data-driven Web sites.</span></span>

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a><span data-ttu-id="c6c2f-858">既存のプロジェクトの動的なデータの有効化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-858">Enabling Dynamic Data for Existing Projects</span></span>

<span data-ttu-id="c6c2f-859">.NET Framework 3.5 SP1 に付属している動的データ機能では、新機能は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-859">Dynamic Data features that shipped in the .NET Framework 3.5 SP1 brought new features such as the following:</span></span>

- <span data-ttu-id="c6c2f-860">フィールド テンプレート – これらテンプレートを使用できるデータ型に基づくデータ バインド コントロールのです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-860">Field templates – These provide data-type-based templates for data-bound controls.</span></span> <span data-ttu-id="c6c2f-861">フィールド テンプレートは、各フィールドのテンプレートのフィールドを使用するよりもデータ コントロールの外観をカスタマイズする簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-861">Field templates provide a simpler way to customize the look of data controls than using template fields for each field.</span></span>
- <span data-ttu-id="c6c2f-862">検証: 動的なデータをで属性クラスを使用してデータを必要なフィールド、範囲のチェック、型チェック、正規表現を使用して一致するパターンなどの一般的なシナリオの検証とカスタム検証を指定できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-862">Validation – Dynamic Data lets you use attributes on data classes to specify validation for common scenarios like required fields, range checking, type checking, pattern matching using regular expressions, and custom validation.</span></span> <span data-ttu-id="c6c2f-863">検証は、データ コントロールによって強制されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-863">Validation is enforced by the data controls.</span></span>

<span data-ttu-id="c6c2f-864">ただし、これらの機能には、次の要件が必要があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-864">However, these features had the following requirements:</span></span>

- <span data-ttu-id="c6c2f-865">データ アクセス層は、Entity Framework または LINQ to SQL に基づいている必要があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-865">The data-access layer had to be based on Entity Framework or LINQ to SQL.</span></span>
- <span data-ttu-id="c6c2f-866">唯一のデータ ソースのこれらの機能がサポートされているコントロール、 *EntityDataSource*または*LinqDataSource*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-866">The only data source controls supported for these features were the *EntityDataSource* or *LinqDataSource* controls.</span></span>
- <span data-ttu-id="c6c2f-867">機能には、機能をサポートする必要があるすべてのファイルがあるために、動的なデータまたは動的データ エンティティ テンプレートを使用して作成した Web プロジェクトが必要です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-867">The features required a Web project that had been created using the Dynamic Data or Dynamic Data Entities templates in order to have all the files that were required to support the feature.</span></span>

<span data-ttu-id="c6c2f-868">ASP.NET 4 での動的データ サポートの主な目標は、ASP.NET アプリケーションの動的なデータの新しい機能を有効にするのにです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-868">A major goal of Dynamic Data support in ASP.NET 4 is to enable the new functionality of Dynamic Data for any ASP.NET application.</span></span> <span data-ttu-id="c6c2f-869">次の例は、利用できる動的なデータ機能の既存のページにコントロールのマークアップを示しています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-869">The following example shows markup for controls that can take advantage of Dynamic Data functionality in an existing page.</span></span>

[!code-aspx[Main](overview/samples/sample91.aspx)]

<span data-ttu-id="c6c2f-870">ページのコードでは、これらのコントロールの動的データ サポートを有効にするために、次のコードを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-870">In the code for the page, the following code must be added in order to enable Dynamic Data support for these controls:</span></span>

[!code-csharp[Main](overview/samples/sample92.cs)]

<span data-ttu-id="c6c2f-871">ときに、 *GridView*コントロールが編集モードでは、動的なデータが自動的に適切な形式で入力したデータが検証されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-871">When the *GridView* control is in edit mode, Dynamic Data automatically validates that the data entered is in the proper format.</span></span> <span data-ttu-id="c6c2f-872">そうでない場合は、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-872">If it is not, an error message is displayed.</span></span>

<span data-ttu-id="c6c2f-873">この機能は、その他のメリットも用意されています。、値を挿入モードの既定値を指定することなどです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-873">This functionality also provides other benefits, such as being able to specify default values for insert mode.</span></span> <span data-ttu-id="c6c2f-874">動的なデータがない場合、フィールドの既定値を実装する必要がありますイベントにアタッチ、コントロールの検索 (を使用して*FindControl*)、およびその値を設定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-874">Without Dynamic Data, to implement a default value for a field, you must attach to an event, locate the control (using *FindControl*), and set its value.</span></span> <span data-ttu-id="c6c2f-875">ASP.NET 4 で、 *EnableDynamicData*呼び出しは、この例で示すように、オブジェクトのいずれかのフィールドの既定値を渡すことができますを 2 番目のパラメーターをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-875">In ASP.NET 4, the *EnableDynamicData* call supports a second parameter that lets you pass default values for any field on the object, as shown in this example:</span></span>

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a><span data-ttu-id="c6c2f-876">DynamicDataManager コントロールを宣言の構文</span><span class="sxs-lookup"><span data-stu-id="c6c2f-876">Declarative DynamicDataManager Control Syntax</span></span>

<span data-ttu-id="c6c2f-877">*DynamicDataManager*宣言によって、構成できるようにするコントロールが強化されましたのみでコードの代わりに、ASP.NET では、ほとんどのコントロールと同様です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-877">The *DynamicDataManager* control has been enhanced so that you can configure it declaratively, as with most controls in ASP.NET, instead of only in code.</span></span> <span data-ttu-id="c6c2f-878">マークアップ、 *DynamicDataManager*コントロールは次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-878">The markup for the *DynamicDataManager* control looks like the following example:</span></span>

[!code-aspx[Main](overview/samples/sample94.aspx)]

<span data-ttu-id="c6c2f-879">このマークアップで参照されている GridView1 コントロールの動的なデータの動作を有効に、 *DataControls*のセクションで、 *DynamicDataManager*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-879">This markup enables Dynamic Data behavior for the GridView1 control that is referenced in the *DataControls* section of the *DynamicDataManager* control.</span></span>

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a><span data-ttu-id="c6c2f-880">エンティティのテンプレート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-880">Entity Templates</span></span>

<span data-ttu-id="c6c2f-881">エンティティのテンプレートでは、カスタム ページを作成することがなくデータのレイアウトをカスタマイズする新しい方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-881">Entity templates offer a new way to customize the layout of data without requiring you to create a custom page.</span></span> <span data-ttu-id="c6c2f-882">ページのテンプレートを使用して、 *FormView*コントロール (の代わりに、 *DetailsView*コントロールが以前のバージョンの動的なデータ ページ テンプレートで使用)、および*DynamicEntity*エンティティのテンプレートをレンダリングするコントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-882">Page templates use the *FormView* control (instead of the *DetailsView* control, as used in page templates in earlier versions of Dynamic Data) and the *DynamicEntity* control to render Entity templates.</span></span> <span data-ttu-id="c6c2f-883">動的なデータを表示するマークアップをより詳細に制御できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-883">This gives you more control over the markup that is rendered by Dynamic Data.</span></span>

<span data-ttu-id="c6c2f-884">次に、エンティティ テンプレートを含む新しいプロジェクト ディレクトリ レイアウトを示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-884">The following list shows the new project directory layout that contains entity templates:</span></span>

[!code-console[Main](overview/samples/sample95.cmd)]

<span data-ttu-id="c6c2f-885">`EntityTemplate`ディレクトリには、データ モデル オブジェクトを表示する方法のテンプレートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-885">The `EntityTemplate` directory contains templates for how to display data model objects.</span></span> <span data-ttu-id="c6c2f-886">使用して既定では、オブジェクトがレンダリングされる、`Default.ascx`によって作成されたマークアップと同じように検索するマークアップを提供するテンプレート、 *DetailsView* ASP.NET 3.5 SP1 での動的なデータによって使用されるコントロールです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-886">By default, objects are rendered by using the `Default.ascx` template, which provides markup that looks just like the markup created by the *DetailsView* control used by Dynamic Data in ASP.NET 3.5 SP1.</span></span> <span data-ttu-id="c6c2f-887">次の例では、マークアップを`Default.ascx`コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-887">The following example shows the markup for the `Default.ascx` control:</span></span>

[!code-aspx[Main](overview/samples/sample96.aspx)]

<span data-ttu-id="c6c2f-888">サイト全体の外観を変更するのには、既定のテンプレートを編集できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-888">The default templates can be edited to change the look and feel for the entire site.</span></span> <span data-ttu-id="c6c2f-889">表示、編集、および挿入操作用のテンプレートがあります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-889">There are templates for display, edit, and insert operations.</span></span> <span data-ttu-id="c6c2f-890">新しいテンプレート追加できますデータ オブジェクトの名前に基づいてオブジェクトの種類を 1 つだけの外観を変更するためにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-890">New templates can be added based on the name of the data object in order to change the look and feel of just one type of object.</span></span> <span data-ttu-id="c6c2f-891">たとえば、次のテンプレートを追加できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-891">For example, you can add the following template:</span></span>

[!code-console[Main](overview/samples/sample97.cmd)]

<span data-ttu-id="c6c2f-892">テンプレートには、次のマークアップが含まれます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-892">The template might contain the following markup:</span></span>

[!code-aspx[Main](overview/samples/sample98.aspx)]

<span data-ttu-id="c6c2f-893">新しいエンティティ テンプレートは、新しいを使用して、ページに表示される*DynamicEntity*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-893">The new entity templates are displayed on a page by using the new *DynamicEntity* control.</span></span> <span data-ttu-id="c6c2f-894">実行時に、このコントロールは、エンティティ テンプレートの内容に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-894">At run time, this control is replaced with the contents of the entity template.</span></span> <span data-ttu-id="c6c2f-895">次のマークアップに示します、 *FormView*内の制御、`Detail.aspx`エンティティ テンプレートを使用するページのテンプレートです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-895">The following markup shows the *FormView* control in the `Detail.aspx` page template that uses the entity template.</span></span> <span data-ttu-id="c6c2f-896">通知、 *DynamicEntity*マークアップ内の要素。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-896">Notice the *DynamicEntity* element in the markup.</span></span>

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a><span data-ttu-id="c6c2f-897">Url および電子メール アドレスの新しいフィールド テンプレート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-897">New Field Templates for URLs and Email Addresses</span></span>

<span data-ttu-id="c6c2f-898">ASP.NET 4 では、次の 2 つの新しい組み込みフィールド テンプレート`EmailAddress.ascx`と`Url.ascx`です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-898">ASP.NET 4 introduces two new built-in field templates, `EmailAddress.ascx` and `Url.ascx`.</span></span> <span data-ttu-id="c6c2f-899">これらのテンプレートとしてマークされているフィールドの使用は*EmailAddress*または*Url*で、 *DataType*属性。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-899">These templates are used for fields that are marked as *EmailAddress* or *Url* with the *DataType* attribute.</span></span> <span data-ttu-id="c6c2f-900">*EmailAddress*オブジェクトを使用して作成されるハイパーリンクとして、フィールドが表示されます、 *mailto:* プロトコルです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-900">For *EmailAddress* objects, the field is displayed as a hyperlink that is created by using the *mailto:* protocol.</span></span> <span data-ttu-id="c6c2f-901">ユーザーは、リンクをクリックして、それとが開き、ユーザーの電子メール クライアント スケルトンのメッセージを作成します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-901">When users click the link, it opens the user's email client and creates a skeleton message.</span></span> <span data-ttu-id="c6c2f-902">オブジェクトとして型指定された*Url*は通常のハイパーリンクとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-902">Objects typed as *Url* are displayed as ordinary hyperlinks.</span></span>

<span data-ttu-id="c6c2f-903">次の例では、フィールドとマークする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-903">The following example shows how fields would be marked.</span></span>

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a><span data-ttu-id="c6c2f-904">DynamicHyperLink コントロールとのリンクを作成します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-904">Creating Links with the DynamicHyperLink Control</span></span>

<span data-ttu-id="c6c2f-905">動的なデータは、エンドユーザーが、Web サイトにアクセスするときに表示される Url を制御するには、.NET Framework 3.5 SP1 で追加された、新しいルーティングの機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-905">Dynamic Data uses the new routing feature that was added in the .NET Framework 3.5 SP1 to control the URLs that end users see when they access the Web site.</span></span> <span data-ttu-id="c6c2f-906">新しい*DynamicHyperLink*コントロールでは、簡単に動的データ サイト内のページへのリンクを作成できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-906">The new *DynamicHyperLink* control makes it easy to build links to pages in a Dynamic Data site.</span></span> <span data-ttu-id="c6c2f-907">次の例を使用する方法を示しています、 *DynamicHyperLink*コントロール。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-907">The following example shows how to use the *DynamicHyperLink* control:</span></span>

[!code-aspx[Main](overview/samples/sample101.aspx)]

<span data-ttu-id="c6c2f-908">このマークアップのリスト ページを参照するリンクの作成、`Products`テーブルで定義されているルートに基づいて、`Global.asax`ファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-908">This markup creates a link that points to the List page for the `Products` table based on routes that are defined in the `Global.asax` file.</span></span> <span data-ttu-id="c6c2f-909">コントロールでは、動的なデータ ページの基になっている既定のテーブル名が自動的に使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-909">The control automatically uses the default table name that the Dynamic Data page is based on.</span></span>

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a><span data-ttu-id="c6c2f-910">データ モデルにおける継承のサポート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-910">Support for Inheritance in the Data Model</span></span>

<span data-ttu-id="c6c2f-911">Entity Framework でも LINQ to SQL の両方のデータ モデルで継承をサポートします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-911">Both the Entity Framework and LINQ to SQL support inheritance in their data models.</span></span> <span data-ttu-id="c6c2f-912">この例を持つデータベースである可能性があります、`InsurancePolicy`テーブル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-912">An example of this might be a database that has an `InsurancePolicy` table.</span></span> <span data-ttu-id="c6c2f-913">含まれる場合も`CarPolicy`と`HousePolicy`と同じフィールドを持つテーブルを`InsurancePolicy`しより多くのフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-913">It might also contain `CarPolicy` and `HousePolicy` tables that have the same fields as `InsurancePolicy` and then add more fields.</span></span> <span data-ttu-id="c6c2f-914">データ モデルで継承されたオブジェクトを理解して、継承されたテーブルのスキャフォールディングをサポートするために、動的なデータが変更されました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-914">Dynamic Data has been modified to understand inherited objects in the data model and to support scaffolding for the inherited tables.</span></span>

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a><span data-ttu-id="c6c2f-915">多対多のリレーションシップ (Entity Framework) のサポート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-915">Support for Many-to-Many Relationships (Entity Framework Only)</span></span>

<span data-ttu-id="c6c2f-916">Entity Framework のコレクションとしてのリレーションシップを公開することで実装されているテーブル間の多対多リレーションシップの豊富なサポートでは、*エンティティ*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-916">The Entity Framework has rich support for many-to-many relationships between tables, which is implemented by exposing the relationship as a collection on an *Entity* object.</span></span> <span data-ttu-id="c6c2f-917">新しい`ManyToMany.ascx`と`ManyToMany_Edit.ascx`を表示して、多対多リレーションシップに関係するデータの編集をサポートするフィールド テンプレートが追加されました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-917">New `ManyToMany.ascx` and `ManyToMany_Edit.ascx` field templates have been added to provide support for displaying and editing data that is involved in many-to-many relationships.</span></span>

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a><span data-ttu-id="c6c2f-918">新しい属性をコントロールの表示と列挙のサポート</span><span class="sxs-lookup"><span data-stu-id="c6c2f-918">New Attributes to Control Display and Support Enumerations</span></span>

<span data-ttu-id="c6c2f-919">*DisplayAttribute*フィールドの表示方法を詳細に制御することが追加されました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-919">The *DisplayAttribute* has been added to give you additional control over how fields are displayed.</span></span> <span data-ttu-id="c6c2f-920">*DisplayName*以前のバージョンを使用するフィールドのキャプションとして使用される名前を変更する動的なデータの属性です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-920">The *DisplayName* attribute in earlier versions of Dynamic Data allowed you to change the name that is used as a caption for a field.</span></span> <span data-ttu-id="c6c2f-921">新しい*DisplayAttribute*クラスを使用して、順序、フィールドを表示およびフィールドをフィルターとして使用するかどうかなど、フィールドを表示するための詳細オプションを指定できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-921">The new *DisplayAttribute* class lets you specify more options for displaying a field, such as the order in which a field is displayed and whether a field will be used as a filter.</span></span> <span data-ttu-id="c6c2f-922">属性は、独立したコントロールのラベルに使用される名前のも用意されています、 *GridView*で使用される名前を制御する、 *DetailsView*のレベルのウォーターマークが使用されると、フィールドのヘルプ テキストを制御します。、。フィールド (フィールドは、テキスト入力を受け入れる) 場合です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-922">The attribute also provides independent control of the name used for the labels in a *GridView* control, the name used in a *DetailsView* control, the help text for the field, and the watermark used for the field (if the field accepts text input).</span></span>

<span data-ttu-id="c6c2f-923">*EnumDataTypeAttribute*列挙体にフィールドをマップできるクラスが追加されました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-923">The *EnumDataTypeAttribute* class has been added to let you map fields to enumerations.</span></span> <span data-ttu-id="c6c2f-924">フィールドにこの属性を適用する場合は、列挙型を指定します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-924">When you apply this attribute to a field, you specify an enumeration type.</span></span> <span data-ttu-id="c6c2f-925">動的なデータを使用して、新しい`Enumeration.ascx`フィールド テンプレートを表示および編集する列挙値の UI を作成します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-925">Dynamic Data uses the new `Enumeration.ascx` field template to create UI for displaying and editing enumeration values.</span></span> <span data-ttu-id="c6c2f-926">テンプレートは、列挙体の名前に、データベースから値をマップします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-926">The template maps the values from the database to the names in the enumeration.</span></span>

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a><span data-ttu-id="c6c2f-927">フィルターのサポートの強化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-927">Enhanced Support for Filters</span></span>

<span data-ttu-id="c6c2f-928">ブール型の列と外部キー列の組み込みフィルターを使用して動的データ 1.0 が付属しています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-928">Dynamic Data 1.0 shipped with built-in filters for Boolean columns and foreign-key columns.</span></span> <span data-ttu-id="c6c2f-929">フィルターでは、それらに表示されていたか、どのような順序では表示を指定不可能です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-929">The filters did not allow you to specify whether they were displayed or in what order they were displayed.</span></span> <span data-ttu-id="c6c2f-930">新しい*DisplayAttribute*属性のアドレスを両方の提供することでこれらの問題をフィルターとして列を表示するかどうかを制御し、どのような順序で表示されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-930">The new *DisplayAttribute* attribute addresses both of these issues by giving you control over whether a column is displayed as a filter and in what order it will be displayed.</span></span>

<span data-ttu-id="c6c2f-931">追加の機能強化がフィルター処理のサポートされていること[、new を使用して書き込む](#0.2__QueryExtender "_QueryExtender") Web フォームの機能。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-931">An additional enhancement is that filtering support has been re[written to use the new](#0.2__QueryExtender "_QueryExtender") feature of Web Forms.</span></span> <span data-ttu-id="c6c2f-932">これにより、フィルターで使用されるデータ ソース コントロールの知識がなくても、フィルターを作成できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-932">This lets you create filters without requiring knowledge of the data source control that the filters will be used with.</span></span> <span data-ttu-id="c6c2f-933">これらの拡張機能、およびフィルターもなっているコントロールのテンプレートに新しいものを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-933">Along with these extensions, filters have also been turned into template controls, which allows you to add new ones.</span></span> <span data-ttu-id="c6c2f-934">最後に、 *DisplayAttribute*述べたクラスが同じで、オーバーライドできる既定のフィルターを使用する方法*UIHint*オーバーライドされるように列の既定のフィールド テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-934">Finally, the *DisplayAttribute* class mentioned earlier allows the default filter to be overridden, in the same way that *UIHint* allows the default field template for a column to be overridden.</span></span>

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a><span data-ttu-id="c6c2f-935">Visual Studio 2010 Web 開発の改良</span><span class="sxs-lookup"><span data-stu-id="c6c2f-935">Visual Studio 2010 Web Development Improvements</span></span>

<span data-ttu-id="c6c2f-936">Visual Studio 2010 での web 開発は、互換性のため大きい CSS、HTML および ASP.NET のマークアップ スニペットと新しい動的 IntelliSense JavaScript による生産性の向上に拡張されています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-936">Web development in Visual Studio 2010 has been enhanced for greater CSS compatibility, increased productivity through HTML and ASP.NET markup snippets and new dynamic IntelliSense JavaScript.</span></span>

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a><span data-ttu-id="c6c2f-937">強化された CSS 互換性</span><span class="sxs-lookup"><span data-stu-id="c6c2f-937">Improved CSS Compatibility</span></span>

<span data-ttu-id="c6c2f-938">Visual Studio 2010 で Visual Web Developer デザイナーが CSS 2.1 基準のコンプライアンス対応を向上させるために更新されました。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-938">The Visual Web Developer designer in Visual Studio 2010 has been updated to improve CSS 2.1 standards compliance.</span></span> <span data-ttu-id="c6c2f-939">デザイナーは、HTML ソースの整合性を維持されは、Visual Studio の以前のバージョンよりも堅固です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-939">The designer better preserves integrity of the HTML source and is more robust than in previous versions of Visual Studio.</span></span> <span data-ttu-id="c6c2f-940">フードのアーキテクチャの改善によっても加えられたよりレンダリング、レイアウト、および保守で将来の機能強化を有効にすることになります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-940">Under the hood, architectural improvements have also been made that will better enable future enhancements in rendering, layout, and serviceability.</span></span>

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a><span data-ttu-id="c6c2f-941">HTML および JavaScript スニペット</span><span class="sxs-lookup"><span data-stu-id="c6c2f-941">HTML and JavaScript Snippets</span></span>

<span data-ttu-id="c6c2f-942">HTML エディターで IntelliSense をオートコンプリート タグ名。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-942">In the HTML editor, IntelliSense auto-completes tag names.</span></span> <span data-ttu-id="c6c2f-943">IntelliSense スニペット機能をオートコンプリート タグ全体などです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-943">The IntelliSense Snippets feature auto-completes entire tags and more.</span></span> <span data-ttu-id="c6c2f-944">Visual Studio 2010 では、IntelliSense スニペットは、javascript、c# および Visual Basic、Visual Studio の以前のバージョンでサポートされていたと共にサポートされます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-944">In Visual Studio 2010, IntelliSense snippets are supported for JavaScript, alongside C# and Visual Basic, which were supported in earlier versions of Visual Studio.</span></span>

<span data-ttu-id="c6c2f-945">Visual Studio 2010 には、オート コンプリート一般的な ASP.NET と HTML タグ、必須の属性を含む役立つ 200 を超えるのスニペットが含まれています (など runat ="server") と、タグに固有の共通の属性 (など*ID*、 *DataSourceID*、 *ControlToValidate*、および*テキスト*)。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-945">Visual Studio 2010 includes over 200 snippets that help you auto-complete common ASP.NET and HTML tags, including required attributes (such as runat="server") and common attributes specific to a tag (such as *ID*, *DataSourceID*, *ControlToValidate*, and *Text*).</span></span>

<span data-ttu-id="c6c2f-946">他のスニペットをダウンロードすることも自分やチームの一般的なタスクを使用するマークアップのブロックをカプセル化する、独自のスニペットを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-946">You can download additional snippets, or you can write your own snippets that encapsulate the blocks of markup that you or your team use for common tasks.</span></span>

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a><span data-ttu-id="c6c2f-947">JavaScript IntelliSense の機能強化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-947">JavaScript IntelliSense Enhancements</span></span>

<span data-ttu-id="c6c2f-948">Visual 2010 で JavaScript IntelliSense は再設計されましたより豊富な編集エクスペリエンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-948">In Visual 2010, JavaScript IntelliSense has been redesigned to provide an even richer editing experience.</span></span> <span data-ttu-id="c6c2f-949">IntelliSense が動的に生成されたメソッドでなどのオブジェクトを今すぐ認識*registerNamespace*他の JavaScript フレームワークで使用される同様の手法です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-949">IntelliSense now recognizes objects that have been dynamically generated by methods such as *registerNamespace* and by similar techniques used by other JavaScript frameworks.</span></span> <span data-ttu-id="c6c2f-950">スクリプトの大規模なライブラリを分析して、ほとんどまたはまったく処理の遅延で IntelliSense を表示するパフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-950">Performance has been improved to analyze large libraries of script and to display IntelliSense with little or no processing delay.</span></span> <span data-ttu-id="c6c2f-951">互換性が大幅に増加したほとんどすべてのサード パーティ製ライブラリをサポートし、多様なコーディング スタイルをサポートするためです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-951">Compatibility has been dramatically increased to support nearly all third-party libraries and to support diverse coding styles.</span></span> <span data-ttu-id="c6c2f-952">ドキュメント コメントは、入力して、IntelliSense によってすぐに活用できるように今すぐ解析されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-952">Documentation comments are now parsed as you type and are immediately leveraged by IntelliSense.</span></span>

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a><span data-ttu-id="c6c2f-953">Visual Studio 2010 で web アプリケーションの展開</span><span class="sxs-lookup"><span data-stu-id="c6c2f-953">Web Application Deployment with Visual Studio 2010</span></span>

<span data-ttu-id="c6c2f-954">ASP.NET 開発者は、Web アプリケーションを展開、多くの場合、検索するなど、次の問題に遭遇します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-954">When ASP.NET developers deploy a Web application, they often find that they encounter issues such as the following:</span></span>

- <span data-ttu-id="c6c2f-955">共有のホスト サイトに展開するには、低速であることができます、FTP などのテクノロジが必要です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-955">Deploying to a shared hosting site requires technologies such as FTP, which can be slow.</span></span> <span data-ttu-id="c6c2f-956">また、手動でデータベースを構成する SQL スクリプトを実行しているなどのタスクを実行する必要があり、アプリケーションとして、仮想ディレクトリ フォルダーを構成するなどの IIS 設定を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-956">In addition, you must manually perform tasks such as running SQL scripts to configure a database and you must change IIS settings, such as configuring a virtual directory folder as an application.</span></span>
- <span data-ttu-id="c6c2f-957">Web アプリケーション ファイルを展開するだけでなく、エンタープライズ環境で管理者頻繁にする必要があります変更 ASP.NET 構成ファイルおよび IIS の設定。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-957">In an enterprise environment, in addition to deploying the Web application files, administrators frequently must modify ASP.NET configuration files and IIS settings.</span></span> <span data-ttu-id="c6c2f-958">データベース管理者は、一連の実行中のアプリケーション データベースを取得する SQL スクリプトを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-958">Database administrators must run a series of SQL scripts to get the application database running.</span></span> <span data-ttu-id="c6c2f-959">このようなインストールに相当する労力は、多くの場合、完了までに時間がかかると、慎重に文書化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-959">Such installations are labor intensive, often take hours to complete, and must be carefully documented.</span></span>

<span data-ttu-id="c6c2f-960">Visual Studio 2010 には、これらの問題に対処して、シームレスに Web アプリケーションを展開するためのテクノロジが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-960">Visual Studio 2010 includes technologies that address these issues and that let you seamlessly deploy Web applications.</span></span> <span data-ttu-id="c6c2f-961">これらのテクノロジの 1 つは、IIS Web 配置ツール (MsDeploy.exe) です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-961">One of these technologies is the IIS Web Deployment Tool (MsDeploy.exe).</span></span>

<span data-ttu-id="c6c2f-962">Visual Studio 2010 で web デプロイ機能では、次の主要な領域があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-962">Web deployment features in Visual Studio 2010 include the following major areas:</span></span>

- <span data-ttu-id="c6c2f-963">Web のパッケージ化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-963">Web packaging</span></span>
- <span data-ttu-id="c6c2f-964">Web.config 変換</span><span class="sxs-lookup"><span data-stu-id="c6c2f-964">Web.config transformation</span></span>
- <span data-ttu-id="c6c2f-965">データベースの配置</span><span class="sxs-lookup"><span data-stu-id="c6c2f-965">Database deployment</span></span>
- <span data-ttu-id="c6c2f-966">1 回のクリックを Web アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-966">One-click publish for Web applications</span></span>

<span data-ttu-id="c6c2f-967">次のセクションでは、これらの機能に関する詳細情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-967">The following sections provide details about these features.</span></span>

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a><span data-ttu-id="c6c2f-968">Web のパッケージ化</span><span class="sxs-lookup"><span data-stu-id="c6c2f-968">Web Packaging</span></span>

<span data-ttu-id="c6c2f-969">Visual Studio 2010 MSDeploy ツールを使用すると呼ばれますが、アプリケーションの圧縮 (.zip) ファイルを作成、 *Web パッケージ*です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-969">Visual Studio 2010 uses the MSDeploy tool to create a compressed (.zip) file for your application, which is referred to as a *Web package*.</span></span> <span data-ttu-id="c6c2f-970">パッケージ ファイルには、アプリケーションのほか、次のコンテンツに関するメタデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-970">The package file contains metadata about your application plus the following content:</span></span>

- <span data-ttu-id="c6c2f-971">IIS の設定、アプリケーション プールの設定や、エラー ページの設定が含まれています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-971">IIS settings, which includes application pool settings, error page settings, and so on.</span></span>
- <span data-ttu-id="c6c2f-972">実際 Web コンテンツ、Web ページには、ユーザー コントロールには、静的なコンテンツ (イメージや HTML ファイル) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-972">The actual Web content, which includes Web pages, user controls, static content (images and HTML files), and so on.</span></span>
- <span data-ttu-id="c6c2f-973">SQL Server データベース スキーマとデータ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-973">SQL Server database schemas and data.</span></span>
- <span data-ttu-id="c6c2f-974">セキュリティ証明書は、GAC、レジストリ設定でインストールするコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-974">Security certificates, components to install in the GAC, registry settings, and so on.</span></span>

<span data-ttu-id="c6c2f-975">Web パッケージを任意のサーバーにコピーされ、IIS マネージャーを使用して手動でインストールされていることができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-975">A Web package can be copied to any server and then installed manually by using IIS Manager.</span></span> <span data-ttu-id="c6c2f-976">または、展開の自動化、パッケージをインストールするコマンド ライン コマンドを使用して、または配置 Api を使用しています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-976">Alternatively, for automated deployment, the package can be installed by using command-line commands or by using deployment APIs.</span></span>

<span data-ttu-id="c6c2f-977">Visual Studio 2010 は、組み込みの MSBuild タスクと Web のパッケージを作成するターゲットを提供します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-977">Visual Studio 2010 provides built in MSBuild tasks and targets to create Web packages.</span></span> <span data-ttu-id="c6c2f-978">詳細については、次を参照してください。 [ASP.NET Web アプリケーション プロジェクトの展開の概要](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx)MSDN Web サイトおよび[10 + 20 上の理由から、Web のパッケージを作成する必要があります理由](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html)Vishal Joshi ブログ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-978">For more information, see [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) on the MSDN Web site and [10 + 20 reasons why you should create a Web Package](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) on Vishal Joshi's blog.</span></span>

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a><span data-ttu-id="c6c2f-979">Web.config 変換</span><span class="sxs-lookup"><span data-stu-id="c6c2f-979">Web.config Transformation</span></span>

<span data-ttu-id="c6c2f-980">Web アプリケーションの配置に対して Visual Studio 2010 が導入されて[XML ドキュメントの変換 (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)、変換する機能は、`Web.config`ファイルの開発設定を実稼働の設定にします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-980">For Web application deployment, Visual Studio 2010 introduces [XML Document Transform (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), which is a feature that lets you transform a `Web.config` file from development settings to production settings.</span></span> <span data-ttu-id="c6c2f-981">変換の設定がという名前の変換ファイルで指定された`web.debug.config`、`web.release.config`のようにします。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-981">Transformation settings are specified in transform files named `web.debug.config`, `web.release.config`, and so on.</span></span> <span data-ttu-id="c6c2f-982">(これらのファイルの名前は一致 MSBuild 構成です)。変換ファイルには、展開済みにする必要がある変更のみが含まれています。`Web.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-982">(The names of these files match MSBuild configurations.) A transform file includes just the changes that you need to make to a deployed `Web.config` file.</span></span> <span data-ttu-id="c6c2f-983">変更内容を指定するには、単純な構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-983">You specify the changes by using simple syntax.</span></span>

<span data-ttu-id="c6c2f-984">次の例の一部を示しています、`web.release.config`リリース構成の展開用にファイルが生成される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-984">The following example shows a portion of a `web.release.config` file that might be produced for deployment of your release configuration.</span></span> <span data-ttu-id="c6c2f-985">置換キーワードの例を指定する配置時に、 *connectionString*内のノード、`Web.config`ファイルは例に記載されている値に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-985">The Replace keyword in the example specifies that during deployment the *connectionString* node in the `Web.config` file will be replaced with the values that are listed in the example.</span></span>

[!code-xml[Main](overview/samples/sample102.xml)]

<span data-ttu-id="c6c2f-986">詳細については、次を参照してください[Web アプリケーション プロジェクトの配置の Web.config 変換構文](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx)を MSDN <a id="0.2_a"> </a> Web サイトおよび[Web 展開: Web.Config 変換](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)。Vishal Joshi ブログ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-986">For more information, see [Web.config Transformation Syntax for Web Application Project Deployment](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) on the MSDN <a id="0.2_a"></a> Web site and[Web Deployment: Web.Config Transformation](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) on Vishal Joshi's blog.</span></span>

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a><span data-ttu-id="c6c2f-987">データベースの配置</span><span class="sxs-lookup"><span data-stu-id="c6c2f-987">Database Deployment</span></span>

<span data-ttu-id="c6c2f-988">Visual Studio 2010 の配置パッケージには、SQL Server データベースへの依存関係を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-988">A Visual Studio 2010 deployment package can include dependencies on SQL Server databases.</span></span> <span data-ttu-id="c6c2f-989">パッケージの定義の一部として、ソース データベースの接続文字列を提供します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-989">As part of the package definition, you provide the connection string for your source database.</span></span> <span data-ttu-id="c6c2f-990">Web パッケージを作成するときに、Visual Studio 2010 は、データベース スキーマについて、必要に応じて、データの SQL スクリプトの作成し、これらをパッケージに追加されます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-990">When you create the Web package, Visual Studio 2010 creates SQL scripts for the database schema and optionally for the data, and then adds these to the package.</span></span> <span data-ttu-id="c6c2f-991">カスタムの SQL スクリプトを提供し、サーバーで実行する必要がありますシーケンスを指定できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-991">You can also provide custom SQL scripts and specify the sequence in which they should run on the server.</span></span> <span data-ttu-id="c6c2f-992">展開時に、対象サーバーを適切な接続文字列を指定します。展開プロセスは、この接続文字列を使用して、データベース スキーマを作成し、データを追加するスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-992">At deployment time, you provide a connection string that is appropriate for the target server; the deployment process then uses this connection string to run the scripts that create the database schema and add the data.</span></span>

<span data-ttu-id="c6c2f-993">さらに、1 回のクリックを使用して、発行、アプリケーションがリモート共有ホスティング サイトに発行されると、データベースを直接発行する展開を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-993">In addition, by using one-click publish, you can configure deployment to publish your database directly when the application is published to a remote shared hosting site.</span></span> <span data-ttu-id="c6c2f-994">詳細については、次を参照してください。[する方法: Web アプリケーション プロジェクトでのデータベースを配置](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx)MSDN Web サイトおよび[VS 2010 でのデータベースの配置](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi ブログ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-994">For more information, see [How to: Deploy a Database With a Web Application Project](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) on the MSDN Web site and [Database Deployment with VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) on Vishal Joshi's blog.</span></span>

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a><span data-ttu-id="c6c2f-995">1 回のクリックを Web アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-995">One-Click Publish for Web Applications</span></span>

<span data-ttu-id="c6c2f-996">Visual Studio 2010 では、リモート サーバーに Web アプリケーションを公開する、IIS のリモート管理サービスを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-996">Visual Studio 2010 also lets you use the IIS remote management service to publish a Web application to a remote server.</span></span> <span data-ttu-id="c6c2f-997">サーバーをテストまたはステージング サーバーまたはホスト アカウントの発行プロファイルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-997">You can create a publish profile for your hosting account or for testing servers or staging servers.</span></span> <span data-ttu-id="c6c2f-998">各プロファイルは、適切な資格情報を安全に保存できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-998">Each profile can save appropriate credentials securely.</span></span> <span data-ttu-id="c6c2f-999">ターゲットのいずれかに展開することができます Web 1 回のクリックを使用して 1 回のクリックでのサーバーは、ツールバーを公開します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-999">You can then deploy to any of the target servers with one click by using the Web one-click publish toolbar.</span></span> <span data-ttu-id="c6c2f-1000">Visual Studio 2010 で、MSBuild コマンドラインを使用して公開することもできます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1000">With Visual Studio 2010, you can also publish by using the MSBuild command line.</span></span> <span data-ttu-id="c6c2f-1001">これにより発行を継続的インテグレーション モデルに含めるチーム ビルド環境を構成できます。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1001">This lets you configure your team build environment to include publishing in a continuous-integration model.</span></span>

<span data-ttu-id="c6c2f-1002">詳細については、次を参照してください。[する方法: Web アプリケーション プロジェクトを使用して 1 回のクリックの 発行および Web デプロイのデプロイ](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx)MSDN Web サイトと[VS 2010 を 1 クリック発行 Web](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) Vishal Joshi ブログ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1002">For more information, see [How to: Deploy a Web Application Project Using One-Click Publish and Web Deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) on the MSDN Web site and [Web 1-Click Publish with VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) on Vishal Joshi's blog.</span></span> <span data-ttu-id="c6c2f-1003">Visual Studio 2010 で Web アプリケーションのデプロイに関するビデオ プレゼンテーションを表示するを参照してください。 [Web Developer Preview の VS 2010](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) Vishal Joshi ブログ。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1003">To view video presentations about Web application deployment in Visual Studio 2010, see [VS 2010 for Web Developer Previews](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) on Vishal Joshi's blog.</span></span>

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a><span data-ttu-id="c6c2f-1004">リソース</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1004">Resources</span></span>

<span data-ttu-id="c6c2f-1005">次の Web サイトでは、ASP.NET 4 および Visual Studio 2010 に関する追加情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1005">The following Web sites provide additional information about ASP.NET 4 and Visual Studio 2010.</span></span>

- <span data-ttu-id="c6c2f-1006">[ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) -MSDN Web サイトで ASP.NET 4 用の公式のドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1006">[ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) — The official documentation for ASP.NET 4 on the MSDN Web site.</span></span>
- <span data-ttu-id="c6c2f-1007">[https://www.asp.net/](https://www.asp.net/) -ASP.NET チームの Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1007">[https://www.asp.net/](https://www.asp.net/) — The ASP.NET team's own Web site.</span></span>
- <span data-ttu-id="c6c2f-1008">[https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) および[ASP.NET 動的データのコンテンツ マップ](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx)-チーム サイトで ASP.NET し ASP.NET 動的データの公式のドキュメントでのオンライン リソース。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1008">[https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) and [ASP.NET Dynamic Data Content Map](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) — Online resources on the ASP.NET team site and in the official documentation for ASP.NET Dynamic Data.</span></span>
- <span data-ttu-id="c6c2f-1009">[https://www.asp.net/ajax/](../../ajax/index.md) -メインの Web リソースを ASP.NET Ajax 開発します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1009">[https://www.asp.net/ajax/](../../ajax/index.md) — The main Web resource for ASP.NET Ajax development.</span></span>
- <span data-ttu-id="c6c2f-1010">[https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — Visual Web Developer チーム ブログ Visual Studio 2010 の機能に関する情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1010">[https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — The Visual Web Developer Team blog, which includes information about features in Visual Studio 2010.</span></span>
- <span data-ttu-id="c6c2f-1011">[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) : ASP.NET のプレビュー リリースの Web リソースをメインです。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1011">[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) — The main Web resource for preview releases of ASP.NET.</span></span>

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a><span data-ttu-id="c6c2f-1012">免責情報</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1012">Disclaimer</span></span>

<span data-ttu-id="c6c2f-1013">このドキュメントは暫定版であり、ここで説明するソフトウェアの最終的な商業リリースより前に大幅に変更される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1013">This is a preliminary document and may be changed substantially prior to final commercial release of the software described herein.</span></span>

<span data-ttu-id="c6c2f-1014">このドキュメントに記載されている情報は、説明されている問題に関する Microsoft Corporation の発行日時点における最新の見解を表します。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1014">The information contained in this document represents the current view of Microsoft Corporation on the issues discussed as of the date of publication.</span></span> <span data-ttu-id="c6c2f-1015">マイクロソフトは変化する市場の状況に対応する必要があるため、本ドキュメントをマイクロソフトの確約と解釈することはできず、発行日以降は、提示されている情報の正確性を保証できません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1015">Because Microsoft must respond to changing market conditions, it should not be interpreted to be a commitment on the part of Microsoft, and Microsoft cannot guarantee the accuracy of any information presented after the date of publication.</span></span>

<span data-ttu-id="c6c2f-1016">このホワイト ペーパーは、情報提供のみを目的としています。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1016">This White Paper is for informational purposes only.</span></span> <span data-ttu-id="c6c2f-1017">マイクロソフトは、このドキュメント内の情報に関して、明示的、暗黙的、または法的ないかなる保証も行いません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1017">MICROSOFT MAKES NO WARRANTIES, EXPRESS, IMPLIED OR STATUTORY, AS TO THE INFORMATION IN THIS DOCUMENT.</span></span>

<span data-ttu-id="c6c2f-1018">お客様ご自身の責任において、適用されるすべての著作権関連法規に従ったご使用を願います。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1018">Complying with all applicable copyright laws is the responsibility of the user.</span></span> <span data-ttu-id="c6c2f-1019">このドキュメントのいかなる部分も、米国 Microsoft Corporation の書面による許諾を受けることなく、その目的を問わず、どのような形態であっても、複製または譲渡することは禁じられています。ここでいう形態とは、複写や記録など、電子的な、または物理的なすべての手段を含みます。ただしこれは、著作権法上のお客様の権利を制限するものではありません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1019">Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.</span></span>

<span data-ttu-id="c6c2f-1020">マイクロソフトは、このドキュメントに記載されている内容に関し、特許、特許申請、商標、著作権、またはその他の無体財産権を有する場合があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1020">Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document.</span></span> <span data-ttu-id="c6c2f-1021">別途マイクロソフトのライセンス契約上に明示の規定のない限り、このドキュメントはこれらの特許、商標、著作権、またはその他の無体財産権に関する権利をお客様に許諾するものではありません。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1021">Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.</span></span>

<span data-ttu-id="c6c2f-1022">特に明記されない限り、使用している会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、およびイベント実在は、架空のもの、および関連会社、組織、製品、ドメイン名、電子メールをアドレス、ロゴ、人物、場所またはイベントはまたは意図して推論する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1022">Unless otherwise noted, the example companies, organizations, products, domain names, email addresses, logos, people, places and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, email address, logo, person, place or event is intended or should be inferred.</span></span>

<span data-ttu-id="c6c2f-1023">© 2009 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1023">© 2009 Microsoft Corporation.</span></span> <span data-ttu-id="c6c2f-1024">All rights reserved.</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1024">All rights reserved.</span></span>

<span data-ttu-id="c6c2f-1025">Microsoft および Windows は、米国および他の国における Microsoft Corporation の登録商標または商標です。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1025">Microsoft and Windows are either registered trademarks or trademarks of Microsoft Corporation in the United States and/or other countries.</span></span>

<span data-ttu-id="c6c2f-1026">本ドキュメントで説明する実際の製造元および製品名は、それぞれの所有者の商標である場合があります。</span><span class="sxs-lookup"><span data-stu-id="c6c2f-1026">The names of actual companies and products mentioned herein may be the trademarks of their respective owners.</span></span>
