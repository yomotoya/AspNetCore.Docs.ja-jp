---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: "ASP.NET MVC 4 の新機能 |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET MVC 4 は、安定したデザイン パターンと、ASP.NET の機能を使用して、スケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークとしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 8b1bdae048afc78399ccc7b0eac7125d9b983c13
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="8f0d4-103">ASP.NET MVC 4 の新機能</span><span class="sxs-lookup"><span data-stu-id="8f0d4-103">What's New in ASP.NET MVC 4</span></span>
====================
<span data-ttu-id="8f0d4-104">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="8f0d4-105">Web キャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-105">Download Web Camps Training Kit</span></span>](http://www.microsoft.com/download/29843)

> <span data-ttu-id="8f0d4-106">ASP.NET MVC 4 は、安定したデザイン パターンと、ASP.NET と .NET framework の機能を使用して、スケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="8f0d4-107">この新しい、4 番目のバージョンの framework は、モバイル web アプリケーションの開発を容易に焦点を当てています。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>
> 
> <span data-ttu-id="8f0d4-108">最初に、新しい ASP.NET MVC 4 プロジェクトを作成するときにここで、モバイル アプリケーションのプロジェクト テンプレートをモバイル デバイス向けのスタンドアロンのアプリのビルドに使用することができます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="8f0d4-109">さらに、ASP.NET MVC 4 統合 jQuery Mobile jQuery.Mobile.MVC NuGet パッケージを使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="8f0d4-110">jQuery Mobile は、HTML5 ベース フレームワークと Windows Phone、iPhone、Android を含む、すべての一般的なモバイル デバイス プラットフォームと互換性がある web アプリを開発するためです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="8f0d4-111">ただし、特殊化する場合は、ASP.NET MVC 4 こともできますをさまざまなデバイスごとに異なるビューを提供し、デバイス固有の最適化を指定できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>
> 
> <span data-ttu-id="8f0d4-112">このハンズオン ラボでは、ASP.NET MVC 4 で起動されます&quot;インターネット アプリケーション&quot;フォト ギャラリーのアプリケーションを作成するプロジェクト テンプレート。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="8f0d4-113">JQuery Mobile、および ASP.NET MVC 4 の新機能を使用して、さまざまなモバイル デバイスとデスクトップの web ブラウザーでの互換性を確保するアプリを段階的に強化されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="8f0d4-114">コードの生成と ASP.NET MVC 4 簡単方法のタスクをサポートすることにより非同期アクション メソッドを記述するための新しいコード レシピについても学びます&lt;ActionResult&gt;タイプを返します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>
> 
> <span data-ttu-id="8f0d4-115">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843)です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="8f0d4-116">目的</span><span class="sxs-lookup"><span data-stu-id="8f0d4-116">Objectives</span></span>

<span data-ttu-id="8f0d4-117">このハンズオン ラボでは学習する方法。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-117">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="8f0d4-118">ASP.NET MVC プロジェクト テンプレートなど、新しいモバイル アプリケーション プロジェクト テンプレートに機能強化の利用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-118">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="8f0d4-119">モバイル デバイスでの表示を向上させるために、HTML5 ビューポート属性と CSS のメディア クエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-119">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="8f0d4-120">プログレッシブの機能強化とタッチ最適化 web UI を構築するため、jQuery Mobile を使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-120">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="8f0d4-121">Mobile に固有のビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-121">Create mobile-specific views</span></span>
- <span data-ttu-id="8f0d4-122">ビュー スイッチャー コンポーネントを使用して、アプリケーションでのモバイルとデスクトップのビューを切り替える</span><span class="sxs-lookup"><span data-stu-id="8f0d4-122">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="8f0d4-123">タスクのサポートを使用して非同期コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-123">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="8f0d4-124">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="8f0d4-124">Prerequisites</span></span>

<span data-ttu-id="8f0d4-125">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-125">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="8f0d4-126">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 B](#AppendixB)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-126">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="8f0d4-127">[ASP.NET MVC 4](../../../mvc4.md) (Microsoft Visual Studio 2012 のインストールに含まれています)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-127">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="8f0d4-128">Windows Phone エミュレーター (に含まれる、 [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="8f0d4-128">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="8f0d4-129">省略可能: [WebMatrix 2](https://www.microsoft.com/web/webmatrix/)で**Electric Plum iPhone シミュレーター** (iPhone シミュレーターと web アプリケーションを参照するために使用する手順 3) の場合のみ拡張機能</span><span class="sxs-lookup"><span data-stu-id="8f0d4-129">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="8f0d4-130">セットアップ</span><span class="sxs-lookup"><span data-stu-id="8f0d4-130">Setup</span></span>

<span data-ttu-id="8f0d4-131">ラボ ドキュメント全体にわたって、コード ブロックを挿入するように指示されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="8f0d4-132">必要に応じて、そのコードのほとんどは、Visual Studio のコード スニペットを手動で追加することを回避するのに Visual Studio 内から使用することができますとして提供されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-132">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="8f0d4-133">コード スニペットをインストールします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-133">To install the code snippets:</span></span>

1. <span data-ttu-id="8f0d4-134">Windows エクスプ ローラー ウィンドウを開き、ラボを参照**Source\Setup**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-134">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="8f0d4-135">ダブルクリックして、 **Setup.cmd**を Visual Studio のコード スニペットをインストールするには、このフォルダー内のファイルです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-135">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="8f0d4-136">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 a: を使用してコード スニペット](#AppendixA)&quot;です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="8f0d4-137">演習</span><span class="sxs-lookup"><span data-stu-id="8f0d4-137">Exercises</span></span>

<span data-ttu-id="8f0d4-138">このハンズオン ラボには、次の実習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="8f0d4-139">新しい ASP.NET MVC 4 プロジェクト テンプレート</span><span class="sxs-lookup"><span data-stu-id="8f0d4-139">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="8f0d4-140">フォト ギャラリーの Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-140">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="8f0d4-141">モバイル デバイスのサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-141">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="8f0d4-142">非同期コント ローラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-142">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="8f0d4-143">各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="8f0d4-144">演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="8f0d4-145">この演習を完了する時間を推定: **60 分**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-145">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="8f0d4-146">手順 1: 新しい ASP.NET MVC 4 プロジェクト テンプレート</span><span class="sxs-lookup"><span data-stu-id="8f0d4-146">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="8f0d4-147">この演習では、ASP.NET MVC 4 プロジェクト テンプレートの拡張機能を調査します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-147">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="8f0d4-148">インターネット アプリケーションのテンプレートだけでなく MVC 3 に既に存在このバージョンが追加されますモバイル アプリケーションの別のテンプレートです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-148">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="8f0d4-149">最初に、各テンプレートの関連する機能の一部になります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-149">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="8f0d4-150">次に、作業する適切なアプローチを使用して、さまざまなプラットフォームでは正常にページを表示します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-150">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="8f0d4-151">タスク 1 - インターネット アプリケーション テンプレートの表示</span><span class="sxs-lookup"><span data-stu-id="8f0d4-151">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="8f0d4-152">開いている**Visual Studio**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-152">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="8f0d4-153">選択、**ファイル |新しい |プロジェクト**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-153">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="8f0d4-154">**新しいプロジェクト**ダイアログで、選択、 **Visual c# |Web**左側のウィンドウでテンプレート ツリー、および選択**ASP.NET MVC 4 Web アプリケーションです。**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-154">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="8f0d4-155">プロジェクトに名前を**PhotoGallery**、場所を選択 (または既定値) をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-155">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0d4-156">後で作成するようになりました PhotoGallery ASP.NET MVC 4 ソリューションをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-156">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="8f0d4-157">![新しいプロジェクトを作成する](whats-new-in-aspnet-mvc-4/_static/image1.png "新規プロジェクトの作成")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-157">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="8f0d4-158">*新しいプロジェクトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-158">*Creating a new project*</span></span>
3. <span data-ttu-id="8f0d4-159">**新しい ASP.NET MVC 4 プロジェクト**ダイアログで、選択、**インターネット アプリケーション**プロジェクト テンプレートをクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-159">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="8f0d4-160">ビュー エンジンとして Razor を選択していないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-160">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="8f0d4-161">![新しい ASP.NET MVC 4 インターネット アプリケーションを作成する](whats-new-in-aspnet-mvc-4/_static/image2.png "新しい ASP.NET MVC 4 インターネット アプリケーションの作成")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-161">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="8f0d4-162">*新しい ASP.NET MVC 4 インターネット アプリケーションの作成*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-162">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0d4-163">ASP.NET MVC 3 razor 構文が導入されました。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-163">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="8f0d4-164">その目的は、文字の数および fast およびワークフローをコーディングする流体を有効にすると、ファイルに必要なキーストロークを最小限に抑えるです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-164">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="8f0d4-165">Razor を活用して既存 c#/VB (またはその他の) 言語スキルと優れた HTML 構築ワークフローを使用できるテンプレートのマークアップ構文を配信します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-165">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="8f0d4-166">キーを押して**f5 キーを押して**にソリューションを実行し、更新されたテンプレートを表示します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-166">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="8f0d4-167">次の機能を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-167">You can check out the following features:</span></span>

    <span data-ttu-id="8f0d4-168">**モダン スタイル テンプレート**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-168">**Modern-style templates**</span></span>

    <span data-ttu-id="8f0d4-169">テンプレートが更新されました、モダン外見の他のスタイルを提供することです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-169">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="8f0d4-170">![ASP.NET MVC 4 のスタイルを変更テンプレート](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 テンプレートのスタイルを変更")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-170">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="8f0d4-171">*ASP.NET MVC 4 のスタイルを変更テンプレート*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-171">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="8f0d4-172">![新しい連絡先 ページ](whats-new-in-aspnet-mvc-4/_static/image4.png "新しい連絡先 ページ")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-172">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="8f0d4-173">*新しい連絡先 ページ*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-173">*New Contact page*</span></span>

    <span data-ttu-id="8f0d4-174">**アダプティブ レンダリング**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-174">**Adaptive Rendering**</span></span>

    <span data-ttu-id="8f0d4-175">チェック アウト、ブラウザー ウィンドウのサイズを変更し、どのページ レイアウトに動的に適応新しいウィンドウのサイズに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-175">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="8f0d4-176">これらのテンプレートは、アダプティブ レンダリング手法をカスタマイズすることがなく、デスクトップおよびモバイルの両方のプラットフォームで正しく表示するために使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-176">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="8f0d4-177">![ASP.NET MVC 4 プロジェクト テンプレートを別のブラウザーのサイズを](whats-new-in-aspnet-mvc-4/_static/image5.png "別のブラウザーのサイズで ASP.NET MVC 4 プロジェクト テンプレート")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-177">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="8f0d4-178">*別のブラウザーのサイズで ASP.NET MVC 4 プロジェクト テンプレート*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-178">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="8f0d4-179">**JavaScript での豊富な UI**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-179">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="8f0d4-180">既定のプロジェクト テンプレートを別の拡張機能より対話的な JavaScript を提供する JavaScript の使用です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-180">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="8f0d4-181">テンプレートで使用されているログインおよび登録のリンクには、jQuery 検証を使用して、クライアント側からの入力フィールドを検証する方法が実現しました。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-181">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![jQuery 検証](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="8f0d4-183">*jQuery 検証*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-183">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0d4-184">2 つログイン セクションでは、最初のセクションで、通知には、サイトから登録されてアカウントを使用しておよび altenativelly ログオンする (既定で無効になっている) google のように別の認証サービスを使用して 2 番目のセクションを記録できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-184">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="8f0d4-185">デバッガーを停止し、Visual Studio に戻り、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-185">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="8f0d4-186">ファイルを開く**AuthConfig.cs**の下にある、**アプリ\_開始**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-186">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="8f0d4-187">Google クライアントを登録する最後の行からコメントを削除する*OAuth*認証します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-187">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > <span data-ttu-id="8f0d4-188">Facebook、Twitter、Microsoft などの任意の OpenID または OAuth サービスを使用して認証を簡単に有効にすることができますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-188">Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.</span></span>
8. <span data-ttu-id="8f0d4-189">キーを押して**f5 キーを押して**ソリューションを実行し、ログイン ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-189">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="8f0d4-190">選択**Google**サービスにログインします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-190">Select **Google** service to log in.</span></span>

    ![サービスのログを選択します。](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="8f0d4-192">*サービスのログを選択します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-192">*Selecting the log in service*</span></span>
10. <span data-ttu-id="8f0d4-193">Google アカウントを使用してをログインします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-193">Log in using your Google account.</span></span>
11. <span data-ttu-id="8f0d4-194">Google アカウントから情報を取得するサイト (localhost) を許可します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-194">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="8f0d4-195">最後に、Google アカウントに関連付けるサイトで登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-195">Finally, you will have to register in the site to associate the Google account.</span></span>

    ![Google アカウントを関連付ける](whats-new-in-aspnet-mvc-4/_static/image8.png)

    <span data-ttu-id="8f0d4-197">*Google アカウントの関連付け*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-197">*Associating your Google account*</span></span>
13. <span data-ttu-id="8f0d4-198">デバッガーを停止し、Visual Studio に戻り、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-198">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="8f0d4-199">他のプロジェクト テンプレートの ASP.NET MVC 4 で導入された新しい機能を確認するようにソリューションを調べるようになりました。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-199">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="8f0d4-200">![ASP.NET MVC 4 インターネット アプリケーション プロジェクト テンプレート](whats-new-in-aspnet-mvc-4/_static/image9.png "ASP.NET MVC 4 インターネット アプリケーションのプロジェクト テンプレート")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-200">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="8f0d4-201">*ASP.NET MVC 4 インターネット アプリケーション プロジェクト テンプレート*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-201">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

    - <span data-ttu-id="8f0d4-202">**HTML 5 マークアップ**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-202">**HTML 5 Markup**</span></span>

        <span data-ttu-id="8f0d4-203">新しいテーマ マークアップを調べるにはテンプレート ビューを参照します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-203">Browse template views to find out the new theme markup.</span></span>

        <span data-ttu-id="8f0d4-204">![Razor、HTML5 マークアップ About.cshtml を使用して新しいテンプレートです。] (whats-new-in-aspnet-mvc-4/_static/image10.png "Razor、HTML5 マークアップ About.cshtml を使用して、新しいテンプレート。")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-204">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

        <span data-ttu-id="8f0d4-205">*Razor、HTML5 のマークアップ (About.cshtml) を使用して新しいテンプレートです。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-205">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
    - <span data-ttu-id="8f0d4-206">**更新された JavaScript ライブラリ**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-206">**Updated JavaScript libraries**</span></span>

        <span data-ttu-id="8f0d4-207">ASP.NET MVC 4 の既定のテンプレートには、KnockoutJS、豊富な作成できる JavaScript MVVM フレームワークと応答性の高い web アプリケーションの JavaScript と HTML を使ったが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-207">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="8f0d4-208">ような MVC3、jQuery、jQuery UI ライブラリにも含まれます ASP.NET MVC 4 です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-208">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

        > [!NOTE]
        > <span data-ttu-id="8f0d4-209">このリンクに KnockOutJS ライブラリに関する詳しい情報を入手できます: [ [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/)です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-209">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="8f0d4-210">さらに、jQuery、jQuery UI について学習できますで[ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/)です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-210">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="8f0d4-211">タスク 2 - モバイル アプリケーション テンプレートの表示</span><span class="sxs-lookup"><span data-stu-id="8f0d4-211">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="8f0d4-212">ASP.NET MVC 4 には、モバイル web サイトやタブレットのブラウザーの開発が容易になります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-212">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="8f0d4-213">このテンプレート (通知をコント ローラーのコードがほぼ同一である)、インターネット アプリケーションのテンプレートと同じアプリケーション構造を持つが、モバイル デバイスのタッチ ベースで正しく表示するために、スタイルが変更されました。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-213">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="8f0d4-214">選択、**ファイル |新しい |プロジェクト**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-214">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="8f0d4-215">**新しいプロジェクト**ダイアログで、選択、 **Visual c# |Web**左側のウィンドウでテンプレート ツリー、および選択、 **ASP.NET MVC 4 Web アプリケーションです。**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-215">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="8f0d4-216">プロジェクトに名前を**PhotoGallery.Mobile**、場所を選択 (または既定値)、選択&quot;ソリューションに追加&quot; をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-216">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="8f0d4-217">**新しい ASP.NET MVC 4 プロジェクト**ダイアログで、選択、 **Mobile アプリケーション**プロジェクト テンプレートをクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-217">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="8f0d4-218">ビュー エンジンとして Razor を選択していないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-218">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="8f0d4-219">![新しい ASP.NET MVC 4 モバイル アプリケーションを作成する](whats-new-in-aspnet-mvc-4/_static/image11.png "新しい ASP.NET MVC 4 モバイル アプリケーションを作成します。")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-219">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="8f0d4-220">*新しい ASP.NET MVC 4 モバイル アプリケーションを作成します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-220">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="8f0d4-221">これで、ソリューションを探索し、モバイル用に ASP.NET MVC 4 ソリューション テンプレートで導入された新機能の一部を確認することは。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-221">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="8f0d4-222">**jQuery Mobile ライブラリ**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-222">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="8f0d4-223">モバイル アプリケーション プロジェクト テンプレートには、モバイル ブラウザーの互換性のため、オープン ソース ライブラリは、jQuery モバイル ライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-223">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="8f0d4-224">jQuery Mobile では、CSS および JavaScript をサポートするモバイル ブラウザーに段階的な強化が適用されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-224">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="8f0d4-225">段階的な強化は、豊富な内容を表示する最も強力なブラウザーだけことができます。 中に、web ページの基本コンテンツを表示するすべてのブラウザーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-225">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="8f0d4-226">JQuery モバイル スタイルに含まれる、JavaScript および CSS ファイルでは、ページのマークアップでの変更を加えずに 画面で、コンテンツに合わせてモバイル ブラウザーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-226">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="8f0d4-228">*テンプレートに含まれる jQuery モバイル ライブラリ*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-228">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="8f0d4-229">**HTML5 ベースのマークアップ**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-229">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="8f0d4-231">*HTML5 マークアップ、(Login.cshtml および index.cshtml) を使用してテンプレートをモバイル アプリケーション*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-231">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="8f0d4-232">キーを押して**f5 キーを押して**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-232">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="8f0d4-233">開く、 **Windows Phone 7 エミュレーター**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-233">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="8f0d4-234">Phone のスタート画面で Internet Explorer を開きます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-234">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="8f0d4-235">チェック アウトし、デスクトップ アプリケーションが開始された URL および電話からその URL に移動 (例: `http://localhost:[PortNumber]/`)。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-235">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="8f0d4-236">ログイン ページに入力するか、チェック アウトすることができるので、ページに関するします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-236">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="8f0d4-237">Web サイトのスタイルが mobile 用の新しい Metro アプリに基づくことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-237">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="8f0d4-238">ASP.NET MVC 4 プロジェクト テンプレートは、ページのすべての要素が表示され、有効になっていることを確認、モバイル デバイスで正しく表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-238">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="8f0d4-239">ヘッダーのリンクがクリックしてまたはタップするのに十分な大きさであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-239">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="8f0d4-240">![プロジェクトのモバイル デバイス上でテンプレート ページ](whats-new-in-aspnet-mvc-4/_static/image14.png "モバイル デバイス上でテンプレート ページのプロジェクト")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-240">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="8f0d4-241">*モバイル デバイスでのプロジェクト テンプレートのページ*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-241">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="8f0d4-242">新しいテンプレートを使用しても、**ビューポート メタ タグ**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-242">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="8f0d4-243">ほとんどのモバイル ブラウザーは、仮想のブラウザー ウィンドウの幅を定義または&quot;ビューポート&quot;、これは、モバイル デバイスの実際の幅よりも大きいです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-243">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="8f0d4-244">これにより、モバイル ブラウザーに仮想の表示内の web ページ全体を表示できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-244">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="8f0d4-245">**ビューポート メタ タグ**により、モバイル デバイスで、幅、高さ、およびブラウザーの領域のスケールを設定する web 開発者**です。**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-245">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="8f0d4-246">ASP.NET MVC 4 アプリケーションにテンプレートをモバイル デバイスの幅に、ビューポートを設定する (&quot;幅デバイスの幅を =&quot;) レイアウト テンプレート (*\shared\_Layout.cshtml*) できるように、すべて、ページには、デバイスの画面幅を設定、ビューポートがあります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-246">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="8f0d4-247">ビューポートのメタ タグによって既定のブラウザー ビューは変更されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-247">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="8f0d4-248">開いている **\_Layout.cshtml**内にある、**ビュー |共有**フォルダー、およびコメント、ビューポートのメタ タグ。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-248">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="8f0d4-249">アプリケーションを実行しない場合既に開き、相違点を確認します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-249">Run the application, if not already opened, and check out the differences.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

    <span data-ttu-id="8f0d4-250">![ビューポートのメタ タグをコメント化した後、サイト](whats-new-in-aspnet-mvc-4/_static/image15.png "ビューポートのメタ タグをコメント化した後、サイト")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-250">![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")</span></span>

    <span data-ttu-id="8f0d4-251">*ビューポートのメタ タグをコメント化した後、サイト*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-251">*The site after commenting the viewport meta tag*</span></span>
10. <span data-ttu-id="8f0d4-252">Visual Studio で、キーを押します。 **shift キーを押し** + **f5 キーを押して**アプリケーションのデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-252">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="8f0d4-253">ビューポートのメタ タグのコメントを解除します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-253">Uncomment the viewport meta tag.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="8f0d4-254">タスク 3 - アダプティブ レンダリングを使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-254">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="8f0d4-255">このタスクでは、モバイル デバイスと Web ブラウザーに正しく、Web ページをカスタマイズすることがなく、同時に表示するために別の方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-255">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="8f0d4-256">同様の目的で、ビューポートのメタ タグを使用していた既にです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-256">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="8f0d4-257">これで、別の強力なメソッドを満たすことになります:*アダプティブ レンダリング*です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-257">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="8f0d4-258">アダプティブ レンダリングが使用する手法**CSS3 メディア クエリ**をページに適用されるスタイルをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-258">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="8f0d4-259">メディア クエリは、特定の条件下の CSS スタイルをグループ化、スタイル シート内の条件を定義します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-259">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="8f0d4-260">条件が true の場合のみ、スタイルが宣言されたオブジェクトに適用されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-260">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="8f0d4-261">アダプティブ レンダリング手法が提供する柔軟性は、さまざまなデバイスで、サイトを表示するためには、そのカスタマイズを使用できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-261">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="8f0d4-262">必要な 1 つのスタイル シートのロジックのコードを記述することがなく、スタイルを選択する数だけのスタイルを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-262">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="8f0d4-263">そのため、その量が減り、重複したコードと目的を表示するためのロジックとページのスタイルを適用する非常に便利な方法になります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-263">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="8f0d4-264">その一方で、帯域幅の消費量が増加、CSS ファイルのサイズが多少大きくなりすぎるとします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-264">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="8f0d4-265">アダプティブ レンダリング手法を使用すると、サイトになります**ブラウザーに関係なく、適切に表示されます。**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-265">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="8f0d4-266">ただし、余分な帯域幅を読み込む場合は、問題を考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-266">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="8f0d4-267">メディア クエリの基本形式は: @media \[スコープ: すべて | ハンドヘルド | 印刷 | プロジェクション | 画面\]([プロパティ: 値].プロパティ: 値)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-267">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="8f0d4-268">メディア クエリの例: &gt;  **@mediaすべてと (幅の最大値: 1000px) と (幅の最小値: 700px) {}:** 700px と 1000px 間のすべての解像度。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-268">Examples of media queries: &gt;**@media all and (max-width: 1000px) and (min-width: 700px) {}:** For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="8f0d4-269">**@media画面と (幅の最小値: 400 px と表) と (幅の最大値: 700px) {...}:**画面に対してのみです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-269">**@media screen and (min-width: 400px) and (max-width: 700px) { ... }:** Only for screens.</span></span> <span data-ttu-id="8f0d4-270">解像度は、400 ~ 700px でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-270">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="8f0d4-271">**@mediaハンドヘルドと (幅の最小値: 20em)、画面と (幅の最小値: 20em) {...}:**のハンドヘルド デバイス (携帯とデバイス) とスクリーンです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-271">**@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:** For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="8f0d4-272">幅の最小値は、20em より大きくする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-272">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="8f0d4-273">上の詳細については、これを見つけることができます、 [W3C サイト](http://www.w3.org/TR/css3-mediaqueries/)です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-273">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="8f0d4-274">アダプティブのレンダリングの動作方法について学びますようになりました、4 既定 web サイト テンプレートの ASP.NET MVC の読みやすさを向上します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-274">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="8f0d4-275">開く、 **PhotoGallery.sln**ソリューション タスク 1 で作成し、選択、 **PhotoGallery**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-275">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="8f0d4-276">キーを押して**f5 キーを押して**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-276">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="8f0d4-277">ブラウザーの幅、または元のサイズの四半期未満を半分に、windows の設定を変更します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-277">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="8f0d4-278">ヘッダー内の項目に対して行う処理に注意してください。 一部の要素は、ヘッダーの表示領域に表示されません。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-278">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="8f0d4-279">開いている**Site.css**ファイルにある、Visual Studio のソリューション エクスプ ローラーから**コンテンツ**プロジェクト フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-279">Open **Site.css** file from the Visual Studio Solution explorer, located in **Content** project folder.</span></span> <span data-ttu-id="8f0d4-280">キーを押して**ctrl キーを押しながら F** Visual Studio の統合された検索を開き、書き込む **@media** を検索、 **CSS メディア クエリ**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-280">Press **CTRL + F** to open Visual Studio integrated search, and write **@media** to locate the **CSS media query**.</span></span>

    <span data-ttu-id="8f0d4-281">このテンプレートで定義されているメディア クエリの条件は、この方法で機能します。 ブラウザーのウィンドウのサイズが以下の場合**850 px**、適用される CSS ルールがこのメディア ブロック内で定義されています。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-281">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="8f0d4-282">![メディア クエリを検索する](whats-new-in-aspnet-mvc-4/_static/image16.png "メディア クエリの検索")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-282">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="8f0d4-283">*メディア クエリの検索*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-283">*Locating the media query*</span></span>
4. <span data-ttu-id="8f0d4-284">850 に設定された最大幅属性値を置き換えます px で**10 px**アダプティブのレンダリングを無効にするために、キーを押します**ctrl キーを押しながら S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-284">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="8f0d4-285">キーを押して、ブラウザーに戻り**ctrl キーを押しながら f5 キーを押して**に加えた変更にページを更新します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-285">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="8f0d4-286">ウィンドウの幅を調整するときは、両方のページの相違点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-286">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="8f0d4-287">![左のページを適用する、@media右上、スタイルのスタイルを省略すると](whats-new-in-aspnet-mvc-4/_static/image17.png "、左のページを適用する、@media右上、スタイルのスタイルを省略すると")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-287">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="8f0d4-288">*左のページを適用する、@media右上、スタイルのスタイルを省略した場合*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-288">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="8f0d4-289">ここで、モバイル デバイス上の動作を確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-289">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="8f0d4-290">![左のページを適用する、@media右上、スタイルのスタイルを省略すると](whats-new-in-aspnet-mvc-4/_static/image18.png "、左のページを適用する、@media右上、スタイルのスタイルを省略すると")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-290">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="8f0d4-291">*左のページを適用する、@media右上、スタイルのスタイルを省略した場合*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-291">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="8f0d4-292">モバイル デバイスを使用する場合に、ページが Web ブラウザーで表示される場合は、その変更が非常に重要なされないことがわかりますが、違いがより明確になります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-292">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="8f0d4-293">イメージの左側にある、カスタム スタイルが読みやすさを向上することを確認できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-293">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="8f0d4-294">アダプティブ レンダリングは、条件付きスタイルの Web サイトと便利な方法で、スタイルの一般的な問題の解決を適用しやすくより多くの多くのシナリオで使用できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-294">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="8f0d4-295">ビューポートのメタ タグと CSS のメディア クエリに限定されない ASP.NET MVC 4 web アプリケーションでこれらの機能の利点を行えるようにします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-295">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="8f0d4-296">Visual Studio で、キーを押します。 **shift キーを押し** + **f5 キーを押して**アプリケーションのデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-296">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="8f0d4-297">手順 2: フォト ギャラリーの Web アプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="8f0d4-297">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="8f0d4-298">この演習では、写真を表示する、フォト ギャラリーのアプリケーションで動作します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-298">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="8f0d4-299">ASP.NET MVC 4 プロジェクト テンプレートを使用するが起動し、サービスから写真を取得し、それらをホーム ページに表示するための機能を追加し。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-299">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="8f0d4-300">次の手順でモバイル デバイスでの表示方法を強化するためにこのソリューションが更新されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-300">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="8f0d4-301">タスク 1 - モック フォト サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-301">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="8f0d4-302">このタスクでは、ギャラリーに表示されるコンテンツを取得するフォト サービスのモックを作成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-302">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="8f0d4-303">これを行うには、それぞれの写真データを含む JSON ファイルが返されるだけ、新しいコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-303">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="8f0d4-304">開いている**Visual Studio**開いていない場合。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-304">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="8f0d4-305">選択、**ファイル |新しい |プロジェクト**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-305">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="8f0d4-306">**新しいプロジェクト**ダイアログで、選択、 **Visual c# |Web**左側のウィンドウでテンプレート ツリー、および選択**ASP.NET MVC 4 Web アプリケーションです。**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-306">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="8f0d4-307">プロジェクトに名前を**PhotoGallery**、場所を選択 (または既定値) をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-307">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="8f0d4-308">既存の ASP.NET MVC 4 から作業を継続する代わりに、**インターネット アプリケーション**からソリューション**演習 1**し、次の手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-308">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="8f0d4-309">**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**プロジェクト テンプレートをクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-309">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="8f0d4-310">Razor ビュー エンジンとして選択されている必要があることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-310">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="8f0d4-311">**ソリューション エクスプ ローラー**を右クリックし、**アプリ\_データ**フォルダー、プロジェクト、および選択の**追加 |既存の項目**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-311">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="8f0d4-312">参照、 **Source\Assets\App\_データ**このラボのフォルダーを追加し、 **Photos.json**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-312">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="8f0d4-313">名前を持つ新しいコント ローラーを作成**PhotoController**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-313">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="8f0d4-314">これを行うを右クリックし、**コント ローラー**フォルダーに移動して**追加**選択**コント ローラー。**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-314">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="8f0d4-315">ままの状態で完了、コント ローラー名、**空の MVC コント ローラー**テンプレートとクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-315">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="8f0d4-316">![追加、PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "PhotoController を追加します。")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-316">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="8f0d4-317">*PhotoController を追加します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-317">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="8f0d4-318">置換、**インデックス**メソッドに次の**ギャラリー**アクション、およびプロジェクトに最近追加した JSON ファイルから返されるコンテンツ。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-318">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="8f0d4-319">(コード スニペットの*ASP.NET MVC 4 ラボ - Ex02 - ギャラリー アクション*)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-319">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. <span data-ttu-id="8f0d4-320">キーを押して**f5 キーを押して**ソリューションを実行し、モック フォト サービスをテストするために、次の URL を参照する: `http://localhost:[port]/photo/gallery` ([ポート] の値は、アプリケーションが起動される現在のポートによって異なります)。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-320">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="8f0d4-321">この URL に要求のコンテンツを取得する必要があります、 **Photos.json**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-321">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="8f0d4-322">![モック フォト サービスのテスト](whats-new-in-aspnet-mvc-4/_static/image20.png "モック フォト サービスのテスト")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-322">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="8f0d4-323">*モック フォト サービスのテスト*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-323">*Testing the mocked photo service*</span></span>

<span data-ttu-id="8f0d4-324">実際の実装で使用する可能性があります[ASP.NET Web API](../../../../web-api/index.md)してフォト ギャラリーのサービスを実装します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-324">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="8f0d4-325">ASP.NET Web API は、さまざまなブラウザーやモバイル デバイスを含む、クライアントに到達できる HTTP サービスを作成しやすくフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-325">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="8f0d4-326">ASP.NET Web API は、.NET Framework に基づいて RESTful アプリケーションを構築するのに最適なプラットフォームです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-326">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="8f0d4-327">タスク 2 - フォト ギャラリーを表示します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-327">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="8f0d4-328">このタスクでこの手順の最初のタスクで作成したモック サービスを使用して、フォト ギャラリーを表示するホーム ページが更新されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-328">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="8f0d4-329">モデル ファイルを追加し、ギャラリー ビューを更新します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-329">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="8f0d4-330">Visual Studio で、キーを押します。 **shift キーを押し** + **f5 キーを押して**アプリケーションのデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-330">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="8f0d4-331">作成、**フォト**クラス内で、**モデル**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-331">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="8f0d4-332">これを行うを右クリックし、**モデル**フォルダーを選択**追加** をクリック**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-332">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="8f0d4-333">名前に設定し、 **Photo.cs**  をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-333">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="8f0d4-334">次のメンバーを追加、**フォト**クラスです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-334">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="8f0d4-335">(コード スニペットの*ASP.NET MVC 4 ラボ - Ex02 - フォト モデル*)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-335">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. <span data-ttu-id="8f0d4-336">**Controllers** フォルダーから **HomeController.cs** ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-336">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="8f0d4-337">次の using ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-337">Add the following using statements.</span></span>

    <span data-ttu-id="8f0d4-338">(コード スニペットの*ASP.NET MVC 4 ラボ - Ex02 - HomeController Using*)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-338">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. <span data-ttu-id="8f0d4-339">更新プログラム、**インデックス**で使用するアクション**HttpClient**ギャラリーのデータを取得し、使用して、 **JavaScriptSerializer**ビュー モデルにシリアル化を解除します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-339">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="8f0d4-340">(コード スニペットの*ASP.NET MVC 4 ラボ - Ex02 - インデックス アクション*)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-340">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. <span data-ttu-id="8f0d4-341">開く、 **Index.cshtml**の下にあるファイル、 **views \home**フォルダーと次のコードでのすべてのコンテンツを置換します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-341">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="8f0d4-342">このコードは、サービスから取得したすべての写真をループし、それらが順序なしのリストに表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-342">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="8f0d4-343">(コード スニペットの*ASP.NET MVC 4 ラボ - Ex02 - フォト一覧*)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-343">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. <span data-ttu-id="8f0d4-344">**ソリューション エクスプ ローラー**を右クリックし、**コンテンツ**フォルダー、プロジェクト、および選択の**追加 |既存の項目**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-344">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="8f0d4-345">参照、 **Source\Assets\Content**このラボのフォルダーを追加し、 **Site.css**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-345">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="8f0d4-346">それに置き換わることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-346">You will have to confirm its replacement.</span></span> <span data-ttu-id="8f0d4-347">ある場合、 **Site.css**ファイルを開く、また、ファイルを再読み込みすることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-347">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="8f0d4-348">ファイル エクスプ ローラーを開き、全体をコピー**写真**フォルダーの下にある、 **Source\Assets**ソリューション エクスプ ローラーでプロジェクトのルート フォルダーには、このラボのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-348">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="8f0d4-349">アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-349">Run the application.</span></span> <span data-ttu-id="8f0d4-350">ギャラリーで写真を表示するホーム ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-350">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="8f0d4-351">![フォト ギャラリー](whats-new-in-aspnet-mvc-4/_static/image21.png "フォト ギャラリー")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-351">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="8f0d4-352">*フォト ギャラリー*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-352">*Photo Gallery*</span></span>
11. <span data-ttu-id="8f0d4-353">Visual Studio で、キーを押します。 **shift キーを押し** + **f5 キーを押して**アプリケーションのデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-353">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="8f0d4-354">手順 3: モバイル デバイスのサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-354">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="8f0d4-355">ASP.NET MVC 4 のキーの更新の 1 つは、モバイル開発のサポートです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-355">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="8f0d4-356">この演習では、前述の手順で作成した PhotoGallery ソリューションを拡張してモバイル アプリケーションの ASP.NET MVC 4 の新機能を調査します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-356">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="8f0d4-357">タスク 1 - ASP.NET MVC 4 アプリケーションで jQuery Mobile をインストールします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-357">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="8f0d4-358">開く、**開始**ソリューションにある**ソース/Ex3-MobileSupport/開始/**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-358">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="8f0d4-359">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-359">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="8f0d4-360">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-360">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8f0d4-361">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-361">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="8f0d4-362">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-362">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="8f0d4-363">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-363">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0d4-364">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-364">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8f0d4-365">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-365">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8f0d4-366">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-366">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="8f0d4-367">開く、 **Package Manager Console**  をクリックして、**ツール** &gt; **ライブラリ パッケージ マネージャー** &gt; **パッケージ マネージャーコンソール**メニュー オプション。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-367">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="8f0d4-368">![NuGet パッケージ マネージャー コンソールを開く](whats-new-in-aspnet-mvc-4/_static/image22.png "NuGet パッケージ マネージャー コンソールを開く")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-368">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="8f0d4-369">*NuGet パッケージ マネージャー コンソールを開く*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-369">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="8f0d4-370">パッケージ マネージャー コンソールをインストールする次のコマンドを実行、 **jQuery.Mobile.MVC**パッケージです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-370">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="8f0d4-371">jQuery Mobile は、タッチ最適化 web UI を構築するため、オープン ソース ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-371">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="8f0d4-372">JQuery.Mobile.MVC NuGet パッケージには、ASP.NET MVC 4 アプリケーションで jQuery Mobile を使用するヘルパーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-372">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0d4-373">次のコマンドを実行するには Nuget から jQuery.Mobile.MVC ライブラリがダウンロードするされます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-373">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="8f0d4-374">PM</span><span class="sxs-lookup"><span data-stu-id="8f0d4-374">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="8f0d4-375">このコマンドは、jQuery Mobile、および、次のいくつかのヘルパー ファイルがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-375">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="8f0d4-376">**Views/shared/\_Layout.Mobile.cshtml**: jQuery Mobile ベース レイアウト小さいスクリーン用に最適化されています。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-376">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="8f0d4-377">Web サイトは、モバイル ブラウザーから要求を受信したときに元のレイアウトが置き換えられます。 (\_Layout.cshtml) このものとします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-377">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="8f0d4-378">ビュー スイッチャー コンポーネント: から成る、 **Views/shared/\_ViewSwitcher.cshtml**部分ビュー、および**ViewSwitcherController.cs**コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-378">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="8f0d4-379">このコンポーネントは、ページのデスクトップ バージョンに切り替えるにはユーザーを有効にするモバイル ブラウザーでリンクを表示します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-379">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="8f0d4-380">![モバイル デバイスのサポートをフォト ギャラリー プロジェクト](whats-new-in-aspnet-mvc-4/_static/image23.png "モバイル デバイスのサポートをフォト ギャラリーのプロジェクト")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-380">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="8f0d4-381">*モバイル デバイスのサポートをフォト ギャラリーのプロジェクト*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-381">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="8f0d4-382">モバイルのバンドルを登録します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-382">Register the Mobile bundles.</span></span> <span data-ttu-id="8f0d4-383">これを行うには、開く、 **Global.asax.cs**ファイルし、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-383">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="8f0d4-384">(コード スニペットの*ASP.NET MVC 4 ラボ - Ex03 - 登録モバイル バンドル*)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-384">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. <span data-ttu-id="8f0d4-385">デスクトップの web ブラウザーを使用してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-385">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="8f0d4-386">開く、 **Windows Phone 7 エミュレーター**にある**[スタート] メニュー |すべてのプログラム |Windows Phone SDK 7.1 |Windows Phone エミュレーター。**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-386">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="8f0d4-387">Phone のスタート画面で Internet Explorer を開きます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-387">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="8f0d4-388">アプリケーションの開始 URL を確認し、電話のブラウザーでその URL を参照 (例: `http://localhost:[PortNumber]/`)。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-388">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="8f0d4-389">JQuery.Mobile.MVC がモバイル デバイス用に最適化されたビューを表示する、プロジェクトに新しい資産を作成するように、アプリケーションを Windows Phone エミュレーターで、異なるが表示されることがわかります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-389">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="8f0d4-390">電話、デスクトップ ビューに切り替える リンクが表示の上部にあるメッセージに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-390">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="8f0d4-391">さらに、  **\_Layout.Mobile.cshtml**がインストールされているパッケージによって作成されたレイアウトが、アプリケーションで別のレイアウトを含むです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-391">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0d4-392">これまで、モバイル ビューに戻るにリンクすることはありません。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-392">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="8f0d4-393">これは、以降のバージョンに含まれます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-393">It will be included in later versions.</span></span>

    <span data-ttu-id="8f0d4-394">![フォト ギャラリーのホーム ページのモバイル ビュー](whats-new-in-aspnet-mvc-4/_static/image24.png "フォト ギャラリーのホーム ページのモバイル ビュー")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-394">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="8f0d4-395">*フォト ギャラリーのホーム ページのモバイル ビュー*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-395">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="8f0d4-396">Visual Studio で、キーを押します。 **shift キーを押し** + **f5 キーを押して**アプリケーションのデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-396">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="8f0d4-397">タスク 2 - モバイル ビューの作成</span><span class="sxs-lookup"><span data-stu-id="8f0d4-397">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="8f0d4-398">このタスクでは、モバイル デバイスの優れた appareance に適応させるコンテンツをインデックス ビューのモバイル バージョンを作成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-398">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="8f0d4-399">コピー、 **Views\Home\Index.cshtml**を表示し、コピーを作成、新しいファイルの名前を変更する貼り付けます**Index.Mobile.cshtml**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-399">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="8f0d4-400">開いている、新しく作成された**Index.Mobile.cshtml**を表示し、既存の置換&lt;ul&gt;このコードを持つタグ。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-400">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="8f0d4-401">これにより、更新されます。、 &lt;ul&gt;から jQuery モバイルのテーマを使用して jQuery モバイル データ注釈を使用したタグです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-401">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > <span data-ttu-id="8f0d4-402">以下の点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-402">Notice that:</span></span>
    > 
    > - <span data-ttu-id="8f0d4-403">**データ ロール**属性に設定**listview** listview スタイルを使用して一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-403">The **data-role** attribute set to **listview** will render the list using the listview styles.</span></span>
    > 
    > - <span data-ttu-id="8f0d4-404">**データ埋め込み**属性が true に設定が丸みのある境界線と余白の一覧に表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-404">The **data-inset** attribute set to true will show the list with rounded border and margin.</span></span>
    > 
    > - <span data-ttu-id="8f0d4-405">**データ フィルター**属性に設定**true**検索ボックスを生成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-405">The **data-filter** attribute set to **true** will generate a search box.</span></span>
    > 
    > <span data-ttu-id="8f0d4-406">プロジェクトのドキュメント内の jQuery モバイル規則に関する詳細については、: [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-406">You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span></span>
3. <span data-ttu-id="8f0d4-407">キーを押して**ctrl キーを押しながら S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-407">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="8f0d4-408">切り替えて、 **Windows Phone エミュレーター**と、サイトを更新します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-408">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="8f0d4-409">一番上にある新しい検索ボックスと同様に、ギャラリーのリストの新しいルック アンド フィールに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-409">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="8f0d4-410">検索ボックスに単語を入力し、(たとえば、 **Tulips**) フォト ギャラリーで検索を開始します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-410">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="8f0d4-411">![Listview のスタイルを使用して、フィルター選択をギャラリー](whats-new-in-aspnet-mvc-4/_static/image25.png "listview のスタイルを使用して、フィルター選択をギャラリー")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-411">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="8f0d4-412">*Listview のスタイルを使用して、フィルター選択をギャラリー*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-412">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="8f0d4-413">要約すると、インデックス ビューのコピーを作成するビューというレシピを使用した、&quot;モバイル&quot;サフィックス。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-413">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="8f0d4-414">このサフィックスは ASP.NET MVC 4 にモバイル デバイスから生成されたすべての要求がこのインデックスのコピーを使用することを示します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-414">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="8f0d4-415">さらに、モバイル デバイスでのサイトのルック アンド フィールを強化するため、jQuery Mobile を使用するインデックス ビューのモバイル バージョンに更新されました。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-415">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="8f0d4-416">Visual Studio に戻り、開いている**Site.Mobile.css**の下にある、**コンテンツ**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-416">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="8f0d4-417">イメージの右側に表示する写真のタイトルの配置を修正します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-417">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="8f0d4-418">これを行うには、次のコードを追加、 **Site.Mobile.css**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-418">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="8f0d4-419">CSS</span><span class="sxs-lookup"><span data-stu-id="8f0d4-419">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="8f0d4-420">キーを押して**ctrl キーを押しながら S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-420">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="8f0d4-421">戻り、 **Windows Phone エミュレーター**と、サイトを更新します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-421">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="8f0d4-422">写真のタイトルが正しく配置されているようになりましたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-422">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="8f0d4-423">![イメージの右側に配置されているタイトル](whats-new-in-aspnet-mvc-4/_static/image26.png "イメージの右側に配置されているタイトル")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-423">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="8f0d4-424">*イメージの右側に配置されているタイトル*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-424">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="8f0d4-425">タスク 3 - jQuery Mobile テーマ</span><span class="sxs-lookup"><span data-stu-id="8f0d4-425">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="8f0d4-426">新しいオブジェクト指向 CSS フレームワーク サイトおよびアプリケーションに完全な統一されたビジュアル デ ザイン テーマを適用できるようにする軸は、すべてのレイアウトと jQuery Mobile でウィジェットを設計します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-426">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="8f0d4-427">jQuery モバイルの既定テーマにはには、文字が与えられている 5 見本が含まれています。 (a、b、c、d、e) のクイック リファレンスです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-427">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="8f0d4-428">このタスクで、既定よりも、さまざまなテーマを使用するモバイルのレイアウトが更新されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-428">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="8f0d4-429">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-429">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="8f0d4-430">開く、  **\_Layout.Mobile.cshtml**にあるファイル**\shared**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-430">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="8f0d4-431">設定のデータの役割を持つ div 要素を検索&quot;ページ&quot;し、更新、**データ テーマ**に&quot; **e**&quot;です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-431">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. <span data-ttu-id="8f0d4-432">キーを押して**ctrl キーを押しながら S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-432">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="8f0d4-433">サイトを更新、 **Windows Phone エミュレーター**し、新しい色スキームのことを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-433">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="8f0d4-434">![さまざまな画面の配色とレイアウトをモバイル](whats-new-in-aspnet-mvc-4/_static/image27.png "さまざまな画面の配色とモバイルのレイアウト")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-434">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="8f0d4-435">*さまざまな画面の配色とモバイルのレイアウト*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-435">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="8f0d4-436">タスク 4 - ビュー スイッチャー コンポーネントおよび機能をオーバーライドするブラウザーを使用して</span><span class="sxs-lookup"><span data-stu-id="8f0d4-436">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="8f0d4-437">モバイルに最適化された web ページの規則では、テキストはデスクトップ ビューまたはユーザーがページのデスクトップ バージョンに切り替えるできるサイトの完全モードと同様に、何かのリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-437">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="8f0d4-438">JQuery.Mobile.MVC パッケージには、サンプルが含まれています。**ビュー スイッチャー**コンポーネントをこの目的で使用される、  **\_Layout.Mobile.cshtml**ビュー。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-438">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="8f0d4-439">![デスクトップ ビューに切り替えるにはリンク](whats-new-in-aspnet-mvc-4/_static/image28.png "デスクトップ ビューに切り替えてへのリンク")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-439">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="8f0d4-440">*デスクトップ ビューに切り替えるにはリンクします。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-440">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="8f0d4-441">ビュー スイッチャーと呼ばれる新しい機能を使用して**ブラウザー オーバーライド**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-441">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="8f0d4-442">この機能は、アプリケーションでから実際に送信されている 1 つの異なるブラウザー (ユーザー エージェント) から送信されているかのように要求を処理できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-442">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="8f0d4-443">このタスクでは、jQuery.Mobile.MVC および ASP.NET MVC 4 の機能をオーバーライドする、新しいブラウザーにより追加されたビュー スイッチャーの実装サンプルを調査します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-443">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="8f0d4-444">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-444">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="8f0d4-445">開く、  **\_Layout.Mobile.cshtml**ビューの下にある、 **\shared**フォルダーと、部分ビューとして参照されているビュー スイッチャー コンポーネントに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-445">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="8f0d4-446">![ビュー スイッチャー コンポーネントを使用してモバイル レイアウト](whats-new-in-aspnet-mvc-4/_static/image29.png "ビュー スイッチャー コンポーネントを使用してモバイルのレイアウト")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-446">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="8f0d4-447">*ビュー スイッチャー コンポーネントを使用してモバイルのレイアウト*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-447">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="8f0d4-448">開く、  **\_ViewSwitcher.cshtml**部分ビュー。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-448">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="8f0d4-449">部分ビューは、新しいメソッドを使用して**ViewContext.HttpContext.GetOverriddenBrowser()** web 要求の起点を特定し、デスクトップまたはモバイル ビューのいずれかを切り替えるには、対応するリンクを表示します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-449">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="8f0d4-450">**GetOverridenBrowser**メソッドを返します、 **HttpBrowserCapabilitiesBase**要求に設定されているユーザー エージェントに対応するインスタンス (実際またはオーバーライドされる)。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-450">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="8f0d4-451">この値を使用するにはプロパティを取得するよう**IsMobileDevice**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-451">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="8f0d4-452">![部分ビューの ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher 部分ビュー")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-452">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="8f0d4-453">*ViewSwitcher 部分ビュー*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-453">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="8f0d4-454">開く、 **ViewSwitcherController.cs**クラス、**コント ローラー**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-454">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="8f0d4-455">チェック アウトする SwitchView アクションは ViewSwitcher コンポーネント内のリンクによって呼び出され、新しい HttpContext メソッドに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-455">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="8f0d4-456">**HttpContext.ClearOverridenBrowser()**メソッドは現在の要求に対するすべてのオーバーライドされたユーザー エージェントを削除します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-456">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="8f0d4-457">**HttpContext.SetOverridenBrowser()**メソッドは、指定されたユーザー エージェントを使用して、要求の実際のユーザー エージェント値をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-457">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="8f0d4-458">![ViewSwitcher コント ローラー](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher コント ローラー")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-458">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="8f0d4-459">*ViewSwitcher コント ローラー*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-459">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="8f0d4-460">ブラウザーのオーバーライドは ASP.NET MVC 4 の中心的な機能も使用可能な jQuery.Mobile.MVC パッケージをインストールしない場合でもです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-460">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="8f0d4-461">ただし、この機能は、ビュー、レイアウト、および部分ビューのみに影響ありの Request.Browser オブジェクトに依存する機能は影響しません。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-461">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="8f0d4-462">タスク 5 - デスクトップ ビューでビュー スイッチャーを追加します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-462">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="8f0d4-463">このタスクでビュー スイッチャーを含めるデスクトップのレイアウトが更新されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-463">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="8f0d4-464">これにより、デスクトップのビューを参照するときに、モバイル ビューに戻るモバイル ユーザーが許可されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-464">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="8f0d4-465">サイトを更新、 **Windows Phone エミュレーター**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-465">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="8f0d4-466">をクリックして、**デスクトップ ビュー**ギャラリーの上部にあるリンクします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-466">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="8f0d4-467">モバイル ビューに戻るようにデスクトップ ビューでビュー スイッチャーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-467">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="8f0d4-468">Visual Studio に戻り、開く、  **\_Layout.cshtml**ビュー。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-468">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="8f0d4-469">ログイン セクションを検索して表示するために呼び出しを挿入、  **\_ViewSwitcher**部分ビューの下、  **\_LogOnPartial**部分ビュー。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-469">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="8f0d4-470">次に、キーを押して**ctrl キーを押しながら S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-470">Then, press **CTRL + S** to save the changes.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. <span data-ttu-id="8f0d4-471">キーを押して**ctrl キーを押しながら S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-471">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="8f0d4-472">Windows Phone エミュレーターでページを更新し、拡大する画面をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-472">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="8f0d4-473">ホーム ページの表示通知、**モバイル ビュー**モバイルからデスクトップ ビューに切り替えるリンクします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-473">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="8f0d4-474">![デスクトップ ビューに表示切り替えコントロールの表示](whats-new-in-aspnet-mvc-4/_static/image32.png "デスクトップ ビューに表示されるビュー スイッチャー")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-474">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="8f0d4-475">*デスクトップ ビューに表示されるビュー スイッチャー*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-475">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="8f0d4-476">もう一度モバイル ビューに切り替えるしを参照**に関する**ページ ([port] http://localhost ホーム/について)。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-476">Switch to the Mobile view again and browse to **About** page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="8f0d4-477">、About.Mobile.cshtml ビューを作成していない場合でも、バージョン情報 ページが表示されているモバイルのレイアウトを使用してに注意してください (\_Layout.Mobile.cshtml)。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-477">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="8f0d4-478">![ページに関する](whats-new-in-aspnet-mvc-4/_static/image33.png "ページについて")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-478">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="8f0d4-479">*ページについて*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-479">*About page*</span></span>
8. <span data-ttu-id="8f0d4-480">最後に、デスクトップ、Web ブラウザーでサイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-480">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="8f0d4-481">デスクトップ ビューに影響なし前の更新プログラムのことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-481">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="8f0d4-482">![PhotoGallery デスクトップ ビュー](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery デスクトップ ビュー")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-482">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="8f0d4-483">*PhotoGallery デスクトップ ビュー*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-483">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="8f0d4-484">タスク 6 - 新しい表示モードの作成</span><span class="sxs-lookup"><span data-stu-id="8f0d4-484">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="8f0d4-485">新しい表示モードの機能はにより、アプリケーションは、要求を生成しているブラウザーによってビューを選択です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-485">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="8f0d4-486">たとえば、デスクトップ ブラウザーでは、ホーム ページを要求している場合、アプリケーションが返す、 **Views\Home\Index.cshtml**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-486">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="8f0d4-487">次に、モバイル ブラウザーは、ホーム ページを要求している場合、アプリケーションを返します、 **Views\Home\Index.mobile.cshtml**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-487">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="8f0d4-488">このタスクで、iPhone デバイス用のカスタマイズされたレイアウトを作成しますおよび iPhone デバイスからの要求をシミュレートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-488">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="8f0d4-489">これを行うには、いずれか、iPhone エミュレーターまたはシミュレーターを使用することができます (と同様に[Electric Mobile シミュレーター](http://www.electricplum.com/)) またはブラウザーのアドオンのユーザー エージェントを変更します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-489">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="8f0d4-490">IPhone をエミュレートするために、Safari ブラウザーでユーザー エージェント文字列を設定する方法については、次を参照してください。 [IE が見かけ上 Safari させる方法について](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html)David Alison のブログでします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-490">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="8f0d4-491">**このタスクは省略可能なことと、実行せず、ラボ全体で続行することができますに注意してください。**</span><span class="sxs-lookup"><span data-stu-id="8f0d4-491">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="8f0d4-492">Visual Studio で、キーを押します。 **shift キーを押し** + **f5 キーを押して**アプリケーションのデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-492">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="8f0d4-493">開いている**Global.asax.cs**に次の追加およびステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-493">Open **Global.asax.cs** and add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. <span data-ttu-id="8f0d4-494">アプリケーションに次の強調表示されたコードを追加\_メソッドを開始します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-494">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="8f0d4-495">(コード スニペットの*ASP.NET MVC 4 ラボ - Ex03 - iPhone DisplayMode*)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-495">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

    <span data-ttu-id="8f0d4-496">新しい登録されている**という DefaultDisplayMode &quot;iPhone&quot;**、静的内で**DisplayModeProvider.Instance.Modes**に対して照合される静的な一覧は、受信要求ごとにします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-496">You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request.</span></span> <span data-ttu-id="8f0d4-497">受信要求には、文字列が含まれている場合&quot;iPhone&quot;、ASP.NET MVC は名前を含むビューを検索、 &quot;iPhone&quot;サフィックス。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-497">If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix.</span></span> <span data-ttu-id="8f0d4-498">パラメーターに 0 では、特定が新しいモードにあることを示しますたとえば、このビューは、一般的なよりも限定&quot;.mobile&quot;モバイル デバイスからの要求と一致するルール。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-498">The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.</span></span>

    <span data-ttu-id="8f0d4-499">このコードを実行すた後 iPhone ブラウザーの要求を生成するときに、アプリケーションで使用、 **\shared\\_Layout.iPhone.cshtml**レイアウトを次の手順で作成されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-499">After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0d4-500">この方法で iPhone のデモのために簡略化され、可能性がありますが正しく機能しない iPhone のすべてのユーザー エージェント文字列の (テストの例は、大文字小文字を区別) の要求をテストします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-500">This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).</span></span>
4. <span data-ttu-id="8f0d4-501">コピーを作成、  **\_Layout.Mobile.cshtml**ファイルで、 **\shared**フォルダーへのコピーの名前を変更および&quot;  **\_Layout.iPhone.csthml**&quot;.</span><span class="sxs-lookup"><span data-stu-id="8f0d4-501">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="8f0d4-502">開いている **\_Layout.iPhone.csthml**前の手順で作成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-502">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="8f0d4-503">データ ロールの属性に設定された div 要素を検索**ページ**を変更して、**データ テーマ**属性を&quot; **、**&quot;です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-503">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

    <span data-ttu-id="8f0d4-504">これで、ASP.NET MVC 4 アプリケーションに 3 つのレイアウトがあります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-504">Now you have 3 layouts in your ASP.NET MVC 4 application:</span></span>

    1. <span data-ttu-id="8f0d4-505">**\_Layout.cshtml**: 既定のレイアウトをデスクトップ ブラウザーのために使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-505">**\_Layout.cshtml**: default layout used for desktop browsers.</span></span>
    2. <span data-ttu-id="8f0d4-506">**\_Layout.mobile.cshtml**: 既定のレイアウトがモバイル デバイスのために使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-506">**\_Layout.mobile.cshtml**: default layout used for mobile devices.</span></span>
    3. <span data-ttu-id="8f0d4-507">**\_Layout.iPhone.cshtml**: から区別するために別の配色を使用して、iPhone デバイス用の特定のレイアウト\_Layout.mobile.cshtml です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-507">**\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.</span></span>
7. <span data-ttu-id="8f0d4-508">キーを押して**f5 キーを押して**アプリケーションを実行し、サイトの参照を**Windows Phone エミュレーター**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-508">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="8f0d4-509">開いている、 **iPhone シミュレーター** (を参照してください[付録 C](#AppendixC)インストールして iPhone シミュレーターを構成する方法について)、サイトを参照するすぎるとします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-509">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="8f0d4-510">各電話番号が、特定のテンプレートを使用することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-510">Notice that each phone is using the specific template.</span></span>

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="8f0d4-512">*各モバイル デバイスのさまざまなビューを使用します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-512">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="8f0d4-513">手順 4: 非同期コント ローラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-513">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="8f0d4-514">Microsoft .NET Framework 4.5 では、.NET プログラミングでの非同期性の新しい基盤を提供する c# および Visual Basic での言語の新機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-514">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="8f0d4-515">この新しい foundation、非同期プログラミングのと同様と同期プログラミングと約簡単です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-515">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="8f0d4-516">使用して、ASP.NET MVC 4 で非同期アクション メソッドを記述することができるよう、 **AsyncController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-516">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="8f0d4-517">CPU バインド以外の要求を実行時間の長いの非同期アクション メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-517">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="8f0d4-518">要求の処理中に作業を実行してから、Web サーバーのブロックを回避できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-518">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="8f0d4-519">AsyncController クラスは通常、実行時間の長い Web サービス呼び出しに使用されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-519">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="8f0d4-520">この演習では、ASP.NET MVC 4 での非同期操作の基本について説明します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-520">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="8f0d4-521">次の資料をチェック_アウトできます詳しく知りたい場合: [ [https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-521">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="8f0d4-522">タスク 1 - 非同期コント ローラーを実装します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-522">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="8f0d4-523">開く、**開始**ソリューションにある**ソース/Ex4-Async/開始/**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-523">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="8f0d4-524">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-524">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="8f0d4-525">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-525">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8f0d4-526">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-526">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="8f0d4-527">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-527">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="8f0d4-528">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-528">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0d4-529">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-529">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8f0d4-530">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-530">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8f0d4-531">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-531">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="8f0d4-532">開く、 **HomeController.cs**クラス、**コント ローラー**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-532">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="8f0d4-533">次の追加ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-533">Add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. <span data-ttu-id="8f0d4-534">更新プログラム、 **HomeController**クラスから継承する**AsyncController**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-534">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="8f0d4-535">AsyncController から派生したコント ローラーが非同期の要求の処理に ASP.NET を有効にしてサービスの同期アクション メソッドは、できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-535">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. <span data-ttu-id="8f0d4-536">追加、 **async**キーワードを**インデックス**メソッド型を返すと**タスク&lt;ActionResult&gt;**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-536">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > <span data-ttu-id="8f0d4-537">**Async**キーワードは、.NET Framework 4.5 は、新しいキーワードのいずれかです。 このメソッドが非同期コードを含むことをコンパイラに伝えます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-537">The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code.</span></span> <span data-ttu-id="8f0d4-538">A**タスク**オブジェクトは、今後のいつか完了可能性がある非同期操作を表します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-538">A **Task** object represents an asynchronous operation that may complete at some point in the future.</span></span>
6. <span data-ttu-id="8f0d4-539">置換、**クライアント。GetAsync()**次のように、完全非同期バージョンを使用して、使用して呼び出し await キーワード。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-539">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="8f0d4-540">(コード スニペットの*ASP.NET MVC 4 ラボ - Ex04 - されます*)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-540">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="8f0d4-541">以前のバージョンで使用していた、**結果**プロパティから、**タスク**結果には、(同期バージョン) が返されるまでスレッドをブロックするオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-541">In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).</span></span>
    > 
    > <span data-ttu-id="8f0d4-542">追加する、 **await**キーワード メソッドの呼び出しから返されるタスクを非同期的に待つようにコンパイラに指示します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-542">Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call.</span></span> <span data-ttu-id="8f0d4-543">これは待機中のメソッドが完了した後にのみ、コードの残りの部分は、コールバックとして実行されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-543">This means that the rest of the code will be executed as a callback only after the awaited method completes.</span></span> <span data-ttu-id="8f0d4-544">もう 1 つ点は、この作業を行うために、try-catch ブロックを変更する必要はありません: バック グラウンドで、またはフォア グラウンドで発生した例外は、フレームワークによって提供されるハンドラーを使用して追加の作業なし引き続きキャッチできます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-544">Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.</span></span>
7. <span data-ttu-id="8f0d4-545">次に示すように、新しいコードで行を置き換えることで、非同期実装を続行するコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-545">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="8f0d4-546">(コード スニペットの*ASP.NET MVC 4 ラボ - Ex04 - ReadAsStringAsync*)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-546">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. <span data-ttu-id="8f0d4-547">アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-547">Run the application.</span></span> <span data-ttu-id="8f0d4-548">わかりますなし、主要な変更がコードでは、サーバー リソースの使用率の向上を行うと、パフォーマンスの向上は、スレッド プールのスレッドはブロックされません。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-548">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0d4-549">ラボで新しい非同期プログラミング機能について詳しく知ることができますできます&quot; **c# および Visual Basic と .NET 4.5 での非同期プログラミング**&quot; Visual Studio のトレーニング キットに含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-549">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="8f0d4-550">タスク 2 - キャンセル トークンと処理のタイムアウト</span><span class="sxs-lookup"><span data-stu-id="8f0d4-550">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="8f0d4-551">タスク インスタンスを返す非同期アクション メソッドには、タイムアウトもサポートできます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-551">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="8f0d4-552">このタスクのキャンセル トークンを使用してタイムアウト シナリオを処理するインデックス メソッドのコードが更新されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-552">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="8f0d4-553">Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-553">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="8f0d4-554">以下を追加するステートメントを使用して、 **HomeController.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-554">Add the following using statement to the **HomeController.cs** file.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. <span data-ttu-id="8f0d4-555">更新を受信するインデックス アクション、 **CancellationToken**引数。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-555">Update the Index action to receive a **CancellationToken** argument.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. <span data-ttu-id="8f0d4-556">更新プログラム、**されます**キャンセル トークンを渡してへの呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-556">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="8f0d4-557">(コード スニペットの*CancellationToken に ASP.NET MVC 4 ラボ - Ex04 - SendAsync*)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-557">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. <span data-ttu-id="8f0d4-558">装飾、*インデックス*メソッドを**AsyncTimeout**属性は 500 ミリ秒に設定し、 **HandleError**属性を処理するように構成**TaskCanceledException**にリダイレクトすることで、 **TimedOut**ビュー。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-558">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="8f0d4-559">(コード スニペットの*ASP.NET MVC 4 ラボ - Ex04 - 属性*)</span><span class="sxs-lookup"><span data-stu-id="8f0d4-559">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. <span data-ttu-id="8f0d4-560">開く、 **PhotoController**クラスと更新プログラム、**ギャラリー**実行時間の長いタスクをシミュレートするために実行 1000 ミリ秒 (1 秒) を遅延するメソッド。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-560">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. <span data-ttu-id="8f0d4-561">開く、 **Web.config**ファイルし、次の要素を追加することによってカスタム エラーを有効にします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-561">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>


    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. <span data-ttu-id="8f0d4-562">新しいビューを作成**\shared**という**TimedOut**および既定のレイアウトを使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-562">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="8f0d4-563">ソリューション エクスプ ローラーで右クリックし、 **\shared**フォルダーと選択**追加 |ビュー**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-563">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="8f0d4-564">![各モバイル デバイスのさまざまなビューを使用して](whats-new-in-aspnet-mvc-4/_static/image36.png "各モバイル デバイスのさまざまなビューを使用します。")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-564">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="8f0d4-565">*各モバイル デバイスのさまざまなビューを使用します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-565">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="8f0d4-566">更新プログラム、 **TimedOut**次に示すようにコンテンツを表示します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-566">Update the **TimedOut** view content as shown below.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. <span data-ttu-id="8f0d4-567">アプリケーションを実行し、ルートの URL に移動します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-567">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="8f0d4-568">追加したときに、 **Thread.Sleep** 1000 ミリ秒のによって生成された、タイムアウト エラーが表示されます、 **AsyncTimeout**属性し catch、 **HandleError**属性です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-568">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="8f0d4-569">![処理のタイムアウト例外](whats-new-in-aspnet-mvc-4/_static/image37.png "処理のタイムアウト例外")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-569">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="8f0d4-570">*処理のタイムアウト例外*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-570">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="8f0d4-571">Windows Azure Web サイトの次にこのアプリケーションを展開するさらに、[付録 d: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixD)です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-571">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="8f0d4-572">まとめ</span><span class="sxs-lookup"><span data-stu-id="8f0d4-572">Summary</span></span>

<span data-ttu-id="8f0d4-573">このハンズオンのラボ、するした経験でいくつかの新機能の ASP.NET MVC 4 に常駐しています。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-573">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="8f0d4-574">次の概念を説明しています。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-574">The following concepts have been discussed:</span></span>

- <span data-ttu-id="8f0d4-575">ASP.NET MVC プロジェクト テンプレートなど、新しいモバイル アプリケーション プロジェクト テンプレートに機能強化の利用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-575">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="8f0d4-576">モバイル デバイスでの表示を向上させるために、HTML5 ビューポート属性と CSS のメディア クエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-576">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="8f0d4-577">プログレッシブの機能強化とタッチ最適化 web UI を構築するため、jQuery Mobile を使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-577">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="8f0d4-578">Mobile に固有のビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-578">Create mobile-specific views</span></span>
- <span data-ttu-id="8f0d4-579">ビュー スイッチャー コンポーネントを使用して、アプリケーションでのモバイルとデスクトップのビューを切り替える</span><span class="sxs-lookup"><span data-stu-id="8f0d4-579">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="8f0d4-580">タスクのサポートを使用して非同期コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-580">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="8f0d4-581">付録 a: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="8f0d4-581">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="8f0d4-582">コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-582">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="8f0d4-583">ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-583">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="8f0d4-584">![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](whats-new-in-aspnet-mvc-4/_static/image38.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-584">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="8f0d4-585">*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-585">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="8f0d4-586">***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="8f0d4-586">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="8f0d4-587">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-587">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="8f0d4-588">(なし、スペースやハイフン) スニペット名を入力してを起動します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-588">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="8f0d4-589">スニペットの名前に一致する IntelliSense 表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-589">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="8f0d4-590">正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-590">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="8f0d4-591">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-591">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="8f0d4-592">![スニペット名を入力する開始](whats-new-in-aspnet-mvc-4/_static/image39.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-592">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="8f0d4-593">*スニペット名の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-593">*Start typing the snippet name*</span></span>

<span data-ttu-id="8f0d4-594">![Tab キーを押して、強調表示されているスニペットを選択する](whats-new-in-aspnet-mvc-4/_static/image40.png "強調表示されているスニペットを選択するキーを押してタブ")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-594">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="8f0d4-595">*Tab キーを押して、強調表示されているスニペットを選択するには*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-595">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="8f0d4-596">![キーを押して タブで再度と、スニペットが展開](whats-new-in-aspnet-mvc-4/_static/image41.png "キーを押して タブで再度と、スニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-596">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="8f0d4-597">*キーを押して タブで再度と、スニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-597">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="8f0d4-598">***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="8f0d4-598">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="8f0d4-599">コード スニペットを挿入する場所を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-599">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="8f0d4-600">選択**スニペットの挿入**続く**マイ コード スニペット**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-600">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="8f0d4-601">クリックして一覧から、関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-601">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="8f0d4-602">![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](whats-new-in-aspnet-mvc-4/_static/image42.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-602">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="8f0d4-603">*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-603">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="8f0d4-604">![クリックして一覧から、関連するスニペットを選択](whats-new-in-aspnet-mvc-4/_static/image43.png "クリックして一覧から、関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-604">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="8f0d4-605">*クリックして一覧から、関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-605">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="8f0d4-606">付録 b: をインストールする Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="8f0d4-606">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="8f0d4-607">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="8f0d4-607">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="8f0d4-608">次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-608">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="8f0d4-609">移動して[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-609">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="8f0d4-610">または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; *Visual Studio Express 2012 for Web と Windows Azure SDK*&quot;です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-610">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="8f0d4-611">をクリックして**を今すぐインストール**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-611">Click on **Install Now**.</span></span> <span data-ttu-id="8f0d4-612">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-612">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="8f0d4-613">1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-613">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="8f0d4-614">![Visual Studio Express インストール](whats-new-in-aspnet-mvc-4/_static/image44.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-614">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="8f0d4-615">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-615">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="8f0d4-616">すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-616">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="8f0d4-618">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-618">*Accepting the license terms*</span></span>
5. <span data-ttu-id="8f0d4-619">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-619">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="8f0d4-621">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-621">*Installation progress*</span></span>
6. <span data-ttu-id="8f0d4-622">インストールが完了したらをクリックして**完了**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-622">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="8f0d4-624">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-624">*Installation completed*</span></span>
7. <span data-ttu-id="8f0d4-625">をクリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-625">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="8f0d4-626">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-626">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web タイルを](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="8f0d4-628">*VS Express Web タイルを*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-628">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="8f0d4-629">付録 c: インストール WebMatrix 2 と iPhone シミュレーター</span><span class="sxs-lookup"><span data-stu-id="8f0d4-629">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="8f0d4-630">IPhone のシミュレートされたデバイスでは、サイトを実行するには、WebMatrix 拡張機能を使用することができます&quot;(iphone用) Electric Mobile シミュレーター&quot;です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-630">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="8f0d4-631">また、シミュレーターを Visual Studio 2012 から実行する同じ拡張機能を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-631">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="8f0d4-632">タスク 1 - インストール WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="8f0d4-632">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="8f0d4-633">移動して[ [https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169)です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-633">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="8f0d4-634">または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; *WebMatrix 2*&quot;です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-634">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*WebMatrix 2*&quot;.</span></span>
2. <span data-ttu-id="8f0d4-635">をクリックして**を今すぐインストール**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-635">Click on **Install Now**.</span></span> <span data-ttu-id="8f0d4-636">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-636">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="8f0d4-637">1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-637">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="8f0d4-638">![WebMatrix 2 をインストール](whats-new-in-aspnet-mvc-4/_static/image49.png "WebMatrix 2 のインストール")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-638">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="8f0d4-639">*WebMatrix 2 をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-639">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="8f0d4-640">すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-640">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="8f0d4-641">![ライセンス条項に同意](whats-new-in-aspnet-mvc-4/_static/image50.png "ライセンス条項に同意")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-641">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="8f0d4-642">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-642">*Accepting the license terms*</span></span>
5. <span data-ttu-id="8f0d4-643">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-643">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="8f0d4-644">![インストールの進行状況](whats-new-in-aspnet-mvc-4/_static/image51.png "インストールの進行状況")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-644">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="8f0d4-645">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-645">*Installation progress*</span></span>
6. <span data-ttu-id="8f0d4-646">インストールが完了したらをクリックして**完了**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-646">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="8f0d4-647">![インストールが完了しました](whats-new-in-aspnet-mvc-4/_static/image52.png "インストールが完了しました")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-647">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="8f0d4-648">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-648">*Installation completed*</span></span>
7. <span data-ttu-id="8f0d4-649">をクリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-649">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="8f0d4-650">タスク 2 - iPhone シミュレーターの拡張機能をインストールします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-650">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="8f0d4-651">実行**WebMatrix**と既存の Web サイトを開くか、新しいものを作成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-651">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="8f0d4-652">をクリックして、**実行**ボタンから、**ホーム**のリボンし、選択**新規追加**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-652">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="8f0d4-653">![新しい WebMatrix 拡張機能を追加する](whats-new-in-aspnet-mvc-4/_static/image53.png "新しい WebMatrix 拡張機能を追加します。")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-653">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="8f0d4-654">*新しい WebMatrix 拡張機能を追加します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-654">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="8f0d4-655">選択**iPhone シミュレーター**  をクリック**インストール**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-655">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="8f0d4-656">![WebMatrix 拡張機能の参照](whats-new-in-aspnet-mvc-4/_static/image54.png "参照 WebMatrix 拡張機能")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-656">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="8f0d4-657">*WebMatrix 拡張機能の参照*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-657">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="8f0d4-658">パッケージの詳細 をクリックして**インストール**拡張機能のインストールを続行します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-658">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="8f0d4-659">![iPhone シミュレーター拡張子](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone シミュレーターの拡張機能")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-659">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="8f0d4-660">*iPhone シミュレーターの拡張機能*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-660">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="8f0d4-661">読み取りおよび拡張機能の使用許諾契約書に同意します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-661">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="8f0d4-662">![WebMatrix 拡張機能の使用許諾契約書](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix 拡張機能の使用許諾契約書")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-662">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="8f0d4-663">*WebMatrix 拡張機能の使用許諾契約書*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-663">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="8f0d4-664">ここで、iPhone シミュレーター オプションを使用して WebMatrix から Web サイトを実行できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-664">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="8f0d4-665">![IPhone を使用して実行](whats-new-in-aspnet-mvc-4/_static/image57.png "iPhone を使用して実行")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-665">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="8f0d4-666">*IPhone を使用して実行します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-666">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="8f0d4-667">タスク 3 - iPhone シミュレーターを実行する Visual Studio 2012 の構成</span><span class="sxs-lookup"><span data-stu-id="8f0d4-667">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="8f0d4-668">開く**Visual Studio 2012**と任意の Web サイトを開くか、新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-668">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="8f0d4-669">[実行] ボタンの下矢印をクリックし、選択**参照**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-669">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="8f0d4-670">![参照](whats-new-in-aspnet-mvc-4/_static/image58.png "参照")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-670">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="8f0d4-671">*参照します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-671">*Browse with*</span></span>
3. <span data-ttu-id="8f0d4-672">&quot;Browse With&quot;ダイアログ ボックスで、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-672">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="8f0d4-673">&quot;プログラムの追加&quot;ダイアログ ボックスで、次の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-673">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

    - <span data-ttu-id="8f0d4-674">**プログラム**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(パスを適宜更新)*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-674">**Program**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)*</span></span>
    - <span data-ttu-id="8f0d4-675">**引数**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="8f0d4-675">**Arguments**: &quot;1&quot;</span></span>
    - <span data-ttu-id="8f0d4-676">**フレンドリ名**: iPhone シミュレーター</span><span class="sxs-lookup"><span data-stu-id="8f0d4-676">**Friendly name**: iPhone Simulator</span></span>

    <span data-ttu-id="8f0d4-677">![プログラムの追加](whats-new-in-aspnet-mvc-4/_static/image59.png "プログラムの追加")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-677">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

    <span data-ttu-id="8f0d4-678">*使用して参照するプログラムを追加します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-678">*Add program to browse with*</span></span>
5. <span data-ttu-id="8f0d4-679">をクリックして**OK**し、ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-679">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="8f0d4-680">これで、Visual Studio 2012 から iPhone シミュレーターで、Web アプリケーションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-680">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="8f0d4-681">![IPhone シミュレーターと参照](whats-new-in-aspnet-mvc-4/_static/image60.png "iPhone シミュレーターでの参照")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-681">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="8f0d4-682">*IPhone シミュレーターでの参照します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-682">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="8f0d4-683">付録 d: Web Deploy を使用して ASP.NET MVC 4 アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-683">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="8f0d4-684">この付録では、Windows Azure 管理ポータルから新しい web サイトを作成し、Windows Azure によって提供される Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを公開する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-684">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="8f0d4-685">タスク 1 - Windows から新しい Web サイトを作成する Azure ポータル</span><span class="sxs-lookup"><span data-stu-id="8f0d4-685">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="8f0d4-686">移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-686">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0d4-687">Windows Azure 無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増大に応じて拡大縮小できます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-687">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="8f0d4-688">サインアップする[ここ](http://aka.ms/aspnet-hol-azure)です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-688">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="8f0d4-689">![Windows Azure ポータルにログオンする](whats-new-in-aspnet-mvc-4/_static/image61.png "Windows Azure ポータルにログオンします。")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-689">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="8f0d4-690">*Windows Azure 管理ポータルにログオンします。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-690">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="8f0d4-691">をクリックして**新規**コマンド バーでします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-691">Click **New** on the command bar.</span></span>

    <span data-ttu-id="8f0d4-692">![新しい Web サイトを作成する](whats-new-in-aspnet-mvc-4/_static/image62.png "新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-692">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="8f0d4-693">*新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-693">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="8f0d4-694">をクリックして**コンピューティング** | **Web サイト**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-694">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="8f0d4-695">選択し、**簡易作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-695">Then select **Quick Create** option.</span></span> <span data-ttu-id="8f0d4-696">新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-696">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0d4-697">Windows Azure Web サイトは、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-697">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="8f0d4-698">簡易作成 オプションでは、完成した web アプリケーションを Windows Azure Web サイトからポータル外を展開することができます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-698">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="8f0d4-699">データベースを設定するための手順は含まれません。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-699">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="8f0d4-700">![簡易作成を使用して新しい Web サイトを作成する](whats-new-in-aspnet-mvc-4/_static/image63.png "簡易作成を使用して新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-700">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="8f0d4-701">*簡易作成を使用して新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-701">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="8f0d4-702">新しいまで待つ**Web サイト**を作成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-702">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="8f0d4-703">Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-703">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="8f0d4-704">新しい Web サイトが動作していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-704">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="8f0d4-705">![新しい web サイトを参照して](whats-new-in-aspnet-mvc-4/_static/image64.png "新しい web サイトを参照")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-705">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="8f0d4-706">*新しい web サイトを参照*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-706">*Browsing to the new web site*</span></span>

    <span data-ttu-id="8f0d4-707">![Web サイトを実行している](whats-new-in-aspnet-mvc-4/_static/image65.png "を実行している Web サイト")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-707">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="8f0d4-708">*実行している web サイト*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-708">*Web site running*</span></span>
6. <span data-ttu-id="8f0d4-709">ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-709">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="8f0d4-710">![Web サイトの管理ページを開く](whats-new-in-aspnet-mvc-4/_static/image66.png "web サイトの管理ページを開く")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-710">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="8f0d4-711">*Web サイトの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-711">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="8f0d4-712">**ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-712">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0d4-713">*発行プロファイル*すべてのパブリケーションを有効になっているメソッドの各 Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-713">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="8f0d4-714">発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-714">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="8f0d4-715">**Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Windows Azure の web サイトに web アプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-715">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="8f0d4-716">![発行プロファイルを web サイトをダウンロードする](whats-new-in-aspnet-mvc-4/_static/image67.png "発行プロファイルを web サイトをダウンロードします。")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-716">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="8f0d4-717">*発行プロファイルを Web サイトをダウンロードします。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-717">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="8f0d4-718">既知の場所に発行プロファイル ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-718">Download the publish profile file to a known location.</span></span> <span data-ttu-id="8f0d4-719">さらにこの練習では、このファイルを使用して、Visual Studio から web アプリケーション、Windows Azure Web サイトを発行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-719">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="8f0d4-720">![発行プロファイル ファイルを保存](whats-new-in-aspnet-mvc-4/_static/image68.png "発行プロファイルの保存")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-720">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="8f0d4-721">*発行プロファイル ファイルを保存します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-721">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="8f0d4-722">タスク 2 - データベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="8f0d4-722">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="8f0d4-723">アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-723">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="8f0d4-724">SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-724">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="8f0d4-725">SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-725">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="8f0d4-726">SQL データベース サーバーを表示するには、Windows Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-726">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="8f0d4-727">使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-727">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="8f0d4-728">注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-728">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="8f0d4-729">データベースを作成しない、まだと後の段階で作成されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-729">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="8f0d4-730">![SQL データベース サーバーのダッシュ ボード](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL データベース サーバーのダッシュ ボード")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-730">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="8f0d4-731">*SQL データベース サーバーのダッシュ ボード*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-731">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="8f0d4-732">次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-732">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="8f0d4-733">実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**のテキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png)ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-733">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![クライアントの IP アドレスを追加します。](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="8f0d4-735">*クライアントの IP アドレスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-735">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="8f0d4-736">1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-736">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![変更を確認します。](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="8f0d4-738">*変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-738">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="8f0d4-739">タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="8f0d4-739">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="8f0d4-740">ASP.NET MVC 4 ソリューションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-740">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="8f0d4-741">**ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-741">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="8f0d4-742">![アプリケーションの発行](whats-new-in-aspnet-mvc-4/_static/image73.png "アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-742">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="8f0d4-743">*Web サイトの発行*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-743">*Publishing the web site*</span></span>
2. <span data-ttu-id="8f0d4-744">最初の作業を保存した発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-744">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="8f0d4-745">![発行プロファイルをインポートする](whats-new-in-aspnet-mvc-4/_static/image74.png "発行プロファイルをインポートします。")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-745">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="8f0d4-746">*発行プロファイルのインポート*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-746">*Importing publish profile*</span></span>
3. <span data-ttu-id="8f0d4-747">をクリックして**への接続検証**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-747">Click **Validate Connection**.</span></span> <span data-ttu-id="8f0d4-748">検証が完了したらクリックして**次**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-748">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0d4-749">検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-749">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="8f0d4-750">![接続の検証](whats-new-in-aspnet-mvc-4/_static/image75.png "接続の検証")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-750">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="8f0d4-751">*接続の検証*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-751">*Validating connection*</span></span>
4. <span data-ttu-id="8f0d4-752">**設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-752">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="8f0d4-753">![Web 配置の構成](whats-new-in-aspnet-mvc-4/_static/image76.png "Web 配置の構成")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-753">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="8f0d4-754">*Web 配置の構成*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-754">*Web deploy configuration*</span></span>
5. <span data-ttu-id="8f0d4-755">次のように、データベースの接続を構成します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-755">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="8f0d4-756">**サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:*プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-756">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="8f0d4-757">**ユーザー名**サーバー管理者のログイン名を入力します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-757">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="8f0d4-758">**パスワード**サーバー管理者のログイン パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-758">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="8f0d4-759">たとえば、新しいデータベース名を入力: *MVC4SampleDB*です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-759">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="8f0d4-760">![対象の接続文字列を構成する](whats-new-in-aspnet-mvc-4/_static/image77.png "対象の接続文字列を構成します。")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-760">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="8f0d4-761">*対象の接続文字列を構成します。*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-761">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="8f0d4-762">次に、 **[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-762">Then click **OK**.</span></span> <span data-ttu-id="8f0d4-763">データベースをクリックを作成するように求められたら**はい**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-763">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="8f0d4-764">![データベースを作成する](whats-new-in-aspnet-mvc-4/_static/image78.png "データベース文字列を作成します。")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-764">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="8f0d4-765">*データベースの作成*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-765">*Creating the database*</span></span>
7. <span data-ttu-id="8f0d4-766">接続の既定のテキスト ボックス内では、Windows azure SQL データベースへの接続に使用する接続文字列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-766">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="8f0d4-767">その後、 **[次へ]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-767">Then click **Next**.</span></span>

    <span data-ttu-id="8f0d4-768">![SQL データベースを指す接続文字列](whats-new-in-aspnet-mvc-4/_static/image79.png "SQL データベースを指す接続文字列")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-768">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="8f0d4-769">*SQL データベースを指す接続文字列*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-769">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="8f0d4-770">**プレビュー** ] ページで [**発行**です。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-770">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="8f0d4-771">![Web アプリケーションの発行](whats-new-in-aspnet-mvc-4/_static/image80.png "web アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-771">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="8f0d4-772">*Web アプリケーションの発行*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-772">*Publishing the web application*</span></span>
9. <span data-ttu-id="8f0d4-773">発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="8f0d4-773">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="8f0d4-774">![アプリケーションが Windows Azure に発行](whats-new-in-aspnet-mvc-4/_static/image81.png "アプリケーションが Windows Azure に発行")</span><span class="sxs-lookup"><span data-stu-id="8f0d4-774">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="8f0d4-775">*Windows Azure に発行されるアプリケーション*</span><span class="sxs-lookup"><span data-stu-id="8f0d4-775">*Application published to Windows Azure*</span></span>
