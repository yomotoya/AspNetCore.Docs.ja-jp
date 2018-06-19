---
title: ASP.NET のコアでアクティブなディレクトリの B2C を Azure クラウド認証
author: camsoper
description: ASP.NET Core での Azure Active Directory B2C の認証を設定する方法を検出します。
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: a7bad452a68cf7fe7aa81645d79a0ee9e7719fe7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
ms.locfileid: "29905076"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a><span data-ttu-id="b159f-103">ASP.NET のコアでアクティブなディレクトリの B2C を Azure クラウド認証</span><span class="sxs-lookup"><span data-stu-id="b159f-103">Cloud authentication with Azure Active Directory B2C in ASP.NET Core</span></span>

<span data-ttu-id="b159f-104">作成者: [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="b159f-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="b159f-105">[Azure Active Directory の B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) は、web およびモバイル アプリケーションのクラウドのアイデンティティ管理ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="b159f-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for web and mobile apps.</span></span> <span data-ttu-id="b159f-106">サービスは、クラウドとオンプレミスでホストされているアプリの認証を提供します。</span><span class="sxs-lookup"><span data-stu-id="b159f-106">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="b159f-107">認証の種類は、ソーシャル ネットワーク アカウントでは、個々 のアカウントを含めるし、エンタープライズ アカウントをフェデレーションします。</span><span class="sxs-lookup"><span data-stu-id="b159f-107">Authentication types include individual accounts, social network accounts, and federated enterprise accounts.</span></span> <span data-ttu-id="b159f-108">また、Azure AD B2C では、最小構成で多要素認証を提供できます。</span><span class="sxs-lookup"><span data-stu-id="b159f-108">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

> [!TIP]
> <span data-ttu-id="b159f-109">Azure Active Directory (Azure AD) の Azure AD B2C の別の製品サービスしています。</span><span class="sxs-lookup"><span data-stu-id="b159f-109">Azure Active Directory (Azure AD) Azure AD B2C are separate product offerings.</span></span> <span data-ttu-id="b159f-110">Azure AD テナントは、組織を表し、Azure AD B2C テナントは証明書利用者アプリケーションで使用される id のコレクションを表します。</span><span class="sxs-lookup"><span data-stu-id="b159f-110">An Azure AD tenant represents an organization, while an Azure AD B2C tenant represents a collection of identities to be used with relying party applications.</span></span> <span data-ttu-id="b159f-111">詳細については、次を参照してください。 [Azure AD B2C: よく寄せられる質問 (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs)です。</span><span class="sxs-lookup"><span data-stu-id="b159f-111">To learn more, see [Azure AD B2C: Frequently asked questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span></span>

<span data-ttu-id="b159f-112">このチュートリアルで学習する方法。</span><span class="sxs-lookup"><span data-stu-id="b159f-112">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b159f-113">Azure Active Directory B2C テナントを作成します。</span><span class="sxs-lookup"><span data-stu-id="b159f-113">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="b159f-114">Azure AD B2C にアプリを登録します。</span><span class="sxs-lookup"><span data-stu-id="b159f-114">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="b159f-115">Visual Studio を使用して Azure AD B2C テナント認証を使用するように構成 ASP.NET Core web アプリを作成するには</span><span class="sxs-lookup"><span data-stu-id="b159f-115">Use Visual Studio to create an ASP.NET Core web app configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="b159f-116">Azure AD B2C テナントの動作を制御するポリシーを構成します。</span><span class="sxs-lookup"><span data-stu-id="b159f-116">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b159f-117">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="b159f-117">Prerequisites</span></span>

<span data-ttu-id="b159f-118">以下は、このチュートリアルで必要です。</span><span class="sxs-lookup"><span data-stu-id="b159f-118">The following are required for this walkthrough:</span></span>

* [<span data-ttu-id="b159f-119">Microsoft Azure サブスクリプション</span><span class="sxs-lookup"><span data-stu-id="b159f-119">Microsoft Azure subscription</span></span>](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* <span data-ttu-id="b159f-120">[Visual Studio 2017年](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs)(任意のエディション)</span><span class="sxs-lookup"><span data-stu-id="b159f-120">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (any edition)</span></span>

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="b159f-121">Azure Active Directory B2C テナントを作成します。</span><span class="sxs-lookup"><span data-stu-id="b159f-121">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="b159f-122">Azure Active Directory B2C テナントを作成する[ドキュメント」の説明に従って](/azure/active-directory-b2c/active-directory-b2c-get-started)です。</span><span class="sxs-lookup"><span data-stu-id="b159f-122">Create an Azure Active Directory B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="b159f-123">メッセージが表示されたら、テナントを Azure サブスクリプションに関連付けるは省略可能なこのチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="b159f-123">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="register-the-app-in-azure-ad-b2c"></a><span data-ttu-id="b159f-124">Azure AD B2C でアプリを登録します。</span><span class="sxs-lookup"><span data-stu-id="b159f-124">Register the app in Azure AD B2C</span></span>

<span data-ttu-id="b159f-125">新しく作成された Azure AD B2C テナント登録を使用してアプリを[ドキュメント」の手順](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下にある、 **web アプリを登録**セクション。</span><span class="sxs-lookup"><span data-stu-id="b159f-125">In the newly created Azure AD B2C tenant, register your app using [the steps in the documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) under the **Register a web app** section.</span></span> <span data-ttu-id="b159f-126">停止時刻、 **web アプリ クライアント シークレットの作成**セクションです。</span><span class="sxs-lookup"><span data-stu-id="b159f-126">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="b159f-127">クライアント シークレットは、このチュートリアルでは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="b159f-127">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="b159f-128">次の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="b159f-128">Use the following values:</span></span>

| <span data-ttu-id="b159f-129">設定</span><span class="sxs-lookup"><span data-stu-id="b159f-129">Setting</span></span>                       | <span data-ttu-id="b159f-130">[値]</span><span class="sxs-lookup"><span data-stu-id="b159f-130">Value</span></span>                     | <span data-ttu-id="b159f-131">メモ</span><span class="sxs-lookup"><span data-stu-id="b159f-131">Notes</span></span>                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b159f-132">**Name**</span><span class="sxs-lookup"><span data-stu-id="b159f-132">**Name**</span></span>                      | <span data-ttu-id="b159f-133">*&lt;アプリ名&gt;*</span><span class="sxs-lookup"><span data-stu-id="b159f-133">*&lt;app name&gt;*</span></span>        | <span data-ttu-id="b159f-134">入力、**名前**コンシューマーにアプリを説明するアプリのです。</span><span class="sxs-lookup"><span data-stu-id="b159f-134">Enter a **Name** for the app that describes your app to consumers.</span></span>                                                                                                                                 |
| <span data-ttu-id="b159f-135">**Web アプリは、ロールまたは web API**</span><span class="sxs-lookup"><span data-stu-id="b159f-135">**Include web app / web API**</span></span> | <span data-ttu-id="b159f-136">[はい]</span><span class="sxs-lookup"><span data-stu-id="b159f-136">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="b159f-137">**暗黙のフローを許可します。**</span><span class="sxs-lookup"><span data-stu-id="b159f-137">**Allow implicit flow**</span></span>       | <span data-ttu-id="b159f-138">[はい]</span><span class="sxs-lookup"><span data-stu-id="b159f-138">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="b159f-139">**応答 URL**</span><span class="sxs-lookup"><span data-stu-id="b159f-139">**Reply URL**</span></span>                 | `https://localhost:44300` | <span data-ttu-id="b159f-140">応答 Url は、エンドポイントが Azure AD B2C がアプリを要求するすべてのトークンを返します。</span><span class="sxs-lookup"><span data-stu-id="b159f-140">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> <span data-ttu-id="b159f-141">Visual Studio では、使用する返信 URL を提供します。</span><span class="sxs-lookup"><span data-stu-id="b159f-141">Visual Studio provides the Reply URL to use.</span></span> <span data-ttu-id="b159f-142">ここでは、次のように入力します。`https://localhost:44300`フォームを完了します。</span><span class="sxs-lookup"><span data-stu-id="b159f-142">For now, enter `https://localhost:44300` to complete the form.</span></span> |
| <span data-ttu-id="b159f-143">**アプリ ID URI**</span><span class="sxs-lookup"><span data-stu-id="b159f-143">**App ID URI**</span></span>                | <span data-ttu-id="b159f-144">空白のままに</span><span class="sxs-lookup"><span data-stu-id="b159f-144">Leave blank</span></span>               | <span data-ttu-id="b159f-145">このチュートリアルでは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="b159f-145">Not required for this tutorial.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="b159f-146">**ネイティブ クライアントは、します。**</span><span class="sxs-lookup"><span data-stu-id="b159f-146">**Include native client**</span></span>     | <span data-ttu-id="b159f-147">×</span><span class="sxs-lookup"><span data-stu-id="b159f-147">No</span></span>                        |                                                                                                                                                                                                    |

> [!WARNING]
> <span data-ttu-id="b159f-148">かどうかの注意してください、localhost 以外の応答 URL を設定する、[応答 URL の一覧で許可されている制約](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url)です。</span><span class="sxs-lookup"><span data-stu-id="b159f-148">If setting up a non-localhost Reply URL, be aware of the [constraints on what is allowed in the Reply URL list](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url).</span></span> 

<span data-ttu-id="b159f-149">アプリを登録すると、テナント内のアプリの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b159f-149">After the app is registered, the list of apps in the tenant is displayed.</span></span> <span data-ttu-id="b159f-150">登録されたアプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="b159f-150">Select the app that was just registered.</span></span> <span data-ttu-id="b159f-151">選択、**コピー**の右側にあるアイコン、**アプリケーション ID**をクリップボードにコピーするフィールドです。</span><span class="sxs-lookup"><span data-stu-id="b159f-151">Select the **Copy** icon to the right of the **Application ID** field to copy it to the clipboard.</span></span>

<span data-ttu-id="b159f-152">何も複数現時点では、Azure AD B2C テナントで構成できますが、ブラウザー ウィンドウを開いたままにしておきます。</span><span class="sxs-lookup"><span data-stu-id="b159f-152">Nothing more can be configured in the Azure AD B2C tenant at this time, but leave the browser window open.</span></span> <span data-ttu-id="b159f-153">多くの構成がある ASP.NET Core アプリケーションの作成後にします。</span><span class="sxs-lookup"><span data-stu-id="b159f-153">There is more configuration after the ASP.NET Core app is created.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a><span data-ttu-id="b159f-154">Visual Studio 2017 で ASP.NET Core アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="b159f-154">Create an ASP.NET Core app in Visual Studio 2017</span></span>

<span data-ttu-id="b159f-155">Visual Studio Web アプリケーション テンプレートは、認証に使用する Azure AD B2C テナント構成されていることができます。</span><span class="sxs-lookup"><span data-stu-id="b159f-155">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="b159f-156">Visual Studio で。</span><span class="sxs-lookup"><span data-stu-id="b159f-156">In Visual Studio:</span></span>

1. <span data-ttu-id="b159f-157">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="b159f-157">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="b159f-158">選択**Web アプリケーション**テンプレートの一覧からです。</span><span class="sxs-lookup"><span data-stu-id="b159f-158">Select **Web Application** from the list of templates.</span></span>
3. <span data-ttu-id="b159f-159">選択、**認証の変更**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b159f-159">Select the **Change Authentication** button.</span></span>
    
    ![変更の認証 ボタン](./azure-ad-b2c/_static/changeauth.png)

4. <span data-ttu-id="b159f-161">**認証の変更**ダイアログで、**個々 のユーザー アカウント**、し、 **、クラウド内の既存のユーザー ストアへの接続**ドロップダウン リストにします。</span><span class="sxs-lookup"><span data-stu-id="b159f-161">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 
    
    ![認証の変更 ダイアログ](./azure-ad-b2c/_static/changeauthdialog.png)

5. <span data-ttu-id="b159f-163">次の値をフォームに入力します。</span><span class="sxs-lookup"><span data-stu-id="b159f-163">Complete the form with the following values:</span></span>
    
    | <span data-ttu-id="b159f-164">設定</span><span class="sxs-lookup"><span data-stu-id="b159f-164">Setting</span></span>                       | <span data-ttu-id="b159f-165">[値]</span><span class="sxs-lookup"><span data-stu-id="b159f-165">Value</span></span>                                                 |
    |-------------------------------|-------------------------------------------------------|
    | <span data-ttu-id="b159f-166">**ドメイン名**</span><span class="sxs-lookup"><span data-stu-id="b159f-166">**Domain Name**</span></span>               | <span data-ttu-id="b159f-167">*&lt;B2C のテナントのドメイン名&gt;*</span><span class="sxs-lookup"><span data-stu-id="b159f-167">*&lt;the domain name of your B2C tenant&gt;*</span></span>          |
    | <span data-ttu-id="b159f-168">**アプリケーション ID**</span><span class="sxs-lookup"><span data-stu-id="b159f-168">**Application ID**</span></span>            | <span data-ttu-id="b159f-169">*&lt;アプリケーション ID、クリップボードからの貼り付け&gt;*</span><span class="sxs-lookup"><span data-stu-id="b159f-169">*&lt;paste the Application ID from the clipboard&gt;*</span></span> |
    | <span data-ttu-id="b159f-170">**コールバックのパス**</span><span class="sxs-lookup"><span data-stu-id="b159f-170">**Callback Path**</span></span>             | <span data-ttu-id="b159f-171">*&lt;既定値を使用します。&gt;*</span><span class="sxs-lookup"><span data-stu-id="b159f-171">*&lt;use the default value&gt;*</span></span>                       |
    | <span data-ttu-id="b159f-172">**サインアップまたはサイン インのポリシー**</span><span class="sxs-lookup"><span data-stu-id="b159f-172">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                        |
    | <span data-ttu-id="b159f-173">**パスワード ポリシーをリセットします。**</span><span class="sxs-lookup"><span data-stu-id="b159f-173">**Reset password policy**</span></span>     | `B2C_1_SSPR`                                          |
    | <span data-ttu-id="b159f-174">**プロファイルのポリシーを編集します。**</span><span class="sxs-lookup"><span data-stu-id="b159f-174">**Edit profile policy**</span></span>       | <span data-ttu-id="b159f-175">*&lt;空白のままに&gt;*</span><span class="sxs-lookup"><span data-stu-id="b159f-175">*&lt;leave blank&gt;*</span></span>                                 |
    
    <span data-ttu-id="b159f-176">選択、**コピー**  の横にリンク**Reply URI** Reply URI をクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="b159f-176">Select the **Copy** link next to **Reply URI** to copy the Reply URI to the clipboard.</span></span> <span data-ttu-id="b159f-177">選択**OK**を閉じる、**認証の変更**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="b159f-177">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="b159f-178">選択**OK** web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="b159f-178">Select **OK** to create the web app.</span></span>

## <a name="finish-the-b2c-app-registration"></a><span data-ttu-id="b159f-179">B2C アプリの登録を完了します。</span><span class="sxs-lookup"><span data-stu-id="b159f-179">Finish the B2C app registration</span></span>

<span data-ttu-id="b159f-180">ブラウザー ウィンドウで開かれたまま B2C アプリのプロパティに戻ります。</span><span class="sxs-lookup"><span data-stu-id="b159f-180">Return to the browser window with the B2C app properties still open.</span></span> <span data-ttu-id="b159f-181">一時的な変更**応答 URL** Visual Studio から以前の値にコピーを指定します。</span><span class="sxs-lookup"><span data-stu-id="b159f-181">Change the temporary **Reply URL** specified earlier to the value copied from Visual Studio.</span></span> <span data-ttu-id="b159f-182">選択**保存**ウィンドウの上部にあります。</span><span class="sxs-lookup"><span data-stu-id="b159f-182">Select **Save** at the top of the window.</span></span>

> [!TIP]
> <span data-ttu-id="b159f-183">応答 URL をコピーしていない場合、web プロジェクト プロパティで、[デバッグ] タブから SSL アドレスを使用し、追加、 **CallbackPath**値から*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="b159f-183">If you didn't copy the Reply URL, use the SSL address from the Debug tab in the web project properties, and append the **CallbackPath** value from *appsettings.json*.</span></span>

## <a name="configure-policies"></a><span data-ttu-id="b159f-184">ポリシーを構成します。</span><span class="sxs-lookup"><span data-stu-id="b159f-184">Configure policies</span></span>

<span data-ttu-id="b159f-185">Azure AD B2C のドキュメントの手順を使用[サインアップまたはサインイン ポリシーを作成する](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)、し[パスワード リセット ポリシーを作成する](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy)です。</span><span class="sxs-lookup"><span data-stu-id="b159f-185">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), and then [create a password reset policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy).</span></span> <span data-ttu-id="b159f-186">ドキュメントの例の値を使用して**Id プロバイダー**、**サインアップ属性**、および**アプリケーション要求**です。</span><span class="sxs-lookup"><span data-stu-id="b159f-186">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="b159f-187">使用して、**を今すぐ実行**ドキュメント」の説明に従って、ポリシーをテストするにはボタンは省略可能です。</span><span class="sxs-lookup"><span data-stu-id="b159f-187">Using the **Run now** button to test the policies as described in the documentation is optional.</span></span>

> [!WARNING]
> <span data-ttu-id="b159f-188">ポリシー名は、ドキュメントで説明されているとおりに正確にこれらのポリシーで使用されていたことを確認、**認証の変更**Visual Studio でのダイアログ。</span><span class="sxs-lookup"><span data-stu-id="b159f-188">Ensure the policy names are exactly as described in the documentation, as those policies were used in the **Change Authentication** dialog in Visual Studio.</span></span> <span data-ttu-id="b159f-189">ポリシー名を検証できます*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="b159f-189">The policy names can be verified in *appsettings.json*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="b159f-190">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="b159f-190">Run the app</span></span>

<span data-ttu-id="b159f-191">Visual Studio で、キーを押して**f5 キーを押して**アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="b159f-191">In Visual Studio, press **F5** to build and run the app.</span></span> <span data-ttu-id="b159f-192">Web アプリが起動後、選択**サインイン**です。</span><span class="sxs-lookup"><span data-stu-id="b159f-192">After the web app launches, select **Sign in**.</span></span>

![アプリへのサインインします。](./azure-ad-b2c/_static/signin.png)

<span data-ttu-id="b159f-194">ブラウザーは、Azure AD B2C テナントにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="b159f-194">The browser redirects to the Azure AD B2C tenant.</span></span> <span data-ttu-id="b159f-195">(1 つには、ポリシーのテストが作成された) 場合は、既存のアカウントでサインインまたは選択**今すぐサインアップ**新しいアカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="b159f-195">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="b159f-196">**パスワードを忘れた場合ですか?** 忘れてもパスワードをリセットするリンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="b159f-196">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

![Azure AD B2C ログイン](./azure-ad-b2c/_static/b2csts.png)

<span data-ttu-id="b159f-198">正常にサインインした後は、ブラウザーは、web アプリにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="b159f-198">After successfully signing in, the browser redirects to the web app.</span></span>

![成功](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a><span data-ttu-id="b159f-200">次の手順</span><span class="sxs-lookup"><span data-stu-id="b159f-200">Next steps</span></span>

<span data-ttu-id="b159f-201">このチュートリアルでは、次の作業を行う方法を学びました。</span><span class="sxs-lookup"><span data-stu-id="b159f-201">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b159f-202">Azure Active Directory B2C テナントを作成します。</span><span class="sxs-lookup"><span data-stu-id="b159f-202">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="b159f-203">Azure AD B2C にアプリを登録します。</span><span class="sxs-lookup"><span data-stu-id="b159f-203">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="b159f-204">Visual Studio を使用して Azure AD B2C テナント認証を使用するように構成 ASP.NET Core Web アプリケーションを作成するには</span><span class="sxs-lookup"><span data-stu-id="b159f-204">Use Visual Studio to create an ASP.NET Core Web Application configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="b159f-205">Azure AD B2C テナントの動作を制御するポリシーを構成します。</span><span class="sxs-lookup"><span data-stu-id="b159f-205">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

<span data-ttu-id="b159f-206">ASP.NET Core アプリケーションが認証に使用する Azure AD B2C、構成されていること、 [Authorize attribute](xref:security/authorization/simple)アプリをセキュリティで保護するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="b159f-206">Now that the ASP.NET Core app is configured to use Azure AD B2C for authentication, the [Authorize attribute](xref:security/authorization/simple) can be used to secure your app.</span></span> <span data-ttu-id="b159f-207">学習することにより、アプリケーションの開発を続行するには。</span><span class="sxs-lookup"><span data-stu-id="b159f-207">Continue developing your app by learning to:</span></span>

* <span data-ttu-id="b159f-208">[Azure AD B2C のユーザー インターフェイスをカスタマイズする](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)です。</span><span class="sxs-lookup"><span data-stu-id="b159f-208">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="b159f-209">[パスワードの複雑さの要件を構成する](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)です。</span><span class="sxs-lookup"><span data-stu-id="b159f-209">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="b159f-210">[多要素認証を有効にする](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。</span><span class="sxs-lookup"><span data-stu-id="b159f-210">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="b159f-211">など、追加の id プロバイダーを構成[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)、 [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)、 [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)、 [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)、 [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)、およびその他。</span><span class="sxs-lookup"><span data-stu-id="b159f-211">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="b159f-212">[Azure AD グラフ API を使用して](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)Azure AD B2C テナントから、グループ メンバーシップなどの追加のユーザー情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="b159f-212">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>
* <span data-ttu-id="b159f-213">[Azure AD B2C を使用して web API を ASP.NET のコア セキュリティで保護された](xref:security/authentication/azure-ad-b2c-webapi)です。</span><span class="sxs-lookup"><span data-stu-id="b159f-213">[Secure an ASP.NET Core web API using Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).</span></span>
* <span data-ttu-id="b159f-214">[Azure AD B2C を使用して .NET web アプリケーションから .NET web API を呼び出す](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="b159f-214">[Call a .NET web API from a .NET web app using Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span></span>
