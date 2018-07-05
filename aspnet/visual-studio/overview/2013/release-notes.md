---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET および Web Tools for Visual Studio 2013 リリース ノート |Microsoft Docs
author: microsoft
description: このドキュメントでは、ASP.NET および Web Tools for Visual Studio 2013 のリリースについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 94050d752348cadf4ed1044e6d8f96834ec981d8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370566"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a><span data-ttu-id="b03ce-103">ASP.NET および Web Tools for Visual Studio 2013 リリース ノート</span><span class="sxs-lookup"><span data-stu-id="b03ce-103">ASP.NET and Web Tools for Visual Studio 2013 Release Notes</span></span>
====================
<span data-ttu-id="b03ce-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b03ce-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="b03ce-105">このドキュメントでは、ASP.NET および Web Tools for Visual Studio 2013 のリリースについて説明します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-105">This document describes the release of ASP.NET and Web Tools for Visual Studio 2013.</span></span>


## <a name="contents"></a><span data-ttu-id="b03ce-106">目次</span><span class="sxs-lookup"><span data-stu-id="b03ce-106">Contents</span></span>

- [<span data-ttu-id="b03ce-107">インストールに関する注記</span><span class="sxs-lookup"><span data-stu-id="b03ce-107">Installation Notes</span></span>](#TOC1)
- [<span data-ttu-id="b03ce-108">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="b03ce-108">Documentation</span></span>](#TOC2)
- [<span data-ttu-id="b03ce-109">ソフトウェアの要件</span><span class="sxs-lookup"><span data-stu-id="b03ce-109">Software Requirements</span></span>](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a><span data-ttu-id="b03ce-110">ASP.NET および Web Tools for Visual Studio 2013 の新機能</span><span class="sxs-lookup"><span data-stu-id="b03ce-110">New Features in ASP.NET and Web Tools for Visual Studio 2013</span></span>

- [<span data-ttu-id="b03ce-111">1 つの ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b03ce-111">One ASP.NET</span></span>](#TOC6)
- [<span data-ttu-id="b03ce-112">新しい Web プロジェクト エクスペリエンス</span><span class="sxs-lookup"><span data-stu-id="b03ce-112">New Web Project Experience</span></span>](#newproj)
- [<span data-ttu-id="b03ce-113">ASP.NET のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="b03ce-113">ASP.NET Scaffolding</span></span>](#scaffold)
- [<span data-ttu-id="b03ce-114">ブラウザー リンク</span><span class="sxs-lookup"><span data-stu-id="b03ce-114">Browser Link</span></span>](#browser-link)
- [<span data-ttu-id="b03ce-115">Visual Studio Web エディターの機能強化</span><span class="sxs-lookup"><span data-stu-id="b03ce-115">Visual Studio Web Editor Enhancements</span></span>](#web-editor)
- [<span data-ttu-id="b03ce-116">Visual Studio での azure App Service Web アプリのサポート</span><span class="sxs-lookup"><span data-stu-id="b03ce-116">Azure App Service Web Apps Support in Visual Studio</span></span>](#waws)
- [<span data-ttu-id="b03ce-117">Web の発行の機能強化</span><span class="sxs-lookup"><span data-stu-id="b03ce-117">Web Publish Enhancements</span></span>](#publish)
- [<span data-ttu-id="b03ce-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="b03ce-118">NuGet 2.7</span></span>](#nuget)
- [<span data-ttu-id="b03ce-119">ASP.NET Web フォーム</span><span class="sxs-lookup"><span data-stu-id="b03ce-119">ASP.NET Web Forms</span></span>](#TOC9)
- [<span data-ttu-id="b03ce-120">ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="b03ce-120">ASP.NET MVC 5</span></span>](#TOC10)
- [<span data-ttu-id="b03ce-121">ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="b03ce-121">ASP.NET Web API 2</span></span>](#TOC11)
- [<span data-ttu-id="b03ce-122">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="b03ce-122">ASP.NET SignalR</span></span>](#TOC13)
- [<span data-ttu-id="b03ce-123">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="b03ce-123">ASP.NET Identity</span></span>](#TOC8)
- [<span data-ttu-id="b03ce-124">Microsoft OWIN コンポーネント</span><span class="sxs-lookup"><span data-stu-id="b03ce-124">Microsoft OWIN Components</span></span>](#TOC7)
- [<span data-ttu-id="b03ce-125">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b03ce-125">Entity Framework 6</span></span>](#ef6)
- [<span data-ttu-id="b03ce-126">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="b03ce-126">ASP.NET Razor 3</span></span>](#TOC14)
- [<span data-ttu-id="b03ce-127">ASP.NET アプリを中断します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-127">ASP.NET App Suspend</span></span>](#TOC15)
- [<span data-ttu-id="b03ce-128">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="b03ce-128">Known Issues and Breaking Changes</span></span>](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a><span data-ttu-id="b03ce-129">インストールに関する注記</span><span class="sxs-lookup"><span data-stu-id="b03ce-129">Installation Notes</span></span>

<span data-ttu-id="b03ce-130">ASP.NET および Web Tools for Visual Studio 2013 でメインのインストーラーにバンドルされて、ダウンロードできます[ここ](https://www.asp.net/downloads)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-130">ASP.NET and Web Tools for Visual Studio 2013 are bundled in the main installer and can be downloaded [here](https://www.asp.net/downloads).</span></span>

<a id="TOC2"></a>
## <a name="documentation"></a><span data-ttu-id="b03ce-131">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="b03ce-131">Documentation</span></span>

<span data-ttu-id="b03ce-132">チュートリアルおよび Visual Studio 2013 の ASP.NET と Web ツールの詳細については、その他の情報は、 [ASP.NET web サイト](https://www.asp.net/)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-132">Tutorials and other information about ASP.NET and Web Tools for Visual Studio 2013 are available from the [ASP.NET web site](https://www.asp.net/).</span></span>

<a id="TOC4"></a>
## <a name="software-requirements"></a><span data-ttu-id="b03ce-133">ソフトウェア要件</span><span class="sxs-lookup"><span data-stu-id="b03ce-133">Software Requirements</span></span>

<span data-ttu-id="b03ce-134">ASP.NET と Web ツール Visual Studio 2013 が必要です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-134">ASP.NET and Web Tools requires Visual Studio 2013.</span></span>

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a><span data-ttu-id="b03ce-135">ASP.NET および Web Tools for Visual Studio 2013 の新機能</span><span class="sxs-lookup"><span data-stu-id="b03ce-135">New Features in ASP.NET and Web Tools for Visual Studio 2013</span></span>

<span data-ttu-id="b03ce-136">次のセクションでは、リリースで導入された機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-136">The following sections describe the features that have been introduced in the release.</span></span>

<a id="TOC6"></a>
## <a name="one-aspnet"></a><span data-ttu-id="b03ce-137">1 つの ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b03ce-137">One ASP.NET</span></span>

<span data-ttu-id="b03ce-138">Visual Studio 2013 のリリースでは、ASP.NET のテクノロジを使用することができますを混在させるし、必要なものと同じに簡単に使用できるエクスペリエンスを統合するための手段を思い出させています。</span><span class="sxs-lookup"><span data-stu-id="b03ce-138">With the release of Visual Studio 2013, we have taken a step towards unifying the experience of using ASP.NET technologies, so that you can easily mix and match the ones you want.</span></span> <span data-ttu-id="b03ce-139">たとえば、MVC を使用してプロジェクトを開始し簡単に後で、プロジェクトに Web フォーム ページを追加したり、Web フォーム プロジェクトで Web Api をスキャフォールディングできます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-139">For example, you can start a project using MVC and easily add Web Forms pages to the project later, or scaffold Web APIs in a Web Forms project.</span></span> <span data-ttu-id="b03ce-140">1 つの ASP.NET はしやすく開発者は ASP.NET で使い慣れた、ことを行います。</span><span class="sxs-lookup"><span data-stu-id="b03ce-140">One ASP.NET is all about making it easier for you as a developer to do the things you love in ASP.NET.</span></span> <span data-ttu-id="b03ce-141">どのようなテクノロジを選択に関係なく 1 つの ASP.NET の信頼された、基になる framework でビルドする信頼ことができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-141">No matter what technology you choose, you can have confidence that you are building on the trusted underlying framework of One ASP.NET.</span></span>

<a id="newproj"></a>
## <a name="new-web-project-experience"></a><span data-ttu-id="b03ce-142">新しい Web プロジェクト エクスペリエンス</span><span class="sxs-lookup"><span data-stu-id="b03ce-142">New Web Project Experience</span></span>

<span data-ttu-id="b03ce-143">Visual Studio 2013 で新しい web プロジェクトの作成の強化されています。</span><span class="sxs-lookup"><span data-stu-id="b03ce-143">We have enhanced the experience of creating new web projects in Visual Studio 2013.</span></span> <span data-ttu-id="b03ce-144">**新しい ASP.NET Web プロジェクト**ダイアログし、テクノロジ (Web フォーム、MVC、Web API) の任意の組み合わせを構成、認証のオプションを構成して、単体テスト プロジェクトを追加、プロジェクトの種類を選択することができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-144">In the **New ASP.NET Web Project** dialog you can select the project type you want, configure any combination of technologies (Web Forms, MVC, Web API), configure authentication options, and add a unit test project.</span></span>

![新しい ASP.NET プロジェクト](release-notes/_static/image1.png)

<span data-ttu-id="b03ce-146">新しいダイアログでは、テンプレートの多くの既定の認証オプションを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-146">The new dialog enables you to change the default authentication options for many of the templates.</span></span> <span data-ttu-id="b03ce-147">たとえば、ASP.NET Web フォーム プロジェクトを作成するときに、次のオプションのいずれかを選択できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-147">For example, when you create an ASP.NET Web Forms project you can select any of the following options:</span></span>

- <span data-ttu-id="b03ce-148">認証なし</span><span class="sxs-lookup"><span data-stu-id="b03ce-148">No Authentication</span></span>
- <span data-ttu-id="b03ce-149">個々 のユーザー アカウント (ASP.NET メンバーシップまたはソーシャル プロバイダー ログ)</span><span class="sxs-lookup"><span data-stu-id="b03ce-149">Individual User Accounts (ASP.NET membership or social provider log in)</span></span>
- <span data-ttu-id="b03ce-150">組織アカウント (インターネット アプリケーションでは Active Directory)</span><span class="sxs-lookup"><span data-stu-id="b03ce-150">Organizational Accounts (Active Directory in an internet application)</span></span>
- <span data-ttu-id="b03ce-151">Windows 認証 (イントラネット アプリケーションで Active Directory)</span><span class="sxs-lookup"><span data-stu-id="b03ce-151">Windows Authentication (Active Directory in an intranet application)</span></span>

![認証オプション](release-notes/_static/image2.png)

<span data-ttu-id="b03ce-153">Web プロジェクトを作成するための新しいプロセスの詳細については、次を参照してください。 [Visual Studio 2013 で ASP.NET Web プロジェクトの作成](creating-web-projects-in-visual-studio.md)です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-153">For more information about the new process for creating web projects, see [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span></span> <span data-ttu-id="b03ce-154">新しい認証オプションの詳細については、次を参照してください。 [ASP.NET Identity](#TOC8)このドキュメントで後述します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-154">For more information about the new authentication options, see [ASP.NET Identity](#TOC8) later in this document.</span></span>

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a><span data-ttu-id="b03ce-155">ASP.NET のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="b03ce-155">ASP.NET Scaffolding</span></span>

<span data-ttu-id="b03ce-156">ASP.NET のスキャフォールディングは、ASP.NET Web アプリケーションのコード生成フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="b03ce-156">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="b03ce-157">これにより、簡単にデータ モデルを操作するプロジェクトに定型コードを追加できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-157">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="b03ce-158">Visual Studio の以前のバージョンでは、スキャフォールディングは、ASP.NET MVC プロジェクトに制限されていました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-158">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="b03ce-159">Visual Studio 2013 では、Web フォームを含む、任意の ASP.NET プロジェクトのスキャフォールディングを使用できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-159">With Visual Studio 2013, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="b03ce-160">Visual Studio 2013 はサポートされていないページを生成する Web フォーム プロジェクトの場合は、MVC 依存関係をプロジェクトに追加することで、Web フォームでスキャフォールディングを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-160">Visual Studio 2013 does not currently support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="b03ce-161">Web フォームのページを生成するためのサポートは今後の更新で追加されます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-161">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="b03ce-162">スキャフォールディングを使用して、必要なすべての依存関係がプロジェクトにインストールされていることができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-162">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="b03ce-163">たとえば、ASP.NET Web フォーム プロジェクトを開始し、スキャフォールディングを使用して Web API コント ローラーを追加すると場合、必要な NuGet パッケージと参照をプロジェクトに自動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-163">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="b03ce-164">Web フォーム プロジェクトには、MVC のスキャフォールディングを追加するには、追加、**スキャフォールディングされた新しい項目**選択**MVC 5 依存関係**ダイアログ ウィンドウでします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-164">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="b03ce-165">MVC; をスキャフォールディングするための 2 つのオプションがあります。最小限かつ完全です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-165">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="b03ce-166">最低限を選択した場合は、NuGet パッケージと ASP.NET MVC の参照のみがプロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-166">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="b03ce-167">すべてのオプションを選択した場合、MVC プロジェクトに必要なコンテンツ ファイルと、最小の依存関係が追加されます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-167">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="b03ce-168">非同期コント ローラーをスキャフォールディングするためのサポートは、Entity Framework 6 からの新しい非同期機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-168">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="b03ce-169">詳細とチュートリアルについては、次を参照してください。 [ASP.NET スキャフォールディング概要](aspnet-scaffolding-overview.md)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-169">For more information and tutorials, see [ASP.NET Scaffolding Overview](aspnet-scaffolding-overview.md).</span></span>

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a><span data-ttu-id="b03ce-170">ブラウザー リンク – ブラウザーと Visual Studio の間の SignalR チャネル</span><span class="sxs-lookup"><span data-stu-id="b03ce-170">Browser Link – SignalR channel between browser and Visual Studio</span></span>

<span data-ttu-id="b03ce-171">新しい[Browser Link](using-browser-link.md)機能では、Visual Studio に複数のブラウザーを接続し、ツールバーのボタンをクリックしてすべての更新することができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-171">The new [Browser Link](using-browser-link.md) feature lets you connect multiple browsers to Visual Studio and refresh them all by clicking a button in the toolbar.</span></span> <span data-ttu-id="b03ce-172">モバイル エミュレーターの場合を含め、開発サイトに複数のブラウザーを接続でき、同時にすべての更新、更新をすべてのブラウザーをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-172">You can connect multiple browsers to your development site, including mobile emulators, and click refresh to refresh all the browsers all at the same time.</span></span> <span data-ttu-id="b03ce-173">ブラウザー リンクには、ブラウザー リンク拡張機能を記述する開発者を有効にするための API も公開します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-173">Browser Link also exposes an API to enable developers to write Browser Link extensions.</span></span>

![](release-notes/_static/image3.png)

<span data-ttu-id="b03ce-174">ブラウザー リンク API を活用するために開発者を有効にするには、Visual Studio と接続されている任意のブラウザー間の境界を越える非常に高度なシナリオを作成することなります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-174">By enabling developers to take advantage of the Browser Link API, it becomes possible to create very advanced scenarios that crosses boundaries between Visual Studio and any browser that's connected.</span></span> <span data-ttu-id="b03ce-175">Web Essentials では、Visual Studio とブラウザーの開発者ツール、リモート モバイル エミュレーターと、はるかに多くの制御の統合されたエクスペリエンスを作成する API を利用します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-175">Web Essentials takes advantage of the API to create an integrated experience between Visual Studio and the browser's developer tools, remote controlling mobile emulators and a lot more.</span></span>

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a><span data-ttu-id="b03ce-176">Visual Studio Web エディターの機能強化</span><span class="sxs-lookup"><span data-stu-id="b03ce-176">Visual Studio Web Editor Enhancements</span></span>

<span data-ttu-id="b03ce-177">Visual Studio 2013 には、web アプリケーションでの Razor ファイルや HTML ファイルに新しい HTML エディターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b03ce-177">Visual Studio 2013 includes a new HTML editor for Razor files and HTML files in web applications.</span></span> <span data-ttu-id="b03ce-178">新しい HTML エディターは、HTML5 に基づいて 1 つの統一されたスキーマを提供します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-178">The new HTML editor provides a single unified schema based on HTML5.</span></span> <span data-ttu-id="b03ce-179">自動かっこ挿入、jQuery UI および IntelliSense の属性、属性 IntelliSense のグループ化、ID とクラス名、Intellisense、書式設定のパフォーマンスを高めるための他の強化、AngularJS とスマート タグ。</span><span class="sxs-lookup"><span data-stu-id="b03ce-179">It has automatic brace completion, jQuery UI and AngularJS attribute IntelliSense, attribute IntelliSense Grouping, ID and class name Intellisense, and other improvements including better performance, formatting and SmartTags.</span></span>

<span data-ttu-id="b03ce-180">次のスクリーン ショットでは、HTML エディターでのブートス トラップ属性 IntelliSense の使用方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-180">The following screenshot demonstrates using Bootstrap attribute IntelliSense in the HTML editor.</span></span>

![HTML エディターの Intellisense](release-notes/_static/image4.png)

<span data-ttu-id="b03ce-182">Visual Studio 2013 には、両方 CoffeeScript エディターが組み込まれているとも付属しています。</span><span class="sxs-lookup"><span data-stu-id="b03ce-182">Visual Studio 2013 also comes with both CoffeeScript and LESS editors built in.</span></span> <span data-ttu-id="b03ce-183">LESS エディター、CSS エディターからすべての優れた機能が付属し、変数、mixin の特定の Intellisense を少ないすべてのドキュメントでは、@importチェーン。</span><span class="sxs-lookup"><span data-stu-id="b03ce-183">The LESS editor comes with all the cool features from the CSS editor and has specific Intellisense for variables and mixins across all the LESS documents in the @import chain.</span></span>

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a><span data-ttu-id="b03ce-184">Visual Studio での azure App Service Web アプリのサポート</span><span class="sxs-lookup"><span data-stu-id="b03ce-184">Azure App Service Web Apps Support in Visual Studio</span></span>

<span data-ttu-id="b03ce-185">Azure SDK for .NET 2.2 で Visual Studio 2013 で使用できます**サーバー エクスプ ローラー**リモートの web アプリと直接対話します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-185">In Visual Studio 2013 with the Azure SDK for .NET 2.2, you can use **Server Explorer** to interact directly with your remote web apps.</span></span> <span data-ttu-id="b03ce-186">Azure アカウントにサインインして新しい web アプリを作成、アプリを構成するか、リアルタイムのログと詳細を表示できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-186">You can sign in to your Azure account, create new web apps, configure apps, view real-time logs, and more.</span></span> <span data-ttu-id="b03ce-187">SDK 2.2 後すぐに予定されているがリリースされたら、Azure でリモートでデバッグ モードで実行することができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-187">Coming soon after SDK 2.2 is released, you'll be able to run in debug mode remotely in Azure.</span></span> <span data-ttu-id="b03ce-188">Azure App Service Web Apps の新しい機能のほとんどは、現在のリリースの Azure SDK for .NET をインストールすると、Visual Studio 2012 で動作も。</span><span class="sxs-lookup"><span data-stu-id="b03ce-188">Most of the new features for Azure App Service Web Apps also work in Visual Studio 2012 when you install the current release of the Azure SDK for .NET.</span></span>

<span data-ttu-id="b03ce-189">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="b03ce-189">For more information, see the following resources:</span></span>

- [<span data-ttu-id="b03ce-190">Azure App Service で ASP.NET web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-190">Create an ASP.NET web app in Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [<span data-ttu-id="b03ce-191">Visual Studio を使用した Azure App Service での Web アプリのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="b03ce-191">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a><span data-ttu-id="b03ce-192">Web の発行の機能強化</span><span class="sxs-lookup"><span data-stu-id="b03ce-192">Web Publish Enhancements</span></span>

<span data-ttu-id="b03ce-193">Visual Studio 2013 には、新規および強化された Web の発行の機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b03ce-193">Visual Studio 2013 includes new and enhanced Web Publish features.</span></span> <span data-ttu-id="b03ce-194">その一部を次に示します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-194">Here are a few of them:</span></span>

- <span data-ttu-id="b03ce-195">簡単に[Web.config ファイルの暗号化を自動化](https://go.microsoft.com/fwlink/?LinkId=325529)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-195">Easily [automate Web.config file encryption](https://go.microsoft.com/fwlink/?LinkId=325529).</span></span> <span data-ttu-id="b03ce-196">(このリンクし、次の 2 つ) をポイント 10/17 日の終わりまで使用できない可能性がある MSDN のドキュメント。</span><span class="sxs-lookup"><span data-stu-id="b03ce-196">(This link and the following two point to documentation on MSDN that might not be available until late in the day on 10/17.)</span></span>
- <span data-ttu-id="b03ce-197">簡単に[デプロイ中にオフライン アプリケーションの自動化](https://go.microsoft.com/fwlink/?LinkId=325530)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-197">Easily [automate taking an application offline during deployment](https://go.microsoft.com/fwlink/?LinkId=325530).</span></span>
- <span data-ttu-id="b03ce-198">Web デプロイを構成する[ファイルのチェックサムを使用して、最終変更日ではなく](https://go.microsoft.com/fwlink/?LinkId=325531)ファイルは、サーバーにコピーするかを決定するため。</span><span class="sxs-lookup"><span data-stu-id="b03ce-198">Configure Web Deploy to [use file checksum instead of last-changed date](https://go.microsoft.com/fwlink/?LinkId=325531) for determining which files should be copied to the server.</span></span>
- <span data-ttu-id="b03ce-199">すばやく、FTP を使用しているか、ファイル システム メソッドを公開するときにだけでなく Web 配置には、個々 の選択したファイル (Web.config を含む) を発行します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-199">Quickly publish individual selected files (including Web.config) when you're using the FTP or file system publish methods as well as with Web Deploy.</span></span>

<span data-ttu-id="b03ce-200">ASP.NET web 配置の詳細については、次を参照してください。 [ASP.NET サイト](https://go.microsoft.com/fwlink/?LinkId=322027)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-200">For more information about ASP.NET web deployment, see [the ASP.NET site](https://go.microsoft.com/fwlink/?LinkId=322027).</span></span>

<a id="nuget"></a>
## <a name="nuget-27"></a><span data-ttu-id="b03ce-201">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="b03ce-201">NuGet 2.7</span></span>

<span data-ttu-id="b03ce-202">NuGet 2.7 にはで詳細に説明されている新機能の豊富なセットが含まれます[NuGet 2.7 リリース ノート](http://docs.nuget.org/docs/release-notes/nuget-2.7)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-202">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="b03ce-203">このバージョンの NuGet では、NuGet のパッケージ復元機能パッケージをダウンロードするための明示的な同意を提供する必要性も削除されます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-203">This version of NuGet also removes the need to provide explicit consent for NuGet's package restore feature to download packages.</span></span> <span data-ttu-id="b03ce-204">NuGet をインストールすることで同意 (と NuGet の設定のダイアログ ボックスで、対応するチェック ボックス) が付与されました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-204">Consent (and the associated checkbox in NuGet's preferences dialog) is now granted by installing NuGet.</span></span> <span data-ttu-id="b03ce-205">今すぐパッケージの復元は、既定では単に動作します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-205">Now package restore simply works by default.</span></span>

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a><span data-ttu-id="b03ce-206">ASP.NET Web フォーム</span><span class="sxs-lookup"><span data-stu-id="b03ce-206">ASP.NET Web Forms</span></span>

### <a name="one-aspnet"></a><span data-ttu-id="b03ce-207">1 つの ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b03ce-207">One ASP.NET</span></span>

<span data-ttu-id="b03ce-208">Web フォーム プロジェクト テンプレートは、1 つの ASP.NET の新しいエクスペリエンスとシームレスに統合します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-208">The Web Forms project templates integrate seamlessly with the new One ASP.NET experience.</span></span> <span data-ttu-id="b03ce-209">MVC と Web API のサポート、Web フォーム プロジェクトを 1 つの ASP.NET プロジェクトの作成ウィザードを使用して認証を構成することができますを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-209">You can add MVC and Web API support to your Web Forms project, and you can configure authentication using the One ASP.NET project creation wizard.</span></span> <span data-ttu-id="b03ce-210">詳細については、次を参照してください。 [Visual Studio 2013 で ASP.NET Web プロジェクトの作成](creating-web-projects-in-visual-studio.md)です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-210">For more information, see [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span></span>

### <a name="aspnet-identity"></a><span data-ttu-id="b03ce-211">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="b03ce-211">ASP.NET Identity</span></span>

<span data-ttu-id="b03ce-212">Web フォーム プロジェクト テンプレートでは、新しい ASP.NET Identity フレームワークをサポートします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-212">The Web Forms project templates support the new ASP.NET Identity framework.</span></span> <span data-ttu-id="b03ce-213">さらに、テンプレートは、イントラネットの Web フォーム プロジェクトの作成をサポートするようになりました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-213">In addition, the templates now support creation of a Web Forms intranet project.</span></span> <span data-ttu-id="b03ce-214">詳細については、次を参照してください。[認証方法](creating-web-projects-in-visual-studio.md#auth)で**Visual Studio 2013 で ASP.NET Web プロジェクトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-214">For more information, see [Authentication Methods](creating-web-projects-in-visual-studio.md#auth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="bootstrap"></a><span data-ttu-id="b03ce-215">ブートス トラップ</span><span class="sxs-lookup"><span data-stu-id="b03ce-215">Bootstrap</span></span>

<span data-ttu-id="b03ce-216">Web フォーム テンプレートを使用して、[ブートス トラップ](http://twitter.github.io/bootstrap/)や応答性のスマートなルック アンド フィールを簡単にカスタマイズできるを提供します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-216">The Web Forms templates use [Bootstrap](http://twitter.github.io/bootstrap/) to provide a sleek and responsive look and feel that you can easily customize.</span></span> <span data-ttu-id="b03ce-217">詳細については、次を参照してください。 [Visual Studio 2013 web プロジェクト テンプレートを使用すると、ブートス トラップ](creating-web-projects-in-visual-studio.md#bootstrap)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-217">For more information, see [Bootstrap in the Visual Studio 2013 web project templates](creating-web-projects-in-visual-studio.md#bootstrap).</span></span>

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a><span data-ttu-id="b03ce-218">ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="b03ce-218">ASP.NET MVC 5</span></span>

### <a name="one-aspnet"></a><span data-ttu-id="b03ce-219">1 つの ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b03ce-219">One ASP.NET</span></span>

<span data-ttu-id="b03ce-220">Web MVC プロジェクト テンプレートは、1 つの ASP.NET の新しいエクスペリエンスとシームレスに統合します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-220">The Web MVC project templates integrate seamlessly with the new One ASP.NET experience.</span></span> <span data-ttu-id="b03ce-221">MVC プロジェクトをカスタマイズし、1 つの ASP.NET プロジェクトの作成ウィザードを使用して認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-221">You can customize your MVC project and configure authentication using the One ASP.NET project creation wizard.</span></span> <span data-ttu-id="b03ce-222">ASP.NET MVC 5 の入門のチュートリアルをご覧[ASP.NET MVC 5 の概要](../../../mvc/overview/getting-started/introduction/getting-started.md)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-222">An introductory tutorial to ASP.NET MVC 5 can be found at [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="b03ce-223">MVC 5、MVC 4 プロジェクトをアップグレードする方法については、次を参照してください。 [ASP.NET MVC 5 と Web API 2 に、ASP.NET MVC 4 と Web API プロジェクトをアップグレードする方法](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-223">For information on upgrading MVC 4 projects to MVC 5, see [How to Upgrade an ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span></span>

### <a name="aspnet-identity"></a><span data-ttu-id="b03ce-224">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="b03ce-224">ASP.NET Identity</span></span>

<span data-ttu-id="b03ce-225">ASP.NET Identity を使用して認証と id 管理には、MVC プロジェクト テンプレートが更新されました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-225">The MVC project templates have been updated to use ASP.NET Identity for authentication and identity management.</span></span> <span data-ttu-id="b03ce-226">Facebook や Google の認証と新しいメンバーシップ API を備えたチュートリアルをご覧[Facebook と Google の OAuth2 や OpenID サインオンでの ASP.NET MVC 5 アプリケーションの作成](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)と[認証を使用した ASP.NET MVC アプリを作成し、SQL DB と Azure App Service へのデプロイ](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-226">A tutorial featuring Facebook and Google authentication and the new membership API can be found at [Create an ASP.NET MVC 5 App with Facebook and Google OAuth2 and OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) and [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).</span></span>

### <a name="bootstrap"></a><span data-ttu-id="b03ce-227">ブートス トラップ</span><span class="sxs-lookup"><span data-stu-id="b03ce-227">Bootstrap</span></span>

<span data-ttu-id="b03ce-228">使用する MVC プロジェクト テンプレートが更新されました[ブートス トラップ](http://getbootstrap.com/)や応答性のスマートなルック アンド フィールを簡単にカスタマイズできるを提供します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-228">The MVC project template has been updated to use [Bootstrap](http://getbootstrap.com/) to provide a sleek and responsive look and feel that you can easily customize.</span></span> <span data-ttu-id="b03ce-229">詳細については、次を参照してください。 [Visual Studio 2013 web プロジェクト テンプレートを使用すると、ブートス トラップ](creating-web-projects-in-visual-studio.md#bootstrap)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-229">For more information, see [Bootstrap in the Visual Studio 2013 web project templates](creating-web-projects-in-visual-studio.md#bootstrap).</span></span>

### <a name="authentication-filters"></a><span data-ttu-id="b03ce-230">認証フィルター</span><span class="sxs-lookup"><span data-stu-id="b03ce-230">Authentication filters</span></span>

<span data-ttu-id="b03ce-231">認証フィルターは、新しい種類のフィルターを ASP.NET MVC パイプライン内の承認フィルターの前に実行し、認証ロジックごとのアクションを指定することは ASP.NET MVC でコント ローラーごとまたはすべてのコント ローラーに対してグローバルにします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-231">Authentication filters are a new kind of filter in ASP.NET MVC that run prior to authorization filters in the ASP.NET MVC pipeline and allow you to specify authentication logic per-action, per-controller, or globally for all controllers.</span></span> <span data-ttu-id="b03ce-232">認証フィルターは、要求に資格情報を処理し、対応するプリンシパルを指定します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-232">Authentication filters process credentials in the request and provide a corresponding principal.</span></span> <span data-ttu-id="b03ce-233">認証フィルターは、未承認の要求に応答認証チャレンジを追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-233">Authentication filters can also add authentication challenges in response to unauthorized requests.</span></span>

### <a name="filter-overrides"></a><span data-ttu-id="b03ce-234">フィルターのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="b03ce-234">Filter overrides</span></span>

<span data-ttu-id="b03ce-235">オーバーライド フィルターを指定することによって、特定のアクション メソッドまたはコント ローラーに適用するフィルターをオーバーライドすることができますようになりました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-235">You can now override which filters apply to a given action method or controller by specifying an override filter.</span></span> <span data-ttu-id="b03ce-236">オーバーライド フィルターは、特定のスコープ (アクションまたはコント ローラー) で実行するかのフィルターの種類のセットを指定します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-236">Override filters specify a set of filter types that should not be run for a given scope (action or controller).</span></span> <span data-ttu-id="b03ce-237">これにより、グローバルに適用されますが、特定のアクションまたはコント ローラーに適用することから特定のグローバル フィルターを除外するフィルターを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-237">This allows you to configure filters that apply globally but then exclude certain global filters from applying to specific actions or controllers.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="b03ce-238">属性ルーティング</span><span class="sxs-lookup"><span data-stu-id="b03ce-238">Attribute routing</span></span>

<span data-ttu-id="b03ce-239">ASP.NET MVC が Tim McCall の作成者によって投稿物に協力してくれた、属性のルーティングをサポートして今すぐ[ http://attributerouting.net](http://attributerouting.net)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-239">ASP.NET MVC now supports attribute routing, thanks to a contribution by Tim McCall, the author of [http://attributerouting.net](http://attributerouting.net).</span></span> <span data-ttu-id="b03ce-240">属性ルーティングでは、アクションとコント ローラーに注釈を付けることによって、ルートを指定できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-240">With attribute routing you can specify your routes by annotating your actions and controllers.</span></span>

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a><span data-ttu-id="b03ce-241">ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="b03ce-241">ASP.NET Web API 2</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="b03ce-242">属性ルーティング</span><span class="sxs-lookup"><span data-stu-id="b03ce-242">Attribute routing</span></span>

<span data-ttu-id="b03ce-243">ASP.NET Web API が Tim McCall の作成者によって投稿物に協力してくれた、属性のルーティングをサポートして今すぐ[ http://attributerouting.net](http://attributerouting.net)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-243">ASP.NET Web API now supports attribute routing, thanks to a contribution by Tim McCall, the author of [http://attributerouting.net](http://attributerouting.net).</span></span> <span data-ttu-id="b03ce-244">属性ルーティングでこのようなコント ローラー、アクションに注釈を付けることによって、Web API ルートを指定できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-244">With attribute routing you can specify your Web API routes by annotating your actions and controllers like this:</span></span>

[!code-csharp[Main](release-notes/samples/sample1.cs)]

<span data-ttu-id="b03ce-245">属性ルーティングでは、Uri の制御、web API でします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-245">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="b03ce-246">たとえば、リソース階層が単一の API コント ローラーを使用して簡単に定義できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-246">For example, you can easily define a resource hierarchy using a single API controller:</span></span>

[!code-csharp[Main](release-notes/samples/sample2.cs)]

<span data-ttu-id="b03ce-247">属性ルーティングも省略可能なパラメーター、既定値、およびルート制約を指定する便利な構文を提供します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-247">Attribute routing also provides a convenient syntax for specifying optional parameters, default values, and route constraints:</span></span>

[!code-csharp[Main](release-notes/samples/sample3.cs)]

<span data-ttu-id="b03ce-248">属性ルーティングの詳細については、次を参照してください。[属性は、Web API 2 でルーティング](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-248">For more information about attribute routing, see [Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).</span></span>

### <a name="oauth-20"></a><span data-ttu-id="b03ce-249">OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="b03ce-249">OAuth 2.0</span></span>

<span data-ttu-id="b03ce-250">Web API と Single Page Application プロジェクト テンプレートでは、OAuth 2.0 を使用して認証できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-250">The Web API and Single Page Application project templates now support authorization using OAuth 2.0.</span></span> <span data-ttu-id="b03ce-251">OAuth 2.0 は、保護されたリソースへのクライアント アクセスを承認するためのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="b03ce-251">OAuth 2.0 is a framework for authorizing client access to protected resources.</span></span> <span data-ttu-id="b03ce-252">クライアントのブラウザーやモバイル デバイスなどのさまざまな動作します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-252">It works for a variety of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="b03ce-253">OAuth 2.0 のサポートは、新しいセキュリティ ミドルウェア ベアラー認証のため、Microsoft OWIN コンポーネントによって提供され、承認サーバーの役割の実装に基づいています。</span><span class="sxs-lookup"><span data-stu-id="b03ce-253">Support for OAuth 2.0 is based on new security middleware provided by the Microsoft OWIN Components for bearer authentication and implementing the authorization server role.</span></span> <span data-ttu-id="b03ce-254">また、クライアントは、Azure Active Directory または Windows Server 2012 R2 で ADFS などの組織の承認サーバーを使用して承認できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-254">Alternatively, clients can be authorized using an organizational authorization server, such as Azure Active Directory or ADFS in Windows Server 2012 R2.</span></span>

### <a name="odata-improvements"></a><span data-ttu-id="b03ce-255">OData の機能強化</span><span class="sxs-lookup"><span data-stu-id="b03ce-255">OData Improvements</span></span>

<span data-ttu-id="b03ce-256">**$Batch と $value、$select、$ のサポートを展開します。**</span><span class="sxs-lookup"><span data-stu-id="b03ce-256">**Support for $select, $expand, $batch, and $value**</span></span>

<span data-ttu-id="b03ce-257">ASP.NET Web API OData の $select を完全にサポートになりました、$ の展開、$value します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-257">ASP.NET Web API OData now has full support for $select, $expand, and $value.</span></span> <span data-ttu-id="b03ce-258">バッチ処理と変更セットの処理要求の $batch を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-258">You can also use $batch for request batching and processing of change sets.</span></span>

<span data-ttu-id="b03ce-259">$Select および $expand オプションを展開し、OData エンドポイントから返されるデータの形状を変更するようにします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-259">The $select and $expand options let you change the shape of the data that is returned from an OData endpoint.</span></span> <span data-ttu-id="b03ce-260">詳細については、次を参照してください。 [Introducing $select および $expand は、Web API odata のサポートを拡張](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-260">For more information, see [Introducing $select and $expand support in Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).</span></span>

<span data-ttu-id="b03ce-261">**強化された機能拡張**</span><span class="sxs-lookup"><span data-stu-id="b03ce-261">**Improved extensibility**</span></span>

<span data-ttu-id="b03ce-262">OData フォーマッタは、拡張可能なようになりました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-262">The OData formatters are now extensible.</span></span> <span data-ttu-id="b03ce-263">Atom エントリのメタデータを追加して名前付きストリームとメディア リンク エントリをサポートして、インスタンス注釈を追加するか、およびリンクの生成方法をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-263">You can add Atom entry metadata, support named stream and media link entries, add instance annotations, and customize how links are generated.</span></span>

<span data-ttu-id="b03ce-264">**型のないサポート**</span><span class="sxs-lookup"><span data-stu-id="b03ce-264">**Type-less support**</span></span>

<span data-ttu-id="b03ce-265">エンティティ型の CLR 型を定義することがなく OData サービスを構築できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-265">You can now build OData services without needing to define CLR types for your entity types.</span></span> <span data-ttu-id="b03ce-266">代わりに、OData コント ローラーがかかることができますかのインスタンスを返す**IEdmObject**、どの OData フォーマッタは、シリアル化/逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-266">Instead, your OData controllers can take or return instances of **IEdmObject**, which are the OData formatters serialize/deserialize.</span></span>

<span data-ttu-id="b03ce-267">**既存のモデルを再利用します。**</span><span class="sxs-lookup"><span data-stu-id="b03ce-267">**Reuse an existing model**</span></span>

<span data-ttu-id="b03ce-268">既存 entity data model (EDM) を既にある場合するようになりました再利用できる、直接新しいゲートウェイを作成する代わりにします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-268">If you already have an existing entity data model (EDM), you can now reuse it directly, instead of having to build a new one.</span></span> <span data-ttu-id="b03ce-269">たとえば、Entity Framework を使用している場合は、EF によって構築する EDM を使用できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-269">For example, if you are using Entity Framework, you can use the EDM that EF builds for you.</span></span>

### <a name="request-batching"></a><span data-ttu-id="b03ce-270">バッチ処理を要求します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-270">Request Batching</span></span>

<span data-ttu-id="b03ce-271">要求がバッチ処理を少ない頻度の高いユーザー インターフェイスを滑らかにネットワーク トラフィックを削減し、1 つの HTTP POST 要求に複数の操作を結合します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-271">Request batching combines multiple operations into a single HTTP POST request, to reduce network traffic and provide a smoother, less chatty user interface.</span></span> <span data-ttu-id="b03ce-272">ASP.NET Web API では、要求がバッチ処理のいくつかの方法をサポートしているようになりました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-272">ASP.NET Web API now supports several strategies for request batching:</span></span>

- <span data-ttu-id="b03ce-273">OData サービスの $batch エンドポイントを使用します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-273">Use the $batch endpoint of an OData service.</span></span>
- <span data-ttu-id="b03ce-274">1 つの MIME マルチパート要求に複数の要求をパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-274">Package multiple requests into a single MIME multipart request.</span></span>
- <span data-ttu-id="b03ce-275">バッチ処理のカスタム書式指定を使用します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-275">Use a custom batching format.</span></span>

<span data-ttu-id="b03ce-276">要求のバッチ処理を有効にするには、だけの Web API 構成にバッチ処理のハンドラーを持つルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-276">To enable request batching, simply add a route with a batching handler to your Web API configuration:</span></span>

[!code-csharp[Main](release-notes/samples/sample4.cs)]

<span data-ttu-id="b03ce-277">制御することもできるかどうかを要求または順番に、または任意の順序で実行します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-277">You can also control whether requests or executed sequentially or in any order.</span></span>

### <a name="portable-aspnet-web-api-client"></a><span data-ttu-id="b03ce-278">移植可能な ASP.NET Web API クライアント</span><span class="sxs-lookup"><span data-stu-id="b03ce-278">Portable ASP.NET Web API Client</span></span>

<span data-ttu-id="b03ce-279">ASP.NET Web API のクライアントを使用して、Windows ストアおよび Windows Phone 8 アプリケーション間で動作するポータブル クラス ライブラリを作成することができますようになりました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-279">You can now use the ASP.NET Web API Client to create portable class libraries that work across your Windows Store and Windows Phone 8 applications.</span></span> <span data-ttu-id="b03ce-280">クライアントとサーバー間で共有できる移植可能なフォーマッタを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-280">You can also create portable formatters that can be shared across client and server.</span></span>

### <a name="improved-testability"></a><span data-ttu-id="b03ce-281">強化されたテストの容易性</span><span class="sxs-lookup"><span data-stu-id="b03ce-281">Improved Testability</span></span>

<span data-ttu-id="b03ce-282">Web API 2 で、API コント ローラーをテスト ユニットに簡単になります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-282">Web API 2 makes it much easier to unit test your API controllers.</span></span> <span data-ttu-id="b03ce-283">だけで、要求メッセージと構成では、API コント ローラーをインスタンス化してテストするアクション メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-283">Just instantiate your API controller with your request message and configuration, and then call the action method you wish to test.</span></span> <span data-ttu-id="b03ce-284">簡単にたとえるなら、 **UrlHelper**のリンクの生成を実行するアクション メソッドのクラス。</span><span class="sxs-lookup"><span data-stu-id="b03ce-284">It is also easy to mock the **UrlHelper** class, for action methods that perform link generation.</span></span>

### <a name="ihttpactionresult"></a><span data-ttu-id="b03ce-285">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="b03ce-285">IHttpActionResult</span></span>

<span data-ttu-id="b03ce-286">今すぐ、Web API アクション メソッドの結果をカプセル化する IHttpActionResult を実装できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-286">You can now implement IHttpActionResult to encapsulate the result of your Web API action methods.</span></span> <span data-ttu-id="b03ce-287">Web API アクション メソッドから返される、IHttpActionResult は、結果の応答メッセージを生成するために、ASP.NET Web API ランタイムによって実行されます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-287">An IHttpActionResult returned from a Web API action method is executed by the ASP.NET Web API runtime to produce the resultant response message.</span></span> <span data-ttu-id="b03ce-288">単位を簡略化する Web API アクションから返される、IHttpActionResult、Web API の実装をテストします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-288">An IHttpActionResult can be returned from any Web API action to simplify unit testing of your Web API implementation.</span></span> <span data-ttu-id="b03ce-289">参考までに特定のステータス コードを返すため結果を含むさまざまな IHttpActionResult 実装が提供されている、コンテンツまたはコンテンツ ネゴシエーションの応答を書式設定します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-289">For convenience a number of IHttpActionResult implementations are provided out of the box including results for returning specific status codes, formatted content or content-negotiated responses.</span></span>

### <a name="httprequestcontext"></a><span data-ttu-id="b03ce-290">HttpRequestContext</span><span class="sxs-lookup"><span data-stu-id="b03ce-290">HttpRequestContext</span></span>

<span data-ttu-id="b03ce-291">新しい**HttpRequestContext**は要求に関連付けられていますが、要求からすぐにご利用いただけません状態を追跡します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-291">The new **HttpRequestContext** tracks any state that is tied to the request but is not immediately available from the request.</span></span> <span data-ttu-id="b03ce-292">たとえば、使用することができます、 **HttpRequestContext**ルート データ要求、クライアント証明書に関連付けられたプリンシパルを取得する、 **UrlHelper**と仮想パスのルート。</span><span class="sxs-lookup"><span data-stu-id="b03ce-292">For example, you can use the **HttpRequestContext** to get route data, the principal associated with the request, the client certificate, the **UrlHelper** and the virtual path root.</span></span> <span data-ttu-id="b03ce-293">簡単に作成することができます、 **HttpRequestContext**単体テストのためです。</span><span class="sxs-lookup"><span data-stu-id="b03ce-293">You can easily create an **HttpRequestContext** for unit testing purposes.</span></span>

<span data-ttu-id="b03ce-294">要求のプリンシパルが使用する代わりに、要求と共に送られるため、 **Thread.CurrentPrincipal**、Web API パイプラインでは、プリンシパルに、要求の有効期間全体で使用できるがようになりました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-294">Because the principal for the request is flowed with the request instead of relying on **Thread.CurrentPrincipal**, the principal is now available throughout the lifetime of the request while it is in the Web API pipeline.</span></span>

### <a name="cors"></a><span data-ttu-id="b03ce-295">CORS</span><span class="sxs-lookup"><span data-stu-id="b03ce-295">CORS</span></span>

<span data-ttu-id="b03ce-296">Brock Allen から別の優れた投稿に協力してくれた ASP.NET 完全サポート クロス オリジン要求の共有 (CORS) します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-296">Thanks to another great contribution from Brock Allen, ASP.NET now fully supports Cross Origin Request Sharing (CORS).</span></span>

<span data-ttu-id="b03ce-297">ブラウザーのセキュリティは、Web ページが別のドメインに AJAX 要求を行うことを防止します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-297">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="b03ce-298">[CORS](http://www.w3.org/TR/cors/) W3C 標準で、サーバーによる同一オリジン ポリシーの緩和にできるです。</span><span class="sxs-lookup"><span data-stu-id="b03ce-298">[CORS](http://www.w3.org/TR/cors/) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="b03ce-299">CORS を使用することによって、不明なリクエストは拒否しながら、一部のクロス オリジン要求のみを明示的に許可できるようになります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-299">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span>

<span data-ttu-id="b03ce-300">プレフライト要求の自動処理を含む CORS web API 2 をサポートします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-300">Web API 2 now supports CORS, including automatic handling of preflight requests.</span></span> <span data-ttu-id="b03ce-301">詳細については、次を参照してください。 [ASP.NET Web API でのクロス オリジン要求を有効にする](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-301">For more information, see [Enabling Cross-Origin Requests in ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).</span></span>

### <a name="authentication-filters"></a><span data-ttu-id="b03ce-302">認証フィルター</span><span class="sxs-lookup"><span data-stu-id="b03ce-302">Authentication Filters</span></span>

<span data-ttu-id="b03ce-303">認証フィルターは、新しい ASP.NET Web API を ASP.NET Web API パイプラインで承認フィルターの前に実行し、認証ロジックごとのアクションを指定することでフィルターの種類をコント ローラーごとまたはすべてのコント ローラーに対してグローバルにします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-303">Authentication filters are a new kind of filter in ASP.NET Web API that run prior to authorization filters in the ASP.NET Web API pipeline and allow you to specify authentication logic per-action, per-controller, or globally for all controllers.</span></span> <span data-ttu-id="b03ce-304">認証フィルターは、要求に資格情報を処理し、対応するプリンシパルを指定します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-304">Authentication filters process credentials in the request and provide a corresponding principal.</span></span> <span data-ttu-id="b03ce-305">認証フィルターは、未承認の要求に応答認証チャレンジを追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-305">Authentication filters can also add authentication challenges in response to unauthorized requests.</span></span>

### <a name="filter-overrides"></a><span data-ttu-id="b03ce-306">フィルターのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="b03ce-306">Filter Overrides</span></span>

<span data-ttu-id="b03ce-307">オーバーライド フィルターを指定することによって、特定のアクション メソッドまたはコント ローラーに適用するフィルターをオーバーライドすることができますようになりました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-307">You can now override which filters apply to a given action method or controller, by specifying an override filter.</span></span> <span data-ttu-id="b03ce-308">オーバーライド フィルターは、特定のスコープ (アクションまたはコント ローラー) で実行しないでくださいフィルターの種類のセットを指定します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-308">Override filters specify a set of filter types that should not run for a given scope (action or controller).</span></span> <span data-ttu-id="b03ce-309">これにより、グローバル フィルターを追加し、特定のアクションまたはコント ローラーから一部を除外することができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-309">This allows you to add global filters, but then exclude some from specific actions or controllers.</span></span>

### <a name="owin-integration"></a><span data-ttu-id="b03ce-310">OWIN の統合</span><span class="sxs-lookup"><span data-stu-id="b03ce-310">OWIN Integration</span></span>

<span data-ttu-id="b03ce-311">ASP.NET Web API 今すぐ完全 OWIN をサポートの OWIN 対応のホストで実行することができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-311">ASP.NET Web API now fully supports OWIN and can be run on any OWIN capable host.</span></span> <span data-ttu-id="b03ce-312">含まれていますが、 **HostAuthenticationFilter** OWIN 認証システムとの統合を提供します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-312">Also included is a **HostAuthenticationFilter** that provides integration with the OWIN authentication system.</span></span>

<span data-ttu-id="b03ce-313">OWIN の統合、SignalR など、他の OWIN ミドルウェアと共に独自のプロセスで Web API を自己ホストできます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-313">With OWIN integration, you can self-host Web API in your own process alongside other OWIN middleware, such as SignalR.</span></span> <span data-ttu-id="b03ce-314">詳細については、次を参照してください。 [Self-Host ASP.NET Web API を使用して OWIN](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-314">For more information, see [Use OWIN to Self-Host ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a><span data-ttu-id="b03ce-315">ASP.NET SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="b03ce-315">ASP.NET SignalR 2.0</span></span>

<span data-ttu-id="b03ce-316">次のセクションでは、SignalR 2.0 の機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-316">The following sections describe features of SignalR 2.0.</span></span>

- [<span data-ttu-id="b03ce-317">OWIN 上に構築されました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-317">Built on OWIN</span></span>](#builtonowin)
- [<span data-ttu-id="b03ce-318">MapHubs および MapConnection が MapSignalR</span><span class="sxs-lookup"><span data-stu-id="b03ce-318">MapHubs and MapConnection are now MapSignalR</span></span>](#MapSignalR)
- [<span data-ttu-id="b03ce-319">クロス ドメインのサポート</span><span class="sxs-lookup"><span data-stu-id="b03ce-319">Cross-Domain Support</span></span>](#crossdomain)
- [<span data-ttu-id="b03ce-320">MonoTouch と MonoDroid による iOS および Android をサポートします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-320">iOS and Android support via MonoTouch and MonoDroid</span></span>](#mobile)
- [<span data-ttu-id="b03ce-321">ポータブル .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="b03ce-321">Portable .NET Client</span></span>](#portable)
- [<span data-ttu-id="b03ce-322">新しいパッケージを自己ホスト</span><span class="sxs-lookup"><span data-stu-id="b03ce-322">New Self-Host Package</span></span>](#selfhost)
- [<span data-ttu-id="b03ce-323">旧バージョンと互換性のあるサーバーのサポート</span><span class="sxs-lookup"><span data-stu-id="b03ce-323">Backward-compatible server support</span></span>](#backwardcompat)
- [<span data-ttu-id="b03ce-324">.NET 4.0 用のサーバーのサポートが削除されました</span><span class="sxs-lookup"><span data-stu-id="b03ce-324">Removed server support for .NET 4.0</span></span>](#remove40)
- [<span data-ttu-id="b03ce-325">クライアントとグループの一覧にメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-325">Sending a message to a list of clients and groups</span></span>](#messagelist)
- [<span data-ttu-id="b03ce-326">特定のユーザーにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-326">Sending a message to a specific user</span></span>](#sendtouser)
- [<span data-ttu-id="b03ce-327">優れたエラー処理のサポート</span><span class="sxs-lookup"><span data-stu-id="b03ce-327">Better error handling support</span></span>](#errorhandling)
- [<span data-ttu-id="b03ce-328">簡単に単体テストのハブ</span><span class="sxs-lookup"><span data-stu-id="b03ce-328">Easier unit testing of hubs</span></span>](#unittesting)
- [<span data-ttu-id="b03ce-329">JavaScript のエラー処理</span><span class="sxs-lookup"><span data-stu-id="b03ce-329">JavaScript error handling</span></span>](#javascripterror)

<span data-ttu-id="b03ce-330">SignalR 2.0 に既存の 1.x プロジェクトをアップグレードする方法の例は、次を参照してください。[アップグレード、SignalR 1.x プロジェクト](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-330">For an example of how to upgrade an existing 1.x project to SignalR 2.0, see [Upgrading a SignalR 1.x Project](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<a id="builtonowin"></a>
### <a name="built-on-owin"></a><span data-ttu-id="b03ce-331">OWIN 上に構築されました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-331">Built on OWIN</span></span>

<span data-ttu-id="b03ce-332">SignalR 2.0 に完全に構築された[OWIN (.NET の Open Web Interface)](http://owin.org/)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-332">SignalR 2.0 is built completely on [OWIN (the Open Web Interface for .NET)](http://owin.org/).</span></span> <span data-ttu-id="b03ce-333">この変更は、SignalR のセットアップ プロセスは、web ホストおよび自己ホスト型の SignalR アプリケーション間でより一貫性がさまざまな API の変更点にも必要です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-333">This change makes the setup process for SignalR much more consistent between web-hosted and self-hosted SignalR applications, but has also required a number of API changes.</span></span>

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a><span data-ttu-id="b03ce-334">MapHubs および MapConnection が MapSignalR</span><span class="sxs-lookup"><span data-stu-id="b03ce-334">MapHubs and MapConnection are now MapSignalR</span></span>

<span data-ttu-id="b03ce-335">OWIN 規格と互換性のため、これらのメソッドが変更されています`MapSignalR`します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-335">For compatibility with OWIN standards, these methods have been renamed to `MapSignalR`.</span></span> <span data-ttu-id="b03ce-336">`MapSignalR` パラメーターはすべてのハブをマップせずに呼び出されます (として`MapHubs`はバージョン 1.x) は個別にマップする**PersistentConnection**オブジェクト、型のパラメーターとして、接続 URL の拡張機能として、接続の種類を指定する、最初の引数。</span><span class="sxs-lookup"><span data-stu-id="b03ce-336">`MapSignalR` called without parameters will map all hubs (as `MapHubs` does in version 1.x); to map individual **PersistentConnection** objects, specify the connection type as the type parameter, and the URL extension for the connection as the first argument.</span></span>

<span data-ttu-id="b03ce-337">`MapSignalR` Owin startup クラスでメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-337">The `MapSignalR` method is called in an Owin startup class.</span></span> <span data-ttu-id="b03ce-338">Visual Studio 2013 には、Owin スタートアップ クラスの新しいテンプレートが含まれていますこのテンプレートを使用するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="b03ce-338">Visual Studio 2013 contains a new template for an Owin startup class; to use this template, do the following:</span></span>

1. <span data-ttu-id="b03ce-339">プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-339">Right-click on the project</span></span>
2. <span data-ttu-id="b03ce-340">選択**追加**、**新しい項目.**</span><span class="sxs-lookup"><span data-stu-id="b03ce-340">Select **Add**, **New Item...**</span></span>
3. <span data-ttu-id="b03ce-341">選択**Owin Startup クラス**します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-341">Select **Owin Startup class**.</span></span> <span data-ttu-id="b03ce-342">新しいクラスの名前**Startup.cs**します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-342">Name the new class **Startup.cs**.</span></span>

<span data-ttu-id="b03ce-343">**Web アプリケーション、** Owin スタートアップ クラスを含む、`MapSignalR`次に示すメソッドは、Web.Config ファイルのアプリケーション設定 ノード内のエントリを使用して、Owin のスタートアップ プロセスに追加されます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-343">In a **Web application,** the Owin startup class containing the `MapSignalR` method is then added to Owin's startup process using an entry in the application settings node of the Web.Config file, as shown below.</span></span>

<span data-ttu-id="b03ce-344">**アプリケーションを自己ホスト型**、Startup クラスがの型パラメーターとして渡される、`WebApp.Start`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b03ce-344">In a **Self-hosted application**, the Startup class is passed as the type parameter of the `WebApp.Start` method.</span></span>

<span data-ttu-id="b03ce-345">**ハブと SignalR の接続のマッピング (web アプリケーションのグローバルなアプリケーション ファイル) から 1.x:**</span><span class="sxs-lookup"><span data-stu-id="b03ce-345">**Mapping hubs and connections in SignalR 1.x (from the global application file in a web application):**</span></span> 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

<span data-ttu-id="b03ce-346">**ハブと SignalR 2.0 (Owin Startup クラス ファイルの場合) からの接続をマッピングします。**</span><span class="sxs-lookup"><span data-stu-id="b03ce-346">**Mapping hubs and connections in SignalR 2.0 (from an Owin Startup class file):**</span></span> 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

<span data-ttu-id="b03ce-347">**アプリケーションを自己ホスト型**、Startup クラスがの型パラメーターとして渡される、`WebApp.Start`メソッドは、次のようです。</span><span class="sxs-lookup"><span data-stu-id="b03ce-347">In a **Self-hosted application**, the Startup class is passed as the type parameter for the `WebApp.Start` method, as shown below.</span></span>

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a><span data-ttu-id="b03ce-348">クロス ドメインのサポート</span><span class="sxs-lookup"><span data-stu-id="b03ce-348">Cross-Domain Support</span></span>

<span data-ttu-id="b03ce-349">SignalR では 1.x では、クロス ドメイン要求が 1 つの EnableCrossDomain フラグによって制御されます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-349">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="b03ce-350">このフラグ JSONP と CORS の両方の要求を制御します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-350">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="b03ce-351">すべての CORS をサポートする柔軟性を高め、SignalR のサーバー コンポーネントから削除されました (JavaScript lients まだ CORS 通常、ブラウザーがサポートすることが検出された場合) を使用し、新しい OWIN ミドルウェアをこれらのシナリオをサポートするために使用しました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-351">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript lients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="b03ce-352">SignalR 2.0 では、クライアントの場合の JSONP を必須 (古いブラウザーでは、クロス ドメイン要求をサポート) を設定を明示的に有効にする必要があります`EnableJSONP`上、`HubConfiguration`オブジェクトを`true`以下に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-352">In SignalR 2.0, If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="b03ce-353">CORS より安全であるために、既定では、JSONP が無効です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-353">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="b03ce-354">SignalR 2.0 では、新しい CORS ミドルウェアを追加するには、追加、`Microsoft.Owin.Cors`呼び出しをプロジェクトにライブラリ`UseCors`SignalR ミドルウェアは、以下のセクションで示すようにする前にします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-354">To add the new CORS middleware in SignalR 2.0, add the `Microsoft.Owin.Cors` library to your project, and call `UseCors` before your SignalR middleware, as shown in the section below.</span></span>

<span data-ttu-id="b03ce-355">**Microsoft.Owin.Cors をプロジェクトに追加する**: このライブラリをインストールするには、パッケージ マネージャー コンソールで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-355">**Adding Microsoft.Owin.Cors to your project**: To install this library, run the following command in the Package Manager Console:</span></span>

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

<span data-ttu-id="b03ce-356">このコマンドは、2.0.0 を追加、プロジェクトにパッケージのバージョン。</span><span class="sxs-lookup"><span data-stu-id="b03ce-356">This command will add the 2.0.0 version of the package to your project.</span></span>

<span data-ttu-id="b03ce-357">**UseCors を呼び出す**</span><span class="sxs-lookup"><span data-stu-id="b03ce-357">**Calling UseCors**</span></span>

<span data-ttu-id="b03ce-358">次のコード スニペットは、SignalR のドメイン間の接続を実装する方法を示します 1.x と 2.0。</span><span class="sxs-lookup"><span data-stu-id="b03ce-358">The following code snippets demonstrate how to implement cross-domain connections in SignalR 1.x and 2.0.</span></span>

<span data-ttu-id="b03ce-359">**SignalR でクロス ドメイン要求を実装する (グローバル アプリケーション ファイル) から 1.x**</span><span class="sxs-lookup"><span data-stu-id="b03ce-359">**Implementing cross-domain requests in SignalR 1.x (from the global application file)**</span></span>

[!code-csharp[Main](release-notes/samples/sample9.cs)]

<span data-ttu-id="b03ce-360">**SignalR 2.0 (c# コード ファイル) からのクロス ドメイン要求を実装します。**</span><span class="sxs-lookup"><span data-stu-id="b03ce-360">**Implementing cross-domain requests in SignalR 2.0 (from a C# code file)**</span></span>

<span data-ttu-id="b03ce-361">次のコードでは、SignalR 2.0 プロジェクトで CORS または JSONP を有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-361">The following code demonstrates how to enable CORS or JSONP in a SignalR 2.0 project.</span></span> <span data-ttu-id="b03ce-362">このコード サンプルを使用して`Map`と`RunSignalR`の代わりに`MapSignalR`、CORS ミドルウェアは、CORS のサポートを必要とする SignalR 要求に対してのみが実行されるように、(で指定されたパスにすべてのトラフィックではなく`MapSignalR`)。`Map`アプリケーション全体に対してではなく、特定の URL プレフィックスでは、実行する必要があるその他のミドルウェアにも使用できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-362">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) `Map` can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a><span data-ttu-id="b03ce-363">MonoTouch と MonoDroid による iOS および Android をサポートします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-363">iOS and Android support via MonoTouch and MonoDroid</span></span>

<span data-ttu-id="b03ce-364">IOS と Android のクライアントから MonoTouch と MonoDroid コンポーネントを使用してサポートが追加されました、 [Xamarin ライブラリ](https://xamarin.com/)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-364">Support has been added for iOS and Android clients using MonoTouch and MonoDroid components from the [Xamarin library](https://xamarin.com/).</span></span> <span data-ttu-id="b03ce-365">それらを使用する方法の詳細については、次を参照してください。 [Xamarin コンポーネントを使用して](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-365">For more information on how to use them, see [Using Xamarin Components](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln).</span></span> <span data-ttu-id="b03ce-366">これらのコンポーネントがで提供されます、 [Xamarin Store](https://store.xamarin.com/) SignalR RTW リリースが使用可能な場合。</span><span class="sxs-lookup"><span data-stu-id="b03ce-366">These components will be available in the [Xamarin Store](https://store.xamarin.com/) when the SignalR RTW release is available.</span></span>

<a id="portable"></a> <span data-ttu-id="b03ce-367">### ポータブル .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="b03ce-367">### Portable .NET client</span></span>

<span data-ttu-id="b03ce-368">Silverlight を WinRT のクロス プラットフォーム開発を容易にし、Windows Phone クライアント、次のプラットフォームをサポートする 1 つのポータブル .NET クライアントに置き換えられました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-368">To better facilitate cross-platform development, the Silverlight, WinRT and Windows Phone clients have been replaced with a single portable .NET client that supports the following platforms:</span></span>

- <span data-ttu-id="b03ce-369">NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b03ce-369">NET 4.5</span></span>
- <span data-ttu-id="b03ce-370">Silverlight 5</span><span class="sxs-lookup"><span data-stu-id="b03ce-370">Silverlight 5</span></span>
- <span data-ttu-id="b03ce-371">WinRT (Windows ストア アプリ用 .NET)</span><span class="sxs-lookup"><span data-stu-id="b03ce-371">WinRT (.NET for Windows Store Apps)</span></span>
- <span data-ttu-id="b03ce-372">Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="b03ce-372">Windows Phone 8</span></span>

<a id="selfhost"></a>

### <a name="new-self-host-package"></a><span data-ttu-id="b03ce-373">新しいパッケージを自己ホスト</span><span class="sxs-lookup"><span data-stu-id="b03ce-373">New Self-Host Package</span></span>

<span data-ttu-id="b03ce-374">SignalR セルフホスト (プロセスでホストされている SignalR アプリケーションまたは他のアプリケーションではなく、web サーバーでホストされている) を使用しやすく NuGet パッケージはようになりました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-374">There is now a NuGet package to make it easier to get started with SignalR Self-Host (SignalR applications that are hosted in a process or other application, rather than being hosted in a web server).</span></span> <span data-ttu-id="b03ce-375">SignalR を使ってビルドされた自己ホスト プロジェクトをアップグレードする 1.x では、Microsoft.AspNet.SignalR.Owin のパッケージを削除し、Microsoft.AspNet.SignalR.SelfHost パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-375">To upgrade a self-host project built with SignalR 1.x, remove the Microsoft.AspNet.SignalR.Owin package, and add the Microsoft.AspNet.SignalR.SelfHost package.</span></span> <span data-ttu-id="b03ce-376">自己ホスト パッケージの概要については、次を参照してください。[チュートリアル: SignalR セルフホスト](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-376">For more information on getting started with the self-host package, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a><span data-ttu-id="b03ce-377">旧バージョンと互換性のあるサーバーのサポート</span><span class="sxs-lookup"><span data-stu-id="b03ce-377">Backward-compatible server support</span></span>

<span data-ttu-id="b03ce-378">以前のバージョンの SignalR、SignalR パッケージがクライアントで使用されると同一であるために必要なサーバーのバージョン。</span><span class="sxs-lookup"><span data-stu-id="b03ce-378">In previous versions of SignalR, the versions of the SignalR package used in the client and the server needed to be identical.</span></span> <span data-ttu-id="b03ce-379">シック クライアント アプリケーションを更新することは困難をサポートするために SignalR 2.0 サポート古いクライアントで新しいサーバー バージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-379">In order to support thick-client applications that would be difficult to update, SignalR 2.0 now supports using a newer server version with an older client.</span></span> <span data-ttu-id="b03ce-380">**注: SignalR 2.0 は新しいクライアントと以前のバージョンで構築されたサーバーをサポートしていません。**</span><span class="sxs-lookup"><span data-stu-id="b03ce-380">**Note: SignalR 2.0 does not support servers built with older versions with newer clients.**</span></span>

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a><span data-ttu-id="b03ce-381">.NET 4.0 用のサーバーのサポートが削除されました</span><span class="sxs-lookup"><span data-stu-id="b03ce-381">Removed server support for .NET 4.0</span></span>

<span data-ttu-id="b03ce-382">SignalR 2.0 では、.NET 4.0 でのサーバーの相互運用性のサポートを削除します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-382">SignalR 2.0 has dropped support for server interoperability with .NET 4.0.</span></span> <span data-ttu-id="b03ce-383">SignalR 2.0 サーバーでは、.NET 4.5 を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-383">.NET 4.5 must be used with SignalR 2.0 servers.</span></span> <span data-ttu-id="b03ce-384">SignalR 2.0 for .NET 4.0 クライアントは引き続きします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-384">There is still a .NET 4.0 client for SignalR 2.0.</span></span>

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a><span data-ttu-id="b03ce-385">クライアントとグループの一覧にメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-385">Sending a message to a list of clients and groups</span></span>

<span data-ttu-id="b03ce-386">SignalR 2.0 では、クライアントとグループ Id の一覧を使用してメッセージを送信することはできます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-386">In SignalR 2.0, it's possible to send a message using a list of client and group IDs.</span></span> <span data-ttu-id="b03ce-387">次のコード スニペットでは、これを行う方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-387">The following code snippets demonstrate how to do this.</span></span>

<span data-ttu-id="b03ce-388">**クライアントと PersistentConnection を使用してグループの一覧にメッセージを送信します。**</span><span class="sxs-lookup"><span data-stu-id="b03ce-388">**Sending a message to a list of clients and groups using PersistentConnection**</span></span>

[!code-csharp[Main](release-notes/samples/sample11.cs)]

<span data-ttu-id="b03ce-389">**クライアントとハブを使用してグループの一覧にメッセージを送信します。**</span><span class="sxs-lookup"><span data-stu-id="b03ce-389">**Sending a message to a list of clients and groups using Hubs**</span></span>

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a><span data-ttu-id="b03ce-390">特定のユーザーにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-390">Sending a message to a specific user</span></span>

<span data-ttu-id="b03ce-391">この機能により、ユーザー Id を指定するユーザー IUserIdProvider 新しいインターフェイスを使用して、IRequest に基づいて。</span><span class="sxs-lookup"><span data-stu-id="b03ce-391">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider:</span></span>

<span data-ttu-id="b03ce-392">**IUserIdProvider インターフェイス**</span><span class="sxs-lookup"><span data-stu-id="b03ce-392">**The IUserIdProvider interface**</span></span>

[!code-csharp[Main](release-notes/samples/sample13.cs)]

<span data-ttu-id="b03ce-393">既定では、ユーザー名とユーザーの IPrincipal.Identity.Name を使用する実装があります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-393">By default there will be an implementation that uses the user's IPrincipal.Identity.Name as the user name.</span></span>

<span data-ttu-id="b03ce-394">ハブで、新しい API を使用してこれらのユーザーにメッセージを送信するようになります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-394">In hubs, you'll be able to send messages to these users via a new API:</span></span>

<span data-ttu-id="b03ce-395">**Clients.User API を使用します。**</span><span class="sxs-lookup"><span data-stu-id="b03ce-395">**Using the Clients.User API**</span></span>

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a><span data-ttu-id="b03ce-396">優れたエラー処理のサポート</span><span class="sxs-lookup"><span data-stu-id="b03ce-396">Better Error Handling Support</span></span>

<span data-ttu-id="b03ce-397">ユーザーがスローできるようになりました**HubException**任意のハブ呼び出しから。</span><span class="sxs-lookup"><span data-stu-id="b03ce-397">Users can now throw **HubException** from any hub invocation.</span></span> <span data-ttu-id="b03ce-398">コンス トラクター、 **HubException**文字列メッセージとオブジェクトにかかる追加のエラー データ。</span><span class="sxs-lookup"><span data-stu-id="b03ce-398">The constructor of the **HubException** can take a string message and an object extra error data.</span></span> <span data-ttu-id="b03ce-399">SignalR は、例外を自動シリアル化し、拒否/失敗のハブ メソッドの呼び出しに使用されますをクライアントに送信します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-399">SignalR will auto-serialize the exception and send it to the client where it will be used to reject/fail the hub method invocation.</span></span>

<span data-ttu-id="b03ce-400">**ハブの詳細な例外表示**設定に影響していない**HubException**ことはありません。 または、クライアントに送信されることは常に送信します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-400">The **show detailed hub exceptions** setting has no bearing on **HubException** being sent back to the client or not; it is always sent.</span></span>

<span data-ttu-id="b03ce-401">**サーバー側コードのデモを HubException をクライアントに送信します。**</span><span class="sxs-lookup"><span data-stu-id="b03ce-401">**Server-side code demonstrating sending a HubException to the client**</span></span>

[!code-csharp[Main](release-notes/samples/sample15.cs)]

<span data-ttu-id="b03ce-402">**JavaScript クライアント コードが、サーバーから送信された HubException への応答のデモ**</span><span class="sxs-lookup"><span data-stu-id="b03ce-402">**JavaScript client code demonstrating responding to a HubException sent from the server**</span></span>

[!code-html[Main](release-notes/samples/sample16.html)]

<span data-ttu-id="b03ce-403">**.NET クライアントをサーバーから送信された HubException への応答を示すコード**</span><span class="sxs-lookup"><span data-stu-id="b03ce-403">**.NET client code demonstrating responding to a HubException sent from the server**</span></span>

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a><span data-ttu-id="b03ce-404">簡単に単体テストのハブ</span><span class="sxs-lookup"><span data-stu-id="b03ce-404">Easier unit testing of hubs</span></span>

<span data-ttu-id="b03ce-405">SignalR 2.0 にはというインターフェイスが含まれています`IHubCallerConnectionContext`hubs モックのクライアント側の呼び出しの作成が簡単です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-405">SignalR 2.0 includes an interface called `IHubCallerConnectionContext` on Hubs that makes it easier to create mock client side invocations.</span></span> <span data-ttu-id="b03ce-406">人気のあるテスト ハーネスでのこのインターフェイスの使用方法を示す次のコード スニペット[xUnit.net](https://github.com/xunit/xunit)と[moq](https://code.google.com/p/moq/)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-406">The following code snippets demonstrate using this interface with popular test harnesses [xUnit.net](https://github.com/xunit/xunit) and [moq](https://code.google.com/p/moq/).</span></span>

<span data-ttu-id="b03ce-407">**単体テストの xUnit.net で SignalR**</span><span class="sxs-lookup"><span data-stu-id="b03ce-407">**Unit testing SignalR with xUnit.net**</span></span>

[!code-csharp[Main](release-notes/samples/sample18.cs)]

<span data-ttu-id="b03ce-408">**単体テストの moq と SignalR**</span><span class="sxs-lookup"><span data-stu-id="b03ce-408">**Unit testing SignalR with moq**</span></span>

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a><span data-ttu-id="b03ce-409">JavaScript のエラー処理</span><span class="sxs-lookup"><span data-stu-id="b03ce-409">JavaScript error handling</span></span>

<span data-ttu-id="b03ce-410">SignalR 2.0 では、JavaScript エラー処理のすべてのコールバックは、未加工の文字列ではなく JavaScript エラー オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-410">In SignalR 2.0, all JavaScript error handling callbacks return JavaScript error objects instead of raw strings.</span></span> <span data-ttu-id="b03ce-411">これによりより豊富な情報、エラー ハンドラーにフローする SignalR ができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-411">This allows SignalR to flow richer information to your error handlers.</span></span> <span data-ttu-id="b03ce-412">内部例外を取得することができます、`source`エラーのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="b03ce-412">You can get the inner exception from the `source` property of the error.</span></span>

<span data-ttu-id="b03ce-413">**Start.Fail 例外を処理する JavaScript クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="b03ce-413">**JavaScript client code that handles the Start.Fail exception**</span></span>

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a><span data-ttu-id="b03ce-414">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="b03ce-414">ASP.NET Identity</span></span>

### <a name="new-aspnet-membership-system"></a><span data-ttu-id="b03ce-415">新しい ASP.NET メンバーシップ システム</span><span class="sxs-lookup"><span data-stu-id="b03ce-415">New ASP.NET Membership System</span></span>

<span data-ttu-id="b03ce-416">ASP.NET Identity は、ASP.NET アプリケーションの新しいメンバーシップ システムです。</span><span class="sxs-lookup"><span data-stu-id="b03ce-416">ASP.NET Identity is the new membership system for ASP.NET applications.</span></span> <span data-ttu-id="b03ce-417">ASP.NET Identity を簡単にアプリケーション データとユーザー固有のプロファイル データを統合します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-417">ASP.NET Identity makes it easy to integrate user-specific profile data with application data.</span></span> <span data-ttu-id="b03ce-418">ASP.NET Identity では、アプリケーションでユーザー プロファイルの永続化モデルを選択することもできます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-418">ASP.NET Identity also allows you to choose the persistence model for user profiles in your application.</span></span> <span data-ttu-id="b03ce-419">SQL Server データベースまたは Azure Storage のテーブルなどの NoSQL データ ストアなど、別のデータ ストアにデータを保存することができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-419">You can store the data in a SQL Server database or another data store, including NoSQL data stores such as Azure Storage Tables.</span></span> <span data-ttu-id="b03ce-420">詳細については、次を参照してください。[個々 のユーザー アカウント](creating-web-projects-in-visual-studio.md#indauth)で**Visual Studio 2013 で ASP.NET Web プロジェクトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-420">For more information, see [Individual User Accounts](creating-web-projects-in-visual-studio.md#indauth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="claims-based-authentication"></a><span data-ttu-id="b03ce-421">クレーム ベースの認証</span><span class="sxs-lookup"><span data-stu-id="b03ce-421">Claims-based authentication</span></span>

<span data-ttu-id="b03ce-422">ASP.NET では、ユーザーの id が信頼された発行者からのクレームのセットとして表されるクレーム ベース認証できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-422">ASP.NET now supports claims-based authentication, where the user's identity is represented as a set of claims from a trusted issuer.</span></span> <span data-ttu-id="b03ce-423">ユーザーは認証されたユーザー名と、アプリケーション データベースに保存されるパスワードを使用してまたはソーシャル id プロバイダーを使用できます (例: Microsoft アカウント、Facebook、Google、Twitter)、または Azure Active Directory を介して組織のアカウントを使用して、またはActive Directory Federation Services (ADFS)。</span><span class="sxs-lookup"><span data-stu-id="b03ce-423">Users can be authenticated using a username and password maintained in an application database, or using social identity providers (for example: Microsoft Accounts, Facebook, Google, Twitter), or using organizational accounts through Azure Active Directory or Active Directory Federation Services (ADFS).</span></span>

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a><span data-ttu-id="b03ce-424">Azure Active Directory および Windows Server Active Directory との統合</span><span class="sxs-lookup"><span data-stu-id="b03ce-424">Integration with Azure Active Directory and Windows Server Active Directory</span></span>

<span data-ttu-id="b03ce-425">Azure Active Directory または Windows Server ・ Active Directory (AD) 認証を使用する ASP.NET プロジェクトを作成できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-425">You can now create ASP.NET projects that use Azure Active Directory or Windows Server Active Directory (AD) for authentication.</span></span> <span data-ttu-id="b03ce-426">詳細については、次を参照してください。[組織アカウント](creating-web-projects-in-visual-studio.md#orgauth)で**Visual Studio 2013 で ASP.NET Web プロジェクトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-426">For more information, see [Organizational Accounts](creating-web-projects-in-visual-studio.md#orgauth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="owin-integration"></a><span data-ttu-id="b03ce-427">OWIN の統合</span><span class="sxs-lookup"><span data-stu-id="b03ce-427">OWIN Integration</span></span>

<span data-ttu-id="b03ce-428">ASP.NET 認証は、任意の OWIN ベースのホストで使用できる、OWIN ミドルウェアに基づいています。</span><span class="sxs-lookup"><span data-stu-id="b03ce-428">ASP.NET authentication is now based on OWIN middleware that can be used on any OWIN-based host.</span></span> <span data-ttu-id="b03ce-429">OWIN の詳細については、次を参照してください。 [Microsoft OWIN コンポーネント](#TOC7)セクション。</span><span class="sxs-lookup"><span data-stu-id="b03ce-429">For more information about OWIN, see the following [Microsoft OWIN Components](#TOC7) section.</span></span>

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a><span data-ttu-id="b03ce-430">Microsoft OWIN コンポーネント</span><span class="sxs-lookup"><span data-stu-id="b03ce-430">Microsoft OWIN Components</span></span>

<span data-ttu-id="b03ce-431">[.NET 用 Web インターフェイスを開き](http://owin.org/)(OWIN) .NET web サーバーおよび web アプリケーション間の抽象化を定義します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-431">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="b03ce-432">OWIN は、web アプリケーション ホストに依存しないのため、サーバーから web アプリケーションを分離します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-432">OWIN decouples the web application from the server, making web applications host-agnostic.</span></span> <span data-ttu-id="b03ce-433">たとえば、IIS の OWIN ベースの web アプリケーションをホストしたり、カスタム プロセスでセルフホストできます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-433">For example, you can host an OWIN-based web application in IIS or self-host it in a custom process.</span></span>

<span data-ttu-id="b03ce-434">Microsoft OWIN コンポーネント (Katana プロジェクトとも呼ばれます) で導入された変更には、新しいサーバーとホストのコンポーネント、新しいヘルパー ライブラリとミドルウェア、および新しい認証ミドルウェアが含まれます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-434">Changes introduced in the Microsoft OWIN components (also known as the Katana project) include new server and host components, new helper libraries and middleware, and new authentication middleware.</span></span>

<span data-ttu-id="b03ce-435">OWIN と Katana の詳細については、次を参照してください。 [OWIN と Katana の新](../../../aspnet/overview/owin-and-katana/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-435">For more information about OWIN and Katana, see [What's new in OWIN and Katana](../../../aspnet/overview/owin-and-katana/index.md).</span></span>

<span data-ttu-id="b03ce-436">**注: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) IIS クラシック モードでアプリケーションを実行できません。 これらは、統合モードで実行する必要があります。**</span><span class="sxs-lookup"><span data-stu-id="b03ce-436">**Note: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) applications cannot run in IIS classic mode; they must be run in integrated mode.**</span></span>

<span data-ttu-id="b03ce-437">**注: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)完全信頼でアプリケーションを実行する必要があります。**</span><span class="sxs-lookup"><span data-stu-id="b03ce-437">**Note: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) applications must be run in full trust.**</span></span>

### <a name="new-servers-and-hosts"></a><span data-ttu-id="b03ce-438">新しいサーバーとホスト</span><span class="sxs-lookup"><span data-stu-id="b03ce-438">New Servers and Hosts</span></span>

<span data-ttu-id="b03ce-439">このリリースでは、新しいコンポーネントは、自己ホスト シナリオを有効に追加されました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-439">With this release, new components were added to enable self-host scenarios.</span></span> <span data-ttu-id="b03ce-440">これらのコンポーネントには、次の NuGet パッケージが含まれます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-440">These components include the following NuGet packages:</span></span>

- <span data-ttu-id="b03ce-441">**Microsoft.Owin.Host.HttpListener**します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-441">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="b03ce-442">使用する OWIN サーバーは、 **HttpListener**を HTTP 要求をリッスンし、OWIN パイプラインに指示します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-442">Provides an OWIN server that uses **HttpListener** to listen for HTTP requests and direct them into the OWIN pipeline.</span></span>
- <span data-ttu-id="b03ce-443">**Microsoft.Owin.Hosting**にコンソール アプリケーションまたは Windows サービスなどのカスタムのプロセスで OWIN パイプラインをセルフホストする開発者のライブラリを提供します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-443">**Microsoft.Owin.Hosting** Provides a library for developers who wish to self-host an OWIN pipeline in a custom process, such as a console application or Windows service.</span></span>
- <span data-ttu-id="b03ce-444">**OwinHost**します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-444">**OwinHost**.</span></span> <span data-ttu-id="b03ce-445">ラップするスタンドアロン実行可能ファイルを提供します。`Microsoft.Owin.Hosting`し、カスタム ホスト アプリケーションを記述することなく、OWIN パイプラインをセルフホストすることができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-445">Provides a stand-alone executable that wraps `Microsoft.Owin.Hosting` and lets you self-host an OWIN pipeline without having to write a custom host application.</span></span>

<span data-ttu-id="b03ce-446">さらに、`Microsoft.Owin.Host.SystemWeb`パッケージには、ミドルウェアにヒントを提供するようになりましたができるように、 **SystemWeb**サーバー、特定の ASP.NET パイプライン ステージの間に、ミドルウェアを呼び出す必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-446">In addition, the `Microsoft.Owin.Host.SystemWeb` package now enables middleware to provide hints to the **SystemWeb** server, indicating that the middleware should be called during a specific ASP.NET pipeline stage.</span></span> <span data-ttu-id="b03ce-447">この機能は、認証ミドルウェアは、ASP.NET パイプラインの早い段階で実行する必要がありますに特に便利です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-447">This feature is particularly useful for authentication middleware, which should run early in the ASP.NET pipeline.</span></span>

### <a name="helper-libraries-and-middleware"></a><span data-ttu-id="b03ce-448">ヘルパー ライブラリとミドルウェア</span><span class="sxs-lookup"><span data-stu-id="b03ce-448">Helper Libraries and Middleware</span></span>

<span data-ttu-id="b03ce-449">OWIN 仕様から関数と型の定義のみを使用して、OWIN コンポーネントを記述できますが、新しい`Microsoft.Owin`パッケージはユーザーにわかりやすい一連の抽象化を提供します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-449">Although you can write OWIN components using only the function and type definitions from the OWIN specification, the new `Microsoft.Owin` package provides a more user-friendly set of abstractions.</span></span> <span data-ttu-id="b03ce-450">このパッケージは、いくつかの以前のパッケージを結合 (例: `Owin.Extensions`、 `Owin.Types`) し、簡単に使用できるその他の OWIN コンポーネントで適切に構成された 1 つのオブジェクト モデルにします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-450">This package combines several earlier packages (e.g., `Owin.Extensions`, `Owin.Types`) into a single, well-structured object model that can then be easily used by other OWIN components.</span></span> <span data-ttu-id="b03ce-451">実際には、Microsoft OWIN コンポーネントの大半は、このパッケージを使用するようになりました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-451">In fact, the majority of Microsoft OWIN components now use this package.</span></span>

> [!NOTE]
> <span data-ttu-id="b03ce-452">[OWIN](http://www.owin.org) IIS クラシック モードでアプリケーションを実行できません。 これらは、統合モードで実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-452">[OWIN](http://www.owin.org) applications cannot run in IIS classic mode; they must be run in integrated mode.</span></span>

> [!NOTE]
> <span data-ttu-id="b03ce-453">[OWIN](http://www.owin.org)完全信頼でアプリケーションを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-453">[OWIN](http://www.owin.org) applications must be run in full trust.</span></span>

<span data-ttu-id="b03ce-454">このリリースには、ミドルウェアを実行中の OWIN アプリケーションを検証するとエラーの調査に役立つエラー ページ ミドルウェアを含む Microsoft.Owin.Diagnostics パッケージも含まれています。</span><span class="sxs-lookup"><span data-stu-id="b03ce-454">This release also includes the Microsoft.Owin.Diagnostics package, which includes middleware to validate a running OWIN application, plus error-page middleware to help investigate failures.</span></span>

### <a name="authentication-components"></a><span data-ttu-id="b03ce-455">認証コンポーネント</span><span class="sxs-lookup"><span data-stu-id="b03ce-455">Authentication Components</span></span>

<span data-ttu-id="b03ce-456">次の認証コンポーネントは使用できます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-456">The following authentication components are available.</span></span>

- <span data-ttu-id="b03ce-457">**Microsoft.Owin.Security.ActiveDirectory**します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-457">**Microsoft.Owin.Security.ActiveDirectory**.</span></span> <span data-ttu-id="b03ce-458">オンプレミスまたはクラウド ベースのディレクトリ サービスを使用して認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-458">Enables authentication using on-premise or cloud-based directory services.</span></span>
- <span data-ttu-id="b03ce-459">**Microsoft.Owin.Security.Cookies** cookie を使用して認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-459">**Microsoft.Owin.Security.Cookies** Enables authentication using cookies.</span></span> <span data-ttu-id="b03ce-460">このパッケージが以前という名前だった`Microsoft.Owin.Security.Forms`します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-460">This package was previously named `Microsoft.Owin.Security.Forms`.</span></span>
- <span data-ttu-id="b03ce-461">**Microsoft.Owin.Security.Facebook** Facebook の OAuth ベースのサービスを使用して認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-461">**Microsoft.Owin.Security.Facebook** Enables authentication using Facebook's OAuth-based service.</span></span>
- <span data-ttu-id="b03ce-462">**Microsoft.Owin.Security.Google** Google の OpenID ベースのサービスを使用して認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-462">**Microsoft.Owin.Security.Google** Enables authentication using Google's OpenID-based service.</span></span>
- <span data-ttu-id="b03ce-463">**Microsoft.Owin.Security.Jwt** JWT トークンを使用して認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-463">**Microsoft.Owin.Security.Jwt** Enables authentication using JWT tokens.</span></span>
- <span data-ttu-id="b03ce-464">**Microsoft.Owin.Security.MicrosoftAccount** Microsoft アカウントを使用して認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-464">**Microsoft.Owin.Security.MicrosoftAccount** Enables authentication using Microsoft accounts.</span></span>
- <span data-ttu-id="b03ce-465">**Microsoft.Owin.Security.OAuth**します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-465">**Microsoft.Owin.Security.OAuth**.</span></span> <span data-ttu-id="b03ce-466">ベアラー トークンを認証するためには、OAuth 承認サーバーだけでなく、ミドルウェアを提供します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-466">Provides an OAuth authorization server as well as middleware for authenticating bearer tokens.</span></span>
- <span data-ttu-id="b03ce-467">**Microsoft.Owin.Security.Twitter** Twitter の OAuth ベースのサービスを使用して認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-467">**Microsoft.Owin.Security.Twitter** Enables authentication using Twitter's OAuth-based service.</span></span>

<span data-ttu-id="b03ce-468">このリリースでは、`Microsoft.Owin.Cors`パッケージで、クロスオリジン HTTP 要求を処理するためのミドルウェアが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b03ce-468">This release also includes the `Microsoft.Owin.Cors` package, which contains middleware for processing cross-origin HTTP requests.</span></span>

> [!NOTE]
> <span data-ttu-id="b03ce-469">JWT に署名するためのサポートは Visual Studio 2013 の最終バージョンで削除されました。</span><span class="sxs-lookup"><span data-stu-id="b03ce-469">Support for JWT signing has been removed in the final version of Visual Studio 2013.</span></span>

<a id="ef6"></a>
## <a name="entity-framework-6"></a><span data-ttu-id="b03ce-470">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b03ce-470">Entity Framework 6</span></span>

<span data-ttu-id="b03ce-471">新機能および Entity Framework 6 の他の変更の一覧は、次を参照してください。 [Entity Framework のバージョン履歴](https://msdn.com/data/jj574253)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-471">For a list of new features and other changes in Entity Framework 6, see [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a><span data-ttu-id="b03ce-472">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="b03ce-472">ASP.NET Razor 3</span></span>

<span data-ttu-id="b03ce-473">ASP.NET Razor 3 には、次の新機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b03ce-473">ASP.NET Razor 3 includes the following new features:</span></span>

- <span data-ttu-id="b03ce-474">タブの編集をサポートします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-474">Support for Tab editing.</span></span> <span data-ttu-id="b03ce-475">Preivously、**ドキュメントのフォーマット**コマンド、自動インデント、および自動 Visual Studio で書式設定が正しく動作しなかったを使用する場合、**タブの保持**オプション。</span><span class="sxs-lookup"><span data-stu-id="b03ce-475">Preivously, the **Format Document** command, auto indenting, and auto formatting in Visual Studio did not work correctly when using the **Keep Tabs** option.</span></span> <span data-ttu-id="b03ce-476">この変更は、Visual Studio の書式設定 タブの Razor コードの書式設定を修正します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-476">This change corrects Visual Studio formatting for Razor code for tab formatting.</span></span>
- <span data-ttu-id="b03ce-477">リンクを生成するときに URL 書き換えルールをサポートします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-477">Support for URL Rewrite rules when generating links.</span></span>
- <span data-ttu-id="b03ce-478">セキュリティ透過的な属性の削除。</span><span class="sxs-lookup"><span data-stu-id="b03ce-478">Removal of security transparent attribute.</span></span>
  > [!NOTE]
  > <span data-ttu-id="b03ce-479">これは重大な変更し、Razor 3 互換性のない MVC4、以前のバージョンと Razor 2 は MVC5 または MVC5 に対してコンパイルされたアセンブリとの互換性はありません。</span><span class="sxs-lookup"><span data-stu-id="b03ce-479">This is a breaking change, and makes Razor 3 incompatible with MVC4 and earlier, while Razor 2 is incompatible with MVC5 or assemblies compiled against MVC5.</span></span>

<span data-ttu-id="b03ce-480">プレリリース版から Visual Studio 2013 で修正された razor 3 問題はあります[ここ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-480">Razor 3 issues fixed in Visual Studio 2013 from pre-release versions can be found [here](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).</span></span>

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a><span data-ttu-id="b03ce-481">ASP.NET アプリを中断します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-481">ASP.NET App Suspend</span></span>

<span data-ttu-id="b03ce-482">ASP.NET アプリの中断は、ユーザー エクスペリエンスと多数の 1 台のコンピューター上の ASP.NET のサイトをホストするための経済的なモデルを大幅に変更する .NET Framework 4.5.1 における革新的機能です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-482">ASP.NET App Suspend is a game-changing feature in the .NET Framework 4.5.1 that radically changes the user experience and economic model for hosting large numbers of ASP.NET sites on a single machine.</span></span> <span data-ttu-id="b03ce-483">詳細については、次を参照してください。 [ASP.NET App Suspend – 応答性の高い共有 .NET web ホスティング](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-483">For more information, see [ASP.NET App Suspend – responsive shared .NET web hosting](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).</span></span>

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="b03ce-484">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="b03ce-484">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="b03ce-485">このセクションでは、Visual Studio 2013 の既知の問題と、ASP.NET と Web ツールにおける重大な変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-485">This section describes known issues and breaking changes in the ASP.NET and Web Tools for Visual Studio 2013.</span></span>

### <a name="nuget"></a><span data-ttu-id="b03ce-486">NuGet</span><span class="sxs-lookup"><span data-stu-id="b03ce-486">NuGet</span></span>

- <span data-ttu-id="b03ce-487">[SLN ファイルを使用する場合、Mono で新しいパッケージの復元が機能しません](https://nuget.codeplex.com/workitem/3596)– 今後 nuget.exe をダウンロードで修正される予定と[NuGet.CommandLine パッケージ](http://www.nuget.org/packages/NuGet.CommandLine/)を更新します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-487">[New package restore doesn't work on Mono when using SLN file](https://nuget.codeplex.com/workitem/3596) – will be fixed in an upcoming nuget.exe download and [NuGet.CommandLine package](http://www.nuget.org/packages/NuGet.CommandLine/) update.</span></span>
- <span data-ttu-id="b03ce-488">[Wix プロジェクトで新しいパッケージの復元は機能しません](https://nuget.codeplex.com/workitem/3598)– 今後 nuget.exe をダウンロードで修正される予定と[NuGet.CommandLine パッケージ](http://www.nuget.org/packages/NuGet.CommandLine/)を更新します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-488">[New package restore doesn't work with Wix projects](https://nuget.codeplex.com/workitem/3598) – will be fixed in an upcoming nuget.exe download and [NuGet.CommandLine package](http://www.nuget.org/packages/NuGet.CommandLine/) update.</span></span>
- <span data-ttu-id="b03ce-489">[自動パッケージの復元では、ソリューション フォルダーの下のプロジェクトが機能しません](https://nuget.codeplex.com/workitem/3625)– NuGet 2.8 で修正される予定です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-489">[Automatic Package restore doesn't work for projects under a solution folder](https://nuget.codeplex.com/workitem/3625) – will be fixed in NuGet 2.8.</span></span>

### <a name="aspnet-web-api"></a><span data-ttu-id="b03ce-490">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="b03ce-490">ASP.NET Web API</span></span>

1. <span data-ttu-id="b03ce-491">`ODataQueryOptions<T>.ApplyTo(IQueryable)` 返さない`IQueryable<T>`常に、サポートが追加されました`$select`と`$expand`します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-491">`ODataQueryOptions<T>.ApplyTo(IQueryable)` doesn't return `IQueryable<T>` always, as we added support for `$select` and `$expand`.</span></span>

    <span data-ttu-id="b03ce-492">以前のサンプル`ODataQueryOptions<T>`から戻り値を常にキャストされた`ApplyTo`に`IQueryable<T>`します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-492">Our earlier samples for `ODataQueryOptions<T>` always casted the return value from `ApplyTo` to `IQueryable<T>`.</span></span> <span data-ttu-id="b03ce-493">以前このため、クエリ オプションを以前方法でサポート (`$filter`、 `$orderby`、 `$skip`、 `$top`)、クエリの形状を変更しないでください。</span><span class="sxs-lookup"><span data-stu-id="b03ce-493">This worked earlier because the query options that we supported earlier (`$filter`, `$orderby`, `$skip`, `$top`) do not change the shape of the query.</span></span> <span data-ttu-id="b03ce-494">これでサポートされています`$select`と`$expand`からの戻り値`ApplyTo`されません`IQueryable<T>`常にします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-494">Now that we support `$select` and `$expand` the return value from `ApplyTo` will not be `IQueryable<T>` always.</span></span>

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    <span data-ttu-id="b03ce-495">前のサンプル コードを使用している場合、引き続き動作、クライアントが送信されなかった場合`$select`と`$expand`します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-495">If you are using the sample code from earlier, it will continue working if the client does not send `$select` and `$expand`.</span></span> <span data-ttu-id="b03ce-496">ただし、サポートする場合`$select`と`$expand`にそのコードを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-496">However, if you wish to support `$select` and `$expand` you have to change that code to this.</span></span>

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. <span data-ttu-id="b03ce-497">**Request.Url または RequestContext.Url が null のバッチ要求中に**</span><span class="sxs-lookup"><span data-stu-id="b03ce-497">**Request.Url or RequestContext.Url is null during a batch request**</span></span>

    <span data-ttu-id="b03ce-498">バッチ処理のシナリオで**UrlHelper**がからアクセスする場合は null **Request.Url**または**RequestContext.Url**します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-498">In a batching scenario, **UrlHelper** is null when accessed from **Request.Url** or **RequestContext.Url**.</span></span>

    <span data-ttu-id="b03ce-499">この問題はここで現在追跡: [BatchRequestContext.Url が要求をバッチ処理の null](http://aspnetwebstack.codeplex.com/workitem/1301)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-499">This issue is currently tracked here: [BatchRequestContext.Url is null for batching request](http://aspnetwebstack.codeplex.com/workitem/1301).</span></span>

    <span data-ttu-id="b03ce-500">この問題の回避の新しいインスタンスを作成するには**UrlHelper**、次の例。</span><span class="sxs-lookup"><span data-stu-id="b03ce-500">The workaround for this issue is to create a new instance of **UrlHelper**, as in the following example:</span></span>

    <span data-ttu-id="b03ce-501">**UrlHelper の新しいインスタンスを作成します。**</span><span class="sxs-lookup"><span data-stu-id="b03ce-501">**Creating a new instance of UrlHelper**</span></span>

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a><span data-ttu-id="b03ce-502">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b03ce-502">ASP.NET MVC</span></span>

1. <span data-ttu-id="b03ce-503">MVC5 と OrgAuth を使用しているビュー AntiForgerToken 検証を行うことがある場合は、使用している場合、ビューにデータを投稿すると、次のエラーを遭遇する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-503">When using MVC5 and OrgAuth, if you have views which do AntiForgerToken validation, you might come across the following error when you post data to the view:</span></span>

    <span data-ttu-id="b03ce-504">**エラー**:</span><span class="sxs-lookup"><span data-stu-id="b03ce-504">**Error**:</span></span>

    <span data-ttu-id="b03ce-505">*'/' アプリケーションでは、サーバー エラーです。*</span><span class="sxs-lookup"><span data-stu-id="b03ce-505">*Server Error in '/' Application.*</span></span>

    <span data-ttu-id="b03ce-506"><em>種類の要求 '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'または'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' が指定した ClaimsIdentity に存在しませんでした。クレーム ベース認証を使用した偽造防止トークンのサポートを有効にするには、構成されている要求プロバイダーが提供している生成 ClaimsIdentity インスタンスでこれらの要求の両方をくださいを確認します。代わりに、構成されている要求プロバイダーは、一意の識別子としてさまざまなクレームの種類を使用する場合は、AntiForgeryConfig.UniqueClaimTypeIdentifier 静的プロパティを設定して構成できます。</em></span><span class="sxs-lookup"><span data-stu-id="b03ce-506"><em>A claim of type '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>' or '<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' was not present on the provided ClaimsIdentity. To enable anti-forgery token support with claims-based authentication, please verify that the configured claims provider is providing both of these claims on the ClaimsIdentity instances it generates. If the configured claims provider instead uses a different claim type as a unique identifier, it can be configured by setting the static property AntiForgeryConfig.UniqueClaimTypeIdentifier.</em></span></span>

    <span data-ttu-id="b03ce-507">**回避策**:</span><span class="sxs-lookup"><span data-stu-id="b03ce-507">**Workaround**:</span></span>

    <span data-ttu-id="b03ce-508">その修正 Global.asax で、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-508">Add the following line in Global.asax to fix it:</span></span>

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    <span data-ttu-id="b03ce-509">これについては、次のリリースを修正します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-509">This will be fixed for the next release.</span></span>
2. <span data-ttu-id="b03ce-510">MVC5 に MVC4 アプリケーションをアップグレードした後は、ソリューションをビルドし、これを起動します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-510">After upgrading an MVC4 app to MVC5, build the solution and launch it.</span></span> <span data-ttu-id="b03ce-511">次のエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-511">You should see the following error:</span></span>

    <span data-ttu-id="b03ce-512">[A][B]System.Web.WebPages.Razor.Configuration.HostSection に System.Web.WebPages.Razor.Configuration.HostSection をキャストすることはできません。</span><span class="sxs-lookup"><span data-stu-id="b03ce-512">[A]System.Web.WebPages.Razor.Configuration.HostSection cannot be cast to [B]System.Web.WebPages.Razor.Configuration.HostSection.</span></span> <span data-ttu-id="b03ce-513">タイプ A 'System.Web.WebPages.Razor、バージョン 2.0.0.0、カルチャを = = neutral, PublicKeyToken = 31bf3856ad364e35' 場所' Default' のコンテキストで ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'。</span><span class="sxs-lookup"><span data-stu-id="b03ce-513">Type A originates from 'System.Web.WebPages.Razor, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'.</span></span> <span data-ttu-id="b03ce-514">タイプ b 'System.Web.WebPages.Razor、バージョン 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 を =' 場所' Default' のコンテキストで ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'。</span><span class="sxs-lookup"><span data-stu-id="b03ce-514">Type B originates from 'System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.</span></span>

    <span data-ttu-id="b03ce-515">上記のエラーを修正するには、開きます*すべて*プロジェクトとは、次の Web.config ファイル (Views フォルダーにあるものを含む)。</span><span class="sxs-lookup"><span data-stu-id="b03ce-515">To fix the above error, open *all* the Web.config files (including the ones in the Views folder) in your project and do the following:</span></span>

   1. <span data-ttu-id="b03ce-516">「5.0.0.0」の"System.Web.Mvc"のバージョン「4.0.0.0」のすべての出現箇所を更新します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-516">Update all occurrences of version "4.0.0.0" of "System.Web.Mvc" to "5.0.0.0".</span></span>
   2. <span data-ttu-id="b03ce-517">バージョン「2.0.0.0」を"System.Web.Helpers"の出現をすべて更新&quot;System.Web.WebPages&quot;と&quot;System.Web.WebPages.Razor&quot; 「3.0.0.0」を</span><span class="sxs-lookup"><span data-stu-id="b03ce-517">Update all occurrences of version "2.0.0.0" of "System.Web.Helpers", &quot;System.Web.WebPages&quot; and &quot;System.Web.WebPages.Razor&quot; to "3.0.0.0"</span></span>

      <span data-ttu-id="b03ce-518">たとえば、上記の変更を行った後にこのようアセンブリ バインドになります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-518">For example, after you make the above changes, the assembly bindings should look like this:</span></span>

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      <span data-ttu-id="b03ce-519">MVC 5、MVC 4 プロジェクトをアップグレードする方法については、次を参照してください。 [ASP.NET MVC 5 と Web API 2 に、ASP.NET MVC 4 と Web API プロジェクトをアップグレードする方法](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-519">For information on upgrading MVC 4 projects to MVC 5, see [How to Upgrade an ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span></span>
3. <span data-ttu-id="b03ce-520">Jquery Unobtrusive Validation をクライアント側の検証を使用する場合、検証メッセージが正しくない場合があります型の HTML input 要素を = 'number' です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-520">When using client-side validation with jQuery Unobtrusive Validation, the validation message is sometimes incorrect for an HTML input element with type='number'.</span></span> <span data-ttu-id="b03ce-521">検証エラーが必須の値 (「[Age] フィールドが必要です」) が表示されます、正しいメッセージの有効な数値が必要であるのではなく無効な数値を入力する場合。</span><span class="sxs-lookup"><span data-stu-id="b03ce-521">The validation error for a required value ("The Age field is required") is shown when an invalid number is entered instead of the correct message that a valid number is required.</span></span>

    <span data-ttu-id="b03ce-522">Create と Edit ビューで整数のプロパティを持つモデルのこの問題が検出スキャフォールディングされたコードで一般的です。</span><span class="sxs-lookup"><span data-stu-id="b03ce-522">This issue is commonly found with scaffolded code for a model with an integer property on the Create and Edit views.</span></span>

    <span data-ttu-id="b03ce-523">この問題を回避するからエディター ヘルパーを変更します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-523">To work around this issue, change the editor helper from:</span></span>

    `@Html.EditorFor(person => person.Age)`

    <span data-ttu-id="b03ce-524">移動先:</span><span class="sxs-lookup"><span data-stu-id="b03ce-524">To:</span></span>

    `@Html.TextBoxFor(person => person.Age)`
4. <span data-ttu-id="b03ce-525">ASP.NET MVC 5 には、部分信頼がサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="b03ce-525">ASP.NET MVC 5 no longer supports partial trust.</span></span> <span data-ttu-id="b03ce-526">MVC または web Api のバイナリにリンクするプロジェクトを削除する必要があります、 [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx)属性と[AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx)属性。</span><span class="sxs-lookup"><span data-stu-id="b03ce-526">Projects linking to the MVC or WebAPI binaries should remove the [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) attribute and the [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) attribute.</span></span> <span data-ttu-id="b03ce-527">これらの属性を削除するには、次のようにコンパイラ エラーがなくなります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-527">Removing these attributes will eliminate compiler errors such as the following.</span></span>

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > <span data-ttu-id="b03ce-528">ただし、このことの副作用は、同じアプリケーションで 4.0、5.0 のアセンブリを使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="b03ce-528">Note, as a side effect of this you cannot use 4.0 and 5.0 assemblies in the same application.</span></span> <span data-ttu-id="b03ce-529">5.0 にそれらのすべてを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-529">You need to update all of them to 5.0.</span></span>

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a><span data-ttu-id="b03ce-530">Facebook の承認を持つ SPA テンプレート、web サイトがイントラネット ゾーンでホストされている IE で不安定になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b03ce-530">SPA Template with Facebook authorization may cause instability in IE while the web site is hosted in intranet zone</span></span>

<span data-ttu-id="b03ce-531">SPA テンプレートは、外部で Facebook ログインを提供します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-531">The SPA template provides external log in with Facebook.</span></span> <span data-ttu-id="b03ce-532">テンプレートで作成されたプロジェクトはローカルで実行してサインイン発生に IE がクラッシュします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-532">When the project created with the template is running locally, signing in may cause IE to crash.</span></span>

<span data-ttu-id="b03ce-533">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="b03ce-533">Solution:</span></span>

1. <span data-ttu-id="b03ce-534">インターネット ゾーンでの web サイトのホストまたは</span><span class="sxs-lookup"><span data-stu-id="b03ce-534">Host the web site in internet zone; or</span></span>

2. <span data-ttu-id="b03ce-535">IE 以外のブラウザーでは、シナリオをテストします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-535">Test the scenario in a browser other than IE.</span></span>

### <a name="web-forms-scaffolding"></a><span data-ttu-id="b03ce-536">Web フォームのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="b03ce-536">Web Forms Scaffolding</span></span>

<span data-ttu-id="b03ce-537">Web フォームのスキャフォールディングでは、VS2013 からが削除され、Visual Studio に対する今後の更新で提供されます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-537">Web Forms Scaffolding has been removed from VS2013 and will be available in a future update to Visual Studio.</span></span> <span data-ttu-id="b03ce-538">ただし、MVC 依存関係を追加して、MVC のスキャフォールディングを生成する、Web フォーム プロジェクト内でスキャフォールディングを引き続き使用することができます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-538">However, you can still use scaffolding within a Web Forms project by adding MVC dependencies and generating scaffolding for MVC.</span></span> <span data-ttu-id="b03ce-539">プロジェクトは、Web フォームと MVC の組み合わせが格納されます。</span><span class="sxs-lookup"><span data-stu-id="b03ce-539">Your project will contain a combination of Web Forms and MVC.</span></span>

<span data-ttu-id="b03ce-540">MVC は、Web フォーム プロジェクトに追加するに新しくスキャフォールディングした項目を追加および選択**MVC 5 依存関係**します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-540">To add MVC to your Web Forms project, add a new scaffolded item and select **MVC 5 Dependencies**.</span></span> <span data-ttu-id="b03ce-541">最小またはスクリプトなどのコンテンツ ファイルのすべてを必要とするかどうかに応じて完全のいずれかを選択します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-541">Select either Minimal or Full depending on whether you need all of the content files, such as scripts.</span></span> <span data-ttu-id="b03ce-542">次に、プロジェクト内のビューとコント ローラーを作成する MVC のスキャフォールディングされた項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-542">Then, add a scaffolded item for MVC, which will create views and a controller in your project.</span></span>

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="b03ce-543">MVC と Web API スキャフォールティング - HTTP 404 Not Found エラー</span><span class="sxs-lookup"><span data-stu-id="b03ce-543">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="b03ce-544">スキャフォールディングされた項目をプロジェクトに追加するときにエラーが発生した場合は、プロジェクトは、不整合な状態になることが勧めします。</span><span class="sxs-lookup"><span data-stu-id="b03ce-544">If an error is encountered when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="b03ce-545">一部のスキャフォールディングが行われた変更はロールバックされますが、インストール済みの NuGet パッケージなどの他の変更はロールバックされません。</span><span class="sxs-lookup"><span data-stu-id="b03ce-545">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="b03ce-546">ルーティング構成の変更がロールバックされた場合、ユーザーはスキャフォールディングされた項目に移動するときに、HTTP 404 エラーを受信します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-546">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="b03ce-547">回避策:</span><span class="sxs-lookup"><span data-stu-id="b03ce-547">Workaround:</span></span>

- <span data-ttu-id="b03ce-548">MVC のこのエラーを修正する新しくスキャフォールディングした項目を追加し、MVC 5 依存関係を選択します (最小または完全のいずれか)。</span><span class="sxs-lookup"><span data-stu-id="b03ce-548">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="b03ce-549">このプロセスでは、プロジェクトに追加のすべての必要な変更します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-549">This process will add all of the required changes to your project.</span></span>
- <span data-ttu-id="b03ce-550">Web api には、このエラーを解決します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-550">To fix this error for Web API:</span></span>

  1. <span data-ttu-id="b03ce-551">プロジェクトには、WebApiConfig クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-551">Add the WebApiConfig class to your project.</span></span>

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. <span data-ttu-id="b03ce-552">WebApiConfig.Register をアプリケーションで構成\_Global.asax でメソッドを次のように開始します。</span><span class="sxs-lookup"><span data-stu-id="b03ce-552">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
