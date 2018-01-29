---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: "付録: 修正プログラム、サンプル アプリケーション (Azure での実際のクラウド アプリの構築) |Microsoft ドキュメント"
author: MikeWasson
description: "Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: c98e79bf8e9a1fe0899ed6d952c3e411ca472f7e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="59c36-104">付録: 修正プログラム、サンプル アプリケーション (Azure での実際のクラウド アプリの構築)</span><span class="sxs-lookup"><span data-stu-id="59c36-104">Appendix: The Fix It Sample Application (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="59c36-105">によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="59c36-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="59c36-106">プロジェクトに修正プログラムをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="59c36-106">Download The Fix It Project</span></span>](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> <span data-ttu-id="59c36-107">**現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。</span><span class="sxs-lookup"><span data-stu-id="59c36-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="59c36-108">13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。</span><span class="sxs-lookup"><span data-stu-id="59c36-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="59c36-109">電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="59c36-110">この付録の内容、実世界クラウド アプリのビルド Azure 電子書籍をするには、修正、サンプル アプリケーションをダウンロードすることに関する追加情報を提供する次のセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="59c36-110">This appendix to the Building Real World Cloud Apps with Azure e-book contains the following sections that provide additional information about the Fix It sample application that you can download:</span></span>

- [<span data-ttu-id="59c36-111">既知の問題</span><span class="sxs-lookup"><span data-stu-id="59c36-111">Known issues</span></span>](#knownissues)
- [<span data-ttu-id="59c36-112">ベスト プラクティス</span><span class="sxs-lookup"><span data-stu-id="59c36-112">Best practices</span></span>](#bestpractices)
- [<span data-ttu-id="59c36-113">ローカル コンピューターで Visual Studio からアプリを実行する方法</span><span class="sxs-lookup"><span data-stu-id="59c36-113">How to run the app from Visual Studio on your local computer</span></span>](#run-in-vs)
- [<span data-ttu-id="59c36-114">Azure App Service Web Apps を Windows PowerShell スクリプトを使用して基本アプリを展開する方法</span><span class="sxs-lookup"><span data-stu-id="59c36-114">How to deploy the base app to Azure App Service Web Apps by using the Windows PowerShell scripts</span></span>](#deploybase)
- [<span data-ttu-id="59c36-115">Windows PowerShell スクリプトのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="59c36-115">Troubleshooting the Windows PowerShell scripts</span></span>](#troubleshooting)
- [<span data-ttu-id="59c36-116">Azure App Service Web Apps を Azure クラウド サービスの処理キューにアプリを展開する方法</span><span class="sxs-lookup"><span data-stu-id="59c36-116">How to deploy the app with queue processing to Azure App Service Web Apps and an Azure Cloud Service</span></span>](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a><span data-ttu-id="59c36-117">既知の問題</span><span class="sxs-lookup"><span data-stu-id="59c36-117">Known issues</span></span>

<span data-ttu-id="59c36-118">修正、アプリはもともと限りシンプルにこの電子ブックに表示されるパターンを説明するために開発されました。</span><span class="sxs-lookup"><span data-stu-id="59c36-118">The Fix It app was originally developed in order to illustrate as simply as possible some of the patterns presented in this e-book.</span></span> <span data-ttu-id="59c36-119">ただし、電子書籍では、現実世界のアプリの構築方法であるためお対象となる修正コード レビューとテスト プロセスにリリースされたソフトウェアで行うようおに似ています。</span><span class="sxs-lookup"><span data-stu-id="59c36-119">However, since the e-book is about building real-world apps, we subjected the Fix It code to a review and testing process similar to what we'd do for released software.</span></span> <span data-ttu-id="59c36-120">問題の数が見つかりましたおよびと同様に、実際のアプリケーションでは、一部の修正は、以降のリリースにそれらのいくつかの遅延はします。</span><span class="sxs-lookup"><span data-stu-id="59c36-120">We found a number of issues, and as with any real-world application, some of them we fixed and some of them we deferred to a later release.</span></span>

<span data-ttu-id="59c36-121">次の一覧には、実稼働アプリケーションでは、修正、サンプル アプリケーションの最初のリリースのアドレスにないことにしました別の理由の 1 つの対処する必要がある問題が含まれています。</span><span class="sxs-lookup"><span data-stu-id="59c36-121">The following list includes issues that should be addressed in a production application, but for one reason or another we decided not to address in the initial release of the Fix It sample application.</span></span>

### <a name="security"></a><span data-ttu-id="59c36-122">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="59c36-122">Security</span></span>

- <span data-ttu-id="59c36-123">存在しない所有者にタスクを割り当てることはできませんを確認します。</span><span class="sxs-lookup"><span data-stu-id="59c36-123">Ensure that you can't assign a task to a non-existent owner.</span></span>
- <span data-ttu-id="59c36-124">実行できますのみを確認してください。 表示および作成するか、自分に割り当てられているタスクを変更します。</span><span class="sxs-lookup"><span data-stu-id="59c36-124">Ensure that you can only view and modify tasks that you created or are assigned to you.</span></span>
- <span data-ttu-id="59c36-125">サインイン ページと認証 cookie を HTTPS を使用します。</span><span class="sxs-lookup"><span data-stu-id="59c36-125">Use HTTPS for sign-in pages and authentication cookies.</span></span>
- <span data-ttu-id="59c36-126">認証 cookie の制限時間を指定します。</span><span class="sxs-lookup"><span data-stu-id="59c36-126">Specify a time limit for authentication cookies.</span></span>

### <a name="input-validation"></a><span data-ttu-id="59c36-127">入力の検証</span><span class="sxs-lookup"><span data-stu-id="59c36-127">Input validation</span></span>

<span data-ttu-id="59c36-128">一般に、運用アプリには操作修正アプリよりも多くの入力の検証。</span><span class="sxs-lookup"><span data-stu-id="59c36-128">In general, a production app would do more input validation than the Fix It app.</span></span> <span data-ttu-id="59c36-129">イメージのサイズなど、またはイメージ ファイルのサイズのアップロードを制限する必要があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-129">For example, the image size / image file size allowed for upload should be limited.</span></span>

### <a name="administrator-functionality"></a><span data-ttu-id="59c36-130">管理者の機能</span><span class="sxs-lookup"><span data-stu-id="59c36-130">Administrator functionality</span></span>

<span data-ttu-id="59c36-131">管理者は、既存のタスクの所有権を変更できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-131">An administrator should be able to change ownership on existing tasks.</span></span> <span data-ttu-id="59c36-132">など、タスクのクリエーターは誰は機関の管理アクセスが有効にしない限り、タスクを維持するために、終了、会社を残すことがあります。</span><span class="sxs-lookup"><span data-stu-id="59c36-132">For example, the creator of a task might leave the company, leaving no one with authority to maintain the task unless administrative access is enabled.</span></span>

### <a name="queue-message-processing"></a><span data-ttu-id="59c36-133">キュー メッセージの処理</span><span class="sxs-lookup"><span data-stu-id="59c36-133">Queue message processing</span></span>

<span data-ttu-id="59c36-134">キュー メッセージの処理の修正、アプリでは、最小限のコードでキューを中心とした作業のパターンを説明するために単純にするよう設計されました。</span><span class="sxs-lookup"><span data-stu-id="59c36-134">Queue message processing in the Fix It app was designed to be simple in order to illustrate the queue-centric work pattern with a minimum amount of code.</span></span> <span data-ttu-id="59c36-135">この単純なコードは、実稼働アプリケーションの適切なできません。</span><span class="sxs-lookup"><span data-stu-id="59c36-135">This simple code would not be adequate for an actual production application.</span></span>

- <span data-ttu-id="59c36-136">コードでは、各キューのメッセージが最大で 1 回処理されることは保証されません。</span><span class="sxs-lookup"><span data-stu-id="59c36-136">The code does not guarantee that each queue message will be processed at most once.</span></span> <span data-ttu-id="59c36-137">キューからメッセージを取得する場合は、によって、メッセージが他のキュー リスナーに表示されていない、タイムアウト期間。</span><span class="sxs-lookup"><span data-stu-id="59c36-137">When you get a message from the queue, there is a timeout period, during which the message is invisible to other queue listeners.</span></span> <span data-ttu-id="59c36-138">もう一度、メッセージが削除される前にタイムアウトになるには、メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="59c36-138">If the timeout expires before the message is deleted, the message becomes visible again.</span></span> <span data-ttu-id="59c36-139">そのため場合は、ワーカー ロール インスタンスは、メッセージの処理時間を費やす、可能であれば理論的には、同じメッセージに 2 回処理に対して、データベース内の重複するタスクの結果として得られます。</span><span class="sxs-lookup"><span data-stu-id="59c36-139">Therefore, if a worker role instance spends a long time processing a message, it is theoretically possible for the same message to get processed twice, resulting in a duplicate task in the database.</span></span> <span data-ttu-id="59c36-140">この問題の詳細については、次を参照してください。[を使用して Azure ストレージ キュー](https://msdn.microsoft.com/library/ff803365.aspx#sec7)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-140">For more information about this issue, see [Using Azure Storage Queues](https://msdn.microsoft.com/library/ff803365.aspx#sec7).</span></span>
- <span data-ttu-id="59c36-141">キューのポーリングのロジックは、メッセージの取得をバッチ処理によってよりコスト効果の高い可能性があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-141">The queue polling logic could be more cost-effective, by batching message retrieval.</span></span> <span data-ttu-id="59c36-142">呼び出すたびに[CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx)、トランザクション コストが発生します。</span><span class="sxs-lookup"><span data-stu-id="59c36-142">Every time you call [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), there is a transaction cost.</span></span> <span data-ttu-id="59c36-143">代わりに、呼び出せます[CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (複数形に注意してください ')、単一のトランザクションで複数のメッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="59c36-143">Instead, you can call [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (note the plural 's'), which gets multiple messages in a single transaction.</span></span> <span data-ttu-id="59c36-144">Azure ストレージ キューのトランザクションのコストは非常に低いコストへの影響ほとんどのシナリオで大きくします。</span><span class="sxs-lookup"><span data-stu-id="59c36-144">The transaction costs for Azure Storage Queues are very low, so the impact on costs is not substantial in most scenarios.</span></span>
- <span data-ttu-id="59c36-145">CPU 関係は、マルチコア仮想マシンを効率的に使用しない、キューのメッセージ処理のコードにループが発生します。</span><span class="sxs-lookup"><span data-stu-id="59c36-145">The tight loop in the queue message-processing code causes CPU affinity, which does not utilize multi-core VMs efficiently.</span></span> <span data-ttu-id="59c36-146">優れた設計では、同時に複数の非同期タスクを実行するのにタスクの並列化を使用します。</span><span class="sxs-lookup"><span data-stu-id="59c36-146">A better design would use task parallelism to run several async tasks in parallel.</span></span>
- <span data-ttu-id="59c36-147">キューのメッセージ処理は、のみ基本的な例外処理がします。</span><span class="sxs-lookup"><span data-stu-id="59c36-147">Queue message-processing has only rudimentary exception handling.</span></span> <span data-ttu-id="59c36-148">たとえば、コードを処理しない[有害メッセージ](https://msdn.microsoft.com/library/ms789028.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-148">For example, the code doesn't handle [poison messages](https://msdn.microsoft.com/library/ms789028.aspx).</span></span> <span data-ttu-id="59c36-149">(メッセージの処理には、例外が発生とする必要が、エラーを記録してメッセージを削除またはワーカー ロールは再度、プロセスを再試行してください、ループが無期限に続行されます。)</span><span class="sxs-lookup"><span data-stu-id="59c36-149">(When message processing causes an exception, you have to log the error and delete the message, or the worker role will try to process it again, and the loop will continue indefinitely.)</span></span>

### <a name="sql-queries-are-unbounded"></a><span data-ttu-id="59c36-150">SQL クエリの制限がないです。</span><span class="sxs-lookup"><span data-stu-id="59c36-150">SQL queries are unbounded</span></span>

<span data-ttu-id="59c36-151">現在の修正、コードでないを配置の制限数の行がインデックス ページのクエリが返す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-151">Current Fix It code places no limit on how many rows the queries for Index pages might return.</span></span> <span data-ttu-id="59c36-152">入力される場合は作業の量が多いデータベースに、受信した結果リストのサイズには、パフォーマンスの問題可能性があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-152">If a large volume of tasks is entered into the database, the size of the resulting lists received could cause performance issues.</span></span> <span data-ttu-id="59c36-153">ソリューションでは、ページングを実装します。</span><span class="sxs-lookup"><span data-stu-id="59c36-153">The solution is to implement paging.</span></span> <span data-ttu-id="59c36-154">例については、次を参照してください。[並べ替え、フィルター処理、および ASP.NET MVC アプリケーションで Entity Framework とページング](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-154">For an example, see [Sorting, Filtering, and Paging with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).</span></span>

### <a name="view-models-recommended"></a><span data-ttu-id="59c36-155">推奨モデルの表示</span><span class="sxs-lookup"><span data-stu-id="59c36-155">View models recommended</span></span>

<span data-ttu-id="59c36-156">修正アプリでは、FixItTask エンティティ クラスを使用して、コント ローラーとビューの間で情報を渡します。</span><span class="sxs-lookup"><span data-stu-id="59c36-156">The Fix It app uses the FixItTask entity class to pass information between the controller and the view.</span></span> <span data-ttu-id="59c36-157">モデルの表示を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="59c36-157">A best practice is to use view models.</span></span> <span data-ttu-id="59c36-158">データのプレゼンテーションについては、ビュー モデルをデザインするときにデータの永続化に必要なものを軸には、ドメイン モデル (FixItTask エンティティ クラスなど) を設計します。</span><span class="sxs-lookup"><span data-stu-id="59c36-158">The domain model (e.g., the FixItTask entity class) is designed around what is needed for data persistence, while a view model can be designed for data presentation.</span></span> <span data-ttu-id="59c36-159">詳細については、次を参照してください。 [12 の ASP.NET MVC のベスト プラクティス](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-159">For more information, see [12 ASP.NET MVC Best Practices](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).</span></span>

### <a name="secure-image-blob-recommended"></a><span data-ttu-id="59c36-160">推奨されるセキュリティで保護されたイメージの blob</span><span class="sxs-lookup"><span data-stu-id="59c36-160">Secure image blob recommended</span></span>

<span data-ttu-id="59c36-161">修正のアプリ ストアでは、イメージをパブリックとして URL を検索する人、イメージにアクセスできることを意味をアップロードします。</span><span class="sxs-lookup"><span data-stu-id="59c36-161">The Fix It app stores uploaded images as public, meaning that anyone who finds the URL can access the images.</span></span> <span data-ttu-id="59c36-162">パブリックではなく、イメージを保護する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-162">The images could be secured instead of public.</span></span>

### <a name="no-powershell-automation-scripts-for-queues"></a><span data-ttu-id="59c36-163">キューの PowerShell オートメーション スクリプトではありません。</span><span class="sxs-lookup"><span data-stu-id="59c36-163">No PowerShell automation scripts for queues</span></span>

<span data-ttu-id="59c36-164">PowerShell オートメーションのサンプル スクリプトは、修正、Azure App Service Web Apps でまったく実行されているの基本バージョンに対してのみ作成されています。</span><span class="sxs-lookup"><span data-stu-id="59c36-164">Sample PowerShell automation scripts were written only for the base version of Fix It that runs entirely in Azure App Service Web Apps.</span></span> <span data-ttu-id="59c36-165">設定して、web アプリケーションとキューの処理に必要なクラウド サービス環境への展開のおスクリプトを提供していません。</span><span class="sxs-lookup"><span data-stu-id="59c36-165">We haven't provided scripts for setting up and deploying to the web app plus Cloud Service environment required for queue processing.</span></span>

### <a name="special-handling-for-html-codes-in-user-input"></a><span data-ttu-id="59c36-166">ユーザー入力の HTML コードに対して特別な処理</span><span class="sxs-lookup"><span data-stu-id="59c36-166">Special handling for HTML codes in user input</span></span>

<span data-ttu-id="59c36-167">ASP.NET は、さまざまな方法が悪意のあるユーザーがユーザー入力テキスト ボックスにスクリプトを入力することでクロスサイト スクリプティング攻撃を試みる可能性がありますを自動的に防ぎます。</span><span class="sxs-lookup"><span data-stu-id="59c36-167">ASP.NET automatically prevents many ways in which malicious users might attempt cross-site scripting attacks by entering script in user input text boxes.</span></span> <span data-ttu-id="59c36-168">MVC`DisplayFor`タスクを表示するために使用するヘルパー タイトルされ、自動的に HTML エンコード値、ブラウザーに送信します。</span><span class="sxs-lookup"><span data-stu-id="59c36-168">And the MVC `DisplayFor` helper used to display task titles and notes automatically HTML-encodes values that it sends to the browser.</span></span> <span data-ttu-id="59c36-169">運用アプリケーションで別の対策をする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-169">But in a production app you might want to take additional measures.</span></span> <span data-ttu-id="59c36-170">詳細については、次を参照してください。 [ASP.NET での要求の検証](https://msdn.microsoft.com/library/hh882339.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-170">For more information, see [Request Validation in ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).</span></span>

<a id="bestpractices"></a>
## <a name="best-practices"></a><span data-ttu-id="59c36-171">ベスト プラクティス</span><span class="sxs-lookup"><span data-stu-id="59c36-171">Best practices</span></span>

<span data-ttu-id="59c36-172">コード レビューで発見し、修正にアプリの元のバージョンのテストの後に修正された問題のいくつかを次に示します。</span><span class="sxs-lookup"><span data-stu-id="59c36-172">Following are some issues that were fixed after being discovered in code review and testing of the original version of the Fix It app.</span></span> <span data-ttu-id="59c36-173">いない認識する特定のベスト プラクティスの一部だけで元コードの作成者によっていくつかが発生したため、コードが簡単に作成されており、リリースされたソフトウェアを想定していません。</span><span class="sxs-lookup"><span data-stu-id="59c36-173">Some were caused by the original coder not being aware of a particular best practice, some simply because the code was written quickly and wasn't intended for released software.</span></span> <span data-ttu-id="59c36-174">ここでの問題お一覧表示するしているは、このレビューからわかったためのものを使用する必要がある場合にしをテストすることもを他のユーザーの web アプリを開発しています。</span><span class="sxs-lookup"><span data-stu-id="59c36-174">We're listing the issues here in case there's something we learned from this review and testing that might be helpful to others who are also developing web apps.</span></span>

### <a name="dispose-the-database-repository"></a><span data-ttu-id="59c36-175">データベースのリポジトリを破棄します。</span><span class="sxs-lookup"><span data-stu-id="59c36-175">Dispose the database repository</span></span>

<span data-ttu-id="59c36-176">`FixItTaskRepository`クラスは、Entity Framework を破棄しなければならない`DbContext`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="59c36-176">The `FixItTaskRepository` class must dispose the Entity Framework `DbContext` instance.</span></span> <span data-ttu-id="59c36-177">実装することで、これを行った`IDisposable`で、`FixItTaskRepository`クラス。</span><span class="sxs-lookup"><span data-stu-id="59c36-177">We did this by implementing `IDisposable` in the `FixItTaskRepository` class:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

<span data-ttu-id="59c36-178">AutoFac が自動的に破棄することに注意してください、`FixItTaskRepository`インスタンス、ためこれを明示的に破棄する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="59c36-178">Note that AutoFac will automatically dispose the `FixItTaskRepository` instance, so we don't need to explicitly dispose it.</span></span>

<span data-ttu-id="59c36-179">削除することもできます、`DbContext`からメンバー変数`FixItTaskRepository`、ローカルを代わりに作成および`DbContext`各リポジトリ メソッド内で変数内、`using`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="59c36-179">Another option is to remove the `DbContext` member variable from `FixItTaskRepository`, and instead create a local `DbContext` variable within each repository method, inside a `using` statement.</span></span> <span data-ttu-id="59c36-180">例:</span><span class="sxs-lookup"><span data-stu-id="59c36-180">For example:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a><span data-ttu-id="59c36-181">DI でようなシングルトンを登録します。</span><span class="sxs-lookup"><span data-stu-id="59c36-181">Register singletons as such with DI</span></span>

<span data-ttu-id="59c36-182">以降のインスタンスを 1 つだけ、`PhotoService`クラスと`Logger`クラスが必要なこれらのクラスを指定する必要があります[依存関係の挿入の単一インスタンスとして登録されている](https://code.google.com/p/autofac/wiki/InstanceScope)で*DependenciesConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="59c36-182">Since only one instance of the `PhotoService` class and `Logger` class is needed, these classes should be [registered as single instances for dependency injection](https://code.google.com/p/autofac/wiki/InstanceScope) in *DependenciesConfig.cs*:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a><span data-ttu-id="59c36-183">セキュリティ: は、ユーザーにエラーの詳細を表示しません。</span><span class="sxs-lookup"><span data-stu-id="59c36-183">Security: Don't show error details to users</span></span>

<span data-ttu-id="59c36-184">オリジナルの修正、アプリがありませんでした汎用エラー ページがあると、UI でまでのすべての例外バブルのため、データベース接続エラーなどのいくつかの例外は、ブラウザーに表示されている完全なスタック トレースを可能性があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-184">The original Fix It app didn't have a generic error page and just let all exceptions bubble up to the UI, so some exceptions such as database connection errors could result in a full stack trace being displayed to the browser.</span></span> <span data-ttu-id="59c36-185">エラーの詳細については、悪意のあるユーザーの攻撃を促進できる場合があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-185">Detailed error information can sometimes facilitate attacks by malicious users.</span></span> <span data-ttu-id="59c36-186">ソリューションでは、例外の詳細を記録し、エラーの詳細が含まれていないことをユーザーにエラー ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="59c36-186">The solution is to log the exception details and display an error page to the user that doesn't include error details.</span></span> <span data-ttu-id="59c36-187">修正、アプリが既にログ記録、およびエラー ページを表示するために追加されました`<customErrors mode=On>`Web.config ファイルにします。</span><span class="sxs-lookup"><span data-stu-id="59c36-187">The Fix It app was already logging, and in order to display an error page, we added `<customErrors mode=On>` in the Web.config file.</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

<span data-ttu-id="59c36-188">これにより、既定では*Views\Shared\Error.cshtml*エラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="59c36-188">By default this causes *Views\Shared\Error.cshtml* to be displayed for errors.</span></span> <span data-ttu-id="59c36-189">カスタマイズできる*Error.cshtml*または独自のエラー ページのビューを作成し、追加、`defaultRedirect`属性。</span><span class="sxs-lookup"><span data-stu-id="59c36-189">You can customize *Error.cshtml* or create your own error page view and add a `defaultRedirect` attribute.</span></span> <span data-ttu-id="59c36-190">特定のエラーの別のエラー ページを指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="59c36-190">You can also specify different error pages for specific errors.</span></span>

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a><span data-ttu-id="59c36-191">セキュリティ: のみ、作成者によって編集するタスクを許可します。</span><span class="sxs-lookup"><span data-stu-id="59c36-191">Security: only allow a task to be edited by its creator</span></span>

<span data-ttu-id="59c36-192">悪意のあるユーザーは別のユーザーのタスクに ID を使用して URL を作成する可能性がありますが、ダッシュ ボードのインデックス ページでは、ユーザーのログオンによって作成されたタスクのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="59c36-192">The Dashboard Index page only shows tasks created by the logged-on user, but a malicious user could create a URL with an ID to another user's task.</span></span> <span data-ttu-id="59c36-193">内のコードを追加しました*DashboardController.cs*をその場合は 404 を返します。</span><span class="sxs-lookup"><span data-stu-id="59c36-193">We added code in *DashboardController.cs* to return a 404 in that case:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a><span data-ttu-id="59c36-194">例外を飲み込むしません。</span><span class="sxs-lookup"><span data-stu-id="59c36-194">Don't swallow exceptions</span></span>

<span data-ttu-id="59c36-195">オリジナルの修正アプリだけが null を返しました SQL クエリから生成される例外をログに記録後。</span><span class="sxs-lookup"><span data-stu-id="59c36-195">The original Fix It app just returned null after logging an exception that resulted from a SQL query:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

<span data-ttu-id="59c36-196">ユーザーを検索してクエリが成功しましたが、同じ行が返されませんでしたかのようになります。</span><span class="sxs-lookup"><span data-stu-id="59c36-196">This would make it look to the user as if the query succeeded but just didn't return any rows.</span></span> <span data-ttu-id="59c36-197">ソリューションでは、キャッチとログ記録の後に例外を再スローします。</span><span class="sxs-lookup"><span data-stu-id="59c36-197">Solution is to re-throw the exception after catching and logging:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a><span data-ttu-id="59c36-198">ワーカー ロールのすべての例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="59c36-198">Catch all exceptions in worker roles</span></span>

<span data-ttu-id="59c36-199">ワーカー ロールでハンドルされない例外、VM がリサイクルされると、try-catch ブロックを実行し、すべての例外を処理するすべてのものをラップするようにします。</span><span class="sxs-lookup"><span data-stu-id="59c36-199">Any unhandled exceptions in a worker role will cause the VM to be recycled, so you want to wrap everything you do in a try-catch block and handle all exceptions.</span></span>

### <a name="specify-length-for-string-properties-in-entity-classes"></a><span data-ttu-id="59c36-200">エンティティ クラスのプロパティの文字列の長を指定します。</span><span class="sxs-lookup"><span data-stu-id="59c36-200">Specify length for string properties in entity classes</span></span>

<span data-ttu-id="59c36-201">単純なコードを表示するために、修正、アプリの元のバージョンが FixItTask エンティティのフィールドの長さを指定していないし、その結果、データベース内には、varchar (max) として定義されました。</span><span class="sxs-lookup"><span data-stu-id="59c36-201">In order to display simple code, the original version of the Fix It app didn't specify lengths for the fields of the FixItTask entity, and as a result they were defined as varchar(max) in the database.</span></span> <span data-ttu-id="59c36-202">結果として、UI が、ほぼすべての量の入力を受け入れてください。</span><span class="sxs-lookup"><span data-stu-id="59c36-202">As a result, the UI would accept almost any amount of input.</span></span> <span data-ttu-id="59c36-203">両方のユーザーに適用される長さセット制限を指定するは、web ページと、データベース内の列のサイズに入力します。</span><span class="sxs-lookup"><span data-stu-id="59c36-203">Specifying lengths sets limits that apply both to user input in the web page and column size in the database:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a><span data-ttu-id="59c36-204">変更することが求められていない場合に、プライベート メンバーを読み取り専用としてマークします。</span><span class="sxs-lookup"><span data-stu-id="59c36-204">Mark private members as readonly when they aren't expected to change</span></span>

<span data-ttu-id="59c36-205">たとえば、`DashboardController`クラスのインスタンス`FixItTaskRepository`が作成され、変更するには想定されていませんとしては、定義したので[readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-205">For example, in the `DashboardController` class an instance of `FixItTaskRepository` is created and isn't expected to change, so we defined it as [readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx).</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a><span data-ttu-id="59c36-206">リストを使用します。一覧ではなく Any() です。Count() &gt; 0</span><span class="sxs-lookup"><span data-stu-id="59c36-206">Use list.Any() instead of list.Count() &gt; 0</span></span>

<span data-ttu-id="59c36-207">リスト内の 1 つまたは複数の項目が指定した条件に一致するかどうかについて注意する場合を使用して、[任意](https://msdn.microsoft.com/library/bb534972.aspx)メソッドを返すので条件に適合する項目が見つかるとすぐに対し、`Count`メソッドは、常に反復処理するのにはすべての項目。</span><span class="sxs-lookup"><span data-stu-id="59c36-207">If you all you care about is whether one or more items in a list fit the specified criteria, use the [Any](https://msdn.microsoft.com/library/bb534972.aspx) method, because it returns as soon as an item fitting the criteria is found, whereas the `Count` method always has to iterate through every item.</span></span> <span data-ttu-id="59c36-208">ダッシュ ボード*Index.cshtml*ファイルでこのコードは最初から。</span><span class="sxs-lookup"><span data-stu-id="59c36-208">The Dashboard *Index.cshtml* file originally had this code:</span></span>

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

<span data-ttu-id="59c36-209">これを変更しました。</span><span class="sxs-lookup"><span data-stu-id="59c36-209">We changed it to this:</span></span>

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a><span data-ttu-id="59c36-210">MVC ヘルパーを使用して MVC ビューの Url を生成します。</span><span class="sxs-lookup"><span data-stu-id="59c36-210">Generate URLs in MVC views using MVC helpers</span></span>

<span data-ttu-id="59c36-211">**作成、修正**修正アプリ ハード ホーム ページで、ボタンは、アンカー要素をコード化されました。</span><span class="sxs-lookup"><span data-stu-id="59c36-211">For the **Create a Fix It** button on the home page, the Fix It app hard coded an anchor element:</span></span>

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

<span data-ttu-id="59c36-212">次のように表示/アクションへのリンクは、使用する方がよい、 [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx)例については、HTML ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="59c36-212">For View/Action links like this it's better to use the [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) HTML helper, for example:</span></span>

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a><span data-ttu-id="59c36-213">ワーカー ロールで Thread.Sleep ではなく Task.Delay を使用します。</span><span class="sxs-lookup"><span data-stu-id="59c36-213">Use Task.Delay instead of Thread.Sleep in worker role</span></span>

<span data-ttu-id="59c36-214">新しいプロジェクト テンプレートの格納`Thread.Sleep`サンプルでは、ワーカー ロールが原因でスレッドをスリープさせることができる、スレッド プールに追加の不要なスレッドを生成します。</span><span class="sxs-lookup"><span data-stu-id="59c36-214">The new-project template puts `Thread.Sleep` in the sample code for a worker role, but causing the thread to sleep can cause the thread pool to spawn additional unnecessary threads.</span></span> <span data-ttu-id="59c36-215">使用することを回避する[Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx)代わりにします。</span><span class="sxs-lookup"><span data-stu-id="59c36-215">You can avoid that by using [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) instead.</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a><span data-ttu-id="59c36-216">Async void を回避します。</span><span class="sxs-lookup"><span data-stu-id="59c36-216">Avoid async void</span></span>

<span data-ttu-id="59c36-217">非同期のメソッドが値を返す必要がある場合を返す、`Task`型なく`void`です。</span><span class="sxs-lookup"><span data-stu-id="59c36-217">If an async method doesn't need to return a value, return a `Task` type rather than `void`.</span></span>

<span data-ttu-id="59c36-218">この例は、`FixItQueueManager`クラス。</span><span class="sxs-lookup"><span data-stu-id="59c36-218">This example is from the `FixItQueueManager` class:</span></span> 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

<span data-ttu-id="59c36-219">使用する必要があります`async void`トップレベルのイベント ハンドラーに対してのみです。</span><span class="sxs-lookup"><span data-stu-id="59c36-219">You should use `async void` only for top-level event handlers.</span></span> <span data-ttu-id="59c36-220">としてメソッドを定義する場合`async void`、呼び出し元のできません**await**メソッドまたはメソッドをスローするすべての例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="59c36-220">If you define a method as `async void`, the caller cannot **await** the method or catch any exceptions the method throws.</span></span> <span data-ttu-id="59c36-221">詳細については、次を参照してください。[非同期プログラミングのベスト プラクティス](https://msdn.microsoft.com/magazine/jj991977.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-221">For more information, see [Best Practices in Asynchronous Programming](https://msdn.microsoft.com/magazine/jj991977.aspx).</span></span> 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a><span data-ttu-id="59c36-222">キャンセル トークンを使用して、ワーカー ロールのループを中断するには</span><span class="sxs-lookup"><span data-stu-id="59c36-222">Use a cancellation token to break from worker role loop</span></span>

<span data-ttu-id="59c36-223">通常、**実行**ワーカー ロール上のメソッドには、無限ループが含まれています。</span><span class="sxs-lookup"><span data-stu-id="59c36-223">Typically, the **Run** method on a worker role contains an infinite loop.</span></span> <span data-ttu-id="59c36-224">ワーカー ロールを停止すると、 [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx)メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="59c36-224">When the worker role is stopping, the [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) method is called.</span></span> <span data-ttu-id="59c36-225">このメソッドを使用して内に行われている作業をキャンセルする必要があります、**実行**メソッドと終了適切にします。</span><span class="sxs-lookup"><span data-stu-id="59c36-225">You should use this method to cancel the work that is being done inside the **Run** method and exit gracefully.</span></span> <span data-ttu-id="59c36-226">それ以外の場合、プロセスは、操作の途中で終了可能性があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-226">Otherwise, the process might be terminated in the middle of an operation.</span></span>

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a><span data-ttu-id="59c36-227">自動 MIME スニッフィング プロシージャからオプトアウトします。</span><span class="sxs-lookup"><span data-stu-id="59c36-227">Opt out of Automatic MIME Sniffing Procedure</span></span>

<span data-ttu-id="59c36-228">場合によっては、Internet Explorer は web サーバーで指定された型とは異なる MIME の種類を報告します。</span><span class="sxs-lookup"><span data-stu-id="59c36-228">In some cases, Internet Explorer reports a MIME type different than the type specified by the web server.</span></span> <span data-ttu-id="59c36-229">たとえば、Internet Explorer が検出されると HTML コンテンツのコンテンツ タイプの HTTP 応答ヘッダーで提供されるファイルで: テキスト/プレーン、Internet Explorer の決定、コンテンツを HTML として表示するか。</span><span class="sxs-lookup"><span data-stu-id="59c36-229">For instance, if Internet Explorer finds HTML content in a file delivered with the HTTP response header Content-Type: text/plain, Internet Explorer determines that the content should be rendered as HTML.</span></span> <span data-ttu-id="59c36-230">残念ながら、この「MIME スニッフィング」も可能性が信頼されていないコンテンツをホストするサーバーのセキュリティの問題にあります。</span><span class="sxs-lookup"><span data-stu-id="59c36-230">Unfortunately, this "MIME-sniffing" can also lead to security problems for servers hosting untrusted content.</span></span> <span data-ttu-id="59c36-231">この問題を解決するには、Internet Explorer 8 MIME の種類の決定コードへの変更の数が確立してにより、アプリケーション開発者[MIME スニッフィングからオプトアウト](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-231">To combat this problem, Internet Explorer 8 has made a number of changes to MIME-type determination code and allows application developers to [opt out of MIME-sniffing](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx).</span></span> <span data-ttu-id="59c36-232">次のコードに追加された、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="59c36-232">The following code was added to the *Web.config* file.</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a><span data-ttu-id="59c36-233">バンドルと縮小が有効にします。</span><span class="sxs-lookup"><span data-stu-id="59c36-233">Enable bundling and minification</span></span>

<span data-ttu-id="59c36-234">Visual Studio では、新しい web プロジェクトを作成するときの JavaScript ファイルのバンドルと縮小は既定で有効ありません。</span><span class="sxs-lookup"><span data-stu-id="59c36-234">When Visual Studio creates a new web project, bundling and minification of JavaScript files is not enabled by default.</span></span> <span data-ttu-id="59c36-235">BundleConfig.cs でコードの行が追加されました。</span><span class="sxs-lookup"><span data-stu-id="59c36-235">We added a line of code in BundleConfig.cs:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a><span data-ttu-id="59c36-236">認証 cookie の有効期限タイムアウトを設定します。</span><span class="sxs-lookup"><span data-stu-id="59c36-236">Set an expiration time-out for authentication cookies</span></span>

<span data-ttu-id="59c36-237">既定では、認証 cookie は、2 週間以内に切れます。</span><span class="sxs-lookup"><span data-stu-id="59c36-237">By default, authentication cookies expire in two weeks.</span></span> <span data-ttu-id="59c36-238">時間が短い方が安全です。</span><span class="sxs-lookup"><span data-stu-id="59c36-238">A shorter time is more secure.</span></span> <span data-ttu-id="59c36-239">この設定を変更することができます*StartupAuth.cs*:</span><span class="sxs-lookup"><span data-stu-id="59c36-239">You can change this setting in *StartupAuth.cs*:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a><span data-ttu-id="59c36-240">ローカル コンピューターで Visual Studio からアプリを実行する方法</span><span class="sxs-lookup"><span data-stu-id="59c36-240">How to run the app from Visual Studio on your local computer</span></span>

<span data-ttu-id="59c36-241">これには修正アプリを実行する 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-241">There are two ways to run the Fix It app:</span></span>

- <span data-ttu-id="59c36-242">SQL データベースに直接書き込み、新しいタスク ベースのアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="59c36-242">Run the base application that writes new tasks directly to the SQL database.</span></span>
- <span data-ttu-id="59c36-243">キューに加えて、バックエンド サービスを使用してタスクを作成するアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="59c36-243">Run the application using a queue plus a backend service to create tasks.</span></span> <span data-ttu-id="59c36-244">キュー パターンが、章で説明されている[キュー中心作業パターン](queue-centric-work-pattern.md)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-244">The queue pattern is described in the chapter [Queue-Centric Work Pattern](queue-centric-work-pattern.md).</span></span>

<a id="runbase"></a>
### <a name="run-the-base-application"></a><span data-ttu-id="59c36-245">ベースのアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="59c36-245">Run the base application</span></span>

1. <span data-ttu-id="59c36-246">インストール[Visual Studio 2013 または Visual Studio 2013 Express for Web](https://www.visualstudio.com/downloads)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-246">Install [Visual Studio 2013 or Visual Studio 2013 Express for Web](https://www.visualstudio.com/downloads).</span></span>
2. <span data-ttu-id="59c36-247">インストール、 [Azure SDK for Visual Studio 2013 用の .NET です。](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="59c36-247">Install the [Azure SDK for .NET for Visual Studio 2013.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)</span></span>
3. <span data-ttu-id="59c36-248">.Zip ファイルをダウンロード、 [MSDN コード ギャラリー](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-248">Download the .zip file from the [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).</span></span>
4. <span data-ttu-id="59c36-249">ファイル エクスプ ローラーで、.zip ファイルを右クリックしてプロパティ をクリックし、プロパティ ウィンドウで ブロック解除 をクリックします。</span><span class="sxs-lookup"><span data-stu-id="59c36-249">In File Explorer, right-click the .zip file and click Properties, then in the Properties window click Unblock.</span></span>
5. <span data-ttu-id="59c36-250">ファイルを解凍します。</span><span class="sxs-lookup"><span data-stu-id="59c36-250">Unzip the file.</span></span>
6. <span data-ttu-id="59c36-251">Visual Studio を起動するには、ある .sln ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="59c36-251">Double-click the .sln file to launch Visual Studio.</span></span>
7. <span data-ttu-id="59c36-252">ツール] メニューの [ライブラリ パッケージ マネージャー、パッケージ マネージャー コンソールをクリックします。</span><span class="sxs-lookup"><span data-stu-id="59c36-252">From the Tools menu, click Library Package Manager, then Package Manager Console.</span></span>
8. <span data-ttu-id="59c36-253">パッケージ マネージャー コンソール (PMC) では、復元をクリックします。</span><span class="sxs-lookup"><span data-stu-id="59c36-253">In the Package Manager Console (PMC), click Restore.</span></span>
9. <span data-ttu-id="59c36-254">Visual Studio を終了します。</span><span class="sxs-lookup"><span data-stu-id="59c36-254">Exit Visual Studio.</span></span>
10. <span data-ttu-id="59c36-255">開始、 [Azure ストレージ エミュレーター](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-255">Start the [Azure storage emulator](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx).</span></span>
11. <span data-ttu-id="59c36-256">前の手順で閉じられたか、ソリューション ファイルを開いて、Visual Studio を再起動します。</span><span class="sxs-lookup"><span data-stu-id="59c36-256">Restart Visual Studio, opening the solution file you closed in the previous step.</span></span>
12. <span data-ttu-id="59c36-257">FixIt プロジェクトがスタートアップ プロジェクトとして設定を確認し、プロジェクトを実行するには、CTRL + F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="59c36-257">Make sure the FixIt project is set as the startup project, and then press CTRL+F5 to run the project.</span></span>

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a><span data-ttu-id="59c36-258">キューの処理で、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="59c36-258">Run the application with queue processing</span></span>

1. <span data-ttu-id="59c36-259">指示に従い[ベースのアプリケーションを実行](#runbase)ブラウザーを終了して、Visual Studio を閉じます。</span><span class="sxs-lookup"><span data-stu-id="59c36-259">Follow the directions for [Run the base application](#runbase), and then close the browser and close Visual Studio.</span></span>
2. <span data-ttu-id="59c36-260">管理者特権で Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="59c36-260">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="59c36-261">(Azure コンピューティング エミュレーターが使用される、管理者特権を必要とする)。</span><span class="sxs-lookup"><span data-stu-id="59c36-261">(You'll be using the Azure compute emulator, and that requires administrator privileges.)</span></span>
3. <span data-ttu-id="59c36-262">アプリケーションで*Web.config*ファイルを*MyFixIt* (web プロジェクト) をプロジェクトでの値を変更`appSettings/UseQueues`"true"にします。</span><span class="sxs-lookup"><span data-stu-id="59c36-262">In the application *Web.config* file in the *MyFixIt* project (the web project), change the value of `appSettings/UseQueues` to "true":</span></span> 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. <span data-ttu-id="59c36-263">場合、 [Azure ストレージ エミュレーター](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx)がまだ実行中、再び起動します。</span><span class="sxs-lookup"><span data-stu-id="59c36-263">If the [Azure storage emulator](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) isn't still running, start it again.</span></span>
5. <span data-ttu-id="59c36-264">FixIt web プロジェクトと MyFixItCloudService プロジェクトを同時に実行します。</span><span class="sxs-lookup"><span data-stu-id="59c36-264">Run the FixIt web project and the MyFixItCloudService project simultaneously.</span></span>

    <span data-ttu-id="59c36-265">Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="59c36-265">Using Visual Studio 2013:</span></span>

    1. <span data-ttu-id="59c36-266">F5 キーを押して FixIt プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="59c36-266">Press F5 to run the FixIt project.</span></span>
    2. <span data-ttu-id="59c36-267">**ソリューション エクスプ ローラー**MyFixItCloudService プロジェクトを右クリックし、クリックして**デバッグ** -- **新しいインスタンスを開始**です。</span><span class="sxs-lookup"><span data-stu-id="59c36-267">In **Solution Explorer**, right-click the MyFixItCloudService project, and then click **Debug** -- **Start New Instance**.</span></span>

    <span data-ttu-id="59c36-268">Visual Studio 2013 Express for Web の使用。</span><span class="sxs-lookup"><span data-stu-id="59c36-268">Using Visual Studio 2013 Express for Web:</span></span>

    1. <span data-ttu-id="59c36-269">ソリューション エクスプ ローラーで FixIt ソリューションを右クリックし、**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="59c36-269">In Solution Explorer, right-click the FixIt solution and select **Properties**.</span></span>
    2. <span data-ttu-id="59c36-270">選択**マルチ スタートアップ プロジェクト**.</span><span class="sxs-lookup"><span data-stu-id="59c36-270">Select **Multiple Startup Projects**..</span></span>
    3. <span data-ttu-id="59c36-271">**アクション**MyFixIt と MyFixItCloudService、下にあるドロップダウン リストを選択**開始**です。</span><span class="sxs-lookup"><span data-stu-id="59c36-271">In the **Action** dropdown list under MyFixIt and MyFixItCloudService, select **Start**.</span></span>
    4. <span data-ttu-id="59c36-272">**[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="59c36-272">Click **OK**.</span></span>
    5. <span data-ttu-id="59c36-273">両方のプロジェクトを実行する場合は F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="59c36-273">Press F5 to run both projects.</span></span>

    <span data-ttu-id="59c36-274">MyFixItCloudService プロジェクトを実行するときに、Visual Studio は、Azure コンピューティング エミュレーターを起動します。</span><span class="sxs-lookup"><span data-stu-id="59c36-274">When you run the MyFixItCloudService project, Visual Studio starts the Azure compute emulator.</span></span> <span data-ttu-id="59c36-275">ファイアウォールの構成によっては、エミュレーターがファイアウォールを通過を許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-275">Depending on your firewall configuration, you might need to allow the emulator through the firewall.</span></span>

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a><span data-ttu-id="59c36-276">Azure App Service Web Apps を Windows PowerShell スクリプトを使用して基本アプリを展開する方法</span><span class="sxs-lookup"><span data-stu-id="59c36-276">How to deploy the base app to Azure App Service Web Apps by using the Windows PowerShell scripts</span></span>

<span data-ttu-id="59c36-277">説明するために、[自動化すべて](automate-everything.md)パターンでは、修正、アプリの azure 環境を設定して、プロジェクトを新しい環境に配置するスクリプトが付属します。</span><span class="sxs-lookup"><span data-stu-id="59c36-277">To illustrate the [Automate Everything](automate-everything.md) pattern, the Fix It app is supplied with scripts that set up an environment in Azure and deploy the project to the new environment.</span></span> <span data-ttu-id="59c36-278">次の手順では、スクリプトを使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="59c36-278">The following instructions explain how to use the scripts.</span></span>

<span data-ttu-id="59c36-279">キューを使用せずに Azure で実行する、キューでローカルで実行する変更を加えた場合は、元に戻す、次の手順に進む前に UseQueues appSetting 値を設定することを確認します。</span><span class="sxs-lookup"><span data-stu-id="59c36-279">If you want to run in Azure without using queues, and you made the changes to run locally with queues, make sure you set the UseQueues appSetting value back to false before proceeding with the following instructions.</span></span>

<span data-ttu-id="59c36-280">これらの手順は、既にダウンロードしていると、修正、ソリューションをローカルで実行し、Azure があるかアカウントまたは Azure サブスクリプションを持っている想定しています。 管理を承認されています。</span><span class="sxs-lookup"><span data-stu-id="59c36-280">These instructions assume you have already downloaded and run the Fix It solution locally, and that you have an Azure account or have an Azure subscription that you are authorized to manage.</span></span>

1. <span data-ttu-id="59c36-281">インストール、 **Azure PowerShell**コンソールです。</span><span class="sxs-lookup"><span data-stu-id="59c36-281">Install the **Azure PowerShell** console.</span></span> <span data-ttu-id="59c36-282">手順については、次を参照してください。[をインストールして、Azure PowerShell を構成する方法](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-282">For instructions, see [How to install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).</span></span>

    <span data-ttu-id="59c36-283">このカスタマイズされたコンソールは、Azure サブスクリプションを使用するように構成します。</span><span class="sxs-lookup"><span data-stu-id="59c36-283">This customized console is configured to work with your Azure subscription.</span></span> <span data-ttu-id="59c36-284">Azure モジュールがインストールされている、 *Program Files* directory、Azure PowerShell コンソールを使用するたびに自動的にインポートしているとします。</span><span class="sxs-lookup"><span data-stu-id="59c36-284">The Azure module is installed in the *Program Files* directory and is automatically imported on every use of the Azure PowerShell console.</span></span>

    <span data-ttu-id="59c36-285">Windows PowerShell ISE などの別のホスト プログラムで作業できるようにする場合を必ず使用して、 [Import-module](https://go.microsoft.com/fwlink/?LinkID=141553)コマンドレットを Azure モジュールをインポートまたは Azure モジュールのコマンドを使用して、モジュールの自動インポートを開始します。</span><span class="sxs-lookup"><span data-stu-id="59c36-285">If you prefer to work in a different host program, such as Windows PowerShell ISE, be sure to use the [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) cmdlet to import the Azure module or use a command in the Azure module to trigger automatic importing of the module.</span></span>
2. <span data-ttu-id="59c36-286">Azure PowerShell を起動、**管理者として実行**オプション。</span><span class="sxs-lookup"><span data-stu-id="59c36-286">Start Azure PowerShell with the **Run as administrator** option.</span></span>
3. <span data-ttu-id="59c36-287">実行、 [Set-executionpolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) Azure PowerShell 実行ポリシーを設定するコマンドレット`RemoteSigned`です。</span><span class="sxs-lookup"><span data-stu-id="59c36-287">Run the [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) cmdlet to set the Azure PowerShell execution policy to `RemoteSigned`.</span></span> <span data-ttu-id="59c36-288">入力**Y** (Yes) のポリシーの変更を完了します。</span><span class="sxs-lookup"><span data-stu-id="59c36-288">Enter **Y** (for Yes) to complete the policy change.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    <span data-ttu-id="59c36-289">この設定では、デジタル署名されていないローカル スクリプトを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="59c36-289">This setting enables you to run local scripts that aren't digitally signed.</span></span> <span data-ttu-id="59c36-290">(を実行ポリシーを設定することもできます`Unrestricted`後で、ブロックを解除する手順の必要はありませんが、これは、セキュリティ上の理由から推奨されません。)。</span><span class="sxs-lookup"><span data-stu-id="59c36-290">(You can also set the execution policy to `Unrestricted`, which would eliminate the need for the unblock step later, but this is not recommended for security reasons.)</span></span>
4. <span data-ttu-id="59c36-291">実行、`Add-AzureAccount`コマンドレットを PowerShell を設定するアカウントの資格情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="59c36-291">Run the `Add-AzureAccount` cmdlet to set up PowerShell with credentials for your account.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    <span data-ttu-id="59c36-292">これらの資格情報は、一定の時間後に期限切れし、再実行する必要がある、`Add-AzureAccount`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="59c36-292">These credentials expire after a period of time and you have to re-run the `Add-AzureAccount` cmdlet.</span></span> <span data-ttu-id="59c36-293">この電子書籍が書き込まれると資格情報が期限切れ前に時間制限は 12 時間以内にです。</span><span class="sxs-lookup"><span data-stu-id="59c36-293">As this e-book is being written, the time limit before credentials expire is 12 hours.</span></span>
5. <span data-ttu-id="59c36-294">複数のサブスクリプションがある場合は、Select-azuresubscription コマンドレットを使用して、テスト環境を作成するサブスクリプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="59c36-294">If you have multiple subscriptions, use the Select-AzureSubscription cmdlet to specify the subscription you want to create the test environment in.</span></span>
6. <span data-ttu-id="59c36-295">使用して、同じ Azure サブスクリプションの管理証明書をインポート、`Get-AzurePublishSettingsFile`と`Import-AzurePublishSettingsFile`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="59c36-295">Import a management certificate for the same Azure subscription by using the `Get-AzurePublishSettingsFile` and `Import-AzurePublishSettingsFile` cmdlets.</span></span> <span data-ttu-id="59c36-296">これらのコマンドレットの最初の証明書ファイルをダウンロードして、2 番目の 1 つで、インポートするためにそのファイルの場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="59c36-296">The first of these cmdlets downloads a certificate file, and in the second one you specify the location of that file in order to import it.</span></span> > [!IMPORTANT]
 > <span data-ttu-id="59c36-297">安全な場所にダウンロードしたファイルを保持または削除、操作を終了すると、Azure サービスを管理するために使用する証明書が含まれています。</span><span class="sxs-lookup"><span data-stu-id="59c36-297">Keep the downloaded file in a safe location or delete it when you're done with it, because it contains a certificate that can be used to manage your Azure services.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    <span data-ttu-id="59c36-298">証明書が使用される SQL データベース サーバー上のファイアウォール ルールを設定するのには、開発用コンピューターの IP アドレスを検出する REST API の呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="59c36-298">The certificate is used for a REST API call that detects the development machine's IP address in order to set a firewall rule on the SQL Database server.</span></span>
7. <span data-ttu-id="59c36-299">実行、 [Set-location](https://go.microsoft.com/fwlink/p/?linkid=293912)コマンドレット (エイリアスは`cd`、`chdir`と`sl`)、スクリプトが含まれているディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="59c36-299">Run the [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (aliases are `cd`, `chdir`, and `sl`) to navigate to the directory that contains the scripts.</span></span> <span data-ttu-id="59c36-300">(ファイルに配置している、*オートメーション*修正ソリューション フォルダー内のフォルダーです)。ディレクトリ名にスペースが含まれている場合は、引用符で囲まれたパスを配置します。</span><span class="sxs-lookup"><span data-stu-id="59c36-300">(They're located in the *Automation* folder in the Fix It solution folder.) Put the path in quotes if any of the directory names contain spaces.</span></span> <span data-ttu-id="59c36-301">たとえばに移動するため、`c:\Sample Apps\FixIt\Automation`ディレクトリは、次のコマンドを入力する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-301">For example, to navigate to the `c:\Sample Apps\FixIt\Automation` directory you could enter the following command:</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. <span data-ttu-id="59c36-302">これらのスクリプトを実行する Windows PowerShell を許可するのには、使用、 [Unblock-file](https://go.microsoft.com/fwlink/p/?linkid=294021)コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="59c36-302">To allow Windows PowerShell to run these scripts, use the [Unblock-File](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet.</span></span> <span data-ttu-id="59c36-303">(スクリプトがブロックされている、インターネットからダウンロードされたことがあるためです。)</span><span class="sxs-lookup"><span data-stu-id="59c36-303">(The scripts are blocked because they were downloaded from the Internet.)</span></span>

    > [!WARNING]
    > <span data-ttu-id="59c36-304">セキュリティ - 実行する前に`Unblock-File`任意のスクリプトまたは実行可能ファイルを使用して、ファイルをメモ帳で開きます、コマンドを確認し、悪意のあるコードが含まれていないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="59c36-304">Security - Before running `Unblock-File` on any script or executable file, open the file in Notepad, examine the commands, and verify that they do not contain any malicious code.</span></span>

    <span data-ttu-id="59c36-305">たとえば、次のコマンドを実行、`Unblock-File`コマンドレットは、現在のディレクトリ内のすべてのスクリプトにします。</span><span class="sxs-lookup"><span data-stu-id="59c36-305">For example, the following command runs the `Unblock-File` cmdlet on all scripts in the current directory.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. <span data-ttu-id="59c36-306">(キューの処理) ベースの web アプリを作成するには、アプリを修正、環境の作成スクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="59c36-306">To create the web app for the base (no queues processing) Fix It app, run the environment creation script.</span></span>

    <span data-ttu-id="59c36-307">必要な`Name`パラメーターが、データベースの名前を指定しはスクリプトを作成する記憶域アカウントにも使用します。</span><span class="sxs-lookup"><span data-stu-id="59c36-307">The required `Name` parameter specifies the name of the database and is also used for the storage account that the script creates.</span></span> <span data-ttu-id="59c36-308">名前は、azurewebsites.net ドメイン内でグローバルに一意にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-308">The name must be globally unique within the azurewebsites.net domain.</span></span> <span data-ttu-id="59c36-309">Fixit またはテストと同様に (または、fixitdemo 例のようにも)、重複する名前を指定する場合、`New-AzureWebsite`コマンドレットは、競合を報告する内部エラーで失敗します。</span><span class="sxs-lookup"><span data-stu-id="59c36-309">If you specify a name that is not unique, like Fixit or Test (or even as in the example, fixitdemo), the `New-AzureWebsite` cmdlet fails with an Internal Error that reports a conflict.</span></span> <span data-ttu-id="59c36-310">スクリプトは、web アプリ、ストレージ アカウント、およびデータベースの名前の要件に準拠するすべて小文字に名前を変換します。</span><span class="sxs-lookup"><span data-stu-id="59c36-310">The script converts the name to all lower-case to comply with name requirements for web apps, storage accounts, and databases.</span></span>

    <span data-ttu-id="59c36-311">必要な`SqlDatabasePassword`パラメーターは、SQL データベースに作成される管理者アカウントのパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="59c36-311">The required `SqlDatabasePassword` parameter specifies the password for the admin account that will be created for SQL Database.</span></span> <span data-ttu-id="59c36-312">パスワードに特殊な XML 文字を含めないでください (&amp; &lt; &gt; ;)。</span><span class="sxs-lookup"><span data-stu-id="59c36-312">Don't include special XML characters in the password (&amp; &lt; &gt; ;).</span></span> <span data-ttu-id="59c36-313">これは、スクリプト作成された、Azure の制限ではない方法を制限します。</span><span class="sxs-lookup"><span data-stu-id="59c36-313">This is a limitation of the way the scripts were written, not a limitation of Azure.</span></span>

    <span data-ttu-id="59c36-314">たとえば、"fixitdemo"をという名前の web アプリを作成して"Passw0rd1"の SQL Server 管理者のパスワードを使用する場合は、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="59c36-314">For example, if you want to create a web app named "fixitdemo" and use a SQL Server administrator password of "Passw0rd1", you could enter the following command:</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    <span data-ttu-id="59c36-315">Azurewebsites.net ドメインで名前が一意である必要があり、パスワードがパスワードの複雑さの SQL データベースの要件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-315">The name must be unique in the azurewebsites.net domain, and the password must meet SQL Database requirements for password complexity.</span></span> <span data-ttu-id="59c36-316">(例 Passw0rd1 が要件を満たしています。)</span><span class="sxs-lookup"><span data-stu-id="59c36-316">(The example Passw0rd1 does meet the requirements.)</span></span>

    <span data-ttu-id="59c36-317">コマンドの最初に注意してください"です。\".</span><span class="sxs-lookup"><span data-stu-id="59c36-317">Note that the command begins with ".\".</span></span> <span data-ttu-id="59c36-318">スクリプトの悪意のある実行を防ぐため、Windows PowerShell では、スクリプトを実行するときに、スクリプト ファイルへの完全修飾パスを指定することが必要です。</span><span class="sxs-lookup"><span data-stu-id="59c36-318">To help prevent malicious execution of scripts, Windows PowerShell requires that you provide the fully qualified path to the script file when you run a script.</span></span> <span data-ttu-id="59c36-319">現在のディレクトリを示すために、ドットを使用することができます (".\")または、ように、完全修飾パスを提供します。</span><span class="sxs-lookup"><span data-stu-id="59c36-319">You can use a dot to indicate the current directory (".\") or provide the fully qualified path, such as:</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    <span data-ttu-id="59c36-320">スクリプトの詳細については、使用、`Get-Help`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="59c36-320">For more information about the script, use the `Get-Help` cmdlet.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    <span data-ttu-id="59c36-321">使用することができます、 `Detailed`、 `Full`、 `Parameters`、および`Examples`返されるヘルプのフィルター処理、Get-help コマンドレットのパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="59c36-321">You can use the `Detailed`, `Full`, `Parameters`, and `Examples` parameters of the Get-Help cmdlet to filter the help that is returned.</span></span>

    <span data-ttu-id="59c36-322">スクリプトが失敗するか、「New-azurewebsite:: 呼び出す Set-azuresubscription と Select-azuresubscription 最初に、」などのエラーが生成される場合は、Azure PowerShell の構成を完了していない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-322">If the script fails or generates errors, such as "New-AzureWebsite : Call Set-AzureSubscription and Select-AzureSubscription first," you might not have completed the configuration of Azure PowerShell.</span></span>

    <span data-ttu-id="59c36-323">示すように作成されたリソースを表示する、Azure 管理ポータルを使用するには、スクリプトの終了後、[自動化すべて](automate-everything.md)章します。</span><span class="sxs-lookup"><span data-stu-id="59c36-323">After the script finishes, you can use the Azure Management Portal to see the resources that were created, as shown in the [Automate Everything](automate-everything.md) chapter.</span></span>
10. <span data-ttu-id="59c36-324">新しい Azure 環境に FixIt プロジェクトを配置するには使用、 *AzureWebsite.ps1*スクリプト。</span><span class="sxs-lookup"><span data-stu-id="59c36-324">To deploy the FixIt project to the new Azure environment, use the *AzureWebsite.ps1* script.</span></span> <span data-ttu-id="59c36-325">例:</span><span class="sxs-lookup"><span data-stu-id="59c36-325">For example:</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    <span data-ttu-id="59c36-326">展開が完了したら、修正、Azure で実行すると、ブラウザーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="59c36-326">When deployment is done, the browser opens with Fix It running in Azure.</span></span>

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a><span data-ttu-id="59c36-327">Windows PowerShell スクリプトのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="59c36-327">Troubleshooting the Windows PowerShell scripts</span></span>

<span data-ttu-id="59c36-328">これらのスクリプトを実行しているときに発生する最も一般的なエラーは、アクセス許可に関連付けられます。</span><span class="sxs-lookup"><span data-stu-id="59c36-328">The most common errors encountered when running these scripts are related to permissions.</span></span> <span data-ttu-id="59c36-329">確認して`Add-AzureAccount`と`Import-AzurePublishSettingsFile`が成功したこと、および同じ Azure サブスクリプションに使用します。</span><span class="sxs-lookup"><span data-stu-id="59c36-329">Make sure that `Add-AzureAccount` and `Import-AzurePublishSettingsFile` were successful and that you used them for the same Azure subscription.</span></span> <span data-ttu-id="59c36-330">場合でも`Add-AzureAccount`が正常に実行する必要があります、再度実行します。</span><span class="sxs-lookup"><span data-stu-id="59c36-330">Even if `Add-AzureAccount` was successful you might have to run it again.</span></span> <span data-ttu-id="59c36-331">によって追加されたアクセス許可`Add-AzureAccount`12 時間以内に期限切れにします。</span><span class="sxs-lookup"><span data-stu-id="59c36-331">The permissions added by `Add-AzureAccount` expire in 12 hours.</span></span>

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a><span data-ttu-id="59c36-332">オブジェクト参照がオブジェクト インスタンスに設定されていません。</span><span class="sxs-lookup"><span data-stu-id="59c36-332">Object reference not set to an instance of an object.</span></span>

<span data-ttu-id="59c36-333">「オブジェクト参照がオブジェクトのインスタンスに設定されていない」つまり Windows PowerShell がプロセス (これは、null 参照の例外です) にオブジェクトを検出できないことを実行など、スクリプトがエラーを返した場合、`Add-AzureAccount`コマンドレットとスクリプトをもう一度やり直してください。</span><span class="sxs-lookup"><span data-stu-id="59c36-333">If the script returns errors, such as "Object reference not set to an instance of an object," which means that Windows PowerShell can't find an object to process (this is a null reference exception), run the `Add-AzureAccount` cmdlet and try the script again.</span></span>

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a><span data-ttu-id="59c36-334">InternalError: サーバーには、内部エラーが発生しました。</span><span class="sxs-lookup"><span data-stu-id="59c36-334">InternalError: The server encountered an internal error.</span></span>

<span data-ttu-id="59c36-335">`New-AzureWebsite`コマンドレットでは、名前は azurewebsites.net ドメイン内で一意ではないときに内部エラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="59c36-335">The `New-AzureWebsite` cmdlet returns an internal error when the name is not unique in the azurewebsites.net domain.</span></span> <span data-ttu-id="59c36-336">エラーを解決するには、Name パラメーターでは、名前の別の値を使用して*新規 AzureWebsiteEnv.ps1*です。</span><span class="sxs-lookup"><span data-stu-id="59c36-336">To resolve the error, use a different value for the name, which is in the Name parameter of *New-AzureWebsiteEnv.ps1*.</span></span>

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a><span data-ttu-id="59c36-337">スクリプトを再起動します。</span><span class="sxs-lookup"><span data-stu-id="59c36-337">Restarting the script</span></span>

<span data-ttu-id="59c36-338">再起動する必要がある場合、*新規 AzureWebsiteEnv.ps1*スクリプト「スクリプトが完了しました」のメッセージを印刷する前に失敗したためをする前に作成したスクリプトが停止しているリソースを削除する場合があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-338">If you need to restart the *New-AzureWebsiteEnv.ps1* script because it failed before it printed the "Script is complete" message, you might want to delete resources that the script created before it stopped.</span></span> <span data-ttu-id="59c36-339">たとえば、スクリプトが作成されたは既に ContosoFixItDemo web アプリと同じ名前のスクリプトをもう一度実行する場合場合、名前が使用されているためにのスクリプトでは失敗します。</span><span class="sxs-lookup"><span data-stu-id="59c36-339">For example, if the script already created the ContosoFixItDemo web app and you run the script again with the same name, the script will fail because the name is in use.</span></span>

<span data-ttu-id="59c36-340">リソースを決定するには、は、停止、前に作成したスクリプトは、次のコマンドレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="59c36-340">To determine which resources the script created before it stopped, use the following cmdlets:</span></span>

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- <span data-ttu-id="59c36-341">`Get-AzureSqlDatabase`: このコマンドレットを実行するにはパイプを使用するデータベース サーバーの名前`Get-AzureSqlDatabase`:</span><span class="sxs-lookup"><span data-stu-id="59c36-341">`Get-AzureSqlDatabase`: To run this cmdlet, pipe the database server name to `Get-AzureSqlDatabase`:</span></span>  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

<span data-ttu-id="59c36-342">これらのリソースを削除するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="59c36-342">To delete these resources, use the following commands.</span></span> <span data-ttu-id="59c36-343">場合、データベース サーバーを削除すると、自動的に削除するサーバーに関連付けられているデータベースに注意してください。</span><span class="sxs-lookup"><span data-stu-id="59c36-343">Note that if you delete the database server, you automatically delete the databases associated with the server.</span></span>

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a><span data-ttu-id="59c36-344">Azure App Service Web Apps を Azure クラウド サービスの処理キューにアプリを展開する方法</span><span class="sxs-lookup"><span data-stu-id="59c36-344">How to deploy the app with queue processing to Azure App Service Web Apps and an Azure Cloud Service</span></span>

<span data-ttu-id="59c36-345">キューを有効にするには、MyFixIt\Web.config ファイルで、次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="59c36-345">To enable queues, make the following change in the MyFixIt\Web.config file.</span></span> <span data-ttu-id="59c36-346">`appSettings`の値を変更`UseQueues`"true"にします。</span><span class="sxs-lookup"><span data-stu-id="59c36-346">Under `appSettings`, change the value of `UseQueues` to "true":</span></span> 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

<span data-ttu-id="59c36-347">その後、MVC アプリケーション展開 Azure App service web アプリへの説明に従って[以前](#deploybase)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-347">Then deploy the MVC application to an web app in Azure App Service, as described [earlier](#deploybase).</span></span>

<span data-ttu-id="59c36-348">次に、新しい Azure クラウド サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="59c36-348">Next, create a new Azure cloud service.</span></span> <span data-ttu-id="59c36-349">修正がアプリに含まれているスクリプトを作成またはしないでこの Azure ポータルを使用する必要がありますので、クラウド サービスを展開します。</span><span class="sxs-lookup"><span data-stu-id="59c36-349">The scripts included with the Fix It app do not create or deploy the cloud service, so you must use Azure portal for this.</span></span> <span data-ttu-id="59c36-350">ポータルで、をクリックして**新規** -- **コンピューティング**–**クラウド サービス** -- **簡易作成**、URL を入力およびデータ センターの場所。</span><span class="sxs-lookup"><span data-stu-id="59c36-350">In the portal, click **New** -- **Compute** – **Cloud Service** -- **Quick Create**, and then enter a URL and a data center location.</span></span> <span data-ttu-id="59c36-351">Web アプリを展開して、同じデータ センターを使用します。</span><span class="sxs-lookup"><span data-stu-id="59c36-351">Use the same data center where you deployed the web app.</span></span>

![](the-fix-it-sample-application/_static/image1.png)

<span data-ttu-id="59c36-352">クラウド サービスを展開する前に、構成ファイルの一部を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="59c36-352">Before you can deploy the cloud service, you need to update some of the configuration files.</span></span>

<span data-ttu-id="59c36-353">MyFixIt.WorkerRoler\app.config の下にある`connectionStrings`の値を置き換える、 `appdb` SQL データベースの実際の接続文字列で接続文字列。</span><span class="sxs-lookup"><span data-stu-id="59c36-353">In MyFixIt.WorkerRoler\app.config, under `connectionStrings`, replace the value of the `appdb` connection string with the actual connection string for the SQL Database.</span></span> <span data-ttu-id="59c36-354">接続文字列は、ポータルから取得できます。</span><span class="sxs-lookup"><span data-stu-id="59c36-354">You can get the connection string from the portal.</span></span> <span data-ttu-id="59c36-355">ポータルで、をクリックして**SQL データベース** - **appdb** - **Ado.net、ODBC、PHP、および JDBC のビューの SQL データベース接続文字列**です。</span><span class="sxs-lookup"><span data-stu-id="59c36-355">In the portal, click **SQL Databases** - **appdb** - **View SQL Database connection strings for ADO .Net, ODBC, PHP, and JDBC**.</span></span> <span data-ttu-id="59c36-356">ADO.NET 接続文字列をコピーし、app.config ファイルに値を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="59c36-356">Copy the ADO.NET connection string and paste the value into the app.config file.</span></span> <span data-ttu-id="59c36-357">置換"{、\_パスワード\_ここ}"、データベース パスワードを使用します。</span><span class="sxs-lookup"><span data-stu-id="59c36-357">Replace "{your\_password\_here}" with your database password.</span></span> <span data-ttu-id="59c36-358">(内のデータベース パスワードを指定した MVC アプリの展開にスクリプトを使用したと仮定した場合、`SqlDatabasePassword`パラメーターのスクリプトを作成します)。</span><span class="sxs-lookup"><span data-stu-id="59c36-358">(Assuming you used the scripts to deploy the MVC app, you specified the database password in the `SqlDatabasePassword` script parameter.)</span></span>

<span data-ttu-id="59c36-359">結果は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="59c36-359">The result should look like the following:</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

<span data-ttu-id="59c36-360">同じ MyFixIt.WorkerRoler\app.config ファイルで下にある`appSettings`、Azure ストレージ アカウントの 2 つのプレース ホルダーの値を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="59c36-360">In the same MyFixIt.WorkerRoler\app.config file, under `appSettings`, replace the two placeholder values for the Azure storage account.</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

<span data-ttu-id="59c36-361">アクセス キーは、ポータルから取得できます。</span><span class="sxs-lookup"><span data-stu-id="59c36-361">You can get the access key from the portal.</span></span> <span data-ttu-id="59c36-362">参照してください[ストレージ アカウントを管理する方法](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-362">See [How To Manage Storage Accounts](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).</span></span>

<span data-ttu-id="59c36-363">MyFixItCloudService\ServiceConfiguration.Cloud.cscfg では、Azure ストレージ アカウントの同じ 2 つのプレース ホルダーの値を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="59c36-363">In MyFixItCloudService\ServiceConfiguration.Cloud.cscfg, replace the same two placeholders values for the Azure storage account.</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

<span data-ttu-id="59c36-364">クラウド サービスをデプロイする準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="59c36-364">Now you are ready to deploy the cloud service.</span></span> <span data-ttu-id="59c36-365">ソリューション エクスプ ローラーで MyFixItCloudService プロジェクトを右クリックし **発行**です。</span><span class="sxs-lookup"><span data-stu-id="59c36-365">In Solution Explore, right-click the MyFixItCloudService project and select **Publish**.</span></span> <span data-ttu-id="59c36-366">詳細については、次を参照してください。"[を Azure にアプリケーションを配置](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)"、2 部である[このチュートリアル](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36)です。</span><span class="sxs-lookup"><span data-stu-id="59c36-366">For more information, see "[Deploy the Application to Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", which is in part 2 of [this tutorial](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="59c36-367">前へ</span><span class="sxs-lookup"><span data-stu-id="59c36-367">Previous</span></span>](more-patterns-and-guidance.md)
