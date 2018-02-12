---
title: "アカウントの確認と ASP.NET Core でのパスワードの回復"
author: rick-anderson
description: "電子メールの確認とパスワードのリセットと ASP.NET Core アプリケーションをビルドする方法を示します。"
manager: wpickett
ms.author: riande
ms.date: 12/1/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: 14c7fdfc1ed8b87aac8ca937298c7da6373bf06d
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="3703e-103">アカウントの確認と ASP.NET Core でのパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="3703e-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="3703e-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="3703e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="3703e-105">このチュートリアルでは、電子メールの確認とパスワードのリセットと ASP.NET Core アプリケーションをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3703e-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="3703e-106">新しい .NET Core プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="3703e-106">Create a New ASP.NET Core Project</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3703e-107">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3703e-107">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3703e-108">この手順は、Windows 上の Visual Studio に適用されます。</span><span class="sxs-lookup"><span data-stu-id="3703e-108">This step applies to Visual Studio on Windows.</span></span> <span data-ttu-id="3703e-109">CLI 手順については、次のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3703e-109">See the next section for CLI instructions.</span></span>

<span data-ttu-id="3703e-110">チュートリアルでは、Visual Studio 2017 Preview 2 またはそれ以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="3703e-110">The tutorial requires Visual Studio 2017 Preview 2 or later.</span></span>

* <span data-ttu-id="3703e-111">Visual Studio で、新しい Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="3703e-111">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="3703e-112">選択**ASP.NET Core 2.0**です。</span><span class="sxs-lookup"><span data-stu-id="3703e-112">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="3703e-113">次の図ショー **.NET Core**選択すると、選択することができますが、 **.NET Framework**です。</span><span class="sxs-lookup"><span data-stu-id="3703e-113">The following image show **.NET Core** selected, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="3703e-114">選択**認証の変更**に設定および**個々 のユーザー アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="3703e-114">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="3703e-115">既定値を保持**ストア ユーザー アカウントをアプリ内**です。</span><span class="sxs-lookup"><span data-stu-id="3703e-115">Keep the default **Store user accounts in-app**.</span></span>

![新しいプロジェクト ダイアログ ボックスを示す"個別のユーザー アカウント radio"が選択されています。](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3703e-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3703e-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3703e-118">このチュートリアルでは、2017 またはそれ以降の Visual Studio が必要です。</span><span class="sxs-lookup"><span data-stu-id="3703e-118">The tutorial requires Visual Studio 2017 or later.</span></span>

* <span data-ttu-id="3703e-119">Visual Studio で、新しい Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="3703e-119">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="3703e-120">選択**認証の変更**に設定および**個々 のユーザー アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="3703e-120">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>

![新しいプロジェクト ダイアログ ボックスを示す"個別のユーザー アカウント radio"が選択されています。](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a><span data-ttu-id="3703e-122">MacOS および Linux 用 .NET core CLI プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="3703e-122">.NET Core CLI project creation for macOS and Linux</span></span>

<span data-ttu-id="3703e-123">CLI または SQLite を使用している場合は、コマンド ウィンドウで、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="3703e-123">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="3703e-124">`--auth Individual`個々 のユーザー アカウントのテンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="3703e-124">`--auth Individual` specifies the Individual User Accounts template.</span></span>
* <span data-ttu-id="3703e-125">Windows では、追加、`-uld`オプション。</span><span class="sxs-lookup"><span data-stu-id="3703e-125">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="3703e-126">`-uld`オプションは、SQLite DB ではなく、LocalDB 接続文字列を作成します。</span><span class="sxs-lookup"><span data-stu-id="3703e-126">The `-uld` option creates a LocalDB connection string rather than a SQLite DB.</span></span>
* <span data-ttu-id="3703e-127">実行`new mvc --help`にこのコマンドのヘルプを表示します。</span><span class="sxs-lookup"><span data-stu-id="3703e-127">Run `new mvc --help` to get help on this command.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="3703e-128">テストの新規ユーザーの登録</span><span class="sxs-lookup"><span data-stu-id="3703e-128">Test new user registration</span></span>

<span data-ttu-id="3703e-129">アプリを実行する、選択、**登録**リンク、およびユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="3703e-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="3703e-130">Entity Framework Core 移行を実行する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="3703e-130">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="3703e-131">この時点では、電子メールにのみ、検証、 [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)属性。</span><span class="sxs-lookup"><span data-stu-id="3703e-131">At this  point, the only validation on the email is with the [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="3703e-132">登録を送信した後、アプリにログインします。</span><span class="sxs-lookup"><span data-stu-id="3703e-132">After you submit the registration, you are logged into the app.</span></span> <span data-ttu-id="3703e-133">、このチュートリアルで後ほどお変更この電子メールが検証されるまで新規ユーザーがログインできないようにします。</span><span class="sxs-lookup"><span data-stu-id="3703e-133">Later in the tutorial, we'll change this so new users cannot log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="3703e-134">ユーザー データベースを表示します。</span><span class="sxs-lookup"><span data-stu-id="3703e-134">View the Identity database</span></span>

# <a name="sql-servertabsql-server"></a>[<span data-ttu-id="3703e-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3703e-135">SQL Server</span></span>](#tab/sql-server)

* <span data-ttu-id="3703e-136">**ビュー**メニューの  **SQL Server オブジェクト エクスプ ローラー** (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="3703e-136">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span> 
* <span data-ttu-id="3703e-137">移動**(localdb) MSSQLLocalDB (SQL Server 13)**です。</span><span class="sxs-lookup"><span data-stu-id="3703e-137">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="3703e-138">右クリックして**dbo します。AspNetUsers** > **データ表示**:</span><span class="sxs-lookup"><span data-stu-id="3703e-138">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![SQL Server オブジェクト エクスプ ローラー AspNetUsers テーブルのコンテキスト メニュー](accconfirm/_static/ssox.png)

<span data-ttu-id="3703e-140">注、`EmailConfirmed`フィールドは`False`します。</span><span class="sxs-lookup"><span data-stu-id="3703e-140">Note the `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="3703e-141">アプリが確認の電子メールを送信するとき、次の手順でこの電子メールを再利用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3703e-141">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="3703e-142">クリックし、行を右クリックして**削除**です。</span><span class="sxs-lookup"><span data-stu-id="3703e-142">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="3703e-143">電子メールのエイリアスが、削除はで容易に、次の手順です。</span><span class="sxs-lookup"><span data-stu-id="3703e-143">Deleting the email alias now will make it easier in the following steps.</span></span>

# <a name="sqlitetabsqlite"></a>[<span data-ttu-id="3703e-144">SQLite</span><span class="sxs-lookup"><span data-stu-id="3703e-144">SQLite</span></span>](#tab/sqlite)

<span data-ttu-id="3703e-145">参照してください[ASP.NET Core MVC プロジェクトで作業して SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql) SQLite DB を表示する方法についてはします。</span><span class="sxs-lookup"><span data-stu-id="3703e-145">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite DB.</span></span> 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a><span data-ttu-id="3703e-146">SSL を要求し、SSL 用の IIS Express のセットアップ</span><span class="sxs-lookup"><span data-stu-id="3703e-146">Require SSL and setup IIS Express for SSL</span></span>

<span data-ttu-id="3703e-147">参照してください[強制 SSL](xref:security/enforcing-ssl)です。</span><span class="sxs-lookup"><span data-stu-id="3703e-147">See [Enforcing SSL](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="3703e-148">確認の電子メールが必要</span><span class="sxs-lookup"><span data-stu-id="3703e-148">Require email confirmation</span></span>

<span data-ttu-id="3703e-149">他のユーザー偽装はいないしていることを確認する新規ユーザーの登録の電子メール アドレスを確認することをお勧め (つまり、まだに登録されている他のユーザーの電子メール)。</span><span class="sxs-lookup"><span data-stu-id="3703e-149">It's a best practice to confirm the email of a new user registration to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="3703e-150">ディスカッション フォーラムいてしないようにすると仮定します"yli@example.com"として登録する"fromnolivetto@contoso.com"。</span><span class="sxs-lookup"><span data-stu-id="3703e-150">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="3703e-151">電子メールの確認なし"nolivetto@contoso.com"アプリから不要な電子メールを取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3703e-151">Without email confirmation, "nolivetto@contoso.com" could get unwanted email from your app.</span></span> <span data-ttu-id="3703e-152">ユーザーが誤ってとして登録されていると仮定します"ylo@example.com"ミススペルを認識していないと"yli、"ができなく、アプリは、正しいメール アドレスがある見つからないために、パスワードの回復を使用します。</span><span class="sxs-lookup"><span data-stu-id="3703e-152">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli," they wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="3703e-153">確認の電子メールは、bot から制限の保護のみを提供し、登録に使用できる多くの作業電子メール エイリアスを持つ判別スパム攻撃者から保護を提供しません。</span><span class="sxs-lookup"><span data-stu-id="3703e-153">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers who have many working email aliases they can use to register.</span></span>

<span data-ttu-id="3703e-154">一般にする新しいユーザーが確認された電子メールがある前に、web サイトにデータを投稿するを防ぐ。</span><span class="sxs-lookup"><span data-stu-id="3703e-154">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span> 

<span data-ttu-id="3703e-155">更新`ConfigureServices`確認電子メールを要求します。</span><span class="sxs-lookup"><span data-stu-id="3703e-155">Update `ConfigureServices` to require a confirmed email:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3703e-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3703e-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3703e-157">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3703e-157">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
<span data-ttu-id="3703e-158">前の行では、登録済みユーザーが自分の電子メールが確認されるまでには記録できなくなります。</span><span class="sxs-lookup"><span data-stu-id="3703e-158">The preceding line prevents registered users from being logged in until their email is confirmed.</span></span> <span data-ttu-id="3703e-159">ただし、その行によって妨げられない新しいユーザーを登録した後に記録されています。</span><span class="sxs-lookup"><span data-stu-id="3703e-159">However, that line doesn't prevent new users from being logged in after they register.</span></span> <span data-ttu-id="3703e-160">既定のコードは、登録した後でユーザーを記録します。</span><span class="sxs-lookup"><span data-stu-id="3703e-160">The default code logs in a user after they register.</span></span> <span data-ttu-id="3703e-161">ログアウトすると、登録するまで再ログインすることはできません。</span><span class="sxs-lookup"><span data-stu-id="3703e-161">Once they log out, they won't be able to log in again until they register.</span></span> <span data-ttu-id="3703e-162">コードのために新規登録されたユーザーは、変更をこのチュートリアルで後ほど**いない**ログに記録します。</span><span class="sxs-lookup"><span data-stu-id="3703e-162">Later in the tutorial we'll change the code so newly registered user are **not** logged in.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="3703e-163">電子メール プロバイダーを構成します。</span><span class="sxs-lookup"><span data-stu-id="3703e-163">Configure email provider</span></span>

<span data-ttu-id="3703e-164">このチュートリアルでは、SendGrid を使用して電子メールを送信します。</span><span class="sxs-lookup"><span data-stu-id="3703e-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="3703e-165">SendGrid アカウントと電子メールを送信するキーが必要です。</span><span class="sxs-lookup"><span data-stu-id="3703e-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="3703e-166">その他の電子メール プロバイダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="3703e-166">You can use other email providers.</span></span> <span data-ttu-id="3703e-167">ASP.NET Core 2.x が含まれています`System.Net.Mail`、アプリから電子メールを送信することができます。</span><span class="sxs-lookup"><span data-stu-id="3703e-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="3703e-168">SendGrid または別の電子メール サービスを使用して電子メールを送信することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3703e-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="3703e-169">SMTP は適切に設定してセキュリティで保護するが困難です。</span><span class="sxs-lookup"><span data-stu-id="3703e-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="3703e-170">[オプション パターン](xref:fundamentals/configuration/options)ユーザー アカウントとキーの設定にアクセスするために使用します。</span><span class="sxs-lookup"><span data-stu-id="3703e-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="3703e-171">詳細については、次を参照してください。[構成](xref:fundamentals/configuration/index)です。</span><span class="sxs-lookup"><span data-stu-id="3703e-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="3703e-172">セキュリティで保護された電子メールのキーを取得するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="3703e-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="3703e-173">このサンプルで、`AuthMessageSenderOptions`でクラスを作成、 *Services/AuthMessageSenderOptions.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3703e-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="3703e-174">設定、`SendGridUser`と`SendGridKey`で、[シークレット マネージャー ツール](../app-secrets.md)です。</span><span class="sxs-lookup"><span data-stu-id="3703e-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](../app-secrets.md).</span></span> <span data-ttu-id="3703e-175">例:</span><span class="sxs-lookup"><span data-stu-id="3703e-175">For example:</span></span>

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="3703e-176">Windows では、シークレット マネージャーが、キーと値のペアを格納、 *secrets.json* %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId > ディレクトリ内のファイルです。</span><span class="sxs-lookup"><span data-stu-id="3703e-176">On Windows, Secret Manager stores your keys/value pairs in a *secrets.json* file in the %APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId> directory.</span></span>

<span data-ttu-id="3703e-177">内容、 *secrets.json*ファイルが暗号化されていません。</span><span class="sxs-lookup"><span data-stu-id="3703e-177">The contents of the *secrets.json* file are not encrypted.</span></span> <span data-ttu-id="3703e-178">*Secrets.json*ファイルを次に示します (、`SendGridKey`値が削除されました)。</span><span class="sxs-lookup"><span data-stu-id="3703e-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="3703e-179">AuthMessageSenderOptions を使用するスタートアップを構成します。</span><span class="sxs-lookup"><span data-stu-id="3703e-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="3703e-180">追加`AuthMessageSenderOptions`サービス コンテナーの最後に、`ConfigureServices`メソッドで、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3703e-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3703e-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3703e-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3703e-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3703e-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="3703e-183">AuthMessageSender クラスを構成します。</span><span class="sxs-lookup"><span data-stu-id="3703e-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="3703e-184">このチュートリアルを使用して電子メール通知を追加する方法を示しています。 [SendGrid](https://sendgrid.com/)、SMTP、およびその他のメカニズムを使用して電子メールを送信することができますが、します。</span><span class="sxs-lookup"><span data-stu-id="3703e-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

* <span data-ttu-id="3703e-185">インストール、 `SendGrid` NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="3703e-185">Install the `SendGrid` NuGet package.</span></span> <span data-ttu-id="3703e-186">パッケージ マネージャー コンソールから、次を入力して、次のコマンド。</span><span class="sxs-lookup"><span data-stu-id="3703e-186">From the Package Manager Console,  enter the following the following command:</span></span>

  `Install-Package SendGrid`

* <span data-ttu-id="3703e-187">参照してください[始める SendGrid 無料](https://sendgrid.com/free/)無料の SendGrid アカウントを登録します。</span><span class="sxs-lookup"><span data-stu-id="3703e-187">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="3703e-188">SendGrid を構成します。</span><span class="sxs-lookup"><span data-stu-id="3703e-188">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3703e-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3703e-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="3703e-190">コードを追加*Services/EmailSender.cs* SendGrid を構成するには、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="3703e-190">Add code in *Services/EmailSender.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3703e-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3703e-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="3703e-192">コードを追加*Services/MessageServices.cs* SendGrid を構成するには、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="3703e-192">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="3703e-193">アカウントの確認とパスワードの回復を有効にします。</span><span class="sxs-lookup"><span data-stu-id="3703e-193">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="3703e-194">テンプレートは、アカウントの確認とパスワードの回復用コードを持っています。</span><span class="sxs-lookup"><span data-stu-id="3703e-194">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="3703e-195">検索、`[HttpPost] Register`メソッドで、 *AccountController.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3703e-195">Find the `[HttpPost] Register` method in the  *AccountController.cs* file.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3703e-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3703e-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3703e-197">新しく登録されたユーザーが次の行をコメント アウトして自動ログオンされているようにします。</span><span class="sxs-lookup"><span data-stu-id="3703e-197">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="3703e-198">メソッド全体を強調表示されている変更された行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3703e-198">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

<span data-ttu-id="3703e-199">注: 上記のコードは失敗を実装する場合`IEmailSender`とテキスト形式の電子メールを送信します。</span><span class="sxs-lookup"><span data-stu-id="3703e-199">Note: The previous code will fail if you implement `IEmailSender` and send a plain text email.</span></span> <span data-ttu-id="3703e-200">参照してください[この問題](https://github.com/aspnet/Home/issues/2152)の詳細については、問題を回避します。</span><span class="sxs-lookup"><span data-stu-id="3703e-200">See [this issue](https://github.com/aspnet/Home/issues/2152) for more information and a workaround.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3703e-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3703e-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3703e-202">アカウントの確認を有効にするコードのコメントを解除します。</span><span class="sxs-lookup"><span data-stu-id="3703e-202">Uncomment the code to enable account confirmation.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="3703e-203">注: お中もにより、新しく登録されたユーザー、次の行をコメント アウトして自動ログオンされています。</span><span class="sxs-lookup"><span data-stu-id="3703e-203">Note: We're also preventing a newly-registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="3703e-204">内のコードのコメントを解除してパスワードの回復を有効にする、`ForgotPassword`アクションで、 *Controllers/AccountController.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3703e-204">Enable password recovery by uncommenting the code in the `ForgotPassword` action in the *Controllers/AccountController.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="3703e-205">フォーム要素をコメントから*Views/Account/ForgotPassword.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="3703e-205">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="3703e-206">削除する必要があります、`<p> For more information on how to enable reset password ... </p>`この資料へのリンクを含む要素です。</span><span class="sxs-lookup"><span data-stu-id="3703e-206">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element which contains a link to this article.</span></span>

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="3703e-207">登録、確認電子メール、およびパスワードのリセット</span><span class="sxs-lookup"><span data-stu-id="3703e-207">Register, confirm email, and reset password</span></span>

<span data-ttu-id="3703e-208">Web アプリを実行し、アカウントの確認とパスワードの回復フローをテストします。</span><span class="sxs-lookup"><span data-stu-id="3703e-208">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="3703e-209">アプリを実行して、新しいユーザーの登録</span><span class="sxs-lookup"><span data-stu-id="3703e-209">Run the app and register a new user</span></span>

 ![Web アプリケーション取引明細の表示](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="3703e-211">アカウントの確認リンクには、電子メールを確認してください。</span><span class="sxs-lookup"><span data-stu-id="3703e-211">Check your email for the account confirmation link.</span></span> <span data-ttu-id="3703e-212">参照してください[電子メールをデバッグ](#debug)電子メールを取得しない場合。</span><span class="sxs-lookup"><span data-stu-id="3703e-212">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="3703e-213">確認の電子メールへのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3703e-213">Click the link to confirm your email.</span></span>
* <span data-ttu-id="3703e-214">電子メール アドレスとパスワードでログインします。</span><span class="sxs-lookup"><span data-stu-id="3703e-214">Log in with your email and password.</span></span>
* <span data-ttu-id="3703e-215">ログオフします。</span><span class="sxs-lookup"><span data-stu-id="3703e-215">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="3703e-216">ビューの管理 ページ</span><span class="sxs-lookup"><span data-stu-id="3703e-216">View the manage page</span></span>

<span data-ttu-id="3703e-217">ブラウザーで、ユーザー名を選択:![ブラウザー ウィンドウでユーザー名](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="3703e-217">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="3703e-218">ユーザー名を確認して、ナビゲーションバーを展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3703e-218">You might need to expand the navbar to see user name.</span></span>

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3703e-220">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3703e-220">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3703e-221">管理 ページが表示され、**プロファイル**タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="3703e-221">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="3703e-222">**電子メール**電子メールを示すチェック ボックスが確認されているかを示します。</span><span class="sxs-lookup"><span data-stu-id="3703e-222">The **Email** shows a check box indicating the email has been confirmed.</span></span> 

![[管理] ページ](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3703e-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3703e-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3703e-225">このチュートリアルで後ほどこのページについて説明します。</span><span class="sxs-lookup"><span data-stu-id="3703e-225">We'll talk about this page later in the tutorial.</span></span>
<span data-ttu-id="3703e-226">![[管理] ページ](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="3703e-226">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="3703e-227">テストのパスワードのリセット</span><span class="sxs-lookup"><span data-stu-id="3703e-227">Test password reset</span></span>

* <span data-ttu-id="3703e-228">ログインしている場合は、選択**ログアウト**です。</span><span class="sxs-lookup"><span data-stu-id="3703e-228">If you're logged in, select **Logout**.</span></span>  
* <span data-ttu-id="3703e-229">選択、**ログイン**リンクを選択して、**パスワードを忘れた場合ですか?**リンクします。</span><span class="sxs-lookup"><span data-stu-id="3703e-229">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="3703e-230">アカウントの登録に使用したメール アドレスを入力します。</span><span class="sxs-lookup"><span data-stu-id="3703e-230">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="3703e-231">パスワードをリセットするリンクを含む電子メールが送信されます。</span><span class="sxs-lookup"><span data-stu-id="3703e-231">An email with a link to reset your password will be sent.</span></span> <span data-ttu-id="3703e-232">電子メールを確認し、パスワードをリセットするリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3703e-232">Check your email and click the link to reset your password.</span></span>  <span data-ttu-id="3703e-233">パスワードが正常にリセットされると、後に、電子メールと新しいパスワードでログインすることができます。</span><span class="sxs-lookup"><span data-stu-id="3703e-233">After your password has been successfully reset, you can login with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="3703e-234">電子メールをデバッグします。</span><span class="sxs-lookup"><span data-stu-id="3703e-234">Debug email</span></span>

<span data-ttu-id="3703e-235">電子メールの作業を取得できません: 場合</span><span class="sxs-lookup"><span data-stu-id="3703e-235">If you can't get email working:</span></span>

* <span data-ttu-id="3703e-236">確認、[電子メール アクティビティ](https://sendgrid.com/docs/User_Guide/email_activity.html)ページ。</span><span class="sxs-lookup"><span data-stu-id="3703e-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="3703e-237">迷惑メール フォルダーをチェックします。</span><span class="sxs-lookup"><span data-stu-id="3703e-237">Check your spam folder.</span></span>
* <span data-ttu-id="3703e-238">別の電子メール プロバイダー (Microsoft、yahoo!、Gmail など) に別の電子メール エイリアスを再試行してください。</span><span class="sxs-lookup"><span data-stu-id="3703e-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="3703e-239">作成、[電子メールを送信するコンソール アプリ](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)です。</span><span class="sxs-lookup"><span data-stu-id="3703e-239">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="3703e-240">別の電子メール アカウントへの送信を再試行してください。</span><span class="sxs-lookup"><span data-stu-id="3703e-240">Try sending to different email accounts.</span></span>

<span data-ttu-id="3703e-241">**注:**セキュリティのベスト プラクティスは、開発、テストや実稼働の機密情報を使用しないようにします。</span><span class="sxs-lookup"><span data-stu-id="3703e-241">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="3703e-242">アプリを Azure に発行する場合は、Web アプリの Azure ポータルでのアプリケーション設定として、SendGrid の機密情報を設定できます。</span><span class="sxs-lookup"><span data-stu-id="3703e-242">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="3703e-243">構成システム環境変数からキーを読み取るをセットアップしています。</span><span class="sxs-lookup"><span data-stu-id="3703e-243">The configuration system is setup to read keys from environment variables.</span></span>

## <a name="prevent-login-at-registration"></a><span data-ttu-id="3703e-244">登録時にログインをできません。</span><span class="sxs-lookup"><span data-stu-id="3703e-244">Prevent login at registration</span></span>

<span data-ttu-id="3703e-245">現在のテンプレートを使用してユーザーには、登録フォームが完了すると、ログインしています (認証)。</span><span class="sxs-lookup"><span data-stu-id="3703e-245">With the current templates, once a user completes the registration form, they're logged in (authenticated).</span></span> <span data-ttu-id="3703e-246">ログを記録する前に自分の電子メールを確認する一般にできます。</span><span class="sxs-lookup"><span data-stu-id="3703e-246">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="3703e-247">以下のセクションを必要とするコードを変更しましたポータルにログインする前に、新しいユーザーが確認された電子メールをあります。</span><span class="sxs-lookup"><span data-stu-id="3703e-247">In the section below, we will modify the code to require new users have a confirmed email before they're logged in.</span></span> <span data-ttu-id="3703e-248">更新プログラム、`[HttpPost] Login`アクションで、 *AccountController.cs*の次の強調表示されている変更されたファイルです。</span><span class="sxs-lookup"><span data-stu-id="3703e-248">Update the `[HttpPost] Login` action in the *AccountController.cs* file with the following highlighted changes.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

<span data-ttu-id="3703e-249">**注:**セキュリティのベスト プラクティスは、開発、テストや実稼働の機密情報を使用しないようにします。</span><span class="sxs-lookup"><span data-stu-id="3703e-249">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="3703e-250">アプリを Azure に発行する場合は、Web アプリの Azure ポータルでのアプリケーション設定として、SendGrid の機密情報を設定できます。</span><span class="sxs-lookup"><span data-stu-id="3703e-250">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="3703e-251">構成システム環境変数からキーを読み取るをセットアップしています。</span><span class="sxs-lookup"><span data-stu-id="3703e-251">The configuration system is setup to read keys from environment variables.</span></span>


## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="3703e-252">ソーシャルとローカルのログイン アカウントを結合します。</span><span class="sxs-lookup"><span data-stu-id="3703e-252">Combine social and local login accounts</span></span>

<span data-ttu-id="3703e-253">注: このセクションでは、ASP.NET Core にのみ適用されます 1.x です。</span><span class="sxs-lookup"><span data-stu-id="3703e-253">Note: This section applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="3703e-254">Asp.net 2.x のコアは、「[この](https://github.com/aspnet/Docs/issues/3753)問題です。</span><span class="sxs-lookup"><span data-stu-id="3703e-254">For ASP.NET Core 2.x, see [this](https://github.com/aspnet/Docs/issues/3753) issue.</span></span>

<span data-ttu-id="3703e-255">このセクションを完了するには、まず外部認証プロバイダーを有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="3703e-255">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="3703e-256">参照してください[Facebook、Google やその他の外部プロバイダーを使用して有効にする認証](social/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="3703e-256">See [Enabling authentication using Facebook, Google and other external providers](social/index.md).</span></span>

<span data-ttu-id="3703e-257">電子メールのリンクをクリックすると、ローカルおよびソーシャルのアカウントを組み合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="3703e-257">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="3703e-258">次の順序で"RickAndMSFT@gmail.com"が最初に、ローカルのログインとして作成ただし、最初に、ソーシャル ログインとしてアカウントを作成し、ローカル ログインを追加します。</span><span class="sxs-lookup"><span data-stu-id="3703e-258">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web アプリケーション:RickAndMSFT@gmail.com認証されたユーザー](accconfirm/_static/rick.png)

<span data-ttu-id="3703e-260">をクリックして、**管理**リンクします。</span><span class="sxs-lookup"><span data-stu-id="3703e-260">Click on the **Manage** link.</span></span> <span data-ttu-id="3703e-261">このアカウントに関連付けられた 0 外部 (ソーシャル ログイン) に注意してください。</span><span class="sxs-lookup"><span data-stu-id="3703e-261">Note the 0 external (social logins) associated with this account.</span></span>

![ビューを管理します。](accconfirm/_static/manage.png)

<span data-ttu-id="3703e-263">別のログイン サービスへのリンクをクリックし、アプリの要求を許可します。</span><span class="sxs-lookup"><span data-stu-id="3703e-263">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="3703e-264">次の図では、Facebook は、外部認証プロバイダーを示します。</span><span class="sxs-lookup"><span data-stu-id="3703e-264">In the image below, Facebook is the external authentication provider:</span></span>

![Facebook を一覧表示する、外部ログイン ビューを管理します。](accconfirm/_static/fb.png)

<span data-ttu-id="3703e-266">2 つのアカウントが統合されました。</span><span class="sxs-lookup"><span data-stu-id="3703e-266">The two accounts have been combined.</span></span> <span data-ttu-id="3703e-267">いずれかのアカウントでログオンすることができます。</span><span class="sxs-lookup"><span data-stu-id="3703e-267">You will be able to log on with either account.</span></span> <span data-ttu-id="3703e-268">ユーザー認証サービスのソーシャル、ログがダウンしているか、ソーシャル アカウントにアクセスを紛失した可能性が高い場合に、ローカル アカウントを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="3703e-268">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they've lost access to their social account.</span></span>
