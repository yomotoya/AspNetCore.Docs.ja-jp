---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: "Windows 認証 (c#) を使用してユーザーを認証 |Microsoft ドキュメント"
author: microsoft
description: "MVC アプリケーションのコンテキストで Windows 認証を使用する方法を説明します。 アプリケーションの web co 内で Windows 認証を有効にする方法を学習するとしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: d52597e65272fa202ef4980924f669dcc4cec593
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="authenticating-users-with-windows-authentication-c"></a><span data-ttu-id="321a7-104">Windows 認証 (c#) を使用してユーザーを認証</span><span class="sxs-lookup"><span data-stu-id="321a7-104">Authenticating Users with Windows Authentication (C#)</span></span>
====================
<span data-ttu-id="321a7-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="321a7-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="321a7-106">MVC アプリケーションのコンテキストで Windows 認証を使用する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="321a7-106">Learn how to use Windows authentication in the context of an MVC application.</span></span> <span data-ttu-id="321a7-107">IIS で認証を構成する方法と、アプリケーションの web 構成ファイル内で Windows 認証を有効にする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="321a7-107">You learn how to enable Windows authentication within your application's web configuration file and how to configure authentication with IIS.</span></span> <span data-ttu-id="321a7-108">最後に、[Authorize] 属性を使用して、特定の Windows ユーザーまたはグループにコント ローラー アクションへのアクセスを制限する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="321a7-108">Finally, you learn how to use the [Authorize] attribute to restrict access to controller actions to particular Windows users or groups.</span></span>


<span data-ttu-id="321a7-109">このチュートリアルの目標は、利用を実行できる方法について説明する、セキュリティのパスワードには、インターネット インフォメーション サービスに組み込まれている機能は、MVC アプリケーション内のビューを保護します。</span><span class="sxs-lookup"><span data-stu-id="321a7-109">The goal of this tutorial is to explain how you can take advantage of the security features built into Internet Information Services to password protect the views in your MVC applications.</span></span> <span data-ttu-id="321a7-110">特定の Windows ユーザーまたは特定の Windows グループのメンバーであるユーザーによってのみ呼び出されるコント ローラーのアクションを許可する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="321a7-110">You learn how to allow controller actions to be invoked only by particular Windows users or users who are members of particular Windows groups.</span></span>

<span data-ttu-id="321a7-111">Windows 認証を使用して意味は会社の内部 web サイト (イントラネット サイト) を作成して、ユーザーが、web サイトにアクセスするときに、標準の Windows ユーザー名とパスワードを使用できるようにするときにします。</span><span class="sxs-lookup"><span data-stu-id="321a7-111">Using Windows authentication makes sense when you are building an internal company website (an intranet site) and you want your users to be able to use their standard Windows user names and passwords when accessing the website.</span></span> <span data-ttu-id="321a7-112">Web サイト (インターネット web サイト) が直面している、外側へを構築する場合は、フォーム認証の代わりに使用を検討してください。</span><span class="sxs-lookup"><span data-stu-id="321a7-112">If you are building an outwards facing website (an Internet website) consider using Forms authentication instead.</span></span>

#### <a name="enabling-windows-authentication"></a><span data-ttu-id="321a7-113">Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="321a7-113">Enabling Windows Authentication</span></span>

<span data-ttu-id="321a7-114">新しい ASP.NET MVC アプリケーションを作成するときに、Windows 認証は既定では無効です。</span><span class="sxs-lookup"><span data-stu-id="321a7-114">When you create a new ASP.NET MVC application, Windows authentication is not enabled by default.</span></span> <span data-ttu-id="321a7-115">フォーム認証は、MVC アプリケーションを有効になっている既定の認証の種類です。</span><span class="sxs-lookup"><span data-stu-id="321a7-115">Forms authentication is the default authentication type enabled for MVC applications.</span></span> <span data-ttu-id="321a7-116">MVC アプリケーションの web の構成 (web.config) ファイルを変更して、Windows 認証を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="321a7-116">You must enable Windows authentication by modifying your MVC application's web configuration (web.config) file.</span></span> <span data-ttu-id="321a7-117">検索、&lt;認証&gt;セクションし、次のようにフォーム認証ではなく Windows を使用するように変更します。</span><span class="sxs-lookup"><span data-stu-id="321a7-117">Find the &lt;authentication&gt; section and modify it to use Windows instead of Forms authentication like this:</span></span>

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

<span data-ttu-id="321a7-118">Windows 認証を有効にすると、web サーバーはユーザーの認証を担当になります。</span><span class="sxs-lookup"><span data-stu-id="321a7-118">When you enable Windows authentication, your web server becomes responsible for authenticating users.</span></span> <span data-ttu-id="321a7-119">通常、2 つの異なる種類の作成して、ASP.NET MVC アプリケーションを展開するときに使用する web サーバーがあります。</span><span class="sxs-lookup"><span data-stu-id="321a7-119">Typically, there are two different types of web servers that you use when creating and deploying an ASP.NET MVC application.</span></span>

<span data-ttu-id="321a7-120">最初に、MVC アプリケーションの開発中には、Visual Studio に付属する ASP.NET 開発 Web サーバーを使用します。</span><span class="sxs-lookup"><span data-stu-id="321a7-120">First, while developing an MVC application, you use the ASP.NET Development Web Server included with Visual Studio.</span></span> <span data-ttu-id="321a7-121">既定では、ASP.NET 開発 Web サーバーは、現在の Windows アカウント (任意のアカウントが Windows にログオンするため) のコンテキストですべてのページを実行します。</span><span class="sxs-lookup"><span data-stu-id="321a7-121">By default, the ASP.NET Development Web Server executes all pages in the context of the current Windows account (whatever account you used to log into Windows).</span></span>

<span data-ttu-id="321a7-122">ASP.NET 開発 Web サーバーには、NTLM 認証もサポートしています。</span><span class="sxs-lookup"><span data-stu-id="321a7-122">The ASP.NET Development Web Server also supports NTLM authentication.</span></span> <span data-ttu-id="321a7-123">NTLM 認証を有効にするには、ソリューション エクスプ ローラー ウィンドウで、プロジェクトの名前を右クリックし、プロパティ を選択します。</span><span class="sxs-lookup"><span data-stu-id="321a7-123">You can enable NTLM authentication by right-clicking the name of your project in the Solution Explorer window and selecting Properties.</span></span> <span data-ttu-id="321a7-124">次に、[Web] タブを選択し、NTLM のチェック ボックスをオン (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="321a7-124">Next, select the Web tab and check the NTLM checkbox (see Figure 1).</span></span>

<span data-ttu-id="321a7-125">**図 1 – 有効にする ASP.NET 開発 Web サーバーの NTLM 認証**</span><span class="sxs-lookup"><span data-stu-id="321a7-125">**Figure 1 – Enabling NTLM authentication for the ASP.NET Development Web Server**</span></span>

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

<span data-ttu-id="321a7-127">実稼働 web アプリケーションの場合、手の形でとして使用する IIS web サーバー。</span><span class="sxs-lookup"><span data-stu-id="321a7-127">For a production web application, on the hand, you use IIS as your web server.</span></span> <span data-ttu-id="321a7-128">IIS では、認証などのいくつかの種類をサポートします。</span><span class="sxs-lookup"><span data-stu-id="321a7-128">IIS supports several types of authentication including:</span></span>

- <span data-ttu-id="321a7-129">基本認証 – は、HTTP 1.0 プロトコルの一部として定義されます。</span><span class="sxs-lookup"><span data-stu-id="321a7-129">Basic Authentication – Defined as part of the HTTP 1.0 protocol.</span></span> <span data-ttu-id="321a7-130">ユーザー名とパスワードをクリア テキスト (Base64 エンコード) で、インターネット経由で送信します。</span><span class="sxs-lookup"><span data-stu-id="321a7-130">Sends user names and passwords in clear text (Base64 encoded) across the Internet.</span></span> <span data-ttu-id="321a7-131">ダイジェスト認証 – は、インターネット経由でそれ自体には、パスワードの代わりに、パスワードのハッシュを送信します。</span><span class="sxs-lookup"><span data-stu-id="321a7-131">- Digest Authentication – Sends a hash of a password, instead of the password itself, across the internet.</span></span> <span data-ttu-id="321a7-132">-統合された Windows (NTLM) 認証 – windows を使用するイントラネット環境で使用する認証の最適な型です。</span><span class="sxs-lookup"><span data-stu-id="321a7-132">- Integrated Windows (NTLM) Authentication – The best type of authentication to use in intranet environments using windows.</span></span> <span data-ttu-id="321a7-133">-証明書の認証: クライアント側の証明書を使用して認証を有効します。</span><span class="sxs-lookup"><span data-stu-id="321a7-133">- Certificate Authentication – Enables authentication using a client-side certificate.</span></span> <span data-ttu-id="321a7-134">証明書は、Windows ユーザー アカウントにマップされます。</span><span class="sxs-lookup"><span data-stu-id="321a7-134">The certificate maps to a Windows user account.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="321a7-135">これらのさまざまな種類の認証の詳細については、次を参照してください[https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).。</span><span class="sxs-lookup"><span data-stu-id="321a7-135">For a more detailed overview of these different types of authentication, see [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).</span></span>


<span data-ttu-id="321a7-136">インターネット インフォメーション サービス マネージャーを使用して、特定の種類の認証を有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="321a7-136">You can use Internet Information Services Manager to enable a particular type of authentication.</span></span> <span data-ttu-id="321a7-137">すべての種類の認証はすべてのオペレーティング システムの場合は使用できないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="321a7-137">Be aware that all types of authentication are not available in the case of every operating system.</span></span> <span data-ttu-id="321a7-138">さらに、Windows vista では IIS 7.0 を使用している場合は、インターネット インフォメーション サービス マネージャーに表示される前に、さまざまな種類の Windows 認証を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="321a7-138">Furthermore, if you are using IIS 7.0 with Windows Vista, you will need to enable the different types of Windows authentication before they appear in the Internet Information Services Manager.</span></span> <span data-ttu-id="321a7-139">開いている**コントロール パネル、プログラム、プログラムと機能、Windows の機能のオンまたはオフ**、インターネット インフォメーション サービス ノードを展開し、(図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="321a7-139">Open **Control Panel, Programs, Programs and Features, Turn Windows features on or off**, and expand the Internet Information Services node (see Figure 2).</span></span>

<span data-ttu-id="321a7-140">**図 2 – 有効にする Windows の IIS の機能**</span><span class="sxs-lookup"><span data-stu-id="321a7-140">**Figure 2 – Enabling Windows IIS features**</span></span>

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

<span data-ttu-id="321a7-142">インターネット インフォメーション サービスを使用して、有効にするにまたは、さまざまな種類の認証を無効にできます。</span><span class="sxs-lookup"><span data-stu-id="321a7-142">Using Internet Information Services, you can enable or disable different types of authentication.</span></span> <span data-ttu-id="321a7-143">たとえば、図 3 を示しています無効にすると匿名認証と統合 Windows (NTLM) 認証を有効にすると IIS 7.0 を使用する場合。</span><span class="sxs-lookup"><span data-stu-id="321a7-143">For example, Figure 3 illustrates disabling anonymous authentication and enabling Integrated Windows (NTLM) authentication when using IIS 7.0.</span></span>

<span data-ttu-id="321a7-144">**図 3: 統合 Windows 認証を有効にします。**</span><span class="sxs-lookup"><span data-stu-id="321a7-144">**Figure 3 – Enabling Integrated Windows Authentication**</span></span>

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a><span data-ttu-id="321a7-146">Windows を承認するユーザーとグループ</span><span class="sxs-lookup"><span data-stu-id="321a7-146">Authorizing Windows Users and Groups</span></span>

<span data-ttu-id="321a7-147">Windows 認証を有効にした後は、コント ローラーのアクションまたはコント ローラーへのアクセスを制御するのに [Authorize] 属性を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="321a7-147">After you enable Windows authentication, you can use the [Authorize] attribute to control access to controllers or controller actions.</span></span> <span data-ttu-id="321a7-148">この属性は、全体の MVC コント ローラーまたは特定のコント ローラーのアクションに適用できます。</span><span class="sxs-lookup"><span data-stu-id="321a7-148">This attribute can be applied to an entire MVC controller or a particular controller action.</span></span>

<span data-ttu-id="321a7-149">たとえば、1 の一覧で、Home コント ローラーは、Index()、CompanySecrets()、および StephenSecrets() という 3 つのアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="321a7-149">For example, the Home controller in Listing 1 exposes three actions named Index(), CompanySecrets(), and StephenSecrets().</span></span> <span data-ttu-id="321a7-150">Index() アクションだれでも呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="321a7-150">Anyone can invoke the Index() action.</span></span> <span data-ttu-id="321a7-151">ただし、Windows のローカル管理者グループのメンバーだけでは、CompanySecrets() アクションを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="321a7-151">However, only members of the Windows local Managers group can invoke the CompanySecrets() action.</span></span> <span data-ttu-id="321a7-152">最後に、のみ、Windows ドメイン ユーザー (Redmond ドメイン) 内の Stephen をという名前では、StephenSecrets() アクションを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="321a7-152">Finally, only the Windows domain user named Stephen (in the Redmond domain) can invoke the StephenSecrets() action.</span></span>

<span data-ttu-id="321a7-153">**1 – controllers \homecontroller.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="321a7-153">**Listing 1 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="321a7-154">ため Windows ユーザー アカウント制御 (UAC)、Windows Vista または Windows Server 2008 を使用する場合、ローカルの Administrators グループの他のグループとは異なる動作が。</span><span class="sxs-lookup"><span data-stu-id="321a7-154">Because of Windows User Account Control (UAC), when working with Windows Vista or Windows Server 2008, the local Administrators group will behave differently than other groups.</span></span> <span data-ttu-id="321a7-155">[Authorize] 属性、コンピューターの UAC 設定を変更する場合を除き、ローカルの Administrators グループのメンバーが正しく認識されません。</span><span class="sxs-lookup"><span data-stu-id="321a7-155">The [Authorize] attribute won't correctly recognize a member of the local Administrators group unless you modify your computer's UAC settings.</span></span>


<span data-ttu-id="321a7-156">適切なアクセス許可がないコント ローラーのアクションを起動するときの動作については、有効になっている認証の種類によって異なります。</span><span class="sxs-lookup"><span data-stu-id="321a7-156">Exactly what happens when you attempt to invoke a controller action without being the right permissions depends on the type of authentication enabled.</span></span> <span data-ttu-id="321a7-157">既定では、ASP.NET 開発サーバーを使用する場合、単に空白のページを取得します。</span><span class="sxs-lookup"><span data-stu-id="321a7-157">By default, when using the ASP.NET Development Server, you simply get a blank page.</span></span> <span data-ttu-id="321a7-158">ページが表示されると、 **401 が承認されていない**HTTP 応答のステータス。</span><span class="sxs-lookup"><span data-stu-id="321a7-158">The page is served with a **401 Not Authorized** HTTP Response Status.</span></span>

<span data-ttu-id="321a7-159">かどうか、その一方で、無効になっている匿名認証と基本認証を有効にすると、IIS を使用しているし、保護されているページを要求するたびに、ログイン ダイアログ プロンプト続けて表示 (図 4 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="321a7-159">If, on the other hand, you are using IIS with Anonymous authentication disabled and Basic authentication enabled, then you keep getting a login dialog prompt each time you request the protected page (see Figure 4).</span></span>

<span data-ttu-id="321a7-160">**図 4-基本認証のログイン ダイアログ ボックス**</span><span class="sxs-lookup"><span data-stu-id="321a7-160">**Figure 4 – Basic authentication login dialog**</span></span>

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a><span data-ttu-id="321a7-162">まとめ</span><span class="sxs-lookup"><span data-stu-id="321a7-162">Summary</span></span>

<span data-ttu-id="321a7-163">このチュートリアルでは、ASP.NET MVC アプリケーションのコンテキストで Windows 認証を使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="321a7-163">This tutorial explained how you can use Windows authentication in the context of an ASP.NET MVC application.</span></span> <span data-ttu-id="321a7-164">IIS で認証を構成する方法と、アプリケーションの web 構成ファイル内で Windows 認証を有効にする方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="321a7-164">You learned how to enable Windows authentication within your application's web configuration file and how to configure authentication with IIS.</span></span> <span data-ttu-id="321a7-165">最後に、[Authorize] 属性を使用して、特定の Windows ユーザーまたはグループにコント ローラー アクションへのアクセスを制限する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="321a7-165">Finally, you learned how to use the [Authorize] attribute to restrict access to controller actions to particular Windows users or groups.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="321a7-166">[前へ](authenticating-users-with-forms-authentication-cs.md)
[次へ](preventing-javascript-injection-attacks-cs.md)</span><span class="sxs-lookup"><span data-stu-id="321a7-166">[Previous](authenticating-users-with-forms-authentication-cs.md)
[Next](preventing-javascript-injection-attacks-cs.md)</span></span>
