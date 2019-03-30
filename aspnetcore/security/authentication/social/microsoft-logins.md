---
title: ASP.NET Core での Microsoft アカウントの外部ログインのセットアップ
author: rick-anderson
description: このチュートリアルでは、既存の ASP.NET Core アプリケーションを Microsoft アカウント ユーザーの認証の統合について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 1733d049d6752c24d7749b5b5ae2a4b866492358
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2019
ms.locfileid: "58751004"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>ASP.NET Core での Microsoft アカウントの外部ログインのセットアップ

作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、サンプルの ASP.NET Core 2.0 プロジェクトが作成を使用して、Microsoft アカウントでサインインするユーザーを有効にする方法、[前のページ](xref:security/authentication/social/index)します。

## <a name="create-the-app-in-microsoft-developer-portal"></a>Microsoft 開発者ポータルで、アプリを作成します。

* 移動します[ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com)して作成するか、Microsoft アカウントにサインインします。

![[サインイン] ダイアログ](index/_static/MicrosoftDevLogin.png)

Microsoft アカウントがない場合は、タップ **[を作成します。](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** サインインした後にリダイレクトされます **アプリケーション** ページ。

![Microsoft 開発者ポータルの Microsoft Edge で開く](index/_static/MicrosoftDev.png)

* タップ**アプリの追加**、画面右上隅にあるし、入力、**アプリケーション名**と**Contact Email**:

![新しいアプリケーションの登録 ダイアログ ボックス](index/_static/MicrosoftDevAppCreate.png)

* このチュートリアルの目的で、クリア、**ガイド付きセットアップ**チェック ボックスをオンします。

* タップ**作成**を続行する、**登録**ページ。 提供、**名前**の値を確認し、**アプリケーション Id**、として使用する`ClientId`チュートリアルの後半で。

![登録ページ](index/_static/MicrosoftDevAppReg.png)

* タップ**プラットフォームの追加**で、**プラットフォーム**セクションし、選択、 **Web**プラットフォーム。

![追加のプラットフォーム ダイアログ ボックス](index/_static/MicrosoftDevAppPlatform.png)

* 新しい**Web**プラットフォーム セクションで、使用、開発の URL を入力`/signin-microsoft`に追加されます、**リダイレクト Url**フィールド (例: `https://localhost:44320/signin-microsoft`)。 このチュートリアルの後半で構成されている Microsoft の認証構成はで、要求を自動的に処理`/signin-microsoft`OAuth フローを実装するためにルート。

![Web プラットフォーム セクションの「](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> URI セグメント`/signin-microsoft`Microsoft 認証プロバイダーの既定のコールバックとして設定されます。 既定のコールバック URI を変更するには、継承を使用して、Microsoft の認証ミドルウェアを構成するときに[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)のプロパティ、 [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions)クラス。

* タップ**URL の追加**URL が追加されたことを確認します。

* 必要に応じて、その他のアプリケーション設定を記入し、タップ**保存**アプリの構成に変更を保存するページの下部にあります。

* サイトをデプロイするときに、再アクセスする必要があります、**登録**ページし、新しいパブリック URL を設定します。

## <a name="store-microsoft-application-id-and-password"></a>Microsoft のアプリケーション Id とパスワードを保存します。

* 注、`Application Id`に表示される、**登録**ページ。

* タップ**新しいパスワードを生成**で、**アプリケーション シークレット**セクション。 これには、アプリケーション パスワードをコピーするボックスが表示されます。

![新しいパスワードの生成 ダイアログ ボックス](index/_static/MicrosoftDevPassword.png)

Microsoft のような機密性の高い設定リンク`Application ID`と`Password`アプリケーションの構成を使用して、 [Secret Manager](xref:security/app-secrets)します。 このチュートリアルの目的で、名前トークン`Authentication:Microsoft:ApplicationId`と`Authentication:Microsoft:Password`します。

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Microsoft アカウント認証を構成します。

このチュートリアルで使用するプロジェクト テンプレートにより[Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount)パッケージが既にインストールされています。

* Visual Studio 2017 では、このパッケージをインストールするには、クリックし、プロジェクトを右クリックし**NuGet パッケージの管理**します。
* で .NET Core CLI をインストールするには、プロジェクト ディレクトリで、次を実行します。

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

Microsoft アカウント サービスの追加、`ConfigureServices`メソッド*Startup.cs*ファイル。

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Microsoft アカウント ミドルウェアを追加、`Configure`メソッド*Startup.cs*ファイル。

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

Microsoft 開発者ポータルで使用される用語は、これらのトークンを名前が`ApplicationId`と`Password`、として公開`ClientId`と`ClientSecret`構成 API です。

参照してください、 [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API リファレンスの詳細については、Microsoft アカウント認証でサポートされる構成オプション。 これは、ユーザーに関するさまざまな情報を要求を使用できます。

## <a name="sign-in-with-microsoft-account"></a>Microsoft アカウントでサインイン

アプリケーションを実行し、をクリックして**ログイン**します。 Microsoft アカウントでサインインするためのオプションが表示されます。

![Web ページでは、アプリケーション ログ。ユーザーが認証されていません。](index/_static/DoneMicrosoft.png)

Microsoft をクリックすると、認証のように、Microsoft にリダイレクトされます。 (サインインされていない) 場合、Microsoft アカウントでサインインしたら、アプリの情報にアクセスできるようにするよう求められます。

![Microsoft 認証ダイアログ ボックス](index/_static/MicrosoftLogin.png)

タップ**はい**とメールを設定する web サイトにリダイレクトされます。

Microsoft の資格情報を使用してログインしました。

![Web アプリケーション:認証されたユーザー](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>トラブルシューティング

* Microsoft アカウント プロバイダーでは、サインイン エラー ページにリダイレクトするの場合、エラーのタイトルと説明クエリ文字列パラメーターの直後に注意してください、 `#` (ハッシュタグ) uri。

  エラー メッセージを Microsoft 認証に問題を示すために見えますが、最も一般的な原因は、アプリケーション Uri と一致しない、**リダイレクト Uri**の指定、 **Web**プラットフォーム.
* **ASP.NET Core 2.x のみ。** ユーザーが呼び出すことによって構成されていない場合`services.AddIdentity`で`ConfigureServices`、認証を試みるが*ArgumentException:'SignInScheme' オプションを指定する必要があります*します。 このチュートリアルで使用するプロジェクト テンプレートによりこれが行われるようになります。
* 最初の移行を適用することで、サイト データベースが作成されていない場合になります*要求の処理中にデータベース操作が失敗しました*エラー。 タップ**適用移行**データベースを作成し、エラーを引き続き更新します。

## <a name="next-steps"></a>次の手順

* この記事では、Microsoft を認証する方法を示しました。 記載されているその他のプロバイダーで認証する同様のアプローチを利用できる、[前のページ](xref:security/authentication/social/index)します。

* 新規に作成する必要があります、web サイトを Azure web アプリを発行すると`Password`Microsoft 開発者ポータルでします。

* 設定、`Authentication:Microsoft:ApplicationId`と`Authentication:Microsoft:Password`として、Azure portal でアプリケーションの設定。 構成システムは、環境変数からキーの読み取りを設定します。
