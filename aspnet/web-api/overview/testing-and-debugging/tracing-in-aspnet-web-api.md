---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: "ASP.NET Web API 2 のトレース |Microsoft ドキュメント"
author: MikeWasson
description: "ASP.NET Web API でのトレースを有効にする方法を示します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f35c8a10018ce796e2d905d6ee839ff09bb380a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="593b3-103">ASP.NET Web API 2 のトレース</span><span class="sxs-lookup"><span data-stu-id="593b3-103">Tracing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="593b3-104">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="593b3-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="593b3-105">Web ベース アプリケーションをデバッグするときに、トレース ログの適切なセットを代わりにはありません。</span><span class="sxs-lookup"><span data-stu-id="593b3-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="593b3-106">このチュートリアルでは、ASP.NET Web API でのトレースを有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="593b3-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="593b3-107">この機能を使用すると、どのような Web API フレームワークは、コント ローラーを呼び出す前後にトレースされます。</span><span class="sxs-lookup"><span data-stu-id="593b3-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="593b3-108">また、独自のコードをトレースに使用することができます。</span><span class="sxs-lookup"><span data-stu-id="593b3-108">You can also use it to trace your own code.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="593b3-109">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="593b3-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="593b3-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (Visual Studio 2015 でも動作します)</span><span class="sxs-lookup"><span data-stu-id="593b3-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="593b3-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="593b3-111">Web API 2</span></span>
> - [<span data-ttu-id="593b3-112">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="593b3-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="593b3-113">System.Diagnostics Web API のトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="593b3-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="593b3-114">最初に、新しい ASP.NET Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="593b3-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="593b3-115">Visual Studio から、**ファイル**メニューの **新規**、し**プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="593b3-115">In Visual Studio, from the **File** menu, select **New**, then **Project**.</span></span> <span data-ttu-id="593b3-116">**テンプレート**、 **Web** **ASP.NET Web アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="593b3-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="593b3-117">Web API プロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="593b3-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="593b3-118">**ツール**メニューの **ライブラリ パッケージ マネージャー**、し**パッケージの管理コンソール**です。</span><span class="sxs-lookup"><span data-stu-id="593b3-118">From the **Tools** menu, select **Library Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="593b3-119">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="593b3-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="593b3-120">最初のコマンドは、最新の Web API トレース パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="593b3-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="593b3-121">コア Web API パッケージも更新します。</span><span class="sxs-lookup"><span data-stu-id="593b3-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="593b3-122">2 番目のコマンドは、最新バージョンに WebApi.WebHost パッケージを更新します。</span><span class="sxs-lookup"><span data-stu-id="593b3-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="593b3-123">Web API の特定のバージョンを対象とする場合は、バージョン フラグは、トレースのパッケージをインストールするときにします。</span><span class="sxs-lookup"><span data-stu-id="593b3-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>


<span data-ttu-id="593b3-124">アプリで WebApiConfig.cs ファイルを開く\_開始フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="593b3-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="593b3-125">次のコードを追加、**登録**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="593b3-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="593b3-126">このコードを追加、 [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx)クラスが Web API パイプラインをします。</span><span class="sxs-lookup"><span data-stu-id="593b3-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="593b3-127">**SystemDiagnosticsTraceWriter**クラスをトレースに書き込みます[System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace)です。</span><span class="sxs-lookup"><span data-stu-id="593b3-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="593b3-128">トレースを表示するには、デバッガーでアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="593b3-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="593b3-129">ブラウザーに移動`/api/values`です。</span><span class="sxs-lookup"><span data-stu-id="593b3-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="593b3-130">トレース ステートメントは、Visual Studio では、出力ウィンドウに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="593b3-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="593b3-131">(から、**ビュー**メニューの **出力**)。</span><span class="sxs-lookup"><span data-stu-id="593b3-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="593b3-132">**SystemDiagnosticsTraceWriter**トレースを書き込みます**System.Diagnostics.Trace**、別のトレース リスナーを登録することができます。 たとえば、書き込むトレース ログ ファイルにします。</span><span class="sxs-lookup"><span data-stu-id="593b3-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="593b3-133">トレース ライターの詳細については、次を参照してください。、[トレース リスナー](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx) MSDN のトピックです。</span><span class="sxs-lookup"><span data-stu-id="593b3-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="593b3-134">SystemDiagnosticsTraceWriter を構成します。</span><span class="sxs-lookup"><span data-stu-id="593b3-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="593b3-135">次のコードでは、トレース ライターを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="593b3-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="593b3-136">制御できる 2 つの設定があります。</span><span class="sxs-lookup"><span data-stu-id="593b3-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="593b3-137">IsVerbose: false の場合、各トレースは最小限の情報を格納します。</span><span class="sxs-lookup"><span data-stu-id="593b3-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="593b3-138">True の場合、トレースは、追加情報を含めます。</span><span class="sxs-lookup"><span data-stu-id="593b3-138">If true, traces include more information.</span></span>
- <span data-ttu-id="593b3-139">MinimumLevel: 最小トレース レベルを設定します。</span><span class="sxs-lookup"><span data-stu-id="593b3-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="593b3-140">順序でのトレース レベルは、デバッグ、Info、Warn、Error、および致命的なエラーです。</span><span class="sxs-lookup"><span data-stu-id="593b3-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="593b3-141">トレースを Web API アプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="593b3-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="593b3-142">トレース ライターを追加することにより、即時にアクセス Web API パイプラインによって作成されたトレースです。</span><span class="sxs-lookup"><span data-stu-id="593b3-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="593b3-143">独自のコードを追跡するのにトレース ライターを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="593b3-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="593b3-144">トレース ライターを取得する**HttpConfiguration.Services.GetTraceWriter**です。</span><span class="sxs-lookup"><span data-stu-id="593b3-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="593b3-145">このメソッドは、経由でアクセスできる、コント ローラーから、 **ApiController.Configuration**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="593b3-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="593b3-146">トレースを作成するに呼び出せる、 **ITraceWriter.Trace**メソッドを直接、ですが、 [ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx)クラスは、わかりやすいは一部の拡張メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="593b3-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="593b3-147">たとえば、**情報**前に示したメソッドは、トレース レベルでトレースを作成**情報**です。</span><span class="sxs-lookup"><span data-stu-id="593b3-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="593b3-148">Web API トレース インフラストラクチャ</span><span class="sxs-lookup"><span data-stu-id="593b3-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="593b3-149">このセクションでは、Web API のカスタム トレース ライターを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="593b3-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="593b3-150">Microsoft.AspNet.WebApi.Tracing パッケージは、Web API の一般的なトレース インフラストラクチャに構築されます。</span><span class="sxs-lookup"><span data-stu-id="593b3-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="593b3-151">Microsoft.AspNet.WebApi.Tracing を使用する代わりにも接続できる他のトレース/ログイン ライブラリでなど[NLog](http://nlog-project.org/)または[log4net](http://logging.apache.org/log4net/)です。</span><span class="sxs-lookup"><span data-stu-id="593b3-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/loggin library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="593b3-152">トレースを収集するには、実装、 **ITraceWriter**インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="593b3-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="593b3-153">単純な例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="593b3-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="593b3-154">**ITraceWriter.Trace**メソッドは、トレースを作成します。</span><span class="sxs-lookup"><span data-stu-id="593b3-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="593b3-155">呼び出し元は、カテゴリおよびトレース レベルを指定します。</span><span class="sxs-lookup"><span data-stu-id="593b3-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="593b3-156">カテゴリは、任意のユーザー定義の文字列にすることができます。</span><span class="sxs-lookup"><span data-stu-id="593b3-156">The category can be any user-defined string.</span></span> <span data-ttu-id="593b3-157">実装**トレース**次を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="593b3-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="593b3-158">新しい**TraceRecord**です。</span><span class="sxs-lookup"><span data-stu-id="593b3-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="593b3-159">ように、要求、カテゴリ、およびトレース レベルで初期化します。</span><span class="sxs-lookup"><span data-stu-id="593b3-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="593b3-160">これらの値は、呼び出し元によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="593b3-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="593b3-161">呼び出す、 *traceAction*を委任します。</span><span class="sxs-lookup"><span data-stu-id="593b3-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="593b3-162">このデリゲート内の残りの部分を埋めるために、呼び出し元が必要、 **TraceRecord**です。</span><span class="sxs-lookup"><span data-stu-id="593b3-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="593b3-163">書き込み、 **TraceRecord**、したい任意のログ記録手法を使用します。</span><span class="sxs-lookup"><span data-stu-id="593b3-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="593b3-164">次に例を呼び出すだけ**System.Diagnostics.Trace**です。</span><span class="sxs-lookup"><span data-stu-id="593b3-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="593b3-165">トレース ライターの設定</span><span class="sxs-lookup"><span data-stu-id="593b3-165">Setting the Trace Writer</span></span>

<span data-ttu-id="593b3-166">トレースを有効にするには、Web API を使用するを構成する必要があります、 **ITraceWriter**実装します。</span><span class="sxs-lookup"><span data-stu-id="593b3-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="593b3-167">これを行う、 **HttpConfiguration**オブジェクトを次のコードに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="593b3-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="593b3-168">1 つだけのトレース ライターは、アクティブにできます。</span><span class="sxs-lookup"><span data-stu-id="593b3-168">Only one trace writer can be active.</span></span> <span data-ttu-id="593b3-169">既定では、Web API の設定、 &quot;、no-op&quot;は何もトレースします。</span><span class="sxs-lookup"><span data-stu-id="593b3-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="593b3-170">(、 &quot;、No-op&quot;トレース コードがトレース ライターがあるかどうかを確認する必要がないように、トレースが存在する**null**トレースを書き込む前にします)。</span><span class="sxs-lookup"><span data-stu-id="593b3-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="593b3-171">どのように Web API の動作をトレース</span><span class="sxs-lookup"><span data-stu-id="593b3-171">How Web API Tracing Works</span></span>

<span data-ttu-id="593b3-172">Web API の使用例の Web API を使用してトレースを*ファサード*パターン: トレースが有効になっている場合は、Web API は、trace の呼び出しを実行するクラスを持つ要求パイプラインのさまざまな部分で折り返されます。 します。</span><span class="sxs-lookup"><span data-stu-id="593b3-172">Tracing in Web API uses a in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="593b3-173">たとえば、コント ローラーを選択すると、パイプラインを使用して、 **IHttpControllerSelector**インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="593b3-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="593b3-174">実装するクラスを挿入する、pipleline トレースを有効になっているに**IHttpControllerSelector**が実際の実装を通じて呼び出し。</span><span class="sxs-lookup"><span data-stu-id="593b3-174">With tracing enabled, the pipleline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Web API トレースは、ファサード パターンを使用します。](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="593b3-176">このデザインの利点は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="593b3-176">The benefits of this design include:</span></span>

- <span data-ttu-id="593b3-177">トレース ライターを追加しないと、トレースは、コンポーネントはインスタンス化されていないと、パフォーマンスの影響を与えるありません。</span><span class="sxs-lookup"><span data-stu-id="593b3-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="593b3-178">など、既定のサービスを置き換える場合は、 **IHttpControllerSelector**独自のカスタム実装とトレースは影響しません、トレースは、ラッパー オブジェクトによって行われるためです。</span><span class="sxs-lookup"><span data-stu-id="593b3-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="593b3-179">既定値に置き換えることで、独自のカスタム framework では、Web API トレース フレームワーク全体を置換することもできます**ITraceManager**サービス。</span><span class="sxs-lookup"><span data-stu-id="593b3-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="593b3-180">実装**ITraceManager.Initialize**トレースは、システムを初期化します。</span><span class="sxs-lookup"><span data-stu-id="593b3-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="593b3-181">これが置き換えられるに注意してください、*全体*のすべての Web API に組み込まれているトレース コードを含むトレース フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="593b3-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
