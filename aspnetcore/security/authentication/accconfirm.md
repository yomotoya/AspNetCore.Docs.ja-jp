---
title: "アカウントの確認と ASP.NET Core でのパスワードの回復"
author: rick-anderson
description: "電子メールの確認とパスワードのリセットと ASP.NET Core アプリケーションをビルドする方法を示します。"
ms.author: riande
manager: wpickett
ms.date: 12/1/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/accconfirm
ms.openlocfilehash: bc9febc41d0637be9f83a02799d360489f257849
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>アカウントの確認と ASP.NET Core でのパスワードの回復

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette) 

このチュートリアルでは、電子メールの確認とパスワードのリセットと ASP.NET Core アプリケーションをビルドする方法を示します。

## <a name="create-a-new-aspnet-core-project"></a>新しい .NET Core プロジェクトを作成する

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

この手順は、Windows 上の Visual Studio に適用されます。 CLI 手順については、次のセクションを参照してください。

チュートリアルでは、Visual Studio 2017 Preview 2 またはそれ以降が必要です。

* Visual Studio で、新しい Web アプリケーション プロジェクトを作成します。
* 選択**ASP.NET Core 2.0**です。 次の図ショー **.NET Core**選択すると、選択することができますが、 **.NET Framework**です。
* 選択**認証の変更**に設定および**個々 のユーザー アカウント**です。
* 既定値を保持**ストア ユーザー アカウントをアプリ内**です。

![新しいプロジェクト ダイアログ ボックスを示す"個別のユーザー アカウント radio"が選択されています。](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

このチュートリアルでは、2017 またはそれ以降の Visual Studio が必要です。

* Visual Studio で、新しい Web アプリケーション プロジェクトを作成します。
* 選択**認証の変更**に設定および**個々 のユーザー アカウント**です。

![新しいプロジェクト ダイアログ ボックスを示す"個別のユーザー アカウント radio"が選択されています。](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a>MacOS および Linux 用 .NET core CLI プロジェクトの作成

CLI または SQLite を使用している場合は、コマンド ウィンドウで、次を実行します。

```console
dotnet new mvc --auth Individual
```

* `--auth Individual`個々 のユーザー アカウントのテンプレートを指定します。
* Windows では、追加、`-uld`オプション。 `-uld`オプションは、SQLite DB ではなく、LocalDB 接続文字列を作成します。
* 実行`new mvc --help`にこのコマンドのヘルプを表示します。

## <a name="test-new-user-registration"></a>テストの新規ユーザーの登録

アプリを実行する、選択、**登録**リンク、およびユーザーを登録します。 Entity Framework Core 移行を実行する手順に従います。 この時点では、電子メールにのみ、検証、 [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)属性。 登録を送信した後、アプリにログインします。 、このチュートリアルで後ほどお変更この電子メールが検証されるまで新規ユーザーがログインできないようにします。

## <a name="view-the-identity-database"></a>ユーザー データベースを表示します。

# <a name="sql-servertabsql-server"></a>[SQL Server](#tab/sql-server)

* **ビュー**メニューの  **SQL Server オブジェクト エクスプ ローラー** (SSOX)。 
* 移動**(localdb) MSSQLLocalDB (SQL Server 13)**です。 右クリックして**dbo します。AspNetUsers** > **データ表示**:

![SQL Server オブジェクト エクスプ ローラー AspNetUsers テーブルのコンテキスト メニュー](accconfirm/_static/ssox.png)

注、`EmailConfirmed`フィールドは`False`します。

アプリが確認の電子メールを送信するとき、次の手順でこの電子メールを再利用する可能性があります。 クリックし、行を右クリックして**削除**です。 電子メールのエイリアスが、削除はで容易に、次の手順です。

# <a name="sqlitetabsqlite"></a>[SQLite](#tab/sqlite)

参照してください[ASP.NET Core MVC プロジェクトで作業して SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql) SQLite DB を表示する方法についてはします。 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a>SSL を要求し、SSL 用の IIS Express のセットアップ

参照してください[強制 SSL](xref:security/enforcing-ssl)です。

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>確認の電子メールが必要

他のユーザー偽装はいないしていることを確認する新規ユーザーの登録の電子メール アドレスを確認することをお勧め (つまり、まだに登録されている他のユーザーの電子メール)。 ディスカッション フォーラムいてしないようにすると仮定します"yli@example.com"として登録する"fromnolivetto@contoso.com"。 電子メールの確認なし"nolivetto@contoso.com"アプリから不要な電子メールを取得する可能性があります。 ユーザーが誤ってとして登録されていると仮定します"ylo@example.com"ミススペルを認識していないと"yli、"ができなく、アプリは、正しいメール アドレスがある見つからないために、パスワードの回復を使用します。 確認の電子メールは、bot から制限の保護のみを提供し、登録に使用できる多くの作業電子メール エイリアスを持つ判別スパム攻撃者から保護を提供しません。

一般にする新しいユーザーが確認された電子メールがある前に、web サイトにデータを投稿するを防ぐ。 

更新`ConfigureServices`確認電子メールを要求します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
前の行では、登録済みユーザーが自分の電子メールが確認されるまでには記録できなくなります。 ただし、その行によって妨げられない新しいユーザーを登録した後に記録されています。 既定のコードは、登録した後でユーザーを記録します。 ログアウトすると、登録するまで再ログインすることはできません。 コードのために新規登録されたユーザーは、変更をこのチュートリアルで後ほど**いない**ログに記録します。

### <a name="configure-email-provider"></a>電子メール プロバイダーを構成します。

このチュートリアルでは、SendGrid を使用して電子メールを送信します。 SendGrid アカウントと電子メールを送信するキーが必要です。 その他の電子メール プロバイダーを使用することができます。 ASP.NET Core 2.x が含まれています`System.Net.Mail`、アプリから電子メールを送信することができます。 SendGrid または別の電子メール サービスを使用して電子メールを送信することをお勧めします。

[オプション パターン](xref:fundamentals/configuration/options)ユーザー アカウントとキーの設定にアクセスするために使用します。 詳細については、次を参照してください。[構成](xref:fundamentals/configuration/index)です。

セキュリティで保護された電子メールのキーを取得するクラスを作成します。 このサンプルで、`AuthMessageSenderOptions`でクラスを作成、 *Services/AuthMessageSenderOptions.cs*ファイル。

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

設定、`SendGridUser`と`SendGridKey`で、[シークレット マネージャー ツール](../app-secrets.md)です。 例:

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Windows では、シークレット マネージャーが、キーと値のペアを格納、 *secrets.json* %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId > ディレクトリ内のファイルです。

内容、 *secrets.json*ファイルが暗号化されていません。 *Secrets.json*ファイルを次に示します (、`SendGridKey`値が削除されました)。

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>AuthMessageSenderOptions を使用するスタートアップを構成します。

追加`AuthMessageSenderOptions`サービス コンテナーの最後に、`ConfigureServices`メソッドで、 *Startup.cs*ファイル。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>AuthMessageSender クラスを構成します。

このチュートリアルを使用して電子メール通知を追加する方法を示しています。 [SendGrid](https://sendgrid.com/)、SMTP、およびその他のメカニズムを使用して電子メールを送信することができますが、します。

* インストール、 `SendGrid` NuGet パッケージです。 パッケージ マネージャー コンソールから、次を入力して、次のコマンド。

  `Install-Package SendGrid`

* 参照してください[始める SendGrid 無料](https://sendgrid.com/free/)無料の SendGrid アカウントを登録します。

#### <a name="configure-sendgrid"></a>SendGrid を構成します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* コードを追加*Services/EmailSender.cs* SendGrid を構成するには、次のようにします。

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
* コードを追加*Services/MessageServices.cs* SendGrid を構成するには、次のようにします。

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>アカウントの確認とパスワードの回復を有効にします。

テンプレートは、アカウントの確認とパスワードの回復用コードを持っています。 検索、`[HttpPost] Register`メソッドで、 *AccountController.cs*ファイル。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

新しく登録されたユーザーが次の行をコメント アウトして自動ログオンされているようにします。

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

メソッド全体を強調表示されている変更された行が表示されます。

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

注: 上記のコードは失敗を実装する場合`IEmailSender`とテキスト形式の電子メールを送信します。 参照してください[この問題](https://github.com/aspnet/Home/issues/2152)の詳細については、問題を回避します。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

アカウントの確認を有効にするコードのコメントを解除します。

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

注: お中もにより、新しく登録されたユーザー、次の行をコメント アウトして自動ログオンされています。

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

内のコードのコメントを解除してパスワードの回復を有効にする、`ForgotPassword`アクションで、 *Controllers/AccountController.cs*ファイル。

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

フォーム要素をコメントから*Views/Account/ForgotPassword.cshtml*です。 削除する必要があります、`<p> For more information on how to enable reset password ... </p>`この資料へのリンクを含む要素です。

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

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

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

管理 ページが表示され、**プロファイル**タブを選択します。 **電子メール**電子メールを示すチェック ボックスが確認されているかを示します。 

![[管理] ページ](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

このチュートリアルで後ほどこのページについて説明します。
![[管理] ページ](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>テストのパスワードのリセット

* ログインしている場合は、選択**ログアウト**です。  
* 選択、**ログイン**リンクを選択して、**パスワードを忘れた場合ですか?**リンクします。
* アカウントの登録に使用したメール アドレスを入力します。
* パスワードをリセットするリンクを含む電子メールが送信されます。 電子メールを確認し、パスワードをリセットするリンクをクリックします。  パスワードが正常にリセットされると、後に、電子メールと新しいパスワードでログインすることができます。

<a name="debug"></a>

### <a name="debug-email"></a>電子メールをデバッグします。

電子メールの作業を取得できません: 場合

* 確認、[電子メール アクティビティ](https://sendgrid.com/docs/User_Guide/email_activity.html)ページ。
* 迷惑メール フォルダーをチェックします。
* 別の電子メール プロバイダー (Microsoft、yahoo!、Gmail など) に別の電子メール エイリアスを再試行してください。
* 作成、[電子メールを送信するコンソール アプリ](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)です。
* 別の電子メール アカウントへの送信を再試行してください。

**注:**セキュリティのベスト プラクティスは、開発、テストや実稼働の機密情報を使用しないようにします。 アプリを Azure に発行する場合は、Web アプリの Azure ポータルでのアプリケーション設定として、SendGrid の機密情報を設定できます。 構成システム環境変数からキーを読み取るをセットアップしています。

## <a name="prevent-login-at-registration"></a>登録時にログインをできません。

現在のテンプレートを使用してユーザーには、登録フォームが完了すると、ログインしています (認証)。 ログを記録する前に自分の電子メールを確認する一般にできます。 以下のセクションを必要とするコードを変更しましたポータルにログインする前に、新しいユーザーが確認された電子メールをあります。 更新プログラム、`[HttpPost] Login`アクションで、 *AccountController.cs*の次の強調表示されている変更されたファイルです。

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

**注:**セキュリティのベスト プラクティスは、開発、テストや実稼働の機密情報を使用しないようにします。 アプリを Azure に発行する場合は、Web アプリの Azure ポータルでのアプリケーション設定として、SendGrid の機密情報を設定できます。 構成システム環境変数からキーを読み取るをセットアップしています。


## <a name="combine-social-and-local-login-accounts"></a>ソーシャルとローカルのログイン アカウントを結合します。

注: このセクションでは、ASP.NET Core にのみ適用されます 1.x です。 Asp.net 2.x のコアは、「[この](https://github.com/aspnet/Docs/issues/3753)問題です。

このセクションを完了するには、まず外部認証プロバイダーを有効にする必要があります。 参照してください[Facebook、Google やその他の外部プロバイダーを使用して有効にする認証](social/index.md)です。

電子メールのリンクをクリックすると、ローカルおよびソーシャルのアカウントを組み合わせることができます。 次の順序で"RickAndMSFT@gmail.com"が最初に、ローカルのログインとして作成ただし、最初に、ソーシャル ログインとしてアカウントを作成し、ローカル ログインを追加します。

![Web アプリケーション:RickAndMSFT@gmail.com認証されたユーザー](accconfirm/_static/rick.png)

をクリックして、**管理**リンクします。 このアカウントに関連付けられた 0 外部 (ソーシャル ログイン) に注意してください。

![ビューを管理します。](accconfirm/_static/manage.png)

別のログイン サービスへのリンクをクリックし、アプリの要求を許可します。 次の図では、Facebook は、外部認証プロバイダーを示します。

![Facebook を一覧表示する、外部ログイン ビューを管理します。](accconfirm/_static/fb.png)

2 つのアカウントが統合されました。 いずれかのアカウントでログオンすることができます。 ユーザー認証サービスのソーシャル、ログがダウンしているか、ソーシャル アカウントにアクセスを紛失した可能性が高い場合に、ローカル アカウントを追加することができます。
