---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: ASP.NET MVC 4 カスタム アクション フィルター |Microsoft Docs
author: rick-anderson
description: ASP.NET MVC には前に、または後、アクション メソッドが呼び出されるフィルター処理のロジックを実行するためのアクション フィルターが用意されています。 アクション フィルターは、カスタム属性 tha.
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 0170fda6849c1dfb53b44908ea55ba2cad0dd067
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826369"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="2d1d8-104">ASP.NET MVC 4 カスタム アクション フィルター</span><span class="sxs-lookup"><span data-stu-id="2d1d8-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="2d1d8-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="2d1d8-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="2d1d8-106">Web のキャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="2d1d8-107">ASP.NET MVC には前に、または後、アクション メソッドが呼び出されるフィルター処理のロジックを実行するためのアクション フィルターが用意されています。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="2d1d8-108">アクション フィルターは、コント ローラーのアクション メソッドへの事前アクションと事後アクションの動作を追加する宣言型の手段を提供するカスタム属性です。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="2d1d8-109">このハンズオン ラボでは、コント ローラーの要求をキャッチし、データベース テーブルに、サイトのアクティビティ ログに記録する MvcMusicStore ソリューションにカスタム アクション フィルター属性を作成します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="2d1d8-110">挿入によって、コント ローラーまたはアクションに、ログ記録フィルターを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="2d1d8-111">最後に、訪問者の一覧を示すログ ビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="2d1d8-112">このハンズオン ラボは、の基本的な知識があることを前提**ASP.NET MVC**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="2d1d8-113">使用していない場合**ASP.NET MVC**前を経由することを勧めします。 **ASP.NET MVC 4 の基礎**ハンズオン ラボ。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="2d1d8-114">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="2d1d8-115">このラボに固有のプロジェクトは、「 [ASP.NET MVC 4 カスタム アクション フィルター](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters)します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="2d1d8-116">目的</span><span class="sxs-lookup"><span data-stu-id="2d1d8-116">Objectives</span></span>

<span data-ttu-id="2d1d8-117">このハンズオン ラボでは、学習する方法。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="2d1d8-118">フィルター処理機能を拡張するカスタム アクション フィルター属性を作成します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="2d1d8-119">特定のレベルの挿入によって、カスタム フィルター属性を適用します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="2d1d8-120">カスタム アクション フィルターをグローバルに登録します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="2d1d8-121">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="2d1d8-121">Prerequisites</span></span>

<span data-ttu-id="2d1d8-122">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="2d1d8-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="2d1d8-124">セットアップ</span><span class="sxs-lookup"><span data-stu-id="2d1d8-124">Setup</span></span>

<span data-ttu-id="2d1d8-125">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="2d1d8-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="2d1d8-126">便宜上、このラボに管理するコードの多くは、Visual Studio コード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="2d1d8-127">実行コード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="2d1d8-128">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="2d1d8-129">演習</span><span class="sxs-lookup"><span data-stu-id="2d1d8-129">Exercises</span></span>

<span data-ttu-id="2d1d8-130">次の演習では、このハンズオン ラボから成ります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="2d1d8-131">手順 1: アクションのログ記録</span><span class="sxs-lookup"><span data-stu-id="2d1d8-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="2d1d8-132">手順 2: 複数のアクション フィルターの管理</span><span class="sxs-lookup"><span data-stu-id="2d1d8-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="2d1d8-133">この演習の所要時間を推定: **30 分**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="2d1d8-134">各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="2d1d8-135">作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="2d1d8-136">手順 1: アクションのログ記録</span><span class="sxs-lookup"><span data-stu-id="2d1d8-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="2d1d8-137">この演習では、ASP.NET MVC 4 のフィルター プロバイダーを使用してカスタム アクションのログ フィルターを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="2d1d8-138">目的のためには、選択したコント ローラーで、すべてのアクティビティを記録する MusicStore サイトにログ記録フィルターが適用されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="2d1d8-139">フィルターを拡張**ActionFilterAttributeClass**オーバーライドと**OnActionExecuting**メソッドを各要求をキャッチし、ログ記録アクションを実行します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="2d1d8-140">HTTP 要求に関するコンテキスト情報、メソッド、結果、およびパラメーターを実行することが ASP.NET MVC によって**ActionExecutingContext**クラス**します。**</span><span class="sxs-lookup"><span data-stu-id="2d1d8-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="2d1d8-141">ASP.NET MVC 4 では、カスタム フィルターを作成せずに使用できる既定のフィルター プロバイダーもあります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="2d1d8-142">ASP.NET MVC 4 では、次の種類のフィルターを提供します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="2d1d8-143">**承認**認証の実行や要求のプロパティの検証などのアクション メソッドを実行するかどうかに関するセキュリティ上の決定は、これをフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="2d1d8-144">**アクション**フィルターで、アクション メソッドの実行をラップします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="2d1d8-145">このフィルターは、アクション メソッドに追加のデータの提供、戻り値の検査、アクション メソッドの実行の取り消しなど、追加の処理を実行できます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="2d1d8-146">**結果**フィルターで、ActionResult オブジェクトの実行をラップします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="2d1d8-147">このフィルターは、HTTP 応答の変更など、結果の追加の処理を実行できます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="2d1d8-148">**例外**フィルターで、結果の実行で始まり、承認フィルターを使用して、アクション メソッドでどこかにスローされた未処理の例外がある場合に実行されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="2d1d8-149">例外フィルターは、ログ記録やエラー ページの表示などのタスクで使用できます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="2d1d8-150">フィルター プロバイダーの詳細についてはこの MSDN リンクを参照してください: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx))。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="2d1d8-151">MVC のミュージック ストア アプリケーションのログ記録機能について</span><span class="sxs-lookup"><span data-stu-id="2d1d8-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="2d1d8-152">このミュージック ストア ソリューションが、サイトのログ記録を新しいデータ モデル テーブル**ActionLog**、以下のフィールド。 要求、呼び出されるアクション、クライアントの ip アドレス、およびタイムスタンプを受信したコント ローラーの名前。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="2d1d8-153">![データ モデル。ActionLog テーブルです。](aspnet-mvc-4-custom-action-filters/_static/image1.png "データ モデル。ActionLog テーブルです。")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="2d1d8-154">*データ モデル - ActionLog テーブル*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="2d1d8-155">ソリューションにあることができますが、操作ログ用の ASP.NET MVC ビューを提供します**MvcMusicStores/ビュー/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="2d1d8-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="2d1d8-156">![アクション ログ ビュー](aspnet-mvc-4-custom-action-filters/_static/image2.png "アクション ログの表示")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="2d1d8-157">*アクション ログの表示*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-157">*Action Log view*</span></span>

<span data-ttu-id="2d1d8-158">この構造は、すべての作業が注目コント ローラーの要求を中断することと、カスタム フィルター処理を使用して、ログ記録を実行します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="2d1d8-159">タスク 1 - コント ローラーの要求をキャッチするカスタム フィルターを作成します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="2d1d8-160">このタスクは、ログのロジックを含むカスタム フィルター属性クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="2d1d8-161">目的のためには、ASP.NET MVC を拡張する**ActionFilterAttribute**クラスとそのインターフェイスを実装**IActionFilter**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="2d1d8-162">**ActionFilterAttribute**属性のすべてのフィルターの基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="2d1d8-163">後、コント ローラー アクションの実行前に特定のロジックを実行する次のメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="2d1d8-164">**OnActionExecuting**(ActionExecutingContext filterContext): 直前のアクション メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="2d1d8-165">**OnActionExecuted**(ActionExecutedContext filterContext): (ビューのレンダリング) の前に、結果が実行される前に、アクション メソッドが呼び出されるとします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="2d1d8-166">**OnResultExecuting**(ResultExecutingContext filterContext): (ビューのレンダリング) の前に、結果が実行される前にだけです。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="2d1d8-167">**OnResultExecuted**(ResultExecutedContext filterContext): (後のビューが表示される)、結果の実行後にします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="2d1d8-168">これらのメソッドをオーバーライドする派生クラスに、独自のフィルター処理のコードを実行できます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="2d1d8-169">開く、**開始**ソリューションがある**\Source\Ex01-LoggingActions\Begin**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="2d1d8-170">続行する前に、いくつか不足している NuGet パッケージをダウンロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="2d1d8-171">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="2d1d8-172">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="2d1d8-173">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="2d1d8-174">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="2d1d8-175">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="2d1d8-176">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="2d1d8-177">詳細については、この記事を参照してください: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="2d1d8-178">新しい c# クラスを追加、**フィルター**フォルダーと名前を付けます*CustomActionFilter.cs*します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="2d1d8-179">このフォルダーでは、すべてのカスタム フィルターを格納します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="2d1d8-180">開いている**CustomActionFilter.cs**への参照を追加および**System.Web.Mvc**と**MvcMusicStore.Models**名前空間。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="2d1d8-181">(コード スニペット - *ASP.NET MVC 4 カスタム アクション フィルター - Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="2d1d8-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="2d1d8-182">継承、 **CustomActionFilter**クラス**ActionFilterAttribute**行い**CustomActionFilter**クラス実装**IActionFilter**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="2d1d8-183">ように**CustomActionFilter**クラス、メソッドをオーバーライド**OnActionExecuting**フィルターの実行ログを記録するために必要なロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="2d1d8-184">これを行うには、内の次の強調表示されたコードを追加**CustomActionFilter**クラス。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="2d1d8-185">(コード スニペット - *ASP.NET MVC 4 カスタム アクション フィルター - Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="2d1d8-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="2d1d8-186">**OnActionExecuting**メソッドを使用して**Entity Framework**新しい ActionLog レジスタを追加します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="2d1d8-187">作成し、新しいエンティティ インスタンスからコンテキスト情報を設定**filterContext**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="2d1d8-188">詳細をご覧ください**ControllerContext**でクラス[msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="2d1d8-189">タスク 2 - ストアのコント ローラー クラスにコードのインターセプターを挿入します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="2d1d8-190">このタスクでは、すべてのログに記録するコント ローラー アクションとコント ローラー クラスに挿入することによってカスタム フィルターを追加します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="2d1d8-191">この演習では、ストアのコント ローラー クラスは、ログがあります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="2d1d8-192">メソッド**OnActionExecuting**から**ActionLogFilterAttribute**挿入された要素が呼び出されたときに、カスタム フィルターが実行されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="2d1d8-193">特定のコント ローラー メソッドをインターセプトすることもできます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="2d1d8-194">開く、 **StoreController**で**MvcMusicStore\Controllers**への参照を追加し、**フィルター**名前空間。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="2d1d8-195">カスタム フィルターを挿入**CustomActionFilter**に**StoreController**クラスを追加して **[CustomActionFilter]** クラス宣言の前に、の属性。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="2d1d8-196">フィルターは、コント ローラー クラスに挿入されますが、ときに、すべてのアクションも挿入されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="2d1d8-197">一連のアクションに対してのみフィルターを適用するには、挿入する必要があります **[CustomActionFilter]** それらのいずれか。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="2d1d8-198">タスク 3 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="2d1d8-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="2d1d8-199">このタスクでは、ログ記録フィルターが動作しているかをテストします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="2d1d8-200">アプリケーションを起動して、ストアにアクセスして、ログに記録されたアクティビティを確認します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="2d1d8-201">**F5** キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="2d1d8-202">参照する **/ActionLog**ログ ビューの初期状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="2d1d8-203">![ページのアクティビティの前にトラッカーの状態をログ記録](aspnet-mvc-4-custom-action-filters/_static/image3.png "ページ アクティビティの前にトラッカーの状態をログ記録")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="2d1d8-204">*ページのアクティビティの前にログの追跡ツールの状態*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="2d1d8-205">既定では、1 つの項目 メニューの既存のジャンルを取得するときに生成された常に表示されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="2d1d8-206">簡略化のためをクリーンアップいたします、 **ActionLog**たびに、アプリケーションが実行されるは、それぞれ特定のタスクの検証のログのみが表示するためのテーブルします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="2d1d8-207">次のコードから削除する必要があります、**セッション\_開始**メソッド (で、 **Global.asax**クラス)、ストア内で実行されるすべてのアクションの履歴ログを保存するために、コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="2d1d8-208">いずれかを**ジャンル** メニューから使用可能なアルバムを閲覧など、操作を実行したりします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="2d1d8-209">参照 **/ActionLog**ログが空のキーを押して**F5**ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="2d1d8-210">訪問者が追跡されなかったことを確認します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="2d1d8-211">![アクティビティ ログに記録を含むアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image4.png "ログに記録するアクティビティを含むアクション ログ")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="2d1d8-212">*アクティビティ ログに記録を含むアクション ログ*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="2d1d8-213">手順 2: 複数のアクション フィルターの管理</span><span class="sxs-lookup"><span data-stu-id="2d1d8-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="2d1d8-214">この演習では、StoreController クラスに 2 つ目のカスタム操作フィルターを追加し、両方のフィルターを実行する特定の順序を定義します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="2d1d8-215">フィルターをグローバルに登録するコードが更新されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="2d1d8-216">フィルターの実行順序を定義するときに考慮するさまざまなオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="2d1d8-217">たとえば、注文プロパティとフィルターのスコープ:</span><span class="sxs-lookup"><span data-stu-id="2d1d8-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="2d1d8-218">定義することができます、**スコープ**フィルターごとに、たとえば、する可能性がありますスコープ内で実行するすべてのアクション フィルター、**コント ローラーのスコープ**とで実行するすべての承認フィルター**グローバル スコープ**.</span><span class="sxs-lookup"><span data-stu-id="2d1d8-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="2d1d8-219">スコープは定義されている実行順序があります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="2d1d8-220">また、各アクション フィルターでは、フィルターのスコープ内での実行順序を決定するために使用される順序プロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="2d1d8-221">カスタム アクション フィルターの実行順序の詳細については、この MSDN の記事をご覧ください。 ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx))。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="2d1d8-222">タスク 1: 新しいカスタム アクション フィルターの作成</span><span class="sxs-lookup"><span data-stu-id="2d1d8-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="2d1d8-223">このタスクでには、フィルターの実行順序を管理する方法を学習、StoreController クラスに挿入する新しいカスタム アクション フィルター作成されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="2d1d8-224">開く、**開始**ソリューションがある**\Source\Ex02-ManagingMultipleActionFilters\Begin**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="2d1d8-225">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="2d1d8-226">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="2d1d8-227">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="2d1d8-228">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="2d1d8-229">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="2d1d8-230">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="2d1d8-231">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="2d1d8-232">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="2d1d8-233">詳細については、この記事を参照してください: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="2d1d8-234">新しい c# クラスを追加、**フィルター**フォルダーと名前を付けます*MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="2d1d8-235">開いている**MyNewCustomActionFilter.cs**への参照を追加および**System.Web.Mvc**と**MvcMusicStore.Models**名前空間。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="2d1d8-236">(コード スニペット - *ASP.NET MVC 4 カスタム アクション フィルター - Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="2d1d8-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="2d1d8-237">既定のクラス宣言を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="2d1d8-238">(コード スニペット - *ASP.NET MVC 4 カスタム アクション フィルター - Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="2d1d8-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="2d1d8-239">このカスタム アクション フィルターは、前の演習で作成したものよりもほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="2d1d8-240">主な違いは、持っていること、 *&quot;ログに記録して&quot;* 送信先のフィルターを識別するためにこの新しいクラスの名前で更新された属性には、ログが登録されています。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="2d1d8-241">タスク 2: StoreController クラスに新しいコード インターセプターを挿入します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="2d1d8-242">このタスクでは、StoreController クラスに新しいカスタム フィルターを追加し、どのように両方のフィルターが連携することを確認するソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="2d1d8-243">開く、 **StoreController**クラスがある**MvcMusicStore\Controllers**し、新しいカスタム フィルターを注入**MyNewCustomActionFilter**に**StoreController**のようなクラスは、次のコードを示します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="2d1d8-244">次に、これら 2 つのカスタム アクション フィルターの動作を確認するためにアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="2d1d8-245">これを行うには、キーを押して**F5**と、アプリケーションが開始されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="2d1d8-246">参照する **/ActionLog**ログ ビューの初期状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="2d1d8-247">![ページのアクティビティの前にトラッカーの状態をログ記録](aspnet-mvc-4-custom-action-filters/_static/image5.png "ページ アクティビティの前にトラッカーの状態をログ記録")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="2d1d8-248">*ページのアクティビティの前にログの追跡ツールの状態*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="2d1d8-249">いずれかを**ジャンル** メニューから使用可能なアルバムを閲覧など、操作を実行したりします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="2d1d8-250">この時間を確認します。2 回、訪問者が追跡されていた: 後で追加した各カスタム アクション フィルター、 **StorageController**クラス。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="2d1d8-251">![アクティビティ ログに記録を含むアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image6.png "ログに記録するアクティビティを含むアクション ログ")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="2d1d8-252">*アクティビティ ログに記録を含むアクション ログ*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="2d1d8-253">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="2d1d8-254">タスク 3: フィルターの順序を管理します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="2d1d8-255">このタスクでは、順序のプロパティを使用して、フィルターの実行順序を管理する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-255">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="2d1d8-256">オープン、 **StoreController**クラスがある**MvcMusicStore\Controllers**を指定し、**順序**などの両方のフィルター プロパティ下図のようにします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="2d1d8-257">ここで、その順序のプロパティの値に応じて、フィルターを実行する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="2d1d8-258">表示されますが、最小の順序の値でフィルター (**CustomActionFilter**) が実行される最初の 1 つ。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="2d1d8-259">キーを押して**F5**と、アプリケーションが開始されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="2d1d8-260">参照する **/ActionLog**ログ ビューの初期状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="2d1d8-261">![ページのアクティビティの前にトラッカーの状態をログ記録](aspnet-mvc-4-custom-action-filters/_static/image7.png "ページ アクティビティの前にトラッカーの状態をログ記録")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="2d1d8-262">*ページのアクティビティの前にログの追跡ツールの状態*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="2d1d8-263">いずれかを**ジャンル** メニューから使用可能なアルバムを閲覧など、操作を実行したりします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="2d1d8-264">フィルターの順序の値順に、今回は、訪問者が追跡されなかったチェック: **CustomActionFilter**ログの最初。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="2d1d8-265">![アクティビティ ログに記録を含むアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image8.png "ログに記録するアクティビティを含むアクション ログ")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="2d1d8-266">*アクティビティ ログに記録を含むアクション ログ*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="2d1d8-267">ここで、フィルターの順序の値を更新し、ログ記録の順序を変更する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="2d1d8-268">**StoreController**クラスを次に示すようにフィルターの順序の値を更新します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="2d1d8-269">キーを押してアプリケーションをもう一度実行**F5**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="2d1d8-270">いずれかを**ジャンル** メニューから使用可能なアルバムを閲覧など、操作を実行したりします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="2d1d8-271">によって作成されたログを現時点では、ことを確認**MyNewCustomActionFilter**最初にフィルターが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="2d1d8-272">![アクティビティ ログに記録を含むアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image9.png "ログに記録するアクティビティを含むアクション ログ")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="2d1d8-273">*アクティビティ ログに記録を含むアクション ログ*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="2d1d8-274">タスク 4: をグローバルにフィルターを登録します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="2d1d8-275">このタスクでは、新しいフィルターを登録するソリューションを更新します (**MyNewCustomActionFilter**) グローバル フィルターとして。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="2d1d8-276">これにより、すべてのアクション実行、アプリケーションと、前のタスクのように StoreController ものだけでなくによってトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-276">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="2d1d8-277">**StoreController**クラスを削除 **[MyNewCustomActionFilter]** 属性と順序プロパティから **[CustomActionFilter]** します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="2d1d8-278">次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="2d1d8-279">開いている**Global.asax**ファイルし、検索、**アプリケーション\_開始**メソッド。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="2d1d8-280">呼び出すことによってグローバル フィルターを登録するたびに、アプリケーションが開始されることに注意してください**RegisterGlobalFilters**メソッド内で**FilterConfig**クラス。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="2d1d8-281">![Global.asax でグローバル フィルターを登録する](aspnet-mvc-4-custom-action-filters/_static/image10.png "Global.asax でグローバル フィルターを登録します。")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="2d1d8-282">*Global.asax でグローバル フィルターを登録します。*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="2d1d8-283">開いている**FilterConfig.cs**内ファイル**アプリ\_開始**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="2d1d8-284">System.Web.Mvc; を使用してへの参照を追加します。MvcMusicStore.Filters; を使用します。名前空間。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="2d1d8-285">Update **RegisterGlobalFilters**メソッドは、カスタム フィルターを追加します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="2d1d8-286">これを行うには、強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="2d1d8-287">キーを押してアプリケーションを実行**F5**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="2d1d8-288">いずれかを**ジャンル** メニューから使用可能なアルバムを閲覧など、操作を実行したりします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="2d1d8-289">チェックなった **[MyNewCustomActionFilter]** すぎる HomeController と ActionLogController で挿入されるは。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="2d1d8-290">![アクティビティ ログに記録を含むアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image11.png "ログに記録するアクティビティを含むアクション ログ")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="2d1d8-291">*グローバル アクティビティをログに記録を含むアクション ログ*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="2d1d8-292">また、Windows Azure Web サイトの次に、このアプリケーションを展開できます[付録 b: 発行 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="2d1d8-293">まとめ</span><span class="sxs-lookup"><span data-stu-id="2d1d8-293">Summary</span></span>

<span data-ttu-id="2d1d8-294">このハンズオン ラボは、カスタム アクションを実行するアクション フィルターを拡張する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="2d1d8-295">任意のフィルター、ページのコント ローラーを挿入する方法についても説明しました。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="2d1d8-296">次の概念を使用しました。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-296">The following concepts were used:</span></span>

- <span data-ttu-id="2d1d8-297">ASP.NET MVC ActionFilterAttribute クラスを使用してカスタム アクション フィルターを作成する方法</span><span class="sxs-lookup"><span data-stu-id="2d1d8-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="2d1d8-298">ASP.NET MVC コント ローラーにフィルターを挿入する方法</span><span class="sxs-lookup"><span data-stu-id="2d1d8-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="2d1d8-299">フィルターの順序のプロパティを使用して順序を管理する方法</span><span class="sxs-lookup"><span data-stu-id="2d1d8-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="2d1d8-300">フィルターをグローバルに登録する方法</span><span class="sxs-lookup"><span data-stu-id="2d1d8-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="2d1d8-301">付録 a: Installing Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="2d1d8-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="2d1d8-302">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="2d1d8-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="2d1d8-303">次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="2d1d8-304">移動して[ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-304">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="2d1d8-305">または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="2d1d8-306">をクリックして**を今すぐインストール**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-306">Click on **Install Now**.</span></span> <span data-ttu-id="2d1d8-307">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="2d1d8-308">1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="2d1d8-309">![Visual Studio Express のインストール](aspnet-mvc-4-custom-action-filters/_static/image12.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="2d1d8-310">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="2d1d8-311">すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="2d1d8-313">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="2d1d8-314">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-314">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="2d1d8-316">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-316">*Installation progress*</span></span>
6. <span data-ttu-id="2d1d8-317">インストールが完了したら、クリックして**完了**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-317">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="2d1d8-319">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-319">*Installation completed*</span></span>
7. <span data-ttu-id="2d1d8-320">クリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="2d1d8-321">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web のタイル](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="2d1d8-323">*VS Express for Web のタイル*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="2d1d8-324">付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="2d1d8-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="2d1d8-325">この付録では、Windows Azure 管理ポータルから新しい web サイトを作成して Windows Azure によって提供される、Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを発行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="2d1d8-326">タスク 1 - 新しい Web サイトを作成する、Windows から Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2d1d8-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="2d1d8-327">移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2d1d8-328">Windows Azure を無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増加に応じてスケールできます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="2d1d8-329">サインアップする[ここ](http://aka.ms/aspnet-hol-azure)します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-329">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="2d1d8-330">![Windows Azure ポータルにログオン](aspnet-mvc-4-custom-action-filters/_static/image17.png "Windows Azure ポータルにログオン")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="2d1d8-331">*Windows Azure 管理ポータルにログオン*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="2d1d8-332">クリックして**新規**コマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="2d1d8-333">![新しい Web サイトを作成する](aspnet-mvc-4-custom-action-filters/_static/image18.png "新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="2d1d8-334">*新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="2d1d8-335">クリックして**コンピューティング** | **Web サイト**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="2d1d8-336">選び**簡易作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="2d1d8-337">新しい web サイトの URL を指定し、をクリックして**Web サイトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2d1d8-338">Windows Azure の Web サイトには、制御および管理できる、クラウドで実行されている web アプリケーション、ホストです。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="2d1d8-339">簡易作成 オプションを使用すると、完成した web アプリケーションを Windows Azure の Web サイトから、ポータル外からデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="2d1d8-340">データベースを設定するための手順は含まれません。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="2d1d8-341">![簡易作成を使用して新しい Web サイトを作成する](aspnet-mvc-4-custom-action-filters/_static/image19.png "簡易作成を使用して新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="2d1d8-342">*簡易作成を使用して新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="2d1d8-343">新しいまで待つ**Web サイト**が作成されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="2d1d8-344">Web サイトを作成した後は、下のリンクをクリックして、 **URL**列。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="2d1d8-345">新しい Web サイトが動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="2d1d8-346">![新しい web サイトを参照](aspnet-mvc-4-custom-action-filters/_static/image20.png "新しい web サイトを参照")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="2d1d8-347">*新しい web サイトを参照*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="2d1d8-348">![Web サイトが実行されている](aspnet-mvc-4-custom-action-filters/_static/image21.png "実行している Web サイト")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="2d1d8-349">*実行している web サイト*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-349">*Web site running*</span></span>
6. <span data-ttu-id="2d1d8-350">ポータルに戻り、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="2d1d8-351">![Web サイトの管理ページを開く](aspnet-mvc-4-custom-action-filters/_static/image22.png "web サイトの管理ページを開く")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="2d1d8-352">*Web サイトの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="2d1d8-353">**ダッシュ ボード**ページの**概要**セクションで、、**発行プロファイルのダウンロード**リンク。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2d1d8-354">*発行プロファイル*すべてのパブリケーションを有効になっているメソッドごとに Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="2d1d8-355">発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベースの文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="2d1d8-356">**Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートと、発行プロファイルのこれらのプログラムの構成を自動化するにはWindows Azure の web サイトへの web アプリケーションを発行しています。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="2d1d8-357">![発行プロファイルのダウンロード web サイト](aspnet-mvc-4-custom-action-filters/_static/image23.png "発行プロファイルの web サイトのダウンロード")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="2d1d8-358">*発行プロファイルの Web サイトのダウンロード*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="2d1d8-359">既知の場所に発行プロファイル ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="2d1d8-360">さらにこの演習ではこのファイルを使用して、Visual Studio から Windows Azure Web サイトに web アプリケーションを発行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="2d1d8-361">![発行プロファイル ファイルを保存する](aspnet-mvc-4-custom-action-filters/_static/image24.png "発行プロファイルを保存しています")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="2d1d8-362">*発行プロファイル ファイルを保存しています*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="2d1d8-363">タスク 2 - データベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="2d1d8-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="2d1d8-364">アプリケーションが SQL Server を使用する場合のデータベースは SQL Database サーバーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="2d1d8-365">SQL Server を使用しない単純なアプリケーションをデプロイする場合は、このタスクをスキップする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="2d1d8-366">SQL Database サーバーは、アプリケーション データベースを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="2d1d8-367">Windows Azure 管理ポータル内のサブスクリプションから SQL Database サーバーを表示する**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="2d1d8-368">使用して 1 つを作成するには作成したサーバーがない、**追加**コマンド バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="2d1d8-369">メモ、**サーバー名および URL、管理者のログイン名とパスワード**次のタスクで使用すると、します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="2d1d8-370">データベースを作成しない、まだ、後の段階でそれが作成されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="2d1d8-371">![SQL データベース サーバーのダッシュ ボード](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL データベース サーバーのダッシュ ボード")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="2d1d8-372">*SQL データベース サーバーのダッシュ ボード*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="2d1d8-373">次のタスクでは、サーバーの一覧で、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="2d1d8-374">次のようにクリックします**構成**、から IP アドレスを選択して**現在のクライアント IP アドレス**で貼り付けます、**開始 IP アドレス**と**終了 IP アドレス。** テキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png)ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![クライアント IP アドレスを追加します。](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="2d1d8-376">*クライアント IP アドレスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="2d1d8-377">1 回、**クライアント IP アドレス**が許可された IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![変更を確認します。](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="2d1d8-379">*変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="2d1d8-380">タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="2d1d8-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="2d1d8-381">ASP.NET MVC 4 のソリューションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="2d1d8-382">**ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="2d1d8-383">![アプリケーションの発行](aspnet-mvc-4-custom-action-filters/_static/image29.png "アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="2d1d8-384">*Web サイトの発行*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="2d1d8-385">最初のタスクで保存した発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="2d1d8-386">![発行プロファイルをインポートする](aspnet-mvc-4-custom-action-filters/_static/image30.png "発行プロファイルをインポートします。")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="2d1d8-387">*発行プロファイルのインポート*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="2d1d8-388">クリックして**接続の検証**です。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-388">Click **Validate Connection**.</span></span> <span data-ttu-id="2d1d8-389">検証が完了したら、クリックして**次**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2d1d8-390">接続の検証ボタンの横に表示される緑のチェック マークが表示されたら、検証が完了しました。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="2d1d8-391">![接続の検証](aspnet-mvc-4-custom-action-filters/_static/image31.png "接続の検証")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="2d1d8-392">*接続の検証*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-392">*Validating connection*</span></span>
4. <span data-ttu-id="2d1d8-393">**設定**ページの**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックします (つまり**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="2d1d8-394">![Web 配置の構成](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web 配置の構成")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="2d1d8-395">*Web 配置の構成*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="2d1d8-396">データベース接続を次のように構成します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="2d1d8-397">**サーバー名**SQL Database サーバーの URL を使用して、入力、 *tcp:* プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="2d1d8-398">**ユーザー名**サーバー管理者ログイン名を入力します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="2d1d8-399">**パスワード**サーバー管理者ログイン パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="2d1d8-400">新しいデータベース名を入力します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-400">Type a new database name.</span></span>

     <span data-ttu-id="2d1d8-401">![ターゲットの接続文字列を構成する](aspnet-mvc-4-custom-action-filters/_static/image33.png "ターゲットの接続文字列を構成します。")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="2d1d8-402">*ターゲットの接続文字列を構成します。*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="2d1d8-403">次に、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-403">Then click **OK**.</span></span> <span data-ttu-id="2d1d8-404">データベースのクリックを作成するように求められたら**はい**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="2d1d8-405">![データベースを作成する](aspnet-mvc-4-custom-action-filters/_static/image34.png "データベース文字列を作成します。")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="2d1d8-406">*データベースの作成*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-406">*Creating the database*</span></span>
7. <span data-ttu-id="2d1d8-407">Windows azure SQL Database への接続に使用する接続文字列は、接続の既定のテキスト ボックス内に表示されます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="2d1d8-408">その後、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-408">Then click **Next**.</span></span>

    <span data-ttu-id="2d1d8-409">![SQL データベースを指す接続文字列](aspnet-mvc-4-custom-action-filters/_static/image35.png "SQL データベースを指す接続文字列")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="2d1d8-410">*SQL データベースを指す接続文字列*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="2d1d8-411">**プレビュー** ] ページで [**発行**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="2d1d8-412">![Web アプリケーションの発行](aspnet-mvc-4-custom-action-filters/_static/image36.png "web アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="2d1d8-413">*Web アプリケーションの発行*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="2d1d8-414">発行プロセスが完了すると、既定のブラウザーが発行した web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="2d1d8-415">付録 c: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="2d1d8-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="2d1d8-416">コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="2d1d8-417">ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="2d1d8-418">![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-custom-action-filters/_static/image37.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="2d1d8-419">*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="2d1d8-420">***キーボード (c# のみ) を使用するコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="2d1d8-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="2d1d8-421">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="2d1d8-422">スニペットの名前 (なし、スペースやハイフン) の入力を開始します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="2d1d8-423">スニペットの名前に一致する IntelliSense の表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="2d1d8-424">適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="2d1d8-425">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="2d1d8-426">![スニペットの名前の入力を開始](aspnet-mvc-4-custom-action-filters/_static/image38.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="2d1d8-427">*スニペットの名前の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="2d1d8-428">![強調表示されているスニペットを選択して Tab キーを押して](aspnet-mvc-4-custom-action-filters/_static/image39.png "キーを押してタブが強調表示されているスニペットを選択するには")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="2d1d8-429">*Tab キーを押して、強調表示されているスニペットを選択します*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="2d1d8-430">![キーを押して タブで再度とスニペットが展開](aspnet-mvc-4-custom-action-filters/_static/image40.png "キーを押して タブで再度とスニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="2d1d8-431">*キーを押して タブで再度とスニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="2d1d8-432">***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加する***1。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="2d1d8-433">コード スニペットを挿入するを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="2d1d8-434">選択**スニペットの挿入**続けて**マイ コード スニペット**します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="2d1d8-435">クリックして、一覧から関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="2d1d8-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="2d1d8-436">![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](aspnet-mvc-4-custom-action-filters/_static/image41.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="2d1d8-437">*コード スニペットを挿入して、スニペットの挿入先の選択します。*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="2d1d8-438">![クリックして、一覧から関連するスニペットを選択](aspnet-mvc-4-custom-action-filters/_static/image42.png "クリックして、一覧から関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="2d1d8-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="2d1d8-439">*クリックして、一覧から関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="2d1d8-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
