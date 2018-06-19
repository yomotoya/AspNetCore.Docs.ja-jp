---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Visual Studio 2013 で ASP.NET スキャフォールディング |Microsoft ドキュメント
author: tfitzmac
description: ASP.NET のスキャフォールディングとは、Visual Studio 2013 に含まれている新機能です。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506731"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="2cd96-103">Visual Studio 2013 で ASP.NET スキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="2cd96-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="2cd96-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2cd96-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2cd96-105">ASP.NET のスキャフォールディングとは、Visual Studio 2013 に含まれている新機能です。</span><span class="sxs-lookup"><span data-stu-id="2cd96-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="2cd96-106">概要</span><span class="sxs-lookup"><span data-stu-id="2cd96-106">Overview</span></span>

<span data-ttu-id="2cd96-107">ASP.NET のスキャフォールディングとは、ASP.NET Web アプリケーションのコード生成フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="2cd96-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="2cd96-108">Visual Studio 2013 には、MVC、Web API プロジェクトに対して事前インストールされているコード ジェネレーターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2cd96-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="2cd96-109">データ モデルと対話するコードを簡単に追加するときに、スキャフォールディングをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="2cd96-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="2cd96-110">スキャフォールディングを使用すると、プロジェクトでの標準的なデータ操作を開発する時間を削減できます。</span><span class="sxs-lookup"><span data-stu-id="2cd96-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="2cd96-111">既定では、Visual Studio 2013 は Web フォームは、プロジェクト コードの生成をサポートしていませんが、プロジェクトに MVC 依存関係を追加するか、または拡張機能のインストールで Web フォームでスキャフォールディングを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="2cd96-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="2cd96-112">両方の方法は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="2cd96-112">Both approaches are shown below.</span></span>

<span data-ttu-id="2cd96-113">Visual Studio 2013 Update 2 (現在 RC) は、シナリオの要件を満たすにスキャフォールディングを ASP.NET を拡張する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="2cd96-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="2cd96-114">この機能により、カスタマイズされたスキャフォールディング テンプレートを作成し、新しい Scaffold の追加 ダイアログ ボックスに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="2cd96-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="2cd96-115">カスタマイズされたテンプレート内のスキャフォールディングされた項目を追加するときに生成されるコードを指定します。</span><span class="sxs-lookup"><span data-stu-id="2cd96-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="2cd96-116">詳細については、次を参照してください。 [for Visual Studio カスタム Scaffolder を作成する](https://go.microsoft.com/fwlink/p/?LinkId=395029)です。</span><span class="sxs-lookup"><span data-stu-id="2cd96-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2cd96-117">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="2cd96-117">Prerequisites</span></span>

<span data-ttu-id="2cd96-118">ASP.NET のスキャフォールディングを使用するのには、次が必要です。</span><span class="sxs-lookup"><span data-stu-id="2cd96-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="2cd96-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2cd96-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="2cd96-120">Web 開発者用ツール (既定の Visual Studio 2013 のインストールの一部)</span><span class="sxs-lookup"><span data-stu-id="2cd96-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="2cd96-121">ASP.NET Web フレームワークとツールの 2013 (既定の Visual Studio 2013 のインストールの一部)</span><span class="sxs-lookup"><span data-stu-id="2cd96-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="2cd96-122">MVC または Web API のスキャフォールディングされた項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="2cd96-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="2cd96-123">スキャフォールディングを追加するには、プロジェクトまたはプロジェクト内のフォルダーを右クリックして**追加**–**スキャフォールディングされた新しい項目**の次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="2cd96-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Scaffold 項目を追加します。](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="2cd96-125">**追加 Scaffold**ウィンドウで、追加する scaffold の種類を選択します。</span><span class="sxs-lookup"><span data-stu-id="2cd96-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Scaffold の種類を選択します。](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="2cd96-127">**コント ローラーの追加**ウィンドウが正しければ、コント ローラーは、Entity Framework 6 から新しい非同期機能を使用するかどうかなどを生成するためのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="2cd96-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![コント ローラーを追加します。](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="2cd96-129">シナリオに関連するクラスとページが作成されます。</span><span class="sxs-lookup"><span data-stu-id="2cd96-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="2cd96-130">たとえば、次の図は、MVC コント ローラーと映画をという名前のモデル クラスをスキャフォールディングによって作成されたビューを示しています。</span><span class="sxs-lookup"><span data-stu-id="2cd96-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![作成したファイル](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="2cd96-132">スキャフォールディングされた項目を Web フォームに追加します。</span><span class="sxs-lookup"><span data-stu-id="2cd96-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="2cd96-133">Web フォームのコードを生成するためのスキャフォールディングを追加するには、Visual studio 拡張機能のインストールか、MVC 依存関係を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2cd96-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="2cd96-134">両方の方法は、以下に示しますが、のみこれらのアプローチのいずれかを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2cd96-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="2cd96-135">拡張機能をスキャフォールディング web フォーム</span><span class="sxs-lookup"><span data-stu-id="2cd96-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="2cd96-136">プロジェクトを Web フォームのスキャフォールディングを使用するため、Visual Studio 拡張機能をインストールすることができます。</span><span class="sxs-lookup"><span data-stu-id="2cd96-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="2cd96-137">Visual Studio で、次のように選択します。**ツール**し**拡張機能と更新プログラム**です。</span><span class="sxs-lookup"><span data-stu-id="2cd96-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="2cd96-138">このダイアログ ボックスからの Visual Studio ギャラリーを検索**Web フォームのスキャフォールディング**です。</span><span class="sxs-lookup"><span data-stu-id="2cd96-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![web フォームのスキャフォールディングをインストールします。](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="2cd96-140">詳細については、次を参照してください。 [Web フォームのスキャフォールディング](https://go.microsoft.com/fwlink/p/?LinkId=396478)です。</span><span class="sxs-lookup"><span data-stu-id="2cd96-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="2cd96-141">MVC 依存関係</span><span class="sxs-lookup"><span data-stu-id="2cd96-141">MVC Dependencies</span></span>

<span data-ttu-id="2cd96-142">MVC 依存関係を追加するには、次のように選択します。**追加** - **スキャフォールディングされた新しい項目**です。</span><span class="sxs-lookup"><span data-stu-id="2cd96-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="2cd96-143">Scaffold の追加 ウィンドウで、次のように選択します。 **MVC 依存関係**、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="2cd96-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![MVC 依存関係を追加します。](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="2cd96-145">MVC; スキャフォールディングの 2 つのオプションがあります。最小限かつ完全します。</span><span class="sxs-lookup"><span data-stu-id="2cd96-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="2cd96-146">最低限を選択した場合は、NuGet パッケージの管理と ASP.NET MVC の参照のみがプロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="2cd96-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="2cd96-147">[完全] オプションを選択した場合、MVC プロジェクトの必要なコンテンツ ファイルだけでなく、最小の依存関係が追加されます。</span><span class="sxs-lookup"><span data-stu-id="2cd96-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="2cd96-148">スキャフォールディングを簡単に使用するには、すべての依存関係を選択します。</span><span class="sxs-lookup"><span data-stu-id="2cd96-148">To easily use scaffolding, select Full dependencies.</span></span>

![すべての依存関係を選択します。](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="2cd96-150">依存関係を追加すると、表示されます、 **readme.txt**ファイル。</span><span class="sxs-lookup"><span data-stu-id="2cd96-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="2cd96-151">慎重に、指示に従ってこのファイルをプロジェクトが正常に動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="2cd96-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="2cd96-152">Readme.txt ファイル」の手順を完了すると、MVC および Web API に関する前のセクションで示すように、新しいスキャフォールディングされた項目を追加できます。</span><span class="sxs-lookup"><span data-stu-id="2cd96-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="2cd96-153">自動的に生成されたビューとコント ローラーは、プロジェクト内で正しく機能します。</span><span class="sxs-lookup"><span data-stu-id="2cd96-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="2cd96-154">チュートリアル</span><span class="sxs-lookup"><span data-stu-id="2cd96-154">Tutorials</span></span>

<span data-ttu-id="2cd96-155">カスタマイズされた scaffolder を作成するを参照してください。 [for Visual Studio カスタム Scaffolder を作成する](https://go.microsoft.com/fwlink/p/?LinkId=395029)です。</span><span class="sxs-lookup"><span data-stu-id="2cd96-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="2cd96-156">生成されたファイルをカスタマイズするを参照してください。[スキャフォールディングされた新しい項目 ダイアログ ボックスから生成されたファイルをカスタマイズする方法について](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="2cd96-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="2cd96-157">スキャフォールディングを使用する例については**Database First の開発**を参照してください[EF Database First ASP.NET MVC で](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md)です。</span><span class="sxs-lookup"><span data-stu-id="2cd96-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="2cd96-158">スキャフォールディングを使用する例については、 **MVC**プロジェクトを参照してください[ASP.NET MVC 5 の概要](../../../mvc/overview/getting-started/introduction/getting-started.md)です。</span><span class="sxs-lookup"><span data-stu-id="2cd96-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="2cd96-159">スキャフォールディングを使用する例については、 **Web API**プロジェクトを参照してください[属性のルーティングが Web API 2 の REST API の作成](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md)です。</span><span class="sxs-lookup"><span data-stu-id="2cd96-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
