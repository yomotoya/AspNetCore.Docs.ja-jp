---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET および Web Tools for Visual Studio 2013 のリリース ノート |Microsoft ドキュメント
author: microsoft
description: このドキュメントでは、ASP.NET および Web ツールの Visual Studio 2013 のリリースについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: e9ddd96f186564834ff6bb2c30cf0ed5444cbf1b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877802"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a><span data-ttu-id="1c109-103">ASP.NET および Web Tools for Visual Studio 2013 のリリース ノート</span><span class="sxs-lookup"><span data-stu-id="1c109-103">ASP.NET and Web Tools for Visual Studio 2013 Release Notes</span></span>
====================
<span data-ttu-id="1c109-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1c109-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1c109-105">このドキュメントでは、ASP.NET および Web ツールの Visual Studio 2013 のリリースについて説明します。</span><span class="sxs-lookup"><span data-stu-id="1c109-105">This document describes the release of ASP.NET and Web Tools for Visual Studio 2013.</span></span>


## <a name="contents"></a><span data-ttu-id="1c109-106">目次</span><span class="sxs-lookup"><span data-stu-id="1c109-106">Contents</span></span>

- [<span data-ttu-id="1c109-107">インストールに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="1c109-107">Installation Notes</span></span>](#TOC1)
- [<span data-ttu-id="1c109-108">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="1c109-108">Documentation</span></span>](#TOC2)
- [<span data-ttu-id="1c109-109">ソフトウェアの要件</span><span class="sxs-lookup"><span data-stu-id="1c109-109">Software Requirements</span></span>](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a><span data-ttu-id="1c109-110">ASP.NET および Web Tools for Visual Studio 2013 の新機能</span><span class="sxs-lookup"><span data-stu-id="1c109-110">New Features in ASP.NET and Web Tools for Visual Studio 2013</span></span>

- [<span data-ttu-id="1c109-111">One ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1c109-111">One ASP.NET</span></span>](#TOC6)
- [<span data-ttu-id="1c109-112">新しい Web プロジェクトのエクスペリエンス</span><span class="sxs-lookup"><span data-stu-id="1c109-112">New Web Project Experience</span></span>](#newproj)
- [<span data-ttu-id="1c109-113">ASP.NET のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="1c109-113">ASP.NET Scaffolding</span></span>](#scaffold)
- [<span data-ttu-id="1c109-114">ブラウザー リンク</span><span class="sxs-lookup"><span data-stu-id="1c109-114">Browser Link</span></span>](#browser-link)
- [<span data-ttu-id="1c109-115">Visual Studio Web エディターの機能強化</span><span class="sxs-lookup"><span data-stu-id="1c109-115">Visual Studio Web Editor Enhancements</span></span>](#web-editor)
- [<span data-ttu-id="1c109-116">Visual Studio での azure App Service Web アプリのサポート</span><span class="sxs-lookup"><span data-stu-id="1c109-116">Azure App Service Web Apps Support in Visual Studio</span></span>](#waws)
- [<span data-ttu-id="1c109-117">Web の発行の機能強化</span><span class="sxs-lookup"><span data-stu-id="1c109-117">Web Publish Enhancements</span></span>](#publish)
- [<span data-ttu-id="1c109-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="1c109-118">NuGet 2.7</span></span>](#nuget)
- [<span data-ttu-id="1c109-119">ASP.NET Web フォーム</span><span class="sxs-lookup"><span data-stu-id="1c109-119">ASP.NET Web Forms</span></span>](#TOC9)
- [<span data-ttu-id="1c109-120">ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="1c109-120">ASP.NET MVC 5</span></span>](#TOC10)
- [<span data-ttu-id="1c109-121">ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="1c109-121">ASP.NET Web API 2</span></span>](#TOC11)
- [<span data-ttu-id="1c109-122">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="1c109-122">ASP.NET SignalR</span></span>](#TOC13)
- [<span data-ttu-id="1c109-123">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="1c109-123">ASP.NET Identity</span></span>](#TOC8)
- [<span data-ttu-id="1c109-124">Microsoft OWIN コンポーネント</span><span class="sxs-lookup"><span data-stu-id="1c109-124">Microsoft OWIN Components</span></span>](#TOC7)
- [<span data-ttu-id="1c109-125">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="1c109-125">Entity Framework 6</span></span>](#ef6)
- [<span data-ttu-id="1c109-126">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="1c109-126">ASP.NET Razor 3</span></span>](#TOC14)
- [<span data-ttu-id="1c109-127">ASP.NET アプリを中断します。</span><span class="sxs-lookup"><span data-stu-id="1c109-127">ASP.NET App Suspend</span></span>](#TOC15)
- [<span data-ttu-id="1c109-128">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="1c109-128">Known Issues and Breaking Changes</span></span>](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a><span data-ttu-id="1c109-129">インストールに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="1c109-129">Installation Notes</span></span>

<span data-ttu-id="1c109-130">ASP.NET および Visual Studio 2013 用 Web ツールがメインのインストーラーにバンドルおよびダウンロードできます[ここ](https://www.asp.net/downloads)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-130">ASP.NET and Web Tools for Visual Studio 2013 are bundled in the main installer and can be downloaded [here](https://www.asp.net/downloads).</span></span>

<a id="TOC2"></a>
## <a name="documentation"></a><span data-ttu-id="1c109-131">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="1c109-131">Documentation</span></span>

<span data-ttu-id="1c109-132">チュートリアルおよび Visual Studio 2013 用 ASP.NET および Web ツールに関するその他の情報は、 [ASP.NET web サイト](https://www.asp.net/)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-132">Tutorials and other information about ASP.NET and Web Tools for Visual Studio 2013 are available from the [ASP.NET web site](https://www.asp.net/).</span></span>

<a id="TOC4"></a>
## <a name="software-requirements"></a><span data-ttu-id="1c109-133">ソフトウェア要件</span><span class="sxs-lookup"><span data-stu-id="1c109-133">Software Requirements</span></span>

<span data-ttu-id="1c109-134">ASP.NET および Web ツール Visual Studio 2013 が必要です。</span><span class="sxs-lookup"><span data-stu-id="1c109-134">ASP.NET and Web Tools requires Visual Studio 2013.</span></span>

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a><span data-ttu-id="1c109-135">ASP.NET および Web Tools for Visual Studio 2013 の新機能</span><span class="sxs-lookup"><span data-stu-id="1c109-135">New Features in ASP.NET and Web Tools for Visual Studio 2013</span></span>

<span data-ttu-id="1c109-136">次のセクションでは、リリースで導入された機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="1c109-136">The following sections describe the features that have been introduced in the release.</span></span>

<a id="TOC6"></a>
## <a name="one-aspnet"></a><span data-ttu-id="1c109-137">One ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1c109-137">One ASP.NET</span></span>

<span data-ttu-id="1c109-138">Visual Studio 2013 のリリースで混在したりしたい場合に一致を簡単にできるように、ASP.NET テクノロジを使用する場合の動作を統合するための手段に移動しました。</span><span class="sxs-lookup"><span data-stu-id="1c109-138">With the release of Visual Studio 2013, we have taken a step towards unifying the experience of using ASP.NET technologies, so that you can easily mix and match the ones you want.</span></span> <span data-ttu-id="1c109-139">たとえば、MVC を使用して、プロジェクトを開始および簡単に後で、プロジェクトに Web フォーム ページを追加したり、Web フォーム プロジェクトでの Web Api のスキャフォールディングです。</span><span class="sxs-lookup"><span data-stu-id="1c109-139">For example, you can start a project using MVC and easily add Web Forms pages to the project later, or scaffold Web APIs in a Web Forms project.</span></span> <span data-ttu-id="1c109-140">1 つの ASP.NET はやすくするための ASP.NET でお使いの場合、操作を実行する開発者です。</span><span class="sxs-lookup"><span data-stu-id="1c109-140">One ASP.NET is all about making it easier for you as a developer to do the things you love in ASP.NET.</span></span> <span data-ttu-id="1c109-141">選択するどのようなテクノロジに関係なく、1 つの ASP.NET の信頼されている、基になる framework でビルドする信頼を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-141">No matter what technology you choose, you can have confidence that you are building on the trusted underlying framework of One ASP.NET.</span></span>

<a id="newproj"></a>
## <a name="new-web-project-experience"></a><span data-ttu-id="1c109-142">新しい Web プロジェクトのエクスペリエンス</span><span class="sxs-lookup"><span data-stu-id="1c109-142">New Web Project Experience</span></span>

<span data-ttu-id="1c109-143">Visual Studio 2013 で新しい web プロジェクトを作成する場合の動作を拡張しています。</span><span class="sxs-lookup"><span data-stu-id="1c109-143">We have enhanced the experience of creating new web projects in Visual Studio 2013.</span></span> <span data-ttu-id="1c109-144">**新しい ASP.NET Web プロジェクト**ダイアログ テクノロジ (Web フォーム、MVC、Web API) の任意の組み合わせを構成する、認証オプションを構成し、単体テスト プロジェクトを追加、プロジェクトの種類を選択することができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-144">In the **New ASP.NET Web Project** dialog you can select the project type you want, configure any combination of technologies (Web Forms, MVC, Web API), configure authentication options, and add a unit test project.</span></span>

![新しい ASP.NET プロジェクト](release-notes/_static/image1.png)

<span data-ttu-id="1c109-146">新しいダイアログ ボックスでは、多くのテンプレートの既定の認証オプションを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-146">The new dialog enables you to change the default authentication options for many of the templates.</span></span> <span data-ttu-id="1c109-147">たとえば、ASP.NET Web フォーム プロジェクトを作成するときに、次のオプションのいずれかを選択できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-147">For example, when you create an ASP.NET Web Forms project you can select any of the following options:</span></span>

- <span data-ttu-id="1c109-148">認証なし</span><span class="sxs-lookup"><span data-stu-id="1c109-148">No Authentication</span></span>
- <span data-ttu-id="1c109-149">個々 のユーザー アカウント (ASP.NET メンバーシップまたはのソーシャル プロバイダー ログ)</span><span class="sxs-lookup"><span data-stu-id="1c109-149">Individual User Accounts (ASP.NET membership or social provider log in)</span></span>
- <span data-ttu-id="1c109-150">組織アカウント (インターネット アプリケーションでは Active Directory)</span><span class="sxs-lookup"><span data-stu-id="1c109-150">Organizational Accounts (Active Directory in an internet application)</span></span>
- <span data-ttu-id="1c109-151">Windows 認証 (イントラネット アプリケーションの Active Directory)</span><span class="sxs-lookup"><span data-stu-id="1c109-151">Windows Authentication (Active Directory in an intranet application)</span></span>

![認証オプション](release-notes/_static/image2.png)

<span data-ttu-id="1c109-153">Web プロジェクトを作成するための新しいプロセスの詳細については、次を参照してください。 [Visual Studio 2013 で ASP.NET Web プロジェクトの作成](creating-web-projects-in-visual-studio.md)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-153">For more information about the new process for creating web projects, see [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span></span> <span data-ttu-id="1c109-154">新しい認証オプションの詳細については、次を参照してください。 [ASP.NET Identity](#TOC8)このドキュメントで後述します。</span><span class="sxs-lookup"><span data-stu-id="1c109-154">For more information about the new authentication options, see [ASP.NET Identity](#TOC8) later in this document.</span></span>

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a><span data-ttu-id="1c109-155">ASP.NET のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="1c109-155">ASP.NET Scaffolding</span></span>

<span data-ttu-id="1c109-156">ASP.NET のスキャフォールディングとは、ASP.NET Web アプリケーションのコード生成フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="1c109-156">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="1c109-157">により、簡単にデータ モデルと連携するプロジェクトに定型コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="1c109-157">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="1c109-158">Visual Studio の以前のバージョンでは、スキャフォールディングは、ASP.NET MVC プロジェクトに制限されていました。</span><span class="sxs-lookup"><span data-stu-id="1c109-158">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="1c109-159">Visual Studio 2013 で Web フォームを含む、すべての ASP.NET プロジェクトのスキャフォールディングを使用できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-159">With Visual Studio 2013, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="1c109-160">Visual Studio 2013 はサポートされていません生成のページは、Web フォーム プロジェクトがプロジェクトに MVC 依存関係を追加することで、Web フォームのスキャフォールディングを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="1c109-160">Visual Studio 2013 does not currently support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="1c109-161">Web フォームのページを生成するためのサポートは、今後の更新で追加されます。</span><span class="sxs-lookup"><span data-stu-id="1c109-161">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="1c109-162">スキャフォールディングを使用する場合は、必要なすべての依存関係がプロジェクトにインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1c109-162">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="1c109-163">たとえば、ASP.NET Web フォーム プロジェクトを開始して、スキャフォールディングを使用して Web API コント ローラーを追加した場合、必要な NuGet パッケージと参照をプロジェクトに自動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="1c109-163">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="1c109-164">MVC のスキャフォールディングを Web フォーム プロジェクトに追加するには、追加、**スキャフォールディングされた新しい項目**選択**MVC 5 の依存関係**ダイアログ ウィンドウでします。</span><span class="sxs-lookup"><span data-stu-id="1c109-164">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="1c109-165">MVC; スキャフォールディングの 2 つのオプションがあります。最小限かつ完全します。</span><span class="sxs-lookup"><span data-stu-id="1c109-165">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="1c109-166">最低限を選択した場合は、NuGet パッケージの管理と ASP.NET MVC の参照のみがプロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="1c109-166">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="1c109-167">[完全] オプションを選択した場合、MVC プロジェクトの必要なコンテンツ ファイルだけでなく、最小の依存関係が追加されます。</span><span class="sxs-lookup"><span data-stu-id="1c109-167">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="1c109-168">非同期コント ローラーをスキャフォールディングのサポートは、Entity Framework 6 から新しい非同期機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="1c109-168">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="1c109-169">詳細な情報およびチュートリアルについては、次を参照してください。 [ASP.NET スキャフォールディング概要](aspnet-scaffolding-overview.md)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-169">For more information and tutorials, see [ASP.NET Scaffolding Overview](aspnet-scaffolding-overview.md).</span></span>

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a><span data-ttu-id="1c109-170">ブラウザー リンク: ブラウザーと Visual Studio の間の SignalR チャネル</span><span class="sxs-lookup"><span data-stu-id="1c109-170">Browser Link – SignalR channel between browser and Visual Studio</span></span>

<span data-ttu-id="1c109-171">新しい[Browser Link](using-browser-link.md)機能では、複数のブラウザーを Visual Studio に接続し、ツールバーのボタンをクリックするだけで更新したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-171">The new [Browser Link](using-browser-link.md) feature lets you connect multiple browsers to Visual Studio and refresh them all by clicking a button in the toolbar.</span></span> <span data-ttu-id="1c109-172">モバイル エミュレーターを含め、開発サイトを複数のブラウザーを接続し、同時にすべて更新する更新のすべてのブラウザーをクリックできます。</span><span class="sxs-lookup"><span data-stu-id="1c109-172">You can connect multiple browsers to your development site, including mobile emulators, and click refresh to refresh all the browsers all at the same time.</span></span> <span data-ttu-id="1c109-173">Browser Link では、開発者がブラウザー リンク拡張を書き込むための API も公開します。</span><span class="sxs-lookup"><span data-stu-id="1c109-173">Browser Link also exposes an API to enable developers to write Browser Link extensions.</span></span>

![](release-notes/_static/image3.png)

<span data-ttu-id="1c109-174">ブラウザー リンク API を利用する開発者を有効にするには、Visual Studio と接続されている任意のブラウザー間の境界を越える非常に高度なシナリオを作成することなります。</span><span class="sxs-lookup"><span data-stu-id="1c109-174">By enabling developers to take advantage of the Browser Link API, it becomes possible to create very advanced scenarios that crosses boundaries between Visual Studio and any browser that's connected.</span></span> <span data-ttu-id="1c109-175">Web Essentials では、Visual Studio とブラウザーの開発ツール、リモートのモバイル エミュレーターと、はるかに多くの制御間統合エクスペリエンスを提供する API を活用します。</span><span class="sxs-lookup"><span data-stu-id="1c109-175">Web Essentials takes advantage of the API to create an integrated experience between Visual Studio and the browser's developer tools, remote controlling mobile emulators and a lot more.</span></span>

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a><span data-ttu-id="1c109-176">Visual Studio Web エディターの機能強化</span><span class="sxs-lookup"><span data-stu-id="1c109-176">Visual Studio Web Editor Enhancements</span></span>

<span data-ttu-id="1c109-177">Visual Studio 2013 には、web アプリケーションで Razor ファイルや HTML ファイルに新しい HTML エディターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1c109-177">Visual Studio 2013 includes a new HTML editor for Razor files and HTML files in web applications.</span></span> <span data-ttu-id="1c109-178">新しい HTML エディターは、HTML5 に基づいて 1 つの統一されたスキーマを提供します。</span><span class="sxs-lookup"><span data-stu-id="1c109-178">The new HTML editor provides a single unified schema based on HTML5.</span></span> <span data-ttu-id="1c109-179">自動の中かっこの補完、jQuery UI、および AngularJS IntelliSense の属性、属性 IntelliSense のグループ化、ID とクラス名、Intellisense、パフォーマンスの向上、書式設定を含むその他の改善があるとスマート タグです。</span><span class="sxs-lookup"><span data-stu-id="1c109-179">It has automatic brace completion, jQuery UI and AngularJS attribute IntelliSense, attribute IntelliSense Grouping, ID and class name Intellisense, and other improvements including better performance, formatting and SmartTags.</span></span>

<span data-ttu-id="1c109-180">次のスクリーン ショットでは、HTML エディターのブートス トラップ属性 IntelliSense の使用方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1c109-180">The following screenshot demonstrates using Bootstrap attribute IntelliSense in the HTML editor.</span></span>

![HTML エディターの Intellisense](release-notes/_static/image4.png)

<span data-ttu-id="1c109-182">Visual Studio 2013 には、両方の CoffeeScript とに組み込まれているエディターも付属しています。</span><span class="sxs-lookup"><span data-stu-id="1c109-182">Visual Studio 2013 also comes with both CoffeeScript and LESS editors built in.</span></span> <span data-ttu-id="1c109-183">LESS エディター、CSS エディターから便利な機能がすべて付属しており mixins および変数の特定の Intellisense の少ないすべてのドキュメントに、@importチェーン。</span><span class="sxs-lookup"><span data-stu-id="1c109-183">The LESS editor comes with all the cool features from the CSS editor and has specific Intellisense for variables and mixins across all the LESS documents in the @import chain.</span></span>

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a><span data-ttu-id="1c109-184">Visual Studio での azure App Service Web アプリのサポート</span><span class="sxs-lookup"><span data-stu-id="1c109-184">Azure App Service Web Apps Support in Visual Studio</span></span>

<span data-ttu-id="1c109-185">Azure SDK for .NET 2.2 で Visual Studio 2013 で使用できます**サーバー エクスプ ローラー**リモートの web アプリと直接通信します。</span><span class="sxs-lookup"><span data-stu-id="1c109-185">In Visual Studio 2013 with the Azure SDK for .NET 2.2, you can use **Server Explorer** to interact directly with your remote web apps.</span></span> <span data-ttu-id="1c109-186">Azure アカウントにサインインすることができます新しい web アプリを作成する、アプリの構成、リアルタイムのログ、および詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="1c109-186">You can sign in to your Azure account, create new web apps, configure apps, view real-time logs, and more.</span></span> <span data-ttu-id="1c109-187">SDK 2.2 後すぐに聞こえるがリリースされたら、Azure でリモートでデバッグ モードで実行することができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-187">Coming soon after SDK 2.2 is released, you'll be able to run in debug mode remotely in Azure.</span></span> <span data-ttu-id="1c109-188">Azure App Service Web Apps の新機能のほとんどは、現在のリリースの Azure SDK for .NET をインストールすると、Visual Studio 2012 で動作もします。</span><span class="sxs-lookup"><span data-stu-id="1c109-188">Most of the new features for Azure App Service Web Apps also work in Visual Studio 2012 when you install the current release of the Azure SDK for .NET.</span></span>

<span data-ttu-id="1c109-189">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="1c109-189">For more information, see the following resources:</span></span>

- [<span data-ttu-id="1c109-190">Azure App service の ASP.NET web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="1c109-190">Create an ASP.NET web app in Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [<span data-ttu-id="1c109-191">Visual Studio を使用して Azure App service web アプリをトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="1c109-191">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a><span data-ttu-id="1c109-192">Web の発行の機能強化</span><span class="sxs-lookup"><span data-stu-id="1c109-192">Web Publish Enhancements</span></span>

<span data-ttu-id="1c109-193">Visual Studio 2013 には、新機能および強化された Web 発行機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="1c109-193">Visual Studio 2013 includes new and enhanced Web Publish features.</span></span> <span data-ttu-id="1c109-194">次に、その一部を示します。</span><span class="sxs-lookup"><span data-stu-id="1c109-194">Here are a few of them:</span></span>

- <span data-ttu-id="1c109-195">簡単に[Web.config ファイルの暗号化を自動化](https://go.microsoft.com/fwlink/?LinkId=325529)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-195">Easily [automate Web.config file encryption](https://go.microsoft.com/fwlink/?LinkId=325529).</span></span> <span data-ttu-id="1c109-196">(このリンクし、次の 2 つ) をポイント 10/17 の 1 日の終わりまで使用できない可能性がある MSDN のドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="1c109-196">(This link and the following two point to documentation on MSDN that might not be available until late in the day on 10/17.)</span></span>
- <span data-ttu-id="1c109-197">簡単に[配置時に、アプリケーションをオフラインの取得を自動化](https://go.microsoft.com/fwlink/?LinkId=325530)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-197">Easily [automate taking an application offline during deployment](https://go.microsoft.com/fwlink/?LinkId=325530).</span></span>
- <span data-ttu-id="1c109-198">Web デプロイを構成する[最終変更日ではなくファイルのチェックサムを使用して](https://go.microsoft.com/fwlink/?LinkId=325531)ファイルは、サーバーにコピーするかを決定するためです。</span><span class="sxs-lookup"><span data-stu-id="1c109-198">Configure Web Deploy to [use file checksum instead of last-changed date](https://go.microsoft.com/fwlink/?LinkId=325531) for determining which files should be copied to the server.</span></span>
- <span data-ttu-id="1c109-199">迅速に FTP を使用しているか、ファイル システムのメソッドを公開するときにだけでなく Web 配置で個々 の選択したファイル (Web.config を含む) を発行します。</span><span class="sxs-lookup"><span data-stu-id="1c109-199">Quickly publish individual selected files (including Web.config) when you're using the FTP or file system publish methods as well as with Web Deploy.</span></span>

<span data-ttu-id="1c109-200">ASP.NET web 配置の詳細については、次を参照してください。 [、ASP.NET サイト](https://go.microsoft.com/fwlink/?LinkId=322027)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-200">For more information about ASP.NET web deployment, see [the ASP.NET site](https://go.microsoft.com/fwlink/?LinkId=322027).</span></span>

<a id="nuget"></a>
## <a name="nuget-27"></a><span data-ttu-id="1c109-201">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="1c109-201">NuGet 2.7</span></span>

<span data-ttu-id="1c109-202">NuGet 2.7 にはで詳細に説明される新機能の豊富なセットが含まれます[NuGet 2.7 リリース ノート](http://docs.nuget.org/docs/release-notes/nuget-2.7)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-202">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="1c109-203">このバージョンの NuGet では、NuGet のパッケージ復元機能は、パッケージをダウンロードするための明示的な同意を提供する必要性も削除されます。</span><span class="sxs-lookup"><span data-stu-id="1c109-203">This version of NuGet also removes the need to provide explicit consent for NuGet's package restore feature to download packages.</span></span> <span data-ttu-id="1c109-204">NuGet をインストールすることで同意 (NuGet の環境設定 ダイアログに関連付けられているチェック ボックスと) が付与されました。</span><span class="sxs-lookup"><span data-stu-id="1c109-204">Consent (and the associated checkbox in NuGet's preferences dialog) is now granted by installing NuGet.</span></span> <span data-ttu-id="1c109-205">ここでパッケージの復元は、既定では単に動作します。</span><span class="sxs-lookup"><span data-stu-id="1c109-205">Now package restore simply works by default.</span></span>

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a><span data-ttu-id="1c109-206">ASP.NET Web フォーム</span><span class="sxs-lookup"><span data-stu-id="1c109-206">ASP.NET Web Forms</span></span>

### <a name="one-aspnet"></a><span data-ttu-id="1c109-207">One ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1c109-207">One ASP.NET</span></span>

<span data-ttu-id="1c109-208">Web フォーム プロジェクト テンプレートは、新しい 1 つの ASP.NET エクスペリエンスとシームレスに統合します。</span><span class="sxs-lookup"><span data-stu-id="1c109-208">The Web Forms project templates integrate seamlessly with the new One ASP.NET experience.</span></span> <span data-ttu-id="1c109-209">Web フォーム プロジェクトに MVC と Web API がサポートされており、1 つの ASP.NET プロジェクトの作成ウィザードを使用して認証を構成することができますを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-209">You can add MVC and Web API support to your Web Forms project, and you can configure authentication using the One ASP.NET project creation wizard.</span></span> <span data-ttu-id="1c109-210">詳細については、次を参照してください。 [Visual Studio 2013 で ASP.NET Web プロジェクトの作成](creating-web-projects-in-visual-studio.md)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-210">For more information, see [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span></span>

### <a name="aspnet-identity"></a><span data-ttu-id="1c109-211">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="1c109-211">ASP.NET Identity</span></span>

<span data-ttu-id="1c109-212">Web フォーム プロジェクト テンプレートでは、新しい ASP.NET Identity framework をサポートします。</span><span class="sxs-lookup"><span data-stu-id="1c109-212">The Web Forms project templates support the new ASP.NET Identity framework.</span></span> <span data-ttu-id="1c109-213">さらに、テンプレートは、Web フォーム イントラネット プロジェクトの作成をサポートするようになりました。</span><span class="sxs-lookup"><span data-stu-id="1c109-213">In addition, the templates now support creation of a Web Forms intranet project.</span></span> <span data-ttu-id="1c109-214">詳細については、次を参照してください。[認証方法](creating-web-projects-in-visual-studio.md#auth)で**Visual Studio 2013 で ASP.NET Web プロジェクトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="1c109-214">For more information, see [Authentication Methods](creating-web-projects-in-visual-studio.md#auth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="bootstrap"></a><span data-ttu-id="1c109-215">ブートス トラップ</span><span class="sxs-lookup"><span data-stu-id="1c109-215">Bootstrap</span></span>

<span data-ttu-id="1c109-216">Web フォーム テンプレートを使用して[ブートス トラップ](http://twitter.github.io/bootstrap/)光沢のあるや応答性のルック アンド フィールを簡単にカスタマイズすることができますを提供します。</span><span class="sxs-lookup"><span data-stu-id="1c109-216">The Web Forms templates use [Bootstrap](http://twitter.github.io/bootstrap/) to provide a sleek and responsive look and feel that you can easily customize.</span></span> <span data-ttu-id="1c109-217">詳細については、次を参照してください。 [Visual Studio 2013 の web プロジェクト テンプレートでブートス トラップ](creating-web-projects-in-visual-studio.md#bootstrap)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-217">For more information, see [Bootstrap in the Visual Studio 2013 web project templates](creating-web-projects-in-visual-studio.md#bootstrap).</span></span>

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a><span data-ttu-id="1c109-218">ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="1c109-218">ASP.NET MVC 5</span></span>

### <a name="one-aspnet"></a><span data-ttu-id="1c109-219">One ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1c109-219">One ASP.NET</span></span>

<span data-ttu-id="1c109-220">Web の MVC プロジェクト テンプレートは、新しい 1 つの ASP.NET エクスペリエンスとシームレスに統合します。</span><span class="sxs-lookup"><span data-stu-id="1c109-220">The Web MVC project templates integrate seamlessly with the new One ASP.NET experience.</span></span> <span data-ttu-id="1c109-221">MVC プロジェクトをカスタマイズし、1 つの ASP.NET プロジェクトの作成ウィザードを使用して認証を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-221">You can customize your MVC project and configure authentication using the One ASP.NET project creation wizard.</span></span> <span data-ttu-id="1c109-222">ASP.NET MVC 5 に入門チュートリアルはあります[ASP.NET MVC 5 の概要](../../../mvc/overview/getting-started/introduction/getting-started.md)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-222">An introductory tutorial to ASP.NET MVC 5 can be found at [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="1c109-223">MVC 5 に MVC 4 プロジェクトをアップグレードする方法については、次を参照してください。 [ASP.NET MVC 5 と Web API 2 に、ASP.NET MVC 4 と Web API プロジェクトをアップグレードする方法](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-223">For information on upgrading MVC 4 projects to MVC 5, see [How to Upgrade an ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span></span>

### <a name="aspnet-identity"></a><span data-ttu-id="1c109-224">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="1c109-224">ASP.NET Identity</span></span>

<span data-ttu-id="1c109-225">ASP.NET の Id を使用して認証と id 管理には、MVC プロジェクト テンプレートが更新されました。</span><span class="sxs-lookup"><span data-stu-id="1c109-225">The MVC project templates have been updated to use ASP.NET Identity for authentication and identity management.</span></span> <span data-ttu-id="1c109-226">提供され、Facebook、Google の認証と新しいメンバーシップ API のチュートリアルが見つかります[Facebook、Google OAuth2 および OpenID サイン オンで、ASP.NET MVC 5 アプリを作成](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)と[認証で ASP.NET MVC アプリケーションを作成し、SQL DB し、Azure App Service に展開](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-226">A tutorial featuring Facebook and Google authentication and the new membership API can be found at [Create an ASP.NET MVC 5 App with Facebook and Google OAuth2 and OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) and [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).</span></span>

### <a name="bootstrap"></a><span data-ttu-id="1c109-227">ブートス トラップ</span><span class="sxs-lookup"><span data-stu-id="1c109-227">Bootstrap</span></span>

<span data-ttu-id="1c109-228">使用する MVC プロジェクト テンプレートが更新されました[ブートス トラップ](http://getbootstrap.com/)光沢のあるや応答性のルック アンド フィールを簡単にカスタマイズすることができますを提供します。</span><span class="sxs-lookup"><span data-stu-id="1c109-228">The MVC project template has been updated to use [Bootstrap](http://getbootstrap.com/) to provide a sleek and responsive look and feel that you can easily customize.</span></span> <span data-ttu-id="1c109-229">詳細については、次を参照してください。 [Visual Studio 2013 の web プロジェクト テンプレートでブートス トラップ](creating-web-projects-in-visual-studio.md#bootstrap)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-229">For more information, see [Bootstrap in the Visual Studio 2013 web project templates](creating-web-projects-in-visual-studio.md#bootstrap).</span></span>

### <a name="authentication-filters"></a><span data-ttu-id="1c109-230">認証フィルター</span><span class="sxs-lookup"><span data-stu-id="1c109-230">Authentication filters</span></span>

<span data-ttu-id="1c109-231">認証フィルターは、新しい ASP.NET MVC パイプラインでの承認フィルターの前に実行し、認証ロジックごとのアクションを指定することを ASP.NET MVC でのフィルターの種類ごとのコント ローラー、またはすべてのコント ローラーに対してグローバルにします。</span><span class="sxs-lookup"><span data-stu-id="1c109-231">Authentication filters are a new kind of filter in ASP.NET MVC that run prior to authorization filters in the ASP.NET MVC pipeline and allow you to specify authentication logic per-action, per-controller, or globally for all controllers.</span></span> <span data-ttu-id="1c109-232">認証フィルターは、要求に資格情報を処理し、対応するプリンシパルを提供します。</span><span class="sxs-lookup"><span data-stu-id="1c109-232">Authentication filters process credentials in the request and provide a corresponding principal.</span></span> <span data-ttu-id="1c109-233">認証フィルターは、未承認の要求に応答認証チャレンジを追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="1c109-233">Authentication filters can also add authentication challenges in response to unauthorized requests.</span></span>

### <a name="filter-overrides"></a><span data-ttu-id="1c109-234">上書きをフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="1c109-234">Filter overrides</span></span>

<span data-ttu-id="1c109-235">オーバーライド フィルターを指定して特定のアクション メソッドまたはコント ローラーに適用するフィルターをオーバーライドすることができますようになりました。</span><span class="sxs-lookup"><span data-stu-id="1c109-235">You can now override which filters apply to a given action method or controller by specifying an override filter.</span></span> <span data-ttu-id="1c109-236">オーバーライド フィルターは、特定のスコープ (アクションまたはコント ローラー) で実行しないでフィルターの種類のセットを指定します。</span><span class="sxs-lookup"><span data-stu-id="1c109-236">Override filters specify a set of filter types that should not be run for a given scope (action or controller).</span></span> <span data-ttu-id="1c109-237">これにより、グローバルに適用が、特定のアクションまたはコント ローラーに適用されない特定のグローバル フィルターを除外するフィルターを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-237">This allows you to configure filters that apply globally but then exclude certain global filters from applying to specific actions or controllers.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="1c109-238">属性ルーティング</span><span class="sxs-lookup"><span data-stu-id="1c109-238">Attribute routing</span></span>

<span data-ttu-id="1c109-239">ASP.NET MVC が Tim McCall の作成者によって貢献感謝の属性がルーティングをサポートして今すぐ[ http://attributerouting.net](http://attributerouting.net)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-239">ASP.NET MVC now supports attribute routing, thanks to a contribution by Tim McCall, the author of [http://attributerouting.net](http://attributerouting.net).</span></span> <span data-ttu-id="1c109-240">属性のルーティングは、アクションとコント ローラーに注釈を付けることによって、ルートを指定できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-240">With attribute routing you can specify your routes by annotating your actions and controllers.</span></span>

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a><span data-ttu-id="1c109-241">ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="1c109-241">ASP.NET Web API 2</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="1c109-242">属性ルーティング</span><span class="sxs-lookup"><span data-stu-id="1c109-242">Attribute routing</span></span>

<span data-ttu-id="1c109-243">ASP.NET Web API が Tim McCall の作成者によって貢献感謝の属性がルーティングをサポートして今すぐ[ http://attributerouting.net](http://attributerouting.net)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-243">ASP.NET Web API now supports attribute routing, thanks to a contribution by Tim McCall, the author of [http://attributerouting.net](http://attributerouting.net).</span></span> <span data-ttu-id="1c109-244">属性のルーティングは、次のようにコント ローラー、アクションに注釈を付けるして、Web API ルートを指定できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-244">With attribute routing you can specify your Web API routes by annotating your actions and controllers like this:</span></span>

[!code-csharp[Main](release-notes/samples/sample1.cs)]

<span data-ttu-id="1c109-245">属性がルーティングすること、Uri より詳細に制御での web API。</span><span class="sxs-lookup"><span data-stu-id="1c109-245">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="1c109-246">たとえば、単一の API コント ローラーを使用してリソースの階層を簡単に定義できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-246">For example, you can easily define a resource hierarchy using a single API controller:</span></span>

[!code-csharp[Main](release-notes/samples/sample2.cs)]

<span data-ttu-id="1c109-247">省略可能なパラメーター、既定値、およびルート制約を指定する便利な構文を提供する属性のルーティングも。</span><span class="sxs-lookup"><span data-stu-id="1c109-247">Attribute routing also provides a convenient syntax for specifying optional parameters, default values, and route constraints:</span></span>

[!code-csharp[Main](release-notes/samples/sample3.cs)]

<span data-ttu-id="1c109-248">属性のルーティングの詳細については、次を参照してください。 [Web API 2 での属性のルーティング](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-248">For more information about attribute routing, see [Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).</span></span>

### <a name="oauth-20"></a><span data-ttu-id="1c109-249">OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="1c109-249">OAuth 2.0</span></span>

<span data-ttu-id="1c109-250">Web API およびシングル ページ アプリケーション プロジェクト テンプレートでは、OAuth 2.0 を使用して認証できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="1c109-250">The Web API and Single Page Application project templates now support authorization using OAuth 2.0.</span></span> <span data-ttu-id="1c109-251">OAuth 2.0 は、保護されたリソースへのクライアント アクセスを承認するためのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="1c109-251">OAuth 2.0 is a framework for authorizing client access to protected resources.</span></span> <span data-ttu-id="1c109-252">この機能は、さまざまなブラウザーやモバイル デバイスを含む、クライアントに対して機能します。</span><span class="sxs-lookup"><span data-stu-id="1c109-252">It works for a variety of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="1c109-253">OAuth 2.0 のサポートは、新しいセキュリティ ミドルウェア ベアラ認証のため、Microsoft OWIN コンポーネントによって提供され、承認サーバーの役割の実装に基づいています。</span><span class="sxs-lookup"><span data-stu-id="1c109-253">Support for OAuth 2.0 is based on new security middleware provided by the Microsoft OWIN Components for bearer authentication and implementing the authorization server role.</span></span> <span data-ttu-id="1c109-254">また、クライアントは、Azure Active Directory または Windows Server 2012 R2 での ADFS など、組織の承認サーバーを使用して承認できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-254">Alternatively, clients can be authorized using an organizational authorization server, such as Azure Active Directory or ADFS in Windows Server 2012 R2.</span></span>

### <a name="odata-improvements"></a><span data-ttu-id="1c109-255">OData の機能強化</span><span class="sxs-lookup"><span data-stu-id="1c109-255">OData Improvements</span></span>

<span data-ttu-id="1c109-256">**$Select、$ のサポートを展開し、$batch、$value**</span><span class="sxs-lookup"><span data-stu-id="1c109-256">**Support for $select, $expand, $batch, and $value**</span></span>

<span data-ttu-id="1c109-257">ASP.NET Web API OData の $select を完全にサポートになりました、$ 順に展開し、$value です。</span><span class="sxs-lookup"><span data-stu-id="1c109-257">ASP.NET Web API OData now has full support for $select, $expand, and $value.</span></span> <span data-ttu-id="1c109-258">バッチ処理と変更セットの処理要求の $batch を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="1c109-258">You can also use $batch for request batching and processing of change sets.</span></span>

<span data-ttu-id="1c109-259">$Select および $expand オプションを展開します。 OData エンドポイントから返されるデータの形状を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-259">The $select and $expand options let you change the shape of the data that is returned from an OData endpoint.</span></span> <span data-ttu-id="1c109-260">詳細については、次を参照してください。[概要 $select および $expand Web API OData のサポートの拡張](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-260">For more information, see [Introducing $select and $expand support in Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).</span></span>

<span data-ttu-id="1c109-261">**強化された機能拡張**</span><span class="sxs-lookup"><span data-stu-id="1c109-261">**Improved extensibility**</span></span>

<span data-ttu-id="1c109-262">OData フォーマッタは、拡張可能なようになりました。</span><span class="sxs-lookup"><span data-stu-id="1c109-262">The OData formatters are now extensible.</span></span> <span data-ttu-id="1c109-263">Atom エントリ メタデータを追加して名前付きストリームとメディア リンク エントリのサポート、インスタンス注釈を追加するか、およびリンクを生成する方法をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="1c109-263">You can add Atom entry metadata, support named stream and media link entries, add instance annotations, and customize how links are generated.</span></span>

<span data-ttu-id="1c109-264">**型のないサポート**</span><span class="sxs-lookup"><span data-stu-id="1c109-264">**Type-less support**</span></span>

<span data-ttu-id="1c109-265">CLR 型、エンティティ型を定義することがなく OData サービスを構築できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-265">You can now build OData services without needing to define CLR types for your entity types.</span></span> <span data-ttu-id="1c109-266">代わりに、OData コント ローラーはまたは実行のインスタンスを返す**IEdmObject**、どの OData フォーマッタは、シリアル化/逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="1c109-266">Instead, your OData controllers can take or return instances of **IEdmObject**, which are the OData formatters serialize/deserialize.</span></span>

<span data-ttu-id="1c109-267">**既存のモデルを再利用します。**</span><span class="sxs-lookup"><span data-stu-id="1c109-267">**Reuse an existing model**</span></span>

<span data-ttu-id="1c109-268">既存 entity data model (EDM) を既にある場合は、今すぐ再利用できることを直接、新しいものを作成する代わりにします。</span><span class="sxs-lookup"><span data-stu-id="1c109-268">If you already have an existing entity data model (EDM), you can now reuse it directly, instead of having to build a new one.</span></span> <span data-ttu-id="1c109-269">たとえば、Entity Framework を使用している場合は、EDM の EF を構築するを使用できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-269">For example, if you are using Entity Framework, you can use the EDM that EF builds for you.</span></span>

### <a name="request-batching"></a><span data-ttu-id="1c109-270">バッチ処理を要求します。</span><span class="sxs-lookup"><span data-stu-id="1c109-270">Request Batching</span></span>

<span data-ttu-id="1c109-271">ネットワーク トラフィックを削減でき、少ない chatty なユーザー インターフェイス、滑らかに、1 つの HTTP POST 要求に複数の操作を結合する要求がバッチ処理します。</span><span class="sxs-lookup"><span data-stu-id="1c109-271">Request batching combines multiple operations into a single HTTP POST request, to reduce network traffic and provide a smoother, less chatty user interface.</span></span> <span data-ttu-id="1c109-272">ASP.NET Web API では、要求がバッチ処理のいくつかの方法をサポートしているようになりました。</span><span class="sxs-lookup"><span data-stu-id="1c109-272">ASP.NET Web API now supports several strategies for request batching:</span></span>

- <span data-ttu-id="1c109-273">OData サービスの $batch エンドポイントを使用します。</span><span class="sxs-lookup"><span data-stu-id="1c109-273">Use the $batch endpoint of an OData service.</span></span>
- <span data-ttu-id="1c109-274">1 つの MIME マルチパート要求に複数の要求をパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="1c109-274">Package multiple requests into a single MIME multipart request.</span></span>
- <span data-ttu-id="1c109-275">バッチ処理のカスタム書式指定を使用します。</span><span class="sxs-lookup"><span data-stu-id="1c109-275">Use a custom batching format.</span></span>

<span data-ttu-id="1c109-276">バッチ要求を有効にするには、Web API の構成にバッチ処理のハンドラーを持つルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="1c109-276">To enable request batching, simply add a route with a batching handler to your Web API configuration:</span></span>

[!code-csharp[Main](release-notes/samples/sample4.cs)]

<span data-ttu-id="1c109-277">制御することもできるかどうかを要求または順番に、または任意の順序で実行します。</span><span class="sxs-lookup"><span data-stu-id="1c109-277">You can also control whether requests or executed sequentially or in any order.</span></span>

### <a name="portable-aspnet-web-api-client"></a><span data-ttu-id="1c109-278">ポータブル ASP.NET Web API のクライアント</span><span class="sxs-lookup"><span data-stu-id="1c109-278">Portable ASP.NET Web API Client</span></span>

<span data-ttu-id="1c109-279">Windows ストアおよび Windows Phone 8 アプリケーション間で動作するポータブル クラス ライブラリを作成する ASP.NET Web API のクライアントを使えるようになりました。</span><span class="sxs-lookup"><span data-stu-id="1c109-279">You can now use the ASP.NET Web API Client to create portable class libraries that work across your Windows Store and Windows Phone 8 applications.</span></span> <span data-ttu-id="1c109-280">クライアントとサーバー間で共有できるポータブル フォーマッタを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="1c109-280">You can also create portable formatters that can be shared across client and server.</span></span>

### <a name="improved-testability"></a><span data-ttu-id="1c109-281">強化されたテストの容易性</span><span class="sxs-lookup"><span data-stu-id="1c109-281">Improved Testability</span></span>

<span data-ttu-id="1c109-282">Web API 2 は非常に簡単に単体テスト、API コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="1c109-282">Web API 2 makes it much easier to unit test your API controllers.</span></span> <span data-ttu-id="1c109-283">だけで、要求メッセージと構成 API コント ローラーのインスタンスを作成し、テストするアクション メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1c109-283">Just instantiate your API controller with your request message and configuration, and then call the action method you wish to test.</span></span> <span data-ttu-id="1c109-284">よくを模擬表示、 **UrlHelper**のリンクの生成を実行するアクション メソッドのクラスです。</span><span class="sxs-lookup"><span data-stu-id="1c109-284">It is also easy to mock the **UrlHelper** class, for action methods that perform link generation.</span></span>

### <a name="ihttpactionresult"></a><span data-ttu-id="1c109-285">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="1c109-285">IHttpActionResult</span></span>

<span data-ttu-id="1c109-286">今すぐ、Web API のアクション メソッドの結果をカプセル化する IHttpActionResult を実装できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-286">You can now implement IHttpActionResult to encapsulate the result of your Web API action methods.</span></span> <span data-ttu-id="1c109-287">Web API のアクション メソッドから返される、IHttpActionResult は、結果の応答メッセージを生成するために、ASP.NET Web API ランタイムによって実行されます。</span><span class="sxs-lookup"><span data-stu-id="1c109-287">An IHttpActionResult returned from a Web API action method is executed by the ASP.NET Web API runtime to produce the resultant response message.</span></span> <span data-ttu-id="1c109-288">単位を簡単に Web API 操作から返される、IHttpActionResult、Web API の実装をテストします。</span><span class="sxs-lookup"><span data-stu-id="1c109-288">An IHttpActionResult can be returned from any Web API action to simplify unit testing of your Web API implementation.</span></span> <span data-ttu-id="1c109-289">都合のよいとき IHttpActionResult 実装の数が特定の状態コードを返すための結果を含むボックスから提供されたコンテンツまたはコンテンツ ネゴシエート応答形式になります。</span><span class="sxs-lookup"><span data-stu-id="1c109-289">For convenience a number of IHttpActionResult implementations are provided out of the box including results for returning specific status codes, formatted content or content-negotiated responses.</span></span>

### <a name="httprequestcontext"></a><span data-ttu-id="1c109-290">HttpRequestContext</span><span class="sxs-lookup"><span data-stu-id="1c109-290">HttpRequestContext</span></span>

<span data-ttu-id="1c109-291">新しい**HttpRequestContext**要求に関連付けられていますが、すぐに利用できません要求から状態を追跡します。</span><span class="sxs-lookup"><span data-stu-id="1c109-291">The new **HttpRequestContext** tracks any state that is tied to the request but is not immediately available from the request.</span></span> <span data-ttu-id="1c109-292">たとえば、使用することができます、 **HttpRequestContext**ルート データ要求、クライアント証明書に関連付けられているプリンシパルを取得する、 **UrlHelper**と仮想パスのルートです。</span><span class="sxs-lookup"><span data-stu-id="1c109-292">For example, you can use the **HttpRequestContext** to get route data, the principal associated with the request, the client certificate, the **UrlHelper** and the virtual path root.</span></span> <span data-ttu-id="1c109-293">簡単に作成することができます、 **HttpRequestContext**単体テストのためです。</span><span class="sxs-lookup"><span data-stu-id="1c109-293">You can easily create an **HttpRequestContext** for unit testing purposes.</span></span>

<span data-ttu-id="1c109-294">要求のプリンシパルはではなく、要求に応じてフローため**Thread.CurrentPrincipal**プリンシパルが Web API パイプラインにある間、要求の有効期間全体で使用できる、します。</span><span class="sxs-lookup"><span data-stu-id="1c109-294">Because the principal for the request is flowed with the request instead of relying on **Thread.CurrentPrincipal**, the principal is now available throughout the lifetime of the request while it is in the Web API pipeline.</span></span>

### <a name="cors"></a><span data-ttu-id="1c109-295">CORS</span><span class="sxs-lookup"><span data-stu-id="1c109-295">CORS</span></span>

<span data-ttu-id="1c109-296">Brock Allen から別の優れた投稿、感謝 ASP.NET 完全がサポートされましたクロス オリジン要求の共有 (CORS)。</span><span class="sxs-lookup"><span data-stu-id="1c109-296">Thanks to another great contribution from Brock Allen, ASP.NET now fully supports Cross Origin Request Sharing (CORS).</span></span>

<span data-ttu-id="1c109-297">ブラウザーのセキュリティは、Web ページが別のドメインに AJAX 要求を行うことを防止します。</span><span class="sxs-lookup"><span data-stu-id="1c109-297">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="1c109-298">[CORS](http://www.w3.org/TR/cors/) W3C 標準により、同じオリジンのポリシーを緩和するサーバーです。</span><span class="sxs-lookup"><span data-stu-id="1c109-298">[CORS](http://www.w3.org/TR/cors/) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="1c109-299">CORS を使用することによって、不明なリクエストは拒否しながら、一部のクロス オリジン要求のみを明示的に許可できるようになります。</span><span class="sxs-lookup"><span data-stu-id="1c109-299">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span>

<span data-ttu-id="1c109-300">Web API 2 は、プレフライト要求の自動処理を含む、CORS をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="1c109-300">Web API 2 now supports CORS, including automatic handling of preflight requests.</span></span> <span data-ttu-id="1c109-301">詳細については、次を参照してください。 [ASP.NET Web API でのクロス オリジン要求の有効化](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-301">For more information, see [Enabling Cross-Origin Requests in ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).</span></span>

### <a name="authentication-filters"></a><span data-ttu-id="1c109-302">認証フィルター</span><span class="sxs-lookup"><span data-stu-id="1c109-302">Authentication Filters</span></span>

<span data-ttu-id="1c109-303">認証フィルターは、新しい ASP.NET Web API パイプラインでの承認フィルターの前に実行し、認証ロジックごとのアクションを指定することを ASP.NET Web API 内のフィルターの種類ごとのコント ローラー、またはすべてのコント ローラーに対してグローバルにします。</span><span class="sxs-lookup"><span data-stu-id="1c109-303">Authentication filters are a new kind of filter in ASP.NET Web API that run prior to authorization filters in the ASP.NET Web API pipeline and allow you to specify authentication logic per-action, per-controller, or globally for all controllers.</span></span> <span data-ttu-id="1c109-304">認証フィルターは、要求に資格情報を処理し、対応するプリンシパルを提供します。</span><span class="sxs-lookup"><span data-stu-id="1c109-304">Authentication filters process credentials in the request and provide a corresponding principal.</span></span> <span data-ttu-id="1c109-305">認証フィルターは、未承認の要求に応答認証チャレンジを追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="1c109-305">Authentication filters can also add authentication challenges in response to unauthorized requests.</span></span>

### <a name="filter-overrides"></a><span data-ttu-id="1c109-306">上書きをフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="1c109-306">Filter Overrides</span></span>

<span data-ttu-id="1c109-307">オーバーライド フィルターを指定して特定のアクション メソッドまたはコント ローラーに適用するフィルターをオーバーライドすることができますようになりました。</span><span class="sxs-lookup"><span data-stu-id="1c109-307">You can now override which filters apply to a given action method or controller, by specifying an override filter.</span></span> <span data-ttu-id="1c109-308">オーバーライド フィルターは、特定のスコープ (アクションまたはコント ローラー) で実行しないでフィルターの種類のセットを指定します。</span><span class="sxs-lookup"><span data-stu-id="1c109-308">Override filters specify a set of filter types that should not run for a given scope (action or controller).</span></span> <span data-ttu-id="1c109-309">これにより、グローバルのフィルターを追加するが、特定のアクションまたはコント ローラーから一部を除外することができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-309">This allows you to add global filters, but then exclude some from specific actions or controllers.</span></span>

### <a name="owin-integration"></a><span data-ttu-id="1c109-310">OWIN の統合</span><span class="sxs-lookup"><span data-stu-id="1c109-310">OWIN Integration</span></span>

<span data-ttu-id="1c109-311">ASP.NET Web API 今すぐ完全 OWIN をサポート、OWIN 対応のホストで実行することができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-311">ASP.NET Web API now fully supports OWIN and can be run on any OWIN capable host.</span></span> <span data-ttu-id="1c109-312">含まれています、 **HostAuthenticationFilter** OWIN 認証システムとの統合を提供します。</span><span class="sxs-lookup"><span data-stu-id="1c109-312">Also included is a **HostAuthenticationFilter** that provides integration with the OWIN authentication system.</span></span>

<span data-ttu-id="1c109-313">OWIN の統合により、自己 SignalR など他の OWIN ミドルウェアと共にプロセスを独自の Web API をホストすることができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-313">With OWIN integration, you can self-host Web API in your own process alongside other OWIN middleware, such as SignalR.</span></span> <span data-ttu-id="1c109-314">詳細については、次を参照してください。 [Self-Host ASP.NET Web API を使用する OWIN](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-314">For more information, see [Use OWIN to Self-Host ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a><span data-ttu-id="1c109-315">ASP.NET SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="1c109-315">ASP.NET SignalR 2.0</span></span>

<span data-ttu-id="1c109-316">次のセクションでは、SignalR 2.0 の機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="1c109-316">The following sections describe features of SignalR 2.0.</span></span>

- [<span data-ttu-id="1c109-317">OWIN 上に構築されました。</span><span class="sxs-lookup"><span data-stu-id="1c109-317">Built on OWIN</span></span>](#builtonowin)
- [<span data-ttu-id="1c109-318">MapHubs と MapConnection、MapSignalR</span><span class="sxs-lookup"><span data-stu-id="1c109-318">MapHubs and MapConnection are now MapSignalR</span></span>](#MapSignalR)
- [<span data-ttu-id="1c109-319">クロス ドメインのサポート</span><span class="sxs-lookup"><span data-stu-id="1c109-319">Cross-Domain Support</span></span>](#crossdomain)
- [<span data-ttu-id="1c109-320">iOS および Android MonoTouch および MonoDroid によるサポートします。</span><span class="sxs-lookup"><span data-stu-id="1c109-320">iOS and Android support via MonoTouch and MonoDroid</span></span>](#mobile)
- [<span data-ttu-id="1c109-321">ポータブル .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="1c109-321">Portable .NET Client</span></span>](#portable)
- [<span data-ttu-id="1c109-322">自己ホストの新しいパッケージ</span><span class="sxs-lookup"><span data-stu-id="1c109-322">New Self-Host Package</span></span>](#selfhost)
- [<span data-ttu-id="1c109-323">旧バージョンと互換性のあるサーバーのサポート</span><span class="sxs-lookup"><span data-stu-id="1c109-323">Backward-compatible server support</span></span>](#backwardcompat)
- [<span data-ttu-id="1c109-324">.NET 4.0 をサーバー サポートを削除</span><span class="sxs-lookup"><span data-stu-id="1c109-324">Removed server support for .NET 4.0</span></span>](#remove40)
- [<span data-ttu-id="1c109-325">クライアントとグループの一覧にメッセージを送信</span><span class="sxs-lookup"><span data-stu-id="1c109-325">Sending a message to a list of clients and groups</span></span>](#messagelist)
- [<span data-ttu-id="1c109-326">特定のユーザーにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="1c109-326">Sending a message to a specific user</span></span>](#sendtouser)
- [<span data-ttu-id="1c109-327">エラー処理サポートの向上</span><span class="sxs-lookup"><span data-stu-id="1c109-327">Better error handling support</span></span>](#errorhandling)
- [<span data-ttu-id="1c109-328">簡単に単体テストのハブ</span><span class="sxs-lookup"><span data-stu-id="1c109-328">Easier unit testing of hubs</span></span>](#unittesting)
- [<span data-ttu-id="1c109-329">JavaScript のエラー処理</span><span class="sxs-lookup"><span data-stu-id="1c109-329">JavaScript error handling</span></span>](#javascripterror)

<span data-ttu-id="1c109-330">SignalR 2.0 を 1.x の既存のプロジェクトをアップグレードする方法の例は、次を参照してください。 [、SignalR のアップグレード 1.x プロジェクト](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-330">For an example of how to upgrade an existing 1.x project to SignalR 2.0, see [Upgrading a SignalR 1.x Project](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<a id="builtonowin"></a>
### <a name="built-on-owin"></a><span data-ttu-id="1c109-331">OWIN 上に構築されました。</span><span class="sxs-lookup"><span data-stu-id="1c109-331">Built on OWIN</span></span>

<span data-ttu-id="1c109-332">SignalR 2.0 に完全に構築された[OWIN (.NET の Open Web Interface)](http://owin.org/)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-332">SignalR 2.0 is built completely on [OWIN (the Open Web Interface for .NET)](http://owin.org/).</span></span> <span data-ttu-id="1c109-333">この変更と、セットアップ プロセス SignalR web ホストおよび自己ホスト型の SignalR アプリケーション間でより一貫性が、API の変更の数も必要です。</span><span class="sxs-lookup"><span data-stu-id="1c109-333">This change makes the setup process for SignalR much more consistent between web-hosted and self-hosted SignalR applications, but has also required a number of API changes.</span></span>

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a><span data-ttu-id="1c109-334">MapHubs と MapConnection、MapSignalR</span><span class="sxs-lookup"><span data-stu-id="1c109-334">MapHubs and MapConnection are now MapSignalR</span></span>

<span data-ttu-id="1c109-335">OWIN 標準と互換性のため、これらのメソッドが名前に変更されました`MapSignalR`です。</span><span class="sxs-lookup"><span data-stu-id="1c109-335">For compatibility with OWIN standards, these methods have been renamed to `MapSignalR`.</span></span> <span data-ttu-id="1c109-336">`MapSignalR` パラメーターは、すべてのハブをマップすることがなく呼び出されます (として`MapHubs`はバージョン 1.x) 以外の場合は個別にマップする**PersistentConnection**オブジェクト、型パラメーターと、URL の拡張機能としての接続として接続の種類を指定する、最初の引数。</span><span class="sxs-lookup"><span data-stu-id="1c109-336">`MapSignalR` called without parameters will map all hubs (as `MapHubs` does in version 1.x); to map individual **PersistentConnection** objects, specify the connection type as the type parameter, and the URL extension for the connection as the first argument.</span></span>

<span data-ttu-id="1c109-337">`MapSignalR` Owin スタートアップ クラスでメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="1c109-337">The `MapSignalR` method is called in an Owin startup class.</span></span> <span data-ttu-id="1c109-338">Visual Studio 2013 には、Owin スタートアップ クラスの新しいテンプレートが含まれていますこのテンプレートを使用するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="1c109-338">Visual Studio 2013 contains a new template for an Owin startup class; to use this template, do the following:</span></span>

1. <span data-ttu-id="1c109-339">プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="1c109-339">Right-click on the project</span></span>
2. <span data-ttu-id="1c109-340">選択**追加**、**新しい項目の追加.**</span><span class="sxs-lookup"><span data-stu-id="1c109-340">Select **Add**, **New Item...**</span></span>
3. <span data-ttu-id="1c109-341">選択**Owin スタートアップ クラス**です。</span><span class="sxs-lookup"><span data-stu-id="1c109-341">Select **Owin Startup class**.</span></span> <span data-ttu-id="1c109-342">新しいクラスの名前を**Startup.cs**です。</span><span class="sxs-lookup"><span data-stu-id="1c109-342">Name the new class **Startup.cs**.</span></span>

<span data-ttu-id="1c109-343">**Web アプリケーション、** Owin スタートアップ クラスを含む、`MapSignalR`次のように、メソッドは Owin の起動プロセスのエントリを使用して、Web.Config ファイルのアプリケーション設定 ノード内に追加されます。</span><span class="sxs-lookup"><span data-stu-id="1c109-343">In a **Web application,** the Owin startup class containing the `MapSignalR` method is then added to Owin's startup process using an entry in the application settings node of the Web.Config file, as shown below.</span></span>

<span data-ttu-id="1c109-344">**セルフホスト アプリケーション**、スタートアップ クラスは、の型パラメーターとして渡される、`WebApp.Start`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="1c109-344">In a **Self-hosted application**, the Startup class is passed as the type parameter of the `WebApp.Start` method.</span></span>

<span data-ttu-id="1c109-345">**ハブとの SignalR 接続とのマッピング (web アプリケーションのグローバルなアプリケーション ファイル) から 1.x:**</span><span class="sxs-lookup"><span data-stu-id="1c109-345">**Mapping hubs and connections in SignalR 1.x (from the global application file in a web application):**</span></span> 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

<span data-ttu-id="1c109-346">**ハブおよび SignalR 2.0 (Owin 起動クラス ファイルの場合) からの接続のマッピング。**</span><span class="sxs-lookup"><span data-stu-id="1c109-346">**Mapping hubs and connections in SignalR 2.0 (from an Owin Startup class file):**</span></span> 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

<span data-ttu-id="1c109-347">**セルフホスト アプリケーション**、スタートアップ クラスは、の型パラメーターとして渡される、`WebApp.Start`メソッドを次のようにします。</span><span class="sxs-lookup"><span data-stu-id="1c109-347">In a **Self-hosted application**, the Startup class is passed as the type parameter for the `WebApp.Start` method, as shown below.</span></span>

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a><span data-ttu-id="1c109-348">クロス ドメインのサポート</span><span class="sxs-lookup"><span data-stu-id="1c109-348">Cross-Domain Support</span></span>

<span data-ttu-id="1c109-349">SignalR で 1.x では、クロス ドメイン要求が単一 EnableCrossDomain フラグによって制御されます。</span><span class="sxs-lookup"><span data-stu-id="1c109-349">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="1c109-350">このフラグは、両方の JSONP および CORS 要求を制御します。</span><span class="sxs-lookup"><span data-stu-id="1c109-350">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="1c109-351">すべての CORS のサポートをより柔軟に行う、SignalR のサーバー コンポーネントから削除されました (JavaScript lients まだ CORS 通常、ブラウザーがサポートすることが検出された場合) を使用し、新しい OWIN ミドルウェアをこれらのシナリオをサポートするために使用されました。</span><span class="sxs-lookup"><span data-stu-id="1c109-351">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript lients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="1c109-352">SignalR 2.0 では JSONP の場合は、クライアントで (古いブラウザーでは、クロス ドメイン要求をサポート) で必要設定を明示的に有効にする必要があります`EnableJSONP`上、`HubConfiguration`オブジェクトを`true`、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="1c109-352">In SignalR 2.0, If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="1c109-353">CORS より安全であるために、既定では、JSONP が無効です。</span><span class="sxs-lookup"><span data-stu-id="1c109-353">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="1c109-354">SignalR 2.0 では、新しい CORS ミドルウェアを追加するには、追加、`Microsoft.Owin.Cors`ライブラリ プロジェクト、および呼び出しを`UseCors`SignalR ミドルウェアは、以下のセクションで示すようにする前にします。</span><span class="sxs-lookup"><span data-stu-id="1c109-354">To add the new CORS middleware in SignalR 2.0, add the `Microsoft.Owin.Cors` library to your project, and call `UseCors` before your SignalR middleware, as shown in the section below.</span></span>

<span data-ttu-id="1c109-355">**Microsoft.Owin.Cors をプロジェクトに追加する**: このライブラリをインストールするには、パッケージ マネージャー コンソールで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1c109-355">**Adding Microsoft.Owin.Cors to your project**: To install this library, run the following command in the Package Manager Console:</span></span>

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

<span data-ttu-id="1c109-356">このコマンドは、2.0.0 を追加、プロジェクトにパッケージのバージョン。</span><span class="sxs-lookup"><span data-stu-id="1c109-356">This command will add the 2.0.0 version of the package to your project.</span></span>

<span data-ttu-id="1c109-357">**通話 UseCors**</span><span class="sxs-lookup"><span data-stu-id="1c109-357">**Calling UseCors**</span></span>

<span data-ttu-id="1c109-358">次のコード スニペットは、SignalR にクロス ドメインの接続を実装する方法を示す 1.x および 2.0。</span><span class="sxs-lookup"><span data-stu-id="1c109-358">The following code snippets demonstrate how to implement cross-domain connections in SignalR 1.x and 2.0.</span></span>

<span data-ttu-id="1c109-359">**SignalR でクロス ドメイン要求を実装する (グローバルなアプリケーション ファイル) から 1.x**</span><span class="sxs-lookup"><span data-stu-id="1c109-359">**Implementing cross-domain requests in SignalR 1.x (from the global application file)**</span></span>

[!code-csharp[Main](release-notes/samples/sample9.cs)]

<span data-ttu-id="1c109-360">**SignalR 2.0 (c# コード ファイル) からのクロス ドメイン要求を実装します。**</span><span class="sxs-lookup"><span data-stu-id="1c109-360">**Implementing cross-domain requests in SignalR 2.0 (from a C# code file)**</span></span>

<span data-ttu-id="1c109-361">次のコードでは、SignalR 2.0 プロジェクトで CORS または JSONP を有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1c109-361">The following code demonstrates how to enable CORS or JSONP in a SignalR 2.0 project.</span></span> <span data-ttu-id="1c109-362">このコード サンプルでは使用`Map`と`RunSignalR`の代わりに`MapSignalR`CORS のサポートを必要とする SignalR 要求に対してのみ、CORS ミドルウェアが実行されるように、(で指定されたパスにすべてのトラフィックではなく`MapSignalR`)。`Map`アプリケーション全体に対してではなく、特定の URL プレフィックスを実行する必要があるその他のすべてのミドルウェアのも使用できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-362">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) `Map` can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a><span data-ttu-id="1c109-363">iOS および Android MonoTouch および MonoDroid によるサポートします。</span><span class="sxs-lookup"><span data-stu-id="1c109-363">iOS and Android support via MonoTouch and MonoDroid</span></span>

<span data-ttu-id="1c109-364">IOS および Android のクライアントから MonoTouch と MonoDroid のコンポーネントを使用してサポートが追加されて、 [Xamarin ライブラリ](https://xamarin.com/)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-364">Support has been added for iOS and Android clients using MonoTouch and MonoDroid components from the [Xamarin library](https://xamarin.com/).</span></span> <span data-ttu-id="1c109-365">それらの使用方法の詳細については、次を参照してください。[を使用して Xamarin コンポーネント](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-365">For more information on how to use them, see [Using Xamarin Components](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln).</span></span> <span data-ttu-id="1c109-366">これらのコンポーネントがで使用できる、 [Xamarin ストア](https://store.xamarin.com/)SignalR RTW リリースが使用可能な場合です。</span><span class="sxs-lookup"><span data-stu-id="1c109-366">These components will be available in the [Xamarin Store](https://store.xamarin.com/) when the SignalR RTW release is available.</span></span>

<a id="portable"></a> <span data-ttu-id="1c109-367">### ポータブル .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="1c109-367">### Portable .NET client</span></span>

<span data-ttu-id="1c109-368">効率的に Silverlight を WinRT、クロスプラット フォーム開発を容易にし、Windows Phone クライアントは、次のプラットフォームをサポートする 1 つのポータブル .NET クライアントに置き換えられています。</span><span class="sxs-lookup"><span data-stu-id="1c109-368">To better facilitate cross-platform development, the Silverlight, WinRT and Windows Phone clients have been replaced with a single portable .NET client that supports the following platforms:</span></span>

- <span data-ttu-id="1c109-369">NET 4.5</span><span class="sxs-lookup"><span data-stu-id="1c109-369">NET 4.5</span></span>
- <span data-ttu-id="1c109-370">Silverlight 5</span><span class="sxs-lookup"><span data-stu-id="1c109-370">Silverlight 5</span></span>
- <span data-ttu-id="1c109-371">WinRT (Windows ストア アプリ用 .NET)</span><span class="sxs-lookup"><span data-stu-id="1c109-371">WinRT (.NET for Windows Store Apps)</span></span>
- <span data-ttu-id="1c109-372">Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="1c109-372">Windows Phone 8</span></span>

<a id="selfhost"></a>

### <a name="new-self-host-package"></a><span data-ttu-id="1c109-373">自己ホストの新しいパッケージ</span><span class="sxs-lookup"><span data-stu-id="1c109-373">New Self-Host Package</span></span>

<span data-ttu-id="1c109-374">SignalR (プロセスでホストされている SignalR アプリケーションまたはその他のアプリケーションではなく、web サーバーでホストされている) が自己ホストを開始するが容易に NuGet パッケージはようになりました。</span><span class="sxs-lookup"><span data-stu-id="1c109-374">There is now a NuGet package to make it easier to get started with SignalR Self-Host (SignalR applications that are hosted in a process or other application, rather than being hosted in a web server).</span></span> <span data-ttu-id="1c109-375">SignalR でビルドされた自己ホスト プロジェクトをアップグレードする 1.x では、Microsoft.AspNet.SignalR.Owin パッケージを削除して、Microsoft.AspNet.SignalR.SelfHost パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="1c109-375">To upgrade a self-host project built with SignalR 1.x, remove the Microsoft.AspNet.SignalR.Owin package, and add the Microsoft.AspNet.SignalR.SelfHost package.</span></span> <span data-ttu-id="1c109-376">自己ホスト パッケージの概要の詳細については、次を参照してください。[チュートリアル: 自己ホスト SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-376">For more information on getting started with the self-host package, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a><span data-ttu-id="1c109-377">旧バージョンと互換性のあるサーバーのサポート</span><span class="sxs-lookup"><span data-stu-id="1c109-377">Backward-compatible server support</span></span>

<span data-ttu-id="1c109-378">以前のバージョンの SignalR、SignalR パッケージをクライアントで使用されると、一致させるために必要なサーバーのバージョン。</span><span class="sxs-lookup"><span data-stu-id="1c109-378">In previous versions of SignalR, the versions of the SignalR package used in the client and the server needed to be identical.</span></span> <span data-ttu-id="1c109-379">更新するが困難になるシック クライアント アプリケーションをサポートするために SignalR 2.0 サポート古いクライアントで新しいサーバー バージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="1c109-379">In order to support thick-client applications that would be difficult to update, SignalR 2.0 now supports using a newer server version with an older client.</span></span> <span data-ttu-id="1c109-380">**注: SignalR 2.0 では比較的新しいクライアントで以前のバージョンでビルドされたサーバーはサポートされません。**</span><span class="sxs-lookup"><span data-stu-id="1c109-380">**Note: SignalR 2.0 does not support servers built with older versions with newer clients.**</span></span>

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a><span data-ttu-id="1c109-381">.NET 4.0 をサーバー サポートを削除</span><span class="sxs-lookup"><span data-stu-id="1c109-381">Removed server support for .NET 4.0</span></span>

<span data-ttu-id="1c109-382">SignalR 2.0 では、.NET 4.0 でのサーバーの相互運用性のサポートを削除します。</span><span class="sxs-lookup"><span data-stu-id="1c109-382">SignalR 2.0 has dropped support for server interoperability with .NET 4.0.</span></span> <span data-ttu-id="1c109-383">SignalR 2.0 サーバーでは、.NET 4.5 を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c109-383">.NET 4.5 must be used with SignalR 2.0 servers.</span></span> <span data-ttu-id="1c109-384">SignalR 2.0 用の .NET 4.0 クライアントは引き続きです。</span><span class="sxs-lookup"><span data-stu-id="1c109-384">There is still a .NET 4.0 client for SignalR 2.0.</span></span>

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a><span data-ttu-id="1c109-385">クライアントとグループの一覧にメッセージを送信</span><span class="sxs-lookup"><span data-stu-id="1c109-385">Sending a message to a list of clients and groups</span></span>

<span data-ttu-id="1c109-386">SignalR 2.0 では、クライアントとグループ Id の一覧を使用してメッセージを送信することはできます。</span><span class="sxs-lookup"><span data-stu-id="1c109-386">In SignalR 2.0, it's possible to send a message using a list of client and group IDs.</span></span> <span data-ttu-id="1c109-387">次のコード スニペットでは、これを行う方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1c109-387">The following code snippets demonstrate how to do this.</span></span>

<span data-ttu-id="1c109-388">**クライアントと PersistentConnection を使用して、グループの一覧にメッセージを送信**</span><span class="sxs-lookup"><span data-stu-id="1c109-388">**Sending a message to a list of clients and groups using PersistentConnection**</span></span>

[!code-csharp[Main](release-notes/samples/sample11.cs)]

<span data-ttu-id="1c109-389">**クライアントとハブを使用してグループの一覧にメッセージを送信**</span><span class="sxs-lookup"><span data-stu-id="1c109-389">**Sending a message to a list of clients and groups using Hubs**</span></span>

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a><span data-ttu-id="1c109-390">特定のユーザーにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="1c109-390">Sending a message to a specific user</span></span>

<span data-ttu-id="1c109-391">この機能により、ユーザー、ユーザー Id を指定する新しいインターフェイス IUserIdProvider を介して、IRequest に基づいて。</span><span class="sxs-lookup"><span data-stu-id="1c109-391">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider:</span></span>

<span data-ttu-id="1c109-392">**IUserIdProvider インターフェイス**</span><span class="sxs-lookup"><span data-stu-id="1c109-392">**The IUserIdProvider interface**</span></span>

[!code-csharp[Main](release-notes/samples/sample13.cs)]

<span data-ttu-id="1c109-393">既定では、ユーザー名とユーザーの IPrincipal.Identity.Name を使用する実装があります。</span><span class="sxs-lookup"><span data-stu-id="1c109-393">By default there will be an implementation that uses the user's IPrincipal.Identity.Name as the user name.</span></span>

<span data-ttu-id="1c109-394">ハブで、新しい API を使用してこれらのユーザーにメッセージを送信することができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-394">In hubs, you'll be able to send messages to these users via a new API:</span></span>

<span data-ttu-id="1c109-395">**Clients.User API を使用します。**</span><span class="sxs-lookup"><span data-stu-id="1c109-395">**Using the Clients.User API**</span></span>

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a><span data-ttu-id="1c109-396">エラー処理サポートの向上</span><span class="sxs-lookup"><span data-stu-id="1c109-396">Better Error Handling Support</span></span>

<span data-ttu-id="1c109-397">ユーザーがスローできるようになりました**HubException**任意ハブ呼び出しからです。</span><span class="sxs-lookup"><span data-stu-id="1c109-397">Users can now throw **HubException** from any hub invocation.</span></span> <span data-ttu-id="1c109-398">コンス トラクター、 **HubException**文字列メッセージおよびオブジェクトを受け取ってできる追加のエラー データ。</span><span class="sxs-lookup"><span data-stu-id="1c109-398">The constructor of the **HubException** can take a string message and an object extra error data.</span></span> <span data-ttu-id="1c109-399">SignalR では、例外を自動でシリアル化し、拒否/失敗のハブ メソッド呼び出しに使用される場所をクライアントに送信します。</span><span class="sxs-lookup"><span data-stu-id="1c109-399">SignalR will auto-serialize the exception and send it to the client where it will be used to reject/fail the hub method invocation.</span></span>

<span data-ttu-id="1c109-400">**詳細なハブの例外を表示する**設定に影響していない**HubException**ことはありません。 または、クライアントに送信されることが常に送信します。</span><span class="sxs-lookup"><span data-stu-id="1c109-400">The **show detailed hub exceptions** setting has no bearing on **HubException** being sent back to the client or not; it is always sent.</span></span>

<span data-ttu-id="1c109-401">**サーバー側コード、HubException をクライアントに送信中のデモンストレーション**</span><span class="sxs-lookup"><span data-stu-id="1c109-401">**Server-side code demonstrating sending a HubException to the client**</span></span>

[!code-csharp[Main](release-notes/samples/sample15.cs)]

<span data-ttu-id="1c109-402">**JavaScript クライアント コードが、サーバーから送信された HubException への応答のデモンストレーション**</span><span class="sxs-lookup"><span data-stu-id="1c109-402">**JavaScript client code demonstrating responding to a HubException sent from the server**</span></span>

[!code-html[Main](release-notes/samples/sample16.html)]

<span data-ttu-id="1c109-403">**.NET クライアントをサーバーから送信された HubException への応答を示すコード**</span><span class="sxs-lookup"><span data-stu-id="1c109-403">**.NET client code demonstrating responding to a HubException sent from the server**</span></span>

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a><span data-ttu-id="1c109-404">簡単に単体テストのハブ</span><span class="sxs-lookup"><span data-stu-id="1c109-404">Easier unit testing of hubs</span></span>

<span data-ttu-id="1c109-405">SignalR 2.0 には、というインターフェイスが含まれています。`IHubCallerConnectionContext`モック クライアント側の呼び出しを作成するのが容易ハブにします。</span><span class="sxs-lookup"><span data-stu-id="1c109-405">SignalR 2.0 includes an interface called `IHubCallerConnectionContext` on Hubs that makes it easier to create mock client side invocations.</span></span> <span data-ttu-id="1c109-406">次のコード スニペットは、このインターフェイスを使用して一般的なテスト ハーネスを示す[xUnit.net](https://github.com/xunit/xunit)と[moq](https://code.google.com/p/moq/)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-406">The following code snippets demonstrate using this interface with popular test harnesses [xUnit.net](https://github.com/xunit/xunit) and [moq](https://code.google.com/p/moq/).</span></span>

<span data-ttu-id="1c109-407">**使用した単体テスト SignalR xUnit.net**</span><span class="sxs-lookup"><span data-stu-id="1c109-407">**Unit testing SignalR with xUnit.net**</span></span>

[!code-csharp[Main](release-notes/samples/sample18.cs)]

<span data-ttu-id="1c109-408">**使用した単体テスト SignalR moq**</span><span class="sxs-lookup"><span data-stu-id="1c109-408">**Unit testing SignalR with moq**</span></span>

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a><span data-ttu-id="1c109-409">JavaScript のエラー処理</span><span class="sxs-lookup"><span data-stu-id="1c109-409">JavaScript error handling</span></span>

<span data-ttu-id="1c109-410">SignalR 2.0 では、すべての JavaScript エラー処理のコールバックは、未加工の文字列ではなく JavaScript エラー オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="1c109-410">In SignalR 2.0, all JavaScript error handling callbacks return JavaScript error objects instead of raw strings.</span></span> <span data-ttu-id="1c109-411">これによりより詳細な情報、エラー ハンドラーにフローする SignalR できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-411">This allows SignalR to flow richer information to your error handlers.</span></span> <span data-ttu-id="1c109-412">内部例外を取得することができます、`source`エラーのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="1c109-412">You can get the inner exception from the `source` property of the error.</span></span>

<span data-ttu-id="1c109-413">**Start.Fail 例外を処理する JavaScript クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="1c109-413">**JavaScript client code that handles the Start.Fail exception**</span></span>

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a><span data-ttu-id="1c109-414">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="1c109-414">ASP.NET Identity</span></span>

### <a name="new-aspnet-membership-system"></a><span data-ttu-id="1c109-415">新規の ASP.NET メンバーシップ システム</span><span class="sxs-lookup"><span data-stu-id="1c109-415">New ASP.NET Membership System</span></span>

<span data-ttu-id="1c109-416">ASP.NET Identity は、ASP.NET アプリケーションの新しいメンバーシップ システムです。</span><span class="sxs-lookup"><span data-stu-id="1c109-416">ASP.NET Identity is the new membership system for ASP.NET applications.</span></span> <span data-ttu-id="1c109-417">ASP.NET Identity しやすいアプリケーションのデータとユーザー固有のプロファイル データを統合します。</span><span class="sxs-lookup"><span data-stu-id="1c109-417">ASP.NET Identity makes it easy to integrate user-specific profile data with application data.</span></span> <span data-ttu-id="1c109-418">ASP.NET Id では、アプリケーションでユーザー プロファイルの永続化モデルを選択することもできます。</span><span class="sxs-lookup"><span data-stu-id="1c109-418">ASP.NET Identity also allows you to choose the persistence model for user profiles in your application.</span></span> <span data-ttu-id="1c109-419">SQL Server データベースまたは Azure ストレージ テーブルなどの NoSQL データ ストアを含む別のデータ ストアでデータを保存することができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-419">You can store the data in a SQL Server database or another data store, including NoSQL data stores such as Azure Storage Tables.</span></span> <span data-ttu-id="1c109-420">詳細については、次を参照してください。[個々 のユーザー アカウント](creating-web-projects-in-visual-studio.md#indauth)で**Visual Studio 2013 で ASP.NET Web プロジェクトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="1c109-420">For more information, see [Individual User Accounts](creating-web-projects-in-visual-studio.md#indauth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="claims-based-authentication"></a><span data-ttu-id="1c109-421">クレーム ベースの認証</span><span class="sxs-lookup"><span data-stu-id="1c109-421">Claims-based authentication</span></span>

<span data-ttu-id="1c109-422">ASP.NET は、ユーザーの id が、信頼できる発行者からのクレームのセットとして表されるクレーム ベース認証をサポートします。</span><span class="sxs-lookup"><span data-stu-id="1c109-422">ASP.NET now supports claims-based authentication, where the user's identity is represented as a set of claims from a trusted issuer.</span></span> <span data-ttu-id="1c109-423">ユーザーが認証されたユーザー名とパスワード、アプリケーション データベースで保守管理を使用してまたはのソーシャル id プロバイダーを使用して指定できます (例: Microsoft アカウント、Facebook、Google、Twitter)、または Azure Active Directory を介して組織アカウントを使用して、またはActive Directory フェデレーション サービス (ADFS)。</span><span class="sxs-lookup"><span data-stu-id="1c109-423">Users can be authenticated using a username and password maintained in an application database, or using social identity providers (for example: Microsoft Accounts, Facebook, Google, Twitter), or using organizational accounts through Azure Active Directory or Active Directory Federation Services (ADFS).</span></span>

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a><span data-ttu-id="1c109-424">Azure Active Directory および Windows Server Active Directory との統合</span><span class="sxs-lookup"><span data-stu-id="1c109-424">Integration with Azure Active Directory and Windows Server Active Directory</span></span>

<span data-ttu-id="1c109-425">認証を Azure Active Directory または Windows Server ・ Active Directory (AD) を使用する ASP.NET プロジェクトを作成できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-425">You can now create ASP.NET projects that use Azure Active Directory or Windows Server Active Directory (AD) for authentication.</span></span> <span data-ttu-id="1c109-426">詳細については、次を参照してください。[組織アカウント](creating-web-projects-in-visual-studio.md#orgauth)で**Visual Studio 2013 で ASP.NET Web プロジェクトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="1c109-426">For more information, see [Organizational Accounts](creating-web-projects-in-visual-studio.md#orgauth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="owin-integration"></a><span data-ttu-id="1c109-427">OWIN の統合</span><span class="sxs-lookup"><span data-stu-id="1c109-427">OWIN Integration</span></span>

<span data-ttu-id="1c109-428">ASP.NET 認証は、任意の OWIN ベースのホストで使用できますの OWIN ミドルウェアに基づいています。</span><span class="sxs-lookup"><span data-stu-id="1c109-428">ASP.NET authentication is now based on OWIN middleware that can be used on any OWIN-based host.</span></span> <span data-ttu-id="1c109-429">OWIN の詳細については、次を参照してください。 [Microsoft OWIN コンポーネント](#TOC7)セクションです。</span><span class="sxs-lookup"><span data-stu-id="1c109-429">For more information about OWIN, see the following [Microsoft OWIN Components](#TOC7) section.</span></span>

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a><span data-ttu-id="1c109-430">Microsoft OWIN コンポーネント</span><span class="sxs-lookup"><span data-stu-id="1c109-430">Microsoft OWIN Components</span></span>

<span data-ttu-id="1c109-431">[.NET 用 Web インターフェイスを開く](http://owin.org/)(OWIN) .NET web サーバーと web アプリケーション間で抽象型を定義します。</span><span class="sxs-lookup"><span data-stu-id="1c109-431">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="1c109-432">OWIN では、web アプリケーション ホストにとらわれないを行う、サーバーから web アプリケーションを分離します。</span><span class="sxs-lookup"><span data-stu-id="1c109-432">OWIN decouples the web application from the server, making web applications host-agnostic.</span></span> <span data-ttu-id="1c109-433">たとえば、IIS で OWIN ベースの web アプリケーションをホストしたり、カスタム プロセス内で自己ホストできます。</span><span class="sxs-lookup"><span data-stu-id="1c109-433">For example, you can host an OWIN-based web application in IIS or self-host it in a custom process.</span></span>

<span data-ttu-id="1c109-434">Microsoft OWIN コンポーネント (Katana プロジェクトとも呼ばれます) で導入された変更には、新しいサーバーとホストのコンポーネント、新しいヘルパー ライブラリとミドルウェア、および新しい認証ミドルウェアが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1c109-434">Changes introduced in the Microsoft OWIN components (also known as the Katana project) include new server and host components, new helper libraries and middleware, and new authentication middleware.</span></span>

<span data-ttu-id="1c109-435">OWIN および Katana の詳細については、次を参照してください。 [OWIN および Katana 新](../../../aspnet/overview/owin-and-katana/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-435">For more information about OWIN and Katana, see [What's new in OWIN and Katana](../../../aspnet/overview/owin-and-katana/index.md).</span></span>

<span data-ttu-id="1c109-436">**注: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)アプリケーションは、IIS クラシック モードで実行できません。 統合モードで実行する必要があります。**</span><span class="sxs-lookup"><span data-stu-id="1c109-436">**Note: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) applications cannot run in IIS classic mode; they must be run in integrated mode.**</span></span>

<span data-ttu-id="1c109-437">**注: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)完全信頼でアプリケーションを実行する必要があります。**</span><span class="sxs-lookup"><span data-stu-id="1c109-437">**Note: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) applications must be run in full trust.**</span></span>

### <a name="new-servers-and-hosts"></a><span data-ttu-id="1c109-438">新しいサーバーとホスト</span><span class="sxs-lookup"><span data-stu-id="1c109-438">New Servers and Hosts</span></span>

<span data-ttu-id="1c109-439">このリリースでは、新しいコンポーネントは、自己ホストのシナリオを有効にする追加されました。</span><span class="sxs-lookup"><span data-stu-id="1c109-439">With this release, new components were added to enable self-host scenarios.</span></span> <span data-ttu-id="1c109-440">これらのコンポーネントには、次の NuGet パッケージが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1c109-440">These components include the following NuGet packages:</span></span>

- <span data-ttu-id="1c109-441">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="1c109-441">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="1c109-442">使用する OWIN サーバーは、 **HttpListener**を HTTP 要求をリッスンし、OWIN パイプラインに指示します。</span><span class="sxs-lookup"><span data-stu-id="1c109-442">Provides an OWIN server that uses **HttpListener** to listen for HTTP requests and direct them into the OWIN pipeline.</span></span>
- <span data-ttu-id="1c109-443">**Microsoft.Owin.Hosting**自己コンソール アプリケーションまたは Windows サービスなど、カスタムのプロセスで OWIN パイプラインをホストしたい開発者向けのライブラリを提供します。</span><span class="sxs-lookup"><span data-stu-id="1c109-443">**Microsoft.Owin.Hosting** Provides a library for developers who wish to self-host an OWIN pipeline in a custom process, such as a console application or Windows service.</span></span>
- <span data-ttu-id="1c109-444">**OwinHost**です。</span><span class="sxs-lookup"><span data-stu-id="1c109-444">**OwinHost**.</span></span> <span data-ttu-id="1c109-445">ラップするスタンドアロンの実行可能ファイルを提供`Microsoft.Owin.Hosting`自己をカスタム ホスト アプリケーションを記述しなくても、OWIN パイプラインをホストすることができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-445">Provides a stand-alone executable that wraps `Microsoft.Owin.Hosting` and lets you self-host an OWIN pipeline without having to write a custom host application.</span></span>

<span data-ttu-id="1c109-446">さらに、`Microsoft.Owin.Host.SystemWeb`パッケージにヒントを指定してミドルウェアを今すぐ有効に、 **SystemWeb**サーバー、特定の ASP.NET パイプライン ステージ中に、ミドルウェアを呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c109-446">In addition, the `Microsoft.Owin.Host.SystemWeb` package now enables middleware to provide hints to the **SystemWeb** server, indicating that the middleware should be called during a specific ASP.NET pipeline stage.</span></span> <span data-ttu-id="1c109-447">この機能は、認証ミドルウェアは、早い段階での ASP.NET パイプラインを実行する必要がありますに特に便利です。</span><span class="sxs-lookup"><span data-stu-id="1c109-447">This feature is particularly useful for authentication middleware, which should run early in the ASP.NET pipeline.</span></span>

### <a name="helper-libraries-and-middleware"></a><span data-ttu-id="1c109-448">ヘルパー ライブラリとミドルウェア</span><span class="sxs-lookup"><span data-stu-id="1c109-448">Helper Libraries and Middleware</span></span>

<span data-ttu-id="1c109-449">OWIN 仕様からの機能と種類の定義のみを使用する OWIN コンポーネントを記述することができますが、新しい`Microsoft.Owin`パッケージは、抽象化のわかりやすいセットを提供します。</span><span class="sxs-lookup"><span data-stu-id="1c109-449">Although you can write OWIN components using only the function and type definitions from the OWIN specification, the new `Microsoft.Owin` package provides a more user-friendly set of abstractions.</span></span> <span data-ttu-id="1c109-450">このパッケージは以前のいくつかのパッケージを組み合わせたもの (例: `Owin.Extensions`、 `Owin.Types`) し、簡単に使用できる他の OWIN コンポーネントで適切に構成された、1 つのオブジェクト モデルをします。</span><span class="sxs-lookup"><span data-stu-id="1c109-450">This package combines several earlier packages (e.g., `Owin.Extensions`, `Owin.Types`) into a single, well-structured object model that can then be easily used by other OWIN components.</span></span> <span data-ttu-id="1c109-451">実際には、Microsoft OWIN コンポーネントの大部分は、このパッケージを使用するようになりました。</span><span class="sxs-lookup"><span data-stu-id="1c109-451">In fact, the majority of Microsoft OWIN components now use this package.</span></span>

> [!NOTE]
> <span data-ttu-id="1c109-452">[OWIN](http://www.owin.org)アプリケーションは、IIS クラシック モードで実行できません。 統合モードで実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c109-452">[OWIN](http://www.owin.org) applications cannot run in IIS classic mode; they must be run in integrated mode.</span></span>

> [!NOTE]
> <span data-ttu-id="1c109-453">[OWIN](http://www.owin.org)完全信頼でアプリケーションを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c109-453">[OWIN](http://www.owin.org) applications must be run in full trust.</span></span>

<span data-ttu-id="1c109-454">このリリースでは、ミドルウェアで実行中の OWIN アプリケーションを検証するとエラーを調査に役立つエラー ページ ミドルウェアが含まれます Microsoft.Owin.Diagnostics パッケージのも含まれます。</span><span class="sxs-lookup"><span data-stu-id="1c109-454">This release also includes the Microsoft.Owin.Diagnostics package, which includes middleware to validate a running OWIN application, plus error-page middleware to help investigate failures.</span></span>

### <a name="authentication-components"></a><span data-ttu-id="1c109-455">認証コンポーネント</span><span class="sxs-lookup"><span data-stu-id="1c109-455">Authentication Components</span></span>

<span data-ttu-id="1c109-456">次の認証コンポーネントを利用できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-456">The following authentication components are available.</span></span>

- <span data-ttu-id="1c109-457">**Microsoft.Owin.Security.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="1c109-457">**Microsoft.Owin.Security.ActiveDirectory**.</span></span> <span data-ttu-id="1c109-458">内部設置型またはクラウド ベースのディレクトリ サービスを使用して認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="1c109-458">Enables authentication using on-premise or cloud-based directory services.</span></span>
- <span data-ttu-id="1c109-459">**Microsoft.Owin.Security.Cookies** cookie を使用して認証を有効します。</span><span class="sxs-lookup"><span data-stu-id="1c109-459">**Microsoft.Owin.Security.Cookies** Enables authentication using cookies.</span></span> <span data-ttu-id="1c109-460">このパッケージが以前に名前付き`Microsoft.Owin.Security.Forms`します。</span><span class="sxs-lookup"><span data-stu-id="1c109-460">This package was previously named `Microsoft.Owin.Security.Forms`.</span></span>
- <span data-ttu-id="1c109-461">**Microsoft.Owin.Security.Facebook** Facebook の OAuth ベースのサービスを使用して認証を有効します。</span><span class="sxs-lookup"><span data-stu-id="1c109-461">**Microsoft.Owin.Security.Facebook** Enables authentication using Facebook's OAuth-based service.</span></span>
- <span data-ttu-id="1c109-462">**Microsoft.Owin.Security.Google** Google の OpenID ベースのサービスを使用して認証を有効します。</span><span class="sxs-lookup"><span data-stu-id="1c109-462">**Microsoft.Owin.Security.Google** Enables authentication using Google's OpenID-based service.</span></span>
- <span data-ttu-id="1c109-463">**Microsoft.Owin.Security.Jwt** JWT トークンを使用して認証を有効します。</span><span class="sxs-lookup"><span data-stu-id="1c109-463">**Microsoft.Owin.Security.Jwt** Enables authentication using JWT tokens.</span></span>
- <span data-ttu-id="1c109-464">**Microsoft.Owin.Security.MicrosoftAccount** Enables authentication using Microsoft accounts.</span><span class="sxs-lookup"><span data-stu-id="1c109-464">**Microsoft.Owin.Security.MicrosoftAccount** Enables authentication using Microsoft accounts.</span></span>
- <span data-ttu-id="1c109-465">**Microsoft.Owin.Security.OAuth**.</span><span class="sxs-lookup"><span data-stu-id="1c109-465">**Microsoft.Owin.Security.OAuth**.</span></span> <span data-ttu-id="1c109-466">ベアラー トークンを認証するため、OAuth 承認サーバーだけでなくミドルウェアを提供します。</span><span class="sxs-lookup"><span data-stu-id="1c109-466">Provides an OAuth authorization server as well as middleware for authenticating bearer tokens.</span></span>
- <span data-ttu-id="1c109-467">**Microsoft.Owin.Security.Twitter** Twitter の OAuth ベースのサービスを使用して認証を有効します。</span><span class="sxs-lookup"><span data-stu-id="1c109-467">**Microsoft.Owin.Security.Twitter** Enables authentication using Twitter's OAuth-based service.</span></span>

<span data-ttu-id="1c109-468">このリリースにも含まれています、`Microsoft.Owin.Cors`パッケージで、HTTP のクロス オリジン要求を処理するためのミドルウェアが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1c109-468">This release also includes the `Microsoft.Owin.Cors` package, which contains middleware for processing cross-origin HTTP requests.</span></span>

> [!NOTE]
> <span data-ttu-id="1c109-469">JWT の署名のサポートは Visual Studio 2013 の最終バージョンで削除されました。</span><span class="sxs-lookup"><span data-stu-id="1c109-469">Support for JWT signing has been removed in the final version of Visual Studio 2013.</span></span>

<a id="ef6"></a>
## <a name="entity-framework-6"></a><span data-ttu-id="1c109-470">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="1c109-470">Entity Framework 6</span></span>

<span data-ttu-id="1c109-471">新機能および Entity Framework 6 の他の変更の一覧は、次を参照してください。 [Entity Framework のバージョン履歴](https://msdn.com/data/jj574253)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-471">For a list of new features and other changes in Entity Framework 6, see [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a><span data-ttu-id="1c109-472">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="1c109-472">ASP.NET Razor 3</span></span>

<span data-ttu-id="1c109-473">ASP.NET Razor 3 には、次の新機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="1c109-473">ASP.NET Razor 3 includes the following new features:</span></span>

- <span data-ttu-id="1c109-474">タブの編集をサポートします。</span><span class="sxs-lookup"><span data-stu-id="1c109-474">Support for Tab editing.</span></span> <span data-ttu-id="1c109-475">Preivously、**ドキュメントのフォーマット**コマンド、自動インデント、および自動の Visual Studio での書式設定が正しく動作しなかったを使用する場合、**タブの保持**オプション。</span><span class="sxs-lookup"><span data-stu-id="1c109-475">Preivously, the **Format Document** command, auto indenting, and auto formatting in Visual Studio did not work correctly when using the **Keep Tabs** option.</span></span> <span data-ttu-id="1c109-476">この変更は、Visual Studio の書式設定 タブの Razor コードの書式設定を修正します。</span><span class="sxs-lookup"><span data-stu-id="1c109-476">This change corrects Visual Studio formatting for Razor code for tab formatting.</span></span>
- <span data-ttu-id="1c109-477">リンクを生成するときに URL 書き換えルールをサポートします。</span><span class="sxs-lookup"><span data-stu-id="1c109-477">Support for URL Rewrite rules when generating links.</span></span>
- <span data-ttu-id="1c109-478">セキュリティ透過的な属性の削除。</span><span class="sxs-lookup"><span data-stu-id="1c109-478">Removal of security transparent attribute.</span></span>
  > [!NOTE]
  > <span data-ttu-id="1c109-479">これは、重大な変更と Razor 3 互換性のない MVC4 以前のバージョンと Razor 2 は MVC5 MVC5 に対してコンパイルされたアセンブリとの互換性はありません。</span><span class="sxs-lookup"><span data-stu-id="1c109-479">This is a breaking change, and makes Razor 3 incompatible with MVC4 and earlier, while Razor 2 is incompatible with MVC5 or assemblies compiled against MVC5.</span></span>

<span data-ttu-id="1c109-480">プレリリース版から Visual Studio 2013 で修正された razor 3 問題はあります[ここ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-480">Razor 3 issues fixed in Visual Studio 2013 from pre-release versions can be found [here](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).</span></span>

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a><span data-ttu-id="1c109-481">ASP.NET アプリを中断します。</span><span class="sxs-lookup"><span data-stu-id="1c109-481">ASP.NET App Suspend</span></span>

<span data-ttu-id="1c109-482">ASP.NET アプリの中断は、ユーザー エクスペリエンスと多数の 1 台のコンピューター上の ASP.NET のサイトをホストするための経済モデルを大幅に変更する .NET Framework 4.5.1 における革新的な機能です。</span><span class="sxs-lookup"><span data-stu-id="1c109-482">ASP.NET App Suspend is a game-changing feature in the .NET Framework 4.5.1 that radically changes the user experience and economic model for hosting large numbers of ASP.NET sites on a single machine.</span></span> <span data-ttu-id="1c109-483">詳細については、次を参照してください。 [ASP.NET App Suspend – 応答共有 .NET web ホスティング](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-483">For more information, see [ASP.NET App Suspend – responsive shared .NET web hosting](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).</span></span>

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="1c109-484">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="1c109-484">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="1c109-485">このセクションでは、Visual Studio 2013 用 ASP.NET および Web ツールの変更点と既知の問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="1c109-485">This section describes known issues and breaking changes in the ASP.NET and Web Tools for Visual Studio 2013.</span></span>

### <a name="nuget"></a><span data-ttu-id="1c109-486">NuGet</span><span class="sxs-lookup"><span data-stu-id="1c109-486">NuGet</span></span>

- <span data-ttu-id="1c109-487">[SLN ファイルを使用するときにモノラルで新しいパッケージの復元は機能しません](https://nuget.codeplex.com/workitem/3596)– 今後 nuget.exe ダウンロードで修正されると[NuGet.CommandLine パッケージ](http://www.nuget.org/packages/NuGet.CommandLine/)を更新します。</span><span class="sxs-lookup"><span data-stu-id="1c109-487">[New package restore doesn't work on Mono when using SLN file](https://nuget.codeplex.com/workitem/3596) – will be fixed in an upcoming nuget.exe download and [NuGet.CommandLine package](http://www.nuget.org/packages/NuGet.CommandLine/) update.</span></span>
- <span data-ttu-id="1c109-488">[Wix プロジェクトで新しいパッケージの復元は機能しません](https://nuget.codeplex.com/workitem/3598)– 今後 nuget.exe ダウンロードで修正されると[NuGet.CommandLine パッケージ](http://www.nuget.org/packages/NuGet.CommandLine/)を更新します。</span><span class="sxs-lookup"><span data-stu-id="1c109-488">[New package restore doesn't work with Wix projects](https://nuget.codeplex.com/workitem/3598) – will be fixed in an upcoming nuget.exe download and [NuGet.CommandLine package](http://www.nuget.org/packages/NuGet.CommandLine/) update.</span></span>
- <span data-ttu-id="1c109-489">[自動のパッケージ復元は、ソリューション フォルダーの下のプロジェクトのそれでも](https://nuget.codeplex.com/workitem/3625)– NuGet 2.8 で修正される予定です。</span><span class="sxs-lookup"><span data-stu-id="1c109-489">[Automatic Package restore doesn't work for projects under a solution folder](https://nuget.codeplex.com/workitem/3625) – will be fixed in NuGet 2.8.</span></span>

### <a name="aspnet-web-api"></a><span data-ttu-id="1c109-490">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="1c109-490">ASP.NET Web API</span></span>

1. <span data-ttu-id="1c109-491">`ODataQueryOptions<T>.ApplyTo(IQueryable)` 返さない`IQueryable<T>`おのサポートが追加されると、常に`$select`と`$expand`です。</span><span class="sxs-lookup"><span data-stu-id="1c109-491">`ODataQueryOptions<T>.ApplyTo(IQueryable)` doesn't return `IQueryable<T>` always, as we added support for `$select` and `$expand`.</span></span>

    <span data-ttu-id="1c109-492">以前サンプル`ODataQueryOptions<T>`から戻り値を常にキャスト`ApplyTo`に`IQueryable<T>`です。</span><span class="sxs-lookup"><span data-stu-id="1c109-492">Our earlier samples for `ODataQueryOptions<T>` always casted the return value from `ApplyTo` to `IQueryable<T>`.</span></span> <span data-ttu-id="1c109-493">以前この以前するクエリのオプションのための方法でサポート (`$filter`、 `$orderby`、 `$skip`、 `$top`) クエリの形状を変更しないでください。</span><span class="sxs-lookup"><span data-stu-id="1c109-493">This worked earlier because the query options that we supported earlier (`$filter`, `$orderby`, `$skip`, `$top`) do not change the shape of the query.</span></span> <span data-ttu-id="1c109-494">これで、サポートされています`$select`と`$expand`からの戻り値`ApplyTo`されません`IQueryable<T>`常にします。</span><span class="sxs-lookup"><span data-stu-id="1c109-494">Now that we support `$select` and `$expand` the return value from `ApplyTo` will not be `IQueryable<T>` always.</span></span>

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    <span data-ttu-id="1c109-495">前述の例とサンプル コードを使用している場合、クライアントが送信されなかった場合操作は続行されます`$select`と`$expand`です。</span><span class="sxs-lookup"><span data-stu-id="1c109-495">If you are using the sample code from earlier, it will continue working if the client does not send `$select` and `$expand`.</span></span> <span data-ttu-id="1c109-496">ただし、サポートする場合`$select`と`$expand`にそのコードを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c109-496">However, if you wish to support `$select` and `$expand` you have to change that code to this.</span></span>

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. <span data-ttu-id="1c109-497">**Request.Url または RequestContext.Url が null で、バッチ要求時に**</span><span class="sxs-lookup"><span data-stu-id="1c109-497">**Request.Url or RequestContext.Url is null during a batch request**</span></span>

    <span data-ttu-id="1c109-498">バッチ処理のシナリオで**UrlHelper**からアクセスする場合は null は**Request.Url**または**RequestContext.Url**です。</span><span class="sxs-lookup"><span data-stu-id="1c109-498">In a batching scenario, **UrlHelper** is null when accessed from **Request.Url** or **RequestContext.Url**.</span></span>

    <span data-ttu-id="1c109-499">この問題はここで現在追跡: [BatchRequestContext.Url が要求をバッチ処理の null](http://aspnetwebstack.codeplex.com/workitem/1301)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-499">This issue is currently tracked here: [BatchRequestContext.Url is null for batching request](http://aspnetwebstack.codeplex.com/workitem/1301).</span></span>

    <span data-ttu-id="1c109-500">この問題の回避策はの新しいインスタンスを作成する**UrlHelper**、次の例のようにします。</span><span class="sxs-lookup"><span data-stu-id="1c109-500">The workaround for this issue is to create a new instance of **UrlHelper**, as in the following example:</span></span>

    <span data-ttu-id="1c109-501">**UrlHelper の新しいインスタンスを作成します。**</span><span class="sxs-lookup"><span data-stu-id="1c109-501">**Creating a new instance of UrlHelper**</span></span>

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a><span data-ttu-id="1c109-502">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="1c109-502">ASP.NET MVC</span></span>

1. <span data-ttu-id="1c109-503">使用すると MVC5 と OrgAuth、AntiForgerToken 検証を実行するビューがある場合、ビューにデータを投稿すると、次のエラーに遭遇した可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1c109-503">When using MVC5 and OrgAuth, if you have views which do AntiForgerToken validation, you might come across the following error when you post data to the view:</span></span>

    <span data-ttu-id="1c109-504">**エラー**:</span><span class="sxs-lookup"><span data-stu-id="1c109-504">**Error**:</span></span>

    <span data-ttu-id="1c109-505">*'/' アプリケーションでのサーバー エラーです。*</span><span class="sxs-lookup"><span data-stu-id="1c109-505">*Server Error in '/' Application.*</span></span>

    <span data-ttu-id="1c109-506"><em>種類のクレーム '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'または'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>'、指定した ClaimsIdentity に存在しませんでした。クレーム ベース認証を使用した偽造防止トークンのサポートを有効にするには、構成されている要求プロバイダーが提供している生成 ClaimsIdentity インスタンスでこれらの要求の両方をしてくださいを確認します。代わりに、構成されている要求プロバイダーは、一意の識別子として、異なる要求種類を使用する場合は、静的プロパティ AntiForgeryConfig.UniqueClaimTypeIdentifier を設定して構成できます。</em></span><span class="sxs-lookup"><span data-stu-id="1c109-506"><em>A claim of type '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>' or '<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' was not present on the provided ClaimsIdentity. To enable anti-forgery token support with claims-based authentication, please verify that the configured claims provider is providing both of these claims on the ClaimsIdentity instances it generates. If the configured claims provider instead uses a different claim type as a unique identifier, it can be configured by setting the static property AntiForgeryConfig.UniqueClaimTypeIdentifier.</em></span></span>

    <span data-ttu-id="1c109-507">**回避策**:</span><span class="sxs-lookup"><span data-stu-id="1c109-507">**Workaround**:</span></span>

    <span data-ttu-id="1c109-508">その修正 Global.asax で、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="1c109-508">Add the following line in Global.asax to fix it:</span></span>

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    <span data-ttu-id="1c109-509">これは、次のリリースの修正されます。</span><span class="sxs-lookup"><span data-stu-id="1c109-509">This will be fixed for the next release.</span></span>
2. <span data-ttu-id="1c109-510">MVC5 に MVC4 アプリケーションをアップグレードした後は、ソリューションをビルドし、これを起動します。</span><span class="sxs-lookup"><span data-stu-id="1c109-510">After upgrading an MVC4 app to MVC5, build the solution and launch it.</span></span> <span data-ttu-id="1c109-511">次のエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1c109-511">You should see the following error:</span></span>

    <span data-ttu-id="1c109-512">[A]System.Web.WebPages.Razor.Configuration.HostSection cannot be cast to [B]System.Web.WebPages.Razor.Configuration.HostSection.</span><span class="sxs-lookup"><span data-stu-id="1c109-512">[A]System.Web.WebPages.Razor.Configuration.HostSection cannot be cast to [B]System.Web.WebPages.Razor.Configuration.HostSection.</span></span> <span data-ttu-id="1c109-513">タイプ A の発生元 'System.Web.WebPages.Razor、バージョン 2.0.0.0、Culture = neutral, PublicKeyToken = 31bf3856ad364e35 を =' コンテキストの位置には、' Default' で' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll' です。</span><span class="sxs-lookup"><span data-stu-id="1c109-513">Type A originates from 'System.Web.WebPages.Razor, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'.</span></span> <span data-ttu-id="1c109-514">タイプ B の発生元 'System.Web.WebPages.Razor, Version 3.0.0.0]、[カルチャを = = neutral, PublicKeyToken = 31bf3856ad364e35' コンテキストの位置には、' Default' で' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll' です。</span><span class="sxs-lookup"><span data-stu-id="1c109-514">Type B originates from 'System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.</span></span>

    <span data-ttu-id="1c109-515">上記のエラーを修正するには、開きます*すべて*プロジェクトとは、次の Web.config ファイル (Views フォルダーにあるものを含む)。</span><span class="sxs-lookup"><span data-stu-id="1c109-515">To fix the above error, open *all* the Web.config files (including the ones in the Views folder) in your project and do the following:</span></span>

   1. <span data-ttu-id="1c109-516">「5.0.0.0」の"System.Web.Mvc"のバージョン「4.0.0.0」のすべての項目を更新します。</span><span class="sxs-lookup"><span data-stu-id="1c109-516">Update all occurrences of version "4.0.0.0" of "System.Web.Mvc" to "5.0.0.0".</span></span>
   2. <span data-ttu-id="1c109-517">"System.Web.Helpers"のバージョン「2.0.0.0」のすべての項目を更新&quot;System.Web.WebPages&quot;と&quot;System.Web.WebPages.Razor&quot; 「3.0.0.0」を</span><span class="sxs-lookup"><span data-stu-id="1c109-517">Update all occurrences of version "2.0.0.0" of "System.Web.Helpers", &quot;System.Web.WebPages&quot; and &quot;System.Web.WebPages.Razor&quot; to "3.0.0.0"</span></span>

      <span data-ttu-id="1c109-518">など、上記の変更を加えた後アセンブリ バインドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="1c109-518">For example, after you make the above changes, the assembly bindings should look like this:</span></span>

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      <span data-ttu-id="1c109-519">MVC 5 に MVC 4 プロジェクトをアップグレードする方法については、次を参照してください。 [ASP.NET MVC 5 と Web API 2 に、ASP.NET MVC 4 と Web API プロジェクトをアップグレードする方法](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)です。</span><span class="sxs-lookup"><span data-stu-id="1c109-519">For information on upgrading MVC 4 projects to MVC 5, see [How to Upgrade an ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span></span>
3. <span data-ttu-id="1c109-520">、JQuery 検証の控えめなクライアント側の検証を使用しているときに、検証メッセージが正しくない場合もあります型の HTML input 要素を = 'number' です。</span><span class="sxs-lookup"><span data-stu-id="1c109-520">When using client-side validation with jQuery Unobtrusive Validation, the validation message is sometimes incorrect for an HTML input element with type='number'.</span></span> <span data-ttu-id="1c109-521">検証エラーに対して必要な値 (「[Age] フィールドが必要」) を表示、適切なメッセージの有効な数値が必要であるではなく無効な数値が入力されている場合。</span><span class="sxs-lookup"><span data-stu-id="1c109-521">The validation error for a required value ("The Age field is required") is shown when an invalid number is entered instead of the correct message that a valid number is required.</span></span>

    <span data-ttu-id="1c109-522">この問題は一般的に生じるスキャフォールディング コードで作成および編集ビューで整数のプロパティを使用してモデルのです。</span><span class="sxs-lookup"><span data-stu-id="1c109-522">This issue is commonly found with scaffolded code for a model with an integer property on the Create and Edit views.</span></span>

    <span data-ttu-id="1c109-523">この問題を回避するからエディター ヘルパーを変更します。</span><span class="sxs-lookup"><span data-stu-id="1c109-523">To work around this issue, change the editor helper from:</span></span>

    `@Html.EditorFor(person => person.Age)`

    <span data-ttu-id="1c109-524">移動先:</span><span class="sxs-lookup"><span data-stu-id="1c109-524">To:</span></span>

    `@Html.TextBoxFor(person => person.Age)`
4. <span data-ttu-id="1c109-525">ASP.NET MVC 5 は、部分的に信頼をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="1c109-525">ASP.NET MVC 5 no longer supports partial trust.</span></span> <span data-ttu-id="1c109-526">プロジェクトの MVC または WebAPI バイナリへのリンクを削除する必要があります、 [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx)属性および[AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx)属性。</span><span class="sxs-lookup"><span data-stu-id="1c109-526">Projects linking to the MVC or WebAPI binaries should remove the [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) attribute and the [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) attribute.</span></span> <span data-ttu-id="1c109-527">これらの属性を削除するには、次のようにコンパイラ エラーがなくなります。</span><span class="sxs-lookup"><span data-stu-id="1c109-527">Removing these attributes will eliminate compiler errors such as the following.</span></span>

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > <span data-ttu-id="1c109-528">ただし、操作の副作用は、同じアプリケーションで 4.0 および 5.0 のアセンブリを使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="1c109-528">Note, as a side effect of this you cannot use 4.0 and 5.0 assemblies in the same application.</span></span> <span data-ttu-id="1c109-529">5.0 に更新し、すべての必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c109-529">You need to update all of them to 5.0.</span></span>

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a><span data-ttu-id="1c109-530">Facebook の承認で SPA テンプレート可能性がありますが不安定になる IE、web サイトがイントラネット ゾーンでホストされているときに</span><span class="sxs-lookup"><span data-stu-id="1c109-530">SPA Template with Facebook authorization may cause instability in IE while the web site is hosted in intranet zone</span></span>

<span data-ttu-id="1c109-531">SPA テンプレートは、Facebook での外部ログを提供します。</span><span class="sxs-lookup"><span data-stu-id="1c109-531">The SPA template provides external log in with Facebook.</span></span> <span data-ttu-id="1c109-532">テンプレートを使用して作成されたプロジェクトは、ローカルで実行中は、サインインする場合がありますに IE がクラッシュします。</span><span class="sxs-lookup"><span data-stu-id="1c109-532">When the project created with the template is running locally, signing in may cause IE to crash.</span></span>

<span data-ttu-id="1c109-533">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="1c109-533">Solution:</span></span>

1. <span data-ttu-id="1c109-534">インターネット ゾーンの web サイトのホストまたは</span><span class="sxs-lookup"><span data-stu-id="1c109-534">Host the web site in internet zone; or</span></span>

2. <span data-ttu-id="1c109-535">IE 以外のブラウザーでシナリオをテストします。</span><span class="sxs-lookup"><span data-stu-id="1c109-535">Test the scenario in a browser other than IE.</span></span>

### <a name="web-forms-scaffolding"></a><span data-ttu-id="1c109-536">Web フォームのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="1c109-536">Web Forms Scaffolding</span></span>

<span data-ttu-id="1c109-537">Web フォームのスキャフォールディングは VS2013 から削除されましたし、Visual Studio に対する今後の更新で使用できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-537">Web Forms Scaffolding has been removed from VS2013 and will be available in a future update to Visual Studio.</span></span> <span data-ttu-id="1c109-538">ただし、MVC 依存関係を追加し、MVC のスキャフォールディングを生成して、Web フォーム プロジェクト内のスキャフォールディングを使用できます。</span><span class="sxs-lookup"><span data-stu-id="1c109-538">However, you can still use scaffolding within a Web Forms project by adding MVC dependencies and generating scaffolding for MVC.</span></span> <span data-ttu-id="1c109-539">プロジェクトは、Web フォームと MVC の組み合わせが格納されます。</span><span class="sxs-lookup"><span data-stu-id="1c109-539">Your project will contain a combination of Web Forms and MVC.</span></span>

<span data-ttu-id="1c109-540">MVC プロジェクトを追加する、Web フォーム、新しいスキャフォールディングされた項目を追加および選択**MVC 5 の依存関係**です。</span><span class="sxs-lookup"><span data-stu-id="1c109-540">To add MVC to your Web Forms project, add a new scaffolded item and select **MVC 5 Dependencies**.</span></span> <span data-ttu-id="1c109-541">最小またはスクリプトなどのコンテンツ ファイルのすべてを必要とするかどうかに応じて完全のいずれかを選択します。</span><span class="sxs-lookup"><span data-stu-id="1c109-541">Select either Minimal or Full depending on whether you need all of the content files, such as scripts.</span></span> <span data-ttu-id="1c109-542">次に、コント ローラーとビューを作成し、プロジェクトの MVC のスキャフォールディングされた項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="1c109-542">Then, add a scaffolded item for MVC, which will create views and a controller in your project.</span></span>

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="1c109-543">MVC と Web API のスキャフォールディング - HTTP 404 Not Found エラー</span><span class="sxs-lookup"><span data-stu-id="1c109-543">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="1c109-544">スキャフォールディングされた項目をプロジェクトに追加するときにエラーが発生した場合、プロジェクトは不整合な状態になることができます。</span><span class="sxs-lookup"><span data-stu-id="1c109-544">If an error is encountered when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="1c109-545">いくつかのスキャフォールディングをで行われた変更はロールバックされますが、インストールされている NuGet パッケージなど、他の変更はロールバックされません。</span><span class="sxs-lookup"><span data-stu-id="1c109-545">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="1c109-546">ルーティングの構成の変更がロールバックされた場合、ユーザーはスキャフォールディングされた項目に移動するときに HTTP 404 エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1c109-546">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="1c109-547">回避策:</span><span class="sxs-lookup"><span data-stu-id="1c109-547">Workaround:</span></span>

- <span data-ttu-id="1c109-548">MVC のこのエラーを解決する新しいスキャフォールディングされた項目を追加し、MVC 5 の依存関係の選択 (最小または完全のいずれか)。</span><span class="sxs-lookup"><span data-stu-id="1c109-548">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="1c109-549">このプロセスでは、プロジェクトに追加のすべての必要な変更します。</span><span class="sxs-lookup"><span data-stu-id="1c109-549">This process will add all of the required changes to your project.</span></span>
- <span data-ttu-id="1c109-550">Web api には、このエラーを解決します。</span><span class="sxs-lookup"><span data-stu-id="1c109-550">To fix this error for Web API:</span></span>

  1. <span data-ttu-id="1c109-551">WebApiConfig クラスをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="1c109-551">Add the WebApiConfig class to your project.</span></span>

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. <span data-ttu-id="1c109-552">アプリケーションで WebApiConfig.Register を構成\_Global.asax で次のようにメソッドを開始します。</span><span class="sxs-lookup"><span data-stu-id="1c109-552">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
