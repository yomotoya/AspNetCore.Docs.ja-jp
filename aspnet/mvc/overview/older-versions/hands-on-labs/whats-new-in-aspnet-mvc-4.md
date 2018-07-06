---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: ASP.NET MVC 4 の新機能新機能 |Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 は、既に確立されている設計パターンと、ASP.NET の機能を使用してスケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークとしています.
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 718a31de3d2d60788ba4affb0463a4ae871ef89a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805351"
---
# <a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="1d5f3-103">ASP.NET MVC 4 の新機能新機能</span><span class="sxs-lookup"><span data-stu-id="1d5f3-103">What's New in ASP.NET MVC 4</span></span>

<span data-ttu-id="1d5f3-104">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="1d5f3-105">Web のキャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="1d5f3-106">ASP.NET MVC 4 は、既に確立されている設計パターンと、ASP.NET と .NET framework の機能を使用してスケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="1d5f3-107">この新しいフレームワークの 4 番目のバージョンは、モバイル web アプリケーションの開発を容易に重点を置いています。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>

<span data-ttu-id="1d5f3-108">最初に、新しい ASP.NET MVC 4 プロジェクトを作成するときに今すぐ、モバイル アプリケーションのプロジェクト テンプレートをモバイル デバイス向けのスタンドアロン アプリのビルドに使用することができます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="1d5f3-109">さらに、ASP.NET MVC 4 では、jQuery.Mobile.MVC NuGet パッケージからの jQuery Mobile と統合されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="1d5f3-110">jQuery Mobile は、これに Windows Phone、iPhone、Android を含む、すべての一般的なモバイル デバイス プラットフォームと互換性のある web アプリを開発するための HTML5 ベースのフレームワーク。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="1d5f3-111">ただし、特殊化する場合は、ASP.NET MVC 4 こともできますを各種デバイス用のさまざまなビューを提供し、デバイス固有の最適化を提供します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>

<span data-ttu-id="1d5f3-112">このハンズオン ラボでは、開始、ASP.NET MVC 4 で&quot;インターネット アプリケーション&quot;フォト ギャラリーのアプリケーションを作成するプロジェクト テンプレート。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="1d5f3-113">さまざまなモバイル デバイスとデスクトップの web ブラウザーでの互換性を確保する jQuery Mobile と ASP.NET MVC 4 の新機能を使用してアプリを段階的に強化されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="1d5f3-114">コードの生成と ASP.NET MVC 4 簡単方法タスクをサポートすることによって、非同期アクション メソッドを記述するための新しいコード レシピについても学習&lt;ActionResult&gt;型を返します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>

> [!NOTE]
> <span data-ttu-id="1d5f3-115">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-115">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="1d5f3-116">このラボに固有のプロジェクトは、「 [ASP.NET 4.5 Web フォームで新](https://github.com/Microsoft-Web/HOL-ASPNETWebForms)します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-116">The project specific to this lab is available at [What's New in Web Forms in ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="1d5f3-117">目的</span><span class="sxs-lookup"><span data-stu-id="1d5f3-117">Objectives</span></span>

<span data-ttu-id="1d5f3-118">このハンズオン ラボでは、学習する方法。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="1d5f3-119">ASP.NET MVC プロジェクト テンプレートなど、新しいモバイル アプリケーション プロジェクト テンプレートを機能強化を活用します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-119">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="1d5f3-120">HTML5 ビューポート属性と CSS メディア クエリを使用して、モバイル デバイスの表示を改善するには</span><span class="sxs-lookup"><span data-stu-id="1d5f3-120">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="1d5f3-121">プログレッシブの機能強化と、タッチに最適化された web UI を構築するために、jQuery Mobile を使用してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-121">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="1d5f3-122">モバイル専用ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-122">Create mobile-specific views</span></span>
- <span data-ttu-id="1d5f3-123">ビュー スイッチャー コンポーネントを使用して、アプリケーションでモバイルおよびデスクトップのビューを切り替える</span><span class="sxs-lookup"><span data-stu-id="1d5f3-123">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="1d5f3-124">タスクのサポートを使用して非同期コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-124">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="1d5f3-125">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="1d5f3-125">Prerequisites</span></span>

<span data-ttu-id="1d5f3-126">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-126">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="1d5f3-127">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 B](#AppendixB)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-127">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="1d5f3-128">[ASP.NET MVC 4](../../../mvc4.md) (Microsoft Visual Studio 2012 のインストールに付属)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-128">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="1d5f3-129">Windows Phone エミュレーター (に含まれる、 [7.1.1 の Windows Phone SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="1d5f3-129">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="1d5f3-130">省略可能 - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/)で**Electric Plum iPhone シミュレーター**拡張機能 (手順 3 は、iPhone シミュレーターで、web アプリケーションを参照するために使用) の場合のみ</span><span class="sxs-lookup"><span data-stu-id="1d5f3-130">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="1d5f3-131">セットアップ</span><span class="sxs-lookup"><span data-stu-id="1d5f3-131">Setup</span></span>

<span data-ttu-id="1d5f3-132">ラボ ドキュメント全体を通じて、コード ブロックを挿入するよう指示されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-132">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="1d5f3-133">便宜上、そのコードのほとんどは、Visual Studio コード スニペット、手動で追加することを回避するために Visual Studio 内から使用することができますとして提供されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-133">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="1d5f3-134">コード スニペットをインストールするには</span><span class="sxs-lookup"><span data-stu-id="1d5f3-134">To install the code snippets:</span></span>

1. <span data-ttu-id="1d5f3-135">Windows エクスプ ローラー ウィンドウを開き、ラボを参照**Source\Setup**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-135">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="1d5f3-136">ダブルクリックして、 **Setup.cmd** Visual Studio のコード スニペットをインストールするには、このフォルダー内のファイル。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-136">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="1d5f3-137">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 a: を使用してコード スニペット](#AppendixA)&quot;します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="1d5f3-138">演習</span><span class="sxs-lookup"><span data-stu-id="1d5f3-138">Exercises</span></span>

<span data-ttu-id="1d5f3-139">このハンズオン ラボには、次の演習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-139">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="1d5f3-140">新しい ASP.NET MVC 4 プロジェクト テンプレート</span><span class="sxs-lookup"><span data-stu-id="1d5f3-140">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="1d5f3-141">フォト ギャラリーの Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-141">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="1d5f3-142">モバイル デバイスのサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-142">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="1d5f3-143">非同期コント ローラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-143">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="1d5f3-144">各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="1d5f3-145">作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="1d5f3-146">この演習の所要時間を推定: **60 分**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-146">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="1d5f3-147">手順 1: 新しい ASP.NET MVC 4 プロジェクト テンプレート</span><span class="sxs-lookup"><span data-stu-id="1d5f3-147">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="1d5f3-148">この演習では、ASP.NET MVC 4 プロジェクト テンプレートでの機能強化について説明します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-148">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="1d5f3-149">インターネット アプリケーション テンプレートに加えて MVC 3 に既に存在するこのバージョンになりましたモバイル アプリケーションの別のテンプレート。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-149">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="1d5f3-150">最初に、各テンプレートの関連する機能の一部になります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-150">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="1d5f3-151">次に、適切なアプローチを使用して、さまざまなプラットフォームでは正常にページのレンダリングの作業します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-151">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="1d5f3-152">タスク 1 - インターネット アプリケーションのテンプレートの表示</span><span class="sxs-lookup"><span data-stu-id="1d5f3-152">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="1d5f3-153">開いている**Visual Studio**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-153">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="1d5f3-154">選択、**ファイル |新しい |プロジェクト**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-154">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="1d5f3-155">**新しいプロジェクト**ダイアログ ボックスで、 **(Visual C#) |Web**左側のウィンドウでテンプレート ツリー、および選択**ASP.NET MVC 4 Web アプリケーション。**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-155">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="1d5f3-156">プロジェクトに名前を**PhotoGallery**、場所を選択します (または既定値のままに) をクリック**OK**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-156">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1d5f3-157">後で今すぐ作成する ASP.NET MVC 4 の PhotoGallery ソリューションをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-157">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="1d5f3-158">![新しいプロジェクトを作成する](whats-new-in-aspnet-mvc-4/_static/image1.png "新しいプロジェクトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-158">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="1d5f3-159">*新しいプロジェクトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-159">*Creating a new project*</span></span>
3. <span data-ttu-id="1d5f3-160">**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**プロジェクト テンプレートをクリックします**OK**。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-160">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="1d5f3-161">Razor ビュー エンジンとして選択したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-161">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="1d5f3-162">![新しい ASP.NET MVC 4 インターネット アプリケーションを作成する](whats-new-in-aspnet-mvc-4/_static/image2.png "新しい ASP.NET MVC 4 インターネット アプリケーションの作成")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-162">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="1d5f3-163">*新しい ASP.NET MVC 4 インターネット アプリケーションの作成*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-163">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1d5f3-164">Razor 構文は、ASP.NET MVC 3 で導入されました。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-164">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="1d5f3-165">その目的は、文字と、高速とワークフローをコーディング流体を有効にするファイルでは、必要なキーストロークの数を最小限に抑えるです。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-165">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="1d5f3-166">Razor を活用して既存の c#/VB (またはその他) の言語スキルと最高の HTML の構築ワークフローを有効にするテンプレート マークアップ構文を提供します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-166">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="1d5f3-167">キーを押して**F5**ソリューションを実行し、更新されたテンプレートを参照してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-167">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="1d5f3-168">次の機能を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-168">You can check out the following features:</span></span>

    <span data-ttu-id="1d5f3-169">**モダン スタイルのテンプレート**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-169">**Modern-style templates**</span></span>

    <span data-ttu-id="1d5f3-170">テンプレートが更新されました、現代的な外観の他のスタイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-170">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="1d5f3-171">![ASP.NET MVC 4 のスタイル テンプレート](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 のテンプレートのスタイル")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-171">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="1d5f3-172">*ASP.NET MVC 4 のスタイルのテンプレート*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-172">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="1d5f3-173">![新しい連絡先ページ](whats-new-in-aspnet-mvc-4/_static/image4.png "新しい連絡先 ページ")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-173">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="1d5f3-174">*新しい連絡先 ページ*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-174">*New Contact page*</span></span>

    <span data-ttu-id="1d5f3-175">**アダプティブ レンダリング**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-175">**Adaptive Rendering**</span></span>

    <span data-ttu-id="1d5f3-176">ブラウザー ウィンドウのサイズを確認し、どのページ レイアウトが動的に、新しいウィンドウ サイズに変化に注目してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-176">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="1d5f3-177">これらのテンプレートでは、アダプティブ レンダリング手法を使用して、カスタマイズすることがなく、デスクトップとモバイルの両方のプラットフォームで正しく表示します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-177">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="1d5f3-178">![ASP.NET MVC 4 プロジェクト テンプレートを別のブラウザーのサイズを](whats-new-in-aspnet-mvc-4/_static/image5.png "別のブラウザーのサイズを使用した ASP.NET MVC 4 プロジェクト テンプレート")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-178">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="1d5f3-179">*別のブラウザーのサイズを使用した ASP.NET MVC 4 プロジェクト テンプレート*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-179">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="1d5f3-180">**JavaScript で高度な UI**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-180">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="1d5f3-181">既定のプロジェクト テンプレートを別の拡張機能より対話的な JavaScript を提供する JavaScript の使用です。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-181">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="1d5f3-182">テンプレートで使用されるログインと登録のリンクは、jQuery の検証を使用して、クライアント側からの入力フィールドを検証する方法を含めました。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-182">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![jQuery の検証](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="1d5f3-184">*jQuery の検証*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-184">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1d5f3-185">2 つがする最初のセクションでのセクションではログには、サイトから登録されたアカウントを使用しておよび altenativelly ログオンする (既定で無効になっている) google などの別の認証サービスを使用して 2 番目のセクションを記録できます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-185">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="1d5f3-186">デバッガーを停止し、Visual Studio に戻り、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-186">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="1d5f3-187">ファイルを開く**AuthConfig.cs**の下にある、**アプリ\_開始**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-187">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="1d5f3-188">Google クライアントを登録する最後の行のコメントは削除*OAuth*認証します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-188">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > <span data-ttu-id="1d5f3-189">Facebook、Twitter、Microsoft などの任意の OpenID または OAuth サービスを使用して認証を簡単に実現できますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-189">Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.</span></span>
8. <span data-ttu-id="1d5f3-190">キーを押して**F5**ソリューションを実行し、ログイン ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-190">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="1d5f3-191">選択**Google**サービスにログインします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-191">Select **Google** service to log in.</span></span>

    ![サービスでのログの選択](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="1d5f3-193">*サービスでのログの選択*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-193">*Selecting the log in service*</span></span>
10. <span data-ttu-id="1d5f3-194">Google アカウントを使用してをログインします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-194">Log in using your Google account.</span></span>
11. <span data-ttu-id="1d5f3-195">Google アカウントから情報を取得する (localhost)、サイトを許可します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-195">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="1d5f3-196">最後に、Google アカウントに関連付けるサイトに登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-196">Finally, you will have to register in the site to associate the Google account.</span></span>

   ![Google アカウントを関連付ける](whats-new-in-aspnet-mvc-4/_static/image8.png)

   <span data-ttu-id="1d5f3-198">*Google アカウントを関連付ける*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-198">*Associating your Google account*</span></span>
13. <span data-ttu-id="1d5f3-199">デバッガーを停止し、Visual Studio に戻り、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-199">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="1d5f3-200">プロジェクト テンプレートの ASP.NET MVC 4 で導入されたいくつかその他の新機能を確認するソリューションを使ってみるようになりました。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-200">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

   <span data-ttu-id="1d5f3-201">![ASP.NET MVC 4 インターネット アプリケーションのプロジェクト テンプレート](whats-new-in-aspnet-mvc-4/_static/image9.png "ASP.NET MVC 4 インターネット アプリケーションのプロジェクト テンプレート")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-201">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

   <span data-ttu-id="1d5f3-202">*ASP.NET MVC 4 インターネット アプリケーションのプロジェクト テンプレート*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-202">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   - <span data-ttu-id="1d5f3-203">**HTML 5 マークアップ**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-203">**HTML 5 Markup**</span></span>

       <span data-ttu-id="1d5f3-204">新しいテーマのマークアップを確認するテンプレート ビューを参照します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-204">Browse template views to find out the new theme markup.</span></span>

       <span data-ttu-id="1d5f3-205">![Razor、HTML5 マークアップ About.cshtml を使用して新しいテンプレートです。](whats-new-in-aspnet-mvc-4/_static/image10.png "Razor、HTML5 マークアップ About.cshtml を使用して、新しいテンプレート。")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-205">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

       <span data-ttu-id="1d5f3-206">*Razor、HTML5 マークアップ (About.cshtml) を使用して新しいテンプレート。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-206">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
   - <span data-ttu-id="1d5f3-207">**更新された JavaScript ライブラリ**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-207">**Updated JavaScript libraries**</span></span>

       <span data-ttu-id="1d5f3-208">これで、ASP.NET MVC 4 の既定のテンプレートには、KnockoutJS、豊富な作成できる JavaScript の MVVM フレームワーク、および JavaScript と HTML を使った応答性に優れた web アプリケーションが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-208">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="1d5f3-209">ような MVC3 で jQuery と jQuery UI ライブラリも含まれます ASP.NET MVC 4 で。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-209">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

     > [!NOTE]
     > <span data-ttu-id="1d5f3-210">このリンクに KnockOutJS ライブラリの詳細を表示できます: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/)します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-210">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="1d5f3-211">さらに、jQuery と jQuery UI について学習できますで[ [ http://docs.jquery.com/ ](http://docs.jquery.com/)](http://docs.jquery.com/)します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-211">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="1d5f3-212">タスク 2 - モバイルのアプリケーション テンプレートの表示</span><span class="sxs-lookup"><span data-stu-id="1d5f3-212">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="1d5f3-213">ASP.NET MVC 4 には、モバイル web サイトやタブレットのブラウザーの開発が容易になります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-213">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="1d5f3-214">このテンプレート (コント ローラー コードとほぼ同様ですが、注意)、インターネット アプリケーション テンプレートと同じアプリケーション構造を持つが、タッチ ベースのモバイル デバイスで正しく表示するためにそのスタイルが変更されました。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-214">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="1d5f3-215">選択、**ファイル |新しい |プロジェクト**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-215">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="1d5f3-216">**新しいプロジェクト**ダイアログ ボックスで、 **(Visual C#) |Web**左側のウィンドウでテンプレート ツリーを選び、 **ASP.NET MVC 4 Web アプリケーション。**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-216">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="1d5f3-217">プロジェクトの名前**PhotoGallery.Mobile**、場所を選択します (または既定値のままに)、選択&quot;ソリューションに追加&quot; をクリック**OK**。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-217">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="1d5f3-218">**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、 **Mobile アプリケーション**プロジェクト テンプレートをクリックします**OK**。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-218">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="1d5f3-219">Razor ビュー エンジンとして選択したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-219">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="1d5f3-220">![新しい ASP.NET MVC 4 モバイル アプリケーションを作成する](whats-new-in-aspnet-mvc-4/_static/image11.png "新しい ASP.NET MVC 4 モバイル アプリケーションを作成します。")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-220">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="1d5f3-221">*新しい ASP.NET MVC 4 モバイル アプリケーションを作成します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-221">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="1d5f3-222">これは、ソリューションを探して、モバイル向け ASP.NET MVC 4 のソリューション テンプレートで導入された新しい機能の一部を確認できます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-222">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="1d5f3-223">**jQuery Mobile ライブラリ**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-223">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="1d5f3-224">モバイル アプリケーションのプロジェクト テンプレートには、モバイル ブラウザーの互換性のためのオープン ソース ライブラリは、jQuery Mobile ライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-224">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="1d5f3-225">jQuery Mobile では、CSS および JavaScript をサポートするモバイル ブラウザーにプログレッシブ エンハンスメントが適用されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-225">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="1d5f3-226">プログレッシブ エンハンスメントしか有効に最も強力なブラウザーにリッチ コンテンツを表示中に、web ページの基本コンテンツを表示するすべてのブラウザーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-226">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="1d5f3-227">JQuery モバイルのスタイルに含まれる、JavaScript と CSS ファイルには、ページ マークアップでの変更を加えずに、画面で、コンテンツに合わせてモバイル ブラウザーが役立ちます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-227">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="1d5f3-229">*テンプレートに含まれる jQuery mobile ライブラリ*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-229">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="1d5f3-230">**HTML5 ベースのマークアップ**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-230">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="1d5f3-232">*HTML5 マークアップ、(Login.cshtml および index.cshtml) を使用してモバイル アプリケーション テンプレート*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-232">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="1d5f3-233">キーを押して**F5**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-233">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="1d5f3-234">開く、 **Windows Phone 7 エミュレーター**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-234">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="1d5f3-235">Phone のスタート画面では、Internet Explorer を開きます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-235">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="1d5f3-236">デスクトップ アプリケーションの開始場所の URL を確認し、携帯電話からその URL に移動 (例: `http://localhost:[PortNumber]/`)。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-236">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="1d5f3-237">ログイン ページに入力するか、チェック アウトすることができるので、ページについて。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-237">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="1d5f3-238">Web サイトのスタイルが mobile 用の新しい Metro アプリに基づいていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-238">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="1d5f3-239">モバイル デバイス、ページのすべての要素が表示され有効になっていることを確認するのには、ASP.NET MVC 4 プロジェクト テンプレートが正しく表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-239">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="1d5f3-240">ヘッダーのリンクがクリックしてまたはタップする対処できるだけの十分なことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-240">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="1d5f3-241">![プロジェクトのテンプレートのページはモバイル デバイスで](whats-new-in-aspnet-mvc-4/_static/image14.png "テンプレート ページはモバイル デバイスでのプロジェクト")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-241">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="1d5f3-242">*モバイル デバイスでのプロジェクト テンプレートのページ*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-242">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="1d5f3-243">新しいテンプレートを使用しても、**ビューポート メタ タグ**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-243">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="1d5f3-244">ほとんどのモバイル ブラウザーが仮想ブラウザー ウィンドウの幅を定義または&quot;ビューポート&quot;、これは、モバイル デバイスの実際の幅より大きい。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-244">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="1d5f3-245">これにより、モバイル ブラウザーに仮想表示内で web ページ全体を表示できます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-245">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="1d5f3-246">**ビューポート メタ タグ**により、web 開発者はモバイル デバイスで、幅、高さ、およびブラウザーの領域のスケールを設定する**します。**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-246">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="1d5f3-247">モバイル アプリケーション用の ASP.NET MVC 4 テンプレートは、デバイスの幅にビューポートを設定 (&quot;幅 = デバイスの幅&quot;) レイアウト テンプレート (*views \shared\_Layout.cshtml*) できるように、すべて、ページには、デバイスの画面幅に設定、ビューポートがあります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-247">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="1d5f3-248">ビューポートのメタ タグによって既定のブラウザー ビューは変更されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-248">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="1d5f3-249">開いている **\_Layout.cshtml**にある、**ビュー |共有**フォルダー、およびコメント ビューポート メタ タグ。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-249">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="1d5f3-250">アプリケーションを実行されていない場合は既に開かれるとの相違点を確認します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-250">Run the application, if not already opened, and check out the differences.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

<span data-ttu-id="1d5f3-251">![ビューポートの meta タグをコメント後サイト](whats-new-in-aspnet-mvc-4/_static/image15.png "ビューポートのメタ タグをコメント化した後、サイト")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-251">![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")</span></span>

<span data-ttu-id="1d5f3-252">*ビューポートの meta タグをコメント化した後、サイト*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-252">*The site after commenting the viewport meta tag*</span></span>
10. <span data-ttu-id="1d5f3-253">Visual Studio で、キーを押します**SHIFT** + **f5 キーを押して**アプリケーションのデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-253">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="1d5f3-254">ビューポートの meta タグをコメント解除します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-254">Uncomment the viewport meta tag.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="1d5f3-255">タスク 3 - アダプティブ レンダリングを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-255">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="1d5f3-256">このタスクでは、モバイル デバイスと Web ブラウザーを正しく、Web ページをカスタマイズすることがなく同時に表示するために別の方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-256">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="1d5f3-257">既に同様の目的でビューポート メタ タグを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-257">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="1d5f3-258">別の強力な方法に変化するようになりました:*アダプティブ レンダリング*します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-258">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="1d5f3-259">アダプティブ レンダリングを使用する手法は、 **CSS3 メディア クエリ**ページに適用されるスタイルをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-259">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="1d5f3-260">メディア クエリでは、特定の条件下で CSS スタイルをグループ化、スタイル シート内の条件を定義します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-260">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="1d5f3-261">条件が true の場合のみ、スタイルは、宣言されたオブジェクトに適用されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-261">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="1d5f3-262">アダプティブ レンダリング手法によって提供される柔軟性により、さまざまなデバイスで、サイトを表示するためのカスタマイズができます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-262">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="1d5f3-263">1 つのスタイル シートのロジックのコードを記述することがなく、スタイルを選択、同数のスタイルを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-263">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="1d5f3-264">したがって、ページのスタイルに適合させる非常に便利な方法は、コードの重複と目的を表示するためのロジックの量が減ることが。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-264">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="1d5f3-265">その一方で、帯域幅の消費量が増加、CSS ファイルのサイズが若干大きくなりすぎるとします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-265">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="1d5f3-266">アダプティブ レンダリング手法を使用して、サイトがなります**ブラウザーに関係なく、適切に表示されます。**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-266">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="1d5f3-267">ただしに問題が追加の帯域幅を読み込む場合を考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-267">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="1d5f3-268">メディア クエリの基本形式です: @media \[スコープ: すべて | ハンドヘルド | 印刷 | プロジェクション | 画面\](プロパティ: 値と.プロパティ: 値)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-268">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="1d5f3-269">メディア クエリの例: &gt;  <strong>@mediaすべてと (幅の最大値: 1000 ピクセル) と (幅の最小値: 700 px) {}:</strong> 700 px と 1000 ピクセルの間のすべての解像度。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-269">Examples of media queries: &gt;<strong>@media all and (max-width: 1000px) and (min-width: 700px) {}:</strong> For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="1d5f3-270"><strong>@media 画面と (幅の最小値: 400 px) と (幅の最大値: 700 px) {…}:</strong>画面に対してのみです。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-270"><strong>@media screen and (min-width: 400px) and (max-width: 700px) { ... }:</strong> Only for screens.</span></span> <span data-ttu-id="1d5f3-271">解像度は、400 ~ 700 px にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-271">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="1d5f3-272"><strong>@media ハンドヘルドと (幅の最小値: 20em)、画面と (幅の最小値: 20em) {…}:</strong>ハンドヘルド デバイス (モバイルおよびデバイス) と画面の。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-272"><strong>@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:</strong> For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="1d5f3-273">幅の最小値は、20em より大きくする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-273">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="1d5f3-274">詳細についてを見つけることができます、 [W3C サイト](http://www.w3.org/TR/css3-mediaqueries/)します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-274">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="1d5f3-275">アダプティブ レンダリングの動作方法を今すぐ調査、ASP.NET MVC の読みやすさを向上させる 4 既定の web サイト テンプレート。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-275">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="1d5f3-276">開く、 **PhotoGallery.sln**選択をタスク 1 で作成したソリューション、 **PhotoGallery**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-276">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="1d5f3-277">キーを押して**F5**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-277">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="1d5f3-278">ブラウザーの幅を半分にまたはより小さいの元のサイズの四半期に、windows の設定のサイズを変更します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-278">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="1d5f3-279">ヘッダー内の項目で発生する内容に注意してください。 一部の要素は、ヘッダーの表示領域に表示されません。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-279">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="1d5f3-280">開いている<strong>Site.css</strong>ファイルで Visual Studio ソリューション エクスプ ローラーから<strong>コンテンツ</strong>プロジェクト フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-280">Open <strong>Site.css</strong> file from the Visual Studio Solution explorer, located in <strong>Content</strong> project folder.</span></span> <span data-ttu-id="1d5f3-281">キーを押して<strong>CTRL + F</strong>を Visual Studio の統合された検索を開いたり書き込んだりする<strong>@media</strong>を検索する、 <strong>CSS メディア クエリ</strong>します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-281">Press <strong>CTRL + F</strong> to open Visual Studio integrated search, and write <strong>@media</strong> to locate the <strong>CSS media query</strong>.</span></span>

    <span data-ttu-id="1d5f3-282">このテンプレートで定義されているメディア クエリの条件は、この方法で機能します。 ときに、ブラウザーのウィンドウのサイズを下回って**850 px**、適用される CSS ルールがこのメディア ブロック内で定義されています。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-282">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="1d5f3-283">![メディア クエリを検索する](whats-new-in-aspnet-mvc-4/_static/image16.png "メディア クエリを検索します。")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-283">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="1d5f3-284">*メディア クエリを検索します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-284">*Locating the media query*</span></span>
4. <span data-ttu-id="1d5f3-285">850 に設定された最大幅属性値を置き換えると px **10px**アダプティブのレンダリングを無効にするために、キーを押します**CTRL + S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-285">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="1d5f3-286">キーを押して、ブラウザーに戻って**ctrl キーを押しながら f5 キーを押して**が変更されたページを更新します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-286">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="1d5f3-287">ウィンドウの幅を調整する場合は、両方のページの違いに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-287">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="1d5f3-288">![ページの適用、左側で、@mediaスタイル、スタイル、右を省略すると](whats-new-in-aspnet-mvc-4/_static/image17.png "左で、ページを適用する、@mediaスタイル、スタイル、右を省略すると")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-288">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="1d5f3-289"><em>ページの適用、左側で、@mediaスタイル、スタイル、右を省略すると</em></span><span class="sxs-lookup"><span data-stu-id="1d5f3-289"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="1d5f3-290">ここで、モバイル デバイス上の動作確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-290">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="1d5f3-291">![ページの適用、左側で、@mediaスタイル、スタイル、右を省略すると](whats-new-in-aspnet-mvc-4/_static/image18.png "左で、ページを適用する、@mediaスタイル、スタイル、右を省略すると")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-291">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="1d5f3-292"><em>ページの適用、左側で、@mediaスタイル、スタイル、右を省略すると</em></span><span class="sxs-lookup"><span data-stu-id="1d5f3-292"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="1d5f3-293">モバイル デバイスを使用する場合、Web ブラウザーで、ページをレンダリングすると、変更は、非常に重要ないないと表示されますが、違いはより明確になります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-293">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="1d5f3-294">イメージの左側にある、カスタムのスタイルが、読みやすさを向上することを確認できます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-294">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="1d5f3-295">条件付きの Web サイトにスタイル設定、および便利な方法でスタイルの一般的な問題を解決するために適用しやすく、多くの詳細シナリオでは、アダプティブ レンダリングを使用できます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-295">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="1d5f3-296">ビューポート meta タグと CSS メディア クエリに限定されない ASP.NET MVC 4 では、任意の web アプリケーションでこれらの機能を活用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-296">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="1d5f3-297">Visual Studio で、キーを押します**SHIFT** + **f5 キーを押して**アプリケーションのデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-297">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="1d5f3-298">手順 2: フォト ギャラリーの Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-298">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="1d5f3-299">この演習では、写真を表示する、フォト ギャラリーのアプリケーションで機能します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-299">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="1d5f3-300">ASP.NET MVC 4 プロジェクト テンプレートから開始し、サービスから写真を取得し、ホーム ページに表示するための機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-300">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="1d5f3-301">次の演習では、モバイル デバイスでの表示方法を強化するためには、このソリューションを更新します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-301">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="1d5f3-302">タスク 1 - 写真のモック サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-302">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="1d5f3-303">このタスクでは、ギャラリーに表示される内容を取得する写真のサービスのモックを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-303">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="1d5f3-304">これを行うには、それぞれの写真データを含む JSON ファイルを返すだけですが、新しいコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-304">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="1d5f3-305">開いている**Visual Studio**開いていない場合。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-305">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="1d5f3-306">選択、**ファイル |新しい |プロジェクト**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-306">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="1d5f3-307">**新しいプロジェクト**ダイアログ ボックスで、 **(Visual C#) |Web**左側のウィンドウでテンプレート ツリー、および選択**ASP.NET MVC 4 Web アプリケーション。**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-307">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="1d5f3-308">プロジェクトに名前を**PhotoGallery**、場所を選択します (または既定値のままに) をクリック**OK**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-308">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="1d5f3-309">または、既存の ASP.NET MVC 4 から作業を継続できます**インターネット アプリケーション**ソリューションから**演習 1**し、次の手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-309">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="1d5f3-310">**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**プロジェクト テンプレートをクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-310">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="1d5f3-311">Razor ビュー エンジンとして選択されている必要があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-311">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="1d5f3-312">**ソリューション エクスプ ローラー**を右クリックし、**アプリ\_データ**フォルダー、プロジェクト、および select の**追加 |既存項目の**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-312">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="1d5f3-313">参照、 **Source\Assets\App\_データ**このラボのフォルダーを追加し、 **Photos.json**ファイル。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-313">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="1d5f3-314">名前の新しいコント ローラーを作成**PhotoController**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-314">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="1d5f3-315">これを行うを右クリックし、**コント ローラー**フォルダーに移動して**追加**選択**コント ローラー。**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-315">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="1d5f3-316">コント ローラーの名前でそのまま使用、**空の MVC コント ローラー**テンプレートとクリック**追加**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-316">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="1d5f3-317">![追加、PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "PhotoController を追加します。")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-317">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="1d5f3-318">*PhotoController を追加します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-318">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="1d5f3-319">置換、**インデックス**メソッドを次**ギャラリー**アクション、およびプロジェクトに最近追加した JSON ファイルからコンテンツの戻り値。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-319">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="1d5f3-320">(コード スニペット - *ASP.NET MVC 4 ラボ - Ex02 - ギャラリー アクション*)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-320">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. <span data-ttu-id="1d5f3-321">キーを押して**F5** 、ソリューションを実行し、写真のモック サービスをテストするには、次の URL を参照する: `http://localhost:[port]/photo/gallery` ([port] の値は、現在のポートが、アプリケーションが起動された場所によって異なります)。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-321">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="1d5f3-322">この URL に要求のコンテンツを取得する必要があります、 **Photos.json**ファイル。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-322">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="1d5f3-323">![写真のモック サービスをテスト](whats-new-in-aspnet-mvc-4/_static/image20.png "モック フォト サービスのテスト")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-323">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="1d5f3-324">*写真のモック サービスのテスト*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-324">*Testing the mocked photo service*</span></span>

<span data-ttu-id="1d5f3-325">実際の実装で使用することが[ASP.NET Web API](../../../../web-api/index.md)フォト ギャラリーのサービスを実装します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-325">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="1d5f3-326">ASP.NET Web API をさまざまなブラウザーやモバイル デバイスなどのクライアントに提供される HTTP サービスを構築するが容易にするフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-326">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="1d5f3-327">ASP.NET Web API は、.NET Framework に基づいて RESTful アプリケーションを構築するのに最適なプラットフォームです。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-327">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="1d5f3-328">タスク 2 - フォト ギャラリーを表示します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-328">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="1d5f3-329">このタスクでは、この演習の最初のタスクで作成したモック サービスを使用して、フォト ギャラリーを表示するホーム ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-329">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="1d5f3-330">モデル ファイルを追加し、ギャラリーのビューを更新します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-330">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="1d5f3-331">Visual Studio で、キーを押します**SHIFT** + **f5 キーを押して**アプリケーションのデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-331">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="1d5f3-332">作成、**フォト**クラス、**モデル**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-332">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="1d5f3-333">これを行うを右クリックし、**モデル**フォルダーで、**追加**クリック**クラス**。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-333">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="1d5f3-334">名前に設定し、 **Photo.cs**クリック**追加**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-334">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="1d5f3-335">次のメンバーを追加、**フォト**クラス。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-335">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="1d5f3-336">(コード スニペット - *ASP.NET MVC 4 ラボ - Ex02 - 写真モデル*)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-336">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. <span data-ttu-id="1d5f3-337">**Controllers** フォルダーから **HomeController.cs** ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-337">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="1d5f3-338">次の using ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-338">Add the following using statements.</span></span>

    <span data-ttu-id="1d5f3-339">(コード スニペット - *ASP.NET MVC 4 ラボ - Ex02 - HomeController Using*)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-339">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. <span data-ttu-id="1d5f3-340">更新プログラム、**インデックス**に使用するアクション**HttpClient**ギャラリーのデータを取得し、使用する、 **JavaScriptSerializer**ビュー モデルに逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-340">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="1d5f3-341">(コード スニペット - *ASP.NET MVC 4 ラボ - Ex02 の Index アクション*)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-341">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. <span data-ttu-id="1d5f3-342">開く、 **Index.cshtml**下にあるファイル、 **views \home**フォルダーと次のコードですべてのコンテンツを置換します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-342">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="1d5f3-343">このコードは、サービスから取得したすべての写真をループ処理し、順不同のリストに表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-343">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="1d5f3-344">(コード スニペット - *ASP.NET MVC 4 ラボ - Ex02 - 写真リスト*)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-344">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. <span data-ttu-id="1d5f3-345">**ソリューション エクスプ ローラー**を右クリックし、**コンテンツ**フォルダー、プロジェクト、および select の**追加 |既存項目の**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-345">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="1d5f3-346">参照、 **Source\Assets\Content**このラボのフォルダーを追加し、 **Site.css**ファイル。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-346">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="1d5f3-347">その置き換えを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-347">You will have to confirm its replacement.</span></span> <span data-ttu-id="1d5f3-348">ある場合、 **Site.css**ファイルを開く、ファイルを再読み込みも確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-348">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="1d5f3-349">ファイル エクスプ ローラーを開き、全体をコピーします**写真**フォルダーの下にある、 **Source\Assets**ソリューション エクスプ ローラーでプロジェクトのルート フォルダーには、このラボのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-349">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="1d5f3-350">アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-350">Run the application.</span></span> <span data-ttu-id="1d5f3-351">ギャラリーで写真を表示するホーム ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-351">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="1d5f3-352">![フォト ギャラリー](whats-new-in-aspnet-mvc-4/_static/image21.png "フォト ギャラリー")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-352">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="1d5f3-353">*フォト ギャラリー*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-353">*Photo Gallery*</span></span>
11. <span data-ttu-id="1d5f3-354">Visual Studio で、キーを押します**SHIFT** + **f5 キーを押して**アプリケーションのデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-354">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="1d5f3-355">手順 3: モバイル デバイスのサポートの追加</span><span class="sxs-lookup"><span data-stu-id="1d5f3-355">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="1d5f3-356">ASP.NET MVC 4 でキーの更新プログラムの 1 つは、モバイル開発のためのサポートです。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-356">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="1d5f3-357">この演習では、前の演習で作成した PhotoGallery ソリューションを拡張してモバイル アプリケーションの ASP.NET MVC 4 の新機能が学びます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-357">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="1d5f3-358">タスク 1 - ASP.NET MVC 4 アプリケーションで jQuery Mobile をインストールします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-358">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="1d5f3-359">開く、**開始**ソリューションがある**ソース/Ex3-MobileSupport/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-359">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="1d5f3-360">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-360">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="1d5f3-361">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-361">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="1d5f3-362">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-362">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="1d5f3-363">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-363">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="1d5f3-364">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-364">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="1d5f3-365">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-365">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="1d5f3-366">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-366">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="1d5f3-367">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-367">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="1d5f3-368">開く、**パッケージ マネージャー コンソール**をクリックして、**ツール** &gt; **ライブラリ パッケージ マネージャー** &gt; **パッケージ マネージャーコンソール**メニュー オプション。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-368">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="1d5f3-369">![NuGet パッケージ マネージャー コンソールを開く](whats-new-in-aspnet-mvc-4/_static/image22.png "NuGet パッケージ マネージャー コンソールを開く")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-369">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="1d5f3-370">*NuGet パッケージ マネージャー コンソールを開く*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-370">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="1d5f3-371">パッケージ マネージャー コンソールをインストールするには、次のコマンドを実行、 **jQuery.Mobile.MVC**パッケージ。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-371">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="1d5f3-372">jQuery Mobile は、タッチに最適化された web UI を構築するためのオープン ソース ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-372">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="1d5f3-373">JQuery.Mobile.MVC NuGet パッケージには、ASP.NET MVC 4 アプリケーションで jQuery Mobile を使用するヘルパーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-373">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1d5f3-374">次のコマンドを実行するには Nuget から jQuery.Mobile.MVC ライブラリがダウンロードするされます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-374">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="1d5f3-375">PM</span><span class="sxs-lookup"><span data-stu-id="1d5f3-375">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="1d5f3-376">このコマンドは、jQuery Mobile、および、次のいくつかのヘルパー ファイルがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-376">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="1d5f3-377">**Views/shared/\_Layout.Mobile.cshtml**: 小さな画面用に最適化された jQuery Mobile ベース レイアウトです。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-377">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="1d5f3-378">元のレイアウトが置き換えられ、web サイトでは、モバイル ブラウザーから要求を受信したときに (\_Layout.cshtml) をこのデータセット。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-378">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="1d5f3-379">ビュー スイッチャー コンポーネント: から成る、 **Views/shared/\_ViewSwitcher.cshtml**部分ビューと**ViewSwitcherController.cs**コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-379">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="1d5f3-380">このコンポーネントは、ページのデスクトップ バージョンに切り替えるユーザーを有効にするモバイル ブラウザーでリンクを表示します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-380">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="1d5f3-381">![モバイル デバイスのサポートをフォト ギャラリー プロジェクト](whats-new-in-aspnet-mvc-4/_static/image23.png "フォト ギャラリーのプロジェクトをモバイル デバイスのサポート")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-381">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="1d5f3-382">*フォト ギャラリーのプロジェクトをモバイル デバイスのサポート*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-382">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="1d5f3-383">モバイル バンドルを登録します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-383">Register the Mobile bundles.</span></span> <span data-ttu-id="1d5f3-384">これを行うには、開く、 **Global.asax.cs**ファイルを開き、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-384">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="1d5f3-385">(コード スニペット - *ASP.NET MVC 4 ラボ - Ex03 - 登録モバイル バンドル*)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-385">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. <span data-ttu-id="1d5f3-386">デスクトップの web ブラウザーを使用してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-386">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="1d5f3-387">開く、 **Windows Phone 7 エミュレーター、** 内にある **[スタート] メニュー |すべてのプログラム |Windows Phone SDK 7.1 |Windows Phone エミュレーター。**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-387">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="1d5f3-388">Phone のスタート画面では、Internet Explorer を開きます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-388">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="1d5f3-389">アプリケーションの開始 URL を確認し、スマート フォンのブラウザーでその URL に移動 (例: `http://localhost:[PortNumber]/`)。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-389">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="1d5f3-390">JQuery.Mobile.MVC がモバイル デバイス用に最適化されたビューを表示するプロジェクトに新しい資産を作成すると、アプリケーションが、Windows Phone エミュレーターでは検索が表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-390">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="1d5f3-391">電話、デスクトップ ビューに切り替えるリンクが表示の上部にあるメッセージに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-391">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="1d5f3-392">さらに、  **\_Layout.Mobile.cshtml**がインストールされているパッケージによって作成されたレイアウトには、アプリケーションで別のレイアウトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-392">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1d5f3-393">ここまでは、モバイル ビューに戻るにリンクすることはありません。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-393">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="1d5f3-394">これは、以降のバージョンに含まれます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-394">It will be included in later versions.</span></span>

    <span data-ttu-id="1d5f3-395">![フォト ギャラリーのホーム ページのモバイル ビュー](whats-new-in-aspnet-mvc-4/_static/image24.png "フォト ギャラリーのホーム ページのモバイル ビュー")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-395">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="1d5f3-396">*フォト ギャラリーのホーム ページのモバイル ビュー*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-396">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="1d5f3-397">Visual Studio で、キーを押します**SHIFT** + **f5 キーを押して**アプリケーションのデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-397">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="1d5f3-398">タスク 2 - モバイル ビューの作成</span><span class="sxs-lookup"><span data-stu-id="1d5f3-398">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="1d5f3-399">このタスクでは、モバイル デバイスでより良い appareance に適応させるコンテンツを含む index ビューのモバイル バージョンを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-399">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="1d5f3-400">コピー、 **Views\Home\Index.cshtml**を表示およびコピーを作成、新しいファイルの名前を変更する貼り付けます**Index.Mobile.cshtml**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-400">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="1d5f3-401">開いている、新しく作成された**Index.Mobile.cshtml**表示し、既存&lt;ul&gt;このコードを持つタグ。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-401">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="1d5f3-402">これにより、更新されます。、 &lt;ul&gt;から jQuery モバイルのテーマを使用して jQuery モバイル データの注釈付きタグ。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-402">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > <span data-ttu-id="1d5f3-403">以下の点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-403">Notice that:</span></span>
    > 
    > - <span data-ttu-id="1d5f3-404">**データ ロール**属性に設定**listview** listview のスタイルを使用してリストをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-404">The **data-role** attribute set to **listview** will render the list using the listview styles.</span></span>
    > 
    > - <span data-ttu-id="1d5f3-405">**データ埋め込み**属性が true に設定が丸い境界線と余白の一覧に表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-405">The **data-inset** attribute set to true will show the list with rounded border and margin.</span></span>
    > 
    > - <span data-ttu-id="1d5f3-406">**データ フィルター**属性に設定**true**検索ボックスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-406">The **data-filter** attribute set to **true** will generate a search box.</span></span>
    > 
    > <span data-ttu-id="1d5f3-407">プロジェクト ドキュメントに jQuery モバイル規則の詳細を確認できます。 [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-407">You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span></span>
3. <span data-ttu-id="1d5f3-408">キーを押して**CTRL + S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-408">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="1d5f3-409">切り替えて、 **Windows Phone エミュレーター**と、サイトを更新します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-409">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="1d5f3-410">上にある新しい検索ボックスと同様に、ギャラリーの一覧の新しいルック アンド フィールに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-410">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="1d5f3-411">検索ボックスに単語を入力し、(たとえば、 **Tulips**) フォト ギャラリーで検索を開始します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-411">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="1d5f3-412">![Listview のスタイルをフィルタ リングを使用してギャラリー](whats-new-in-aspnet-mvc-4/_static/image25.png "listview のスタイルを使用してフィルターを使用したギャラリー")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-412">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="1d5f3-413">*Listview のスタイルを使用してフィルターを使用したギャラリー*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-413">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="1d5f3-414">要約すると、Index ビューのコピーを作成するビューというレシピを使用した、&quot;モバイル&quot;サフィックス。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-414">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="1d5f3-415">このサフィックスは ASP.NET MVC 4 モバイル デバイスから生成されたすべての要求がこのインデックスのコピーを使用することを示します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-415">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="1d5f3-416">さらに、jQuery Mobile を使用して、モバイル デバイスでのサイトのルック アンド フィールを強化するために、インデックス ビューのモバイル バージョンに更新されました。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-416">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="1d5f3-417">Visual Studio に戻り、開いている**Site.Mobile.css**の下にある、**コンテンツ**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-417">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="1d5f3-418">イメージの右側に表示する写真のタイトルの位置を修正します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-418">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="1d5f3-419">これを行うには、次のコードを追加、 **Site.Mobile.css**ファイル。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-419">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="1d5f3-420">CSS</span><span class="sxs-lookup"><span data-stu-id="1d5f3-420">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="1d5f3-421">キーを押して**CTRL + S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-421">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="1d5f3-422">戻り、 **Windows Phone エミュレーター**と、サイトを更新します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-422">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="1d5f3-423">写真のタイトルが今すぐに位置付けることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-423">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="1d5f3-424">![イメージの右側に配置されているタイトル](whats-new-in-aspnet-mvc-4/_static/image26.png "イメージの右側に配置されているタイトル")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-424">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="1d5f3-425">*イメージの右側に配置されているタイトル*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-425">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="1d5f3-426">タスク 3 - jQuery Mobile テーマ</span><span class="sxs-lookup"><span data-stu-id="1d5f3-426">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="1d5f3-427">すべてのレイアウトとウィジェットの jQuery Mobile では、新しいオブジェクト指向 CSS フレームワークを中心にサイトおよびアプリケーションに統合されたビジュアル デ ザインを完全なテーマを適用できるように設計されています。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-427">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="1d5f3-428">jQuery モバイル デバイスの既定テーマにはには、文字が与えられている 5 つの見本が含まれています。 (a、b、c、d、e) のクイック リファレンス。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-428">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="1d5f3-429">このタスクで、既定よりも、さまざまなテーマを使用するモバイル レイアウトが更新されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-429">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="1d5f3-430">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-430">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="1d5f3-431">開く、  **\_Layout.Mobile.cshtml**にあるファイル**views \shared**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-431">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="1d5f3-432">設定データ ロールを持つ div 要素を検索&quot;ページ&quot;し、更新、**データ テーマ**に&quot; **e**&quot;します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-432">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. <span data-ttu-id="1d5f3-433">キーを押して**CTRL + S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-433">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="1d5f3-434">サイトを更新、 **Windows Phone エミュレーター**に新しい色スキームを注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-434">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="1d5f3-435">![別の配色でモバイル レイアウト](whats-new-in-aspnet-mvc-4/_static/image27.png "別の配色とモバイルのレイアウト")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-435">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="1d5f3-436">*別の配色とモバイルのレイアウト*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-436">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="1d5f3-437">タスク 4 - ビュー スイッチャー コンポーネントと機能をオーバーライドするブラウザーを使用して</span><span class="sxs-lookup"><span data-stu-id="1d5f3-437">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="1d5f3-438">モバイルに最適化された web ページ用の規則では、デスクトップ ビューなど、ページのデスクトップ バージョンに切り替えることができますサイトの完全なモードはテキストがリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-438">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="1d5f3-439">JQuery.Mobile.MVC パッケージには、サンプルが含まれています。**ビュー スイッチャー**この目的で使用されるコンポーネント、  **\_Layout.Mobile.cshtml**ビュー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-439">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="1d5f3-440">![デスクトップ ビューに切り替えるリンク](whats-new-in-aspnet-mvc-4/_static/image28.png "デスクトップ ビューに切り替えますへのリンク")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-440">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="1d5f3-441">*デスクトップ ビューに切り替えるリンクします。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-441">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="1d5f3-442">ビュー スイッチャーと呼ばれる新しい機能を使用して**ブラウザー オーバーライド**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-442">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="1d5f3-443">この機能は、アプリケーションで別のブラウザー (ユーザー エージェント) から実際に送信されているものから、要求を処理できます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-443">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="1d5f3-444">このタスクでは、jQuery.Mobile.MVC と ASP.NET MVC 4 の機能をオーバーライドする、新しいブラウザーによって追加されたビュー スイッチャーの実装例を説明します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-444">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="1d5f3-445">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-445">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="1d5f3-446">開く、  **\_Layout.Mobile.cshtml**ビューの下にある、 **views \shared**フォルダーと部分ビューとして参照されているビュー スイッチャー コンポーネントに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-446">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="1d5f3-447">![ビュー スイッチャーのコンポーネントを使用して、モバイル レイアウト](whats-new-in-aspnet-mvc-4/_static/image29.png "ビュー スイッチャーのコンポーネントを使用して、モバイル レイアウト")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-447">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="1d5f3-448">*ビュー スイッチャーのコンポーネントを使用して、モバイル レイアウト*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-448">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="1d5f3-449">開く、  **\_ViewSwitcher.cshtml**部分ビュー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-449">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="1d5f3-450">部分ビューは、新しいメソッドを使用して**ViewContext.HttpContext.GetOverriddenBrowser()** web 要求の起点を特定して、デスクトップまたはモバイル ビューのいずれかを切り替えるに対応するリンクを表示します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-450">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="1d5f3-451">**GetOverridenBrowser**メソッドを返します、 **HttpBrowserCapabilitiesBase**要求に設定されているユーザー エージェントに対応するインスタンス (実際またはオーバーライド) します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-451">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="1d5f3-452">この値を使用して、プロパティを取得します。 **IsMobileDevice**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-452">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="1d5f3-453">![ViewSwitcher 部分ビュー](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher 部分ビュー")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-453">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="1d5f3-454">*ViewSwitcher 部分ビュー*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-454">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="1d5f3-455">開く、 **ViewSwitcherController.cs**クラス、**コント ローラー**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-455">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="1d5f3-456">チェック アウトする SwitchView アクションは ViewSwitcher コンポーネントでリンクによって呼び出され、HttpContext の新しいメソッドに注目してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-456">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="1d5f3-457">**HttpContext.ClearOverridenBrowser()** メソッドは、現在の要求でオーバーライドされたユーザー エージェントを削除します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-457">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="1d5f3-458">**HttpContext.SetOverridenBrowser()** メソッドは、指定されたユーザー エージェントを使用して、要求の実際のユーザー エージェント値をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-458">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="1d5f3-459">![ViewSwitcher コント ローラー](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher コント ローラー")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-459">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="1d5f3-460">*ViewSwitcher コント ローラー*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-460">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="1d5f3-461">ブラウザーのオーバーライドは ASP.NET MVC 4、コア機能も使用可能な場合でも、jQuery.Mobile.MVC パッケージをインストールしないでください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-461">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="1d5f3-462">ただし、この機能は、ビュー、レイアウト、および部分ビュー、のみに影響して、Request.Browser オブジェクトに依存する機能は影響しません。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-462">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="1d5f3-463">タスク 5 - デスクトップ ビューにビュー切り替えを追加します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-463">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="1d5f3-464">このタスクでデスクトップ レイアウトに含めるビュー スイッチャーが更新されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-464">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="1d5f3-465">これによって、モバイル ユーザー デスクトップのビューを参照するときに、モバイル ビューに戻ります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-465">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="1d5f3-466">サイトを更新、 **Windows Phone エミュレーター**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-466">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="1d5f3-467">をクリックして、**デスクトップ ビュー**ギャラリーの上部にあるリンクです。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-467">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="1d5f3-468">モバイル ビューに戻ることを許可するデスクトップ ビューにビュー切り替えがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-468">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="1d5f3-469">Visual Studio に戻り、開く、  **\_Layout.cshtml**ビュー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-469">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="1d5f3-470">ログイン セクションを見つけて表示するために呼び出しを挿入、  **\_ViewSwitcher**下の部分ビュー、  **\_LogOnPartial**部分ビュー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-470">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="1d5f3-471">次に、キーを押して**CTRL + S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-471">Then, press **CTRL + S** to save the changes.</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. <span data-ttu-id="1d5f3-472">キーを押して**CTRL + S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-472">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="1d5f3-473">Windows Phone エミュレーターでページを更新し、拡大する画面をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-473">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="1d5f3-474">ホーム ページが表示されますが、**モバイル ビュー**モバイルからデスクトップ ビューに切り替えるリンク。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-474">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="1d5f3-475">![デスクトップ ビューに表示の切り替えを表示](whats-new-in-aspnet-mvc-4/_static/image32.png "デスクトップ ビューにレンダリングされるビュー スイッチャー")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-475">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="1d5f3-476">*デスクトップ ビューにレンダリングされるビュー スイッチャー*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-476">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="1d5f3-477">もう一度モバイル ビューに切り替えるしを参照<strong>に関する</strong>ページ (http://localhost[port]/Home/About)。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-477">Switch to the Mobile view again and browse to <strong>About</strong> page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="1d5f3-478">、、About.Mobile.cshtml ビューを作成していない場合でも、[About] ページが表示されます、モバイル レイアウトを使用して (\_Layout.Mobile.cshtml)。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-478">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="1d5f3-479">![ページについて](whats-new-in-aspnet-mvc-4/_static/image33.png "ページについて")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-479">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="1d5f3-480">*ページについて*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-480">*About page*</span></span>
8. <span data-ttu-id="1d5f3-481">最後に、デスクトップ、Web ブラウザーでサイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-481">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="1d5f3-482">デスクトップ ビューに影響なし以前の更新プログラムのことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-482">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="1d5f3-483">![PhotoGallery デスクトップ ビュー](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery デスクトップ ビュー")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-483">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="1d5f3-484">*PhotoGallery デスクトップ ビュー*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-484">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="1d5f3-485">タスク 6 - 新しい表示モードの作成</span><span class="sxs-lookup"><span data-stu-id="1d5f3-485">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="1d5f3-486">新しい表示モード機能には、要求を生成しているブラウザーによってビューを選択して、アプリケーションことができます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-486">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="1d5f3-487">たとえば、デスクトップ ブラウザーでは、ホーム ページを要求している場合、アプリケーションが返されます、 **Views\Home\Index.cshtml**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-487">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="1d5f3-488">モバイル ブラウザーでは、ホーム ページを要求している場合、アプリケーションが返されます、 **Views\Home\Index.mobile.cshtml**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-488">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="1d5f3-489">このタスクでは iPhone デバイスでは、カスタマイズされたレイアウトを作成して iPhone デバイスからの要求をシミュレートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-489">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="1d5f3-490">これを行うには、いずれかを iPhone エミュレーターまたはシミュレーターを使用することができます (など[電気 Mobile シミュレーター](http://www.electricplum.com/)) またはユーザー エージェントを変更するアドオンを使用したブラウザー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-490">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="1d5f3-491">IPhone をエミュレートするために、Safari ブラウザーでユーザー エージェント文字列を設定する方法については、次を参照してください。 [Safari pretend IE ができるようにする方法](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html)David Alison のブログにします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-491">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="1d5f3-492">**このタスクは省略可能なことと、実行せず、ラボで続行することができますに注意してください。**</span><span class="sxs-lookup"><span data-stu-id="1d5f3-492">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="1d5f3-493">Visual Studio で、キーを押します**SHIFT** + **f5 キーを押して**アプリケーションのデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-493">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="1d5f3-494">開いている**Global.asax.cs**し、以下の追加ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-494">Open **Global.asax.cs** and add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. <span data-ttu-id="1d5f3-495">次の強調表示されたコードをアプリケーションに追加\_メソッドを開始します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-495">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="1d5f3-496">(コード スニペット - *ASP.NET MVC 4 ラボ - Ex03 - iPhone DisplayMode*)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-496">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

<span data-ttu-id="1d5f3-497">新しい登録が**DefaultDisplayMode という&quot;iPhone&quot;**、静的な内で**DisplayModeProvider.Instance.Modes**静的リストに対して照合されます。各受信要求。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-497">You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request.</span></span> <span data-ttu-id="1d5f3-498">受信要求には、文字列が含まれている場合&quot;iPhone&quot;、ASP.NET MVC は名前を含むビューを検索、 &quot;iPhone&quot;サフィックス。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-498">If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix.</span></span> <span data-ttu-id="1d5f3-499">パラメーターに 0 を特定する方法は、新しいモードを示しますたとえば、このビューは、一般的なよりも具体的&quot;.mobile&quot;モバイル デバイスからの要求と一致するルール。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-499">The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.</span></span>

<span data-ttu-id="1d5f3-500">このコードの実行、iPhone ブラウザーが要求を生成するときに、後に、アプリケーションで使用、 **views \shared\\_Layout.iPhone.cshtml**レイアウトは次の手順で作成されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-500">After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.</span></span>

> [!NOTE]
> <span data-ttu-id="1d5f3-501">この方法で iPhone のデモの目的で簡略化された、(テストの例では大文字小文字を区別) のすべての iPhone ユーザー エージェント文字列について想定どおり動作しない可能性があります、要求をテストします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-501">This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).</span></span>

4. <span data-ttu-id="1d5f3-502">コピーを作成、  **\_Layout.Mobile.cshtml**ファイル、 **views \shared**フォルダーへのコピーの名前を変更および&quot;  **\_Layout.iPhone.csthml**&quot;.</span><span class="sxs-lookup"><span data-stu-id="1d5f3-502">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="1d5f3-503">開いている **\_Layout.iPhone.csthml**前の手順で作成しました。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-503">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="1d5f3-504">データ ロールの属性を設定すると div 要素を検索**ページ**を変更して、**データ テーマ**属性を&quot; **、**&quot;します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-504">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

<span data-ttu-id="1d5f3-505">これで、ASP.NET MVC 4 アプリケーションで 3 つのレイアウトがあります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-505">Now you have 3 layouts in your ASP.NET MVC 4 application:</span></span>

1. <span data-ttu-id="1d5f3-506">**\_Layout.cshtml**: 既定のレイアウトがデスクトップ ブラウザーを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-506">**\_Layout.cshtml**: default layout used for desktop browsers.</span></span>
2. <span data-ttu-id="1d5f3-507">**\_Layout.mobile.cshtml**: モバイル デバイスを使用する既定のレイアウト。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-507">**\_Layout.mobile.cshtml**: default layout used for mobile devices.</span></span>
3. <span data-ttu-id="1d5f3-508">**\_Layout.iPhone.cshtml**: から区別するために別の配色を使用して、iPhone デバイス用の特定のレイアウト\_Layout.mobile.cshtml します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-508">**\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.</span></span>
7. <span data-ttu-id="1d5f3-509">キーを押して**F5**アプリケーションを実行し、サイトを参照する、 **Windows Phone エミュレーター**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-509">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="1d5f3-510">開く、 **iPhone シミュレーター** (を参照してください[付録 C](#AppendixC)インストールして、iPhone シミュレーターを構成する方法の詳細について)、しすぎて、サイトを参照します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-510">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="1d5f3-511">各電話番号が、特定のテンプレートを使用することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-511">Notice that each phone is using the specific template.</span></span>

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="1d5f3-513">*各モバイル デバイスのさまざまなビューを使用します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-513">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="1d5f3-514">手順 4: 非同期コント ローラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-514">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="1d5f3-515">Microsoft .NET Framework 4.5 では、新しい基盤の .NET プログラミングでの非同期性を提供する c# および Visual Basic の新しい言語機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-515">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="1d5f3-516">この新しい基盤は、-のようなと約同期プログラミングと同程度に簡単に非同期プログラミングになります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-516">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="1d5f3-517">使用して ASP.NET MVC 4 での非同期アクション メソッドを記述することができるよう、 **AsyncController**クラス。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-517">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="1d5f3-518">実行時間の長い非同期アクション メソッドを使用する、非 CPU バインド要求です。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-518">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="1d5f3-519">要求の処理中に作業を実行してから、Web サーバーのブロックを回避できます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-519">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="1d5f3-520">AsyncController クラスは、通常の実行時間の長い Web サービス呼び出しに使用されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-520">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="1d5f3-521">この演習では、ASP.NET MVC 4 での非同期操作の基本について説明します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-521">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="1d5f3-522">詳細を紹介する場合は、次の記事を確認できます。 [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-522">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="1d5f3-523">タスク 1 - 非同期コント ローラーを実装します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-523">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="1d5f3-524">開く、**開始**ソリューションがある**ソース/Ex4-非同期/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-524">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="1d5f3-525">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-525">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="1d5f3-526">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-526">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="1d5f3-527">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-527">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="1d5f3-528">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-528">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="1d5f3-529">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-529">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="1d5f3-530">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-530">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="1d5f3-531">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-531">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="1d5f3-532">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-532">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="1d5f3-533">開く、 **HomeController.cs**クラスから、**コント ローラー**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-533">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="1d5f3-534">次の追加ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-534">Add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. <span data-ttu-id="1d5f3-535">更新プログラム、 **HomeController**クラスから継承する**AsyncController**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-535">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="1d5f3-536">AsyncController から派生したコント ローラーが非同期の要求を処理する ASP.NET を有効にして、サービス同期アクション メソッドは、できます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-536">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. <span data-ttu-id="1d5f3-537">追加、 **async**キーワードを**インデックス**メソッドと型を返すように**タスク&lt;ActionResult&gt;** します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-537">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > <span data-ttu-id="1d5f3-538">**Async**キーワードは、.NET Framework 4.5 は、新しいキーワードの 1 つは、このメソッドが非同期コードを含むことをコンパイラに伝えます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-538">The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code.</span></span> <span data-ttu-id="1d5f3-539">A**タスク**オブジェクトは、ある時点で、今後が完了する非同期操作を表します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-539">A **Task** object represents an asynchronous operation that may complete at some point in the future.</span></span>
6. <span data-ttu-id="1d5f3-540">置換、**クライアント。GetAsync()** 次に示す await キーワードを使用して、完全な非同期バージョンの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-540">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="1d5f3-541">(コード スニペット - *ASP.NET MVC 4 ラボ - Ex04 - GetAsync*)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-541">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="1d5f3-542">以前のバージョンで使用していた、**結果**プロパティから、**タスク**結果には、(同期バージョン) が返されるまでスレッドをブロックするオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-542">In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).</span></span>
    > 
    > <span data-ttu-id="1d5f3-543">追加、 **await**キーワード、メソッドの呼び出しから返されるタスクを非同期に待機コンパイラに指示します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-543">Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call.</span></span> <span data-ttu-id="1d5f3-544">これは待機中のメソッドが完了した後にのみ、コードの残りの部分は、コールバックとして実行されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-544">This means that the rest of the code will be executed as a callback only after the awaited method completes.</span></span> <span data-ttu-id="1d5f3-545">もう 1 つ気付くはこの作業を行うためには、try-catch ブロックを変更する必要はありません: フォア グラウンドまたはバック グラウンドで発生する例外は、フレームワークによって提供されるハンドラーを使用して、追加作業なし引き続きキャッチできます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-545">Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.</span></span>
7. <span data-ttu-id="1d5f3-546">次に示すように、新しいコードで行を置き換えることで、非同期の実装を続行するコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-546">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="1d5f3-547">(コード スニペット - *ASP.NET MVC 4 ラボ - Ex04 - ReadAsStringAsync*)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-547">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. <span data-ttu-id="1d5f3-548">アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-548">Run the application.</span></span> <span data-ttu-id="1d5f3-549">わかりますなし、主要な変更が、コードでは、サーバー リソースの使用率の向上を行うと、パフォーマンスの向上、スレッド プールのスレッドはブロックされません。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-549">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1d5f3-550">ラボで新しい非同期プログラミング機能の詳細を確認できます&quot; **c# および Visual Basic を使用して .NET 4.5 での非同期プログラミング**&quot; Visual Studio のトレーニング キットに含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-550">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="1d5f3-551">タスク 2 - キャンセル トークンを使用した処理のタイムアウト</span><span class="sxs-lookup"><span data-stu-id="1d5f3-551">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="1d5f3-552">タスクのインスタンスを返す非同期アクション メソッドには、タイムアウトもサポートできます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-552">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="1d5f3-553">このタスクでは、キャンセル トークンを使用してタイムアウトのシナリオを処理するためにインデックス メソッドのコードを更新します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-553">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="1d5f3-554">Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-554">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="1d5f3-555">以下を追加するステートメントを使用して、 **HomeController.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-555">Add the following using statement to the **HomeController.cs** file.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. <span data-ttu-id="1d5f3-556">更新を受信するインデックス アクション、 **CancellationToken**引数。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-556">Update the Index action to receive a **CancellationToken** argument.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. <span data-ttu-id="1d5f3-557">更新プログラム、 **GetAsync**呼び出しキャンセル トークンを渡す。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-557">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="1d5f3-558">(コード スニペット - *CancellationToken を ASP.NET MVC 4 ラボ - Ex04 - SendAsync*)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-558">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. <span data-ttu-id="1d5f3-559">装飾、*インデックス*メソッドを**AsyncTimeout**属性が 500 ミリ秒に設定し、 **HandleError**処理するために構成されている属性**TaskCanceledException**リダイレクトすることで、 **TimedOut**ビュー。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-559">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="1d5f3-560">(コード スニペット -*ラボ - Ex04 - 属性の ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="1d5f3-560">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. <span data-ttu-id="1d5f3-561">開く、 **PhotoController**クラスと更新プログラム、**ギャラリー**実行時間の長いタスクをシミュレートするために実行 1000 ミリ秒 (1 秒) を遅延するメソッド。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-561">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. <span data-ttu-id="1d5f3-562">開く、 **Web.config**ファイルを開き、次の要素を追加してカスタム エラーを有効にします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-562">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. <span data-ttu-id="1d5f3-563">新しいビューを作成**views \shared**という**TimedOut**および既定のレイアウトを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-563">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="1d5f3-564">ソリューション エクスプ ローラーで右クリックし、 **views \shared**フォルダーと選択**追加 |ビュー**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-564">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="1d5f3-565">![各モバイル デバイスのさまざまなビューを使用して](whats-new-in-aspnet-mvc-4/_static/image36.png "各モバイル デバイスのさまざまなビューを使用します。")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-565">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="1d5f3-566">*各モバイル デバイスのさまざまなビューを使用します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-566">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="1d5f3-567">更新プログラム、 **TimedOut**次に示すようにコンテンツを表示します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-567">Update the **TimedOut** view content as shown below.</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. <span data-ttu-id="1d5f3-568">アプリケーションを実行し、ルート URL に移動します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-568">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="1d5f3-569">追加したときに、 **Thread.Sleep** 1000 ミリ秒間に、によって生成された、タイムアウト エラーが表示されます、 **AsyncTimeout**属性し catch、 **HandleError**属性。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-569">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="1d5f3-570">![処理のタイムアウト例外](whats-new-in-aspnet-mvc-4/_static/image37.png "処理のタイムアウト例外")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-570">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="1d5f3-571">*処理のタイムアウト例外*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-571">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="1d5f3-572">また、Windows Azure Web サイトの次に、このアプリケーションを展開できます[付録 d: 発行 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixD)します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-572">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="1d5f3-573">まとめ</span><span class="sxs-lookup"><span data-stu-id="1d5f3-573">Summary</span></span>

<span data-ttu-id="1d5f3-574">このハンズオン ラボでを見いくつかの新機能の ASP.NET MVC 4 に常駐しています。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-574">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="1d5f3-575">次の概念を説明しています。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-575">The following concepts have been discussed:</span></span>

- <span data-ttu-id="1d5f3-576">ASP.NET MVC プロジェクト テンプレートなど、新しいモバイル アプリケーション プロジェクト テンプレートを機能強化を活用します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-576">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="1d5f3-577">HTML5 ビューポート属性と CSS メディア クエリを使用して、モバイル デバイスの表示を改善するには</span><span class="sxs-lookup"><span data-stu-id="1d5f3-577">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="1d5f3-578">プログレッシブの機能強化と、タッチに最適化された web UI を構築するために、jQuery Mobile を使用してください。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-578">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="1d5f3-579">モバイル専用ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-579">Create mobile-specific views</span></span>
- <span data-ttu-id="1d5f3-580">ビュー スイッチャー コンポーネントを使用して、アプリケーションでモバイルおよびデスクトップのビューを切り替える</span><span class="sxs-lookup"><span data-stu-id="1d5f3-580">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="1d5f3-581">タスクのサポートを使用して非同期コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-581">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="1d5f3-582">付録 a: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="1d5f3-582">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="1d5f3-583">コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-583">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="1d5f3-584">ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-584">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="1d5f3-585">![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](whats-new-in-aspnet-mvc-4/_static/image38.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-585">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="1d5f3-586">*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-586">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="1d5f3-587">***キーボード (c# のみ) を使用するコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="1d5f3-587">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="1d5f3-588">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-588">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="1d5f3-589">スニペットの名前 (なし、スペースやハイフン) の入力を開始します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-589">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="1d5f3-590">スニペットの名前に一致する IntelliSense の表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-590">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="1d5f3-591">適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-591">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="1d5f3-592">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-592">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="1d5f3-593">![スニペットの名前の入力を開始](whats-new-in-aspnet-mvc-4/_static/image39.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-593">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="1d5f3-594">*スニペットの名前の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-594">*Start typing the snippet name*</span></span>

<span data-ttu-id="1d5f3-595">![強調表示されているスニペットを選択して Tab キーを押して](whats-new-in-aspnet-mvc-4/_static/image40.png "キーを押してタブが強調表示されているスニペットを選択するには")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-595">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="1d5f3-596">*Tab キーを押して、強調表示されているスニペットを選択します*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-596">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="1d5f3-597">![キーを押して タブで再度とスニペットが展開](whats-new-in-aspnet-mvc-4/_static/image41.png "キーを押して タブで再度とスニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-597">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="1d5f3-598">*キーを押して タブで再度とスニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-598">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="1d5f3-599">***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="1d5f3-599">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="1d5f3-600">コード スニペットを挿入するを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-600">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="1d5f3-601">選択**スニペットの挿入**続けて**マイ コード スニペット**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-601">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="1d5f3-602">クリックして、一覧から関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-602">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="1d5f3-603">![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](whats-new-in-aspnet-mvc-4/_static/image42.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-603">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="1d5f3-604">*コード スニペットを挿入して、スニペットの挿入先の選択します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-604">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="1d5f3-605">![クリックして、一覧から関連するスニペットを選択](whats-new-in-aspnet-mvc-4/_static/image43.png "クリックして、一覧から関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-605">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="1d5f3-606">*クリックして、一覧から関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-606">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="1d5f3-607">付録 b: Installing Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="1d5f3-607">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="1d5f3-608">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="1d5f3-608">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="1d5f3-609">次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-609">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="1d5f3-610">移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-610">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="1d5f3-611">または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-611">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="1d5f3-612">をクリックして**を今すぐインストール**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-612">Click on **Install Now**.</span></span> <span data-ttu-id="1d5f3-613">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-613">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="1d5f3-614">1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-614">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="1d5f3-615">![Visual Studio Express のインストール](whats-new-in-aspnet-mvc-4/_static/image44.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-615">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="1d5f3-616">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-616">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="1d5f3-617">すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-617">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="1d5f3-619">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-619">*Accepting the license terms*</span></span>
5. <span data-ttu-id="1d5f3-620">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-620">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="1d5f3-622">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-622">*Installation progress*</span></span>
6. <span data-ttu-id="1d5f3-623">インストールが完了したら、クリックして**完了**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-623">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="1d5f3-625">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-625">*Installation completed*</span></span>
7. <span data-ttu-id="1d5f3-626">クリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-626">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="1d5f3-627">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-627">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web のタイル](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="1d5f3-629">*VS Express for Web のタイル*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-629">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="1d5f3-630">付録 c: Installing WebMatrix 2 および iPhone シミュレーター</span><span class="sxs-lookup"><span data-stu-id="1d5f3-630">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="1d5f3-631">IPhone のシミュレートされたデバイスで、サイトを実行するには、WebMatrix の拡張機能を使用することができます&quot;iPhone 用の電気の Mobile シミュレーター&quot;します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-631">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="1d5f3-632">また、Visual Studio 2012 から、シミュレーターを実行する同じ拡張機能を構成できます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-632">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="1d5f3-633">タスク 1 - 2 WebMatrix のインストール</span><span class="sxs-lookup"><span data-stu-id="1d5f3-633">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="1d5f3-634">移動して[ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169)します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-634">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="1d5f3-635">または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>WebMatrix 2</em>&quot;します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-635">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>WebMatrix 2</em>&quot;.</span></span>
2. <span data-ttu-id="1d5f3-636">をクリックして**を今すぐインストール**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-636">Click on **Install Now**.</span></span> <span data-ttu-id="1d5f3-637">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-637">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="1d5f3-638">1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-638">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="1d5f3-639">![WebMatrix 2 のインストール](whats-new-in-aspnet-mvc-4/_static/image49.png "WebMatrix 2 のインストール")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-639">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="1d5f3-640">*WebMatrix 2 をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-640">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="1d5f3-641">すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-641">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="1d5f3-642">![ライセンス条項に同意](whats-new-in-aspnet-mvc-4/_static/image50.png "ライセンス条項に同意")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-642">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="1d5f3-643">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-643">*Accepting the license terms*</span></span>
5. <span data-ttu-id="1d5f3-644">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-644">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="1d5f3-645">![インストールの進行状況](whats-new-in-aspnet-mvc-4/_static/image51.png "インストールの進行状況")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-645">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="1d5f3-646">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-646">*Installation progress*</span></span>
6. <span data-ttu-id="1d5f3-647">インストールが完了したら、クリックして**完了**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-647">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="1d5f3-648">![インストールが完了した](whats-new-in-aspnet-mvc-4/_static/image52.png "インストールが完了しました")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-648">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="1d5f3-649">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-649">*Installation completed*</span></span>
7. <span data-ttu-id="1d5f3-650">クリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-650">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="1d5f3-651">タスク 2 - iPhone シミュレーターの拡張機能をインストールします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-651">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="1d5f3-652">実行**WebMatrix**と既存の Web サイトを開くか、新しく作成します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-652">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="1d5f3-653">をクリックして、**実行**からボタン、**ホーム**のリボンし、選択**新規追加**。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-653">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="1d5f3-654">![WebMatrix の新しい拡張機能の追加](whats-new-in-aspnet-mvc-4/_static/image53.png "WebMatrix の新しい拡張機能の追加")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-654">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="1d5f3-655">*WebMatrix の新しい拡張機能の追加*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-655">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="1d5f3-656">選択**iPhone シミュレーター**クリック**インストール**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-656">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="1d5f3-657">![WebMatrix 拡張機能の参照](whats-new-in-aspnet-mvc-4/_static/image54.png "参照 WebMatrix 拡張機能")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-657">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="1d5f3-658">*WebMatrix 拡張機能の参照*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-658">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="1d5f3-659">パッケージの詳細で次のようにクリックします。**インストール**拡張機能のインストールを続行します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-659">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="1d5f3-660">![iPhone シミュレーター拡張子](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone シミュレーターの拡張機能")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-660">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="1d5f3-661">*iPhone シミュレーターの拡張機能*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-661">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="1d5f3-662">読み、拡張機能の使用許諾契約書に同意します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-662">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="1d5f3-663">![WebMatrix 拡張機能の使用許諾契約書](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix 拡張機能の使用許諾契約書")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-663">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="1d5f3-664">*WebMatrix 拡張機能の使用許諾契約書*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-664">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="1d5f3-665">ここで、iPhone シミュレーター オプションを使用して WebMatrix から Web サイトを実行できます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-665">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="1d5f3-666">![IPhone を使用して実行](whats-new-in-aspnet-mvc-4/_static/image57.png "iPhone を使用して実行")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-666">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="1d5f3-667">*IPhone を使用して実行します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-667">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="1d5f3-668">タスク 3 - iPhone シミュレーターを実行する Visual Studio 2012 の構成</span><span class="sxs-lookup"><span data-stu-id="1d5f3-668">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="1d5f3-669">開く**Visual Studio 2012**と任意の Web サイトを開くか、新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-669">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="1d5f3-670">[実行] ボタンの下矢印をクリックして選択します**参照**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-670">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="1d5f3-671">![参照](whats-new-in-aspnet-mvc-4/_static/image58.png "と参照")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-671">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="1d5f3-672">*[参照] します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-672">*Browse with*</span></span>
3. <span data-ttu-id="1d5f3-673">&quot;ブラウザー&quot;ダイアログ ボックスで、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-673">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="1d5f3-674">&quot;プログラムの追加&quot;ダイアログ ボックスで、次の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-674">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

   - <span data-ttu-id="1d5f3-675"><strong>プログラム</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (パスを適宜更新する)</em></span><span class="sxs-lookup"><span data-stu-id="1d5f3-675"><strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)</em></span></span>
   - <span data-ttu-id="1d5f3-676">**引数**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="1d5f3-676">**Arguments**: &quot;1&quot;</span></span>
   - <span data-ttu-id="1d5f3-677">**フレンドリ名**: iPhone シミュレーター</span><span class="sxs-lookup"><span data-stu-id="1d5f3-677">**Friendly name**: iPhone Simulator</span></span>

     <span data-ttu-id="1d5f3-678">![プログラムの追加](whats-new-in-aspnet-mvc-4/_static/image59.png "プログラムの追加")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-678">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

     <span data-ttu-id="1d5f3-679">*使用して参照するプログラムを追加します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-679">*Add program to browse with*</span></span>
5. <span data-ttu-id="1d5f3-680">クリックして**OK**し、ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-680">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="1d5f3-681">Visual Studio 2012 から iPhone シミュレーターで、Web アプリケーションを実行できるように整いました。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-681">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="1d5f3-682">![IPhone シミュレーターで参照](whats-new-in-aspnet-mvc-4/_static/image60.png "iPhone シミュレーターでの参照")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-682">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="1d5f3-683">*IPhone シミュレーターでの参照します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-683">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="1d5f3-684">付録 d: Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="1d5f3-684">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="1d5f3-685">この付録では、Windows Azure 管理ポータルから新しい web サイトを作成して Windows Azure によって提供される、Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを発行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-685">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="1d5f3-686">タスク 1 - 新しい Web サイトを作成する、Windows から Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1d5f3-686">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="1d5f3-687">移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-687">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1d5f3-688">Windows Azure を無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増加に応じてスケールできます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-688">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="1d5f3-689">サインアップする[ここ](http://aka.ms/aspnet-hol-azure)します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-689">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="1d5f3-690">![Windows Azure ポータルにログオン](whats-new-in-aspnet-mvc-4/_static/image61.png "Windows Azure ポータルにログオン")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-690">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="1d5f3-691">*Windows Azure 管理ポータルにログオン*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-691">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="1d5f3-692">クリックして**新規**コマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-692">Click **New** on the command bar.</span></span>

    <span data-ttu-id="1d5f3-693">![新しい Web サイトを作成する](whats-new-in-aspnet-mvc-4/_static/image62.png "新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-693">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="1d5f3-694">*新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-694">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="1d5f3-695">クリックして**コンピューティング** | **Web サイト**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-695">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="1d5f3-696">選び**簡易作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-696">Then select **Quick Create** option.</span></span> <span data-ttu-id="1d5f3-697">新しい web サイトの URL を指定し、をクリックして**Web サイトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-697">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1d5f3-698">Windows Azure の Web サイトには、制御および管理できる、クラウドで実行されている web アプリケーション、ホストです。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-698">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="1d5f3-699">簡易作成 オプションを使用すると、完成した web アプリケーションを Windows Azure の Web サイトから、ポータル外からデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-699">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="1d5f3-700">データベースを設定するための手順は含まれません。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-700">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="1d5f3-701">![簡易作成を使用して新しい Web サイトを作成する](whats-new-in-aspnet-mvc-4/_static/image63.png "簡易作成を使用して新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-701">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="1d5f3-702">*簡易作成を使用して新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-702">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="1d5f3-703">新しいまで待つ**Web サイト**が作成されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-703">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="1d5f3-704">Web サイトを作成した後は、下のリンクをクリックして、 **URL**列。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-704">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="1d5f3-705">新しい Web サイトが動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-705">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="1d5f3-706">![新しい web サイトを参照](whats-new-in-aspnet-mvc-4/_static/image64.png "新しい web サイトを参照")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-706">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="1d5f3-707">*新しい web サイトを参照*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-707">*Browsing to the new web site*</span></span>

    <span data-ttu-id="1d5f3-708">![Web サイトが実行されている](whats-new-in-aspnet-mvc-4/_static/image65.png "実行している Web サイト")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-708">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="1d5f3-709">*実行している web サイト*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-709">*Web site running*</span></span>
6. <span data-ttu-id="1d5f3-710">ポータルに戻り、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-710">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="1d5f3-711">![Web サイトの管理ページを開く](whats-new-in-aspnet-mvc-4/_static/image66.png "web サイトの管理ページを開く")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-711">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="1d5f3-712">*Web サイトの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-712">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="1d5f3-713">**ダッシュ ボード**ページの**概要**セクションで、、**発行プロファイルのダウンロード**リンク。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-713">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1d5f3-714">*発行プロファイル*すべてのパブリケーションを有効になっているメソッドごとに Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-714">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="1d5f3-715">発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベースの文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-715">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="1d5f3-716">**Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートと、発行プロファイルのこれらのプログラムの構成を自動化するにはWindows Azure の web サイトへの web アプリケーションを発行しています。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-716">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="1d5f3-717">![発行プロファイルのダウンロード web サイト](whats-new-in-aspnet-mvc-4/_static/image67.png "発行プロファイルの web サイトのダウンロード")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-717">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="1d5f3-718">*発行プロファイルの Web サイトのダウンロード*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-718">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="1d5f3-719">既知の場所に発行プロファイル ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-719">Download the publish profile file to a known location.</span></span> <span data-ttu-id="1d5f3-720">さらにこの演習ではこのファイルを使用して、Visual Studio から Windows Azure Web サイトに web アプリケーションを発行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-720">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="1d5f3-721">![発行プロファイル ファイルを保存する](whats-new-in-aspnet-mvc-4/_static/image68.png "発行プロファイルを保存しています")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-721">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="1d5f3-722">*発行プロファイル ファイルを保存しています*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-722">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="1d5f3-723">タスク 2 - データベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="1d5f3-723">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="1d5f3-724">アプリケーションが SQL Server を使用する場合のデータベースは SQL Database サーバーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-724">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="1d5f3-725">SQL Server を使用しない単純なアプリケーションをデプロイする場合は、このタスクをスキップする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-725">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="1d5f3-726">SQL Database サーバーは、アプリケーション データベースを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-726">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="1d5f3-727">Windows Azure 管理ポータル内のサブスクリプションから SQL Database サーバーを表示する**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-727">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="1d5f3-728">使用して 1 つを作成するには作成したサーバーがない、**追加**コマンド バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-728">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="1d5f3-729">メモ、**サーバー名および URL、管理者のログイン名とパスワード**次のタスクで使用すると、します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-729">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="1d5f3-730">データベースを作成しない、まだ、後の段階でそれが作成されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-730">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="1d5f3-731">![SQL データベース サーバーのダッシュ ボード](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL データベース サーバーのダッシュ ボード")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-731">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="1d5f3-732">*SQL データベース サーバーのダッシュ ボード*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-732">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="1d5f3-733">次のタスクでは、サーバーの一覧で、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-733">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="1d5f3-734">次のようにクリックします**構成**、から IP アドレスを選択して**現在のクライアント IP アドレス**で貼り付けます、**開始 IP アドレス**と**終了 IP アドレス。** テキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png)ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-734">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![クライアント IP アドレスを追加します。](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="1d5f3-736">*クライアント IP アドレスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-736">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="1d5f3-737">1 回、**クライアント IP アドレス**が許可された IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-737">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![変更を確認します。](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="1d5f3-739">*変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-739">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="1d5f3-740">タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="1d5f3-740">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="1d5f3-741">ASP.NET MVC 4 のソリューションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-741">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="1d5f3-742">**ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-742">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="1d5f3-743">![アプリケーションの発行](whats-new-in-aspnet-mvc-4/_static/image73.png "アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-743">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="1d5f3-744">*Web サイトの発行*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-744">*Publishing the web site*</span></span>
2. <span data-ttu-id="1d5f3-745">最初のタスクで保存した発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-745">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="1d5f3-746">![発行プロファイルをインポートする](whats-new-in-aspnet-mvc-4/_static/image74.png "発行プロファイルをインポートします。")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-746">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="1d5f3-747">*発行プロファイルのインポート*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-747">*Importing publish profile*</span></span>
3. <span data-ttu-id="1d5f3-748">クリックして**接続の検証**です。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-748">Click **Validate Connection**.</span></span> <span data-ttu-id="1d5f3-749">検証が完了したら、クリックして**次**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-749">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1d5f3-750">接続の検証ボタンの横に表示される緑のチェック マークが表示されたら、検証が完了しました。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-750">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="1d5f3-751">![接続の検証](whats-new-in-aspnet-mvc-4/_static/image75.png "接続の検証")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-751">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="1d5f3-752">*接続の検証*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-752">*Validating connection*</span></span>
4. <span data-ttu-id="1d5f3-753">**設定**ページの**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックします (つまり**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-753">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="1d5f3-754">![Web 配置の構成](whats-new-in-aspnet-mvc-4/_static/image76.png "Web 配置の構成")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-754">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="1d5f3-755">*Web 配置の構成*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-755">*Web deploy configuration*</span></span>
5. <span data-ttu-id="1d5f3-756">データベース接続を次のように構成します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-756">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="1d5f3-757">**サーバー名**SQL Database サーバーの URL を使用して、入力、 *tcp:* プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-757">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="1d5f3-758">**ユーザー名**サーバー管理者ログイン名を入力します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-758">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="1d5f3-759">**パスワード**サーバー管理者ログイン パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-759">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="1d5f3-760">たとえば、新しいデータベース名を入力: *MVC4SampleDB*します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-760">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="1d5f3-761">![ターゲットの接続文字列を構成する](whats-new-in-aspnet-mvc-4/_static/image77.png "ターゲットの接続文字列を構成します。")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-761">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="1d5f3-762">*ターゲットの接続文字列を構成します。*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-762">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="1d5f3-763">次に、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-763">Then click **OK**.</span></span> <span data-ttu-id="1d5f3-764">データベースのクリックを作成するように求められたら**はい**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-764">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="1d5f3-765">![データベースを作成する](whats-new-in-aspnet-mvc-4/_static/image78.png "データベース文字列を作成します。")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-765">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="1d5f3-766">*データベースの作成*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-766">*Creating the database*</span></span>
7. <span data-ttu-id="1d5f3-767">Windows azure SQL Database への接続に使用する接続文字列は、接続の既定のテキスト ボックス内に表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-767">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="1d5f3-768">その後、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-768">Then click **Next**.</span></span>

    <span data-ttu-id="1d5f3-769">![SQL データベースを指す接続文字列](whats-new-in-aspnet-mvc-4/_static/image79.png "SQL データベースを指す接続文字列")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-769">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="1d5f3-770">*SQL データベースを指す接続文字列*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-770">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="1d5f3-771">**プレビュー** ] ページで [**発行**します。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-771">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="1d5f3-772">![Web アプリケーションの発行](whats-new-in-aspnet-mvc-4/_static/image80.png "web アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-772">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="1d5f3-773">*Web アプリケーションの発行*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-773">*Publishing the web application*</span></span>
9. <span data-ttu-id="1d5f3-774">発行プロセスが完了すると、既定のブラウザーが発行した web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="1d5f3-774">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="1d5f3-775">![Windows Azure にアプリケーションが発行される](whats-new-in-aspnet-mvc-4/_static/image81.png "アプリケーションは Windows Azure に発行")</span><span class="sxs-lookup"><span data-stu-id="1d5f3-775">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="1d5f3-776">*Windows Azure に発行されたアプリケーション*</span><span class="sxs-lookup"><span data-stu-id="1d5f3-776">*Application published to Windows Azure*</span></span>
