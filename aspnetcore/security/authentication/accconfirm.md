---
title: アカウントの確認と ASP.NET Core でのパスワードの回復
author: rick-anderson
description: 電子メールの確認とパスワードのリセットと ASP.NET Core アプリを構築する方法について説明します。
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803273"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="dc920-103">アカウントの確認と ASP.NET Core でのパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="dc920-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="dc920-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="dc920-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="dc920-105">このチュートリアルでは、電子メールの確認とパスワードのリセットと ASP.NET Core アプリをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="dc920-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="dc920-106">このチュートリアルは**いない**先頭トピック。</span><span class="sxs-lookup"><span data-stu-id="dc920-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="dc920-107">理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc920-107">You should be familiar with:</span></span>

* [<span data-ttu-id="dc920-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc920-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="dc920-109">認証</span><span class="sxs-lookup"><span data-stu-id="dc920-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="dc920-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="dc920-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="dc920-111">参照してください[この PDF ファイル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)の ASP.NET Core MVC 1.1 と 2.x のバージョン。</span><span class="sxs-lookup"><span data-stu-id="dc920-111">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dc920-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="dc920-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="dc920-113">.NET Core CLI を使用した新しい ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="dc920-113">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc920-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc920-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

* <span data-ttu-id="dc920-115">`--auth Individual` 個々 のユーザー アカウントのプロジェクト テンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="dc920-115">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="dc920-116">Windows では、追加、`-uld`オプション。</span><span class="sxs-lookup"><span data-stu-id="dc920-116">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="dc920-117">これは、SQLite の代わりに LocalDB を使用する必要がありますを指定します。</span><span class="sxs-lookup"><span data-stu-id="dc920-117">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="dc920-118">実行`new mvc --help`にこのコマンドのヘルプを表示します。</span><span class="sxs-lookup"><span data-stu-id="dc920-118">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc920-119">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc920-119">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="dc920-120">CLI または SQLite を使用している場合は、コマンド ウィンドウで、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="dc920-120">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="dc920-121">`--auth Individual` 個々 のユーザー アカウントのプロジェクト テンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="dc920-121">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="dc920-122">Windows では、追加、`-uld`オプション。</span><span class="sxs-lookup"><span data-stu-id="dc920-122">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="dc920-123">これは、SQLite の代わりに LocalDB を使用する必要がありますを指定します。</span><span class="sxs-lookup"><span data-stu-id="dc920-123">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="dc920-124">実行`new mvc --help`にこのコマンドのヘルプを表示します。</span><span class="sxs-lookup"><span data-stu-id="dc920-124">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="dc920-125">または、Visual Studio で新しい ASP.NET Core プロジェクトを作成できます。</span><span class="sxs-lookup"><span data-stu-id="dc920-125">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="dc920-126">Visual Studio で、作成、新しい**Web アプリケーション**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="dc920-126">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="dc920-127">選択**ASP.NET Core 2.0**します。</span><span class="sxs-lookup"><span data-stu-id="dc920-127">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="dc920-128">**.NET core**が次の図で選択されている選択することができますが、 **.NET Framework**します。</span><span class="sxs-lookup"><span data-stu-id="dc920-128">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="dc920-129">選択**認証の変更**に設定し、**個々 のユーザー アカウント**します。</span><span class="sxs-lookup"><span data-stu-id="dc920-129">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="dc920-130">既定値を保持**ストア ユーザー アカウントをアプリ内**します。</span><span class="sxs-lookup"><span data-stu-id="dc920-130">Keep the default **Store user accounts in-app**.</span></span>

![選択されている"個々 のユーザー アカウント radio"を示す新しいプロジェクト ダイアログ ボックス](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="dc920-132">新規ユーザー登録をテストします。</span><span class="sxs-lookup"><span data-stu-id="dc920-132">Test new user registration</span></span>

<span data-ttu-id="dc920-133">アプリを実行し、選択、**登録**リンク、およびユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="dc920-133">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="dc920-134">Entity Framework Core migrations を実行する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="dc920-134">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="dc920-135">この時点では、電子メールの検証のみ、 [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)属性。</span><span class="sxs-lookup"><span data-stu-id="dc920-135">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="dc920-136">登録を送信した後は、アプリにログインします。</span><span class="sxs-lookup"><span data-stu-id="dc920-136">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="dc920-137">チュートリアルの後半では、新しいユーザーが自分の電子メールが検証されるまでにログインできないため、コードが更新されます。</span><span class="sxs-lookup"><span data-stu-id="dc920-137">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="dc920-138">Id データベースを表示します。</span><span class="sxs-lookup"><span data-stu-id="dc920-138">View the Identity database</span></span>

<span data-ttu-id="dc920-139">参照してください[、ASP.NET Core MVC プロジェクトでの SQLite の使用](xref:tutorials/first-mvc-app-xplat/working-with-sql)SQLite データベースを表示する方法の詳細について。</span><span class="sxs-lookup"><span data-stu-id="dc920-139">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="dc920-140">For Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="dc920-140">For Visual Studio:</span></span>

* <span data-ttu-id="dc920-141">**ビュー**メニューの  **SQL Server オブジェクト エクスプ ローラー** (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="dc920-141">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="dc920-142">移動します **(localdb) (SQL Server 13) MSSQLLocalDB**します。</span><span class="sxs-lookup"><span data-stu-id="dc920-142">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="dc920-143">右クリックして**dbo します。AspNetUsers** > **データを表示する**:</span><span class="sxs-lookup"><span data-stu-id="dc920-143">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![SQL Server オブジェクト エクスプ ローラー AspNetUsers テーブルのコンテキスト メニュー](accconfirm/_static/ssox.png)

<span data-ttu-id="dc920-145">テーブルに注意してください`EmailConfirmed`フィールドは`False`します。</span><span class="sxs-lookup"><span data-stu-id="dc920-145">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="dc920-146">アプリが送信された確認メールを送信するとき、次の手順でこの電子メールをもう一度使用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="dc920-146">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="dc920-147">クリックし、行を右クリックして**削除**します。</span><span class="sxs-lookup"><span data-stu-id="dc920-147">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="dc920-148">電子メール エイリアスを削除する簡単で、次の手順。</span><span class="sxs-lookup"><span data-stu-id="dc920-148">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="dc920-149">HTTPS が必要です。</span><span class="sxs-lookup"><span data-stu-id="dc920-149">Require HTTPS</span></span>

<span data-ttu-id="dc920-150">参照してください[Https](xref:security/enforcing-ssl)します。</span><span class="sxs-lookup"><span data-stu-id="dc920-150">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="dc920-151">確認の電子メールが必要です。</span><span class="sxs-lookup"><span data-stu-id="dc920-151">Require email confirmation</span></span>

<span data-ttu-id="dc920-152">新規ユーザー登録の電子メール アドレスを確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="dc920-152">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="dc920-153">電子メールの他のユーザーが偽装はいないしていることを確認することを確認できます (つまりに登録していない他のユーザーの電子メールで)。</span><span class="sxs-lookup"><span data-stu-id="dc920-153">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="dc920-154">ディスカッション フォーラムでは、行われていればし、しないようにする"yli@example.comとして登録する"から"nolivetto@contoso.com"。</span><span class="sxs-lookup"><span data-stu-id="dc920-154">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="dc920-155">電子メールの確認なし"nolivetto@contoso.com"アプリから不要な電子メールを受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="dc920-155">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="dc920-156">ユーザーが誤ってとして登録されているとします"ylo@example.com""yli"のスペル ミスを認識していなかったとします。</span><span class="sxs-lookup"><span data-stu-id="dc920-156">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="dc920-157">アプリは、正しいメール アドレスがあるないために、パスワードの回復を使用できるでしょう。</span><span class="sxs-lookup"><span data-stu-id="dc920-157">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="dc920-158">確認の電子メールは、ボットからの限られた保護のみを提供します。</span><span class="sxs-lookup"><span data-stu-id="dc920-158">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="dc920-159">確認の電子メールは、多くの電子メール アカウントを使用して悪意のあるユーザーから保護を提供しません。</span><span class="sxs-lookup"><span data-stu-id="dc920-159">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="dc920-160">新しいユーザーが確認された電子メールを受ける前に、web サイトにデータを送信するを防ぐために一般的にします。</span><span class="sxs-lookup"><span data-stu-id="dc920-160">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="dc920-161">Update`ConfigureServices`確認された電子メールを要求します。</span><span class="sxs-lookup"><span data-stu-id="dc920-161">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="dc920-162">`config.SignIn.RequireConfirmedEmail = true;` 登録済みのユーザーで自分の電子メールを確認するまでのログ記録はできません。</span><span class="sxs-lookup"><span data-stu-id="dc920-162">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="dc920-163">電子メール プロバイダーを構成します。</span><span class="sxs-lookup"><span data-stu-id="dc920-163">Configure email provider</span></span>

<span data-ttu-id="dc920-164">このチュートリアルでは、SendGrid が電子メールの送信に使用されます。</span><span class="sxs-lookup"><span data-stu-id="dc920-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="dc920-165">SendGrid アカウントとキーが電子メールを送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc920-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="dc920-166">その他の電子メール プロバイダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="dc920-166">You can use other email providers.</span></span> <span data-ttu-id="dc920-167">ASP.NET Core の 2.x を含む`System.Net.Mail`、アプリから電子メールを送信できます。</span><span class="sxs-lookup"><span data-stu-id="dc920-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="dc920-168">SendGrid または別の電子メール サービスを使用して電子メールを送信することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="dc920-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="dc920-169">SMTP では、セキュリティで保護し、設定を正しく困難です。</span><span class="sxs-lookup"><span data-stu-id="dc920-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="dc920-170">[オプション パターン](xref:fundamentals/configuration/options)ユーザー アカウントとキーの設定にアクセスするために使用します。</span><span class="sxs-lookup"><span data-stu-id="dc920-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="dc920-171">詳細については、次を参照してください。[構成](xref:fundamentals/configuration/index)します。</span><span class="sxs-lookup"><span data-stu-id="dc920-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="dc920-172">電子メールをセキュリティで保護されたキーを取得するためのクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="dc920-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="dc920-173">このサンプルで、`AuthMessageSenderOptions`でクラスを作成、 *Services/AuthMessageSenderOptions.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="dc920-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="dc920-174">設定、`SendGridUser`と`SendGridKey`で、 [secret manager ツール](xref:security/app-secrets)します。</span><span class="sxs-lookup"><span data-stu-id="dc920-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="dc920-175">例えば:</span><span class="sxs-lookup"><span data-stu-id="dc920-175">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="dc920-176">Secret Manager、Windows 上のキー/値のペアが格納、 *secrets.json*ファイル、`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="dc920-176">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="dc920-177">内容、 *secrets.json*ファイルは暗号化されません。</span><span class="sxs-lookup"><span data-stu-id="dc920-177">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="dc920-178">*Secrets.json*ファイルを次に示します (、`SendGridKey`値が削除されました)。</span><span class="sxs-lookup"><span data-stu-id="dc920-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="dc920-179">AuthMessageSenderOptions を使用するスタートアップを構成します。</span><span class="sxs-lookup"><span data-stu-id="dc920-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="dc920-180">追加`AuthMessageSenderOptions`サービス コンテナーの最後に、`ConfigureServices`メソッドで、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="dc920-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc920-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc920-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc920-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc920-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="dc920-183">AuthMessageSender クラスを構成します。</span><span class="sxs-lookup"><span data-stu-id="dc920-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="dc920-184">このチュートリアルは、使用して電子メール通知を追加する方法を示します[SendGrid](https://sendgrid.com/)が SMTP およびその他のメカニズムを使用して電子メールを送信できます。</span><span class="sxs-lookup"><span data-stu-id="dc920-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="dc920-185">インストール、 `SendGrid` NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="dc920-185">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="dc920-186">コマンドライン: から</span><span class="sxs-lookup"><span data-stu-id="dc920-186">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="dc920-187">パッケージ マネージャー コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="dc920-187">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="dc920-188">参照してください[SendGrid を無料で開始する](https://sendgrid.com/free/)無料の SendGrid アカウントを登録します。</span><span class="sxs-lookup"><span data-stu-id="dc920-188">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="dc920-189">SendGrid を構成します。</span><span class="sxs-lookup"><span data-stu-id="dc920-189">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc920-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc920-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="dc920-191">SendGrid を構成するには、次のようなコードを追加*Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="dc920-191">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc920-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc920-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="dc920-193">内のコードを追加*Services/MessageServices.cs* SendGrid を構成するには、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="dc920-193">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="dc920-194">アカウントの確認とパスワードの回復を有効にします。</span><span class="sxs-lookup"><span data-stu-id="dc920-194">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="dc920-195">テンプレートには、アカウントの確認とパスワードの回復用コードがあります。</span><span class="sxs-lookup"><span data-stu-id="dc920-195">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="dc920-196">検索、`OnPostAsync`メソッド*Pages/Account/Register.cshtml.cs*します。</span><span class="sxs-lookup"><span data-stu-id="dc920-196">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc920-197">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc920-197">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="dc920-198">新しく登録されたユーザーが、次の行をコメント アウトによって自動的にログオンされているようにします。</span><span class="sxs-lookup"><span data-stu-id="dc920-198">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="dc920-199">メソッド全体を強調表示されている変更された行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="dc920-199">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc920-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc920-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="dc920-201">アカウントの確認を有効にするのには、次のコードをコメント解除します。</span><span class="sxs-lookup"><span data-stu-id="dc920-201">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="dc920-202">**注:** コードが原因で、新しく登録されたユーザーが、次の行をコメント アウトによって自動的にログオンされています。</span><span class="sxs-lookup"><span data-stu-id="dc920-202">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="dc920-203">内のコードのコメントを解除してパスワードの回復を有効にする、`ForgotPassword`のアクション*Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="dc920-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="dc920-204">フォーム要素をコメント解除します*Views/Account/ForgotPassword.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="dc920-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="dc920-205">削除する必要があります、`<p> For more information on how to enable reset password ... </p>`要素で、この記事へのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="dc920-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="dc920-206">登録、確認の電子メール、およびパスワードのリセット</span><span class="sxs-lookup"><span data-stu-id="dc920-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="dc920-207">Web アプリを実行し、アカウントの確認とパスワードの回復フローをテストします。</span><span class="sxs-lookup"><span data-stu-id="dc920-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="dc920-208">アプリを実行し、新しいユーザーの登録</span><span class="sxs-lookup"><span data-stu-id="dc920-208">Run the app and register a new user</span></span>

  ![Web アプリケーションのアカウントの登録の表示](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="dc920-210">アカウント確認用のリンクは、電子メールを確認します。</span><span class="sxs-lookup"><span data-stu-id="dc920-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="dc920-211">参照してください[デバッグ電子メール](#debug)電子メールが届かない場合。</span><span class="sxs-lookup"><span data-stu-id="dc920-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="dc920-212">電子メールを確認するリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="dc920-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="dc920-213">電子メール アドレスとパスワードでログインします。</span><span class="sxs-lookup"><span data-stu-id="dc920-213">Log in with your email and password.</span></span>
* <span data-ttu-id="dc920-214">ログオフします。</span><span class="sxs-lookup"><span data-stu-id="dc920-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="dc920-215">ビューの管理 ページ</span><span class="sxs-lookup"><span data-stu-id="dc920-215">View the manage page</span></span>

<span data-ttu-id="dc920-216">ブラウザーで、ユーザー名を選択:![ユーザー名を備えたブラウザー ウィンドウ](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="dc920-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="dc920-217">ユーザー名を参照するナビゲーション バーを展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc920-217">You might need to expand the navbar to see user name.</span></span>

![ナビゲーション バー](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc920-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc920-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="dc920-220">管理ページが表示されますが、**プロファイル**タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="dc920-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="dc920-221">**電子メール**電子メールを示すチェック ボックスが確認されているかを示します。</span><span class="sxs-lookup"><span data-stu-id="dc920-221">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![管理 ページ](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc920-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc920-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="dc920-224">これは、チュートリアルの後半で説明しました。</span><span class="sxs-lookup"><span data-stu-id="dc920-224">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="dc920-225">![[管理] ページ](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="dc920-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="dc920-226">テストのパスワードのリセット</span><span class="sxs-lookup"><span data-stu-id="dc920-226">Test password reset</span></span>

* <span data-ttu-id="dc920-227">ログインしている場合は、選択**ログアウト**します。</span><span class="sxs-lookup"><span data-stu-id="dc920-227">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="dc920-228">選択、**ログイン**リンクし、選択、**パスワードを忘れた場合でしょうか。** リンク。</span><span class="sxs-lookup"><span data-stu-id="dc920-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="dc920-229">アカウントを登録するために使用する電子メール アドレスを入力します。</span><span class="sxs-lookup"><span data-stu-id="dc920-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="dc920-230">パスワードをリセットするリンクを含む電子メールが送信されます。</span><span class="sxs-lookup"><span data-stu-id="dc920-230">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="dc920-231">電子メールを確認し、パスワードをリセットするリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="dc920-231">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="dc920-232">パスワードが正常にリセットされると後、はの電子メール アドレスと新しいパスワードを使用してログインすることができます。</span><span class="sxs-lookup"><span data-stu-id="dc920-232">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="dc920-233">電子メールをデバッグします。</span><span class="sxs-lookup"><span data-stu-id="dc920-233">Debug email</span></span>

<span data-ttu-id="dc920-234">電子メールの作業が発生したことはできません: 場合</span><span class="sxs-lookup"><span data-stu-id="dc920-234">If you can't get email working:</span></span>

* <span data-ttu-id="dc920-235">作成、[電子メールを送信するためのコンソール アプリ](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)します。</span><span class="sxs-lookup"><span data-stu-id="dc920-235">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="dc920-236">レビュー、[電子メール アクティビティ](https://sendgrid.com/docs/User_Guide/email_activity.html)ページ。</span><span class="sxs-lookup"><span data-stu-id="dc920-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="dc920-237">迷惑メール フォルダーを確認します。</span><span class="sxs-lookup"><span data-stu-id="dc920-237">Check your spam folder.</span></span>
* <span data-ttu-id="dc920-238">別のメール プロバイダー (Microsoft、Yahoo、Gmail など) に別の電子メール エイリアスをお試しください。</span><span class="sxs-lookup"><span data-stu-id="dc920-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="dc920-239">別のメール アカウントへの送信を再試行してください。</span><span class="sxs-lookup"><span data-stu-id="dc920-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="dc920-240">**セキュリティのベスト プラクティス**は**いない**テストと開発における運用シークレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="dc920-240">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="dc920-241">アプリを Azure に発行する場合は、Web アプリを Azure portal でアプリケーション設定と SendGrid シークレットを設定できます。</span><span class="sxs-lookup"><span data-stu-id="dc920-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="dc920-242">構成システムは、環境変数からキーの読み取りを設定します。</span><span class="sxs-lookup"><span data-stu-id="dc920-242">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="dc920-243">ソーシャル、ローカルのログイン アカウントを組み合わせる</span><span class="sxs-lookup"><span data-stu-id="dc920-243">Combine social and local login accounts</span></span>

<span data-ttu-id="dc920-244">このセクションを完了するには、まず、外部認証プロバイダーを有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc920-244">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="dc920-245">参照してください[Facebook、Google、および外部プロバイダー認証](xref:security/authentication/social/index)します。</span><span class="sxs-lookup"><span data-stu-id="dc920-245">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="dc920-246">電子メールのリンクをクリックして、ローカルおよびソーシャル アカウントを組み合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="dc920-246">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="dc920-247">次の順序で"RickAndMSFT@gmail.com"が最初に、ローカルのログインとして作成ただし、最初に、ソーシャル ログインとしてアカウントを作成し、ローカル ログインを追加します。</span><span class="sxs-lookup"><span data-stu-id="dc920-247">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web アプリケーション:RickAndMSFT@gmail.com認証されたユーザー](accconfirm/_static/rick.png)

<span data-ttu-id="dc920-249">をクリックして、**管理**リンク。</span><span class="sxs-lookup"><span data-stu-id="dc920-249">Click on the **Manage** link.</span></span> <span data-ttu-id="dc920-250">このアカウントに関連付けられた 0 外部 (ソーシャル ログイン) に注意してください。</span><span class="sxs-lookup"><span data-stu-id="dc920-250">Note the 0 external (social logins) associated with this account.</span></span>

![ビューを管理します。](accconfirm/_static/manage.png)

<span data-ttu-id="dc920-252">別のログイン サービスへのリンクをクリックし、アプリの要求をそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="dc920-252">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="dc920-253">次の図では、Facebook は、外部認証プロバイダーは。</span><span class="sxs-lookup"><span data-stu-id="dc920-253">In the following image, Facebook is the external authentication provider:</span></span>

![Facebook を一覧表示する、外部ログイン ビューを管理します。](accconfirm/_static/fb.png)

<span data-ttu-id="dc920-255">2 つのアカウントが統合されました。</span><span class="sxs-lookup"><span data-stu-id="dc920-255">The two accounts have been combined.</span></span> <span data-ttu-id="dc920-256">いずれかのアカウントでログオンすることは。</span><span class="sxs-lookup"><span data-stu-id="dc920-256">You are able to log on with either account.</span></span> <span data-ttu-id="dc920-257">ユーザーがソーシャル ログインの認証サービスがダウンしているか、自分のソーシャル アカウントにアクセスを紛失した可能性が高い場合に、ローカル アカウントを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="dc920-257">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="dc920-258">サイトがユーザー アカウントの確認を有効にします。</span><span class="sxs-lookup"><span data-stu-id="dc920-258">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="dc920-259">ユーザーとサイトのアカウント確認を有効にする既存のすべてのユーザーをロックアウトします。</span><span class="sxs-lookup"><span data-stu-id="dc920-259">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="dc920-260">自分のアカウントが確認されないために、既存のユーザーはロックアウトされます。</span><span class="sxs-lookup"><span data-stu-id="dc920-260">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="dc920-261">既存のユーザーのロックアウトを回避するには、次の方法のいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="dc920-261">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="dc920-262">確認されているすべての既存ユーザーをマークするデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="dc920-262">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="dc920-263">既存のユーザーを確認します。</span><span class="sxs-lookup"><span data-stu-id="dc920-263">Confirm exiting users.</span></span> <span data-ttu-id="dc920-264">確認リンクを含むメールなどのバッチの送信をします。</span><span class="sxs-lookup"><span data-stu-id="dc920-264">For example, batch-send emails with confirmation links.</span></span>
