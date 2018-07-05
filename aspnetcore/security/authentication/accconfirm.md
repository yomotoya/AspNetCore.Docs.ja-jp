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
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>アカウントの確認と ASP.NET Core でのパスワードの回復

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)

このチュートリアルでは、電子メールの確認とパスワードのリセットと ASP.NET Core アプリをビルドする方法を示します。 このチュートリアルは**いない**先頭トピック。 理解しておく必要があります。

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [認証](xref:security/authentication/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

参照してください[この PDF ファイル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)の ASP.NET Core MVC 1.1 と 2.x のバージョン。

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>.NET Core CLI を使用した新しい ASP.NET Core プロジェクトを作成します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

* `--auth Individual` 個々 のユーザー アカウントのプロジェクト テンプレートを指定します。
* Windows では、追加、`-uld`オプション。 これは、SQLite の代わりに LocalDB を使用する必要がありますを指定します。
* 実行`new mvc --help`にこのコマンドのヘルプを表示します。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

CLI または SQLite を使用している場合は、コマンド ウィンドウで、次を実行します。

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` 個々 のユーザー アカウントのプロジェクト テンプレートを指定します。
* Windows では、追加、`-uld`オプション。 これは、SQLite の代わりに LocalDB を使用する必要がありますを指定します。
* 実行`new mvc --help`にこのコマンドのヘルプを表示します。

---

または、Visual Studio で新しい ASP.NET Core プロジェクトを作成できます。

* Visual Studio で、作成、新しい**Web アプリケーション**プロジェクト。
* 選択**ASP.NET Core 2.0**します。 **.NET core**が次の図で選択されている選択することができますが、 **.NET Framework**します。
* 選択**認証の変更**に設定し、**個々 のユーザー アカウント**します。
* 既定値を保持**ストア ユーザー アカウントをアプリ内**します。

![選択されている"個々 のユーザー アカウント radio"を示す新しいプロジェクト ダイアログ ボックス](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>新規ユーザー登録をテストします。

アプリを実行し、選択、**登録**リンク、およびユーザーを登録します。 Entity Framework Core migrations を実行する手順に従います。 この時点では、電子メールの検証のみ、 [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)属性。 登録を送信した後は、アプリにログインします。 チュートリアルの後半では、新しいユーザーが自分の電子メールが検証されるまでにログインできないため、コードが更新されます。

## <a name="view-the-identity-database"></a>Id データベースを表示します。

参照してください[、ASP.NET Core MVC プロジェクトでの SQLite の使用](xref:tutorials/first-mvc-app-xplat/working-with-sql)SQLite データベースを表示する方法の詳細について。

For Visual Studio:

* **ビュー**メニューの  **SQL Server オブジェクト エクスプ ローラー** (SSOX)。
* 移動します **(localdb) (SQL Server 13) MSSQLLocalDB**します。 右クリックして**dbo します。AspNetUsers** > **データを表示する**:

![SQL Server オブジェクト エクスプ ローラー AspNetUsers テーブルのコンテキスト メニュー](accconfirm/_static/ssox.png)

テーブルに注意してください`EmailConfirmed`フィールドは`False`します。

アプリが送信された確認メールを送信するとき、次の手順でこの電子メールをもう一度使用する場合があります。 クリックし、行を右クリックして**削除**します。 電子メール エイリアスを削除する簡単で、次の手順。

---

## <a name="require-https"></a>HTTPS が必要です。

参照してください[Https](xref:security/enforcing-ssl)します。

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>確認の電子メールが必要です。

新規ユーザー登録の電子メール アドレスを確認することをお勧めします。 電子メールの他のユーザーが偽装はいないしていることを確認することを確認できます (つまりに登録していない他のユーザーの電子メールで)。 ディスカッション フォーラムでは、行われていればし、しないようにする"yli@example.comとして登録する"から"nolivetto@contoso.com"。 電子メールの確認なし"nolivetto@contoso.com"アプリから不要な電子メールを受け取ることができます。 ユーザーが誤ってとして登録されているとします"ylo@example.com""yli"のスペル ミスを認識していなかったとします。 アプリは、正しいメール アドレスがあるないために、パスワードの回復を使用できるでしょう。 確認の電子メールは、ボットからの限られた保護のみを提供します。 確認の電子メールは、多くの電子メール アカウントを使用して悪意のあるユーザーから保護を提供しません。

新しいユーザーが確認された電子メールを受ける前に、web サイトにデータを送信するを防ぐために一般的にします。

Update`ConfigureServices`確認された電子メールを要求します。

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` 登録済みのユーザーで自分の電子メールを確認するまでのログ記録はできません。

### <a name="configure-email-provider"></a>電子メール プロバイダーを構成します。

このチュートリアルでは、SendGrid が電子メールの送信に使用されます。 SendGrid アカウントとキーが電子メールを送信する必要があります。 その他の電子メール プロバイダーを使用することができます。 ASP.NET Core の 2.x を含む`System.Net.Mail`、アプリから電子メールを送信できます。 SendGrid または別の電子メール サービスを使用して電子メールを送信することをお勧めします。 SMTP では、セキュリティで保護し、設定を正しく困難です。

[オプション パターン](xref:fundamentals/configuration/options)ユーザー アカウントとキーの設定にアクセスするために使用します。 詳細については、次を参照してください。[構成](xref:fundamentals/configuration/index)します。

電子メールをセキュリティで保護されたキーを取得するためのクラスを作成します。 このサンプルで、`AuthMessageSenderOptions`でクラスを作成、 *Services/AuthMessageSenderOptions.cs*ファイル。

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

設定、`SendGridUser`と`SendGridKey`で、 [secret manager ツール](xref:security/app-secrets)します。 例えば:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Secret Manager、Windows 上のキー/値のペアが格納、 *secrets.json*ファイル、`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`ディレクトリ。

内容、 *secrets.json*ファイルは暗号化されません。 *Secrets.json*ファイルを次に示します (、`SendGridKey`値が削除されました)。

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>AuthMessageSenderOptions を使用するスタートアップを構成します。

追加`AuthMessageSenderOptions`サービス コンテナーの最後に、`ConfigureServices`メソッドで、 *Startup.cs*ファイル。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>AuthMessageSender クラスを構成します。

このチュートリアルは、使用して電子メール通知を追加する方法を示します[SendGrid](https://sendgrid.com/)が SMTP およびその他のメカニズムを使用して電子メールを送信できます。

インストール、 `SendGrid` NuGet パッケージ。

* コマンドライン: から

    `dotnet add package SendGrid`

* パッケージ マネージャー コンソールで、次のコマンドを入力します。

  `Install-Package SendGrid`

参照してください[SendGrid を無料で開始する](https://sendgrid.com/free/)無料の SendGrid アカウントを登録します。

#### <a name="configure-sendgrid"></a>SendGrid を構成します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

SendGrid を構成するには、次のようなコードを追加*Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* 内のコードを追加*Services/MessageServices.cs* SendGrid を構成するには、次のようにします。

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>アカウントの確認とパスワードの回復を有効にします。

テンプレートには、アカウントの確認とパスワードの回復用コードがあります。 検索、`OnPostAsync`メソッド*Pages/Account/Register.cshtml.cs*します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

新しく登録されたユーザーが、次の行をコメント アウトによって自動的にログオンされているようにします。

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

メソッド全体を強調表示されている変更された行が表示されます。

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

アカウントの確認を有効にするのには、次のコードをコメント解除します。

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**注:** コードが原因で、新しく登録されたユーザーが、次の行をコメント アウトによって自動的にログオンされています。

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

内のコードのコメントを解除してパスワードの回復を有効にする、`ForgotPassword`のアクション*Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

フォーム要素をコメント解除します*Views/Account/ForgotPassword.cshtml*します。 削除する必要があります、`<p> For more information on how to enable reset password ... </p>`要素で、この記事へのリンクが含まれています。

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>登録、確認の電子メール、およびパスワードのリセット

Web アプリを実行し、アカウントの確認とパスワードの回復フローをテストします。

* アプリを実行し、新しいユーザーの登録

  ![Web アプリケーションのアカウントの登録の表示](accconfirm/_static/loginaccconfirm1.png)

* アカウント確認用のリンクは、電子メールを確認します。 参照してください[デバッグ電子メール](#debug)電子メールが届かない場合。
* 電子メールを確認するリンクをクリックします。
* 電子メール アドレスとパスワードでログインします。
* ログオフします。

### <a name="view-the-manage-page"></a>ビューの管理 ページ

ブラウザーで、ユーザー名を選択:![ユーザー名を備えたブラウザー ウィンドウ](accconfirm/_static/un.png)

ユーザー名を参照するナビゲーション バーを展開する必要があります。

![ナビゲーション バー](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

管理ページが表示されますが、**プロファイル**タブを選択します。 **電子メール**電子メールを示すチェック ボックスが確認されているかを示します。

![管理 ページ](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

これは、チュートリアルの後半で説明しました。
![[管理] ページ](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>テストのパスワードのリセット

* ログインしている場合は、選択**ログアウト**します。
* 選択、**ログイン**リンクし、選択、**パスワードを忘れた場合でしょうか。** リンク。
* アカウントを登録するために使用する電子メール アドレスを入力します。
* パスワードをリセットするリンクを含む電子メールが送信されます。 電子メールを確認し、パスワードをリセットするリンクをクリックします。 パスワードが正常にリセットされると後、はの電子メール アドレスと新しいパスワードを使用してログインすることができます。

<a name="debug"></a>

### <a name="debug-email"></a>電子メールをデバッグします。

電子メールの作業が発生したことはできません: 場合

* 作成、[電子メールを送信するためのコンソール アプリ](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)します。
* レビュー、[電子メール アクティビティ](https://sendgrid.com/docs/User_Guide/email_activity.html)ページ。
* 迷惑メール フォルダーを確認します。
* 別のメール プロバイダー (Microsoft、Yahoo、Gmail など) に別の電子メール エイリアスをお試しください。
* 別のメール アカウントへの送信を再試行してください。

**セキュリティのベスト プラクティス**は**いない**テストと開発における運用シークレットを使用します。 アプリを Azure に発行する場合は、Web アプリを Azure portal でアプリケーション設定と SendGrid シークレットを設定できます。 構成システムは、環境変数からキーの読み取りを設定します。

## <a name="combine-social-and-local-login-accounts"></a>ソーシャル、ローカルのログイン アカウントを組み合わせる

このセクションを完了するには、まず、外部認証プロバイダーを有効にする必要があります。 参照してください[Facebook、Google、および外部プロバイダー認証](xref:security/authentication/social/index)します。

電子メールのリンクをクリックして、ローカルおよびソーシャル アカウントを組み合わせることができます。 次の順序で"RickAndMSFT@gmail.com"が最初に、ローカルのログインとして作成ただし、最初に、ソーシャル ログインとしてアカウントを作成し、ローカル ログインを追加します。

![Web アプリケーション:RickAndMSFT@gmail.com認証されたユーザー](accconfirm/_static/rick.png)

をクリックして、**管理**リンク。 このアカウントに関連付けられた 0 外部 (ソーシャル ログイン) に注意してください。

![ビューを管理します。](accconfirm/_static/manage.png)

別のログイン サービスへのリンクをクリックし、アプリの要求をそのまま使用します。 次の図では、Facebook は、外部認証プロバイダーは。

![Facebook を一覧表示する、外部ログイン ビューを管理します。](accconfirm/_static/fb.png)

2 つのアカウントが統合されました。 いずれかのアカウントでログオンすることは。 ユーザーがソーシャル ログインの認証サービスがダウンしているか、自分のソーシャル アカウントにアクセスを紛失した可能性が高い場合に、ローカル アカウントを追加することができます。

## <a name="enable-account-confirmation-after-a-site-has-users"></a>サイトがユーザー アカウントの確認を有効にします。

ユーザーとサイトのアカウント確認を有効にする既存のすべてのユーザーをロックアウトします。 自分のアカウントが確認されないために、既存のユーザーはロックアウトされます。 既存のユーザーのロックアウトを回避するには、次の方法のいずれかを使用します。

* 確認されているすべての既存ユーザーをマークするデータベースを更新します。
* 既存のユーザーを確認します。 確認リンクを含むメールなどのバッチの送信をします。
