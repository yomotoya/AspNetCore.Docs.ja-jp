---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: ASP.NET では、しないとは |Microsoft Docs
author: Rick-Anderson
description: このトピックでは、ASP.NET web プロジェクト内でユーザーが、いくつかの一般的なミスをについて説明します。 これらの共通を回避するために行う必要がありますの推奨事項を提供しています.
ms.author: riande
ms.date: 05/08/2014
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 69040ca6a1ddeaf029062da45475dd2171b1afa6
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021444"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="061e2-104">ASP.NET では、しないとは</span><span class="sxs-lookup"><span data-stu-id="061e2-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="061e2-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="061e2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="061e2-106">このトピックでは、ASP.NET web プロジェクト内でユーザーが、いくつかの一般的なミスをについて説明します。</span><span class="sxs-lookup"><span data-stu-id="061e2-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="061e2-107">これらの一般的な誤りを回避するために行う必要がありますの推奨事項を提供します。</span><span class="sxs-lookup"><span data-stu-id="061e2-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="061e2-108">基にして、[プレゼンテーション](http://vimeo.com/68390507)によって**Damian Edwards** Norwegian Developers Conference でします。</span><span class="sxs-lookup"><span data-stu-id="061e2-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="061e2-109">免責情報</span><span class="sxs-lookup"><span data-stu-id="061e2-109">Disclaimer</span></span>

<span data-ttu-id="061e2-110">このトピックではありませんの完全なガイドとして、アプリケーションが安全かつ効率的なことを確認します。</span><span class="sxs-lookup"><span data-stu-id="061e2-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="061e2-111">このトピックに記載されていないいるセキュリティとパフォーマンスのベスト プラクティスに従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="061e2-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="061e2-112">.NET クラスとプロセスに関連する一般的なミスを回避する方法のみお勧めします。</span><span class="sxs-lookup"><span data-stu-id="061e2-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="061e2-113">概要</span><span class="sxs-lookup"><span data-stu-id="061e2-113">Overview</span></span>

<span data-ttu-id="061e2-114">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="061e2-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="061e2-115">標準への準拠</span><span class="sxs-lookup"><span data-stu-id="061e2-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="061e2-116">コントロール アダプター</span><span class="sxs-lookup"><span data-stu-id="061e2-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="061e2-117">コントロールのスタイル プロパティ</span><span class="sxs-lookup"><span data-stu-id="061e2-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="061e2-118">ページおよびコントロールのコールバック</span><span class="sxs-lookup"><span data-stu-id="061e2-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="061e2-119">ブラウザー機能の検出</span><span class="sxs-lookup"><span data-stu-id="061e2-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="061e2-120">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="061e2-120">Security</span></span>](#security)

    - [<span data-ttu-id="061e2-121">要求の検証</span><span class="sxs-lookup"><span data-stu-id="061e2-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="061e2-122">Cookieless フォーム認証とセッション</span><span class="sxs-lookup"><span data-stu-id="061e2-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="061e2-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="061e2-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="061e2-124">中程度の信頼</span><span class="sxs-lookup"><span data-stu-id="061e2-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="061e2-125">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="061e2-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="061e2-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="061e2-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="061e2-127">信頼性とパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="061e2-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="061e2-128">PreSendRequestHeaders と PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="061e2-128">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="061e2-129">Web フォームでの非同期ページ イベント</span><span class="sxs-lookup"><span data-stu-id="061e2-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="061e2-130">ファイア アンド フォーゲット作業</span><span class="sxs-lookup"><span data-stu-id="061e2-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="061e2-131">要求エンティティ本体</span><span class="sxs-lookup"><span data-stu-id="061e2-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="061e2-132">Response.Redirect と Response.End</span><span class="sxs-lookup"><span data-stu-id="061e2-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="061e2-133">EnableViewState およびに ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="061e2-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="061e2-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="061e2-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="061e2-135">長い実行されている要求 (> 110 秒)</span><span class="sxs-lookup"><span data-stu-id="061e2-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="061e2-136">標準への準拠</span><span class="sxs-lookup"><span data-stu-id="061e2-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="061e2-137">コントロール アダプター</span><span class="sxs-lookup"><span data-stu-id="061e2-137">Control Adapters</span></span>

<span data-ttu-id="061e2-138">: 推奨事項は、アダプティブ レンダリングは、コントロール アダプターを使用してを停止し、CSS メディア クエリと標準に準拠した HTML を使用します。</span><span class="sxs-lookup"><span data-stu-id="061e2-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="061e2-139">コントロール アダプターは、さまざまなデバイスや環境向けにカスタマイズされたプレゼンテーション コードを表示するために .NET 2.0 で導入されました。</span><span class="sxs-lookup"><span data-stu-id="061e2-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="061e2-140">ここで、CSS、HTML によるこのアダプティブ レンダリングを実現できます。</span><span class="sxs-lookup"><span data-stu-id="061e2-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="061e2-141">コントロール アダプターの使用を停止して、CSS、HTML、既存のアダプターを変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="061e2-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="061e2-142">詳細については、次を参照してください。[メディア クエリ](http://www.w3.org/TR/css3-mediaqueries/)と[How To: Your ASP.NET Web フォームのモバイル ページの追加/MVC アプリケーション](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="061e2-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="061e2-143">コントロールのスタイル プロパティ</span><span class="sxs-lookup"><span data-stu-id="061e2-143">Style Properties on Controls</span></span>

<span data-ttu-id="061e2-144">推奨事項: は、コントロールのマークアップにスタイル値の設定を停止し、代わりに CSS スタイル シートの書式設定の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="061e2-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="061e2-145">Web サーバー コントロールには、多数行のスタイル プロパティを設定するために使用できるプロパティにはが含まれます。</span><span class="sxs-lookup"><span data-stu-id="061e2-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="061e2-146">たとえば、前景色プロパティは、コントロールのテキストの色を設定します。</span><span class="sxs-lookup"><span data-stu-id="061e2-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="061e2-147">CSS スタイル シートをより効率的にこれと同じ効果を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="061e2-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="061e2-148">スタイル シートを使用すると、スタイルの値を一元化し、アプリケーション全体でこれらの値を設定しないでください。</span><span class="sxs-lookup"><span data-stu-id="061e2-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="061e2-149">次の例は、CSS クラスを赤にセットのテキストを示します。</span><span class="sxs-lookup"><span data-stu-id="061e2-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="061e2-150">次の例では、CSS クラスを動的に適用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="061e2-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="061e2-151">ページおよびコントロールのコールバック</span><span class="sxs-lookup"><span data-stu-id="061e2-151">Page and Control Callbacks</span></span>

<span data-ttu-id="061e2-152">: 推奨事項は、ページおよびコントロールのコールバックを使用してを停止し、代わりに、次のいずれかを使用します。 AJAX、UpdatePanel、MVC アクション メソッド、Web API、または SignalR。</span><span class="sxs-lookup"><span data-stu-id="061e2-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="061e2-153">以前のバージョンの ASP.NET では、ページおよびコントロールのコールバック メソッドには、ページ全体を更新することがなく、web ページの一部を更新することが有効になります。</span><span class="sxs-lookup"><span data-stu-id="061e2-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="061e2-154">部分ページ更新を行うことができますようになりました[AJAX](../../../ajax/index.md)、 [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx)、 [MVC](../../../mvc/index.md)、 [Web API](../../../web-api/index.md)または[SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="061e2-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="061e2-155">フレンドリな Url で問題が生じるため、コールバック メソッドを使用して、ルーティングを停止する必要があります。</span><span class="sxs-lookup"><span data-stu-id="061e2-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="061e2-156">既定では、コントロールがコールバック メソッドでは、有効にしないが、コントロールでは、この機能を有効にした場合無効する必要があります。</span><span class="sxs-lookup"><span data-stu-id="061e2-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="061e2-157">ブラウザー機能の検出</span><span class="sxs-lookup"><span data-stu-id="061e2-157">Browser Capability Detection</span></span>

<span data-ttu-id="061e2-158">推奨事項: は、静的ブラウザー機能の検出を使用してを停止し、代わりに動的機能検出を使用します。</span><span class="sxs-lookup"><span data-stu-id="061e2-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="061e2-159">以前のバージョンの ASP.NET では、各ブラウザーでサポートされている機能は、XML ファイルに保存されていました。</span><span class="sxs-lookup"><span data-stu-id="061e2-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="061e2-160">静的な参照を通じて検出機能のサポートは、最適な方法ではありません。</span><span class="sxs-lookup"><span data-stu-id="061e2-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="061e2-161">ここで、動的に検出できますなどの機能検出フレームワークを使用して、ブラウザーのサポート機能[Modernizr](http://modernizr.com/)します。</span><span class="sxs-lookup"><span data-stu-id="061e2-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="061e2-162">機能検出は、メソッドまたはプロパティを使用しようとして、ブラウザーで、目的の結果が生成されるかどうかはどうかを確認し、サポートを判断します。</span><span class="sxs-lookup"><span data-stu-id="061e2-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="061e2-163">既定では、Modernizr は、Web アプリケーション テンプレートに含まれます。</span><span class="sxs-lookup"><span data-stu-id="061e2-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="061e2-164">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="061e2-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="061e2-165">要求の検証</span><span class="sxs-lookup"><span data-stu-id="061e2-165">Request Validation</span></span>

<span data-ttu-id="061e2-166">推奨事項: は、ユーザー入力を検証し、ユーザーからの出力をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="061e2-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="061e2-167">要求の検証は、各要求を検査し、見かけ上の脅威が見つかった場合は、要求を停止します。 ASP.NET の機能です。</span><span class="sxs-lookup"><span data-stu-id="061e2-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="061e2-168">クロスサイト スクリプティング攻撃に対してアプリケーションを保護するための要求の検証に依存しません。</span><span class="sxs-lookup"><span data-stu-id="061e2-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="061e2-169">代わりに、ユーザーからのすべての入力を検証し、出力をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="061e2-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="061e2-170">まれに、正規表現を使用して、入力を検証することができますが、値と一致する場合を決定する .NET クラスを使用してユーザー入力を検証する必要がありますより複雑な場合に、値が許可されています。</span><span class="sxs-lookup"><span data-stu-id="061e2-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="061e2-171">次の例では、Uri クラスの静的メソッドを使用して、ユーザーによって提供された Uri が有効かどうかを判断する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="061e2-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="061e2-172">ただし、Uri を十分に確認する必要がありますもチェックを指定するかどうかを確認する`http`または`https`します。</span><span class="sxs-lookup"><span data-stu-id="061e2-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="061e2-173">次の例では、インスタンス メソッドを使用して、Uri が有効であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="061e2-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="061e2-174">ユーザー入力を HTML としてレンダリングまたはユーザーの入力を含む SQL クエリで、前に、悪意のあるコードが含まれていないことを確認する値をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="061e2-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="061e2-175">HTML のできますマークアップで値をエンコード、 &lt;%: %&gt;構文を次に示すよう。</span><span class="sxs-lookup"><span data-stu-id="061e2-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="061e2-176">Razor 構文では、HTML ことができます。 または、を使用したエンコード @ 以下に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="061e2-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="061e2-177">例を次に示しますを HTML エンコード方法、分離コードでの値。</span><span class="sxs-lookup"><span data-stu-id="061e2-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="061e2-178">SQL コマンドの値を安全にエンコードする場合などのコマンド パラメーターを使用して、 [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="061e2-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="061e2-179">Cookieless フォーム認証とセッション</span><span class="sxs-lookup"><span data-stu-id="061e2-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="061e2-180">推奨事項には、cookie が必要です。</span><span class="sxs-lookup"><span data-stu-id="061e2-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="061e2-181">クエリ文字列での認証情報を渡すことは安全ではありません。</span><span class="sxs-lookup"><span data-stu-id="061e2-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="061e2-182">アプリケーションに認証が含まれているため、cookie が必要です。</span><span class="sxs-lookup"><span data-stu-id="061e2-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="061e2-183">Cookie は、機密情報を保存する場合は、クッキーに対して SSL を要求することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="061e2-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="061e2-184">次の例では、フォーム認証が SSL 経由で送信される cookie が必要である、Web.config ファイルで指定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="061e2-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="061e2-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="061e2-185">EnableViewStateMac</span></span>

<span data-ttu-id="061e2-186">推奨事項: 決して false に設定します。</span><span class="sxs-lookup"><span data-stu-id="061e2-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="061e2-187">既定で EnbableViewStateMac が設定を true にします。</span><span class="sxs-lookup"><span data-stu-id="061e2-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="061e2-188">アプリケーションがビュー ステートを使用していない場合でも EnableViewStateMac を false に設定しないでください。</span><span class="sxs-lookup"><span data-stu-id="061e2-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="061e2-189">この値を false に設定すると、アプリケーションがクロスサイト スクリプティングに対する脆弱性。</span><span class="sxs-lookup"><span data-stu-id="061e2-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="061e2-190">ASP.NET 4.5.2 以降では、ランタイムが適用**EnableViewStateMac は true を =** します。</span><span class="sxs-lookup"><span data-stu-id="061e2-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="061e2-191">False に設定すると、場合でも、ランタイムはこの値は無視され、true に設定された値が続行されます。</span><span class="sxs-lookup"><span data-stu-id="061e2-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="061e2-192">詳細については、次を参照してください。 [ASP.NET 4.5.2 および EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="061e2-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="061e2-193">次の例では、EnableViewStateMac を true に設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="061e2-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="061e2-194">実際には既定で true が true には、この値を設定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="061e2-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="061e2-195">ただし、設定した場合が false に任意のページで、アプリケーションで、この値をすぐに修正する必要があります。</span><span class="sxs-lookup"><span data-stu-id="061e2-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="061e2-196">中程度の信頼</span><span class="sxs-lookup"><span data-stu-id="061e2-196">Medium Trust</span></span>

<span data-ttu-id="061e2-197">推奨事項に依存しない中程度の信頼 (または他の信頼レベル) セキュリティ境界として。</span><span class="sxs-lookup"><span data-stu-id="061e2-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="061e2-198">部分信頼では、アプリケーションを適切に保護できないは使用できません。</span><span class="sxs-lookup"><span data-stu-id="061e2-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="061e2-199">代わりに、完全な信頼を使用し、個別のアプリケーション プールで信頼されていないアプリケーションを分離します。</span><span class="sxs-lookup"><span data-stu-id="061e2-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="061e2-200">また、一意の id では、各アプリケーション プールを実行します。</span><span class="sxs-lookup"><span data-stu-id="061e2-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="061e2-201">詳細については、次を参照してください。 [ASP.NET 部分信頼でアプリケーションの分離が保証されない](https://support.microsoft.com/kb/2698981)します。</span><span class="sxs-lookup"><span data-stu-id="061e2-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="061e2-202">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="061e2-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="061e2-203">推奨事項: セキュリティ設定を無効にしない&lt;appSettings&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="061e2-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="061e2-204">AppSettings 要素には、セキュリティ更新プログラムの必要な多くの値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="061e2-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="061e2-205">変更または、これらの値を無効にする必要がありますされません。</span><span class="sxs-lookup"><span data-stu-id="061e2-205">You should not change or disable these values.</span></span> <span data-ttu-id="061e2-206">場合は、更新プログラムを展開するときに、これらの値を無効にする必要があります、すぐに再有効化展開が完了後します。</span><span class="sxs-lookup"><span data-stu-id="061e2-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="061e2-207">詳細については、次を参照してください。 [ASP.NET appSettings 要素](https://msdn.microsoft.com/library/hh975440.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="061e2-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="061e2-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="061e2-208">UrlPathEncode</span></span>

<span data-ttu-id="061e2-209">推奨事項: を使用して、 [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx)代わりにします。</span><span class="sxs-lookup"><span data-stu-id="061e2-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="061e2-210">UrlPathEncode メソッドは、非常に特定のブラウザーの互換性の問題を解決するのには、.NET Framework に追加されました。</span><span class="sxs-lookup"><span data-stu-id="061e2-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="061e2-211">URL を適切にエンコードしないと、クロス サイト スクリプティングからアプリケーションを保護しません。</span><span class="sxs-lookup"><span data-stu-id="061e2-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="061e2-212">アプリケーションで使用する必要がありますしないでください。</span><span class="sxs-lookup"><span data-stu-id="061e2-212">You should never use it in your application.</span></span> <span data-ttu-id="061e2-213">代わりに、 [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="061e2-213">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="061e2-214">次の例では、ハイパーリンク コントロール用のクエリ文字列パラメーターとしてエンコードされた URL を渡す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="061e2-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="061e2-215">信頼性とパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="061e2-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="061e2-216">PreSendRequestHeaders と PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="061e2-216">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="061e2-217">: 推奨事項は、マネージ モジュールでこれらのイベントを使用することはしません。</span><span class="sxs-lookup"><span data-stu-id="061e2-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="061e2-218">代わりに、必要なタスクを実行するネイティブ IIS モジュールを記述します。</span><span class="sxs-lookup"><span data-stu-id="061e2-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="061e2-219">参照してください[ネイティブ コード HTTP モジュールを作成して](https://msdn.microsoft.com/library/ms693629.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="061e2-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="061e2-220">使用することができます、 [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx)と[PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) IIS のネイティブ モジュールでのイベント。</span><span class="sxs-lookup"><span data-stu-id="061e2-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="061e2-221">使用しない`PreSendRequestHeaders`と`PreSendRequestContent`を実装するマネージ モジュールで`IHttpModule`します。</span><span class="sxs-lookup"><span data-stu-id="061e2-221">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="061e2-222">これらのプロパティを設定すると、非同期要求で問題が発生することができます。</span><span class="sxs-lookup"><span data-stu-id="061e2-222">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="061e2-223">アプリケーション要求ルーティング処理 (ARR) と websocket の組み合わせは、w3wp クラッシュを引き起こす可能性のあるアクセス違反例外に生じる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="061e2-223">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="061e2-224">たとえば、iiscore!W3_CONTEXT_BASE::GetIsLastNotification + iiscore.dll で 68 では、アクセス違反例外 (0xC0000005) が原因です。</span><span class="sxs-lookup"><span data-stu-id="061e2-224">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="061e2-225">Web フォームでの非同期ページ イベント</span><span class="sxs-lookup"><span data-stu-id="061e2-225">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="061e2-226">推奨事項: この Web フォームで非同期ページのライフ サイクル イベントの void メソッドを記述しないように、代わりに使用[Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx)非同期コードの。</span><span class="sxs-lookup"><span data-stu-id="061e2-226">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="061e2-227">ページ イベントをマークすると**async**と**void**、非同期コードの実行が完了するとわかったことはできません。</span><span class="sxs-lookup"><span data-stu-id="061e2-227">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="061e2-228">代わりに、Page.RegisterAsyncTask を使用して、その完了を追跡することができる方法で非同期コードを実行します。</span><span class="sxs-lookup"><span data-stu-id="061e2-228">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="061e2-229">ボタンは、次の例では、非同期コードを含むハンドラーをクリックします。</span><span class="sxs-lookup"><span data-stu-id="061e2-229">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="061e2-230">この例では、推奨されるプラクティスではなく、非同期タスクの簡単な例としてのみ指定される、非同期的に文字列値の読み取り。</span><span class="sxs-lookup"><span data-stu-id="061e2-230">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="061e2-231">非同期タスクを使用している場合、Http ランタイムのターゲット フレームワークを 4.5 に Web.config ファイルで設定します。</span><span class="sxs-lookup"><span data-stu-id="061e2-231">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="061e2-232">新しい同期コンテキストで 4.5 ターンにターゲット フレームワークを設定すると、.NET 4.5 で追加されました。</span><span class="sxs-lookup"><span data-stu-id="061e2-232">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="061e2-233">この値は、既定では、Visual Studio 2012 の新しいプロジェクトに設定されますが、設定になっていない既存のプロジェクトを使用している場合は。</span><span class="sxs-lookup"><span data-stu-id="061e2-233">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="061e2-234">ファイア アンド フォーゲット作業</span><span class="sxs-lookup"><span data-stu-id="061e2-234">Fire-and-Forget Work</span></span>

<span data-ttu-id="061e2-235">推奨事項: ASP.NET 内の要求を処理する際に、ファイア アンド フォーゲット作業 (このような ThreadPool.QueueUserWorkItem メソッドの呼び出しまたはデリゲートを繰り返し呼び出すタイマーを作成する) を起動しないようにします。</span><span class="sxs-lookup"><span data-stu-id="061e2-235">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="061e2-236">ファイア アンド フォーゲット ASP.NET 内で実行される作業アプリケーションの場合は、アプリケーションは同期で取得できます。いつでもでも、アプリ ドメインがあり、継続的なプロセスが、アプリケーションの現在の状態と一致して不要になったを破壊することができます。</span><span class="sxs-lookup"><span data-stu-id="061e2-236">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="061e2-237">この種の ASP.NET の外部で作業を移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="061e2-237">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="061e2-238">進行中の作業を実行する Azure で Web ジョブ、Windows サービスまたはワーカー ロールを使用し、別のプロセスからそのコードを実行できます。</span><span class="sxs-lookup"><span data-stu-id="061e2-238">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="061e2-239">という名前の Nuget パッケージを追加することができる場合、ASP.NET 内でこの作業を実行する必要があります[WebBackgrounder](http://www.nuget.org/packages/webbackgrounder)コードを実行します。</span><span class="sxs-lookup"><span data-stu-id="061e2-239">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="061e2-240">要求エンティティ本体</span><span class="sxs-lookup"><span data-stu-id="061e2-240">Request Entity Body</span></span>

<span data-ttu-id="061e2-241">推奨事項は、Request.Form または Request.InputStream を読み取り、ハンドラーのイベントを実行する前にしないでください。</span><span class="sxs-lookup"><span data-stu-id="061e2-241">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="061e2-242">最も早い Request.Form または Request.InputStream から読み取る必要がありますが、ハンドラーの中にイベントを実行します。</span><span class="sxs-lookup"><span data-stu-id="061e2-242">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="061e2-243">MVC では、コント ローラーは、ハンドラーと、実行イベントは、アクション メソッドの実行時にします。</span><span class="sxs-lookup"><span data-stu-id="061e2-243">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="061e2-244">Web フォームで、ページは、ハンドラーと、execute イベント Page.Init イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="061e2-244">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="061e2-245">要求エンティティ本体を execute イベントより前読み取る場合は、要求の処理干渉します。</span><span class="sxs-lookup"><span data-stu-id="061e2-245">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="061e2-246">Execute イベントの前に、要求エンティティ本体を読み取る場合は、いずれかを使用して、 [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx)または[Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="061e2-246">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="061e2-247">GetBufferlessInputStream を使用すると、要求から生のストリームを取得し、要求全体を処理するための責任を負うものです。</span><span class="sxs-lookup"><span data-stu-id="061e2-247">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="061e2-248">呼び出し元 GetBufferlessInputStream、Request.Form および Request.InputStream は ASP.NET によって作成されてがいないために、使用できません後です。</span><span class="sxs-lookup"><span data-stu-id="061e2-248">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="061e2-249">GetBufferedInputStream を使用する場合は、要求からストリームのコピーを取得します。</span><span class="sxs-lookup"><span data-stu-id="061e2-249">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="061e2-250">Request.Form と Request.InputStream が使用可能な要求の後で ASP.NET の他のコピーを設定します。</span><span class="sxs-lookup"><span data-stu-id="061e2-250">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="061e2-251">Response.Redirect と Response.End</span><span class="sxs-lookup"><span data-stu-id="061e2-251">Response.Redirect and Response.End</span></span>

<span data-ttu-id="061e2-252">推奨事項: するスレッドを呼び出した後に処理する方法の相違点に注意してください[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="061e2-252">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="061e2-253">[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx)メソッド Response.End メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="061e2-253">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="061e2-254">同期プロセスでは、Request.Redirect を呼び出すとが、現在のスレッドがすぐに中止します。</span><span class="sxs-lookup"><span data-stu-id="061e2-254">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="061e2-255">ただし、非同期処理で Response.Redirect を呼び出すことは中止されない、現在のスレッドの要求は引き続きコードが実行されるようにします。</span><span class="sxs-lookup"><span data-stu-id="061e2-255">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="061e2-256">非同期の処理では、コードの実行を停止するメソッドからタスクを返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="061e2-256">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="061e2-257">MVC プロジェクトでは、Response.Redirect を呼び出さないでください。</span><span class="sxs-lookup"><span data-stu-id="061e2-257">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="061e2-258">代わりに、RedirectResult を返します。</span><span class="sxs-lookup"><span data-stu-id="061e2-258">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="061e2-259">EnableViewState およびに ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="061e2-259">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="061e2-260">推奨事項: 使用に ViewStateMode、コントロールがビュー ステートを使用する詳細な制御を提供する、EnableViewState の代わりにします。</span><span class="sxs-lookup"><span data-stu-id="061e2-260">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="061e2-261">EnableViewState ページ ディレクティブで false に設定すると、ビュー ステートは、ページ内のすべてのコントロールで無効にされ、有効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="061e2-261">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="061e2-262">ページ内の特定のコントロールのみにビュー ステートを有効にする場合に ViewStateMode を無効にページの設定します。</span><span class="sxs-lookup"><span data-stu-id="061e2-262">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="061e2-263">次に、ビュー ステートを実際に必要なコントロールのみで有効に ViewStateMode を設定します。</span><span class="sxs-lookup"><span data-stu-id="061e2-263">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="061e2-264">ビュー ステートが必要なコントロールのみを有効にすると、web ページのビュー ステートのサイズを縮小できます。</span><span class="sxs-lookup"><span data-stu-id="061e2-264">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="061e2-265">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="061e2-265">SqlMembershipProvider</span></span>

<span data-ttu-id="061e2-266">推奨事項は、ユニバーサル プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="061e2-266">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="061e2-267">現在のプロジェクト テンプレートで SqlMembershipProvider 代わられました[ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers)、NuGet パッケージとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="061e2-267">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="061e2-268">SqlMembershipProvider テンプレートの以前のバージョンでビルドされたプロジェクトを使用している場合は、ユニバーサル プロバイダーに切り替える必要があります。</span><span class="sxs-lookup"><span data-stu-id="061e2-268">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="061e2-269">Universal Providers Entity Framework でサポートされているすべてのデータベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="061e2-269">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="061e2-270">詳細については、次を参照してください。 [ASP.NET ユニバーサル プロバイダーの導入](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="061e2-270">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="061e2-271">実行時間の長い要求 (> 110 秒)</span><span class="sxs-lookup"><span data-stu-id="061e2-271">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="061e2-272">: 推奨事項を使用して、 [Websocket](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx)または[SignalR](../../../signalr/index.md)接続されているクライアントは、および非同期の I/O 操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="061e2-272">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="061e2-273">実行時間の長い要求は、web アプリケーションで予期しない結果とパフォーマンスの低下の原因になることができます。</span><span class="sxs-lookup"><span data-stu-id="061e2-273">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="061e2-274">要求の既定のタイムアウト設定は、110 秒です。</span><span class="sxs-lookup"><span data-stu-id="061e2-274">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="061e2-275">実行時間の長い要求でセッション状態を使用する場合、ASP.NET は 110 秒後にセッション オブジェクトのロックが解除されます。</span><span class="sxs-lookup"><span data-stu-id="061e2-275">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="061e2-276">ただし、アプリケーションがありますセッション オブジェクトに対する操作の途中で、ロックがリリースされ、操作が正常に完了しない可能性。</span><span class="sxs-lookup"><span data-stu-id="061e2-276">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="061e2-277">最初の要求の実行中に、ユーザーからの 2 番目の要求がブロックされている場合、2 番目の要求は不整合な状態でセッション オブジェクトにアクセス可能性があります。</span><span class="sxs-lookup"><span data-stu-id="061e2-277">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="061e2-278">アプリケーションには、I/O 操作ブロックしている (または同期) にはが含まれている場合は、アプリケーションは応答できません。</span><span class="sxs-lookup"><span data-stu-id="061e2-278">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="061e2-279">パフォーマンスを向上させるのには、.NET Framework の非同期 I/O 操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="061e2-279">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="061e2-280">また、サーバーにクライアントを接続するための Websocket や SignalR を使用します。</span><span class="sxs-lookup"><span data-stu-id="061e2-280">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="061e2-281">これらの機能は、実行時間の長い要求を効率的に処理するために設計されています。</span><span class="sxs-lookup"><span data-stu-id="061e2-281">These features are designed to efficiently handle long-running requests.</span></span>
