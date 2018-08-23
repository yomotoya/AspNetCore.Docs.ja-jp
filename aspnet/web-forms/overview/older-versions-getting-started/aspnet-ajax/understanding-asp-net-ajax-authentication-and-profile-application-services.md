---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: ASP.NET AJAX 認証とプロファイル アプリケーション サービスを理解する |Microsoft Docs
author: scottcate
description: 認証サービスは、認証クッキーを受信するために資格情報を提供することができ、ゲートウェイ サービスのカスタム ユーザーを許可するのには、.
ms.author: riande
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: d722130e625a9f867923280fce0ef35f19bfeb9d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837982"
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a><span data-ttu-id="5bb8d-103">ASP.NET AJAX 認証とプロファイル アプリケーション サービスを理解します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-103">Understanding ASP.NET AJAX Authentication and Profile Application Services</span></span>
====================
<span data-ttu-id="5bb8d-104">によって[Scott Cate](https://github.com/scottcate)</span><span class="sxs-lookup"><span data-stu-id="5bb8d-104">by [Scott Cate](https://github.com/scottcate)</span></span>

[<span data-ttu-id="5bb8d-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="5bb8d-105">Download PDF</span></span>](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> <span data-ttu-id="5bb8d-106">認証サービスにより、ユーザー、認証クッキーを受信するために資格情報を指定して、ゲートウェイ サービスのカスタム ユーザー プロファイルを許可するのには ASP.NET によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-106">The Authentication service allows users to provide credentials in order to receive an authentication cookie, and is the gateway service to allow custom user profiles provided by ASP.NET.</span></span> <span data-ttu-id="5bb8d-107">ASP.NET AJAX の認証サービスの使用は、フォーム認証を使用中のアプリケーション (ログインの制御など) しない機能しなくなります AJAX 認証サービスにアップグレードすることで、標準の ASP.NET フォーム認証と互換性のあります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-107">Use of the ASP.NET AJAX authentication service is compatible with standard ASP.NET Forms authentication, so applications currently using Forms authentication (such as with the Login control) will not be broken by upgrading to the AJAX authentication service.</span></span>


## <a name="introduction"></a><span data-ttu-id="5bb8d-108">はじめに</span><span class="sxs-lookup"><span data-stu-id="5bb8d-108">Introduction</span></span>

<span data-ttu-id="5bb8d-109">.NET Framework 3.5 の一部として、Microsoft は、かなり大きな環境のアップグレードでは; を配信します。新しい開発環境があるだけでなく統合言語クエリ (LINQ) の新機能とその他の言語拡張機能は近日公開予定。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-109">As part of the .NET Framework 3.5, Microsoft is delivering a sizable environment upgrade; not only is a new development environment available, but the new Language-Integrated Query (LINQ) features and other language enhancements are forthcoming.</span></span> <span data-ttu-id="5bb8d-110">さらに、.NET Framework 基本クラス ライブラリのファースト クラスのメンバーとして他のツールセットでは、特に、ASP.NET AJAX Extensions の使い慣れた機能の一部が含まれているがされています。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-110">In addition, some familiar features of other toolsets, notably the ASP.NET AJAX Extensions, are being included as first-class members of the .NET Framework Base Class Library.</span></span> <span data-ttu-id="5bb8d-111">これらの拡張機能は、ページ全体の更新では、(ASP.NET のプロファイル API を含む)、クライアント スクリプトと広範なクライアント側 API を使用して Web サービスにアクセスする機能を必要とせずにページの部分的なレンダリングを含む、多くの新機能豊富なクライアント機能を有効にします。ASP.NET サーバー側コントロール セットに表示されるコントロール パターンの多くをミラーに設計されています。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-111">These extensions enable many new rich client features, including partial rendering of pages without requiring a full page refresh, the ability to access Web Services via client script (including the ASP.NET profiling API), and an extensive client-side API designed to mirror many of the control schemes seen in the ASP.NET server-side control set.</span></span>

<span data-ttu-id="5bb8d-112">このホワイト ペーパーでは、実装と ASP.NET のプロファイルの使用してフォーム認証サービスは、Microsoft ASP.NET AJAX ExtensionsThe AJAX Extensions で公開されて、フォーム認証としてそれをサポートするは驚くほど簡単 (だけでなくプロファイリング サービス) は、Web サービス プロキシ スクリプトを通じて公開されます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-112">This whitepaper looks at the implementation and use of the ASP.NET Profiling and Forms Authentication services as they are exposed by the Microsoft ASP.NET AJAX ExtensionsThe AJAX Extensions make Forms authentication incredibly easy to support, as it (as well as the Profiling Service) is exposed through a Web Service proxy script.</span></span> <span data-ttu-id="5bb8d-113">AJAX Extensions には、AuthenticationServiceManager クラスを使用したカスタム認証もサポートします。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-113">The AJAX Extensions also support custom authentication through the AuthenticationServiceManager class.</span></span>

<span data-ttu-id="5bb8d-114">このホワイト ペーパーでは、Visual Studio 2008 の Beta 2 リリースと、.NET Framework 3.5 に基づいています。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-114">This whitepaper is based on the Beta 2 release of the Visual Studio 2008 and the .NET Framework 3.5.</span></span> <span data-ttu-id="5bb8d-115">このホワイト ペーパーには、いない Visual Web Developer Express を Visual Studio 2008 Beta 2 で使用され、チュートリアルに従って、Visual Studio のユーザー インターフェイスを提供することも想定しています。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-115">This whitepaper also assumes that you will be working with Visual Studio 2008 Beta 2, not Visual Web Developer Express, and will provide walkthroughs according to the user interface of Visual Studio.</span></span> <span data-ttu-id="5bb8d-116">一部のコード サンプルでは、Visual Web Developer Express で使用できないプロジェクト テンプレートを利用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-116">Some code samples may utilize project templates unavailable in Visual Web Developer Express.</span></span>

## <a name="profiles-and-authentication"></a><span data-ttu-id="5bb8d-117">*プロファイルと認証*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-117">*Profiles and Authentication*</span></span>

<span data-ttu-id="5bb8d-118">Microsoft ASP.NET のプロファイルと認証サービスは、ASP.NET フォーム認証システムによって提供され、ASP.NET の標準的なコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-118">The Microsoft ASP.NET Profiles and Authentication services are provided by the ASP.NET Forms Authentication system, and are standard components of ASP.NET.</span></span> <span data-ttu-id="5bb8d-119">ASP.NET AJAX Extensions では、クライアント AJAX ライブラリの名前空間の Sys.Services 非常に簡単ですモデルでのスクリプト プロキシを介してこれらのサービスへのスクリプト アクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-119">The ASP.NET AJAX Extensions provide script access to these services via script proxies, through a fairly straightforward model under the Sys.Services namespace of the client AJAX library.</span></span>

<span data-ttu-id="5bb8d-120">認証サービスにより、ユーザー、認証クッキーを受信するために資格情報を指定して、ゲートウェイ サービスのカスタム ユーザー プロファイルを許可するのには ASP.NET によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-120">The Authentication service allows users to provide credentials in order to receive an authentication cookie, and is the gateway service to allow custom user profiles provided by ASP.NET.</span></span> <span data-ttu-id="5bb8d-121">ASP.NET AJAX の認証サービスの使用は、フォーム認証を使用中のアプリケーション (ログインの制御など) しない機能しなくなります AJAX 認証サービスにアップグレードすることで、標準の ASP.NET フォーム認証と互換性のあります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-121">Use of the ASP.NET AJAX authentication service is compatible with standard ASP.NET Forms authentication, so applications currently using Forms authentication (such as with the Login control) will not be broken by upgrading to the AJAX authentication service.</span></span>

<span data-ttu-id="5bb8d-122">プロファイル サービスは、自動統合と、認証サービスによって提供されるメンバーシップに基づいてユーザー データのストレージを使用します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-122">The Profile service allows the automatic integration and storage of user data based on membership as provided by the Authentication service.</span></span> <span data-ttu-id="5bb8d-123">格納されたデータが、web.config ファイルで指定された、さまざまなプロファイリング サービス プロバイダーは、データ管理を処理します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-123">The stored data is specified by the web.config file, and the various profiling service providers handle the data management.</span></span> <span data-ttu-id="5bb8d-124">認証サービスと同様、AJAX プロファイル サービスは、できるように、現在 ASP.NET プロファイル サービスの機能を組み込むページは、AJAX のサポートを含めることによって壊れていない必要があります、標準の ASP.NET プロファイル サービスとの互換性。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-124">As with the Authentication service, the AJAX Profile service is compatible with the standard ASP.NET profile service, so that pages currently incorporating features of the ASP.NET Profile service should not be broken by including AJAX support.</span></span>

<span data-ttu-id="5bb8d-125">ASP.NET の認証とプロファイル サービス自体を組み込むアプリケーションがこのホワイト ペーパーの範囲外です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-125">Incorporating the ASP.NET Authentication and Profiling services themselves into an application is outside of the scope of this whitepaper.</span></span> <span data-ttu-id="5bb8d-126">トピックの詳細については、MSDN ライブラリを参照してください参照資料でのメンバーシップを使用したユーザーを管理する[ https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-126">For more information on the topic, see the MSDN Library reference article Managing Users by Using Membership at [https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx).</span></span> <span data-ttu-id="5bb8d-127">ASP.NET には、ASP.NET メンバーシップの既定の認証サービス プロバイダーは、SQL Server でのメンバーシップを自動的に設定するためのユーティリティも含まれています。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-127">ASP.NET also includes a utility to automatically set up Membership with a SQL Server, which is the default authentication service provider for ASP.NET Membership.</span></span> <span data-ttu-id="5bb8d-128">詳細については、ASP.NET SQL Server の登録ツールの記事を参照してください (Aspnet\_regsql.exe) で[ https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-128">For more information, see the article ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe) at [https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).</span></span>

## <a name="using-the-aspnet-ajax-authentication-service"></a><span data-ttu-id="5bb8d-129">*ASP.NET AJAX の認証サービスの使用*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-129">*Using the ASP.NET AJAX Authentication Service*</span></span>

<span data-ttu-id="5bb8d-130">Web.config ファイルでは、ASP.NET AJAX 認証サービスを有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-130">The ASP.NET AJAX Authentication service must be enabled in the web.config file:</span></span>

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

<span data-ttu-id="5bb8d-131">認証サービスでは、ASP.NET フォーム認証を有効にする必要があり、cookie (スクリプトを有効にできませんクッキーなしのセッション cookie なしのセッションが URL パラメーターが必要なため)、クライアント ブラウザーで有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-131">The Authentication service requires ASP.NET Forms authentication to be enabled and requires cookies to be enabled on the client browser (a script cannot enable a cookieless session since cookieless sessions require URL parameters).</span></span>

<span data-ttu-id="5bb8d-132">AJAX 認証サービスが有効にし、構成、クライアント スクリプトすぐに利用できます Sys.Services.AuthenticationService オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-132">Once the AJAX authentication service is enabled and configured, client script can immediately take advantage of the Sys.Services.AuthenticationService object.</span></span> <span data-ttu-id="5bb8d-133">クライアント スクリプトを活用するためにしますが、主として、`login`メソッドと`isLoggedIn`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-133">Primarily, client script will want to take advantage of the `login` method and `isLoggedIn` property.</span></span> <span data-ttu-id="5bb8d-134">多くのパラメーターを受け入れることができます、login メソッドの既定値を提供するいくつかのプロパティが存在します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-134">Several properties exist to provide defaults for the login method, which can accept a large number of parameters.</span></span>

<span data-ttu-id="5bb8d-135">*Sys.Services.AuthenticationService メンバー*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-135">*Sys.Services.AuthenticationService members*</span></span>

<span data-ttu-id="5bb8d-136">*ログイン方法:*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-136">*login method:*</span></span>

<span data-ttu-id="5bb8d-137">Login() メソッドは、ユーザーの資格情報を認証するための要求を開始します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-137">The login() method begins a request to authenticate the user's credentials.</span></span> <span data-ttu-id="5bb8d-138">このメソッドは、非同期実行をブロックしません。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-138">This method is asynchronous and does not block execution.</span></span>

<span data-ttu-id="5bb8d-139">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-139">*Parameters:*</span></span>

| <span data-ttu-id="5bb8d-140">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-140">**Parameter Name**</span></span> | <span data-ttu-id="5bb8d-141">**意味**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-141">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="5bb8d-142">userName</span><span class="sxs-lookup"><span data-stu-id="5bb8d-142">userName</span></span> | <span data-ttu-id="5bb8d-143">必須。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-143">Required.</span></span> <span data-ttu-id="5bb8d-144">認証するユーザー名。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-144">The username to authenticate.</span></span> |
| <span data-ttu-id="5bb8d-145">パスワード</span><span class="sxs-lookup"><span data-stu-id="5bb8d-145">password</span></span> | <span data-ttu-id="5bb8d-146">省略可能 (既定値は null) です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-146">Optional (defaults to null).</span></span> <span data-ttu-id="5bb8d-147">ユーザーのパスワード。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-147">The user's password.</span></span> |
| <span data-ttu-id="5bb8d-148">isPersistent</span><span class="sxs-lookup"><span data-stu-id="5bb8d-148">isPersistent</span></span> | <span data-ttu-id="5bb8d-149">省略可能 (既定値は false)。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-149">Optional (defaults to false).</span></span> <span data-ttu-id="5bb8d-150">かどうか、ユーザーの認証クッキーがセッション間で保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-150">Whether the user's authentication cookie should persist across sessions.</span></span> <span data-ttu-id="5bb8d-151">False の場合、ブラウザーを閉じるし、セッションの有効期限が切れるユーザーがログオンします。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-151">If false, the user will log out when the browser is closed or the session expires.</span></span> |
| <span data-ttu-id="5bb8d-152">redirectUrl</span><span class="sxs-lookup"><span data-stu-id="5bb8d-152">redirectUrl</span></span> | <span data-ttu-id="5bb8d-153">省略可能 (既定値は null) です。認証に成功するブラウザーをリダイレクトする URL。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-153">Optional (defaults to null).The URL to redirect the browser to upon successful authentication.</span></span> <span data-ttu-id="5bb8d-154">このパラメーターが null または空の文字列の場合は、リダイレクトは行われません。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-154">If this parameter is null or an empty string, no redirection occurs.</span></span> |
| <span data-ttu-id="5bb8d-155">customInfo</span><span class="sxs-lookup"><span data-stu-id="5bb8d-155">customInfo</span></span> | <span data-ttu-id="5bb8d-156">省略可能 (既定値は null) です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-156">Optional (defaults to null).</span></span> <span data-ttu-id="5bb8d-157">このパラメーターは、現在使用されていないと、将来使用するために予約されています。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-157">This parameter is currently unused and is reserved for future use.</span></span> |
| <span data-ttu-id="5bb8d-158">loginCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="5bb8d-158">loginCompletedCallback</span></span> | <span data-ttu-id="5bb8d-159">省略可能 (既定値は null) です。ログインが正常に完了したときに呼び出す関数。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-159">Optional (defaults to null).The function to call when the login has successfully completed.</span></span> <span data-ttu-id="5bb8d-160">指定した場合、このパラメーターは、defaultLoginCompleted プロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-160">If specified, this parameter overrides the defaultLoginCompleted property.</span></span> |
| <span data-ttu-id="5bb8d-161">failedCallback</span><span class="sxs-lookup"><span data-stu-id="5bb8d-161">failedCallback</span></span> | <span data-ttu-id="5bb8d-162">省略可能 (既定値は null) です。ログインに失敗したときに呼び出す関数。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-162">Optional (defaults to null).The function to call when the login has failed.</span></span> <span data-ttu-id="5bb8d-163">指定した場合、このパラメーターは、defaultFailedCallback プロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-163">If specified, this parameter overrides the defaultFailedCallback property.</span></span> |
| <span data-ttu-id="5bb8d-164">userContext</span><span class="sxs-lookup"><span data-stu-id="5bb8d-164">userContext</span></span> | <span data-ttu-id="5bb8d-165">省略可能 (既定値は null) です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-165">Optional (defaults to null).</span></span> <span data-ttu-id="5bb8d-166">カスタム ユーザー コンテキスト データをコールバック関数に渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-166">Custom user context data that should be passed to the callback functions.</span></span> |

<span data-ttu-id="5bb8d-167">*値が返されます。*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-167">*Return Value:*</span></span>

<span data-ttu-id="5bb8d-168">この関数では、戻り値は含まれません。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-168">This function does not include a return value.</span></span> <span data-ttu-id="5bb8d-169">ただし、複数の動作は、この関数の呼び出しの完了時にインクルードされます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-169">However, a number of behaviors are included upon completion of a call to this function:</span></span>

- <span data-ttu-id="5bb8d-170">現在のページが更新されますか、または場合に変更する、`redirectUrl`パラメーターが null でも空の文字列。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-170">The current page will either be refreshed or be changed if the `redirectUrl` parameter was neither null nor an empty string.</span></span>
- <span data-ttu-id="5bb8d-171">ただし、パラメーターが null または空の文字列の場合、`loginCompletedCallback`パラメーター、または`defaultLoginCompletedCallback`プロパティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-171">However, if the parameter was null or an empty string, the `loginCompletedCallback` parameter, or `defaultLoginCompletedCallback` property is called.</span></span>
- <span data-ttu-id="5bb8d-172">Web サービスへの呼び出しに失敗した場合、`failedCallback`パラメーターの`defaultFailedCallback`プロパティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-172">If the call to the web service fails, the `failedCallback` parameter of `defaultFailedCallback` property is called.</span></span>

<span data-ttu-id="5bb8d-173">*logout メソッド:*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-173">*logout method:*</span></span>

<span data-ttu-id="5bb8d-174">Logout() メソッドは、資格情報の cookie を削除し、web アプリケーションから現在のユーザーをログアウトします。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-174">The logout() method removes the credentials cookie and logs out the current user from the web application.</span></span>

<span data-ttu-id="5bb8d-175">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-175">*Parameters:*</span></span>

| <span data-ttu-id="5bb8d-176">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-176">**Parameter Name**</span></span> | <span data-ttu-id="5bb8d-177">**意味**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-177">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="5bb8d-178">redirectUrl</span><span class="sxs-lookup"><span data-stu-id="5bb8d-178">redirectUrl</span></span> | <span data-ttu-id="5bb8d-179">省略可能 (既定値は null) です。認証に成功するブラウザーをリダイレクトする URL。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-179">Optional (defaults to null).The URL to redirect the browser to upon successful authentication.</span></span> <span data-ttu-id="5bb8d-180">このパラメーターが null または空の文字列の場合は、リダイレクトは行われません。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-180">If this parameter is null or an empty string, no redirection occurs.</span></span> |
| <span data-ttu-id="5bb8d-181">logoutCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="5bb8d-181">logoutCompletedCallback</span></span> | <span data-ttu-id="5bb8d-182">省略可能 (既定値は null) です。ログアウトが正常に完了したときに呼び出す関数。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-182">Optional (defaults to null).The function to call when the logout has successfully completed.</span></span> <span data-ttu-id="5bb8d-183">指定した場合、このパラメーターは、defaultLogoutCompleted プロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-183">If specified, this parameter overrides the defaultLogoutCompleted property.</span></span> |
| <span data-ttu-id="5bb8d-184">failedCallback</span><span class="sxs-lookup"><span data-stu-id="5bb8d-184">failedCallback</span></span> | <span data-ttu-id="5bb8d-185">省略可能 (既定値は null) です。ログインに失敗したときに呼び出す関数。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-185">Optional (defaults to null).The function to call when the login has failed.</span></span> <span data-ttu-id="5bb8d-186">指定した場合、このパラメーターは、defaultFailedCallback プロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-186">If specified, this parameter overrides the defaultFailedCallback property.</span></span> |
| <span data-ttu-id="5bb8d-187">userContext</span><span class="sxs-lookup"><span data-stu-id="5bb8d-187">userContext</span></span> | <span data-ttu-id="5bb8d-188">省略可能 (既定値は null) です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-188">Optional (defaults to null).</span></span> <span data-ttu-id="5bb8d-189">カスタム ユーザー コンテキスト データをコールバック関数に渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-189">Custom user context data that should be passed to the callback functions.</span></span> |

<span data-ttu-id="5bb8d-190">*値が返されます。*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-190">*Return Value:*</span></span>

<span data-ttu-id="5bb8d-191">この関数では、戻り値は含まれません。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-191">This function does not include a return value.</span></span> <span data-ttu-id="5bb8d-192">ただし、複数の動作は、この関数の呼び出しの完了時にインクルードされます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-192">However, a number of behaviors are included upon completion of a call to this function:</span></span>

- <span data-ttu-id="5bb8d-193">現在のページが更新されますか、または場合に変更する、`redirectUrl`パラメーターが null でも空の文字列。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-193">The current page will either be refreshed or be changed if the `redirectUrl` parameter was neither null nor an empty string.</span></span>
- <span data-ttu-id="5bb8d-194">ただし、パラメーターが null または空の文字列の場合、`logoutCompletedCallback`パラメーター、または`defaultLogoutCompletedCallback`プロパティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-194">However, if the parameter was null or an empty string, the `logoutCompletedCallback` parameter, or `defaultLogoutCompletedCallback` property is called.</span></span>
- <span data-ttu-id="5bb8d-195">Web サービスへの呼び出しに失敗した場合、`failedCallback`パラメーターの`defaultFailedCallback`プロパティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-195">If the call to the web service fails, the `failedCallback` parameter of `defaultFailedCallback` property is called.</span></span>

<span data-ttu-id="5bb8d-196">*defaultFailedCallback プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-196">*defaultFailedCallback property (get, set):*</span></span>

<span data-ttu-id="5bb8d-197">このプロパティは、web サービスとの通信に障害が発生した場合に呼び出す必要がある関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-197">This property specifies a function that should be called if a failure to communicate with the web service occurs.</span></span> <span data-ttu-id="5bb8d-198">デリゲート (または関数の参照)、受信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-198">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="5bb8d-199">このプロパティで指定された関数の参照は、次のシグネチャが必要です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-199">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

<span data-ttu-id="5bb8d-200">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-200">*Parameters:*</span></span>

| <span data-ttu-id="5bb8d-201">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-201">**Parameter Name**</span></span> | <span data-ttu-id="5bb8d-202">**意味**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-202">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="5bb8d-203">エラー</span><span class="sxs-lookup"><span data-stu-id="5bb8d-203">error</span></span> | <span data-ttu-id="5bb8d-204">エラー情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-204">Specifies the error information.</span></span> |
| <span data-ttu-id="5bb8d-205">userContext</span><span class="sxs-lookup"><span data-stu-id="5bb8d-205">userContext</span></span> | <span data-ttu-id="5bb8d-206">ログインまたはログアウト関数が呼び出されたときにユーザー コンテキスト情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-206">Specifies the user context information provided when the login or logout function was called.</span></span> |
| <span data-ttu-id="5bb8d-207">methodName</span><span class="sxs-lookup"><span data-stu-id="5bb8d-207">methodName</span></span> | <span data-ttu-id="5bb8d-208">呼び出し元のメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-208">The name of the calling method.</span></span> |

<span data-ttu-id="5bb8d-209">*defaultLoginCompletedCallback プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-209">*defaultLoginCompletedCallback property (get, set):*</span></span>

<span data-ttu-id="5bb8d-210">このプロパティは、ログイン web サービスの呼び出しが完了したときに呼び出される関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-210">This property specifies a function that should be called when the login web service call has completed.</span></span> <span data-ttu-id="5bb8d-211">デリゲート (または関数の参照)、受信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-211">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="5bb8d-212">このプロパティで指定された関数の参照は、次のシグネチャが必要です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-212">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

<span data-ttu-id="5bb8d-213">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-213">*Parameters:*</span></span>

| <span data-ttu-id="5bb8d-214">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-214">**Parameter Name**</span></span> | <span data-ttu-id="5bb8d-215">**意味**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-215">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="5bb8d-216">validCredentials</span><span class="sxs-lookup"><span data-stu-id="5bb8d-216">validCredentials</span></span> | <span data-ttu-id="5bb8d-217">ユーザーが有効な資格情報を指定するかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-217">Specifies whether the user provided valid credentials.</span></span> <span data-ttu-id="5bb8d-218">`true` で、ユーザーが正常にログインしている場合それ以外の場合`false`します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-218">`true` if the user successfully logged in; otherwise `false`.</span></span> |
| <span data-ttu-id="5bb8d-219">userContext</span><span class="sxs-lookup"><span data-stu-id="5bb8d-219">userContext</span></span> | <span data-ttu-id="5bb8d-220">Login 関数が呼び出されたときにユーザー コンテキスト情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-220">Specifies the user context information provided when the login function was called.</span></span> |
| <span data-ttu-id="5bb8d-221">methodName</span><span class="sxs-lookup"><span data-stu-id="5bb8d-221">methodName</span></span> | <span data-ttu-id="5bb8d-222">呼び出し元のメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-222">The name of the calling method.</span></span> |

<span data-ttu-id="5bb8d-223">*defaultLogoutCompletedCallback プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-223">*defaultLogoutCompletedCallback property (get, set):*</span></span>

<span data-ttu-id="5bb8d-224">このプロパティは、ログアウトの web サービス呼び出しが完了したときに呼び出される関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-224">This property specifies a function that should be called when the logout web service call has completed.</span></span> <span data-ttu-id="5bb8d-225">デリゲート (または関数の参照)、受信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-225">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="5bb8d-226">このプロパティで指定された関数の参照は、次のシグネチャが必要です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-226">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

<span data-ttu-id="5bb8d-227">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-227">*Parameters:*</span></span>

| <span data-ttu-id="5bb8d-228">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-228">**Parameter Name**</span></span> | <span data-ttu-id="5bb8d-229">**意味**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-229">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="5bb8d-230">結果</span><span class="sxs-lookup"><span data-stu-id="5bb8d-230">result</span></span> | <span data-ttu-id="5bb8d-231">このパラメーターは必ず`null`; 将来使用するために予約されています。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-231">This parameter will always be `null`; it is reserved for future use.</span></span> |
| <span data-ttu-id="5bb8d-232">userContext</span><span class="sxs-lookup"><span data-stu-id="5bb8d-232">userContext</span></span> | <span data-ttu-id="5bb8d-233">Login 関数が呼び出されたときにユーザー コンテキスト情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-233">Specifies the user context information provided when the login function was called.</span></span> |
| <span data-ttu-id="5bb8d-234">methodName</span><span class="sxs-lookup"><span data-stu-id="5bb8d-234">methodName</span></span> | <span data-ttu-id="5bb8d-235">呼び出し元のメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-235">The name of the calling method.</span></span> |

<span data-ttu-id="5bb8d-236">*isloggedin 関数のプロパティ (get):*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-236">*isLoggedIn property (get):*</span></span>

<span data-ttu-id="5bb8d-237">このプロパティは、ユーザーの現在の認証状態を取得します。ページ要求中に、ScriptManager オブジェクトによって設定されます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-237">This property gets the current authentication state of the user; it is set by the ScriptManager object during a page request.</span></span>

<span data-ttu-id="5bb8d-238">このプロパティを返します`true`場合は、ユーザーは、それ以外の場合以外に現在ログインしているを返します`false`します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-238">This property returns `true` if the user is currently logged in; otherwise, it returns `false`.</span></span>

<span data-ttu-id="5bb8d-239">*パスのプロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-239">*path property (get, set):*</span></span>

<span data-ttu-id="5bb8d-240">このプロパティは、認証 web サービスの場所をプログラムで決定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-240">This property programmatically determines the location of the authentication web service.</span></span> <span data-ttu-id="5bb8d-241">いずれかの既定の認証プロバイダーをオーバーライドするために使用できる、ScriptManager コントロールの AuthenticationService 子ノードのパスのプロパティの宣言を設定 (詳細については、使用方法を参照してくださいカスタム認証サービス プロバイダー。トピックの下)。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-241">It can be used to override the default authentication provider, as well as one set declaratively in the Path property of the ScriptManager control's AuthenticationService child node (for more information, see the Using a Custom Authentication Service Provider topic below).</span></span>

<span data-ttu-id="5bb8d-242">既定の認証サービスの場所が変更されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-242">Note that the location of the default authentication service does not change.</span></span> <span data-ttu-id="5bb8d-243">ただし、ASP.NET AJAX を使用すると、ASP.NET AJAX 認証サービスのプロキシと同じクラス インターフェイスを提供する web サービスの場所を指定できます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-243">However, ASP.NET AJAX allows you to specify the location of a web service that provides the same class interface as the ASP.NET AJAX authentication service proxy.</span></span>

<span data-ttu-id="5bb8d-244">また、現在のサイトから、スクリプト要求を転送する値にこのプロパティを設定しないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-244">Note also that this property should not be set to a value that directs the script request off of the current site.</span></span> <span data-ttu-id="5bb8d-245">現在のアプリケーションでは、認証資格情報が表示されないが、ため役に立ちません。 なりますまた、テクノロジの基になる AJAX は、サイト間の要求を投稿する必要があります、クライアントのブラウザーにセキュリティ例外を生成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-245">Because the current application would not receive the authentication credentials, it would be useless; also, the technology underlying AJAX should not post cross-site requests, and may generate a security exception in the client browser.</span></span>

<span data-ttu-id="5bb8d-246">このプロパティは、`String`認証 web サービスへのパスを表すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-246">This property is a `String` object representing the path to the authentication web service.</span></span>

<span data-ttu-id="5bb8d-247">*timeout プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-247">*timeout property (get, set):*</span></span>

<span data-ttu-id="5bb8d-248">このプロパティは、失敗したと仮定すると、ログイン要求の前に、認証サービスを待機する時間の長さを決定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-248">This property determines the length of time to wait for the authentication service before assuming the login request has failed.</span></span> <span data-ttu-id="5bb8d-249">呼び出しが完了するを待機中にタイムアウトになると、要求が失敗しましたコールバックが呼び出されます、呼び出しは完了しません。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-249">If the timeout expires while waiting for a call to complete, the request-failed callback will be called, and the call will not complete.</span></span>

<span data-ttu-id="5bb8d-250">このプロパティは、`Number`認証サービスから結果を待機するミリ秒数を表すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-250">This property is a `Number` object representing the number of milliseconds to wait for results from the authentication service.</span></span>

<span data-ttu-id="5bb8d-251">*認証サービスにログインするコード サンプル。*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-251">*Code Sample: Logging into the Authentication Service*</span></span>

<span data-ttu-id="5bb8d-252">次のマークアップでは、AuthenticationService クラスのメソッドのログインとログアウトの単純なスクリプトの呼び出しでの ASP.NET ページの例を示します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-252">The following markup is an example ASP.NET page with a simple script call to the login and logout methods of the AuthenticationService class.</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a><span data-ttu-id="5bb8d-253">ASP.NET AJAX を使用してデータをプロファイルにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-253">Accessing ASP.NET Profiling Data via AJAX</span></span>

<span data-ttu-id="5bb8d-254">サービスのプロファイリング、ASP.NET は、ASP.NET AJAX 拡張機能を通じても公開されます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-254">The ASP.NET profiling service is also exposed through the ASP.NET AJAX Extensions.</span></span> <span data-ttu-id="5bb8d-255">ASP.NET プロファイル サービスはユーザー データを格納および取得する度で詳細な API を提供する優れた生産性ツールを指定できます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-255">Since the ASP.NET profiling service provides a rich, granular API by which to store and retrieve user data, this can be an excellent productivity tool.</span></span>

<span data-ttu-id="5bb8d-256">プロファイル サービスは、web.config; で有効にする必要があります。既定ではありません。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-256">The profile service must be enabled in web.config; it is not by default.</span></span> <span data-ttu-id="5bb8d-257">そうことを確認、`profileService`子要素が有効になっている = true に指定された web.config、およびプロパティの読み取りまたは次のように記述されたことを指定したこと。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-257">To do so, ensure that the `profileService` child element has enabled= true specified in web.config, and that you have specified which properties can be read or written as follows:</span></span>

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

<span data-ttu-id="5bb8d-258">プロファイル サービスも構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-258">The profile service must also be configured.</span></span> <span data-ttu-id="5bb8d-259">プロファイリング サービスの構成は、このホワイト ペーパーの範囲外では、プロファイルの構成設定で定義されているグループをグループ名のサブプロパティとしてアクセス可能になることに注意してください。 ことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-259">Although configuration of the profiling service is outside of the scope of this whitepaper, it is worthwhile to note that groups as defined in profile configuration settings will be accessible as sub-properties of the group name.</span></span> <span data-ttu-id="5bb8d-260">で指定した次のプロファイル セクション。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-260">For example, with the following profile section specified:</span></span>

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

<span data-ttu-id="5bb8d-261">クライアント スクリプトでは、名前、Address.Line1、Address.Line2、Address.City、Address.State、Address.Zip、および BackgroundColor ProfileService クラスのプロパティ フィールドのプロパティとしてへのアクセスにはできます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-261">Client script would be able to access Name, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip, and BackgroundColor as properties of the properties field of the ProfileService class.</span></span>

<span data-ttu-id="5bb8d-262">AJAX プロファイル サービスが構成されているとページですぐに使用されます。ただしを使用する前に 1 回読み込むことになります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-262">Once the AJAX Profiling Service is configured, it will be immediately available in pages; however, it will have to be loaded once before use.</span></span>

<span data-ttu-id="5bb8d-263">*Sys.Services.ProfileService メンバー*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-263">*Sys.Services.ProfileService members*</span></span>

<span data-ttu-id="5bb8d-264">*プロパティ フィールド:*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-264">*properties field:*</span></span>

<span data-ttu-id="5bb8d-265">プロパティ フィールドでは、ドット、演算子の名前の規則で参照できる子プロパティとして構成されているプロファイルのすべてのデータを公開します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-265">The properties field exposes all configured profile data as child properties that can be referenced by the dot-operator-name convention.</span></span> <span data-ttu-id="5bb8d-266">プロパティ グループの子であるプロパティは、"GroupName.PropertyName"と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-266">Properties that are children of property groups are referred to as GroupName.PropertyName.</span></span> <span data-ttu-id="5bb8d-267">ユーザーの状態を取得する上で示した例のプロファイルの構成、次の識別子を使用できます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-267">In the example profile configuration presented above, to get the state of the user, you could use the following identifier:</span></span>

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

<span data-ttu-id="5bb8d-268">*メソッドを読み込みます。*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-268">*load method:*</span></span>

<span data-ttu-id="5bb8d-269">サーバーから、選択した一覧またはすべてのプロパティを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-269">Loads a selected list or all properties from the server.</span></span>

<span data-ttu-id="5bb8d-270">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-270">*Parameters:*</span></span>

| <span data-ttu-id="5bb8d-271">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-271">**Parameter Name**</span></span> | <span data-ttu-id="5bb8d-272">**意味**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-272">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="5bb8d-273">propertyNames</span><span class="sxs-lookup"><span data-stu-id="5bb8d-273">propertyNames</span></span> | <span data-ttu-id="5bb8d-274">省略可能 (既定値は null) です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-274">Optional (defaults to null).</span></span> <span data-ttu-id="5bb8d-275">サーバーから読み込まれるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-275">The properties to be loaded from the server.</span></span> |
| <span data-ttu-id="5bb8d-276">loadCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="5bb8d-276">loadCompletedCallback</span></span> | <span data-ttu-id="5bb8d-277">省略可能 (既定値は null) です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-277">Optional (defaults to null).</span></span> <span data-ttu-id="5bb8d-278">読み込みが完了したときに呼び出す関数。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-278">The function to call when loading has completed.</span></span> |
| <span data-ttu-id="5bb8d-279">failedCallback</span><span class="sxs-lookup"><span data-stu-id="5bb8d-279">failedCallback</span></span> | <span data-ttu-id="5bb8d-280">省略可能 (既定値は null) です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-280">Optional (defaults to null).</span></span> <span data-ttu-id="5bb8d-281">エラーが発生した場合に呼び出す関数。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-281">The function to call if an error occurs.</span></span> |
| <span data-ttu-id="5bb8d-282">userContext</span><span class="sxs-lookup"><span data-stu-id="5bb8d-282">userContext</span></span> | <span data-ttu-id="5bb8d-283">省略可能 (既定値は null) です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-283">Optional (defaults to null).</span></span> <span data-ttu-id="5bb8d-284">コールバック関数に渡されるコンテキスト情報。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-284">Context information to be passed to the callback function.</span></span> |

<span data-ttu-id="5bb8d-285">Load 関数の戻り値ではありません。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-285">The load function does not have a return value.</span></span> <span data-ttu-id="5bb8d-286">かどうか、呼び出しが正常に完了にはいずれかを呼び出す、`loadCompletedCallback`パラメーターまたは`defaultLoadCompletedCallback`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-286">If the call completed successfully, it will call either the `loadCompletedCallback` parameter or `defaultLoadCompletedCallback` property.</span></span> <span data-ttu-id="5bb8d-287">呼び出しに失敗した、またはいずれか、タイムアウトが期限切れの場合、`failedCallback`パラメーターまたは`defaultFailedCallback`プロパティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-287">If the call failed, or the timeout expired, either the `failedCallback` parameter or `defaultFailedCallback` property will be called.</span></span>

<span data-ttu-id="5bb8d-288">場合、`propertyNames`パラメーターが指定されていない、すべての読み取りに構成されたプロパティは、サーバーから取得されます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-288">If the `propertyNames` parameter is not supplied, all read-configured properties are retrieved from the server.</span></span>

<span data-ttu-id="5bb8d-289">*メソッドを保存します。*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-289">*save method:*</span></span>

<span data-ttu-id="5bb8d-290">Save() メソッドは、ASP.NET のユーザーのプロファイルを指定したプロパティ リスト (またはすべてのプロパティ) を保存します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-290">The save() method saves the specified property list (or all properties) to the user's ASP.NET profile.</span></span>

<span data-ttu-id="5bb8d-291">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-291">*Parameters:*</span></span>

| <span data-ttu-id="5bb8d-292">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-292">**Parameter Name**</span></span> | <span data-ttu-id="5bb8d-293">**意味**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-293">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="5bb8d-294">propertyNames</span><span class="sxs-lookup"><span data-stu-id="5bb8d-294">propertyNames</span></span> | <span data-ttu-id="5bb8d-295">省略可能 (既定値は null) です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-295">Optional (defaults to null).</span></span> <span data-ttu-id="5bb8d-296">サーバーに保存するプロパティです。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-296">The properties to be saved to the server.</span></span> |
| <span data-ttu-id="5bb8d-297">saveCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="5bb8d-297">saveCompletedCallback</span></span> | <span data-ttu-id="5bb8d-298">省略可能 (既定値は null) です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-298">Optional (defaults to null).</span></span> <span data-ttu-id="5bb8d-299">保存するときに呼び出される関数が完了しました。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-299">The function to call when saving has completed.</span></span> |
| <span data-ttu-id="5bb8d-300">failedCallback</span><span class="sxs-lookup"><span data-stu-id="5bb8d-300">failedCallback</span></span> | <span data-ttu-id="5bb8d-301">省略可能 (既定値は null) です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-301">Optional (defaults to null).</span></span> <span data-ttu-id="5bb8d-302">エラーが発生した場合に呼び出す関数。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-302">The function to call if an error occurs.</span></span> |
| <span data-ttu-id="5bb8d-303">userContext</span><span class="sxs-lookup"><span data-stu-id="5bb8d-303">userContext</span></span> | <span data-ttu-id="5bb8d-304">省略可能 (既定値は null) です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-304">Optional (defaults to null).</span></span> <span data-ttu-id="5bb8d-305">コールバック関数に渡されるコンテキスト情報。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-305">Context information to be passed to the callback function.</span></span> |

<span data-ttu-id="5bb8d-306">Save 関数には、戻り値はありません。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-306">The save function does not have a return value.</span></span> <span data-ttu-id="5bb8d-307">いずれかが呼び出す、呼び出しが正常に完了した場合、`saveCompletedCallback`パラメーターまたは`defaultSaveCompletedCallback`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-307">If the call completes successfully, it will call either the `saveCompletedCallback` parameter or `defaultSaveCompletedCallback` property.</span></span> <span data-ttu-id="5bb8d-308">呼び出しに失敗した、またはいずれか、タイムアウトが期限切れの場合、`failedCallback`または`defaultFailedCallback`プロパティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-308">If the call failed, or the timeout expired, either the `failedCallback` or `defaultFailedCallback` property will be called.</span></span>

<span data-ttu-id="5bb8d-309">場合、`propertyNames`パラメーターが null、すべてのプロファイル プロパティは、サーバーに送信され、サーバーによって決定するプロパティを保存できるし、することはできません。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-309">If the `propertyNames` parameter is null, all profile properties will be sent to the server, and the server will decide which properties can be saved and which cannot.</span></span>

<span data-ttu-id="5bb8d-310">*defaultFailedCallback プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-310">*defaultFailedCallback property (get, set):*</span></span>

<span data-ttu-id="5bb8d-311">このプロパティは、web サービスとの通信に障害が発生した場合に呼び出す必要がある関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-311">This property specifies a function that should be called if a failure to communicate with the web service occurs.</span></span> <span data-ttu-id="5bb8d-312">デリゲート (または関数の参照)、受信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-312">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="5bb8d-313">このプロパティで指定された関数の参照は、次のシグネチャが必要です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-313">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

<span data-ttu-id="5bb8d-314">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-314">*Parameters:*</span></span>

| <span data-ttu-id="5bb8d-315">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-315">**Parameter Name**</span></span> | <span data-ttu-id="5bb8d-316">**意味**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-316">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="5bb8d-317">Error</span><span class="sxs-lookup"><span data-stu-id="5bb8d-317">Error</span></span> | <span data-ttu-id="5bb8d-318">エラー情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-318">Specifies the error information.</span></span> |
| <span data-ttu-id="5bb8d-319">userContext</span><span class="sxs-lookup"><span data-stu-id="5bb8d-319">userContext</span></span> | <span data-ttu-id="5bb8d-320">時に提供されたユーザーのコンテキスト情報を指定します、読み込みまたは保存関数が呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-320">Specifies the user context information provided when the load or save function was called.</span></span> |
| <span data-ttu-id="5bb8d-321">methodName</span><span class="sxs-lookup"><span data-stu-id="5bb8d-321">methodName</span></span> | <span data-ttu-id="5bb8d-322">呼び出し元のメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-322">The name of the calling method.</span></span> |

<span data-ttu-id="5bb8d-323">*defaultSaveCompleted プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-323">*defaultSaveCompleted property (get, set):*</span></span>

<span data-ttu-id="5bb8d-324">このプロパティは、ユーザーのプロファイル データの保存の完了時に呼び出す必要がある関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-324">This property specifies a function that should be called upon the completion of saving the user's profile data.</span></span> <span data-ttu-id="5bb8d-325">デリゲート (または関数の参照)、受信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-325">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="5bb8d-326">このプロパティで指定された関数の参照は、次のシグネチャが必要です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-326">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

<span data-ttu-id="5bb8d-327">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-327">*Parameters:*</span></span>

| <span data-ttu-id="5bb8d-328">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-328">**Parameter Name**</span></span> | <span data-ttu-id="5bb8d-329">**意味**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-329">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="5bb8d-330">numPropsSaved</span><span class="sxs-lookup"><span data-stu-id="5bb8d-330">numPropsSaved</span></span> | <span data-ttu-id="5bb8d-331">保存されたプロパティの数を指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-331">Specifies the number of properties that were saved.</span></span> |
| <span data-ttu-id="5bb8d-332">userContext</span><span class="sxs-lookup"><span data-stu-id="5bb8d-332">userContext</span></span> | <span data-ttu-id="5bb8d-333">時に提供されたユーザーのコンテキスト情報を指定します、読み込みまたは保存関数が呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-333">Specifies the user context information provided when the load or save function was called.</span></span> |
| <span data-ttu-id="5bb8d-334">methodName</span><span class="sxs-lookup"><span data-stu-id="5bb8d-334">methodName</span></span> | <span data-ttu-id="5bb8d-335">呼び出し元のメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-335">The name of the calling method.</span></span> |

<span data-ttu-id="5bb8d-336">*defaultLoadCompleted プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-336">*defaultLoadCompleted property (get, set):*</span></span>

<span data-ttu-id="5bb8d-337">このプロパティは、ユーザーのプロファイル データの読み込みの完了時に呼び出す必要がある関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-337">This property specifies a function that should be called upon the completion of loading of the user's profile data.</span></span> <span data-ttu-id="5bb8d-338">デリゲート (または関数の参照)、受信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-338">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="5bb8d-339">このプロパティで指定された関数の参照は、次のシグネチャが必要です。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-339">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

<span data-ttu-id="5bb8d-340">*パラメーター:*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-340">*Parameters:*</span></span>

| <span data-ttu-id="5bb8d-341">**パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-341">**Parameter Name**</span></span> | <span data-ttu-id="5bb8d-342">**意味**</span><span class="sxs-lookup"><span data-stu-id="5bb8d-342">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="5bb8d-343">numPropsLoaded</span><span class="sxs-lookup"><span data-stu-id="5bb8d-343">numPropsLoaded</span></span> | <span data-ttu-id="5bb8d-344">読み込まれるプロパティの数を指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-344">Specifies the number of properties loaded.</span></span> |
| <span data-ttu-id="5bb8d-345">userContext</span><span class="sxs-lookup"><span data-stu-id="5bb8d-345">userContext</span></span> | <span data-ttu-id="5bb8d-346">時に提供されたユーザーのコンテキスト情報を指定します、読み込みまたは保存関数が呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-346">Specifies the user context information provided when the load or save function was called.</span></span> |
| <span data-ttu-id="5bb8d-347">methodName</span><span class="sxs-lookup"><span data-stu-id="5bb8d-347">methodName</span></span> | <span data-ttu-id="5bb8d-348">呼び出し元のメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-348">The name of the calling method.</span></span> |

<span data-ttu-id="5bb8d-349">*パスのプロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-349">*path property (get, set):*</span></span>

<span data-ttu-id="5bb8d-350">このプロパティは、プログラムでプロファイル web サービスの場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-350">This property programmatically determines the location of the profile web service.</span></span> <span data-ttu-id="5bb8d-351">既定のプロファイル サービス プロバイダーと 1 つをオーバーライドするために使用できます、ScriptManager コントロールの ProfileService 子ノードのパスのプロパティの宣言を設定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-351">It can be used to override the default profile service provider, as well as one set declaratively in the Path property of the ScriptManager control's ProfileService child node.</span></span>

<span data-ttu-id="5bb8d-352">既定のプロファイル サービスの場所が変更されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-352">Note that the location of the default profile service does not change.</span></span> <span data-ttu-id="5bb8d-353">ただし、ASP.NET AJAX を使用すると、ASP.NET AJAX 認証サービスのプロキシと同じクラス インターフェイスを提供する web サービスの場所を指定できます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-353">However, ASP.NET AJAX allows you to specify the location of a web service that provides the same class interface as the ASP.NET AJAX authentication service proxy.</span></span>

<span data-ttu-id="5bb8d-354">また、現在のサイトから、スクリプト要求を転送する値にこのプロパティを設定しないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-354">Note also that this property should not be set to a value that directs the script request off of the current site.</span></span> <span data-ttu-id="5bb8d-355">テクノロジの基になる AJAX では、サイト間の要求を投稿する必要があり、クライアントのブラウザーにセキュリティ例外を生成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-355">The technology underlying AJAX should not post cross-site requests, and may generate a security exception in the client browser.</span></span>

<span data-ttu-id="5bb8d-356">このプロパティは、`String`プロファイル web サービスへのパスを表すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-356">This property is a `String` object representing the path to the profile web service.</span></span>

<span data-ttu-id="5bb8d-357">*timeout プロパティ (get、set):*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-357">*timeout property (get, set):*</span></span>

<span data-ttu-id="5bb8d-358">このプロパティは、失敗したと仮定すると、読み込み前に、プロファイル サービスを待つか、要求を保存する時間の長さを決定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-358">This property determines the length of time to wait for the profile service before assuming the load or save request has failed.</span></span> <span data-ttu-id="5bb8d-359">呼び出しが完了するを待機中にタイムアウトになると、要求が失敗しましたコールバックが呼び出されます、呼び出しは完了しません。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-359">If the timeout expires while waiting for a call to complete, the request-failed callback will be called, and the call will not complete.</span></span>

<span data-ttu-id="5bb8d-360">このプロパティは、`Number`プロファイル サービスから結果を待機するミリ秒数を表すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-360">This property is a `Number` object representing the number of milliseconds to wait for results from the profile service.</span></span>

<span data-ttu-id="5bb8d-361">*コード サンプル: ページの読み込み時のプロファイル データの読み込み*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-361">*Code sample: Loading profile data at page load*</span></span>

<span data-ttu-id="5bb8d-362">次のコードでは、ユーザーが認証されたかどうかを確認し、として、ページのユーザーの推奨される背景色を読み込む場合は。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-362">The following code will check to see whether a user is authenticated, and if so, will load the user's preferred background color as the page's.</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a><span data-ttu-id="5bb8d-363">*カスタム認証のサービス プロバイダーを使用します。*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-363">*Using a Custom Authentication Service Provider*</span></span>

<span data-ttu-id="5bb8d-364">ASP.NET AJAX Extensions を使用すると、カスタム web サービスを通じて、機能を公開することで、カスタム スクリプトの認証サービス プロバイダーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-364">The ASP.NET AJAX Extensions allow you to create a custom script authentication service provider by exposing your functionality through a custom web service.</span></span> <span data-ttu-id="5bb8d-365">使用するために、web サービスが 2 つのメソッドを公開する必要があります`Login`と`Logout`; 既定の ASP.NET AJAX 認証 web サービスと同じメソッド シグネチャを持つこれらのメソッドを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-365">In order to be used, your web service must expose two methods, `Login` and `Logout`; and these methods must be specified with the same method signatures as the default ASP.NET AJAX Authentication web service.</span></span>

<span data-ttu-id="5bb8d-366">カスタム web サービスを作成した後は、コード、またはクライアント スクリプトを使用してプログラムでは、宣言によって、ページのいずれかへのパスを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-366">Once you have created the custom web service, you will need to specify the path to it, either declaratively on your page, programmatically in code, or via the client script.</span></span>

<span data-ttu-id="5bb8d-367">*宣言によってパスを設定します。*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-367">*To set the path declaratively:*</span></span>

<span data-ttu-id="5bb8d-368">宣言によって、パスを設定するには、ASP.NET ページの scriptmanager コントロール オブジェクトの子である AuthenticationService を含めます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-368">To set the path declaratively, include the AuthenticationService child of the ScriptManager object on your ASP.NET page:</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

<span data-ttu-id="5bb8d-369">*コードでパスを設定します。*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-369">*To set the path in code:*</span></span>

<span data-ttu-id="5bb8d-370">パスをプログラムで設定するには、スクリプト マネージャーのインスタンスを使用してパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-370">To set the path programmatically, specify the path via the instance of your script manager:</span></span>

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

<span data-ttu-id="5bb8d-371">*スクリプトでパスを設定します。*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-371">*To set the path in script:*</span></span>

<span data-ttu-id="5bb8d-372">スクリプトでパスをプログラムで設定するには、利用、 `path` AuthenticationService クラスのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-372">To set the path programmatically in script, utilize the `path` property of the AuthenticationService class:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

<span data-ttu-id="5bb8d-373">*カスタム認証のサンプル Web サービス*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-373">*Sample Web Service for Custom Authentication*</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a><span data-ttu-id="5bb8d-374">まとめ</span><span class="sxs-lookup"><span data-stu-id="5bb8d-374">Summary</span></span>

<span data-ttu-id="5bb8d-375">ASP.NET サービス - 具体的にはプロファイル、メンバーシップ、および認証サービスでは、クライアント ブラウザーで JavaScript を簡単に公開されます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-375">ASP.NET services - specifically the profiling, membership, and authentication services - are easily exposed to JavaScript on the client browser.</span></span> <span data-ttu-id="5bb8d-376">これにより、開発者は複雑に Updatepanel などのコントロールに依存することがなく、シームレスに認証メカニズムと、クライアント側のコードを統合できます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-376">This allows developers to integrate their client-side code with the authentication mechanism seamlessly, without depending on controls such as UpdatePanels to do the heavy lifting.</span></span> <span data-ttu-id="5bb8d-377">Web の構成設定を利用することで同様に、クライアントからプロファイル データを保護することができます。既定では、データがないと開発者する必要があります選択でプロファイル プロパティ。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-377">Profile data can be protected from the client as well, by utilizing web configuration settings; no data is available by default, and developers must opt-in to profile properties.</span></span>

<span data-ttu-id="5bb8d-378">さらに、同等のメソッド シグネチャを持つ簡略化された web サービスの実装を作成すると、開発者はこれらの組み込みの ASP.NET サービス プロバイダーがカスタム スクリプトを作成できます。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-378">Furthermore, by creating simplified web service implementations with equivalent method signatures, developers can create custom script providers for these intrinsic ASP.NET services.</span></span> <span data-ttu-id="5bb8d-379">これらの手法のサポートは、さまざまな特定のニーズに対応する柔軟性を開発者に提供しながら、リッチ クライアント アプリケーションの開発を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-379">Support for these techniques simplifies the development of rich client applications, while providing developers with a wide range of flexibility to meet specific needs.</span></span>

## <a name="bio"></a><span data-ttu-id="5bb8d-380">*自己紹介*</span><span class="sxs-lookup"><span data-stu-id="5bb8d-380">*Bio*</span></span>

<span data-ttu-id="5bb8d-381">1997 年からマイクロソフトの Web テクノロジで働いてあり myKB.com プレジデント、Scott Cate ([www.myKB.com](http://www.myKB.com)) ベースのナレッジ ベースのソフトウェア ソリューションに重点を置いてアプリケーションを ASP.NET の記述を専門としています。</span><span class="sxs-lookup"><span data-stu-id="5bb8d-381">Scott Cate has been working with Microsoft Web technologies since 1997 and is the President of myKB.com ([www.myKB.com](http://www.myKB.com)) where he specializes in writing ASP.NET based applications focused on Knowledge Base Software solutions.</span></span> <span data-ttu-id="5bb8d-382">Scott は時に電子メールが接続可能[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)またはで彼のブログ[ScottCate.com](http://ScottCate.com)</span><span class="sxs-lookup"><span data-stu-id="5bb8d-382">Scott can be contacted via email at [scott.cate@myKB.com](mailto:scott.cate@myKB.com) or his blog at [ScottCate.com](http://ScottCate.com)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5bb8d-383">[前へ](understanding-asp-net-ajax-updatepanel-triggers.md)
> [次へ](understanding-asp-net-ajax-localization.md)</span><span class="sxs-lookup"><span data-stu-id="5bb8d-383">[Previous](understanding-asp-net-ajax-updatepanel-triggers.md)
[Next](understanding-asp-net-ajax-localization.md)</span></span>
