---
title: アカウントの確認と ASP.NET Core でのパスワードの回復
author: rick-anderson
description: 電子メールの確認とパスワードのリセットと ASP.NET Core アプリケーションをビルドする方法を説明します。
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: b6dbe234973431448c18d3cc82a6ac98d4f53a3b
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34730452"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>アカウントの確認と ASP.NET Core でのパスワードの回復

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)

このチュートリアルでは、電子メールの確認とパスワードのリセットと ASP.NET Core アプリケーションをビルドする方法を示します。 このチュートリアルでは、**いない**先頭トピックです。 理解しておく必要があります。

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [認証](xref:security/authentication/index)
* [アカウントの確認とパスワードの回復](xref:security/authentication/accconfirm)
* [Entity Framework Core](xref:data/ef-mvc/intro)

参照してください[この PDF ファイル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core MVC 1.1 および 2.x バージョン。

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>.NET Core CLI を使用して新しい ASP.NET Core プロジェクトを作成します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* `--auth Individual` 個々 のユーザー アカウントのプロジェクト テンプレートを指定します。
* Windows では、追加、`-uld`オプション。 SQLite ではなく LocalDB を使用する必要がありますを指定します。
* 実行`new mvc --help`にこのコマンドのヘルプを表示します。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

CLI または SQLite を使用している場合は、コマンド ウィンドウで、次を実行します。

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` 個々 のユーザー アカウントのプロジェクト テンプレートを指定します。
* Windows では、追加、`-uld`オプション。 SQLite ではなく LocalDB を使用する必要がありますを指定します。
* 実行`new mvc --help`にこのコマンドのヘルプを表示します。

---

または、Visual Studio で新しい ASP.NET Core プロジェクトを作成することができます。

* Visual Studio で、新しい**Web アプリケーション**プロジェクト。
* 選択**ASP.NET Core 2.0**です。 **.NET core**が次の図では、選択されている選択することができますが、 **.NET Framework**です。
* 選択**認証の変更**に設定および**個々 のユーザー アカウント**です。
* 既定値を保持**ストア ユーザー アカウントをアプリ内**です。

![新しいプロジェクト ダイアログ ボックスを示す"個別のユーザー アカウント radio"が選択されています。](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>テストの新規ユーザーの登録

アプリを実行する、選択、**登録**リンク、およびユーザーを登録します。 Entity Framework Core 移行を実行する手順に従います。 この時点では、電子メールにのみ、検証、 [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)属性。 登録を完了した後、アプリにログインします。 チュートリアルの後半では、自分の電子メールが検証されるまで新規ユーザーがログインできないため、コードが更新されます。

## <a name="view-the-identity-database"></a>ユーザー データベースを表示します。

参照してください[SQLite と ASP.NET Core MVC プロジェクトで作業](xref:tutorials/first-mvc-app-xplat/working-with-sql)SQLite データベースを表示する方法についてです。

For Visual Studio は。

* **ビュー**メニューの  **SQL Server オブジェクト エクスプ ローラー** (SSOX)。
* 移動 **(localdb) MSSQLLocalDB (SQL Server 13)** です。 右クリックして**dbo します。AspNetUsers** > **データ表示**:

![SQL Server オブジェクト エクスプ ローラー AspNetUsers テーブルのコンテキスト メニュー](accconfirm/_static/ssox.png)

テーブルに注意してください`EmailConfirmed`フィールドは`False`します。

アプリが確認の電子メールを送信するとき、次の手順でこの電子メールを再利用する可能性があります。 クリックし、行を右クリックして**削除**です。 電子メールのエイリアスを削除すると簡単に、次の手順にできます。

---

## <a name="require-https"></a>HTTPS が必要

参照してください[Https](xref:security/enforcing-ssl)です。

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>確認の電子メールが必要

新しいユーザーの登録の電子メール アドレスを確認することをお勧めします。 電子メールの他のユーザー偽装はいないしていることを確認するために確認役立ちます (つまり、まだに登録されている他のユーザーの電子メール)。 ディスカッション フォーラムいてしないようにすると仮定します"yli@example.com"として登録する"fromnolivetto@contoso.com"です。 電子メールの確認なし"nolivetto@contoso.com"は、アプリから不要な電子メールを受け取ることができます。 ユーザーが誤ってとして登録されていると仮定します"ylo@example.com""yli"のスペル ミスを認識していたとします。 できなく、アプリは、正しいメール アドレスがある見つからないために、パスワードの回復を使用します。 確認の電子メールは、bot から保護には制限を提供します。 確認の電子メールは、多くの電子メール アカウントを持つ悪意のあるユーザーから保護を提供しません。

一般にする新しいユーザーが確認された電子メールがある前に、web サイトにデータを投稿するを防ぐ。

更新`ConfigureServices`確認電子メールを要求します。

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` 登録済みユーザーがで自分の電子メールが確認されるまでのログ記録するを防ぎます。

### <a name="configure-email-provider"></a>電子メール プロバイダーを構成します。

このチュートリアルでは、SendGrid を使用して電子メールを送信します。 SendGrid アカウントと電子メールを送信するキーが必要です。 その他の電子メール プロバイダーを使用することができます。 ASP.NET Core 2.x が含まれています`System.Net.Mail`、アプリから電子メールを送信することができます。 SendGrid または別の電子メール サービスを使用して電子メールを送信することをお勧めします。 SMTP は適切に設定してセキュリティで保護するが困難です。

[オプション パターン](xref:fundamentals/configuration/options)ユーザー アカウントとキーの設定にアクセスするために使用します。 詳細については、次を参照してください。[構成](xref:fundamentals/configuration/index)です。

セキュリティで保護された電子メールのキーを取得するクラスを作成します。 このサンプルで、`AuthMessageSenderOptions`でクラスを作成、 *Services/AuthMessageSenderOptions.cs*ファイル。

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

設定、`SendGridUser`と`SendGridKey`で、[シークレット マネージャー ツール](xref:security/app-secrets)です。 例えば:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Windows では、シークレット マネージャーが内のキー/値ペアを格納、 *secrets.json*ファイルで、`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`ディレクトリ。

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

このチュートリアルを使用して電子メール通知を追加する方法を示しています。 [SendGrid](https://sendgrid.com/)、SMTP、およびその他のメカニズムを使用して電子メールを送信することができますが、します。

インストール、 `SendGrid` NuGet パッケージ。

* : コマンドラインから

    `dotnet add package SendGrid`

* パッケージ マネージャー コンソールで、次のコマンドを入力します。

  `Install-Package SendGrid`

参照してください[始める SendGrid 無料](https://sendgrid.com/free/)無料の SendGrid アカウントを登録します。

#### <a name="configure-sendgrid"></a>SendGrid を構成します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

SendGrid を構成するには、次のようなコードを追加*Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* コードを追加*Services/MessageServices.cs* SendGrid を構成するには、次のようにします。

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>アカウントの確認とパスワードの回復を有効にします。

テンプレートは、アカウントの確認とパスワードの回復用コードを持っています。 検索、`OnPostAsync`メソッド*Pages/Account/Register.cshtml.cs*です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

新しく登録されたユーザーが次の行をコメント アウトして自動ログオンされているようにします。

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

メソッド全体を強調表示されている変更された行が表示されます。

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

アカウントの確認を有効にするには、次のコードをコメントを解除します。

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**注:** コードなると、新しく登録されたユーザーは、次の行をコメント アウトして自動ログオンされているが妨げ。

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

内のコードのコメントを解除してパスワードの回復を有効にする、`ForgotPassword`のアクション*Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

フォーム要素をコメントから*Views/Account/ForgotPassword.cshtml*です。 削除する必要があります、`<p> For more information on how to enable reset password ... </p>`要素は、この記事の内容へのリンクが含まれています。

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>登録、確認電子メール、およびパスワードのリセット

Web アプリを実行し、アカウントの確認とパスワードの回復フローをテストします。

* アプリを実行して、新しいユーザーの登録

  ![Web アプリケーション取引明細の表示](accconfirm/_static/loginaccconfirm1.png)

* アカウントの確認リンクには、電子メールを確認してください。 参照してください[電子メールをデバッグ](#debug)電子メールを取得しない場合。
* 確認の電子メールへのリンクをクリックします。
* 電子メール アドレスとパスワードでログインします。
* ログオフします。

### <a name="view-the-manage-page"></a>ビューの管理 ページ

ブラウザーで、ユーザー名を選択:![ブラウザー ウィンドウでユーザー名](accconfirm/_static/un.png)

ユーザー名を確認して、ナビゲーションバーを展開する必要があります。

![ナビゲーション バー](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

管理 ページが表示され、**プロファイル**タブを選択します。 **電子メール**電子メールを示すチェック ボックスが確認されているかを示します。

![[管理] ページ](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

これは、チュートリアルの後半で説明されています。
![[管理] ページ](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>テストのパスワードのリセット

* ログインしている場合は、選択**ログアウト**です。
* 選択、**ログイン**リンクを選択して、**パスワードを忘れた場合ですか?** リンクします。
* アカウントの登録に使用したメール アドレスを入力します。
* パスワードをリセットするリンクを含む電子メールが送信されます。 電子メールを確認し、パスワードをリセットするリンクをクリックします。 パスワードが正常にリセットされると後、は、電子メールと新しいパスワードを使用してログインすることができます。

<a name="debug"></a>

### <a name="debug-email"></a>電子メールをデバッグします。

電子メールの作業を取得できません: 場合

* 作成、[電子メールを送信するコンソール アプリ](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)です。
* 確認、[電子メール アクティビティ](https://sendgrid.com/docs/User_Guide/email_activity.html)ページ。
* 迷惑メール フォルダーをチェックします。
* 別の電子メール プロバイダー (Microsoft、yahoo!、Gmail など) に別の電子メール エイリアスを再試行してください。
* 別の電子メール アカウントへの送信を再試行してください。

**セキュリティのベスト プラクティス**を**いない**開発、テストや実稼働のシークレットを使用します。 アプリを Azure に発行する場合は、Web アプリの Azure ポータルでのアプリケーション設定として、SendGrid の機密情報を設定できます。 構成システムは、環境変数からキーを読み取れませんを設定します。

## <a name="combine-social-and-local-login-accounts"></a>ソーシャルとローカルのログイン アカウントを結合します。

このセクションを完了するには、まず外部認証プロバイダーを有効にする必要があります。 参照してください[Facebook、Google、および外部プロバイダー認証](xref:security/authentication/social/index)です。

電子メールのリンクをクリックすると、ローカルおよびソーシャルのアカウントを組み合わせることができます。 次の順序で"RickAndMSFT@gmail.com"が最初に、ローカルのログインとして作成ただし、最初に、ソーシャル ログインとしてアカウントを作成し、ローカル ログインを追加します。

![Web アプリケーション:RickAndMSFT@gmail.com認証されたユーザー](accconfirm/_static/rick.png)

をクリックして、**管理**リンクします。 このアカウントに関連付けられた 0 外部 (ソーシャル ログイン) に注意してください。

![ビューを管理します。](accconfirm/_static/manage.png)

別のログイン サービスへのリンクをクリックし、アプリの要求を許可します。 次の図では、Facebook は、外部認証プロバイダーを示します。

![Facebook を一覧表示する、外部ログイン ビューを管理します。](accconfirm/_static/fb.png)

2 つのアカウントが統合されました。 いずれかのアカウントでログオンするのには、できます。 ユーザーに、ソーシャル ログイン認証サービスがダウンしているか、ソーシャル アカウントにアクセスを紛失した可能性が高い場合に、ローカル アカウントを追加することができます。

## <a name="enable-account-confirmation-after-a-site-has-users"></a>後のサイトがあるユーザー アカウントの確認を有効にします。

有効にするアカウントの確認、サイトでユーザーとは、既存のすべてのユーザーをロックします。 自分のアカウントが確認されないために、既存のユーザーはロックアウトされます。 既存ユーザーのロックアウトを回避するには、次の方法のいずれかを使用します。

* 確認されているすべての既存ユーザーを示すためにデータベースを更新します。
* 既存のユーザーを確認します。 たとえば、バッチ送信確認リンクによる電子メールのです。
