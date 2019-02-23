---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Web API の SSL の使用 |Microsoft Docs
author: MikeWasson
description: SSL クライアント証明書の使用など、ASP.NET Web API を使用した SSL を使用する方法を示します。
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 69c0d217f605096d968435c062ee9931f8dff75f
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2019
ms.locfileid: "56744106"
---
<a name="working-with-ssl-in-web-api"></a><span data-ttu-id="fa60d-103">Web API の SSL の使用</span><span class="sxs-lookup"><span data-stu-id="fa60d-103">Working with SSL in Web API</span></span>
====================
<span data-ttu-id="fa60d-104">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fa60d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fa60d-105">いくつかの一般的な認証方式は、プレーンな HTTP は安全ではありません。</span><span class="sxs-lookup"><span data-stu-id="fa60d-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="fa60d-106">具体的には、基本認証とフォーム認証、暗号化されていない資格情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="fa60d-107">セキュリティ保護、これらの認証スキーム*する必要があります*SSL を使用します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="fa60d-108">さらに、SSL クライアント証明書は、クライアントの認証に使用できます。</span><span class="sxs-lookup"><span data-stu-id="fa60d-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="fa60d-109">サーバーで SSL を有効にします。</span><span class="sxs-lookup"><span data-stu-id="fa60d-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="fa60d-110">IIS 7 またはそれ以降の SSL を設定します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="fa60d-111">作成または証明書を取得します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-111">Create or get a certificate.</span></span> <span data-ttu-id="fa60d-112">テストは、自己署名証明書を作成できます。</span><span class="sxs-lookup"><span data-stu-id="fa60d-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="fa60d-113">HTTPS バインドを追加します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="fa60d-114">詳細については、次を参照してください。 [IIS 7 で SSL の設定方法](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="fa60d-115">ローカル テストでは、Visual Studio からの IIS Express での SSL を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="fa60d-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="fa60d-116">[プロパティ] ウィンドウで次のように設定します。 **SSL が有効になっている**に**True**します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="fa60d-117">値に注意してください**SSL URL**; HTTPS 接続をテストするため、この URL を使用します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="fa60d-118">Web API コント ローラーでの SSL の適用</span><span class="sxs-lookup"><span data-stu-id="fa60d-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="fa60d-119">HTTPS と HTTP バインディングの両方があれば、クライアントがサイトにアクセスするのに HTTP を使用できます。</span><span class="sxs-lookup"><span data-stu-id="fa60d-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="fa60d-120">その他のリソースには SSL が必要ですが、HTTP を利用する一部のリソースを許可する場合があります。</span><span class="sxs-lookup"><span data-stu-id="fa60d-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="fa60d-121">その場合は、アクション フィルターを使用して、保護されたリソースに対して SSL を要求します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="fa60d-122">次のコードは、SSL をチェックする Web API 認証フィルターを示しています。</span><span class="sxs-lookup"><span data-stu-id="fa60d-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="fa60d-123">SSL を必要とする Web API アクションすべてには、このフィルターを追加します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="fa60d-124">SSL クライアント証明書</span><span class="sxs-lookup"><span data-stu-id="fa60d-124">SSL Client Certificates</span></span>

<span data-ttu-id="fa60d-125">SSL は、公開キー インフラストラクチャ証明書を使用して認証を提供します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="fa60d-126">サーバーは、クライアントにサーバーを認証する証明書を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa60d-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="fa60d-127">あまり一般的で、サーバーに証明書を提供するクライアントですが、これはクライアントを認証するための 1 つのオプションです。</span><span class="sxs-lookup"><span data-stu-id="fa60d-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="fa60d-128">Ssl クライアント証明書を使用するには、ユーザーに署名証明書を配布する方法が必要です。</span><span class="sxs-lookup"><span data-stu-id="fa60d-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="fa60d-129">多くのアプリケーションの種類、これは、優れたユーザー エクスペリエンスになりませんが一部の環境 (たとえば、enterprise) で可能な場合があります。</span><span class="sxs-lookup"><span data-stu-id="fa60d-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="fa60d-130">長所</span><span class="sxs-lookup"><span data-stu-id="fa60d-130">Advantages</span></span> | <span data-ttu-id="fa60d-131">短所</span><span class="sxs-lookup"><span data-stu-id="fa60d-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="fa60d-132">-証明書資格情報では、ユーザー名/パスワードよりも強力です。</span><span class="sxs-lookup"><span data-stu-id="fa60d-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="fa60d-133">SSL は、認証、メッセージの整合性、およびメッセージの暗号化との完全なセキュリティで保護されたチャネルを提供します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="fa60d-134">取得して、PKI 証明書の管理-する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa60d-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="fa60d-135">-クライアントのプラットフォームでは、SSL クライアント証明書をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa60d-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="fa60d-136">でクライアント証明書を受け入れるように IIS を構成するには、IIS マネージャーを開き、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="fa60d-137">ツリー ビューで [サイト] ノードをクリックします。</span><span class="sxs-lookup"><span data-stu-id="fa60d-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="fa60d-138">ダブルクリックして、 **SSL 設定**中央のペインで機能します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="fa60d-139">**クライアント証明書**、これらのオプションのいずれかを選択します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="fa60d-140">**受け入れる**:IIS は、クライアントからの証明書を受け取りますが、1 つは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="fa60d-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="fa60d-141">**必要な**:クライアント証明書が必要です。</span><span class="sxs-lookup"><span data-stu-id="fa60d-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="fa60d-142">(このオプションを有効にするには、必要がありますも選択する"SSL")</span><span class="sxs-lookup"><span data-stu-id="fa60d-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="fa60d-143">ApplicationHost.config ファイルでこれらのオプションを設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="fa60d-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="fa60d-144">**SslNegotiateCert**フラグは、IIS は、クライアントからの証明書を受け取りますが、(IIS マネージャーで"Accept"オプションに相当) の 1 つは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="fa60d-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="fa60d-145">証明書を要求するには、設定、 **SslRequireCert**フラグ。</span><span class="sxs-lookup"><span data-stu-id="fa60d-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="fa60d-146">テストは、ローカル applicationhost でこれらのオプションで IIS Express を設定することもできます。"Documents\IISExpress\config"にある構成ファイル。</span><span class="sxs-lookup"><span data-stu-id="fa60d-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="fa60d-147">テスト用クライアント証明書の作成</span><span class="sxs-lookup"><span data-stu-id="fa60d-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="fa60d-148">テスト目的で、使用することができます[MakeCert.exe](/windows/desktop/SecCrypto/makecert)クライアント証明書を作成します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-148">For testing purposes, you can use [MakeCert.exe](/windows/desktop/SecCrypto/makecert) to create a client certificate.</span></span> <span data-ttu-id="fa60d-149">まず、テスト ルート証明機関を作成します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="fa60d-150">Makecert は、秘密キーのパスワードを入力することを求められます。</span><span class="sxs-lookup"><span data-stu-id="fa60d-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="fa60d-151">次に、サーバーの"信頼されたルート証明機関"ストアには、次のように、テストに、証明書を追加します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="fa60d-152">MMC を開きます。</span><span class="sxs-lookup"><span data-stu-id="fa60d-152">Open MMC.</span></span>
2. <span data-ttu-id="fa60d-153">**ファイル**、**スナップインの追加と削除**します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="fa60d-154">選択**コンピューター アカウント**します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="fa60d-155">選択**ローカル コンピューター**ウィザードを完了します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="fa60d-156">ナビゲーション ウィンドウの下には、「信頼されたルート証明機関」ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="fa60d-157">**アクション**メニューで、**すべてのタスク**、順にクリックします**インポート**証明書のインポート ウィザードを開始します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="fa60d-158">TempCA.cer を証明書ファイルを参照します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="fa60d-159">をクリックして**オープン**、順にクリックします**次**ウィザードを完了します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="fa60d-160">(求め、パスワードを再入力します。)</span><span class="sxs-lookup"><span data-stu-id="fa60d-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="fa60d-161">今すぐ最初の証明書によって署名されているクライアント証明書を作成します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="fa60d-162">Web API のクライアント証明書の使用</span><span class="sxs-lookup"><span data-stu-id="fa60d-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="fa60d-163">サーバー側では、呼び出すことによって、クライアント証明書を取得できます[GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx)要求メッセージ。</span><span class="sxs-lookup"><span data-stu-id="fa60d-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="fa60d-164">クライアント証明書がない場合は、null、メソッドを返します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="fa60d-165">それ以外を返します、 **X509Certificate2**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="fa60d-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="fa60d-166">このオブジェクトを使用して、証明書の発行者とサブジェクトなどから情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="fa60d-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="fa60d-167">認証や承認のこの情報を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="fa60d-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
