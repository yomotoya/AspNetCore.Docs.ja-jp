---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: ASP.NET Web API 2 でのトレース |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API でのトレースを有効にする方法を示しています。
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 697e7e91ae2d9d5712d9306a291635793063117b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832807"
---
<a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="b8021-103">ASP.NET Web API 2 でのトレース</span><span class="sxs-lookup"><span data-stu-id="b8021-103">Tracing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="b8021-104">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b8021-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="b8021-105">Web ベースのアプリケーションをデバッグするときは、適切な一連のトレース ログの代替はありません。</span><span class="sxs-lookup"><span data-stu-id="b8021-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="b8021-106">このチュートリアルでは、ASP.NET Web API でのトレースを有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b8021-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="b8021-107">Web API フレームワークが、コント ローラーを呼び出す前後にはトレースには、この機能を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="b8021-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="b8021-108">独自のコードをトレースに使用できます。</span><span class="sxs-lookup"><span data-stu-id="b8021-108">You can also use it to trace your own code.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b8021-109">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="b8021-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b8021-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (Visual Studio 2015 でも機能します)</span><span class="sxs-lookup"><span data-stu-id="b8021-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="b8021-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="b8021-111">Web API 2</span></span>
> - [<span data-ttu-id="b8021-112">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="b8021-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="b8021-113">System.Diagnostics Web API でのトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="b8021-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="b8021-114">最初に新しい ASP.NET Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b8021-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="b8021-115">Visual Studio から、**ファイル**メニューの **新規**、し**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="b8021-115">In Visual Studio, from the **File** menu, select **New**, then **Project**.</span></span> <span data-ttu-id="b8021-116">**テンプレート**、 **Web**、 **ASP.NET Web アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="b8021-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="b8021-117">Web API プロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="b8021-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="b8021-118">**ツール**メニューの **ライブラリ パッケージ マネージャー**、し**Package Manage Console**します。</span><span class="sxs-lookup"><span data-stu-id="b8021-118">From the **Tools** menu, select **Library Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="b8021-119">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="b8021-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="b8021-120">最初のコマンドは、最新の Web API トレース パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="b8021-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="b8021-121">Core Web API のパッケージも更新されます。</span><span class="sxs-lookup"><span data-stu-id="b8021-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="b8021-122">2 番目のコマンドは、最新バージョンに WebApi.WebHost パッケージを更新します。</span><span class="sxs-lookup"><span data-stu-id="b8021-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="b8021-123">Web API の特定のバージョンを対象とする場合を使用して、バージョン フラグは、トレースのパッケージをインストールするときにします。</span><span class="sxs-lookup"><span data-stu-id="b8021-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>


<span data-ttu-id="b8021-124">アプリで WebApiConfig.cs ファイルを開く\_開始フォルダー。</span><span class="sxs-lookup"><span data-stu-id="b8021-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="b8021-125">次のコードを追加、**登録**メソッド。</span><span class="sxs-lookup"><span data-stu-id="b8021-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="b8021-126">このコードを追加、 [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx)クラスを Web API パイプライン。</span><span class="sxs-lookup"><span data-stu-id="b8021-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="b8021-127">**SystemDiagnosticsTraceWriter**クラスへのトレースを書き込みます[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)します。</span><span class="sxs-lookup"><span data-stu-id="b8021-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="b8021-128">トレースを表示するには、デバッガーでアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="b8021-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="b8021-129">ブラウザーに移動します。`/api/values`します。</span><span class="sxs-lookup"><span data-stu-id="b8021-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="b8021-130">トレース ステートメントは、Visual Studio の [出力] ウィンドウに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="b8021-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="b8021-131">(から、**ビュー**メニューの **出力**)。</span><span class="sxs-lookup"><span data-stu-id="b8021-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="b8021-132">**SystemDiagnosticsTraceWriter**トレースを書き込みます**System.Diagnostics.Trace**、別のトレース リスナーを登録することができます。 たとえば、書き込むトレース ログ ファイルにします。</span><span class="sxs-lookup"><span data-stu-id="b8021-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="b8021-133">トレース ライターの詳細については、次を参照してください。、[トレース リスナー](https://msdn.microsoft.com/library/4y5y10s7.aspx) msdn トピック。</span><span class="sxs-lookup"><span data-stu-id="b8021-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="b8021-134">SystemDiagnosticsTraceWriter を構成します。</span><span class="sxs-lookup"><span data-stu-id="b8021-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="b8021-135">次のコードでは、トレース ライターを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b8021-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="b8021-136">制御可能な 2 つの設定があります。</span><span class="sxs-lookup"><span data-stu-id="b8021-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="b8021-137">IsVerbose: false の場合、各トレースは最小限の情報を格納します。</span><span class="sxs-lookup"><span data-stu-id="b8021-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="b8021-138">True の場合、トレースには、詳細が含まれます。</span><span class="sxs-lookup"><span data-stu-id="b8021-138">If true, traces include more information.</span></span>
- <span data-ttu-id="b8021-139">MinimumLevel: 最小トレース レベルを設定します。</span><span class="sxs-lookup"><span data-stu-id="b8021-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="b8021-140">順序でのトレース レベルは、デバッグ、Info、Warn、エラー、および致命的なエラー。</span><span class="sxs-lookup"><span data-stu-id="b8021-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="b8021-141">トレースを Web API アプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="b8021-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="b8021-142">トレース ライターを追加する即時にアクセス Web API パイプラインによって作成されたトレース。</span><span class="sxs-lookup"><span data-stu-id="b8021-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="b8021-143">独自のコードをトレースにトレース ライターを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="b8021-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="b8021-144">トレース ライターを取得する**HttpConfiguration.Services.GetTraceWriter**します。</span><span class="sxs-lookup"><span data-stu-id="b8021-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="b8021-145">コント ローラーからこのメソッドはからアクセスできる、 **ApiController.Configuration**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="b8021-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="b8021-146">トレースを書き込むには、呼び出すことができます、 **ITraceWriter.Trace**メソッドを直接が、 [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx)クラスのわかりやすいは一部の拡張メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="b8021-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="b8021-147">たとえば、**情報**前に示したメソッドは、トレース レベルでトレースを作成します**情報**します。</span><span class="sxs-lookup"><span data-stu-id="b8021-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="b8021-148">Web API トレース インフラストラクチャ</span><span class="sxs-lookup"><span data-stu-id="b8021-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="b8021-149">このセクションでは、Web API のカスタム トレース ライターを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b8021-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="b8021-150">Microsoft.AspNet.WebApi.Tracing パッケージは、Web API で一般的なトレース インフラストラクチャ上に構築されます。</span><span class="sxs-lookup"><span data-stu-id="b8021-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="b8021-151">Microsoft.AspNet.WebApi.Tracing を使用せずに組み込むことも可能で他のトレース/ログイン ライブラリなど[NLog](http://nlog-project.org/)または[log4net](http://logging.apache.org/log4net/)します。</span><span class="sxs-lookup"><span data-stu-id="b8021-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/loggin library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="b8021-152">トレースを収集するには、実装、 **ITraceWriter**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="b8021-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="b8021-153">簡単な例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="b8021-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="b8021-154">**ITraceWriter.Trace**メソッドは、トレースを作成します。</span><span class="sxs-lookup"><span data-stu-id="b8021-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="b8021-155">呼び出し元には、カテゴリおよびトレース レベルを指定します。</span><span class="sxs-lookup"><span data-stu-id="b8021-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="b8021-156">カテゴリには、任意のユーザー定義文字列を指定できます。</span><span class="sxs-lookup"><span data-stu-id="b8021-156">The category can be any user-defined string.</span></span> <span data-ttu-id="b8021-157">実装**トレース**次を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8021-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="b8021-158">新規作成**TraceRecord**します。</span><span class="sxs-lookup"><span data-stu-id="b8021-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="b8021-159">ように、要求、カテゴリ、およびトレースのレベルで初期化します。</span><span class="sxs-lookup"><span data-stu-id="b8021-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="b8021-160">これらの値は、呼び出し元によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="b8021-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="b8021-161">呼び出す、 *traceAction*を委任します。</span><span class="sxs-lookup"><span data-stu-id="b8021-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="b8021-162">残りの部分を埋めるために、このデリゲート内で、呼び出し元が期待どおり、 **TraceRecord**します。</span><span class="sxs-lookup"><span data-stu-id="b8021-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="b8021-163">書き込み、 **TraceRecord**を任意のログ記録手法を使用します。</span><span class="sxs-lookup"><span data-stu-id="b8021-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="b8021-164">次に例を呼び出すだけです**System.Diagnostics.Trace**します。</span><span class="sxs-lookup"><span data-stu-id="b8021-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="b8021-165">トレース ライターの設定</span><span class="sxs-lookup"><span data-stu-id="b8021-165">Setting the Trace Writer</span></span>

<span data-ttu-id="b8021-166">トレースを有効にするを使用する Web API を構成する必要があります、 **ITraceWriter**実装します。</span><span class="sxs-lookup"><span data-stu-id="b8021-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="b8021-167">これを行う、 **HttpConfiguration**オブジェクト、次のコードに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="b8021-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="b8021-168">1 つだけのトレース ライターはアクティブにすることはできます。</span><span class="sxs-lookup"><span data-stu-id="b8021-168">Only one trace writer can be active.</span></span> <span data-ttu-id="b8021-169">既定では、Web API の設定、&quot;は no-op&quot;は何もトレースします。</span><span class="sxs-lookup"><span data-stu-id="b8021-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="b8021-170">(、&quot;は no-op&quot;トレース コードがトレース ライターがあるかどうかを確認する必要がないように、トレースが存在する**null**トレースを書き込む前にします)。</span><span class="sxs-lookup"><span data-stu-id="b8021-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="b8021-171">Web API の動作をトレースの方法</span><span class="sxs-lookup"><span data-stu-id="b8021-171">How Web API Tracing Works</span></span>

<span data-ttu-id="b8021-172">Web API の使用例の Web API を使用してトレースを*ファサード*パターン: Web API トレースを有効にすると、トレースの呼び出しを実行するクラスを使用して、要求パイプラインのさまざまな部分が折り返されます。</span><span class="sxs-lookup"><span data-stu-id="b8021-172">Tracing in Web API uses a in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="b8021-173">たとえば、コント ローラーを選択するときに、パイプラインを使用して、 **IHttpControllerSelector**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="b8021-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="b8021-174">実装するクラスを挿入する、pipleline トレースが有効になっている、 **IHttpControllerSelector**が実際の実装を通じて呼び出し。</span><span class="sxs-lookup"><span data-stu-id="b8021-174">With tracing enabled, the pipleline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Web API のトレースは、ファサード パターンを使用します。](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="b8021-176">この設計の利点は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b8021-176">The benefits of this design include:</span></span>

- <span data-ttu-id="b8021-177">トレース ライターを追加しない場合、トレーシング コンポーネントはインスタンス化されませんをパフォーマンスに与える影響はありません。</span><span class="sxs-lookup"><span data-stu-id="b8021-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="b8021-178">など、既定のサービスを置き換える場合**IHttpControllerSelector**を独自のカスタム実装とトレースが影響を受けませんトレースはラッパー オブジェクトによって行われるためです。</span><span class="sxs-lookup"><span data-stu-id="b8021-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="b8021-179">既定値を置き換えることで、独自のカスタム フレームワークで Web API トレース フレームワーク全体を置換することもできます**ITraceManager**サービス。</span><span class="sxs-lookup"><span data-stu-id="b8021-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="b8021-180">実装**ITraceManager.Initialize**トレース システムを初期化します。</span><span class="sxs-lookup"><span data-stu-id="b8021-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="b8021-181">これに置き換わることに注意してください、*全体*トレース フレームワーク、すべての Web API に組み込まれているトレース コードを含むです。</span><span class="sxs-lookup"><span data-stu-id="b8021-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
