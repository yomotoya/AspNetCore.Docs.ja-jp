---
title: "Azure Active Directory B2C のクラウド認証"
author: camsoper
description: "ASP.NET Core での Azure Active Directory B2C の認証を設定する方法を検出します。"
ms.author: casoper
manager: wpickett
ms.date: 01/12/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/azure-ad-b2c
custom: mvc
ms.openlocfilehash: 5c4716022c61e33b0301fa0077f911dcc4b3628c
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2018
---
# <a name="cloud-authentication-with-azure-active-directory-b2c"></a><span data-ttu-id="8bf23-103">Azure Active Directory B2C のクラウド認証</span><span class="sxs-lookup"><span data-stu-id="8bf23-103">Cloud authentication with Azure Active Directory B2C</span></span>

<span data-ttu-id="8bf23-104">作成者: [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="8bf23-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="8bf23-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) は、web およびモバイル アプリケーションのクラウド id 管理ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="8bf23-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for your web and mobile apps.</span></span> <span data-ttu-id="8bf23-106">サービスは、クラウドとオンプレミスでホストされているアプリの認証を提供します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-106">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="8bf23-107">認証の種類には、ソーシャル ネットワーク アカウントでは、個々 のアカウントを含めるし、エンタープライズ アカウントをフェデレーションします。</span><span class="sxs-lookup"><span data-stu-id="8bf23-107">Authentication types include include individual accounts, social network accounts, and federated enterprise accounts.</span></span>  <span data-ttu-id="8bf23-108">また、Azure AD B2C では、最小構成で多要素認証を提供できます。</span><span class="sxs-lookup"><span data-stu-id="8bf23-108">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

<span data-ttu-id="8bf23-109">このチュートリアルで学習する方法。</span><span class="sxs-lookup"><span data-stu-id="8bf23-109">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8bf23-110">Azure Active Directory B2C テナントを作成します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-110">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="8bf23-111">Azure AD B2C にアプリを登録します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-111">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="8bf23-112">Visual Studio を使用して Azure AD B2C テナント認証を使用するように構成 ASP.NET Core web アプリを作成するには</span><span class="sxs-lookup"><span data-stu-id="8bf23-112">Use Visual Studio to create an ASP.NET Core web app configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="8bf23-113">Azure AD B2C テナントの動作を制御するポリシーを構成します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-113">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8bf23-114">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="8bf23-114">Prerequisites</span></span>

<span data-ttu-id="8bf23-115">以下は、このチュートリアルで必要です。</span><span class="sxs-lookup"><span data-stu-id="8bf23-115">The following are required for this walkthrough:</span></span>

* <span data-ttu-id="8bf23-116">[Microsoft Azure サブスクリプション](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)です。</span><span class="sxs-lookup"><span data-stu-id="8bf23-116">[Microsoft Azure subscription](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).</span></span> 
* <span data-ttu-id="8bf23-117">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (任意のエディション)</span><span class="sxs-lookup"><span data-stu-id="8bf23-117">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (any edition)</span></span>

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="8bf23-118">Azure Active Directory B2C テナントを作成します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-118">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="8bf23-119">Azure Active Directory B2C テナントを作成する[ドキュメント」の説明に従って](/azure/active-directory-b2c/active-directory-b2c-get-started)です。</span><span class="sxs-lookup"><span data-stu-id="8bf23-119">Create an Azure Active Directory B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="8bf23-120">メッセージが表示されたら、テナントを Azure サブスクリプションに関連付けるは省略可能なこのチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="8bf23-120">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="register-the-app-in-azure-ad-b2c"></a><span data-ttu-id="8bf23-121">Azure AD B2C でアプリを登録します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-121">Register the app in Azure AD B2C</span></span>

<span data-ttu-id="8bf23-122">新しく作成された Azure AD B2C テナント登録を使用してアプリを[ドキュメント」の手順](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下にある、 **web アプリを登録**セクション。</span><span class="sxs-lookup"><span data-stu-id="8bf23-122">In the newly created Azure AD B2C tenant, register your app using [the steps in the documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) under the **Register a web app** section.</span></span> <span data-ttu-id="8bf23-123">停止時刻、 **web アプリ クライアント シークレットの作成**セクションです。</span><span class="sxs-lookup"><span data-stu-id="8bf23-123">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="8bf23-124">クライアント シークレットは、このチュートリアルでは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="8bf23-124">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="8bf23-125">次の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-125">Use the following values:</span></span>

| <span data-ttu-id="8bf23-126">設定</span><span class="sxs-lookup"><span data-stu-id="8bf23-126">Setting</span></span>                       | <span data-ttu-id="8bf23-127">[値]</span><span class="sxs-lookup"><span data-stu-id="8bf23-127">Value</span></span>                     | <span data-ttu-id="8bf23-128">メモ</span><span class="sxs-lookup"><span data-stu-id="8bf23-128">Notes</span></span>                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8bf23-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="8bf23-129">**Name**</span></span>                      | <span data-ttu-id="8bf23-130">*\<アプリ名\>*</span><span class="sxs-lookup"><span data-stu-id="8bf23-130">*\<app name\>*</span></span>            | <span data-ttu-id="8bf23-131">入力、**名前**コンシューマーにアプリを説明するアプリのです。</span><span class="sxs-lookup"><span data-stu-id="8bf23-131">Enter a **Name** for the app that describes your app to consumers.</span></span>                                                                                                                                 |
| <span data-ttu-id="8bf23-132">**Web アプリは、ロールまたは web API**</span><span class="sxs-lookup"><span data-stu-id="8bf23-132">**Include web app / web API**</span></span> | <span data-ttu-id="8bf23-133">[はい]</span><span class="sxs-lookup"><span data-stu-id="8bf23-133">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="8bf23-134">**暗黙のフローを許可します。**</span><span class="sxs-lookup"><span data-stu-id="8bf23-134">**Allow implicit flow**</span></span>       | <span data-ttu-id="8bf23-135">[はい]</span><span class="sxs-lookup"><span data-stu-id="8bf23-135">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="8bf23-136">**応答 URL**</span><span class="sxs-lookup"><span data-stu-id="8bf23-136">**Reply URL**</span></span>                 | `https://localhost:44300` | <span data-ttu-id="8bf23-137">応答 Url は、エンドポイントが Azure AD B2C がアプリを要求するすべてのトークンを返します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-137">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> <span data-ttu-id="8bf23-138">Visual Studio では、使用する返信 URL を提供します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-138">Visual Studio provides the Reply URL to use.</span></span> <span data-ttu-id="8bf23-139">ここでは、次のように入力します。`https://localhost:44300`フォームを完了します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-139">For now, enter `https://localhost:44300` to complete the form.</span></span> |
| <span data-ttu-id="8bf23-140">**アプリ ID URI**</span><span class="sxs-lookup"><span data-stu-id="8bf23-140">**App ID URI**</span></span>                | <span data-ttu-id="8bf23-141">空白のままに</span><span class="sxs-lookup"><span data-stu-id="8bf23-141">Leave blank</span></span>               | <span data-ttu-id="8bf23-142">このチュートリアルでは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="8bf23-142">Not required for this tutorial.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="8bf23-143">**ネイティブ クライアントが含まれます**</span><span class="sxs-lookup"><span data-stu-id="8bf23-143">**Include native client**</span></span>     | <span data-ttu-id="8bf23-144">×</span><span class="sxs-lookup"><span data-stu-id="8bf23-144">No</span></span>                        |                                                                                                                                                                                                    |

> [!WARNING]
> <span data-ttu-id="8bf23-145">かどうかの注意してください、localhost 以外の応答 URL を設定する、[応答 URL の一覧で許可されている制約](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url)です。</span><span class="sxs-lookup"><span data-stu-id="8bf23-145">If setting up a non-localhost Reply URL, be aware of the [constraints on what is allowed in the Reply URL list](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url).</span></span> 

<span data-ttu-id="8bf23-146">アプリを登録すると、テナント内のアプリの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8bf23-146">After the app is registered, the list of apps in the tenant is displayed.</span></span> <span data-ttu-id="8bf23-147">登録されたアプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-147">Select the app that was just registered.</span></span> <span data-ttu-id="8bf23-148">選択、**コピー**の右側にあるアイコン、**アプリケーション ID**アプリケーション ID をクリップボードにコピーするフィールドです。</span><span class="sxs-lookup"><span data-stu-id="8bf23-148">Select the **Copy** icon to the right of the **Application ID** field to copy the Application ID to the clipboard.</span></span>

<span data-ttu-id="8bf23-149">何も複数現時点では、Azure AD B2C テナントで構成できますが、ブラウザー ウィンドウを開いたままにしておきます。</span><span class="sxs-lookup"><span data-stu-id="8bf23-149">Nothing more can be configured in the Azure AD B2C tenant at this time, but leave the browser window open.</span></span> <span data-ttu-id="8bf23-150">多くの構成がある ASP.NET Core アプリケーションの作成後にします。</span><span class="sxs-lookup"><span data-stu-id="8bf23-150">There is more configuration after the ASP.NET Core app is created.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a><span data-ttu-id="8bf23-151">Visual Studio 2017 で ASP.NET Core アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-151">Create an ASP.NET Core app in Visual Studio 2017</span></span>

<span data-ttu-id="8bf23-152">Visual Studio Web アプリケーション テンプレートは、認証に使用する Azure AD B2C テナント構成されていることができます。</span><span class="sxs-lookup"><span data-stu-id="8bf23-152">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="8bf23-153">Visual Studio で。</span><span class="sxs-lookup"><span data-stu-id="8bf23-153">In Visual Studio:</span></span>

1. <span data-ttu-id="8bf23-154">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-154">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="8bf23-155">選択**Web アプリケーション**テンプレートの一覧からです。</span><span class="sxs-lookup"><span data-stu-id="8bf23-155">Select **Web Application** from the list of templates.</span></span>
3. <span data-ttu-id="8bf23-156">選択、**認証の変更**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8bf23-156">Select the **Change Authentication** button.</span></span>
    
    ![変更の認証 ボタン](./azure-ad-b2c/_static/changeauth.png)

4. <span data-ttu-id="8bf23-158">**認証の変更**ダイアログで、**個々 のユーザー アカウント**、し、 **、クラウド内の既存のユーザー ストアへの接続**ドロップダウン リストにします。</span><span class="sxs-lookup"><span data-stu-id="8bf23-158">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 
    
    ![認証の変更 ダイアログ](./azure-ad-b2c/_static/changeauthdialog.png)

5. <span data-ttu-id="8bf23-160">次の値をフォームに入力します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-160">Complete the form with the following values:</span></span>
    
    | <span data-ttu-id="8bf23-161">設定</span><span class="sxs-lookup"><span data-stu-id="8bf23-161">Setting</span></span>                       | <span data-ttu-id="8bf23-162">[値]</span><span class="sxs-lookup"><span data-stu-id="8bf23-162">Value</span></span>                                             |
    |-------------------------------|---------------------------------------------------|
    | <span data-ttu-id="8bf23-163">**ドメイン名**</span><span class="sxs-lookup"><span data-stu-id="8bf23-163">**Domain Name**</span></span>               | <span data-ttu-id="8bf23-164">*\<B2C テナントのドメイン名\>*</span><span class="sxs-lookup"><span data-stu-id="8bf23-164">*\<the domain name of your B2C tenant\>*</span></span>          |
    | <span data-ttu-id="8bf23-165">**アプリケーション ID**</span><span class="sxs-lookup"><span data-stu-id="8bf23-165">**Application ID**</span></span>            | <span data-ttu-id="8bf23-166">*\<アプリケーション ID をクリップボードから貼り付けます\>*</span><span class="sxs-lookup"><span data-stu-id="8bf23-166">*\<paste the Application ID from the clipboard\>*</span></span> |
    | <span data-ttu-id="8bf23-167">**コールバックのパス**</span><span class="sxs-lookup"><span data-stu-id="8bf23-167">**Callback Path**</span></span>             | <span data-ttu-id="8bf23-168">*\<既定値を使用します。\>*</span><span class="sxs-lookup"><span data-stu-id="8bf23-168">*\<use the default value\>*</span></span>                       |
    | <span data-ttu-id="8bf23-169">**サインアップまたはサインイン ポリシー**</span><span class="sxs-lookup"><span data-stu-id="8bf23-169">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                    |
    | <span data-ttu-id="8bf23-170">**パスワードのリセット ポリシー**</span><span class="sxs-lookup"><span data-stu-id="8bf23-170">**Reset password policy**</span></span>     | `B2C_1_SSPR`                                      |
    | <span data-ttu-id="8bf23-171">**プロファイルのポリシーを編集します。**</span><span class="sxs-lookup"><span data-stu-id="8bf23-171">**Edit profile policy**</span></span>       | <span data-ttu-id="8bf23-172">*\<空白のままに\>*</span><span class="sxs-lookup"><span data-stu-id="8bf23-172">*\<leave blank\>*</span></span>                                 |

    <span data-ttu-id="8bf23-173">選択、**コピー**  の横にリンク**Reply URI** Reply URI をクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="8bf23-173">Select the **Copy** link next to **Reply URI** to copy the Reply URI to the clipboard.</span></span> <span data-ttu-id="8bf23-174">選択**OK**を閉じる、**認証の変更**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="8bf23-174">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="8bf23-175">選択**OK** web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-175">Select **OK** to create the web app.</span></span>

## <a name="finish-the-b2c-app-registration"></a><span data-ttu-id="8bf23-176">B2C アプリの登録を完了します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-176">Finish the B2C app registration</span></span>

<span data-ttu-id="8bf23-177">ブラウザー ウィンドウで開かれたまま B2C アプリのプロパティに戻ります。</span><span class="sxs-lookup"><span data-stu-id="8bf23-177">Return to the browser window with the B2C app properties still open.</span></span> <span data-ttu-id="8bf23-178">一時的な変更**応答 URL** Visual Studio から以前の値にコピーを指定します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-178">Change the temporary **Reply URL** specified earlier to the value copied from Visual Studio.</span></span> <span data-ttu-id="8bf23-179">選択**保存**ウィンドウの上部にあります。</span><span class="sxs-lookup"><span data-stu-id="8bf23-179">Select **Save** at the top of the window.</span></span>

> [!TIP]
> <span data-ttu-id="8bf23-180">応答 URL をコピーしていない場合、web プロジェクト プロパティで、[デバッグ] タブから SSL アドレスを使用し、追加、 **CallbackPath**値から*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="8bf23-180">If you didn't copy the Reply URL, use the SSL address from the Debug tab in the web project properties, and append the **CallbackPath** value from *appsettings.json*.</span></span>

## <a name="configure-policies"></a><span data-ttu-id="8bf23-181">ポリシーを構成します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-181">Configure policies</span></span>

<span data-ttu-id="8bf23-182">Azure AD B2C のドキュメントの手順を使用[サインアップまたはサインイン ポリシーを作成する](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)、し[パスワード リセット ポリシーを作成する](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy)です。</span><span class="sxs-lookup"><span data-stu-id="8bf23-182">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), and then [create a password reset policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy).</span></span> <span data-ttu-id="8bf23-183">ドキュメントの例の値を使用して**Id プロバイダー**、**サインアップ属性**、および**アプリケーション要求**です。</span><span class="sxs-lookup"><span data-stu-id="8bf23-183">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="8bf23-184">使用して、**を今すぐ実行**ドキュメント」の説明に従って、ポリシーをテストするにはボタンは省略可能です。</span><span class="sxs-lookup"><span data-stu-id="8bf23-184">Using the **Run now** button to test the policies as described in the documentation is optional.</span></span>

> [!WARNING]
> <span data-ttu-id="8bf23-185">ポリシー名は、ドキュメントで説明されているとおりに正確にこれらのポリシーで使用されていたことを確認、**認証の変更**Visual Studio でのダイアログ。</span><span class="sxs-lookup"><span data-stu-id="8bf23-185">Ensure the policy names are exactly as described in the documentation, as those policies were used in the **Change Authentication** dialog in Visual Studio.</span></span> <span data-ttu-id="8bf23-186">ポリシー名を検証できます*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="8bf23-186">The policy names can be verified in *appsettings.json*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="8bf23-187">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="8bf23-187">Run the app</span></span>

<span data-ttu-id="8bf23-188">Visual Studio で、キーを押して**f5 キーを押して**アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-188">In Visual Studio, press **F5** to build and run the app.</span></span> <span data-ttu-id="8bf23-189">Web アプリが起動後、選択**サインイン**です。</span><span class="sxs-lookup"><span data-stu-id="8bf23-189">After the web app launches, select **Sign in**.</span></span>

![アプリへのサインインします。](./azure-ad-b2c/_static/signin.png)

<span data-ttu-id="8bf23-191">ブラウザーは、Azure AD B2C テナントにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="8bf23-191">The browser redirects to the Azure AD B2C tenant.</span></span> <span data-ttu-id="8bf23-192">(1 つには、ポリシーのテストが作成された) 場合は、既存のアカウントでサインインまたは選択**今すぐサインアップ**新しいアカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-192">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="8bf23-193">**パスワードを忘れた場合ですか?**忘れてもパスワードをリセットするリンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-193">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

![Azure AD B2C ログイン](./azure-ad-b2c/_static/b2csts.png)

<span data-ttu-id="8bf23-195">正常にサインインした後は、ブラウザーは、web アプリにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="8bf23-195">After successfully signing in, the browser redirects to the web app.</span></span>

![成功](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a><span data-ttu-id="8bf23-197">次の手順</span><span class="sxs-lookup"><span data-stu-id="8bf23-197">Next steps</span></span>

<span data-ttu-id="8bf23-198">このチュートリアルでは、学習した方法。</span><span class="sxs-lookup"><span data-stu-id="8bf23-198">In this tutorial, you will learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8bf23-199">Azure Active Directory B2C テナントを作成します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-199">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="8bf23-200">Azure AD B2C にアプリを登録します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-200">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="8bf23-201">Visual Studio を使用して Azure AD B2C テナント認証を使用するように構成 ASP.NET Core Web アプリケーションを作成するには</span><span class="sxs-lookup"><span data-stu-id="8bf23-201">Use Visual Studio to create an ASP.NET Core Web Application configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="8bf23-202">Azure AD B2C テナントの動作を制御するポリシーを構成します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-202">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

<span data-ttu-id="8bf23-203">ASP.NET Core アプリケーションが認証に使用する Azure AD B2C、構成されていること、 [Authorize attribute](xref:security/authorization/simple)アプリをセキュリティで保護するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="8bf23-203">Now that the ASP.NET Core app is configured to use Azure AD B2C for authentication, the [Authorize attribute](xref:security/authorization/simple) can be used to secure your app.</span></span> <span data-ttu-id="8bf23-204">学習することにより、アプリケーションの開発を続行するには。</span><span class="sxs-lookup"><span data-stu-id="8bf23-204">Continue developing your app by learning to:</span></span>

* <span data-ttu-id="8bf23-205">[Azure AD B2C ユーザー インターフェイスのカスタマイズ](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)です。</span><span class="sxs-lookup"><span data-stu-id="8bf23-205">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="8bf23-206">[パスワードの複雑さの要件を構成する](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)です。</span><span class="sxs-lookup"><span data-stu-id="8bf23-206">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="8bf23-207">[多要素認証を有効にする](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)です。</span><span class="sxs-lookup"><span data-stu-id="8bf23-207">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="8bf23-208">など、追加の id プロバイダーを構成[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)、 [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)、 [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)、 [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)、 [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)、およびその他。</span><span class="sxs-lookup"><span data-stu-id="8bf23-208">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="8bf23-209">[Azure AD Graph API を使用して](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)Azure AD B2C テナントからグループ メンバーシップなど、追加のユーザー情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="8bf23-209">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>