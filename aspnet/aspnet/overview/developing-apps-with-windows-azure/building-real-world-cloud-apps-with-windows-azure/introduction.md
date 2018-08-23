---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Azure での実際のクラウド アプリの構築 |Microsoft Docs
author: MikeWasson
description: この電子書籍手法について説明、パターンに基づいた現実世界のクラウド ソリューションを構築します。 パターンが的にも開発プロセスに適用する.
ms.author: riande
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: eade14bc27e2bface84fb0bdd2f3c5bf8ef28432
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827662"
---
<a name="building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="816fe-104">Azure での実際のクラウド アプリの構築</span><span class="sxs-lookup"><span data-stu-id="816fe-104">Building Real-World Cloud Apps with Azure</span></span>
====================
<span data-ttu-id="816fe-105">によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="816fe-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="816fe-106">[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="816fe-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="816fe-107">この電子書籍手法について説明、パターンに基づいた現実世界のクラウド ソリューションを構築します。</span><span class="sxs-lookup"><span data-stu-id="816fe-107">This e-book walks you through a patterns-based approach to building real-world cloud solutions.</span></span> <span data-ttu-id="816fe-108">パターンは、ほかのアーキテクチャとコーディングのプラクティス、開発プロセスに適用されます。</span><span class="sxs-lookup"><span data-stu-id="816fe-108">The patterns apply to the development process as well as to architecture and coding practices.</span></span>
> 
> <span data-ttu-id="816fe-109">Scott Guthrie によって開発され、2013 年 6 月の Norwegian 開発者 Conference (NDC) で自分が配信プレゼンテーションに基づいて、コンテンツ ([パート 1](http://vimeo.com/68215538)、[パート 2](http://vimeo.com/68215602)) とで Microsoft Tech Ed オーストラリア2013 年 9 月 ([パート 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324)、[パート 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325))。</span><span class="sxs-lookup"><span data-stu-id="816fe-109">The content is based on a presentation developed by Scott Guthrie and delivered by him at the Norwegian Developers Conference (NDC) in June of 2013 ([part 1](http://vimeo.com/68215538), [part 2](http://vimeo.com/68215602)), and at Microsoft Tech Ed Australia in September, 2013 ([part 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [part 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)).</span></span> <span data-ttu-id="816fe-110">[その他の多く](more-patterns-and-guidance.md#acknowledgments)更新およびビデオから記述形式への移行中に、コンテンツを増強します。</span><span class="sxs-lookup"><span data-stu-id="816fe-110">[Many others](more-patterns-and-guidance.md#acknowledgments) updated and augmented the content while transitioning it from video to written form.</span></span>


## <a name="intended-audience"></a><span data-ttu-id="816fe-111">対象読者</span><span class="sxs-lookup"><span data-stu-id="816fe-111">Intended Audience</span></span>

<span data-ttu-id="816fe-112">クラウドの開発に関心をクラウドに移行を検討またはクラウド開発に慣れていない開発者は最も重要な概念と認識する必要があるプラクティスの概要を簡潔ここで紹介します。</span><span class="sxs-lookup"><span data-stu-id="816fe-112">Developers who are curious about developing for the cloud, considering a move to the cloud, or are new to cloud development will find here a concise overview of the most important concepts and practices they need to know.</span></span> <span data-ttu-id="816fe-113">具体的な例については、各章は、詳細については、その他のリソースにリンクしています。 とは、概念が示されています。</span><span class="sxs-lookup"><span data-stu-id="816fe-113">The concepts are illustrated with concrete examples, and each chapter links to other resources for more in-depth information.</span></span> <span data-ttu-id="816fe-114">例では、およびその他のリソースへのリンクは Microsoft のフレームワークとサービス向けですが、示されている原則は、他の web 開発フレームワークに適用して、クラウド環境も。</span><span class="sxs-lookup"><span data-stu-id="816fe-114">The examples and the links to additional resources are for Microsoft frameworks and services, but the principles illustrated apply to other web development frameworks and cloud environments as well.</span></span>

<span data-ttu-id="816fe-115">ヘルプはアイデアをここで成功に導くためにより、クラウドの開発は既にしている開発者があります。</span><span class="sxs-lookup"><span data-stu-id="816fe-115">Developers who are already developing for the cloud may find ideas here that will help make them more successful.</span></span> <span data-ttu-id="816fe-116">系列の各章読み取れるいない、独立してを選択し、興味のトピックを選択できるように。</span><span class="sxs-lookup"><span data-stu-id="816fe-116">Each chapter in the series can be read independently, so you can pick and choose topics that you're interested in.</span></span>

<span data-ttu-id="816fe-117">Scott Guthrie を監視するすべてのユーザー*構築現実世界の Cloud Apps with Azure*プレゼンテーションの詳細および更新された情報の詳細はこちらを希望しています。</span><span class="sxs-lookup"><span data-stu-id="816fe-117">Anyone who watched Scott Guthrie's *Building Real World Cloud Apps with Azure* presentation and wants more details and updated information will find that here.</span></span>

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a><span data-ttu-id="816fe-118">クラウド開発パターン</span><span class="sxs-lookup"><span data-stu-id="816fe-118">Cloud development patterns</span></span>

<span data-ttu-id="816fe-119">この電子書籍では、13 の推奨クラウド開発のパターンについて説明します。</span><span class="sxs-lookup"><span data-stu-id="816fe-119">This e-book explains thirteen recommended patterns for cloud development.</span></span> <span data-ttu-id="816fe-120">「パターン」は広い意味での作業を行うことをお勧めの意味はここで使用: 開発、設計、およびクラウド アプリをコーディングに着手する最適な方法です。</span><span class="sxs-lookup"><span data-stu-id="816fe-120">"Pattern" is used here in a broad sense to mean a recommended way to do things: how best to go about developing, designing, and coding cloud apps.</span></span> <span data-ttu-id="816fe-121">これらは、それらの操作を行う場合に役立てること「成功の pit に分類されます」が主要なパターンです。</span><span class="sxs-lookup"><span data-stu-id="816fe-121">These are key patterns which will help you "fall into the pit of success" if you follow them.</span></span>

- <span data-ttu-id="816fe-122">[すべてを自動化](automate-everything.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-122">[Automate everything](automate-everything.md).</span></span>

    - <span data-ttu-id="816fe-123">スクリプトを使用して、効率を最大化し、反復的なプロセスでエラーを最小限に抑えます。</span><span class="sxs-lookup"><span data-stu-id="816fe-123">Use scripts to maximize efficiency and minimize errors in repetitive processes.</span></span>
    - <span data-ttu-id="816fe-124">チュートリアル: Azure の管理スクリプトです。</span><span class="sxs-lookup"><span data-stu-id="816fe-124">Demo: Azure management scripts.</span></span>
- <span data-ttu-id="816fe-125">[ソース管理](source-control.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-125">[Source control](source-control.md).</span></span> 

    - <span data-ttu-id="816fe-126">DevOps ワークフローを容易にするために、ソース管理では分岐構造を設定します。</span><span class="sxs-lookup"><span data-stu-id="816fe-126">Set up branching structure in source control to facilitate DevOps workflow.</span></span>
    - <span data-ttu-id="816fe-127">デモ: は、スクリプトをソース コントロールに追加します。</span><span class="sxs-lookup"><span data-stu-id="816fe-127">Demo: add scripts to source control.</span></span>
    - <span data-ttu-id="816fe-128">デモ: は、ソース管理からの機密データを保持します。</span><span class="sxs-lookup"><span data-stu-id="816fe-128">Demo: keep sensitive data out of source control.</span></span>
    - <span data-ttu-id="816fe-129">チュートリアル: Visual Studio で Git を使用します。</span><span class="sxs-lookup"><span data-stu-id="816fe-129">Demo: use Git in Visual Studio.</span></span>
- <span data-ttu-id="816fe-130">[継続的インテグレーションと配信](continuous-integration-and-continuous-delivery.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-130">[Continuous integration and delivery](continuous-integration-and-continuous-delivery.md).</span></span> 

    - <span data-ttu-id="816fe-131">各ソース コントロール チェックイン配置ビルドを自動化します。</span><span class="sxs-lookup"><span data-stu-id="816fe-131">Automate build and deployment with each source control check-in.</span></span>
- <span data-ttu-id="816fe-132">[Web 開発のベスト プラクティス](web-development-best-practices.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-132">[Web development best practices](web-development-best-practices.md).</span></span> 

    - <span data-ttu-id="816fe-133">ステートレス web 層を保持します。</span><span class="sxs-lookup"><span data-stu-id="816fe-133">Keep web tier stateless.</span></span>
    - <span data-ttu-id="816fe-134">デモ: スケーリングと、Azure App Service で Web アプリで自動スケーリングします。</span><span class="sxs-lookup"><span data-stu-id="816fe-134">Demo: scaling and auto-scaling in Web Apps in Azure App Service.</span></span>
    - <span data-ttu-id="816fe-135">セッション状態を回避します。</span><span class="sxs-lookup"><span data-stu-id="816fe-135">Avoid session state.</span></span>
    - <span data-ttu-id="816fe-136">CDN が利用できない場合は、フォールバック CDN を使用します。</span><span class="sxs-lookup"><span data-stu-id="816fe-136">Use a CDN with a fallback when the CDN is unavailable.</span></span>
    - <span data-ttu-id="816fe-137">非同期プログラミング モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="816fe-137">Use asynchronous programming model.</span></span>
    - <span data-ttu-id="816fe-138">ASP.NET MVC と Entity Framework でのデモ: 非同期です。</span><span class="sxs-lookup"><span data-stu-id="816fe-138">Demo: async in ASP.NET MVC and Entity Framework.</span></span>
- <span data-ttu-id="816fe-139">[シングル サインオン](single-sign-on.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-139">[Single sign-on](single-sign-on.md).</span></span> 

    - <span data-ttu-id="816fe-140">Azure Active Directory を紹介します。</span><span class="sxs-lookup"><span data-stu-id="816fe-140">Introduction to Azure Active Directory.</span></span>
    - <span data-ttu-id="816fe-141">チュートリアル: Azure Active Directory を使用する ASP.NET アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="816fe-141">Demo: create an ASP.NET app that uses Azure Active Directory.</span></span>
- <span data-ttu-id="816fe-142">[データ ストレージ オプション](data-storage-options.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-142">[Data storage options](data-storage-options.md).</span></span> 

    - <span data-ttu-id="816fe-143">データ ストアの種類。</span><span class="sxs-lookup"><span data-stu-id="816fe-143">Types of data stores.</span></span>
    - <span data-ttu-id="816fe-144">適切なデータ ストアを選択する方法。</span><span class="sxs-lookup"><span data-stu-id="816fe-144">How to choose the right data store.</span></span>
    - <span data-ttu-id="816fe-145">チュートリアル: Azure SQL Database。</span><span class="sxs-lookup"><span data-stu-id="816fe-145">Demo: Azure SQL Database.</span></span>
- <span data-ttu-id="816fe-146">[データのパーティション分割戦略](data-partitioning-strategies.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-146">[Data partitioning strategies](data-partitioning-strategies.md).</span></span> 

    - <span data-ttu-id="816fe-147">データをパーティション分割して、垂直、水平、またはその両方に、リレーショナル データベースのスケーリングを容易にします。</span><span class="sxs-lookup"><span data-stu-id="816fe-147">Partition data vertically, horizontally, or both to facilitate scaling a relational database.</span></span>
- <span data-ttu-id="816fe-148">[非構造化 blob storage](unstructured-blob-storage.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-148">[Unstructured blob storage](unstructured-blob-storage.md).</span></span> 

    - <span data-ttu-id="816fe-149">Blob サービスを使用して、クラウド内のファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="816fe-149">Store files in the cloud by using the blob service.</span></span>
    - <span data-ttu-id="816fe-150">デモ: Fix It アプリで blob ストレージを使用します。</span><span class="sxs-lookup"><span data-stu-id="816fe-150">Demo: using blob storage in the Fix It app.</span></span>
- <span data-ttu-id="816fe-151">[障害から復旧する設計](design-to-survive-failures.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-151">[Design to survive failures](design-to-survive-failures.md).</span></span> 

    - <span data-ttu-id="816fe-152">エラーの種類。</span><span class="sxs-lookup"><span data-stu-id="816fe-152">Types of failures.</span></span>
    - <span data-ttu-id="816fe-153">エラーのスコープです。</span><span class="sxs-lookup"><span data-stu-id="816fe-153">Failure Scope.</span></span>
    - <span data-ttu-id="816fe-154">についての Sla です。</span><span class="sxs-lookup"><span data-stu-id="816fe-154">Understanding SLAs.</span></span>
- <span data-ttu-id="816fe-155">[監視とテレメトリ](monitoring-and-telemetry.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-155">[Monitoring and telemetry](monitoring-and-telemetry.md).</span></span> 

    - <span data-ttu-id="816fe-156">なぜテレメトリ アプリを購入両方と、アプリをインストルメント化するコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="816fe-156">Why you should both buy a telemetry app and write your own code to instrument your app.</span></span>
    - <span data-ttu-id="816fe-157">Azure 用の New Relic のデモ:</span><span class="sxs-lookup"><span data-stu-id="816fe-157">Demo: New Relic for Azure</span></span>
    - <span data-ttu-id="816fe-158">デモ: は、修正、アプリをコードを記録します。</span><span class="sxs-lookup"><span data-stu-id="816fe-158">Demo: logging code in the Fix It app.</span></span>
    - <span data-ttu-id="816fe-159">Fix It アプリのデモ: 依存関係挿入します。</span><span class="sxs-lookup"><span data-stu-id="816fe-159">Demo: dependency injection in the Fix It app.</span></span>
    - <span data-ttu-id="816fe-160">Azure でのデモ: 組み込みのログ記録のサポート。</span><span class="sxs-lookup"><span data-stu-id="816fe-160">Demo: built-in logging support in Azure.</span></span>
- <span data-ttu-id="816fe-161">[一時的な障害処理](transient-fault-handling.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-161">[Transient fault handling](transient-fault-handling.md).</span></span> 

    - <span data-ttu-id="816fe-162">一時的な障害の影響を緩和するのにには、スマート再試行/バックオフ ロジックを使用します。</span><span class="sxs-lookup"><span data-stu-id="816fe-162">Use smart retry/back-off logic to mitigate the effect of transient failures.</span></span>
    - <span data-ttu-id="816fe-163">デモ: 再試行/バックオフで Entity Framework 6 です。</span><span class="sxs-lookup"><span data-stu-id="816fe-163">Demo: retry/back-off in Entity Framework 6.</span></span>
- <span data-ttu-id="816fe-164">[分散キャッシュ](distributed-caching.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-164">[Distributed caching](distributed-caching.md).</span></span> 

    - <span data-ttu-id="816fe-165">スケーラビリティが向上し、分散キャッシュを使用してデータベースのトランザクション コストを削減します。</span><span class="sxs-lookup"><span data-stu-id="816fe-165">Improve scalability and reduce database transaction costs by using distributed caching.</span></span>
- <span data-ttu-id="816fe-166">[キューを中心とした作業パターン](queue-centric-work-pattern.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-166">[Queue-centric work pattern](queue-centric-work-pattern.md).</span></span> 

    - <span data-ttu-id="816fe-167">高可用性を有効にして、web とワーカーの層を疎結合することで、スケーラビリティが向上します。</span><span class="sxs-lookup"><span data-stu-id="816fe-167">Enable high availability and improve scalability by loosely coupling web and worker tiers.</span></span>
    - <span data-ttu-id="816fe-168">Fix It アプリのデモ: Azure storage キュー。</span><span class="sxs-lookup"><span data-stu-id="816fe-168">Demo: Azure storage queues in the Fix It app.</span></span>
- <span data-ttu-id="816fe-169">[他のクラウド アプリのパターンとガイダンス](more-patterns-and-guidance.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-169">[More cloud app patterns and guidance](more-patterns-and-guidance.md).</span></span>
- [<span data-ttu-id="816fe-170">付録: Fix It サンプル アプリケーション</span><span class="sxs-lookup"><span data-stu-id="816fe-170">Appendix: The Fix It Sample Application</span></span>](the-fix-it-sample-application.md)

    - <span data-ttu-id="816fe-171">既知の問題</span><span class="sxs-lookup"><span data-stu-id="816fe-171">Known Issues</span></span>
    - <span data-ttu-id="816fe-172">ベスト プラクティス</span><span class="sxs-lookup"><span data-stu-id="816fe-172">Best Practices</span></span>
    - <span data-ttu-id="816fe-173">ダウンロード、ビルド、実行、およびデプロイする方法。</span><span class="sxs-lookup"><span data-stu-id="816fe-173">How to download, build, run, and deploy.</span></span>

<span data-ttu-id="816fe-174">これらのパターンは、すべてのクラウド環境に適用されますが、について Microsoft テクノロジと Visual Studio、Team Foundation Service、ASP.NET、および Azure などのサービスに基づく例を使用して説明します。</span><span class="sxs-lookup"><span data-stu-id="816fe-174">These patterns apply to all cloud environments, but we'll illustrate them by using examples based on Microsoft technologies and services, such as Visual Studio, Team Foundation Service, ASP.NET, and Azure.</span></span>

<span data-ttu-id="816fe-175">この章の残りの部分では、Fix It サンプル アプリケーションとで Fix It アプリが実行されているクラウド環境を Azure App Service で Web アプリを紹介します。</span><span class="sxs-lookup"><span data-stu-id="816fe-175">This remainder of this chapter introduces the Fix It sample application and the Web Apps in Azure App Service cloud environment that the Fix It app runs in.</span></span>

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a><span data-ttu-id="816fe-176">It サンプル アプリケーションの修正</span><span class="sxs-lookup"><span data-stu-id="816fe-176">The Fix it sample application</span></span>

<span data-ttu-id="816fe-177">スクリーン ショットとコードの例では、この電子書籍のほとんどはアプリに基づいて、Fix It によって開発された[Scott Guthrie](https://weblogs.asp.net/scottgu/)を推奨されるクラウド アプリの開発パターンとプラクティスを示します。</span><span class="sxs-lookup"><span data-stu-id="816fe-177">Most of the screen shots and code examples shown in this e-book are based on the Fix It app originally developed by [Scott Guthrie](https://weblogs.asp.net/scottgu/) to demonstrate recommended cloud app development patterns and practices.</span></span>

![アプリのホーム ページを修正します。](introduction/_static/image1.png)

<span data-ttu-id="816fe-179">サンプル アプリは、チケット システムの簡単な作業項目です。</span><span class="sxs-lookup"><span data-stu-id="816fe-179">The sample app is a simple work item ticketing system.</span></span> <span data-ttu-id="816fe-180">問題を解決する場合は、チケットと割り当て、人と他のユーザーにログインして割り当てられているチケットを参照してくださいをそれらに作成し、チケットのマーク付け、作業が終わったときに完了しました。</span><span class="sxs-lookup"><span data-stu-id="816fe-180">When you need something fixed, you create a ticket and assign it to someone, and others can log in and see the tickets assigned to them and mark tickets as completed when the work is done.</span></span>

<span data-ttu-id="816fe-181">これは標準の Visual Studio web プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="816fe-181">It's a standard Visual Studio web project.</span></span> <span data-ttu-id="816fe-182">ASP.NET MVC 上に構築し、SQL Server データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="816fe-182">It is built on ASP.NET MVC and uses a SQL Server database.</span></span> <span data-ttu-id="816fe-183">IIS Express でローカルで実行できるし、クラウドで実行する Azure の Web サイトを展開できます。</span><span class="sxs-lookup"><span data-stu-id="816fe-183">It can run locally in IIS Express and can be deployed to a Azure Web Site to run in the cloud.</span></span> <span data-ttu-id="816fe-184">フォーム認証と、ローカル データベースを使用して、または Google などのソーシャル プロバイダーを使用してログインすることができます。</span><span class="sxs-lookup"><span data-stu-id="816fe-184">You can log in using forms authentication and a local database or by using a social provider such as Google.</span></span> <span data-ttu-id="816fe-185">(後でも紹介します Active Directory の組織アカウントでログインする方法。)</span><span class="sxs-lookup"><span data-stu-id="816fe-185">(Later we'll also show how to log in with an Active Directory organizational account.)</span></span>

![ログイン ページ](introduction/_static/image2.png)

<span data-ttu-id="816fe-187">ログイン後にチケットを作成のユーザーに割り当てして修正する内容の画像をアップロードします。</span><span class="sxs-lookup"><span data-stu-id="816fe-187">Once you're logged in you can create a ticket, assign it to someone, and upload a picture of what you want to get fixed.</span></span>

![Fix It タスクを作成します。](introduction/_static/image3.png)

![修正タスクの作成](introduction/_static/image4.png)

<span data-ttu-id="816fe-190">完了したため、チケットの詳細を表示、およびアイテムを開封済みに割り当てられているチケットを参照してください、作成した作業項目の進行状況を追跡できます。</span><span class="sxs-lookup"><span data-stu-id="816fe-190">You can track the progress of work items you created, see tickets assigned to you, view ticket details, and mark items as completed.</span></span>

<span data-ttu-id="816fe-191">これは、機能の観点から非常に単純なアプリですが、このように何百万ものユーザーにスケールでき、データベースの障害と接続の終了などの回復性があることを構築する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="816fe-191">This is a very simple app from a feature perspective, but you'll see how to build it so that it can scale to millions of users and will be resilient to things like database failures and connection terminations.</span></span> <span data-ttu-id="816fe-192">使用すると、簡単に開始し、アプリをより迅速かつ効率的に開発サイクルを反復処理するように自動でアジャイルな開発ワークフローを作成する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="816fe-192">You'll also see how to create an automated and agile development workflow, which enables you to start simple and make the app better and better by iterating the development cycle efficiently and quickly.</span></span>

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a><span data-ttu-id="816fe-193">Azure App Service で web アプリ</span><span class="sxs-lookup"><span data-stu-id="816fe-193">Web Apps in Azure App Service</span></span>

<span data-ttu-id="816fe-194">Fix It のアプリケーションで使用するクラウド環境とは、Web サイトと呼ばれる Azure のサービスです。</span><span class="sxs-lookup"><span data-stu-id="816fe-194">The cloud environment used for the Fix It application is a service of Azure that we call Web Sites.</span></span> <span data-ttu-id="816fe-195">このサービスは、ことは Vm を作成し、それらを更新された保持することがなく Azure で web アプリをホストする、インストールして構成する IIS などの方法です。Vm で、サイトをホストし、自動的にバックアップと回復、およびその他のサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="816fe-195">This service is a way that you can host your own web app in Azure without having to create VMs and keep them updated, install and configure IIS, etc. We host your site on our VMs and automatically provide backup and recovery and other services for you.</span></span> <span data-ttu-id="816fe-196">Web サイト サービスでは、ASP.NET、Node.js、PHP、および Python で動作します。</span><span class="sxs-lookup"><span data-stu-id="816fe-196">The Web Sites service works with ASP.NET, Node.js, PHP, and Python.</span></span> <span data-ttu-id="816fe-197">Visual Studio、Web Deploy、FTP、Git または TFS を使用して、非常に迅速にデプロイすることができます。</span><span class="sxs-lookup"><span data-stu-id="816fe-197">It enables you to deploy very quickly using Visual Studio, Web Deploy, FTP, Git, or TFS.</span></span> <span data-ttu-id="816fe-198">一般的には、デプロイを開始する時間と、インターネット経由で、更新が利用可能時間はわずか数秒です。</span><span class="sxs-lookup"><span data-stu-id="816fe-198">It's usually just a few seconds between the time you start a deployment and the time your update is available over the Internet.</span></span> <span data-ttu-id="816fe-199">開始するにはすべて無料ですし、トラフィックの増加に応じてスケール アップできます。</span><span class="sxs-lookup"><span data-stu-id="816fe-199">It's all free to get started, and you can scale up as your traffic grows.</span></span>

<span data-ttu-id="816fe-200">バック グラウンドでは、Azure App Service で Web アプリは、多くのアーキテクチャのコンポーネントと、独自の Vm 上で IIS を使用して web サイトをホストする場合、自分で構築することになる機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="816fe-200">Behind the scenes, Web Apps in Azure App Service provides a lot of architectural components and features that you'd have to build yourself if you were going to host a web site using IIS on your own VMs.</span></span> <span data-ttu-id="816fe-201">1 つのコンポーネントは、自動的に IIS を構成し、多くの Vm で、サイトを実行するように、アプリケーションをインストールする展開のエンド ポイントです。</span><span class="sxs-lookup"><span data-stu-id="816fe-201">One component is a deployment end point that automatically configures IIS and installs your application on as many VMs as you want to run your site on.</span></span>

![展開サービス](introduction/_static/image5.png)

<span data-ttu-id="816fe-203">Web サイトにユーザーが、IIS Vm を直接ヒットはありませんに至るまで[Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing)ロード バランサー。</span><span class="sxs-lookup"><span data-stu-id="816fe-203">When a user hits the web site, they don't hit the IIS VMs directly, they go through [Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) load balancers.</span></span> <span data-ttu-id="816fe-204">、サーバーとこれらを使用できますが、ここでの利点は、いるセットアップが自動的にします。</span><span class="sxs-lookup"><span data-stu-id="816fe-204">You can use these with your own servers, but the advantage here is that they're set up for you automatically.</span></span> <span data-ttu-id="816fe-205">セッション アフィニティ、IIS では、キューの深さなどの要因が考慮に入れてスマート ヒューリスティックを使用して各 CPU 使用率機械の Vm にトラフィックを送信するホストする web サイト。</span><span class="sxs-lookup"><span data-stu-id="816fe-205">They use a smart heuristic that takes into account factors such as session affinity, queue depth in IIS, and CPU usage on each machine to direct traffic to the VMs that host your web site.</span></span>

![ARR のロード バランサー](introduction/_static/image6.png)

<span data-ttu-id="816fe-207">マシンがダウンした場合 Azure に自動的に、ローテーションからプル、新しい VM インスタンスをスピンアップし、開始--すべてで、アプリケーションのダウンタイムなくの新しいインスタンスにトラフィックを誘導します。</span><span class="sxs-lookup"><span data-stu-id="816fe-207">If a machine goes down, Azure automatically pulls it from the rotation, spins up a new VM instance, and starts directing traffic to the new instance -- all with no down time for your application.</span></span>

![マシンの障害からの自動復旧](introduction/_static/image7.png)

<span data-ttu-id="816fe-209">このすべては自動的に実行します。</span><span class="sxs-lookup"><span data-stu-id="816fe-209">All of this takes place automatically.</span></span> <span data-ttu-id="816fe-210">行う必要があるものは、web サイトを作成し、Windows PowerShell、Visual Studio、または Azure 管理ポータルを使用して、アプリケーションをデプロイするだけです。</span><span class="sxs-lookup"><span data-stu-id="816fe-210">All you need to do is create a web site and deploy your application to it, using Windows PowerShell, Visual Studio, or the Azure management portal.</span></span>

<span data-ttu-id="816fe-211">迅速かつ簡単なチュートリアルについては Visual Studio で web アプリケーションを作成して、Azure の Web サイトにデプロイする方法を示す、次を参照してください。 [Azure と ASP.NET の概要](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-211">For a quick and easy step-by-step tutorial that shows how to create a web application in Visual Studio and deploy it to a Azure Web Site, see [Get started with Azure and ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<a id="summary"></a>
## <a name="summary"></a><span data-ttu-id="816fe-212">まとめ</span><span class="sxs-lookup"><span data-stu-id="816fe-212">Summary</span></span>

<span data-ttu-id="816fe-213">この概要は、この書籍が取り扱うトピック、サンプル アプリケーションのスクリーン ショットとクラウド環境を Azure App Service で Web アプリの概要のリストを提供しています。</span><span class="sxs-lookup"><span data-stu-id="816fe-213">This introduction has provided a list of topics the book will cover, screenshots of the sample application, and a brief overview of the Web Apps in Azure App Service cloud environment.</span></span> <span data-ttu-id="816fe-214">アプリの開発でと、クラウド向けの優れたメリットの 1 つは、テスト環境を作成し、コードを展開するなどの反復的な開発タスクを自動化する簡単であります。</span><span class="sxs-lookup"><span data-stu-id="816fe-214">One of the great advantages of developing apps in and for the cloud is that it's easy to automate repetitive development tasks such as creating a test environment and deploying your code to it.</span></span> <span data-ttu-id="816fe-215">サブジェクトで実行されている方法、 [[次へ] の章](automate-everything.md)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-215">How to do that is the subject of the [next chapter](automate-everything.md).</span></span>

## <a name="resources"></a><span data-ttu-id="816fe-216">リソース</span><span class="sxs-lookup"><span data-stu-id="816fe-216">Resources</span></span>

<span data-ttu-id="816fe-217">この章で説明したトピックの詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="816fe-217">For more information about the topics covered in this chapter, see the following resources.</span></span>

<span data-ttu-id="816fe-218">ドキュメント:</span><span class="sxs-lookup"><span data-stu-id="816fe-218">Documentation:</span></span>

- <span data-ttu-id="816fe-219">[Web アプリを Azure App Service で](https://azure.microsoft.com/services/app-service/web/)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-219">[Web Apps in Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span></span> <span data-ttu-id="816fe-220">Web アプリに関するドキュメントについては Azure ポータルのページ。</span><span class="sxs-lookup"><span data-stu-id="816fe-220">Portal page for Azure documentation about Web Apps.</span></span>
- [<span data-ttu-id="816fe-221">Web Apps、Cloud Services、および Vm: いつどれを使用するでしょうか。</span><span class="sxs-lookup"><span data-stu-id="816fe-221">Web Apps, Cloud Services, and VMs: When to use which?</span></span>](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) <span data-ttu-id="816fe-222">WAWS この章で示すようには、Azure で web アプリを実行する 3 つの方法の 1 つにすぎません。</span><span class="sxs-lookup"><span data-stu-id="816fe-222">WAWS as shown in this chapter is just one of three ways you can run web apps in Azure.</span></span> <span data-ttu-id="816fe-223">この記事では、3 つの方法の違いについて説明し、シナリオでは、適切などの 1 つを選択する方法に関するガイダンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="816fe-223">This article explains the differences between the three ways and gives guidance on how to choose which one is right for your scenario.</span></span> <span data-ttu-id="816fe-224">Web サイトでは、ようなクラウド サービスは、Azure の PaaS 機能です。</span><span class="sxs-lookup"><span data-stu-id="816fe-224">Like Web Sites, Cloud Services is a PaaS feature of Azure.</span></span> <span data-ttu-id="816fe-225">Vm は、IaaS 機能です。</span><span class="sxs-lookup"><span data-stu-id="816fe-225">VMs are an IaaS feature.</span></span> <span data-ttu-id="816fe-226">IaaS と PaaS の詳細については、次を参照してください。、[データ オプション](data-storage-options.md#paasiaas)」の章。</span><span class="sxs-lookup"><span data-stu-id="816fe-226">For an explanation of PaaS versus IaaS, see the [Data Options](data-storage-options.md#paasiaas) chapter.</span></span>

<span data-ttu-id="816fe-227">ビデオ:</span><span class="sxs-lookup"><span data-stu-id="816fe-227">Videos:</span></span>

- [<span data-ttu-id="816fe-228">Scott Guthrie は、手順 0 - 新機能、Azure のクラウド OS から開始しますか。</span><span class="sxs-lookup"><span data-stu-id="816fe-228">Scott Guthrie starts at Step 0 - What is the Azure Cloud OS?</span></span>](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- <span data-ttu-id="816fe-229">[Web サイトのアーキテクチャ - Stefan schackow 共演](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-229">[Web Sites Architecture - with Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).</span></span>
- <span data-ttu-id="816fe-230">[Azure の Web サイトの内部構造」Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski)します。</span><span class="sxs-lookup"><span data-stu-id="816fe-230">[Azure Web Sites Internals with Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="816fe-231">次へ</span><span class="sxs-lookup"><span data-stu-id="816fe-231">Next</span></span>](automate-everything.md)
