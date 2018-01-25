---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: "ページ (Razor) サイトの ASP.NET Web から電子メールを送信する |Microsoft ドキュメント"
author: tfitzmac
description: "この章では、web サイトから自動化された電子メール メッセージを送信する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: c5878c3bc468daef050dcebee99f64441066409a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="1c102-103">ASP.NET Web Pages (Razor) サイトから電子メールを送信します。</span><span class="sxs-lookup"><span data-stu-id="1c102-103">Sending Email from an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="1c102-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1c102-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1c102-105">この記事では、ASP.NET Web Pages (Razor) を使用すると、web サイトから電子メール メッセージを送信する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1c102-105">This article explains how to send an email message from a website when you use ASP.NET Web Pages (Razor).</span></span>
> 
> <span data-ttu-id="1c102-106">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="1c102-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="1c102-107">Web サイトから電子メール メッセージを送信する方法です。</span><span class="sxs-lookup"><span data-stu-id="1c102-107">How to send an email message from your website.</span></span>
> - <span data-ttu-id="1c102-108">電子メール メッセージにファイルを添付する方法。</span><span class="sxs-lookup"><span data-stu-id="1c102-108">How to attach a file to an email message.</span></span>
> 
> <span data-ttu-id="1c102-109">これは、アーティクルで導入された ASP.NET の機能です。</span><span class="sxs-lookup"><span data-stu-id="1c102-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="1c102-110">`WebMail`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="1c102-110">The `WebMail` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1c102-111">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="1c102-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1c102-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="1c102-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="1c102-113">このチュートリアルは、ASP.NET Web Pages 2 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="1c102-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a><span data-ttu-id="1c102-114">Web サイトからの電子メール メッセージの送信</span><span class="sxs-lookup"><span data-stu-id="1c102-114">Sending Email Messages from Your Website</span></span>

<span data-ttu-id="1c102-115">あらゆる種類の web サイトから電子メールを送信しなければならない理由があります。</span><span class="sxs-lookup"><span data-stu-id="1c102-115">There are all sorts of reasons why you might need to send email from your website.</span></span> <span data-ttu-id="1c102-116">ユーザーに確認メッセージを送信する可能性があります。 か (たとえば、新しいユーザーが登録されている)。 自分宛てに通知を送信する可能性があります。`WebMail`ヘルパーを簡単にすると、電子メールを送信します。</span><span class="sxs-lookup"><span data-stu-id="1c102-116">You might send confirmation messages to users, or you might send notifications to yourself (for example, that a new user has registered.) The `WebMail` helper makes it easy for you to send email.</span></span>

<span data-ttu-id="1c102-117">使用する、`WebMail`ヘルパーに渡し、SMTP サーバーにアクセスする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c102-117">To use the `WebMail` helper, you have to have access to an SMTP server.</span></span> <span data-ttu-id="1c102-118">(は、SMTP *Simple Mail Transfer Protocol*)。SMTP サーバーは、メール サーバーのみメッセージを受信者のサーバー &#8212; を転送します。電子メールの送信側です。</span><span class="sxs-lookup"><span data-stu-id="1c102-118">(SMTP stands for *Simple Mail Transfer Protocol*.) An SMTP server is an email server that only forwards messages to the recipient's server &#8212; it's the outbound side of email.</span></span> <span data-ttu-id="1c102-119">Web サイトのホスティング プロバイダーを使用する場合、おそらくセットアップした電子メールとできます教えてくれる SMTP サーバー名とは何です。</span><span class="sxs-lookup"><span data-stu-id="1c102-119">If you use a hosting provider for your website, they probably set you up with email and they can tell you what your SMTP server name is.</span></span> <span data-ttu-id="1c102-120">企業ネットワーク内、使用している場合、管理者または IT 部門通常得られますに使用できる SMTP サーバーに関する情報。</span><span class="sxs-lookup"><span data-stu-id="1c102-120">If you're working inside a corporate network, an administrator or your IT department can usually give you the information about an SMTP server that you can use.</span></span> <span data-ttu-id="1c102-121">自宅で作業している場合でも、通常の電子メール プロバイダーに SMTP サーバーの名前を確認するユーザーを使用してテストすることできます可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1c102-121">If you're working at home, you might even be able to test using your ordinary email provider, who can tell you the name of their SMTP server.</span></span> <span data-ttu-id="1c102-122">通常必要です。</span><span class="sxs-lookup"><span data-stu-id="1c102-122">You typically need:</span></span>

- <span data-ttu-id="1c102-123">SMTP サーバーの名前です。</span><span class="sxs-lookup"><span data-stu-id="1c102-123">The name of the SMTP server.</span></span>
- <span data-ttu-id="1c102-124">ポート番号。</span><span class="sxs-lookup"><span data-stu-id="1c102-124">The port number.</span></span> <span data-ttu-id="1c102-125">これは、ほとんどの場合は 25 です。</span><span class="sxs-lookup"><span data-stu-id="1c102-125">This is almost always 25.</span></span> <span data-ttu-id="1c102-126">ただし、ISP ポート 587 を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c102-126">However, your ISP may require you to use port 587.</span></span> <span data-ttu-id="1c102-127">電子メールで安全な sockets layer (SSL) を使用している場合は、別のポートを必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c102-127">If you are using secure sockets layer (SSL) for email, you might need a different port.</span></span> <span data-ttu-id="1c102-128">電子メール プロバイダーに確認します。</span><span class="sxs-lookup"><span data-stu-id="1c102-128">Check with your email provider.</span></span>
- <span data-ttu-id="1c102-129">資格情報 (ユーザー名とパスワード)。</span><span class="sxs-lookup"><span data-stu-id="1c102-129">Credentials (user name, password).</span></span>

<span data-ttu-id="1c102-130">この手順では、2 つのページを作成します。</span><span class="sxs-lookup"><span data-stu-id="1c102-130">In this procedure, you create two pages.</span></span> <span data-ttu-id="1c102-131">最初のページには、入力できるように説明は、テクニカル サポート フォームに入力されている場合、形式があります。</span><span class="sxs-lookup"><span data-stu-id="1c102-131">The first page has a form that lets users enter a description, as if they were filling in a technical-support form.</span></span> <span data-ttu-id="1c102-132">最初のページでは、2 番目のページにその情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="1c102-132">The first page submits its information to a second page.</span></span> <span data-ttu-id="1c102-133">2 番目のページでは、コードは、ユーザーの情報を抽出し、電子メール メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="1c102-133">In the second page, code extracts the user's information and sends an email message.</span></span> <span data-ttu-id="1c102-134">問題のレポートが受信されたことを確認するメッセージも表示されます。</span><span class="sxs-lookup"><span data-stu-id="1c102-134">It also displays a message confirming that the problem report has been received.</span></span>

![[イメージ]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> <span data-ttu-id="1c102-136">この例を簡潔にするに、コードを初期化します、`WebMail`使用するページの右側にヘルパー。</span><span class="sxs-lookup"><span data-stu-id="1c102-136">To keep this example simple, the code initializes the `WebMail` helper right in the page where you use it.</span></span> <span data-ttu-id="1c102-137">ただし、実際の web サイトであるグローバルのファイルに次のように初期化コードを配置することをお勧めを初期化するように、 `WebMail` helper for web サイトのすべてのファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="1c102-137">However, for real websites, it's a better idea to put initialization code like this in a global file, so that you initialize the `WebMail` helper for all files in your website.</span></span> <span data-ttu-id="1c102-138">詳細については、次を参照してください。[サイト全体の動作をカスタマイズする ASP.NET Web Pages の](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers)します。</span><span class="sxs-lookup"><span data-stu-id="1c102-138">For more information, see [Customizing Site-Wide Behavior for ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).</span></span>


1. <span data-ttu-id="1c102-139">新しい web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="1c102-139">Create a new website.</span></span>
2. <span data-ttu-id="1c102-140">という名前の新しいページを追加*EmailRequest.cshtml*し、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="1c102-140">Add a new page named *EmailRequest.cshtml* and add the following markup:</span></span> 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    <span data-ttu-id="1c102-141">注意して、 `action` 、form 要素の属性設定されている*ProcessRequest.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="1c102-141">Notice that the `action` attribute of the form element has been set to *ProcessRequest.cshtml*.</span></span> <span data-ttu-id="1c102-142">これは、フォームを現在のページに戻るの代わりにそのページに送信することを意味します。</span><span class="sxs-lookup"><span data-stu-id="1c102-142">This means that the form will be submitted to that page instead of back to the current page.</span></span>
3. <span data-ttu-id="1c102-143">という名前の新しいページを追加*ProcessRequest.cshtml* web サイトにし、次のコードとマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="1c102-143">Add a new page named *ProcessRequest.cshtml* to the website and add the following code and markup:</span></span>   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    <span data-ttu-id="1c102-144">コードでは、ページに送信されたフォーム フィールドの値を取得します。</span><span class="sxs-lookup"><span data-stu-id="1c102-144">In the code, you get the values of the form fields that were submitted to the page.</span></span> <span data-ttu-id="1c102-145">次に呼び出し、`WebMail`ヘルパーの`Send`メソッドを作成し、電子メール メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="1c102-145">You then call the `WebMail` helper's `Send` method to create and send the email message.</span></span> <span data-ttu-id="1c102-146">この場合、使用する値は、フォームから送信された値と連結されるテキストされています。</span><span class="sxs-lookup"><span data-stu-id="1c102-146">In this case, the values to use are made up of text that you concatenate with the values that were submitted from the form.</span></span>

    <span data-ttu-id="1c102-147">このページのコードは、内部、`try/catch`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="1c102-147">The code for this page is inside a `try/catch` block.</span></span> <span data-ttu-id="1c102-148">何らかの理由で送信しようとすると、メールが動作しない場合 (たとえば、この設定は右側)、内のコード、`catch`ブロックが実行され、設定、`errorMessage`変数をエラーが発生したことにします。</span><span class="sxs-lookup"><span data-stu-id="1c102-148">If for any reason the attempt to send an email doesn't work (for example, the settings aren't right), the code in the `catch` block runs and sets the `errorMessage` variable to the error that has occurred.</span></span> <span data-ttu-id="1c102-149">(詳細については`try/catch`ブロックまたは`<text>`タグは、「 [ASP.NET Web ページは、Razor 構文を使用プログラミングの概要](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors))。</span><span class="sxs-lookup"><span data-stu-id="1c102-149">(For more information about `try/catch` blocks or the `<text>` tag, see [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)</span></span>

    <span data-ttu-id="1c102-150">ページの本文に場合、`errorMessage`変数が空 (既定)、ユーザーには、電子メール メッセージが送信されたメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1c102-150">In the body of the page, if the `errorMessage` variable is empty (the default), the user sees a message that the email message has been sent.</span></span> <span data-ttu-id="1c102-151">場合、`errorMessage`変数が true の場合、ユーザーが参照されていると、メッセージを送信中にエラー メッセージに設定します。</span><span class="sxs-lookup"><span data-stu-id="1c102-151">If the `errorMessage` variable is set to true, the user sees a message that there's been a problem sending the message.</span></span>

    <span data-ttu-id="1c102-152">エラー メッセージが表示されるページの部分では、追加のテストに注意してください:`if(debuggingFlag)`です。</span><span class="sxs-lookup"><span data-stu-id="1c102-152">Notice that in the portion of the page that displays an error message, there's an additional test: `if(debuggingFlag)`.</span></span> <span data-ttu-id="1c102-153">これは、電子メールを送信する問題がある場合は true に設定できる変数です。</span><span class="sxs-lookup"><span data-stu-id="1c102-153">This is a variable that you can set to true if you're having trouble sending email.</span></span> <span data-ttu-id="1c102-154">ときに`debuggingFlag`が true の場合、電子メールの送信に問題がある場合、追加のエラー メッセージが表示何でも ASP.NET が報告を電子メール メッセージを送信しようとしたときに表示するとします。</span><span class="sxs-lookup"><span data-stu-id="1c102-154">When `debuggingFlag` is true, and if there's a problem sending email, an additional error message is displayed that shows whatever ASP.NET has reported when it tried to send the email message.</span></span> <span data-ttu-id="1c102-155">正当な警告、ただし: ASP.NET が電子メール メッセージを送信できない場合に報告されるエラー メッセージをジェネリックにすることができます。</span><span class="sxs-lookup"><span data-stu-id="1c102-155">Fair warning, though: the error messages that ASP.NET reports when it can't send an email message can be generic.</span></span> <span data-ttu-id="1c102-156">たとえば場合、(たとえば、サーバー名 に、エラーが発生した) ために、ASP.NET は、SMTP サーバーを接続できません、エラーは`Failure sending mail`します。</span><span class="sxs-lookup"><span data-stu-id="1c102-156">For example, if ASP.NET can't contact the SMTP server (for example, because you made an error in the server name), the error is `Failure sending mail`.</span></span>

    > [!NOTE] 
    > 
    > <span data-ttu-id="1c102-157">**重要な**例外オブジェクトからエラー メッセージを取得するときに (`ex`コードで)、操作を行います*いない*たままにし、通常では、そのメッセージをユーザーに渡します。</span><span class="sxs-lookup"><span data-stu-id="1c102-157">**Important** When you get an error message from an exception object (`ex` in the code), do *not* routinely pass that message through to users.</span></span> <span data-ttu-id="1c102-158">多くの場合、例外オブジェクトには、ユーザーは表示されず、セキュリティの脆弱性をすることができますも情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="1c102-158">Exception objects often include information that users should not see and that can even be a security vulnerability.</span></span> <span data-ttu-id="1c102-159">理由は次のコードには、変数が含まれています。`debuggingFlag`エラー メッセージを表示するスイッチとして使用されると、既定では、変数を false に設定する理由。</span><span class="sxs-lookup"><span data-stu-id="1c102-159">That's why this code includes the variable `debuggingFlag` that's used as a switch to display the error message, and why the variable by default is set to false.</span></span> <span data-ttu-id="1c102-160">True (そのため、エラー メッセージを表示) するには、その変数を設定する必要があります*のみ*電子メールの送信に関する問題が発生すると、デバッグする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c102-160">You should set that variable to true (and therefore display the error message) *only* if you're having a problem with sending email and you need to debug.</span></span> <span data-ttu-id="1c102-161">問題を修正して、一度設定`debuggingFlag`を false に戻します。</span><span class="sxs-lookup"><span data-stu-id="1c102-161">Once you have fixed any problems, set `debuggingFlag` back to false.</span></span>

    <span data-ttu-id="1c102-162">変更、次のコードに関連する設定を電子メールで送信します。</span><span class="sxs-lookup"><span data-stu-id="1c102-162">Modify the following email related settings in the code:</span></span>

    - <span data-ttu-id="1c102-163">設定`your-SMTP-host`へのアクセスが SMTP サーバーの名前にします。</span><span class="sxs-lookup"><span data-stu-id="1c102-163">Set `your-SMTP-host` to the name of the SMTP server that you have access to.</span></span>
    - <span data-ttu-id="1c102-164">設定`your-user-name-here`SMTP サーバーのアカウントのユーザー名にします。</span><span class="sxs-lookup"><span data-stu-id="1c102-164">Set `your-user-name-here` to the user name for your SMTP server account.</span></span>
    - <span data-ttu-id="1c102-165">設定`your-account-password`SMTP サーバーのアカウントのパスワードにします。</span><span class="sxs-lookup"><span data-stu-id="1c102-165">Set `your-account-password` to the password for your SMTP server account.</span></span>
    - <span data-ttu-id="1c102-166">設定`your-email-address-here`の電子メール アドレス。</span><span class="sxs-lookup"><span data-stu-id="1c102-166">Set `your-email-address-here` to your own email address.</span></span> <span data-ttu-id="1c102-167">これは、メッセージの送信元電子メール アドレスです。</span><span class="sxs-lookup"><span data-stu-id="1c102-167">This is the email address that the message is sent from.</span></span> <span data-ttu-id="1c102-168">(電子メール プロバイダーによってしない指定できます異なる`From`解決し、として、ユーザー名を使用、`From`アドレスです。)。</span><span class="sxs-lookup"><span data-stu-id="1c102-168">(Some email providers don't let you specify a different `From` address and will use your user name as the `From` address.)</span></span>

    > [!TIP] 
    > 
    > <a id="configuring_email_settings"></a>
    > ### <a name="configuring-email-settings"></a><span data-ttu-id="1c102-169">電子メールの設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="1c102-169">Configuring Email Settings</span></span>
    > 
    > <span data-ttu-id="1c102-170">SMTP サーバー、ポート番号、およびなどの適切な設定があるかどうかを確認することもあります課題があります。</span><span class="sxs-lookup"><span data-stu-id="1c102-170">It can be a challenge sometimes to make sure you have the right settings for the SMTP server, port number, and so on.</span></span> <span data-ttu-id="1c102-171">いくつかのヒントを次に示します。</span><span class="sxs-lookup"><span data-stu-id="1c102-171">Here are a few tips:</span></span>
    > 
    > - <span data-ttu-id="1c102-172">SMTP サーバー名は、のようなものでは多くの場合、`smtp.provider.com`または`smtp.provider.net`です。</span><span class="sxs-lookup"><span data-stu-id="1c102-172">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="1c102-173">ただし、ホスティング プロバイダーにサイトを発行する場合、SMTP サーバー名の点にあります`localhost`です。</span><span class="sxs-lookup"><span data-stu-id="1c102-173">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="1c102-174">これは、公開したすると、サイトが、プロバイダーのサーバーで実行されている、電子メール サーバーは、アプリケーションの観点からローカル可能性があるためです。</span><span class="sxs-lookup"><span data-stu-id="1c102-174">This is because after you've published and your site is running on the provider's server, the email server might be local from the perspective of your application.</span></span> <span data-ttu-id="1c102-175">サーバー名には、この変更は、発行プロセスの一部として SMTP サーバー名を変更する必要がある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1c102-175">This change in server names might mean you have to change the SMTP server name as part of your publishing process.</span></span>
    > - <span data-ttu-id="1c102-176">ポート番号は、通常は 25 です。</span><span class="sxs-lookup"><span data-stu-id="1c102-176">The port number is usually 25.</span></span> <span data-ttu-id="1c102-177">ただし、一部のプロバイダーを使用する必要ポート 587 またはいくつか他のポートです。</span><span class="sxs-lookup"><span data-stu-id="1c102-177">However, some providers require you to use port 587 or some other port.</span></span>
    > - <span data-ttu-id="1c102-178">適切な資格情報を使用することを確認します。</span><span class="sxs-lookup"><span data-stu-id="1c102-178">Make sure that you use the right credentials.</span></span> <span data-ttu-id="1c102-179">ホスティング プロバイダーには、サイトを公開した場合は、プロバイダーが具体的には示されている電子メールには、資格情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="1c102-179">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="1c102-180">これらは、発行に使用する資格情報と異なる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1c102-180">These might be different from the credentials you use to publish.</span></span>
    > - <span data-ttu-id="1c102-181">場合によって資格情報をまったく必要ありません。</span><span class="sxs-lookup"><span data-stu-id="1c102-181">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="1c102-182">個人の ISP を使用して電子メールを送信する場合は、電子メール プロバイダーが、資格情報を知っている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1c102-182">If you're sending email using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="1c102-183">パブリッシュした後は、ローカル コンピューターでテストするとよりも別の資格情報を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c102-183">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
    > - <span data-ttu-id="1c102-184">設定する必要がある、電子メール プロバイダーには、暗号化が使用されている場合`WebMail.EnableSsl`に`true`です。</span><span class="sxs-lookup"><span data-stu-id="1c102-184">If your email provider uses encryption, you have to set `WebMail.EnableSsl` to `true`.</span></span>
4. <span data-ttu-id="1c102-185">実行、 *EmailRequest.cshtml*ブラウザーのページです。</span><span class="sxs-lookup"><span data-stu-id="1c102-185">Run the *EmailRequest.cshtml* page in a browser.</span></span> <span data-ttu-id="1c102-186">(ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。</span><span class="sxs-lookup"><span data-stu-id="1c102-186">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
5. <span data-ttu-id="1c102-187">自分の名前と問題の説明を入力し、をクリックして、**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1c102-187">Enter your name and a problem description, and then click the **Submit** button.</span></span> <span data-ttu-id="1c102-188">リダイレクトしている、 *ProcessRequest.cshtml*  ページで、メッセージのことを確認して、電子メール メッセージを送信しています。</span><span class="sxs-lookup"><span data-stu-id="1c102-188">You're redirected to the *ProcessRequest.cshtml* page, which confirms your message and which sends you an email message.</span></span> 

    ![[イメージ]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a><span data-ttu-id="1c102-190">電子メールを使用してファイルを送信します。</span><span class="sxs-lookup"><span data-stu-id="1c102-190">Sending a File Using Email</span></span>

<span data-ttu-id="1c102-191">電子メール メッセージに添付されているファイルを送信することもできます。</span><span class="sxs-lookup"><span data-stu-id="1c102-191">You can also send files that are attached to email messages.</span></span> <span data-ttu-id="1c102-192">この手順では、テキスト ファイルと、2 つの HTML ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="1c102-192">In this procedure, you create a text file and two HTML pages.</span></span> <span data-ttu-id="1c102-193">電子メールの添付ファイルとして、テキスト ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="1c102-193">You'll use the text file as an email attachment.</span></span>

1. <span data-ttu-id="1c102-194">Web サイトで、新しいテキスト ファイルを追加し、名前を付けます*MyFile.txt*です。</span><span class="sxs-lookup"><span data-stu-id="1c102-194">In the website, add a new text file and name it *MyFile.txt*.</span></span>
2. <span data-ttu-id="1c102-195">次のテキストをコピーし、ファイルに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="1c102-195">Copy the following text and paste it in the file:</span></span> 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. <span data-ttu-id="1c102-196">という名前のページを作成する*SendFile.cshtml*し、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="1c102-196">Create a page named *SendFile.cshtml* and add the following markup:</span></span> 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. <span data-ttu-id="1c102-197">という名前のページを作成する*ProcessFile.cshtml*し、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="1c102-197">Create a page named *ProcessFile.cshtml* and add the following markup:</span></span> 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. <span data-ttu-id="1c102-198">変更、次の例のコードに関連する設定を電子メールで送信します。</span><span class="sxs-lookup"><span data-stu-id="1c102-198">Modify the following email related settings in the code from the example:</span></span>

    - <span data-ttu-id="1c102-199">設定`your-SMTP-host`へのアクセスが SMTP サーバーの名前にします。</span><span class="sxs-lookup"><span data-stu-id="1c102-199">Set `your-SMTP-host` to the name of an SMTP server that you have access to.</span></span>
    - <span data-ttu-id="1c102-200">設定`your-user-name-here`SMTP サーバーのアカウントのユーザー名にします。</span><span class="sxs-lookup"><span data-stu-id="1c102-200">Set `your-user-name-here` to the user name for your SMTP server account.</span></span>
    - <span data-ttu-id="1c102-201">設定`your-email-address-here`の電子メール アドレス。</span><span class="sxs-lookup"><span data-stu-id="1c102-201">Set `your-email-address-here` to your own email address.</span></span> <span data-ttu-id="1c102-202">これは、メッセージの送信元電子メール アドレスです。</span><span class="sxs-lookup"><span data-stu-id="1c102-202">This is the email address that the message is sent from.</span></span>
    - <span data-ttu-id="1c102-203">設定`your-account-password`SMTP サーバーのアカウントのパスワードにします。</span><span class="sxs-lookup"><span data-stu-id="1c102-203">Set `your-account-password` to the password for your SMTP server account.</span></span>
    - <span data-ttu-id="1c102-204">設定`target-email-address-here`の電子メール アドレス。</span><span class="sxs-lookup"><span data-stu-id="1c102-204">Set `target-email-address-here` to your own email address.</span></span> <span data-ttu-id="1c102-205">(ようにする前に、テストが、別のユーザーに電子メールを送信すると通常、送信できますを自分でします。)</span><span class="sxs-lookup"><span data-stu-id="1c102-205">(As before, you'd normally send an email to someone else, but for testing, you can send it to yourself.)</span></span>
6. <span data-ttu-id="1c102-206">実行、 *SendFile.cshtml*ブラウザーのページです。</span><span class="sxs-lookup"><span data-stu-id="1c102-206">Run the *SendFile.cshtml* page in a browser.</span></span>
7. <span data-ttu-id="1c102-207">名前、件名、および添付するテキスト ファイルの名前を入力 (*MyFile.txt*)。</span><span class="sxs-lookup"><span data-stu-id="1c102-207">Enter your name, a subject line, and the name of the text file to attach (*MyFile.txt*).</span></span>
8. <span data-ttu-id="1c102-208">[`Submit`] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1c102-208">Click the `Submit` button.</span></span> <span data-ttu-id="1c102-209">前にリダイレクトするするいると、 *ProcessFile.cshtml*  ページで、メッセージを確認するメッセージを送信する電子メール添付ファイルにします。</span><span class="sxs-lookup"><span data-stu-id="1c102-209">As before, you're redirected to the *ProcessFile.cshtml* page, which confirms your message and which sends you an email message with the attached file.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="1c102-210">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="1c102-210">Additional Resources</span></span>


- [<span data-ttu-id="1c102-211">ASP.NET Web ページ (Razor) トラブルシューティング ガイド</span><span class="sxs-lookup"><span data-stu-id="1c102-211">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>](https://go.microsoft.com/fwlink/?LinkId=253001)
- [<span data-ttu-id="1c102-212">簡易メール転送プロトコル</span><span class="sxs-lookup"><span data-stu-id="1c102-212">Simple Mail Transfer Protocol</span></span>](https://msdn.microsoft.com/library/aa480435.aspx)
- [<span data-ttu-id="1c102-213">ASP.NET Web ページのサイト全体の動作をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="1c102-213">Customizing Site-Wide Behavior for ASP.NET Web Pages</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
