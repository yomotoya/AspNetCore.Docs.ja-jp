---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Azure での実際のクラウド アプリの構築 |Microsoft ドキュメント
author: MikeWasson
description: この電子書籍を紹介パターン ベースのアプローチを実際のクラウド ソリューションを構築します。 パターンを適用するために、開発プロセスもとして、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 5a62818a2dc21128bb0a42a8b296ade460e7b060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870532"
---
<a name="building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="86390-104">Azure での実際のクラウド アプリの構築</span><span class="sxs-lookup"><span data-stu-id="86390-104">Building Real-World Cloud Apps with Azure</span></span>
====================
<span data-ttu-id="86390-105">によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="86390-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="86390-106">[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="86390-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="86390-107">この電子書籍を紹介パターン ベースのアプローチを実際のクラウド ソリューションを構築します。</span><span class="sxs-lookup"><span data-stu-id="86390-107">This e-book walks you through a patterns-based approach to building real-world cloud solutions.</span></span> <span data-ttu-id="86390-108">パターンは、アーキテクチャとコーディング方法だけでなく、開発プロセスに適用されます。</span><span class="sxs-lookup"><span data-stu-id="86390-108">The patterns apply to the development process as well as to architecture and coding practices.</span></span>
> 
> <span data-ttu-id="86390-109">Scott guthrie で開発および提供したノルウェー語開発者会議 (NDC) に 2013 年 6 月のプレゼンテーションに基づいて、コンテンツ ([パート 1](http://vimeo.com/68215538)、[パート 2](http://vimeo.com/68215602))、および Microsoft テクニカル Ed オーストラリアで2013 年 9 月、([パート 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324)、[パート 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325))。</span><span class="sxs-lookup"><span data-stu-id="86390-109">The content is based on a presentation developed by Scott Guthrie and delivered by him at the Norwegian Developers Conference (NDC) in June of 2013 ([part 1](http://vimeo.com/68215538), [part 2](http://vimeo.com/68215602)), and at Microsoft Tech Ed Australia in September, 2013 ([part 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [part 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)).</span></span> <span data-ttu-id="86390-110">[その他の多く](more-patterns-and-guidance.md#acknowledgments)が更新され、書き込まれたフォームへのビデオから移行中に、コンテンツを補完します。</span><span class="sxs-lookup"><span data-stu-id="86390-110">[Many others](more-patterns-and-guidance.md#acknowledgments) updated and augmented the content while transitioning it from video to written form.</span></span>


## <a name="intended-audience"></a><span data-ttu-id="86390-111">想定読者</span><span class="sxs-lookup"><span data-stu-id="86390-111">Intended Audience</span></span>

<span data-ttu-id="86390-112">クラウドへの移行を検討して、クラウド向けの開発に関する調べたいまたはクラウド開発に慣れていない開発者は、ここで、最も重要な概念とプラクティスを理解する必要があるの簡潔な概要で検出されます。</span><span class="sxs-lookup"><span data-stu-id="86390-112">Developers who are curious about developing for the cloud, considering a move to the cloud, or are new to cloud development will find here a concise overview of the most important concepts and practices they need to know.</span></span> <span data-ttu-id="86390-113">具体的な例については、各章にリンクの詳細については、その他のリソースと概念を示します。</span><span class="sxs-lookup"><span data-stu-id="86390-113">The concepts are illustrated with concrete examples, and each chapter links to other resources for more in-depth information.</span></span> <span data-ttu-id="86390-114">例では、およびその他のリソースへのリンクは、Microsoft のフレームワークとサービスが示す原則は他の web 開発フレームワークに適用され、クラウド環境にもします。</span><span class="sxs-lookup"><span data-stu-id="86390-114">The examples and the links to additional resources are for Microsoft frameworks and services, but the principles illustrated apply to other web development frameworks and cloud environments as well.</span></span>

<span data-ttu-id="86390-115">アイデアは、ここに役立つように成功には、既にクラウド向けに開発する開発者があります。</span><span class="sxs-lookup"><span data-stu-id="86390-115">Developers who are already developing for the cloud may find ideas here that will help make them more successful.</span></span> <span data-ttu-id="86390-116">系列の各章読み取れるされません個別に選択およびで関心のあるトピックを選択できるようです。</span><span class="sxs-lookup"><span data-stu-id="86390-116">Each chapter in the series can be read independently, so you can pick and choose topics that you're interested in.</span></span>

<span data-ttu-id="86390-117">Scott Guthrie のマークされている人*現実世界クラウド アプリのビルドと Azure*プレゼンテーションの詳細および最新の情報が見つかりますをここで希望しています。</span><span class="sxs-lookup"><span data-stu-id="86390-117">Anyone who watched Scott Guthrie's *Building Real World Cloud Apps with Azure* presentation and wants more details and updated information will find that here.</span></span>

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a><span data-ttu-id="86390-118">クラウド開発パターン</span><span class="sxs-lookup"><span data-stu-id="86390-118">Cloud development patterns</span></span>

<span data-ttu-id="86390-119">この電子書籍では、13 個の推奨クラウド開発するためのパターンについて説明します。</span><span class="sxs-lookup"><span data-stu-id="86390-119">This e-book explains thirteen recommended patterns for cloud development.</span></span> <span data-ttu-id="86390-120">「パターン」を広くするという意味での作業を行うことをお勧めはここで使用します。 には、開発、設計、およびクラウド アプリをコーディングする最適な方法です。</span><span class="sxs-lookup"><span data-stu-id="86390-120">"Pattern" is used here in a broad sense to mean a recommended way to do things: how best to go about developing, designing, and coding cloud apps.</span></span> <span data-ttu-id="86390-121">これらは、それらの操作を行う場合に役立てること「の成功度の pit に分類」が主要なパターンです。</span><span class="sxs-lookup"><span data-stu-id="86390-121">These are key patterns which will help you "fall into the pit of success" if you follow them.</span></span>

- <span data-ttu-id="86390-122">[すべてを自動化](automate-everything.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-122">[Automate everything](automate-everything.md).</span></span>

    - <span data-ttu-id="86390-123">スクリプトを使用して、効率を最大化し、反復的なプロセスでエラーを最小限に抑えます。</span><span class="sxs-lookup"><span data-stu-id="86390-123">Use scripts to maximize efficiency and minimize errors in repetitive processes.</span></span>
    - <span data-ttu-id="86390-124">チュートリアル: Azure 管理スクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="86390-124">Demo: Azure management scripts.</span></span>
- <span data-ttu-id="86390-125">[ソース管理](source-control.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-125">[Source control](source-control.md).</span></span> 

    - <span data-ttu-id="86390-126">DevOps ワークフローを容易にするために、ソース管理での分岐構造を設定します。</span><span class="sxs-lookup"><span data-stu-id="86390-126">Set up branching structure in source control to facilitate DevOps workflow.</span></span>
    - <span data-ttu-id="86390-127">デモ: は、スクリプトをソース管理に追加します。</span><span class="sxs-lookup"><span data-stu-id="86390-127">Demo: add scripts to source control.</span></span>
    - <span data-ttu-id="86390-128">デモ: は、ソース管理からの機密データを保持します。</span><span class="sxs-lookup"><span data-stu-id="86390-128">Demo: keep sensitive data out of source control.</span></span>
    - <span data-ttu-id="86390-129">デモ: は、Visual Studio での Git を使用します。</span><span class="sxs-lookup"><span data-stu-id="86390-129">Demo: use Git in Visual Studio.</span></span>
- <span data-ttu-id="86390-130">[継続的インテグレーションと配信](continuous-integration-and-continuous-delivery.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-130">[Continuous integration and delivery](continuous-integration-and-continuous-delivery.md).</span></span> 

    - <span data-ttu-id="86390-131">ビルドと展開を各ソース制御チェックで自動化できます。</span><span class="sxs-lookup"><span data-stu-id="86390-131">Automate build and deployment with each source control check-in.</span></span>
- <span data-ttu-id="86390-132">[Web 開発のベスト プラクティス](web-development-best-practices.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-132">[Web development best practices](web-development-best-practices.md).</span></span> 

    - <span data-ttu-id="86390-133">ステートレス web 層を保持します。</span><span class="sxs-lookup"><span data-stu-id="86390-133">Keep web tier stateless.</span></span>
    - <span data-ttu-id="86390-134">デモ: はスケーリングと Azure App service Web アプリでの自動スケーリングします。</span><span class="sxs-lookup"><span data-stu-id="86390-134">Demo: scaling and auto-scaling in Web Apps in Azure App Service.</span></span>
    - <span data-ttu-id="86390-135">セッション状態を回避します。</span><span class="sxs-lookup"><span data-stu-id="86390-135">Avoid session state.</span></span>
    - <span data-ttu-id="86390-136">CDN が利用できない場合は、フォールバック CDN を使用します。</span><span class="sxs-lookup"><span data-stu-id="86390-136">Use a CDN with a fallback when the CDN is unavailable.</span></span>
    - <span data-ttu-id="86390-137">非同期プログラミング モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="86390-137">Use asynchronous programming model.</span></span>
    - <span data-ttu-id="86390-138">ASP.NET MVC および Entity Framework でのデモ: 非同期です。</span><span class="sxs-lookup"><span data-stu-id="86390-138">Demo: async in ASP.NET MVC and Entity Framework.</span></span>
- <span data-ttu-id="86390-139">[シングル サインオン](single-sign-on.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-139">[Single sign-on](single-sign-on.md).</span></span> 

    - <span data-ttu-id="86390-140">Azure Active Directory の概要です。</span><span class="sxs-lookup"><span data-stu-id="86390-140">Introduction to Azure Active Directory.</span></span>
    - <span data-ttu-id="86390-141">デモ: は、Azure Active Directory を使用する ASP.NET アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="86390-141">Demo: create an ASP.NET app that uses Azure Active Directory.</span></span>
- <span data-ttu-id="86390-142">[データ記憶域オプション](data-storage-options.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-142">[Data storage options](data-storage-options.md).</span></span> 

    - <span data-ttu-id="86390-143">データ ストアの種類です。</span><span class="sxs-lookup"><span data-stu-id="86390-143">Types of data stores.</span></span>
    - <span data-ttu-id="86390-144">適切なデータ ストアを選択する方法。</span><span class="sxs-lookup"><span data-stu-id="86390-144">How to choose the right data store.</span></span>
    - <span data-ttu-id="86390-145">チュートリアル: Azure SQL データベースです。</span><span class="sxs-lookup"><span data-stu-id="86390-145">Demo: Azure SQL Database.</span></span>
- <span data-ttu-id="86390-146">[データのパーティション分割戦略](data-partitioning-strategies.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-146">[Data partitioning strategies](data-partitioning-strategies.md).</span></span> 

    - <span data-ttu-id="86390-147">垂直方向に、行方向にデータをパーティション分割またはその両方がリレーショナル データベースのスケーリングを容易になります。</span><span class="sxs-lookup"><span data-stu-id="86390-147">Partition data vertically, horizontally, or both to facilitate scaling a relational database.</span></span>
- <span data-ttu-id="86390-148">[非構造化 blob ストレージ](unstructured-blob-storage.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-148">[Unstructured blob storage](unstructured-blob-storage.md).</span></span> 

    - <span data-ttu-id="86390-149">Blob サービスを使用してクラウドにファイルを格納します。</span><span class="sxs-lookup"><span data-stu-id="86390-149">Store files in the cloud by using the blob service.</span></span>
    - <span data-ttu-id="86390-150">デモ: blob ストレージ アプリを使用してください。</span><span class="sxs-lookup"><span data-stu-id="86390-150">Demo: using blob storage in the Fix It app.</span></span>
- <span data-ttu-id="86390-151">[障害にも対処デザイン](design-to-survive-failures.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-151">[Design to survive failures](design-to-survive-failures.md).</span></span> 

    - <span data-ttu-id="86390-152">失敗の種類。</span><span class="sxs-lookup"><span data-stu-id="86390-152">Types of failures.</span></span>
    - <span data-ttu-id="86390-153">障害のスコープです。</span><span class="sxs-lookup"><span data-stu-id="86390-153">Failure Scope.</span></span>
    - <span data-ttu-id="86390-154">Understanding Sla です。</span><span class="sxs-lookup"><span data-stu-id="86390-154">Understanding SLAs.</span></span>
- <span data-ttu-id="86390-155">[監視と遠隔測定](monitoring-and-telemetry.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-155">[Monitoring and telemetry](monitoring-and-telemetry.md).</span></span> 

    - <span data-ttu-id="86390-156">理由テレメトリ アプリを購入両方と、アプリをインストルメント化するコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="86390-156">Why you should both buy a telemetry app and write your own code to instrument your app.</span></span>
    - <span data-ttu-id="86390-157">Azure の New Relic のデモ:</span><span class="sxs-lookup"><span data-stu-id="86390-157">Demo: New Relic for Azure</span></span>
    - <span data-ttu-id="86390-158">デモ: は、修正、アプリにコードを記録します。</span><span class="sxs-lookup"><span data-stu-id="86390-158">Demo: logging code in the Fix It app.</span></span>
    - <span data-ttu-id="86390-159">修正アプリでのデモ: 依存関係の挿入します。</span><span class="sxs-lookup"><span data-stu-id="86390-159">Demo: dependency injection in the Fix It app.</span></span>
    - <span data-ttu-id="86390-160">Azure でのデモ: 組み込みのログ記録のサポート。</span><span class="sxs-lookup"><span data-stu-id="86390-160">Demo: built-in logging support in Azure.</span></span>
- <span data-ttu-id="86390-161">[一時的なエラー処理](transient-fault-handling.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-161">[Transient fault handling](transient-fault-handling.md).</span></span> 

    - <span data-ttu-id="86390-162">一時的な障害の影響を軽減するためにスマート再試行/バックオフ ロジックを使用します。</span><span class="sxs-lookup"><span data-stu-id="86390-162">Use smart retry/back-off logic to mitigate the effect of transient failures.</span></span>
    - <span data-ttu-id="86390-163">デモ: 再試行/バックオフ Entity Framework 6 でします。</span><span class="sxs-lookup"><span data-stu-id="86390-163">Demo: retry/back-off in Entity Framework 6.</span></span>
- <span data-ttu-id="86390-164">[分散キャッシュ](distributed-caching.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-164">[Distributed caching](distributed-caching.md).</span></span> 

    - <span data-ttu-id="86390-165">スケーラビリティが向上し、分散キャッシュを使用してデータベースのトランザクション コストを削減します。</span><span class="sxs-lookup"><span data-stu-id="86390-165">Improve scalability and reduce database transaction costs by using distributed caching.</span></span>
- <span data-ttu-id="86390-166">[キューを中心とした作業パターン](queue-centric-work-pattern.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-166">[Queue-centric work pattern](queue-centric-work-pattern.md).</span></span> 

    - <span data-ttu-id="86390-167">高可用性を有効にして、web およびワーカー層を疎結合、スケーラビリティが向上します。</span><span class="sxs-lookup"><span data-stu-id="86390-167">Enable high availability and improve scalability by loosely coupling web and worker tiers.</span></span>
    - <span data-ttu-id="86390-168">修正、アプリのデモ: Azure ストレージ キュー。</span><span class="sxs-lookup"><span data-stu-id="86390-168">Demo: Azure storage queues in the Fix It app.</span></span>
- <span data-ttu-id="86390-169">[他のクラウド アプリのパターンとガイダンス](more-patterns-and-guidance.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-169">[More cloud app patterns and guidance](more-patterns-and-guidance.md).</span></span>
- [<span data-ttu-id="86390-170">付録: Fix It サンプル アプリケーション</span><span class="sxs-lookup"><span data-stu-id="86390-170">Appendix: The Fix It Sample Application</span></span>](the-fix-it-sample-application.md)

    - <span data-ttu-id="86390-171">既知の問題</span><span class="sxs-lookup"><span data-stu-id="86390-171">Known Issues</span></span>
    - <span data-ttu-id="86390-172">ベスト プラクティス</span><span class="sxs-lookup"><span data-stu-id="86390-172">Best Practices</span></span>
    - <span data-ttu-id="86390-173">ダウンロード、ビルド、実行、および配置する方法。</span><span class="sxs-lookup"><span data-stu-id="86390-173">How to download, build, run, and deploy.</span></span>

<span data-ttu-id="86390-174">すべてのクラウド環境にこれらのパターンを適用しますが、について Microsoft のテクノロジおよび Visual Studio、Team Foundation Service、ASP.NET、および Azure などのサービスに基づく例を使用して説明します。</span><span class="sxs-lookup"><span data-stu-id="86390-174">These patterns apply to all cloud environments, but we'll illustrate them by using examples based on Microsoft technologies and services, such as Visual Studio, Team Foundation Service, ASP.NET, and Azure.</span></span>

<span data-ttu-id="86390-175">この章の残りの部分には、修正、サンプル アプリケーションとで、修正、アプリが実行されているクラウド環境を Azure App Service の Web アプリが導入されています。</span><span class="sxs-lookup"><span data-stu-id="86390-175">This remainder of this chapter introduces the Fix It sample application and the Web Apps in Azure App Service cloud environment that the Fix It app runs in.</span></span>

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a><span data-ttu-id="86390-176">これはサンプル アプリケーションの修正プログラム</span><span class="sxs-lookup"><span data-stu-id="86390-176">The Fix it sample application</span></span>

<span data-ttu-id="86390-177">スクリーン ショットとこの電子書籍に示すようにコード例のほとんどは、修正、アプリによって開発されたに基づきます[Scott Guthrie](https://weblogs.asp.net/scottgu/)に推奨されるクラウド アプリの開発パターンとプラクティスを紹介します。</span><span class="sxs-lookup"><span data-stu-id="86390-177">Most of the screen shots and code examples shown in this e-book are based on the Fix It app originally developed by [Scott Guthrie](https://weblogs.asp.net/scottgu/) to demonstrate recommended cloud app development patterns and practices.</span></span>

![アプリのホーム ページを修正します。](introduction/_static/image1.png)

<span data-ttu-id="86390-179">サンプル アプリは、チケット システム簡単な作業項目です。</span><span class="sxs-lookup"><span data-stu-id="86390-179">The sample app is a simple work item ticketing system.</span></span> <span data-ttu-id="86390-180">問題を解決する場合、作成、チケットと割り当て、他のユーザーと他のユーザーにログインして、割り当てられているチケットを参照してくださいそれらにしてチケットを作業が行われるときに完了としてマークします。</span><span class="sxs-lookup"><span data-stu-id="86390-180">When you need something fixed, you create a ticket and assign it to someone, and others can log in and see the tickets assigned to them and mark tickets as completed when the work is done.</span></span>

<span data-ttu-id="86390-181">これは、標準の Visual Studio web プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="86390-181">It's a standard Visual Studio web project.</span></span> <span data-ttu-id="86390-182">ASP.NET MVC 上でビルドされ、SQL Server データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="86390-182">It is built on ASP.NET MVC and uses a SQL Server database.</span></span> <span data-ttu-id="86390-183">IIS Express でローカルに実行できるし、クラウドで実行する Azure Web サイトに展開できます。</span><span class="sxs-lookup"><span data-stu-id="86390-183">It can run locally in IIS Express and can be deployed to a Azure Web Site to run in the cloud.</span></span> <span data-ttu-id="86390-184">フォーム認証と、ローカル データベースを使用して、または Google などのソーシャル プロバイダーを使用してログインすることができます。</span><span class="sxs-lookup"><span data-stu-id="86390-184">You can log in using forms authentication and a local database or by using a social provider such as Google.</span></span> <span data-ttu-id="86390-185">(後でも紹介 Active Directory の組織アカウントでログインする方法です。)</span><span class="sxs-lookup"><span data-stu-id="86390-185">(Later we'll also show how to log in with an Active Directory organizational account.)</span></span>

![ログイン ページ](introduction/_static/image2.png)

<span data-ttu-id="86390-187">ログインしている後にチケットを作成、だれかに割り当てるして固定を取得する対象の画像をアップロードします。</span><span class="sxs-lookup"><span data-stu-id="86390-187">Once you're logged in you can create a ticket, assign it to someone, and upload a picture of what you want to get fixed.</span></span>

![修正タスクを作成します。](introduction/_static/image3.png)

![作成されたタスクを修正します。](introduction/_static/image4.png)

<span data-ttu-id="86390-190">作成した作業項目の進行状況を追跡できます、完了済みとして、チケットの詳細の表示、および記号の項目に割り当てられているチケットを参照してください。</span><span class="sxs-lookup"><span data-stu-id="86390-190">You can track the progress of work items you created, see tickets assigned to you, view ticket details, and mark items as completed.</span></span>

<span data-ttu-id="86390-191">これは、機能の観点から非常にシンプルなアプリが数百万のユーザーに拡張できるし、回復力のあるデータベースの障害と接続の終了などになりますできるようにそれをビルドする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="86390-191">This is a very simple app from a feature perspective, but you'll see how to build it so that it can scale to millions of users and will be resilient to things like database failures and connection terminations.</span></span> <span data-ttu-id="86390-192">単純なを起動し、アプリをより迅速かつ効率的に開発サイクルの反復処理するようにすることにより、自動とアジャイル開発のワークフローを作成する方法も表示されます。</span><span class="sxs-lookup"><span data-stu-id="86390-192">You'll also see how to create an automated and agile development workflow, which enables you to start simple and make the app better and better by iterating the development cycle efficiently and quickly.</span></span>

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a><span data-ttu-id="86390-193">Azure App Service の web アプリ</span><span class="sxs-lookup"><span data-stu-id="86390-193">Web Apps in Azure App Service</span></span>

<span data-ttu-id="86390-194">修正アプリケーション用に使用されるクラウド環境は、Web サイトと呼ばれる Azure のサービスです。</span><span class="sxs-lookup"><span data-stu-id="86390-194">The cloud environment used for the Fix It application is a service of Azure that we call Web Sites.</span></span> <span data-ttu-id="86390-195">このサービスは、ことができ、更新しておきます。 Vm を作成しなくても Azure で web アプリをホストする、インストールをなど、IIS を構成する方法です。お、Vm では、サイトをホストして、バックアップと回復、およびその他のサービスを自動的に提供します。</span><span class="sxs-lookup"><span data-stu-id="86390-195">This service is a way that you can host your own web app in Azure without having to create VMs and keep them updated, install and configure IIS, etc. We host your site on our VMs and automatically provide backup and recovery and other services for you.</span></span> <span data-ttu-id="86390-196">Web サイト サービスは、ASP.NET、Node.js、PHP、および Python で動作します。</span><span class="sxs-lookup"><span data-stu-id="86390-196">The Web Sites service works with ASP.NET, Node.js, PHP, and Python.</span></span> <span data-ttu-id="86390-197">Visual Studio、Web Deploy、FTP、Git または TFS を使用した非常に迅速に展開することができます。</span><span class="sxs-lookup"><span data-stu-id="86390-197">It enables you to deploy very quickly using Visual Studio, Web Deploy, FTP, Git, or TFS.</span></span> <span data-ttu-id="86390-198">一般的には、インターネット経由で、更新プログラムが利用可能な時間と、デプロイを開始する時間の間はわずか数秒です。</span><span class="sxs-lookup"><span data-stu-id="86390-198">It's usually just a few seconds between the time you start a deployment and the time your update is available over the Internet.</span></span> <span data-ttu-id="86390-199">開始するにはすべて無料ですし、トラフィックの増大に応じてスケール アップできます。</span><span class="sxs-lookup"><span data-stu-id="86390-199">It's all free to get started, and you can scale up as your traffic grows.</span></span>

<span data-ttu-id="86390-200">バック グラウンドでは、Azure App service Web Apps は、多くの場合は、独自の Vm 上で IIS を使用して web サイトをホストする予定がご自身でビルドする必要がありますのアーキテクチャのコンポーネントと機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="86390-200">Behind the scenes, Web Apps in Azure App Service provides a lot of architectural components and features that you'd have to build yourself if you were going to host a web site using IIS on your own VMs.</span></span> <span data-ttu-id="86390-201">1 つのコンポーネントは、自動的に IIS を構成し、多くの Vm では、サイトを実行するように、アプリケーションをインストールする展開のエンド ポイントです。</span><span class="sxs-lookup"><span data-stu-id="86390-201">One component is a deployment end point that automatically configures IIS and installs your application on as many VMs as you want to run your site on.</span></span>

![展開サービス](introduction/_static/image5.png)

<span data-ttu-id="86390-203">ユーザーが web サイトに達すると IIS Vm に直接ヒットがない、を通じて[Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing)ロード バランサーです。</span><span class="sxs-lookup"><span data-stu-id="86390-203">When a user hits the web site, they don't hit the IIS VMs directly, they go through [Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) load balancers.</span></span> <span data-ttu-id="86390-204">独自のサーバーでこれらを使用できますが、利点は、こと、セットアップが完了するために自動的にします。</span><span class="sxs-lookup"><span data-stu-id="86390-204">You can use these with your own servers, but the advantage here is that they're set up for you automatically.</span></span> <span data-ttu-id="86390-205">スマート ヒューリスティックはセッション アフィニティ、IIS では、キューの深さなどの要因を考慮を使用し、ごとの CPU 使用率マシンから Vm へのトラフィックを直接ホストする web サイトです。</span><span class="sxs-lookup"><span data-stu-id="86390-205">They use a smart heuristic that takes into account factors such as session affinity, queue depth in IIS, and CPU usage on each machine to direct traffic to the VMs that host your web site.</span></span>

![ARR のロード バランサー](introduction/_static/image6.png)

<span data-ttu-id="86390-207">場合は、マシンがダウンしても、Azure に自動的にローテーションから取り出す、スピンアップする新しい VM インスタンスでは、か、--すべてアプリケーションのダウンタイムなしの新しいインスタンスにトラフィックの送信を開始します。</span><span class="sxs-lookup"><span data-stu-id="86390-207">If a machine goes down, Azure automatically pulls it from the rotation, spins up a new VM instance, and starts directing traffic to the new instance -- all with no down time for your application.</span></span>

![コンピューターの障害からの自動復旧](introduction/_static/image7.png)

<span data-ttu-id="86390-209">これはすべて自動的に行わします。</span><span class="sxs-lookup"><span data-stu-id="86390-209">All of this takes place automatically.</span></span> <span data-ttu-id="86390-210">行うには必要なは、web サイトを作成し、Windows PowerShell、Visual Studio または Azure 管理ポータルを使用して、アプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="86390-210">All you need to do is create a web site and deploy your application to it, using Windows PowerShell, Visual Studio, or the Azure management portal.</span></span>

<span data-ttu-id="86390-211">すばやくかつ簡単なチュートリアルは Visual Studio で web アプリケーションを作成し、Azure Web サイトを展開する方法を示す、次を参照してください。 [Azure と ASP.NET の概要](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。</span><span class="sxs-lookup"><span data-stu-id="86390-211">For a quick and easy step-by-step tutorial that shows how to create a web application in Visual Studio and deploy it to a Azure Web Site, see [Get started with Azure and ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<a id="summary"></a>
## <a name="summary"></a><span data-ttu-id="86390-212">まとめ</span><span class="sxs-lookup"><span data-stu-id="86390-212">Summary</span></span>

<span data-ttu-id="86390-213">この概要は、書籍は説明トピックでは、サンプル アプリケーションのスクリーン ショットと Azure App Service のクラウド環境で Web アプリの概要の一覧を提供しています。</span><span class="sxs-lookup"><span data-stu-id="86390-213">This introduction has provided a list of topics the book will cover, screenshots of the sample application, and a brief overview of the Web Apps in Azure App Service cloud environment.</span></span> <span data-ttu-id="86390-214">クラウドのアプリの開発の利点としてのいずれかとは簡単にテスト環境を作成し、コードを展開するなどの反復的な開発タスクを自動化します。</span><span class="sxs-lookup"><span data-stu-id="86390-214">One of the great advantages of developing apps in and for the cloud is that it's easy to automate repetitive development tasks such as creating a test environment and deploying your code to it.</span></span> <span data-ttu-id="86390-215">サブジェクトで実行されている方法、[次のチャプター](automate-everything.md)です。</span><span class="sxs-lookup"><span data-stu-id="86390-215">How to do that is the subject of the [next chapter](automate-everything.md).</span></span>

## <a name="resources"></a><span data-ttu-id="86390-216">リソース</span><span class="sxs-lookup"><span data-stu-id="86390-216">Resources</span></span>

<span data-ttu-id="86390-217">この章で説明したトピックの詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="86390-217">For more information about the topics covered in this chapter, see the following resources.</span></span>

<span data-ttu-id="86390-218">ドキュメント:</span><span class="sxs-lookup"><span data-stu-id="86390-218">Documentation:</span></span>

- <span data-ttu-id="86390-219">[Web アプリを Azure App Service で](https://azure.microsoft.com/services/app-service/web/)です。</span><span class="sxs-lookup"><span data-stu-id="86390-219">[Web Apps in Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span></span> <span data-ttu-id="86390-220">Web アプリに関するドキュメントについては Azure ポータルのページです。</span><span class="sxs-lookup"><span data-stu-id="86390-220">Portal page for Azure documentation about Web Apps.</span></span>
- [<span data-ttu-id="86390-221">Web アプリ、クラウド サービス、および Vm: どれを使用するときにしますか?</span><span class="sxs-lookup"><span data-stu-id="86390-221">Web Apps, Cloud Services, and VMs: When to use which?</span></span>](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) <span data-ttu-id="86390-222">WAWS この章で示すようには、Azure で web アプリを実行する 3 つの方法の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="86390-222">WAWS as shown in this chapter is just one of three ways you can run web apps in Azure.</span></span> <span data-ttu-id="86390-223">この記事では、3 つの方法の違いについて説明し、シナリオに合った適切なものを選択する方法のガイダンスを示します。</span><span class="sxs-lookup"><span data-stu-id="86390-223">This article explains the differences between the three ways and gives guidance on how to choose which one is right for your scenario.</span></span> <span data-ttu-id="86390-224">Web サイトのようにクラウド サービスは、Azure の PaaS 機能です。</span><span class="sxs-lookup"><span data-stu-id="86390-224">Like Web Sites, Cloud Services is a PaaS feature of Azure.</span></span> <span data-ttu-id="86390-225">Vm は、IaaS 機能です。</span><span class="sxs-lookup"><span data-stu-id="86390-225">VMs are an IaaS feature.</span></span> <span data-ttu-id="86390-226">IaaS と PaaS の詳細については、次を参照してください。、[データ オプション](data-storage-options.md#paasiaas)章します。</span><span class="sxs-lookup"><span data-stu-id="86390-226">For an explanation of PaaS versus IaaS, see the [Data Options](data-storage-options.md#paasiaas) chapter.</span></span>

<span data-ttu-id="86390-227">ビデオ:</span><span class="sxs-lookup"><span data-stu-id="86390-227">Videos:</span></span>

- [<span data-ttu-id="86390-228">Scott Guthrie は、ステップ 0 では、Azure のクラウド OS から開始しますか。</span><span class="sxs-lookup"><span data-stu-id="86390-228">Scott Guthrie starts at Step 0 - What is the Azure Cloud OS?</span></span>](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- <span data-ttu-id="86390-229">[Web サイト アーキテクチャ - Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)です。</span><span class="sxs-lookup"><span data-stu-id="86390-229">[Web Sites Architecture - with Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).</span></span>
- <span data-ttu-id="86390-230">[Nir Mashkowski と azure の Web サイトの内部構造](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski)です。</span><span class="sxs-lookup"><span data-stu-id="86390-230">[Azure Web Sites Internals with Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="86390-231">次へ</span><span class="sxs-lookup"><span data-stu-id="86390-231">Next</span></span>](automate-everything.md)
