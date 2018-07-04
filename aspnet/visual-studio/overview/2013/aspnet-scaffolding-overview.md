---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Visual Studio 2013 で ASP.NET スキャフォールディング |Microsoft Docs
author: tfitzmac
description: ASP.NET のスキャフォールディングとは、Visual Studio 2013 に含まれている新機能です。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: f014ef1439376349f49f10f1f4063945ab81e7c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369093"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="6ef18-103">Visual Studio 2013 で ASP.NET スキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="6ef18-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="6ef18-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6ef18-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6ef18-105">ASP.NET のスキャフォールディングとは、Visual Studio 2013 に含まれている新機能です。</span><span class="sxs-lookup"><span data-stu-id="6ef18-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="6ef18-106">概要</span><span class="sxs-lookup"><span data-stu-id="6ef18-106">Overview</span></span>

<span data-ttu-id="6ef18-107">ASP.NET のスキャフォールディングは、ASP.NET Web アプリケーションのコード生成フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="6ef18-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="6ef18-108">Visual Studio 2013 には、MVC と Web API プロジェクトに対して事前にインストールされているコード ジェネレーターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6ef18-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="6ef18-109">データ モデルとやり取りするコードをすばやく追加するときに、スキャフォールディングをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="6ef18-110">スキャフォールディングを使用すると、開発プロジェクトでの標準的なデータ操作に時間を削減できます。</span><span class="sxs-lookup"><span data-stu-id="6ef18-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="6ef18-111">既定では、Visual Studio 2013 は Web フォーム プロジェクトでは、コードの生成をサポートしていませんが、MVC 依存関係をプロジェクトに追加するか、拡張機能のインストールで Web フォームのスキャフォールディングを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="6ef18-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="6ef18-112">どちらの方法は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-112">Both approaches are shown below.</span></span>

<span data-ttu-id="6ef18-113">Visual Studio 2013 Update 2 (現在 RC) は、シナリオの要件を満たすために ASP.NET スキャフォールディングを拡張する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="6ef18-114">この機能により、カスタマイズされたスキャフォールディング テンプレートを作成し、新しいスキャフォールディングの追加 ダイアログ ボックスに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="6ef18-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="6ef18-115">カスタマイズされたテンプレート内では、スキャフォールディングされた項目を追加するときに生成されるコードを指定します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="6ef18-116">詳細については、次を参照してください。 [for Visual Studio カスタム Scaffolder を作成する](https://go.microsoft.com/fwlink/p/?LinkId=395029)します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ef18-117">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="6ef18-117">Prerequisites</span></span>

<span data-ttu-id="6ef18-118">ASP.NET のスキャフォールディングを使用するには、が必要です。</span><span class="sxs-lookup"><span data-stu-id="6ef18-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="6ef18-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6ef18-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="6ef18-120">Web 開発者ツール (既定の Visual Studio 2013 のインストールの一部)</span><span class="sxs-lookup"><span data-stu-id="6ef18-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="6ef18-121">ASP.NET Web Frameworks and Tools 2013 (Visual Studio 2013 の既定のインストールの一部)</span><span class="sxs-lookup"><span data-stu-id="6ef18-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="6ef18-122">MVC または Web API へのスキャフォールディングされた項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="6ef18-123">スキャフォールディングを追加するには、プロジェクトまたはプロジェクト内のフォルダーを右クリックして**追加**–**スキャフォールディングされた新しい項目**、次の図のようにします。</span><span class="sxs-lookup"><span data-stu-id="6ef18-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![スキャフォールディングの項目を追加します。](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="6ef18-125">**スキャフォールディングの追加**ウィンドウで、スキャフォールディングの追加の種類を選択します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![スキャフォールディングの種類を選択します](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="6ef18-127">**コント ローラーの追加**ウィンドウは、Entity Framework 6 からの新しい非同期機能を使用するかどうかなど、コント ローラーを生成するためのオプションを選択することを示します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![コント ローラーを追加します。](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="6ef18-129">自分のシナリオは、関連するクラスとページが作成されます。</span><span class="sxs-lookup"><span data-stu-id="6ef18-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="6ef18-130">たとえば、次の図は、MVC コント ローラーと映画をという名前のモデル クラスのスキャフォールディングを使用して作成されたビューを示します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![作成したファイル](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="6ef18-132">Web フォームのスキャフォールディングされた項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="6ef18-133">Web フォームのコードを生成するスキャフォールディングを追加するには、Visual studio 拡張機能をインストールするか、MVC 依存関係を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6ef18-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="6ef18-134">どちらの方法は、以下に示しますが、これらの方法のいずれかを実行するだけで済みます。</span><span class="sxs-lookup"><span data-stu-id="6ef18-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="6ef18-135">Web フォームの拡張機能のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="6ef18-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="6ef18-136">Web フォーム プロジェクトでスキャフォールディングを使用するための Visual Studio 拡張機能をインストールすることができます。</span><span class="sxs-lookup"><span data-stu-id="6ef18-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="6ef18-137">Visual Studio で、次のように選択します。**ツール**し**拡張機能と更新**します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="6ef18-138">このダイアログ ボックスから検索用の Visual Studio ギャラリー **Web フォームのスキャフォールディング**します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![web フォームのスキャフォールディングをインストールします。](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="6ef18-140">詳細については、次を参照してください。 [Web フォームのスキャフォールディング](https://go.microsoft.com/fwlink/p/?LinkId=396478)します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="6ef18-141">MVC 依存関係</span><span class="sxs-lookup"><span data-stu-id="6ef18-141">MVC Dependencies</span></span>

<span data-ttu-id="6ef18-142">MVC 依存関係を追加するには、次のように選択します。**追加** - **スキャフォールディングされた新しい項目**します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="6ef18-143">[スキャフォールディングの追加] ウィンドウで、次のように選択します。 **MVC 依存関係の**以下に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="6ef18-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![MVC 依存関係を追加します。](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="6ef18-145">MVC; をスキャフォールディングするための 2 つのオプションがあります。最小限かつ完全です。</span><span class="sxs-lookup"><span data-stu-id="6ef18-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="6ef18-146">最低限を選択した場合は、NuGet パッケージと ASP.NET MVC の参照のみがプロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="6ef18-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="6ef18-147">すべてのオプションを選択した場合、MVC プロジェクトに必要なコンテンツ ファイルと、最小の依存関係が追加されます。</span><span class="sxs-lookup"><span data-stu-id="6ef18-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="6ef18-148">スキャフォールディングを簡単に使用するには、完全な依存関係を選択します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-148">To easily use scaffolding, select Full dependencies.</span></span>

![完全な依存関係を選択します。](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="6ef18-150">依存関係を追加した後、 **readme.txt**ファイル。</span><span class="sxs-lookup"><span data-stu-id="6ef18-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="6ef18-151">慎重にプロジェクトが正しく動作するためには、このファイルの指示に従ってください。</span><span class="sxs-lookup"><span data-stu-id="6ef18-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="6ef18-152">Readme.txt ファイル」の手順を完了すると、ときに、MVC と Web API の詳細については、前のセクションで示すように新しくスキャフォールディングした項目を追加できます。</span><span class="sxs-lookup"><span data-stu-id="6ef18-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="6ef18-153">自動的に生成されたビューとコント ローラーは、プロジェクト内で正しく機能します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="6ef18-154">チュートリアル</span><span class="sxs-lookup"><span data-stu-id="6ef18-154">Tutorials</span></span>

<span data-ttu-id="6ef18-155">カスタマイズされた scaffolder を作成するを参照してください。 [for Visual Studio カスタム Scaffolder を作成する](https://go.microsoft.com/fwlink/p/?LinkId=395029)します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="6ef18-156">生成されたファイルをカスタマイズするを参照してください。[スキャフォールディングされた新しい項目 ダイアログ ボックスから生成されたファイルをカスタマイズする方法について](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="6ef18-157">スキャフォールディングを使用する例については**Database First の開発**を参照してください[EF Database First と ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md)します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="6ef18-158">スキャフォールディングを使用する例については、 **MVC**プロジェクトを参照してください[ASP.NET MVC 5 の概要](../../../mvc/overview/getting-started/introduction/getting-started.md)します。</span><span class="sxs-lookup"><span data-stu-id="6ef18-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="6ef18-159">スキャフォールディングを使用する例については、 **Web API**プロジェクトを参照してください[属性ルーティングで Web API 2 で REST API の作成](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md)です。</span><span class="sxs-lookup"><span data-stu-id="6ef18-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
