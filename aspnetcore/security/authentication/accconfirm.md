---
title: アカウントの確認と ASP.NET Core でのパスワードの回復
author: rick-anderson
description: 電子メールの確認とパスワードのリセットと ASP.NET Core アプリを構築する方法について説明します。
ms.author: riande
ms.date: 7/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 0dc9907f9f54c8a0daf2e05a3769897e5145935f
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444143"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>アカウントの確認と ASP.NET Core でのパスワードの回復

::: moniker range="<= aspnetcore-2.0"

参照してください[この PDF ファイル](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core 1.1 と version 2.1 です。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)

このチュートリアルでは、電子メールの確認とパスワードのリセットと ASP.NET Core アプリをビルドする方法を示します。 このチュートリアルは**いない**先頭トピック。 理解しておく必要があります。

* [ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start)
* [認証](xref:security/authentication/identity)
* [Entity Framework Core](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a>Web アプリを作成し、Identity のスキャフォールディング

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

* Visual Studio で、作成、新しい**Web アプリケーション**という名前のプロジェクト**WebPWrecover**します。
* 選択**ASP.NET Core 2.1**します。
* 既定値を保持**認証**設定**認証なし**します。 認証は、次の手順で追加されます。

次の手順。

* レイアウト ページを設定 *~/Pages/Shared/_Layout.cshtml*
* 選択*アカウント/登録*
* 新規作成**データ コンテキスト クラス**

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

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

実行`dotnet aspnet-codegenerator identity --help`スキャフォールディング ツールのヘルプを表示します。

------

指示に従って[認証を有効に](xref:security/authentication/scaffold-identity#useauthentication):

* 追加`app.UseAuthentication();`に `Startup.Configure`
* 追加`<partial name="_LoginPartial" />`レイアウト ファイルにします。

## <a name="test-new-user-registration"></a>新規ユーザー登録をテストします。

アプリを実行し、選択、**登録**リンク、およびユーザーを登録します。 この時点では、電子メールの検証のみ、 [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)属性。 登録を送信した後は、アプリにログインします。 チュートリアルの後半では、新しいユーザーが自分の電子メールが検証されるまでにログインできないため、コードが更新されます。

[!INCLUDE[](~/includes/view-identity-db.md)]

テーブルに注意してください`EmailConfirmed`フィールドは`False`します。

アプリが送信された確認メールを送信するとき、次の手順でこの電子メールをもう一度使用する場合があります。 クリックし、行を右クリックして**削除**します。 電子メール エイリアスを削除する簡単で、次の手順。

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>確認の電子メールが必要です。

新規ユーザー登録の電子メール アドレスを確認することをお勧めします。 電子メールの他のユーザーが偽装はいないしていることを確認することを確認できます (つまりに登録していない他のユーザーの電子メールで)。 ディスカッション フォーラムでは、行われていればし、しないようにする"yli@example.comとして登録する"から"nolivetto@contoso.com"。 電子メールの確認なし"nolivetto@contoso.com"アプリから不要な電子メールを受け取ることができます。 ユーザーが誤ってとして登録されているとします"ylo@example.com""yli"のスペル ミスを認識していなかったとします。 アプリは、正しいメール アドレスがあるないために、パスワードの回復を使用できるでしょう。 確認の電子メールは、ボットからの限られた保護を提供します。 確認の電子メールは、多くの電子メール アカウントを使用して悪意のあるユーザーから保護を提供しません。

新しいユーザーが確認された電子メールを受ける前に、web サイトにデータを送信するを防ぐために一般的にします。

Update *Areas/Identity/IdentityHostingStartup.cs*確認された電子メールを要求します。

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

`config.SignIn.RequireConfirmedEmail = true;` 登録済みのユーザーで自分の電子メールを確認するまでのログ記録はできません。

### <a name="configure-email-provider"></a>電子メール プロバイダーを構成します。

このチュートリアルで[SendGrid](https://sendgrid.com)電子メールを送信するために使用します。 SendGrid アカウントとキーが電子メールを送信する必要があります。 その他の電子メール プロバイダーを使用することができます。 ASP.NET Core の 2.x を含む`System.Net.Mail`、アプリから電子メールを送信できます。 SendGrid または別の電子メール サービスを使用して電子メールを送信することをお勧めします。 SMTP では、セキュリティで保護し、設定を正しく困難です。

電子メールをセキュリティで保護されたキーを取得するためのクラスを作成します。 このサンプルでは、次のように作成します*Services/AuthMessageSenderOptions.cs*:。

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a>SendGrid のユーザーの機密情報を構成します。

一意な追加`<UserSecretsId>`値を`<PropertyGroup>`プロジェクト ファイルの要素。

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

設定、`SendGridUser`と`SendGridKey`で、 [secret manager ツール](xref:security/app-secrets)します。 例:

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
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
 
詳細については、次を参照してください。、[オプション パターン](xref:fundamentals/configuration/options)と[構成](xref:fundamentals/configuration/index)します。

### <a name="install-sendgrid"></a>SendGrid をインストールします。

このチュートリアルは、使用して電子メール通知を追加する方法を示します[SendGrid](https://sendgrid.com/)が SMTP およびその他のメカニズムを使用して電子メールを送信できます。

インストール、 `SendGrid` NuGet パッケージ。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

パッケージ マネージャー コンソールで、次のコマンドを入力します。

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

コンソールで、次のコマンドを入力します。

```cli
dotnet add package SendGrid
```

------

参照してください[SendGrid を無料で開始する](https://sendgrid.com/free/)無料の SendGrid アカウントを登録します。
### <a name="implement-iemailsender"></a>IEmailSender を実装します。

実装`IEmailSender`、作成*Services/EmailSender.cs*次のようなコードで。

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a>電子メールをサポートするために起動時を構成します。

次のコードを追加、`ConfigureServices`メソッドで、 *Startup.cs*ファイル。

* 追加`EmailSender`シングルトン サービスとして。
* 登録、`AuthMessageSenderOptions`構成インスタンス。

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a>アカウントの確認とパスワードの回復を有効にします。

テンプレートには、アカウントの確認とパスワードの回復用コードがあります。 検索、`OnPostAsync`メソッド*Areas/Identity/Pages/Account/Register.cshtml.cs*します。

新しく登録されたユーザーが、次の行をコメント アウトによって自動的にログオンされているようにします。

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

メソッド全体を強調表示されている変更された行が表示されます。

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

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

管理ページが表示されますが、**プロファイル**タブを選択します。 **電子メール**電子メールを示すチェック ボックスが確認されているかを示します。

### <a name="test-password-reset"></a>テストのパスワードのリセット

* ログインしている場合は、選択**ログアウト**します。
* 選択、**ログイン**リンクし、選択、**パスワードを忘れた場合でしょうか。** リンク。
* アカウントを登録するために使用する電子メール アドレスを入力します。
* パスワードをリセットするリンクを含む電子メールが送信されます。 電子メールを確認し、パスワードをリセットするリンクをクリックします。 パスワードが正常にリセットされると後、はの電子メール アドレスと新しいパスワードを使用してログインすることができます。

<a name="debug"></a>

### <a name="debug-email"></a>電子メールをデバッグします。

電子メールの作業が発生したことはできません: 場合

* ブレークポイントを設定`EmailSender.Execute`を確認する`SendGridClient.SendEmailAsync`が呼び出されます。
* 作成、[電子メールを送信するためのコンソール アプリ](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)に同様のコードを使用して`EmailSender.Execute`します。
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

::: moniker-end
