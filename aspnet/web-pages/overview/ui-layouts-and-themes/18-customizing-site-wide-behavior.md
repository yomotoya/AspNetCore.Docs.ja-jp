---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: ASP.NET Web Pages (Razor) サイトのサイト全体の動作のカスタマイズ |Microsoft Docs
author: tfitzmac
description: この章では、ページだけではなく、web サイト全体または、フォルダー全体を設定する方法について説明します。
ms.author: riande
ms.date: 02/17/2014
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: a6737c05d3326a2cd7535f32e99482b0de394575
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828103"
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a><span data-ttu-id="0d1ba-103">ASP.NET Web Pages (Razor) サイトのサイト全体の動作をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-103">Customizing Site-Wide Behavior for ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="0d1ba-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0d1ba-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0d1ba-105">この記事では、ASP.NET Web Pages (Razor) の web サイトでページのサイト側設定する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-105">This article explains how to make site-side settings for pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="0d1ba-106">学習内容。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="0d1ba-107">コードを実行できるようにする方法は、サイト内のすべてのページの (グローバル値またはヘルパーの設定) が値セット。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-107">How to run code that lets you set values (global values or helper settings) for all pages in a site.</span></span>
> - <span data-ttu-id="0d1ba-108">コード フォルダー内のすべてのページの値を設定することができますを実行する方法。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-108">How to run code that lets you set values for all pages in a folder.</span></span>
> - <span data-ttu-id="0d1ba-109">ページの前後にコードを実行する方法を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-109">How to run code before and after a page loads.</span></span>
> - <span data-ttu-id="0d1ba-110">サーバーの全体のエラー ページにエラーを送信する方法。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-110">How to send errors to a central error page.</span></span>
> - <span data-ttu-id="0d1ba-111">フォルダー内のすべてのページに認証を追加する方法。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-111">How to add authentication to all pages in a folder.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0d1ba-112">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="0d1ba-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="0d1ba-113">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="0d1ba-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="0d1ba-114">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="0d1ba-114">WebMatrix 3</span></span>
> - <span data-ttu-id="0d1ba-115">ASP.NET Web Helpers Library (NuGet パッケージ)</span><span class="sxs-lookup"><span data-stu-id="0d1ba-115">ASP.NET Web Helpers Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="0d1ba-116">また、このチュートリアルは ASP.NET Web ページ 3 と連携し、Visual Studio 2013 (または Visual Studio Express 2013 for Web)、こと以外は、ASP.NET Web Helpers Library を使用できません。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-116">This tutorial also works with ASP.NET Web Pages 3 and Visual Studio 2013 (or Visual Studio Express 2013 for Web), except you cannot use the ASP.NET Web Helpers Library.</span></span>


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a><span data-ttu-id="0d1ba-117">ASP.NET Web ページの web サイトのスタートアップ コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-117">Adding Website Startup Code for ASP.NET Web Pages</span></span>

<span data-ttu-id="0d1ba-118">ASP.NET Web ページを記述するコードの大部分には、個々 のページは、そのページに必要なすべてのコードを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-118">For much of the code that you write in ASP.NET Web Pages, an individual page can contain all the code that's required for that page.</span></span> <span data-ttu-id="0d1ba-119">たとえば、ページは、電子メール メッセージを送信する場合、1 つのページにその操作のすべてのコードを配置することは。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-119">For example, if a page sends an email message, it's possible to put all the code for that operation in a single page.</span></span> <span data-ttu-id="0d1ba-120">これは、電子メールを送信するための設定を初期化するためにコードを含めることができます (つまり、SMTP サーバーの) と、電子メール メッセージを送信するためです。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-120">This can include the code to initialize the settings for sending email (that is, for the SMTP server) and for sending the email message.</span></span>

<span data-ttu-id="0d1ba-121">ただし、状況によっては、任意のページで実行する前に、いくつかのコードを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-121">However, in some situations, you might want to run some code before any page on the site runs.</span></span> <span data-ttu-id="0d1ba-122">これは、サイト内の任意の場所で使用できる値を設定するために役立ちます (と呼ばれる*グローバル値*)。たとえば、いくつかのヘルパーには、電子メールの設定またはアカウント キーのような値を指定することが必要です。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-122">This is useful for setting values that can be used anywhere in the site (referred to as *global values*.) For example, some helpers require you to provide values like email settings or account keys.</span></span> <span data-ttu-id="0d1ba-123">グローバル値にこれらの設定を保持する便利なことができます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-123">It can be handy to keep these settings in global values.</span></span>

<span data-ttu-id="0d1ba-124">という名前のページを作成してこれを行う *\_AppStart.cshtml*サイトのルートにします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-124">You can do this by creating a page named *\_AppStart.cshtml* in the root of the site.</span></span> <span data-ttu-id="0d1ba-125">このページが存在する場合は、初めてサイト内の任意のページが要求を実行します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-125">If this page exists, it runs the first time any page in the site is requested.</span></span> <span data-ttu-id="0d1ba-126">そのため、これはグローバル値を設定するコードを実行することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-126">Therefore, it's a good place to run code to set global values.</span></span> <span data-ttu-id="0d1ba-127">(ため *\_AppStart.cshtml*アンダー スコア プレフィックスでは、ASP.NET は、ユーザーは、それを直接要求した場合でもに、ブラウザーのページを送信しません)。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-127">(Because *\_AppStart.cshtml* has an underscore prefix, ASP.NET won't send the page to a browser even if users request it directly.)</span></span>

<span data-ttu-id="0d1ba-128">次の図は、どのように *\_AppStart.cshtml* works ページします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-128">The following diagram shows how the *\_AppStart.cshtml* page works.</span></span> <span data-ttu-id="0d1ba-129">ページの要求を受信し、場合、これは、いずれかの最初の要求 ページ、サイトでは、ASP.NET で最初に確認かどうかを *\_AppStart.cshtml*ページが存在します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-129">When a request comes in for a page, and if this is the first request for any page in the site, ASP.NET first checks whether a *\_AppStart.cshtml* page exists.</span></span> <span data-ttu-id="0d1ba-130">内の場合は、コード、  *\_AppStart.cshtml*ページを実行し、要求されたページを実行します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-130">If so, any code in the *\_AppStart.cshtml* page runs, and then the requested page runs.</span></span>

![[イメージ]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a><span data-ttu-id="0d1ba-132">Web サイトのグローバル値の設定</span><span class="sxs-lookup"><span data-stu-id="0d1ba-132">Setting Global Values for Your Website</span></span>

1. <span data-ttu-id="0d1ba-133">WebMatrix web サイトのルート フォルダー、という名前のファイルを作成 *\_AppStart.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-133">In the root folder of a WebMatrix website, create a file named *\_AppStart.cshtml*.</span></span> <span data-ttu-id="0d1ba-134">ファイルは、サイトのルートでなければなりません。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-134">The file must be in the root of the site.</span></span>
2. <span data-ttu-id="0d1ba-135">次のように、既存のコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-135">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    <span data-ttu-id="0d1ba-136">このコードで値を格納する、`AppState`サイト内のすべてのページに自動的に設定されているディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-136">This code stores a value in the `AppState` dictionary, which is automatically available to all pages in the site.</span></span> <span data-ttu-id="0d1ba-137">なお、  *\_AppStart.cshtml*ファイルでは、これでは、すべてのマークアップはありません。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-137">Notice that the *\_AppStart.cshtml* file does not have any markup in it.</span></span> <span data-ttu-id="0d1ba-138">ページはコードを実行し、最初に要求されたページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-138">The page will run the code and then redirect to the page that was originally requested.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0d1ba-139">コードを配置すると、注意、  *\_AppStart.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-139">Be careful when you put code in the *\_AppStart.cshtml* file.</span></span> <span data-ttu-id="0d1ba-140">コード内でエラーが発生した場合、  *\_AppStart.cshtml*ファイル、web サイトを開始しません。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-140">If any errors occur in code in the *\_AppStart.cshtml* file, the website won't start.</span></span>
3. <span data-ttu-id="0d1ba-141">ルート フォルダーに作成するという名前の新しいページ*AppName.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-141">In the root folder, create a new page named *AppName.cshtml*.</span></span>
4. <span data-ttu-id="0d1ba-142">次のように、既定のマークアップとコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-142">Replace the default markup and code with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    <span data-ttu-id="0d1ba-143">このコードから値を抽出し、`AppState`で設定するオブジェクト、  *\_AppStart.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-143">This code extracts the value from the `AppState` object that you set in the *\_AppStart.cshtml* page.</span></span>
5. <span data-ttu-id="0d1ba-144">実行、 *AppName.cshtml*ブラウザーでページ。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-144">Run the *AppName.cshtml* page in a browser.</span></span> <span data-ttu-id="0d1ba-145">(内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。ページには、グローバルな値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-145">(Make sure the page is selected in the **Files** workspace before you run it.) The page displays the global value.</span></span> 

    ![[イメージ]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a><span data-ttu-id="0d1ba-147">ヘルパーの値の設定</span><span class="sxs-lookup"><span data-stu-id="0d1ba-147">Setting Values for Helpers</span></span>

<span data-ttu-id="0d1ba-148">適切な使い方、  *\_AppStart.cshtml*ファイルは、サイトで使用して、初期化する必要があるヘルパーの値を設定します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-148">A good use for the *\_AppStart.cshtml* file is to set values for helpers that you use in your site and that have to be initialized.</span></span> <span data-ttu-id="0d1ba-149">一般的な例としては、の電子メール設定、`WebMail`ヘルパーとのプライベートおよびパブリック キー、`ReCaptcha`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-149">Typical examples are email settings for the `WebMail` helper and the private and public keys for the `ReCaptcha` helper.</span></span> <span data-ttu-id="0d1ba-150">上記のような場合で 1 回の値を設定することができます、  *\_AppStart.cshtml*し、いる既にすべてのページで設定、サイト。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-150">In cases like these, you can set the values once in the *\_AppStart.cshtml* and then they're already set for all the pages in your site.</span></span>

<span data-ttu-id="0d1ba-151">この手順は、設定する方法を示します`WebMail`設定グローバルにします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-151">This procedure shows you how to set `WebMail` settings globally.</span></span> <span data-ttu-id="0d1ba-152">(使用の詳細については、`WebMail`ヘルパーを参照してください[ASP.NET Web ページ サイトを追加する電子メール](../getting-started/11-adding-email-to-your-web-site.md))。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-152">(For more information about using the `WebMail` helper, see [Adding Email to an ASP.NET Web Pages Site](../getting-started/11-adding-email-to-your-web-site.md).)</span></span>

1. <span data-ttu-id="0d1ba-153">」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)、既に追加していない場合。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-153">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="0d1ba-154">まだ持っていない場合、  *\_AppStart.cshtml*という名前のファイルを作成、web サイトのルート フォルダーにファイルを *\_AppStart.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-154">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
3. <span data-ttu-id="0d1ba-155">次の追加`WebMail`設定を *\_AppStart.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-155">Add the following `WebMail` settings to the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    <span data-ttu-id="0d1ba-156">変更、次のコードに関連する設定を電子メールで送信します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-156">Modify the following email related settings in the code:</span></span>

   - <span data-ttu-id="0d1ba-157">設定`your-SMTP-host`へのアクセスが SMTP サーバーの名前にします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-157">Set `your-SMTP-host` to the name of the SMTP server that you have access to.</span></span>
   - <span data-ttu-id="0d1ba-158">設定`your-user-name-here`SMTP サーバー アカウントのユーザー名にします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-158">Set `your-user-name-here` to the user name for your SMTP server account.</span></span>
   - <span data-ttu-id="0d1ba-159">設定`your-account-password`SMTP サーバー アカウントのパスワードにします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-159">Set `your-account-password` to the password for your SMTP server account.</span></span>
   - <span data-ttu-id="0d1ba-160">設定`your-email-address-here`を自分の電子メール アドレス。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-160">Set `your-email-address-here` to your own email address.</span></span> <span data-ttu-id="0d1ba-161">これは、メッセージの送信元電子メール アドレスです。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-161">This is the email address that the message is sent from.</span></span> <span data-ttu-id="0d1ba-162">(一部の電子メール プロバイダーから、別の指定は、`From`アドレスし、としてユーザー名を使用、`From`アドレスです)。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-162">(Some email providers don't let you specify a different `From` address and will use your user name as the `From` address.)</span></span>

     <span data-ttu-id="0d1ba-163">SMTP 設定の詳細については、次を参照してください[電子メール設定を構成する](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings)記事[ASP.NET Web Pages (Razor) サイトから電子メールを送信する](https://go.microsoft.com/fwlink/?LinkID=202899)と[メールの送信問題](https://go.microsoft.com/fwlink/?LinkId=253001#email)。で、 [ASP.NET Web Pages (Razor) トラブルシューティング ガイド](https://go.microsoft.com/fwlink/?LinkId=253001)します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-163">For more information about SMTP settings, see [Configuring Email Settings](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) in the article [Sending Email from an ASP.NET Web Pages (Razor) Site](https://go.microsoft.com/fwlink/?LinkID=202899) and [Issues with Sending Email](https://go.microsoft.com/fwlink/?LinkId=253001#email) in the [ASP.NET Web Pages (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).</span></span>
4. <span data-ttu-id="0d1ba-164">保存、  *\_AppStart.cshtml*ファイルして閉じます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-164">Save the *\_AppStart.cshtml* file and close it.</span></span>
5. <span data-ttu-id="0d1ba-165">Web サイトのルート フォルダーに作成するという名前の新しいページ*TestEmail.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-165">In the root folder of a website, create new page named *TestEmail.cshtml*.</span></span>
6. <span data-ttu-id="0d1ba-166">次のように、既存のコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-166">Replace the existing content with the following:</span></span> 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. <span data-ttu-id="0d1ba-167">実行、 *TestEmail.cshtml*ブラウザーでページ。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-167">Run the *TestEmail.cshtml* page in a browser.</span></span>
8. <span data-ttu-id="0d1ba-168">自分宛てに電子メール メッセージを送信し、フィールドに入力**送信**します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-168">Fill in the fields to send yourself an email message and then click **Send**.</span></span>
9. <span data-ttu-id="0d1ba-169">メッセージを取得したかどうかを確認する電子メールを確認します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-169">Check your email to make sure you've gotten the message.</span></span>

<span data-ttu-id="0d1ba-170">この例の重要な部分は通常は変更しない設定 — などの SMTP サーバーと電子メールの資格情報の名前: で設定されて、  *\_AppStart.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-170">The important part of this example is that the settings that you don't usually change — like the name of your SMTP server and your email credentials — are set in the *\_AppStart.cshtml* file.</span></span> <span data-ttu-id="0d1ba-171">これにより、再度設定する各ページで電子メールを送信する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-171">That way you don't need to set them again in each page where you send email.</span></span> <span data-ttu-id="0d1ba-172">(が何らかの理由は、これらの設定を変更する必要が場合を設定できますに個別にページにします。)ページで、通常は毎回、受信者、電子メール メッセージの本文などを変更する値を設定するだけです。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-172">(Although if for some reason you need to change those settings, you can set them individually in a page.) In the page, you only set the values that typically change each time, like the recipient and the body of the email message.</span></span>

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a><span data-ttu-id="0d1ba-173">フォルダー内のファイルの前後にコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-173">Running Code Before and After Files in a Folder</span></span>

<span data-ttu-id="0d1ba-174">同様に使用できること *\_AppStart.cshtml*前に、(と後) で実行されるコードを記述するコードを記述する、サイト内のページの実行前に、実行の特定のフォルダー内の任意のページ。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-174">Just like you can use *\_AppStart.cshtml* to write code before pages in the site run, you can write code that runs before (and after) any page in a particular folder run.</span></span> <span data-ttu-id="0d1ba-175">これは、同じレイアウト ページ、フォルダー内のすべてのページまたは確認するための中、フォルダー内のページを実行する前に、ユーザーがログインする設定などに便利です。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-175">This is useful for things like setting the same layout page for all the pages in a folder, or for checking that a user is logged in before running a page in the folder.</span></span>

<span data-ttu-id="0d1ba-176">ページの具体的にはフォルダー、コードを作成できますという名前のファイルで *\_PageStart.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-176">For pages in particular folders, you can create code in a file named *\_PageStart.cshtml*.</span></span> <span data-ttu-id="0d1ba-177">次の図は、どのように *\_PageStart.cshtml* works ページします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-177">The following diagram shows how the *\_PageStart.cshtml* page works.</span></span> <span data-ttu-id="0d1ba-178">ASP.NET が最初のチェック、ページの要求を受信すると、  *\_AppStart.cshtml*ページしを実行します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-178">When a request comes in for a page, ASP.NET first checks for a *\_AppStart.cshtml* page and runs that.</span></span> <span data-ttu-id="0d1ba-179">ASP.NET があるかどうかをチェックし、  *\_PageStart.cshtml*ページ、および場合を実行します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-179">Then ASP.NET checks whether there's a *\_PageStart.cshtml* page, and if so, runs that.</span></span> <span data-ttu-id="0d1ba-180">これは、後、要求されたページを実行します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-180">It then runs the requested page.</span></span>

<span data-ttu-id="0d1ba-181">内で、  *\_PageStart.cshtml*  ページで、場所を指定するなどして実行する、要求されたページの処理中に、`RunPage`メソッド。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-181">Inside the *\_PageStart.cshtml* page, you can specify where during processing you want the requested page to run by including a `RunPage` method.</span></span> <span data-ttu-id="0d1ba-182">これにより、要求されたページの実行前に、コードを実行できますし、その後にもう一度です。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-182">This lets you run code before the requested page runs and then again after it.</span></span> <span data-ttu-id="0d1ba-183">含めない場合`RunPage`、すべてのコードで *\_PageStart.cshtml*実行してから、要求されたページが自動的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-183">If you don't include `RunPage`, all the code in *\_PageStart.cshtml* runs, and then the requested page runs automatically.</span></span>

![[イメージ]](18-customizing-site-wide-behavior/_static/image3.jpg)

<span data-ttu-id="0d1ba-185">ASP.NET では、階層を作成できます。  *\_PageStart.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-185">ASP.NET lets you create a hierarchy of *\_PageStart.cshtml* files.</span></span> <span data-ttu-id="0d1ba-186">配置することができます、  *\_PageStart.cshtml*ファイルとサブフォルダー、サイトのルートにします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-186">You can put a *\_PageStart.cshtml* file in the root of the site and in any subfolder.</span></span> <span data-ttu-id="0d1ba-187">ページが要求されたときに、  *\_PageStart.cshtml*に続けて、最上位レベル (サイトのルート) に最も近い実行ファイル、  *\_PageStart.cshtml*次のファイルサブフォルダー、サブフォルダーの構造を要求されたページを含むフォルダーに到達するまでの下位にあります。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-187">When a page is requested, the *\_PageStart.cshtml* file at the top-most level (nearest to the site root) runs, followed by the *\_PageStart.cshtml* file in the next subfolder, and so on down the subfolder structure until the request reaches the folder that contains the requested page.</span></span> <span data-ttu-id="0d1ba-188">すべての適用後 *\_PageStart.cshtml*ファイルを実行、要求されたページが実行されます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-188">After all the applicable *\_PageStart.cshtml* files have run, the requested page runs.</span></span>

<span data-ttu-id="0d1ba-189">たとえばの次の組み合わせがある *\_PageStart.cshtml*ファイルと*Default.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-189">For example, you might have the following combination of *\_PageStart.cshtml* files and *Default.cshtml* file:</span></span>

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

<span data-ttu-id="0d1ba-190">実行すると */myfolder/default.cshtml*次を確認します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-190">When you run */myfolder/default.cshtml*, you'll see the following:</span></span>

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a><span data-ttu-id="0d1ba-191">フォルダー内のすべてのページの初期化コードを実行しています。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-191">Running Initialization Code for All Pages in a Folder</span></span>

<span data-ttu-id="0d1ba-192">良い使用 *\_PageStart.cshtml*ファイルは、1 つのフォルダーですべてのファイルと同じレイアウト ページを初期化します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-192">A good use for *\_PageStart.cshtml* files is to initialize the same layout page for all files in a single folder.</span></span>

1. <span data-ttu-id="0d1ba-193">という名前の新しいフォルダーを作成、ルート フォルダーで*InitPages*します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-193">In the root folder, create a new folder named *InitPages*.</span></span>
2. <span data-ttu-id="0d1ba-194">*InitPages*という名前のファイルを作成、web サイトのフォルダー  *\_PageStart.cshtml*次の既定のマークアップとコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-194">In the *InitPages* folder of your website, create a file named *\_PageStart.cshtml* and replace the default markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. <span data-ttu-id="0d1ba-195">という名前のフォルダーを作成、web サイトのルートに*Shared*します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-195">In the root of the website, create a folder named *Shared*.</span></span>
4. <span data-ttu-id="0d1ba-196">*Shared*フォルダー、という名前のファイルを作成する *\_Layout1.cshtml*次の既定のマークアップとコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-196">In the *Shared* folder, create a file named *\_Layout1.cshtml* and replace the default markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. <span data-ttu-id="0d1ba-197">*InitPages*フォルダー、という名前のファイルを作成する*Content1.cshtml*次の既存のコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-197">In the *InitPages* folder, create a file named *Content1.cshtml* and replace the existing content with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. <span data-ttu-id="0d1ba-198">*InitPages*フォルダー、という別のファイルを作成する*Content2.cshtml*次の既定のマークアップを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-198">In the *InitPages* folder, create another file named *Content2.cshtml* and replace the default markup with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. <span data-ttu-id="0d1ba-199">実行*Content1.cshtml*ブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-199">Run *Content1.cshtml* in a browser.</span></span> 

    ![[イメージ]](18-customizing-site-wide-behavior/_static/image4.jpg)

    <span data-ttu-id="0d1ba-201">ときに、 *Content1.cshtml*ページの実行、  *\_PageStart.cshtml*ファイルのセット`Layout`され、また`PageData["MyBackground"]`色にします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-201">When the *Content1.cshtml* page runs, the *\_PageStart.cshtml* file sets `Layout` and also sets `PageData["MyBackground"]` to a color.</span></span> <span data-ttu-id="0d1ba-202">*Content1.cshtml*レイアウトと色が適用されます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-202">In *Content1.cshtml*, the layout and color are applied.</span></span>
8. <span data-ttu-id="0d1ba-203">表示*Content2.cshtml*ブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-203">Display *Content2.cshtml* in a browser.</span></span> 

    <span data-ttu-id="0d1ba-204">レイアウトは同じですが、両方のページを使用して、同じページのレイアウトと色で初期化するため *\_PageStart.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-204">The layout is the same, because both pages use the same layout page and color as initialized in *\_PageStart.cshtml*.</span></span>

## <a name="using-pagestartcshtml-to-handle-errors"></a><span data-ttu-id="0d1ba-205">使用して\_PageStart.cshtml エラーを処理するには</span><span class="sxs-lookup"><span data-stu-id="0d1ba-205">Using \_PageStart.cshtml to Handle Errors</span></span>

<span data-ttu-id="0d1ba-206">別なを使用して、  *\_PageStart.cshtml*ファイルは、いずれかで発生するプログラミング エラー (例外) を処理する方法を作成する *.cshtml*フォルダー内のページ。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-206">Another good use for the *\_PageStart.cshtml* file is to create a way to handle programming errors (exceptions) that might occur in any *.cshtml* page in a folder.</span></span> <span data-ttu-id="0d1ba-207">この例では、これを行う 1 つの方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-207">This example shows you one way to do this.</span></span>

1. <span data-ttu-id="0d1ba-208">という名前のフォルダーを作成、ルート フォルダーで*InitCatch*します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-208">In the root folder, create a folder named *InitCatch*.</span></span>
2. <span data-ttu-id="0d1ba-209">*InitCatch*という名前のファイルを作成、web サイトのフォルダー  *\_PageStart.cshtml*次の既存のマークアップとコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-209">In the *InitCatch* folder of your website, create a file named *\_PageStart.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    <span data-ttu-id="0d1ba-210">このコードで指定して実行する、要求されたページに明示的に呼び出すことによって、`RunPage`メソッド内で、`try`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-210">In this code, you try running the requested page explicitly by calling the `RunPage` method inside a `try` block.</span></span> <span data-ttu-id="0d1ba-211">要求された任意のプログラミング エラーが発生した場合のページの内部のコード、`catch`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-211">If any programming errors occur in the requested page, the code inside the `catch` block runs.</span></span> <span data-ttu-id="0d1ba-212">コードをページにリダイレクトするこの例では、(*Error.cshtml*) し、URL の一部として、エラーが発生したファイルの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-212">In this case, the code redirects to a page (*Error.cshtml*) and passes the name of the file that experienced the error as part of the URL.</span></span> <span data-ttu-id="0d1ba-213">(ページを作成します、間もなく。)</span><span class="sxs-lookup"><span data-stu-id="0d1ba-213">(You'll create the page shortly.)</span></span>
3. <span data-ttu-id="0d1ba-214">*InitCatch*という名前のファイルを作成、web サイトのフォルダー *Exception.cshtml*次の既存のマークアップとコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-214">In the *InitCatch* folder of your website, create a file named *Exception.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    <span data-ttu-id="0d1ba-215">この例のために、このページで何をしているが意図的に作成エラーが存在しないデータベース ファイルを開こうとしました。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-215">For purposes of this example, what you're doing in this page is deliberately creating an error by trying to open a database file that doesn't exist.</span></span>
4. <span data-ttu-id="0d1ba-216">という名前のファイルを作成、ルート フォルダーで*Error.cshtml*次の既存のマークアップとコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-216">In the root folder, create a file named *Error.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    <span data-ttu-id="0d1ba-217">このページでは、式で`@Request["source"]`URL から値を取得し、それを表示します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-217">In this page, the expression `@Request["source"]` gets the value out of the URL and displays it.</span></span>
5. <span data-ttu-id="0d1ba-218">ツールバーで、次のようにクリックします。**保存**します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-218">In the toolbar, click **Save**.</span></span>
6. <span data-ttu-id="0d1ba-219">実行*Exception.cshtml*ブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-219">Run *Exception.cshtml* in a browser.</span></span> 

    ![[イメージ]](18-customizing-site-wide-behavior/_static/image5.jpg)

    <span data-ttu-id="0d1ba-221">エラーが発生したため、 *Exception.cshtml*、  *\_PageStart.cshtml*ページにリダイレクト、 *Error.cshtml*ファイルで、メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-221">Because an error occurs in *Exception.cshtml*, the *\_PageStart.cshtml* page redirects to the *Error.cshtml* file, which displays the message.</span></span>

    <span data-ttu-id="0d1ba-222">例外の詳細については、次を参照してください。 [ASP.NET Web ページは、Razor 構文を使用プログラミングの概要について](https://go.microsoft.com/fwlink/?LinkID=251587)します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-222">For more information about exceptions, see [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkID=251587).</span></span>

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a><span data-ttu-id="0d1ba-223">使用して\_PageStart.cshtml フォルダーへのアクセスを制限するには</span><span class="sxs-lookup"><span data-stu-id="0d1ba-223">Using \_PageStart.cshtml to Restrict Folder Access</span></span>

<span data-ttu-id="0d1ba-224">使用することも、  *\_PageStart.cshtml*フォルダー内のすべてのファイルへのアクセスを制限するファイル。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-224">You can also use the *\_PageStart.cshtml* file to restrict access to all the files in a folder.</span></span>

1. <span data-ttu-id="0d1ba-225">WebMatrix で、使用して新しい web サイトを作成、**テンプレートからサイト**オプション。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-225">In WebMatrix, create a new website using the **Site From Template** option.</span></span>
2. <span data-ttu-id="0d1ba-226">使用可能なテンプレートから次のように選択します。**スターター サイト**します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-226">From the available templates, select **Starter Site**.</span></span>
3. <span data-ttu-id="0d1ba-227">という名前のフォルダーを作成、ルート フォルダーで*AuthenticatedContent*します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-227">In the root folder, create a folder named *AuthenticatedContent*.</span></span>
4. <span data-ttu-id="0d1ba-228">*AuthenticatedContent*フォルダー、という名前のファイルを作成する *\_PageStart.cshtml*次の既存のマークアップとコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-228">In the *AuthenticatedContent* folder, create a file named *\_PageStart.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    <span data-ttu-id="0d1ba-229">コードは、フォルダー内のすべてのファイルがキャッシュされないようにすることで開始します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-229">The code starts by preventing all files in the folder from being cached.</span></span> <span data-ttu-id="0d1ba-230">(これは、次のユーザーが利用できる 1 つのユーザーのキャッシュされたページをたくない公共のコンピューターのようなシナリオに必要)。次に、コードでは、ユーザーがサイトにサインイン済みで、フォルダー内の各ページを表示するかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-230">(This is required for scenarios like public computers, where you don't want one user's cached pages to be available to the next user.) Next, the code determines whether the user has signed in to the site before they can view any of the pages in the folder.</span></span> <span data-ttu-id="0d1ba-231">ユーザーがサインインされていない場合、コードは、ログイン ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-231">If the user is not signed in, the code redirects to the login page.</span></span> <span data-ttu-id="0d1ba-232">ログイン ページにユーザーという名前のクエリ文字列値を含める場合は最初に要求されたページを返すことができます`ReturnUrl`します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-232">The login page can return the user to the page that was originally requested if you include a query string value named `ReturnUrl`.</span></span>
5. <span data-ttu-id="0d1ba-233">新しいページを作成、 *AuthenticatedContent*という名前のフォルダー *Page.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-233">Create a new page in the *AuthenticatedContent* folder named *Page.cshtml*.</span></span>
6. <span data-ttu-id="0d1ba-234">次のように、既定のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-234">Replace the default markup with the following:</span></span>  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. <span data-ttu-id="0d1ba-235">実行*Page.cshtml*ブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-235">Run *Page.cshtml* in a browser.</span></span> <span data-ttu-id="0d1ba-236">コードでは、ログイン ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-236">The code redirects you to a login page.</span></span> <span data-ttu-id="0d1ba-237">ログインする前に登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-237">You must register before logging in.</span></span> <span data-ttu-id="0d1ba-238">登録し、ログインして後、は、ページに移動し、その内容を表示します。</span><span class="sxs-lookup"><span data-stu-id="0d1ba-238">After you've registered and logged in, you can navigate to the page and view its contents.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="0d1ba-239">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="0d1ba-239">Additional Resources</span></span>

[<span data-ttu-id="0d1ba-240">ASP.NET Web ページの Razor 構文を使用したプログラミングの概要</span><span class="sxs-lookup"><span data-stu-id="0d1ba-240">Introduction to ASP.NET Web Pages Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=251587)
