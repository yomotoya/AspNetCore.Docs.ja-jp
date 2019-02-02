---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: ASP.NET では、しないとは |Microsoft Docs
author: Rick-Anderson
description: このトピックでは、ASP.NET web プロジェクト内でユーザーが、いくつかの一般的なミスをについて説明します。 これらの共通を回避するために行う必要がありますの推奨事項を提供しています.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 512d2e2b39467635390fa175546f79d8c9f89f4a
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667714"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="aaf9c-104">ASP.NET では行わないことと、その代わりに行うこと</span><span class="sxs-lookup"><span data-stu-id="aaf9c-104">What not to do in ASP.NET, and what to do instead</span></span>

> <span data-ttu-id="aaf9c-105">このトピックでは、ASP.NET web プロジェクト内でユーザーが、いくつかの一般的なミスをについて説明します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-105">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="aaf9c-106">これらの一般的な誤りを回避するために行う必要がありますの推奨事項を提供します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-106">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="aaf9c-107">基にして、[プレゼンテーション](http://vimeo.com/68390507)によって**Damian Edwards** Norwegian Developers Conference でします。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-107">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="aaf9c-108">免責情報</span><span class="sxs-lookup"><span data-stu-id="aaf9c-108">Disclaimer</span></span>

<span data-ttu-id="aaf9c-109">このトピックではありませんの完全なガイドとして、アプリケーションが安全かつ効率的なことを確認します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-109">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="aaf9c-110">このトピックに記載されていないいるセキュリティとパフォーマンスのベスト プラクティスに従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-110">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="aaf9c-111">.NET クラスとプロセスに関連する一般的なミスを回避する方法のみお勧めします。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-111">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="aaf9c-112">概要</span><span class="sxs-lookup"><span data-stu-id="aaf9c-112">Overview</span></span>

<span data-ttu-id="aaf9c-113">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-113">This topic contains the following sections:</span></span>

- [<span data-ttu-id="aaf9c-114">標準への準拠</span><span class="sxs-lookup"><span data-stu-id="aaf9c-114">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="aaf9c-115">コントロール アダプター</span><span class="sxs-lookup"><span data-stu-id="aaf9c-115">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="aaf9c-116">コントロールのスタイル プロパティ</span><span class="sxs-lookup"><span data-stu-id="aaf9c-116">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="aaf9c-117">ページおよびコントロールのコールバック</span><span class="sxs-lookup"><span data-stu-id="aaf9c-117">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="aaf9c-118">ブラウザー機能の検出</span><span class="sxs-lookup"><span data-stu-id="aaf9c-118">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="aaf9c-119">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="aaf9c-119">Security</span></span>](#security)

    - [<span data-ttu-id="aaf9c-120">要求の検証</span><span class="sxs-lookup"><span data-stu-id="aaf9c-120">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="aaf9c-121">Cookieless フォーム認証とセッション</span><span class="sxs-lookup"><span data-stu-id="aaf9c-121">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="aaf9c-122">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="aaf9c-122">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="aaf9c-123">中程度の信頼</span><span class="sxs-lookup"><span data-stu-id="aaf9c-123">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="aaf9c-124">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="aaf9c-124">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="aaf9c-125">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="aaf9c-125">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="aaf9c-126">信頼性とパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="aaf9c-126">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="aaf9c-127">PreSendRequestHeaders と PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="aaf9c-127">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="aaf9c-128">Web フォームでの非同期ページ イベント</span><span class="sxs-lookup"><span data-stu-id="aaf9c-128">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="aaf9c-129">ファイア アンド フォーゲット作業</span><span class="sxs-lookup"><span data-stu-id="aaf9c-129">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="aaf9c-130">要求エンティティ本体</span><span class="sxs-lookup"><span data-stu-id="aaf9c-130">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="aaf9c-131">Response.Redirect と Response.End</span><span class="sxs-lookup"><span data-stu-id="aaf9c-131">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="aaf9c-132">EnableViewState およびに ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="aaf9c-132">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="aaf9c-133">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="aaf9c-133">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="aaf9c-134">長い実行されている要求 (> 110 秒)</span><span class="sxs-lookup"><span data-stu-id="aaf9c-134">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="aaf9c-135">標準への準拠</span><span class="sxs-lookup"><span data-stu-id="aaf9c-135">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="aaf9c-136">コントロール アダプター</span><span class="sxs-lookup"><span data-stu-id="aaf9c-136">Control adapters</span></span>

<span data-ttu-id="aaf9c-137">推奨事項:アダプティブ レンダリングは、コントロール アダプターの使用を停止し、代わりに CSS メディア クエリと標準に準拠した HTML を使用します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-137">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="aaf9c-138">コントロール アダプターは、さまざまなデバイスや環境向けにカスタマイズされたプレゼンテーション コードを表示するために .NET 2.0 で導入されました。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-138">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="aaf9c-139">ここで、CSS、HTML によるこのアダプティブ レンダリングを実現できます。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-139">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="aaf9c-140">コントロール アダプターの使用を停止して、CSS、HTML、既存のアダプターを変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-140">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="aaf9c-141">詳細については、次を参照してください[メディア クエリ](http://www.w3.org/TR/css3-mediaqueries/)と[How To:。モバイル ページを追加、ASP.NET Web フォーム/MVC アプリケーション](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-141">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="aaf9c-142">コントロールのスタイル プロパティ</span><span class="sxs-lookup"><span data-stu-id="aaf9c-142">Style properties on controls</span></span>

<span data-ttu-id="aaf9c-143">推奨事項:コントロールのマークアップにスタイル値の設定を停止し、代わりに CSS スタイル シートで書式設定の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-143">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="aaf9c-144">Web サーバー コントロールには、多数行のスタイル プロパティを設定するために使用できるプロパティにはが含まれます。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-144">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="aaf9c-145">たとえば、前景色プロパティは、コントロールのテキストの色を設定します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-145">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="aaf9c-146">CSS スタイル シートをより効率的にこれと同じ効果を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-146">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="aaf9c-147">スタイル シートを使用すると、スタイルの値を一元化し、アプリケーション全体でこれらの値を設定しないでください。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-147">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="aaf9c-148">次の例は、CSS クラスを赤にセットのテキストを示します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-148">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="aaf9c-149">次の例では、CSS クラスを動的に適用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-149">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="aaf9c-150">ページおよびコントロールのコールバック</span><span class="sxs-lookup"><span data-stu-id="aaf9c-150">Page and control callbacks</span></span>

<span data-ttu-id="aaf9c-151">推奨事項:ページおよびコントロールのコールバックの使用を停止し、代わりに、次のいずれかを使用します。AJAX、UpdatePanel、MVC アクション メソッド、Web API、または SignalR。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-151">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="aaf9c-152">以前のバージョンの ASP.NET では、ページおよびコントロールのコールバック メソッドには、ページ全体を更新することがなく、web ページの一部を更新することが有効になります。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-152">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="aaf9c-153">部分ページ更新を行うことができますようになりました[AJAX](../../../ajax/index.md)、 [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx)、 [MVC](../../../mvc/index.md)、 [Web API](../../../web-api/index.md)または[SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="aaf9c-153">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="aaf9c-154">フレンドリな Url で問題が生じるため、コールバック メソッドを使用して、ルーティングを停止する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-154">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="aaf9c-155">既定では、コントロールがコールバック メソッドでは、有効にしないが、コントロールでは、この機能を有効にした場合無効する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-155">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="aaf9c-156">ブラウザー機能の検出</span><span class="sxs-lookup"><span data-stu-id="aaf9c-156">Browser capability detection</span></span>

<span data-ttu-id="aaf9c-157">推奨事項:静的ブラウザー機能の検出の使用を停止し、代わりに動的機能検出を使用します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-157">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="aaf9c-158">以前のバージョンの ASP.NET では、各ブラウザーでサポートされている機能は、XML ファイルに保存されていました。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-158">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="aaf9c-159">静的な参照を通じて検出機能のサポートは、最適な方法ではありません。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-159">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="aaf9c-160">ここで、動的に検出できますなどの機能検出フレームワークを使用して、ブラウザーのサポート機能[Modernizr](http://modernizr.com/)します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-160">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="aaf9c-161">機能検出は、メソッドまたはプロパティを使用しようとして、ブラウザーで、目的の結果が生成されるかどうかはどうかを確認し、サポートを判断します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-161">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="aaf9c-162">既定では、Modernizr は、Web アプリケーション テンプレートに含まれます。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-162">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="aaf9c-163">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="aaf9c-163">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="aaf9c-164">要求の検証</span><span class="sxs-lookup"><span data-stu-id="aaf9c-164">Request validation</span></span>

<span data-ttu-id="aaf9c-165">推奨事項:ユーザーの入力を検証し、ユーザーからの出力をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-165">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="aaf9c-166">要求の検証は、各要求を検査し、見かけ上の脅威が見つかった場合は、要求を停止します。 ASP.NET の機能です。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-166">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="aaf9c-167">クロスサイト スクリプティング攻撃に対してアプリケーションを保護するための要求の検証に依存しません。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-167">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="aaf9c-168">代わりに、ユーザーからのすべての入力を検証し、出力をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-168">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="aaf9c-169">まれに、正規表現を使用して、入力を検証することができますが、値と一致する場合を決定する .NET クラスを使用してユーザー入力を検証する必要がありますより複雑な場合に、値が許可されています。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-169">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="aaf9c-170">次の例では、Uri クラスの静的メソッドを使用して、ユーザーによって提供された Uri が有効かどうかを判断する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-170">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="aaf9c-171">ただし、Uri を十分に確認する必要がありますもチェックを指定するかどうかを確認する`http`または`https`します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-171">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="aaf9c-172">次の例では、インスタンス メソッドを使用して、Uri が有効であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-172">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="aaf9c-173">ユーザー入力を HTML としてレンダリングまたはユーザーの入力を含む SQL クエリで、前に、悪意のあるコードが含まれていないことを確認する値をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-173">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="aaf9c-174">HTML のできますマークアップで値をエンコード、 &lt;%: %&gt;構文を次に示すよう。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-174">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="aaf9c-175">Razor 構文では、HTML ことができます。 または、を使用したエンコード @ 以下に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-175">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="aaf9c-176">例を次に示しますを HTML エンコード方法、分離コードでの値。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-176">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="aaf9c-177">SQL コマンドの値を安全にエンコードする場合などのコマンド パラメーターを使用して、 [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-177">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="aaf9c-178">Cookieless フォーム認証とセッション</span><span class="sxs-lookup"><span data-stu-id="aaf9c-178">Cookieless forms authentication and session</span></span>

<span data-ttu-id="aaf9c-179">推奨事項:Cookie が必要です。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-179">Recommendation: Require cookies.</span></span>

<span data-ttu-id="aaf9c-180">クエリ文字列での認証情報を渡すことは安全ではありません。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-180">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="aaf9c-181">アプリケーションに認証が含まれているため、cookie が必要です。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-181">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="aaf9c-182">Cookie は、機密情報を保存する場合は、クッキーに対して SSL を要求することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-182">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="aaf9c-183">次の例では、フォーム認証が SSL 経由で送信される cookie が必要である、Web.config ファイルで指定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-183">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="aaf9c-184">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="aaf9c-184">EnableViewStateMac</span></span>

<span data-ttu-id="aaf9c-185">推奨事項:False に設定しないでください。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-185">Recommendation: Never set to false.</span></span>

<span data-ttu-id="aaf9c-186">既定で EnbableViewStateMac が設定を true にします。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-186">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="aaf9c-187">アプリケーションがビュー ステートを使用していない場合でも EnableViewStateMac を false に設定しないでください。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-187">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="aaf9c-188">この値を false に設定すると、アプリケーションがクロスサイト スクリプティングに対する脆弱性。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-188">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="aaf9c-189">ASP.NET 4.5.2 以降では、ランタイムが適用**EnableViewStateMac は true を =** します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-189">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="aaf9c-190">False に設定すると、場合でも、ランタイムはこの値は無視され、true に設定された値が続行されます。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-190">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="aaf9c-191">詳細については、次を参照してください。 [ASP.NET 4.5.2 および EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-191">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="aaf9c-192">次の例では、EnableViewStateMac を true に設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-192">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="aaf9c-193">実際には既定で true が true には、この値を設定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-193">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="aaf9c-194">ただし、設定した場合が false に任意のページで、アプリケーションで、この値をすぐに修正する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-194">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="aaf9c-195">中程度の信頼</span><span class="sxs-lookup"><span data-stu-id="aaf9c-195">Medium trust</span></span>

<span data-ttu-id="aaf9c-196">推奨事項:セキュリティ境界として、中程度の信頼 (または他の信頼レベル) に依存しない操作を行います。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-196">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="aaf9c-197">部分信頼では、アプリケーションを適切に保護できないは使用できません。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-197">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="aaf9c-198">代わりに、完全な信頼を使用し、個別のアプリケーション プールで信頼されていないアプリケーションを分離します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-198">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="aaf9c-199">また、一意の id では、各アプリケーション プールを実行します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-199">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="aaf9c-200">詳細については、次を参照してください。 [ASP.NET 部分信頼でアプリケーションの分離が保証されない](https://support.microsoft.com/kb/2698981)します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-200">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="aaf9c-201">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="aaf9c-201">&lt;appSettings&gt;</span></span>

<span data-ttu-id="aaf9c-202">推奨事項:セキュリティ設定を無効にしない&lt;appSettings&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-202">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="aaf9c-203">AppSettings 要素には、セキュリティ更新プログラムの必要な多くの値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-203">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="aaf9c-204">変更または、これらの値を無効にする必要がありますされません。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-204">You should not change or disable these values.</span></span> <span data-ttu-id="aaf9c-205">場合は、更新プログラムを展開するときに、これらの値を無効にする必要があります、すぐに再有効化展開が完了後します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-205">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="aaf9c-206">詳細については、次を参照してください。 [ASP.NET appSettings 要素](https://msdn.microsoft.com/library/hh975440.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-206">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="aaf9c-207">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="aaf9c-207">UrlPathEncode</span></span>

<span data-ttu-id="aaf9c-208">推奨事項:使用[UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx)代わりにします。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-208">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="aaf9c-209">UrlPathEncode メソッドは、非常に特定のブラウザーの互換性の問題を解決するのには、.NET Framework に追加されました。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-209">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="aaf9c-210">URL を適切にエンコードしないと、クロス サイト スクリプティングからアプリケーションを保護しません。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-210">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="aaf9c-211">アプリケーションで使用する必要がありますしないでください。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-211">You should never use it in your application.</span></span> <span data-ttu-id="aaf9c-212">代わりに、 [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-212">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="aaf9c-213">次の例では、ハイパーリンク コントロール用のクエリ文字列パラメーターとしてエンコードされた URL を渡す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-213">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="aaf9c-214">信頼性とパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="aaf9c-214">Reliability and performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="aaf9c-215">PreSendRequestHeaders と PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="aaf9c-215">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="aaf9c-216">推奨事項:マネージ モジュールでは、これらのイベントを使用しません。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-216">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="aaf9c-217">代わりに、必要なタスクを実行するネイティブ IIS モジュールを記述します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-217">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="aaf9c-218">参照してください[ネイティブ コード HTTP モジュールを作成して](https://msdn.microsoft.com/library/ms693629.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-218">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="aaf9c-219">使用することができます、 [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx)と[PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) IIS のネイティブ モジュールでのイベント。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-219">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="aaf9c-220">使用しない`PreSendRequestHeaders`と`PreSendRequestContent`を実装するマネージ モジュールで`IHttpModule`します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-220">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="aaf9c-221">これらのプロパティを設定すると、非同期要求で問題が発生することができます。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-221">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="aaf9c-222">アプリケーション要求ルーティング処理 (ARR) と websocket の組み合わせは、w3wp クラッシュを引き起こす可能性のあるアクセス違反例外に生じる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-222">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="aaf9c-223">たとえば、iiscore!W3_CONTEXT_BASE::GetIsLastNotification + iiscore.dll で 68 では、アクセス違反例外 (0xC0000005) が原因です。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-223">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="aaf9c-224">Web フォームでの非同期ページ イベント</span><span class="sxs-lookup"><span data-stu-id="aaf9c-224">Asynchronous page events with web forms</span></span>

<span data-ttu-id="aaf9c-225">推奨事項:Web フォームで非同期ページのライフ サイクル イベントの void メソッドの書き込みを避けるため、代わりに使用[Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx)非同期コードの。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-225">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="aaf9c-226">ページ イベントをマークすると**async**と**void**、非同期コードの実行が完了するとわかったことはできません。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-226">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="aaf9c-227">代わりに、Page.RegisterAsyncTask を使用して、その完了を追跡することができる方法で非同期コードを実行します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-227">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="aaf9c-228">ボタンは、次の例では、非同期コードを含むハンドラーをクリックします。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-228">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="aaf9c-229">この例では、推奨されるプラクティスではなく、非同期タスクの簡単な例としてのみ指定される、非同期的に文字列値の読み取り。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-229">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="aaf9c-230">非同期タスクを使用している場合は、4.5 (またはそれ以降) に Http ランタイムのターゲット フレームワークを設定、Web.config ファイルでします。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-230">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 (or later) in the Web.config file.</span></span> <span data-ttu-id="aaf9c-231">新しい同期コンテキストで 4.5 ターンにターゲット フレームワークを設定すると、.NET 4.5 で追加されました。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-231">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="aaf9c-232">この値は、Visual Studio で新しいプロジェクトで既定で設定されますが、設定になっていない既存のプロジェクトを使用している場合は。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-232">This value is set by default in new projects in Visual Studio, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="aaf9c-233">ファイア アンド フォーゲット操作します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-233">Fire-and-forget work</span></span>

<span data-ttu-id="aaf9c-234">推奨事項:ASP.NET 内の要求を処理する際は、ファイア アンド フォーゲット作業 (このような ThreadPool.QueueUserWorkItem メソッドの呼び出しまたはデリゲートを繰り返し呼び出すタイマーを作成する) を起動しないようにします。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-234">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="aaf9c-235">ファイア アンド フォーゲット ASP.NET 内で実行される作業アプリケーションの場合は、アプリケーションは同期で取得できます。いつでもでも、アプリ ドメインがあり、継続的なプロセスが、アプリケーションの現在の状態と一致して不要になったを破壊することができます。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-235">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="aaf9c-236">この種の ASP.NET の外部で作業を移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-236">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="aaf9c-237">進行中の作業を実行する Azure で Web ジョブ、Windows サービスまたはワーカー ロールを使用し、別のプロセスからそのコードを実行できます。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-237">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="aaf9c-238">という名前の Nuget パッケージを追加することができる場合、ASP.NET 内でこの作業を実行する必要があります[WebBackgrounder](http://www.nuget.org/packages/webbackgrounder)コードを実行します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-238">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="aaf9c-239">要求エンティティ本体</span><span class="sxs-lookup"><span data-stu-id="aaf9c-239">Request entity body</span></span>

<span data-ttu-id="aaf9c-240">推奨事項:ハンドラーのイベントを実行する前に、Request.Form または Request.InputStream の読み取りを回避します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-240">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="aaf9c-241">最も早い Request.Form または Request.InputStream から読み取る必要がありますが、ハンドラーの中にイベントを実行します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-241">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="aaf9c-242">MVC では、コント ローラーは、ハンドラーと、実行イベントは、アクション メソッドの実行時にします。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-242">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="aaf9c-243">Web フォームで、ページは、ハンドラーと、execute イベント Page.Init イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-243">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="aaf9c-244">要求エンティティ本体を execute イベントより前読み取る場合は、要求の処理干渉します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-244">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="aaf9c-245">Execute イベントの前に、要求エンティティ本体を読み取る場合は、いずれかを使用して、 [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx)または[Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-245">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="aaf9c-246">GetBufferlessInputStream を使用すると、要求から生のストリームを取得し、要求全体を処理するための責任を負うものです。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-246">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="aaf9c-247">呼び出し元 GetBufferlessInputStream、Request.Form および Request.InputStream は ASP.NET によって作成されてがいないために、使用できません後です。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-247">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="aaf9c-248">GetBufferedInputStream を使用する場合は、要求からストリームのコピーを取得します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-248">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="aaf9c-249">Request.Form と Request.InputStream が使用可能な要求の後で ASP.NET の他のコピーを設定します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-249">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="aaf9c-250">Response.Redirect と Response.End</span><span class="sxs-lookup"><span data-stu-id="aaf9c-250">Response.Redirect and Response.End</span></span>

<span data-ttu-id="aaf9c-251">推奨事項:スレッドを呼び出した後に処理する方法の相違点に注意してください[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-251">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="aaf9c-252">[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx)メソッド Response.End メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-252">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="aaf9c-253">同期プロセスでは、Request.Redirect を呼び出すとが、現在のスレッドがすぐに中止します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-253">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="aaf9c-254">ただし、非同期処理で Response.Redirect を呼び出すことは中止されない、現在のスレッドの要求は引き続きコードが実行されるようにします。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-254">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="aaf9c-255">非同期の処理では、コードの実行を停止するメソッドからタスクを返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-255">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="aaf9c-256">MVC プロジェクトでは、Response.Redirect を呼び出さないでください。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-256">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="aaf9c-257">代わりに、RedirectResult を返します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-257">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="aaf9c-258">EnableViewState およびに ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="aaf9c-258">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="aaf9c-259">推奨事項:EnableViewState、代わりに ViewStateMode を使用すると、コントロールがビュー ステートを使用する詳細な制御を提供します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-259">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="aaf9c-260">EnableViewState ページ ディレクティブで false に設定すると、ビュー ステートは、ページ内のすべてのコントロールで無効にされ、有効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-260">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="aaf9c-261">ページ内の特定のコントロールのみにビュー ステートを有効にする場合に ViewStateMode を無効にページの設定します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-261">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="aaf9c-262">次に、ビュー ステートを実際に必要なコントロールのみで有効に ViewStateMode を設定します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-262">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="aaf9c-263">ビュー ステートが必要なコントロールのみを有効にすると、web ページのビュー ステートのサイズを縮小できます。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-263">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="aaf9c-264">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="aaf9c-264">SqlMembershipProvider</span></span>

<span data-ttu-id="aaf9c-265">推奨事項:ユニバーサル プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-265">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="aaf9c-266">現在のプロジェクト テンプレートで SqlMembershipProvider 代わられました[ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers)、NuGet パッケージとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-266">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="aaf9c-267">SqlMembershipProvider テンプレートの以前のバージョンでビルドされたプロジェクトを使用している場合は、ユニバーサル プロバイダーに切り替える必要があります。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-267">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="aaf9c-268">Universal Providers Entity Framework でサポートされているすべてのデータベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-268">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="aaf9c-269">詳細については、次を参照してください。 [ASP.NET ユニバーサル プロバイダーの導入](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-269">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="aaf9c-270">実行時間の長い要求 (> 110 秒)</span><span class="sxs-lookup"><span data-stu-id="aaf9c-270">Long-running requests (>110 seconds)</span></span>

<span data-ttu-id="aaf9c-271">推奨事項:使用して、 [Websocket](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx)または[SignalR](../../../signalr/index.md)接続されているクライアントは、および非同期の I/O 操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-271">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="aaf9c-272">実行時間の長い要求は、web アプリケーションで予期しない結果とパフォーマンスの低下の原因になることができます。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-272">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="aaf9c-273">要求の既定のタイムアウト設定は、110 秒です。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-273">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="aaf9c-274">実行時間の長い要求でセッション状態を使用する場合、ASP.NET は 110 秒後にセッション オブジェクトのロックが解除されます。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-274">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="aaf9c-275">ただし、アプリケーションがありますセッション オブジェクトに対する操作の途中で、ロックがリリースされ、操作が正常に完了しない可能性。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-275">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="aaf9c-276">最初の要求の実行中に、ユーザーからの 2 番目の要求がブロックされている場合、2 番目の要求は不整合な状態でセッション オブジェクトにアクセス可能性があります。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-276">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="aaf9c-277">アプリケーションには、I/O 操作ブロックしている (または同期) にはが含まれている場合は、アプリケーションは応答できません。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-277">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="aaf9c-278">パフォーマンスを向上させるのには、.NET Framework の非同期 I/O 操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-278">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="aaf9c-279">また、サーバーにクライアントを接続するための Websocket や SignalR を使用します。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-279">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="aaf9c-280">これらの機能は、実行時間の長い要求を効率的に処理するために設計されています。</span><span class="sxs-lookup"><span data-stu-id="aaf9c-280">These features are designed to efficiently handle long-running requests.</span></span>
