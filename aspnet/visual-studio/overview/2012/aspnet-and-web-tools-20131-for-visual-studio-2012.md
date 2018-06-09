---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: ASP.NET および Web ツール 2013.1 for Visual Studio 2012 のリリース ノート |Microsoft ドキュメント
author: microsoft
description: このドキュメントでは、Visual Studio 2012 用の ASP.NET および Web ツール 2013.1 のリリースについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036428"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="c56cc-103">ASP.NET および Web ツール 2013.1 for Visual Studio 2012 のリリース ノート</span><span class="sxs-lookup"><span data-stu-id="c56cc-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="c56cc-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c56cc-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c56cc-105">このドキュメントでは、Visual Studio 2012 用の ASP.NET および Web ツール 2013.1 のリリースについて説明します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="c56cc-106">目次</span><span class="sxs-lookup"><span data-stu-id="c56cc-106">Contents</span></span>

- [<span data-ttu-id="c56cc-107">インストールに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="c56cc-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="c56cc-108">ソフトウェアの要件</span><span class="sxs-lookup"><span data-stu-id="c56cc-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="c56cc-109">ASP.NET および Web ツール 2013.1 for Visual Studio 2012 の新機能</span><span class="sxs-lookup"><span data-stu-id="c56cc-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="c56cc-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="c56cc-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="c56cc-111">テンプレート</span><span class="sxs-lookup"><span data-stu-id="c56cc-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="c56cc-112">ASP.NET MVC 5 テンプレート</span><span class="sxs-lookup"><span data-stu-id="c56cc-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="c56cc-113">ASP.NET Web API 2 のテンプレート</span><span class="sxs-lookup"><span data-stu-id="c56cc-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="c56cc-114">項目テンプレート</span><span class="sxs-lookup"><span data-stu-id="c56cc-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="c56cc-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c56cc-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="c56cc-116">ASP.NET のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="c56cc-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="c56cc-117">Razor エディター</span><span class="sxs-lookup"><span data-stu-id="c56cc-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="c56cc-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="c56cc-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="c56cc-119">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="c56cc-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="c56cc-120">ASP.NET のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="c56cc-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="c56cc-121">MVC と Web API のスキャフォールディング - HTTP 404 Not Found エラー</span><span class="sxs-lookup"><span data-stu-id="c56cc-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="c56cc-122">Visual Studio Express 2012 for Web がスキャフォールディングされた項目を追加した後の動作を停止します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="c56cc-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="c56cc-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="c56cc-124">サーバー エラーが発生する参照または f5 キーを持つ cshtml ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="c56cc-125">Url 書き換えと Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="c56cc-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="c56cc-126">テンプレート</span><span class="sxs-lookup"><span data-stu-id="c56cc-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="c56cc-127">インストールに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="c56cc-127">Installation Notes</span></span>

<span data-ttu-id="c56cc-128">[インストール](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids)ASP.NET および Web Tools for Visual Studio 2012 2013.1 です。</span><span class="sxs-lookup"><span data-stu-id="c56cc-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="c56cc-129">ソフトウェア要件</span><span class="sxs-lookup"><span data-stu-id="c56cc-129">Software Requirements</span></span>

<span data-ttu-id="c56cc-130">Visual Studio 2012 または Visual Studio Express 2012 for Web のいずれかが必要です。</span><span class="sxs-lookup"><span data-stu-id="c56cc-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="c56cc-131">ASP.NET および Web ツール 2013.1 for Visual Studio 2012 の新機能</span><span class="sxs-lookup"><span data-stu-id="c56cc-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="c56cc-132">ブートス トラップ</span><span class="sxs-lookup"><span data-stu-id="c56cc-132">Bootstrap</span></span>

<span data-ttu-id="c56cc-133">MVC 5 コント ローラーとビューをスキャフォールディングする際、ビューのマークアップを使用して[ブートス トラップ](http://getbootstrap.com/)です。</span><span class="sxs-lookup"><span data-stu-id="c56cc-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="c56cc-134">テンプレート</span><span class="sxs-lookup"><span data-stu-id="c56cc-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="c56cc-135">ASP.NET MVC 5 テンプレート</span><span class="sxs-lookup"><span data-stu-id="c56cc-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="c56cc-136">新しい MVC 5 のテンプレートが追加されました。</span><span class="sxs-lookup"><span data-stu-id="c56cc-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="c56cc-137">最新の MVC 5 の NuGet パッケージが参照され、スキャフォールディングを使用して、コント ローラーとビューを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="c56cc-138">ASP.NET Web API 2 のテンプレート</span><span class="sxs-lookup"><span data-stu-id="c56cc-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="c56cc-139">新しい Web API 2 のテンプレートが追加されました。</span><span class="sxs-lookup"><span data-stu-id="c56cc-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="c56cc-140">最新の Web API 2 の NuGet パッケージが参照され、スキャフォールディングを使用して、コント ローラーとビューを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="c56cc-141">項目テンプレート</span><span class="sxs-lookup"><span data-stu-id="c56cc-141">Item Templates</span></span>

<span data-ttu-id="c56cc-142">MVC 5 ビュー、Web ページ (Razor 3)、および Web API 2 コント ローラー用の新しい項目テンプレートが追加されました。</span><span class="sxs-lookup"><span data-stu-id="c56cc-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="c56cc-143">プロジェクトに新しい項目の追加中に NuGet パッケージの関連する、インストールされます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="c56cc-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c56cc-144">Entity Framework 6</span></span>

<span data-ttu-id="c56cc-145">Entity Framework を使用して、MVC または Web API のコント ローラーをスキャフォールディングする際、Framework 6 を使用します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="c56cc-146">Entity Framework の詳細については、次を参照してください。、 [Entity Framework のバージョン履歴](https://msdn.com/data/jj574253)です。</span><span class="sxs-lookup"><span data-stu-id="c56cc-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="c56cc-147">ダウンロードして、Visual Studio 2012 用の Entity Framework 6 のツールをインストールすることができます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="c56cc-148">参照してください、 [Entity Framework の入手](https://msdn.com/data/ee712906#tooling)です。</span><span class="sxs-lookup"><span data-stu-id="c56cc-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="c56cc-149">ASP.NET のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="c56cc-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="c56cc-150">ASP.NET のスキャフォールディングとは、ASP.NET Web アプリケーションのコード生成フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="c56cc-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="c56cc-151">により、簡単にデータ モデルと連携するプロジェクトに定型コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="c56cc-152">Visual Studio の以前のバージョンでは、スキャフォールディングは、ASP.NET MVC プロジェクトに制限されていました。</span><span class="sxs-lookup"><span data-stu-id="c56cc-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="c56cc-153">この更新で、Web フォームを含む、すべての ASP.NET プロジェクトのスキャフォールディングを使用できます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="c56cc-154">この更新プログラムが、Web フォーム プロジェクトの生成のページをサポートしていませんが、プロジェクトに MVC 依存関係を追加することで、Web フォームのスキャフォールディングを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="c56cc-155">Web フォームのページを生成するためのサポートは、今後の更新で追加されます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="c56cc-156">スキャフォールディングを使用する場合は、必要なすべての依存関係がプロジェクトにインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="c56cc-157">たとえば、ASP.NET Web フォーム プロジェクトを開始して、スキャフォールディングを使用して Web API コント ローラーを追加した場合、必要な NuGet パッケージと参照をプロジェクトに自動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="c56cc-158">MVC のスキャフォールディングを Web フォーム プロジェクトに追加するには、追加、**スキャフォールディングされた新しい項目**選択**MVC 5 の依存関係**ダイアログ ウィンドウでします。</span><span class="sxs-lookup"><span data-stu-id="c56cc-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="c56cc-159">MVC; スキャフォールディングの 2 つのオプションがあります。最小限かつ完全します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="c56cc-160">最低限を選択した場合は、NuGet パッケージの管理と ASP.NET MVC の参照のみがプロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="c56cc-161">[完全] オプションを選択した場合、MVC プロジェクトの必要なコンテンツ ファイルだけでなく、最小の依存関係が追加されます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="c56cc-162">非同期コント ローラーをスキャフォールディングのサポートは、Entity Framework 6 から新しい非同期機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="c56cc-163">詳細な情報およびチュートリアルについては、次を参照してください。 [ASP.NET スキャフォールディング概要](../2013/aspnet-scaffolding-overview.md)です。</span><span class="sxs-lookup"><span data-stu-id="c56cc-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="c56cc-164">これらのチュートリアルを Visual Studio 2013、スキャフォールディングを表示することも ASP.NET および Web ツール 2013.1 for Visual Studio 2012 に適用します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="c56cc-165">Razor エディター</span><span class="sxs-lookup"><span data-stu-id="c56cc-165">Razor Editor</span></span>

<span data-ttu-id="c56cc-166">この更新で、Visual Studio 2012 今すぐ Razor 3 ツール/が編集をサポートします。</span><span class="sxs-lookup"><span data-stu-id="c56cc-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="c56cc-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="c56cc-167">NuGet 2.7</span></span>

<span data-ttu-id="c56cc-168">NuGet 2.7 にはで詳細に説明される新機能の豊富なセットが含まれます[NuGet 2.7 リリース ノート](http://docs.nuget.org/docs/release-notes/nuget-2.7)です。</span><span class="sxs-lookup"><span data-stu-id="c56cc-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="c56cc-169">このバージョンの NuGet では、不足しているパッケージを復元する NuGet を明示的に許可するユーザーの必要性を削除します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="c56cc-170">NuGet 2.7、ユーザーを暗黙的にインストールするときに自動的に足りないパッケージを復元することに同意します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="c56cc-171">ユーザーは、Visual Studio で NuGet 設定によるパッケージの復元から明示的に選択できます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="c56cc-172">この変更は、パッケージの復元の動作方法を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="c56cc-173">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="c56cc-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="c56cc-174">ASP.NET のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="c56cc-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="c56cc-175">MVC と Web API のスキャフォールディング - HTTP 404 Not Found エラー</span><span class="sxs-lookup"><span data-stu-id="c56cc-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="c56cc-176">スキャフォールディングされた項目をプロジェクトに追加するときにエラーが発生した場合、プロジェクトは不整合な状態になることができます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="c56cc-177">いくつかのスキャフォールディングをで行われた変更はロールバックされますが、インストールされている NuGet パッケージなど、他の変更はロールバックされません。</span><span class="sxs-lookup"><span data-stu-id="c56cc-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="c56cc-178">ルーティングの構成の変更がロールバックされた場合、ユーザーはスキャフォールディングされた項目に移動するときに HTTP 404 エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="c56cc-179">MVC のこのエラーを解決する新しいスキャフォールディングされた項目を追加し、MVC 5 の依存関係の選択 (最小または完全のいずれか)。</span><span class="sxs-lookup"><span data-stu-id="c56cc-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="c56cc-180">このプロセスでは、プロジェクトに追加のすべての必要な変更します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="c56cc-181">Web api には、このエラーを解決します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="c56cc-182">次の WebApiConfig クラスをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="c56cc-183">アプリケーションで WebApiConfig.Register を構成\_Global.asax で次のようにメソッドを開始します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="c56cc-184">Visual Studio Express 2012 for Web がスキャフォールディングされた項目を追加した後の動作を停止します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="c56cc-185">Entity Framework (Web API 2 コント ローラーなどのアクション、Entity Framework を使用) とスキャフォールディング項目の追加後に Visual Studio Express 2012 for Web が動作しなくなった場合は Visual Studio Express は、アセンブリのネイティブ イメージの読み込みに失敗したこと可能性System.Web.Extensions に依存します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="c56cc-186">この問題を解決するには、System.Web.Extensions の MSIL のイメージを使用する Visual Studio Express を構成します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="c56cc-187">管理者モードでコマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="c56cc-188">%ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE または %programfiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (64 ビット Windows) 用に移動します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="c56cc-189">VWDExpress.exe.config をテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="c56cc-190">次の行を追加、&lt;構成&gt;/&lt;ランタイム&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="c56cc-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="c56cc-191">Visual Studio の再起動は Express 2012 for Web です。</span><span class="sxs-lookup"><span data-stu-id="c56cc-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="c56cc-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="c56cc-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="c56cc-193">Cshtml ファイル withBrowse WithorF5causes サーバー エラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="c56cc-194">Visual Studio 2012 (または Visual Studio 2012 MVC 5 の場合、作成されたプロジェクトを Visual Studio 2013 で開きます) で MVC 5 のプロジェクトを作成し、Browse With または f5 キーを使用して、cshtml ファイルを表示しようとすると、ときにエラーが発生状態 - を**でサーバー エラー'/' アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="c56cc-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="c56cc-195">サーバーに移動しようとしました。 `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="c56cc-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="c56cc-196">この問題を解決するには、変更、**開始動作**に、プロジェクトで設定**特定のページ**です。</span><span class="sxs-lookup"><span data-stu-id="c56cc-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="c56cc-197">ページの値を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c56cc-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="c56cc-198">この変更を加えたら、f5 キーを選択すると、アプリケーションのルートに移動した (`http://localhost:XXXX`)。</span><span class="sxs-lookup"><span data-stu-id="c56cc-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="c56cc-199">この動作は Visual Studio 2013 での MVC 5 プロジェクトの動作と同じ場所、**現在のページ**設定が開いているページを起動します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="c56cc-200">Url 書き換えと Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="c56cc-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="c56cc-201">ASP.NET Razor 3 または ASP.NET MVC 5 にアップグレードすると、tilde(~) 表記が正しく動作しなく URL 書き換えを使用している場合。</span><span class="sxs-lookup"><span data-stu-id="c56cc-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="c56cc-202">URL 書き換えなど HTML 要素の tilde(~) 表記に影響&lt;A/&gt;、&lt;スクリプト/&gt;、&lt;リンク/&gt;、され、その結果、チルダが不要になったルート ディレクトリにマップします。</span><span class="sxs-lookup"><span data-stu-id="c56cc-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="c56cc-203">たとえば、要求を書き直してください**asp.net/content**に**asp.net**、href 属性に&lt;A href ="~/content/"/&gt;に解決される **/content/コンテンツ/** の代わりに **/** です。</span><span class="sxs-lookup"><span data-stu-id="c56cc-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="c56cc-204">この変更を抑制するには設定、 **IIS\_WasUrlRewritten**を各 Web ページまたは false コンテキスト**アプリケーション\_BeginRequest** Global.asax でします。</span><span class="sxs-lookup"><span data-stu-id="c56cc-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="c56cc-205">テンプレート</span><span class="sxs-lookup"><span data-stu-id="c56cc-205">Templates</span></span>

<span data-ttu-id="c56cc-206">"構成 Web [url] の ASP.NET 4.5 に失敗しました"を示すエラー メッセージを表示するときに ASP.NET MVC プロジェクトを作成するには、Windows 8.1 または Windows Server 2012 R2、Visual Studio の Visual Studio 2012 で</span><span class="sxs-lookup"><span data-stu-id="c56cc-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![構成エラー](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="c56cc-208">これらのリリースの Windows にインストールされているときに、Visual Studio 2012 が ASP.NET 4.5 機能を有効いないために、このエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="c56cc-209">ASP.NET 4.5 を有効にするに記載の手順を実行[Windows の機能のオンまたはオフ](https://windows.microsoft.com/windows-8/turn-windows-features-on-off)です。</span><span class="sxs-lookup"><span data-stu-id="c56cc-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![Windows の機能の無効を切り替える](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="c56cc-211">また、コマンドラインを使用して ASP.NET 4.5 を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="c56cc-212">管理者モードでコマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="c56cc-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="c56cc-213">ASP.NET 4.5 を有効にするのには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c56cc-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
