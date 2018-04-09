---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: ASP.NET AJAX 認証およびプロファイル アプリケーション サービスを理解する |Microsoft ドキュメント
author: scottcate
description: 認証サービスは、認証 cookie を受信するために資格情報を提供することができ、ゲートウェイ サービスのカスタム ユーザーを許可するのには、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 0bf6538d0c4ae9488e6ac29ccba6d4b243cf070e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a><span data-ttu-id="e84bc-103">ASP.NET AJAX 認証およびプロファイル アプリケーション サービスを理解します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-103">Understanding ASP.NET AJAX Authentication and Profile Application Services</span></span>
====================
<span data-ttu-id="e84bc-104">によって[Scott カテゴリ](https://github.com/scottcate)</span><span class="sxs-lookup"><span data-stu-id="e84bc-104">by [Scott Cate](https://github.com/scottcate)</span></span>

[<span data-ttu-id="e84bc-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="e84bc-105">Download PDF</span></span>](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> <span data-ttu-id="e84bc-106">認証サービスでは、認証 cookie を受信するために資格情報を提供するユーザーとは、ASP.NET によって提供されるゲートウェイ サービスのカスタム ユーザー プロファイルを許可します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-106">The Authentication service allows users to provide credentials in order to receive an authentication cookie, and is the gateway service to allow custom user profiles provided by ASP.NET.</span></span> <span data-ttu-id="e84bc-107">ASP.NET AJAX 認証サービスの使用は標準の ASP.NET フォーム認証では、互換性のあるフォーム認証を使用中のアプリケーション (ログインに制御など) は分けることのできない AJAX 認証サービスをアップグレードすること。</span><span class="sxs-lookup"><span data-stu-id="e84bc-107">Use of the ASP.NET AJAX authentication service is compatible with standard ASP.NET Forms authentication, so applications currently using Forms authentication (such as with the Login control) will not be broken by upgrading to the AJAX authentication service.</span></span>


## <a name="introduction"></a><span data-ttu-id="e84bc-108">はじめに</span><span class="sxs-lookup"><span data-stu-id="e84bc-108">Introduction</span></span>

<span data-ttu-id="e84bc-109">.NET Framework 3.5 の一部として、Microsoft を生み出しているかなりの数の環境のアップグレードです。新しい開発環境があるだけでなく、統合言語クエリ (LINQ) の新機能とその他の言語拡張機能は近日公開予定です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-109">As part of the .NET Framework 3.5, Microsoft is delivering a sizable environment upgrade; not only is a new development environment available, but the new Language-Integrated Query (LINQ) features and other language enhancements are forthcoming.</span></span> <span data-ttu-id="e84bc-110">さらに、使い慣れたその他のツールセット、目立った例として、ASP.NET AJAX Extensions の機能の一部含まれている .NET Framework の基本クラス ライブラリのファースト クラスのメンバーとして。</span><span class="sxs-lookup"><span data-stu-id="e84bc-110">In addition, some familiar features of other toolsets, notably the ASP.NET AJAX Extensions, are being included as first-class members of the .NET Framework Base Class Library.</span></span> <span data-ttu-id="e84bc-111">これらの拡張機能は、ページ全体の更新、クライアント スクリプト (プロファイル API、ASP.NET を含む)、および広範なクライアント側 API を介して Web サービスにアクセスする機能を必要とせずにページの部分のレンダリングを含む、多くの新しいのリッチ クライアント機能を有効にします。ASP.NET サーバー側コントロール セットに表示するコントロール パターンの多くのミラーに設計されています。</span><span class="sxs-lookup"><span data-stu-id="e84bc-111">These extensions enable many new rich client features, including partial rendering of pages without requiring a full page refresh, the ability to access Web Services via client script (including the ASP.NET profiling API), and an extensive client-side API designed to mirror many of the control schemes seen in the ASP.NET server-side control set.</span></span>

<span data-ttu-id="e84bc-112">このホワイト ペーパーでは、実装と ASP.NET のプロファイルの使用、およびフォーム認証サービスは Microsoft ASP.NET AJAX ExtensionsThe AJAX 拡張機能で公開されているようにフォーム認証をサポートするには驚くほど簡単 (だけでなくプロファイリング サービス) は、Web サービス プロキシ スクリプトを通じて公開されます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-112">This whitepaper looks at the implementation and use of the ASP.NET Profiling and Forms Authentication services as they are exposed by the Microsoft ASP.NET AJAX ExtensionsThe AJAX Extensions make Forms authentication incredibly easy to support, as it (as well as the Profiling Service) is exposed through a Web Service proxy script.</span></span> <span data-ttu-id="e84bc-113">AJAX 拡張機能では、AuthenticationServiceManager クラスを通じてカスタムの認証もサポートします。</span><span class="sxs-lookup"><span data-stu-id="e84bc-113">The AJAX Extensions also support custom authentication through the AuthenticationServiceManager class.</span></span>

<span data-ttu-id="e84bc-114">このホワイト ペーパーでは、Visual Studio 2008 の Beta 2 リリースと、.NET Framework 3.5 に基づいています。</span><span class="sxs-lookup"><span data-stu-id="e84bc-114">This whitepaper is based on the Beta 2 release of the Visual Studio 2008 and the .NET Framework 3.5.</span></span> <span data-ttu-id="e84bc-115">このホワイト ペーパーには、いない Visual Web Developer Express、Visual Studio 2008 Beta 2 を操作して Visual Studio のユーザー インターフェイスに従ってチュートリアルを提供するも想定しています。</span><span class="sxs-lookup"><span data-stu-id="e84bc-115">This whitepaper also assumes that you will be working with Visual Studio 2008 Beta 2, not Visual Web Developer Express, and will provide walkthroughs according to the user interface of Visual Studio.</span></span> <span data-ttu-id="e84bc-116">一部のコード サンプルでは、Visual Web Developer Express で使用できないプロジェクト テンプレートを利用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-116">Some code samples may utilize project templates unavailable in Visual Web Developer Express.</span></span>

## <a name="profiles-and-authentication"></a><span data-ttu-id="e84bc-117">*プロファイルと認証*</span><span class="sxs-lookup"><span data-stu-id="e84bc-117">*Profiles and Authentication*</span></span>

<span data-ttu-id="e84bc-118">Microsoft ASP.NET プロファイルや認証サービス、ASP.NET フォーム認証システムによって提供され、ASP.NET の標準的なコンポーネントであります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-118">The Microsoft ASP.NET Profiles and Authentication services are provided by the ASP.NET Forms Authentication system, and are standard components of ASP.NET.</span></span> <span data-ttu-id="e84bc-119">ASP.NET AJAX 拡張機能は、クライアント AJAX ライブラリの Sys.Services の名前空間で非常に簡単なモデルを使用して、スクリプトのプロキシを介してこれらのサービスにスクリプトのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-119">The ASP.NET AJAX Extensions provide script access to these services via script proxies, through a fairly straightforward model under the Sys.Services namespace of the client AJAX library.</span></span>

<span data-ttu-id="e84bc-120">認証サービスでは、認証 cookie を受信するために資格情報を提供するユーザーとは、ASP.NET によって提供されるゲートウェイ サービスのカスタム ユーザー プロファイルを許可します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-120">The Authentication service allows users to provide credentials in order to receive an authentication cookie, and is the gateway service to allow custom user profiles provided by ASP.NET.</span></span> <span data-ttu-id="e84bc-121">ASP.NET AJAX 認証サービスの使用は標準の ASP.NET フォーム認証では、互換性のあるフォーム認証を使用中のアプリケーション (ログインに制御など) は分けることのできない AJAX 認証サービスをアップグレードすること。</span><span class="sxs-lookup"><span data-stu-id="e84bc-121">Use of the ASP.NET AJAX authentication service is compatible with standard ASP.NET Forms authentication, so applications currently using Forms authentication (such as with the Login control) will not be broken by upgrading to the AJAX authentication service.</span></span>

<span data-ttu-id="e84bc-122">プロファイル サービスは、自動統合および認証サービスによって提供されるように、メンバーシップに基づくユーザー データの記憶域を使用します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-122">The Profile service allows the automatic integration and storage of user data based on membership as provided by the Authentication service.</span></span> <span data-ttu-id="e84bc-123">格納されたデータは、web.config ファイルで指定して、さまざまなプロファイリング サービス プロバイダーがデータの管理を処理します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-123">The stored data is specified by the web.config file, and the various profiling service providers handle the data management.</span></span> <span data-ttu-id="e84bc-124">認証サービスと同様、AJAX プロファイル サービスは、ページが現在 ASP.NET プロファイル サービスの機能を組み込む必要がありますいないによって損なわれている AJAX のサポートを含むように、標準の ASP.NET プロファイル サービスとの互換性。</span><span class="sxs-lookup"><span data-stu-id="e84bc-124">As with the Authentication service, the AJAX Profile service is compatible with the standard ASP.NET profile service, so that pages currently incorporating features of the ASP.NET Profile service should not be broken by including AJAX support.</span></span>

<span data-ttu-id="e84bc-125">このホワイト ペーパーのスコープ外では、ASP.NET 認証とプロファイル サービス自体をアプリケーションに組み込むことです。</span><span class="sxs-lookup"><span data-stu-id="e84bc-125">Incorporating the ASP.NET Authentication and Profiling services themselves into an application is outside of the scope of this whitepaper.</span></span> <span data-ttu-id="e84bc-126">トピックの詳細については、MSDN ライブラリを参照してください参照資料にメンバーシップを使用したユーザーを管理する[ https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-126">For more information on the topic, see the MSDN Library reference article Managing Users by Using Membership at [https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx).</span></span> <span data-ttu-id="e84bc-127">ASP.NET には、ASP.NET メンバーシップの既定の認証サービス プロバイダーである SQL Server でのメンバーシップを自動的に設定するためのユーティリティも含まれています。</span><span class="sxs-lookup"><span data-stu-id="e84bc-127">ASP.NET also includes a utility to automatically set up Membership with a SQL Server, which is the default authentication service provider for ASP.NET Membership.</span></span> <span data-ttu-id="e84bc-128">詳細については、ASP.NET SQL Server の登録ツールの記事を参照してください (Aspnet\_regsql.exe) で[ https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-128">For more information, see the article ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe) at [https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).</span></span>

## <a name="using-the-aspnet-ajax-authentication-service"></a><span data-ttu-id="e84bc-129">*ASP.NET AJAX 認証サービスを使用します。*</span><span class="sxs-lookup"><span data-stu-id="e84bc-129">*Using the ASP.NET AJAX Authentication Service*</span></span>

<span data-ttu-id="e84bc-130">Web.config ファイルでは、ASP.NET AJAX 認証サービスを有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-130">The ASP.NET AJAX Authentication service must be enabled in the web.config file:</span></span>

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

<span data-ttu-id="e84bc-131">認証サービスでは、ASP.NET フォーム認証を有効にするを必要とし、cookie (スクリプトを有効にできません cookie なしのセッション cookie なしのセッションには、URL パラメーターが必要としな) クライアントのブラウザーで有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-131">The Authentication service requires ASP.NET Forms authentication to be enabled and requires cookies to be enabled on the client browser (a script cannot enable a cookieless session since cookieless sessions require URL parameters).</span></span>

<span data-ttu-id="e84bc-132">AJAX 認証サービスが有効になっているし、構成されている、クライアント スクリプトすぐに活用できます Sys.Services.AuthenticationService オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="e84bc-132">Once the AJAX authentication service is enabled and configured, client script can immediately take advantage of the Sys.Services.AuthenticationService object.</span></span> <span data-ttu-id="e84bc-133">クライアント スクリプトを活用するためにしますが、主に、`login`メソッドおよび`isLoggedIn`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="e84bc-133">Primarily, client script will want to take advantage of the `login` method and `isLoggedIn` property.</span></span> <span data-ttu-id="e84bc-134">いくつかのプロパティは、既定値メソッドを指定する、ログイン、多数のパラメーターを受け取ることができますが存在します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-134">Several properties exist to provide defaults for the login method, which can accept a large number of parameters.</span></span>

<span data-ttu-id="e84bc-135">*Sys.Services.AuthenticationService メンバー*</span><span class="sxs-lookup"><span data-stu-id="e84bc-135">*Sys.Services.AuthenticationService members*</span></span>

<span data-ttu-id="e84bc-136">*ログイン方法:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-136">*login method:*</span></span>

<span data-ttu-id="e84bc-137">Login() メソッドでは、ユーザーの資格情報を認証するための要求を開始します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-137">The login() method begins a request to authenticate the user's credentials.</span></span> <span data-ttu-id="e84bc-138">このメソッドは、非同期実行をブロックしません。</span><span class="sxs-lookup"><span data-stu-id="e84bc-138">This method is asynchronous and does not block execution.</span></span>

<span data-ttu-id="e84bc-139">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-139">*Parameters:*</span></span>

| <span data-ttu-id="e84bc-140">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="e84bc-140">**Parameter Name**</span></span> | <span data-ttu-id="e84bc-141">**意味**</span><span class="sxs-lookup"><span data-stu-id="e84bc-141">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="e84bc-142">userName</span><span class="sxs-lookup"><span data-stu-id="e84bc-142">userName</span></span> | <span data-ttu-id="e84bc-143">必須。</span><span class="sxs-lookup"><span data-stu-id="e84bc-143">Required.</span></span> <span data-ttu-id="e84bc-144">認証にユーザー名。</span><span class="sxs-lookup"><span data-stu-id="e84bc-144">The username to authenticate.</span></span> |
| <span data-ttu-id="e84bc-145">パスワード</span><span class="sxs-lookup"><span data-stu-id="e84bc-145">password</span></span> | <span data-ttu-id="e84bc-146">省略可能な (null の既定値)。</span><span class="sxs-lookup"><span data-stu-id="e84bc-146">Optional (defaults to null).</span></span> <span data-ttu-id="e84bc-147">ユーザーのパスワード。</span><span class="sxs-lookup"><span data-stu-id="e84bc-147">The user's password.</span></span> |
| <span data-ttu-id="e84bc-148">isPersistent</span><span class="sxs-lookup"><span data-stu-id="e84bc-148">isPersistent</span></span> | <span data-ttu-id="e84bc-149">省略可能な (既定値は false)。</span><span class="sxs-lookup"><span data-stu-id="e84bc-149">Optional (defaults to false).</span></span> <span data-ttu-id="e84bc-150">かどうか、ユーザーの認証の cookie は、セッション間で保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-150">Whether the user's authentication cookie should persist across sessions.</span></span> <span data-ttu-id="e84bc-151">False の場合、ユーザーがログアウト、ブラウザーが閉じているか、セッションの有効期限が切れるときにします。</span><span class="sxs-lookup"><span data-stu-id="e84bc-151">If false, the user will log out when the browser is closed or the session expires.</span></span> |
| <span data-ttu-id="e84bc-152">redirectUrl</span><span class="sxs-lookup"><span data-stu-id="e84bc-152">redirectUrl</span></span> | <span data-ttu-id="e84bc-153">省略可能な (null の既定値)。正常な認証時にブラウザーをリダイレクトする URL。</span><span class="sxs-lookup"><span data-stu-id="e84bc-153">Optional (defaults to null).The URL to redirect the browser to upon successful authentication.</span></span> <span data-ttu-id="e84bc-154">このパラメーターが null または空の文字列の場合は、リダイレクトは行われません。</span><span class="sxs-lookup"><span data-stu-id="e84bc-154">If this parameter is null or an empty string, no redirection occurs.</span></span> |
| <span data-ttu-id="e84bc-155">customInfo</span><span class="sxs-lookup"><span data-stu-id="e84bc-155">customInfo</span></span> | <span data-ttu-id="e84bc-156">省略可能な (null の既定値)。</span><span class="sxs-lookup"><span data-stu-id="e84bc-156">Optional (defaults to null).</span></span> <span data-ttu-id="e84bc-157">このパラメーターは、現在使用されていないは将来使用するために予約されています。</span><span class="sxs-lookup"><span data-stu-id="e84bc-157">This parameter is currently unused and is reserved for future use.</span></span> |
| <span data-ttu-id="e84bc-158">loginCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="e84bc-158">loginCompletedCallback</span></span> | <span data-ttu-id="e84bc-159">省略可能な (null の既定値)。ログインが正常に完了したときに呼び出す関数。</span><span class="sxs-lookup"><span data-stu-id="e84bc-159">Optional (defaults to null).The function to call when the login has successfully completed.</span></span> <span data-ttu-id="e84bc-160">指定する場合、このパラメーターは defaultLoginCompleted プロパティを上書きします。</span><span class="sxs-lookup"><span data-stu-id="e84bc-160">If specified, this parameter overrides the defaultLoginCompleted property.</span></span> |
| <span data-ttu-id="e84bc-161">failedCallback</span><span class="sxs-lookup"><span data-stu-id="e84bc-161">failedCallback</span></span> | <span data-ttu-id="e84bc-162">省略可能な (null の既定値)。ログインが失敗したときに呼び出される関数。</span><span class="sxs-lookup"><span data-stu-id="e84bc-162">Optional (defaults to null).The function to call when the login has failed.</span></span> <span data-ttu-id="e84bc-163">指定する場合、このパラメーターは defaultFailedCallback プロパティを上書きします。</span><span class="sxs-lookup"><span data-stu-id="e84bc-163">If specified, this parameter overrides the defaultFailedCallback property.</span></span> |
| <span data-ttu-id="e84bc-164">userContext</span><span class="sxs-lookup"><span data-stu-id="e84bc-164">userContext</span></span> | <span data-ttu-id="e84bc-165">省略可能な (null の既定値)。</span><span class="sxs-lookup"><span data-stu-id="e84bc-165">Optional (defaults to null).</span></span> <span data-ttu-id="e84bc-166">コールバック関数に渡す必要があるカスタム ユーザー コンテキスト データ。</span><span class="sxs-lookup"><span data-stu-id="e84bc-166">Custom user context data that should be passed to the callback functions.</span></span> |

<span data-ttu-id="e84bc-167">*戻り値。*</span><span class="sxs-lookup"><span data-stu-id="e84bc-167">*Return Value:*</span></span>

<span data-ttu-id="e84bc-168">この関数では、戻り値は含まれません。</span><span class="sxs-lookup"><span data-stu-id="e84bc-168">This function does not include a return value.</span></span> <span data-ttu-id="e84bc-169">ただし、動作の数はこの関数に対する呼び出しの完了時に説明します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-169">However, a number of behaviors are included upon completion of a call to this function:</span></span>

- <span data-ttu-id="e84bc-170">現在のページが更新されますか、または場合、変更する、`redirectUrl`パラメーターが null でも、空の文字列。</span><span class="sxs-lookup"><span data-stu-id="e84bc-170">The current page will either be refreshed or be changed if the `redirectUrl` parameter was neither null nor an empty string.</span></span>
- <span data-ttu-id="e84bc-171">ただし、パラメーターが null または空の文字列の場合、`loginCompletedCallback`パラメーター、または`defaultLoginCompletedCallback`プロパティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-171">However, if the parameter was null or an empty string, the `loginCompletedCallback` parameter, or `defaultLoginCompletedCallback` property is called.</span></span>
- <span data-ttu-id="e84bc-172">Web サービスへの呼び出しに失敗した場合、`failedCallback`のパラメーター`defaultFailedCallback`プロパティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-172">If the call to the web service fails, the `failedCallback` parameter of `defaultFailedCallback` property is called.</span></span>

<span data-ttu-id="e84bc-173">*logout メソッド:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-173">*logout method:*</span></span>

<span data-ttu-id="e84bc-174">Logout() メソッドは、資格情報 cookie を削除し、web アプリケーションから現在のユーザーをログアウトします。</span><span class="sxs-lookup"><span data-stu-id="e84bc-174">The logout() method removes the credentials cookie and logs out the current user from the web application.</span></span>

<span data-ttu-id="e84bc-175">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-175">*Parameters:*</span></span>

| <span data-ttu-id="e84bc-176">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="e84bc-176">**Parameter Name**</span></span> | <span data-ttu-id="e84bc-177">**意味**</span><span class="sxs-lookup"><span data-stu-id="e84bc-177">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="e84bc-178">redirectUrl</span><span class="sxs-lookup"><span data-stu-id="e84bc-178">redirectUrl</span></span> | <span data-ttu-id="e84bc-179">省略可能な (null の既定値)。正常な認証時にブラウザーをリダイレクトする URL。</span><span class="sxs-lookup"><span data-stu-id="e84bc-179">Optional (defaults to null).The URL to redirect the browser to upon successful authentication.</span></span> <span data-ttu-id="e84bc-180">このパラメーターが null または空の文字列の場合は、リダイレクトは行われません。</span><span class="sxs-lookup"><span data-stu-id="e84bc-180">If this parameter is null or an empty string, no redirection occurs.</span></span> |
| <span data-ttu-id="e84bc-181">logoutCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="e84bc-181">logoutCompletedCallback</span></span> | <span data-ttu-id="e84bc-182">省略可能な (null の既定値)。ログアウトが正常に完了したときに呼び出す関数。</span><span class="sxs-lookup"><span data-stu-id="e84bc-182">Optional (defaults to null).The function to call when the logout has successfully completed.</span></span> <span data-ttu-id="e84bc-183">指定する場合、このパラメーターは defaultLogoutCompleted プロパティを上書きします。</span><span class="sxs-lookup"><span data-stu-id="e84bc-183">If specified, this parameter overrides the defaultLogoutCompleted property.</span></span> |
| <span data-ttu-id="e84bc-184">failedCallback</span><span class="sxs-lookup"><span data-stu-id="e84bc-184">failedCallback</span></span> | <span data-ttu-id="e84bc-185">省略可能な (null の既定値)。ログインが失敗したときに呼び出される関数。</span><span class="sxs-lookup"><span data-stu-id="e84bc-185">Optional (defaults to null).The function to call when the login has failed.</span></span> <span data-ttu-id="e84bc-186">指定する場合、このパラメーターは defaultFailedCallback プロパティを上書きします。</span><span class="sxs-lookup"><span data-stu-id="e84bc-186">If specified, this parameter overrides the defaultFailedCallback property.</span></span> |
| <span data-ttu-id="e84bc-187">userContext</span><span class="sxs-lookup"><span data-stu-id="e84bc-187">userContext</span></span> | <span data-ttu-id="e84bc-188">省略可能な (null の既定値)。</span><span class="sxs-lookup"><span data-stu-id="e84bc-188">Optional (defaults to null).</span></span> <span data-ttu-id="e84bc-189">コールバック関数に渡す必要があるカスタム ユーザー コンテキスト データ。</span><span class="sxs-lookup"><span data-stu-id="e84bc-189">Custom user context data that should be passed to the callback functions.</span></span> |

<span data-ttu-id="e84bc-190">*戻り値。*</span><span class="sxs-lookup"><span data-stu-id="e84bc-190">*Return Value:*</span></span>

<span data-ttu-id="e84bc-191">この関数では、戻り値は含まれません。</span><span class="sxs-lookup"><span data-stu-id="e84bc-191">This function does not include a return value.</span></span> <span data-ttu-id="e84bc-192">ただし、動作の数はこの関数に対する呼び出しの完了時に説明します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-192">However, a number of behaviors are included upon completion of a call to this function:</span></span>

- <span data-ttu-id="e84bc-193">現在のページが更新されますか、または場合、変更する、`redirectUrl`パラメーターが null でも、空の文字列。</span><span class="sxs-lookup"><span data-stu-id="e84bc-193">The current page will either be refreshed or be changed if the `redirectUrl` parameter was neither null nor an empty string.</span></span>
- <span data-ttu-id="e84bc-194">ただし、パラメーターが null または空の文字列の場合、`logoutCompletedCallback`パラメーター、または`defaultLogoutCompletedCallback`プロパティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-194">However, if the parameter was null or an empty string, the `logoutCompletedCallback` parameter, or `defaultLogoutCompletedCallback` property is called.</span></span>
- <span data-ttu-id="e84bc-195">Web サービスへの呼び出しに失敗した場合、`failedCallback`のパラメーター`defaultFailedCallback`プロパティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-195">If the call to the web service fails, the `failedCallback` parameter of `defaultFailedCallback` property is called.</span></span>

<span data-ttu-id="e84bc-196">*defaultFailedCallback プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="e84bc-196">*defaultFailedCallback property (get, set):*</span></span>

<span data-ttu-id="e84bc-197">このプロパティは、web サービスとの通信に障害が発生した場合に呼び出す必要があります関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-197">This property specifies a function that should be called if a failure to communicate with the web service occurs.</span></span> <span data-ttu-id="e84bc-198">デリゲート (または関数の参照) を受信してする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-198">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="e84bc-199">このプロパティで指定された関数の参照は、次のシグネチャが必要です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-199">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

<span data-ttu-id="e84bc-200">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-200">*Parameters:*</span></span>

| <span data-ttu-id="e84bc-201">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="e84bc-201">**Parameter Name**</span></span> | <span data-ttu-id="e84bc-202">**意味**</span><span class="sxs-lookup"><span data-stu-id="e84bc-202">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="e84bc-203">エラー</span><span class="sxs-lookup"><span data-stu-id="e84bc-203">error</span></span> | <span data-ttu-id="e84bc-204">エラー情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-204">Specifies the error information.</span></span> |
| <span data-ttu-id="e84bc-205">userContext</span><span class="sxs-lookup"><span data-stu-id="e84bc-205">userContext</span></span> | <span data-ttu-id="e84bc-206">ログインまたはログアウト関数が呼び出されたときを指定されたユーザー コンテキスト情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-206">Specifies the user context information provided when the login or logout function was called.</span></span> |
| <span data-ttu-id="e84bc-207">methodName</span><span class="sxs-lookup"><span data-stu-id="e84bc-207">methodName</span></span> | <span data-ttu-id="e84bc-208">呼び出し元のメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="e84bc-208">The name of the calling method.</span></span> |

<span data-ttu-id="e84bc-209">*defaultLoginCompletedCallback プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="e84bc-209">*defaultLoginCompletedCallback property (get, set):*</span></span>

<span data-ttu-id="e84bc-210">このプロパティは、ログインの web サービス呼び出しが完了したときに呼び出される関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-210">This property specifies a function that should be called when the login web service call has completed.</span></span> <span data-ttu-id="e84bc-211">デリゲート (または関数の参照) を受信してする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-211">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="e84bc-212">このプロパティで指定された関数の参照は、次のシグネチャが必要です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-212">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

<span data-ttu-id="e84bc-213">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-213">*Parameters:*</span></span>

| <span data-ttu-id="e84bc-214">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="e84bc-214">**Parameter Name**</span></span> | <span data-ttu-id="e84bc-215">**意味**</span><span class="sxs-lookup"><span data-stu-id="e84bc-215">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="e84bc-216">validCredentials</span><span class="sxs-lookup"><span data-stu-id="e84bc-216">validCredentials</span></span> | <span data-ttu-id="e84bc-217">ユーザーが有効な資格情報を指定するかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-217">Specifies whether the user provided valid credentials.</span></span> <span data-ttu-id="e84bc-218">`true` ユーザーが正常にログインした場合それ以外の場合`false`です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-218">`true` if the user successfully logged in; otherwise `false`.</span></span> |
| <span data-ttu-id="e84bc-219">userContext</span><span class="sxs-lookup"><span data-stu-id="e84bc-219">userContext</span></span> | <span data-ttu-id="e84bc-220">ログイン関数が呼び出されたときを指定されたユーザー コンテキスト情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-220">Specifies the user context information provided when the login function was called.</span></span> |
| <span data-ttu-id="e84bc-221">methodName</span><span class="sxs-lookup"><span data-stu-id="e84bc-221">methodName</span></span> | <span data-ttu-id="e84bc-222">呼び出し元のメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="e84bc-222">The name of the calling method.</span></span> |

<span data-ttu-id="e84bc-223">*defaultLogoutCompletedCallback プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="e84bc-223">*defaultLogoutCompletedCallback property (get, set):*</span></span>

<span data-ttu-id="e84bc-224">このプロパティは、ログアウト web サービス呼び出しが完了したときに呼び出される関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-224">This property specifies a function that should be called when the logout web service call has completed.</span></span> <span data-ttu-id="e84bc-225">デリゲート (または関数の参照) を受信してする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-225">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="e84bc-226">このプロパティで指定された関数の参照は、次のシグネチャが必要です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-226">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

<span data-ttu-id="e84bc-227">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-227">*Parameters:*</span></span>

| <span data-ttu-id="e84bc-228">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="e84bc-228">**Parameter Name**</span></span> | <span data-ttu-id="e84bc-229">**意味**</span><span class="sxs-lookup"><span data-stu-id="e84bc-229">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="e84bc-230">結果</span><span class="sxs-lookup"><span data-stu-id="e84bc-230">result</span></span> | <span data-ttu-id="e84bc-231">このパラメーターは必ず`null`; 将来使用するために予約されています。</span><span class="sxs-lookup"><span data-stu-id="e84bc-231">This parameter will always be `null`; it is reserved for future use.</span></span> |
| <span data-ttu-id="e84bc-232">userContext</span><span class="sxs-lookup"><span data-stu-id="e84bc-232">userContext</span></span> | <span data-ttu-id="e84bc-233">ログイン関数が呼び出されたときを指定されたユーザー コンテキスト情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-233">Specifies the user context information provided when the login function was called.</span></span> |
| <span data-ttu-id="e84bc-234">methodName</span><span class="sxs-lookup"><span data-stu-id="e84bc-234">methodName</span></span> | <span data-ttu-id="e84bc-235">呼び出し元のメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="e84bc-235">The name of the calling method.</span></span> |

<span data-ttu-id="e84bc-236">*isLoggedIn プロパティ (get):*</span><span class="sxs-lookup"><span data-stu-id="e84bc-236">*isLoggedIn property (get):*</span></span>

<span data-ttu-id="e84bc-237">このプロパティは、ユーザーの現在の認証状態を取得します。ページの要求中には、ScriptManager をオブジェクトで設定されます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-237">This property gets the current authentication state of the user; it is set by the ScriptManager object during a page request.</span></span>

<span data-ttu-id="e84bc-238">このプロパティを返します`true`を返しますそれ以外の場合、ユーザーは現在ログインしている場合、`false`です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-238">This property returns `true` if the user is currently logged in; otherwise, it returns `false`.</span></span>

<span data-ttu-id="e84bc-239">*path プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="e84bc-239">*path property (get, set):*</span></span>

<span data-ttu-id="e84bc-240">このプロパティは、認証 web サービスの場所をプログラムによって決まります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-240">This property programmatically determines the location of the authentication web service.</span></span> <span data-ttu-id="e84bc-241">既定の認証プロバイダーと 1 つをオーバーライドするために使用できる ScriptManager コントロールの認証サービスの子ノードのパスのプロパティで宣言によって設定 (詳細については、使用方法を参照してくださいカスタム認証サービス プロバイダー。下記のトピック) です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-241">It can be used to override the default authentication provider, as well as one set declaratively in the Path property of the ScriptManager control's AuthenticationService child node (for more information, see the Using a Custom Authentication Service Provider topic below).</span></span>

<span data-ttu-id="e84bc-242">既定の認証サービスの場所で変更されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e84bc-242">Note that the location of the default authentication service does not change.</span></span> <span data-ttu-id="e84bc-243">ただし、ASP.NET AJAX には、ASP.NET AJAX 認証サービスのプロキシと同じクラス インターフェイスを提供する web サービスの場所を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-243">However, ASP.NET AJAX allows you to specify the location of a web service that provides the same class interface as the ASP.NET AJAX authentication service proxy.</span></span>

<span data-ttu-id="e84bc-244">このプロパティを現在のサイトからスクリプト要求に指示する値に設定されないことにも注意してください。</span><span class="sxs-lookup"><span data-stu-id="e84bc-244">Note also that this property should not be set to a value that directs the script request off of the current site.</span></span> <span data-ttu-id="e84bc-245">現在のアプリケーションでは、認証資格情報が表示されないが、そのことは無意味です。また、テクノロジの基になる AJAX クロスサイト リクエストを投稿してはいけません、クライアントのブラウザーにセキュリティ例外を生成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-245">Because the current application would not receive the authentication credentials, it would be useless; also, the technology underlying AJAX should not post cross-site requests, and may generate a security exception in the client browser.</span></span>

<span data-ttu-id="e84bc-246">このプロパティは、`String`認証 web サービスへのパスを表すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="e84bc-246">This property is a `String` object representing the path to the authentication web service.</span></span>

<span data-ttu-id="e84bc-247">*timeout プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="e84bc-247">*timeout property (get, set):*</span></span>

<span data-ttu-id="e84bc-248">このプロパティは、認証サービスのログイン要求と想定するまで待機する時間の長さが失敗したを決定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-248">This property determines the length of time to wait for the authentication service before assuming the login request has failed.</span></span> <span data-ttu-id="e84bc-249">呼び出しが完了するを待機中にタイムアウトになると、要求が失敗しましたコールバックが呼び出されます、呼び出しは完了していません。</span><span class="sxs-lookup"><span data-stu-id="e84bc-249">If the timeout expires while waiting for a call to complete, the request-failed callback will be called, and the call will not complete.</span></span>

<span data-ttu-id="e84bc-250">このプロパティは、`Number`認証サービスからの結果を待機するミリ秒数を表すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="e84bc-250">This property is a `Number` object representing the number of milliseconds to wait for results from the authentication service.</span></span>

<span data-ttu-id="e84bc-251">*認証サービスにログインするコード サンプル。*</span><span class="sxs-lookup"><span data-stu-id="e84bc-251">*Code Sample: Logging into the Authentication Service*</span></span>

<span data-ttu-id="e84bc-252">次のマークアップは、単純なスクリプトの呼び出し、ログインとログアウト クラスのメソッドに、認証サービスで ASP.NET ページの例です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-252">The following markup is an example ASP.NET page with a simple script call to the login and logout methods of the AuthenticationService class.</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a><span data-ttu-id="e84bc-253">ASP.NET AJAX を使用してデータをプロファイルにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="e84bc-253">Accessing ASP.NET Profiling Data via AJAX</span></span>

<span data-ttu-id="e84bc-254">サービスのプロファイリング、ASP.NET は、ASP.NET AJAX Extensions を通じて公開されています。</span><span class="sxs-lookup"><span data-stu-id="e84bc-254">The ASP.NET profiling service is also exposed through the ASP.NET AJAX Extensions.</span></span> <span data-ttu-id="e84bc-255">ASP.NET プロファイリング サービスには、ユーザー データを格納および取得する、細分化された豊富な API が用意されています、ため、優れた生産性ツールを指定できます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-255">Since the ASP.NET profiling service provides a rich, granular API by which to store and retrieve user data, this can be an excellent productivity tool.</span></span>

<span data-ttu-id="e84bc-256">Web.config; で、プロファイル サービスを有効にする必要があります。既定ではありません。</span><span class="sxs-lookup"><span data-stu-id="e84bc-256">The profile service must be enabled in web.config; it is not by default.</span></span> <span data-ttu-id="e84bc-257">これを行うことを確認、`profileService`子要素が有効になっている = true に指定された、web.config ファイルとプロパティの読み取りまたは次のように書き込まれることができますが指定されています。</span><span class="sxs-lookup"><span data-stu-id="e84bc-257">To do so, ensure that the `profileService` child element has enabled= true specified in web.config, and that you have specified which properties can be read or written as follows:</span></span>

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

<span data-ttu-id="e84bc-258">プロファイル サービスも構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-258">The profile service must also be configured.</span></span> <span data-ttu-id="e84bc-259">プロファイル サービスの構成は、このホワイト ペーパーのスコープ外では、する意義があるグループ プロファイルの構成設定で定義されているをグループ名のサブ プロパティとしてアクセス可能になることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e84bc-259">Although configuration of the profiling service is outside of the scope of this whitepaper, it is worthwhile to note that groups as defined in profile configuration settings will be accessible as sub-properties of the group name.</span></span> <span data-ttu-id="e84bc-260">たとえば、次にプロファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-260">For example, with the following profile section specified:</span></span>

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

<span data-ttu-id="e84bc-261">名前、およびアクセスする Address.Line1、Address.Line2、Address.City、Address.State、Address.Zip、BackgroundColor ProfileService クラスのプロパティ フィールドのプロパティとしてできるは、クライアント スクリプト。</span><span class="sxs-lookup"><span data-stu-id="e84bc-261">Client script would be able to access Name, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip, and BackgroundColor as properties of the properties field of the ProfileService class.</span></span>

<span data-ttu-id="e84bc-262">AJAX プロファイリング サービスが構成されたら、すぐにで使用できるページです。ただし、使用する前に 1 回読み込む必要があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-262">Once the AJAX Profiling Service is configured, it will be immediately available in pages; however, it will have to be loaded once before use.</span></span>

<span data-ttu-id="e84bc-263">*Sys.Services.ProfileService メンバー*</span><span class="sxs-lookup"><span data-stu-id="e84bc-263">*Sys.Services.ProfileService members*</span></span>

<span data-ttu-id="e84bc-264">*プロパティ フィールド:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-264">*properties field:*</span></span>

<span data-ttu-id="e84bc-265">プロパティ フィールドは、ドット演算子名規約によって参照可能な子のプロパティとして構成済みのプロファイルのすべてのデータを公開します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-265">The properties field exposes all configured profile data as child properties that can be referenced by the dot-operator-name convention.</span></span> <span data-ttu-id="e84bc-266">プロパティ グループの子であるプロパティは、"GroupName.PropertyName"と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-266">Properties that are children of property groups are referred to as GroupName.PropertyName.</span></span> <span data-ttu-id="e84bc-267">上に示した例のプロファイルの構成で、ユーザーの状態を取得するに、次の識別子が使用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-267">In the example profile configuration presented above, to get the state of the user, you could use the following identifier:</span></span>

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

<span data-ttu-id="e84bc-268">*load メソッド:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-268">*load method:*</span></span>

<span data-ttu-id="e84bc-269">サーバーから、選択した一覧またはすべてのプロパティを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-269">Loads a selected list or all properties from the server.</span></span>

<span data-ttu-id="e84bc-270">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-270">*Parameters:*</span></span>

| <span data-ttu-id="e84bc-271">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="e84bc-271">**Parameter Name**</span></span> | <span data-ttu-id="e84bc-272">**意味**</span><span class="sxs-lookup"><span data-stu-id="e84bc-272">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="e84bc-273">propertyNames</span><span class="sxs-lookup"><span data-stu-id="e84bc-273">propertyNames</span></span> | <span data-ttu-id="e84bc-274">省略可能な (null の既定値)。</span><span class="sxs-lookup"><span data-stu-id="e84bc-274">Optional (defaults to null).</span></span> <span data-ttu-id="e84bc-275">サーバーから読み込まれるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="e84bc-275">The properties to be loaded from the server.</span></span> |
| <span data-ttu-id="e84bc-276">loadCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="e84bc-276">loadCompletedCallback</span></span> | <span data-ttu-id="e84bc-277">省略可能な (null の既定値)。</span><span class="sxs-lookup"><span data-stu-id="e84bc-277">Optional (defaults to null).</span></span> <span data-ttu-id="e84bc-278">読み込みが完了したときに呼び出す関数。</span><span class="sxs-lookup"><span data-stu-id="e84bc-278">The function to call when loading has completed.</span></span> |
| <span data-ttu-id="e84bc-279">failedCallback</span><span class="sxs-lookup"><span data-stu-id="e84bc-279">failedCallback</span></span> | <span data-ttu-id="e84bc-280">省略可能な (null の既定値)。</span><span class="sxs-lookup"><span data-stu-id="e84bc-280">Optional (defaults to null).</span></span> <span data-ttu-id="e84bc-281">エラーが発生した場合に呼び出される関数。</span><span class="sxs-lookup"><span data-stu-id="e84bc-281">The function to call if an error occurs.</span></span> |
| <span data-ttu-id="e84bc-282">userContext</span><span class="sxs-lookup"><span data-stu-id="e84bc-282">userContext</span></span> | <span data-ttu-id="e84bc-283">省略可能な (null の既定値)。</span><span class="sxs-lookup"><span data-stu-id="e84bc-283">Optional (defaults to null).</span></span> <span data-ttu-id="e84bc-284">コールバック関数に渡されるコンテキスト情報。</span><span class="sxs-lookup"><span data-stu-id="e84bc-284">Context information to be passed to the callback function.</span></span> |

<span data-ttu-id="e84bc-285">Load 関数の戻り値ではありません。</span><span class="sxs-lookup"><span data-stu-id="e84bc-285">The load function does not have a return value.</span></span> <span data-ttu-id="e84bc-286">正常に完了しました、これは呼び出しいずれか、`loadCompletedCallback`パラメーターまたは`defaultLoadCompletedCallback`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="e84bc-286">If the call completed successfully, it will call either the `loadCompletedCallback` parameter or `defaultLoadCompletedCallback` property.</span></span> <span data-ttu-id="e84bc-287">呼び出しが失敗した、またはいずれかのタイムアウトの期限が切れた場合、`failedCallback`パラメーターまたは`defaultFailedCallback`プロパティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-287">If the call failed, or the timeout expired, either the `failedCallback` parameter or `defaultFailedCallback` property will be called.</span></span>

<span data-ttu-id="e84bc-288">場合、`propertyNames`パラメーターが指定されていない、すべての読み取りに構成されたプロパティは、サーバーから取得されます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-288">If the `propertyNames` parameter is not supplied, all read-configured properties are retrieved from the server.</span></span>

<span data-ttu-id="e84bc-289">*メソッドを保存します。*</span><span class="sxs-lookup"><span data-stu-id="e84bc-289">*save method:*</span></span>

<span data-ttu-id="e84bc-290">Save() メソッドは、指定したプロパティ一覧 (またはすべてのプロパティ) をユーザーの ASP.NET のプロファイルに保存します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-290">The save() method saves the specified property list (or all properties) to the user's ASP.NET profile.</span></span>

<span data-ttu-id="e84bc-291">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-291">*Parameters:*</span></span>

| <span data-ttu-id="e84bc-292">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="e84bc-292">**Parameter Name**</span></span> | <span data-ttu-id="e84bc-293">**意味**</span><span class="sxs-lookup"><span data-stu-id="e84bc-293">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="e84bc-294">propertyNames</span><span class="sxs-lookup"><span data-stu-id="e84bc-294">propertyNames</span></span> | <span data-ttu-id="e84bc-295">省略可能な (null の既定値)。</span><span class="sxs-lookup"><span data-stu-id="e84bc-295">Optional (defaults to null).</span></span> <span data-ttu-id="e84bc-296">サーバーに保存するプロパティです。</span><span class="sxs-lookup"><span data-stu-id="e84bc-296">The properties to be saved to the server.</span></span> |
| <span data-ttu-id="e84bc-297">saveCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="e84bc-297">saveCompletedCallback</span></span> | <span data-ttu-id="e84bc-298">省略可能な (null の既定値)。</span><span class="sxs-lookup"><span data-stu-id="e84bc-298">Optional (defaults to null).</span></span> <span data-ttu-id="e84bc-299">保存するときに呼び出される関数が完了しました。</span><span class="sxs-lookup"><span data-stu-id="e84bc-299">The function to call when saving has completed.</span></span> |
| <span data-ttu-id="e84bc-300">failedCallback</span><span class="sxs-lookup"><span data-stu-id="e84bc-300">failedCallback</span></span> | <span data-ttu-id="e84bc-301">省略可能な (null の既定値)。</span><span class="sxs-lookup"><span data-stu-id="e84bc-301">Optional (defaults to null).</span></span> <span data-ttu-id="e84bc-302">エラーが発生した場合に呼び出される関数。</span><span class="sxs-lookup"><span data-stu-id="e84bc-302">The function to call if an error occurs.</span></span> |
| <span data-ttu-id="e84bc-303">userContext</span><span class="sxs-lookup"><span data-stu-id="e84bc-303">userContext</span></span> | <span data-ttu-id="e84bc-304">省略可能な (null の既定値)。</span><span class="sxs-lookup"><span data-stu-id="e84bc-304">Optional (defaults to null).</span></span> <span data-ttu-id="e84bc-305">コールバック関数に渡されるコンテキスト情報。</span><span class="sxs-lookup"><span data-stu-id="e84bc-305">Context information to be passed to the callback function.</span></span> |

<span data-ttu-id="e84bc-306">ファイルの関数は戻り値がありません。</span><span class="sxs-lookup"><span data-stu-id="e84bc-306">The save function does not have a return value.</span></span> <span data-ttu-id="e84bc-307">呼び出しが正常に完了すると、いずれかが呼び出されます、`saveCompletedCallback`パラメーターまたは`defaultSaveCompletedCallback`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="e84bc-307">If the call completes successfully, it will call either the `saveCompletedCallback` parameter or `defaultSaveCompletedCallback` property.</span></span> <span data-ttu-id="e84bc-308">呼び出しが失敗した、またはいずれかのタイムアウトの期限が切れた場合、`failedCallback`または`defaultFailedCallback`プロパティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-308">If the call failed, or the timeout expired, either the `failedCallback` or `defaultFailedCallback` property will be called.</span></span>

<span data-ttu-id="e84bc-309">場合、`propertyNames`パラメーターが null で、すべてのプロファイル プロパティは、サーバーに送信されますおよびサーバーによって決定されるプロパティを保存できるし、することはできません。</span><span class="sxs-lookup"><span data-stu-id="e84bc-309">If the `propertyNames` parameter is null, all profile properties will be sent to the server, and the server will decide which properties can be saved and which cannot.</span></span>

<span data-ttu-id="e84bc-310">*defaultFailedCallback プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="e84bc-310">*defaultFailedCallback property (get, set):*</span></span>

<span data-ttu-id="e84bc-311">このプロパティは、web サービスとの通信に障害が発生した場合に呼び出す必要があります関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-311">This property specifies a function that should be called if a failure to communicate with the web service occurs.</span></span> <span data-ttu-id="e84bc-312">デリゲート (または関数の参照) を受信してする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-312">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="e84bc-313">このプロパティで指定された関数の参照は、次のシグネチャが必要です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-313">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

<span data-ttu-id="e84bc-314">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-314">*Parameters:*</span></span>

| <span data-ttu-id="e84bc-315">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="e84bc-315">**Parameter Name**</span></span> | <span data-ttu-id="e84bc-316">**意味**</span><span class="sxs-lookup"><span data-stu-id="e84bc-316">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="e84bc-317">Error</span><span class="sxs-lookup"><span data-stu-id="e84bc-317">Error</span></span> | <span data-ttu-id="e84bc-318">エラー情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-318">Specifies the error information.</span></span> |
| <span data-ttu-id="e84bc-319">userContext</span><span class="sxs-lookup"><span data-stu-id="e84bc-319">userContext</span></span> | <span data-ttu-id="e84bc-320">時に提供されたユーザー コンテキスト情報を指定します、読み込みまたは保存関数が呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="e84bc-320">Specifies the user context information provided when the load or save function was called.</span></span> |
| <span data-ttu-id="e84bc-321">methodName</span><span class="sxs-lookup"><span data-stu-id="e84bc-321">methodName</span></span> | <span data-ttu-id="e84bc-322">呼び出し元のメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="e84bc-322">The name of the calling method.</span></span> |

<span data-ttu-id="e84bc-323">*defaultSaveCompleted プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="e84bc-323">*defaultSaveCompleted property (get, set):*</span></span>

<span data-ttu-id="e84bc-324">このプロパティは、ユーザーのプロファイル データの保存の完了時に呼び出す必要があります関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-324">This property specifies a function that should be called upon the completion of saving the user's profile data.</span></span> <span data-ttu-id="e84bc-325">デリゲート (または関数の参照) を受信してする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-325">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="e84bc-326">このプロパティで指定された関数の参照は、次のシグネチャが必要です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-326">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

<span data-ttu-id="e84bc-327">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-327">*Parameters:*</span></span>

| <span data-ttu-id="e84bc-328">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="e84bc-328">**Parameter Name**</span></span> | <span data-ttu-id="e84bc-329">**意味**</span><span class="sxs-lookup"><span data-stu-id="e84bc-329">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="e84bc-330">numPropsSaved</span><span class="sxs-lookup"><span data-stu-id="e84bc-330">numPropsSaved</span></span> | <span data-ttu-id="e84bc-331">保存されたプロパティの数を指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-331">Specifies the number of properties that were saved.</span></span> |
| <span data-ttu-id="e84bc-332">userContext</span><span class="sxs-lookup"><span data-stu-id="e84bc-332">userContext</span></span> | <span data-ttu-id="e84bc-333">時に提供されたユーザー コンテキスト情報を指定します、読み込みまたは保存関数が呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="e84bc-333">Specifies the user context information provided when the load or save function was called.</span></span> |
| <span data-ttu-id="e84bc-334">methodName</span><span class="sxs-lookup"><span data-stu-id="e84bc-334">methodName</span></span> | <span data-ttu-id="e84bc-335">呼び出し元のメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="e84bc-335">The name of the calling method.</span></span> |

<span data-ttu-id="e84bc-336">*defaultLoadCompleted プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="e84bc-336">*defaultLoadCompleted property (get, set):*</span></span>

<span data-ttu-id="e84bc-337">このプロパティは、ユーザーのプロファイル データの読み込みの完了時に呼び出す必要があります関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-337">This property specifies a function that should be called upon the completion of loading of the user's profile data.</span></span> <span data-ttu-id="e84bc-338">デリゲート (または関数の参照) を受信してする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-338">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="e84bc-339">このプロパティで指定された関数の参照は、次のシグネチャが必要です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-339">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

<span data-ttu-id="e84bc-340">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="e84bc-340">*Parameters:*</span></span>

| <span data-ttu-id="e84bc-341">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="e84bc-341">**Parameter Name**</span></span> | <span data-ttu-id="e84bc-342">**意味**</span><span class="sxs-lookup"><span data-stu-id="e84bc-342">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="e84bc-343">numPropsLoaded</span><span class="sxs-lookup"><span data-stu-id="e84bc-343">numPropsLoaded</span></span> | <span data-ttu-id="e84bc-344">読み込まれるプロパティの数を指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-344">Specifies the number of properties loaded.</span></span> |
| <span data-ttu-id="e84bc-345">userContext</span><span class="sxs-lookup"><span data-stu-id="e84bc-345">userContext</span></span> | <span data-ttu-id="e84bc-346">時に提供されたユーザー コンテキスト情報を指定します、読み込みまたは保存関数が呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="e84bc-346">Specifies the user context information provided when the load or save function was called.</span></span> |
| <span data-ttu-id="e84bc-347">methodName</span><span class="sxs-lookup"><span data-stu-id="e84bc-347">methodName</span></span> | <span data-ttu-id="e84bc-348">呼び出し元のメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="e84bc-348">The name of the calling method.</span></span> |

<span data-ttu-id="e84bc-349">*path プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="e84bc-349">*path property (get, set):*</span></span>

<span data-ttu-id="e84bc-350">このプロパティは、プロファイルの web サービスの場所をプログラムによって決まります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-350">This property programmatically determines the location of the profile web service.</span></span> <span data-ttu-id="e84bc-351">既定のプロファイル サービス プロバイダーと 1 つをオーバーライドするために使用できる ScriptManager コントロールの ProfileService 子ノードの Path プロパティには宣言して設定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-351">It can be used to override the default profile service provider, as well as one set declaratively in the Path property of the ScriptManager control's ProfileService child node.</span></span>

<span data-ttu-id="e84bc-352">既定のプロファイル サービスの場所で変更されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e84bc-352">Note that the location of the default profile service does not change.</span></span> <span data-ttu-id="e84bc-353">ただし、ASP.NET AJAX には、ASP.NET AJAX 認証サービスのプロキシと同じクラス インターフェイスを提供する web サービスの場所を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-353">However, ASP.NET AJAX allows you to specify the location of a web service that provides the same class interface as the ASP.NET AJAX authentication service proxy.</span></span>

<span data-ttu-id="e84bc-354">このプロパティを現在のサイトからスクリプト要求に指示する値に設定されないことにも注意してください。</span><span class="sxs-lookup"><span data-stu-id="e84bc-354">Note also that this property should not be set to a value that directs the script request off of the current site.</span></span> <span data-ttu-id="e84bc-355">テクノロジの基になる AJAX では、サイト間で要求を投稿してはいけません、クライアント ブラウザーにセキュリティ例外を生成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-355">The technology underlying AJAX should not post cross-site requests, and may generate a security exception in the client browser.</span></span>

<span data-ttu-id="e84bc-356">このプロパティは、`String`プロファイル web サービスへのパスを表すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="e84bc-356">This property is a `String` object representing the path to the profile web service.</span></span>

<span data-ttu-id="e84bc-357">*timeout プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="e84bc-357">*timeout property (get, set):*</span></span>

<span data-ttu-id="e84bc-358">このプロパティは、プロファイル サービスの負荷と想定するまで待機するか、要求を保存する時間の長さが失敗したを決定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-358">This property determines the length of time to wait for the profile service before assuming the load or save request has failed.</span></span> <span data-ttu-id="e84bc-359">呼び出しが完了するを待機中にタイムアウトになると、要求が失敗しましたコールバックが呼び出されます、呼び出しは完了していません。</span><span class="sxs-lookup"><span data-stu-id="e84bc-359">If the timeout expires while waiting for a call to complete, the request-failed callback will be called, and the call will not complete.</span></span>

<span data-ttu-id="e84bc-360">このプロパティは、`Number`プロファイル サービスからの結果を待機するミリ秒数を表すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="e84bc-360">This property is a `Number` object representing the number of milliseconds to wait for results from the profile service.</span></span>

<span data-ttu-id="e84bc-361">*コード サンプル: ページの読み込み時のプロファイル データを読み込んでいます*</span><span class="sxs-lookup"><span data-stu-id="e84bc-361">*Code sample: Loading profile data at page load*</span></span>

<span data-ttu-id="e84bc-362">次のコードでは、ユーザーが認証されたかどうかを確認でき、ページのとして、ユーザーの推奨される背景色を読み込む場合は、です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-362">The following code will check to see whether a user is authenticated, and if so, will load the user's preferred background color as the page's.</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a><span data-ttu-id="e84bc-363">*カスタム認証サービス プロバイダーを使用します。*</span><span class="sxs-lookup"><span data-stu-id="e84bc-363">*Using a Custom Authentication Service Provider*</span></span>

<span data-ttu-id="e84bc-364">ASP.NET AJAX 拡張機能を使用すると、カスタム web サービスを通じて、機能を公開するカスタム スクリプト認証サービス プロバイダーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-364">The ASP.NET AJAX Extensions allow you to create a custom script authentication service provider by exposing your functionality through a custom web service.</span></span> <span data-ttu-id="e84bc-365">使用するために、web サービスが 2 つのメソッドを公開する必要があります`Login`と`Logout`; 既定の ASP.NET AJAX の認証 web サービスとして、同じメソッド シグネチャを持つこれらのメソッドを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-365">In order to be used, your web service must expose two methods, `Login` and `Logout`; and these methods must be specified with the same method signatures as the default ASP.NET AJAX Authentication web service.</span></span>

<span data-ttu-id="e84bc-366">カスタム web サービスを作成した後は、コード、またはクライアント スクリプトを使用してプログラムでその宣言によって、ページのいずれかへのパスを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e84bc-366">Once you have created the custom web service, you will need to specify the path to it, either declaratively on your page, programmatically in code, or via the client script.</span></span>

<span data-ttu-id="e84bc-367">*パスを宣言して設定するには。*</span><span class="sxs-lookup"><span data-stu-id="e84bc-367">*To set the path declaratively:*</span></span>

<span data-ttu-id="e84bc-368">パスを設定するには、宣言によって、ASP.NET ページに ScriptManager オブジェクトの認証サービスの子のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e84bc-368">To set the path declaratively, include the AuthenticationService child of the ScriptManager object on your ASP.NET page:</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

<span data-ttu-id="e84bc-369">*コードでパスを設定します。*</span><span class="sxs-lookup"><span data-stu-id="e84bc-369">*To set the path in code:*</span></span>

<span data-ttu-id="e84bc-370">、パスをプログラムで設定するには、スクリプト マネージャーのインスタンスを使用してパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-370">To set the path programmatically, specify the path via the instance of your script manager:</span></span>

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

<span data-ttu-id="e84bc-371">*スクリプトでパスを設定します。*</span><span class="sxs-lookup"><span data-stu-id="e84bc-371">*To set the path in script:*</span></span>

<span data-ttu-id="e84bc-372">、スクリプト内のプログラムでパスを設定する場合、使用、`path`認証サービス クラスのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="e84bc-372">To set the path programmatically in script, utilize the `path` property of the AuthenticationService class:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

<span data-ttu-id="e84bc-373">*カスタム認証用のサンプル Web サービス*</span><span class="sxs-lookup"><span data-stu-id="e84bc-373">*Sample Web Service for Custom Authentication*</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a><span data-ttu-id="e84bc-374">まとめ</span><span class="sxs-lookup"><span data-stu-id="e84bc-374">Summary</span></span>

<span data-ttu-id="e84bc-375">ASP.NET サービス - 具体的には、プロファイリング、メンバーシップ、および認証サービスには、クライアントのブラウザーで JavaScript を簡単に公開されます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-375">ASP.NET services - specifically the profiling, membership, and authentication services - are easily exposed to JavaScript on the client browser.</span></span> <span data-ttu-id="e84bc-376">これにより、面倒な作業を行う Updatepanel などのコントロールに依存することがなく、シームレスに認証メカニズムと、クライアント側のコードを統合する開発者です。</span><span class="sxs-lookup"><span data-stu-id="e84bc-376">This allows developers to integrate their client-side code with the authentication mechanism seamlessly, without depending on controls such as UpdatePanels to do the heavy lifting.</span></span> <span data-ttu-id="e84bc-377">Web の構成設定を利用することにより、同様に、クライアントからプロファイル データを保護することができます。既定では、データがないと開発者必要がありますにオプトイン プロファイル プロパティです。</span><span class="sxs-lookup"><span data-stu-id="e84bc-377">Profile data can be protected from the client as well, by utilizing web configuration settings; no data is available by default, and developers must opt-in to profile properties.</span></span>

<span data-ttu-id="e84bc-378">さらに、簡略化された web サービスの実装を同等のメソッド署名を作成する開発者は、これらの組み込みの ASP.NET サービス プロバイダーがカスタム スクリプトを作成できます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-378">Furthermore, by creating simplified web service implementations with equivalent method signatures, developers can create custom script providers for these intrinsic ASP.NET services.</span></span> <span data-ttu-id="e84bc-379">これらの手法のサポートは、さまざまな特定のニーズを満たすための柔軟性を開発者に提供しながら、リッチ クライアント アプリケーションの開発を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="e84bc-379">Support for these techniques simplifies the development of rich client applications, while providing developers with a wide range of flexibility to meet specific needs.</span></span>

## <a name="bio"></a><span data-ttu-id="e84bc-380">*Bio*</span><span class="sxs-lookup"><span data-stu-id="e84bc-380">*Bio*</span></span>

<span data-ttu-id="e84bc-381">Scott カテゴリは、1997 年以降の Microsoft の Web テクノロジの使用されているがあり、myKB.com の代表者 ([www.myKB.com](http://www.myKB.com))、専門分野は、ASP.NET の書き込みの際にベースのアプリケーションのナレッジ ベースのソフトウェア ソリューションに重点を置きます。</span><span class="sxs-lookup"><span data-stu-id="e84bc-381">Scott Cate has been working with Microsoft Web technologies since 1997 and is the President of myKB.com ([www.myKB.com](http://www.myKB.com)) where he specializes in writing ASP.NET based applications focused on Knowledge Base Software solutions.</span></span> <span data-ttu-id="e84bc-382">Scott が接続時に電子メール[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)または彼のブログで[ScottCate.com](http://ScottCate.com)</span><span class="sxs-lookup"><span data-stu-id="e84bc-382">Scott can be contacted via email at [scott.cate@myKB.com](mailto:scott.cate@myKB.com) or his blog at [ScottCate.com](http://ScottCate.com)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e84bc-383">[前へ](understanding-asp-net-ajax-updatepanel-triggers.md)
> [次へ](understanding-asp-net-ajax-localization.md)</span><span class="sxs-lookup"><span data-stu-id="e84bc-383">[Previous](understanding-asp-net-ajax-updatepanel-triggers.md)
[Next](understanding-asp-net-ajax-localization.md)</span></span>
