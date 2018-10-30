---
title: アカウントの確認と ASP.NET Core でのパスワードの回復
author: rick-anderson
description: 電子メールの確認とパスワードのリセットと ASP.NET Core アプリを構築する方法について説明します。
ms.author: riande
ms.date: 7/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 1fae5af24359afc991a30cd2b8e2f6927845962b
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244802"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="36b08-103">アカウントの確認と ASP.NET Core でのパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="36b08-103">Account confirmation and password recovery in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="36b08-104">参照してください[この PDF ファイル](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core 1.1 と version 2.1 です。</span><span class="sxs-lookup"><span data-stu-id="36b08-104">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="36b08-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="36b08-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="36b08-106">このチュートリアルでは、電子メールの確認とパスワードのリセットと ASP.NET Core アプリをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="36b08-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="36b08-107">このチュートリアルは**いない**先頭トピック。</span><span class="sxs-lookup"><span data-stu-id="36b08-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="36b08-108">理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="36b08-108">You should be familiar with:</span></span>

* [<span data-ttu-id="36b08-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="36b08-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="36b08-110">認証</span><span class="sxs-lookup"><span data-stu-id="36b08-110">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="36b08-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="36b08-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="36b08-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="36b08-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="36b08-113">Web アプリを作成し、Identity のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="36b08-113">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="36b08-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="36b08-114">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="36b08-115">Visual Studio で、作成、新しい**Web アプリケーション**という名前のプロジェクト**WebPWrecover**します。</span><span class="sxs-lookup"><span data-stu-id="36b08-115">In Visual Studio, create a new **Web Application** project named **WebPWrecover**.</span></span>
* <span data-ttu-id="36b08-116">選択**ASP.NET Core 2.1**します。</span><span class="sxs-lookup"><span data-stu-id="36b08-116">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="36b08-117">既定値を保持**認証**設定**認証なし**します。</span><span class="sxs-lookup"><span data-stu-id="36b08-117">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="36b08-118">認証は、次の手順で追加されます。</span><span class="sxs-lookup"><span data-stu-id="36b08-118">Authentication is added in the next step.</span></span>

<span data-ttu-id="36b08-119">次の手順。</span><span class="sxs-lookup"><span data-stu-id="36b08-119">In the next step:</span></span>

* <span data-ttu-id="36b08-120">レイアウト ページを設定 *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="36b08-120">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="36b08-121">選択*アカウント/登録*</span><span class="sxs-lookup"><span data-stu-id="36b08-121">Select *Account/Register*</span></span>
* <span data-ttu-id="36b08-122">新規作成**データ コンテキスト クラス**</span><span class="sxs-lookup"><span data-stu-id="36b08-122">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="36b08-123">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="36b08-123">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet new webapp -o WebPWrecover
cd WebPWrecover
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -fi Account.Register -dc WebPWrecover.Models.WebPWrecoverContext
dotnet ef migrations add CreateIdentitySchema
dotnet ef database drop -f
dotnet ef database update
dotnet build
```

<span data-ttu-id="36b08-124">実行`dotnet aspnet-codegenerator identity --help`スキャフォールディング ツールのヘルプを表示します。</span><span class="sxs-lookup"><span data-stu-id="36b08-124">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="36b08-125">指示に従って[認証を有効に](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="36b08-125">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="36b08-126">追加`app.UseAuthentication();`に `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="36b08-126">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="36b08-127">追加`<partial name="_LoginPartial" />`レイアウト ファイルにします。</span><span class="sxs-lookup"><span data-stu-id="36b08-127">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="36b08-128">新規ユーザー登録をテストします。</span><span class="sxs-lookup"><span data-stu-id="36b08-128">Test new user registration</span></span>

<span data-ttu-id="36b08-129">アプリを実行し、選択、**登録**リンク、およびユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="36b08-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="36b08-130">この時点では、電子メールの検証のみ、 [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)属性。</span><span class="sxs-lookup"><span data-stu-id="36b08-130">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="36b08-131">登録を送信した後は、アプリにログインします。</span><span class="sxs-lookup"><span data-stu-id="36b08-131">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="36b08-132">チュートリアルの後半では、新しいユーザーが自分の電子メールが検証されるまでにログインできないため、コードが更新されます。</span><span class="sxs-lookup"><span data-stu-id="36b08-132">Later in the tutorial, the code is updated so new users can't log in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="36b08-133">テーブルに注意してください`EmailConfirmed`フィールドは`False`します。</span><span class="sxs-lookup"><span data-stu-id="36b08-133">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="36b08-134">アプリが送信された確認メールを送信するとき、次の手順でこの電子メールをもう一度使用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="36b08-134">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="36b08-135">クリックし、行を右クリックして**削除**します。</span><span class="sxs-lookup"><span data-stu-id="36b08-135">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="36b08-136">電子メール エイリアスを削除する簡単で、次の手順。</span><span class="sxs-lookup"><span data-stu-id="36b08-136">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="36b08-137">確認の電子メールが必要です。</span><span class="sxs-lookup"><span data-stu-id="36b08-137">Require email confirmation</span></span>

<span data-ttu-id="36b08-138">新規ユーザー登録の電子メール アドレスを確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="36b08-138">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="36b08-139">電子メールの他のユーザーが偽装はいないしていることを確認することを確認できます (つまりに登録していない他のユーザーの電子メールで)。</span><span class="sxs-lookup"><span data-stu-id="36b08-139">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="36b08-140">ディスカッション フォーラムでは、行われていればし、しないようにする"yli@example.comとして登録する"から"nolivetto@contoso.com"。</span><span class="sxs-lookup"><span data-stu-id="36b08-140">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="36b08-141">電子メールの確認なし"nolivetto@contoso.com"アプリから不要な電子メールを受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="36b08-141">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="36b08-142">ユーザーが誤ってとして登録されているとします"ylo@example.com""yli"のスペル ミスを認識していなかったとします。</span><span class="sxs-lookup"><span data-stu-id="36b08-142">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="36b08-143">アプリは、正しいメール アドレスがあるないために、パスワードの回復を使用できるでしょう。</span><span class="sxs-lookup"><span data-stu-id="36b08-143">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="36b08-144">確認の電子メールは、ボットからの限られた保護を提供します。</span><span class="sxs-lookup"><span data-stu-id="36b08-144">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="36b08-145">確認の電子メールは、多くの電子メール アカウントを使用して悪意のあるユーザーから保護を提供しません。</span><span class="sxs-lookup"><span data-stu-id="36b08-145">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="36b08-146">新しいユーザーが確認された電子メールを受ける前に、web サイトにデータを送信するを防ぐために一般的にします。</span><span class="sxs-lookup"><span data-stu-id="36b08-146">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="36b08-147">Update *Areas/Identity/IdentityHostingStartup.cs*確認された電子メールを要求します。</span><span class="sxs-lookup"><span data-stu-id="36b08-147">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="36b08-148">`config.SignIn.RequireConfirmedEmail = true;` 登録済みのユーザーで自分の電子メールを確認するまでのログ記録はできません。</span><span class="sxs-lookup"><span data-stu-id="36b08-148">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="36b08-149">電子メール プロバイダーを構成します。</span><span class="sxs-lookup"><span data-stu-id="36b08-149">Configure email provider</span></span>

<span data-ttu-id="36b08-150">このチュートリアルで[SendGrid](https://sendgrid.com)電子メールを送信するために使用します。</span><span class="sxs-lookup"><span data-stu-id="36b08-150">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="36b08-151">SendGrid アカウントとキーが電子メールを送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="36b08-151">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="36b08-152">その他の電子メール プロバイダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="36b08-152">You can use other email providers.</span></span> <span data-ttu-id="36b08-153">ASP.NET Core の 2.x を含む`System.Net.Mail`、アプリから電子メールを送信できます。</span><span class="sxs-lookup"><span data-stu-id="36b08-153">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="36b08-154">SendGrid または別の電子メール サービスを使用して電子メールを送信することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="36b08-154">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="36b08-155">SMTP では、セキュリティで保護し、設定を正しく困難です。</span><span class="sxs-lookup"><span data-stu-id="36b08-155">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="36b08-156">電子メールをセキュリティで保護されたキーを取得するためのクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="36b08-156">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="36b08-157">このサンプルでは、次のように作成します*Services/AuthMessageSenderOptions.cs*:。</span><span class="sxs-lookup"><span data-stu-id="36b08-157">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="36b08-158">SendGrid のユーザーの機密情報を構成します。</span><span class="sxs-lookup"><span data-stu-id="36b08-158">Configure SendGrid user secrets</span></span>

<span data-ttu-id="36b08-159">一意な追加`<UserSecretsId>`値を`<PropertyGroup>`プロジェクト ファイルの要素。</span><span class="sxs-lookup"><span data-stu-id="36b08-159">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="36b08-160">設定、`SendGridUser`と`SendGridKey`で、 [secret manager ツール](xref:security/app-secrets)します。</span><span class="sxs-lookup"><span data-stu-id="36b08-160">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="36b08-161">例えば:</span><span class="sxs-lookup"><span data-stu-id="36b08-161">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="36b08-162">Secret Manager、Windows 上のキー/値のペアが格納、 *secrets.json*ファイル、`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="36b08-162">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="36b08-163">内容、 *secrets.json*ファイルは暗号化されません。</span><span class="sxs-lookup"><span data-stu-id="36b08-163">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="36b08-164">*Secrets.json*ファイルを次に示します (、`SendGridKey`値が削除されました)。</span><span class="sxs-lookup"><span data-stu-id="36b08-164">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```
 
<span data-ttu-id="36b08-165">詳細については、次を参照してください。、[オプション パターン](xref:fundamentals/configuration/options)と[構成](xref:fundamentals/configuration/index)します。</span><span class="sxs-lookup"><span data-stu-id="36b08-165">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="36b08-166">SendGrid をインストールします。</span><span class="sxs-lookup"><span data-stu-id="36b08-166">Install SendGrid</span></span>

<span data-ttu-id="36b08-167">このチュートリアルは、使用して電子メール通知を追加する方法を示します[SendGrid](https://sendgrid.com/)が SMTP およびその他のメカニズムを使用して電子メールを送信できます。</span><span class="sxs-lookup"><span data-stu-id="36b08-167">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="36b08-168">インストール、 `SendGrid` NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="36b08-168">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="36b08-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="36b08-169">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="36b08-170">パッケージ マネージャー コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="36b08-170">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="36b08-171">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="36b08-171">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="36b08-172">コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="36b08-172">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="36b08-173">参照してください[SendGrid を無料で開始する](https://sendgrid.com/free/)無料の SendGrid アカウントを登録します。</span><span class="sxs-lookup"><span data-stu-id="36b08-173">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="36b08-174">IEmailSender を実装します。</span><span class="sxs-lookup"><span data-stu-id="36b08-174">Implement IEmailSender</span></span>

<span data-ttu-id="36b08-175">実装`IEmailSender`、作成*Services/EmailSender.cs*次のようなコードで。</span><span class="sxs-lookup"><span data-stu-id="36b08-175">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="36b08-176">電子メールをサポートするために起動時を構成します。</span><span class="sxs-lookup"><span data-stu-id="36b08-176">Configure startup to support email</span></span>

<span data-ttu-id="36b08-177">次のコードを追加、`ConfigureServices`メソッドで、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="36b08-177">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="36b08-178">追加`EmailSender`シングルトン サービスとして。</span><span class="sxs-lookup"><span data-stu-id="36b08-178">Add `EmailSender` as a singleton service.</span></span>
* <span data-ttu-id="36b08-179">登録、`AuthMessageSenderOptions`構成インスタンス。</span><span class="sxs-lookup"><span data-stu-id="36b08-179">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="36b08-180">アカウントの確認とパスワードの回復を有効にします。</span><span class="sxs-lookup"><span data-stu-id="36b08-180">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="36b08-181">テンプレートには、アカウントの確認とパスワードの回復用コードがあります。</span><span class="sxs-lookup"><span data-stu-id="36b08-181">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="36b08-182">検索、`OnPostAsync`メソッド*Areas/Identity/Pages/Account/Register.cshtml.cs*します。</span><span class="sxs-lookup"><span data-stu-id="36b08-182">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="36b08-183">新しく登録されたユーザーが、次の行をコメント アウトによって自動的にログオンされているようにします。</span><span class="sxs-lookup"><span data-stu-id="36b08-183">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="36b08-184">メソッド全体を強調表示されている変更された行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="36b08-184">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="36b08-185">登録、確認の電子メール、およびパスワードのリセット</span><span class="sxs-lookup"><span data-stu-id="36b08-185">Register, confirm email, and reset password</span></span>

<span data-ttu-id="36b08-186">Web アプリを実行し、アカウントの確認とパスワードの回復フローをテストします。</span><span class="sxs-lookup"><span data-stu-id="36b08-186">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="36b08-187">アプリを実行し、新しいユーザーの登録</span><span class="sxs-lookup"><span data-stu-id="36b08-187">Run the app and register a new user</span></span>

  ![Web アプリケーションのアカウントの登録の表示](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="36b08-189">アカウント確認用のリンクは、電子メールを確認します。</span><span class="sxs-lookup"><span data-stu-id="36b08-189">Check your email for the account confirmation link.</span></span> <span data-ttu-id="36b08-190">参照してください[デバッグ電子メール](#debug)電子メールが届かない場合。</span><span class="sxs-lookup"><span data-stu-id="36b08-190">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="36b08-191">電子メールを確認するリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="36b08-191">Click the link to confirm your email.</span></span>
* <span data-ttu-id="36b08-192">電子メール アドレスとパスワードでログインします。</span><span class="sxs-lookup"><span data-stu-id="36b08-192">Log in with your email and password.</span></span>
* <span data-ttu-id="36b08-193">ログオフします。</span><span class="sxs-lookup"><span data-stu-id="36b08-193">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="36b08-194">ビューの管理 ページ</span><span class="sxs-lookup"><span data-stu-id="36b08-194">View the manage page</span></span>

<span data-ttu-id="36b08-195">ブラウザーで、ユーザー名を選択:![ユーザー名を備えたブラウザー ウィンドウ](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="36b08-195">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="36b08-196">ユーザー名を参照するナビゲーション バーを展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="36b08-196">You might need to expand the navbar to see user name.</span></span>

![ナビゲーション バー](accconfirm/_static/x.png)

<span data-ttu-id="36b08-198">管理ページが表示されますが、**プロファイル**タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="36b08-198">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="36b08-199">**電子メール**電子メールを示すチェック ボックスが確認されているかを示します。</span><span class="sxs-lookup"><span data-stu-id="36b08-199">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="36b08-200">テストのパスワードのリセット</span><span class="sxs-lookup"><span data-stu-id="36b08-200">Test password reset</span></span>

* <span data-ttu-id="36b08-201">ログインしている場合は、選択**ログアウト**します。</span><span class="sxs-lookup"><span data-stu-id="36b08-201">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="36b08-202">選択、**ログイン**リンクし、選択、**パスワードを忘れた場合でしょうか。** リンク。</span><span class="sxs-lookup"><span data-stu-id="36b08-202">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="36b08-203">アカウントを登録するために使用する電子メール アドレスを入力します。</span><span class="sxs-lookup"><span data-stu-id="36b08-203">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="36b08-204">パスワードをリセットするリンクを含む電子メールが送信されます。</span><span class="sxs-lookup"><span data-stu-id="36b08-204">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="36b08-205">電子メールを確認し、パスワードをリセットするリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="36b08-205">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="36b08-206">パスワードが正常にリセットされると後、はの電子メール アドレスと新しいパスワードを使用してログインすることができます。</span><span class="sxs-lookup"><span data-stu-id="36b08-206">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="36b08-207">電子メールをデバッグします。</span><span class="sxs-lookup"><span data-stu-id="36b08-207">Debug email</span></span>

<span data-ttu-id="36b08-208">電子メールの作業が発生したことはできません: 場合</span><span class="sxs-lookup"><span data-stu-id="36b08-208">If you can't get email working:</span></span>

* <span data-ttu-id="36b08-209">ブレークポイントを設定`EmailSender.Execute`を確認する`SendGridClient.SendEmailAsync`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="36b08-209">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="36b08-210">作成、[電子メールを送信するためのコンソール アプリ](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)に同様のコードを使用して`EmailSender.Execute`します。</span><span class="sxs-lookup"><span data-stu-id="36b08-210">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="36b08-211">レビュー、[電子メール アクティビティ](https://sendgrid.com/docs/User_Guide/email_activity.html)ページ。</span><span class="sxs-lookup"><span data-stu-id="36b08-211">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="36b08-212">迷惑メール フォルダーを確認します。</span><span class="sxs-lookup"><span data-stu-id="36b08-212">Check your spam folder.</span></span>
* <span data-ttu-id="36b08-213">別のメール プロバイダー (Microsoft、Yahoo、Gmail など) に別の電子メール エイリアスをお試しください。</span><span class="sxs-lookup"><span data-stu-id="36b08-213">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="36b08-214">別のメール アカウントへの送信を再試行してください。</span><span class="sxs-lookup"><span data-stu-id="36b08-214">Try sending to different email accounts.</span></span>

<span data-ttu-id="36b08-215">**セキュリティのベスト プラクティス**は**いない**テストと開発における運用シークレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="36b08-215">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="36b08-216">アプリを Azure に発行する場合は、Web アプリを Azure portal でアプリケーション設定と SendGrid シークレットを設定できます。</span><span class="sxs-lookup"><span data-stu-id="36b08-216">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="36b08-217">構成システムは、環境変数からキーの読み取りを設定します。</span><span class="sxs-lookup"><span data-stu-id="36b08-217">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="36b08-218">ソーシャル、ローカルのログイン アカウントを組み合わせる</span><span class="sxs-lookup"><span data-stu-id="36b08-218">Combine social and local login accounts</span></span>

<span data-ttu-id="36b08-219">このセクションを完了するには、まず、外部認証プロバイダーを有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="36b08-219">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="36b08-220">参照してください[Facebook、Google、および外部プロバイダー認証](xref:security/authentication/social/index)します。</span><span class="sxs-lookup"><span data-stu-id="36b08-220">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="36b08-221">電子メールのリンクをクリックして、ローカルおよびソーシャル アカウントを組み合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="36b08-221">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="36b08-222">次の順序で"RickAndMSFT@gmail.com"が最初に、ローカルのログインとして作成ただし、最初に、ソーシャル ログインとしてアカウントを作成し、ローカル ログインを追加します。</span><span class="sxs-lookup"><span data-stu-id="36b08-222">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web アプリケーション:RickAndMSFT@gmail.com認証されたユーザー](accconfirm/_static/rick.png)

<span data-ttu-id="36b08-224">をクリックして、**管理**リンク。</span><span class="sxs-lookup"><span data-stu-id="36b08-224">Click on the **Manage** link.</span></span> <span data-ttu-id="36b08-225">このアカウントに関連付けられた 0 外部 (ソーシャル ログイン) に注意してください。</span><span class="sxs-lookup"><span data-stu-id="36b08-225">Note the 0 external (social logins) associated with this account.</span></span>

![ビューを管理します。](accconfirm/_static/manage.png)

<span data-ttu-id="36b08-227">別のログイン サービスへのリンクをクリックし、アプリの要求をそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="36b08-227">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="36b08-228">次の図では、Facebook は、外部認証プロバイダーは。</span><span class="sxs-lookup"><span data-stu-id="36b08-228">In the following image, Facebook is the external authentication provider:</span></span>

![Facebook を一覧表示する、外部ログイン ビューを管理します。](accconfirm/_static/fb.png)

<span data-ttu-id="36b08-230">2 つのアカウントが統合されました。</span><span class="sxs-lookup"><span data-stu-id="36b08-230">The two accounts have been combined.</span></span> <span data-ttu-id="36b08-231">いずれかのアカウントでログオンすることは。</span><span class="sxs-lookup"><span data-stu-id="36b08-231">You are able to log on with either account.</span></span> <span data-ttu-id="36b08-232">ユーザーがソーシャル ログインの認証サービスがダウンしているか、自分のソーシャル アカウントにアクセスを紛失した可能性が高い場合に、ローカル アカウントを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="36b08-232">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="36b08-233">サイトがユーザー アカウントの確認を有効にします。</span><span class="sxs-lookup"><span data-stu-id="36b08-233">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="36b08-234">ユーザーとサイトのアカウント確認を有効にする既存のすべてのユーザーをロックアウトします。</span><span class="sxs-lookup"><span data-stu-id="36b08-234">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="36b08-235">自分のアカウントが確認されないために、既存のユーザーはロックアウトされます。</span><span class="sxs-lookup"><span data-stu-id="36b08-235">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="36b08-236">既存のユーザーのロックアウトを回避するには、次の方法のいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="36b08-236">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="36b08-237">確認されているすべての既存ユーザーをマークするデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="36b08-237">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="36b08-238">既存のユーザーを確認します。</span><span class="sxs-lookup"><span data-stu-id="36b08-238">Confirm exiting users.</span></span> <span data-ttu-id="36b08-239">確認リンクを含むメールなどのバッチの送信をします。</span><span class="sxs-lookup"><span data-stu-id="36b08-239">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
