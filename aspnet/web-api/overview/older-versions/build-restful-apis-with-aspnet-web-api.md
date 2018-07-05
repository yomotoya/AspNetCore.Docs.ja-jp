---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: ASP.NET Web API を使用した RESTful Api の構築 |Microsoft Docs
author: rick-anderson
description: 近年、HTTP が HTML ページを提供するためだけでないことは明らかになった。 いくつかの o を使用して、Web Api を構築するための強力なプラットフォームではもしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 88ac5102a1cf14050412abc336e7a8260a9fa80d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363521"
---
<a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="cc5d2-104">ASP.NET Web API を使用した RESTful Api を構築します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-104">Build RESTful APIs with ASP.NET Web API</span></span>
====================
<span data-ttu-id="cc5d2-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="cc5d2-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="cc5d2-106">近年、HTTP が HTML ページを提供するためだけでないことは明らかになった。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-106">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="cc5d2-107">さらにいくつかの単純な概念など、いくつかの動詞 (GET、POST、およびなど) を使用して、Web Api を構築するための強力なプラットフォームではまた*Uri*と*ヘッダー*します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-107">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="cc5d2-108">ASP.NET Web API は、一連の HTTP プログラミングを簡略化するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-108">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="cc5d2-109">ASP.NET MVC ランタイムの上に用意されているので Web API は HTTP の低レベルのトランスポートの詳細を自動的に処理します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-109">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="cc5d2-110">同時に、Web API は自然な HTTP プログラミング モデルを公開します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-110">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="cc5d2-111">Web API の 1 つの目標が、実際には、*いない*HTTP の現実で抽象化します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-111">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="cc5d2-112">結果として、Web API は、柔軟で簡単に拡張できます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-112">As a result, Web API is both flexible and easy to extend.</span></span> <span data-ttu-id="cc5d2-113">このハンズオン ラボでは、連絡先のマネージャーのアプリケーション用の単純な REST API を構築するのに Web API を使用します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-113">In this hands-on lab, you will use Web API to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="cc5d2-114">また、API を使用するクライアントをビルドします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-114">You will also build a client to consume the API.</span></span> <span data-ttu-id="cc5d2-115">REST のアーキテクチャ スタイルは確かにありませんが、唯一の有効な方法で HTTP する HTTP - を活用する効果的な方法を実証済みです。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-115">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="cc5d2-116">連絡先のマネージャーでは、一覧表示、追加、およびその他の連絡先を削除するための RESTful を公開します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-116">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> <span data-ttu-id="cc5d2-117">このラボでは、HTTP、REST の基本的な知識を必要とし、HTML、JavaScript、および jQuery の基本的な知識があることを前提します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-117">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="cc5d2-118">ASP.NET Web サイトで ASP.NET Web API フレームワークに専用の領域がある[ https://asp.net/web-api](https://asp.net/web-api)します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-118">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="cc5d2-119">このサイトが引き続き最新の情報、サンプル、および Web API に関連するニュースご確認いただけます。 よくを提供する場合は、事実上あらゆるデバイスや開発フレームワークを使用できるカスタムの Web Api の作成のアートをもっと深く掘り下げてたいです。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-119">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="cc5d2-120">ASP.NET Web API、ASP.NET MVC 4 に似ていますが、使用可能な依存関係挿入フレームワークのいくつか非常に簡単に使用できるように、コント ローラーから、サービス層を分離することの点で優れた柔軟性。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-120">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="cc5d2-121">Ninject をからダウンロード可能な ASP.NET Web API プロジェクトの依存関係挿入を使用する方法を示しています。 MSDN にある優れたサンプル[ここ](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-121">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="cc5d2-122">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="cc5d2-123">目的</span><span class="sxs-lookup"><span data-stu-id="cc5d2-123">Objectives</span></span>

<span data-ttu-id="cc5d2-124">このハンズオン ラボでは、学習する方法。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="cc5d2-125">RESTful Web API を実装します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-125">Implement a RESTful Web API</span></span>
- <span data-ttu-id="cc5d2-126">HTML クライアントから API を呼び出す</span><span class="sxs-lookup"><span data-stu-id="cc5d2-126">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="cc5d2-127">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="cc5d2-127">Prerequisites</span></span>

<span data-ttu-id="cc5d2-128">このハンズオン ラボを完了する、次が必要。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-128">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="cc5d2-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 B](#AppendixB)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="cc5d2-130">セットアップ</span><span class="sxs-lookup"><span data-stu-id="cc5d2-130">Setup</span></span>

<span data-ttu-id="cc5d2-131">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="cc5d2-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="cc5d2-132">便宜上、このラボに管理するコードの多くは、Visual Studio コード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="cc5d2-133">実行コード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="cc5d2-134">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 a: を使用してコード スニペット](#AppendixA)&quot;します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="cc5d2-135">演習</span><span class="sxs-lookup"><span data-stu-id="cc5d2-135">Exercises</span></span>

<span data-ttu-id="cc5d2-136">このハンズオン ラボには、次の演習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-136">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="cc5d2-137">手順 1: 読み取り専用の Web API を作成します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-137">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="cc5d2-138">手順 2: 読み取り/書き込み Web API の作成します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-138">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="cc5d2-139">手順 3: HTML クライアントから Web API を使用します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-139">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="cc5d2-140">各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-140">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="cc5d2-141">作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-141">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="cc5d2-142">この演習の所要時間を推定: **60 分**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-142">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="cc5d2-143">手順 1: 読み取り専用の Web API を作成します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-143">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="cc5d2-144">この演習では、連絡先のマネージャーの読み取り専用の GET メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-144">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="cc5d2-145">タスク 1 - API プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="cc5d2-145">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="cc5d2-146">このタスクでは、Web API の web アプリケーションを作成するのに新しい ASP.NET web プロジェクト テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-146">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="cc5d2-147">実行**Visual Studio 2012 の Express for Web**に移動する**開始**と種類**VS Express for Web**キーを押します **」と入力**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-147">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="cc5d2-148">**ファイル**メニューの **新しいプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-148">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="cc5d2-149">選択、 **(Visual C#) |Web**プロジェクトの種類のツリー ビューからの型を射影し、選択、 **ASP.NET MVC 4 Web アプリケーション**プロジェクトの種類。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-149">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="cc5d2-150">プロジェクトの設定**名前**に*ContactManager*と**ソリューション名**に*開始*、 をクリックし、 **ok**.</span><span class="sxs-lookup"><span data-stu-id="cc5d2-150">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="cc5d2-151">![新しい ASP.NET MVC 4.0 Web アプリケーション プロジェクトを作成する](build-restful-apis-with-aspnet-web-api/_static/image1.png "新しい ASP.NET MVC 4.0 Web アプリケーション プロジェクトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-151">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="cc5d2-152">*新しい ASP.NET MVC 4.0 Web アプリケーション プロジェクトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-152">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="cc5d2-153">ASP.NET MVC 4 プロジェクトの種類 ダイアログ ボックスで、選択、 **Web API**プロジェクトの種類。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-153">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="cc5d2-154">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-154">Click **OK**.</span></span>

    <span data-ttu-id="cc5d2-155">![Web API プロジェクトの種類を指定する](build-restful-apis-with-aspnet-web-api/_static/image2.png "Web API プロジェクトの種類を指定します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-155">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="cc5d2-156">*Web API プロジェクトの種類を指定します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-156">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="cc5d2-157">タスク 2 - 連絡先のマネージャーの API コント ローラーの作成</span><span class="sxs-lookup"><span data-stu-id="cc5d2-157">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="cc5d2-158">このタスクでは、API のメソッドが存在するコント ローラー クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-158">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="cc5d2-159">という名前のファイルを削除**ValuesController.cs**内**コント ローラー**プロジェクトからフォルダー。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-159">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="cc5d2-160">右クリックし、**コント ローラー**フォルダーをクリックし、プロジェクトで**追加 |コント ローラー**コンテキスト メニュー。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-160">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="cc5d2-161">![プロジェクトに新しいコント ローラーを追加する](build-restful-apis-with-aspnet-web-api/_static/image3.png "をプロジェクトに新しいコント ローラーの追加")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-161">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="cc5d2-162">*プロジェクトに新しいコント ローラーの追加*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-162">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="cc5d2-163">**コント ローラーの追加**ダイアログが表示されたら、選択**空の API コント ローラー**テンプレート メニュー。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-163">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="cc5d2-164">コント ローラー クラスの名前を**ContactController**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-164">Name the controller class **ContactController**.</span></span> <span data-ttu-id="cc5d2-165">をクリックし、**追加します。**</span><span class="sxs-lookup"><span data-stu-id="cc5d2-165">Then, click **Add.**</span></span>

    <span data-ttu-id="cc5d2-166">![コント ローラーの追加 ダイアログを使用して、新しい Web API コント ローラーを作成する](build-restful-apis-with-aspnet-web-api/_static/image4.png "コント ローラーの追加 ダイアログを使用して、新しい Web API コント ローラーを作成するには")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-166">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="cc5d2-167">*コント ローラーの追加 ダイアログを使用して、新しい Web API コント ローラーを作成するには*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-167">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="cc5d2-168">次のコードを追加、 **ContactController**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-168">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="cc5d2-169">(コード スニペット - *API メソッドを取得する Web API のラボ - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="cc5d2-169">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="cc5d2-170">キーを押して**F5**アプリケーションをデバッグします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-170">Press **F5** to debug the application.</span></span> <span data-ttu-id="cc5d2-171">Web API プロジェクトの既定のホーム ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-171">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="cc5d2-172">![ASP.NET Web API アプリケーションの既定のホーム ページ](build-restful-apis-with-aspnet-web-api/_static/image5.png "ASP.NET Web API アプリケーションの既定のホーム ページ")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-172">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="cc5d2-173">*ASP.NET Web API アプリケーションの既定のホーム ページ*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-173">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="cc5d2-174">Internet Explorer のウィンドウでキーを押して、 **F12**キーを開く、 **Developer Tools**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-174">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="cc5d2-175">をクリックして、**ネットワーク**タブをクリックし、をクリックし、**キャプチャを開始**ウィンドウにネットワーク トラフィックのキャプチャを開始するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-175">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="cc5d2-176">![[ネットワーク] タブを開き、開始するネットワーク キャプチャ](build-restful-apis-with-aspnet-web-api/_static/image6.png "ネットワーク キャプチャの [ネットワーク] タブを開き、開始します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-176">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="cc5d2-177">*[ネットワーク] タブを開き、ネットワーク キャプチャを開始します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-177">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="cc5d2-178">ブラウザーのアドレス バーで URL を追加**api/連絡先**し、enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-178">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="cc5d2-179">送信の詳細は、ネットワーク キャプチャのウィンドウに表示されます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-179">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="cc5d2-180">応答の MIME の種類は **、application/json**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-180">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="cc5d2-181">これは、既定の出力形式の JSON の方法を示します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-181">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="cc5d2-182">![ネットワーク ビューで、Web API 要求の出力を表示する](build-restful-apis-with-aspnet-web-api/_static/image7.png "ネットワーク ビューで、Web API 要求の出力を表示します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-182">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="cc5d2-183">*ネットワーク ビューで、Web API 要求の出力を表示します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-183">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc5d2-184">この時点で、Internet Explorer 10 の既定の動作は、ユーザーは保存または Web API の呼び出しから結果ストリームを開くようなかどうかになります。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-184">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="cc5d2-185">出力は、Web API URL の呼び出しの JSON の結果を含むテキスト ファイルになります。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-185">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="cc5d2-186">開発者ツール ウィンドウから、応答のコンテンツを視聴できるようにするには、ダイアログを取り消さないでください。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-186">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="cc5d2-187">をクリックして、**詳細ビューに移動**の詳細については、この API 呼び出しの応答を表示するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-187">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="cc5d2-188">![詳細ビューに切り替える](build-restful-apis-with-aspnet-web-api/_static/image8.png "詳細ビューに切り替える")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-188">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="cc5d2-189">*詳細ビューに切り替える*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-189">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="cc5d2-190">をクリックして、**応答本文**タブには、実際の JSON 応答のテキストを表示します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-190">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="cc5d2-191">![ネットワーク モニターでのテキストを出力 JSON の表示](build-restful-apis-with-aspnet-web-api/_static/image9.png "ネットワーク モニターでのテキストを出力 JSON の表示")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-191">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="cc5d2-192">*ネットワーク モニターで JSON 出力のテキストを表示します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-192">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="cc5d2-193">タスク 3 - 連絡先のモデルを作成して、連絡先のコント ローラーの拡張</span><span class="sxs-lookup"><span data-stu-id="cc5d2-193">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="cc5d2-194">このタスクでは、API のメソッドが存在するコント ローラー クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-194">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="cc5d2-195">右クリックし、**モデル**フォルダーと選択**追加 |クラス.** コンテキスト メニュー。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-195">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="cc5d2-196">![Web アプリケーションに新しいモデルを追加する](build-restful-apis-with-aspnet-web-api/_static/image10.png "web アプリケーションに新しいモデルを追加します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-196">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="cc5d2-197">*Web アプリケーションに新しいモデルを追加します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-197">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="cc5d2-198">**新しい項目の追加**ダイアログ ボックスで、ファイルの名前**Contact.cs**クリック**追加します。**</span><span class="sxs-lookup"><span data-stu-id="cc5d2-198">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="cc5d2-199">![連絡先の新しいクラス ファイルを作成する](build-restful-apis-with-aspnet-web-api/_static/image11.png "Contact の新しいクラス ファイルを作成します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-199">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="cc5d2-200">*連絡先の新しいクラス ファイルを作成します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-200">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="cc5d2-201">次の強調表示されたコードを追加、**連絡先**クラス。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-201">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="cc5d2-202">(コード スニペット - *Web API のラボ - Ex01 - 連絡先クラス*)</span><span class="sxs-lookup"><span data-stu-id="cc5d2-202">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="cc5d2-203">**ContactController**クラス、単語を選択する**文字列**のメソッド定義で、**取得**メソッド、および、word 型*連絡先*。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-203">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="cc5d2-204">単語の先頭にインジケーターが表示されますで単語を入力すると、**連絡先**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-204">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="cc5d2-205">いずれかを押し、 **Ctrl**キーしピリオド (.) キーを押すかマウスを使用して自動的に入力する、コード エディターでアシスタンス ダイアログを開く アイコンをクリックして、**を使用して**モデルのディレクティブ名前空間。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-205">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![名前空間宣言の Intellisense の機能を使用します。](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="cc5d2-207">*名前空間宣言の Intellisense の機能を使用します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-207">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="cc5d2-208">コードを変更する、**取得**メソッドにお問い合わせくださいモデル インスタンスの配列返すようにします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-208">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="cc5d2-209">(コード スニペット -*連絡先の一覧を返す Web API ラボ - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="cc5d2-209">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="cc5d2-210">キーを押して**F5**ブラウザーで web アプリケーションをデバッグします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-210">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="cc5d2-211">API の応答の出力に加えられた変更を表示するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-211">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="cc5d2-212">ブラウザーが開いたら、次のようにキーを押して**F12**開発者ツールが開いていないまだ場合。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-212">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="cc5d2-213">をクリックして、**ネットワーク**タブ。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-213">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="cc5d2-214">キーを押して、**キャプチャ開始**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-214">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="cc5d2-215">URL サフィックスを追加する**api/連絡先**キーを押して、アドレス バーに URL を**Enter**キー。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-215">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="cc5d2-216">キーを押して、**詳細ビューに移動**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-216">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="cc5d2-217">選択、**応答本文**タブ。連絡先のインスタンスの配列のシリアル化された形式を表す JSON 文字列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-217">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="cc5d2-218">![複雑な Web API のメソッド呼び出しの出力が JSON にシリアル化された](build-restful-apis-with-aspnet-web-api/_static/image13.png "複雑な Web API のメソッド呼び出しの出力が JSON にシリアル化されました。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-218">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="cc5d2-219">*複雑な Web API のメソッド呼び出しの出力が JSON にシリアル化されました。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-219">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="cc5d2-220">タスク 4 - 展開の機能をサービス層</span><span class="sxs-lookup"><span data-stu-id="cc5d2-220">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="cc5d2-221">このタスクでは、開発者は、サービス機能により、実際に作業を実行するサービスの再利用性、コント ローラーのレイヤーから分離しやすく、サービス層に機能を抽出する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-221">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="cc5d2-222">ソリューションのルートに新しいフォルダーを作成し、名前**サービス**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-222">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="cc5d2-223">これを行うには、右クリックして**ContactManager**プロジェクトで、**追加** | **新しいフォルダー**、名前を付けます*サービス*します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-223">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="cc5d2-224">![作成サービス フォルダー](build-restful-apis-with-aspnet-web-api/_static/image14.png "サービスを作成するフォルダー")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-224">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="cc5d2-225">*サービスのフォルダーを作成します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-225">*Creating Services folder*</span></span>
2. <span data-ttu-id="cc5d2-226">右クリックし、**サービス**フォルダーと選択**追加 |クラス.** コンテキスト メニュー。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-226">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="cc5d2-227">![サービス フォルダーに新しいクラスの追加](build-restful-apis-with-aspnet-web-api/_static/image15.png "Services フォルダーに新しいクラスの追加")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-227">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="cc5d2-228">*サービス フォルダーに新しいクラスの追加*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-228">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="cc5d2-229">ときに、**新しい項目の追加**ダイアログが表示されたら、新しいクラスの名前**ContactRepository**クリック**追加**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-229">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="cc5d2-230">![連絡先リポジトリ サービス層のコードを格納するクラス ファイルを作成する](build-restful-apis-with-aspnet-web-api/_static/image16.png "連絡先リポジトリ サービス層のコードを記述するクラス ファイルを作成します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-230">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="cc5d2-231">*連絡先リポジトリ サービス層のコードを格納するクラス ファイルを作成します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-231">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="cc5d2-232">使用して、追加するディレクティブ、 **ContactRepository.cs**モデル名前空間を追加するファイル。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-232">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="cc5d2-233">次の強調表示されたコードを追加、 **ContactRepository.cs** GetAllContacts メソッドを実装するファイル。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-233">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="cc5d2-234">(コード スニペット - *Web API のラボ - Ex01 - 連絡先リポジトリ*)</span><span class="sxs-lookup"><span data-stu-id="cc5d2-234">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="cc5d2-235">開く、 **ContactController.cs**ファイルがまだ開いていない場合。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-235">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="cc5d2-236">次の追加、ファイルの名前空間の宣言セクションにステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-236">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="cc5d2-237">次の強調表示されたコードを追加、 **ContactController.cs**サービス実装のメンバーが作成できるクラスの残りの部分を使用するために、リポジトリのインスタンスを表すプライベート フィールドを追加するクラス。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-237">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="cc5d2-238">(コード スニペット - *Web API のラボ - Ex01 - 連絡先のコント ローラー*)</span><span class="sxs-lookup"><span data-stu-id="cc5d2-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="cc5d2-239">変更、**取得**その it では、メソッド、連絡先リポジトリ サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-239">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="cc5d2-240">(コード スニペット -*リポジトリを使用して連絡先の一覧を返す Web API ラボ - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="cc5d2-240">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="cc5d2-241">ブレークポイントを設定、 **ContactController**の**取得**メソッドの定義。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-241">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="cc5d2-242">![連絡先のコント ローラーにブレークポイントを追加する](build-restful-apis-with-aspnet-web-api/_static/image17.png "連絡先のコント ローラーにブレークポイントを追加します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-242">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="cc5d2-243">*連絡先のコント ローラーにブレークポイントを追加します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-243">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="cc5d2-244">**F5** キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-244">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="cc5d2-245">ブラウザーが開き、キーを押して**F12**開発者ツールを開きます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-245">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="cc5d2-246">をクリックして、**ネットワーク**タブ。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-246">Click the **Network** tab.</span></span>
14. <span data-ttu-id="cc5d2-247">をクリックして、**キャプチャ開始**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-247">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="cc5d2-248">サフィックスを持つ、アドレス バーに URL を追加**api/連絡先**キーを押します **」と入力**API コント ローラーを読み込めません。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-248">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="cc5d2-249">Visual Studio 2012 が 1 回だけ中断する必要があります**取得**メソッドが実行を開始します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-249">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="cc5d2-250">![Get メソッド内での重大な](build-restful-apis-with-aspnet-web-api/_static/image18.png "Get メソッド内で互換性に影響します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-250">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="cc5d2-251">*Get メソッド内で互換性に影響します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-251">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="cc5d2-252">**F5** キーを押して続行します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-252">Press **F5** to continue.</span></span>
18. <span data-ttu-id="cc5d2-253">インターネット エクスプ ローラーに戻ってになっていないフォーカスがある場合。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-253">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="cc5d2-254">ネットワーク キャプチャのウィンドウに注意してください。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-254">Note the network capture window.</span></span>

    <span data-ttu-id="cc5d2-255">![Web API 呼び出しの結果を示すインターネット エクスプ ローラーのビューをネットワーク](build-restful-apis-with-aspnet-web-api/_static/image19.png "ネットワーク、Web API 呼び出しの結果を示すインターネット エクスプ ローラーのビュー")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-255">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="cc5d2-256">*Web API の呼び出しの結果を表示する Internet Explorer でネットワークの表示*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-256">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="cc5d2-257">をクリックして、**詳細ビューに移動**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-257">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="cc5d2-258">をクリックして、**応答本文**タブ。API の呼び出しと、サービス層によって取得された 2 つの連絡先を表示する方法の JSON の出力に注意してください。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-258">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="cc5d2-259">![開発者ツール ウィンドウで、Web API からの JSON 出力を表示する](build-restful-apis-with-aspnet-web-api/_static/image20.png "開発者ツール ウィンドウで、Web API からの JSON 出力の表示")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-259">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="cc5d2-260">*開発者ツール ウィンドウで、Web API からの JSON 出力の表示*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-260">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="cc5d2-261">手順 2: 読み取り/書き込み Web API の作成します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-261">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="cc5d2-262">この演習では、POST を実装したと PUT メソッドのデータ編集機能を有効にする連絡先のマネージャー。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-262">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="cc5d2-263">タスク 1 - Web API プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="cc5d2-263">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="cc5d2-264">このタスクでは、ユーザー入力を受け入れることができるように、演習 1 で作成した Web API プロジェクトを強化するために準備します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-264">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="cc5d2-265">実行**Visual Studio 2012 の Express for Web**に移動する**開始**と種類**VS Express for Web**キーを押します **」と入力**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-265">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="cc5d2-266">開く、**開始**ソリューションがある**ソース/Ex02-ReadWriteWebAPI/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-266">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="cc5d2-267">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-267">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="cc5d2-268">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-268">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="cc5d2-269">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-269">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="cc5d2-270">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-270">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="cc5d2-271">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-271">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="cc5d2-272">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-272">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="cc5d2-273">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-273">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="cc5d2-274">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-274">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="cc5d2-275">開く、 **Services/ContactRepository.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-275">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="cc5d2-276">タスク 2 - データ永続化機能を連絡先リポジトリの実装に追加します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-276">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="cc5d2-277">このタスクでは、ContactRepository クラスを永続化およびユーザーの入力と連絡先の新しいインスタンスをそのまま使用できるように、演習 1 で作成した Web API プロジェクトを強化します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-277">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="cc5d2-278">次の定数を追加、 **ContactRepository** web サーバー キャッシュ項目キー名この演習の後半の名を表すクラス。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-278">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="cc5d2-279">コンス トラクターを追加、 **ContactRepository**次のコードを格納しています。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-279">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="cc5d2-280">(コード スニペット - *Web API のラボ - Ex02 - 連絡先リポジトリ コンス トラクター*)</span><span class="sxs-lookup"><span data-stu-id="cc5d2-280">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="cc5d2-281">コードを変更する、 **GetAllContacts**メソッドを次のようにします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-281">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="cc5d2-282">(コード スニペット - *Web API のラボ - Ex02 - Get All Contacts*)</span><span class="sxs-lookup"><span data-stu-id="cc5d2-282">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="cc5d2-283">この例では、デモンストレーションのためは、値は同時に、複数のクライアントに利用できるではなく、セッションの記憶域メカニズムまたは要求の記憶域の有効期間を使用できるように、ストレージ メディアとして、web サーバーのキャッシュを使用します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-283">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="cc5d2-284">Entity Framework、XML ストレージ、またはその他のさまざまなを使用して、web サーバーのキャッシュの代わりに 1 つでした。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-284">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="cc5d2-285">という名前の新しいメソッドを実装**SaveContact**を**ContactRepository**の連絡先を保存する作業を行うクラス。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-285">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="cc5d2-286">**SaveContact**メソッドは、1 つを実行する必要があります**連絡先**パラメーターおよび戻り値のブール値の成功または失敗を示す値します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-286">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="cc5d2-287">(コード スニペット - *API ラボ - Ex02 - SaveContact メソッドを実装する Web*)</span><span class="sxs-lookup"><span data-stu-id="cc5d2-287">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="cc5d2-288">手順 3: HTML クライアントから Web API を使用します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-288">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="cc5d2-289">この演習では、Web API を呼び出す HTML クライアントを作成します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-289">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="cc5d2-290">このクライアントは、JavaScript を使用して Web API を使用したデータ交換を容易になり、HTML マークアップを使用して web ブラウザーで結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-290">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="cc5d2-291">タスク 1 - 連絡先を表示するための GUI を提供する、インデックス ビューを変更します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-291">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="cc5d2-292">このタスクでは、既存の連絡先の一覧を表示する、HTML ブラウザーでの要件をサポートする web アプリケーションの既定のインデックス ビューを変更します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-292">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="cc5d2-293">開く**Visual Studio 2012 の Express for Web**がまだ開いていない場合。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-293">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="cc5d2-294">開く、**開始**ソリューションがある**ソース/Ex03-ConsumingWebAPI/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-294">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="cc5d2-295">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-295">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="cc5d2-296">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-296">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="cc5d2-297">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-297">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="cc5d2-298">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-298">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="cc5d2-299">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-299">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="cc5d2-300">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-300">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="cc5d2-301">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-301">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="cc5d2-302">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-302">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="cc5d2-303">開く、 **Index.cshtml**ファイルにある**ビュー/ホーム**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-303">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="cc5d2-304">Id を持つ div 要素内の HTML コードを置き換える**本文**次のように見えるようにします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-304">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="cc5d2-305">Web API への HTTP 要求を実行するファイルの下部にある次の Javascript コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-305">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="cc5d2-306">開く、 **ContactController.cs**ファイルがまだ開いていない場合。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-306">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="cc5d2-307">ブレークポイントを配置、**取得**のメソッド、 **ContactController**クラス。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-307">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="cc5d2-308">![API コント ローラーの Get メソッドにブレークポイントを配置する](build-restful-apis-with-aspnet-web-api/_static/image21.png "API コント ローラーの Get メソッドにブレークポイントを配置します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-308">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="cc5d2-309">*API コント ローラーの Get メソッドにブレークポイントを配置します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-309">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="cc5d2-310">キーを押して**F5**プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-310">Press **F5** to run the project.</span></span> <span data-ttu-id="cc5d2-311">ブラウザーでは、HTML ドキュメントを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-311">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc5d2-312">アプリケーションのルート URL を参照していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-312">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="cc5d2-313">ページが読み込まれ、JavaScript の実行、ブレークポイントがヒットして、コント ローラーでは、コードの実行を一時停止します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-313">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="cc5d2-314">![VS Express for Web を使用する Web API 呼び出しにデバッグ](build-restful-apis-with-aspnet-web-api/_static/image22.png "Web の VS Express を使用して Web API の呼び出しにデバッグ")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-314">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="cc5d2-315">*Visual Studio 2012 Express for Web を使用する Web API 呼び出しにデバッグ*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-315">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="cc5d2-316">キーを押して、ブレークポイントを削除**F5**またはデバッグ ツールバーの**続行**ブラウザーで、ビューの読み込みを続行ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-316">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="cc5d2-317">Web API の呼び出しが完了すると、ブラウザーでリスト項目として表示されている呼び出し、Web API から返された連絡先が表示されます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-317">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="cc5d2-318">![リスト項目として、ブラウザーに表示される API 呼び出しの結果](build-restful-apis-with-aspnet-web-api/_static/image23.png "リスト項目として、ブラウザーに表示される API 呼び出しの結果")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-318">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="cc5d2-319">*リスト項目として、ブラウザーに表示される API 呼び出しの結果*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-319">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="cc5d2-320">デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-320">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="cc5d2-321">タスク 2 - 連絡先を作成するための GUI を提供する、インデックス ビューを変更します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-321">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="cc5d2-322">このタスクでは、MVC アプリケーションのインデックス ビューを変更する続けます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-322">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="cc5d2-323">フォームがユーザー入力をキャプチャして、新しい連絡先を作成する Web API に送信する HTML ページに追加され、GUI から日付を収集する新しい Web API コント ローラー メソッドが作成されます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-323">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="cc5d2-324">開く、 **ContactController.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-324">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="cc5d2-325">という名前のコント ローラー クラスに新しいメソッドを追加**Post**次のコードに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-325">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="cc5d2-326">(コード スニペット - *Web API のラボ - Ex03 の Post メソッド*)</span><span class="sxs-lookup"><span data-stu-id="cc5d2-326">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="cc5d2-327">開く、 **Index.cshtml** Visual Studio でファイルがまだ開いていない場合。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-327">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="cc5d2-328">前のタスクで追加した順序なしリストの直後後、ファイルに次の HTML コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-328">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="cc5d2-329">ドキュメントの下部にあるスクリプト要素内では、データを投稿、Web API HTTP POST 呼び出しを使用する ボタンのクリック イベントを処理する次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-329">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="cc5d2-330">**ContactController.cs**にブレークポイントを配置、 **Post**メソッド。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-330">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="cc5d2-331">キーを押して**F5**ブラウザーでアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-331">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="cc5d2-332">新しい連絡先の名前と Id をクリックしますをブラウザーでページが読み込まれると、入力、**保存**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-332">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="cc5d2-333">![クライアント HTML ドキュメントがブラウザーに読み込まれた](build-restful-apis-with-aspnet-web-api/_static/image24.png "クライアント HTML ドキュメントがブラウザーに読み込まれました。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-333">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="cc5d2-334">*クライアントの HTML ドキュメントがブラウザーに読み込まれました。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-334">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="cc5d2-335">デバッガー ウィンドウの分割される場合、 **Post**メソッド、プロパティの確認、**にお問い合わせください**パラメーター。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-335">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="cc5d2-336">値は、フォームに入力したデータを一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-336">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="cc5d2-337">![クライアントから Web API に送信される Contact オブジェクト](build-restful-apis-with-aspnet-web-api/_static/image25.png "Contact オブジェクトがクライアントから Web API に送信されます。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-337">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="cc5d2-338">*クライアントから Web API に送信される Contact オブジェクト*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-338">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="cc5d2-339">ステップ実行するまで、デバッガー内のメソッド、**応答**変数が作成されました。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-339">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="cc5d2-340">検査時に、**ローカル**デバッガーのウィンドウは、すべてのプロパティが設定されている表示されます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-340">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="cc5d2-341">![次のデバッガーで作成応答](build-restful-apis-with-aspnet-web-api/_static/image26.png "応答を次のデバッガーでの作成")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-341">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="cc5d2-342">*次のデバッガーでの作成の応答*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-342">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="cc5d2-343">キーを押す場合**F5**  をクリックしてまたは**続行**デバッガーで、要求が完了します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-343">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="cc5d2-344">によって格納されている連絡先の一覧に新しい連絡先が追加されて、ブラウザーに切り替えた後、 **ContactRepository**実装します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-344">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="cc5d2-345">![ブラウザーが新しい連絡先のインスタンスの作成に成功を反映して](build-restful-apis-with-aspnet-web-api/_static/image27.png "ブラウザーには、新しい連絡先のインスタンスの作成に成功が反映されます。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-345">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="cc5d2-346">*ブラウザーには、新しい連絡先のインスタンスの作成に成功が反映されます。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-346">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="cc5d2-347">また、Azure の次に、このアプリケーションを展開できます[付録 c: 発行 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixC)します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-347">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="cc5d2-348">まとめ</span><span class="sxs-lookup"><span data-stu-id="cc5d2-348">Summary</span></span>

<span data-ttu-id="cc5d2-349">新しい ASP.NET Web API フレームワークとフレームワークを使用して、RESTful Web Api の実装にこのラボでは紹介しました。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-349">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="cc5d2-350">ここでは、任意の数のメカニズムを使用してデータの永続化を容易にする新しいリポジトリを作成し、このラボでは、例として指定する単純なものではなくそのサービスを接続します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-350">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="cc5d2-351">Web API には、さまざまな HTTP と JSON または XML をサポートする任意の言語で記述された、HTML 以外のクライアントからの通信を有効にするなどの追加機能がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-351">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="cc5d2-352">一般的な web アプリケーションの外部で Web API をホストする機能も可能であれば、できるだけでなく、独自のシリアル化形式を作成する機能です。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-352">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="cc5d2-353">ASP.NET Web サイトで ASP.NET Web API フレームワークに専用の領域がある[ [ https://asp.net/web-api ](https://asp.net/web-api)](https://asp.net/web-api)します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-353">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="cc5d2-354">このサイトが引き続き最新の情報、サンプル、および Web API に関連するニュースご確認いただけます。 よくを提供する場合は、事実上あらゆるデバイスや開発フレームワークを使用できるカスタムの Web Api の作成のアートをもっと深く掘り下げてたいです。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-354">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="cc5d2-355">付録 a: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="cc5d2-355">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="cc5d2-356">コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-356">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="cc5d2-357">ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-357">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="cc5d2-358">![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](build-restful-apis-with-aspnet-web-api/_static/image28.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-358">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="cc5d2-359">*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-359">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="cc5d2-360">キーボード (c# のみ) を使用するコード スニペットを追加するには</span><span class="sxs-lookup"><span data-stu-id="cc5d2-360">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="cc5d2-361">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-361">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="cc5d2-362">スニペットの名前 (なし、スペースやハイフン) の入力を開始します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-362">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="cc5d2-363">スニペットの名前に一致する IntelliSense の表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-363">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="cc5d2-364">適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-364">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="cc5d2-365">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-365">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="cc5d2-366">![スニペットの名前の入力を開始](build-restful-apis-with-aspnet-web-api/_static/image29.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-366">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="cc5d2-367">*スニペットの名前の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-367">*Start typing the snippet name*</span></span>

    <span data-ttu-id="cc5d2-368">![強調表示されているスニペットを選択して Tab キーを押して](build-restful-apis-with-aspnet-web-api/_static/image30.png "キーを押してタブが強調表示されているスニペットを選択するには")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-368">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="cc5d2-369">*Tab キーを押して、強調表示されているスニペットを選択します*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-369">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="cc5d2-370">![キーを押して タブで再度とスニペットが展開](build-restful-apis-with-aspnet-web-api/_static/image31.png "キーを押して タブで再度とスニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-370">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="cc5d2-371">*キーを押して タブで再度とスニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-371">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="cc5d2-372">(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加するには</span><span class="sxs-lookup"><span data-stu-id="cc5d2-372">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="cc5d2-373">コード スニペットを挿入するを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-373">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="cc5d2-374">選択**スニペットの挿入**続けて**マイ コード スニペット**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-374">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="cc5d2-375">クリックして、一覧から関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-375">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="cc5d2-376">![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](build-restful-apis-with-aspnet-web-api/_static/image32.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-376">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="cc5d2-377">*コード スニペットを挿入して、スニペットの挿入先の選択します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-377">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="cc5d2-378">![クリックして、一覧から関連するスニペットを選択](build-restful-apis-with-aspnet-web-api/_static/image33.png "クリックして、一覧から関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-378">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="cc5d2-379">*クリックして、一覧から関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-379">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="cc5d2-380">付録 b: Installing Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="cc5d2-380">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="cc5d2-381">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="cc5d2-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="cc5d2-382">次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="cc5d2-383">移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="cc5d2-384">または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Azure SDK</em>&quot;します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="cc5d2-385">をクリックして**を今すぐインストール**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-385">Click on **Install Now**.</span></span> <span data-ttu-id="cc5d2-386">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="cc5d2-387">1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="cc5d2-388">![Visual Studio Express のインストール](build-restful-apis-with-aspnet-web-api/_static/image34.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-388">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="cc5d2-389">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="cc5d2-390">すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="cc5d2-392">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="cc5d2-393">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-393">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="cc5d2-395">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-395">*Installation progress*</span></span>
6. <span data-ttu-id="cc5d2-396">インストールが完了したら、クリックして**完了**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-396">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="cc5d2-398">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-398">*Installation completed*</span></span>
7. <span data-ttu-id="cc5d2-399">クリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="cc5d2-400">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web のタイル](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="cc5d2-402">*VS Express for Web のタイル*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-402">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="cc5d2-403">付録 c: Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="cc5d2-403">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="cc5d2-404">この付録では、Azure Portal から新しい web サイトを作成して Azure によって提供される、Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを発行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-404">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="cc5d2-405">タスク 1 - Azure Portal から新しい Web サイトの作成</span><span class="sxs-lookup"><span data-stu-id="cc5d2-405">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="cc5d2-406">移動して、 [Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-406">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc5d2-407">Azure を無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増加に応じてスケールできます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-407">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="cc5d2-408">サインアップする[ここ](http://aka.ms/aspnet-hol-azure)します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="cc5d2-409">![Windows Azure ポータルにログオン](build-restful-apis-with-aspnet-web-api/_static/image39.png "Windows Azure ポータルにログオン")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-409">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="cc5d2-410">*ポータルにログオン*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-410">*Log on to Portal*</span></span>
2. <span data-ttu-id="cc5d2-411">クリックして**新規**コマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="cc5d2-412">![新しい Web サイトを作成する](build-restful-apis-with-aspnet-web-api/_static/image40.png "新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-412">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="cc5d2-413">*新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="cc5d2-414">クリックして**コンピューティング** | **Web サイト**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="cc5d2-415">選び**簡易作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="cc5d2-416">新しい web サイトの URL を指定し、をクリックして**Web サイトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc5d2-417">Azure は、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-417">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="cc5d2-418">簡易作成 オプションを使用すると、ポータルの外部から Azure に完成した web アプリケーションをデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-418">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="cc5d2-419">データベースを設定するための手順は含まれません。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="cc5d2-420">![簡易作成を使用して新しい Web サイトを作成する](build-restful-apis-with-aspnet-web-api/_static/image41.png "簡易作成を使用して新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-420">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="cc5d2-421">*簡易作成を使用して新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="cc5d2-422">新しいまで待つ**Web サイト**が作成されます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="cc5d2-423">Web サイトを作成した後は、下のリンクをクリックして、 **URL**列。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="cc5d2-424">新しい Web サイトが動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="cc5d2-425">![新しい web サイトを参照](build-restful-apis-with-aspnet-web-api/_static/image42.png "新しい web サイトを参照")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-425">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="cc5d2-426">*新しい web サイトを参照*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="cc5d2-427">![Web サイトが実行されている](build-restful-apis-with-aspnet-web-api/_static/image43.png "実行している Web サイト")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-427">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="cc5d2-428">*実行している web サイト*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-428">*Web site running*</span></span>
6. <span data-ttu-id="cc5d2-429">ポータルに戻り、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="cc5d2-430">![Web サイトの管理ページを開く](build-restful-apis-with-aspnet-web-api/_static/image44.png "web サイトの管理ページを開く")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-430">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="cc5d2-431">*Web サイトの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="cc5d2-432">**ダッシュ ボード**ページの**概要**セクションで、、**発行プロファイルのダウンロード**リンク。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc5d2-433">*発行プロファイル*を有効になっているパブリケーションの各メソッドの Azure の web アプリケーションを発行するために必要な情報がすべて含まれます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-433">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="cc5d2-434">発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベースの文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="cc5d2-435">**Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートと、発行プロファイルのこれらのプログラムの構成を自動化するにはAzure に web アプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="cc5d2-436">![発行プロファイルのダウンロード web サイト](build-restful-apis-with-aspnet-web-api/_static/image45.png "発行プロファイルの web サイトのダウンロード")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-436">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="cc5d2-437">*発行プロファイルの Web サイトのダウンロード*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="cc5d2-438">既知の場所に発行プロファイル ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="cc5d2-439">さらにこの演習ではこのファイルを使用して、Visual Studio から Azure への web アプリケーションを発行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-439">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="cc5d2-440">![発行プロファイル ファイルを保存する](build-restful-apis-with-aspnet-web-api/_static/image46.png "発行プロファイルを保存しています")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-440">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="cc5d2-441">*発行プロファイル ファイルを保存しています*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-441">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="cc5d2-442">タスク 2 - データベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="cc5d2-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="cc5d2-443">アプリケーションが SQL Server を使用する場合のデータベースは SQL Database サーバーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="cc5d2-444">SQL Server を使用しない単純なアプリケーションをデプロイする場合は、このタスクをスキップする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="cc5d2-445">SQL Database サーバーは、アプリケーション データベースを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="cc5d2-446">SQL Database サーバーを表示するには、サブスクリプションで Azure 管理ポータルで**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**.</span><span class="sxs-lookup"><span data-stu-id="cc5d2-446">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="cc5d2-447">使用して 1 つを作成するには作成したサーバーがない、**追加**コマンド バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="cc5d2-448">メモ、**サーバー名および URL、管理者のログイン名とパスワード**次のタスクで使用すると、します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="cc5d2-449">データベースを作成しない、まだ、後の段階でそれが作成されます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="cc5d2-450">![SQL データベース サーバーのダッシュ ボード](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL データベース サーバーのダッシュ ボード")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-450">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="cc5d2-451">*SQL データベース サーバーのダッシュ ボード*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="cc5d2-452">次のタスクでは、サーバーの一覧で、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="cc5d2-453">次のようにクリックします**構成**、から IP アドレスを選択して**現在のクライアント IP アドレス**で貼り付けます、**開始 IP アドレス**と**終了 IP アドレス。** テキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png)ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![クライアント IP アドレスを追加します。](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="cc5d2-455">*クライアント IP アドレスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="cc5d2-456">1 回、**クライアント IP アドレス**が許可された IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![変更を確認します。](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="cc5d2-458">*変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-458">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="cc5d2-459">タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="cc5d2-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="cc5d2-460">ASP.NET MVC 4 のソリューションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="cc5d2-461">**ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="cc5d2-462">![アプリケーションの発行](build-restful-apis-with-aspnet-web-api/_static/image51.png "アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-462">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="cc5d2-463">*Web サイトの発行*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="cc5d2-464">最初のタスクで保存した発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="cc5d2-465">![発行プロファイルをインポートする](build-restful-apis-with-aspnet-web-api/_static/image52.png "発行プロファイルをインポートします。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-465">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="cc5d2-466">*発行プロファイルのインポート*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="cc5d2-467">クリックして**接続の検証**です。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-467">Click **Validate Connection**.</span></span> <span data-ttu-id="cc5d2-468">検証が完了したら、クリックして**次**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc5d2-469">接続の検証ボタンの横に表示される緑のチェック マークが表示されたら、検証が完了しました。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="cc5d2-470">![接続の検証](build-restful-apis-with-aspnet-web-api/_static/image53.png "接続の検証")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-470">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="cc5d2-471">*接続の検証*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-471">*Validating connection*</span></span>
4. <span data-ttu-id="cc5d2-472">**設定**ページの**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックします (つまり**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="cc5d2-473">![Web 配置の構成](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web 配置の構成")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-473">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="cc5d2-474">*Web 配置の構成*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="cc5d2-475">データベース接続を次のように構成します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="cc5d2-476">**サーバー名**SQL Database サーバーの URL を使用して、入力、 *tcp:* プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="cc5d2-477">**ユーザー名**サーバー管理者ログイン名を入力します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="cc5d2-478">**パスワード**サーバー管理者ログイン パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="cc5d2-479">たとえば、新しいデータベース名を入力: *MVC4SampleDB*します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-479">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="cc5d2-480">![ターゲットの接続文字列を構成する](build-restful-apis-with-aspnet-web-api/_static/image55.png "ターゲットの接続文字列を構成します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-480">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="cc5d2-481">*ターゲットの接続文字列を構成します。*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="cc5d2-482">次に、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-482">Then click **OK**.</span></span> <span data-ttu-id="cc5d2-483">データベースのクリックを作成するように求められたら**はい**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="cc5d2-484">![データベースを作成する](build-restful-apis-with-aspnet-web-api/_static/image56.png "データベース文字列を作成します。")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-484">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="cc5d2-485">*データベースの作成*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-485">*Creating the database*</span></span>
7. <span data-ttu-id="cc5d2-486">Windows azure SQL Database への接続に使用する接続文字列は、接続の既定のテキスト ボックス内に表示されます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="cc5d2-487">その後、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-487">Then click **Next**.</span></span>

    <span data-ttu-id="cc5d2-488">![SQL データベースを指す接続文字列](build-restful-apis-with-aspnet-web-api/_static/image57.png "SQL データベースを指す接続文字列")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-488">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="cc5d2-489">*SQL データベースを指す接続文字列*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="cc5d2-490">**プレビュー** ] ページで [**発行**します。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="cc5d2-491">![Web アプリケーションの発行](build-restful-apis-with-aspnet-web-api/_static/image58.png "web アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-491">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="cc5d2-492">*Web アプリケーションの発行*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="cc5d2-493">発行プロセスが完了すると、既定のブラウザーが発行した web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="cc5d2-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="cc5d2-494">![Windows Azure にアプリケーションが発行される](build-restful-apis-with-aspnet-web-api/_static/image59.png "アプリケーションは Windows Azure に発行")</span><span class="sxs-lookup"><span data-stu-id="cc5d2-494">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="cc5d2-495">*Azure に発行されたアプリケーション*</span><span class="sxs-lookup"><span data-stu-id="cc5d2-495">*Application published to Azure*</span></span>
