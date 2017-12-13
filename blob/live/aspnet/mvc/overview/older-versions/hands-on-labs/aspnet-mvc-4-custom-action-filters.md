---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: "ASP.NET MVC 4 のカスタム アクション フィルター |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET MVC には、事前にまたはアクション メソッドが呼び出された後にフィルタ リング ロジックを実行するためのアクション フィルターが用意されています。 アクション フィルターは、カスタム属性 tha しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 6362f0506934ca3b3cc86e1a927af75e7bc4e1d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="66173-104">ASP.NET MVC 4 のカスタム アクション フィルター</span><span class="sxs-lookup"><span data-stu-id="66173-104">ASP.NET MVC 4 Custom Action Filters</span></span>
====================
<span data-ttu-id="66173-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="66173-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="66173-106">ASP.NET MVC には、事前にまたはアクション メソッドが呼び出された後にフィルタ リング ロジックを実行するためのアクション フィルターが用意されています。</span><span class="sxs-lookup"><span data-stu-id="66173-106">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="66173-107">アクション フィルターは、コント ローラーのアクション メソッドにアクション前とアクション後の動作を追加する宣言型の手段を提供するカスタム属性です。</span><span class="sxs-lookup"><span data-stu-id="66173-107">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>
> 
> <span data-ttu-id="66173-108">このハンズオン ラボでは、コント ローラーの要求をキャッチし、データベース テーブルに、サイトの操作をログ記録を MvcMusicStore ソリューションにカスタム アクション フィルター属性を作成します。</span><span class="sxs-lookup"><span data-stu-id="66173-108">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="66173-109">任意のコント ローラーまたはアクションをインジェクションして、ログ記録フィルターを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="66173-109">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="66173-110">最後に、ユーザーの一覧を示すログ ビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="66173-110">Finally, you will see the log view that shows the list of visitors.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="66173-111">このハンズオン ラボは、の基本的な知識がある前提としています。 **ASP.NET MVC**です。</span><span class="sxs-lookup"><span data-stu-id="66173-111">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="66173-112">使用していない場合**ASP.NET MVC**経由で移動する前をお勧め**ASP.NET MVC 4 基礎**ハンズオン ラボ。</span><span class="sxs-lookup"><span data-stu-id="66173-112">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="66173-113">目的</span><span class="sxs-lookup"><span data-stu-id="66173-113">Objectives</span></span>

<span data-ttu-id="66173-114">このハンズオン ラボでは学習する方法。</span><span class="sxs-lookup"><span data-stu-id="66173-114">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="66173-115">フィルター処理機能を拡張するカスタム アクション フィルター属性を作成します。</span><span class="sxs-lookup"><span data-stu-id="66173-115">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="66173-116">特定のレベルにインジェクションでカスタム フィルター属性を適用します。</span><span class="sxs-lookup"><span data-stu-id="66173-116">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="66173-117">カスタム アクション フィルターをグローバルに登録します。</span><span class="sxs-lookup"><span data-stu-id="66173-117">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="66173-118">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="66173-118">Prerequisites</span></span>

<span data-ttu-id="66173-119">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="66173-119">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="66173-120">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="66173-120">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="66173-121">セットアップ</span><span class="sxs-lookup"><span data-stu-id="66173-121">Setup</span></span>

<span data-ttu-id="66173-122">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="66173-122">**Installing Code Snippets**</span></span>

<span data-ttu-id="66173-123">便宜上、このラボに沿ったを管理するコードの多くは、Visual Studio のコード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="66173-123">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="66173-124">実行のコード スニペットをインストールする**.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="66173-124">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="66173-125">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;です。</span><span class="sxs-lookup"><span data-stu-id="66173-125">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="66173-126">演習</span><span class="sxs-lookup"><span data-stu-id="66173-126">Exercises</span></span>

<span data-ttu-id="66173-127">次の演習では、このハンズオン ラボから成ります。</span><span class="sxs-lookup"><span data-stu-id="66173-127">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="66173-128">手順 1: アクションのログ記録</span><span class="sxs-lookup"><span data-stu-id="66173-128">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="66173-129">手順 2: 複数のアクション フィルターの管理</span><span class="sxs-lookup"><span data-stu-id="66173-129">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="66173-130">この演習を完了する時間を推定: **30 分**です。</span><span class="sxs-lookup"><span data-stu-id="66173-130">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="66173-131">各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="66173-131">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="66173-132">演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="66173-132">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="66173-133">手順 1: アクションのログ記録</span><span class="sxs-lookup"><span data-stu-id="66173-133">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="66173-134">この演習では、ASP.NET MVC 4 のフィルター プロバイダーを使用して、カスタム アクション ログ フィルターを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="66173-134">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="66173-135">目的とするログ記録フィルターを選択したコント ローラーのすべてのアクティビティが記録 MusicStore サイトに適用されます。</span><span class="sxs-lookup"><span data-stu-id="66173-135">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="66173-136">フィルターが拡張する**ActionFilterAttributeClass**オーバーライドと**OnActionExecuting**メソッドを各要求をキャッチして、ログ記録のアクションを実行します。</span><span class="sxs-lookup"><span data-stu-id="66173-136">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="66173-137">HTTP 要求に関するコンテキスト情報は ASP.NET MVC によって提供されるメソッド、結果、およびパラメーターの実行は**ActionExecutingContext**クラス**です。**</span><span class="sxs-lookup"><span data-stu-id="66173-137">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="66173-138">ASP.NET MVC 4 では、カスタム フィルターを作成せずに使用できる既定のフィルター プロバイダーもあります。</span><span class="sxs-lookup"><span data-stu-id="66173-138">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="66173-139">ASP.NET MVC 4 には、次の種類のフィルターが用意されています。</span><span class="sxs-lookup"><span data-stu-id="66173-139">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="66173-140">**承認**認証の実行や要求のプロパティを検証するなど、アクション メソッドを実行するかどうかに関するセキュリティ上の決定は、これをフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="66173-140">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="66173-141">**アクション**フィルター、アクション メソッドの実行をラップします。</span><span class="sxs-lookup"><span data-stu-id="66173-141">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="66173-142">このフィルターは、アクション メソッドに余分なデータを提供して、戻り値の検査、アクション メソッドの実行の取り消しなど、追加の処理を実行できます。</span><span class="sxs-lookup"><span data-stu-id="66173-142">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="66173-143">**結果**ActionResult オブジェクトの実行をラップするフィルター。</span><span class="sxs-lookup"><span data-stu-id="66173-143">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="66173-144">このフィルターは、HTTP 応答の変更など、結果の追加の処理を実行できます。</span><span class="sxs-lookup"><span data-stu-id="66173-144">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="66173-145">**例外**フィルターで、結果の実行で承認フィルターを使用して開始および終了、アクション メソッドでスローどこかにハンドルされない例外がある場合に実行します。</span><span class="sxs-lookup"><span data-stu-id="66173-145">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="66173-146">例外フィルターは、ログ記録やエラー ページの表示などのタスクで使用できます。</span><span class="sxs-lookup"><span data-stu-id="66173-146">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="66173-147">フィルター プロバイダーの詳細については、この MSDN リンクを参照してください: ([https://msdn.microsoft.com/en-us/library/dd410209.aspx](https://msdn.microsoft.com/en-us/library/dd410209.aspx))。</span><span class="sxs-lookup"><span data-stu-id="66173-147">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/en-us/library/dd410209.aspx](https://msdn.microsoft.com/en-us/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="66173-148">MVC 音楽ストア アプリケーションのログ記録機能について</span><span class="sxs-lookup"><span data-stu-id="66173-148">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="66173-149">この Music Store ソリューションが、サイトのログ記録用に新しいデータ モデル テーブル**ActionLog**、次のフィールドを持つ: 要求、呼び出されるアクション、クライアントの ip アドレス、およびタイムスタンプを受信するコント ローラーの名前。</span><span class="sxs-lookup"><span data-stu-id="66173-149">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="66173-150">![データ モデル。ActionLog テーブルです。](aspnet-mvc-4-custom-action-filters/_static/image1.png "データ モデル。ActionLog テーブルです。")</span><span class="sxs-lookup"><span data-stu-id="66173-150">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="66173-151">*データ モデルの ActionLog テーブル*</span><span class="sxs-lookup"><span data-stu-id="66173-151">*Data model - ActionLog table*</span></span>

<span data-ttu-id="66173-152">ソリューションに含まれているアクション ログの ASP.NET MVC ビューを提供する**MvcMusicStores/ビュー/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="66173-152">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="66173-153">![アクション ログ ビュー](aspnet-mvc-4-custom-action-filters/_static/image2.png "アクション ログの表示")</span><span class="sxs-lookup"><span data-stu-id="66173-153">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="66173-154">*アクション ログの表示*</span><span class="sxs-lookup"><span data-stu-id="66173-154">*Action Log view*</span></span>

<span data-ttu-id="66173-155">この構造体を指定するには、すべての作業はコント ローラーの要求を中断することと、カスタム フィルターを使用して、ログ記録の実行にフォーカスします。</span><span class="sxs-lookup"><span data-stu-id="66173-155">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="66173-156">タスク 1 - コント ローラーの要求をキャッチするカスタム フィルターを作成します。</span><span class="sxs-lookup"><span data-stu-id="66173-156">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="66173-157">このタスクでは、ログのロジックを格納するカスタム フィルター属性クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="66173-157">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="66173-158">ASP.NET MVC を拡張する目的に**ActionFilterAttribute**クラスとインターフェイスを実装する**IActionFilter**です。</span><span class="sxs-lookup"><span data-stu-id="66173-158">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="66173-159">**ActionFilterAttribute**すべての属性フィルターの基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="66173-159">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="66173-160">後、コント ローラー アクションの実行前に、特定のロジックを実行する次のメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="66173-160">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="66173-161">**OnActionExecuting**(ActionExecutingContext filterContext): 操作の直前にメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="66173-161">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="66173-162">**OnActionExecuted**(ActionExecutedContext filterContext): (ビューのレンダリング) の前に、結果が実行される前に、アクション メソッドが呼び出された後にします。</span><span class="sxs-lookup"><span data-stu-id="66173-162">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="66173-163">**OnResultExecuting**(ResultExecutingContext filterContext): (ビューのレンダリング) の前に、結果が実行される前に、だけです。</span><span class="sxs-lookup"><span data-stu-id="66173-163">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="66173-164">**OnResultExecuted**(ResultExecutedContext filterContext): (ビューが表示される) 後に結果が実行された後にします。</span><span class="sxs-lookup"><span data-stu-id="66173-164">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="66173-165">派生クラスには、これらのメソッドをオーバーライドすることでは、独自のフィルター処理コードを実行できます。</span><span class="sxs-lookup"><span data-stu-id="66173-165">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="66173-166">開く、**開始**ソリューションにある**\Source\Ex01-LoggingActions\Begin**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="66173-166">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

    1. <span data-ttu-id="66173-167">続行する前に、いくつか不足している NuGet パッケージをダウンロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="66173-167">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="66173-168">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="66173-168">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="66173-169">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="66173-169">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="66173-170">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="66173-170">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="66173-171">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="66173-171">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="66173-172">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="66173-172">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="66173-173">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="66173-173">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="66173-174">詳細については、この記事を参照してください: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)です。</span><span class="sxs-lookup"><span data-stu-id="66173-174">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="66173-175">新しい c# クラスを追加、**フィルター**フォルダーし名前を付けます*CustomActionFilter.cs*です。</span><span class="sxs-lookup"><span data-stu-id="66173-175">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="66173-176">このフォルダーでは、すべてのカスタム フィルターを格納します。</span><span class="sxs-lookup"><span data-stu-id="66173-176">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="66173-177">開いている**CustomActionFilter.cs**への参照を追加および**System.Web.Mvc**と**MvcMusicStore.Models**名前空間。</span><span class="sxs-lookup"><span data-stu-id="66173-177">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="66173-178">(コード スニペットの*ASP.NET MVC 4 のカスタム アクション フィルター - Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="66173-178">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="66173-179">継承、 **CustomActionFilter**クラス**ActionFilterAttribute**し**CustomActionFilter**クラス実装**IActionFilter**インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="66173-179">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="66173-180">ください**CustomActionFilter**クラス、メソッドをオーバーライド**OnActionExecuting**フィルターの実行ログを記録するために必要なロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="66173-180">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="66173-181">これを行うには、次の強調表示されたコード内での追加**CustomActionFilter**クラスです。</span><span class="sxs-lookup"><span data-stu-id="66173-181">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="66173-182">(コード スニペットの*ASP.NET MVC 4 のカスタム アクション フィルター - Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="66173-182">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="66173-183">**OnActionExecuting**メソッドを使用して**Entity Framework**新しい ActionLog レジスタを追加します。</span><span class="sxs-lookup"><span data-stu-id="66173-183">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="66173-184">作成してからコンテキスト情報を含むエンティティの新しいインスタンスを塗りつぶします**filterContext**です。</span><span class="sxs-lookup"><span data-stu-id="66173-184">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="66173-185">に関する詳細を読み取ることができます**ControllerContext**でクラス[msdn](https://msdn.microsoft.com/en-us/library/system.web.mvc.controllercontext.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="66173-185">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/en-us/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="66173-186">タスク 2 - ストアのコント ローラー クラスにコード インターセプターを挿入します。</span><span class="sxs-lookup"><span data-stu-id="66173-186">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="66173-187">このタスクでは、すべてのコント ローラー クラスおよび記録されるコント ローラーのアクションを挿入し、カスタム フィルターを追加します。</span><span class="sxs-lookup"><span data-stu-id="66173-187">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="66173-188">この演習では、ログが、ストアのコント ローラー クラスになります。</span><span class="sxs-lookup"><span data-stu-id="66173-188">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="66173-189">メソッド**OnActionExecuting**から**ActionLogFilterAttribute**挿入された要素が呼び出されたときに、カスタム フィルターが実行されます。</span><span class="sxs-lookup"><span data-stu-id="66173-189">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="66173-190">特定のコント ローラーのメソッドをインターセプトすることもできます。</span><span class="sxs-lookup"><span data-stu-id="66173-190">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="66173-191">開く、 **StoreController**で**MvcMusicStore\Controllers**への参照を追加し、**フィルター**名前空間。</span><span class="sxs-lookup"><span data-stu-id="66173-191">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="66173-192">カスタム フィルターを挿入**CustomActionFilter**に**StoreController**クラスを追加して**[CustomActionFilter]**クラス宣言の前に、の属性です。</span><span class="sxs-lookup"><span data-stu-id="66173-192">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

    > [!NOTE]
    > <span data-ttu-id="66173-193">フィルターは、コント ローラー クラスに組み込まれてと、そのすべてのアクションは挿入もします。</span><span class="sxs-lookup"><span data-stu-id="66173-193">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="66173-194">一連のアクションに対してのみフィルターを適用するには、挿入する必要があります**[CustomActionFilter]**それらのいずれか。</span><span class="sxs-lookup"><span data-stu-id="66173-194">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="66173-195">タスク 3 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="66173-195">Task 3 - Running the Application</span></span>

<span data-ttu-id="66173-196">このタスクでは、ログ記録フィルターが機能しているかをテストします。</span><span class="sxs-lookup"><span data-stu-id="66173-196">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="66173-197">アプリケーションを起動して、ストアにアクセスしようし、ログに記録されたアクティビティはチェックします。</span><span class="sxs-lookup"><span data-stu-id="66173-197">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="66173-198">**F5** キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="66173-198">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="66173-199">参照**/ActionLog**ログ ビューの初期状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="66173-199">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="66173-200">![ログの追跡ツールの状態 ページのアクティビティの前に](aspnet-mvc-4-custom-action-filters/_static/image3.png "ログの追跡ツールの状態 ページのアクティビティの前に")</span><span class="sxs-lookup"><span data-stu-id="66173-200">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="66173-201">*ページのアクティビティの前にログの追跡ツールの状態*</span><span class="sxs-lookup"><span data-stu-id="66173-201">*Log tracker status before page activity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="66173-202">既定では、1 つの項目、メニューの既存のジャンルを取得するときに生成された常に表示されます。</span><span class="sxs-lookup"><span data-stu-id="66173-202">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
    > 
    > <span data-ttu-id="66173-203">クリーンアップおわかりやすくするための目的で、 **ActionLog**たびに、アプリケーションが実行されるは、それぞれ特定のタスクの検証のログのみが表示するためのテーブルです。</span><span class="sxs-lookup"><span data-stu-id="66173-203">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
    > 
    > <span data-ttu-id="66173-204">次のコードを削除する必要がありますが、**セッション\_開始**メソッド (で、 **Global.asax**クラス)、ストア内で実行されるすべての操作の履歴ログを保存するのにはコント ローラー。</span><span class="sxs-lookup"><span data-stu-id="66173-204">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="66173-205">いずれかをクリックして、**ジャンル** メニューから使用可能なアルバムを参照するように、操作を実行したりします。</span><span class="sxs-lookup"><span data-stu-id="66173-205">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="66173-206">参照**/ActionLog**され、ログが空のキーを押して**f5 キーを押して**ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="66173-206">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="66173-207">訪問が追跡されなかったことを確認します。</span><span class="sxs-lookup"><span data-stu-id="66173-207">Check that your visits were tracked:</span></span>

    <span data-ttu-id="66173-208">![ログ記録アクティビティのアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image4.png "ログに記録するアクティビティのアクション ログ")</span><span class="sxs-lookup"><span data-stu-id="66173-208">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="66173-209">*ログ記録アクティビティのアクション ログ*</span><span class="sxs-lookup"><span data-stu-id="66173-209">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="66173-210">手順 2: 複数のアクション フィルターの管理</span><span class="sxs-lookup"><span data-stu-id="66173-210">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="66173-211">この演習では、StoreController クラスに 2 番目のカスタム操作フィルターを追加し、両方のフィルターを実行する特定の順序を定義します。</span><span class="sxs-lookup"><span data-stu-id="66173-211">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="66173-212">フィルターをグローバルに登録するコードが更新されます。</span><span class="sxs-lookup"><span data-stu-id="66173-212">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="66173-213">さまざまなオプションを考慮に入れるフィルターの実行順序を定義するときにします。</span><span class="sxs-lookup"><span data-stu-id="66173-213">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="66173-214">たとえば、Order プロパティと、フィルターのスコープ:</span><span class="sxs-lookup"><span data-stu-id="66173-214">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="66173-215">定義することができます、**スコープ**フィルターごとに、たとえば、でしたスコープを設定する内で実行するすべてのアクション フィルター、**コント ローラーのスコープ**とで実行するすべての承認フィルター**グローバル スコープ**.</span><span class="sxs-lookup"><span data-stu-id="66173-215">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="66173-216">スコープでは、定義済みの実行順序があります。</span><span class="sxs-lookup"><span data-stu-id="66173-216">The scopes have a defined execution order.</span></span>

<span data-ttu-id="66173-217">さらに、各アクション フィルターでは、フィルターのスコープ内の実行順序を決定するために使用する順序プロパティがいます。</span><span class="sxs-lookup"><span data-stu-id="66173-217">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="66173-218">カスタム アクション フィルターの実行順序の詳細については、この MSDN の記事をご覧ください: ([https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx)) です。</span><span class="sxs-lookup"><span data-stu-id="66173-218">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="66173-219">タスク 1: 新しいカスタム アクション フィルターの作成</span><span class="sxs-lookup"><span data-stu-id="66173-219">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="66173-220">このタスクは作成 StoreController クラスに挿入する新しいカスタム アクション フィルターのフィルターの実行順序を管理する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="66173-220">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="66173-221">開く、**開始**ソリューションにある**\Source\Ex02-ManagingMultipleActionFilters\Begin**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="66173-221">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="66173-222">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="66173-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="66173-223">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="66173-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="66173-224">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="66173-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="66173-225">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="66173-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="66173-226">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="66173-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="66173-227">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="66173-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="66173-228">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="66173-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="66173-229">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="66173-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="66173-230">詳細については、この記事を参照してください: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)です。</span><span class="sxs-lookup"><span data-stu-id="66173-230">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="66173-231">新しい c# クラスを追加、**フィルター**フォルダーし名前を付けます*MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="66173-231">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="66173-232">開いている**MyNewCustomActionFilter.cs**への参照を追加および**System.Web.Mvc**と**MvcMusicStore.Models**名前空間。</span><span class="sxs-lookup"><span data-stu-id="66173-232">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="66173-233">(コード スニペットの*ASP.NET MVC 4 のカスタム アクション フィルター - Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="66173-233">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="66173-234">既定のクラス宣言を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="66173-234">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="66173-235">(コード スニペットの*ASP.NET MVC 4 のカスタム アクション フィルター - Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="66173-235">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="66173-236">このカスタム アクション フィルターは、前述の手順で作成されたものよりもほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="66173-236">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="66173-237">主な違いがある、 *&quot;によってログに記録&quot;*状態フィルターを識別するこの新しいクラスの名前で更新された属性には、ログが登録されています。</span><span class="sxs-lookup"><span data-stu-id="66173-237">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="66173-238">タスク 2: StoreController クラスに新しいコード インターセプターを挿入します。</span><span class="sxs-lookup"><span data-stu-id="66173-238">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="66173-239">このタスクでは、StoreController クラスに新しいカスタム フィルターを追加し、どのように両方のフィルターが共同作業を確認するようにソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="66173-239">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="66173-240">開く、 **StoreController**クラスにある**MvcMusicStore\Controllers**し、新しいカスタム フィルターを挿入**MyNewCustomActionFilter**に**StoreController**ようなクラスが、次のコードに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="66173-240">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="66173-241">ここで、これら 2 つのカスタム アクション フィルターの動作を確認するためにアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="66173-241">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="66173-242">これを行うには、キーを押して**f5 キーを押して**あり、アプリケーションが開始されるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="66173-242">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="66173-243">参照**/ActionLog**ログ ビューの初期状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="66173-243">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="66173-244">![ログの追跡ツールの状態 ページのアクティビティの前に](aspnet-mvc-4-custom-action-filters/_static/image5.png "ログの追跡ツールの状態 ページのアクティビティの前に")</span><span class="sxs-lookup"><span data-stu-id="66173-244">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="66173-245">*ページのアクティビティの前にログの追跡ツールの状態*</span><span class="sxs-lookup"><span data-stu-id="66173-245">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="66173-246">いずれかをクリックして、**ジャンル** メニューから使用可能なアルバムを参照するように、操作を実行したりします。</span><span class="sxs-lookup"><span data-stu-id="66173-246">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="66173-247">です。 この時間を確認します。訪問履歴の管理が 2 回されていた: 内の各カスタム アクション フィルターを追加した後、 **StorageController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="66173-247">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="66173-248">![ログ記録アクティビティのアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image6.png "ログに記録するアクティビティのアクション ログ")</span><span class="sxs-lookup"><span data-stu-id="66173-248">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="66173-249">*ログ記録アクティビティのアクション ログ*</span><span class="sxs-lookup"><span data-stu-id="66173-249">*Action log with activity logged*</span></span>
6. <span data-ttu-id="66173-250">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="66173-250">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="66173-251">タスク 3: フィルターの順序を管理します。</span><span class="sxs-lookup"><span data-stu-id="66173-251">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="66173-252">このタスクでは、これらの順序を使用してフィルターの実行順序を管理する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="66173-252">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="66173-253">開く、 **StoreController**クラスにある**MvcMusicStore\Controllers**を指定し、**順序**などのプロパティのフィルターの両方で次に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="66173-253">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="66173-254">ここで、その Order プロパティの値に応じて、フィルターを実行する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="66173-254">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="66173-255">最小の注文の値を含むフィルターと考えることが (**CustomActionFilter**) が実行される最初の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="66173-255">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="66173-256">キーを押して**f5 キーを押して**あり、アプリケーションが開始されるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="66173-256">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="66173-257">参照**/ActionLog**ログ ビューの初期状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="66173-257">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="66173-258">![ログの追跡ツールの状態 ページのアクティビティの前に](aspnet-mvc-4-custom-action-filters/_static/image7.png "ログの追跡ツールの状態 ページのアクティビティの前に")</span><span class="sxs-lookup"><span data-stu-id="66173-258">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="66173-259">*ページのアクティビティの前にログの追跡ツールの状態*</span><span class="sxs-lookup"><span data-stu-id="66173-259">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="66173-260">いずれかをクリックして、**ジャンル** メニューから使用可能なアルバムを参照するように、操作を実行したりします。</span><span class="sxs-lookup"><span data-stu-id="66173-260">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="66173-261">フィルターの順序の値順にチェックを現時点では、訪問が追跡されなかった: **CustomActionFilter**ログの最初。</span><span class="sxs-lookup"><span data-stu-id="66173-261">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="66173-262">![ログ記録アクティビティのアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image8.png "ログに記録するアクティビティのアクション ログ")</span><span class="sxs-lookup"><span data-stu-id="66173-262">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="66173-263">*ログ記録アクティビティのアクション ログ*</span><span class="sxs-lookup"><span data-stu-id="66173-263">*Action log with activity logged*</span></span>
6. <span data-ttu-id="66173-264">ここで、フィルターの順序の値を更新し、ログの順序がどのように変化するかを確認します。</span><span class="sxs-lookup"><span data-stu-id="66173-264">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="66173-265">**StoreController**クラス、次に示すように、フィルターの順序の値を更新します。</span><span class="sxs-lookup"><span data-stu-id="66173-265">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="66173-266">キーを押してアプリケーションを再実行**f5 キーを押して**です。</span><span class="sxs-lookup"><span data-stu-id="66173-266">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="66173-267">いずれかをクリックして、**ジャンル** メニューから使用可能なアルバムを参照するように、操作を実行したりします。</span><span class="sxs-lookup"><span data-stu-id="66173-267">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="66173-268">によって作成されたログを現時点では、ことを確認して**MyNewCustomActionFilter**フィルターが最初に表示します。</span><span class="sxs-lookup"><span data-stu-id="66173-268">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="66173-269">![ログ記録アクティビティのアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image9.png "ログに記録するアクティビティのアクション ログ")</span><span class="sxs-lookup"><span data-stu-id="66173-269">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="66173-270">*ログ記録アクティビティのアクション ログ*</span><span class="sxs-lookup"><span data-stu-id="66173-270">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="66173-271">タスク 4: をグローバル フィルターの登録</span><span class="sxs-lookup"><span data-stu-id="66173-271">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="66173-272">このタスクでは、新しいフィルターを登録するようにソリューションを更新します (**MyNewCustomActionFilter**) グローバル フィルターとして。</span><span class="sxs-lookup"><span data-stu-id="66173-272">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="66173-273">これにより、これはすべての操作実行前のタスクと同様に StoreController ものだけでなくと、アプリケーションでによってトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="66173-273">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="66173-274">**StoreController**クラス、削除**[MyNewCustomActionFilter]**属性と元の order プロパティ**[CustomActionFilter]**です。</span><span class="sxs-lookup"><span data-stu-id="66173-274">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="66173-275">次のようになります。</span><span class="sxs-lookup"><span data-stu-id="66173-275">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="66173-276">開いている**Global.asax**ファイルし、検索、**アプリケーション\_開始**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="66173-276">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="66173-277">呼び出して各 thime アプリケーションが開始されることに注意してくださいがグローバル フィルターを登録する**RegisterGlobalFilters**メソッド内で**FilterConfig**クラスです。</span><span class="sxs-lookup"><span data-stu-id="66173-277">Notice that each thime the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="66173-278">![Global.asax でグローバル フィルターを登録する](aspnet-mvc-4-custom-action-filters/_static/image10.png "Global.asax でグローバル フィルターを登録します。")</span><span class="sxs-lookup"><span data-stu-id="66173-278">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="66173-279">*Global.asax でグローバル フィルターを登録します。*</span><span class="sxs-lookup"><span data-stu-id="66173-279">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="66173-280">開いている**FilterConfig.cs**内でファイル**アプリ\_開始**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="66173-280">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="66173-281">System.Web.Mvc; を使用してへの参照を追加します。MvcMusicStore.Filters; を使用します。名前空間です。</span><span class="sxs-lookup"><span data-stu-id="66173-281">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="66173-282">更新**RegisterGlobalFilters**メソッドは、カスタム フィルターを追加します。</span><span class="sxs-lookup"><span data-stu-id="66173-282">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="66173-283">これを行うには、強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="66173-283">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="66173-284">キーを押してアプリケーションを実行**f5 キーを押して**です。</span><span class="sxs-lookup"><span data-stu-id="66173-284">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="66173-285">いずれかをクリックして、**ジャンル** メニューから使用可能なアルバムを参照するように、操作を実行したりします。</span><span class="sxs-lookup"><span data-stu-id="66173-285">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="66173-286">チェックされている**[MyNewCustomActionFilter]**すぎる HomeController および ActionLogController で挿入するがします。</span><span class="sxs-lookup"><span data-stu-id="66173-286">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="66173-287">![ログ記録アクティビティのアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image11.png "ログに記録するアクティビティのアクション ログ")</span><span class="sxs-lookup"><span data-stu-id="66173-287">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="66173-288">*ログに記録するグローバルのアクティビティのアクション ログ*</span><span class="sxs-lookup"><span data-stu-id="66173-288">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="66173-289">Windows Azure Web サイトの次にこのアプリケーションを展開するさらに、[付録 b: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)です。</span><span class="sxs-lookup"><span data-stu-id="66173-289">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="66173-290">概要</span><span class="sxs-lookup"><span data-stu-id="66173-290">Summary</span></span>

<span data-ttu-id="66173-291">このハンズオン ラボを完了して、カスタム アクションを実行するアクション フィルターを拡張する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="66173-291">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="66173-292">任意のページ コント ローラーのフィルターを挿入する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="66173-292">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="66173-293">次の概念が使われていました。</span><span class="sxs-lookup"><span data-stu-id="66173-293">The following concepts were used:</span></span>

- <span data-ttu-id="66173-294">ASP.NET MVC ActionFilterAttribute クラスを使用したカスタム アクション フィルターを作成する方法</span><span class="sxs-lookup"><span data-stu-id="66173-294">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="66173-295">ASP.NET MVC のコント ローラーにフィルターを挿入する方法</span><span class="sxs-lookup"><span data-stu-id="66173-295">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="66173-296">フィルターの順序のプロパティを使用して順序を管理する方法</span><span class="sxs-lookup"><span data-stu-id="66173-296">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="66173-297">フィルターをグローバルに登録する方法</span><span class="sxs-lookup"><span data-stu-id="66173-297">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="66173-298">付録 a: をインストールする Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="66173-298">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="66173-299">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="66173-299">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="66173-300">次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。</span><span class="sxs-lookup"><span data-stu-id="66173-300">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="66173-301">移動して[ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。</span><span class="sxs-lookup"><span data-stu-id="66173-301">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="66173-302">または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; *Visual Studio Express 2012 for Web と Windows Azure SDK*&quot;です。</span><span class="sxs-lookup"><span data-stu-id="66173-302">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="66173-303">をクリックして**を今すぐインストール**です。</span><span class="sxs-lookup"><span data-stu-id="66173-303">Click on **Install Now**.</span></span> <span data-ttu-id="66173-304">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="66173-304">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="66173-305">1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="66173-305">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="66173-306">![Visual Studio Express インストール](aspnet-mvc-4-custom-action-filters/_static/image12.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="66173-306">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="66173-307">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="66173-307">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="66173-308">すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="66173-308">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="66173-310">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="66173-310">*Accepting the license terms*</span></span>
5. <span data-ttu-id="66173-311">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="66173-311">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="66173-313">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="66173-313">*Installation progress*</span></span>
6. <span data-ttu-id="66173-314">インストールが完了したらをクリックして**完了**です。</span><span class="sxs-lookup"><span data-stu-id="66173-314">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="66173-316">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="66173-316">*Installation completed*</span></span>
7. <span data-ttu-id="66173-317">をクリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="66173-317">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="66173-318">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="66173-318">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web タイルを](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="66173-320">*VS Express Web タイルを*</span><span class="sxs-lookup"><span data-stu-id="66173-320">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="66173-321">付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="66173-321">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="66173-322">この付録では、Windows Azure 管理ポータルから新しい web サイトを作成し、Windows Azure によって提供される Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを公開する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="66173-322">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="66173-323">タスク 1 - Windows から新しい Web サイトを作成する Azure ポータル</span><span class="sxs-lookup"><span data-stu-id="66173-323">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="66173-324">移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="66173-324">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="66173-325">Windows Azure 無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増大に応じて拡大縮小できます。</span><span class="sxs-lookup"><span data-stu-id="66173-325">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="66173-326">サインアップする[ここ](http://aka.ms/aspnet-hol-azure)です。</span><span class="sxs-lookup"><span data-stu-id="66173-326">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="66173-327">![Windows Azure ポータルにログオンする](aspnet-mvc-4-custom-action-filters/_static/image17.png "Windows Azure ポータルにログオンします。")</span><span class="sxs-lookup"><span data-stu-id="66173-327">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="66173-328">*Windows Azure 管理ポータルにログオンします。*</span><span class="sxs-lookup"><span data-stu-id="66173-328">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="66173-329">をクリックして**新規**コマンド バーでします。</span><span class="sxs-lookup"><span data-stu-id="66173-329">Click **New** on the command bar.</span></span>

    <span data-ttu-id="66173-330">![新しい Web サイトを作成する](aspnet-mvc-4-custom-action-filters/_static/image18.png "新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="66173-330">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="66173-331">*新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="66173-331">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="66173-332">をクリックして**コンピューティング** | **Web サイト**です。</span><span class="sxs-lookup"><span data-stu-id="66173-332">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="66173-333">選択し、**簡易作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="66173-333">Then select **Quick Create** option.</span></span> <span data-ttu-id="66173-334">新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="66173-334">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="66173-335">Windows Azure Web サイトは、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。</span><span class="sxs-lookup"><span data-stu-id="66173-335">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="66173-336">簡易作成 オプションでは、完成した web アプリケーションを Windows Azure Web サイトからポータル外を展開することができます。</span><span class="sxs-lookup"><span data-stu-id="66173-336">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="66173-337">データベースを設定するための手順は含まれません。</span><span class="sxs-lookup"><span data-stu-id="66173-337">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="66173-338">![簡易作成を使用して新しい Web サイトを作成する](aspnet-mvc-4-custom-action-filters/_static/image19.png "簡易作成を使用して新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="66173-338">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="66173-339">*簡易作成を使用して新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="66173-339">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="66173-340">新しいまで待つ**Web サイト**を作成します。</span><span class="sxs-lookup"><span data-stu-id="66173-340">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="66173-341">Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。</span><span class="sxs-lookup"><span data-stu-id="66173-341">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="66173-342">新しい Web サイトが動作していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="66173-342">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="66173-343">![新しい web サイトを参照して](aspnet-mvc-4-custom-action-filters/_static/image20.png "新しい web サイトを参照")</span><span class="sxs-lookup"><span data-stu-id="66173-343">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="66173-344">*新しい web サイトを参照*</span><span class="sxs-lookup"><span data-stu-id="66173-344">*Browsing to the new web site*</span></span>

    <span data-ttu-id="66173-345">![Web サイトを実行している](aspnet-mvc-4-custom-action-filters/_static/image21.png "を実行している Web サイト")</span><span class="sxs-lookup"><span data-stu-id="66173-345">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="66173-346">*実行している web サイト*</span><span class="sxs-lookup"><span data-stu-id="66173-346">*Web site running*</span></span>
6. <span data-ttu-id="66173-347">ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="66173-347">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="66173-348">![Web サイトの管理ページを開く](aspnet-mvc-4-custom-action-filters/_static/image22.png "web サイトの管理ページを開く")</span><span class="sxs-lookup"><span data-stu-id="66173-348">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="66173-349">*Web サイトの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="66173-349">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="66173-350">**ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。</span><span class="sxs-lookup"><span data-stu-id="66173-350">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="66173-351">*発行プロファイル*すべてのパブリケーションを有効になっているメソッドの各 Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="66173-351">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="66173-352">発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="66173-352">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="66173-353">**Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Windows Azure の web サイトに web アプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="66173-353">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="66173-354">![発行プロファイルを web サイトをダウンロードする](aspnet-mvc-4-custom-action-filters/_static/image23.png "発行プロファイルを web サイトをダウンロードします。")</span><span class="sxs-lookup"><span data-stu-id="66173-354">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="66173-355">*発行プロファイルを Web サイトをダウンロードします。*</span><span class="sxs-lookup"><span data-stu-id="66173-355">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="66173-356">既知の場所に発行プロファイル ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="66173-356">Download the publish profile file to a known location.</span></span> <span data-ttu-id="66173-357">さらにこの練習では、このファイルを使用して、Visual Studio から web アプリケーション、Windows Azure Web サイトを発行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="66173-357">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="66173-358">![発行プロファイル ファイルを保存](aspnet-mvc-4-custom-action-filters/_static/image24.png "発行プロファイルの保存")</span><span class="sxs-lookup"><span data-stu-id="66173-358">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="66173-359">*発行プロファイル ファイルを保存します。*</span><span class="sxs-lookup"><span data-stu-id="66173-359">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="66173-360">タスク 2 - データベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="66173-360">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="66173-361">アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="66173-361">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="66173-362">SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。</span><span class="sxs-lookup"><span data-stu-id="66173-362">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="66173-363">SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="66173-363">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="66173-364">SQL データベース サーバーを表示するには、Windows Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**です。</span><span class="sxs-lookup"><span data-stu-id="66173-364">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="66173-365">使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="66173-365">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="66173-366">注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。</span><span class="sxs-lookup"><span data-stu-id="66173-366">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="66173-367">データベースを作成しない、まだと後の段階で作成されます。</span><span class="sxs-lookup"><span data-stu-id="66173-367">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="66173-368">![SQL データベース サーバーのダッシュ ボード](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL データベース サーバーのダッシュ ボード")</span><span class="sxs-lookup"><span data-stu-id="66173-368">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="66173-369">*SQL データベース サーバーのダッシュ ボード*</span><span class="sxs-lookup"><span data-stu-id="66173-369">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="66173-370">次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。</span><span class="sxs-lookup"><span data-stu-id="66173-370">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="66173-371">実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**のテキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png)ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="66173-371">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![クライアントの IP アドレスを追加します。](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="66173-373">*クライアントの IP アドレスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="66173-373">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="66173-374">1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="66173-374">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![変更を確認します。](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="66173-376">*変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="66173-376">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="66173-377">タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="66173-377">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="66173-378">ASP.NET MVC 4 ソリューションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="66173-378">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="66173-379">**ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="66173-379">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="66173-380">![アプリケーションの発行](aspnet-mvc-4-custom-action-filters/_static/image29.png "アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="66173-380">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="66173-381">*Web サイトの発行*</span><span class="sxs-lookup"><span data-stu-id="66173-381">*Publishing the web site*</span></span>
2. <span data-ttu-id="66173-382">最初の作業を保存した発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="66173-382">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="66173-383">![発行プロファイルをインポートする](aspnet-mvc-4-custom-action-filters/_static/image30.png "発行プロファイルをインポートします。")</span><span class="sxs-lookup"><span data-stu-id="66173-383">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="66173-384">*発行プロファイルのインポート*</span><span class="sxs-lookup"><span data-stu-id="66173-384">*Importing publish profile*</span></span>
3. <span data-ttu-id="66173-385">をクリックして**への接続検証**です。</span><span class="sxs-lookup"><span data-stu-id="66173-385">Click **Validate Connection**.</span></span> <span data-ttu-id="66173-386">検証が完了したらクリックして**次**です。</span><span class="sxs-lookup"><span data-stu-id="66173-386">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="66173-387">検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。</span><span class="sxs-lookup"><span data-stu-id="66173-387">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="66173-388">![接続の検証](aspnet-mvc-4-custom-action-filters/_static/image31.png "接続の検証")</span><span class="sxs-lookup"><span data-stu-id="66173-388">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="66173-389">*接続の検証*</span><span class="sxs-lookup"><span data-stu-id="66173-389">*Validating connection*</span></span>
4. <span data-ttu-id="66173-390">**設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="66173-390">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="66173-391">![Web 配置の構成](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web 配置の構成")</span><span class="sxs-lookup"><span data-stu-id="66173-391">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="66173-392">*Web 配置の構成*</span><span class="sxs-lookup"><span data-stu-id="66173-392">*Web deploy configuration*</span></span>
5. <span data-ttu-id="66173-393">次のように、データベースの接続を構成します。</span><span class="sxs-lookup"><span data-stu-id="66173-393">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="66173-394">**サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:*プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="66173-394">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="66173-395">**ユーザー名**サーバー管理者のログイン名を入力します。</span><span class="sxs-lookup"><span data-stu-id="66173-395">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="66173-396">**パスワード**サーバー管理者のログイン パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="66173-396">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="66173-397">新しいデータベース名を入力します。</span><span class="sxs-lookup"><span data-stu-id="66173-397">Type a new database name.</span></span>

    <span data-ttu-id="66173-398">![対象の接続文字列を構成する](aspnet-mvc-4-custom-action-filters/_static/image33.png "対象の接続文字列を構成します。")</span><span class="sxs-lookup"><span data-stu-id="66173-398">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="66173-399">*対象の接続文字列を構成します。*</span><span class="sxs-lookup"><span data-stu-id="66173-399">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="66173-400">次に、 **[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="66173-400">Then click **OK**.</span></span> <span data-ttu-id="66173-401">データベースをクリックを作成するように求められたら**はい**です。</span><span class="sxs-lookup"><span data-stu-id="66173-401">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="66173-402">![データベースを作成する](aspnet-mvc-4-custom-action-filters/_static/image34.png "データベース文字列を作成します。")</span><span class="sxs-lookup"><span data-stu-id="66173-402">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="66173-403">*データベースの作成*</span><span class="sxs-lookup"><span data-stu-id="66173-403">*Creating the database*</span></span>
7. <span data-ttu-id="66173-404">接続の既定のテキスト ボックス内では、Windows azure SQL データベースへの接続に使用する接続文字列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="66173-404">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="66173-405">その後、 **[次へ]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="66173-405">Then click **Next**.</span></span>

    <span data-ttu-id="66173-406">![SQL データベースを指す接続文字列](aspnet-mvc-4-custom-action-filters/_static/image35.png "SQL データベースを指す接続文字列")</span><span class="sxs-lookup"><span data-stu-id="66173-406">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="66173-407">*SQL データベースを指す接続文字列*</span><span class="sxs-lookup"><span data-stu-id="66173-407">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="66173-408">**プレビュー** ] ページで [**発行**です。</span><span class="sxs-lookup"><span data-stu-id="66173-408">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="66173-409">![Web アプリケーションの発行](aspnet-mvc-4-custom-action-filters/_static/image36.png "web アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="66173-409">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="66173-410">*Web アプリケーションの発行*</span><span class="sxs-lookup"><span data-stu-id="66173-410">*Publishing the web application*</span></span>
9. <span data-ttu-id="66173-411">発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="66173-411">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="66173-412">付録 c: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="66173-412">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="66173-413">コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="66173-413">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="66173-414">ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="66173-414">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="66173-415">![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-custom-action-filters/_static/image37.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")</span><span class="sxs-lookup"><span data-stu-id="66173-415">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="66173-416">*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="66173-416">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="66173-417">***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="66173-417">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="66173-418">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="66173-418">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="66173-419">(なし、スペースやハイフン) スニペット名を入力してを起動します。</span><span class="sxs-lookup"><span data-stu-id="66173-419">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="66173-420">スニペットの名前に一致する IntelliSense 表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="66173-420">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="66173-421">正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="66173-421">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="66173-422">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="66173-422">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="66173-423">![スニペット名を入力する開始](aspnet-mvc-4-custom-action-filters/_static/image38.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="66173-423">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="66173-424">*スニペット名の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="66173-424">*Start typing the snippet name*</span></span>

<span data-ttu-id="66173-425">![Tab キーを押して、強調表示されているスニペットを選択する](aspnet-mvc-4-custom-action-filters/_static/image39.png "強調表示されているスニペットを選択するキーを押してタブ")</span><span class="sxs-lookup"><span data-stu-id="66173-425">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="66173-426">*Tab キーを押して、強調表示されているスニペットを選択するには*</span><span class="sxs-lookup"><span data-stu-id="66173-426">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="66173-427">![キーを押して タブで再度と、スニペットが展開](aspnet-mvc-4-custom-action-filters/_static/image40.png "キーを押して タブで再度と、スニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="66173-427">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="66173-428">*キーを押して タブで再度と、スニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="66173-428">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="66173-429">***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。</span><span class="sxs-lookup"><span data-stu-id="66173-429">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="66173-430">コード スニペットを挿入する場所を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="66173-430">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="66173-431">選択**スニペットの挿入**続く**マイ コード スニペット**です。</span><span class="sxs-lookup"><span data-stu-id="66173-431">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="66173-432">クリックして一覧から、関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="66173-432">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="66173-433">![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](aspnet-mvc-4-custom-action-filters/_static/image41.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="66173-433">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="66173-434">*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*</span><span class="sxs-lookup"><span data-stu-id="66173-434">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="66173-435">![クリックして一覧から、関連するスニペットを選択](aspnet-mvc-4-custom-action-filters/_static/image42.png "クリックして一覧から、関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="66173-435">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="66173-436">*クリックして一覧から、関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="66173-436">*Pick the relevant snippet from the list, by clicking on it*</span></span>
