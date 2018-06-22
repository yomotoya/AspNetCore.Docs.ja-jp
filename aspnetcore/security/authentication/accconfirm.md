---
title: アカウントの確認と ASP.NET Core でのパスワードの回復
author: rick-anderson
description: 電子メールの確認とパスワードのリセットと ASP.NET Core アプリケーションをビルドする方法を説明します。
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: db41dd47518fa8b35c006b3291068e7724cf6cca
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275081"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="89974-103">アカウントの確認と ASP.NET Core でのパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="89974-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="89974-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="89974-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="89974-105">このチュートリアルでは、電子メールの確認とパスワードのリセットと ASP.NET Core アプリケーションをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="89974-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="89974-106">このチュートリアルでは、**いない**先頭トピックです。</span><span class="sxs-lookup"><span data-stu-id="89974-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="89974-107">理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="89974-107">You should be familiar with:</span></span>

* [<span data-ttu-id="89974-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89974-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="89974-109">認証</span><span class="sxs-lookup"><span data-stu-id="89974-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="89974-110">アカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="89974-110">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="89974-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="89974-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="89974-112">参照してください[この PDF ファイル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core MVC 1.1 および 2.x バージョン。</span><span class="sxs-lookup"><span data-stu-id="89974-112">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="89974-113">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="89974-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="89974-114">.NET Core CLI を使用して新しい ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="89974-114">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="89974-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="89974-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="89974-117">`--auth Individual` 個々 のユーザー アカウントのプロジェクト テンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="89974-117">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="89974-118">Windows では、追加、`-uld`オプション。</span><span class="sxs-lookup"><span data-stu-id="89974-118">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="89974-119">SQLite ではなく LocalDB を使用する必要がありますを指定します。</span><span class="sxs-lookup"><span data-stu-id="89974-119">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="89974-120">実行`new mvc --help`にこのコマンドのヘルプを表示します。</span><span class="sxs-lookup"><span data-stu-id="89974-120">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="89974-121">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="89974-121">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="89974-122">CLI または SQLite を使用している場合は、コマンド ウィンドウで、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="89974-122">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="89974-123">`--auth Individual` 個々 のユーザー アカウントのプロジェクト テンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="89974-123">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="89974-124">Windows では、追加、`-uld`オプション。</span><span class="sxs-lookup"><span data-stu-id="89974-124">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="89974-125">SQLite ではなく LocalDB を使用する必要がありますを指定します。</span><span class="sxs-lookup"><span data-stu-id="89974-125">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="89974-126">実行`new mvc --help`にこのコマンドのヘルプを表示します。</span><span class="sxs-lookup"><span data-stu-id="89974-126">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="89974-127">または、Visual Studio で新しい ASP.NET Core プロジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="89974-127">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="89974-128">Visual Studio で、新しい**Web アプリケーション**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="89974-128">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="89974-129">選択**ASP.NET Core 2.0**です。</span><span class="sxs-lookup"><span data-stu-id="89974-129">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="89974-130">**.NET core**が次の図では、選択されている選択することができますが、 **.NET Framework**です。</span><span class="sxs-lookup"><span data-stu-id="89974-130">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="89974-131">選択**認証の変更**に設定および**個々 のユーザー アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="89974-131">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="89974-132">既定値を保持**ストア ユーザー アカウントをアプリ内**です。</span><span class="sxs-lookup"><span data-stu-id="89974-132">Keep the default **Store user accounts in-app**.</span></span>

![新しいプロジェクト ダイアログ ボックスを示す"個別のユーザー アカウント radio"が選択されています。](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="89974-134">テストの新規ユーザーの登録</span><span class="sxs-lookup"><span data-stu-id="89974-134">Test new user registration</span></span>

<span data-ttu-id="89974-135">アプリを実行する、選択、**登録**リンク、およびユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="89974-135">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="89974-136">Entity Framework Core 移行を実行する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="89974-136">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="89974-137">この時点では、電子メールにのみ、検証、 [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)属性。</span><span class="sxs-lookup"><span data-stu-id="89974-137">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="89974-138">登録を完了した後、アプリにログインします。</span><span class="sxs-lookup"><span data-stu-id="89974-138">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="89974-139">チュートリアルの後半では、自分の電子メールが検証されるまで新規ユーザーがログインできないため、コードが更新されます。</span><span class="sxs-lookup"><span data-stu-id="89974-139">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="89974-140">ユーザー データベースを表示します。</span><span class="sxs-lookup"><span data-stu-id="89974-140">View the Identity database</span></span>

<span data-ttu-id="89974-141">参照してください[SQLite と ASP.NET Core MVC プロジェクトで作業](xref:tutorials/first-mvc-app-xplat/working-with-sql)SQLite データベースを表示する方法についてです。</span><span class="sxs-lookup"><span data-stu-id="89974-141">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="89974-142">For Visual Studio は。</span><span class="sxs-lookup"><span data-stu-id="89974-142">For Visual Studio:</span></span>

* <span data-ttu-id="89974-143">**ビュー**メニューの  **SQL Server オブジェクト エクスプ ローラー** (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="89974-143">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="89974-144">移動 **(localdb) MSSQLLocalDB (SQL Server 13)** です。</span><span class="sxs-lookup"><span data-stu-id="89974-144">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="89974-145">右クリックして**dbo します。AspNetUsers** > **データ表示**:</span><span class="sxs-lookup"><span data-stu-id="89974-145">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![SQL Server オブジェクト エクスプ ローラー AspNetUsers テーブルのコンテキスト メニュー](accconfirm/_static/ssox.png)

<span data-ttu-id="89974-147">テーブルに注意してください`EmailConfirmed`フィールドは`False`します。</span><span class="sxs-lookup"><span data-stu-id="89974-147">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="89974-148">アプリが確認の電子メールを送信するとき、次の手順でこの電子メールを再利用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="89974-148">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="89974-149">クリックし、行を右クリックして**削除**です。</span><span class="sxs-lookup"><span data-stu-id="89974-149">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="89974-150">電子メールのエイリアスを削除すると簡単に、次の手順にできます。</span><span class="sxs-lookup"><span data-stu-id="89974-150">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="89974-151">HTTPS が必要</span><span class="sxs-lookup"><span data-stu-id="89974-151">Require HTTPS</span></span>

<span data-ttu-id="89974-152">参照してください[Https](xref:security/enforcing-ssl)です。</span><span class="sxs-lookup"><span data-stu-id="89974-152">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="89974-153">確認の電子メールが必要</span><span class="sxs-lookup"><span data-stu-id="89974-153">Require email confirmation</span></span>

<span data-ttu-id="89974-154">新しいユーザーの登録の電子メール アドレスを確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="89974-154">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="89974-155">電子メールの他のユーザー偽装はいないしていることを確認するために確認役立ちます (つまり、まだに登録されている他のユーザーの電子メール)。</span><span class="sxs-lookup"><span data-stu-id="89974-155">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="89974-156">ディスカッション フォーラムいてしないようにすると仮定します"yli@example.com"として登録する"fromnolivetto@contoso.com"です。</span><span class="sxs-lookup"><span data-stu-id="89974-156">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="89974-157">電子メールの確認なし"nolivetto@contoso.com"は、アプリから不要な電子メールを受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="89974-157">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="89974-158">ユーザーが誤ってとして登録されていると仮定します"ylo@example.com""yli"のスペル ミスを認識していたとします。</span><span class="sxs-lookup"><span data-stu-id="89974-158">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="89974-159">できなく、アプリは、正しいメール アドレスがある見つからないために、パスワードの回復を使用します。</span><span class="sxs-lookup"><span data-stu-id="89974-159">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="89974-160">確認の電子メールは、bot から保護には制限を提供します。</span><span class="sxs-lookup"><span data-stu-id="89974-160">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="89974-161">確認の電子メールは、多くの電子メール アカウントを持つ悪意のあるユーザーから保護を提供しません。</span><span class="sxs-lookup"><span data-stu-id="89974-161">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="89974-162">一般にする新しいユーザーが確認された電子メールがある前に、web サイトにデータを投稿するを防ぐ。</span><span class="sxs-lookup"><span data-stu-id="89974-162">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="89974-163">更新`ConfigureServices`確認電子メールを要求します。</span><span class="sxs-lookup"><span data-stu-id="89974-163">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="89974-164">`config.SignIn.RequireConfirmedEmail = true;` 登録済みユーザーがで自分の電子メールが確認されるまでのログ記録するを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="89974-164">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="89974-165">電子メール プロバイダーを構成します。</span><span class="sxs-lookup"><span data-stu-id="89974-165">Configure email provider</span></span>

<span data-ttu-id="89974-166">このチュートリアルでは、SendGrid を使用して電子メールを送信します。</span><span class="sxs-lookup"><span data-stu-id="89974-166">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="89974-167">SendGrid アカウントと電子メールを送信するキーが必要です。</span><span class="sxs-lookup"><span data-stu-id="89974-167">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="89974-168">その他の電子メール プロバイダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="89974-168">You can use other email providers.</span></span> <span data-ttu-id="89974-169">ASP.NET Core 2.x が含まれています`System.Net.Mail`、アプリから電子メールを送信することができます。</span><span class="sxs-lookup"><span data-stu-id="89974-169">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="89974-170">SendGrid または別の電子メール サービスを使用して電子メールを送信することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="89974-170">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="89974-171">SMTP は適切に設定してセキュリティで保護するが困難です。</span><span class="sxs-lookup"><span data-stu-id="89974-171">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="89974-172">[オプション パターン](xref:fundamentals/configuration/options)ユーザー アカウントとキーの設定にアクセスするために使用します。</span><span class="sxs-lookup"><span data-stu-id="89974-172">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="89974-173">詳細については、次を参照してください。[構成](xref:fundamentals/configuration/index)です。</span><span class="sxs-lookup"><span data-stu-id="89974-173">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="89974-174">セキュリティで保護された電子メールのキーを取得するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="89974-174">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="89974-175">このサンプルで、`AuthMessageSenderOptions`でクラスを作成、 *Services/AuthMessageSenderOptions.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="89974-175">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="89974-176">設定、`SendGridUser`と`SendGridKey`で、[シークレット マネージャー ツール](xref:security/app-secrets)です。</span><span class="sxs-lookup"><span data-stu-id="89974-176">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="89974-177">例えば:</span><span class="sxs-lookup"><span data-stu-id="89974-177">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="89974-178">Windows では、シークレット マネージャーが内のキー/値ペアを格納、 *secrets.json*ファイルで、`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="89974-178">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="89974-179">内容、 *secrets.json*ファイルは暗号化されません。</span><span class="sxs-lookup"><span data-stu-id="89974-179">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="89974-180">*Secrets.json*ファイルを次に示します (、`SendGridKey`値が削除されました)。</span><span class="sxs-lookup"><span data-stu-id="89974-180">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="89974-181">AuthMessageSenderOptions を使用するスタートアップを構成します。</span><span class="sxs-lookup"><span data-stu-id="89974-181">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="89974-182">追加`AuthMessageSenderOptions`サービス コンテナーの最後に、`ConfigureServices`メソッドで、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="89974-182">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="89974-183">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="89974-183">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="89974-184">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="89974-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="89974-185">AuthMessageSender クラスを構成します。</span><span class="sxs-lookup"><span data-stu-id="89974-185">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="89974-186">このチュートリアルを使用して電子メール通知を追加する方法を示しています。 [SendGrid](https://sendgrid.com/)、SMTP、およびその他のメカニズムを使用して電子メールを送信することができますが、します。</span><span class="sxs-lookup"><span data-stu-id="89974-186">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="89974-187">インストール、 `SendGrid` NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="89974-187">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="89974-188">: コマンドラインから</span><span class="sxs-lookup"><span data-stu-id="89974-188">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="89974-189">パッケージ マネージャー コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="89974-189">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="89974-190">参照してください[始める SendGrid 無料](https://sendgrid.com/free/)無料の SendGrid アカウントを登録します。</span><span class="sxs-lookup"><span data-stu-id="89974-190">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="89974-191">SendGrid を構成します。</span><span class="sxs-lookup"><span data-stu-id="89974-191">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="89974-192">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="89974-192">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="89974-193">SendGrid を構成するには、次のようなコードを追加*Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="89974-193">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="89974-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="89974-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="89974-195">コードを追加*Services/MessageServices.cs* SendGrid を構成するには、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="89974-195">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="89974-196">アカウントの確認とパスワードの回復を有効にします。</span><span class="sxs-lookup"><span data-stu-id="89974-196">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="89974-197">テンプレートは、アカウントの確認とパスワードの回復用コードを持っています。</span><span class="sxs-lookup"><span data-stu-id="89974-197">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="89974-198">検索、`OnPostAsync`メソッド*Pages/Account/Register.cshtml.cs*です。</span><span class="sxs-lookup"><span data-stu-id="89974-198">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="89974-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="89974-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="89974-200">新しく登録されたユーザーが次の行をコメント アウトして自動ログオンされているようにします。</span><span class="sxs-lookup"><span data-stu-id="89974-200">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="89974-201">メソッド全体を強調表示されている変更された行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="89974-201">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="89974-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="89974-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="89974-203">アカウントの確認を有効にするには、次のコードをコメントを解除します。</span><span class="sxs-lookup"><span data-stu-id="89974-203">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="89974-204">**注:** コードなると、新しく登録されたユーザーは、次の行をコメント アウトして自動ログオンされているが妨げ。</span><span class="sxs-lookup"><span data-stu-id="89974-204">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="89974-205">内のコードのコメントを解除してパスワードの回復を有効にする、`ForgotPassword`のアクション*Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="89974-205">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="89974-206">フォーム要素をコメントから*Views/Account/ForgotPassword.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="89974-206">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="89974-207">削除する必要があります、`<p> For more information on how to enable reset password ... </p>`要素は、この記事の内容へのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="89974-207">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="89974-208">登録、確認電子メール、およびパスワードのリセット</span><span class="sxs-lookup"><span data-stu-id="89974-208">Register, confirm email, and reset password</span></span>

<span data-ttu-id="89974-209">Web アプリを実行し、アカウントの確認とパスワードの回復フローをテストします。</span><span class="sxs-lookup"><span data-stu-id="89974-209">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="89974-210">アプリを実行して、新しいユーザーの登録</span><span class="sxs-lookup"><span data-stu-id="89974-210">Run the app and register a new user</span></span>

  ![Web アプリケーション取引明細の表示](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="89974-212">アカウントの確認リンクには、電子メールを確認してください。</span><span class="sxs-lookup"><span data-stu-id="89974-212">Check your email for the account confirmation link.</span></span> <span data-ttu-id="89974-213">参照してください[電子メールをデバッグ](#debug)電子メールを取得しない場合。</span><span class="sxs-lookup"><span data-stu-id="89974-213">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="89974-214">確認の電子メールへのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="89974-214">Click the link to confirm your email.</span></span>
* <span data-ttu-id="89974-215">電子メール アドレスとパスワードでログインします。</span><span class="sxs-lookup"><span data-stu-id="89974-215">Log in with your email and password.</span></span>
* <span data-ttu-id="89974-216">ログオフします。</span><span class="sxs-lookup"><span data-stu-id="89974-216">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="89974-217">ビューの管理 ページ</span><span class="sxs-lookup"><span data-stu-id="89974-217">View the manage page</span></span>

<span data-ttu-id="89974-218">ブラウザーで、ユーザー名を選択:![ブラウザー ウィンドウでユーザー名](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="89974-218">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="89974-219">ユーザー名を確認して、ナビゲーションバーを展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="89974-219">You might need to expand the navbar to see user name.</span></span>

![ナビゲーション バー](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="89974-221">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="89974-221">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="89974-222">管理 ページが表示され、**プロファイル**タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="89974-222">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="89974-223">**電子メール**電子メールを示すチェック ボックスが確認されているかを示します。</span><span class="sxs-lookup"><span data-stu-id="89974-223">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![[管理] ページ](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="89974-225">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="89974-225">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="89974-226">これは、チュートリアルの後半で説明されています。</span><span class="sxs-lookup"><span data-stu-id="89974-226">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="89974-227">![[管理] ページ](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="89974-227">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="89974-228">テストのパスワードのリセット</span><span class="sxs-lookup"><span data-stu-id="89974-228">Test password reset</span></span>

* <span data-ttu-id="89974-229">ログインしている場合は、選択**ログアウト**です。</span><span class="sxs-lookup"><span data-stu-id="89974-229">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="89974-230">選択、**ログイン**リンクを選択して、**パスワードを忘れた場合ですか?** リンクします。</span><span class="sxs-lookup"><span data-stu-id="89974-230">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="89974-231">アカウントの登録に使用したメール アドレスを入力します。</span><span class="sxs-lookup"><span data-stu-id="89974-231">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="89974-232">パスワードをリセットするリンクを含む電子メールが送信されます。</span><span class="sxs-lookup"><span data-stu-id="89974-232">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="89974-233">電子メールを確認し、パスワードをリセットするリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="89974-233">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="89974-234">パスワードが正常にリセットされると後、は、電子メールと新しいパスワードを使用してログインすることができます。</span><span class="sxs-lookup"><span data-stu-id="89974-234">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="89974-235">電子メールをデバッグします。</span><span class="sxs-lookup"><span data-stu-id="89974-235">Debug email</span></span>

<span data-ttu-id="89974-236">電子メールの作業を取得できません: 場合</span><span class="sxs-lookup"><span data-stu-id="89974-236">If you can't get email working:</span></span>

* <span data-ttu-id="89974-237">作成、[電子メールを送信するコンソール アプリ](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)です。</span><span class="sxs-lookup"><span data-stu-id="89974-237">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="89974-238">確認、[電子メール アクティビティ](https://sendgrid.com/docs/User_Guide/email_activity.html)ページ。</span><span class="sxs-lookup"><span data-stu-id="89974-238">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="89974-239">迷惑メール フォルダーをチェックします。</span><span class="sxs-lookup"><span data-stu-id="89974-239">Check your spam folder.</span></span>
* <span data-ttu-id="89974-240">別の電子メール プロバイダー (Microsoft、yahoo!、Gmail など) に別の電子メール エイリアスを再試行してください。</span><span class="sxs-lookup"><span data-stu-id="89974-240">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="89974-241">別の電子メール アカウントへの送信を再試行してください。</span><span class="sxs-lookup"><span data-stu-id="89974-241">Try sending to different email accounts.</span></span>

<span data-ttu-id="89974-242">**セキュリティのベスト プラクティス**を**いない**開発、テストや実稼働のシークレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="89974-242">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="89974-243">アプリを Azure に発行する場合は、Web アプリの Azure ポータルでのアプリケーション設定として、SendGrid の機密情報を設定できます。</span><span class="sxs-lookup"><span data-stu-id="89974-243">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="89974-244">構成システムは、環境変数からキーを読み取れませんを設定します。</span><span class="sxs-lookup"><span data-stu-id="89974-244">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="89974-245">ソーシャルとローカルのログイン アカウントを結合します。</span><span class="sxs-lookup"><span data-stu-id="89974-245">Combine social and local login accounts</span></span>

<span data-ttu-id="89974-246">このセクションを完了するには、まず外部認証プロバイダーを有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="89974-246">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="89974-247">参照してください[Facebook、Google、および外部プロバイダー認証](xref:security/authentication/social/index)です。</span><span class="sxs-lookup"><span data-stu-id="89974-247">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="89974-248">電子メールのリンクをクリックすると、ローカルおよびソーシャルのアカウントを組み合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="89974-248">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="89974-249">次の順序で"RickAndMSFT@gmail.com"が最初に、ローカルのログインとして作成ただし、最初に、ソーシャル ログインとしてアカウントを作成し、ローカル ログインを追加します。</span><span class="sxs-lookup"><span data-stu-id="89974-249">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web アプリケーション:RickAndMSFT@gmail.com認証されたユーザー](accconfirm/_static/rick.png)

<span data-ttu-id="89974-251">をクリックして、**管理**リンクします。</span><span class="sxs-lookup"><span data-stu-id="89974-251">Click on the **Manage** link.</span></span> <span data-ttu-id="89974-252">このアカウントに関連付けられた 0 外部 (ソーシャル ログイン) に注意してください。</span><span class="sxs-lookup"><span data-stu-id="89974-252">Note the 0 external (social logins) associated with this account.</span></span>

![ビューを管理します。](accconfirm/_static/manage.png)

<span data-ttu-id="89974-254">別のログイン サービスへのリンクをクリックし、アプリの要求を許可します。</span><span class="sxs-lookup"><span data-stu-id="89974-254">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="89974-255">次の図では、Facebook は、外部認証プロバイダーを示します。</span><span class="sxs-lookup"><span data-stu-id="89974-255">In the following image, Facebook is the external authentication provider:</span></span>

![Facebook を一覧表示する、外部ログイン ビューを管理します。](accconfirm/_static/fb.png)

<span data-ttu-id="89974-257">2 つのアカウントが統合されました。</span><span class="sxs-lookup"><span data-stu-id="89974-257">The two accounts have been combined.</span></span> <span data-ttu-id="89974-258">いずれかのアカウントでログオンするのには、できます。</span><span class="sxs-lookup"><span data-stu-id="89974-258">You are able to log on with either account.</span></span> <span data-ttu-id="89974-259">ユーザーに、ソーシャル ログイン認証サービスがダウンしているか、ソーシャル アカウントにアクセスを紛失した可能性が高い場合に、ローカル アカウントを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="89974-259">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="89974-260">後のサイトがあるユーザー アカウントの確認を有効にします。</span><span class="sxs-lookup"><span data-stu-id="89974-260">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="89974-261">有効にするアカウントの確認、サイトでユーザーとは、既存のすべてのユーザーをロックします。</span><span class="sxs-lookup"><span data-stu-id="89974-261">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="89974-262">自分のアカウントが確認されないために、既存のユーザーはロックアウトされます。</span><span class="sxs-lookup"><span data-stu-id="89974-262">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="89974-263">既存ユーザーのロックアウトを回避するには、次の方法のいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="89974-263">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="89974-264">確認されているすべての既存ユーザーを示すためにデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="89974-264">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="89974-265">既存のユーザーを確認します。</span><span class="sxs-lookup"><span data-stu-id="89974-265">Confirm exiting users.</span></span> <span data-ttu-id="89974-266">たとえば、バッチ送信確認リンクによる電子メールのです。</span><span class="sxs-lookup"><span data-stu-id="89974-266">For example, batch-send emails with confirmation links.</span></span>
