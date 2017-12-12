---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: "ASP.NET Web Pages (Razor) トラブルシューティング ガイド |Microsoft ドキュメント"
author: tfitzmac
description: "この記事では、ASP.NET Web Pages (Razor) と一部の推奨されるソリューションを使用するときにする必要がありますのある問題について説明します。 ASP.NET Web ページのソフトウェア バージョンしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: e507d97903583c7233456e9139e1a869f8bf75bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="b6d7d-104">ASP.NET Web Pages (Razor) トラブルシューティング ガイド</span><span class="sxs-lookup"><span data-stu-id="b6d7d-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="b6d7d-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b6d7d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b6d7d-106">この記事では、ASP.NET Web Pages (Razor) と一部の推奨されるソリューションを使用するときにする必要がありますのある問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="b6d7d-107">ソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="b6d7d-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="b6d7d-108">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="b6d7d-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="b6d7d-109">このチュートリアルは、ASP.NET Web Pages 2 と ASP.NET Web Pages 1.0 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="b6d7d-110">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="b6d7d-111">ページの実行に関する問題</span><span class="sxs-lookup"><span data-stu-id="b6d7d-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="b6d7d-112">Razor コードの問題</span><span class="sxs-lookup"><span data-stu-id="b6d7d-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="b6d7d-113">セキュリティとメンバーシップに関する問題</span><span class="sxs-lookup"><span data-stu-id="b6d7d-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="b6d7d-114">電子メールの送信に関する問題</span><span class="sxs-lookup"><span data-stu-id="b6d7d-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="b6d7d-115">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="b6d7d-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="b6d7d-116">一般的な質問については、次を参照してください。 [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000)です。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="b6d7d-117">ページの実行に関する問題</span><span class="sxs-lookup"><span data-stu-id="b6d7d-117">Issues with Running Pages</span></span>

<span data-ttu-id="b6d7d-118">さまざまな問題を防ぐことができます*.cshtml*と*.vbhtml*ページから正常に実行します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="b6d7d-119">このセクションでは、一般的なエラー メッセージを一覧表示し、可能性のある原因です。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="b6d7d-120">HTTP エラー 403 - アクセス不可: アクセスが拒否されました</span><span class="sxs-lookup"><span data-stu-id="b6d7d-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="b6d7d-121">*このディレクトリまたは入力した資格情報を使用してページを表示するアクセス許可がありません。*</span><span class="sxs-lookup"><span data-stu-id="b6d7d-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="b6d7d-122">このエラーは、サーバーに .NET Framework の正しいバージョンが実行されていない場合に発生することができます。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="b6d7d-123">(ローカルまたはリモート)、サーバーを実行しているコンピューターに少なくとも .NET Framework 4 をインストールすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="b6d7d-124">また、適切なバージョンを実行する、アプリケーション自体が構成されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="b6d7d-125">WebMatrix で作業中にこの問題をローカルで発生する場合にクリックして、**サイト** ワークスペースで、し、ツリー ビューのをクリックして**設定**です。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="b6d7d-126">**.NET Framework のバージョンの選択**一覧で、選択**.NET 4 (Integrated)**です。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="b6d7d-127">このバージョンは既に設定されている場合は、管理者として WebMatrix を実行してください。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="b6d7d-128">Web サイトのルートが少なくとも 1 つであること確認*.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="b6d7d-129">Web サーバーがリモート サーバーにこのエラーが発生する場合は、サーバー管理者に問い合わせてください。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="b6d7d-130">サーバーに .NET Framework 4 があることを確認または後でインストールされているようにします。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="b6d7d-131">また、アプリケーションが.net Framework のバージョンを使用するように構成されるアプリケーション プールで実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="b6d7d-132">サーバーを制御する場合は、.NET Framework の正しいバージョンが実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="b6d7d-133">インストールの修復を実行してもみて、`aspnet_regiis -iru`コマンド。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="b6d7d-134">(たとえば、.NET Framework をインストールした後に IIS をインストールする場合 IIS がない正しく構成する ASP.NET ページを実行する。)詳細については、次を参照してください。 [ASP.NET IIS 登録ツール (Aspnet\_regiis.exe)](https://msdn.microsoft.com/en-US/library/k6h9cz8h(v=vs.100).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/en-US/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="b6d7d-135">HTTP エラー 403.14 - 許可されていません</span><span class="sxs-lookup"><span data-stu-id="b6d7d-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="b6d7d-136">*Web サーバーは、このディレクトリの内容が表示されないするように構成します。*</span><span class="sxs-lookup"><span data-stu-id="b6d7d-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="b6d7d-137">このエラーは、保護されているリソースを要求する場合に発生することができます (と同様に、 *Web.config*ファイル) か、保護されているフォルダーにある (と同様に*アプリ\_データ*または*アプリ\_コード*)。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="b6d7d-138">HTTP エラー 404.17 - が見つかりません。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="b6d7d-139">*要求されたコンテンツでは、スクリプトである可能性があり、静的ファイル ハンドラーでは提供されません。*</span><span class="sxs-lookup"><span data-stu-id="b6d7d-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="b6d7d-140">このエラーは、サーバーは、.NET Framework 4 を使用する正しく構成されていないか、後で、およびでのコードは認識されない場合に発生する可能性が`@{ }`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="b6d7d-141">前の説明を参照して*HTTP エラー 403 - アクセス不可: アクセスが拒否された*です。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="b6d7d-142">HTTP エラー 404.7 - が見つかりません。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="b6d7d-143">*要求フィルター モジュールはファイル拡張子の拒否するように構成します。*</span><span class="sxs-lookup"><span data-stu-id="b6d7d-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="b6d7d-144">場合、このエラーが発生する可能性が*.cshtml*または*.vbhtml*サーバーで、拡張機能を明示的にブロックされています。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="b6d7d-145">拡張機能を含む Url が含まれないときに、この問題の症状がその Url 作業*.cshtml*または*.vbhtml*機能しません。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="b6d7d-146">考えられる解決方法は、サイトの拡張機能を再度有効にする*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="b6d7d-147">次の例は、有効にする方法を示しています、 *.cshtml*拡張機能です。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="b6d7d-148">HTTP エラー 404.8 - が見つかりません。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="b6d7d-149">*要求フィルター モジュールが、hiddenSegment セクションを含む URL のパスを拒否するように構成します。*</span><span class="sxs-lookup"><span data-stu-id="b6d7d-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="b6d7d-150">このエラーは、保護されているリソースを要求する場合に発生することができます (と同様に、 *Web.config*ファイル) か、保護されているフォルダーにある (と同様に*アプリ\_データ*または*アプリ\_コード*)。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="b6d7d-151">この種類のページは (サーバー エラーは '/' アプリケーションで) を処理できません。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="b6d7d-152">HTTP エラー 404.17 については、前の説明を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="b6d7d-153">Razor コードの問題</span><span class="sxs-lookup"><span data-stu-id="b6d7d-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="b6d7d-154">名前 '*クラス*'、現在のコンテキストに存在しません</span><span class="sxs-lookup"><span data-stu-id="b6d7d-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="b6d7d-155">多くの場合、このエラーが発生する理由は`class`参照、ヘルパーが、ヘルパーはインストールされません。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="b6d7d-156">たとえば、ヘルパーに渡しを使用しようとする場合 NuGet からパッケージをインストールしていない場合は、このエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="b6d7d-157">探して支援者をインストールするには、WebMatrix で、ギャラリーを使用します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="b6d7d-158">かどうか、ヘルパーがインストールされているが、ページも認識されません、try を追加、`using`ステートメントのコードにします。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="b6d7d-159">`using`ステートメントでは、ヘルパーが含まれる名前空間の参照。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="b6d7d-160">たとえば、ASP.NET Web Helpers パッケージ内にある、基本的なヘルパーはでは、`System.Web.Helpers`名前空間。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="b6d7d-161">ヘルパーを使用するページの上部には、この行を追加します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="b6d7d-162">セキュリティとメンバーシップに関する問題</span><span class="sxs-lookup"><span data-stu-id="b6d7d-162">Issues with Security and Membership</span></span>

<span data-ttu-id="b6d7d-163">組み込みのセキュリティ (メンバーシップ) システムは、ASP.NET Web Pages (Razor) で使用する場合、は、次の問題が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="b6d7d-164">このメソッドを呼び出すには、"Membership.Provider"プロパティが"ExtendedMembershipProvider"のインスタンスを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="b6d7d-165">このエラーは、ことを示すことがない`AspNetSqlMembershipProvider`クラスが構成されています。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="b6d7d-166">(症状には、サイトがローカルで正常に動作が、ホスティング プロバイダーのサーバーにパブリッシュするときにこのエラーがスローされます)。この問題の 1 つの解決策をサイトの次を追加することで、簡易なメンバーシップを明示的に有効にする*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="b6d7d-167">電子メールの送信に関する問題</span><span class="sxs-lookup"><span data-stu-id="b6d7d-167">Issues with Sending Email</span></span>

<span data-ttu-id="b6d7d-168">電子メールの送信に関する問題をデバッグする困難なことができます。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="b6d7d-169">最初の問題は、SMTP サーバーに接続できないことを指定できます。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="b6d7d-170">接続が成功した場合、ASP.NET では、オフがメッセージを SMTP サーバーに渡します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="b6d7d-171">ただし、SMTP サーバーが送信することを防止するメッセージ自体に問題があります。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="b6d7d-172">アプリケーションが正常に電子メールを送信しない場合は、次の操作を再試行してください。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="b6d7d-173">SMTP サーバー名は、のようなものでは多くの場合、`smtp.provider.com`または`smtp.provider.net`です。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="b6d7d-174">ただし、ホスティング プロバイダーにサイトを発行する場合、SMTP サーバー名の点にあります`localhost`です。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="b6d7d-175">このような状況では、公開したされ、プロバイダーのサーバーに、サイトが実行中、SMTP サーバーは、アプリケーションの観点からローカル可能性があるために発生します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="b6d7d-176">このサーバー名の変更は、発行プロセスの一部として SMTP サーバー名を変更する必要がある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="b6d7d-177">ポート番号は、通常は 25 です。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-177">The port number is usually 25.</span></span> <span data-ttu-id="b6d7d-178">ただし、一部のプロバイダーを使用する必要ポート 587 またはいくつか他のポートです。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="b6d7d-179">ポート番号と期待どおりに使用する SMTP サーバーの所有者を確認します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="b6d7d-180">適切な資格情報を使用することを確認します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="b6d7d-181">ホスティング プロバイダーには、サイトを公開した場合は、プロバイダーが具体的には示されている電子メールには、資格情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="b6d7d-182">これらの資格情報は、発行に使用する資格情報と異なる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="b6d7d-183">場合によって資格情報をまったく必要ありません。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="b6d7d-184">個人の ISP を使用して電子メールを送信する場合は、電子メール プロバイダーが、資格情報を知っている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="b6d7d-185">パブリッシュした後は、ローカル コンピューターでテストするとよりも別の資格情報を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="b6d7d-186">場合は、電子メール プロバイダーは、暗号化を使用して、設定`WebMail.EnableSsl`に`true`です。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="b6d7d-187">電子メールを送信中にエラーがある場合は、次のような標準の ASP.NET エラー メッセージを参照してください可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![電子メールの問題がある場合に、ASP.NET エラー メッセージ](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="b6d7d-189">使用して電子メールの送信に関する問題をデバッグすることも、`try-catch`次の例のように、ブロックします。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="b6d7d-190">使用すると、`try-catch`ブロック、ASP.NET は、標準的なエラー メッセージを表示しません。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="b6d7d-191">エラーをキャプチャする代わりに、`catch`ブロックの部分です。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="b6d7d-192">適切な値に置き換えてください`your-SMTP-server-name`のようにします。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="b6d7d-193">この方法が表示されるエラー メッセージの一部を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="b6d7d-194">*メールの送信に失敗します。*</span><span class="sxs-lookup"><span data-stu-id="b6d7d-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="b6d7d-195">または</span><span class="sxs-lookup"><span data-stu-id="b6d7d-195">-or-</span></span>

    <span data-ttu-id="b6d7d-196">*接続されているパーティが一定の時間、または確立された接続が接続されているホストが応答に失敗しましたが失敗しました。 正しく応答しなかったために、接続試行が失敗しました*</span><span class="sxs-lookup"><span data-stu-id="b6d7d-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="b6d7d-197">このエラーは通常、アプリケーションが、SMTP サーバーに接続できなかったことを意味します。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="b6d7d-198">サーバー名を確認して、ポート番号。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-198">Check the server name and port number.</span></span>
- <span data-ttu-id="b6d7d-199">*メールボックスが使用できません。サーバーの応答: 5.1.0 &lt; someuser@invaliddomain &gt;送信者を拒否しました: 無効な送信者のドメイン*</span><span class="sxs-lookup"><span data-stu-id="b6d7d-199">*Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain*</span></span>

    <span data-ttu-id="b6d7d-200">このメッセージは、ことを示すことができます、`From`アドレスが間違っているかがありません。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="b6d7d-201">*指定した文字列は、電子メール アドレスに必要な形式ではありません。*</span><span class="sxs-lookup"><span data-stu-id="b6d7d-201">*The specified string is not in the form required for an e-mail address.*</span></span>

    <span data-ttu-id="b6d7d-202">このエラーが発生することの値、`To`または`From`プロパティは、電子メール アドレスとして認識されません。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="b6d7d-203">(ASP.NET が電子メール アドレスが有効などの形式が正しくのみになることを確認することはできません *name@domain.com* )。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="b6d7d-204">エラーが表示されるマークアップを削除 (`@errorMessage`) 実際のサイトにページを公開する前にします。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="b6d7d-205">いないユーザーがサーバーから取得するエラー メッセージが表示できるようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="b6d7d-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="b6d7d-206">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="b6d7d-206">Additional Resources</span></span>

[<span data-ttu-id="b6d7d-207">ASP.NET Web Pages (Razor) FAQ</span><span class="sxs-lookup"><span data-stu-id="b6d7d-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="b6d7d-208">[WebMatrix と ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) ASP.NET web サイトのフォーラム</span><span class="sxs-lookup"><span data-stu-id="b6d7d-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
