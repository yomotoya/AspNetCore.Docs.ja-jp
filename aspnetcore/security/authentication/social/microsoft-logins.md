---
title: "外部ログインの Microsoft アカウントの設定"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/24/2017
ms.topic: article
ms.assetid: 66DB4B94-C78C-4005-BA03-3D982B87C268
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 4e1512aa151a87f677b400f16dbf53ca22b61e9e
ms.sourcegitcommit: 93785b6b29a4996066fba05149348f1bdf881263
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/12/2017
---
# <a name="configuring-microsoft-account-authentication"></a>Microsoft アカウントの認証を構成します。

作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルで作成されたサンプルの ASP.NET Core 2.0 プロジェクトを使用して、Microsoft アカウントでサインインするユーザーを有効にする方法を示します、[前のページ](index.md)です。

## <a name="create-the-app-in-microsoft-developer-portal"></a>Microsoft 開発者ポータルでのアプリを作成します。

* 移動[https://apps.dev.microsoft.com](https://apps.dev.microsoft.com)して作成するか、Microsoft アカウントにサインインします。

![ダイアログをサインインします。](index/_static/MicrosoftDevLogin.png)

既に Microsoft アカウントを持っていない場合は、タップ**[を作成します。](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** サインインした後にリダイレクトされます**マイ アプリケーション**ページ。

![Microsoft Developer Portal の Microsoft Edge で開く](index/_static/MicrosoftDev.png)

* タップ**アプリの追加**で右上コーナーを入力し、**アプリケーション名**と**Contact Email**:

![新しいアプリケーションの登録 ダイアログ ボックス](index/_static/MicrosoftDevAppCreate.png)

* このチュートリアルの目的で、消去、**セットアップのガイド付き**チェック ボックスをオンします。

* タップ**を作成する**を続行するのには、**登録**ページです。 提供、**名**の値をメモし、**アプリケーション Id**、として使用する`ClientId`のチュートリアルの後。

![[登録] ページ](index/_static/MicrosoftDevAppReg.png)

* タップ**プラットフォームを追加**で、**プラットフォーム**セクションし、選択、 **Web**プラットフォーム。

![追加プラットフォーム ダイアログ ボックス](index/_static/MicrosoftDevAppPlatform.png)

* 新しい**Web**プラットフォーム セクションで、使用、開発の URL を入力*/signin-microsoft*に追加された、**リダイレクト Url**フィールド (例: `https://localhost:44320/signin-microsoft`)。 このチュートリアルで後で構成されている Microsoft の認証スキームはで、要求を自動的に処理*/signin-microsoft* OAuth フローを実装するルート。

![Web プラットフォーム セクションの「](index/_static/MicrosoftRedirectUri.png)

* タップ**URL の追加**URL が追加されたことを確認します。

* 必要に応じてその他のアプリケーション設定を入力し、タップ**保存**アプリの構成に変更を保存するページの下部にあります。

* サイトを展開するときにを再表示する必要があります、**登録**ページおよび新しいパブリック URL を設定します。

## <a name="store-microsoft-application-id-and-password"></a>Microsoft のアプリケーション Id とパスワードを保存します。

* 注、`Application Id`に表示される、**登録**ページ。

* タップ**新しいパスワードの生成**で、**アプリケーション シークレット**セクションです。 これには、アプリケーション パスワードをコピーするボックスが表示されます。

![新しいパスワードの生成 ダイアログ ボックス](index/_static/MicrosoftDevPassword.png)

Microsoft のような機密設定をリンク`Application ID`と`Password`、アプリケーションを使用して構成する、[シークレット Manager](../../app-secrets.md)です。 このチュートリアルの目的で、名前トークン`Authentication:Microsoft:ApplicationId`と`Authentication:Microsoft:Password`です。

## <a name="configure-microsoft-account-authentication"></a>Microsoft アカウントの認証を構成します。

このチュートリアルで使用されるプロジェクト テンプレートにより[Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount)パッケージが既にインストールされています。

* Visual Studio 2017 でこのパッケージをインストールするには、クリックし、プロジェクトを右クリックし**NuGet パッケージの管理**です。
* .NET Core cli をインストールするには、プロジェクト ディレクトリに、次を実行します。

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Microsoft アカウントのサービスを追加、`ConfigureServices`メソッド*Startup.cs*ファイル。

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Microsoft アカウント ミドルウェアを追加、`Configure`メソッド*Startup.cs*ファイル。

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

Microsoft 開発者ポータルで使用される用語は、これらのトークンを名前が`ApplicationId`と`Password`、として公開される`ClientId`と`ClientSecret`API の構成にします。

参照してください、 [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Microsoft アカウントの認証でサポートされる構成オプションの詳細についての API リファレンスです。 ユーザーに関するさまざまな情報を要求するために使用できます。

## <a name="sign-in-with-microsoft-account"></a>Microsoft アカウントでサインイン

アプリケーションを実行し、をクリックして**ログイン**です。 Microsoft でサインインするオプションが表示されます。

![Web ページで、アプリケーション ログ: ユーザーが認証されません](index/_static/DoneMicrosoft.png)

Microsoft をクリックすると、認証のため Microsoft にリダイレクトされます。 (署名されていない) 場合は、Microsoft アカウントによるサインインの後に、アプリがあなたの情報にアクセスするよう求められます。

![Microsoft 認証ダイアログ ボックス](index/_static/MicrosoftLogin.png)

タップ**はい**は、電子メールを設定する web サイトにリダイレクトするとします。

今すぐ Microsoft 資格情報を使用してをログインしています。

![Web アプリケーション: 認証されたユーザー](index/_static/Done.png)

## <a name="troubleshooting"></a>トラブルシューティング

* 場合は、Microsoft アカウント プロバイダーにリダイレクトするサインインのエラー ページ、エラー タイトルと説明クエリ文字列パラメーターすぐ後ろに注意してください、 `#` (ハッシュタグ) Uri にします。

  エラー メッセージでは、マイクロソフトの認証に問題がある、最も一般的な原因は、アプリケーション Uri と一致しない、**のリダイレクト Uri**向けに指定された、 **Web**プラットフォーム.
* **ASP.NET Core 2.x のみ:**場合の Id が呼び出すことによって構成されていない`services.AddIdentity`で`ConfigureServices`、認証を試みるが*ArgumentException: 'SignInScheme' オプションを指定する必要があります*です。 このチュートリアルで使用されるプロジェクト テンプレートは、これが行われるようにします。
* 最初の移行を適用することで、サイト データベースが作成されていない場合、表示される*要求の処理中にデータベース操作が失敗しました*エラーです。 タップ**適用移行**データベースを作成し、エラーを越えて続行を更新します。

## <a name="next-steps"></a>次のステップ

* この記事では、Microsoft との認証方法を示しました。 記載されているその他のプロバイダーでの認証に同様のアプローチを行うことができる、[前のページ](index.md)です。

* 新規に作成する必要があります、web サイトを Azure web アプリに発行した後`Password`Microsoft 開発者ポータルにします。

* 設定、`Authentication:Microsoft:ApplicationId`と`Authentication:Microsoft:Password`として Azure ポータルでのアプリケーション設定。 構成システムは、環境変数からキーを読み取れませんを設定します。
