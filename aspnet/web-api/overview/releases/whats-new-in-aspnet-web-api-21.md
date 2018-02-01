---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: "ASP.NET Web API 2.1 の新機能 |Microsoft ドキュメント"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="d6d65-102">ASP.NET Web API 2.1 の新機能</span><span class="sxs-lookup"><span data-stu-id="d6d65-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="d6d65-103">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d6d65-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="d6d65-104">このトピックでは、ASP.NET Web API 2.1 の新機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="d6d65-105">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="d6d65-105">Download</span></span>](#download)
- [<span data-ttu-id="d6d65-106">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="d6d65-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="d6d65-107">ASP.NET Web API 2.1 の新機能</span><span class="sxs-lookup"><span data-stu-id="d6d65-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="d6d65-108">グローバルのエラー処理</span><span class="sxs-lookup"><span data-stu-id="d6d65-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="d6d65-109">属性のルーティングの機能強化</span><span class="sxs-lookup"><span data-stu-id="d6d65-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="d6d65-110">ヘルプ ページ機能強化</span><span class="sxs-lookup"><span data-stu-id="d6d65-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="d6d65-111">IgnoreRoute サポート</span><span class="sxs-lookup"><span data-stu-id="d6d65-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="d6d65-112">BSON メディア タイプ フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="d6d65-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="d6d65-113">非同期のフィルターのサポートの向上</span><span class="sxs-lookup"><span data-stu-id="d6d65-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="d6d65-114">クエリのライブラリを書式設定、クライアントの解析</span><span class="sxs-lookup"><span data-stu-id="d6d65-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="d6d65-115">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="d6d65-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="d6d65-116">バグの修正</span><span class="sxs-lookup"><span data-stu-id="d6d65-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="d6d65-117">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="d6d65-117">Download</span></span>

<span data-ttu-id="d6d65-118">ランタイムの機能は、NuGet ギャラリー上の NuGet パッケージとしてリリースされています。</span><span class="sxs-lookup"><span data-stu-id="d6d65-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="d6d65-119">次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様です。</span><span class="sxs-lookup"><span data-stu-id="d6d65-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="d6d65-120">ASP.NET Web API 2.1 RTM の最新のパッケージは、次のバージョン:「5.1.2」です。</span><span class="sxs-lookup"><span data-stu-id="d6d65-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="d6d65-121">インストールまたはを介してこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)です。</span><span class="sxs-lookup"><span data-stu-id="d6d65-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="d6d65-122">リリースには、NuGet で対応するローカライズ版パッケージも含まれています。</span><span class="sxs-lookup"><span data-stu-id="d6d65-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="d6d65-123">インストールまたは NuGet パッケージ マネージャー コンソールを使用してリリースされた NuGet パッケージを更新できます。</span><span class="sxs-lookup"><span data-stu-id="d6d65-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="d6d65-124">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="d6d65-124">Documentation</span></span>

<span data-ttu-id="d6d65-125">チュートリアルと ASP.NET Web API 2.1 RTM に関する他の情報は、ASP.NET web サイトから使用可能な ([https://www.asp.net/web-api](../../index.md))。</span><span class="sxs-lookup"><span data-stu-id="d6d65-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="d6d65-126">ASP.NET Web API 2.1 の新機能</span><span class="sxs-lookup"><span data-stu-id="d6d65-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="d6d65-127">グローバルのエラー処理</span><span class="sxs-lookup"><span data-stu-id="d6d65-127">Global Error Handling</span></span>

<span data-ttu-id="d6d65-128">中央の 1 つのメカニズムにより、すべての未処理の例外を記録ようになりましたことができ、未処理の例外の動作をカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="d6d65-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="d6d65-129">フレームワークは、未処理の例外とするなどの発生時に処理される要求のコンテキストに関する情報を表示する複数の例外ロガーをサポートします。</span><span class="sxs-lookup"><span data-stu-id="d6d65-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="d6d65-130">たとえば、次のコードは、すべてのハンドルされない例外の記録に System.Diagnostics.TraceSource を使用します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="d6d65-131">既定の例外ハンドラーを置き換えることもが実行されるように、未処理の例外時に送信される HTTP 応答メッセージを完全にカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="d6d65-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="d6d65-132">提供されており、[サンプル](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt)人気のある ELMAH フレームワークを使用してすべての未処理の例外をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="d6d65-133">属性のルーティングの機能強化</span><span class="sxs-lookup"><span data-stu-id="d6d65-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="d6d65-134">属性のようになりましたルーティング、バージョン管理とヘッダー ベースのルートの選択を有効にすると、制約がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="d6d65-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="d6d65-135">また、属性ルートの多くの側面を使用してカスタマイズ可能なようになりましたが、 **IDirectRouteFactory**インターフェイスと**RouteFactoryAttribute**クラスです。</span><span class="sxs-lookup"><span data-stu-id="d6d65-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="d6d65-136">ルート プレフィックスが経由で拡張可能なようになりました、 **IRoutePrefix**インターフェイスと**RoutePrefixAttribute**クラスです。</span><span class="sxs-lookup"><span data-stu-id="d6d65-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="d6d65-137">提供されており、[サンプル](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt)で 'api-version' HTTP ヘッダーでコント ローラーを動的にフィルター処理する制約を使用します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="d6d65-138">ヘルプ ページ機能強化</span><span class="sxs-lookup"><span data-stu-id="d6d65-138">Help Page Improvements</span></span>

<span data-ttu-id="d6d65-139">Web API 2.1 に次の機能強化が含まれています[API ヘルプ ページ](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="d6d65-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="d6d65-140">アクションのパラメーターまたは戻り値の個々 のプロパティのドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="d6d65-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="d6d65-141">データ モデルの注釈のドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="d6d65-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="d6d65-142">ヘルプ ページの UI のデザインも更新され、これらの変更に合わせてです。</span><span class="sxs-lookup"><span data-stu-id="d6d65-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="d6d65-143">IgnoreRoute サポート</span><span class="sxs-lookup"><span data-stu-id="d6d65-143">IgnoreRoute Support</span></span>

<span data-ttu-id="d6d65-144">Web API 2.1 サポートする一連の Web API、ルーティングの URL パターンを無視して**IgnoreRoute**拡張メソッド**HttpRouteCollection**です。</span><span class="sxs-lookup"><span data-stu-id="d6d65-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="d6d65-145">これらのメソッドを指定したテンプレートに一致するすべての Url を無視する Web API が発生し、該当する場合、追加の処理を適用するホストを許可します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="d6d65-146">次の例で始まる Uri は無視されます、&quot;コンテンツ&quot;セグメント。</span><span class="sxs-lookup"><span data-stu-id="d6d65-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="d6d65-147">BSON メディア タイプ フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="d6d65-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="d6d65-148">Web API をサポート、 [BSON](http://bsonspec.org/)ワイヤ形式をクライアントとサーバーの両方です。</span><span class="sxs-lookup"><span data-stu-id="d6d65-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="d6d65-149">BSON をサーバー側で有効にする、 **BsonMediaTypeFormatter**フォーマッタのコレクションに。</span><span class="sxs-lookup"><span data-stu-id="d6d65-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="d6d65-150">.NET クライアントが BSON 形式を使用する方法を次に示します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="d6d65-151">提供されており、[サンプル](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt)クライアントとサーバーの両方の側を表示します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="d6d65-152">詳細については、次を参照してください[Web API 2.1 で BSON サポート。](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="d6d65-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="d6d65-153">非同期のフィルターのサポートの向上</span><span class="sxs-lookup"><span data-stu-id="d6d65-153">Better Support for Async Filters</span></span>

<span data-ttu-id="d6d65-154">Web API には、非同期的に実行するフィルターを作成する簡単な方法がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="d6d65-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="d6d65-155">この機能は役立ちますが、フィルターは、データベース アクセスなど、非同期操作を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d6d65-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="d6d65-156">以前は、非同期フィルターを作成する必要がありましたインターフェイスを実装する、フィルター、フィルターの基本クラスにのみ同期メソッドが公開されているため。</span><span class="sxs-lookup"><span data-stu-id="d6d65-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="d6d65-157">これで、仮想オーバーライドできます`On*Async`フィルターのメソッドの基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="d6d65-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="d6d65-158">例:</span><span class="sxs-lookup"><span data-stu-id="d6d65-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="d6d65-159">**AuthorizationFilterAttribute**、 **ActionFilterAttribute**、および**ExceptionFilterAttribute**すべてのクラスは、Web API 2.1 で非同期をサポートします。</span><span class="sxs-lookup"><span data-stu-id="d6d65-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="d6d65-160">クエリのライブラリを書式設定、クライアントの解析</span><span class="sxs-lookup"><span data-stu-id="d6d65-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="d6d65-161">以前は、 **System.Net.Http.Formatting**解析およびサーバー側コードでは、URI のクエリの更新がサポートされていますが、同等のポータブル ライブラリでこの機能がありません。</span><span class="sxs-lookup"><span data-stu-id="d6d65-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="d6d65-162">2.1 では Web API、クライアント アプリケーションに簡単に解析をできますクエリ文字列を更新します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="d6d65-163">次の例では、解析、変更、および URI、クエリを生成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="d6d65-164">(例は、わかりやすくするためのコンソール アプリケーションを表示します)。</span><span class="sxs-lookup"><span data-stu-id="d6d65-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="d6d65-165">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="d6d65-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="d6d65-166">このセクションでは、既知の問題と、ASP.NET Web API 2.1 RTM で重大な変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="d6d65-167">属性のルーティング</span><span class="sxs-lookup"><span data-stu-id="d6d65-167">Attribute Routing</span></span>

<span data-ttu-id="d6d65-168">属性のルーティングの一致のあいまいさは、最初の一致を選択するのではなく、エラーを報告します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="d6d65-169">属性ルートが使用を禁止して、 *{controller}*パラメーターを使用してと、 *{controller}*ルートのパラメーターは、アクションに配置します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="d6d65-170">これらのパラメーターと、あいまいさと非常に高いが発生します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="d6d65-171">5.0 パッケージをプロジェクトに既に存在しないもので 5.1 パッケージの結果を含むプロジェクトに MVC または Web API のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="d6d65-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="d6d65-172">ASP.NET Web API 2.1 RTM の NuGet パッケージを更新しても、ASP.NET スキャフォールディングなど、Visual Studio のツールや ASP.NET Web アプリケーション プロジェクト テンプレートには更新されません。</span><span class="sxs-lookup"><span data-stu-id="d6d65-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="d6d65-173">ASP.NET ランタイム パッケージ (5.0.0.0) の以前のバージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="d6d65-174">プロジェクトで使用されていない場合に、ASP.NET スキャフォールディングでその結果、必要なパッケージの以前のバージョン (5.0.0.0) がインストールされますか。</span><span class="sxs-lookup"><span data-stu-id="d6d65-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="d6d65-175">ただし、Visual Studio 2013 RTM または更新プログラム 1 で ASP.NET スキャフォールディングでは、プロジェクト内の最新のパッケージは上書きされません。</span><span class="sxs-lookup"><span data-stu-id="d6d65-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="d6d65-176">Web API 2.1 または ASP.NET MVC 5.1 にパッケージを更新した後に ASP.NET のスキャフォールディングを使用する場合は、Web API と MVC のバージョンが一致を確認します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="d6d65-177">型名の変更</span><span class="sxs-lookup"><span data-stu-id="d6d65-177">Type Renames</span></span>

<span data-ttu-id="d6d65-178">RC から 2.1 RTM に属性のルーティング拡張機能を使用する型の一部が変更されました。</span><span class="sxs-lookup"><span data-stu-id="d6d65-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="d6d65-179">古い型の名前 (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="d6d65-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="d6d65-180">新しい型名 (2.1 RTM)</span><span class="sxs-lookup"><span data-stu-id="d6d65-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="d6d65-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="d6d65-181">IDirectRouteProvider</span></span> | <span data-ttu-id="d6d65-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="d6d65-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="d6d65-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="d6d65-183">RouteProviderAttribute</span></span> | <span data-ttu-id="d6d65-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="d6d65-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="d6d65-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="d6d65-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="d6d65-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="d6d65-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="d6d65-187">例外フィルターが非同期操作でスローされた例外の集約のラップを解除できません。</span><span class="sxs-lookup"><span data-stu-id="d6d65-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="d6d65-188">以前は、非同期アクションがスローされた場合、 **AggregateException**、例外フィルターは、例外をラップ解除および**OnException**基本例外を取得します。</span><span class="sxs-lookup"><span data-stu-id="d6d65-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="d6d65-189">2.1 では、例外フィルターがラップ解除しません、および**OnException**取得元**AggregateException**です。</span><span class="sxs-lookup"><span data-stu-id="d6d65-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="d6d65-190">バグ修正</span><span class="sxs-lookup"><span data-stu-id="d6d65-190">Bug Fixes</span></span>

<span data-ttu-id="d6d65-191">このリリースでは、いくつかのバグ修正も含まれます。</span><span class="sxs-lookup"><span data-stu-id="d6d65-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="d6d65-192">完全な一覧は、ここをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d6d65-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="d6d65-193">5.1.0 パッケージ</span><span class="sxs-lookup"><span data-stu-id="d6d65-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="d6d65-194">5.1.1 パッケージ</span><span class="sxs-lookup"><span data-stu-id="d6d65-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="d6d65-195">5.1.2 パッケージには、IntelliSense の更新がないバグの修正が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d6d65-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
