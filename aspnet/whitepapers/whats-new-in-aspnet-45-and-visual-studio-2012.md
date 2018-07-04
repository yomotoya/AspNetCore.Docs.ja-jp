---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: ASP.NET 4.5 と Visual Studio 2012 における新 |Microsoft Docs
author: rick-anderson
description: このドキュメントでは、新機能と ASP.NET 4.5 で追加される予定の機能強化について説明します。 Web 開発の強化についても説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/29/2012
ms.topic: article
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
ms.technology: ''
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: e89b8faa7d513209436d40d15821694cf24fa453
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369348"
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a><span data-ttu-id="521b6-104">ASP.NET 4.5 と Visual Studio 2012 における新します。</span><span class="sxs-lookup"><span data-stu-id="521b6-104">What's New in ASP.NET 4.5 and Visual Studio 2012</span></span>
====================
> <span data-ttu-id="521b6-105">このドキュメントでは、新機能と ASP.NET 4.5 で追加される予定の機能強化について説明します。</span><span class="sxs-lookup"><span data-stu-id="521b6-105">This document describes new features and enhancements that are being introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="521b6-106">Visual Studio 2012 での web 開発の強化についても説明します。</span><span class="sxs-lookup"><span data-stu-id="521b6-106">It also describes improvements being made for web development in Visual Studio 2012.</span></span> <span data-ttu-id="521b6-107">このドキュメントでは、2012 年 2 月 29 日発行されました。</span><span class="sxs-lookup"><span data-stu-id="521b6-107">This document was originally published on February 29, 2012.</span></span>


- [<span data-ttu-id="521b6-108">ASP.NET Core ランタイムおよびフレームワーク</span><span class="sxs-lookup"><span data-stu-id="521b6-108">ASP.NET Core Runtime and Framework</span></span>](#_Toc318097372)

    - [<span data-ttu-id="521b6-109">非同期的に読み取りと HTTP 要求と応答の書き込み</span><span class="sxs-lookup"><span data-stu-id="521b6-109">Asynchronously Reading and Writing HTTP Requests and Responses</span></span>](#_Toc318097373)
    - [<span data-ttu-id="521b6-110">HttpRequest の処理の機能強化</span><span class="sxs-lookup"><span data-stu-id="521b6-110">Improvements to HttpRequest handling</span></span>](#_Toc318097374)
    - [<span data-ttu-id="521b6-111">非同期的に応答をフラッシュ</span><span class="sxs-lookup"><span data-stu-id="521b6-111">Asynchronously flushing a response</span></span>](#_Toc318097375)
    - [<span data-ttu-id="521b6-112">サポート*await*と*タスク*-ベースの非同期モジュールとハンドラー</span><span class="sxs-lookup"><span data-stu-id="521b6-112">Support for *await* and *Task*-Based Asynchronous Modules and Handlers</span></span>](#_Toc318097376)
    - [<span data-ttu-id="521b6-113">非同期 HTTP モジュール</span><span class="sxs-lookup"><span data-stu-id="521b6-113">Asynchronous HTTP modules</span></span>](#_Toc318097377)
    - [<span data-ttu-id="521b6-114">非同期 HTTP ハンドラ</span><span class="sxs-lookup"><span data-stu-id="521b6-114">Asynchronous HTTP handlers</span></span>](#_Toc318097378)
    - [<span data-ttu-id="521b6-115">新しい ASP.NET 要求の検証機能</span><span class="sxs-lookup"><span data-stu-id="521b6-115">New ASP.NET Request Validation Features</span></span>](#_Toc318097379)
    - [<span data-ttu-id="521b6-116">(「レイジー」) の要求の検証を延期</span><span class="sxs-lookup"><span data-stu-id="521b6-116">Deferred ("lazy") request validation</span></span>](#_Toc318097380)
    - [<span data-ttu-id="521b6-117">未検証の要求のサポート</span><span class="sxs-lookup"><span data-stu-id="521b6-117">Support for unvalidated requests</span></span>](#_Toc318097381)
    - [<span data-ttu-id="521b6-118">AntiXSS ライブラリ</span><span class="sxs-lookup"><span data-stu-id="521b6-118">AntiXSS Library</span></span>](#_Toc318097382)
    - [<span data-ttu-id="521b6-119">Websocket プロトコルのサポート</span><span class="sxs-lookup"><span data-stu-id="521b6-119">Support for WebSockets Protocol</span></span>](#_Toc318097383)
    - [<span data-ttu-id="521b6-120">バンドルと縮小</span><span class="sxs-lookup"><span data-stu-id="521b6-120">Bundling and Minification</span></span>](#_Toc318097384)
    - [<span data-ttu-id="521b6-121">Web ホストのパフォーマンスの向上</span><span class="sxs-lookup"><span data-stu-id="521b6-121">Performance Improvements for Web Hosting</span></span>](#_Toc_perf)

        - [<span data-ttu-id="521b6-122">主要なパフォーマンスの要因</span><span class="sxs-lookup"><span data-stu-id="521b6-122">Key Performance Factors</span></span>](#_Toc_perf_1)
        - [<span data-ttu-id="521b6-123">新しいパフォーマンス機能の要件</span><span class="sxs-lookup"><span data-stu-id="521b6-123">Requirements for New Performance Features</span></span>](#_Toc_perf_2)
        - [<span data-ttu-id="521b6-124">一般的なアセンブリの共有</span><span class="sxs-lookup"><span data-stu-id="521b6-124">Sharing Common Assemblies</span></span>](#_Toc_perf_3)
        - [<span data-ttu-id="521b6-125">マルチコア JIT コンパイルを使用して、すばやい起動時間の</span><span class="sxs-lookup"><span data-stu-id="521b6-125">Using multi-Core JIT compilation for faster startup</span></span>](#_Toc_perf_4)
        - [<span data-ttu-id="521b6-126">チューニングのガベージ コレクションのメモリを最適化するには</span><span class="sxs-lookup"><span data-stu-id="521b6-126">Tuning garbage collection to optimize for memory</span></span>](#_Toc_perf_5)
        - [<span data-ttu-id="521b6-127">Web アプリケーションのプリフェッチ</span><span class="sxs-lookup"><span data-stu-id="521b6-127">Prefetching for web applications</span></span>](#_Toc_perf_6)
- [<span data-ttu-id="521b6-128">ASP.NET Web フォーム</span><span class="sxs-lookup"><span data-stu-id="521b6-128">ASP.NET Web Forms</span></span>](#_Toc318097385)

    - [<span data-ttu-id="521b6-129">厳密に型指定されたデータ コントロール</span><span class="sxs-lookup"><span data-stu-id="521b6-129">Strongly Typed Data Controls</span></span>](#_Toc318097386)
    - [<span data-ttu-id="521b6-130">モデル バインド</span><span class="sxs-lookup"><span data-stu-id="521b6-130">Model Binding</span></span>](#_Toc318097387)

        - [<span data-ttu-id="521b6-131">データの選択</span><span class="sxs-lookup"><span data-stu-id="521b6-131">Selecting data</span></span>](#_Toc318097388)
        - [<span data-ttu-id="521b6-132">値プロバイダー</span><span class="sxs-lookup"><span data-stu-id="521b6-132">Value providers</span></span>](#_Toc318097389)
        - [<span data-ttu-id="521b6-133">コントロールの値によってフィルター処理</span><span class="sxs-lookup"><span data-stu-id="521b6-133">Filtering by values from a control</span></span>](#_Toc318097390)
    - [<span data-ttu-id="521b6-134">HTML エンコードされたデータ バインディング式</span><span class="sxs-lookup"><span data-stu-id="521b6-134">HTML Encoded Data-Binding Expressions</span></span>](#_Toc318097391)
    - [<span data-ttu-id="521b6-135">控え目な検証</span><span class="sxs-lookup"><span data-stu-id="521b6-135">Unobtrusive Validation</span></span>](#_Toc318097392)
    - [<span data-ttu-id="521b6-136">HTML5 の更新プログラム</span><span class="sxs-lookup"><span data-stu-id="521b6-136">HTML5 Updates</span></span>](#_Toc318097393)
- [<span data-ttu-id="521b6-137">ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="521b6-137">ASP.NET MVC 4</span></span>](#_Toc318097394)
- [<span data-ttu-id="521b6-138">ASP.NET Web ページ 2</span><span class="sxs-lookup"><span data-stu-id="521b6-138">ASP.NET Web Pages 2</span></span>](#_Toc318097395)
- [<span data-ttu-id="521b6-139">Visual Studio 2012 リリース候補</span><span class="sxs-lookup"><span data-stu-id="521b6-139">Visual Studio 2012 Release Candidate</span></span>](#_Toc318097396)

    - [<span data-ttu-id="521b6-140">Visual Studio 2010 と Visual Studio 2012 リリース候補 (プロジェクト互換性) 間の共有プロジェクト</span><span class="sxs-lookup"><span data-stu-id="521b6-140">Project Sharing Between Visual Studio 2010 and Visual Studio 2012 Release Candidate (Project Compatibility)</span></span>](#project-compatibility)
    - [<span data-ttu-id="521b6-141">ASP.NET 4.5 web サイト テンプレートの構成の変更</span><span class="sxs-lookup"><span data-stu-id="521b6-141">Configuration Changes in ASP.NET 4.5 Website Templates</span></span>](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [<span data-ttu-id="521b6-142">ASP.NET ルーティング IIS 7 でネイティブ サポート</span><span class="sxs-lookup"><span data-stu-id="521b6-142">Native Support in IIS 7 for ASP.NET Routing</span></span>](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [<span data-ttu-id="521b6-143">HTML エディター</span><span class="sxs-lookup"><span data-stu-id="521b6-143">HTML Editor</span></span>](#_Toc318097397)

        - [<span data-ttu-id="521b6-144">スマート タスク</span><span class="sxs-lookup"><span data-stu-id="521b6-144">Smart Tasks</span></span>](#_Toc318097398)
        - [<span data-ttu-id="521b6-145">待機する ARIA のサポート</span><span class="sxs-lookup"><span data-stu-id="521b6-145">WAI-ARIA support</span></span>](#_Toc318097399)
        - [<span data-ttu-id="521b6-146">新しい HTML5 スニペット</span><span class="sxs-lookup"><span data-stu-id="521b6-146">New HTML5 snippets</span></span>](#_Toc318097400)
        - [<span data-ttu-id="521b6-147">ユーザー コントロールに展開します。</span><span class="sxs-lookup"><span data-stu-id="521b6-147">Extract to user control</span></span>](#_Toc318097401)
        - [<span data-ttu-id="521b6-148">属性でコード ナゲット用の IntelliSense</span><span class="sxs-lookup"><span data-stu-id="521b6-148">IntelliSense for code nuggets in attributes</span></span>](#_Toc318097402)
        - [<span data-ttu-id="521b6-149">自動開始タグまたは終了タグの名前を変更するときの対応タグの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="521b6-149">Automatic renaming of matching tag when you rename an opening or closing tag</span></span>](#_Toc318097403)
        - [<span data-ttu-id="521b6-150">イベント ハンドラーの生成</span><span class="sxs-lookup"><span data-stu-id="521b6-150">Event handler generation</span></span>](#_Toc318097404)
        - [<span data-ttu-id="521b6-151">スマート インデント</span><span class="sxs-lookup"><span data-stu-id="521b6-151">Smart indent</span></span>](#_Toc318097405)
        - [<span data-ttu-id="521b6-152">ステートメント入力候補の自動減少します。</span><span class="sxs-lookup"><span data-stu-id="521b6-152">Auto-reduce statement completion</span></span>](#_Toc318097406)
    - [<span data-ttu-id="521b6-153">JavaScript エディター</span><span class="sxs-lookup"><span data-stu-id="521b6-153">JavaScript Editor</span></span>](#_Toc318097407)

        - [<span data-ttu-id="521b6-154">コードのアウトライン表示</span><span class="sxs-lookup"><span data-stu-id="521b6-154">Code outlining</span></span>](#_Toc318097408)
        - [<span data-ttu-id="521b6-155">かっこの一致</span><span class="sxs-lookup"><span data-stu-id="521b6-155">Brace matching</span></span>](#_Toc318097409)
        - [<span data-ttu-id="521b6-156">定義に移動</span><span class="sxs-lookup"><span data-stu-id="521b6-156">Go to Definition</span></span>](#_Toc318097410)
        - [<span data-ttu-id="521b6-157">ECMAScript5 のサポート</span><span class="sxs-lookup"><span data-stu-id="521b6-157">ECMAScript5 support</span></span>](#_Toc318097411)
        - [<span data-ttu-id="521b6-158">DOM の IntelliSense</span><span class="sxs-lookup"><span data-stu-id="521b6-158">DOM IntelliSense</span></span>](#_Toc318097412)
        - [<span data-ttu-id="521b6-159">VSDOC の署名のオーバー ロード</span><span class="sxs-lookup"><span data-stu-id="521b6-159">VSDOC signature overloads</span></span>](#_Toc318097413)
        - [<span data-ttu-id="521b6-160">暗黙的な参照</span><span class="sxs-lookup"><span data-stu-id="521b6-160">Implicit references</span></span>](#_Toc318097414)
    - [<span data-ttu-id="521b6-161">CSS エディター</span><span class="sxs-lookup"><span data-stu-id="521b6-161">CSS Editor</span></span>](#_Toc318097415)

        - [<span data-ttu-id="521b6-162">ステートメント入力候補の自動減少します。</span><span class="sxs-lookup"><span data-stu-id="521b6-162">Auto-reduce statement completion</span></span>](#_Toc318097416)
        - [<span data-ttu-id="521b6-163">階層インデントします。</span><span class="sxs-lookup"><span data-stu-id="521b6-163">Hierarchical indentation.</span></span>](#_Toc318097417)
        - [<span data-ttu-id="521b6-164">CSS ハックのサポート</span><span class="sxs-lookup"><span data-stu-id="521b6-164">CSS hacks support</span></span>](#_Toc318097418)
        - [<span data-ttu-id="521b6-165">ベンダー固有のスキーマ (- moz-、- webkit)</span><span class="sxs-lookup"><span data-stu-id="521b6-165">Vendor specific schemas (-moz-,-webkit)</span></span>](#_Toc318097419)
        - [<span data-ttu-id="521b6-166">コメント化とコメント解除のサポート</span><span class="sxs-lookup"><span data-stu-id="521b6-166">Commenting and uncommenting support</span></span>](#_Toc318097420)
        - [<span data-ttu-id="521b6-167">カラー ピッカー</span><span class="sxs-lookup"><span data-stu-id="521b6-167">Color picker</span></span>](#_Toc318097421)
        - [<span data-ttu-id="521b6-168">スニペット</span><span class="sxs-lookup"><span data-stu-id="521b6-168">Snippets</span></span>](#_Toc318097422)
        - [<span data-ttu-id="521b6-169">カスタムのリージョン</span><span class="sxs-lookup"><span data-stu-id="521b6-169">Custom regions</span></span>](#_Toc318097423)
    - [<span data-ttu-id="521b6-170">Page Inspector</span><span class="sxs-lookup"><span data-stu-id="521b6-170">Page Inspector</span></span>](#_Toc318097424)
    - [<span data-ttu-id="521b6-171">発行</span><span class="sxs-lookup"><span data-stu-id="521b6-171">Publishing</span></span>](#_Toc318097425)

        - [<span data-ttu-id="521b6-172">発行プロファイル</span><span class="sxs-lookup"><span data-stu-id="521b6-172">Publish profiles</span></span>](#_Toc318097426)
        - [<span data-ttu-id="521b6-173">ASP.NET プリコンパイルとマージ</span><span class="sxs-lookup"><span data-stu-id="521b6-173">ASP.NET precompilation and merge</span></span>](#_Toc318097427)
- [<span data-ttu-id="521b6-174">IIS Express</span><span class="sxs-lookup"><span data-stu-id="521b6-174">IIS Express</span></span>](#_Toc318097428)
- [<span data-ttu-id="521b6-175">免責事項</span><span class="sxs-lookup"><span data-stu-id="521b6-175">Disclaimer</span></span>](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a><span data-ttu-id="521b6-176">ASP.NET Core ランタイムおよびフレームワーク</span><span class="sxs-lookup"><span data-stu-id="521b6-176">ASP.NET Core Runtime and Framework</span></span>

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a><span data-ttu-id="521b6-177">非同期的に読み取りと HTTP 要求と応答の書き込み</span><span class="sxs-lookup"><span data-stu-id="521b6-177">Asynchronously Reading and Writing HTTP Requests and Responses</span></span>

<span data-ttu-id="521b6-178">ASP.NET 4 に導入されたを使用してストリームとして HTTP 要求のエンティティを読み取る、 *HttpRequest.GetBufferlessInputStream*メソッド。</span><span class="sxs-lookup"><span data-stu-id="521b6-178">ASP.NET 4 introduced the ability to read an HTTP request entity as a stream using the *HttpRequest.GetBufferlessInputStream* method.</span></span> <span data-ttu-id="521b6-179">このメソッドは、要求のエンティティへのストリーミング アクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="521b6-179">This method provided streaming access to the request entity.</span></span> <span data-ttu-id="521b6-180">ただし、その同期的に実行、要求の期間のスレッドが関連付けられます。</span><span class="sxs-lookup"><span data-stu-id="521b6-180">However, it executed synchronously, which tied up a thread for the duration of a request.</span></span>

<span data-ttu-id="521b6-181">ASP.NET 4.5 には、HTTP 要求エンティティで非同期的にストリームを読み取る機能、および非同期的にフラッシュする機能がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="521b6-181">ASP.NET 4.5 supports the ability to read streams asynchronously on an HTTP request entity, and the ability to flush asynchronously.</span></span> <span data-ttu-id="521b6-182">ASP.NET 4.5 は倍、コント ローラーの統合が容易に .aspx ページ ハンドラーと ASP.NET MVC などのダウン ストリームの HTTP ハンドラーを提供する HTTP 要求エンティティをバッファーする機能も提供します。</span><span class="sxs-lookup"><span data-stu-id="521b6-182">ASP.NET 4.5 also gives you the ability to double-buffer an HTTP request entity, which provides easier integration with downstream HTTP handlers such as .aspx page handlers and ASP.NET MVC controllers.</span></span>

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a><span data-ttu-id="521b6-183">HttpRequest の処理の機能強化</span><span class="sxs-lookup"><span data-stu-id="521b6-183">Improvements to HttpRequest handling</span></span>

<span data-ttu-id="521b6-184">ASP.NET 4.5 からによって返される Stream 参照*HttpRequest.GetBufferlessInputStream*同期および非同期の読み取りメソッドをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="521b6-184">The Stream reference returned by ASP.NET 4.5 from *HttpRequest.GetBufferlessInputStream* supports both synchronous and asynchronous read methods.</span></span> <span data-ttu-id="521b6-185">*Stream*から返されるオブジェクト*GetBufferlessInputStream* BeginRead および EndRead の両方のメソッドを実装するようになりました。</span><span class="sxs-lookup"><span data-stu-id="521b6-185">The *Stream* object returned from *GetBufferlessInputStream* now implements both the BeginRead and EndRead methods.</span></span> <span data-ttu-id="521b6-186">非同期の*Stream*メソッドを使用して、ASP.NET の非同期読み取りループの各反復処理の間、現在のスレッドを解放するときに、チャンク単位で要求のエンティティを非同期的に読み取りますできます。</span><span class="sxs-lookup"><span data-stu-id="521b6-186">The asynchronous *Stream* methods let you asynchronously read the request entity in chunks, while ASP.NET releases the current thread between each iteration of an asynchronous read loop.</span></span>

<span data-ttu-id="521b6-187">ASP.NET 4.5 は、バッファー内の方法で要求のエンティティを読み取るためのコンパニオン メソッドも追加されています: *HttpRequest.GetBufferedInputStream*します。</span><span class="sxs-lookup"><span data-stu-id="521b6-187">ASP.NET 4.5 has also added a companion method for reading the request entity in a buffered way: *HttpRequest.GetBufferedInputStream*.</span></span> <span data-ttu-id="521b6-188">この新しいオーバー ロードのように機能*GetBufferlessInputStream*、同期および非同期の読み取りをサポートします。</span><span class="sxs-lookup"><span data-stu-id="521b6-188">This new overload works like *GetBufferlessInputStream*, supporting both synchronous and asynchronous reads.</span></span> <span data-ttu-id="521b6-189">ただし、読み取る、 *GetBufferedInputStream*もダウン ストリームのモジュールとハンドラーにより引き続き要求のエンティティがアクセスできるように ASP.NET の内部バッファーにエンティティのバイトをコピーします。</span><span class="sxs-lookup"><span data-stu-id="521b6-189">However, as it reads, *GetBufferedInputStream* also copies the entity bytes into ASP.NET internal buffers so that downstream modules and handlers can still access the request entity.</span></span> <span data-ttu-id="521b6-190">たとえば、アップ ストリームの一部、パイプライン内のコードは既に読み取られて、要求のエンティティを使用して*GetBufferedInputStream*、使用することもできます*HttpRequest.Form*または*指定ディレクトリ*.</span><span class="sxs-lookup"><span data-stu-id="521b6-190">For example, if some upstream code in the pipeline has already read the request entity using *GetBufferedInputStream*, you can still use *HttpRequest.Form* or *HttpRequest.Files*.</span></span> <span data-ttu-id="521b6-191">これにより、(データベースに大きなファイルのアップロードのストリーミングなど) の要求の非同期処理を実行するが .aspx ページや ASP.NET の MVC コント ローラーが、その後です。</span><span class="sxs-lookup"><span data-stu-id="521b6-191">This lets you perform asynchronous processing on a request (for example, streaming a large file upload to a database), but still run .aspx pages and MVC ASP.NET controllers afterward.</span></span>

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a><span data-ttu-id="521b6-192">非同期的に応答をフラッシュ</span><span class="sxs-lookup"><span data-stu-id="521b6-192">Asynchronously flushing a response</span></span>

<span data-ttu-id="521b6-193">HTTP クライアントに応答を送信すると、クライアントが遠くにあるまたは接続が低帯域幅のかなりの時間がかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="521b6-193">Sending responses to an HTTP client can take considerable time when the client is far away or has a low-bandwidth connection.</span></span> <span data-ttu-id="521b6-194">通常、アプリケーションによって作成される、ASP.NET は応答のバイト数をバッファーします。</span><span class="sxs-lookup"><span data-stu-id="521b6-194">Normally ASP.NET buffers the response bytes as they are created by an application.</span></span> <span data-ttu-id="521b6-195">ASP.NET は、最後に、要求処理の未収バッファーの 1 つの送信操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="521b6-195">ASP.NET then performs a single send operation of the accrued buffers at the very end of request processing.</span></span>

<span data-ttu-id="521b6-196">定期的に呼び出す必要がある場合、バッファー内の応答が大きい (たとえば、クライアントに大きなファイルのストリーミング) *HttpResponse.Flush*バッファー内の出力をクライアントに送信を管理下にあるメモリ使用量を保持します。</span><span class="sxs-lookup"><span data-stu-id="521b6-196">If the buffered response is large (for example, streaming a large file to a client), you must periodically call *HttpResponse.Flush* to send buffered output to the client and keep memory usage under control.</span></span> <span data-ttu-id="521b6-197">ただし、ため*フラッシュ*同期呼び出しを繰り返し呼び出すは*フラッシュ*スレッド実行可能性のある時間の長い要求の継続時間を消費します。</span><span class="sxs-lookup"><span data-stu-id="521b6-197">However, because *Flush* is a synchronous call, iteratively calling *Flush* still consumes a thread for the duration of potentially long-running requests.</span></span>

<span data-ttu-id="521b6-198">ASP.NET 4.5 を使用して非同期的にフラッシュを実行するサポートが追加、 *BeginFlush*と*EndFlush*のメソッド、 *HttpResponse*クラス。</span><span class="sxs-lookup"><span data-stu-id="521b6-198">ASP.NET 4.5 adds support for performing flushes asynchronously using the *BeginFlush* and *EndFlush* methods of the *HttpResponse* class.</span></span> <span data-ttu-id="521b6-199">これらのメソッドを使用して、非同期モジュールと増分データをオペレーティング システムのスレッドを停止せず、クライアントに送信する非同期のハンドラーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-199">Using these methods, you can create asynchronous modules and asynchronous handlers that incrementally send data to a client without tying up operating-system threads.</span></span> <span data-ttu-id="521b6-200">間に*BeginFlush*と*EndFlush*呼び出し、ASP.NET が現在のスレッドを解放します。</span><span class="sxs-lookup"><span data-stu-id="521b6-200">In between *BeginFlush* and *EndFlush* calls, ASP.NET releases the current thread.</span></span> <span data-ttu-id="521b6-201">これを実行時間の長い HTTP ダウンロードをサポートするために必要なアクティブなスレッドの合計数大幅に削減します。</span><span class="sxs-lookup"><span data-stu-id="521b6-201">This substantially reduces the total number of active threads that are needed in order to support long-running HTTP downloads.</span></span>

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a><span data-ttu-id="521b6-202">サポート*await*と*タスク*-ベースの非同期モジュールとハンドラー</span><span class="sxs-lookup"><span data-stu-id="521b6-202">Support for *await* and *Task* - Based Asynchronous Modules and Handlers</span></span>

<span data-ttu-id="521b6-203">.NET Framework 4 と呼ばれる非同期のプログラミング概念を導入された、*タスク*します。</span><span class="sxs-lookup"><span data-stu-id="521b6-203">The .NET Framework 4 introduced an asynchronous programming concept referred to as a *task*.</span></span> <span data-ttu-id="521b6-204">タスクがによって表される、*タスク*型と関連する型を*System.Threading.Tasks*名前空間。</span><span class="sxs-lookup"><span data-stu-id="521b6-204">Tasks are represented by the *Task* type and related types in the *System.Threading.Tasks* namespace.</span></span> <span data-ttu-id="521b6-205">.NET Framework 4.5 は、操作できるようにするコンパイラが強化された*タスク*オブジェクトの単純な。</span><span class="sxs-lookup"><span data-stu-id="521b6-205">The .NET Framework 4.5 builds on this with compiler enhancements that make working with *Task* objects simple.</span></span> <span data-ttu-id="521b6-206">コンパイラが 2 つの新しいキーワードをサポートする、.NET Framework 4.5: *await*と*async*します。</span><span class="sxs-lookup"><span data-stu-id="521b6-206">In the .NET Framework 4.5, the compilers support two new keywords: *await* and *async*.</span></span> <span data-ttu-id="521b6-207">*Await*キーワードは簡略記法を示すためのコードがコードの他のいくつかの一部で非同期的に待機します。</span><span class="sxs-lookup"><span data-stu-id="521b6-207">The *await* keyword is syntactical shorthand for indicating that a piece of code should asynchronously wait on some other piece of code.</span></span> <span data-ttu-id="521b6-208">*Async*キーワードは、タスク ベースの非同期メソッドとしてメソッドをマークするために使用できるヒントを表します。</span><span class="sxs-lookup"><span data-stu-id="521b6-208">The *async* keyword represents a hint that you can use to mark methods as task-based asynchronous methods.</span></span>

<span data-ttu-id="521b6-209">組み合わせ*await*、 *async*、および*タスク*オブジェクトにより、.NET 4.5 で非同期コードを記述するためのはるかに簡単です。</span><span class="sxs-lookup"><span data-stu-id="521b6-209">The combination of *await*, *async*, and the *Task* object makes it much easier for you to write asynchronous code in .NET 4.5.</span></span> <span data-ttu-id="521b6-210">ASP.NET 4.5 では、非同期 HTTP モジュールと、新しいコンパイラの機能強化を使用して、非同期 HTTP ハンドラーを記述できるようにする新しい Api でこれらの簡略化をサポートします。</span><span class="sxs-lookup"><span data-stu-id="521b6-210">ASP.NET 4.5 supports these simplifications with new APIs that let you write asynchronous HTTP modules and asynchronous HTTP handlers using the new compiler enhancements.</span></span>

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a><span data-ttu-id="521b6-211">非同期 HTTP モジュール</span><span class="sxs-lookup"><span data-stu-id="521b6-211">Asynchronous HTTP modules</span></span>

<span data-ttu-id="521b6-212">返すメソッド内で非同期操作を実行しながら、*タスク*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="521b6-212">Suppose that you want to perform asynchronous work within a method that returns a *Task* object.</span></span> <span data-ttu-id="521b6-213">次のコード例では、Microsoft のホーム ページをダウンロードする非同期の呼び出しを実行する非同期メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="521b6-213">The following code example defines an asynchronous method that makes an asynchronous call to download the Microsoft home page.</span></span> <span data-ttu-id="521b6-214">使用に注意してください、 *async*メソッド シグネチャ内のキーワードと*await*呼び出し*DownloadStringTaskAsync*。</span><span class="sxs-lookup"><span data-stu-id="521b6-214">Notice the use of the *async* keyword in the method signature and the *await* call to *DownloadStringTaskAsync*.</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

<span data-ttu-id="521b6-215">すべて記述する必要がある、.NET Framework のダウンロードを完了するを待つだけでなく、ダウンロードが完了したら、呼び出し履歴を自動的に復元中に、コール スタックをアンワインドは自動的に処理します。</span><span class="sxs-lookup"><span data-stu-id="521b6-215">That's all you have to write — the .NET Framework will automatically handle unwinding the call stack while waiting for the download to complete, as well as automatically restoring the call stack after the download is done.</span></span>

<span data-ttu-id="521b6-216">ここで、非同期の ASP.NET HTTP モジュールでこの非同期メソッドを使用するとします。</span><span class="sxs-lookup"><span data-stu-id="521b6-216">Now suppose that you want to use this asynchronous method in an asynchronous ASP.NET HTTP module.</span></span> <span data-ttu-id="521b6-217">ASP.NET 4.5 には、ヘルパー メソッドが含まれています (*EventHandlerTaskAsyncHelper*) と新しいデリゲート型 (*TaskEventHandler*) と、古いタスク ベースの非同期メソッドを統合するように使用できます。ASP.NET HTTP パイプラインによって公開される非同期プログラミング モデル。</span><span class="sxs-lookup"><span data-stu-id="521b6-217">ASP.NET 4.5 includes a helper method (*EventHandlerTaskAsyncHelper*) and a new delegate type (*TaskEventHandler*) that you can use to integrate task-based asynchronous methods with the older asynchronous programming model exposed by the ASP.NET HTTP pipeline.</span></span> <span data-ttu-id="521b6-218">この例ではどのように。</span><span class="sxs-lookup"><span data-stu-id="521b6-218">This example shows how:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a><span data-ttu-id="521b6-219">非同期 HTTP ハンドラ</span><span class="sxs-lookup"><span data-stu-id="521b6-219">Asynchronous HTTP handlers</span></span>

<span data-ttu-id="521b6-220">ASP.NET の非同期のハンドラーを記述する従来の方法で実装するためには、 *IHttpAsyncHandler*インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="521b6-220">The traditional approach to writing asynchronous handlers in ASP.NET is to implement the *IHttpAsyncHandler* interface.</span></span> <span data-ttu-id="521b6-221">ASP.NET 4.5 では、 *HttpTaskAsyncHandler*非同期基本型から派生したことができます、非同期ハンドラーの記述が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="521b6-221">ASP.NET 4.5 introduces the *HttpTaskAsyncHandler* asynchronous base type that you can derive from, which makes it much easier to write asynchronous handlers.</span></span>

<span data-ttu-id="521b6-222">*HttpTaskAsyncHandler*型が抽象であり、オーバーライドする必要があります、 *ProcessRequestAsync*メソッド。</span><span class="sxs-lookup"><span data-stu-id="521b6-222">The *HttpTaskAsyncHandler* type is abstract and requires you to override the *ProcessRequestAsync* method.</span></span> <span data-ttu-id="521b6-223">内部的に ASP.NET が、戻り値の署名の統合のくれます。 (、*タスク*オブジェクト) の*ProcessRequestAsync* ASP.NET パイプラインで使用される古いの非同期プログラミング モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="521b6-223">Internally ASP.NET takes care of integrating the return signature (a *Task* object) of *ProcessRequestAsync* with the older asynchronous programming model used by the ASP.NET pipeline.</span></span>

<span data-ttu-id="521b6-224">次の例は、使用する方法を示しています。*タスク*と*await*非同期 HTTP ハンドラーの実装の一部として。</span><span class="sxs-lookup"><span data-stu-id="521b6-224">The following example shows how you can use *Task* and *await* as part of the implementation of an asynchronous HTTP handler:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a><span data-ttu-id="521b6-225">新しい ASP.NET 要求の検証機能</span><span class="sxs-lookup"><span data-stu-id="521b6-225">New ASP.NET Request Validation Features</span></span>

<span data-ttu-id="521b6-226">既定では、ASP.NET は要求の検証を実行します: マークアップやフィールド、ヘッダー、cookie、およびなどでスクリプトを探す要求を調べます。</span><span class="sxs-lookup"><span data-stu-id="521b6-226">By default, ASP.NET performs request validation — it examines requests to look for markup or script in fields, headers, cookies, and so on.</span></span> <span data-ttu-id="521b6-227">いずれかが検出された場合、ASP.NET は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="521b6-227">If any is detected, ASP.NET throws an exception.</span></span> <span data-ttu-id="521b6-228">これは、潜在的なクロス サイト スクリプティング攻撃に対する防御の最前線として機能します。</span><span class="sxs-lookup"><span data-stu-id="521b6-228">This acts as a first line of defense against potential cross-site scripting attacks.</span></span>

<span data-ttu-id="521b6-229">ASP.NET 4.5 では、未検証要求データを選択的に読みやすくします。</span><span class="sxs-lookup"><span data-stu-id="521b6-229">ASP.NET 4.5 makes it easy to selectively read unvalidated request data.</span></span> <span data-ttu-id="521b6-230">ASP.NET 4.5 は、人気のある AntiXSS ライブラリで、外部ライブラリは、以前にも統合します。</span><span class="sxs-lookup"><span data-stu-id="521b6-230">ASP.NET 4.5 also integrates the popular AntiXSS library, which was formerly an external library.</span></span>

<span data-ttu-id="521b6-231">開発者が、選択的に、アプリケーションの要求検証を無効にする機能をよく寄せられます。</span><span class="sxs-lookup"><span data-stu-id="521b6-231">Developers have frequently asked for the ability to selectively turn off request validation for their applications.</span></span> <span data-ttu-id="521b6-232">たとえば、アプリケーションのフォーラム ソフトウェアが、可能性がある場合は、HTML 形式のフォーラムの投稿とコメントの送信が要求の検証はチェックは、他のすべてを確認できるようにします。</span><span class="sxs-lookup"><span data-stu-id="521b6-232">For example, if your application is forum software, you might want to allow users to submit HTML-formatted forum posts and comments, but still make sure that request validation is checking everything else.</span></span>

<span data-ttu-id="521b6-233">ASP.NET 4.5 を選択して検証されていない入力操作を簡単にする 2 つの機能が導入されています: (「レイジー」) の要求の検証および未検証要求データへのアクセスを遅延します。</span><span class="sxs-lookup"><span data-stu-id="521b6-233">ASP.NET 4.5 introduces two features that make it easy for you to selectively work with unvalidated input: deferred ("lazy") request validation and access to unvalidated request data.</span></span>

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a><span data-ttu-id="521b6-234">(「レイジー」) の要求の検証を延期</span><span class="sxs-lookup"><span data-stu-id="521b6-234">Deferred ("lazy") request validation</span></span>

<span data-ttu-id="521b6-235">既定を ASP.NET 4.5 で、すべての要求データは要求の検証の対象です。</span><span class="sxs-lookup"><span data-stu-id="521b6-235">In ASP.NET 4.5, by default all request data is subject to request validation.</span></span> <span data-ttu-id="521b6-236">ただし、要求データを実際にアクセスするまで、要求の検証を延期するアプリケーションを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="521b6-236">However, you can configure the application to defer request validation until you actually access request data.</span></span> <span data-ttu-id="521b6-237">(これとも呼ば遅延要求の検証、特定のデータ シナリオの遅延読み込みのような条件に基づいています。)遅延検証を設定して、Web.config ファイルで使用するアプリケーションを構成することができます、 *requestValidationMode*属性で 4.5 を*httpRUntime*要素は、次の例のように。</span><span class="sxs-lookup"><span data-stu-id="521b6-237">(This is sometimes referred to as lazy request validation, based on terms like lazy loading for certain data scenarios.) You can configure the application to use deferred validation in the Web.config file by setting the *requestValidationMode* attribute to 4.5 in the *httpRUntime* element, as in the following example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

<span data-ttu-id="521b6-238">要求の検証モードが 4.5 に設定されている場合、コードがその値にアクセスする場合にのみ、特定の要求の値に対してのみ、要求の検証がトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="521b6-238">When request validation mode is set to 4.5, request validation is triggered only for a specific request value and only when your code accesses that value.</span></span> <span data-ttu-id="521b6-239">たとえば、次のコードは、Request.Form["forum の値を取得します。\_投稿"]、要求の検証はフォームのコレクション内の要素に対してのみ呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-239">For example, if your code gets the value of Request.Form["forum\_post"], request validation is invoked only for that element in the form collection.</span></span> <span data-ttu-id="521b6-240">内の他の要素のいずれも、*フォーム*コレクションが検証されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-240">None of the other elements in the *Form* collection are validated.</span></span> <span data-ttu-id="521b6-241">要求の検証を行っていれば、以前のバージョンの ASP.NET では、要求全体のコレクションのコレクション内の任意の要素にアクセスしたときにトリガーされました。</span><span class="sxs-lookup"><span data-stu-id="521b6-241">In previous versions of ASP.NET, request validation was triggered for the entire request collection when any element in the collection was accessed.</span></span> <span data-ttu-id="521b6-242">新しい動作によって、別のアプリケーション コンポーネントを他の部分で要求の検証をトリガーすることがなく、要求データの異なる部分を見て簡単にできます。</span><span class="sxs-lookup"><span data-stu-id="521b6-242">The new behavior makes it easier for different application components to look at different pieces of request data without triggering request validation on other pieces.</span></span>

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a><span data-ttu-id="521b6-243">未検証の要求のサポート</span><span class="sxs-lookup"><span data-stu-id="521b6-243">Support for unvalidated requests</span></span>

<span data-ttu-id="521b6-244">遅延要求の検証が単独で要求の検証をバイパスする選択的に問題が解決しません。</span><span class="sxs-lookup"><span data-stu-id="521b6-244">Deferred request validation alone doesn't solve the problem of selectively bypassing request validation.</span></span> <span data-ttu-id="521b6-245">Request.Form["forum 呼び出し\_投稿"] もトリガーは、その特定の要求値の検証を要求します。</span><span class="sxs-lookup"><span data-stu-id="521b6-245">The call to Request.Form["forum\_post"] still triggers request validation for that specific request value.</span></span> <span data-ttu-id="521b6-246">ただし、そのフィールド内のマークアップを許可するための検証をトリガーせずにこのフィールドにアクセスする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="521b6-246">However, you might want to access this field without triggering validation because you want to allow markup in that field.</span></span>

<span data-ttu-id="521b6-247">これは、ASP.NET 4.5 には、未検証要求データへのようになりましたサポートしています。</span><span class="sxs-lookup"><span data-stu-id="521b6-247">To allow this, ASP.NET 4.5 now supports unvalidated access to request data.</span></span> <span data-ttu-id="521b6-248">ASP.NET 4.5 が含まれていますが、新しい*Unvalidated*コレクション プロパティで、 *HttpRequest*クラス。</span><span class="sxs-lookup"><span data-stu-id="521b6-248">ASP.NET 4.5 includes a new *Unvalidated* collection property in the *HttpRequest* class.</span></span> <span data-ttu-id="521b6-249">このコレクションがこのようなすべての要求のデータの一般的な値へのアクセスを提供します*フォーム*、 *QueryString*、 *Cookie*、および*Url*します。</span><span class="sxs-lookup"><span data-stu-id="521b6-249">This collection provides access to all of the common values of request data, like *Form*, *QueryString*, *Cookies*, and *Url*.</span></span>

<span data-ttu-id="521b6-250">フォーラムの例を使用して、未検証要求データを読めるように、まず新しい要求の検証モードを使用するアプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="521b6-250">Using the forum example, to be able to read unvalidated request data, you first need to configure the application to use the new request validation mode:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

<span data-ttu-id="521b6-251">使用することができますし、 *HttpRequest.Unvalidated*未検証のフォーム値を読み取るためのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="521b6-251">You can then use the *HttpRequest.Unvalidated* property to read the unvalidated form value:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> <span data-ttu-id="521b6-252">セキュリティ -*未検証要求データを使用して、注意してください。*</span><span class="sxs-lookup"><span data-stu-id="521b6-252">Security - *Use unvalidated request data with care!*</span></span> <span data-ttu-id="521b6-253">ASP.NET 4.5 では、未検証の要求のプロパティと非常に特定の未検証要求データのアクセスが容易にコレクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="521b6-253">ASP.NET 4.5 added the unvalidated request properties and collections to make it easier for you to access very specific unvalidated request data.</span></span> <span data-ttu-id="521b6-254">ただし、危険なテキストがない、ユーザーに表示されることを確認する要求の生データのカスタム検証を実行することも必要があります。</span><span class="sxs-lookup"><span data-stu-id="521b6-254">However, you must still perform custom validation on the raw request data to ensure that dangerous text is not rendered to users.</span></span>


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a><span data-ttu-id="521b6-255">AntiXSS ライブラリ</span><span class="sxs-lookup"><span data-stu-id="521b6-255">AntiXSS Library</span></span>

<span data-ttu-id="521b6-256">ASP.NET 4.5 により、Microsoft AntiXSS ライブラリの普及すると、そのライブラリのバージョン 4.0 からコア エンコーディング ルーチンはようになりましたが組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="521b6-256">Due to the popularity of the Microsoft AntiXSS Library, ASP.NET 4.5 now incorporates core encoding routines from version 4.0 of that library.</span></span>

<span data-ttu-id="521b6-257">エンコーディング ルーチンはによって実装される、 *AntiXssEncoder* 、新しい型*System.Web.Security.AntiXss*名前空間。</span><span class="sxs-lookup"><span data-stu-id="521b6-257">The encoding routines are implemented by the *AntiXssEncoder* type in the new *System.Web.Security.AntiXss* namespace.</span></span> <span data-ttu-id="521b6-258">使用することができます、 *AntiXssEncoder*型で実装されている静的なエンコード方法のいずれかを呼び出すことによって、直接型。</span><span class="sxs-lookup"><span data-stu-id="521b6-258">You can use the *AntiXssEncoder* type directly by calling any of the static encoding methods that are implemented in the type.</span></span> <span data-ttu-id="521b6-259">ただし、新しい ANTI-XSS ルーチンを使用するための最も簡単な方法が使用する ASP.NET アプリケーションを構成するのには、 *AntiXssEncoder*既定クラス。</span><span class="sxs-lookup"><span data-stu-id="521b6-259">However, the easiest approach for using the new anti-XSS routines is to configure an ASP.NET application to use the *AntiXssEncoder* class by default.</span></span> <span data-ttu-id="521b6-260">これを行うには、Web.config ファイルに次の属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="521b6-260">To do this, add the following attribute to the Web.config file:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

<span data-ttu-id="521b6-261">ときに、 *encoderType*属性を使用して、設定されて、 *AntiXssEncoder*型、すべての出力を新しいエンコーディング ルーチンを使用して自動的に ASP.NET でのエンコードします。</span><span class="sxs-lookup"><span data-stu-id="521b6-261">When the *encoderType* attribute is set to use the *AntiXssEncoder* type, all output encoding in ASP.NET automatically uses the new encoding routines.</span></span>

<span data-ttu-id="521b6-262">ASP.NET 4.5 組み込まれている外部 AntiXSS ライブラリの一部を次に示します。</span><span class="sxs-lookup"><span data-stu-id="521b6-262">These are the portions of the external AntiXSS library that have been incorporated into ASP.NET 4.5:</span></span>

- <span data-ttu-id="521b6-263">*HtmlEncode*、 *HtmlFormUrlEncode*、および*HtmlAttributeEncode*</span><span class="sxs-lookup"><span data-stu-id="521b6-263">*HtmlEncode*, *HtmlFormUrlEncode*, and *HtmlAttributeEncode*</span></span>
- <span data-ttu-id="521b6-264">*XmlAttributeEncode*と*XmlEncode*</span><span class="sxs-lookup"><span data-stu-id="521b6-264">*XmlAttributeEncode* and *XmlEncode*</span></span>
- <span data-ttu-id="521b6-265">*UrlEncode*と*UrlPathEncode* (新規)</span><span class="sxs-lookup"><span data-stu-id="521b6-265">*UrlEncode* and *UrlPathEncode* (new)</span></span>
- <span data-ttu-id="521b6-266">*CssEncode*</span><span class="sxs-lookup"><span data-stu-id="521b6-266">*CssEncode*</span></span>

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a><span data-ttu-id="521b6-267">Websocket プロトコルのサポート</span><span class="sxs-lookup"><span data-stu-id="521b6-267">Support for WebSockets Protocol</span></span>

<span data-ttu-id="521b6-268">Websocket プロトコルは、HTTP 経由でクライアントとサーバー間のセキュリティで保護された、リアルタイムの双方向通信を確立する方法を定義する標準ベースのネットワーク プロトコルです。</span><span class="sxs-lookup"><span data-stu-id="521b6-268">WebSockets protocol is a standards-based network protocol that defines how to establish secure, real-time bidirectional communications between a client and a server over HTTP.</span></span> <span data-ttu-id="521b6-269">Microsoft は、プロトコルを定義するために、IETF と W3C 標準の本体を持つきました。</span><span class="sxs-lookup"><span data-stu-id="521b6-269">Microsoft has worked with both the IETF and W3C standards bodies to help define the protocol.</span></span> <span data-ttu-id="521b6-270">WebSockets プロトコルは、Microsoft のクライアントとモバイル オペレーティング システムの両方で Websocket プロトコルをサポートしている多くのリソースへの投資と任意のクライアント (ブラウザーだけでなく) でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="521b6-270">The WebSockets protocol is supported by any client (not just browsers), with Microsoft investing substantial resources supporting WebSockets protocol on both client and mobile operating systems.</span></span>

<span data-ttu-id="521b6-271">Websocket プロトコルでは、はるかに簡単に、クライアントとサーバー間の実行時間の長いデータ転送を作成します。</span><span class="sxs-lookup"><span data-stu-id="521b6-271">WebSockets protocol makes it much easier to create long-running data transfers between a client and a server.</span></span> <span data-ttu-id="521b6-272">たとえば、チャット アプリケーションの作成がはるかに簡単クライアントとサーバー間の実際の実行時間の長い接続を確立するため。</span><span class="sxs-lookup"><span data-stu-id="521b6-272">For example, writing a chat application is much easier because you can establish a true long-running connection between a client and a server.</span></span> <span data-ttu-id="521b6-273">定期的なポーリングまたは HTTP 長いポーリングのソケットの動作をシミュレートするような回避策に頼る必要はありません。</span><span class="sxs-lookup"><span data-stu-id="521b6-273">You do not have to resort to workarounds like periodic polling or HTTP long-polling to simulate the behavior of a socket.</span></span>

<span data-ttu-id="521b6-274">ASP.NET 4.5 と IIS 8 の非同期的に読み取りと Websocket オブジェクトの文字列とバイナリ データの両方を記述するマネージ Api を使用する ASP.NET 開発者を有効にする WebSockets のサポート、低レベルに含まれます。</span><span class="sxs-lookup"><span data-stu-id="521b6-274">ASP.NET 4.5 and IIS 8 include low-level WebSockets support, enabling ASP.NET developers to use managed APIs for asynchronously reading and writing both string and binary data on a WebSockets object.</span></span> <span data-ttu-id="521b6-275">ASP.NET 4.5 では、新しい*System.Web.WebSockets* Websocket プロトコルを使用するための型を含む名前空間。</span><span class="sxs-lookup"><span data-stu-id="521b6-275">For ASP.NET 4.5, there is a new *System.Web.WebSockets* namespace that contains types for working with WebSockets protocol.</span></span>

<span data-ttu-id="521b6-276">ブラウザー クライアントが、DOM を作成して Websocket 接続を確立*WebSocket*次の例のように、ASP.NET アプリケーションで URL を指すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="521b6-276">A browser client establishes a WebSockets connection by creating a DOM *WebSocket* object that points to a URL in an ASP.NET application, as in the following example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

<span data-ttu-id="521b6-277">あらゆる種類のモジュールまたはハンドラーを使用して ASP.NET では、Websocket のエンドポイントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-277">You can create WebSockets endpoints in ASP.NET using any kind of module or handler.</span></span> <span data-ttu-id="521b6-278">前の例では、.ashx ファイルが使用された、.ashx ファイル ハンドラーを作成する簡単な方法であるためです。</span><span class="sxs-lookup"><span data-stu-id="521b6-278">In the previous example, an .ashx file was used, because .ashx files are a quick way to create a handler.</span></span>

<span data-ttu-id="521b6-279">に従って、Websocket プロトコルは、ASP.NET アプリケーションは、Websocket 要求に要求が HTTP GET 要求からアップグレードする必要がありますを指定して、クライアントの Websocket 要求を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="521b6-279">According to the WebSockets protocol, an ASP.NET application accepts a client's WebSockets request by indicating that the request should be upgraded from an HTTP GET request to a WebSockets request.</span></span> <span data-ttu-id="521b6-280">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="521b6-280">Here's an example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

<span data-ttu-id="521b6-281">*AcceptWebSocketRequest*メソッドは、ASP.NET は、現在の HTTP 要求をアンワインドして、関数デリゲートに制御を転送するために関数デリゲートを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="521b6-281">The *AcceptWebSocketRequest* method accepts a function delegate because ASP.NET unwinds the current HTTP request and then transfers control to the function delegate.</span></span> <span data-ttu-id="521b6-282">この方法は、使用する方法のような概念的に*System.Threading.Thread*作業を実行するバック グラウンドでスレッドの開始のデリゲートを定義します。</span><span class="sxs-lookup"><span data-stu-id="521b6-282">Conceptually this approach is similar to how you use *System.Threading.Thread*, where you define a thread-start delegate in which background work is performed.</span></span>

<span data-ttu-id="521b6-283">ASP.NET とクライアントでは、Websocket ハンドシェイクを正常に完了しました、ASP.NET は、デリゲートを呼び出すし、Websocket アプリケーションでは、実行が開始されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-283">After ASP.NET and the client have successfully completed a WebSockets handshake, ASP.NET calls your delegate and the WebSockets application starts running.</span></span> <span data-ttu-id="521b6-284">次のコード例では、ASP.NET の組み込みの Websocket のサポートを使用する単純なエコー アプリケーションを示します。</span><span class="sxs-lookup"><span data-stu-id="521b6-284">The following code example shows a simple echo application that uses the built-in WebSockets support in ASP.NET:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

<span data-ttu-id="521b6-285">.NET 4.5 でのサポート、 *await*キーワードとタスク ベースの非同期操作は、Websocket のアプリケーションを記述するために自然に適合します。</span><span class="sxs-lookup"><span data-stu-id="521b6-285">The support in .NET 4.5 for the *await* keyword and asynchronous task-based operations is a natural fit for writing WebSockets applications.</span></span> <span data-ttu-id="521b6-286">Websocket 要求を ASP.NET 内で完全に非同期的に実行するコード例に示します。</span><span class="sxs-lookup"><span data-stu-id="521b6-286">The code example shows that a WebSockets request runs completely asynchronously inside ASP.NET.</span></span> <span data-ttu-id="521b6-287">アプリケーションが呼び出すことによってクライアントから送信されるメッセージの非同期的に待機*ソケットを待機します。ReceiveAsync*します。</span><span class="sxs-lookup"><span data-stu-id="521b6-287">The application waits asynchronously for a message to be sent from a client by calling *await socket.ReceiveAsync*.</span></span> <span data-ttu-id="521b6-288">呼び出すことによって、クライアントに非同期メッセージを送信する同様に、*ソケットを待機します。SendAsync*します。</span><span class="sxs-lookup"><span data-stu-id="521b6-288">Similarly, you can send an asynchronous message to a client by calling *await socket.SendAsync*.</span></span>

<span data-ttu-id="521b6-289">Websocket 経由でメッセージを受信するアプリケーション、ブラウザーで、 *onmessage*関数。</span><span class="sxs-lookup"><span data-stu-id="521b6-289">In the browser, an application receives WebSockets messages through an *onmessage* function.</span></span> <span data-ttu-id="521b6-290">ブラウザーからメッセージを送信するを呼び出す、*送信*のメソッド、 *WebSocket* DOM の種類、この例で示すように。</span><span class="sxs-lookup"><span data-stu-id="521b6-290">To send a message from a browser, you call the *send* method of the *WebSocket* DOM type, as shown in this example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

<span data-ttu-id="521b6-291">今後、この機能を抽象離れたは低レベル コーディングの一部に必要であるこのリリースの Websocket アプリケーションへの更新プログラムがリリース可能性があります。</span><span class="sxs-lookup"><span data-stu-id="521b6-291">In the future, we might release updates to this functionality that abstract away some of the low-level coding that is required in this release for WebSockets applications.</span></span>

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a><span data-ttu-id="521b6-292">バンドルと縮小</span><span class="sxs-lookup"><span data-stu-id="521b6-292">Bundling and Minification</span></span>

<span data-ttu-id="521b6-293">バンドルには、個々 の JavaScript と CSS ファイルをまとめて 1 つのファイルのように扱うことができますをバンドルすることができます。</span><span class="sxs-lookup"><span data-stu-id="521b6-293">Bundling lets you combine individual JavaScript and CSS files into a bundle that can be treated like a single file.</span></span> <span data-ttu-id="521b6-294">縮小では、空白や不要なその他の文字を削除することで JavaScript と CSS ファイルでまとめます。</span><span class="sxs-lookup"><span data-stu-id="521b6-294">Minification condenses JavaScript and CSS files by removing whitespace and other characters that are not required.</span></span> <span data-ttu-id="521b6-295">これらの機能は、Web フォーム、ASP.NET MVC、および Web ページを使用します。</span><span class="sxs-lookup"><span data-stu-id="521b6-295">These features work with Web Forms, ASP.NET MVC, and Web Pages.</span></span>

<span data-ttu-id="521b6-296">バンドルはバンドル クラスまたは ScriptBundle と StyleBundle その子クラスのいずれかを使用して作成されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-296">Bundles are created using the Bundle class or one of its child classes, ScriptBundle and StyleBundle.</span></span> <span data-ttu-id="521b6-297">バンドルのインスタンスを構成した後、バンドルは公開着信要求にグローバル BundleCollection インスタンスに追加するだけで。</span><span class="sxs-lookup"><span data-stu-id="521b6-297">After configuring an instance of a bundle, the bundle is made available to incoming requests by simply adding it to a global BundleCollection instance.</span></span> <span data-ttu-id="521b6-298">既定のテンプレートでは、バンドルの構成は BundleConfig ファイルで実行されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-298">In the default templates, bundle configuration is performed in a BundleConfig file.</span></span> <span data-ttu-id="521b6-299">この既定の構成は、中核となるスクリプトとテンプレートで使用される css ファイルのすべてのバンドルを作成します。</span><span class="sxs-lookup"><span data-stu-id="521b6-299">This default configuration creates bundles for all of the core scripts and css files used by the templates.</span></span>

<span data-ttu-id="521b6-300">バンドルは、2 つの可能なヘルパー メソッドのいずれかを使用して、ビュー内から参照されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-300">Bundles are referenced from within views by using one of a couple possible helper methods.</span></span> <span data-ttu-id="521b6-301">デバッグとリリース モードでのバンドルの異なるマークアップのレンダリングをサポートするために、ScriptBundle および StyleBundle クラスはレンダリング ヘルパー メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="521b6-301">In order to support rendering different markup for a bundle when in debug vs. release mode, the ScriptBundle and StyleBundle classes have the helper method, Render.</span></span> <span data-ttu-id="521b6-302">デバッグ モードでレンダリングは、バンドル内の各リソースのマークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="521b6-302">When in debug mode, Render will generate markup for each resource in the bundle.</span></span> <span data-ttu-id="521b6-303">リリース モードで Render はバンドル全体を 1 つのマークアップ要素を生成します。</span><span class="sxs-lookup"><span data-stu-id="521b6-303">When in release mode, Render will generate a single markup element for the entire bundle.</span></span> <span data-ttu-id="521b6-304">切り替えてデバッグからリリースまでモードを次に示すように、web.config の compilation 要素の debug 属性を変更することで実現できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-304">Toggling between debug and release mode can be accomplished by modifying the debug attribute of the compilation element in web.config as shown below:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

<span data-ttu-id="521b6-305">さらに、有効化または最適化を無効にするは、BundleTable.EnableOptimizations プロパティ経由で直接設定できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-305">Additionally, enabling or disabling optimization can be set directly via the BundleTable.EnableOptimizations property.</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

<span data-ttu-id="521b6-306">ファイルがバンドルされているときにまず並べ替えのアルファベット順 (に表示されている方法は、**ソリューション エクスプ ローラー**)。</span><span class="sxs-lookup"><span data-stu-id="521b6-306">When files are bundled, they are first sorted alphabetically (the way they are displayed in **Solution Explorer**).</span></span> <span data-ttu-id="521b6-307">ライブラリが認識されるため、構成されると、(jQuery、MooTools、Dojo など) にカスタムの拡張機能は最初に読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="521b6-307">They are then organized so that known libraries and their custom extensions (such as jQuery, MooTools, and Dojo) are loaded first.</span></span> <span data-ttu-id="521b6-308">たとえば、上記のように、Scripts フォルダーをバンドルするため、最終的な順序になります。</span><span class="sxs-lookup"><span data-stu-id="521b6-308">For example, the final order for the bundling of the Scripts folder as shown above will be:</span></span>

1. <span data-ttu-id="521b6-309">jquery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="521b6-309">jquery-1.6.2.js</span></span>
2. <span data-ttu-id="521b6-310">jquery-ui.js</span><span class="sxs-lookup"><span data-stu-id="521b6-310">jquery-ui.js</span></span>
3. <span data-ttu-id="521b6-311">jquery.tools.js</span><span class="sxs-lookup"><span data-stu-id="521b6-311">jquery.tools.js</span></span>
4. <span data-ttu-id="521b6-312">a.js</span><span class="sxs-lookup"><span data-stu-id="521b6-312">a.js</span></span>

<span data-ttu-id="521b6-313">CSS ファイルもアルファベット順に並べ替えるし、reset.css と normalize.css が、他のファイルの前にできるようにし、再編成します。</span><span class="sxs-lookup"><span data-stu-id="521b6-313">CSS files are also sorted alphabetically and then reorganized so that reset.css and normalize.css come before any other file.</span></span> <span data-ttu-id="521b6-314">前に示した、Styles フォルダーのバンドルの最終的な並べ替えを行うと、これになります。</span><span class="sxs-lookup"><span data-stu-id="521b6-314">The final sorting of the bundling of the Styles folder shown above will be this:</span></span>

1. <span data-ttu-id="521b6-315">reset.css</span><span class="sxs-lookup"><span data-stu-id="521b6-315">reset.css</span></span>
2. <span data-ttu-id="521b6-316">content.css</span><span class="sxs-lookup"><span data-stu-id="521b6-316">content.css</span></span>
3. <span data-ttu-id="521b6-317">forms.css</span><span class="sxs-lookup"><span data-stu-id="521b6-317">forms.css</span></span>
4. <span data-ttu-id="521b6-318">globals.css</span><span class="sxs-lookup"><span data-stu-id="521b6-318">globals.css</span></span>
5. <span data-ttu-id="521b6-319">menu.css</span><span class="sxs-lookup"><span data-stu-id="521b6-319">menu.css</span></span>
6. <span data-ttu-id="521b6-320">styles.css</span><span class="sxs-lookup"><span data-stu-id="521b6-320">styles.css</span></span>

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a><span data-ttu-id="521b6-321">Web ホストのパフォーマンスの向上</span><span class="sxs-lookup"><span data-stu-id="521b6-321">Performance Improvements for Web Hosting</span></span>

<span data-ttu-id="521b6-322">.NET Framework 4.5 および Windows 8 は、web サーバーのワークロードの大幅なパフォーマンス向上を実現するのに役立つ機能を紹介します。</span><span class="sxs-lookup"><span data-stu-id="521b6-322">The .NET Framework 4.5 and Windows 8 introduce features that can help you achieve a significant performance boost for web-server workloads.</span></span> <span data-ttu-id="521b6-323">軽減が含まれます (最大 35%) 両方の起動時間と ASP.NET を使用するサイトをホストしている web のメモリ使用量。</span><span class="sxs-lookup"><span data-stu-id="521b6-323">This includes a reduction (up to 35%) in both startup time and in the memory footprint of web hosting sites that use ASP.NET.</span></span>

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a><span data-ttu-id="521b6-324">主要なパフォーマンスの要因</span><span class="sxs-lookup"><span data-stu-id="521b6-324">Key performance factors</span></span>

<span data-ttu-id="521b6-325">理想的には、すべての web サイトをアクティブにする必要があり、次の要求に迅速な応答を保証するために、メモリ内たびになった。</span><span class="sxs-lookup"><span data-stu-id="521b6-325">Ideally, all websites should be active and in memory to assure quick response to the next request, whenever it comes.</span></span> <span data-ttu-id="521b6-326">サイトの応答性に影響を与える要因は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="521b6-326">Factors that can affect site responsiveness include:</span></span>

- <span data-ttu-id="521b6-327">サイト、アプリ プールのリサイクル後に再起動するためにかかる時間。</span><span class="sxs-lookup"><span data-stu-id="521b6-327">The time it takes for a site to restart after an app pool recycles.</span></span> <span data-ttu-id="521b6-328">これは、サイトのアセンブリがメモリ内に不要になった場合に、サイトの web サーバー プロセスの起動にかかる時間です。</span><span class="sxs-lookup"><span data-stu-id="521b6-328">This is the time it takes to launch a web server process for the site when the site assemblies are no longer in memory.</span></span> <span data-ttu-id="521b6-329">(プラットフォーム アセンブリは、メモリ内の他のサイトで使用されているため) です。このような状況は「コールド サイト、フレームワークのウォーム起動」と呼ばれますだけ"コールド サイトまたはスタートアップ"。</span><span class="sxs-lookup"><span data-stu-id="521b6-329">(The platform assemblies are still in memory, since they are used by other sites.) This situation is referred to as "cold site, warm framework startup" or just "cold site startup."</span></span>
- <span data-ttu-id="521b6-330">サイトが占有するメモリの量。</span><span class="sxs-lookup"><span data-stu-id="521b6-330">How much memory the site occupies.</span></span> <span data-ttu-id="521b6-331">この用語は「サイトごとのメモリ使用量」または「ワーキング セットを共有します」</span><span class="sxs-lookup"><span data-stu-id="521b6-331">Terms for this are "per-site memory consumption" or "unshared working set."</span></span>

<span data-ttu-id="521b6-332">新しいパフォーマンスの向上は、これらの要因の両方に注目します。</span><span class="sxs-lookup"><span data-stu-id="521b6-332">The new performance improvements focus on both of these factors.</span></span>

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a><span data-ttu-id="521b6-333">新しいパフォーマンス機能の要件</span><span class="sxs-lookup"><span data-stu-id="521b6-333">Requirements for New Performance Features</span></span>

<span data-ttu-id="521b6-334">新機能に関する要件は、これらのカテゴリに分けることができます。</span><span class="sxs-lookup"><span data-stu-id="521b6-334">The requirements for the new features can be broken down into these categories:</span></span>

- <span data-ttu-id="521b6-335">.NET Framework 4 で実行される機能を強化します。</span><span class="sxs-lookup"><span data-stu-id="521b6-335">Improvements that run on the .NET Framework 4.</span></span>
- <span data-ttu-id="521b6-336">.NET Framework 4.5 を必要とするが、任意のバージョンの Windows で実行できる機能を強化します。</span><span class="sxs-lookup"><span data-stu-id="521b6-336">Improvements that require the .NET Framework 4.5 but can run on any version of Windows.</span></span>
- <span data-ttu-id="521b6-337">Windows 8 で実行されている .NET Framework 4.5 でのみ利用できる機能を強化します。</span><span class="sxs-lookup"><span data-stu-id="521b6-337">Improvements that are available only with .NET Framework 4.5 running on Windows 8.</span></span>

<span data-ttu-id="521b6-338">各レベルを有効にすることができる改善のパフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="521b6-338">Performance increases with each level of improvement that you are able to enable.</span></span>

<span data-ttu-id="521b6-339">他のシナリオに適用される広範なパフォーマンス機能の .NET Framework 4.5 の機能強化の一部を利用します。</span><span class="sxs-lookup"><span data-stu-id="521b6-339">Some of the .NET Framework 4.5 improvements take advantage of broader performance features that apply to other scenarios as well.</span></span>

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a><span data-ttu-id="521b6-340">一般的なアセンブリの共有</span><span class="sxs-lookup"><span data-stu-id="521b6-340">Sharing Common Assemblies</span></span>

<span data-ttu-id="521b6-341">**要件**: .NET Framework 4 および Visual Studio 11 Developer Preview SDK</span><span class="sxs-lookup"><span data-stu-id="521b6-341">**Requirement**: .NET Framework 4 and Visual Studio 11 Developer Preview SDK</span></span>

<span data-ttu-id="521b6-342">サーバー上の別のサイトは、多くの場合、同じヘルパー アセンブリ (たとえば、スターター キットまたはサンプル アプリケーションからのアセンブリ) を使用します。</span><span class="sxs-lookup"><span data-stu-id="521b6-342">Different sites on a server often use the same helper assemblies (for example, assemblies from a starter kit or sample application).</span></span> <span data-ttu-id="521b6-343">各サイトの Bin ディレクトリにこれらのアセンブリのコピーには。</span><span class="sxs-lookup"><span data-stu-id="521b6-343">Each site has its own copy of these assemblies in its Bin directory.</span></span> <span data-ttu-id="521b6-344">アセンブリのオブジェクト コードは同じですが、物理的に独立したアセンブリは、各アセンブリは、サイトのコールド起動時に個別に読み込む必要がありますので、メモリとは別に保持されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-344">Even though the object code for the assemblies is identical, they're physically separate assemblies, so each assembly has to be read separately during cold site startup and kept separately in memory.</span></span>

<span data-ttu-id="521b6-345">新しい interning 機能がこの非効率性を解決し、RAM の要件と負荷の両方を削減時間。</span><span class="sxs-lookup"><span data-stu-id="521b6-345">The new interning functionality solves this inefficiency and reduces both RAM requirements and load time.</span></span> <span data-ttu-id="521b6-346">インターンにより Windows ファイル システムでは、各アセンブリの 1 つのコピーを保持して、サイトの Bin フォルダーに個別のアセンブリは 1 つのコピーへのシンボリック リンクに置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="521b6-346">Interning lets Windows keep a single copy of each assembly in the file system, and individual assemblies in the site Bin folders are replaced with symbolic links to the single copy.</span></span> <span data-ttu-id="521b6-347">個々 のサイトでは、アセンブリの異なるバージョンを必要とする場合、シンボリック リンクは、アセンブリの新しいバージョンに置き換え、対象のサイトだけが影響を受けます。</span><span class="sxs-lookup"><span data-stu-id="521b6-347">If an individual site needs a distinct version of the assembly, the symbolic link is replaced by the new version of the assembly, and only that site is affected.</span></span>

<span data-ttu-id="521b6-348">シンボリック リンクを使用してアセンブリを共有するには、aspnet という名前の新しいツールが必要です。\_intern.exe を作成し、隔離されたアセンブリのストアを管理できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-348">Sharing assemblies using symbolic links requires a new tool named aspnet\_intern.exe, which lets you create and manage the store of interned assemblies.</span></span> <span data-ttu-id="521b6-349">Visual Studio 11 Developer Preview SDK の一部として提供されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-349">It is provided as a part of the Visual Studio 11 Developer Preview SDK.</span></span> <span data-ttu-id="521b6-350">(ただし、のみ、.NET Framework 4 がインストールされた、最新版がインストールされていると仮定すると、システムの機能は[更新](https://support.microsoft.com/kb/2468871))。</span><span class="sxs-lookup"><span data-stu-id="521b6-350">(However, it will work on a system that has only the .NET Framework 4 installed, assuming you have installed the latest [update](https://support.microsoft.com/kb/2468871).)</span></span>

<span data-ttu-id="521b6-351">Aspnet を実行するすべての対象となるアセンブリが確保されていることを確認するには、\_intern.exe 定期的に (たとえば、スケジュールされたタスクとして毎週 1 回)。</span><span class="sxs-lookup"><span data-stu-id="521b6-351">To make sure all eligible assemblies have been interned, you run aspnet\_intern.exe periodically (for example, once a week as a scheduled task).</span></span> <span data-ttu-id="521b6-352">一般的な用途は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="521b6-352">A typical use is as follows:</span></span>

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

<span data-ttu-id="521b6-353">すべてのオプションを表示するには、引数なしでツールを実行します。</span><span class="sxs-lookup"><span data-stu-id="521b6-353">To see all options, run the tool with no arguments.</span></span>

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a><span data-ttu-id="521b6-354">マルチコア JIT コンパイルを使用して、すばやい起動時間の</span><span class="sxs-lookup"><span data-stu-id="521b6-354">Using multi-Core JIT compilation for faster startup</span></span>

<span data-ttu-id="521b6-355">**要件**: .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="521b6-355">**Requirement**: .NET Framework 4.5</span></span>

<span data-ttu-id="521b6-356">コールド サイトの起動時では、アセンブリは、ディスクから読み取る必要はだけでなくが、サイトは、JIT コンパイルする必要があります。</span><span class="sxs-lookup"><span data-stu-id="521b6-356">For a cold site startup, not only do assemblies have to be read from disk, but the site must be JIT-compiled.</span></span> <span data-ttu-id="521b6-357">複雑なサイトでは、これには、大幅な遅延を追加します。</span><span class="sxs-lookup"><span data-stu-id="521b6-357">For a complex site, this can add significant delays.</span></span> <span data-ttu-id="521b6-358">.NET Framework 4.5 では新しい汎用的な手法では、JIT コンパイルを利用可能なプロセッサ コアに分散させることでこれらの遅延が軽減されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-358">A new general-purpose technique in the .NET Framework 4.5 reduces these delays by spreading JIT-compilation across available processor cores.</span></span> <span data-ttu-id="521b6-359">これは多くの部分、し、できるだけ早い段階中に収集された情報を使用して以前起動サイトのします。</span><span class="sxs-lookup"><span data-stu-id="521b6-359">It does this as much and as early as possible by using information gathered during previous launches of the site.</span></span> <span data-ttu-id="521b6-360">この機能によって実装される、 [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx)メソッド。</span><span class="sxs-lookup"><span data-stu-id="521b6-360">This functionality implemented by the [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) method.</span></span>

<span data-ttu-id="521b6-361">ASP.NET では、既定では複数のコアを使用しての JIT コンパイルが有効になっているため、この機能を活用するために何もする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="521b6-361">JIT-compiling using multiple cores is enabled by default in ASP.NET, so you do not need to do anything to take advantage of this feature.</span></span> <span data-ttu-id="521b6-362">この機能を無効にする場合は、Web.config ファイルで、次の設定を確認します。</span><span class="sxs-lookup"><span data-stu-id="521b6-362">If you want to disable this feature, make the following setting in the Web.config file:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a><span data-ttu-id="521b6-363">チューニングのガベージ コレクションのメモリを最適化するには</span><span class="sxs-lookup"><span data-stu-id="521b6-363">Tuning garbage collection to optimize for memory</span></span>

<span data-ttu-id="521b6-364">**要件**: .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="521b6-364">**Requirement**: .NET Framework 4.5</span></span>

<span data-ttu-id="521b6-365">サイトが実行されている場合、ガベージ コレクター (GC) ヒープの使用、メモリの消費の重要な要素を使用できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-365">Once a site is running, its use of the garbage-collector (GC) heap can be a significant factor in its memory consumption.</span></span> <span data-ttu-id="521b6-366">任意のガベージ コレクターのように、.NET Framework GC では CPU 時間 (頻度とコレクションの有意性) と (新しい、解放された、または解放できるオブジェクトに使用される余分なスペース) のメモリ消費量のバランス。</span><span class="sxs-lookup"><span data-stu-id="521b6-366">Like any garbage collector, the .NET Framework GC makes tradeoffs between CPU time (frequency and significance of collections) and memory consumption (extra space that is used for new, freed, or free-able objects).</span></span> <span data-ttu-id="521b6-367">適切なバランスを実現するために GC を構成する方法のガイダンスを提供して、以前のリリース (たとえばを参照してください[ASP.NET 2.0 および 3.5 共有をホストしている構成](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration))。</span><span class="sxs-lookup"><span data-stu-id="521b6-367">For previous releases, we have provided guidance on how to configure the GC to achieve the right balance (for example, see [ASP.NET 2.0/3.5 Shared Hosting Configuration](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).</span></span>

<span data-ttu-id="521b6-368">サイトの追加のパフォーマンスを提供するすべての GC の以前の推奨設定だけでなく、新しいチューニングできるようにする複数のスタンドアロン設定、ワークロード固有の構成設定ではなく、.NET Framework 4.5 を使用できます。ワーキング セット。</span><span class="sxs-lookup"><span data-stu-id="521b6-368">For the .NET Framework 4.5, instead of multiple standalone settings, a workload-defined configuration setting is available that enables all of the previously recommended GC settings as well as new tuning that delivers additional performance for the per-site working set.</span></span>

<span data-ttu-id="521b6-369">GC メモリ調整を有効にするには、Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config ファイルに次の設定を追加します。</span><span class="sxs-lookup"><span data-stu-id="521b6-369">To enable GC memory tuning, add the following setting to the Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config file:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

<span data-ttu-id="521b6-370">(変更前のガイダンス aspnet.config を使い慣れている場合、は、この設定により、以前の設定を置き換えられることに注意してください。 — たとえば、gcServer、gcConcurrent などを設定する必要はありません。されませんが、以前の設定を削除する。)</span><span class="sxs-lookup"><span data-stu-id="521b6-370">(If you're familiar with the previous guidance for changes to aspnet.config, note that this setting replaces the old settings — for example, there is no need to set gcServer, gcConcurrent, etc. You do not have to remove the old settings.)</span></span>

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a><span data-ttu-id="521b6-371">Web アプリケーションのプリフェッチ</span><span class="sxs-lookup"><span data-stu-id="521b6-371">Prefetching for web applications</span></span>

<span data-ttu-id="521b6-372">**要件**: Windows 8 で実行されている .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="521b6-372">**Requirement**: .NET Framework 4.5 running on Windows 8</span></span>

<span data-ttu-id="521b6-373">Windows には複数のリリースと呼ばれるテクノロジが含まれて、[プリフェッチャ](http://en.wikipedia.org/wiki/Prefetcher)アプリケーションの起動時のディスク読み取りのコストを減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="521b6-373">For several releases, Windows has included a technology known as the [prefetcher](http://en.wikipedia.org/wiki/Prefetcher) that reduces the disk-read cost of application startup.</span></span> <span data-ttu-id="521b6-374">コールド起動はクライアント アプリケーションの大部分の問題であるため、このテクノロジいないサーバーに不可欠なコンポーネントのみを含む Windows Server に含まれています。</span><span class="sxs-lookup"><span data-stu-id="521b6-374">Because cold startup is a problem predominantly for client applications, this technology has not been included in Windows Server, which includes only components that are essential to a server.</span></span> <span data-ttu-id="521b6-375">プリフェッチは、個々 の web サイトの起動を最適化できます、Windows Server の最新バージョンで使用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="521b6-375">Prefetching is now available in the latest version of Windows Server, where it can optimize the launch of individual websites.</span></span>

<span data-ttu-id="521b6-376">Windows server では、既定では、プリフェッチャは有効になっていません。</span><span class="sxs-lookup"><span data-stu-id="521b6-376">For Windows Server, the prefetcher is not enabled by default.</span></span> <span data-ttu-id="521b6-377">有効にし、高密度の web ホスティングのプリフェッチャを構成には、コマンドラインで次の一連のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="521b6-377">To enable and configure the prefetcher for high-density web hosting, run the following set of commands at the command line:</span></span>

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

<span data-ttu-id="521b6-378">次に、ASP.NET アプリケーションでプリフェッチャを統合するには、Web.config ファイルに、次のように追加します。</span><span class="sxs-lookup"><span data-stu-id="521b6-378">Then, to integrate the prefetcher with ASP.NET applications, add the following to the Web.config file:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a><span data-ttu-id="521b6-379">ASP.NET Web フォーム</span><span class="sxs-lookup"><span data-stu-id="521b6-379">ASP.NET Web Forms</span></span>

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a><span data-ttu-id="521b6-380">厳密に型指定されたデータ コントロール</span><span class="sxs-lookup"><span data-stu-id="521b6-380">Strongly Typed Data Controls</span></span>

<span data-ttu-id="521b6-381">ASP.NET 4.5 には Web フォームには、データを操作するためのいくつかの機能強化が含まれています。</span><span class="sxs-lookup"><span data-stu-id="521b6-381">In ASP.NET 4.5, Web Forms includes some improvements for working with data.</span></span> <span data-ttu-id="521b6-382">最初の向上は、厳密に型指定されたデータ コントロールです。</span><span class="sxs-lookup"><span data-stu-id="521b6-382">The first improvement is strongly typed data controls.</span></span> <span data-ttu-id="521b6-383">使用してデータ バインドされた値を表示する前のバージョンの ASP.NET Web フォーム コントロールの*Eval*とデータ バインディング式。</span><span class="sxs-lookup"><span data-stu-id="521b6-383">For Web Forms controls in previous versions of ASP.NET, you display a data-bound value using *Eval* and a data-binding expression:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

<span data-ttu-id="521b6-384">使用する双方向データ バインド、*バインド*:</span><span class="sxs-lookup"><span data-stu-id="521b6-384">For two-way data binding, you use *Bind*:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

<span data-ttu-id="521b6-385">、実行時に、これらの呼び出しは、リフレクションを使用して、指定されたメンバーの値を読み取るし、マークアップで結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-385">At run time, these calls use reflection to read the value of the specified member and then display the result in the markup.</span></span> <span data-ttu-id="521b6-386">この方法を簡単に、自由に unshaped データに対してデータをバインドします。</span><span class="sxs-lookup"><span data-stu-id="521b6-386">This approach makes it easy to data bind against arbitrary, unshaped data.</span></span>

<span data-ttu-id="521b6-387">ただし、このようなデータ バインディング式では、メンバー名 (定義へ移動) などのナビゲーションやコンパイル時にこれらの名前のチェックの IntelliSense などの機能をサポートしません。</span><span class="sxs-lookup"><span data-stu-id="521b6-387">However, data-binding expressions like this don't support features like IntelliSense for member names, navigation (like Go To Definition), or compile-time checking for these names.</span></span>

<span data-ttu-id="521b6-388">この問題を解決するには、ASP.NET 4.5 では、コントロールにバインドされたデータのデータ型を宣言する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="521b6-388">To address this issue, ASP.NET 4.5 adds the ability to declare the data type of the data that a control is bound to.</span></span> <span data-ttu-id="521b6-389">これを行う新しい*ItemType*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="521b6-389">You do this using the new *ItemType* property.</span></span> <span data-ttu-id="521b6-390">2 つの新しい型指定された変数は、データ バインディング式のスコープ内で使用可能なこのプロパティを設定すると:*項目*と*BindItem*します。</span><span class="sxs-lookup"><span data-stu-id="521b6-390">When you set this property, two new typed variables are available in the scope of data-binding expressions: *Item* and *BindItem*.</span></span> <span data-ttu-id="521b6-391">変数は厳密に型指定、ため、Visual Studio の開発エクスペリエンスのすべてのメリットを取得します。</span><span class="sxs-lookup"><span data-stu-id="521b6-391">Because the variables are strongly typed, you get the full benefits of the Visual Studio development experience.</span></span>


<span data-ttu-id="521b6-392">双方向のデータ バインディング式を使用して、 *BindItem*変数。</span><span class="sxs-lookup"><span data-stu-id="521b6-392">For two-way data-binding expressions, use the *BindItem* variable:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


<span data-ttu-id="521b6-393">データ バインディングをサポートする ASP.NET Web フォーム フレームワークのほとんどのコントロールがサポートするために更新されました、 *ItemType*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="521b6-393">Most controls in the ASP.NET Web Forms framework that support data binding have been updated to support the *ItemType* property.</span></span>

<a id="_Toc318097387"></a>
### <a name="model-binding"></a><span data-ttu-id="521b6-394">モデル バインド</span><span class="sxs-lookup"><span data-stu-id="521b6-394">Model Binding</span></span>

<span data-ttu-id="521b6-395">モデル バインドでは、コードに重点を置いたデータ アクセスを使用する ASP.NET Web フォーム コントロールでのデータ バインディングを拡張します。</span><span class="sxs-lookup"><span data-stu-id="521b6-395">Model binding extends data binding in ASP.NET Web Forms controls to work with code-focused data access.</span></span> <span data-ttu-id="521b6-396">概念が組み込まれて、 *ObjectDataSource*コントロールと ASP.NET mvc モデル バインディングから。</span><span class="sxs-lookup"><span data-stu-id="521b6-396">It incorporates concepts from the *ObjectDataSource* control and from model binding in ASP.NET MVC.</span></span>

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a><span data-ttu-id="521b6-397">データの選択</span><span class="sxs-lookup"><span data-stu-id="521b6-397">Selecting data</span></span>

<span data-ttu-id="521b6-398">モデル バインドを使用してデータを選択するデータ コントロールを構成するには、コントロールを設定する*SelectMethod*プロパティ ページのコード内のメソッドの名前にします。</span><span class="sxs-lookup"><span data-stu-id="521b6-398">To configure a data control to use model binding to select data, you set the control's *SelectMethod* property to the name of a method in the page's code.</span></span> <span data-ttu-id="521b6-399">データ コントロールでは、ページのライフ サイクルの適切なタイミングでメソッドを呼び出すし、自動的に返されるデータをバインドします。</span><span class="sxs-lookup"><span data-stu-id="521b6-399">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="521b6-400">明示的に呼び出す必要はありません、 *DataBind*メソッド。</span><span class="sxs-lookup"><span data-stu-id="521b6-400">There's no need to explicitly call the *DataBind* method.</span></span>

<span data-ttu-id="521b6-401">次の例では、 *GridView*という名前のメソッドを使用するコントロールが構成されている*GetCategories*:</span><span class="sxs-lookup"><span data-stu-id="521b6-401">In the following example, the *GridView* control is configured to use a method named *GetCategories*:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

<span data-ttu-id="521b6-402">作成する、 *GetCategories*ページのコード内のメソッド。</span><span class="sxs-lookup"><span data-stu-id="521b6-402">You create the *GetCategories* method in the page's code.</span></span> <span data-ttu-id="521b6-403">単純な select 操作のメソッド パラメーターは必要ありませんし返す必要があります、 *IEnumerable*または*IQueryable*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="521b6-403">For a simple select operation, the method needs no parameters and should return an *IEnumerable* or *IQueryable* object.</span></span> <span data-ttu-id="521b6-404">場合、新しい*ItemType*プロパティが設定されます (下で説明したようにできるが、データ バインド式に厳密に型指定[データ コントロールを厳密に型指定された](#_Toc318097386)前)、これらのインターフェイスのジェネリック バージョン返される必要があります: *IEnumerable&lt;T&gt;* または*IQueryable&lt;T&gt;* で、 *T*パラメーターの型と一致する、 *ItemType*プロパティ (たとえば、 *IQueryable&lt;カテゴリ&gt;*)。</span><span class="sxs-lookup"><span data-stu-id="521b6-404">If the new *ItemType* property is set (which enables strongly typed data-binding expressions, as explained under [Strongly Typed Data Controls](#_Toc318097386) earlier), the generic versions of these interfaces should be returned — *IEnumerable&lt;T&gt;* or *IQueryable&lt;T&gt;*, with the *T* parameter matching the type of the *ItemType* property (for example, *IQueryable&lt;Category&gt;*).</span></span>

<span data-ttu-id="521b6-405">次の例のコードを示しています、 *GetCategories*メソッド。</span><span class="sxs-lookup"><span data-stu-id="521b6-405">The following example shows the code for a *GetCategories* method.</span></span> <span data-ttu-id="521b6-406">この例では、Northwind サンプル データベースで Entity Framework Code First モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="521b6-406">This example uses the Entity Framework Code First model with the Northwind sample database.</span></span> <span data-ttu-id="521b6-407">コードは、クエリが、関連する製品での各カテゴリの詳細を返すことを確認して、 *Include*メソッド。</span><span class="sxs-lookup"><span data-stu-id="521b6-407">The code makes sure that the query returns details of the related products for each category by way of the *Include* method.</span></span> <span data-ttu-id="521b6-408">(これにより、 *TemplateField*マークアップ内の要素では、各カテゴリの製品の数を表示を必要とせず、 [n + 1 選択](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem))。</span><span class="sxs-lookup"><span data-stu-id="521b6-408">(This ensures that the *TemplateField* element in the markup displays the count of products in each category without requiring an [n+1 select](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

<span data-ttu-id="521b6-409">ページが実行すると、 *GridView*の制御呼び出し、 *GetCategories*メソッドに自動的に構成されているフィールドを使用して、返されるデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="521b6-409">When the page runs, the *GridView* control calls the *GetCategories* method automatically and renders the returned data using the configured fields:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

<span data-ttu-id="521b6-410">Select メソッドを返すため、 *IQueryable*オブジェクト、 *GridView*コントロールが実行する前に、クエリをさらに細かく操作できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-410">Because the select method returns an *IQueryable* object, the *GridView* control can further manipulate the query before executing it.</span></span> <span data-ttu-id="521b6-411">たとえば、 *GridView*コントロールの並べ替えとページングに返されたクエリ式を追加できます*IQueryable*が実行される前にオブジェクトの基になるによってこれらの操作が実行されるようにLINQ プロバイダーです。</span><span class="sxs-lookup"><span data-stu-id="521b6-411">For example, the *GridView* control can add query expressions for sorting and paging to the returned *IQueryable* object before it is executed, so that those operations are performed by the underlying LINQ provider.</span></span> <span data-ttu-id="521b6-412">ここでは、Entity Framework では、これらの操作がデータベースで実行される保証されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-412">In this case, Entity Framework will ensure those operations are performed in the database.</span></span>

<span data-ttu-id="521b6-413">次の例は、 *GridView*コントロールの並べ替えとページングを許可するように変更します。</span><span class="sxs-lookup"><span data-stu-id="521b6-413">The following example shows the *GridView* control modified to allow sorting and paging:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

<span data-ttu-id="521b6-414">これで、ページを実行する際は、コントロールがデータの現在のページのみが表示されることと、選択した列順序付けがあることを確認してようにできます。</span><span class="sxs-lookup"><span data-stu-id="521b6-414">Now when the page runs, the control can make sure that only the current page of data is displayed and that it's ordered by the selected column:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

<span data-ttu-id="521b6-415">返されたデータをフィルター処理、パラメーターを select メソッドに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="521b6-415">To filter the returned data, parameters have to be added to the select method.</span></span> <span data-ttu-id="521b6-416">これらのパラメーターは、実行時にモデル バインディングに設定して、それらを使用して、データを返す前にクエリを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="521b6-416">These parameters will be populated by the model binding at run time, and you can use them to alter the query before returning the data.</span></span>

<span data-ttu-id="521b6-417">たとえば、クエリ文字列のキーワードを入力して、ユーザー フィルター製品を使用するとします。</span><span class="sxs-lookup"><span data-stu-id="521b6-417">For example, assume that you want to let users filter products by entering a keyword in the query string.</span></span> <span data-ttu-id="521b6-418">メソッドにパラメーターを追加およびパラメーターの値を使用するコードを更新できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-418">You can add a parameter to the method and update the code to use the parameter value:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

<span data-ttu-id="521b6-419">このコードが含まれています、*場所*式の値が指定されている場合*キーワード*し、クエリの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="521b6-419">This code includes a *Where* expression if a value is provided for *keyword* and then returns the query results.</span></span>

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a><span data-ttu-id="521b6-420">値プロバイダー</span><span class="sxs-lookup"><span data-stu-id="521b6-420">Value providers</span></span>

<span data-ttu-id="521b6-421">前の例が場所に関する特定の値、*キーワード*からパラメーターが登場します。</span><span class="sxs-lookup"><span data-stu-id="521b6-421">The previous example was not specific about where the value for the *keyword* parameter was coming from.</span></span> <span data-ttu-id="521b6-422">この情報を示すためには、パラメーターの属性を使用できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-422">To indicate this information, you can use a parameter attribute.</span></span> <span data-ttu-id="521b6-423">この例で使用することができます、 *QueryStringAttribute*クラス内にある、 *System.Web.ModelBinding*名前空間。</span><span class="sxs-lookup"><span data-stu-id="521b6-423">For this example, you can use the *QueryStringAttribute* class that's in the *System.Web.ModelBinding* namespace:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

<span data-ttu-id="521b6-424">これに指示するクエリ文字列から値をバインドしようとするモデル バインド、*キーワード*実行時にパラメーター。</span><span class="sxs-lookup"><span data-stu-id="521b6-424">This instructs model binding to try to bind a value from the query string to the *keyword* parameter at run time.</span></span> <span data-ttu-id="521b6-425">(これを伴う可能性が型の変換を実行するが、ここではない。)値を指定することはできません、型が null 非許容の場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="521b6-425">(This might involve performing type conversion, although it doesn't in this case.) If a value cannot be provided and the type is non-nullable, an exception is thrown.</span></span>

<span data-ttu-id="521b6-426">これらのメソッドの値のソース値のプロバイダーとして参照され、使用する値プロバイダーを示すパラメーターの属性の値プロバイダー属性として参照されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-426">The sources of values for these methods are referred to as value providers, and the parameter attributes that indicate which value provider to use are referred to as value provider attributes.</span></span> <span data-ttu-id="521b6-427">Web フォームには、クエリ文字列、cookie、フォーム値、コントロール、ビュー ステート、セッション状態、およびプロファイルのプロパティなどの Web フォーム アプリケーションで値プロバイダーとすべてのユーザー入力の一般的なソースの対応する属性が含まれます。</span><span class="sxs-lookup"><span data-stu-id="521b6-427">Web Forms will include value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application, such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="521b6-428">カスタム値プロバイダーを記述することもできます。</span><span class="sxs-lookup"><span data-stu-id="521b6-428">You can also write custom value providers.</span></span>

<span data-ttu-id="521b6-429">既定では、パラメーター名は、値プロバイダーのコレクションの値を検索するキーとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-429">By default, the parameter name is used as the key to find a value in the value provider collection.</span></span> <span data-ttu-id="521b6-430">キーワードをという名前のクエリ文字列値の検索は、コード例では、(たとえば、~/default.aspx?keyword=chef)。</span><span class="sxs-lookup"><span data-stu-id="521b6-430">In the example, the code will look for a query-string value named keyword (for example, ~/default.aspx?keyword=chef).</span></span> <span data-ttu-id="521b6-431">パラメーター属性に引数として渡すことによって、カスタムのキーを指定できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-431">You can specify a custom key by passing it as an argument to the parameter attribute.</span></span> <span data-ttu-id="521b6-432">たとえば、q をという名前のクエリ文字列変数の値を使用することがこれを行います。</span><span class="sxs-lookup"><span data-stu-id="521b6-432">For example, to use the value of the query-string variable named q, you could do this:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

<span data-ttu-id="521b6-433">このメソッドがページのコードの場合は、ユーザーは、クエリ文字列を使用してキーワードを渡すことによって、結果をフィルター処理できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-433">If this method is in the page's code, users can filter the results by passing a keyword using the query string:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

<span data-ttu-id="521b6-434">モデル バインドを手動でコーディングする必要がありますそれ以外の場合、多くのタスクを実行します値を読み取る、の null 値の確認、適切な型に変換しようとして、変換が成功したかどうかをチェックおよび最後に、の値を使用して、。クエリ。</span><span class="sxs-lookup"><span data-stu-id="521b6-434">Model binding accomplishes many tasks that you would otherwise have to code by hand: reading the value, checking for a null value, attempting to convert it to the appropriate type, checking whether the conversion was successful, and finally, using the value in the query.</span></span> <span data-ttu-id="521b6-435">モデルのはるかに少ないコードでは、アプリケーション全体で機能を再利用できるように、結果をバインドします。</span><span class="sxs-lookup"><span data-stu-id="521b6-435">Model binding results in far less code and in the ability to reuse the functionality throughout your application.</span></span>

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a><span data-ttu-id="521b6-436">コントロールの値によってフィルター処理</span><span class="sxs-lookup"><span data-stu-id="521b6-436">Filtering by values from a control</span></span>

<span data-ttu-id="521b6-437">ユーザーがドロップダウン リストからフィルター値を選択できるようにする例を拡張するとします。</span><span class="sxs-lookup"><span data-stu-id="521b6-437">Suppose you want to extend the example to let the user choose a filter value from a drop-down list.</span></span> <span data-ttu-id="521b6-438">マークアップに次のドロップダウン リストを追加してから、もう 1 つのメソッドを使用してそのデータを取得するように構成、 *SelectMethod*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="521b6-438">Add the following drop-down list to the markup and configure it to get its data from another method using the *SelectMethod* property:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

<span data-ttu-id="521b6-439">通常、追加することも、*後*要素を*GridView*コントロール コントロール一致する製品が見つからない場合、メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-439">Typically you would also add an *EmptyDataTemplate* element to the *GridView* control so that the control will display a message if no matching products are found:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

<span data-ttu-id="521b6-440">ページのコードで、新しい選択ドロップダウン リストのメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="521b6-440">In the page code, add the new select method for the drop-down list:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

<span data-ttu-id="521b6-441">最後に、更新、 *GetProducts*ドロップダウン リストから選択したカテゴリの ID を格納する新しいパラメーターを受け取る方法を選択します。</span><span class="sxs-lookup"><span data-stu-id="521b6-441">Finally, update the *GetProducts* select method to take a new parameter that contains the ID of the selected category from the drop-down list:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

<span data-ttu-id="521b6-442">ユーザーがドロップダウン リストからカテゴリを選択、ページの実行時にこれで、 *GridView*コントロールが自動的にフィルター選択されたデータを表示する再バインドします。</span><span class="sxs-lookup"><span data-stu-id="521b6-442">Now when the page runs, users can select a category from the drop-down list, and the *GridView* control is automatically re-bound to show the filtered data.</span></span> <span data-ttu-id="521b6-443">モデル バインドが選択メソッドのパラメーターの値を追跡し、ポストバック後にすべてのパラメーター値が変更されたかどうかを検出したため、可能です。</span><span class="sxs-lookup"><span data-stu-id="521b6-443">This is possible because model binding tracks the values of parameters for select methods and detects whether any parameter value has changed after a postback.</span></span> <span data-ttu-id="521b6-444">場合は、モデル バインドはデータに再バインドに関連付けられているデータ コントロールを強制します。</span><span class="sxs-lookup"><span data-stu-id="521b6-444">If so, model binding forces the associated data control to re-bind to the data.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a><span data-ttu-id="521b6-445">HTML エンコードされたデータ バインディング式</span><span class="sxs-lookup"><span data-stu-id="521b6-445">HTML Encoded Data-Binding Expressions</span></span>

<span data-ttu-id="521b6-446">できますようになりましたの HTML エンコード データ バインディング式の結果。</span><span class="sxs-lookup"><span data-stu-id="521b6-446">You can now HTML-encode the result of data-binding expressions.</span></span> <span data-ttu-id="521b6-447">末尾にコロン (:) を追加、 &lt;%# データ バインディング式を示すプレフィックス。</span><span class="sxs-lookup"><span data-stu-id="521b6-447">Add a colon (:) to the end of the &lt;%# prefix that marks the data-binding expression:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a><span data-ttu-id="521b6-448">控え目な検証</span><span class="sxs-lookup"><span data-stu-id="521b6-448">Unobtrusive Validation</span></span>

<span data-ttu-id="521b6-449">クライアント側検証ロジックに控え目な JavaScript を使用する組み込みの検証コントロールを構成できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-449">You can now configure the built-in validator controls to use unobtrusive JavaScript for client-side validation logic.</span></span> <span data-ttu-id="521b6-450">これが大幅にインラインで、ページのマークアップでレンダリングを JavaScript の量が減少し、全体的なページ サイズを縮小します。</span><span class="sxs-lookup"><span data-stu-id="521b6-450">This significantly reduces the amount of JavaScript rendered inline in the page markup and reduces the overall page size.</span></span> <span data-ttu-id="521b6-451">控えめな JavaScript の検証コントロールは、次の方法のいずれかで構成できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-451">You can configure unobtrusive JavaScript for validator controls in any of these ways:</span></span>

- <span data-ttu-id="521b6-452">次の設定を追加してグローバルに、 *&lt;appSettings&gt;* Web.config ファイル内の要素。</span><span class="sxs-lookup"><span data-stu-id="521b6-452">Globally by adding the following setting to the *&lt;appSettings&gt;* element in the Web.config file:</span></span> 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- <span data-ttu-id="521b6-453">静的なを設定することによってグローバルに*System.Web.UI.ValidationSettings.UnobtrusiveValidationMode*プロパティを*UnobtrusiveValidationMode.WebForms* (通常、*アプリケーション\_開始*Global.asax ファイル内のメソッド)。</span><span class="sxs-lookup"><span data-stu-id="521b6-453">Globally by setting the static *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* property to *UnobtrusiveValidationMode.WebForms* (typically in the *Application\_Start* method in the Global.asax file).</span></span>
- <span data-ttu-id="521b6-454">新しい設定 ページを個別に*UnobtrusiveValidationMode*のプロパティ、*ページ*クラスを*UnobtrusiveValidationMode.WebForms*します。</span><span class="sxs-lookup"><span data-stu-id="521b6-454">Individually for a page by setting the new *UnobtrusiveValidationMode* property of the *Page* class to *UnobtrusiveValidationMode.WebForms*.</span></span>

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a><span data-ttu-id="521b6-455">HTML5 の更新プログラム</span><span class="sxs-lookup"><span data-stu-id="521b6-455">HTML5 Updates</span></span>

<span data-ttu-id="521b6-456">一部の機能強化が HTML5 の新機能を活用するためにサーバー コントロールを Web フォームに加えられました。</span><span class="sxs-lookup"><span data-stu-id="521b6-456">Some improvements have been made to Web Forms server controls to take advantage of new features of HTML5:</span></span>

- <span data-ttu-id="521b6-457">*TextMode*のプロパティ、 *TextBox*のような新しい HTML5 入力タイプをサポートするためにコントロールが更新された*電子メール*、 *datetime*と具合です。</span><span class="sxs-lookup"><span data-stu-id="521b6-457">The *TextMode* property of the *TextBox* control has been updated to support the new HTML5 input types like *email*, *datetime*, and so on.</span></span>
- <span data-ttu-id="521b6-458">*FileUpload*なりましたこの HTML5 機能をサポートするブラウザーから複数のファイルのアップロードを制御します。</span><span class="sxs-lookup"><span data-stu-id="521b6-458">The *FileUpload* control now supports multiple file uploads from browsers that support this HTML5 feature.</span></span>
- <span data-ttu-id="521b6-459">検証コントロールは、今すぐサポート検証 HTML5 入力要素を制御します。</span><span class="sxs-lookup"><span data-stu-id="521b6-459">Validator controls now support validating HTML5 input elements.</span></span>
- <span data-ttu-id="521b6-460">これで、URL を表す属性を持つ新しい HTML5 の要素をサポートして runat ="server"です。</span><span class="sxs-lookup"><span data-stu-id="521b6-460">New HTML5 elements that have attributes that represent a URL now support runat="server".</span></span> <span data-ttu-id="521b6-461">その結果、規則を使用できます ASP.NET URL のパスのような ~ を表すアプリケーション ルート演算子 (たとえば、&lt;ビデオ runat ="server"src="~/myVideo.wmv"/&gt;)。</span><span class="sxs-lookup"><span data-stu-id="521b6-461">As a result, you can use ASP.NET conventions in URL paths, like the ~ operator to represent the application root (for example, &lt;video runat="server" src="~/myVideo.wmv" /&gt;).</span></span>
- <span data-ttu-id="521b6-462">*UpdatePanel*入力フィールドの転記 HTML5 をサポートするコントロールが修正されました。</span><span class="sxs-lookup"><span data-stu-id="521b6-462">The *UpdatePanel* control has been fixed to support posting HTML5 input fields.</span></span>

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a><span data-ttu-id="521b6-463">ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="521b6-463">ASP.NET MVC 4</span></span>

<span data-ttu-id="521b6-464">ASP.NET MVC 4 Beta が Visual Studio 11 Beta に含まれています。</span><span class="sxs-lookup"><span data-stu-id="521b6-464">ASP.NET MVC 4 Beta is now included with Visual Studio 11 Beta.</span></span> <span data-ttu-id="521b6-465">ASP.NET MVC は、モデル-ビュー-コント ローラー (MVC) パターンを活用することでテスト可能かつ保守が容易な Web アプリケーションを開発するためのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="521b6-465">ASP.NET MVC is a framework for developing highly testable and maintainable Web applications by leveraging the Model-View-Controller (MVC) pattern.</span></span> <span data-ttu-id="521b6-466">ASP.NET MVC 4 では、簡単に、モバイル web アプリケーションを構築することと、任意のデバイスにアクセスできる HTTP サービスの構築に役立つ、ASP.NET Web API が含まれています。</span><span class="sxs-lookup"><span data-stu-id="521b6-466">ASP.NET MVC 4 makes it easy to build applications for the mobile Web and includes ASP.NET Web API, which helps you build HTTP services that can reach any device.</span></span> <span data-ttu-id="521b6-467">詳細については、次を参照してください。、 [ASP.NET MVC 4 リリース ノート](mvc4-release-notes.md)します。</span><span class="sxs-lookup"><span data-stu-id="521b6-467">For more information, see the [ASP.NET MVC 4 Release Notes](mvc4-release-notes.md).</span></span>

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a><span data-ttu-id="521b6-468">ASP.NET Web ページ 2</span><span class="sxs-lookup"><span data-stu-id="521b6-468">ASP.NET Web Pages 2</span></span>

<span data-ttu-id="521b6-469">新機能を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="521b6-469">New features include the following:</span></span>

- <span data-ttu-id="521b6-470">新規および更新されたサイト テンプレートです。</span><span class="sxs-lookup"><span data-stu-id="521b6-470">New and updated site templates.</span></span>
- <span data-ttu-id="521b6-471">サーバー側とクライアント側の検証を使用して追加、*検証*ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="521b6-471">Adding server-side and client-side validation using the *Validation* helper.</span></span>
- <span data-ttu-id="521b6-472">資産マネージャーを使用するスクリプトを登録する権限です。</span><span class="sxs-lookup"><span data-stu-id="521b6-472">The ability to register scripts using an assets manager.</span></span>
- <span data-ttu-id="521b6-473">Facebook と OAuth および OpenID を使用して他のサイトからのログインを有効にします。</span><span class="sxs-lookup"><span data-stu-id="521b6-473">Enabling logins from Facebook and other sites using OAuth and OpenID.</span></span>
- <span data-ttu-id="521b6-474">使用してマップを追加する、*マップ*ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="521b6-474">Adding maps using the *Maps* helper.</span></span>
- <span data-ttu-id="521b6-475">Web ページ アプリケーションでの並列実行されています。</span><span class="sxs-lookup"><span data-stu-id="521b6-475">Running Web Pages applications side-by-side.</span></span>
- <span data-ttu-id="521b6-476">モバイル デバイス用のページをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="521b6-476">Rendering pages for mobile devices.</span></span>

<span data-ttu-id="521b6-477">詳細については、これらの機能とページ全体のコード例は、次を参照してください。 [Web ページ 2 のベータ版のベスト機能](https://go.microsoft.com/fwlink/?LinkID=227824)します。</span><span class="sxs-lookup"><span data-stu-id="521b6-477">For more information about these features and full-page code examples, see [The Top Features in Web Pages 2 Beta](https://go.microsoft.com/fwlink/?LinkID=227824).</span></span>

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a><span data-ttu-id="521b6-478">Visual Web Developer 11 Beta</span><span class="sxs-lookup"><span data-stu-id="521b6-478">Visual Web Developer 11 Beta</span></span>

<span data-ttu-id="521b6-479">このセクションでは、Visual Web Developer の 11 ベータ版および Visual Studio 2012 Release Candidate での web 開発の強化についての情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="521b6-479">This section provides information about improvements for web development in Visual Web Developer 11 Beta and Visual Studio 2012 Release Candidate.</span></span>

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a><span data-ttu-id="521b6-480">Visual Studio 2010 と Visual Studio 2012 リリース候補 (プロジェクト互換性) 間の共有プロジェクト</span><span class="sxs-lookup"><span data-stu-id="521b6-480">Project Sharing Between Visual Studio 2010 and Visual Studio 2012 Release Candidate (Project Compatibility)</span></span>

<span data-ttu-id="521b6-481">Visual Studio 2012 Release Candidate、まで Visual Studio の新しいバージョンで既存のプロジェクトを開くと、変換ウィザードが起動します。</span><span class="sxs-lookup"><span data-stu-id="521b6-481">Until Visual Studio 2012 Release Candidate, opening an existing project in a newer version of Visual Studio launched the Conversion Wizard.</span></span> <span data-ttu-id="521b6-482">これは、でした旧バージョンと互換性のある新しい形式にプロジェクトとソリューションのコンテンツ (資産) のアップグレードを強制します。</span><span class="sxs-lookup"><span data-stu-id="521b6-482">This forced an upgrade of the content (assets) of a project and solution to new formats that were not backward compatible.</span></span> <span data-ttu-id="521b6-483">そのため、変換した後を開けませんでしたプロジェクト Visual Studio の以前のバージョン。</span><span class="sxs-lookup"><span data-stu-id="521b6-483">Therefore, after the conversion you could not open the project in the older version of Visual Studio.</span></span>

<span data-ttu-id="521b6-484">多くのお客様が意見が適切なアプローチでないこと。</span><span class="sxs-lookup"><span data-stu-id="521b6-484">Many customers have told us that this was not the right approach.</span></span> <span data-ttu-id="521b6-485">Visual Studio 11 Beta でサポートされる共有のプロジェクトおよび Visual Studio 2010 SP1 を使用したソリューション。</span><span class="sxs-lookup"><span data-stu-id="521b6-485">In Visual Studio 11 Beta, we now support sharing projects and solutions with Visual Studio 2010 SP1.</span></span> <span data-ttu-id="521b6-486">これは、ことを意味 2010年プロジェクトを Visual Studio 2012 Release Candidate を開く場合は引き続き Visual Studio 2010 SP1 でプロジェクトを開くことができません。</span><span class="sxs-lookup"><span data-stu-id="521b6-486">This means that if you open a 2010 project in Visual Studio 2012 Release Candidate, you will still be able to open the project in Visual Studio 2010 SP1.</span></span>

> [!NOTE]
> <span data-ttu-id="521b6-487">いくつかの種類のプロジェクトは、Visual Studio 2010 SP1 と Visual Studio 2012 リリース候補間で共有できません。</span><span class="sxs-lookup"><span data-stu-id="521b6-487">A few types of projects cannot be shared between Visual Studio 2010 SP1 and Visual Studio 2012 Release Candidate.</span></span> <span data-ttu-id="521b6-488">セットアップ プロジェクトの場合) などの特別な目的の一部 (ASP.NET MVC 2 プロジェクト) などの古いプロジェクトまたはプロジェクトが含まれます。</span><span class="sxs-lookup"><span data-stu-id="521b6-488">These include some older projects (such as ASP.NET MVC 2 projects) or projects for special purposes (such as Setup projects).</span></span>

<span data-ttu-id="521b6-489">Visual Studio 11 Beta で初めての Visual Studio 2010 SP1 の Web プロジェクトを開くと、次のプロパティは、プロジェクト ファイルに追加されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-489">When you open a Visual Studio 2010 SP1 Web project for the first time in Visual Studio 11 Beta, the following properties are added to the project file:</span></span>

- <span data-ttu-id="521b6-490">FileUpgradeFlags</span><span class="sxs-lookup"><span data-stu-id="521b6-490">FileUpgradeFlags</span></span>
- <span data-ttu-id="521b6-491">UpgradeBackupLocation</span><span class="sxs-lookup"><span data-stu-id="521b6-491">UpgradeBackupLocation</span></span>
- <span data-ttu-id="521b6-492">OldToolsVersion</span><span class="sxs-lookup"><span data-stu-id="521b6-492">OldToolsVersion</span></span>
- <span data-ttu-id="521b6-493">VisualStudioVersion</span><span class="sxs-lookup"><span data-stu-id="521b6-493">VisualStudioVersion</span></span>
- <span data-ttu-id="521b6-494">VSToolsPath</span><span class="sxs-lookup"><span data-stu-id="521b6-494">VSToolsPath</span></span>

<span data-ttu-id="521b6-495">FileUpgradeFlags、UpgradeBackupLocation、および OldToolsVersion は、プロジェクト ファイルをアップグレードするプロセスによって使用されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-495">FileUpgradeFlags, UpgradeBackupLocation, and OldToolsVersion are used by the process that upgrades the project file.</span></span> <span data-ttu-id="521b6-496">Visual Studio 2010 でプロジェクトの操作に影響を与えるありません。</span><span class="sxs-lookup"><span data-stu-id="521b6-496">They have no impact on working with the project in Visual Studio 2010.</span></span>

<span data-ttu-id="521b6-497">VisualStudioVersion は、現在のプロジェクトの Visual Studio のバージョンを示す MSBuild 4.5 で使用される新しいプロパティです。</span><span class="sxs-lookup"><span data-stu-id="521b6-497">VisualStudioVersion is a new property used by MSBuild 4.5 that indicates the version of Visual Studio for the current project.</span></span> <span data-ttu-id="521b6-498">MSBuild 4.0 (Visual Studio 2010 SP1 を使用する MSBuild のバージョン) でこのプロパティが存在していないため、プロジェクト ファイルに既定値を挿入します。</span><span class="sxs-lookup"><span data-stu-id="521b6-498">Because this property didn't exist in MSBuild 4.0 (the version of MSBuild that Visual Studio 2010 SP1 uses), we inject a default value into the project file.</span></span>

<span data-ttu-id="521b6-499">MSBuildExtensionsPath32 設定によって表されるパスからインポートする正しい .targets ファイルを特定する VSToolsPath プロパティが使用されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-499">The VSToolsPath property is used to determine the correct .targets file to import from the path represented by the MSBuildExtensionsPath32 setting.</span></span>

<span data-ttu-id="521b6-500">インポート要素に関連するいくつかの変更もあります。</span><span class="sxs-lookup"><span data-stu-id="521b6-500">There are also some changes related to Import elements.</span></span> <span data-ttu-id="521b6-501">これらの変更は、Visual Studio の両方のバージョン間の互換性をサポートするために必要です。</span><span class="sxs-lookup"><span data-stu-id="521b6-501">These changes are required in order to support compatibility between both versions of Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="521b6-502">プロジェクトは、2 つの異なるコンピューターに Visual Studio 2010 SP1 と Visual Studio 11 Beta の間で共有されている場合と、プロジェクトでは、アプリでローカルのデータベースが含まれている場合\_データ フォルダー、行う必要があります、データベースで使用される SQL Server のバージョンがあることを確認両方のコンピューターにインストールします。</span><span class="sxs-lookup"><span data-stu-id="521b6-502">If a project is being shared between Visual Studio 2010 SP1 and Visual Studio 11 Beta on two different computers, and if the project includes a local database in the App\_Data folder, you must make sure that the version of SQL Server used by the database is installed on both computers.</span></span>

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a><span data-ttu-id="521b6-503">ASP.NET 4.5 web サイト テンプレートの構成の変更</span><span class="sxs-lookup"><span data-stu-id="521b6-503">Configuration Changes in ASP.NET 4.5 Website Templates</span></span>

<span data-ttu-id="521b6-504">既定値に、次の変更を加え*Web.config* Visual Studio 2012 Release Candidate で web サイト テンプレートを使用して作成されたサイトのファイル。</span><span class="sxs-lookup"><span data-stu-id="521b6-504">The following changes have been made to the default *Web.config* file for site that are created using website templates in Visual Studio 2012 Release Candidate:</span></span>

- <span data-ttu-id="521b6-505">`<httpRuntime>`要素、`encoderType`属性が ASP.NET に追加された AntiXSS 型を使用する既定で設定ようになりました。</span><span class="sxs-lookup"><span data-stu-id="521b6-505">In the `<httpRuntime>` element, the `encoderType` attribute is now set by default to use the AntiXSS types that were added to ASP.NET.</span></span> <span data-ttu-id="521b6-506">詳細については、次を参照してください。 [AntiXSS ライブラリ](#_Toc318097382)します。</span><span class="sxs-lookup"><span data-stu-id="521b6-506">For details, see [AntiXSS Library](#_Toc318097382).</span></span>
- <span data-ttu-id="521b6-507">さらに、`<httpRuntime>`要素、`requestValidationMode`属性が「4.5」に設定します。</span><span class="sxs-lookup"><span data-stu-id="521b6-507">Also in the `<httpRuntime>` element, the `requestValidationMode` attribute is set to "4.5".</span></span> <span data-ttu-id="521b6-508">つまり、既定では遅延 (「レイジー」) の検証を使用する要求の検証が構成されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-508">This means that by default, request validation is configured to use deferred ("lazy") validation.</span></span> <span data-ttu-id="521b6-509">詳細については、次を参照してください。 [ASP.NET 要求の検証機能の新しい](#_Toc318097379)します。</span><span class="sxs-lookup"><span data-stu-id="521b6-509">For details, see [New ASP.NET Request Validation Features](#_Toc318097379).</span></span>
- <span data-ttu-id="521b6-510">`<modules>`の要素、`<system.webServer>`セクションが含まれていない、`runAllManagedModulesForAllRequests`属性。</span><span class="sxs-lookup"><span data-stu-id="521b6-510">The `<modules>` element of the `<system.webServer>` section does not contain a `runAllManagedModulesForAllRequests` attribute.</span></span> <span data-ttu-id="521b6-511">(既定値は、false)。つまり版を SP1 に更新されていない IIS 7 を使用している場合は、新しいサイトでのルーティングに関する問題を必要する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="521b6-511">(Its default value is false.) This means that if you are using a version of IIS 7 that has not been updated to SP1, you might have issues with routing in a new site.</span></span> <span data-ttu-id="521b6-512">詳細については、次を参照してください。 [ASP.NET ルーティング IIS 7 でネイティブ サポート](#Native_Support_In_IIS7_For_ASPNET_Routine)します。</span><span class="sxs-lookup"><span data-stu-id="521b6-512">For more information, see [Native Support in IIS 7 for ASP.NET Routing](#Native_Support_In_IIS7_For_ASPNET_Routine).</span></span>

<span data-ttu-id="521b6-513">これらの変更は、既存のアプリケーションには影響しません。</span><span class="sxs-lookup"><span data-stu-id="521b6-513">These changes do not affect existing applications.</span></span> <span data-ttu-id="521b6-514">ただし、既存の web サイトと新しいテンプレートを使用して ASP.NET 4.5 用に作成した新しい web サイトの動作の違いを表すことができます。</span><span class="sxs-lookup"><span data-stu-id="521b6-514">However, they might represent a difference in behavior between existing websites and new websites that you create for ASP.NET 4.5 using the new templates.</span></span>

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a><span data-ttu-id="521b6-515">ASP.NET ルーティング IIS 7 でネイティブ サポート</span><span class="sxs-lookup"><span data-stu-id="521b6-515">Native Support in IIS 7 for ASP.NET Routing</span></span>

<span data-ttu-id="521b6-516">これはなく ASP.NET への変更がそのため、SP1 更新プログラムが適用されていない IIS 7 のバージョンを使用している場合の影響を新しい web サイト プロジェクトのテンプレートの変更です。</span><span class="sxs-lookup"><span data-stu-id="521b6-516">This is not a change to ASP.NET as such, but a change in templates for new website projects that can affect you if you are working a version of IIS 7 that has not had the SP1 update applied.</span></span>

<span data-ttu-id="521b6-517">ASP.NET では、ルーティングをサポートするためにアプリケーションに次の構成設定を追加できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-517">In ASP.NET, you can add the following configuration setting to applications in order to support routing:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

<span data-ttu-id="521b6-518">ときに**runAllManagedModulesForAllRequests**ような URL を真になる`http://mysite/myapp/home`があっても、ASP.NET にはない *.aspx*、 *.mvc*、または上のような拡張機能、URL。</span><span class="sxs-lookup"><span data-stu-id="521b6-518">When **runAllManagedModulesForAllRequests** is true, a URL like `http://mysite/myapp/home` goes to ASP.NET, even though there is no *.aspx*, *.mvc*, or similar extension on the URL.</span></span>

<span data-ttu-id="521b6-519">IIS 7 に対して行われた更新プログラム、 **runAllManagedModulesForAllRequests**不要な設定と、ASP.NET ルーティングをネイティブにサポートしています。</span><span class="sxs-lookup"><span data-stu-id="521b6-519">An update that was made to IIS 7 makes the **runAllManagedModulesForAllRequests** setting unnecessary and supports ASP.NET routing natively.</span></span> <span data-ttu-id="521b6-520">(更新プログラムについては、Microsoft サポート記事を参照してください[更新プログラムを処理する IIS 7.0 または IIS 7.5 のハンドラーは要求 Url を特定できるように、期間で終わっていないことがある](https://support.microsoft.com/kb/980368)。)。</span><span class="sxs-lookup"><span data-stu-id="521b6-520">(For information about the update, see the Microsoft Support article [An update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).)</span></span>

<span data-ttu-id="521b6-521">Web サイトが IIS 7 で実行されると、IIS が更新された場合は設定する必要はありません**runAllManagedModulesForAllRequests**を true にします。</span><span class="sxs-lookup"><span data-stu-id="521b6-521">If your website is running on IIS 7 and if IIS has been updated, you do not need to set **runAllManagedModulesForAllRequests** to true.</span></span> <span data-ttu-id="521b6-522">実際には、true に設定するとは使用しないで、不要な処理オーバーヘッドを要求に追加されるためです。</span><span class="sxs-lookup"><span data-stu-id="521b6-522">In fact, setting it to true is not recommended, because it adds unnecessary processing overhead to request.</span></span> <span data-ttu-id="521b6-523">この設定が true の場合のものも含め、すべての要求 *.htm*、 *.jpg*、他の静的ファイルは、ASP.NET 要求パイプラインを介しても移動します。</span><span class="sxs-lookup"><span data-stu-id="521b6-523">When this setting is true, all requests, including those for *.htm*, *.jpg*, and other static files, also go through the ASP.NET request pipeline.</span></span>

<span data-ttu-id="521b6-524">Visual Studio 2012 RC に用意されているテンプレートを使用して、新しい ASP.NET 4.5 web サイトを作成する場合、web サイトの構成は含まれません、 **runAllManagedModulesForAllRequests**設定します。</span><span class="sxs-lookup"><span data-stu-id="521b6-524">If you create a new ASP.NET 4.5 website using the templates that are provided in Visual Studio 2012 RC, the configuration for the website does not include the **runAllManagedModulesForAllRequests** setting.</span></span> <span data-ttu-id="521b6-525">これは、既定では、設定が false であることを意味します。</span><span class="sxs-lookup"><span data-stu-id="521b6-525">This means that by default the setting is false.</span></span>

<span data-ttu-id="521b6-526">Windows 7 で、SP1 をインストールせず、web サイトを実行し、IIS 7 では、必要な更新プログラムは含まれません。</span><span class="sxs-lookup"><span data-stu-id="521b6-526">If you then run the website on Windows 7 without SP1 installed, IIS 7 will not include the required update.</span></span> <span data-ttu-id="521b6-527">その結果、ルーティングは動作しませんし、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-527">As a consequence, routing will not work and you will see errors.</span></span> <span data-ttu-id="521b6-528">ルーティングが機能しないという問題があるを場合は、次のいずれかを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="521b6-528">If you have a problem where routing does not work, you can do either the following:</span></span>

- <span data-ttu-id="521b6-529">SP1 では、IIS 7 に、更新プログラムを追加するには、Windows 7 を更新します。</span><span class="sxs-lookup"><span data-stu-id="521b6-529">Update Windows 7 to SP1, which will add the update to IIS 7.</span></span>
- <span data-ttu-id="521b6-530">前の表に、Microsoft サポート記事に記載されている更新プログラムをインストールします。</span><span class="sxs-lookup"><span data-stu-id="521b6-530">Install the update that's described in the Microsoft Support article listed previously.</span></span>
- <span data-ttu-id="521b6-531">設定**runAllManagedModulesForAllRequests** web サイトの Web.config ファイルで true に設定します。</span><span class="sxs-lookup"><span data-stu-id="521b6-531">Set **runAllManagedModulesForAllRequests** to true in that website's Web.config file.</span></span> <span data-ttu-id="521b6-532">要求にいくつかのオーバーヘッドのこれが追加されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="521b6-532">Note that this will add some overhead to requests.</span></span>

<a id="_Toc318097397"></a>
### <a name="html-editor"></a><span data-ttu-id="521b6-533">HTML エディター</span><span class="sxs-lookup"><span data-stu-id="521b6-533">HTML Editor</span></span>

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a><span data-ttu-id="521b6-534">スマート タスク</span><span class="sxs-lookup"><span data-stu-id="521b6-534">Smart Tasks</span></span>

<span data-ttu-id="521b6-535">デザイン ビューで、ダイアログ ボックスおよびウィザードに設定するが簡単に、多くの場合のサーバー コントロールの複雑なプロパティに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="521b6-535">In Design view, complex properties of server controls often have associated dialog boxes and wizards to make it easy to set them.</span></span> <span data-ttu-id="521b6-536">特別なダイアログ ボックスを使用して、データ ソースを追加するなど、 *Repeater*制御または列を追加する、 *GridView*コントロール。</span><span class="sxs-lookup"><span data-stu-id="521b6-536">For example, you can use a special dialog box to add a data source to a *Repeater* control or add columns to a *GridView* control.</span></span>

<span data-ttu-id="521b6-537">ただし、この種類の複雑なプロパティの UI のヘルプは、ソース ビューで使用されていませんが。</span><span class="sxs-lookup"><span data-stu-id="521b6-537">However, this type of UI help for complex properties has not been available in Source view.</span></span> <span data-ttu-id="521b6-538">そのため、Visual Studio 11 では、ソース ビューのスマート タスクを紹介します。</span><span class="sxs-lookup"><span data-stu-id="521b6-538">Therefore, Visual Studio 11 introduces Smart Tasks for Source view.</span></span> <span data-ttu-id="521b6-539">スマート タスクは、c# および Visual Basic エディターでよく使用される機能のコンテキスト対応のショートカットです。</span><span class="sxs-lookup"><span data-stu-id="521b6-539">Smart Tasks are context-aware shortcuts for commonly used features in the C# and Visual Basic editors.</span></span>

<span data-ttu-id="521b6-540">ASP.NET Web フォームのコントロールのスマート タスクに表示されるサーバー タグとして小さいグリフ カーソルが要素の内部がときに。</span><span class="sxs-lookup"><span data-stu-id="521b6-540">For ASP.NET Web Forms controls, Smart Tasks appear on server tags as a small glyph when the insertion point is inside the element:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

<span data-ttu-id="521b6-541">スマート タスク グリフをクリックするか、ctrl キーを押しますを展開します。</span><span class="sxs-lookup"><span data-stu-id="521b6-541">The Smart Task expands when you click the glyph or press CTRL+.</span></span> <span data-ttu-id="521b6-542">(ドット)、コード エディターと同じようにします。</span><span class="sxs-lookup"><span data-stu-id="521b6-542">(dot), just as in the code editors.</span></span> <span data-ttu-id="521b6-543">デザイン ビューでのスマート タスクと同様のショートカットが表示されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-543">It then displays shortcuts that are similar to the Smart Tasks in Design view.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

<span data-ttu-id="521b6-544">たとえば、前の図に、スマート タスクは、GridView タスク オプションを示します。</span><span class="sxs-lookup"><span data-stu-id="521b6-544">For example, the Smart Task in the previous illustration shows the GridView Tasks options.</span></span> <span data-ttu-id="521b6-545">列の編集を選択した場合は、次のダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-545">If you choose Edit Columns, the following dialog box is displayed:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

<span data-ttu-id="521b6-546">入力と同じプロパティ ダイアログ ボックスのセットをデザイン ビューで設定できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-546">Filling in the dialog box sets the same properties you can set in Design view.</span></span> <span data-ttu-id="521b6-547">[Ok] をクリックすると、コントロールのマークアップは、新しい設定で更新されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-547">When you click OK, the markup for the control is updated with the new settings:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a><span data-ttu-id="521b6-548">待機する ARIA のサポート</span><span class="sxs-lookup"><span data-stu-id="521b6-548">WAI-ARIA support</span></span>

<span data-ttu-id="521b6-549">アクセス可能な web サイトを作成するには、ますます重要が高まっています。</span><span class="sxs-lookup"><span data-stu-id="521b6-549">Writing accessible websites is becoming increasingly important.</span></span> <span data-ttu-id="521b6-550">[WAI ARIA のユーザー補助の標準](http://www.w3.org/WAI/intro/aria)開発者がアクセスできる web サイトを作成する方法を定義します。</span><span class="sxs-lookup"><span data-stu-id="521b6-550">The [WAI-ARIA accessibility standard](http://www.w3.org/WAI/intro/aria) defines how developers should write accessible websites.</span></span> <span data-ttu-id="521b6-551">この標準が Visual Studio で完全にサポートされています。</span><span class="sxs-lookup"><span data-stu-id="521b6-551">This standard is now fully supported in Visual Studio.</span></span>

<span data-ttu-id="521b6-552">たとえば、*ロール*属性が intellisense の全機能を持つようになりました。</span><span class="sxs-lookup"><span data-stu-id="521b6-552">For example, the *role* attribute now has full IntelliSense:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

<span data-ttu-id="521b6-553">待機する ARIA 標準は、属性が付いているも導入されています。 *aria-* セマンティクスを HTML5 のドキュメントに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="521b6-553">The WAI-ARIA standard also introduces attributes that are prefixed with *aria-* that let you add semantics to an HTML5 document.</span></span> <span data-ttu-id="521b6-554">これらは、visual Studio が完全にサポート*aria-* 属性。</span><span class="sxs-lookup"><span data-stu-id="521b6-554">Visual Studio also fully supports these *aria-* attributes:</span></span>

<span data-ttu-id="521b6-555">![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="521b6-555">![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)</span></span>

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a><span data-ttu-id="521b6-556">新しい HTML5 スニペット</span><span class="sxs-lookup"><span data-stu-id="521b6-556">New HTML5 snippets</span></span>

<span data-ttu-id="521b6-557">高速かつ一般的に使用される HTML5 マークアップを記述しやすくするには、Visual Studio には、さまざまなスニペットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="521b6-557">To make it faster and easier to write commonly used HTML5 markup, Visual Studio includes a number of snippets.</span></span> <span data-ttu-id="521b6-558">ビデオのスニペットを例に示します。</span><span class="sxs-lookup"><span data-stu-id="521b6-558">An example is the video snippet:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

<span data-ttu-id="521b6-559">IntelliSense で、要素が選択されたときに 2 回は、スニペットを呼び出すには、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="521b6-559">To invoke the snippet, press Tab twice when the element is selected in IntelliSense:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

<span data-ttu-id="521b6-560">これには、カスタマイズ可能なスニペットが生成されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-560">This produces a snippet that you can customize.</span></span>

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a><span data-ttu-id="521b6-561">ユーザー コントロールに展開します。</span><span class="sxs-lookup"><span data-stu-id="521b6-561">Extract to user control</span></span>

<span data-ttu-id="521b6-562">大規模な web ページでユーザー コントロールに個々 の項目を移動することをお勧めできます。</span><span class="sxs-lookup"><span data-stu-id="521b6-562">In large web pages, it can be a good idea to move individual pieces into user controls.</span></span> <span data-ttu-id="521b6-563">この形式リファクタリングのでは、ページの読みやすさを高めることができ、ページの構造を簡略化できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-563">This form of refactoring can help increase the readability of the page and can simplify the page structure.</span></span>

<span data-ttu-id="521b6-564">、ソース ビュー内の Web フォーム ページを編集するときに、簡単にすることができます、ページにテキストを選択するようになりました、右クリックし、抽出をユーザー コントロールを選択します。</span><span class="sxs-lookup"><span data-stu-id="521b6-564">To make this easier, when you edit Web Forms pages in Source view, you can now select text in a page, right-click it, and then choose Extract to User Control:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a><span data-ttu-id="521b6-565">属性でコード ナゲット用の IntelliSense</span><span class="sxs-lookup"><span data-stu-id="521b6-565">IntelliSense for code nuggets in attributes</span></span>

<span data-ttu-id="521b6-566">Visual Studio は、任意のページまたはコントロール内のサーバー側コード ナゲットの IntelliSense を常に提供しました。</span><span class="sxs-lookup"><span data-stu-id="521b6-566">Visual Studio has always provided IntelliSense for server-side code nuggets in any page or control.</span></span> <span data-ttu-id="521b6-567">今すぐ Visual Studio には、HTML の属性もにコード ナゲットの IntelliSense が含まれます。</span><span class="sxs-lookup"><span data-stu-id="521b6-567">Now Visual Studio includes IntelliSense for code nuggets in HTML attributes as well.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

<span data-ttu-id="521b6-568">これにより、簡単にデータ バインディング式を作成します。</span><span class="sxs-lookup"><span data-stu-id="521b6-568">This makes it easier to create data-binding expressions:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a><span data-ttu-id="521b6-569">自動開始タグまたは終了タグの名前を変更するときの対応タグの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="521b6-569">Automatic renaming of matching tag when you rename an opening or closing tag</span></span>

<span data-ttu-id="521b6-570">HTML 要素の名前を変更する場合 (などを変更する、 *div*するタグを*ヘッダー*タグ)、対応する、開くまたは終了タグがリアルタイムでも変更します。</span><span class="sxs-lookup"><span data-stu-id="521b6-570">If you rename an HTML element (for example, you change a *div* tag to be a *header* tag), the corresponding opening or closing tag also changes in real time.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

<span data-ttu-id="521b6-571">これは、終了タグを変更するか、誤ったマイクを変更するには、忘れたエラーを回避できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-571">This helps avoid the error where you forget to change a closing tag or change the wrong one.</span></span>

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a><span data-ttu-id="521b6-572">イベント ハンドラーの生成</span><span class="sxs-lookup"><span data-stu-id="521b6-572">Event handler generation</span></span>

<span data-ttu-id="521b6-573">Visual Studio で、イベント ハンドラーを記述し、それらを手動でバインドするためのソース ビューで機能できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="521b6-573">Visual Studio now includes features in Source view to help you write event handlers and bind them manually.</span></span> <span data-ttu-id="521b6-574">ソース ビューで、イベントの名前を編集する場合は、IntelliSense が表示される&lt;新しいイベントの作成&gt;、正しいシグネチャを持つはイベント ハンドラーを作成、ページのコードにします。</span><span class="sxs-lookup"><span data-stu-id="521b6-574">If you are editing an event name in Source view, IntelliSense displays &lt;Create New Event&gt;, which will create an event handler in the page's code that has the right signature:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

<span data-ttu-id="521b6-575">既定では、イベント ハンドラーはイベント処理メソッドの名前のコントロールの ID を使用します。</span><span class="sxs-lookup"><span data-stu-id="521b6-575">By default, the event handler will use the control's ID for the name of the event-handling method:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

<span data-ttu-id="521b6-576">結果として得られるイベント ハンドラーを (この場合は、c# で) 次のようになります。</span><span class="sxs-lookup"><span data-stu-id="521b6-576">The resulting event handler will look like this (in this case, in C#):</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a><span data-ttu-id="521b6-577">スマート インデント</span><span class="sxs-lookup"><span data-stu-id="521b6-577">Smart indent</span></span>

<span data-ttu-id="521b6-578">空の HTML 要素内で Enter を押すと、エディターは、適切な場所にカーソルを配置します。</span><span class="sxs-lookup"><span data-stu-id="521b6-578">When you press Enter while inside an empty HTML element, the editor will put the insertion point in the right place:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

<span data-ttu-id="521b6-579">この場所で Enter を押すと、終了タグは下に移動し、開始タグと対応するためにインデントします。</span><span class="sxs-lookup"><span data-stu-id="521b6-579">If you press Enter in this location, the closing tag is moved down and indented to match the opening tag.</span></span> <span data-ttu-id="521b6-580">カーソルがインデントされます。</span><span class="sxs-lookup"><span data-stu-id="521b6-580">The insertion point is also indented:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a><span data-ttu-id="521b6-581">ステートメント入力候補の自動減少します。</span><span class="sxs-lookup"><span data-stu-id="521b6-581">Auto-reduce statement completion</span></span>

<span data-ttu-id="521b6-582">IntelliSense の一覧のみに関連するオプションを表示するように入力した内容に基づいて Visual Studio 今すぐフィルター:</span><span class="sxs-lookup"><span data-stu-id="521b6-582">The IntelliSense list in Visual Studio now filters based on what you type so that it displays only relevant options:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

<span data-ttu-id="521b6-583">IntelliSense リスト内の個々 の単語のタイトル ケースに基づくフィルターも IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="521b6-583">IntelliSense also filters based on the title case of the individual words in the IntelliSense list.</span></span> <span data-ttu-id="521b6-584">たとえば、"dl"を入力する場合は、dl と asp: データの両方が表示されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-584">For example, if you type "dl", both dl and asp:DataList are displayed:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

<span data-ttu-id="521b6-585">この機能によって、高速化を既知の要素の入力候補を取得します。</span><span class="sxs-lookup"><span data-stu-id="521b6-585">This feature makes it faster to get statement completion for known elements.</span></span>

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a><span data-ttu-id="521b6-586">JavaScript エディター</span><span class="sxs-lookup"><span data-stu-id="521b6-586">JavaScript Editor</span></span>

<span data-ttu-id="521b6-587">Visual Studio 2012 Release Candidate の JavaScript エディターがまったく新しくなり、Visual Studio の JavaScript の操作のエクスペリエンスが大幅に向上します。</span><span class="sxs-lookup"><span data-stu-id="521b6-587">The JavaScript editor in Visual Studio 2012 Release Candidate is completely new and it greatly improves the experience of working with JavaScript in Visual Studio.</span></span>

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a><span data-ttu-id="521b6-588">コードのアウトライン表示</span><span class="sxs-lookup"><span data-stu-id="521b6-588">Code outlining</span></span>

<span data-ttu-id="521b6-589">アウトライン領域はすると、現在、フォーカスに関連するではないファイルの部分を折りたたむことができます、すべての関数は自動的に作成されました。</span><span class="sxs-lookup"><span data-stu-id="521b6-589">Outlining regions are now automatically created for all functions, allowing you to collapse parts of the file that aren't pertinent to your current focus.</span></span>

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a><span data-ttu-id="521b6-590">かっこの一致</span><span class="sxs-lookup"><span data-stu-id="521b6-590">Brace matching</span></span>

<span data-ttu-id="521b6-591">開始タグまたは右中かっこにカーソルを配置するときに、エディターには、一致するものが強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-591">When you put the insertion point on an opening or closing brace, the editor highlights the matching one.</span></span>

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a><span data-ttu-id="521b6-592">定義へ移動</span><span class="sxs-lookup"><span data-stu-id="521b6-592">Go to Definition</span></span>

<span data-ttu-id="521b6-593">定義コマンドへの移動では、関数または変数のソースにジャンプできます。</span><span class="sxs-lookup"><span data-stu-id="521b6-593">The Go to Definition command lets you jump to the source for a function or variable.</span></span>

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a><span data-ttu-id="521b6-594">ECMAScript5 のサポート</span><span class="sxs-lookup"><span data-stu-id="521b6-594">ECMAScript5 support</span></span>

<span data-ttu-id="521b6-595">エディターでは、ECMAScript5、JavaScript 言語を記述する標準の最新バージョンで、新しい構文および Api をサポートします。</span><span class="sxs-lookup"><span data-stu-id="521b6-595">The editor supports the new syntax and APIs in ECMAScript5, the latest version of the standard that describes the JavaScript language.</span></span>

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a><span data-ttu-id="521b6-596">DOM の IntelliSense</span><span class="sxs-lookup"><span data-stu-id="521b6-596">DOM IntelliSense</span></span>

<span data-ttu-id="521b6-597">多くの新しい HTML5 Api などのサポートが、DOM Api 用の IntelliSense を強化されています*querySelector*、DOM ストレージ、クロス ドキュメント メッセージング、および*キャンバス*します。</span><span class="sxs-lookup"><span data-stu-id="521b6-597">IntelliSense for DOM APIs has been improved, with support for many new HTML5 APIs including *querySelector*, DOM Storage, cross-document messaging, and *canvas*.</span></span> <span data-ttu-id="521b6-598">ネイティブ型ライブラリの定義ではなく 1 つの単純な JavaScript ファイルで DOM の IntelliSense はこれで駆動されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-598">DOM IntelliSense is now driven by a single simple JavaScript file, rather than by a native type library definition.</span></span> <span data-ttu-id="521b6-599">これにより、簡単に拡張または置換できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-599">This makes it easy to extend or replace.</span></span>

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a><span data-ttu-id="521b6-600">VSDOC の署名のオーバー ロード</span><span class="sxs-lookup"><span data-stu-id="521b6-600">VSDOC signature overloads</span></span>

<span data-ttu-id="521b6-601">IntelliSense の詳細なコメントは今すぐ新しい JavaScript 関数のオーバー ロードを別々 の宣言された*&lt;署名&gt;* 要素は、この例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="521b6-601">Detailed IntelliSense comments can now be declared for separate overloads of JavaScript functions by using the new *&lt;signature&gt;* element, as shown in this example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a><span data-ttu-id="521b6-602">暗黙的な参照</span><span class="sxs-lookup"><span data-stu-id="521b6-602">Implicit references</span></span>

<span data-ttu-id="521b6-603">JavaScript ファイルを暗黙的に含まれるファイルの一覧で指定された JavaScript ファイルまたはブロックの参照、つまりがその内容に IntelliSense を取得しますサーバーの全体の一覧に追加できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-603">You can now add JavaScript files to a central list that will be implicitly included in the list of files that any given JavaScript file or block references, meaning you'll get IntelliSense for its contents.</span></span> <span data-ttu-id="521b6-604">たとえば、jQuery ファイルをファイルのサーバーの全体の一覧を追加することができ、受け取ります IntelliSense jQuery 関数のファイルの任意の JavaScript ブロックに明示的に参照しているかどうか (を使用して///&lt;参照/&gt;) かどうか。</span><span class="sxs-lookup"><span data-stu-id="521b6-604">For example, you can add jQuery files to the central list of files, and you'll get IntelliSense for jQuery functions in any JavaScript block of file, whether you've referenced it explicitly (using /// &lt;reference /&gt;) or not.</span></span>

<a id="_Toc318097415"></a>
### <a name="css-editor"></a><span data-ttu-id="521b6-605">CSS エディター</span><span class="sxs-lookup"><span data-stu-id="521b6-605">CSS Editor</span></span>

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a><span data-ttu-id="521b6-606">ステートメント入力候補の自動減少します。</span><span class="sxs-lookup"><span data-stu-id="521b6-606">Auto-reduce statement completion</span></span>

<span data-ttu-id="521b6-607">CSS プロパティに基づく CSS 今すぐフィルターと、選択したスキーマによってサポートされる値の IntelliSense の一覧です。</span><span class="sxs-lookup"><span data-stu-id="521b6-607">The IntelliSense list for CSS now filters based on the CSS properties and values supported by the selected schema.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

<span data-ttu-id="521b6-608">IntelliSense には、タイトルの大文字と小文字の検索もサポートしています。</span><span class="sxs-lookup"><span data-stu-id="521b6-608">IntelliSense also supports title case searches:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a><span data-ttu-id="521b6-609">階層インデント</span><span class="sxs-lookup"><span data-stu-id="521b6-609">Hierarchical indentation</span></span>

<span data-ttu-id="521b6-610">CSS エディターは、提供する、カスケードする規則が論理的に編成する方法の概要に階層のルールを表示するのに、インデントに使用します。</span><span class="sxs-lookup"><span data-stu-id="521b6-610">The CSS editor uses indentation to display hierarchical rules, which gives you an overview of how the cascading rules are logically organized.</span></span> <span data-ttu-id="521b6-611">次の例では、#list セレクターがリストのカスケード型の子とそのためインデントされます。</span><span class="sxs-lookup"><span data-stu-id="521b6-611">In the following example, the #list a selector is a cascading child of list and is therefore indented.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

<span data-ttu-id="521b6-612">次の例より複雑な継承を示します。</span><span class="sxs-lookup"><span data-stu-id="521b6-612">The following example shows more complex inheritance:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

<span data-ttu-id="521b6-613">ルールのインデントは、その親規則によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-613">The indentation of a rule is determined by its parent rules.</span></span> <span data-ttu-id="521b6-614">階層インデントが既定で有効になっているが、[オプション] ダイアログ ボックス (ツール、メニュー バーのオプション) は無効にできます。</span><span class="sxs-lookup"><span data-stu-id="521b6-614">Hierarchical indentation is enabled by default, but you can disable it the Options dialog box (Tools, Options from the menu bar):</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a><span data-ttu-id="521b6-615">CSS ハックのサポート</span><span class="sxs-lookup"><span data-stu-id="521b6-615">CSS hacks support</span></span>

<span data-ttu-id="521b6-616">現実世界の CSS ファイルの数百の分析には、CSS ハックは非常に一般的なと、Visual Studio で最も広く使用されているものをサポートするようになりましたことが表示されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-616">Analysis of hundreds of real-world CSS files shows that CSS hacks are very common, and now Visual Studio supports the most widely used ones.</span></span> <span data-ttu-id="521b6-617">このサポートには、IntelliSense と、星の検証が含まれています (\*)、およびアンダー スコア (\_) プロパティ ハック。</span><span class="sxs-lookup"><span data-stu-id="521b6-617">This support includes IntelliSense and validation of the star (\*) and underscore (\_) property hacks:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

<span data-ttu-id="521b6-618">階層インデントを適用する場合にも維持するためにも、一般的なセレクターのハッキングがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="521b6-618">Typical selector hacks are also supported so that hierarchical indentation is maintained even when they are applied.</span></span> <span data-ttu-id="521b6-619">Internet Explorer 7 をターゲットに使用される一般的なセレクターの裏技は、前に、セレクターを付加する *\*::first-child + html*します。</span><span class="sxs-lookup"><span data-stu-id="521b6-619">A typical selector hack used to target Internet Explorer 7 is to prepend a selector with *\*:first-child + html*.</span></span> <span data-ttu-id="521b6-620">その規則を使用して、階層のインデントを保持します。</span><span class="sxs-lookup"><span data-stu-id="521b6-620">Using that rule will maintain the hierarchical indentation:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a><span data-ttu-id="521b6-621">ベンダー固有のスキーマ (-moz、webkit)</span><span class="sxs-lookup"><span data-stu-id="521b6-621">Vendor specific schemas (-moz-, -webkit)</span></span>

<span data-ttu-id="521b6-622">CSS3 では、異なる時刻でさまざまなブラウザーで実装されている多くのプロパティについて説明します。</span><span class="sxs-lookup"><span data-stu-id="521b6-622">CSS3 introduces many properties that have been implemented by different browsers at different times.</span></span> <span data-ttu-id="521b6-623">これ以前ベンダー固有の構文を使用して、開発者は特定のブラウザーのコードを強制します。</span><span class="sxs-lookup"><span data-stu-id="521b6-623">This previously forced developers to code for specific browsers by using vendor-specific syntax.</span></span> <span data-ttu-id="521b6-624">これらのブラウザー固有プロパティは、IntelliSense には含まれるようになりました。</span><span class="sxs-lookup"><span data-stu-id="521b6-624">These browser-specific properties are now included in IntelliSense.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a><span data-ttu-id="521b6-625">コメント化とコメント解除のサポート</span><span class="sxs-lookup"><span data-stu-id="521b6-625">Commenting and uncommenting support</span></span>

<span data-ttu-id="521b6-626">コメントをして、コード エディター (Ctrl + K、コメントに C と Ctrl + K、コメントを解除する) を使用する同じショートカット キーを使用して CSS ルールのコメントを解除できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-626">You can now comment and uncomment CSS rules using the same shortcuts that you use in the code editor (Ctrl+K,C to comment and Ctrl+K,U to uncomment).</span></span>

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a><span data-ttu-id="521b6-627">カラー ピッカー</span><span class="sxs-lookup"><span data-stu-id="521b6-627">Color picker</span></span>

<span data-ttu-id="521b6-628">Visual Studio の以前のバージョンでは、色関連の属性用の IntelliSense は、名前付きの色の値のドロップダウン リストを行いました。</span><span class="sxs-lookup"><span data-stu-id="521b6-628">In previous versions of Visual Studio, IntelliSense for color-related attributes consisted of a drop-down list of named color values.</span></span> <span data-ttu-id="521b6-629">そのリストは、全機能装備のカラー ピッカーによって置き換えられました。</span><span class="sxs-lookup"><span data-stu-id="521b6-629">That list has been replaced by a full-featured color picker.</span></span>

<span data-ttu-id="521b6-630">色の値を入力すると、カラー ピッカーは自動的が表示され、既定の色パレットを続けて使用されていた色の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-630">When you enter a color value, the color picker is displayed automatically and presents a list of previously used colors followed by a default color palette.</span></span> <span data-ttu-id="521b6-631">マウスまたはキーボードを使用してカラーを選択することができます。</span><span class="sxs-lookup"><span data-stu-id="521b6-631">You can select a color using the mouse or the keyboard.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

<span data-ttu-id="521b6-632">一覧は、完全なカラー ピッカーに展開できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-632">The list can be expanded into a complete color picker.</span></span> <span data-ttu-id="521b6-633">ピッカーを使用して、RGBA に不透明度のスライダーを移動すると、任意の色を自動的に変換することでアルファ チャネルを制御できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-633">The picker lets you control the alpha channel by automatically converting any color into RGBA when you move the opacity slider:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a><span data-ttu-id="521b6-634">スニペット</span><span class="sxs-lookup"><span data-stu-id="521b6-634">Snippets</span></span>

<span data-ttu-id="521b6-635">CSS エディターのスニペットやすく簡単かつ迅速にクロス ブラウザーのスタイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="521b6-635">Snippets in the CSS editor make it easier and faster to create cross-browser styles.</span></span> <span data-ttu-id="521b6-636">ブラウザーに固有の設定を必要とする多くの CSS3 プロパティは、スニペットにロールされているようになりました。</span><span class="sxs-lookup"><span data-stu-id="521b6-636">Many CSS3 properties that require browser-specific settings have now been rolled into snippets.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

<span data-ttu-id="521b6-637">CSS スニペットでシンボルを入力して (CSS3 メディア クエリ) のような高度なシナリオをサポートする (@)、IntelliSense の一覧を示しています。</span><span class="sxs-lookup"><span data-stu-id="521b6-637">CSS snippets support advanced scenarios (like CSS3 media queries) by typing the at-symbol (@), which shows the IntelliSense list.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

<span data-ttu-id="521b6-638">選択すると@media値、Tab キーを押すと、CSS エディターは、次のスニペットを挿入します。</span><span class="sxs-lookup"><span data-stu-id="521b6-638">When you select @media value and press Tab, the CSS editor inserts the following snippet:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

<span data-ttu-id="521b6-639">コード スニペットと同様、独自の CSS スニペットを作成できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-639">As with snippets for code, you can create your own CSS snippets.</span></span>

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a><span data-ttu-id="521b6-640">カスタムのリージョン</span><span class="sxs-lookup"><span data-stu-id="521b6-640">Custom regions</span></span>

<span data-ttu-id="521b6-641">コード エディターで使用されている、コード領域をという名前は、CSS の編集に利用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="521b6-641">Named code regions, which are already available in the code editor, are now available for CSS editing.</span></span> <span data-ttu-id="521b6-642">これにより、関連するスタイル要素を簡単にグループ化することができます。</span><span class="sxs-lookup"><span data-stu-id="521b6-642">This lets you easily group related style blocks.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

<span data-ttu-id="521b6-643">領域が折りたたまれている領域の名前が表示されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-643">When a region is collapsed it displays the name of the region:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a><span data-ttu-id="521b6-644">Page Inspector</span><span class="sxs-lookup"><span data-stu-id="521b6-644">Page Inspector</span></span>

<span data-ttu-id="521b6-645">Page Inspector は、ツール、Visual Studio IDE での web ページ (HTML、Web フォーム、ASP.NET MVC、または Web ページ) を表示し、ソース コードと結果の出力の両方を調べることができます。</span><span class="sxs-lookup"><span data-stu-id="521b6-645">Page Inspector is a tool that renders a web page (HTML, Web Forms, ASP.NET MVC, or Web Pages) in the Visual Studio IDE and lets you examine both the source code and the resulting output.</span></span> <span data-ttu-id="521b6-646">ASP.NET ページ、Page Inspector ではサーバー側コードがブラウザーにレンダリングされる HTML マークアップを生成して判断できます。</span><span class="sxs-lookup"><span data-stu-id="521b6-646">For ASP.NET pages, Page Inspector lets you determine which server-side code has produced the HTML markup that is rendered to the browser.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

<span data-ttu-id="521b6-647">Page Inspector の詳細については、次のチュートリアルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="521b6-647">For more information about Page Inspector, please see the following tutorials:</span></span>

- <span data-ttu-id="521b6-648">Page Inspector を使用して[ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)</span><span class="sxs-lookup"><span data-stu-id="521b6-648">Using Page Inspector in [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)</span></span>
- <span data-ttu-id="521b6-649">Page Inspector を使用して[ASP.NET Web フォーム](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)</span><span class="sxs-lookup"><span data-stu-id="521b6-649">Using Page Inspector in [ASP.NET Web Forms](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)</span></span>

<a id="_Toc318097425"></a>
### <a name="publishing"></a><span data-ttu-id="521b6-650">置換</span><span class="sxs-lookup"><span data-stu-id="521b6-650">Publishing</span></span>

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a><span data-ttu-id="521b6-651">プロファイルを発行する</span><span class="sxs-lookup"><span data-stu-id="521b6-651">Publish profiles</span></span>

<span data-ttu-id="521b6-652">Visual Studio 2010 では、Web アプリケーション プロジェクトの発行の情報は、バージョン コントロールに格納されていないと、他のユーザーと共有するためのものではありません。</span><span class="sxs-lookup"><span data-stu-id="521b6-652">In Visual Studio 2010, publishing information for Web application projects is not stored in version control and is not designed for sharing with others.</span></span> <span data-ttu-id="521b6-653">Visual Studio 2012 Release Candidate、発行プロファイルの形式が変更されました。</span><span class="sxs-lookup"><span data-stu-id="521b6-653">In Visual Studio 2012 Release Candidate, the format of the publish profile has been changed.</span></span> <span data-ttu-id="521b6-654">チームの成果物、公開されているし、MSBuild に基づくビルドから利用することができます。</span><span class="sxs-lookup"><span data-stu-id="521b6-654">It has been made a team artifact, and it is now easy to leverage from builds based on MSBuild.</span></span> <span data-ttu-id="521b6-655">ビルドの構成情報が設定されて、[発行] ダイアログ ボックスでパブリッシュする前に、ビルド構成を簡単に切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="521b6-655">Build configuration information is in the Publish dialog box so that you can easily switch build configurations before publishing.</span></span>

<span data-ttu-id="521b6-656">発行プロファイル PublishProfiles フォルダーに格納されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-656">Publish profiles are stored in the PublishProfiles folder.</span></span> <span data-ttu-id="521b6-657">フォルダーの場所はどのようなプログラミング言語を使用しています。</span><span class="sxs-lookup"><span data-stu-id="521b6-657">The location of the folder depends on what programming language you are using:</span></span>

- <span data-ttu-id="521b6-658">C#: Properties\PublishProfiles</span><span class="sxs-lookup"><span data-stu-id="521b6-658">C#: Properties\PublishProfiles</span></span>
- <span data-ttu-id="521b6-659">Visual Basic: My Project\PublishProfiles</span><span class="sxs-lookup"><span data-stu-id="521b6-659">Visual Basic: My Project\PublishProfiles</span></span>

<span data-ttu-id="521b6-660">各プロファイルは、MSBuild ファイルです。</span><span class="sxs-lookup"><span data-stu-id="521b6-660">Each profile is an MSBuild file.</span></span> <span data-ttu-id="521b6-661">発行中には、このファイルは、プロジェクトの MSBuild ファイルにインポートされます。</span><span class="sxs-lookup"><span data-stu-id="521b6-661">During publishing, this file is imported into the project's MSBuild file.</span></span> <span data-ttu-id="521b6-662">Visual Studio 2010 で発行またはパッケージのプロセスを変更する場合があるという名前のファイルに、カスタマイズを配置する**ProjectName**。 wpp.targets します。</span><span class="sxs-lookup"><span data-stu-id="521b6-662">In Visual Studio 2010, if you want to make changes to the publish or package process, you have to put your customizations in a file named **ProjectName**.wpp.targets.</span></span> <span data-ttu-id="521b6-663">これはまだサポートされてが、発行プロファイル自体で、カスタマイズを配置できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="521b6-663">This is still supported, but you can now put your customizations in the publish profile itself.</span></span> <span data-ttu-id="521b6-664">これにより、そのプロファイルに対してのみ、カスタマイズが使用されます。</span><span class="sxs-lookup"><span data-stu-id="521b6-664">That way, the customizations will be used only for that profile.</span></span>

<span data-ttu-id="521b6-665">活用が MSBuild を使用してプロファイルを発行することもできます。</span><span class="sxs-lookup"><span data-stu-id="521b6-665">You can now also leverage publish profiles from MSBuild.</span></span> <span data-ttu-id="521b6-666">これを行うには、プロジェクトをビルドするときに、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="521b6-666">To do so, use the following command when you build the project:</span></span>

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

<span data-ttu-id="521b6-667">Project.csproj 値は、プロジェクトのパスと ProfileName は発行プロファイルの名前です。</span><span class="sxs-lookup"><span data-stu-id="521b6-667">The project.csproj value is the path of the project, and ProfileName is the name of the profile to publish.</span></span> <span data-ttu-id="521b6-668">用のプロファイル名を渡す代わりに、または、 *PublishProfile*プロパティを渡すことができます、完全なパスで発行プロファイルにします。</span><span class="sxs-lookup"><span data-stu-id="521b6-668">Alternatively, instead of passing the profile name for the *PublishProfile* property, you can pass in the full path to the publish profile.</span></span>

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a><span data-ttu-id="521b6-669">ASP.NET プリコンパイルとマージ</span><span class="sxs-lookup"><span data-stu-id="521b6-669">ASP.NET precompilation and merge</span></span>

<span data-ttu-id="521b6-670">Web アプリケーション プロジェクトでは、Visual Studio 2012 Release Candidate をプリコンパイルし、サイトのコンテンツを発行するときに、またはプロジェクトのパッケージをマージすることができます、パッケージ化/発行の Web プロパティ ページのオプションを追加します。</span><span class="sxs-lookup"><span data-stu-id="521b6-670">For Web application projects, Visual Studio 2012 Release Candidate adds an option on the Package/Publish Web properties page that lets you precompile and merge your site's content when you publish or package the project.</span></span> <span data-ttu-id="521b6-671">これらのオプションを参照してください。 し、ソリューション エクスプ ローラーでプロジェクトを右クリックし、プロパティを選択、パッケージ化/発行の Web プロパティ ページを選択します。</span><span class="sxs-lookup"><span data-stu-id="521b6-671">To see these options, right-click the project in Solution Explorer, choose Properties, and then choose the Package/Publish Web property page.</span></span> <span data-ttu-id="521b6-672">次の図は、プリコンパイル オプションを公開する前にこのアプリケーションを示します。</span><span class="sxs-lookup"><span data-stu-id="521b6-672">The following illustration shows the Precompile this application before publishing option.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

<span data-ttu-id="521b6-673">このオプションを選択すると、Visual Studio は、発行するたびに、アプリケーションまたはパッケージ、web アプリケーションをプリコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="521b6-673">When this option is selected, Visual Studio precompiles the application whenever you publish or package the web application.</span></span> <span data-ttu-id="521b6-674">サイトのプリコンパイル方法またはアセンブリをマージする方法を制御する場合は、これらのオプションを構成する [詳細設定] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="521b6-674">If you want to control how the site is precompiled or how assemblies are merged, click the Advanced button to configure those options.</span></span>

<a id="_Toc318097428"></a>
### <a name="iis-express"></a><span data-ttu-id="521b6-675">IIS Express</span><span class="sxs-lookup"><span data-stu-id="521b6-675">IIS Express</span></span>

<span data-ttu-id="521b6-676">Visual Studio でのテストの web プロジェクトの既定の web サーバーは、IIS Express ようになりました。</span><span class="sxs-lookup"><span data-stu-id="521b6-676">The default web server for testing web projects in Visual Studio is now IIS Express.</span></span> <span data-ttu-id="521b6-677">Visual Studio 開発サーバーは開発中は、ローカル web サーバーのオプションですが IIS Express は、推奨されるサーバーではようになりました。</span><span class="sxs-lookup"><span data-stu-id="521b6-677">The Visual Studio Development Server is still an option for local web server during development, but IIS Express is now the recommended server.</span></span> <span data-ttu-id="521b6-678">IIS Express を使用して、Visual Studio 11 Beta でのエクスペリエンスは、Visual Studio 2010 SP1 で使用するとよく似ています。</span><span class="sxs-lookup"><span data-stu-id="521b6-678">The experience of using IIS Express in Visual Studio 11 Beta is very similar to using it in Visual Studio 2010 SP1.</span></span>

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a><span data-ttu-id="521b6-679">免責情報</span><span class="sxs-lookup"><span data-stu-id="521b6-679">Disclaimer</span></span>

<span data-ttu-id="521b6-680">このドキュメントは暫定版であり、ここで説明するソフトウェアの最終的な商業リリースより前に大幅に変更される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="521b6-680">This is a preliminary document and may be changed substantially prior to final commercial release of the software described herein.</span></span>

<span data-ttu-id="521b6-681">このドキュメントに記載されている情報は、説明されている問題に関する Microsoft Corporation の発行日時点における最新の見解を表します。</span><span class="sxs-lookup"><span data-stu-id="521b6-681">The information contained in this document represents the current view of Microsoft Corporation on the issues discussed as of the date of publication.</span></span> <span data-ttu-id="521b6-682">マイクロソフトは変化する市場の状況に対応する必要があるため、本ドキュメントをマイクロソフトの確約と解釈することはできず、発行日以降は、提示されている情報の正確性を保証できません。</span><span class="sxs-lookup"><span data-stu-id="521b6-682">Because Microsoft must respond to changing market conditions, it should not be interpreted to be a commitment on the part of Microsoft, and Microsoft cannot guarantee the accuracy of any information presented after the date of publication.</span></span>

<span data-ttu-id="521b6-683">このホワイト ペーパーは、情報提供のみを目的としています。</span><span class="sxs-lookup"><span data-stu-id="521b6-683">This White Paper is for informational purposes only.</span></span> <span data-ttu-id="521b6-684">マイクロソフトは、このドキュメント内の情報に関して、明示的、暗黙的、または法的ないかなる保証も行いません。</span><span class="sxs-lookup"><span data-stu-id="521b6-684">MICROSOFT MAKES NO WARRANTIES, EXPRESS, IMPLIED OR STATUTORY, AS TO THE INFORMATION IN THIS DOCUMENT.</span></span>

<span data-ttu-id="521b6-685">お客様ご自身の責任において、適用されるすべての著作権関連法規に従ったご使用を願います。</span><span class="sxs-lookup"><span data-stu-id="521b6-685">Complying with all applicable copyright laws is the responsibility of the user.</span></span> <span data-ttu-id="521b6-686">このドキュメントのいかなる部分も、米国 Microsoft Corporation の書面による許諾を受けることなく、その目的を問わず、どのような形態であっても、複製または譲渡することは禁じられています。ここでいう形態とは、複写や記録など、電子的な、または物理的なすべての手段を含みます。ただしこれは、著作権法上のお客様の権利を制限するものではありません。</span><span class="sxs-lookup"><span data-stu-id="521b6-686">Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.</span></span>

<span data-ttu-id="521b6-687">マイクロソフトは、このドキュメントに記載されている内容に関し、特許、特許申請、商標、著作権、またはその他の無体財産権を有する場合があります。</span><span class="sxs-lookup"><span data-stu-id="521b6-687">Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document.</span></span> <span data-ttu-id="521b6-688">別途マイクロソフトのライセンス契約上に明示の規定のない限り、このドキュメントはこれらの特許、商標、著作権、またはその他の無体財産権に関する権利をお客様に許諾するものではありません。</span><span class="sxs-lookup"><span data-stu-id="521b6-688">Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.</span></span>

<span data-ttu-id="521b6-689">明記されない限り、会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、イベント実在架空のものと関連会社、組織、製品、ドメイン名、電子メールをアドレス、ロゴ、人物、場所、またはイベントは、または推論する必要があります。</span><span class="sxs-lookup"><span data-stu-id="521b6-689">Unless otherwise noted, the example companies, organizations, products, domain names, email addresses, logos, people, places and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, email address, logo, person, place or event is intended or should be inferred.</span></span>

<span data-ttu-id="521b6-690">(C) 2012 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="521b6-690">© 2012 Microsoft Corporation.</span></span> <span data-ttu-id="521b6-691">All rights reserved.</span><span class="sxs-lookup"><span data-stu-id="521b6-691">All rights reserved.</span></span>

<span data-ttu-id="521b6-692">Microsoft および Windows は、米国および他の国における Microsoft Corporation の登録商標または商標です。</span><span class="sxs-lookup"><span data-stu-id="521b6-692">Microsoft and Windows are either registered trademarks or trademarks of Microsoft Corporation in the United States and/or other countries.</span></span>

<span data-ttu-id="521b6-693">本ドキュメントで説明する実際の製造元および製品名は、それぞれの所有者の商標である場合があります。</span><span class="sxs-lookup"><span data-stu-id="521b6-693">The names of actual companies and products mentioned herein may be the trademarks of their respective owners.</span></span>
