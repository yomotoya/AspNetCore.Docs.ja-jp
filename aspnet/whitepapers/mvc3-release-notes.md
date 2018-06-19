---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 |Microsoft ドキュメント
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 0bfe9cdc215226457ccfafff2b85ace87325b91b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
ms.locfileid: "30899104"
---
<a name="aspnet-mvc-3"></a><span data-ttu-id="02b53-102">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="02b53-102">ASP.NET MVC 3</span></span>
====================
- [<span data-ttu-id="02b53-103">概要</span><span class="sxs-lookup"><span data-stu-id="02b53-103">Overview</span></span>](#overview)
- [<span data-ttu-id="02b53-104">インストールに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="02b53-104">Installation Notes</span></span>](#installation-notes)
- [<span data-ttu-id="02b53-105">ソフトウェアの要件</span><span class="sxs-lookup"><span data-stu-id="02b53-105">Software Requirements</span></span>](#software-requirements)
- [<span data-ttu-id="02b53-106">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="02b53-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="02b53-107">サポート</span><span class="sxs-lookup"><span data-stu-id="02b53-107">Support</span></span>](#support)
- [<span data-ttu-id="02b53-108">3 Tools Update の ASP.NET MVC に ASP.NET MVC 2 プロジェクトをアップグレードします。</span><span class="sxs-lookup"><span data-stu-id="02b53-108">Upgrading an ASP.NET MVC 2 Project to ASP.NET MVC 3 Tools Update</span></span>](#upgrading)
- [<span data-ttu-id="02b53-109">ASP.NET MVC 3 Tools Update (2011 年 4 月 12 日)</span><span class="sxs-lookup"><span data-stu-id="02b53-109">ASP.NET MVC 3 Tools Update (April 12, 2011)</span></span>](#tu-changes)

    - [<span data-ttu-id="02b53-110">"コント ローラーの追加 ダイアログ ボックスがビューとデータのアクセス コードを含むコント ローラーをスキャフォールディングが可能</span><span class="sxs-lookup"><span data-stu-id="02b53-110">"Add Controller" dialog box can now scaffold controllers with views and data access code</span></span>](#tu-AddControllerDialog)
    - [<span data-ttu-id="02b53-111">機能強化、"ASP.NET MVC 3 プロジェクトを新しい" ダイアログ ボックス</span><span class="sxs-lookup"><span data-stu-id="02b53-111">Improvements to the "ASP.NET MVC 3 New Project" Dialog Box</span></span>](#tu-ImprovementsNewDialogBox)
    - [<span data-ttu-id="02b53-112">プロジェクト テンプレートに Modernizr 1.7 が</span><span class="sxs-lookup"><span data-stu-id="02b53-112">Project templates now include Modernizr 1.7</span></span>](#tu-Modernizr)
    - [<span data-ttu-id="02b53-113">プロジェクト テンプレートは、jQuery、jQuery UI、および jQuery の更新バージョンの検証</span><span class="sxs-lookup"><span data-stu-id="02b53-113">Project templates include updated versions of jQuery, jQuery UI, and jQuery Validation</span></span>](#tu-UpdatedJQuery)
    - [<span data-ttu-id="02b53-114">プロジェクト テンプレートでは、事前インストールされている NuGet パッケージとして ADO.NET Entity Framework 4.1 が追加されました</span><span class="sxs-lookup"><span data-stu-id="02b53-114">Project templates now include ADO.NET Entity Framework 4.1 as a pre-installed NuGet package</span></span>](#tu-EF)
    - [<span data-ttu-id="02b53-115">プロジェクト テンプレートは、NuGet プレインストール パッケージとして JavaScript ライブラリを含める</span><span class="sxs-lookup"><span data-stu-id="02b53-115">Project templates include JavaScript libraries as pre-installed NuGet packages</span></span>](#tu-JavaScriptLibsNuget)
    - [<span data-ttu-id="02b53-116">既知の問題</span><span class="sxs-lookup"><span data-stu-id="02b53-116">Known Issues</span></span>](#tu-KI)
- [<span data-ttu-id="02b53-117">ASP.NET MVC 3 RTM (2011 年 1 月 13 日)</span><span class="sxs-lookup"><span data-stu-id="02b53-117">ASP.NET MVC 3 RTM (January 13, 2011)</span></span>](#MVC3RTM)

    - [<span data-ttu-id="02b53-118">変更: 1.8.7 に jQuery UI のバージョンを更新します。</span><span class="sxs-lookup"><span data-stu-id="02b53-118">Change: Updated the version of jQuery UI to 1.8.7</span></span>](#RTM-1)
    - [<span data-ttu-id="02b53-119">変更: ModelMetadataProvider バックアップ DataAnnotationsModelMetadataProvider 既定を変更します。</span><span class="sxs-lookup"><span data-stu-id="02b53-119">Change: Changed the default ModelMetadataProvider back to DataAnnotationsModelMetadataProvider</span></span>](#RTM-2)
    - [<span data-ttu-id="02b53-120">修正: 取り消し中に空白の結果を含む Razor 式の一部を貼り付ける</span><span class="sxs-lookup"><span data-stu-id="02b53-120">Fixed: Pasting part of a Razor expression that contains whitespace results in it being reversed</span></span>](#RTM-3)
    - [<span data-ttu-id="02b53-121">修正: エディターで開かれている Razor ファイルの名前を変更して無効にして構文の色表示機能 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="02b53-121">Fixed: Renaming a Razor file that is opened in the editor disables syntax colorization and IntelliSense</span></span>](#RTM-4)
    - [<span data-ttu-id="02b53-122">既知の問題</span><span class="sxs-lookup"><span data-stu-id="02b53-122">Known Issues</span></span>](#RTM-KI)
    - [<span data-ttu-id="02b53-123">重大な変更</span><span class="sxs-lookup"><span data-stu-id="02b53-123">Breaking Changes</span></span>](#RTM-BC)
- [<span data-ttu-id="02b53-124">ASP.NET MVC 3 Release Candidate 2 (2010 年 12 月 10 日)</span><span class="sxs-lookup"><span data-stu-id="02b53-124">ASP.NET MVC 3 Release Candidate 2 (December 10, 2010)</span></span>](#_Toc2)

    - [<span data-ttu-id="02b53-125">1.4.4 jQuery、jQuery 検証 1.7 と jQuery UI 1.8.6y UI 1.8.6 を含むようにプロジェクト テンプレートの変更</span><span class="sxs-lookup"><span data-stu-id="02b53-125">Project Templates Changed to Include jQuery 1.4.4, jQuery Validation 1.7, and jQuery UI 1.8.6y UI 1.8.6</span></span>](#_Toc2_1)
    - [<span data-ttu-id="02b53-126">追加された"AdditionalMetadataAttribute"クラス</span><span class="sxs-lookup"><span data-stu-id="02b53-126">Added "AdditionalMetadataAttribute" Class</span></span>](#_Toc2_2)
    - [<span data-ttu-id="02b53-127">強化されたビューのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="02b53-127">Improved View Scaffolding</span></span>](#_Toc2_3)
    - [<span data-ttu-id="02b53-128">追加された Html.Raw メソッド</span><span class="sxs-lookup"><span data-stu-id="02b53-128">Added Html.Raw Method</span></span>](#_Toc2_3)
    - [<span data-ttu-id="02b53-129">名前が変更された"Controller.ViewModel"プロパティと"ViewBag"を"View"プロパティ</span><span class="sxs-lookup"><span data-stu-id="02b53-129">Renamed "Controller.ViewModel" Property and the "View" Property To "ViewBag"</span></span>](#_Toc2_4)
    - [<span data-ttu-id="02b53-130">"SessionStateAttribute"に名前が変更された"ControllerSessionStateAttribute"クラス</span><span class="sxs-lookup"><span data-stu-id="02b53-130">Renamed "ControllerSessionStateAttribute" Class to "SessionStateAttribute"</span></span>](#_Toc2_5)
    - [<span data-ttu-id="02b53-131">「ため」に名前を変更した RemoteAttribute"Fields"プロパティ</span><span class="sxs-lookup"><span data-stu-id="02b53-131">Renamed RemoteAttribute "Fields" Property to "AdditionalFields"</span></span>](#_Toc2_6)
    - [<span data-ttu-id="02b53-132">"AllowHtmlAttribute"を"SkipRequestValidationAttribute"の名前を変更</span><span class="sxs-lookup"><span data-stu-id="02b53-132">Renamed "SkipRequestValidationAttribute" to "AllowHtmlAttribute"</span></span>](#_Toc2_7)
    - [<span data-ttu-id="02b53-133">最初の有用なエラー メッセージを表示して変更された"Html.ValidationMessage"メソッド</span><span class="sxs-lookup"><span data-stu-id="02b53-133">Changed "Html.ValidationMessage" Method to Display the First Useful Error Message</span></span>](#_Toc2_8)
    - [<span data-ttu-id="02b53-134">固定@model宣言、ドキュメントに空白を追加するには</span><span class="sxs-lookup"><span data-stu-id="02b53-134">Fixed @model Declaration to not Add Whitespace to the Document</span></span>](#_Toc2_9)
    - [<span data-ttu-id="02b53-135">エンジンに固有のファイル名をサポートするために、ビュー エンジンに追加された"FileExtensions"プロパティ</span><span class="sxs-lookup"><span data-stu-id="02b53-135">Added "FileExtensions" Property to View Engines to Support Engine-Specific File Names</span></span>](#_Toc2_10)
    - [<span data-ttu-id="02b53-136">"For"属性の適切な値を出力する固定"LabelFor"ヘルパー</span><span class="sxs-lookup"><span data-stu-id="02b53-136">Fixed "LabelFor" Helper to Emit the Correct Value for the "For" Attribute</span></span>](#_Toc2_11)
    - [<span data-ttu-id="02b53-137">モデル バインド中に明示的な値を優先する固定"RenderAction"メソッド</span><span class="sxs-lookup"><span data-stu-id="02b53-137">Fixed "RenderAction" Method to Give Explicit Values Precedence During Model Binding</span></span>](#_Toc2_12)
    - [<span data-ttu-id="02b53-138">重大な変更</span><span class="sxs-lookup"><span data-stu-id="02b53-138">Breaking Changes</span></span>](#_Toc2_BC)
    - [<span data-ttu-id="02b53-139">既知の問題</span><span class="sxs-lookup"><span data-stu-id="02b53-139">Known Issues</span></span>](#_Toc2_KI)
- [<span data-ttu-id="02b53-140">ASP.NET MVC 3 リリース候補 (2010 年 11 月 9 日)</span><span class="sxs-lookup"><span data-stu-id="02b53-140">ASP.NET MVC 3 Release Candidate (Nov 9, 2010)</span></span>](#TOC_ASP_NET_3_RC)

    - [<span data-ttu-id="02b53-141">ASP.NET MVC 3 RC の新機能</span><span class="sxs-lookup"><span data-stu-id="02b53-141">New Features in ASP.NET MVC 3 RC</span></span>](#_Toc276711785)
    - [<span data-ttu-id="02b53-142">NuGet パッケージ マネージャー</span><span class="sxs-lookup"><span data-stu-id="02b53-142">NuGet Package Manager</span></span>](#_Toc276711786)
    - [<span data-ttu-id="02b53-143">向上""新しいプロジェクト ダイアログ ボックス</span><span class="sxs-lookup"><span data-stu-id="02b53-143">Improved "New Project" Dialog Box</span></span>](#_Toc276711787)
    - [<span data-ttu-id="02b53-144">Sessionless コント ローラー</span><span class="sxs-lookup"><span data-stu-id="02b53-144">Sessionless Controllers</span></span>](#_Toc276711788)
    - [<span data-ttu-id="02b53-145">新しい検証属性</span><span class="sxs-lookup"><span data-stu-id="02b53-145">New Validation Attributes</span></span>](#_Toc276711789)
    - [<span data-ttu-id="02b53-146">"LabelFor"と"LabelForModel"メソッドの新しいオーバー ロード</span><span class="sxs-lookup"><span data-stu-id="02b53-146">New Overloads for "LabelFor" and "LabelForModel" Methods</span></span>](#_Toc276711790)
    - [<span data-ttu-id="02b53-147">子アクション出力キャッシュ</span><span class="sxs-lookup"><span data-stu-id="02b53-147">Child Action Output Caching</span></span>](#_Toc276711791)
    - [<span data-ttu-id="02b53-148">"ビューの追加 ダイアログ ボックスの機能強化</span><span class="sxs-lookup"><span data-stu-id="02b53-148">"Add View" Dialog Box Improvements</span></span>](#_Toc276711792)
    - [<span data-ttu-id="02b53-149">詳細な要求の検証</span><span class="sxs-lookup"><span data-stu-id="02b53-149">Granular Request Validation</span></span>](#_Toc276711793)
    - [<span data-ttu-id="02b53-150">重大な変更</span><span class="sxs-lookup"><span data-stu-id="02b53-150">Breaking Changes</span></span>](#_Toc276711794)
    - [<span data-ttu-id="02b53-151">既知の問題</span><span class="sxs-lookup"><span data-stu-id="02b53-151">Known Issues</span></span>](#_Toc276711795)
- [<span data-ttu-id="02b53-152">ASP です。MVC 3 ベータ ノート (2010 年 10 月 6 日)</span><span class="sxs-lookup"><span data-stu-id="02b53-152">ASP.MVC 3 Beta Notes (Oct 6, 2010)</span></span>](#TOC_ASP_NET_3_Beta)

    - [<span data-ttu-id="02b53-153">ASP.NET MVC 3 のベータ版の新機能</span><span class="sxs-lookup"><span data-stu-id="02b53-153">New Features in ASP.NET MVC 3 Beta</span></span>](#0.1__Toc274034215)
    - [<span data-ttu-id="02b53-154">NuPack パッケージ マネージャー</span><span class="sxs-lookup"><span data-stu-id="02b53-154">NuPack Package Manager</span></span>](#0.1__Toc274034216)
    - <span data-ttu-id="02b53-155">[[新しいプロジェクト] ダイアログ ボックスの向上](#0.1__Toc274034217)</span><span class="sxs-lookup"><span data-stu-id="02b53-155">[Improved New Project Dialog Box](#0.1__Toc274034217)</span></span>
    - [<span data-ttu-id="02b53-156">厳密に指定することは簡略化された Razor ビューでのモデルの入力</span><span class="sxs-lookup"><span data-stu-id="02b53-156">Simplified Way to Specify Strongly Typed Models in Razor Views</span></span>](#0.1__Toc274034218)
    - [<span data-ttu-id="02b53-157">新しい ASP.NET Web ページのヘルパー メソッドをサポートします。</span><span class="sxs-lookup"><span data-stu-id="02b53-157">Support for New ASP.NET Web Pages Helper Methods</span></span>](#0.1__Toc274034219)
    - [<span data-ttu-id="02b53-158">追加の依存関係の挿入のサポート</span><span class="sxs-lookup"><span data-stu-id="02b53-158">Additional Dependency Injection Support</span></span>](#0.1__Toc274034220)
    - [<span data-ttu-id="02b53-159">控えめな jQuery ベース Ajax のサポート</span><span class="sxs-lookup"><span data-stu-id="02b53-159">New Support for Unobtrusive jQuery-Based Ajax</span></span>](#0.1__Toc274034221)
    - [<span data-ttu-id="02b53-160">控えめな jQuery 検証用の新しいサポート</span><span class="sxs-lookup"><span data-stu-id="02b53-160">New Support for Unobtrusive jQuery Validation</span></span>](#0.1__Toc274034222)
    - [<span data-ttu-id="02b53-161">クライアント検証と控えめな JavaScript の新しいアプリケーション全体のフラグ</span><span class="sxs-lookup"><span data-stu-id="02b53-161">New Application-Wide Flags for Client Validation and Unobtrusive JavaScript</span></span>](#0.1__Toc274034223)
    - [<span data-ttu-id="02b53-162">ビューの実行前に実行されるコードのサポート</span><span class="sxs-lookup"><span data-stu-id="02b53-162">New Support for Code that Runs Before Views Run</span></span>](#0.1__Toc274034224)
    - [<span data-ttu-id="02b53-163">VBHTML Razor 構文の新しいサポート</span><span class="sxs-lookup"><span data-stu-id="02b53-163">New Support for the VBHTML Razor Syntax</span></span>](#0.1__Toc274034225)
    - [<span data-ttu-id="02b53-164">ValidateInputAttribute をより細かく制御</span><span class="sxs-lookup"><span data-stu-id="02b53-164">More Granular Control over ValidateInputAttribute</span></span>](#0.1__Toc274034226)
    - [<span data-ttu-id="02b53-165">ヘルパーの匿名オブジェクトを使用して指定された HTML 属性名にアンダー スコアをハイフンに変換します。</span><span class="sxs-lookup"><span data-stu-id="02b53-165">Helpers Convert Underscores to Hyphens for HTML Attribute Names Specified Using Anonymous Objects</span></span>](#0.1__Toc274034227)
    - [<span data-ttu-id="02b53-166">バグの修正</span><span class="sxs-lookup"><span data-stu-id="02b53-166">Bug Fixes</span></span>](#0.1__Toc274034228)
    - [<span data-ttu-id="02b53-167">重大な変更</span><span class="sxs-lookup"><span data-stu-id="02b53-167">Breaking Changes</span></span>](#0.1__Toc274034229)
    - [<span data-ttu-id="02b53-168">既知の問題</span><span class="sxs-lookup"><span data-stu-id="02b53-168">Known Issues</span></span>](#0.1__Toc274034230)
- [<span data-ttu-id="02b53-169">免責事項</span><span class="sxs-lookup"><span data-stu-id="02b53-169">Disclaimer</span></span>](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a><span data-ttu-id="02b53-170">概要</span><span class="sxs-lookup"><span data-stu-id="02b53-170">Overview</span></span>

<span data-ttu-id="02b53-171">このドキュメントでは、Visual Studio 2010 用の ASP.NET MVC 3 RTM のリリースについて説明します。</span><span class="sxs-lookup"><span data-stu-id="02b53-171">This document describes the release of ASP.NET MVC 3 RTM for Visual Studio 2010.</span></span> <span data-ttu-id="02b53-172">ASP.NET MVC は、モデル ビュー コント ローラー (MVC) パターンを使用して、Web アプリケーションを開発するためのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="02b53-172">ASP.NET MVC is a framework for developing Web applications that uses the Model-View-Controller (MVC) pattern.</span></span> <span data-ttu-id="02b53-173">ASP.NET MVC 3 インストーラーには、次のコンポーネントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-173">The ASP.NET MVC 3 installer includes the following components:</span></span>

- <span data-ttu-id="02b53-174">ASP.NET MVC 3 ランタイム コンポーネント</span><span class="sxs-lookup"><span data-stu-id="02b53-174">ASP.NET MVC 3 runtime components</span></span>
- <span data-ttu-id="02b53-175">ASP.NET MVC 3 Visual Studio 2010 ツール</span><span class="sxs-lookup"><span data-stu-id="02b53-175">ASP.NET MVC 3 Visual Studio 2010 tools</span></span>
- <span data-ttu-id="02b53-176">ASP.NET Web ページの実行時コンポーネント</span><span class="sxs-lookup"><span data-stu-id="02b53-176">ASP.NET Web Pages run-time components</span></span>
- <span data-ttu-id="02b53-177">ASP.NET Web ページ Visual Studio 2010 ツール</span><span class="sxs-lookup"><span data-stu-id="02b53-177">ASP.NET Web Pages Visual Studio 2010 tools</span></span>
- <span data-ttu-id="02b53-178">Microsoft Package Manager for .NET (NuGet)</span><span class="sxs-lookup"><span data-stu-id="02b53-178">Microsoft Package Manager for .NET (NuGet)</span></span>
- <span data-ttu-id="02b53-179">Razor 構文のサポートを有効にする Visual Studio 2010 用の更新プログラム。</span><span class="sxs-lookup"><span data-stu-id="02b53-179">An update for Visual Studio 2010 that enables support for Razor syntax.</span></span> <span data-ttu-id="02b53-180">(詳細については、サポート技術情報の記事 2483190 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="02b53-180">(For details, see KnowledgeBase article 2483190.)</span></span>

<span data-ttu-id="02b53-181">ASP.NET MVC 3 の各リリース前のバージョンのリリース ノートの完全なセットは、次の URL で ASP.NET web サイトで確認できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-181">The full set of release notes for each pre-release version of ASP.NET MVC 3 can be found on the ASP.NET website at the following URL:</span></span>

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a><span data-ttu-id="02b53-182">インストールに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="02b53-182">Installation Notes</span></span>

<span data-ttu-id="02b53-183">ASP.NET MVC 3 RTM、Web Platform Installer (Web PI) を使用してをインストールするには、次のページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="02b53-183">To install ASP.NET MVC 3 RTM using the Web Platform Installer (Web PI), visit the following page:</span></span>

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

<span data-ttu-id="02b53-184">代わりに、次のページから Visual Studio 2010 の ASP.NET MVC 3 RTM 用のインストーラーをダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="02b53-184">Alternatively, you can download the installer for ASP.NET MVC 3 RTM for Visual Studio 2010 from the following page:</span></span>

https://go.microsoft.com/fwlink/?LinkID=208140

<span data-ttu-id="02b53-185">ASP.NET MVC 3 をインストールして、サイド バイ サイドで実行できる ASP.NET MVC 2 とします。</span><span class="sxs-lookup"><span data-stu-id="02b53-185">ASP.NET MVC 3 can be installed and can run side-by-side with ASP.NET MVC 2.</span></span>

<a id="software-requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="02b53-186">ソフトウェア要件</span><span class="sxs-lookup"><span data-stu-id="02b53-186">Software Requirements</span></span>

<span data-ttu-id="02b53-187">ASP.NET MVC 3 ランタイム コンポーネントには、次のソフトウェアが必要です。</span><span class="sxs-lookup"><span data-stu-id="02b53-187">The ASP.NET MVC 3 run-time components require the following software:</span></span>

- <span data-ttu-id="02b53-188">.NET framework version 4。</span><span class="sxs-lookup"><span data-stu-id="02b53-188">.NET Framework version 4.</span></span> 

    <span data-ttu-id="02b53-189">ASP.NET MVC 3 Visual Studio 2010 ツールには、次のソフトウェアが必要です。</span><span class="sxs-lookup"><span data-stu-id="02b53-189">ASP.NET MVC 3 Visual Studio 2010 tools require the following software:</span></span>
- <span data-ttu-id="02b53-190">Visual Studio 2010 または Visual Web Developer 2010 Express です。</span><span class="sxs-lookup"><span data-stu-id="02b53-190">Visual Studio 2010 or Visual Web Developer 2010 Express.</span></span>

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="02b53-191">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="02b53-191">Documentation</span></span>

<span data-ttu-id="02b53-192">ASP.NET MVC のドキュメントは、次の URL で MSDN Web サイトで入手できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-192">Documentation for ASP.NET MVC is available on the MSDN Web site at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

<span data-ttu-id="02b53-193">チュートリアルおよび ASP.NET MVC に関する他の情報を次の URL で ASP.NET Web サイトの MVC ページで使用できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-193">Tutorials and other information about ASP.NET MVC are available on the MVC page of the ASP.NET Web site at the following URL:</span></span>

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a><span data-ttu-id="02b53-194">サポート</span><span class="sxs-lookup"><span data-stu-id="02b53-194">Support</span></span>

<span data-ttu-id="02b53-195">これは、完全にサポートされるリリースです。</span><span class="sxs-lookup"><span data-stu-id="02b53-195">This is a fully supported release.</span></span> <span data-ttu-id="02b53-196">テクニカル サポートについての情報が見つかりません、 [Microsoft サポート web サイト](https://support.microsoft.com/)です。</span><span class="sxs-lookup"><span data-stu-id="02b53-196">Information about getting technical support can be found at the [Microsoft Support website](https://support.microsoft.com/).</span></span>

<span data-ttu-id="02b53-197">自由にここで、ASP.NET コミュニティのメンバーは、頻繁に非公式のサポートを提供する、ASP.NET MVC フォーラムへのこのリリースに関する質問を投稿します。</span><span class="sxs-lookup"><span data-stu-id="02b53-197">Also feel free to post questions about this release to the ASP.NET MVC forum, where members of the ASP.NET community are frequently able to provide informal support:</span></span>

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a><span data-ttu-id="02b53-198">3 Tools Update の ASP.NET MVC に ASP.NET MVC 2 プロジェクトをアップグレードします。</span><span class="sxs-lookup"><span data-stu-id="02b53-198">Upgrading an ASP.NET MVC 2 Project to ASP.NET MVC 3 Tools Update</span></span>

<span data-ttu-id="02b53-199">ASP.NET MVC 3 は、する柔軟性が ASP.NET MVC 3、ASP.NET MVC 2 アプリケーションをアップグレードするタイミングを選択する際に、同じコンピューターに ASP.NET MVC 2 のサイド バイ サイドでインストールできます。</span><span class="sxs-lookup"><span data-stu-id="02b53-199">ASP.NET MVC 3 can be installed side by side with ASP.NET MVC 2 on the same computer, which gives you flexibility in choosing when to upgrade an ASP.NET MVC 2 application to ASP.NET MVC 3.</span></span>

<span data-ttu-id="02b53-200">バージョン 3 への既存の ASP.NET MVC 2 アプリケーションを手動でアップグレードするには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="02b53-200">To manually upgrade an existing ASP.NET MVC 2 application to version 3, do the following:</span></span>

1. <span data-ttu-id="02b53-201">コンピューターに新しい空の ASP.NET MVC 3 プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="02b53-201">Create a new empty ASP.NET MVC 3 project on your computer.</span></span> <span data-ttu-id="02b53-202">このプロジェクトにはアップグレードに必要ないくつかのファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="02b53-202">This project will contain some files that are required for the upgrade.</span></span>
2. <span data-ttu-id="02b53-203">ASP.NET MVC 3 プロジェクトから ASP.NET MVC 2 プロジェクトの対応する場所に、次のファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="02b53-203">Copy the following files from the ASP.NET MVC 3 project into the corresponding location of your ASP.NET MVC 2 project.</span></span> <span data-ttu-id="02b53-204">新しいファイル名 (jquery-1.5.1.js) のために、jQuery ライブラリへの参照を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-204">You'll need to update any references to the jQuery library to account for the new filename ( jQuery-1.5.1.js):</span></span> 

    - <span data-ttu-id="02b53-205">/Views/Web.config</span><span class="sxs-lookup"><span data-stu-id="02b53-205">/Views/Web.config</span></span>
    - <span data-ttu-id="02b53-206">/packages.config</span><span class="sxs-lookup"><span data-stu-id="02b53-206">/packages.config</span></span>
    - <span data-ttu-id="02b53-207">/scripts/\*.js</span><span class="sxs-lookup"><span data-stu-id="02b53-207">/scripts/\*.js</span></span>
    - <span data-ttu-id="02b53-208">/Content/themes/\*.\*</span><span class="sxs-lookup"><span data-stu-id="02b53-208">/Content/themes/\*.\*</span></span>
3. <span data-ttu-id="02b53-209">コピー、*パッケージ*ソリューションの .sln ファイルがあるディレクトリでは、ソリューションのルートには、空の ASP.NET MVC 3 プロジェクト ソリューションのルートにフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="02b53-209">Copy the *packages* folder in the root of the empty ASP.NET MVC 3 project solution into the root of your solution, which is in the directory where the solution's .sln file is located.</span></span>
4. <span data-ttu-id="02b53-210">ASP.NET MVC 2 プロジェクトにすべての領域が含まれている場合、/Views/Web.config ファイルのコピー、*ビュー*区分のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="02b53-210">If your ASP.NET MVC 2 project contains any areas, copy the /Views/Web.config file to the *Views* folder of each area.</span></span>
5. <span data-ttu-id="02b53-211">ASP.NET MVC 2 プロジェクトの両方の Web.config ファイルでグローバル検索し、ASP.NET MVC のバージョンを置換します。</span><span class="sxs-lookup"><span data-stu-id="02b53-211">In both Web.config files in the ASP.NET MVC 2 project, globally search and replace the ASP.NET MVC version.</span></span> <span data-ttu-id="02b53-212">次を検索します。</span><span class="sxs-lookup"><span data-stu-id="02b53-212">Find the following:</span></span> 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    <span data-ttu-id="02b53-213">次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="02b53-213">Replace it with the following:</span></span>

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. <span data-ttu-id="02b53-214">ソリューション エクスプ ローラーへの参照を削除*System.Web.Mvc* (指し示します DLL バージョン 2 から) への参照を追加し、 *System.Web.Mvc* (v3.0.0.0)。</span><span class="sxs-lookup"><span data-stu-id="02b53-214">In Solution Explorer, delete the reference to *System.Web.Mvc* (which points to the DLL from version 2), then add a reference to *System.Web.Mvc* (v3.0.0.0).</span></span>
7. <span data-ttu-id="02b53-215">System.Web.WebPages.dll および System.Web.Helpers.dll への参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="02b53-215">Add a reference to System.Web.WebPages.dll and System.Web.Helpers.dll.</span></span> <span data-ttu-id="02b53-216">これらのアセンブリは、次のフォルダーに配置されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-216">These assemblies are located in the following folders:</span></span> 

    - <span data-ttu-id="02b53-217">%ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies</span><span class="sxs-lookup"><span data-stu-id="02b53-217">%ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies</span></span>
    - <span data-ttu-id="02b53-218">%ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies</span><span class="sxs-lookup"><span data-stu-id="02b53-218">%ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies</span></span>
8. <span data-ttu-id="02b53-219">ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="02b53-219">In Solution Explorer, right-click the project name and select Unload Project.</span></span> <span data-ttu-id="02b53-220">[プロジェクト名を再度右クリックして編集] を選択*ProjectName*.csproj です。</span><span class="sxs-lookup"><span data-stu-id="02b53-220">Then right-click the project name again and select Edit *ProjectName*.csproj.</span></span>
9. <span data-ttu-id="02b53-221">検索、 *ProjectTypeGuids*要素と置換 {f85e285d-a4e0-4152-9332-ab1d724d3325} を {e53f8fea-eae0-44a6-8774-ffd645390401} です。</span><span class="sxs-lookup"><span data-stu-id="02b53-221">Locate the *ProjectTypeGuids* element and replace {F85E285D-A4E0-4152-9332-AB1D724D3325} with {E53F8FEA-EAE0-44A6-8774-FFD645390401}.</span></span>
10. <span data-ttu-id="02b53-222">変更を保存し、プロジェクトを右クリックし、プロジェクトの再読み込みを選択します。</span><span class="sxs-lookup"><span data-stu-id="02b53-222">Save the changes, right-click the project, and then select Reload Project.</span></span>
11. <span data-ttu-id="02b53-223">アプリケーションのルートの Web.config ファイル内に次の設定を追加、*アセンブリ*セクションです。</span><span class="sxs-lookup"><span data-stu-id="02b53-223">In the application's root Web.config file, add the following settings to the *assemblies* section.</span></span> 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. <span data-ttu-id="02b53-224">プロジェクトでは、ASP.NET MVC 2 を使用してコンパイルされたすべてのサード パーティ製ライブラリを参照する場合は、強調表示されている、次を追加*bindingRedirect*下のアプリケーション ルートの Web.config ファイルに要素、 *構成*セクション。</span><span class="sxs-lookup"><span data-stu-id="02b53-224">If the project references any third-party libraries that are compiled using ASP.NET MVC 2, add the following highlighted *bindingRedirect* element to the Web.config file in the application root under the *configuration* section:</span></span> 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a><span data-ttu-id="02b53-225">ASP.NET MVC 3 での変更ツールを更新します。</span><span class="sxs-lookup"><span data-stu-id="02b53-225">Changes in ASP.NET MVC 3 Tools Update</span></span>

<span data-ttu-id="02b53-226">このセクションでは、ASP.NET MVC 3 Tools Update リリースでは、ASP.NET MVC 3 RTM リリース以降の変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="02b53-226">This section describes changes made in the ASP.NET MVC 3 Tools Update release since the ASP.NET MVC 3 RTM release.</span></span>

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a><span data-ttu-id="02b53-227">"コント ローラーの追加 ダイアログ ボックスがビューとデータのアクセス コードを含むコント ローラーをスキャフォールディングが可能</span><span class="sxs-lookup"><span data-stu-id="02b53-227">"Add Controller" dialog box can now scaffold controllers with views and data access code</span></span>

<span data-ttu-id="02b53-228">スキャフォールディングは、コント ローラーと、アプリケーションのビューをすばやく生成する方法です。</span><span class="sxs-lookup"><span data-stu-id="02b53-228">Scaffolding is a way of quickly generating a controller and views for your application.</span></span> <span data-ttu-id="02b53-229">コードが生成された後をプロジェクトの要件を満たすように編集できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-229">After the code has been generated, you can edit it to meet your project's requirements.</span></span>

<span data-ttu-id="02b53-230">起動する、*コント ローラーの追加*ASP.NET MVC 3、ダイアログ ボックスを右クリックし、*コント ローラー*フォルダー*ソリューション エクスプ ローラー*をクリックして*追加*、クリックして*コント ローラー*です。</span><span class="sxs-lookup"><span data-stu-id="02b53-230">To launch the *Add Controller* dialog box in ASP.NET MVC 3, right-click the *Controllers* folder in *Solution Explorer*, click *Add*, and then click *Controller*.</span></span> <span data-ttu-id="02b53-231">ダイアログ ボックスは、追加のスキャフォールディングのオプションを提供する拡張されています。</span><span class="sxs-lookup"><span data-stu-id="02b53-231">The dialog box has been enhanced to offer additional scaffolding options.</span></span>

![](mvc3-release-notes/_static/image1.png)

<span data-ttu-id="02b53-232">次の 3 つスキャフォールディング テンプレートが使用可能な既定です。</span><span class="sxs-lookup"><span data-stu-id="02b53-232">There are three scaffolding templates available by default.</span></span>

#### <a name="empty-controller"></a><span data-ttu-id="02b53-233">空のコント ローラー</span><span class="sxs-lookup"><span data-stu-id="02b53-233">Empty Controller</span></span>

<span data-ttu-id="02b53-234">このテンプレートは、空のコント ローラー ファイルを生成します。</span><span class="sxs-lookup"><span data-stu-id="02b53-234">This template generates an empty controller file.</span></span> <span data-ttu-id="02b53-235">このテンプレートは確認せずに相当*作成、編集、詳細のアクションを追加、削除のシナリオ*ASP.NET MVC の以前のバージョン。</span><span class="sxs-lookup"><span data-stu-id="02b53-235">This template is equivalent to not checking *Add actions for create, edit, details, delete scenarios* in previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="02b53-236">この場合は、他のオプションはありません。</span><span class="sxs-lookup"><span data-stu-id="02b53-236">If you choose this, no further options are available.</span></span>

#### <a name="controller-with-empty-readwrite-actions"></a><span data-ttu-id="02b53-237">空の読み取り/書き込みアクションのコント ローラー</span><span class="sxs-lookup"><span data-stu-id="02b53-237">Controller with empty read/write actions</span></span>

<span data-ttu-id="02b53-238">このテンプレートは、メソッドのすべての必要なアクション メソッド コードではなく実装にコント ローラー ファイルを生成します。</span><span class="sxs-lookup"><span data-stu-id="02b53-238">This template generates a controller file that has all the required action methods but no implementation code in the methods.</span></span> <span data-ttu-id="02b53-239">このテンプレートは、することに相当*作成、編集、詳細のアクションを追加、削除のシナリオ*ASP.NET MVC の以前のバージョン。</span><span class="sxs-lookup"><span data-stu-id="02b53-239">This template is equivalent to checking *Add actions for create, edit, details, delete scenarios* in previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="02b53-240">この場合は、他のオプションはありません。</span><span class="sxs-lookup"><span data-stu-id="02b53-240">If you choose this, no further options are available.</span></span>

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a><span data-ttu-id="02b53-241">読み取り/書き込みアクションおよび Entity Framework を使用して、ビューとコント ローラー</span><span class="sxs-lookup"><span data-stu-id="02b53-241">Controller with read/write actions and views, using Entity Framework</span></span>

<span data-ttu-id="02b53-242">このテンプレートでは、作業データ入力ユーザー インターフェイスをすばやく作成することができます。</span><span class="sxs-lookup"><span data-stu-id="02b53-242">This template enables you to quickly create a working data-entry user interface.</span></span> <span data-ttu-id="02b53-243">一般的な要件と、次のように、シナリオの範囲を処理するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-243">It generates code that handles a range of common requirements and scenarios, such as the following:</span></span>

- <span data-ttu-id="02b53-244">*データ アクセス*です。</span><span class="sxs-lookup"><span data-stu-id="02b53-244">*Data access*.</span></span> <span data-ttu-id="02b53-245">生成されたコードは、データベース内でエンティティを読み書きします。</span><span class="sxs-lookup"><span data-stu-id="02b53-245">The generated code reads and writes entities in a database.</span></span> <span data-ttu-id="02b53-246">既存のデータ コンテキスト クラスを選択した場合、または新しいを生成するテンプレートを使用する場合、Entity Framework Code First のアプローチと連携*DbContext*クラスです。</span><span class="sxs-lookup"><span data-stu-id="02b53-246">It works with the Entity Framework Code First approach if you choose an existing data context class or if you let the template generate a new *DbContext* class.</span></span> <span data-ttu-id="02b53-247">既存の選択した場合は、Entity Framework Database First または Model First アプローチででも動作*ObjectContext*クラスです。</span><span class="sxs-lookup"><span data-stu-id="02b53-247">It also works with the Entity Framework Database First or Model First approach if you choose an existing *ObjectContext* class.</span></span>
- <span data-ttu-id="02b53-248">*検証*です。</span><span class="sxs-lookup"><span data-stu-id="02b53-248">*Validation*.</span></span> <span data-ttu-id="02b53-249">生成されたコードは、モデル クラスで宣言されている規則に従ってフォームの送信が検証されるため、ASP.NET MVC モデル バインディング機能とメタデータ機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="02b53-249">The generated code uses ASP.NET MVC model binding and metadata features so that form submissions are validated according to rules declared on your model class.</span></span> <span data-ttu-id="02b53-250">などの組み込みの検証規則が含まれます、*必要*と*StringLength*属性、およびカスタム検証規則。</span><span class="sxs-lookup"><span data-stu-id="02b53-250">This includes built-in validation rules, such as the *Required* and *StringLength* attributes, and custom validation rules.</span></span>
- <span data-ttu-id="02b53-251">*一対多リレーションシップ*です。</span><span class="sxs-lookup"><span data-stu-id="02b53-251">*One-to-many relationships*.</span></span> <span data-ttu-id="02b53-252">モデル クラス間で一対多の外部キー リレーションシップを定義する場合、生成されたコードには関連エンティティを選択するためのドロップダウン リストが生成されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-252">If you define one-to-many foreign-key relationships between your model classes, the generated code will produce drop-down lists for selecting related entities.</span></span> <span data-ttu-id="02b53-253">たとえば、次の Entity Framework Code First 規約に従って次のモデル クラスを定義する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-253">For example, you might define the following model classes following Entity Framework Code First conventions:</span></span> 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    <span data-ttu-id="02b53-254">コント ローラーをスキャフォールディングすると、*製品*クラス、そのビュー、ユーザーが選択、*カテゴリ*ごとにオブジェクト*製品*インスタンス。</span><span class="sxs-lookup"><span data-stu-id="02b53-254">When you then scaffold a controller for the *Product* class, its views will allow users to choose a *Category* object for each *Product* instance.</span></span>

    <span data-ttu-id="02b53-255">このテンプレートの追加のオプションを使用する、*コント ローラーの追加* ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="02b53-255">This template enables additional options in the *Add Controller* dialog box.</span></span> <span data-ttu-id="02b53-256">*モデル クラス*データを作成または編集できるユーザーの種類を決定するソリューションでどのモデル クラスを選択できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-256">For *Model class*, you can choose any model class in your solution, which determines the type of data that users will be able to create or edit:</span></span>
- <span data-ttu-id="02b53-257">Entity Framework Code First を使用する場合は、どのモデル クラスを選択できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-257">If you want to use Entity Framework Code First, you can choose any model class.</span></span>
- <span data-ttu-id="02b53-258">Entity Framework Database First または Entity Framework Model First を使用している場合は、概念モデルで定義されているエンティティ クラスを選択することを確認します。</span><span class="sxs-lookup"><span data-stu-id="02b53-258">If you are using Entity Framework Database First or Entity Framework Model First, be sure to choose an entity class defined in your conceptual model.</span></span>

<span data-ttu-id="02b53-259">*データ コンテキスト クラス*、これらの選択を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="02b53-259">For *Data Context class*, you can make these choices:</span></span>

- <span data-ttu-id="02b53-260">Code First を使用していない既存のデータ コンテキスト クラスを選択する場合 * * 新しいデータ コンテキスト * *。</span><span class="sxs-lookup"><span data-stu-id="02b53-260">If you want to use Code First and have no existing data context class, choose **New data context **.</span></span> <span data-ttu-id="02b53-261">データ コンテキスト クラスは、生成されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-261">A data context class will then be generated for you.</span></span>
- <span data-ttu-id="02b53-262">Code First を使用して既存のデータ コンテキスト クラスがある場合は、ここで選択します。</span><span class="sxs-lookup"><span data-stu-id="02b53-262">If you want to use Code First and have an existing data context class, choose it here.</span></span> <span data-ttu-id="02b53-263">選択したモデル クラスを永続化に更新されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-263">It will be updated to persist the model class you have selected.</span></span>
- <span data-ttu-id="02b53-264">Database First または Model First を使用している場合は、ここで、オブジェクト コンテキスト クラスを選択します。</span><span class="sxs-lookup"><span data-stu-id="02b53-264">If you are using Database First or Model First, choose your object context class here.</span></span>

<span data-ttu-id="02b53-265">ビューの場合、使用するビュー エンジンを選択するかのすべてのビューのスキャフォールディングしたくない場合は、[なし] を選択します。</span><span class="sxs-lookup"><span data-stu-id="02b53-265">For Views, choose the view engine you want to use, or choose None if you don't want to scaffold any views.</span></span>

<span data-ttu-id="02b53-266">選択できる、高度なさらに、生成されたビューのオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="02b53-266">You can select Advanced Optionsto specify further options for the generated views.</span></span> <span data-ttu-id="02b53-267">たとえば、レイアウトまたはマスター ページを使用を選択できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-267">For example, you can choose the layout or master page to use.</span></span>

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a><span data-ttu-id="02b53-268">機能強化、"ASP.NET MVC 3 プロジェクトを新しい" ダイアログ ボックス</span><span class="sxs-lookup"><span data-stu-id="02b53-268">Improvements to the "ASP.NET MVC 3 New Project" Dialog Box</span></span>

<span data-ttu-id="02b53-269">新しい ASP.NET MVC 3 プロジェクトの作成に使用する ダイアログ ボックスには、次に示すように複数の機能強化が含まれます。</span><span class="sxs-lookup"><span data-stu-id="02b53-269">The dialog box you use to create new ASP.NET MVC 3 projects includes multiple improvements, as listed below.</span></span>

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a><span data-ttu-id="02b53-270">新しい「イントラネット プロジェクト」テンプレート</span><span class="sxs-lookup"><span data-stu-id="02b53-270">New "Intranet Project" Template</span></span>

<span data-ttu-id="02b53-271">プロジェクト テンプレートの一覧には、新しいイントラネット アプリケーション テンプレートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-271">The Project Template list includes a new Intranet Application template.</span></span> <span data-ttu-id="02b53-272">このテンプレートには、フォーム認証ではなく Windows 認証を使用して web アプリケーションを構築するための設定が含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-272">This template contains settings for building a web application using Windows authentication instead of forms authentication.</span></span> <span data-ttu-id="02b53-273">イントラネット アプリケーションには、プロジェクトのテンプレートにカプセル化することはできませんのある IIS 設定が必要であるため、テンプレートには、プロジェクト テンプレートを IIS で使用する方法についての記載された readme ファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="02b53-273">Because an intranet application requires some IIS settings that can't be encapsulated in a project template, the template includes a readme file with instructions for how to make the project template work in IIS.</span></span> <span data-ttu-id="02b53-274">ドキュメントを新しいイントラネット アプリケーション テンプレートは、次の URL で MSDN web サイトで使用できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-274">Documentation for the a new Intranet Application template is available on the MSDN website at the following URL:</span></span>

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a><span data-ttu-id="02b53-275">プロジェクト テンプレートが HTML5 に対応</span><span class="sxs-lookup"><span data-stu-id="02b53-275">Project templates are now HTML5 enabled</span></span>

<span data-ttu-id="02b53-276">これで、新しいプロジェクト ダイアログ ボックスには、プロジェクト テンプレートに HTML5 固有の機能を追加するオプションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-276">The new-project dialog box now contains an option to add HTML5-specific features to the project templates.</span></span> <span data-ttu-id="02b53-277">新しい HTML5 を含むビューを生成するオプションを選択すると、 `<header>`、 `<footer>`、および`<navigation>`要素。</span><span class="sxs-lookup"><span data-stu-id="02b53-277">Selecting the option causes views to be generated that contain the new HTML5 `<header>`, `<footer>`, and `<navigation>` elements.</span></span>

<span data-ttu-id="02b53-278">以前のバージョンのブラウザーは HTML5 固有のタグをサポートしていませんことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="02b53-278">Note that earlier versions of browsers do not support HTML5-specific tags.</span></span> <span data-ttu-id="02b53-279">この制限に対処するには、HTML5 プロジェクト テンプレートには、Modernizr ライブラリへの参照が含まれます。</span><span class="sxs-lookup"><span data-stu-id="02b53-279">To address this limitation, the HTML5 project templates include a reference to the Modernizr library.</span></span> <span data-ttu-id="02b53-280">(詳しくは、次のセクションを参照してください)。</span><span class="sxs-lookup"><span data-stu-id="02b53-280">(See the next section.)</span></span>

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a><span data-ttu-id="02b53-281">プロジェクト テンプレートに Modernizr 1.7 が</span><span class="sxs-lookup"><span data-stu-id="02b53-281">Project templates now include Modernizr 1.7</span></span>

<span data-ttu-id="02b53-282">Modernizr は、これらの機能をサポートしていないブラウザーで CSS 3 および HTML5 のサポートを有効にする JavaScript ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="02b53-282">Modernizr is a JavaScript library that enables support for CSS 3 and HTML5 in browsers that do not yet support these features.</span></span> <span data-ttu-id="02b53-283">このライブラリは、ASP.NET MVC 3 プロジェクトのテンプレートでの事前インストールされている NuGet パッケージとして含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-283">This library is included as a pre-installed NuGet package in templates for ASP.NET MVC 3 projects.</span></span> <span data-ttu-id="02b53-284">Modernizr の詳細については、次を参照してください。 [ http://www.modernizr.com/](http://www.modernizr.com/)です。</span><span class="sxs-lookup"><span data-stu-id="02b53-284">For more information about Modernizr, see [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a><span data-ttu-id="02b53-285">プロジェクト テンプレートは、jQuery、jQuery UI、および jQuery の更新バージョンの検証</span><span class="sxs-lookup"><span data-stu-id="02b53-285">Project templates include updated versions of jQuery, jQuery UI, and jQuery Validation</span></span>

<span data-ttu-id="02b53-286">プロジェクト テンプレートには、次のバージョンの jQuery スクリプトが追加されました。</span><span class="sxs-lookup"><span data-stu-id="02b53-286">The project templates now include the following versions of the jQuery scripts:</span></span>

- <span data-ttu-id="02b53-287">jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="02b53-287">jQuery 1.5.1</span></span>
- <span data-ttu-id="02b53-288">jQuery 検証 1.8</span><span class="sxs-lookup"><span data-stu-id="02b53-288">jQuery Validation 1.8</span></span>
- <span data-ttu-id="02b53-289">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="02b53-289">jQuery UI 1.8.11</span></span>

<span data-ttu-id="02b53-290">これらのライブラリは、NuGet プレインストール パッケージとして含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-290">These libraries are included as pre-installed NuGet packages.</span></span>

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a><span data-ttu-id="02b53-291">プロジェクト テンプレートでは、事前インストールされている NuGet パッケージとして ADO.NET Entity Framework 4.1 が追加されました</span><span class="sxs-lookup"><span data-stu-id="02b53-291">Project templates now include ADO.NET Entity Framework 4.1 as a pre-installed NuGet package</span></span>

<span data-ttu-id="02b53-292">ADO.NET Entity Framework 4.1 には、Code First 機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-292">The ADO.NET Entity Framework 4.1 includes the Code First feature.</span></span> <span data-ttu-id="02b53-293">コードは、既存のデータベースと Model First パターンに代えて使用できる ADO.NET Entity Framework 用の新しい開発パターンを最初です。</span><span class="sxs-lookup"><span data-stu-id="02b53-293">Code First is a new development pattern for the ADO.NET Entity Framework that provides an alternative to the existing Database First and Model First patterns.</span></span>

<span data-ttu-id="02b53-294">コードはまず、Visual Basic または c# で記述された POCO クラス ("plain old CLR object") を使用してモデルを定義するフォーカスしています。</span><span class="sxs-lookup"><span data-stu-id="02b53-294">Code First is focused around defining your model using POCO classes ("plain old CLR objects") written in Visual Basic or C#.</span></span> <span data-ttu-id="02b53-295">これらのクラスは、既存のデータベースにマップすることができますか、データベース スキーマを生成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="02b53-295">These classes can then be mapped to an existing database or be used to generate a database schema.</span></span> <span data-ttu-id="02b53-296">使用して追加の構成を指定することができます*DataAnnotations*属性または fluent Api を使用します。</span><span class="sxs-lookup"><span data-stu-id="02b53-296">Additional configuration can be supplied using *DataAnnotations* attributes or using fluent APIs.</span></span>

<span data-ttu-id="02b53-297">コード Firstwith ASP.NET MVC の使用に関するドキュメントは、次の Url で ASP.NET web サイトで使用できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-297">Documentation for using Code Firstwith ASP.NET MVC is available on the ASP.NET website at the following URLs:</span></span>

<span data-ttu-id="02b53-298">[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="02b53-298">[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)</span></span>

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a><span data-ttu-id="02b53-299">プロジェクト テンプレートは、NuGet プレインストール パッケージとして JavaScript ライブラリを含める</span><span class="sxs-lookup"><span data-stu-id="02b53-299">Project templates include JavaScript libraries as pre-installed NuGet packages</span></span>

<span data-ttu-id="02b53-300">プロジェクトがそれらをインストールすることで、JavaScript ファイルに記載されている (たとえば、Modernizr ライブラリ) を含む新しい ASP.NET MVC 3 プロジェクトを作成するときに、プロジェクト テンプレートで、Scripts フォルダーにスクリプトを直接追加する代わりに NuGet を使用します。内容。</span><span class="sxs-lookup"><span data-stu-id="02b53-300">When you create a new ASP.NET MVC 3 project, the project includes the JavaScript files mentioned previously (for example, the Modernizr library) by installing them using NuGet instead of directly adding the scripts to the Scripts folder in the project template contents.</span></span> <span data-ttu-id="02b53-301">これにより、NuGet を使用して、スクリプトの新しいバージョンがリリースされたときに、最新バージョンにスクリプトを更新することができます。</span><span class="sxs-lookup"><span data-stu-id="02b53-301">This enables you to use NuGet to update the scripts to the latest version when new versions of the scripts are released.</span></span>

<span data-ttu-id="02b53-302">たとえば、jQuery の新しいリリースの頻度を指定するには、プロジェクト テンプレートに含まれる jQuery のバージョンがある時点で古くなってます。</span><span class="sxs-lookup"><span data-stu-id="02b53-302">For example, given the frequency of new jQuery releases, the version of jQuery included in the project template will at some point be out of date.</span></span> <span data-ttu-id="02b53-303">ただし、jQuery はインストール済みの NuGet パッケージとして含まれているため、通知されます ダイアログ ボックスで NuGet jQuery の新しいバージョンが利用可能な場合です。</span><span class="sxs-lookup"><span data-stu-id="02b53-303">However, because jQuery is included as an installed NuGet package, you will be notified in the NuGet dialog box when newer versions of jQuery are available.</span></span>

<span data-ttu-id="02b53-304">JQuery には、ファイル名にバージョン番号が含まれているため jQuery を最新バージョンに更新も更新が必要です、`<script>`新しいファイル名を使用する、jQuery ファイルを参照しているタグ。</span><span class="sxs-lookup"><span data-stu-id="02b53-304">Because jQuery includes the version number in the file name, updating jQuery to the latest version also requires updating the `<script>` tag that references the jQuery file to use the new file name.</span></span> <span data-ttu-id="02b53-305">その他のスクリプト ライブラリでは、スクリプト名が、バージョン番号は含まれませんが最新バージョンをより簡単に更新されるようにします。</span><span class="sxs-lookup"><span data-stu-id="02b53-305">Other included script libraries do not include the version number in the script name, so they can be more easily updated to their latest versions.</span></span>

<a id="tu-KI"></a>
## <a name="known-issues"></a><span data-ttu-id="02b53-306">既知の問題</span><span class="sxs-lookup"><span data-stu-id="02b53-306">Known Issues</span></span>

- <span data-ttu-id="02b53-307">場合によっては、インストールが失敗する、エラー メッセージ「のインストールに失敗しましたエラー コード (0x80070643)」です。</span><span class="sxs-lookup"><span data-stu-id="02b53-307">In some cases, installation may fail with the error message "Installation failed with error code (0x80070643)".</span></span> <span data-ttu-id="02b53-308">この問題を回避する方法については、次を参照してください。[サポート技術情報の記事 2531566](https://support.microsoft.com/kb/2531566)です。</span><span class="sxs-lookup"><span data-stu-id="02b53-308">For information about how to work around this issue, see [KnowledgeBase article 2531566](https://support.microsoft.com/kb/2531566).</span></span>
- <span data-ttu-id="02b53-309">コント ローラーを追加するためのスキャフォールディングでは、Entity Framework 内のエンティティ継承サポートを利用するエンティティはスキャフォールディングされません。</span><span class="sxs-lookup"><span data-stu-id="02b53-309">The scaffolding for adding a controller does not scaffold entities that take advantage of entity inheritance support within Entity Framework.</span></span> <span data-ttu-id="02b53-310">など、基数を指定*Person*によって継承されるクラス、*学生*クラスをスキャフォールディング、*学生*クラスのコードはコンパイルされませんが生成されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-310">For example, given a base *Person* class that's inherited by a *Student* class, scaffolding the *Student* class will result in generated code that does not compile.</span></span>
- <span data-ttu-id="02b53-311">原因をソリューション フォルダー内の新しい ASP.NET MVC 3 プロジェクトを作成する、 *NullReferenceException*エラーです。</span><span class="sxs-lookup"><span data-stu-id="02b53-311">Creating a new ASP.NET MVC 3 project within a solution folder causes a *NullReferenceException* error.</span></span> <span data-ttu-id="02b53-312">回避策は、ソリューションのルートに ASP.NET MVC 3 プロジェクトを作成し、ソリューション フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="02b53-312">The workaround is to create the ASP.NET MVC 3 project in the root of the solution and then move it into the solution folder.</span></span>
- <span data-ttu-id="02b53-313">Razor 構文の IntelliSense では、ReSharper がインストールされている場合は機能しません。</span><span class="sxs-lookup"><span data-stu-id="02b53-313">IntelliSense for Razor syntax does not work when ReSharper is installed.</span></span> <span data-ttu-id="02b53-314">ReSharper をインストールして ASP.NET MVC 3 Razor IntelliSense サポートのメリットを利用する場合、エントリを参照してください。 [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri のブログでは、現時点で併用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="02b53-314">If you have ReSharper installed and want to take advantage of the Razor IntelliSense support in ASP.NET MVC 3, see the entry [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) on Hadi Hariri's blog, which discusses ways to use them together today.</span></span>
- <span data-ttu-id="02b53-315">インストール中に、使用許諾契約書への同意 ダイアログ ボックスでは、ものよりも小さいウィンドウをライセンス条項が表示されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-315">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>
- <span data-ttu-id="02b53-316">Razor ビューを編集する場合 (.cshtml または *。vbhtml*ファイル) を表示します。</span><span class="sxs-lookup"><span data-stu-id="02b53-316">When you are editing a Razor view (.cshtml or .*vbhtml* file), views.</span></span> <span data-ttu-id="02b53-317">ASP.NET MVC 3 では、Razor ビューのスニペットは含まれません。スニペットを aspxselecting ASP.NET MVC 用のコード スニペットが表示されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-317">ASP.NET MVC 3 does not include any snippets for Razor views..aspxselecting a code snippet for ASP.NET MVC will show snippets for</span></span>
- <span data-ttu-id="02b53-318">For Visual Web Developer Express で Visual Studio がインストールされていない、コンピューターに ASP.NET MVC 3 をインストールし、後で Visual Studio をインストールした場合、ASP.NET MVC 3 を再インストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-318">If you install ASP.NET MVC 3 for Visual Web Developer Express on a computer where Visual Studio is not installed, and then later install Visual Studio, you must reinstall ASP.NET MVC 3.</span></span> <span data-ttu-id="02b53-319">Visual Studio と Visual Web Developer Express は、ASP.NET MVC 3 インストーラーによってアップグレードされるコンポーネントを共有します。</span><span class="sxs-lookup"><span data-stu-id="02b53-319">Visual Studio and Visual Web Developer Express share components that are upgraded by the ASP.NET MVC 3 installer.</span></span> <span data-ttu-id="02b53-320">Visual Web Developer Express があるし、後で Visual Web Developer Express インストール コンピューターに Visual Studio for ASP.NET MVC 3 をインストールする場合、同じ問題が適用されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-320">The same issue applies if you install ASP.NET MVC 3 for Visual Studio on a computer that does not have Visual Web Developer Express and then later install Visual Web Developer Express.</span></span>

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a><span data-ttu-id="02b53-321">ASP.NET MVC 3 RTM での変更</span><span class="sxs-lookup"><span data-stu-id="02b53-321">Changes in ASP.NET MVC 3 RTM</span></span>

<span data-ttu-id="02b53-322">このセクションでは、変更とバグ修正が RC2 リリース後、ASP.NET MVC 3 RTM リリースで行われたについて説明します。</span><span class="sxs-lookup"><span data-stu-id="02b53-322">This section describes changes and bug fixes made in the ASP.NET MVC 3 RTM release since the RC2 release.</span></span>

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a><span data-ttu-id="02b53-323">変更: 1.8.7 に jQuery UI のバージョンを更新します。</span><span class="sxs-lookup"><span data-stu-id="02b53-323">Change: Updated the version of jQuery UI to 1.8.7</span></span>

<span data-ttu-id="02b53-324">Visual Studio 用の ASP.NET MVC プロジェクト テンプレートは、jQuery UI ライブラリの最新バージョンを対象に更新されました。</span><span class="sxs-lookup"><span data-stu-id="02b53-324">The ASP.NET MVC project templates for Visual Studio were updated to include the latest version of the jQuery UI library.</span></span> <span data-ttu-id="02b53-325">テンプレートには、jQuery UI、関連付けられている CSS およびイメージ ファイルなどに必要なリソース ファイルの最小限のセットも含まれます。</span><span class="sxs-lookup"><span data-stu-id="02b53-325">The templates also include the minimal set of resource files required by jQuery UI, such as the associated CSS and image files.</span></span>

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a><span data-ttu-id="02b53-326">変更: ModelMetadataProvider バックアップ DataAnnotationsModelMetadataProvider 既定を変更します。</span><span class="sxs-lookup"><span data-stu-id="02b53-326">Change: Changed the default ModelMetadataProvider back to DataAnnotationsModelMetadataProvider</span></span>

<span data-ttu-id="02b53-327">RC2 リリースが導入された ASP.NET MVC 3 の*CachedDataAnnotationsMetadataProvider*指定すると、既存の上にキャッシュ クラス*DataAnnotationsModelMetadataProvider*クラスとして、パフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="02b53-327">The RC2 release of ASP.NET MVC 3 introduced a *CachedDataAnnotationsMetadataProvider* class that provided caching on top of the existing *DataAnnotationsModelMetadataProvider* class as a performance improvement.</span></span> <span data-ttu-id="02b53-328">ただし、いくつかのバグが報告この実装でため、変更を元に戻すし、記載されている計画の MVC プロジェクトに移動[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)です。</span><span class="sxs-lookup"><span data-stu-id="02b53-328">However, some bugs were reported with this implementation, so the change has been reverted and moved into the MVC Futures project, which is available at [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).</span></span>

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a><span data-ttu-id="02b53-329">修正: 取り消し中に空白の結果を含む Razor 式の一部を貼り付ける</span><span class="sxs-lookup"><span data-stu-id="02b53-329">Fixed: Pasting part of a Razor expression that contains whitespace results in it being reversed</span></span>

<span data-ttu-id="02b53-330">ASP.NET MVC 3 のプレリリース版で Razor ファイルに空白を含む Razor 式の一部を貼り付けるときは、結果の式を逆にします。</span><span class="sxs-lookup"><span data-stu-id="02b53-330">In pre-release versions of ASP.NET MVC 3, when you paste a part of a Razor expression that contains whitespace into a Razor file, the resulting expression is reversed.</span></span> <span data-ttu-id="02b53-331">たとえば、次の Razor コード ブロックがあるとします。</span><span class="sxs-lookup"><span data-stu-id="02b53-331">For example, consider the following Razor code block:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

<span data-ttu-id="02b53-332">最初の方法でテキスト"最初 param"を選択してから 2 番目のメソッドに引数として貼り付けます、結果はとおりです。</span><span class="sxs-lookup"><span data-stu-id="02b53-332">If you select the text "first param" in the first method and paste it as an argument into the second method, the result is as follows:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

<span data-ttu-id="02b53-333">正常に動作は、貼り付け操作は、次のことになります。</span><span class="sxs-lookup"><span data-stu-id="02b53-333">The correct behavior is that the paste operation should result in the following:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

<span data-ttu-id="02b53-334">式が正しく貼り付け操作中に保持されるように、RTM のリリースでこの問題は修正されました。</span><span class="sxs-lookup"><span data-stu-id="02b53-334">This issue has been fixed in the RTM release so that the expression is correctly preserved during the paste operation.</span></span>

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a><span data-ttu-id="02b53-335">修正: エディターで開かれている Razor ファイルの名前を変更して無効にして構文の色表示機能 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="02b53-335">Fixed: Renaming a Razor file that is opened in the editor disables syntax colorization and IntelliSense</span></span>

<span data-ttu-id="02b53-336">構文の強調表示し、そのファイルの動作を停止するための IntelliSense は、ファイルがエディター ウィンドウで開いているときに、ソリューション エクスプ ローラーを使用して、Razor ファイルの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="02b53-336">Renaming a Razor file using Solution Explorer while the file is opened in the editor window causes syntax highlighting and IntelliSense to stop working for that file.</span></span> <span data-ttu-id="02b53-337">これは修正されました強調表示されるよう、IntelliSense は、名前の変更後に維持されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-337">This has been fixed so that highlighting and IntelliSense are maintained after a rename.</span></span>

<a id="RTM-KI"></a>
## <a name="known-issues"></a><span data-ttu-id="02b53-338">既知の問題</span><span class="sxs-lookup"><span data-stu-id="02b53-338">Known Issues</span></span>

- <span data-ttu-id="02b53-339">場合は、NuGet パッケージ マネージャー コンソールが開いているときに Visual Studio 2010 SP1 Beta を閉じると、Visual Studio がクラッシュし、再起動しようとしています。</span><span class="sxs-lookup"><span data-stu-id="02b53-339">If you close Visual Studio 2010 SP1 Beta while the NuGet Package Manager Console is open, Visual Studio crashes and attempts to restart.</span></span> <span data-ttu-id="02b53-340">これは、問題は修正する Visual Studio 2010 SP1 の RTM リリースでします。</span><span class="sxs-lookup"><span data-stu-id="02b53-340">This will be fixed in the RTM release of Visual Studio 2010 SP1.</span></span>
- <span data-ttu-id="02b53-341">ASP.NET MVC 3 インストーラーは、初期のバージョンの NuGet package manager をインストールすることはのみです。</span><span class="sxs-lookup"><span data-stu-id="02b53-341">The ASP.NET MVC 3 installer is only able to install an initial version of the NuGet package manager.</span></span> <span data-ttu-id="02b53-342">最初のバージョンをインストールした後、NuGet はインストールすることができ、Visual Studio 拡張機能マネージャーを使用して更新します。</span><span class="sxs-lookup"><span data-stu-id="02b53-342">After you have installed the initial version, NuGet can be installed and updated using Visual Studio Extension Manager.</span></span> <span data-ttu-id="02b53-343">NuGet のインストールがある場合は、NuGet の最新バージョンに更新するには、Visual Studio 拡張機能ギャラリーに移動します。</span><span class="sxs-lookup"><span data-stu-id="02b53-343">If you already have NuGet installed, go to the Visual Studio Extension Gallery to update to the latest version of NuGet.</span></span>
- <span data-ttu-id="02b53-344">原因をソリューション フォルダー内の新しい ASP.NET MVC 3 プロジェクトを作成する、 *NullReferenceException*エラーです。</span><span class="sxs-lookup"><span data-stu-id="02b53-344">Creating a new ASP.NET MVC 3 project within a solution folder causes a *NullReferenceException* error.</span></span> <span data-ttu-id="02b53-345">回避策は、ソリューションのルートに ASP.NET MVC 3 プロジェクトを作成し、ソリューション フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="02b53-345">The workaround is to create the ASP.NET MVC 3 project in the root of the solution and then move it into the solution folder.</span></span>
- <span data-ttu-id="02b53-346">インストーラーは、完了に ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-346">The installer might take much longer than previous versions of ASP.NET MVC to complete.</span></span> <span data-ttu-id="02b53-347">これは、Visual Studio 2010 のコンポーネントが更新されるためです。</span><span class="sxs-lookup"><span data-stu-id="02b53-347">This is because it updates components of Visual Studio 2010.</span></span>
- <span data-ttu-id="02b53-348">Razor 構文の IntelliSense では、ReSharper がインストールされている場合は機能しません。</span><span class="sxs-lookup"><span data-stu-id="02b53-348">IntelliSense for Razor syntax does not work when ReSharper is installed.</span></span> <span data-ttu-id="02b53-349">ReSharper をインストールして ASP.NET MVC 3 Razor IntelliSense サポートのメリットを利用する場合、エントリを参照してください。 [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri のブログでは、現時点で併用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="02b53-349">If you have ReSharper installed and want to take advantage of the Razor IntelliSense support in ASP.NET MVC 3, see the entry [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) on Hadi Hariri's blog, which discusses ways to use them together today.</span></span>
- <span data-ttu-id="02b53-350">ASP.NET MVC 3 のベータ版で作成した CCSHTML および VBHTML のビューには、ビルド アクションが正しく設定はありません、これらを表示する結果を含む型を省略すると、プロジェクトが発行されるときにします。</span><span class="sxs-lookup"><span data-stu-id="02b53-350">CCSHTML and VBHTML views created with the Beta version of ASP.NET MVC 3 do not have their build action set correctly, with the result that these view types are omitted when the project is published.</span></span> <span data-ttu-id="02b53-351">これらのファイルのビルド アクション値は、"Content"に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-351">The Build Action value for these files should be set to "Content".</span></span> <span data-ttu-id="02b53-352">ASP.NET MVC 3 RTM はの新しいファイルには、この問題の修復がプレリリース版で作成されたプロジェクトの既存のファイルの設定を修正しません。</span><span class="sxs-lookup"><span data-stu-id="02b53-352">ASP.NET MVC 3 RTM fixes this issue for new files, but doesn't correct the setting for existing files for a project created with prerelease versions.</span></span>
- ![](mvc3-release-notes/_static/image3.png)
- <span data-ttu-id="02b53-353">インストール中に、使用許諾契約書への同意 ダイアログ ボックスでは、ものよりも小さいウィンドウをライセンス条項が表示されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-353">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>
- <span data-ttu-id="02b53-354">Razor ビュー (.cshtml ファイル) を編集するときは、Visual Studio でのコント ローラーに移動するメニュー項目は使用できない、コード スニペットはありません。</span><span class="sxs-lookup"><span data-stu-id="02b53-354">When you are editing a Razor view (.cshtml file), the Go To Controller menu item in Visual Studio will not be available, and there are no code snippets.</span></span>
- <span data-ttu-id="02b53-355">For Visual Web Developer Express で Visual Studio がインストールされていない、コンピューターに ASP.NET MVC 3 をインストールし、後で Visual Studio をインストールした場合、ASP.NET MVC 3 を再インストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-355">If you install ASP.NET MVC 3 for Visual Web Developer Express on a computer where Visual Studio is not installed, and then later install Visual Studio, you must reinstall ASP.NET MVC 3.</span></span> <span data-ttu-id="02b53-356">Visual Studio と Visual Web Developer Express は、ASP.NET MVC 3 インストーラーによってアップグレードされるコンポーネントを共有します。</span><span class="sxs-lookup"><span data-stu-id="02b53-356">Visual Studio and Visual Web Developer Express share components that are upgraded by the ASP.NET MVC 3 installer.</span></span> <span data-ttu-id="02b53-357">Visual Web Developer Express があるし、後で Visual Web Developer Express インストール コンピューターに Visual Studio for ASP.NET MVC 3 をインストールする場合、同じ問題が適用されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-357">The same issue applies if you install ASP.NET MVC 3 for Visual Studio on a computer that does not have Visual Web Developer Express and then later install Visual Web Developer Express.</span></span>

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a><span data-ttu-id="02b53-358">互換性に影響する変更点</span><span class="sxs-lookup"><span data-stu-id="02b53-358">Breaking Changes</span></span>

- <span data-ttu-id="02b53-359">ASP.NET MVC の以前のバージョンで 1 回のいくつかの場合を除く要求フィルターは、アクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="02b53-359">In previous versions of ASP.NET MVC, action filters are create per request except in a few cases.</span></span> <span data-ttu-id="02b53-360">この動作が保証された動作しますが、実装の詳細だけを実行されていないと、フィルターのコントラクトがステートレスを検討するでした。</span><span class="sxs-lookup"><span data-stu-id="02b53-360">This behavior was never a guaranteed behavior but merely an implementation detail and the contract for filters was to consider them stateless.</span></span> <span data-ttu-id="02b53-361">ASP.NET MVC 3 では、フィルターは積極的にキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="02b53-361">In ASP.NET MVC 3, filters are cached more aggressively.</span></span> <span data-ttu-id="02b53-362">したがって、不適切なインスタンスの状態を格納する任意のカスタム アクション フィルターは壊れている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-362">Therefore, any custom action filters which improperly store instance state might be broken.</span></span>
- <span data-ttu-id="02b53-363">例外フィルターが同じであるため、例外フィルターの実行の順序が変更された*順序*値。</span><span class="sxs-lookup"><span data-stu-id="02b53-363">The order of execution for exception filters has changed for exception filters that have the same *Order* value.</span></span> <span data-ttu-id="02b53-364">ASP.NET MVC 2 以前のバージョンで同じであるコント ローラー上の例外がフィルター*順序*アクション メソッドで例外フィルターの前に実行されるため、アクション メソッドの値します。</span><span class="sxs-lookup"><span data-stu-id="02b53-364">In ASP.NET MVC 2 and earlier, exception filters on the controller that have the same *Order* value as those on an action method are executed before the exception filters on the action method.</span></span> <span data-ttu-id="02b53-365">例外フィルターが適用されるときに、大文字と小文字を一般にこのように、指定したせず*順序*値。</span><span class="sxs-lookup"><span data-stu-id="02b53-365">This would typically be the case when exception filters are applied without a specified *Order* value.</span></span> <span data-ttu-id="02b53-366">ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="02b53-366">In ASP.NET MVC 3, this order has been reversed so that the most specific exception handler executes first.</span></span> <span data-ttu-id="02b53-367">以前のバージョンと場合、*順序*プロパティが明示的に指定されて、フィルターは、指定した順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-367">As in earlier versions, if the *Order* property is explicitly specified, the filters are run in the specified order.</span></span>
- <span data-ttu-id="02b53-368">という名前の新しいプロパティ*FileExtensions*に追加された、 *VirtualPathProviderViewEngine*基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="02b53-368">A new property named *FileExtensions* was added to the *VirtualPathProviderViewEngine* base class.</span></span> <span data-ttu-id="02b53-369">ASP.NET では、名前ではなく) パスを使用して、ビューを検索、この新しいプロパティで指定されたリストに含まれるファイル拡張子を持つビューのみが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-369">When ASP.NET looks up a view by path (not by name), only views with a file extension contained in the list specified by this new property are considered.</span></span> <span data-ttu-id="02b53-370">これは、Web フォーム ビューのカスタムのファイル拡張子を有効にするために、カスタム ビルド プロバイダーが登録されているプロバイダーが名前ではなく、完全なパスを使用してそれらのビューを参照し、アプリケーションで重大な変更です。</span><span class="sxs-lookup"><span data-stu-id="02b53-370">This is a breaking change in applications where a custom build provider is registered in order to enable a custom file extension for Web Form views and where the provider references those views by using a full path rather than a name.</span></span> <span data-ttu-id="02b53-371">回避策がの値を変更するには、 *FileExtensions*プロパティにカスタムのファイル拡張子を含めます。</span><span class="sxs-lookup"><span data-stu-id="02b53-371">The workaround is to modify the value of the *FileExtensions* property to include the custom file extension.</span></span>
- <span data-ttu-id="02b53-372">直接実装するカスタム コント ローラー ファクトリの実装、 *IControllerFactory*インターフェイスは、新しい実装を提供する必要があります*GetControllerSessionBehavior*がメソッドこのリリースでは、インターフェイスに追加されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-372">Custom controller factory implementations that directly implement the *IControllerFactory* interface must provide an implementation of the new *GetControllerSessionBehavior* method that was added to the interface in this release.</span></span> <span data-ttu-id="02b53-373">一般に、お勧めするこのインターフェイスを直接実装およびしない代わりに、クラスから派生させる*DefaultControllerFactory*です。</span><span class="sxs-lookup"><span data-stu-id="02b53-373">In general, it is recommended that you do not implement this interface directly and instead derive your class from *DefaultControllerFactory*.</span></span>

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a><span data-ttu-id="02b53-374">ASP.NET MVC 3 RC2 での変更</span><span class="sxs-lookup"><span data-stu-id="02b53-374">Changes in ASP.NET MVC 3 RC2</span></span>

<span data-ttu-id="02b53-375">このセクションでは、RC のリリース以降、ASP.NET MVC 3 RC2 リリースで行われた変更 (新機能とバグ修正) について説明します。</span><span class="sxs-lookup"><span data-stu-id="02b53-375">This section describes changes (new features and bug fixes) made in the ASP.NET MVC 3 RC2 release since the RC release.</span></span>

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a><span data-ttu-id="02b53-376">1.4.4 jQuery、jQuery 検証 1.7 と jQuery UI 1.8.6 を含むようにプロジェクト テンプレートの変更</span><span class="sxs-lookup"><span data-stu-id="02b53-376">Project Templates Changed to Include jQuery 1.4.4, jQuery Validation 1.7, and jQuery UI 1.8.6</span></span>

<span data-ttu-id="02b53-377">ASP.NET MVC 3 のプロジェクト テンプレートが加わりました最新バージョンの jQuery、jQuery 検証、および jQuery UI。</span><span class="sxs-lookup"><span data-stu-id="02b53-377">The project templates for ASP.NET MVC 3 now include the latest versions of jQuery, jQuery Validation, and jQuery UI.</span></span> <span data-ttu-id="02b53-378">jQuery UI は、プロジェクト テンプレートに新たに追加し、便利なユーザー インターフェイスのウィジェットを提供します。</span><span class="sxs-lookup"><span data-stu-id="02b53-378">jQuery UI is a new addition to the project templates and provides useful user interface widgets.</span></span> <span data-ttu-id="02b53-379">JQuery UI の詳細については、自分のホーム ページを参照してください: [ http://jqueryui.com/](http://jqueryui.com/)です。</span><span class="sxs-lookup"><span data-stu-id="02b53-379">For more information about jQuery UI, visit their homepage: [http://jqueryui.com/](http://jqueryui.com/).</span></span>

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a><span data-ttu-id="02b53-380">追加された"AdditionalMetadataAttribute"クラス</span><span class="sxs-lookup"><span data-stu-id="02b53-380">Added "AdditionalMetadataAttribute" Class</span></span>

<span data-ttu-id="02b53-381">使用することができます、 *AdditionalMetadataAttribute*を設定するクラス、 *ModelMetadata.AdditionalValues*モデル プロパティのディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="02b53-381">You can use the *AdditionalMetadataAttribute* class to populate the *ModelMetadata.AdditionalValues* dictionary for a model property.</span></span>

<span data-ttu-id="02b53-382">たとえば、ビュー モデルは、管理者にのみ表示されるプロパティを持ちます。</span><span class="sxs-lookup"><span data-stu-id="02b53-382">For example, suppose a view model has properties that should be displayed only to an administrator.</span></span> <span data-ttu-id="02b53-383">そのモデル AdminOnly をキーと true として次の例のように、値として使用して、新しい属性で注釈を付けることができます。</span><span class="sxs-lookup"><span data-stu-id="02b53-383">That model can be annotated with the new attribute using AdminOnly as the key and true as the value, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

<span data-ttu-id="02b53-384">製品ビュー モデルが表示される場合に、このメタデータは、表示またはエディターのテンプレートを使用可能になります。</span><span class="sxs-lookup"><span data-stu-id="02b53-384">This metadata is made available to any display or editor template when a product view model is rendered.</span></span> <span data-ttu-id="02b53-385">メタデータ情報を解釈するアプリケーション開発者としてユーザーの責任です。</span><span class="sxs-lookup"><span data-stu-id="02b53-385">It is up to you as application developer to interpret the metadata information.</span></span>

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a><span data-ttu-id="02b53-386">強化されたビューのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="02b53-386">Improved View Scaffolding</span></span>

<span data-ttu-id="02b53-387">今すぐスキャフォールディング ビューで使用される T4 テンプレートなどのテンプレート ヘルパー メソッドの呼び出しの生成*EditorFor*などのヘルパーではなく*TextBoxFor*です。</span><span class="sxs-lookup"><span data-stu-id="02b53-387">The T4 templates used for scaffolding views now generate calls to template helper methods such as *EditorFor* instead of helpers such as *TextBoxFor*.</span></span> <span data-ttu-id="02b53-388">この変更は、ビューの追加 ダイアログ ボックスのビューを生成するときに、データの注釈属性の形式でモデルのメタデータのサポートを向上します。</span><span class="sxs-lookup"><span data-stu-id="02b53-388">This change improves support for metadata on the model in the form of data annotation attributes when the Add View dialog box generates a view.</span></span>

<span data-ttu-id="02b53-389">ビューのスキャフォールディングには、検出機能の向上および使用状況の規約に基づいて、モデルの主キーの情報も含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-389">The Add View scaffolding also includes improved detection and usage of primary key information on the model, based on convention.</span></span> <span data-ttu-id="02b53-390">たとえば、ビューの追加 ダイアログ ボックスはこの情報を使用して、編集可能なフォームのフィールドとして主キーの値がないスキャフォールディングされたことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="02b53-390">For example, the Add View dialog box uses this information to ensure that the primary key value is not scaffolded as an editable form field.</span></span>

<span data-ttu-id="02b53-391">テンプレートの作成と編集を既定値は、クライアントの検証に必要な jQuery スクリプトへの参照を含めます。</span><span class="sxs-lookup"><span data-stu-id="02b53-391">The default Edit and Create templates include references to the jQuery scripts needed for client validation.</span></span>

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a><span data-ttu-id="02b53-392">追加された Html.Raw メソッド</span><span class="sxs-lookup"><span data-stu-id="02b53-392">Added Html.Raw Method</span></span>

<span data-ttu-id="02b53-393">既定では、Razor はすべての値を HTML エンコード エンジンを表示します。</span><span class="sxs-lookup"><span data-stu-id="02b53-393">By default, the Razor view engine HTML-encodes all values.</span></span> <span data-ttu-id="02b53-394">次のコード スニペットがのあいさつの変数の内部 HTML をエンコードするとしてそのページに表示されるように、`<strong>Hello World!</strong>`です。</span><span class="sxs-lookup"><span data-stu-id="02b53-394">For example, the following code snippet encodes the HTML inside the greeting variable so that it is displayed in the page as `<strong>Hello World!</strong>`.</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

<span data-ttu-id="02b53-395">新しい*Html.Raw*メソッドは、念のために、コンテンツがわかっている場合は、エンコードされていない HTML を表示する簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="02b53-395">The new *Html.Raw* method provides a simple way of displaying unencoded HTML when the content is known to be safe.</span></span> <span data-ttu-id="02b53-396">次の例には、同じ文字列が表示されますが、文字列がマークアップとしてレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="02b53-396">The following example displays the same string, but the string is rendered as markup:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a><span data-ttu-id="02b53-397">名前が変更された"Controller.ViewModel"プロパティと"ViewBag"を"View"プロパティ</span><span class="sxs-lookup"><span data-stu-id="02b53-397">Renamed "Controller.ViewModel" Property and the "View" Property To "ViewBag"</span></span>

<span data-ttu-id="02b53-398">以前は、 *ViewModel*プロパティ*コント ローラー*に当てはめて考える、*ビュー*ビューのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="02b53-398">Previously, the *ViewModel* property of *Controller* corresponded to the *View* property of a view.</span></span> <span data-ttu-id="02b53-399">これらのプロパティの両方の手段のアクセスの値、 *ViewDataDictionary*オブジェクトの動的なプロパティ アクセサーの構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="02b53-399">Both of these properties provide a way to access values of the *ViewDataDictionary* object using dynamic property-accessor syntax.</span></span> <span data-ttu-id="02b53-400">両方のプロパティは、混乱を避けるためより一貫したものと同じである名前変更されています。</span><span class="sxs-lookup"><span data-stu-id="02b53-400">Both properties have been renamed to be the same in order to avoid confusion and to be more consistent.</span></span>

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a><span data-ttu-id="02b53-401">"SessionStateAttribute"に名前が変更された"ControllerSessionStateAttribute"クラス</span><span class="sxs-lookup"><span data-stu-id="02b53-401">Renamed "ControllerSessionStateAttribute" Class to "SessionStateAttribute"</span></span>

<span data-ttu-id="02b53-402">*ControllerSessionStateAttribute*クラスは、ASP.NET MVC 3 RC のリリースで導入されました。</span><span class="sxs-lookup"><span data-stu-id="02b53-402">The *ControllerSessionStateAttribute* class was introduced in the RC release of ASP.NET MVC 3.</span></span> <span data-ttu-id="02b53-403">プロパティより簡潔なである名前が変更されました。</span><span class="sxs-lookup"><span data-stu-id="02b53-403">The property was renamed to be more succinct.</span></span>

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a><span data-ttu-id="02b53-404">「ため」に名前を変更した RemoteAttribute"Fields"プロパティ</span><span class="sxs-lookup"><span data-stu-id="02b53-404">Renamed RemoteAttribute "Fields" Property to "AdditionalFields"</span></span>

<span data-ttu-id="02b53-405">*RemoteAttribute*クラスの*フィールド*ユーザー間でのいくつかの混乱の原因となったプロパティ。</span><span class="sxs-lookup"><span data-stu-id="02b53-405">The *RemoteAttribute* class's *Fields* property caused some confusion among users.</span></span> <span data-ttu-id="02b53-406">このプロパティの名前を変更する*ため*その目的を明確化します。</span><span class="sxs-lookup"><span data-stu-id="02b53-406">Renaming this property to *AdditionalFields* clarifies its intent.</span></span>

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a><span data-ttu-id="02b53-407">"AllowHtmlAttribute"を"SkipRequestValidationAttribute"の名前を変更</span><span class="sxs-lookup"><span data-stu-id="02b53-407">Renamed "SkipRequestValidationAttribute" to "AllowHtmlAttribute"</span></span>

<span data-ttu-id="02b53-408">*SkipRequestValidationAttribute*に属性が変更されました*AllowHtmlAttribute*用途をわかりやすく示したです。</span><span class="sxs-lookup"><span data-stu-id="02b53-408">The *SkipRequestValidationAttribute* attribute was renamed to *AllowHtmlAttribute* to better represent its intended usage.</span></span>

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a><span data-ttu-id="02b53-409">最初の有用なエラー メッセージを表示して変更された"Html.ValidationMessage"メソッド</span><span class="sxs-lookup"><span data-stu-id="02b53-409">Changed "Html.ValidationMessage" Method to Display the First Useful Error Message</span></span>

<span data-ttu-id="02b53-410">*Html.ValidationMessage*最初のエラーを表示するだけではなく最初の有用なエラー メッセージを表示するメソッドが修正されました。</span><span class="sxs-lookup"><span data-stu-id="02b53-410">The *Html.ValidationMessage* method was fixed to show the first useful error message instead of simply displaying the first error.</span></span>

<span data-ttu-id="02b53-411">モデル バインド中に、 *ModelState*ディクショナリは、モデル自体を含む、プロパティに関するエラー メッセージに複数のソースからデータを読み込むことができます (これを実装する場合*IValidatableObject*)、プロパティにアクセスするときにスローされた例外および検証属性のプロパティに適用されるからです。</span><span class="sxs-lookup"><span data-stu-id="02b53-411">During model binding, the *ModelState* dictionary can be populated from multiple sources with error messages about the property, including from the model itself (if it implements *IValidatableObject*), from validation attributes applied to the property, and from exceptions thrown while the property is being accessed.</span></span>

<span data-ttu-id="02b53-412">ときに、 *Html.ValidationMessage*メソッドには、検証メッセージが表示されます、これらは一般的にないため、エンドユーザーに対して、例外が含まれているモデル状態エントリはスキップされます。</span><span class="sxs-lookup"><span data-stu-id="02b53-412">When the *Html.ValidationMessage* method displays a validation message, it skips model-state entries that include an exception, because these are generally not intended for the end user.</span></span> <span data-ttu-id="02b53-413">代わりに、メソッドは、例外に関連付けられていないと、そのメッセージが表示される最初の検証メッセージを探します。</span><span class="sxs-lookup"><span data-stu-id="02b53-413">Instead, the method looks for the first validation message that is not associated with an exception and displays that message.</span></span> <span data-ttu-id="02b53-414">このようなメッセージが見つからない場合の既定値は最初の例外に関連付けられている一般的なエラー メッセージです。</span><span class="sxs-lookup"><span data-stu-id="02b53-414">If no such message is found, it defaults to a generic error message that is associated with the first exception.</span></span>

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a><span data-ttu-id="02b53-415">固定@model宣言、ドキュメントに空白を追加するには</span><span class="sxs-lookup"><span data-stu-id="02b53-415">Fixed @model Declaration to not Add Whitespace to the Document</span></span>

<span data-ttu-id="02b53-416">以前のリリースで、 <em>@model</em>ビューの上部にある宣言は、レンダリングされた HTML 出力に空白行を追加します。</span><span class="sxs-lookup"><span data-stu-id="02b53-416">In earlier releases, the <em>@model</em> declaration at the top of a view added a blank line to the rendered HTML output.</span></span> <span data-ttu-id="02b53-417">これは、宣言は空白を挿入していないように修正されました。</span><span class="sxs-lookup"><span data-stu-id="02b53-417">This has been fixed so that the declaration does not introduce whitespace.</span></span>

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a><span data-ttu-id="02b53-418">エンジンに固有のファイル名をサポートするために、ビュー エンジンに追加された"FileExtensions"プロパティ</span><span class="sxs-lookup"><span data-stu-id="02b53-418">Added "FileExtensions" Property to View Engines to Support Engine-Specific File Names</span></span>

<span data-ttu-id="02b53-419">ビュー エンジンは、次の例のように、明示的なビューのパスを使用してビューを返すことができます。</span><span class="sxs-lookup"><span data-stu-id="02b53-419">A view engine can return a view using an explicit view path as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

<span data-ttu-id="02b53-420">最初のビュー エンジンは、常に、ビューをレンダリングしようとします。</span><span class="sxs-lookup"><span data-stu-id="02b53-420">The first view engine always attempts to render the view.</span></span> <span data-ttu-id="02b53-421">既定では、Web フォーム ビュー エンジンは、最初のビュー エンジンです。Web フォーム エンジンは、Razor ビューをレンダリングできません、ためエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="02b53-421">By default, the Web Forms view engine is the first view engine; because the Web Forms engine cannot render a Razor view, an error occurs.</span></span> <span data-ttu-id="02b53-422">ビュー エンジンのようになりましたが、 *FileExtensions*を使用するファイル拡張子を指定するプロパティをサポートします。</span><span class="sxs-lookup"><span data-stu-id="02b53-422">View engines now have a *FileExtensions* property that is used to specify which file extensions they support.</span></span> <span data-ttu-id="02b53-423">ASP.NET では、ビュー エンジンが、ファイルを表示できるかどうかが判断した場合、このプロパティがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="02b53-423">This property is checked when ASP.NET determines whether a view engine can render a file.</span></span> <span data-ttu-id="02b53-424">これは、互換性に影響する変更点と、詳細についてに含まれる、[の重大な変更](#_Toc2_BC)このドキュメントの「します。</span><span class="sxs-lookup"><span data-stu-id="02b53-424">This is a breaking change and more details are included in the [Breaking Changes](#_Toc2_BC) section of this document.</span></span>

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a><span data-ttu-id="02b53-425">"For"属性の適切な値を出力する固定"LabelFor"ヘルパー</span><span class="sxs-lookup"><span data-stu-id="02b53-425">Fixed "LabelFor" Helper to Emit the Correct Value for the "For" Attribute</span></span>

<span data-ttu-id="02b53-426">バグが修正されたは、where、 *LabelFor*レンダリング メソッド、*の*と一致する属性、*入力*要素の*名前*属性の代わりにその ID の</span><span class="sxs-lookup"><span data-stu-id="02b53-426">A bug was fixed where the *LabelFor* method rendered a *for* attribute that matches the *input* element's *name* attribute instead of its ID.</span></span> <span data-ttu-id="02b53-427">W3C に従って、*の*属性に一致する必要があります、*入力*要素の id。</span><span class="sxs-lookup"><span data-stu-id="02b53-427">According to the W3C, the *for* attribute should match the *input* element's ID.</span></span>

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a><span data-ttu-id="02b53-428">モデル バインド中に明示的な値を優先する固定"RenderAction"メソッド</span><span class="sxs-lookup"><span data-stu-id="02b53-428">Fixed "RenderAction" Method to Give Explicit Values Precedence During Model Binding</span></span>

<span data-ttu-id="02b53-429">以前のバージョンでは、明示的な値に渡された、 *RenderAction*メソッドは、子アクションの内部モデル バインド中に現在のフォーム値を優先するため無視されたされています。</span><span class="sxs-lookup"><span data-stu-id="02b53-429">In earlier versions, explicit values that were passed to the *RenderAction* method were being ignored in favor of the current form values during model binding inside a child action.</span></span> <span data-ttu-id="02b53-430">解決策により、明示的な値がモデル バインド中に優先順位を取得します。</span><span class="sxs-lookup"><span data-stu-id="02b53-430">The fix ensures that explicit values take precedence during model binding.</span></span>

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a><span data-ttu-id="02b53-431">互換性に影響する変更点</span><span class="sxs-lookup"><span data-stu-id="02b53-431">Breaking Changes</span></span>

- <span data-ttu-id="02b53-432">ASP.NET MVC の以前のバージョンでは、アクション フィルターは、いくつかの場合を除く要求ごと作成されました。</span><span class="sxs-lookup"><span data-stu-id="02b53-432">In previous versions of ASP.NET MVC, action filters were created per request except in a few cases.</span></span> <span data-ttu-id="02b53-433">この動作が保証された動作しますが、実装の詳細だけを実行されていないと、フィルターのコントラクトがステートレスを検討するでした。</span><span class="sxs-lookup"><span data-stu-id="02b53-433">This behavior was never a guaranteed behavior but merely an implementation detail and the contract for filters was to consider them stateless.</span></span> <span data-ttu-id="02b53-434">ASP.NET MVC 3 では、フィルターは積極的にキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="02b53-434">In ASP.NET MVC 3, filters are cached more aggressively.</span></span> <span data-ttu-id="02b53-435">したがって、不適切なインスタンスの状態を格納する任意のカスタム アクション フィルターは壊れている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-435">Therefore, any custom action filters which improperly store instance state might be broken.</span></span>
- <span data-ttu-id="02b53-436">例外フィルターが同じであるため、例外フィルターの実行の順序が変更された*順序*値。</span><span class="sxs-lookup"><span data-stu-id="02b53-436">The order of execution for exception filters has changed for exception filters that have the same *Order* value.</span></span> <span data-ttu-id="02b53-437">ASP.NET MVC 2 以前のバージョンで同じがあったコント ローラー上の例外がフィルター*順序*例外フィルター、アクション メソッドにする前に実行されたアクション メソッドのように値します。</span><span class="sxs-lookup"><span data-stu-id="02b53-437">In ASP.NET MVC 2 and earlier, exception filters on the controller that had the same *Order* value as those on an action method were executed before the exception filters on the action method.</span></span> <span data-ttu-id="02b53-438">例外フィルターが適用されたときに、大文字と小文字を一般にこのように、指定したせず*順序*値。</span><span class="sxs-lookup"><span data-stu-id="02b53-438">This would typically be the case when exception filters were applied without a specified *Order* value.</span></span> <span data-ttu-id="02b53-439">ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="02b53-439">In ASP.NET MVC 3, this order has been reversed so that the most specific exception handler executes first.</span></span> <span data-ttu-id="02b53-440">以前のバージョンと場合、*順序*プロパティが明示的に指定されて、フィルターは、指定した順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-440">As in earlier versions, if the *Order* property is explicitly specified, the filters are run in the specified order.</span></span>
- <span data-ttu-id="02b53-441">という名前の新しいプロパティ*FileExtensions*に追加された、 *VirtualPathProviderViewEngine*基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="02b53-441">A new property named *FileExtensions* was added to the *VirtualPathProviderViewEngine* base class.</span></span> <span data-ttu-id="02b53-442">ASP.NET では、名前ではなく) パスを使用して、ビューを検索、この新しいプロパティで指定されたリストに含まれるファイル拡張子を持つビューのみが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-442">When ASP.NET looks up a view by path (not by name), only views with a file extension contained in the list specified by this new property are considered.</span></span> <span data-ttu-id="02b53-443">これは、Web フォーム ビューのカスタムのファイル拡張子を有効にするために、カスタム ビルド プロバイダーが登録されているプロバイダーが名前ではなく、完全なパスを使用してそれらのビューを参照し、アプリケーションで重大な変更です。</span><span class="sxs-lookup"><span data-stu-id="02b53-443">This is a breaking change in applications where a custom build provider is registered in order to enable a custom file extension for Web Form views and where the provider references those views by using a full path rather than a name.</span></span> <span data-ttu-id="02b53-444">回避策がの値を変更するには、 *FileExtensions*プロパティにカスタムのファイル拡張子を含めます。</span><span class="sxs-lookup"><span data-stu-id="02b53-444">The workaround is to modify the value of the *FileExtensions* property to include the custom file extension.</span></span>
- <span data-ttu-id="02b53-445">直接実装するカスタム コント ローラー ファクトリの実装、 <em>IControllerFactory</em>インターフェイスは、新しい実装を提供する必要があります<em>GetControllerSessionBehavior</em> <em>このリリースでは、インターフェイスに追加されたメソッドに</em>です。</span><span class="sxs-lookup"><span data-stu-id="02b53-445">Custom controller factory implementations that directly implement the <em>IControllerFactory</em> interface must provide an implementation of the new <em>GetControllerSessionBehavior</em><em>method that was added to the interface in this release</em>.</span></span> <span data-ttu-id="02b53-446">一般に、お勧めするこのインターフェイスを直接実装およびしない代わりに、クラスから派生させる<em>DefaultControllerFactory</em>です。</span><span class="sxs-lookup"><span data-stu-id="02b53-446">In general, it is recommended that you do not implement this interface directly and instead derive your class from <em>DefaultControllerFactory</em>.</span></span>

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a><span data-ttu-id="02b53-447">既知の問題</span><span class="sxs-lookup"><span data-stu-id="02b53-447">Known Issues</span></span>

- <span data-ttu-id="02b53-448">ASP.NET MVC 3 インストーラーは、初期のバージョンの NuGet package manager をインストールすることはのみです。</span><span class="sxs-lookup"><span data-stu-id="02b53-448">The ASP.NET MVC 3 installer is only able to install an initial version of the NuGet package manager.</span></span> <span data-ttu-id="02b53-449">最初のバージョンをインストールした後、NuGet はインストールすることができ、Visual Studio 拡張機能マネージャーを使用して更新します。</span><span class="sxs-lookup"><span data-stu-id="02b53-449">After you have installed the initial version, NuGet can be installed and updated using Visual Studio Extension Manager.</span></span> <span data-ttu-id="02b53-450">NuGet のインストールがある場合は、NuGet の最新バージョンに更新するには、Visual Studio 拡張機能ギャラリーに移動します。</span><span class="sxs-lookup"><span data-stu-id="02b53-450">If you already have NuGet installed, go to the Visual Studio Extension Gallery to update to the latest version of NuGet.</span></span>
- <span data-ttu-id="02b53-451">原因をソリューション フォルダー内の新しい ASP.NET MVC 3 プロジェクトを作成する、 *NullReferenceException*エラーです。</span><span class="sxs-lookup"><span data-stu-id="02b53-451">Creating a new ASP.NET MVC 3 project within a solution folder causes a *NullReferenceException* error.</span></span> <span data-ttu-id="02b53-452">回避策は、ソリューションのルートに ASP.NET MVC 3 プロジェクトを作成し、ソリューション フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="02b53-452">The workaround is to create the ASP.NET MVC 3 project in the root of the solution and then move it into the solution folder.</span></span>
- <span data-ttu-id="02b53-453">インストーラーは、完了に ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-453">The installer might take much longer than previous versions of ASP.NET MVC to complete.</span></span> <span data-ttu-id="02b53-454">これは、Visual Studio 2010 のコンポーネントが更新されるためです。</span><span class="sxs-lookup"><span data-stu-id="02b53-454">This is because it updates components of Visual Studio 2010.</span></span>
- <span data-ttu-id="02b53-455">Razor 構文の IntelliSense では、ReSharper がインストールされている場合は機能しません。</span><span class="sxs-lookup"><span data-stu-id="02b53-455">IntelliSense for Razor syntax does not work when ReSharper is installed.</span></span> <span data-ttu-id="02b53-456">ReSharper をインストールして ASP.NET MVC 3 RC2 Razor IntelliSense サポートのメリットを利用する場合、エントリを参照してください。 [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri のブログでは、現時点で併用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="02b53-456">If you have ReSharper installed and want to take advantage of the Razor IntelliSense support in ASP.NET MVC 3 RC2, see the entry [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) on Hadi Hariri's blog, which discusses ways to use them together today.</span></span>
- <span data-ttu-id="02b53-457">ASP.NET MVC 3 のベータ版で作成した CSHTML および VBHTML のビューには、ビルド アクションが正しく設定はありません、これらを表示する結果を含む型を省略すると、プロジェクトが発行されるときにします。</span><span class="sxs-lookup"><span data-stu-id="02b53-457">CSHTML and VBHTML views created with the Beta version of ASP.NET MVC 3 do not have their build action set correctly, with the result that these view types are omitted when the project is published.</span></span> <span data-ttu-id="02b53-458">*ビルド アクション*コンテンツにこれらのファイルを設定する必要がありますの値"です。</span><span class="sxs-lookup"><span data-stu-id="02b53-458">The *Build Action* value for these files should be set to Content".</span></span> <span data-ttu-id="02b53-459">ASP.NET MVC 3 RC2 では、新しいファイルの場合は、この問題を修正がベータ版で作成されたプロジェクトの既存のファイルの設定を修正しません。![](mvc3-release-notes/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="02b53-459">ASP.NET MVC 3 RC2 fixes this issue for new files, but doesn't correct the setting for existing files for a project created with the Beta version.![](mvc3-release-notes/_static/image4.png)</span></span>
- <span data-ttu-id="02b53-460">インストール中に、使用許諾契約書への同意 ダイアログ ボックスでは、ものよりも小さいウィンドウをライセンス条項が表示されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-460">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>
- <span data-ttu-id="02b53-461">Razor ビュー (.cshtml ファイル) を編集するときは、Visual Studio でのコント ローラーに移動するメニュー項目は使用できない、コード スニペットはありません。</span><span class="sxs-lookup"><span data-stu-id="02b53-461">When you are editing a Razor view (.cshtml file), the Go To Controller menu item in Visual Studio will not be available, and there are no code snippets.</span></span>
- <span data-ttu-id="02b53-462">For Visual Web Developer Express で Visual Studio がインストールされていない、コンピューターに ASP.NET MVC 3 をインストールし、後で Visual Studio をインストールした場合、ASP.NET MVC 3 を再インストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-462">If you install ASP.NET MVC 3 for Visual Web Developer Express on a computer where Visual Studio is not installed, and then later install Visual Studio, you must reinstall ASP.NET MVC 3.</span></span> <span data-ttu-id="02b53-463">Visual Studio と Visual Web Developer Express は、ASP.NET MVC 3 インストーラーによってアップグレードされるコンポーネントを共有します。</span><span class="sxs-lookup"><span data-stu-id="02b53-463">Visual Studio and Visual Web Developer Express share components that are upgraded by the ASP.NET MVC 3 installer.</span></span> <span data-ttu-id="02b53-464">Visual Web Developer Express があるし、後で Visual Web Developer Express インストール コンピューターに Visual Studio for ASP.NET MVC 3 をインストールする場合、同じ問題が適用されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-464">The same issue applies if you install ASP.NET MVC 3 for Visual Studio on a computer that does not have Visual Web Developer Express and then later install Visual Web Developer Express.</span></span>
- <span data-ttu-id="02b53-465">ASP.NET MVC 3 RC 2 のインストールを更新しません NuGet 既にインストールされていること。</span><span class="sxs-lookup"><span data-stu-id="02b53-465">Installing ASP.NET MVC 3 RC 2 does not update NuGet if you already have it installed.</span></span> <span data-ttu-id="02b53-466">NuGet をアップグレードするには、Visual Studio 拡張機能マネージャーに移動する必要がありますとして表示、利用可能な更新します。</span><span class="sxs-lookup"><span data-stu-id="02b53-466">To upgrade NuGet, go to the Visual Studio Extension manager and it should show up as an available update.</span></span> <span data-ttu-id="02b53-467">そこから最新のリリースには、NuGet をアップグレードできます。</span><span class="sxs-lookup"><span data-stu-id="02b53-467">You can upgrade NuGet to the latest release from there.</span></span>

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a><span data-ttu-id="02b53-468">ASP.NET MVC 3 のリリース候補</span><span class="sxs-lookup"><span data-stu-id="02b53-468">ASP.NET MVC 3 Release Candidate</span></span>

<span data-ttu-id="02b53-469">ASP.NET MVC リリース候補は、2010 年 11 月 9 日にリリースされました。</span><span class="sxs-lookup"><span data-stu-id="02b53-469">ASP.NET MVC Release Candidate was released on November 9, 2010.</span></span>

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a><span data-ttu-id="02b53-470">ASP.NET MVC 3 RC の新機能</span><span class="sxs-lookup"><span data-stu-id="02b53-470">New Features in ASP.NET MVC 3 RC</span></span>

<span data-ttu-id="02b53-471">このセクションの内容が導入された機能について説明します、ベータ リリース以降、ASP.NET MVC 3 RC のリリースでします。</span><span class="sxs-lookup"><span data-stu-id="02b53-471">This section describes features that have been introduced in the ASP.NET MVC 3 RC release since the Beta release.</span></span>

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a><span data-ttu-id="02b53-472">NuGet パッケージ マネージャー</span><span class="sxs-lookup"><span data-stu-id="02b53-472">NuGet Package Manager</span></span>

<span data-ttu-id="02b53-473">ASP.NET MVC 3 では、NuGet Package Manager (旧称 NuPack)、これはライブラリおよびツールを Visual Studio プロジェクトに追加するための統合パッケージ管理ツールが含まれます。</span><span class="sxs-lookup"><span data-stu-id="02b53-473">ASP.NET MVC 3 includes the NuGet Package Manager (formerly known as NuPack), which is an integrated package management tool for adding libraries and tools to Visual Studio projects.</span></span> <span data-ttu-id="02b53-474">このツールは、開発者は、ソース ツリーにライブラリを取得する現在ための手順を自動化します。</span><span class="sxs-lookup"><span data-stu-id="02b53-474">This tool automates the steps that developers take today to get a library into their source tree.</span></span>

<span data-ttu-id="02b53-475">コマンド ライン ツール、Visual Studio のコンテキスト メニューから Visual Studio 2010 内の統合されたコンソール ウィンドウと PowerShell コマンドレットのセットは、NuGet を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="02b53-475">You can work with NuGet as a command-line tool, as an integrated console window inside Visual Studio 2010, from the Visual Studio context menu, and as a set of PowerShell cmdlets.</span></span>

<span data-ttu-id="02b53-476">NuGet の詳細については、読み取り、 [Nuget のドキュメント](https://docs.microsoft.com/nuget/)です。</span><span class="sxs-lookup"><span data-stu-id="02b53-476">For more information about NuGet, read the [Nuget Documentation](https://docs.microsoft.com/nuget/).</span></span>

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a><span data-ttu-id="02b53-477">向上""新しいプロジェクト ダイアログ ボックス</span><span class="sxs-lookup"><span data-stu-id="02b53-477">Improved "New Project" Dialog Box</span></span>

<span data-ttu-id="02b53-478">新しいプロジェクトを作成するときに新しいプロジェクト ダイアログ ボックスでできるようになりました、ビュー エンジンと ASP.NET MVC プロジェクトの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="02b53-478">When you create a new project, the New Project dialog box now lets you specify the view engine as well as an ASP.NET MVC project type.</span></span>

![](mvc3-release-notes/_static/image5.png)

<span data-ttu-id="02b53-479">このリリースでは、テンプレートとエンジンがダイアログ ボックスに表示するビューの一覧を変更するためのサポートが含まれます。</span><span class="sxs-lookup"><span data-stu-id="02b53-479">Support for modifying the list of templates and view engines listed in the dialog box is included in this release.</span></span>

<span data-ttu-id="02b53-480">既定のテンプレートは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="02b53-480">The default templates are the following:</span></span>

<span data-ttu-id="02b53-481">空です。</span><span class="sxs-lookup"><span data-stu-id="02b53-481">Empty.</span></span> <span data-ttu-id="02b53-482">最小の既定のディレクトリ構造 ASP.NET MVC プロジェクトの既定の ASP.NET MVC は、次のスタイル、および既定の JavaScript ファイルを含むスクリプト ディレクトリを含む Site.css ファイルを含む、ASP.NET MVC プロジェクトのファイル セットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-482">Contains a minimal set of files for an ASP.NET MVC project, including the default directory structure for ASP.NET MVC projects, a Site.css file that contains the default ASP.NET MVC styles, and a Scripts directory that contains the default JavaScript files.</span></span>

<span data-ttu-id="02b53-483">インターネット アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="02b53-483">Internet Application.</span></span> <span data-ttu-id="02b53-484">ASP.NET MVC でのメンバーシップ プロバイダーを使用する方法を示すサンプルの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-484">Contains sample functionality that demonstrates how to use the membership provider with ASP.NET MVC.</span></span>

<span data-ttu-id="02b53-485">ダイアログ ボックスに表示されているプロジェクト テンプレートの一覧は、Windows レジストリで指定されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-485">The list of project templates that is displayed in the dialog box is specified in the Windows registry.</span></span>

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a><span data-ttu-id="02b53-486">Sessionless コント ローラー</span><span class="sxs-lookup"><span data-stu-id="02b53-486">Sessionless Controllers</span></span>

<span data-ttu-id="02b53-487">新しい*ControllerSessionStateAttribute*うえでセッション状態の動作をより細かく制御コント ローラーを指定して、 [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx)列挙値。</span><span class="sxs-lookup"><span data-stu-id="02b53-487">The new *ControllerSessionStateAttribute* gives you more control over session-state behavior for controllers by specifying a [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) enumeration value.</span></span>

<span data-ttu-id="02b53-488">次の例では、コント ローラーへのすべての要求のセッション状態をオフにする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="02b53-488">The following example shows how to turn off session state for all requests to a controller.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

<span data-ttu-id="02b53-489">次の例では、コント ローラーにすべての要求に対する読み取り専用のセッション状態を設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="02b53-489">The following example shows how to set read-only session state for all requests to a controller.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a><span data-ttu-id="02b53-490">新しい検証属性</span><span class="sxs-lookup"><span data-stu-id="02b53-490">New Validation Attributes</span></span>

#### <a name="compareattribute"></a><span data-ttu-id="02b53-491">CompareAttribute</span><span class="sxs-lookup"><span data-stu-id="02b53-491">CompareAttribute</span></span>

<span data-ttu-id="02b53-492">新しい*CompareAttribute*検証属性を使用して、モデルの 2 つの異なるプロパティの値を比較できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-492">The new *CompareAttribute* validation attribute lets you compare the values of two different properties of a model.</span></span> <span data-ttu-id="02b53-493">次の例で、 *ComparePassword*プロパティに一致する必要があります、*パスワード*有効にするのにはフィールドです。</span><span class="sxs-lookup"><span data-stu-id="02b53-493">In the following example, the *ComparePassword* property must match the *Password* field in order to be valid.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a><span data-ttu-id="02b53-494">RemoteAttribute</span><span class="sxs-lookup"><span data-stu-id="02b53-494">RemoteAttribute</span></span>

<span data-ttu-id="02b53-495">新しい*RemoteAttribute*メソッドを呼び出して、実際の検証ロジックを実行するサーバー上のクライアント側検証を有効、jQuery 検証プラグインのリモート検証の検証属性を活用します。</span><span class="sxs-lookup"><span data-stu-id="02b53-495">The new *RemoteAttribute* validation attribute takes advantage of the jQuery Validation plug-in's remote validator, which enables client-side validation to call a method on the server that performs the actual validation logic.</span></span>

<span data-ttu-id="02b53-496">次の例で、 *UserName*プロパティには、 *RemoteAttribute*適用します。</span><span class="sxs-lookup"><span data-stu-id="02b53-496">In the following example, the *UserName* property has the *RemoteAttribute* applied.</span></span> <span data-ttu-id="02b53-497">クライアントの検証がという名前のアクションを呼び出して、編集ビューでは、このプロパティを編集するには、 *UserNameAvailable*上、 *UsersController*このフィールドを検証するためにクラスです。</span><span class="sxs-lookup"><span data-stu-id="02b53-497">When editing this property in an Edit view, client validation will call an action named *UserNameAvailable* on the *UsersController* class in order to validate this field.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

<span data-ttu-id="02b53-498">次の例は、対応するコント ローラーを示しています。</span><span class="sxs-lookup"><span data-stu-id="02b53-498">The following example shows the corresponding controller.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

<span data-ttu-id="02b53-499">既定では、属性を適用するプロパティの名前は、クエリ文字列パラメーターとしてアクション メソッドに送信されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-499">By default, the property name that the attribute is applied to is sent to the action method as a query-string parameter.</span></span>

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a><span data-ttu-id="02b53-500">"LabelFor"と"LabelForModel"メソッドの新しいオーバー ロード</span><span class="sxs-lookup"><span data-stu-id="02b53-500">New Overloads for "LabelFor" and "LabelForModel" Methods</span></span>

<span data-ttu-id="02b53-501">新しいオーバー ロードが追加されて、 *LabelFor*と*LabelForModel*するのに便利な方法は、ラベルのテキストを指定します。</span><span class="sxs-lookup"><span data-stu-id="02b53-501">New overloads have been added for the *LabelFor* and *LabelForModel* methods that let you specify the label text.</span></span> <span data-ttu-id="02b53-502">次の例では、これらのオーバー ロードを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="02b53-502">The following example shows how to use these overloads.</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a><span data-ttu-id="02b53-503">子アクション出力キャッシュ</span><span class="sxs-lookup"><span data-stu-id="02b53-503">Child Action Output Caching</span></span>

<span data-ttu-id="02b53-504">*OutputCacheAttribute*を使用して呼び出す子アクションの出力がキャッシュをサポートしている、 *Html.RenderAction*または*Html.Action*ヘルパー メソッドです。</span><span class="sxs-lookup"><span data-stu-id="02b53-504">The *OutputCacheAttribute* supports output caching of child actions that are called by using the *Html.RenderAction* or *Html.Action* helper methods.</span></span> <span data-ttu-id="02b53-505">次の例では、別のアクションを呼び出すビューを示します。</span><span class="sxs-lookup"><span data-stu-id="02b53-505">The following example shows a view that calls another action.</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

<span data-ttu-id="02b53-506">*GetDate*アクションの注釈が付いて、 *OutputCacheAttribute*:</span><span class="sxs-lookup"><span data-stu-id="02b53-506">The *GetDate* action is annotated with the *OutputCacheAttribute*:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

<span data-ttu-id="02b53-507">このコードを実行すると Html.Action("GetDate") への呼び出しの結果が 100 秒間キャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="02b53-507">When this code runs, the result of the call to Html.Action("GetDate") is cached for 100 seconds.</span></span>

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a><span data-ttu-id="02b53-508">"ビューの追加 ダイアログ ボックスの機能強化</span><span class="sxs-lookup"><span data-stu-id="02b53-508">"Add View" Dialog Box Improvements</span></span>

<span data-ttu-id="02b53-509">厳密に型指定されたビューを追加するときに、ビューの追加 ダイアログ ボックスはよりも多くの中核となる .NET Framework 型などの以前のリリースで該当しない種類の詳細を今すぐフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="02b53-509">When you add a strongly typed view, the Add View dialog box now filters out more non-applicable types than in previous releases, such as many core .NET Framework types.</span></span> <span data-ttu-id="02b53-510">また、一覧は、クラス名と種類を検索しやすくなる完全修飾型名ではなくは今すぐ並べ替えられます。</span><span class="sxs-lookup"><span data-stu-id="02b53-510">Also, the list is now sorted by the class name and not by the fully qualified type name, which makes it easier to find types.</span></span> <span data-ttu-id="02b53-511">たとえば、型名は、次の例のようにが表示されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-511">For example, the type name is now displayed as in the following example:</span></span>

<span data-ttu-id="02b53-512">クラス名 (名前空間)</span><span class="sxs-lookup"><span data-stu-id="02b53-512">ClassName (namespace)</span></span>

<span data-ttu-id="02b53-513">以前のリリースでこのよう表示されている次のよう。</span><span class="sxs-lookup"><span data-stu-id="02b53-513">In earlier releases, this would have been displayed as the following:</span></span>

<span data-ttu-id="02b53-514">Namespace.ClassName</span><span class="sxs-lookup"><span data-stu-id="02b53-514">Namespace.ClassName</span></span>

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a><span data-ttu-id="02b53-515">詳細な要求の検証</span><span class="sxs-lookup"><span data-stu-id="02b53-515">Granular Request Validation</span></span>

<span data-ttu-id="02b53-516">*除外*プロパティ*ValidateInputAttribute*は存在しません。</span><span class="sxs-lookup"><span data-stu-id="02b53-516">The *Exclude* property of *ValidateInputAttribute* no longer exists.</span></span> <span data-ttu-id="02b53-517">モデル バインド中に、モデルの特定のプロパティをスキップする要求の検証には、代わりに、新しい*SkipRequestValidationAttribute*です。</span><span class="sxs-lookup"><span data-stu-id="02b53-517">Instead, to have request validation skipped for specific properties of a model during model binding, use the new *SkipRequestValidationAttribute*.</span></span>

<span data-ttu-id="02b53-518">たとえば、アクション メソッドは、ブログの投稿を編集するために使用します。</span><span class="sxs-lookup"><span data-stu-id="02b53-518">For example, suppose an action method is used to edit a blog post:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

<span data-ttu-id="02b53-519">次の例では、ブログの投稿のビュー モデルを示します。</span><span class="sxs-lookup"><span data-stu-id="02b53-519">The following example shows the view model for a blog post.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

<span data-ttu-id="02b53-520">ユーザーは、Description プロパティの一部のマークアップを送信するときにモデル バインドは要求の検証のため失敗します。</span><span class="sxs-lookup"><span data-stu-id="02b53-520">When a user submits some markup for the Description property, model binding will fail due to request validation.</span></span> <span data-ttu-id="02b53-521">ブログの投稿の説明のモデル バインド中に要求の検証を無効にするには、適用、 *SkipRequpestValidationAttribute*プロパティのこの例のように: です。</span><span class="sxs-lookup"><span data-stu-id="02b53-521">To disable request validation during model binding for the blog post Description, apply the *SkipRequpestValidationAttribute* to the property, as shown in this example:.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

<span data-ttu-id="02b53-522">また、モデルのすべてのプロパティ用の要求の検証をオフに、次のように適用します。 *ValidateInputAttribute*の値を持つ*false*アクション メソッドに。</span><span class="sxs-lookup"><span data-stu-id="02b53-522">Alternatively, to turn off request validation for every property of the model, apply *ValidateInputAttribute* with a value of *false* to the action method:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a><span data-ttu-id="02b53-523">互換性に影響する変更点</span><span class="sxs-lookup"><span data-stu-id="02b53-523">Breaking Changes</span></span>

- <span data-ttu-id="02b53-524">例外フィルターが同じであるため、例外フィルターの実行の順序が変更された*順序*値。</span><span class="sxs-lookup"><span data-stu-id="02b53-524">The order of execution for exception filters has changed for exception filters that have the same *Order* value.</span></span> <span data-ttu-id="02b53-525">ASP.NET MVC 2 以前のバージョンで同じがあったコント ローラー上の例外がフィルター*順序*例外フィルター、アクション メソッドにする前に実行されたアクション メソッド上のものとします。</span><span class="sxs-lookup"><span data-stu-id="02b53-525">In ASP.NET MVC 2 and earlier, exception filters on the controller that had the same *Order* as those on an action method were executed before the exception filters on the action method.</span></span> <span data-ttu-id="02b53-526">例外フィルターが適用されたときに、大文字と小文字を一般にこのように、指定したせず*順序*値。</span><span class="sxs-lookup"><span data-stu-id="02b53-526">This would typically be the case when exception filters were applied without a specified *Order* value.</span></span> <span data-ttu-id="02b53-527">ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="02b53-527">In ASP.NET MVC 3, this order has been reversed so that the most specific exception handler executes first.</span></span> <span data-ttu-id="02b53-528">以前のバージョンと場合、*順序*プロパティが明示的に指定されて、フィルターは、指定した順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-528">As in earlier versions, if the *Order* property is explicitly specified, the filters are run in the specified order.</span></span>
- <span data-ttu-id="02b53-529">という名前の新しいプロパティを追加*FileExtensions*を*VirtualPathProviderViewEngine*基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="02b53-529">Added a new property named *FileExtensions* to the *VirtualPathProviderViewEngine* base class.</span></span> <span data-ttu-id="02b53-530">検索時に、ビュー パスを使用して、名前ではなく) に含まれているファイル拡張子を持つビューのみ、この新しいプロパティで指定されたリストと見なされます。</span><span class="sxs-lookup"><span data-stu-id="02b53-530">When looking up a view by path (and not by name), only views with a file extension contained in the list specified by this new property is considered.</span></span> <span data-ttu-id="02b53-531">これは、web フォーム ビューのカスタムのファイル拡張子を有効にするカスタム ビルド プロバイダーを登録し、名前ではなく、完全なパスを使用してそれらのビューを参照している人にとって重大な変更です。</span><span class="sxs-lookup"><span data-stu-id="02b53-531">This is a breaking change for those who register a custom build provider to enable a custom file extension for web form views and are referencing those views by using a full path rather than a name.</span></span> <span data-ttu-id="02b53-532">回避策がの値を変更するには、 *FileExtensions*プロパティにカスタムのファイル拡張子を含めます。</span><span class="sxs-lookup"><span data-stu-id="02b53-532">The workaround is to modify the value of the *FileExtensions* property to include the custom file extension.</span></span>

<a id="_Toc276711795"></a>
## <a name="known-issues"></a><span data-ttu-id="02b53-533">既知の問題</span><span class="sxs-lookup"><span data-stu-id="02b53-533">Known Issues</span></span>

- <span data-ttu-id="02b53-534">インストーラーは、Visual Studio 2010 のコンポーネントを更新するために完了する ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-534">The installer may take much longer than previous versions of ASP.NET MVC to complete because it updates components of Visual Studio 2010.</span></span>
- <span data-ttu-id="02b53-535">ビューのスキャフォールディング astrongly を選択するときに、スキャフォールディング書き込み専用プロパティの表示を入力します。</span><span class="sxs-lookup"><span data-stu-id="02b53-535">The Add View scaffolding when selecting astrongly typed view scaffolds write-only properties.</span></span> <span data-ttu-id="02b53-536">これらは、スキャフォールディングによって常に無視する必要があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-536">These should always be ignored by scaffolding.</span></span> <span data-ttu-id="02b53-537">ビューの追加 ダイアログもスキャフォールディング読み取り専用プロパティを「編集」または「作成」ビューを生成するときにします。</span><span class="sxs-lookup"><span data-stu-id="02b53-537">The Add View dialog also scaffolds read-only properties when generating an "Edit" or "Create" view.</span></span> <span data-ttu-id="02b53-538">読み取り専用プロパティは、表示とリスト ビューのスキャフォールディングされたのみ必要があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-538">Read-only properties should only be scaffolded for the Display and List views.</span></span>
- <span data-ttu-id="02b53-539">ASP.NET MVC 3 は、非同期 CTP と共にインストールされるときに、デバッグは機能しません。</span><span class="sxs-lookup"><span data-stu-id="02b53-539">Debugging doesn't work when ASP.NET MVC 3 is installed alongside the Async CTP.</span></span> <span data-ttu-id="02b53-540">ASP.NET MVC 3 では、非同期 CTP を並列でインストールされているをすることはできません。</span><span class="sxs-lookup"><span data-stu-id="02b53-540">ASP.NET MVC 3 cannot be installed side-by-side with the Async CTP.</span></span> <span data-ttu-id="02b53-541">デバッグを修復する非同期 CTP をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="02b53-541">Uninstall the Async CTP to repair debugging.</span></span> <span data-ttu-id="02b53-542">詳細については、読み取り[このブログの投稿](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html)について ASP.NET MVC 3 RC のすべての部分をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="02b53-542">For more details, read [this blog post](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) about uninstalling all the pieces of ASP.NET MVC 3 RC.</span></span>
- <span data-ttu-id="02b53-543">Razor Intellisense では、Resharper がインストールされている場合は機能しません。</span><span class="sxs-lookup"><span data-stu-id="02b53-543">Razor Intellisense does not work when Resharper is installed.</span></span> <span data-ttu-id="02b53-544">ReSharper をインストールして ASP.NET MVC 3 RC をお読みください Razor intellisense サポートのメリットを利用する[このブログの投稿](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)JetBrains 現時点で併用する方法を説明するからです。</span><span class="sxs-lookup"><span data-stu-id="02b53-544">If you have ReSharper installed and want to take advantage of the Razor intellisense support in ASP.NET MVC 3 RC, please read [this blog post](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) from JetBrains which discusses ways to use them together today.</span></span>
- <span data-ttu-id="02b53-545">ASP.NET MVC 3 のベータ版で作成した CSHTML および VBHTML のビューはありませんビルド アクションが正しくパブリッシュの対象から除外しています。</span><span class="sxs-lookup"><span data-stu-id="02b53-545">CSHTML and VBHTML views created with Beta of ASP.NET MVC 3 do not have their build action correctly which omits them from publishing.</span></span> <span data-ttu-id="02b53-546">*ビルド アクション*これらのファイルは、"Content"に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-546">The *Build Action* for these files should be set to "Content".</span></span> <span data-ttu-id="02b53-547">ASP.NET MVC 3 RC では、新しいファイルには、この問題を修正がベータ版で作成されたプロジェクトの既存のファイルの設定を修正しません。</span><span class="sxs-lookup"><span data-stu-id="02b53-547">ASP.NET MVC 3 RC fixes this issue for new files, but doesn't correct the setting for existing files for a project created with the Beta.</span></span>
- <span data-ttu-id="02b53-548">インストーラーは、Visual Studio 2010 のコンポーネントを更新するために完了する ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-548">The installer may take much longer than previous versions of ASP.NET MVC to complete because it updates components of Visual Studio 2010.</span></span>
- <span data-ttu-id="02b53-549">ビューの追加のスキャフォールディング強く"Edit"を選択すると、ビューのスキャフォールディングを型指定されたときに読み取り専用プロパティを指定します。</span><span class="sxs-lookup"><span data-stu-id="02b53-549">The Add View scaffolding when selecting an "Edit" strongly typed view scaffolds read only properties.</span></span> <span data-ttu-id="02b53-550">同様に、書き込み専用プロパティは「表示」ビューのスキャフォールディングされました。</span><span class="sxs-lookup"><span data-stu-id="02b53-550">Likewise, write-only properties are scaffolded for "Display" views.</span></span>
- <span data-ttu-id="02b53-551">インストール中に、使用許諾契約書への同意 ダイアログ ボックスでは、ものよりも小さいウィンドウをライセンス条項が表示されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-551">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>
- <span data-ttu-id="02b53-552">Visual Studio Async CTP をインストールすると、リリースでは、Razor ツールをインストール、ASP.NET MVC 3 の一部として含まれている競合が発生します。</span><span class="sxs-lookup"><span data-stu-id="02b53-552">Installing the Visual Studio Async CTP causes a conflict with the Razor release that is included as part of the ASP.NET MVC 3 tooling installation.</span></span> <span data-ttu-id="02b53-553">同じコンピューターに Visual Studio Async CTP と Razor リリースの両方をインストールしてくださいいないを確認します。</span><span class="sxs-lookup"><span data-stu-id="02b53-553">Make sure that you do not try to install both the Visual Studio Async CTP and the Razor release on the same machine.</span></span>
- <span data-ttu-id="02b53-554">Razor ビュー (.cshtml ファイル) を編集するときは、Visual Studio でのコント ローラーに移動するメニュー項目は使用できない、コード スニペットはありません。</span><span class="sxs-lookup"><span data-stu-id="02b53-554">When you are editing a Razor view (.cshtml file), the Go To Controller menu item in Visual Studio will not be available, and there are no code snippets.</span></span>

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a><span data-ttu-id="02b53-555">ASP.NET MVC 3 のベータ版</span><span class="sxs-lookup"><span data-stu-id="02b53-555">ASP.NET MVC 3 Beta</span></span>

<span data-ttu-id="02b53-556">ASP.NET MVC 3 のベータ版は、2010 年 10 月 6 日にリリースされました。</span><span class="sxs-lookup"><span data-stu-id="02b53-556">ASP.NET MVC 3 Beta was released on October 6, 2010.</span></span> <span data-ttu-id="02b53-557">次の注意事項は、ベータ リリースに固有対象となります更新や変更前の ASP.NET MVC 3 のリリース候補セクションで参照されています。</span><span class="sxs-lookup"><span data-stu-id="02b53-557">The following notes are specific to the Beta release, and are subject to any updates or changes referenced in the ASP.NET MVC 3 Release Candidate section above.</span></span>

## <a id="0.1__Toc274034215"></a>  <span data-ttu-id="02b53-558">新しい Featuresin ASP.NET MVC 3 のベータ版</span><span class="sxs-lookup"><span data-stu-id="02b53-558">New Featuresin ASP.NET MVC 3 Beta</span></span>

<a id="0.1__Default_validation_system"></a><span data-ttu-id="02b53-559">このセクションの内容が導入された機能について説明します、ASP.NET MVC 3 のベータ リリースでします。</span><span class="sxs-lookup"><span data-stu-id="02b53-559">This section describes features that have been introduced in the ASP.NET MVC 3 Beta release.</span></span>

### <a id="0.1__Toc274034216"></a>  <span data-ttu-id="02b53-560">NuGet パッケージ マネージャー</span><span class="sxs-lookup"><span data-stu-id="02b53-560">NuGet Package Manager</span></span>

<span data-ttu-id="02b53-561">ASP.NET MVC 3 では、NuGet パッケージ マネージャーは、追加するライブラリ用の統合パッケージ管理ツールおよび Visual Studio プロジェクトにツールが含まれます。</span><span class="sxs-lookup"><span data-stu-id="02b53-561">ASP.NET MVC 3 includes NuGet Package Manager, which is an integrated package management tool for adding libraries and tools to Visual Studio projects.</span></span> <span data-ttu-id="02b53-562">ほとんどの場合、開発者は、ソース ツリーにライブラリを取得する現在ための手順を自動化します。</span><span class="sxs-lookup"><span data-stu-id="02b53-562">For the most part, it automates the steps that developers take today to get a library into their source tree.</span></span>

<span data-ttu-id="02b53-563">コマンド ライン ツール、Visual Studio のコンテキスト メニューから Visual Studio 2010 内の統合されたコンソール ウィンドウと PowerShell コマンドレットのセットは、NuGet を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="02b53-563">You can work with NuGet as a command line tool, as an integrated console window inside Visual Studio 2010, from the Visual Studio context menu, and as set of PowerShell cmdlets.</span></span>

<span data-ttu-id="02b53-564">NuGet の詳細については、読み取り、 [NuGet のドキュメント](https://docs.microsoft.com/nuget/)です。</span><span class="sxs-lookup"><span data-stu-id="02b53-564">For more information about NuGet, read the [NuGet Documentation](https://docs.microsoft.com/nuget/).</span></span>

### <a id="0.1__Toc274034217"></a>  <span data-ttu-id="02b53-565">[新しいプロジェクト] ダイアログ ボックスの向上</span><span class="sxs-lookup"><span data-stu-id="02b53-565">Improved New Project Dialog Box</span></span>

<span data-ttu-id="02b53-566">新しいプロジェクトを作成するときに新しいプロジェクト ダイアログ ボックスでできるようになりました、ビュー エンジンと ASP.NET MVC プロジェクトの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="02b53-566">When you create a new project, the New Project dialog box now lets you specify the view engine as well as an ASP.NET MVC project type.</span></span>

![](mvc3-release-notes/_static/image6.png)

<span data-ttu-id="02b53-567">このリリースでは、テンプレートとエンジンがダイアログ ボックスに表示するビューの一覧を変更するためのサポートが含まれていません。</span><span class="sxs-lookup"><span data-stu-id="02b53-567">Support for modifying the list of templates and view engines listed in the dialog box is not included in this release.</span></span>

<span data-ttu-id="02b53-568">既定のテンプレートは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="02b53-568">The default templates are the following:</span></span>

<span data-ttu-id="02b53-569">空です。</span><span class="sxs-lookup"><span data-stu-id="02b53-569">Empty.</span></span> <span data-ttu-id="02b53-570">最小の既定のディレクトリ構造 ASP.NET MVC プロジェクトの既定の ASP.NET MVC は、次のスタイル、および既定の JavaScript ファイルを含むスクリプト ディレクトリを含む小さな Site.css ファイルを含む、ASP.NET MVC プロジェクトのファイル セットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-570">Contains a minimal set of files for an ASP.NET MVC project, including the default directory structure for ASP.NET MVC projects, a small Site.css file that contains the default ASP.NET MVC styles, and a Scripts directory that contains the default JavaScript files.</span></span>

<span data-ttu-id="02b53-571">インターネット アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="02b53-571">Internet Application.</span></span> <span data-ttu-id="02b53-572">ASP.NET MVC でのメンバーシップ プロバイダーを使用する方法を示すサンプルの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-572">Contains sample functionality that demonstrates how to use the membership provider within ASP.NET MVC.</span></span>

### <a id="0.1__Toc274034218"></a>  <span data-ttu-id="02b53-573">厳密に指定することは簡略化された Razor ビューでのモデルの入力</span><span class="sxs-lookup"><span data-stu-id="02b53-573">Simplified Way to Specify Strongly Typed Models in Razor Views</span></span>

<span data-ttu-id="02b53-574">新しいを使用して、厳密に型指定の Razor ビューのモデルの型を指定する方法を簡略化された@modelCSHTML ビューのディレクティブと@ModelTypeディレクティブ VBHTML ビューです。</span><span class="sxs-lookup"><span data-stu-id="02b53-574">The way to specify the model type for strongly typed Razor views has been simplified by using the new @model directive for CSHTML views and @ModelType directive for VBHTML views.</span></span> <span data-ttu-id="02b53-575">ASP.NET MVC の以前のバージョンでは、Razor の厳密に型指定されたモデルは、この方法を表示を指定します。</span><span class="sxs-lookup"><span data-stu-id="02b53-575">In earlier versions of ASP.NET MVC, you would specify a strongly typed model for Razor views this way:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

<span data-ttu-id="02b53-576">このリリースでは、次の構文を使用できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-576">In this release, you can use the following syntax:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  <span data-ttu-id="02b53-577">新しい ASP.NET Web ページのヘルパー メソッドをサポートします。</span><span class="sxs-lookup"><span data-stu-id="02b53-577">Support for New ASP.NET Web Pages Helper Methods</span></span>

<span data-ttu-id="02b53-578">新しい ASP.NET Web Pages テクノロジには、一連ビューとコント ローラーに一般的に使用される機能を追加するための便利なヘルパー メソッドにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-578">The new ASP.NET Web Pages technology includes a set of helper methods that are useful for adding commonly used functionality to views and controllers.</span></span> <span data-ttu-id="02b53-579">ASP.NET MVC 3 では、コント ローラーとビュー内でこれらのヘルパー メソッドを使用して、(必要な場合) をサポートします。</span><span class="sxs-lookup"><span data-stu-id="02b53-579">ASP.NET MVC 3 supports using these helper methods within controllers and views (where appropriate).</span></span> <span data-ttu-id="02b53-580">これらのメソッドは、System.Web.Helpers アセンブリに格納されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-580">These methods are contained in the System.Web.Helpers assembly.</span></span> <span data-ttu-id="02b53-581">次の表は、ASP.NET Web Pages のヘルパー メソッドのいくつかを示します。</span><span class="sxs-lookup"><span data-stu-id="02b53-581">The following table lists a few of the ASP.NET Web Pages helper methods.</span></span>

| <span data-ttu-id="02b53-582">**ヘルパー**</span><span class="sxs-lookup"><span data-stu-id="02b53-582">**Helper**</span></span> | <span data-ttu-id="02b53-583">**説明**</span><span class="sxs-lookup"><span data-stu-id="02b53-583">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="02b53-584">グラフ</span><span class="sxs-lookup"><span data-stu-id="02b53-584">Chart</span></span> | <span data-ttu-id="02b53-585">ビュー内のグラフを表示します。</span><span class="sxs-lookup"><span data-stu-id="02b53-585">Renders a chart within a view.</span></span> <span data-ttu-id="02b53-586">Chart.ToWebImage、Chart.Save、Chart.Write などのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-586">Contains methods such as Chart.ToWebImage, Chart.Save, and Chart.Write.</span></span> |
| <span data-ttu-id="02b53-587">Crypto</span><span class="sxs-lookup"><span data-stu-id="02b53-587">Crypto</span></span> | <span data-ttu-id="02b53-588">ハッシュを正しく作成するアルゴリズムを使用では、ソルトし、パスワードをハッシュします。</span><span class="sxs-lookup"><span data-stu-id="02b53-588">Uses hashing algorithms to create properly salted and hashed passwords.</span></span> |
| <span data-ttu-id="02b53-589">WebGrid</span><span class="sxs-lookup"><span data-stu-id="02b53-589">WebGrid</span></span> | <span data-ttu-id="02b53-590">オブジェクト (通常は、データベースからのデータ) のコレクションをグリッドとして表示します。</span><span class="sxs-lookup"><span data-stu-id="02b53-590">Renders a collection of objects (typically, data from a database) as a grid.</span></span> <span data-ttu-id="02b53-591">ページングや並べ替えをサポートします。</span><span class="sxs-lookup"><span data-stu-id="02b53-591">Supports paging and sorting.</span></span> |
| <span data-ttu-id="02b53-592">WebImage</span><span class="sxs-lookup"><span data-stu-id="02b53-592">WebImage</span></span> | <span data-ttu-id="02b53-593">イメージを表示します。</span><span class="sxs-lookup"><span data-stu-id="02b53-593">Renders an image.</span></span> |
| <span data-ttu-id="02b53-594">WebMail</span><span class="sxs-lookup"><span data-stu-id="02b53-594">WebMail</span></span> | <span data-ttu-id="02b53-595">電子メールを送信します。</span><span class="sxs-lookup"><span data-stu-id="02b53-595">Sends an email message.</span></span> |

<span data-ttu-id="02b53-596">ヘルパーと基本的な構文を示すクイック リファレンス トピックでは、使用可能なドキュメントの一部として、ASP.NET Razor 構文で、次の URL:</span><span class="sxs-lookup"><span data-stu-id="02b53-596">A quick reference topic that lists the helpers and basic syntax is available as part of the ASP.NET Razor syntax documentation at the following URL:</span></span>

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  <span data-ttu-id="02b53-597">追加の依存関係の挿入のサポート</span><span class="sxs-lookup"><span data-stu-id="02b53-597">Additional Dependency Injection Support</span></span>

<span data-ttu-id="02b53-598">ASP.NET MVC 3 Preview 1 のリリースでの構築、現在のリリースには、2 つの新しいサービスや既存の 4 つのサービスのサポートを追加および依存関係の解決と共通サービス ロケーターのサポートの改善が含まれます。</span><span class="sxs-lookup"><span data-stu-id="02b53-598">Building on the ASP.NET MVC 3 Preview 1 release, the current release includes added support for two new services and four existing services, and improved support for dependency resolution and the Common Service Locator.</span></span>

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a><span data-ttu-id="02b53-599">粒度の細かいコント ローラー インスタンスを作成して新しい IControllerActivator インターフェイス</span><span class="sxs-lookup"><span data-stu-id="02b53-599">New IControllerActivator Interface for Fine-Grained Controller Instantiation</span></span>

<span data-ttu-id="02b53-600">新しい IControllerActivator インターフェイスは、依存関係の挿入を使用してコント ローラーがインスタンス化する方法より詳細に制御を提供します。</span><span class="sxs-lookup"><span data-stu-id="02b53-600">The new IControllerActivator interface provides more fine-grained control over how controllers are instantiated via dependency injection.</span></span> <span data-ttu-id="02b53-601">次の例は、インターフェイスを示しています。</span><span class="sxs-lookup"><span data-stu-id="02b53-601">The following example shows the interface:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

<span data-ttu-id="02b53-602">これは、コント ローラー ファクトリの役割です。</span><span class="sxs-lookup"><span data-stu-id="02b53-602">Contrast this to the role of the controller factory.</span></span> <span data-ttu-id="02b53-603">コント ローラー ファクトリは、コント ローラーの種類を検索するためとそのコント ローラーの種類のインスタンスをインスタンス化するための両方を担当する IControllerFactory インターフェイスの実装です。</span><span class="sxs-lookup"><span data-stu-id="02b53-603">A controller factory is an implementation of the IControllerFactory interface, which is responsible both for locating a controller type and for instantiating an instance of that controller type.</span></span>

<span data-ttu-id="02b53-604">コント ローラー アクティベーターは担当のみコント ローラーの種類のインスタンスをインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="02b53-604">Controller activators are responsible only for instantiating an instance of a controller type.</span></span> <span data-ttu-id="02b53-605">コント ローラーの種類の参照は行いません。</span><span class="sxs-lookup"><span data-stu-id="02b53-605">They do not perform the controller type lookup.</span></span> <span data-ttu-id="02b53-606">適切なコント ローラーの種類を特定すると、コント ローラーの実際のインスタンス化を処理するコント ローラー ファクトリ IControllerActivator のインスタンスに委任する必要があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-606">After locating a proper controller type, controller factories should delegate to an instance of IControllerActivator to handle the actual instantiation of the controller.</span></span>

<span data-ttu-id="02b53-607">DefaultControllerFactory クラスには、新しい IControllerFactory インスタンスを受け取るコンス トラクターがあります。</span><span class="sxs-lookup"><span data-stu-id="02b53-607">The DefaultControllerFactory class has a new constructor that accepts an IControllerFactory instance.</span></span> <span data-ttu-id="02b53-608">これにより、既定のコント ローラーの種類のルックアップ動作をオーバーライドすることがなくコント ローラー作成のこの面を管理する依存関係の挿入を適用できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-608">This lets you apply Dependency Injection to manage this aspect of controller creation without having to override the default controller-type lookup behavior.</span></span>

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a><span data-ttu-id="02b53-609">IDependencyResolver を置き換え IServiceLocator インターフェイス</span><span class="sxs-lookup"><span data-stu-id="02b53-609">IServiceLocator Interface Replaced with IDependencyResolver</span></span>

<span data-ttu-id="02b53-610">コミュニティからのフィードバックに基づき、ASP.NET MVC 3 のベータ リリースは置き換え IServiceLocator インターフェイスの使用する ASP.NET MVC のニーズに応じた取り除か IDependencyResolver インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="02b53-610">Based on community feedback, the ASP.NET MVC 3 Beta release has replaced the use of the IServiceLocator interface with a slimmed-down IDependencyResolver interface specific to the needs of ASP.NET MVC.</span></span> <span data-ttu-id="02b53-611">次の例は、新しいインターフェイスを示しています。</span><span class="sxs-lookup"><span data-stu-id="02b53-611">The following example shows the new interface:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

<span data-ttu-id="02b53-612">この変更の一環として、ServiceLocator クラスも置き換えられた DependencyResolver クラスです。</span><span class="sxs-lookup"><span data-stu-id="02b53-612">As part of this change, the ServiceLocator class was also replaced with the DependencyResolver class.</span></span> <span data-ttu-id="02b53-613">依存関係競合回避モジュールの登録は、ASP.NET MVC の以前のバージョンと同様です。</span><span class="sxs-lookup"><span data-stu-id="02b53-613">Registration of a dependency resolver is similar to earlier versions of ASP.NET MVC:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

<span data-ttu-id="02b53-614">このインターフェイスの実装は、要求された型に対して登録されているサービスを提供する基になる依存性の注入コンテナーに委任だけ必要があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-614">Implementations of this interface should simply delegate to the underlying dependency injection container to provide the registered service for the requested type.</span></span>

<span data-ttu-id="02b53-615">要求された型の登録済みのサービスがない場合は、ASP.NET MVC は GetService から null を返すと、GetServices から空のコレクションを返すには、このインターフェイスの実装が必要です。</span><span class="sxs-lookup"><span data-stu-id="02b53-615">When there are no registered services of the requested type, ASP.NET MVC expects implementations of this interface to return null from GetService and to return an empty collection from GetServices.</span></span>

<span data-ttu-id="02b53-616">新しい DependencyResolver クラスを使用して、新しい IDependencyResolver インターフェイスまたは共通サービス ロケーターのインターフェイス (IServiceLocator) のいずれかを実装するクラスを登録できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-616">The new DependencyResolver class lets you register classes that implement either the new IDependencyResolver interface or the Common Service Locator interface (IServiceLocator).</span></span> <span data-ttu-id="02b53-617">共通サービス ロケーターの詳細については、次を参照してください。 [github CommonServiceLocator](https://github.com/unitycontainer/commonservicelocator)です。</span><span class="sxs-lookup"><span data-stu-id="02b53-617">For more information about Common Service Locator, see [CommonServiceLocator on GitHub](https://github.com/unitycontainer/commonservicelocator).</span></span>

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a><span data-ttu-id="02b53-618">粒度の細かいビュー ページ インスタンスを作成して新しい IViewActivator インターフェイス</span><span class="sxs-lookup"><span data-stu-id="02b53-618">New IViewActivator Interface for Fine-Grained View Page Instantiation</span></span>

<span data-ttu-id="02b53-619">新しい IViewPageActivator インターフェイスは、依存関係の挿入を使用してページの表示がインスタンス化する方法より詳細に制御を提供します。</span><span class="sxs-lookup"><span data-stu-id="02b53-619">The new IViewPageActivator interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="02b53-620">これは、WebFormView インスタンスと RazorView インスタンスの両方に適用されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-620">This applies to both WebFormView instances and RazorView instances.</span></span> <span data-ttu-id="02b53-621">次の例は、新しいインターフェイスを示しています。</span><span class="sxs-lookup"><span data-stu-id="02b53-621">The following example shows the new interface:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

<span data-ttu-id="02b53-622">これらのクラスを受け付けることができます、ViewPage、ViewUserControl、および WebViewPage 型がインスタンス化する方法を制御する依存関係の挿入を使用する、IViewPageActivator コンス トラクター引数。</span><span class="sxs-lookup"><span data-stu-id="02b53-622">These classes now accept an IViewPageActivator constructor argument, which lets you use dependency injection to control how the ViewPage, ViewUserControl, and WebViewPage types are instantiated.</span></span>

#### <a name="new-dependency-resolver-support-for-existing-services"></a><span data-ttu-id="02b53-623">既存のサービスの新しい依存関係競合回避モジュールのサポート</span><span class="sxs-lookup"><span data-stu-id="02b53-623">New Dependency Resolver Support for Existing Services</span></span>

<span data-ttu-id="02b53-624">新しいリリースには、次のサービスの依存関係の解像度のサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-624">The new release includes dependency resolution support for the following services:</span></span>

- <span data-ttu-id="02b53-625">モデル検証プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="02b53-625">Model validation providers.</span></span> <span data-ttu-id="02b53-626">ModelValidatorProvider を実装するクラスは、依存関係競合回避モジュールに登録することができ、システムがクライアント側とサーバー側の検証をサポートするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-626">Classes that implement ModelValidatorProvider can be registered in the dependency resolver, and the system will use them to support client- and server-side validation.</span></span>
- <span data-ttu-id="02b53-627">モデル メタデータ プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="02b53-627">Model metadata provider.</span></span> <span data-ttu-id="02b53-628">ModelMetadataProvider を実装する 1 つのクラスは、依存関係競合回避モジュールに登録することができ、システムを使用すると、テンプレートおよび検証システムのメタデータを提供する使用されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-628">A single class that implements ModelMetadataProvider can be registered in the dependency resolver, and the system will use it to provide metadata for the templating and validation systems.</span></span>
- <span data-ttu-id="02b53-629">値プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="02b53-629">Value providers.</span></span> <span data-ttu-id="02b53-630">ValueProviderFactory を実装するクラスは、依存関係競合回避モジュールに登録することができ、システムは、コント ローラーで、モデル バインド中に使用される値プロバイダーを作成することを使用します。</span><span class="sxs-lookup"><span data-stu-id="02b53-630">Classes that implement ValueProviderFactory can be registered in the dependency resolver, and the system will use them to create value providers that are consumed by the controller and during model binding.</span></span>
- <span data-ttu-id="02b53-631">モデル バインダー。</span><span class="sxs-lookup"><span data-stu-id="02b53-631">Model binders.</span></span> <span data-ttu-id="02b53-632">IModelBinderProvider を実装するクラスは、依存関係競合回避モジュールに登録することができ、システムが使用して作成するモデル バインド システムで使用されるモデル バインダー。</span><span class="sxs-lookup"><span data-stu-id="02b53-632">Classes that implement IModelBinderProvider can be registered in the dependency resolver, and the system will use them to create model binders that are consumed by the model binding system.</span></span>

### <a id="0.1__Toc274034221"></a>  <span data-ttu-id="02b53-633">控えめな jQuery ベース Ajax のサポート</span><span class="sxs-lookup"><span data-stu-id="02b53-633">New Support for Unobtrusive jQuery-Based Ajax</span></span>

<span data-ttu-id="02b53-634">ASP.NET MVC には、次のよう Ajax ヘルパー メソッドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="02b53-634">ASP.NET MVC includes Ajax helper methods such as the following:</span></span>

- <span data-ttu-id="02b53-635">Ajax.ActionLink</span><span class="sxs-lookup"><span data-stu-id="02b53-635">Ajax.ActionLink</span></span>
- <span data-ttu-id="02b53-636">Ajax.RouteLink</span><span class="sxs-lookup"><span data-stu-id="02b53-636">Ajax.RouteLink</span></span>
- <span data-ttu-id="02b53-637">Ajax.BeginForm</span><span class="sxs-lookup"><span data-stu-id="02b53-637">Ajax.BeginForm</span></span>
- <span data-ttu-id="02b53-638">Ajax.BeginRouteForm</span><span class="sxs-lookup"><span data-stu-id="02b53-638">Ajax.BeginRouteForm</span></span>

<span data-ttu-id="02b53-639">これらのメソッドでは、JavaScript を使用して、完全なポストバックを使用するのではなく、サーバーでアクション メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="02b53-639">These methods use JavaScript to invoke an action method on the server rather than using a full postback.</span></span> <span data-ttu-id="02b53-640">この機能は利用するために jQuery の控えめな方法で更新されました。</span><span class="sxs-lookup"><span data-stu-id="02b53-640">This functionality has been updated to take advantage of jQuery in an unobtrusive manner.</span></span> <span data-ttu-id="02b53-641">インライン クライアント スクリプトをそのままの状態生成ではなくこれらのヘルパー メソッドの動作から分離マークアップを使用して HTML5 属性を生成することによって、*データ ajax*プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="02b53-641">Rather than intrusively emitting inline client scripts, these helper methods separate the behavior from the markup by emitting HTML5 attributes using the *data-ajax* prefix.</span></span> <span data-ttu-id="02b53-642">マークアップには、動作は、適切な JavaScript ファイルを参照することによって適用されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-642">Behavior is then applied to the markup by referencing the appropriate JavaScript files.</span></span> <span data-ttu-id="02b53-643">次の JavaScript ファイルが参照されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="02b53-643">Make sure that the following JavaScript files are referenced:</span></span>

- <span data-ttu-id="02b53-644">jquery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="02b53-644">jquery-1.4.1.js</span></span>
- <span data-ttu-id="02b53-645">jquery.unobtrusive.ajax.js</span><span class="sxs-lookup"><span data-stu-id="02b53-645">jquery.unobtrusive.ajax.js</span></span>

<span data-ttu-id="02b53-646">この機能は、ASP.NET MVC 3 プロジェクト、新しいテンプレートにある Web.config ファイルで既定で有効が、既存のプロジェクトに既定で無効にします。</span><span class="sxs-lookup"><span data-stu-id="02b53-646">This feature is enabled by default in the Web.config file in the ASP.NET MVC 3 new project templates, but is disabled by default for existing projects.</span></span> <span data-ttu-id="02b53-647">詳細については、次を参照してください。[クライアント検証と控えめな JavaScript アプリケーション全体のフラグを追加](#0.1_AddedApplicationWideFlagsForClientValida)このドキュメントで後述します。</span><span class="sxs-lookup"><span data-stu-id="02b53-647">For more information, see [Added application-wide flags for client validation and unobtrusive JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) later in this document.</span></span>

### <a id="0.1__Toc274034222"></a>  <span data-ttu-id="02b53-648">控えめな jQuery 検証用の新しいサポート</span><span class="sxs-lookup"><span data-stu-id="02b53-648">New Support for Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="02b53-649">既定では、ASP.NET MVC 3 のベータ版は、クライアント側の検証を実行するために、控えめな方法で jQuery 検証を使用します。</span><span class="sxs-lookup"><span data-stu-id="02b53-649">By default, ASP.NET MVC 3 Beta uses jQuery validation in an unobtrusive manner in order to perform client-side validation.</span></span> <span data-ttu-id="02b53-650">控えめなクライアント検証を有効にするには、ビュー内から次のような呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="02b53-650">To enable unobtrusive client validation, make a call like the following from within a view:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

<span data-ttu-id="02b53-651">これは、ViewContext.UnobtrusiveJavaScriptEnabled プロパティが true で、次の呼び出しを行うことによって行うことができますに設定されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-651">This requires that ViewContext.UnobtrusiveJavaScriptEnabled property is set to true, which you can do by making the following call:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

<span data-ttu-id="02b53-652">また、次の JavaScript ファイルが参照されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="02b53-652">Also make sure the following JavaScript files are referenced.</span></span>

- <span data-ttu-id="02b53-653">jquery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="02b53-653">jquery-1.4.1.js</span></span>
- <span data-ttu-id="02b53-654">jquery.validate.js</span><span class="sxs-lookup"><span data-stu-id="02b53-654">jquery.validate.js</span></span>
- <span data-ttu-id="02b53-655">jquery.validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="02b53-655">jquery.validate.unobtrusive.js</span></span>

<span data-ttu-id="02b53-656">この機能は、ASP.NET MVC 3 プロジェクト、新しいテンプレートにある Web.config ファイルでは既定で有効になっているが、既存のプロジェクトに既定で無効にします。</span><span class="sxs-lookup"><span data-stu-id="02b53-656">This feature is enabled on by default in the Web.config file in the ASP.NET MVC 3 new project templates, but is disabled by default for existing projects.</span></span> <span data-ttu-id="02b53-657">詳細については、次を参照してください。[クライアント検証と控えめな JavaScript アプリケーション全体のフラグを新しい](#0.1_AddedApplicationWideFlagsForClientValida)このドキュメントで後述します。</span><span class="sxs-lookup"><span data-stu-id="02b53-657">For more information, see [New application-wide flags for client validation and unobtrusive JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) later in this document.</span></span>

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  <span data-ttu-id="02b53-658">クライアント検証と控えめな JavaScript の新しいアプリケーション全体のフラグ</span><span class="sxs-lookup"><span data-stu-id="02b53-658">New Application-Wide Flags for Client Validation and Unobtrusive JavaScript</span></span>

<span data-ttu-id="02b53-659">有効にするか、クライアント検証と控えめな JavaScript が次の例のように、HtmlHelper クラスの静的メンバーを使用してグローバルに無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="02b53-659">You can enable or disable client validation and unobtrusive JavaScript globally using static members of the HtmlHelper class, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

<span data-ttu-id="02b53-660">既定のプロジェクト テンプレートは、既定では、控えめな JavaScript を有効にします。</span><span class="sxs-lookup"><span data-stu-id="02b53-660">The default project templates enable unobtrusive JavaScript by default.</span></span> <span data-ttu-id="02b53-661">有効にしたり、次の設定を使用して、アプリケーションのルートの Web.config ファイルでこれらの機能を無効にしたりできます。</span><span class="sxs-lookup"><span data-stu-id="02b53-661">You can also enable or disable these features in the root Web.config file of your application using the following settings:</span></span>

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

<span data-ttu-id="02b53-662">これらの機能を有効にするには既定ではため、次の例に示すように、既定の設定を上書きすることが HtmlHelper クラスへの新しいオーバー ロードの導入します。</span><span class="sxs-lookup"><span data-stu-id="02b53-662">Because you can enable these features by default, new overloads were introduced to the HtmlHelper class that let you override the default settings, as shown in the following examples:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

<span data-ttu-id="02b53-663">旧バージョンとの互換性のため、既定ではこれら両方の機能は無効です。</span><span class="sxs-lookup"><span data-stu-id="02b53-663">For backward compatibility, both of these features are disabled by default.</span></span>

### <a id="0.1__Toc274034224"></a>  <span data-ttu-id="02b53-664">ビューの実行前に実行されるコードのサポート</span><span class="sxs-lookup"><span data-stu-id="02b53-664">New Support for Code that Runs Before Views Run</span></span>

<span data-ttu-id="02b53-665">という名前のファイルを配置できるようになりました\_viewstart.cshtml (または\_viewstart.vbhtml) Views ディレクトリにし、そのディレクトリとそのサブディレクトリに複数のビューの間で共有されるをコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="02b53-665">You can now put a file named \_viewstart.cshtml (or \_viewstart.vbhtml) in the Views directory and add code to it that will be shared among multiple views in that directory and its subdirectories.</span></span> <span data-ttu-id="02b53-666">次のコードを配置するなど、 \_viewstart.cshtml フォルダー ページに、~/Views:</span><span class="sxs-lookup"><span data-stu-id="02b53-666">For example, you might put the following code into the \_viewstart.cshtml page in the ~/Views folder:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

<span data-ttu-id="02b53-667">これは、Views フォルダー内のすべてのビューとすべてのサブフォルダーを再帰的にレイアウト ページを設定します。</span><span class="sxs-lookup"><span data-stu-id="02b53-667">This sets the layout page for every view within the Views folder and all its subfolders recursively.</span></span> <span data-ttu-id="02b53-668">ときに、ビュー、表示されるコード、 \_viewstart.cshtml ファイルはコードの表示を実行する前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-668">When a view is being rendered, the code in the \_viewstart.cshtml file runs before the view code runs.</span></span> <span data-ttu-id="02b53-669">\_Viewstart.cshtml コードは、そのフォルダー内のすべてのビューには適用されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-669">The \_viewstart.cshtml code applies to every view in that folder.</span></span>

<span data-ttu-id="02b53-670">既定では、コード、 \_viewstart.cshtml ファイルは、任意のサブフォルダー内のビューにも適用されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-670">By default, the code in the \_viewstart.cshtml file also applies to views in any subfolder.</span></span> <span data-ttu-id="02b53-671">ただし、個別のサブフォルダーは、独自のバージョンを持つことができます、 \_viewstart.cshtml ファイルです。 その場合は、ローカル バージョンが優先されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-671">However, individual subfolders can have their own version of the \_viewstart.cshtml file; in that case, the local version takes precedence.</span></span> <span data-ttu-id="02b53-672">たとえば、HomeController のすべてのビューに共通するコードを実行するには、次のように配置します。、 \_~/Views/Home フォルダーにある viewstart.cshtml ファイル。</span><span class="sxs-lookup"><span data-stu-id="02b53-672">For example, to run code that is common to all views for the HomeController, put a \_viewstart.cshtml file in the ~/Views/Home folder.</span></span>

### <a id="0.1__Toc274034225"></a>  <span data-ttu-id="02b53-673">VBHTML Razor 構文の新しいサポート</span><span class="sxs-lookup"><span data-stu-id="02b53-673">New Support for the VBHTML Razor Syntax</span></span>

<span data-ttu-id="02b53-674">ASP.NET MVC の以前のプレビューには、c# に基づいた Razor 構文を使用して、ビューのサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="02b53-674">The previous ASP.NET MVC preview included support for views using Razor syntax based on C#.</span></span> <span data-ttu-id="02b53-675">これらのビューは、.cshtml ファイルの拡張子を使用します。</span><span class="sxs-lookup"><span data-stu-id="02b53-675">These views use the .cshtml file extension.</span></span> <span data-ttu-id="02b53-676">Razor をサポートするために継続的な作業の一環として、ASP.NET MVC 3 のベータ版には、Razor 構文 .vbhtml ファイル拡張子を使用する Visual Basic でのサポートが導入されています。</span><span class="sxs-lookup"><span data-stu-id="02b53-676">As part of ongoing work to support Razor, the ASP.NET MVC 3 Beta introduces support for the Razor syntax in Visual Basic, which uses the .vbhtml file extension.</span></span>

<span data-ttu-id="02b53-677">VBHTML ページで Visual Basic 構文の使用の概要については、次の URL でチュートリアルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="02b53-677">For an introduction to using Visual Basic syntax in VBHTML pages, see the tutorial at the following URL:</span></span>

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  <span data-ttu-id="02b53-678">ValidateInputAttribute をより細かく制御</span><span class="sxs-lookup"><span data-stu-id="02b53-678">More Granular Control over ValidateInputAttribute</span></span>

<span data-ttu-id="02b53-679">ASP.NET MVC には、受信要求に悪意のある入力が含まれていないことを確認してください ASP.NET 要求の検証のコア インフラストラクチャを呼び出す ValidateInputAttribute クラスが常に含まれます。</span><span class="sxs-lookup"><span data-stu-id="02b53-679">ASP.NET MVC has always included the ValidateInputAttribute class, which invokes the core ASP.NET request validation infrastructure to make sure that the incoming request does not contain potentially malicious input.</span></span> <span data-ttu-id="02b53-680">既定では、入力の検証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="02b53-680">By default, input validation is enabled.</span></span> <span data-ttu-id="02b53-681">次の例のように、ValidateInputAttribute 属性を使用して要求の検証を無効にできます。</span><span class="sxs-lookup"><span data-stu-id="02b53-681">It is possible to disable request validation by using the ValidateInputAttribute attribute, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

<span data-ttu-id="02b53-682">ただし、多くの web アプリケーションでは、その他のフィールドはいけない中に、HTML を使用する必要がある個々 のフォーム フィールドがあります。</span><span class="sxs-lookup"><span data-stu-id="02b53-682">However, many web applications have individual form fields that need to allow HTML, while the remaining fields should not.</span></span> <span data-ttu-id="02b53-683">ValidateInputAttribute クラスでは、要求の検証に含める必要があるないフィールドの一覧を指定できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-683">The ValidateInputAttribute class now lets you specify a list of fields that should not be included in request validation.</span></span>

<span data-ttu-id="02b53-684">たとえば、ブログ エンジンを開発している場合、フィールドの本文および概要のマークアップを許可する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-684">For example, if you are developing a blog engine, you might want to allow markup in the Body and Summary fields.</span></span> <span data-ttu-id="02b53-685">これらのフィールドは、各プロパティの名前 ("Body"および「概要」) に対応する名前の属性を持つ 2 つの入力要素によって表現されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-685">These fields might be represented by two input element, each with a name attribute corresponding to the property name ("Body" and "Summary").</span></span> <span data-ttu-id="02b53-686">これらのフィールドの検証には、要求を無効にするには、(コンマ区切り) ValidateInput クラス、例を次の除外プロパティの名前のみを指定します。</span><span class="sxs-lookup"><span data-stu-id="02b53-686">To disable request validation for these fields only, specify the names (comma-separated) in the Exclude property of the ValidateInput class, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  <span data-ttu-id="02b53-687">ヘルパーの匿名オブジェクトを使用して指定された HTML 属性名にアンダー スコアをハイフンに変換します。</span><span class="sxs-lookup"><span data-stu-id="02b53-687">Helpers Convert Underscores to Hyphens for HTML Attribute Names Specified Using Anonymous Objects</span></span>

<span data-ttu-id="02b53-688">ヘルパー メソッドを使用して、次の例のように、匿名オブジェクトを使用して属性の名前/値ペアを指定できます。</span><span class="sxs-lookup"><span data-stu-id="02b53-688">Helper methods let you specify attribute name/value pairs using an anonymous object, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

<span data-ttu-id="02b53-689">このアプローチしないを使用できます、属性名にハイフンを使用するため、ASP.NET のプロパティ名にハイフンは使用できません。</span><span class="sxs-lookup"><span data-stu-id="02b53-689">This approach doesn't let you use hyphens in the attribute name, because a hyphen cannot be used for a property name in ASP.NET.</span></span> <span data-ttu-id="02b53-690">ただし、HTML5 属性のカスタム重要なのハイフンたとえば、HTML5 では、「データの」プレフィックスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-690">However, hyphens are important for custom HTML5 attributes; for example, HTML5 uses the "data-" prefix.</span></span>

<span data-ttu-id="02b53-691">同時にアンダー スコアは HTML では、属性名は使用できませんが、プロパティ名で有効です。</span><span class="sxs-lookup"><span data-stu-id="02b53-691">At the same time, underscores cannot be used for attribute names in HTML, but are valid within property names.</span></span> <span data-ttu-id="02b53-692">そのため、匿名オブジェクトを使用して属性を指定して、属性名がアンダー スコアを含める場合は、およびに、ヘルパー メソッドは、アンダー スコアをハイフンに変換されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-692">Therefore, if you specify attributes using an anonymous object, and if the attribute names include an underscore,, helper methods will convert the underscores to hyphens.</span></span> <span data-ttu-id="02b53-693">たとえば、次のヘルパー構文は、アンダー スコアを使用します。</span><span class="sxs-lookup"><span data-stu-id="02b53-693">For example, the following helper syntax uses an underscore:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

<span data-ttu-id="02b53-694">ヘルパーの実行時に、前の例は、次のマークアップを表示します。</span><span class="sxs-lookup"><span data-stu-id="02b53-694">The previous example renders the following markup when the helper runs:</span></span>

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  <span data-ttu-id="02b53-695">バグの修正</span><span class="sxs-lookup"><span data-stu-id="02b53-695">Bug Fixes</span></span>

<span data-ttu-id="02b53-696">EditorFor と DisplayFor テンプレート ヘルパーの既定のオブジェクト テンプレートは、DisplayAttribute.Order プロパティで指定された順序付けできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="02b53-696">The default object template for the EditorFor and DisplayFor template helpers now supports the ordering specified in the DisplayAttribute.Order property.</span></span> <span data-ttu-id="02b53-697">(以前のバージョンで順序の設定が使用されません。)</span><span class="sxs-lookup"><span data-stu-id="02b53-697">(In previous versions, the Order setting was not used.)</span></span>

<span data-ttu-id="02b53-698">現在、クライアントの検証には、検証属性が適用されているオーバーライドされたプロパティの検証がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="02b53-698">Client validation now supports validation of overridden properties that have validation attributes applied.</span></span>

<span data-ttu-id="02b53-699">JsonValueProviderFactory は既定で登録されているようになりました。</span><span class="sxs-lookup"><span data-stu-id="02b53-699">JsonValueProviderFactory is now registered by default.</span></span>

## <a id="0.1__Toc274034229"></a>  <span data-ttu-id="02b53-700">重大な変更</span><span class="sxs-lookup"><span data-stu-id="02b53-700">Breaking Changes</span></span>

<span data-ttu-id="02b53-701">例外フィルターの実行順序を同じ順序の値を持つ例外フィルターが変更されました。</span><span class="sxs-lookup"><span data-stu-id="02b53-701">The order of execution for exception filters has changed for exception filters that have the same Order value.</span></span> <span data-ttu-id="02b53-702">ASP.NET MVC 2 以前のバージョンで上の例外フィルターと同じ順序で、コント ローラー アクション メソッドで例外フィルターの前に実行されたアクション メソッド上のものとします。</span><span class="sxs-lookup"><span data-stu-id="02b53-702">In ASP.NET MVC 2 and earlier, exception filters on the controller with the same Order as those on an action method were executed before the exception filters on the action method.</span></span> <span data-ttu-id="02b53-703">指定された順序値を指定せずに例外フィルターが適用されたときに、大文字と小文字を一般にこのようにします。</span><span class="sxs-lookup"><span data-stu-id="02b53-703">This would typically be the case when exception filters were applied without a specified Order value.</span></span> <span data-ttu-id="02b53-704">ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="02b53-704">In ASP.NET MVC 3, this order has been reversed so that the most specific exception handler executes first.</span></span> <span data-ttu-id="02b53-705">以前のバージョンと Order プロパティが明示的に指定されている場合、フィルターが指定した順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-705">As in earlier versions, if the Order property is explicitly specified, the filters are run in the specified order.</span></span>

## <a id="0.1__Toc274034230"></a>  <span data-ttu-id="02b53-706">既知の問題</span><span class="sxs-lookup"><span data-stu-id="02b53-706">Known Issues</span></span>

<span data-ttu-id="02b53-707">インストール中に、使用許諾契約書への同意 ダイアログ ボックスでは、ものよりも小さいウィンドウをライセンス条項が表示されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-707">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>

<span data-ttu-id="02b53-708">Razor ビューには、IntelliSense のサポートも構文の強調表示はありません。</span><span class="sxs-lookup"><span data-stu-id="02b53-708">Razor views do not have IntelliSense support nor syntax highlighting.</span></span> <span data-ttu-id="02b53-709">Visual Studio での Razor 構文のサポートが以降のリリースの一部として含めるなると予想されます。</span><span class="sxs-lookup"><span data-stu-id="02b53-709">It is anticipated that support for Razor syntax in Visual Studio will be included as part of a later release.</span></span>

<span data-ttu-id="02b53-710">Razor ビュー (CSHTML ファイル) を編集するとき、 <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> Visual Studio でのコント ローラーに移動するメニュー項目は使用できないし、コード スニペットはありません。</span><span class="sxs-lookup"><span data-stu-id="02b53-710">When you are editing a Razor view (CSHTML file), the <a id="0.1__Toc224729061"></a><a id="0.1__Toc238051347"></a> Go To Controller menu item in Visual Studio will not be available, and there are no code snippets.</span></span>

<span data-ttu-id="02b53-711">使用する場合、 @model CSHTML を厳密に型を指定する構文を表示、言語固有のショートカットの種類は認識されません。</span><span class="sxs-lookup"><span data-stu-id="02b53-711">When using the @model syntax to specify a strongly typed CSHTML view, language-specific shortcuts for types are not recognized.</span></span> <span data-ttu-id="02b53-712">たとえば、 @model int は機能しませんが、 @model Int32 は機能します。</span><span class="sxs-lookup"><span data-stu-id="02b53-712">For example, @model int will not work, but @model Int32 will work.</span></span> <span data-ttu-id="02b53-713">このバグの回避策では、モデルの種類を指定する場合は、実際の型名を使用します。</span><span class="sxs-lookup"><span data-stu-id="02b53-713">The workaround for this bug is to use the actual type name when you specify the model type.</span></span>

<span data-ttu-id="02b53-714">使用する場合、 @model CSHTML ビューを厳密に型を指定するための構文 (または@ModelTypeVBHTML ビューを厳密に型を指定)、null 許容型や配列の宣言はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="02b53-714">When using the @model syntax to specify a strongly typed CSHTML view (or @ModelType to specify a strongly typed VBHTML view), nullable types and array declarations are not supported.</span></span> <span data-ttu-id="02b53-715">たとえば、 @model int? はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="02b53-715">For example, @model int? is not supported.</span></span> <span data-ttu-id="02b53-716">代わりに、`@model Nullable<Int32>`です。</span><span class="sxs-lookup"><span data-stu-id="02b53-716">Instead, use `@model Nullable<Int32>`.</span></span> <span data-ttu-id="02b53-717">構文@modelstring[] もサポートされていません; 代わりに、`@model IList<string>`です。</span><span class="sxs-lookup"><span data-stu-id="02b53-717">The syntax @model string[] is also not supported; instead, use `@model IList<string>`.</span></span>

<span data-ttu-id="02b53-718">ASP.NET MVC 3、ASP.NET MVC 2 プロジェクトをアップグレードするときに、Web.config ファイルの appSettings セクションに、次を追加することを確認してください。</span><span class="sxs-lookup"><span data-stu-id="02b53-718">When you upgrade an ASP.NET MVC 2 project to ASP.NET MVC 3, make sure to add the following to the appSettings section of the Web.config file:</span></span>

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

<span data-ttu-id="02b53-719">常に認証されていないユーザーにリダイレクト ~/Account/ログイン、Web.config で使用されるフォーム認証設定を無視してフォーム認証の原因となる既知の問題があります。回避策では、次のアプリケーション設定を追加します。</span><span class="sxs-lookup"><span data-stu-id="02b53-719">There's a known issue that causes Forms Authentication to always redirect unauthenticated users to ~/Account/Login, ignoring the forms authentication setting used in Web.config. The workaround is to add the following app setting.</span></span>

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  <span data-ttu-id="02b53-720">免責事項</span><span class="sxs-lookup"><span data-stu-id="02b53-720">Disclaimer</span></span>

<span data-ttu-id="02b53-721">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="02b53-721">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="02b53-722">All rights reserved.</span><span class="sxs-lookup"><span data-stu-id="02b53-722">All rights reserved.</span></span> <span data-ttu-id="02b53-723">このドキュメントは提供される"としてでは、します"。</span><span class="sxs-lookup"><span data-stu-id="02b53-723">This document is provided "as-is."</span></span> <span data-ttu-id="02b53-724">情報および見解 URL および他のインターネット Web サイトの参照を含む、このドキュメントでは、通知なく変更可能性があります。</span><span class="sxs-lookup"><span data-stu-id="02b53-724">Information and views expressed in this document, including URL and other Internet Web site references, may change without notice.</span></span> <span data-ttu-id="02b53-725">お客様は、その使用に関するリスクを負うものとします。</span><span class="sxs-lookup"><span data-stu-id="02b53-725">You bear the risk of using it.</span></span>

<span data-ttu-id="02b53-726">このドキュメントは、Microsoft 製品の知的財産権に関する法的な権利をお客様に許諾するものではありません。</span><span class="sxs-lookup"><span data-stu-id="02b53-726">This document does not provide you with any legal rights to any intellectual property in any Microsoft product.</span></span> <span data-ttu-id="02b53-727">内部的な参照目的に限り、このドキュメントを複製して使用することができます。</span><span class="sxs-lookup"><span data-stu-id="02b53-727">You may copy and use this document for your internal, reference purposes.</span></span>
