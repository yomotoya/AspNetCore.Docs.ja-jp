---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: ASP.NET および Web Tools 2013.1 for Visual Studio 2012 のリリース ノート |Microsoft Docs
author: microsoft
description: このドキュメントでは、ASP.NET および Web Tools 2013.1 for Visual Studio 2012 のリリースについて説明します。
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: a0b3d52910ac33c403ecbe2340c12b202c25147b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835787"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="17b33-103">ASP.NET と Web Tools 2013.1 for Visual Studio 2012 のリリース ノート</span><span class="sxs-lookup"><span data-stu-id="17b33-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="17b33-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="17b33-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="17b33-105">このドキュメントでは、ASP.NET および Web Tools 2013.1 for Visual Studio 2012 のリリースについて説明します。</span><span class="sxs-lookup"><span data-stu-id="17b33-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="17b33-106">目次</span><span class="sxs-lookup"><span data-stu-id="17b33-106">Contents</span></span>

- [<span data-ttu-id="17b33-107">インストールに関する注記</span><span class="sxs-lookup"><span data-stu-id="17b33-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="17b33-108">ソフトウェアの要件</span><span class="sxs-lookup"><span data-stu-id="17b33-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="17b33-109">ASP.NET および Web Tools 2013.1 for Visual Studio 2012 の新機能</span><span class="sxs-lookup"><span data-stu-id="17b33-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="17b33-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="17b33-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="17b33-111">テンプレート</span><span class="sxs-lookup"><span data-stu-id="17b33-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="17b33-112">ASP.NET MVC 5 テンプレート</span><span class="sxs-lookup"><span data-stu-id="17b33-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="17b33-113">ASP.NET Web API 2 のテンプレート</span><span class="sxs-lookup"><span data-stu-id="17b33-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="17b33-114">項目テンプレート</span><span class="sxs-lookup"><span data-stu-id="17b33-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="17b33-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="17b33-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="17b33-116">ASP.NET のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="17b33-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="17b33-117">Razor エディター</span><span class="sxs-lookup"><span data-stu-id="17b33-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="17b33-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="17b33-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="17b33-119">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="17b33-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="17b33-120">ASP.NET のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="17b33-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="17b33-121">MVC と Web API スキャフォールティング - HTTP 404 Not Found エラー</span><span class="sxs-lookup"><span data-stu-id="17b33-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="17b33-122">Visual Studio Express 2012 for Web スキャフォールディングされた項目を追加した後の作業を停止します</span><span class="sxs-lookup"><span data-stu-id="17b33-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="17b33-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="17b33-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="17b33-124">サーバー エラーが発生 cshtml ファイルで参照または f5 キーを表示します。</span><span class="sxs-lookup"><span data-stu-id="17b33-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="17b33-125">Url の書き換えと Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="17b33-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="17b33-126">テンプレート</span><span class="sxs-lookup"><span data-stu-id="17b33-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="17b33-127">インストールに関する注記</span><span class="sxs-lookup"><span data-stu-id="17b33-127">Installation Notes</span></span>

<span data-ttu-id="17b33-128">[インストール](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids)ASP.NET および Web Tools 2013.1 for Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="17b33-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="17b33-129">ソフトウェア要件</span><span class="sxs-lookup"><span data-stu-id="17b33-129">Software Requirements</span></span>

<span data-ttu-id="17b33-130">Visual Studio 2012 または Visual Studio Express 2012 for Web のいずれかが必要です。</span><span class="sxs-lookup"><span data-stu-id="17b33-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="17b33-131">ASP.NET および Web Tools 2013.1 for Visual Studio 2012 の新機能</span><span class="sxs-lookup"><span data-stu-id="17b33-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="17b33-132">ブートス トラップ</span><span class="sxs-lookup"><span data-stu-id="17b33-132">Bootstrap</span></span>

<span data-ttu-id="17b33-133">MVC 5 コント ローラーとビューをスキャフォールディングするときに、ビューのマークアップを使用して[ブートス トラップ](http://getbootstrap.com/)します。</span><span class="sxs-lookup"><span data-stu-id="17b33-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="17b33-134">テンプレート</span><span class="sxs-lookup"><span data-stu-id="17b33-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="17b33-135">ASP.NET MVC 5 テンプレート</span><span class="sxs-lookup"><span data-stu-id="17b33-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="17b33-136">新しい MVC 5 テンプレートを追加しました。</span><span class="sxs-lookup"><span data-stu-id="17b33-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="17b33-137">最新の MVC 5 の NuGet パッケージが参照され、スキャフォールディングを使用して、コント ローラーとビューを追加できます。</span><span class="sxs-lookup"><span data-stu-id="17b33-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="17b33-138">ASP.NET Web API 2 のテンプレート</span><span class="sxs-lookup"><span data-stu-id="17b33-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="17b33-139">新しい Web API 2 のテンプレートを追加しました。</span><span class="sxs-lookup"><span data-stu-id="17b33-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="17b33-140">最新の Web API 2 の NuGet パッケージが参照され、スキャフォールディングを使用して、コント ローラーとビューを追加できます。</span><span class="sxs-lookup"><span data-stu-id="17b33-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="17b33-141">項目テンプレート</span><span class="sxs-lookup"><span data-stu-id="17b33-141">Item Templates</span></span>

<span data-ttu-id="17b33-142">MVC 5 ビュー、Web ページ (Razor 3)、および Web API 2 コント ローラーの新しい項目テンプレートを追加しました。</span><span class="sxs-lookup"><span data-stu-id="17b33-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="17b33-143">プロジェクトに新しい項目を追加するときに、関連する NuGet パッケージがインストールします。</span><span class="sxs-lookup"><span data-stu-id="17b33-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="17b33-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="17b33-144">Entity Framework 6</span></span>

<span data-ttu-id="17b33-145">Entity Framework を使用して、MVC または Web API のコント ローラーをスキャフォールディングするときに、Framework 6 を使用します。</span><span class="sxs-lookup"><span data-stu-id="17b33-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="17b33-146">Entity Framework の詳細については、次を参照してください。、 [Entity Framework のバージョン履歴](https://msdn.com/data/jj574253)します。</span><span class="sxs-lookup"><span data-stu-id="17b33-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="17b33-147">ダウンロードして、Visual Studio 2012 用の Entity Framework 6 Tools をインストールすることができますも。</span><span class="sxs-lookup"><span data-stu-id="17b33-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="17b33-148">参照してください、 [Entity Framework の入手](https://msdn.com/data/ee712906#tooling)します。</span><span class="sxs-lookup"><span data-stu-id="17b33-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="17b33-149">ASP.NET のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="17b33-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="17b33-150">ASP.NET のスキャフォールディングは、ASP.NET Web アプリケーションのコード生成フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="17b33-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="17b33-151">これにより、簡単にデータ モデルを操作するプロジェクトに定型コードを追加できます。</span><span class="sxs-lookup"><span data-stu-id="17b33-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="17b33-152">Visual Studio の以前のバージョンでは、スキャフォールディングは、ASP.NET MVC プロジェクトに制限されていました。</span><span class="sxs-lookup"><span data-stu-id="17b33-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="17b33-153">この更新で、Web フォームを含む、任意の ASP.NET プロジェクトのスキャフォールディングを使用できます。</span><span class="sxs-lookup"><span data-stu-id="17b33-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="17b33-154">この更新プログラムが Web フォーム プロジェクトでは、ページの生成をサポートしていませんが、MVC 依存関係をプロジェクトに追加することで、Web フォームでスキャフォールディングを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="17b33-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="17b33-155">Web フォームのページを生成するためのサポートは今後の更新で追加されます。</span><span class="sxs-lookup"><span data-stu-id="17b33-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="17b33-156">スキャフォールディングを使用して、必要なすべての依存関係がプロジェクトにインストールされていることができます。</span><span class="sxs-lookup"><span data-stu-id="17b33-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="17b33-157">たとえば、ASP.NET Web フォーム プロジェクトを開始し、スキャフォールディングを使用して Web API コント ローラーを追加すると場合、必要な NuGet パッケージと参照をプロジェクトに自動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="17b33-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="17b33-158">Web フォーム プロジェクトには、MVC のスキャフォールディングを追加するには、追加、**スキャフォールディングされた新しい項目**選択**MVC 5 依存関係**ダイアログ ウィンドウでします。</span><span class="sxs-lookup"><span data-stu-id="17b33-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="17b33-159">MVC; をスキャフォールディングするための 2 つのオプションがあります。最小限かつ完全です。</span><span class="sxs-lookup"><span data-stu-id="17b33-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="17b33-160">最低限を選択した場合は、NuGet パッケージと ASP.NET MVC の参照のみがプロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="17b33-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="17b33-161">すべてのオプションを選択した場合、MVC プロジェクトに必要なコンテンツ ファイルと、最小の依存関係が追加されます。</span><span class="sxs-lookup"><span data-stu-id="17b33-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="17b33-162">非同期コント ローラーをスキャフォールディングするためのサポートは、Entity Framework 6 からの新しい非同期機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="17b33-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="17b33-163">詳細とチュートリアルについては、次を参照してください。 [ASP.NET スキャフォールディング概要](../2013/aspnet-scaffolding-overview.md)します。</span><span class="sxs-lookup"><span data-stu-id="17b33-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="17b33-164">これらのチュートリアルは、Visual Studio 2013 では、スキャフォールディングを説明しますも ASP.NET と Visual Studio 2012 for Web Tools 2013.1 に適用されます。</span><span class="sxs-lookup"><span data-stu-id="17b33-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="17b33-165">Razor エディター</span><span class="sxs-lookup"><span data-stu-id="17b33-165">Razor Editor</span></span>

<span data-ttu-id="17b33-166">この更新で、Visual Studio 2012 サポート Razor 3 ツール/編集します。</span><span class="sxs-lookup"><span data-stu-id="17b33-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="17b33-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="17b33-167">NuGet 2.7</span></span>

<span data-ttu-id="17b33-168">NuGet 2.7 にはで詳細に説明されている新機能の豊富なセットが含まれます[NuGet 2.7 リリース ノート](http://docs.nuget.org/docs/release-notes/nuget-2.7)します。</span><span class="sxs-lookup"><span data-stu-id="17b33-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="17b33-169">このバージョンの NuGet では、不足しているパッケージを復元する NuGet を明示的に許可するユーザーの必要性を削除します。</span><span class="sxs-lookup"><span data-stu-id="17b33-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="17b33-170">NuGet 2.7 をインストールするときに自動的に不足しているパッケージを復元するユーザーが暗黙的に同意します。</span><span class="sxs-lookup"><span data-stu-id="17b33-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="17b33-171">ユーザーは、Visual Studio で NuGet の設定を使用してパッケージの復元から明示的に除外できます。</span><span class="sxs-lookup"><span data-stu-id="17b33-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="17b33-172">この変更は、パッケージ復元のしくみを簡略化します。</span><span class="sxs-lookup"><span data-stu-id="17b33-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="17b33-173">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="17b33-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="17b33-174">ASP.NET のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="17b33-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="17b33-175">MVC と Web API スキャフォールティング - HTTP 404 Not Found エラー</span><span class="sxs-lookup"><span data-stu-id="17b33-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="17b33-176">スキャフォールディングされた項目をプロジェクトに追加するときにエラーが発生した場合は、プロジェクトは、不整合な状態になることは勧めします。</span><span class="sxs-lookup"><span data-stu-id="17b33-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="17b33-177">一部のスキャフォールディングが行われた変更はロールバックされますが、インストール済みの NuGet パッケージなどの他の変更はロールバックされません。</span><span class="sxs-lookup"><span data-stu-id="17b33-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="17b33-178">ルーティング構成の変更がロールバックされた場合、ユーザーはスキャフォールディングされた項目に移動するときに、HTTP 404 エラーを受信します。</span><span class="sxs-lookup"><span data-stu-id="17b33-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="17b33-179">MVC のこのエラーを修正する新しくスキャフォールディングした項目を追加し、MVC 5 依存関係を選択します (最小または完全のいずれか)。</span><span class="sxs-lookup"><span data-stu-id="17b33-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="17b33-180">このプロセスでは、プロジェクトに追加のすべての必要な変更します。</span><span class="sxs-lookup"><span data-stu-id="17b33-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="17b33-181">Web api には、このエラーを解決します。</span><span class="sxs-lookup"><span data-stu-id="17b33-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="17b33-182">次の WebApiConfig クラスをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="17b33-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="17b33-183">WebApiConfig.Register をアプリケーションで構成\_Global.asax でメソッドを次のように開始します。</span><span class="sxs-lookup"><span data-stu-id="17b33-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="17b33-184">Visual Studio Express 2012 for Web スキャフォールディングされた項目を追加した後の作業を停止します</span><span class="sxs-lookup"><span data-stu-id="17b33-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="17b33-185">Visual Studio Express 2012 for Web は、Entity Framework (Web API 2 コント ローラー アクション、Entity Framework を使用) などでスキャフォールディングされた項目を追加した後の作業を停止する場合は、Visual Studio Express は、アセンブリのネイティブ イメージの読み込みに失敗したことが可能System.Web.Extensions に依存します。</span><span class="sxs-lookup"><span data-stu-id="17b33-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="17b33-186">この問題を解決するには、System.Web.Extensions の MSIL イメージを使用する、Visual Studio Express を構成します。</span><span class="sxs-lookup"><span data-stu-id="17b33-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="17b33-187">管理者モードでコマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="17b33-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="17b33-188">%ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE または %programfiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (の 64 ビット Windows) に移動します。</span><span class="sxs-lookup"><span data-stu-id="17b33-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="17b33-189">VWDExpress.exe.config をテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="17b33-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="17b33-190">下にある次の行を追加、&lt;構成&gt;/&lt;ランタイム&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="17b33-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="17b33-191">For Web Express 2012 を Visual Studio を再起動します。</span><span class="sxs-lookup"><span data-stu-id="17b33-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="17b33-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="17b33-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="17b33-193">Cshtml ファイル withBrowse WithorF5causes サーバー エラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="17b33-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="17b33-194">-を示すエラーが表示されます、MVC 5 プロジェクトを Visual Studio 2012 (または Visual Studio 2013 で作成された Visual Studio 2012、MVC 5 プロジェクトで開く) で作成 cshtml ファイルを参照または f5 キーを使用して表示しようとすると、**でサーバー エラー'/' アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="17b33-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="17b33-195">サーバーに移動しようとしました。 `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="17b33-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="17b33-196">この問題を解決するには、変更、**開始動作**、プロジェクトに設定**特定のページ**。</span><span class="sxs-lookup"><span data-stu-id="17b33-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="17b33-197">ページの値を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="17b33-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="17b33-198">この変更を加えたら、f5 キーを選択すると、アプリケーションのルートに移動します (`http://localhost:XXXX`)。</span><span class="sxs-lookup"><span data-stu-id="17b33-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="17b33-199">この動作は、Visual Studio 2013 での MVC 5 プロジェクトの動作と同じ場所、**現在のページ**設定は、開いているページを起動します。</span><span class="sxs-lookup"><span data-stu-id="17b33-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="17b33-200">Url の書き換えと Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="17b33-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="17b33-201">ASP.NET Razor 3 または ASP.NET MVC 5 にアップグレードすると、tilde(~) 表記法が正しく動作しなく URL の書き換えを使用している場合。</span><span class="sxs-lookup"><span data-stu-id="17b33-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="17b33-202">URL 書き換えの HTML 要素の tilde(~) 表記はなどに影響&lt;A/&gt;、&lt;スクリプト/&gt;、&lt;リンク/&gt;、し、その結果、チルダが不要になったルート ディレクトリにマップします。</span><span class="sxs-lookup"><span data-stu-id="17b33-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="17b33-203">要求を書き換える場合など、 **asp.net/content**に**asp.net**、href 属性に&lt;A href ="~/content/"/&gt;に解決される **/content/コンテンツ/** の代わりに **/** します。</span><span class="sxs-lookup"><span data-stu-id="17b33-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="17b33-204">この変更を抑制するのに設定することができます、 **IIS\_WasUrlRewritten**を各 Web ページまたは false にコンテキスト**アプリケーション\_BeginRequest** Global.asax でします。</span><span class="sxs-lookup"><span data-stu-id="17b33-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="17b33-205">テンプレート</span><span class="sxs-lookup"><span data-stu-id="17b33-205">Templates</span></span>

<span data-ttu-id="17b33-206">作成すると ASP.NET MVC プロジェクトでは、Windows 8.1 または Windows Server 2012 R2 の Visual Studio の Visual Studio 2012 には、"Configuring Web [url] の ASP.NET 4.5 に失敗しました。"を示すエラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="17b33-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![構成エラー](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="17b33-208">Visual Studio 2012 ではこれらのリリースの Windows をインストールしていることを ASP.NET 4.5 機能有効になりませんので、このエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="17b33-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="17b33-209">ASP.NET 4.5 を有効にする」に記載の手順に従います[オンまたはオフにする Windows 機能](https://windows.microsoft.com/windows-8/turn-windows-features-on-off)します。</span><span class="sxs-lookup"><span data-stu-id="17b33-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![Windows の機能を無効を切り替える](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="17b33-211">または、コマンドラインを使用して ASP.NET 4.5 を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="17b33-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="17b33-212">管理者モードでコマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="17b33-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="17b33-213">ASP.NET 4.5 を有効にするのには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="17b33-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
