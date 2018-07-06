---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: (Azure での実際のクラウド アプリの構築) すべてを自動化 |Microsoft Docs
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 87b697847845ab88943e7a659ccd9487b9408d5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839636"
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="60e03-104">(Azure での実際のクラウド アプリの構築) すべてを自動化します。</span><span class="sxs-lookup"><span data-stu-id="60e03-104">Automate Everything (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="60e03-105">によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="60e03-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="60e03-106">[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="60e03-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="60e03-107">**構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。</span><span class="sxs-lookup"><span data-stu-id="60e03-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="60e03-108">13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。</span><span class="sxs-lookup"><span data-stu-id="60e03-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="60e03-109">電子書籍の概要については、次を参照してください。[第 1 章](introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="60e03-109">For an introduction to the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="60e03-110">実際には、見て最初の 3 つのパターンには、あらゆるソフトウェア開発プロジェクト、特にクラウド プロジェクトが適用されます。</span><span class="sxs-lookup"><span data-stu-id="60e03-110">The first three patterns we'll look at actually apply to any software development project, but especially to cloud projects.</span></span> <span data-ttu-id="60e03-111">このパターンは、開発タスクの自動化についてです。</span><span class="sxs-lookup"><span data-stu-id="60e03-111">This pattern is about automating development tasks.</span></span> <span data-ttu-id="60e03-112">手動プロセスは低速でエラーが発生しやすい; は、重要なトピック高速かつ信頼性が高く、アジャイルのワークフローを設定することの支援としてそれらの多くを自動化します。</span><span class="sxs-lookup"><span data-stu-id="60e03-112">It's an important topic because manual processes are slow and error-prone; automating as many of them as possible helps set up a fast, reliable, and agile workflow.</span></span> <span data-ttu-id="60e03-113">困難または不可能なオンプレミス環境を自動化するには多くのタスクを簡単に自動化できるため、クラウド開発を一意に重要です。</span><span class="sxs-lookup"><span data-stu-id="60e03-113">It's uniquely important for cloud development because you can easily automate many tasks that are difficult or impossible to automate in an on-premises environment.</span></span> <span data-ttu-id="60e03-114">たとえば、テスト全体を設定することが新しい web サーバーとバックエンド Vm を含む環境では、データベース、blob storage (ファイル ストレージ)、キューなど。</span><span class="sxs-lookup"><span data-stu-id="60e03-114">For example, you can set up whole test environments including new web server and back-end VMs, databases, blob storage (file storage), queues, etc.</span></span>

## <a name="devops-workflow"></a><span data-ttu-id="60e03-115">DevOps ワークフロー</span><span class="sxs-lookup"><span data-stu-id="60e03-115">DevOps Workflow</span></span>

<span data-ttu-id="60e03-116">"DevOps"という用語が聞こえるますます</span><span class="sxs-lookup"><span data-stu-id="60e03-116">Increasingly you hear the term "DevOps."</span></span> <span data-ttu-id="60e03-117">ソフトウェアを効率的に開発するために開発と運用タスクを統合することがある認識外という用語を開発しました。</span><span class="sxs-lookup"><span data-stu-id="60e03-117">The term developed out of a recognition that you have to integrate development and operations tasks in order to develop software efficiently.</span></span> <span data-ttu-id="60e03-118">有効にワークフローの種類をすることができますアプリを開発、デプロイ運用環境の演算子の使用からについて説明します、学習した内容への応答で変更、迅速かつ確実に、サイクルを繰り返します。</span><span class="sxs-lookup"><span data-stu-id="60e03-118">The kind of workflow you want to enable is one in which you can develop an app, deploy it, learn from production usage of it, change it in response to what you've learned, and repeat the cycle quickly and reliably.</span></span>

<span data-ttu-id="60e03-119">一部成功するクラウドの開発チームは、実際の環境に 1 日複数回をデプロイします。</span><span class="sxs-lookup"><span data-stu-id="60e03-119">Some successful cloud development teams deploy multiple times a day to a live environment.</span></span> <span data-ttu-id="60e03-120">Azure チーム、大規模な展開に使用更新でも管理はすべて 2 ~ 3 か月ごとの 2 ~ 3 日間とメジャー リリース 2 ~ 3 週間ごとのリリースのマイナー アップデートします。</span><span class="sxs-lookup"><span data-stu-id="60e03-120">The Azure team used to deploy a major update every 2-3 months, but now it releases minor updates every 2-3 days and major releases every 2-3 weeks.</span></span> <span data-ttu-id="60e03-121">そのリズムにではお客様のフィードバックに素早く対応する実際に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="60e03-121">Getting into that cadence really helps you be responsive to customer feedback.</span></span>

<span data-ttu-id="60e03-122">このためには、開発とデプロイ サイクルが反復可能な信頼性の高い、予測可能なおよび低サイクル時間を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="60e03-122">In order to do that, you have to enable a development and deployment cycle that is repeatable, reliable, predictable, and has low cycle time.</span></span>

![DevOps ワークフロー](automate-everything/_static/image1.png)

<span data-ttu-id="60e03-124">つまり、機能のアイデアがある場合と、顧客の使用しフィードバックを提供するとの間の期間はできるだけ短くする必要があります。</span><span class="sxs-lookup"><span data-stu-id="60e03-124">In other words, the period of time between when you have an idea for a feature and when the customers are using it and providing feedback must be as short as possible.</span></span> <span data-ttu-id="60e03-125">最初の 3 つのパターン: は、すべてのソースの管理を自動化し、継続的インテグレーションと配信の本質はそのようなプロセスを有効にするために推奨されるベスト プラクティス。</span><span class="sxs-lookup"><span data-stu-id="60e03-125">The first three patterns – automate everything, source control, and continuous integration and delivery -- are all about best practices that we recommend in order to enable that kind of process.</span></span>

## <a name="azure-management-scripts"></a><span data-ttu-id="60e03-126">Azure の管理スクリプト</span><span class="sxs-lookup"><span data-stu-id="60e03-126">Azure management scripts</span></span>

<span data-ttu-id="60e03-127">[この電子書籍の紹介](introduction.md)、web ベース コンソールで、Azure 管理ポータルを説明しました。</span><span class="sxs-lookup"><span data-stu-id="60e03-127">In the [introduction to this e-book](introduction.md), you saw the web-based console, the Azure Management Portal.</span></span> <span data-ttu-id="60e03-128">管理ポータルを使用すると、監視およびすべての Azure にデプロイしたリソースを管理できます。</span><span class="sxs-lookup"><span data-stu-id="60e03-128">The management portal enables you to monitor and manage all of the resources that you have deployed on Azure.</span></span> <span data-ttu-id="60e03-129">作成、web アプリと仮想マシンなどのサービスを削除し、それらのサービスを構成、サービスの操作を監視およびなどを簡単になります。</span><span class="sxs-lookup"><span data-stu-id="60e03-129">It's an easy way to create and delete services such as web apps and VMs, configure those services, monitor service operation, and so forth.</span></span> <span data-ttu-id="60e03-130">すばらしいツールですが、これを使用して手動プロセスであります。</span><span class="sxs-lookup"><span data-stu-id="60e03-130">It's a great tool, but using it is a manual process.</span></span> <span data-ttu-id="60e03-131">場合は、あらゆるサイズの実稼働アプリケーションを開発しようとしているし、チーム環境で特にお勧め、ポータルについて説明し、Azure を調査するために UI を使用し、繰り返し行うプロセスを自動化します。</span><span class="sxs-lookup"><span data-stu-id="60e03-131">If you're going to develop a production application of any size, and especially in a team environment, we recommend that you go through the portal UI in order to learn and explore Azure, and then automate the processes that you'll be doing repetitively.</span></span>

<span data-ttu-id="60e03-132">管理ポータルで、または Visual Studio から手動で行うことができますほぼすべてのものは、REST 管理 API を呼び出すことによっても実行できます。</span><span class="sxs-lookup"><span data-stu-id="60e03-132">Nearly everything that you can do manually in the management portal or from Visual Studio can also be done by calling the REST management API.</span></span> <span data-ttu-id="60e03-133">使用してスクリプトを記述する[Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx)などのオープン ソース フレームワークを使用できますか[Chef](http://www.opscode.com/chef/)または[Puppet](http://puppetlabs.com/puppet/what-is-puppet)します。</span><span class="sxs-lookup"><span data-stu-id="60e03-133">You can write scripts using [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), or you can use an open source framework such as [Chef](http://www.opscode.com/chef/) or [Puppet](http://puppetlabs.com/puppet/what-is-puppet).</span></span> <span data-ttu-id="60e03-134">Mac または Linux の環境での Bash コマンド ライン ツールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="60e03-134">You can also use the Bash command-line tool in a Mac or Linux environment.</span></span> <span data-ttu-id="60e03-135">Azure では、これらすべての異なる環境でのスクリプト Api とは、 [.NET 管理 API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)スクリプトではなくコードを記述する場合。</span><span class="sxs-lookup"><span data-stu-id="60e03-135">Azure has scripting APIs for all those different environments, and it has a [.NET management API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) in case you want to write code instead of script.</span></span>

<span data-ttu-id="60e03-136">Fix It アプリのテスト環境を作成し、その環境にプロジェクトをデプロイのプロセスを自動化するいくつかの Windows PowerShell スクリプトを作成しましたし、これらのスクリプトの内容の一部を確認します。</span><span class="sxs-lookup"><span data-stu-id="60e03-136">For the Fix It app we've created some Windows PowerShell scripts that automate the processes of creating a test environment and deploying the project to that environment, and we'll review some of the contents of those scripts.</span></span>

## <a name="environment-creation-script"></a><span data-ttu-id="60e03-137">環境の作成スクリプト</span><span class="sxs-lookup"><span data-stu-id="60e03-137">Environment creation script</span></span>

<span data-ttu-id="60e03-138">最初のスクリプトを見ていきますが名前付き*新規 AzureWebsiteEnv.ps1*します。</span><span class="sxs-lookup"><span data-stu-id="60e03-138">The first script we'll look at is named *New-AzureWebsiteEnv.ps1*.</span></span> <span data-ttu-id="60e03-139">修正をテスト用のアプリを導入することが Azure 環境を作成します。</span><span class="sxs-lookup"><span data-stu-id="60e03-139">It creates an Azure environment that you can deploy the Fix It app to for testing.</span></span> <span data-ttu-id="60e03-140">このスクリプトを実行する主なタスクは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60e03-140">The main tasks that this script performs are the following:</span></span>

- <span data-ttu-id="60e03-141">Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="60e03-141">Create a web app.</span></span>
- <span data-ttu-id="60e03-142">ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="60e03-142">Create a storage account.</span></span> <span data-ttu-id="60e03-143">(必要な blob やキューの後半の章でわかります。)</span><span class="sxs-lookup"><span data-stu-id="60e03-143">(Required for blobs and queues, as you'll see in later chapters.)</span></span>
- <span data-ttu-id="60e03-144">SQL Database サーバーと 2 つのデータベースの作成: アプリケーション データベース、およびメンバーシップ データベース。</span><span class="sxs-lookup"><span data-stu-id="60e03-144">Create a SQL Database server and two databases: an application database, and a membership database.</span></span>
- <span data-ttu-id="60e03-145">アプリは、ストレージ アカウントとデータベースへのアクセスに使用する azure の設定を格納します。</span><span class="sxs-lookup"><span data-stu-id="60e03-145">Store settings in Azure that the app will use to access the storage account and databases.</span></span>
- <span data-ttu-id="60e03-146">展開を自動化するために使用する設定ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="60e03-146">Create settings files that will be used to automate deployment.</span></span>

### <a name="run-the-script"></a><span data-ttu-id="60e03-147">スクリプトを実行します</span><span class="sxs-lookup"><span data-stu-id="60e03-147">Run the script</span></span>


> [!NOTE]
> <span data-ttu-id="60e03-148">この章のこの部分は、それらを実行するために入力したコマンドおよびスクリプトの例を示します。</span><span class="sxs-lookup"><span data-stu-id="60e03-148">This part of the chapter shows examples of scripts and the commands that you enter in order to run them.</span></span> <span data-ttu-id="60e03-149">このデモ スクリプトを実行するために知っておくべきすべてのものが提供されません。</span><span class="sxs-lookup"><span data-stu-id="60e03-149">This a demo and doesn't provide everything you need to know in order to run the scripts.</span></span> <span data-ttu-id="60e03-150">方法を操作を行います it 手順については、次を参照してください。[付録:、、サンプル アプリケーションの修正](the-fix-it-sample-application.md#deploybase)します。</span><span class="sxs-lookup"><span data-stu-id="60e03-150">For step-by-step how-to-do-it instructions, see [Appendix: The Fix It Sample Application](the-fix-it-sample-application.md#deploybase).</span></span>


<span data-ttu-id="60e03-151">Azure サービスを管理する PowerShell スクリプトを実行するには、Azure PowerShell コンソールをインストールし、Azure サブスクリプションで動作するように構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60e03-151">To run a PowerShell script that manages Azure services you have to install the Azure PowerShell console and configure it to work with your Azure subscription.</span></span> <span data-ttu-id="60e03-152">設定しているとは、次のようなコマンドを使用して、Fix It 環境の作成スクリプトを実行できます。</span><span class="sxs-lookup"><span data-stu-id="60e03-152">Once you're set up, you can run the Fix It environment creation script with a command like this one:</span></span>

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

<span data-ttu-id="60e03-153">`Name`パラメーターは、データベースとストレージ アカウントを作成するときに使用される名前を指定し、`SqlDatabasePassword`パラメーターは、SQL Database 用に作成される管理者アカウントのパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="60e03-153">The `Name` parameter specifies the name to be used when creating the database and storage accounts, and the `SqlDatabasePassword` parameter specifies the password for the admin account that will be created for SQL Database.</span></span> <span data-ttu-id="60e03-154">見て後で使用できるその他のパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="60e03-154">There are other parameters you can use that we'll look at later.</span></span>

![PowerShell ウィンドウ](automate-everything/_static/image2.png)

<span data-ttu-id="60e03-156">スクリプトが完了後することができますを参照してください、管理ポータルで作成したもの。</span><span class="sxs-lookup"><span data-stu-id="60e03-156">After the script finishes you can see in the management portal what was created.</span></span> <span data-ttu-id="60e03-157">2 つのデータベースにあります。</span><span class="sxs-lookup"><span data-stu-id="60e03-157">You'll find two databases:</span></span>

![Databases](automate-everything/_static/image3.png)

<span data-ttu-id="60e03-159">ストレージ アカウント:</span><span class="sxs-lookup"><span data-stu-id="60e03-159">A storage account:</span></span>

![ストレージ アカウント](automate-everything/_static/image4.png)

<span data-ttu-id="60e03-161">Web アプリ:</span><span class="sxs-lookup"><span data-stu-id="60e03-161">And a web app:</span></span>

![Web サイト](automate-everything/_static/image5.png)

<span data-ttu-id="60e03-163">**構成**タブ web アプリがストレージ アカウントの設定および SQL データベース接続文字列設定されている修正プログラムをアプリを表示できます。</span><span class="sxs-lookup"><span data-stu-id="60e03-163">On the **Configure** tab for the web app, you can see that it has the storage account settings and SQL database connection strings set up for the Fix It app.</span></span>

![appSettings、connectionStrings](automate-everything/_static/image6.png)

<span data-ttu-id="60e03-165">*Automation*フォルダーもに、  *&lt;websitename&gt;.pubxml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="60e03-165">The *Automation* folder now also contains a *&lt;websitename&gt;.pubxml* file.</span></span> <span data-ttu-id="60e03-166">このファイルには、MSBuild がアプリケーションを作成した Azure 環境のデプロイに使用する設定が格納されます。</span><span class="sxs-lookup"><span data-stu-id="60e03-166">This file stores settings that MSBuild will use to deploy the application to the Azure environment that was just created.</span></span> <span data-ttu-id="60e03-167">例えば:</span><span class="sxs-lookup"><span data-stu-id="60e03-167">For example:</span></span>

[!code-xml[Main](automate-everything/samples/sample1.xml)]

<span data-ttu-id="60e03-168">ご覧のように、スクリプトが、完全なテスト環境を作成し、プロセス全体は約 90 秒間で行われます。</span><span class="sxs-lookup"><span data-stu-id="60e03-168">As you can see, the script has created a complete test environment, and the whole process is done in about 90 seconds.</span></span>

<span data-ttu-id="60e03-169">テスト環境を作成する場合、チームの他のユーザーが、スクリプトを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="60e03-169">If someone else on your team wants to create a test environment, they can just run the script.</span></span> <span data-ttu-id="60e03-170">だけでなく、高速ですが、できると確信を使用するものと同じ環境を使用していることもできます。</span><span class="sxs-lookup"><span data-stu-id="60e03-170">Not only is it fast, but also they can be confident that they are using an environment identical to the one you're using.</span></span> <span data-ttu-id="60e03-171">さほど設定する場合は、すべてのユーザーがモ ノ手動で管理ポータル UI を使用しているは確信はできませんでした。</span><span class="sxs-lookup"><span data-stu-id="60e03-171">You couldn't be quite as confident of that if everyone was setting things up manually by using the management portal UI.</span></span>

### <a name="a-look-at-the-scripts"></a><span data-ttu-id="60e03-172">スクリプトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60e03-172">A look at the scripts</span></span>

<span data-ttu-id="60e03-173">実際には 3 つのスクリプトをこの作業を行うにがあります。</span><span class="sxs-lookup"><span data-stu-id="60e03-173">There are actually three scripts that do this work.</span></span> <span data-ttu-id="60e03-174">コマンドラインから 1 つを呼び出すし、一部のタスクの実行を自動的に他の 2 つを使用します。</span><span class="sxs-lookup"><span data-stu-id="60e03-174">You call one from the command line and it automatically uses the other two to do some of the tasks:</span></span>

- <span data-ttu-id="60e03-175">*新しい AzureWebSiteEnv.ps1*メイン スクリプトに示します。</span><span class="sxs-lookup"><span data-stu-id="60e03-175">*New-AzureWebSiteEnv.ps1* is the main script.</span></span>

    - <span data-ttu-id="60e03-176">*新しい AzureStorage.ps1*ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="60e03-176">*New-AzureStorage.ps1* creates the storage account.</span></span>
    - <span data-ttu-id="60e03-177">*新しい AzureSql.ps1*データベースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="60e03-177">*New-AzureSql.ps1* creates the databases.</span></span>

### <a name="parameters-in-the-main-script"></a><span data-ttu-id="60e03-178">メインのスクリプトのパラメーター</span><span class="sxs-lookup"><span data-stu-id="60e03-178">Parameters in the main script</span></span>

<span data-ttu-id="60e03-179">メインのスクリプトでは、*新規 AzureWebSiteEnv.ps1*、いくつかのパラメーターを定義します。</span><span class="sxs-lookup"><span data-stu-id="60e03-179">The main script, *New-AzureWebSiteEnv.ps1*, defines several parameters:</span></span>

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

<span data-ttu-id="60e03-180">2 つのパラメーターが必要です。</span><span class="sxs-lookup"><span data-stu-id="60e03-180">Two parameters are required:</span></span>

- <span data-ttu-id="60e03-181">このスクリプトを作成する web アプリの名前。</span><span class="sxs-lookup"><span data-stu-id="60e03-181">The name of the web app that the script creates.</span></span> <span data-ttu-id="60e03-182">(これにも、URL の使用: `<name>.azurewebsites.net`)。</span><span class="sxs-lookup"><span data-stu-id="60e03-182">(This is also used for the URL: `<name>.azurewebsites.net`.)</span></span>
- <span data-ttu-id="60e03-183">このスクリプトを作成するデータベース サーバーの場合は、新しい管理ユーザーのパスワード。</span><span class="sxs-lookup"><span data-stu-id="60e03-183">The password for the new administrative user of the database server that the script creates.</span></span>

<span data-ttu-id="60e03-184">省略可能なパラメーターを使用すると、データ センターの場所 (既定値は「米国西部」)、データベース サーバー管理者名 ("dbuser"既定値)、およびデータベース サーバーのファイアウォール規則を指定できます。</span><span class="sxs-lookup"><span data-stu-id="60e03-184">Optional parameters enable you to specify the data center location (defaults to "West US"), database server administrator name (defaults to "dbuser"), and a firewall rule for the database server.</span></span>

### <a name="create-the-web-app"></a><span data-ttu-id="60e03-185">Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="60e03-185">Create the web app</span></span>

<span data-ttu-id="60e03-186">まず、スクリプトでは呼び出すことによって、web アプリを作成、`New-AzureWebsite`内に web アプリの名前と場所パラメーター値を渡すコマンドレット。</span><span class="sxs-lookup"><span data-stu-id="60e03-186">The first thing the script does is create the web app by calling the `New-AzureWebsite` cmdlet, passing in to it the web app name and location parameter values:</span></span>

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a><span data-ttu-id="60e03-187">ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="60e03-187">Create the storage account</span></span>

<span data-ttu-id="60e03-188">メイン スクリプトを実行し、<em>新規 AzureStorage.ps1</em>スクリプトを指定する"<em>&lt;websitename&gt;</em>ストレージ"、ストレージ アカウント名の同じデータ センターの場所として、web アプリです。</span><span class="sxs-lookup"><span data-stu-id="60e03-188">Then the main script runs the <em>New-AzureStorage.ps1</em> script, specifying "<em>&lt;websitename&gt;</em>storage" for the storage account name, and the same data center location as the web app.</span></span>

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

<span data-ttu-id="60e03-189">*新しい AzureStorage.ps1*呼び出し、`New-AzureStorageAccount`ストレージ アカウントを作成するコマンドレットは、アカウント名とアクセス キーの値を返します。</span><span class="sxs-lookup"><span data-stu-id="60e03-189">*New-AzureStorage.ps1* calls the `New-AzureStorageAccount` cmdlet to create the storage account, and it returns the account name and access key values.</span></span> <span data-ttu-id="60e03-190">アプリケーションでは、blob とストレージ アカウント内のキューにアクセスするために、これらの値を必要があります。</span><span class="sxs-lookup"><span data-stu-id="60e03-190">The application will need these values in order to access the blobs and queues in the storage account.</span></span>

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

<span data-ttu-id="60e03-191">常にたくない、新しいストレージ アカウントを作成するには必要に応じて既存のストレージ アカウントを使用するよう指示するパラメーターを追加することにより、スクリプトを強化する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="60e03-191">You might not always want to create a new storage account; you could enhance the script by adding a parameter that optionally directs it to use an existing storage account.</span></span>

### <a name="create-the-databases"></a><span data-ttu-id="60e03-192">データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="60e03-192">Create the databases</span></span>

<span data-ttu-id="60e03-193">メイン スクリプトが、データベース作成スクリプトを実行し、*新規 AzureSql.ps1*、後に既定のデータベースの設定およびファイアウォールのルール名。</span><span class="sxs-lookup"><span data-stu-id="60e03-193">The main script then runs the database creation script, *New-AzureSql.ps1*, after setting up default database and firewall rule names:</span></span>

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

<span data-ttu-id="60e03-194">データベースの作成スクリプトでは、開発用コンピューターの IP アドレスを取得し、開発用コンピューターに接続して、サーバーを管理するためのファイアウォール規則を設定します。</span><span class="sxs-lookup"><span data-stu-id="60e03-194">The database creation script retrieves the dev machine's IP address and sets a firewall rule so the dev machine can connect to and manage the server.</span></span> <span data-ttu-id="60e03-195">データベース作成スクリプトのデータベースを設定するいくつかの手順に進みます。</span><span class="sxs-lookup"><span data-stu-id="60e03-195">The database creation script then goes through several steps to set up the databases:</span></span>

- <span data-ttu-id="60e03-196">使用して、サーバーを作成、`New-AzureSqlDatabaseServer`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="60e03-196">Creates the server by using the `New-AzureSqlDatabaseServer` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- <span data-ttu-id="60e03-197">開発用コンピューターに接続する web アプリを有効にして、サーバーを管理するを有効にするファイアウォール規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="60e03-197">Creates firewall rules to enable the dev machine to manage the server and to enable the web app to connect to it.</span></span> 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- <span data-ttu-id="60e03-198">使用して、サーバー名と資格情報を含むデータベース コンテキストを作成、`New-AzureSqlDatabaseServerContext`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="60e03-198">Creates a database context that includes the server name and credentials, by using the `New-AzureSqlDatabaseServerContext` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    <span data-ttu-id="60e03-199">`New-PSCredentialFromPlainText` 関数を呼び出すスクリプトでは、`ConvertTo-SecureString`返し、パスワードを暗号化するコマンドレット、`PSCredential`オブジェクト、同じ型の型を`Get-Credential`コマンドレットを返します。</span><span class="sxs-lookup"><span data-stu-id="60e03-199">`New-PSCredentialFromPlainText` is a function in the script that calls the `ConvertTo-SecureString` cmdlet to encrypt the password and returns a `PSCredential` object, the same type that the `Get-Credential` cmdlet returns.</span></span>
- <span data-ttu-id="60e03-200">使用して、アプリケーション データベースと、メンバーシップ データベースを作成、`New-AzureSqlDatabase`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="60e03-200">Creates the application database and the membership database by using the `New-AzureSqlDatabase` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- <span data-ttu-id="60e03-201">各データベースのローカルに定義された関数 tocreates 接続文字列を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="60e03-201">Calls a locally defined function tocreates a connection string for each database.</span></span> <span data-ttu-id="60e03-202">アプリケーションは、データベースにアクセスするのにこれらの接続文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="60e03-202">The application will use these connection strings to access the databases.</span></span> 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    <span data-ttu-id="60e03-203">Get SQLAzureDatabaseConnectionString は、渡されたパラメーター値から接続文字列を作成するスクリプトで定義されている関数です。</span><span class="sxs-lookup"><span data-stu-id="60e03-203">Get-SQLAzureDatabaseConnectionString is a function defined in the script that creates the connection string from the parameter values supplied to it.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- <span data-ttu-id="60e03-204">データベース サーバー名と接続文字列のハッシュ テーブルを返します。</span><span class="sxs-lookup"><span data-stu-id="60e03-204">Returns a hash table with the database server name and the connection strings.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

<span data-ttu-id="60e03-205">Fix It アプリは、個別にメンバーシップとアプリケーション データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="60e03-205">The Fix It app uses separate membership and application databases.</span></span> <span data-ttu-id="60e03-206">単一のデータベース内のメンバーシップとアプリケーションの両方のデータを格納することもできます。</span><span class="sxs-lookup"><span data-stu-id="60e03-206">It's also possible to put both membership and application data in a single database.</span></span>

### <a name="store-app-settings-and-connection-strings"></a><span data-ttu-id="60e03-207">ストア アプリの設定と接続文字列</span><span class="sxs-lookup"><span data-stu-id="60e03-207">Store app settings and connection strings</span></span>

<span data-ttu-id="60e03-208">Azure では、機能の設定と接続文字列を読み取ろうとしたときに、アプリケーションに返されるものを自動的に上書きを保存することができます、`appSettings`または`connectionStrings`Web.config ファイル内のコレクション。</span><span class="sxs-lookup"><span data-stu-id="60e03-208">Azure has a feature that enables you to store settings and connection strings that automatically override what is returned to the application when it tries to read the `appSettings` or `connectionStrings` collections in the Web.config file.</span></span> <span data-ttu-id="60e03-209">これは、適用する代替[Web.config 変換](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)を展開する場合。</span><span class="sxs-lookup"><span data-stu-id="60e03-209">This is an alternative to applying [Web.config transformations](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) when you deploy.</span></span> <span data-ttu-id="60e03-210">詳細については、次を参照してください。[機密データを Azure に保存](source-control.md#appsettings)この電子書籍の後半。</span><span class="sxs-lookup"><span data-stu-id="60e03-210">For more information, see [Store sensitive data in Azure](source-control.md#appsettings) later in this e-book.</span></span>

<span data-ttu-id="60e03-211">Azure のすべての環境の作成スクリプトを格納、`appSettings`と`connectionStrings`アプリケーションが Azure での実行時、ストレージ アカウントとデータベースにアクセスする必要がある値。</span><span class="sxs-lookup"><span data-stu-id="60e03-211">The environment creation script stores in Azure all of the `appSettings` and `connectionStrings` values that the application needs to access the storage account and databases when it runs in Azure.</span></span>

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

<span data-ttu-id="60e03-212">[New Relic](http://newrelic.com/)テレメトリ フレームワークで示したです、[監視とテレメトリ](monitoring-and-telemetry.md)」の章。</span><span class="sxs-lookup"><span data-stu-id="60e03-212">[New Relic](http://newrelic.com/) is a telemetry framework that we demonstrate in the [Monitoring and Telemetry](monitoring-and-telemetry.md) chapter.</span></span> <span data-ttu-id="60e03-213">環境の作成スクリプトでは、New Relic の設定を選択するかどうかを確認する web アプリも再起動します。</span><span class="sxs-lookup"><span data-stu-id="60e03-213">The environment creation script also restarts the web app to make sure that it picks up the New Relic settings.</span></span>

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a><span data-ttu-id="60e03-214">展開の準備</span><span class="sxs-lookup"><span data-stu-id="60e03-214">Preparing for deployment</span></span>

<span data-ttu-id="60e03-215">プロセスの最後に、環境の作成スクリプトは、配置スクリプトで使用されるファイルを作成する 2 つの関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="60e03-215">At the end of the process, the environment creation script calls two functions to create files that will be used by the deployment script.</span></span>

<span data-ttu-id="60e03-216">これらの関数のいずれかの発行プロファイルを作成します *(&lt;websitename&gt;.pubxml*ファイル)。</span><span class="sxs-lookup"><span data-stu-id="60e03-216">One of these functions creates a publish profile *(&lt;websitename&gt;.pubxml* file).</span></span> <span data-ttu-id="60e03-217">コードは、発行設定を取得する Azure REST API を呼び出すし、内の情報を保存、 *.publishsettings*ファイル。</span><span class="sxs-lookup"><span data-stu-id="60e03-217">The code calls the Azure REST API to get the publish settings, and it saves the information in a *.publishsettings* file.</span></span> <span data-ttu-id="60e03-218">テンプレート ファイルとそのファイルから情報を使用して (*pubxml.template*) を作成する、 *.pubxml*発行プロファイルを含むファイル。</span><span class="sxs-lookup"><span data-stu-id="60e03-218">Then it uses the information from that file along with a template file (*pubxml.template*) to create the *.pubxml* file that contains the publish profile.</span></span> <span data-ttu-id="60e03-219">この 2 段階のプロセスは、Visual Studio で何をシミュレート: ダウンロード、 *.publishsettings*ファイルし、発行プロファイルを作成するためにインポートします。</span><span class="sxs-lookup"><span data-stu-id="60e03-219">This two-step process simulates what you do in Visual Studio: download a *.publishsettings* file and import that to create a publish profile.</span></span>

<span data-ttu-id="60e03-220">その他の関数では、別のテンプレート ファイル (web サイト environment.template) を使用して、作成、 *web サイト environment.xml*と共に配置スクリプトで使用する設定を含むファイル、 *.pubxml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="60e03-220">The other function uses another template file (website-environment.template) to create a *website-environment.xml* file that contains settings the deployment script will use along with the *.pubxml* file.</span></span>

### <a name="troubleshooting-and-error-handling"></a><span data-ttu-id="60e03-221">トラブルシューティングとエラー処理</span><span class="sxs-lookup"><span data-stu-id="60e03-221">Troubleshooting and error handling</span></span>

<span data-ttu-id="60e03-222">スクリプトは、プログラムと同様に、: 失敗したと、エラーとその原因に関する情報を確認するようにします。</span><span class="sxs-lookup"><span data-stu-id="60e03-222">Scripts are like programs: they can fail, and when they do you want to know as much as you can about the failure and what caused it.</span></span> <span data-ttu-id="60e03-223">このため、環境の作成スクリプトがの値を変更、`VerbosePreference`変数から`SilentlyContinue`に`Continue`すべて詳細メッセージが表示されるようします。</span><span class="sxs-lookup"><span data-stu-id="60e03-223">For this reason, the environment creation script changes the value of the `VerbosePreference` variable from `SilentlyContinue` to `Continue` so that all verbose messages are displayed.</span></span> <span data-ttu-id="60e03-224">値も変更、`ErrorActionPreference`変数から`Continue`に`Stop`でも未終了エラーを検出、スクリプトを停止しますように。</span><span class="sxs-lookup"><span data-stu-id="60e03-224">It also changes the value of the `ErrorActionPreference` variable from `Continue` to `Stop`, so that the script stops even when it encounters non-terminating errors:</span></span>

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

<span data-ttu-id="60e03-225">すべての作業は、前に、スクリプトは、処理が完了したら、経過時間を求めるために、開始時刻を格納します。</span><span class="sxs-lookup"><span data-stu-id="60e03-225">Before it does any work, the script stores the start time so that it can calculate the elapsed time when it's done:</span></span>

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

<span data-ttu-id="60e03-226">その作業を完了した後、スクリプトの経過時間が表示されます。</span><span class="sxs-lookup"><span data-stu-id="60e03-226">After it completes its work, the script displays the elapsed time:</span></span>

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

<span data-ttu-id="60e03-227">すべてのキー操作のスクリプトを書き込みます詳細メッセージは、たとえば。</span><span class="sxs-lookup"><span data-stu-id="60e03-227">And for every key operation the script writes verbose messages, for example:</span></span>

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a><span data-ttu-id="60e03-228">配置スクリプト</span><span class="sxs-lookup"><span data-stu-id="60e03-228">Deployment script</span></span>

<span data-ttu-id="60e03-229">どのような*新規 AzureWebsiteEnv.ps1*環境の作成のスクリプトでは、*発行 AzureWebsite.ps1*スクリプトでは、アプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="60e03-229">What the *New-AzureWebsiteEnv.ps1* script does for environment creation, the *Publish-AzureWebsite.ps1* script does for application deployment.</span></span>

<span data-ttu-id="60e03-230">配置スクリプトから web アプリの名前を取得する、 *web サイト environment.xml*環境の作成スクリプトによって作成されたファイル。</span><span class="sxs-lookup"><span data-stu-id="60e03-230">The deployment script gets the name of the web app from the *website-environment.xml* file created by the environment creation script.</span></span>

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

<span data-ttu-id="60e03-231">デプロイ ユーザー パスワードを取得、 *.publishsettings*ファイル。</span><span class="sxs-lookup"><span data-stu-id="60e03-231">It gets the deployment user password from the *.publishsettings* file:</span></span>

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

<span data-ttu-id="60e03-232">実行、 [MSBuild](http://msbuildbook.com/)構築し、プロジェクトを展開するコマンド。</span><span class="sxs-lookup"><span data-stu-id="60e03-232">It executes the [MSBuild](http://msbuildbook.com/) command that builds and deploys the project:</span></span>

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

<span data-ttu-id="60e03-233">指定した場合と、`Launch`呼び出し、コマンドラインでパラメーター、`Show-AzureWebsite`コマンドレットを web サイトの URL を既定のブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="60e03-233">And if you've specified the `Launch` parameter on the command line, it calls the `Show-AzureWebsite` cmdlet to open your default browser to the website URL.</span></span>

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

<span data-ttu-id="60e03-234">次のようなコマンドを使用して、配置スクリプトを実行できます。</span><span class="sxs-lookup"><span data-stu-id="60e03-234">You can run the deployment script with a command like this one:</span></span>

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

<span data-ttu-id="60e03-235">ブラウザーが開き、サイトでは、クラウドで実行されているが完了したら、 `<websitename>.azurewebsites.net` URL。</span><span class="sxs-lookup"><span data-stu-id="60e03-235">And when it's done, the browser opens with the site running in the cloud at the `<websitename>.azurewebsites.net` URL.</span></span>

![Windows Azure にデプロイされたアプリを修正します。](automate-everything/_static/image7.png)

## <a name="summary"></a><span data-ttu-id="60e03-237">まとめ</span><span class="sxs-lookup"><span data-stu-id="60e03-237">Summary</span></span>

<span data-ttu-id="60e03-238">これらのスクリプトを同じ手順が常に同じオプションを使用して、同じ順序で実行することを確信できます。</span><span class="sxs-lookup"><span data-stu-id="60e03-238">With these scripts you can be confident that the same steps will always be executed in the same order using the same options.</span></span> <span data-ttu-id="60e03-239">こうことを確認、チームの各開発者がどうしても見落としまたはめちゃくちゃに何か別のチーム メンバーの環境または運用環境で同様に機能しません実際に自分のマシンで独自のものをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="60e03-239">This helps ensure that each developer on the team doesn't miss something or mess something up or deploy something custom on his own machine that won't actually work the same way in another team member's environment or in production.</span></span>

<span data-ttu-id="60e03-240">同様の方法では、REST API、Windows PowerShell スクリプト、.NET 言語 API、または Linux またはファルダで実行できる Bash ユーティリティを使用して、管理ポータルで実行できるほとんどの Azure の管理機能を自動化できます。</span><span class="sxs-lookup"><span data-stu-id="60e03-240">In a similar way, you can automate most Azure management functions that you can do in the management portal, by using the REST API, Windows PowerShell scripts, a .NET language API, or a Bash utility that you can run on Linux or Mac.</span></span>

<span data-ttu-id="60e03-241">[[次へ] の章](source-control.md)をソース コードを確認し、ソース コード リポジトリで、スクリプトを含める重要な理由について説明します。</span><span class="sxs-lookup"><span data-stu-id="60e03-241">In the [next chapter](source-control.md) we'll look at source code and explain why it's important to include your scripts in your source code repository.</span></span>

## <a name="resources"></a><span data-ttu-id="60e03-242">リソース</span><span class="sxs-lookup"><span data-stu-id="60e03-242">Resources</span></span>

- <span data-ttu-id="60e03-243">[インストールし、Azure の Windows PowerShell を構成](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)します。</span><span class="sxs-lookup"><span data-stu-id="60e03-243">[Install and Configure Windows PowerShell for Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).</span></span> <span data-ttu-id="60e03-244">Azure PowerShell コマンドレットをインストールする方法も考慮に入れて必要がある、Azure を管理するためにコンピューターに証明書をインストールする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="60e03-244">Explains how to install the Azure PowerShell cmdlets and how to install the certificate that you need on your computer in order to manage your Azure account.</span></span> <span data-ttu-id="60e03-245">これは、PowerShell 自体を学習するためのリソースへのリンクもあるために開始する最適な場所です。</span><span class="sxs-lookup"><span data-stu-id="60e03-245">This is a great place to get started because it also has links to resources for learning PowerShell itself.</span></span>
- <span data-ttu-id="60e03-246">[Azure スクリプト センター](https://docs.microsoft.com/azure/automation/automation-runbook-gallery)します。</span><span class="sxs-lookup"><span data-stu-id="60e03-246">[Azure Script Center](https://docs.microsoft.com/azure/automation/automation-runbook-gallery).</span></span> <span data-ttu-id="60e03-247">チュートリアル、コマンドレットのリファレンス ドキュメントとソース コード、およびサンプル スクリプトへのリンクでの Azure サービスを管理するスクリプトを開発するためのリソースへの WindowsAzure.com ポータル</span><span class="sxs-lookup"><span data-stu-id="60e03-247">WindowsAzure.com portal to resources for developing scripts that manage Azure services, with links to getting started tutorials, cmdlet reference documentation and source code, and sample scripts</span></span>
- <span data-ttu-id="60e03-248">[週末 Scripter: Azure と PowerShell の開始](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="60e03-248">[Weekend Scripter: Getting Started with Azure and PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx).</span></span> <span data-ttu-id="60e03-249">Windows PowerShell に専用のブログでは、この投稿は、PowerShell を使用して、Azure の管理機能の導入として優れていますを提供します。</span><span class="sxs-lookup"><span data-stu-id="60e03-249">In a blog dedicated to Windows PowerShell, this post provides a great introduction to using PowerShell for Azure management functions.</span></span>
- <span data-ttu-id="60e03-250">[インストールし、構成、Azure クロス プラットフォーム コマンド ライン インターフェイス](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)します。</span><span class="sxs-lookup"><span data-stu-id="60e03-250">[Install and Configure the Azure Cross-Platform Command-Line Interface](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).</span></span> <span data-ttu-id="60e03-251">システムを Windows だけでなく、Mac および Linux とで動作する Azure のスクリプティング フレームワークの入門チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="60e03-251">Getting-started tutorial for an Azure scripting framework that works on Mac and Linux as well as Windows systems.</span></span>
- <span data-ttu-id="60e03-252">[Azure Sdk のダウンロードとツール」のコマンド ライン ツールの「](https://azure.microsoft.com/downloads/)します。</span><span class="sxs-lookup"><span data-stu-id="60e03-252">[Command-line tools section of the Download Azure SDKs and Tools topic](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="60e03-253">ドキュメントと azure コマンド ライン ツールに関連するダウンロード ポータル ページです。</span><span class="sxs-lookup"><span data-stu-id="60e03-253">Portal page for documentation and downloads related to command-line tools for Azure.</span></span>
- <span data-ttu-id="60e03-254">[すべてを Azure 管理ライブラリと .NET を使用した自動化](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="60e03-254">[Automating everything with the Azure Management Libraries and .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx).</span></span> <span data-ttu-id="60e03-255">Scott Hanselman は、Azure 用の .NET 管理 API が導入されています。</span><span class="sxs-lookup"><span data-stu-id="60e03-255">Scott Hanselman introduces the .NET management API for Azure.</span></span>
- <span data-ttu-id="60e03-256">[Windows PowerShell スクリプトを使用して、開発環境およびテスト環境に発行する](https://msdn.microsoft.com/library/azure/dn642480.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="60e03-256">[Using Windows PowerShell Scripts to Publish to Dev and Test Environments](https://msdn.microsoft.com/library/azure/dn642480.aspx).</span></span> <span data-ttu-id="60e03-257">使用する方法を説明する MSDN ドキュメントでは、web プロジェクトの Visual Studio が自動的に生成するスクリプトを発行します。</span><span class="sxs-lookup"><span data-stu-id="60e03-257">MSDN documentation that explains how to use publish scripts that Visual Studio automatically generates for web projects.</span></span>
- <span data-ttu-id="60e03-258">[PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597)します。</span><span class="sxs-lookup"><span data-stu-id="60e03-258">[PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597).</span></span> <span data-ttu-id="60e03-259">Visual Studio 拡張機能を Visual Studio での Windows PowerShell 言語サポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="60e03-259">Visual Studio extension that adds language support for Windows PowerShell in Visual Studio.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="60e03-260">[前へ](introduction.md)
> [次へ](source-control.md)</span><span class="sxs-lookup"><span data-stu-id="60e03-260">[Previous](introduction.md)
[Next](source-control.md)</span></span>
