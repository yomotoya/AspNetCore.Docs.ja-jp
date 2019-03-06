---
title: ASP.NET Core のロード/ストレス テスト
author: Jeremy-Meng
description: いくつかの注目すべきツールとロード テストとストレスの ASP.NET Core アプリをテストするための方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: 587df6e216943d3eeec779df4d0554dd0fc2fda0
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345429"
---
# <a name="load-and-stress-testing-aspnet-core"></a><span data-ttu-id="f4e10-103">ロード テストとストレス テストの ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4e10-103">Load and stress testing ASP.NET Core</span></span>

<span data-ttu-id="f4e10-104">ロード テストとストレス テストは、web アプリが高パフォーマンスであることを確認する重要な拡張性の高いです。</span><span class="sxs-lookup"><span data-stu-id="f4e10-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="f4e10-105">その目標が異なる類似のテストを多くの場合、共有もします。</span><span class="sxs-lookup"><span data-stu-id="f4e10-105">Their goals are different even they often share similar tests.</span></span>

<span data-ttu-id="f4e10-106">**ロード テスト**:アプリは、応答の目標を引き続き満たしながら特定のシナリオのユーザーの指定した負荷を処理できるかどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="f4e10-106">**Load tests**: Tests whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="f4e10-107">アプリは、通常の状況で実行されます。</span><span class="sxs-lookup"><span data-stu-id="f4e10-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="f4e10-108">**ストレス テスト**:テスト アプリの安定性では、極端な場合は、多くの場合、長期間実行されている場合:</span><span class="sxs-lookup"><span data-stu-id="f4e10-108">**Stress tests**: Tests app stability when running under extreme conditions and often a long period of time:</span></span>

* <span data-ttu-id="f4e10-109">高度なユーザー ロード – スパイクまたは徐々 に増やしていきます。</span><span class="sxs-lookup"><span data-stu-id="f4e10-109">High user load – either spikes or gradually increasing.</span></span>
* <span data-ttu-id="f4e10-110">コンピューティング リソースの制約。</span><span class="sxs-lookup"><span data-stu-id="f4e10-110">Limited computing resources.</span></span>  

<span data-ttu-id="f4e10-111">ストレス条件下でをアプリの障害から回復し、適切に想定される動作に戻りますか。</span><span class="sxs-lookup"><span data-stu-id="f4e10-111">Under stress, can the app recover from failure and gracefully return to expected behavior?</span></span> <span data-ttu-id="f4e10-112">アプリは、ストレス条件下で*いない*通常の状況下で実行します。</span><span class="sxs-lookup"><span data-stu-id="f4e10-112">Under stress, the app is *not* run under normal conditions.</span></span>

<span data-ttu-id="f4e10-113">Visual Studio 2019 は、ロード テスト機能を備えた Visual Studio として最後のバージョンとなります。</span><span class="sxs-lookup"><span data-stu-id="f4e10-113">Visual Studio 2019 will be the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="f4e10-114">ロード テスト ツールを必要とするお客様には、Apache JMeter、Akamai CloudTest、Blazemeter などの代替のロード テスト ツールの使用をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="f4e10-114">For customers requiring load testing tools, we recommend using alternate load testing tools such as Apache JMeter, Akamai CloudTest, Blazemeter.</span></span> <span data-ttu-id="f4e10-115">詳細については、次を参照してください。、 [Visual Studio 2019 Preview リリース ノート](/visualstudio/releases/2019/release-notes-preview#test-tools)します。</span><span class="sxs-lookup"><span data-stu-id="f4e10-115">For more information, see the [Visual Studio 2019 Preview Release Notes](/visualstudio/releases/2019/release-notes-preview#test-tools).</span></span>

<span data-ttu-id="f4e10-116">ロード テスト サービスを Azure DevOps では、2020年で終了します。</span><span class="sxs-lookup"><span data-stu-id="f4e10-116">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="f4e10-117">詳細については、次を参照してください。[クラウド ベースのロード テスト サービス終了](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/)します。</span><span class="sxs-lookup"><span data-stu-id="f4e10-117">For more information see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="f4e10-118">Visual Studio ツール</span><span class="sxs-lookup"><span data-stu-id="f4e10-118">Visual Studio Tools</span></span>

<span data-ttu-id="f4e10-119">Visual Studio では、作成、開発、および web パフォーマンスとロード テストをデバッグすることができます。</span><span class="sxs-lookup"><span data-stu-id="f4e10-119">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="f4e10-120">Web ブラウザーでアクションを記録することによってテストを作成するオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="f4e10-120">An option is available to create tests by recording actions in web browser.</span></span>

<span data-ttu-id="f4e10-121">[クイック スタート:ロード テスト プロジェクトを作成](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)作成、構成、およびロード テストを Visual Studio 2017 を使用してプロジェクトを実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f4e10-121">[Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) shows how to create, configure, and run a load test projects using Visual Studio 2017.</span></span>

<span data-ttu-id="f4e10-122">詳しくは、「[その他の技術情報](#add)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="f4e10-122">See [Additional Resources](#add) for more information.</span></span>

<span data-ttu-id="f4e10-123">Azure DevOps を使用してクラウドで実行またはオンプレミスで実行するロード テストを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="f4e10-123">Load tests can be configured to run in on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="f4e10-124">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="f4e10-124">Azure DevOps</span></span>

<span data-ttu-id="f4e10-125">使用してロード テストの実行を開始することができます、 [Azure DevOps テスト計画](/azure/devops/test/load-test/index?view=vsts)サービス。</span><span class="sxs-lookup"><span data-stu-id="f4e10-125">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="f4e10-126">サービスには、次の種類のテストの形式がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="f4e10-126">The service supports the following types of test format:</span></span>

- <span data-ttu-id="f4e10-127">Visual Studio テスト – Visual Studio で作成した web テストします。</span><span class="sxs-lookup"><span data-stu-id="f4e10-127">Visual Studio test – web test created in Visual Studio.</span></span>
- <span data-ttu-id="f4e10-128">HTTP アーカイブ ベースのテスト – アーカイブ内にキャプチャされた HTTP トラフィックがテスト中に再生されます。</span><span class="sxs-lookup"><span data-stu-id="f4e10-128">HTTP Archive-based test – captured HTTP traffic inside archive is replayed during testing.</span></span>
- <span data-ttu-id="f4e10-129">[URL ベースのテスト](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts)– テスト、要求の種類、ヘッダー、およびクエリ文字列を読み込む Url を指定できます。</span><span class="sxs-lookup"><span data-stu-id="f4e10-129">[URL-based test](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="f4e10-130">実行時間などのパラメーターの設定を実行するには、ロード パターンでは、ユーザーなどの数を構成できます。</span><span class="sxs-lookup"><span data-stu-id="f4e10-130">Run setting parameters such as duration, load pattern, number of users, etc., can be configured.</span></span>
- <span data-ttu-id="f4e10-131">[Apache JMeter](https://jmeter.apache.org/)をテストします。</span><span class="sxs-lookup"><span data-stu-id="f4e10-131">[Apache JMeter](https://jmeter.apache.org/) test.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="f4e10-132">Azure ポータル</span><span class="sxs-lookup"><span data-stu-id="f4e10-132">Azure portal</span></span>

<span data-ttu-id="f4e10-133">[Azure ポータルでは、設定して、Web アプリのロード テストを実行する](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts)Azure portal で App Service の [パフォーマンス] タブから直接します。</span><span class="sxs-lookup"><span data-stu-id="f4e10-133">[Azure portal allows setting up and running load testing of Web Apps,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the Performance tab of the App Service in Azure portal.</span></span>

![](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="f4e10-134">テストでは、指定した URL、または複数の Url をテストできる、Visual Studio Web テスト ファイルで手動テストを指定できます。</span><span class="sxs-lookup"><span data-stu-id="f4e10-134">The test can be a manual test with a specified URL, or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="f4e10-135">テストの最後に、アプリのパフォーマンス特性を表示するレポートを生成します。</span><span class="sxs-lookup"><span data-stu-id="f4e10-135">At end of the test, reports are generated to show the performance characteristics of the app.</span></span> <span data-ttu-id="f4e10-136">統計の例は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f4e10-136">Example statistics include:</span></span>

- <span data-ttu-id="f4e10-137">平均応答時間</span><span class="sxs-lookup"><span data-stu-id="f4e10-137">Average response time</span></span>
- <span data-ttu-id="f4e10-138">最大スループット: 1 秒あたりの要求</span><span class="sxs-lookup"><span data-stu-id="f4e10-138">Max throughput: requests per second</span></span>
- <span data-ttu-id="f4e10-139">失敗率</span><span class="sxs-lookup"><span data-stu-id="f4e10-139">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="f4e10-140">サード パーティのツール</span><span class="sxs-lookup"><span data-stu-id="f4e10-140">Third-party Tools</span></span>

<span data-ttu-id="f4e10-141">次の一覧には、さまざまな機能セットとサード パーティの web パフォーマンス ツールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f4e10-141">The following list contains third-party web performance tools with various feature sets:</span></span>

- <span data-ttu-id="f4e10-142">[Apache JMeter](https://jmeter.apache.org/) :ロード テスト ツールの完全なおすすめのスイートです。</span><span class="sxs-lookup"><span data-stu-id="f4e10-142">[Apache JMeter](https://jmeter.apache.org/) : Full featured suite of load testing tools.</span></span> <span data-ttu-id="f4e10-143">スレッドに依存します。 には、ユーザーごとの 1 つのスレッドが必要があります。</span><span class="sxs-lookup"><span data-stu-id="f4e10-143">Thread-bound: need one thread per user.</span></span>
- [<span data-ttu-id="f4e10-144">ab - Apache HTTP server がベンチマーク ツール</span><span class="sxs-lookup"><span data-stu-id="f4e10-144">ab - Apache HTTP server benchmarking tool</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
- <span data-ttu-id="f4e10-145">[ガットリング](https://gatling.io/):GUI ツールとテスト レコーダーでデスクトップ ツールです。</span><span class="sxs-lookup"><span data-stu-id="f4e10-145">[Gatling](https://gatling.io/) : Desktop tool with a GUI and test recorders.</span></span> <span data-ttu-id="f4e10-146">JMeter よりパフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="f4e10-146">More performant than JMeter.</span></span>
- <span data-ttu-id="f4e10-147">[Locust.io](https://locust.io/) :スレッドに制限されません。</span><span class="sxs-lookup"><span data-stu-id="f4e10-147">[Locust.io](https://locust.io/) : Not bounded by threads.</span></span>

<a name="add"></a>
## <a name="additional-resources"></a><span data-ttu-id="f4e10-148">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="f4e10-148">Additional Resources</span></span>

<span data-ttu-id="f4e10-149">[ロード テストのブログ シリーズ](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)作成者: Charles Sterling します。</span><span class="sxs-lookup"><span data-stu-id="f4e10-149">[Load Test blog series](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) by Charles Sterling.</span></span> <span data-ttu-id="f4e10-150">日が指定されたが、ほとんどの項目は引き続き関連します。</span><span class="sxs-lookup"><span data-stu-id="f4e10-150">Dated but most of the topics are still relevant.</span></span>
