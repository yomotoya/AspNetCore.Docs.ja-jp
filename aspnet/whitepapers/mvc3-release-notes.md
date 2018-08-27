---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 |Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/06/2010
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 7342b5f4a7e2327f3f3850941510a6e46ec30842
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827367"
---
<a name="aspnet-mvc-3"></a><span data-ttu-id="2ed36-102">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="2ed36-102">ASP.NET MVC 3</span></span>
====================
- [<span data-ttu-id="2ed36-103">概要</span><span class="sxs-lookup"><span data-stu-id="2ed36-103">Overview</span></span>](#overview)
- [<span data-ttu-id="2ed36-104">インストールに関する注記</span><span class="sxs-lookup"><span data-stu-id="2ed36-104">Installation Notes</span></span>](#installation-notes)
- [<span data-ttu-id="2ed36-105">ソフトウェアの要件</span><span class="sxs-lookup"><span data-stu-id="2ed36-105">Software Requirements</span></span>](#software-requirements)
- [<span data-ttu-id="2ed36-106">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="2ed36-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="2ed36-107">サポート</span><span class="sxs-lookup"><span data-stu-id="2ed36-107">Support</span></span>](#support)
- [<span data-ttu-id="2ed36-108">ASP.NET mvc、ASP.NET MVC 2 プロジェクトをアップグレードする 3 つのツールの更新します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-108">Upgrading an ASP.NET MVC 2 Project to ASP.NET MVC 3 Tools Update</span></span>](#upgrading)
- [<span data-ttu-id="2ed36-109">ASP.NET MVC 3 Tools Update (2011 年 4 月 12 日)</span><span class="sxs-lookup"><span data-stu-id="2ed36-109">ASP.NET MVC 3 Tools Update (April 12, 2011)</span></span>](#tu-changes)

    - [<span data-ttu-id="2ed36-110">"コント ローラーの追加 ダイアログ ボックスは、ビューとデータ アクセス コードを使用してコント ローラーをスキャフォールディングできますようになりました</span><span class="sxs-lookup"><span data-stu-id="2ed36-110">"Add Controller" dialog box can now scaffold controllers with views and data access code</span></span>](#tu-AddControllerDialog)
    - [<span data-ttu-id="2ed36-111">機能強化、"ASP.NET MVC 3 新しいプロジェクト ダイアログ ボックス</span><span class="sxs-lookup"><span data-stu-id="2ed36-111">Improvements to the "ASP.NET MVC 3 New Project" Dialog Box</span></span>](#tu-ImprovementsNewDialogBox)
    - [<span data-ttu-id="2ed36-112">プロジェクト テンプレートに Modernizr 1.7 が追加されました</span><span class="sxs-lookup"><span data-stu-id="2ed36-112">Project templates now include Modernizr 1.7</span></span>](#tu-Modernizr)
    - [<span data-ttu-id="2ed36-113">プロジェクト テンプレートは、更新されたバージョンの jQuery、jQuery UI、および jQuery の検証</span><span class="sxs-lookup"><span data-stu-id="2ed36-113">Project templates include updated versions of jQuery, jQuery UI, and jQuery Validation</span></span>](#tu-UpdatedJQuery)
    - [<span data-ttu-id="2ed36-114">プロジェクト テンプレートでは、NuGet プレインストール パッケージとして ADO.NET Entity Framework 4.1 が追加されました</span><span class="sxs-lookup"><span data-stu-id="2ed36-114">Project templates now include ADO.NET Entity Framework 4.1 as a pre-installed NuGet package</span></span>](#tu-EF)
    - [<span data-ttu-id="2ed36-115">プロジェクト テンプレートは、NuGet プレインストール パッケージとして JavaScript ライブラリを含める</span><span class="sxs-lookup"><span data-stu-id="2ed36-115">Project templates include JavaScript libraries as pre-installed NuGet packages</span></span>](#tu-JavaScriptLibsNuget)
    - [<span data-ttu-id="2ed36-116">既知の問題</span><span class="sxs-lookup"><span data-stu-id="2ed36-116">Known Issues</span></span>](#tu-KI)
- [<span data-ttu-id="2ed36-117">ASP.NET MVC 3 RTM (2011 年 1 月 13 日)</span><span class="sxs-lookup"><span data-stu-id="2ed36-117">ASP.NET MVC 3 RTM (January 13, 2011)</span></span>](#MVC3RTM)

    - [<span data-ttu-id="2ed36-118">変更: 1.8.7 に jQuery UI のバージョンを更新します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-118">Change: Updated the version of jQuery UI to 1.8.7</span></span>](#RTM-1)
    - [<span data-ttu-id="2ed36-119">変更: DataAnnotationsModelMetadataProvider を ModelMetadataProvider 既定値を変更します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-119">Change: Changed the default ModelMetadataProvider back to DataAnnotationsModelMetadataProvider</span></span>](#RTM-2)
    - [<span data-ttu-id="2ed36-120">修正済み: 取り消し中に空白の結果を含む Razor 式の一部を貼り付ける</span><span class="sxs-lookup"><span data-stu-id="2ed36-120">Fixed: Pasting part of a Razor expression that contains whitespace results in it being reversed</span></span>](#RTM-3)
    - [<span data-ttu-id="2ed36-121">修正済み: エディターで開かれている Razor ファイル名の変更を無効にします構文の色付けと IntelliSense</span><span class="sxs-lookup"><span data-stu-id="2ed36-121">Fixed: Renaming a Razor file that is opened in the editor disables syntax colorization and IntelliSense</span></span>](#RTM-4)
    - [<span data-ttu-id="2ed36-122">既知の問題</span><span class="sxs-lookup"><span data-stu-id="2ed36-122">Known Issues</span></span>](#RTM-KI)
    - [<span data-ttu-id="2ed36-123">重大な変更</span><span class="sxs-lookup"><span data-stu-id="2ed36-123">Breaking Changes</span></span>](#RTM-BC)
- [<span data-ttu-id="2ed36-124">ASP.NET MVC 3 Release Candidate 2 (2010 年 12 月 10 日)</span><span class="sxs-lookup"><span data-stu-id="2ed36-124">ASP.NET MVC 3 Release Candidate 2 (December 10, 2010)</span></span>](#_Toc2)

    - [<span data-ttu-id="2ed36-125">JQuery 1.4.4、jQuery 検証 1.7、jQuery UI 1.8.6y UI 1.8.6 など、プロジェクト テンプレートの変更</span><span class="sxs-lookup"><span data-stu-id="2ed36-125">Project Templates Changed to Include jQuery 1.4.4, jQuery Validation 1.7, and jQuery UI 1.8.6y UI 1.8.6</span></span>](#_Toc2_1)
    - [<span data-ttu-id="2ed36-126">追加された"AdditionalMetadataAttribute"クラス</span><span class="sxs-lookup"><span data-stu-id="2ed36-126">Added "AdditionalMetadataAttribute" Class</span></span>](#_Toc2_2)
    - [<span data-ttu-id="2ed36-127">強化されたビューのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="2ed36-127">Improved View Scaffolding</span></span>](#_Toc2_3)
    - [<span data-ttu-id="2ed36-128">追加された Html.Raw メソッド</span><span class="sxs-lookup"><span data-stu-id="2ed36-128">Added Html.Raw Method</span></span>](#_Toc2_3)
    - [<span data-ttu-id="2ed36-129">名前が変更された"Controller.ViewModel"プロパティと"ViewBag"を"View"プロパティ</span><span class="sxs-lookup"><span data-stu-id="2ed36-129">Renamed "Controller.ViewModel" Property and the "View" Property To "ViewBag"</span></span>](#_Toc2_4)
    - [<span data-ttu-id="2ed36-130">名前が変更された"ControllerSessionStateAttribute"クラス"SessionStateAttribute"を</span><span class="sxs-lookup"><span data-stu-id="2ed36-130">Renamed "ControllerSessionStateAttribute" Class to "SessionStateAttribute"</span></span>](#_Toc2_5)
    - [<span data-ttu-id="2ed36-131">「ため」に名前を変更した RemoteAttribute"Fields"プロパティ</span><span class="sxs-lookup"><span data-stu-id="2ed36-131">Renamed RemoteAttribute "Fields" Property to "AdditionalFields"</span></span>](#_Toc2_6)
    - [<span data-ttu-id="2ed36-132">"AllowHtmlAttribute"を"SkipRequestValidationAttribute"の名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-132">Renamed "SkipRequestValidationAttribute" to "AllowHtmlAttribute"</span></span>](#_Toc2_7)
    - [<span data-ttu-id="2ed36-133">最初に、役立つエラー メッセージを表示する変更された"Html.ValidationMessage"メソッド</span><span class="sxs-lookup"><span data-stu-id="2ed36-133">Changed "Html.ValidationMessage" Method to Display the First Useful Error Message</span></span>](#_Toc2_8)
    - [<span data-ttu-id="2ed36-134">固定@model空白の文書に追加の宣言</span><span class="sxs-lookup"><span data-stu-id="2ed36-134">Fixed @model Declaration to not Add Whitespace to the Document</span></span>](#_Toc2_9)
    - [<span data-ttu-id="2ed36-135">エンジン固有のファイル名をサポートするために、ビュー エンジンに追加された"FileExtensions"プロパティ</span><span class="sxs-lookup"><span data-stu-id="2ed36-135">Added "FileExtensions" Property to View Engines to Support Engine-Specific File Names</span></span>](#_Toc2_10)
    - [<span data-ttu-id="2ed36-136">固定"LabelFor"ヘルパー"For"属性の正しい値を出力するには</span><span class="sxs-lookup"><span data-stu-id="2ed36-136">Fixed "LabelFor" Helper to Emit the Correct Value for the "For" Attribute</span></span>](#_Toc2_11)
    - [<span data-ttu-id="2ed36-137">モデル バインド中に明示的な値を優先する固定"RenderAction"メソッド</span><span class="sxs-lookup"><span data-stu-id="2ed36-137">Fixed "RenderAction" Method to Give Explicit Values Precedence During Model Binding</span></span>](#_Toc2_12)
    - [<span data-ttu-id="2ed36-138">重大な変更</span><span class="sxs-lookup"><span data-stu-id="2ed36-138">Breaking Changes</span></span>](#_Toc2_BC)
    - [<span data-ttu-id="2ed36-139">既知の問題</span><span class="sxs-lookup"><span data-stu-id="2ed36-139">Known Issues</span></span>](#_Toc2_KI)
- [<span data-ttu-id="2ed36-140">ASP.NET MVC 3 リリース候補 (2010 年 11 月 9 日)</span><span class="sxs-lookup"><span data-stu-id="2ed36-140">ASP.NET MVC 3 Release Candidate (Nov 9, 2010)</span></span>](#TOC_ASP_NET_3_RC)

    - [<span data-ttu-id="2ed36-141">ASP.NET MVC 3 RC の新機能</span><span class="sxs-lookup"><span data-stu-id="2ed36-141">New Features in ASP.NET MVC 3 RC</span></span>](#_Toc276711785)
    - [<span data-ttu-id="2ed36-142">NuGet パッケージ マネージャー</span><span class="sxs-lookup"><span data-stu-id="2ed36-142">NuGet Package Manager</span></span>](#_Toc276711786)
    - <span data-ttu-id="2ed36-143">[[新しいプロジェクト] の改善 ダイアログ ボックス](#_Toc276711787)</span><span class="sxs-lookup"><span data-stu-id="2ed36-143">[Improved "New Project" Dialog Box](#_Toc276711787)</span></span>
    - [<span data-ttu-id="2ed36-144">Sessionless コント ローラー</span><span class="sxs-lookup"><span data-stu-id="2ed36-144">Sessionless Controllers</span></span>](#_Toc276711788)
    - [<span data-ttu-id="2ed36-145">新しい検証属性</span><span class="sxs-lookup"><span data-stu-id="2ed36-145">New Validation Attributes</span></span>](#_Toc276711789)
    - [<span data-ttu-id="2ed36-146">"LabelFor"と"LabelForModel"メソッドの新しいオーバー ロード</span><span class="sxs-lookup"><span data-stu-id="2ed36-146">New Overloads for "LabelFor" and "LabelForModel" Methods</span></span>](#_Toc276711790)
    - [<span data-ttu-id="2ed36-147">子アクションの出力キャッシュ</span><span class="sxs-lookup"><span data-stu-id="2ed36-147">Child Action Output Caching</span></span>](#_Toc276711791)
    - [<span data-ttu-id="2ed36-148">"ビューの追加 ダイアログ ボックスの機能強化</span><span class="sxs-lookup"><span data-stu-id="2ed36-148">"Add View" Dialog Box Improvements</span></span>](#_Toc276711792)
    - [<span data-ttu-id="2ed36-149">詳細な要求の検証</span><span class="sxs-lookup"><span data-stu-id="2ed36-149">Granular Request Validation</span></span>](#_Toc276711793)
    - [<span data-ttu-id="2ed36-150">重大な変更</span><span class="sxs-lookup"><span data-stu-id="2ed36-150">Breaking Changes</span></span>](#_Toc276711794)
    - [<span data-ttu-id="2ed36-151">既知の問題</span><span class="sxs-lookup"><span data-stu-id="2ed36-151">Known Issues</span></span>](#_Toc276711795)
- [<span data-ttu-id="2ed36-152">ASP します。MVC 3 Beta ノート (2010 年 10 月 6 日)</span><span class="sxs-lookup"><span data-stu-id="2ed36-152">ASP.MVC 3 Beta Notes (Oct 6, 2010)</span></span>](#TOC_ASP_NET_3_Beta)

    - [<span data-ttu-id="2ed36-153">ASP.NET MVC 3 のベータ版の新機能</span><span class="sxs-lookup"><span data-stu-id="2ed36-153">New Features in ASP.NET MVC 3 Beta</span></span>](#0.1__Toc274034215)
    - [<span data-ttu-id="2ed36-154">NuPack パッケージ マネージャー</span><span class="sxs-lookup"><span data-stu-id="2ed36-154">NuPack Package Manager</span></span>](#0.1__Toc274034216)
    - [<span data-ttu-id="2ed36-155">強化された新しいプロジェクト ダイアログ ボックス</span><span class="sxs-lookup"><span data-stu-id="2ed36-155">Improved New Project Dialog Box</span></span>](#0.1__Toc274034217)
    - [<span data-ttu-id="2ed36-156">厳密に指定する方法を簡略化された Razor ビューで型指定されたモデル</span><span class="sxs-lookup"><span data-stu-id="2ed36-156">Simplified Way to Specify Strongly Typed Models in Razor Views</span></span>](#0.1__Toc274034218)
    - [<span data-ttu-id="2ed36-157">新しい ASP.NET Web ページのヘルパー メソッドをサポートします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-157">Support for New ASP.NET Web Pages Helper Methods</span></span>](#0.1__Toc274034219)
    - [<span data-ttu-id="2ed36-158">追加の依存関係挿入をサポート</span><span class="sxs-lookup"><span data-stu-id="2ed36-158">Additional Dependency Injection Support</span></span>](#0.1__Toc274034220)
    - [<span data-ttu-id="2ed36-159">控えめな jQuery ベースの Ajax のサポート</span><span class="sxs-lookup"><span data-stu-id="2ed36-159">New Support for Unobtrusive jQuery-Based Ajax</span></span>](#0.1__Toc274034221)
    - [<span data-ttu-id="2ed36-160">控えめな jQuery 検証のサポート</span><span class="sxs-lookup"><span data-stu-id="2ed36-160">New Support for Unobtrusive jQuery Validation</span></span>](#0.1__Toc274034222)
    - [<span data-ttu-id="2ed36-161">クライアント検証と控えめな JavaScript の新しいアプリケーション全体のフラグ</span><span class="sxs-lookup"><span data-stu-id="2ed36-161">New Application-Wide Flags for Client Validation and Unobtrusive JavaScript</span></span>](#0.1__Toc274034223)
    - [<span data-ttu-id="2ed36-162">ビューの実行前に実行されるコードの新しいサポート</span><span class="sxs-lookup"><span data-stu-id="2ed36-162">New Support for Code that Runs Before Views Run</span></span>](#0.1__Toc274034224)
    - [<span data-ttu-id="2ed36-163">VBHTML Razor 構文のサポート</span><span class="sxs-lookup"><span data-stu-id="2ed36-163">New Support for the VBHTML Razor Syntax</span></span>](#0.1__Toc274034225)
    - [<span data-ttu-id="2ed36-164">ValidateInputAttribute をより細かく制御</span><span class="sxs-lookup"><span data-stu-id="2ed36-164">More Granular Control over ValidateInputAttribute</span></span>](#0.1__Toc274034226)
    - [<span data-ttu-id="2ed36-165">ヘルパーの匿名オブジェクトを使用して指定された HTML 属性名にアンダー スコアをハイフンに変換します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-165">Helpers Convert Underscores to Hyphens for HTML Attribute Names Specified Using Anonymous Objects</span></span>](#0.1__Toc274034227)
    - [<span data-ttu-id="2ed36-166">バグの修正</span><span class="sxs-lookup"><span data-stu-id="2ed36-166">Bug Fixes</span></span>](#0.1__Toc274034228)
    - [<span data-ttu-id="2ed36-167">重大な変更</span><span class="sxs-lookup"><span data-stu-id="2ed36-167">Breaking Changes</span></span>](#0.1__Toc274034229)
    - [<span data-ttu-id="2ed36-168">既知の問題</span><span class="sxs-lookup"><span data-stu-id="2ed36-168">Known Issues</span></span>](#0.1__Toc274034230)
- [<span data-ttu-id="2ed36-169">免責事項</span><span class="sxs-lookup"><span data-stu-id="2ed36-169">Disclaimer</span></span>](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a><span data-ttu-id="2ed36-170">概要</span><span class="sxs-lookup"><span data-stu-id="2ed36-170">Overview</span></span>

<span data-ttu-id="2ed36-171">このドキュメントでは、Visual Studio 2010 用の ASP.NET MVC 3 RTM のリリースについて説明します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-171">This document describes the release of ASP.NET MVC 3 RTM for Visual Studio 2010.</span></span> <span data-ttu-id="2ed36-172">ASP.NET MVC は、モデル-ビュー-コント ローラー (MVC) パターンを使用して、Web アプリケーションを開発するためのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="2ed36-172">ASP.NET MVC is a framework for developing Web applications that uses the Model-View-Controller (MVC) pattern.</span></span> <span data-ttu-id="2ed36-173">ASP.NET MVC 3 インストーラーには、次のコンポーネントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-173">The ASP.NET MVC 3 installer includes the following components:</span></span>

- <span data-ttu-id="2ed36-174">ASP.NET MVC 3 ランタイム コンポーネント</span><span class="sxs-lookup"><span data-stu-id="2ed36-174">ASP.NET MVC 3 runtime components</span></span>
- <span data-ttu-id="2ed36-175">ASP.NET MVC 3 Visual Studio 2010 ツール</span><span class="sxs-lookup"><span data-stu-id="2ed36-175">ASP.NET MVC 3 Visual Studio 2010 tools</span></span>
- <span data-ttu-id="2ed36-176">ASP.NET Web ページの実行時コンポーネント</span><span class="sxs-lookup"><span data-stu-id="2ed36-176">ASP.NET Web Pages run-time components</span></span>
- <span data-ttu-id="2ed36-177">ASP.NET Web ページ Visual Studio 2010 ツール</span><span class="sxs-lookup"><span data-stu-id="2ed36-177">ASP.NET Web Pages Visual Studio 2010 tools</span></span>
- <span data-ttu-id="2ed36-178">Microsoft .NET (NuGet) 用のパッケージ マネージャー</span><span class="sxs-lookup"><span data-stu-id="2ed36-178">Microsoft Package Manager for .NET (NuGet)</span></span>
- <span data-ttu-id="2ed36-179">Razor 構文のサポートを有効にする Visual Studio 2010 用の更新プログラム。</span><span class="sxs-lookup"><span data-stu-id="2ed36-179">An update for Visual Studio 2010 that enables support for Razor syntax.</span></span> <span data-ttu-id="2ed36-180">(詳細は、サポート技術情報記事 2483190 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="2ed36-180">(For details, see KnowledgeBase article 2483190.)</span></span>

<span data-ttu-id="2ed36-181">ASP.NET MVC 3 の各リリース前のバージョンのリリース ノートの完全なセットは、次の URL で ASP.NET web サイトにあります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-181">The full set of release notes for each pre-release version of ASP.NET MVC 3 can be found on the ASP.NET website at the following URL:</span></span>

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a><span data-ttu-id="2ed36-182">インストールに関する注記</span><span class="sxs-lookup"><span data-stu-id="2ed36-182">Installation Notes</span></span>

<span data-ttu-id="2ed36-183">ASP.NET MVC 3 RTM、Web Platform Installer (Web PI) を使用してをインストールするには、次のページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="2ed36-183">To install ASP.NET MVC 3 RTM using the Web Platform Installer (Web PI), visit the following page:</span></span>

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

<span data-ttu-id="2ed36-184">または、次のページから Visual Studio 2010 の ASP.NET MVC 3 rtm インストーラーをダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-184">Alternatively, you can download the installer for ASP.NET MVC 3 RTM for Visual Studio 2010 from the following page:</span></span>

https://go.microsoft.com/fwlink/?LinkID=208140

<span data-ttu-id="2ed36-185">ASP.NET MVC 3 をインストールできるし、サイド バイ サイドでを実行できる ASP.NET MVC 2 とします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-185">ASP.NET MVC 3 can be installed and can run side-by-side with ASP.NET MVC 2.</span></span>

<a id="software-requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="2ed36-186">ソフトウェア要件</span><span class="sxs-lookup"><span data-stu-id="2ed36-186">Software Requirements</span></span>

<span data-ttu-id="2ed36-187">ASP.NET MVC 3 ランタイム コンポーネントには、次のソフトウェアが必要です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-187">The ASP.NET MVC 3 run-time components require the following software:</span></span>

- <span data-ttu-id="2ed36-188">.NET framework version 4。</span><span class="sxs-lookup"><span data-stu-id="2ed36-188">.NET Framework version 4.</span></span> 

    <span data-ttu-id="2ed36-189">ASP.NET MVC 3 Visual Studio 2010 ツールには、次のソフトウェアが必要です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-189">ASP.NET MVC 3 Visual Studio 2010 tools require the following software:</span></span>
- <span data-ttu-id="2ed36-190">Visual Studio 2010 または Visual Web Developer 2010 Express。</span><span class="sxs-lookup"><span data-stu-id="2ed36-190">Visual Studio 2010 or Visual Web Developer 2010 Express.</span></span>

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="2ed36-191">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="2ed36-191">Documentation</span></span>

<span data-ttu-id="2ed36-192">ASP.NET MVC のドキュメントは、次の URL で MSDN Web サイトで入手できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-192">Documentation for ASP.NET MVC is available on the MSDN Web site at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

<span data-ttu-id="2ed36-193">チュートリアルと ASP.NET MVC に関する他の情報は、次の URL で ASP.NET Web サイトの MVC ページ入手できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-193">Tutorials and other information about ASP.NET MVC are available on the MVC page of the ASP.NET Web site at the following URL:</span></span>

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a><span data-ttu-id="2ed36-194">サポート</span><span class="sxs-lookup"><span data-stu-id="2ed36-194">Support</span></span>

<span data-ttu-id="2ed36-195">これは、完全にサポートされているリリースです。</span><span class="sxs-lookup"><span data-stu-id="2ed36-195">This is a fully supported release.</span></span> <span data-ttu-id="2ed36-196">テクニカル サポートの利用に関する情報をご覧、 [Microsoft サポート web サイト](https://support.microsoft.com/)します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-196">Information about getting technical support can be found at the [Microsoft Support website](https://support.microsoft.com/).</span></span>

<span data-ttu-id="2ed36-197">自由に、ASP.NET コミュニティのメンバーが非公式のサポートを提供することが頻繁に、ASP.NET MVC フォーラムへのこのリリースに関する質問を投稿します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-197">Also feel free to post questions about this release to the ASP.NET MVC forum, where members of the ASP.NET community are frequently able to provide informal support:</span></span>

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a><span data-ttu-id="2ed36-198">ASP.NET mvc、ASP.NET MVC 2 プロジェクトをアップグレードする 3 つのツールの更新します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-198">Upgrading an ASP.NET MVC 2 Project to ASP.NET MVC 3 Tools Update</span></span>

<span data-ttu-id="2ed36-199">ASP.NET MVC 3 は、ASP.NET MVC 2 と並行 ASP.NET MVC 3、ASP.NET MVC 2 アプリケーションをアップグレードするタイミングを選択できる柔軟性を提供する、同じコンピューターにインストールできます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-199">ASP.NET MVC 3 can be installed side by side with ASP.NET MVC 2 on the same computer, which gives you flexibility in choosing when to upgrade an ASP.NET MVC 2 application to ASP.NET MVC 3.</span></span>

<span data-ttu-id="2ed36-200">バージョン 3 に既存の ASP.NET MVC 2 アプリケーションを手動でアップグレードするには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="2ed36-200">To manually upgrade an existing ASP.NET MVC 2 application to version 3, do the following:</span></span>

1. <span data-ttu-id="2ed36-201">コンピューターに新しい空の ASP.NET MVC 3 プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-201">Create a new empty ASP.NET MVC 3 project on your computer.</span></span> <span data-ttu-id="2ed36-202">このプロジェクトは、アップグレードに必要ないくつかのファイルが格納されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-202">This project will contain some files that are required for the upgrade.</span></span>
2. <span data-ttu-id="2ed36-203">ASP.NET MVC 3 プロジェクトから ASP.NET MVC 2 プロジェクトの対応する場所に、次のファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-203">Copy the following files from the ASP.NET MVC 3 project into the corresponding location of your ASP.NET MVC 2 project.</span></span> <span data-ttu-id="2ed36-204">新しいファイル名 (jquery-1.5.1.js) に対応する jQuery ライブラリへの参照を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-204">You'll need to update any references to the jQuery library to account for the new filename ( jQuery-1.5.1.js):</span></span> 

    - <span data-ttu-id="2ed36-205">/Views/Web.config</span><span class="sxs-lookup"><span data-stu-id="2ed36-205">/Views/Web.config</span></span>
    - <span data-ttu-id="2ed36-206">/packages.config</span><span class="sxs-lookup"><span data-stu-id="2ed36-206">/packages.config</span></span>
    - <span data-ttu-id="2ed36-207">/scripts/\*.js</span><span class="sxs-lookup"><span data-stu-id="2ed36-207">/scripts/\*.js</span></span>
    - <span data-ttu-id="2ed36-208">/Content/themes/\*.\*</span><span class="sxs-lookup"><span data-stu-id="2ed36-208">/Content/themes/\*.\*</span></span>
3. <span data-ttu-id="2ed36-209">コピー、*パッケージ*ソリューションの .sln ファイルがあるディレクトリでは、ソリューションのルートに空の ASP.NET MVC 3 プロジェクト ソリューションのルート フォルダー。</span><span class="sxs-lookup"><span data-stu-id="2ed36-209">Copy the *packages* folder in the root of the empty ASP.NET MVC 3 project solution into the root of your solution, which is in the directory where the solution's .sln file is located.</span></span>
4. <span data-ttu-id="2ed36-210">ASP.NET MVC 2 プロジェクトに区分が含まれている場合、/Views/Web.config ファイルのコピー、*ビュー*各領域のフォルダー。</span><span class="sxs-lookup"><span data-stu-id="2ed36-210">If your ASP.NET MVC 2 project contains any areas, copy the /Views/Web.config file to the *Views* folder of each area.</span></span>
5. <span data-ttu-id="2ed36-211">ASP.NET MVC 2 プロジェクトの両方の Web.config ファイルでグローバルに検索し、ASP.NET MVC のバージョンを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-211">In both Web.config files in the ASP.NET MVC 2 project, globally search and replace the ASP.NET MVC version.</span></span> <span data-ttu-id="2ed36-212">次を検索します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-212">Find the following:</span></span> 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    <span data-ttu-id="2ed36-213">これは、次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-213">Replace it with the following:</span></span>

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. <span data-ttu-id="2ed36-214">ソリューション エクスプ ローラーへの参照を削除*System.Web.Mvc* (を指す、DLL からバージョン 2) への参照を追加し、 *System.Web.Mvc* (v3.0.0.0 へ)。</span><span class="sxs-lookup"><span data-stu-id="2ed36-214">In Solution Explorer, delete the reference to *System.Web.Mvc* (which points to the DLL from version 2), then add a reference to *System.Web.Mvc* (v3.0.0.0).</span></span>
7. <span data-ttu-id="2ed36-215">System.Web.WebPages.dll および System.Web.Helpers.dll への参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-215">Add a reference to System.Web.WebPages.dll and System.Web.Helpers.dll.</span></span> <span data-ttu-id="2ed36-216">これらのアセンブリは、次のフォルダーに配置されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-216">These assemblies are located in the following folders:</span></span> 

    - <span data-ttu-id="2ed36-217">%ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies</span><span class="sxs-lookup"><span data-stu-id="2ed36-217">%ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies</span></span>
    - <span data-ttu-id="2ed36-218">%ProgramFiles%\Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies</span><span class="sxs-lookup"><span data-stu-id="2ed36-218">%ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies</span></span>
8. <span data-ttu-id="2ed36-219">ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-219">In Solution Explorer, right-click the project name and select Unload Project.</span></span> <span data-ttu-id="2ed36-220">プロジェクト名をもう一度右クリックし、編集*ProjectName*.csproj します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-220">Then right-click the project name again and select Edit *ProjectName*.csproj.</span></span>
9. <span data-ttu-id="2ed36-221">検索、 *ProjectTypeGuids*要素と置換 {F85E285D-A4E0-4152-9332-AB1D724D3325} {E53F8FEA-EAE0-44A6-8774-FFD645390401}。</span><span class="sxs-lookup"><span data-stu-id="2ed36-221">Locate the *ProjectTypeGuids* element and replace {F85E285D-A4E0-4152-9332-AB1D724D3325} with {E53F8FEA-EAE0-44A6-8774-FFD645390401}.</span></span>
10. <span data-ttu-id="2ed36-222">変更を保存し、プロジェクトを右クリックし、プロジェクトの再読み込みを選択します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-222">Save the changes, right-click the project, and then select Reload Project.</span></span>
11. <span data-ttu-id="2ed36-223">アプリケーションのルートの Web.config ファイルに次の設定を追加、*アセンブリ*セクション。</span><span class="sxs-lookup"><span data-stu-id="2ed36-223">In the application's root Web.config file, add the following settings to the *assemblies* section.</span></span> 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. <span data-ttu-id="2ed36-224">プロジェクトでは、ASP.NET MVC 2 を使用してコンパイルされたすべてのサード パーティ製ライブラリを参照する場合は、強調表示されている次のコードを追加*bindingRedirect*要素下のアプリケーション ルートの Web.config ファイルを*構成*セクション。</span><span class="sxs-lookup"><span data-stu-id="2ed36-224">If the project references any third-party libraries that are compiled using ASP.NET MVC 2, add the following highlighted *bindingRedirect* element to the Web.config file in the application root under the *configuration* section:</span></span> 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a><span data-ttu-id="2ed36-225">ASP.NET MVC 3 での変更ツールを更新します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-225">Changes in ASP.NET MVC 3 Tools Update</span></span>

<span data-ttu-id="2ed36-226">このセクションでは、ASP.NET MVC 3 RTM リリース以降、ASP.NET MVC 3 Tools Update リリースで行われた変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-226">This section describes changes made in the ASP.NET MVC 3 Tools Update release since the ASP.NET MVC 3 RTM release.</span></span>

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a><span data-ttu-id="2ed36-227">"コント ローラーの追加 ダイアログ ボックスは、ビューとデータ アクセス コードを使用してコント ローラーをスキャフォールディングできますようになりました</span><span class="sxs-lookup"><span data-stu-id="2ed36-227">"Add Controller" dialog box can now scaffold controllers with views and data access code</span></span>

<span data-ttu-id="2ed36-228">スキャフォールディングをコント ローラーとビュー、アプリケーションをすばやく生成できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-228">Scaffolding is a way of quickly generating a controller and views for your application.</span></span> <span data-ttu-id="2ed36-229">コードが生成されたら、プロジェクトの要件に合わせて編集できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-229">After the code has been generated, you can edit it to meet your project's requirements.</span></span>

<span data-ttu-id="2ed36-230">起動する、*コント ローラーの追加*ASP.NET MVC 3、ダイアログ ボックスを右クリックし、*コント ローラー*フォルダー*ソリューション エクスプ ローラー*、 をクリックして*追加*、 をクリックし、*コント ローラー*します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-230">To launch the *Add Controller* dialog box in ASP.NET MVC 3, right-click the *Controllers* folder in *Solution Explorer*, click *Add*, and then click *Controller*.</span></span> <span data-ttu-id="2ed36-231">ダイアログ ボックスは、スキャフォールディングの追加オプションを提供する拡張されています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-231">The dialog box has been enhanced to offer additional scaffolding options.</span></span>

![](mvc3-release-notes/_static/image1.png)

<span data-ttu-id="2ed36-232">次の 3 つスキャフォールディング テンプレートは使用可能な既定です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-232">There are three scaffolding templates available by default.</span></span>

#### <a name="empty-controller"></a><span data-ttu-id="2ed36-233">空のコント ローラー</span><span class="sxs-lookup"><span data-stu-id="2ed36-233">Empty Controller</span></span>

<span data-ttu-id="2ed36-234">このテンプレートには、空のコント ローラー ファイルが生成されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-234">This template generates an empty controller file.</span></span> <span data-ttu-id="2ed36-235">このテンプレートはしないことに相当*作成、編集、詳細のアクションを追加、削除、シナリオ*ASP.NET MVC の以前のバージョン。</span><span class="sxs-lookup"><span data-stu-id="2ed36-235">This template is equivalent to not checking *Add actions for create, edit, details, delete scenarios* in previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="2ed36-236">これを選択すると、さらにオプションの使用はありません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-236">If you choose this, no further options are available.</span></span>

#### <a name="controller-with-empty-readwrite-actions"></a><span data-ttu-id="2ed36-237">空の読み取り/書き込みアクションがコント ローラー</span><span class="sxs-lookup"><span data-stu-id="2ed36-237">Controller with empty read/write actions</span></span>

<span data-ttu-id="2ed36-238">このテンプレートは、メソッドの実装コードではなく、すべての必要なアクション メソッドを含むコント ローラー ファイルを生成します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-238">This template generates a controller file that has all the required action methods but no implementation code in the methods.</span></span> <span data-ttu-id="2ed36-239">このテンプレートはことに相当*作成、編集、詳細のアクションを追加、削除、シナリオ*ASP.NET MVC の以前のバージョン。</span><span class="sxs-lookup"><span data-stu-id="2ed36-239">This template is equivalent to checking *Add actions for create, edit, details, delete scenarios* in previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="2ed36-240">これを選択すると、さらにオプションの使用はありません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-240">If you choose this, no further options are available.</span></span>

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a><span data-ttu-id="2ed36-241">読み取り/書き込みアクションと Entity Framework を使用して、ビュー コント ローラー</span><span class="sxs-lookup"><span data-stu-id="2ed36-241">Controller with read/write actions and views, using Entity Framework</span></span>

<span data-ttu-id="2ed36-242">このテンプレートでは、作業データ入力ユーザー インターフェイスをすばやく作成することができます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-242">This template enables you to quickly create a working data-entry user interface.</span></span> <span data-ttu-id="2ed36-243">一般的な要件と、次のシナリオの範囲を処理するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-243">It generates code that handles a range of common requirements and scenarios, such as the following:</span></span>

- <span data-ttu-id="2ed36-244">*データ アクセス*します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-244">*Data access*.</span></span> <span data-ttu-id="2ed36-245">生成されたコードは、データベース内でエンティティを読み書きします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-245">The generated code reads and writes entities in a database.</span></span> <span data-ttu-id="2ed36-246">既存のデータ コンテキスト クラスを選択した場合、または新しい生成テンプレートを使用する場合、Entity Framework Code First アプローチで動作*DbContext*クラス。</span><span class="sxs-lookup"><span data-stu-id="2ed36-246">It works with the Entity Framework Code First approach if you choose an existing data context class or if you let the template generate a new *DbContext* class.</span></span> <span data-ttu-id="2ed36-247">既存の選択した場合は、Entity Framework Database First または Model First アプローチでも動作*ObjectContext*クラス。</span><span class="sxs-lookup"><span data-stu-id="2ed36-247">It also works with the Entity Framework Database First or Model First approach if you choose an existing *ObjectContext* class.</span></span>
- <span data-ttu-id="2ed36-248">*検証*です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-248">*Validation*.</span></span> <span data-ttu-id="2ed36-249">生成されたコードは、モデル クラスで宣言されている規則に従ってフォーム送信が検証できるように、ASP.NET MVC のモデル バインドとメタデータ機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-249">The generated code uses ASP.NET MVC model binding and metadata features so that form submissions are validated according to rules declared on your model class.</span></span> <span data-ttu-id="2ed36-250">など、組み込みの検証の規則が含まれます、*必要*と*StringLength*属性、およびカスタム検証規則。</span><span class="sxs-lookup"><span data-stu-id="2ed36-250">This includes built-in validation rules, such as the *Required* and *StringLength* attributes, and custom validation rules.</span></span>
- <span data-ttu-id="2ed36-251">*一対多リレーションシップ*します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-251">*One-to-many relationships*.</span></span> <span data-ttu-id="2ed36-252">モデル クラス間に一対多外部キー リレーションシップを定義する場合、生成されたコードには関連エンティティを選択するためのドロップダウン リストが生成されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-252">If you define one-to-many foreign-key relationships between your model classes, the generated code will produce drop-down lists for selecting related entities.</span></span> <span data-ttu-id="2ed36-253">たとえば、次に Entity Framework Code First の規則に従うモデル クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-253">For example, you might define the following model classes following Entity Framework Code First conventions:</span></span> 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    <span data-ttu-id="2ed36-254">コント ローラーをスキャフォールディングすると、*製品*クラス、そのビューにより、ユーザーを選択する、*カテゴリ*オブジェクトごとに*製品*インスタンス。</span><span class="sxs-lookup"><span data-stu-id="2ed36-254">When you then scaffold a controller for the *Product* class, its views will allow users to choose a *Category* object for each *Product* instance.</span></span>

    <span data-ttu-id="2ed36-255">このテンプレートの追加のオプションを使用して、*コント ローラーの追加* ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="2ed36-255">This template enables additional options in the *Add Controller* dialog box.</span></span> <span data-ttu-id="2ed36-256">*モデル クラス*ソリューションを作成または編集できるユーザー データの種類を決定するはどのモデル クラスを選択できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-256">For *Model class*, you can choose any model class in your solution, which determines the type of data that users will be able to create or edit:</span></span>
- <span data-ttu-id="2ed36-257">Entity Framework Code First を使用する場合は、どのモデル クラスを選択できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-257">If you want to use Entity Framework Code First, you can choose any model class.</span></span>
- <span data-ttu-id="2ed36-258">Entity Framework Database First または Entity Framework Model First を使用している場合は、概念モデルで定義されているエンティティ クラスを選択することを確認します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-258">If you are using Entity Framework Database First or Entity Framework Model First, be sure to choose an entity class defined in your conceptual model.</span></span>

<span data-ttu-id="2ed36-259">*データ コンテキスト クラス*、これらの選択を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-259">For *Data Context class*, you can make these choices:</span></span>

- <span data-ttu-id="2ed36-260">Code First を使用して、既存のデータ コンテキスト クラスではありませんしたい場合 * * 新しいデータ コンテキスト * *。</span><span class="sxs-lookup"><span data-stu-id="2ed36-260">If you want to use Code First and have no existing data context class, choose **New data context **.</span></span> <span data-ttu-id="2ed36-261">データ コンテキスト クラスが、しが生成されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-261">A data context class will then be generated for you.</span></span>
- <span data-ttu-id="2ed36-262">既存のデータ コンテキスト クラスを Code First を使用する場合は、ここで選択します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-262">If you want to use Code First and have an existing data context class, choose it here.</span></span> <span data-ttu-id="2ed36-263">選択したモデル クラスを永続化が更新されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-263">It will be updated to persist the model class you have selected.</span></span>
- <span data-ttu-id="2ed36-264">Database First または Model First を使用している場合は、ここで、オブジェクト コンテキスト クラスを選択します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-264">If you are using Database First or Model First, choose your object context class here.</span></span>

<span data-ttu-id="2ed36-265">ビューの場合、使用するビュー エンジンを選択または任意のビューをスキャフォールディングしたくない場合、[なし] を選択します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-265">For Views, choose the view engine you want to use, or choose None if you don't want to scaffold any views.</span></span>

<span data-ttu-id="2ed36-266">高度なオプションを選択するさらに、生成されたビューのオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-266">You can select Advanced Optionsto specify further options for the generated views.</span></span> <span data-ttu-id="2ed36-267">たとえば、レイアウトまたはマスター ページを使用を選択できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-267">For example, you can choose the layout or master page to use.</span></span>

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a><span data-ttu-id="2ed36-268">機能強化、"ASP.NET MVC 3 新しいプロジェクト ダイアログ ボックス</span><span class="sxs-lookup"><span data-stu-id="2ed36-268">Improvements to the "ASP.NET MVC 3 New Project" Dialog Box</span></span>

<span data-ttu-id="2ed36-269">新しい ASP.NET MVC 3 プロジェクトの作成に使用する ダイアログ ボックスには、次に示すように複数の機能強化が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-269">The dialog box you use to create new ASP.NET MVC 3 projects includes multiple improvements, as listed below.</span></span>

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a><span data-ttu-id="2ed36-270">新しい「イントラネット プロジェクト」テンプレート</span><span class="sxs-lookup"><span data-stu-id="2ed36-270">New "Intranet Project" Template</span></span>

<span data-ttu-id="2ed36-271">プロジェクト テンプレートの一覧には、新しいイントラネット アプリケーション テンプレートが用意されています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-271">The Project Template list includes a new Intranet Application template.</span></span> <span data-ttu-id="2ed36-272">このテンプレートには、フォーム認証ではなく Windows 認証を使用して web アプリケーションを構築するための設定が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-272">This template contains settings for building a web application using Windows authentication instead of forms authentication.</span></span> <span data-ttu-id="2ed36-273">イントラネット アプリケーションには、プロジェクトのテンプレートにカプセル化することはできません、いくつかの IIS 設定が必要であるため、テンプレートには、プロジェクト テンプレートを IIS で使用する方法についての readme ファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-273">Because an intranet application requires some IIS settings that can't be encapsulated in a project template, the template includes a readme file with instructions for how to make the project template work in IIS.</span></span> <span data-ttu-id="2ed36-274">ドキュメントを新しいイントラネット アプリケーション テンプレートは、次の URL で MSDN web サイトで使用できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-274">Documentation for the a new Intranet Application template is available on the MSDN website at the following URL:</span></span>

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a><span data-ttu-id="2ed36-275">プロジェクト テンプレートが HTML5 に対応</span><span class="sxs-lookup"><span data-stu-id="2ed36-275">Project templates are now HTML5 enabled</span></span>

<span data-ttu-id="2ed36-276">新しいプロジェクト ダイアログ ボックスにプロジェクト テンプレートは、HTML5 固有の機能を追加するオプションが追加されました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-276">The new-project dialog box now contains an option to add HTML5-specific features to the project templates.</span></span> <span data-ttu-id="2ed36-277">オプションをオンにすると、新たに HTML5 が含まれているビューを生成する`<header>`、 `<footer>`、および`<navigation>`要素。</span><span class="sxs-lookup"><span data-stu-id="2ed36-277">Selecting the option causes views to be generated that contain the new HTML5 `<header>`, `<footer>`, and `<navigation>` elements.</span></span>

<span data-ttu-id="2ed36-278">以前のバージョンのブラウザーは、HTML5 固有のタグをサポートしないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2ed36-278">Note that earlier versions of browsers do not support HTML5-specific tags.</span></span> <span data-ttu-id="2ed36-279">この制限に対処するため、HTML5 プロジェクト テンプレートに Modernizr ライブラリへの参照が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-279">To address this limitation, the HTML5 project templates include a reference to the Modernizr library.</span></span> <span data-ttu-id="2ed36-280">(詳しくは、次のセクションを参照してください)。</span><span class="sxs-lookup"><span data-stu-id="2ed36-280">(See the next section.)</span></span>

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a><span data-ttu-id="2ed36-281">プロジェクト テンプレートに Modernizr 1.7 が追加されました</span><span class="sxs-lookup"><span data-stu-id="2ed36-281">Project templates now include Modernizr 1.7</span></span>

<span data-ttu-id="2ed36-282">Modernizr は、これらの機能をサポートしていないブラウザーで、CSS 3 および HTML5 のサポートを有効にする JavaScript ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="2ed36-282">Modernizr is a JavaScript library that enables support for CSS 3 and HTML5 in browsers that do not yet support these features.</span></span> <span data-ttu-id="2ed36-283">このライブラリは、ASP.NET MVC 3 プロジェクト用のテンプレートに事前にインストールされている NuGet パッケージとして含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-283">This library is included as a pre-installed NuGet package in templates for ASP.NET MVC 3 projects.</span></span> <span data-ttu-id="2ed36-284">Modernizr の詳細については、次を参照してください。 [ http://www.modernizr.com/](http://www.modernizr.com/)します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-284">For more information about Modernizr, see [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a><span data-ttu-id="2ed36-285">プロジェクト テンプレートは、更新されたバージョンの jQuery、jQuery UI、および jQuery の検証</span><span class="sxs-lookup"><span data-stu-id="2ed36-285">Project templates include updated versions of jQuery, jQuery UI, and jQuery Validation</span></span>

<span data-ttu-id="2ed36-286">プロジェクト テンプレートには、次のバージョンの jQuery スクリプトが追加されました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-286">The project templates now include the following versions of the jQuery scripts:</span></span>

- <span data-ttu-id="2ed36-287">jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="2ed36-287">jQuery 1.5.1</span></span>
- <span data-ttu-id="2ed36-288">jQuery 検証 1.8</span><span class="sxs-lookup"><span data-stu-id="2ed36-288">jQuery Validation 1.8</span></span>
- <span data-ttu-id="2ed36-289">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="2ed36-289">jQuery UI 1.8.11</span></span>

<span data-ttu-id="2ed36-290">これらのライブラリは、NuGet プレインストール パッケージとして含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-290">These libraries are included as pre-installed NuGet packages.</span></span>

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a><span data-ttu-id="2ed36-291">プロジェクト テンプレートでは、NuGet プレインストール パッケージとして ADO.NET Entity Framework 4.1 が追加されました</span><span class="sxs-lookup"><span data-stu-id="2ed36-291">Project templates now include ADO.NET Entity Framework 4.1 as a pre-installed NuGet package</span></span>

<span data-ttu-id="2ed36-292">ADO.NET の Entity Framework 4.1 には、Code First 機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-292">The ADO.NET Entity Framework 4.1 includes the Code First feature.</span></span> <span data-ttu-id="2ed36-293">コードは、既存の Database First と Model First パターンの代替を提供する ADO.NET Entity Framework の新しい開発パターンを最初です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-293">Code First is a new development pattern for the ADO.NET Entity Framework that provides an alternative to the existing Database First and Model First patterns.</span></span>

<span data-ttu-id="2ed36-294">コードはまず、Visual Basic または c# で記述された POCO クラス ("plain old CLR object") を使用して、モデルの定義をフォーカスしています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-294">Code First is focused around defining your model using POCO classes ("plain old CLR objects") written in Visual Basic or C#.</span></span> <span data-ttu-id="2ed36-295">これらのクラスは、既存のデータベースにマップすることができますし、またはデータベース スキーマの生成に使用します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-295">These classes can then be mapped to an existing database or be used to generate a database schema.</span></span> <span data-ttu-id="2ed36-296">使用して追加の構成を指定する*DataAnnotations*属性または fluent Api を使用します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-296">Additional configuration can be supplied using *DataAnnotations* attributes or using fluent APIs.</span></span>

<span data-ttu-id="2ed36-297">コード Firstwith ASP.NET MVC を使用するためのドキュメントは、次の Url で ASP.NET web サイトで使用できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-297">Documentation for using Code Firstwith ASP.NET MVC is available on the ASP.NET website at the following URLs:</span></span>

<span data-ttu-id="2ed36-298">[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="2ed36-298">[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)</span></span>

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a><span data-ttu-id="2ed36-299">プロジェクト テンプレートは、NuGet プレインストール パッケージとして JavaScript ライブラリを含める</span><span class="sxs-lookup"><span data-stu-id="2ed36-299">Project templates include JavaScript libraries as pre-installed NuGet packages</span></span>

<span data-ttu-id="2ed36-300">それらをインストールすることで (たとえば、Modernizr ライブラリ) に記載されている JavaScript ファイルがプロジェクトに含まれています新しい ASP.NET MVC 3 プロジェクトを作成するときに、プロジェクト テンプレートで、Scripts フォルダーにスクリプトを直接追加する代わりに NuGet を使用します。内容。</span><span class="sxs-lookup"><span data-stu-id="2ed36-300">When you create a new ASP.NET MVC 3 project, the project includes the JavaScript files mentioned previously (for example, the Modernizr library) by installing them using NuGet instead of directly adding the scripts to the Scripts folder in the project template contents.</span></span> <span data-ttu-id="2ed36-301">これにより、NuGet を使用して、スクリプトの新しいバージョンがリリースされたときに、最新バージョンに、スクリプトを更新することができます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-301">This enables you to use NuGet to update the scripts to the latest version when new versions of the scripts are released.</span></span>

<span data-ttu-id="2ed36-302">たとえば、jQuery の新しいリリースの頻度を指定するには、プロジェクト テンプレートに含まれる jQuery のバージョンがある時点で期限切れ。</span><span class="sxs-lookup"><span data-stu-id="2ed36-302">For example, given the frequency of new jQuery releases, the version of jQuery included in the project template will at some point be out of date.</span></span> <span data-ttu-id="2ed36-303">ただし、jQuery はインストールされている NuGet パッケージとして含まれるため、通知されます NuGet ダイアログ ボックスで、jQuery の新しいバージョンが利用可能な場合。</span><span class="sxs-lookup"><span data-stu-id="2ed36-303">However, because jQuery is included as an installed NuGet package, you will be notified in the NuGet dialog box when newer versions of jQuery are available.</span></span>

<span data-ttu-id="2ed36-304">JQuery には、ファイル名にバージョン番号が含まれているため、jQuery を最新バージョンに更新も更新が必要です、`<script>`新しいファイル名を使用する jQuery ファイルを参照するタグ。</span><span class="sxs-lookup"><span data-stu-id="2ed36-304">Because jQuery includes the version number in the file name, updating jQuery to the latest version also requires updating the `<script>` tag that references the jQuery file to use the new file name.</span></span> <span data-ttu-id="2ed36-305">その他のスクリプトが含まれるライブラリでは、スクリプト名 でバージョン番号は含まれないので、最新バージョンにより簡単に更新することができます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-305">Other included script libraries do not include the version number in the script name, so they can be more easily updated to their latest versions.</span></span>

<a id="tu-KI"></a>
## <a name="known-issues"></a><span data-ttu-id="2ed36-306">既知の問題</span><span class="sxs-lookup"><span data-stu-id="2ed36-306">Known Issues</span></span>

- <span data-ttu-id="2ed36-307">場合によっては、インストール、エラー メッセージ「インストールに失敗しましたエラー コード (0x80070643)」で失敗します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-307">In some cases, installation may fail with the error message "Installation failed with error code (0x80070643)".</span></span> <span data-ttu-id="2ed36-308">この問題を回避する方法については、次を参照してください。[サポート技術情報の記事 2531566](https://support.microsoft.com/kb/2531566)します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-308">For information about how to work around this issue, see [KnowledgeBase article 2531566](https://support.microsoft.com/kb/2531566).</span></span>
- <span data-ttu-id="2ed36-309">コント ローラーを追加するためのスキャフォールディングでは、Entity Framework 内でエンティティ継承サポートを利用するエンティティはスキャフォールディングされません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-309">The scaffolding for adding a controller does not scaffold entities that take advantage of entity inheritance support within Entity Framework.</span></span> <span data-ttu-id="2ed36-310">たとえば、ベースを指定*人*クラスによって継承される、*学生*クラスをスキャフォールディング、*学生*クラスと、生成されたコード コンパイルされていません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-310">For example, given a base *Person* class that's inherited by a *Student* class, scaffolding the *Student* class will result in generated code that does not compile.</span></span>
- <span data-ttu-id="2ed36-311">ソリューション フォルダー内で新しい ASP.NET MVC 3 プロジェクトを作成するには、原因、 *NullReferenceException*エラー。</span><span class="sxs-lookup"><span data-stu-id="2ed36-311">Creating a new ASP.NET MVC 3 project within a solution folder causes a *NullReferenceException* error.</span></span> <span data-ttu-id="2ed36-312">回避策では、ソリューションのルートに ASP.NET MVC 3 プロジェクトを作成し、ソリューション フォルダーに移動しています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-312">The workaround is to create the ASP.NET MVC 3 project in the root of the solution and then move it into the solution folder.</span></span>
- <span data-ttu-id="2ed36-313">Razor 構文の IntelliSense では、ReSharper がインストールされている場合は機能しません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-313">IntelliSense for Razor syntax does not work when ReSharper is installed.</span></span> <span data-ttu-id="2ed36-314">ReSharper がインストールされているあり ASP.NET MVC 3 での Razor IntelliSense サポートを利用する場合、エントリを参照してください。 [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)併用する方法が説明されています Hadi Hariri のブログ、します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-314">If you have ReSharper installed and want to take advantage of the Razor IntelliSense support in ASP.NET MVC 3, see the entry [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) on Hadi Hariri's blog, which discusses ways to use them together today.</span></span>
- <span data-ttu-id="2ed36-315">インストール中に、使用許諾契約書への同意 ダイアログ ボックスのものよりも小さなウィンドウで、ライセンス条項が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-315">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>
- <span data-ttu-id="2ed36-316">Razor ビューを編集するときに (.cshtml または *。vbhtml*ファイル)、ビュー。</span><span class="sxs-lookup"><span data-stu-id="2ed36-316">When you are editing a Razor view (.cshtml or .*vbhtml* file), views.</span></span> <span data-ttu-id="2ed36-317">ASP.NET MVC 3 では、Razor ビューのスニペットは含まれません.ASP.NET MVC 用のコード スニペットを aspxselecting スニペットが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-317">ASP.NET MVC 3 does not include any snippets for Razor views..aspxselecting a code snippet for ASP.NET MVC will show snippets for</span></span>
- <span data-ttu-id="2ed36-318">ASP.NET MVC 3 for Visual Web Developer Express で Visual Studio がインストールされていない、コンピューターにインストールし、後で Visual Studio をインストールして場合に、ASP.NET MVC 3 を再インストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-318">If you install ASP.NET MVC 3 for Visual Web Developer Express on a computer where Visual Studio is not installed, and then later install Visual Studio, you must reinstall ASP.NET MVC 3.</span></span> <span data-ttu-id="2ed36-319">Visual Studio と Visual Web Developer Express は、ASP.NET MVC 3 インストーラーによってアップグレードしたコンポーネントを共有します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-319">Visual Studio and Visual Web Developer Express share components that are upgraded by the ASP.NET MVC 3 installer.</span></span> <span data-ttu-id="2ed36-320">Visual Web Developer Express があるし、後で Visual Web Developer Express インストール コンピューターに Visual Studio for ASP.NET MVC 3 をインストールする場合、同じ問題が適用されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-320">The same issue applies if you install ASP.NET MVC 3 for Visual Studio on a computer that does not have Visual Web Developer Express and then later install Visual Web Developer Express.</span></span>

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a><span data-ttu-id="2ed36-321">ASP.NET MVC 3 RTM での変更</span><span class="sxs-lookup"><span data-stu-id="2ed36-321">Changes in ASP.NET MVC 3 RTM</span></span>

<span data-ttu-id="2ed36-322">このセクションでは、変更と RC2 リリース後、ASP.NET MVC 3 RTM リリースで行われたバグ修正について説明します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-322">This section describes changes and bug fixes made in the ASP.NET MVC 3 RTM release since the RC2 release.</span></span>

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a><span data-ttu-id="2ed36-323">変更: 1.8.7 に jQuery UI のバージョンを更新します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-323">Change: Updated the version of jQuery UI to 1.8.7</span></span>

<span data-ttu-id="2ed36-324">Visual Studio の ASP.NET MVC プロジェクト テンプレートは、jQuery UI ライブラリの最新バージョンを含むように更新されました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-324">The ASP.NET MVC project templates for Visual Studio were updated to include the latest version of the jQuery UI library.</span></span> <span data-ttu-id="2ed36-325">テンプレートには、jQuery、関連付けられている CSS とイメージ ファイルなどの UI に必要なリソース ファイルの最小セットも含まれます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-325">The templates also include the minimal set of resource files required by jQuery UI, such as the associated CSS and image files.</span></span>

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a><span data-ttu-id="2ed36-326">変更: DataAnnotationsModelMetadataProvider を ModelMetadataProvider 既定値を変更します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-326">Change: Changed the default ModelMetadataProvider back to DataAnnotationsModelMetadataProvider</span></span>

<span data-ttu-id="2ed36-327">導入された ASP.NET MVC 3 の RC2 リリースを*CachedDataAnnotationsMetadataProvider*クラスに提供される既存の上にキャッシュ*DataAnnotationsModelMetadataProvider*クラスとして、パフォーマンスの向上。</span><span class="sxs-lookup"><span data-stu-id="2ed36-327">The RC2 release of ASP.NET MVC 3 introduced a *CachedDataAnnotationsMetadataProvider* class that provided caching on top of the existing *DataAnnotationsModelMetadataProvider* class as a performance improvement.</span></span> <span data-ttu-id="2ed36-328">変更を元に戻すしで利用可能な計画の MVC プロジェクトに移動するため、この実装でに報告されたいくつかのバグのただし、 [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-328">However, some bugs were reported with this implementation, so the change has been reverted and moved into the MVC Futures project, which is available at [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).</span></span>

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a><span data-ttu-id="2ed36-329">修正済み: 取り消し中に空白の結果を含む Razor 式の一部を貼り付ける</span><span class="sxs-lookup"><span data-stu-id="2ed36-329">Fixed: Pasting part of a Razor expression that contains whitespace results in it being reversed</span></span>

<span data-ttu-id="2ed36-330">ASP.NET MVC 3 のプレリリース バージョンは、Razor ファイルに空白を含む Razor 式の一部を貼り付けると、結果の式が逆になります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-330">In pre-release versions of ASP.NET MVC 3, when you paste a part of a Razor expression that contains whitespace into a Razor file, the resulting expression is reversed.</span></span> <span data-ttu-id="2ed36-331">たとえば、次の Razor コード ブロックがあるとします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-331">For example, consider the following Razor code block:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

<span data-ttu-id="2ed36-332">最初のメソッドでテキスト"最初 param"を選択し、2 番目のメソッドに引数として貼り付けます場合、結果はとおりです。</span><span class="sxs-lookup"><span data-stu-id="2ed36-332">If you select the text "first param" in the first method and paste it as an argument into the second method, the result is as follows:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

<span data-ttu-id="2ed36-333">正しい動作は、貼り付け操作は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-333">The correct behavior is that the paste operation should result in the following:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

<span data-ttu-id="2ed36-334">式が正しく貼り付け操作中に保持されるように RTM リリースでこの問題は修正されています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-334">This issue has been fixed in the RTM release so that the expression is correctly preserved during the paste operation.</span></span>

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a><span data-ttu-id="2ed36-335">修正済み: エディターで開かれている Razor ファイル名の変更を無効にします構文の色付けと IntelliSense</span><span class="sxs-lookup"><span data-stu-id="2ed36-335">Fixed: Renaming a Razor file that is opened in the editor disables syntax colorization and IntelliSense</span></span>

<span data-ttu-id="2ed36-336">エディター ウィンドウで、ファイルを開いたときに、ソリューション エクスプ ローラーを使用して Razor ファイルの名前を変更すると、構文の強調表示、IntelliSense ファイルの動作を停止します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-336">Renaming a Razor file using Solution Explorer while the file is opened in the editor window causes syntax highlighting and IntelliSense to stop working for that file.</span></span> <span data-ttu-id="2ed36-337">強調表示するためにこれを修正されていて、IntelliSense は、名前の変更後も保持します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-337">This has been fixed so that highlighting and IntelliSense are maintained after a rename.</span></span>

<a id="RTM-KI"></a>
## <a name="known-issues"></a><span data-ttu-id="2ed36-338">既知の問題</span><span class="sxs-lookup"><span data-stu-id="2ed36-338">Known Issues</span></span>

- <span data-ttu-id="2ed36-339">場合は、NuGet パッケージ マネージャー コンソールが開いているときに Visual Studio 2010 SP1 Beta を閉じると、Visual Studio がクラッシュして再起動しようとしています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-339">If you close Visual Studio 2010 SP1 Beta while the NuGet Package Manager Console is open, Visual Studio crashes and attempts to restart.</span></span> <span data-ttu-id="2ed36-340">これは、問題を修正する予定の Visual Studio 2010 SP1 の RTM リリースでします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-340">This will be fixed in the RTM release of Visual Studio 2010 SP1.</span></span>
- <span data-ttu-id="2ed36-341">ASP.NET MVC 3 インストーラーでは、NuGet パッケージ マネージャーの初期のバージョンをインストールすることのみです。</span><span class="sxs-lookup"><span data-stu-id="2ed36-341">The ASP.NET MVC 3 installer is only able to install an initial version of the NuGet package manager.</span></span> <span data-ttu-id="2ed36-342">最初のバージョンをインストールした後、NuGet はインストールすることができ、Visual Studio 拡張機能マネージャーを使用して更新します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-342">After you have installed the initial version, NuGet can be installed and updated using Visual Studio Extension Manager.</span></span> <span data-ttu-id="2ed36-343">NuGet がインストールされて既にある場合は、NuGet の最新バージョンに更新するには、Visual Studio 拡張機能ギャラリーに移動します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-343">If you already have NuGet installed, go to the Visual Studio Extension Gallery to update to the latest version of NuGet.</span></span>
- <span data-ttu-id="2ed36-344">ソリューション フォルダー内で新しい ASP.NET MVC 3 プロジェクトを作成するには、原因、 *NullReferenceException*エラー。</span><span class="sxs-lookup"><span data-stu-id="2ed36-344">Creating a new ASP.NET MVC 3 project within a solution folder causes a *NullReferenceException* error.</span></span> <span data-ttu-id="2ed36-345">回避策では、ソリューションのルートに ASP.NET MVC 3 プロジェクトを作成し、ソリューション フォルダーに移動しています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-345">The workaround is to create the ASP.NET MVC 3 project in the root of the solution and then move it into the solution folder.</span></span>
- <span data-ttu-id="2ed36-346">インストーラーが完了する ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-346">The installer might take much longer than previous versions of ASP.NET MVC to complete.</span></span> <span data-ttu-id="2ed36-347">Visual Studio 2010 のコンポーネントを更新するためです。</span><span class="sxs-lookup"><span data-stu-id="2ed36-347">This is because it updates components of Visual Studio 2010.</span></span>
- <span data-ttu-id="2ed36-348">Razor 構文の IntelliSense では、ReSharper がインストールされている場合は機能しません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-348">IntelliSense for Razor syntax does not work when ReSharper is installed.</span></span> <span data-ttu-id="2ed36-349">ReSharper がインストールされているあり ASP.NET MVC 3 での Razor IntelliSense サポートを利用する場合、エントリを参照してください。 [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)併用する方法が説明されています Hadi Hariri のブログ、します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-349">If you have ReSharper installed and want to take advantage of the Razor IntelliSense support in ASP.NET MVC 3, see the entry [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) on Hadi Hariri's blog, which discusses ways to use them together today.</span></span>
- <span data-ttu-id="2ed36-350">ASP.NET MVC 3 のベータ版で作成した CCSHTML と VBHTML のビューでは、ビルド アクションが正しく設定されていない、プロジェクトが発行されたときにこれらを表示する結果の型が省略されています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-350">CCSHTML and VBHTML views created with the Beta version of ASP.NET MVC 3 do not have their build action set correctly, with the result that these view types are omitted when the project is published.</span></span> <span data-ttu-id="2ed36-351">これらのファイルのビルド アクションの値は、"Content"に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-351">The Build Action value for these files should be set to "Content".</span></span> <span data-ttu-id="2ed36-352">ASP.NET MVC 3 RTM が、新しいファイルの場合は、この問題を修正、プレリリース版で作成されたプロジェクトの既存のファイルの設定を修正しません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-352">ASP.NET MVC 3 RTM fixes this issue for new files, but doesn't correct the setting for existing files for a project created with prerelease versions.</span></span>
- ![](mvc3-release-notes/_static/image3.png)
- <span data-ttu-id="2ed36-353">インストール中に、使用許諾契約書への同意 ダイアログ ボックスのものよりも小さなウィンドウで、ライセンス条項が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-353">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>
- <span data-ttu-id="2ed36-354">Razor ビュー (.cshtml ファイル) を編集するときは、Visual Studio でのコント ローラーに移動 メニュー項目を使用可能にすることはできませんし、コード スニペットはありません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-354">When you are editing a Razor view (.cshtml file), the Go To Controller menu item in Visual Studio will not be available, and there are no code snippets.</span></span>
- <span data-ttu-id="2ed36-355">ASP.NET MVC 3 for Visual Web Developer Express で Visual Studio がインストールされていない、コンピューターにインストールし、後で Visual Studio をインストールして場合に、ASP.NET MVC 3 を再インストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-355">If you install ASP.NET MVC 3 for Visual Web Developer Express on a computer where Visual Studio is not installed, and then later install Visual Studio, you must reinstall ASP.NET MVC 3.</span></span> <span data-ttu-id="2ed36-356">Visual Studio と Visual Web Developer Express は、ASP.NET MVC 3 インストーラーによってアップグレードしたコンポーネントを共有します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-356">Visual Studio and Visual Web Developer Express share components that are upgraded by the ASP.NET MVC 3 installer.</span></span> <span data-ttu-id="2ed36-357">Visual Web Developer Express があるし、後で Visual Web Developer Express インストール コンピューターに Visual Studio for ASP.NET MVC 3 をインストールする場合、同じ問題が適用されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-357">The same issue applies if you install ASP.NET MVC 3 for Visual Studio on a computer that does not have Visual Web Developer Express and then later install Visual Web Developer Express.</span></span>

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a><span data-ttu-id="2ed36-358">互換性に影響する変更点</span><span class="sxs-lookup"><span data-stu-id="2ed36-358">Breaking Changes</span></span>

- <span data-ttu-id="2ed36-359">ASP.NET MVC の以前のバージョンでは、アクション フィルターは、いくつかの場合を除く要求ごと作成します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-359">In previous versions of ASP.NET MVC, action filters are create per request except in a few cases.</span></span> <span data-ttu-id="2ed36-360">この動作が保証された動作が実装の詳細だけで、されていないことと、フィルターのコントラクトがステートレスに考慮すべきでした。</span><span class="sxs-lookup"><span data-stu-id="2ed36-360">This behavior was never a guaranteed behavior but merely an implementation detail and the contract for filters was to consider them stateless.</span></span> <span data-ttu-id="2ed36-361">ASP.NET MVC 3 では、フィルターがより積極的にキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-361">In ASP.NET MVC 3, filters are cached more aggressively.</span></span> <span data-ttu-id="2ed36-362">そのため、不適切なインスタンスの状態を格納するカスタム アクション フィルターは、壊れている場合があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-362">Therefore, any custom action filters which improperly store instance state might be broken.</span></span>
- <span data-ttu-id="2ed36-363">例外フィルターの実行の順序が同じである例外フィルターの変更された*順序*値。</span><span class="sxs-lookup"><span data-stu-id="2ed36-363">The order of execution for exception filters has changed for exception filters that have the same *Order* value.</span></span> <span data-ttu-id="2ed36-364">ASP.NET MVC 2 以降では、同じであるコント ローラー上の例外がフィルター*順序*アクション メソッドで例外フィルターの前に実行されるため、アクション メソッドの値します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-364">In ASP.NET MVC 2 and earlier, exception filters on the controller that have the same *Order* value as those on an action method are executed before the exception filters on the action method.</span></span> <span data-ttu-id="2ed36-365">通常、ある場合、この例外フィルターが適用されるときに指定されていない*順序*値。</span><span class="sxs-lookup"><span data-stu-id="2ed36-365">This would typically be the case when exception filters are applied without a specified *Order* value.</span></span> <span data-ttu-id="2ed36-366">ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-366">In ASP.NET MVC 3, this order has been reversed so that the most specific exception handler executes first.</span></span> <span data-ttu-id="2ed36-367">以前のバージョンと場合、*順序*プロパティが明示的に指定されて、フィルターは、指定した順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-367">As in earlier versions, if the *Order* property is explicitly specified, the filters are run in the specified order.</span></span>
- <span data-ttu-id="2ed36-368">という名前の新しいプロパティ*FileExtensions*に追加された、 *VirtualPathProviderViewEngine*基本クラス。</span><span class="sxs-lookup"><span data-stu-id="2ed36-368">A new property named *FileExtensions* was added to the *VirtualPathProviderViewEngine* base class.</span></span> <span data-ttu-id="2ed36-369">ASP.NET では、(名前) ではなくパスでビューを検索、この新しいプロパティで指定されたリストに含まれるファイル拡張子を持つビューのみが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-369">When ASP.NET looks up a view by path (not by name), only views with a file extension contained in the list specified by this new property are considered.</span></span> <span data-ttu-id="2ed36-370">これは、Web フォーム ビューのファイルのカスタム拡張機能を有効にするには、カスタム ビルド プロバイダーが登録されているし、プロバイダー名ではなく、完全なパスを使用してこれらのビューが参照して、アプリケーションで重大な変更です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-370">This is a breaking change in applications where a custom build provider is registered in order to enable a custom file extension for Web Form views and where the provider references those views by using a full path rather than a name.</span></span> <span data-ttu-id="2ed36-371">回避の値を変更するには、 *FileExtensions*プロパティをカスタムのファイル拡張子が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-371">The workaround is to modify the value of the *FileExtensions* property to include the custom file extension.</span></span>
- <span data-ttu-id="2ed36-372">直接実装するカスタム コント ローラー ファクトリの実装、 *IControllerFactory*インターフェイスは、新しい実装を提供する必要があります*GetControllerSessionBehavior*がメソッドこのリリースでは、インターフェイスに追加されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-372">Custom controller factory implementations that directly implement the *IControllerFactory* interface must provide an implementation of the new *GetControllerSessionBehavior* method that was added to the interface in this release.</span></span> <span data-ttu-id="2ed36-373">一般に、お勧めするこのインターフェイスを直接実装およびしない代わりに、クラスから派生させる*DefaultControllerFactory*します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-373">In general, it is recommended that you do not implement this interface directly and instead derive your class from *DefaultControllerFactory*.</span></span>

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a><span data-ttu-id="2ed36-374">ASP.NET MVC 3 RC2 での変更</span><span class="sxs-lookup"><span data-stu-id="2ed36-374">Changes in ASP.NET MVC 3 RC2</span></span>

<span data-ttu-id="2ed36-375">このセクションでは、RC のリリース以降、ASP.NET MVC 3 RC2 リリースで行われた変更 (新機能とバグの修正) について説明します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-375">This section describes changes (new features and bug fixes) made in the ASP.NET MVC 3 RC2 release since the RC release.</span></span>

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a><span data-ttu-id="2ed36-376">JQuery 1.4.4、jQuery 検証 1.7、jQuery UI 1.8.6 など、プロジェクト テンプレートの変更</span><span class="sxs-lookup"><span data-stu-id="2ed36-376">Project Templates Changed to Include jQuery 1.4.4, jQuery Validation 1.7, and jQuery UI 1.8.6</span></span>

<span data-ttu-id="2ed36-377">ASP.NET MVC 3 のプロジェクト テンプレートでは、jQuery、jQuery の検証、および jQuery の最新バージョンが追加 UI。</span><span class="sxs-lookup"><span data-stu-id="2ed36-377">The project templates for ASP.NET MVC 3 now include the latest versions of jQuery, jQuery Validation, and jQuery UI.</span></span> <span data-ttu-id="2ed36-378">jQuery UI は、プロジェクト テンプレートに新たに追加し、便利なユーザー インターフェイス ウィジェットを提供します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-378">jQuery UI is a new addition to the project templates and provides useful user interface widgets.</span></span> <span data-ttu-id="2ed36-379">JQuery UI の詳細については、自分のホーム ページを参照してください: [ http://jqueryui.com/](http://jqueryui.com/)します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-379">For more information about jQuery UI, visit their homepage: [http://jqueryui.com/](http://jqueryui.com/).</span></span>

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a><span data-ttu-id="2ed36-380">追加された"AdditionalMetadataAttribute"クラス</span><span class="sxs-lookup"><span data-stu-id="2ed36-380">Added "AdditionalMetadataAttribute" Class</span></span>

<span data-ttu-id="2ed36-381">使用することができます、 *AdditionalMetadataAttribute*を設定するクラス、 *ModelMetadata.AdditionalValues*モデル プロパティのディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="2ed36-381">You can use the *AdditionalMetadataAttribute* class to populate the *ModelMetadata.AdditionalValues* dictionary for a model property.</span></span>

<span data-ttu-id="2ed36-382">たとえば、ビュー モデルに、管理者にのみ表示されるプロパティがあるとします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-382">For example, suppose a view model has properties that should be displayed only to an administrator.</span></span> <span data-ttu-id="2ed36-383">そのモデル AdminOnly をキーと true として次の例のように、値として使用して、新しい属性で注釈を付けることができます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-383">That model can be annotated with the new attribute using AdminOnly as the key and true as the value, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

<span data-ttu-id="2ed36-384">このメタデータは、製品のビュー モデルが表示されるときに、表示またはエディターのテンプレートを使用されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-384">This metadata is made available to any display or editor template when a product view model is rendered.</span></span> <span data-ttu-id="2ed36-385">メタデータ情報を解釈するアプリケーション開発者としての開発者次第です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-385">It is up to you as application developer to interpret the metadata information.</span></span>

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a><span data-ttu-id="2ed36-386">強化されたビューのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="2ed36-386">Improved View Scaffolding</span></span>

<span data-ttu-id="2ed36-387">ここではビューのスキャフォールディングを使用する T4 テンプレートなどのテンプレート ヘルパー メソッドの呼び出しの生成*EditorFor*などのヘルパーではなく*TextBoxFor*します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-387">The T4 templates used for scaffolding views now generate calls to template helper methods such as *EditorFor* instead of helpers such as *TextBoxFor*.</span></span> <span data-ttu-id="2ed36-388">この変更は、ビューの追加 ダイアログ ボックスのビューを生成するときに、データ注釈属性の形式でモデルのメタデータのサポートを向上します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-388">This change improves support for metadata on the model in the form of data annotation attributes when the Add View dialog box generates a view.</span></span>

<span data-ttu-id="2ed36-389">ビューの追加のスキャフォールディングには、検出機能の向上と規則に基づいて、モデルの主キーの情報の使用状況も含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-389">The Add View scaffolding also includes improved detection and usage of primary key information on the model, based on convention.</span></span> <span data-ttu-id="2ed36-390">たとえば、ビューの追加 ダイアログ ボックスはこの情報を使用して主キーの値が編集可能なフォームのフィールドとスキャフォールディングしないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-390">For example, the Add View dialog box uses this information to ensure that the primary key value is not scaffolded as an editable form field.</span></span>

<span data-ttu-id="2ed36-391">既定の編集とテンプレートの作成には、クライアントの検証に必要な jQuery スクリプトへの参照が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-391">The default Edit and Create templates include references to the jQuery scripts needed for client validation.</span></span>

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a><span data-ttu-id="2ed36-392">追加された Html.Raw メソッド</span><span class="sxs-lookup"><span data-stu-id="2ed36-392">Added Html.Raw Method</span></span>

<span data-ttu-id="2ed36-393">既定では、Razor はすべての値を HTML エンコード エンジンを表示します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-393">By default, the Razor view engine HTML-encodes all values.</span></span> <span data-ttu-id="2ed36-394">次のコード スニペットがあいさつの変数の内部 HTML をエンコードするとしてページに表示されるように、`<strong>Hello World!</strong>`します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-394">For example, the following code snippet encodes the HTML inside the greeting variable so that it is displayed in the page as `<strong>Hello World!</strong>`.</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

<span data-ttu-id="2ed36-395">新しい*Html.Raw*メソッドは、コンテンツの安全がわかっている場合は、エンコードされていない HTML を表示する簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-395">The new *Html.Raw* method provides a simple way of displaying unencoded HTML when the content is known to be safe.</span></span> <span data-ttu-id="2ed36-396">次の例が、同じ文字列が表示されますが、文字列がマークアップとしてレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-396">The following example displays the same string, but the string is rendered as markup:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a><span data-ttu-id="2ed36-397">名前が変更された"Controller.ViewModel"プロパティと"ViewBag"を"View"プロパティ</span><span class="sxs-lookup"><span data-stu-id="2ed36-397">Renamed "Controller.ViewModel" Property and the "View" Property To "ViewBag"</span></span>

<span data-ttu-id="2ed36-398">以前は、 *ViewModel*プロパティの*コント ローラー*に対応する、*ビュー*ビューのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="2ed36-398">Previously, the *ViewModel* property of *Controller* corresponded to the *View* property of a view.</span></span> <span data-ttu-id="2ed36-399">値をアクセスする方法を提供これら両方のプロパティ、 *ViewDataDictionary*オブジェクトの動的なプロパティ アクセサーの構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-399">Both of these properties provide a way to access values of the *ViewDataDictionary* object using dynamic property-accessor syntax.</span></span> <span data-ttu-id="2ed36-400">両方のプロパティは、混乱を避けるためより一貫したものと同じである名前に変更されています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-400">Both properties have been renamed to be the same in order to avoid confusion and to be more consistent.</span></span>

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a><span data-ttu-id="2ed36-401">名前が変更された"ControllerSessionStateAttribute"クラス"SessionStateAttribute"を</span><span class="sxs-lookup"><span data-stu-id="2ed36-401">Renamed "ControllerSessionStateAttribute" Class to "SessionStateAttribute"</span></span>

<span data-ttu-id="2ed36-402">*ControllerSessionStateAttribute*クラスは、ASP.NET MVC 3 の RC リリースで導入されました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-402">The *ControllerSessionStateAttribute* class was introduced in the RC release of ASP.NET MVC 3.</span></span> <span data-ttu-id="2ed36-403">簡潔にするプロパティが変更されました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-403">The property was renamed to be more succinct.</span></span>

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a><span data-ttu-id="2ed36-404">「ため」に名前を変更した RemoteAttribute"Fields"プロパティ</span><span class="sxs-lookup"><span data-stu-id="2ed36-404">Renamed RemoteAttribute "Fields" Property to "AdditionalFields"</span></span>

<span data-ttu-id="2ed36-405">*RemoteAttribute*クラスの*フィールド*ユーザー間で混乱の原因となったプロパティ。</span><span class="sxs-lookup"><span data-stu-id="2ed36-405">The *RemoteAttribute* class's *Fields* property caused some confusion among users.</span></span> <span data-ttu-id="2ed36-406">このプロパティの名前を変更する*ため*その意図を明確にします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-406">Renaming this property to *AdditionalFields* clarifies its intent.</span></span>

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a><span data-ttu-id="2ed36-407">"AllowHtmlAttribute"を"SkipRequestValidationAttribute"の名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-407">Renamed "SkipRequestValidationAttribute" to "AllowHtmlAttribute"</span></span>

<span data-ttu-id="2ed36-408">*SkipRequestValidationAttribute*に属性が変更されました*AllowHtmlAttribute*をわかりやすく、用途を表します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-408">The *SkipRequestValidationAttribute* attribute was renamed to *AllowHtmlAttribute* to better represent its intended usage.</span></span>

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a><span data-ttu-id="2ed36-409">最初に、役立つエラー メッセージを表示する変更された"Html.ValidationMessage"メソッド</span><span class="sxs-lookup"><span data-stu-id="2ed36-409">Changed "Html.ValidationMessage" Method to Display the First Useful Error Message</span></span>

<span data-ttu-id="2ed36-410">*Html.ValidationMessage*最初のエラーを表示するだけではなく最初の有用なエラー メッセージを表示するメソッドが修正されました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-410">The *Html.ValidationMessage* method was fixed to show the first useful error message instead of simply displaying the first error.</span></span>

<span data-ttu-id="2ed36-411">モデルのバインド中に、 *ModelState*ディクショナリは、モデル自体からなど、プロパティに関するエラー メッセージで複数のソースから設定できます (実装している場合*IValidatableObject*)、およびプロパティがアクセスされているときにスローされる例外、プロパティに適用される検証属性から。</span><span class="sxs-lookup"><span data-stu-id="2ed36-411">During model binding, the *ModelState* dictionary can be populated from multiple sources with error messages about the property, including from the model itself (if it implements *IValidatableObject*), from validation attributes applied to the property, and from exceptions thrown while the property is being accessed.</span></span>

<span data-ttu-id="2ed36-412">ときに、 *Html.ValidationMessage*メソッド検証メッセージを表示する、これらは一般にないため、エンドユーザーは、例外が含まれているモデル状態エントリをスキップします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-412">When the *Html.ValidationMessage* method displays a validation message, it skips model-state entries that include an exception, because these are generally not intended for the end user.</span></span> <span data-ttu-id="2ed36-413">代わりに、このメソッドは最初の検証メッセージを例外に関連付けられていないと、そのメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-413">Instead, the method looks for the first validation message that is not associated with an exception and displays that message.</span></span> <span data-ttu-id="2ed36-414">このようなメッセージが検出されない場合の既定値は、最初の例外に関連付けられている一般的なエラー メッセージです。</span><span class="sxs-lookup"><span data-stu-id="2ed36-414">If no such message is found, it defaults to a generic error message that is associated with the first exception.</span></span>

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a><span data-ttu-id="2ed36-415">固定@model空白の文書に追加の宣言</span><span class="sxs-lookup"><span data-stu-id="2ed36-415">Fixed @model Declaration to not Add Whitespace to the Document</span></span>

<span data-ttu-id="2ed36-416">以前のリリースで、 <em>@model</em>ビューの上部にある宣言が表示される HTML 出力に空白行を追加します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-416">In earlier releases, the <em>@model</em> declaration at the top of a view added a blank line to the rendered HTML output.</span></span> <span data-ttu-id="2ed36-417">これは、宣言は空白を挿入しないように修正されています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-417">This has been fixed so that the declaration does not introduce whitespace.</span></span>

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a><span data-ttu-id="2ed36-418">エンジン固有のファイル名をサポートするために、ビュー エンジンに追加された"FileExtensions"プロパティ</span><span class="sxs-lookup"><span data-stu-id="2ed36-418">Added "FileExtensions" Property to View Engines to Support Engine-Specific File Names</span></span>

<span data-ttu-id="2ed36-419">ビュー エンジンは、次の例のように、明示的なビューのパスを使用してビューを返すことができます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-419">A view engine can return a view using an explicit view path as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

<span data-ttu-id="2ed36-420">最初のビュー エンジンは、常にビューをレンダリングしようとします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-420">The first view engine always attempts to render the view.</span></span> <span data-ttu-id="2ed36-421">既定では、Web フォーム ビュー エンジンは、最初のビュー エンジンです。Web フォームのエンジンは、Razor ビューをレンダリングできません、ためエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-421">By default, the Web Forms view engine is the first view engine; because the Web Forms engine cannot render a Razor view, an error occurs.</span></span> <span data-ttu-id="2ed36-422">ビュー エンジンのようになりましたが、 *FileExtensions*ファイル拡張子を指定するために使用するプロパティをサポートします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-422">View engines now have a *FileExtensions* property that is used to specify which file extensions they support.</span></span> <span data-ttu-id="2ed36-423">このプロパティは、ASP.NET では、ビュー エンジンが、ファイルを表示できるかどうかが判断した場合にチェックされます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-423">This property is checked when ASP.NET determines whether a view engine can render a file.</span></span> <span data-ttu-id="2ed36-424">これは重大な変更と詳細についてに含まれる、[重大な変更](#_Toc2_BC)このドキュメントの「します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-424">This is a breaking change and more details are included in the [Breaking Changes](#_Toc2_BC) section of this document.</span></span>

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a><span data-ttu-id="2ed36-425">固定"LabelFor"ヘルパー"For"属性の正しい値を出力するには</span><span class="sxs-lookup"><span data-stu-id="2ed36-425">Fixed "LabelFor" Helper to Emit the Correct Value for the "For" Attribute</span></span>

<span data-ttu-id="2ed36-426">バグはどこで修正された、 *LabelFor*レンダリング メソッドを*の*と一致する属性、*入力*要素の*名前*属性の代わりにID の</span><span class="sxs-lookup"><span data-stu-id="2ed36-426">A bug was fixed where the *LabelFor* method rendered a *for* attribute that matches the *input* element's *name* attribute instead of its ID.</span></span> <span data-ttu-id="2ed36-427">W3C に従って、*の*属性に一致する必要があります、*入力*要素の id。</span><span class="sxs-lookup"><span data-stu-id="2ed36-427">According to the W3C, the *for* attribute should match the *input* element's ID.</span></span>

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a><span data-ttu-id="2ed36-428">モデル バインド中に明示的な値を優先する固定"RenderAction"メソッド</span><span class="sxs-lookup"><span data-stu-id="2ed36-428">Fixed "RenderAction" Method to Give Explicit Values Precedence During Model Binding</span></span>

<span data-ttu-id="2ed36-429">以前のバージョンでは、明示的な値に渡された、 *RenderAction*メソッドは現在のフォーム値を優先して子アクション内でのモデル バインド中に無視されているされました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-429">In earlier versions, explicit values that were passed to the *RenderAction* method were being ignored in favor of the current form values during model binding inside a child action.</span></span> <span data-ttu-id="2ed36-430">この修正により、明示的な値がモデル バインド中に優先順位を取得します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-430">The fix ensures that explicit values take precedence during model binding.</span></span>

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a><span data-ttu-id="2ed36-431">互換性に影響する変更点</span><span class="sxs-lookup"><span data-stu-id="2ed36-431">Breaking Changes</span></span>

- <span data-ttu-id="2ed36-432">ASP.NET MVC の以前のバージョンでは、アクション フィルターが 1 回のような場合を除く要求作成されました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-432">In previous versions of ASP.NET MVC, action filters were created per request except in a few cases.</span></span> <span data-ttu-id="2ed36-433">この動作が保証された動作が実装の詳細だけで、されていないことと、フィルターのコントラクトがステートレスに考慮すべきでした。</span><span class="sxs-lookup"><span data-stu-id="2ed36-433">This behavior was never a guaranteed behavior but merely an implementation detail and the contract for filters was to consider them stateless.</span></span> <span data-ttu-id="2ed36-434">ASP.NET MVC 3 では、フィルターがより積極的にキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-434">In ASP.NET MVC 3, filters are cached more aggressively.</span></span> <span data-ttu-id="2ed36-435">そのため、不適切なインスタンスの状態を格納するカスタム アクション フィルターは、壊れている場合があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-435">Therefore, any custom action filters which improperly store instance state might be broken.</span></span>
- <span data-ttu-id="2ed36-436">例外フィルターの実行の順序が同じである例外フィルターの変更された*順序*値。</span><span class="sxs-lookup"><span data-stu-id="2ed36-436">The order of execution for exception filters has changed for exception filters that have the same *Order* value.</span></span> <span data-ttu-id="2ed36-437">ASP.NET MVC 2 以降では、同じがあったコント ローラー上の例外がフィルター*順序*アクション メソッドで例外フィルターの前に実行されたアクション メソッドの値します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-437">In ASP.NET MVC 2 and earlier, exception filters on the controller that had the same *Order* value as those on an action method were executed before the exception filters on the action method.</span></span> <span data-ttu-id="2ed36-438">通常、ある場合、この例外フィルターが適用されたときに指定されていない*順序*値。</span><span class="sxs-lookup"><span data-stu-id="2ed36-438">This would typically be the case when exception filters were applied without a specified *Order* value.</span></span> <span data-ttu-id="2ed36-439">ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-439">In ASP.NET MVC 3, this order has been reversed so that the most specific exception handler executes first.</span></span> <span data-ttu-id="2ed36-440">以前のバージョンと場合、*順序*プロパティが明示的に指定されて、フィルターは、指定した順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-440">As in earlier versions, if the *Order* property is explicitly specified, the filters are run in the specified order.</span></span>
- <span data-ttu-id="2ed36-441">という名前の新しいプロパティ*FileExtensions*に追加された、 *VirtualPathProviderViewEngine*基本クラス。</span><span class="sxs-lookup"><span data-stu-id="2ed36-441">A new property named *FileExtensions* was added to the *VirtualPathProviderViewEngine* base class.</span></span> <span data-ttu-id="2ed36-442">ASP.NET では、(名前) ではなくパスでビューを検索、この新しいプロパティで指定されたリストに含まれるファイル拡張子を持つビューのみが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-442">When ASP.NET looks up a view by path (not by name), only views with a file extension contained in the list specified by this new property are considered.</span></span> <span data-ttu-id="2ed36-443">これは、Web フォーム ビューのファイルのカスタム拡張機能を有効にするには、カスタム ビルド プロバイダーが登録されているし、プロバイダー名ではなく、完全なパスを使用してこれらのビューが参照して、アプリケーションで重大な変更です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-443">This is a breaking change in applications where a custom build provider is registered in order to enable a custom file extension for Web Form views and where the provider references those views by using a full path rather than a name.</span></span> <span data-ttu-id="2ed36-444">回避の値を変更するには、 *FileExtensions*プロパティをカスタムのファイル拡張子が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-444">The workaround is to modify the value of the *FileExtensions* property to include the custom file extension.</span></span>
- <span data-ttu-id="2ed36-445">直接実装するカスタム コント ローラー ファクトリの実装、 <em>IControllerFactory</em>インターフェイスは、新しい実装を提供する必要があります<em>GetControllerSessionBehavior</em> <em>このリリースでは、インターフェイスに追加されたメソッド</em>します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-445">Custom controller factory implementations that directly implement the <em>IControllerFactory</em> interface must provide an implementation of the new <em>GetControllerSessionBehavior</em><em>method that was added to the interface in this release</em>.</span></span> <span data-ttu-id="2ed36-446">一般に、お勧めするこのインターフェイスを直接実装およびしない代わりに、クラスから派生させる<em>DefaultControllerFactory</em>します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-446">In general, it is recommended that you do not implement this interface directly and instead derive your class from <em>DefaultControllerFactory</em>.</span></span>

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a><span data-ttu-id="2ed36-447">既知の問題</span><span class="sxs-lookup"><span data-stu-id="2ed36-447">Known Issues</span></span>

- <span data-ttu-id="2ed36-448">ASP.NET MVC 3 インストーラーでは、NuGet パッケージ マネージャーの初期のバージョンをインストールすることのみです。</span><span class="sxs-lookup"><span data-stu-id="2ed36-448">The ASP.NET MVC 3 installer is only able to install an initial version of the NuGet package manager.</span></span> <span data-ttu-id="2ed36-449">最初のバージョンをインストールした後、NuGet はインストールすることができ、Visual Studio 拡張機能マネージャーを使用して更新します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-449">After you have installed the initial version, NuGet can be installed and updated using Visual Studio Extension Manager.</span></span> <span data-ttu-id="2ed36-450">NuGet がインストールされて既にある場合は、NuGet の最新バージョンに更新するには、Visual Studio 拡張機能ギャラリーに移動します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-450">If you already have NuGet installed, go to the Visual Studio Extension Gallery to update to the latest version of NuGet.</span></span>
- <span data-ttu-id="2ed36-451">ソリューション フォルダー内で新しい ASP.NET MVC 3 プロジェクトを作成するには、原因、 *NullReferenceException*エラー。</span><span class="sxs-lookup"><span data-stu-id="2ed36-451">Creating a new ASP.NET MVC 3 project within a solution folder causes a *NullReferenceException* error.</span></span> <span data-ttu-id="2ed36-452">回避策では、ソリューションのルートに ASP.NET MVC 3 プロジェクトを作成し、ソリューション フォルダーに移動しています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-452">The workaround is to create the ASP.NET MVC 3 project in the root of the solution and then move it into the solution folder.</span></span>
- <span data-ttu-id="2ed36-453">インストーラーが完了する ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-453">The installer might take much longer than previous versions of ASP.NET MVC to complete.</span></span> <span data-ttu-id="2ed36-454">Visual Studio 2010 のコンポーネントを更新するためです。</span><span class="sxs-lookup"><span data-stu-id="2ed36-454">This is because it updates components of Visual Studio 2010.</span></span>
- <span data-ttu-id="2ed36-455">Razor 構文の IntelliSense では、ReSharper がインストールされている場合は機能しません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-455">IntelliSense for Razor syntax does not work when ReSharper is installed.</span></span> <span data-ttu-id="2ed36-456">ReSharper がインストールされているあり ASP.NET MVC 3 RC2 での Razor IntelliSense サポートを利用する場合、エントリを参照してください。 [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)併用する方法が説明されています Hadi Hariri のブログ、します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-456">If you have ReSharper installed and want to take advantage of the Razor IntelliSense support in ASP.NET MVC 3 RC2, see the entry [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) on Hadi Hariri's blog, which discusses ways to use them together today.</span></span>
- <span data-ttu-id="2ed36-457">ASP.NET MVC 3 のベータ版で作成された CSHTML と VBHTML のビューでは、ビルド アクションが正しく設定されていない、プロジェクトが発行されたときにこれらを表示する結果の型が省略されています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-457">CSHTML and VBHTML views created with the Beta version of ASP.NET MVC 3 do not have their build action set correctly, with the result that these view types are omitted when the project is published.</span></span> <span data-ttu-id="2ed36-458">*ビルド アクション*値は、これらのファイルは、コンテンツに設定する必要があります"。</span><span class="sxs-lookup"><span data-stu-id="2ed36-458">The *Build Action* value for these files should be set to Content".</span></span> <span data-ttu-id="2ed36-459">ASP.NET MVC 3 RC2 では、新しいファイルの場合は、この問題が修正がベータ版で作成されたプロジェクトの既存のファイルの設定を修正しません。![](mvc3-release-notes/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2ed36-459">ASP.NET MVC 3 RC2 fixes this issue for new files, but doesn't correct the setting for existing files for a project created with the Beta version.![](mvc3-release-notes/_static/image4.png)</span></span>
- <span data-ttu-id="2ed36-460">インストール中に、使用許諾契約書への同意 ダイアログ ボックスのものよりも小さなウィンドウで、ライセンス条項が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-460">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>
- <span data-ttu-id="2ed36-461">Razor ビュー (.cshtml ファイル) を編集するときは、Visual Studio でのコント ローラーに移動 メニュー項目を使用可能にすることはできませんし、コード スニペットはありません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-461">When you are editing a Razor view (.cshtml file), the Go To Controller menu item in Visual Studio will not be available, and there are no code snippets.</span></span>
- <span data-ttu-id="2ed36-462">ASP.NET MVC 3 for Visual Web Developer Express で Visual Studio がインストールされていない、コンピューターにインストールし、後で Visual Studio をインストールして場合に、ASP.NET MVC 3 を再インストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-462">If you install ASP.NET MVC 3 for Visual Web Developer Express on a computer where Visual Studio is not installed, and then later install Visual Studio, you must reinstall ASP.NET MVC 3.</span></span> <span data-ttu-id="2ed36-463">Visual Studio と Visual Web Developer Express は、ASP.NET MVC 3 インストーラーによってアップグレードしたコンポーネントを共有します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-463">Visual Studio and Visual Web Developer Express share components that are upgraded by the ASP.NET MVC 3 installer.</span></span> <span data-ttu-id="2ed36-464">Visual Web Developer Express があるし、後で Visual Web Developer Express インストール コンピューターに Visual Studio for ASP.NET MVC 3 をインストールする場合、同じ問題が適用されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-464">The same issue applies if you install ASP.NET MVC 3 for Visual Studio on a computer that does not have Visual Web Developer Express and then later install Visual Web Developer Express.</span></span>
- <span data-ttu-id="2ed36-465">ASP.NET MVC 3 RC 2 をインストールしても、NuGet は更新されない場合、既にインストールされていること。</span><span class="sxs-lookup"><span data-stu-id="2ed36-465">Installing ASP.NET MVC 3 RC 2 does not update NuGet if you already have it installed.</span></span> <span data-ttu-id="2ed36-466">NuGet をアップグレードするには、Visual Studio 拡張機能マネージャーに移動する必要がありますとして表示、利用可能な更新。</span><span class="sxs-lookup"><span data-stu-id="2ed36-466">To upgrade NuGet, go to the Visual Studio Extension manager and it should show up as an available update.</span></span> <span data-ttu-id="2ed36-467">NuGet は、そこから最新のリリースにアップグレードできます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-467">You can upgrade NuGet to the latest release from there.</span></span>

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a><span data-ttu-id="2ed36-468">ASP.NET MVC 3 のリリース候補</span><span class="sxs-lookup"><span data-stu-id="2ed36-468">ASP.NET MVC 3 Release Candidate</span></span>

<span data-ttu-id="2ed36-469">ASP.NET MVC リリース候補は、2010 年 11 月 9 日にリリースされました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-469">ASP.NET MVC Release Candidate was released on November 9, 2010.</span></span>

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a><span data-ttu-id="2ed36-470">ASP.NET MVC 3 RC の新機能</span><span class="sxs-lookup"><span data-stu-id="2ed36-470">New Features in ASP.NET MVC 3 RC</span></span>

<span data-ttu-id="2ed36-471">このセクションが導入された機能について説明します、ベータ版リリース以降、ASP.NET MVC 3 RC リリースでします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-471">This section describes features that have been introduced in the ASP.NET MVC 3 RC release since the Beta release.</span></span>

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a><span data-ttu-id="2ed36-472">NuGet パッケージ マネージャー</span><span class="sxs-lookup"><span data-stu-id="2ed36-472">NuGet Package Manager</span></span>

<span data-ttu-id="2ed36-473">ASP.NET MVC 3 には、(旧称 NuPack)、Visual Studio プロジェクトにライブラリとツールを追加するための統合パッケージ管理ツールである NuGet パッケージ マネージャーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-473">ASP.NET MVC 3 includes the NuGet Package Manager (formerly known as NuPack), which is an integrated package management tool for adding libraries and tools to Visual Studio projects.</span></span> <span data-ttu-id="2ed36-474">このツールは、開発者は、ソース ツリーにライブラリを取得する現在実行中の手順を自動化します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-474">This tool automates the steps that developers take today to get a library into their source tree.</span></span>

<span data-ttu-id="2ed36-475">コマンド ライン ツール、Visual Studio のコンテキスト メニューから Visual Studio 2010 内の統合されたコンソール ウィンドウと一連の PowerShell コマンドレットは、NuGet を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-475">You can work with NuGet as a command-line tool, as an integrated console window inside Visual Studio 2010, from the Visual Studio context menu, and as a set of PowerShell cmdlets.</span></span>

<span data-ttu-id="2ed36-476">NuGet の詳細については、読み取り、 [Nuget のドキュメント](https://docs.microsoft.com/nuget/)します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-476">For more information about NuGet, read the [Nuget Documentation](https://docs.microsoft.com/nuget/).</span></span>

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a><span data-ttu-id="2ed36-477">新しいプロジェクト の改善 ダイアログ ボックス</span><span class="sxs-lookup"><span data-stu-id="2ed36-477">Improved "New Project" Dialog Box</span></span>

<span data-ttu-id="2ed36-478">新しいプロジェクトを作成するときに、[新しいプロジェクト] ダイアログ ボックスできるようになりました、ビュー エンジンも ASP.NET MVC プロジェクトの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-478">When you create a new project, the New Project dialog box now lets you specify the view engine as well as an ASP.NET MVC project type.</span></span>

![](mvc3-release-notes/_static/image5.png)

<span data-ttu-id="2ed36-479">このリリースでは、テンプレート、およびビュー エンジンの表示 ダイアログ ボックスの一覧を変更するためのサポートが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-479">Support for modifying the list of templates and view engines listed in the dialog box is included in this release.</span></span>

<span data-ttu-id="2ed36-480">既定のテンプレート、次に示します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-480">The default templates are the following:</span></span>

<span data-ttu-id="2ed36-481">空です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-481">Empty.</span></span> <span data-ttu-id="2ed36-482">最低限 ASP.NET MVC プロジェクトでの既定ディレクトリ構造、ASP.NET MVC をスタイル設定、既定値と既定の JavaScript ファイルを含む Scripts ディレクトリを含む Site.css ファイルを含む、ASP.NET MVC プロジェクトのファイルにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-482">Contains a minimal set of files for an ASP.NET MVC project, including the default directory structure for ASP.NET MVC projects, a Site.css file that contains the default ASP.NET MVC styles, and a Scripts directory that contains the default JavaScript files.</span></span>

<span data-ttu-id="2ed36-483">インターネット アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="2ed36-483">Internet Application.</span></span> <span data-ttu-id="2ed36-484">ASP.NET MVC を使用したメンバーシップ プロバイダーを使用する方法を示すサンプルの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-484">Contains sample functionality that demonstrates how to use the membership provider with ASP.NET MVC.</span></span>

<span data-ttu-id="2ed36-485">ダイアログ ボックスに表示されるプロジェクト テンプレートの一覧は、Windows レジストリで指定されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-485">The list of project templates that is displayed in the dialog box is specified in the Windows registry.</span></span>

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a><span data-ttu-id="2ed36-486">Sessionless コント ローラー</span><span class="sxs-lookup"><span data-stu-id="2ed36-486">Sessionless Controllers</span></span>

<span data-ttu-id="2ed36-487">新しい*ControllerSessionStateAttribute*うえでセッション状態の動作の制御コント ローラーを指定して、 [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx)列挙値。</span><span class="sxs-lookup"><span data-stu-id="2ed36-487">The new *ControllerSessionStateAttribute* gives you more control over session-state behavior for controllers by specifying a [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) enumeration value.</span></span>

<span data-ttu-id="2ed36-488">次の例では、すべての要求をコント ローラーのセッション状態をオフにする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-488">The following example shows how to turn off session state for all requests to a controller.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

<span data-ttu-id="2ed36-489">次の例では、コント ローラーにすべての要求の読み取り専用のセッション状態を設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-489">The following example shows how to set read-only session state for all requests to a controller.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a><span data-ttu-id="2ed36-490">新しい検証属性</span><span class="sxs-lookup"><span data-stu-id="2ed36-490">New Validation Attributes</span></span>

#### <a name="compareattribute"></a><span data-ttu-id="2ed36-491">CompareAttribute</span><span class="sxs-lookup"><span data-stu-id="2ed36-491">CompareAttribute</span></span>

<span data-ttu-id="2ed36-492">新しい*CompareAttribute*検証属性では、モデルの 2 つの異なるプロパティの値を比較することができます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-492">The new *CompareAttribute* validation attribute lets you compare the values of two different properties of a model.</span></span> <span data-ttu-id="2ed36-493">次の例では、 *ComparePassword*プロパティに一致する必要があります、*パスワード*有効にするのにはフィールド。</span><span class="sxs-lookup"><span data-stu-id="2ed36-493">In the following example, the *ComparePassword* property must match the *Password* field in order to be valid.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a><span data-ttu-id="2ed36-494">RemoteAttribute</span><span class="sxs-lookup"><span data-stu-id="2ed36-494">RemoteAttribute</span></span>

<span data-ttu-id="2ed36-495">新しい*RemoteAttribute*によって実際の検証ロジックを実行するサーバーでメソッドを呼び出すクライアント側の検証、jQuery 検証プラグインのリモート検証の検証属性を利用します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-495">The new *RemoteAttribute* validation attribute takes advantage of the jQuery Validation plug-in's remote validator, which enables client-side validation to call a method on the server that performs the actual validation logic.</span></span>

<span data-ttu-id="2ed36-496">次の例では、*ユーザー名*プロパティは、 *RemoteAttribute*適用します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-496">In the following example, the *UserName* property has the *RemoteAttribute* applied.</span></span> <span data-ttu-id="2ed36-497">クライアント検証がという名前のアクションを呼び出して、編集ビューでは、このプロパティを編集するには、 *UserNameAvailable*上、 *UsersController*このフィールドを検証するためにクラス。</span><span class="sxs-lookup"><span data-stu-id="2ed36-497">When editing this property in an Edit view, client validation will call an action named *UserNameAvailable* on the *UsersController* class in order to validate this field.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

<span data-ttu-id="2ed36-498">次の例は、対応するコント ローラーを示しています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-498">The following example shows the corresponding controller.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

<span data-ttu-id="2ed36-499">既定では、属性が適用されるプロパティ名はクエリ文字列パラメーターとしてアクション メソッドに送信されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-499">By default, the property name that the attribute is applied to is sent to the action method as a query-string parameter.</span></span>

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a><span data-ttu-id="2ed36-500">"LabelFor"と"LabelForModel"メソッドの新しいオーバー ロード</span><span class="sxs-lookup"><span data-stu-id="2ed36-500">New Overloads for "LabelFor" and "LabelForModel" Methods</span></span>

<span data-ttu-id="2ed36-501">新しいオーバー ロードが追加されました、 *LabelFor*と*LabelForModel*できるメソッドは、ラベルのテキストを指定します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-501">New overloads have been added for the *LabelFor* and *LabelForModel* methods that let you specify the label text.</span></span> <span data-ttu-id="2ed36-502">次の例では、これらのオーバー ロードを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-502">The following example shows how to use these overloads.</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a><span data-ttu-id="2ed36-503">子アクションの出力キャッシュ</span><span class="sxs-lookup"><span data-stu-id="2ed36-503">Child Action Output Caching</span></span>

<span data-ttu-id="2ed36-504">*OutputCacheAttribute*を使用して呼び出される子アクションの出力がキャッシュをサポートしています、 *Html.RenderAction*または*Html.Action*ヘルパー メソッド。</span><span class="sxs-lookup"><span data-stu-id="2ed36-504">The *OutputCacheAttribute* supports output caching of child actions that are called by using the *Html.RenderAction* or *Html.Action* helper methods.</span></span> <span data-ttu-id="2ed36-505">次の例では、別のアクションを呼び出すビューを示します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-505">The following example shows a view that calls another action.</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

<span data-ttu-id="2ed36-506">*GetDate*アクションは、注釈が付けられた、 *OutputCacheAttribute*:</span><span class="sxs-lookup"><span data-stu-id="2ed36-506">The *GetDate* action is annotated with the *OutputCacheAttribute*:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

<span data-ttu-id="2ed36-507">このコードを実行すると、Html.Action("GetDate") への呼び出しの結果が 100 秒間キャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-507">When this code runs, the result of the call to Html.Action("GetDate") is cached for 100 seconds.</span></span>

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a><span data-ttu-id="2ed36-508">"ビューの追加 ダイアログ ボックスの機能強化</span><span class="sxs-lookup"><span data-stu-id="2ed36-508">"Add View" Dialog Box Improvements</span></span>

<span data-ttu-id="2ed36-509">厳密に型指定されたビューを追加するときにビューの追加 ダイアログ ボックスは多くの中核となる .NET Framework 型などの以前のリリースでより詳細に該当しない種類を今すぐフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-509">When you add a strongly typed view, the Add View dialog box now filters out more non-applicable types than in previous releases, such as many core .NET Framework types.</span></span> <span data-ttu-id="2ed36-510">また、クラスの名前と種類を検索しやすくなる完全修飾型名ではなくリストの並べ替えようになりました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-510">Also, the list is now sorted by the class name and not by the fully qualified type name, which makes it easier to find types.</span></span> <span data-ttu-id="2ed36-511">たとえば、型名は次の例のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-511">For example, the type name is now displayed as in the following example:</span></span>

<span data-ttu-id="2ed36-512">クラス名 (名前空間)</span><span class="sxs-lookup"><span data-stu-id="2ed36-512">ClassName (namespace)</span></span>

<span data-ttu-id="2ed36-513">以前のリリースでこのされて表示される次のよう。</span><span class="sxs-lookup"><span data-stu-id="2ed36-513">In earlier releases, this would have been displayed as the following:</span></span>

<span data-ttu-id="2ed36-514">Namespace.ClassName</span><span class="sxs-lookup"><span data-stu-id="2ed36-514">Namespace.ClassName</span></span>

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a><span data-ttu-id="2ed36-515">詳細な要求の検証</span><span class="sxs-lookup"><span data-stu-id="2ed36-515">Granular Request Validation</span></span>

<span data-ttu-id="2ed36-516">*除外*プロパティの*ValidateInputAttribute*は存在しません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-516">The *Exclude* property of *ValidateInputAttribute* no longer exists.</span></span> <span data-ttu-id="2ed36-517">代わりに、モデルの特定のプロパティのモデル バインド中にスキップされた要求の検証には、使用、新しい*SkipRequestValidationAttribute*します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-517">Instead, to have request validation skipped for specific properties of a model during model binding, use the new *SkipRequestValidationAttribute*.</span></span>

<span data-ttu-id="2ed36-518">たとえば、アクション メソッドは、ブログの投稿を編集するために使用します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-518">For example, suppose an action method is used to edit a blog post:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

<span data-ttu-id="2ed36-519">次の例では、ブログの投稿のビュー モデルを示します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-519">The following example shows the view model for a blog post.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

<span data-ttu-id="2ed36-520">ユーザーは、Description プロパティの一部のマークアップを送信する、モデル バインドは要求の検証のため失敗します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-520">When a user submits some markup for the Description property, model binding will fail due to request validation.</span></span> <span data-ttu-id="2ed36-521">ブログの投稿の説明のモデル バインド中に要求の検証を無効にするには、適用、 *SkipRequpestValidationAttribute*プロパティのこの例で示すように: です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-521">To disable request validation during model binding for the blog post Description, apply the *SkipRequpestValidationAttribute* to the property, as shown in this example:.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

<span data-ttu-id="2ed36-522">または、モデルのすべてのプロパティで要求の検証を無効に、次のように適用します。 *ValidateInputAttribute*の値を持つ*false*アクション メソッドに。</span><span class="sxs-lookup"><span data-stu-id="2ed36-522">Alternatively, to turn off request validation for every property of the model, apply *ValidateInputAttribute* with a value of *false* to the action method:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a><span data-ttu-id="2ed36-523">互換性に影響する変更点</span><span class="sxs-lookup"><span data-stu-id="2ed36-523">Breaking Changes</span></span>

- <span data-ttu-id="2ed36-524">例外フィルターの実行の順序が同じである例外フィルターの変更された*順序*値。</span><span class="sxs-lookup"><span data-stu-id="2ed36-524">The order of execution for exception filters has changed for exception filters that have the same *Order* value.</span></span> <span data-ttu-id="2ed36-525">ASP.NET MVC 2 以降では、同じがあったコント ローラー上の例外がフィルター*順序*アクション メソッドで例外フィルターの前に実行されたアクション メソッドのようにします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-525">In ASP.NET MVC 2 and earlier, exception filters on the controller that had the same *Order* as those on an action method were executed before the exception filters on the action method.</span></span> <span data-ttu-id="2ed36-526">通常、ある場合、この例外フィルターが適用されたときに指定されていない*順序*値。</span><span class="sxs-lookup"><span data-stu-id="2ed36-526">This would typically be the case when exception filters were applied without a specified *Order* value.</span></span> <span data-ttu-id="2ed36-527">ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-527">In ASP.NET MVC 3, this order has been reversed so that the most specific exception handler executes first.</span></span> <span data-ttu-id="2ed36-528">以前のバージョンと場合、*順序*プロパティが明示的に指定されて、フィルターは、指定した順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-528">As in earlier versions, if the *Order* property is explicitly specified, the filters are run in the specified order.</span></span>
- <span data-ttu-id="2ed36-529">という新しいプロパティを追加*FileExtensions*を*VirtualPathProviderViewEngine*基本クラス。</span><span class="sxs-lookup"><span data-stu-id="2ed36-529">Added a new property named *FileExtensions* to the *VirtualPathProviderViewEngine* base class.</span></span> <span data-ttu-id="2ed36-530">検索時にビューのパスで、名前ではなく) に含まれているファイル拡張子を持つビューにのみ、この新しいプロパティで指定された一覧と見なされます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-530">When looking up a view by path (and not by name), only views with a file extension contained in the list specified by this new property is considered.</span></span> <span data-ttu-id="2ed36-531">これは、web フォーム ビューのファイルのカスタム拡張機能を有効にするカスタム ビルド プロバイダーを登録し、名前ではなく、完全なパスを使用してこれらのビューを参照している方のための互換性に影響する変更です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-531">This is a breaking change for those who register a custom build provider to enable a custom file extension for web form views and are referencing those views by using a full path rather than a name.</span></span> <span data-ttu-id="2ed36-532">回避の値を変更するには、 *FileExtensions*プロパティをカスタムのファイル拡張子が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-532">The workaround is to modify the value of the *FileExtensions* property to include the custom file extension.</span></span>

<a id="_Toc276711795"></a>
## <a name="known-issues"></a><span data-ttu-id="2ed36-533">既知の問題</span><span class="sxs-lookup"><span data-stu-id="2ed36-533">Known Issues</span></span>

- <span data-ttu-id="2ed36-534">インストーラーは、Visual Studio 2010 のコンポーネントを更新するため、完了する ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-534">The installer may take much longer than previous versions of ASP.NET MVC to complete because it updates components of Visual Studio 2010.</span></span>
- <span data-ttu-id="2ed36-535">ビューの追加のスキャフォールディング astrongly を選択するときに、ビューのスキャフォールディング書き込み専用プロパティを入力します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-535">The Add View scaffolding when selecting astrongly typed view scaffolds write-only properties.</span></span> <span data-ttu-id="2ed36-536">これらは、スキャフォールディングによって常に無視する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-536">These should always be ignored by scaffolding.</span></span> <span data-ttu-id="2ed36-537">ビューの追加 ダイアログは、「編集」または「作成」のビューを生成するときにも読み取り専用プロパティをスキャフォールディングします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-537">The Add View dialog also scaffolds read-only properties when generating an "Edit" or "Create" view.</span></span> <span data-ttu-id="2ed36-538">読み取り専用プロパティを表示およびリスト ビューのスキャフォールディングのみ必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-538">Read-only properties should only be scaffolded for the Display and List views.</span></span>
- <span data-ttu-id="2ed36-539">ASP.NET MVC 3 を Async CTP と共にインストールすると、デバッグが機能しません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-539">Debugging doesn't work when ASP.NET MVC 3 is installed alongside the Async CTP.</span></span> <span data-ttu-id="2ed36-540">ASP.NET MVC 3 では、Async CTP と並行してインストールをすることはできません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-540">ASP.NET MVC 3 cannot be installed side-by-side with the Async CTP.</span></span> <span data-ttu-id="2ed36-541">デバッグを修復する Async CTP をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-541">Uninstall the Async CTP to repair debugging.</span></span> <span data-ttu-id="2ed36-542">詳細については、「[このブログの投稿](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html)ASP.NET MVC 3 RC すべて部分アンインストールの詳細。</span><span class="sxs-lookup"><span data-stu-id="2ed36-542">For more details, read [this blog post](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) about uninstalling all the pieces of ASP.NET MVC 3 RC.</span></span>
- <span data-ttu-id="2ed36-543">Razor Intellisense では、Resharper がインストールされている場合は機能しません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-543">Razor Intellisense does not work when Resharper is installed.</span></span> <span data-ttu-id="2ed36-544">ReSharper をインストールしてお読みください ASP.NET MVC 3 RC での Razor intellisense サポートの利用する[このブログの投稿](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)jetbrains を併用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-544">If you have ReSharper installed and want to take advantage of the Razor intellisense support in ASP.NET MVC 3 RC, please read [this blog post](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) from JetBrains which discusses ways to use them together today.</span></span>
- <span data-ttu-id="2ed36-545">ASP.NET MVC 3 のベータ版で作成された CSHTML と VBHTML のビューがありませんビルド アクションが正しく公開からそれらが省略されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-545">CSHTML and VBHTML views created with Beta of ASP.NET MVC 3 do not have their build action correctly which omits them from publishing.</span></span> <span data-ttu-id="2ed36-546">*ビルド アクション*は、これらのファイルは、"Content"に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-546">The *Build Action* for these files should be set to "Content".</span></span> <span data-ttu-id="2ed36-547">ASP.NET MVC 3 RC では、新しいファイルの場合は、この問題が修正がベータ版で作成されたプロジェクトの既存のファイルの設定を修正しません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-547">ASP.NET MVC 3 RC fixes this issue for new files, but doesn't correct the setting for existing files for a project created with the Beta.</span></span>
- <span data-ttu-id="2ed36-548">インストーラーは、Visual Studio 2010 のコンポーネントを更新するため、完了する ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-548">The installer may take much longer than previous versions of ASP.NET MVC to complete because it updates components of Visual Studio 2010.</span></span>
- <span data-ttu-id="2ed36-549">ビューの追加のスキャフォールディング ビューのスキャフォールディング"Edit"を選択すると、厳密に型指定されたときに読み取り専用プロパティを指定します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-549">The Add View scaffolding when selecting an "Edit" strongly typed view scaffolds read only properties.</span></span> <span data-ttu-id="2ed36-550">同様に、書き込み専用プロパティは「表示」ビューをスキャフォールディングします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-550">Likewise, write-only properties are scaffolded for "Display" views.</span></span>
- <span data-ttu-id="2ed36-551">インストール中に、使用許諾契約書への同意 ダイアログ ボックスのものよりも小さなウィンドウで、ライセンス条項が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-551">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>
- <span data-ttu-id="2ed36-552">Visual Studio Async CTP をインストールすると、リリースでは、Razor、ASP.NET MVC 3 のツールのインストールの一部として含まれている競合が発生します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-552">Installing the Visual Studio Async CTP causes a conflict with the Razor release that is included as part of the ASP.NET MVC 3 tooling installation.</span></span> <span data-ttu-id="2ed36-553">Visual Studio Async CTP と Razor リリースの両方を同じコンピューターにインストールしないでことを確認します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-553">Make sure that you do not try to install both the Visual Studio Async CTP and the Razor release on the same machine.</span></span>
- <span data-ttu-id="2ed36-554">Razor ビュー (.cshtml ファイル) を編集するときは、Visual Studio でのコント ローラーに移動 メニュー項目を使用可能にすることはできませんし、コード スニペットはありません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-554">When you are editing a Razor view (.cshtml file), the Go To Controller menu item in Visual Studio will not be available, and there are no code snippets.</span></span>

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a><span data-ttu-id="2ed36-555">ASP.NET MVC 3 のベータ版</span><span class="sxs-lookup"><span data-stu-id="2ed36-555">ASP.NET MVC 3 Beta</span></span>

<span data-ttu-id="2ed36-556">ASP.NET MVC 3 のベータ版は、2010 年 10 月 6 日にリリースされました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-556">ASP.NET MVC 3 Beta was released on October 6, 2010.</span></span> <span data-ttu-id="2ed36-557">次のノートは、ベータ リリースに固有し、更新や変更前の ASP.NET MVC 3 のリリース候補のセクションで参照されている影響を受けます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-557">The following notes are specific to the Beta release, and are subject to any updates or changes referenced in the ASP.NET MVC 3 Release Candidate section above.</span></span>

## <a id="0.1__Toc274034215"></a>  <span data-ttu-id="2ed36-558">新しい Featuresin ASP.NET MVC 3 のベータ版</span><span class="sxs-lookup"><span data-stu-id="2ed36-558">New Featuresin ASP.NET MVC 3 Beta</span></span>

<a id="0.1__Default_validation_system"></a><span data-ttu-id="2ed36-559">このセクションが導入された機能について説明します、ASP.NET MVC 3 のベータ リリースでします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-559">This section describes features that have been introduced in the ASP.NET MVC 3 Beta release.</span></span>

### <a id="0.1__Toc274034216"></a>  <span data-ttu-id="2ed36-560">NuGet パッケージ マネージャー</span><span class="sxs-lookup"><span data-stu-id="2ed36-560">NuGet Package Manager</span></span>

<span data-ttu-id="2ed36-561">ASP.NET MVC 3 では、NuGet パッケージ マネージャーは、追加のライブラリの統合パッケージ管理ツールおよび Visual Studio プロジェクトのツールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-561">ASP.NET MVC 3 includes NuGet Package Manager, which is an integrated package management tool for adding libraries and tools to Visual Studio projects.</span></span> <span data-ttu-id="2ed36-562">ほとんどの場合、開発者は、ソース ツリーにライブラリを取得する現在実行中の手順を自動化します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-562">For the most part, it automates the steps that developers take today to get a library into their source tree.</span></span>

<span data-ttu-id="2ed36-563">コマンド ライン ツール、Visual Studio のコンテキスト メニューから Visual Studio 2010 内の統合されたコンソール ウィンドウおよび PowerShell コマンドレットのセットは、NuGet を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-563">You can work with NuGet as a command line tool, as an integrated console window inside Visual Studio 2010, from the Visual Studio context menu, and as set of PowerShell cmdlets.</span></span>

<span data-ttu-id="2ed36-564">NuGet の詳細については、読み取り、 [NuGet のドキュメント](https://docs.microsoft.com/nuget/)します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-564">For more information about NuGet, read the [NuGet Documentation](https://docs.microsoft.com/nuget/).</span></span>

### <a id="0.1__Toc274034217"></a>  <span data-ttu-id="2ed36-565">強化された新しいプロジェクト ダイアログ ボックス</span><span class="sxs-lookup"><span data-stu-id="2ed36-565">Improved New Project Dialog Box</span></span>

<span data-ttu-id="2ed36-566">新しいプロジェクトを作成するときに、[新しいプロジェクト] ダイアログ ボックスできるようになりました、ビュー エンジンも ASP.NET MVC プロジェクトの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-566">When you create a new project, the New Project dialog box now lets you specify the view engine as well as an ASP.NET MVC project type.</span></span>

![](mvc3-release-notes/_static/image6.png)

<span data-ttu-id="2ed36-567">このリリースでは、テンプレート、およびビュー エンジンの表示 ダイアログ ボックスの一覧を変更するためのサポートが含まれていません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-567">Support for modifying the list of templates and view engines listed in the dialog box is not included in this release.</span></span>

<span data-ttu-id="2ed36-568">既定のテンプレート、次に示します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-568">The default templates are the following:</span></span>

<span data-ttu-id="2ed36-569">空です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-569">Empty.</span></span> <span data-ttu-id="2ed36-570">最低限 ASP.NET MVC プロジェクトでの既定ディレクトリ構造を既定の ASP.NET MVC をスタイル設定、および既定の JavaScript ファイルを含む Scripts ディレクトリを含む小さい Site.css ファイルを含む、ASP.NET MVC プロジェクトのファイルにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-570">Contains a minimal set of files for an ASP.NET MVC project, including the default directory structure for ASP.NET MVC projects, a small Site.css file that contains the default ASP.NET MVC styles, and a Scripts directory that contains the default JavaScript files.</span></span>

<span data-ttu-id="2ed36-571">インターネット アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="2ed36-571">Internet Application.</span></span> <span data-ttu-id="2ed36-572">ASP.NET MVC 内でのメンバーシップ プロバイダーを使用する方法を示すサンプルの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-572">Contains sample functionality that demonstrates how to use the membership provider within ASP.NET MVC.</span></span>

### <a id="0.1__Toc274034218"></a>  <span data-ttu-id="2ed36-573">厳密に指定する方法を簡略化された Razor ビューで型指定されたモデル</span><span class="sxs-lookup"><span data-stu-id="2ed36-573">Simplified Way to Specify Strongly Typed Models in Razor Views</span></span>

<span data-ttu-id="2ed36-574">新しいを使用して、厳密に型指定の Razor ビューのモデルの種類を指定する方法を簡略化された@modelCSHTML ビューのディレクティブと@ModelTypeVBHTML ビューのディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="2ed36-574">The way to specify the model type for strongly typed Razor views has been simplified by using the new @model directive for CSHTML views and @ModelType directive for VBHTML views.</span></span> <span data-ttu-id="2ed36-575">ASP.NET MVC の以前のバージョンでは、Razor の厳密に型指定されたモデル ビューのこのように指定します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-575">In earlier versions of ASP.NET MVC, you would specify a strongly typed model for Razor views this way:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

<span data-ttu-id="2ed36-576">このリリースでは、次の構文を使用できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-576">In this release, you can use the following syntax:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  <span data-ttu-id="2ed36-577">新しい ASP.NET Web ページのヘルパー メソッドをサポートします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-577">Support for New ASP.NET Web Pages Helper Methods</span></span>

<span data-ttu-id="2ed36-578">新しい ASP.NET Web Pages テクノロジには、一連ビューとコント ローラーによく使用される機能を追加するのに便利なヘルパー メソッドにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-578">The new ASP.NET Web Pages technology includes a set of helper methods that are useful for adding commonly used functionality to views and controllers.</span></span> <span data-ttu-id="2ed36-579">ASP.NET MVC 3 では、コント ローラーとビュー内でこれらのヘルパー メソッドを使用して、(適切な場所) をサポートします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-579">ASP.NET MVC 3 supports using these helper methods within controllers and views (where appropriate).</span></span> <span data-ttu-id="2ed36-580">これらのメソッドは、System.Web.Helpers アセンブリ内に格納されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-580">These methods are contained in the System.Web.Helpers assembly.</span></span> <span data-ttu-id="2ed36-581">次の表では、ASP.NET Web Pages のヘルパー メソッドのいくつかを示します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-581">The following table lists a few of the ASP.NET Web Pages helper methods.</span></span>

| <span data-ttu-id="2ed36-582">**ヘルパー**</span><span class="sxs-lookup"><span data-stu-id="2ed36-582">**Helper**</span></span> | <span data-ttu-id="2ed36-583">**説明**</span><span class="sxs-lookup"><span data-stu-id="2ed36-583">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="2ed36-584">グラフ</span><span class="sxs-lookup"><span data-stu-id="2ed36-584">Chart</span></span> | <span data-ttu-id="2ed36-585">ビュー内のグラフを表示します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-585">Renders a chart within a view.</span></span> <span data-ttu-id="2ed36-586">Chart.ToWebImage、Chart.Save、Chart.Write などのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-586">Contains methods such as Chart.ToWebImage, Chart.Save, and Chart.Write.</span></span> |
| <span data-ttu-id="2ed36-587">Crypto</span><span class="sxs-lookup"><span data-stu-id="2ed36-587">Crypto</span></span> | <span data-ttu-id="2ed36-588">正常に作成するアルゴリズムのハッシュを使用してでは、ソルトし、パスワードをハッシュします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-588">Uses hashing algorithms to create properly salted and hashed passwords.</span></span> |
| <span data-ttu-id="2ed36-589">WebGrid</span><span class="sxs-lookup"><span data-stu-id="2ed36-589">WebGrid</span></span> | <span data-ttu-id="2ed36-590">オブジェクト (通常は、データベースからのデータ) のコレクションをグリッドとして表示します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-590">Renders a collection of objects (typically, data from a database) as a grid.</span></span> <span data-ttu-id="2ed36-591">ページングと並べ替えをサポートします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-591">Supports paging and sorting.</span></span> |
| <span data-ttu-id="2ed36-592">WebImage</span><span class="sxs-lookup"><span data-stu-id="2ed36-592">WebImage</span></span> | <span data-ttu-id="2ed36-593">イメージを表示します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-593">Renders an image.</span></span> |
| <span data-ttu-id="2ed36-594">WebMail</span><span class="sxs-lookup"><span data-stu-id="2ed36-594">WebMail</span></span> | <span data-ttu-id="2ed36-595">電子メールを送信します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-595">Sends an email message.</span></span> |

<span data-ttu-id="2ed36-596">ヘルパーと基本的な構文を示すクイック リファレンスのトピックでは使用可能な次の URL で ASP.NET Razor 構文のドキュメントの一部として。</span><span class="sxs-lookup"><span data-stu-id="2ed36-596">A quick reference topic that lists the helpers and basic syntax is available as part of the ASP.NET Razor syntax documentation at the following URL:</span></span>

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  <span data-ttu-id="2ed36-597">追加の依存関係挿入をサポート</span><span class="sxs-lookup"><span data-stu-id="2ed36-597">Additional Dependency Injection Support</span></span>

<span data-ttu-id="2ed36-598">ASP.NET MVC 3 Preview 1 リリースのビルド、現在のリリースには、2 つの新しいサービスや既存の 4 つのサービスのサポートを追加し、依存関係の解決と共通サービス ロケーターのサポートの強化が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-598">Building on the ASP.NET MVC 3 Preview 1 release, the current release includes added support for two new services and four existing services, and improved support for dependency resolution and the Common Service Locator.</span></span>

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a><span data-ttu-id="2ed36-599">きめ細かなコント ローラー インスタンスを作成して新しい IControllerActivator インターフェイス</span><span class="sxs-lookup"><span data-stu-id="2ed36-599">New IControllerActivator Interface for Fine-Grained Controller Instantiation</span></span>

<span data-ttu-id="2ed36-600">新しい IControllerActivator インターフェイスは、依存関係の挿入を使用してコント ローラーがインスタンス化する方法をより細かく管理を提供します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-600">The new IControllerActivator interface provides more fine-grained control over how controllers are instantiated via dependency injection.</span></span> <span data-ttu-id="2ed36-601">次の例は、インターフェイスを示しています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-601">The following example shows the interface:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

<span data-ttu-id="2ed36-602">コント ローラー ファクトリの役割には、この操作を比較します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-602">Contrast this to the role of the controller factory.</span></span> <span data-ttu-id="2ed36-603">コント ローラー ファクトリは、コント ローラーの種類を検索するため、そのコント ローラーの種類のインスタンスをインスタンス化するための両方を担当する IControllerFactory インターフェイスの実装です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-603">A controller factory is an implementation of the IControllerFactory interface, which is responsible both for locating a controller type and for instantiating an instance of that controller type.</span></span>

<span data-ttu-id="2ed36-604">コント ローラー アクティベーターがコント ローラーの種類のインスタンスをインスタンス化のみを担当します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-604">Controller activators are responsible only for instantiating an instance of a controller type.</span></span> <span data-ttu-id="2ed36-605">コント ローラーの種類の参照は行いません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-605">They do not perform the controller type lookup.</span></span> <span data-ttu-id="2ed36-606">適切なコント ローラーの種類を特定すると、コント ローラー ファクトリは、コント ローラーの実際のインスタンス化を処理するために IControllerActivator のインスタンスに委任する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-606">After locating a proper controller type, controller factories should delegate to an instance of IControllerActivator to handle the actual instantiation of the controller.</span></span>

<span data-ttu-id="2ed36-607">DefaultControllerFactory クラスには、IControllerFactory インスタンスを受け取る新しいコンス トラクターがあります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-607">The DefaultControllerFactory class has a new constructor that accepts an IControllerFactory instance.</span></span> <span data-ttu-id="2ed36-608">既定のコント ローラーの種類のルックアップ動作をオーバーライドすることがなく、コント ローラーの作成のこの側面を管理する依存関係の挿入を適用できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-608">This lets you apply Dependency Injection to manage this aspect of controller creation without having to override the default controller-type lookup behavior.</span></span>

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a><span data-ttu-id="2ed36-609">IDependencyResolver を置き換え IServiceLocator インターフェイス</span><span class="sxs-lookup"><span data-stu-id="2ed36-609">IServiceLocator Interface Replaced with IDependencyResolver</span></span>

<span data-ttu-id="2ed36-610">コミュニティからのフィードバックに基づき、ASP.NET MVC 3 のベータ リリースは、ASP.NET MVC のニーズに応じたスリムにした IDependencyResolver インターフェイスを持つ、IServiceLocator インターフェイスの使用を交換することが。</span><span class="sxs-lookup"><span data-stu-id="2ed36-610">Based on community feedback, the ASP.NET MVC 3 Beta release has replaced the use of the IServiceLocator interface with a slimmed-down IDependencyResolver interface specific to the needs of ASP.NET MVC.</span></span> <span data-ttu-id="2ed36-611">次の例は、新しいインターフェイスを示しています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-611">The following example shows the new interface:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

<span data-ttu-id="2ed36-612">この変更の一環として、DependencyResolver クラスを使用して、ServiceLocator クラスも置き換えられました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-612">As part of this change, the ServiceLocator class was also replaced with the DependencyResolver class.</span></span> <span data-ttu-id="2ed36-613">依存関係競合回避モジュールの登録は、ASP.NET MVC の以前のバージョンと同様です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-613">Registration of a dependency resolver is similar to earlier versions of ASP.NET MVC:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

<span data-ttu-id="2ed36-614">このインターフェイスの実装は、要求された型に対して登録されているサービスを提供する基になる依存関係挿入コンテナーに単に委任必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-614">Implementations of this interface should simply delegate to the underlying dependency injection container to provide the registered service for the requested type.</span></span>

<span data-ttu-id="2ed36-615">要求された型の登録済みのサービスがない場合は、ASP.NET MVC は GetService から null を返すと、GetServices から空のコレクションを返すには、このインターフェイスの実装が必要です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-615">When there are no registered services of the requested type, ASP.NET MVC expects implementations of this interface to return null from GetService and to return an empty collection from GetServices.</span></span>

<span data-ttu-id="2ed36-616">新しい DependencyResolver クラスを使用して、新しい IDependencyResolver インターフェイスまたは共通サービス ロケーターのインターフェイス (IServiceLocator) のいずれかを実装するクラスを登録できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-616">The new DependencyResolver class lets you register classes that implement either the new IDependencyResolver interface or the Common Service Locator interface (IServiceLocator).</span></span> <span data-ttu-id="2ed36-617">共通サービス ロケーターの詳細については、次を参照してください。 [github CommonServiceLocator](https://github.com/unitycontainer/commonservicelocator)します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-617">For more information about Common Service Locator, see [CommonServiceLocator on GitHub](https://github.com/unitycontainer/commonservicelocator).</span></span>

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a><span data-ttu-id="2ed36-618">詳細なビュー ページ インスタンス化用の新しい IViewActivator インターフェイス</span><span class="sxs-lookup"><span data-stu-id="2ed36-618">New IViewActivator Interface for Fine-Grained View Page Instantiation</span></span>

<span data-ttu-id="2ed36-619">新しい IViewPageActivator インターフェイスは、依存関係の挿入を使用してページの表示がインスタンス化する方法をより細かく管理を提供します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-619">The new IViewPageActivator interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="2ed36-620">これは、WebFormView インスタンスと RazorView インスタンスの両方に適用されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-620">This applies to both WebFormView instances and RazorView instances.</span></span> <span data-ttu-id="2ed36-621">次の例は、新しいインターフェイスを示しています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-621">The following example shows the new interface:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

<span data-ttu-id="2ed36-622">これらのクラスは、できる ViewPage、ViewUserControl、WebViewPage 型がインスタンス化する方法を制御するために依存関係の挿入を使用する、IViewPageActivator コンス トラクター引数を受け入れるようになりました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-622">These classes now accept an IViewPageActivator constructor argument, which lets you use dependency injection to control how the ViewPage, ViewUserControl, and WebViewPage types are instantiated.</span></span>

#### <a name="new-dependency-resolver-support-for-existing-services"></a><span data-ttu-id="2ed36-623">既存のサービスの新しい依存関係競合回避モジュールのサポート</span><span class="sxs-lookup"><span data-stu-id="2ed36-623">New Dependency Resolver Support for Existing Services</span></span>

<span data-ttu-id="2ed36-624">新しいリリースには、次のサービスの依存関係解決のサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-624">The new release includes dependency resolution support for the following services:</span></span>

- <span data-ttu-id="2ed36-625">モデル検証プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="2ed36-625">Model validation providers.</span></span> <span data-ttu-id="2ed36-626">ModelValidatorProvider を実装するクラスは、依存関係競合回避モジュールに登録することができ、システムがクライアント側とサーバー側の検証をサポートするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-626">Classes that implement ModelValidatorProvider can be registered in the dependency resolver, and the system will use them to support client- and server-side validation.</span></span>
- <span data-ttu-id="2ed36-627">モデル メタデータ プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="2ed36-627">Model metadata provider.</span></span> <span data-ttu-id="2ed36-628">ModelMetadataProvider を実装する 1 つのクラスは、依存関係競合回避モジュールに登録することができ、システムは、テンプレート、および検証システムのメタデータを提供することを使用します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-628">A single class that implements ModelMetadataProvider can be registered in the dependency resolver, and the system will use it to provide metadata for the templating and validation systems.</span></span>
- <span data-ttu-id="2ed36-629">値プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="2ed36-629">Value providers.</span></span> <span data-ttu-id="2ed36-630">ValueProviderFactory を実装するクラスは、依存関係競合回避モジュールに登録することができ、システムが、コント ローラーによって、モデル バインド中に使用される値プロバイダーの作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-630">Classes that implement ValueProviderFactory can be registered in the dependency resolver, and the system will use them to create value providers that are consumed by the controller and during model binding.</span></span>
- <span data-ttu-id="2ed36-631">モデル バインダー。</span><span class="sxs-lookup"><span data-stu-id="2ed36-631">Model binders.</span></span> <span data-ttu-id="2ed36-632">IModelBinderProvider を実装するクラスは、依存関係競合回避モジュールに登録することができ、システムが、モデル バインド システムで使用されるモデル バインダーの作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-632">Classes that implement IModelBinderProvider can be registered in the dependency resolver, and the system will use them to create model binders that are consumed by the model binding system.</span></span>

### <a id="0.1__Toc274034221"></a>  <span data-ttu-id="2ed36-633">控えめな jQuery ベースの Ajax のサポート</span><span class="sxs-lookup"><span data-stu-id="2ed36-633">New Support for Unobtrusive jQuery-Based Ajax</span></span>

<span data-ttu-id="2ed36-634">ASP.NET MVC には、次などの Ajax ヘルパー メソッドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-634">ASP.NET MVC includes Ajax helper methods such as the following:</span></span>

- <span data-ttu-id="2ed36-635">Ajax.ActionLink</span><span class="sxs-lookup"><span data-stu-id="2ed36-635">Ajax.ActionLink</span></span>
- <span data-ttu-id="2ed36-636">Ajax.RouteLink</span><span class="sxs-lookup"><span data-stu-id="2ed36-636">Ajax.RouteLink</span></span>
- <span data-ttu-id="2ed36-637">Ajax.BeginForm</span><span class="sxs-lookup"><span data-stu-id="2ed36-637">Ajax.BeginForm</span></span>
- <span data-ttu-id="2ed36-638">Ajax.BeginRouteForm</span><span class="sxs-lookup"><span data-stu-id="2ed36-638">Ajax.BeginRouteForm</span></span>

<span data-ttu-id="2ed36-639">これらのメソッドでは、JavaScript を使用して、完全なポストバックを使用するのではなく、サーバーに、アクション メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-639">These methods use JavaScript to invoke an action method on the server rather than using a full postback.</span></span> <span data-ttu-id="2ed36-640">この機能が活用するために jQuery unobtrusive の方法で更新されました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-640">This functionality has been updated to take advantage of jQuery in an unobtrusive manner.</span></span> <span data-ttu-id="2ed36-641">インライン クライアント スクリプトを生成するそのままの状態ではなくこれらのヘルパー メソッドの動作から分離マークアップを使用して HTML5 属性を生成することによって、*データ ajax*プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="2ed36-641">Rather than intrusively emitting inline client scripts, these helper methods separate the behavior from the markup by emitting HTML5 attributes using the *data-ajax* prefix.</span></span> <span data-ttu-id="2ed36-642">動作は、適切な JavaScript ファイルを参照することによって、マークアップに適用されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-642">Behavior is then applied to the markup by referencing the appropriate JavaScript files.</span></span> <span data-ttu-id="2ed36-643">次の JavaScript ファイルが参照されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-643">Make sure that the following JavaScript files are referenced:</span></span>

- <span data-ttu-id="2ed36-644">jquery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="2ed36-644">jquery-1.4.1.js</span></span>
- <span data-ttu-id="2ed36-645">jquery.unobtrusive.ajax.js</span><span class="sxs-lookup"><span data-stu-id="2ed36-645">jquery.unobtrusive.ajax.js</span></span>

<span data-ttu-id="2ed36-646">この機能は、新規の ASP.NET MVC 3 プロジェクト テンプレートでは、Web.config ファイルで既定で有効ですが、既存のプロジェクトは既定では無効します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-646">This feature is enabled by default in the Web.config file in the ASP.NET MVC 3 new project templates, but is disabled by default for existing projects.</span></span> <span data-ttu-id="2ed36-647">詳細については、次を参照してください。[のクライアント検証と控えめな JavaScript アプリケーション全体のフラグを追加](#0.1_AddedApplicationWideFlagsForClientValida)このドキュメントで後述します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-647">For more information, see [Added application-wide flags for client validation and unobtrusive JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) later in this document.</span></span>

### <a id="0.1__Toc274034222"></a>  <span data-ttu-id="2ed36-648">控えめな jQuery 検証のサポート</span><span class="sxs-lookup"><span data-stu-id="2ed36-648">New Support for Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="2ed36-649">既定では、ASP.NET MVC 3 のベータ版は、控えめな方法でクライアント側の検証を実行するには jQuery の検証を使用します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-649">By default, ASP.NET MVC 3 Beta uses jQuery validation in an unobtrusive manner in order to perform client-side validation.</span></span> <span data-ttu-id="2ed36-650">控えめなクライアント検証を有効にするには、ビュー内から次のような呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="2ed36-650">To enable unobtrusive client validation, make a call like the following from within a view:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

<span data-ttu-id="2ed36-651">これは、ViewContext.UnobtrusiveJavaScriptEnabled プロパティが、次の呼び出しを行うにはできる true に設定されていることが必要です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-651">This requires that ViewContext.UnobtrusiveJavaScriptEnabled property is set to true, which you can do by making the following call:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

<span data-ttu-id="2ed36-652">また、次の JavaScript ファイルが参照されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="2ed36-652">Also make sure the following JavaScript files are referenced.</span></span>

- <span data-ttu-id="2ed36-653">jquery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="2ed36-653">jquery-1.4.1.js</span></span>
- <span data-ttu-id="2ed36-654">jquery.validate.js</span><span class="sxs-lookup"><span data-stu-id="2ed36-654">jquery.validate.js</span></span>
- <span data-ttu-id="2ed36-655">jquery.validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="2ed36-655">jquery.validate.unobtrusive.js</span></span>

<span data-ttu-id="2ed36-656">この機能は、新規の ASP.NET MVC 3 プロジェクト テンプレートでは、Web.config ファイルでは既定で有効になっているが、既存のプロジェクトは既定では無効になります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-656">This feature is enabled on by default in the Web.config file in the ASP.NET MVC 3 new project templates, but is disabled by default for existing projects.</span></span> <span data-ttu-id="2ed36-657">詳細については、次を参照してください。[のクライアント検証と控えめな JavaScript アプリケーション全体のフラグを新しい](#0.1_AddedApplicationWideFlagsForClientValida)このドキュメントで後述します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-657">For more information, see [New application-wide flags for client validation and unobtrusive JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) later in this document.</span></span>

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  <span data-ttu-id="2ed36-658">クライアント検証と控えめな JavaScript の新しいアプリケーション全体のフラグ</span><span class="sxs-lookup"><span data-stu-id="2ed36-658">New Application-Wide Flags for Client Validation and Unobtrusive JavaScript</span></span>

<span data-ttu-id="2ed36-659">有効にするか、クライアントの検証と控えめな JavaScript の次の例のように、HtmlHelper クラスの静的メンバーを使用してグローバルに無効にします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-659">You can enable or disable client validation and unobtrusive JavaScript globally using static members of the HtmlHelper class, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

<span data-ttu-id="2ed36-660">既定のプロジェクト テンプレートは、既定では、控えめな JavaScript を有効にします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-660">The default project templates enable unobtrusive JavaScript by default.</span></span> <span data-ttu-id="2ed36-661">有効にするか、次の設定を使用して、アプリケーションのルート Web.config ファイルでこれらの機能を無効にします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-661">You can also enable or disable these features in the root Web.config file of your application using the following settings:</span></span>

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

<span data-ttu-id="2ed36-662">既定では、これらの機能を有効にすることができます、ため、新しいオーバー ロードは HtmlHelper クラスに次の例に示すようには、既定の設定を上書きすることが導入されました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-662">Because you can enable these features by default, new overloads were introduced to the HtmlHelper class that let you override the default settings, as shown in the following examples:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

<span data-ttu-id="2ed36-663">旧バージョンと互換性のため、これら両方の機能は既定で無効です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-663">For backward compatibility, both of these features are disabled by default.</span></span>

### <a id="0.1__Toc274034224"></a>  <span data-ttu-id="2ed36-664">ビューの実行前に実行されるコードの新しいサポート</span><span class="sxs-lookup"><span data-stu-id="2ed36-664">New Support for Code that Runs Before Views Run</span></span>

<span data-ttu-id="2ed36-665">という名前のファイルを挿入できるように\_viewstart.cshtml (または\_viewstart.vbhtml) で Views ディレクトリをそのディレクトリとそのサブディレクトリ内の複数のビューの間で共有されるコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-665">You can now put a file named \_viewstart.cshtml (or \_viewstart.vbhtml) in the Views directory and add code to it that will be shared among multiple views in that directory and its subdirectories.</span></span> <span data-ttu-id="2ed36-666">次のコードを配置するなど、 \_viewstart.cshtml ~/Views フォルダー ページ。</span><span class="sxs-lookup"><span data-stu-id="2ed36-666">For example, you might put the following code into the \_viewstart.cshtml page in the ~/Views folder:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

<span data-ttu-id="2ed36-667">これには、Views フォルダー内のすべてのビューとそのサブフォルダー再帰的にレイアウト ページを設定します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-667">This sets the layout page for every view within the Views folder and all its subfolders recursively.</span></span> <span data-ttu-id="2ed36-668">ときに、ビュー、表示されるコード、 \_viewstart.cshtml ファイルは、コードの表示を実行する前に、実行します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-668">When a view is being rendered, the code in the \_viewstart.cshtml file runs before the view code runs.</span></span> <span data-ttu-id="2ed36-669">\_Viewstart.cshtml コードは、そのフォルダー内のすべてのビューに適用されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-669">The \_viewstart.cshtml code applies to every view in that folder.</span></span>

<span data-ttu-id="2ed36-670">既定では、コードで、 \_viewstart.cshtml ファイルは、任意のサブフォルダー内のビューにも適用されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-670">By default, the code in the \_viewstart.cshtml file also applies to views in any subfolder.</span></span> <span data-ttu-id="2ed36-671">ただし、個別のサブフォルダーでは、独自のバージョンのことが、 \_viewstart.cshtml ファイルの場合も、ローカル バージョンは、優先順位にします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-671">However, individual subfolders can have their own version of the \_viewstart.cshtml file; in that case, the local version takes precedence.</span></span> <span data-ttu-id="2ed36-672">たとえば、HomeController のすべてのビューに共通するコードを実行する配置を\_~/Views/Home フォルダーにある viewstart.cshtml ファイル。</span><span class="sxs-lookup"><span data-stu-id="2ed36-672">For example, to run code that is common to all views for the HomeController, put a \_viewstart.cshtml file in the ~/Views/Home folder.</span></span>

### <a id="0.1__Toc274034225"></a>  <span data-ttu-id="2ed36-673">VBHTML Razor 構文のサポート</span><span class="sxs-lookup"><span data-stu-id="2ed36-673">New Support for the VBHTML Razor Syntax</span></span>

<span data-ttu-id="2ed36-674">ASP.NET MVC の以前のプレビューには、c# に基づく Razor 構文を使用してビューのサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-674">The previous ASP.NET MVC preview included support for views using Razor syntax based on C#.</span></span> <span data-ttu-id="2ed36-675">これらのビューは、.cshtml ファイルの拡張機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-675">These views use the .cshtml file extension.</span></span> <span data-ttu-id="2ed36-676">Razor をサポートするために進行中の作業の一環として、ASP.NET MVC 3 のベータ版では、.vbhtml ファイル拡張機能を使用する Visual Basic での Razor 構文のサポートが導入されています。</span><span class="sxs-lookup"><span data-stu-id="2ed36-676">As part of ongoing work to support Razor, the ASP.NET MVC 3 Beta introduces support for the Razor syntax in Visual Basic, which uses the .vbhtml file extension.</span></span>

<span data-ttu-id="2ed36-677">VBHTML ページで Visual Basic 構文の使用の概要については、次の URL でチュートリアルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="2ed36-677">For an introduction to using Visual Basic syntax in VBHTML pages, see the tutorial at the following URL:</span></span>

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  <span data-ttu-id="2ed36-678">ValidateInputAttribute をより細かく制御</span><span class="sxs-lookup"><span data-stu-id="2ed36-678">More Granular Control over ValidateInputAttribute</span></span>

<span data-ttu-id="2ed36-679">ASP.NET MVC には、受信要求に、可能性のある悪意のある入力が含まれていないことを確認するコアの ASP.NET 要求の検証インフラストラクチャを呼び出す ValidateInputAttribute クラスが常に含まれます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-679">ASP.NET MVC has always included the ValidateInputAttribute class, which invokes the core ASP.NET request validation infrastructure to make sure that the incoming request does not contain potentially malicious input.</span></span> <span data-ttu-id="2ed36-680">既定では、入力の検証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-680">By default, input validation is enabled.</span></span> <span data-ttu-id="2ed36-681">次の例のように、ValidateInputAttribute 属性を使用して要求の検証を無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-681">It is possible to disable request validation by using the ValidateInputAttribute attribute, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

<span data-ttu-id="2ed36-682">ただし、多くの web アプリケーションでは、残りのフィールドがないときに、HTML を許可する必要がある個々 のフォーム フィールドがあります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-682">However, many web applications have individual form fields that need to allow HTML, while the remaining fields should not.</span></span> <span data-ttu-id="2ed36-683">ValidateInputAttribute クラスでは、要求の検証に含まれないフィールドの一覧を指定できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-683">The ValidateInputAttribute class now lets you specify a list of fields that should not be included in request validation.</span></span>

<span data-ttu-id="2ed36-684">たとえば、ブログ エンジンを開発している場合、フィールドの本文および概要のマークアップを許可する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-684">For example, if you are developing a blog engine, you might want to allow markup in the Body and Summary fields.</span></span> <span data-ttu-id="2ed36-685">これらのフィールドは、各プロパティの名前 ("Body"および「概要」) に対応する name 属性を持つ 2 つの入力要素によって表される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-685">These fields might be represented by two input element, each with a name attribute corresponding to the property name ("Body" and "Summary").</span></span> <span data-ttu-id="2ed36-686">要求を無効にするのみ、これらのフィールドの検証が (コンマ区切り) 次の例のように、ValidateInput クラスの除外するプロパティの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-686">To disable request validation for these fields only, specify the names (comma-separated) in the Exclude property of the ValidateInput class, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  <span data-ttu-id="2ed36-687">ヘルパーの匿名オブジェクトを使用して指定された HTML 属性名にアンダー スコアをハイフンに変換します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-687">Helpers Convert Underscores to Hyphens for HTML Attribute Names Specified Using Anonymous Objects</span></span>

<span data-ttu-id="2ed36-688">ヘルパー メソッドを使用して、次の例のように、匿名オブジェクトを使用して属性の名前/値ペアを指定できます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-688">Helper methods let you specify attribute name/value pairs using an anonymous object, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

<span data-ttu-id="2ed36-689">ハイフンは、ASP.NET でプロパティの名前を使用できないため、属性名でハイフンを使用するこのアプローチを使用しません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-689">This approach doesn't let you use hyphens in the attribute name, because a hyphen cannot be used for a property name in ASP.NET.</span></span> <span data-ttu-id="2ed36-690">ただし、ハイフンはカスタム HTML5 属性にとって重要です。たとえば、HTML5 では、「データの」プレフィックスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-690">However, hyphens are important for custom HTML5 attributes; for example, HTML5 uses the "data-" prefix.</span></span>

<span data-ttu-id="2ed36-691">同時に、アンダー スコアは、HTML では、属性名は使用できませんが、プロパティ名で有効です。</span><span class="sxs-lookup"><span data-stu-id="2ed36-691">At the same time, underscores cannot be used for attribute names in HTML, but are valid within property names.</span></span> <span data-ttu-id="2ed36-692">そのため、匿名のオブジェクトを使用して属性を指定して、属性名がアンダー スコアを含める場合は、およびに、ヘルパー メソッドは、アンダー スコアをハイフンに変換されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-692">Therefore, if you specify attributes using an anonymous object, and if the attribute names include an underscore,, helper methods will convert the underscores to hyphens.</span></span> <span data-ttu-id="2ed36-693">たとえば、次のヘルパー構文は、アンダー スコアを使用します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-693">For example, the following helper syntax uses an underscore:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

<span data-ttu-id="2ed36-694">前の例は、ヘルパーの実行時に、次のマークアップを表示します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-694">The previous example renders the following markup when the helper runs:</span></span>

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  <span data-ttu-id="2ed36-695">バグの修正</span><span class="sxs-lookup"><span data-stu-id="2ed36-695">Bug Fixes</span></span>

<span data-ttu-id="2ed36-696">EditorFor と DisplayFor テンプレート ヘルパーの既定のオブジェクト テンプレートは、DisplayAttribute.Order プロパティで指定された順序付けできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-696">The default object template for the EditorFor and DisplayFor template helpers now supports the ordering specified in the DisplayAttribute.Order property.</span></span> <span data-ttu-id="2ed36-697">(以前のバージョンで順序の設定が使用されません。)</span><span class="sxs-lookup"><span data-stu-id="2ed36-697">(In previous versions, the Order setting was not used.)</span></span>

<span data-ttu-id="2ed36-698">クライアント検証をここでは、検証属性が適用されているオーバーライドされたプロパティの検証をサポートします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-698">Client validation now supports validation of overridden properties that have validation attributes applied.</span></span>

<span data-ttu-id="2ed36-699">JsonValueProviderFactory は既定で登録されているようになりました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-699">JsonValueProviderFactory is now registered by default.</span></span>

## <a id="0.1__Toc274034229"></a>  <span data-ttu-id="2ed36-700">重大な変更</span><span class="sxs-lookup"><span data-stu-id="2ed36-700">Breaking Changes</span></span>

<span data-ttu-id="2ed36-701">例外フィルターの実行の順序には、同じ順序の値を持つ例外フィルターが変更されました。</span><span class="sxs-lookup"><span data-stu-id="2ed36-701">The order of execution for exception filters has changed for exception filters that have the same Order value.</span></span> <span data-ttu-id="2ed36-702">ASP.NET MVC 2 以降では、上の例外フィルターと同じ順序で、コント ローラー アクション メソッドで例外フィルターの前に実行されたアクション メソッドのようにします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-702">In ASP.NET MVC 2 and earlier, exception filters on the controller with the same Order as those on an action method were executed before the exception filters on the action method.</span></span> <span data-ttu-id="2ed36-703">指定された順序値を指定せずに例外フィルターが適用されたときにこの通常される場合。</span><span class="sxs-lookup"><span data-stu-id="2ed36-703">This would typically be the case when exception filters were applied without a specified Order value.</span></span> <span data-ttu-id="2ed36-704">ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-704">In ASP.NET MVC 3, this order has been reversed so that the most specific exception handler executes first.</span></span> <span data-ttu-id="2ed36-705">以前のバージョンと順序のプロパティが明示的に指定されている場合、フィルターが指定した順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-705">As in earlier versions, if the Order property is explicitly specified, the filters are run in the specified order.</span></span>

## <a id="0.1__Toc274034230"></a>  <span data-ttu-id="2ed36-706">既知の問題</span><span class="sxs-lookup"><span data-stu-id="2ed36-706">Known Issues</span></span>

<span data-ttu-id="2ed36-707">インストール中に、使用許諾契約書への同意 ダイアログ ボックスのものよりも小さなウィンドウで、ライセンス条項が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-707">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>

<span data-ttu-id="2ed36-708">Razor ビューには、IntelliSense のサポートも構文の強調表示はありません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-708">Razor views do not have IntelliSense support nor syntax highlighting.</span></span> <span data-ttu-id="2ed36-709">Visual Studio での Razor 構文のサポートを今後のリリースの一部として含めるされると予想されます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-709">It is anticipated that support for Razor syntax in Visual Studio will be included as part of a later release.</span></span>

<span data-ttu-id="2ed36-710">Razor ビュー (CSHTML ファイル) を編集するとき、 <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> Visual Studio でのコント ローラーに移動 メニュー項目を使用可能にすることはできませんし、コード スニペットではありません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-710">When you are editing a Razor view (CSHTML file), the <a id="0.1__Toc224729061"></a><a id="0.1__Toc238051347"></a> Go To Controller menu item in Visual Studio will not be available, and there are no code snippets.</span></span>

<span data-ttu-id="2ed36-711">使用する場合、@model厳密に型指定された CSHTML を指定する構文を表示、言語固有のショートカットの種類が認識されません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-711">When using the @model syntax to specify a strongly typed CSHTML view, language-specific shortcuts for types are not recognized.</span></span> <span data-ttu-id="2ed36-712">たとえば、 @model int は機能しませんが、 @model Int32 は動作します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-712">For example, @model int will not work, but @model Int32 will work.</span></span> <span data-ttu-id="2ed36-713">このバグの回避策では、モデルの種類を指定する場合は、実際の型名を使用します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-713">The workaround for this bug is to use the actual type name when you specify the model type.</span></span>

<span data-ttu-id="2ed36-714">使用する場合、@model厳密に型指定された CSHTML ビューを指定するための構文 (または@ModelTypeVBHTML 厳密に型指定されたビューを指定)、null 許容型と配列の宣言がサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-714">When using the @model syntax to specify a strongly typed CSHTML view (or @ModelType to specify a strongly typed VBHTML view), nullable types and array declarations are not supported.</span></span> <span data-ttu-id="2ed36-715">たとえば、 @model int? はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-715">For example, @model int? is not supported.</span></span> <span data-ttu-id="2ed36-716">代わりに、`@model Nullable<Int32>`します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-716">Instead, use `@model Nullable<Int32>`.</span></span> <span data-ttu-id="2ed36-717">構文@modelstring[] もサポートされていません。 代わりに、`@model IList<string>`します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-717">The syntax @model string[] is also not supported; instead, use `@model IList<string>`.</span></span>

<span data-ttu-id="2ed36-718">ASP.NET MVC 2 プロジェクトを ASP.NET MVC 3 にアップグレードする場合は、Web.config ファイルの appSettings セクションに、次を追加することを確認してください。</span><span class="sxs-lookup"><span data-stu-id="2ed36-718">When you upgrade an ASP.NET MVC 2 project to ASP.NET MVC 3, make sure to add the following to the appSettings section of the Web.config file:</span></span>

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

<span data-ttu-id="2ed36-719">フォーム認証を常に認証されていないユーザーをリダイレクト ~/Account/ログイン、Web.config で使用されるフォーム認証設定を無視するを原因となる既知の問題があります。回避策では、次のアプリ設定を追加します。</span><span class="sxs-lookup"><span data-stu-id="2ed36-719">There's a known issue that causes Forms Authentication to always redirect unauthenticated users to ~/Account/Login, ignoring the forms authentication setting used in Web.config. The workaround is to add the following app setting.</span></span>

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  <span data-ttu-id="2ed36-720">免責事項</span><span class="sxs-lookup"><span data-stu-id="2ed36-720">Disclaimer</span></span>

<span data-ttu-id="2ed36-721">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="2ed36-721">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="2ed36-722">All rights reserved.</span><span class="sxs-lookup"><span data-stu-id="2ed36-722">All rights reserved.</span></span> <span data-ttu-id="2ed36-723">このドキュメントが提供される"としてのです"。</span><span class="sxs-lookup"><span data-stu-id="2ed36-723">This document is provided "as-is."</span></span> <span data-ttu-id="2ed36-724">情報および見解 URL やその他のインターネット Web サイトの参照を含め、このドキュメントでは、予告なく変更可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2ed36-724">Information and views expressed in this document, including URL and other Internet Web site references, may change without notice.</span></span> <span data-ttu-id="2ed36-725">お客様は、その使用に関するリスクを負うものとします。</span><span class="sxs-lookup"><span data-stu-id="2ed36-725">You bear the risk of using it.</span></span>

<span data-ttu-id="2ed36-726">このドキュメントは、Microsoft 製品の知的財産権に関する法的な権利をお客様に許諾するものではありません。</span><span class="sxs-lookup"><span data-stu-id="2ed36-726">This document does not provide you with any legal rights to any intellectual property in any Microsoft product.</span></span> <span data-ttu-id="2ed36-727">内部的な参照目的に限り、このドキュメントを複製して使用することができます。</span><span class="sxs-lookup"><span data-stu-id="2ed36-727">You may copy and use this document for your internal, reference purposes.</span></span>
