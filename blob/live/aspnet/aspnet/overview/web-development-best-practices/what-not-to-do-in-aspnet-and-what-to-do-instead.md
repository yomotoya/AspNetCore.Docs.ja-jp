---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: "ASP.NET では、操作を行わないとください |Microsoft ドキュメント"
author: tfitzmac
description: "このトピックでは、ASP.NET web プロジェクト内のユーザーが、いくつかの一般的な誤りを説明します。 これらの commo を避けるために行う必要がありますの推奨事項を提供しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 6790cd0deb36c9fb297ccd4df371f763dba17844
ms.sourcegitcommit: 17b025bd33f4474f0deaafc6d0447a4e72bcad87
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/27/2017
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="da6b7-104">ASP.NET では、操作を行わないと何を代わりに行うには</span><span class="sxs-lookup"><span data-stu-id="da6b7-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="da6b7-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="da6b7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="da6b7-106">このトピックでは、ASP.NET web プロジェクト内のユーザーが、いくつかの一般的な誤りを説明します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="da6b7-107">これらの一般的な間違いを避けるために行う必要がありますの推奨事項を提供します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="da6b7-108">基にして、[プレゼンテーション](http://vimeo.com/68390507)によって**Damian Edwards**開発者カンファレンスでノルウェー語。</span><span class="sxs-lookup"><span data-stu-id="da6b7-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="da6b7-109">免責情報</span><span class="sxs-lookup"><span data-stu-id="da6b7-109">Disclaimer</span></span>

<span data-ttu-id="da6b7-110">このトピックはありません完全なガイドとして、アプリケーションが安全かつ効率的なことを確認します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="da6b7-111">このトピックに記載されていないいるセキュリティおよびパフォーマンスのベスト プラクティスに従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="da6b7-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="da6b7-112">のみ、.NET クラスおよびプロセスに関連する一般的な誤りを回避する方法を提案します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="da6b7-113">概要</span><span class="sxs-lookup"><span data-stu-id="da6b7-113">Overview</span></span>

<span data-ttu-id="da6b7-114">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="da6b7-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="da6b7-115">標準への準拠</span><span class="sxs-lookup"><span data-stu-id="da6b7-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="da6b7-116">コントロール アダプター</span><span class="sxs-lookup"><span data-stu-id="da6b7-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="da6b7-117">コントロールのスタイル プロパティ</span><span class="sxs-lookup"><span data-stu-id="da6b7-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="da6b7-118">ページおよびコントロールのコールバック</span><span class="sxs-lookup"><span data-stu-id="da6b7-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="da6b7-119">ブラウザー機能の検出</span><span class="sxs-lookup"><span data-stu-id="da6b7-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="da6b7-120">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="da6b7-120">Security</span></span>](#security)

    - [<span data-ttu-id="da6b7-121">要求の検証</span><span class="sxs-lookup"><span data-stu-id="da6b7-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="da6b7-122">Cookie なしのフォーム認証とセッション</span><span class="sxs-lookup"><span data-stu-id="da6b7-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="da6b7-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="da6b7-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="da6b7-124">中程度の信頼</span><span class="sxs-lookup"><span data-stu-id="da6b7-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="da6b7-125">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="da6b7-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="da6b7-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="da6b7-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="da6b7-127">信頼性とパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="da6b7-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="da6b7-128">PreSendRequestHeaders と PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="da6b7-128">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="da6b7-129">Web フォーム ページの非同期イベント</span><span class="sxs-lookup"><span data-stu-id="da6b7-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="da6b7-130">Fire 忘れた作業</span><span class="sxs-lookup"><span data-stu-id="da6b7-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="da6b7-131">要求エンティティ本体</span><span class="sxs-lookup"><span data-stu-id="da6b7-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="da6b7-132">Response.Redirect と Response.End</span><span class="sxs-lookup"><span data-stu-id="da6b7-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="da6b7-133">エントリおよび ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="da6b7-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="da6b7-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="da6b7-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="da6b7-135">長いを実行している要求 (> 110 秒)</span><span class="sxs-lookup"><span data-stu-id="da6b7-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="da6b7-136">標準への準拠</span><span class="sxs-lookup"><span data-stu-id="da6b7-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="da6b7-137">コントロール アダプター</span><span class="sxs-lookup"><span data-stu-id="da6b7-137">Control Adapters</span></span>

<span data-ttu-id="da6b7-138">に関する推奨事項: が、アダプティブ表示用コントロール アダプターの使用を停止し、CSS メディア クエリおよび標準に準拠した HTML を代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="da6b7-139">コントロールのアダプターは、異なるデバイスや環境用にカスタマイズされたプレゼンテーション コードを表示するために .NET 2.0 で導入されました。</span><span class="sxs-lookup"><span data-stu-id="da6b7-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="da6b7-140">ここで、このアダプティブ レンダリングは、CSS、および HTML を実行できます。</span><span class="sxs-lookup"><span data-stu-id="da6b7-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="da6b7-141">コントロール アダプターの使用を停止し、CSS、および HTML を既存のアダプターに変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da6b7-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="da6b7-142">詳細については、次を参照してください。[メディア クエリ](http://www.w3.org/TR/css3-mediaqueries/)と[How To: モバイル ページ、ASP.NET Web フォームを追加/MVC アプリケーション](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="da6b7-143">コントロールのスタイル プロパティ</span><span class="sxs-lookup"><span data-stu-id="da6b7-143">Style Properties on Controls</span></span>

<span data-ttu-id="da6b7-144">推奨事項: は、コントロールのマークアップにスタイル値の設定を停止し、代わりに CSS スタイル シートの書式設定の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="da6b7-145">Web サーバー コントロールは、数十個の行のスタイル プロパティを設定するために使用するプロパティを含めます。</span><span class="sxs-lookup"><span data-stu-id="da6b7-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="da6b7-146">たとえば、前景色プロパティは、コントロールのテキストの色を設定します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="da6b7-147">CSS スタイル シートをより効率的にこれと同じ効果を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="da6b7-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="da6b7-148">スタイル シートを使用すると、スタイルの値を集中管理し、アプリケーション全体でこれらの値を設定しないでください。</span><span class="sxs-lookup"><span data-stu-id="da6b7-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="da6b7-149">次の例では、赤にセット テキスト、CSS クラスを示します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="da6b7-150">次の例では、CSS クラスを動的に適用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="da6b7-151">ページおよびコントロールのコールバック</span><span class="sxs-lookup"><span data-stu-id="da6b7-151">Page and Control Callbacks</span></span>

<span data-ttu-id="da6b7-152">に関する推奨事項: がページおよびコントロールのコールバックの使用を停止し、代わりに、次のいずれかを使用: AJAX、UpdatePanel、MVC アクション メソッド、Web API、または SignalR です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="da6b7-153">以前のバージョンの ASP.NET では、ページおよびコントロールのコールバック メソッドには、ページ全体を更新することがなく、web ページの一部を更新することが有効になります。</span><span class="sxs-lookup"><span data-stu-id="da6b7-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="da6b7-154">部分ページ更新を実行できるようになりました[AJAX](../../../ajax/index.md)、 [UpdatePanel](https://msdn.microsoft.com/en-US/library/bb386454.aspx)、 [MVC](../../../mvc/index.md)、 [Web API](../../../web-api/index.md)または[SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="da6b7-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/en-US/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="da6b7-155">フレンドリな Url で問題が発生することがあるために、コールバック メソッドを使用して、ルーティングを停止する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da6b7-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="da6b7-156">既定では、コントロールには、コールバック メソッドが有効にしていないが、コントロールに、この機能を有効にした場合は無効にします。</span><span class="sxs-lookup"><span data-stu-id="da6b7-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="da6b7-157">ブラウザー機能の検出</span><span class="sxs-lookup"><span data-stu-id="da6b7-157">Browser Capability Detection</span></span>

<span data-ttu-id="da6b7-158">推奨事項: が静的なブラウザーの機能の検出を使用してを停止し、代わりに動的な機能の検出を使用します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="da6b7-159">ASP.NET の以前のバージョンでは、各ブラウザーでサポートされる機能は、XML ファイルに保存されていました。</span><span class="sxs-lookup"><span data-stu-id="da6b7-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="da6b7-160">静的なルックアップによる検出の機能サポートは、最善の方法ではありません。</span><span class="sxs-lookup"><span data-stu-id="da6b7-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="da6b7-161">ここで、動的に検出できます、ブラウザーでサポートされている機能などの機能の検出のフレームワークを使用して、 [Modernizr](http://modernizr.com/)です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="da6b7-162">機能の検出は、メソッドまたはプロパティを使用しようとして、ブラウザーで、目的の結果が生成されるかどうかはどうかを確認し、サポートを判断します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="da6b7-163">既定では、Web アプリケーション テンプレートに Modernizr が含まれています。</span><span class="sxs-lookup"><span data-stu-id="da6b7-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="da6b7-164">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="da6b7-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="da6b7-165">要求の検証</span><span class="sxs-lookup"><span data-stu-id="da6b7-165">Request Validation</span></span>

<span data-ttu-id="da6b7-166">推奨事項: は、ユーザー入力を検証し、ユーザーからの出力をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="da6b7-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="da6b7-167">要求の検証は、各要求を検査し、見かけ上の脅威が見つかった場合は、要求を停止する ASP.NET の機能です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="da6b7-168">クロスサイト スクリプティング攻撃に対してアプリケーションを保護するための要求の検証に依存しません。</span><span class="sxs-lookup"><span data-stu-id="da6b7-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="da6b7-169">代わりに、ユーザーからのすべての入力を検証し、出力をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="da6b7-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="da6b7-170">まれに、正規表現を使用して、入力を検証することができますが、値と一致する場合を決定する .NET クラスを使用してユーザー入力が値を許可するより複雑な場合は検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da6b7-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="da6b7-171">次の例では、Uri クラスの静的メソッドを使用して、ユーザーによって提供される Uri が有効かどうかを判断する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="da6b7-172">ただし、Uri を十分に確認する必要がありますも確認するを指定しているかどうかを確認する`http`または`https`です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="da6b7-173">次の例では、インスタンス メソッドを使用して、Uri が有効であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="da6b7-174">ユーザー入力を HTML としてレンダリングまたは SQL クエリでユーザー入力を含む、前に、悪意のあるコードが含まれていないことを確認する値をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="da6b7-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="da6b7-175">HTML を作成することができますマークアップで値をエンコード、 &lt;%: %&gt;構文を次のようにします。</span><span class="sxs-lookup"><span data-stu-id="da6b7-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="da6b7-176">Razor 構文では、HTML ことができます。 または、を使用したエンコード @、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="da6b7-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="da6b7-177">次の例に示す方法を HTML エンコード分離コード内の値。</span><span class="sxs-lookup"><span data-stu-id="da6b7-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="da6b7-178">SQL コマンドの値を安全にエンコードする場合などのコマンド パラメーターを使用して、 [SqlParameter](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlparameter.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="da6b7-179">Cookie なしのフォーム認証とセッション</span><span class="sxs-lookup"><span data-stu-id="da6b7-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="da6b7-180">推奨事項では、cookie が必要です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="da6b7-181">クエリ文字列内の認証情報を渡すことは安全ではありません。</span><span class="sxs-lookup"><span data-stu-id="da6b7-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="da6b7-182">そのため、アプリケーションに認証が含まれている場合は、cookie が必要です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="da6b7-183">Cookie は、機密情報を保存する場合は、cookie に SSL を必要とすることを検討します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="da6b7-184">次の例では、フォーム認証で SSL 経由で送信される cookie は、Web.config ファイルで指定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="da6b7-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="da6b7-185">EnableViewStateMac</span></span>

<span data-ttu-id="da6b7-186">推奨事項: 決して false に設定します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="da6b7-187">既定では、EnbableViewStateMac は設定を true にします。</span><span class="sxs-lookup"><span data-stu-id="da6b7-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="da6b7-188">アプリケーションがビュー状態を使用していない場合でも、false に EnableViewStateMac は設定されません。</span><span class="sxs-lookup"><span data-stu-id="da6b7-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="da6b7-189">この値を false に設定すると、アプリケーションがクロス サイト スクリプトに対する脆弱性。</span><span class="sxs-lookup"><span data-stu-id="da6b7-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="da6b7-190">以降で ASP.NET 4.5.2 は、ランタイムでは、 **EnableViewStateMac = true**です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="da6b7-191">False に設定すると、場合でも、ランタイムはこの値は無視し、true に設定されている値が続行されます。</span><span class="sxs-lookup"><span data-stu-id="da6b7-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="da6b7-192">詳細については、次を参照してください。 [ASP.NET 4.5.2 および EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="da6b7-193">次の例では、EnableViewStateMac を true に設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="da6b7-194">実際には既定で true になっているために、true には、この値を設定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="da6b7-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="da6b7-195">ただし、設定した場合は false に任意のページで、アプリケーションは、この値を直ちに修正する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da6b7-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="da6b7-196">中程度の信頼</span><span class="sxs-lookup"><span data-stu-id="da6b7-196">Medium Trust</span></span>

<span data-ttu-id="da6b7-197">推奨事項に依存しない中程度の信頼 (またはその他の信頼レベル)、セキュリティ境界として。</span><span class="sxs-lookup"><span data-stu-id="da6b7-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="da6b7-198">部分信頼は、アプリケーションを適切に保護できないは使用できません。</span><span class="sxs-lookup"><span data-stu-id="da6b7-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="da6b7-199">代わりに、完全信頼を使用し、個別のアプリケーション プールで信頼されていないアプリケーションを分離します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="da6b7-200">また、一意の id では、各アプリケーション プールを実行します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="da6b7-201">詳細については、次を参照してください。 [ASP.NET 部分信頼でアプリケーションの分離が保証されない](https://support.microsoft.com/kb/2698981)です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="da6b7-202">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="da6b7-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="da6b7-203">に関する推奨事項: セキュリティ設定を無効にしない&lt;appSettings&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="da6b7-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="da6b7-204">AppSettings 要素には、セキュリティ更新プログラムの必要な多くの値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="da6b7-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="da6b7-205">変更または、これらの値を無効にする必要がありますされません。</span><span class="sxs-lookup"><span data-stu-id="da6b7-205">You should not change or disable these values.</span></span> <span data-ttu-id="da6b7-206">場合は、更新プログラムを展開するときに、これらの値を無効にする必要があります、すぐに再度有効に展開が完了後します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="da6b7-207">詳細については、「 [ASP.NET appSettings 要素](https://msdn.microsoft.com/en-us/library/hh975440.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/en-us/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="da6b7-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="da6b7-208">UrlPathEncode</span></span>

<span data-ttu-id="da6b7-209">推奨事項: を使用して[UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx)代わりにします。</span><span class="sxs-lookup"><span data-stu-id="da6b7-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="da6b7-210">UrlPathEncode メソッドは、非常に特定のブラウザーの互換性の問題を解決するのには、.NET Framework に追加されました。</span><span class="sxs-lookup"><span data-stu-id="da6b7-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="da6b7-211">URL は適切にエンコードされませんし、クロス サイト スクリプトから、アプリケーションを保護しません。</span><span class="sxs-lookup"><span data-stu-id="da6b7-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="da6b7-212">アプリケーションで使用する必要がありますしないでください。</span><span class="sxs-lookup"><span data-stu-id="da6b7-212">You should never use it in your application.</span></span> <span data-ttu-id="da6b7-213">代わりに、 [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-213">Instead, use [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="da6b7-214">次の例では、ハイパーリンク コントロール用のクエリ文字列パラメーターとしてエンコードされた URL を渡す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="da6b7-215">信頼性とパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="da6b7-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="da6b7-216">PreSendRequestHeaders と PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="da6b7-216">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="da6b7-217">推奨事項では、マネージ モジュールでこれらのイベントは使用しません。</span><span class="sxs-lookup"><span data-stu-id="da6b7-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="da6b7-218">代わりに、必要なタスクを実行するネイティブの IIS モジュールを記述します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="da6b7-219">参照してください[ネイティブ コード HTTP モジュールを作成する](https://msdn.microsoft.com/en-us/library/ms693629.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/en-us/library/ms693629.aspx).</span></span>

<span data-ttu-id="da6b7-220">使用することができます、 [PreSendRequestHeaders](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestheaders.aspx)と[PreSendRequestContent](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestcontent.aspx)ネイティブの IIS モジュールを持つイベント。</span><span class="sxs-lookup"><span data-stu-id="da6b7-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="da6b7-221">使用しないでください`PreSendRequestHeaders`と`PreSendRequestContent`を実装するマネージ モジュールで`IHttpModule`です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-221">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="da6b7-222">これらのプロパティを設定すると、非同期要求で問題が発生することができます。</span><span class="sxs-lookup"><span data-stu-id="da6b7-222">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="da6b7-223">アプリケーション要求ルーティング処理 (ARR) と websocket の組み合わせが例外が発生するアクセス違反 w3wp クラッシュする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="da6b7-223">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="da6b7-224">たとえば、iiscore!W3_CONTEXT_BASE::GetIsLastNotification + 68 iiscore.dll でアクセス違反例外 (0xC0000005) が発生しました。</span><span class="sxs-lookup"><span data-stu-id="da6b7-224">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="da6b7-225">Web フォーム ページの非同期イベント</span><span class="sxs-lookup"><span data-stu-id="da6b7-225">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="da6b7-226">推奨事項: が Web フォームで、ページ ライフ サイクル イベントの void メソッドを非同期に書き込みを回避し、代わりに使用して[Page.RegisterAsyncTask](https://msdn.microsoft.com/en-us/library/system.web.ui.page.registerasynctask.aspx)非同期コードにします。</span><span class="sxs-lookup"><span data-stu-id="da6b7-226">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/en-us/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="da6b7-227">ページのイベントをマークする**非同期**と**void**、非同期コードが終了した場合を判断することはできません。</span><span class="sxs-lookup"><span data-stu-id="da6b7-227">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="da6b7-228">代わりに、Page.RegisterAsyncTask を使用して、その完了を追跡することができるように、非同期コードを実行します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-228">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="da6b7-229">ボタンの次の例では、非同期コードを含むハンドラーをクリックします。</span><span class="sxs-lookup"><span data-stu-id="da6b7-229">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="da6b7-230">この例が含まれます文字列値を非同期的に読み取りと推奨される方法ではなく、非同期タスクの簡単な例としてのみ提供されています。</span><span class="sxs-lookup"><span data-stu-id="da6b7-230">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="da6b7-231">非同期タスクを使用している場合は、Web.config ファイルで 4.5 に Http ランタイム ターゲット フレームワークを設定します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-231">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="da6b7-232">新しい同期コンテキストで 4.5 ターンにターゲット フレームワークを設定すると、.NET 4.5 で追加されました。</span><span class="sxs-lookup"><span data-stu-id="da6b7-232">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="da6b7-233">この値は、既定では、Visual Studio 2012 の新しいプロジェクトに設定されているが、設定になっていない既存のプロジェクトで作業している場合は。</span><span class="sxs-lookup"><span data-stu-id="da6b7-233">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="da6b7-234">Fire 忘れた作業</span><span class="sxs-lookup"><span data-stu-id="da6b7-234">Fire-and-Forget Work</span></span>

<span data-ttu-id="da6b7-235">推奨事項: ASP.NET 内で要求を処理する場合に、火災忘れた作業 (このような threadpool.queueuserworkitem にメソッドを呼び出すまたはデリゲートを繰り返し呼び出すタイマーを作成する) を起動しないでください。</span><span class="sxs-lookup"><span data-stu-id="da6b7-235">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="da6b7-236">アプリケーションに ASP.NET 内で実行される起動忘れた作業がある場合、アプリケーションは同期で取得できます。いつでも、アプリケーション ドメインが、継続的なプロセスは、アプリケーションの現在の状態を一致不要になった可能性があることを意味するを破壊することができます。</span><span class="sxs-lookup"><span data-stu-id="da6b7-236">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="da6b7-237">この種の ASP.NET の外部での作業を移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da6b7-237">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="da6b7-238">進行中の作業を実行する Azure で Web ジョブ、Windows サービスまたはワーカー ロールを使用して別のプロセスからそのコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-238">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="da6b7-239">呼ばれる Nuget パッケージを追加することができる場合、ASP.NET 内でこの作業を実行する必要があります[WebBackgrounder](http://www.nuget.org/packages/webbackgrounder)コードを実行します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-239">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="da6b7-240">要求エンティティ本体</span><span class="sxs-lookup"><span data-stu-id="da6b7-240">Request Entity Body</span></span>

<span data-ttu-id="da6b7-241">推奨事項: は、Request.Form または Request.InputStream を読み取り、ハンドラーのイベントを実行する前にしないでください。</span><span class="sxs-lookup"><span data-stu-id="da6b7-241">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="da6b7-242">最も早い Request.Form または Request.InputStream から読み取る必要がありますが、ハンドラーの中にイベントを実行します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-242">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="da6b7-243">MVC では、コント ローラーは、ハンドラーと、execute イベント、アクション メソッドを実行するとします。</span><span class="sxs-lookup"><span data-stu-id="da6b7-243">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="da6b7-244">Web フォームのページは、ハンドラーと、execute イベント Page.Init イベントの発生時です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-244">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="da6b7-245">Execute イベントより前に要求エンティティ本体を読み取るする場合は、要求の処理干渉します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-245">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="da6b7-246">Execute イベントの前に要求エンティティ本体を読み取る必要がある場合を使用するか[Request.GetBufferlessInputStream](https://msdn.microsoft.com/en-us/library/ff406798.aspx)または[Request.GetBufferedInputStream](https://msdn.microsoft.com/en-us/library/system.web.httprequest.getbufferedinputstream.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-246">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/en-us/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/en-us/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="da6b7-247">GetBufferlessInputStream を使用する場合、要求の生のストリームを取得し、全体の要求を処理するための責任を負うものです。</span><span class="sxs-lookup"><span data-stu-id="da6b7-247">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="da6b7-248">GetBufferlessInputStream を呼び出した後 Request.Form および Request.InputStream は使用できません、ASP.NET によって作成されてがいないためです。</span><span class="sxs-lookup"><span data-stu-id="da6b7-248">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="da6b7-249">GetBufferedInputStream を使用する場合は、要求からストリームのコピーを取得します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-249">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="da6b7-250">Request.Form と Request.InputStream が使用可能な要求の後で ASP.NET がもう一方のコピーを作成します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-250">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="da6b7-251">Response.Redirect と Response.End</span><span class="sxs-lookup"><span data-stu-id="da6b7-251">Response.Redirect and Response.End</span></span>

<span data-ttu-id="da6b7-252">に関する推奨事項: スレッドの呼び出し後の処理方法の相違点に注意してください[Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-252">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="da6b7-253">[Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx)メソッド Response.End メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-253">The [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="da6b7-254">同期プロセスで Request.Redirect を呼び出すと、すぐに中止する現在のスレッドとします。</span><span class="sxs-lookup"><span data-stu-id="da6b7-254">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="da6b7-255">ただし、非同期的な処理で Response.Redirect を呼び出すことは中止されない、現在のスレッドのため、要求のコードの実行が続行されます。</span><span class="sxs-lookup"><span data-stu-id="da6b7-255">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="da6b7-256">非同期的な処理で、コードの実行を停止するメソッドからタスクを返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="da6b7-256">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="da6b7-257">MVC プロジェクトでは、呼び出す必要はありません Response.Redirect です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-257">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="da6b7-258">代わりに、RedirectResult を返します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-258">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="da6b7-259">エントリおよび ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="da6b7-259">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="da6b7-260">ビュー ステートを使用するコントロールをきめ細かく制御のエントリではなく、推奨事項: 使用 ViewStateMode です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-260">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="da6b7-261">エントリ ページ ディレクティブで false に設定すると、ビュー ステートがページ内のすべてのコントロールで無効にされ、有効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="da6b7-261">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="da6b7-262">ページ内の特定のコントロールのみにビュー ステートを有効にする場合は、ページの [無効] に ViewStateMode を設定します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-262">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="da6b7-263">次に、ViewStateMode が有効にビュー ステートを実際に必要なコントロールのみに設定します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-263">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="da6b7-264">必要なコントロールのみのビュー ステートを有効にするには、web ページのビュー ステートのサイズを縮小することができます。</span><span class="sxs-lookup"><span data-stu-id="da6b7-264">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="da6b7-265">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="da6b7-265">SqlMembershipProvider</span></span>

<span data-ttu-id="da6b7-266">推奨事項は、Universal Providers を使用します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-266">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="da6b7-267">現在のプロジェクト テンプレートで SqlMembershipProvider は置き換えられました[ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers)、これは、NuGet パッケージとして入手できます。</span><span class="sxs-lookup"><span data-stu-id="da6b7-267">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="da6b7-268">SqlMembershipProvider テンプレートの以前のバージョンでビルドされたプロジェクトを使用している場合は、Universal Providers に切り替える必要があります。</span><span class="sxs-lookup"><span data-stu-id="da6b7-268">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="da6b7-269">ユニバーサル プロバイダーは、Entity Framework でサポートされているすべてのデータベースで動作します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-269">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="da6b7-270">詳細については、次を参照してください。 [ASP.NET Universal Providers を導入](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-270">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="da6b7-271">実行時間の長い要求 (> 110 秒)</span><span class="sxs-lookup"><span data-stu-id="da6b7-271">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="da6b7-272">推奨事項: を使用して[Websocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket.aspx)または[SignalR](../../../signalr/index.md)接続されているクライアント、および非同期の I/O 操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-272">Recommendation: Use [WebSockets](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="da6b7-273">実行時間の長い要求は、web アプリケーションで予期しない結果とパフォーマンスの低下の原因になることができます。</span><span class="sxs-lookup"><span data-stu-id="da6b7-273">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="da6b7-274">要求の既定のタイムアウト設定は、110 (秒) です。</span><span class="sxs-lookup"><span data-stu-id="da6b7-274">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="da6b7-275">実行時間の長い要求でセッション状態を使用している場合、ASP.NET は 110 秒後にセッション オブジェクトのロックが解除されます。</span><span class="sxs-lookup"><span data-stu-id="da6b7-275">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="da6b7-276">ただし、アプリケーションがありますセッション オブジェクトに対する操作の途中で、ロックを解除し、操作を完了しない可能性。</span><span class="sxs-lookup"><span data-stu-id="da6b7-276">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="da6b7-277">ユーザーから 2 番目の要求がブロックされた場合、最初の要求の実行中に、2 番目の要求は不整合な状態でセッション オブジェクトにアクセス可能性があります。</span><span class="sxs-lookup"><span data-stu-id="da6b7-277">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="da6b7-278">アプリケーションには、ブロックしている (または同期) の I/O 操作が含まれている場合は、アプリケーションは応答できません。</span><span class="sxs-lookup"><span data-stu-id="da6b7-278">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="da6b7-279">パフォーマンスを向上させるには、.NET Framework の非同期 I/O 操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="da6b7-279">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="da6b7-280">また、Websocket または SignalR を使用して、サーバーにクライアントを接続するためです。</span><span class="sxs-lookup"><span data-stu-id="da6b7-280">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="da6b7-281">これらの機能は、実行時間の長い要求を効率的に処理する設計されています。</span><span class="sxs-lookup"><span data-stu-id="da6b7-281">These features are designed to efficiently handle long-running requests.</span></span>
