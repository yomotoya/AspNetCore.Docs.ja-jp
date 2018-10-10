---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: '付録: 修正プログラム、サンプル アプリケーション (Azure での実際のクラウド アプリの構築) |Microsoft Docs'
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: de3c8ea29f2c271136f58d8165bb92f4ab28ce83
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912827"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="2119b-104">付録: 修正プログラム、サンプル アプリケーション (Azure での実際のクラウド アプリの構築)</span><span class="sxs-lookup"><span data-stu-id="2119b-104">Appendix: The Fix It Sample Application (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="2119b-105">によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson]((https://twitter.com/RickAndMSFT))、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2119b-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="2119b-106">プロジェクトに修正プログラムをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="2119b-106">Download The Fix It Project</span></span>](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> <span data-ttu-id="2119b-107">**構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。</span><span class="sxs-lookup"><span data-stu-id="2119b-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="2119b-108">13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。</span><span class="sxs-lookup"><span data-stu-id="2119b-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="2119b-109">電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>

<span data-ttu-id="2119b-110">Azure 電子書籍で構築実世界クラウド アプリには、この付録には、Fix It サンプル アプリケーションをダウンロードすることに関する追加情報を提供する次のセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2119b-110">This appendix to the Building Real World Cloud Apps with Azure e-book contains the following sections that provide additional information about the Fix It sample application that you can download:</span></span>

- [<span data-ttu-id="2119b-111">既知の問題</span><span class="sxs-lookup"><span data-stu-id="2119b-111">Known issues</span></span>](#knownissues)
- [<span data-ttu-id="2119b-112">ベスト プラクティス</span><span class="sxs-lookup"><span data-stu-id="2119b-112">Best practices</span></span>](#bestpractices)
- [<span data-ttu-id="2119b-113">ローカル コンピューターに Visual Studio からアプリを実行する方法</span><span class="sxs-lookup"><span data-stu-id="2119b-113">How to run the app from Visual Studio on your local computer</span></span>](#run-in-vs)
- [<span data-ttu-id="2119b-114">Windows PowerShell スクリプトを使用して基本アプリを Azure App Service Web Apps にデプロイする方法</span><span class="sxs-lookup"><span data-stu-id="2119b-114">How to deploy the base app to Azure App Service Web Apps by using the Windows PowerShell scripts</span></span>](#deploybase)
- [<span data-ttu-id="2119b-115">Windows PowerShell スクリプトのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="2119b-115">Troubleshooting the Windows PowerShell scripts</span></span>](#troubleshooting)
- [<span data-ttu-id="2119b-116">Azure App Service Web Apps と Azure クラウド サービスを処理キューに、アプリをデプロイする方法</span><span class="sxs-lookup"><span data-stu-id="2119b-116">How to deploy the app with queue processing to Azure App Service Web Apps and an Azure Cloud Service</span></span>](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a><span data-ttu-id="2119b-117">既知の問題</span><span class="sxs-lookup"><span data-stu-id="2119b-117">Known issues</span></span>

<span data-ttu-id="2119b-118">Fix It アプリは、もともと限りシンプルにこの電子書籍を示したパターンの一部を説明するために開発されました。</span><span class="sxs-lookup"><span data-stu-id="2119b-118">The Fix It app was originally developed in order to illustrate as simply as possible some of the patterns presented in this e-book.</span></span> <span data-ttu-id="2119b-119">ただし、電子書籍では、実際のアプリの構築方法であるため対象となる Fix It コード レビューとテスト プロセスにリリースされたソフトウェアの作業と似ています。</span><span class="sxs-lookup"><span data-stu-id="2119b-119">However, since the e-book is about building real-world apps, we subjected the Fix It code to a review and testing process similar to what we'd do for released software.</span></span> <span data-ttu-id="2119b-120">数多くの問題が見つかったと、実際のアプリケーションと同様にその一部を修正しましたされ、今後のリリースにそれらのいくつかの遅延。</span><span class="sxs-lookup"><span data-stu-id="2119b-120">We found a number of issues, and as with any real-world application, some of them we fixed and some of them we deferred to a later release.</span></span>

<span data-ttu-id="2119b-121">次の一覧には、実稼働アプリケーションではアドレス Fix It サンプル アプリケーションの初期リリースでないことにしました別の理由の 1 つの問題に対処する必要がありますが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2119b-121">The following list includes issues that should be addressed in a production application, but for one reason or another we decided not to address in the initial release of the Fix It sample application.</span></span>

### <a name="security"></a><span data-ttu-id="2119b-122">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="2119b-122">Security</span></span>

- <span data-ttu-id="2119b-123">存在しない所有者にタスクを割り当てることはできませんを確認します。</span><span class="sxs-lookup"><span data-stu-id="2119b-123">Ensure that you can't assign a task to a non-existent owner.</span></span>
- <span data-ttu-id="2119b-124">しかことを確認します。 表示および作成するか、自分に割り当てられたタスクを変更します。</span><span class="sxs-lookup"><span data-stu-id="2119b-124">Ensure that you can only view and modify tasks that you created or are assigned to you.</span></span>
- <span data-ttu-id="2119b-125">サインイン ページと認証 cookie を HTTPS を使用します。</span><span class="sxs-lookup"><span data-stu-id="2119b-125">Use HTTPS for sign-in pages and authentication cookies.</span></span>
- <span data-ttu-id="2119b-126">認証クッキーの期限を指定します。</span><span class="sxs-lookup"><span data-stu-id="2119b-126">Specify a time limit for authentication cookies.</span></span>

### <a name="input-validation"></a><span data-ttu-id="2119b-127">入力の検証</span><span class="sxs-lookup"><span data-stu-id="2119b-127">Input validation</span></span>

<span data-ttu-id="2119b-128">一般に、運用環境のアプリが行う Fix It アプリよりも多くの入力の検証。</span><span class="sxs-lookup"><span data-stu-id="2119b-128">In general, a production app would do more input validation than the Fix It app.</span></span> <span data-ttu-id="2119b-129">たとえば、イメージのサイズ]、[画像のアップロードを制限するか許可されているファイルのサイズ。</span><span class="sxs-lookup"><span data-stu-id="2119b-129">For example, the image size / image file size allowed for upload should be limited.</span></span>

### <a name="administrator-functionality"></a><span data-ttu-id="2119b-130">管理者の機能</span><span class="sxs-lookup"><span data-stu-id="2119b-130">Administrator functionality</span></span>

<span data-ttu-id="2119b-131">管理者は、既存のタスクでの所有権を変更できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-131">An administrator should be able to change ownership on existing tasks.</span></span> <span data-ttu-id="2119b-132">たとえば、タスクの作成者は、機関の管理アクセスが有効にしない限り、タスクを維持するために、だれを終了、会社を退職可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-132">For example, the creator of a task might leave the company, leaving no one with authority to maintain the task unless administrative access is enabled.</span></span>

### <a name="queue-message-processing"></a><span data-ttu-id="2119b-133">キュー メッセージの処理</span><span class="sxs-lookup"><span data-stu-id="2119b-133">Queue message processing</span></span>

<span data-ttu-id="2119b-134">Fix It アプリで処理するキュー メッセージは、最小限のコードでキューを中心とした作業パターンを説明するために簡単に設計されています。</span><span class="sxs-lookup"><span data-stu-id="2119b-134">Queue message processing in the Fix It app was designed to be simple in order to illustrate the queue-centric work pattern with a minimum amount of code.</span></span> <span data-ttu-id="2119b-135">この単純なコードは、実際の運用アプリケーションのための適切なできません。</span><span class="sxs-lookup"><span data-stu-id="2119b-135">This simple code would not be adequate for an actual production application.</span></span>

- <span data-ttu-id="2119b-136">コードでは、各キュー メッセージが最大で 1 回処理されることは保証されません。</span><span class="sxs-lookup"><span data-stu-id="2119b-136">The code does not guarantee that each queue message will be processed at most once.</span></span> <span data-ttu-id="2119b-137">キューからメッセージを取得するときに、メッセージが他のキュー リスナーに表示されていない、タイムアウト期間があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-137">When you get a message from the queue, there is a timeout period, during which the message is invisible to other queue listeners.</span></span> <span data-ttu-id="2119b-138">もう一度メッセージが削除される前にタイムアウトになるには、メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2119b-138">If the timeout expires before the message is deleted, the message becomes visible again.</span></span> <span data-ttu-id="2119b-139">そのため、ワーカー ロール インスタンスは、メッセージの処理時間を費やして、ある理論的に、同じメッセージを 2 回、処理可能、データベース内の重複するタスクの結果します。</span><span class="sxs-lookup"><span data-stu-id="2119b-139">Therefore, if a worker role instance spends a long time processing a message, it is theoretically possible for the same message to get processed twice, resulting in a duplicate task in the database.</span></span> <span data-ttu-id="2119b-140">この問題の詳細については、次を参照してください。[を使用して Azure Storage キュー](https://msdn.microsoft.com/library/ff803365.aspx#sec7)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-140">For more information about this issue, see [Using Azure Storage Queues](https://msdn.microsoft.com/library/ff803365.aspx#sec7).</span></span>
- <span data-ttu-id="2119b-141">キューのポーリングのロジックは、メッセージの取得をバッチ処理によってよりコスト効果の高い可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-141">The queue polling logic could be more cost-effective, by batching message retrieval.</span></span> <span data-ttu-id="2119b-142">呼び出すたびに[CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx)トランザクションのコストがあります。</span><span class="sxs-lookup"><span data-stu-id="2119b-142">Every time you call [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), there is a transaction cost.</span></span> <span data-ttu-id="2119b-143">代わりに、呼び出すことができます[CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (、複数形に注意してください ')、単一のトランザクションで複数のメッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="2119b-143">Instead, you can call [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (note the plural 's'), which gets multiple messages in a single transaction.</span></span> <span data-ttu-id="2119b-144">Azure Storage キューのトランザクション コストは、ため、ほとんどのシナリオで大幅なコストへの影響は非常に低いです。</span><span class="sxs-lookup"><span data-stu-id="2119b-144">The transaction costs for Azure Storage Queues are very low, so the impact on costs is not substantial in most scenarios.</span></span>
- <span data-ttu-id="2119b-145">キュー メッセージの処理コードの緊密なループにより、CPU アフィニティは、複数のコアの Vm を効率的に使用されません。</span><span class="sxs-lookup"><span data-stu-id="2119b-145">The tight loop in the queue message-processing code causes CPU affinity, which does not utilize multi-core VMs efficiently.</span></span> <span data-ttu-id="2119b-146">優れた設計は、いくつかの非同期タスクの並列実行するのにタスクの並列化を使用します。</span><span class="sxs-lookup"><span data-stu-id="2119b-146">A better design would use task parallelism to run several async tasks in parallel.</span></span>
- <span data-ttu-id="2119b-147">キューのメッセージ処理では、のみ初歩的な例外処理があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-147">Queue message-processing has only rudimentary exception handling.</span></span> <span data-ttu-id="2119b-148">たとえば、コードを処理しない[有害メッセージ](https://msdn.microsoft.com/library/ms789028.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-148">For example, the code doesn't handle [poison messages](https://msdn.microsoft.com/library/ms789028.aspx).</span></span> <span data-ttu-id="2119b-149">(メッセージの処理には、例外が発生、エラーを記録し、メッセージを削除する必要があるまたはワーカー ロールは、もう一度処理しようし、ループが無期限に続行されます。)</span><span class="sxs-lookup"><span data-stu-id="2119b-149">(When message processing causes an exception, you have to log the error and delete the message, or the worker role will try to process it again, and the loop will continue indefinitely.)</span></span>

### <a name="sql-queries-are-unbounded"></a><span data-ttu-id="2119b-150">SQL クエリの制限がないです。</span><span class="sxs-lookup"><span data-stu-id="2119b-150">SQL queries are unbounded</span></span>

<span data-ttu-id="2119b-151">現在の修正、コードは、インデックス ページのクエリが返す可能性があります行の数に制限を配置しません。</span><span class="sxs-lookup"><span data-stu-id="2119b-151">Current Fix It code places no limit on how many rows the queries for Index pages might return.</span></span> <span data-ttu-id="2119b-152">膨大な量のタスクがデータベースに入力された場合、受信した結果のリストのサイズにはパフォーマンスの問題可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-152">If a large volume of tasks is entered into the database, the size of the resulting lists received could cause performance issues.</span></span> <span data-ttu-id="2119b-153">ソリューションでは、ページングを実装します。</span><span class="sxs-lookup"><span data-stu-id="2119b-153">The solution is to implement paging.</span></span> <span data-ttu-id="2119b-154">例については、次を参照してください。[並べ替え、フィルター処理、および ASP.NET MVC アプリケーションで Entity Framework でページング](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-154">For an example, see [Sorting, Filtering, and Paging with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).</span></span>

### <a name="view-models-recommended"></a><span data-ttu-id="2119b-155">推奨モデルの表示</span><span class="sxs-lookup"><span data-stu-id="2119b-155">View models recommended</span></span>

<span data-ttu-id="2119b-156">Fix It アプリでは、FixItTask エンティティ クラスを使用して、コント ローラーとビューの間で情報を渡します。</span><span class="sxs-lookup"><span data-stu-id="2119b-156">The Fix It app uses the FixItTask entity class to pass information between the controller and the view.</span></span> <span data-ttu-id="2119b-157">ビュー モデルを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2119b-157">A best practice is to use view models.</span></span> <span data-ttu-id="2119b-158">ドメイン モデル (FixItTask エンティティ クラスなど) は、データの表示、ビュー モデルをデザインするときにデータ永続化のために必要なものに基づいて設計されています。</span><span class="sxs-lookup"><span data-stu-id="2119b-158">The domain model (e.g., the FixItTask entity class) is designed around what is needed for data persistence, while a view model can be designed for data presentation.</span></span> <span data-ttu-id="2119b-159">詳細については、次を参照してください。 [12 の ASP.NET MVC のベスト プラクティス](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-159">For more information, see [12 ASP.NET MVC Best Practices](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).</span></span>

### <a name="secure-image-blob-recommended"></a><span data-ttu-id="2119b-160">セキュリティで保護されたイメージの blob がお勧めします</span><span class="sxs-lookup"><span data-stu-id="2119b-160">Secure image blob recommended</span></span>

<span data-ttu-id="2119b-161">つまりイメージの URL を見つけた他のユーザー アクセスできる、パブリックとしてイメージを Fix It アプリ ストアにアップロードされます。</span><span class="sxs-lookup"><span data-stu-id="2119b-161">The Fix It app stores uploaded images as public, meaning that anyone who finds the URL can access the images.</span></span> <span data-ttu-id="2119b-162">パブリックではなく、イメージを保護する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-162">The images could be secured instead of public.</span></span>

### <a name="no-powershell-automation-scripts-for-queues"></a><span data-ttu-id="2119b-163">キューの PowerShell オートメーション スクリプトはありません。</span><span class="sxs-lookup"><span data-stu-id="2119b-163">No PowerShell automation scripts for queues</span></span>

<span data-ttu-id="2119b-164">オートメーションのサンプル PowerShell スクリプトは、のみの修正が完全に Azure App Service Web Apps で実行される基本バージョン用に作成されました。</span><span class="sxs-lookup"><span data-stu-id="2119b-164">Sample PowerShell automation scripts were written only for the base version of Fix It that runs entirely in Azure App Service Web Apps.</span></span> <span data-ttu-id="2119b-165">設定して、web アプリとキューの処理に必要なクラウド サービス環境に展開するためスクリプトを指定していません。</span><span class="sxs-lookup"><span data-stu-id="2119b-165">We haven't provided scripts for setting up and deploying to the web app plus Cloud Service environment required for queue processing.</span></span>

### <a name="special-handling-for-html-codes-in-user-input"></a><span data-ttu-id="2119b-166">ユーザー入力に HTML コードの特別な処理</span><span class="sxs-lookup"><span data-stu-id="2119b-166">Special handling for HTML codes in user input</span></span>

<span data-ttu-id="2119b-167">ASP.NET には、さまざまな方法が悪意のあるユーザーがユーザー入力テキスト ボックスにスクリプトを入力してクロス サイト スクリプティング攻撃を試みることもありますが自動的にできないようにします。</span><span class="sxs-lookup"><span data-stu-id="2119b-167">ASP.NET automatically prevents many ways in which malicious users might attempt cross-site scripting attacks by entering script in user input text boxes.</span></span> <span data-ttu-id="2119b-168">MVC`DisplayFor`タスクを表示するために使用するヘルパーがタイトルし、自動的にブラウザーに送信される HTML エンコードの値をメモします。</span><span class="sxs-lookup"><span data-stu-id="2119b-168">And the MVC `DisplayFor` helper used to display task titles and notes automatically HTML-encodes values that it sends to the browser.</span></span> <span data-ttu-id="2119b-169">ただし、運用アプリでその他の対策をする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-169">But in a production app you might want to take additional measures.</span></span> <span data-ttu-id="2119b-170">詳細については、次を参照してください。 [asp.net 要求の検証](https://msdn.microsoft.com/library/hh882339.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="2119b-170">For more information, see [Request Validation in ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).</span></span>

<a id="bestpractices"></a>
## <a name="best-practices"></a><span data-ttu-id="2119b-171">ベスト プラクティス</span><span class="sxs-lookup"><span data-stu-id="2119b-171">Best practices</span></span>

<span data-ttu-id="2119b-172">コード レビューで発見し、修正、アプリの元のバージョンのテストの後に修正されているいくつかの問題を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2119b-172">Following are some issues that were fixed after being discovered in code review and testing of the original version of the Fix It app.</span></span> <span data-ttu-id="2119b-173">いない認識すること、特定のベスト プラクティスのいくつか単純に元のプログラマによっていくつか発生したため、コードが簡単に作成されており、リリースされたソフトウェアの意図しません。</span><span class="sxs-lookup"><span data-stu-id="2119b-173">Some were caused by the original coder not being aware of a particular best practice, some simply because the code was written quickly and wasn't intended for released software.</span></span> <span data-ttu-id="2119b-174">このレビューから学んだものを使用する必要がある場合、ここでの問題を一覧表示しているし、他の web アプリの開発も、ユーザーに役立つテストにあります。</span><span class="sxs-lookup"><span data-stu-id="2119b-174">We're listing the issues here in case there's something we learned from this review and testing that might be helpful to others who are also developing web apps.</span></span>

### <a name="dispose-the-database-repository"></a><span data-ttu-id="2119b-175">データベースのリポジトリを破棄します。</span><span class="sxs-lookup"><span data-stu-id="2119b-175">Dispose the database repository</span></span>

<span data-ttu-id="2119b-176">`FixItTaskRepository`クラスは、Entity Framework を破棄する必要があります`DbContext`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="2119b-176">The `FixItTaskRepository` class must dispose the Entity Framework `DbContext` instance.</span></span> <span data-ttu-id="2119b-177">実装することで、これを行った`IDisposable`で、`FixItTaskRepository`クラス。</span><span class="sxs-lookup"><span data-stu-id="2119b-177">We did this by implementing `IDisposable` in the `FixItTaskRepository` class:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

<span data-ttu-id="2119b-178">AutoFac を自動的に破棄するメモ、`FixItTaskRepository`のインスタンスのため、明示的に破棄する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2119b-178">Note that AutoFac will automatically dispose the `FixItTaskRepository` instance, so we don't need to explicitly dispose it.</span></span>

<span data-ttu-id="2119b-179">削除することも、`DbContext`からメンバー変数`FixItTaskRepository`、代わりにローカルに作成および`DbContext`各リポジトリ メソッド内で変数内で、`using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="2119b-179">Another option is to remove the `DbContext` member variable from `FixItTaskRepository`, and instead create a local `DbContext` variable within each repository method, inside a `using` statement.</span></span> <span data-ttu-id="2119b-180">例えば:</span><span class="sxs-lookup"><span data-stu-id="2119b-180">For example:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a><span data-ttu-id="2119b-181">シングルトンに DI よう登録します。</span><span class="sxs-lookup"><span data-stu-id="2119b-181">Register singletons as such with DI</span></span>

<span data-ttu-id="2119b-182">以降のインスタンスを 1 つだけ、`PhotoService`クラスと`Logger`クラスが必要なこれらのクラスを指定する必要があります[依存関係の挿入の 1 つのインスタンスとして登録されている](https://code.google.com/p/autofac/wiki/InstanceScope)で*DependenciesConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="2119b-182">Since only one instance of the `PhotoService` class and `Logger` class is needed, these classes should be [registered as single instances for dependency injection](https://code.google.com/p/autofac/wiki/InstanceScope) in *DependenciesConfig.cs*:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a><span data-ttu-id="2119b-183">セキュリティ: ユーザーにエラーの詳細を表示しません。</span><span class="sxs-lookup"><span data-stu-id="2119b-183">Security: Don't show error details to users</span></span>

<span data-ttu-id="2119b-184">アプリ Fix It 元は、一般的なエラー ページがあるし、データベース接続エラーなどのいくつかの例外は、ブラウザーに表示されている完全なスタック トレースになる可能性がありますので、UI までのすべての例外バブルに任せてしませんでした。</span><span class="sxs-lookup"><span data-stu-id="2119b-184">The original Fix It app didn't have a generic error page and just let all exceptions bubble up to the UI, so some exceptions such as database connection errors could result in a full stack trace being displayed to the browser.</span></span> <span data-ttu-id="2119b-185">エラーの詳細については、悪意のあるユーザーからの攻撃を促進できる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-185">Detailed error information can sometimes facilitate attacks by malicious users.</span></span> <span data-ttu-id="2119b-186">ソリューションでは、例外の詳細を記録し、エラーの詳細が含まれていないユーザーにエラー ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="2119b-186">The solution is to log the exception details and display an error page to the user that doesn't include error details.</span></span> <span data-ttu-id="2119b-187">Fix It アプリが既にログ記録、およびエラー ページを表示するために追加しました`<customErrors mode=On>`Web.config ファイルにします。</span><span class="sxs-lookup"><span data-stu-id="2119b-187">The Fix It app was already logging, and in order to display an error page, we added `<customErrors mode=On>` in the Web.config file.</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

<span data-ttu-id="2119b-188">これにより、既定で*Views\Shared\Error.cshtml*エラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="2119b-188">By default this causes *Views\Shared\Error.cshtml* to be displayed for errors.</span></span> <span data-ttu-id="2119b-189">カスタマイズできる*Error.cshtml*または独自のエラー ページのビューを作成し、追加、`defaultRedirect`属性。</span><span class="sxs-lookup"><span data-stu-id="2119b-189">You can customize *Error.cshtml* or create your own error page view and add a `defaultRedirect` attribute.</span></span> <span data-ttu-id="2119b-190">特定のエラーの別のエラー ページを指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="2119b-190">You can also specify different error pages for specific errors.</span></span>

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a><span data-ttu-id="2119b-191">セキュリティ: は、作成者によって編集タスクのみを許可します。</span><span class="sxs-lookup"><span data-stu-id="2119b-191">Security: only allow a task to be edited by its creator</span></span>

<span data-ttu-id="2119b-192">ダッシュ ボードのインデックス ページは、ログオンしているユーザーによって作成されたタスクだけを表示しますが、悪意のあるユーザーは別のユーザーのタスクに ID を持つ URL を作成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-192">The Dashboard Index page only shows tasks created by the logged-on user, but a malicious user could create a URL with an ID to another user's task.</span></span> <span data-ttu-id="2119b-193">内のコードを追加しました*DashboardController.cs*をその場合は、404 を返します。</span><span class="sxs-lookup"><span data-stu-id="2119b-193">We added code in *DashboardController.cs* to return a 404 in that case:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a><span data-ttu-id="2119b-194">例外を飲み込むはありません。</span><span class="sxs-lookup"><span data-stu-id="2119b-194">Don't swallow exceptions</span></span>

<span data-ttu-id="2119b-195">アプリ Fix It 元だけが null を返しましたログインすると、SQL クエリの結果として生じた例外。</span><span class="sxs-lookup"><span data-stu-id="2119b-195">The original Fix It app just returned null after logging an exception that resulted from a SQL query:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

<span data-ttu-id="2119b-196">クエリが成功しましたが、同じ行が返されませんでしたかのように、ユーザーに外観になります。</span><span class="sxs-lookup"><span data-stu-id="2119b-196">This would make it look to the user as if the query succeeded but just didn't return any rows.</span></span> <span data-ttu-id="2119b-197">ソリューションのキャッチとログ記録の後に例外を再スローすることです。</span><span class="sxs-lookup"><span data-stu-id="2119b-197">Solution is to re-throw the exception after catching and logging:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a><span data-ttu-id="2119b-198">ワーカー ロールのすべての例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="2119b-198">Catch all exceptions in worker roles</span></span>

<span data-ttu-id="2119b-199">VM を再利用すると、ワーカー ロールでハンドルされない例外、try-catch ブロックで実行し、すべての例外を処理するすべてをラップするためです。</span><span class="sxs-lookup"><span data-stu-id="2119b-199">Any unhandled exceptions in a worker role will cause the VM to be recycled, so you want to wrap everything you do in a try-catch block and handle all exceptions.</span></span>

### <a name="specify-length-for-string-properties-in-entity-classes"></a><span data-ttu-id="2119b-200">エンティティ クラスのプロパティの文字列長を指定します。</span><span class="sxs-lookup"><span data-stu-id="2119b-200">Specify length for string properties in entity classes</span></span>

<span data-ttu-id="2119b-201">Fix It アプリの元のバージョンの単純なコードを表示するために、FixItTask エンティティのフィールドの長さを指定していないし、その結果、データベースでは、varchar (max) として定義されました。</span><span class="sxs-lookup"><span data-stu-id="2119b-201">In order to display simple code, the original version of the Fix It app didn't specify lengths for the fields of the FixItTask entity, and as a result they were defined as varchar(max) in the database.</span></span> <span data-ttu-id="2119b-202">その結果、UI がほぼすべての量の入力、受け入れていました。</span><span class="sxs-lookup"><span data-stu-id="2119b-202">As a result, the UI would accept almost any amount of input.</span></span> <span data-ttu-id="2119b-203">Web ページと、データベース内の列のサイズで両方のユーザーに適用される指定の長さセット制限を入力します。</span><span class="sxs-lookup"><span data-stu-id="2119b-203">Specifying lengths sets limits that apply both to user input in the web page and column size in the database:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a><span data-ttu-id="2119b-204">変更することが予期されないときに、プライベート メンバーを readonly としてマークします。</span><span class="sxs-lookup"><span data-stu-id="2119b-204">Mark private members as readonly when they aren't expected to change</span></span>

<span data-ttu-id="2119b-205">たとえば、`DashboardController`クラスのインスタンス`FixItTaskRepository`が作成され、変更するには想定されていませんとして定義したので[readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-205">For example, in the `DashboardController` class an instance of `FixItTaskRepository` is created and isn't expected to change, so we defined it as [readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx).</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a><span data-ttu-id="2119b-206">リストを使用します。一覧ではなく Any() します。Count() &gt; 0</span><span class="sxs-lookup"><span data-stu-id="2119b-206">Use list.Any() instead of list.Count() &gt; 0</span></span>

<span data-ttu-id="2119b-207">リスト内の 1 つまたは複数の項目が指定した条件に一致するかどうかが重要する場合を使用して、[任意](https://msdn.microsoft.com/library/bb534972.aspx)メソッドを返すため、条件に適合する項目が見つかるとすぐに対し、`Count`メソッドは常に反復処理するにはすべてのアイテム。</span><span class="sxs-lookup"><span data-stu-id="2119b-207">If you all you care about is whether one or more items in a list fit the specified criteria, use the [Any](https://msdn.microsoft.com/library/bb534972.aspx) method, because it returns as soon as an item fitting the criteria is found, whereas the `Count` method always has to iterate through every item.</span></span> <span data-ttu-id="2119b-208">ダッシュ ボード*Index.cshtml*ファイルは、このコードを最初から。</span><span class="sxs-lookup"><span data-stu-id="2119b-208">The Dashboard *Index.cshtml* file originally had this code:</span></span>

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

<span data-ttu-id="2119b-209">これを変更しました。</span><span class="sxs-lookup"><span data-stu-id="2119b-209">We changed it to this:</span></span>

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a><span data-ttu-id="2119b-210">MVC ヘルパーを使用して MVC ビューで Url を生成します。</span><span class="sxs-lookup"><span data-stu-id="2119b-210">Generate URLs in MVC views using MVC helpers</span></span>

<span data-ttu-id="2119b-211">**作成修正**Fix It アプリ ハード ホーム ページで、ボタンは、アンカー要素をコード化されました。</span><span class="sxs-lookup"><span data-stu-id="2119b-211">For the **Create a Fix It** button on the home page, the Fix It app hard coded an anchor element:</span></span>

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

<span data-ttu-id="2119b-212">このようなビュー/アクション リンクは、使用する方がよい、 [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx)例については、HTML ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="2119b-212">For View/Action links like this it's better to use the [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) HTML helper, for example:</span></span>

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a><span data-ttu-id="2119b-213">Thread.Sleep ではなく Task.Delay を使用して、ワーカー ロール</span><span class="sxs-lookup"><span data-stu-id="2119b-213">Use Task.Delay instead of Thread.Sleep in worker role</span></span>

<span data-ttu-id="2119b-214">新しいプロジェクト テンプレートは、`Thread.Sleep`サンプルで worker ロールでは、スレッドをスリープ状態の原因はコード不要な追加のスレッドを生成するスレッド プールが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="2119b-214">The new-project template puts `Thread.Sleep` in the sample code for a worker role, but causing the thread to sleep can cause the thread pool to spawn additional unnecessary threads.</span></span> <span data-ttu-id="2119b-215">使用して回避することができます[Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx)代わりにします。</span><span class="sxs-lookup"><span data-stu-id="2119b-215">You can avoid that by using [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) instead.</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a><span data-ttu-id="2119b-216">Async void を避ける</span><span class="sxs-lookup"><span data-stu-id="2119b-216">Avoid async void</span></span>

<span data-ttu-id="2119b-217">非同期メソッドが値を返す必要がある場合を返す、`Task`型なく`void`します。</span><span class="sxs-lookup"><span data-stu-id="2119b-217">If an async method doesn't need to return a value, return a `Task` type rather than `void`.</span></span>

<span data-ttu-id="2119b-218">この例は、`FixItQueueManager`クラス。</span><span class="sxs-lookup"><span data-stu-id="2119b-218">This example is from the `FixItQueueManager` class:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

<span data-ttu-id="2119b-219">使用する必要があります`async void`最上位レベルのイベント ハンドラーに対してのみです。</span><span class="sxs-lookup"><span data-stu-id="2119b-219">You should use `async void` only for top-level event handlers.</span></span> <span data-ttu-id="2119b-220">としてメソッドを定義する場合`async void`、呼び出し元ができない**await**メソッドまたはメソッドをスローするすべての例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="2119b-220">If you define a method as `async void`, the caller cannot **await** the method or catch any exceptions the method throws.</span></span> <span data-ttu-id="2119b-221">詳細については、次を参照してください。[非同期プログラミングのベスト プラクティス](https://msdn.microsoft.com/magazine/jj991977.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-221">For more information, see [Best Practices in Asynchronous Programming](https://msdn.microsoft.com/magazine/jj991977.aspx).</span></span>

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a><span data-ttu-id="2119b-222">キャンセル トークンを使用して、ワーカー ロールのループを中断するには</span><span class="sxs-lookup"><span data-stu-id="2119b-222">Use a cancellation token to break from worker role loop</span></span>

<span data-ttu-id="2119b-223">通常、**実行**をワーカー ロールでのメソッドには、無限ループが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2119b-223">Typically, the **Run** method on a worker role contains an infinite loop.</span></span> <span data-ttu-id="2119b-224">Worker ロールが停止しているときに、 [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx)メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="2119b-224">When the worker role is stopping, the [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) method is called.</span></span> <span data-ttu-id="2119b-225">このメソッドは、内部で行われている処理の取り消しを使用する必要があります、**実行**メソッドと終了適切にします。</span><span class="sxs-lookup"><span data-stu-id="2119b-225">You should use this method to cancel the work that is being done inside the **Run** method and exit gracefully.</span></span> <span data-ttu-id="2119b-226">それ以外の場合、操作の途中でプロセスを終了する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-226">Otherwise, the process might be terminated in the middle of an operation.</span></span>

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a><span data-ttu-id="2119b-227">自動 MIME スニッフィングのプロシージャをオプトアウトします。</span><span class="sxs-lookup"><span data-stu-id="2119b-227">Opt out of Automatic MIME Sniffing Procedure</span></span>

<span data-ttu-id="2119b-228">場合によっては、Internet Explorer は web サーバーで指定された型よりもさまざまな MIME の種類を報告します。</span><span class="sxs-lookup"><span data-stu-id="2119b-228">In some cases, Internet Explorer reports a MIME type different than the type specified by the web server.</span></span> <span data-ttu-id="2119b-229">たとえば、HTTP 応答ヘッダーのコンテンツ タイプで提供されるファイルで Internet Explorer がコンテンツにある HTML を見つけた場合: テキスト/プレーン コンテンツを HTML として表示すること、Internet Explorer が決定します。</span><span class="sxs-lookup"><span data-stu-id="2119b-229">For instance, if Internet Explorer finds HTML content in a file delivered with the HTTP response header Content-Type: text/plain, Internet Explorer determines that the content should be rendered as HTML.</span></span> <span data-ttu-id="2119b-230">残念ながら、この「MIME スニッフィング」可能性も信頼されていないコンテンツをホストするサーバーのセキュリティの問題にします。</span><span class="sxs-lookup"><span data-stu-id="2119b-230">Unfortunately, this "MIME-sniffing" can also lead to security problems for servers hosting untrusted content.</span></span> <span data-ttu-id="2119b-231">Internet Explorer 8 に、この問題を解決するには、MIME の種類の決定コードへの変更の数が確立およびにより、アプリケーション開発者[MIME スニッフィングのオプトアウト](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-231">To combat this problem, Internet Explorer 8 has made a number of changes to MIME-type determination code and allows application developers to [opt out of MIME-sniffing](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx).</span></span> <span data-ttu-id="2119b-232">次のコードに追加された、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2119b-232">The following code was added to the *Web.config* file.</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a><span data-ttu-id="2119b-233">バンドルと縮小が有効にします。</span><span class="sxs-lookup"><span data-stu-id="2119b-233">Enable bundling and minification</span></span>

<span data-ttu-id="2119b-234">Visual Studio では、新しい web プロジェクトを作成するときの JavaScript ファイルのバンドルと縮小は既定で有効ありません。</span><span class="sxs-lookup"><span data-stu-id="2119b-234">When Visual Studio creates a new web project, bundling and minification of JavaScript files is not enabled by default.</span></span> <span data-ttu-id="2119b-235">BundleConfig.cs でのコード行を追加しました。</span><span class="sxs-lookup"><span data-stu-id="2119b-235">We added a line of code in BundleConfig.cs:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a><span data-ttu-id="2119b-236">認証クッキーの有効期限のタイムアウトを設定します。</span><span class="sxs-lookup"><span data-stu-id="2119b-236">Set an expiration time-out for authentication cookies</span></span>

<span data-ttu-id="2119b-237">既定では、認証 cookie は、2 週間以内に期限切れ。</span><span class="sxs-lookup"><span data-stu-id="2119b-237">By default, authentication cookies expire in two weeks.</span></span> <span data-ttu-id="2119b-238">短い時間の方が安全です。</span><span class="sxs-lookup"><span data-stu-id="2119b-238">A shorter time is more secure.</span></span> <span data-ttu-id="2119b-239">この設定を変更する*StartupAuth.cs*:</span><span class="sxs-lookup"><span data-stu-id="2119b-239">You can change this setting in *StartupAuth.cs*:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a><span data-ttu-id="2119b-240">ローカル コンピューターに Visual Studio からアプリを実行する方法</span><span class="sxs-lookup"><span data-stu-id="2119b-240">How to run the app from Visual Studio on your local computer</span></span>

<span data-ttu-id="2119b-241">Fix It アプリを実行する 2 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="2119b-241">There are two ways to run the Fix It app:</span></span>

- <span data-ttu-id="2119b-242">新しいタスクを SQL database に直接書き込むベースのアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="2119b-242">Run the base application that writes new tasks directly to the SQL database.</span></span>
- <span data-ttu-id="2119b-243">キューに加えて、バックエンド サービスを使用してタスクを作成するアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="2119b-243">Run the application using a queue plus a backend service to create tasks.</span></span> <span data-ttu-id="2119b-244">キュー パターンが、」の章で説明されている[キューを中心とした作業パターン](queue-centric-work-pattern.md)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-244">The queue pattern is described in the chapter [Queue-Centric Work Pattern](queue-centric-work-pattern.md).</span></span>

<a id="runbase"></a>
### <a name="run-the-base-application"></a><span data-ttu-id="2119b-245">ベースのアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="2119b-245">Run the base application</span></span>

1. <span data-ttu-id="2119b-246">インストール[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-246">Install [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span>
2. <span data-ttu-id="2119b-247">インストール、 [Azure SDK for .NET for Visual Studio](https://azure.microsoft.com/downloads/)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-247">Install the [Azure SDK for .NET for Visual Studio](https://azure.microsoft.com/downloads/).</span></span>
3. <span data-ttu-id="2119b-248">.Zip ファイルをダウンロード、 [MSDN コード ギャラリー](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-248">Download the .zip file from the [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).</span></span>
4. <span data-ttu-id="2119b-249">ファイル エクスプ ローラーで .zip ファイルを右クリックして、プロパティ をクリックし、プロパティ ウィンドウで ブロック解除 をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2119b-249">In File Explorer, right-click the .zip file and click Properties, then in the Properties window click Unblock.</span></span>
5. <span data-ttu-id="2119b-250">ファイルを解凍します。</span><span class="sxs-lookup"><span data-stu-id="2119b-250">Unzip the file.</span></span>
6. <span data-ttu-id="2119b-251">Visual Studio を起動する .sln ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="2119b-251">Double-click the .sln file to launch Visual Studio.</span></span>
7. <span data-ttu-id="2119b-252">**ツール** メニューのをクリックして**NuGet パッケージ マネージャー**、し**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="2119b-252">From the **Tools** menu, click **NuGet Package Manager**, then **Package Manager Console**.</span></span>
8. <span data-ttu-id="2119b-253">パッケージ マネージャー コンソール (PMC) では、復元をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2119b-253">In the Package Manager Console (PMC), click Restore.</span></span>
9. <span data-ttu-id="2119b-254">Visual Studio を終了します。</span><span class="sxs-lookup"><span data-stu-id="2119b-254">Exit Visual Studio.</span></span>
10. <span data-ttu-id="2119b-255">開始、 [Azure ストレージ エミュレーター](/azure/storage/common/storage-use-emulator)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-255">Start the [Azure storage emulator](/azure/storage/common/storage-use-emulator).</span></span>
11. <span data-ttu-id="2119b-256">前の手順で終了するソリューション ファイルを開き、Visual Studio を再起動します。</span><span class="sxs-lookup"><span data-stu-id="2119b-256">Restart Visual Studio, opening the solution file you closed in the previous step.</span></span>
12. <span data-ttu-id="2119b-257">FixIt プロジェクトがスタートアップ プロジェクトとして設定されていることを確認し、プロジェクトを実行するには、CTRL + F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2119b-257">Make sure the FixIt project is set as the startup project, and then press CTRL+F5 to run the project.</span></span>

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a><span data-ttu-id="2119b-258">キューの処理でアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="2119b-258">Run the application with queue processing</span></span>

1. <span data-ttu-id="2119b-259">指示に従い[ベースのアプリケーションを実行](#runbase)ブラウザーを終了して、Visual Studio を閉じます。</span><span class="sxs-lookup"><span data-stu-id="2119b-259">Follow the directions for [Run the base application](#runbase), and then close the browser and close Visual Studio.</span></span>
2. <span data-ttu-id="2119b-260">管理者特権で Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="2119b-260">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="2119b-261">(Azure コンピューティング エミュレーターを使用して管理者特権が必要です)。</span><span class="sxs-lookup"><span data-stu-id="2119b-261">(You'll be using the Azure compute emulator, and that requires administrator privileges.)</span></span>
3. <span data-ttu-id="2119b-262">アプリケーションで*Web.config*ファイル、 *MyFixIt* (web プロジェクト) をプロジェクトでの値を変更、 `appSettings/UseQueues` "true"に。</span><span class="sxs-lookup"><span data-stu-id="2119b-262">In the application *Web.config* file in the *MyFixIt* project (the web project), change the value of `appSettings/UseQueues` to "true":</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. <span data-ttu-id="2119b-263">場合、 [Azure ストレージ エミュレーター](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx)がまだ実行中、もう一度開始します。</span><span class="sxs-lookup"><span data-stu-id="2119b-263">If the [Azure storage emulator](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) isn't still running, start it again.</span></span>
5. <span data-ttu-id="2119b-264">FixIt web プロジェクトと MyFixItCloudService プロジェクトを同時に実行します。</span><span class="sxs-lookup"><span data-stu-id="2119b-264">Run the FixIt web project and the MyFixItCloudService project simultaneously.</span></span>

    <span data-ttu-id="2119b-265">Visual Studio を使用します。</span><span class="sxs-lookup"><span data-stu-id="2119b-265">Using Visual Studio:</span></span>

   1. <span data-ttu-id="2119b-266">キーを押して**F5** FixIt プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="2119b-266">Press **F5** to run the FixIt project.</span></span>
   2. <span data-ttu-id="2119b-267">**ソリューション エクスプ ローラー**MyFixItCloudService プロジェクトを右クリックし、クリックして**デバッグ** > **新しいインスタンスを開始**します。</span><span class="sxs-lookup"><span data-stu-id="2119b-267">In **Solution Explorer**, right-click the MyFixItCloudService project, and then click **Debug** > **Start New Instance**.</span></span>

    <span data-ttu-id="2119b-268">Visual Studio 2013 Express for Web の使用。</span><span class="sxs-lookup"><span data-stu-id="2119b-268">Using Visual Studio 2013 Express for Web:</span></span>

   3. <span data-ttu-id="2119b-269">ソリューション エクスプ ローラーでは、FixIt ソリューションを右クリックして**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="2119b-269">In Solution Explorer, right-click the FixIt solution and select **Properties**.</span></span>
   4. <span data-ttu-id="2119b-270">選択**マルチ スタートアップ プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="2119b-270">Select **Multiple Startup Projects**.</span></span>
   5. <span data-ttu-id="2119b-271">**アクション**MyFixIt と MyFixItCloudService、下のドロップダウン リストで選択**開始**します。</span><span class="sxs-lookup"><span data-stu-id="2119b-271">In the **Action** dropdown list under MyFixIt and MyFixItCloudService, select **Start**.</span></span>
   6. <span data-ttu-id="2119b-272">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2119b-272">Click **OK**.</span></span>
   7. <span data-ttu-id="2119b-273">キーを押して**F5**両方のプロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="2119b-273">Press **F5** to run both projects.</span></span>

      <span data-ttu-id="2119b-274">MyFixItCloudService プロジェクトを実行すると、Visual Studio は、Azure コンピューティング エミュレーターを起動します。</span><span class="sxs-lookup"><span data-stu-id="2119b-274">When you run the MyFixItCloudService project, Visual Studio starts the Azure compute emulator.</span></span> <span data-ttu-id="2119b-275">ファイアウォールの構成によっては、ファイアウォール経由のエミュレーターを許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-275">Depending on your firewall configuration, you might need to allow the emulator through the firewall.</span></span>

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a><span data-ttu-id="2119b-276">Windows PowerShell スクリプトを使用して基本アプリを Azure App Service Web Apps にデプロイする方法</span><span class="sxs-lookup"><span data-stu-id="2119b-276">How to deploy the base app to Azure App Service Web Apps by using the Windows PowerShell scripts</span></span>

<span data-ttu-id="2119b-277">説明するために、[自動化すべて](automate-everything.md)パターンでは、修正、アプリの Azure 環境を設定し、プロジェクトを新しい環境にデプロイするスクリプトが付属します。</span><span class="sxs-lookup"><span data-stu-id="2119b-277">To illustrate the [Automate Everything](automate-everything.md) pattern, the Fix It app is supplied with scripts that set up an environment in Azure and deploy the project to the new environment.</span></span> <span data-ttu-id="2119b-278">次の手順では、スクリプトを使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2119b-278">The following instructions explain how to use the scripts.</span></span>

<span data-ttu-id="2119b-279">キューを使用せずに Azure で実行するローカル キューで実行する変更を加えた場合は、次の手順に進む前に false に UseQueues appSetting 値を設定することを確認します。</span><span class="sxs-lookup"><span data-stu-id="2119b-279">If you want to run in Azure without using queues, and you made the changes to run locally with queues, make sure you set the UseQueues appSetting value back to false before proceeding with the following instructions.</span></span>

<span data-ttu-id="2119b-280">これらの手順は、既にダウンロードしてあると Fix It ソリューションをローカルで実行し、Azure があるかアカウントまたは Azure サブスクリプションがある想定しています。 管理が承認されています。</span><span class="sxs-lookup"><span data-stu-id="2119b-280">These instructions assume you have already downloaded and run the Fix It solution locally, and that you have an Azure account or have an Azure subscription that you are authorized to manage.</span></span>

1. <span data-ttu-id="2119b-281">インストール、 **Azure PowerShell**コンソール。</span><span class="sxs-lookup"><span data-stu-id="2119b-281">Install the **Azure PowerShell** console.</span></span> <span data-ttu-id="2119b-282">手順については、次を参照してください。[をインストールして、Azure PowerShell を構成する方法](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-282">For instructions, see [How to install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).</span></span>

    <span data-ttu-id="2119b-283">このカスタマイズされたコンソールを構成するには、Azure サブスクリプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="2119b-283">This customized console is configured to work with your Azure subscription.</span></span> <span data-ttu-id="2119b-284">Azure モジュールがインストールされている、 *Program Files*ディレクトリ、Azure PowerShell コンソールを使用するたびに自動的にインポートされます。</span><span class="sxs-lookup"><span data-stu-id="2119b-284">The Azure module is installed in the *Program Files* directory and is automatically imported on every use of the Azure PowerShell console.</span></span>

    <span data-ttu-id="2119b-285">Windows PowerShell ISE などの別のホスト プログラムで作業する場合に使用することを確認して、 [Import-module](https://go.microsoft.com/fwlink/?LinkID=141553)コマンドレットを Azure モジュールをインポートまたはモジュールの自動インポートをトリガーする、Azure モジュールのコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="2119b-285">If you prefer to work in a different host program, such as Windows PowerShell ISE, be sure to use the [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) cmdlet to import the Azure module or use a command in the Azure module to trigger automatic importing of the module.</span></span>
2. <span data-ttu-id="2119b-286">Azure PowerShell を起動、**管理者として実行**オプション。</span><span class="sxs-lookup"><span data-stu-id="2119b-286">Start Azure PowerShell with the **Run as administrator** option.</span></span>
3. <span data-ttu-id="2119b-287">実行、 [Set-executionpolicy](https://go.microsoft.com/fwlink/p/?linkid=293941)コマンドレットに、Azure PowerShell 実行ポリシーを設定する`RemoteSigned`します。</span><span class="sxs-lookup"><span data-stu-id="2119b-287">Run the [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) cmdlet to set the Azure PowerShell execution policy to `RemoteSigned`.</span></span> <span data-ttu-id="2119b-288">入力**Y** (Yes) のポリシーの変更を完了します。</span><span class="sxs-lookup"><span data-stu-id="2119b-288">Enter **Y** (for Yes) to complete the policy change.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    <span data-ttu-id="2119b-289">この設定では、デジタル署名されていないローカル スクリプトを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="2119b-289">This setting enables you to run local scripts that aren't digitally signed.</span></span> <span data-ttu-id="2119b-290">(実行ポリシーを設定することもできます`Unrestricted`後で、ブロックを解除する手順の必要はありませんが、これは、セキュリティ上の理由から推奨されません。)。</span><span class="sxs-lookup"><span data-stu-id="2119b-290">(You can also set the execution policy to `Unrestricted`, which would eliminate the need for the unblock step later, but this is not recommended for security reasons.)</span></span>
4. <span data-ttu-id="2119b-291">実行、`Add-AzureAccount`コマンドレット、アカウントの資格情報を PowerShell を使用します。</span><span class="sxs-lookup"><span data-stu-id="2119b-291">Run the `Add-AzureAccount` cmdlet to set up PowerShell with credentials for your account.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    <span data-ttu-id="2119b-292">これらの資格情報は、一定期間後に有効期限が切れるし、再実行する必要が、`Add-AzureAccount`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="2119b-292">These credentials expire after a period of time and you have to re-run the `Add-AzureAccount` cmdlet.</span></span> <span data-ttu-id="2119b-293">この電子書籍が書き込まれると資格情報の有効期限が切れる前に制限時間は 12 時間です。</span><span class="sxs-lookup"><span data-stu-id="2119b-293">As this e-book is being written, the time limit before credentials expire is 12 hours.</span></span>
5. <span data-ttu-id="2119b-294">複数のサブスクリプションがある場合は、テスト環境を作成するサブスクリプションを指定する Select-azuresubscription コマンドレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="2119b-294">If you have multiple subscriptions, use the Select-AzureSubscription cmdlet to specify the subscription you want to create the test environment in.</span></span>
6. <span data-ttu-id="2119b-295">使用して、同じ Azure サブスクリプションの管理証明書をインポート、`Get-AzurePublishSettingsFile`と`Import-AzurePublishSettingsFile`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="2119b-295">Import a management certificate for the same Azure subscription by using the `Get-AzurePublishSettingsFile` and `Import-AzurePublishSettingsFile` cmdlets.</span></span> <span data-ttu-id="2119b-296">これらのコマンドレットの最初の証明書ファイルをダウンロードして、2 つ目でインポートするためにそのファイルの場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="2119b-296">The first of these cmdlets downloads a certificate file, and in the second one you specify the location of that file in order to import it.</span></span> > [!IMPORTANT]
   > <span data-ttu-id="2119b-297">安全な場所にダウンロードしたファイルを保持または削除したら、Azure サービスの管理に使用できる証明書が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2119b-297">Keep the downloaded file in a safe location or delete it when you're done with it, because it contains a certificate that can be used to manage your Azure services.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    <span data-ttu-id="2119b-298">証明書が使用される SQL Database サーバー上のファイアウォール規則を設定するには、開発用コンピューターの IP アドレスを検出する REST API 呼び出し。</span><span class="sxs-lookup"><span data-stu-id="2119b-298">The certificate is used for a REST API call that detects the development machine's IP address in order to set a firewall rule on the SQL Database server.</span></span>
7. <span data-ttu-id="2119b-299">実行、 [Set-location](https://go.microsoft.com/fwlink/p/?linkid=293912)コマンドレット (エイリアスは`cd`、 `chdir`、および`sl`) スクリプトを含むディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="2119b-299">Run the [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (aliases are `cd`, `chdir`, and `sl`) to navigate to the directory that contains the scripts.</span></span> <span data-ttu-id="2119b-300">(ファイルに配置している、 *Automation* Fix It ソリューション フォルダー内のフォルダーです)。ディレクトリ名にスペースが含まれている場合は、引用符で囲まれたパスを配置します。</span><span class="sxs-lookup"><span data-stu-id="2119b-300">(They're located in the *Automation* folder in the Fix It solution folder.) Put the path in quotes if any of the directory names contain spaces.</span></span> <span data-ttu-id="2119b-301">たとえばに移動するため、`c:\Sample Apps\FixIt\Automation`ディレクトリは、次のコマンドを入力する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-301">For example, to navigate to the `c:\Sample Apps\FixIt\Automation` directory you could enter the following command:</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. <span data-ttu-id="2119b-302">これらのスクリプトを実行する Windows PowerShell を許可するのには、使用、 [Unblock-file](https://go.microsoft.com/fwlink/p/?linkid=294021)コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="2119b-302">To allow Windows PowerShell to run these scripts, use the [Unblock-File](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet.</span></span> <span data-ttu-id="2119b-303">(スクリプトは、インターネットからダウンロードされているためにがブロックされます)</span><span class="sxs-lookup"><span data-stu-id="2119b-303">(The scripts are blocked because they were downloaded from the Internet.)</span></span>

    > [!WARNING]
    > <span data-ttu-id="2119b-304">セキュリティ - 実行する前に`Unblock-File`スクリプトまたは実行可能ファイルで、メモ帳で開きのコマンドを確認および悪意のあるコードが含まれていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="2119b-304">Security - Before running `Unblock-File` on any script or executable file, open the file in Notepad, examine the commands, and verify that they do not contain any malicious code.</span></span>

    <span data-ttu-id="2119b-305">たとえば、次のコマンドを実行、`Unblock-File`コマンドレットは、現在のディレクトリ内のすべてのスクリプトにします。</span><span class="sxs-lookup"><span data-stu-id="2119b-305">For example, the following command runs the `Unblock-File` cmdlet on all scripts in the current directory.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. <span data-ttu-id="2119b-306">(キューの処理) ベースの web アプリを作成するには、そのアプリを修正、環境の作成スクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="2119b-306">To create the web app for the base (no queues processing) Fix It app, run the environment creation script.</span></span>

    <span data-ttu-id="2119b-307">必要な`Name`パラメーターは、データベースの名前を指定し、スクリプトを作成するストレージ アカウントにも使用します。</span><span class="sxs-lookup"><span data-stu-id="2119b-307">The required `Name` parameter specifies the name of the database and is also used for the storage account that the script creates.</span></span> <span data-ttu-id="2119b-308">名前は azurewebsites.net ドメイン内でグローバルに一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-308">The name must be globally unique within the azurewebsites.net domain.</span></span> <span data-ttu-id="2119b-309">Fixit やテストなど (または fixitdemo、例のようにも)、重複する名前を指定する場合、`New-AzureWebsite`コマンドレットは、競合を報告する内部エラーで失敗します。</span><span class="sxs-lookup"><span data-stu-id="2119b-309">If you specify a name that is not unique, like Fixit or Test (or even as in the example, fixitdemo), the `New-AzureWebsite` cmdlet fails with an Internal Error that reports a conflict.</span></span> <span data-ttu-id="2119b-310">スクリプトは、web アプリ、ストレージ アカウント、およびデータベースの名前の要件に準拠するすべて小文字に名前を変換します。</span><span class="sxs-lookup"><span data-stu-id="2119b-310">The script converts the name to all lower-case to comply with name requirements for web apps, storage accounts, and databases.</span></span>

    <span data-ttu-id="2119b-311">必要な`SqlDatabasePassword`パラメーターは、SQL Database 用に作成される管理者アカウントのパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="2119b-311">The required `SqlDatabasePassword` parameter specifies the password for the admin account that will be created for SQL Database.</span></span> <span data-ttu-id="2119b-312">パスワードに特殊な XML 文字を含めないでください (&amp; &lt; &gt; ;)。</span><span class="sxs-lookup"><span data-stu-id="2119b-312">Don't include special XML characters in the password (&amp; &lt; &gt; ;).</span></span> <span data-ttu-id="2119b-313">これは、スクリプトに書かれて、Azure の制限ではない方法の制限です。</span><span class="sxs-lookup"><span data-stu-id="2119b-313">This is a limitation of the way the scripts were written, not a limitation of Azure.</span></span>

    <span data-ttu-id="2119b-314">たとえば、"fixitdemo"という名前の web アプリを作成して SQL Server の管理者パスワード"Passw0rd1"を使用する場合は、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="2119b-314">For example, if you want to create a web app named "fixitdemo" and use a SQL Server administrator password of "Passw0rd1", you could enter the following command:</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    <span data-ttu-id="2119b-315">名前は azurewebsites.net ドメインで一意である必要があり、パスワードがパスワードの複雑さの SQL データベースの要件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-315">The name must be unique in the azurewebsites.net domain, and the password must meet SQL Database requirements for password complexity.</span></span> <span data-ttu-id="2119b-316">(例 Passw0rd1 が要件を満たしています。)</span><span class="sxs-lookup"><span data-stu-id="2119b-316">(The example Passw0rd1 does meet the requirements.)</span></span>

    <span data-ttu-id="2119b-317">コマンドの最初に注意してくださいです".\"。</span><span class="sxs-lookup"><span data-stu-id="2119b-317">Note that the command begins with ".\".</span></span> <span data-ttu-id="2119b-318">Windows PowerShell では、悪意のあるスクリプトの実行を防ぐため、スクリプトを実行するときに、スクリプト ファイルへの完全修飾パスを提供することが必要です。</span><span class="sxs-lookup"><span data-stu-id="2119b-318">To help prevent malicious execution of scripts, Windows PowerShell requires that you provide the fully qualified path to the script file when you run a script.</span></span> <span data-ttu-id="2119b-319">現在のディレクトリを示すために、ドットを使用することができます (".\")または、完全修飾パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="2119b-319">You can use a dot to indicate the current directory (".\") or provide the fully qualified path, such as:</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    <span data-ttu-id="2119b-320">スクリプトの詳細については、使用、`Get-Help`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="2119b-320">For more information about the script, use the `Get-Help` cmdlet.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    <span data-ttu-id="2119b-321">使用することができます、 `Detailed`、 `Full`、 `Parameters`、および`Examples`返されるヘルプをフィルター処理、Get-help コマンドレットのパラメーター。</span><span class="sxs-lookup"><span data-stu-id="2119b-321">You can use the `Detailed`, `Full`, `Parameters`, and `Examples` parameters of the Get-Help cmdlet to filter the help that is returned.</span></span>

    <span data-ttu-id="2119b-322">スクリプトが失敗したり、「New-azurewebsite:: 呼び出す Set-azuresubscription と Select-azuresubscription 最初に、」などのエラーを生成する場合は、Azure PowerShell の構成を完了していない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-322">If the script fails or generates errors, such as "New-AzureWebsite : Call Set-AzureSubscription and Select-AzureSubscription first," you might not have completed the configuration of Azure PowerShell.</span></span>

    <span data-ttu-id="2119b-323">ように作成されたリソースを表示する、Azure 管理ポータルを使用するには、スクリプトの完了後、[自動化すべて](automate-everything.md)」の章。</span><span class="sxs-lookup"><span data-stu-id="2119b-323">After the script finishes, you can use the Azure Management Portal to see the resources that were created, as shown in the [Automate Everything](automate-everything.md) chapter.</span></span>
10. <span data-ttu-id="2119b-324">FixIt プロジェクトを新しい Azure 環境に展開するには、使用、 *AzureWebsite.ps1*スクリプト。</span><span class="sxs-lookup"><span data-stu-id="2119b-324">To deploy the FixIt project to the new Azure environment, use the *AzureWebsite.ps1* script.</span></span> <span data-ttu-id="2119b-325">例えば:</span><span class="sxs-lookup"><span data-stu-id="2119b-325">For example:</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    <span data-ttu-id="2119b-326">デプロイが完了したら、修正、Azure で実行されているとブラウザーが開きます。</span><span class="sxs-lookup"><span data-stu-id="2119b-326">When deployment is done, the browser opens with Fix It running in Azure.</span></span>

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a><span data-ttu-id="2119b-327">Windows PowerShell スクリプトのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="2119b-327">Troubleshooting the Windows PowerShell scripts</span></span>

<span data-ttu-id="2119b-328">これらのスクリプトを実行しているときに発生する最も一般的なエラーに関連するアクセス許可。</span><span class="sxs-lookup"><span data-stu-id="2119b-328">The most common errors encountered when running these scripts are related to permissions.</span></span> <span data-ttu-id="2119b-329">確認します`Add-AzureAccount`と`Import-AzurePublishSettingsFile`が正常に実行し、同じ Azure サブスクリプションのためにします。</span><span class="sxs-lookup"><span data-stu-id="2119b-329">Make sure that `Add-AzureAccount` and `Import-AzurePublishSettingsFile` were successful and that you used them for the same Azure subscription.</span></span> <span data-ttu-id="2119b-330">場合でも`Add-AzureAccount`が成功したが、もう一度実行する場合があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-330">Even if `Add-AzureAccount` was successful you might have to run it again.</span></span> <span data-ttu-id="2119b-331">によって追加されたアクセス許可`Add-AzureAccount`12 時間以内に期限切れにします。</span><span class="sxs-lookup"><span data-stu-id="2119b-331">The permissions added by `Add-AzureAccount` expire in 12 hours.</span></span>

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a><span data-ttu-id="2119b-332">オブジェクト参照がオブジェクト インスタンスに設定されていません。</span><span class="sxs-lookup"><span data-stu-id="2119b-332">Object reference not set to an instance of an object.</span></span>

<span data-ttu-id="2119b-333">「オブジェクト参照、オブジェクトのインスタンスに設定されていない」は、Windows PowerShell がプロセス (これは、null 参照の例外です) にオブジェクトを見つけることはできませんが実行されるなど、スクリプトは、エラーを返した場合、`Add-AzureAccount`コマンドレットとスクリプトをもう一度やり直してください。</span><span class="sxs-lookup"><span data-stu-id="2119b-333">If the script returns errors, such as "Object reference not set to an instance of an object," which means that Windows PowerShell can't find an object to process (this is a null reference exception), run the `Add-AzureAccount` cmdlet and try the script again.</span></span>

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a><span data-ttu-id="2119b-334">InternalError: サーバーには、内部エラーが発生しました。</span><span class="sxs-lookup"><span data-stu-id="2119b-334">InternalError: The server encountered an internal error.</span></span>

<span data-ttu-id="2119b-335">`New-AzureWebsite`コマンドレットでは、azurewebsites.net ドメインで名前が一意でないときに内部エラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="2119b-335">The `New-AzureWebsite` cmdlet returns an internal error when the name is not unique in the azurewebsites.net domain.</span></span> <span data-ttu-id="2119b-336">エラーを解決するには、Name パラメーターには、名前の別の値を使用して、*新規 AzureWebsiteEnv.ps1*します。</span><span class="sxs-lookup"><span data-stu-id="2119b-336">To resolve the error, use a different value for the name, which is in the Name parameter of *New-AzureWebsiteEnv.ps1*.</span></span>

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a><span data-ttu-id="2119b-337">スクリプトを再起動します。</span><span class="sxs-lookup"><span data-stu-id="2119b-337">Restarting the script</span></span>

<span data-ttu-id="2119b-338">再起動する必要がある場合、*新規 AzureWebsiteEnv.ps1*スクリプト「スクリプトが完了しました」のメッセージを印刷する前に、しなかったために、前に作成したスクリプトが停止しているリソースを削除する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-338">If you need to restart the *New-AzureWebsiteEnv.ps1* script because it failed before it printed the "Script is complete" message, you might want to delete resources that the script created before it stopped.</span></span> <span data-ttu-id="2119b-339">たとえば、スクリプトを既に作成して ContosoFixItDemo web アプリと同じ名前のスクリプトをもう一度実行する場合、名前が使用されているためにのスクリプトでは失敗します。</span><span class="sxs-lookup"><span data-stu-id="2119b-339">For example, if the script already created the ContosoFixItDemo web app and you run the script again with the same name, the script will fail because the name is in use.</span></span>

<span data-ttu-id="2119b-340">停止するまでに作成したスクリプトを対象のリソースを確認するには、次のコマンドレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="2119b-340">To determine which resources the script created before it stopped, use the following cmdlets:</span></span>

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- <span data-ttu-id="2119b-341">`Get-AzureSqlDatabase`: このコマンドレットを実行するにはデータベース サーバーの名前をパイプ`Get-AzureSqlDatabase`:   `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`</span><span class="sxs-lookup"><span data-stu-id="2119b-341">`Get-AzureSqlDatabase`: To run this cmdlet, pipe the database server name to `Get-AzureSqlDatabase`:   `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`</span></span>

<span data-ttu-id="2119b-342">これらのリソースを削除するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="2119b-342">To delete these resources, use the following commands.</span></span> <span data-ttu-id="2119b-343">場合は、データベース サーバーを削除すると、自動的にデータベースを削除するサーバーに関連付けられているに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2119b-343">Note that if you delete the database server, you automatically delete the databases associated with the server.</span></span>

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a><span data-ttu-id="2119b-344">Azure App Service Web Apps と Azure クラウド サービスを処理キューに、アプリをデプロイする方法</span><span class="sxs-lookup"><span data-stu-id="2119b-344">How to deploy the app with queue processing to Azure App Service Web Apps and an Azure Cloud Service</span></span>

<span data-ttu-id="2119b-345">キューを有効にするには、MyFixIt\Web.config ファイルで、次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="2119b-345">To enable queues, make the following change in the MyFixIt\Web.config file.</span></span> <span data-ttu-id="2119b-346">`appSettings`の値を変更`UseQueues`"true"にします。</span><span class="sxs-lookup"><span data-stu-id="2119b-346">Under `appSettings`, change the value of `UseQueues` to "true":</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

<span data-ttu-id="2119b-347">説明に従って、Azure App Service で web アプリに、MVC アプリケーションを展開し、[以前](#deploybase)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-347">Then deploy the MVC application to an web app in Azure App Service, as described [earlier](#deploybase).</span></span>

<span data-ttu-id="2119b-348">次に、新しい Azure クラウド サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="2119b-348">Next, create a new Azure cloud service.</span></span> <span data-ttu-id="2119b-349">Fix It アプリに含まれているスクリプトを作成またはしないでこの Azure portal を使用する必要がありますので、クラウド サービスをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="2119b-349">The scripts included with the Fix It app do not create or deploy the cloud service, so you must use Azure portal for this.</span></span> <span data-ttu-id="2119b-350">ポータルで、次のようにクリックします。**新規** -- **コンピューティング**–**クラウド サービス** -- **簡易作成**、し、URL を入力します。データ センターの場所。</span><span class="sxs-lookup"><span data-stu-id="2119b-350">In the portal, click **New** -- **Compute** – **Cloud Service** -- **Quick Create**, and then enter a URL and a data center location.</span></span> <span data-ttu-id="2119b-351">Web アプリを配置した同じデータ センターを使用します。</span><span class="sxs-lookup"><span data-stu-id="2119b-351">Use the same data center where you deployed the web app.</span></span>

![](the-fix-it-sample-application/_static/image1.png)

<span data-ttu-id="2119b-352">クラウド サービスを展開する前に、構成ファイルの一部を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2119b-352">Before you can deploy the cloud service, you need to update some of the configuration files.</span></span>

<span data-ttu-id="2119b-353">MyFixIt.WorkerRoler\app.config で `connectionStrings`の値を置き換える、 `appdb` SQL データベースの実際の接続文字列で接続文字列。</span><span class="sxs-lookup"><span data-stu-id="2119b-353">In MyFixIt.WorkerRoler\app.config, under `connectionStrings`, replace the value of the `appdb` connection string with the actual connection string for the SQL Database.</span></span> <span data-ttu-id="2119b-354">接続文字列は、ポータルから取得できます。</span><span class="sxs-lookup"><span data-stu-id="2119b-354">You can get the connection string from the portal.</span></span> <span data-ttu-id="2119b-355">ポータルで、次のようにクリックします。 **SQL データベース** - **appdb** - **ADO .Net、ODBC、PHP、および JDBC の接続文字列を SQL データベースの表示**します。</span><span class="sxs-lookup"><span data-stu-id="2119b-355">In the portal, click **SQL Databases** - **appdb** - **View SQL Database connection strings for ADO .Net, ODBC, PHP, and JDBC**.</span></span> <span data-ttu-id="2119b-356">ADO.NET 接続文字列をコピーして、app.config ファイルに値を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="2119b-356">Copy the ADO.NET connection string and paste the value into the app.config file.</span></span> <span data-ttu-id="2119b-357">置換"{、\_パスワード\_ここ}"をデータベースのパスワード。</span><span class="sxs-lookup"><span data-stu-id="2119b-357">Replace "{your\_password\_here}" with your database password.</span></span> <span data-ttu-id="2119b-358">(内のデータベース パスワードを指定した MVC アプリをデプロイするスクリプトを使用すると仮定すると、`SqlDatabasePassword`パラメーターのスクリプトを作成します)。</span><span class="sxs-lookup"><span data-stu-id="2119b-358">(Assuming you used the scripts to deploy the MVC app, you specified the database password in the `SqlDatabasePassword` script parameter.)</span></span>

<span data-ttu-id="2119b-359">結果は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2119b-359">The result should look like the following:</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

<span data-ttu-id="2119b-360">同じ MyFixIt.WorkerRoler\app.config ファイルで  `appSettings`、Azure ストレージ アカウントの 2 つのプレース ホルダーの値を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2119b-360">In the same MyFixIt.WorkerRoler\app.config file, under `appSettings`, replace the two placeholder values for the Azure storage account.</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

<span data-ttu-id="2119b-361">アクセス キーは、ポータルから取得できます。</span><span class="sxs-lookup"><span data-stu-id="2119b-361">You can get the access key from the portal.</span></span> <span data-ttu-id="2119b-362">参照してください[ストレージ アカウントを管理する方法](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-362">See [How To Manage Storage Accounts](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).</span></span>

<span data-ttu-id="2119b-363">MyFixItCloudService\ServiceConfiguration.Cloud.cscfg、Azure ストレージ アカウントの同じ 2 つのプレース ホルダー値に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="2119b-363">In MyFixItCloudService\ServiceConfiguration.Cloud.cscfg, replace the same two placeholders values for the Azure storage account.</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

<span data-ttu-id="2119b-364">クラウド サービスをデプロイする準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="2119b-364">Now you are ready to deploy the cloud service.</span></span> <span data-ttu-id="2119b-365">ソリューション エクスプ ローラーでは、MyFixItCloudService プロジェクトを右クリックして**発行**します。</span><span class="sxs-lookup"><span data-stu-id="2119b-365">In Solution Explore, right-click the MyFixItCloudService project and select **Publish**.</span></span> <span data-ttu-id="2119b-366">詳細については、次を参照してください。"[アプリケーションを Azure にデプロイ](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)"、第 2 部である[このチュートリアル](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36)します。</span><span class="sxs-lookup"><span data-stu-id="2119b-366">For more information, see "[Deploy the Application to Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", which is in part 2 of [this tutorial](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2119b-367">前へ</span><span class="sxs-lookup"><span data-stu-id="2119b-367">Previous</span></span>](more-patterns-and-guidance.md)
